---
title: "Azure Storage-Optionen für R Server in HDInsight Premium | Microsoft Docs"
description: "Lernen Sie die verschiedenen Speicheroptionen kennen, die Benutzern mit R Server in HDInsight zur Verfügung stehen."
services: HDInsight
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 1cf30096-d3ca-45ea-b526-aa3954402f66
ms.service: HDInsight
ms.devlang: R
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 01/09/2017
ms.author: jeffstok
translationtype: Human Translation
ms.sourcegitcommit: 2df17cddf629cb72b7fa4d590dfaa69311c96aa4
ms.openlocfilehash: 3e47a7e0382009a07b885a28c6e8d90f9bff9cfb


---
# <a name="azure-storage-options-for-r-server-on-hdinsight"></a>Azure Storage-Optionen für R Server in HDInsight
Microsoft R Server in HDInsight hat Zugriff auf Azure Blob [Azure Data Lake Storage](https://azure.microsoft.com/services/data-lake-store/), um Daten, Code, Ergebnisobjekte von Analysen usw. dauerhaft zu speichern.

Bei der Erstellung eines Hadoop-Clusters in HDInsight geben Sie ein Azure Storage-Konto oder einen Data Lake Store an. Ein spezieller Speichercontainer dieses Kontos enthält das Dateisystem für den von Ihnen erstellten Cluster (z.B. das Hadoop Distributed File System). Aus Leistungsgründen wird der HDInsight-Cluster in demselben Rechenzentrum wie das von Ihnen angegebene primäre Speicherkonto erstellt. Weitere Informationen finden Sie unter [Verwenden von Azure Blob-Speicher mit HDInsight](hdinsight-hadoop-use-blob-storage.md "Use Azure Blob storage with HDInsight").   

## <a name="use-multiple-azure-blob-storage-accounts"></a>Verwenden mehrerer Azure-Blobspeicherkonten
Bei Bedarf können Sie mit Ihrem HDI-Cluster auf mehrere Azure-Speicherkonten zugreifen. Hierfür müssen Sie die zusätzlichen Speicherkonten in der Benutzeroberfläche angeben, wenn Sie den Cluster erstellen, und diese Schritte ausführen, um sie in R zu verwenden.  

1. Erstellen Sie einen HDInsight-Cluster mit einem Speicherkonto mit dem Namen **storage1** und dem Standardcontainer **container1**.
2. Geben Sie zusätzlich ein Speicherkonto mit dem Namen **storage2**an.  
3. Kopieren Sie die Datei „mycsv.csv“ in das Verzeichnis „/share“, und führen Sie eine Analyse für diese Datei durch.  

        hadoop fs –mkdir /share
        hadoop fs –copyFromLocal myscsv.scv /share  

4. Legen Sie im R-Code den Namensknoten auf **default** fest, und geben Sie das Verzeichnis und die Datei für die Verarbeitung an.  

        myNameNode <- "default"
        myPort <- 0

    Speicherort der Daten:  

        bigDataDirRoot <- "/share"  

    Spark-Rechenkontext definieren:

        mySparkCluster <- RxSpark(consoleOutput=TRUE)

    Rechenkontext festlegen:

        rxSetComputeContext(mySparkCluster)

    HDFS-Dateisystem (Hadoop Distributed File System) definieren:

        hdfsFS <- RxHdfsFileSystem(hostName=myNameNode, port=myPort)

    Eingabedatei für Analyse in HDFS angeben:

        inputFile <-file.path(bigDataDirRoot,"mycsv.csv")

Alle Verzeichnis- und Dateiverweise zeigen auf das Speicherkonto wasbs://container1@storage1.blob.core.windows.net. Dies ist das **Standardspeicherkonto**, das dem HDInsight-Cluster zugeordnet ist.

Angenommen, Sie möchten eine Datei mit dem Namen „mySpecial.csv“ verarbeiten, die im Verzeichnis „/private“ von **container2** in **storage2** enthalten ist.

Legen Sie den Namensknotenverweis im R-Code auf das Speicherkonto **storage2** fest.

````
myNameNode <- "wasbs://container2@storage2.blob.core.windows.net"
myPort <- 0
````

Speicherort der Daten:

````
bigDataDirRoot <- "/private"
````

Spark-Rechenkontext definieren:

````
mySparkCluster <- RxSpark(consoleOutput=TRUE, nameNode=myNameNode, port=myPort)
````

Rechenkontext festlegen:

````
rxSetComputeContext(mySparkCluster)
````

HDFS-Dateisystem definieren:

````
hdfsFS <- RxHdfsFileSystem(hostName=myNameNode, port=myPort)
````

Eingabedatei für Analyse in HDFS angeben:

````
inputFile <-file.path(bigDataDirRoot,"mySpecial.csv")
````

Alle Verzeichnis- und Dateiverweise zeigen jetzt auf das Speicherkonto wasbs://container2@storage2.blob.core.windows.net. Dies ist der **Namensknoten**, den Sie angegeben haben.

Beachten Sie, dass Sie das Verzeichnis „/user/RevoShare/<SSH username>“ unter **storage2** wie folgt konfigurieren müssen:

````
hadoop fs -mkdir wasbs://container2@storage2.blob.core.windows.net/user
hadoop fs -mkdir wasbs://container2@storage2.blob.core.windows.net/user/RevoShare
hadoop fs -mkdir wasbs://container2@storage2.blob.core.windows.net/user/RevoShare/<RDP username>
````

## <a name="use-an-azure-data-lake-store"></a>Verwenden eines Azure Data Lake-Speichers
Zum Verwenden von Data Lake-Speichern mit Ihrem HDInsight-Konto müssen Sie Ihrem Cluster Zugriff auf die einzelnen Azure Data Lake-Speicher gewähren, die Sie verwenden möchten. Sie verwenden den Speicher im R-Skript so ähnlich wie ein sekundäres Speicherkonto (wie im vorherigen Verfahren beschrieben).

## <a name="add-cluster-access-to-your-azure-data-lake-stores"></a>Hinzufügen von Clusterzugriff auf Ihre Azure Data Lake-Speicher
Sie greifen auf einen Data Lake-Speicher zu, indem Sie einen Azure AD-Dienstprinzipal (Azure Active Directory) verwenden, der Ihrem HDInsight-Cluster zugeordnet ist.

### <a name="to-add-a-service-principal"></a>So fügen Sie einen Dienstprinzipal hinzu
1.Wählen Sie beim Erstellen des HDInsight-Clusters auf der Registerkarte **Datenquelle** eine **Azure Active Directory-Identität für den Cluster** aus.

2.Wählen Sie im Dialogfeld **Azure Active Directory-Identität für den Cluster** unter **AD-Dienstprinzipal auswählen** die Option **Neu erstellen**.

Nachdem Sie dem Dienstprinzipal einen Namen gegeben und ein Kennwort dafür erstellt haben, klicken Sie auf **ADLS-Zugriff verwalten**, um den Dienstprinzipal Ihren Data Lake Stores zuzuordnen.

Sie können auch nach Erstellung des Clusters einem oder weiteren Data Lake Stores Clusterzugriff hinzufügen, indem Sie den Azure-Portal-Eintrag für einen Data Lake Store öffnen und **Daten-Explorer > Zugriff > Hinzufügen** aufrufen. 

Weitere Einzelheiten zum Hinzufügen von HDI-Clusterzugriff zu Data Lake Stores finden Sie im Artikel [Erstellen eines HDInsight-Clusters mit Data Lake-Speicher mithilfe des Azure-Portals](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-hdinsight-hadoop-use-portal#create-an-hdinsight-cluster-with-access-to-azure-data-lake-store)

## <a name="use-the-data-lake-store-with-r-server"></a>Verwenden des Data Lake-Speichers mit R Server
Nachdem Sie einem Data Lake-Speicher Zugriff gewährt haben, können Sie den Speicher in R Server unter HDInsight wie ein sekundäres Azure-Speicherkonto nutzen. Der einzige Unterschied ist, dass sich das Präfix **wasb://** wie folgt in **adl://** ändert:

````
# Point to the ADL store (e.g. ADLtest)
myNameNode <- "adl://rkadl1.azuredatalakestore.net"
myPort <- 0

# Location of the data (assumes a /share directory on the ADL account)
bigDataDirRoot <- "/share"  

# Define Spark compute context
mySparkCluster <- RxSpark(consoleOutput=TRUE, nameNode=myNameNode, port=myPort)

# Set compute context
rxSetComputeContext(mySparkCluster)

# Define HDFS file system
hdfsFS <- RxHdfsFileSystem(hostName=myNameNode, port=myPort)

# Specify the input file in HDFS to analyze
inputFile <-file.path(bigDataDirRoot,"AirlineDemoSmall.csv")

# Create factors for days of the week
colInfo <- list(DayOfWeek = list(type = "factor",
               levels = c("Monday", "Tuesday", "Wednesday", "Thursday",
                          "Friday", "Saturday", "Sunday")))

# Define the data source
airDS <- RxTextData(file = inputFile, missingValueString = "M",
                    colInfo  = colInfo, fileSystem = hdfsFS)

# Run a linear regression
model <- rxLinMod(ArrDelay~CRSDepTime+DayOfWeek, data = airDS)
````

Unten sind die Befehle zum Konfigurieren des Data Lake-Speicherkontos mit dem RevoShare-Verzeichnis und Hinzufügen der CSV-Beispieldatei aus dem vorherigen Beispiel angegeben:

````
hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user
hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user/RevoShare
hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user/RevoShare/<user>

hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/share

hadoop fs -copyFromLocal /usr/lib64/R Server-7.4.1/library/RevoScaleR/SampleData/AirlineDemoSmall.csv adl://rkadl1.azuredatalakestore.net/share

hadoop fs –ls adl://rkadl1.azuredatalakestore.net/share
````

## <a name="use-azure-files-on-the-edge-node"></a>Verwenden von Azure Files auf dem Edgeknoten
Es ist auch eine komfortable Datenspeicheroption zur Verwendung auf dem Edgeknoten vorhanden, die die Bezeichnung [Azure Files](../storage/storage-how-to-use-files-linux.md "Azure Files"). Hiermit können Sie eine Azure Storage-Dateifreigabe auf einem Linux-Dateisystem bereitstellen. Dies kann zum Speichern von Datendateien, R-Skripts und Ergebnisobjekten nützlich sein, die unter Umständen später benötigt werden, wenn es sinnvoll ist, anstelle von HDFS das native Dateisystem auf dem Edgeknoten zu verwenden.

Ein großer Vorteil von Azure Files besteht darin, dass die Dateifreigaben von allen Systemen mit einem unterstützten Betriebssystem, z.B. Windows oder Linux, bereitgestellt und verwendet werden können. Beispielsweise ist die Verwendung durch einen anderen HDInsight-Cluster, der Ihnen oder einem Mitglied Ihres Teams gehört, eine Azure VM oder sogar ein lokales System möglich.

## <a name="next-steps"></a>Nächste Schritte
Da Sie nun die Azure-Speicheroptionen kennen, verwenden Sie die folgenden Links, um weitere Methoden zum Arbeiten mit R Server unter HDInsight zu erfahren.

* [Übersicht über R Server in HDInsight](hdinsight-hadoop-r-server-overview.md)
* [Erste Schritte mit R Server in Hadoop](hdinsight-hadoop-r-server-get-started.md)
* [Hinzufügen von RStudio Server zu HDInsight (falls nicht bereits während der Clustererstellung hinzugefügt)](hdinsight-hadoop-r-server-install-r-studio.md)
* [Rechenkontextoptionen für R Server in HDInsight](hdinsight-hadoop-r-server-compute-contexts.md)




<!--HONumber=Feb17_HO3-->


