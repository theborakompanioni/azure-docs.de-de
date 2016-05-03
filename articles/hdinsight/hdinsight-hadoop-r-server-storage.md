<properties
   pageTitle="Azure Storage-Optionen für R Server in HDInsight (Vorschau) | Azure"
   description="Lernen Sie die verschiedenen Speicheroptionen kennen, die Benutzern mit R Server in HDInsight (Vorschau) zur Verfügung stehen."
   services="HDInsight"
   documentationCenter=""
   authors="jeffstokes72"
   manager="paulettem"
   editor="cgronlun"
/>

<tags
   ms.service="HDInsight"
   ms.devlang="R"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-services"
   ms.date="03/28/2016"
   ms.author="jeffstok"
/>

# Azure Storage-Optionen für R Server in HDInsight (Vorschau)

R Server in HDInsight (Vorschau) hat Zugriff auf Azure-Blobspeicher und bald auch auf Azure Data Lake-Speicher, um Daten, Code, Ergebnisobjekte von Analysen usw. dauerhaft zu speichern.

Bei der Erstellung eines Hadoop-Clusters in HDInsight geben Sie ein Azure-Speicherkonto an. Ein spezieller Blobspeichercontainer dieses Kontos wird angegeben, um das Dateisystem für den von Ihnen erstellten Cluster aufzunehmen, also das Hadoop Distributed File System (HDFS). Aus Leistungsgründen wird der HDInsight-Cluster in demselben Rechenzentrum wie das von Ihnen angegebene primäre Speicherkonto erstellt. Weitere Informationen finden Sie unter [Verwenden von Azure-Blobspeicher mit HDInsight](hdinsight-hadoop-use-blob-storage.md "Verwenden von Azure Blob-Speicher mit HDInsight").


## Verwenden mehrerer Azure Blob Storage-Konten

Bei Bedarf ist es möglich, mit Ihrem HDI-Cluster auf mehrere Azure-Speicherkonten zuzugreifen. Hierfür müssen Sie die zusätzlichen Speicherkonten in der Benutzeroberfläche bei der Clustererstellung angeben und diese Schritte ausführen, um sie in R zu verwenden.

1.	Angenommen, Sie erstellen einen HDInsight-Cluster mit einem Speicherkonto mit dem Namen „storage1“ und dem Standardcontainer „container1“. Sie können auch ein zusätzliches Speicherkonto „storage2“ angeben.  
2.	Anschließend kopieren Sie die Datei „mycsv.csv“ in das Verzeichnis „/share“, um eine Analyse für die Datei durchzuführen.  

    hadoop fs –mkdir /share hadoop fs –copyFromLocal myscsv.scv /share

3.	Im R-Code legen Sie den Namensknoten auf „default“ fest und geben das Verzeichnis und die Datei für die Verarbeitung an.

    myNameNode <- "default" myPort <- 0

  Speicherort der Daten

    bigDataDirRoot <- "/share"  

  Spark-Rechenkontext definieren

    mySparkCluster <- RxSpark(consoleOutput=TRUE)

  Rechenkontext festlegen

    rxSetComputeContext(mySparkCluster)

  HDFS-Dateisystem definieren

    hdfsFS <- RxHdfsFileSystem(hostName=myNameNode, port=myPort)

  Eingabedatei für HDFS für Analyse angeben

    inputFile <-file.path(bigDataDirRoot,"mycsv.csv")
 
Alle Verzeichnis- und Dateiverweise zeigen auf das Speicherkonto wasb://container1@storage1.blob.core.windows.net, da dies das **Standardspeicherkonto** ist, das dem HDInsight-Cluster zugeordnet ist.

Angenommen, Sie möchten eine Datei mit dem Namen „mySpecial.csv“ verarbeiten, die im Verzeichnis „/private“ des Containers „container2“ unter dem Speicherkontonamen „storage2“ gespeichert ist.

Im R-Code können Sie den Namensknotenverweis in das Speicherkonto „storage2“ ändern.

    myNameNode <- "wasb://container2@storage2.blob.core.windows.net"
    myPort <- 0

  Speicherort der Daten

    bigDataDirRoot <- "/private"

  Spark-Rechenkontext definieren

    mySparkCluster <- RxSpark(consoleOutput=TRUE)

  Rechenkontext festlegen

    rxSetComputeContext(mySparkCluster)

  HDFS-Dateisystem definieren

    hdfsFS <- RxHdfsFileSystem(hostName=myNameNode, port=myPort)

  Eingabedatei für HDFS für Analyse angeben

    inputFile <-file.path(bigDataDirRoot,"mySpecial.csv")
 
Alle Verzeichnis- und Dateiverweise zeigen jetzt auf das Speicherkonto wasb://container2@storage2.blob.core.windows.net, da dies der von Ihnen angegebene **Namensknoten** ist.

Beachten Sie, dass Sie das Verzeichnis „/user/RevoShare/<SSH username>“ unter dem Speicherkonto „storage2“ konfigurieren müssen:

    hadoop fs -mkdir wasb://container2@storage2.blob.core.windows.net/user
    hadoop fs -mkdir wasb://container2@storage2.blob.core.windows.net/user/RevoShare
    hadoop fs -mkdir wasb://container2@storage2.blob.core.windows.net/user/RevoShare/<RDP username>


## Verwenden von Azure-Dateien auf dem Edgeknoten 

Es ist auch eine benutzerfreundliche Datenspeicheroption zur Verwendung auf dem Edgeknoten verfügbar, die die Bezeichnung [Azure Files](../storage/storage-how-to-use-files-linux.md "Azure Files") hat. Hiermit können Sie eine Azure Storage-Dateifreigabe auf dem Linux-Dateisystem bereitstellen. Dies kann zum Speichern von Datendateien, R-Skripts und Ergebnisobjekten nützlich sein, die unter Umständen später benötigt werden, wenn es sinnvoll ist, anstelle von HDFS das native Dateisystem auf dem Edgeknoten zu verwenden. Ein großer Vorteil der Verwendung von Azure Files ist, dass die Dateifreigaben von jedem System mit einem unterstützten Betriebssystem (Win, Linux) verwendet werden können. Dies kann beispielsweise ein anderer HDInsight-Cluster sein, über das Sie oder ein Mitglied Ihres Teams verfügt, eine Azure-VM oder auch ein lokales System.


## Nächste Schritte

Nachdem Sie jetzt wissen, wie Sie einen neuen HDInsight-Cluster mit Verwendung von R Server erstellen, und Sie die Grundlagen der Verwendung der R-Konsole aus einer SSH-Sitzung kennengelernt haben, können Sie die folgenden Links nutzen, um weitere Möglichkeiten zur Arbeit mit R Server in HDInsight kennenzulernen.

- [Übersicht über R Server in Hadoop](hdinsight-hadoop-r-server-overview.md)
- [Erste Schritte mit R Server in Hadoop](hdinsight-hadoop-r-server-get-started.md)
- [Hinzufügen von RStudio Server zu HDInsight Premium](hdinsight-hadoop-r-server-install-r-studio.md)
- [Rechenkontextoptionen für R Server in HDInsight Premium](hdinsight-hadoop-r-server-compute-contexts.md)

<!---HONumber=AcomDC_0420_2016-->