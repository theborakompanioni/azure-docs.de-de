<properties
	pageTitle="Skalieren der Medienverarbeitung im klassischen Azure-Portal"
	description="Erfahren Sie, wie Sie Media Services durch Angabe der Anzahl von On-Demand Streaming Reserved Units und Encoding Reserved Units für Ihr Konto skalieren können."
	services="media-services"
	documentationCenter=""
	authors="juliako,milangada"
	manager="erikre"
	editor=""/>

<tags
	ms.service="media-services"
	ms.workload="media"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="03/29/2016"
	ms.author="juliako"/>


# Skalieren der Medienverarbeitung im klassischen Azure-Portal

## Bewältigen dieser Aufgabe mit anderen Technologien

Diese Seite bietet einen Überblick über das Skalieren der Medienverarbeitung und zeigt, wie Sie hierzu das klassische Azure-Portal nutzen können. Sie bewältigen diese Aufgabe auch mit anderen Technologien:

> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-encoding-units.md)
- [Portal](media-services-portal-encoding-units.md)
- [REST](https://msdn.microsoft.com/library/azure/dn859236.aspx)
- [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
- [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)

## Übersicht

Ein Media Services-Konto ist dem Typ einer reservierten Einheit zugeordnet, der die Geschwindigkeit bestimmt, mit der Medienverarbeitungsaufgaben erfolgen. Sie können zwischen den folgenden reservierten Einheitentypen wählen: **S1**, **S2**, or **S3**. Derselbe Codierungsauftrag wird bei Verwendung der reservierten Einheit **S2** beispielsweise schneller ausgeführt als mit dem Typ **S1**. Weitere Informationen finden Sie unter [Reserved Unit Types](https://azure.microsoft.com/blog/high-speed-encoding-with-azure-media-services/) (Typen reservierter Einheiten).

Zusätzlich zum Typ reservierter Einheiten können Sie angeben, dass für Ihr Konto reservierte Einheiten bereitgestellt werden sollen. Die Anzahl der bereitgestellten reservierten Einheiten bestimmt die Anzahl der Medienaufgaben, die gleichzeitig unter einem angegebenen Konto verarbeitet werden können. Wenn Ihr Konto beispielsweise über fünf reservierte Einheiten verfügt, werden fünf Medienaufgaben gleichzeitig ausgeführt, so lange es auszuführende Aufgaben gibt. Die restlichen Aufgaben bleiben in der Warteschlange und werden nacheinander für die Verarbeitung aufgerufen, wenn eine aktive Aufgabe abgeschlossen wird. Wenn für ein Konto keine reservierten Einheiten bereitgestellt wurden, werden die Aufgaben nacheinander aufgerufen. In diesem Fall hängt die Wartezeit zwischen dem Abschließen einer Aufgabe und dem Start der nächsten Aufgabe von der Verfügbarkeit von Ressourcen im System ab.

## Auswählen zwischen verschiedenen Typen reservierter Einheiten

Die folgende Tabelle hilft Ihnen bei der Entscheidung, wenn Sie zwischen verschiedenen Codierungsgeschwindigkeit wählen müssen. Sie enthält darüber hinaus einige Fälle für Vergleichstests sowie SAS-URLs, über die Sie Videos für eigene Tests herunterladen können:

|**S1**|**S2**|**S3**|
----------|------------|----------|------------
Beabsichtigter Anwendungsfall| Single-Bitrate-Codierung. <br/>Dateien mit SD-Auflösung oder einer niedrigeren Auflösung, nicht zeitkritisch, niedrige Kosten|Single-Bitrate- und Multi-Bitrate-Codierung.<br/>Normale Verwendung für SD- und HD-Codierung |Single-Bitrate- und Multi-Bitrate-Codierung.<br/>Videos mit Full HD- und 4K-Auflösung Zeitkritisch, schnellere Codierung 
Vergleichstest|[Eingabedatei: Dauer: 5 Minuten, 640x360p bei 29,97 Bildern pro Sekunde](https://wamspartners.blob.core.windows.net/for-long-term-share/Whistler_5min_360p30.mp4?sr=c&si=AzureDotComReadOnly&sig=OY0TZ%2BP2jLK7vmcQsCTAWl33GIVCu67I02pgarkCTNw%3D).<br/><br/>Codierung in eine Single-Bitrate-MP4-Datei mit gleicher Auflösung. Dauer: ca. 11 Minuten.|[Eingabedatei: Dauer 5 Minuten, 1280x720p bei 29,97 Bildern pro Sekunde](https://wamspartners.blob.core.windows.net/for-long-term-share/Whistler_5min_720p30.mp4?sr=c&si=AzureDotComReadOnly&sig=OY0TZ%2BP2jLK7vmcQsCTAWl33GIVCu67I02pgarkCTNw%3D)<br/><br/>Die Codierung mit der Voreinstellung „H264 Single Bitrate 720p“ dauert etwa 5 Minuten.<br/><br/>Die Codierung mit der Voreinstellung „H264 Multiple Bitrate 720p“ dauert etwa 11,5 Minuten.|[Eingabedatei: Dauer: 5 Minuten, 1920x1080p bei 29,97 Bildern pro Sekunde](https://wamspartners.blob.core.windows.net/for-long-term-share/Whistler_5min_1080p30.mp4?sr=c&si=AzureDotComReadOnly&sig=OY0TZ%2BP2jLK7vmcQsCTAWl33GIVCu67I02pgarkCTNw%3D) <br/><br/>Die Codierung mit der Voreinstellung „H264 Single Bitrate 1080p“ dauert etwa 2,7 Minuten.<br/><br/>Die Codierung mit der Voreinstellung „H264 Multiple Bitrate 1080p“ dauert etwa 5,7 Minuten.

##Überlegungen

>[AZURE.IMPORTANT] Es gelten die folgenden Bedingungen:

- Reservierte Einheiten dienen zur Parallelisierung der gesamten Medienverarbeitung, einschließlich der Indizierung von Aufträgen mit Azure Media Indexer. Allerdings wird die Indizierung von Aufträgen im Gegensatz zur Codierung mit schnelleren reservierten Einheiten nicht schneller verarbeitet.

- Bei Verwendung des gemeinsam genutzten Pools, d.h. ohne reservierte Einheiten, haben die Codierungsaufgaben die gleiche Leistung wie S1-RUs. Allerdings können sich die Aufgaben beliebig lange in der Warteschlange befinden, und es wird immer nur maximal eine Aufgabe ausgeführt.

- Der **S3**-Typ für reservierte Einheiten ist in den folgenden Rechenzentren nicht verfügbar: Brasilien, Süden; Indien, Westen; Indien, Mitte und Indien, Süden.

- Die höchste für den 24-Stunden-Zeitraum angegebene Anzahl an Einheiten wird zum Berechnen der Kosten verwendet.

## Ändern Sie den Typ reservierter Einheiten

Führen Sie folgende Schritte aus, um den Typ reservierter Einheiten und die Anzahl reservierter Einheiten zu ändern:

1. Klicken Sie im [klassischen Azure-Portal](https://manage.windowsazure.com/) auf **Media Services**. Klicken Sie anschließend auf den Namen des Media Service

2. Wählen Sie die Seite **CODIERUNG** aus.

	Um den **TYP RESERVIERTER EINHEITEN** zu ändern, wählen Sie S1, S2 oder S3.

	Verwenden Sie zum Ändern der Anzahl reservierter Einheiten für den ausgewählten Typ reservierter Einheiten den Schieberegler unter **CODIERUNG**.


	![Prozessorenseite](./media/media-services-portal-encoding-units/media-services-encoding-scale.png)

3. Klicken Sie auf die Schaltfläche "Speichern", um die Änderungen zu speichern.

	Die neuen reservierten Einheiten werden zugewiesen, wenn Sie auf SPEICHERN klicken.
 

##Kontingente und Einschränkungen

Informationen zu Kontingenten und Einschränkungen sowie zum Öffnen von Supporttickets finden Sie unter [Kontingente und Einschränkungen](media-services-quotas-and-limitations.md).



##Media Services-Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##Feedback geben

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

<!---HONumber=AcomDC_0518_2016-->