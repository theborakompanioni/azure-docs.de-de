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
   ms.date="06/01/2016"
   ms.author="jeffstok"
/>

# Azure Storage-Optionen für R Server in HDInsight (Vorschau)

R Server in HDInsight (Vorschau) hat Zugriff auf Azure-Blobspeicher und [Azure Data Lake-Speicher](https://azure.microsoft.com/services/data-lake-store/), um Daten, Code, Ergebnisobjekte von Analysen usw. dauerhaft zu speichern.

Bei der Erstellung eines Hadoop-Clusters in HDInsight geben Sie ein Azure-Speicherkonto an. Ein spezieller Blobspeichercontainer dieses Kontos wird angegeben, um das Dateisystem für den von Ihnen erstellten Cluster aufzunehmen, also das Hadoop Distributed File System (HDFS). Aus Leistungsgründen wird der HDInsight-Cluster in demselben Rechenzentrum wie das von Ihnen angegebene primäre Speicherkonto erstellt. Weitere Informationen finden Sie unter [Verwenden von Azure-Blobspeicher mit HDInsight](hdinsight-hadoop-use-blob-storage.md "Verwenden von Azure Blob-Speicher mit HDInsight").


## Verwenden mehrerer Azure Blob Storage-Konten

Bei Bedarf ist es möglich, mit Ihrem HDI-Cluster auf mehrere Azure-Speicherkonten zuzugreifen. Hierfür müssen Sie die zusätzlichen Speicherkonten in der Benutzeroberfläche bei der Clustererstellung angeben und diese Schritte ausführen, um sie in R zu verwenden.

1.	Angenommen, Sie erstellen einen HDInsight-Cluster mit einem Speicherkonto mit dem Namen „storage1“ und dem Standardcontainer „container1“. Sie können auch ein zusätzliches Speicherkonto „storage2“ angeben.  
2.	Anschließend kopieren Sie die Datei „mycsv.csv“ in das Verzeichnis „/share“, um eine Analyse für die Datei durchzuführen.  

````
hadoop fs –mkdir /share
hadoop fs –copyFromLocal myscsv.scv /share  
````

3.	Im R-Code legen Sie den Namensknoten auf „default“ fest und geben das Verzeichnis und die Datei für die Verarbeitung an.  

````
myNameNode <- "default"
myPort <- 0
````

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

## Verwenden eines Azure Data Lake-Speichers

Um Azure Data Lake-Speicher mit Ihrem HDInsight-Konto zu verwenden, müssen Sie Ihrem Cluster Zugriff auf jeden Azure Data Lake-Speicher gewähren, den Sie verwenden möchten, und dann in Ihrem R-Skript in einer Weise auf den Speicher verweisen, die der oben beschriebenen Verwendung eines sekundären Azure-Speicherkontos ähnelt.

## Hinzufügen von Clusterzugriff auf Ihre Azure Data Lake-Speicher

Der Zugriff auf einen Azure Data Lake-Speicher wird durch die Verwendung eines Azure Active Directory-Dienstprinzipals (AAD) eingerichtet, der Ihrem HDInsight-Cluster zugeordnet ist. Um beim Erstellen Ihres HDInsight-Clusters einen Dienstprinzipal hinzuzufügen, klicken Sie auf der Registerkarte „Datenquelle“ auf die Option „Azure Active Directory-Identität für den Cluster“ und dann auf „Neu erstellen“ für den Dienstprinzipal. Nachdem Sie einen Namen und ein Kennwort zugewiesen haben, wird eine neue Registerkarte geöffnet, auf der Sie Ihre Azure Data Lake-Speicher dem Dienstprinzipal zuordnen können.

Beachten Sie, dass Sie Zugriff auf einen Azure Data Lake-Speicher auch später hinzufügen können, indem Sie den Azure Data Lake-Speicher im Azure-Portal öffnen und zu „Daten-Explorer > Zugriff“ wechseln. Dieses Beispieldialogfeld veranschaulicht das Erstellen eines Dienstprinzipals und dessen Zuordnung zum Azure Data Lake-Speicher mit „rkadl11“.

![Erstellen von ADL-Speicherdienstprinzip 1](./media/hdinsight-hadoop-r-server-storage/hdinsight-hadoop-r-server-storage-adls-sp1.png)


![Erstellen von ADL-Speicherdienstprinzip 2](./media/hdinsight-hadoop-r-server-storage/hdinsight-hadoop-r-server-storage-adls-sp2.png)

## Verwenden von Azure Data Lake-Speicher mit R Server
Sobald Sie Zugriff auf einen Azure Data Lake-Speicher mit Verwendung des Dienstprinzipals des Clusters erteilt haben, können Sie ihn in R Server unter HDInsight in gleicher Weise wie ein sekundäres Azure-Speicherkonto verwenden. Der einzige Unterschied ist, dass das „wasb://“-Präfix nun zu „adl://“ wird, z.B.

````
# point to the ADL store (e.g. ADLtest) 
myNameNode <- "adl://rkadl1.azuredatalakestore.net"
myPort <- 0

# Location of the data (assumes a /share directory on the ADL account) 
bigDataDirRoot <- "/share"  

# define Spark compute context
mySparkCluster <- RxSpark(consoleOutput=TRUE)

# set compute context
rxSetComputeContext(mySparkCluster)

# define HDFS file system
hdfsFS <- RxHdfsFileSystem(hostName=myNameNode, port=myPort)

# specify the input file in HDFS to analyze
inputFile <-file.path(bigDataDirRoot,"AirlineDemoSmall.csv")

# create Factors for days of the week
colInfo <- list(DayOfWeek = list(type = "factor",
               levels = c("Monday", "Tuesday", "Wednesday", "Thursday",
                          "Friday", "Saturday", "Sunday")))

# define the data source 
airDS <- RxTextData(file = inputFile, missingValueString = "M",
                    colInfo  = colInfo, fileSystem = hdfsFS)

# Run a linear regression
model <- rxLinMod(ArrDelay~CRSDepTime+DayOfWeek, data = airDS)
````

> [AZURE.NOTE] Hier sind die Befehle zum Konfigurieren des Azure Data Lake-Speicherkontos mit dem RevoShare-Verzeichnis und Hinzufügen der Beispiel-CSV-Datei für das obige Beispiel:

````
hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user 
hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user/RevoShare 
hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user/RevoShare/<user>

hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/share

hadoop fs -copyFromLocal /usr/lib64/R Server-7.4.1/library/RevoScaleR/SampleData/AirlineDemoSmall.csv adl://rkadl1.azuredatalakestore.net/share

hadoop fs –ls adl://rkadl1.azuredatalakestore.net/share
````

## Verwenden von Azure-Dateien auf dem Edgeknoten 

Es ist auch eine benutzerfreundliche Datenspeicheroption zur Verwendung auf dem Edgeknoten verfügbar, die die Bezeichnung [Azure Files](../storage/storage-how-to-use-files-linux.md "Azure Files") hat. Hiermit können Sie eine Azure Storage-Dateifreigabe auf dem Linux-Dateisystem bereitstellen. Dies kann zum Speichern von Datendateien, R-Skripts und Ergebnisobjekten nützlich sein, die unter Umständen später benötigt werden, wenn es sinnvoll ist, anstelle von HDFS das native Dateisystem auf dem Edgeknoten zu verwenden. Ein großer Vorteil der Verwendung von Azure Files ist, dass die Dateifreigaben von jedem System mit einem unterstützten Betriebssystem (Win, Linux) verwendet werden können. Dies kann beispielsweise ein anderer HDInsight-Cluster sein, über das Sie oder ein Mitglied Ihres Teams verfügt, eine Azure-VM oder auch ein lokales System.


## Nächste Schritte

Nachdem Sie jetzt wissen, wie Sie einen neuen HDInsight-Cluster mit Verwendung von R Server erstellen, und Sie die Grundlagen der Verwendung der R-Konsole aus einer SSH-Sitzung kennengelernt haben, können Sie die folgenden Links nutzen, um weitere Möglichkeiten zur Arbeit mit R Server in HDInsight kennenzulernen.

- [Übersicht über R Server in Hadoop](hdinsight-hadoop-r-server-overview.md)
- [Erste Schritte mit R Server in Hadoop](hdinsight-hadoop-r-server-get-started.md)
- [Hinzufügen von RStudio Server zu HDInsight Premium](hdinsight-hadoop-r-server-install-r-studio.md)
- [Rechenkontextoptionen für R Server in HDInsight Premium](hdinsight-hadoop-r-server-compute-contexts.md)

<!---HONumber=AcomDC_0608_2016-->