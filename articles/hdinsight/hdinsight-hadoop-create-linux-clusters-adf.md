---
title: Erstellen von Azure HDInsight (Hadoop)-Clustern mithilfe von Data Factory | Microsoft-Dokumentation
description: Sie erfahren, wie Sie bedarfsgesteuerte Hadoop-Cluster in HDInsight mit Azure Data Factory erstellen.
services: hdinsight
documentationcenter: 
tags: azure-portal
author: spelluru
manager: jhubbard
editor: cgronlun
ms.assetid: 1f3b3a78-4d16-4d99-ba6e-06f7bb185d6a
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/23/2017
ms.author: spelluru
translationtype: Human Translation
ms.sourcegitcommit: d6a9be6c5c681cba04fa8ba216c89234ae35b846
ms.openlocfilehash: 8e554fc73906a912c426cabf6f6dffdc10002c9b
ms.lasthandoff: 02/27/2017


---
# <a name="create-on-demand-hadoop-clusters-in-hdinsight-using-azure-data-factory"></a>Erstellen bedarfsgesteuerter Hadoop-Cluster in HDInsight mit Azure Data Factory
[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

[Azure Data Factory](../data-factory/data-factory-introduction.md) ist ein cloudbasierter Datenintegrationsdienst, der das Verschieben und Transformieren von Daten orchestriert und automatisiert. Er kann einen HDInsight Hadoop-Cluster genau rechtzeitig erstellen, um einen eingehenden Datenslice zu verarbeiten, und den Cluster löschen, wenn die Verarbeitung abgeschlossen ist. Vorteile eines bedarfsgesteuerten HDInsight Hadoop-Clusters:

- Sie zahlen nur die Zeit, die der Auftrag im HDInsight Hadoop-Cluster ausgeführt wird (zuzüglich einer kurzen konfigurierbaren Leerlaufzeit). Die Abrechnung für die HDInsight-Cluster erfolgt anteilsmäßig auf Minutenbasis, unabhängig davon, ob Sie sie verwenden. Wenn Sie einen bedarfsgesteuerten, mit HDInsight verknüpften Dienst in der Data Factory verwenden, werden die Cluster bei Bedarf erstellt. Zudem werden die Cluster automatisch gelöscht, wenn die Aufträge abgeschlossen sind. Sie bezahlen daher nur für die Ausführungszeit von Aufträgen und eine kurze Leerlaufzeit (Einstellung für die Gültigkeitsdauer). 
- Sie können einen Workflow mit einer Data Factory-Pipeline erstellen. Beispiel: Sie können die Pipeline zum Kopieren von Daten aus einer lokalen SQL Server-Instanz in einen Azure-Blobspeicher und zum Verarbeiten der Daten durch Ausführen eines Hive-Skripts und eines Pig-Skripts in einem bedarfsgesteuerten HDInsight Hadoop-Cluster verwenden. Kopieren Sie dann die Ergebnisdaten in ein Azure SQL Data Warehouse für BI-Anwendungen.
- Sie können die regelmäßige Ausführung des Workflows planen (stündlich, täglich, wöchentlich, monatlich usw.).

In Azure Data Factory kann eine Data Factory über mindestens eine Datenpipeline verfügen. Eine Datenpipeline verfügt über mindestens eine Aktivität. Es gibt zwei Arten von Aktivitäten: [Datenverschiebungen](../data-factory/data-factory-data-movement-activities.md) und [Datentransformationen](../data-factory/data-factory-data-transformation-activities.md). Sie können Datenverschiebungen (derzeit nur Kopieraktivitäten) zum Verschieben von Daten aus einem Quelldatenspeicher in einen Zieldatenspeicher verwenden. Sie können Datentransformationsaktivitäten verwenden, um Daten zu übertragen und zu verarbeiten. Die HDInsight-Hive-Aktivität ist eine der Transformationsaktivitäten, die von Data Factory unterstützt werden. In diesem Tutorial verwenden Sie die Hive-Transformation. 

Sie können eine Hive-Aktivität so konfigurieren, dass Ihr eigener HDInsight Hadoop-Cluster oder ein bedarfsgesteuerter HDInsight Hadoop-Cluster verwendet wird. In diesem Tutorial wird die Hive-Aktivität in der Data Factory-Pipeline so konfiguriert, dass ein bedarfsgesteuerter HDInsight-Cluster verwendet wird. Aus diesem Grund geschieht während der Ausführung der Aktivität zur Verarbeitung eines Datenslice Folgendes: 

1. Ein HDInsight Hadoop-Cluster wird automatisch rechtzeitig zur Verarbeitung des Slice für Sie erstellt.  
2. Die Eingabedaten werden durch Ausführen des folgenden HiveQL-Skripts im Cluster verarbeitet. 
3. Der HDInsight Hadoop-Cluster wird gelöscht, sobald die Verarbeitung abgeschlossen ist, und der Cluster befindet sich während der konfigurierten Zeitspanne im Leerlauf (TimeToLive-Einstellung). Wenn der nächste Datenslice zur Verarbeitung innerhalb dieser TimeToLive-Leerlaufzeit verfügbar ist, wird der gleiche Cluster wie für die Verarbeitung des Slice verwendet.  
      
In diesem Tutorial führt das HiveQL-Skript, das der Hive-Aktivität zugeordnet ist, die folgenden Aktionen aus: 

1. Erstellt eine externe Tabelle, die auf die unformatierten Webprotokolldaten verweist, die in einem Azure-Blobspeicher gespeichert sind.
2. Unterteilt die unformatierten Daten nach Jahr und Monat.
3. Speichert die partitionierten Daten im Azure-Blobspeicher. 

In diesem Tutorial erstellt das HiveQL-Skript, das der Hive-Aktivität zugeordnet ist, eine externe Tabelle, die auf die unformatierten in Azure Blob Storage gespeicherten Webprotokolldaten verweist. Hier sind die Beispielzeilen für jeden Monat in der Eingabedatei.

```
2014-01-01,02:01:09,SAMPLEWEBSITE,GET,/blogposts/mvc4/step2.png,X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,53175,871
2014-02-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871
2014-03-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871
```

Das HiveQL-Skript unterteilt die Rohdaten nach Jahr und Monat. Es erstellt auf Grundlage der vorherigen Eingabe drei Ausgabeordner. Jeder Ordner enthält eine Datei mit den Einträgen aus jedem Monat.

```
adfgetstarted/partitioneddata/year=2014/month=1/000000_0
adfgetstarted/partitioneddata/year=2014/month=2/000000_0
adfgetstarted/partitioneddata/year=2014/month=3/000000_0
```

Eine Liste der Data Factory-Datentransformationsaktivitäten zusätzlich zur Hive-Aktivität finden Sie unter [Transformation und Analyse mit Azure Data Factory](../data-factory/data-factory-data-transformation-activities.md).

> [!NOTE]
> Derzeit können Sie nur HDInsight-Cluster der Version 3.2 aus Azure Data Factory erstellen.

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie die Anweisungen in diesem Artikel ausführen können, benötigen Sie Folgendes:

* [Azure-Abonnement](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Azure PowerShell.

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell.md)]

### <a name="prepare-storage-account"></a>Vorbereiten des Speicherkontos
Sie können in diesem Szenario bis zu drei Speicherkonten verwenden:

- Standardspeicherkonto für den HDInsight-Cluster
- Speicherkonto für die Eingabedaten
- Speicherkonto für die Ausgabedaten

Zur Vereinfachung des Tutorials verwenden Sie ein Speicherkonto für die drei Aufgaben. Die Beispielskripts für Azure PowerShell aus diesem Abschnitt führen die folgenden Aufgaben aus:

1. Melden Sie sich bei Azure an.
2. Erstellen Sie eine Azure-Ressourcengruppe.
3. Erstellen eines Azure-Speicherkontos.
4. Erstellen eines Blobcontainers im Speicherkonto
5. Kopieren der folgenden beiden Dateien in den Blobcontainer:

   * Eingabedatendatei: [https://hditutorialdata.blob.core.windows.net/adfhiveactivity/inputdata/input.log](https://hditutorialdata.blob.core.windows.net/adfhiveactivity/inputdata/input.log)
   * HiveQL-Skript: [https://hditutorialdata.blob.core.windows.net/adfhiveactivity/script/partitionweblogs.hql](https://hditutorialdata.blob.core.windows.net/adfhiveactivity/script/partitionweblogs.hql)

     Beide Dateien werden in einem öffentlichen Blobcontainer gespeichert.


**So bereiten Sie den Speicher vor und kopieren die Dateien mithilfe von Azure PowerShell**:
> [!IMPORTANT]
> Geben Sie Namen für die Azure-Ressourcengruppe und das Azure-Speicherkonto an, die anhand des Skripts erstellt werden sollen.
> Notieren Sie den **Namen der Ressourcengruppe**, den **Namen des Speicherkontos** und den **Speicherkontoschlüssel**, die vom Skript ausgegeben werden. Sie benötigen diese Angaben im nächsten Abschnitt.

```powershell
$resourceGroupName = "<Azure Resource Group Name>"
$storageAccountName = "<Azure Storage Account Name>"
$location = "East US 2"

$sourceStorageAccountName = "hditutorialdata"  
$sourceContainerName = "adfhiveactivity"

$destStorageAccountName = $storageAccountName
$destContainerName = "adfgetstarted" # don't change this value.

####################################
# Connect to Azure
####################################
#region - Connect to Azure subscription
Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
try{Get-AzureRmContext}
catch{Login-AzureRmAccount}
#endregion

####################################
# Create a resource group, storage, and container
####################################

#region - create Azure resources
Write-Host "`nCreating resource group, storage account and blob container ..." -ForegroundColor Green

New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
New-AzureRmStorageAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $destStorageAccountName `
    -type Standard_LRS `
    -Location $location

$destStorageAccountKey = (Get-AzureRmStorageAccountKey `
    -ResourceGroupName $resourceGroupName `
    -Name $destStorageAccountName)[0].Value

$sourceContext = New-AzureStorageContext `
    -StorageAccountName $sourceStorageAccountName `
    -Anonymous
$destContext = New-AzureStorageContext `
    -StorageAccountName $destStorageAccountName `
    -StorageAccountKey $destStorageAccountKey

New-AzureStorageContainer -Name $destContainerName -Context $destContext
#endregion

####################################
# Copy files
####################################
#region - copy files
Write-Host "`nCopying files ..." -ForegroundColor Green

$blobs = Get-AzureStorageBlob `
    -Context $sourceContext `
    -Container $sourceContainerName

$blobs|Start-AzureStorageBlobCopy `
    -DestContext $destContext `
    -DestContainer $destContainerName

Write-Host "`nCopied files ..." -ForegroundColor Green
Get-AzureStorageBlob -Context $destContext -Container $destContainerName
#endregion

Write-host "`nYou will use the following values:" -ForegroundColor Green
write-host "`nResource group name: $resourceGroupName"
Write-host "Storage Account Name: $destStorageAccountName"
write-host "Storage Account Key: $destStorageAccountKey"

Write-host "`nScript completed" -ForegroundColor Green
```

Wenn Sie Hilfe mit dem PowerShell-Skript benötigen, lesen Sie [Verwenden von Azure PowerShell mit Azure Storage](../storage/storage-powershell-guide-full.md). Wenn Sie die Azure CLI verwenden möchten, finden Sie weitere Informationen zum Azure CLI-Skript im Abschnitt [Anhang](#appendix).

**So untersuchen Sie das Speicherkonto und den Inhalt**

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Klicken Sie im linken Bereich auf **Ressourcengruppen**.
3. Doppelklicken Sie auf den Namen der Ressourcengruppe, die Sie im PowerShell-Skript erstellt haben. Verwenden Sie den Filter, wenn zu viele Ressourcengruppen aufgeführt werden.
4. Auf der Kachel **Ressourcen** sollte eine Ressource aufgeführt sein, wenn Sie die Ressourcengruppe nicht für andere Projekte freigegeben haben. Diese Ressource ist das Speicherkonto mit dem Namen, den Sie zuvor angegeben haben. Klicken Sie auf den Namen des Speicherkontos.
5. Klicken Sie auf die Kachel **Blobs**.
6. Klicken Sie auf den Container **adfgetstarted**. Es werden zwei Ordner angezeigt: **inputdata** und **script**. 
7. Öffnen Sie die Ordner, und überprüfen Sie die Dateien in den Ordnern. „inputdata“ enthält die Datei „input.log“ mit den Eingabedaten, und der Ordner „script“ enthält die HiveQL-Skriptdatei. 

## <a name="create-a-data-factory-using-resource-manager-template"></a>Erstellen einer Data Factory mit Resource Manager-Vorlage
Wenn das Speicherkonto, die Eingabedaten und das HiveQL-Skript vorbereitet sind, können Sie eine Azure Data Factory erstellen. Es gibt mehrere Methoden zum Erstellen einer Data Factory. In diesem Tutorial erstellen Sie eine Data Factory durch Bereitstellen einer Azure Resource Manager-Vorlage mithilfe des Azure-Portals. Sie können auch eine Resource Manager-Vorlage mit der [Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md) und [Azure PowerShell](../azure-resource-manager/resource-group-template-deploy.md#deploy) bereitstellen. Andere Methoden für die Erstellung einer Data Factory finden Sie unter [Tutorial: Erstellen Ihrer ersten Data Factory](../data-factory/data-factory-build-your-first-pipeline.md).

1. Klicken Sie auf die folgende Abbildung, um sich bei Azure anzumelden, und öffnen Sie die Resource Manager-Vorlage im Azure-Portal. Die Vorlage befindet sich unter „https://hditutorialdata.blob.core.windows.net/adfhiveactivity/data-factory-hdinsight-on-demand.json“. Im Abschnitt [Data Factory entities in the template](#data-factory-entities-in-the-template) (Data Factory-Entitäten in der Vorlage) finden Sie detaillierte Informationen zu in der Vorlage definierten Entitäten. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fadfhiveactivity%2Fdata-factory-hdinsight-on-demand.json" target="_blank"><img src="./media/hdinsight-hadoop-create-linux-clusters-adf/deploy-to-azure.png" alt="Deploy to Azure"></a>
2. Wählen Sie die Option **Vorhandene verwenden** für die Einstellung **Ressourcengruppe** und den Namen der Ressourcengruppe aus, die Sie im vorherigen Schritt (mithilfe des PowerShell-Skripts) erstellt haben. 
3. Geben Sie einen Namen für die Data Factory ein (**Name der Data Factory**). Dieser Name muss global eindeutig sein.
4. Geben Sie den **Speicherkontonamen** und **Speicherkontoschlüssel** ein, die Sie im vorherigen Schritt notiert haben.
5. Wählen Sie die Option **Ich stimme den oben genannten Geschäftsbedingungen zu** aus, nachdem Sie die **Geschäftsbedingungen** gelesen haben.
6. Wählen Sie die Option **An Dashboard anheften** aus. 
6. Klicken Sie auf **Purchase/Create** (Kaufen/Erstellen). Eine Kachel mit dem Namen **Vorlagenbereitstellung bereitstellen** wird auf dem Dashboard angezeigt. Warten Sie, bis das Blatt **Ressourcengruppe** für Ihre Ressourcengruppe geöffnet wird. Sie können auch auf die Kachel mit dem Namen Ihrer Ressourcengruppe klicken, um das Blatt der Ressourcengruppe zu öffnen. 
6. Klicken Sie auf die Kachel, um die Ressourcengruppe zu öffnen, wenn das Blatt der Ressourcengruppe nicht bereits geöffnet ist. Jetzt sollte neben der Speicherkontoressource eine weitere Data Factory-Ressource aufgeführt werden.
7. Klicken Sie auf den Namen der Data Factory (Wert, den Sie für den Parameter **Name der Data Factory** angegeben haben).
8. Klicken Sie auf dem Blatt „Data Factory“ auf die Kachel **Diagramm**. Das Diagramm zeigt eine Aktivität mit einem Eingabedataset und einem Ausgabedataset:

    ![Azure Data Factory – Diagramm der bedarfsgesteuerten HDInsight Hive-Aktivitätspipeline](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-adf-pipeline-diagram.png)

    Die Namen werden in der Resource Manager-Vorlage definiert.
9. Doppelklicken Sie auf **AzureBlobOutput**.
10. Unter **Zuletzt aktualisierte Slices**sollte ein Slice angezeigt werden. Wenn der Status **In Bearbeitung** ist, warten Sie, bis er in **Bereit** geändert wird. Die Erstellung eines HDInsight-Clusters dauert etwa **20 Minuten**. 

### <a name="check-the-data-factory-output"></a>Überprüfen der Data Factory-Ausgabe

1. Verwenden Sie das gleiche Verfahren in der letzten Sitzung, um den Inhalt des Containers „adfgetstarted“ zu überprüfen. Es gibt zwei neue Container zusätzlich zu **adfgetsarted**:

   * Ein Container mit dem Namen mit folgendem Muster: `adf<yourdatafactoryname>-linkedservicename-datetimestamp`. Dieser Container ist der Standardcontainer für den HDInsight-Cluster. 
   * adfjobs: Dieser Container ist der Container für die ADF-Auftragsprotokolle.

     Die Data Factory-Ausgabe wird in **afgetstarted** gespeichert, wie in der Resource Manager-Vorlage konfiguriert.
2. Klicken Sie auf **adfgetstarted**.
3. Doppelklicken Sie auf **partitioneddata**. Ein Ordner **year=2014** wird angezeigt, da alle Webprotokolle ein Datum aus dem Jahr 2014 haben.

    ![Azure Data Factory – Ausgabe der bedarfsgesteuerten HDInsight Hive-Aktivitätspipeline](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-adf-output-year.png)

    Wenn Sie in der Liste einen Drilldown ausführen, sollten Sie drei Ordner für Januar, Februar und März sehen. Und es gibt ein Protokoll für jeden Monat.

    ![Azure Data Factory – Ausgabe der bedarfsgesteuerten HDInsight Hive-Aktivitätspipeline](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-adf-output-month.png)

## <a name="data-factory-entities-in-the-template"></a>Data Factory-Entitäten in der Vorlage
So sieht die Resource Manager-Vorlage der obersten Ebene für eine Data Factory aus: 

```json
{
    "contentVersion": "1.0.0.0",
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "parameters": { ...
    },
    "variables": { ...
    },
    "resources": [
        {
            "name": "[parameters('dataFactoryName')]",
            "apiVersion": "[variables('apiVersion')]",
            "type": "Microsoft.DataFactory/datafactories",
            "location": "westus",
            "resources": [
                { ... },
                { ... },
                { ... },
                { ... }
            ]
        }
    ]
}
```

### <a name="define-data-factory"></a>Definieren einer Data Factory
Das folgende Beispiel zeigt, wie Sie in der Resource Manager-Vorlage eine Data Factory definieren:  

```json
"resources": [
{
    "name": "[parameters('dataFactoryName')]",
    "apiVersion": "[variables('apiVersion')]",
    "type": "Microsoft.DataFactory/datafactories",
    "location": "westus",
}
```
„dataFactoryName“ ist der Name der Data Factory, die Sie beim Bereitstellen der Vorlage angeben. Data Factory wird derzeit nur in den Regionen „USA, Osten“, „USA, Westen“ und „Europa, Norden“ unterstützt.
   
### <a name="defining-entities-within-the-data-factory"></a>Definieren von Entitäten in der Data Factory
In der JSON-Vorlage werden folgende Data Factory-Entitäten definiert: 

* [Mit Azure Storage verknüpfter Dienst](#azure-storage-linked-service)
* [Bedarfsgesteuerter verknüpfter HDInsight-Dienst](#hdinsight-on-demand-linked-service)
* [Azure-Blob-Eingabedataset](#azure-blob-input-dataset)
* [Azure-Blob-Ausgabedataset](#azure-blob-output-dataset)
* [Datenpipeline mit einer Kopieraktivität](#data-pipeline)

#### <a name="azure-storage-linked-service"></a>Mit Azure Storage verknüpfter Dienst
Die mit Ihrem Dienst verknüpfte Azure Storage-Instanz verbindet Ihr Azure-Speicherkonto mit der Data Factory. In diesem Tutorial wird dasselbe Speicherkonto verwendet, das als Standardspeicherkonto, Eingabedatenspeicher und Ausgabedatenspeicher für HDInsight verwendet wird. Daher definieren Sie nur einen mit Azure Storage verknüpften Dienst. In der Definition des verknüpften Diensts geben Sie den Namen und Schlüssel Ihres Azure-Speicherkontos an. Informationen zu JSON-Eigenschaften zum Definieren eines mit Azure Storage verknüpften Diensts finden Sie unter [Mit Azure Storage verknüpfter Dienst](../data-factory/data-factory-azure-blob-connector.md#azure-storage-linked-service). 

```json
{
    "name": "[variables('storageLinkedServiceName')]",
    "type": "linkedservices",
    "dependsOn": [ "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]" ],
    "apiVersion": "[variables('apiVersion')]",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "[concat('DefaultEndpointsProtocol=https;AccountName=',parameters('storageAccountName'),';AccountKey=',parameters('storageAccountKey'))]"
        }
    }
}
```
Die Verbindungszeichenfolge (**connectionString**) enthält die Parameter „storageAccountName“ und „storageAccountKey“. Sie geben beim Bereitstellen der Vorlage Werte für diese Parameter an.  

#### <a name="hdinsight-on-demand-linked-service"></a>Bedarfsgesteuerter verknüpfter HDInsight-Dienst
In der Definition des bedarfsgesteuerten verknüpften HDInsight-Diensts geben Sie Werte für die Konfigurationsparameter an, die während der Laufzeit vom Data Factory-Dienst zum Erstellen eines HDInsight Hadoop-Clusters verwendet werden. Der Artikel [Verknüpfte Computedienste](../data-factory/data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) enthält Details zu JSON-Eigenschaften, die zum Definieren eines bedarfsgesteuerten verknüpften HDInsight-Diensts verwendet werden.  

```json

{
    "type": "linkedservices",
    "name": "[variables('hdInsightOnDemandLinkedServiceName')]",
    "dependsOn": [
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedservices/', variables('storageLinkedServiceName'))]"
    ],
    "apiVersion": "[variables('apiVersion')]",
    "properties": {
        "type": "HDInsightOnDemand",
        "typeProperties": {
            "osType": "linux",
            "version": "3.2",
            "clusterSize": 1,
            "sshUserName": "myuser",                            
            "sshPassword": "MyPassword!",
            "timeToLive": "00:30:00",
            "linkedServiceName": "[variables('storageLinkedServiceName')]"
        }
    }
}
```
Beachten Sie folgende Punkte: 

* Die Data Factory erstellt einen **Linux-basierten** HDInsight-Cluster für Sie.
* Der HDInsight Hadoop-Cluster wird in der gleichen Region wie das Speicherkonto erstellt.
* Beachten Sie die Einstellung *timeToLive*. Die Data Factory löscht den Cluster automatisch, nachdem der Cluster für 30 Minuten im Leerlauf war.
* Der HDInsight-Cluster erstellt einen **Standardcontainer** im Blobspeicher, den Sie im JSON-Code angegeben haben (**linkedServiceName**). HDInsight löscht diesen Container nicht, wenn der Cluster gelöscht wird. Dieses Verhalten ist beabsichtigt. Durch den bedarfsgesteuerten, mit HDInsight verknüpften Dienst wird jedes Mal ein HDInsight-Cluster erstellt, wenn ein Slice verarbeitet werden muss, es sei denn, ein aktiver Cluster (**timeToLive**) ist vorhanden und wird gelöscht, nachdem die Verarbeitung abgeschlossen ist.

Ausführliche Informationen finden Sie unter [Bedarfsgesteuerter verknüpfter HDInsight-Dienst](../data-factory/data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) .

> [!IMPORTANT]
> Wenn mehr Segmente verarbeitet werden, werden in Ihrem Azure-Blobspeicher viele Container angezeigt. Falls Sie diese für die Problembehandlung der Aufträge nicht benötigen, sollten Sie sie ggf. löschen, um die Speicherkosten zu verringern. Die Namen dieser Container basieren auf dem folgenden Muster: „adf**ihrdatafactoryname**-**nameverknüpfterdienst**-datumuhrzeitstempel“. Verwenden Sie Tools wie den [Microsoft-Speicher-Explorer](http://storageexplorer.com/), um Container in Ihrem Azure-Blobspeicher zu löschen.

#### <a name="azure-blob-input-dataset"></a>Azure-Blob-Eingabedataset
In der Definition des Eingabedatasets geben Sie die Namen des Blobcontainers, des Ordners und der Datei mit den Eingabedaten an. Informationen zu JSON-Eigenschaften zum Definieren eines Azure-Blobdatasets finden Sie unter [Eigenschaften des Dataset-Typs „Azure-Blob“](../data-factory/data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties). 

```json

{
    "type": "datasets",
    "name": "[variables('blobInputDatasetName')]",
    "dependsOn": [
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedServices/', variables('storageLinkedServiceName'))]"
    ],
    "apiVersion": "[variables('apiVersion')]",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "[variables('storageLinkedServiceName')]",
        "typeProperties": {
            "fileName": "input.log",
            "folderPath": "adfgetstarted/inputdata",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "availability": {
            "frequency": "Month",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}

```

Beachten Sie die folgenden spezifischen Einstellungen in der JSON-Definition: 

```json
"fileName": "input.log",
"folderPath": "adfgetstarted/inputdata",
```

#### <a name="azure-blob-output-dataset"></a>Azure-Blob-Ausgabedataset
In der Definition des Ausgabedatasets geben Sie die Namen des Blobcontainers und des Ordners mit den Ausgabedaten an. Informationen zu JSON-Eigenschaften zum Definieren eines Azure-Blobdatasets finden Sie unter [Eigenschaften des Dataset-Typs „Azure-Blob“](../data-factory/data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).  

```json

{
    "type": "datasets",
    "name": "[variables('blobOutputDatasetName')]",
    "dependsOn": [
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedServices/', variables('storageLinkedServiceName'))]"
    ],
    "apiVersion": "[variables('apiVersion')]",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "[variables('storageLinkedServiceName')]",
        "typeProperties": {
            "folderPath": "adfgetstarted/partitioneddata",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "availability": {
            "frequency": "Month",
            "interval": 1,
            "style": "EndOfInterval"
        }
    }
}
```

„folderPath“ gibt den Pfad zu dem Ordner an, der die Ausgabedaten enthält: 

```json
"folderPath": "adfgetstarted/partitioneddata",
```

Die Einstellung für die [Datasetverfügbarkeit](../data-factory/data-factory-create-datasets.md#Availability) lautet wie folgt:

```json
"availability": {
    "frequency": "Month",
    "interval": 1,
    "style": "EndOfInterval"
},
```

In Azure Data Factory steuert die Verfügbarkeit des Ausgabedatasets die Pipeline. In diesem Beispiel wird der Slice jeweils am letzten Tag des Monats (EndOfInterval) erstellt. Weitere Informationen finden Sie unter [Planung und Ausführung mit Data Factory](../data-factory/data-factory-scheduling-and-execution.md).

#### <a name="data-pipeline"></a>Datenpipeline
Sie definieren eine Pipeline zum Transformieren von Daten, indem Sie Hive-Skript in einem bedarfsgesteuerten Azure HDInsight-Cluster ausführen. Informationen zu JSON-Elementen zum Definieren einer Pipeline für dieses Beispiel finden Sie unter [Pipeline-JSON](../data-factory/data-factory-create-pipelines.md#pipeline-json). 

```json
{
    "type": "datapipelines",
    "name": "[parameters('dataFactoryName')]",
    "dependsOn": [
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedServices/', variables('storageLinkedServiceName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedServices/', variables('hdInsightOnDemandLinkedServiceName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/datasets/', variables('blobInputDatasetName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/datasets/', variables('blobOutputDatasetName'))]"
    ],
    "apiVersion": "[variables('apiVersion')]",
    "properties": {
        "description": "Azure Data Factory pipeline with an Hadoop Hive activity",
        "activities": [
            {
                "type": "HDInsightHive",
                "typeProperties": {
                    "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                    "scriptLinkedService": "[variables('storageLinkedServiceName')]",
                    "defines": {
                        "inputtable": "[concat('wasb://adfgetstarted@', parameters('storageAccountName'), '.blob.core.windows.net/inputdata')]",
                        "partitionedtable": "[concat('wasb://adfgetstarted@', parameters('storageAccountName'), '.blob.core.windows.net/partitioneddata')]"
                    }
                },
                "inputs": [
                    {
                        "name": "AzureBlobInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutput"
                    }
                ],
                "policy": {
                    "concurrency": 1,
                    "retry": 3
                },
                "name": "RunSampleHiveActivity",
                "linkedServiceName": "HDInsightOnDemandLinkedService"
            }
        ],
        "start": "2016-01-01T00:00:00Z",
        "end": "2016-01-31T00:00:00Z",
        "isPaused": false
    }
}
```

Die Pipeline enthält eine Aktivität: HDInsightHive. Da das Start- und das Enddatum im Januar 2016 liegen, werden nur Daten für einen Monat (ein Slice) verarbeitet. Für *Startdatum* und *Enddatum* der Aktivität ist ein Datum in der Vergangenheit angegeben, sodass die Data Factory Daten für den Monat sofort verarbeitet. Wenn „end“ ein Datum in der Zukunft ist, erstellt die Data Factory einen weiteren Slice, sobald der Zeitpunkt erreicht ist. Weitere Informationen finden Sie unter [Planung und Ausführung mit Data Factory](../data-factory/data-factory-scheduling-and-execution.md).

## <a name="clean-up-the-tutorial"></a>Bereinigen des Tutorials

### <a name="delete-the-blob-containers-created-by-on-demand-hdinsight-cluster"></a>Löschen der Blobcontainer, die anhand des bedarfsgesteuerten HDInsight-Clusters erstellt wurden
Beim bedarfsgesteuerten verknüpften HDInsight-Dienst wird jedes Mal ein HDInsight-Cluster erstellt, wenn ein Slice verarbeitet werden muss – es sei denn, ein aktiver Cluster (timeToLive) ist vorhanden und wird gelöscht, nachdem die Verarbeitung abgeschlossen ist. Azure Data Factory erstellt für jeden Cluster einen Blobcontainer im Azure-Blobspeicher, der als Standardkonto für den Cluster verwendet wird. Auch wenn der HDInsight-Cluster gelöscht wird, werden der standardmäßige Blobspeichercontainer und das zugehörige Speicherkonto nicht gelöscht. Dieses Verhalten ist beabsichtigt. Wenn mehr Segmente verarbeitet werden, werden in Ihrem Azure-Blobspeicher viele Container angezeigt. Falls Sie diese für die Problembehandlung der Aufträge nicht benötigen, sollten Sie sie ggf. löschen, um die Speicherkosten zu verringern. Die Namen dieser Container folgen einem Muster: `adfyourdatafactoryname-linkedservicename-datetimestamp`.

Löschen Sie die Ordner **adfjobs** und **adfyourdatafactoryname-linkedservicename-datetimestamp**. Der Container „adfjobs“ enthält Auftragsprotokolle aus der Azure Data Factory. 

### <a name="delete-the-resource-group"></a>Löschen der Ressourcengruppe
[Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) dient zum Bereitstellen, Verwalten und Überwachen Ihrer Lösung als Gruppe.  Durch das Löschen einer Ressourcengruppe werden alle Komponenten in der Gruppe gelöscht.  

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Klicken Sie im linken Bereich auf **Ressourcengruppen**.
3. Klicken Sie auf den Namen der Ressourcengruppe, die Sie im PowerShell-Skript erstellt haben. Verwenden Sie den Filter, wenn zu viele Ressourcengruppen aufgeführt werden. Dadurch wird die Ressourcengruppe auf einem neuen Blatt geöffnet.
4. Auf der Kachel **Ressourcen** sollten das Standardspeicherkonto und die Data Factory aufgeführt sein, wenn Sie die Ressourcengruppe nicht für andere Projekte freigegeben haben.
5. Klicken Sie oben auf dem Blatt auf **Löschen**. Auf diese Weise werden das Speicherkonto und die Daten im Speicherkonto gelöscht.
6. Geben Sie den Namen der Ressourcengruppe ein, um den Löschvorgang zu bestätigen, und klicken Sie dann auf **Löschen**.

Falls Sie beim Löschen der Ressourcengruppe das Speicherkonto nicht löschen möchten, können Sie die folgende Architektur in Betracht ziehen und die Geschäftsdaten vom Standardspeicherkonto trennen. In diesem Fall verfügen Sie über eine Ressourcengruppe für das Speicherkonto mit den Geschäftsdaten und eine andere Ressourcengruppe für das Standardspeicherkonto für Ihren mit HDInsight verknüpften Dienst und die Data Factory. Wenn Sie die zweite Ressourcengruppe löschen, hat dies keinen Einfluss auf das Speicherkonto für Geschäftsdaten. Gehen Sie dazu wie folgt vor:

* Fügen Sie der Ressourcengruppe der obersten Ebene Folgendes zu, zusammen mit der Ressource „Microsoft.DataFactory/datafactories“ in der Resource Manager-Vorlage. Sie erstellt ein Speicherkonto:

    ```json
    {
        "name": "[parameters('defaultStorageAccountName')]",
        "type": "Microsoft.Storage/storageAccounts",
        "location": "[parameters('location')]",
        "apiVersion": "[variables('defaultApiVersion')]",
        "dependsOn": [ ],
        "tags": {

        },
        "properties": {
            "accountType": "Standard_LRS"
        }
    },
    ```
* Fügen Sie dem neuen Speicherkonto einen neuen verknüpften Dienstpunkt hinzu:

    ```json
    {
        "dependsOn": [ "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]" ],
        "type": "linkedservices",
        "name": "[variables('defaultStorageLinkedServiceName')]",
        "apiVersion": "[variables('apiVersion')]",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": "[concat('DefaultEndpointsProtocol=https;AccountName=',parameters('defaultStorageAccountName'),';AccountKey=',listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('defaultStorageAccountName')), variables('defaultApiVersion')).key1)]"
            }
        }
    },
    ```
* Konfigurieren Sie den bedarfsgesteuerten verknüpften HDInsight-Dienst mit zusätzlichen dependsOn- und additionalLinkedServiceNames-Elementen:

    ```json
    {
        "dependsOn": [
            "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]",
            "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedservices/', variables('defaultStorageLinkedServiceName'))]",
            "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedservices/', variables('storageLinkedServiceName'))]"

        ],
        "type": "linkedservices",
        "name": "[variables('hdInsightOnDemandLinkedServiceName')]",
        "apiVersion": "[variables('apiVersion')]",
        "properties": {
            "type": "HDInsightOnDemand",
            "typeProperties": {
                "osType": "linux",
                "version": "3.2",
                "clusterSize": 1,
                "sshUserName": "myuser",                            
                "sshPassword": "MyPassword!",
                "timeToLive": "00:30:00",
                "linkedServiceName": "[variables('storageLinkedServiceName')]",
                "additionalLinkedServiceNames": "[variables('defaultStorageLinkedServiceName')]"
            }
        }
    },            
    ```
## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie gelernt, wie Sie mit Azure Data Factory einen bedarfsgesteuerten HDInsight-Cluster zum Verarbeiten von Hive-Aufträgen erstellen. Weitere Informationen:

* [Hadoop-Tutorial: Erste Schritte bei der Verwendung von Linux-basiertem Hadoop in HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md)
* [Erstellen von Linux-basierten Hadoop-Clustern in HDInsight](hdinsight-hadoop-provision-linux-clusters.md)
* [HDInsight-Dokumentation](https://azure.microsoft.com/documentation/services/hdinsight/)
* [Data Factory-Dokumentation](https://azure.microsoft.com/documentation/services/data-factory/)

## <a name="appendix"></a>Anhang

### <a name="azure-cli-script"></a>Azure CLI-Skript
Sie können das Tutorial mit der Azure CLI anstelle von Azure PowerShell absolvieren. Zur Verwendung der Azure CLI installieren Sie die Azure CLI zuerst anhand folgender Anweisungen:[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

#### <a name="use-azure-cli-to-prepare-the-storage-and-copy-the-files"></a>Verwenden der Azure CLI zum Vorbereiten des Speichers und Kopieren der Dateien

```
azure login

azure config mode arm

azure group create --name "<Azure Resource Group Name>" --location "East US 2"

azure storage account create --resource-group "<Azure Resource Group Name>" --location "East US 2" --type "LRS" <Azure Storage Account Name>

azure storage account keys list --resource-group "<Azure Resource Group Name>" "<Azure Storage Account Name>"
azure storage container create "adfgetstarted" --account-name "<Azure Storage AccountName>" --account-key "<Azure Storage Account Key>"

azure storage blob copy start "https://hditutorialdata.blob.core.windows.net/adfhiveactivity/inputdata/input.log" --dest-account-name "<Azure Storage Account Name>" --dest-account-key "<Azure Storage Account Key>" --dest-container "adfgetstarted"
azure storage blob copy start "https://hditutorialdata.blob.core.windows.net/adfhiveactivity/script/partitionweblogs.hql" --dest-account-name "<Azure Storage Account Name>" --dest-account-key "<Azure Storage Account Key>" --dest-container "adfgetstarted"
```

Der Containername lautet *adfgetstarted*. Ändern Sie ihn nicht. Andernfalls müssen Sie die Resource Manager-Vorlage aktualisieren. Wenn Sie Hilfe mit diesem Skript der Befehlszeilenschnittstelle benötigen, lesen Sie [Verwenden der Azure-Befehlszeilenschnittstelle mit Azure Storage](../storage/storage-azure-cli.md).


