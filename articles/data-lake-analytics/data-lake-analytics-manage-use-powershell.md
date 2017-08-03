---
title: Verwalten von Azure Data Lake Analytics mithilfe von Azure PowerShell | Microsoft Docs
description: "Erfahren Sie, wie Sie Konten, Datenquellen, Aufträge und Katalogelemente in Data Lake Analytics verwalten. "
services: data-lake-analytics
documentationcenter: 
author: matt1883
manager: jhubbard
editor: cgronlun
ms.assetid: ad14d53c-fed4-478d-ab4b-6d2e14ff2097
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/26/2017
ms.author: mahi
ms.translationtype: HT
ms.sourcegitcommit: c3ea7cfba9fbf1064e2bd58344a7a00dc81eb148
ms.openlocfilehash: d4e4bb5e18b63a9d9494a2294743fa9f45b64fa1
ms.contentlocale: de-de
ms.lasthandoff: 07/19/2017

---
# <a name="manage-azure-data-lake-analytics-using-azure-powershell"></a>Verwalten von Azure Data Lake Analytics mithilfe von Azure PowerShell
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Erfahren Sie, wie Sie Konten, Datenquellen, Aufträge und Katalogelemente in Azure Data Lake Analytics mithilfe von Azure PowerShell verwalten. 

## <a name="prerequisites"></a>Voraussetzungen

Bei der Erstellung eines Data Lake Analytics-Kontos müssen Sie Folgendes kennen:

* **Abonnement-ID**: die Azure-Abonnement-ID, zu der Ihr Data Lake Analytics-Konto gehört
* **Ressourcengruppe**: der Name der Azure-Ressourcengruppe, die Ihr Data Lake Analytics-Konto enthält
* **Data Lake Analytics-Kontoname**: Der Kontoname darf nur Kleinbuchstaben und Zahlen enthalten.
* **Data Lake Store-Standardkonto**: Jedes Data Lake Analytics-Konto verfügt über ein Data Lake Store-Standardkonto. Diese Konten müssen sich an demselben Standort befinden.
* **Standort**: der Standort des Data Lake Analytics-Kontos, z.B. „USA, Osten 2“ oder andere unterstützte Standorte. Die unterstützten Standorte finden Sie auf der [Preisseite](https://azure.microsoft.com/pricing/details/data-lake-analytics/).

In den PowerShell-Codeausschnitten dieses Tutorials werden die folgenden Variablen zum Speichern dieser Informationen verwendet:

```powershell
$subId = "<SubscriptionId>"
$rg = "<ResourceGroupName>"
$adla = "<DataLakeAnalyticsAccountName>"
$adls = "<DataLakeStoreAccountName>"
$location = "<Location>"
```

## <a name="log-in"></a>Anmelden

Melden Sie sich mit einer Abonnement-ID an.

```powershell
Login-AzureRmAccount -SubscriptionId $subId
```

Melden Sie sich mit einem Abonnementnamen an.

```
Login-AzureRmAccount -SubscriptionName $subname 
```

Das Cmdlet `Login-AzureRmAccount` fordert immer zur Eingabe von Anmeldeinformationen auf. Damit diese Aufforderung nicht erfolgt, können Sie die folgenden Cmdlets verwenden:

```powershell
# Save login session information
Save-AzureRmProfile -Path D:\profile.json  

# Load login session information
Select-AzureRmProfile -Path D:\profile.json 
```

## <a name="managing-accounts"></a>Verwalten von Konten

### <a name="create-a-data-lake-analytics-account"></a>Erstellen eines Data Lake Analytics-Kontos

Wenn Sie nicht bereits über eine [Ressourcengruppe](../azure-resource-manager/resource-group-overview.md#resource-groups) verfügen, müssen Sie eine erstellen. 

```powershell
New-AzureRmResourceGroup -Name  $rg -Location $location
```

Für jedes Data Lake Analytics-Konto ist ein Data Lake Store-Standardkonto erforderlich, das zum Speichern von Protokollen verwendet wird. Sie können ein vorhandenes Konto wiederverwenden oder ein Konto erstellen. 

```powershell
New-AdlStore -ResourceGroupName $rg -Name $adls -Location $location
```

Wenn eine Ressourcengruppe und ein Data Lake Store-Konto vorhanden sind, können Sie ein Data Lake Analytics-Konto erstellen.

```powershell
New-AdlAnalyticsAccount -ResourceGroupName $rg -Name $adla -Location $location -DefaultDataLake $adls
```

### <a name="get-information-about-an-account"></a>Abrufen von Informationen zu einem Konto

Rufen Sie Details zu einem Konto ab.

```powershell
Get-AdlAnalyticsAccount -Name $adla
```

Überprüfen Sie, ob ein bestimmtes Data Lake Analytics-Konto vorhanden ist. Das Cmdlet gibt entweder `True` oder `False` zurück.

```powershell
Test-AdlAnalyticsAccount -Name $adla
```

Überprüfen Sie, ob ein bestimmtes Data Lake Store-Konto vorhanden ist. Das Cmdlet gibt entweder `True` oder `False` zurück.

```powershell
Test-AdlStoreAccount -Name $adls
```

### <a name="listing-accounts"></a>Auflisten von Konten

Listen Sie Data Lake Analytics-Konten im aktuellen Abonnement auf.

```powershell
Get-AdlAnalyticsAccount
```

Listen Sie Data Lake Analytics-Konten in einer bestimmten Ressourcengruppe auf.

```powershell
Get-AdlAnalyticsAccount -ResourceGroupName $rg
```

## <a name="managing-firewall-rules"></a>Verwalten von Firewallregeln

### <a name="add-or-remove-firewall-rules"></a>Hinzufügen oder Entfernen von Firewallregeln

Listen Sie Firewallregeln auf.

```powershell
Get-AdlAnalyticsFirewallRule -Account $adla
```

Fügen Sie eine Firewallregel hinzu.

```powershell
$ruleName = "Allow access from on-prem server"
$startIpAddress = "<start IP address>"
$endIpAddress = "<end IP address>"

Add-AdlAnalyticsFirewallRule -Account $adla -Name $ruleName -StartIpAddress $startIpAddress -EndIpAddress $endIpAddress
```

Ändern Sie eine Firewallregel.

```powershell
Set-AdlAnalyticsFirewallRule -Account $adla -Name $ruleName -StartIpAddress $startIpAddress -EndIpAddress $endIpAddress
```

Entfernen Sie eine Firewallregel.

```powershell
Remove-AdlAnalyticsFirewallRule -Account $adla -Name $ruleName
```

### <a name="enable-or-disable-firewall-rules"></a>Aktivieren oder Deaktivieren von Firewallregeln

Aktivieren Sie Firewallregeln.

```powershell
Set-AdlAnalyticsAccount -Name $adla -FirewallState Enabled
```

Lassen Sie Azure-IP-Adressen zu.

```powershell
Set-AdlAnalyticsAccount -Name $adla -AllowAzureIpState Enabled
```

Deaktivieren Sie Firewallregeln.

```powershell
Set-AdlAnalyticsAccount -Name $adla -FirewallState Disabled
```

## <a name="managing-data-sources"></a>Verwalten von Datenquellen
Azure Data Lake Analytics unterstützt derzeit die folgenden Datenquellen:

* [Azure Data Lake-Speicher](../data-lake-store/data-lake-store-overview.md)
* [Azure Storage (in englischer Sprache)](../storage/storage-introduction.md)

Beim Erstellen eines Analytics-Kontos müssen Sie ein Data Lake Store-Konto als Standarddatenquelle festlegen. Das Data Lake-Standardspeicherkonto dient zum Speichern von Auftragsmetadaten und -überwachungsprotokollen. Nachdem Sie ein Data Lake Analytics-Konto erstellt haben, können Sie zusätzliche Data Lake Store-Konten und/oder Speicherkonten hinzufügen. 

### <a name="find-the-default-data-lake-store-account"></a>Ermitteln des Data Lake-Standardspeicherkontos

```powershell
$adla_acct = Get-AdlAnalyticsAccount -Name $adla
$dataLakeStoreName = $adla_acct.DefaultDataLakeAccount
```

Alternativ beim Aufstellen der Liste mit Datenquellen können Sie das Data Lake Store-Standardkonto mithilfe des folgenden Musters ermitteln:

```powershell
Get-AdlAnalyticsDataSource -Account $adla  | ? { $_.IsDefault } 
```

### <a name="add-data-sources"></a>Hinzufügen von Datenquellen

Fügen Sie ein zusätzliches Storage (Blob)-Konto hinzu.

```powershell
$AzureStorageAccountName = "<AzureStorageAccountName>"
$AzureStorageAccountKey = "<AzureStorageAccountKey>"

Add-AdlAnalyticsDataSource -Account $adla -Blob $AzureStorageAccountName -AccessKey $AzureStorageAccountKey
```

Fügen Sie ein zusätzliches Data Lake Store-Konto hinzu.

```powershell
$AzureDataLakeStoreName = "<AzureDataLakeStoreAccountName"
Add-AdlAnalyticsDataSource -Account $adla -DataLakeStore $AzureDataLakeStoreName 
```

### <a name="list-data-sources"></a>Auflisten von Datenquellen

```powershell
# List all the data sources
Get-AdlAnalyticsDataSource -Name $adla

# List attached Data Lake Store accounts
Get-AdlAnalyticsDataSource -Name $adla | where -Property Type -EQ "DataLakeStore"

# List attached Storage accounts
Get-AdlAnalyticsDataSource -Name $adla | where -Property Type -EQ "Blob"
```

## <a name="managing-jobs"></a>Verwalten von Aufträgen

### <a name="submit-a-u-sql-job"></a>Senden eines U-SQL-Auftrags

Senden Sie eine Zeichenfolge als U-SQL-Skript.

```powershell
$script = @"
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS D( customer, amount );
OUTPUT @a
    TO "/data.csv"
    USING Outputters.Csv();
"@

$scriptpath = "d:\test.usql"
$script | Out-File $scriptpath 

Submit-AdlJob -AccountName $adla -Script $script -Name "Demo"
```


Senden Sie eine Datei als U-SQL-Skript.

```powershell
$scriptpath = "d:\test.usql"
$script | Out-File $scriptpath 
Submit-AdlJob -AccountName $adla –ScriptPath $scriptpath -Name "Demo"
```

### <a name="list-jobs"></a>Auflisten von Aufträgen

Listen Sie alle Aufträge des Kontos auf. Die Ausgabe enthält die derzeit ausgeführten Aufträge sowie die Aufträge, die vor Kurzem abgeschlossen wurden.

```powershell
Get-AdlJob -Account $adla
```

### <a name="list-a-specific-number-of-jobs"></a>Auflisten einer bestimmten Anzahl von Aufträgen

Die Liste der Aufträge wird standardmäßig nach der Übermittlungszeit sortiert. Die zuletzt gesendeten Aufträge werden somit zuerst angezeigt. Im ADLA-Konto werden Aufträge standardmäßig 180 Tage lang beibehalten, mit dem Cmdlet „Get-AdlJob“ werden jedoch nur die ersten 500 Aufträge zurückgegeben. Verwenden Sie den Parameter „-Top“, um eine bestimmte Anzahl von Aufträgen aufzulisten.

```powershell
$jobs = Get-AdlJob -Account $adla -Top 10
```

### <a name="list-jobs-based-on-the-value-of-job-property"></a>Auflisten von Aufträgen basierend auf dem Wert der job-Eigenschaft

Listen Sie Aufträge auf, die am letzten Tag übermittelt wurden.

```
$d = [DateTime]::Now.AddDays(-1)
Get-AdlJob -Account $adla -SubmittedAfter $d
```

Listen Sie Aufträge auf, die in den letzten fünf Tagen übermittelt und erfolgreich abgeschlossen wurden.

```
$d = (Get-Date).AddDays(-5)
Get-AdlJob -Account $adla -SubmittedAfter $d -State Ended -Result Succeeded
```

Listen Sie erfolgreiche Vorgänge auf.

```
Get-AdlJob -Account $adla -State Ended -Result Succeeded
```

Listen Sie fehlerhaften Aufträge auf.

```powershell
Get-AdlJob -Account $adla -State Ended -Result Failed
```

Listen Sie alle fehlerhaften Aufträge auf, die in den letzten sieben Tagen von joe@contoso.com übermittelt wurden.

```powershell
Get-AdlJob -Account $adla `
    -Submitter "joe@contoso.com" `
    -SubmittedAfter (Get-Date).AddDays(-7) `
    -Result Failed
```

### <a name="filtering-a-list-of-jobs"></a>Filtern einer Liste von Aufträgen

Nachdem Sie eine Liste von Aufträgen in der aktuellen PowerShell-Sitzung erstellt haben, können Sie sie mit normalen PowerShell-Cmdlets filtern.

Filtern Sie eine Liste von Aufträgen nach den Aufträgen, die in den letzten 24 Stunden übermittelt wurden.

```
$upperdate = Get-Date
$lowerdate = $upperdate.AddHours(-24)
$jobs | Where-Object { $_.EndTime -ge $lowerdate }
```

Filtern Sie eine Liste von Aufträgen nach den Aufträgen, die in den letzten 24 Stunden beendet wurden.

```
$upperdate = Get-Date
$lowerdate = $upperdate.AddHours(-24)
$jobs | Where-Object { $_.SubmitTime -ge $lowerdate }
```

Filtern Sie eine Liste von Aufträgen nach den gestarteten Aufträgen. Ein Auftrag kann zum Kompilierzeitpunkt einen Fehler verursachen – und wird daher nicht gestartet. Sehen Sie sich die fehlerhaften Aufträge an, die tatsächlich gestartet wurden und dann zu Fehlern führten.

```powershell
$jobs | Where-Object { $_.StartTime -ne $null }
```

### <a name="analyzing-a-list-of-jobs"></a>Analysieren einer Liste von Aufträgen

Verwenden Sie das Cmdlet `Group-Object`, um eine Liste von Aufträgen zu analysieren.

```
# Count the number of jobs by Submitter
$jobs | Group-Object Submitter | Select -Property Count,Name

# Count the number of jobs by Result
$jobs | Group-Object Result | Select -Property Count,Name

# Count the number of jobs by State
$jobs | Group-Object State | Select -Property Count,Name

#  Count the number of jobs by DegreeOfParallelism
$jobs | Group-Object DegreeOfParallelism | Select -Property Count,Name
```
Beim Durchführen einer Analyse kann es nützlich sein, den Job-Objekten Eigenschaften hinzuzufügen, um so das Filtern und Gruppieren zu vereinfachen. Im folgenden Codeausschnitt wird gezeigt, wie eine JobInfo mit berechneten Eigenschaften versehen wird.

```
function annotate_job( $j )
{
    $dic1 = @{
        Label='AUHours';
        Expression={ ($_.DegreeOfParallelism * ($_.EndTime-$_.StartTime).TotalHours)}}
    $dic2 = @{
        Label='DurationSeconds';
        Expression={ ($_.EndTime-$_.StartTime).TotalSeconds}}
    $dic3 = @{
        Label='DidRun';
        Expression={ ($_.StartTime -ne $null)}}

    $j2 = $j | select *, $dic1, $dic2, $dic3
    $j2
}

$jobs = Get-AdlJob -Account $adla -Top 10
$jobs = $jobs | %{ annotate_job( $_ ) }
```

### <a name="get-information-about-a-job"></a>Abrufen von Informationen zu einem Auftrag

Rufen Sie den Status eines bestimmten Auftrags ab.

```powershell
Get-AdlJob -AccountName $adla -JobId $job.JobId
```

Anstatt `Get-AdlAnalyticsJob` zu wiederholen, bis ein Auftrag abgeschlossen ist, können Sie das Cmdlet `Wait-AdlJob` verwenden, um zu warten, bis der Auftrag beendet wurde.

```powershell
Wait-AdlJob -Account $adla -JobId $job.JobId
```

### <a name="examine-the-job-outputs"></a>Überprüfen der Auftragsausgaben

Überprüfen Sie nach Beendigung des Auftrags, ob die Ausgabedatei vorhanden ist, indem Sie die Dateien eines Ordners auflisten.

```powershell
Get-AdlStoreChildItem -Account $adls -Path "/"
```

Überprüfen Sie das Vorhandensein einer Datei.

```powershell
Test-AdlStoreItem -Account $adls -Path "/data.csv"
```

### <a name="cancel-a-job"></a>Abbrechen eines Auftrags

```powershell
Stop-AdlJob -Account $adls -JobID $jobID
```

## <a name="uploading-and-downloading"></a>Hoch- und Herunterladen

Laden Sie eine Datei hoch.

```powershell
Import-AdlStoreItem -AccountName $adls -Path "c:\data.tsv" -Destination "/data_copy.csv" 
```

Laden Sie einen gesamten Ordner rekursiv hoch.

```powershell
Import-AdlStoreItem -AccountName $adls -Path "c:\myData\" -Destination "/myData/" -Recurse
```

Laden Sie eine Datei herunter.

```powershell
Export-AdlStoreItem -AccountName $adls -Path "/data.csv" -Destination "c:\data.csv"
```

Laden Sie einen gesamten Ordner rekursiv herunter.

```powershell
Export-AdlStoreItem -AccountName $adls -Path "/" -Destination "c:\myData\" -Recurse
```

> [!NOTE]
> Wenn der Upload- oder Downloadvorgang unterbrochen wird, können Sie versuchen, den Vorgang fortzusetzen, indem Sie das Cmdlet mit dem Flag ``-Resume`` erneut ausführen.

## <a name="manage-catalog-items"></a>Verwalten von Katalogelemente
Der U-SQL-Katalog wird zum Strukturieren von Daten und Code verwendet, damit diese von U-SQL-Skripts gemeinsam genutzt werden können. Der Katalog ermöglicht die höchstmögliche Leistung mit Daten in Azure Data Lake. Weitere Informationen finden Sie unter [Verwenden des U-SQL-Katalogs](data-lake-analytics-use-u-sql-catalog.md).

### <a name="list-items-in-the-u-sql-catalog"></a>Auflisten von Elementen im U-SQL-Katalog


```powershell
# List U-SQL databases
Get-AdlCatalogItem -Account $adla -ItemType Database 

# List tables within a database
Get-AdlCatalogItem -Account $adla -ItemType Table -Path "database"

# List tables within a schema.
Get-AdlCatalogItem -Account $adla -ItemType Table -Path "database.schema"
```

Listen Sie alle Assemblys in allen Datenbanken in einem ADLA-Konto auf.

```powershell
$dbs = Get-AdlCatalogItem -Account $adla -ItemType Database

foreach ($db in $dbs)
{
    $asms = Get-AdlCatalogItem -Account $adla -ItemType Assembly -Path $db.Name

    foreach ($asm in $asms)
    {
        $asmname = "[" + $db.Name + "].[" + $asm.Name + "]"
        Write-Host $asmname
    }
}
```

### <a name="get-details-about-a-catalog-item"></a>Abrufen von Details zu einem Katalogelement

```powershell
# Get details of a table
Get-AdlCatalogItem  -Account $adla -ItemType Table -Path "master.dbo.mytable"

# Test existence of a U-SQL database.
Test-AdlCatalogItem  -Account $adla -ItemType Database -Path "master"
```

### <a name="create-credentials-in-a-catalog"></a>Erstellen von Anmeldeinformationen in einem Katalog

Erstellen Sie innerhalb einer U-SQL-Datenbank ein credential-Objekt für eine in Azure gehostete Datenbank. Derzeit können über PowerShell lediglich U-SQL-Anmeldeinformationen als Katalogelement erstellt werden.

```powershell
$dbName = "master"
$credentialName = "ContosoDbCreds"
$dbUri = "https://contoso.database.windows.net:8080"

New-AdlCatalogCredential -AccountName $adla `
          -DatabaseName $db `
          -CredentialName $credentialName `
          -Credential (Get-Credential) `
          -Uri $dbUri
```

### <a name="get-basic-information-about-an-adla-account"></a>Abrufen grundlegender Informationen zu einem ADLA-Konto

Nach Eingabe eines Kontonamens werden mit dem folgenden Code grundlegende Informationen zu dem Account abgerufen.

```
$adla_acct = Get-AdlAnalyticsAccount -Name "saveenrdemoadla"
$adla_name = $adla_acct.Name
$adla_subid = $adla_acct.Id.Split("/")[2]
$adla_sub = Get-AzureRmSubscription -SubscriptionId $adla_subid
$adla_subname = $adla_sub.Name
$adla_defadls_datasource = Get-AdlAnalyticsDataSource -Account $adla_name  | ? { $_.IsDefault } 
$adla_defadlsname = $adla_defadls_datasource.Name

Write-Host "ADLA Account Name" $adla_name
Write-Host "Subscription Id" $adla_subid
Write-Host "Subscription Name" $adla_subname
Write-Host "Defautl ADLS Store" $adla_defadlsname
Write-Host 

Write-Host '$subname' " = ""$adla_subname"" "
Write-Host '$subid' " = ""$adla_subid"" "
Write-Host '$adla' " = ""$adla_name"" "
Write-Host '$adls' " = ""$adla_defadlsname"" "
```
## <a name="working-with-azure"></a>Arbeiten mit Azure

### <a name="get-details-of-azurerm-errors"></a>Abrufen von Details zu AzureRm-Fehlern

```powershell
Resolve-AzureRmError -Last
```

### <a name="verify-if-you-are-running-as-an-administrator"></a>Überprüfen, ob die Ausführung als Administrator erfolgt

```powershell
function Test-Administrator  
{  
    $user = [Security.Principal.WindowsIdentity]::GetCurrent();
    $p = New-Object Security.Principal.WindowsPrincipal $user
    $p.IsInRole([Security.Principal.WindowsBuiltinRole]::Administrator)  
}
```

### <a name="find-a-tenantid"></a>Suchen einer TenantID

Über einen Abonnementnamen:

```powershell
function Get-TenantIdFromSubcriptionName( [string] $subname )
{
    $sub = (Get-AzureRmSubscription -SubscriptionName $subname)
    $sub.TenantId
}

Get-TenantIdFromSubcriptionName "ADLTrainingMS"
```

Über eine Abonnement-ID:

```powershell
function Get-TenantIdFromSubcriptionId( [string] $subid )
{
    $sub = (Get-AzureRmSubscription -SubscriptionId $subid)
    $sub.TenantId
}

$subid = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
Get-TenantIdFromSubcriptionId $subid
```

Über eine Domänenadresse wie z.B. „contoso.com“:


```powershell
function Get-TenantIdFromDomain( $domain )
{
    $url = "https://login.windows.net/" + $domain + "/.well-known/openid-configuration"
    return (Invoke-WebRequest $url|ConvertFrom-Json).token_endpoint.Split('/')[3]
}

$domain = "contoso.com"
Get-TenantIdFromDomain $domain
```

### <a name="list-all-your-subscriptions-and-tenant-ids"></a>Auflisten all Ihrer Abonnements und Mandanten-IDs

```powershell
$subs = Get-AzureRmSubscription
foreach ($sub in $subs)
{
    Write-Host $sub.Name "("  $sub.Id ")"
    Write-Host "`tTenant Id" $sub.TenantId
}
```

## <a name="create-a-data-lake-analytics-account-using-a-template"></a>Erstellen eines Data Lake Analytics-Kontos mithilfe einer Vorlage

Sie können mit dem folgenden PowerShell-Skript eine Azure-Ressourcengruppenvorlage verwenden:

```powershell
$subId = "<Your Azure Subscription ID>"

$rg = "<New Azure Resource Group Name>"
$location = "<Location (such as East US 2)>"
$adls = "<New Data Lake Store Account Name>"
$adla = "<New Data Lake Analytics Account Name>"

$deploymentName = "MyDataLakeAnalyticsDeployment"
$armTemplateFile = "<LocalFolderPath>\azuredeploy.json"  # update the JSON template path 

# Log in to Azure
Login-AzureRmAccount -SubscriptionId $subId

# Create the resource group
New-AzureRmResourceGroup -Name $rg -Location $location

# Create the Data Lake Analytics account with the default Data Lake Store account.
$parameters = @{"adlAnalyticsName"=$adla; "adlStoreName"=$adls}
New-AzureRmResourceGroupDeployment -Name $deploymentName -ResourceGroupName $rg -TemplateFile $armTemplateFile -TemplateParameterObject $parameters 
```

Weitere Informationen finden Sie unter [Bereitstellen einer Anwendung mit einer Azure Resource Manager-Vorlage](../azure-resource-manager/resource-group-template-deploy.md) und [Verfassen von Azure Resource Manager-Vorlagen](../azure-resource-manager/resource-group-authoring-templates.md).

**Beispielvorlage**

Speichern Sie den folgenden Text als `.json`-Datei, und nutzen Sie dann das oben aufgeführte PowerShell-Skript für die Verwendung der Vorlage. 

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "adlAnalyticsName": {
      "type": "string",
      "metadata": {
        "description": "The name of the Data Lake Analytics account to create."
      }
    },
    "adlStoreName": {
      "type": "string",
      "metadata": {
        "description": "The name of the Data Lake Store account to create."
      }
    }
  },
  "resources": [
    {
      "name": "[parameters('adlStoreName')]",
      "type": "Microsoft.DataLakeStore/accounts",
      "location": "East US 2",
      "apiVersion": "2015-10-01-preview",
      "dependsOn": [ ],
      "tags": { }
    },
    {
      "name": "[parameters('adlAnalyticsName')]",
      "type": "Microsoft.DataLakeAnalytics/accounts",
      "location": "East US 2",
      "apiVersion": "2015-10-01-preview",
      "dependsOn": [ "[concat('Microsoft.DataLakeStore/accounts/',parameters('adlStoreName'))]" ],
      "tags": { },
      "properties": {
        "defaultDataLakeStoreAccount": "[parameters('adlStoreName')]",
        "dataLakeStoreAccounts": [
          { "name": "[parameters('adlStoreName')]" }
        ]
      }
    }
  ],
  "outputs": {
    "adlAnalyticsAccount": {
      "type": "object",
      "value": "[reference(resourceId('Microsoft.DataLakeAnalytics/accounts',parameters('adlAnalyticsName')))]"
    },
    "adlStoreAccount": {
      "type": "object",
      "value": "[reference(resourceId('Microsoft.DataLakeStore/accounts',parameters('adlStoreName')))]"
    }
  }
}
```

## <a name="next-steps"></a>Nächste Schritte
* [Übersicht über Microsoft Azure Data Lake Analytics](data-lake-analytics-overview.md)
* Erste Schritte mit Data Lake Analytics mithilfe [des Azure-Portals](data-lake-analytics-get-started-portal.md) | [von Azure PowerShell](data-lake-analytics-get-started-powershell.md) | [von Azure CLI 2.0](data-lake-analytics-get-started-cli2.md)
* Verwalten von Azure Data Lake Analytics mithilfe [des Azure-Portals](data-lake-analytics-manage-use-portal.md) | [von Azure PowerShell](data-lake-analytics-manage-use-powershell.md) | [der Befehlszeilenschnittstelle](data-lake-analytics-manage-use-cli.md) 

