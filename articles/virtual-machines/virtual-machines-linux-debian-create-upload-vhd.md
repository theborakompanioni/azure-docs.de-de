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
    ms.date="08/24/2016"
    ms.author="szark"/>




# <a name="prepare-a-debian-vhd-for-azure"></a>Vorbereiten einer Debian-VHD für Azure

## <a name="prerequisites"></a>Voraussetzungen
In diesem Abschnitt wird davon ausgegangen, dass Sie bereits ein Debian Linux-Betriebssystem aus einer ISO-Datei von der [Debian-Website](https://www.debian.org/distrib/) auf einer virtuelle Festplatte installiert haben. Es gibt mehrere Tools zum Erstellen von VHD-Dateien. Hyper-V ist nur ein Beispiel. Anweisungen zur Verwendung von Hyper-V finden Sie unter [Installieren von Hyper-V und Erstellen eines virtuellen Computers](https://technet.microsoft.com/library/hh846766.aspx).


## <a name="installation-notes"></a>Installationshinweise

- Beachten Sie auch den Artikelabschnitt [Allgemeine Linux-Systemanforderungen](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes), in dem weitere Tipps zur Vorbereitung von Linux für Azure enthalten sind.
- Das modernere VHDX-Format wird in Azure noch nicht unterstützt. Sie können den Datenträger mit dem Hyper-V-Manager oder dem **Convert-VHD-Cmdlet** in das VHD-Format konvertieren.
- Beim Installieren des Linux-Systems wird empfohlen, anstelle von LVM (bei vielen Installationen oftmals voreingestellt) die Standardpartitionen zu verwenden. Dadurch lässt sich vermeiden, dass ein LVM-Namenskonflikt mit geklonten virtuellen Computern auftritt, besonders dann, wenn ein BS-Datenträger zu Fehlerbehebungszwecken mit einem anderen virtuellen Computer verbunden wird. [LVM](virtual-machines-linux-configure-lvm.md) oder [RAID](virtual-machines-linux-configure-raid.md) können wahlweise auf Datenträgern verwendet werden.
- Konfigurieren Sie keine SWAP-Partition auf einem Betriebssystemdatenträger. Der Azure-Linux-Agent kann konfiguriert werden, eine Auslagerungsdatei auf dem temporären Ressourcendatenträger zu erstellen. Weitere Informationen dazu finden Sie in den folgenden Schritten.
- Alle virtuellen Festplatten müssen eine Größe aufweisen, die ein Vielfaches von 1 MB ist.


## <a name="use-azure-manage-to-create-debian-vhds"></a>Verwenden von azure-manage zum Erstellen von Debian-VHDs

Zum Generieren von Debian-VHDs für Azure sind Tools verfügbar, z.B. die [azure-manage](https://github.com/credativ/azure-manage)-Skripts von [credativ](http://www.credativ.com/). Dies ist der empfohlene Ansatz, mit dem vermieden werden kann, ein Image von Grund auf neu zu erstellen. Wenn Sie beispielsweise eine Debian 8-VHD erstellen möchten, führen Sie die folgenden Befehle aus, um azure-manage (und Abhängigkeiten) herunterzuladen und das Skript „azure_build_image“ auszuführen:

    # sudo apt-get update
    # sudo apt-get install git qemu-utils mbr kpartx debootstrap

    # sudo apt-get install python3-pip python3-dateutil python3-cryptography
    # sudo pip3 install azure-storage azure-servicemanagement-legacy azure-common pytest pyyaml
    # git clone https://github.com/credativ/azure-manage.git
    # cd azure-manage
    # sudo pip3 install .

    # sudo azure_build_image --option release=jessie --option image_size_gb=30 --option image_prefix=debian-jessie-azure section


## <a name="manually-prepare-a-debian-vhd"></a>Manuelles Vorbereiten einer Debian-VHD

1. Wählen Sie im Hyper-V-Manager den virtuellen Computer aus.

2. Klicken Sie auf **Verbinden** , um ein Konsolenfenster für den virtuellen Computer zu öffnen.

3. Kommentieren Sie die Zeile für **deb cdrom** in `/etc/apt/source.list` aus, wenn Sie den virtuellen Computer mit einer ISO-Datei einrichten.

4. Bearbeiten Sie die Datei `/etc/default/grub`, und ändern Sie den **GRUB_CMDLINE_LINUX**-Parameter wie folgt, um zusätzliche Kernelparameter für Azure einzubinden.

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

9. Klicken Sie im Hyper-V-Manager auf **Aktion** > „Herunterfahren“. Ihre Linux-VHD kann nun in Azure hochgeladen werden.


## <a name="next-steps"></a>Nächste Schritte

Sie können jetzt mit Ihrer Debian-VHD-Datei neue virtuelle Azure-Computer in Azure erstellen. Wenn Sie die VHD-Datei zum ersten Mal in Azure hochladen, führen Sie die Schritte 2 und 3 in [Erstellen und Hochladen einer virtuellen Festplatte, die das Linux-Betriebssystem enthält](virtual-machines-linux-classic-create-upload-vhd.md)aus.



<!--HONumber=Oct16_HO2-->


