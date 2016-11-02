<properties
    pageTitle="Sammeln von Azure-Speicherdaten in Log Analytics (Übersicht) | Microsoft Azure"
    description="Azure-Ressourcen können mit Azure-Diagnose Protokolle und Metriken in ein Azure-Speicherkonto schreiben. Log Analytics kann diese Daten indizieren und durchsuchbar machen."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>


# <a name="collecting-azure-storage-data-in-log-analytics-overview"></a>Sammeln von Azure-Speicherdaten in Log Analytics – Übersicht

Viele Azure-Ressourcen können Protokolle und Metriken in ein Azure-Speicherkonto schreiben. Log Analytics kann diese Daten verarbeiten und damit das Überwachen Ihrer Azure-Ressourcen erleichtern.

Für das Schreiben in Azure Storage kann eine Ressource Azure-Diagnose oder eine eigene Methode zum Schreiben von Daten verwenden. Diese Daten können in verschiedenen Formaten an einen der folgenden Speicherorte geschrieben werden:

+ Azure-Tabelle
+ Azure-Blob
+ EventHub

Log Analytics unterstützt Azure-Dienste, die Daten mit [Azure Diagnoseprotokollen](../azure-portal/monitoring-overview-of-diagnostic-logs.md) schreiben. Darüber hinaus unterstützt Log Analytics andere Dienste, die Protokolle und Metriken in verschiedenen Formaten und an unterschiedlichen Speicherorten ausgeben.  

>[AZURE.NOTE] Ihnen werden die üblichen Azure-Datenraten für Speicherung und Transaktionen in Rechnung gestellt, wenn Sie Diagnosedaten an ein Speicherkonto senden und Log Analytics die Daten aus Ihrem Speicherkonto liest.

![Azure-Speicherdiagramm](media/log-analytics-azure-storage/azure-storage-diagram.png)

## <a name="supported-azure-resources"></a>Unterstützte Azure-Ressourcen

Log Analytics kann Daten für die folgenden Azure-Ressourcen sammeln:

| Ressourcentyp | Protokolle (Diagnosekategorien) | Log Analytics-Lösung |
| --------------------------------------- | -------------------------------- | --------------- |
| Application Insights | Availability <br> Benutzerdefinierte Ereignisse <br> Ausnahmen <br> Anforderungen <br> | Application Insights (Vorschau) |
| API Management | | *keine* (Vorschau) |
| Automation <br> Microsoft.Automation/AutomationAccounts | JobLogs <br> JobStreams          | AzureAutomation (Vorschau) |
| Schlüsseltresor <br> Microsoft.KeyVault/Vaults               | AuditEvent                       | KeyVault (Vorschau) |
| Application Gateway <br> Microsoft.Network/ApplicationGateways   | ApplicationGatewayAccessLog <br> ApplicationGatewayPerformanceLog | AzureNetworking (Vorschau) |
| Netzwerksicherheitsgruppen (NSG) <br> Microsoft.Network/NetworkSecurityGroups | NetworkSecurityGroupEvent <br> NetworkSecurityGroupRuleCounter | AzureNetworking (Vorschau) |
| Service Fabric                          | ETWEvent <br> Operative Ereignisse <br> Reliable Actor-Ereignis <br> Reliable Services-Ereignis| ServiceFabric (Vorschau) |
| Virtual Machines | Linux-Syslog <br> Windows-Ereignis <br> IIS-Protokoll <br> Windows-ETWEvent | *keine* |
| Webrollen <br> Workerrollen | Linux-Syslog <br> Windows-Ereignis <br> IIS-Protokoll <br> Windows-ETWEvent | *keine* |

>[AZURE.NOTE] Für die Überwachung von virtuellen Azure-Computern (Linux und Windows) empfehlen wir die Installation der [Log Analytics-VM-Erweiterung](log-analytics-azure-vm-extension.md). Der Agent bietet ausführlichere Informationen zu Ihren virtuellen Computern als die in den Speicher geschriebenen Diagnosedaten.

Sie können uns durch Ihre Stimme auf unserer [Feedbackseite](http://feedback.azure.com/forums/267889-azure-log-analytics/category/88086-log-management-and-log-collection-policy)helfen, die Prioritäten für weitere zu analysierende Protokolle für OMS zu verteilen.


- Unter [Analysieren von Azure-Diagnoseprotokollen mit Log Analytics](log-analytics-azure-storage-json.md) erfahren Sie, wie Log Analytics Protokolle von Azure-Diensten, die [Azure-Diagnoseprotokolle](../azure-portal/monitoring-overview-of-diagnostic-logs.md) unterstützen, liest:
  - Azure-Schlüsseltresor
  - Azure-Automatisierung
  - Application Gateway
  - Netzwerksicherheitsgruppen
- Unter [Verwenden von Blob Storage für IIS und Table Storage für Ereignisse](log-analytics-azure-storage-iis-table.md) erfahren Sie, wie Log Analytics die Protokolle für Azure-Dienste, die Diagnosedaten in Table Storage schreiben, oder IIS-Protokolle, die in Blob Storage geschrieben werden, liest. Dazu gehören:
  - Service Fabric
  - Webrollen
  - Workerrollen
  - Virtual Machines


Application Insights befindet sich in der privaten Vorschau und verwendet den fortlaufenden Export in Blob Storage. Wenn Sie an der privaten Vorschau teilnehmen möchten, wenden Sie sich an Ihr Microsoft-Kundenteam, oder lesen Sie die Details auf der [Feedback-Website](https://feedback.azure.com/forums/267889-log-analytics/suggestions/6519248-integration-with-app-insights).

## <a name="next-steps"></a>Nächste Schritte

- [Analysieren von Azure-Diagnoseprotokollen mit Log Analytics](log-analytics-azure-storage-json.md) enthält Informationen zum Lesen der Protokolle von Azure-Diensten, die Diagnosedaten im JSON-Format in Blob Storage schreiben.
- [Verwenden von Blob Storage für IIS und Table Storage für Ereignisse](log-analytics-azure-storage-iis-table.md) enthält Informationen zum Lesen der Protokolle für Azure-Dienste, die Diagnosedaten in Table Storage schreiben, oder der IIS-Protokolle, die in Blob Storage geschrieben werden.
- [Aktivieren Sie Lösungen](log-analytics-add-solutions.md) , um Einblick in die Daten bereitzustellen.
- [Erstellen Sie Suchabfragen](log-analytics-log-searches.md) , um die Daten zu analysieren.



<!--HONumber=Oct16_HO2-->


