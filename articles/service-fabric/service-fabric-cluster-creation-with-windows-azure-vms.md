---
title: "Erstellen eines eigenständigen Clusters mit Azure-VMs, auf denen Windows ausgeführt wird | Microsoft Docs"
description: Es wird beschrieben, wie Sie einen Azure Service Fabric-Cluster auf virtuellen Azure-Computern mit Windows Server erstellen und verwalten.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 7eeb40d2-fb22-4a77-80ca-f1b46b22edbc
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/21/2017
ms.author: ryanwi;chackdan
redirect_url: /azure/service-fabric/service-fabric-cluster-creation-via-arm
ms.translationtype: Human Translation
ms.sourcegitcommit: b4802009a8512cb4dcb49602545c7a31969e0a25
ms.openlocfilehash: ba1f6e5662700c309fcf198a4e114fd9b2b47c5f
ms.contentlocale: de-de
ms.lasthandoff: 03/29/2017


---
# <a name="create-a-three-node-standalone-service-fabric-cluster-with-azure-virtual-machines-running-windows-server"></a>Erstellen eines eigenständigen Service Fabric-Clusters mit drei Knoten und Azure-VMs mit Windows Server
In diesem Artikel wird beschrieben, wie Sie einen Cluster auf Windows-basierten Azure-VMs (Virtual Machines) erstellen, indem Sie den eigenständigen Service Fabric-Installer für Windows Server verwenden. Dies ist ein Sonderfall von [Erstellen und Verwalten eines Windows Server-Clusters](service-fabric-cluster-creation-for-windows-server.md), bei dem die VMs [unter Windows Server ausgeführte Azure-VMs](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) sind. Sie erstellen aber keinen [Service Fabric-Cluster basierend auf der Azure-Cloud](service-fabric-cluster-creation-via-portal.md). Der Unterschied zu dieser Vorgehensweise besteht darin, dass der eigenständige Service Fabric-Cluster, der mit den folgenden Schritten erstellt wird, vollständig von Ihnen verwaltet wird. Die Service Fabric-Cluster auf Azure-Cloudbasis werden dagegen über den Service Fabric-Ressourcenanbieter verwaltet und aktualisiert.

## <a name="steps-to-create-the-standalone-cluster"></a>Schritte zum Erstellen des eigenständigen Clusters
1. Melden Sie sich am Azure-Portal an und erstellen Sie eine neue Windows Server 2012 R2 Datacenter- oder Windows Server 2016 Datacenter-VM in einer Ressourcengruppe. Weitere Details finden Sie im Artikel [Erstellen eines virtuellen Windows-Computers im Azure-Portal](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
2. Fügen Sie weitere VMs zur selben Ressourcengruppe hinzu. Stellen Sie sicher, dass alle VMs bei der Erstellung den gleichen Administratorbenutzernamen mit dem entsprechenden Kennwort erhalten. Nach der Erstellung sollten alle drei VMs in demselben virtuellen Netzwerk angezeigt werden.
3. Stellen Sie eine Verbindung mit den einzelnen VMs her, und deaktivieren Sie die Windows-Firewall über den [Server-Manager mit dem Dashboard „Lokaler Server“](https://technet.microsoft.com/library/jj134147.aspx). So wird sichergestellt, dass der Netzwerkdatenverkehr zwischen den Computern fließen kann. Rufen Sie bei einer Verbindung mit dem Computer jeweils die IP-Adresse ab, indem Sie eine Eingabeaufforderung öffnen und `ipconfig`eingeben. Sie können auch die IP-Adresse jedes Computers im Portal anzeigen, indem Sie die Ressource des virtuellen Netzwerks für die Ressourcengruppe auswählen und die Netzwerkschnittstellen, die für jeden dieser Computer erstellt wurden, überprüfen.
4. Stellen Sie eine Verbindung mit einer der VMs her, und testen Sie, ob die anderen beiden VMs per Ping erreichbar sind.
5. Stellen Sie eine Verbindung mit einer der VMs her, [laden Sie das eigenständige Service Fabric-Paket für Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) in einen neuen Ordner auf dem Computer herunter, und extrahieren Sie das Paket.
6. Öffnen Sie die Datei *ClusterConfig.Unsecure.MultiMachine.json* im Editor, und bearbeiten Sie jeden Knoten mit den drei IP-Adressen auf den Computern. Ändern Sie oben den Clusternamen, und speichern Sie die Datei.  Ein Teilbeispiel des Clustermanifests ist unten dargestellt.
   
    ```
    {
        "name": "SampleCluster",
        "clusterConfigurationVersion": "1.0.0",
        "apiVersion": "01-2017",
        "nodes": [
        {
            "nodeName": "standalonenode0",
            "iPAddress": "10.1.0.4",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc1/r0",
            "upgradeDomain": "UD0"
        },
        {
            "nodeName": "standalonenode1",
            "iPAddress": "10.1.0.5",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc2/r0",
            "upgradeDomain": "UD1"
        },
        {
            "nodeName": "standalonenode2",
            "iPAddress": "10.1.0.6",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc3/r0",
            "upgradeDomain": "UD2"
        }
        ],
    ```
7. Wenn dieser Cluster sicher sein soll, wählen Sie die Sicherheitsmaßnahme aus, die verwendet werden soll, und befolgen Sie die Schritte unter folgenden Links: [X.509-Zertifikat](service-fabric-windows-cluster-x509-security.md) oder [Windows-Sicherheit](service-fabric-windows-cluster-windows-security.md). Wenn Cluster mit Windows-Sicherheit eingerichtet werden, müssen Sie einen Domänencontroller zur Verwaltung von Active Directory einrichten. Beachten Sie, dass die Verwendung eines Domänencontrollercomputers als Service Fabric-Knoten nicht unterstützt wird.
8. Öffnen Sie ein [PowerShell ISE-Fenster](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/introducing-the-windows-powershell-ise). Navigieren Sie zu dem Ordner, in dem Sie das heruntergeladene eigenständige Installer-Paket extrahiert und die Clusterkonfigurationsdatei gespeichert haben. Führen Sie den folgenden PowerShell-Befehl zur Bereitstellung des Clusters aus:
   
    ```
    .\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json
    ```

Das Skript konfiguriert das Service Fabric-Cluster remote und sollte den Bereitstellungsstatus melden.

9. Nach ca. einer Minute können Sie überprüfen, ob der Cluster betriebsbereit ist, indem Sie eine Verbindung mit dem Service Fabric Explorer über eine der IP-Adressen des Computers herstellen, z.B. `http://10.1.0.5:19080/Explorer/index.html`. 

## <a name="next-steps"></a>Nächste Schritte
* [Erstellen von eigenständigen Service Fabric-Clustern unter Windows Server oder Linux](service-fabric-deploy-anywhere.md)
* [Hinzufügen oder Entfernen von Knoten für einen eigenständigen Service Fabric-Cluster unter Windows Server](service-fabric-cluster-windows-server-add-remove-nodes.md)
* [Konfigurationseinstellungen für eigenständige Windows-Cluster](service-fabric-cluster-manifest.md)
* [Schützen eines eigenständigen Windows-Clusters mithilfe von Windows-Sicherheit](service-fabric-windows-cluster-windows-security.md)
* [Schützen des eigenständigen Windows-Clusters mit Zertifikaten](service-fabric-windows-cluster-x509-security.md)


