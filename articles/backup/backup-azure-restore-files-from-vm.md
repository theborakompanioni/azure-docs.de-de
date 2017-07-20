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
ms.translationtype: Human Translation
ms.sourcegitcommit: b1d56fcfb472e5eae9d2f01a820f72f8eab9ef08
ms.openlocfilehash: c65976c7394c7f9691526c0914854ef09184ab07
ms.contentlocale: de-de
ms.lasthandoff: 07/06/2017


---
# <a name="recover-files-from-azure-virtual-machine-backup-preview"></a>Wiederherstellen von Dateien aus einem Wiederherstellungspunkt für virtuelle Azure-Computer (Vorschau)

Azure Backup bietet die Möglichkeit zum Wiederherstellen von [virtuellen Azure-Computern und Datenträgern](./backup-azure-arm-restore-vms.md) aus Azure-VM-Sicherungen. In diesem Artikel wird erläutert, wie Sie Elemente wie Dateien und Ordner aus einer Azure-VM-Sicherung wiederherstellen können.

> [!Note]
> Dies ist für Azure-VMs möglich, die mit dem Ressourcen-Manager-Modell bereitgestellt wurden und durch einen Recovery Services-Tresor geschützt werden.
> Die Dateiwiederherstellung aus einer verschlüsselten VM-Sicherung wird nicht unterstützt.
>

## <a name="mount-the-volume-and-copy-files"></a>Bereitstellen des Volumes und Kopien von Dateien

1. Melden Sie sich beim [Azure-Portal](http://portal.Azure.com) an. Suchen Sie den entsprechenden Recovery Services-Tresor und das erforderliche Sicherungselement.

2. Klicken Sie auf dem Blatt „Sicherungselement“ auf **Dateiwiederherstellung (Vorschau)**.

    ![Öffnen des Sicherungselements im Recovery Services-Tresors](./media/backup-azure-restore-files-from-vm/open-vault-item.png)

    Das Blatt **Dateiwiederherstellung** wird geöffnet.

    ![Blatt „Dateiwiederherstellung“](./media/backup-azure-restore-files-from-vm/file-recovery-blade.png)

3. Wählen Sie im Dropdownmenü  **Wiederherstellungspunkt auswählen**  den Wiederherstellungspunkt mit den gewünschten Dateien aus. Standardmäßig ist der letzte Wiederherstellungspunkt bereits ausgewählt.

4. Klicken Sie auf **Ausführbare Datei herunterladen** (bei einer Microsoft Azure-VM) oder **Skript herunterladen** (bei einer Linux-Azure-VM) zum Herunterladen der Software, die Sie zum Kopieren von Dateien aus dem Wiederherstellungspunkt verwenden.

  Mit der ausführbaren Datei bzw. dem Skript wird eine Verbindung zwischen dem lokalen Computer und angegebenen Wiederherstellungspunkt hergestellt.

5. Führen Sie auf dem Computer, auf dem Sie die Dateien wiederherstellen möchten, die ausführbare Datei bzw. das Skript aus. Die Ausführung muss mit Administratoranmeldeinformationen erfolgen. Wenn Sie das Skript auf einem Computer mit eingeschränktem Zugriff ausführen, stellen Sie sicher, dass Zugriff auf Folgendes besteht:

    - go.microsoft.com
    - Azure-Endpunkte für Azure-VM-Sicherungen
    - Ausgehender Port 3260

   Für Linux benötigt das Skript zum Herstellen der Verbindung mit dem Wiederherstellungspunkt die Komponenten „open-iscsi“ und „lshw“. Wenn diese nicht auf dem Computer vorhanden sind, auf dem es ausgeführt wird, wird um die Erlaubnis zum Installieren der entsprechenden Komponenten gebeten, die bei Erteilung installiert werden.
      
    ![Blatt „Dateiwiederherstellung“](./media/backup-azure-restore-files-from-vm/executable-output.png)
    
   
   Sie können das Skript auf allen Computern ausführen, die dasselbe (oder ein kompatibles) Betriebssystem wie die gesicherte VM haben. Siehe hierzu die Tabelle [Kompatible Betriebssysteme](backup-azure-restore-files-from-vm.md#compatible-os) mit den kompatiblen Betriebssystemen. Wenn der geschützte virtuelle Azure-Computer das Feature „Windows-Speicherplätze“ (für Windows-VMs) oder LVM/RAID-Arrays (für Linux-VMs) verwendet, können Sie die ausführbare Datei bzw. das Skript nicht auf diesem virtuellen Computer ausführen. Führen Sie es stattdessen auf einem beliebigen anderen Computer mit einem kompatiblen Betriebssystem aus.

### <a name="compatible-os"></a>Kompatible Betriebssysteme

#### <a name="for-windows"></a>Für Windows

Die folgende Tabelle zeigt die Kompatibilität zwischen Server- und Clientbetriebssystemen. Dateien können nur zwischen kompatiblen Betriebssystemen wiederhergestellt werden.

|Serverbetriebssystem | Kompatibles Clientbetriebssystem  |
| --------------- | ---- |
| Windows Server 2012 R2 | Windows 8.1 |
| Windows Server 2012    | Windows 8  |
| Windows Server 2008 R2 | Windows 7   |

#### <a name="for-linux"></a>Für Linux

Unter Linux ist die grundlegende Anforderung, dass das Betriebssystem des Computers, auf dem das Skript ausgeführt wird, das Dateisystem der Dateien auf der gesicherten Linux-VM unterstützen muss. Stellen Sie bei Auswahl eines Computers zum Ausführen des Skripts sicher, dass dieser über das kompatible Betriebssystem und die Versionen verfügt, die in der folgenden Tabelle angegeben sind.

|Linux-Betriebssystem | Versionen  |
| --------------- | ---- |
| Ubuntu | ab 12.04 |
| CentOS | ab 6.5  |
| RHEL | ab 6.7 |
| Debian | ab 7 |
| Oracle Linux | ab 6.4 |

Das Skript erfordert auch, dass Python- und Bash-Komponenten ausgeführt werden und eine sichere Verbindung mit dem Wiederherstellungspunkt herstellen.

|Komponente | Version  |
| --------------- | ---- |
| Bash | ab 4 |
| Python | ab 2.6.6  |


### <a name="identifying-volumes"></a>Bestimmen von Volumes

#### <a name="for-windows"></a>Für Windows

Wenn Sie die ausführbare Datei ausführen, stellt das Betriebssystem neue Volumes bereit, denen Laufwerkbuchstaben zugewiesen werden. Über den Windows-Explorer oder Datei-Explorer können Sie zu diesen Laufwerken navigieren. Die den Volumes zugewiesenen Laufwerkbuchstaben entsprechen ggf. nicht den Buchstaben des ursprünglichen virtuellen Computers, doch der Volumename wird beibehalten. Wenn z.B. das Volume auf dem ursprünglichen virtuellen Computer „Data Disk (E:\)“ hieß, kann dieses Volume auf dem lokalen Computer als „Data Disk ('Beliebiger verfügbarer Laufwerkbuchstaben':\)“ angefügt werden. Durchsuchen Sie alle in der Skriptausgabe erwähnten Volumes, bis Sie Ihre Dateien/Ordner gefunden haben.  
       
   ![Blatt „Dateiwiederherstellung“](./media/backup-azure-restore-files-from-vm/volumes-attached.png)
           
#### <a name="for-linux"></a>Für Linux

Unter Linux werden die Volumes des Wiederherstellungspunkts im Ordner bereitgestellt, in dem das Skript ausgeführt wird. Die angefügten Datenträger, Volumes und entsprechenden Bereitstellungspfade werden entsprechend angezeigt. Diese Bereitstellungspfade werden Benutzern mit der Berechtigung „root“ angezeigt. Durchsuchen Sie die Volumes in der Ausgabe des Skripts.

  ![Blatt „Linux-Dateiwiederherstellung“](./media/backup-azure-restore-files-from-vm/linux-mount-paths.png)
  

## <a name="closing-the-connection"></a>Schließen der Verbindung

Nachdem die Dateien bestimmt und an den lokalen Speicherort kopiert wurden, können Sie die zusätzlichen Laufwerke entfernen (oder ihre Bereitstellung aufheben). Zum Aufheben der Laufwerke klicken Sie im Azure-Portal auf dem Blatt **Dateiwiederherstellung** auf **Bereitstellung der Datenträger aufheben**.

![Aufheben der Bereitstellung der Datenträger](./media/backup-azure-restore-files-from-vm/unmount-disks3.png)

Nachdem die Bereitstellung der Datenträger aufgehoben wurde, erhalten Sie die Meldung, dass der Vorgang erfolgreich war. Die Aktualisierung der Verbindung kann einige Minuten dauern, sodass Sie die Datenträger entfernen können.

Nachdem unter Linux die Verbindung mit dem Wiederherstellungspunkt getrennt wurde, entfernt das Betriebssystem die entsprechenden Bereitstellungspfade nicht automatisch. Diese sind als „verwaiste“ Volumes vorhanden und sichtbar, lösen aber einen Fehler aus, wenn ein Zugriff bzw. Schreibzugriff auf die Dateien erfolgt. Sie können manuell entfernt werden. Das Skript ermittelt bei seiner Ausführung solche Volumes von vorherigen Wiederherstellungspunkten und bereinigt diese nach Zustimmung.

## <a name="special-configurations"></a>Besondere Konfigurationen

### <a name="dynamic-disks"></a>Dynamische Datenträger

Verfügt die gesicherte Azure-VM über Volumes, die mehrere Datenträger (übergreifende und Stripesetvolumes) und/oder fehlertolerante Datenträger (gespiegelte und RAID-5-Volumes) auf dynamischen Datenträgern umfassen, können Sie das ausführbare Skript nicht auf derselben VM ausführen. Führen Sie das ausführbare Skript stattdessen auf einem beliebigen anderen Computer mit einem kompatiblen Betriebssystem aus.

### <a name="windows-storage-spaces"></a>Windows-Speicherplätze

„Windows-Speicherplätze“ ist eine Technologie im Windows-Speicher, die es ermöglicht, Speicher zu virtualisieren. Sie können mit „Windows-Speicherplätze“ branchenübliche Datenträger in Speicherpools gruppieren und anschließend mithilfe des in diesen Speicherpools verfügbaren Speicherplatzes virtuelle Datenträger erstellen, die „Speicherplätze“ genannt werden.

Wenn der virtuelle Azure-Computer mithilfe von „Windows-Speicherplätze“ gesichert wurde, können Sie das ausführbare Skript nicht auf diesem virtuellen Computer ausführen. Führen Sie das ausführbare Skript stattdessen auf einem beliebigen anderen Computer mit einem kompatiblen Betriebssystem aus.

### <a name="lvmraid-arrays"></a>LVM-/RAID-Arrays

Unter Linux werden LVM- (Logical Volume Manager) bzw. softwaregestützte RAID-Arrays zum Verwalten logischer Volumes verwendet, die sich über mehrere Datenträger erstrecken. Wenn die gesicherte Linux-VM mit LVM- bzw. RAID-Arrays arbeitet, kann das Skript nicht auf derselben VM ausgeführt werden. Führen Sie das Skript stattdessen auf einem anderen Computer mit kompatiblem Betriebssystem und Unterstützung des Dateisystems der gesicherten VM aus.

Die Ausgabe des Skripts zeigt die Datenträger und Volumes des LVM- bzw. RAID-Arrays samt Partitionstyp (siehe unten).

   ![Blatt mit der LVM-Ausgabe unter Linux](./media/backup-azure-restore-files-from-vm/linux-LVMOutput.png)
   
Der Benutzer muss die folgenden Befehle ausführen, um diese Partitionen online zu schalten. 

**Für LVM-Partitionen**

```
$ pvs <volume name as shown above in the script output> 
```
Hiermit werden die Volumegruppennamen unter einem physischen Volume aufgeführt.

```
$ lvdisplay <volume-group-name from the pvs command’s results> 
```
Hiermit werden alle logischen Volumes mit Name und Pfad in einer Volumegruppe aufgeführt.

```
$ mount <LV path> </mountpath>
```
Hiermit werden die logischen Volumes im Pfad Ihrer Wahl bereitgestellt.


**Für RAID-Arrays**

```
$ mdadm –detail –scan
```
Hiermit werden Details zu allen RAID-Datenträgern angezeigt. Der entsprechende RAID-Datenträger wird als `/dev/mdm/<RAID array name in the backed up VM>` angezeigt.

Verwenden Sie den Befehl „mount“, wenn der RAID-Datenträger physische Volumes hat.
```
$ mount [RAID Disk Path] [/mounthpath]
```

Wenn für diesen RAID-Datenträger ein anderer LVM konfiguriert ist, befolgen Sie die obigen Anweisungen für LVM-Partitionen, wobei der Volumename dann der Name des RAID-Datenträgers ist.

## <a name="troubleshooting"></a>Problembehandlung

Wenn Sie Probleme beim Wiederherstellen von Dateien von den virtuellen Computern haben, überprüfen Sie die folgende Tabelle mit zusätzlichen Informationen.

| Fehlermeldung/Szenario | Mögliche Ursache | Empfohlene Maßnahme |
| ------------------------ | -------------- | ------------------ |
| EXE-Ausgabe: *Ausnahme beim Herstellen einer Verbindung mit dem Ziel* |Skript kann nicht auf den Wiederherstellungspunkt zugreifen. | Prüfen Sie, ob der Computer die oben genannten Zugriffsanforderungen erfüllt.|  
|   EXE-Ausgabe: *Das Ziel wurde bereits in einer iSCSI-Sitzung angemeldet.* | Das Skript wurde bereits auf dem gleichen Computer ausgeführt, und die Laufwerke wurden angefügt. | Die Volumes des Wiederherstellungspunkts wurden bereits angefügt. Sie wurden ggf. NICHT mit denselben Laufwerkbuchstaben des ursprünglichen virtuellen Computers bereitgestellt. Suchen Sie im Datei-Explorer auf allen verfügbaren Volumes nach Ihrer Datei. |
| EXE-Ausgabe: *Dieses Skript ist ungültig, da die Bereitstellung der Datenträger über das Portal aufgehoben bzw. das 12-Stunden-Limit überschritten wurde. Laden Sie ein neues Skript aus dem Portal herunter.* |  Die Bereitstellung der Datenträger über das Portal wurde aufgehoben oder das 12-Stunden-Limit überschritten. |    Diese bestimmte EXE-Datei ist jetzt ungültig und kann nicht ausgeführt werden. Wenn Sie auf die Dateien dieser zeitpunktbezogenen Wiederherstellung zugreifen möchten, fordern Sie im Portal eine neue EXE-Datei an.|
| Auf dem Computer, auf dem die EXE-Datei ausgeführt wird: Die Bereitstellung der neuen Volumes wird erst aufgehoben, nachdem auf die Schaltfläche „Bereitstellung aufheben“ geklickt wurde. |    Der iSCSI-Initiator auf dem Computer reagiert nicht bzw. aktualisiert nicht seine Verbindung mit dem Ziel und verwaltet nicht den Cache. |    Warten Sie einige Minuten, nachdem auf die Schaltfläche „Bereitstellung aufheben“ geklickt wurde. Wenn die Bereitstellung der neuen Volumes noch nicht aufgehoben wurde, navigieren Sie durch alle Volumes. Dies zwingt den Initiator zum Aktualisieren der Verbindung, und das Volume wird mit der Fehlermeldung aufgehoben, dass der Datenträger nicht verfügbar ist.|
| EXE-Ausgabe: Das Skript wird erfolgreich ausgeführt, aber „Neue Volumes angefügt“ wird nicht in der Ausgabe des Skripts angezeigt. | Dies ist ein vorübergehender Fehler.   | Die Volumes wurden bereits angefügt. Öffnen Sie Explorer, um zu ihnen zu navigieren. Wenn Sie zum Ausführen von Skripts jedes Mal den gleichen Computer verwenden, sollten Sie den Computer neu starten, und die Liste sollte in den nachfolgenden Ausführungen der EXE-Datei angezeigt werden. |
| Linux-spezifische Probleme: Die gewünschten Volumes können nicht angezeigt werden. | Das Betriebssystem des Computers, auf dem das Skript ausgeführt wird, erkennt möglicherweise nicht das zugrunde liegende Dateisystem der gesicherten VM. | Prüfen Sie, ob der Wiederherstellungspunkt absturz- oder dateikonsistent ist. Falls dateikonsistent, führen Sie das Skript auf einem anderen Computer aus, dessen Betriebssystem das Dateisystem der gesicherten VM erkennt. |
| Windows-spezifische Probleme: Die gewünschten Volumes können nicht angezeigt werden. | Möglicherweise wurden die Datenträger angefügt, aber die Volumes wurden nicht konfiguriert. | Suchen Sie auf dem Bildschirm „Datenträgerverwaltung“ die zusätzlichen Datenträger im Zusammenhang mit dem Wiederherstellungspunkt. Wenn sich einer dieser Datenträger im Offlinestatus befindet, versuchen Sie, diesen online zu schalten, indem Sie mit der rechten Maustaste auf den Datenträger und dann auf „Online“ klicken.|

