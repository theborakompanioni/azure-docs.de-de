---
title: Weiterleiten von Azure Automation DSC-Berichtsdaten an OMS Log Analytics | Microsoft-Dokumentation
description: "In diesem Artikel wird erläutert, wie Berichtsdaten zum gewünschten Zustand (Desired State Configuration, DSC) an Microsoft Operations Management Suite Log Analytics gesendet werden, um zusätzliche Einblicke und Verwaltungsoptionen zu erhalten."
services: automation
documentationcenter: 
author: eslesar
manager: carmonm
editor: tysonn
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/24/2017
ms.author: eslesar
ms.translationtype: Human Translation
ms.sourcegitcommit: 67ee6932f417194d6d9ee1e18bb716f02cf7605d
ms.openlocfilehash: 316031c5297a0201c8db4a9e177298c78962c673
ms.contentlocale: de-de
ms.lasthandoff: 05/26/2017


---
# <a name="forward-azure-automation-dsc-reporting-data-to-oms-log-analytics"></a>Weiterleiten von Azure Automation DSC-Berichtsdaten an OMS Log Analytics

Automation kann den DSC-Knotenstatus an Ihren Microsoft Operations Management Suite (OMS) Log Analytics-Arbeitsbereich senden.  
Der Konformitätsstatus kann im Azure-Portal oder mit PowerShell angezeigt werden, und zwar für Knoten und einzelne DSC-Ressourcen in Knotenkonfigurationen. Log Analytics bietet folgende Vorteile:

* Abrufen von Konformitätsinformationen für verwaltete Knoten und einzelne Ressourcen
* Auslösen einer E-Mail oder Warnung basierend auf dem Konformitätsstatus
* Schreiben erweiterter Abfragen für Ihre verwalteten Knoten
* Korrelieren des Konformitätsstatus über Automation-Konten
* Visualisieren des Konformitätsverlaufs von Knoten über einen Zeitraum

## <a name="prerequisites"></a>Voraussetzungen

Zum Senden Ihrer Automation DSC-Berichte an Log Analytics benötigen Sie Folgendes:

* Die [Azure PowerShell](/powershell/azure/overview)-Version von November 2016 (v2.3.0) oder höher.
* Ein Azure Automation-Konto. Weitere Informationen finden Sie unter [Erste Schritte mit Azure Automation](automation-offering-get-started.md).
* Einen Log Analytics-Arbeitsbereich mit dem Dienstangebot **Automation & Control**. Weitere Informationen finden Sie unter [Erste Schritte mit Log Analytics](../log-analytics/log-analytics-get-started.md).
* Mindestens einen Azure Automation DSC-Knoten. Weitere Informationen finden Sie unter [Integrieren von Computern für die Verwaltung durch Azure Automation DSC](automation-dsc-onboarding.md). 

## <a name="set-up-integration-with-log-analytics"></a>Einrichten der Integration in Log Analytics

Zum Importieren von Daten aus Azure Automation DSC in Log Analytics müssen Sie die folgenden Schritte ausführen:

1. Melden Sie sich in PowerShell bei Ihrem Azure-Konto an. Weitere Informationen finden Sie unter [Anmelden mit Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/authenticate-azureps?view=azurermps-4.0.0).
1. Rufen Sie die _ResourceId_ Ihres Automation-Kontos ab, indem Sie den folgenden PowerShell-Befehl ausführen: (Wenn Sie über mehrere Automation-Konten verfügen, wählen Sie die _ResourceID_ für das Konto aus, das Sie konfigurieren möchten.)

  ```powershell
  # Find the ResourceId for the Automation Account
  Find-AzureRmResource -ResourceType "Microsoft.Automation/automationAccounts"
  ```
1. Rufen Sie die _ResourceId_ Ihres Log Analytics-Arbeitsbereichs ab, indem Sie den folgenden PowerShell-Befehl ausführen: (Wenn Sie über mehrere Arbeitsbereiche verfügen, wählen Sie die _ResourceID_ für den Arbeitsbereich aus, den Sie konfigurieren möchten.)

  ```powershell
  # Find the ResourceId for the Log Analytics workspace
  Find-AzureRmResource -ResourceType "Microsoft.OperationalInsights/workspaces"
  ```
1. Führen Sie den folgenden PowerShell-Befehl aus, und ersetzen Sie `<AutomationResourceId>` und `<WorkspaceResourceId>` durch die _ResourceId_-Werte aus den vorherigen Schritten:

  ```powershell
  Set-AzureRmDiagnosticSetting -ResourceId <AutomationResourceId> -WorkspaceId <WorkspaceResourceId> -Enabled $true -Categories "DscNodeStatus"
  ```

Wenn Sie das Importieren von Daten aus Azure Automation DSC in Log Analytics beenden möchten, führen Sie den folgenden PowerShell-Befehl aus.

```powershell
Set-AzureRmDiagnosticSetting -ResourceId <AutomationResourceId> -WorkspaceId <WorkspaceResourceId> -Enabled $false -Categories "DscNodeStatus"
```

## <a name="view-the-dsc-logs"></a>Anzeigen der DSC-Protokolle

Nachdem Sie die Integration in Log Analytics für Ihre Automation DSC-Daten eingerichtet haben, wird die Schaltfläche **Protokollsuche** auf dem Blatt **DSC-Knoten** Ihres Automation-Kontos angezeigt. Klicken Sie auf die Schaltfläche **Protokollsuche**, um die Protokolle für DSC-Knotendaten anzuzeigen.

![Schaltfläche „Protokollsuche“](media/automation-dsc-diagnostics/log-search-button.png)

Das Blatt **Protokollsuche** wird geöffnet. Dort wird ein Vorgang **DscNodeStatusData** für jeden DSC-Knoten und ein Vorgang **DscResourceStatusData** für jede [DSC-Ressource](https://msdn.microsoft.com/powershell/dsc/resources), die in der Konfiguration des Knotens aufgerufen wird, angezeigt.

Der Vorgang **DscResourceStatusData** enthält Fehlerinformationen für fehlerhafte DSC-Ressourcen.

Klicken Sie auf jeden Vorgang in der Liste, um die Daten für diesen Vorgang anzuzeigen.

Sie können die Protokolle auch anzeigen, indem Sie in Log Analytics eine Suche durchführen. Weitere Informationen finden Sie unter [Suchen von Daten mithilfe der Protokollsuche](../log-analytics/log-analytics-log-searches.md).
Geben Sie die folgende Abfrage ein, um die DSC-Protokolle zu suchen: `Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category = "DscNodeStatus"`

Sie können die Abfrage auch anhand des Vorgangsnamens eingrenzen. Beispiel: Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category = "DscNodeStatus" OperationName = "DscNodeStatusData"

### <a name="send-an-email-when-a-dsc-compliance-check-fails"></a>Senden einer E-Mail bei einem Fehler während der DSC-Konformitätsüberprüfung

Eine der häufigsten Anfragen unserer Kunden betrifft die Möglichkeit, eine E-Mail oder Textnachricht zu senden, wenn bei einer DSC-Konfiguration ein Problem auftritt.   

Beim Erstellen einer Warnungsregel beginnen Sie mit dem Erstellen einer Protokollsuche für die DSC-Berichtsdatensätze, von denen die Warnung aufgerufen werden soll.  Klicken Sie auf die Schaltfläche **Warnung**, um die Warnungsregel zu erstellen und zu konfigurieren.

1. Klicken Sie auf der Seite mit der Log Analytics-Übersicht auf **Protokollsuche**.
1. Erstellen Sie eine Protokollsuchabfrage für Ihre Warnung, indem Sie folgenden Text in das Abfragefeld eingeben: `Type=AzureDiagnostics Category=DscNodeStatus NodeName_s=DSCTEST1 OperationName=DscNodeStatusData ResultType=Failed`.

  Wenn Sie Protokolle von mehreren Automation-Konten oder Abonnements in Ihrem Arbeitsbereich eingerichtet haben, können Sie Ihre Warnungen nach Abonnement oder Automation-Konto gruppieren.  
  Der Name des Automation-Kontos kann vom Ressourcenfeld in der DscNodeStatusData-Suche abgeleitet werden.  
1. Klicken Sie oben auf der Seite auf **Warnung**, um den Bildschirm **Warnungsregel hinzufügen** zu öffnen. Weitere Informationen zu den Konfigurationsoptionen für Warnungen finden Sie unter [Warnungen in Log Analytics](../log-analytics/log-analytics-alerts.md#alert-rules).

### <a name="find-failed-dsc-resources-across-all-nodes"></a>Suchen von Fehlern bei DSC-Ressourcen in allen Knoten

Ein Vorteil der Verwendung von Log Analytics ist, dass Sie nach Fehlern bei der Überprüfung der Knoten suchen können.
So finden Sie alle Instanzen von DSC-Ressourcen mit Fehlern

1. Klicken Sie auf der Seite mit der Log Analytics-Übersicht auf **Protokollsuche**.
1. Erstellen Sie eine Protokollsuchabfrage für Ihre Warnung, indem Sie folgenden Text in das Abfragefeld eingeben: `Type=AzureDiagnostics Category=DscNodeStatus OperationName=DscResourceStatusData ResultType=Failed`.

### <a name="view-historical-dsc-node-status"></a>Anzeigen von Verlaufsdaten zum DSC-Knotenstatus

Abschließend möchten Sie möglicherweise den Statusverlauf Ihre DSC-Knotens visualisieren.  
Sie können die folgende Abfrage verwenden, um nach dem Statusverlauf Ihres DSC-Knotens zu suchen.

`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=DscNodeStatus NOT(ResultType="started") | measure Count() by ResultType interval 1hour`  

Dadurch wird ein Diagramm mit dem Knotenstatus über einen Zeitraum angezeigt.

## <a name="log-analytics-records"></a>Log Analytics-Datensätze

Die Diagnose von Azure Automation erstellt zwei Kategorien von Datensätzen in Log Analytics.

### <a name="dscnodestatusdata"></a>DscNodeStatusData

| Eigenschaft | Beschreibung |
| --- | --- |
| TimeGenerated |Datum und Uhrzeit der Durchführung der Konformitätsprüfung. |
| NameVorgang |DscNodeStatusData |
| ResultType |Gibt an, ob der Knoten konform ist |
| NodeName_s |Der Name des verwalteten Knotens. |
| NodeComplianceStatus_s |Gibt an, ob der Knoten konform ist |
| DscReportStatus |Gibt an, ob die Konformitätsüberprüfung erfolgreich ausgeführt wurde |
| ConfigurationMode | Gibt an, wie die Konfiguration auf den Knoten angewendet wird. Mögliche Werte sind __ApplyOnly__, __ApplyandMonitior__ und __ApplyandAutoCorrect__. <ul><li>__ApplyOnly:__ DSC wendet die Konfiguration an und führt nur dann weitere Schritte durch, wenn eine neue Konfiguration per Push an den Zielknoten übertragen oder eine neue Konfiguration von einem Server abgerufen wird. Nach der ersten Anwendung einer neuen Konfiguration führt DSC keine Überprüfung auf Abweichungen von einem zuvor konfigurierten Zustand durch. DSC versucht, die Konfiguration anzuwenden, bis der Vorgang erfolgreich abgeschlossen wurde und bevor __ApplyOnly__ in Kraft tritt. </li><li> __ApplyAndMonitor:__ Dies ist der Standardwert. Der LCM wendet alle neuen Konfigurationen an. Nach der ersten Anwendung einer neuen Konfiguration meldet DSC Abweichungen in Protokollen, wenn der Zielknoten vom gewünschten Zustand abweicht. DSC versucht, die Konfiguration anzuwenden, bis der Vorgang erfolgreich abgeschlossen wurde und bevor __ApplyAndMonitor__ in Kraft tritt.</li><li>__ApplyAndAutoCorrect:__ DSC wendet alle neuen Konfigurationen an. Nach der ersten Anwendung einer neuen Konfiguration meldet DSC Abweichungen in Protokollen, wenn der Zielknoten vom gewünschten Zustand abweicht, und wendet dann die aktuelle Konfiguration erneut an.</li></ul> |
| HostName_s | Der Name des verwalteten Knotens. |
| IPAddress | Die IPv4-Adresse des verwalteten Knotens. |
| Kategorie | DscNodeStatus |
| Ressource | Der Name des Azure Automation-Kontos. |
| Tenant_g | GUID, die den Mandanten für den Aufrufer identifiziert. |
| NodeId_g |Eindeutiger Bezeichner (GUID), der den verwalteten Knoten identifiziert. |
| DscReportId_g |Eindeutiger Bezeichner (GUID), der den Bericht identifiziert. |
| LastSeenTime_t |Datum und Uhrzeit der letzten Anzeige des Berichts. |
| ReportStartTime_t |Datum und Uhrzeit des Berichtsstarts. |
| ReportEndTime_t |Datum und Uhrzeit des Berichtsabschlusses. |
| NumberOfResources_d |Die Anzahl der DSC-Ressourcen, die bei der Anwendung der Konfiguration auf den Knoten abgerufen wurden. |
| SourceSystem | So wurden die Daten von Log Analytics gesammelt. Immer *Azure* für Azure-Diagnose. |
| ResourceId |Gibt das Azure Automation-Konto an. |
| ResultDescription | Die Beschreibung für diesen Vorgang. |
| SubscriptionId | Die Azure-Abonnement-ID (GUID) für das Automation-Konto. |
| ResourceGroup | Name der Ressourcengruppe für das Automation-Konto. |
| ResourceProvider | MICROSOFT.AUTOMATION |
| ResourceType | AUTOMATIONACCOUNTS |
| CorrelationId |Eindeutiger Bezeichner (GUID), bei dem es sich um die Korrelations-ID des Konformitätsberichts handelt. |

### <a name="dscresourcestatusdata"></a>DscResourceStatusData

| Eigenschaft | Beschreibung |
| --- | --- |
| TimeGenerated |Datum und Uhrzeit der Durchführung der Konformitätsprüfung. |
| NameVorgang |DscResourceStatusData|
| ResultType |Gibt an, ob die Ressource konform ist. |
| NodeName_s |Der Name des verwalteten Knotens. |
| Kategorie | DscNodeStatus |
| Ressource | Der Name des Azure Automation-Kontos. |
| Tenant_g | GUID, die den Mandanten für den Aufrufer identifiziert. |
| NodeId_g |Eindeutiger Bezeichner (GUID), der den verwalteten Knoten identifiziert. |
| DscReportId_g |Eindeutiger Bezeichner (GUID), der den Bericht identifiziert. |
| DscResourceId_s |Der Name der DSC-Ressourceninstanz. |
| DscResourceName_s |Der Name der DSC-Ressource. |
| DscResourceStatus_s |Gibt an, ob die DSC-Ressource konform ist. |
| DscModuleName_s |Der Name des PowerShell-Moduls, das die DSC-Ressource enthält. |
| DscModuleVersion_s |Die Version des PowerShell-Moduls, das die DSC-Ressource enthält. |
| DscConfigurationName_s |Der Name der Konfiguration, die auf den Knoten angewendet wird. |
| ErrorCode_s | Der Fehlercode, wenn die Ressource fehlerhaft ist. |
| ErrorMessage_s |Die Fehlermeldung, wenn die Ressource fehlerhaft ist. |
| DscResourceDuration_d |Die Zeit in Sekunden, für die die DSC-Ressource ausgeführt wurde. |
| SourceSystem | So wurden die Daten von Log Analytics gesammelt. Immer *Azure* für Azure-Diagnose. |
| ResourceId |Gibt das Azure Automation-Konto an. |
| ResultDescription | Die Beschreibung für diesen Vorgang. |
| SubscriptionId | Die Azure-Abonnement-ID (GUID) für das Automation-Konto. |
| ResourceGroup | Name der Ressourcengruppe für das Automation-Konto. |
| ResourceProvider | MICROSOFT.AUTOMATION |
| ResourceType | AUTOMATIONACCOUNTS |
| CorrelationId |Eindeutiger Bezeichner (GUID), bei dem es sich um die Korrelations-ID des Konformitätsberichts handelt. |

## <a name="summary"></a>Zusammenfassung

Sie erhalten einen besseren Einblick in Ihre Automation DSC-Knoten, wenn Sie die Automation DSC-Daten an Log Analytics senden und:

* Warnungen einrichten, um bei Problemen benachrichtigt zu werden
* Benutzerdefinierte Ansichten und Suchabfragen zum Anzeigen von Runbook-Ergebnissen, Runbook-Auftragsstatus und anderer wichtiger Schlüsselindikatoren oder Metriken verwenden.  

Log Analytics bietet eine höhere operative Transparenz für Ihre Automation DSC-Daten, sodass schneller auf Vorfälle reagiert werden kann.  

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zum Erstellen verschiedener Suchabfragen und zur Überprüfung der Automation DSC-Protokolle mit Log Analytics finden Sie unter [Protokollsuchen in Log Analytics](../log-analytics/log-analytics-log-searches.md).
* Weitere Informationen zur Verwendung von Azure Automation DSC finden Sie unter [Erste Schritte mit Azure Automation DSC](automation-dsc-getting-started.md).
* Weitere Informationen zu OMS Log Analytics und Datenerfassungsquellen finden Sie unter [Herstellen einer Verbindung zwischen Azure-Speichern und Log Analytics](../log-analytics/log-analytics-azure-storage.md)


