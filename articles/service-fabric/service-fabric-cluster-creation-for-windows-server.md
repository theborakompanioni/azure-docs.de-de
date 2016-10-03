<properties
   pageTitle="Erstellen und Verwalten eines eigenständigen Azure Service Fabric-Clusters | Microsoft Azure"
   description="Hier erfahren Sie, wie Sie auf einem beliebigen Computer (physisch oder virtuell) mit Windows Server lokal oder in einer Cloud einen Azure Service Fabric-Cluster erstellen und verwalten."
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
   ms.date="09/15/2016"
   ms.author="dkshir;chackdan"/>


# Erstellen und Verwalten eines Windows Server-Clusters

Azure Service Fabric ermöglicht die Erstellung von Service Fabric-Clustern auf beliebigen VMs oder Computern, auf denen Windows Server ausgeführt wird. So können Sie Service Fabric-Anwendungen in jeder Umgebung bereitstellen und ausführen, in der sich miteinander verbundene Windows Server-Computer befinden – lokal oder bei einem Cloudanbieter. Service Fabric stellt ein Setuppaket zum Erstellen von Service Fabric-Clustern bereit. Es wird als eigenständiges Paket für Windows Server bezeichnet.

Dieser Artikel führt Sie durch die Schritte zum lokalen Erstellen eines Clusters mithilfe des eigenständigen Pakets für Service Fabric, aber es kann auch problemlos für jede andere Umgebung angepasst werden, z.B. andere Cloudanbieter.

>[AZURE.NOTE] Dieses eigenständige Paket für Windows Server und seine Features befinden sich derzeit in der Vorschauphase und werden deshalb nicht für die kommerzielle Nutzung unterstützt. Am Ende des Dokuments finden Sie eine Liste der Features in der Vorschauphase. [Klicken Sie hier](http://go.microsoft.com/fwlink/?LinkID=733084), wenn Sie jetzt eine Kopie der Lizenzbedingungen herunterladen möchten.

<a id="downloadpackage"></a>
## Herunterladen des Eigenständigen Pakets für Service Fabric


[Laden Sie das eigenständige Windows Server-Paket für Service Fabric 2012 R2 und höher herunter](http://go.microsoft.com/fwlink/?LinkId=730690). Es hat den Namen *Microsoft.Azure.ServiceFabric.WindowsServer.&lt;Version&gt;.zip*.

>[AZURE.NOTE] Bei Verwenden der Browser Internet Explorer oder Microsoft Edge zum Herunterladen dieses Pakets müssen Sie die Website [http://download.microsoft.com](http://download.microsoft.com) den vertrauenswürdigen Websites im Intranet hinzufügen. Dies verhindert, dass das „Zones“-Tag in die Dateien geschrieben wird, wenn die ZIP-Datei heruntergeladen wird.

 ![TrustedZone][TrustedZone]

Im Downloadpaket finden Sie die folgenden Dateien:

|**Dateiname**|**Kurzbeschreibung**|
|-----------------------|--------------------------|
|MicrosoftAzureServiceFabric.cab|Die CAB-Datei mit den Binärdateien, die für jeden Computer im Cluster bereitgestellt werden.|
|ClusterConfig.Unsecure.DevCluster.json|Beispiel für eine Clusterkonfigurationsdatei, die die Einstellungen für einen ungeschützten Entwicklungscluster mit einem virtuellen oder physischen Computer und drei Knoten enthält, einschließlich der Informationen für jeden Knoten im Cluster. |
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
Führen Sie die folgenden Schritte aus, bevor Sie den Cluster erstellen.

### Schritt 1: Planen der Clusterinfrastruktur
Da Sie im Begriff sind, auf Ihren Computern einen Service Fabric-Cluster zu erstellen, können Sie entscheiden, welche Arten von Fehlern der Cluster überstehen soll. Benötigen Sie beispielsweise separate Stromversorgungsleitungen oder Internetverbindungen zur Versorgung dieser Computer? Berücksichtigen Sie darüber hinaus auch die physische Sicherheit der Computer. Wo ist der physische Standort, und wer muss darauf zugreifen? Nachdem Sie diese Entscheidungen getroffen haben, können Sie die Computer den verschiedenen Fehlerdomänen logisch zuordnen (weiter unten finden Sie weitere Informationen). Die Infrastrukturplanung für Produktionscluster ist komplizierter als für die Testcluster.

<a id="preparemachines"></a>
### Schritt 2: Vorbereiten der Computer, um die Voraussetzungen zu erfüllen
Voraussetzungen für jeden Computer, den Sie dem Cluster hinzufügen möchten:

- Mindestens 4 GB RAM empfohlen
- Netzwerkkonnektivität: Stellen Sie sicher, dass die Computer sich in einem bzw. mehreren sicheren Netzwerken befinden.
- Windows Server 2012 R2 oder Windows Server 2012 ([KB2858668](https://support.microsoft.com/kb/2858668) muss installiert sein).
- [.NET Framework 4.5.1 oder höher](https://www.microsoft.com/download/details.aspx?id=40773), vollständig installiert.
- [Windows PowerShell 3.0](https://msdn.microsoft.com/powershell/scripting/setup/installing-windows-powershell).
- Der Clusteradministrator, der den Cluster bereitstellt und konfiguriert, muss auf jedem Computer [Administratorrechte](https://social.technet.microsoft.com/wiki/contents/articles/13436.windows-server-2012-how-to-add-an-account-to-a-local-administrator-group.aspx) besitzen.
- Der [„RemoteRegistry“-Dienst](https://technet.microsoft.com/library/cc754820) muss auf allen Computern ausgeführt werden.

### Schritt 3: Bestimmen der anfänglichen Clustergröße
Für jeden Knoten in einem eigenständigen Service Fabric-Cluster wird die Service Fabric-Laufzeit bereitgestellt und ist Mitglied des Clusters. In einer gängigen Produktionsbereitstellung ist ein Knoten pro Betriebssysteminstanz (physisch oder virtuell) vorhanden. Die Clustergröße wird durch Ihre geschäftlichen Anforderungen bestimmt; Sie benötigen jedoch eine minimale Clustergröße von drei Knoten (Computer/VMs). Für Entwicklungszwecke können Sie über mehrere Knoten auf einem bestimmten Computer verfügen. In einer Produktionsumgebung unterstützt Service Fabric nur einen Knoten pro physischen oder virtuellen Computer.

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

Nachdem Sie die oben im Planungs- und Vorbereitungsabschnitt beschriebenen Schritte durchlaufen haben, können Sie Ihren Cluster erstellen.

### Schritt 1: Ändern der Clusterkonfiguration
Der Cluster wird in der Datei *ClusterConfig.json* beschrieben. Ausführliche Informationen zu den Abschnitten in dieser Datei finden Sie im Artikel [Konfigurationseinstellungen für eigenständige Windows-Cluster](service-fabric-cluster-manifest.md). Öffnen Sie eine der *ClusterConfig.json*-Dateien aus dem Paket, das Sie heruntergeladen haben, und ändern Sie die folgenden Einstellungen:

|**Konfigurationseinstellung**|**Beschreibung**|
|-----------------------|--------------------------|
|**NodeTypes**|Mit Knotentypen können Sie die Clusterknoten in verschiedene Gruppen unterteilen. Ein Cluster muss über mindestens einen Knotentyp verfügen. Alle Knoten in einer Gruppe haben die folgenden gemeinsamen Merkmale: <br> **Name**: Dies ist der Knotentypname. <br>**Endpoint Ports**: Verschiedene benannte Endpunkte (Ports), die diesem Knotentyp zugeordnet sind. Sie können eine beliebige Portnummer verwenden, solange sie nicht mit anderen Elementen in diesem Manifest in Konflikt steht und nicht bereits von einem anderen Programm auf dem (virtuellen) Computer verwendet wird. <br> **Platzierungseigenschaften**: Eigenschaften für diesen Knotentyp, die Sie später als Platzierungseinschränkungen für Systemdienste oder eigene Dienste verwenden. Diese Eigenschaften sind benutzerdefinierte Schlüssel-Wert-Paare, mit denen für einen bestimmten Knoten zusätzliche Metadaten bereitgestellt werden. Beispiele für Knoteneigenschaften: Vorhandensein einer Festplatte oder Grafikkarte am Knoten, Anzahl von Spindeln des Festplattenlaufwerks, Kerne und andere physische Eigenschaften. <br> **Capacities**: Knotenkapazitäten definieren Name und Menge einer bestimmten Ressource, die ein bestimmter Knoten nutzen kann. Ein Knoten definiert z. B., dass er über Kapazität für eine Metrik mit dem Namen „MemoryInMb“ verfügt und standardmäßig 2.048 MB an verfügbarem Arbeitsspeicher aufweist. Anhand dieser Kapazitäten wird zur Laufzeit sichergestellt, dass Dienste, die bestimmte Mengen von Ressourcen benötigen, auf den Knoten angeordnet werden, die in ausreichender Menge über diese Ressourcen verfügen.<br>**IsPrimary**: Wenn Sie mehr als ein NodeType-Element definiert haben, sollten Sie sicherstellen, dass nur ein Element mit dem Wert *true* als primäres Element festgelegt ist. Auf diesem Element werden die Systemdienste ausgeführt. Alle anderen Knotentypen sollten auf den Wert *false* festgelegt werden.|
|**Nodes**|Dies sind die Details für jeden Knoten, der Teil des Clusters ist (Knotentyp, Knotenname, IP-Adresse, Fehlerdomäne und Upgradedomäne des Knotens). Die Computer, aus denen Sie den Cluster erstellen möchten, müssen mit ihren IP-Adressen hier aufgeführt werden. <br> Wenn Sie die gleiche IP-Adresse für alle Knoten verwenden, wird ein Cluster mit einem Computer erstellt, den Sie für Tests verwenden können. Cluster mit einem Computer dürfen nicht zur Bereitstellung von Produktionsworkloads verwendet werden.|

### Schritt 2: Ausführen des Clustererstellungsskripts
Sobald Sie die Clusterkonfiguration im JSON-Dokument geändert und alle Knoteninformationen hinzugefügt haben, führen Sie das PowerShell-Skript *CreateServiceFabricCluster.ps1* zur Erstellung des Clusters im Paketordner aus. Übergeben Sie an diesen den Pfad zur JSON-Konfigurationsdatei, und akzeptieren Sie die Lizenzbedingungen.

Dieses Skript kann auf jedem Computer ausgeführt werden, der Administratorzugriffsrechte auf alle Computer hat, die als Knoten in der Clusterkonfigurationsdatei aufgeführt sind. Der Computer, auf dem dieses Skript ausgeführt wird, kann, muss jedoch nicht Teil des Clusters sein.

```
#Create an unsecured local development cluster

.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json -AcceptEULA true
```
```
#Create an unsecured multi-machine cluster

.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json -AcceptEULA true
```

>[AZURE.NOTE] Die Bereitstellungsprotokolle sind lokal auf dem virtuellen bzw. physischen Computer verfügbar, auf dem Sie den PowerShell-Befehl „CreateServiceFabricCluster“ ausgeführt haben. Sie befinden sich im Unterordner „DeploymentTraces“ des Ordners, in dem Sie den PowerShell-Befehl ausgeführt haben. Um zu ermitteln, ob Service Fabric auf einem Computer richtig bereitgestellt wurde, können Sie die installierten Dateien im Verzeichnis „C:\\ProgramData“ überprüfen. Die Ausführung der Prozesse „FabricHost.exe“ und „Fabric.exe“ können Sie im Task-Manager verfolgen.

### Schritt 3: Verbinden mit dem Cluster

In [diesem Dokument](service-fabric-connect-to-secure-cluster.md) finden Sie Anweisungen zum Herstellen einer Verbindung mit einem sicheren Cluster.

Führen Sie den folgenden PowerShell-Befehl aus, um eine Verbindung mit einem unsicheren Cluster herzustellen:

```powershell

Connect-ServiceFabricCluster -ConnectionEndpoint <*IPAddressofaMachine*>:<Client connection end point port>

Connect-ServiceFabricCluster -ConnectionEndpoint 192.13.123.2345:19000

```
### Schritt 4: Öffnen von Service Fabric Explorer

Jetzt können Sie über Service Fabric Explorer eine Verbindung mit dem Cluster herstellen, und zwar entweder direkt von einem der Computer mit http://localhost:19080/Explorer/index.html oder per Remotezugriff mit „http://<*IPAddressofaMachine*>:19080/Explorer/index.html“.



## Hinzufügen und Entfernen von Knoten des Clusters

Wenn sich Ihre geschäftlichen Anforderungen ändern, können Sie dem eigenständigen Service Fabric-Cluster Knoten hinzufügen oder Knoten daraus entfernen. Ausführliche Schritte finden Sie unter [Hinzufügen oder Entfernen von Knoten für einen eigenständigen Service Fabric-Cluster](service-fabric-cluster-windows-server-add-remove-nodes.md).


## Entfernen des Clusters

Führen Sie zum Entfernen eines Clusters das PowerShell-Skript *RemoveServiceFabricCluster.ps1* im Paketordner aus, übergeben Sie den Pfad zur JSON-Konfigurationsdatei, und geben Sie optional einen Speicherort für das Löschprotokoll an.

Dieses Skript kann auf jedem Computer ausgeführt werden, der Administratorzugriffsrechte auf alle Computer hat, die als Knoten in der Clusterkonfigurationsdatei aufgeführt sind. Der Computer, auf dem dieses Skript ausgeführt wird, kann Teil des Clusters sein, aber dies ist nicht zwingend erforderlich.

```
.\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json   
```

## Features in der Vorschauphase in diesem Paket

Das gesamte Paket befindet sich derzeit in der Vorschauphase.

## Nächste Schritte
- [Konfigurationseinstellungen für eigenständige Windows-Cluster](service-fabric-cluster-manifest.md)
- [Hinzufügen oder Entfernen von Knoten für einen eigenständigen Service Fabric-Cluster unter Windows Server](service-fabric-cluster-windows-server-add-remove-nodes.md)
- [Erstellen eines eigenständigen Service Fabric-Clusters mit drei Knoten und Azure-VMs mit Windows Server](service-fabric-cluster-creation-with-windows-azure-vms.md)
- [Schützen eines eigenständigen Windows-Clusters mithilfe von Windows-Sicherheit](service-fabric-windows-cluster-windows-security.md)
- [Schützen des eigenständigen Windows-Clusters mit Zertifikaten](service-fabric-windows-cluster-x509-security.md)


<!--Image references-->
[TrustedZone]: ./media/service-fabric-cluster-creation-for-windows-server/TrustedZone.png

<!---HONumber=AcomDC_0921_2016-->