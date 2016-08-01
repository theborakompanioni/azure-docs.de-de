<properties
	pageTitle="Hinzufügen eines Datenträgers zu einem virtuellen Linux-Computer | Microsoft Azure"
	description="Erfahren Sie, wie Sie Ihrem virtuellen Linux-Computer einen persistenten Datenträger hinzufügen."
	keywords="virtueller Linux-Computer,Ressourcendatenträger hinzufügen"
	services="virtual-machines-linux"
	documentationCenter=""
	authors="rickstercdn"
	manager="timlt"
	editor="tysonn"
	tags="azure-resource-manager" />

<tags
	ms.service="virtual-machines-linux"
	ms.topic="article"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-linux"
	ms.devlang="na"
	ms.date="04/29/2016"
	ms.author="rclaus"/>

# Hinzufügen eines Datenträgers zu einem virtuellen Linux-Computer

In diesem Artikel wird gezeigt, wie Sie einen persistenten Datenträger an den virtuellen Computer anfügen, um Ihre Daten beizubehalten, auch wenn der virtuelle Computer aufgrund einer Wartung oder Größenänderung neu bereitgestellt wird. Zum Hinzufügen eines Datenträgers benötigen Sie die [Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md), die im Resource Manager-Modus (`azure config mode arm`) konfiguriert wurde.

## Schnellbefehle

Ersetzen Sie in den folgenden Befehlsbeispielen die Werte zwischen &lt; und &gt; durch die Werte aus Ihrer eigenen Umgebung.

```bash
azure vm disk attach-new <myuniquegroupname> <myuniquevmname> <size-in-GB>
```

## Anfügen eines Datenträgers

Neue Datenträger lassen sich schnell anfügen. Geben Sie einfach `azure vm disk attach-new <myuniquegroupname> <myuniquevmname> <size-in-GB>` ein, um eine neue GB-Festplatte für den virtuellen Computer zu erstellen und anzufügen. Wenn Sie kein Speicherkonto explizit angeben, werden alle von Ihnen erstellten Datenträger im gleichen Speicherkonto platziert, in dem sich auch der Betriebssystemdatenträger befindet. Das sollte in etwa so aussehen:

```bash
azure vm disk attach-new myuniquegroupname myuniquevmname 5
```

Ausgabe

```bash
info:    Executing command vm disk attach-new
+ Looking up the VM "myuniquevmname"
info:    New data disk location: https://cliexxx.blob.core.windows.net/vhds/myuniquevmname-20150526-0xxxxxxx43.vhd
+ Updating VM "myuniquevmname"
info:    vm disk attach-new command OK
```

## Herstellen einer Verbindung mit dem virtuellen Linux-Computer zum Bereitstellen des neuen Datenträgers

> [AZURE.NOTE] In diesem Thema wird eine Verbindung mit einem virtuellen Computer mithilfe von Benutzernamen und Kennwörtern hergestellt. Informationen zur Verwendung öffentlicher und privater Schlüsselpaare für die Kommunikation mit Ihrem virtuellen Computer finden Sie unter [Verwenden von SSH mit Linux auf Azure](virtual-machines-linux-ssh-from-linux.md). Sie können die **SSH**-Verbindung von VMs ändern, die mit dem Befehl `azure vm quick-create` erstellt wurden, indem Sie den Befehl `azure vm reset-access` zum vollständigen Zurücksetzen des **SSH**-Zugriffs verwenden, Benutzer hinzufügen oder entfernen oder Dateien für öffentliche Schlüssel zum Sichern des Zugriffs hinzufügen.

Sie benötigen SSH für Ihren virtuellen Azure-Computer, um den neuen Datenträger zu partitionieren, formatieren und bereitzustellen, damit er von Ihrem virtuellen Linux-Computer verwendet werden kann. Falls Sie mit dem Herstellen einer Verbindung über **SSH** nicht vertraut sind: Der Befehl hat das Format `ssh <username>@<FQDNofAzureVM> -p <the ssh port>` und sieht wie folgt aus:

```bash
ssh ops@myuni-westu-1432328437727-pip.westus.cloudapp.azure.com -p 22
```

Ausgabe

```bash
The authenticity of host 'myuni-westu-1432328437727-pip.westus.cloudapp.azure.com (191.239.51.1)' can't be established.
ECDSA key fingerprint is bx:xx:xx:xx:xx:xx:xx:xx:xx:x:x:x:x:x:x:xx.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'myuni-westu-1432328437727-pip.westus.cloudapp.azure.com,191.239.51.1' (ECDSA) to the list of known hosts.
ops@myuni-westu-1432328437727-pip.westus.cloudapp.azure.com's password:
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

ops@myuniquevmname:~$
```

Nachdem Sie die Verbindung zu Ihrem virtuellen Computer hergestellt haben, können Sie einen Datenträger anfügen. Suchen Sie zunächst den Datenträger mithilfe von `dmesg | grep SCSI` (die Methode zum Ermitteln Ihres neuen Datenträgers kann anders sein). In diesem Fall sieht er in etwa so aus:

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

Wir verwenden den Datenträger `sdc`. Partitionieren Sie nun den Datenträger mit `sudo fdisk /dev/sdc` – wir gehen davon aus, dass der Datenträger `sdc` war. Legen Sie ihn auf Partition 1 als primären Datenträger fest, und übernehmen Sie die anderen Standardwerte.

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

Erstellen Sie mit `mkdir` ein Verzeichnis zum Bereitstellen des neuen Dateisystems.

```bash
sudo mkdir /datadrive
```

Stellen Sie das Verzeichnis mit `mount` bereit:

```bash
sudo mount /dev/sdc1 /datadrive
```

Der Datenträger kann nun als `/datadrive` verwendet werden.

```bash
ls
```

Ausgabe

```bash
bin   datadrive  etc   initrd.img  lib64       media  opt   root  sbin  sys  usr  vmlinuz
boot  dev        home  lib         lost+found  mnt    proc  run   srv   tmp  var
```

> [AZURE.NOTE] Sie können die Verbindung zum virtuellen Linux-Computer auch mithilfe eines SSH-Schlüssels für die Identifikation herstellen. Weitere Informationen finden Sie unter [Verwenden von SSH mit Linux auf Azure](virtual-machines-linux-ssh-from-linux.md).


### TRIM/UNMAP-Unterstützung für Linux in Azure
Einige Linux-Kernels unterstützen TRIM/UNMAP-Vorgänge, um ungenutzte Blöcke auf dem Datenträger zu verwerfen. Dies ist in erster Linie für Standardspeicher nützlich, um Azure darüber zu informieren, dass gelöschte Seiten nicht mehr gültig sind und verworfen werden können. Dies kann Kosten sparen, wenn Sie große Dateien erstellen und diese dann löschen.

Es gibt zwei Methoden, TRIM-Unterstützung auf Ihrem virtuellen Linux-Computer zu aktivieren. Den empfohlenen Ansatz finden Sie wie üblich in Ihrer Distribution:

- Verwenden Sie die `discard`-Bereitstellungsoption in `/etc/fstab`, Beispiel:

		UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,discard   1   2

- Alternativ können Sie den Befehl `fstrim` manuell über die Befehlszeile ausführen oder zu „crontab“ für eine regelmäßige Ausführung hinzufügen:

	**Ubuntu**

		# sudo apt-get install util-linux
		# sudo fstrim /datadrive

	**RHEL/CentOS**

		# sudo yum install util-linux
		# sudo fstrim /datadrive


## Nächste Schritte

- Beachten Sie, dass der neue Datenträger bei einem Neustart in der Regel nicht für den virtuellen Computer zur Verfügung steht, es sei denn, Sie schreiben diese Informationen in Ihre [fstab-Datei](http://en.wikipedia.org/wiki/Fstab).
- Lesen Sie die Empfehlungen unter [Optimieren virtueller Linux-Computer in Azure](virtual-machines-linux-optimization.md), um sicherzustellen, dass Ihr virtueller Linux-Computer richtig konfiguriert ist.
- Erweitern Sie die Speicherkapazität durch Hinzufügen zusätzlicher Datenträger, und [konfigurieren Sie RAID](virtual-machines-linux-configure-raid.md) für zusätzliche Leistung.

<!---HONumber=AcomDC_0720_2016-->