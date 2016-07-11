<properties
   pageTitle="PowerShell-Skript zum Bereitstellen eines Linux-HPC-Clusters | Microsoft Azure"
   description="Führen Sie ein PowerShell-Skript aus, um einen Linux-HPC-Cluster in Azure-Infrastrukturdiensten bereitzustellen."
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
   ms.date="04/05/2016"
   ms.author="danlep"/>

# Erstellen eines High Performance Computing (HPC)-Clusters mit dem HPC Pack-IaaS-Bereitstellungsskript

Führen Sie das PowerShell-Skript für die HPC Pack-IaaS-Bereitstellung auf einem Clientcomputer aus, um einen vollständigen HPC-Cluster für Linux-Workloads in Azure-Infrastrukturdiensten (IaaS) bereitzustellen. Falls Sie einen HPC Pack-Cluster in Azure für Windows-Workloads bereitstellen möchten, finden Sie weitere Informationen unter [Erstellen eines High Performance Computing (HPC)-Clusters mit virtuellen Windows-Computern mit dem HPC Pack-IaaS-Bereitstellungsskript](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

[AZURE.INCLUDE [virtual-machines-common-classic-hpcpack-cluster-powershell-script](../../includes/virtual-machines-common-classic-hpcpack-cluster-powershell-script.md)]

## Beispielkonfigurationsdateien

### Beispiel 1

Die folgende Konfigurationsdatei erstellt eine neue Domänengesamtstruktur und stellt einen HPC Pack-Cluster bereit, der über einen Hauptknoten mit lokalen Datenbanken und 20 Linux-Computeknoten verfügt. Alle Clouddienste werden direkt am Standort in Ostasien erstellt. Die Linux-Computeknoten werden in vier Clouddiensten und vier Speicherkonten erstellt (d.h. _MyLnxCN-0001_ bis _MyLnxCN-0005_ in _MyLnxCNService01_ und _mylnxstorage01_, _MyLnxCN-0006_ bis _MyLnxCN-0010_ in _MyLnxCNService02_ und _mylnxstorage02_, _MyLnxCN-0011_ bis _MyLnxCN-0015_ in _MyLnxCNService03_ und _mylnxstorage03_ sowie _MyLnxCN-0016_ bis _MyLnxCN-0020_ in _MyLnxCNService04_ und _mylnxstorage04_). Die Computeknoten werden aus einem Linux-Image (OpenLogic CentOS Version 7.0) erstellt.

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
    <NodeCount>20</NodeCount>
    <ImageName>5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20150325 </ImageName>
    <SSHKeyPairForRoot>
      <PfxFile>d:\mytestcert1.pfx</PfxFile>
      <Password>MyPsw!!2</Password>
    </SSHKeyPairForRoot>
  </LinuxComputeNodes>
</IaaSClusterConfig>
```
## Problembehandlung

* **Fehler „VNet nicht vorhanden“:** Wenn Sie das HPC Pack-IaaS-Bereitstellungsskript ausführen, um in Azure gleichzeitig mehrere Cluster unter einem Abonnement bereitzustellen, schlagen eine oder mehrere Bereitstellungen unter Umständen mit einem Fehler der Art „VNet *VNet-Name* nicht vorhanden“ fehl. Führen Sie das Skript für die fehlgeschlagene Bereitstellung erneut aus, wenn dieser Fehler auftritt.

* **Problem beim Zugreifen auf das Internet über das virtuelle Azure-Netzwerk:** Falls Sie einen HPC Pack-Cluster mit einem neuen Domänencontroller erstellen, indem Sie das Bereitstellungsskript verwenden, oder wenn Sie eine Hauptknoten-VM manuell zu einem Domänencontroller heraufstufen, können Probleme beim Herstellen der Internetverbindung für die virtuellen Computer im virtuellen Azure-Netzwerk auftreten. Dies kann passieren, wenn auf dem Domänencontroller automatisch ein DNS-Weiterleitungsserver konfiguriert wird und dieser Server die Auflösung nicht richtig durchführt.

    Sie können dieses Problem umgehen, indem Sie sich am Domänencontroller anmelden und entweder die Konfigurationseinstellung für die Weiterleitung entfernen oder einen gültigen DNS-Weiterleitungsserver konfigurieren. Klicken Sie hierzu im Server-Manager auf **Extras** > **DNS**, um den DNS-Manager zu öffnen, und doppelklicken Sie dann auf **Weiterleitungen**.
    
## Nächste Schritte

* Tutorials, in denen das Skript zum Erstellen eines Clusters und Ausführen einer Linux-HPC-Workload verwendet werden, finden Sie unter [Ausführen von NAMD mit dem Microsoft HPC Pack auf Linux-Computeknoten in Azure](virtual-machines-linux-classic-hpcpack-cluster-namd.md) oder [Ausführen von OpenFOAM mit dem Microsoft HPC Pack auf Linux-Computeknoten in Azure](virtual-machines-linux-classic-hpcpack-cluster-openfoam.md).

* Sie können auch eine Azure-Ressourcen-Manager-Vorlage verwenden, um einen HPC Pack-Cluster bereitzustellen. Ein Beispiel finden Sie unter [Create an HPC cluster with Linux compute nodes](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-linux-cn/) (Erstellen eines HPC-Clusters mit Linux-Computeknoten).

<!---HONumber=AcomDC_0629_2016-->