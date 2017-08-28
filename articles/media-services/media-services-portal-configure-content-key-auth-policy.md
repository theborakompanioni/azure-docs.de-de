---
title: "Konfigurieren einer Autorisierungsrichtlinie für Inhaltsschlüssel im Azure-Portal | Microsoft Docs"
description: "Erfahren Sie, wie Sie eine Autorisierungsrichtlinie für einen Inhaltsschlüssel konfigurieren."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: ee82a3fa-c34b-48f2-a108-8ba321f1691e
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: juliako
ms.translationtype: HT
ms.sourcegitcommit: 760543dc3880cb0dbe14070055b528b94cffd36b
ms.openlocfilehash: 2ad1199ac4cbf40ea9eb9c55046094fd988c7325
ms.contentlocale: de-de
ms.lasthandoff: 08/10/2017

---
# <a name="configure-content-key-authorization-policy"></a>Konfigurieren einer Autorisierungsrichtlinie für Inhaltsschlüssel
[!INCLUDE [media-services-selector-content-key-auth-policy](../../includes/media-services-selector-content-key-auth-policy.md)]

## <a name="overview"></a>Übersicht
Mit Microsoft Azure Media Services können Sie MPEG-DASH-, Smooth Streaming- und HTTP-Live-Streaming-Datenströme (HLS) übermitteln, die mit Advanced Encryption Standards (AES) (mit 128-Bit-Verschlüsselungschlüsseln) oder [Microsoft PlayReady DRM](https://www.microsoft.com/playready/overview/)geschützt sind. AMS ermöglicht Ihnen auch, mit Widevine DRM verschlüsselte DASH-Datenströme bereitzustellen. PlayReady und Widevine sind gemäß der Spezifikation Common Encryption (ISO/IEC 23001-7 CENC) verschlüsselt.

Media Services bietet einen **Schlüssel-/Lizenzübermittlungsdienst** , von dem die Clients AES-Schlüssel oder PlayReady/Widevine-Lizenzen zur Wiedergabe von verschlüsselten Inhalten abrufen können.

In diesem Thema wird veranschaulicht, wie Sie das Azure-Portal zur Konfiguration der Autorisierungsrichtlinie für Inhaltsschlüssel verwenden. Der Schlüssel kann später verwendet werden, um Inhalte dynamisch zu verschlüsseln. Zur Zeit können Sie die folgenden Streamingformate verschlüsseln: HLS, MPEG DASH und Smooth Streaming. Progressive Downloads können nicht verschlüsselt werden.

Wenn ein Player einen Datenstrom anfordert, der für die dynamische Verschlüsselung konfiguriert ist, verwendet Media Services den konfigurierten Schlüssel, um Inhalte dynamisch mit der AES- oder DRM-Verschlüsselung zu verschlüsseln. Um den Stream zu entschlüsseln, fordert der Player den Schlüssel vom Schlüsselübermittlungsdienst an. Um zu entscheiden, ob der Benutzer berechtigt ist, den Schlüssel zu erhalten, wertet der Dienst die Autorisierungsrichtlinien aus, die Sie für den Schlüssel angegeben haben.

Wenn Sie mehrere Inhaltsschlüssel verwenden oder eine andere **Schlüssel-/Lizenzübermittlungsdienst** -URL als die des Schlüsselübermittlungsdiensts von Media Services angeben möchten, verwenden Sie das Media Services .NET-SDK oder REST-APIs.

[Konfigurieren einer Autorisierungsrichtlinie für Inhaltsschlüssel mit dem .NET-SDK](media-services-dotnet-configure-content-key-auth-policy.md)

[Konfigurieren einer Autorisierungsrichtlinie für Inhaltsschlüssel mit der REST-API](media-services-rest-configure-content-key-auth-policy.md)

### <a name="some-considerations-apply"></a>Folgende Überlegungen sollten berücksichtigt werden:
* Beim Erstellen Ihres AMS-Kontos wird dem Konto ein **Standard**-Streamingendpunkt mit dem Status **Beendet** hinzugefügt. Um mit dem Streamen der Inhalte zu beginnen und die dynamische Paketerstellung und dynamische Verschlüsselung zu nutzen, muss der Streamingendpunkt den Status **Wird ausgeführt** aufweisen. 
* Ihr Medienobjekt muss einen Satz von MP4-Dateien bzw. Smooth Streaming-Dateien mit adaptiver Bitrate enthalten. Weitere Informationen finden Sie unter [Codieren von Medienobjekten](media-services-encode-asset.md).
* ContentKeyAuthorizationPolicy und die zugehörigen Objekte (Richtlinienoptionen und Einschränkungen) werden vom Schlüsselübermittlungsdienst für 15 Minuten zwischengespeichert.  Wenn Sie ContentKeyAuthorizationPolicy erstellen und angeben, dass eine „Token“-Einschränkung verwendet werden soll, diese anschließend testen und dann die Richtlinie auf eine „Open“-Einschränkung aktualisieren, dauert es ungefähr 15 Minuten, bis die Richtlinie zur „Open“-Version der Richtlinie wechselt.

## <a name="how-to-configure-the-key-authorization-policy"></a>Vorgehensweise: Konfigurieren der Schlüsselautorisierungsrichtlinie
Um die Schlüsselautorisierungsrichtlinie zu konfigurieren, wählen Sie die Seite **INHALTSSCHUTZ** aus.

Media Services unterstützt mehrere Möglichkeiten zur Authentifizierung von Benutzern, die Schlüssel anfordern. Autorisierungsrichtlinien für Inhaltsschlüssel können die Autorisierungseinschränkung **Open**, **Token** oder **IP** aufweisen (**IP** kann mit REST oder dem .NET SDK konfiguriert werden).

### <a name="open-restriction"></a>Open-Einschränkung
Bei Verwendung einer **Open** -Einschränkung übermittelt das System den Schlüssel an jede Person, die einen Schlüssel anfordert. Diese Einschränkung kann zu Testzwecken nützlich sein.

![OpenPolicy][open_policy]

### <a name="token-restriction"></a>Token-Einschränkung
Zum Auswählen der durch "Token" eingeschränkten Richtlinie klicken Sie auf die Schaltfläche **TOKEN** .

Die durch **Token** eingeschränkte Richtlinie gilt nur zusammen mit einem Token, das von einem **Secure Token Service (STS)** ausgestellt wurde. Media Services unterstützt Token im **Simple Web Tokens**-Format ([SWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2)) und **JSON Web Token**-Format (JWT). Informationen finden Sie unter [JWT token authentication](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)("JWT-Tokenauthentifizierung", in englischer Sprache).

**Secure Token Services**werden von Media Services nicht bereitgestellt. Sie können einen benutzerdefinierten STS erstellen oder Microsoft Azure ACS zum Ausstellen von Token nutzen. Der STS muss für die Erstellung eines mit dem angegebenen Schlüssel signierten Tokens und die Ausstellungsansprüche konfiguriert sein, die Sie in der Konfiguration der Tokeneinschränkung angegeben haben. Der Schlüsselübermittlungsdienst von Media Services gibt den Verschlüsselungsschlüssel an den Client zurück, wenn das Token gültig ist und die Ansprüche im Token mit den für den Inhaltsschlüssel konfigurierten Ansprüchen übereinstimmen. Weitere Informationen finden Sie unter [Use Azure ACS to issue tokens](http://mingfeiy.com/acs-with-key-services)("Ausstellen von Token mithilfe von Azure ACS", in englischer Sprache).

Beim Konfigurieren der durch **TOKEN** eingeschränkten Richtlinie müssen Sie Werte für **verification key**, **issuer** und **audience** festlegen. PrimaryVerificationKey enthält den Schlüssel, mit dem das Token signiert wurde, und Issuer ist der STS (Secure Token Service), von dem das Token ausgestellt wurde. Audience (manchmal auch Scope) beschreibt den Verwendungszweck des Tokens oder die Ressource, auf die durch das Token Zugriff gewährt wird. Der Schlüsselübermittlungsdienst von Media Services überprüft, ob die Werte im Token mit den Werten in der Vorlage übereinstimmen.

### <a name="playready"></a>PlayReady
Wenn Sie Inhalte mit **PlayReady** schützen, müssen Sie in Ihrer Autorisierungsrichtlinie u.a. eine XML-Zeichenfolge zur Definition der PlayReady-Lizenzvorlage angeben. Standardmäßig ist die folgende Richtlinie festgelegt:

<PlayReadyLicenseResponseTemplate xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1"> <LicenseTemplates> <PlayReadyLicenseTemplate><AllowTestDevices>true</AllowTestDevices> <ContentKey i:type="ContentEncryptionKeyFromHeader" /> <LicenseType>Nicht beständig</LicenseType> <PlayRight> <AllowPassingVideoContentToUnknownOutput>Zulässig</AllowPassingVideoContentToUnknownOutput> </PlayRight> </PlayReadyLicenseTemplate> </LicenseTemplates> </PlayReadyLicenseResponseTemplate>

Klicken Sie auf die Schaltfläche **XML für Importrichtlinie** , und geben Sie anderen XML-Code an, der dem [hier](media-services-playready-license-template-overview.md)definierten XML-Schema entspricht.

## <a name="next-step"></a>Nächster Schritt
Überprüfen Sie die Media Services-Lernpfade.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geben
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

[open_policy]: ./media/media-services-portal-configure-content-key-auth-policy/media-services-protect-content-with-open-restriction.png
[token_policy]: ./media/media-services-key-authorization-policy/media-services-protect-content-with-token-restriction.png


