---
title: "Azure CLI-Beispielskript – Kopieren (Verschieben) verwalteter Datenträger in dasselbe oder ein anderes Abonnement | Microsoft-Dokumentation"
description: "Azure CLI-Beispielskript – Kopieren (Verschieben) verwalteter Datenträger in dasselbe oder ein anderes Abonnement"
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
ms.openlocfilehash: 90228d66150cf961557b6dbeed11308932677d83
ms.contentlocale: de-de
ms.lasthandoff: 05/23/2017

---

# <a name="copy-managed-disks-to-same-or-different-subscription-with-cli"></a>Kopieren verwalteter Datenträger in dasselbe oder ein anderes Abonnement mithilfe der Befehlszeilenschnittstelle

Dieses Skript kopiert einen verwalteten Datenträger in dasselbe oder ein anderes Abonnement in derselben Region. 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli[main](../../../cli_scripts/storage/copy-managed-disks-to-same-or-different-subscription/copy-managed-disks-to-same-or-different-subscription.sh "Verwaltete Datenträger kopieren")]


## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle zum Erstellen eines neuen verwalteten Datenträgers im Zielabonnement mit der ID des verwalteten Quelldatenträgers. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [az disk show](https://docs.microsoft.com/cli/azure/disk#show) | Ruft alle Eigenschaften eines verwalteten Datenträgers anhand der Eigenschaften für den Namen und die Ressourcengruppe des verwalteten Datenträgers ab. Die ID-Eigenschaft wird verwendet, um den verwalteten Datenträger in ein anderes Abonnement zu kopieren.  |
| [az disk create](https://docs.microsoft.com/cli/azure/disk#create) | Kopiert einen verwalteten Datenträger durch Erstellen eines neuen verwalteten Datenträgers in einem anderen Abonnement mit der ID und dem Namen des übergeordneten verwalteten Datenträgers  |

## <a name="next-steps"></a>Nächste Schritte

[Erstellen virtueller Computer aus verwalteten Datenträgern](./../../virtual-machines/scripts/virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Zusätzliche CLI-Skriptbeispiele für virtuelle Computer und verwaltete Datenträger finden Sie in der [Dokumentation zu Linux-VMs in Azure](../../virtual-machines/linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

