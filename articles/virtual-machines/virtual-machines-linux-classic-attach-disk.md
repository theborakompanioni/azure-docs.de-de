<properties
	pageTitle="Anfügen eines Datenträgers an einen virtuellen Linux-Computer | Microsoft Azure"
	description="Sie erfahren, wie Sie einen Datenträger an einen virtuellen Azure-Computer unter Linux anfügen und initialisieren können, damit er betriebsbereit ist."
	services="virtual-machines-linux"
	documentationCenter=""
	authors="iainfoulds"
	manager="timlt"
	editor="tysonn"
	tags="azure-service-management"/>

<tags
	ms.service="virtual-machines-linux"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-linux"
	ms.devlang="na"
	ms.topic="article"
	ms.date="06/07/2016"
	ms.author="iainfou"/>

# Gewusst wie: Anfügen eines Datenträgers an einen virtuellen Linux-Computer

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]. So fügen Sie [einen Datenträger mithilfe des Resource Manager-Bereitstellungsmodell an](virtual-machines-linux-add-disk.md).

Sie können sowohl leere Datenträger, als auch Datenträger mit Daten an Ihre virtuellen Azure-Computer anfügen. Beide Arten von Datenträgern sind VHD-Dateien, die sich in einem Azure Storage-Konto befinden. Wie bei jedem Hinzufügen von Datenträgern an einen Linux-Computer, müssen Sie nach dem Anfügen des Datenträgers diesen initialisieren und formatieren, damit er verwendet werden kann. In diesem Artikel wird beschrieben, wie Sie sowohl leere Datenträger als auch Datenträger mit Daten an Ihre VMs anfügen, und wie Sie daraufhin einen neuen Datenträger initialisieren und formatieren.

> [AZURE.NOTE] Es empfiehlt sich, einen oder mehrere separate Datenträger zu verwenden, um die Daten eines virtuellen Computers zu speichern. Wenn Sie einen virtuellen Azure-Computer erstellen, verfügt dieser über einen Betriebssystem-Datenträger und über einen temporären Datenträger. **Verwenden Sie den temporären Datenträger nicht zum Speichern von persistenten Daten.** Wie der Name schon sagt, bietet dieser Datenträger nur temporäre Speicherung. Er ermöglicht keine Redundanz oder Sicherung, da er sich nicht im Azure-Speicher befindet. Unter Linux wird der temporäre Datenträger normalerweise vom Azure Linux Agent verwaltet und automatisch an **/mnt/resource** (oder auf Ubuntu-Images an **/mnt**) angefügt. Andererseits kann ein Datenträger vom Linux-Kernel beispielsweise den Namen `/dev/sdc` erhalten, und Sie müssen diese Ressource partitionieren, formatieren und einbinden. Weitere Informationen finden Sie im [Benutzerhandbuch für Azure Linux-Agent][Agent].

[AZURE.INCLUDE [howto-attach-disk-windows-linux](../../includes/howto-attach-disk-linux.md)]

## Initialisieren eines neues Datenträgers unter Linux

1. Stellen Sie mithilfe des SSH-Netzwerkprotokolls eine Verbindung zu Ihrer VM her. Weitere Informationen finden Sie unter [Anmelden bei einem mit Linux betriebenen virtuellen Computer][Logon].

2. Als Nächstes müssen Sie die Geräte-ID für den zu initialisierenden Datenträger finden. Dafür gibt zwei Möglichkeiten:

	a) Grep für SCSI-Geräte in den Protokollen, z.B. wie im nachfolgenden Befehl:

			$sudo grep SCSI /var/log/messages

	Für aktuelle Ubuntu-Verteilungen müssen Sie möglicherweise `sudo grep SCSI /var/log/syslog` verwenden, da die Anmeldung bei `/var/log/messages` möglicherweise standardmäßig deaktiviert ist.

	In den angezeigten Meldungen können Sie den Bezeichner des letzten hinzugefügten Datenträgers finden.

	![Datenträgermeldungen abrufen](./media/virtual-machines-linux-classic-attach-disk/scsidisklog.png)

	OR

	(b) Verwenden Sie den `lsscsi`-Befehl, um die Geräte-ID herauszufinden.`lsscsi` kann entweder über `yum install lsscsi` (für auf Red Hat basierende Distributionen) oder über `apt-get install lsscsi` (für auf Debian basierende Distributionen) installiert werden. Den gesuchten Datenträger finden Sie anhand seiner _LUN_ oder **logischen Gerätenummer**. Zum Beispiel kann die _LUN_ für die angefügten Datenträger leicht mit `azure vm disk list <virtual-machine>` angezeigt werden:

			~$ azure vm disk list TestVM
			info:    Executing command vm disk list
			+ Fetching disk images
			+ Getting virtual machines
			+ Getting VM disks
			data:    Lun  Size(GB)  Blob-Name                         OS
			data:    ---  --------  --------------------------------  -----
			data:         30        TestVM-2645b8030676c8f8.vhd  Linux
			data:    0    100       TestVM-76f7ee1ef0f6dddc.vhd
			info:    vm disk list command OK

	Vergleichen Sie dies mit der Ausgabe von `lsscsi` für denselben virtuellen Beispielcomputer:

			ops@TestVM:~$ lsscsi
			[1:0:0:0]    cd/dvd  Msft     Virtual CD/ROM   1.0   /dev/sr0
			[2:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sda
			[3:0:1:0]    disk    Msft     Virtual Disk     1.0   /dev/sdb
			[5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc

	Die letzte Zahl im Tupel in jeder Zeile ist die _LUN_. Weitere Informationen finden Sie unter `man lsscsi`.

3. Geben Sie an der Eingabeaufforderung den folgenden Befehl ein, um Ihr Gerät zu erstellen:

		$sudo fdisk /dev/sdc


4. Geben Sie nach Aufforderung **n** ein, um eine neue Partition zu erstellen.


	![Neues Gerät erstellen](./media/virtual-machines-linux-classic-attach-disk/fdisknewpartition.png)

5. Geben Sie nach Aufforderung **p** ein, um die Partition zur primären zu machen, geben Sie **1** ein, um sie zur ersten Partition zu machen, und drücken Sie die Eingabetaste, um den Standardwert für den Zylinder zu übernehmen. Bei einigen Systemen werden unter Umständen anstelle des Zylinders die Standardwerte des ersten und letzten Abschnitts angezeigt. Sie können diese Standardeinstellungen auch übernehmen.


	![Partition erstellen](./media/virtual-machines-linux-classic-attach-disk/fdisknewpartition.png)



6. Geben Sie **p** ein, um die Details zum Datenträger anzuzeigen, der gerade partitioniert wird.


	![Informationen zum Datenträger auflisten](./media/virtual-machines-linux-classic-attach-disk/fdisknewpartition.png)



7. Geben Sie **w** ein, um die Einstellungen für den Datenträger zu schreiben.


	![Datenträgeränderungen schreiben](./media/virtual-machines-linux-classic-attach-disk/fdiskwritedisk.png)

8. Sie können nun das Dateisystem auf der neuen Partition erstellen. Fügen Sie die Nummer der Partition an die Geräte-ID an (im folgenden Beispiel `/dev/sdc1`). Das folgende Beispiel erstellt eine „ext4“-Partition auf /dev/sdc1:

		# sudo mkfs -t ext4 /dev/sdc1

	![Dateisystem erstellen](./media/virtual-machines-linux-classic-attach-disk/mkfsext4.png)

	>[AZURE.NOTE] Beachten Sie, dass SUSE Linux Enterprise 11-Systeme nur den schreibgeschützten Zugriff für ext4-Dateisysteme unterstützen. Für diese Systeme wird empfohlen, das neue Dateisystem als ext3 statt als ext4 zu formatieren.


9. Erstellen Sie ein Verzeichnis zum Bereitstellen des neuen Dateisystems, wie nachfolgend beschrieben:

		# sudo mkdir /datadrive


10. Schließlich können Sie das Laufwerk wie folgt bereitstellen:

		# sudo mount /dev/sdc1 /datadrive

	Der Datenträger kann nun als **/datadrive** verwendet werden.
	
	![Erstellen des Verzeichnisses und Bereitstellen der Festplatte](./media/virtual-machines-linux-classic-attach-disk/mkdirandmount.png)


11. Fügen Sie das neue Laufwerk zu /etc/fstab hinzu:

	Um sicherzustellen, dass das Laufwerk nach einem Neustart automatisch wieder eingebunden wird, muss es zur Datei /etc/fstab hinzugefügt werden. Außerdem wird dringend empfohlen, den UUID (Universally Unique IDentifier) in /etc/fstab zu verwenden, um auf das Laufwerk und nicht auf den Gerätenamen (d.h. auf /dev/sdc1) zu verweisen. Dadurch wird vermieden, dass der falsche Datenträger an einem bestimmten Standort bereitgestellt wird, wenn das Betriebssystem einen Fehler während des Starts entdeckt, und alle verbleibenden Datenträger anschließend diesen Geräte-IDs zugewiesen werden. Sie können das Hilfsprogramm **blkid** verwenden, um den UUID des neuen Laufwerks herauszufinden:

		# sudo -i blkid

	Die Ausgabe sieht etwa wie folgt aus:

		/dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
		/dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
		/dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"


	>[AZURE.NOTE] Eine falsche Bearbeitung der Datei **/etc/fstab** könnte zu einem nicht startfähigen System führen. Wenn Sie sich nicht sicher sind, finden Sie in der Dokumentation der Verteilung Informationen zur korrekten Bearbeitung dieser Datei. Außerdem wird empfohlen, ein Backup der Datei /etc/fstab zu erstellen, bevor Sie sie bearbeiten.

	Öffnen Sie als Nächstes die Datei **/etc/fstab** in einem Texteditor:

		# sudo vi /etc/fstab

	In diesem Beispiel verwenden wir den UUID-Wert für das neue Gerät **/dev/sdc1**, das in den vorhergehenden Schritten erstellt wurde, und den Bereitstellungspunkt **/datadrive**. Fügen Sie am Ende der Datei **/etc/fstab** die folgende Zeile hinzu:

		UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults   1   2

	In auf SUSE Linux basierenden Systemen müssen Sie möglicherweise ein leicht anderes Format verwenden:

		/dev/disk/by-uuid/33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext3   defaults   1   2

	Sie können nun testen, ob das Dateisystem korrekt eingebunden wurde, indem Sie die Einbindung aufheben und wiederherstellen. Verwenden Sie dafür den zuvor erstellten Beispielbereitstellungspunkt `/datadrive`:

		# sudo umount /datadrive
		# sudo mount /datadrive

	Wenn der Befehl `mount` zu einem Fehler führt, prüfen Sie die Datei "/etc/fstab" auf korrekte Syntax. Wenn zusätzliche Datenlaufwerke oder Partitionen erstellt werden, müssen Sie diese ebenfalls einzeln in "/etc/fstab" einfügen.

	Das Laufwerk muss beschreibbar sein. Sie erreichen dies mit folgendem Befehl:

		# sudo chmod go+w /datadrive

>[AZURE.NOTE] Wenn Sie später einen Datenträger entfernen, ohne "fstab" zu bearbeiteten, kann der Start des virtuellen Computers fehlschlagen. Für den Fall, dass dieses Problem häufiger auftritt, bieten die meisten Verteilungen die fstab-Optionen `nofail` und/oder `nobootwait`, die einen Systemstart auch dann erlauben, wenn der Datenträger zur Startzeit nicht eingebunden werden kann. Weitere Informationen zu diesen Parametern finden Sie in der Dokumentation zu Ihrer Verteilung.

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
Informieren Sie sich in den folgenden Artikeln ausführlicher über das Verwenden von virtuellen Linux-Computern:

- [Anmelden bei einem mit Linux betriebenen virtuellen Computer][Logon]

- [Trennen eines Datenträgers von einem virtuellen Linux-Computer](virtual-machines-linux-classic-detach-disk.md)

- [Verwenden der Azure-Befehlszeilenschnittstelle beim klassischen Bereitstellungsmodell](../virtual-machines-command-line-tools.md)

<!--Link references-->
[Agent]: virtual-machines-linux-agent-user-guide.md
[Logon]: virtual-machines-linux-classic-log-on.md

<!---HONumber=AcomDC_0720_2016-->