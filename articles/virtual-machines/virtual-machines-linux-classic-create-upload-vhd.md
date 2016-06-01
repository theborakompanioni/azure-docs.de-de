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
	ms.date="04/12/2016"
	ms.author="iainfou"/>

# Erstellen und Hochladen einer virtuellen Festplatte, die das Linux-Betriebssystem enthält

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Ressourcen-Manager-Modell.

Dieser Artikel erläutert, wie Sie eine virtuelle Festplatte (Virtual Hard Disk, VHD) erstellen und hochladen, um sie als eigenes Image für die Erstellung von virtuellen Computern in Azure zu nutzen. Sie erfahren, wie Sie das Betriebssystem vorbereiten, um es zum Erstellen mehrerer virtueller Computer auf Grundlage des Images zu verwenden.

**Wichtig**: Das Azure-Plattform-SLA bezieht sich nur dann auf die virtuellen Computer mit Linux-Betriebssystem, wenn eine unterstützte Distribution mit Konfigurationsdetails verwendet wird, die unter [Linux auf von Azure unterstützten Distributionen](virtual-machines-linux-endorsed-distros.md) angegeben sind. Alle Linux-Distributionen im Azure-Image-Katalog sind unterstützte Distributionen mit der erforderlichen Konfiguration.


## Voraussetzungen
In diesem Artikel wird davon ausgegangen, dass Sie über die folgenden Elemente verfügen:

- **In einer .vhd-Datei installiertes Linux-Betriebssystem**: Sie haben ein unterstütztes Linux-Betriebssystem auf einem virtuellen Datenträger installiert. Es gibt verschiedene Tools zum Erstellen von VHD-Dateien. Beispielsweise können Sie eine Virtualisierungslösung wie Hyper-V zum Erstellen der VHD-Datei und zum Installieren des Betriebssystems verwenden. Anweisungen hierzu finden Sie unter [Installieren der Hyper-V-Rolle und Konfigurieren eines virtuellen Computers](http://technet.microsoft.com/library/hh846766.aspx).

	> [AZURE.NOTE] Das modernere VHDX-Format wird in Azure noch nicht unterstützt. Sie können den Datenträger mit dem Hyper-V-Manager oder dem convert-vhd-Cmdlet in das VHD-Format konvertieren.

Eine Liste der bestätigten Distributionen finden Sie unter [Linux zu für Azure bestätigte Distributionen](virtual-machines-linux-endorsed-distros.md). Eine allgemeine Liste mit Linux-Distributionen finden Sie unter [Informationen zu nicht unterstützten Verteilungen](virtual-machines-linux-create-upload-generic.md).

- **Azure-Befehlszeilenschnittstelle** – Installieren und verwenden Sie die [Azure-Befehlszeilenschnittstelle](../virtual-machines-command-line-tools.md) zum Hochladen der VHD.

> [AZURE.TIP] Azure unterstützt das Hochladen von dynamischen virtuellen Festplatten nicht, daher müssen Sie solche Datenträger in statische virtuelle Festplatten konvertieren, bevor Sie sie hochladen. Sie können Tools wie [Azure VHD Utilities for GO](https://github.com/Microsoft/azure-vhd-utils-for-go) (Azure-VHD-Hilfsprogramme für Go) verwenden, um dynamische Datenträger zu konvertieren.

<a id="prepimage"> </a>
## Schritt 1: Vorbereiten des hochzuladenden Images

Microsoft Azure unterstützt eine Vielzahl von Linux-Distributionen (siehe [Unterstützte Distributionen](virtual-machines-linux-endorsed-distros.md)). Die folgenden Artikel führen Sie durch die Vorbereitung der verschiedenen Linux-Verteilungen, die auf Azure unterstützt werden:

- **[CentOS-basierte Verteilungen](virtual-machines-linux-create-upload-centos.md)**
- **[Debian Linux](virtual-machines-linux-debian-create-upload-vhd.md)**
- **[Oracle Linux](virtual-machines-linux-oracle-create-upload-vhd.md)**
- **[Red Hat Enterprise Linux](virtual-machines-linux-redhat-create-upload-vhd.md)**
- **[SLES und openSUSE](virtual-machines-linux-suse-create-upload-vhd.md)**
- **[Ubuntu](virtual-machines-linux-create-upload-ubuntu.md)**
- **[Sonstige – nicht unterstützte Distributionen](virtual-machines-linux-create-upload-generic.md)**

Beachten Sie auch die **[Installationshinweise für Linux](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes)**. Dort erhalten Sie weitere Tipps zur Vorbereitung der Linux-Images für Azure.

Nachdem Sie die Schritte in den obigen Leitfäden ausgeführt haben, sollten Sie über eine VHD-Datei verfügen, die in Azure hochgeladen werden kann.

<a id="connect"> </a>
## Schritt 2: Vorbereiten der Verbindung mit Azure

Stellen Sie sicher, dass Sie die Azure-Befehlszeilenschnittstelle im klassischen Bereitstellungsmodell (`azure config mode asm`) verwenden, und melden Sie sich anschließend bei Ihrem Konto an:

```
azure login
```


<a id="upload"> </a>
## Schritt 3: Hochladen des Image in Azure

Zum Hochladen der VHD-Datei wird ein Speicherkonto benötigt. Wählen Sie entweder ein vorhandenes Speicherkonto aus, oder erstellen Sie ein neues. Informationen zum Erstellen eines Speicherkontos finden Sie unter [Erstellen Sie ein Speicherkonto](../storage/storage-create-storage-account.md).

Wenn Sie die .vhd-Datei hochladen, können Sie diese .vhd-Datei an einem beliebigen Speicherort innerhalb des Blobspeichers ablegen. In den folgenden Befehlsbeispielen stellt **BlobStorageURL** die URL für das zu verwendende Speicherkonto dar. Bei **YourImagesFolder** handelt es sich um den Container innerhalb des Blobspeichers, in dem Sie Ihre Images speichern möchten. **VHDName** steht für die Bezeichnung, die im [Azure-Portal](http://portal.azure.com) oder im [klassischen Azure-Portal](http://manage.windowsazure.com) zur Identifizierung der virtuellen Festplatte angezeigt wird. **PathToVHDFile** stellt den vollständigen Pfad und den Namen der VHD-Datei auf Ihrem Computer dar.

Verwenden Sie die Azure-Befehlszeilenschnittstelle zum Hochladen des Images mithilfe des folgenden Befehls:

		azure vm image create <ImageName> --blob-url <BlobStorageURL>/<YourImagesFolder>/<VHDName> --os Linux <PathToVHDFile>

Weitere Informationen finden Sie unter [Befehle der Azure-Befehlszeilenschnittstelle im Modus „Azure-Dienstverwaltung“ (Azure Service Management, ASM)](../virtual-machines-command-line-tools.md).

[Step 1: Prepare the image to be uploaded]: #prepimage
[Step 2: Prepare the connection to Azure]: #connect
[Step 3: Upload the image to Azure]: #upload

<!---HONumber=AcomDC_0518_2016-->