---
title: "Erstellen von Hadoop-Clustern mit Speicherkonten mit sicherer Übertragung in Azure HDInsight | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie HDInsight-Cluster mit Azure-Speicherkonten mit sicherer Übertragung erstellen."
keywords: "Erste Schritte mit Hadoop, Hadoop Linux, Hadoop Schnellstart, sichere Übertragung, Azure-Speicherkonto"
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/07/2017
ms.author: jgao
ms.translationtype: HT
ms.sourcegitcommit: 1dbb1d5aae55a4c926b9d8632b416a740a375684
ms.openlocfilehash: 5e9cae224e5a36e1529c962ad8cbab77176dda8c
ms.contentlocale: de-de
ms.lasthandoff: 08/07/2017

---
# <a name="create-hadoop-cluster-with-secure-transfer-storage-accounts-in-azure-hdinsight"></a>Erstellen von Hadoop-Clustern mit Speicherkonten mit sicherer Übertragung in Azure HDInsight

Das Feature [Sichere Übertragung erforderlich](../storage/storage-require-secure-transfer.md) erhöht die Sicherheit Ihres Azure Storage-Kontos, indem alle Anforderungen für Ihr Konto über eine sichere Verbindung übertragen werden müssen. Dieses Feature und das WASB-Schema werden erst ab HDInsight-Clusterversion 3.6 unterstützt. 

>[!NOTE] 
> Das Erstellen von Clustern mit Speicherkonten mit sicherer Übertragung unter Verwendung des .NET SDKs wird derzeit nicht unterstützt. Legen Sie zur Umgehung dieses Problems „wasbs“ in der Eigenschaft „fs.defaultFS“ in der core-site-Konfiguration als Teil von „ClusterCreateParametersExtended“ fest.

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie mit diesem Tutorial beginnen können, benötigen Sie Folgendes:

* **Azure-Abonnement:**Um ein kostenloses Testkonto für die Dauer eines Monats zu erstellen, navigieren Sie zu [azure.microsoft.com/free](https://azure.microsoft.com/free).
* **Ein Azure Storage-Konto mit sicherer Übertragung.** Entsprechende Anleitungen finden Sie unter [Erstellen Sie ein Speicherkonto.](../storage/storage-create-storage-account.md#create-a-storage-account) sowie unter [Vorschreiben einer sicheren Übertragung](../storage/storage-require-secure-transfer.md).
* **Blobcontainer im Speicherkonto.** 
## <a name="create-cluster"></a>Cluster erstellen

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


In diesem Abschnitt erstellen Sie einen Hadoop-Cluster in HDInsight mit einer [Azure Resource Manager-Vorlage](../azure-resource-manager/resource-group-template-deploy.md). Die Vorlage befindet sich auf [GitHub](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-with-existing-default-storage-account/). Für dieses Tutorial ist keine Erfahrung mit Resource Manager-Vorlagen erforderlich. Andere Methoden zur Erstellung von Clustern und Informationen zu den in diesem Tutorial verwendeten Eigenschaften finden Sie unter [Erstellen von HDInsight-Clustern](hdinsight-hadoop-provision-linux-clusters.md).

1. Klicken Sie auf die folgende Abbildung, um sich bei Azure anzumelden, und öffnen Sie die Resource Manager-Vorlage im Azure-Portal. 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-with-existing-default-storage-account%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-hadoop-linux-tutorial-get-started/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Erstellen Sie den Cluster wie folgt: 

    - Geben Sie die HDInsight-Version 3.6 an.  Die Standardversion ist 3.5. Es wird mindestens Version 3.6 benötigt.
    - Geben Sie ein Speicherkonto mit sicherer Übertragung an.
    - Verwenden Sie einen kurzen Namen für das Speicherkonto.
    - Speicherkonto und Blobcontainer müssen vorab erstellt werden. 

    Eine entsprechende Anleitung finden Sie unter [Cluster erstellen](./hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster). 

Wenn Sie mithilfe der Skriptaktion Ihre eigenen Konfigurationsdateien bereitstellen, müssen Sie WASBS in den folgenden Einstellungen verwenden:

- fs.defaultFS (core-site)
- spark.eventLog.dir 
- spark.history.fs.logDirectory

## <a name="add-additional-storage-accounts"></a>Hinzufügen weiterer Speicherkonten

Weitere Speicherkonten mit sicherer Übertragung können auf unterschiedliche Weise hinzugefügt werden:

- Ändern Sie die Azure Resource Manager-Vorlage im letzten Abschnitt.
- Erstellen Sie über das [Azure-Portal](https://portal.azure.com) einen Cluster, und geben Sie ein verknüpftes Speicherkonto an.
- Verwenden Sie die Skriptaktion, um einem vorhandenen HDInsight-Cluster weitere Speicherkonten mit sicherer Übertragung hinzuzufügen.  Weitere Informationen finden Sie unter [Hinzufügen zusätzlicher Speicherkonten zu HDInsight](hdinsight-hadoop-add-storage.md).

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie gelernt, wie Sie einen HDInsight-Cluster erstellen und die sichere Übertragung an die Speicherkonten aktivieren.

Weitere Informationen zur Datenanalyse mit HDInsight finden Sie in den folgenden Artikeln:

* Weitere Informationen zum Verwenden von Hive mit HDInsight (etwa zum Ausführen von Hive-Abfragen in Visual Studio) finden Sie im Artikel zum [Verwenden von Hive mit HDInsight][hdinsight-use-hive].
* Informationen zu Pig (einer Sprache zum Transformieren von Daten) finden Sie im Artikel zum [Verwenden von Pig mit HDInsight][hdinsight-use-pig].
* Informationen zu MapReduce (einer Möglichkeit zum Schreiben von Programmen, die Daten in Hadoop verarbeiten) finden Sie im Artikel zum [Verwenden von MapReduce mit HDInsight][hdinsight-use-mapreduce].
* Informationen zur Verwendung der HDInsight-Tools für Visual Studio zum Analysieren von Daten in HDInsight finden Sie unter [Erste Schritte bei der Verwendung von Hadoop-Tools für Visual Studio für HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md).

Weitere Informationen dazu, wie HDInsight Daten speichert und wie Sie Daten an HDInsight übertragen, finden Sie in den folgenden Artikeln:

* Informationen zur Verwendung von Azure Storage durch HDInsight finden Sie unter [Verwenden von Azure Storage mit HDInsight](hdinsight-hadoop-use-blob-storage.md).
* Informationen zum Hochladen von Daten in HDInsight finden Sie im Artikel zum [Hochladen von Daten zu HDInsight][hdinsight-upload-data].

Weitere Informationen zum Erstellen und Verwalten von HDInsight-Clustern finden Sie in den folgenden Artikeln:

* Informationen zum Verwalten eines Linux-basierten HDInsight-Clusters finden Sie unter [Verwalten von HDInsight-Clustern mit Ambari](hdinsight-hadoop-manage-ambari.md).
* Informationen zu den Optionen, die Sie beim Erstellen eines HDInsight-Clusters auswählen können, finden Sie unter [Erstellen von HDInsight unter Linux mit benutzerdefinierten Optionen](hdinsight-hadoop-provision-linux-clusters.md).
* Wenn Sie mit Linux und Hadoop vertraut sind und detaillierte Informationen zu Hadoop in HDInsight benötigen, finden Sie diese unter [Informationen zur Verwendung von HDInsight unter Linux](hdinsight-hadoop-linux-information.md). Dieser Artikel enthält u.a. folgende Informationen:
  
  * URLs für im Cluster gehostete Dienste, z. B. Ambari und WebHCat
  * Speicherort von Hadoop-Dateien und -Beispielen im lokalen Dateisystem
  * Verwendung von Azure-Speicher (WASB) anstelle von HDFS als Standarddatenspeicher

[1]: ../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md

[hdinsight-provision]: hdinsight-provision-linux-clusters.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md



