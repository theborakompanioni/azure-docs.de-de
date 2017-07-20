---
title: "Azure PowerShell-Beispielskript – Kopieren (Verschieben) einer Momentaufnahme eines verwalteten Datenträgers in das gleiche oder ein anderes Abonnement | Microsoft-Dokumentation"
description: "Azure PowerShell-Beispielskript – Kopieren (Verschieben) einer Momentaufnahme eines verwalteten Datenträgers in das gleiche oder ein anderes Abonnement"
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
ms.openlocfilehash: 5ae87c485f06f9eec6b64e8a072e334c73b56a47
ms.contentlocale: de-de
ms.lasthandoff: 06/07/2017

---

# <a name="copy-snapshot-of-a-managed-disk-in-same-subscription-or-different-subscription-with-powershell"></a>Kopieren einer Momentaufnahme eines verwalteten Datenträgers in das gleiche oder ein anderes Abonnement mit PowerShell

Dieses Skript erstellt eine Kopie einer Momentaufnahme eines vorhandenen verwalteten Datenträgers im gleichen Abonnement oder einem anderen Abonnement. Verwenden Sie dieses Skript, um eine Momentaufnahme zur Datenaufbewahrung in ein anderes Abonnement zu verschieben. Das Speichern von Momentaufnahmen im einem anderen Abonnement schützt Sie vor dem versehentlichem Löschen von Momentaufnahmen in Ihrem Hauptabonnement. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/storage/copy-snapshot-to-same-or-different-subscription/copy-snapshot-to-same-or-different-subscription.ps1 "Momentaufnahmen kopieren")]


## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle zum Erstellen einer Momentaufnahme im Zielabonnement mit der ID der Quellmomentaufnahme. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [New-AzureRmSnapshotConfig](/powershell/module/azurerm.compute/New-AzureRmSnapshotConfig) | Erstellt die Momentaufnahmenkonfiguration, die für die Erstellung der Momentaufnahme verwendet wird. Enthält die Ressourcen-ID der übergeordneten Momentaufnahme und des Speicherorts, der mit dem Speicherort der übergeordneten Momentaufnahme übereinstimmt.  |
| [New-AzureRmSnapshot](/powershell/module/azurerm.compute/New-AzureRmDisk) | Erstellt eine Momentaufnahme mit Momentaufnahmenkonfiguration, Momentaufnahmenname und Name der Ressourcengruppe, die als Parameter übergeben werden. |


## <a name="next-steps"></a>Nächste Schritte

[Erstellen eines virtuellen Computers aus einer Momentaufnahme](./../../virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/overview).

Zusätzliche VM-PowerShell-Skriptbeispiele finden Sie in der [Dokumentation zu Windows-VMs in Azure](../../virtual-machines/windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
