---
title: "Verschiedene Möglichkeiten zum Erstellen einer Linux-VM mit Azure CLI 1.0 | Microsoft-Dokumentation"
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
ms.topic: 
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/11/2017
ms.author: iainfou
ms.translationtype: HT
ms.sourcegitcommit: 818f7756189ed4ceefdac9114a0b89ef9ee8fb7a
ms.openlocfilehash: 1eb90d44797d66f3e09811918ce5a7f4ad4287c6
ms.contentlocale: de-de
ms.lasthandoff: 07/14/2017

---
# <a name="different-ways-to-create-a-linux-virtual-machine-in-azure"></a>Verschiedene Möglichkeiten zum Erstellen eines virtuellen Linux-Computers in Azure
In Azure können Sie einen virtuellen Linux-Computer mit Ihren bevorzugten Tools und Workflows erstellen. Dieser Artikel fasst die Unterschiede und Beispiele für die Erstellung virtueller Linux-Computer zusammen.

## <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle
Sie können virtuelle Computer in Azure mithilfe einer der folgenden CLI-Versionen erstellen:

- Azure-CLI 1.0: Unsere CLI für das klassische Bereitstellungsmodell und das Resource Manager-Bereitstellungsmodell (in diesem Artikel)
- [Azure CLI 2.0:](../windows/creation-choices.md) Unsere CLI der nächsten Generation für das Resource Manager-Bereitstellungsmodell

Die Azure CLI 1.0 ist plattformübergreifend über ein npm-Paket, per Distributionspaket oder per Docker-Container verfügbar. Weitere Informationen finden Sie unter [How to install and configure the Azure CLI](../../cli-install-nodejs.md)(Installieren und Konfigurieren der Azure-Befehlszeilenschnittstelle). In den folgenden Tutorials wird die Verwendung der Azure CLI 1.0 anhand von Beispielen veranschaulicht. Jeder Artikel enthält ausführliche Informationen zu den CLI-Schnellstartbefehlen:

* [Erstellen eines virtuellen Linux-Computers in Azure mithilfe der Befehlszeilenschnittstelle](quick-create-cli-nodejs.md)
  
  * Im folgenden Beispiel wird ein virtueller CoreOS-Computer mit dem öffentlichen Schlüssel *azure_id_rsa.pub* erstellt:
    
    ```azurecli
    azure vm quick-create -ssh-publickey-file ~/.ssh/azure_id_rsa.pub \
      --image-urn CoreOS
    ```
* [Erstellen einer geschützten Linux-VM mit einer Azure-Vorlage](create-ssh-secured-vm-from-template.md)
  
  * Das folgende Beispiel erstellt einen virtuellen Computer unter Verwendung einer auf GitHub gespeicherten Vorlage:
    
    ```azurecli
    azure group create --name myResourceGroup --location eastus 
      --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
    ```
* [Erstellen einer vollständigen Linux-Umgebung über die Azure-Befehlszeilenschnittstelle](create-cli-complete-nodejs.md)
  
  * Beinhaltet das Erstellen eines Lastenausgleichs und mehrerer virtueller Computer in einer Verfügbarkeitsgruppe.
* [Hinzufügen eines Datenträgers zu einem virtuellen Linux-Computer](add-disk.md)
  
  * Im folgenden Beispiel wird der VM *myVM* ein Datenträger mit *5* GB hinzugefügt:
    
    ```azurecli
    azure vm disk attach-new \
        --resource-group myResourceGroup \
        --vm-name myVM \
        --size-in-GB 5
    ```

## <a name="azure-portal"></a>Azure-Portal
Über das [Azure-Portal](https://portal.azure.com) können Sie schnell einen virtuellen Computer erstellen, da bei dieser Variante nichts auf Ihrem System installiert werden muss. Verwenden Sie das Azure-Portal, um den virtuellen Computer zu erstellen:

* [Erstellen einer Linux-VM mit dem Azure-Portal](quick-create-portal.md) 

## <a name="operating-system-and-image-choices"></a>Betriebssystem und Image-Optionen
Beim Erstellen einer VM muss ein für das auszuführende Betriebssystem geeignetes Image gewählt werden. Azure und seine Partner bieten zahlreiche Images an – einige mit bereits vorinstallierten Anwendungen und Tools. Alternativ können Sie auch Ihre eigenen Images hochladen (wie [im folgenden Abschnitt](#use-your-own-image)beschrieben).

### <a name="azure-images"></a>Azure-Images
Mithilfe der CLI-Befehle vom Typ `azure vm image` können Sie verfügbare Elemente nach Herausgeber, Distributionsversion und Build anzeigen.

So listen Sie die verfügbaren Herausgeber auf:

```azurecli
azure vm image list-publishers --location eastus
```

So listen Sie die verfügbaren Produkte (Angebote) für einen Herausgeber auf:

```azurecli
azure vm image list-offers --location eastus --publisher Canonical
```

So listen Sie die verfügbaren SKUs (Distributionsversionen) eines Angebots auf:

```azurecli
azure vm image list-skus --location eastus --publisher Canonical --offer UbuntuServer
```

So listen Sie alle verfügbaren Images für eine bestimmte Version auf:

```azurecli
azure vm image list --location eastus --publisher Canonical --offer UbuntuServer --sku 16.04.0-LTS
```

Die Befehle `azure vm quick-create` und `azure vm create` verfügen über Aliase, die Sie zum schnellen Zugreifen auf die gängigeren Distributionen und ihre neuesten Versionen verwenden können. Das geht häufig schneller als bei jeder Erstellung eines virtuellen Computers den Herausgeber, das Angebot, die SKU und die Version anzugeben:

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

* [Von Azure unterstützte Distributionen](endorsed-distros.md)
* [Informationen zu nicht unterstützten Distributionen](create-upload-generic.md)
* [Hochladen und Erstellen eines virtuellen Linux-Computers aus einem benutzerdefinierten Datenträgerimage](upload-vhd.md)
* [Erfassen eines virtuellen Linux-Computers zur Verwendung als Resource Manager-Vorlage](capture-image.md)
  
  * Beispielbefehle zum Erfassen eines vorhandenen virtuellen Computers:
    
    ```azurecli
    azure vm deallocate --resource-group myResourceGroup --vm-name myVM
    azure vm generalize --resource-group myResourceGroup --vm-name myVM
    azure vm capture --resource-group myResourceGroup --vm-name myVM --vhd-name-prefix myCapturedVM
    ```

## <a name="next-steps"></a>Nächste Schritte
* Erstellen Sie einen virtuellen Linux-Computer über das [Portal](quick-create-portal.md), mit der [Befehlszeilenschnittstelle](quick-create-cli.md) oder mithilfe einer [Azure Resource Manager-Vorlage](../windows/cli-deploy-templates.md).
* Nach dem Erstellen eines virtuellen Linux-Computers können Sie [einen Datenträger hinzufügen](add-disk.md).
* Schritte zum schnellen Zurücksetzen von Kennwörtern oder SSH-Schlüsseln sowie zum Verwalten von Benutzern finden Sie [hier](using-vmaccess-extension.md)


