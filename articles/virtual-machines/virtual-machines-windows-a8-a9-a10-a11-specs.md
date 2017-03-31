---
title: Informationen zu rechenintensiven virtuellen Computern mit Windows | Microsoft Docs
description: "Hier finden Sie Hintergrundinformationen und Überlegungen zur Verwendung der H-Reihe von Azure sowie der rechenintensiven Größen A8, A9, A10 und A11 für virtuelle Windows-Computer und Clouddienste."
services: virtual-machines-windows, cloud-services
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 28d8e1f2-8e61-4fbe-bfe8-80a68443baba
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/14/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 4f2230ea0cc5b3e258a1a26a39e99433b04ffe18
ms.openlocfilehash: 1ab628ff6e7e8ed23c2c3b733907500dc6d4dde7
ms.lasthandoff: 03/25/2017


---
# <a name="about-h-series-and-compute-intensive-a-series-vms-for-windows"></a>Informationen zu virtuellen Computern der H-Reihe und der rechenintensiven A-Reihe für Windows
Hier finden Sie Hintergrundinformationen und einige Überlegungen zur Verwendung der neueren H-Reihe von Azure und der älteren Instanzen A8, A9, A10 und A11, die auch als *rechenintensive* Instanzen bezeichnet werden. Dieser Artikel konzentriert sich auf das Verwenden dieser Instanzen für virtuelle Windows-Computer. Sie können sie auch für [Linux-VMs](virtual-machines-linux-a8-a9-a10-a11-specs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) verwenden.

Informationen zu grundlegenden Spezifikationen, Speicherkapazitäten und Details zu den Datenträgern finden Sie unter [Größen für virtuelle Computer](virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

[!INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## <a name="access-to-the-rdma-network"></a>Zugreifen auf das RDMA-Netzwerk
RDMA-fähige müssen Instanzen folgende Anforderungen erfüllen, um auf das Azure RDMA-Netzwerk für Windows-MPI-Datenverkehr zugreifen zu können: 

* **Betriebssystem**
  
  * **Virtuelle Computer** : Windows Server 2012 R2, Windows Server 2012
  * **Clouddienste** : Gastbetriebssystemfamilie Windows Server 2012 R2, Windows Server 2012 oder Windows Server 2008 R2

    > [!NOTE]
    > Windows Server 2016 unterstützt derzeit keine RDMA-Verbindungen in Azure.
    >
    
* **MPI** : Microsoft MPI (MS-MPI) 2012 R2 oder höher, Intel MPI Library 5.x

  Unterstützte MPI-Implementierungen verwenden die Microsoft Network Direct-Schnittstelle für die Kommunikation zwischen Instanzen. 
* **HpcVmDrivers-VM-Erweiterung**: Auf RDMA-fähigen virtuellen Computern muss die HpcVmDrivers-Erweiterung hinzugefügt werden, um Windows-Netzwerkgerätetreiber zu installieren, die für RDMA-Verbindungen benötigt werden. (In bestimmten Bereitstellungen von A8- und A9-Instanzen wird die Erweiterung HpcVmDrivers automatisch hinzugefügt.) Wenn Sie die VM-Erweiterung einem virtuellen Computer hinzufügen müssen, können Sie dazu [Azure PowerShell](/powershell/azureps-cmdlets-docs)-Cmdlets verwenden. 

  
  So installieren Sie z.B. die neueste Version (1.1) der HpcVMDrivers-Erweiterung auf einem vorhandenen, RDMA-fähigen virtuellen Computer namens myVM im Resource Manager-Bereitstellungsmodell:

  ```PowerShell
  Set-AzureRmVMExtension -ResourceGroupName "myResourceGroup" -Location "westus" -VMName "myVM" -ExtensionName "HpcVmDrivers" -Publisher "Microsoft.HpcCompute" -Type "HpcVmDrivers" -TypeHandlerVersion "1.1"
  ```
  Weitere Informationen finden Sie unter [Erweiterungen und Features für virtuelle Computer](virtual-machines-windows-extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Erweiterungen können auch für virtuelle Computer verwendet werden, die mit dem [klassischen Bereitstellungsmodell](windows/classic/manage-extensions.md) bereitgestellt wurden.


## <a name="considerations-for-hpc-pack-and-windows"></a>Überlegungen zu HPC Pack und Windows
[Microsoft HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), die kostenlose HPC-Cluster- und Auftragsverwaltungslösung von Microsoft, ist für die Verwendung der rechenintensiven Instanzen mit Windows Server nicht erforderlich. Sie stellt jedoch ein Option zum Erstellen eines Computeclusters in Azure zum Ausführen Windows-basierter MPI-Anwendungen und anderer HPC-Workloads dar. HPC Pack 2012 R2 und neuere Versionen enthalten eine Laufzeitumgebung für MS-MPI, die das Azure RDMA-Netzwerk bei der Bereitstellung auf RDMA-fähigen virtuellen Computern verwendet.

Weitere Informationen und Prüflisten zur Verwendung rechenintensiver Instanzen mit HPC Pack unter Windows Server finden Sie unter [Einrichten eines Windows RDMA-Clusters mit HPC Pack zum Ausführen von MPI-Anwendungen](windows/classic/hpcpack-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="next-steps"></a>Nächste Schritte
* Ausführliche Informationen zur Verfügbarkeit und Preisgestaltung der rechenintensiven Größen finden Sie unter [Virtuelle Computer – Preise](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows) sowie unter [Cloud Services – Preise](https://azure.microsoft.com/pricing/details/cloud-services/).
* Informationen zu Speicherkapazitäten und Details zu den Datenträgern finden Sie unter [Größen für virtuelle Computer](virtual-machines-linux-sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Informationen zu den ersten Schritten bei der Bereitstellung und Verwendung rechenintensiver Instanzen mit HPC Pack unter Windows finden Sie unter [Einrichten eines Windows RDMA-Clusters mit HPC Pack zum Ausführen von MPI-Anwendungen](windows/classic/hpcpack-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
* Informationen zur Verwendung rechenintensiver Instanzen zum Ausführen von MPI-Anwendungen mit Azure Batch finden Sie unter [Verwendung von Tasks mit mehreren Instanzen zum Ausführen von MPI-Anwendungen (Message Passing Interface) in Azure Batch](../batch/batch-mpi.md).


