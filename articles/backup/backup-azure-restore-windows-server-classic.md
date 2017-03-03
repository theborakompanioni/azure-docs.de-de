---
title: Wiederherstellen von Daten auf einem Windows-Server oder -Client von Azure unter Verwendung des klassischen Bereitstellungsmodells | Microsoft Docs
description: "Erfahren Sie, wie Daten auf einem Windows Server- oder Windows-Clientcomputer wiederhergestellt werden."
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shivamg
editor: 
ms.assetid: 85585dfc-c764-4e8c-8f0e-40b969640ac2
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 1/31/2017
ms.author: saurse;trinadhk;markgal;
translationtype: Human Translation
ms.sourcegitcommit: be6bc58ab856309004904626db166331b29199a8
ms.openlocfilehash: aa8ccc15971bed76d7ce8fd554e6a0f89d985fb8
ms.lasthandoff: 02/02/2017


---
# <a name="restore-files-to-a-windows-server-or-windows-client-machine-using-the-classic-deployment-model"></a>Wiederherstellen von Dateien auf einem Windows-Server- oder Windows-Clientcomputer mit dem klassischen Bereitstellungsmodell
> [!div class="op_single_selector"]
> * [Klassisches Portal](backup-azure-restore-windows-server-classic.md)
> * [Azure-Portal](backup-azure-restore-windows-server.md)
>
>

In diesem Artikel wird das Wiederherstellen von Daten aus einem Sicherungstresor erläutert. Zum Wiederherstellen von Daten verwenden Sie den Assistenten für die Wiederherstellung von Daten im Microsoft Azure Recovery Services-Agent (MARS). Bei der Datenwiederherstellung ist Folgendes möglich:

* Wiederherstellen von Daten auf dem gleichen Computer, auf dem die Sicherungen erstellt wurden
* Wiederherstellen auf einem anderen Computer.

Im Januar 2017 hat Microsoft ein Preview-Update für den MARS-Agent veröffentlicht. Dieses Update bietet Fehlerbehebungen sowie eine sofortige Wiederherstellung, wodurch Sie eine Wiederherstellungspunkt-Momentaufnahme mit Schreibzugriff als Wiederherstellungsvolume bereitstellen können. Sie können dann das Wiederherstellungsvolume durchsuchen und Dateien auf einen lokalen Computer kopieren, wodurch Sie Dateien selektiv wiederherstellen können.

> [!NOTE]
> Das [Azure Backup-Update vom Januar 2017](https://support.microsoft.com/en-us/help/3216528?preview) ist erforderlich, wenn Sie die sofortige Wiederherstellung verwenden möchten, um Daten wiederherzustellen. Außerdem müssen die Sicherungsdaten in Tresoren an einem der Orte geschützt sein, die im Supportartikel aufgeführt sind. Unter [Azure Backup-Update vom Januar 2017](https://support.microsoft.com/en-us/help/3216528?preview) finden Sie die neueste Liste der Regionen, in denen die sofortige Wiederherstellung unterstützt wird. Die sofortige Wiederherstellung ist derzeit **nicht** in allen Regionen verfügbar.
>

Die sofortige Wiederherstellung ist für die Nutzung in Recovery Services-Tresoren im Azure-Portal und in Sicherungstresoren im klassischen Portal verfügbar. Wenn Sie die sofortige Wiederherstellung einsetzen möchten, laden Sie das MARS-Update herunter und befolgen Sie die Verfahren, in denen die sofortige Wiederherstellung erwähnt wird.


## <a name="use-instant-restore-to-recover-data-to-the-same-machine"></a>Verwenden der sofortigen Wiederherstellung zum Wiederherstellen von Daten auf demselben Computer

Wenn Sie versehentlich eine Datei gelöscht haben und sie auf demselben Computer wiederherstellen möchten (auf dem die Sicherung erstellt wurde), können Sie die folgenden Schritte zum Wiederherstellen der Daten ausführen.

1. Öffnen Sie das Snap-In **Microsoft Azure Backup** . Falls Sie nicht wissen, wo das Snap-In installiert wurde, können Sie den für **Microsoft Azure Backup** verwendeten Computer oder Server durchsuchen.

    Die Desktop-App sollten in den Suchergebnissen angezeigt werden.

2. Klicken Sie auf **Daten wiederherstellen**, um den Assistenten zu starten.

    ![Wiederherstellen von Daten](./media/backup-azure-restore-windows-server/recover.png)

3. Wählen Sie im Bereich **Erste Schritte** die Option **Dieser Server (`<server name>`)**, um die Daten auf demselben Server oder Computer wiederherzustellen, und klicken Sie auf **Weiter**.

    ![Auswählen der Option „Dieser Server“ zum Wiederherstellen der Daten auf demselben Computer](./media/backup-azure-restore-windows-server/samemachine_gettingstarted_instantrestore.png)

4. Wählen Sie im Bereich **Wiederherstellungsmodus auswählen** die Option **Einzelne Dateien und Ordner**, und klicken Sie auf **Weiter**.

    ![Dateien durchsuchen](./media/backup-azure-restore-windows-server/samemachine_selectrecoverymode_instantrestore.png)

5. Wählen Sie im Bereich **Volume und Datum auswählen** das Volume aus, das die wiederherzustellenden Dateien bzw. Ordner enthält.

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
    > Wenn Sie nicht auf „Bereitstellung aufheben“ klicken, bleibt die Bereitstellung des Wiederherstellungsvolumes ab dem Bereitstellungszeitpunkt sechs Stunden lang erhalten. Während der Bereitstellung des Volumes werden keine Sicherungsvorgänge durchgeführt. Alle Sicherungsvorgänge, deren Ausführung für den Zeitraum der Volumebereitstellung geplant ist, werden ausgeführt, nachdem die Bereitstellung des Wiederherstellungsvolumes aufgehoben wurde.
    >


## <a name="recover-data-to-the-same-machine"></a>Wiederherstellen von Daten auf demselben Computer
Wenn Sie versehentlich eine Datei gelöscht haben und sie auf demselben Computer wiederherstellen möchten (auf dem die Sicherung erstellt wurde), können Sie die folgenden Schritte zum Wiederherstellen der Daten ausführen.

1. Öffnen Sie das Snap-In **Microsoft Azure Backup** .
2. Klicken Sie auf **Daten wiederherstellen** , um den Workflow zu initiieren.

    ![Daten wiederherstellen](./media/backup-azure-restore-windows-server-classic/recover.png)
3. Wählen Sie die Option **Dieser Server (*NameIhresComputers*)**, wenn Sie die gesicherte Datei auf demselben Computer wiederherstellen möchten.

    ![Auf demselben Computer](./media/backup-azure-restore-windows-server-classic/samemachine.png)
4. Sie können entscheiden, ob Sie **Dateien durchsuchen** oder **Dateien suchen** möchten.

    Behalten Sie die Standardoption bei, wenn Sie beabsichtigen, eine oder mehrere Dateien wiederherzustellen, deren Pfad bekannt ist. Wenn Sie die Ordnerstruktur nicht genau kennen und nach einer Datei suchen möchten, wählen Sie die Option **Dateien suchen** . In diesem Abschnitt verwenden wir die Standardoption.

    ![Dateien durchsuchen](./media/backup-azure-restore-windows-server-classic/browseandsearch.png)
5. Wählen Sie das Volume aus, von dem Sie die Datei wiederherstellen möchten.

    Sie können den Zustand eines beliebigen Zeitpunkts wiederherstellen. Datumsangaben, die im Kalendersteuerelement **fett** dargestellt werden, geben die Verfügbarkeit eines Wiederherstellungspunkts an. Nachdem Sie ein Datum ausgewählt haben, können Sie basierend auf Ihrem Sicherungszeitplan (und dem Erfolg eines Sicherungsvorgangs) einen Zeitpunkt in der Dropdownliste **Zeit** auswählen.

    ![Volume und Datum](./media/backup-azure-restore-windows-server-classic/volanddate.png)
6. Wählen Sie die Elemente, die wiederhergestellt werden sollen. Sie können eine Mehrfachauswahl von Ordnern/Dateien treffen, die Sie wiederherstellen möchten.

    ![Auswählen von Dateien](./media/backup-azure-restore-windows-server-classic/selectfiles.png)
7. Geben Sie die Wiederherstellungsparameter an.

    ![Wiederherstellungsoptionen](./media/backup-azure-restore-windows-server-classic/recoveroptions.png)

   * Sie können eine Wiederherstellung am ursprünglichen Speicherort (bei der die Datei oder der Ordner überschrieben wird) oder an einem anderen Speicherort auf demselben Computer durchführen.
   * Wenn die Datei oder der Ordner für die Wiederherstellung bereits am Zielspeicherort vorhanden ist, können Sie Kopien erstellen (zwei Versionen derselben Datei), die Dateien im Zielverzeichnis überschreiben oder die Wiederherstellung der Dateien überspringen, die am Zielspeicherort vorhanden sind.
   * Es wird dringend empfohlen, die Standardoption beizubehalten und die ACLs für die wiederherzustellenden Dateien ebenfalls wiederherzustellen.
8. Sobald Sie diese Eingaben bereitgestellt haben, klicken Sie auf **Weiter**. Der Wiederherstellungsworkflow, bei dem die Dateien auf diesem Computer wiederhergestellt werden, wird gestartet.

## <a name="recover-to-an-alternate-machine"></a>Wiederherstellen auf einem anderen Computer
Wenn der gesamte Server verloren geht, können Sie Daten aus Azure Backup dennoch auf einem anderen Computer wiederherstellen. Der Workflow wird in den folgenden Schritten beschrieben.  

In diesen Schritten wird folgende Terminologie verwendet:

* *Quellcomputer* – Der ursprüngliche Computer, von dem die Sicherung erstellt wurde und der zurzeit nicht verfügbar ist.
* *Zielcomputer* – Der Computer, auf dem die Daten wiederhergestellt werden.
* *Beispieltresor* – Der Sicherungstresor, bei dem der *Quellcomputer* und der *Zielcomputer* registriert sind. <br/>

> [!NOTE]
> Sicherungen von einem Computer können nicht auf einem Computer wiederhergestellt werden, auf dem eine frühere Version des Betriebssystems ausgeführt wird. Wenn beispielsweise Sicherungen auf einem Windows 7-Computer erstellt werden, können diese auf einem Computer unter Windows 8 oder höher wiederhergestellt werden. Umgekehrt ist dies jedoch nicht möglich.
>
>

1. Öffnen Sie das Snap-In **Microsoft Azure Backup** auf dem *Zielcomputer*.
2. Stellen Sie sicher, dass der *Zielcomputer* und der *Quellcomputer* im gleichen Sicherungstresor registriert sind.
3. Klicken Sie auf **Daten wiederherstellen** , um den Workflow zu initiieren.

    ![Daten wiederherstellen](./media/backup-azure-restore-windows-server-classic/recover.png)
4. Wählen Sie **Anderer Server**

    ![Anderer Server](./media/backup-azure-restore-windows-server-classic/anotherserver.png)
5. Geben Sie die Datei mit den Tresoranmeldeinformationen an, die dem *Beispieltresor*entspricht. Wenn die Datei mit den Tresoranmeldeinformationen ungültig (oder abgelaufen) ist, laden Sie eine neue Anmeldeinformationsdatei für den Tresor aus dem *Beispieltresor* im klassischen Azure-Portal herunter. Sobald die Anmeldeinformationsdatei für den Tresor bereitgestellt wurde, wird der Sicherungstresor angezeigt, der der Tresor-Anmeldeinformationsdatei entspricht.
6. Wählen Sie den *Quellcomputer* aus der Liste der angezeigten Computer aus.

    ![Liste der Computer](./media/backup-azure-restore-windows-server-classic/machinelist.png)
7. Wählen Sie entweder die Option **Dateien suchen** oder **Dateien durchsuchen**. In diesem Abschnitt verwenden wir die Option **Dateien suchen** .

    ![Suchen,](./media/backup-azure-restore-windows-server-classic/search.png)
8. Wählen Sie im nächsten Bildschirm das Volume und das Datum aus. Suchen Sie nach den Ordner-/Dateinamen, die Sie wiederherstellen möchten.

    ![Elemente suchen](./media/backup-azure-restore-windows-server-classic/searchitems.png)
9. Wählen Sie den Speicherort aus, an dem die Dateien wiederhergestellt werden sollen.

    ![Wiederherstellungsspeicherort](./media/backup-azure-restore-windows-server-classic/restorelocation.png)
10. Geben Sie die Verschlüsselungspassphrase an, die während der Registrierung des *Quellcomputers* beim *Beispieltresor* bereitgestellt wurde.

    ![Verschlüsselung](./media/backup-azure-restore-windows-server-classic/encryption.png)
11. Klicken Sie nach dem Bereitstellen der Eingabe auf **Wiederherstellen**. Die Wiederherstellung der gesicherten Dateien am angegebenen Ziel wird ausgelöst.

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

8. Wählen Sie im Bereich **Volume und Datum auswählen** das Volume aus, das die wiederherzustellenden Dateien bzw. Ordner enthält.

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
    > Wenn Sie nicht auf „Bereitstellung aufheben“ klicken, bleibt die Bereitstellung des Wiederherstellungsvolumes ab dem Bereitstellungszeitpunkt sechs Stunden lang erhalten. Während der Bereitstellung des Volumes werden keine Sicherungsvorgänge durchgeführt. Alle Sicherungsvorgänge, deren Ausführung für den Zeitraum der Volumebereitstellung geplant ist, werden ausgeführt, nachdem die Bereitstellung des Wiederherstellungsvolumes aufgehoben wurde.
    >


## <a name="next-steps"></a>Nächste Schritte
* [Azure Backup – Häufig gestellte Fragen](backup-azure-backup-faq.md)
* Besuchen Sie das [Azure Sicherungs-Forum](http://go.microsoft.com/fwlink/p/?LinkId=290933).

## <a name="learn-more"></a>Weitere Informationen
* [Azure Backup – Übersicht](http://go.microsoft.com/fwlink/p/?LinkId=222425)
* [Sichern von virtuellen Azure-Computern](backup-azure-vms-introduction.md)
* [Sichern von Microsoft-Workloads](backup-azure-dpm-introduction.md)

