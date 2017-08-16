---
title: Verwenden des Azure Backup-Agents zum Sichern von Dateien und Ordnern | Microsoft-Dokumentation
description: "Verwenden Sie den Microsoft Azure Backup-Agent, um Windows-Dateien und -Ordner in Azure zu sichern. Erstellen Sie einen Recovery Services-Tresor, installieren Sie den Backup-Agent, definieren Sie die Sicherungsrichtlinie, und führen Sie die erste Sicherung der Dateien und Ordner aus."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: Sicherungstresor; Windows-Server sichern; Windows sichern;
ms.assetid: 7f5b1943-b3c1-4ddb-8fb7-3560533c68d5
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/15/2017
ms.author: markgal;trinadhk;
ms.translationtype: Human Translation
ms.sourcegitcommit: b1d56fcfb472e5eae9d2f01a820f72f8eab9ef08
ms.openlocfilehash: aa6b75ef1786021b56a59ad1bea2d45eb74ad25f
ms.contentlocale: de-de
ms.lasthandoff: 07/06/2017

---
# <a name="back-up-a-windows-server-or-client-to-azure-using-the-resource-manager-deployment-model"></a>Sichern eines Windows-Servers oder -Clients in Azure unter Verwendung des Resource Manager-Bereitstellungsmodells

> [!div class="op_single_selector"]
> * [Azure-Portal](backup-configure-vault.md)
> * [Klassisches Portal](backup-configure-vault-classic.md)
>
>

In diesem Artikel erfahren Sie, wie Sie Dateien und Ordner eines Windows-Servers (oder Windows-Clients) mithilfe von Azure Backup und des Resource Manager-Bereitstellungsmodells in Azure sichern.

[!INCLUDE [learn-about-deployment-models](../../includes/backup-deployment-models.md)]

![Sicherungsprozessschritte](./media/backup-configure-vault/initial-backup-process.png)

## <a name="before-you-start"></a>Vorbereitung
Um einen Server oder Client in Azure zu sichern, benötigen Sie ein Azure-Konto. Falls Sie nicht über ein Konto verfügen, können Sie in nur wenigen Minuten ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen.

## <a name="create-a-recovery-services-vault"></a>Erstellen eines Recovery Services-Tresors
Bei einem Recovery Services-Tresor handelt es sich um eine Entität, in der alle Sicherungen und Wiederherstellungspunkte gespeichert werden, die Sie im Laufe der Zeit erstellen. Der Recovery Services-Tresor enthält auch die Sicherungsrichtlinie, die auf die geschützten Dateien und Ordner angewendet wird. Wenn Sie einen Recovery Services-Tresor erstellen, sollten Sie auch die entsprechende Speicherredundanzoption auswählen.

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

    * Wählen Sie **Neu erstellen** aus, wenn Sie eine neue Ressourcengruppe erstellen möchten.
    Oder
    * Wählen Sie **Use existing** (Vorhandene verwenden) aus, und klicken Sie auf das Dropdownmenü, um eine Liste mit verfügbaren Ressourcengruppen anzuzeigen.

  Umfassende Informationen zu Ressourcengruppen finden Sie in der [Übersicht über Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).

7. Klicken Sie auf **Standort** , um die geografische Region für den Tresor auszuwählen. Die Auswahl bestimmt die geografische Region, an die Ihre Sicherungsdaten gesendet werden.

8. Klicken Sie im unteren Bereich des Blatts für den Recovery Services-Tresor auf **Erstellen**.

  Die Erstellung des Recovery Services-Tresors kann mehrere Minuten dauern. Verfolgen Sie die Statusbenachrichtigungen rechts oben im Portal. Nach Abschluss des Erstellungsvorgangs wird der Tresor in der Liste mit den Recovery Services-Tresoren angezeigt. Sollte der Tresor auch nach mehreren Minuten nicht angezeigt werden, klicken Sie auf **Aktualisieren**.

  ![Klicken auf die Schaltfläche „Aktualisieren“](./media/backup-try-azure-backup-in-10-mins/refresh-button.png)</br>

  Sobald Ihr Tresor in der Liste mit den Recovery Services-Tresoren angezeigt wird, können Sie die Speicherredundanz festlegen.


### <a name="set-storage-redundancy"></a>Festlegen der Speicherredundanz
Bei der erstmaligen Erstellung eines Recovery Services-Tresors bestimmen Sie, wie der Speicher repliziert werden soll.

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

  Standardmäßig verfügt Ihr Tresor über einen georedundanten Speicher. Wenn Sie Azure als primären Speicherendpunkt für die Sicherung verwenden, verwenden Sie weiterhin die Option **Georedundant**. Wenn Sie Azure nicht als primären Speicherendpunkt für die Sicherung verwenden, wählen Sie **Lokal redundant** aus. Dadurch verringern sich die Kosten für Azure-Speicher. Weitere Informationen zu den Optionen für [georedundanten](../storage/storage-redundancy.md#geo-redundant-storage) und [lokal redundanten](../storage/storage-redundancy.md#locally-redundant-storage) Speicher finden Sie in [dieser Übersicht über Speicherredundanz](../storage/storage-redundancy.md).

Nachdem Sie nun über einen Tresor verfügen, bereiten Sie Ihre Infrastruktur auf die Sicherung von Dateien und Ordnern vor, indem Sie den Microsoft Azure Recovery Services-Agent herunterladen und installieren, Tresoranmeldeinformationen herunterladen und dann den Agent mit diesen Anmeldeinformationen beim Tresor registrieren.

## <a name="configure-the-vault"></a>Konfigurieren des Tresors

1. Klicken Sie auf dem Blatt „Recovery Services-Tresor“ (für den gerade erstellten Tresor) im Abschnitt „Erste Schritte“ auf **Backup**. Wählen Sie dann auf dem Blatt **Erste Schritte mit der Sicherung** die Option **Sicherungsziel** aus.

  ![Öffnen des Blatts „Backup Goal“ (Sicherungsziel)](./media/backup-try-azure-backup-in-10-mins/open-backup-settings.png)

  Das Blatt **Sicherungsziel** wird geöffnet. Wenn der Recovery Services-Tresor bereits zuvor konfiguriert wurde, wird das Blatt **Sicherungsziel** geöffnet, nachdem Sie auf dem Blatt des Recovery Services-Tresors auf **Sicherung** geklickt haben.

  ![Öffnen des Blatts „Backup Goal“ (Sicherungsziel)](./media/backup-try-azure-backup-in-10-mins/backup-goal-blade.png)

2. Wählen Sie im Dropdownmenü **Wo wird Ihre Workload ausgeführt?** die Option **Lokal** aus.

  Sie wählen **Lokal**, da Ihr Windows Server- oder Windows-Computer ein physischer Computer ist, der sich nicht in Azure befindet.

3. Wählen Sie im Menü **Was möchten Sie sichern?** die Option **Dateien und Ordner** aus, und klicken Sie anschließend auf **OK**.

  ![Konfigurieren von Dateien und Ordnern](./media/backup-try-azure-backup-in-10-mins/set-file-folder.png)

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
> Die Sicherung kann noch nicht über das Azure-Portal aktiviert werden. Verwenden Sie den Microsoft Azure Recovery Services-Agent, um Ihre Dateien und Ordner zu sichern.
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

## <a name="network-and-connectivity-requirements"></a>Netzwerk- und Konnektivitätsanforderungen

Wenn Ihr Computer/Proxy nur eingeschränkten Zugang zum Internet hat, stellen Sie sicher, dass die Firewall-Einstellungen auf dem Computer/Proxy für die folgenden URLs konfiguriert sind: <br>
    1. www.msftncsi.com
    2. *.Microsoft.com
    3. *.WindowsAzure.com
    4. *.microsoftonline.com
    5. *.windows.ne


## <a name="create-the-backup-policy"></a>Erstellen der Sicherungsrichtlinie
Die Sicherungsrichtlinie besteht aus dem Zeitplan für die Erfassung von Wiederherstellungspunkten und dem Zeitraum, für den die Wiederherstellungspunkte gespeichert werden. Verwenden Sie den Microsoft Azure Backup-Agent, um die Sicherungsrichtlinie für Dateien und Ordner zu erstellen.

### <a name="to-create-a-backup-schedule"></a>So erstellen Sie einen Sicherungszeitplan
1. Öffnen Sie den Microsoft Azure Backup-Agent. Den Agent finden Sie, indem Sie auf Ihrem Computer nach **Microsoft Azure Backup**suchen.

    ![Starten des Azure Backup-Agents](./media/backup-configure-vault/snap-in-search.png)
2. Klicken Sie im Bereich **Aktionen** des Backup-Agents auf **Sicherung planen**, um den Assistenten zum Planen von Sicherungen zu starten.

    ![Planen einer Windows Server-Sicherung](./media/backup-configure-vault/schedule-first-backup.png)

3. Klicken Sie im Assistenten zum Planen von Sicherungen auf der Seite **Erste Schritte** auf **Weiter**.
4. Klicken Sie auf der Seite **Elemente für Sicherung auswählen** auf **Elemente hinzufügen**.

  Das Dialogfeld „Elemente auswählen“ wird geöffnet.

5. Wählen Sie die Dateien und Ordner aus, die Sie sichern möchten, und klicken Sie auf **OK**.
6. Klicken Sie auf der Seite **Elemente für Sicherung auswählen** auf **Weiter**.
7. Geben Sie auf der Seite **Sicherungszeitplan angeben** den Sicherungszeitplan an, und klicken Sie auf **Weiter**.

    Sie können tägliche (maximal drei pro Tag) oder wöchentliche Sicherungen planen.

    ![Elemente einer Windows Server-Sicherung](./media/backup-configure-vault/specify-backup-schedule-close.png)

   > [!NOTE]
   > Weitere Informationen zum Angeben des Sicherungszeitplans finden Sie im Artikel [Verwenden von Azure Backup als Ersatz für Ihre Bandinfrastruktur](backup-azure-backup-cloud-as-tape.md).
   >
   >

8. Wählen Sie auf der Seite **Aufbewahrungsrichtlinie auswählen** die gewünschten Aufbewahrungsrichtlinien für die Sicherungskopie, und klicken Sie auf **Weiter**.

    Die Aufbewahrungsrichtlinie gibt an, wie lange die Sicherung gespeichert wird. Statt lediglich eine einfache Richtlinie für alle Sicherungspunkte anzugeben, können Sie je nach Sicherungszeitpunkt verschiedene Aufbewahrungsrichtlinien festlegen. Sie können die tägliche, wöchentliche, monatliche und jährliche Aufbewahrungsrichtlinie an Ihre Anforderungen anpassen.
9. Wählen Sie auf der Seite „Erstsicherungstyp wählen“ den Typ für die erste Sicherung. Lassen Sie die Option **Automatisch über das Netzwerk** aktiviert, und klicken Sie auf **Weiter**.

    Sie können die Sicherung automatisch über das Netzwerk oder offline durchführen. In den verbleibenden Abschnitten dieses Artikels wird das Verfahren für eine automatische Sicherung beschrieben. Falls Sie vorziehen, eine Offlinesicherung durchzuführen, finden Sie unter [Workflow zur Offlinesicherung in Azure Backup](backup-azure-backup-import-export.md) weitere Informationen.
10. Lesen Sie sich die Informationen auf der Seite „Bestätigung“ durch, und klicken Sie dann auf **Fertig stellen**.
11. Klicken Sie auf **Schließen**, nachdem der Assistent die Erstellung des Sicherungszeitplans abgeschlossen hat.

### <a name="enable-network-throttling"></a>Aktivieren der Netzwerkdrosselung
Der Microsoft Azure Backup-Agent ermöglicht eine Netzwerkdrosselung. Über die Drosselung wird die Verwendung der Netzwerkbandbreite bei der Datenübertragung gesteuert. Diese Steuerungsmöglichkeit kann hilfreich sein, wenn Sie Daten während der Geschäftszeiten sichern möchten, der Sicherungsprozess aber keine Auswirkung auf den weiteren Internetdatenverkehr haben soll. Die Drosselung gilt für Sicherungs- und Wiederherstellungsaktivitäten.

> [!NOTE]
> Die Netzwerkdrosselung ist nicht verfügbar für Windows Server 2008 R2 SP1, Windows Server 2008 SP2 oder Windows 7 (mit Servicepacks). Das Azure Backup-Netzwerkdrosselungs-Feature beauftragt Quality of Service (QoS) auf dem lokalen Betriebssystem. Obwohl Azure Backup diese Betriebssysteme schützen kann, funktioniert die auf diesen Plattformen verfügbare Version von QoS mit der Azure Backup-Netzwerkdrosselung nicht. Die Netzwerkdrosselung kann auf allen anderen [unterstützten Betriebssystemen](backup-azure-backup-faq.md) verwendet werden.
>
>

**So aktivieren Sie die Netzwerkdrosselung**

1. Klicken Sie im Microsoft Azure Backup-Agent auf **Eigenschaften ändern**.

    ![Eigenschaften ändern](./media/backup-configure-vault/change-properties.png)
2. Aktivieren Sie auf der Registerkarte **Drosselung** das Kontrollkästchen **Internet-Bandbreiteneinschränkung für Sicherungsvorgänge aktivieren**.

    ![Netzwerkdrosselung](./media/backup-configure-vault/throttling-dialog.png)
3. Nachdem Sie die Drosselung aktiviert haben, geben Sie die zulässige Bandbreite für die Backupdatenübertragung für **Arbeitsstunden** und **Arbeitsfreie Stunden** an.

    Die Bandbreitenwerte beginnen bei 512 Kilobits pro Sekunde (KBit/s) und können mit bis zu 1.023 Megabits pro Sekunde (MBit/s) angegeben werden. Sie können außerdem den Start und das Ende für die **Arbeitsstunden** angeben und festlegen, welche Tage der Woche als Arbeitstage betrachtet werden. Stunden außerhalb der festgelegten Arbeitsstunden gelten arbeitsfreie Stunden.
4. Klicken Sie auf **OK**.

### <a name="to-back-up-files-and-folders-for-the-first-time"></a>So führen Sie erstmals eine Sicherung von Dateien und Ordnern durch
1. Klicken Sie im Backup-Agent auf **Jetzt sichern**, um das erste Seeding über das Netzwerk abzuschließen.

    ![Windows Server jetzt sichern](./media/backup-configure-vault/backup-now.png)
2. Überprüfen Sie auf der Seite „Bestätigung“ die Einstellungen, die vom Assistenten für die sofortige Sicherung zum Sichern des Computers verwendet werden. Klicken Sie dann auf **Sichern**.
3. Klicken Sie auf **Schließen** , um den Assistenten zu schließen. Wenn Sie auf „Schließen“ klicken, bevor der Sicherungsvorgang abgeschlossen ist, wird der Assistent im Hintergrund weiter ausgeführt.

Nach Abschluss des ersten Backups wird der Status des Auftrags in der Backup-Konsole als **Auftrag abgeschlossen** angezeigt.

![Abgeschlossen](./media/backup-configure-vault/ircomplete.png)

## <a name="questions"></a>Fragen?
Wenn Sie Fragen haben oder Anregungen zu gewünschten Funktionen mitteilen möchten, [senden Sie uns Ihr Feedback](http://aka.ms/azurebackup_feedback).

## <a name="next-steps"></a>Nächste Schritte
Zusätzliche Informationen zum Sichern von virtuellen Computern oder anderen Workloads finden Sie hier:

* Nachdem Sie nun Ihre Dateien und Ordner gesichert haben, können Sie [Ihre Tresore und Server verwalten](backup-azure-manage-windows-server.md).
* Informationen zum Wiederherstellen einer Sicherung finden Sie im Artikel zum [Wiederherstellen von Dateien auf einem Windows-Computer](backup-azure-restore-windows-server.md).

