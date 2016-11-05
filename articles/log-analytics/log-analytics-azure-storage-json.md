---
title: Analysieren von Azure-Diagnoseprotokollen mit Log Analytics | Microsoft Docs
description: Log Analytics kann Protokolle von Azure-Diensten lesen, die Azure-Diagnoseprotokolle im JSON-Format in Blob Storage schreiben.
services: log-analytics
documentationcenter: ''
author: bandersmsft
manager: jwhit
editor: ''

ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/10/2016
ms.author: banders

---
# <a name="analyze-azure-diagnostic-logs-using-log-analytics"></a>Analysieren von Azure-Diagnoseprotokollen mit Log Analytics
Log Analytics kann Protokolle von den folgenden Azure-Diensten sammeln, die [Azure-Diagnoseprotokolle](../azure-portal/monitoring-overview-of-diagnostic-logs.md) im JSON-Format in Blob Storage schreiben:

* Automation (Vorschau)
* Key Vault (Vorschau)
* Application Gateway (Vorschau)
* Netzwerksicherheitsgruppe (Vorschau)

Die folgenden Abschnitte führen Sie durch die Verwendung von PowerShell für folgende Schritte:

* Konfigurieren von Log Analytics, um die Protokolle für jede Ressource aus dem Speicher zu sammeln  
* Aktivieren der Log Analytics-Lösung für den Azure-Dienst

Bevor Log Analytics Daten für diese Ressourcen sammeln kann, muss Azure-Diagnose aktiviert sein. Verwenden des Cmdlets `Set-AzureRmDiagnosticSetting` zum Aktivieren der Protokollierung.

Weitere Informationen zum Aktivieren der Diagnoseprotokollierung finden Sie in den folgenden Artikeln:

* [Schlüsseltresor](../key-vault/key-vault-logging.md)
* [Application Gateway](../application-gateway/application-gateway-diagnostics.md)
* [Netzwerksicherheitsgruppen (NSG)](../virtual-network/virtual-network-nsg-manage-log.md)

Diese Dokumentation enthält auch Informationen zu Folgendem:

* Problembehandlung bei der Datensammlung
* Beenden der Datensammlung

## <a name="configure-log-analytics-to-collect-azure-diagnostic-logs"></a>Konfigurieren von Log Analytics zum Sammeln von Azure-Diagnoseprotokollen
Das Sammeln von Protokollen für diese Dienste sowie das Aktivieren der Lösung für die Visualisierung der Protokolle erfolgt mithilfe von PowerShell-Skripts.

Das folgende Beispiel aktiviert die Protokollierung auf allen unterstützten Ressourcen.

```
# update to be the storage account that logs will be written to. Storage account must be in the same region as the resource to monitor
# format is similar to "/subscriptions/ec11ca60-ab12-345e-678d-0ea07bbae25c/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount"
$storageAccountId = ""

$supportedResourceTypes = ("Microsoft.Automation/AutomationAccounts", "Microsoft.KeyVault/Vaults", "Microsoft.Network/NetworkSecurityGroups", "Microsoft.Network/ApplicationGateways")

# update location to match your storage account location
$resources = Get-AzureRmResource | where { $_.ResourceType -in $supportedResourceTypes -and $_.Location -eq "westus" }

foreach ($resource in $resources) {
    Set-AzureRmDiagnosticSetting -ResourceId $resource.ResourceId -StorageAccountId $storageAccountId -Enabled $true -RetentionEnabled $true -RetentionInDays 1
}
```


Für einige Ressourcen kann die genannte Konfiguration auch im Azure-Portal durchgeführt werden. Die dazu erforderlichen Schritte werden unter [Übersicht über Azure-Diagnoseprotokolle](../azure-portal/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-diagnostic-logs) beschrieben.

## <a name="configure-log-analytics-to-collect-azure-diagnostic-logs"></a>Konfigurieren von Log Analytics zum Sammeln von Azure-Diagnoseprotokollen
Wir haben ein PowerShell-Skriptmodul bereitgestellt, das zwei Cmdlets zur Unterstützung beim Konfigurieren von Log Analytics exportiert:

1. `Add-AzureDiagnosticsToLogAnalyticsUI` fordert Sie zur Eingabe auf und kann einfache Konfigurationen einrichten.
2. `Add-AzureDiagnosticsToLogAnalytics` verwendet die zu überwachenden Ressourcen als Eingabe und konfiguriert anschließend Log Analytics  

### <a name="pre-requisites"></a>Voraussetzungen
1. Azure PowerShell mit Version 1.0.8 oder höher der Operational Insights-Cmdlets.
   * [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md)
   * Überprüfen Sie Ihre Version der Cmdlets: `Import-Module AzureRM.OperationalInsights -MinimumVersion 1.0.8 `
2. Diagnoseprotokollierung wird für die Azure-Ressource konfiguriert, die Sie überwachen möchten. Verwenden Sie `Set-AzureRmDiagnosticSetting`, oder lesen Sie unter [Verwenden von Log Analytics für das Sammeln von Daten aus Azure-Speicherkonten](log-analytics-azure-storage.md) nach, wie die Diagnose aktiviert wird.
3. Ein [Log Analytics](https://portal.azure.com/#create/Microsoft.LogAnalyticsOMS) -Arbeitsbereich  
4. Das PowerShell-Modul AzureDiagnosticsAndLogAnalytics
   * Laden Sie das Modul [AzureDiagnosticsAndLogAnalytics](https://www.powershellgallery.com/packages/AzureDiagnosticsAndLogAnalytics/) aus dem PowerShell-Katalog herunter.

### <a name="option-1:-run-the-interactive-configuration-scripts"></a>Option 1: Ausführen des interaktiven Konfigurationsskripts
Öffnen Sie PowerShell, und führen Sie sie aus:

```
# Connect to Azure
Login-AzureRmAccount
# If you have diagnostics logs being written to classic storage you will also need to run
Add-AzureAccount

# Import the module
Install-Module -Name AzureDiagnosticsAndLogAnalytics

# Run the UI configuration script
Add-AzureDiagnosticsToLogAnalyticsUI

```

Sie sehen eine Liste der verfügbaren Auswahl und werden aufgefordert, Ihre Auswahl zu treffen.
Sie werden aufgefordert, für jede der folgenden Optionen eine Auswahl zu treffen:

* Ressourcentypen (Azure-Dienst), aus denen Protokolle erfasst werden sollen
* Ressourceninstanzen, aus denen Protokolle erfasst werden sollen
* Log Analytics-Arbeitsbereich für das Sammeln der Daten

Nach der Ausführung dieses Skripts sollten Sie etwa 30 Minuten, nachdem neue Diagnosedaten in den Speicher geschrieben wurden, in Log Analytics Einträge sehen. Wenn nach dieser Zeit keine Datensätze verfügbar sind, nutzen Sie den folgenden Abschnitt zur Problembehandlung.

### <a name="option-2:-build-a-list-of-resources-and-pass-them-to-the-configuration-cmdlet"></a>Option 2: Erstellen einer Liste von Ressourcen und deren Übergabe an das Konfigurations-Cmdlet
Sie können eine Liste der Ressourcen erstellen, für die Azure-Diagnose aktiviert ist, und dann die Ressourcen an das Konfigurations-Cmdlet übergeben.

Zusätzliche Informationen zu diesem Cmdlet erhalten Sie, indem Sie `Get-Help Add-AzureDiagnosticsToLogAnalytics` ausführen.

Weitere Einzelheiten zu OMS finden Sie unter [PowerShell-Cmdlets für Log Analytics](https://msdn.microsoft.com/library/mt188224.aspx).

> [!NOTE]
> Wenn sich die Ressourcen und der Arbeitsbereich in unterschiedlichen Azure-Abonnements befinden, können Sie bei Bedarf mit `Select-AzureRmSubscription -SubscriptionId <Subscription the resource is in>` zwischen diesen wechseln.
> 
> 

```
# Connect to Azure
Login-AzureRmAccount
# If you have diagnostics logs being written to classic storage you will also need to run
Add-AzureAccount

# Import the module
Install-Module -Name AzureDiagnosticsAndLogAnalytics

# Update the values below with the name of the resource that has Azure diagnostics enabled and the name of your Log Analytics workspace
$resourceName = "***example-resource***"
$workspaceName = "***log-analytics-workspace***"

# Find the resource to monitor:
$resource = Find-AzureRmResource -ResourceType "Microsoft.KeyVault/Vaults" -ResourceNameContains $resourceName

# Find the Log Analytics workspace to configure, for example:
$workspace = Find-AzureRmResource -ResourceType "Microsoft.OperationalInsights/workspaces" -ResourceNameContains $workspaceName

# Perform configuration
Add-AzureDiagnosticsToLogAnalytics $resource $workspace

# Enable the Log Analytics solution
Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -intelligencepackname KeyVault -Enabled $true

```
Nach der Ausführung dieses Skripts sollten Sie etwa 30 Minuten, nachdem neue Diagnosedaten in den Speicher geschrieben wurden, in Log Analytics Einträge sehen. Wenn nach dieser Zeit keine Datensätze verfügbar sind, nutzen Sie den folgenden Abschnitt zur Problembehandlung.  

> [!NOTE]
> Diese Konfiguration wird nicht im Azure-Portal angezeigt. Sie können die Konfiguration mit dem `Get-AzureRmOperationalInsightsStorageInsight` -Cmdlet überprüfen.  
> 
> 

## <a name="stopping-log-analytics-from-collecting-azure-diagnostic-logs"></a>Beenden des Sammelns von Azure-Diagnoseprotokollen durch Log Analytics
Wenn Sie die Log Analytics-Konfiguration für eine Ressource löschen möchten, verwenden Sie das Cmdlet `Remove-AzureRmOperationalInsightsStorageInsight`.

## <a name="troubleshooting-configuration-for-azure-diagnostic-logs"></a>Behandeln von Problemen bei der Konfiguration für Azure-Diagnoseprotokolle
*Problem*

Der folgende Fehler wird angezeigt, wenn Sie eine Ressource konfigurieren, deren Protokollierung im klassischen Speicher erfolgt:

```
Select-AzureSubscription : The subscription id 7691b0d1-e786-4757-857c-7360e61896c3 doesn't exist.

Parameter name: id
```

*Lösung*

Melden Sie sich mit `Add-AzureAccount` bei der API für das klassische Bereitstellungsmodell an.

## <a name="troubleshooting-data-collection-for-azure-diagnostic-logs"></a>Behandeln von Problemen bei der Datensammlung für Azure-Diagnoseprotokolle
Wenn in Log Analytics keine Daten für Ihre Azure-Ressource angezeigt werden, können Sie die folgenden Problembehandlungsschritte ausführen:

* Überprüfen Sie den Datenfluss auf das Speicherkonto.
* Überprüfen Sie, ob die Log Analytics-Lösung für den Dienst aktiviert ist.
* Überprüfen Sie, ob Log Analytics zum Lesen aus dem Speicher konfiguriert ist.
* Aktualisieren Sie den Schlüssel des Speicherkontos.

### <a name="verify-data-is-flowing-to-the-storage-account"></a>Überprüfen Sie den Datenfluss auf das Speicherkonto.
Sie können mit einem Tool für das Durchsuchen von Azure Storage (z.B. Visual Studio) oder mithilfe von PowerShell überprüfen, ob Daten in das Speicherkonto geschrieben werden.

Um das Speicherkonto zu finden, für dessen Nutzung zur Protokollierung die Ressource konfiguriert ist, verwenden Sie folgenden PowerShell-Code:

`Find-AzureRmResource -ResourceType "Microsoft.KeyVault/Vaults" | select ResourceId | Get-AzureRmDiagnosticSetting `

Der von der Azure-Diagnose verwendete Speichercontainer hat einen Namen mit dem Format:  

`insights-logs-<Category>`

Weitere Informationen zum Anzeigen des Speicherkontoinhalts finden Sie unter [Verwenden von Storage-Cmdlets für das Überprüfen eines Containers auf neu geschriebene Daten](../storage/storage-powershell-guide-full.md).

### <a name="verify-the-log-analytics-solution-for-the-service-is-enabled"></a>Überprüfen Sie, ob die Log Analytics-Lösung für den Dienst aktiviert ist.
Bei Verwendung von `Add-AzureDiagnosticsToLogAnalyticsUI` wird die richtige Log Analytics-Lösung automatisch für Sie aktiviert.

Um zu überprüfen, ob eine Lösung aktiviert ist, führen Sie folgenden PowerShell-Code aus:

`Get-AzureRmOperationalInsightsIntelligencePacks -ResourceGroupName $logAnalyticsResourceGroup -WorkspaceName $logAnalyticsWorkspaceName`

Wenn die Lösung nicht aktiviert ist, können Sie sie mit dem folgenden PowerShell-Code aktivieren:

`Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName $logAnalyticsResourceGroup -WorkspaceName $logAnalyticsWorkspaceName -IntelligencePackName $solution -Enabled $true`

Um den Namen der Lösung zur Aktivierung für jeden Ressourcentyp zu finden, verwenden Sie den folgenden PowerShell-Code (diese Variable ist verfügbar, nachdem Sie das Modul importiert haben):

`$MonitorableResourcesToOMSSolutions`

### <a name="verify-that-log-analytics-is-configured-to-read-from-storage"></a>Überprüfen Sie, ob Log Analytics zum Lesen aus dem Speicher konfiguriert ist.
Wenn Sie zusätzliche Azure-Ressourcen hinzufügen, müssen Sie für sie sowohl die Diagnoseprotokollierung aktivieren als auch Log Analytics konfigurieren.
Um zu überprüfen, für welche Ressourcen und Speicherkonten die Erfassung von Protokollen durch Log Analytics konfiguriert ist, verwenden Sie folgenden PowerShell-Code:

```
# Find the Workspace ResourceGroup and Name
$logAnalyticsWorkspace = Get-AzureRmOperationalInsightsWorkspace

#Get the configuration for all resources:
Get-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $logAnalyticsWorkspace.ResourceGroupName -WorkspaceName $logAnalyticsWorkspace.Name

#Get the just the resources configured:
Get-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $logAnalyticsWorkspace.ResourceGroupName -WorkspaceName $logAnalyticsWorkspace.Name | select Containers
```

### <a name="update-the-storage-key"></a>Aktualisieren des Speicherschlüssels
Wenn Sie den Schlüssel für das Speicherkonto ändern, muss die Log Analytics-Konfiguration ebenfalls mit dem neuen Schlüssel aktualisiert werden.
Sie können die Log Analytics-Konfiguration mit folgendem PowerShell-Code mit einem neuen Schlüssel aktualisieren:

`Set-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $logAnalyticsWorkspace.ResourceGroupName -WorkspaceName $logAnalyticsWorkspace.Name –Name <Storage Insight Name> -StorageAccountKey $newKey `

Um den Storage Insight-Namen zu finden, verwenden Sie das `Get-AzureRmOperationalInsightsStorageInsight` -Cmdlet, wie in vorherigen Beispielen gezeigt.

## <a name="next-steps"></a>Nächste Schritte
* [Verwenden von Blob Storage für IIS und Table Storage für Ereignisse](log-analytics-azure-storage-iis-table.md) enthält Informationen zum Lesen der Protokolle für Azure-Dienste, die Diagnosedaten in Table Storage schreiben, oder der IIS-Protokolle, die in Blob Storage geschrieben werden.
* [Aktivieren Sie Lösungen](log-analytics-add-solutions.md) , um Einblick in die Daten bereitzustellen.
* [Erstellen Sie Suchabfragen](log-analytics-log-searches.md) , um die Daten zu analysieren.

<!--HONumber=Oct16_HO2-->


