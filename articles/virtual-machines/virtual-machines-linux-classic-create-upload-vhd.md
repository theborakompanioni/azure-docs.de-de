<properties
	pageTitle="Erstellen und Hochladen einer Linux-VHD | Microsoft Azure"
	description="Hier erfahren Sie, wie Sie eine virtuelle Azure-Festplatte (Virtual Hard Disk, VHD), die das Linux-Betriebssystem enthält, mit dem klassischen Bereitstellungsmodell erstellen und hochladen."
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
	ms.date="09/01/2016"
	ms.author="iainfou"/>

# Erstellen und Hochladen einer virtuellen Festplatte, die das Linux-Betriebssystem enthält

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] Sie können auch [ein benutzerdefiniertes Datenträgerimage mit dem Azure Resource Manager hochladen](virtual-machines-linux-upload-vhd.md).

Dieser Artikel erläutert, wie Sie eine virtuelle Festplatte (Virtual Hard Disk, VHD) erstellen und hochladen, um sie als eigenes Image für die Erstellung von virtuellen Computern in Azure zu nutzen. Sie erfahren, wie Sie das Betriebssystem vorbereiten, um es zum Erstellen mehrerer virtueller Computer auf Grundlage des Images zu verwenden.


## Voraussetzungen
In diesem Artikel wird davon ausgegangen, dass Sie über die folgenden Elemente verfügen:

- **In einer VHD-Datei installiertes Linux-Betriebssystem**: Sie haben eine von Azure unterstützte Linux-Distribution (siehe [Linux auf von Azure unterstützten Verteilungen](virtual-machines-linux-endorsed-distros.md)) installiert (falls nicht, siehe [Informationen zu nicht unterstützten Verteilungen](virtual-machines-linux-create-upload-generic.md)). Für die Erstellung virtueller Computer und Festplatten stehen verschiedene Tools zur Verfügung:
	- Installieren und konfigurieren Sie [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) oder [KVM](http://www.linux-kvm.org/page/RunningKVM), und verwenden Sie dabei „VHD“ als Imageformat. Bei Bedarf können Sie ein Image mithilfe von `qemu-img convert` [konvertieren](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats).
	- Unter [Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) und [Windows Server 2012/2012 R2](https://technet.microsoft.com/library/hh846766.aspx) können Sie auch Hyper-V verwenden.

> [AZURE.NOTE] Das modernere VHDX-Format wird in Azure noch nicht unterstützt. Wenn Sie einen virtuellen Computer erstellen, geben Sie als Format VHD an. VHDX-Datenträger können bei Bedarf mit [`qemu-img convert`](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) oder mit dem PowerShell-Cmdlet [`Convert-VHD`](https://technet.microsoft.com/library/hh848454.aspx) in VHD konvertiert werden. Azure unterstützt außerdem das Hochladen von dynamischen VHDs nicht, daher müssen Sie solche Datenträger vor dem Hochladen in statische VHDs konvertieren. Mit Tools wie [Azure VHD Utilities for GO](https://github.com/Microsoft/azure-vhd-utils-for-go) (Azure-VHD-Hilfsprogramme für Go) können Sie dynamische Datenträger im Zuge des Uploads zu Azure konvertieren.

- **Azure-Befehlszeilenschnittstelle**: Installieren Sie die neueste [Azure-Befehlszeilenschnittstelle](../virtual-machines-command-line-tools.md), um die VHD hochzuladen.

<a id="prepimage"> </a>
## Schritt 1: Vorbereiten des hochzuladenden Images

Azure unterstützt eine Vielzahl von Linux-Distributionen (siehe [Unterstützte Distributionen](virtual-machines-linux-endorsed-distros.md)). Die folgenden Artikel führen Sie durch die Vorbereitung der verschiedenen Linux-Distributionen, die in Azure unterstützt werden. Nachdem Sie die Schritte in den folgenden Leitfäden ausgeführt haben, kehren Sie hierher zurück, sobald Sie über eine VHD-Datei verfügen, die in Azure hochgeladen werden kann:

- **[CentOS-basierte Verteilungen](virtual-machines-linux-create-upload-centos.md)**
- **[Debian Linux](virtual-machines-linux-debian-create-upload-vhd.md)**
- **[Oracle Linux](virtual-machines-linux-oracle-create-upload-vhd.md)**
- **[Red Hat Enterprise Linux](virtual-machines-linux-redhat-create-upload-vhd.md)**
- **[SLES und openSUSE](virtual-machines-linux-suse-create-upload-vhd.md)**
- **[Ubuntu](virtual-machines-linux-create-upload-ubuntu.md)**
- **[Sonstige – nicht unterstützte Distributionen](virtual-machines-linux-create-upload-generic.md)**

> [AZURE.NOTE] Das Azure-Plattform-SLA bezieht sich nur dann auf die virtuellen Computer mit Linux-Betriebssystem, wenn eine unterstützte Distribution mit Konfigurationsdetails verwendet wird, die unter [Linux auf von Azure unterstützten Verteilungen](virtual-machines-linux-endorsed-distros.md) angegeben sind. Alle Linux-Distributionen im Azure-Image-Katalog sind unterstützte Distributionen mit der erforderlichen Konfiguration.

Beachten Sie auch die **[Installationshinweise für Linux](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes)**. Diese enthalten allgemeine Tipps zur Vorbereitung von Linux-Images für Azure.


<a id="connect"> </a>
## Schritt 2: Vorbereiten der Verbindung mit Azure

Stellen Sie sicher, dass Sie die Azure-Befehlszeilenschnittstelle im klassischen Bereitstellungsmodell (`azure config mode asm`) verwenden, und melden Sie sich anschließend bei Ihrem Konto an:

```
azure login
```


<a id="upload"> </a>
## Schritt 3: Hochladen des Image in Azure

Zum Hochladen der VHD-Datei benötigen Sie ein Speicherkonto. Sie können ein vorhandenes Speicherkonto auswählen oder [ein neues erstellen](../storage/storage-create-storage-account.md).

Verwenden Sie die Azure-Befehlszeilenschnittstelle zum Hochladen des Images mithilfe des folgenden Befehls:

```bash
azure vm image create <ImageName> `
	--blob-url <BlobStorageURL>/<YourImagesFolder>/<VHDName> `
	--os Linux <PathToVHDFile>
```

Im vorherigen Beispiel:

- **BlobStorageURL** ist die URL für das Speicherkonto, das Sie verwenden möchten.
- Bei **YourImagesFolder** handelt es sich um den Container innerhalb des Blobspeichers, in dem Sie Ihre Images speichern möchten.
- **VHDName** steht für die Bezeichnung, die im Portal zur Identifizierung der virtuellen Festplatte angezeigt wird.
- **PathToVHDFile** stellt den vollständigen Pfad und den Namen der VHD-Datei auf Ihrem Computer dar.

Im Folgenden finden Sie ein vollständiges Beispiel:

```bash
azure vm image create UbuntuLTS `
	--blob-url https://teststorage.blob.core.windows.net/vhds/UbuntuLTS.vhd `
	--os Linux /home/ahmet/UbuntuLTS.vhd
```

## Schritt 4: Virtuellen Computer über das Image erstellen
Sie erstellen einen virtuellen Computer mit `azure vm create` auf die gleiche Weise wie einen normalen virtuellen Computer. Geben Sie den Namen an, den Sie Ihrem Image im vorherigen Schritt gegeben haben. Im folgenden Beispiel verwenden wir den Namen des **UbuntuLTS**-Image, den Sie im vorherigen Schritt vergeben haben:

```bash
azure vm create --userName ops --password P@ssw0rd! --vm-size Small --ssh `
	--location "West US" "DeployedUbuntu" UbuntuLTS
```

Um Ihre eigenen virtuellen Computer zu erstellen, geben Sie Ihren eigenen Benutzernamen + Kennwort, Speicherort, DNS-Namen und Imagenamen an.

## Nächste Schritte

Weitere Informationen finden Sie unter [Befehle der Azure-Befehlszeilenschnittstelle im Modus „Azure-Dienstverwaltung“ (Azure Service Management, ASM)](../virtual-machines-command-line-tools.md).

[Step 1: Prepare the image to be uploaded]: #prepimage
[Step 2: Prepare the connection to Azure]: #connect
[Step 3: Upload the image to Azure]: #upload

<!---HONumber=AcomDC_0907_2016-->