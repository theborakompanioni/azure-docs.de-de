<properties 
   pageTitle="Tabelle zu Beschränkungen für Stream Analytics"
   description="Beschreibung der Systembegrenzungen und empfohlenen Größen für Stream Analytics-Komponenten und -Verbindungen."
   services="stream-analytics"
   documentationCenter="NA"
   authors="jeffstokes72"
   manager="paulettm"
   editor="cgronlun" />
<tags 
   ms.service="stream-analytics"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="big-data"
   ms.date="07/25/2016"
   ms.author="jeffstok" />

| Begrenzungsbezeichner | Begrenzung | Kommentare |
|----------------- | ------------|--------- |
| Maximale Anzahl der Streamingeinheiten pro Abonnement und Region | 50 | Beim [Microsoft Support](https://support.microsoft.com/de-DE) können Sie die Erhöhung der Streamingeinheiten auf über 50 für Ihr Abonnement anfordern. |
| Maximaler Durchsatz einer Streamingeinheit | 1 MB/s* | Der maximale Durchsatz pro Streamingeinheit hängt vom jeweiligen Szenario ab. Der tatsächliche Durchsatz kann geringer ausfallen und hängt von der Komplexität der Abfragen und von der Partitionierung ab. Weitere Informationen finden Sie im Artikel [Skalieren von Azure Stream Analytics-Aufträgen](../articles/stream-analytics/stream-analytics-scale-jobs.md). |
| Maximale Anzahl der Eingaben pro Auftrag | 60 | Es gibt eine harte Grenze von 60 Eingaben pro Stream Analytics-Auftrag. |
| Maximale Anzahl der Ausgaben pro Auftrag | 60 | Es gibt eine harte Grenze von 60 Ausgaben pro Stream Analytics-Auftrag. |
| Maximale Anzahl von Funktionen pro Auftrag | 60 | Es gibt eine harte Grenze von 60 Ausgaben pro Stream Analytics-Auftrag. |
| Maximale Anzahl von Aufträgen pro Region | 1500 | Jedes Abonnement kann bis zu 1.500 Aufträge pro geografischer Region haben. |

<!---HONumber=AcomDC_0727_2016-->