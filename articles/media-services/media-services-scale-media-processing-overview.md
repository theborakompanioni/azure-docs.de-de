<properties
	pageTitle="Übersicht über das Skalieren der Medienverarbeitung | Microsoft Azure"
	description="Dieses Thema bietet eine Übersicht über die Skalierung der Medienverarbeitung mithilfe von Azure Media Services."
	services="media-services"
	documentationCenter=""
	authors="juliako"
	manager="erikre"
	editor=""/>

<tags
	ms.service="media-services"
	ms.workload="media"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/29/2016"
	ms.author="juliako"/>


# Übersicht über das Skalieren der Medienverarbeitung

Diese Seite bietet eine Übersicht über die Gründe und Vorgehensweise bei der Skalierung der Medienverarbeitung.

## Übersicht

Ein Media Services-Konto ist dem Typ einer reservierten Einheit zugeordnet, der die Geschwindigkeit bestimmt, mit der Medienverarbeitungsaufgaben erfolgen. Sie können zwischen den folgenden reservierten Einheitentypen wählen: **S1**, **S2**, or **S3**. Derselbe Codierungsauftrag wird bei Verwendung der reservierten Einheit **S2** beispielsweise schneller ausgeführt als mit dem Typ **S1**. Weitere Informationen finden Sie unter [Reserved Unit Types](https://azure.microsoft.com/blog/high-speed-encoding-with-azure-media-services/) (Typen reservierter Einheiten).

Zusätzlich zum Typ reservierter Einheiten können Sie angeben, dass für Ihr Konto reservierte Einheiten bereitgestellt werden sollen. Die Anzahl der bereitgestellten reservierten Einheiten bestimmt die Anzahl der Medienaufgaben, die gleichzeitig unter einem angegebenen Konto verarbeitet werden können. Wenn Ihr Konto beispielsweise über fünf reservierte Einheiten verfügt, werden fünf Medienaufgaben gleichzeitig ausgeführt, solange es auszuführende Aufgaben gibt. Die restlichen Aufgaben bleiben in der Warteschlange und werden nacheinander für die Verarbeitung aufgerufen, wenn eine aktive Aufgabe abgeschlossen wird. Wenn für ein Konto keine reservierten Einheiten bereitgestellt wurden, werden die Aufgaben nacheinander aufgerufen. In diesem Fall hängt die Wartezeit zwischen dem Abschließen einer Aufgabe und dem Start der nächsten Aufgabe von der Verfügbarkeit von Ressourcen im System ab.

## Auswählen zwischen verschiedenen Typen reservierter Einheiten

Die folgende Tabelle hilft Ihnen bei der Entscheidung, wenn Sie zwischen verschiedenen Codierungsgeschwindigkeit wählen müssen. Sie enthält darüber hinaus einige Fälle für Vergleichstests sowie SAS-URLs, über die Sie Videos für eigene Tests herunterladen können:

Szenarios|**S1**|**S2**|**S3**|
----------|------------|----------|------------
Beabsichtigter Anwendungsfall| Single-Bitrate-Codierung. <br/>Dateien mit SD-Auflösung oder einer niedrigeren Auflösung, nicht zeitkritisch, niedrige Kosten.|Single-Bitrate- und Multi-Bitrate-Codierung.<br/>Normale Verwendung für SD- und HD-Codierung. |Single-Bitrate- und Multi-Bitrate-Codierung.<br/>Videos mit Full HD- und 4K-Auflösung. Zeitkritisch, schnellere Codierung 
Vergleichstest|[Eingabedatei: Dauer: 5 Minuten, 640x360p bei 29,97 Bildern pro Sekunde](https://wamspartners.blob.core.windows.net/for-long-term-share/Whistler_5min_360p30.mp4?sr=c&si=AzureDotComReadOnly&sig=OY0TZ%2BP2jLK7vmcQsCTAWl33GIVCu67I02pgarkCTNw%3D).<br/><br/>Codierung in eine Single-Bitrate-MP4-Datei mit gleicher Auflösung. Dauer: ca. 11 Minuten.|[Eingabedatei: Dauer 5 Minuten, 1280x720p bei 29,97 Bildern pro Sekunde](https://wamspartners.blob.core.windows.net/for-long-term-share/Whistler_5min_720p30.mp4?sr=c&si=AzureDotComReadOnly&sig=OY0TZ%2BP2jLK7vmcQsCTAWl33GIVCu67I02pgarkCTNw%3D)<br/><br/>Die Codierung mit der Voreinstellung „H264 Single Bitrate 720p“ dauert etwa 5 Minuten.<br/><br/>Die Codierung mit der Voreinstellung „H264 Multiple Bitrate 720p“ dauert etwa 11,5 Minuten.|[Eingabedatei: Dauer: 5 Minuten, 1920x1080p bei 29,97 Bildern pro Sekunde](https://wamspartners.blob.core.windows.net/for-long-term-share/Whistler_5min_1080p30.mp4?sr=c&si=AzureDotComReadOnly&sig=OY0TZ%2BP2jLK7vmcQsCTAWl33GIVCu67I02pgarkCTNw%3D). <br/><br/>Die Codierung mit der Voreinstellung „H264 Single Bitrate 1080p“ dauert etwa 2,7 Minuten.<br/><br/>Die Codierung mit der Voreinstellung „H264 Multiple Bitrate 1080p“ dauert etwa 5,7 Minuten.

##Überlegungen

>[AZURE.IMPORTANT] Prüfen Sie die in diesem Abschnitt beschriebenen Überlegungen.

- Reservierte Einheiten dienen zur Parallelisierung der gesamten Medienverarbeitung, einschließlich der Indizierung von Aufträgen mit Azure Media Indexer. Allerdings erfolgt die Indizierung von Aufträgen im Gegensatz zur Codierung mit schnelleren reservierten Einheiten nicht schneller.

- Bei Verwendung des gemeinsam genutzten Pools, d.h. ohne reservierte Einheiten, haben die Codierungsaufgaben die gleiche Leistung wie S1-RUs. Allerdings können sich die Aufgaben beliebig lange in der Warteschlange befinden, und es wird immer nur maximal eine Aufgabe ausgeführt.

- Der **S2**-Typ für reservierte Einheiten ist in den folgenden Rechenzentren nicht verfügbar: Brasilien, Süden; Indien, Westen; Indien, Mitte; Indien, Süden.

- Der **S3**-Typ für reservierte Einheiten ist in den folgenden Rechenzentren nicht verfügbar: Brasilien, Süden; Indien, Westen; Indien, Mitte.

- Die höchste für den 24-Stunden-Zeitraum angegebene Anzahl an Einheiten wird zum Berechnen der Kosten verwendet.


##Kontingente und Einschränkungen

Informationen zu Kontingenten und Einschränkungen sowie zum Öffnen von Supporttickets finden Sie unter [Kontingente und Einschränkungen](media-services-quotas-and-limitations.md).

##Nächster Schritt

Führen Sie das Skalieren der Medienverarbeitung mit einer dieser Technologien durch:

> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-encoding-units.md)
- [Portal](media-services-portal-scale-media-processing.md)
- [REST](https://msdn.microsoft.com/library/azure/dn859236.aspx)
- [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
- [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)

##Media Services-Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##Feedback geben

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

<!---HONumber=AcomDC_0907_2016-->