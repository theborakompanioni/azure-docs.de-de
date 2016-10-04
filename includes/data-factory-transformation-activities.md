Azure Data Factory unterstützt die folgenden Transformationsaktivitäten, die Pipelines entweder einzeln oder mit einer anderen Aktivität verkettet hinzugefügt werden können.

Datentransformationsaktivität | Compute-Umgebung 
:----------------------- | :--------------------
[Hive](../articles/data-factory/data-factory-hive-activity.md) | HDInsight [Hadoop] [Pig](../articles/data-factory/data-factory-pig-activity.md) | HDInsight [Hadoop] [MapReduce](../articles/data-factory/data-factory-map-reduce.md) | HDInsight [Hadoop] [Hadoop Streaming](../articles/data-factory/data-factory-hadoop-streaming-activity.md) | HDInsight [Hadoop] [Machine Learning-Aktivitäten: Batchausführung und Ressourcenaktualisierung](../articles/data-factory/data-factory-azure-ml-batch-execution-activity.md) | Azure-VM 
[Gespeicherte Prozedur](../articles/data-factory/data-factory-stored-proc-activity.md) | Azure SQL, Azure SQL Data Warehouse oder SQL Server |
[Data Lake Analytics U-SQL](../articles/data-factory/data-factory-usql-activity.md) | Azure Data Lake Analytics 
[DotNet](../articles/data-factory/data-factory-use-custom-activities.md) | HDInsight [Hadoop] oder Azure Batch
   
> [AZURE.NOTE] 
Sie können die MapReduce-Aktivität verwenden, um Spark-Programme in Ihrem HDInsight Spark-Cluster auszuführen. Weitere Informationen finden Sie unter [Aufrufen von Spark-Programmen aus Data Factory](../articles/data-factory/data-factory-spark.md). Sie können eine benutzerdefinierte Aktivität erstellen, um R-Skripts in Ihrem HDInsight-Cluster mit installiertem R auszuführen. Informationen hierzu finden Sie unter [RunRScriptUsingADFSample](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample).

<!---HONumber=AcomDC_0928_2016-->