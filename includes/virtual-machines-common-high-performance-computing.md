Organisationen haben hohen Computing-Bedarf. Zu diesen Big Compute-Workloads zählen unter anderem technische Entwicklungen und Analysen, die Berechnung finanzieller Risiken, das Rendern von Bildern, komplexe Modelle sowie Monte Carlo-Simulationen. 

Mit Azure können Sie HPC-Workloads (High Performance Computing) und Batch-Workloads in Clustern mit virtuellen Azure-Computern ausführen und dabei Ihre bevorzugten Azure-Infrastrukturdienste oder verwalteten Dienste nutzen. Azure ermöglicht flexibles Hochskalieren von Computeressourcen auf tausende von virtuellen Computern oder Kernen sowie anschließendes Herunterskalieren, wenn Sie wieder weniger Ressourcen benötigen. 

Azure-HPC-Lösungen führen effizient rechenintensive Linux- und Windows-Workloads aus – von parallelen Batchaufträgen bis hin zu herkömmlichen HPC-Simulationen. Führende HPC-Anwendungen unterstützen die Ausführung auf virtuellen Azure-Computern, und Sie können von speziellen VM-Größen und Images für rechenintensive Aufträge profitieren. 


## <a name="solution-architectures"></a>Lösungsarchitekturen

Im Anschluss finden Sie einige Beispiele für rechenintensive Lösungsarchitekturen in Azure:

* Führen Sie HPC-Anwendungen auf virtuellen Computern oder in [VM-Skalierungsgruppen](../articles/virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) unter Verwendung eines Linux- oder Windows-Clusters oder eines Raster-Managers Ihrer Wahl aus. [Weitere Informationen](https://azure.microsoft.com/en-us/solutions/architecture/hpc-cluster/)
* Erstellen Sie Hybridlösungen, die einen lokalen HPC-Cluster erweitern, um Workload-Spitzen auf virtuelle Azure-Computer auszulagern (Burst). [Weitere Informationen](https://azure.microsoft.com/en-us/solutions/architecture/hpc-on-prem-burst/)
* Verwenden Sie den verwalteten und skalierbaren Azure [Batch](https://azure.microsoft.com/services/batch/)-Dienst, um rechenintensive Workloads ohne Verwaltung der zugrunde liegenden Infrastruktur auszuführen. [Weitere Informationen](https://azure.microsoft.com/en-us/solutions/architecture/hpc-big-compute-saas/)

## <a name="hpc-cluster-and-grid-solutions"></a>HPC-Cluster und Raster-Lösungen
Sie können vertraute HPC-Workloadverwaltungstools für virtuelle Azure-Computer bereitstellen oder erweitern und Ihre rechenintensiven Workloads ausführen. Dies gilt unter anderem für Open-Source-Tools wie den [Slurm-Workload-Manager](https://slurm.schedmd.com/) sowie für Cluster- oder Raster-Manager von Microsoft und anderen Herausgebern.

### <a name="linux-and-oss-cluster-solutions"></a>Linux- und OSS-Clusterlösungen
Verwenden Sie Azure Resource Manager-Vorlagen, um Linux-HPC-Clusterlösungen auf virtuellen Computern oder in VM-Skalierungsgruppen bereitzustellen. Beispiele:

* [Slurm](https://azure.microsoft.com/documentation/templates/slurm/)
* [Torque](https://azure.microsoft.com/documentation/templates/torque-cluster/)
* [PBS Professional](https://github.com/xpillons/azure-hpc/tree/master/Compute-Grid-Infra)

Sehen Sie sich auch die Sammlung mit [Fünf-Klick-Vorlagen](https://github.com/tanewill/5clickTemplates) an.

### <a name="grid-managers"></a>Raster-Manager
Microsoft arbeitet mit kommerziellen Raster-Managern zusammen, um deren Lösungen auf virtuellen Azure-Computern verfügbar zu machen. Beispiele:

* [IBM Spectrum Symphony und Symphony LSF](https://azure.microsoft.com/blog/ibm-and-microsoft-azure-support-spectrum-symphony-and-spectrum-lsf/)
* [TIBCO DataSynapse GridServer](https://azure.microsoft.com/blog/tibco-datasynapse-comes-to-the-azure-marketplace/) 
* [Bright Cluster Manager](http://www.brightcomputing.com/technology-partners/microsoft)
* [Univa Grid Engine](http://www.univa.com/products/grid-engine)

### <a name="microsoft-hpc-pack"></a>Microsoft HPC Pack
[HPC Pack](https://technet.microsoft.com/library/cc514029(v=ws.11).aspx) ist die kostenlose HPC-Lösung von Microsoft, die auf Microsoft Azure- und Windows Server-Technologien basiert. Sie bietet mehrere Optionen für die Ausführung auf virtuellen Computern unter [Windows](../articles/virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) und [Linux](../articles/virtual-machines/linux/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

In diesem Artikel werden folgende Themen erläutert:

* [Erstellen eines HPC Pack 2016-Clusters in Azure](../articles/virtual-machines/windows/hpcpack-2016-cluster.md)
* [Burst in Azure mit HPC Pack](https://technet.microsoft.com/library/gg481749(v=ws.11).aspx)




## <a name="azure-batch"></a>Azure Batch
Bei [Batch](../articles/batch/batch-technical-overview.md) handelt es sich um eine Plattform zum Ausführen umfangreicher paralleler Anwendungen und HPC-Anwendungen (High Performance Computing) in der Cloud. Azure Batch plant die Ausführung rechenintensiver Aufgaben mit einem verwalteten Pool virtueller Computer und kann Computeressourcen automatisch skalieren, um den Anforderungen Ihrer Aufträge gerecht zu werden. 

Verwenden Sie die Batch-SDKs und -Tools, um HPC-Anwendungen oder Containerworkloads in Azure zu integrieren, Daten in Azure bereitzustellen und Pipelines für die Auftragsausführung zu erstellen. Darüber hinaus können Sie überschüssige Azure VM-Kapazität (mit niedriger Priorität) zu einem [günstigeren Preis](https://azure.microsoft.com/pricing/details/batch/) nutzen und so die Kosten für die Ausführung einiger Workloads in Batch erheblich senken.

In diesem Artikel werden folgende Themen erläutert:

* [Erste Schritte beim Entwickeln mit Batch](../articles/batch/batch-dotnet-get-started.md)
* [Verwenden von Codebeispielen für Azure Batch](https://github.com/Azure/azure-batch-samples)
* [Verwenden von virtuellen Computern mit niedriger Priorität mit Batch (Vorschauversion)](../articles/batch/batch-low-pri-vms.md)
* [Ausführen von HPC-Workloads in Containern mit Batch Shipyard](https://github.com/Azure/batch-shipyard)
* [Verwenden der Sprache R mit Batch](https://github.com/Azure/doAzureParallel)

## <a name="hpc-and-gpu-vm-sizes"></a>VM-Größen mit HPC und GPU
Azure bietet eine Reihe von Größen für virtuelle Computer unter [Linux](../articles/virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) und [Windows](../articles/virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), darunter auch Größen, die für rechenintensive Workloads konzipiert sind. Virtuelle Computer vom Typ „H16r“ und „H16mr“ können beispielsweise eine Verbindung mit einem Back-End-RDMA-Netzwerk mit hohem Durchsatz herstellen. Dieses Cloudnetzwerk kann die Leistung eng gekoppelter paralleler Anwendungen unter [Microsoft MPI](https://msdn.microsoft.com/library/bb524831.aspx) oder Intel MPI verbessern. Virtuelle Computer der N-Serie verfügen über NVIDIA-GPUs für rechen- oder grafikintensive Anwendungen wie KI-Lernen und Visualisierung. 

Weitere Informationen:

* HPC-Größen für virtuelle Computer unter [Linux](../articles/virtual-machines/linux/sizes-hpc.md) und [Windows](../articles/virtual-machines/windows/sizes-hpc.md) 
* Größen für virtuelle Computer mit GPU unter [Linux](../articles/virtual-machines/linux/sizes-gpu.md) und [Windows](../articles/virtual-machines/windows/sizes-gpu.md) 

In diesem Artikel werden folgende Themen erläutert:

* [Einrichten eines Linux RDMA-Clusters zum Ausführen von MPI-Anwendungen](../articles/virtual-machines/linux/classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Einrichten eines Windows RDMA-Clusters mit Microsoft HPC Pack zum Ausführen von MPI-Anwendungen](../articles/virtual-machines/windows/classic/hpcpack-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [Verwenden rechenintensiver virtueller Computer in Batch-Pools](../articles/batch/batch-pool-compute-intensive-sizes.md)

## <a name="hpc-images"></a>HPC-Images

Images für HPC-fähige virtuelle Linux- und Windows-Computer finden Sie im [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/). Beispiele:

* [CentOS-basiertes HPC von RogueWave](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/RogueWave.CentOSbased73HPC?tab=Overview)
* [SUSE Linux Enterprise Server für HPC](https://azure.microsoft.com/marketplace/partners/suse/suselinuxenterpriseserver12optimizedforhighperformancecompute/)
* [TIBCO Grid Server 6.2.0 Engine](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/tibco-software.gridserverlinuxengine?tab=Overview)
* [Virtueller Computer für Data Science für Linux und Windows](../articles/machine-learning/machine-learning-data-science-virtual-machine-overview.md)


 
## <a name="hpc-applications"></a>HPC-Anwendungen

Führen Sie benutzerdefinierte oder kommerzielle HPC-Anwendungen in Azure aus. Für mehrere werden Benchmarks erstellt, um eine effiziente Skalierung mit zusätzlichen virtuellen Computern oder Computekernen zu ermöglichen. Die folgenden Abschnitte sind Beispiele.

> [!IMPORTANT]
> Informieren Sie sich bei Verwendung kommerzieller Anwendungen beim jeweiligen Hersteller über lizenzbedingte oder anderweitige Einschränkungen für die Ausführung in der Cloud. Nicht alle Hersteller bieten ein nutzungsbasiertes Lizenzmodell an. Unter Umständen benötigen Sie für Ihre Lösung einen Lizenzserver in der Cloud oder eine Verbindung mit einem lokalen Lizenzserver.

### <a name="engineering-applications"></a>Technische Anwendungen


* [Altair RADIOSS](https://azure.microsoft.com/blog/availability-of-altair-radioss-rdma-on-microsoft-azure/)
* [ANSYS CFD](https://azure.microsoft.com/blog/ansys-cfd-and-microsoft-azure-perform-the-best-hpc-scalability-in-the-cloud/)
* [MATLAB Distributed Computing Server](../articles/virtual-machines/windows/matlab-mdcs-cluster.md)
* [StarCCM+](https://blogs.msdn.microsoft.com/azurecat/2017/07/07/run-star-ccm-in-an-azure-hpc-cluster/)
* [OpenFOAM](https://simulation.azure.com/casestudies/Team-182-ABB-UC-Final.pdf)



### <a name="graphics-and-rendering"></a>Grafik und Rendering

* [Autodesk Maya, 3ds Max und Arnold](../articles/batch/batch-rendering-service.md) unter Azure Batch (Vorschauversion)

### <a name="ai-and-deep-learning"></a>KI und Deep Learning

* [Microsoft Cognitive Toolkit](https://docs.microsoft.com/cognitive-toolkit/cntk-on-azure)
* [Deep Learning-Toolkit für virtuelle Data Science-Computer](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.dsvm-deep-learning)
* [Batch Shipyard-Rezepte für Deep Learning](https://github.com/Azure/batch-shipyard/tree/master/recipes#deeplearning)

## <a name="solution-partners"></a>Lösungspartner

Beispiele für Partner, die Batch- und HPC-Anwendungen in Azure entwickeln oder bereitstellen:

* [Cycle Computing](https://cyclecomputing.com/) (inzwischen [mit Microsoft zusammengeschlossen](https://blogs.microsoft.com/blog/2017/08/15/microsoft-acquires-cycle-computing-accelerate-big-computing-cloud/))
* [Rescale](https://www.rescale.com/azure/)
* [UberCloud](https://www.theubercloud.com/)

## <a name="hpc-storage"></a>HPC-Speicher

Herkömmliche Clouddateisysteme sind den Anforderungen, die umfangreiche Batch- und HPC-Workloads in puncto Datenspeicherung und -zugriff haben, nicht gewachsen. Sie können in Azure parallele Dateisystemlösungen wie etwa [Lustre](http://lustre.org/) und [BeeGFS](http://www.beegfs.com/content/) implementieren.

Weitere Informationen:

* [Parallel file systems for HPC storage on Azure](https://blogs.msdn.microsoft.com/azurecat/2017/03/17/parallel-file-systems-for-hpc-storage-on-azure/) (Parallele Dateisysteme für HPC-Speicher in Azure)


## <a name="related-azure-services"></a>Verwandte Azure-Dienste

Die meisten Azure-HPC-Lösungen basieren auf Azure Virtual Machines, VM-Skalierungsgruppen, Batch und ähnlichen Computediensten. Ihre Lösung kann jedoch von zahlreichen verwandten Azure-Diensten profitieren. Im Anschluss finden Sie eine unvollständige Liste:

### <a name="storage"></a>Speicher

* [Blob, Table und Queue Storage](../articles/storage/storage-introduction.md)
* [File Storage](../articles/storage/storage-files-introduction.md)

### <a name="data-and-analytics"></a>Daten und Analysen
* [HDInsight](../articles/hdinsight/hdinsight-hadoop-introduction.md) für Hadoop-Cluster in Azure
* [Data Factory](../articles/data-factory/data-factory-introduction.md)
* [Data Lake Store](../articles/data-lake-store/data-lake-store-overview.md)
* [Machine Learning](../articles/machine-learning/machine-learning-what-is-machine-learning.md)
* [SQL-Datenbank](../articles/sql-database/sql-database-technical-overview.md)

### <a name="networking"></a>Netzwerk
* [Virtual Network](../articles/virtual-network/virtual-networks-overview.md)
* [ExpressRoute](../articles/expressroute/expressroute-introduction.md)

### <a name="containers"></a>Container
* [Container Service](../articles/container-service/dcos-swarm/container-service-intro.md)
* [Containerregistrierung](../articles/container-registry/container-registry-intro.md)



## <a name="customer-stories"></a>Kundenstimmen

Im Anschluss finden Sie Beispiele für Kunden, die mithilfe von Azure-HPC-Lösungen geschäftliche Probleme gelöst haben:

* [ANEO](https://customers.microsoft.com/story/it-provider-finds-highly-scalable-cloud-based-hpc-redu) 
* [AXA Global P&C](https://customers.microsoft.com/story/axa-global-p-and-c)
* [Axioma](https://customers.microsoft.com/story/axioma-delivers-fintechs-first-born-in-the-cloud-multi-asset-class-enterprise-risk-solution)
* [d3View](https://customers.microsoft.com/story/big-data-solution-provider-adopts-new-cloud-gains-thou)
* [Hymans Robertson](https://customers.microsoft.com/story/hymans-robertson)
* [MetLife](https://enterprise.microsoft.com/en-us/customer-story/industries/insurance/metlife/)
* [Microsoft Research](https://customers.microsoft.com/doclink/fast-lmm-and-windows-azure-put-genetics-research-on-fa)
* [Milliman](https://customers.microsoft.com/story/actuarial-firm-works-to-transform-insurance-industry-w)
* [Mitsubishi UFJ Securities International](https://customers.microsoft.com/story/powering-risk-compute-grids-in-the-cloud)
* [Schlumberger](http://azure.microsoft.com/blog/big-compute-for-large-engineering-simulations)
* [Towers Watson](https://customers.microsoft.com/story/insurance-tech-provider-delivers-disruptive-solutions)


## <a name="next-steps"></a>Nächste Schritte
* Informieren Sie sich ausführlicher über Big Compute-Lösungen für [technische Simulationen](https://simulation.azure.com/), [Rendering](https://simulation.azure.com/) und [Banken und Kapitalmärkte](https://finance.azure.com/).
* Die neuesten Ankündigungen finden Sie im [Blog des HPC- und Batch-Teams von Microsoft](http://blogs.technet.com/b/windowshpc/) und im [Azure-Blog](https://azure.microsoft.com/blog/tag/hpc/).


