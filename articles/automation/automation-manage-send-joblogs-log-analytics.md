---
title: Weiterleiten von Azure Automation-Auftragsdaten an OMS Log Analytics | Microsoft-Dokumentation
description: "In diesem Artikel wird erläutert, wie Auftragsstatus und Runbookauftragsdatenströme an Microsoft Operations Management Suite Log Analytics gesendet werden, um zusätzliche Einblicke und Verwaltungsoptionen zu erhalten."
services: automation
documentationcenter: 
author: MGoedtel
manager: jwhit
editor: tysonn
ms.assetid: c12724c6-01a9-4b55-80ae-d8b7b99bd436
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/24/2017
ms.author: magoedte
translationtype: Human Translation
ms.sourcegitcommit: 83ae00afbcbb5d3ff38ee1f934e3b2f8d1c8f624
ms.openlocfilehash: 95a6933d64428255eb061e7077c3e0768c72e207
ms.lasthandoff: 02/24/2017


---
# <a name="forward-job-status-and-job-streams-from-automation-to-log-analytics-oms"></a>Weiterleiten von Auftragsstatus und Auftragsdatenströmen von Automation an Log Analytics (OMS)
Automation kann Runbookauftragsstatus und Auftragsdatenströme an Ihren Microsoft Operations Management Suite (OMS) Log Analytics-Arbeitsbereich senden.  Auftragsprotokolle und -streams werden im Azure-Portal oder mit PowerShell für einzelne Aufträge angezeigt, d.h., Sie können einfache Untersuchen durchführen. Log Analytics bietet folgende Vorteile:

* Gewinnen Sie Einblicke in Ihre Automation-Aufträge
* Lösen Sie basierend auf Ihrem Runbookauftragsstatus (beispielsweise „Fehler“ oder „Angehalten“) das Senden einer E-Mail oder einer Warnung aus.
* Schreiben Sie erweiterte Abfragen für Ihre Auftragsdatenströme
* Korrelieren Sie Aufträge über Automation-Konten hinweg
* Visualisieren Sie Ihren Auftragsverlauf     

## <a name="prerequisites-and-deployment-considerations"></a>Voraussetzungen und Überlegungen zur Bereitstellung
Zum Senden Ihrer Automation-Protokolle an Log Analytics benötigen Sie:

1. Die [Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/)-Version von November 2016 (v2.3.0) oder höher.
2. Einen Log Analytics-Arbeitsbereich Weitere Informationen finden Sie unter [Erste Schritte mit Log Analytics](../log-analytics/log-analytics-get-started.md). 
3. Die Ressourcen-ID für Ihr Azure Automation-Konto

Um die Ressourcen-ID für Ihr Azure Automation-Konto und Ihren Log Analytics-Arbeitsbereich zu suchen, führen Sie den folgenden PowerShell-Befehl aus:

```powershell
# Find the ResourceId for the Automation Account
Find-AzureRmResource -ResourceType "Microsoft.Automation/automationAccounts"

# Find the ResourceId for the Log Analytics workspace
Find-AzureRmResource -ResourceType "Microsoft.OperationalInsights/workspaces"
```

Wenn Sie mehrere Automation-Konten oder -Arbeitsbereiche haben, suchen Sie in der Ausgabe der vorherigen Befehle den *Namen*, den Sie konfigurieren müssen, und kopieren Sie den Wert für *ResourceId*.

Wenn Sie den *Namen* Ihres Automation-Kontos suchen müssen, wählen Sie im Azure-Portal auf dem Blatt **Automation-Konto** Ihr Automation-Konto aus, und wählen Sie dann **Alle Einstellungen**.  Wählen Sie auf dem Blatt **Alle Einstellungen** unter **Kontoeinstellungen** die Option **Eigenschaften** aus.  Die Werte werden auf dem Blatt **Eigenschaften** angezeigt.<br> ![Automation-Konto – Eigenschaften](media/automation-manage-send-joblogs-log-analytics/automation-account-properties.png).

## <a name="set-up-integration-with-log-analytics"></a>Einrichten der Integration in Log Analytics
1. Starten Sie auf Ihrem Computer **Windows PowerShell** über den **Startbildschirm**.  
2. Kopieren und fügen Sie den folgenden PowerShell-Befehl ein, und bearbeiten Sie den Wert für `$workspaceId` und `$automationAccountId`.  Gültige Werte für den Parameter `-Environment` sind *AzureCloud* oder *AzureUSGovernment*, je nachdem, in welcher Cloudumgebung Sie arbeiten.     

```powershell
[cmdletBinding()]
    Param
    (
        [Parameter(Mandatory=$True)]
        [ValidateSet("AzureCloud","AzureUSGovernment")]
        [string]$Environment="AzureCloud"
    )

#Check to see which cloud environment to sign into.
Switch ($Environment)
   {
       "AzureCloud" {Login-AzureRmAccount}
       "AzureUSGovernment" {Login-AzureRmAccount -EnvironmentName AzureUSGovernment} 
   }

# if you have one Log Analytics workspace you can use the following command to get the resource id of the workspace
$workspaceId = (Get-AzureRmOperationalInsightsWorkspace).ResourceId

$automationAccountId = "/SUBSCRIPTIONS/ec11ca60-1234-491e-5678-0ea07feae25c/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.AUTOMATION/ACCOUNTS/DEMO" 

Set-AzureRmDiagnosticSetting -ResourceId $automationAccountId -WorkspaceId $workspaceId -Enabled $true

```

Nach dem Ausführen dieses Skripts werden in Log Analytics innerhalb von zehn Minuten Datensätze neuer Auftragsprotokolle oder -streams angezeigt, die geschrieben werden.

Führen Sie die folgende Abfrage aus, um die Protokolle anzuzeigen: `Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION"`

### <a name="verify-configuration"></a>Überprüfen der Konfiguration
Zum Sicherstellen, dass Ihr Automation-Konto Protokolle an Ihren Log Analytics-Arbeitsbereich sendet, überprüfen Sie mit folgendem PowerShell-Befehl, ob die Diagnose für das Automation-Konto ordnungsgemäß festgelegt ist:

```powershell
[cmdletBinding()]
    Param
    (
        [Parameter(Mandatory=$True)]
        [ValidateSet("AzureCloud","AzureUSGovernment")]
        [string]$Environment="AzureCloud"
    )

#Check to see which cloud environment to sign into.
Switch ($Environment)
   {
       "AzureCloud" {Login-AzureRmAccount}
       "AzureUSGovernment" {Login-AzureRmAccount -EnvironmentName AzureUSGovernment} 
   }
# if you have one Log Analytics workspace you can use the following command to get the resource id of the workspace
$workspaceId = (Get-AzureRmOperationalInsightsWorkspace).ResourceId

$automationAccountId = "/SUBSCRIPTIONS/ec11ca60-1234-491e-5678-0ea07feae25c/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.AUTOMATION/ACCOUNTS/DEMO" 

Get-AzureRmDiagnosticSetting -ResourceId $automationAccountId
```

Überprüfen Sie in der Ausgabe Folgendes:
+ Der Wert für *Aktiviert* unter *Protokolle* ist *True*.
+ Der Wert für *WorkspaceId* ist auf die Ressourcen-ID Ihres Log Analytics-Arbeitsbereichs festgelegt.


## <a name="log-analytics-records"></a>Log Analytics-Datensätze
Die Diagnose von Azure Automation erstellt zwei Typen von Datensätzen in Log Analytics. 

### <a name="job-logs"></a>Auftragsprotokolle
| Eigenschaft | Beschreibung |
| --- | --- |
| TimeGenerated |Ausführungsdatum und -uhrzeit des Runbookauftrags. |
| RunbookName_s |Der Name des Runbooks. |
| Caller_s |Der Benutzer oder das System, von dem der Vorgang initiiert wurde.  Mögliche Werte sind entweder eine E-Mail-Adresse oder, bei geplanten Aufträgen, ein System. |
| Tenant_g | GUID, die den Mandanten für den Aufrufer identifiziert. |
| JobId_g |Die GUID, bei der es sich um die ID des Runbookauftrags handelt. |
| ResultType |Der Status des Runbookauftrags.  Mögliche Werte:<br>- Gestartet<br>- Beendet<br>- Ausgesetzt<br>- Fehler<br>- Erfolgreich |
| Kategorie | Klassifizierung des Datentyps.  Für Automation lautet der Wert „JobLogs“. |
| NameVorgang | Gibt den Typ des in Azure ausgeführten Vorgangs an.  Für Automation lautet der Wert „Job“. |
| Ressource | Name des Automation-Kontos |
| SourceSystem | So wurden die Daten von Log Analytics gesammelt. Immer *Azure* für Azure-Diagnose. |
| ResultDescription |Beschreibt den resultierenden Zustand des Runbookauftrags.  Mögliche Werte:<br>- Auftrag gestartet<br>- Fehler beim Ausführen des Auftrags<br>- Auftrag abgeschlossen |
| CorrelationId |Die GUID, bei der es sich um die Korrelations-ID des Runbookauftrags handelt. |
| ResourceId |Gibt die Ressourcen-ID des Runbooks für das Azure Automation-Konto an. |
| SubscriptionId | Die Azure-Abonnement-ID (GUID) für das Automation-Konto. |
| ResourceGroup | Name der Ressourcengruppe für das Automation-Konto. |
| ResourceProvider | MICROSOFT.AUTOMATION |
| ResourceType | AUTOMATIONACCOUNTS |


### <a name="job-streams"></a>Auftragsdatenströme
| Eigenschaft | Beschreibung |
| --- | --- |
| TimeGenerated |Ausführungsdatum und -uhrzeit des Runbookauftrags. |
| RunbookName_s |Der Name des Runbooks. |
| Caller_s |Der Benutzer oder das System, von dem der Vorgang initiiert wurde.  Mögliche Werte sind entweder eine E-Mail-Adresse oder, bei geplanten Aufträgen, ein System. |
| StreamType_s |Der Typ des Auftragsstreams. Mögliche Werte:<br>- Status<br>- Ausgabe<br>- Warnung<br>- Fehler<br>- Debuggen<br>- Ausführlich |
| Tenant_g | GUID, die den Mandanten für den Aufrufer identifiziert. |
| JobId_g |Die GUID, bei der es sich um die ID des Runbookauftrags handelt. |
| ResultType |Der Status des Runbookauftrags.  Mögliche Werte:<br>– In Bearbeitung |
| Kategorie | Klassifizierung des Datentyps.  Für Automation lautet der Wert „JobStreams“. |
| NameVorgang | Gibt den Typ des in Azure ausgeführten Vorgangs an.  Für Automation lautet der Wert „Job“. |
| Ressource | Name des Automation-Kontos |
| SourceSystem | So wurden die Daten von Log Analytics gesammelt. Immer *Azure* für Azure-Diagnose. |
| ResultDescription |Enthält den Ausgabedatenstrom des Runbooks. |
| CorrelationId |Die GUID, bei der es sich um die Korrelations-ID des Runbookauftrags handelt. |
| ResourceId |Gibt die Ressourcen-ID des Runbooks für das Azure Automation-Konto an. |
| SubscriptionId | Die Azure-Abonnement-ID (GUID) für das Automation-Konto. |
| ResourceGroup | Name der Ressourcengruppe für das Automation-Konto. |
| ResourceProvider | MICROSOFT.AUTOMATION |
| ResourceType | AUTOMATIONACCOUNTS |

## <a name="viewing-automation-logs-in-log-analytics"></a>Anzeigen von Automation-Protokollen in Log Analytics
Nachdem Sie begonnen haben, Ihre Automation-Auftragsprotokolle an Log Analytics zu senden, sehen wir uns nun an, wie Sie diese Protokolle in Log Analytics verwenden können.

Führen Sie die folgende Abfrage aus, um die Protokolle anzuzeigen: `Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION"`

### <a name="send-an-email-when-a-runbook-job-fails-or-suspends"></a>Senden einer E-Mail, wenn ein Runbookauftrag mit einem Fehler abgebrochen oder angehalten wird
Eine der häufigsten Anfragen unserer Kunden betrifft die Möglichkeit, eine E-Mail oder eine SMS zu senden, wenn es bei einem Runbookauftrag ein Problem gibt.   

Zum Erstellen einer Warnungsregel erstellen Sie zunächst eine Protokollsuche für die Datensätze des Runbookauftrags, die die Warnung auslösen sollen.  Klicken Sie auf die Schaltfläche **Warnung**, um die Warnungsregel zu erstellen und zu konfigurieren.

1. Klicken Sie auf der Seite mit der Log Analytics-Übersicht auf **Protokollsuche**.
2. Erstellen Sie eine Protokollsuchabfrage für die Warnung, indem Sie folgenden Suchtext in das Abfragefeld eingeben: `Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobLogs (ResultType=Failed OR ResultType=Suspended)` Sie können auch nach dem Runbook-Namen gruppieren, indem Sie folgende Abfrage verwenden: `Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobLogs (ResultType=Failed OR ResultType=Suspended) | measure Count() by RunbookName_s`   

   Wenn Sie Protokolle von mehreren Automation-Konten oder Abonnements in Ihrem Arbeitsbereich eingerichtet haben, können Sie Ihre Warnungen nach Abonnement oder Automation-Konto gruppieren.  Der Name des Automation-Kontos kann vom Ressourcenfeld in der JobLogs-Suche abgeleitet werden.  
3. Klicken Sie oben auf der Seite auf **Warnung**, um den Bildschirm **Warnungsregel hinzufügen** zu öffnen. Weitere Informationen zu den Konfigurationsoptionen für Warnungen finden Sie unter [Warnungen in Log Analytics](../log-analytics/log-analytics-alerts.md#creating-an-alert-rule).

### <a name="find-all-jobs-that-have-completed-with-errors"></a>Durchführen einer Suche nach allen Aufträgen, die mit Fehlern abgeschlossen wurden
Neben dem Erstellen von Warnungen für Fehler können Sie auch ermitteln, wenn ein Runbook-Auftrag einen Fehler ohne Abbruch aufweist. In diesen Fällen erzeugt PowerShell einen Fehlerdatenstrom, die Fehler ohne Abbruch verursachen jedoch kein Anhalten oder Fehlschlagen Ihres Auftrags.    

1. Klicken Sie in Ihrem Log Analytics-Arbeitsbereich auf **Protokollsuche**.
2. Geben Sie in das Abfragefeld `Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobStreams StreamType_s=Error | measure count() by JobId_g` ein und klicken Sie dann auf **Search**.

### <a name="view-job-streams-for-a-job"></a>Anzeigen von Auftragsdatenströmen für einen Auftrag
Wenn Sie einen Auftrag debuggen, sollten Sie möglicherweise auch einen Blick in die Auftragsdatenströme werfen.  Mit der folgenden Abfrage werden alle Datenströme für einen einzelnen Auftrag mit der GUID „2ebd22ea-e05e-4eb9-9d76-d73cbd4356e0“ abgerufen:   

`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobStreams JobId_g="2ebd22ea-e05e-4eb9-9d76-d73cbd4356e0" | sort TimeGenerated | select ResultDescription`

### <a name="view-historical-job-status"></a>Anzeigen des Auftragsstatusverlaufs
Abschließend möchten Sie möglicherweise Ihren Auftragsverlauf visualisieren.  Sie können die folgende Abfrage verwenden, um nach dem Statusverlauf Ihrer Aufträge zu suchen.

`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobLogs NOT(ResultType="started") | measure Count() by ResultType interval 1hour`  
<br> ![Diagramm zum Auftragsstatusverlauf in OMS](media/automation-manage-send-joblogs-log-analytics/historical-job-status-chart.png)<br>

## <a name="summary"></a>Zusammenfassung
Sie erhalten einen besseren Einblick in den Status Ihrer Automation-Aufträge, wenn Sie den Status und die Streamdaten Ihres Automation-Auftrags an Log Analytics senden und:
+ Warnungen einrichten, um bei Problemen benachrichtigt zu werden
+ Benutzerdefinierte Ansichten und Suchabfragen zum Anzeigen von Runbook-Ergebnissen, Runbook-Auftragsstatus und anderer wichtiger Schlüsselindikatoren oder Metriken verwenden.  

Log Analytics bietet eine höhere operative Transparenz für Ihre Automation-Aufträge, sodass schneller auf Vorfälle reagiert werden kann.  

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zum Erstellen verschiedener Suchabfragen und zur Überprüfung der Automation-Auftragsprotokolle mit Log Analytics finden Sie unter [Protokollsuchen in Log Analytics](../log-analytics/log-analytics-log-searches.md)
* Unter [Runbookausgabe und -meldungen](automation-runbook-output-and-messages.md)
* Weitere Informationen zum Ausführen von Runbooks, zum Überwachen von Runbookaufträgen sowie andere technische Details finden Sie unter [Verfolgen eines Runbookauftrags](automation-runbook-execution.md)
* Weitere Informationen zu OMS Log Analytics und Datenerfassungsquellen finden Sie unter [Herstellen einer Verbindung zwischen Azure-Speichern und Log Analytics](../log-analytics/log-analytics-azure-storage.md)

