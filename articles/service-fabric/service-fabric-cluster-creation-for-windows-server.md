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
   ms.date="07/05/2016"
   ms.author="chackdan"/>


# Erstellen eines Windows Server-Clusters

Azure Service Fabric ermöglicht die Erstellung von Service Fabric-Clustern auf beliebigen VMs oder Computern, auf denen Windows Server ausgeführt wird. So können Sie Service Fabric-Anwendungen in jeder lokalen oder cloudbasierten Umgebung bereitstellen und ausführen, in der sich miteinander verbundene Computer befinden, auf denen Windows Server ausgeführt wird. Service Fabric stellt ein Setuppaket bereit, mit dem Sie Service Fabric-Cluster erstellen können. Es wird als eigenständiges Paket für Windows Server bezeichnet.

Dieser Artikel führt Sie durch die Schritte zum lokalen Erstellen eines Clusters mithilfe des eigenständigen Pakets für Service Fabric, aber es kann auch problemlos für jede andere Umgebung angepasst werden, z.B. andere Cloudanbieter.

>[AZURE.NOTE] Dieses eigenständige Paket für Windows Server befindet sich derzeit in der Vorschauphase und wird für Produktionsworkloads nicht unterstützt. Klicken Sie [hier](http://go.microsoft.com/fwlink/?LinkID=733084), wenn Sie jetzt eine Kopie der Lizenzbedingungen herunterladen möchten.

<a id="downloadpackage"></a>
## Herunterladen des Eigenständigen Pakets für Service Fabric


[Laden Sie das eigenständige Windows Server-Paket für Service Fabric 2012 R2 und höher herunter](http://go.microsoft.com/fwlink/?LinkId=730690). Es hat den Namen *Microsoft.Azure.ServiceFabric.WindowsServer.&lt;Version&gt;.zip*.

Im Downloadpaket finden Sie die folgenden Dateien:

|**Dateiname**|**Kurzbeschreibung**|
|-----------------------|--------------------------|
|MicrosoftAzureServiceFabric.cab|Die CAB-Datei mit den Binärdateien, die für jeden Computer im Cluster bereitgestellt werden.|
|ClusterConfig.Unsecure.DevCluster.json|Beispiel für eine Clusterkonfigurationsdatei, die die Einstellungen für einen ungeschützten Entwicklungscluster mit einem virtuellen oder physischen Computer und drei Knoten enthält, einschließlich der Informationen für jeden Knoten, der im Cluster enthalten ist. |
|ClusterConfig.Unsecure.MultiMachine.json|Beispiel für eine Clusterkonfigurationsdatei, die die Einstellungen für einen ungeschützten Cluster mit mehreren virtuellen oder physischen Computern enthält, einschließlich der Informationen für jeden Computer im Cluster.|
|ClusterConfig.Windows.DevCluster.json|Beispiel für eine Clusterkonfigurationsdatei, die alle Einstellungen für einen geschützten Entwicklungscluster mit einem virtuellen oder physischen Computer und drei Knoten enthält, einschließlich der Informationen für jeden Knoten, der im Cluster enthalten ist. Der Cluster wird mithilfe von [Windows-Identitäten](https://msdn.microsoft.com/library/ff649396.aspx) geschützt.|
|ClusterConfig.Windows.MultiMachine.json|Beispiel für eine Clusterkonfigurationsdatei, die alle Einstellungen für einen sicheren Cluster mit mehreren virtuellen oder physischen Computern und Windows-Sicherheit enthält, einschließlich der Informationen für jeden Computer, der Teil des geschützten Clusters ist. Der Cluster wird mithilfe von [Windows-Identitäten](https://msdn.microsoft.com/library/ff649396.aspx) geschützt.|
|ClusterConfig.x509.DevCluster.json|Beispiel für eine Clusterkonfigurationsdatei, die alle Einstellungen für einen geschützten Entwicklungscluster mit einem virtuellen oder physischen Computer und drei Knoten enthält, einschließlich der Informationen für jeden Knoten im Cluster. Der Cluster wird mittels X.509-Zertifikaten geschützt.|
|ClusterConfig.x509.MultiMachine.json|Beispiel für eine Clusterkonfigurationsdatei, die alle Einstellungen für den geschützten Cluster mit mehreren virtuellen oder physischen Computern enthält, einschließlich der Informationen für jeden Knoten im geschützten Cluster. Der Cluster wird mittels X.509-Zertifikaten geschützt.|
|EULA.txt|Die Lizenzbedingungen für die Verwendung des eigenständigen Windows Server-Pakets für Microsoft Azure Service Fabric. [Klicken Sie hier](http://go.microsoft.com/fwlink/?LinkID=733084), wenn Sie jetzt eine Kopie der Lizenzbedingungen herunterladen möchten.|
|Readme.txt|Link zu Versionshinweisen und grundlegenden Installationsanweisungen. Dies ist eine Teilmenge der Anweisungen, die Sie auf dieser Seite finden.|
|CreateServiceFabricCluster.ps1|PowerShell-Skript, das den Cluster mit den Einstellungen in der Datei „ClusterConfig.json“ erstellt.|
|RemoveServiceFabricCluster.ps1|PowerShell-Skript, das einen Cluster mit den Einstellungen in der Datei „ClusterConfig.json“ entfernt.|
|AddNode.ps1|PowerShell-Skript zum Hinzufügen eines Knotens zu einem vorhandenen bereitgestellten Cluster.|
|RemoveNode.ps1|PowerShell-Skript zum Entfernen eines Knotens aus einem vorhandenen bereitgestellten Cluster.|


## Planen und Vorbereiten der Clusterbereitstellung
Die folgenden Schritte müssen ausgeführt werden, bevor Sie den Cluster erstellen.

### Schritt 1: Planen der Clusterinfrastruktur
Da Sie im Begriff sind, auf Ihren Computern einen Service Fabric-Cluster zu erstellen, stehen Sie vor der Entscheidung, welche Arten von Fehlern der Cluster überstehen soll. Benötigen Sie beispielsweise separate Stromversorgungsleitungen oder Internetverbindungen zur Versorgung dieser Computer? Darüber hinaus sollten Sie auch die physische Sicherheit dieser Computer berücksichtigen. Wo ist der physische Standort, und wer muss darauf zugreifen? Sobald Sie diese Entscheidungen treffen, ordnen Sie die Computer logisch den verschiedenen Fehlerdomänen zu (weitere Informationen siehe unten). Die Infrastrukturplanung für Produktionscluster ist komplizierter als für die Testcluster.

### Schritt 2: Vorbereiten der Computer, um die Voraussetzungen zu erfüllen
Voraussetzungen für jeden Computer, den Sie dem Cluster hinzufügen möchten:

- Mindestens 2 GB RAM empfohlen
- Netzwerkverbindung: Stellen Sie sicher, dass die Computer sich in einem bzw. mehreren sicheren Netzwerken befinden.
- Windows Server 2012 R2 oder Windows Server 2012 (hierfür müssen Sie KB2858668 installiert haben).
- .NET Framework 4.5.1 oder höher, vollständig installiert
- Windows PowerShell 3.0
- Der Clusteradministrator, der den Cluster bereitstellt und konfiguriert, muss auf jedem Computer Administratorrechte besitzen.
- Der „RemoteRegistry“-Dienst muss auf allen Computern ausgeführt werden.

### Schritt 3: Bestimmen der anfänglichen Clustergröße
Auf jedem Knoten wurde die Service Fabric-Laufzeit bereitgestellt, und jeder Knoten ist Mitglied des Clusters. In einer gängigen Produktionsbereitstellung ist ein Knoten pro Betriebssysteminstanz (physisch oder virtuell) vorhanden. Die Clustergröße wird durch Ihre geschäftlichen Anforderungen bestimmt; Sie benötigen jedoch eine minimale Clustergröße von drei Knoten (Computer/VMs). Beachten Sie, dass Sie für Entwicklungszwecke über mehrere Knoten auf einem bestimmten Computer verfügen können. In einer Produktionsumgebung unterstützt Service Fabric nur einen Knoten pro physischen oder virtuellen Computer.

### Schritt 4: Bestimmen der Anzahl von Fehlerdomänen und Upgradedomänen
Eine **Fehlerdomäne (FD)** ist eine physische Fehlereinheit, die in direktem Zusammenhang mit der physischen Infrastruktur von Rechenzentren steht. Eine Fehlerdomäne besteht aus Hardwarekomponenten (Computer, Switches, Netzwerk usw.), die sich eine einzelne Fehlerquelle (Single Point of Failure) teilen. Obwohl es keine 1:1-Zuordnung zwischen Fehlerdomänen und Racks gibt, kann praktisch jedes Rack als Fehlerdomäne betrachtet werden. Bei der Berücksichtigung der Knoten in Ihrem Cluster sollten Sie die Knoten unbedingt auf mindestens drei Fehlerdomänen verteilen.

Beim Angeben von FDs in *ClusterConfig.json* können Sie jeweils den Namen der FD wählen. Service Fabric unterstützt hierarchische FDs, damit sie Ihre Infrastrukturtopologie reflektieren können. Beispielsweise sind die folgenden Fehlerdomänen zulässig:

- „faultDomain“: „fd:/Room1/Rack1/Machine1“
- „faultDomain“: „fd:/FD1“
- „faultDomain“: „fd:/Room1/Rack1/PDU1/M1“


Eine **Upgradedomäne (UD)** ist eine logische Einheit von Knoten. Während eines orchestrierten Service Fabric-Upgrades (Anwendungsupgrade oder Clusterupgrade) werden alle Knoten in einer Upgradedomäne außer Betrieb genommen, um das Upgrade auszuführen, während Knoten in anderen UDs zur Erfüllung von Anforderungen zur Verfügung stehen. Die Firmwareupgrades, die Sie auf Ihren Computern ausführen, berücksichtigen UDs nicht, darum können Sie sie nur jeweils auf einem Computer ausführen und nicht auf allen gleichzeitig.

Die einfachste Möglichkeit, sich diese Konzepte vorzustellen, besteht darin, FDs als Einheit des ungeplanten Ausfalls und UDs als Einheit der geplanten Wartung zu betrachten.

Beim Angeben von UDs in *ClusterConfig.json* können Sie jeweils den Namen der UD wählen. Beispielsweise ist Folgendes zulässig:

- „upgradeDomain“: „UD0“
- „upgradeDomain“: „UD1A“
- „upgradeDomain“: „DomainRed“
- „upgradeDomain“: „Blue“

Ausführlichere Informationen zu Upgrade- und Fehlerdomänen finden Sie im Artikel [Beschreiben eines Service Fabric-Clusters](service-fabric-cluster-resource-manager-cluster-description.md).

### Schritt 5: Herunterladen des eigenständigen Service Fabric-Pakets für Windows Server
[Laden Sie das eigenständige Service Fabric-Paket für Windows Server herunter](http://go.microsoft.com/fwlink/?LinkId=730690), und entpacken Sie es entweder auf einem Bereitstellungscomputer, der nicht Teil des Clusters ist, oder auf einem der Computer, die Teil des Clusters sein werden.

<a id="createcluster"></a>
## Erstellen Ihres Clusters

Nachdem Sie die oben im Planungs- und Vorbereitungsabschnitt beschriebenen Schritte durchlaufen haben, können Sie jetzt Ihren Cluster erstellen.

### Schritt 1: Ändern der Clusterkonfiguration
Der Cluster wird in der Datei *ClusterConfig.json* beschrieben. Ausführliche Informationen zu den Abschnitten in dieser Datei finden Sie im Artikel [Konfigurationseinstellungen für eigenständige Windows-Cluster](service-fabric-cluster-manifest.md). Öffnen Sie eine der *ClusterConfig.json*-Dateien aus dem Paket, das Sie heruntergeladen haben, und ändern Sie die folgenden Einstellungen:

|**Konfigurationseinstellung**|**Beschreibung**|
|-----------------------|--------------------------|
|**NodeTypes**|Mit Knotentypen können Sie die Clusterknoten in verschiedene Gruppen unterteilen. Ein Cluster muss über mindestens einen Knotentyp verfügen. Alle Knoten in einer Gruppe haben die folgenden gemeinsamen Merkmale. <br> **Name**: Dies ist der Knotentypname. <br>**Endpoints Ports**: Verschiedene benannte Endpunkte (Ports), die diesem Knotentyp zugeordnet sind. Sie können eine beliebige Portnummer verwenden, solange sie nicht mit anderen Elementen in diesem Manifest in Konflikt steht und nicht bereits von einer anderen Anwendung verwendet wird, die auf dem physischen oder virtuellen Computer ausgeführt wird. <br> ** Placement Properties**: Eigenschaften für diesen Knotentyp, die Sie später als Platzierungseinschränkungen für die Systemdienste oder eigene Dienste verwenden können. Diese Eigenschaften sind benutzerdefinierte Schlüssel-Wert-Paare, mit denen für einen bestimmten Knoten zusätzliche Metadaten bereitgestellt werden. Beispiele für Knoteneigenschaften: Vorhandensein einer Festplatte oder Grafikkarte am Knoten, Anzahl von Spindeln des Festplattenlaufwerks, Kerne und andere physische Eigenschaften. <br> **Capacities**: Knotenkapazitäten definieren Name und Menge einer bestimmten Ressource, die ein bestimmter Knoten nutzen kann. Ein Knoten definiert z. B., dass er über Kapazität für eine Metrik mit dem Namen „MemoryInMb“ verfügt und standardmäßig 2.048 MB an verfügbarem Arbeitsspeicher aufweist. Anhand dieser Kapazitäten wird zur Laufzeit sichergestellt, dass Dienste, die bestimmte Mengen von Ressourcen benötigen, so auf Knoten angeordnet werden, dass diese Ressourcen verfügbar bleiben.<br>**IsPrimary**: Wenn Sie mehr als ein NodeType-Element definiert haben, sollten Sie sicherstellen, dass nur ein Element mit dem Wert *true* als primäres Element festgelegt ist. Auf diesem Element werden die Systemdienste ausgeführt. Alle anderen Knotentypen sollten auf den Wert *false* festgelegt werden.|
|**Nodes**|Die Details für jeden Knoten, der Teil des Clusters ist (Knotentyp, Knotenname, IP-Adresse, Fehlerdomäne und Upgradedomäne des Knotens). Die Computer, aus denen Sie den Cluster erstellen möchten, müssen mit ihren IP-Adressen hier aufgeführt werden. <br> Wenn Sie die gleichen IP-Adressen für alle Knoten verwenden, wird ein Cluster mit einem Computer erstellt, den Sie für Tests verwenden können. Cluster mit einem Computer dürfen nicht zur Bereitstellung von Produktionsworkloads verwendet werden.|

### Schritt 2: Ausführen des Clustererstellungsskripts
Sobald Sie die Clusterkonfiguration im JSON-Dokument geändert und alle Knoteninformationen hinzugefügt haben, führen Sie das PowerShell-Skript *CreateServiceFabricCluster.ps1* zur Erstellung des Clusters aus dem Paketordner heraus aus, und übergeben Sie ihm den Pfad zur JSON-Konfigurationsdatei und zum Speicherort der CAB-Paketdatei.

Dieses Skript kann auf jedem Computer ausgeführt werden, der Administratorzugriffsrechte auf alle Computer hat, die als Knoten in der Clusterkonfigurationsdatei aufgeführt sind. Der Computer, auf dem dieses Skript ausgeführt wird, kann, muss jedoch nicht Teil des Clusters sein.

```
#Create an unsecure local development cluster

.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json -MicrosoftServiceFabricCabFilePath .\MicrosoftAzureServiceFabric.cab -AcceptEULA $true
```
```
#Create an unsecure multi-machine cluster

.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json -MicrosoftServiceFabricCabFilePath .\MicrosoftAzureServiceFabric.cab -AcceptEULA $true
```

>[AZURE.NOTE] Die Bereitstellungsprotokolle sind lokal auf dem virtuellen bzw. physischen Computer verfügbar, auf dem Sie den PowerShell-Befehl „CreateServiceFabricCluster“ ausgeführt haben. Sie befinden sich im Unterordner „DeploymentTraces“ des Ordners, in dem Sie den PowerShell-Befehl ausgeführt haben. Um zu ermitteln, ob Service Fabric auf einem Computer richtig bereitgestellt wurde, können Sie die installierten Dateien im Verzeichnis „C:\\ProgramData“ überprüfen. Die Ausführung der Prozesse „FabricHost.exe“ und „Fabric.exe“ können Sie im Task-Manager verfolgen.

### Schritt 3: Verbinden mit dem Cluster
Jetzt können Sie über Service Fabric Explorer eine Verbindung mit dem Cluster herstellen, und zwar entweder direkt von einem der Computer mit http://localhost:19080/Explorer/index.html oder per Remotezugriff mit „http://<*IPAddressofaMachine*>:19080/Explorer/index.html“.

## Hinzufügen von Knoten zum Cluster

1. Bereiten Sie den virtuellen bzw. physischen Computer vor, den Sie dem Cluster hinzufügen möchten (siehe Schritt 2 im vorherigen Abschnitt „Planen und Vorbereiten der Clusterbereitstellung“).
2. Planen Sie, welcher Fehlerdomäne und Upgradedomäne Sie diesen virtuellen bzw. physischen Computer hinzufügen werden.
3. Kopieren oder [laden Sie das eigenständige Paket für Service Fabric für Windows Server herunter](http://go.microsoft.com/fwlink/?LinkId=730690), und entpacken Sie das Paket auf dem virtuellen bzw. physischen Computer, den Sie dem Cluster hinzufügen möchten.
4. Öffnen Sie eine PowerShell-Administratoreingabeaufforderung, und navigieren Sie zum Speicherort des extrahierten Pakets.
5. Führen Sie das PowerShell-Skript *AddNode.ps1* mit den Parametern aus, die den hinzuzufügenden neuen Knoten beschreiben. Im folgenden Beispiel wird ein neuer Knoten mit dem Namen VM5, dem Typ NodeType0 und der IP-Adresse 182.17.34.52 für UD1 und FD1 hinzugefügt. *ExistingClusterConnectionEndPoint* ist ein Verbindungsendpunkt für einen Knoten, der im Cluster bereits vorhanden ist. Sie können hierfür eine *beliebige* Knoten-IP-Adresse im Cluster wählen.

```
.\AddNode.ps1 -MicrosoftServiceFabricCabFilePath .\MicrosoftAzureServiceFabric.cab -NodeName VM5 -NodeType NodeType0 -NodeIPAddressorFQDN 182.17.34.52 -ExistingClusterConnectionEndPoint 182.17.34.50:19000 -UpgradeDomain UD1 -FaultDomain FD1
```

## Entfernen von Knoten aus dem Cluster

1. Stellen Sie eine Remotedesktopverbindung mit dem virtuellen bzw. physischen Computer her, den Sie aus dem Cluster entfernen möchten.
2. Kopieren oder [laden Sie das eigenständige Paket für Service Fabric für Windows Server herunter](http://go.microsoft.com/fwlink/?LinkId=730690), und entpacken Sie das Paket auf dem virtuellen bzw. physischen Computer, den Sie dem Cluster hinzufügen möchten.
3. Öffnen Sie eine PowerShell-Administratoreingabeaufforderung, und navigieren Sie zum Speicherort des extrahierten Pakets.
4. Führen Sie das PowerShell-Skript *RemoveNode.ps1* aus. Im folgenden Beispiel wird der aktuelle Knoten aus dem Cluster entfernt. *ExistingClusterConnectionEndPoint* ist ein Verbindungsendpunkt für einen Knoten, der im Cluster bereits vorhanden ist. Sie können hierfür eine *beliebige* Knoten-IP-Adresse im Cluster wählen.

```
.\RemoveNode.ps1 -MicrosoftServiceFabricCabFilePath .\MicrosoftAzureServiceFabric.cab -ExistingClusterConnectionEndPoint 182.17.34.50:19000
```

## Entfernen des Clusters
Führen Sie zum Entfernen eines Clusters das PowerShell-Skript *RemoveServiceFabricCluster.ps1* aus dem Paketordner aus, und übergeben Sie den Pfad zur JSON-Konfigurationsdatei und den Speicherort der CAB-Paketdatei.

Dieses Skript kann auf jedem Computer ausgeführt werden, der Administratorzugriffsrechte auf alle Computer hat, die als Knoten in der Clusterkonfigurationsdatei aufgeführt sind. Der Computer, auf dem dieses Skript ausgeführt wird, kann Teil des Clusters sein, aber dies ist nicht zwingend erforderlich.

```
.\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json   -MicrosoftServiceFabricCabFilePath .\MicrosoftAzureServiceFabric.cab
```

## Gewusst wie: Erstellen eines Clusters mit drei Knoten mit Azure IaaS-VMs
In den folgenden Schritten wird beschrieben, wie Sie einen Cluster auf Azure IaaS-VMs erstellen, indem Sie das Installationsprogramm für die eigenständige Windows Server-Bereitstellung verwenden. Beachten Sie, dass die Service Fabric-Laufzeit in diesem IaaS-Cluster vollständig von Ihnen verwaltet wird. Dies unterscheidet sich von Clustern, die über das Azure-Portal erstellt und vom Service Fabric-Ressourcenanbieter aktualisiert werden.

1. Melden Sie sich am Azure-Portal an, und erstellen Sie eine neue Windows Server 2012 R2 Datacenter-VM in einer Ressourcengruppe.
2. Fügen Sie derselben Ressourcengruppe zwei weitere Windows Server 2012 R2 Datacenter-VMs hinzu. Stellen Sie sicher, dass alle VMs bei der Erstellung den gleichen Administratorbenutzernamen mit dem entsprechenden Kennwort erhalten. Nach der Erstellung sollten alle drei VMs in demselben virtuellen Netzwerk angezeigt werden.
3. Stellen Sie eine Verbindung mit den einzelnen VMs her, und deaktivieren Sie die Windows-Firewall über den Server-Manager mit dem Dashboard „Lokaler Server“. So wird sichergestellt, dass der Netzwerkdatenverkehr zwischen den Computern fließen kann. Rufen Sie auf jedem Computer jeweils die IP-Adresse ab, indem Sie eine Eingabeaufforderung öffnen und *ipconfig* eingeben. Alternativ dazu können Sie die IP-Adresse eines Computers auch anzeigen, wenn Sie im Azure-Portal die Virtual Network-Ressource für die Ressourcengruppe auswählen.
4. Stellen Sie eine Verbindung mit einem der Computer her, und testen Sie, ob die anderen beiden Computer per Ping erreichbar sind.
5. Stellen Sie eine Verbindung mit einer der VMs her, und laden Sie das Paket für die eigenständige Windows Server-Bereitstellung in einen neuen Ordner auf dem Computer herunter, und entzippen Sie das Paket.
6. Öffnen Sie die Datei *ClusterConfig.Unsecure.MultiMachine.json* im Editor, und bearbeiten Sie jeden Knoten mit den drei IP-Adressen auf den Computern, ändern Sie den Clusternamen, und speichern Sie die Datei. Ein Teilbeispiel für das Clustermanifest mit den IP-Adressen für jeden Computer ist unten zu sehen.

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

7. Öffnen Sie ein PowerShell ISE-Fenster, und navigieren Sie zu dem Ordner, in den Sie das Paket für die eigenständige Installation heruntergeladen und die oben angegebene Manifestdatei gespeichert haben. Führen Sie den folgenden PowerShell-Befehl aus:

    ```
    .\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json -MicrosoftServiceFabricCabFilePath .\MicrosoftAzureServiceFabric.cab
    ```

8. Sie sollten verfolgen können, wie PowerShell ausgeführt, eine Verbindung mit jedem Computer hergestellt und ein Cluster erstellt wird. Nach ca. einer Minute können Sie ausprobieren, ob der Cluster betriebsbereit ist, indem Sie eine Verbindung mit dem Service Fabric Explorer über eine der IP-Adressen der Computer herstellen, z.B. http://10.7.0.5:19080/Explorer/index.html. Da es sich um einen eigenständigen Cluster auf IaaS-VMs handelt, müssen Sie Zertifikate auf den VMs bereitstellen oder einen Computer als Windows Server Active Directory-Controller für die Windows-Authentifizierung einrichten (genauso wie in der lokalen Umgebung), wenn Sie für Sicherheit sorgen möchten. Unter „Nächste Schritte“ finden Sie weitere Informationen zum Einrichten von sicheren Clustern.

## Nächste Schritte
- [Erstellen von eigenständigen Service Fabric-Clustern unter Windows Server oder Linux](service-fabric-deploy-anywhere.md)
- [Konfigurationseinstellungen für eigenständige Windows-Cluster](service-fabric-cluster-manifest.md)

- [Schützen eines eigenständigen Windows-Clusters mithilfe von Windows-Sicherheit](service-fabric-windows-cluster-windows-security.md)
- [Schützen des eigenständigen Windows-Clusters mit Zertifikaten](service-fabric-windows-cluster-x509-security.md)

<!---HONumber=AcomDC_0713_2016-->