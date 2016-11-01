<properties
   pageTitle="Erste Schritte mit Azure Batch PowerShell | Microsoft Azure"
   description="Schnelle Einführung in die Azure PowerShell-Cmdlets zum Verwalten des Azure Batch-Diensts"
   services="batch"
   documentationCenter=""
   authors="dlepow"
   manager="timlt"
   editor=""/>

<tags
   ms.service="batch"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="powershell"
   ms.workload="big-compute"
   ms.date="07/28/2016"
   ms.author="danlep"/>

# Erste Schritte mit Azure Batch-PowerShell-Cmdlets
Mit den Azure Batch-PowerShell-Cmdlets können Sie viele Aufgaben durchführen, für die Sie die Batch-APIs, das Azure-Portal und die Azure Befehlszeilenschnittstelle (CLI) verwenden, und Skripts dafür erstellen. Dies ist eine kurze Einführung in die Cmdlets, mit denen Sie Ihre Batch-Konten verwalten und Batch-Ressourcen wie Pools, Aufträge und Aufgaben verwenden. Dieser Artikel basiert auf Cmdlets in Azure PowerShell Version 1.6.0.

Eine vollständige Liste mit Batch-Cmdlets und deren ausführliche Cmdlet-Syntax finden Sie in der Referenz zu [Azure Batch Cmdlets](https://msdn.microsoft.com/library/azure/mt125957.aspx) (Azure Batch-Cmdlets).


## Voraussetzungen

* **Azure PowerShell**: Eine Anleitung zum Herunterladen und Installieren von Azure PowerShell finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).
   
    * Da die Azure Batch-Cmdlets Teil des Azure-Ressourcen-Manager-Moduls sind, müssen Sie das Cmdlet **Login-AzureRmAccount** ausführen, um eine Verbindung mit Ihrem Abonnement herzustellen.
    
    * Wir empfehlen, dass Sie Azure PowerShell regelmäßig aktualisieren, damit Sie von den Dienstupdates und Verbesserungen profitieren.
    
* **Registrieren beim Batch-Anbieternamespace (einmalig)**: Bevor Sie mit Batch-Konten arbeiten können, müssen Sie sich beim Batch-Anbieternamespace registrieren. Dieser Vorgang muss nur einmal pro Abonnement ausgeführt werden. Führen Sie das folgende Cmdlet aus:

        Register-AzureRMResourceProvider -ProviderNamespace Microsoft.Batch


## Verwalten von Batch-Konten und Schlüsseln

### Erstellen eines Batch-Kontos

**New-AzureRmBatchAccount** erstellt ein neues Batch-Konto in einer angegebenen Ressourcengruppe. Wenn Sie noch keine Ressourcengruppe erstellt haben, erstellen Sie sie durch Ausführen des Cmdlets [New-AzureRmResourceGroup](https://msdn.microsoft.com/library/azure/mt603739.aspx). Geben Sie dabei eine der Azure-Regionen im **Location**-Parameter an, z. B. „Central US“. Zum Beispiel:


    New-AzureRmResourceGroup –Name MyBatchResourceGroup –location "Central US"


Erstellen Sie anschließend ein neues Batch-Konto in der Ressourcengruppe, und geben Sie einen Namen für das Konto in <*account\_name*> und den Speicherort und Namen der Ressourcengruppe an. Die Erstellung des Batch-Kontos kann einige Zeit dauern. Zum Beispiel:


    New-AzureRmBatchAccount –AccountName <account_name> –Location "Central US" –ResourceGroupName MyBatchResourceGroup

> [AZURE.NOTE] Der Batch-Kontoname muss in der Azure-Region für die Ressourcengruppe eindeutig sein, zwischen 3 und 24 Zeichen umfassen und darf nur Kleinbuchstaben und Zahlen enthalten.

### Abrufen von Kontozugriffsschlüsseln
**Get-AzureRmBatchAccountKeys** zeigt die Zugriffsschlüssel an, die einem Azure Batch-Konto zugeordnet sind. Führen Sie beispielsweise Folgendes aus, um den primären und sekundären Schlüssel des Kontos abzurufen, das Sie erstellt haben.

    $Account = Get-AzureRmBatchAccountKeys –AccountName <accountname>

    $Account.PrimaryAccountKey

    $Account.SecondaryAccountKey


### Generieren eines neuen Zugriffsschlüssels
**New-AzureRmBatchAccountKey** generiert einen neuen primären oder sekundären Kontoschlüssel für ein Azure Batch-Konto. Geben Sie zum Generieren eines neuen primären Schlüssels für Ihr Batch-Konto z. B. Folgendes ein:


    New-AzureRmBatchAccountKey -AccountName <account_name> -KeyType Primary


> [AZURE.NOTE] Um einen neuen sekundären Schlüssel zu generieren, geben Sie "Secondary" für den **KeyType**-Parameter ein. Sie müssen den primären und sekundären Schlüssel separat neu generieren.

### Löschen eines Batch-Kontos
**Remove-AzureRmBatchAccount** löscht ein Batch-Konto. Zum Beispiel:


    Remove-AzureRmBatchAccount -AccountName <account_name>

Bestätigen Sie bei der entsprechenden Aufforderung, dass Sie das Konto entfernen möchten. Das Entfernen des Kontos kann einige Zeit in Anspruch nehmen.

## Erstellen eines BatchAccountContext-Objekts

Zum Authentifizieren mithilfe von Batch PowerShell-Cmdlets beim Erstellen und Verwalten von Batch-Pools, Aufträgen, Aufgaben und anderen Ressourcen müssen Sie zuerst ein BatchAccountContext-Objekt erstellen. Mit diesem Objekt werden Ihr Kontoname und die Schlüssel gespeichert:

    $context = Get-AzureRmBatchAccountKeys -AccountName <account_name>

Übergeben Sie das BatchAccountContext-Objekt in Cmdlets, die den Parameter **BatchContext** verwenden.

> [AZURE.NOTE] Standardmäßig wird der primäre Schlüssel des Kontos für die Authentifizierung verwendet. Sie können jedoch ausdrücklich den zu verwendenden Schlüssel auswählen, indem Sie die **KeyInUse**-Eigenschaft des BatchAccountContext-Objekts ändern: `$context.KeyInUse = "Secondary"`.



## Erstellen und Ändern von Batch-Ressourcen
Verwenden Sie Cmdlets wie **New-AzureBatchPool**, **New-AzureBatchJob** und **New-AzureBatchTask**, um unter einem Batch-Konto Ressourcen zu erstellen. Die entsprechenden Cmdlets **Get-** und **Set-** zum Aktualisieren der Eigenschaften vorhandener Ressourcen sind vorhanden sowie Cmdlets vom Typ **Remove-**, um unter einem Batch-Konto Ressourcen zu entfernen.

Bei der Verwendung von vielen dieser Cmdlets müssen Sie zusätzlich zum Übergeben eines BatchContext-Objekts auch Objekte erstellen oder übergeben, die ausführliche Ressourceneinstellungen enthalten. Dies wird im folgenden Beispiel veranschaulicht. In der detaillierten Hilfe für die einzelnen Cmdlets finden Sie weitere Beispiele.

### Batch-Pool erstellen

Beim Erstellen oder Aktualisieren eines Batch-Pools wählen Sie eine Clouddienstkonfiguration oder die Konfiguration eines virtuellen Computers für das Betriebssystem auf den Computeknoten (siehe [Übersicht über Azure Batch-Features](batch-api-basics.md#pool)). Anhand Ihrer Auswahl wird bestimmt, ob die Computeknoten ein Image mit einer [Azure-Gastbetriebssystemversion](../cloud-services/cloud-services-guestos-update-matrix.md#releases) oder mit einem der unterstützten Linux- oder Windows VM-Images im Azure Marketplace erhalten.

Übergeben Sie beim Ausführen von **New-AzureBatchPool** die Betriebssystemeinstellungen in einem PSCloudServiceConfiguration- oder PSVirtualMachineConfiguration-Objekt. Mit dem folgenden Cmdlet wird beispielsweise ein neuer Batch-Pool mit Computeknoten der Größe „Small“ (Klein) in der Clouddienstkonfiguration erstellt, und es wird ein Image der aktuellen Betriebssystemversion der Familie 3 (Windows Server 2012) verwendet. Hier gibt der Parameter **CloudServiceConfiguration** die Variable *$configuration* als PSCloudServiceConfiguration-Objekt an. Mit dem Parameter **BatchContext** wird eine zuvor definierte Variable *$context* als BatchAccountContext-Objekt angegeben.


    $configuration = New-Object -TypeName "Microsoft.Azure.Commands.Batch.Models.PSCloudServiceConfiguration" -ArgumentList @(3,"*")
    
    New-AzureBatchPool -Id "AutoScalePool" -VirtualMachineSize "Small" -CloudServiceConfiguration $configuration -AutoScaleFormula '$TargetDedicated=4;' -BatchContext $context

Die Zielanzahl von Computeknoten im neuen Pool wird mit einer Formel für die automatische Skalierung bestimmt. In diesem Fall lautet die Formel einfach **$TargetDedicated=4** und gibt an, dass die Anzahl von Computeknoten im Pool maximal „4“ beträgt.

## Abfragen von Pools, Aufträgen, Aufgaben und weiteren Details

Verwenden Sie Cmdlets wie **Get-AzureBatchPool**, **Get-AzureBatchJob** und **Get-AzureBatchTask** zum Abfragen von Entitäten, die in einem Batch-Konto erstellt wurden.


### Abfragen von Daten

Verwenden Sie beispielsweise **Get-AzureBatchPools** zum Suchen Ihrer Pools. Damit werden standardmäßig alle Pools unter Ihrem Konto abgefragt, sofern Sie das BatchAccountContext-Objekt bereits in *$context* gespeichert haben:


    Get-AzureBatchPool -BatchContext $context

### Verwenden eines OData-Filters

Mit dem **Filter**-Parameter können Sie einen OData-Filter angeben, um nur bestimmte gewünschte Objekte zu suchen. Sie können beispielsweise alle Pools suchen, deren IDs mit "myPool" beginnen:


    $filter = "startswith(id,'myPool')"

    Get-AzureBatchPool -Filter $filter -BatchContext $context


Diese Methode ist nicht so flexibel wie die Verwendung von "Where-Object" in einer lokalen Pipeline. Die Abfrage wird jedoch direkt an den Batch-Dienst gesendet, sodass die gesamte Filterung auf dem Server erfolgt, was Internetbandbreite einspart.

### Verwenden des Id-Parameters

Eine Alternative zu einem OData-Filter stellt die Verwendung des **Id**-Parameters dar. So führen Sie eine Abfrage für einen bestimmten Pool mit der ID "myPool" aus:


    Get-AzureBatchPool -Id "myPool" -BatchContext $context


Der **Id**-Parameter unterstützt ausschließlich die Suche nach der vollständigen ID, jedoch keine Platzhalter oder Filter im OData-Format.



### Verwenden des MaxCount-Parameters

Jedes Cmdlet gibt standardmäßig bis zu 1.000 Objekte zurück. Wenn dieser Grenzwert erreicht ist, können Sie entweder den Filter weiter eingrenzen, sodass weniger Objekte zurückgegeben werden, oder mit dem **MaxCount**-Parameter explizit einen maximalen Wert festlegen. Beispiel:


    Get-AzureBatchTask -MaxCount 2500 -BatchContext $context

Setzen Sie den **MaxCount**-Parameter auf 0 oder eine negative Zahl, um die Obergrenze zu entfernen.

### Verwenden der Pipeline

Batch-Cmdlets können die PowerShell-Pipeline zum Senden von Daten zwischen Cmdlets nutzen. Dies hat dieselbe Auswirkung wie die Angabe eines Parameters, vereinfacht jedoch das Auflisten mehrerer Entitäten. Der folgende Befehl findet z. B. alle Aufgaben unter Ihrem Konto:


    Get-AzureBatchJob -BatchContext $context | Get-AzureBatchTask -BatchContext $context


## Nächste Schritte
* Die ausführliche Cmdlet-Syntax sowie Beispiele finden Sie in der [Referenz zu Azure Batch-Cmdlets](https://msdn.microsoft.com/library/azure/mt125957.aspx).

* Unter [Effizientes Abfragen des Azure Batch-Diensts](batch-efficient-list-queries.md) finden Sie weitere Informationen dazu, wie Sie die Anzahl der Elemente sowie die Art der Informationen verringern, die bei Abfragen an Batch zurückgegeben werden.

<!---HONumber=AcomDC_0803_2016-->