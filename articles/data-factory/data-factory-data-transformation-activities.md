<properties 
	pageTitle="Datentransformation: Verarbeiten und Transformieren von Daten | Microsoft Azure" 
	description="Hier finden Sie Informationen zur Datentransformation in Azure Data Factory. Transformieren und Verarbeiten Sie Daten in einem Azure HDInsight-Cluster oder in Azure Batch." 
	keywords="Datentransformation, Daten verarbeiten, Daten transformieren, Transformationsaktivität"
	services="data-factory" 
	documentationCenter="" 
	authors="spelluru" 
	manager="jhubbard" 
	editor="monicar"/>

<tags 
	ms.service="data-factory" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="09/12/2016" 
	ms.author="spelluru"/>

# Transformieren und Analysieren von Daten in Azure Data Factory
Dieser Artikel enthält Informationen zu Datentransformationsaktivitäten in Azure Data Factory und erklärt, wie Azure Data Factory Ihre Rohdaten in Prognosen und Einblicke verwandelt. Sie werden in einer Compute-Umgebung wie Azure HDInsight-Cluster oder einem Azure Batch ausgeführt. Darüber hinaus finden Sie hier Links zu einigen Artikeln, in denen die Verwendung bestimmter Transformationsaktivitäten erläutert wird.
 
Azure Data Factory unterstützt die folgenden Transformationsaktivitäten, die [Pipelines](data-factory-create-pipelines.md) entweder einzeln oder mit einer anderen Aktivität verkettet hinzugefügt werden können.

Datentransformationsaktivität | Compute-Umgebung 
:----------------------- | :--------------------
[Hive](data-factory-hive-activity.md) | HDInsight [Hadoop]
[Pig](data-factory-pig-activity.md) | HDInsight [Hadoop]
[MapReduce](data-factory-map-reduce.md) | HDInsight [Hadoop]
[Hadoop Streaming](data-factory-hadoop-streaming-activity.md) | HDInsight [Hadoop]
[Machine Learning-Aktivitäten: Batchausführung und Ressourcenaktualisierung](data-factory-azure-ml-batch-execution-activity.md) | Azure-VM 
[Gespeicherte Prozedur](data-factory-stored-proc-activity.md) | Azure SQL, Azure SQL Data Warehouse oder SQL Server |
[Data Lake Analytics U-SQL](data-factory-usql-activity.md) | Azure Data Lake Analytics 
[DotNet](data-factory-use-custom-activities.md) | HDInsight [Hadoop] oder Azure Batch
   
> [AZURE.NOTE] 
Sie können die MapReduce-Aktivität verwenden, um Spark-Programme in Ihrem HDInsight Spark-Cluster auszuführen. Weitere Informationen finden Sie unter [Aufrufen von Spark-Programmen aus Data Factory](data-factory-spark.md). Sie können eine benutzerdefinierte Aktivität erstellen, um R-Skripts in Ihrem HDInsight-Cluster mit installiertem R auszuführen. Informationen hierzu finden Sie unter [RunRScriptUsingADFSample](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample).
 

Sie erstellen einen verknüpften Dienst für die Computeumgebung und verwenden dann den verknüpften Dienst, wenn Sie eine Transformationsaktivität definieren. Es gibt zwei Typen von Compute-Umgebungen, die von Data Factory unterstützt werden.

1. **Bei Bedarf**: In diesem Fall wird die Compute-Umgebung vollständig von Data Factory verwaltet. Der Data Factory-Dienst erstellt diese Umgebung automatisch, bevor ein Auftrag zur Verarbeitung von Daten übermittelt wird. Sobald der Auftrag abgeschlossen wurde, wird die Umgebung entfernt. Sie können differenzierte Einstellungen für die bedarfsgesteuerte Computeumgebung zur Auftragsausführung, Clusterverwaltung sowie für Bootstrappingaktionen konfigurieren und steuern.
2. **Eigene verwenden**: In diesem Fall können Sie Ihre eigene Compute-Umgebung (z. B. HDInsight-Cluster) als verknüpften Dienst in Data Factory registrieren. Die Compute-Umgebung wird von Ihnen verwaltet und von Data Factory zum Ausführen von Aktivitäten verwendet. 

Unter dem Artikel [Verknüpfte Computedienste](data-factory-compute-linked-services.md) finden Sie Informationen zu Computediensten, die von Data Factory unterstützt werden.

<!---HONumber=AcomDC_0914_2016-->