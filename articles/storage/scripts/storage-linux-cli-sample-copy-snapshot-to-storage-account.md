---
title: "Azure CLI-Beispielskript – Exportieren/Kopieren einer Momentaufnahme als VHD in ein Speicherkonto in einer anderen Region | Microsoft-Dokumentation"
description: "Azure CLI-Beispielskript – Exportieren/Kopieren einer Momentaufnahme als VHD in ein Speicherkonto in demselben oder einem anderen Abonnement"
services: virtual-machines-linux
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/19/2017
ms.author: ramankum
ms.translationtype: Human Translation
ms.sourcegitcommit: be747170a0d8a7a6defd790a3f8a122c4d397671
ms.openlocfilehash: 6785b444e0a89ec3ee0c2cebd8a5af123bb367f3
ms.contentlocale: de-de
ms.lasthandoff: 05/23/2017

---

# <a name="exportcopy-managed-snapshots-as-vhd-to-a-storage-account-in-different-region-with-cli"></a>Exportieren/Kopieren verwalteter Momentaufnahmen als VHD in ein Speicherkonto in einer anderen Region mit der Befehlszeilenschnittstelle

Dieses Skript exportiert eine verwaltete Momentaufnahme in ein Speicherkonto in einer anderen Region. Zuerst generiert es den SAS-URI der Momentaufnahme, mit dem es diese anschließend in ein Speicherkonto in einer anderen Region kopiert. Verwenden Sie dieses Skript, um Sicherungen Ihrer verwalteten Datenträger für die Notfallwiederherstellung in einer anderen Region zu verwalten. 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli[main](../../../cli_scripts/storage/copy-snapshots-to-storage-account/copy-snapshots-to-storage-account.sh "Momentaufnahmen kopieren")]


## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle zum Generieren des SAS-URI für eine verwaltete Momentaufnahme und zum Kopieren der Momentaufnahme in ein Speicherkonto mithilfe dieses SAS-URI. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [az snapshot grant-access](https://docs.microsoft.com/cli/azure/snapshot#grant-access) | Generiert eine schreibgeschützte Shared Access Signature (SAS), die verwendet wird, um die zugrunde liegende VHD-Datei in ein Speicherkonto zu kopieren oder lokal herunterzuladen  |
| [az storage blob copy start](https://docs.microsoft.com/en-us/cli/azure/storage/blob/copy#start) | Kopiert ein Blob asynchron aus einem Speicherkonto in ein anderes |

## <a name="next-steps"></a>Nächste Schritte

[Erstellen verwalteter Datenträger aus einer VHD](./../scripts/storage-linux-cli-sample-create-managed-disk-from-vhd.md?toc=%2fcli%2fmodule%2ftoc.json)

[Erstellen virtueller Computer aus verwalteten Datenträgern](./../../virtual-machines/scripts/virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fcli%2fmodule%2ftoc.json)

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Zusätzliche CLI-Skriptbeispiele für virtuelle Computer und verwaltete Datenträger finden Sie in der [Dokumentation zu Linux-VMs in Azure](../../virtual-machines/linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

