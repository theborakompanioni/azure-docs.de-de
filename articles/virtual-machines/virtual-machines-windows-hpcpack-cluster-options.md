---
title: Windows HPC Pack-Clusteroptionen in der Cloud | Microsoft Docs
description: Informationen zu Optionen mit Microsoft HPC Pack, um einen Windows HPC-Cluster (High Performance Computing) in der Azure-Cloud zu erstellen und zu verwalten.
services: virtual-machines-windows,cloud-services,batch
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management,hpc-pack
ms.assetid: 02c5566d-2129-483c-9ecf-0d61030442d7
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 02/06/2017
ms.author: danlep
translationtype: Human Translation
ms.sourcegitcommit: a0e80acad4e8db2177e699f90f7296c64b38e718
ms.openlocfilehash: 5124f9253950b032fa39c805cb355cc8bfb718c5
ms.lasthandoff: 02/07/2017


---
# <a name="options-with-hpc-pack-to-create-and-manage-a-windows-hpc-cluster-in-azure"></a>Optionen zum Erstellen und Verwalten eines Windows HPC-Clusters (High Performance Computing) in Azure mit Microsoft HPC Pack
[!INCLUDE [virtual-machines-common-hpcpack-cluster-options](../../includes/virtual-machines-common-hpcpack-cluster-options.md)]

Dieser Artikel konzentriert sich auf Optionen zum Erstellen von HPC Pack-Clustern zum Ausführen von Windows-Workloads. Es gibt auch Optionen zum Erstellen von HPC Pack-Clustern zur Ausführung von [Linux-HPC-Workloads](virtual-machines-linux-hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


## <a name="run-an-hpc-pack-cluster-in-azure-vms"></a>Ausführen eines HPC Pack-Clusters in virtuellen Azure-Computern
### <a name="azure-templates"></a>Azure-Vorlagen
* (GitHub) [HPC Pack 2016 cluster templates](https://github.com/MsHpcPack/HPCPack2016)
* (Marketplace) [HPC Pack cluster for Windows workloads](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterwindowscn/)
* (Marketplace) [HPC Pack cluster for Excel workloads](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterexcelcn/)
* (Schnellstart) [Create an HPC cluster](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster)
* (Schnellstart) [Create an HPC cluster with custom compute node image](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster-custom-image)

### <a name="azure-vm-images"></a>Azure VM-Images
* [HPC Pack 2016-Hauptknoten unter Windows Server 2016](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.HPCPack2016HeadNodeonWindowsServer2016?tab=Overview)
* [HPC Pack 2016-Computeknoten unter Windows Server 2016](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.HPCPack2016ComputeNodeonWindowsServer2016?tab=Overview)
* [HPC Pack 2016-Hauptknoten unter Windows Server 2012 R2](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.HPCPack2016HeadNodeonWindowsServer2012R2?tab=Overview)
* [HPC Pack 2016-Computeknoten unter Windows Server 2012 R2](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.HPCPack2016ComputeNodeonWindowsServer2012R2?tab=Overview)
* [HPC Pack 2012 R2-Hauptknoten unter Windows Server 2012 R2](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/)
* [HPC Pack 2012 R2-Computeknoten unter Windows Server 2012 R2](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2computenodeonwindowsserver2012r2/)
* [HPC Pack-Computeknoten mit Excel unter Windows Server 2012 R2](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2computenodewithexcelonwindowsserver2012r2/)

### <a name="powershell-deployment-script"></a>PowerShell-Bereitstellungsskript
* [Erstellen eines HPC-Clusters mit dem HPC Pack IaaS-Bereitstellungsskript](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

### <a name="tutorials"></a>Lernprogramme
* [Tutorial: Bereitstellen eines HPC Pack 2016-Clusters in Azure](virtual-machines-windows-hpcpack-2016-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Tutorial: Erste Schritte mit einem HPC Pack-Cluster in Azure zum Ausführen von Excel- und SOA-Workloads](virtual-machines-windows-excel-cluster-hpcpack.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

### <a name="manual-deployment-with-the-azure-portal"></a>Manuelle Bereitstellung über das Azure-Portal
* [Einrichten des Hauptknotens eines HPC Pack-Clusters in einem virtuellen Azure-Computer](virtual-machines-windows-hpcpack-cluster-headnode.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

### <a name="cluster-management"></a>Clusterverwaltung
* [Verwalten von Computeknoten in einem HPC Pack-Cluster in Azure](virtual-machines-windows-classic-hpcpack-cluster-node-manage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [Vergrößern und Verkleinern von Azure-Compute-Ressourcen in einem HPC Pack-Cluster](virtual-machines-windows-classic-hpcpack-cluster-node-autogrowshrink.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [Übermitteln von Aufträgen an einen HPC Pack-Cluster in Azure](virtual-machines-windows-hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Auftragsverwaltung in HPC Pack](https://technet.microsoft.com/library/jj899585.aspx)
* [Verwalten eines HPC Pack-Clusters in Azure mit Azure Active Directory](virtual-machines-windows-hpcpack-cluster-active-directory.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

## <a name="add-worker-role-nodes-to-an-hpc-pack-cluster"></a>Hinzufügen von Workerrolleknoten zu einem HPC Pack-Cluster
* [Burst to Azure with Microsoft HPC Pack (in englischer Sprache)](https://technet.microsoft.com/library/gg481749.aspx)
* [Tutorial: Einrichten eines Hybridclusters mit HPC Pack in Azure](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md)
* [Hinzufügen von Azure-"Burst"-Knoten zu einem HPC Pack-Hauptknoten in Azure](virtual-machines-windows-classic-hpcpack-cluster-node-burst.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

## <a name="integrate-with-azure-batch"></a>Integration in Azure Batch
* [Burst to Azure Batch with Microsoft HPC Pack (in englischer Sprache)](https://technet.microsoft.com/library/mt612877.aspx)

## <a name="create-rdma-clusters-for-mpi-workloads"></a>Erstellen von RDMA-Clustern für MPI-Workloads
* [Einrichten eines Windows RDMA-Clusters mit HPC Pack zum Ausführen von MPI-Anwendungen](virtual-machines-windows-classic-hpcpack-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)


