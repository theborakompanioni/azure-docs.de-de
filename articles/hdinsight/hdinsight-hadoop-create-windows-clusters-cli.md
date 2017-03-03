---
title: Erstellen Windows-basierter Hadoop-Cluster in HDInsight mit der Azure-Befehlszeilenschnittstelle
description: "Erfahren Sie, wie Sie Windows-basierte Hadoop-Cluster für Azure HDInsight mithilfe der Azure CLI erstellen."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: c766544c-c16f-4bfa-89d0-592325d24250
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/06/2017
ms.author: jgao
translationtype: Human Translation
ms.sourcegitcommit: a2b32f23381ed1f9912edf6432f029e51bdf1be4
ms.openlocfilehash: 393b7e44b21fe510e07b4048ddd3bdbcc31d90a9
ms.lasthandoff: 02/07/2017


---
# <a name="create-windows-based-hadoop-clusters-in-hdinsight-using-azure-cli"></a>Erstellen Windows-basierter Hadoop-Cluster in HDInsight mit der Azure-Befehlszeilenschnittstelle

[!INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Erfahren Sie, wie Sie Windows-basierte Hadoop-Cluster in HDInsight mithilfe der Azure CLI erstellen. 

> [!IMPORTANT]
> Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Ende des Lebenszyklus von HDInsight unter Windows](hdinsight-component-versioning.md#hdi-version-32-and-33-nearing-deprecation-date). Die Informationen in diesem Artikel gelten nur für Windows-basierte HDInsight-Cluster. Informationen zum Erstellen von Linux-basierten Clustern finden Sie unter [Erstellen von Linux-basierten Clustern in HDInsight mit der Azure CLI](hdinsight-hadoop-create-linux-clusters-azure-cli.md).

## <a name="prerequisites"></a>Voraussetzungen:
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Bevor Sie die Anweisungen in diesem Artikel ausführen können, benötigen Sie Folgendes:

* **Azure-Abonnement**. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Azure-Befehlszeilenschnittstelle**.
  
[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)] 

### <a name="access-control-requirements"></a>Voraussetzungen für die Zugriffssteuerung
[!INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="connect-to-azure"></a>Herstellen einer Verbindung mit Azure
Stellen Sie über den folgenden Befehl eine Verbindung mit Azure her:

    azure login

Weitere Informationen zur Authentifizierung mit einem Geschäfts- oder Schulkonto finden Sie unter [Herstellen einer Verbindung mit einem Azure-Abonnement über die Azure-Befehlszeilenschnittstelle](../xplat-cli-connect.md).

Führen Sie den folgenden Befehl aus, um in den Azure Resource Manager-Modus zu wechseln:

    azure config mode arm

Um Hilfe zu erhalten, verwenden Sie die Option **-h** .  Beispiel:

    azure hdinsight cluster create -h

## <a name="create-clusters"></a>Erstellen von Clustern
Zum Erstellen eines HDInsight-Clusters benötigen Sie eine Ressourcenverwaltungsgruppe und ein Azure Blob Storage-Konto. Zum Erstellen eines HDInsight-Clusters müssen Sie Folgendes angeben:

* **Azure-Ressourcengruppe**: Ein Data Lake Analytics-Konto muss innerhalb einer Azure-Ressourcengruppe erstellt werden. Azure Resource Manager ermöglicht es Ihnen, mit den Ressourcen in Ihrer Anwendung als Gruppe zu arbeiten. Sie können alle Ressourcen für Ihre Anwendung in einem einzigen, koordinierten Vorgang bereitstellen, aktualisieren oder löschen.
  
    So listen Sie die Ressourcengruppen in Ihrem Abonnement auf:
  
        azure group list
  
    So erstellen Sie eine neue Ressourcengruppe:
  
        azure group create -n "<Resource Group Name>" -l "<Azure Location>"
* **HDInsight-Clustername**
* **Standort**: Eines der Azure-Datencenter, das HDInsight-Cluster unterstützt. Eine Liste unterstützter Standorte finden Sie unter [HDInsight – Preise](https://azure.microsoft.com/pricing/details/hdinsight/).
* **Standardspeicherkonto**: HDInsight verwendet als Standarddateisystem einen Azure-Blobspeichercontainer. Sie benötigen ein Azure-Speicherkonto, um einen HDInsight-Cluster erstellen zu können.
  
    So erstellen Sie ein neues Azure-Speicherkonto:
  
        azure storage account create "<Azure Storage Account Name>" -g "<Resource Group Name>" -l "<Azure Location>" --type LRS
  
  > [!NOTE]
  > Das Speicherkonto muss sich im selben Datencenter wie HDInsight befinden.
  > Der Speicherkontotyp darf nicht ZRS sein, da ZRS keine Tabellen unterstützt.
  > 
  > 
  
    Informationen zum Erstellen eines Azure Storage-Kontos im Azure-Portal finden Sie unter [Erstellen, Verwalten oder Löschen eines Speicherkontos][azure-create-storageaccount].
  
    Falls Sie bereits ein Speicherkonto haben, aber dessen Kontonamen und Kontoschlüssel nicht kennen, können Sie diese Daten mit den folgenden Befehlen abrufen:
  
        -- Lists Storage accounts
        azure storage account list
        -- Shows a Storage account
        azure storage account show "<Storage Account Name>"
        -- Lists the keys for a Storage account
        azure storage account keys list "<Storage Account Name>" -g "<Resource Group Name>"
  
    Ausführliche Informationen zum Abrufen dieser Informationen über das Azure-Portal finden Sie unter [Informationen zu Azure-Speicherkonten](../storage/storage-create-storage-account.md#manage-your-storage-account) im Abschnitt „Verwalten von Speicherkonten“.
* **(Optional) Standardblobcontainer**: Der Befehl **azure hdinsight cluster create** erstellt den Container, sofern dieser noch nicht vorhanden ist. Falls Sie den Container zuvor erstellen möchten, können Sie den folgenden Befehl verwenden:
  
    azure storage container create --account-name "<Storage Account Name>" --account-key <Storage Account Key> [ContainerName]

Sobald Sie Ihr Speicherkonto vorbereitet haben, können Sie einen Cluster erstellen.

    azure hdinsight cluster create -g <Resource Group Name> -c <HDInsight Cluster Name> -l <Location> --osType <Windows | Linux> --version <Cluster Version> --clusterType <Hadoop | HBase | Spark | Storm> --workerNodeCount 2 --headNodeSize Large --workerNodeSize Large --defaultStorageAccountName <Azure Storage Account Name>.blob.core.windows.net --defaultStorageAccountKey "<Default Storage Account Key>" --defaultStorageContainer <Default Blob Storage Container> --userName admin --password "<HTTP User Password>" --rdpUserName <RDP Username> --rdpPassword "<RDP User Password" --rdpAccessExpiry "03/01/2016"


## <a name="create-clusters-using-configuration-files"></a>Erstellen von Clustern mithilfe von Konfigurationsdateien
Normalerweise erstellen Sie einen HDInsight-Cluster, führen Aufträge dafür aus und löschen den Cluster anschließend wieder, um die Kosten möglichst gering zu halten. Über die Befehlszeilenschnittstelle können Sie die Konfigurationen in einer Datei speichern, um sie bei der nächsten Clustererstellung erneut verwenden zu können.  

    azure hdinsight config create [options ] <Config File Path> <overwirte>
    azure hdinsight config add-config-values [options] <Config File Path>
    azure hdinsight config add-script-action [options] <Config File Path>

Beispiel: Erstellen einer Konfigurationsdatei mit einer Skriptaktion, die beim Erstellen eines Clusters ausgeführt wird.

    azure hdinsight config create "C:\myFiles\configFile.config"
    azure hdinsight config add-script-action --configFilePath "C:\myFiles\configFile.config" --nodeType HeadNode --uri <Script Action URI> --name myScriptAction --parameters "-param value"
    azure hdinsight cluster create --configurationPath "C:\myFiles\configFile.config"

## <a name="create-clusters-with-script-action"></a>Erstellen von Clustern mithilfe von Skriptaktionen
Erstellen einer Konfigurationsdatei mit einer Skriptaktion, die beim Erstellen eines Clusters ausgeführt wird.

    azure hdinsight config create "C:\myFiles\configFile.config"
    azure hdinsight config add-script-action --configFilePath "C:\myFiles\configFile.config" --nodeType HeadNode --uri <scriptActionURI> --name myScriptAction --parameters "-param value"

Erstellen eines Clusters mit einer Skriptaktion

    azure hdinsight cluster create -g myarmgroup01 -l westus -y Windows --clusterType Hadoop --version 3.2 --defaultStorageAccountName mystorageaccount --defaultStorageAccountKey <defaultStorageAccountKey> --defaultStorageContainer mycontainer --userName admin --password <clusterPassword> --sshUserName sshuser --sshPassword <sshPassword> --workerNodeCount 1 –configurationPath "C:\myFiles\configFile.config" myNewCluster01


Weitere Informationen zu Skriptaktionen finden Sie unter [Anpassen von HDInsight-Clustern mithilfe von Skriptaktionen (Linux)](hdinsight-hadoop-customize-cluster.md).

## <a name="create-clusters-using-resource-manager-templates"></a>Erstellen von Clustern mit Resource Manager-Vorlagen
Sie können die CLI zum Erstellen von Clustern verwenden, indem Azure Resource Manager-Vorlagen aufgerufen werden. Weitere Informationen finden Sie unter [Bereitstellen über die Azure-Befehlszeilenschnittstelle](hdinsight-hadoop-create-windows-clusters-arm-templates.md#deploy-with-azure-cli).

## <a name="see-also"></a>Siehe auch
* [Erste Schritte mit Azure HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md) – Erfahren Sie, wie Sie die Arbeit mit Ihrem HDInsight-Cluster aufnehmen können.
* [Programmgesteuerte Übermittlung von Hadoop-Jobs](hdinsight-submit-hadoop-jobs-programmatically.md) – Erfahren Sie, wie Sie Aufträge programmgesteuert an HDInsight übermitteln können.
* [Verwalten von Hadoop-Clustern in HDInsight mit der Azure-Befehlszeilenschnittstelle](hdinsight-administer-use-command-line.md)
* [Verwenden der Azure-Befehlszeilenschnittstelle für Mac, Linux und Windows mit der Azure-Dienstverwaltung](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2)


