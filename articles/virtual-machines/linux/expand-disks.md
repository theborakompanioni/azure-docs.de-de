---
title: Erweitern von virtuellen Festplatten auf virtuellen Linux-Computern in Azure | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie virtuelle Festplatten auf einer Linux-VM mit Azure CLI 2.0 erweitern.
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/21/2017
ms.author: iainfou
ms.translationtype: HT
ms.sourcegitcommit: cf381b43b174a104e5709ff7ce27d248a0dfdbea
ms.openlocfilehash: b82cc0473c003da767ee230ab485c69b233977d1
ms.contentlocale: de-de
ms.lasthandoff: 08/23/2017

---

# <a name="how-to-expand-virtual-hard-disks-on-a-linux-vm-with-the-azure-cli"></a>Erweitern von virtuellen Festplatten auf virtuellen Linux-Computern mit der Azure-CLI
Die Standardgröße der virtuellen Festplatte für das Betriebssystem (operating system; OS) beträgt normalerweise 30 GB auf einem virtuellen Linux-Computer (VM) in Azure. Sie können [Datenträger hinzufügen](add-disk.md), um zusätzlichen Speicherplatz zur Verfügung zu stellen, aber möglicherweise möchten Sie auch einen vorhandenen Datenträger für Daten erweitern. Dieser Artikel erläutert, wie verwaltete Datenträger für eine Linux-VM mithilfe von Azure CLI 2.0 erweitert werden können. Sie können auch den nicht verwalteten Betriebssystemdatenträger mit [Azure CLI 1.0](expand-disks-nodejs.md) erweitern.

> [!WARNING]
> Stellen Sie immer sicher, dass Sie Ihre Daten sichern, bevor Sie Änderungen an der Größe von Datenträgern vornehmen. Weitere Informationen finden Sie unter [Sichern virtueller Linux-Computer in Azure](tutorial-backup-vms.md).

## <a name="expand-disk"></a>Erweitern des Datenträgers
Achten Sie darauf, dass Sie die neueste Version von [Azure CLI 2.0](/cli/azure/install-az-cli2) installiert haben und mit [az login](/cli/azure/#login) bei einem Azure-Konto angemeldet sind.

Für diesen Artikel ist ein vorhandener virtueller Computer in Azure mit mindestens einem angefügten und vorbereiteten Datenträger erforderlich. Wenn Sie noch nicht über einen virtuellen Computer verfügen, den Sie verwenden können, finden Sie entsprechende Informationen unter [Erstellen und Vorbereiten eines virtuellen Computers mit Datenträgern](tutorial-manage-disks.md#create-and-attach-disks).

Ersetzen Sie in den folgenden Beispielen die Beispielparameternamen durch Ihre eigenen Werte. Beispiele für Parameternamen sind *myResourceGroup* und *myVM*.

1. Vorgänge auf virtuellen Festplatten können nicht durchgeführt werden, wenn die VM ausgeführt wird. Heben Sie die Zuordnung der VM mit [az vm deallocate](/cli/azure/vm#deallocate) auf. Im folgenden Beispiel wird die Zuordnung für die VM *myVM* in der Ressourcengruppe *myResourceGroup* aufgehoben:

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

    > [!NOTE]
    > `az vm stop` gibt die Computerressourcen nicht frei. Verwenden Sie `az vm deallocate`, um Computerressourcen freizugeben. Die VM muss aufgehoben werden, um die virtuelle Festplatte zu erweitern.

2. Sie überprüfen die Liste der verwalteten Datenträger in einer Ressourcengruppe mit [az disk list](/cli/azure/disk#list). Im folgenden Beispiel wird eine Liste mit verwalteten Datenträgern in der Ressourcengruppe *myResourceGroup* aufgelistet:

    ```azurecli
    az disk list \
        --resource-group myResourceGroup \
        --query '[*].{Name:name,Gb:diskSizeGb,Tier:accountType}' \
        --output table
    ```

    Sie erweitern den erforderlichen Datenträger mit [az disk update](/cli/azure/disk#update). Im folgenden Beispiel wird der verwaltete Datenträger *myDataDisk* auf eine Größe von *200* GB erweitert:

    ```azurecli
    az disk update \
        --resource-group myResourceGroup \
        --name myDataDisk \
        --size-gb 200
    ```

    > [!NOTE]
    > Wenn Sie einen verwalteten Datenträger erweitern, wird die aktualisierte Größe der nächsten verwalteten Datenträgergröße zugeordnet. Eine Tabelle der verfügbaren verwalteten Datenträgergrößen und -ebenen finden Sie unter [Übersicht über Azure Managed Disks – Preise und Abrechnung](../windows/managed-disks-overview.md#pricing-and-billing).

3. Starten Sie den virtuellen Computer mit [az vm start](/cli/azure/vm#start). Im folgenden Beispiel wird die VM *myVM* in der Ressourcengruppe *myResourceGroup* gestartet:

    ```azurecli
    az vm start --resource-group myResourceGroup --name myVM
    ```

4. SSH mit Ihrer VM mit den entsprechenden Anmeldeinformationen. Sie können die öffentliche IP-Adresse Ihres virtuellen Computers mit dem Befehl [az vm show](/cli/azure/vm#show) abrufen:

    ```azurecli
    az vm show --resource-group myResourceGroup --name myVM -d --query [publicIps] --o tsv
    ```

5. Um den erweiterten Datenträger zu verwenden, müssen Sie die zugrunde liegende Partition und das Dateisystem erweitern.

    a. Wenn der Datenträger bereits eingebunden ist, heben Sie die Einbindung auf:

    ```bash
    sudo umount /dev/sdc1
    ```

    b. Verwenden Sie `parted`, um Datenträgerinformationen anzuzeigen und die Größe der Partition zu ändern:

    ```bash
    sudo parted /dev/sdc
    ```

    Informationen zum vorhandenen Partitionslayout können Sie mit `print` anzeigen. Die Ausgabe ähnelt dem folgenden Beispiel, in dem angegeben wird, dass die Größe des zugrunde liegenden Datenträgers 215 GB beträgt:

    ```bash
    GNU Parted 3.2
    Using /dev/sdc1
    Welcome to GNU Parted! Type 'help' to view a list of commands.
    (parted) print
    Model: Unknown Msft Virtual Disk (scsi)
    Disk /dev/sdc1: 215GB
    Sector size (logical/physical): 512B/4096B
    Partition Table: loop
    Disk Flags:
    
    Number  Start  End    Size   File system  Flags
        1      0.00B  107GB  107GB  ext4
    ```

    c. Erweitern Sie die Partition mit `resizepart`. Geben Sie die Partitionsnummer *1* und die Größe für die neue Partition ein:

    ```bash
    (parted) resizepart
    Partition number? 1
    End?  [107GB]? 215GB
    ```

    d. Geben Sie zum Beenden den Befehl `quit` ein.

5. Überprüfen Sie nach der Änderung der Partitionsgröße die Partitionskonsistenz mit `e2fsck`:

    ```bash
    sudo e2fsck -f /dev/sdc1
    ```

6. Ändern Sie nun die Größe des Dateisystems mit `resize2fs`:

    ```bash
    sudo resize2fs /dev/sdc1
    ```

7. Binden Sie die Partition am gewünschten Speicherort ein, z.B. `/datadrive`:

    ```bash
    sudo mount /dev/sdc1 /datadrive
    ```

8. Verwenden Sie `df -h`, um zu überprüfen, ob die Größe des Betriebssystemdatenträgers geändert wurde. In der folgenden Beispielausgabe ist zu sehen, dass der Datenträger für Daten */dev/sdc1* jetzt eine Größe von 200 GB hat:

    ```bash
    Filesystem      Size   Used  Avail Use% Mounted on
    /dev/sdc1        197G   60M   187G   1% /datadrive
    ```

## <a name="next-steps"></a>Nächste Schritte
Wenn Sie zusätzlichen Speicher benötigen, [fügen Sie Datenträger zu einer Linux-VM hinzu](add-disk.md). Weitere Informationen zur Datenträgerverschlüsselung finden Sie unter [Encrypt disks on a Linux VM using the Azure CLI (Verschlüsseln von Datenträgern auf einer Linux-VM mithilfe der Azure-Befehlszeilenschnittstelle)](encrypt-disks.md).

