---
title: "Hinzufügen eines Datenträgers zu einem virtuellen Linux-Computer | Microsoft Docs"
description: "Erfahren Sie, wie Sie Ihrem virtuellen Linux-Computer einen persistenten Datenträger hinzufügen."
keywords: "virtueller Linux-Computer,Ressourcendatenträger hinzufügen"
services: virtual-machines-linux
documentationcenter: 
author: rickstercdn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 3005a066-7a84-4dc5-bdaa-574c75e6e411
ms.service: virtual-machines-linux
ms.topic: article
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.date: 02/02/2017
ms.author: rasquill
translationtype: Human Translation
ms.sourcegitcommit: 50a71382982256e98ec821fd63c95fbe5a767963
ms.openlocfilehash: 91f4ada749c3f37903a8757843b10060b73d95a2


---
# <a name="add-a-disk-to-a-linux-vm"></a>Hinzufügen eines Datenträgers zu einem virtuellen Linux-Computer
In diesem Artikel wird gezeigt, wie Sie einen persistenten Datenträger an den virtuellen Computer anfügen, um Ihre Daten beizubehalten, auch wenn der virtuelle Computer aufgrund einer Wartung oder Größenänderung neu bereitgestellt wird. Zum Hinzufügen eines Datenträgers benötigen Sie die im Resource Manager-Modus (`azure config mode arm`) konfigurierte [Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md) (CLI).  

## <a name="quick-commands"></a>Schnellbefehle
Im folgenden Beispiel wird ein `50`-GB-Datenträger dem virtuellen Computer mit dem Namen `myVM` in der Ressourcengruppe `myResourceGroup` angefügt.

Für verwaltete Datenträger:

```azurecli
az vm disk attach –g myResourceGroup –-vm-name myVM –-disk myDataDisk –-new
```

Für nicht verwaltete Datenträger:

```azurecli
azure vm disk attach-new myResourceGroup myVM 50
```

## <a name="attach-a-managed-disk"></a>Anfügen eines verwalteten Datenträgers

Durch die Verwendung von verwalteten Datenträgern können Sie sich auf Ihre virtuellen Computer und die zugehörigen Datenträger konzentrieren und müssen sich nicht um Azure Storage-Konten kümmern. Sie können einen verwalteten Datenträger schnell erstellen und an einen virtuellen Computer anfügen, indem Sie die gleiche Azure-Ressourcengruppe verwenden. Alternativ dazu können Sie eine beliebige Anzahl von Datenträgern erstellen und diese dann anfügen.


### <a name="attach-a-new-disk-to-a-vm"></a>Anfügen eines neuen Datenträgers an einen virtuellen Computer

Wenn Sie nur einen neuen Datenträger für Ihren virtuellen Computer benötigen, können Sie den Befehl `az vm disk attach` verwenden.

```azurecli
az vm disk attach –g myResourceGroup –-vm-name myVM –-disk myDataDisk –-new
```

### <a name="attach-an-existing-disk"></a>Anfügen eines vorhandenen Datenträgers 

In vielen Fällen fügen Sie Datenträger an, die bereits erstellt wurden. Hierbei müssen Sie die Datenträger-ID finden und diese an den Befehl `az vm disk attach-disk` übergeben. Der folgende Code verwendet einen Datenträger, der mit `az disk create -g myResourceGroup -n myDataDisk --size-gb 50` erstellt wurde.

```azurecli
# find the disk id
diskId=$(az disk show -g myResourceGroup -n myDataDisk --query 'id' -o tsv)
az vm disk attach-disk -g myResourceGroup --vm-name myVM --disk $diskId
```

Die Ausgabe sieht in etwa wie folgt aus (Sie können die Option `-o table` bei jedem Befehl zum Formatieren der Ausgabe verwenden):

```json
{
  "accountType": "Standard_LRS",
  "creationData": {
    "createOption": "Empty",
    "imageReference": null,
    "sourceResourceId": null,
    "sourceUri": null,
    "storageAccountId": null
  },
  "diskSizeGb": 50,
  "encryptionSettings": null,
  "id": "/subscriptions/<guid>/resourceGroups/rasquill-script/providers/Microsoft.Compute/disks/myDataDisk",
  "location": "westus",
  "name": "myDataDisk",
  "osType": null,
  "ownerId": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "tags": null,
  "timeCreated": "2017-02-02T23:35:47.708082+00:00",
  "type": "Microsoft.Compute/disks"
}
```


## <a name="attach-an-unmanaged-disk"></a>Anfügen eines nicht verwalteten Datenträgers

Das Anfügen eines neuen Datenträgers lässt sich schnell erledigen, wenn Sie den Datenträger im gleichen Speicherkonto erstellen können, in dem sich auch Ihr virtueller Computer befindet. Geben Sie `azure vm disk attach-new` ein, um eine neue GB-Festplatte für den virtuellen Computer zu erstellen und anzufügen. Wenn Sie kein Speicherkonto explizit angeben, werden alle von Ihnen erstellten Datenträger im gleichen Speicherkonto platziert, in dem sich auch der Betriebssystemdatenträger befindet. Im folgenden Beispiel wird ein `50`-GB-Datenträger dem virtuellen Computer mit dem Namen `myVM` in der Ressourcengruppe `myResourceGroup` angefügt.

```azurecli
azure vm disk attach-new myResourceGroup myVM 50
```

Ausgabe

```azurecli
info:    Executing command vm disk attach-new
+ Looking up the VM "myVM"
info:    New data disk location: https://mystorageaccount.blob.core.windows.net/vhds/myVM-20150526-043.vhd
+ Updating VM "myVM"
info:    vm disk attach-new command OK
```

## <a name="connect-to-the-linux-vm-to-mount-the-new-disk"></a>Herstellen einer Verbindung mit dem virtuellen Linux-Computer zum Bereitstellen des neuen Datenträgers
> [!NOTE]
> In diesem Thema wird mit Benutzernamen und Kennwörtern eine Verbindung mit einer VM hergestellt. Informationen zur Verwendung von öffentlichen und privaten Schlüsselpaaren für die Kommunikation mit Ihrer VM finden Sie unter [Verwenden von SSH mit Linux in Azure](virtual-machines-linux-mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Sie können die **SSH**-Verbindung von VMs ändern, die mit dem Befehl `azure vm quick-create` erstellt wurden, indem Sie den Befehl `azure vm reset-access` zum vollständigen Zurücksetzen des **SSH**-Zugriffs verwenden, Benutzer hinzufügen oder entfernen oder Dateien für öffentliche Schlüssel zum Sichern des Zugriffs hinzufügen.
> 
> 

Sie benötigen SSH für Ihren virtuellen Azure-Computer, um den neuen Datenträger zu partitionieren, zu formatieren und bereitzustellen, damit er von Ihrem virtuellen Linux-Computer verwendet werden kann. Falls Sie mit dem Herstellen einer Verbindung über **SSH** nicht vertraut sind: Der Befehl hat das Format `ssh <username>@<FQDNofAzureVM> -p <the ssh port>` und sieht wie folgt aus:

```bash
ssh ops@mypublicdns.westus.cloudapp.azure.com -p 22
```

Ausgabe

```bash
The authenticity of host 'mypublicdns.westus.cloudapp.azure.com (191.239.51.1)' can't be established.
ECDSA key fingerprint is bx:xx:xx:xx:xx:xx:xx:xx:xx:x:x:x:x:x:x:xx.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'mypublicdns.westus.cloudapp.azure.com,191.239.51.1' (ECDSA) to the list of known hosts.
ops@mypublicdns.westus.cloudapp.azure.com's password:
Welcome to Ubuntu 14.04.2 LTS (GNU/Linux 3.16.0-37-generic x86_64)

* Documentation:  https://help.ubuntu.com/

System information as of Fri May 22 21:02:32 UTC 2015

System load: 0.37              Memory usage: 2%   Processes:       207
Usage of /:  41.4% of 1.94GB   Swap usage:   0%   Users logged in: 0

Graph this data and manage this system at:
  https://landscape.canonical.com/

Get cloud support with Ubuntu Advantage Cloud Guest:
  http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

ops@myVM:~$
```

Nachdem Sie die Verbindung zu Ihrem virtuellen Computer hergestellt haben, können Sie einen Datenträger anfügen.  Suchen Sie zunächst den Datenträger mithilfe von `dmesg | grep SCSI` (die Methode zum Ermitteln Ihres neuen Datenträgers kann anders sein). In diesem Fall sieht er in etwa so aus:

```bash
dmesg | grep SCSI
```

Ausgabe

```bash
[    0.294784] SCSI subsystem initialized
[    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
[    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
[    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
[ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
```

Wir verwenden den Datenträger `sdc`. Partitionieren Sie nun den Datenträger mit `sudo fdisk /dev/sdc` – wir gehen davon aus, dass der Datenträger `sdc` war. Legen Sie ihn auf Partition 1 als primären Datenträger fest, und übernehmen Sie die anderen Standardwerte.

```bash
sudo fdisk /dev/sdc
```

Ausgabe

```bash
Device contains neither a valid DOS partition table, nor Sun, SGI or OSF disklabel
Building a new DOS disklabel with disk identifier 0x2a59b123.
Changes will remain in memory only, until you decide to write them.
After that, of course, the previous content won't be recoverable.

Warning: invalid flag 0x0000 of partition table 4 will be corrected by w(rite)

Command (m for help): n
Partition type:
   p   primary (0 primary, 0 extended, 4 free)
   e   extended
Select (default p): p
Partition number (1-4, default 1): 1
First sector (2048-10485759, default 2048):
Using default value 2048
Last sector, +sectors or +size{K,M,G} (2048-10485759, default 10485759):
Using default value 10485759
```

Erstellen Sie die Partition durch Eingabe von `p` an der Eingabeaufforderung:

```bash
Command (m for help): p

Disk /dev/sdc: 5368 MB, 5368709120 bytes
255 heads, 63 sectors/track, 652 cylinders, total 10485760 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x2a59b123

   Device Boot      Start         End      Blocks   Id  System
/dev/sdc1            2048    10485759     5241856   83  Linux

Command (m for help): w
The partition table has been altered!

Calling ioctl() to re-read partition table.
Syncing disks.
```

Schreiben Sie zudem mithilfe des Befehls **mkfs** ein Dateisystem für die Partition. Geben Sie dabei Ihren Dateisystemtyp und den Gerätenamen an. In diesem Thema verwenden wir `ext4` und `/dev/sdc1` von oben:

```bash
sudo mkfs -t ext4 /dev/sdc1
```

Ausgabe

```bash
mke2fs 1.42.9 (4-Feb-2014)
Discarding device blocks: done
Filesystem label=
OS type: Linux
Block size=4096 (log=2)
Fragment size=4096 (log=2)
Stride=0 blocks, Stripe width=0 blocks
327680 inodes, 1310464 blocks
65523 blocks (5.00%) reserved for the super user
First data block=0
Maximum filesystem blocks=1342177280
40 block groups
32768 blocks per group, 32768 fragments per group
8192 inodes per group
Superblock backups stored on blocks:
    32768, 98304, 163840, 229376, 294912, 819200, 884736
Allocating group tables: done
Writing inode tables: done
Creating journal (32768 blocks): done
Writing superblocks and filesystem accounting information: done
```

Erstellen Sie mit `mkdir`ein Verzeichnis zum Bereitstellen des neuen Dateisystems.

```bash
sudo mkdir /datadrive
```

Stellen Sie das Verzeichnis mit `mount`bereit:

```bash
sudo mount /dev/sdc1 /datadrive
```

Der Datenträger kann nun als `/datadrive`verwendet werden.

```bash
ls
```

Ausgabe

```bash
bin   datadrive  etc   initrd.img  lib64       media  opt   root  sbin  sys  usr  vmlinuz
boot  dev        home  lib         lost+found  mnt    proc  run   srv   tmp  var
```

Um sicherzustellen, dass das Laufwerk nach einem Neustart automatisch wieder eingebunden wird, muss es der Datei „/etc/fstab“ hinzugefügt werden. Außerdem wird dringend empfohlen, den UUID (Universally Unique IDentifier) in „/etc/fstab“ zu verwenden, um auf das Laufwerk und nicht auf den Gerätenamen (z.B. `/dev/sdc1`) zu verweisen. Wenn das Betriebssystem während des Starts einen Datenträgerfehler erkennt, wird durch den UUID verhindert, dass an einem bestimmten Ort der falsche Datenträger bereitgestellt wird. Die verbleibenden Datenträger werden dann diesen Geräte-IDs zugewiesen. Verwenden Sie das Hilfsprogramm **blkid**, um den UUID des neuen Laufwerks herauszufinden:

```bash
sudo -i blkid
```

Die Ausgabe sieht in etwa wie folgt aus:

```bash
/dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
/dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
/dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
```

> [!NOTE]
> Eine falsche Bearbeitung der Datei **/etc/fstab** könnte zu einem nicht startfähigen System führen. Wenn Sie sich nicht sicher sind, helfen Ihnen die Informationen zur richtigen Bearbeitung dieser Datei in der Dokumentation weiter. Außerdem wird empfohlen, ein Backup der Datei /etc/fstab zu erstellen, bevor Sie sie bearbeiten.
> 
> 

Öffnen Sie als Nächstes die Datei **/etc/fstab** in einem Text-Editor:

```bash
sudo vi /etc/fstab
```

In diesem Beispiel verwenden wir den UUID-Wert für das neue Gerät **/dev/sdc1**, das in den vorherigen Schritten erstellt wurde, und den Bereitstellungspunkt **/datadrive**. Fügen Sie am Ende der Datei **/etc/fstab** die folgende Zeile hinzu:

```bash
UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,nofail   1   2
```

> [!NOTE]
> Wenn Sie später einen Datenträger entfernen, ohne „fstab“ zu bearbeiten, kann der Start des virtuellen Computers fehlschlagen. Bei den meisten Distributionen wird entweder die fstab-Option `nofail` oder `nobootwait` bereitgestellt (bzw. auch beide). Mit diesen Optionen kann ein System auch dann starten, wenn der Datenträger beim Start nicht bereitgestellt wird. Weitere Informationen zu diesen Parametern finden Sie in der Dokumentation zu Ihrer Distribution.
> 
> Die **nofail**-Option stellt sicher, dass der virtuelle Computer gestartet wird, selbst wenn das Dateisystem beschädigt oder der Datenträger zur Startzeit nicht vorhanden ist. Ohne diese Option können Verhalten auftreten, die unter [Cannot SSH to Linux VM due to FSTAB errors](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/) (Aufgrund von FSTAB-Fehler keine SSH-Verbindung mit Linux-VM möglich) beschrieben sind.

### <a name="trimunmap-support-for-linux-in-azure"></a>TRIM/UNMAP-Unterstützung für Linux in Azure
Einige Linux-Kernels unterstützen TRIM/UNMAP-Vorgänge, um ungenutzte Blöcke auf dem Datenträger zu verwerfen. Dies ist in erster Linie für Standardspeicher nützlich, um Azure darüber zu informieren, dass gelöschte Seiten nicht mehr gültig sind und verworfen werden können. Dies kann Kosten sparen, wenn Sie große Dateien erstellen und diese dann löschen.

Es gibt zwei Methoden, TRIM-Unterstützung auf Ihrem virtuellen Linux-Computer zu aktivieren. Den empfohlenen Ansatz finden Sie wie üblich in Ihrer Distribution:

* Verwenden Sie die Bereitstellungsoption `discard` in `/etc/fstab`, z.B.:

    ```bash
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
* Beachten Sie, dass der neue Datenträger bei einem Neustart nicht für den virtuellen Computer zur Verfügung steht, sofern Sie diese Informationen nicht in Ihre [fstab-Datei](http://en.wikipedia.org/wiki/Fstab) geschrieben haben.
* Lesen Sie die Empfehlungen unter [Optimieren virtueller Linux-Computer in Azure](virtual-machines-linux-optimization.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) , um sicherzustellen, dass Ihr virtueller Linux-Computer richtig konfiguriert ist.
* Erweitern Sie die Speicherkapazität durch Hinzufügen zusätzlicher Datenträger, und [konfigurieren Sie RAID](virtual-machines-linux-configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) für zusätzliche Leistung.




<!--HONumber=Feb17_HO2-->


