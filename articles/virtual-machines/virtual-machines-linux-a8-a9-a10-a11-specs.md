<properties
 pageTitle="Informationen zu A8-, A9-, A10- und A11-Instanzen und Linux | Microsoft Azure"
 description="Hier finden Sie Hintergrundinformationen und Überlegungen zur Verwendung der rechenintensiven Azure-Instanzgrößen A8, A9, A10 und A11 für virtuelle Linux-Computer."
 services="virtual-machines-linux"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management"/>
<tags
ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="infrastructure-services"
 ms.date="08/04/2016"
 ms.author="danlep"/>

# Informationen zu den rechenintensiven A8-, A9-, A10- und A11-Instanzen 

Hier finden Sie Hintergrundinformationen und einige Überlegungen zur Verwendung der Azure-Instanzen A8 und A9, A10 und A11, auch bekannt als *rechenintensive* Instanzen. Dieser Artikel konzentriert sich auf das Verwenden dieser Instanzen für virtuelle Linux-Computer. Dieser Artikel ist auch für [virtuelle Windows-Computer](virtual-machines-windows-a8-a9-a10-a11-specs.md) verfügbar.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

[AZURE.INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## Zugreifen auf das RDMA-Netzwerk

Innerhalb eines einzelnen Clouddiensts oder einer Verfügbarkeitsgruppe können Cluster von Linux-VMs der Größe A8 und A9, auf denen eine der folgenden unterstützten Linux-HPC-Distributionen und eine unterstützte MPI-Implementierung ausgeführt werden, auf das RDMA-Netzwerk in Azure zum Ausführen von Linux-MPI-Anwendungen zugreifen. Bereitstellungsoptionen und Beispielkonfigurationsschritte finden Sie unter [Einrichten eines Linux RDMA-Clusters zum Ausführen von MPI-Anwendungen](virtual-machines-linux-classic-rdma-cluster.md).

* **Verteilungen** – SUSE Linux Enterprise Server (SLES) 12 für HPC, SLES 12 für HPC (Premium), 7.1 HPC CentOS-basiert oder 6.5 HPC CentOS-basiert, aus einem Azure Marketplace-Image bereitgestellt

* **MPI** – Intel MPI Library 5.x

    >[AZURE.NOTE] Intel MPI 5.1 ist bereits auf den CentOS-basierten HPC-Images im Marketplace installiert. Um Intel MPI auf SLES 12 HPC-VMs zu verwenden, müssen Sie es separat installieren.

Derzeit werden Azure Linux RDMA-Treiber nur installiert, wenn Sie RDMA-fähiges SLES 12 HPC und CentOS-HPC-Images aus dem Azure Marketplace bereitstellen. Sie können die Treiber nicht auf anderen Linux-VMs installieren, die Sie bereitstellen.

>[AZURE.NOTE]Für die CentOS-basierten HPC-Images aus dem Marketplace sind Kernel-Updates in der **yum**-Konfigurationsdatei deaktiviert. Der Grund: Die Linux RDMA-Treiber werden als RPM-Paket verteilt, und Treiberupdates funktionieren möglicherweise nicht, wenn der Kernel aktualisiert wird.


## RDMA-Treiberupdates für SLES 12
Nachdem Sie eine VM basierend auf einem SLES 12 HPC-Image erstellt haben, müssen Sie die RDMA-Treiber auf den virtuellen Computern für die RDMA-Netzwerkverbindung aktualisieren.

>[AZURE.IMPORTANT]Dieser Schritt ist für SLES 12 HPC-VM-Bereitstellungen in allen Azure-Regionen **erforderlich**. Dieser Schritt ist nicht erforderlich, wenn Sie eine CentOS-basierte 7.1 HPC- oder 6.5 HPC-VM bereitgestellt haben.

Beenden Sie alle **Zypper**-Prozesse oder alle Prozesse, die die SUSE-Repository-Datenbanken auf dem virtuellen Computer sperren, bevor Sie die Treiber aktualisieren. Andernfalls werden die Treiber möglicherweise nicht korrekt aktualisiert.

Um die Linux RDMA-Treiber auf jedem virtuellen Computer zu aktualisieren, führen Sie eine der folgenden Grupopen von Azure-CLI-Befehle auf Ihrem Clientcomputer aus.

**Für eine im klassischen Bereitstellungsmodell bereitgestellte SLES 12 HPC-VM**

```
azure config mode asm

azure vm extension set <vm-name> RDMAUpdateForLinux Microsoft.OSTCExtensions 0.1
```

**Für eine im Resource Manager-Bereitstellungsmodell bereitgestellte SLES 12 HPC-VM**

```
azure config mode arm

azure vm extension set <resource-group> <vm-name> RDMAUpdateForLinux Microsoft.OSTCExtensions 0.1
```

>[AZURE.NOTE]Die Treiber-Installation kann etwas dauern und der Befehl wird ohne Ausgabe zurückgegeben. Nach dem Update wird Ihr virtueller Computer neu gestartet und sollte innerhalb weniger Minuten einsatzbereit sein.

### Beispielskript für Treiberupdates

Wenn Sie über einen Cluster von SLES 12 HPC-VMs verfügen, können Sie das Treiberupdate für alle Knoten im Cluster durchführen. Das folgende Skript aktualisiert z.B. die Treiber in einem Cluster mit 8 Knoten.

```

#!/bin/bash -x

# Define a prefix naming scheme for compute nodes, e.g., cluster11, cluster12, etc.

vmname=cluster

# Plug in appropriate numbers in the for loop below.

for (( i=11; i<19; i++ )); do

# For VMs created in the classic deployment model use the following command in your script.

azure vm extension set $vmname$i RDMAUpdateForLinux Microsoft.OSTCExtensions 0.1

# For VMs created in the Resource Manager deployment model, use the following command in your script.

# azure vm extension set <resource-group> $vmname$i RDMAUpdateForLinux Microsoft.OSTCExtensions 0.1

done

```


## Überlegungen zu HPC Pack und Linux

[HPC Pack](https://technet.microsoft.com/library/jj899572.aspx) ist die kostenlose HPC-Cluster- und Auftragsverwaltungslösung von Microsoft. Die neuesten Versionen von HPC Pack 2012 R2 unterstützen das Ausführen von mehreren Linux-Distributionen auf Computeknoten, die auf virtuellen Azure-Computern bereitgestellt wurden und von einem Windows Server-Hauptknoten verwaltet werden. Auf virtuellen A8- oder A9-Computern bereitgestellte Linux-Computeknoten, auf denen eine unterstützte MPI-Implementierung ausgeführt wird, sind für MPI-Anwendungen geeignet, die auf das RDMA-Netzwerk zugreifen. Eine Einführung finden Sie unter [Erste Schritte mit Linux-Computeknoten in einem HPC Pack-Cluster in Azure](virtual-machines-linux-classic-hpcpack-cluster.md).

## Überlegungen zur Netzwerktopologie

* Für Linux-VMs in Azure der Größe A8 oder A9 ist „Eth1“ für RDMA-Netzwerkverkehr reserviert. Ändern Sie keine Eth1-Einstellungen oder anderen Informationen in der Konfigurationsdatei, die sich auf dieses Netzwerk beziehen. „Eth0“ ist für den regulären Azure-Netzwerkverkehr reserviert.

* In Azure wird IP over InfiniBand (IB) nicht unterstützt. Nur RDMA over IB wird unterstützt.


## Nächste Schritte

* Ausführliche Informationen zu Verfügbarkeit und Preisen von A8-, A9-, A10- und A11-Instanzen finden Sie unter [Virtual Machines – Preise](https://azure.microsoft.com/pricing/details/virtual-machines/#Linux).

* Informationen zu Speicherkapazitäten und Details zu den Datenträgern finden Sie unter [Größen für virtuelle Computer](virtual-machines-linux-sizes.md).

* Informationen zu ersten Schritten bei der Bereitstellung und Verwendung von A8- und A9-Instanzen mit RDMA unter Linux finden Sie unter [Einrichten eines Linux RDMA-Clusters zum Ausführen von MPI-Anwendungen](virtual-machines-linux-classic-rdma-cluster.md).

<!---HONumber=AcomDC_0810_2016-->