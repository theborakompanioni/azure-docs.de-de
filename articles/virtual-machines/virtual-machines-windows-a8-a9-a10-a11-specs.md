---
title: Informationen zu rechenintensiven virtuellen Computern mit Windows | Microsoft Docs
description: Hier finden Sie Hintergrundinformationen und Überlegungen zur Verwendung der H-Reihe von Azure sowie der rechenintensiven Größen A8, A9, A10 und A11 für virtuelle Windows-Computer und Clouddienste.
services: virtual-machines-windows, cloud-services
documentationcenter: ''
author: dlepow
manager: timlt
editor: ''
tags: azure-resource-manager,azure-service-management

ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 09/21/2016
ms.author: danlep

---
# Informationen zu virtuellen Computern der H-Reihe und der rechenintensiven A-Reihe
Hier finden Sie Hintergrundinformationen und einige Überlegungen zur Verwendung der neueren H-Reihe von Azure und der älteren Instanzen A8, A9, A10 und A11, die auch als *rechenintensive* Instanzen bezeichnet werden. Dieser Artikel konzentriert sich auf das Verwenden dieser Instanzen für virtuelle Windows-Computer. Dieser Artikel ist auch für [virtuelle Linux-Computer](virtual-machines-linux-a8-a9-a10-a11-specs.md) verfügbar.

[!INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## Zugreifen auf das RDMA-Netzwerk
Sie können Cluster von RDMA-fähigen Windows Server-Instanzen erstellen und eine der unterstützten MPI-Implementierungen bereitstellen, um das Azure RDMA-Netzwerk zu nutzen. Dieses Netzwerk mit niedriger Latenz und hohem Durchsatz ist für MPI-Verkehr reserviert.

* **Betriebssystem**
  
  * **Virtuelle Computer**: Windows Server 2012 R2, Windows Server 2012
  * **Clouddienste**: Gastbetriebssystemfamilie Windows Server 2012 R2, Windows Server 2012 oder Windows Server 2008 R2
* **MPI**: Microsoft MPI (MS-MPI) 2012 R2 oder höher, Intel MPI Library 5.x

Unterstützte MPI-Implementierungen verwenden die Microsoft Network Direct-Schnittstelle für die Kommunikation zwischen Instanzen. Bereitstellungsoptionen und Beispielkonfigurationsschritte finden Sie unter [Einrichten eines Windows RDMA-Clusters mit HPC Pack zum Ausführen von MPI-Anwendungen](virtual-machines-windows-classic-hpcpack-rdma-cluster.md) und [Verwendung von Tasks mit mehreren Instanzen zum Ausführen von MPI-Anwendungen (Message Passing Interface) in Azure Batch](../batch/batch-mpi.md).

> [!NOTE]
> Auf RDMA-fähigen, rechenintensiven virtuellen Computern muss den virtuellen Computern die HpcVmDrivers-Erweiterung hinzugefügt werden, damit die für RDMA-Verbindungen benötigten Windows-Netzwerkgerätetreiber installiert werden. In den meisten Bereitstellungen wird die HpcVmDrivers-Erweiterung automatisch hinzugefügt. Wenn Sie die Erweiterung selbst hinzufügen müssen, finden Sie unter [Verwalten von Erweiterungen für virtuelle Computer](virtual-machines-windows-classic-manage-extensions.md) weitere Informationen.
> 
> 

## Überlegungen zu HPC Pack und Windows
[Microsoft HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), die kostenlose HPC-Cluster- und Auftragsverwaltungslösung von Microsoft, ist für die Verwendung der rechenintensiven Instanzen mit Windows Server nicht erforderlich. Sie stellt jedoch ein Option zum Erstellen eines Computeclusters in Azure zum Ausführen Windows-basierter MPI-Anwendungen und anderer HPC-Workloads dar. HPC Pack 2012 R2 und neuere Versionen enthalten eine Laufzeitumgebung für MS-MPI, die das Azure RDMA-Netzwerk bei der Bereitstellung auf RDMA-fähigen virtuellen Computern verwendet.

Weitere Informationen und Prüflisten zur Verwendung rechenintensiver Instanzen mit HPC Pack auf Windows Server finden Sie unter [Einrichten eines Windows RDMA-Clusters mit HPC Pack zum Ausführen von MPI-Anwendungen](virtual-machines-windows-classic-hpcpack-rdma-cluster.md).

## Nächste Schritte
* Ausführliche Informationen zur Verfügbarkeit und Preisgestaltung der rechenintensiven Größen finden Sie unter [Virtuelle Computer – Preise](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows) sowie unter [Cloud Services – Preise](https://azure.microsoft.com/pricing/details/cloud-services/).
* Informationen zu Speicherkapazitäten und Details zu den Datenträgern finden Sie unter [Größen für virtuelle Computer](virtual-machines-linux-sizes.md).
* Informationen zu ersten Schritten für die Bereitstellung und Verwendung rechenintensiver Instanzen mit HPC Pack unter Windows finden Sie unter [Einrichten eines Windows RDMA-Clusters mit HPC Pack zum Ausführen von MPI-Anwendungen](virtual-machines-windows-classic-hpcpack-rdma-cluster.md).
* Informationen zur Verwendung von A8- und A9-Instanzen zum Ausführen von MPI-Anwendungen mit Azure Batch finden Sie unter [Verwendung von Tasks mit mehreren Instanzen zum Ausführen von MPI-Anwendungen (Message Passing Interface) in Azure Batch](../batch/batch-mpi.md).

<!---HONumber=AcomDC_0928_2016-->