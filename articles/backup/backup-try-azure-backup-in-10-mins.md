<properties
   pageTitle="Sichern von Dateien und Ordnern aus Windows in Azure | Microsoft Azure"
   description="Erfahren Sie, wie Sie Ihre Windows Server-Daten sichern, indem Sie einen Tresor erstellen, den Backup-Agent installieren und Dateien und Ordner in Azure sichern."
   services="backup"
   documentationCenter=""
   authors="Jim-Parker"
   manager="jwhit"
   editor=""
   keywords="Sichern; Sicherung"/>

<tags
   ms.service="backup"
   ms.workload="storage-backup-recovery"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.date="04/14/2016"
   ms.author="jimpark;"/>

# Einführung: Sichern von Dateien und Ordnern von einem Windows-Server oder -Client in Azure

In diesem Artikel wird beschrieben, wie Sie Windows Server-Dateien und -Ordner (oder Windows-Client) per Azure Backup in Azure sichern. Dies ist ein Tutorial, in dem die Grundlagen beschrieben werden. Wenn Sie mit Azure Backup starten möchten, befinden Sie sich am richtigen Ort.

Falls Sie weitere Informationen zu Azure Backup erhalten möchten, können Sie diese [Übersicht](backup-introduction-to-azure-backup.md) lesen.

Zum Sichern von Dateien und Ordnern in Azure sind die folgenden Aktivitäten erforderlich:

![Schritt 1](./media/backup-try-azure-backup-in-10-mins/step-1.png) Registrieren Sie sich für ein Azure-Abonnement (falls Sie noch keins besitzen).<br> ![Schritt 2](./media/backup-try-azure-backup-in-10-mins/step-2.png) Erstellen Sie einen Sicherungstresor, und laden Sie die erforderlichen Komponenten herunter.<br> ![Schritt 3](./media/backup-try-azure-backup-in-10-mins/step-3.png) Installieren und registrieren Sie den Backup-Agent.<br> ![Schritt 4](./media/backup-try-azure-backup-in-10-mins/step-4.png) Sichern Sie Ihre Dateien und Ordner.


![Sichern Ihres Windows-Computers mit Azure Backup](./media/backup-try-azure-backup-in-10-mins/windows-machine-backup-process.png)

## Schritt 1: Erhalten eines Azure-Abonnements

Falls Sie noch nicht über ein Azure-Abonnement verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, mit dem Sie auf alle Azure-Dienste zugreifen können.

## Schritt 2: Erstellen eines Sicherungstresors und Herunterladen der erforderlichen Komponenten

Um Ihre Dateien und Ordner zu sichern, müssen Sie einen Sicherungstresor in der Region erstellen, in der die Daten gespeichert werden sollen. Außerdem geben Sie an, wie der Speicher repliziert wird, und laden die Anmeldeinformationen und den Backup-Agent herunter.

### Erstellen eines Sicherungstresors

1. Melden Sie sich mit Ihrem Azure-Abonnement beim [Azure-Portal](https://portal.azure.com/) an, sofern Sie noch nicht angemeldet sind.

2. Klicken Sie auf **Neu > Hybridintegration > Sicherung**.

    ![Beginnen mit der Vorbereitung zum Sichern Ihrer Dateien und Ordner](./media/backup-try-azure-backup-in-10-mins/second-blade-backup.png)

3. Geben Sie unter **Name** einen benutzerfreundlichen Namen zur Identifizierung des Sicherungstresors ein.

4. Wählen Sie für **Region** die Region aus, die Ihrem Standort am nächsten liegt, um schnelle Dateiübertragungen zu ermöglichen.

5. Klicken Sie auf **TRESOR ERSTELLEN**.

    ![Erstellen eines Tresors](./media/backup-try-azure-backup-in-10-mins/demo-vault-name.png)

    Wenn der Sicherungstresor fertig ist, wird er in den Ressourcen für Recovery Services als **Aktiv** aufgeführt.

    ![Tresorstatus „Aktiv“](./media/backup-try-azure-backup-in-10-mins/recovery-services-select-vault.png)

Nach dem Erstellen des Tresors wählen Sie aus, wie der Speicher repliziert wird.

>[AZURE.NOTE] Sie müssen sofort nach dem Erstellen eines Tresors und vor der Registrierung von Computern dafür auswählen, wie der Speicher repliziert wird. Nachdem ein Element für den Tresor registriert wurde, wird die Speicherreplikation gesperrt und kann geändert werden.

### So wählen Sie aus, wie der Speicher repliziert wird

1. Klicken Sie auf den Tresor, den Sie erstellt haben.
2. Klicken Sie auf der Seite „Schnellstart“ auf **Konfigurieren**.

    ![Konfigurieren des Tresors](./media/backup-try-azure-backup-in-10-mins/configure-vault.png)

3. Wählen Sie die gewünschte Speicheroption.

    Wählen Sie [georedundanter Speicher](../storage/storage-redundancy.md#geo-redundant-storage) aus, wenn Sie Azure als primäre Sicherung verwenden. Wählen Sie [lokal redundanter Speicher](../storage/storage-redundancy.md#locally-redundant-storage) aus, wenn Sie Azure als tertiäre Sicherung verwenden.

    ![Option zum Auswählen der Speicherreplikation](./media/backup-try-azure-backup-in-10-mins/geo-redundant.png)

4. Klicken Sie bei Auswahl von **Lokal redundant** auf **Speichern**, da **Georedundant** die Standardeinstellung ist.

Sie verwenden Tresoranmeldeinformationen, um Ihren Computer beim Sicherungstresor zu authentifizieren. Hier ist angegeben, wie Sie diese Anmeldeinformationen herunterladen.

### So laden Sie Tresoranmeldeinformationen herunter
Die Datei mit den Tresoranmeldeinformationen wird nur während des Registrierungsprozesses verwendet und läuft nach 48 Stunden ab.

1. Klicken Sie auf ![Auswählen des neuen Tresors](./media/backup-try-azure-backup-in-10-mins/quick-start-icon.png), um zur Seite **Schnellstart** für Ihren Tresor zurückzukehren.

2. Klicken Sie auf **Tresoranmeldeinformationen herunterladen > Speichern**.

Als Nächstes laden Sie den Backup-Agent herunter.

### So laden Sie den Backup-Agent herunter

Klicken Sie auf **Agent für Windows Server oder System Center Data Protection Manager oder Windows Client > Speichern**.

![Speichern des Backup-Agents](./media/backup-try-azure-backup-in-10-mins/agent.png)

Nachdem der Tresor nun erstellt wurde und Sie alles heruntergeladen haben, können Sie den Backup-Agent installieren und registrieren.

## Schritt 3: Installieren und Registrieren des Backup-Agents

1. Doppelklicken Sie am Speicherort auf die Datei **MARSagentinstaller.exe**.
2. Führen Sie die Schritte im Setup-Assistenten für den Microsoft Azure Recovery Services Agent aus. Zum Abschließen des Assistenten sind folgende Schritte erforderlich:
    - Wählen Sie einen Speicherort für den Installations- und Cacheordner aus.
    - Geben Sie Ihre Proxyserverinformationen an, wenn Sie einen Proxyserver verwenden, um eine Internetverbindung herzustellen.
    - Geben Sie die Informationen zum Benutzernamen und zum Kennwort ein, wenn Sie einen authentifizierten Proxy verwenden.
    - Speichern Sie die Verschlüsselungspassphrase an einem sicheren Ort.

    >[AZURE.NOTE] Wenn Sie die Passphrase verlieren oder vergessen, kann Microsoft Ihnen bei der Wiederherstellung der Sicherungsdaten nicht behilflich sein. Speichern Sie die Datei daher an einem sicheren Ort. Sie ist erforderlich, um eine Sicherung wiederherzustellen.

Der Agent wurde jetzt installiert, und Ihr Computer wurde im Tresor registriert. Sie können die Sicherung jetzt konfigurieren und planen.

## Schritt 4: Sichern von Dateien und Ordnern
Falls der Backup-Agent bereits geöffnet ist, finden Sie ihn, indem Sie auf Ihrem Computer nach Microsoft Azure Backup suchen.

1. Klicken Sie im **Backup-Agent** auf **Sicherung planen**.

    ![Planen eines Windows Server-Backups](./media/backup-try-azure-backup-in-10-mins/snap-in-schedule-backup-closeup.png)

2. Führen Sie die Schritte im Assistenten „Sicherung planen“ aus. Dies sind folgende Schritte:

    - Wählen Sie aus, welche Dateien und Ordner Sie sichern möchten.
    - Geben Sie Ihren Sicherungszeitplan an (täglich oder wöchentlich).
    - Legen Sie die Aufbewahrungsrichtlinie fest.
    - Wählen Sie aus, wie Sie die erste Sicherung durchführen möchten (über das Netzwerk oder offline).

    Erfahren Sie mehr zur [Offlinedurchführung der ersten Sicherung](backup-azure-backup-import-export.md). <br><br>

3. Kehren Sie nach Abschluss des Assistenten zum **Backup-Agent** zurück, und klicken Sie auf **Jetzt sichern**, um die erste Sicherung über das Netzwerk durchzuführen.

    ![Windows Server jetzt sichern](./media/backup-try-azure-backup-in-10-mins/backup-now.png)

4. Klicken Sie auf dem Bildschirm **Bestätigung** auf **Sichern**. Wenn Sie den Assistenten schließen, bevor der Sicherungsvorgang abgeschlossen ist, wird er im Hintergrund weiter ausgeführt.

    Nachdem die erste Sicherung abgeschlossen ist, wird in der Ansicht **Aufträge** in der Konsole angezeigt, dass der Auftrag abgeschlossen ist.

    ![Anfängliche Sicherung abgeschlossen](./media/backup-try-azure-backup-in-10-mins/ircomplete.png)

Glückwunsch! Sie haben Ihre Dateien und Ordner erfolgreich mit Azure Backup gesichert.

## Nächste Schritte
- Lesen Sie die weiteren Informationen zum [Sichern von Windows-Computern](backup-configure-vault.md).
- Nachdem Sie nun Ihre Dateien und Ordner gesichert haben, können Sie [Ihre Tresore und Server verwalten](backup-azure-manage-windows-server.md).
- Falls Sie eine Sicherung wiederherstellen müssen, können Sie diesen Artikel zum [Wiederherstellen von Dateien auf einem Windows-Computer](backup-azure-restore-windows-server.md) verwenden.

<!---HONumber=AcomDC_0420_2016-->