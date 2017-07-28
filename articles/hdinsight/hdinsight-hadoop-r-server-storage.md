---
title: "Azure Storage-Lösungen für R Server in HDInsight – Azure | Microsoft-Dokumentation"
description: "Lernen Sie die verschiedenen Speicheroptionen kennen, die Benutzern mit R Server in HDInsight zur Verfügung stehen."
services: HDInsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 1cf30096-d3ca-45ea-b526-aa3954402f66
ms.service: HDInsight
ms.custom: hdinsightactive
ms.devlang: R
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 06/19/2017
ms.author: bradsev
ms.translationtype: Human Translation
ms.sourcegitcommit: 7948c99b7b60d77a927743c7869d74147634ddbf
ms.openlocfilehash: 017a66ae1ade5e0f64fc799b7bb6aa97b67791a8
ms.contentlocale: de-de
ms.lasthandoff: 06/20/2017


---
# <a name="azure-storage-solutions-for-r-server-on-hdinsight"></a>Azure Storage-Lösungen für R Server in HDInsight

Microsoft R Server in HDInsight bietet eine Vielzahl an Speicherlösungen zum dauerhaften Speichern von Daten, Codes oder Objekten, die Ergebnisse aus der Analyse enthalten. Dazu zählen die folgenden Optionen:

- [Azure-Blob](https://azure.microsoft.com/services/storage/blobs/)
- [Azure Data Lake-Speicher](https://azure.microsoft.com/services/data-lake-store/)
- [Azure File Storage](https://azure.microsoft.com/services/storage/files/)

Sie haben auch die Möglichkeit, mit Ihrem HDI-Cluster auf mehrere Azure-Speicherkonten oder -Container zuzugreifen. Azure File Storage ist eine komfortable Datenspeicheroption zur Verwendung auf dem Edgeknoten, mit der Sie eine Azure Storage-Dateifreigabe z.B. auf einem Linux-Dateisystem bereitstellen können. Die Dateifreigaben können mit Azure File jedoch von allen Systemen mit einem unterstützten Betriebssystem, z.B. Windows oder Linux, bereitgestellt und verwendet werden. 

Bei der Erstellung eines Hadoop-Clusters in HDInsight geben Sie entweder ein **Azure Storage**-Konto oder einen **Data Lake-Speicher** an. Ein spezieller Speichercontainer dieses Kontos enthält das Dateisystem für den von Ihnen erstellten Cluster (z.B. das Hadoop Distributed File System). Weitere Informationen und Anleitungen finden Sie unter:

- [Verwenden von Azure Storage mit HDInsight](hdinsight-hadoop-use-blob-storage.md)
- [Verwenden von Data Lake Store mit Azure HDInsight-Clustern](hdinsight-hadoop-use-data-lake-store.md) 

Weitere Informationen zu den Azure Storage-Lösungen finden Sie unter [Einführung in Microsoft Azure Storage](../storage/storage-introduction.md). 

Eine Anleitung zur Auswahl der am besten geeigneten Speicheroption für Ihr Szenario finden Sie unter [Entscheidung zwischen Azure-Blobs, Azure Files und Azure-Datenträger](../storage/storage-decide-blobs-files-disks.md). 


## <a name="use-azure-blob-storage-accounts-with-r-server"></a>Verwenden von Azure-Blobspeicherkonten mit R Server

Bei Bedarf können Sie mit Ihrem HDI-Cluster auf mehrere Azure-Speicherkonten zugreifen. Hierfür müssen Sie die zusätzlichen Speicherkonten in der Benutzeroberfläche angeben, wenn Sie den Cluster erstellen, und diese Schritte ausführen, um sie mit R Server zu verwenden.

> [!WARNING]
> Aus Leistungsgründen wird der HDInsight-Cluster in demselben Rechenzentrum wie das von Ihnen angegebene primäre Speicherkonto erstellt. Die Verwendung eines Speicherkontos an einem anderen Ort als dem HDInsight-Cluster wird nicht unterstützt.

1. Erstellen Sie einen HDInsight-Cluster mit einem Speicherkonto mit dem Namen **storage1** und dem Standardcontainer **container1**.
2. Geben Sie zusätzlich ein Speicherkonto mit dem Namen **storage2**an.  
3. Kopieren Sie die Datei „mycsv.csv“ in das Verzeichnis „/share“, und führen Sie eine Analyse für diese Datei durch.  

        hadoop fs –mkdir /share
        hadoop fs –copyFromLocal myscsv.scv /share  

4. Legen Sie im R-Code den Namensknoten auf **default** fest, und geben Sie das Verzeichnis und die Datei für die Verarbeitung an.  

        myNameNode <- "default"
        myPort <- 0

        #Location of the data:  
        bigDataDirRoot <- "/share"  

        #Define Spark compute context:
        mySparkCluster <- RxSpark(consoleOutput=TRUE)

        #Set compute context:
        rxSetComputeContext(mySparkCluster)

        #Define the Hadoop Distributed File System (HDFS) file system:
        hdfsFS <- RxHdfsFileSystem(hostName=myNameNode, port=myPort)

        #Specify the input file to analyze in HDFS:
        inputFile <-file.path(bigDataDirRoot,"mycsv.csv")

Alle Verzeichnis- und Dateiverweise zeigen auf das Speicherkonto wasbs://container1@storage1.blob.core.windows.net. Dies ist das **Standardspeicherkonto**, das dem HDInsight-Cluster zugeordnet ist.

Angenommen, Sie möchten eine Datei mit dem Namen „mySpecial.csv“ verarbeiten, die im Verzeichnis „/private“ von **container2** in **storage2** enthalten ist.

Legen Sie den Namensknotenverweis im R-Code auf das Speicherkonto **storage2** fest.


    myNameNode <- "wasbs://container2@storage2.blob.core.windows.net"
    myPort <- 0

    #Location of the data:
    bigDataDirRoot <- "/private"

    #Define Spark compute context:
    mySparkCluster <- RxSpark(consoleOutput=TRUE, nameNode=myNameNode, port=myPort)

    #Set compute context:
    rxSetComputeContext(mySparkCluster)

    #Define HDFS file system:
    hdfsFS <- RxHdfsFileSystem(hostName=myNameNode, port=myPort)

    #Specify the input file to analyze in HDFS:
    inputFile <-file.path(bigDataDirRoot,"mySpecial.csv")

Alle Verzeichnis- und Dateiverweise zeigen jetzt auf das Speicherkonto wasbs://container2@storage2.blob.core.windows.net. Dies ist der **Namensknoten**, den Sie angegeben haben.

Sie müssen das Verzeichnis „/user/RevoShare/<SSH username>“ unter **storage2** wie folgt konfigurieren:


    hadoop fs -mkdir wasbs://container2@storage2.blob.core.windows.net/user
    hadoop fs -mkdir wasbs://container2@storage2.blob.core.windows.net/user/RevoShare
    hadoop fs -mkdir wasbs://container2@storage2.blob.core.windows.net/user/RevoShare/<RDP username>



## <a name="use-an-azure-data-lake-store-with-r-server"></a>Verwenden eines Azure Data Lake-Speichers mit R Server

Zum Verwenden von Data Lake-Speichern mit Ihrem HDInsight-Konto müssen Sie Ihrem Cluster Zugriff auf die einzelnen Azure Data Lake-Speicher gewähren, die Sie verwenden möchten. Anleitungen zur Verwendung des Azure-Portals zum Erstellen eines HDInsight-Clusters mit einem Azure Data Lake-Speicherkonto als Standardspeicher oder als zusätzlichen Speicher finden Sie unter [Erstellen eines HDInsight-Clusters mit Data Lake Store mithilfe des Azure-Portals](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).

Nun verwenden Sie den Speicher im R-Skript so ähnlich wie ein sekundäres Azure-Speicherkonto (wie im vorherigen Verfahren beschrieben).

### <a name="add-cluster-access-to-your-azure-data-lake-stores"></a>Hinzufügen von Clusterzugriff auf Ihre Azure Data Lake-Speicher
Sie greifen auf einen Data Lake-Speicher zu, indem Sie einen Azure AD-Dienstprinzipal (Azure Active Directory) verwenden, der Ihrem HDInsight-Cluster zugeordnet ist.

So fügen Sie einen Azure AD-Dienstprinzipal hinzu:

1. Wählen Sie beim Erstellen des HDInsight-Clusters auf der Registerkarte **Datenquelle** eine **Azure Active Directory-Identität für den Cluster** aus.

2. Wählen Sie im Dialogfeld **Azure Active Directory-Identität für den Cluster** unter **AD-Dienstprinzipal auswählen** die Option **Neu erstellen**.

Nachdem Sie dem Dienstprinzipal einen Namen gegeben und ein Kennwort dafür erstellt haben, klicken Sie auf **ADLS-Zugriff verwalten**, um den Dienstprinzipal Ihren Data Lake-Speichern zuzuordnen.

Sie können Clusterzugriff auf einen oder mehrere Data Lake-Speicher auch nach der Clustererstellung hinzufügen. Öffnen Sie den Azure-Portal-Eintrag für einen Data Lake-Speicher, und wechseln Sie zu **Daten-Explorer > Zugriff > Hinzufügen**. 

### <a name="how-to-access-the-data-lake-store-from-r-server"></a>Zugreifen auf den Data Lake-Speicher mit R Server

Nachdem Sie einem Data Lake-Speicher Zugriff gewährt haben, können Sie den Speicher in R Server unter HDInsight wie ein sekundäres Azure-Speicherkonto nutzen. Der einzige Unterschied ist, dass sich das Präfix **wasb://** wie folgt in **adl://** ändert:


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


Die folgenden Befehle werden zum Konfigurieren des Data Lake-Speicherkontos mit dem RevoShare-Verzeichnis und zum Hinzufügen der CSV-Beispieldatei aus dem vorherigen Beispiel verwendet:


    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user
    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user/RevoShare
    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user/RevoShare/<user>

    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/share

    hadoop fs -copyFromLocal /usr/lib64/R Server-7.4.1/library/RevoScaleR/SampleData/AirlineDemoSmall.csv adl://rkadl1.azuredatalakestore.net/share

    hadoop fs –ls adl://rkadl1.azuredatalakestore.net/share


## <a name="use-azure-file-storage-with-r-server"></a>Verwenden von Azure File Storage mit R Server

Es ist auch eine komfortable Datenspeicheroption zur Verwendung auf dem Edgeknoten mit der Bezeichnung [Azure Files]((https://azure.microsoft.com/services/storage/files/) vorhanden. Hiermit können Sie eine Azure Storage-Dateifreigabe auf einem Linux-Dateisystem bereitstellen. Diese Option kann zum Speichern von Datendateien, R-Skripts und Ergebnisobjekten nützlich sein, die unter Umständen später benötigt werden, besonders wenn es sinnvoll ist, anstelle von HDFS das native Dateisystem auf dem Edgeknoten zu verwenden. 

Ein großer Vorteil von Azure Files besteht darin, dass die Dateifreigaben von allen Systemen mit einem unterstützten Betriebssystem, z.B. Windows oder Linux, bereitgestellt und verwendet werden können. Beispielsweise ist die Verwendung durch einen anderen HDInsight-Cluster, der Ihnen oder einem Mitglied Ihres Teams gehört, eine Azure VM oder sogar ein lokales System möglich. Weitere Informationen finden Sie unter:

- [Verwenden des Azure-Dateispeichers unter Linux](../storage/storage-how-to-use-files-linux.md)
- [Verwenden von Azure File Storage unter Windows](../storage/storage-dotnet-how-to-use-files.md)


## <a name="next-steps"></a>Nächste Schritte

Da Sie nun die Azure-Speicheroptionen kennen, verwenden Sie die folgenden Links, um Methoden zur Erledigung von Data Science-Aufgaben mit R Server unter HDInsight kennenzulernen.

* [Übersicht über R Server in HDInsight](hdinsight-hadoop-r-server-overview.md)
* [Erste Schritte mit R Server in Hadoop](hdinsight-hadoop-r-server-get-started.md)
* [Hinzufügen von RStudio Server zu HDInsight (falls nicht bereits während der Clustererstellung hinzugefügt)](hdinsight-hadoop-r-server-install-r-studio.md)
* [Rechenkontextoptionen für R Server in HDInsight](hdinsight-hadoop-r-server-compute-contexts.md)


