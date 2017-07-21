---
title: "Azure PowerShell-Beispielskript – Exportieren/Kopieren einer Momentaufnahme als VHD in ein Speicherkonto in einer anderen Region | Microsoft-Dokumentation"
description: "Azure PowerShell-Beispielskript – Exportieren/Kopieren einer Momentaufnahme als VHD in ein Speicherkonto in einer anderen Region"
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
ms.translationtype: Human Translation
ms.sourcegitcommit: 09f24fa2b55d298cfbbf3de71334de579fbf2ecd
ms.openlocfilehash: 7c3a5521e3e62c05b2d06296d99459e51cc3aeb4
ms.contentlocale: de-de
ms.lasthandoff: 06/07/2017

---

# <a name="exportcopy-managed-snapshots-as-vhd-to-a-storage-account-in-different-region-with-powershell"></a>Exportieren/Kopieren verwalteter Momentaufnahmen als VHD in ein Speicherkonto in einer anderen Region mit PowerShell

Dieses Skript exportiert eine verwaltete Momentaufnahme in ein Speicherkonto in einer anderen Region. Zuerst generiert es den SAS-URI der Momentaufnahme, den es anschließend verwendet, um dieses in ein Speicherkonto in einer anderen Region zu kopieren. Verwenden Sie dieses Skript, um Sicherungen Ihrer verwalteten Datenträger für die Notfallwiederherstellung in einer anderen Region zu verwalten.  

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/storage/copy-snapshot-to-storage-account/copy-snapshot-to-storage-account.ps1 "Momentaufnahmen kopieren")]


## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle zum Generieren des SAS-URI für eine verwaltete Momentaufnahme und zum Kopieren der Momentaufnahme in ein Speicherkonto mithilfe dieses SAS-URI. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [Grant-AzureRmSnapshotAccess](/powershell/module/azurerm.compute/New-AzureRmDisk) | Generiert ein SAS-URI für eine Momentaufnahme zum Kopieren in ein anderes Speicherkonto. |
| [New-AzureStorageContext](/powershell/module/azure.storage/New-AzureStorageContext) | Erstellt einen Speicherkontokontext, der den Kontonamen und -schlüssel verwendet. Dieser Kontext kann verwendet werden, um Lese-/Schreibvorgänge im Speicherkonto auszuführen. |
| [Start-AzureStorageBlobCopy](/powershell/module/azure.storage/Start-AzureStorageBlobCopy) | Kopiert die zugrunde liegende VHD einer Momentaufnahme in ein Speicherkonto. |

## <a name="next-steps"></a>Nächste Schritte

[Create a managed disk from a VHD (Erstellen eines verwalteten Datenträgers aus einer VHD)](./../scripts/storage-windows-powershell-sample-create-managed-disk-from-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json)

[Erstellen virtueller Computer aus verwalteten Datenträgern](./../../virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/overview).

Zusätzliche VM-PowerShell-Skriptbeispiele finden Sie in der [Dokumentation zu Windows-VMs in Azure](../../virtual-machines/windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
