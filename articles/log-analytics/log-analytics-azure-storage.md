---
title: "Sammeln von Azure-Dienstprotokollen und Metriken für Log Analytics | Microsoft-Dokumentation"
description: "Konfigurieren Sie Diagnosen für Azure-Ressourcen, um Protokolle und Metriken in Log Analytics zu schreiben."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 84105740-3697-4109-bc59-2452c1131bfe
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: a0c8af30fbed064001c3fd393bf0440aa1cb2835
ms.openlocfilehash: 935680e33c1b32619b7acb8d50a61e48b6703402
ms.lasthandoff: 02/28/2017


---
# <a name="collect-azure-service-logs-and-metrics-for-use-in-log-analytics"></a>Sammeln von Azure-Dienstprotokollen und Metriken zur Verwendung in Log Analytics

Protokolle und Metriken für Azure-Dienste können auf vier Arten erfasst werden:

1. Azure-Diagnosen direkt an Log Analytics (*Diagnose* in der folgenden Tabelle)
2. Azure-Diagnosen über Azure Storage an Log Analytics (*Storage* in der folgenden Tabelle)
3. Connectors für Azure-Dienste (*Connectors* in der folgenden Tabelle)
4. Skripts zum Sammeln und anschließenden Veröffentlichen von Daten in Log Analytics (Leerstellen in der folgenden Tabelle und für nicht aufgeführte Dienste)


| Dienst                 | Ressourcentyp                           | Protokolle        | Metriken     | Lösung |
| --- | --- | --- | --- | --- |
| Anwendungsgateways    | Microsoft.Network/applicationGateways   | Diagnose | Diagnose | [Azure Application Gateway-Analyse](log-analytics-azure-networking-analytics.md#azure-application-gateway-analytics-solution-in-log-analytics) |
| Application Insights    |                                         | Connector   | Connector   | [Application Insights-Connector](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/) (Vorschau) |
| Automation-Konten     | Microsoft.Automation/AutomationAccounts | Diagnose |             | [Weitere Informationen](../automation/automation-manage-send-joblogs-log-analytics.md)|
| Batch-Konten          | Microsoft.Batch/batchAccounts           | Diagnose | Diagnose | |
| Klassische Clouddienste  |                                         | Speicher     |             | [Weitere Informationen](log-analytics-azure-storage-iis-table.md) |
| Cognitive Services      | Microsoft.CognitiveServices/accounts    |             | Diagnose | |
| Data Lake Analytics     | Microsoft.DataLakeAnalytics/accounts    | Diagnose |             | |
| Data Lake Store         | Microsoft.DataLakeStore/accounts        | Diagnose |             | |
| Event Hub-Namespace     | Microsoft.EventHub/namespaces           | Diagnose | Diagnose | |
| IoT Hubs                | Microsoft.Devices/IotHubs               |             | Diagnose | |
| Schlüsseltresor               | Microsoft.KeyVault/vaults               | Diagnose |             | [KeyVault-Analyse](log-analytics-azure-key-vault.md) |
| Load Balancer          | Microsoft.Network/loadBalancers         | Diagnose |             |  |
| Logik-Apps              | Microsoft.Logic/workflows <br> Microsoft.Logic/integrationAccounts | Diagnose | Diagnose | |
| Netzwerksicherheitsgruppen | Microsoft.Network/networksecuritygroups | Diagnose |             | [Azure-Netzwerksicherheitsgruppen-Analyse](log-analytics-azure-networking-analytics.md#azure-network-security-group-analytics-solution-in-log-analytics) |
| Suchdienste         | Microsoft.Search/searchServices         | Diagnose | Diagnose | |
| Service Bus-Namespace   | Microsoft.ServiceBus/namespaces         | Diagnose | Diagnose | |
| Service Fabric          |                                         | Speicher     |             | [Service Fabric-Analysen (Vorschau)](log-analytics-service-fabric.md) |
| SQL (v12)               | Microsoft.Sql/servers/databases <br> Microsoft.Sql/servers/elasticPools |             | Diagnose | |
| Virtual Machines        | Microsoft.Compute/virtualMachines       | Durchwahl   | Durchwahl <br> Diagnose  | |
| VM-Skalierungsgruppen | Microsoft.Compute/virtualMachines <br> Microsoft.Compute/virtualMachineScaleSets/virtualMachines |             | Diagnose | |
| Webserverfarmen        | Microsoft.Web/serverfarms               |             | Diagnose | |
| Websites               | Microsoft.Web/sites <br> Microsoft.Web/sites/slots |             | Diagnose | [Weitere Informationen](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webappazure-oms-monitoring) |


> [!NOTE]
> Für die Überwachung von virtuellen Azure-Computern (Linux und Windows) empfehlen wir die Installation der [Log Analytics-VM-Erweiterung](log-analytics-azure-vm-extension.md). Der Agent liefert Erkenntnisse, die aus Ihren virtuellen Computern zusammengetragen wurden. Die Erweiterung kann auch für VM-Skalierungsgruppen verwendet werden.
>
>

## <a name="azure-diagnostics-direct-to-log-analytics"></a>Azure-Diagnosen direkt an Log Analytics
Viele Azure-Ressourcen können Diagnoseprotokolle und Metriken direkt in Log Analytics schreiben. Dies ist die bevorzugte Datensammlungsmethode für die Analyse. Bei Verwendung von Azure-Diagnosen werden Daten unmittelbar in Log Analytics geschrieben und müssen nicht zuerst in den Speicher geschrieben werden.

Azure-Ressourcen, die [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview.md) unterstützen, können ihre Protokolle und Metriken direkt an Log Analytics senden.

* Ausführliche Informationen zu den verfügbaren Metriken finden Sie unter [Supported metrics with Azure Monitor](../monitoring-and-diagnostics/monitoring-supported-metrics.md) (Von Azure Monitor unterstützte Metriken).
* Ausführliche Informationen zu den verfügbaren Protokollen finden Sie unter [Supported services and schema for Diagnostic Logs](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#supported-services-and-schema-for-diagnostic-logs) (Unterstützte Dienste und Schema für Diagnoseprotokolle).

### <a name="enable-diagnostics-with-powershell"></a>Aktivieren der Diagnose mit PowerShell
Sie benötigen die [Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/)-Version von November 2016 (v2.3.0) oder höher.

Das folgende PowerShell-Beispiel zeigt, wie mithilfe von [Set-AzureRmDiagnosticSetting](https://docs.microsoft.com/powershell/resourcemanager/azurerm.insights/v2.3.0/set-azurermdiagnosticsetting) die Diagnose für eine Netzwerksicherheitsgruppe aktiviert wird. Die gleiche Herangehensweise kann für alle unterstützten Ressourcen verwendet werden. Legen Sie `$resourceId` auf die Ressourcen-ID der Ressource fest, für die Sie die Diagnose aktivieren möchten.

```powershell
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$resourceId = "/SUBSCRIPTIONS/ec11ca60-1234-491e-5678-0ea07feae25c/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/DEMO"

Set-AzureRmDiagnosticSetting -ResourceId $ResourceId  -WorkspaceId $workspaceId -Enabled $true
```

### <a name="enable-diagnostics-with-resource-manager-templates"></a>Aktivieren der Diagnose mithilfe von Resource Manager-Vorlagen

Wenn die Diagnose für eine Ressource bei deren Erstellung aktiviert und an Ihren Log Analytics-Arbeitsbereich gesendet werden soll, können Sie eine Vorlage wie die folgende verwenden. Hierbei handelt es sich zwar um ein Beispiel für ein Automation-Konto, es kann jedoch für jeden unterstützten Ressourcentyp verwendet werden.

```json
        {
            "type": "Microsoft.Automation/automationAccounts/providers/diagnosticSettings",
            "name": "[concat(parameters('omsAutomationAccountName'), '/', 'Microsoft.Insights/service')]",
            "apiVersion": "2015-07-01",
            "dependsOn": [
                "[concat('Microsoft.Automation/automationAccounts/', parameters('omsAutomationAccountName'))]",
                "[concat('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspaceName'))]"
            ],
            "properties": {
                "workspaceId": "[resourceId('Microsoft.OperationalInsights/workspaces', parameters('omsWorkspaceName'))]",
                "logs": [
                    {
                        "category": "JobLogs",
                        "enabled": true
                    },
                    {
                        "category": "JobStreams",
                        "enabled": true
                    }
                ]
            }
        }
```

[!INCLUDE [log-analytics-troubleshoot-azure-diagnostics](../../includes/log-analytics-troubleshoot-azure-diagnostics.md)]

## <a name="azure-diagnostics-to-storage-then-to-log-analytics"></a>Azure-Diagnose über den Speicher an Log Analytics

Bei einigen Ressourcen können Sie die Protokolle an Azure Storage senden und Log Analytics so konfigurieren, dass die Protokolle aus dem Speicher gelesen werden.

Mit dieser Vorgehensweise kann Log Analytics Diagnosen für folgende Ressourcen und Protokolle aus Azure Storage erfassen:

| Ressource | Protokolle |
| --- | --- |
| Service Fabric |ETWEvent <br> Operative Ereignisse <br> Reliable Actor-Ereignis <br> Reliable Services-Ereignis |
| Virtual Machines |Linux-Syslog <br> Windows-Ereignis <br> IIS-Protokoll <br> Windows-ETWEvent |
| Webrollen <br> Workerrollen |Linux-Syslog <br> Windows-Ereignis <br> IIS-Protokoll <br> Windows-ETWEvent |

> [!NOTE]
> Ihnen werden die üblichen Azure-Datenraten für Speicherung und Transaktionen in Rechnung gestellt, wenn Sie Diagnosedaten an ein Speicherkonto senden und Log Analytics die Daten aus Ihrem Speicherkonto liest.
>
>

Weitere Information dazu, wie Log Analytics diese Protokolle erfassen kann, finden Sie unter [Verwenden von Blob Storage für IIS und Table Storage für Ereignisse](log-analytics-azure-storage-iis-table.md).

## <a name="connectors-for-azure-services"></a>Connectors für Azure-Dienste

Es gibt einen Connector für Application Insights, mit dem Daten, die von Application Insights gesammelt wurden, an Log Analytics gesendet werden können.

Weitere Informationen zum Application Insights-Connector finden Sie [hier](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/).

## <a name="scripts-to-collect-and-post-data-to-log-analytics"></a>Skripts zum Sammeln und Veröffentlichen von Daten für Log Analytics

Für Azure-Dienste, bei denen Protokolle und Metriken nicht direkt an Log Analytics gesendet werden können, können Sie das Protokoll und die Metriken mithilfe von Azure Automation erfassen. Das Skript kann die Daten dann über die [Datensammler-API](log-analytics-data-collector-api.md) an Log Analytics senden.

Der Azure-Vorlagenkatalog enthält [Beispiele für die Verwendung von Azure Automation](https://azure.microsoft.com/en-us/resources/templates/?term=OMS) zum Sammeln von Daten aus Diensten und zum Senden der Daten an Log Analytics.

## <a name="next-steps"></a>Nächste Schritte

* [Verwenden von Blob Storage für IIS und Table Storage für Ereignisse](log-analytics-azure-storage-iis-table.md) enthält Informationen zum Lesen der Protokolle für Azure-Dienste, die Diagnosedaten in Table Storage schreiben, oder der IIS-Protokolle, die in Blob Storage geschrieben werden.
* [Aktivieren Sie Lösungen](log-analytics-add-solutions.md) , um Einblick in die Daten bereitzustellen.
* [Erstellen Sie Suchabfragen](log-analytics-log-searches.md) , um die Daten zu analysieren.

