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
ms.devlang: azurecli
ms.topic: get-started-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/11/2017
ms.author: iainfou
ms.translationtype: Human Translation
ms.sourcegitcommit: 97fa1d1d4dd81b055d5d3a10b6d812eaa9b86214
ms.openlocfilehash: 756711abb014829971af126c5cb60c12e79c920e
ms.contentlocale: de-de
ms.lasthandoff: 05/11/2017


---
# <a name="different-ways-to-create-a-linux-vm"></a>Verschiedene Möglichkeiten zum Erstellen einer Linux-VM
In Azure können Sie einen virtuellen Linux-Computer mit Ihren bevorzugten Tools und Workflows erstellen. In diesem Artikel sind die Unterschiede und Beispiele für die Erstellung virtueller Linux-Computer zusammengefasst, einschließlich Azure CLI 2.0. Sie können auch Erstellungsoptionen anzeigen, einschließlich [Azure CLI 1.0](creation-choices-nodejs.md).

[Azure CLI 2.0](/cli/azure/install-az-cli2) ist plattformübergreifend über ein npm-Paket, per Distributionspaket oder per Docker-Container verfügbar. Installieren Sie den am besten geeigneten Build für Ihre Umgebung, und melden sich mit dem [AZ-Login](/cli/azure/#login) in einem Azure-Konto an.

* [Erstellen eines virtuellen Linux-Computers mit der Azure CLI 2.0](quick-create-cli.md)
  
  * Erstellen Sie mit [az group create](/cli/azure/group#create) eine Ressourcengruppe mit dem Namen *myResourceGroup*: 
   
    ```azurecli
    az group create --name myResourceGroup --location eastus
    ```
    
  * Erstellen Sie mit [az vm create](/cli/azure/vm#create) einen virtuellen Computer namens *myVM* mit dem aktuellen *UbuntuLTS*-Image, und generieren Sie SSH-Schlüssel (sofern bereits vorhanden) in *~/.ssh*:

    ```azurecli
    az vm create \
        --resource-group myResourceGroup \
        --name myVM \
        --image UbuntuLTS \
        --generate-ssh-keys
    ```

* [Erstellen eines virtuellen Linux-Computers mit einer Azure-Vorlage](create-ssh-secured-vm-from-template.md)
  
  * Im folgenden Beispiel wird [az group deployment create](/cli/azure/group/deployment#create) zum Erstellen eines virtuellen Computers aus einer Vorlage verwendet, die auf GitHub gespeichert ist:
    
    ```azurecli
    az group deployment create --resource-group myResourceGroup \ 
      --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json \
      --parameters @myparameters.json
    ```
* [Erstellen eines virtuellen Linux-Computers und Anpassen mit Cloud-Init](tutorial-automate-vm-deployment.md)

* [Erstellen einer hochverfügbaren Anwendung mit Lastenausgleich auf mehreren virtuellen Linux-Computern](tutorial-load-balancer.md)


## <a name="azure-portal"></a>Azure-Portal
Über das [Azure-Portal](https://portal.azure.com) können Sie schnell einen virtuellen Computer erstellen, da bei dieser Variante nichts auf Ihrem System installiert werden muss. Verwenden Sie das Azure-Portal, um den virtuellen Computer zu erstellen:

* [Erstellen einer Linux-VM mit dem Azure-Portal](quick-create-portal.md) 


## <a name="operating-system-and-image-choices"></a>Betriebssystem und Image-Optionen
Beim Erstellen einer VM muss ein für das auszuführende Betriebssystem geeignetes Image gewählt werden. Azure und seine Partner bieten zahlreiche Images an – einige mit bereits vorinstallierten Anwendungen und Tools. Alternativ können Sie auch Ihre eigenen Images hochladen (wie [im folgenden Abschnitt](#use-your-own-image)beschrieben).

### <a name="azure-images"></a>Azure-Images
Mithilfe der Befehle vom Typ [az vm image](/cli/azure/vm/image) können Sie verfügbare Elemente nach Herausgeber, Distributionsversion und Build anzeigen.

Liste mit den verfügbaren Herausgebern:

```azurecli
az vm image list-publishers --location eastus
```

Liste mit den verfügbaren Produkten (Angeboten) für einen Herausgeber:

```azurecli
az vm image list-offers --publisher Canonical --location eastus
```

Liste mit den verfügbaren SKUs (Distributionsversionen) eines Angebots:

```azurecli
az vm image list-skus --publisher Canonical --offer UbuntuServer --location eastus
```

Liste mit allen verfügbaren Images für eine bestimmte Version:

```azurecli
az vm image list --publisher Canonical --offer UbuntuServer --sku 16.04.0-LTS --location eastus
```

Weitere Beispiele für das Durchsuchen und Verwenden von verfügbaren Images finden Sie unter [Auswählen von Linux-VM-Images mit der Azure-CLI](cli-ps-findimage.md).

Der Befehl [az vm create](/cli/azure/vm#create) verfügt über Aliase, die Sie zum schnellen Zugreifen auf die gängigeren Distributionen und ihre neuesten Versionen verwenden können. Das geht häufig schneller als bei jeder Erstellung eines virtuellen Computers den Herausgeber, das Angebot, die SKU und die Version anzugeben:

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
Sollten Sie spezielle Anpassungen benötigen, verwenden Sie ein Image, das auf einem vorhandenen virtuellen Azure-Computer basiert. Hierzu können Sie diesen virtuellen Computer erfassen. Sie können auch ein lokal erstelltes Image hochladen. Weitere Informationen zu unterstützten Distributionen und zur Verwendung eigener Images finden Sie in den folgenden Artikeln:

* [Von Azure unterstützte Distributionen](endorsed-distros.md)
* [Informationen zu nicht unterstützten Distributionen](create-upload-generic.md)
* [Erstellen eines benutzerdefinierten Images eines virtuellen Azure-Computers mithilfe der Befehlszeilenschnittstelle](tutorial-custom-images.md)
  
  * Schnellstartbeispielbefehle zum Erstellen eines Images aus einem vorhandenen virtuellen Azure-Computer:
    
    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    az vm generalize --resource-group myResourceGroup --name myVM
    az vm image create --resource-group myResourceGroup --source myVM --name myImage
    ```

## <a name="next-steps"></a>Nächste Schritte
* Erstellen Sie mit der [CLI](quick-create-cli.md) einen virtuellen Linux-Computer über das [Portal](quick-create-portal.md) oder mithilfe einer [Azure Resource Manager-Vorlage](../windows/cli-deploy-templates.md).
* Nach dem Erstellen eines virtuellen Linux-Computers lesen Sie die Informationen zu [Azure-Datenträgern und -Speicher](tutorial-manage-disks.md).
* Schritte zum schnellen Zurücksetzen von Kennwörtern oder SSH-Schlüsseln sowie zum Verwalten von Benutzern finden Sie [hier](using-vmaccess-extension.md)

