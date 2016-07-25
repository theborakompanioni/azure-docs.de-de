<properties
   pageTitle="PowerShell-Skript zum Bereitstellen eines Linux-HPC-Clusters | Microsoft Azure"
   description="Ausführen eines PowerShell-Skripts zum Bereitstellen eines Linux HPC Pack-Clusters auf virtuellen Azure-Computern"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="dlepow"
   manager="timlt"
   editor=""
   tags="azure-service-management,hpc-pack"/>
<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="big-compute"
   ms.date="07/07/2016"
   ms.author="danlep"/>

# Erstellen eines High Performance Computing (HPC)-Clusters mit dem HPC Pack-IaaS-Bereitstellungsskript

Führen Sie das PowerShell-Skript für die HPC Pack-IaaS-Bereitstellung aus, um einen vollständigen HPC Pack-Cluster für Linux-Workloads auf virtuellen Azure-Computern bereitzustellen. Der Cluster besteht aus einem mit Active Directory verknüpften Hauptknoten mit Windows Server und Microsoft HPC Pack sowie Computeknoten, auf denen eine der von HPC Pack unterstützten Linux-Distributionen ausgeführt wird. Falls Sie einen HPC Pack-Cluster in Azure für Windows-Workloads bereitstellen möchten, finden Sie weitere Informationen unter [Erstellen eines High Performance Computing (HPC)-Clusters mit virtuellen Windows-Computern mit dem HPC Pack-IaaS-Bereitstellungsskript](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md). Sie können auch eine Azure-Ressourcen-Manager-Vorlage verwenden, um einen HPC Pack-Cluster bereitzustellen. Ein Beispiel finden Sie unter [Create an HPC cluster with Linux compute nodes](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-linux-cn/) (Erstellen eines HPC-Clusters mit Linux-Computeknoten).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

[AZURE.INCLUDE [virtual-machines-common-classic-hpcpack-cluster-powershell-script](../../includes/virtual-machines-common-classic-hpcpack-cluster-powershell-script.md)]

## Beispielkonfigurationsdatei

Die folgende Konfigurationsdatei erstellt einen neuen Domänencontroller und eine neue Domänengesamtstruktur und stellt einen HPC Pack-Cluster bereit, der über einen Hauptknoten mit lokalen Datenbanken und zehn Linux-Computeknoten verfügt. Alle Clouddienste werden direkt am Standort in Ostasien erstellt. Die Linux-Computeknoten werden in zwei Clouddiensten und zwei Speicherkonten erstellt (d.h. _MyLnxCN-0001_ bis _MyLnxCN-0005_ in _MyLnxCNService01_ und _mylnxstorage01_ und _MyLnxCN-0006_ bis _MyLnxCN-0010_ in _MyLnxCNService02_ und _mylnxstorage02_). Die Computeknoten werden aus einem Linux-Image (OpenLogic CentOS Version 7.0) erstellt.

Ersetzen Sie den Abonnementnamen und den Konto- und Dienstnamen durch Ihre eigenen Werte.

```
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
## Problembehandlung

* **Fehler „VNet nicht vorhanden“:** Wenn Sie das HPC Pack-IaaS-Bereitstellungsskript ausführen, um in Azure gleichzeitig mehrere Cluster unter einem Abonnement bereitzustellen, schlagen eine oder mehrere Bereitstellungen unter Umständen mit einem Fehler der Art „VNet *VNet-Name* nicht vorhanden“ fehl. Führen Sie das Skript für die fehlgeschlagene Bereitstellung erneut aus, wenn dieser Fehler auftritt.

* **Problem beim Zugreifen auf das Internet über das virtuelle Azure-Netzwerk:** Falls Sie einen HPC Pack-Cluster mit einem neuen Domänencontroller erstellen, indem Sie das Bereitstellungsskript verwenden, oder wenn Sie eine Hauptknoten-VM manuell zu einem Domänencontroller heraufstufen, können Probleme beim Herstellen der Internetverbindung für die virtuellen Computer im virtuellen Azure-Netzwerk auftreten. Dies kann passieren, wenn auf dem Domänencontroller automatisch ein DNS-Weiterleitungsserver konfiguriert wird und dieser Server die Auflösung nicht richtig durchführt.

    Sie können dieses Problem umgehen, indem Sie sich am Domänencontroller anmelden und entweder die Konfigurationseinstellung für die Weiterleitung entfernen oder einen gültigen DNS-Weiterleitungsserver konfigurieren. Klicken Sie hierzu im Server-Manager auf **Extras** > **DNS**, um den DNS-Manager zu öffnen, und doppelklicken Sie dann auf **Weiterleitungen**.
    
## Nächste Schritte

* Unter [Erste Schritte mit Linux-Computeknoten in einem HPC Pack-Cluster in Azure](virtual-machines-linux-classic-hpcpack-cluster.md) finden Sie Informationen zu unterstützten Linux-Distributionen sowie zum Verschieben von Daten und Senden von Aufträgen an HPC Pack-Cluster mit Linux-Computeknoten.
* Tutorials, in denen das Skript zum Erstellen eines Cluster und Ausführen einer Linux HPC-Workload verwendet wird, finden Sie in folgenden Artikeln:
    * [Ausführen von NAMD mit dem Microsoft HPC Pack auf Linux-Computeknoten in Azure](virtual-machines-linux-classic-hpcpack-cluster-namd.md)
    * [Ausführen von OpenFOAM mit Microsoft HPC Pack auf einem Linux-RDMA-Cluster in Azure](virtual-machines-linux-classic-hpcpack-cluster-openfoam.md)
    * [Ausführen von STAR-CCM+ mit Microsoft HPC Pack auf einem Linux-RDMA-Cluster in Azure](virtual-machines-linux-classic-hpcpack-cluster-starccm.md)

<!---HONumber=AcomDC_0713_2016-->