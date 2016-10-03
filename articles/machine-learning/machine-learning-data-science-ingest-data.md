<properties 
	pageTitle="Laden von Daten in Speicherumgebungen für Analysen | Microsoft Azure" 
	description="Verschieben von Daten in den und aus dem Azure-Blobspeicher" 
	services="machine-learning,storage" 
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
	ms.author="bradsev" />

# Laden von Daten in Speicherumgebungen für Analysen

Der Team Data Science-Prozess erfordert, dass Daten in einer Vielzahl von Speicherumgebungen erfasst oder geladen werden, um in jeder Phase des Prozesses auf die am besten geeignete Weise verarbeitet oder analysiert zu werden. Datenziele, die häufig für die Verarbeitung genutzt werden, sind u.a. Azure Blob Storage, SQL Azure-Datenbanken, SQL Server auf virtuellen Azure-Computern, HDInsight (Hadoop) und Azure Machine Learning.

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

Dieses **Menü** bietet Links zu Themen, in denen beschrieben wird, wie Daten in diesen Zielumgebungen erfasst werden, in denen die Daten gespeichert und verarbeitet werden.

Technische und geschäftliche Anforderungen sowie der ursprüngliche Speicherort, das Format und der Umfang Ihrer Daten bestimmen die Zielumgebungen, in denen die Daten erfasst werden müssen, um Ihre Analyseziele zu erreichen. Es ist nicht ungewöhnlich für ein Szenario, dass Daten zwischen mehreren Umgebungen verschoben werden müssen, um die verschiedenen Aufgaben zu erledigen, die zum Entwickeln eines prädiktiven Modells erforderlich sind. Zu dieser Abfolge von Aufgaben kann z. B. die Untersuchung, Vorverarbeitung, Bereinigung und das Downsampling von Daten sowie das Trainieren von Modellen gehören.

<!---HONumber=AcomDC_0921_2016-->