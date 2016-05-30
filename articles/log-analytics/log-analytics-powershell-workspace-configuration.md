<properties
	pageTitle="Verwenden von PowerShell zum Erstellen und Konfigurieren eines Log Analytics-Arbeitsbereichs | Microsoft Azure"
	description="Log Analytics verwendet Daten von Servern in Ihrer lokalen oder Cloudinfrastruktur. Sie können Computerdaten aus dem Azure-Speicher sammeln, wenn sie von Azure-Diagnose generiert werden."
	services="log-analytics"
	documentationCenter=""
	authors="richrundmsft"
	manager="jochan"
	editor=""/>

<tags
	ms.service="log-analytics"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="powershell"
	ms.topic="article"
	ms.date="05/16/2016"
	ms.author="richrund"/>

# Verwalten von Log Analytics mit PowerShell

Mit den [PowerShell-Cmdlets für Log Analytics](http://msdn.microsoft.com/library/mt188224.aspx) können Sie eine Vielzahl von Funktionen in Log Analytics an der Befehlszeile oder in einem Skript ausführen. Beispiele für die Aufgaben, die Sie mit PowerShell ausführen können:

+ Erstellen eines Arbeitsbereichs
+ Hinzufügen oder Entfernen einer Lösung
+ Importieren und Exportieren von gespeicherten Suchvorgängen
+ Hinzufügen des Log Analytics-Agents auf virtuellen Azure-Computern
+ Konfiguration von Log Analytics zum Indizieren der Daten, die mit der Azure-Diagnose gesammelt werden

Dieser Artikel enthält zwei Codebeispiele, die einige der Funktionen veranschaulicht, die Sie mit PowerShell ausführen können. In der [Referenz zu den PowerShell-Cmdlets für Log Analytics](http://msdn.microsoft.com/library/mt188224.aspx) finden Sie noch weitere Funktionen.

> [AZURE.NOTE] Log Analytics wurde früher als Operational Insights bezeichnet, daher wird dieser Name in den Cmdlets verwendet.

## Voraussetzungen

Damit Sie PowerShell mit Ihrem Log Analytics-Arbeitsbereich verwenden können, benötigen Sie Folgendes:

+ Ein Azure-Abonnement und 
+ Einen Azure Log Analytics Arbeitsbereich, der mit Ihrem Azure-Abonnement verknüpft ist

Wenn Sie einen OMS-Arbeitsbereich erstellt haben, diesen aber noch nicht mit einem Azure-Abonnement verknüpft haben, können Sie diese Verknüpfung im Azure-Portal, im OMS-Portal oder über die Cmdlets Get-AzureRMOperationalInsightsLinkTargets und New-AzureRMOperationalInsightsWorkspace erstellen.

## Erstellen eines Log Analytics-Arbeitsbereichs und Hinzufügen von Lösungen und gespeicherten Suchvorgängen

Das folgende Beispielskript veranschaulicht Folgendes:

1.	Erstellen eines Arbeitsbereichs
2.	Auflisten der verfügbaren Lösungen
3.	Hinzufügen von Lösungen zum Arbeitsbereich
4.	Importieren gespeicherter Suchvorgänge
5.	Exportieren gespeicherter Suchvorgänge

```

$ResourceGroup = "oms-example"
$WorkspaceName = "log-analytics-" + (Get-Random -Maximum 99999) # workspace names need to be unique - Get-Random helps with this for the example code
$Location = "westeurope"

# List of solutions to enable
$Solutions = "Security", "Updates", "SQLAssessment"

# Saved Searches to import
$ExportedSearches = @"
[
    {
        "Category":  "My Saved Searches",
        "DisplayName":  "WAD Events (All)",
        "Query":  "Type=Event SourceSystem:AzureStorage ",
        "Version":  1
    },
    {        
        "Category":  "My Saved Searches",
        "DisplayName":  "Current Disk Queue Length",
        "Query":  "Type=Perf ObjectName=LogicalDisk InstanceName="C:" CounterName="Current Disk Queue Length"",
        "Version":  1
    }
]
"@ | ConvertFrom-Json

# Create the resource group if needed
try {
    Get-AzureRmResourceGroup -Name $ResourceGroup -ErrorAction Stop
} catch {
    New-AzureRmResourceGroup -Name $ResourceGroup -Location $Location
}

# Create the workspace
New-AzureRmOperationalInsightsWorkspace -Location $Location -Name $WorkspaceName -Sku Standard -ResourceGroupName $ResourceGroup

# List all solutions and their installation status
Get-AzureRmOperationalInsightsIntelligencePacks -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Add solutions
foreach ($soln in $Solutions) {
    Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -IntelligencePackName $soln -Enabled $true
}

#List enabled solutions
(Get-AzureRmOperationalInsightsIntelligencePacks -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName).Where({($_.enabled -eq $true)})

# Import Saved Searches
foreach ($search in $ExportedSearches) {
    $id = $search.Category + "|" + $search.DisplayName
    New-AzureRmOperationalInsightsSavedSearch -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -SavedSearchId $id -DisplayName $search.DisplayName -Category $search.Category -Query $search.Query -Version $search.Version
}

# Export Saved Searches
(Get-AzureRmOperationalInsightsSavedSearch -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName).Value.Properties | ConvertTo-Json 

```

## Konfigurieren von Log Analytics zum Indizieren der Azure-Diagnose 

Für die Überwachung von Azure-Ressourcen, einschließlich Web- und Workerrollen, Service Fabric-Clustern, Netzwerksicherheitsgruppen, Schlüsseltresoren und Anwendungsgateways, ohne Agents muss für die Ressourcen zunächst die Azure-Diagnose aktiviert werden, um in ein Speicherkonto zu schreiben. Anschließend kann Log Analytics so konfiguriert werden, dass Protokolle aus dem Speicherkonto gesammelt werden.

Sie können auch PowerShell verwenden, um einen Log Analytics-Arbeitsbereich in einem Azure-Abonnement zu konfigurieren und damit Protokolle aus unterschiedlichen Azure-Abonnements zu sammeln.

Das folgende Beispiel veranschaulicht die Vorgehensweise:

1.	Auflisten der vorhandenen Speicherkonten und Speicherorte, aus denen Log Analytics Daten indiziert
2.	Erstellen einer neuen Konfiguration zum Lesen aus einem Speicherkonto
3.	Aktualisieren der neu erstellten Konfiguration zum Indizieren von Daten von zusätzlichen Speicherorten
4.	Löschen der neu erstellten Konfiguration

```
# validTables = "WADWindowsEventLogsTable", "LinuxsyslogVer2v0", "WADServiceFabric*EventTable", "WADETWEventTable" 
$workspace = (Get-AzureRmOperationalInsightsWorkspace).Where({$_.Name -eq "your workspace name"})

# Update these two with the storage account resource id and the storage account key for the storage account you want to Log Analytics to  
$storageId = "/subscriptions/ec11ca60-1234-491e-5678-0ea07feae25c/resourceGroups/demo/providers/Microsoft.Storage/storageAccounts/wadv2storage"
$key = "abcd=="

# List existing insights
Get-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name

# Create a new insight
New-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" -StorageAccountResourceId $storageId -StorageAccountKey $key -Tables @("WADWindowsEventLogsTable") -Containers @("wad-iis-logfiles")

# Update existing insight
Set-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" -Tables @("WADWindowsEventLogsTable", "WADETWEventTable") -Containers @("wad-iis-logfiles", "insights-logs-networksecuritygroupevent/resourceId=/SUBSCRIPTIONS/ec11ca60-1234-491e-5678-0ea07feae25c/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/DEMO")

# Remove the insight
Remove-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" 

```

## Nächste Schritte

- Weitere Informationen zum Verwenden von PowerShell zur Konfiguration von Log Analytics finden Sie in den [PowerShell-Cmdlets für Log Analytics](http://msdn.microsoft.com/library/mt188224.aspx).

<!---HONumber=AcomDC_0518_2016-->