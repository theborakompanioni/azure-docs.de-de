---
title: Hochladen oder Kopieren einer benutzerdefinierten Linux-VM mithilfe der Azure CLI 2.0 | Microsoft-Dokumentation
description: Laden Sie mit dem Resource Manager-Bereitstellungsmodell und der Azure CLI 2.0 einen benutzerdefinierten virtuellen Computer hoch, oder kopieren Sie ihn.
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: a8c7818f-eb65-409e-aa91-ce5ae975c564
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 07/06/2017
ms.author: cynthn
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: 7c297725c26ea6c44403a10ecdcc3542f89f10b4
ms.contentlocale: de-de
ms.lasthandoff: 08/21/2017

---
# <a name="create-a-linux-vm-from-custom-disk-with-the-azure-cli-20"></a>Erstellen eines virtuellen Linux-Computers aus einem benutzerdefinierten Datenträger mithilfe der Azure CLI 2.0

<!-- rename to create-vm-specialized -->

In diesem Artikel erfahren Sie, wie Sie einen benutzerdefinierten virtuellen Datenträger (Virtual Hard Disk, VHD) hochladen und neue virtuelle Linux-Computer (Virtual Machines, VMs) aus dem benutzerdefinierten Datenträger erstellen. Sie können eine Linux-Distribution installieren und konfigurieren und die VHD dann zur schnellen Erstellung eines neuen virtuellen Azure-Computers verwenden.

Wenn Sie mehrere virtuelle Computer aus Ihrem benutzerdefinierten Datenträger erstellen möchten, sollten Sie ein Image ihres virtuellen Computers oder Ihrer VHD erstellen. Weitere Informationen finden Sie in [Erstellen eines benutzerdefinierten Images eines virtuellen Azure-Computers mithilfe der Befehlszeilenschnittstelle](tutorial-custom-images.md).

Sie haben zwei Möglichkeiten:
* [Hochladen einer VHD-Datei](#option-1-upload-a-specialized-vhd)
* [Kopieren einer vorhandenen Azure-VM](#option-2-copy-an-existing-azure-vm)

## <a name="quick-commands"></a>Schnellbefehle

Beim Erstellen eines neuen virtuellen Computers aus einem benutzerdefinierten oder spezialisierten Datenträger mit [az vm create](/cli/azure/vm#create) **fügen Sie** den Datenträger an (--attach-os-disk), anstatt ein benutzerdefiniertes oder Marketplace-Image anzugeben (--image). Das folgende Beispiel erstellt einen virtuellen Computer namens *myVM* mithilfe des verwalteten Datenträgers namens *myManagedDisk*, der aus Ihrer benutzerdefinierten VHD erstellt wurde:

```azurecli
az vm create --resource-group myResourceGroup --location eastus --name myVM \
   --os-type linux --attach-os-disk myManagedDisk
```

## <a name="requirements"></a>Anforderungen
Um die folgenden Schritte ausführen zu können, benötigen Sie Folgendes:

* Einen virtuellen Linux-Computer, der für die Verwendung in Azure vorbereitet wurde. Im Abschnitt [Vorbereiten des virtuellen Computers](#prepare-the-vm) in diesem Artikel wird erläutert, wo Sie distributionsspezifische Informationen über das Installieren des Azure Linux-Agents (waagent) finden. Diese Installation ist notwendig, damit die VM in Azure reibungslos funktioniert und damit Sie sich mit SSH mit der VM verbinden können.
* Die VHD-Datei einer vorhandenen [ von Azure unterstützten Linux-Distribution](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (oder [Informationen zu nicht unterstützten Distributionen](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) an einen virtuellen Datenträger im VHD-Format. Für die Erstellung virtueller Computer und Festplatten stehen verschiedene Tools zur Verfügung:
  * Installieren und konfigurieren Sie [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) oder [KVM](http://www.linux-kvm.org/page/RunningKVM), und verwenden Sie dabei „VHD“ als Imageformat. Bei Bedarf können Sie [ein Image](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) mit **qemu-img convert** konvertieren.
  * Unter [Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) und [Windows Server 2012/2012 R2](https://technet.microsoft.com/library/hh846766.aspx) können Sie auch Hyper-V verwenden.

> [!NOTE]
> Das modernere VHDX-Format wird in Azure noch nicht unterstützt. Wenn Sie einen virtuellen Computer erstellen, geben Sie als Format VHD an. VHDX-Datenträger können bei Bedarf mit [qemu-img convert](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) oder mit dem PowerShell-Cmdlet [Convert-VHD](https://technet.microsoft.com/library/hh848454.aspx) in eine VHD konvertiert werden. Azure unterstützt außerdem das Hochladen von dynamischen VHDs nicht, daher müssen Sie solche Datenträger vor dem Hochladen in statische VHDs konvertieren. Mit Tools wie [Azure VHD Utilities for GO](https://github.com/Microsoft/azure-vhd-utils-for-go) (Azure-VHD-Hilfsprogramme für Go) können Sie dynamische Datenträger im Zuge des Uploads zu Azure konvertieren.
> 
> 


* Achten Sie darauf, dass Sie die neueste Version von [Azure CLI 2.0](/cli/azure/install-az-cli2) installiert haben und mit [az login](/cli/azure/#login) bei einem Azure-Konto angemeldet sind.

Ersetzen Sie in den folgenden Beispielen die Beispielparameternamen durch Ihre eigenen Werte. Beispielparameternamen sind u.a. *myResourceGroup*, *mystorageaccount* und *mydisks*.

<a id="prepimage"> </a>

## <a name="prepare-the-vm"></a>Vorbereiten des virtuellen Computers

Azure unterstützt eine Vielzahl von Linux-Distributionen (siehe [Unterstützte Distributionen](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). Die folgenden Artikel führen Sie durch die Vorbereitung der verschiedenen Linux-Distributionen, die in Azure unterstützt werden:

* [CentOS-basierte Verteilungen](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Debian Linux](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Oracle Linux](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Red Hat Enterprise Linux](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [SLES & openSUSE](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Ubuntu](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Sonstige - Nicht unterstützte Distributionen](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Beachten Sie auch die [Installationshinweise für Linux](create-upload-generic.md#general-linux-installation-notes). Diese enthalten allgemeine Tipps zur Vorbereitung von Linux-Images für Azure.

> [!NOTE]
> Die [Azure Platform-SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) gilt nur dann für virtuelle Computer unter Linux, wenn eine der unterstützten Distributionen mit Konfigurationsdetails verwendet wird, die im Abschnitt mit den unterstützten Versionen unter [Linux auf von Azure unterstützten Verteilungen](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) angegeben sind.
> 
> 

## <a name="option-1-upload-a-vhd"></a>Option 1: Hochladen einer VHD

Sie können eine benutzerdefinierte VHD hochladen, die bei Ihnen auf einem lokalen Computer ausgeführt wird oder die Sie aus einer anderen Cloud exportiert haben. Um die VHD dafür zu verwenden, eine neue Azure VM zu erstellen, müssen Sie die VHD in ein Speicherkonto hochladen und aus ihr einen verwalteten Datenträger erstellen. 

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Vor dem Hochladen Ihres benutzerdefinierten Datenträgers und dem Erstellen virtueller Computer müssen Sie zuerst mit [az group create](/cli/azure/group#create) eine Ressourcengruppe erstellen.

Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen *myResourceGroup* am Standort *eastus*: [Azure Managed Disks – Übersicht](../windows/managed-disks-overview.md)
```azurecli
az group create \
    --name myResourceGroup \
    --location eastus
```

### <a name="create-a-storage-account"></a>Erstellen Sie ein Speicherkonto.

Erstellen Sie mithilfe von [az storage account create](/cli/azure/storage/account#create) ein Speicherkonto für Ihren benutzerdefinierten Datenträger und die virtuellen Computer. 

Das folgende Beispiel erstellt ein Speicherkonto namens *mystorageaccount* in der zuvor erstellten Ressourcengruppe:

```azurecli
az storage account create \
    --resource-group myResourceGroup \
    --location eastus \
    --name mystorageaccount \
    --kind Storage \
    --sku Standard_LRS
```

### <a name="list-storage-account-keys"></a>Auflisten von Speicherkontoschlüsseln
Azure generiert zwei 512-Bit-Zugriffsschlüssel für jedes Speicherkonto. Die Zugriffsschlüssel werden für die Authentifizierung beim Speicherkonto verwendet, um beispielsweise Schreibvorgänge auszuführen. Erfahren Sie [hier](../../storage/common/storage-create-storage-account.md#manage-your-storage-account)mehr über das Verwalten von Speicherzugriff. Zeigen Sie mit [az storage account keys list](/cli/azure/storage/account/keys#list) die Zugriffsschlüssel an.

Zeigen Sie die Zugriffsschlüssel für das erstellte Speicherkonto an:

```azurecli
az storage account keys list \
    --resource-group myResourceGroup \
    --account-name mystorageaccount
```

Die Ausgabe sieht in etwa wie folgt aus:

```azurecli
info:    Executing command storage account keys list
+ Getting storage account keys
data:    Name  Key                                                                                       Permissions
data:    ----  ----------------------------------------------------------------------------------------  -----------
data:    key1  d4XAvZzlGAgWdvhlWfkZ9q4k9bYZkXkuPCJ15NTsQOeDeowCDAdB80r9zA/tUINApdSGQ94H9zkszYyxpe8erw==  Full
data:    key2  Ww0T7g4UyYLaBnLYcxIOTVziGAAHvU+wpwuPvK4ZG0CDFwu/mAxS/YYvAQGHocq1w7/3HcalbnfxtFdqoXOw8g==  Full
info:    storage account keys list command OK
```
Notieren Sie sich **key1**, da Sie ihn in den nächsten Schritten für die Interaktion mit Ihrem Speicherkonto benötigen.

### <a name="create-a-storage-container"></a>Erstellen eines Speichercontainers
Auf die gleiche Weise, in der Sie verschiedene Verzeichnisse erstellen, um Ihr lokales Dateisystem logisch zu organisieren, erstellen Sie Container in einem Speicherkonto, um die Datenträger zu organisieren. Ein Speicherkonto kann eine beliebige Anzahl von Containern enthalten. Erstellen Sie mit [az storage container create](/cli/azure/storage/container#create) einen Container.

Im folgenden Beispiel wird ein Container namens *mydisks* erstellt:

```azurecli
az storage container create \
    --account-name mystorageaccount \
    --name mydisks
```

### <a name="upload-the-vhd"></a>Hochladen der VHD
Laden Sie jetzt Ihren benutzerdefinierten Datenträger mit [az storage blob upload](/cli/azure/storage/blob#upload) hoch. Das Hochladen und Speichern Ihrer benutzerdefinierten Datenträger erfolgt als Seitenblob.

Geben Sie Ihren Zugriffsschlüssel, den Container, den Sie im vorherigen Schritt erstellt haben, und dann den Pfad zum benutzerdefinierten Datenträger auf dem lokalen Computer an:

```azurecli
az storage blob upload --account-name mystorageaccount \
    --account-key key1 \
    --container-name mydisks \
    --type page \
    --file /path/to/disk/mydisk.vhd \
    --name myDisk.vhd
```
Das Hochladen der VHD kann eine Weile dauern.

### <a name="create-a-managed-disk"></a>Erstellen eines verwalteten Datenträgers


Erstellen Sie einen verwalteten Datenträger mit [az disk create](/cli/azure/disk#create). Das folgende Beispiel erstellt den verwalteten Datenträger namens *myManagedDisk* aus der VHD, die Sie in Ihr benanntes Speicherkonto und den Container hochgeladen haben:

```azurecli
az disk create \
    --resource-group myResourceGroup \
    --name myManagedDisk \
  --source https://mystorageaccount.blob.core.windows.net/mydisks/myDisk.vhd
```
## <a name="option-2-copy-an-existing-vm"></a>Option 2: Kopieren einer vorhandenen VM

Um eine weitere Kopie zu erstellen, können Sie auch zuerst die benutzerdefinierte VM in Azure erstellen und dann den Betriebssystemdatenträger kopieren und an eine neue VM anfügen. Dies ist für Testzwecke ausreichend, aber wenn Sie eine vorhandene Azure-VM als Modell für mehrere neue VMs verwenden möchten, sollten Sie stattdessen ein **Image** erstellen. Weitere Informationen über das Erstellen eines Images einer vorhandenen Azure-VM finden Sie in [Erstellen eines benutzerdefinierten Images eines virtuellen Azure-Computers mithilfe der Befehlszeilenschnittstelle](tutorial-custom-images.md)

### <a name="create-a-snapshot"></a>Erstellen einer Momentaufnahme

Mit diesem Beispiel wird eine Momentaufnahme von einer VM namens *myVM* in der Ressourcengruppe *myResourceGroup* erstellt. Der Name der erstellten Momentaufnahme lautet *osDiskSnapshot*.

```azure-cli
osDiskId=$(az vm show -g myResourceGroup -n myVM --query "storageProfile.osDisk.managedDisk.id" -o tsv)
az snapshot create \
    -g myResourceGroup \
    --source "$osDiskId" \
    --name osDiskSnapshot
```
###  <a name="create-the-managed-disk"></a>Erstellen des verwalteten Datenträgers

Erstellen eines neuen verwalteten Datenträgers aus der Momentaufnahme.

Ermitteln Sie die ID der Momentaufnahme. In unserem Beispiel heißt die Momentaufnahme *osDiskSnapshot* und befindet sich in der Ressourcengruppe *myResourceGroup*.

```azure-cli
snapshotId=$(az snapshot show --name osDiskSnapshot --resource-group myResourceGroup --query [id] -o tsv)
```

Erstellen Sie den verwalteten Datenträger. In diesem Beispiel erstellen wir einen verwalteten Datenträger namens *myManagedDisk* aus unserer Momentaufnahme. Der Datenträger wird 128 GB Standardspeicher bieten.

```azure-cli
az disk create \
    --resource-group myResourceGroup \
    --name myManagedDisk \
    --sku Standard_LRS \
    --size-gb 128 \
    --source $snapshotId
```

## <a name="create-the-vm"></a>Erstellen des virtuellen Computers

Erstellen Sie nun Ihren virtuellen Computer mit [az vm create](/cli/azure/vm#create), und fügen Sie den verwalteten Datenträger als Betriebssystemdatenträger an (--attach-os-disk). Das folgende Beispiel erstellt den virtuellen Computer *MyNewVM* mithilfe des verwalteten Datenträgers, der aus der hochgeladenen VHD erstellt wurde:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNewVM \
    --os-type linux \
    --attach-os-disk myManagedDisk
```

Mit den Anmeldeinformationen der Quell-VM sollten Sie sich mit SSH mit der VM verbinden können. 

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie den benutzerdefinierten virtuellen Datenträger vorbereitet und hochgeladen haben, können Sie sich mit der [Verwendung von Resource Manager und Vorlagen](../../azure-resource-manager/resource-group-overview.md)beschäftigen. Informationen zum Hinzufügen eines Datenträgers zu Ihren neuen virtuellen Computern finden Sie [hier](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) . Falls auf Ihren virtuellen Computern Anwendungen ausgeführt werden, auf die Sie zugreifen müssen, müssen Sie [Ports und Endpunkte öffnen](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


