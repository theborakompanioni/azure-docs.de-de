<properties
   pageTitle="Ressourcen für Batch- und HPC-Workloads in der Cloud | Microsoft Azure"
   description="Hier werden die technischen Ressourcen vorgestellt, die Ihnen beim Ausführen Ihrer umfangreichen parallelen Workloads sowie Ihrer Batch- und HPC (High Performance Computing)-Workloads in Azure helfen."
   services="batch, cloud-services, virtual-machines"
   documentationCenter=""
   authors="dlepow"
   manager="timlt"
   editor=""/>

<tags
   ms.service="multiple"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="big-compute"
   ms.date="09/22/2016"
   ms.author="danlep"/>

# Big Compute in Azure: technische Ressourcen für Batch und High Performance Computing 
In diesem Leitfaden werden die technischen Ressourcen vorgestellt, die Sie beim Ausführen Ihrer umfangreichen parallelen Workloads sowie Ihrer Batch- und HPC-Workloads (High Performance Computing) in Azure unterstützen. Erweitern Sie Ihre vorhandenen Batch- oder HPC-Workloads in die Azure-Cloud, oder erstellen Sie mithilfe einer Reihe von Azure-Diensten neue Big Compute-Lösungen.

## Lösungsoptionen

Erfahren Sie mehr über Big Compute-Optionen in Azure, und wählen Sie den richtigen Ansatz für Ihren Workload und Ihre Geschäftsanforderungen aus.

* [Batch- und HPC-Lösungen](batch-hpc-solutions.md)

* [Video: Big Compute in der Cloud mit Azure und HPC ](https://azure.microsoft.com/documentation/videos/teched-europe-2014-big-compute-in-the-cloud-with-high-performance-computing-on-azure/)


## Azure Batch

[Batch](https://azure.microsoft.com/services/batch/) ist ein Plattformdienst, mit dem Sie auf einfache Weise Ihre Linux- und Windows-Anwendungen für die Cloud aktivieren und Aufträge ausführen können, ohne einen Cluster und einen Auftragsplaner einrichten und verwalten zu müssen. Verwenden Sie das SDK, um Clientanwendungen mithilfe verschiedener Sprachen in Azure Batch zu integrieren, Daten in Azure bereitzustellen und Auftragsausführungspipelines zu erstellen.

* [Dokumentation](https://azure.microsoft.com/documentation/services/batch/)

* Referenz zu [.NET](https://msdn.microsoft.com/library/azure/mt348682.aspx), [Python](http://azure-sdk-for-python.readthedocs.io/latest/), [Node.js](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/), [Java](http://azure.github.io/azure-sdk-for-java/) und [REST](https://msdn.microsoft.com/library/azure/dn820158.aspx)-API

* Referenz zur [Batch Management .NET-Bibliothek](https://msdn.microsoft.com/library/mt463120.aspx)

* Tutorials: Erste Schritte mit der [Azure Batch-Bibliothek für .NET](batch-dotnet-get-started.md) und dem [Batch Python-Client](batch-python-tutorial.md)

* [Batch-Forum](https://social.msdn.microsoft.com/Forums/de-DE/home?forum=azurebatch)

* [Batch-Videos](https://azure.microsoft.com/documentation/videos/index/?services=batch)

## HPC-Clusterlösungen

Sie können Ihren vorhandenen Windows- oder Linux-HPC-Cluster zum Ausführen rechenintensiver Workloads in Azure bereitstellen oder erweitern.

### Microsoft HPC Pack

HPC Pack ist die kostenlose HPC-Lösung von Microsoft, die auf Microsoft Azure und Windows Server-Technologien basiert und in der Lage ist, Windows- und Linux HPC-Workloads auszuführen.

* [HPC Pack 2012 R2 Update 3 herunterladen](https://www.microsoft.com/download/details.aspx?id=49922)

* [Dokumentation](https://technet.microsoft.com/library/jj899572.aspx)


* HPC Pack-Clusteroptionen für [Linux](../virtual-machines/virtual-machines-linux-hpcpack-cluster-options.md) und [Windows](../virtual-machines/virtual-machines-windows-hpcpack-cluster-options.md) in Azure

* [Burst to Azure with Microsoft HPC Pack (in englischer Sprache)](https://technet.microsoft.com/library/gg481749.aspx)

* [Burst to Azure Batch with Microsoft HPC Pack (in englischer Sprache)](https://technet.microsoft.com/library/mt612877.aspx)


* [Windows-HPC-Foren](https://social.microsoft.com/Forums/home?category=windowshpc)

### Linux- und OSS-Clusterlösungen

Verwenden Sie die folgenden Azure-Vorlagen zum Bereitstellen von Linux-HPC-Clustern.

* [Spin up a SLURM cluster](https://azure.microsoft.com/documentation/templates/slurm/) und [Blogbeitrag](http://blogs.technet.com/b/windowshpc/archive/2015/06/06/deploy-a-slurm-cluster-on-azure.aspx)

* [Spin up a Torque cluster](https://azure.microsoft.com/documentation/templates/torque-cluster/)

* [Intel Cloud Edition for Lustre Software – Eval](https://azure.microsoft.com/marketplace/partners/intel/lustre-cloud-edition-evaleval-lustre-2-7/)

## Microsoft MPI

[Microsoft MPI](https://msdn.microsoft.com/library/bb524831.aspx) (MS-MPI) ist eine Microsoft-Implementierung des Message Passing Interface-Standards für die Entwicklung und Ausführung paralleler Anwendungen auf der Windows-Plattform. Die neueste Version ist MS-MPI v7.


* [MS-MPI herunterladen](http://go.microsoft.com/FWLink/p/?LinkID=389556)

* [MS-MPI-Referenz](https://msdn.microsoft.com/library/dn473458.aspx)

* [MPI-Forum](https://social.microsoft.com/Forums/de-DE/home?forum=windowshpcmpi)

## Rechenintensive Instanzen

Azure bietet zum Ausführen Ihrer Linux- und Windows-HPC-Workloads [verschiedene Größen für virtuelle Computer](../virtual-machines/virtual-machines-windows-sizes.md) an, einschließlich [rechenintensiver](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md) Instanzen, die mit einem Back-End-RDMA-Netzwerk verbunden werden können.


* [Einrichten eines Linux RDMA-Clusters zum Ausführen von MPI-Anwendungen](../virtual-machines/virtual-machines-linux-classic-rdma-cluster.md)

* [Einrichten eines Windows RDMA-Clusters mit Microsoft HPC Pack zum Ausführen von MPI-Anwendungen](../virtual-machines/virtual-machines-windows-classic-hpcpack-rdma-cluster.md)



## Beispiele und Demos

* [Codebeispiele für Azure Batch mit C# und Python](https://github.com/Azure/azure-batch-samples)

* [Batch Shipyard](https://azure.github.io/batch-shipyard/)-Toolkit zur einfachen Bereitstellung von mit Docker erstellten Workloads in Batchform

* [Testversion von SUSE Linux Enterprise Server für HPC](https://azure.microsoft.com/marketplace/partners/suse/suselinuxenterpriseserver12optimizedforhighperformancecompute/)

## Verwandte Azure-Dienste

* [Data Factory](https://azure.microsoft.com/documentation/services/data-factory/)

* [Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/)

* [HDInsight](https://azure.microsoft.com/documentation/services/hdinsight/)

* [Virtuelle Computer](https://azure.microsoft.com/documentation/services/virtual-machines/)

* [Skalierungsgruppen für virtuelle Computer](https://azure.microsoft.com/documentation/services/virtual-machine-scale-sets/)

* [Cloud Services](https://azure.microsoft.com/documentation/services/cloud-services/)

* [App Service](https://azure.microsoft.com/documentation/services/app-service/)

* [Media Services](https://azure.microsoft.com/documentation/services/media-services/)

* [Funktionen](https://azure.microsoft.com/documentation/services/functions/)

## Architekturpläne

* [HPC and data orchestration using Azure Batch and Azure Data Factory](http://go.microsoft.com/fwlink/?linkid=717686) (HPC und Datenorchestrierung mit Azure Batch und Azure Data Factory, PDF) und [Artikel](../data-factory/data-factory-data-processing-using-batch.md)

## Branchenlösungen

* [Bankwesen und Kapitalmärkte](https://finance.azure.com/)

* [Simulationen für den Maschinenbau](https://simulation.azure.com/)

## Kundenstimmen

* [ANEO](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=4168)

* [d3View](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=22088)

* [Ludwig Institute of Cancer Research](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=5830)

* [Microsoft Research](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=15634)

* [Milliman](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=14967)

* [Mitsubishi UFJ Securities International](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=26266)

* [Schlumberger](http://azure.microsoft.com/blog/big-compute-for-large-engineering-simulations)

* [Towers Watson](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18222)

* [UberCloud](https://simulation.azure.com/casestudies/Team-182-ABB-UC-Final.pdf)



## Nächste Schritte

* Die neuesten Ankündigungen finden Sie im [Blog des HPC- und Batch-Teams von Microsoft](http://blogs.technet.com/b/windowshpc/) und im [Azure-Blog](https://azure.microsoft.com/blog/tag/hpc/).
* Weitere Informationen finden Sie auch unter [Neuigkeiten bei Batch](https://azure.microsoft.com/updates/?service=batch) oder indem Sie den [RSS-Feed](https://azure.microsoft.com/updates/feed/?service=batch) abonnieren.

<!---HONumber=AcomDC_0928_2016-->