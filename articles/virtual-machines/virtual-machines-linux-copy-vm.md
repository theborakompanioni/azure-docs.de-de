---
title: Kopieren eines virtuellen Linux-Computers mit der Azure CLI 2.0 (Vorschau) | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mithilfe der Azure CLI 2.0 (Vorschau) eine Kopie eines virtuellen Azure-Computers unter Linux im Resource Manager-Bereitstellungsmodell erstellen.
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
ms.sourcegitcommit: 344949f10929d9a1ba8f63c5b30f242ddc66fdec
ms.openlocfilehash: 967e2bafdc702c9ddba634089899afee382c5b71


---
# <a name="create-a-copy-of-a-linux-virtual-machine-with-the-azure-cli-20-preview"></a>Erstellen einer Kopie eines virtuellen Linux-Computers mit der Azure CLI 2.0 (Vorschau)
In diesem Artikel erfahren Sie, wie Sie eine Kopie des virtuellen Azure-Computers (VM) unter Linux im Resource Manager-Bereitstellungsmodell erstellen können. Kopieren Sie zuerst die Datenträger für das Betriebssystem und die Daten in einen neuen Container, richten Sie dann die Netzwerkressourcen ein, und erstellen Sie den virtuellen Computer.

Sie können auch [einen virtuellen Computer aus einem benutzerdefinierten Datenträgerimage hochladen und erstellen](virtual-machines-linux-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="cli-versions-to-complete-the-task"></a>CLI-Versionen zum Durchführen dieser Aufgabe
Führen Sie die Aufgabe mit einer der folgenden CLI-Versionen durch:

- [Azure CLI 1.0:](virtual-machines-linux-copy-vm-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Unsere CLI für das klassische Bereitstellungsmodell und das Resource Manager-Bereitstellungsmodell
- Azure CLI 2.0 (Vorschau): Unsere Befehlszeilenschnittstelle der nächsten Generation für das Resource Manager-Bereitstellungsmodell (dieser Artikel)

## <a name="prerequisites"></a>Voraussetzungen
- Die neueste [Azure CLI 2.0 (Vorschau)](/cli/azure/install-az-cli2) muss installiert sein, und Sie müssen mithilfe von [az login](/cli/azure/#login) bei einem Azure-Konto angemeldet sein.
- Sie benötigen einen virtuellen Azure-Computer, den Sie beim Erstellen einer Kopie als Quelle verwenden.

## <a name="stop-the-vm"></a>Beenden des virtuellen Computers
Heben Sie die Zuordnung des virtuellen Quellcomputers mit [az vm deallocate](/cli/azure/vm#deallocate) auf: Im folgenden Beispiel wird die Zuordnung für den virtuellen Computer `myVM` in der Ressourcengruppe `myResourceGroup` aufgehoben:

```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

## <a name="copy-the-vm"></a>Kopieren des virtuellen Computers
Zum Kopieren eines virtuellen Computers erstellen Sie eine Kopie der zugrunde liegenden virtuellen Festplatte. Dieser Prozess ermöglicht die Erstellung eines spezialisierten virtuellen Computers, der die gleiche Konfiguration und die gleichen Einstellungen wie der virtuelle Quellcomputer aufweist. Der Vorgang zum Kopieren des virtuellen Datenträgers ist bei Azure Managed Disks und nicht verwalteten Datenträgern nicht ganz identisch. Verwaltete Datenträger werden von der Azure-Plattform verarbeitet und erfordern weder Vorbereitung noch einen Speicherort. Da es sich bei verwalteten Datenträgern um Ressourcen der obersten Ebene handelt, ist ihre Verwendung einfacher – Sie können eine direkte Kopie der Datenträgerressource erstellen. Weitere Informationen zu Azure Managed Disks finden Sie in der [Übersicht über Managed Disks](../storage/storage-managed-disks-overview.md). Wählen Sie einen der entsprechenden folgenden Schritte für den Speichertyp Ihres virtuellen Quellcomputers:

- [Verwaltete Datenträger](#managed-disks)
- [Nicht verwaltete Datenträger](#unmanaged-disks) 

### <a name="managed-disks"></a>Verwaltete Datenträger
Listen Sie die einzelnen virtuellen Computer und die Namen ihrer verwalteten Betriebssystem-Datenträger mit [az vm list](/cli/azure/vm#list) auf. Im folgenden Beispiel werden alle virtuellen Computer in der Ressourcengruppe `myResourceGroup` aufgelistet:

```azurecli
az vm list -g myTestRG --query '[].{Name:name,DiskName:storageProfile.osDisk.name}' --output table
```

Die Ausgabe sieht in etwa wie das folgende Beispiel aus:

```azurecli
Name    DiskName
------  --------
myVM    myDisk
```

Kopieren Sie den Datenträger, indem Sie einen neuen verwalteten Datenträger mit [az disk create](/cli/azure/disk#create) erstellen. Im folgenden Beispiel wird ein Datenträger mit dem Namen `myCopiedDisk` auf Grundlage des verwalteten Datenträgers namens `myDisk` erstellt:

```azurecli
az disk create --resource-group myResourceGroup --name myCopiedDisk --source myDisk
``` 

Überprüfen Sie die verwalteten Datenträger jetzt in der Ressourcengruppe mit [az disk list](/cli/azure/disk#list). Im folgenden Beispiel werden alle verwalteten Datenträger in der Ressourcengruppe `myResourceGroup` aufgelistet:

```azurecli
az disk list --resource-group myResourceGroup --output table
```

Fahren Sie mit dem [Einrichten und Überprüfen der Einstellungen des virtuellen Netzwerks](#set-up-the-virtual-network) fort.


### <a name="unmanaged-disks"></a>Nicht verwaltete Datenträger
Um eine Kopie einer VHD zu erstellen, benötigen Sie die Speicherkontoschlüssel und den URI des Datenträgers. Verwenden Sie [az storage account keys list](/cli/azure/storage/account/keys#list), um die Speicherkontoschlüssel anzuzeigen. Das folgende Beispiel listet die Schlüssel für das Speicherkonto namens `mystorageaccount` in der Ressourcengruppe `myResourceGroup` auf:

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

Verwenden Sie [az vm list](/cli/azure/vm#list), um eine Liste der virtuellen Computer und ihrer URIs anzuzeigen. Im folgenden Beispiel werden die virtuellen Computer in der Ressourcengruppe `myResourceGroup` aufgelistet:

```azurecli
az vm list -g myResourceGroup --query '[].{Name:name,URI:storageProfile.osDisk.vhd.uri}' --output table
```

Die Ausgabe sieht in etwa wie das folgende Beispiel aus:

```azurecli
Name    URI
------  -------------------------------------------------------------
myVM    https://mystorageaccount.blob.core.windows.net/vhds/myVHD.vhd
```

Kopieren Sie die VHD mit [az storage blob copy start](/cli/azure/storage/blob/copy#start). Stellen Sie die erforderlichen Speicherkontoschlüssel und den VHD-URI mithilfe der Informationen von **az storage account keys list** und **az vm list** bereit.

```azurecli
az storage blob copy start \
    --account-name mystorageaccount \
    --account-key gi7crXhD8PMs8DRWqAM7fAjQEFmENiGlOprNxZGbnpylsw/nq+lQQg0c4jiKoV3Nytr3dAiXZRpL8jflpAr2Ug== \
    --source-uri https://mystorageaccount.blob.core.windows.net/vhds/myVHD.vhd \
    --destination-container vhds --destination-blob myCopiedVHD.vhd
```

## <a name="set-up-the-virtual-network"></a>Einrichten des virtuellen Netzwerks
Mit den folgenden Schritten erstellen Sie ein neues virtuelles Netzwerk, ein neues Subnetz, eine neue öffentliche IP-Adresse und eine neue virtuelle Netzwerkschnittstellenkarte (Network Interface Card, NIC). Diese Schritte sind optional. Wenn Sie einen virtuellen Computer zur Problembehandlung oder für zusätzliche Bereitstellungen kopieren, möchten Sie wahrscheinlich keinen virtuellen Computer in einem vorhandenen virtuellen Netzwerk verwenden. Führen Sie die folgenden Schritte aus, wenn Sie eine virtuelle Netzwerkinfrastruktur für Ihre kopierten virtuellen Computer erstellen möchten. Fahren Sie andernfalls mit dem [Erstellen eines virtuellen Computers](#create-a-vm) fort.

Erstellen Sie das virtuelle Netzwerk mit [az network vnet create](/cli/azure/network/vnet#create). Im folgenden Beispiel wird ein virtuelles Netzwerk mit dem Namen `myVnet` und ein Subnetz mit dem Namen `mySubnet` erstellt:

```azurecli
az network vnet create --resource-group myResourceGroup --location westus --name myVnet \
    --address-prefix 192.168.0.0/16 --subnet-name mySubnet --subnet-prefix 192.168.1.0/24
```

Erstellen Sie mit [az network public-ip create](/cli/azure/network/public-ip#create) eine öffentliche IP-Adresse. Im folgenden Beispiel wird eine öffentliche IP-Adresse namens `myPublicIP` mit dem DNS-Namen `mypublicdns` erstellt. (Der DNS-Name muss eindeutig sein. Geben Sie daher einen eigenen eindeutigen Namen an.)

```azurecli
az network public-ip create --resource-group myResourceGroup --location westus \
    --name myPublicIP --dns-name mypublicdns --allocation-method static --idle-timeout 4
```

Erstellen Sie die NIC mit [az network nic create](/cli/azure/network/nic#create). Im folgenden Beispiel wird eine NIC namens `myNic` erstellt, die an das Subnetz `mySubnet` angefügt ist:

```azurecli
az network nic create --resource-group myResourceGroup --location westeurope --name myNic \
    --vnet-name myVnet --subnet mySubnet
```

## <a name="create-a-vm"></a>Erstellen einer VM
Sie können nun mit [az vm create](/cli/azure/vm#create) einen virtuellen Computer erstellen. Wie beim Kopieren von Datenträgern ist der Vorgang bei verwalteten Datenträgern und nicht verwalteten Datenträgern nicht ganz identisch. Erstellen Sie einen virtuellen Computer mit einem der folgenden Befehle.

### <a name="managed-disks"></a>Verwaltete Datenträger
Erstellen Sie mit [az vm create](/cli/azure/vm#create) einen virtuellen Computer. Geben Sie wie folgt den kopierten verwalteten Datenträger an, der als Betriebssystem-Datenträger (`--attach-os-disk`) verwendet werden soll:

```azurecli
az vm create --resource-group myResourceGroup --name myCopiedVM \
    --admin-username azureuser --ssh-key-value ~/.ssh/id_rsa.pub \
    --nics myNic --size Standard_DS1_v2 --os-type Linux \
    --image UbuntuLTS
    --attach-os-disk myCopiedDisk
```

### <a name="unmanaged-disks"></a>Nicht verwaltete Datenträger
Erstellen Sie mit [az vm create](/cli/azure/vm#create) einen virtuellen Computer. Geben Sie wie folgt das Speicherkonto, den Containernamen und die VHD an, die Sie alle beim Erstellen der Kopie mit **az storage blob copy start** (`--image`) verwendet haben:

```azurecli
az vm create --resource-group myResourceGroup --name myCopiedVM  \
    --admin-username azureuser --ssh-key-value ~/.ssh/id_rsa.pub \
    --nics myNic --size Standard_DS1_v2 --os-type Linux \
    --image https://mystorageaccount.blob.core.windows.net/vhds/myCopiedVHD.vhd \
    --use-unmanaged-disk
```

## <a name="next-steps"></a>Nächste Schritte
Informationen zur Verwendung der Azure-CLI für die Verwaltung des neuen virtuellen Computers finden Sie unter [Befehle der Azure-Befehlszeilenschnittstelle im Resource Manager-Modus](azure-cli-arm-commands.md).




<!--HONumber=Feb17_HO2-->


