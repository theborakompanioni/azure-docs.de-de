<properties
   pageTitle="Erstellen eines eigenständigen Clusters mit Azure-VMs, auf denen Windows ausgeführt wird | Microsoft Azure"
   description="Es wird beschrieben, wie Sie einen Azure Service Fabric-Cluster auf virtuellen Azure-Computern mit Windows Server erstellen und verwalten."
   services="service-fabric"
   documentationCenter=".net"
   authors="dsk-2015"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/05/2016"
   ms.author="dkshir;chackdan"/>



# Erstellen eines eigenständigen Service Fabric-Clusters mit drei Knoten und Azure-VMs mit Windows Server

In diesem Artikel wird beschrieben, wie Sie einen Cluster auf Windows-basierten Azure-VMs erstellen, indem Sie den eigenständigen Service Fabric-Installer für Windows Server verwenden. Dies ist ein Sonderfall von [Erstellen und Verwalten eines Windows Server-Clusters](service-fabric-cluster-creation-for-windows-server.md), bei dem die VMs [unter Windows Server ausgeführte Azure-VMs](../virtual-machines/virtual-machines-windows-hero-tutorial.md) sind. Sie erstellen aber nicht einen [Service Fabric-Cluster auf Azure-Cloudbasis](service-fabric-cluster-creation-via-portal.md). Der Unterschied besteht darin, dass der eigenständige Service Fabric-Cluster, der mit den folgenden Schritten erstellt wird, vollständig von Ihnen verwaltet wird. Die Service Fabric-Cluster auf Azure-Cloudbasis werden dagegen über den Service Fabric-Ressourcenanbieter verwaltet und aktualisiert.


## Schritte zum Erstellen des eigenständigen Clusters

1. Melden Sie sich am Azure-Portal an, und erstellen Sie eine neue Windows Server 2012 R2 Datacenter-VM in einer Ressourcengruppe. Weitere Details finden Sie im Artikel [Erstellen eines virtuellen Windows-Computers im Azure-Portal](../virtual-machines/virtual-machines-windows-hero-tutorial.md).
2. Fügen Sie derselben Ressourcengruppe einige weitere Windows Server 2012 R2 Datacenter-VMs hinzu. Stellen Sie sicher, dass alle VMs bei der Erstellung den gleichen Administratorbenutzernamen mit dem entsprechenden Kennwort erhalten. Nach der Erstellung sollten alle drei VMs in demselben virtuellen Netzwerk angezeigt werden.
3. Stellen Sie eine Verbindung mit den einzelnen VMs her, und deaktivieren Sie die Windows-Firewall über den [Server-Manager mit dem Dashboard „Lokaler Server“](https://technet.microsoft.com/library/jj134147.aspx). So wird sichergestellt, dass der Netzwerkdatenverkehr zwischen den Computern fließen kann. Rufen Sie bei einer Verbindung mit dem Computer jeweils die IP-Adresse ab, indem Sie eine Eingabeaufforderung öffnen und `ipconfig` eingeben. Alternativ dazu können Sie die IP-Adresse eines Computers auch anzeigen, wenn Sie im Azure-Portal die Virtual Network-Ressource für die Ressourcengruppe auswählen.
4. Stellen Sie eine Verbindung mit einer der VMs her, und testen Sie, ob die anderen beiden VMs per Ping erreichbar sind.
5. Stellen Sie eine Verbindung mit einer der VMs her, [laden Sie das eigenständige Service Fabric-Paket für Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) in einen neuen Ordner auf dem Computer herunter, und extrahieren Sie das Paket.
6. Öffnen Sie die Datei *ClusterConfig.Unsecure.MultiMachine.json* im Editor, und bearbeiten Sie jeden Knoten mit den drei IP-Adressen auf den Computern. Ändern Sie oben den Clusternamen, und speichern Sie die Datei. Ein Teilbeispiel des Clustermanifests ist unten dargestellt.

    ```
    {
        "name": "TestCluster",
        "clusterManifestVersion": "1.0.0",
        "apiVersion": "2015-01-01-alpha",
        "nodes": [
        {
            "nodeName": "vm0",
        	"metadata": "Replace the localhost with valid IP address or FQDN below",
            "iPAddress": "10.7.0.5",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc1/r0",
            "upgradeDomain": "UD0"
        },
        {
            "nodeName": "vm1",
        	"metadata": "Replace the localhost with valid IP address or FQDN below",
            "iPAddress": "10.7.0.4",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc2/r0",
            "upgradeDomain": "UD1"
        },
        {
            "nodeName": "vm2",
        	"metadata": "Replace the localhost with valid IP address or FQDN below",
            "iPAddress": "10.7.0.6",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc3/r0",
            "upgradeDomain": "UD2"
        }
    ],
    ```

7. Öffnen Sie ein [PowerShell ISE-Fenster](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/introducing-the-windows-powershell-ise). Navigieren Sie zu dem Ordner, in dem Sie das heruntergeladene eigenständige Installerpaket extrahiert und die Clustermanifestdatei gespeichert haben. Führen Sie den folgenden PowerShell-Befehl aus:

    ```
    .\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json -MicrosoftServiceFabricCabFilePath .\MicrosoftAzureServiceFabric.cab
    ```

8. Sie sollten verfolgen können, wie PowerShell ausgeführt, eine Verbindung mit jedem Computer hergestellt und ein Cluster erstellt wird. Nach ca. einer Minute können Sie überprüfen, ob der Cluster betriebsbereit ist, indem Sie eine Verbindung mit dem Service Fabric Explorer über eine der IP-Adressen des Computers herstellen, z.B. `http://10.7.0.5:19080/Explorer/index.html`. Da es sich um einen eigenständigen Cluster mit Azure-VMs handelt, müssen Sie [Zertifikate auf den Azure-VMs bereitstellen](service-fabric-windows-cluster-x509-security.md) oder einen Computer als [Windows Server Active Directory-Controller für die Windows-Authentifizierung](service-fabric-windows-cluster-windows-security.md) einrichten (genauso wie in der lokalen Umgebung), wenn Sie für Sicherheit sorgen möchten.


## Nächste Schritte
- [Erstellen von eigenständigen Service Fabric-Clustern unter Windows Server oder Linux](service-fabric-deploy-anywhere.md)
- [Hinzufügen oder Entfernen von Knoten für einen eigenständigen Service Fabric-Cluster unter Windows Server](service-fabric-cluster-windows-server-add-remove-nodes.md)
- [Konfigurationseinstellungen für eigenständige Windows-Cluster](service-fabric-cluster-manifest.md)
- [Schützen eines eigenständigen Windows-Clusters mithilfe von Windows-Sicherheit](service-fabric-windows-cluster-windows-security.md)
- [Schützen des eigenständigen Windows-Clusters mit Zertifikaten](service-fabric-windows-cluster-x509-security.md)

<!---HONumber=AcomDC_0810_2016-->