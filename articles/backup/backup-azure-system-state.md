---
title: Sichern des Windows-Systemstatus in Azure | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie den Systemstatus von Windows Server und/oder Windows-Computern in Azure sichern.
services: backup
documentationcenter: 
author: saurabhsensharma
manager: carmonm
editor: 
keywords: Sichern; Sicherung; Sichern von Dateien und Ordnern
ms.assetid: 5b15ebf1-2214-4722-b937-96e2be8872bb
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: saurse;markgal
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: 6fbd96935f444d8b0c6d068ebd0d28e612f19816
ms.contentlocale: de-de
ms.lasthandoff: 08/21/2017

---
# <a name="back-up-windows-system-state-in-resource-manager-deployment"></a>Sichern des Windows-Systemstatus in der Ressourcen Manager-Bereitstellung
Dieser Artikel beschreibt, wie Sie den Systemstatus von Windows Server in Azure sichern. Dies ist ein Tutorial, in dem die Grundlagen beschrieben werden.

Falls Sie weitere Informationen zu Azure Backup erhalten möchten, können Sie diese [Übersicht](backup-introduction-to-azure-backup.md)lesen.

Falls Sie noch nicht über ein Azure-Abonnement verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, mit dem Sie auf alle Azure-Dienste zugreifen können.

## <a name="create-a-recovery-services-vault"></a>Erstellen eines Recovery Services-Tresors
Um Ihre Dateien und Ordner zu sichern, müssen Sie einen Recovery Services-Tresor in der Region erstellen, in der die Daten gespeichert werden sollen. Außerdem müssen Sie festlegen, wie der Speicher repliziert werden soll.

### <a name="to-create-a-recovery-services-vault"></a>So erstellen Sie einen Recovery Services-Tresor
1. Melden Sie sich mit Ihrem Azure-Abonnement beim [Azure-Portal](https://portal.azure.com/) an, sofern Sie noch nicht angemeldet sind.
2. Klicken Sie im Hub-Menü auf **More Services** (Weitere Dienste), geben Sie in der Ressourcenliste **Recovery Services** ein, und klicken Sie auf **Recovery Services-Tresore**.

    ![Erstellen eines Recovery Services-Tresors – Schritt 1](./media/backup-try-azure-backup-in-10-mins/open-rs-vault-list.png) <br/>

    Wenn Recovery Services-Tresore im Abonnement vorhanden sind, werden die Tresore aufgeführt.
3. Klicken Sie im Menü **Recovery Services-Tresore** auf **Hinzufügen**.

    ![Erstellen eines Recovery Services-Tresors – Schritt 2](./media/backup-try-azure-backup-in-10-mins/rs-vault-menu.png)

    Das Blatt „Recovery Services-Tresor“ wird geöffnet, und Sie werden aufgefordert, **Name**, **Abonnement**, **Ressourcengruppe** und **Standort** anzugeben.

    ![Erstellen eines Recovery Services-Tresors – Schritt 3](./media/backup-try-azure-backup-in-10-mins/rs-vault-step-3.png)

4. Geben Sie unter **Name**einen Anzeigenamen für den Tresor ein. Der Name muss für das Azure-Abonnement eindeutig sein. Geben Sie einen Namen ein, der zwischen 2 und 50 Zeichen enthält. Er muss mit einem Buchstaben beginnen und darf nur Buchstaben, Zahlen und Bindestriche enthalten.

5. Wählen Sie im Abschnitt **Abonnement** über das Dropdownmenü das Azure-Abonnement aus. Wenn Sie nur ein Abonnement verwenden, wird dieses Abonnement angezeigt, und Sie können mit dem nächsten Schritt fortfahren. Falls Sie nicht sicher sind, welches Abonnement geeignet ist, können Sie das Standardabonnement bzw. das vorgeschlagene Abonnement verwenden. Es sind nur dann mehrere Auswahlmöglichkeiten verfügbar, wenn Ihr Organisationskonto mehreren Azure-Abonnements zugeordnet ist.

6. Gehen Sie im Abschnitt **Ressourcengruppe** wie folgt vor:

    * Wählen Sie **Neu erstellen** aus, wenn Sie eine Ressourcengruppe erstellen möchten.
    Oder
    * Wählen Sie **Use existing** (Vorhandene verwenden) aus, und klicken Sie auf das Dropdownmenü, um eine Liste mit verfügbaren Ressourcengruppen anzuzeigen.

  Umfassende Informationen zu Ressourcengruppen finden Sie in der [Übersicht über Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).

7. Klicken Sie auf **Standort** , um die geografische Region für den Tresor auszuwählen. Die Auswahl bestimmt die geografische Region, an die Ihre Sicherungsdaten gesendet werden.

8. Klicken Sie im unteren Bereich des Blatts für den Recovery Services-Tresor auf **Erstellen**.

    Die Erstellung des Recovery Services-Tresors kann mehrere Minuten dauern. Verfolgen Sie die Statusbenachrichtigungen rechts oben im Portal. Nach Abschluss des Erstellungsvorgangs wird der Tresor in der Liste mit den Recovery Services-Tresoren angezeigt. Sollte der Tresor auch nach mehreren Minuten nicht angezeigt werden, klicken Sie auf **Aktualisieren**.

    ![Klicken auf die Schaltfläche „Aktualisieren“](./media/backup-try-azure-backup-in-10-mins/refresh-button.png)</br>

    Sobald Ihr Tresor in der Liste mit den Recovery Services-Tresoren angezeigt wird, können Sie die Speicherredundanz festlegen.

### <a name="set-storage-redundancy-for-the-vault"></a>Festlegen der Speicherredundanz für den Tresor
Vergewissern Sie sich beim Erstellen eines Recovery Services-Tresors, dass die Speicherredundanz wie gewünscht konfiguriert ist.

1. Klicken Sie auf dem Blatt **Recovery Services-Tresore** auf den neuen Tresor.

    ![Auswählen des neuen Tresors in der Liste mit Recovery Services-Tresoren](./media/backup-try-azure-backup-in-10-mins/rs-vault-list.png)

    Wenn Sie den Tresor auswählen, wird das Blatt **Recovery Services-Tresor** schmaler, und das Blatt mit den Einstellungen (*auf dem oben der Name des Tresors angegeben ist*) sowie das Blatt mit den Tresordetails werden geöffnet.

    ![Anzeigen der Speicherkonfiguration für den neuen Tresor](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration-2.png)
2. Scrollen Sie auf dem Blatt mit den Einstellungen des neuen Tresors mithilfe des vertikalen Schiebereglers nach unten zum Verwaltungsabschnitt, und klicken Sie auf **Sicherungsinfrastruktur**.
    Das Blatt der Sicherungsinfrastruktur wird geöffnet.
3. Klicken Sie auf dem Blatt der Sicherungsinfrastruktur auf **Sicherungskonfiguration**, um das Blatt **Sicherungskonfiguration** zu öffnen.

    ![Speicherkonfiguration für neuen Tresor festlegen](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration.png)
4. Wählen Sie die passende Speicherreplikationsoption für Ihren Tresor aus.

    ![Speicherkonfigurationsoptionen](./media/backup-try-azure-backup-in-10-mins/choose-storage-configuration.png)

    Standardmäßig verfügt Ihr Tresor über einen georedundanten Speicher. Wenn Sie Azure als primären Speicherendpunkt für die Sicherung verwenden, verwenden Sie weiterhin die Option **Georedundant**. Wenn Sie Azure nicht als primären Speicherendpunkt für die Sicherung verwenden, wählen Sie **Lokal redundant** aus. Dadurch verringern sich die Kosten für Azure-Speicher. Weitere Informationen zu den Optionen für [georedundanten](../storage/common/storage-redundancy.md#geo-redundant-storage) und [lokal redundanten](../storage/common/storage-redundancy.md#locally-redundant-storage) Speicher finden Sie in [dieser Übersicht über Speicherredundanz](../storage/common/storage-redundancy.md).

Sie haben einen Tresor erstellt und können ihn nun für das Sichern des Windows-Systemstatus konfigurieren.

## <a name="configure-the-vault"></a>Konfigurieren des Tresors
1. Klicken Sie auf dem Blatt „Recovery Services-Tresor“ (für den gerade erstellten Tresor) im Abschnitt „Erste Schritte“ auf **Backup**. Wählen Sie dann auf dem Blatt **Erste Schritte mit der Sicherung** die Option **Sicherungsziel** aus.

    ![Öffnen des Blatts „Backup Goal“ (Sicherungsziel)](./media/backup-try-azure-backup-in-10-mins/open-backup-settings.png)

    Das Blatt **Sicherungsziel** wird geöffnet.

    ![Öffnen des Blatts „Backup Goal“ (Sicherungsziel)](./media/backup-try-azure-backup-in-10-mins/backup-goal-blade.png)

2. Wählen Sie im Dropdownmenü **Wo wird Ihre Workload ausgeführt?** die Option **Lokal** aus.

    Sie wählen **Lokal**, da Ihr Windows Server- oder Windows-Computer ein physischer Computer ist, der sich nicht in Azure befindet.

3. Wählen Sie im Menü **Was möchten Sie sichern?** die Option **Systemstatus** aus, und klicken Sie anschließend auf **OK**.

    ![Konfigurieren von Dateien und Ordnern](./media/backup-azure-system-state/backup-goal-system-state.png)

    Nach dem Klicken auf „OK“ erscheint ein Häkchen neben **Sicherungsziel**, und das Blatt **Vorbereiten der Infrastruktur** wird geöffnet.

    ![Sicherungsziel konfiguriert, dann Vorbereiten der Infrastruktur](./media/backup-try-azure-backup-in-10-mins/backup-goal-configed.png)

4. Klicken Sie auf dem Blatt **Vorbereiten der Infrastruktur** auf **Agent für Windows Server oder Windows-Client herunterladen**.

    ![Agent für Windows Server oder Windows-Client herunterladen](./media/backup-try-azure-backup-in-10-mins/choose-agent-for-server-client.png)

    Wenn Sie Windows Server Essential verwenden, laden Sie den Agent für Windows Server Essential herunter. Sie werden in einem Popupmenü aufgefordert, „MARSAgentInstaller.exe“ auszuführen oder zu speichern.

    ![Dialogfeld „MARSAgentInstaller“](./media/backup-try-azure-backup-in-10-mins/mars-installer-run-save.png)

5. Klicken Sie im Downloadpopupmenü auf **Speichern**.

    Die Datei **MARSagentinstaller.exe** wird standardmäßig in Ihrem Downloadordner gespeichert. Nachdem das Installationsprogramm heruntergeladen wurde, erscheint ein Popup mit der Frage, ob Sie das Installationsprogramm ausführen oder den Ordner öffnen möchten.

    ![Agent für Windows Server oder Windows-Client herunterladen](./media/backup-try-azure-backup-in-10-mins/mars-installer-complete.png)

    Sie müssen den Agent noch nicht installieren. Sie können den Agent installieren, nachdem Sie die Anmeldeinformationen für den Tresor heruntergeladen haben.

6. Klicken Sie auf dem Blatt **Vorbereiten der Infrastruktur** auf **Herunterladen**.

    ![Herunterladen der Tresoranmeldeinformationen](./media/backup-try-azure-backup-in-10-mins/download-vault-credentials.png)

    Die Anmeldeinformationen für den Tresor werden in den Ordner „Downloads“ heruntergeladen. Nach dem Herunterladen der Anmeldeinformationen für den Tresor erscheint ein Popup mit der Frage, ob Sie die Anmeldeinformationen öffnen oder speichern möchten. Klicken Sie auf **Speichern**. Wenn Sie versehentlich auf **Öffnen** klicken, brechen Sie den Vorgang zum Öffnen der Anmeldeinformationen für den Tresor ab. Sie können die Anmeldeinformationen für den Tresor nicht öffnen. Fahren Sie mit dem nächsten Schritt fort. Die Anmeldeinformationen für den Tresor befinden sich im Ordner „Downloads“.   

    ![Herunterladen der Tresoranmeldeinformationen abgeschlossen](./media/backup-try-azure-backup-in-10-mins/vault-credentials-downloaded.png)

## <a name="install-and-register-the-agent"></a>Installieren und Registrieren des Agents

> [!NOTE]
> Die Sicherung kann noch nicht über das Azure-Portal aktiviert werden. Verwenden Sie den Microsoft Azure Recovery Services-Agent, um den Systemstatus von Windows Server zu sichern.
>

1. Doppelklicken Sie im Downloadordner (bzw. am entsprechenden Speicherort) auf die Datei **MARSagentinstaller.exe**.

    Das Installationsprogramm sendet eine Reihe von Nachrichten, während es den Recovery Services-Agent extrahiert, installiert und registriert.

    ![Anmeldeinformationen zum Ausführen des Installationsprogramms für den Recovery Services-Agent](./media/backup-try-azure-backup-in-10-mins/mars-installer-registration.png)

2. Führen Sie die Schritte im Setup-Assistenten für den Microsoft Azure Recovery Services Agent aus. Zum Abschließen des Assistenten sind folgende Schritte erforderlich:

   * Wählen Sie einen Speicherort für den Installations- und Cacheordner aus.
   * Geben Sie Ihre Proxyserverinformationen an, wenn Sie einen Proxyserver verwenden, um eine Internetverbindung herzustellen.
   * Geben Sie die Informationen zum Benutzernamen und zum Kennwort ein, wenn Sie einen authentifizierten Proxy verwenden.
   * Angeben der heruntergeladenen Tresoranmeldeinformationen
   * Speichern Sie die Verschlüsselungspassphrase an einem sicheren Ort.

     > [!NOTE]
     > Wenn Sie die Passphrase verlieren oder vergessen, kann Microsoft Ihnen bei der Wiederherstellung der Sicherungsdaten nicht behilflich sein. Speichern Sie die Datei daher an einem sicheren Ort. Sie ist erforderlich, um eine Sicherung wiederherzustellen.
     >
     >

Der Agent wurde jetzt installiert, und Ihr Computer wurde im Tresor registriert. Sie können die Sicherung jetzt konfigurieren und planen.

## <a name="back-up-windows-server-system-state-preview"></a>Sichern des Systemstatus von Windows Server (Vorschau)
Die erste Sicherung umfasst drei wichtige Aufgaben:

* Aktivieren der Sicherung des Systemstatus mithilfe des Azure Backup-Agent
* Planen der Sicherung
* Erstmaliges Sichern von Dateien und Ordnern

Für die erste Sicherung verwenden Sie den Microsoft Azure Recovery Services-Agent.

### <a name="to-enable-system-state-backup-using-the-azure-backup-agent"></a>So aktivieren Sie die Sicherung des Systemstatus mithilfe des Azure Backup-Agent

1. Führen Sie den folgenden Befehl zum Beenden des Azure Backup-Moduls in einer PowerShell-Sitzung aus.

  ```
  PS C:\> Net stop obengine
  ```

2. Öffnen Sie die Windows-Registrierung.

  ```
  PS C:\> regedit.exe
  ```

3. Fügen Sie den folgenden Registrierungsschlüssel mit dem angegebenen DWORD-Wert hinzu.

  | Registrierungspfad | Registrierungsschlüssel | DWORD-Wert |
  |---------------|--------------|-------------|
  | HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider | TurnOffSSBFeature | 2 |

4. Starten Sie das Backup-Modul neu, indem Sie den folgenden Befehl an einer Eingabeaufforderung mit erhöhten Rechten ausführen.

  ```
  PS C:\> Net start obengine
  ```

### <a name="to-schedule-the-backup-job"></a>So planen Sie den Sicherungsauftrag

1. Öffnen Sie den Microsoft Azure Recovery Services-Agent. Den Agent finden Sie, indem Sie auf Ihrem Computer nach **Microsoft Azure Backup**suchen.

    ![Starten des Azure Recovery Services-Agents](./media/backup-try-azure-backup-in-10-mins/snap-in-search.png)

2. Klicken Sie im der Recovery Services-Agent auf **Sicherung planen**.

    ![Planen einer Windows Server-Sicherung](./media/backup-try-azure-backup-in-10-mins/schedule-first-backup.png)

3. Klicken Sie im Assistenten zum Planen der Sicherung auf der Seite mit den ersten Schritten auf **Weiter**.

4. Klicken Sie auf der Seite „Elemente für Sicherung auswählen“ auf **Elemente hinzufügen**.

5. Wählen Sie **Systemstatus** aus, und klicken Sie anschließend auf **OK**.

6. Klicken Sie auf **Weiter**.


7. Die Systemstatussicherung und der Aufbewahrungszeitplan sind so festgelegt, dass jeden Sonntag um 21:00 Uhr Ortszeit eine automatische Sicherung durchgeführt wird, und die Beibehaltungsdauer ist auf 60 Tage festgelegt.

   > [!NOTE]
   > Die Systemstatussicherung und die Beibehaltungsrichtlinie sind automatisch konfiguriert. Wenn Sie Dateien und Ordner zusätzlich zum Windows Server-Systemstatus sichern, geben Sie nur die Sicherungs- und Beibehaltungsrichtlinie für Dateisicherungen des Assistenten an. 
   >

8. Lesen Sie sich die Informationen auf der Seite „Bestätigung“ durch, und klicken Sie dann auf **Fertig stellen**.

9. Klicken Sie auf **Schließen**, nachdem der Assistent die Erstellung des Sicherungszeitplans abgeschlossen hat.

### <a name="to-back-up-windows-server-system-state-for-the-first-time"></a>So sichern Sie den Windows Server-Systemstatus zum ersten Mal

1. Stellen Sie sicher, dass es keine ausstehenden Updates für Windows Server gibt, die einen Neustart erfordern.

2. Klicken Sie im Recovery Services-Agent auf **Jetzt sichern** , um das anfängliche Seeding über das Netzwerk abzuschließen.

    ![Windows Server – Jetzt sichern](./media/backup-try-azure-backup-in-10-mins/backup-now.png)

3. Überprüfen Sie auf der Seite „Bestätigung“ die Einstellungen, die vom Assistenten für die sofortige Sicherung zum Sichern des Computers verwendet werden. Klicken Sie dann auf **Sichern**.

4. Klicken Sie auf **Schließen** , um den Assistenten zu schließen. Wenn Sie den Assistenten schließen, bevor der Sicherungsvorgang abgeschlossen ist, wird der Assistent im Hintergrund weiter ausgeführt.

5. Wenn Sie Dateien und Ordner auf Ihrem Server zusätzlich zum Windows Server-Systemstatus sichern, wird der Assistent für die sofortige Sicherung nur Dateien sichern. Verwenden Sie zum Ausführen einer Ad-hoc-Sicherung des Systemstatus den folgenden PowerShell-Befehl:

    ```
    PS C:\> Start-OBSystemStateBackup
    ```

  Nach Abschluss des ersten Backups wird der Status des Auftrags in der Backup-Konsole als **Auftrag abgeschlossen** angezeigt.

  ![Abgeschlossen](./media/backup-try-azure-backup-in-10-mins/ircomplete.png)

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

Die folgenden Fragen und Antworten bieten zusätzliche Informationen.

### <a name="what-is-the-staging-volume"></a>Was ist das Staging-Volume?

Das Staging-Volume stellt den temporären Speicherort dar, in dem die systemintern verfügbare Windows Server-Sicherung die Sicherung des Systemstatus bereitstellt. Der Azure Backup-Agent komprimiert und verschlüsselt anschließend diese temporäre Sicherung und sendet sie an den konfigurierten Recovery Services-Tresor über ein sicheres HTTPS-Protokoll. **Es wird dringend empfohlen, das Staging-Volume in einem Nicht-Windows-Betriebssystemvolume einzurichten. Wenn Sie Probleme mit Sicherungen des Systemstatus bemerken, ist das Überprüfen des Speicherorts Ihres Staging-Volumes der erste durchzuführende Schritt zur Problembehandlung.** 

### <a name="how-can-i-change-the-staging-volume-path-specified-in-the-azure-backup-agent"></a>Wie kann ich den im Azure Backup-Agent angegebenen Staging-Volume-Pfad ändern?

Das Staging-Volume befindet sich standardmäßig im Ordner „Cache“. 

1. Um diesen Speicherort zu ändern, verwenden Sie den folgenden Befehl (in einer Eingabeaufforderung mit erhöhten Rechten):
  ```
  PS C:\> Net stop obengine
  ```

2. Aktualisieren Sie die folgenden Registrierungseinträge mit dem Pfad zum neuen Staging-Volume-Ordner.

  |Registrierungspfad|Registrierungsschlüssel|Wert|
  |-------------|------------|-----|
  |HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider | SSBStagingPath | Neuer Staging-Volume-Speicherort |

Beim Staging-Pfad muss die Groß-/Kleinschreibung beachtet werden, und es muss exakt dieselbe Groß-/Kleinschreibung wie auf dem Server verwendet werden. 

3. Nachdem Sie den Pfad des Staging-Volumes verändert haben, starten Sie das Modul für die Sicherung neu:
  ```
  PS C:\> Net start obengine
  ```
4. Damit der geänderte Pfad übernommen wird, öffnen Sie den Microsoft Azure Recovery Services-Agent und lösen eine Ad-hoc-Sicherung des Systemstatus aus.

### <a name="why-is-the-system-state-default-retention-set-to-60-days"></a>Warum wird die Standardaufbewahrungsdauer des Systemstatus auf 60 Tage festgelegt?

Die Nutzungsdauer einer Systemstatus-Sicherung ist identisch mit der Einstellung „Tombstone-Lebensdauer“ für die Rolle „Windows Server Active Directory“. Der Standardwert für den Eintrag der Tombstone-Lebensdauer beträgt 60 Tage. Dieser Wert kann für den Verzeichnisdienst (NTDS) „Config-Objekt“ festgelegt werden.

### <a name="how-do-i-change-the-default-backup-and-retention-policy-for-system-state"></a>Wie ändere ich die standardmäßige Sicherung und die Aufbewahrungsrichtlinie für den Systemstatus?

So ändern Sie die standardmäßige Sicherung und die Aufbewahrungsrichtlinie für den Systemstatus
1. Beenden Sie das Modul für die Sicherung. Führen Sie den folgenden Befehl in einer Eingabeaufforderung mit erhöhten Rechten aus:

  ```
  PS C:\> Net stop obengine
  ```

2. Fügen Sie die folgenden Registrierungsschlüsseleinträge hinzu oder aktualisieren sie diese in HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider.

  |Registrierungsname|Beschreibung|Wert|
  |-------------|-----------|-----|
  |SSBScheduleTime|Wird zum Konfigurieren des Zeitpunkts der Sicherung verwendet. Der Standardwert ist 21: 00 Uhr Ortszeit.|DWORD: Format HHMM (dezimal), z.B. 2130 für 9:30 Uhr Ortszeit|
  |SSBScheduleDays|Wird verwendet, um die Tage zu konfigurieren, an denen die Sicherung des Systemstatus zum angegebenen Zeitpunkt ausgeführt werden muss. Einzelne Ziffern geben die Wochentage an. 0 steht für Sonntag, 1 ist Montag, und so weiter. Der Standardtag für die Sicherung ist Sonntag.|DWORD: Tage der Woche zum Ausführen der Sicherung (dezimal), z.B. plant 1230 Sicherungen am Montag, Dienstag, Mittwoch und Sonntag.|
  |SSBRetentionDays|Wird verwendet, um die Anzahl der Tage für das Beibehalten einer Sicherung zu konfigurieren. Der Standardwert ist 60. Der größte zulässige Wert ist 180.|DWORD: Tage zum Beibehalten einer Sicherung (dezimal).|

3. Verwenden Sie den folgenden Befehl, um das Sicherungsmodul neu zu starten.
    ```
    PS C:\> Net start obengine
    ```

4. Öffnen Sie den Microsoft Recovery Services-Agenten.

5. Klicken Sie auf **Sicherung planen**, und klicken Sie anschließend auf **Weiter**, bis die übernommenen Änderungen angezeigt werden.

6. Klicken Sie auf **Beenden**, um die Änderungen zu übernehmen.


## <a name="questions"></a>Fragen?
Wenn Sie Fragen haben oder Anregungen zu gewünschten Funktionen mitteilen möchten, [senden Sie uns Ihr Feedback](http://aka.ms/azurebackup_feedback).

## <a name="next-steps"></a>Nächste Schritte
* Informieren Sie sich ausführlicher über das [Sichern von Windows-Computern](backup-configure-vault.md).
* Nachdem Sie nun Ihre Dateien und Ordner gesichert haben, können Sie [Ihre Tresore und Server verwalten](backup-azure-manage-windows-server.md).
* Informationen zum Wiederherstellen einer Sicherung finden Sie im Artikel zum [Wiederherstellen von Dateien auf einem Windows-Computer](backup-azure-restore-windows-server.md).

