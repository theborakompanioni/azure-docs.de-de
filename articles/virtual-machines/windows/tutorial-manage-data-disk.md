---
title: "Verwalten von Azure-Datenträgern mit Azure PowerShell | Microsoft-Dokumentation"
description: "Tutorial: Verwalten von Azure-Datenträgern mit Azure PowerShell"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 04/21/2017
ms.author: nepeters
ms.translationtype: Human Translation
ms.sourcegitcommit: be3ac7755934bca00190db6e21b6527c91a77ec2
ms.openlocfilehash: d68fc54f3fd08b3d7d66a9cd1ddb91c340191468
ms.contentlocale: de-de
ms.lasthandoff: 05/03/2017

---

# <a name="manage-azure-disks-with-powershell"></a>Verwalten von Azure-Datenträgern mit PowerShell

In diesem Tutorial lernen Sie die verschiedenen Typen von VM-Datenträgern kennen und erhalten Informationen zum Auswählen einer Datenträgerkonfiguration sowie zum Erstellen und Anfügen von Datenträgern an virtuelle Azure-Computer. Darüber hinaus wird in diesem Tutorial erläutert, wie Datenträger-Momentaufnahmen erstellt werden.  

Die Schritte in diesem Tutorial können mit dem neuesten [Azure PowerShell](/powershell/azure/overview)-Modul ausgeführt werden.

## <a name="default-azure-disks"></a>Azure-Standarddatenträger

Beim Erstellen eines virtuellen Azure-Computers werden zwei Datenträger automatisch an den virtuellen Computer angefügt. 

**Betriebssystem-Datenträger**: Betriebssystem-Datenträger können in der Größe auf bis zu 1TB angepasst werden und hosten das Betriebssystem des virtuellen Computers.  Dem Betriebssystem-Datenträger wird standardmäßig der Laufwerkbuchstabe *c:* zugewiesen. Die Konfiguration der Datenträgerzwischenspeicherung des Betriebssystem-Datenträgers ist für die Leistung des Betriebssystems optimiert. Der Betriebssystem-Datenträger **sollte keine** Anwendungen oder Daten hosten. Verwenden Sie für Anwendungen und Daten einen Datenträger. Dies wird weiter unten in diesem Artikel ausführlich erläutert.

**Temporärer Datenträger:** Temporäre Datenträger verwenden ein Solid State Drive, das sich auf demselben Azure-Host wie der virtuelle Computer befindet. Temporäre Datenträger sind äußerst leistungsfähig und können für Vorgänge wie die temporäre Datenverarbeitung verwendet werden. Wenn der virtuelle Computer jedoch auf einen neuen Host verschoben wird, werden alle auf einem temporären Datenträger gespeicherten Daten entfernt. Die Größe des temporären Datenträgers richtet sich nach der Größe des virtuellen Computers. Temporären Datenträgern wird standardmäßig der Laufwerkbuchstabe *d:* zugewiesen.

### <a name="temporary-disk-sizes"></a>Größe von temporären Datenträgern

| Typ | Größe des virtuellen Computers | Max. Größe des temporären Datenträgers |
|----|----|----|
| [Allgemeiner Zweck](sizes-general.md) | A- und D-Serie | 800 |
| [Computeoptimiert](sizes-compute.md) | F-Serie | 800 |
| [Arbeitsspeicheroptimiert](../virtual-machines-windows-sizes-memory.md) | D- und G-Serie | 6.144 |
| [Speicheroptimiert](../virtual-machines-windows-sizes-storage.md) | L-Serie | 5.630 |
| [GPU](sizes-gpu.md) | N-Serie | 1.440 |
| [Hohe Leistung](sizes-hpc.md) | A- und H-Serie | 2000 |

## <a name="azure-data-disks"></a>Azure-Datenträger

Zum Installieren von Anwendungen und zum Speichern von Daten können weitere Datenträger hinzugefügt werden. Datenträger sollten in allen Fällen verwendet werden, in denen eine dauerhafte und dynamische Datenspeicherung erwünscht ist. Jeder Datenträger hat eine maximale Kapazität von 1 TB. Die Größe eines virtuellen Computers bestimmt die Anzahl der Datenträger, die an den virtuellen Computer angefügt werden können. Für jeden Kernspeicher eines virtuellen Computers können zwei Datenträger angefügt werden. 

### <a name="max-data-disks-per-vm"></a>Max. Anzahl der Datenträger pro virtuellem Computer

| Typ | Größe des virtuellen Computers | Max. Anzahl der Datenträger pro virtuellem Computer |
|----|----|----|
| [Allgemeiner Zweck](sizes-general.md) | A- und D-Serie | 32 |
| [Computeoptimiert](sizes-compute.md) | F-Serie | 32 |
| [Arbeitsspeicheroptimiert](../virtual-machines-windows-sizes-memory.md) | D- und G-Serie | 64 |
| [Speicheroptimiert](../virtual-machines-windows-sizes-storage.md) | L-Serie | 64 |
| [GPU](sizes-gpu.md) | N-Serie | 48 |
| [Hohe Leistung](sizes-hpc.md) | A- und H-Serie | 32 |

## <a name="vm-disk-types"></a>VM-Datenträgertypen

In Azure stehen zwei verschiedene Datenträgertypen zur Verfügung.

### <a name="standard-disk"></a>Standarddatenträger

Standardspeicher basiert auf Festplatten und stellt eine kostengünstige, performante Speicherlösung dar. Standarddatenträger sind ideal für eine kostengünstige Entwicklungs- und Testworkload.

### <a name="premium-disk"></a>Premium-Datenträger

Premium-Datenträger zeichnen sich durch SSD-basierte hohe Leistung und geringe Wartezeit aus. Sie eignen sich hervorragend für virtuelle Computer, auf denen die Produktionsworkload ausgeführt wird. Storage Premium unterstützt virtuelle Computer der DS-, DSv2-, GS- und FS-Serie. Premium-Datenträger gibt es in drei Varianten (P10 P20, P30). Der Datenträgertyp wird durch die Größe des Datenträgers vorgegeben. Bei der Auswahl einer Datenträgergröße wird der Wert auf den nächsten Datenträgertyp aufgerundet. Wenn die Größe beispielsweise unter 128 GB liegt, wird der Datenträgertyp P10 verwendet, bei einer Größe zwischen 129 und 512 GB der Datenträgertyp P20 und bei über 512 GB der Datenträgertyp P30. 

### <a name="premium-disk-performance"></a>Leistung von Premium-Datenträgern

|Storage Premium-Datenträgertyp | P10 | P20 | P30 |
| --- | --- | --- | --- |
| Datenträgergröße (aufgerundet) | 128 GB | 512 GB | 1.024 GB (1 TB) |
| IOPS pro Datenträger | 500 | 2.300 | 5.000 |
Durchsatz pro Datenträger | 100 MB/s | 150 MB/s | 200 MB/s |

In dieser Tabelle ist zwar die maximale IOPS-Anzahl pro Datenträger angegeben, eine höhere Leistung kann aber durch Striping mehrerer Datenträger erreicht werden. An einen virtuellen Standard_GS5-Computer können beispielsweise 64 Datenträger angefügt werden. Wenn jeder dieser Datenträger die Größe des Typs P30 aufweisen, kann eine maximale Größe von 80.000 IOPS erreicht werden. Ausführliche Informationen zur maximalen IOPS-Anzahl pro virtuellem Computer finden Sie unter [Größen für virtuelle Linux-Computer](./sizes.md).

## <a name="create-and-attach-disks"></a>Erstellen und Anfügen von Datenträgern

Für das Beispiel in diesem Tutorial muss ein virtueller Computer vorhanden sein. Gegebenenfalls können Sie mit diesem [Beispielskript](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) einen virtuellen Computer erstellen. Ersetzen Sie beim Durcharbeiten des Tutorials bei Bedarf den Namen der Ressourcengruppe und des virtuellen Computers.

Erstellen Sie mit [New-AzureRmDiskConfig](/powershell/module/azurerm.compute/new-azurermdiskconfig) die anfängliche Konfiguration. Im folgenden Beispiel wird ein Datenträger mit einer Größe von 128 GB erstellt.

```powershell
$diskConfig = New-AzureRmDiskConfig -Location EastUS -CreateOption Empty -DiskSizeGB 128
```

Erstellen Sie den Datenträger mit dem Befehl [New-AzureRmDisk](/powershell/module/azurerm.compute/new-azurermdisk).

```powershell
$dataDisk = New-AzureRmDisk -ResourceGroupName myResourceGroup -DiskName myDataDisk -Disk $diskConfig
```

Rufen Sie mit dem Befehl [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) den virtuellen Computer ab, den Sie dem Datenträger hinzufügen möchten.

```powershell
$vm = Get-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM
```

Fügen Sie mit dem Befehl [Add-AzureRmVMDataDisk](/powershell/module/azurerm.compute/add-azurermvmdatadisk) den Datenträger der Konfiguration des virtuellen Computers hinzu.

```powershell
$vm = Add-AzureRmVMDataDisk -VM $vm -Name myDataDisk -CreateOption Attach -ManagedDiskId $dataDisk.Id -Lun 1
```

Aktualisieren Sie den virtuellen Computer mit dem Befehl [Update-AzureRmVM](/powershell/module/azurerm.compute/add-azurermvmdatadisk).

```powershell
Update-AzureRmVM -ResourceGroupName myResourceGroup -VM $vm
```

## <a name="prepare-data-disks"></a>Vorbereiten von Datenträgern

Nach dem Anfügen eines Datenträgers an den virtuellen Computer muss das Betriebssystem zur Verwendung des Datenträgers konfiguriert werden. Im folgenden Beispiel wird gezeigt, wie der erste dem virtuellen Computer hinzugefügte Datenträger manuell konfiguriert wird. Dieser Vorgang kann auch mit der [benutzerdefinierten Skripterweiterung](./tutorial-automate-vm-deployment.md) automatisiert werden.

### <a name="manual-configuration"></a>Manuelle Konfiguration

Erstellen Sie eine RDP-Verbindung mit dem virtuellen Computer. Öffnen Sie PowerShell, und führen Sie das folgende Skript aus.

```powershell
Get-Disk | Where partitionstyle -eq 'raw' | `
Initialize-Disk -PartitionStyle MBR -PassThru | `
New-Partition -AssignDriveLetter -UseMaximumSize | `
Format-Volume -FileSystem NTFS -NewFileSystemLabel "myDataDisk" -Confirm:$false
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Informationen zu VM-Datenträgern erhalten. Im nächsten Tutorial erfahren Sie, wie die VM-Konfiguration automatisiert werden kann.

[Automatisieren der VM-Konfiguration](./tutorial-automate-vm-deployment.md)

