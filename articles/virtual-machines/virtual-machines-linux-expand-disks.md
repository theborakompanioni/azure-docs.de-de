---
title: "Erweitern des Betriebssystemdatenträgers auf Linux-VM in Azure | Microsoft Docs"
description: "Erfahren Sie, wie Sie den virtuellen Betriebssystemdatenträger auf einer Linux-VM unter Verwendung der Azure-Befehlszeilenschnittstelle und des Resource Manager-Bereitstellungsmodells erweitern"
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
ms.date: 11/22/2016
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: c89ce056cb0fa84b2169df161660916d083bec3f
ms.openlocfilehash: bcef9de167fa31001ee4086d7e5b85c2d8bc4613


---

# <a name="expand-os-disk-on-a-linux-vm-using-the-azure-cli"></a>Erweitern des Betriebssystemdatenträgers auf einer Linux-VM mithilfe der Azure-Befehlszeilenschnittstelle
Die Standardgröße der virtuellen Festplatte für das Betriebssystem (operating system; OS) beträgt normalerweise 30 GB auf einem virtuellen Linux-Computer (VM) in Azure. Sie können [Datenträger hinzufügen](virtual-machines-linux-add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), um zusätzlichen Speicherplatz zur Verfügung zu stellen, aber möglicherweise möchten Sie auch den Betriebssystemdatenträger erweitern. Dieser Artikel erläutert, wie die Betriebssystemdatenträger für eine Linux-VM mithilfe der Azure-Befehlszeilenschnittstelle erweitert werden können.


## <a name="prerequisites"></a>Voraussetzungen
Installieren Sie die [neueste Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md), und melden Sie sich mithilfe des Resource Manager-Modus wie folgt bei einem [Azure-Konto](https://azure.microsoft.com/pricing/free-trial/) an:

```azurecli
azure config mode arm
```

Ersetzen Sie in den folgenden Beispielen die Beispielparameternamen durch Ihre eigenen Werte. Als Beispielparameternamen werden `myResourceGroup` und `myVM` verwendet.


## <a name="expand-os-disk"></a>Erweitern des Betriebssystemdatenträgers

1. Vorgänge auf virtuellen Festplatten können nicht durchgeführt werden, wenn die VM ausgeführt wird. Im folgenden Beispiel wird die VM namens `myVM` in der Ressourcengruppe namens `myResourceGroup` beendet und aufgehoben:

    ```azurecli
    azure vm deallocate --resource-group myResourceGroup --name myVM
    ```

    > [!NOTE]
    > `azure vm stop` gibt die Computerressourcen nicht frei. Verwenden Sie `azure vm deallocate`, um Computerressourcen freizugeben. Die VM muss aufgehoben werden, um die virtuelle Festplatte zu erweitern.

2. Aktualisieren Sie die Größe des Betriebssystemdatenträgers unter Verwendung des Befehls `azure vm set`. Im folgenden Beispiel wird die VM namens `myVM` in der Ressourcengruppe namens `myResourceGroup` auf `50` GB aktualisiert:

    ```azurecli
    azure vm set --resource-group myResourceGroup --name myVM --new-os-disk-size 50
    ```

3. Starten Sie Ihre VM wie folgt:

    ```azurecli
    azure vm start --resource-group myResourceGroup --name myVM
    ```

4. SSH mit Ihrer VM mit den entsprechenden Anmeldeinformationen. Verwenden Sie `df -h`, um zu überprüfen, ob die Größe des Betriebssystemdatenträgers geändert wurde. Die folgende Beispielausgabe zeigt, dass die primäre Partition (`/dev/sda1`) jetzt 50 GB beträgt:

    ```bash
    Filesystem      Size  Used Avail Use% Mounted on
    udev            1.7G     0  1.7G   0% /dev
    tmpfs           344M  5.0M  340M   2% /run
    /dev/sda1        49G  1.3G   48G   3% /
    ```

## <a name="next-steps"></a>Nächste Schritte
Wenn Sie zusätzlichen Speicher benötigen, [fügen Sie Datenträger zu einer Linux-VM hinzu](virtual-machines-linux-add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Weitere Informationen zur Datenträgerverschlüsselung finden Sie unter [Encrypt disks on a Linux VM using the Azure CLI (Verschlüsseln von Datenträgern auf einer Linux-VM mithilfe der Azure-Befehlszeilenschnittstelle)](virtual-machines-linux-encrypt-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).



<!--HONumber=Jan17_HO1-->


