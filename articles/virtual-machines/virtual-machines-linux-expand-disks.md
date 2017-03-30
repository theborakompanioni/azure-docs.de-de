---
title: Erweitern virtueller Festplatten auf virtuellen Linux-Computern in Azure | Microsoft Docs
description: Erfahren Sie, wie Sie virtuelle Festplatten auf einer Linux-VM mit Azure CLI 2.0 erweitern.
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/22/2017
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: fd35f1774ffda3d3751a6fa4b6e17f2132274916
ms.openlocfilehash: e8eab5aae3160c017d4b6b18aa21c66f728a6a7a
ms.lasthandoff: 03/16/2017

---

# <a name="how-to-expand-virtual-hard-disks-on-a-linux-vm-with-the-azure-cli-20"></a>Erweitern virtueller Festplatten auf einer Linux-VM mit Azure CLI 2.0
Die Standardgröße der virtuellen Festplatte für das Betriebssystem (operating system; OS) beträgt normalerweise 30 GB auf einem virtuellen Linux-Computer (VM) in Azure. Sie können [Datenträger hinzufügen](virtual-machines-linux-add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), um zusätzlichen Speicherplatz zur Verfügung zu stellen, aber möglicherweise möchten Sie auch den Betriebssystemdatenträger oder vorhandene Datenträger für Daten erweitern. Dieser Artikel erläutert, wie verwaltete Datenträger für eine Linux-VM mithilfe von Azure CLI 2.0 erweitert werden können. Sie können auch den nicht verwalteten Betriebssystemdatenträger mit [Azure CLI 1.0](virtual-machines-linux-expand-disks-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) erweitern.

## <a name="expand-managed-disk"></a>Erweitern verwalteter Datenträger
Achten Sie darauf, dass Sie die neueste Version von [Azure CLI 2.0](/cli/azure/install-az-cli2) installiert haben und mit [az login](/cli/azure/#login) bei einem Azure-Konto angemeldet sind.

Ersetzen Sie in den folgenden Beispielen die Beispielparameternamen durch Ihre eigenen Werte. Als Beispielparameternamen werden `myResourceGroup` und `myVM` verwendet.

1. Vorgänge auf virtuellen Festplatten können nicht durchgeführt werden, wenn die VM ausgeführt wird. Heben Sie die Zuordnung der VM mit [az vm deallocate](/cli/azure/vm#deallocate) auf. Im folgenden Beispiel wird die Zuordnung für die VM `myVM` in der Ressourcengruppe `myResourceGroup` aufgehoben:

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

    > [!NOTE]
    > `az vm stop` gibt die Computerressourcen nicht frei. Verwenden Sie `az vm deallocate`, um Computerressourcen freizugeben. Die VM muss aufgehoben werden, um die virtuelle Festplatte zu erweitern.

2. Sie überprüfen die Liste der verwalteten Datenträger in einer Ressourcengruppe mit [az disk list](/cli/azure/disk#list). Im folgenden Beispiel werden alle verwalteten Datenträger in der Ressourcengruppe `myResourceGroup` aufgelistet:

    ```azurecli
    az disk list -g myResourceGroup \
        --query '[*].{Name:name,Gb:diskSizeGb,Tier:accountType}' \
        --output table
    ```

    Sie erweitern den erforderlichen Datenträger mit [az disk update](/cli/azure/disk#update). Im folgenden Beispiel wird der verwaltete Datenträger `myDataDisk` auf eine Größe von `200` GB erweitert:

    ```azurecli
    az disk update --resource-group myResourceGroup --name myDataDisk --size-gb 200
    ```

    > [!NOTE]
    > Wenn Sie einen verwalteten Datenträger erweitern, wird die aktualisierte Größe der nächsten verwalteten Datenträgergröße zugeordnet. Eine Tabelle der verfügbaren verwalteten Datenträgergrößen und -ebenen finden Sie unter [Übersicht über Azure Managed Disks – Preise und Abrechnung](../storage/storage-managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#pricing-and-billing).

3. Starten Sie den virtuellen Computer mit [az vm start](/cli/azure/vm#start). Im folgenden Beispiel wird die VM `myVM` in der Ressourcengruppe `myResourceGroup` gestartet:

    ```azurecli
    az vm start --resource-group myResourceGroup --name myVM
    ```

4. SSH mit Ihrer VM mit den entsprechenden Anmeldeinformationen. Verwenden Sie `df -h`, um zu überprüfen, ob die Größe des Betriebssystemdatenträgers geändert wurde. Die folgende Beispielausgabe zeigt, dass die primäre Partition (`/dev/sda1`) jetzt 200 GB beträgt:

    ```bash
    Filesystem      Size   Used  Avail Use% Mounted on
    /dev/sdc1        194G   52M   193G   1% /datadrive
    ```

## <a name="next-steps"></a>Nächste Schritte
Wenn Sie zusätzlichen Speicher benötigen, [fügen Sie Datenträger zu einer Linux-VM hinzu](virtual-machines-linux-add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Weitere Informationen zur Datenträgerverschlüsselung finden Sie unter [Encrypt disks on a Linux VM using the Azure CLI (Verschlüsseln von Datenträgern auf einer Linux-VM mithilfe der Azure-Befehlszeilenschnittstelle)](virtual-machines-linux-encrypt-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

