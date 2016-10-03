<properties 
	pageTitle="Entnehmen von Datenstichproben in Azure-Blobcontainern, SQL Server und Hive-Tabellen | Microsoft Azure" 
	description="Informationen zum Durchsuchen von Daten, die in verschiedenen Azure-Umgebungen gespeichert sind." 
	services="machine-learning" 
	documentationCenter="" 
	authors="bradsev" 
	manager="jhubbard" 
	editor="cgronlun" />

<tags 
	ms.service="machine-learning" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="09/19/2016" 
	ms.author="fashah;garye;bradsev" />

#<a name="heading"></a>Entnehmen von Datenstichproben in Azure-Blobcontainern, SQL Server und Hive-Tabellen

Dieses Dokument enthält Links zu Themen, die das Entnehmen von Stichproben aus Daten beschreiben, die an einem von drei verschiedenen Azure-Speicherorten gespeichert sind:

- **Azure Blob-Containerdaten**: Stichproben werden durch programmgesteuertes Herunterladen entnommen, um dann anschließend mit Python-Code Stichproben zu erstellen.
- **SQL Server-Daten**: Stichproben werden mithilfe von SQL und der Python-Programmiersprache erstellt.
- **Hive-Tabellendaten**: Stichproben werden mit Hive-Abfragen erstellt.

Das nachstehende **Menü** enthält Links zu Themen, die das Entnehmen von Datenstichproben aus diesen Azure-Speicherumgebungen beschreiben.

[AZURE.INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

Dieser Stichprobentask ist ein Schritt im [Team Data Science-Prozess (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

## Warum Datenstichproben entnehmen?

Wenn das Dataset, das Sie analysieren möchten, groß ist, sollten Sie in der Regel eine Komprimierung der Daten durchführen, um eine geringere aber immer noch repräsentative Größe zu erhalten. Dies erleichtert das Verständnis der Daten, das Durchsuchen und die Funktionsverarbeitung. Die Funktion besteht innerhalb des Cortana-Analyseprozesses darin, schnell Prototypen der Funktionen zur Datenverarbeitung und Modelle für das maschinelle Lernen zu erstellen.

<!---HONumber=AcomDC_0921_2016-->