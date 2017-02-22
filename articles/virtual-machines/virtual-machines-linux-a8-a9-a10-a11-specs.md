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
ms.date: 11/18/2016
ms.author: danlep
translationtype: Human Translation
ms.sourcegitcommit: f73cbfec2ce2e41589b84997891ff0b60266c9b2
ms.openlocfilehash: 10a10e138c9c8ed8c15136bf1d6565edc57758b5


---
# <a name="about-h-series-and-compute-intensive-a-series-vms"></a>Informationen zu virtuellen Computern der H-Serie und der rechenintensiven A-Serie
Hier finden Sie Hintergrundinformationen und einige Überlegungen zur Verwendung der neueren Azure H-Serie und der älteren Größen A8, A9, A10 und A11, die auch als *rechenintensive* Instanzen bezeichnet werden. Dieser Artikel konzentriert sich auf die Verwendung dieser Größen für virtuelle Linux-Computer. Dieser Artikel ist auch für [virtuelle Windows-Computer](virtual-machines-windows-a8-a9-a10-a11-specs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)verfügbar. 

Informationen zu grundlegenden Spezifikationen, Speicherkapazitäten und Details zu den Datenträgern finden Sie unter [Größen für virtuelle Computer](virtual-machines-linux-sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

[!INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## <a name="access-to-the-rdma-network"></a>Zugreifen auf das RDMA-Netzwerk
Sie können Cluster mit RDMA-fähigen virtuellen Linux-Computern erstellen, auf denen eine der folgenden unterstützten Linux-HPC-Distributionen und eine unterstützte MPI-Implementierung ausgeführt werden, um von den Vorteilen des Azure RDMA-Netzwerks zu profitieren. Bereitstellungsoptionen und Beispielkonfigurationsschritte finden Sie unter [Einrichten eines Linux RDMA-Clusters zum Ausführen von MPI-Anwendungen](virtual-machines-linux-classic-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) .

* **Distributionen:** Sie müssen virtuelle Computer auf der Grundlage von RDMA-fähigen SLES-basierten (SUSE Linux Enterprise Server) oder OpenLogic CentOS-basierten HPC-Images aus dem Azure Marketplace bereitstellen. Die erforderlichen Linux RDMA-Treiber werden nur von den folgenden Marketplace-Images unterstützt:
  
  * SLES 12 SP1 für HPC, SLES 12 SP1 für HPC (Premium)
  * SLES 12 für HPC, SLES 12 für HPC (Premium)
  * 7.1 HPC (CentOS-basiert)
  * 6.5 HPC (CentOS-basiert)
    
    > [!NOTE]
    > Für virtuelle Computer der H-Serie empfehlen wir entweder ein Image vom Typ „SLES 12 SP1 für HPC“ oder ein Image vom Typ „7.1 HPC (CentOS-basiert)“.
    > 
    > Bei den CentOS-basierten HPC-Images sind Kernel-Updates in der **yum** -Konfigurationsdatei deaktiviert. Der Grund: Die Linux RDMA-Treiber werden als RPM-Paket verteilt, und Treiberupdates funktionieren möglicherweise nicht, wenn der Kernel aktualisiert wird.
    > 
    > 
* **MPI** : Intel MPI Library 5.x
  
    Je nach verwendetem Marketplace-Image sind für Intel MPI unter Umständen zusätzliche Lizenzierungs-, Installations- oder Konfigurationsschritte erforderlich: 
  
  * **SLES 12 SP1 für HPC** : Führen Sie den folgenden Befehl aus, um die auf dem virtuellen Computer bereitgestellten Intel MPI-Pakete zu installieren:
    
          sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm
  * **SLES 12 für HPC** : Zum Herunterladen und Installieren von Intel MPI ist eine separate Registrierung erforderlich. Anleitungen finden Sie in der [Dokumentation zu Intel MPI Library](https://software.intel.com/en-us/intel-mpi-library/documentation).
  * **CentOS-basierte HPC-Images**: Intel MPI 5.1 ist bereits vorinstalliert.  
    
    Wenn Sie MPI-Aufträge auf gruppierten virtuellen Computer ausführen möchten, sind zusätzliche Konfigurationsschritte erforderlich. So müssen Sie in einem Cluster mit virtuellen Computern beispielsweise die Vertrauensstellung zwischen den Computeknoten einrichten. Informationen zu typischen Einstellungen finden Sie unter [Einrichten eines Linux RDMA-Clusters zum Ausführen von MPI-Anwendungen](virtual-machines-linux-classic-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

## <a name="considerations-for-hpc-pack-and-linux"></a>Überlegungen zu HPC Pack und Linux
[HPC Pack](https://technet.microsoft.com/library/jj899572.aspx)ist eine kostenlose HPC-Cluster- und Auftragsverwaltungslösung von Microsoft und bietet eine Option für die Verwendung der rechenintensiven Instanzen mit Linux. Die neuesten Versionen von HPC Pack 2012 R2 unterstützen das Ausführen von mehreren Linux-Distributionen auf Computeknoten, die auf virtuellen Azure-Computern bereitgestellt wurden und von einem Windows Server-Hauptknoten verwaltet werden. Mit RDMA-fähigen Linux-Computeknoten, auf denen Intel MPI ausgeführt wird, kann HPC Pack Linux-basierte MPI-Anwendungen mit Zugriff auf das RDMA-Netzwerk planen und ausführen. Eine Einführung finden Sie unter [Erste Schritte mit Linux-Computeknoten in einem HPC Pack-Cluster in Azure](virtual-machines-linux-classic-hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

## <a name="network-topology-considerations"></a>Überlegungen zur Netzwerktopologie
* Auf RDMA-fähigen virtuellen Linux-Computern in Azure ist „Eth1“ für RDMA-Netzwerkdatenverkehr reserviert. Ändern Sie keine Eth1-Einstellungen oder anderen Informationen in der Konfigurationsdatei, die sich auf dieses Netzwerk beziehen. „Eth0“ ist für den regulären Azure-Netzwerkverkehr reserviert.
* In Azure wird IP over InfiniBand (IB) nicht unterstützt. Nur RDMA over IB wird unterstützt.

## <a name="rdma-driver-updates-for-sles-12"></a>RDMA-Treiberupdates für SLES 12
Nach dem Erstellen eines virtuellen Computers auf der Grundlage eines SLES 12-HPC-Images müssen unter Umständen die RDMA-Treiber auf den virtuellen Computern aktualisiert werden, um eine RDMA-Netzwerkverbindung zu ermöglichen. 

> [!IMPORTANT]
> Bei SLES 12 ist dieser Schritt für die Bereitstellung virtueller HPC-Computer in allen Azure-Regionen **erforderlich** . Der Schritt ist nicht erforderlich, wenn Sie einen virtuellen Computer mit SLES 12 SP1 für HPC, 7.1 HPC (CentOS-basiert) oder 6.5 HPC (CentOS-basiert) bereitstellen. 
> 
> 

Beenden Sie alle **Zypper** -Prozesse oder alle Prozesse, die die SUSE-Repository-Datenbanken auf dem virtuellen Computer sperren, bevor Sie die Treiber aktualisieren. Andernfalls werden die Treiber möglicherweise nicht korrekt aktualisiert.  

Um die Linux RDMA-Treiber auf jedem virtuellen Computer zu aktualisieren, führen Sie eine der folgenden Gruppen von Azure-CLI-Befehle auf Ihrem Clientcomputer aus.

**Unter dem klassischen Bereitstellungsmodell bereitgestellter virtueller Computer mit SLES 12 für HPC**

```
azure config mode asm

azure vm extension set <vm-name> RDMAUpdateForLinux Microsoft.OSTCExtensions 0.1
```

**Unter dem Resource Manager-Bereitstellungsmodell bereitgestellter virtueller Computer mit SLES 12 für HPC**

```
azure config mode arm

azure vm extension set <resource-group> <vm-name> RDMAUpdateForLinux Microsoft.OSTCExtensions 0.1
```

> [!NOTE]
> Die Treiberinstallation kann einige Zeit dauern, und der Befehl wird ohne Ausgabe abgeschlossen. Nach dem Update wird Ihr virtueller Computer neu gestartet und sollte innerhalb weniger Minuten einsatzbereit sein.
> 
> 

### <a name="sample-script-for-driver-updates"></a>Beispielskript für Treiberupdates
Wenn Sie über einen Cluster mit virtuellen Computern mit SLES 12 für HPC verfügen, können Sie das Treiberupdate für alle Knoten im Cluster durchführen. Das folgende Skript aktualisiert z.B. die Treiber in einem Cluster mit 8 Knoten.

```

#!/bin/bash -x

# Define a prefix naming scheme for compute nodes, e.g., cluster11, cluster12, etc.

vmname=cluster

# Plug in appropriate numbers in the for loop below.

for (( i=11; i<19; i++ )); do

# For VMs created in the classic deployment model, use the following command in your script.

azure vm extension set $vmname$i RDMAUpdateForLinux Microsoft.OSTCExtensions 0.1

# For VMs created in the Resource Manager deployment model, use the following command in your script.

# azure vm extension set <resource-group> $vmname$i RDMAUpdateForLinux Microsoft.OSTCExtensions 0.1

done

```


## <a name="next-steps"></a>Nächste Schritte
* Ausführliche Informationen zu Verfügbarkeit und Preisen rechenintensiver Größen finden Sie unter [Virtuelle Linux-Computer – Preise](https://azure.microsoft.com/pricing/details/virtual-machines/#Linux).
* Informationen zu Speicherkapazitäten und Details zu den Datenträgern finden Sie unter [Größen für virtuelle Computer](virtual-machines-linux-sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Informationen zu ersten Schritten bei der Bereitstellung und Verwendung rechenintensiver Größen mit RDMA unter Linux finden Sie unter [Einrichten eines Linux RDMA-Clusters zum Ausführen von MPI-Anwendungen](virtual-machines-linux-classic-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).




<!--HONumber=Nov16_HO4-->


