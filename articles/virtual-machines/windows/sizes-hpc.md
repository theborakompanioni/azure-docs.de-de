---
title: "Größen von virtuellen Azure Windows-Computern – HPC | Microsoft-Dokumentation"
description: "Auflistung der verschiedenen verfügbaren Größen für virtuelle Windows HPC-Computer (High Performance Computing) in Azure."
services: virtual-machines-windows
documentationcenter: 
author: jonbeck7
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/28/2017
ms.author: jonbeck
ms.translationtype: HT
ms.sourcegitcommit: 2ad539c85e01bc132a8171490a27fd807c8823a4
ms.openlocfilehash: 162638197d096294c27df5eb7817b65e90dd9729
ms.contentlocale: de-de
ms.lasthandoff: 07/12/2017

---

# <a name="high-performance-compute-vm-sizes"></a>Größen von virtuellen HPC-Computern (High Performance Computing)

[!INCLUDE [virtual-machines-common-sizes-hpc](../../../includes/virtual-machines-common-sizes-hpc.md)]

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## <a name="rdma-capable-instances"></a>RDMA-fähige Instanzen
Eine Teilmenge der rechenintensiven Instanzen (H16r, H16mr, A8 und A9) verfügt über eine Netzwerkschnittstelle für RDMA-Verbindungen (Remote Direct Memory Access). Diese Schnittstelle steht zusätzlich zur standardmäßigen Azure-Netzwerkschnittstelle anderer VM-Größen zur Verfügung. 
  
Über diese Schnittstelle können die RDMA-fähigen Instanzen über ein InfiniBand-Netzwerk kommunizieren. Dabei können FDR-Raten (virtuelle Computer der Größe H16r oder H16mr) bzw. QDR-Raten (virtuelle Computer der Größe A8 oder A9) verwendet werden. Durch diese RDMA-Funktionen können Skalierbarkeit und Leistung von MPI-Anwendungen (Message Passing Interface) gesteigert werden.

Es folgen Anforderungen an RDMA-fähige Windows-VMs für den Zugriff auf das Azure RDMA-Netzwerk: 

* **Betriebssystem**
  
  Windows Server 2012 R2, Windows Server 2012
  
  > [!NOTE]
  > Windows Server 2016 unterstützt derzeit keine RDMA-Verbindungen in Azure.
  >

* **Verfügbarkeitsgruppe oder Clouddienst :** Stellen Sie die RDMA-fähigen VMs in der gleichen Verfügbarkeitsgruppe (bei Verwenden des Azure Resource Manager-Bereitstellungsmodells) oder im gleichen Clouddienst (bei Verwenden des klassischen Bereitstellungsmodells) bereit. Bei Verwendung von Azure Batch müssen sich die RDMA-fähigen virtuellen Computer im gleichen Pool befinden.

* **MPI** : Microsoft MPI (MS-MPI) 2012 R2 oder höher, Intel MPI Library 5.x

  Unterstützte MPI-Implementierungen verwenden die Microsoft Network Direct-Schnittstelle für die Kommunikation zwischen Instanzen. 

* **RDMA-Netzwerkadressbereich:** Für das RDMA-Netzwerk in Azure wird der Adressbereich 172.16.0.0/16 reserviert. Wenn Sie MPI-Anwendungen auf Instanzen ausführen möchten, die in einem virtuellen Azure-Netzwerk bereitgestellt wurden, vergewissern Sie sich, dass der Adressraum des virtuellen Netzwerks sich nicht mit dem RDMA-Netzwerk überschneidet.

* **VM-Erweiterung HpcVmDrivers**: Auf RDMA-fähigen VMs müssen Sie die Erweiterung HpcVmDrivers hinzufügen, um Windows-Netzwerkgerätetreiber für RDMA-Verbindungen zu installieren. (In bestimmten Bereitstellungen von A8- und A9-Instanzen wird die Erweiterung HpcVmDrivers automatisch hinzugefügt.) Um die VM-Erweiterung einer VM hinzuzufügen, können Sie [Azure PowerShell](/powershell/azure/overview)-Cmdlets verwenden. 

  
  Der folgende Befehl installiert die neueste Version 1.1 der Erweiterung HpcVMDrivers auf einer vorhandenen RDMA-fähigen VM mit dem Namen *myVM*, die in der Ressourcengruppe mit dem Namen *myResourceGroup* in der Region *USA, Westen* bereitgestellt ist:

  ```PowerShell
  Set-AzureRmVMExtension -ResourceGroupName "myResourceGroup" -Location "westus" -VMName "myVM" -ExtensionName "HpcVmDrivers" -Publisher "Microsoft.HpcCompute" -Type "HpcVmDrivers" -TypeHandlerVersion "1.1"
  ```
  
  Weitere Informationen finden Sie unter [Erweiterungen und Features für virtuelle Computer](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Erweiterungen können auch für virtuelle Computer verwendet werden, die mit dem [klassischen Bereitstellungsmodell](classic/manage-extensions.md) bereitgestellt wurden.


## <a name="using-hpc-pack"></a>Verwenden von HPC Pack

[Microsoft HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), Microsofts kostenlose HPC-Cluster- und Auftragsverwaltungslösung, stellt eine Option zum Erstellen eines Computeclusters in Azure zum Ausführen Windows-basierter MPI-Anwendungen und anderer HPC-Workloads dar. HPC Pack 2012 R2 und neuere Versionen enthalten eine Laufzeitumgebung für MS-MPI, die das Azure RDMA-Netzwerk bei der Bereitstellung auf RDMA-fähigen VMs verwendet.




## <a name="other-sizes"></a>Andere Größen
- [Allgemeiner Zweck](sizes-general.md)
- [Computeoptimiert](sizes-compute.md)
- [Arbeitsspeicheroptimiert](../virtual-machines-windows-sizes-memory.md)
- [Speicheroptimiert](../virtual-machines-windows-sizes-storage.md)
- [GPU-optimiert](sizes-gpu.md)

## <a name="next-steps"></a>Nächste Schritte

- Prüflisten zur Verwendung rechenintensiver Instanzen mit HPC Pack unter Windows Server finden Sie unter [Einrichten eines Windows RDMA-Clusters mit HPC Pack zum Ausführen von MPI-Anwendungen](classic/hpcpack-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

- Informationen zur Verwendung rechenintensiver Instanzen zum Ausführen von MPI-Anwendungen mit Azure Batch finden Sie unter [Verwendung von Tasks mit mehreren Instanzen zum Ausführen von MPI-Anwendungen (Message Passing Interface) in Azure Batch](../../batch/batch-mpi.md).

- Weitere Informationen dazu, wie Sie mit [Azure-Computeeinheiten (ACU)](acu.md) die Computeleistung von Azure-SKUs vergleichen können.





