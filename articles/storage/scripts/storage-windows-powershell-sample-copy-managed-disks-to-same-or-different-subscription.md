---
title: "Azure PowerShell-Beispielskript – Kopieren (Verschieben) verwalteter Datenträger in das gleiche oder ein anderes Abonnement | Microsoft-Dokumentation"
description: "Azure PowerShell-Beispielskript – Kopieren (Verschieben) verwalteter Datenträger in das gleiche oder ein anderes Abonnement"
services: managed-disks
documentationcenter: storage
author: ramankum
manager: kavithag
editor: ramankum
tags: azure-service-management
ms.assetid: 
ms.service: managed-disks-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 06/06/2017
ms.author: ramankum
ms.translationtype: Human Translation
ms.sourcegitcommit: 09f24fa2b55d298cfbbf3de71334de579fbf2ecd
ms.openlocfilehash: 4dc83821a84fa8721c07a8660adeced26a1709f9
ms.contentlocale: de-de
ms.lasthandoff: 06/07/2017

---

# <a name="copy-managed-disks-in-the-same-subscription-or-different-subscription-with-powershell"></a>Kopieren von verwalteten Datenträgern in das gleiche oder ein anderes Abonnement mit PowerShell

Dieses Skript erstellt eine Kopie eines vorhandenen verwalteten Datenträgers im gleichen Abonnement oder einem anderen Abonnement. Der neue Datenträger wird in der gleichen Region wie der übergeordnete verwaltete Datenträger erstellt.   

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/storage/copy-managed-disks-to-same-or-different-subscription/copy-managed-disks-to-same-or-different-subscription.ps1 "Verwalteten Datenträger kopieren")]


## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle zum Erstellen eines neuen verwalteten Datenträgers im Zielabonnement mit der ID des verwalteten Quelldatenträgers. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [New-AzureRmDiskConfig](/powershell/module/azurerm.compute/New-AzureRmDiskConfig) | Erstellt die Datenträgerkonfiguration, die für die Datenträgererstellung verwendet wird. Enthält die Ressourcen-ID des übergeordneten Datenträgers und des Speicherorts, der mit dem Speicherort des übergeordneten Datenträgers übereinstimmt.  |
| [New-AzureRmDisk](/powershell/module/azurerm.compute/New-AzureRmDisk) | Erstellt einen Datenträger mit Datenträgerkonfiguration, Datenträgername und Name der Ressourcengruppe, die als Parameter übergeben werden. |


## <a name="next-steps"></a>Nächste Schritte

[Erstellen virtueller Computer aus verwalteten Datenträgern](./../../virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/overview).

Zusätzliche VM-PowerShell-Skriptbeispiele finden Sie in der [Dokumentation zu Windows-VMs in Azure](../../virtual-machines/windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
