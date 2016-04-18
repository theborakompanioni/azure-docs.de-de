<properties
	pageTitle="Vorbereiten einer Debian Linux-VHD | Microsoft Azure"
	description="Erfahren Sie, wie Debian 7- und 8-VHD-Dateien für die Bereitstellung in Azure erstellt werden."
	services="virtual-machines-linux"
	documentationCenter=""
	authors="szarkos"
	manager="timlt"
	editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
	ms.service="virtual-machines-linux"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-linux"
	ms.devlang="na"
	ms.topic="article"
	ms.date="03/25/2016"
	ms.author="szark"/>




# Vorbereiten einer Debian-VHD für Azure

## Voraussetzungen
In diesem Abschnitt wird davon ausgegangen, dass Sie bereits ein Debian Linux-Betriebssystem aus einer ISO-Datei von der [Debian-Website](https://www.debian.org/distrib/) auf eine virtuelle Festplatte installiert haben. Es gibt mehrere Tools zum Erstellen von VHD-Dateien. Hyper-V ist nur ein Beispiel. Anweisungen zur Verwendung von Hyper-V finden Sie unter [Installieren der Hyper-V-Rolle und Konfigurieren eines virtuellen Computers](https://technet.microsoft.com/library/hh846766.aspx).


## Installationshinweise

- Das modernere VHDX-Format wird in Azure noch nicht unterstützt. Sie können den Datenträger mit dem Hyper-V-Manager oder dem **Convert-VHD-Cmdlet** in das VHD-Format konvertieren.
- Beim Installieren des Linux-Systems wird empfohlen, anstelle von LVM (bei vielen Installationen oftmals voreingestellt) die Standardpartitionen zu verwenden. Dadurch lässt sich vermeiden, dass ein LVM-Namenskonflikt mit geklonten virtuellen Computern auftritt, besonders dann, wenn ein BS-Datenträger zu Fehlerbehebungszwecken mit einem anderen virtuellen Computer verbunden wird. LVM oder RAID kann wahlweise auf Datenträgern verwendet werden.
- Konfigurieren Sie keine SWAP-Partition auf einem Betriebssystemdatenträger. Der Azure-Linux-Agent kann konfiguriert werden, eine Auslagerungsdatei auf dem temporären Ressourcendatenträger zu erstellen. Weitere Informationen dazu finden Sie in den folgenden Schritten.
- Alle virtuellen Festplatten müssen eine Größe aufweisen, die ein Vielfaches von 1 MB ist.


## Verwenden von azure-manage zum Erstellen von Debian-VHDs

Zum Generieren von Debian-VHDs für Azure sind Tools verfügbar, z.B. die [azure-manage](https://gitlab.credativ.com/de/azure-manage)-Skripts von [Credativ](http://www.credativ.com/). Dies ist der empfohlene Ansatz, mit dem vermieden werden kann, ein Image von Grund auf neu zu erstellen. Wenn Sie beispielsweise eine Debian 8-VHD erstellen möchten, führen Sie die folgenden Befehle aus, um azure-manage (und Abhängigkeiten) herunterzuladen und das Skript „azure\_build\_image“ auszuführen:

	# sudo apt-get update
	# sudo apt-get install git qemu-utils mbr kpartx debootstrap

	# sudo apt-get install python3-pip
	# sudo pip3 install azure-storage azure-servicemanagement-legacy pytest pyyaml
	# git clone https://gitlab.credativ.com/de/azure-manage.git
	# cd azure-manage
	# sudo pip3 install .

	# sudo azure_build_image --option release=jessie --option image_size_gb=30 --option image_prefix=debian-jessie-azure section


## Manuelles Vorbereiten einer Debian-VHD

1. Wählen Sie im Hyper-V-Manager den virtuellen Computer aus.

2. Klicken Sie auf **Verbinden**, um ein Konsolenfenster für den virtuellen Computer zu öffnen.

3. Kommentieren Sie die Zeile für **Deb-CD-ROM** in `/etc/apt/source.list`, wenn Sie den virtuellen Computer mit einer ISO-Datei einrichten.

4. Bearbeiten Sie die Datei `/etc/default/grub` und ändern Sie den **GRUB\_CMDLINE\_LINUX**-Parameter wie folgt, um zusätzliche Kernelparameter für Azure einzubinden.

        GRUB_CMDLINE_LINUX="console=tty0 console=ttyS0,115200 earlyprintk=ttyS0,115200 rootdelay=30"

5. Erstellen Sie Grub neu und führen Sie Folgendes aus:

        # sudo update-grub

6. Fügen Sie „/etc/apt/sources.list“ die Debian-Azure-Repositorys für Debian 7 oder 8 hinzu:

	**Debian 7.x „Wheezy“**

		deb http://debian-archive.trafficmanager.net/debian wheezy-backports main
		deb-src http://debian-archive.trafficmanager.net/debian wheezy-backports main
		deb http://debian-archive.trafficmanager.net/debian-azure wheezy main
		deb-src http://debian-archive.trafficmanager.net/debian-azure wheezy main


	**Debian 8.x „Jessie“**

		deb http://debian-archive.trafficmanager.net/debian jessie-backports main
		deb-src http://debian-archive.trafficmanager.net/debian jessie-backports main
		deb http://debian-archive.trafficmanager.net/debian-azure jessie main
		deb-src http://debian-archive.trafficmanager.net/debian-azure jessie main


7. Installieren des Azure Linux Agent:

		# sudo apt-get update
		# sudo apt-get install waagent

8. Für Debian 7 muss der 3.16-basierte Kernel aus dem wheezy-backports-Repository ausgeführt werden. Erstellen Sie zunächst eine Datei namens „/etc/apt/preferences.d/linux.pref“ mit dem folgenden Inhalt:

		Package: linux-image-amd64 initramfs-tools
		Pin: release n=wheezy-backports
		Pin-Priority: 500

	Führen Sie dann „sudo apt-get install linux-image-amd64“ aus, um den neuen Kernel zu installieren.

8. Setzen Sie den virtuellen Computer zurück und bereiten Sie ihn für die Bereitstellung in Azure vor. Führen Sie Folgendes aus:

        # sudo waagent –force -deprovision
        # export HISTSIZE=0
        # logout

9. Klicken Sie im Hyper-V-Manager auf **Aktion -> Herunterfahren**. Ihre Linux-VHD kann nun in Azure hochgeladen werden.


## Nächste Schritte

Sie können jetzt mit Ihrer Debian-VHD-Datei neue virtuelle Azure-Computer in Azure erstellen. Wenn Sie die VHD-Datei zum ersten Mal in Azure hochladen, führen Sie die Schritte 2 und 3 unter [Creating and uploading a virtual hard disk that contains the Linux operating system](virtual-machines-linux-classic-create-upload-vhd.md) (Erstellen und Hochladen einer virtuellen Festplatte, die das Linux-Betriebssystem enthält) aus.

<!---HONumber=AcomDC_0406_2016-->