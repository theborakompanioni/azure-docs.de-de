
<properties
	pageTitle="Verwalten und Überwachen von Sicherungen virtueller Computer, die von Resource Manager bereitgestellt werden | Microsoft Azure"
	description="Es wird beschrieben, wie Sie Sicherungen virtueller Computer verwalten und überwachen, die von Resource Manager bereitgestellt werden."
	services="backup"
	documentationCenter=""
	authors="trinadhk"
	manager="shreeshd"
	editor=""/>

<tags
	ms.service="backup"
	ms.workload="storage-backup-recovery"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="06/03/2016"
	ms.author="jimpark; markgal;"/>

# Verwalten und Überwachen der Sicherung von virtuellen Azure-Computern

> [AZURE.SELECTOR]
- [Verwalten von Azure-VM-Sicherungen](backup-azure-manage-vms.md)
- [Verwalten von klassischen VM-Sicherungen](backup-azure-manage-vms-classic.md)

Dieser Artikel enthält eine Anleitung dazu, wie Sie die Sicherungen für Ihre virtuellen Computer (VMs) verwalten. Außerdem wird beschrieben, welche Sicherungsinformationen im Portal-Dashboard verfügbar sind. Die Anleitung in diesem Artikel gilt für die Verwendung von VMs mit Recovery Services-Tresoren. Die Erstellung von virtuellen Computern und der Schutz virtueller Computer werden in diesem Artikel nicht beschrieben. Eine Einführung zu dem Thema, wie Sie von Azure Resource Manager bereitgestellte VMs in Azure mit einem Recovery Services-Tresor schützen können, finden Sie unter [First look: Back up VMs to a Recovery Services vault](backup-azure-vms-first-look-arm.md) (Einführung: Sichern von VMs in einem Recovery Services-Tresor).

## Zugreifen auf Tresore und geschützte virtuelle Computer

Im Azure-Portal ermöglicht das Dashboard für den Recovery Services-Tresor den Zugriff auf Informationen zum Tresor, z.B.:

- Letzte Sicherungsmomentaufnahme, die gleichzeitig der letzte Wiederherstellungspunkt ist <br>
- Sicherungsrichtlinie <br>
- Gesamtgröße aller Sicherungsmomentaufnahmen <br>
- Anzahl von virtuellen Computern, die durch den Tresor geschützt sind <br>

Viele Verwaltungsaufgaben mit einer Sicherung des virtuellen Computers beginnen mit dem Öffnen des Tresors im Dashboard. Da Tresore aber zum Schützen mehrerer Elemente (bzw. mehrerer virtueller Computer) verwendet werden können, müssen Sie zum Anzeigen der Details zu einem bestimmten virtuellen Computer das Dashboard für Tresorelemente öffnen. Im folgenden Verfahren wird veranschaulicht, wie Sie das *Tresordashboard* öffnen und dann mit dem *Dashboard für Tresorelemente* fortfahren. Beide Verfahren enthalten Tipps dazu, wie Sie den Tresor und das Tresorelement dem Azure-Dashboard hinzufügen, indem Sie den Befehl „An Dashboard anheften“ verwenden. Mit der Option „An Dashboard anheften“ können Sie eine Verknüpfung zum Tresor oder Element erstellen. Über die Verknüpfung können Sie auch häufig verwendete Befehle ausführen.

>[AZURE.TIP] Wenn mehrere Dashboards und Blätter geöffnet sind, können Sie den dunkelblauen Schieberegler unten im Fenster verwenden, um die Anzeige des Azure-Dashboards zu ändern.

![Vollständige Ansicht mit Schieberegler](./media/backup-azure-manage-vms/bottom-slider.png)

### Öffnen Sie im Dashboard einen Recovery Services-Tresor:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

2. Klicken Sie im Hub-Menü auf **Durchsuchen**, und geben Sie in der Liste mit den Ressourcen **Recovery Services** ein. Wenn Sie mit der Eingabe beginnen, wird die Liste anhand Ihrer Eingaben gefiltert. Klicken Sie auf **Recovery Services-Tresor**.

    ![Erstellen eines Recovery Services-Tresors – Schritt 1](./media/backup-azure-manage-vms/browse-to-rs-vaults.png) <br/>

    Die Liste mit den Recovery Services-Tresoren wird angezeigt.

    ![Liste der Recovery Services-Tresore](./media/backup-azure-manage-vms/list-o-vaults.png) <br/>

    >[AZURE.TIP] Wenn Sie einen Tresor an das Azure-Dashboard anheften, ist der Tresor nach dem Öffnen des Azure-Portals sofort verfügbar. Klicken Sie zum Anheften eines Tresors an das Dashboard in der Tresorliste mit der rechten Maustaste auf den Tresor, und wählen Sie **An Dashboard anheften**.

3. Wählen Sie in der Liste mit den Tresoren den Tresor aus, dessen Dashboard Sie öffnen möchten. Wenn Sie den Tresor ausgewählt haben, werden das Tresor-Dashboard und das Blatt **Einstellungen** geöffnet. In der folgenden Abbildung ist das Dashboard **Contoso-vault***** hervorgehoben.

    ![Tresor-Dashboard und Blatt „Einstellungen“ öffnen](./media/backup-azure-manage-vms/full-view-rs-vault.png)

### Öffnen eines Dashboards für Tresorelemente

Im vorherigen Verfahren haben Sie das Tresor-Dashboard geöffnet. So öffnen Sie das Dashboard für Tresorelemente

1. Klicken Sie im Tresor-Dashboard auf der Kachel **Sicherungselemente** auf **Virtuelle Azure-Computer**.

    ![Kachel „Sicherungselemente“ öffnen](./media/backup-azure-manage-vms/contoso-vault.png)

    Auf dem Blatt **Sicherungselemente** ist jeweils der letzte Sicherungsauftrag für jedes Element aufgeführt. In diesem Beispiel ist ein virtueller Computer „demovm-markgal“ vorhanden, der mit diesem Tresor geschützt wird.

    ![Kachel „Sicherungselemente“](./media/backup-azure-manage-vms/backup-items-blade.png)

    >[AZURE.TIP] Um den Zugriff zu erleichtern, können Sie ein Tresorelement an das Azure-Dashboard anheften. Klicken Sie zum Anheften eines Tresorelements in der Liste mit den Tresorelementen mit der rechten Maustaste auf das Element, und wählen Sie **An Dashboard anheften**.

2. Klicken Sie im Blatt **Sicherungselemente** auf das Element, um das Dashboard für die Tresorelemente zu öffnen.

    ![Kachel „Sicherungselemente“](./media/backup-azure-manage-vms/backup-items-blade-select-item.png)

    Das Dashboard für die Tresorelemente wird mit dem Blatt **Einstellungen** geöffnet.

    ![Sicherungselemente-Dashboard mit Blatt „Einstellungen“](./media/backup-azure-manage-vms/item-dashboard-settings.png)

    Über das Dashboard für die Tresorelemente können Sie viele wichtige Verwaltungsaufgaben durchführen, z.B.:

    - Ändern von Richtlinien oder Erstellen einer neuen Sicherungsrichtlinie<br>
	- Anzeigen von Wiederherstellungspunkten und des Konsistenzstatus<br>
	- Bedarfsabhängiges Sichern eines virtuellen Computers <br>
	- Beenden des Schutzes virtueller Computer <br>
	- Fortsetzen des Schutzes eines virtuellen Computers <br>
	- Löschen von Sicherungsdaten (oder eines Wiederherstellungspunkts) <br>
	- [Wiederherstellen einer Sicherung (oder eines Wiederherstellungspunkts)](./backup-azure-arm-restore-vms.md#restore-a-recovery-point) <br>

Für die folgenden Verfahren ist der Startpunkt das Dashboard für die Tresorelemente.

## Ändern von Richtlinien oder Erstellen einer neuen Sicherungsrichtlinie

1. Klicken Sie im [Dashboard für die Tresorelemente](backup-azure-manage-vms.md#open-a-vault-item-dashboard) auf **Alle Einstellungen**, um das Blatt **Einstellungen** zu öffnen.

    ![Blatt „Sicherungsrichtlinie“](./media/backup-azure-manage-vms/all-settings-button.png)

2. Klicken Sie im Blatt **Einstellungen** auf **Sicherungsrichtlinie**, um das Blatt zu öffnen.

    Im Blatt werden die Details zur Sicherungshäufigkeit und zur Aufbewahrungsdauer angezeigt.

    ![Blatt „Sicherungsrichtlinie“](./media/backup-azure-manage-vms/backup-policy-blade.png)

3. Im Menü **Sicherungsrichtlinie auswählen**:
    - Wählen Sie zum Ändern von Richtlinien eine andere Richtlinie aus, und klicken Sie auf **Speichern**. Die neue Richtlinie wird sofort auf den Tresor angewendet. <br>
    - Wählen Sie zum Erstellen einer neuen Richtlinie die Option **Neu erstellen**.

    ![Sicherung virtueller Computer](./media/backup-azure-manage-vms/backup-policy-create-new.png)

    Eine Anleitung zum Erstellen einer Sicherungsrichtlinie finden Sie unter [Definieren einer Sicherungsrichtlinie](backup-azure-manage-vms.md#defining-a-backup-policy).


## Bedarfsabhängig Sicherung eines virtuellen Computers
Sie können eine bedarfsabhängige Sicherung eines virtuellen Computers erstellen, sobald dieser für den Schutz konfiguriert ist. Wenn die erste Sicherung für den virtuellen Computer ansteht, wird mithilfe einer bedarfsabhängigen Sicherung eine vollständige Kopie des virtuellen Computers im Recovery Services-Tresor erstellt. Nachdem die erste Sicherung durchgeführt wurde, werden bei einer bedarfsabhängigen Sicherung nur Änderungen der vorherigen Momentaufnahme an den Recovery Services-Tresor gesendet. Es handelt sich also immer um einen inkrementellen Vorgang.

>[AZURE.NOTE] Die Aufbewahrungsdauer für eine bedarfsabhängige Sicherung ist der Aufbewahrungswert, der in der Richtlinie für den Sicherungspunkt „Täglich“ angegeben wird. Wenn kein täglicher Sicherungspunkt ausgewählt ist, wird der wöchentliche Sicherungspunkt verwendet.

So lösen Sie eine bedarfsabhängige Sicherung eines virtuellen Computers aus

1. Klicken Sie im [Dashboard für die Tresorelemente](backup-azure-manage-vms.md#open-a-vault-item-dashboard) auf **Jetzt sichern**.

    ![Schaltfläche „Jetzt sichern“](./media/backup-azure-manage-vms/backup-now-button.png)

    Im Portal wird überprüft, ob Sie einen bedarfsabhängigen Sicherungsauftrag starten möchten. Klicken Sie auf **Ja**, um den Sicherungsauftrag zu starten.

    ![Schaltfläche „Jetzt sichern“](./media/backup-azure-manage-vms/backup-now-check.png)

    Der Sicherungsauftrag erstellt einen neuen Wiederherstellungspunkt. Die Beibehaltungsdauer des erstellten Wiederherstellungspunkts entspricht der Beibehaltungsdauer, die in der Richtlinie für den virtuellen Computer angegeben ist. Klicken Sie zum Nachverfolgen des Status für den Auftrag im Tresor-Dashboard auf die Kachel **Sicherungsaufträge**.


## Beenden des Schutzes für virtuelle Computer
Wenn Sie sich für das Beenden des Schutzes für einen virtuellen Computer entscheiden, werden Sie gefragt, ob Sie die Wiederherstellungspunkte beibehalten möchten. Es gibt zwei Möglichkeiten zum Beenden des Schutzes für virtuelle Computer: Beenden aller zukünftigen Sicherungsaufträge und Löschen aller Wiederherstellungspunkte oder Beenden aller zukünftigen Sicherungsaufträge unter Beibehaltung der Wiederherstellungspunkte. Mit der Beibehaltung der Wiederherstellungspunkte im Speicher sind Kosten verbunden. Der Vorteil der Beibehaltung von Wiederherstellungspunkten ist, dass Sie den virtuellen Computer bei Bedarf später wiederherstellen können. Klicken Sie [hier](https://azure.microsoft.com/pricing/details/backup/), um die Preisübersicht für virtuelle Computer anzuzeigen. Wenn Sie sich für das Löschen aller Wiederherstellungspunkte entschieden haben, ist keine Option für das Wiederherstellen des virtuellen Computers vorhanden.

So beenden Sie den Schutz für einen virtuellen Computer

1. Klicken Sie im [Dashboard für die Tresorelemente](backup-azure-manage-vms.md#open-a-vault-item-dashboard) auf **Sicherung beenden**.

    ![Schaltfläche „Sicherung beenden“](./media/backup-azure-manage-vms/stop-backup-button.png)

    Das Blatt „Sicherung beenden“ wird geöffnet.

    ![Blatt „Sicherung beenden“](./media/backup-azure-manage-vms/stop-backup-blade.png)

2. Wählen Sie im Blatt **Sicherung beenden** aus, ob die Sicherungsdaten beibehalten oder gelöscht werden sollen. Das Feld mit den Informationen enthält jeweils die Details zu Ihrer Wahl.

    ![Schutz beenden](./media/backup-azure-manage-vms/retain-or-delete-option.png)

3. Fahren Sie mit Schritt 4 fort, wenn Sie sich für die Beibehaltung der Sicherungsdaten entschieden haben. Falls Sie das Löschen der Sicherungsdaten gewählt haben, müssen Sie bestätigen, dass die Sicherungsaufträge beendet und die Wiederherstellungspunkte gelöscht werden sollen. Geben Sie den Namen des Elements ein.

    ![Überprüfung beenden](./media/backup-azure-manage-vms/item-verification-box.png)

    Falls Sie unsicher sind, wie der Name lautet, können Sie mit der Maus auf das Ausrufezeichen zeigen, um den Namen anzuzeigen. Der Name des Elements ist auch oben im Blatt unter **Sicherung beenden** angegeben.

4. Geben Sie optional einen **Grund** oder einen **Kommentar** ein.

5. Klicken Sie auf ![Schaltfläche „Sicherung beenden“](./media/backup-azure-manage-vms/stop-backup-button-blue.png), um den Sicherungsauftrag für das aktuelle Element zu beenden.

    Sie werden mit einer Benachrichtigung darüber informiert, dass die Sicherungsaufträge beendet wurden.

    ![Bestätigen des Beendens des Schutzes](./media/backup-azure-manage-vms/stop-message.png)


## Fortsetzen des Schutzes eines virtuellen Computers
Wenn beim Beenden des Schutzes für den virtuellen Computer die Option **Sicherungsdaten beibehalten** gewählt wurde, ist es möglich, den Schutz fortzusetzen. Wenn die Option **Sicherungsdaten löschen** gewählt wurde, kann der Schutz für den virtuellen Computer nicht fortgesetzt werden.

So setzen Sie den Schutz für den virtuellen Computer fort

1. Klicken Sie im [Dashboard für die Tresorelemente](backup-azure-manage-vms.md#open-a-vault-item-dashboard) auf **Sicherung fortsetzen**.

    ![Schutz fortsetzen](./media/backup-azure-manage-vms/resume-backup-button.png)

    Das Blatt „Sicherungsrichtlinie“ wird geöffnet.

    >[AZURE.NOTE] Wenn Sie den virtuellen Computer erneut schützen, können Sie eine andere Richtlinie als die Richtlinie auswählen, gemäß der der virtuelle Computer zuvor geschützt wurde.

2. Führen Sie die Schritte unter [Ändern von Richtlinien oder Erstellen einer neuen Sicherungsrichtlinie](backup-azure-manage-vms.md#change-policies-or-create-a-new-backup-policy) aus, um die Richtlinie für den virtuellen Computer zuzuweisen.

    Nachdem die Sicherungsrichtlinie auf den virtuellen Computer angewendet wurde, wird die folgende Meldung angezeigt:

    ![Erfolgreich geschützte VM](./media/backup-azure-manage-vms/success-message.png)

## Löschen von Sicherungsdaten
Sie können die Sicherungsdaten, die einem virtuellen Computer zugeordnet sind, während des Auftrags **Sicherung beenden** oder jederzeit nach dem Beenden der Sicherungen löschen. Wenn es für Sie von Vorteil ist, das Beenden des Sicherungsauftrags für einen virtuellen Computer zuzulassen und mit der Entscheidung über das Löschen der Wiederherstellungspunkte mehrere Tage oder Wochen zu warten, ist dies möglich. Im Gegensatz zur Wiederherstellung von Wiederherstellungspunkten können Sie beim Löschen von Sicherungsdaten keine bestimmten Wiederherstellungspunkte auswählen, die gelöscht werden sollen. Wenn Sie sich für das Löschen entscheiden, werden alle Wiederherstellungspunkte gelöscht, die dem Element zugeordnet sind.

Beim folgenden Verfahren wird davon ausgegangen, dass der Sicherungsauftrag für den virtuellen Computer beendet oder deaktiviert wurde. Erst nachdem der Sicherungsauftrag deaktiviert wurde, sind die Optionen **Sicherung fortsetzen** und **Sicherung löschen** im Dashboard für die Tresorelemente verfügbar.

![Schaltflächen zum Fortsetzen und Löschen](./media/backup-azure-manage-vms/resume-delete-buttons.png)

So löschen Sie Sicherungsdaten auf einem virtuellen Computer mit *deaktivierter Sicherung*

1. Klicken Sie im [Dashboard für die Tresorelemente](backup-azure-manage-vms.md#open-a-vault-item-dashboard) auf **Sicherung löschen**.

    ![VM-Typ](./media/backup-azure-manage-vms/delete-backup-buttom.png)

    Das Blatt **Sicherungsdaten löschen** wird geöffnet.

    ![VM-Typ](./media/backup-azure-manage-vms/delete-backup-blade.png)

2. Sie müssen den Namen des Elements eingeben, um zu bestätigen, dass Sie die Wiederherstellungspunkte löschen möchten.

    ![Überprüfung beenden](./media/backup-azure-manage-vms/item-verification-box.png)

    Falls Sie unsicher sind, wie der Name lautet, können Sie mit der Maus auf das Ausrufezeichen zeigen, um den Namen anzuzeigen. Der Name des Elements wird oben im Blatt unter **Sicherungsdaten löschen** angegeben.

3. Geben Sie optional einen **Grund** oder einen **Kommentar** ein.

4. Klicken Sie auf ![Schaltfläche „Sicherung beenden“](./media/backup-azure-manage-vms/delete-button.png), um die Sicherungsdaten für das aktuelle Element zu löschen.

    Sie werden mit einer Benachrichtigung darüber informiert, dass die Sicherungsdaten gelöscht wurden.

## Überwachen von Vorgängen
Sie können sich die Betriebs- und Ereignisprotokolle ansehen, um die Verwaltungsvorgänge anzuzeigen, die für den Recovery Services-Tresor durchgeführt wurden. Vorgangsprotokolle bieten eine hervorragende Grundlage für Nachbesprechungen und für die Überwachung von Sicherungsvorgängen. Sie können das Feature „Überwachungsprotokolle“ verwenden, um die Protokolle für alle Vorgänge des *Abonnements* anzuzeigen. Weitere Informationen zu Ereignissen, Vorgängen und Überwachungsprotokollen finden Sie im Artikel [Anzeigen von Ereignis- und Überwachungsprotokollen](../azure-portal/insights-debugging-with-events.md). Verwenden Sie die Einstellung **Überwachungsprotokolle**, um die Ereignis- und Vorgangsprotokolle für einen Recovery Services-Tresor oder ein Tresorelement anzuzeigen.

Die folgenden Vorgänge werden in Überwachungsprotokollen aufgezeichnet:

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

So zeigen Sie Ereignisprotokolle für einen Recovery Services-Tresor an

1. Navigieren Sie im [Tresor-Dashboard](backup-azure-manage-vms.md#open-a-recovery-services-vault-in-the-dashboard) zur Option **Überwachungsprotokolle**, und klicken Sie darauf, um das Blatt **Ereignisse** zu öffnen.

    ![Überwachungsprotokolle](./media/backup-azure-manage-vms/audit-logs.png)

    Das Blatt „Ereignisse“ wird mit den gefilterten Betriebsereignissen für den aktuellen Tresor geöffnet. Im Blatt wird die Liste mit den Ereignissen vom Typ „Kritisch“, „Fehler“, „Warnung“ und „Information“ angezeigt, die im Verlauf der letzten Woche eingetreten sind. Der Zeitraum ist ein Standardwert, der in den Einstellungen unter **Filter** festgelegt wird. Im Blatt **Ereignisse** wird auch ein Balkendiagramm angezeigt, mit dem nachverfolgt wird, wann die Ereignisse eintreten. Falls das Balkendiagramm nicht angezeigt werden soll, klicken Sie im Menü **Ereignisse** auf **Diagramm anzeigen**, um es auszublenden.

    ![Filter „Überwachungsprotokolle“](./media/backup-azure-manage-vms/audit-logs-filter.png)

2. Weitere Informationen zu einem bestimmten Vorgang aus der Liste mit den Vorgängen erhalten Sie, indem Sie auf den Vorgang klicken, um das dazugehörige Blatt zu öffnen. Das Blatt enthält ausführliche Informationen zum Vorgang und eine Liste mit den Ereignissen, die in diesem Zeitraum eingetreten sind.

    ![Vorgangsdetails](./media/backup-azure-manage-vms/audit-logs-details-window.png)

3. Klicken Sie in der Liste mit den Ereignissen auf den Vorgang, um das Blatt „Details“ zu öffnen, wenn Sie ausführliche Informationen zu einem bestimmten Ereignis anzeigen möchten.

    ![Ereignisdetails](./media/backup-azure-manage-vms/audit-logs-details-window-deep.png)

    Die Informationen auf Ereignisebene sind die ausführlichsten Informationen, die angezeigt werden können. Im restlichen Teil dieses Verfahrens wird beschrieben, wie Sie die verfügbaren Informationen bearbeiten oder ändern.

4. Klicken Sie zum Bearbeiten der Liste mit den verfügbaren Filtern im Menü **Ereignisse** auf **Filter**, um das Blatt zu öffnen.

    ![Blatt „Filter“ öffnen](./media/backup-azure-manage-vms/audi-logs-filter-button.png)

5. Passen Sie im Blatt **Filter** die Filter **Ebene**, **Zeitraum** und **Aufrufer** an. Die anderen Filter sind nicht verfügbar, da sie so festgelegt wurden, dass sie die aktuellen Informationen für den Recovery Services-Tresor angeben.

    ![Details zu Überwachungsprotokollabfragen](./media/backup-azure-manage-vms/filter-blade.png)

    Sie können die **Ebene** des Ereignisses angeben: Kritisch, Fehler, Warnung oder Information. Sie können eine beliebige Kombination von Ereignisebenen wählen, aber Sie müssen mindestens eine Ebene auswählen. Aktivieren oder deaktivieren Sie die Ebenen jeweils. Mit dem Filter **Zeitraum** können Sie angeben, wie lange Ereignisse erfasst werden. Wenn Sie einen benutzerdefinierten Zeitraum verwenden, können Sie die Start- und Endzeiten festlegen.

6. Klicken Sie auf **Aktualisieren**, wenn Sie bereit zum Abfragen der Vorgangsprotokolle mit dem Filter sind. Die Ergebnisse werden im Blatt **Ereignisse** angezeigt.

    ![Vorgangsdetails](./media/backup-azure-manage-vms/edited-list-of-events.png)


## Warnungsbenachrichtigungen
Sie können benutzerdefinierte Warnungsbenachrichtigungen für die Aufträge im Portal abrufen. Definieren Sie zum Abrufen dieser Aufträge PowerShell-basierte Warnungsregeln in den Ereignissen der Vorgangsprotokolle. Verwenden Sie *PowerShell, Version 1.3.0 oder höher*.

Verwenden Sie einen Befehl der folgenden Art, um eine benutzerdefinierte Benachrichtigung zur Warnung bei Sicherungsfehlern zu definieren:

```
PS C:\> $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail contoso@microsoft.com
PS C:\> Add-AzureRmLogAlertRule -Name backupFailedAlert -Location "East US" -ResourceGroup RecoveryServices-DP2RCXUGWS3MLJF4LKPI3A3OMJ2DI4SRJK6HIJH22HFIHZVVELRQ-East-US -OperationName Microsoft.Backup/RecoveryServicesVault/Backup -Status Failed -TargetResourceId /subscriptions/86eeac34-eth9a-4de3-84db-7a27d121967e/resourceGroups/RecoveryServices-DP2RCXUGWS3MLJF4LKPI3A3OMJ2DI4SRJK6HIJH22HFIHZVVELRQ-East-US/providers/microsoft.backupbvtd2/RecoveryServicesVault/trinadhVault -Actions $actionEmail
```

**ResourceId**: Sie erhalten diese Informationen in den Überwachungsprotokollen. Die ResourceId ist in der Spalte „Ressource“ der Vorgangsprotokolle angegeben.

**OperationName**: Hat das Format „Microsoft.RecoveryServices/recoveryServicesVault/<EventName>“, wobei EventName folgende Werte haben kann: Register, Unregister, ConfigureProtection, Backup, Restore, StopProtection, DeleteBackupData, CreateProtectionPolicy, DeleteProtectionPolicy, UpdateProtectionPolicy.

**Status**: Unterstützte Werte sind „Started“, „Succeeded“ und „Failed“.

**ResourceGroup**: Dies ist die Ressourcengruppe, der die Ressource angehört. Sie können den generierten Protokollen die Spalte „Ressourcengruppe“ hinzufügen. „Ressourcengruppe“ ist einer der verfügbaren Typen von Ereignisinformationen.

**Name**: Name der Warnungsregel.

**CustomEmails**: Geben Sie die benutzerdefinierte E-Mail-Adresse an, an die die Warnungsbenachrichtigung gesendet werden soll.

**SendToServiceOwners**: Mit dieser Option wird die Warnungsbenachrichtigung an alle Administratoren und Co-Administratoren des Abonnements gesendet. Sie kann im **New-AzureRmAlertRuleEmail**-Cmdlet verwendet werden.

### Einschränkungen für Warnungen
Ereignisbasierte Warnungen unterliegen den folgenden Einschränkungen:

1. Warnungen werden auf allen virtuellen Computern im Recovery Services-Tresor ausgelöst. Es ist nicht möglich, die Warnung für eine bestimmte Gruppe von virtuellen Computern in einem Recovery Services-Tresor anzupassen.
2. Dieses Feature befindet sich in der Vorschau. [Weitere Informationen](../azure-portal/insights-powershell-samples.md#create-alert-rules)
3. Warnungen werden von der Adresse „alerts-noreply@mail.windowsazure.com“ gesendet. Sie können den E-Mail-Absender derzeit nicht ändern.

[AZURE.INCLUDE [backup-create-backup-policy-for-vm](../../includes/backup-create-backup-policy-for-vm.md)]

## Nächste Schritte

Informationen zum erneuten Erstellen eines virtuellen Computers über einen Wiederherstellungspunkt finden Sie unter [Wiederherstellen virtueller Azure-Computer](backup-azure-restore-vms.md). Informationen zum Schutz Ihrer virtuellen Computer finden Sie unter [First look: Back up VMs to a Recovery Services vault](backup-azure-vms-first-look-arm.md) (Einführung: Sichern von VMs in einem Recovery Services-Tresor).

<!---HONumber=AcomDC_0608_2016-->