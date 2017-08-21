---
title: "Größen von virtuellen Azure Linux-Computern – HPC | Microsoft-Dokumentation"
description: "Auflistung der verschiedenen verfügbaren Größen für virtuelle Linux HPC-Computer (High Performance Computing) in Azure."
services: virtual-machines-linux
documentationcenter: 
author: jonbeck7
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/28/2017
ms.author: jonbeck
ms.translationtype: HT
ms.sourcegitcommit: 49bc337dac9d3372da188afc3fa7dff8e907c905
ms.openlocfilehash: 4962c26e6bf2ed36ce670cc78f4ecfcab871a8bf
ms.contentlocale: de-de
ms.lasthandoff: 07/14/2017

---

# <a name="high-performance-compute-linux-vm-sizes"></a>Größen von virtuellen Linux HPC-Computern (High Performance Computing)

[!INCLUDE [virtual-machines-common-sizes-hpc](../../../includes/virtual-machines-common-sizes-hpc.md)]

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## <a name="rdma-capable-instances"></a>RDMA-fähige Instanzen
Eine Teilmenge der rechenintensiven Instanzen (H16r, H16mr, A8 und A9) verfügt über eine Netzwerkschnittstelle für RDMA-Verbindungen (Remote Direct Memory Access). Diese Schnittstelle steht zusätzlich zur standardmäßigen Azure-Netzwerkschnittstelle anderer VM-Größen zur Verfügung. 
  
Über diese Schnittstelle können die RDMA-fähigen Instanzen über ein InfiniBand-Netzwerk kommunizieren. Dabei können FDR-Raten (virtuelle Computer der Größe H16r oder H16mr) bzw. QDR-Raten (virtuelle Computer der Größe A8 oder A9) verwendet werden. Durch diese RDMA-Funktionen können Skalierbarkeit und Leistung von MPI-Anwendungen (Message Passing Interface) gesteigert werden.

Es folgen Anforderungen für RDMA-fähige Linux-VMs für den Zugriff auf das Azure RDMA-Netzwerk:
 
* **Distributionen:** Sie müssen virtuelle Computer auf der Grundlage von RDMA-fähigen SLES-basierten (SUSE Linux Enterprise Server) oder Rogue Wave Software (früher OpenLogic) CentOS-basierten HPC-Images aus dem Azure Marketplace bereitstellen. Die folgenden Marketplace-Images unterstützen RDMA-Verbindungen:
  
    * SLES 12 SP1 für HPC oder SLES 12 SP1 für HPC (Premium)
    
    * 7.3 (CentOS-basiert), 7.1 HPC (CentOS-basiert), 6.8 (CentOS-basiert) oder 6.5 HPC (CentOS-basiert)  
 
        > [!NOTE]
        > Für virtuelle Computer der H-Serie empfehlen wir entweder ein Image des Typs „SLES 12 SP1 für HPC“ oder ein Image des Typs „7.1 HPC (CentOS-basiert)“ oder höher.
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
    
    Wenn Sie MPI-Aufträge auf gruppierten virtuellen Computer ausführen möchten, sind zusätzliche Konfigurationsschritte erforderlich. So müssen Sie in einem Cluster mit virtuellen Computern beispielsweise die Vertrauensstellung zwischen den Computeknoten einrichten. Informationen zu typischen Einstellungen finden Sie unter [Einrichten eines Linux RDMA-Clusters zum Ausführen von MPI-Anwendungen](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

### <a name="network-topology-considerations"></a>Überlegungen zur Netzwerktopologie
* Auf RDMA-fähigen virtuellen Linux-Computern in Azure ist „Eth1“ für RDMA-Netzwerkdatenverkehr reserviert. Ändern Sie keine Eth1-Einstellungen oder anderen Informationen in der Konfigurationsdatei, die sich auf dieses Netzwerk beziehen. „Eth0“ ist für den regulären Azure-Netzwerkverkehr reserviert.

* In Azure wird IP over InfiniBand (IB) nicht unterstützt. Nur RDMA over IB wird unterstützt.

## <a name="using-hpc-pack"></a>Verwenden von HPC Pack
[HPC Pack](https://technet.microsoft.com/library/jj899572.aspx)ist eine kostenlose HPC-Cluster- und Auftragsverwaltungslösung von Microsoft und bietet eine Option für die Verwendung der rechenintensiven Instanzen mit Linux. Die neuesten Versionen von HPC Pack unterstützen das Ausführen von mehreren Linux-Distributionen auf Serverknoten, die auf virtuellen Azure-Computern bereitgestellt wurden und von einem Windows Server-Hauptknoten verwaltet werden. Mit RDMA-fähigen Linux-Computeknoten, auf denen Intel MPI ausgeführt wird, kann HPC Pack Linux-basierte MPI-Anwendungen mit Zugriff auf das RDMA-Netzwerk planen und ausführen. Informationen finden Sie unter [Erste Schritte mit Linux-Computeknoten in einem HPC Pack-Cluster in Azure](classic/hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

## <a name="other-sizes"></a>Andere Größen
- [Allgemeiner Zweck](sizes-general.md)
- [Computeoptimiert](sizes-compute.md)
- [Arbeitsspeicheroptimiert](sizes-memory.md)
- [Speicheroptimiert](sizes-storage.md)
- [GPU](../windows/sizes-gpu.md)


## <a name="next-steps"></a>Nächste Schritte

- Informationen zu ersten Schritten bei der Bereitstellung und Verwendung rechenintensiver Größen mit RDMA unter Linux finden Sie unter [Einrichten eines Linux RDMA-Clusters zum Ausführen von MPI-Anwendungen](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

- Weitere Informationen dazu, wie Sie mit [Azure-Computeeinheiten (ACU)](acu.md) die Computeleistung von Azure-SKUs vergleichen können.





