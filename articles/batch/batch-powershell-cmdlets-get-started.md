---
title: "Erste Schritte mit PowerShell für Azure Batch | Microsoft-Dokumentation"
description: "Schnelle Einführung in die Azure PowerShell-Cmdlets zum Verwalten des Azure Batch-Diensts"
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: 
ms.assetid: f9ad62c5-27bf-4e6b-a5bf-c5f5914e6199
ms.service: batch
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: powershell
ms.workload: big-compute
ms.date: 01/23/2017
ms.author: tamram
translationtype: Human Translation
ms.sourcegitcommit: bf22cd3426e936c8d74377f59443e5e1a6834286
ms.openlocfilehash: 9bd1c2a1f273d2487e4ce9063c286896cc414e42


---
# <a name="get-started-with-azure-batch-powershell-cmdlets"></a>Erste Schritte mit Azure Batch-PowerShell-Cmdlets
Mit den Azure Batch-PowerShell-Cmdlets können Sie viele Aufgaben durchführen, für die Sie die Batch-APIs, das Azure-Portal und die Azure Befehlszeilenschnittstelle (CLI) verwenden, und Skripts dafür erstellen. Dies ist eine kurze Einführung in die Cmdlets, mit denen Sie Ihre Batch-Konten verwalten und Batch-Ressourcen wie Pools, Aufträge und Aufgaben verwenden.

Eine vollständige Liste mit Batch-Cmdlets und deren ausführliche Cmdlet-Syntax finden Sie in der Referenz zu [Azure Batch Cmdlets](https://msdn.microsoft.com/library/azure/mt125957.aspx)(Azure Batch-Cmdlets).

Dieser Artikel basiert auf Cmdlets in Azure PowerShell Version 3.0.0. Wir empfehlen, dass Sie Azure PowerShell regelmäßig aktualisieren, damit Sie von den Dienstupdates und Verbesserungen profitieren.

## <a name="prerequisites"></a>Voraussetzungen
Führen Sie Folgendes durch, um Azure PowerShell zum Verwalten Ihrer Batch-Ressourcen zu verwenden.

* [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs)
* Führen Sie das **Login-AzureRmAccount**-Cmdlet aus, um eine Verbindung mit Ihrem Abonnement herzustellen (die Azure Batch-Cmdlets sind Teil des Azure Resource Manager-Moduls):
  
    `Login-AzureRmAccount`
* **Führen Sie die Registrierung beim Batch-Anbieternamespace durch**. Dieser Vorgang muss nur **einmal pro Abonnement** ausgeführt werden.
  
    `Register-AzureRMResourceProvider -ProviderNamespace Microsoft.Batch`

## <a name="manage-batch-accounts-and-keys"></a>Verwalten von Batch-Konten und Schlüsseln
### <a name="create-a-batch-account"></a>Erstellen eines Batch-Kontos
**New-AzureRmBatchAccount** erstellt ein Batch-Konto in einer angegebenen Ressourcengruppe. Falls Sie noch nicht über eine Ressourcengruppe verfügen, können Sie eine erstellen, indem Sie das [New-AzureRmResourceGroup](https://msdn.microsoft.com/library/azure/mt603739.aspx)-Cmdlet ausführen. Geben Sie eine Azure-Region im Parameter **Standort** an, z.B. „USA, Mitte“. Beispiel:

    New-AzureRmResourceGroup –Name MyBatchResourceGroup –location "Central US"

Erstellen Sie anschließend ein Batch-Konto in der Ressourcengruppe, und geben Sie einen Namen für das Konto in <*account_name*> und den Speicherort und Namen der Ressourcengruppe an. Die Erstellung des Batch-Kontos kann einige Zeit dauern. Zum Beispiel:

    New-AzureRmBatchAccount –AccountName <account_name> –Location "Central US" –ResourceGroupName <res_group_name>

> [!NOTE]
> Der Batch-Kontoname muss in der Azure-Region für die Ressourcengruppe eindeutig sein, zwischen 3 und 24 Zeichen umfassen und darf nur Kleinbuchstaben und Zahlen enthalten.
> 
> 

### <a name="get-account-access-keys"></a>Abrufen von Kontozugriffsschlüsseln
**Get-AzureRmBatchAccountKeys** zeigt die Zugriffsschlüssel an, die einem Azure Batch-Konto zugeordnet sind. Führen Sie beispielsweise Folgendes aus, um den primären und sekundären Schlüssel des Kontos abzurufen, das Sie erstellt haben.

    $Account = Get-AzureRmBatchAccountKeys –AccountName <account_name>

    $Account.PrimaryAccountKey

    $Account.SecondaryAccountKey

### <a name="generate-a-new-access-key"></a>Generieren eines neuen Zugriffsschlüssels
**New-AzureRmBatchAccountKey** generiert einen neuen primären oder sekundären Kontoschlüssel für ein Azure Batch-Konto. Geben Sie zum Generieren eines neuen primären Schlüssels für Ihr Batch-Konto z. B. Folgendes ein:

    New-AzureRmBatchAccountKey -AccountName <account_name> -KeyType Primary

> [!NOTE]
> Um einen neuen sekundären Schlüssel zu generieren, geben Sie "Secondary" für den **KeyType** -Parameter ein. Sie müssen den primären und sekundären Schlüssel separat neu generieren.
> 
> 

### <a name="delete-a-batch-account"></a>Löschen eines Batch-Kontos
**Remove-AzureRmBatchAccount** löscht ein Batch-Konto. Zum Beispiel:

    Remove-AzureRmBatchAccount -AccountName <account_name>

Bestätigen Sie bei der entsprechenden Aufforderung, dass Sie das Konto entfernen möchten. Das Entfernen des Kontos kann einige Zeit in Anspruch nehmen.

## <a name="create-a-batchaccountcontext-object"></a>Erstellen eines BatchAccountContext-Objekts
Zum Authentifizieren mithilfe von Batch PowerShell-Cmdlets beim Erstellen und Verwalten von Batch-Pools, Aufträgen, Aufgaben und anderen Ressourcen müssen Sie zuerst ein BatchAccountContext-Objekt erstellen. Mit diesem Objekt werden Ihr Kontoname und die Schlüssel gespeichert:

    $context = Get-AzureRmBatchAccountKeys -AccountName <account_name>

Übergeben Sie das BatchAccountContext-Objekt in Cmdlets, die den Parameter **BatchContext** verwenden.

> [!NOTE]
> Standardmäßig wird der primäre Schlüssel des Kontos für die Authentifizierung verwendet. Sie können aber ausdrücklich den zu verwendenden Schlüssel auswählen, indem Sie die **KeyInUse**-Eigenschaft des BatchAccountContext-Objekts ändern: `$context.KeyInUse = "Secondary"`.
> 
> 

## <a name="create-and-modify-batch-resources"></a>Erstellen und Ändern von Batch-Ressourcen
Verwenden Sie Cmdlets wie **New-AzureBatchPool**, **New-AzureBatchJob** und **New-AzureBatchTask**, um unter einem Batch-Konto Ressourcen zu erstellen. Die entsprechenden Cmdlets **Get-** und **Set-** zum Aktualisieren der Eigenschaften vorhandener Ressourcen sind vorhanden, sowie auch Cmdlets vom Typ **Remove-**, um unter einem Batch-Konto Ressourcen zu entfernen.

Bei der Verwendung von vielen dieser Cmdlets müssen Sie zusätzlich zum Übergeben eines BatchContext-Objekts auch Objekte erstellen oder übergeben, die ausführliche Ressourceneinstellungen enthalten. Dies wird im folgenden Beispiel veranschaulicht. In der detaillierten Hilfe für die einzelnen Cmdlets finden Sie weitere Beispiele.

### <a name="create-a-batch-pool"></a>Batch-Pool erstellen
Beim Erstellen oder Aktualisieren eines Batch-Pools wählen Sie eine Clouddienstkonfiguration oder die Konfiguration eines virtuellen Computers für das Betriebssystem auf den Computeknoten (siehe [Übersicht über Azure Batch-Features](batch-api-basics.md#pool)). Anhand Ihrer Auswahl wird bestimmt, ob die Computeknoten ein Image mit einer [Azure-Gastbetriebssystemversion](../cloud-services/cloud-services-guestos-update-matrix.md#releases) oder mit einem der unterstützten Linux- oder Windows VM-Images im Azure Marketplace erhalten.

Übergeben Sie beim Ausführen von **New-AzureBatchPool**die Betriebssystemeinstellungen in einem PSCloudServiceConfiguration- oder PSVirtualMachineConfiguration-Objekt. Mit dem folgenden Cmdlet wird beispielsweise ein neuer Batch-Pool mit Computeknoten der Größe „Small“ (Klein) in der Clouddienstkonfiguration erstellt, und es wird ein Image der aktuellen Betriebssystemversion der Familie 3 (Windows Server 2012) verwendet. Hier gibt der Parameter **CloudServiceConfiguration** die Variable *$configuration* als PSCloudServiceConfiguration-Objekt an. Mit dem Parameter **BatchContext** wird eine zuvor definierte Variable *$context* als BatchAccountContext-Objekt angegeben.

    $configuration = New-Object -TypeName "Microsoft.Azure.Commands.Batch.Models.PSCloudServiceConfiguration" -ArgumentList @(4,"*")

    New-AzureBatchPool -Id "AutoScalePool" -VirtualMachineSize "Small" -CloudServiceConfiguration $configuration -AutoScaleFormula '$TargetDedicated=4;' -BatchContext $context

Die Zielanzahl von Computeknoten im neuen Pool wird mit einer Formel für die automatische Skalierung bestimmt. In diesem Fall lautet die Formel einfach **$TargetDedicated=4**und gibt an, dass die Anzahl von Computeknoten im Pool maximal „4“ beträgt.

## <a name="query-for-pools-jobs-tasks-and-other-details"></a>Abfragen von Pools, Aufträgen, Aufgaben und weiteren Details
Verwenden Sie Cmdlets wie **Get-AzureBatchPool**, **Get-AzureBatchJob** und **Get-AzureBatchTask** zum Abfragen von Entitäten, die in einem Batch-Konto erstellt wurden.

### <a name="query-for-data"></a>Abfragen von Daten
Verwenden Sie beispielsweise **Get-AzureBatchPools** zum Suchen Ihrer Pools. Damit werden standardmäßig alle Pools unter Ihrem Konto abgefragt, sofern Sie das BatchAccountContext-Objekt bereits in *$context*gespeichert haben:

    Get-AzureBatchPool -BatchContext $context

### <a name="use-an-odata-filter"></a>Verwenden eines OData-Filters
Mit dem **Filter** -Parameter können Sie einen OData-Filter angeben, um nur bestimmte gewünschte Objekte zu suchen. Sie können beispielsweise alle Pools suchen, deren IDs mit "myPool" beginnen:

    $filter = "startswith(id,'myPool')"

    Get-AzureBatchPool -Filter $filter -BatchContext $context

Diese Methode ist nicht so flexibel wie die Verwendung von "Where-Object" in einer lokalen Pipeline. Die Abfrage wird jedoch direkt an den Batch-Dienst gesendet, sodass die gesamte Filterung auf dem Server erfolgt, was Internetbandbreite einspart.

### <a name="use-the-id-parameter"></a>Verwenden des Id-Parameters
Eine Alternative zu einem OData-Filter stellt die Verwendung des **Id** -Parameters dar. So führen Sie eine Abfrage für einen bestimmten Pool mit der ID "myPool" aus:

    Get-AzureBatchPool -Id "myPool" -BatchContext $context

Der **Id** -Parameter unterstützt ausschließlich die Suche nach der vollständigen ID, jedoch keine Platzhalter oder Filter im OData-Format.

### <a name="use-the-maxcount-parameter"></a>Verwenden des MaxCount-Parameters
Jedes Cmdlet gibt standardmäßig bis zu 1.000 Objekte zurück. Wenn dieser Grenzwert erreicht ist, können Sie entweder den Filter weiter eingrenzen, sodass weniger Objekte zurückgegeben werden, oder mit dem **MaxCount** -Parameter explizit einen maximalen Wert festlegen. Zum Beispiel:

    Get-AzureBatchTask -MaxCount 2500 -BatchContext $context

Setzen Sie den **MaxCount** -Parameter auf 0 oder eine negative Zahl, um die Obergrenze zu entfernen.

### <a name="use-the-powershell-pipeline"></a>Verwenden der PowerShell-Pipeline
Batch-Cmdlets können die PowerShell-Pipeline zum Senden von Daten zwischen Cmdlets nutzen. Dies hat die gleiche Auswirkung wie die Angabe eines Parameters, vereinfacht aber die Verwendung mehrerer Entitäten.

Beispielsweise können Sie nach allen Aufgaben Ihres Kontos suchen und diese anzeigen:

    Get-AzureBatchJob -BatchContext $context | Get-AzureBatchTask -BatchContext $context

Starten Sie jeden Computeknoten eines Pools neu:

    Get-AzureBatchComputeNode -PoolId "myPool" -BatchContext $context | Restart-AzureBatchComputeNode -BatchContext $context

## <a name="application-package-management"></a>Anwendungspaketverwaltung
Anwendungspakete ermöglichen eine vereinfachte Anwendungsbereitstellung auf den Computeknoten in Ihren Pools. Mit den Batch PowerShell-Cmdlets können Sie Anwendungspakete in Ihrem Batch-Konto hochladen und verwalten und Paketversionen für Computeknoten bereitstellen.

**Erstellen** einer Anwendung:

    New-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

**Hinzufügen** eines Anwendungspakets:

    New-AzureRmBatchApplicationPackage -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -ApplicationVersion "1.0" -Format zip -FilePath package001.zip

Festlegen der **Standardversion** für die Anwendung:

    Set-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -DefaultVersion "1.0"

**Auflisten** der Pakete einer Anwendung

    $application = Get-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

    $application.ApplicationPackages

**Löschen** eines Anwendungspakets

    Remove-AzureRmBatchApplicationPackage -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -ApplicationVersion "1.0"

**Löschen** einer Anwendung

    Remove-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

> [!NOTE]
> Sie müssen alle Anwendungspaketversionen einer Anwendung löschen, bevor Sie die Anwendung löschen. Sie erhalten einen Konfliktfehler, wenn Sie versuchen, eine Anwendung zu löschen, die derzeit über Anwendungspakete verfügt.
> 
> 

### <a name="deploy-an-application-package"></a>Bereitstellen eines Anwendungspakets
Beim Erstellen eines Pools können Sie Anwendungspakete angeben, die Sie bereitstellen möchten. Wenn Sie ein Paket bei der Poolerstellung angeben, wird es auf den einzelnen Knoten bereitgestellt, sobald diese dem Pool hinzugefügt werden. Pakete werden auch bereitgestellt, wenn ein Knoten neu gestartet oder ein Reimaging für den Knoten durchgeführt wird.

Geben Sie bei der Poolerstellung die `-ApplicationPackageReference`-Option an, um ein Anwendungspaket auf den Knoten des Pools bereitzustellen, wenn diese dem Pool hinzugefügt werden. Erstellen Sie zunächst ein **PSApplicationPackageReference**-Objekt, und konfigurieren Sie es mit der Anwendungs-ID und Paketversion, die Sie auf den Computeknoten des Pools bereitstellen möchten:

    $appPackageReference = New-Object Microsoft.Azure.Commands.Batch.Models.PSApplicationPackageReference

    $appPackageReference.ApplicationId = "MyBatchApplication"

    $appPackageReference.Version = "1.0"

Erstellen Sie nun den Pool, und geben Sie das Paketverweisobjekt als Argument für die Option `ApplicationPackageReferences` an:

    New-AzureBatchPool -Id "PoolWithAppPackage" -VirtualMachineSize "Small" -CloudServiceConfiguration $configuration -BatchContext $context -ApplicationPackageReferences $appPackageReference

Weitere Informationen zu Anwendungspaketen finden Sie unter [Anwendungsbereitstellung mit Azure Batch-Anwendungspaketen](batch-application-packages.md).

> [!IMPORTANT]
> Damit Sie Anwendungspakete verwenden können, müssen Sie zunächst ein [Azure Storage-Konto mit Ihrem Batch-Konto verknüpfen](#linked-storage-account-autostorage) .
> 
> 

### <a name="update-a-pools-application-packages"></a>Aktualisieren der Anwendungspakete eines Pools
Erstellen Sie zum Aktualisieren der Anwendungen, die einem vorhandenen Pool zugewiesen sind, zuerst ein PSApplicationPackageReference-Objekt mit den gewünschten Eigenschaften (Anwendungs-ID und Paketversion):

    $appPackageReference = New-Object Microsoft.Azure.Commands.Batch.Models.PSApplicationPackageReference

    $appPackageReference.ApplicationId = "MyBatchApplication"

    $appPackageReference.Version = "2.0"

Rufen Sie als Nächstes den Pool aus Batch ab, löschen Sie alle vorhandenen Pakete, fügen Sie Ihren neuen Paketverweis hinzu, und aktualisieren Sie den Batch-Dienst mit den neuen Pooleinstellungen:

    $pool = Get-AzureBatchPool -BatchContext $context -Id "PoolWithAppPackage"

    $pool.ApplicationPackageReferences.Clear()

    $pool.ApplicationPackageReferences.Add($appPackageReference)

    Set-AzureBatchPool -BatchContext $context -Pool $pool

Sie haben die Eigenschaften des Pools jetzt im Batch-Dienst aktualisiert. Um das neue Anwendungspaket tatsächlich auf den Computeknoten des Pools bereitzustellen, müssen Sie diese Knoten aber neu starten oder ein Reimaging dafür durchführen. Sie können jeden Knoten eines Pools mit diesem Befehl neu starten:

    Get-AzureBatchComputeNode -PoolId "PoolWithAppPackage" -BatchContext $context | Restart-AzureBatchComputeNode -BatchContext $context

> [!TIP]
> Sie können für die Computeknoten eines Pools mehrere Anwendungspakete bereitstellen. Wenn Sie ein Anwendungspaket *hinzufügen* möchten, anstatt die derzeit bereitgestellten Pakete zu ersetzen, lassen Sie im obigen Code einfach die Zeile `$pool.ApplicationPackageReferences.Clear()` weg.
> 
> 

## <a name="next-steps"></a>Nächste Schritte
* Die ausführliche Cmdlet-Syntax sowie Beispiele finden Sie in der [Referenz zu Azure Batch-Cmdlets](https://msdn.microsoft.com/library/azure/mt125957.aspx).
* Weitere Informationen zu Anwendungen und Anwendungspaketen unter Batch finden Sie unter [Anwendungsbereitstellung mit Azure Batch-Anwendungspaketen](batch-application-packages.md).




<!--HONumber=Jan17_HO4-->


