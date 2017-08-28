---
title: "Schützen Ihrer Inhalte mit Azure Media Services | Microsoft-Dokumentation"
description: "In diesem Artikel finden Sie eine Übersicht über die Content Protection mit Media Services."
services: media-services
documentationcenter: 
author: Juliako
manager: SyntaxC4
editor: 
ms.assetid: 81bc00e1-dcda-4d69-b9ab-8768b793422b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: juliako
ms.translationtype: Human Translation
ms.sourcegitcommit: 555e0b6340d09517bfd87efe209f0304f3266788
ms.openlocfilehash: bf2bd9bca8817f64790ac62d2981a51aa36566a3
ms.contentlocale: de-de
ms.lasthandoff: 01/27/2017

---
# <a name="protecting-content-overview"></a>Schützen von Inhalten – Übersicht
Microsoft Azure Media Services ermöglicht die Sicherung Ihrer Medien ab dem Zeitpunkt, an dem sie Ihren Computer durch Speicherung, Verarbeitung und Übermittlung verlassen. Mit Media Services können Sie zu übermittelnde Live- und On-Demand-Inhalte dynamisch mit AES (Advanced Encryption Standard) (mit 128-Bit-Verschlüsselungsschlüsseln) oder unter Verwendung einer anderen gängigen DRM-Lösung (Microsoft PlayReady, Google Widevine und Apple FairPlay) verschlüsseln. Media Services bietet auch einen Dienst für die Übermittlung von AES-Schlüsseln und DRM-Lizenzen (PlayReady, Widevine und FairPlay) an autorisierte Clients. 

Die folgende Abbildung veranschaulicht die von AMS unterstützten Content Protection-Workflows: 

![Schützen mit PlayReady](./media/media-services-content-protection-overview/media-services-content-protection-with-multi-drm.png)

>[!NOTE]
>Beim Erstellen Ihres AMS-Kontos wird dem Konto ein **Standard**-Streamingendpunkt mit dem Status **Beendet** hinzugefügt. Um mit dem Streamen der Inhalte zu beginnen und die dynamische Paketerstellung und dynamische Verschlüsselung zu nutzen, muss der Streamingendpunkt, von dem Sie Inhalte streamen möchten, den Status **Wird ausgeführt** aufweisen. 

In diesem Thema werden relevante [Konzepte und Begriffe](media-services-content-protection-overview.md) für Content Protection mit AMS behandelt. Darüber hinaus enthält das Thema [Links](media-services-content-protection-overview.md#common-scenarios) zu Themen mit Informationen zu Content Protection-Aufgaben. 

## <a name="dynamic-encryption"></a>Dynamische Verschlüsselung
Mit Microsoft Azure Media Services können Sie zu übermittelnde Inhalte dynamisch mit unverschlüsseltem AES-Schlüssel oder mit DRM-Verschlüsselung (Microsoft PlayReady, Google Widevine und Apple FairPlay) verschlüsseln.

Zur Zeit können Sie die folgenden Streamingformate verschlüsseln: HLS, MPEG DASH und Smooth Streaming. Progressive Downloads können nicht verschlüsselt werden.

Wenn ein Medienobjekt durch Media Services verschlüsselt werden soll, müssen Sie dem Medienobjekt einen Verschlüsselungsschlüssel (CommonEncryption oder EnvelopeEncryption) zuordnen und zusätzlich Autorisierungsrichtlinien für den Schlüssel konfigurieren.

Außerdem müssen Sie die Übermittlungsrichtlinie des Medienobjekts konfigurieren. Wenn Sie ein speicherverschlüsseltes Medienobjekt streamen möchten, geben Sie durch Konfigurieren der Übermittlungsrichtlinie an, wie die Übermittlung erfolgen soll.

Wenn ein Player einen Datenstrom anfordert, verwendet Media Services den angegebenen Schlüssel, um den Inhalt dynamisch mit einem unverschlüsselten AES-Schlüssel oder mit DRM-Verschlüsselung zu verschlüsseln. Um den Stream zu entschlüsseln, fordert der Player den Schlüssel vom Schlüsselübermittlungsdienst an. Um zu entscheiden, ob der Benutzer berechtigt ist, den Schlüssel zu erhalten, wertet der Dienst die Autorisierungsrichtlinien aus, die Sie für den Schlüssel angegeben haben.


## <a name="storage-encryption"></a>Speicherverschlüsselung
Verwenden Sie die Speicherverschlüsselung, um Ihre unverschlüsselten Inhalte lokal mithilfe der AES-256-Bit-Verschlüsselung zu verschlüsseln und sie dann in Azure Storage hochzuladen, wo sie im Ruhezustand verschlüsselt und gespeichert werden. Medienobjekte, die durch Speicherverschlüsselung geschützt sind, werden automatisch entschlüsselt, vor der Codierung in einem verschlüsselten Dateisystem platziert und optional vor dem Hochladen als neues Ausgabemedienobjekt erneut verschlüsselt. Der primäre Anwendungsfall für die Speicherverschlüsselung ist, wenn Sie Ihre qualitativ hochwertigen Eingabemediendateien mit starker Verschlüsselung beim Speichern im Ruhezustand auf dem Datenträger sichern möchten.

Um ein speicherverschlüsseltes Medienobjekt zu übermitteln, müssen Sie die Übermittlungsrichtlinie des Medienobjekts konfigurieren und Media Services so mitteilen, wie die Inhalte bereitgestellt werden sollen. Bevor das Medienobjekt gestreamt werden kann, wird die Speicherverschlüsselung vom Streamingserver entfernt und der Inhalt mithilfe der angegebenen Übermittlungsrichtlinie (AES, Common Encryption oder unverschlüsselt) gestreamt.

## <a name="common-encryption-cenc"></a>Allgemeine Verschlüsselung (Common Encryption, CENC)
Die allgemeine Verschlüsselung wird verwendet, wenn Sie Ihre Inhalte mit PlayReady und/oder mit Widevine verschlüsseln.

## <a name="using-cbcs-aapl-encryption"></a>Verwenden der cbcs-aapl-Verschlüsselung
Cbcs-aapl wird verwendet, wenn Sie Ihre Inhalte mit FairPlay verschlüsseln.

## <a name="envelope-encryption"></a>Umschlagverschlüsselung
Verwenden Sie diese Option, wenn Sie Ihre Inhalte mit einem unverschlüsselten AES-128-Schlüssel schützen möchten. Eine höhere Sicherheit erreichen Sie allerdings mit einer der in diesem Thema aufgeführten DRM-Lösungen. 

## <a name="licenses-and-keys-delivery-service"></a>Dienst für die Übermittlung von Lizenzen und Schlüsseln
Media Services bietet einen Dienst zum Übermitteln von DRM-Lizenzen (PlayReady, Widevine und FairPlay) und unverschlüsselten AES-Schlüsseln an autorisierte Clients. Zum Konfigurieren von Autorisierungs- und Authentifizierungsrichtlinien für Ihre Lizenzen und Schlüssel können Sie das [Azure-Portal](media-services-portal-protect-content.md), die REST-API oder das Media Services SDK für .NET verwenden.

## <a name="token-restriction"></a>Token-Einschränkung
Die Autorisierungsrichtlinie für Inhaltsschlüssel kann eine oder mehrere Autorisierungseinschränkungen aufweisen: offen oder Tokeneinschränkung. Die durch Token eingeschränkte Richtlinie gilt nur zusammen mit einem Token, das von einem Secure Token Service (STS) ausgestellt wurde. Media Services unterstützt Token im Simple Web Tokens (SWT)-Format und JSON Web Token (JWT)-Format. Secure Token Services werden von Media Services nicht bereitgestellt. Sie können einen benutzerdefinierten STS erstellen oder Microsoft Azure ACS zum Ausstellen von Token nutzen. Der STS muss für die Erstellung eines mit dem angegebenen Schlüssel signierten Tokens und die Ausstellungsansprüche konfiguriert sein, die Sie in der Konfiguration der Tokeneinschränkung angegeben haben. Der Schlüsselübermittlungsdienst von Media Services gibt den angeforderten Schlüssel (oder die Lizenz) an den Client zurück, wenn das Token gültig ist und die Ansprüche im Token mit den für den Schlüssel (oder die Lizenz) konfigurierten Ansprüchen übereinstimmen.

Bei der Konfiguration der Richtlinie mit Token-Einschränkung müssen die Parameter PrimaryVerificationKey, Issuer und Audience angegeben werden. PrimaryVerificationKey enthält den Schlüssel, mit dem das Token signiert wurde, und Issuer ist der STS (Secure Token Service), von dem das Token ausgestellt wurde. Audience (manchmal auch Scope) beschreibt den Verwendungszweck des Tokens oder die Ressource, auf die durch das Token Zugriff gewährt wird. Der Schlüsselübermittlungsdienst von Media Services überprüft, ob die Werte im Token mit den Werten in der Vorlage übereinstimmen.

## <a name="streaming-urls"></a>Streaming-URLs
Falls Ihr Asset mit mehreren DRM-Lösungen verschlüsselt wurde, empfiehlt es sich, in der Streaming-URL ein Verschlüsselungstag zu verwenden: (format='m3u8-aapl', encryption='xxx').

Es gelten die folgenden Bedingungen:

* Sie können nur einen einzelnen (oder keinen) Verschlüsselungstyp angegeben.
* Der Verschlüsselungstyp muss nicht in der URL angegeben werden, wenn auf das Asset nur eine einzelne Verschlüsselung angewendet wurde.
* Beim Verschlüsselungstyp wird die Groß-/Kleinschreibung nicht beachtet.
* Folgende Verschlüsselungstypen können angegeben werden:  
  * **cenc**: Allgemeine Verschlüsselung (PlayReady oder Widevine)
  * **cbcs-aapl**: Fairplay
  * **cbc**: AES-Umschlagverschlüsselung

## <a name="common-scenarios"></a>Gängige Szenarien
Die folgenden Themen enthalten Informationen zum Schützen von Inhalten im Speicher, zum Übermitteln dynamisch verschlüsselter Streamingmedien sowie zum Verwenden des AMS-Schlüssel-/Lizenzübermittlungsdiensts:

* [Verwenden der dynamischen AES-128-Verschlüsselung und des Schlüsselübermittlungsdiensts](media-services-protect-with-aes128.md) 
* [Verwenden von dynamischer allgemeiner Verschlüsselung mit PlayReady und/oder Widevine ](media-services-protect-with-drm.md)
* [Schützen von HLS-Inhalten mit Apple FairPlay und/oder Microsoft PlayReady](media-services-protect-hls-with-fairplay.md)

### <a name="additional-scenarios"></a>Zusätzliche Szenarien
* [How to integrate Azure PlayReady License service with your own encryptor/streaming server](http://mingfeiy.com/integrate-azure-playready-license-service-encryptorstreaming-server)(Integrieren des Azure PlayReady-Lizenzierungsdiensts in einen eigenen Verschlüsselungs-/Streamingserver)
* [Übermitteln von DRM-Lizenzen an Azure Media Services mithilfe von castLabs](media-services-castlabs-integration.md)

>[!NOTE]
>Ein Szenario, in dem Sie einen externen DRM-Server (bzw. eine DRM-Technologie) und einen Datenstrom von AMS verwenden, wird derzeit nicht unterstützt.


## <a name="media-services-learning-paths"></a>Media Services-Lernpfade
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geben
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>Verwandte Links
[Announcing PlayReady as a service and AES dynamic encryption with Azure Media Services ("Ankündigen von PlayReady als Dienst und dynamische AES-Verschlüsselung mit Azure Media Services", in englischer Sprache)](http://mingfeiy.com/playready)

[Azure Media Services PlayReady license delivery pricing explained ("Erläuterung der Preisgestaltung bei der Azure Media Services PlayReady-Lizenzübermittlung", in englischer Sprache)](http://mingfeiy.com/playready-pricing-explained-in-azure-media-services)

[How to debug for AES encrypted stream in Azure Media Services ("Debuggen AES-verschlüsselter Streams in Azure Media Services", in englischer Sprache)](http://mingfeiy.com/debug-aes-encrypted-stream-azure-media-services)

[JWT-Tokenauthentifizierung](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)

[Integrieren einer Azure Media Services-OWIN MVC-basierten App in Azure Active Directory und Einschränken der Übermittlung von Inhaltsschlüsseln auf Grundlage von JWT-Ansprüchen](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/)

[Ausstellen von Token mithilfe von Azure ACS](http://mingfeiy.com/acs-with-key-services)

[content-protection]: ./media/media-services-content-protection-overview/media-services-content-protection.png

