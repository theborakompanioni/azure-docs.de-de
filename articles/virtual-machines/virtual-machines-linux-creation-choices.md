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
ms.date: 01/03/2016
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: 44c46fff9ccf9c7dba9ee380faf5f8213b58e3c3
ms.openlocfilehash: 4397d84ef4d97bdee387777a193ec0b969f2d5e1


---
# <a name="different-ways-to-create-a-linux-vm-including-azure-cli-20-preview"></a>Verschiedene Möglichkeiten zum Erstellen eines virtuellen Linux-Computers, u.a. mithilfe der Azure-CLI 2.0 (Vorschau)
In Azure können Sie einen virtuellen Linux-Computer mit Ihren bevorzugten Tools und Workflows erstellen. Dieser Artikel fasst die Unterschiede und Beispiele für die Erstellung virtueller Linux-Computer zusammen.

## <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

Führen Sie die Aufgabe mit einer der folgenden CLI-Versionen durch:

- Azure-CLI 1.0: Unsere CLI für das klassische Bereitstellungsmodell und das Resource Manager-Bereitstellungsmodell
- [Azure CLI 2.0 (Vorschau):](../xplat-cli-install.md) Unsere CLI der nächsten Generation für das Resource Manager-Bereitstellungsmodell

Azure-CLI 2.0 (Vorschau) ist plattformübergreifend über ein npm-Paket, per Distributionspaket oder per Docker-Container verfügbar. Achten Sie darauf, dass Sie mit **az login** angemeldet sind.

In den folgenden Tutorials wird die Verwendung der Azure-CLI 2.0 (Vorschau) anhand von Beispielen veranschaulicht. Jeder Artikel enthält ausführliche Informationen zu den gezeigten Befehlen:

* [Erstellen eines virtuellen Linux-Computers mithilfe der Azure-Befehlszeilenschnittstelle 2.0 (Vorschau)](virtual-machines-linux-quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
  
  * Dieses Beispiel erstellt eine Ressourcengruppe namens „myResourceGroup“: 
    
    ```azurecli
    az group create -n myResourceGroup -l westus
    ```

  * Dieses Beispiel erstellt einen virtuellen Computer in der neuen Ressourcengruppe. Dabei wird das aktuelle Debian-Image mit einem öffentlichen Schlüssel mit dem Namen `id_rsa.pub` verwendet:

    ```azurecli
    az vm create \
    --image credativ:Debian:8:latest \
    --admin-username ops \
    --ssh-key-value ~/.ssh/id_rsa.pub \
    --public-ip-address-dns-name mydns \
    --resource-group myResourceGroup \
    --location westus \
    --name myVM
    ```

* [Erstellen einer geschützten Linux-VM mit einer Azure-Vorlage](virtual-machines-linux-create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
  
  * Das folgende Beispiel erstellt einen virtuellen Computer unter Verwendung einer auf GitHub gespeicherten Vorlage:
    
    ```azurecli
    az group deployment create -g myResourceGroup \ 
      --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json \
      --parameters @myparameters.json
    ```
    
* [Erstellen einer vollständigen Linux-Umgebung über die Azure-Befehlszeilenschnittstelle](virtual-machines-linux-create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
  
  * Beinhaltet das Erstellen eines Lastenausgleichs und mehrerer virtueller Computer in einer Verfügbarkeitsgruppe.

* [Hinzufügen eines Datenträgers zu einem virtuellen Linux-Computer](virtual-machines-linux-add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
  
  * Das folgende Beispiel fügt einem vorhandenen virtuellen Computer namens `myVM`einen 5-GB-Datenträger hinzu:
    
    ```azurecli
    az vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
      --disk-size 5 --vhd https://myStorage.blob.core.windows.net/vhds/myDataDisk1.vhd
    ```

## <a name="azure-portal"></a>Azure-Portal
Über das [Azure-Portal](https://portal.azure.com) können Sie schnell einen virtuellen Computer erstellen, da bei dieser Variante nichts auf Ihrem System installiert werden muss. Verwenden Sie das Azure-Portal, um den virtuellen Computer zu erstellen:

* [Erstellen einer Linux-VM mit dem Azure-Portal](virtual-machines-linux-quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) 
* [Anfügen eines Datenträgers mit dem Azure-Portal](virtual-machines-linux-attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="operating-system-and-image-choices"></a>Betriebssystem und Image-Optionen
Beim Erstellen einer VM muss ein für das auszuführende Betriebssystem geeignetes Image gewählt werden. Azure und seine Partner bieten zahlreiche Images an – einige mit bereits vorinstallierten Anwendungen und Tools. Alternativ können Sie auch Ihre eigenen Images hochladen (wie [im folgenden Abschnitt](#use-your-own-image)beschrieben).

### <a name="azure-images"></a>Azure-Images
Mithilfe der CLI-Befehle vom Typ `az vm image` können Sie verfügbare Elemente nach Herausgeber, Distributionsversion und Build anzeigen.

Liste mit den verfügbaren Herausgebern:

```azurecli
az vm image list-publishers -l WestUS
```

Liste mit den verfügbaren Produkten (Angeboten) für einen Herausgeber:

```azurecli
az vm image list-offers --publisher-name Canonical -l WestUS
```

Liste mit den verfügbaren SKUs (Distributionsversionen) eines Angebots:

```azurecli
az vm image list-skus --publisher-name Canonical --offer UbuntuServer -l WestUS
```

Liste mit allen verfügbaren Images für eine bestimmte Version:

```azurecli
az vm image list --publisher Canonical --offer UbuntuServer --sku 16.04.0-LTS -l WestUS
```

Weitere Beispiele für das Durchsuchen und Verwenden von verfügbaren Images finden Sie unter [Auswählen von Linux-VM-Images mit der Azure-CLI](virtual-machines-linux-cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Der Befehl `az vm create` verfügt über Aliase, die Sie zum schnellen Zugreifen auf die gängigeren Distributionen und ihre neuesten Versionen verwenden können. Das geht häufig schneller als bei jeder Erstellung eines virtuellen Computers den Herausgeber, das Angebot, die SKU und die Version anzugeben:

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
  
  * Beispielbefehle zum Erfassen eines vorhandenen virtuellen Computers:
    
    ```azurecli
    az vm deallocate -g myResourceGroup -n myVM
    az vm generalize -g myResourceGroup -n myVM
    az vm capture -g myResourceGroup -n myVM --vhd-name-prefix myCapturedVM
    ```

## <a name="next-steps"></a>Nächste Schritte
* Erstellen Sie einen virtuellen Linux-Computer über das [Portal](virtual-machines-linux-quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), mit der [Befehlszeilenschnittstelle](virtual-machines-linux-quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) oder mithilfe einer [Azure Resource Manager-Vorlage](virtual-machines-linux-cli-deploy-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Nach dem Erstellen eines virtuellen Linux-Computers können Sie [einen Datenträger hinzufügen](virtual-machines-linux-add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Schritte zum schnellen Zurücksetzen von Kennwörtern oder SSH-Schlüsseln sowie zum Verwalten von Benutzern finden Sie [hier](virtual-machines-linux-using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)




<!--HONumber=Jan17_HO1-->


