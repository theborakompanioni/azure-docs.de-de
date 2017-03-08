---
title: Erstellen einer Kopie des virtuellen Linux-Computers mithilfe von Azure CLI 1.0 | Microsoft Docs
description: Erfahren Sie, wie Sie mithilfe von Azure CLI 1.0 eine Kopie eines virtuellen Azure-Computers unter Linux im Resource Manager-Bereitstellungsmodell erstellen.
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
tags: azure-resource-manager
ms.assetid: 770569d2-23c1-4a5b-801e-cddcd1375164
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/28/2016
ms.author: cynthn
translationtype: Human Translation
ms.sourcegitcommit: 6fda4b6e77104b6022b86010b53b46ae5df1b82e
ms.openlocfilehash: e85094fbb63b5cf0c5f3b080e047dbc3b8561a26
ms.lasthandoff: 02/27/2017


---
# <a name="create-a-copy-of-a-linux-virtual-machine-running-on-azure-with-the-azure-cli-10"></a>Erstellen einer Kopie eines virtuellen Linux-Computers, der in Azure ausgeführt wird, mithilfe von Azure CLI 1.0
In diesem Artikel erfahren Sie, wie Sie eine Kopie des virtuellen Azure-Computers (VM) unter Linux im Resource Manager-Bereitstellungsmodell erstellen können. Kopieren Sie zuerst die Datenträger für das Betriebssystem und die Daten in einen neuen Container, richten Sie dann die Netzwerkressourcen ein, und erstellen Sie den neuen virtuellen Computer.

Sie können auch [einen virtuellen Computer aus einem benutzerdefinierten Datenträgerimage hochladen und erstellen](virtual-machines-linux-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="cli-versions-to-complete-the-task"></a>CLI-Versionen zum Durchführen dieser Aufgabe
Führen Sie die Aufgabe mit einer der folgenden CLI-Versionen durch:

- Azure-CLI 1.0: Unsere CLI für das klassische Bereitstellungsmodell und das Resource Manager-Bereitstellungsmodell (in diesem Artikel)
- [Azure CLI 2.0:](virtual-machines-linux-copy-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Unsere CLI der nächsten Generation für das Resource Manager-Bereitstellungsmodell

## <a name="before-you-begin"></a>Voraussetzungen
Stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind, bevor Sie mit den Schritten beginnen:

* Sie haben die [Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md) heruntergeladen und auf Ihrem Computer installiert. 
* Außerdem benötigen Sie einige Informationen zu Ihrem vorhandenen virtuellen Azure-Computer unter Linux:

| Informationen zum virtuellen Quellcomputer | Ursprung |
| --- | --- |
| Name des virtuellen Computers |`azure vm list` |
| Ressourcengruppenname |`azure vm list` |
| Standort |`azure vm list` |
| Speicherkontoname |`azure storage account list -g <resourceGroup>` |
| Containername |`azure storage container list -a <sourcestorageaccountname>` |
| Name der VHD-Datei des virtuellen Quellcomputers |`azure storage blob list --container <containerName>` |

* Sie müssen Sie einige Entscheidungen für den neuen virtuellen Computer treffen:    <br> - Containername    <br> - Name des virtuellen Computers    <br> - Größe des virtuellen Computers    <br> - Name des virtuellen Netzwerks    <br> - Subnetzname    <br> - IP-Name    <br> – NIC-Name

## <a name="login-and-set-your-subscription"></a>Anmelden und Festlegen des Abonnements
1. Melden Sie sich bei der Befehlszeilenschnittstelle an.

    ```azurecli
    azure login
    ```
2. Stellen Sie sicher, dass Sie sich im Resource Manager-Modus befinden.

    ```azurecli
    azure config mode arm
    ```
3. Legen Sie das richtige Abonnement fest. In der Azure-Kontenliste werden all Ihre Abonnements angezeigt.

    ```azurecli
    azure account set mySubscriptionID
    ```

## <a name="stop-the-vm"></a>Beenden des virtuellen Computers
Beenden Sie den virtuellen Quellcomputer, und geben Sie ihn frei. Über die Liste der virtuellen Azure-Computer erhalten Sie eine Liste aller virtuellen Computer in Ihrem Abonnement und deren Ressourcengruppennamen.

```azurecli
azure vm stop myResourceGroup myVM
azure vm deallocate myResourceGroup MyVM
```


## <a name="copy-the-vhd"></a>Kopieren der VHD-Datei
Mit `azure storage blob copy start`können Sie die VHD-Datei aus dem Quellspeicher zum Ziel zu kopieren. In diesem Beispiel kopieren wir die VHD-Datei in dasselbe Speicherkonto, aber in einen anderen Container.

Um die VHD-Datei in einen anderen Container im selben Speicherkonto zu kopieren, geben Sie Folgendes ein:

```azurecli
azure storage blob copy start \
        https://mystorageaccountname.blob.core.windows.net:8080/mycontainername/myVHD.vhd \
        myNewContainerName
```

## <a name="set-up-the-virtual-network-for-your-new-vm"></a>Einrichten des virtuellen Netzwerks für den neuen virtuellen Computer
Richten Sie ein virtuelles Netzwerk und eine NIC für den neuen virtuellen Computer ein. 

```azurecli
azure network vnet create myResourceGroup myVnet -l myLocation

azure network vnet subnet create -a <address.prefix.in.CIDR/format> myResourceGroup myVnet mySubnet

azure network public-ip create myResourceGroup myPublicIP -l myLocation

azure network nic create myResourceGroup myNic -k mySubnet -m myVnet -p myPublicIP -l myLocation
```


## <a name="create-the-new-vm"></a>Erstellen des neuen virtuellen Computers
Nun können Sie auf der Grundlage der hochgeladenen virtuellen Festplatte [mithilfe einer Resource Manager-Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd) oder über die Befehlszeilenschnittstelle einen virtuellen Computer erstellen, indem Sie den URI des kopierten Datenträgers wie folgt angeben:

```azurecli
azure vm create -n myVM -l myLocation -g myResourceGroup -f myNic \
        -z Standard_DS1_v2 -y Linux \
        https://mystorageaccountname.blob.core.windows.net:8080/mycontainername/myVHD.vhd 
```



## <a name="next-steps"></a>Nächste Schritte
Informationen zur Verwendung der Azure-CLI für die Verwaltung des neuen virtuellen Computers finden Sie unter [Befehle der Azure-Befehlszeilenschnittstelle im Resource Manager-Modus](azure-cli-arm-commands.md).


