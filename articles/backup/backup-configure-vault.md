<properties
	pageTitle="Sichern eines Windows-Servers oder -Clients in Azure mit Azure Backup unter Verwendung des Resource Manager-Bereitstellungsmodells | Microsoft Azure"
	description="Sichern Sie Windows-Server oder -Clients in Azure, indem Sie einen Sicherungstresor erstellen, Anmeldeinformationen herunterladen, den Backup-Agent installieren und eine erste Sicherung Ihrer Dateien und Ordner durchführen."
	services="backup"
	documentationCenter=""
	authors="Jim-Parker"
	manager="jwhit"
	editor=""
	keywords="Sicherungstresor; Windows-Server sichern; Windows sichern;"/>

<tags
	ms.service="backup"
	ms.workload="storage-backup-recovery"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="05/10/2016"
	ms.author="jimpark; trinadhk; markgal"/>

# Sichern eines Windows-Servers oder -Clients in Azure mit Azure Backup unter Verwendung des Resource Manager-Bereitstellungsmodells

> [AZURE.SELECTOR]
- [Azure-Portal](backup-configure-vault.md)
- [Klassisches Portal](backup-configure-vault-classic.md)

In diesem Artikel erfahren Sie, wie Sie Dateien und Ordner eines Windows-Servers (oder Windows-Clients) mithilfe von Azure Backup und des Resource Manager-Bereitstellungsmodells in Azure sichern.

![Sicherungsprozessschritte](./media/backup-configure-vault/initial-backup-process.png)

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]Klassisches Bereitstellungsmodell.

## Vorbereitung
Um einen Server oder Client in Azure zu sichern, benötigen Sie ein Azure-Konto. Falls Sie nicht über ein Konto verfügen, können Sie in nur wenigen Minuten ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen.

## Schritt 1: Erstellen eines Recovery Services-Tresors

Bei einem Recovery Services-Tresor handelt es sich um eine Entität, in der alle Sicherungen und Wiederherstellungspunkte gespeichert werden, die Sie im Laufe der Zeit erstellen. Der Recovery Services-Tresor enthält auch die Sicherungsrichtlinie, die auf die geschützten Dateien und Ordner angewendet wird. Wenn Sie einen Recovery Services-Tresor erstellen, sollten Sie auch die entsprechende Speicherredundanzoption auswählen.

### So erstellen Sie einen Recovery Services-Tresor

1. Melden Sie sich mit Ihrem Azure-Abonnement beim [Azure-Portal](https://portal.azure.com/) an, sofern Sie noch nicht angemeldet sind.

2. Klicken Sie im Hub-Menü auf **Durchsuchen**, und geben Sie in der Liste mit den Ressourcen **Recovery Services** ein. Wenn Sie mit der Eingabe beginnen, wird die Liste anhand Ihrer Eingaben gefiltert. Klicken Sie auf **Recovery Services-Tresore**.

    ![Erstellen eines Recovery Services-Tresors – Schritt 1](./media/backup-configure-vault/browse-to-rs-vaults.png) <br/>

    Die Liste mit den Recovery Services-Tresoren wird angezeigt.

3. Klicken Sie im Menü **Recovery Services-Tresore** auf **Hinzufügen**.

    ![Erstellen eines Recovery Services-Tresors – Schritt 2](./media/backup-configure-vault/rs-vault-menu.png)

    Das Blatt „Recovery Services-Tresor“ wird geöffnet, und Sie werden aufgefordert, **Name**, **Abonnement**, **Ressourcengruppe** und **Standort** anzugeben.

    ![Erstellen eines Recovery Services-Tresors – Schritt 5](./media/backup-configure-vault/rs-vault-attributes.png)

4. Geben Sie unter **Name** einen Anzeigenamen für den Tresor ein. Der Name muss für das Azure-Abonnement eindeutig sein. Geben Sie einen Namen ein, der zwischen 2 und 50 Zeichen enthält. Er muss mit einem Buchstaben beginnen und darf nur Buchstaben, Zahlen und Bindestriche enthalten.

5. Klicken Sie auf **Abonnement**, um die Liste mit den verfügbaren Abonnements anzuzeigen. Falls Sie nicht sicher sind, welches Abonnement geeignet ist, können Sie das Standardabonnement bzw. das vorgeschlagene Abonnement verwenden. Es sind nur dann mehrere Auswahlmöglichkeiten verfügbar, wenn Ihr Organisationskonto mehreren Azure-Abonnements zugeordnet ist.

6. Klicken Sie auf **Ressourcengruppe**, um die Liste mit den verfügbaren Ressourcengruppen anzuzeigen, oder klicken Sie auf **Neu**, um eine neue Ressourcengruppe zu erstellen. Die vollständigen Informationen zu Ressourcengruppen finden Sie unter [Verwenden des Azure-Portals zum Bereitstellen und Verwalten Ihrer Azure-Ressourcen](../azure-portal/resource-group-portal.md).

7. Klicken Sie auf **Standort**, um die geografische Region für den Tresor auszuwählen. Die Auswahl bestimmt die geografische Region, an die Ihre Sicherungsdaten gesendet werden. Wenn Sie eine geografische Region in der Nähe Ihres eigenen Standorts auswählen, können Sie dadurch die Netzwerklatenz beim Sichern in Azure reduzieren.

8. Klicken Sie auf **Erstellen**. Es kann einige Zeit dauern, bis der Recovery Services-Tresor erstellt wurde. Verfolgen Sie die Benachrichtigungen oben rechts im Portal. Nachdem der Tresor erstellt wurde, sollte er im Portal geöffnet werden. Sollte Ihr Tresor nach Abschluss des Vorgangs nicht in der Liste angezeigt werden, klicken Sie auf **Aktualisieren**. Klicken Sie in der aktualisierten Liste auf den Namen des Tresors.

### So bestimmen Sie die Speicherredundanz
Bei der erstmaligen Erstellung eines Recovery Services-Tresors bestimmen Sie, wie der Speicher repliziert werden soll.

1. Klicken Sie auf dem Blatt **Einstellungen**, das automatisch zusammen mit Ihrem Tresordashboard geöffnet wird, auf **Sicherungsinfrastruktur**.

2. Klicken Sie auf dem Blatt „Sicherungsinfrastruktur“ auf **Sicherungskonfiguration**, um den **Speicherreplikationstyp** anzuzeigen.

    ![Erstellen eines Recovery Services-Tresors – Schritt 5](./media/backup-configure-vault/backup-infrastructure.png)

3. Wählen Sie die Speicherreplikationsoption für den Tresor aus.

    ![Liste mit den Recovery Services-Tresoren](./media/backup-configure-vault/choose-storage-configuration.png)

    Standardmäßig verfügt Ihr Tresor über einen georedundanten Speicher. Wenn Sie Azure als primären Speicherendpunkt für die Sicherung verwenden, verwenden Sie weiterhin den georedundanten Speicher. Wenn Sie Azure nicht als primären Speicherendpunkt für die Sicherung verwenden, wählen Sie den lokal redundanten Speicher aus, um die Kosten für die Datenspeicherung in Azure zu verringern. Weitere Informationen zu den Optionen für [georedundanten](../storage/storage-redundancy.md#geo-redundant-storage) und [lokal redundanten](../storage/storage-redundancy.md#locally-redundant-storage) Speicher finden Sie in [dieser Übersicht](../storage/storage-redundancy.md).

    Wenn Sie die Speicheroption für Ihren Tresor ausgewählt haben, können Sie Ihre Dateien und Ordner dem Tresor zuordnen.

Nachdem Sie nun über einen Tresor verfügen, können Sie Ihre Infrastruktur zum Sichern von Dateien und Ordnern vorbereiten, indem Sie den Microsoft Azure Recovery Services-Agent herunterladen und installieren, Tresoranmeldeinformationen herunterladen und dann mit diesen Anmeldeinformationen den Agent beim Tresor registrieren.

## Schritt 2: Herunterladen der Dateien

>[AZURE.NOTE] Die Aktivierung der Sicherung über das Azure-Portal wird in Kürze verfügbar. Aktuell wird der Microsoft Azure Recovery Services-Agent lokal zum Sichern von Dateien und Ordnern verwendet.

1. Klicken Sie auf dem Dashboard des Recovery Services-Tresors auf **Einstellungen**.

    ![Öffnen des Blatts „Backup Goal“ (Sicherungsziel)](./media/backup-configure-vault/settings-button.png)

2. Klicken Sie auf dem Blatt „Einstellungen“ auf **Erste Schritte > Sicherung**.

    ![Öffnen des Blatts „Backup Goal“ (Sicherungsziel)](./media/backup-configure-vault/getting-started-backup.png)

3. Klicken Sie auf dem Blatt „Sicherung“ auf **Sicherungsziel**.

    ![Öffnen des Blatts „Backup Goal“ (Sicherungsziel)](./media/backup-configure-vault/backup-goal.png)

4. Wählen Sie im Menü „Wo wird Ihre Workload ausgeführt?“ die Option **Lokal** aus.

5. Wählen Sie im Menü „Was möchten Sie sichern?“ die Option **Dateien und Ordner** aus, und klicken Sie anschließend auf **OK**.

#### Herunterladen des Recovery Services-Agents

1. Klicken Sie auf dem Blatt **Infrastruktur vorbereiten** auf **Agent für Windows Server oder Windows-Client herunterladen**.

    ![Vorbereiten der Infrastruktur](./media/backup-configure-vault/prepare-infrastructure-short.png)

2. Klicken Sie im Downloadpopupfenster auf **Speichern**. Die Datei **MARSagentinstaller.exe** wird standardmäßig in Ihrem Downloadordner gespeichert.

#### Herunterladen der Tresoranmeldedaten

1. Klicken Sie auf dem Blatt „Infrastruktur vorbereiten“ auf **Herunterladen > Speichern**.

    ![Vorbereiten der Infrastruktur](./media/backup-configure-vault/prepare-infrastructure-download.png)

## Schritt 3: Installieren und Registrieren des Agents

1. Doppelklicken Sie im Downloadordner (bzw. am entsprechenden Speicherort) auf die Datei **MARSagentinstaller.exe**.

2. Führen Sie die Schritte im Setup-Assistenten für den Microsoft Azure Recovery Services Agent aus. Zum Abschließen des Assistenten sind folgende Schritte erforderlich:

    - Wählen Sie einen Speicherort für den Installations- und Cacheordner aus.
    - Geben Sie Ihre Proxyserverinformationen an, wenn Sie einen Proxyserver verwenden, um eine Internetverbindung herzustellen.
    - Geben Sie die Informationen zum Benutzernamen und zum Kennwort ein, wenn Sie einen authentifizierten Proxy verwenden.
    - Angeben der heruntergeladenen Tresoranmeldeinformationen
    - Speichern Sie die Verschlüsselungspassphrase an einem sicheren Ort.

    >[AZURE.NOTE] Wenn Sie die Passphrase verlieren oder vergessen, kann Microsoft Ihnen bei der Wiederherstellung der Sicherungsdaten nicht behilflich sein. Speichern Sie die Datei daher an einem sicheren Ort. Sie ist erforderlich, um eine Sicherung wiederherzustellen.

Der Agent wurde jetzt installiert, und Ihr Computer wurde im Tresor registriert. Sie können die Sicherung jetzt konfigurieren und planen.

## Schritt 4: Abschließen der ersten Sicherung

Die erste Sicherung umfasst zwei wichtige Aufgaben:

- Planen der Sicherung
- Erstmaliges Sichern von Dateien und Ordnern

Für die erste Sicherung verwenden Sie den Microsoft Azure Backup-Agent.

### So planen Sie die Sicherung

1. Öffnen Sie den Microsoft Azure Backup-Agent. Den Agent finden Sie, indem Sie auf Ihrem Computer nach **Microsoft Azure Backup** suchen.

    ![Starten des Azure Backup-Agents](./media/backup-configure-vault/snap-in-search.png)

2. Klicken Sie im Backup-Agent auf **Sicherung planen**.

    ![Planen einer Windows Server-Sicherung](./media/backup-configure-vault/schedule-first-backup.png)

3. Klicken Sie im Assistenten zum Planen der Sicherung auf der Seite mit den ersten Schritten auf **Weiter**.

4. Klicken Sie auf der Seite „Elemente für Sicherung auswählen“ auf **Elemente hinzufügen**.

5. Wählen Sie die Dateien und Ordner aus, die Sie sichern möchten, und klicken Sie auf **OK**.

6. Klicken Sie auf **Weiter**.

7. Geben Sie auf der Seite **Sicherungszeitplan angeben** den **Sicherungszeitplan** an, und klicken Sie auf **Weiter**.

    Sie können tägliche (maximal drei pro Tag) oder wöchentliche Sicherungen planen.

    ![Elemente eines Windows Server-Backups](./media/backup-configure-vault/specify-backup-schedule-close.png)

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

    ![Eigenschaften ändern](./media/backup-configure-vault/change-properties.png)

2. Aktivieren Sie auf der Registerkarte **Drosselung** das Kontrollkästchen **Internet-Bandbreiteneinschränkung für Sicherungsvorgänge aktivieren**.

    ![Netzwerkdrosselung](./media/backup-configure-vault/throttling-dialog.png)

3. Nachdem Sie die Drosselung aktiviert haben, geben Sie die zulässige Bandbreite für die Sicherungsdatenübertragung für **Arbeitsstunden** und **Arbeitsfreie Stunden** an.

    Die Bandbreitenwerte beginnen bei 512 Kilobits pro Sekunde (KBit/s) und können mit bis zu 1.023 Megabits pro Sekunde (MBit/s) angegeben werden. Sie können außerdem den Start und das Ende für die **Arbeitsstunden** angeben und festlegen, welche Tage der Woche als Arbeitstage betrachtet werden. Stunden außerhalb der festgelegten Arbeitsstunden gelten arbeitsfreie Stunden.

4. Klicken Sie auf **OK**.

### So führen Sie erstmals eine Sicherung von Dateien und Ordnern durch

1. Klicken Sie im Backup-Agent auf **Jetzt sichern**, um das erste Seeding über das Netzwerk abzuschließen.

    ![Windows Server jetzt sichern](./media/backup-configure-vault/backup-now.png)

2. Überprüfen Sie auf der Seite „Bestätigung“ die Einstellungen, die vom Assistenten für die sofortige Sicherung zum Sichern des Computers verwendet werden. Klicken Sie dann auf **Sichern**.

3. Klicken Sie auf **Schließen**, um den Assistenten zu schließen. Wenn Sie auf „Schließen“ klicken, bevor der Sicherungsvorgang abgeschlossen ist, wird der Assistent im Hintergrund weiter ausgeführt.

Nach Abschluss der ersten Sicherung wird der Status des Auftrags in der Backup-Konsole als **Auftrag abgeschlossen** angezeigt.

![Abgeschlossen](./media/backup-configure-vault/ircomplete.png)

## Fragen?
Wenn Sie Fragen haben oder Anregungen zu gewünschten Funktionen mitteilen möchten, [senden Sie uns Ihr Feedback](http://aka.ms/azurebackup_feedback).

## Nächste Schritte
Zusätzliche Informationen zum Sichern von virtuellen Computern oder anderen Workloads finden Sie hier:

- Nachdem Sie nun Ihre Dateien und Ordner gesichert haben, können Sie [Ihre Tresore und Server verwalten](backup-azure-manage-windows-server.md).
- Informationen zum Wiederherstellen einer Sicherung finden Sie im Artikel zum [Wiederherstellen von Dateien auf einem Windows-Computer](backup-azure-restore-windows-server.md).

<!---HONumber=AcomDC_0518_2016-->