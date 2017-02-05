---
title: Konfigurieren von LVM auf einem virtuellen Linux-Computer | Microsoft Docs
description: "Erfahren Sie, wie Sie LVM für Linux in Azure konfigurieren."
services: virtual-machines-linux
documentationcenter: na
author: szarkos
manager: timlt
editor: tysonn
tag: azure-service-management,azure-resource-manager
ms.assetid: 7f533725-1484-479d-9472-6b3098d0aecc
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2017
ms.author: szark
translationtype: Human Translation
ms.sourcegitcommit: 9b77d0e6d21ece908960a60d17a0460b806399ee
ms.openlocfilehash: df7a1f44f5c7733a4c4b889db13291f8d3d1a3d8


---
# <a name="configure-lvm-on-a-linux-vm-in-azure"></a>Konfigurieren von LVM auf einem virtuellen Linux-Computer in Azure
In diesem Dokument wird das Konfigurieren von Logical Volume Manager (LVM) auf Ihrem virtuellen Azure-Computer erläutert. Es zwar möglich, LVM auf einem beliebigen, mit dem virtuellen Computer verbundenen Datenträger zu konfigurieren, bei den meisten Cloudimages wird LVM jedoch nicht auf dem Betriebssystemdatenträger konfiguriert. So werden Probleme mit doppelten Volumegruppen vermieden, wenn der Betriebssystemdatenträger einmal an einen anderen virtuellen Computer mit der gleichen Distribution und dem gleichen Typ angeschlossen wird, z.B. in einem Wiederherstellungsszenario. Es wird daher empfohlen, LVM nur auf den Datenträgern zu verwenden.

## <a name="linear-vs-striped-logical-volumes"></a>Lineare Volumes im Vergleich zu logischen Stripesetvolumes
LVM kann verwendet werden, um physische Datenträger in einem einzelnen Speichervolume zusammenzufassen. Standardmäßig erstellt LVM normalerweise lineare logische Volumes, d.h., der physische Speicher wird verkettet. In diesem Fall werden Lese-/Schreibvorgänge in der Regel nur an einen einzelnen Datenträger gesendet. Im Gegensatz dazu können auch logische Stripesetvolumes erstellt werden, bei denen Lese- und Schreibvorgänge auf mehrere Datenträger in der Volumegruppe verteilt werden (ähnlich wie bei RAID0). Aus Leistungsgründen ist es wahrscheinlich, dass die logischen Volumes als Stripesetvolumes eingerichtet werden sollen, damit für Lese- und Schreibvorgänge alle angeschlossenen Datenträger genutzt werden.

In diesem Dokument wird erläutert, wie Sie mehrere Datenträger in einer einzelnen Volumegruppe kombinieren und dann ein logisches Stripesetvolume erstellen. Die folgenden Schritte wurden etwas verallgemeinert, damit sie für die meisten Distributionen verwendet werden können. In den meisten Fällen weichen die Dienstprogramme und Workflows für die Verwaltung von LVM in Azure nicht von anderen Umgebungen ab. Wenden Sie sich wie immer auch an Ihren Linux-Hersteller, um Dokumentation und Informationen zu bewährten Vorgehensweisen zur Verwendung von LVM mit Ihrer Distribution zu erhalten.

## <a name="attaching-data-disks"></a>Anfügen von Datenträgern
Bei der Verwendung von LVM beginnen Sie normalerweise mit zwei oder mehr zusätzlichen leeren Datenträgern. Basierend auf Ihren E/A-Anforderungen können Sie wählen, in unserem Standardspeicher gespeicherte Datenträger mit bis zu 500 IOPS und Datenträger anzufügen oder in unserem Premiumspeicher gespeicherte Datenträger mit bis zu 5000 IOPS und Datenträger anzufügen. In diesem Artikel wird nicht erläutert, wie Sie Datenträger an einen virtuellen Linux-Computer bereitstellen und anfügen. Eine ausführliche Anleitung, wie Sie einen leeren Datenträger an einen virtuellen Linux-Computer in Azure anfügen, finden Sie im Microsoft Azure-Artikel [Anfügen eines Datenträgers](virtual-machines-linux-add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) .

## <a name="install-the-lvm-utilities"></a>Installieren der LVM-Dienstprogramme
* **Ubuntu**

    ```bash  
    sudo apt-get update
    sudo apt-get install lvm2
    ```

* **RHEL, CentOS und Oracle Linux**

    ```bash   
    sudo yum install lvm2
    ```

* **SLES 12 und openSUSE**

    ```bash   
    sudo zypper install lvm2
    ```

* **SLES 11**

    ```bash   
    sudo zypper install lvm2
    ```

    In SLES11 müssen Sie auch `/etc/sysconfig/lvm` bearbeiten und `LVM_ACTIVATED_ON_DISCOVERED` auf „enable“ festlegen:

    ```sh   
    LVM_ACTIVATED_ON_DISCOVERED="enable" 
    ```

## <a name="configure-lvm"></a>Konfigurieren von LVM
In diesem Handbuch wird davon ausgegangen, dass Sie über drei angefügte Datenträger verfügen, die als `/dev/sdc`, `/dev/sdd` und `/dev/sde` bezeichnet werden. Beachten Sie, dass diese möglicherweise nicht immer die gleichen Pfadnamen auf Ihrem virtuellen Computer aufweisen. Sie können „`sudo fdisk -l`“ oder einen ähnlichen Befehl zum Auflisten der verfügbaren Datenträger verwenden.

1. Vorbereiten der physischen Volumes:

    ```bash    
    sudo pvcreate /dev/sd[cde]
    Physical volume "/dev/sdc" successfully created
    Physical volume "/dev/sdd" successfully created
    Physical volume "/dev/sde" successfully created
    ```

2. Erstellen Sie eine Volumegruppe. In diesem Beispiel lautet der Name der Volumegruppe `data-vg01`:

    ```bash    
    sudo vgcreate data-vg01 /dev/sd[cde]
    Volume group "data-vg01" successfully created
    ```

3. Erstellen Sie die logischen Volumes. Mit dem Befehl unten wird ein einzelnes logisches Volume mit dem Namen `data-lv01` erstellt, das die gesamte Volumegruppe umfasst. Sie können aber auch mehrere logische Volumes in der Volumegruppe erstellen.

    ```bash   
    sudo lvcreate --extents 100%FREE --stripes 3 --name data-lv01 data-vg01
    Logical volume "data-lv01" created.
    ```

4. Formatieren Sie die logischen Volumes.

    ```bash  
    sudo mkfs -t ext4 /dev/data-vg01/data-lv01
    ```
   
   > [!NOTE]
   > Verwenden Sie in SLES11 `-t ext3` anstelle von „ext4“. SLES11 unterstützt nur schreibgeschützten Zugriff auf ext4-Dateisysteme.

## <a name="add-the-new-file-system-to-etcfstab"></a>Hinzufügen des neuen Laufwerks zu "/etc/fstab"
> [!IMPORTANT]
> Eine fehlerhafte Bearbeitung der Datei `/etc/fstab` könnte zu einem nicht startfähigen System führen. Wenn Sie sich nicht sicher sind, finden Sie in der Dokumentation der Verteilung Informationen zur korrekten Bearbeitung dieser Datei. Außerdem wird empfohlen, ein Backup der Datei `/etc/fstab` zu erstellen, bevor Sie sie bearbeiten.

1. Erstellen Sie den gewünschten Bereitstellungspunkt für das neue Dateisystem, zum Beispiel:

    ```bash  
    sudo mkdir /data
    ```

2. Ermitteln des Pfads für die logischen Volumes

    ```bash    
    lvdisplay
    --- Logical volume ---
    LV Path                /dev/data-vg01/data-lv01
    ....
    ```

3. Öffnen Sie `/etc/fstab` in einem Texteditor, und fügen Sie einen Eintrag für das neue Dateisystem hinzu, z.B.:

    ```bash    
    /dev/data-vg01/data-lv01  /data  ext4  defaults  0  2
    ```   
    Speichern und schließen Sie `/etc/fstab`.

4. Testen Sie, ob der Eintrag für `/etc/fstab` richtig ist:

    ```bash    
    sudo mount -a
    ```

    Wenn dieser Befehl zu einer Fehlermeldung führt, überprüfen Sie die Syntax in der Datei `/etc/fstab`.
   
    Führen Sie nun den Befehl `mount` aus, um sicherzustellen, dass das Dateisystem eingebunden wurde:

    ```bash    
    mount
    ......
    /dev/mapper/data--vg01-data--lv01 on /data type ext4 (rw)
    ```

5. (Optional) Failsafe-Startparameter in `/etc/fstab`
   
    Viele Distributionen enthalten die Bereitstellungsparameter `nobootwait` oder `nofail`, die der Datei `/etc/fstab` hinzugefügt werden können. Diese Parameter lassen Fehler beim Bereitstellen eines bestimmten Dateisystems zu und ermöglichen dem Linux-System das Fortsetzen des Startvorgangs, selbst wenn das RAID-Dateisystem nicht ordnungsgemäß geladen werden kann. Weitere Informationen zu diesen Parametern erhalten Sie in der Dokumentation der Distribution.
   
    Beispiel (Ubuntu):

    ```bash 
    /dev/data-vg01/data-lv01  /data  ext4  defaults,nobootwait  0  2
    ```

## <a name="trimunmap-support"></a>TRIM/UNMAP-Unterstützung
Einige Linux-Kernels unterstützen TRIM/UNMAP-Vorgänge, um ungenutzte Blöcke auf dem Datenträger zu verwerfen. Diese Vorgänge sind in erster Linie für Standardspeicher nützlich, um Azure darüber zu informieren, dass gelöschte Seiten nicht mehr gültig sind und verworfen werden können. Das Verwerfen von Seiten kann Kosten sparen, wenn Sie große Dateien erstellen und diese dann löschen.

Es gibt zwei Methoden, TRIM-Unterstützung auf Ihrem virtuellen Linux-Computer zu aktivieren. Den empfohlenen Ansatz finden Sie wie üblich in Ihrer Distribution:

- Verwenden Sie die Bereitstellungsoption `discard` in `/etc/fstab`, z.B.:

    ```bash 
    /dev/data-vg01/data-lv01  /data  ext4  defaults,discard  0  2
    ```

- In einigen Fällen kann sich die Option `discard` auf die Leistung auswirken. Alternativ können Sie den Befehl `fstrim` manuell über die Befehlszeile ausführen oder ihn „crontab“ hinzufügen, um eine regelmäßige Ausführung zu erzielen:

    **Ubuntu**

    ```bash 
    # sudo apt-get install util-linux
    # sudo fstrim /datadrive
    ```

    **RHEL/CentOS**

    ```bash 
    # sudo yum install util-linux
    # sudo fstrim /datadrive
    ```



<!--HONumber=Dec16_HO1-->


