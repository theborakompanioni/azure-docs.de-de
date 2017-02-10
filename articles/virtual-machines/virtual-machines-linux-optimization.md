---
title: Optimieren virtueller Linux-Computer in Azure | Microsoft Docs
description: Hier finden Sie einige Optimierungstipps, um mit virtuellen Linux-Computern in Azure eine optimale Leistung zu erzielen.
keywords: virtuelle Linux-Maschine, virtuelle Maschine Linux, virtuelle Ubuntu-Maschine
services: virtual-machines-linux
documentationcenter: 
author: rickstercdn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 8baa30c8-d40e-41ac-93d0-74e96fe18d4c
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 09/06/2016
ms.author: rclaus
translationtype: Human Translation
ms.sourcegitcommit: ee34a7ebd48879448e126c1c9c46c751e477c406
ms.openlocfilehash: ca1427e993416db09e7ff1603cbbf419db71f046


---
# <a name="optimize-your-linux-vm-on-azure"></a>Optimieren virtueller Linux-Computer in Azure
Virtuelle Linux-Maschinen (VM) lassen sich einfach über die Befehlszeile oder über das Portal erstellen. In diesem Tutorial erfahren Sie, wie Sie mit virtuellen Computern im Rahmen der Microsoft Azure Platform optimale Ergebnisse erzielen. In diesem Thema wird eine Ubuntu Server-VM verwendet, aber Sie können virtuelle Linux-Computer auch mithilfe [Ihrer eigenen Images als Vorlagen](virtual-machines-linux-create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)erstellen.  

## <a name="prerequisites"></a>Voraussetzungen
Dieses Thema setzt voraus, dass Sie bereits über ein funktionierendes Azure-Abonnement verfügen ([Anmeldung für eine kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/)) sowie [die Azure-Befehlszeilenschnittstelle installiert](../xplat-cli-install.md) und einen virtuellen Computer in Ihrem Azure-Abonnement bereitgestellt haben. Vor der Verwendung von Azure müssen Sie sich zunächst bei Ihrem Abonnement authentifizieren. Geben Sie hierzu über die Azure-Befehlszeilenschnittstelle einfach `azure login` ein, um den interaktiven Prozess zu starten. 

## <a name="azure-os-disk"></a>Betriebssystem-Datenträger von Azure
Nach dem Erstellen eines virtuellen Linux-Computers in Azure sind diesem zwei Datenträger zugeordnet: „/dev/sda“ (der Betriebssystem-Datenträger) und „/dev/sdb“ (der temporäre Datenträger). Verwenden Sie den Betriebssystem-Hauptdatenträger (/dev/sda) ausschließlich für das Betriebssystem.  Er ist für schnelles Starten des virtuellen Computers optimiert eignet sich nicht besonders für Ihre Workloads. Es empfiehlt sich, dem virtuellen Computer mindestens einen Datenträger anzufügen, um eine beständige und optimierte Datenspeicherung zu erhalten. 

## <a name="adding-disks-for-size-and-performance-targets"></a>Hinzufügen von Datenträgern für Größe und Leistung
Abhängig von der VM-Größe können Sie bis zu 16 zusätzliche Datenträger (A-Serie), 32 Datenträger (D-Serie) bzw. 64 Datenträger (G-Serie) anfügen, die jeweils eine Größe von bis zu 1 TB haben können. Orientieren Sie sich beim Hinzufügen zusätzlicher Datenträger an Ihren Platz- und IOPS-Anforderungen. Jeder Datenträger hat ein Leistungsziel von 500 IOPS (Storage Standard) bzw. von bis zu 5000 IOPS (Storage Premium).  Weitere Informationen zu Storage Premium-Datenträgern finden Sie unter [Premium Storage: Leistungsfähiger Speicher für Azure VMs](../storage/storage-premium-storage.md).

Um bei Storage Premium-Datenträgern mit der Cacheeinstellung „ReadOnly“ oder „None“ die höchstmögliche IOPS-Leistung zu erzielen, müssen so genannte „Barriers“ beim Einbinden des Dateisystems in Linux deaktiviert werden. „Barriers“ werden nicht benötigt, da Schreibvorgänge auf Storage Premium-Datenträger bei diesen Cacheeinstellungen beständig sind.

* Wenn Sie **reiserFS**verwenden, deaktivieren Sie Sperren mithilfe der Bereitstellungsoption „barrier=none“ (verwenden Sie zum Aktivieren von Sperren „barrier=flush“).
* Wenn Sie **ext3/ext4**verwenden, deaktivieren Sie Sperren mithilfe der Bereitstellungsoption „barrier=0“ (verwenden Sie zum Aktivieren von Sperren „barrier=1“).
* Wenn Sie **XFS**verwenden, deaktivieren Sie Sperren mithilfe der Bereitstellungsoption „nobarrier“ (verwenden Sie zum Aktivieren von Sperren „barrier“).

## <a name="storage-account-considerations"></a>Überlegungen zum Speicherkonto
Wenn Sie den virtuellen Linux-Computer in Azure erstellen, empfiehlt es sich, Datenträger aus Speicherkonten anzufügen, die sich in der gleichen Region befinden wie Ihr virtueller Computer, um weite Strecken zu vermeiden und die Netzwerklatenz zu minimieren.  Die Kapazität jedes Storage Standard-Kontos ist auf maximal 20.000 IOPS und eine Größe von 500 TB beschränkt.  Dies entspricht etwa 40 stark ausgelasteten Datenträgern (einschließlich Betriebssystem-Datenträger und aller von Ihnen erstellten Datenträger). Bei Storage Premium-Konten gilt kein IOPS-Limit, die Größe ist jedoch auf 32 TB beschränkt. 

Wenn Sie sehr IOPS-intensive Workloads verarbeiten müssen und sich für Datenträger des Typs „Standardspeicher“ entschieden haben, müssen Sie die Datenträger ggf. auf mehrere Speicherkonten aufteilen, um zu verhindern, dass Sie das Limit von 20.000 IOPS erreichen. Der virtuelle Computer kann eine Mischung aus Datenträgern verschiedener Speicherkonten und Speicherkontotypen enthalten, um eine optimale Konfiguration zu erreichen. 

## <a name="your-vm-temporary-drive"></a>Temporäres Laufwerk des virtuellen Computers
Beim Erstellen eines virtuellen Computers stellt Azure standardmäßig einen Betriebssystem-Datenträger (/dev/sda) und einen temporären Datenträger (/dev/sdb) bereit.  Alle weiteren Datenträger, die Sie hinzufügen, werden als „/dev/sdc“, „/dev/sdd“, „/dev/sde“ usw. angezeigt. Die Daten auf dem temporären Datenträger (/dev/sdb) sind nicht beständig und können in bestimmten Fällen verloren gehen – etwa, wenn die Größe des virtuellen Computers geändert oder der virtuelle Computer neu bereitgestellt oder aufgrund von Wartungsarbeiten neu gestartet wird.  Größe und Typ des temporären Datenträgers hängen mit der Größe des virtuellen Computers zusammen, die Sie zum Zeitpunkt der Bereitstellung ausgewählt haben. Im Falle eines virtuellen Premium-Computers (DS-, G- und DS_V2-Serie) wird das temporäre Laufwerk durch eine lokale SSD unterstützt, um eine höhere Leistung von bis zu 48.000 IOPS zu erzielen. 

## <a name="linux-swap-file"></a>Linux-Auslagerungsdatei
Wenn Ihre Azure-VM von einem Ubuntu- oder CoreOS-Image erstellt wurde, können Sie mit CustomData eine Cloud-Config-Datei an Cloud-Init senden. Wenn Sie [ein benutzerdefiniertes Linux-Image hochgeladen haben](virtual-machines-linux-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), das cloud-init verwendet, können Sie auch Swap-Partitionen konfigurieren, die cloud-init verwenden.

Für Ubuntu-Cloud-Images müssen Sie cloud-init verwenden, um die Swap-Partition zu konfigurieren. Weitere Details finden Sie auf der folgenden Seite im Ubuntu-Wiki: [AzureSwapPartitions](https://wiki.ubuntu.com/AzureSwapPartitions).

Über den Azure Marketplace bereitgestellte VM-Images verfügen für Images ohne cloud-init-Unterstützung über einen in das Betriebssystem integrierten VM-Linux-Agent. Dieser Agent ermöglicht dem virtuellen Computer die Interaktion mit verschiedenen Azure-Diensten. Falls Sie ein Standardimage aus dem Azure Marketplace bereitgestellt haben, gehen Sie wie folgt vor, um die Einstellungen für die Linux-Auslagerungsdatei korrekt zu konfigurieren:

Ändern Sie in der Datei **/etc/waagent.conf** zwei Einträge. Diese steuern, ob eine dedizierte Auslagerungsdatei vorhanden ist und welche Größe sie besitzt. `ResourceDisk.EnableSwap=N` und `ResourceDisk.SwapSizeMB=0` sind die Parameter, die Sie ändern möchten. 

Sie müssen sie wie folgt ändern:

* ResourceDisk.EnableSwap=Y
* ResourceDisk.SwapSizeMB={Größe in MB entsprechend Ihren Anforderungen} 

Nachdem Sie die Änderung vorgenommen haben, müssen Sie den Agent (waagent) oder den virtuellen Linux-Computer neu starten, damit diese Änderungen übernommen werden.  Zeigen Sie mithilfe des Befehls `free` den freien Speicherplatz an, um sich zu vergewissern, dass die Änderungen umgesetzt wurden und eine Auslagerungsdatei erstellt wurde. Im folgenden Beispiel wurde infolge der Änderung der Datei „waagent.conf“ eine Auslagerungsdatei mit einer Größe von 512 MB erstellt:

    admin@mylinuxvm:~$ free
                total       used       free     shared    buffers     cached
    Mem:       3525156     804168    2720988        408       8428     633192
    -/+ buffers/cache:     162548    3362608
    Swap:       524284          0     524284
    admin@mylinuxvm:~$

## <a name="io-scheduling-algorithm-for-premium-storage"></a>E/A-Scheduling-Algorithmus für Storage Premium
Mit Version 2.6.18 des Linux-Kernels wurde der standardmäßige E/A-Scheduling-Algorithmus von „Deadline“ in „CFQ“ (Completely Fair Queuing) geändert. Bei wahlfreien E/A-Zugriffsmustern sind die Leistungsunterschiede zwischen „CFQ“ und „Deadline“ vernachlässigbar.  Bei SSD-basierten Datenträgern, bei denen vorwiegend ein sequenzielles E/A-Muster verwendet wird, lässt sich durch einen Wechsel zum zuvor verwendeten NOOP- oder Deadline-Algorithmus unter Umständen eine bessere E/A-Leistung erzielen.

### <a name="view-the-current-io-scheduler"></a>Anzeigen des aktuellen E/A-Schedulers
Verwenden Sie den folgenden Befehl:  

    admin@mylinuxvm:~# cat /sys/block/sda/queue/scheduler

Sie sehen die folgende Ausgabe; diese gibt den aktuellen Scheduler an.  

    noop [deadline] cfq

### <a name="change-the-current-device-devsda-of-io-scheduling-algorithm"></a>Ändern des aktuellen Geräts (/dev/sda) des E/A-Scheduling-Algorithmus
Verwenden Sie die folgenden Befehle:  

    azureuser@mylinuxvm:~$ sudo su -
    root@mylinuxvm:~# echo "noop" >/sys/block/sda/queue/scheduler
    root@mylinuxvm:~# sed -i 's/GRUB_CMDLINE_LINUX=""/GRUB_CMDLINE_LINUX_DEFAULT="quiet splash elevator=noop"/g' /etc/default/grub
    root@mylinuxvm:~# update-grub

> [!NOTE]
> Nur für /dev/sda ist diese Einstellung nicht sinnvoll. Sie muss für alle Datenträger festgelegt werden, bei denen vorwiegend ein sequenzielles E/A-Muster verwendet wird.  
> 
> 

Die folgende Ausgabe zeigt an, dass grub.cfg erfolgreich neu erstellt wurde und dass der Standard-Scheduler zu NOOP aktualisiert wurde.  

    Generating grub configuration file ...
    Found linux image: /boot/vmlinuz-3.13.0-34-generic
    Found initrd image: /boot/initrd.img-3.13.0-34-generic
    Found linux image: /boot/vmlinuz-3.13.0-32-generic
    Found initrd image: /boot/initrd.img-3.13.0-32-generic
    Found memtest86+ image: /memtest86+.elf
    Found memtest86+ image: /memtest86+.bin
    done

Bei Redhat-Verteilungen benötigen Sie nur den folgenden Befehl:   

    echo 'echo noop >/sys/block/sda/queue/scheduler' >> /etc/rc.local

## <a name="using-software-raid-to-achieve-higher-iops"></a>Erzielen höherer IOPS-Werte mithilfe von Software-RAID
Wenn Ihre Workloads mehr IOPS erfordern als ein einzelner Datenträger bereitstellen kann, müssen Sie eine Software-RAID-Konfiguration mit mehreren Datenträgern verwenden. Da Azure bereits Datenträgerresilienz auf der lokalen Fabric-Ebene bietet, erreichen Sie die höchste Leistung mit einer RAID-0-Konfiguration.  Sie müssen in der Azure-Umgebung neue Datenträger bereitstellen/erstellen und sie an den virtuellen Linux-Computer anfügen, bevor Sie die Laufwerke partitionieren, formatieren und einbinden.  Ausführlichere Informationen zum Konfigurieren einer Software-RAID-Einrichtung für den virtuellen Linux-Computer in Azure finden Sie unter **[Konfigurieren von Software-RAID unter Linux](virtual-machines-linux-configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**.

## <a name="next-steps"></a>Nächste Schritte
Wie bei allen Optimierungen müssen auch hier vor und nach jeder Änderung Tests ausgeführt werden, um die Auswirkungen der jeweiligen Änderung zu ermitteln.  Die Optimierung ist ein schrittweiser Prozess, der auf unterschiedlichen Computern in Ihrer Umgebung unterschiedliche Ergebnisse liefert.  Was bei einer Konfiguration funktioniert, ist für andere möglicherweise ungeeignet.

Einige nützliche Links zu weiteren Ressourcen: 

* [Premium-Speicher: Hochleistungsspeicher für Workloads in Azure Virtual Machine](../storage/storage-premium-storage.md)
* [Benutzerhandbuch für Azure Linux-Agent](virtual-machines-linux-agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Optimieren der MySQL-Leistung auf virtuellen Azure Linux-Computern](virtual-machines-linux-classic-optimize-mysql.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Konfigurieren von Software-RAID unter Linux](virtual-machines-linux-configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)




<!--HONumber=Nov16_HO3-->


