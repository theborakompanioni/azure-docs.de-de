---
title: "Anfügen eines Datenträgers an einen virtuellen Linux-Computer | Microsoft Docs"
description: "Erfahren Sie, wie Sie einen Datenträger mit dem klassischen Bereitstellungsmodell einer Linux-VM anfügen und den Datenträger initialisieren, sodass er einsatzbereit ist."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 4901384d-2a6f-4f46-bba0-337a348b7f87
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 11/14/2016
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: 17ddda372f3a232be62e565b700bb1be967fb8e3
ms.openlocfilehash: 96ec97914784f53739b11b4ef7916de620b4ed6e


---
# <a name="how-to-attach-a-data-disk-to-a-linux-virtual-machine"></a>Gewusst wie: Anfügen eines Datenträgers an einen virtuellen Linux-Computer
> [!IMPORTANT] 
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Resource Manager- und klassische Bereitstellung](../azure-resource-manager/resource-manager-deployment-model.md). Dieser Artikel befasst sich mit der Verwendung des klassischen Bereitstellungsmodells. Microsoft empfiehlt für die meisten neuen Bereitstellungen die Verwendung des Resource Manager-Modells. Erfahren Sie, wie Sie [einen Datenträger mithilfe des Resource Manager-Bereitstellungsmodell anfügen](virtual-machines-linux-add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Sie können sowohl leere Datenträger, als auch Datenträger mit Daten an Ihre virtuellen Azure-Computer anfügen. Beide Arten von Datenträgern sind VHD-Dateien, die sich in einem Azure Storage-Konto befinden. Wie bei jedem Hinzufügen von Datenträgern zu einem Linux-Computer müssen Sie den Datenträger nach dem Anfügen initialisieren und formatieren, damit er verwendet werden kann. In diesem Artikel wird beschrieben, wie Sie sowohl leere Datenträger als auch Datenträger mit Daten an Ihre VMs anfügen, und wie Sie daraufhin einen neuen Datenträger initialisieren und formatieren.

> [!NOTE]
> Es empfiehlt sich, einen oder mehrere separate Datenträger zu verwenden, um die Daten eines virtuellen Computers zu speichern. Wenn Sie einen virtuellen Azure-Computer erstellen, verfügt dieser über einen Betriebssystem-Datenträger und über einen temporären Datenträger. **Verwenden Sie den temporären Datenträger nicht zum Speichern von persistenten Daten.** Wie der Name schon sagt, bietet dieser Datenträger nur temporäre Speicherung. Er ermöglicht keine Redundanz oder Sicherung, da er sich nicht im Azure-Speicher befindet.
> Unter Linux wird der temporäre Datenträger normalerweise vom Azure Linux Agent verwaltet und automatisch an **/mnt/resource** (oder bei Ubuntu-Images an **/mnt**) angefügt. Andererseits kann ein Datenträger vom Linux-Kernel beispielsweise den Namen `/dev/sdc` erhalten, und Sie müssen diese Ressource partitionieren, formatieren und einbinden. Weitere Informationen finden Sie im [Benutzerhandbuch für Azure Linux-Agent][Agent].
> 
> 

[!INCLUDE [howto-attach-disk-windows-linux](../../includes/howto-attach-disk-linux.md)]

## <a name="initialize-a-new-data-disk-in-linux"></a>Initialisieren eines neues Datenträgers unter Linux
1. Stellen Sie mithilfe des SSH-Netzwerkprotokolls eine Verbindung zu Ihrer VM her. Weitere Informationen finden Sie unter [Anmelden bei einem virtuellen Computer, auf dem Linux ausgeführt wird][Logon].
2. Als Nächstes müssen Sie die Geräte-ID für den zu initialisierenden Datenträger finden. Dafür gibt zwei Möglichkeiten:
   
    a) Grep für SCSI-Geräte in den Protokollen, z.B. wie im nachfolgenden Befehl:
   
    ```bash
    sudo grep SCSI /var/log/messages
    ```
   
    Für aktuelle Ubuntu-Verteilungen müssen Sie möglicherweise `sudo grep SCSI /var/log/syslog` verwenden, da die Anmeldung bei `/var/log/messages` möglicherweise standardmäßig deaktiviert ist.
   
    In den angezeigten Meldungen können Sie den Bezeichner des letzten hinzugefügten Datenträgers finden.
   
    ![Datenträgermeldungen abrufen](./media/virtual-machines-linux-classic-attach-disk/scsidisklog.png)
   
    OR
   
    b) Verwenden Sie den Befehl `lsscsi`, um die Geräte-ID zu ermitteln. `lsscsi` kann entweder über `yum install lsscsi` (für auf Red Hat basierende Distributionen) oder über `apt-get install lsscsi` (für auf Debian basierende Distributionen) installiert werden. Den gesuchten Datenträger finden Sie anhand seiner *LUN* (Logical Unit Number) oder **logischen Gerätenummer**. Zum Beispiel kann die *LUN* für die angefügten Datenträger mit `azure vm disk list <virtual-machine>` angezeigt werden:

    ```azurecli
    azure vm disk list myVM
    ```

    Die Ausgabe sieht in etwa wie folgt aus:

    ```azurecli
    info:    Executing command vm disk list
    + Fetching disk images
    + Getting virtual machines
    + Getting VM disks
    data:    Lun  Size(GB)  Blob-Name                         OS
    data:    ---  --------  --------------------------------  -----
    data:         30        myVM-2645b8030676c8f8.vhd  Linux
    data:    0    100       myVM-76f7ee1ef0f6dddc.vhd
    info:    vm disk list command OK
    ```
   
    Vergleichen Sie diese Daten mit der Ausgabe von `lsscsi` für den gleichen virtuellen Beispielcomputer:
   
    ```bash
    [1:0:0:0]    cd/dvd  Msft     Virtual CD/ROM   1.0   /dev/sr0
    [2:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sda
    [3:0:1:0]    disk    Msft     Virtual Disk     1.0   /dev/sdb
    [5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc
    ```
   
    Die letzte Zahl im Tupel in jeder Zeile ist die *LUN*. Weitere Informationen finden Sie unter `man lsscsi` .
3. Geben Sie an der Eingabeaufforderung den folgenden Befehl ein, um Ihr Gerät zu erstellen:
   
    ```bash
    sudo fdisk /dev/sdc
    ```

4. Geben Sie nach Aufforderung **n** ein, um eine Partition zu erstellen.

    ![Gerät erstellen](./media/virtual-machines-linux-classic-attach-disk/fdisknewpartition.png)

5. Geben Sie bei Aufforderung **p** ein, um die Partition als primäre Partition festzulegen. Geben Sie **1** ein, um sie zur ersten Partition zu machen, und drücken Sie die Eingabetaste, um den Standardwert für den Zylinder zu übernehmen. Bei einigen Systemen werden unter Umständen anstelle des Zylinders die Standardwerte des ersten und letzten Abschnitts angezeigt. Sie können diese Standardeinstellungen auch übernehmen.

    ![Partition erstellen](./media/virtual-machines-linux-classic-attach-disk/fdisknewpartdetails.png)


6. Geben Sie **p** ein, um die Details zum Datenträger anzuzeigen, der gerade partitioniert wird.

    ![Informationen zum Datenträger auflisten](./media/virtual-machines-linux-classic-attach-disk/fdiskpartitiondetails.png)


7. Geben Sie **w** ein, um die Einstellungen für den Datenträger zu schreiben.

    ![Datenträgeränderungen schreiben](./media/virtual-machines-linux-classic-attach-disk/fdiskwritedisk.png)

8. Sie können nun das Dateisystem auf der neuen Partition erstellen. Fügen Sie die Nummer der Partition an die Geräte-ID an (im folgenden Beispiel `/dev/sdc1`). Das folgende Beispiel erstellt eine „ext4“-Partition auf /dev/sdc1:
   
    ```bash
    sudo mkfs -t ext4 /dev/sdc1
    ```
   
    ![Dateisystem erstellen](./media/virtual-machines-linux-classic-attach-disk/mkfsext4.png)
   
   > [!NOTE]
   > SUSE Linux Enterprise 11-Systeme unterstützen für ext4-Dateisysteme nur den schreibgeschützten Zugriff. Für diese Systeme sollten Sie das neue Dateisystem als ext3 statt ext4 formatieren.

9. Erstellen Sie ein Verzeichnis zum Bereitstellen des neuen Dateisystems, wie nachfolgend beschrieben:
   
    ```bash
    sudo mkdir /datadrive
    ```

10. Schließlich können Sie das Laufwerk wie folgt bereitstellen:
   
    ```bash
    sudo mount /dev/sdc1 /datadrive
    ```
   
    Der Datenträger kann nun als **/datadrive**verwendet werden.
   
    ![Erstellen des Verzeichnisses und Bereitstellen der Festplatte](./media/virtual-machines-linux-classic-attach-disk/mkdirandmount.png)

11. Fügen Sie das neue Laufwerk zu /etc/fstab hinzu:
   
    Um sicherzustellen, dass das Laufwerk nach einem Neustart automatisch wieder eingebunden wird, muss es der Datei „/etc/fstab“ hinzugefügt werden. Außerdem wird dringend empfohlen, den UUID (Universally Unique IDentifier) in /etc/fstab zu verwenden, um auf das Laufwerk und nicht auf den Gerätenamen (d.h. auf /dev/sdc1) zu verweisen. Die Verwendung von UUID vermeidet, dass der falsche Datenträger an einem bestimmten Standort bereitgestellt wird, wenn das Betriebssystem einen Fehler während des Starts entdeckt, und alle verbleibenden Datenträger anschließend diesen Geräte-IDs zugewiesen werden. Sie können das Hilfsprogramm **blkid** verwenden, um die UUID des neuen Laufwerks herauszufinden:
   
    ```bash
    sudo -i blkid
    ```
   
    Die Ausgabe sieht in etwa wie im folgenden Beispiel aus:
   
    ```bash
    /dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
    /dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
    /dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
    ```

    > [!NOTE]
    > Eine falsche Bearbeitung der Datei **/etc/fstab** könnte zu einem nicht startfähigen System führen. Wenn Sie sich nicht sicher sind, helfen Ihnen die Informationen zur richtigen Bearbeitung dieser Datei in der Dokumentation weiter. Außerdem wird empfohlen, ein Backup der Datei /etc/fstab zu erstellen, bevor Sie sie bearbeiten.

    Öffnen Sie als Nächstes die Datei **/etc/fstab** in einem Text-Editor:

    ```bash
    sudo vi /etc/fstab
    ```

    In diesem Beispiel verwenden wir den UUID-Wert für das neue Gerät **/dev/sdc1**, das in den vorherigen Schritten erstellt wurde, und den Bereitstellungspunkt **/datadrive**. Fügen Sie am Ende der Datei **/etc/fstab** die folgende Zeile hinzu:

    ```sh
    UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,nofail   1   2
    ```

    In auf SUSE Linux basierenden Systemen müssen Sie möglicherweise ein leicht anderes Format verwenden:

    ```sh
    /dev/disk/by-uuid/33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext3   defaults,nofail   1   2
    ```

    > [!NOTE]
    > Die Option `nofail` stellt sicher, dass der virtuelle Computer gestartet wird, selbst wenn das Dateisystem beschädigt oder der Datenträger zur Startzeit nicht vorhanden ist. Ohne diese Option können Verhalten auftreten, die unter [Cannot SSH to Linux VM due to FSTAB errors](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/) (Aufgrund von FSTAB-Fehler keine SSH-Verbindung mit Linux-VM möglich) beschrieben sind.

    Sie können nun testen, ob das Dateisystem ordnungsgemäß eingebunden wurde, indem Sie die Einbindung aufheben und wiederherstellen. Verwenden Sie dafür den zuvor erstellten Beispielbereitstellungspunkt `/datadrive`:

    ```bash
    sudo umount /datadrive
    sudo mount /datadrive
    ```

    Wenn der Befehl `mount` zu einem Fehler führt, prüfen Sie die Datei "/etc/fstab" auf korrekte Syntax. Wenn zusätzliche Datenlaufwerke oder Partitionen erstellt werden, fügen Sie diese ebenfalls einzeln in „/etc/fstab“ ein.

    Machen Sie das Laufwerk mit folgendem Befehl beschreibbar:

    ```bash
    sudo chmod go+w /datadrive
    ```

    > [!NOTE]
    > Wenn Sie später einen Datenträger entfernen, ohne "fstab" zu bearbeiteten, kann der Start des virtuellen Computers fehlschlagen. Für den Fall, dass dieses Problem häufiger auftritt, bieten die meisten Verteilungen die fstab-Optionen `nofail` und/oder `nobootwait`, die einen Systemstart auch dann erlauben, wenn der Datenträger zur Startzeit nicht eingebunden werden kann. Weitere Informationen zu diesen Parametern finden Sie in der Dokumentation zu Ihrer Distribution.

### <a name="trimunmap-support-for-linux-in-azure"></a>TRIM/UNMAP-Unterstützung für Linux in Azure
Einige Linux-Kernels unterstützen TRIM/UNMAP-Vorgänge, um ungenutzte Blöcke auf dem Datenträger zu verwerfen. Diese Vorgänge sind in erster Linie für Standardspeicher nützlich, um Azure darüber zu informieren, dass gelöschte Seiten nicht mehr gültig sind und verworfen werden können. Das Verwerfen von Seiten kann Kosten sparen, wenn Sie große Dateien erstellen und diese dann löschen.

Es gibt zwei Methoden, TRIM-Unterstützung auf Ihrem virtuellen Linux-Computer zu aktivieren. Den empfohlenen Ansatz finden Sie wie üblich in Ihrer Distribution:

* Verwenden Sie die Bereitstellungsoption `discard` in `/etc/fstab`, z.B.:

    ```sh
    UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,discard   1   2
    ```

* In einigen Fällen kann sich die Option `discard` auf die Leistung auswirken. Alternativ können Sie den Befehl `fstrim` manuell über die Befehlszeile ausführen oder ihn „crontab“ hinzufügen, um eine regelmäßige Ausführung zu erzielen:
  
    **Ubuntu**
  
    ```bash
    sudo apt-get install util-linux
    sudo fstrim /datadrive
    ```
  
    **RHEL/CentOS**
  
    ```bash
    sudo yum install util-linux
    sudo fstrim /datadrive
    ```

## <a name="troubleshooting"></a>Problembehandlung
[!INCLUDE [virtual-machines-linux-lunzero](../../includes/virtual-machines-linux-lunzero.md)]

## <a name="next-steps"></a>Nächste Schritte
Informieren Sie sich in den folgenden Artikeln ausführlicher über das Verwenden von virtuellen Linux-Computern:

* [Anmelden bei einem virtuellen Computer unter Linux][Logon]
* [Trennen eines Datenträgers von einem virtuellen Linux-Computer](virtual-machines-linux-classic-detach-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Verwenden der Azure-Befehlszeilenschnittstelle beim klassischen Bereitstellungsmodell](../virtual-machines-command-line-tools.md)
* [Konfigurieren von RAID auf einem virtuellen Linux-Computer in Azure](virtual-machines-linux-configure-raid.md)
* [Konfigurieren von LVM auf einem virtuellen Linux-Computer in Azure](virtual-machines-linux-configure-lvm.md)

<!--Link references-->
[Agent]: virtual-machines-linux-agent-user-guide.md
[Logon]: virtual-machines-linux-mac-create-ssh-keys.md



<!--HONumber=Dec16_HO1-->


