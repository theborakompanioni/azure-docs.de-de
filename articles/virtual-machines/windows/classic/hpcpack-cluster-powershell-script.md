---
title: PowerShell-Skript zum Bereitstellen eines Windows-HPC-Clusters | Microsoft Docs
description: "Ausführen eines PowerShell-Skripts zum Bereitstellen eines Windows HPC Pack 2012 R2-Clusters auf virtuellen Azure-Computern"
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,hpc-pack
ms.assetid: 286b2be8-2533-40df-b02a-26156b1f1133
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 12/29/2016
ms.author: danlep
translationtype: Human Translation
ms.sourcegitcommit: 356de369ec5409e8e6e51a286a20af70a9420193
ms.openlocfilehash: 85b125ab19671b61d2541af6378c95feb88bf952
ms.lasthandoff: 03/27/2017


---
# <a name="create-a-windows-high-performance-computing-hpc-cluster-with-the-hpc-pack-iaas-deployment-script"></a>Erstellen eines Windows-High Performance Computing (HPC)-Clusters mit dem HPC Pack-IaaS-Bereitstellungsskript
Führen Sie das PowerShell-Skript für die HPC Pack-IaaS-Bereitstellung aus, um einen vollständigen HPC Pack 2012 R2-Cluster für Windows-Workloads auf virtuellen Azure-Computern bereitzustellen. Der Cluster besteht aus einem mit Active Directory verknüpften Hauptknoten mit Windows Server und Microsoft HPC Pack sowie weiteren von Ihnen angegebenen Windows-Computeressourcen. Falls Sie einen HPC Pack-Cluster in Azure für Linux-Workloads bereitstellen möchten, finden Sie weitere Informationen unter [Erstellen eines High Performance Computing (HPC)-Clusters mit virtuellen Linux-Computern mit dem HPC Pack-IaaS-Bereitstellungsskript](../../linux/classic/hpcpack-cluster-powershell-script.md). Sie können auch eine Azure-Ressourcen-Manager-Vorlage verwenden, um einen HPC Pack-Cluster bereitzustellen. Beispiele finden Sie unter [Create an HPC cluster](https://azure.microsoft.com/documentation/templates/create-hpc-cluster/) (Erstellen eines HPC-Clusters) und [Create an HPC cluster with custom compute node image](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-custom-image/) (Erstellen eines HPC-Clusters mit einem benutzerdefinierten Computeknotenimage).

> [!IMPORTANT] 
> Das in diesem Artikel beschriebene PowerShell-Skript erstellt einen Microsoft HPC Pack 2012 R2-Cluster in Azure mit dem klassischen Bereitstellungsmodell. Microsoft empfiehlt für die meisten neuen Bereitstellungen die Verwendung des Ressourcen-Manager-Modells.
> Das in diesem Artikel beschriebene Skript unterstützt außerdem nicht HPC Pack 2016.

[!INCLUDE [virtual-machines-common-classic-hpcpack-cluster-powershell-script](../../../../includes/virtual-machines-common-classic-hpcpack-cluster-powershell-script.md)]

## <a name="example-configuration-files"></a>Beispielkonfigurationsdateien
Ersetzen Sie in den folgenden Beispielen die Abonnement-ID oder den Abonnementnamen sowie den Konto- und Dienstnamen durch Ihre eigenen Werte.

### <a name="example-1"></a>Beispiel 1
Die folgende Konfigurationsdatei stellt einen HPC Pack-Cluster bereit, der über einen Hauptknoten mit lokalen Datenbanken und fünf Computeknoten verfügt, auf denen das Betriebssystem Windows Server 2012 R2 ausgeführt wird. Alle Clouddienste werden direkt am Standort „USA, Westen“ erstellt. Der Hauptknoten fungiert als Domänencontroller der Domänengesamtstruktur.

```Xml
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionId>08701940-C02E-452F-B0B1-39D50119F267</SubscriptionId>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <Location>West US</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>MyHPCCN-%1000%</VMNamePattern>
    <ServiceName>MyHPCCNService</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>5</NodeCount>
    <OSVersion>WindowsServer2012R2</OSVersion>
  </ComputeNodes>
</IaaSClusterConfig>
```

### <a name="example-2"></a>Beispiel 2
Mit der folgenden Konfigurationsdatei wird ein HPC Pack-Cluster in einer vorhandenen Domänengesamtstruktur bereitgestellt. Der Cluster verfügt über einen Hauptknoten mit lokalen Datenbanken und zwölf Computeknoten mit angewendeter BGInfo-VM-Erweiterung.
Die automatische Installation von Windows-Updates ist für alle virtuellen Computer in der Domänengesamtstruktur deaktiviert. Alle Clouddienste werden direkt am Standort in Ostasien erstellt. Die Computeknoten werden in drei Clouddiensten und drei Speicherkonten erstellt: *MyHPCCN-0001* bis *MyHPCCN-0005* in *MyHPCCNService01* und *mycnstorage01*; *MyHPCCN-0006* bis *MyHPCCN0010* in *MyHPCCNService02* und *mycnstorage02*; und *MyHPCCN-0011* bis *MyHPCCN-0012* in *MyHPCCNService03* und *mycnstorage03*. Die Computeknoten werden aus einem vorhandenen privaten Image erstellt, das über einen Computeknoten erfasst wird. Der Dienst zum automatischen Vergrößern und Verkleinern ist mit standardmäßigen Vergrößerungs- und Verkleinerungsintervallen aktiviert.

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
     <NoWindowsAutoUpdate />
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
  </HeadNode>
  <Certificates>
    <Certificate>
      <Id>1</Id>
      <PfxFile>d:\mytestcert1.pfx</PfxFile>
      <Password>MyPsw!!2</Password>
    </Certificate>
  </Certificates>
  <ComputeNodes>
    <VMNamePattern>MyHPCCN-%0001%</VMNamePattern>
<ServiceNamePattern>MyHPCCNService%01%</ServiceNamePattern>
<MaxNodeCountPerService>5</MaxNodeCountPerService>
<StorageAccountNamePattern>mycnstorage%01%</StorageAccountNamePattern>
    <VMSize>Medium</VMSize>
    <NodeCount>12</NodeCount>
    <ImageName HPCPackInstalled=”true”>MyHPCComputeNodeImage</ImageName>
    <VMExtensions>
       <VMExtension>
          <ExtensionName>BGInfo</ExtensionName>
          <Publisher>Microsoft.Compute</Publisher>
          <Version>1.*</Version>
       </VMExtension>
    </VMExtensions>
  </ComputeNodes>
  <AutoGrowShrink>
    <CertificateId>1</CertificateId>
  </AutoGrowShrink>
</IaaSClusterConfig>

```

### <a name="example-3"></a>Beispiel 3
Mit der folgenden Konfigurationsdatei wird ein HPC Pack-Cluster in einer vorhandenen Domänengesamtstruktur bereitgestellt. Der Cluster enthält einen Hauptknoten, einen Datenbankserver mit einem 500-GB-Datenträger, zwei Brokerknoten, auf denen das Betriebssystem Windows Server 2012 R2 ausgeführt wird, und fünf Serverknoten, auf denen das Betriebssystem Windows Server 2012 R2 ausgeführt wird. Der MyHPCCNService-Clouddienst wird in der Affinitätsgruppe *MyIBAffinityGroup* erstellt, und alle anderen Clouddienste werden in der Affinitätsgruppe *MyAffinityGroup* erstellt. Die HPC-Auftragsplaner-REST-API und das HPC-Webportal sind auf dem Hauptknoten aktiviert.

```Xml
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <AffinityGroup>MyAffinityGroup</AffinityGroup>
  <Location>East Asia</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>    
  <Domain>
    <DCOption>ExistingDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>NewRemoteDB</DBOption>
    <DBVersion>SQLServer2014_Enterprise</DBVersion>
    <DBServer>
      <VMName>MyDBServer</VMName>
      <ServiceName>MyHPCService</ServiceName>
      <VMSize>ExtraLarge</VMSize>
      <DataDiskSizeInGB>500</DataDiskSizeInGB>
    </DBServer>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>MyHPCCN-%0000%</VMNamePattern>
    <ServiceName>MyHPCCNService</ServiceName>
    <VMSize>A8</VMSize>
<NodeCount>5</NodeCount>
<AffinityGroup>MyIBAffinityGroup</AffinityGroup>
  </ComputeNodes>
  <BrokerNodes>
    <VMNamePattern>MyHPCBN-%0000%</VMNamePattern>
    <ServiceName>MyHPCBNService</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>2</NodeCount>
  </BrokerNodes>
</IaaSClusterConfig>
```



### <a name="example-4"></a>Beispiel 4
Mit der folgenden Konfigurationsdatei wird ein HPC Pack-Cluster in einer vorhandenen Domänengesamtstruktur bereitgestellt. Der Cluster verfügt über einen Hauptknoten mit lokalen Datenbanken. Es werden zwei Vorlagen für Azure-Knoten und drei mittelgroße Azure-Knoten für die Azure-Knotenvorlage *AzureTemplate1* erstellt. Eine Skriptdatei wird auf dem Hauptknoten ausgeführt, nachdem der Hauptknoten konfiguriert wurde.

```Xml
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <AffinityGroup>MyAffinityGroup</AffinityGroup>
  <Location>East Asia</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>ExistingDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
<VMSize>ExtraLarge</VMSize>
    <PostConfigScript>c:\MyHNPostActions.ps1</PostConfigScript>
  </HeadNode>
  <Certificates>
    <Certificate>
      <Id>1</Id>
      <PfxFile>d:\mytestcert1.pfx</PfxFile>
      <Password>MyPsw!!2</Password>
    </Certificate>
    <Certificate>
      <Id>2</Id>
      <PfxFile>d:\mytestcert2.pfx</PfxFile>
    </Certificate>    
  </Certificates>
  <AzureBurst>
    <AzureNodeTemplate>
      <TemplateName>AzureTemplate1</TemplateName>
      <SubscriptionId>bb9252ba-831f-4c9d-ae14-9a38e6da8ee4</SubscriptionId>
      <CertificateId>1</CertificateId>
      <ServiceName>mytestsvc1</ServiceName>
      <StorageAccount>myteststorage1</StorageAccount>
      <NodeCount>3</NodeCount>
      <RoleSize>Medium</RoleSize>
    </AzureNodeTemplate>
    <AzureNodeTemplate>
      <TemplateName>AzureTemplate2</TemplateName>
      <SubscriptionId>ad4b9f9f-05f2-4c74-a83f-f2eb73000e0b</SubscriptionId>
      <CertificateId>1</CertificateId>
      <ServiceName>mytestsvc2</ServiceName>
      <StorageAccount>myteststorage2</StorageAccount>
      <Proxy>
        <UsesStaticProxyCount>false</UsesStaticProxyCount>     
        <ProxyRatio>100</ProxyRatio>
        <ProxyRatioBase>400</ProxyRatioBase>
      </Proxy>
      <OSVersion>WindowsServer2012</OSVersion>
    </AzureNodeTemplate>
  </AzureBurst>
</IaaSClusterConfig>
```

## <a name="troubleshooting"></a>Problembehandlung
* **Fehler „VNet nicht vorhanden“** – Wenn Sie das Skript ausführen, um in Azure gleichzeitig mehrere Cluster unter einem Abonnement bereitzustellen, schlagen eine oder mehrere Bereitstellungen unter Umständen mit einem Fehler der Art „VNet *VNet\_Name* nicht vorhanden“ fehl.
  Führen Sie das Skript für die fehlgeschlagene Bereitstellung erneut aus, wenn dieser Fehler auftritt.
* **Problem beim Zugreifen auf das Internet über das virtuelle Azure-Netzwerk** : Wenn Sie einen Cluster mit einem neuen Domänencontroller erstellen, indem Sie das Bereitstellungsskript verwenden, oder wenn Sie einen virtuellen Hauptknotencomputer manuell zu einem Domänencontroller heraufstufen, treten unter Umständen Probleme beim Herstellen der Internetverbindung für die virtuellen Computer auf. Dieses Problem kann auftreten, wenn auf dem Domänencontroller automatisch ein DNS-Weiterleitungsserver konfiguriert wird und dieser die Auflösung nicht richtig durchführt.
  
    Sie können dieses Problem umgehen, indem Sie sich am Domänencontroller anmelden und entweder die Konfigurationseinstellung für die Weiterleitung entfernen oder einen gültigen DNS-Weiterleitungsserver konfigurieren. Klicken Sie zum Konfigurieren dieser Einstellung im Server-Manager auf **Extras** >
    **DNS**, um den DNS-Manager zu öffnen, und doppelklicken Sie dann auf **Weiterleitungen**.
* **Problem beim Zugreifen auf das RDMA-Netzwerk über rechenintensive virtuelle Computer** : Wenn Sie Windows Server-Computeknoten- oder -Brokerknoten-VMs hinzufügen, die eine RDMA-fähige Größe wie A8 oder A9 verwenden, treten beim Verbinden dieser virtuellen Computer mit dem RDMA-Anwendungsnetzwerk unter Umständen Probleme auf. Ein möglicher Grund hierfür ist die falsche Installation der HpcVmDrivers-Erweiterung, wenn die virtuellen Computer dem Cluster hinzugefügt werden. Beispielsweise kann es vorkommen, dass die Erweiterung im Installationsstatus verharrt.
  
    Um dieses Problem zu umgehen, überprüfen Sie auf den virtuellen Computern zunächst den Status der Erweiterung. Wenn die Erweiterung nicht richtig installiert ist, können Sie versuchen, die Knoten aus dem HPC-Cluster zu entfernen und sie dann neu hinzuzufügen. Beispielsweise können Sie Computeknoten-VMs hinzufügen, indem Sie das Skript „Add-HpcIaaSNode.ps1“ auf dem Hauptknoten ausführen.

## <a name="next-steps"></a>Nächste Schritte
* Führen Sie eine Test-Workload auf dem Cluster aus. Ein Beispiel hierzu finden Sie im [Leitfaden für die ersten Schritte](https://technet.microsoft.com/library/jj884144)von HPC Pack.
* Ein Tutorial für ein Skript zur Cluster-Bereitstellung und zur Ausführung einer HPC-Workload, finden Sie unter [Erste Schritte mit Excel- und SOA-Workloads in einem HPC Pack-Cluster in Azure](../../virtual-machines-windows-excel-cluster-hpcpack.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Probieren Sie die Tools von HPC Pack zum Starten, Beenden, Hinzufügen und Entfernen von Computeknoten von einem von Ihnen erstellten Cluster aus. Weitere Informationen finden Sie unter [Verwalten von Computeknoten in einem HPC Pack-Cluster in Azure](hpcpack-cluster-node-manage.md).
* Informationen zum Übermitteln von Aufträgen an den Cluster von einem lokalen Computer finden Sie unter [Übermitteln von HPC-Aufträgen von einem lokalen Computer an einen in Azure bereitgestellten HPC Pack-Cluster](../../virtual-machines-windows-hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


