---
title: "Verschiedene Möglichkeiten zum Erstellen eines virtuellen Linux-Computers in Azure | Microsoft Azure"
description: "Hier finden Sie Informationen zu den verschiedenen Möglichkeiten zum Erstellen eines virtuellen Linux-Computers unter Azure sowie Links zu Tools und Tutorials für die einzelnen Methoden."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f38f8a44-6c88-4490-a84a-46388212d24c
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 01/03/2017
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: d4cff286de1abd492ce7276c300b50d71f06345b
ms.openlocfilehash: 1287a028122080c0d9745502a4a98a957894a0de
ms.lasthandoff: 02/27/2017


---
# <a name="different-ways-to-create-a-linux-vm"></a>Verschiedene Möglichkeiten zum Erstellen einer Linux-VM
In Azure können Sie einen virtuellen Linux-Computer mit Ihren bevorzugten Tools und Workflows erstellen. In diesem Artikel sind die Unterschiede und Beispiele für die Erstellung virtueller Linux-Computer zusammengefasst, einschließlich Azure CLI 2.0. Sie können auch Erstellungsoptionen anzeigen, einschließlich [Azure CLI 1.0](virtual-machines-linux-creation-choices-nodejs.md).

[Azure CLI 2.0](/cli/azure/install-az-cli2) ist plattformübergreifend über ein npm-Paket, per Distributionspaket oder per Docker-Container verfügbar. Installieren Sie den am besten geeigneten Build für Ihre Umgebung, und melden sich mit dem [AZ-Login](/cli/azure/#login) in einem Azure-Konto an.

In den folgenden Beispielen wird Azure CLI 2.0 verwendet. Jeder Artikel enthält ausführliche Informationen zu den gezeigten Befehlen. Sie finden auch Beispiele zu Linux-Erstellungsoptionen mithilfe der [Azure CLI 1.0](virtual-machines-linux-creation-choices-nodejs.md).

* [Erstellen eines virtuellen Linux-Computers mit Azure CLI 2.0](virtual-machines-linux-quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
  
  * Dieses Beispiel verwendet [az group create](/cli/azure/group#create) zum Erstellen einer Ressourcengruppe mit dem Namen `myResourceGroup`: 
-    
    ```azurecli
    az group create --name myResourceGroup --location westus
    ```
    
  * Dieses Beispiel verwendet [az vm create](/cli/azure/vm#create) zum Erstellen eines virtuellen Computers mit dem Namen `myVM` unter Verwendung des neuesten Debian-Images mit Azure Managed Disks mit einem öffentlichen Schlüssel namens `id_rsa.pub`:

    ```azurecli
    az vm create \
    --image credativ:Debian:8:latest \
     --admin-username azureuser \
    --ssh-key-value ~/.ssh/id_rsa.pub \
az vm disk attach –g myResourceGroup –-vm-name myVM –-disk myDataDisk  –-new --size-gb 5    --public-ip-address-dns-name myPublicDNS \
    --resource-group myResourceGroup \
    --location westus \
    --name myVM
    ```

    * Wenn Sie nicht verwaltete Datenträger verwenden möchten, fügen Sie dem oben genannten Befehl das Flag `--use-unmanaged-disks` an. Ein Speicherkonto wird für Sie erstellt. Weitere Informationen finden Sie in der [Übersicht über Azure Managed Disks](../storage/storage-managed-disks-overview.md).

* [Erstellen einer geschützten Linux-VM mit einer Azure-Vorlage](virtual-machines-linux-create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
  
  * Im folgenden Beispiel wird [az group deployment create](/cli/azure/group/deployment#create) zum Erstellen eines virtuellen Computers mit einer Vorlage verwendet, die auf GitHub gespeichert ist:
    
    ```azurecli
    az group deployment create --resource-group myResourceGroup \ 
      --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json \
      --parameters @myparameters.json
    ```
    
* [Erstellen einer vollständigen Linux-Umgebung über die Azure-Befehlszeilenschnittstelle](virtual-machines-linux-create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
  
  * Beinhaltet das Erstellen eines Lastenausgleichs und mehrerer virtueller Computer in einer Verfügbarkeitsgruppe.

* [Hinzufügen eines Datenträgers zu einem virtuellen Linux-Computer](virtual-machines-linux-add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
  
  * Im folgenden Beispiel wird [az vm disk attach-new](/cli/azure/vm/disk#attach-new) zum Hinzufügen eines verwalteten 50-GB-Datenträgers zu einem vorhandenen virtuellen Computer mit dem Namen `myVM` verwendet:
  
    ```azurecli
    az vm disk attach –g myResourceGroup –-vm-name myVM –-disk myDataDisk  \
    –-new --size-gb 50
    ```

## <a name="azure-portal"></a>Azure-Portal
Über das [Azure-Portal](https://portal.azure.com) können Sie schnell einen virtuellen Computer erstellen, da bei dieser Variante nichts auf Ihrem System installiert werden muss. Verwenden Sie das Azure-Portal, um den virtuellen Computer zu erstellen:

* [Erstellen einer Linux-VM mit dem Azure-Portal](virtual-machines-linux-quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) 
* [Anfügen eines Datenträgers mit dem Azure-Portal](virtual-machines-linux-attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="operating-system-and-image-choices"></a>Betriebssystem und Image-Optionen
Beim Erstellen einer VM muss ein für das auszuführende Betriebssystem geeignetes Image gewählt werden. Azure und seine Partner bieten zahlreiche Images an – einige mit bereits vorinstallierten Anwendungen und Tools. Alternativ können Sie auch Ihre eigenen Images hochladen (wie [im folgenden Abschnitt](#use-your-own-image)beschrieben).

### <a name="azure-images"></a>Azure-Images
Mithilfe der Befehle vom Typ [az vm image](/cli/azure/vm/image) können Sie verfügbare Elemente nach Herausgeber, Distributionsversion und Build anzeigen.

Liste mit den verfügbaren Herausgebern:

```azurecli
az vm image list-publishers --location WestUS
```

Liste mit den verfügbaren Produkten (Angeboten) für einen Herausgeber:

```azurecli
az vm image list-offers --publisher Canonical --location WestUS
```

Liste mit den verfügbaren SKUs (Distributionsversionen) eines Angebots:

```azurecli
az vm image list-skus --publisher Canonical --offer UbuntuServer --location WestUS
```

Liste mit allen verfügbaren Images für eine bestimmte Version:

```azurecli
az vm image list --publisher Canonical --offer UbuntuServer --sku 16.04.0-LTS --location WestUS
```

Weitere Beispiele für das Durchsuchen und Verwenden von verfügbaren Images finden Sie unter [Auswählen von Linux-VM-Images mit der Azure-CLI](virtual-machines-linux-cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Der Befehl **az vm create** verfügt über Aliase, die Sie zum schnellen Zugreifen auf die gängigeren Distributionen und ihre neuesten Versionen verwenden können. Das geht häufig schneller als bei jeder Erstellung eines virtuellen Computers den Herausgeber, das Angebot, die SKU und die Version anzugeben:

| Alias | Herausgeber | Angebot | SKU | Version |
|:--- |:--- |:--- |:--- |:--- |
| CentOS |OpenLogic |Centos |7.2 |neueste |
| CoreOS |CoreOS |CoreOS |Stable |neueste |
| Debian |credativ |Debian |8 |neueste |
| openSUSE |SUSE |openSUSE |13.2 |neueste |
| RHEL |Redhat |RHEL |7.2 |neueste |
| SLES |SLES |SLES |12-SP1 |neueste |
| UbuntuLTS |Canonical |UbuntuServer |14.04.4-LTS |neueste |

### <a name="use-your-own-image"></a>Verwenden eines eigenen Image
Sollten Sie spezielle Anpassungen benötigen, verwenden Sie ein Image, das auf einem vorhandenen virtuellen Azure-Computer basiert. Hierzu können Sie diesen virtuellen Computer *erfassen*. Sie können auch ein lokal erstelltes Image hochladen. Weitere Informationen zu unterstützten Distributionen und zur Verwendung eigener Images finden Sie in den folgenden Artikeln:

* [Von Azure unterstützte Distributionen](virtual-machines-linux-endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Informationen zu nicht unterstützten Distributionen](virtual-machines-linux-create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Erfassen eines virtuellen Linux-Computers zur Verwendung als Resource Manager-Vorlage](virtual-machines-linux-capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
  
  * Beispiele für Schnellstartbefehle vom Typ **az vm** zum Erfassen eines vorhandenen virtuellen Computers mit nicht verwalteten Datenträgern:
    
    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    az vm generalize --resource-group myResourceGroup --name myVM
    az vm capture --resource-group myResourceGroup --name myVM --vhd-name-prefix myCapturedVM
    ```

## <a name="next-steps"></a>Nächste Schritte
* Erstellen Sie einen virtuellen Linux-Computer über das [Portal](virtual-machines-linux-quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), mit der [Befehlszeilenschnittstelle](virtual-machines-linux-quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) oder mithilfe einer [Azure Resource Manager-Vorlage](virtual-machines-linux-cli-deploy-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Nach dem Erstellen eines virtuellen Linux-Computers können Sie [einen Datenträger hinzufügen](virtual-machines-linux-add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Schritte zum schnellen Zurücksetzen von Kennwörtern oder SSH-Schlüsseln sowie zum Verwalten von Benutzern finden Sie [hier](virtual-machines-linux-using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

