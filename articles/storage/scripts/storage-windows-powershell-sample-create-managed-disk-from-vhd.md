---
title: "Azure PowerShell-Beispielskript: Erstellen verwalteter Datenträger aus einer VHD-Datei in einem Speicherkonto in demselben oder einem anderen Abonnement | Microsoft-Dokumentation"
description: "Azure PowerShell-Beispielskript: Erstellen verwalteter Datenträger aus einer VHD-Datei in einem Speicherkonto in demselben oder einem anderen Abonnement"
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
ms.openlocfilehash: 079ab69da3c2cb6cc38f0a766ffeb47ed84c90a3
ms.contentlocale: de-de
ms.lasthandoff: 06/07/2017

---

# <a name="create-a-managed-disk-from-a-vhd-file-in-a-storage-account-in-same-or-different-subscription-with-powershell"></a>Erstellen verwalteter Datenträger aus einer VHD-Datei in einem Speicherkonto in demselben oder einem anderen Abonnement mithilfe von PowerShell

Dieses Skript erstellt einen verwalteten Datenträger aus einer VHD-Datei in einem Speicherkonto in demselben oder einem anderen Abonnement. Sie können dieses Skript verwenden, um eine bestimmte (nicht generalisierte/mit Sysprep vorbereitete) VHD auf den verwalteten Betriebssystem-Datenträger zu importieren, um damit einen virtuellen Computer zu erstellen. Außerdem können Sie damit eine Daten-VHD auf verwaltete Datenträger importieren. 

Erstellen Sie nicht mehrere identische verwaltete Datenträger aus einer VHD-Datei in einem kurzen Zeitraum. Um verwaltete Datenträger aus einer VHD-Datei zu erstellen, wird eine Blob-Momentaufnahme der VHD-Datei erstellt und anschließend verwendet, um verwaltete Datenträger zu erstellen. Innerhalb einer Minute kann nur eine Blob-Momentaufnahme erstellt werden, die aufgrund der Drosselung Fehler bei der Datenträgererstellung auslösen kann. Erstellen Sie eine verwaltete Momentaufnahme aus der VHD-Datei, um diese Drosselung zu vermeiden (weitere Informationen finden Sie unter [Create a snapshot from a VHD](./../scripts/storage-windows-powershell-sample-create-snapshot-from-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json)). Verwenden Sie anschließend die verwaltete Momentaufnahme, um in einer kurzen Zeitspanne mehrere verwaltete Datenträger zu erstellen. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/storage/create-managed-disks-from-vhd-in-different-subscription/create-managed-disks-from-vhd-in-different-subscription.ps1 "Verwaltete Datenträger aus VHDs erstellen")]


## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle, um einen verwalteten Datenträger aus einer VHD in verschiedenen Abonnements zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [New-AzureRmDiskConfig](/powershell/module/azurerm.compute/New-AzureRmDiskConfig) | Erstellt die Datenträgerkonfiguration, die für die Datenträgererstellung verwendet wird. Dies umfasst den Speichertyp, den Speicherort, die Ressourcen-ID des Speicherkontos, in dem die übergeordnete VHD und VHD-URI der übergeordneter VHD-Datei gespeichert wird. |
| [New-AzureRmDisk](/powershell/module/azurerm.compute/New-AzureRmDisk) | Erstellt einen Datenträger mit Datenträgerkonfiguration, Datenträgername und Name der Ressourcengruppe, die als Parameter übergeben werden. |

## <a name="next-steps"></a>Nächste Schritte

[Erstellen eines virtuellen Computers mit einem vorhandenen verwalteten Betriebssystemdatenträger mit PowerShell](./../../virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/overview).

Zusätzliche VM-PowerShell-Skriptbeispiele finden Sie in der [Dokumentation zu Windows-VMs in Azure](../../virtual-machines/windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
