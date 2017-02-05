---
title: "Konfigurieren von Content Protection-Richtlinien über das Azure-Portal | Microsoft Docs"
description: "In diesem Artikel erfahren Sie, wie Sie mithilfe des Azure-Portals Content Protection-Richtlinien konfigurieren. Außerdem erfahren Sie, wie Sie die dynamische Verschlüsselung für Ihre Assets aktivieren."
services: media-services
documentationcenter: 
author: Juliako
manager: erikre
editor: 
ms.assetid: 270b3272-7411-40a9-ad42-5acdbba31154
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: juliako
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 0f9b0cf4e88a1ab27546063ae89cde8de7761609


---
# <a name="configuring-content-protection-policies-using-the-azure-portal"></a>Konfigurieren von Content Protection-Richtlinien über das Azure-Portal
> [!NOTE]
> Sie benötigen ein Azure-Konto, um dieses Lernprogramm auszuführen. Ausführliche Informationen finden Sie unter [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).
> 
> 

## <a name="overview"></a>Übersicht
Microsoft Azure Media Services (AMS) ermöglicht es Ihnen, Ihre Medien zu schützen, sobald sie Ihren Computer verlassen – sei es durch Speicherung, Verarbeitung oder Übermittlung. Mit Media Services können Sie zu übermittelnde Inhalte dynamisch mit AES (Advanced Encryption Standard) (mit 128-Bit-Verschlüsselungsschlüsseln), CENC (Common Encryption) mit PlayReady- und/oder Widevine-DRM sowie mit Apple FairPlay verschlüsseln. 

AMS bietet einen Dienst zum Übermitteln von DRM-Lizenzen und unverschlüsselten AES-Schlüsseln an autorisierte Clients. Über das Azure-Portal können Sie eine einzelne **Schlüssel-/Lizenzautorisierungsrichtlinie** für alle Arten von Verschlüsselungen erstellen.

In diesem Artikel erfahren Sie, wie Sie Content Protection-Richtlinien über das Azure-Portal konfigurieren. Außerdem erfahren Sie, wie Sie die dynamische Verschlüsselung auf Ihre Assets anwenden.

> [!NOTE]
> Wenn Sie Schutzrichtlinien über das klassische Azure-Portal erstellt haben, werden diese unter Umständen nicht im [Azure-Portal](https://portal.azure.com/) angezeigt. Die alten Richtlinien sind jedoch weiterhin vorhanden. Sie können mithilfe des Azure Media Services .NET SDKs oder mithilfe des Tools [Azure Media Services Explorer](https://github.com/Azure/Azure-Media-Services-Explorer/releases) untersucht werden (klicken Sie zum Anzeigen der Richtlinien mit der rechten Maustaste auf das Medienobjekt, wählen Sie „Informationen anzeigen [F4]“ aus, klicken Sie auf die Registerkarte „Inhaltsschlüssel“, und klicken Sie anschließend auf den Schlüssel). 
> 
> Wenn Sie Ihr Asset unter Verwendung neuer Richtlinien verschlüsseln möchten, konfigurieren Sie sie über das Azure-Portal, klicken Sie auf „Speichern“, und wenden Sie die dynamische Verschlüsselung erneut an. 
> 
> 

## <a name="start-configuring-content-protection"></a>Erste Schritte beim Konfigurieren von Content Protection
Gehen Sie wie folgt vor, um Content Protection über das Portal global für Ihr AMS-Konto zu konfigurieren:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) Ihr Azure Media Services-Konto aus.
2. Wählen Sie **Einstellungen** > **Inhaltsschutz** aus.

![Inhalt schützen](./media/media-services-portal-content-protection/media-services-content-protection001.png)

## <a name="keylicense-authorization-policy"></a>Schlüssel-/Lizenzautorisierungsrichtlinie
AMS unterstützt mehrere Methoden zur Authentifizierung von Benutzern, die Schlüssel oder Lizenzen anfordern. Die Inhaltsschlüssel-Authentifizierungsrichtlinie muss von Ihnen konfiguriert und von Ihrem Client erfüllt werden, damit der Schlüssel/die Lizenz an den Client übermittelt wird. Die Autorisierungsrichtlinie für Inhaltsschlüssel kann eine oder mehrere Autorisierungseinschränkungen aufweisen: **offen** oder **Token**.

Über das Azure-Portal können Sie eine einzelne **Schlüssel-/Lizenzautorisierungsrichtlinie** für alle Arten von Verschlüsselungen erstellen.

### <a name="open"></a>Offen
Bei Verwendung einer offenen Einschränkung übermittelt das System den Schlüssel an jeden Benutzer, der einen Schlüssel anfordert. Diese Einschränkung kann beim Testen hilfreich sein. 

### <a name="token"></a>Tokeneinschränkung
Die durch Token eingeschränkte Richtlinie gilt nur zusammen mit einem Token, das von einem Secure Token Service (STS) ausgestellt wurde. Media Services unterstützt Token im Simple Web Tokens (SWT)-Format und JSON Web Token (JWT)-Format. Secure Token Services werden von Media Services nicht bereitgestellt. Sie können einen benutzerdefinierten STS erstellen oder Microsoft Azure ACS zum Ausstellen von Token nutzen. Der STS muss für die Erstellung eines mit dem angegebenen Schlüssel signierten Tokens und die Ausstellungsansprüche konfiguriert sein, die Sie in der Konfiguration der Tokeneinschränkung angegeben haben. Der Schlüsselübermittlungsdienst von Media Services gibt den angeforderten Schlüssel (oder die Lizenz) an den Client zurück, wenn das Token gültig ist und die Ansprüche im Token mit den für den Schlüssel (oder die Lizenz) konfigurierten Ansprüchen übereinstimmen.

Beim Konfigurieren der Richtlinie mit Tokeneinschränkung müssen der primäre Verifizierungsschlüssel, der Aussteller und die Zielgruppe angegeben werden. PrimaryVerificationKey enthält den Schlüssel, mit dem das Token signiert wurde, und Issuer ist der STS (Secure Token Service), von dem das Token ausgestellt wurde. Audience (manchmal auch Scope) beschreibt den Verwendungszweck des Tokens oder die Ressource, auf die durch das Token Zugriff gewährt wird. Der Schlüsselübermittlungsdienst von Media Services überprüft, ob die Werte im Token mit den Werten in der Vorlage übereinstimmen.

![Inhalt schützen](./media/media-services-portal-content-protection/media-services-content-protection002.png)

## <a name="playready-rights-template"></a>PlayReady-Rechtevorlage
Ausführliche Informationen zur PlayReady-Rechtevorlage finden Sie unter [Media Services PlayReady-Lizenzvorlage – Übersicht](media-services-playready-license-template-overview.md).

### <a name="non-persistent"></a>Nicht beständig
Wenn Sie die Lizenz als nicht beständig konfigurieren, bleibt sie nur für die Dauer der Verwendung durch den Player im Arbeitsspeicher.  

![Inhalt schützen](./media/media-services-portal-content-protection/media-services-content-protection003.png)

### <a name="persistent"></a>Beständig
Wenn Sie die Lizenz als beständig konfigurieren, wird sie auf dem Client im beständigen Speicher gespeichert.

![Inhalt schützen](./media/media-services-portal-content-protection/media-services-content-protection004.png)

## <a name="widevine-rights-template"></a>Widevine-Rechtevorlage
Ausführliche Informationen zur Widevine-Rechtevorlage finden Sie in der [Übersicht über die Widevine-Lizenzvorlage](media-services-widevine-license-template-overview.md).

### <a name="basic"></a>Basic
Bei Verwendung der Option **Basic**wird die Vorlage mit allen Standardwerten erstellt.

### <a name="advanced"></a>Erweitert
Eine ausführliche Erläuterung der erweiterten Option für Widevine-Konfigurationen finden Sie [hier](media-services-widevine-license-template-overview.md) .

![Inhalt schützen](./media/media-services-portal-content-protection/media-services-content-protection005.png)

## <a name="fairplay-configuration"></a>FairPlay-Konfiguration
Um die FairPlay-Verschlüsselung zu aktivieren, müssen Sie das App-Zertifikat und den geheimen Anwendungsschlüssel (Application Secret Key, ASK) über die FairPlay-Konfigurationsoption angeben. Ausführliche Informationen zur FairPlay-Konfiguration und zu den Voraussetzungen finden Sie in [diesem Artikel](media-services-protect-hls-with-fairplay.md) .

![Inhalt schützen](./media/media-services-portal-content-protection/media-services-content-protection006.png)

## <a name="apply-dynamic-encryption-to-your-asset"></a>Anwenden der dynamischen Verschlüsselung auf Ihr Asset
Führen Sie die folgenden Schritte aus, um die dynamische Verschlüsselung zu nutzen:

* Codieren Sie Ihre Quelldatei als Gruppe von MP4-Dateien mit adaptiver Bitrate.
* Rufen Sie mindestens eine On-Demand-Streamingeinheit für den Streamingendpunkt ab, von dem aus Sie die Übermittlung Ihrer Inhalte planen. Weitere Informationen finden Sie unter [Skalieren von reservierten Einheiten für bedarfsgesteuertes Streaming](media-services-portal-manage-streaming-endpoints.md).

### <a name="select-an-asset-that-you-want-to-encrypt"></a>Auswählen eines zu verschlüsselnden Assets
Wählen Sie **Einstellungen** > **Medienobjekte** aus, um alle Ihre Medienobjekte anzuzeigen.

![Inhalt schützen](./media/media-services-portal-content-protection/media-services-content-protection007.png)

### <a name="encrypt-with-aes-or-drm"></a>Verschlüsseln mit AES oder DRM
Wenn Sie für ein Medienobjekt auf **Verschlüsseln** klicken, haben Sie die Wahl zwischen zwei Optionen: **AES** oder **DRM**. 

#### <a name="aes"></a>AES
Die AES-Verschlüsselung mit unverschlüsseltem Schlüssel wird für alle Streamingprotokolle aktiviert: Smooth Streaming, HLS und MPEG-DASH.

![Inhalt schützen](./media/media-services-portal-content-protection/media-services-content-protection008.png)

#### <a name="drm"></a>DRM
Auf der DRM-Registerkarte stehen verschiedene Optionen für Content Protection-Richtlinien (die Sie inzwischen konfiguriert haben müssen) sowie eine Reihe von Streamingprotokollen zur Verfügung.

* **PlayReady and Widevine with MPEG-DASH** (PlayReady und Widevine mit MPEG-DASH): Verschlüsselt Ihren MPEG-DASH-Datenstrom dynamisch mit PlayReady- und Widevine-DRMs.
* **PlayReady and Widevine with MPEG-DASH + FairPlay with HLS** (PlayReady und Widevine mit MPEG-DASH plus FairPlay mit HLS): Verschlüsselt Ihren MPEG-DASH-Datenstrom dynamisch mit PlayReady- und Widevine-DRMs. HLS-Datenströme werden mit FairPlay verschlüsselt.
* **PlayReady only with Smooth Streaming, HLS and MPEG-DASH** (Nur PlayReady mit Smooth Streaming, HLS und MPEG-DASH): Verschlüsselt Smooth Streaming-, HLS- und MPEG-DASH-Datenströme dynamisch mit PlayReady-DRM.
* **Widevine only with MPEG-DASH** (Nur Widevine mit MPEG-DASH): Verschlüsselt Ihren MPEG-DASH-Datenstrom dynamisch mit Widevine-DRM.
* **FairPlay only with HLS** (Nur FairPlay mit HLS): Verschlüsselt Ihren HLS-Datenstrom dynamisch mit FairPlay.

Um die FairPlay-Verschlüsselung zu aktivieren, müssen Sie das App-Zertifikat und den geheimen Anwendungsschlüssel (Application Secret Key, ASK) über die FairPlay-Konfigurationsoption auf dem Blatt mit den Content Protection-Einstellungen angeben.

![Inhalt schützen](./media/media-services-portal-content-protection/media-services-content-protection009.png)

Wählen Sie eine Verschlüsselung aus, und klicken Sie anschließend auf **Übernehmen**.

## <a name="next-steps"></a>Nächste Schritte
Überprüfen Sie die Media Services-Lernpfade.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geben
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]




<!--HONumber=Nov16_HO3-->


