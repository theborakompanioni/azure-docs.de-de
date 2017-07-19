---
title: Azure Media Services-Telemetrie | Microsoft Docs
description: "Dieser Artikel bietet eine Übersicht über die Azure Media Services-Telemetrie."
services: media-services
documentationcenter: 
author: Juliako
manager: erikre
editor: 
ms.assetid: 95c20ec4-c782-4063-8042-b79f95741d28
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: juliako
ms.translationtype: HT
ms.sourcegitcommit: 1500c02fa1e6876b47e3896c40c7f3356f8f1eed
ms.openlocfilehash: bc16ef727f0c3942b0be8c633717fd52da246c55
ms.contentlocale: de-de
ms.lasthandoff: 07/19/2017

---

# <a name="azure-media-services-telemetry"></a>Azure Media Services-Telemetrie

Mit Azure Media Services (AMS) können Sie auf Telemetrie-/Metrikdaten zugreifen. Mit der aktuellen Version der AMS können Sie Telemetriedaten für **Kanal**-, **StreamingEndpoint**- und **Archive**-Live-Entitäten sammeln. 

Telemetrie wird in einem Azure Storage-Konto in eine Speichertabelle geschrieben, die Sie angeben (normalerweise verwenden Sie das mit Ihrem AMS-Konto verknüpfte Speicherkonto). 

Das Telemetriesystem verwaltet keine Beibehaltung der Daten. Sie können die alten Telemetriedaten durch Löschen der Speichertabellen entfernen.

In diesem Thema wird das Konfigurieren und Nutzen der AMS-Telemetrie erläutert.

## <a name="configuring-telemetry"></a>Konfigurieren der Telemetrie

Die Telemetrie kann auf Komponentenebene konfiguriert werden. Dabei stehen zwei Detailstufen zur Verfügung: „Normal“ und „Ausführlich“. Derzeit geben beide Ebenen die gleichen Informationen zurück. Empfohlen wird die Verwendung von „Normal“. 

Die folgenden Themen zeigen, wie die Telemetrie aktiviert wird:

[Aktivieren der Telemetrie mit .NET](media-services-dotnet-telemetry.md) 

[Aktivieren der Telemetrie mit REST](media-services-rest-telemetry.md)

## <a name="consuming-telemetry-information"></a>Verwenden von Telemetriedaten

Die Telemetriedaten werden in dem Speicherkonto, das Sie bei der Konfiguration der Telemetrie für das Media Services-Konto angegeben haben, in eine Azure Storage-Tabelle geschrieben. In diesem Abschnitt werden die Speichertabellen für die Metriken beschrieben.

Sie können Telemetriedaten in einer der folgenden Arten verwenden:

- Lesen Sie Daten direkt aus dem Azure-Tabellenspeicher (z.B. mithilfe des Speicher-SDK). Eine Beschreibung der Telemetriespeichertabellen finden Sie in [diesem](https://msdn.microsoft.com/library/mt742089.aspx) Thema unter **Consuming telemetry information** (Nutzung von Telemetrieinformationen).

oder

- Verwenden Sie – wie in [diesem](media-services-dotnet-telemetry.md) Thema beschrieben – die Unterstützung für das Lesen von Speicherdaten im .NET SDK der Media Services. 


Das unten beschriebene Telemetrieschema soll gute Leistung innerhalb der Grenzen von Azure Table Storage garantieren:

- Daten werden durch Konto-ID und Dienst-ID partitioniert, damit die Telemetriedaten aus jedem Dienst unabhängig voneinander abgefragt werden können.
- Die Partitionen enthalten das Datum, um eine sinnvolle obere Grenze der Partitionsgröße festzulegen.
- Zeilenschlüssel sind in umgekehrter zeitlicher Reihenfolge angeordnet, damit die neuesten Telemetrieelemente für einen bestimmten Dienst abgefragt werden.

Folgendes sorgt für die Effizienz vieler gängiger Abfragen:

- Paralleles, unabhängiges Herunterladen von Daten für verschiedene Dienste.
- Abrufen aller Daten für einen bestimmten Dienst in einem bestimmten Datumsbereich.
- Abrufen der neuesten Daten für einen Dienst.

### <a name="telemetry-table-storage-output-schema"></a>Ausgabeschema des Telemetrietabellenspeichers

Telemetriedaten werden in aggregierter Form in einer Tabelle „TelemetryMetrics20160321“ gespeichert, wobei „20160321“ das Datum der erstellten Tabelle ist. Das Telemetriesystem erstellt für jeden neuen Tag (Grundlage: 00:00 UTC) eine separate Tabelle. Die Tabelle wird verwendet, um wiederholte Werte wie z.B. Erfassungsbitrate innerhalb eines bestimmten Zeitfensters, gesendete Bytes usw. zu speichern. 

Eigenschaft|Wert|Beispiele/Hinweise
---|---|---
PartitionKey|{Konto-ID}_{Entitäts-ID}|e49bef329c29495f9b9570989682069d_64435281c50a4dd8ab7011cb0f4cdf66<br/<br/>Die Konto-ID ist im Partitionsschlüssel enthalten, um Workflows zu vereinfachen, in denen mehrere Media Services-Konten in das gleiche Speicherkonto schreiben.
Zeilenschlüssel|{Sekunden bis Mitternacht}_{Zufallswert}|01688_00199<br/><br/>Der Zeilenschlüssel beginnt mit der Anzahl von Sekunden bis Mitternacht, um Abfragen nach den „oberen n“ innerhalb einer Partition zu ermöglichen. Weitere Informationen dazu finden Sie in [diesem Artikel](../storage/storage-table-design-guide.md#log-tail-pattern). 
Zeitstempel|Datum/Uhrzeit|Zeitstempel von Azure-Tabelle 2016-09-09T22:43:42.241Z
Typ|Der Typ der Entität, die Telemetriedaten liefert|Kanal/StreamingEndpoint/Archiv<br/><br/>Der Ereignistyp ist einfach ein Zeichenfolgenwert.
Name|Der Name des Telemetrieereignisses|ChannelHeartbeat/StreamingEndpointRequestLog
ObservedTime|Die Uhrzeit (UTC), zu der das Telemetrieereignis aufgetreten ist.|2016-09-09T22:42:36.924Z<br/><br/>Die beobachtete Zeit wird von der Entität gemeldet, die die Telemetriedaten sendet (z.B. ein Kanal). Da zwischen Komponenten Zeitsynchronisierungsprobleme auftreten können, ist dies ein ungefährer Wert.
ServiceID|{Service-ID}|f70bd731-691d-41c6-8f2d-671d0bdc9c7e
Entitätsspezifische Eigenschaften|Gemäß der Definition durch das-Ereignis|StreamName: stream1, Bitrate 10123, …<br/><br/>Die übrigen Eigenschaften sind für den angegebenen Ereignistyp definiert. Der Azure-Tabelleninhalt besteht aus Schlüssel-Wert-Paaren.  (D.h., unterschiedliche Zeilen in der Tabelle haben unterschiedliche Sätze von Eigenschaften.)

### <a name="entity-specific-schema"></a>Entitätsspezifisches Schema

Es gibt drei Arten von entitätsspezifischen telemetrischen Dateneinträgen, die jeweils mit folgender Häufigkeit abgelegt werden:

- Streamingendpunkte: alle 30 Sekunden
- Livekanäle: jede Minute
- Livearchiv: jede Minute

**Streamingendpunkt**

Eigenschaft|Wert|Beispiele
---|---|---
PartitionKey|PartitionKey|e49bef329c29495f9b9570989682069d_64435281c50a4dd8ab7011cb0f4cdf66
Zeilenschlüssel|Zeilenschlüssel|01688_00199
Zeitstempel|Zeitstempel|Automatischer Zeitstempel von Azure-Tabelle 2016-09-09T22:43:42.241Z
Typ|Typ|StreamingEndpoint
Name|Name|StreamingEndpointRequestLog
ObservedTime|ObservedTime|2016-09-09T22:42:36.924Z
ServiceID|Service-ID|f70bd731-691d-41c6-8f2d-671d0bdc9c7e
HostName|Hostname des Endpunkts|builddemoserver.origin.mediaservices.windows.net
StatusCode|Zeichnet HTTP-Status auf.|200
ResultCode|Ergebniscodedetail|S_OK
RequestCount|Gesamte Anforderungen in der Aggregation|3
BytesSent|Aggregierte gesendete Bytes|2987358
ServerLatency|Durchschnittliche Serverlatenz (einschließlich Speicher)|129
E2ELatency|Durchschnittliche End-to-End-Latenz|250

**Livekanal**

Eigenschaft|Wert|Beispiele/Hinweise
---|---|---
PartitionKey|PartitionKey|e49bef329c29495f9b9570989682069d_64435281c50a4dd8ab7011cb0f4cdf66
Zeilenschlüssel|Zeilenschlüssel|01688_00199
Zeitstempel|Zeitstempel|Zeitstempel von Azure-Tabelle 2016-09-09T22:43:42.241Z
Typ|Typ|Kanal
Name|Name|ChannelHeartbeat
ObservedTime|ObservedTime|2016-09-09T22:42:36.924Z
ServiceID|Service-ID|f70bd731-691d-41c6-8f2d-671d0bdc9c7e
TrackType|Typ der Nachverfolgung Video/Audio/Text|Video/Audio
TrackName|Name der Nachverfolgung|video/audio_1
Bitrate|Nachverfolgungs-Bitrate|785000
CustomAttributes||   
IncomingBitrate|Tatsächliche Eingangsbitrate|784548
OverlapCount|Überlappung bei Erfassung|0
DiscontinuityCount|Diskontinuität für Nachverfolgung|0
LastTimestamp|Zeitstempel der letzten erfassten Daten|1800488800
NonincreasingCount|Anzahl der Fragmente, die verworfen wurden, weil der Zeitstempel nicht heraufgesetzt wurde.|2
UnalignedKeyFrames|Ob Fragmente (aller Qualitätsstufen) empfangen wurden, deren Keyframes nicht ausgerichtet waren. |true
UnalignedPresentationTime|Ob Fragmente (aller Qualitätsstufen/Nachverfolgungen) empfangen wurden, deren Präsentationszeit nicht ausgerichtet war.|true
UnexpectedBitrate|„True“, wenn die berechnete/tatsächliche Bitrate für die Audio-/Videonachverfolgung > 40.000 Bit/s und IncomingBitrate == 0 OR IncomingBitrate und actualBitrate sich um 50% unterscheiden. |true
Healthy|„True“, wenn <br/>overlapCount, <br/>DiscontinuityCount, <br/>NonIncreasingCount, <br/>UnalignedKeyFrames, <br/>UnalignedPresentationTime, <br/>UnexpectedBitrate<br/> alle 0 sind.|true<br/><br/>„Healthy“ ist eine zusammengesetzte Funktion, die „false“ zurückgibt, wenn eine der folgenden Bedingungen zutrifft:<br/><br/>- OverlapCount > 0<br/>- DiscontinuityCount > 0<br/>- NonincreasingCount > 0<br/>- UnalignedKeyFrames == True<br/>- UnalignedPresentationTime == True<br/>- UnexpectedBitrate == True

**Livearchiv**

Eigenschaft|Wert|Beispiele/Hinweise
---|---|---
PartitionKey|PartitionKey|e49bef329c29495f9b9570989682069d_64435281c50a4dd8ab7011cb0f4cdf66
Zeilenschlüssel|Zeilenschlüssel|01688_00199
Zeitstempel|Zeitstempel|Zeitstempel von Azure-Tabelle 2016-09-09T22:43:42.241Z
Typ|Typ|Archivieren
Name|Name|ArchiveHeartbeat
ObservedTime|ObservedTime|2016-09-09T22:42:36.924Z
ServiceID|Service-ID|f70bd731-691d-41c6-8f2d-671d0bdc9c7e
ManifestName|Programm-URL|asset-eb149703-ed0a-483c-91c4-e4066e72cce3/a0a5cfbf-71ec-4bd2-8c01-a92a2b38c9ba.ism
TrackName|Name der Nachverfolgung|audio_1
TrackType|Typ der Nachverfolgung|Audio/Video
CustomAttribute|Eine hexadezimale Zeichenfolge, die zwischen verschiedenen Nachverfolgungen mit gleichem Namen und gleicher Bitrate (mehrere Kamerawinkel) unterscheidet.|
Bitrate|Nachverfolgungs-Bitrate|785000
Healthy|„True“, wenn FragmentDiscardedCount == 0 && ArchiveAcquisitionError == „False“|„True“ (diese beiden Werte sind in der Metrik nicht vorhanden, jedoch im Quellereignis)<br/><br/>„Healthy“ ist eine zusammengesetzte Funktion, die „false“ zurückgibt, wenn eine der folgenden Bedingungen zutrifft:<br/><br/>-FragmentDiscardedCount > 0<br/>- ArchiveAcquisitionError == True

## <a name="general-qa"></a>Allgemeine Fragen und Antworten

### <a name="how-to-consume-metrics-data"></a>Wie nutze ich Metrikdaten?

Metrikdaten werden als eine Reihe von Azure-Tabellen im Speicherkonto des Kunden gespeichert. Diese Daten können mit den folgenden Tools verwendet werden:

- AMS SDK
- Microsoft Azure Storage Explorer (unterstützt das Exportieren in durch Trennzeichen getrennte Dateien [CSV-Format] und die Verarbeitung in Excel)
- REST-API

### <a name="how-to-find-average-bandwidth-consumption"></a>Wie ermittle ich den durchschnittlichen Bandbreitenbedarf?

Der durchschnittliche Bandbreitenbedarf ist der Durchschnitt von BytesSent in einem bestimmten Zeitraum.

### <a name="how-to-define-streaming-unit-count"></a>Wie definiere ich die Anzahl der Streamingeinheiten?

Die Anzahl der Streamingeinheiten kann als Ergebnis der Division des Spitzendurchsatzes der Streamingendpunkte des Diensts durch den Spitzendurchsatz eines Streamingendpunkts definiert werden. Der nutzbare Spitzendurchsatz eines Streamingendpunkts ist 160MBit/s.
Nehmen Sie z.B. an, dass der Spitzendurchsatz des Diensts eines Kunden 40MBit/s beträgt (der maximale Wert von BytesSent in einem bestimmten Zeitraum). Dann entspricht die Anzahl der Streamingeinheiten (40MBit/s)*(8 Bits/Byte)/(160MBit/s) = 2 Streamingeinheiten.

### <a name="how-to-find-average-requestssecond"></a>Wie ermittle ich die durchschnittliche Zahl der Anforderungen/Sekunde?

Um die durchschnittliche Anzahl von Anforderungen/Sekunde zu ermitteln, berechnen Sie die durchschnittliche Anzahl der Anforderungen (RequestCount) in einem bestimmten Zeitraum.

### <a name="how-to-define-channel-health"></a>Wie wird Kanalintegrität definiert?

Kanalintegrität kann als zusammengesetzte boolesche Funktion definiert werden, derart, dass es „false“ ist, wenn eine der folgenden Bedingungen zutrifft:

- OverlapCount > 0
- DiscontinuityCount > 0
- NonincreasingCount > 0
- UnalignedKeyFrames == True 
- UnalignedPresentationTime == True 
- UnexpectedBitrate == True


### <a name="how-to-detect-discontinuities"></a>Wie erkenne ich Diskontinuitäten?

Suchen Sie zum Erkennen von Diskontinuitäten alle Kanaldateneinträge, in denen DiscontinuityCount > 0 ist. Der entsprechende ObservedTime-Zeitstempel gibt die Zeiten an, zu denen die Diskontinuitäten aufgetreten sind.

### <a name="how-to-detect-timestamp-overlaps"></a>Wie erkenne ich Zeitstempelüberlappungen?

Suchen Sie zum Erkennen von Zeitstempelüberlappungen alle Kanaldateneinträge, in denen OverlapCount > 0. Der entsprechende ObservedTime-Zeitstempel gibt die Zeiten an, zu denen die Zeitstempelüberlappungen aufgetreten sind.

### <a name="how-to-find-streaming-request-failures-and-reasons"></a>Wie ermittle ich Streaminganforderungsfehler und deren Gründe?

Um Streaminganforderungsfehler und deren Gründe zu ermitteln, suchen Sie alle Streamingendpunkt-Dateneinträge, in denen ResultCode nicht gleich „S_OK“ ist. Das entsprechende StatusCode-Feld gibt den Grund für den Anforderungsfehler an.

### <a name="how-to-consume-data-with-external-tools"></a>Wie nutze ich Daten mit externen Tools?

Telemetrische Daten können mit den folgenden Tools verarbeitet und visuell dargestellt werden:

- PowerBI
- Application Insights
- Azure Monitor (früher Shoebox)
- AMS-Livedashboard
- Azure-Portal (ausstehendes Release)

### <a name="how-to-manage-data-retention"></a>Wie verwalte ich die Beibehaltung der Daten?

Das Telemetriesystem bietet weder eine Verwaltung der Datenbeibehaltung noch die automatische Löschung alter Datensätze. Daher müssen Sie alte Datensätze manuell von der Speichertabelle aus verwalten und löschen. Die Vorgehensweise können Sie dem Speicher-SDK entnehmen.

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geben

[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

