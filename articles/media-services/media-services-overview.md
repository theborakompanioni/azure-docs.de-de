---
title: "Azure Media Services – Übersicht und häufige Szenarios | Microsoft Docs"
description: "Dieses Thema bietet eine Übersicht über Azure Media Services."
services: media-services
documentationcenter: 
author: Juliako
manager: erikre
editor: 
ms.assetid: 7a5e9723-c379-446b-b4d6-d0e41bd7d31f
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 01/05/2017
ms.author: juliako;anilmur
ms.translationtype: Human Translation
ms.sourcegitcommit: 95b8c100246815f72570d898b4a5555e6196a1a0
ms.openlocfilehash: 0a84e69f33ae30040c2365e6a5180e17e8df7539
ms.contentlocale: de-de
ms.lasthandoff: 05/18/2017


---
# <a name="azure-media-services-overview-and-common-scenarios"></a>Azure Media Services – Übersicht und häufige Szenarios

Microsoft Azure Media Services ist eine erweiterbare, cloudbasierte Plattform, die Entwicklern das Erstellen von skalierbaren Medienverwaltungslösungen und Bereitstellungsanwendungen ermöglicht. Media Services basiert auf REST-APIs, mit denen Sie auf sichere Weise Video- oder Audioinhalte hochladen, speichern, codieren und paketieren können – sowohl für eine bedarfsgesteuerte als auch für eine Livestreaming-basierte Bereitstellung auf verschiedenen Clients (z. B. TV, PC und mobile Geräte).

Sie können mithilfe von Media Services vollständige End-to-End-Workflows erstellen. Es ist auch möglich, Drittanbieterkomponenten für einige Elemente Ihres Workflows zu nutzen. Beispielsweise können Sie die Codierung mit einem Encoder eines Drittanbieters durchführen. Anschließend sorgen Sie mit Media Services für Upload, Schutz, Paketierung und Bereitstellung.

Sie können Ihre Inhalte live streamen oder sie bei Bedarf bereitstellen. In diesem Thema werden allgemeine Szenarien für die Übermittlung von Inhalten [live](media-services-overview.md#live_scenarios) oder [bei Bedarf](media-services-overview.md#vod_scenarios) gezeigt. Das Thema enthält außerdem Links zu anderen relevanten Themen.

## <a name="sdks-and-tools"></a>SDKs und Tools

Zum Entwickeln von Media Services-Lösungen können Sie folgende Komponenten verwenden:

* [Media Services-REST-API](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference)
* Eines der verfügbaren Client-SDKs:
    * [Azure Media Services SDK für .NET](https://github.com/Azure/azure-sdk-for-media-services)
    * [Azure SDK für Java](https://github.com/Azure/azure-sdk-for-java)
    * [Azure PHP SDK](https://github.com/Azure/azure-sdk-for-php)
    * [Azure Media Services für Node.js](https://github.com/michelle-becker/node-ams-sdk/blob/master/lib/request.js) (Dies ist eine nicht von Microsoft stammende Version des Node.js SDK. Sie wird von einer Community verwaltet und bietet derzeit keine 100 %-ige Abdeckung der AMS-APIs).
* Vorhandene Tools:
    * [Azure-Portal](https://portal.azure.com/)
    * [Azure Media Services-Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (Azure Media Services Explorer [AMSE] ist eine WinForms-/C#-Anwendung für Windows)

Die folgende Abbildung zeigt einige der am häufigsten verwendeten Objekte beim Entwickeln mit dem Media Services OData-Modell.

Klicken Sie auf Bild, um es in voller Größe anzeigen.  

<a href="./media/media-services-overview/media-services-overview-object-model.png" target="_blank"><img src="./media/media-services-overview/media-services-overview-object-model-small.png"></a> 

Sie können das gesamte Modell [hier](https://media.windows.net/API/$metadata?api-version=2.15) anzeigen.  


## <a name="media-services-learning-paths"></a>Media Services-Lernpfade
Sie können sich die AMS-Lernpfade hier ansehen:

* [Media Services - Live Streaming (in englischer Sprache)](https://azure.microsoft.com/documentation/learning-paths/media-services-streaming-live/)
* [Media Services - on Demand Streaming (in englischer Sprache)](https://azure.microsoft.com/documentation/learning-paths/media-services-streaming-on-demand/)

## <a name="prerequisites"></a>Voraussetzungen

Um mit Azure Media Services loszulegen, sollten Sie Folgendes haben:

1. Ein Azure-Konto. Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Weitere Informationen finden Sie unter [Kostenloses Azure-Testkonto](https://azure.microsoft.com).
2. Ein Azure Media Services-Konto. Verwenden Sie das Azure-Portal, .NET oder die REST-API zum Erstellen eines Azure Media Services-Kontos. Weitere Informationen finden Sie unter [Erstellen von Konten](media-services-portal-create-account.md).
3. (Optional) Eine eingerichtete Entwicklungsumgebung. Wählen Sie .NET oder REST-API für Ihre Entwicklungsumgebung. Weitere Informationen finden Sie unter [Einrichten der Umgebung](media-services-dotnet-how-to-use.md).

    Informieren Sie sich auch darüber, wie Sie [programmgesteuert eine Verbindung herstellen](media-services-dotnet-connect-programmatically.md) können.
4. Einen Standard- oder Premium-Streamingendpunkt im Status „Gestartet“.  Weitere Informationen finden Sie unter [Verwalten von Streamingendpunkten](https://docs.microsoft.com/en-us/azure/media-services/media-services-portal-manage-streaming-endpoints).

## <a name="concepts-and-overview"></a>Konzepte und Übersicht
Azure Media Services-Konzepte finden Sie unter [Konzepte](media-services-concepts.md).

Eine Reihe mit Vorgehensweisen (Gewusst wie), in der die wichtigsten Komponenten von Azure Media Services beschrieben werden, finden Sie unter [Azure Media Services Step-by-Step tutorials](https://docs.com/fukushima-shigeyuki/3439/english-azure-media-services-step-by-step-series)(Schritt-für-Schritt-Tutorials zu Azure Media Services). Diese Serie bietet eine hervorragende Übersicht über die Konzepte und verwendet das AMSE-Tool, um die AMS-Aufgaben zu erläutern. Beachten Sie, dass es sich beim AMSE-Tool um ein Windows-Tool handelt. Dieses Tool unterstützt die meisten Aufgaben, die Sie programmgesteuert mit dem [AMS SDK für .NET](https://github.com/Azure/azure-sdk-for-media-services), dem [Azure SDK für Java](https://github.com/Azure/azure-sdk-for-java) oder dem [Azure PHP SDK](https://github.com/Azure/azure-sdk-for-php) durchführen können.

## <a id="vod_scenarios"></a>Bereitstellen von Medien-on-Demand mit Azure Media Services: häufige Szenarios und Aufgaben.
In diesem Abschnitt werden allgemeine Szenarien erläutert und Links zu relevanten Themen bereitgestellt. Das folgende Diagramm zeigt die Hauptbestandteile der Media Services-Plattform, die am Bereitstellen von On-Demand-Inhalten beteiligt sind.

![VoD-Workflow](./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png)

>[!NOTE]
>Beim Erstellen Ihres AMS-Kontos wird dem Konto ein **Standard**-Streamingendpunkt mit dem Status **Beendet** hinzugefügt. Um mit dem Streamen der Inhalte zu beginnen und die dynamische Paketerstellung und dynamische Verschlüsselung zu nutzen, muss der Streamingendpunkt, von dem Sie Inhalte streamen möchten, den Status **Wird ausgeführt** aufweisen.

### <a name="protect-content-in-storage-and-deliver-streaming-media-in-the-clear-non-encrypted"></a>Schützen von Inhalte im Speicher und Übermitteln von Streamingmedien ohne Verschlüsselung
1. Laden Sie eine Zwischendatei (Mezzanine File) hoher Qualität in ein Medienobjekt hoch.

    Es wird empfohlen, eine Speicherverschlüsselung auf Medienobjekte anzuwenden, um Ihre Inhalte beim Hochladen und während der Speicherung zu schützen.
2. Codieren Sie das Medienobjekt in einen Satz von MP4-Dateien mit adaptiver Bitrate.

    Es wird empfohlen, eine Speicherverschlüsselung das Ausgabemedienobjekt anzuwenden, um Ihre Inhalte während der Speicherung zu schützen.
3. Konfigurieren Sie eine Übermittlungsrichtlinie für Medienobjekte (wird zur dynamischen Paketerstellung verwendet).

    Wenn Ihr Medienobjekt speicherverschlüsselt ist, **müssen** Sie die Übermittlungsrichtlinien für Medienobjekte konfigurieren.
4. Veröffentlichen Sie das Medienobjekt durch Erstellen eines OnDemand-Locators.
5. Streamen Sie die veröffentlichten Inhalte.

### <a name="protect-content-in-storage-deliver-dynamically-encrypted-streaming-media"></a>Schützen von Inhalten im Speicher, Übermitteln dynamisch verschlüsselter Streamingmedien

1. Laden Sie eine Zwischendatei (Mezzanine File) hoher Qualität in ein Medienobjekt hoch. Wenden Sie die Speicherverschlüsselung auf das Medienobjekt an.
2. Codieren Sie das Medienobjekt in einen Satz von MP4-Dateien mit adaptiver Bitrate. Wenden Sie die Speicherverschlüsselung auf das Ausgabemedienobjekt an.
3. Erstellen Sie einen Inhaltsverschlüsselungsschlüssel für das Medienobjekt, das während der Wiedergabe dynamisch verschlüsselt werden soll.
4. Konfigurieren Sie Autorisierungsrichtlinien für Inhaltsschlüssel.
5. Konfigurieren Sie Übermittlungsrichtlinien für Medienobjekte (wird zur dynamischen Paketerstellung und zur dynamischen Verschlüsselung verwendet).
6. Veröffentlichen Sie das Medienobjekt durch Erstellen eines OnDemand-Locators.
7. Streamen Sie die veröffentlichten Inhalte.

### <a name="use-media-analytics-to-derive-actionable-insights-from-your-videos"></a>Gewinnen nützlicher Erkenntnisse aus Ihren Videos mithilfe von Media Analytics
Media Analytics ist eine Sammlung aus Sprach- und Bildanalysekomponenten, mit denen Organisationen und Unternehmen anhand von Videodateien Erkenntnisse gewinnen, aus denen sich umsetzbare Maßnahmen ableiten lassen. Weitere Informationen finden Sie unter [Azure Media Services Analytics – Übersicht](media-services-analytics-overview.md).

1. Laden Sie eine Zwischendatei (Mezzanine File) hoher Qualität in ein Medienobjekt hoch.
2. Verwenden Sie eine der folgenden Media Analytics-Dienste zum Verarbeiten Ihrer Videos:

   * **Indexer** – [Verarbeiten von Videos mit Azure Media Indexer 2](media-services-process-content-with-indexer2.md)
   * **Hyperlapse** – [Hyperlapsing von Mediendateien mit Azure Media Hyperlapse](media-services-hyperlapse-content.md)
   * **Bewegungserkennung** – [Bewegungserkennung für Azure Media Analytics](media-services-motion-detection.md)
   * **Gesichtserkennung und Gesichtsausdrücke** – [Gesichts- und Emotionenerkennung mit Azure Media Analytics](media-services-face-and-emotion-detection.md)
   * **Videozusammenfassung** – [Verwenden von Azure Media Video Thumbnails zum Erstellen einer Videozusammenfassung](media-services-video-summarization.md)
3. Media Analytics-Medienprozessoren generieren MP4- oder JSON-Dateien. Wenn ein Medienprozessor eine MP4-Datei erstellt hat, können Sie die Datei progressiv herunterladen. Wenn ein Medienprozessor eine JSON-Datei erstellt hat, können Sie die Datei aus dem Azure-Blobspeicher herunterladen.

### <a name="deliver-progressive-download"></a>Bereitstellen eines progressiven Downloads
1. Laden Sie eine Zwischendatei (Mezzanine File) hoher Qualität in ein Medienobjekt hoch.
2. Codieren Sie das Medienobjekt in eine einzelne MP4-Datei.
3. Veröffentlichen Sie das Medienobjekt durch Erstellen eines OnDemand- oder SAS-Locators.

    Wenn Sie einen SAS-Locator verwenden, wird der Inhalt aus dem Azure-Blob-Speicher heruntergeladen. In diesem Fall müssen keine Streamingendpunkte im Status „Gestartet“ vorhanden sein.
4. Führen Sie einen progressiven Download der Inhalte durch.

## <a id="live_scenarios"></a>Bereitstellen von Livestreamingereignissen mit Azure Media Services
Beim Arbeiten mit Livestreaming werden normalerweise die folgenden Komponenten verwendet:

* Eine Kamera, mit der ein Ereignis übertragen wird.
* Ein Live-Video-Encoder, der Signale von der Kamera in Datenströme konvertiert, die an einen Livestreaming-Dienst gesendet werden.

Optional: mehrere zeitsynchronisierte Liveencoder. Für bestimmte kritische Liveereignisse, die eine sehr hohe Verfügbarkeit und Quality of Experience erfordern, wird der Einsatz redundanter Aktiv-Aktiv-Encoder mit Zeitsynchronisierung empfohlen, um ein nahtloses Failover ohne Datenverlust zu erreichen.

* Ein Livestreaming-Dienst, der Ihnen Folgendes ermöglicht:
* Erfassen von Liveinhalten über verschiedene Livestreaming-Protokolle (z. B. RTMP oder Smooth Streaming)
* Optional: Codieren des Datenstroms in einen Datenstrom mit adaptiver Bitrate
* Vorschau des Livestreams
* Aufzeichnen und Speichern des erfassten Inhalts für ein späteres Streaming (Video-on-Demand)
* Übermitteln der Inhalte über häufig verwendete Streamingprotokolle (etwa MPEG DASH, Smooth oder HLS) direkt an die Kunden oder an ein Content Delivery Network (CDN) zur weiteren Verteilung

**Microsoft Azure Media Services** (AMS) können Sie Livestreaminginhalte erfassen, codieren, in der Vorschau anzeigen, speichern und bereitstellen.

Bei der Übermittlung Ihrer Inhalte für Kunden besteht Ihr Ziel darin, qualitativ hochwertige Videos für unterschiedliche Geräte unter verschiedenen Netzwerkbedingungen bereitzustellen. Damit Qualität und Netzwerkbedingungen sichergestellt werden, codieren Sie den Datenstrom mit Live-Encodern in einen Videodatenstrom mit mehreren Bitraten (adaptive Bitrate).  Verwenden Sie die [dynamische Paketerstellung](media-services-dynamic-packaging-overview.md) von Media Services, um den Datenstrom dynamisch erneut in verschiedene Protokolle zu packen. Media Services unterstützt die folgenden Technologien mit Adaptive Bitrate Streaming: HTTP Live Streaming (HLS), Smooth Streaming und MPEG DASH.

In Azure Media Services verarbeiten die **Kanäle**, **Programme** und **Streamingendpunkte** alle Livestreamingfunktionen, einschließlich Erfassung, Formatierung, DVR, Sicherheit, Skalierbarkeit und Redundanz.

Ein **Kanal** stellt eine Pipeline zum Verarbeiten von Livestreaming-Inhalten dar. Ein Kanal kann Live-Eingabedatenströme auf folgende Arten empfangen:

* Von einem lokalen Liveencoder wird Mehrfachbitraten-basiertes **RTMP** oder **Smooth Streaming** (fragmentiertes MP4) an den für **Pass-Through** konfigurierten Kanal gesendet. Bei der **Pass-Through-Übertragung** durchlaufen die erfassten Datenströme die **Kanäle** ohne weitere Transcodierung oder Codierung. Sie können die folgenden Liveencoder verwenden, von denen Smooth Streaming mit Mehrfachbitrate ausgegeben werden kann: MediaExcel, Imagine Communications, Ateme, Envivio, Cisco und Elemental. Die folgenden Liveencoder geben RTMP aus: Adobe Flash Live Encoder, Haivision, Telestream Wirecast, Teradek und Tricaster.  Ein Liveencoder kann auch einen Single-Bitrate-Datenstrom an einen Kanal senden, der nicht für die Livecodierung konfiguriert ist. Dies wird jedoch nicht empfohlen. Auf Anforderung wird der Datenstrom den Kunden von Media Services bereitgestellt.

> [!NOTE]
> Die Verwendung der Pass-Through-Methode ist die wirtschaftlichste Form des Livestreamings, wenn mehrere Ereignisse über einen längeren Zeitraum gestreamt werden und Sie bereits in lokale Encoder investiert haben. Preisdetails finden Sie [hier](https://azure.microsoft.com/pricing/details/media-services/) .
>
>

* Ein lokaler Liveencoder sendet einen Single-Bitrate-Datenstrom an den Kanal, der zum Ausführen von Livecodierung mit Media Services in einem der folgenden Formate aktiviert wurde: RTP (MPEG-TS), RTMP oder Smooth Streaming (fragmentiertes MP4). Vom Kanal wird dann eine Livecodierung des Single-Bitrate-Eingabedatenstroms in einen Multi-Bitrate-Videodatenstrom (adaptiv) ausgeführt. Auf Anforderung wird der Datenstrom den Kunden von Media Services bereitgestellt.

### <a name="working-with-channels-that-receive-multi-bitrate-live-stream-from-on-premises-encoders-pass-through"></a>Arbeiten mit Kanälen, die Livedatenströme mit mehreren Bitraten von lokalen Encodern empfangen (Pass-Through)
Das folgende Diagramm zeigt die Hauptkomponenten der AMS-Plattform, die am **Pass-Through-Workflow** beteiligt sind:

![Liveworkflow][live-overview2]

Weitere Informationen finden Sie unter [Arbeiten mit Kanälen, die Livedatenströme mit mehreren Bitraten von lokalen Encodern empfangen](media-services-live-streaming-with-onprem-encoders.md).

### <a name="working-with-channels-that-are-enabled-to-perform-live-encoding-with-azure-media-services"></a>Arbeiten mit Kanälen, die zum Ausführen von Livecodierung mit Azure Media Services aktiviert wurden
Das folgende Diagramm zeigt die Hauptkomponenten der AMS-Plattform, die in Livestreaming-Workflows beteiligt sind, wenn ein Kanal zum Ausführen der Livecodierung mit Media Services aktiviert ist.

![Liveworkflow][live-overview1]

Weitere Informationen finden Sie unter [Arbeiten mit Kanälen, die zum Ausführen von Livecodierung mit Azure Media Services aktiviert wurden](media-services-manage-live-encoder-enabled-channels.md).

## <a name="consuming-content"></a>Nutzung von Inhalten
Azure Media Services stellt die Tools zur Verfügung, die zum Erstellen leistungsstarker, dynamischer Clientplayeranwendungen für die gängigsten Plattformen erforderlich sind, darunter: iOS-Geräte, Android-Geräte, Windows, Windows Phone, Xbox und Set-Top-Boxen. Das Thema enthält Links zu SDKs und Player-Frameworks, mit denen Sie Ihre eigenen Clientanwendungen entwickeln können, die Streamingmedien aus Media Services verarbeiten.

[Entwickeln von Videoplayeranwendungen](media-services-develop-video-players.md)

## <a name="enabling-azure-cdn"></a>Aktivieren von Azure CDN
Von Media Services wird die Integration mit Azure CDN unterstützt. Informationen zum Aktivieren von Azure CDN finden Sie unter [Verwalten von Streamingendpunkten in Media Services-Konten](media-services-portal-manage-streaming-endpoints.md).

## <a name="scaling-a-media-services-account"></a>Skalieren eines Media Services-Kontos

Sie können **Media Services** skalieren, indem Sie die Anzahl **reservierter Einheiten für das Streaming** und die Anzahl **reservierter Einheiten für die Codierung** angeben, die für Ihr Konto bereitgestellt werden sollen.

Außerdem können Sie Ihr Media Services-Konto skalieren, indem Sie Speicherkonten hinzufügen. Jedes Speicherkonto ist auf 500 TB beschränkt. Um den Speicher über die Standardbeschränkungen hinaus zu erweitern, können Sie mehrere Speicherkonten mit einem einzelnen Media Services-Konto verknüpfen.
Media Services-Kunden wählen je nach ihren Anforderungen entweder einen **Standard**-Streamingendpunkt oder mindestens einen **Premium**-Streamingendpunkt. Ein Standard-Streamingendpunkt eignet sich für die meisten Streamingworkloads. Er umfasst die gleichen Features wie Premium-Streamingeinheiten. Wenn Sie über eine erweiterte Workload verfügen, Ihre Anforderungen an die Streamingkapazität nicht für die Durchsatzziele von Standard-Streamingendpunkten geeignet sind oder Sie die Kapazität des StreamingEndpoint-Diensts steuern möchten, um steigende Bandbreitenanforderungen durch die Anpassung von Skalierungseinheiten zu bewältigen (auch als Premium-Streamingeinheiten bezeichnet), empfehlen wir Ihnen die Zuordnung von Skalierungseinheiten.

[Dieses Thema](media-services-portal-scale-streaming-endpoints.md) enthält Links zu relevanten Themen.

## <a name="support"></a>Support
[Azure-Support](https://azure.microsoft.com/support/options/) bietet Supportoptionen für Azure, Media Services eingeschlossen.

## <a name="provide-feedback"></a>Feedback geben
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="service-level-agreement-sla"></a>Vereinbarung zum Servicelevel (SLA)
* Media Services gewährleistet eine Verfügbarkeit von 99,9 % für die REST-API-Transaktionen zur Media Services-Codierung.
* Das Streaming sorgt für eine erfolgreiche Verarbeitung von Dienstanforderungen mit einer garantierten Verfügbarkeit von 99,9% für vorhandene Medieninhalte, wenn ein Standard- oder Premium-Streamingendpunkt erworben wird.
* Für Livekanäle wird garantiert, dass ausgeführte Kanäle mindestens 99,9 % der Zeit über externe Konnektivität verfügen.
* Für den Schutz von Inhalten garantieren wir, dass Schlüsselanforderungen mindestens 99,9 % der Zeit erfolgreich bearbeitet werden.
* Für Indexer werden Indexer-Aufgabenanforderungen, die mit einer reservierten Einheit für die Codierung verarbeitet werden, 99,9 % der Zeit erfolgreich bearbeitet.

Weitere Informationen finden Sie im [Microsoft Azure-SLA](https://azure.microsoft.com/support/legal/sla/).

<!-- Images -->
[overview]: ./media/media-services-overview/media-services-overview.png
[vod-overview]: ./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png
[live-overview1]: ./media/media-services-live-streaming-workflow/media-services-live-streaming-new.png
[live-overview2]: ./media/media-services-live-streaming-workflow/media-services-live-streaming-current.png

