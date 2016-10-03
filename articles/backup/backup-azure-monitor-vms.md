<properties
   pageTitle="Überwachen von mit Resource Manager bereitgestellten Sicherungen virtueller Computer | Microsoft Azure"
   description="Es wird beschrieben, wie Sie Ereignisse und Warnungen von Sicherungen virtueller Computer überwachen, die mit Resource Manager bereitgestellt werden. Senden Sie E-Mails basierend auf Warnungen."
   services="backup"
   documentationCenter="dev-center-name"
   authors="markgalioto"
   manager="cfreeman"
   editor=""/>

<tags
ms.service="backup"
ms.workload="storage-backup-recovery"
ms.tgt_pltfrm="na"
ms.devlang="na"
ms.topic="article"
ms.date="08/25/2016"
ms.author="trinadhk; giridham;"/>

# Überwachen von Warnungen für Sicherungen von virtuellen Azure-Computern

Warnungen sind Antworten des Diensts mit dem Hinweis, dass ein Ereignisschwellenwert erreicht oder überschritten wurde. Informationen zur zeitlichen Entstehung von Problemen können sehr wichtig sein, um die Geschäftskosten gering zu halten. Auftretende Warnungen halten sich normalerweise nicht an einen Zeitplan, und daher ist es wichtig informiert zu werden, sobald die Warnungen auftreten. Beispiel: Wenn bei einem Sicherungs- oder Wiederherstellungsauftrag ein Fehler auftritt, wird innerhalb von fünf Minuten nach Auftreten des Fehlers eine Warnung gesendet. Im Dashboard des Tresors werden auf der Kachel „Sicherungswarnungen“ Ereignisse der Ebenen „Kritisch“ und „Warnung“ angezeigt. In den Einstellungen der Sicherungswarnungen können Sie alle Ereignisse anzeigen. Aber was ist zu tun, wenn eine Warnung bei der Arbeit an einem anderen Problem auftritt? Wenn Sie nicht wissen, wann die Warnung auftritt, kann dies nur ein unbedeutender Vorfall sein, oder es kann zu einer Kompromittierung von Daten kommen. Stellen Sie wie folgt sicher, dass die richtigen Personen über eine Warnung benachrichtigt werden: Konfigurieren Sie den Dienst so, dass beim Auftreten Warnungsbenachrichtigungen per E-Mail gesendet werden. Weitere Informationen zum Einrichten von E-Mail-Benachrichtigungen finden Sie unter [Konfigurieren von Benachrichtigungen](backup-azure-monitor-vms.md#configure-notifications).

## Wie finde ich Informationen zu den Warnungen?

Sie müssen das Blatt „Sicherungswarnungen“ öffnen, um Informationen zu dem Ereignis anzuzeigen, das eine Warnung ausgelöst hat. Es gibt zwei Möglichkeiten, das Blatt „Sicherungswarnungen“ zu öffnen: über die Kachel „Sicherungswarnungen“ im Dashboard des Tresors oder über das Blatt „Warnungen und Ereignisse“.

Gehen Sie wie folgt vor, um das Blatt „Sicherungswarnungen“ über die Kachel „Sicherungswarnungen“ zu öffnen:

- Klicken Sie im Dashboard des Tresors auf der Kachel **Sicherungswarnungen** auf **Kritisch** oder **Warnung**, um die Vorgangsereignisse für diesen Schweregrad anzuzeigen.

    ![Kachel „Sicherungswarnungen“](./media/backup-azure-monitor-vms/backup-alerts-tile.png)


Gehen Sie wie folgt vor, um das Blatt „Sicherungswarnungen“ über das Blatt „Warnungen und Ereignisse“ zu öffnen:

1. Klicken Sie im Dashboard des Tresors auf **Alle Einstellungen**. ![Schaltfläche „Alle Einstellungen“](./media/backup-azure-monitor-vms/all-settings-button.png)

2. Klicken Sie auf dem Blatt **Einstellungen** auf **Warnungen und Ereignisse**. ![Schaltfläche „Warnungen und Ereignisse“](./media/backup-azure-monitor-vms/alerts-and-events-button.png)

3. Klicken Sie auf dem Blatt **Warnungen und Ereignisse** auf **Sicherungswarnungen**. ![Schaltfläche „Sicherungswarnungen“](./media/backup-azure-monitor-vms/backup-alerts.png)

    Das Blatt **Sicherungswarnungen** wird geöffnet, und die gefilterten Warnungen werden angezeigt.

    ![Kachel „Sicherungswarnungen“](./media/backup-azure-monitor-vms/backup-alerts-critical.png)

4. Klicken Sie in der Liste mit den Ereignissen auf eine Warnung, um das dazugehörige Blatt **Details** zu öffnen, wenn Sie ausführliche Informationen zu einer bestimmten Warnung anzeigen möchten.

    ![Ereignisdetails](./media/backup-azure-monitor-vms/audit-logs-event-detail.png)

    Informationen zum Anpassen der Attribute, die in der Liste angezeigt werden, finden Sie unter [Anzeigen von weiteren Ereignisattributen](backup-azure-monitor-vms.md#view-additional-event-attributes).

## Konfigurieren von Benachrichtigungen

 Sie können den Dienst so konfigurieren, dass E-Mail-Benachrichtigungen für die Warnungen gesendet werden, die im Verlauf der letzten Stunde aufgetreten sind oder wenn bestimmte Arten von Ereignissen eintreten.

So richten Sie E-Mail-Benachrichtigungen für Warnungen ein

1. Klicken Sie im Menü „Sicherungswarnungen“ auf **Benachrichtigungen konfigurieren**.

    ![Menü „Sicherungswarnungen“](./media/backup-azure-monitor-vms/backup-alerts-menu.png)

    Das Blatt „Benachrichtigungen konfigurieren“ wird geöffnet.

    ![Blatt „Benachrichtigungen konfigurieren“](./media/backup-azure-monitor-vms/configure-notifications.png)

2. Klicken Sie auf dem Blatt „Benachrichtigungen konfigurieren“ für E-Mail-Benachrichtigungen auf **Ein**.

    Die Bereiche „Empfänger“ und „Schweregrad“ sind mit einem Sternchen gekennzeichnet, weil die Angabe dieser Informationen erforderlich ist. Geben Sie mindestens eine E-Mail-Adresse ein, und wählen Sie mindestens einen Schweregrad aus.

3. Geben Sie unter **Empfänger (E-Mail)** die E-Mail-Adressen für die Personen ein, die die Benachrichtigungen erhalten sollen. Verwenden Sie das folgende Format: username@domainname.com. Trennen Sie mehrere E-Mail-Adressen per Semikolon (;).

4. Wählen Sie im Bereich **Benachrichtigen** die Option **Pro Warnung**, damit eine Benachrichtigung gesendet wird, wenn die angegebene Warnung auftritt. Wählen Sie die Option **Stündliche Übersicht**, wenn eine Übersicht für die letzte Stunde gesendet werden soll.

5. Wählen Sie unter **Schweregrad** mindestens eine Ebene aus, bei der die E-Mail-Benachrichtigung ausgelöst werden soll.

6. Klicken Sie auf **Speichern**.
### Welche Warnungstypen sind für die Azure IaaS-VM-Sicherung verfügbar?
| Warnstufe | Gesendete Warnungen |
| ------------- | ------------- |
| Kritisch | Sicherungsfehler, Wiederherstellungsfehler |
| Warnung | Keine |
| Information | Keine | 

### Gibt es Fälle, in denen keine E-Mail gesendet wird, auch wenn Benachrichtigungen konfiguriert wurden?

Es gibt Situationen, in denen keine Warnung gesendet wird, obwohl die Benachrichtigungen richtig konfiguriert wurden. In den folgenden Situationen werden keine E-Mail-Benachrichtigungen gesendet, um unnötige Warnungen zu vermeiden:

- Für Benachrichtigungen wurde die stündliche Übersicht konfiguriert, und eine Warnung wird ausgelöst und innerhalb dieser Stunde gelöst.
- Der Auftrag wird abgebrochen.
- Ein Sicherungsauftrag wird ausgelöst und schlägt dann fehl, und ein weiterer Sicherungsauftrag wird ausgeführt.
- Ein geplanter Sicherungsauftrag für einen im Resource Manager-Modus aktivierten virtuellen Computer wird gestartet, der virtuelle Computer ist jedoch nicht mehr vorhanden.

## Anpassen der Ansicht von Ereignissen

Die Einstellung **Überwachungsprotokolle** verfügt über einen vordefinierten Satz von Filtern und Spalten mit Informationen zu Vorgangsereignissen. Sie können die Ansicht so anpassen, dass nach dem Öffnen des Blatts **Ereignisse** die gewünschten Informationen angezeigt werden.

1. Navigieren Sie im [Dashboard des Tresors](./backup-azure-manage-vms.md#open-a-recovery-services-vault-in-the-dashboard) zur Option **Überwachungsprotokolle**, und klicken Sie darauf, um das Blatt **Ereignisse** zu öffnen.

    ![Überwachungsprotokolle](./media/backup-azure-monitor-vms/audit-logs-1606-1.png)

    Das Blatt **Ereignisse** wird mit den gefilterten Vorgangsereignissen für den aktuellen Tresor geöffnet.

    ![Filter „Überwachungsprotokolle“](./media/backup-azure-monitor-vms/audit-logs-filter.png)

    Im Blatt wird die Liste mit den Ereignissen vom Typ „Kritisch“, „Fehler“, „Warnung“ und „Information“ angezeigt, die im Verlauf der letzten Woche eingetreten sind. Der Zeitraum ist ein Standardwert, der unter **Filter** festgelegt wird. Auf dem Blatt **Ereignisse** wird auch ein Balkendiagramm angezeigt, mit dem nachverfolgt wird, wann die Ereignisse eintreten. Falls das Balkendiagramm nicht angezeigt werden soll, klicken Sie im Menü **Ereignisse** auf **Diagramm ausblenden**, um es auszublenden. In der Standardansicht der Option „Ereignisse“ werden Informationen zu „Vorgang“, „Ebene“, „Status“, „Ressource“ und „Zeit“ angezeigt. Informationen zum Verfügbarmachen von zusätzlichen Ereignisattributen finden Sie im Abschnitt zum [Erweitern von Ereignisinformationen](backup-azure-monitor-vms.md#view-additional-event-attributes).

2. Weitere Informationen zu einem Vorgangsereignis erhalten Sie, indem Sie in der Spalte **Vorgang** auf ein Vorgangsereignis klicken, um das dazugehörige Blatt zu öffnen. Das Blatt enthält ausführliche Informationen zu den Ereignissen. Die Ereignisse werden nach der Korrelations-ID und einer Liste mit den Ereignissen gruppiert, die innerhalb des Zeitraums eingetreten sind.

    ![Vorgangsdetails](./media/backup-azure-monitor-vms/audit-logs-details-window.png)

3. Klicken Sie in der Liste mit den Ereignissen auf das Ereignis, um das Blatt **Details** zu öffnen, wenn Sie ausführliche Informationen zu einem bestimmten Ereignis anzeigen möchten.

    ![Ereignisdetails](./media/backup-azure-monitor-vms/audit-logs-details-window-deep.png)

    Die Informationen auf Ereignisebene sind die ausführlichsten Informationen, die angezeigt werden können. Wenn Sie diese Menge an Informationen für jedes Ereignis anzeigen möchten und dieser Detailgrad auf dem Blatt **Ereignisse** sichtbar sein soll, hilft Ihnen der Abschnitt zum [Erweitern von Ereignisinformationen](backup-azure-monitor-vms.md#view-additional-event-attributes) weiter.


## Anpassen des Ereignisfilters
Verwenden Sie die Option **Filter**, um die Informationen anzupassen oder auszuwählen, die auf einem bestimmten Blatt angezeigt werden. Gehen Sie wie folgt vor, um die Ereignisinformationen zu filtern:

1. Navigieren Sie im [Dashboard des Tresors](./backup-azure-manage-vms.md#open-a-recovery-services-vault-in-the-dashboard) zur Option **Überwachungsprotokolle**, und klicken Sie darauf, um das Blatt **Ereignisse** zu öffnen.

    ![Überwachungsprotokolle](./media/backup-azure-monitor-vms/audit-logs-1606-1.png)

    Das Blatt **Ereignisse** wird mit den gefilterten Vorgangsereignissen für den aktuellen Tresor geöffnet.

    ![Filter „Überwachungsprotokolle“](./media/backup-azure-monitor-vms/audit-logs-filter.png)

2. Klicken Sie im Menü **Ereignisse** auf **Filter**, um das Blatt zu öffnen.

    ![Blatt „Filter“ öffnen](./media/backup-azure-monitor-vms/audit-logs-filter-button.png)

3. Passen Sie auf dem Blatt **Filter** die Filter **Ebene**, **Zeitspanne** und **Aufrufer** an. Die anderen Filter sind nicht verfügbar, da sie so festgelegt wurden, dass sie die aktuellen Informationen für den Recovery Services-Tresor angeben.

    ![Details zu Überwachungsprotokollabfragen](./media/backup-azure-monitor-vms/filter-blade.png)

    Sie können die **Ebene** des Ereignisses angeben: Kritisch, Fehler, Warnung oder Information. Sie können eine beliebige Kombination von Ereignisebenen wählen, aber Sie müssen mindestens eine Ebene auswählen. Aktivieren oder deaktivieren Sie die Ebenen jeweils. Mit dem Filter **Zeitspanne** können Sie angeben, wie lange Ereignisse erfasst werden. Wenn Sie einen benutzerdefinierten Zeitraum verwenden, können Sie die Start- und Endzeiten festlegen.

4. Klicken Sie auf **Aktualisieren**, wenn Sie zum Abfragen der Vorgangsprotokolle mit dem Filter bereit sind. Die Ergebnisse werden auf dem Blatt **Ereignisse** angezeigt.

    ![Vorgangsdetails](./media/backup-azure-monitor-vms/edited-list-of-events.png)


### Anzeigen von weiteren Ereignisattributen
Mit der Schaltfläche **Spalten** können Sie weitere Ereignisattribute aktivieren, die in der Liste auf dem Blatt **Ereignisse** angezeigt werden sollen. In der Standardliste mit Ereignissen werden Informationen zu „Vorgang“, „Ebene“, „Status“, „Ressource“ und „Zeit“ angezeigt. Gehen Sie wie folgt vor, um weitere Attribute zu aktivieren:

1. Klicken Sie auf dem Blatt **Ereignisse** auf **Spalten**.

    ![Spalten öffnen](./media/backup-azure-monitor-vms/audi-logs-column-button.png)

    Das Blatt **Spalten auswählen** wird geöffnet.

    ![Blatt „Spalten“](./media/backup-azure-monitor-vms/columns-blade.png)

2. Klicken Sie auf das Kontrollkästchen, um das Attribut auszuwählen. Sie können das Kontrollkästchen für das Attribut aktivieren und deaktivieren.

3. Klicken Sie auf **Zurücksetzen**, um die Liste mit den Attributen auf dem Blatt **Ereignisse** zurückzusetzen. Verwenden Sie nach dem Hinzufügen oder Entfernen von Attributen aus der Liste die Option **Zurücksetzen**, um die neue Liste mit den Ereignisattributen anzuzeigen.

4. Klicken Sie auf **Aktualisieren**, um die Daten in den Ereignisattributen zu aktualisieren. Die folgende Tabelle enthält Informationen zu den einzelnen Attributen.

| Spaltenname |Beschreibung|
| -----------------|-----------|
| Vorgang|Name des Vorgangs|
| Ebene|Die Ebene des Vorgangs kann wie folgt lauten: „Information“, „Warnung“, „Fehler“ oder „Kritisch“.|
|Status|Beschreibender Status des Vorgangs|
|Ressource|URL, mit der die Ressource identifiziert wird (auch als Ressourcen-ID bezeichnet)|
|Zeit|Zeit ab dem aktuellen Zeitpunkt, zu dem das Ereignis eingetreten ist|
|Aufrufer|Aufrufer (System oder Benutzer), der das Ereignis aufgerufen oder ausgelöst hat|
|Zeitstempel|Zeitpunkt, zu dem das Ereignis ausgelöst wurde|
|Ressourcengruppe|Zugeordnete Ressourcengruppe|
|Ressourcentyp|Interner Ressourcentyp, der vom Resource Manager verwendet wird|
|Abonnement-ID|Zugeordnete Abonnement-ID|
|Kategorie|Kategorie des Ereignisses|
|Korrelations-ID|Allgemeine ID für verwandte Ereignisse|



## Verwenden von PowerShell zum Anpassen von Warnungen
Sie können benutzerdefinierte Warnungsbenachrichtigungen für die Aufträge im Portal abrufen. Definieren Sie zum Abrufen dieser Aufträge PowerShell-basierte Warnungsregeln in den Ereignissen der Vorgangsprotokolle. Verwenden Sie *PowerShell, Version 1.3.0 oder höher*.

Verwenden Sie einen Befehl wie das folgende Skript, um eine benutzerdefinierte Benachrichtigung zur Warnung bei Sicherungsfehlern zu definieren:

```
PS C:\> $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail contoso@microsoft.com
PS C:\> Add-AzureRmLogAlertRule -Name backupFailedAlert -Location "East US" -ResourceGroup RecoveryServices-DP2RCXUGWS3MLJF4LKPI3A3OMJ2DI4SRJK6HIJH22HFIHZVVELRQ-East-US -OperationName Microsoft.Backup/RecoveryServicesVault/Backup -Status Failed -TargetResourceId /subscriptions/86eeac34-eth9a-4de3-84db-7a27d121967e/resourceGroups/RecoveryServices-DP2RCXUGWS3MLJF4LKPI3A3OMJ2DI4SRJK6HIJH22HFIHZVVELRQ-East-US/providers/microsoft.backupbvtd2/RecoveryServicesVault/trinadhVault -Actions $actionEmail
```

**ResourceId**: Sie erhalten die ResourceId über die Überwachungsprotokolle. Die ResourceId ist eine URL, die in der Spalte „Ressource“ der Vorgangsprotokolle angegeben ist.

**OperationName** : OperationName hat das Format „Microsoft.RecoveryServices/recoveryServicesVault/ *EventName* “, wobei *EventName* wie folgt lauten kann:<br/>
- Register <br/>
- Unregister <br/>
- ConfigureProtection <br/>
- Backup <br/>
- Restore <br/>
- StopProtection <br/>
- DeleteBackupData <br/>
- CreateProtectionPolicy <br/>
- DeleteProtectionPolicy <br/>
- UpdateProtectionPolicy <br/>

**Status**: Unterstützte Werte sind „Started“, „Succeeded“ oder „Failed“.

**ResourceGroup**: Dies ist die Ressourcengruppe, der die Ressource angehört. Sie können den generierten Protokollen die Spalte „Ressourcengruppe“ hinzufügen. „Ressourcengruppe“ ist einer der verfügbaren Typen von Ereignisinformationen.

**Name**: Name der Warnungsregel.

**CustomEmail**: Geben Sie die benutzerdefinierte E-Mail-Adresse an, an die eine Warnungsbenachrichtigung gesendet werden soll.

**SendToServiceOwners**: Mit dieser Option werden Warnungsbenachrichtigungen an alle Administratoren und Co-Administratoren des Abonnements gesendet. Sie kann im **New-AzureRmAlertRuleEmail** -Cmdlet verwendet werden.

### Einschränkungen für Warnungen
Ereignisbasierte Warnungen unterliegen den folgenden Einschränkungen:

1. Warnungen werden auf allen virtuellen Computern im Recovery Services-Tresor ausgelöst. Es ist nicht möglich, die Warnung für eine Teilmenge von virtuellen Computern in einem Recovery Services-Tresor anzupassen.
2. Dieses Feature befindet sich in der Vorschau. [Weitere Informationen](../azure-portal/insights-powershell-samples.md#create-alert-rules)
3. Warnungen werden von der Adresse „alerts-noreply@mail.windowsazure.com“ gesendet. Sie können den E-Mail-Absender derzeit nicht ändern.


## Nächste Schritte

Ereignisprotokolle bieten eine hervorragende Grundlage für Nachbesprechungen und für die Überwachung von Sicherungsvorgängen. Die folgenden Vorgänge werden protokolliert:

- Registrieren
- Aufheben der Registrierung
- Konfigurieren der Schutzoptionen
- Sicherung (sowohl geplante als auch bedarfsabhängige Sicherung)
- Wiederherstellen
- Schutz beenden
- Löschen von Sicherungsdaten
- Richtlinie hinzufügen
- Löschen von Richtlinien
- Aktualisieren von Richtlinien
- Auftrag abbrechen

Eine umfassende Beschreibung der Ereignis-, Vorgangs- und Überwachungsprotokolle von Azure-Diensten finden Sie im Artikel [Anzeigen von Ereignis- und Überwachungsprotokollen](../azure-portal/insights-debugging-with-events.md).

Informationen zum erneuten Erstellen eines virtuellen Computers über einen Wiederherstellungspunkt finden Sie unter [Wiederherstellen virtueller Azure-Computer](backup-azure-restore-vms.md). Informationen zum Schutz Ihrer virtuellen Computer finden Sie unter [First look: Back up VMs to a Recovery Services vault](backup-azure-vms-first-look-arm.md) (Einführung: Sichern von VMs in einem Recovery Services-Tresor). Informationen zu den Verwaltungsaufgaben für VM-Sicherungen finden Sie im Artikel [Verwalten der Sicherungen von virtuellen Azure-Computern](backup-azure-manage-vms.md).

<!---HONumber=AcomDC_0921_2016-->