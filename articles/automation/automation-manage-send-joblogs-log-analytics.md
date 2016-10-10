<properties
    pageTitle="Weiterleiten von Auftragsstatus und Auftragsdatenströmen von Automation an Log Analytics (OMS) | Microsoft Azure"
    description="In diesem Artikel wird erläutert, wie Auftragsstatus und Runbookauftragsdatenströme an Microsoft Operations Management Suite Log Analytics gesendet werden, um zusätzliche Einblicke und Verwaltungsoptionen zu erhalten."
    services="automation"
    documentationCenter=""
    authors="MGoedtel"
    manager="jwhit"
    editor="tysonn" />
<tags
    ms.service="automation"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/22/2016"
    ms.author="magoedte" />

# Weiterleiten von Auftragsstatus und Auftragsdatenströmen von Automation an Log Analytics (OMS)

Automation kann Runbookauftragsstatus und Auftragsdatenströme an Ihren Microsoft Operations Management Suite (OMS) Log Analytics-Arbeitsbereich senden. Diese Informationen können im Azure-Portal oder mit PowerShell angezeigt werden (einzelne Auftragsstatus oder alle Aufträge für ein bestimmtes Automation-Konto). Um Ihre erweiterten betrieblichen Anforderungen zu erfüllen, müssen jedoch benutzerdefinierte PowerShell-Skripts erstellt werden. Log Anaytics bietet nun folgende Vorteile:

- Gewinnen Sie Einblicke in Ihre Automation-Aufträge
- Lösen Sie basierend auf Ihrem Runbookauftragsstatus (beispielsweise „Fehler“ oder „Angehalten“) das Senden einer E-Mail oder einer Warnung aus
- Schreiben Sie erweiterte Abfragen für Ihre Auftragsdatenströme
- Korrelieren Sie Aufträge über Automation-Konten hinweg
- Visualisieren Sie Ihren Auftragsverlauf

## Voraussetzungen und Überlegungen zur Bereitstellung

Um Ihre Automation-Protokolle an Log Analytics zu senden, benötigen Sie Folgendes:

1. Ein OMS-Abonnement. Weitere Informationen finden Sie unter [Erste Schritte mit Log Analytics](../log-analytics/log-analytics-get-started.md).

    >[AZURE.NOTE]Damit diese Konfiguration ordnungsgemäß funktioniert, müssen der OMS-Arbeitsbereich und das Automation-Konto im selben Azure-Abonnement enthalten sein.
  
2. Ein [Azure Storage-Konto](../storage/storage-create-storage-account.md)
   
    >[AZURE.NOTE]Das Storage-Konto *muss* sich in derselben Region befinden wie das Automation-Konto.
 
3. Azure PowerShell mit Version 1.0.8 oder höher der Operational Insights-Cmdlets. Informationen zu diesem Release und zur Installation finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).
4. Azure-Diagnose und Log Analytics PowerShell. Weitere Informationen zu diesem Release und zur Installation finden Sie unter [Azure-Diagnose und Log Analytics](https://www.powershellgallery.com/packages/AzureDiagnosticsAndLogAnalytics/0.1).
5. Laden Sie das PowerShell-Skript **Enable-AzureDiagnostics.ps1** aus dem [PowerShell-Katalog](https://www.powershellgallery.com/packages/Enable-AzureDiagnostics/1.0/DisplayScript) herunter. Dieses Skript konfiguriert Folgendes:
 - Ein Speicherkonto, in dem die Runbookauftragsstatus und die Datenstromdaten für ein von Ihnen angegebenes Automation-Konto gespeichert werden.
 - Aktivierung der Erfassung dieser Daten aus Ihrem Automation-Konto, um die Daten im JSON-Format in einem Azure Blob Storage-Konto zu speichern.
 - Konfiguration der Erfassung von Daten aus Ihrem Blob Storage-Konto und der Weitergabe an OMS Log Analytics.
 - Aktivieren der Automation Log Analytics-Lösung in Ihrem OMS-Arbeitsbereich.

Das Skript **Enable-AzureDiagnostics.ps1** erfordert während seiner Ausführung folgende Parameter:

- *AutomationAccountName*: der Name Ihres Automation-Kontos
- *LogAnalyticsWorkspaceName*: der Name Ihres OMS-Arbeitsbereichs

Wählen Sie im Azure-Portal auf dem Blatt **Automation-Konto** Ihr Automation-Konto aus, und wählen Sie dann **Alle Einstellungen** aus, um die Werte für *AutomationAccountName* anzuzeigen. Wählen Sie auf dem Blatt **Alle Einstellungen** unter **Kontoeinstellungen** die Option **Eigenschaften** aus. Die Werte werden auf dem Blatt **Eigenschaften** angezeigt.<br> ![Automation-Konto – Eigenschaften](media/automation-manage-send-joblogs-log-analytics/automation-account-properties.png)


## Einrichten der Integration in Log Analytics

1. Starten Sie auf Ihrem Computer über den **Startbildschirm** **Windows PowerShell**.
2. Navigieren Sie von der PowerShell-Befehlszeilenshell zu dem Ordner mit dem heruntergeladen Skript, und führen Sie es aus. Ändern Sie dabei die Werte für die Parameter *-AutomationAccountName* und *-LogAnalyticsWorkspaceName*.

    >[AZURE.NOTE] Nach dem Ausführen des Skripts werden Sie aufgefordert, sich gegenüber Azure zu authentifizieren. Sie **müssen** sich mit einem Konto anmelden, das Mitglied der Rolle „Abonnement-Administratoren“ und Co-Administrator des Abonnements ist.
    
        .\Enable-AzureDiagnostics -AutomationAccountName <NameofAutomationAccount> `
        -LogAnalyticsWorkspaceName <NameofOMSWorkspace> `

3. Nach der Ausführung dieses Skripts sollten Sie etwa 30 Minuten, nachdem neue Diagnosedaten in den Speicher geschrieben worden sind, in Log Analytics Einträge sehen. Wenn nach diesem Zeitraum keine Datensätze verfügbar sind, lesen Sie den Abschnitt zur Problembehandlung im Artikel [JSON-Dateien in Blob Storage](../log-analytics/log-analytics-azure-storage-json.md#troubleshooting-configuration-for-azure-diagnostics-written-to-blob-in-json), um weitere Informationen zu erhalten.

### Überprüfen der Konfiguration

Um zu bestätigen, dass das Skript Ihr Automation-Konto und Ihren OMS-Arbeitsbereich erfolgreich konfiguriert hat, können Sie in PowerShell folgende Schritte ausführen. Bevor Sie mit diesen Schritten beginnen, navigieren Sie im Azure-Portal zu Log Analytics (OMS), um die Werte für Ihren OMS-Arbeitsbereichsnamen und den Ressourcengruppennamen zu ermitteln. Auf dem Blatt „Log Analytics (OMS)“ finden Sie die Werte für **Name** und **Ressourcengruppe**.<br> ![OMS Log Analytics-Arbeitsbereichsliste](media/automation-manage-send-joblogs-log-analytics/oms-la-workspaces-list-blade.png) Diese beiden Werte werden verwendet, um die Konfiguration in Ihrem OMS-Arbeitsbereich mithilfe des PowerShell-Cmdlets [Get-AzureRmOperationalInsightsStorageInsight](https://msdn.microsoft.com/library/mt603567.aspx) zu überprüfen.

1.  Navigieren Sie im Azure-Portal zu den Speicherkonten, und suchen Sie nach folgendem Speicherkonto, bei dem die Namenskonvention befolgt wird: *AutomationAccountNameomsstorage*. Kurz nach Abschluss eines Runbookauftrags sollten Sie sehen, dass zwei Blobcontainer erstellt werden: **insights-logs-joblogs** und **insights-logs-jobstreams**.

2.  Führen Sie in PowerShell den folgenden PowerShell-Code aus, wobei Sie die Werte für die Parameter **ResourceGroupName** und **WorkspaceName** ändern, die Sie zuvor kopiert oder notiert haben.

    Login-AzureRmAccount Get-AzureRmSubscription -SubscriptionName „Abonnementname“ | Set-AzureRmContext Get-AzureRmOperationalInsightsStorageInsight -ResourceGroupName „OMS-Ressourcengruppenname“ ` -Workspace „OMS-Arbeitsbereichsname“

    Dieser Befehl gibt Speicherdetails für den angegebenen OMS-Arbeitsbereich zurück. Es soll überprüft werden, ob die Speicherdetails für das Automation-Konto vorhanden sind, die zuvor angegeben wurden, und ob der Wert des **State**-Objekts **OK** lautet.<br> ![Ergebnisse des Cmdlets „Get-AzureRmOperationalInsightsStorageInsights“](media/automation-manage-send-joblogs-log-analytics/automation-posh-getstorageinsights-results.png).


## Log Analytics-Datensätze

Automation erstellt zwei Arten von Datensätzen im OMS-Repository.

### Auftragsprotokolle

Eigenschaft | Beschreibung|
----------|----------|
Time | Datum und Uhrzeit der Ausführung des Runbookauftrags.|
resourceId | Gibt den Ressourcentyp in Azure an. Für Automation ist der Wert das dem Runbook zugeordnete Automation-Konto.|
operationName | Gibt den Typ des in Azure ausgeführten Vorgangs an. Für Automation ist der Wert der Auftrag.|
resultType | Der Status des Runbookauftrags. Mögliche Werte:<br>- Gestartet<br>- Beendet<br>- Angehalten<br>- Fehler<br>- Erfolgreich|
resultDescription | Beschreibt den resultierenden Zustand des Runbookauftrags. Mögliche Werte:<br>- Auftrag gestartet<br>- Fehler beim Ausführen des Auftrags<br>- Auftrag abgeschlossen|
CorrelationId | Die GUID, bei der es sich um die Korrelations-ID des Runbookauftrags handelt.|
Kategorie | Klassifizierung des Datentyps. Für Automation ist der Wert „JobLogs“.|
RunbookName | Der Name des Runbooks.|
JobId | Die GUID, bei der es sich um die ID des Runbookauftrags handelt.|
Aufrufer | Der Benutzer oder das System, der/das den Vorgang initiiert hat. Mögliche Werte sind entweder eine E-Mail-Adresse oder, bei geplanten Aufträgen, ein System.|

### Auftragsdatenströme
Eigenschaft | Beschreibung|
----------|----------|
Time | Datum und Uhrzeit der Ausführung des Runbookauftrags.|
resourceId | Gibt den Ressourcentyp in Azure an. Für Automation ist der Wert das dem Runbook zugeordnete Automation-Konto.|
operationName | Gibt den Typ des in Azure ausgeführten Vorgangs an. Für Automation ist der Wert der Auftrag.|
resultType | Der Status des Runbookauftrags. Mögliche Werte:<br>- InProgress|
resultDescription | Enthält den Ausgabestream des Runbooks.|
CorrelationId | Die GUID, bei der es sich um die Korrelations-ID des Runbookauftrags handelt.|
Kategorie | Klassifizierung des Datentyps. Für Automation ist der Wert „JobStreams“.|
RunbookName | Der Name des Runbooks.|
JobId | Die GUID, bei der es sich um die ID des Runbookauftrags handelt.|
Aufrufer | Der Benutzer oder das System, der/das den Vorgang initiiert hat. Mögliche Werte sind entweder eine E-Mail-Adresse oder, bei geplanten Aufträgen, ein System.| 
StreamType | Der Typ des Auftragsstreams. Mögliche Werte:<br>-Status<br>- Ausgabe<br>- Warnung<br>- Fehler<br>- Debuggen<br>- Ausführlich|

## Anzeigen von Automation-Protokollen in Log Analytics 

Nachdem Sie begonnen haben, Ihre Automation-Auftragsprotokolle an Log Analytics zu senden, möchten wir uns nun ansehen, wie Sie diese Protokolle in OMS verwenden können.

### Senden einer E-Mail, wenn ein Runbookauftrag mit einem Fehler abgebrochen oder angehalten wird 

Eine der häufigsten Anfragen unserer Kunden betrifft die Möglichkeit, eine E-Mail oder eine SMS zu senden, wenn es bei einem Runbookauftrag ein Problem gibt.

Zum Erstellen einer Warnungsregel erstellen Sie zunächst eine Protokollsuche für die Datensätze des Runbookauftrags, die die Warnung auslösen sollen. Anschließend ist die Schaltfläche **Warnung** verfügbar, sodass Sie die Warnungsregel erstellen und konfigurieren können.

1.	Klicken Sie auf der Seite mit der OMS-Übersicht auf **Protokollsuche**.
2.	Erstellen Sie eine Protokollsuchabfrage für Ihre Warnung, indem Sie folgenden Text in das Abfragefeld eingeben: `Category=JobLogs (ResultType=Failed || ResultType=Suspended)`. Sie können auch eine Gruppierung nach RunbookName vornehmen, indem Sie Folgendes eingeben: `Category=JobLogs (ResultType=Failed || ResultType=Suspended) | measure Count() by RunbookName_s`.
  
    Wenn Sie Protokolle von mehr als einem Automation-Konto oder Abonnement in Ihrem Arbeitsbereich eingerichtet haben, ist es möglicherweise sinnvoll, Ihre Warnungen nach Abonnement oder Automation-Konto zu gruppieren. Der Name des Automation-Kontos kann vom Ressourcenfeld in der JobLogs-Suche abgeleitet werden.

3.	Klicken Sie oben auf der Seite auf **Warnung**, um den Bildschirm **Warnungsregel hinzufügen** zu öffnen. Weitere Informationen zu den Konfigurationsoptionen für Warnungen finden Sie unter [Warnungen in Log Analytics](../log-analytics/log-analytics-alerts.md#creating-an-alert-rule).

### Durchführen einer Suche nach allen Aufträgen, die mit Fehlern abgeschlossen wurden 

Neben den Warnungen, die bei Fehlern mit Abbruch des Vorgangs ausgelöst werden, möchten Sie wahrscheinlich auch darüber informiert werden, wenn in einem Runbookauftrag ein Fehler ohne Abbruch des Vorgangs aufgetreten ist (PowerShell erzeugt einen Fehlerdatenstrom, Fehler ohne Abbruch führen jedoch nicht dazu, dass Ihr Auftrag angehalten oder mit einem Fehler beendet wird).

1. Klicken Sie im OMS-Portal auf **Protokollsuche**.
2. Geben Sie in das Abfragefeld `Category=JobStreams StreamType_s=Error | measure count() by JobId_g` ein und klicken Sie dann auf **Search**.


### Anzeigen von Auftragsdatenströmen für einen Auftrag  

Wenn Sie einen Auftrag debuggen, sollten Sie möglicherweise auch einen Blick in die Auftragsdatenströme werfen. Mit der folgenden Abfrage werden alle Datenströme für einen einzelnen Auftrag mit der GUID 2ebd22ea-e05e-4eb9-9d76-d73cbd4356e0 abgerufen:

`Category=JobStreams JobId_g="2ebd22ea-e05e-4eb9-9d76-d73cbd4356e0" | sort TimeGenerated | select ResultDescription`

### Anzeigen des Auftragsstatusverlaufs 

Abschließend möchten Sie möglicherweise Ihren Auftragsverlauf visualisieren. Sie können die folgende Abfrage verwenden, um nach dem Statusverlauf Ihrer Aufträge zu suchen.

`Category=JobLogs NOT(ResultType="started") | measure Count() by ResultType interval 1day` <br> ![Diagramm zum Auftragsstatusverlauf in OMS](media/automation-manage-send-joblogs-log-analytics/historical-job-status-chart.png)<br>


## Zusammenfassung

Wenn Sie Ihre Automation-Auftragsstatus und Datenstromdaten an Log Analytics senden, können Sie bessere Einblicke in den Status Ihrer Automation-Aufträge erhalten. Richten Sie dazu Warnungen ein, um benachrichtigt zu werden, wenn ein Problem auftritt. Verwenden Sie außerdem benutzerdefinierte Dashboards mit erweiterten Abfragen, um die Runbookergebnisse, den Runbookauftragsstatus und andere zugehörige Schlüsselindikatoren oder Metriken zu visualisieren. So erreichen Sie eine höhere operative Transparenz und können schneller auf Vorfälle reagieren.


## Nächste Schritte

- Weitere Informationen zum Erstellen verschiedener Suchabfragen und zur Überprüfung der Automation-Auftragsprotokolle mit Log Analytics finden Sie unter [Protokollsuchen in Log Analytics](../log-analytics/log-analytics-log-searches.md).
- Unter [Runbookausgabe und -meldungen](automation-runbook-output-and-messages.md) erfahren Sie, wie Sie Ausgabe- und Fehlermeldungen erstellen und diese von Runbooks abrufen.
- Weitere Informationen zum Ausführen von Runbooks, zum Überwachen von Runbookaufträgen sowie andere technische Details finden Sie unter [Verfolgen eines Runbookauftrags](automation-runbook-execution.md).
- Weitere Informationen zu OMS Log Analytics und Datenerfassungsquellen finden Sie unter [Herstellen einer Verbindung zwischen Azure-Speichern und Log Analytics](../log-analytics/log-analytics-azure-storage.md)

<!---HONumber=AcomDC_0928_2016-->