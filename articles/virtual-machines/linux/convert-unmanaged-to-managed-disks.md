---
title: "Durchführen der Konvertierung von nicht verwalteten zu verwalteten Datenträgern für eine Linux-VM in Azure | Microsoft-Dokumentation"
description: "Konvertieren einer VM von nicht verwalteten Datenträgern zu verwalteten Azure-Datenträgern mithilfe von Azure CLI 2.0"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 02/09/2017
ms.author: iainfou
ms.translationtype: Human Translation
ms.sourcegitcommit: c785ad8dbfa427d69501f5f142ef40a2d3530f9e
ms.openlocfilehash: 6bab6cbd84c55e668f2caf9b9f94621eec982203
ms.contentlocale: de-de
ms.lasthandoff: 05/26/2017

---

# <a name="how-to-convert-a-linux-vm-from-unmanaged-disks-to-azure-managed-disks"></a>Konvertieren einer Linux-VM von nicht verwalteten Datenträgern zu Azure Managed Disks

Wenn Sie in Azure über vorhandene Linux-VMs verfügen, für die nicht verwaltete Datenträger in Speicherkonten verwendet werden, und für diese VMs die Verwendung von verwalteten Datenträgern (Managed Disks) möglich sein soll, können Sie die VMs konvertieren. Bei diesem Prozess werden sowohl der Betriebssystemdatenträger als auch alle anderen angefügten Datenträger konvertiert. Da für den Konvertierungsprozess ein Neustart der VM erforderlich ist, sollten Sie die Migration Ihrer VMs während eines bereits vorhandenen Wartungsfensters durchführen. Der Migrationsprozess kann nicht rückgängig gemacht werden. Testen Sie den Migrationsprozess, indem Sie einen virtuellen Testcomputer migrieren, bevor Sie die Migration in der Produktion durchführen.

> [!IMPORTANT]
> Bei der Konvertierung heben Sie die Zuordnung der VM auf. Die VM erhält eine neue IP-Adresse, wenn sie nach der Konvertierung gestartet wird. Verwenden Sie eine reservierte IP, wenn eine Abhängigkeit von einer festen IP-Adresse besteht.

Sie können einen nicht verwalteten Datenträger nicht in einen verwalteten Datenträger konvertieren, wenn der nicht verwaltete Datenträger sich in einem Speicherkonto befindet, das mithilfe von [Azure Storage Service Encryption (SSE)](../../storage/storage-service-encryption.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) verschlüsselt ist oder vorher einmal verschlüsselt war. Die folgenden Schritte beschreiben, wie Sie nicht verwaltete Datenträger konvertieren, die sich in einem verschlüsselten Speicherkonto befinden bzw. befanden:

- Kopieren Sie die virtuelle Festplatte (VHD) mit [az storage blob copy start](/cli/azure/storage/blob/copy#start) in ein Speicherkonto, das nie für Azure Storage Service Encryption aktiviert wurde.
- Erstellen Sie einen virtuellen Computer, der verwaltete Datenträger verwendet, und geben Sie diese VHD-Datei während der Erstellung mit [az vm create](/cli/azure/vm#create) an. Alternativ dazu:
- Fügen Sie die kopierte VHD mit [az vm disk attach](/cli/azure/vm/disk#attach) an einen ausgeführten virtuellen Computer mit verwalteten Datenträgern an.

## <a name="convert-vm-to-azure-managed-disks"></a>Konvertieren einer VM in Azure Managed Disks
In diesem Abschnitt wird beschrieben, wie Sie für Ihre vorhandenen Azure-VMs die Konvertierung von nicht verwalteten Datenträgern in verwaltete Datenträger durchführen. Sie können diesen Prozess verwenden, um die Konvertierung von nicht verwalteten Premium-Datenträgern (SSD) in verwaltete Premium-Datenträger oder von nicht verwalteten Standard-Datenträgern (HDD) in verwaltete Standard-Datenträger durchzuführen.

1. Heben Sie die Zuordnung der VM mit [az vm deallocate](/cli/azure/vm#deallocate) auf. Im folgenden Beispiel wird die Zuordnung für die VM `myVM` in der Ressourcengruppe `myResourceGroup` aufgehoben:

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

2. Führen Sie für die VM mit [az vm convert](/cli/azure/vm#convert) die Konvertierung in verwaltete Datenträger durch. Mit dem folgenden Prozess wird die VM `myVM` konvertiert, einschließlich des Betriebssystemdatenträgers und der anderen Datenträger:

    ```azurecli
    az vm convert --resource-group myResourceGroup --name myVM
    ```

3. Starten Sie die VM nach der Konvertierung in verwaltete Datenträger mit [az vm start](/cli/azure/vm#start). Im folgenden Beispiel wird die VM `myVM` in der Ressourcengruppe `myResourceGroup` gestartet.

    ```azurecli
    az vm start --resource-group myResourceGroup --name myVM
    ```

## <a name="convert-vm-in-an-availability-set-to-managed-disks"></a>Durchführen der Konvertierung in verwaltete Datenträger für eine VM in einer Verfügbarkeitsgruppe

Falls sich die VMs, die Sie in verwaltete Datenträger konvertieren möchten, in einer Verfügbarkeitsgruppe befinden, müssen Sie zuerst für die Verfügbarkeitsgruppe die Konvertierung in eine verwaltete Verfügbarkeitsgruppe durchführen.

Für alle VMs in der Verfügbarkeitsgruppe muss die Zuordnung aufgehoben werden, bevor Sie die Verfügbarkeitsgruppe konvertieren können. Planen Sie die Konvertierung aller VMs in verwaltete Datenträger, nachdem die Verfügbarkeitsgruppe selbst in eine verwaltete Verfügbarkeitsgruppe konvertiert wurde. Anschließend können Sie alle VMs starten und den Betrieb normal fortsetzen.

1. Listen Sie alle VMs einer Verfügbarkeitsgruppe mit [az vm availability-set list](/cli/azure/vm/availability-set#list) auf. Im folgenden Beispiel werden alle VMs der Verfügbarkeitsgruppe `myAvailabilitySet` aufgelistet, die in der Ressourcengruppe `myResourceGroup` enthalten sind:

    ```azurecli
    az vm availability-set show --resource-group myResourceGroup \
        --name myAvailabilitySet --query [virtualMachines[*].id] --output table
    ```

2. Verwenden Sie [az vm deallocate](/cli/azure/vm#deallocate), um die Zuordnung für alle VMs aufzuheben. Im folgenden Beispiel wird die Zuordnung für die VM `myVM` in der Ressourcengruppe `myResourceGroup` aufgehoben:

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

3. Konvertieren Sie die Verfügbarkeitsgruppe mit [az vm availability-set convert](/cli/azure/vm/availability-set#convert). Im folgenden Beispiel wird die Verfügbarkeitsgruppe `myAvailabilitySet` aus der Ressourcengruppe `myResourceGroup` konvertiert:

    ```azurecli
    az vm availability-set convert --resource-group myResourceGroup \
        --name myAvailabilitySet
    ```

4. Konvertieren Sie alle VMs in verwaltete Datenträger, indem Sie [az vm convert](/cli/azure/vm#convert) verwenden. Mit dem folgenden Prozess wird die VM `myVM` konvertiert, einschließlich des Betriebssystemdatenträgers und der anderen Datenträger:

    ```azurecli
    az vm convert --resource-group myResourceGroup --name myVM
    ```

5. Starten Sie alle VMs nach der Konvertierung in verwaltete Datenträger mit [az vm start](/cli/azure/vm#start). Im folgenden Beispiel wird die VM `myVM` in der Ressourcengruppe `myResourceGroup` gestartet.

    ```azurecli
    az vm start --resource-group myResourceGroup --name myVM
    ```

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Speicheroptionen finden Sie in der [Übersicht über Managed Disks](../../storage/storage-managed-disks-overview.md).

