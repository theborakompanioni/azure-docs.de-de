---
title: "Azure PowerShell-Skript-Beispiel: Erstellen einer Momentaufnahme aus einer VHD, um mehrere identisch verwaltete Datenträger in kürzester Zeit zu erstellen | Microsoft-Dokumentation"
description: "Azure PowerShell-Skript-Beispiel: Erstellen einer Momentaufnahme aus einer VHD, um mehrere identisch verwaltete Datenträger in kürzester Zeit zu erstellen"
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
ms.date: 06/05/2017
ms.author: ramankum
ms.translationtype: Human Translation
ms.sourcegitcommit: 09f24fa2b55d298cfbbf3de71334de579fbf2ecd
ms.openlocfilehash: 3db2ba858e5c685bb2b53a4f09423acdcdb10c22
ms.contentlocale: de-de
ms.lasthandoff: 06/07/2017

---

# <a name="create-a-snapshot-from-a-vhd-to-create-multiple-identical-managed-disks-in-small-amount-of-time-with-powershell"></a>Erstellen einer Momentaufnahme aus einer VHD, um mehrere identisch verwaltete Datenträger in kürzester Zeit zu erstellen

Dieses Skript erstellt eine Momentaufnahme aus einer VHD-Datei in einem Speicherkonto in demselben oder einem anderen Abonnement. Verwenden Sie dieses Skript, um eine spezialisierte (nicht generalisierte/sysprepped) VHD in eine Momentaufnahme zu importieren. Verwenden Sie dann die Momentaufnahme, um mehrere identisch verwaltete Datenträger in kürzester Zeit zu erstellen. Außerdem verwenden Sie es zum Importieren einer Daten-VHD in eine Momentaufnahme und dann verwenden Sie die Momentaufnahme, um mehrere identisch verwaltete Datenträger in kürzester Zeit zu erstellen. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-powershell[Main](../../../powershell_scripts/storage/create-snapshots-from-vhd-in-different-subscription/create-snapshots-from-vhd-in-different-subscription.ps1 "Momentaufnahme von der VHD erstellen")]


## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle, um einen verwalteten Datenträger aus einer VHD in verschiedenen Abonnements zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [New-AzureRmDiskConfig](/powershell/module/azurerm.compute/New-AzureRmDiskConfig) | Erstellt die Datenträgerkonfiguration, die für die Datenträgererstellung verwendet wird. Dies umfasst den Speichertyp, den Speicherort, die Ressourcen-Id des Speicherkontos, in dem die übergeordnete VHD und VHD-URI der übergeordneter VHD-Datei gespeichert wird. |
| [New-AzureRmDisk](/powershell/module/azurerm.compute/New-AzureRmDisk) | Erstellt einen Datenträger mit Datenträgerkonfiguration, Datenträgername und Name der Ressourcengruppe, die als Parameter übergeben werden. |

## <a name="next-steps"></a>Nächste Schritte

[Erstellen Sie einen verwalteten Datenträger aus der Momentaufnahme](./../../storage/scripts/storage-windows-powershell-sample-create-managed-disk-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json)


[Erstellen Sie einen virtuellen Computer durch Anfügen eines verwalteten Datenträgers als Betriebssystemdatenträger](./../../virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/overview).

Zusätzliche VM-PowerShell-Skriptbeispiele finden Sie in der [Dokumentation zu Windows-VMs in Azure](../../virtual-machines/windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
