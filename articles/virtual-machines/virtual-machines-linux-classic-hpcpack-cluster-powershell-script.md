---
title: PowerShell-Skript zum Bereitstellen eines Linux-HPC-Clusters | Microsoft Docs
description: "Ausführen eines PowerShell-Skripts zum Bereitstellen eines Linux HPC Pack 2012 R2-Clusters auf virtuellen Azure-Computern"
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,hpc-pack
ms.assetid: 73041960-58d3-4ecf-9540-d7e1a612c467
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: big-compute
ms.date: 12/29/2016
ms.author: danlep
translationtype: Human Translation
ms.sourcegitcommit: ff9fb5f0b2229a470ea3f5c736622ee1e9228c93
ms.openlocfilehash: 34920aa6cb351f52fd630cde2f97850266268f96


---
# <a name="create-a-linux-high-performance-computing-hpc-cluster-with-the-hpc-pack-iaas-deployment-script"></a>Erstellen eines High Performance Computing-Clusters (HPC) mit dem HPC Pack-IaaS-Bereitstellungsskript
Führen Sie das PowerShell-Skript für die HPC Pack-IaaS-Bereitstellung aus, um einen vollständigen HPC Pack 2012 R2-Cluster für Linux-Workloads auf virtuellen Azure-Computern bereitzustellen. Der Cluster besteht aus einem mit Active Directory verknüpften Hauptknoten mit Windows Server und Microsoft HPC Pack sowie Computeknoten, auf denen eine der von HPC Pack unterstützten Linux-Distributionen ausgeführt wird. Falls Sie einen HPC Pack-Cluster in Azure für Windows-Workloads bereitstellen möchten, finden Sie weitere Informationen unter [Erstellen eines High Performance Computing (HPC)-Clusters mit virtuellen Windows-Computern mit dem HPC Pack-IaaS-Bereitstellungsskript](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json). Sie können auch eine Azure-Ressourcen-Manager-Vorlage verwenden, um einen HPC Pack-Cluster bereitzustellen. Ein Beispiel finden Sie unter [Create an HPC cluster with Linux compute nodes](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-linux-cn/)(Erstellen eines HPC-Clusters mit Linux-Computeknoten).

> [!IMPORTANT] 
> Das in diesem Artikel beschriebene PowerShell-Skript erstellt einen Microsoft HPC Pack 2012 R2-Cluster in Azure mit dem klassischen Bereitstellungsmodell. Microsoft empfiehlt für die meisten neuen Bereitstellungen die Verwendung des Ressourcen-Manager-Modells.
> Das in diesem Artikel beschriebene Skript unterstützt außerdem nicht HPC Pack 2016.

[!INCLUDE [virtual-machines-common-classic-hpcpack-cluster-powershell-script](../../includes/virtual-machines-common-classic-hpcpack-cluster-powershell-script.md)]

## <a name="example-configuration-file"></a>Beispielkonfigurationsdatei
Die folgende Konfigurationsdatei erstellt einen Domänencontroller und eine Domänengesamtstruktur und stellt einen HPC Pack-Cluster bereit, der über einen Hauptknoten mit lokalen Datenbanken und zehn Linux-Serverknoten verfügt. Alle Clouddienste werden direkt am Standort in Ostasien erstellt. Die Linux-Serverknoten werden in zwei Clouddiensten und zwei Speicherkonten erstellt (d.h. *MyLnxCN-0001* bis *MyLnxCN-0005* in *MyLnxCNService01* und *mylnxstorage01*, und *MyLnxCN-0006* bis *MyLnxCN-0010* in *MyLnxCNService02* und *mylnxstorage02*). Die Computeknoten werden aus einem Linux-Image (OpenLogic CentOS Version 7.0) erstellt. 

Ersetzen Sie den Abonnementnamen und den Konto- und Dienstnamen durch Ihre eigenen Werte.

```Xml
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <Location>East Asia</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>NewDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
    <DomainController>
      <VMName>MyDCServer</VMName>
      <ServiceName>MyHPCService</ServiceName>
      <VMSize>Large</VMSize>
    </DomainController>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>MyLnxCN-%0001%</VMNamePattern>
    <ServiceNamePattern>MyLnxCNService%01%</ServiceNamePattern>
    <MaxNodeCountPerService>5</MaxNodeCountPerService>
    <StorageAccountNamePattern>mylnxstorage%01%</StorageAccountNamePattern>
    <VMSize>Medium</VMSize>
    <NodeCount>10</NodeCount>
    <ImageName>5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20150325 </ImageName>
  </LinuxComputeNodes>
</IaaSClusterConfig>
```
## <a name="troubleshooting"></a>Problembehandlung
* **Fehler „VNET ist nicht vorhanden“**. Wenn Sie das HPC Pack IaaS-Bereitstellungsskript ausführen, um in Azure gleichzeitig mehrere Cluster unter einem Abonnement bereitzustellen, schlagen eine oder mehrere Bereitstellungen unter Umständen mit einem Fehler der Art „VNET *VNET\_Name* nicht vorhanden“ fehl.
  Führen Sie das Skript für die entsprechende Bereitstellung erneut aus, wenn dieser Fehler auftritt.
* **Problem beim Zugreifen auf das Internet über das virtuelle Azure-Netzwerk**. Wenn Sie einen HPC Pack-Cluster mit einem neuen Domänencontroller erstellen, indem Sie das Bereitstellungsskript verwenden, oder wenn Sie einen virtuellen Hauptknotencomputer manuell zu einem Domänencontroller heraufstufen, treten unter Umständen Probleme beim Herstellen der Internetverbindung für die VMs im virtuellen Azure-Netzwerk auf. Dies kann passieren, wenn auf dem Domänencontroller automatisch ein DNS-Weiterleitungsserver konfiguriert wird und dieser Server die Auflösung nicht richtig durchführt.
  
    Sie können dieses Problem umgehen, indem Sie sich am Domänencontroller anmelden und entweder die Konfigurationseinstellung für die Weiterleitung entfernen oder einen gültigen DNS-Weiterleitungsserver konfigurieren. Klicken Sie hierzu im Server-Manager auf **Extras** > **DNS**, um den DNS-Manager zu öffnen, und doppelklicken Sie dann auf **Weiterleitungen**.

## <a name="next-steps"></a>Nächste Schritte
* Unter [Erste Schritte mit Linux-Computeknoten in einem HPC Pack-Cluster in Azure](virtual-machines-linux-classic-hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) finden Sie Informationen zu unterstützten Linux-Distributionen sowie zum Verschieben von Daten und Senden von Aufträgen an HPC Pack-Cluster mit Linux-Serverknoten.
* Tutorials, in denen das Skript zum Erstellen eines Cluster und Ausführen einer Linux HPC-Workload verwendet wird, finden Sie in folgenden Artikeln:
  * [Ausführen von NAMD mit dem Microsoft HPC Pack auf Linux-Computeknoten in Azure](virtual-machines-linux-classic-hpcpack-cluster-namd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
  * [Ausführen von OpenFOAM mit Microsoft HPC Pack auf Linux-Serverknoten in Azure](virtual-machines-linux-classic-hpcpack-cluster-openfoam.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
  * [Ausführen von STAR-CCM+ mit Microsoft HPC Pack auf einem Linux-RDMA-Cluster in Azure](virtual-machines-linux-classic-hpcpack-cluster-starccm.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)




<!--HONumber=Feb17_HO3-->


