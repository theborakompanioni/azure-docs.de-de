--- 
title: Kopieren einer Linux-VM mit Azure CLI 2.0 | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine Kopie Ihrer Azure Linux-VM mithilfe von Azure CLI 2.0 und Managed Disks erstellen.
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
tags: azure-resource-manager
ms.assetid: 770569d2-23c1-4a5b-801e-cddcd1375164
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 03/10/2017
ms.author: cynthn
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: 7983061a933370803669480296d7625106e1360c
ms.contentlocale: de-de
ms.lasthandoff: 08/21/2017

---                    
               
# <a name="create-a-copy-of-a-linux-vm-by-using-azure-cli-20-and-managed-disks"></a>Erstellen einer Kopie Ihrer Linux-VM mithilfe von Azure CLI 2.0 und Managed Disks


In diesem Artikel erfahren Sie, wie Sie eine Kopie des virtuellen Azure-Computers (VM) unter Linux mit Azure CLI 2.0 und dem Azure Resource Manager-Bereitstellungsmodell erstellen. Sie können diese Schritte auch mit [Azure CLI 1.0](copy-vm-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) ausführen.

Sie können auch [einen virtuellen Computer aus einer VHD hochladen und erstellen](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="prerequisites"></a>Voraussetzungen


-   Installieren Sie [Azure CLI 2.0](/cli/azure/install-az-cli2).

-   Melden Sie sich mit [az login](/cli/azure/#login) bei einem Azure-Konto an.

-   Halten Sie eine Azure-VM zur Verwendung als Quelle für die Kopie bereit.

## <a name="step-1-stop-the-source-vm"></a>Schritt 1: Beenden des virtuellen Quellcomputers


Heben Sie die Zuordnung des virtuellen Quellcomputers mit [az vm deallocate](/cli/azure/vm#deallocate) auf.
Im folgenden Beispiel wird die Zuordnung für die VM **myVM** in der Ressourcengruppe **myResourceGroup** aufgehoben:

```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

## <a name="step-2-copy-the-source-vm"></a>Schritt 2: Kopieren des virtuellen Quellcomputers


Zum Kopieren eines virtuellen Computers erstellen Sie eine Kopie der zugrunde liegenden virtuellen Festplatte. Dieser Prozess erstellt eine spezialisierte VHD als Managed Disk, die die gleiche Konfiguration und die gleichen Einstellungen wie der virtuelle Quellcomputer aufweist.

Weitere Informationen zu Azure Managed Disks finden Sie in der [Übersicht über Azure Managed Disks](../windows/managed-disks-overview.md). 

1.  Listen Sie die einzelnen virtuellen Computer und den Namen des dazugehörigen Betriebssystemdatenträgers mit [az vm list](/cli/azure/vm#list) auf. Im folgenden Beispiel werden alle virtuellen Computer in der Ressourcengruppe **myResourceGroup** aufgelistet:
    
    ```azurecli
    az vm list -g myTestRG --query '[].{Name:name,DiskName:storageProfile.osDisk.name}' --output table
    ```

    Die Ausgabe sieht in etwa wie das folgende Beispiel aus:

    ```azurecli
    Name    DiskName
    ------  --------
    myVM    myDisk
    ```

1.  Kopieren Sie den Datenträger, indem Sie einen neuen verwalteten Datenträger mit [az disk create](/cli/azure/disk#create) erstellen. Im folgenden Beispiel wird ein Datenträger mit dem Namen **myCopiedDisk** auf Grundlage des verwalteten Datenträgers **myDisk** erstellt:

    ```azurecli
    az disk create --resource-group myResourceGroup --name myCopiedDisk --source myDisk
    ``` 

1.  Überprüfen Sie die verwalteten Datenträger jetzt in der Ressourcengruppe mit [az disk list](/cli/azure/disk#list). Im folgenden Beispiel werden alle verwalteten Datenträger in der Ressourcengruppe **myResourceGroup** aufgelistet:

    ```azurecli
    az disk list --resource-group myResourceGroup --output table
    ```

1.  Fahren Sie mit [„Schritt 3: Einrichten eines virtuellen Netzwerks“](#step-3-set-up-a-virtual-network) fort.


## <a name="step-3-set-up-a-virtual-network"></a>Schritt 3: Einrichten eines virtuellen Netzwerks


Mit den folgenden optionalen Schritten erstellen Sie ein neues virtuelles Netzwerk, ein neues Subnetz, eine neue öffentliche IP-Adresse und eine neue virtuelle Netzwerkschnittstellenkarte (Network Interface Card, NIC).

Wenn Sie einen virtuellen Computer zur Problembehandlung oder für zusätzliche Bereitstellungen kopieren, kann es sein, dass Sie keinen virtuellen Computer in einem vorhandenen virtuellen Netzwerk verwenden möchten.

Führen Sie die nächsten Schritte aus, wenn Sie eine VM-Infrastruktur für Ihre kopierten VMs erstellen möchten. Falls Sie kein virtuelles Netzwerk erstellen möchten, können Sie mit [Schritt 4: Erstellen eines virtuellen Computers](#step-4-create-a-vm) fortfahren.

1.  Erstellen Sie das virtuelle Netzwerk mit [az network vnet create](/cli/azure/network/vnet#create). Im folgenden Beispiel werden ein virtuelles Netzwerk mit dem Namen **myVnet** und ein Subnetz mit dem Namen **mySubnet** erstellt:

    ```azurecli
    az network vnet create --resource-group myResourceGroup --location westus --name myVnet \
        --address-prefix 192.168.0.0/16 --subnet-name mySubnet --subnet-prefix 192.168.1.0/24
    ```

1.  Erstellen Sie mit [az network public-ip create](/cli/azure/network/public-ip#create) eine öffentliche IP-Adresse. Im folgenden Beispiel wird eine öffentliche IP-Adresse namens **myPublicIP** mit dem DNS-Namen **mypublicdns** erstellt. (Der DNS-Name muss eindeutig sein. Geben Sie daher einen eindeutigen Namen an.)

    ```azurecli
    az network public-ip create --resource-group myResourceGroup --location westus \
        --name myPublicIP --dns-name mypublicdns --allocation-method static --idle-timeout 4
    ```

1.  Erstellen Sie die NIC mit [az network nic create](/cli/azure/network/nic#create).
    Im folgenden Beispiel wird die NIC **myNic** erstellt, die an das Subnetz **mySubnet** angefügt ist:

    ```azurecli
    az network nic create --resource-group myResourceGroup --location westus --name myNic \
        --vnet-name myVnet --subnet mySubnet --public-ip-address myPublicIP
    ```

## <a name="step-4-create-a-vm"></a>Schritt 4: Erstellen eines virtuellen Computers

Sie können nun mit [az vm create](/cli/azure/vm#create) einen virtuellen Computer erstellen.

Geben Sie wie folgt den kopierten verwalteten Datenträger an, der als Betriebssystemdatenträger (--attach-os-disk) verwendet werden soll:

```azurecli
az vm create --resource-group myResourceGroup --name myCopiedVM \
    --admin-username azureuser --ssh-key-value ~/.ssh/id_rsa.pub \
    --nics myNic --size Standard_DS1_v2 --os-type Linux \
    --attach-os-disk myCopiedDisk
```

## <a name="next-steps"></a>Nächste Schritte

Informationen zur Verwendung der Azure-CLI für die Verwaltung des neuen virtuellen Computers finden Sie unter [Befehle der Azure-Befehlszeilenschnittstelle im Resource Manager-Modus](../azure-cli-arm-commands.md).

