---
title: Erstellen und Hochladen einer Linux-VHD in Azure | Microsoft Docs
description: "Erstellen Sie eine virtuelle Azure-Festplatte (Virtual Hard Disk, VHD), die das Linux-Betriebssystem enthält, mit dem klassischen Bereitstellungsmodell, und laden Sie diese hoch."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 8058ff98-db03-4309-9bf4-69842bd64dd4
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 11/28/2016
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 8bb0357a1ac2effd1144afd2af1741205592d253
ms.lasthandoff: 04/03/2017


---
# <a name="creating-and-uploading-a-virtual-hard-disk-that-contains-the-linux-operating-system"></a>Erstellen und Hochladen einer virtuellen Festplatte, die das Linux-Betriebssystem enthält
> [!IMPORTANT] 
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Resource Manager- und klassische Bereitstellung](../../../resource-manager-deployment-model.md). Dieser Artikel befasst sich mit der Verwendung des klassischen Bereitstellungsmodells. Microsoft empfiehlt für die meisten neuen Bereitstellungen die Verwendung des Ressourcen-Manager-Modells. Sie können auch [ein benutzerdefiniertes Datenträgerimage mit dem Azure Resource Manager hochladen](../upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Dieser Artikel erläutert, wie Sie eine virtuelle Festplatte (Virtual Hard Disk, VHD) erstellen und hochladen, um sie als eigenes Image für die Erstellung von virtuellen Computern in Azure zu nutzen. Sie erfahren, wie Sie das Betriebssystem vorbereiten, um es zum Erstellen mehrerer virtueller Computer auf Grundlage des Images zu verwenden. 


## <a name="prerequisites"></a>Voraussetzungen
In diesem Artikel wird davon ausgegangen, dass Sie über die folgenden Elemente verfügen:

* **In einer VHD-Datei installiertes Linux-Betriebssystem**: Sie haben eine [von Azure unterstützte Linux-Distribution](../endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) auf einem virtuellen Datenträger im VHD-Format installiert (falls nicht, siehe [Informationen zu nicht unterstützten Verteilungen](../create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). Für die Erstellung virtueller Computer und Festplatten stehen verschiedene Tools zur Verfügung:
  * Installieren und konfigurieren Sie [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) oder [KVM](http://www.linux-kvm.org/page/RunningKVM), und verwenden Sie dabei „VHD“ als Imageformat. Bei Bedarf können Sie ein Image mithilfe von `qemu-img convert` [konvertieren](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats).
  * Unter [Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) und [Windows Server 2012/2012 R2](https://technet.microsoft.com/library/hh846766.aspx) können Sie auch Hyper-V verwenden.

> [!NOTE]
> Das modernere VHDX-Format wird in Azure noch nicht unterstützt. Wenn Sie einen virtuellen Computer erstellen, geben Sie als Format VHD an. VHDX-Datenträger können bei Bedarf mit [`qemu-img convert`](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) oder mit dem PowerShell-Cmdlet [`Convert-VHD`](https://technet.microsoft.com/library/hh848454.aspx) in VHD konvertiert werden. Azure unterstützt außerdem das Hochladen von dynamischen VHDs nicht, daher müssen Sie solche Datenträger vor dem Hochladen in statische VHDs konvertieren. Mit Tools wie [Azure VHD Utilities for GO](https://github.com/Microsoft/azure-vhd-utils-for-go) (Azure-VHD-Hilfsprogramme für Go) können Sie dynamische Datenträger im Zuge des Uploads zu Azure konvertieren.

* **Azure-Befehlszeilenschnittstelle** : Installieren Sie die neueste [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2) , um die VHD hochzuladen.

<a id="prepimage"> </a>

## <a name="step-1-prepare-the-image-to-be-uploaded"></a>Schritt 1: Vorbereiten des hochzuladenden Images
Azure unterstützt eine Vielzahl von Linux-Distributionen (siehe [Unterstützte Distributionen](../endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). Die folgenden Artikel führen Sie durch die Vorbereitung der verschiedenen Linux-Distributionen, die in Azure unterstützt werden. Nachdem Sie die Schritte in den folgenden Leitfäden ausgeführt haben, kehren Sie hierher zurück, sobald Sie über eine VHD-Datei verfügen, die in Azure hochgeladen werden kann:

* **[CentOS-basierte Verteilungen](../create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Debian Linux](../debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Oracle Linux](../oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Red Hat Enterprise Linux](../redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[SLES und openSUSE](../suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Ubuntu](../create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Sonstige – nicht unterstützte Distributionen](../create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**

> [!NOTE]
> Das Azure-Plattform-SLA bezieht sich nur dann auf die virtuellen Computer mit Linux-Betriebssystem, wenn eine unterstützte Distribution mit Konfigurationsdetails verwendet wird, die unter [Linux auf von Azure unterstützten Verteilungen](../endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) angegeben sind. Alle Linux-Distributionen im Azure-Image-Katalog sind unterstützte Distributionen mit der erforderlichen Konfiguration.
> 
> 

Beachten Sie auch die **[Installationshinweise für Linux](../create-upload-generic.md#general-linux-installation-notes)**. Diese enthalten allgemeine Tipps zur Vorbereitung von Linux-Images für Azure.

<a id="connect"> </a>

## <a name="step-2-prepare-the-connection-to-azure"></a>Schritt 2: Vorbereiten der Verbindung mit Azure
Stellen Sie sicher, dass Sie die Azure-Befehlszeilenschnittstelle im klassischen Bereitstellungsmodell (`azure config mode asm`) verwenden, und melden Sie sich anschließend bei Ihrem Konto an:

```azurecli
azure login
```


<a id="upload"> </a>

## <a name="step-3-upload-the-image-to-azure"></a>Schritt 3: Hochladen des Image in Azure
Zum Hochladen der VHD-Datei benötigen Sie ein Speicherkonto. Sie können ein vorhandenes Speicherkonto auswählen oder [ein neues erstellen](../../../storage/storage-create-storage-account.md).

Verwenden Sie die Azure-Befehlszeilenschnittstelle zum Hochladen des Images mithilfe des folgenden Befehls:

```azurecli
azure vm image create <ImageName> `
    --blob-url <BlobStorageURL>/<YourImagesFolder>/<VHDName> `
    --os Linux <PathToVHDFile>
```

Im vorherigen Beispiel:

* **BlobStorageURL** ist die URL für das Speicherkonto, das Sie verwenden möchten.
* **YourImagesFolder** handelt es sich um den Container innerhalb des Blobspeichers, in dem Sie Ihre Images speichern möchten.
* **VHDName** steht für die Bezeichnung, die im Portal zur Identifizierung der virtuellen Festplatte angezeigt wird.
* **PathToVHDFile** stellt den vollständigen Pfad und den Namen der VHD-Datei auf Ihrem Computer dar.

Der folgende Befehl stellt ein vollständiges Beispiel dar:

```azurecli
azure vm image create myImage `
    --blob-url https://mystorage.blob.core.windows.net/vhds/myimage.vhd `
    --os Linux /home/ahmet/myimage.vhd
```

## <a name="step-4-create-a-vm-from-the-image"></a>Schritt 4: Virtuellen Computer über das Image erstellen
Sie erstellen einen virtuellen Computer mit `azure vm create` auf die gleiche Weise wie einen normalen virtuellen Computer. Geben Sie den Namen an, den Sie Ihrem Image im vorherigen Schritt gegeben haben. Im folgenden Beispiel verwenden wir den Imagenamen **myImage**, den Sie im vorherigen Schritt vergeben haben:

```azurecli
azure vm create --userName ops --password P@ssw0rd! --vm-size Small --ssh `
    --location "West US" "myDeployedVM" myImage
```

Um Ihre eigenen virtuellen Computer zu erstellen, geben Sie Ihren eigenen Benutzernamen + Kennwort, Speicherort, DNS-Namen und Imagenamen an.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie unter [Befehle der Azure-Befehlszeilenschnittstelle im Modus „Azure-Dienstverwaltung“ (Azure Service Management, ASM)](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).

[Step 1: Prepare the image to be uploaded]:#prepimage
[Step 2: Prepare the connection to Azure]:#connect
[Step 3: Upload the image to Azure]:#upload

