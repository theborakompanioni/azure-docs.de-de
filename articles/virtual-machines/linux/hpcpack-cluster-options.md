---
title: Linux-HPC Pack-Clusteroptionen in der Cloud | Microsoft Docs
description: Informationen zu Optionen mit Microsoft HPC Pack, um einen Linux-HPC-Cluster (High Performance Computing) in der Azure-Cloud zu erstellen und zu verwalten
services: virtual-machines-linux,cloud-services
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management,hpc-pack
ms.assetid: ac60624e-aefa-40c3-8bc1-ef6d5c0ef1a2
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: big-compute
ms.date: 02/06/2017
ms.author: danlep
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 616006d29149f7f47b03bd127cf3c83ad63a5c39
ms.lasthandoff: 04/03/2017


---
# <a name="options-with-hpc-pack-to-create-and-manage-an-hpc-cluster-in-azure-for-linux-workloads"></a>Optionen zum Erstellen und Verwalten eines HPC-Clusters (High Performance Computing) mit HPC Pack in Azure für Linux-Workloads
[!INCLUDE [virtual-machines-common-hpcpack-cluster-options](../../../includes/virtual-machines-common-hpcpack-cluster-options.md)]

Dieser Artikel konzentriert sich auf Optionen für die Verwendung von HPC Pack zum Ausführen von Linux-Workloads. Es gibt auch Optionen zum Ausführen von [Windows-HPC-Workloads mit HPC Pack](../windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="run-an-hpc-pack-cluster-in-azure-vms"></a>Ausführen eines HPC Pack-Clusters in virtuellen Azure-Computern
### <a name="azure-templates"></a>Azure-Vorlagen
* (GitHub) [HPC Pack 2016 cluster templates](https://github.com/MsHpcPack/HPCPack2016)
* (Marketplace) [HPC Pack cluster for Linux workloads](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/)
* (Schnellstart) [Create an HPC cluster with Linux compute nodes](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster-linux-cn)

### <a name="powershell-deployment-script"></a>PowerShell-Bereitstellungsskript
* [Erstellen eines Linux-HPC-Clusters mit dem HPC Pack-IaaS-Bereitstellungsskript](../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

### <a name="tutorials"></a>Tutorials
* [Tutorial: Erste Schritte mit Linux-Computeknoten in einem HPC Pack-Cluster in Azure](classic/hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Tutorial: Ausführen von NAMD mit Microsoft HPC Pack auf Linux-Computeknoten in Azure](classic/hpcpack-cluster-namd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Tutorial: Ausführen von OpenFOAM mit Microsoft HPC Pack auf einem Linux-RDMA-Cluster in Azure](classic/hpcpack-cluster-openfoam.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Tutorial: Ausführen von STAR-CCM+ mit Microsoft HPC Pack auf einem Linux-RDMA-Cluster in Azure](classic/hpcpack-cluster-starccm.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

### <a name="cluster-management"></a>Clusterverwaltung
* [Übermitteln von Aufträgen an einen HPC Pack-Cluster in Azure](../windows/hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Auftragsverwaltung in HPC Pack](https://technet.microsoft.com/library/jj899585.aspx)

## <a name="create-rdma-clusters-for-mpi-workloads"></a>Erstellen von RDMA-Clustern für MPI-Workloads
* [Tutorial: Ausführen von OpenFOAM mit Microsoft HPC Pack auf einem Linux-RDMA-Cluster in Azure](classic/hpcpack-cluster-openfoam.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Einrichten eines Linux RDMA-Clusters zum Ausführen von MPI-Anwendungen](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)


