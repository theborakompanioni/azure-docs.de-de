---
title: Wiederherstellen von Daten in Azure auf einem Windows Server- oder Windows-Computer | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie in Azure gespeicherte Daten auf einem Windows Server- oder Windows-Computer wiederherstellen.
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shivamg
editor: 
ms.assetid: 742f4b9e-c0ab-4eeb-8e22-ee29b83c22c4
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/16/2017
ms.author: saurse;trinadhk;markgal;
ms.translationtype: HT
ms.sourcegitcommit: 540180e7d6cd02dfa1f3cac8ccd343e965ded91b
ms.openlocfilehash: 231dd61f95267b3a504ed70e9b3a5abc470b69b2
ms.contentlocale: de-de
ms.lasthandoff: 08/16/2017

---
# <a name="restore-files-to-a-windows-server-or-windows-client-machine-using-resource-manager-deployment-model"></a>Wiederherstellen von Dateien auf einem Windows-Server- oder Windows-Clientcomputer mit dem Resource Manager-Bereitstellungsmodell
> [!div class="op_single_selector"]
> * [Azure-Portal](backup-azure-restore-windows-server.md)
> * [Klassisches Portal](backup-azure-restore-windows-server-classic.md)
>
>

In diesem Artikel wird das Wiederherstellen von Daten aus einem Sicherungstresor erläutert. Zum Wiederherstellen von Daten verwenden Sie den Assistenten zum Wiederherstellen von Daten im Microsoft Azure Recovery Services-Agent (MARS). Bei der Datenwiederherstellung ist Folgendes möglich:

* Wiederherstellen von Daten auf dem gleichen Computer, auf dem die Sicherungen erstellt wurden
* Wiederherstellen auf einem anderen Computer.

Im Januar 2017 hat Microsoft ein Vorschau-Update für den MARS-Agent veröffentlicht. Dieses Update bietet Fehlerbehebungen sowie eine sofortige Wiederherstellung, wodurch Sie eine Wiederherstellungspunkt-Momentaufnahme mit Schreibzugriff als Wiederherstellungsvolume bereitstellen können. Sie können dann das Wiederherstellungsvolume durchsuchen und Dateien auf einen lokalen Computer kopieren, wodurch Sie Dateien selektiv wiederherstellen können.

> [!NOTE]
> Das [Azure Backup-Update vom Januar 2017](https://support.microsoft.com/en-us/help/3216528?preview) ist erforderlich, wenn Sie die sofortige Wiederherstellung verwenden möchten, um Daten wiederherzustellen. Außerdem müssen die Sicherungsdaten in Tresoren an einem der Orte geschützt sein, die im Supportartikel aufgeführt sind. Unter [Azure Backup-Update vom Januar 2017](https://support.microsoft.com/en-us/help/3216528?preview) finden Sie die neueste Liste der Regionen, in denen die sofortige Wiederherstellung unterstützt wird. Die sofortige Wiederherstellung ist derzeit **nicht** in allen Regionen verfügbar.
>

Die sofortige Wiederherstellung ist für die Nutzung in Recovery Services-Tresoren im Azure-Portal und in Sicherungstresoren im klassischen Portal verfügbar. Wenn Sie die sofortige Wiederherstellung einsetzen möchten, laden Sie das MARS-Update herunter und befolgen Sie die Verfahren, in denen die sofortige Wiederherstellung erwähnt wird.

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="use-instant-restore-to-recover-data-to-the-same-machine"></a>Verwenden der sofortigen Wiederherstellung zum Wiederherstellen von Daten auf demselben Computer

Wenn Sie versehentlich eine Datei gelöscht haben und sie auf demselben Computer wiederherstellen möchten (auf dem die Sicherung erstellt wurde), können Sie die folgenden Schritte zum Wiederherstellen der Daten ausführen.

1. Öffnen Sie das Snap-In **Microsoft Azure Backup** . Falls Sie nicht wissen, wo das Snap-In installiert wurde, können Sie den für **Microsoft Azure Backup** verwendeten Computer oder Server durchsuchen.

    Die Desktop-App sollten in den Suchergebnissen angezeigt werden.

2. Klicken Sie auf **Daten wiederherstellen**, um den Assistenten zu starten.

    ![Daten wiederherstellen](./media/backup-azure-restore-windows-server/recover.png)

3. Wählen Sie im Bereich **Erste Schritte** die Option **Dieser Server (`<server name>`)**, um die Daten auf demselben Server oder Computer wiederherzustellen, und klicken Sie auf **Weiter**.

    ![Auswählen der Option „Dieser Server“ zum Wiederherstellen der Daten auf demselben Computer](./media/backup-azure-restore-windows-server/samemachine_gettingstarted_instantrestore.png)

4. Wählen Sie im Bereich **Wiederherstellungsmodus auswählen** die Option **Einzelne Dateien und Ordner**, und klicken Sie auf **Weiter**.

    ![Dateien durchsuchen](./media/backup-azure-restore-windows-server/samemachine_selectrecoverymode_instantrestore.png)

5. Wählen Sie im Bereich **Volume und Datum auswählen** das Volume aus, dass die wiederherzustellenden Dateien bzw. Ordner enthält.

    Wählen Sie im Kalender einen Wiederherstellungspunkt aus. Sie können den Zustand eines beliebigen Wiederherstellungszeitpunkts wiederherstellen. Wenn Datumsangaben in **Fettdruck** angezeigt werden, ist dies ein Hinweis darauf, dass mindestens ein Wiederherstellungszeitpunkt verfügbar ist. Falls mehrere Wiederherstellungspunkte verfügbar sind, müssen Sie nach dem Auswählen eines Datums im Dropdownmenü **Uhrzeit** den jeweiligen Wiederherstellungspunkt auswählen.

    ![Volume und Datum](./media/backup-azure-restore-windows-server/samemachine_selectvolumedate_instantrestore.png)

6. Klicken Sie auf **Bereitstellen**, nachdem Sie den Wiederherstellungspunkt ausgewählt haben, der wiederhergestellt werden soll.

    Azure Backup stellt den lokalen Wiederherstellungspunkt bereit und verwendet ihn als Wiederherstellungsvolume.

7. Klicken Sie im Bereich **Dateien suchen und wiederherstellen** auf **Durchsuchen**, um Windows-Explorer zu öffnen und nach den gewünschten Dateien und Ordnern zu suchen.

    ![Wiederherstellungsoptionen](./media/backup-azure-restore-windows-server/samemachine_browserecover_instantrestore.png)


8. Kopieren Sie in Windows-Explorer die wiederherzustellenden Dateien bzw. Ordner, und fügen Sie sie an einem lokalen Speicherort auf dem Server oder Computer ein. Sie können die Dateien direkt vom Wiederherstellungsvolume öffnen oder streamen und überprüfen, ob die richtigen Versionen wiederhergestellt werden.

    ![Kopieren und Einfügen von Dateien und Ordnern vom bereitgestellten Volume am lokalen Speicherort](./media/backup-azure-restore-windows-server/samemachine_copy_instantrestore.png)

9. Klicken Sie im Bereich **Dateien suchen und wiederherstellen** auf **Bereitstellung aufheben**, wenn Sie die Wiederherstellung der Dateien bzw. Ordner abgeschlossen haben. Klicken Sie anschließend auf **Ja**, um zu bestätigen, dass Sie die Bereitstellung des Volumes aufheben möchten.

    ![Aufheben der Bereitstellung des Volumes und Bestätigen des Vorgangs](./media/backup-azure-restore-windows-server/samemachine_unmount_instantrestore.png)

    > [!Important]
    > Wenn Sie nicht auf „Bereitstellung aufheben“ klicken, bleibt die Bereitstellung des Wiederherstellungsvolumes ab dem Bereitstellungszeitpunkt sechs Stunden lang bestehen. Im Falle eines laufenden Dateikopiervorgangs wird die Bereitstellungszeit jedoch auf bis zu 24 Stunden verlängert. Während der Bereitstellung des Volumes werden keine Sicherungsvorgänge durchgeführt. Alle Sicherungsvorgänge, deren Ausführung für den Zeitraum der Volumebereitstellung geplant ist, werden ausgeführt, nachdem die Bereitstellung des Wiederherstellungsvolumes aufgehoben wurde.
    >


## <a name="use-instant-restore-to-restore-data-to-an-alternate-machine"></a>Verwenden der sofortigen Wiederherstellung zum Wiederherstellen von Daten auf einem anderen Computer
Wenn der gesamte Server verloren geht, können Sie Daten aus Azure Backup dennoch auf einem anderen Computer wiederherstellen. Der Workflow wird in den folgenden Schritten beschrieben.


In diesen Schritten wird folgende Terminologie verwendet:

* *Quellcomputer* – Der ursprüngliche Computer, von dem die Sicherung erstellt wurde und der zurzeit nicht verfügbar ist.
* *Zielcomputer* – Der Computer, auf dem die Daten wiederhergestellt werden.
* *Beispieltresor* – Der Recovery Services-Tresor, bei dem der *Quellcomputer* und der *Zielcomputer* registriert sind. <br/>

> [!NOTE]
> Sicherungen können nicht auf einem Zielcomputer wiederhergestellt werden, auf dem eine ältere Version des Betriebssystems ausgeführt wird. Beispielsweise kann eine Sicherung, die von einem Windows 7-Computer erstellt wurde, auf einem Windows 8-Computer (oder höher) wiederhergestellt werden. Es ist aber nicht möglich, eine Sicherung, die von einem Windows 8-Computer erstellt wurde, auf einem Windows 7-Computer wiederherzustellen.
>
>

1. Öffnen Sie das Snap-In **Microsoft Azure Backup** auf dem *Zielcomputer*.

2. Stellen Sie sicher, dass der *Zielcomputer* und der *Quellcomputer* bei demselben Recovery Services-Tresor registriert sind.

3. Klicken Sie auf **Daten wiederherstellen**, um den **Assistenten zum Wiederherstellen von Daten** zu öffnen.

    ![Daten wiederherstellen](./media/backup-azure-restore-windows-server/recover.png)

4. Wählen Sie im Bereich **Erste Schritte** die Option **Anderer Server**.

    ![Anderer Server](./media/backup-azure-restore-windows-server/alternatemachine_gettingstarted_instantrestore.png)

5. Geben Sie die Datei mit den Tresoranmeldeinformationen an, die dem *Beispieltresor* entspricht, und klicken Sie auf **Weiter**.

    Wenn die Datei mit den Tresoranmeldeinformationen ungültig (oder abgelaufen) ist, laden Sie eine neue Anmeldeinformationsdatei für den Tresor aus dem *Beispieltresor* im Azure-Portal herunter. Nachdem Sie gültige Tresoranmeldeinformationen angegeben haben, wird der Name des entsprechenden Sicherungstresors angezeigt.


6. Wählen Sie im Bereich **Sicherungsserver auswählen** in der Liste mit den angezeigten Computern den *Quellcomputer* aus, und geben Sie die Passphrase an. Klicken Sie auf **Weiter**.

    ![Liste der Computer](./media/backup-azure-restore-windows-server/alternatemachine_selectmachine_instantrestore.png)

7. Wählen Sie im Bereich **Wiederherstellungsmodus auswählen** die Option **Einzelne Dateien und Ordner**, und klicken Sie auf **Weiter**.

    ![Suche](./media/backup-azure-restore-windows-server/alternatemachine_selectrecoverymode_instantrestore.png)

8. Wählen Sie im Bereich **Volume und Datum auswählen** das Volume aus, dass die wiederherzustellenden Dateien bzw. Ordner enthält.

    Wählen Sie im Kalender einen Wiederherstellungspunkt aus. Sie können den Zustand eines beliebigen Wiederherstellungszeitpunkts wiederherstellen. Wenn Datumsangaben in **Fettdruck** angezeigt werden, ist dies ein Hinweis darauf, dass mindestens ein Wiederherstellungszeitpunkt verfügbar ist. Falls mehrere Wiederherstellungspunkte verfügbar sind, müssen Sie nach dem Auswählen eines Datums im Dropdownmenü **Uhrzeit** den jeweiligen Wiederherstellungspunkt auswählen.

    ![Elemente suchen](./media/backup-azure-restore-windows-server/alternatemachine_selectvolumedate_instantrestore.png)

9. Klicken Sie auf **Bereitstellen**, um den Wiederherstellungspunkt lokal als Wiederherstellungsvolume auf Ihrem *Zielcomputer* bereitzustellen.

10. Klicken Sie im Bereich **Dateien suchen und wiederherstellen** auf **Durchsuchen**, um Windows-Explorer zu öffnen und nach den gewünschten Dateien und Ordnern zu suchen.

    ![Verschlüsselung](./media/backup-azure-restore-windows-server/alternatemachine_browserecover_instantrestore.png)

11. Kopieren Sie in Windows-Explorer die Dateien bzw. Ordner vom Wiederherstellungsvolume, und fügen Sie sie am Speicherort auf Ihrem *Zielcomputer* ein. Sie können die Dateien direkt vom Wiederherstellungsvolume öffnen oder streamen und überprüfen, ob die richtigen Versionen wiederhergestellt werden.

    ![Verschlüsselung](./media/backup-azure-restore-windows-server/alternatemachine_copy_instantrestore.png)

12. Klicken Sie im Bereich **Dateien suchen und wiederherstellen** auf **Bereitstellung aufheben**, wenn Sie die Wiederherstellung der Dateien bzw. Ordner abgeschlossen haben. Klicken Sie anschließend auf **Ja**, um zu bestätigen, dass Sie die Bereitstellung des Volumes aufheben möchten.

    ![Verschlüsselung](./media/backup-azure-restore-windows-server/alternatemachine_unmount_instantrestore.png)

    > [!Important]
    > Wenn Sie nicht auf „Bereitstellung aufheben“ klicken, bleibt die Bereitstellung des Wiederherstellungsvolumes ab dem Bereitstellungszeitpunkt sechs Stunden lang bestehen. Im Falle eines laufenden Dateikopiervorgangs wird die Bereitstellungszeit jedoch auf bis zu 24 Stunden verlängert. Während der Bereitstellung des Volumes werden keine Sicherungsvorgänge durchgeführt. Alle Sicherungsvorgänge, deren Ausführung für den Zeitraum der Volumebereitstellung geplant ist, werden ausgeführt, nachdem die Bereitstellung des Wiederherstellungsvolumes aufgehoben wurde.
    >

## <a name="troubleshooting"></a>Problembehandlung
Falls Azure Backup das Wiederherstellungsvolume auch mehrere Minuten nach dem Klicken auf **Einbinden** nicht erfolgreich eingebunden hat oder beim Einbinden des Wiederherstellungsvolumes Fehler auftreten, führen Sie die folgenden Schritte für eine normale Wiederherstellung aus.

1.  Brechen Sie den laufenden Einbindungsvorgang ab, falls dieser bereits mehrere Minuten lang ausgeführt wird.

2.  Vergewissern Sie sich, dass Sie über die neueste Version des Azure Backup-Agents verfügen. Klicken Sie zum Ermitteln der Versionsinformationen für den Azure Backup-Agent im Bereich **Aktionen** der Microsoft Azure Backup-Konsole auf **About Microsoft Azure Recovery Services Agent** (Informationen zum Microsoft Azure Recovery Services-Agent), und vergewissern Sie sich, dass die **Versionsummer** mindestens der in [diesem Artikel](https://go.microsoft.com/fwlink/?linkid=229525) angegebenen Version entspricht. Sie können die neueste Version [hier](https://go.microsoft.com/fwLink/?LinkID=288905) herunterladen.

3.  Wechseln Sie zu **Geräte-Manager** -> **Speichercontroller**, und vergewissern Sie sich, dass **Microsoft iSCSI-Initiator** vorhanden ist. Ist dies der Fall, fahren Sie direkt mit Schritt 7 fort. 

4.  Ist der in Schritt 3 erwähnte Microsoft iSCSI-Initiator-Dienst nicht vorhanden, suchen Sie unter **Geräte-Manager** -> **Speichercontroller** nach einem Eintrag vom Typ **Unbekanntes Gerät** mit der Hardware-ID **ROOT\ISCSIPRT**.

5.  Klicken Sie mit der rechten Maustaste auf **Unbekanntes Gerät**, und wählen Sie **Treibersoftware aktualisieren** aus.

6.  Aktualisieren Sie den Treiber durch Auswählen der Option **Automatisch nach aktueller Treibersoftware suchen**. Nach Abschluss des Updates sollte **Unbekanntes Gerät** durch **Microsoft iSCSI-Initiator** ersetzt werden, wie im Anschluss dargestellt. 

    ![Verschlüsselung](./media/backup-azure-restore-windows-server/UnknowniSCSIDevice.png)

7.  Wechseln Sie zu **Task-Manager** -> **Dienste (lokal)** -> **Microsoft iSCSI-Initiator-Dienst**. 

    ![Verschlüsselung](./media/backup-azure-restore-windows-server/MicrosoftInitiatorServiceRunning.png)
    
8.  Starten Sie den Microsoft iSCSI-Initiator-Dienst neu, indem Sie mit der rechten Maustaste auf den Dienst und anschließend auf **Beenden** klicken. Klicken Sie dann erneut mit der rechten Maustaste auf den Dienst und anschließend auf **Starten**.

9.  Wiederholen Sie die Wiederherstellung mit der sofortigen Wiederherstellung. 

Ist die Wiederherstellung weiterhin nicht erfolgreich, starten Sie Ihren Server/Client neu. Sollte ein Neustart nicht wünschenswert oder die Wiederherstellung auch nach einem Neustart des Servers nicht erfolgreich sein, versuchen Sie, die Bereitstellung über einen anderen Computer durchzuführen, und wenden Sie sich an den Azure-Support, indem Sie über das [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) eine Supportanfrage senden.

## <a name="next-steps"></a>Nächste Schritte
* Nachdem Sie nun Ihre Dateien und Ordner wiederhergestellt haben, können Sie [Ihre Sicherungen verwalten](backup-azure-manage-windows-server.md).

