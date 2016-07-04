<properties
   pageTitle="Erstellen eines Azure Service Fabric-Clusters – lokal oder in einer Cloud | Microsoft Azure"
   description="Hier erfahren Sie, wie Sie auf einem beliebigen Computer (physisch oder virtuell) mit Windows Server lokal oder in einer Cloud einen Azure Service Fabric-Cluster erstellen."
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="06/21/2016"
   ms.author="chackdan"/>


# Erstellen eines Azure Service Fabric-Clusters – lokal oder in der Cloud

Azure Service Fabric ermöglicht die Erstellung von Service Fabric-Clustern auf beliebigen VMs oder Computern, auf denen Windows Server ausgeführt wird. So können Sie Service Fabric-Anwendungen in jeder lokalen oder cloudbasierten Umgebung bereitstellen und ausführen, in der sich miteinander verbundene Computer befinden, auf denen Windows Server ausgeführt wird. Service Fabric bietet ein Setuppaket zum Erstellen solcher Service Fabric-Cluster.

Dieser Artikel führt Sie durch die Schritte zum lokalen Erstellen eines Clusters mithilfe des eigenständigen Pakets für Service Fabric, doch es kann mühelos für jede andere Umgebung angepasst werden, z. B. andere Clouds.

>[AZURE.NOTE] Dieses eigenständige Angebot befindet sich derzeit in der Vorschauphase. Klicken Sie [hier](http://go.microsoft.com/fwlink/?LinkID=733084), wenn Sie jetzt eine Kopie der Lizenzbedingungen herunterladen möchten.

<a id="downloadpackage"></a>
## Herunterladen des Eigenständigen Pakets für Service Fabric


[Laden Sie das eigenständige Paket für Service Fabric für Windows Server 2012 R2 herunter](http://go.microsoft.com/fwlink/?LinkId=730690), das den Namen *Microsoft.Azure.ServiceFabric.WindowsServer.&lt;version&gt;.zip* besitzt.

Im Downloadpaket finden Sie die folgenden Dateien:

|**Dateiname**|**Kurzbeschreibung**|
|-----------------------|--------------------------|
|MicrosoftAzureServiceFabric.cab|Die CAB-Datei, die die Binärdateien enthält, die für jeden Computer im Cluster bereitgestellt werden.|
|ClusterConfig.Unsecure.DevCluster.JSON|Beispiel einer Clusterkonfigurationsdatei, die alle Einstellungen für einen unsicheren Entwicklungscluster mit einem virtuellen oder physischen Computer und drei Knoten enthält, einschließlich der Informationen für jeden Knoten, der Teil des Clusters ist. |
|ClusterConfig.Unsecure.MultiMachine.JSON|Beispiel einer Clusterkonfigurationsdatei, die alle Einstellungen für den Cluster enthält, einschließlich der Informationen für jeden Computer, der Teil eines unsicheren Clusters mit mehreren virtuellen oder physischen Computern ist.|
|ClusterConfig.Windows.DevCluster.JSON|Beispiel einer Clusterkonfigurationsdatei, die alle Einstellungen für einen sicheren Entwicklungscluster mit einem virtuellen oder physischen Computer und drei Knoten enthält, einschließlich der Informationen für jeden Knoten, der Teil des Clusters ist. Der Cluster wird mittels [Windows-Identitäten](https://msdn.microsoft.com/library/ff649396.aspx) geschützt.|
|ClusterConfig.Windows.MultiMachine.JSON|Beispiel einer Clusterkonfigurationsdatei, die alle Einstellungen für den sicheren Cluster enthält, einschließlich der Informationen für jeden Computer, der Teil eines sicheren Clusters mit mehreren virtuellen oder physischen Computern ist. Der Cluster wird mittels [Windows-Identitäten](https://msdn.microsoft.com/library/ff649396.aspx) geschützt.|
|ClusterConfig.x509.DevCluster.JSON|Beispiel einer Clusterkonfigurationsdatei, die alle Einstellungen für einen sicheren Entwicklungscluster mit einem virtuellen oder physischen Computer und drei Knoten enthält, einschließlich der Informationen für jeden Knoten, der Teil des Clusters ist. Der Cluster wird mittels X.509-Windows-Zertifikaten geschützt.|
|ClusterConfig.x509.MultiMachine.JSON|Beispiel einer Clusterkonfigurationsdatei, die alle Einstellungen für den sicheren Cluster enthält, einschließlich der Informationen für jeden Computer, der Teil eines sicheren Clusters mit mehreren virtuellen oder physischen Computern ist. Der Cluster wird mittels X.509-Zertifikaten geschützt.|
|EULA.txt|Die Lizenzbedingungen für die Verwendung des eigenständigen Pakets für Microsoft Azure Service Fabric. [Klicken Sie hier](http://go.microsoft.com/fwlink/?LinkID=733084), wenn Sie jetzt eine Kopie der Lizenzbedingungen herunterladen möchten.|
|Readme.txt|Link zu Versionshinweisen und grundlegenden Installationsanweisungen. Es ist ein kleiner Teil der Anweisungen, die Sie auf dieser Seite finden.|
|CreateServiceFabricCluster.ps1|PowerShell-Skript, das den Cluster mit den Einstellungen in der Datei „ClusterConfig.JSON“ erstellt.|
|RemoveServiceFabricCluster.ps1|PowerShell-Skript zur Clusterentfernung, das die Einstellungen in „ClusterConfig.JSON“ nutzt.|
|AddNode.ps1|PowerShell-Skript zum Hinzufügen eines Knotens zu einem vorhandenen Cluster.|
|RemoveNode.ps1|PowerShell-Skript zum Entfernen eines Knotens aus einem Cluster.|


## Planen und Vorbereiten der Clusterbereitstellung
Die folgenden Schritte müssen ausgeführt werden, bevor Sie den Cluster erstellen.

### Schritt 1: Planen der Clusterinfrastruktur
Da Sie im Begriff sind, auf Ihren Computern einen Service Fabric-Cluster zu erstellen, stehen Sie vor der Entscheidung, welche Arten von Fehlern der Cluster überstehen soll. Benötigen Sie beispielsweise separate Stromversorgungsleitungen oder Internetverbindungen zur Versorgung dieser Computer? Darüber hinaus sollten Sie auch die physische Sicherheit dieser Computer berücksichtigen. Wo ist der physische Standort, und wer muss darauf zugreifen? Sobald Sie diese Entscheidungen treffen, ordnen Sie die Computer logisch den verschiedenen Fehlerdomänen zu (weitere Informationen siehe unten). Die Infrastrukturplanung für Produktionscluster ist viel komplizierter als für die Testcluster.

### Schritt 2: Vorbereiten der Computer, um die Voraussetzungen zu erfüllen
Voraussetzungen für jeden Computer, den Sie dem Cluster hinzufügen möchten:

- Mindestens 2 GB RAM empfohlen
- Netzwerkkonnektivität – Stellen Sie sicher, dass die Computer sich in einem sicheren Netzwerk/in sicheren Netzwerken befinden.
- Windows Server 2012 R2 oder Windows Server 2012 (hierfür müssen Sie KB2858668 installiert haben).
- .NET Framework 4.5.1 oder höher, vollständig installiert
- Windows PowerShell 3.0
- Der Clusteradministrator, der den Cluster bereitstellt und konfiguriert, muss auf jedem Computer Administratorrechte besitzen.
- Der „RemoteRegistry“-Dienst muss auf allen Computern ausgeführt werden.

### Schritt 3: Bestimmen der anfänglichen Clustergröße
Jeder Knoten besteht aus einem vollständigen Service Fabric-Stapel und ist ein einzelnes Element des Service Fabric-Clusters. In einer normalen Service Fabric-Bereitstellung ist ein Knoten pro BS-Instanz (physisch oder virtuell) vorhanden. Die Clustergröße wird durch Ihre geschäftlichen Anforderungen bestimmt; Sie benötigen jedoch eine minimale Clustergröße von drei Knoten (Computer/VMs). Beachten Sie, dass Sie für Entwicklungszwecke über mehrere Knoten auf einem bestimmten Computer verfügen können. In einer Produktionsumgebung unterstützt Service Fabric nur einen Knoten pro physischen oder virtuellen Computer.

### Schritt 4: Bestimmen der Anzahl von Fehlerdomänen und Upgradedomänen
Eine **Fehlerdomäne (FD)** ist eine physische Fehlereinheit, die in direktem Zusammenhang mit der physischen Infrastruktur von Rechenzentren steht. Eine Fehlerdomäne besteht aus Hardwarekomponenten (Computer, Switches usw.), die sich einen Single Point of Failure teilen. Obwohl es keine 1:1-Zuordnung zwischen Fehlerdomänen und Racks gibt, kann praktisch jedes Rack als Fehlerdomäne betrachtet werden. Bei der Berücksichtigung der Knoten in Ihrem Cluster sollten Sie die Knoten unbedingt auf mindestens drei Fehlerdomänen verteilen.

Beim Angeben von FDs in *ClusterConfig.JSON* können Sie den Namen der FD wählen. Service Fabric unterstützt hierarchische FDs, damit sie Ihre Infrastrukturtopologie reflektieren können. Beispielsweise sind die folgenden zulässig:

- „faultDomain“: „fd:/Room1/Rack1/Machine1“
- „faultDomain“: „fd:/FD1“
- „faultDomain“: „fd:/Room1/Rack1/PDU1/M1“


Eine **Upgradedomäne (UD)** ist eine logische Einheit von Knoten. Während eines orchestrierten Service Fabric-Upgrades (Anwendungsupgrade oder Clusterupgrade) werden alle Knoten in einer Upgradedomäne außer Betrieb genommen, um das Upgrade auszuführen, während Knoten in anderen UDs zur Erfüllung von Anforderungen zur Verfügung stehen. Die Firmwareupgrades, die Sie auf Ihren Computern ausführen, berücksichtigen UDs nicht, darum können Sie sie nur jeweils auf einem Computer ausführen und nicht auf allen gleichzeitig.

Die einfachste Möglichkeit, sich diese Konzepte vorzustellen, besteht darin, FDs als Einheit des ungeplanten Ausfalls und UDs als Einheit der geplanten Wartung zu betrachten.

Beim Angeben von UDs in *ClusterConfig.JSON** können Sie den Namen der UD wählen. Beispielsweise sind die folgenden zulässig:

- „upgradeDomain“: „UD0“
- „upgradeDomain“: „UD1A“
- „upgradeDomain“: „DomainRed“
- „upgradeDomain“: „Blue“

### Schritt 5: Herunterladen des eigenständigen Pakets für Service Fabric für Windows Server
[Laden Sie das eigenständige Paket für Service Fabric für Windows Server herunter](http://go.microsoft.com/fwlink/?LinkId=730690), und entpacken Sie es entweder auf einem Bereitstellungscomputer, der nicht Teil des Clusters ist, oder auf einem der Computer, die Teil des Clusters sein werden.

<a id="createcluster"></a>
## Erstellen Ihres Clusters

Nachdem Sie die oben im Planungs- und Vorbereitungsabschnitt beschriebenen Schritte durchlaufen haben, können Sie jetzt Ihren Cluster erstellen.

### Schritt 1: Ändern der Clusterkonfiguration
Öffnen Sie *ClusterConfig.JSON* aus dem Paket, das Sie heruntergeladen haben. Sie können in einem Editor Ihrer Wahl die folgenden Einstellungen ändern:

|**Konfigurationseinstellung**|**Beschreibung**|
|-----------------------|--------------------------|
|NodeTypes|Mit Knotentypen können Sie die Clusterknoten in verschiedene Gruppen unterteilen. Ein Cluster muss über mindestens einen Knotentyp verfügen. Alle Knoten in einer Gruppe haben die folgenden gemeinsamen Merkmale. <br> *Name*: Der Name des Knotentyps. <br>*EndPoints*: Verschiedene benannte Endpunkte (Ports), die diesem Knotentyp zugeordnet sind. Sie können eine beliebige Portnummer verwenden, solange sie nicht mit anderen Elementen in diesem Manifest in Konflikt steht und nicht bereits von einem anderen Programm auf dem (virtuellen) Computer verwendet wird. <br> *PlacementProperties*: Eigenschaften für diesen Knotentyp, die Sie später als Platzierungseinschränkungen für Systemdienste oder eigene Dienste verwenden. Diese Eigenschaften sind benutzerdefinierte Schlüssel-Wert-Paare, mit denen für einen bestimmten Knoten zusätzliche Metadaten bereitgestellt werden. Beispiele für Knoteneigenschaften: Vorhandensein einer Festplatte oder Grafikkarte am Knoten, Spindelanzahl des Festplattenlaufwerks, Kerne und andere physische Eigenschaften. <br> *Capacities*: Knotenkapazitäten definieren Name und Menge einer bestimmten Ressource, die ein bestimmter Knoten nutzen kann. Ein Knoten definiert z. B., dass er über Kapazität für eine Metrik mit dem Namen „MemoryInMb“ verfügt und standardmäßig 2.048 MB an verfügbarem Arbeitsspeicher aufweist. Diese Kapazitäten werden zur Laufzeit verwendet, um sicherzustellen, dass Dienste, die bestimmte Ressourcenmengen beanspruchen, auf Knoten mit den verfügbaren Ressourcen platziert werden.|
|Nodes|Die Details für jeden Knoten, der Teil des Clusters ist (Knotentyp, Knotenname, IP-Adresse, Fehlerdomäne und Upgradedomäne des Knotens). Die Computer, aus denen Sie den Cluster erstellen möchten, müssen mit ihren IP-Adressen hier aufgeführt werden. <br> Wenn Sie die gleichen IP-Adressen für alle Knoten verwenden, wird ein Cluster mit einem Computer erstellt, den Sie für Tests verwenden können. Cluster mit einem Computer dürfen nicht zur Bereitstellung von Produktionsworkloads verwendet werden.|

### Schritt 2: Ausführen des Clustererstellungsskripts
Sobald Sie die Clusterkonfiguration im JSON-Dokument geändert und alle Knoteninformationen hinzugefügt haben, führen Sie das PowerShell-Skript zur Erstellung des Clusters aus dem Paketordner heraus aus, und übergeben Sie ihm den Pfad zur Konfigurationsdatei und zum Speicherort des Stammverzeichnisses des Pakets.

Dieses Skript kann auf jedem Computer ausgeführt werden, der Administratorzugriffsrechte auf alle Computer hat, die als Knoten in der Clusterkonfigurationsdatei aufgeführt sind. Der Computer, auf dem dieses Skript ausgeführt wird, kann, muss jedoch nicht Teil des Clusters sein.

```
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.JSON -MicrosoftServiceFabricCabFilePath .\MicrosoftAzureServiceFabric.cab
```

>[AZURE.NOTE] Die Bereitstellungsprotokolle sind lokal auf dem virtuellen bzw. physischen Computer verfügbar, auf dem Sie den PowerShell-Befehl „CreateServiceFabricCluster“ ausgeführt haben. Sie befinden sich in einen Unterordner namens „DeploymentTraces“ im Ordner, in dem Sie den PS-Befehl ausgeführt haben.

## Hinzufügen von Knoten zum Service Fabric-Cluster 

1. Bereiten Sie den virtuellen bzw. physischen Computer vor, den Sie dem Cluster hinzufügen möchten (siehe Schritt 2 im vorherigen Abschnitt „Planen und Vorbereiten der Clusterbereitstellung“). 
2. Planen Sie, welcher Fehlerdomäne und Upgradedomäne Sie diesen virtuellen bzw. physischen Computer hinzufügen werden.
3. [Laden Sie das eigenständige Paket für Service Fabric für Windows Server herunter](http://go.microsoft.com/fwlink/?LinkId=730690), und entpacken Sie das Paket entweder auf dem virtuellen oder dem physischen Computer, den Sie dem Cluster hinzufügen möchten. 
4. Öffnen Sie eine PowerShell-Administratoreingabeaufforderung, und navigieren Sie zum Speicherort des extrahierten Pakets.
5. Führen Sie „AddNode.ps1“ aus.

```
.\AddNode.ps1 -MicrosoftServiceFabricCabFilePath .\MicrosoftAzureServiceFabric.cab -NodeName VM5 -NodeType NodeType0 -NodeIPAddressorFQDN 182.17.34.52 -ExistingClusterConnectionEndPoint 182.17.34.52:19000 -UpgradeDomain UD1 -FaultDomain FD1
```

## Entfernen von Knoten aus Ihrem Service Fabric-Cluster 

1. Verbinden Sie sich per Terminaldienste mit dem virtuellen bzw. physischen Computer, den Sie aus dem Cluster entfernen möchten.
2. Öffnen Sie eine PowerShell-Administratoreingabeaufforderung, und navigieren Sie zum Speicherort des extrahierten Pakets.
5. Führen Sie „RemoveNode.ps1“ aus.

```
.\RemoveNode.ps1 -MicrosoftServiceFabricCabFilePath .\MicrosoftAzureServiceFabric.cab -ExistingClusterConnectionEndPoint 182.17.34.52:19000
```

## Löschen Ihres Service Fabric-Clusters 
1. Verbinden Sie sich per Terminaldienste mit dem virtuellen bzw. physischen Computer, der Teil des Clusters ist.
2. Öffnen Sie eine PowerShell-Administratoreingabeaufforderung, und navigieren Sie zum Speicherort des extrahierten Pakets.
5. Führen Sie „RemoveNode.ps1“ aus.

```
.\RemoveNode.ps1 -MicrosoftServiceFabricCabFilePath .\MicrosoftAzureServiceFabric.cab -ExistingClusterConnectionEndPoint 182.17.34.52:19000
```


## Nächste Schritte
- [Grundlegendes zur Clustersicherheit](service-fabric-cluster-security.md)
- [ Service Fabric-SDK und erste Schritte](service-fabric-get-started.md)
- [Verwalten von Service Fabric-Anwendungen in Visual Studio](service-fabric-manage-application-in-visual-studio.md)
- [Umgebungsunabhängige Bereitstellung (Deploy Anywhere) unter Windows Server und Linux mit Service Fabric](service-fabric-deploy-anywhere.md)

<!---HONumber=AcomDC_0622_2016-->