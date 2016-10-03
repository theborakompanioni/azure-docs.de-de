<properties
   pageTitle="Hinzufügen oder Entfernen von Knoten für einen eigenständigen Service Fabric-Cluster | Microsoft Azure"
   description="Enthält Informationen zum Hinzufügen oder Entfernen von Knoten für einen Azure Service Fabric-Cluster auf einem physischen oder virtuellen Computer mit Windows Server, der lokal oder in einer Cloud angeordnet sein kann."
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
   ms.date="09/20/2016"
   ms.author="dkshir;chackdan"/>


# Hinzufügen oder Entfernen von Knoten für einen eigenständigen Service Fabric-Cluster unter Windows Server

Nachdem Sie Ihren [eigenständigen Service Fabric-Cluster auf Windows Server-Computern erstellt haben](service-fabric-cluster-creation-for-windows-server.md), können sich Ihre geschäftlichen Anforderungen ändern, sodass Sie Ihrem Cluster ggf. mehrere Knoten hinzufügen bzw. diese daraus entfernen müssen. Dieser Artikel enthält die ausführlichen Schritte, die hierfür erforderlich sind.


## Hinzufügen von Knoten zum Cluster

1. Bereiten Sie den (virtuellen) Computer vor, den Sie dem Cluster hinzufügen möchten, indem Sie die Schritte im Abschnitt [Vorbereiten der Computer zur Erfüllung der Voraussetzungen für die Clusterbereitstellung](service-fabric-cluster-creation-for-windows-server.md#preparemachines) ausführen.
2. Planen Sie, welcher Fehlerdomäne und Upgradedomäne Sie diesen virtuellen bzw. physischen Computer hinzufügen werden.
3. Stellen Sie eine Remotedesktopverbindung mit dem virtuellen bzw. physischen Computer her, den Sie dem Cluster hinzufügen möchten.
4. [Laden Sie das eigenständige Paket für Service Fabric für Windows Server auf den (virtuellen) Computer herunter](http://go.microsoft.com/fwlink/?LinkId=730690) (bzw. kopieren Sie es), und entzippen Sie das Paket.
5. Führen Sie PowerShell als Administrator aus, und navigieren Sie zum Speicherort des extrahierten Pakets.
6. Führen Sie das PowerShell-Skript *AddNode.ps1* mit den Parametern aus, die den hinzuzufügenden neuen Knoten beschreiben. Im folgenden Beispiel wird ein neuer Knoten mit dem Namen VM5, dem Typ NodeType0 und der IP-Adresse 182.17.34.52 für UD1 und FD1 hinzugefügt. *ExistingClusterConnectionEndPoint* ist ein Verbindungsendpunkt für einen Knoten, der im Cluster bereits vorhanden ist. Für diesen Endpunkt können Sie die IP-Adresse von *jedem* Knoten im Cluster auswählen.

```
.\AddNode.ps1 -NodeName VM5 -NodeType NodeType0 -NodeIPAddressorFQDN 182.17.34.52 -ExistingClusterConnectionEndPoint 182.17.34.50:19000 -UpgradeDomain UD1 -FaultDomain FD1 -AcceptEULA true
```

## Entfernen von Knoten aus dem Cluster

1. Stellen Sie eine Remotedesktopverbindung mit dem virtuellen bzw. physischen Computer her, den Sie aus dem Cluster entfernen möchten.
2. Kopieren oder [laden Sie das eigenständige Paket für Service Fabric für Windows Server herunter](http://go.microsoft.com/fwlink/?LinkId=730690), und entpacken Sie das Paket auf diesem virtuellen bzw. physischen Computer.
3. Führen Sie PowerShell als Administrator aus, und navigieren Sie zum Speicherort des extrahierten Pakets.
4. Führen Sie das PowerShell-Skript *RemoveNode.ps1* aus. Im folgenden Beispiel wird der aktuelle Knoten aus dem Cluster entfernt. *ExistingClusterConnectionEndPoint* ist ein Verbindungsendpunkt für einen Knoten, der im Cluster bereits vorhanden ist. Für diesen Endpunkt können Sie die IP-Adresse von *jedem* Knoten im Cluster auswählen.

```
.\RemoveNode.ps1 -ExistingClusterConnectionEndPoint 182.17.34.50:19000
```


## Nächste Schritte
- [Konfigurationseinstellungen für eigenständige Windows-Cluster](service-fabric-cluster-manifest.md)
- [Schützen eines eigenständigen Windows-Clusters mithilfe von Windows-Sicherheit](service-fabric-windows-cluster-windows-security.md)
- [Schützen des eigenständigen Windows-Clusters mit Zertifikaten](service-fabric-windows-cluster-x509-security.md)
- [Erstellen eines eigenständigen Service Fabric-Clusters mit drei Knoten und Azure-VMs mit Windows Server](service-fabric-cluster-creation-with-windows-azure-vms.md)

<!---HONumber=AcomDC_0921_2016-->