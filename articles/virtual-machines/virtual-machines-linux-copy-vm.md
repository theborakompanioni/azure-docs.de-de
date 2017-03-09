---
title: Kopieren einer Linux-VM mit Azure CLI 2.0 | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mithilfe von Azure CLI 2.0 (Vorschau) eine Kopie Ihrer Azure Linux-VM mit dem Resource Manager-Bereitstellungsmodell erstellen.
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
ms.date: 02/02/2017
ms.author: cynthn
translationtype: Human Translation
ms.sourcegitcommit: 1beaca707bfc5058083213b564b3898545f57556
ms.openlocfilehash: edf98951d4ed6709a9894e36ec5ae80b589dd639
ms.lasthandoff: 02/27/2017


---
# <a name="create-a-copy-of-a-linux-vm-by-using-azure-cli-20-preview"></a>Erstellen einer Kopie einer Linux-VM mit Azure CLI 2.0 (Vorschau)
In diesem Artikel erfahren Sie, wie Sie eine Kopie des virtuellen Azure-Computers (VM) unter Linux mit dem Azure Resource Manager-Bereitstellungsmodell erstellen.

Kopieren Sie die Datenträger für das Betriebssystem und die Daten in einen neuen Container, und richten Sie dann die Netzwerkressourcen ein, und erstellen Sie die VM.

Sie können auch [einen virtuellen Computer aus einem benutzerdefinierten Datenträgerimage hochladen und erstellen](virtual-machines-linux-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="cli-versions"></a>CLI-Versionen
Für diese Aufgabe können Sie eine der folgenden Versionen der Befehlszeilenschnittstelle (CLI) verwenden:

* [Azure CLI 1.0](virtual-machines-linux-copy-vm-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json): Für das klassische Bereitstellungsmodell und das Resource Manager-Bereitstellungsmodell
* Azure CLI 2.0 (Vorschau): Befehlszeilenschnittstelle der nächsten Generation für das Resource Manager-Bereitstellungsmodell (in diesem Artikel beschrieben)

## <a name="prerequisites"></a>Voraussetzungen
* Installation von [Azure CLI 2.0 (Vorschau)](/cli/azure/install-az-cli2) und Anmeldung an einem Azure-Konto mit [az login](/cli/azure/#login)
* Azure-VM als Quelle für die Kopie

## <a name="step-1-stop-the-source-vm"></a>Schritt 1: Beenden des virtuellen Quellcomputers
Heben Sie die Zuordnung des virtuellen Quellcomputers mit [az vm deallocate](/cli/azure/vm#deallocate) auf. Im folgenden Beispiel wird die Zuordnung für den virtuellen Computer `myVM` in der Ressourcengruppe `myResourceGroup` aufgehoben:

```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

## <a name="step-2-copy-the-source-vm"></a>Schritt 2: Kopieren des virtuellen Quellcomputers
Zum Kopieren eines virtuellen Computers erstellen Sie eine Kopie der zugrunde liegenden virtuellen Festplatte. Mit diesem Prozess erstellen Sie einen spezialisierten virtuellen Computer, der die gleiche Konfiguration und die gleichen Einstellungen wie der virtuelle Quellcomputer aufweist.

Der Prozess zum Kopieren eines virtuellen Datenträgers unterscheidet sich für Azure Managed Disks und nicht verwaltete Datenträger. Verwaltete Datenträger (Managed Disks) werden von der Azure-Plattform verarbeitet und erfordern weder Vorbereitungsschritte noch einen Speicherort. Da es sich bei verwalteten Datenträgern um Ressourcen der obersten Ebene handelt, ist ihre Nutzung einfacher. Sie können eine direkte Kopie der Datenträgerressource erstellen.

Weitere Informationen zu Azure Managed Disks finden Sie in der [Übersicht über Azure Managed Disks](../storage/storage-managed-disks-overview.md).

Befolgen Sie je nach Speichertyp Ihrer Quell-VM die Anleitungen in den nächsten beiden Abschnitten, und fahren Sie dann mit „Schritt 3: Einrichten eines virtuellen Netzwerks“ fort.

### <a name="managed-disks"></a>Verwaltete Datenträger

1. Listen Sie die einzelnen virtuellen Computer und den Namen des dazugehörigen vom Betriebssystem verwalteten Datenträgers mit [az vm list](/cli/azure/vm#list) auf. Im folgenden Beispiel werden alle virtuellen Computer der Ressourcengruppe `myResourceGroup` aufgelistet:

    ```azurecli
    az vm list -g myTestRG --query '[].{Name:name,DiskName:storageProfile.osDisk.name}' --output table
    ```

    Die Ausgabe sieht in etwa wie das folgende Beispiel aus:

    ```azurecli
    Name    DiskName
    ------  --------
    myVM    myDisk
```

2. Erstellen Sie zum Kopieren des Datenträgers mit [az disk create](/cli/azure/disk#create) einen neuen verwalteten Datenträger. Im folgenden Beispiel wird der Datenträger `myCopiedDisk` auf Grundlage des verwalteten Datenträgers `myDisk` erstellt:

    ```azurecli
    az disk create --resource-group myResourceGroup --name myCopiedDisk --source myDisk
    ```

3. Überprüfen Sie die verwalteten Datenträger jetzt in der Ressourcengruppe mit [az disk list](/cli/azure/disk#list). Im folgenden Beispiel werden alle verwalteten Datenträger der Ressourcengruppe `myResourceGroup` aufgelistet:

    ```azurecli
    az disk list --resource-group myResourceGroup --output table
    ```

4. Fahren Sie mit [„Schritt 3: Einrichten eines virtuellen Netzwerks“](#set-up-the-virtual-network) fort.


### <a name="unmanaged-disks"></a>Nicht verwaltete Datenträger

1. Zum Erstellen der Kopie einer virtuellen Festplatte benötigen Sie die Azure-Speicherkontoschlüssel und den URI des Datenträgers. Verwenden Sie [az storage account keys list](/cli/azure/storage/account/keys#list), um die Speicherkontoschlüssel anzuzeigen.

 Im folgenden Beispiel werden die Schlüssel für das Speicherkonto `mystorageaccount` in der Ressourcengruppe `myResourceGroup` aufgelistet:

    ```azurecli
    az storage account keys list --resource-group myResourceGroup \
        --name mystorageaccount --output table
    ```

 Die Ausgabe sieht in etwa wie das folgende Beispiel aus:

    ```azurecli
    KeyName    Permissions    Value
    ---------  -------------  ----------------------------------------------------------------------------------------
    key1       Full           gi7crXhD8PMs8DRWqAM7fAjQEFmENiGlOprNxZGbnpylsw/nq+lQQg0c4jiKoV3Nytr3dAiXZRpL8jflpAr2Ug==
    key2       Full           UMoyQjWuKCBUAVDr1ANRe/IWTE2o0ZdmQH2JSZzXKNmDKq83368Fw9nyTBcTEkSKI9cm5tlTL8J15ArbPMo8eA==
    ```

2. Verwenden Sie [az vm list](/cli/azure/vm#list), um eine Liste mit VMs und den dazugehörigen URIs anzuzeigen. Im folgenden Beispiel werden die virtuellen Computer der Ressourcengruppe `myResourceGroup` aufgelistet:

    ```azurecli
    az vm list -g myResourceGroup --query '[].{Name:name,URI:storageProfile.osDisk.vhd.uri}' --output table
    ```

    Die Ausgabe sieht in etwa wie das folgende Beispiel aus:

    ```azurecli
    Name    URI
    ------  -------------------------------------------------------------
    myVM    https://mystorageaccount.blob.core.windows.net/vhds/myVHD.vhd
    ```

3. Kopieren Sie die virtuelle Festplatte mit [az storage blob copy start](/cli/azure/storage/blob/copy#start). Verwenden Sie die Informationen aus den Listen von `az storage account keys` und `az vm`, um die erforderlichen Speicherkontoschlüssel und den URI der virtuellen Festplatte anzugeben.

    ```azurecli
    az storage blob copy start \
        --account-name mystorageaccount \
        --account-key gi7crXhD8PMs8DRWqAM7fAjQEFmENiGlOprNxZGbnpylsw/nq+lQQg0c4jiKoV3Nytr3dAiXZRpL8jflpAr2Ug== \
        --source-uri https://mystorageaccount.blob.core.windows.net/vhds/myVHD.vhd \
        --destination-container vhds --destination-blob myCopiedVHD.vhd
    ```

## <a name="step-3-set-up-a-virtual-network"></a>Schritt 3: Einrichten eines virtuellen Netzwerks
Mit den folgenden optionalen Schritten erstellen Sie ein neues virtuelles Netzwerk, ein neues Subnetz, eine neue öffentliche IP-Adresse und eine neue virtuelle Netzwerkschnittstellenkarte (Network Interface Card, NIC).

Wenn Sie einen virtuellen Computer zur Problembehandlung oder für zusätzliche Bereitstellungen kopieren, kann es sein, dass Sie keinen virtuellen Computer in einem vorhandenen virtuellen Netzwerk verwenden möchten.

Führen Sie die nächsten Schritte aus, wenn Sie eine VM-Infrastruktur für Ihre kopierten VMs erstellen möchten. Falls Sie kein virtuelles Netzwerk erstellen möchten, können Sie mit [„Schritt 4: Erstellen eines virtuellen Computers“](#create-a-vm) fortfahren.

1. Erstellen Sie das virtuelle Netzwerk mit [az network vnet create](/cli/azure/network/vnet#create). Im folgenden Beispiel werden das virtuelle Netzwerk `myVnet` und das Subnetz `mySubnet` erstellt:

    ```azurecli
    az network vnet create --resource-group myResourceGroup --location westus --name myVnet \
        --address-prefix 192.168.0.0/16 --subnet-name mySubnet --subnet-prefix 192.168.1.0/24
    ```

2. Erstellen Sie mit [az network public-ip create](/cli/azure/network/public-ip#create) eine öffentliche IP-Adresse. Im folgenden Beispiel wird die öffentliche IP-Adresse `myPublicIP` mit dem DNS-Namen `mypublicdns` erstellt. (Der DNS-Name muss eindeutig sein. Geben Sie daher einen eindeutigen Namen an.)

    ```azurecli
    az network public-ip create --resource-group myResourceGroup --location westus \
        --name myPublicIP --dns-name mypublicdns --allocation-method static --idle-timeout 4
    ```

3. Erstellen Sie die NIC mit [az network nic create](/cli/azure/network/nic#create). Im folgenden Beispiel wird die NIC `myNic` erstellt, die an das Subnetz `mySubnet` angefügt ist:

    ```azurecli
    az network nic create --resource-group myResourceGroup --location westeurope --name myNic \
        --vnet-name myVnet --subnet mySubnet
    ```

## <a name="step-4-create-a-vm"></a>Schritt 4: Erstellen eines virtuellen Computers
Sie können nun mit [az vm create](/cli/azure/vm#create) einen virtuellen Computer erstellen. Wie beim Kopieren eines Datenträgers auch, unterscheidet sich der Prozess für verwaltete und nicht verwaltete Datenträger leicht. Führen Sie je nach Speichertyp Ihres virtuellen Quellcomputers die Schritte der Anleitung in einem der nächsten beiden Abschnitte aus.

### <a name="managed-disks"></a>Verwaltete Datenträger
1. Erstellen Sie mit [az vm create](/cli/azure/vm#create) eine VM.
2. Geben Sie wie folgt den kopierten verwalteten Datenträger an, der als Betriebssystemdatenträger (`--attach-os-disk`) verwendet werden soll:

    ```azurecli
    az vm create --resource-group myResourceGroup --name myCopiedVM \
        --admin-username azureuser --ssh-key-value ~/.ssh/id_rsa.pub \
        --nics myNic --size Standard_DS1_v2 --os-type Linux \
        --image UbuntuLTS
        --attach-os-disk myCopiedDisk
    ```

### <a name="unmanaged-disks"></a>Nicht verwaltete Datenträger
1. Erstellen Sie mit [az vm create](/cli/azure/vm#create) einen virtuellen Computer.
2. Geben Sie wie folgt das Speicherkonto, den Containernamen und die virtuelle Festplatte an, die Sie beim Erstellen des kopierten Datenträgers mit `az storage blob copy start` (`--image`) verwendet haben:

    ```azurecli
    az vm create --resource-group myResourceGroup --name myCopiedVM  \
        --admin-username azureuser --ssh-key-value ~/.ssh/id_rsa.pub \
        --nics myNic --size Standard_DS1_v2 --os-type Linux \
        --image https://mystorageaccount.blob.core.windows.net/vhds/myCopiedVHD.vhd \
        --use-unmanaged-disk
    ```

## <a name="next-steps"></a>Nächste Schritte
Informationen zur Verwendung der Azure-CLI für die Verwaltung des neuen virtuellen Computers finden Sie unter [Befehle der Azure-Befehlszeilenschnittstelle im Resource Manager-Modus](azure-cli-arm-commands.md).

