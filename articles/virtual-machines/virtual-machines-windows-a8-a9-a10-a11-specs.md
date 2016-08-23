<properties
 pageTitle="Informationen zu A8-, A9-, A10- und A11-Instanzen mit Windows| Microsoft Azure"
 description="Hier finden Sie Hintergrundinformationen und Überlegungen zur Verwendung der rechenintensiven Azure-Instanzgrößen A8, A9, A10 und A11 für virtuelle Windows-Computer und Clouddienste."
 services="virtual-machines-windows, cloud-services"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="infrastructure-services"
 ms.date="08/04/2016"
 ms.author="danlep"/>

# Informationen zu den rechenintensiven A8-, A9-, A10- und A11-Instanzen

Hier finden Sie Hintergrundinformationen und einige Überlegungen zur Verwendung der Azure-Instanzen A8 und A9, A10 und A11, auch bekannt als *rechenintensive* Instanzen. Dieser Artikel konzentriert sich auf das Verwenden dieser Instanzen für virtuelle Windows-Computer. Dieser Artikel ist auch für [virtuelle Linux-Computer](virtual-machines-linux-a8-a9-a10-a11-specs.md) verfügbar.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

[AZURE.INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## Zugreifen auf das RDMA-Netzwerk

In einem einzelnen Clouddienst, einer einzelnen Verfügbarkeitsgruppe oder einem einzelnen Azure-Batchpool können die A8- und A9-Instanzen auf das RDMA-Netzwerk in Azure zugreifen, um Windows-MPI-Anwendungen auszuführen, die die Microsoft Network Direct-Schnittstelle zur Kommunikation zwischen Instanzen verwenden.

Nachfolgend finden Sie weitere Informationen zu den Voraussetzungen für MPI-Anwendungen zum Zugreifen auf das RDMA-Netzwerk auf virtuellen Windows-Computern, in Clouddiensten und in Azure-Batchpools der Instanzen A8 oder A9. Typische Bereitstellungsszenarios finden Sie unter [Einrichten eines Windows-RDMA-Clusters mit HPC Pack zum Ausführen von MPI-Anwendungen](virtual-machines-windows-classic-hpcpack-rdma-cluster.md) und [Verwendung von Tasks mit mehreren Instanzen zum Ausführen von MPI-Anwendungen (Message Passing Interface) in Azure Batch](../batch/batch-mpi.md).


Voraussetzung | Virtuelle Computer | Clouddienste oder Batchpool 
---------- | ------------ | ------------- 
Betriebssystem | Windows Server 2012 R2 oder Windows Server 2012 | Gastbetriebssystemfamilie Windows Server 2012 R2, Windows Server 2012 oder Windows Server 2008 R2 
MPI | MS-MPI 2012 R2 oder höher oder Intel MPI Library 5 | MS-MPI 2012 R2 oder höher oder Intel MPI Library 5 


>[AZURE.NOTE]Auf virtuellen A8- und A9-Computern muss den virtuellen Computern die HpcVmDrivers-Erweiterung hinzugefügt werden, sodass die für RDMA-Verbindungen benötigten Windows-Netzwerkgerätetreiber installiert werden. Je nach Bereitstellungsmethode wird die HpcVmDriversDrivers-Erweiterung zu einem virtuellen Computer der Größe A8 oder A9 automatisch hinzugefügt, oder Sie fügen sie selbst hinzu. Informationen zum eigenhändigen Hinzufügen der Erweiterung finden Sie unter [Verwalten von Erweiterungen für virtuelle Computer](virtual-machines-windows-classic-manage-extensions.md).

## Überlegungen zu HPC Pack und Windows

HPC Pack ist zwar für die Verwendung der A8-, A9-, A10- und A11-Instanzen mit Windows Server nicht erforderlich, kann jedoch als Tool zum Ausführen Windows-basierter MPI-Anwendungen nützlich sein, die auf das RDMA-Netzwerk in Azure zugreifen. HPC Pack 2012 R2 und höhere Versionen enthalten eine Laufzeitumgebung für die Microsoft-Implementierung des Message Passing Interface (MS MPI) für Windows, die das Azure RDMA-Netzwerk bei Bereitstellung auf A8 und A9-Instanzen verwenden können.

Weitere Informationen und Prüflisten zur Verwendung rechenintensiver Instanzen mit HPC Pack auf Windows Server finden Sie unter [Einrichten eines Windows RDMA-Clusters mit HPC Pack zum Ausführen von MPI-Anwendungen](virtual-machines-windows-classic-hpcpack-rdma-cluster.md).




## Nächste Schritte

* Ausführliche Informationen zur Verfügbarkeit und Preisgestaltung der Instanzen A8 und A9, A10 und A11 finden Sie unter [Virtuelle Computer Preise](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows) sowie unter [Cloud Services Preise](https://azure.microsoft.com/pricing/details/cloud-services/).

* Informationen zu Speicherkapazitäten und Details zu den Datenträgern finden Sie unter [Größen für virtuelle Computer](virtual-machines-linux-sizes.md).

* Informationen zu ersten Schritten für die Bereitstellung und Verwendung von A8- und A9-Instanzen mit HPC Pack unter Windows finden Sie unter [Einrichten eines Windows RDMA-Clusters mit HPC Pack zum Ausführen von MPI-Anwendungen](virtual-machines-windows-classic-hpcpack-rdma-cluster.md).

* Informationen zur Verwendung von A8- und A9-Instanzen zum Ausführen von MPI-Anwendungen mit Azure Batch finden Sie unter [Verwendung von Tasks mit mehreren Instanzen zum Ausführen von MPI-Anwendungen (Message Passing Interface) in Azure Batch](../batch/batch-mpi.md).

<!---HONumber=AcomDC_0810_2016-->