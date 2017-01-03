---
title: Verwenden von PowerShell zum Erstellen und Konfigurieren eines Log Analytics-Arbeitsbereichs | Microsoft-Dokumentation
description: "Log Analytics verwendet Daten von Servern in Ihrer lokalen oder Cloudinfrastruktur. Sie können Computerdaten aus dem Azure-Speicher sammeln, wenn sie von Azure-Diagnose generiert werden."
services: log-analytics
documentationcenter: 
author: richrundmsft
manager: jochan
editor: 
ms.assetid: 3b9b7ade-3374-4596-afb1-51b695f481c2
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: powershell
ms.topic: article
ms.date: 11/21/2016
ms.author: richrund
translationtype: Human Translation
ms.sourcegitcommit: b39cd142925be91bd7a90183cada7ba040a344c0
ms.openlocfilehash: b8ebf6a2b3c8d2e5b173e429f39c9836e7d214ac


---
# <a name="manage-log-analytics-using-powershell"></a>Verwalten von Log Analytics mit PowerShell
Mit den [PowerShell-Cmdlets für Log Analytics](https://msdn.microsoft.com/library/mt188224\(v=azure.300\).aspx) können Sie verschiedene Funktionen in Log Analytics von der Befehlszeile oder in einem Skript ausführen.  Beispiele für die Aufgaben, die Sie mit PowerShell ausführen können:

* Erstellen eines Arbeitsbereichs
* Hinzufügen oder Entfernen einer Lösung
* Importieren und Exportieren von gespeicherten Suchvorgängen
* Erstellen einer Computergruppe
* Aktivieren der Sammlung von IIS-Protokollen auf Computern mit installiertem Windows-Agent
* Sammeln von Leistungsindikatoren von Windows- und Linux-Computern
* Sammeln von Ereignissen aus Syslog auf Linux-Computern 
* Sammeln von Ereignissen aus Windows-Ereignisprotokollen
* Sammeln von benutzerdefinierten Ereignisprotokollen
* Hinzufügen des Log Analytics-Agents auf virtuellen Azure-Computern
* Konfiguration von Log Analytics zum Indizieren der Daten, die mit der Azure-Diagnose gesammelt werden

Dieser Artikel enthält zwei Codebeispiele, die einige der Funktionen veranschaulicht, die Sie mit PowerShell ausführen können.  In der [Referenz zu den PowerShell-Cmdlets für Log Analytics](https://msdn.microsoft.com/library/mt188224\(v=azure.300\).aspx) finden Sie noch weitere Funktionen.

> [!NOTE]
> Log Analytics wurde früher als Operational Insights bezeichnet, daher wird dieser Name in den Cmdlets verwendet.
> 
> 

## <a name="prerequisites"></a>Voraussetzungen
In diesen Beispielen wird Version 2.3.0 oder höher des AzureRm.OperationalInsights-Moduls verwendet.


## <a name="create-and-configure-a-log-analytics-workspace"></a>Erstellen und Konfigurieren eines Log Analytics-Arbeitsbereichs
Das folgende Beispielskript veranschaulicht Folgendes:

1. Erstellen eines Arbeitsbereichs
2. Auflisten der verfügbaren Lösungen
3. Hinzufügen von Lösungen zum Arbeitsbereich
4. Importieren gespeicherter Suchvorgänge
5. Exportieren gespeicherter Suchvorgänge
6. Erstellen einer Computergruppe
7. Aktivieren der Sammlung von IIS-Protokollen auf Computern mit installiertem Windows-Agent
8. Sammeln von Leistungsindikatoren logischer Datenträger von Linux-Computern (Prozentsatz verwendeter I-Knoten, MB frei, Prozentsatz des verwendeten Speicherplatzes, Übertragungen/s, Lesevorgänge/s, Schreibvorgänge/s)
9. Sammeln von Syslog-Ereignissen auf Linux-Computern
10. Sammeln von Fehler- und Warnereignissen aus dem Anwendungsereignisprotokoll von Windows-Computern
11. Erfassen des Leistungsindikators „Verfügbarer Arbeitsspeicher in MB“ von Windows-Computern
12. Sammeln eines benutzerdefinierten Protokolls 

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
        "Query":  "Type=Perf ObjectName=LogicalDisk InstanceName=\"C:\" CounterName=\"Current Disk Queue Length\"",
        "Version":  1
    }
]
"@ | ConvertFrom-Json

# Custom Log to collect
$CustomLog = @"
{
    "customLogName": "sampleCustomLog1", 
    "description": "Example custom log datasource", 
    "inputs": [
        { 
            "location": { 
            "fileSystemLocations": { 
                "windowsFileTypeLogPaths": [ "e:\\iis5\\*.log" ], 
                "linuxFileTypeLogPaths": [ "/var/logs" ] 
                } 
            }, 
        "recordDelimiter": { 
            "regexDelimiter": { 
                "pattern": "\\n", 
                "matchIndex": 0, 
                "matchIndexSpecified": true, 
                "numberedGroup": null 
                } 
            } 
        }
    ], 
    "extractions": [
        { 
            "extractionName": "TimeGenerated", 
            "extractionType": "DateTime", 
            "extractionProperties": { 
                "dateTimeExtraction": { 
                    "regex": null, 
                    "joinStringRegex": null 
                    } 
                } 
            }
        ] 
    }
"@

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
foreach ($solution in $Solutions) {
    Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -IntelligencePackName $solution -Enabled $true
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

# Create Computer Group based on a query
New-AzureRmOperationalInsightsComputerGroup -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -SavedSearchId "My Web Servers" -DisplayName "Web Servers" -Category "My Saved Searches" -Query "Computer=""web*"" | distinct Computer" -Version 1

# Create a computer group based on names (up to 5000)
$computerGroup = """servername1.contoso.com"",""servername2.contoso.com"",""servername3.contoso.com"",""servername4.contoso.com"""
New-AzureRmOperationalInsightsComputerGroup -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -SavedSearchId "My Named Servers" -DisplayName "Named Servers" -Category "My Saved Searches" -Query $computerGroup -Version 1

# Enable IIS Log Collection using agent
Enable-AzureRmOperationalInsightsIISLogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Linux Perf
New-AzureRmOperationalInsightsLinuxPerformanceObjectDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -ObjectName "Logical Disk" -InstanceName "*"  -CounterNames @("% Used Inodes", "Free Megabytes", "% Used Space", "Disk Transfers/sec", "Disk Reads/sec", "Disk Reads/sec", "Disk Writes/sec") -IntervalSeconds 20  -Name "Example Linux Disk Performance Counters"
Enable-AzureRmOperationalInsightsLinuxCustomLogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Linux Syslog
New-AzureRmOperationalInsightsLinuxSyslogDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -Facility "kern" -CollectEmergency -CollectAlert -CollectCritical -CollectError -CollectWarning -Name "Example kernal syslog collection"
Enable-AzureRmOperationalInsightsLinuxSyslogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Windows Event
New-AzureRmOperationalInsightsWindowsEventDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -EventLogName "Application" -CollectErrors -CollectWarnings -Name "Example Application Event Log"

# Windows Perf
New-AzureRmOperationalInsightsWindowsPerformanceCounterDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -ObjectName "Memory" -InstanceName "*" -CounterName "Available MBytes" -IntervalSeconds 20 -Name "Example Windows Performance Counter"

# Custom Logs
New-AzureRmOperationalInsightsCustomLogDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -CustomLogRawJson "$CustomLog" -Name "Example Custom Log Collection"

```

## <a name="configuring-log-analytics-to-index-azure-diagnostics"></a>Konfigurieren von Log Analytics zum Indizieren der Azure-Diagnose
Für die Überwachung von Azure-Ressourcen ohne Agents müssen die Ressourcen Azure-Diagnose aktiviert haben und zum Schreiben in einen Log Analytics-Arbeitsbereich konfiguriert worden sein. Bei diesem Ansatz werden Daten direkt an Log Analytics gesendet, und Daten müssen nicht in ein Speicherkonto geschrieben werden. Unterstützte Ressourcen:

| Ressourcentyp | Protokolle | Metriken |
| --- | --- | --- |
| Anwendungsgateways    | Ja | Ja |
| Automation-Konten     | Ja | |
| Batch-Konten          | Ja | Ja |
| Data Lake Analytics     | Ja | | 
| Data Lake Store         | Ja | |
| Elastischer SQL-Pool        |     | Ja |
| Event Hub-Namespace     |     | Ja |
| IoT Hubs                |     | Ja |
| Schlüsseltresor               | Ja | |
| Load Balancer          | Ja | |
| Logik-Apps              | Ja | Ja |
| Netzwerksicherheitsgruppen | Ja | |
| Redis-Cache             |     | Ja |
| Suchdienste         | Ja | Ja |
| Service Bus-Namespace   |     | Ja |
| SQL (v12)               |     | Ja |
| Websites               |     | Ja |
| Webserverfarmen        |     | Ja |

Ausführliche Informationen zu den verfügbaren Metriken finden Sie unter [Supported metrics with Azure Monitor](../monitoring-and-diagnostics/monitoring-supported-metrics.md) (Von Azure Monitor unterstützte Metriken).

Ausführliche Informationen zu den verfügbaren Protokollen finden Sie unter [Supported services and schema for Diagnostic Logs](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#supported-services-and-schema-for-diagnostic-logs) (Unterstützte Dienste und Schema für Diagnoseprotokolle).

```
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$resourceId = "/SUBSCRIPTIONS/ec11ca60-1234-491e-5678-0ea07feae25c/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/DEMO" 

Set-AzureRmDiagnosticSetting -ResourceId $resourceId -WorkspaceId $workspaceId -Enabled $true
```

Sie können mit dem vorherigen Cmdlet auch Protokolle von Ressourcen in verschiedenen Abonnements sammeln. Das Cmdlet kann abonnementübergreifend verwendet werden, da Sie die ID der Ressource, die Protokolle erstellt, und des Arbeitsbereichs angeben, an den die Protokolle gesendet werden.


## <a name="configuring-log-analytics-to-index-azure-diagnostics-from-storage"></a>Konfigurieren von Log Analytics zum Indizieren der Azure-Diagnose aus dem Speicher
Damit Sie Protokolldaten in einer ausgeführten Instanz eines klassischen Clouddiensts oder eines Service Fabric-Clusters erfassen können, müssen Sie die Daten zunächst in den Azure-Speicher schreiben. Log Analytics wird dann konfiguriert, um die Protokolle aus dem Speicherkonto zu sammeln. Unterstützte Ressourcen:

* Azure Cloud Services (Web- und Workerrollen)
* Service Fabric-Cluster

Das folgende Beispiel veranschaulicht die Vorgehensweise:

1. Auflisten der vorhandenen Speicherkonten und Speicherorte, aus denen Log Analytics Daten indiziert
2. Erstellen einer Konfiguration zum Lesen aus einem Speicherkonto
3. Aktualisieren der neu erstellten Konfiguration zum Indizieren von Daten von zusätzlichen Speicherorten
4. Löschen der neu erstellten Konfiguration

```
# validTables = "WADWindowsEventLogsTable", "LinuxsyslogVer2v0", "WADServiceFabric*EventTable", "WADETWEventTable" 
$workspace = (Get-AzureRmOperationalInsightsWorkspace).Where({$_.Name -eq "your workspace name"})

# Update these two lines with the storage account resource ID and the storage account key for the storage account you want to Log Analytics to  
$storageId = "/subscriptions/ec11ca60-1234-491e-5678-0ea07feae25c/resourceGroups/demo/providers/Microsoft.Storage/storageAccounts/wadv2storage"
$key = "abcd=="

# List existing insights
Get-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name

# Create a new insight
New-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" -StorageAccountResourceId $storageId -StorageAccountKey $key -Tables @("WADWindowsEventLogsTable") -Containers @("wad-iis-logfiles")

# Update existing insight
Set-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" -Tables @("WADWindowsEventLogsTable", "WADETWEventTable") -Containers @("wad-iis-logfiles")

# Remove the insight
Remove-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" 

```

Sie können mit dem vorherigen Skript auch Protokolle aus Speicherkonten in verschiedenen Abonnements sammeln. Das Skript kann abonnementübergreifend verwendet werden, da Sie die Speicherkontoressourcen-ID und einen entsprechenden Zugriffsschlüssel bereitstellen. Wenn Sie den Zugriffsschlüssel ändern, müssen Sie die Speicherdetails aktualisieren, sodass sie den neuen Schlüssel enthalten.


## <a name="next-steps"></a>Nächste Schritte
* [PowerShell-Cmdlets für Log Analytics](https://msdn.microsoft.com/library/mt188224\(v=azure.300\).aspx) .




<!--HONumber=Jan17_HO1-->


