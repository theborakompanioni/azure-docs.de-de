---
title: Informationen zu rechenintensiven virtuellen Computern mit Linux | Microsoft Docs
description: "Hier finden Sie Hintergrundinformationen und Überlegungen zur Verwendung der H-Serie und der rechenintensiven Größen A8, A9, A10 und A11 für virtuelle Linux-Computer."
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 16cf6e2d-f401-4b22-af8c-9a337179f5f6
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/14/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: fd35f1774ffda3d3751a6fa4b6e17f2132274916
ms.openlocfilehash: e6818ea7db680f34bb4ee900396c74fc6e42c4b6
ms.lasthandoff: 03/16/2017

---
# <a name="about-h-series-and-compute-intensive-a-series-vms-for-linux"></a>Informationen zu virtuellen Computern der H-Reihe und der rechenintensiven A-Reihe für Linux
Hier finden Sie Hintergrundinformationen und einige Überlegungen zur Verwendung der neueren Azure H-Serie und der älteren Größen A8, A9, A10 und A11, die auch als *rechenintensive* Instanzen bezeichnet werden. Dieser Artikel konzentriert sich auf die Verwendung dieser Größen für virtuelle Linux-Computer. Sie können sie auch für [Windows-VMs](virtual-machines-windows-a8-a9-a10-a11-specs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) verwenden. 

Informationen zu grundlegenden Spezifikationen, Speicherkapazitäten und Details zu den Datenträgern finden Sie unter [Größen für virtuelle Computer](virtual-machines-linux-sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

[!INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## <a name="access-to-the-rdma-network"></a>Zugreifen auf das RDMA-Netzwerk
Sie können Cluster mit RDMA-fähigen virtuellen Linux-Computern erstellen, auf denen eine der folgenden unterstützten Linux-HPC-Distributionen und eine unterstützte MPI-Implementierung ausgeführt werden, um von den Vorteilen des Azure RDMA-Netzwerks zu profitieren. Bereitstellungsoptionen und Beispielkonfigurationsschritte finden Sie unter [Einrichten eines Linux RDMA-Clusters zum Ausführen von MPI-Anwendungen](virtual-machines-linux-classic-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) .

* **Distributionen:** Sie müssen virtuelle Computer auf der Grundlage von RDMA-fähigen SLES-basierten (SUSE Linux Enterprise Server) oder OpenLogic CentOS-basierten HPC-Images aus dem Azure Marketplace bereitstellen. Die folgenden Marketplace-Images unterstützen RDMA-Verbindungen:
  
    * SLES 12 SP1 für HPC, SLES 12 SP1 für HPC (Premium)
    
    * 7.1 HPC (CentOS-basiert), 6.5 HPC (CentOS-basiert)  
 
        > [!NOTE]
        > Für virtuelle Computer der H-Serie empfehlen wir entweder ein Image vom Typ „SLES 12 SP1 für HPC“ oder ein Image vom Typ „7.1 HPC (CentOS-basiert)“.
        >
        > Bei den CentOS-basierten HPC-Images sind Kernel-Updates in der **yum** -Konfigurationsdatei deaktiviert. Der Grund: Die Linux RDMA-Treiber werden als RPM-Paket verteilt, und Treiberupdates funktionieren möglicherweise nicht, wenn der Kernel aktualisiert wird.
        > 
        > 
* **MPI** : Intel MPI Library 5.x
  
    Je nach verwendetem Marketplace-Image sind für Intel MPI unter Umständen zusätzliche Lizenzierungs-, Installations- oder Konfigurationsschritte erforderlich: 
  
  * **SLES 12 SP1 für HPC-Image:** Intel-MPI-Pakete werden auf dem virtuellen Computer verteilt. Die Installation erfolgt, indem Sie den folgenden Befehl ausführen:

      ```bash
      sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm
      ```

  * **CentOS-basierte HPC-Images**: Intel MPI 5.1 ist bereits vorinstalliert.  
    
    Wenn Sie MPI-Aufträge auf gruppierten virtuellen Computer ausführen möchten, sind zusätzliche Konfigurationsschritte erforderlich. So müssen Sie in einem Cluster mit virtuellen Computern beispielsweise die Vertrauensstellung zwischen den Computeknoten einrichten. Informationen zu typischen Einstellungen finden Sie unter [Einrichten eines Linux RDMA-Clusters zum Ausführen von MPI-Anwendungen](virtual-machines-linux-classic-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

## <a name="considerations-for-hpc-pack-and-linux"></a>Überlegungen zu HPC Pack und Linux
[HPC Pack](https://technet.microsoft.com/library/jj899572.aspx)ist eine kostenlose HPC-Cluster- und Auftragsverwaltungslösung von Microsoft und bietet eine Option für die Verwendung der rechenintensiven Instanzen mit Linux. Die neuesten Versionen von HPC Pack unterstützen das Ausführen von mehreren Linux-Distributionen auf Serverknoten, die auf virtuellen Azure-Computern bereitgestellt wurden und von einem Windows Server-Hauptknoten verwaltet werden. Mit RDMA-fähigen Linux-Computeknoten, auf denen Intel MPI ausgeführt wird, kann HPC Pack Linux-basierte MPI-Anwendungen mit Zugriff auf das RDMA-Netzwerk planen und ausführen. Eine Einführung finden Sie unter [Erste Schritte mit Linux-Computeknoten in einem HPC Pack-Cluster in Azure](virtual-machines-linux-classic-hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

## <a name="network-topology-considerations"></a>Überlegungen zur Netzwerktopologie
* Auf RDMA-fähigen virtuellen Linux-Computern in Azure ist „Eth1“ für RDMA-Netzwerkdatenverkehr reserviert. Ändern Sie keine Eth1-Einstellungen oder anderen Informationen in der Konfigurationsdatei, die sich auf dieses Netzwerk beziehen. „Eth0“ ist für den regulären Azure-Netzwerkverkehr reserviert.
* In Azure wird IP over InfiniBand (IB) nicht unterstützt. Nur RDMA over IB wird unterstützt.



## <a name="next-steps"></a>Nächste Schritte
* Ausführliche Informationen zu Verfügbarkeit und Preisen rechenintensiver Größen finden Sie unter [Virtuelle Linux-Computer – Preise](https://azure.microsoft.com/pricing/details/virtual-machines/#Linux).
* Informationen zu Speicherkapazitäten und Details zu den Datenträgern finden Sie unter [Größen für virtuelle Computer](virtual-machines-linux-sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Informationen zu ersten Schritten bei der Bereitstellung und Verwendung rechenintensiver Größen mit RDMA unter Linux finden Sie unter [Einrichten eines Linux RDMA-Clusters zum Ausführen von MPI-Anwendungen](virtual-machines-linux-classic-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).


