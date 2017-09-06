---
title: "Konvertieren einer Linux-VM in Azure von nicht verwalteten Datenträgern in verwaltete Datenträger – Azure Managed Disks | Microsoft-Dokumentation"
description: "Hier wird beschrieben, wie mithilfe der Azure CLI 2.0 im Resource Manager-Bereitstellungsmodell eine Konvertierung von Linux-VMs von nicht verwalteten Datenträgern in verwaltete Datenträger ausgeführt wird."
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
ms.date: 06/23/2017
ms.author: iainfou
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: 94f8e3330fb2d6547811315fcfdb8ced338e0247
ms.contentlocale: de-de
ms.lasthandoff: 08/21/2017

---

# <a name="convert-a-linux-virtual-machine-from-unmanaged-disks-to-managed-disks"></a>Konvertieren einer Linux-VM von nicht verwalteten Datenträgern in verwaltete Datenträger

Wenn Sie über vorhandene virtuelle Linux-Computer (VMs) verfügen, die nicht verwaltete Datenträger verwenden, können Sie die VMs über den [Azure Managed Disks-Dienst](../windows/managed-disks-overview.md) konvertieren, sodass verwaltete Datenträger verwendet werden. Bei diesem Prozess werden sowohl der Betriebssystemdatenträger als auch alle anderen angefügten Datenträger konvertiert.

In diesem Artikel wird beschrieben, wie Sie VMs über die Azure-Befehlszeilenschnittstelle konvertieren. Wenn Sie die Befehlszeilenschnittstelle installieren oder aktualisieren müssen, finden Sie unter [Installieren von Azure CLI 2.0](/cli/azure/install-azure-cli) Informationen dazu. 

## <a name="before-you-begin"></a>Voraussetzungen

[!INCLUDE [virtual-machines-common-convert-disks-considerations](../../../includes/virtual-machines-common-convert-disks-considerations.md)]


## <a name="convert-single-instance-vms"></a>Konvertieren von Einzelinstanz-VMs
In diesem Abschnitt wird beschrieben, wie Sie für Einzelinstanz-VMs von Azure die Konvertierung von nicht verwalteten Datenträgern in verwaltete Datenträger durchführen. (Wenn Ihre VMs in einer Verfügbarkeitsgruppe enthalten sind, lesen Sie den nächsten Abschnitt.) Sie können diesen Prozess verwenden, um nicht verwaltete Premium-Datenträger (SSD) in verwaltete Premium-Datenträger oder nicht verwaltete Standard-Datenträger (HDD) in verwaltete Standard-Datenträger zu konvertieren.

1. Heben Sie die Zuordnung der VM mit [az vm deallocate](/cli/azure/vm#deallocate) auf. Im folgenden Beispiel wird die Zuordnung für die VM `myVM` in der Ressourcengruppe `myResourceGroup` aufgehoben:

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

2. Führen Sie mit [az vm convert](/cli/azure/vm#convert) die Konvertierung in verwaltete Datenträger durch. Mit dem folgenden Prozess wird die VM `myVM` konvertiert, einschließlich des Betriebssystemdatenträgers und aller Datenträger:

    ```azurecli
    az vm convert --resource-group myResourceGroup --name myVM
    ```

3. Starten Sie die VM nach der Konvertierung in verwaltete Datenträger mit [az vm start](/cli/azure/vm#start). Im folgenden Beispiel wird die VM `myVM` in der Ressourcengruppe `myResourceGroup` gestartet.

    ```azurecli
    az vm start --resource-group myResourceGroup --name myVM
    ```

## <a name="convert-vms-in-an-availability-set"></a>Konvertieren von virtuellen Computern in einer Verfügbarkeitsgruppe

Falls sich die VMs, die Sie in verwaltete Datenträger konvertieren möchten, in einer Verfügbarkeitsgruppe befinden, müssen Sie zuerst für die Verfügbarkeitsgruppe die Konvertierung in eine verwaltete Verfügbarkeitsgruppe durchführen.

Für alle VMs in der Verfügbarkeitsgruppe muss die Zuordnung aufgehoben werden, bevor Sie die Verfügbarkeitsgruppe konvertieren können. Planen Sie die Konvertierung aller VMs in verwaltete Datenträger, nachdem die Verfügbarkeitsgruppe selbst in eine verwaltete Verfügbarkeitsgruppe konvertiert wurde. Starten Sie anschließend alle VMs, und setzen Sie den normalen Betrieb fort.

1. Listen Sie alle VMs einer Verfügbarkeitsgruppe mit [az vm availability-set list](/cli/azure/vm/availability-set#list) auf. Im folgenden Beispiel werden alle VMs der Verfügbarkeitsgruppe `myAvailabilitySet` aufgelistet, die in der Ressourcengruppe `myResourceGroup` enthalten sind:

    ```azurecli
    az vm availability-set show \
        --resource-group myResourceGroup \
        --name myAvailabilitySet \
        --query [virtualMachines[*].id] \
        --output table
    ```

2. Verwenden Sie [az vm deallocate](/cli/azure/vm#deallocate), um die Zuordnung aller VMs aufzuheben. Im folgenden Beispiel wird die Zuordnung für die VM `myVM` in der Ressourcengruppe `myResourceGroup` aufgehoben:

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

3. Konvertieren Sie die Verfügbarkeitsgruppe mit [az vm availability-set convert](/cli/azure/vm/availability-set#convert). Im folgenden Beispiel wird die Verfügbarkeitsgruppe `myAvailabilitySet` aus der Ressourcengruppe `myResourceGroup` konvertiert:

    ```azurecli
    az vm availability-set convert \
        --resource-group myResourceGroup \
        --name myAvailabilitySet
    ```

4. Konvertieren Sie alle VMs in verwaltete Datenträger, indem Sie [az vm convert](/cli/azure/vm#convert) verwenden. Mit dem folgenden Prozess wird die VM `myVM` konvertiert, einschließlich des Betriebssystemdatenträgers und aller Datenträger:

    ```azurecli
    az vm convert --resource-group myResourceGroup --name myVM
    ```

5. Starten Sie alle VMs nach der Konvertierung in verwaltete Datenträger mit [az vm start](/cli/azure/vm#start). Im folgenden Beispiel wird die VM `myVM` in der Ressourcengruppe `myResourceGroup` gestartet:

    ```azurecli
    az vm start --resource-group myResourceGroup --name myVM
    ```

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Speicheroptionen finden Sie in der [Übersicht über Managed Disks](../windows/managed-disks-overview.md).

