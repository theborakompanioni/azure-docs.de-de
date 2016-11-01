<properties
	pageTitle="Sichern eines Windows-Servers oder -Clients in Azure mit dem klassischen Bereitstellungsmodell | Microsoft Azure"
	description="Sichern Sie Windows-Server oder -Clients in Azure, indem Sie einen Sicherungstresor erstellen, Anmeldeinformationen herunterladen, den Backup-Agent installieren und eine erste Sicherung Ihrer Dateien und Ordner durchführen."
	services="backup"
	documentationCenter=""
	authors="markgalioto"
	manager="cfreeman"
	editor=""
	keywords="Sicherungstresor; Windows-Server sichern; Windows sichern;"/>

<tags
	ms.service="backup"
	ms.workload="storage-backup-recovery"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/08/2016"
	ms.author="jimpark; trinadhk; markgal"/>


# Sichern eines Windows-Servers oder -Clients in Azure mit dem klassischen Bereitstellungsmodell

> [AZURE.SELECTOR]
- [Klassisches Portal](backup-configure-vault-classic.md)
- [Azure-Portal](backup-configure-vault.md)

In diesem Artikel werden die Verfahren beschrieben, die Sie zur Vorbereitung Ihrer Umgebung und zum Sichern eines Windows-Servers (oder -Clients) in Azure ausführen müssen. Darüber hinaus erfahren Sie, welche Punkte Sie bei der Bereitstellung Ihrer Sicherungslösung beachten müssen. Wenn Sie Azure Backup das erste Mal einsetzen, finden Sie in diesem Artikel Informationen für den Schnelleinstieg.

![Tresor erstellen](./media/backup-configure-vault-classic/initial-backup-process.png)

>[AZURE.IMPORTANT] Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: Resource Manager-Modell und klassisches Modell. Dieser Artikel befasst sich mit der Verwendung des klassischen Bereitstellungsmodells. Microsoft empfiehlt für die meisten neuen Bereitstellungen die Verwendung des Ressourcen-Manager-Modells.

## Vorbereitung
Um einen Server oder Client in Azure zu sichern, benötigen Sie ein Azure-Konto. Falls Sie nicht über ein Konto verfügen, können Sie in nur wenigen Minuten ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen.

## Schritt 1: Erstellen eines Sicherungstresors
Um Dateien und Ordner von einem Server oder Client in Azure zu sichern, müssen Sie einen Sicherungstresor in der geografischen Region erstellen, in der die Daten gespeichert werden sollen.

### Erstellen eines Sicherungstresors

1. Melden Sie sich beim [klassischen Portal](https://manage.windowsazure.com/) an.

2. Klicken Sie auf **Neu** > **Data Services** > **Recovery Services** > **Sicherungstresor**, und wählen Sie **Schnellerfassung** aus.

3. Geben Sie für den Parameter **Name** einen benutzerfreundlichen Namen für den Sicherungstresor ein. Geben Sie einen Namen ein, der zwischen 2 und 50 Zeichen enthält. Er muss mit einem Buchstaben beginnen und darf nur Buchstaben, Zahlen und Bindestriche enthalten. Dieser Name muss für jedes Abonnement eindeutig sein.

4. Wählen Sie für den Parameter **Region** die geografische Region für den Sicherungstresor aus. Die Auswahl bestimmt die geografische Region, an die Ihre Sicherungsdaten gesendet werden. Wenn Sie eine geografische Region in der Nähe Ihres eigenen Standorts auswählen, können Sie dadurch die Netzwerklatenz beim Sichern in Azure reduzieren.

5. Klicken Sie auf **Tresor erstellen**.

    ![Erstellen eines Sicherungstresors](./media/backup-configure-vault-classic/demo-vault-name.png)

    Es kann eine Weile dauern, bis der Sicherungstresor fertiggestellt wird. Überwachen Sie die Benachrichtigungen unten im klassischen Portal, um den Status zu überprüfen.

    Nach dem erfolgreichen Erstellen des Sicherungstresors wird dies in einer entsprechenden Meldung angezeigt. Er wird zudem in der Ressourcenliste von **Recovery Services** als **Aktiv** angezeigt.

    ![Status zum Erstellen eines Tresors](./media/backup-configure-vault-classic/recovery-services-select-vault.png)

4. Wählen Sie die Speicherredundanzoption, indem Sie die hier beschriebenen Schritte ausführen.

    >[AZURE.IMPORTANT] Der beste Zeitpunkt zum Identifizieren von Speicherredundanzoptionen ist unmittelbar nach der Erstellung des Tresors, bevor Computer beim Tresor registriert werden. Nachdem ein Element beim Tresor registriert wurde, wird die Speicherredundanzoption gesperrt und kann nicht mehr geändert werden.

    Wenn Sie Azure als primären Speicherendpunkt für die Sicherung verwenden (wenn Sie beispielsweise von einem Windows-Server in Azure sichern), sollten Sie die Option [Georedundanter Speicher](../storage/storage-redundancy.md#geo-redundant-storage) (Standard) in Betracht ziehen.

    Wenn Sie Azure als tertiären Speicherendpunkt für die Sicherung verwenden (wenn Sie beispielsweise System Center Data Protection Manager zum Erstellen einer lokalen Sicherungskopie und Azure für die langfristige Aufbewahrung benutzen), sollten Sie ggf. einen [lokal redundanten Speicher](../storage/storage-redundancy.md#locally-redundant-storage) auswählen. Dadurch werden die Kosten zum Speichern von Daten in Azure gesenkt, und es wird eine geringere Dauerhaftigkeit für Ihre Daten bereitgestellt, die möglicherweise für tertiäre Kopien ausreicht.

    **So wählen Sie die Speicherredundanzoption aus**

    a. Klicken Sie auf den Tresor, den Sie gerade erstellt haben.

    b. Wählen Sie auf der Seite „Schnellstart“ **Konfigurieren**.

    ![Status zum Konfigurieren eines Tresors](./media/backup-configure-vault-classic/configure-vault.png)

    c. Wählen Sie die entsprechende Speicherredundanzoption aus.

    Wenn Sie **Lokal redundant** auswählen, müssen Sie auf **Speichern** klicken (weil **Georedundant** die Standardoption ist).

    d. Klicken Sie im linken Navigationsbereich auf **Recovery Services**, um zur Liste mit den Ressourcen für Recovery Services zurückzukehren.

## Schritt 2: Herunterladen der Datei mit Tresoranmeldeinformationen
Der lokale Computer muss bei einem Sicherungstresor authentifiziert werden, bevor Daten in Azure gesichert werden können. Die Authentifizierung erfolgt mithilfe von *Tresoranmeldeinformationen*. Die Datei mit Tresoranmeldeinformationen wird über einen sicheren Kanal aus dem klassischen Portal heruntergeladen. Der private Schlüssel des Zertifikats ist nicht dauerhaft im Verwaltungsportal oder im Dienst gespeichert.

Erfahren Sie mehr über die [Verwendung von Tresoranmeldeinformationen zum Authentifizieren bei dem Backupdienst](backup-introduction-to-azure-backup.md#what-is-the-vault-credential-file).

### So laden Sie die Datei mit den Tresoranmeldeinformationen auf einen lokalen Computer herunter

1. Klicken Sie im linken Navigationsbereich auf **Recovery Services**, und wählen Sie den von Ihnen erstellten Sicherungstresor aus.

    ![Abgeschlossen](./media/backup-configure-vault-classic/rs-left-nav.png)

2.  Klicken Sie auf der Seite "Schnellstart" auf **Tresoranmeldeinformationen herunterladen**.

    Das klassische Portal generiert Tresoranmeldeinformationen mit einer Kombination aus dem Tresornamen und dem aktuellen Datum. Die Datei mit den Tresoranmeldeinformationen wird nur während des Registrierungsworkflows verwendet und läuft nach 48 Stunden ab.

    Sie können die Datei mit den Tresoranmeldeinformationen aus dem Portal herunterladen.

3. Klicken Sie auf **Speichern**, um die Tresoranmeldeinformationen in den Ordner „Downloads“ des lokalen Kontos herunterzuladen. Sie können auch im Menü **Speichern** die Option **Speichern unter** wählen, um einen Speicherort für die Datei mit den Tresoranmeldeinformationen anzugeben.

    >[AZURE.NOTE] Stellen Sie sicher, dass die Datei mit den Tresoranmeldeinformationen an einem Ort gespeichert wird, der von Ihrem Computer aus zugänglich ist. Wenn sie auf einer Dateifreigabe oder in einem Server Message Block gespeichert ist, stellen Sie sicher, dass Sie die entsprechenden Berechtigungen für den Zugriff besitzen.

## Schritt 3: Herunterladen, Installieren und Registrieren des Backup-Agents
Nachdem Sie den Sicherungstresor erstellt und die Datei mit den Anmeldeinformationen heruntergeladen haben, muss ein Agent auf jedem Ihrer Windows-Computer installiert werden.

### So laden Sie den Agent herunter und installieren und registrieren den Agent

1. Klicken Sie auf **Recovery Services**, und wählen Sie den Sicherungstresor aus, den Sie bei einem Server registrieren möchten.

2. Klicken Sie auf der Seite „Schnellstart“ auf **Agent für Windows Server oder System Center Data Protection Manager oder Windows-Client**. Klicken Sie anschließend auf **Save**.

    ![Agent speichern](./media/backup-configure-vault-classic/agent.png)

3. Klicken Sie nach Abschluss des Downloads von „MARSagentInstaller.exe“ auf **Ausführen** (oder doppelklicken Sie am Speicherort auf **MARSAgentInstaller.exe**).

4. Wählen Sie den Installationsordner und den Cacheordner aus, die für den Agent erforderlich sind, und klicken Sie auf **Weiter**. Der von Ihnen angegebene Cachespeicherort muss freien Speicherplatz in einer Größenordnung enthalten, die mindestens 5 Prozent der Sicherungsdaten entspricht.

5. Sie können die Internetverbindung weiterhin mit den standardmäßigen Proxyeinstellungen herstellen. Wenn Sie zum Herstellen der Internetverbindung einen Proxyserver verwenden, aktivieren Sie auf der Seite „Proxykonfiguration“ das Kontrollkästchen **Benutzerdefinierte Proxyeinstellungen verwenden**, und geben Sie die Details für den Proxyserver ein. Wenn Sie einen authentifizierten Proxy verwenden, geben Sie die Informationen zum Benutzernamen und Kennwort ein, und klicken Sie auf **Weiter**.

7. Klicken Sie auf **Installieren**, um mit der Installation des Agents zu beginnen. Der Backup-Agent installiert .NET Framework 4.5 und Windows PowerShell (falls noch nicht geschehen), um die Installation abzuschließen.

8. Klicken Sie nach der Installation des Agents auf **Mit Registrierung fortfahren**, um den Workflow fortzusetzen.

9. Navigieren Sie auf der Seite „Tresor-ID“ zu der Datei mit den Tresoranmeldeinformationen, die Sie zuvor heruntergeladen haben, und wählen Sie sie aus.

    Die Datei mit den Tresoranmeldeinformationen ist nach dem Herunterladen aus dem Portal nur 48 Stunden lang gültig. Wenn auf dieser Seite Fehler auftreten (z.B. „Datei mit Tresoranmeldeinformationen abgelaufen“), melden Sie sich beim Portal an, und laden Sie die Datei mit den Tresoranmeldeinformationen erneut herunter.

    Stellen Sie sicher, dass die Datei mit den Tresoranmeldeinformationen an einem Speicherort verfügbar ist, der für die Setupanwendung zugänglich ist. Wenn Zugriffsfehler auftreten, kopieren Sie die Datei mit den Tresoranmeldeinformationen in einen temporären Speicherort auf denselben Computer, und wiederholen Sie den Vorgang.

    Wenn ein Fehler in Bezug auf die Tresoranmeldeinformationen angezeigt wird (z.B. „Ungültige Tresoranmeldeinformationen angegeben“), ist die Datei entweder beschädigt oder enthält nicht die aktuellen Anmeldeinformationen, die dem Wiederherstellungsdienst zugeordnet sind. Wiederholen Sie den Vorgang, nachdem Sie eine neue Datei mit Tresoranmeldeinformationen vom Portal heruntergeladen haben. Dieser Fehler kann auch auftreten, wenn der Benutzer mehrmals hintereinander schnell auf die Option **Tresoranmeldedaten herunterladen** klickt. In diesem Fall ist nur die zuletzt heruntergeladene Datei mit Tresoranmeldeinformationen gültig.

9. Auf der Seite Bildschirm „Verschlüsselungseinstellung“ können Sie entweder eine Passphrase generieren oder eine Passphrase angeben (mindestens 16 Zeichen). Vergessen Sie nicht, die Passphrase an einem sicheren Speicherort zu speichern.

10. Klicken Sie auf **Fertig stellen**. Mit dem Assistenten zum Registrieren von Servern wird der Server bei Backup registriert.

    >[AZURE.WARNING] Wenn Sie die Passphrase verlieren oder vergessen, kann Microsoft Ihnen bei der Wiederherstellung der Sicherungsdaten nicht behilflich sein. Sie sind im Besitz der Verschlüsselungspassphrase, und Microsoft kann nicht auf die von Ihnen verwendete Passphrase zugreifen. Speichern Sie die Datei an einem sicheren Ort, da sie bei einem Wiederherstellungsvorgang erforderlich ist.

11. Lassen Sie nach dem Festlegen des Verschlüsselungsschlüssels das Kontrollkästchen **Microsoft Azure Recovery Services-Agent starten** aktiviert, und klicken Sie dann auf **Schließen**.

## Schritt 4: Abschließen der ersten Sicherung

Die erste Sicherung umfasst zwei wichtige Aufgaben:

- Erstellen des Sicherungszeitplans
- Erstmaliges Sichern von Dateien und Ordnern

Wenn die erste Sicherung von der Sicherungsrichtlinie abgeschlossen wird, werden Sicherungspunkte erstellt, die Sie zum Wiederherstellen der Daten verwenden können. Die Sicherungsrichtlinie führt diesen Schritt basierend auf dem von Ihnen erstellten Zeitplan aus.

### So planen Sie die Sicherung

1. Öffnen Sie den Microsoft Azure Backup-Agent. (Er wird automatisch geöffnet, wenn Sie beim Schließen des Assistenten zum Registrieren von Servern das Kontrollkästchen **Microsoft Azure Recovery Services-Agent starten** aktiviert gelassen haben.) Den Agent finden Sie, indem Sie auf Ihrem Computer nach **Microsoft Azure Backup** suchen.

    ![Starten des Azure Backup-Agents](./media/backup-configure-vault-classic/snap-in-search.png)

2. Klicken Sie im Backup-Agent auf **Sicherung planen**.

    ![Planen einer Windows Server-Sicherung](./media/backup-configure-vault-classic/schedule-backup-close.png)

3. Klicken Sie im Assistenten zum Planen der Sicherung auf der Seite mit den ersten Schritten auf **Weiter**.

4. Klicken Sie auf der Seite „Elemente für Sicherung auswählen“ auf **Elemente hinzufügen**.

5. Wählen Sie die Dateien und Ordner aus, die Sie sichern möchten, und klicken Sie auf **OK**.

6. Klicken Sie auf **Weiter**.

7. Geben Sie auf der Seite **Sicherungszeitplan angeben** den **Sicherungszeitplan** an, und klicken Sie auf **Weiter**.

    Sie können tägliche (maximal drei pro Tag) oder wöchentliche Sicherungen planen.

    ![Elemente einer Windows Server-Sicherung](./media/backup-configure-vault-classic/specify-backup-schedule-close.png)

    >[AZURE.NOTE] Weitere Informationen zum Angeben des Sicherungszeitplans finden Sie im Artikel [Verwenden von Azure Backup als Ersatz für Ihre Bandinfrastruktur](backup-azure-backup-cloud-as-tape.md).

8. Wählen Sie auf der Seite **Aufbewahrungsrichtlinie auswählen** die **Aufbewahrungsrichtlinie** für die Sicherungskopie aus.

    Die Aufbewahrungsrichtlinie gibt an, wie lange die Sicherung gespeichert werden soll. Statt lediglich eine einfache Richtlinie für alle Sicherungspunkte anzugeben, können Sie je nach Sicherungszeitpunkt verschiedene Aufbewahrungsrichtlinien festlegen. Sie können die tägliche, wöchentliche, monatliche und jährliche Aufbewahrungsrichtlinie an Ihre Anforderungen anpassen.

9. Wählen Sie auf der Seite „Erstsicherungstyp wählen“ den Typ für die erste Sicherung. Lassen Sie die Option **Automatisch über das Netzwerk** aktiviert, und klicken Sie auf **Weiter**.

    Sie können die Sicherung automatisch über das Netzwerk oder offline durchführen. In den verbleibenden Abschnitten dieses Artikels wird das Verfahren für eine automatische Sicherung beschrieben. Falls Sie vorziehen, eine Offlinesicherung durchzuführen, finden Sie unter [Workflow zur Offlinesicherung in Azure Backup](backup-azure-backup-import-export.md) weitere Informationen.

10. Lesen Sie sich die Informationen auf der Seite „Bestätigung“ durch, und klicken Sie dann auf **Fertig stellen**.

11. Klicken Sie auf **Schließen**, nachdem der Assistent die Erstellung des Sicherungszeitplans abgeschlossen hat.

### Aktivieren der Netzwerkdrosselung (optional)

Der Backup-Agent ermöglicht die Netzwerkdrosselung. Über die Drosselung wird die Verwendung der Netzwerkbandbreite bei der Datenübertragung gesteuert. Diese Steuerungsmöglichkeit kann hilfreich sein, wenn Sie Daten während der Geschäftszeiten sichern möchten, der Sicherungsprozess aber keine Auswirkung auf den weiteren Internetdatenverkehr haben soll. Die Drosselung gilt für Sicherungs- und Wiederherstellungsaktivitäten.

**So aktivieren Sie die Netzwerkdrosselung**

1. Klicken Sie im Backup-Agent auf **Eigenschaften ändern**.

    ![Eigenschaften ändern](./media/backup-configure-vault-classic/change-properties.png)

2. Aktivieren Sie auf der Registerkarte **Drosselung** das Kontrollkästchen **Internet-Bandbreiteneinschränkung für Sicherungsvorgänge aktivieren**.

    ![Netzwerkdrosselung](./media/backup-configure-vault-classic/throttling-dialog.png)

3. Nachdem Sie die Drosselung aktiviert haben, geben Sie die zulässige Bandbreite für die Sicherungsdatenübertragung für **Arbeitsstunden** und **Arbeitsfreie Stunden** an.

    Die Bandbreitenwerte beginnen bei 512 Kilobits pro Sekunde (KBit/s) und können mit bis zu 1.023 Megabits pro Sekunde (MBit/s) angegeben werden. Sie können außerdem den Start und das Ende für die **Arbeitsstunden** angeben und festlegen, welche Tage der Woche als Arbeitstage betrachtet werden. Stunden außerhalb der festgelegten Arbeitsstunden gelten arbeitsfreie Stunden.

4. Klicken Sie auf **OK**.

### So führen Sie eine sofortige Sicherung aus

1. Klicken Sie im Backup-Agent auf **Jetzt sichern**, um das erste Seeding über das Netzwerk abzuschließen.

    ![Windows Server jetzt sichern](./media/backup-configure-vault-classic/backup-now.png)

2. Überprüfen Sie auf der Seite „Bestätigung“ die Einstellungen, die vom Assistenten für die sofortige Sicherung zum Sichern des Computers verwendet werden. Klicken Sie dann auf **Sichern**.

3. Klicken Sie auf **Schließen**, um den Assistenten zu schließen. Wenn Sie auf „Schließen“ klicken, bevor der Sicherungsvorgang abgeschlossen ist, wird der Assistent im Hintergrund weiter ausgeführt.

Nach Abschluss der ersten Sicherung wird der Status des Auftrags in der Backup-Konsole als **Auftrag abgeschlossen** angezeigt.

![Abgeschlossen](./media/backup-configure-vault-classic/ircomplete.png)

## Nächste Schritte
- Registrieren Sie sich für ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/).

Zusätzliche Informationen zum Sichern von virtuellen Computern oder anderen Workloads finden Sie hier:

- [Sichern von IaaS-VMs](backup-azure-vms-prepare.md)
- [Sichern von Workloads in Azure mit Microsoft Azure Backup Server](backup-azure-microsoft-azure-backup.md)
- [Sichern von Workloads in Azure mit DPM](backup-azure-dpm-introduction.md)

<!---HONumber=AcomDC_0810_2016-->