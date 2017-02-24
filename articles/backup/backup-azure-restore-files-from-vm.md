---
title: 'Azure Backup: Wiederherstellen von Dateien und Ordnern aus einer Azure-VM-Sicherung | Microsoft-Dokumentation'
description: "Informationen zum Wiederherstellen von Dateien aus einem Wiederherstellungspunkt für virtuelle Azure-Computer"
services: backup
documentationcenter: dev-center-name
author: pvrk
manager: shivamg
keywords: Wiederherstellung auf Elementebene; Wiederherstellung von Dateien aus Azure-VM-Sicherung; Wiederherstellen von Dateien aus Azure-VM
ms.assetid: f1c067a2-4826-4da4-b97a-c5fd6c189a77
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 2/6/2017
ms.author: pullabhk;markgal
translationtype: Human Translation
ms.sourcegitcommit: 5c31fcac727a1206cb416cbe7340392a2816729a
ms.openlocfilehash: 93b689c132ec7e391a9107e4e3eb73a48e3ba09d


---
# <a name="recover-files-from-azure-virtual-machine-backup-preview"></a>Wiederherstellen von Dateien aus einem Wiederherstellungspunkt für virtuelle Azure-Computer (Vorschau)

Azure Backup bietet die Möglichkeit zum Wiederherstellen von [virtuellen Azure-Computern und Datenträgern](./backup-azure-arm-restore-vms.md) aus Azure-VM-Sicherungen. In diesem Artikel wird erläutert, wie Sie Elemente wie Dateien und Ordner aus einer Azure-VM-Sicherung wiederherstellen können.

> [!Note]
> Dies ist für Azure-VMs möglich, die mit dem Ressourcen-Manager-Modell bereitgestellt wurden und durch einen Recovery Services-Tresor geschützt werden.
> Derzeit ist die Wiederherstellung von Dateien aus einer Microsoft Azure-VM-Sicherung in der Vorschauphase. Die Wiederherstellung von Dateien aus einer Azure-VM unter Linux wird in Kürze verfügbar sein.
Derzeit wird die Dateiwiederherstellung aus einer verschlüsselten VM-Sicherung nicht unterstützt.
>

## <a name="mount-the-volume-and-copy-files"></a>Bereitstellen des Volumes und Kopien von Dateien

1. Melden Sie sich beim [Azure-Portal](http://portal.Azure.com) an. Suchen Sie den entsprechenden Recovery Services-Tresor und das erforderliche Sicherungselement.

2. Klicken Sie auf dem Blatt „Sicherungselement“ auf **Dateiwiederherstellung (Vorschau)**.

    ![Öffnen des Sicherungselements im Recovery Services-Tresors](./media/backup-azure-restore-files-from-vm/open-vault-item.png)

    Das Blatt **Dateiwiederherstellung** wird geöffnet.

    ![Blatt „Dateiwiederherstellung“](./media/backup-azure-restore-files-from-vm/file-recovery-blade.png)

3. Wählen Sie im Dropdownmenü ** Wiederherstellungspunkt auswählen ** den Wiederherstellungspunkt mit den gewünschten Dateien aus. Standardmäßig ist der letzte Wiederherstellungspunkt bereits ausgewählt.

4. Klicken Sie auf **Ausführbare Datei herunterladen** zum Herunterladen der Software, die Sie zum Kopieren von Dateien aus dem Wiederherstellungspunkt verwenden.

  Die ausführbare Datei ist ein Skript, das eine Verbindung zwischen dem lokalen Computer und angegebenen Wiederherstellungspunkt herstellt.

5. Führen Sie auf dem Computer, auf dem Sie die Dateien wiederherstellen möchten, die ausführbare Datei aus. Sie müssen das Skript mit Administratoranmeldeinformationen ausführen. Wenn Sie das Skript auf einem Computer mit eingeschränktem Zugriff ausführen, stellen Sie sicher, dass Zugriff auf Folgendes besteht:

    - go.microsoft.com
    - Azure-Endpunkte für Azure-VM-Sicherungen
    - Ausgehender Port 3260

    ![Blatt „Dateiwiederherstellung“](./media/backup-azure-restore-files-from-vm/executable-output.png)

    Sie können das Skript auf allen Computern mit demselben (oder einem kompatiblen) Betriebssystem wie auf dem Computer ausführen, der zum Erstellen des Wiederherstellungspunkts verwendet wurde. Siehe hierzu die Tabelle [Kompatible Betriebssysteme](backup-azure-restore-files-from-vm.md#compatible-os) mit den kompatiblen Betriebssystemen. Wenn der geschützte virtuelle Azure-Computer das Feature „Windows-Speicherplätze“ verwendet, können Sie das ausführbare Skript nicht auf diesem virtuellen Computer ausführen. Führen Sie stattdessen das ausführbare Skript auf einem anderen Computer aus, auf dem „Windows-Speicherplätze“ verwendet wird. Das Ausführen des ausführbaren Skripts auf einem Computer mit einem kompatiblen Betriebssystem wird empfohlen.

    ![Blatt „Dateiwiederherstellung“](./media/backup-azure-restore-files-from-vm/volumes-attached.png)

### <a name="compatible-os"></a>Kompatible Betriebssysteme

Die folgende Tabelle zeigt die Kompatibilität zwischen Server- und Clientbetriebssystemen. Dateien können nur zwischen kompatiblen Betriebssystemen wiederhergestellt werden.

|Serverbetriebssystem | Kompatibles Clientbetriebssystem  |
| --------------- | ---- |
| Windows Server 2012 R2 | Windows 8.1 |
| Windows Server 2012    | Windows 8  |
| Windows Server 2008 R2 | Windows 7   |


### <a name="drive-letter-assignments"></a>Zuordnung von Laufwerkbuchstaben

Wenn Sie das Skript ausführen, stellt das Betriebssystem neue Volumes bereit und weist Laufwerkbuchstaben zu. Über den Windows-Explorer oder Datei-Explorer können Sie zu diesen Laufwerken navigieren. Die den Volumes zugewiesenen Laufwerkbuchstaben entsprechen ggf. nicht den Buchstaben des ursprünglichen virtuellen Computers, doch der Volumename wird beibehalten. Wenn z.B. das Volume auf dem ursprünglichen virtuellen Computer „Data Disk (E:\)“ hieß, kann dieses Volume auf dem lokalen Computer als „Data Disk ('Beliebiger verfügbarer Laufwerkbuchstaben':\)“ angefügt werden. Durchsuchen Sie alle in der Skriptausgabe erwähnten Volumes, bis Sie Ihre Dateien/Ordner gefunden haben.  


## <a name="closing-the-connection"></a>Schließen der Verbindung

Nachdem die Dateien bestimmt und an den lokalen Speicherort kopiert wurden, können Sie die zusätzlichen Laufwerke entfernen (oder ihre Bereitstellung aufheben). Zum Aufheben der Laufwerke klicken Sie im Azure-Portal auf dem Blatt **Dateiwiederherstellung** auf **Bereitstellung der Datenträger aufheben**.

![Aufheben der Bereitstellung der Datenträger](./media/backup-azure-restore-files-from-vm/unmount-disks3.png)

Nachdem die Bereitstellung der Datenträger aufgehoben wurde, erhalten Sie die Meldung, dass der Vorgang erfolgreich war. Die Aktualisierung der Verbindung kann einige Minuten dauern, sodass Sie die Datenträger entfernen können.

## <a name="windows-storage-spaces"></a>Windows-Speicherplätze

„Windows-Speicherplätze“ ist eine Technologie im Windows-Speicher, die es ermöglicht, Speicher zu virtualisieren. Sie können mit „Windows-Speicherplätze“ branchenübliche Datenträger in Speicherpools gruppieren und anschließend mithilfe des in diesen Speicherpools verfügbaren Speicherplatzes virtuelle Datenträger erstellen, die „Speicherplätze“ genannt werden.

Wenn der virtuelle Azure-Computer mithilfe von „Windows-Speicherplätze“ gesichert wurde, können Sie das ausführbare Skript nicht auf diesem virtuellen Computer ausführen. Führen Sie stattdessen das ausführbare Skript auf einem anderen Computer aus, auf dem „Windows-Speicherplätze“ verwendet wird. Das Ausführen des ausführbaren Skripts auf einem Computer mit einem kompatiblen Betriebssystem wird empfohlen.

## <a name="troubleshooting"></a>Problembehandlung

Wenn Sie Probleme beim Wiederherstellen von Dateien von den virtuellen Computern haben, überprüfen Sie die folgende Tabelle mit zusätzlichen Informationen.

| Fehlermeldung/Szenario | Mögliche Ursache | Empfohlene Maßnahme |
| ------------------------ | -------------- | ------------------ |
| EXE-Ausgabe: *Ausnahme beim Herstellen einer Verbindung mit dem Ziel* |Skript kann nicht auf den Wiederherstellungspunkt zugreifen.    | Prüfen Sie, ob der Computer die oben genannten Zugriffsanforderungen erfüllt.|  
|    EXE-Ausgabe: *Das Ziel wurde bereits in einer iSCSI-Sitzung angemeldet.* |    Das Skript wurde bereits auf dem gleichen Computer ausgeführt, und die Laufwerke wurden angefügt. |    Die Volumes des Wiederherstellungspunkts wurden bereits angefügt. Sie wurden ggf. NICHT mit denselben Laufwerkbuchstaben des ursprünglichen virtuellen Computers bereitgestellt. Suchen Sie im Datei-Explorer auf allen verfügbaren Volumes nach Ihrer Datei. |
| EXE-Ausgabe: *Dieses Skript ist ungültig, da die Bereitstellung der Datenträger über das Portal aufgehoben bzw. das 12-Stunden-Limit überschritten wurde. Laden Sie ein neues Skript aus dem Portal herunter.* |    Die Bereitstellung der Datenträger über das Portal wurde aufgehoben oder das 12-Stunden-Limit überschritten. |    Diese bestimmte EXE-Datei ist jetzt ungültig und kann nicht ausgeführt werden. Wenn Sie auf die Dateien dieser zeitpunktbezogenen Wiederherstellung zugreifen möchten, fordern Sie im Portal eine neue EXE-Datei an.|
| Auf dem Computer, auf dem die EXE-Datei ausgeführt wird: Die Bereitstellung der neuen Volumes wird erst aufgehoben, nachdem auf die Schaltfläche „Bereitstellung aufheben“ geklickt wurde. |    Der iSCSI-Initiator auf dem Computer reagiert nicht bzw. aktualisiert nicht seine Verbindung mit dem Ziel und verwaltet nicht den Cache. |    Warten Sie einige Minuten, nachdem auf die Schaltfläche „Bereitstellung aufheben“ geklickt wurde. Wenn die Bereitstellung der neuen Volumes noch nicht aufgehoben wurde, navigieren Sie durch alle Volumes. Dies zwingt den Initiator zum Aktualisieren der Verbindung, und das Volume wird mit der Fehlermeldung aufgehoben, dass der Datenträger nicht verfügbar ist.|
| EXE-Ausgabe: Das Skript wird erfolgreich ausgeführt, aber „Neue Volumes angefügt“ wird nicht in der Ausgabe des Skripts angezeigt. |    Dies ist ein vorübergehender Fehler.    | Die Volumes wurden bereits angefügt. Öffnen Sie Explorer, um zu ihnen zu navigieren. Wenn Sie zum Ausführen von Skripts jedes Mal den gleichen Computer verwenden, sollten Sie den Computer neu starten, und die Liste sollte in den nachfolgenden Ausführungen der EXE-Datei angezeigt werden. |



<!--HONumber=Feb17_HO1-->


