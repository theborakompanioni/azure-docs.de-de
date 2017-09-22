---
title: "Azure PowerShell-Skript-Beispiel: Erstellen einer Momentaufnahme aus einer VHD, um mehrere identisch verwaltete Datenträger in kürzester Zeit zu erstellen | Microsoft-Dokumentation"
description: "Azure PowerShell-Skript-Beispiel: Erstellen einer Momentaufnahme aus einer VHD, um mehrere identisch verwaltete Datenträger in kürzester Zeit zu erstellen"
services: virtual-machines-windows
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 06/05/2017
ms.author: ramankum
ms.translationtype: HT
ms.sourcegitcommit: 8f9234fe1f33625685b66e1d0e0024469f54f95c
ms.openlocfilehash: ae21e8ab5b718cc8cedb33aa31a399ebf7a80379
ms.contentlocale: de-de
ms.lasthandoff: 09/20/2017

---

# <a name="create-a-snapshot-from-a-vhd-to-create-multiple-identical-managed-disks-in-small-amount-of-time-with-powershell"></a>Erstellen einer Momentaufnahme aus einer VHD, um mehrere identisch verwaltete Datenträger in kürzester Zeit zu erstellen

Dieses Skript erstellt eine Momentaufnahme aus einer VHD-Datei in einem Speicherkonto in demselben oder einem anderen Abonnement. Verwenden Sie dieses Skript, um eine spezialisierte (nicht generalisierte/sysprepped) VHD in eine Momentaufnahme zu importieren. Verwenden Sie dann die Momentaufnahme, um mehrere identisch verwaltete Datenträger in kürzester Zeit zu erstellen. Außerdem verwenden Sie es zum Importieren einer Daten-VHD in eine Momentaufnahme und dann verwenden Sie die Momentaufnahme, um mehrere identisch verwaltete Datenträger in kürzester Zeit zu erstellen. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-snapshots-from-vhd-in-different-subscription/create-snapshots-from-vhd-in-different-subscription.ps1 "Create snapshot from VHD")]


## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle, um einen verwalteten Datenträger aus einer VHD in verschiedenen Abonnements zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [New-AzureRmDiskConfig](/powershell/module/azurerm.compute/New-AzureRmDiskConfig) | Erstellt die Datenträgerkonfiguration, die für die Datenträgererstellung verwendet wird. Dies umfasst den Speichertyp, den Speicherort, die Ressourcen-Id des Speicherkontos, in dem die übergeordnete VHD und VHD-URI der übergeordneter VHD-Datei gespeichert wird. |
| [New-AzureRmDisk](/powershell/module/azurerm.compute/New-AzureRmDisk) | Erstellt einen Datenträger mit Datenträgerkonfiguration, Datenträgername und Name der Ressourcengruppe, die als Parameter übergeben werden. |

## <a name="next-steps"></a>Nächste Schritte

[Erstellen Sie einen verwalteten Datenträger aus der Momentaufnahme](virtual-machines-windows-powershell-sample-create-managed-disk-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json)


[Erstellen Sie einen virtuellen Computer durch Anfügen eines verwalteten Datenträgers als Betriebssystemdatenträger](./virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/overview).

Zusätzliche VM-PowerShell-Skriptbeispiele finden Sie in der [Dokumentation zu Windows-VMs in Azure](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
