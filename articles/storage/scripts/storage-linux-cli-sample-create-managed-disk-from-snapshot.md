---
title: "Azure CLI-Beispielskript – Erstellen eines verwalteten Datenträgers aus einer Momentaufnahme | Microsoft-Dokumentation"
description: "Azure CLI-Beispielskript – Erstellen eines verwalteten Datenträgers aus einer Momentaufnahme"
services: managed-disks-linux
documentationcenter: storage
author: ramankum
manager: kavithag
editor: ramankum
tags: azure-service-management
ms.assetid: 
ms.service: managed-disks-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/19/2017
ms.author: ramankum
ms.translationtype: Human Translation
ms.sourcegitcommit: be747170a0d8a7a6defd790a3f8a122c4d397671
ms.openlocfilehash: 5d0c06f0359916ddb2230c67013679bbe8e8aff7
ms.contentlocale: de-de
ms.lasthandoff: 05/23/2017

---

# <a name="create-a-managed-disk-from-a-snapshot-with-cli"></a>Erstellen verwalteter Datenträger aus einer Momentaufnahme mithilfe der Befehlszeilenschnittstelle

Dieses Skript erstellt einen verwalteten Datenträger aus einer Momentaufnahme. Sie können es zum Wiederherstellen eines virtuellen Computers aus Momentaufnahmen von Betriebssystem und Datenträgern verwenden. Erstellen Sie verwaltete Datenträger für Betriebssystem und Daten aus den jeweiligen Momentaufnahmen und anschließend einen neuen virtuellen Computer durch Anfügen der verwalteten Datenträger. Sie können auch die Datenträger eines vorhandenen virtuellen Computers wiederherstellen, indem Sie die aus Momentaufnahmen erstellten Datenträger anfügen.


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli[main](../../../cli_scripts/storage/create-managed-disks-from-snapshot/create-managed-disks-from-snapshot.sh "Verwaltete Datenträger aus Momentaufnahmen erstellen")]


## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle, um einen verwalteten Datenträger aus einer Momentaufnahme zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [az snapshot show](https://docs.microsoft.com/cli/azure/snapshot#show) | Ruft alle Eigenschaften einer Momentaufnahme anhand der Eigenschaften für den Namen und die Ressourcengruppe der Momentaufnahme ab. Die ID-Eigenschaft wird verwendet, um den verwalteten Datenträger zu erstellen.  |
| [az disk create](https://docs.microsoft.com/cli/azure/disk#create) | Erstellt einen verwalteten Datenträger mit der ID einer verwalteten Momentaufnahme |

## <a name="next-steps"></a>Nächste Schritte

[Erstellen virtueller Computer durch Anfügen eines vorhandenen verwalteten Datenträgers als Betriebssystemdatenträger](./../../virtual-machines/scripts/virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fcli%2fmodule%2ftoc.json)

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Zusätzliche CLI-Skriptbeispiele für virtuelle Computer und verwaltete Datenträger finden Sie in der [Dokumentation zu Linux-VMs in Azure](../../virtual-machines/linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

