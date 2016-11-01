<properties
   pageTitle="Löschen eines Azure Backup-Tresors | Microsoft Azure"
   description="Es wird beschrieben, wie Sie einen Azure Backup-Tresor löschen. Problembehandlung, falls ein Sicherungstresor nicht gelöscht werden kann "
   services="service-name"
   documentationCenter="dev-center-name"
   authors="markgalioto"
   manager="cfreeman"
   editor=""/>

<tags
   ms.service="backup"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="storage-backup-recovery"
   ms.date="08/29/2016"
   ms.author="markgal;trinadhk"/>

# Löschen eines Azure Backup-Tresors

Der Azure Backup-Dienst umfasst zwei Arten von Tresoren: den Backup-Tresor und den Recovery Services-Tresor. Der Backup-Tresor wurde zuerst eingeführt. Anschließend folgte der Recovery Services-Tresor als Unterstützung für die erweiterten Resource Manager-Bereitstellungen. Aufgrund der erweiterten Funktionen und Informationsabhängigkeiten, die im Tresor gespeichert werden müssen, kann das Löschen eines Recovery Services-Tresors schwierig erscheinen. Dies muss aber nicht der Fall sein.

|**Bereitstellungstyp**|**Portal**|**Tresorname**|
|--------------|----------|---------|
|Klassisch|Klassisch|Sicherungstresor|
|Ressourcen-Manager|Azure|Recovery Services-Tresor|


> [AZURE.NOTE] Mit Resource Manager bereitgestellte Lösungen können nicht mit einem Sicherungstresor geschützt werden. Sie können einen Recovery Services-Tresor aber zum Schützen von Servern und virtuellen Computern verwenden, die mit dem klassischen Modell bereitgestellt wurden.

In diesem Artikel verwenden wir den Begriff „Tresor“ für die generische Form des Backup-Tresors bzw. Recovery Services-Tresors. Wir nutzen den formellen Namen „Backup-Tresor“ bzw. „Recovery Services-Tresor“, wenn zwischen den Tresoren unterschieden werden muss.



## Löschen eines Recovery Services-Tresors

Das Löschen eines Recovery Services-Tresors ist ein Prozess, der aus nur einem Schritt besteht – *sofern der Tresor keine Ressourcen enthält*. Bevor Sie einen Recovery Services-Tresor löschen, müssen Sie alle Ressourcen im Tresor entfernen oder löschen. Wenn Sie versuchen, einen Tresor zu löschen, der Ressourcen enthält, erhalten Sie eine Fehlermeldung wie in der folgenden Abbildung.

![Fehler beim Löschen des Tresors](./media/backup-azure-delete-vault/vault-deletion-error.png) <br/>

Beim Klicken auf **Wiederholen** wird so lange der gleiche Fehler angezeigt, bis Sie die Ressourcen aus dem Tresor gelöscht haben. Wenn Sie an dieser Fehlermeldung hängen bleiben, können Sie auf **Abbrechen** klicken und die unten angegebenen Schritte ausführen, um die Ressourcen im Recovery Services-Tresor zu löschen.


### Entfernen der Elemente aus einem Tresor zum Schutz einer VM

Wenn Sie den Recovery Services-Tresor bereits geöffnet haben, können Sie zum zweiten Schritt springen.

1.  Öffnen Sie das Azure-Portal und dann über das Dashboard den Tresor, den Sie löschen möchten.

    Klicken Sie im Hub-Menü auf **Weitere Dienste**, und geben Sie in der Liste mit den Ressourcen **Recovery Services** ein, wenn Sie den Recovery Services-Tresor nicht im Dashboard angeheftet haben. Sobald Sie mit der Eingabe beginnen, wird die Liste auf der Grundlage Ihrer Eingabe gefiltert. Klicken Sie auf **Recovery Services-Tresore**.

    ![Erstellen eines Recovery Services-Tresors – Schritt 1](./media/backup-azure-delete-vault/open-recovery-services-vault.png) <br/>

    Die Liste mit den Recovery Services-Tresoren wird angezeigt. Wählen Sie in der Liste den Tresor aus, den Sie löschen möchten.

    ![Tresor in Liste auswählen](./media/backup-azure-work-with-vaults/choose-vault-to-delete.png)

2. Sehen Sie sich in der Tresoransicht den Bereich **Zusammenfassung** an. Wenn ein Tresor gelöscht werden soll, dürfen keine geschützten Elemente vorhanden sein. Falls unter **Sicherungselemente** oder **Sicherungsverwaltungsserver** eine andere Zahl als Null angegeben ist, müssen Sie diese Elemente entfernen, bevor Sie den Tresor löschen können.

    ![Bereich „Zusammenfassung“ für geschützte Elemente anzeigen](./media/backup-azure-delete-vault/contoso-bkpvault-settings.png)

    VMs und Dateien/Ordner werden als Sicherungselemente angesehen und sind im Bereich „Zusammenfassung“ unter **Sicherungselemente** aufgeführt. Ein DPM-Server ist unter „Zusammenfassung“ im Bereich **Sicherungsverwaltungsserver** aufgeführt. **Replizierte Elemente** bezieht sich auf den Azure Site Recovery-Dienst.

3. Suchen Sie nach den Elementen im Tresor, um mit dem Entfernen der geschützten Elemente aus dem Tresor zu beginnen. Klicken Sie im Dashboard des Tresors auf **Einstellungen** und dann auf **Sicherungselemente**, um das Blatt zu öffnen.

    ![Tresor in Liste auswählen](./media/backup-azure-delete-vault/open-settings-and-backup-items.png)

    Das Blatt **Sicherungselemente** enthält separate Listen, die auf dem Elementtyp basieren: „Virtuelle Azure-Computer“ oder „Dateien/Ordner“ (siehe Abbildung). Standardmäßig wird für die Liste „Elementtyp“ die Option „Virtuelle Azure-Computer“ angezeigt. Wählen Sie im Dropdownmenü die Option **Dateien/Ordner**, um die Liste mit den Elementen vom Typ „Dateien/Ordner“ anzuzeigen, die im Tresor enthalten sind.

4. Bevor Sie ein Element aus dem Tresor zum Schutz einer VM löschen können, müssen Sie den Sicherungsauftrag des Elements beenden und die Wiederherstellungspunktdaten löschen. Gehen Sie für jedes Element im Tresor wie folgt vor:

    a. Klicken Sie auf dem Blatt **Sicherungselemente** mit der rechten Maustaste auf das Element, und wählen Sie im Kontextmenü die Option **Sicherung beenden**.

    ![Sicherungsauftrag beenden](./media/backup-azure-delete-vault/stop-the-backup-process.png)

    Das Blatt „Sicherung beenden“ wird geöffnet.

    b. Wählen Sie auf dem Blatt **Sicherung beenden** im Menü **Option wählen** die Option **Sicherungsdaten löschen**, geben Sie den Namen des Elements ein, und klicken Sie auf **Sicherung beenden**.

      Geben Sie den Namen des Elements ein, um zu bestätigen, dass Sie es löschen möchten. Die Schaltfläche **Sicherung beenden** wird erst aktiviert, nachdem Sie das zu beendende Element bestätigt haben. Wenn das Dialogfeld zum Eingeben des Namens eines Sicherungselements nicht angezeigt wird, haben Sie die Option **Sicherungsdaten beibehalten** gewählt.

    ![Sicherungsdaten löschen](./media/backup-azure-delete-vault/stop-backup-blade-delete-backup-data.png)

      Optional können Sie einen Grund angeben, warum Sie die Daten löschen, und Sie können Kommentare hinzufügen. Warten Sie nach dem Klicken auf **Sicherung beenden** ab, bis der Löschvorgang abgeschlossen ist, und versuchen Sie dann, den Tresor zu löschen. Um zu überprüfen, ob der Vorgang abgeschlossen wurde, können Sie die Azure-Meldungen anzeigen: ![Sicherungsdaten löschen](./media/backup-azure-delete-vault/messages.png). <br/> Nach Abschluss des Vorgangs erhalten Sie eine Meldung mit Hinweis, dass der Sicherungsvorgang beendet wurde und die Sicherungsdaten für das Element gelöscht wurden.

    c. Klicken Sie nach dem Löschen eines Elements aus der Liste auf das Menü **Sicherungselemente** und dann auf **Aktualisieren**, um die restlichen Elemente des Tresors anzuzeigen.

      ![Sicherungsdaten löschen](./media/backup-azure-delete-vault/empty-items-list.png)

      Scrollen Sie auf dem Blatt „Backup-Tresor“ zum Bereich **Zusammenfassung**, wenn in der Liste keine Elemente enthalten sind. Hier sollten keine **Sicherungselemente**, **Sicherungsverwaltungsserver** oder **Replizierten Elemente** angezeigt werden. Falls für den Tresor weiterhin Elemente angezeigt werden, sollten Sie zu Schritt 3 oben zurückkehren und eine andere Elementtypliste auswählen.

5. Klicken Sie auf **Löschen**, wenn in der Symbolleiste des Tresors keine Elemente mehr enthalten sind.

    ![Sicherungsdaten löschen](./media/backup-azure-delete-vault/delete-vault.png)

6. Klicken Sie auf **Ja**, wenn Sie aufgefordert werden, das Löschen des Tresors zu bestätigen.

    Der Tresor wird gelöscht, und Sie gelangen im Portal zurück zum Dienstmenü **Neu**.


## Was passiert, wenn ich den Sicherungsprozess beendet, die Daten aber beibehalten habe?

Wenn Sie den Sicherungsprozess beendet und die Daten versehentlich *beibehalten* haben, müssen Sie die Sicherungsdaten löschen, bevor Sie den Tresor löschen können. So löschen Sie die gesicherten Daten

1. Klicken Sie auf dem Blatt **Sicherungselemente** mit der rechten Maustaste auf das Element, und klicken Sie dann im Kontextmenü auf **Sicherungsdaten löschen**.

    ![Sicherungsdaten löschen](./media/backup-azure-delete-vault/delete-backup-data-menu.png)

    Das Blatt **Sicherungsdaten löschen** wird geöffnet.

2. Geben Sie auf dem Blatt **Sicherungsdaten löschen** den Namen des Elements ein, und klicken Sie auf **Löschen**.

    ![Sicherungsdaten löschen](./media/backup-azure-delete-vault/delete-retained-vault.png)

    Springen Sie nach dem Löschen der Daten oben zu Schritt 4c, und setzen Sie den Vorgang fort.

## Löschen eines Tresors zum Schutz eines DPM-Servers

Bevor Sie einen Tresor löschen können, der als Schutz für einen DPM-Server verwendet wird, müssen Sie alle erstellten Wiederherstellungspunkte löschen und anschließend die Registrierung des Servers für den Tresor aufheben.

So löschen Sie die Daten, die einer Schutzgruppe zugeordnet sind

1. Klicken Sie in der DPM-Verwaltungskonsole auf **Schutz**, wählen Sie eine Schutzgruppe, wählen Sie ein Schutzgruppenmitglied, und klicken Sie im Menüband des Tools auf **Entfernen**. Sie müssen das Mitglied auswählen, damit die Schaltfläche **Entfernen** im Menüband des Tools angezeigt wird. Im Beispiel lautet der Name des Mitglieds **dummyvm9**. Halten Sie die STRG-TASTE gedrückt, um mehrere Mitglieder auszuwählen, falls die Schutzgruppe mehrere Mitglieder enthält.

    ![Sicherungsdaten löschen](./media/backup-azure-delete-vault/az-portal-delete-protection-group.png)

    Das Dialogfeld **Schutz beenden** wird geöffnet.

2. Wählen Sie im Dialogfeld **Schutz beenden** die Option **Geschützte Daten löschen**, und klicken Sie auf **Schutz beenden**.

    ![Sicherungsdaten löschen](./media/backup-azure-delete-vault/delete-dpm-protection-group.png)

    Sie sollten keine geschützten Daten beibehalten, da Sie den Tresor bereinigen müssen, um ihn löschen zu können. Je nachdem, wie viele Wiederherstellungspunkte und Daten in der Schutzgruppe enthalten sind, kann es einige Sekunden oder auch einige Minuten dauern, bis die Daten gelöscht wurden. Im Dialogfeld **Schutz beenden** wird der Status angezeigt, nachdem der Vorgang abgeschlossen wurde.

    ![Sicherungsdaten löschen](./media/backup-azure-delete-vault/success-deleting-protection-group.png)

3. Setzen Sie diesen Vorgang für alle Mitglieder in allen Schutzgruppen fort.

    Sie müssen alle geschützten Daten und die Schutzgruppe(n) entfernen.

4. Wechseln Sie nach dem Löschen aller Mitglieder aus der Schutzgruppe zum Azure-Portal. Öffnen Sie das Dashboard des Tresors, und stellen Sie sicher, dass keine **Sicherungselemente**, **Sicherungsverwaltungsserver** oder **Replizierten Elemente** vorhanden sind. Klicken Sie auf der Symbolleiste des Tresors auf **Löschen**.

    ![Sicherungsdaten löschen](./media/backup-azure-delete-vault/delete-vault.png)

    Wenn für den Tresor Sicherungsverwaltungsserver registriert wurden, können Sie den Tresor auch dann nicht löschen, wenn er keine Daten enthält. Wenn Sie der Meinung sind, dass Sie die Sicherungsverwaltungsserver des Tresors gelöscht haben, und im Bereich **Zusammenfassung** trotzdem noch Server angezeigt werden, helfen Ihnen die Informationen unter [Suchen nach Sicherungsverwaltungsservern, die für den Tresor registriert sind](backup-azure-delete-vault.md#find-the-backup-management-servers-registered-to-the-vault) weiter.

5. Klicken Sie auf **Ja**, wenn Sie aufgefordert werden, das Löschen des Tresors zu bestätigen.

    Der Tresor wird gelöscht, und Sie gelangen im Portal zurück zum Dienstmenü **Neu**.


## Löschen eines Tresors zum Schutz eines Produktionsservers

Bevor Sie einen Tresor löschen können, der als Schutz für einen Produktionsserver verwendet wird, müssen Sie den Server löschen oder seine Registrierung für den Tresor aufheben.

So löschen Sie den Produktionsserver, der dem Tresor zugeordnet ist

1. Öffnen Sie im Azure-Portal das Dashboard des Tresors, und klicken Sie auf **Einstellungen** > **Sicherungsinfrastruktur** > **Produktionsserver**.

    ![Blatt „Produktionsserver“ öffnen](./media/backup-azure-delete-vault/delete-production-server.png)

    Das Blatt **Produktionsserver** wird geöffnet und enthält eine Liste mit allen Produktionsservern des Tresors.

    ![Liste mit Produktionsservern](./media/backup-azure-delete-vault/list-of-production-servers.png)

2. Klicken Sie auf dem Blatt **Produktionsserver** mit der rechten Maustaste auf den Server, und klicken Sie dann auf **Löschen**.

    ![Produktionsserver löschen](./media/backup-azure-delete-vault/delete-server-on-production-server-blade.png)

    Das Blatt **Löschen** wird geöffnet.

    ![Produktionsserver löschen](./media/backup-azure-delete-vault/delete-blade.png)

3. Bestätigen Sie auf dem Blatt **Löschen** den Namen des Servers, der gelöscht werden soll, und klicken Sie auf **Löschen**. Sie müssen den Namen des Servers richtig eingeben, um die Schaltfläche **Löschen** zu aktivieren.

    Nachdem der Tresor gelöscht wurde, erhalten Sie eine Meldung mit dem Hinweis, dass der Tresor gelöscht wurde. Scrollen Sie nach dem Löschen aller Server des Tresors zurück zum Bereich „Zusammenfassung“ des Tresor-Dashboards.

4. Stellen Sie im Dashboard des Tresors sicher, dass keine **Sicherungselemente**, **Sicherungsverwaltungsserver** oder **Replizierten Elemente** vorhanden sind. Klicken Sie auf der Symbolleiste des Tresors auf **Löschen**.

5. Klicken Sie auf **Ja**, wenn Sie aufgefordert werden, das Löschen des Tresors zu bestätigen.

    Der Tresor wird gelöscht, und Sie gelangen im Portal zurück zum Dienstmenü **Neu**.


## Löschen eines Backup-Tresors

Die folgende Anleitung gilt für das Löschen eines Backup-Tresors im klassischen Portal. Sowohl für einen Backup-Tresor als auch für einen Recovery Services-Tresor gilt: Bevor Sie den Tresor löschen können, müssen Sie die Elemente und die beibehaltenen Daten löschen.

1. Öffnen Sie das klassische Portal.

2. Wählen Sie in der Liste mit den Sicherungstresoren den Tresor aus, den Sie löschen möchten.

    ![Sicherungsdaten löschen](./media/backup-azure-delete-vault/classic-portal-delete-vault-open-vault.png)

    Das Dashboard des Tresors wird geöffnet. Sehen Sie sich die Anzahl von Windows-Servern und virtuellen Azure-Computern an, die dem Tresor zugeordnet sind. Sehen Sie sich auch die Gesamtmenge des verbrauchten Speichers an. Sie müssen alle Sicherungsaufträge beenden und die vorhandenen Daten löschen, bevor Sie den Tresor löschen können.

3. Klicken Sie auf die Registerkarte **Geschützte Elemente** und dann auf **Schutz beenden**.

    ![Sicherungsdaten löschen](./media/backup-azure-delete-vault/classic-portal-delete-vault-stop-protect.png)

    Das Dialogfeld **Schutz von „Ihr Tresor“ beenden** wird angezeigt.

4. Aktivieren Sie im Dialogfeld **Schutz von „Ihr Tresor“ beenden** das Kontrollkästchen **Zugehörige Sicherungsdaten löschen**, und klicken Sie auf ![Häkchen](./media/backup-azure-delete-vault/checkmark.png). <br/> Optional können Sie einen Grund für das Beenden des Schutzes auswählen und einen Kommentar angeben.

    ![Sicherungsdaten löschen](./media/backup-azure-delete-vault/classic-portal-delete-vault-verify-stop-protect.png)

    Nach dem Löschen der Elemente im Tresor ist der Tresor leer.

    ![Sicherungsdaten löschen](./media/backup-azure-delete-vault/classic-portal-delete-vault-post-delete-data.png)

5. Klicken Sie in der Liste mit den Registerkarten auf **Registrierte Elemente**. Wählen Sie ein im Tresor registriertes Element jeweils aus, und klicken Sie auf **Registrierung aufheben**.

    ![Sicherungsdaten löschen](./media/backup-azure-delete-vault/classic-portal-unregister.png)

6. Klicken Sie in der Liste mit den Registerkarten auf **Dashboard**, um die Registerkarte zu öffnen. Stellen Sie sicher, dass in der Cloud keine registrierten Server oder virtuellen Azure-Computer geschützt sind. Vergewissern Sie sich auch, dass im Speicher keine Daten enthalten sind. Klicken Sie auf **Löschen**, um den Tresor zu löschen.

    ![Sicherungsdaten löschen](./media/backup-azure-delete-vault/classic-portal-list-of-tabs-dashboard.png)

    Der Bestätigungsbildschirm „Sicherung löschen“ des Tresors wird geöffnet. Wählen Sie einen Grund für das Löschen des Tresors aus, und klicken Sie auf ![Häkchen](./media/backup-azure-delete-vault/checkmark.png). <br/>

    ![Sicherungsdaten löschen](./media/backup-azure-delete-vault/classic-portal-delete-vault-confirmation-1.png)

    Der Tresor wird gelöscht, und Sie gelangen zurück zum Dashboard des klassischen Portals.

### Suchen nach Sicherungsverwaltungsservern, die für den Tresor registriert sind

Wenn Sie für einen Tresor mehrere Server registriert haben, kann es schwierig sein, alle im Kopf zu behalten. So zeigen Sie die für den Tresor registrierten Server an und löschen sie

1. Öffnen Sie das Dashboard des Tresors.

2. Klicken Sie im Bereich **Zusammenfassung** auf **Einstellungen**, um das entsprechende Blatt zu öffnen.

    ![Blatt „Einstellungen“ öffnen](./media/backup-azure-delete-vault/backup-vault-click-settings.png)

3. Klicken Sie auf dem Blatt **Einstellungen** auf **Sicherungsinfrastruktur**.

4. Klicken Sie auf dem Blatt **Sicherungsinfrastruktur** auf **Sicherungsverwaltungsserver**. Das Blatt „Sicherungsverwaltungsserver“ wird geöffnet.

    ![Liste mit Sicherungsverwaltungsservern](./media/backup-azure-delete-vault/list-of-backup-management-servers.png)

5. Klicken Sie mit der rechten Maustaste auf den Namen des Servers, und klicken Sie dann auf **Löschen**, um einen Server aus der Liste zu löschen. Das Blatt **Löschen** wird geöffnet.

6. Geben Sie auf dem Blatt **Löschen** den Namen des Servers an. Wenn es sich um einen langen Namen handelt, können Sie ihn auch in der Liste mit den Sicherungsverwaltungsservern kopieren und einfügen. Klicken Sie dann auf **Löschen**.

<!---HONumber=AcomDC_0914_2016-->