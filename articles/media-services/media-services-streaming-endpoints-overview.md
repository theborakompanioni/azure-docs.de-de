---
title: "Azure Media Services-Streamingendpunkte – Übersicht | Microsoft-Dokumentation"
description: "Dieses Thema bietet eine Übersicht über Azure Media Services-Streamingendpunkte."
services: media-services
documentationcenter: 
author: Juliako
writer: juliako
manager: SyntaxC4
editor: 
ms.assetid: 097ab5e5-24e1-4e8e-b112-be74172c2701
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: juliako
ms.translationtype: Human Translation
ms.sourcegitcommit: 1500c02fa1e6876b47e3896c40c7f3356f8f1eed
ms.openlocfilehash: cbdc964b16cbc6cf291dbc4b6f60b20d67d9fc41
ms.contentlocale: de-de
ms.lasthandoff: 06/30/2017

---
# <a name="streaming-endpoints-overview"></a>Streamingendpunkte – Übersicht 

##<a name="overview"></a>Übersicht

In Microsoft Azure Media Services (AMS) stellt ein **Streamingendpunkt** einen Streamingdienst dar, der Inhalte zur weiteren Verteilung direkt einer Clientwiedergabeanwendung oder einem Content Delivery Network (CDN) bereitstellen kann. Media Services bietet auch eine nahtlose Integration von Azure CDN. Der ausgehende Stream des StreamingEndpoint-Diensts kann ein Livestream oder ein bei Bedarf abgerufenes Video oder ein progressiver Download Ihres Medienobjekts in Ihrem Media Services-Konto sein. Jedes Azure Media Services-Konto enthält einen Standard-StreamingEndpoint. Zusätzliche StreamingEndpoints können unter dem Konto erstellt werden. Es gibt zwei Versionen von StreamingEndpoint, 1.0 und 2.0. Ab dem 10. Januar 2017 gehört zu allen neu erstellten AMS-Konten **standardmäßig** StreamingEndpoint in Version 2.0. Zusätzliche Streamingendpunkte, die Sie diesem Konto hinzufügen, haben auch die Version 2.0. Diese Änderung hat keine Auswirkung auf vorhandene Konten. Vorhandenen Streamingendpunkte haben die Version 1.0 und können auf Version 2.0 aktualisiert werden. Durch diese Änderung ergeben sich Änderungen am Verhalten, an der Abrechnung und an Features (weitere Informationen finden Sie im nachstehenden Abschnitt **Streamingtypen und -versionen**).

Darüber wurden in der (im Januar 2017 veröffentlichten) Azure Media Services-Version 2.15 der Entität „Streamingendpunkt“ die folgenden Eigenschaften hinzugefügt: **CdnProvider**, **CdnProfile**, **FreeTrialEndTime** und **StreamingEndpointVersion**. Eine detaillierte Übersicht über diese Eigenschaften finden Sie [hier](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint). 

Beim Erstellen eines Azure Media Services-Kontos wird ein Standard-Streamingendpunkt im Zustand **Beendet** erstellt. Der Standard-Streamingendpunkt kann nicht gelöscht werden. Abhängig von der Verfügbarkeit von Azure CDN in der Zielregion bietet der neu erstellte Standard-Streamingendpunkt standardmäßig eine Integration mit dem CDN-Anbieter „StandardVerizon“. 

>[!NOTE]
>Vor dem Starten des Streamingendpunkts kann die Azure CDN-Integration deaktiviert werden.

Dieses Thema bietet einen Überblick über die wichtigsten Funktionen, die von Streamingendpunkten bereitgestellt werden.

## <a name="streaming-types-and-versions"></a>Streamingtypen und -versionen

### <a name="standardpremium-types-version-20"></a>Standard-/Premium-Typen (Version 2.0)

Ab der Media Services-Version vom Januar 2017 gibt es zwei Streamingtypen: **Standard** und **Premium**. Diese Typen sind Teil der Streamingendpunktversion „2.0“.

Typ|Beschreibung
---|---
**Standard**|Dies ist die Standardoption, die für die Mehrzahl der Szenarien funktioniert.<br/>Bei dieser Option erhalten Sie eine feste/begrenzte SLA. Die ersten 15 Tage nach Starten des Streamingendpunkts sind kostenlos.<br/>Wenn Sie mehrere Streamingendpunkte erstellen, ist nur der erste die ersten 15 Tage kostenlos. Die anderen werden nach ihrer Inbetriebnahme in Rechnung gestellt. <br/>Beachten Sie, dass die kostenlose Testversion nur für neu erstellte Media Services-Konten und den Standard-Streamingendpunkt gilt. Für vorhandene Streamingendpunkte und zusätzlich erstellte Streamingendpunkte gibt es keinen kostenlosen Testzeitraum, auch wenn diese auf Version 2.0 aktualisiert oder mit Version 2.0 erstellt wurden.
**Premium**|Diese Option eignet sich für professionelle Szenarien, die eine höhere Skalierung oder mehr Steuerung erfordern.<br/>Eine variable SLA, die auf erworbener Premium-Streamingeinheitskapazität basiert, dedizierte Streamingendpunkte in einer isolierten Umgebung und keine Konkurrenz um Ressourcen.

Ausführlichere Informationen finden Sie im nachfolgenden Abschnitt **Vergleich der Streamingtypen**.

### <a name="classic-type-version-10"></a>Klassischer Typ (Version 1.0)

Für Benutzer, die vor der Version vom 10. Januar 2017 AMS-Konten erstellt haben, verfügen Sie über den **klassischen** Typ von Streamingendpunkt. Dieser Typ ist Teil der Streamingendpunktversion „1.0“.

Wenn Ihr Streamingendpunkt der **Version 1.0** mindestens eine Premium-Streamingeinheit aufweist, handelt es sich um den Premium-Streamingendpunkt, der ohne zusätzliche Konfigurationsschritte alle AMS-Features bietet (genau wie der Typ **Standard/Premium**.

>[!NOTE]
>**Klassische** Streamingendpunkte (Version 1.0 ohne Streameinheit) bieten eingeschränkte Features und keine SLA. Es wird empfohlen, zum Typ **Standard** zu migrieren, um sich eine bessere Umgebung zu sichern und Features wie die dynamische Paketerstellung oder Verschlüsselung zu nutzen, die der Typ **Standard** bietet. Um zum Typ **Standard** zu migrieren, wechseln Sie zum [Azure-Portal](https://portal.azure.com/) und wählen **Standard abonnieren** aus. Weitere Informationen zur Migration finden Sie im Abschnitt [Migration](#migration-between-types).
>
>Beachten Sie, dass dieser Vorgang nicht rückgängig gemacht werden kann und Auswirkungen auf die Kosten hat.
>
 
## <a name="comparing-streaming-types"></a>Vergleichen von Streamingtypen

### <a name="versions"></a>Versionen

|Typ|StreamingEndpointVersion|ScaleUnits|CDN|Abrechnung|SLA| 
|--------------|----------|-----------------|-----------------|-----------------|-----------------|    
|Klassisch|1,0|0|NA|Kostenlos|NA|
|Standard-Streamingendpunkt|2,0|0|Ja|Kostenpflichtig|Ja|
|Premium-Streamingeinheiten|1,0|>0|Ja|Kostenpflichtig|Ja|
|Premium-Streamingeinheiten|2,0|>0|Ja|Kostenpflichtig|Ja|

### <a name="features"></a>Features

Feature|Standard|Premium
---|---|---
Erste 15 Tage kostenlos| Ja |Nein
Durchsatz |Bis zu 600 Mbit/s, wenn Azure CDN nicht verwendet wird. Wird mit CDN skaliert.|200 Mbit/s pro Streamingeinheit. Wird mit CDN skaliert.
SLA | 99,9|99,9 (200 Mbit/s pro Streamingeinheit).
CDN|Azure CDN, CDN eines Drittanbieters oder kein CDN.|Azure CDN, CDN eines Drittanbieters oder kein CDN.
Die Abrechnung erfolgt anteilsmäßig| Täglich|Täglich
Dynamische Verschlüsselung|Ja|Ja
Dynamische Paketerstellung|Ja|Ja
Skalieren|Automatische Skalierung bis zum Zieldurchsatz.|Zusätzliche Streamingeinheiten
IP-Filterung/G20/Benutzerdefinierter Host|Ja|Ja
Progressiver Download|Ja|Ja
Empfohlene Verwendung |Für den Großteil der Streamingszenarien empfohlen.|Professionelle Nutzung.<br/>Wenn Sie glauben, dass Ihre Anforderungen über „Standard“ hinausgehen. Kontaktieren Sie uns (amsstreaming bei microsoft.com, wenn Sie eine gleichzeitige Zielgruppe von mehr als 50.000 Teilnehmern erwarten.


## <a name="migration-between-types"></a>Migration zwischen Typen

Aus | Zu | Aktion
---|---|---
Klassisch|Standard|Muss abonnieren
Klassisch|Premium| Skalierung (Zusätzliche Streamingeinheiten)
Standard/Premium|Klassisch|Nicht verfügbar (bei Streamingendpunktversion 1.0. Es ist zulässig, zu „Klassisch“ zu wechseln, wenn die Einstellung „scaleunits“ auf „0“ festgelegt ist)
Standard (mit/ohne CDN)|Premium mit identischen Konfigurationen|Zulässig im Zustand **Gestartet**. (über Azure-Portal)
Premium (mit/ohne CDN)|Standard mit identischen Konfigurationen|Zulässig im Zustand **Gestartet** (über Azure-Portal)
Standard (mit/ohne CDN)|Premium mit anderen Konfigurationen|Zulässig im Zustand **Beendet** (über Azure-Portal) Im Zustand „Wird ausgeführt“ nicht zulässig.
Premium (mit/ohne CDN)|Standard mit anderen Konfigurationen|Zulässig im Zustand **Beendet** (über Azure-Portal) Im Zustand „Wird ausgeführt“ nicht zulässig.
Version 1.0 mit mindestens einer Streamingeinheit mit CDN|Standard-/Premium ohne CDN|Zulässig im Zustand **Beendet**. Nicht zulässig im Zustand **Gestartet**.
Version 1.0 mit mindestens einer Streamingeinheit mit CDN|Standard mit/ohne CDN|Zulässig im Zustand **Beendet**. Nicht zulässig im Zustand **Gestartet**. CDN mit Version 1.0 wird gelöscht und ein neues erstellt und gestartet.
Version 1.0 mit mindestens einer Streamingeinheit mit CDN|Premium mit/ohne CDN|Zulässig im Zustand **Beendet**. Nicht zulässig im Zustand **Gestartet**. Klassisches CDN wird gelöscht und ein neues erstellt und gestartet.

## <a name="next-steps"></a>Nächste Schritte
Überprüfen Sie die Media Services-Lernpfade.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geben
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


