---
title: "Erstellen und Verwalten eines eigenständigen Azure Service Fabric-Clusters | Microsoft Docs"
description: Erstellen und verwalten Sie einen Azure Service Fabric-Cluster auf einem beliebigen Computer (physisch oder virtuell) mit Windows Server, lokal oder in einer Cloud.
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: 
ms.assetid: 31349169-de19-4be6-8742-ca20ac41eb9e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/06/2016
ms.author: dkshir;chackdan
translationtype: Human Translation
ms.sourcegitcommit: 6db229794c90e985de834bd3bfb6e0b030de2df5
ms.openlocfilehash: 0cb59a2e94a9c985cb56d9dd20c05e2e22a45151


---
# <a name="create-and-manage-a-cluster-running-on-windows-server"></a>Erstellen und Verwalten eines Windows Server-Clusters
Mithilfe von Azure Service Fabric können Sie Service Fabric-Cluster auf beliebigen virtuellen oder physischen Computern erstellen, auf denen Windows Server ausgeführt wird. So können Sie Service Fabric-Anwendungen in jeder Umgebung bereitstellen und ausführen, in der sich miteinander verbundene Windows Server-Computer befinden – lokal oder bei einem Cloudanbieter. Service Fabric stellt ein Setuppaket zum Erstellen von Service Fabric-Clustern bereit. Es wird als eigenständiges Paket für Windows Server bezeichnet.

Dieser Artikel führt Sie durch die Schritte zum lokalen Erstellen eines Clusters mithilfe des eigenständigen Pakets für Service Fabric, aber es kann auch problemlos für jede andere Umgebung angepasst werden, z.B. andere Cloudanbieter.

> [!NOTE]
> Dieses eigenständige Windows Server-Paket ist im Handel erhältlich und für Produktionsbereitstellungen geeignet. Das Paket enthält unter Umständen neue Service Fabric-Features, die sich in der Vorschauphase befinden. Scrollen Sie nach unten zum Abschnitt „Features in der Vorschauphase in diesem Paket“, um zur Liste mit den Vorschaufeatures zu gelangen. Sie können jetzt [eine Kopie des Endbenutzer-Lizenzvertrags herunterladen](http://go.microsoft.com/fwlink/?LinkID=733084).
> 
> 

<a id="getsupport"></a>

## <a name="get-support-for-the-service-fabric-standalone-package"></a>Support für das eigenständige Service Fabric-Paket
* Befragen Sie im [Azure Service Fabric-Forum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=AzureServiceFabric?) die Community zum eigenständigen Service Fabric-Paket für Windows Server.
* Erstellen Sie ein Ticket für [Professional Support für Service Fabric](http://support.microsoft.com/oas/default.aspx?prid=16146).  Weitere Informationen zu Professional Support von Microsoft finden Sie [hier](https://support.microsoft.com/en-us/gp/offerprophone?wa=wsignin1.0).
* Unterstützung für dieses Paket wird auch im Rahmen von [Microsoft Premier Support](https://support.microsoft.com/en-us/premier) bereitgestellt. 

<a id="downloadpackage"></a>

## <a name="download-the-service-fabric-standalone-package"></a>Herunterladen des Eigenständigen Pakets für Service Fabric
[Laden Sie das eigenständige Paket für Service Fabric für Windows Server 2012 R2 und höher herunter](http://go.microsoft.com/fwlink/?LinkId=730690), das „Microsoft.Azure.ServiceFabric.WindowsServer.&lt;version&gt;.zip“ heißt.

Im Downloadpaket finden Sie die folgenden Dateien:

| **Dateiname** | **Kurzbeschreibung** |
| --- | --- |
| MicrosoftAzureServiceFabric.cab |Die CAB-Datei mit den Binärdateien, die für jeden Computer im Cluster bereitgestellt werden. |
| ClusterConfig.Unsecure.DevCluster.json |Beispiel für eine Clusterkonfigurationsdatei, die die Einstellungen für einen ungeschützten Entwicklungscluster mit einem einzelnen virtuellen oder physischen Computer und drei Knoten enthält, einschließlich der Informationen für jeden Knoten im Cluster. |
| ClusterConfig.Unsecure.MultiMachine.json |Beispiel für eine Clusterkonfigurationsdatei, die die Einstellungen für einen ungeschützten Cluster mit mehreren virtuellen oder physischen Computern enthält, einschließlich der Informationen für jeden Computer im Cluster. |
| ClusterConfig.Windows.DevCluster.json |Beispiel für eine Clusterkonfigurationsdatei, die alle Einstellungen für einen geschützten Entwicklungscluster mit einem einzelnen virtuellen oder physischen Computer und drei Knoten enthält, einschließlich der Informationen für jeden Knoten im Cluster. Der Cluster wird mithilfe von [Windows-Identitäten](https://msdn.microsoft.com/library/ff649396.aspx) geschützt. |
| ClusterConfig.Windows.MultiMachine.json |Beispiel für eine Clusterkonfigurationsdatei, die alle Einstellungen für einen sicheren Cluster mit mehreren virtuellen oder physischen Computern und Windows-Sicherheit enthält, einschließlich der Informationen für jeden Computer, der Teil des geschützten Clusters ist. Der Cluster wird mithilfe von [Windows-Identitäten](https://msdn.microsoft.com/library/ff649396.aspx) geschützt. |
| ClusterConfig.x509.DevCluster.json |Beispiel für eine Clusterkonfigurationsdatei, die alle Einstellungen für einen geschützten Entwicklungscluster mit einem einzelnen virtuellen oder physischen Computer und drei Knoten enthält, einschließlich der Informationen für jeden Knoten im Cluster. Der Cluster wird mittels X.509-Zertifikaten geschützt. |
| ClusterConfig.x509.MultiMachine.json |Beispiel für eine Clusterkonfigurationsdatei, die alle Einstellungen für den geschützten Cluster mit mehreren virtuellen oder physischen Computern enthält, einschließlich der Informationen für jeden Knoten im geschützten Cluster. Der Cluster wird mittels X.509-Zertifikaten geschützt. |
| EULA_ENU.txt |Die Lizenzbedingungen für die Verwendung des eigenständigen Windows Server-Pakets für Microsoft Azure Service Fabric. Sie können jetzt [eine Kopie des Endbenutzer-Lizenzvertrags herunterladen](http://go.microsoft.com/fwlink/?LinkID=733084). |
| Readme.txt |Link zu Versionshinweisen und grundlegenden Installationsanweisungen. Dies ist eine Teilmenge der Anweisungen in diesem Dokument. |
| CreateServiceFabricCluster.ps1 |Ein PowerShell-Skript, das den Cluster mit den Einstellungen in „ClusterConfig.json“ erstellt |
| RemoveServiceFabricCluster.ps1 |Ein PowerShell-Skript, das einen Cluster mit den Einstellungen in „ClusterConfig.json“ entfernt |
| ThirdPartyNotice.rtf |Hinweis zu im Paket enthaltener Drittanbietersoftware. |
| AddNode.ps1 |PowerShell-Skript zum Hinzufügen eines Knotens zu einem vorhandenen, bereitgestellten Cluster. |
| RemoveNode.ps1 |PowerShell-Skript zum Entfernen eines Knotens aus einem vorhandenen, bereitgestellten Cluster. |
| CleanFabric.ps1 |PowerShell-Skript zum Entfernen einer eigenständigen Service Fabric-Installation vom aktuellen Computer. Vorherige MSI-Installationen müssen jeweils mit ihrem dazugehörigen Deinstallationsprogramm entfernt werden. |
| TestConfiguration.ps1 |PowerShell-Skript zum Analysieren der Infrastruktur auf der Grundlage von „cluster.json“. |

## <a name="plan-and-prepare-your-cluster-deployment"></a>Planen und Vorbereiten der Clusterbereitstellung
Führen Sie die folgenden Schritte aus, bevor Sie den Cluster erstellen.

### <a name="step-1-plan-your-cluster-infrastructure"></a>Schritt 1: Planen der Clusterinfrastruktur
Da Sie im Begriff sind, auf Ihren Computern einen Service Fabric-Cluster zu erstellen, können Sie entscheiden, welche Arten von Fehlern der Cluster überstehen soll. Benötigen Sie beispielsweise separate Stromversorgungsleitungen oder Internetverbindungen zur Versorgung dieser Computer? Berücksichtigen Sie darüber hinaus auch die physische Sicherheit der Computer. Wo befinden sich die Computer, und wer benötigt Zugang zu ihnen? Nachdem Sie diese Entscheidungen getroffen haben, können Sie die Computer logisch den verschiedenen Fehlerdomänen zuordnen (siehe Schritt 4). Die Infrastrukturplanung für Produktionscluster ist komplizierter als für Testcluster.

<a id="preparemachines"></a>

### <a name="step-2-prepare-the-machines-to-meet-the-prerequisites"></a>Schritt 2: Vorbereiten der Computer, um die Voraussetzungen zu erfüllen
Voraussetzungen für jeden Computer, den Sie dem Cluster hinzufügen möchten:

* Mindestens 16 GB RAM empfohlen
* Mindestens 40 GB verfügbarer Speicherplatz empfohlen
* CPU mit mindestens vier Kernen empfohlen
* Verbindung mit einem sicheren Netzwerk oder Netzwerken für alle Computer
* Windows Server 2012 R2 oder Windows Server 2012 ( [KB2858668](https://support.microsoft.com/kb/2858668) muss installiert sein).
* [.NET Framework 4.5.1 oder höher](https://www.microsoft.com/download/details.aspx?id=40773), vollständig installiert.
* [Windows PowerShell 3.0](https://msdn.microsoft.com/powershell/scripting/setup/installing-windows-powershell).
* Der [„RemoteRegistry“-Dienst](https://technet.microsoft.com/library/cc754820) muss auf allen Computern ausgeführt werden.

Der Clusteradministrator, der den Cluster bereitstellt und konfiguriert, muss auf jedem Computer [Administratorrechte](https://social.technet.microsoft.com/wiki/contents/articles/13436.windows-server-2012-how-to-add-an-account-to-a-local-administrator-group.aspx) besitzen. Service Fabric kann nicht auf einem Domänencontroller installiert werden.

### <a name="step-3-determine-the-initial-cluster-size"></a>Schritt 3: Bestimmen der anfänglichen Clustergröße
Für jeden Knoten in einem eigenständigen Service Fabric-Cluster wird die Service Fabric-Laufzeit bereitgestellt und ist Mitglied des Clusters. In einer gängigen Produktionsbereitstellung ist ein Knoten pro Betriebssysteminstanz (physisch oder virtuell) vorhanden. Die Clustergröße wird durch Ihre Geschäftsanforderungen bestimmt. Sie benötigen jedoch eine minimale Clustergröße von drei Knoten (auf physischen oder virtuellen Computern).
Für Entwicklungszwecke können Sie über mehrere Knoten auf einem bestimmten Computer verfügen. In einer Produktionsumgebung unterstützt Service Fabric nur einen Knoten pro physischen oder virtuellen Computer.

### <a name="step-4-determine-the-number-of-fault-domains-and-upgrade-domains"></a>Schritt 4: Bestimmen der Anzahl von Fehlerdomänen und Upgradedomänen
Eine *Fehlerdomäne* (fault domain; FD) ist eine physische Fehlereinheit, die in direktem Zusammenhang mit der physischen Infrastruktur von Rechenzentren steht. Eine Fehlerdomäne besteht aus Hardwarekomponenten (Computer, Switches, Netzwerken usw.), die sich eine einzelne Fehlerquelle teilen. Obwohl es keine 1:1-Zuordnung zwischen Fehlerdomänen und Racks gibt, kann praktisch jedes Rack als Fehlerdomäne betrachtet werden. Bei der Berücksichtigung der Knoten in Ihrem Cluster sollten Sie die Knoten unbedingt auf mindestens drei Fehlerdomänen verteilen.

Beim Angeben von FDs in „ClusterConfig.json“ können Sie jeweils den Namen der FD wählen. Service Fabric unterstützt hierarchische FDs, damit sie Ihre Infrastrukturtopologie reflektieren können.  Beispielsweise sind die folgenden Fehlerdomänen zulässig:

* „faultDomain“: „fd:/Room1/Rack1/Machine1“
* „faultDomain“: „fd:/FD1“
* „faultDomain“: „fd:/Room1/Rack1/PDU1/M1“

Eine *Upgradedomäne* (UD) ist eine logische Einheit von Knoten. Während eines orchestrierten Service Fabric-Upgrades (Anwendungsupgrade oder Clusterupgrade) werden alle Knoten in einer Upgradedomäne außer Betrieb genommen, um das Upgrade auszuführen, während Knoten in anderen UDs zur Erfüllung von Anforderungen zur Verfügung stehen. Die Firmwareupgrades, die Sie auf Ihren Computern ausführen, berücksichtigen UDs nicht, darum können Sie sie nur jeweils auf einem Computer ausführen und nicht auf allen gleichzeitig.

Die einfachste Möglichkeit, sich diese Konzepte vorzustellen, besteht darin, FDs als Einheit des ungeplanten Ausfalls und UDs als Einheit der geplanten Wartung zu betrachten.

Beim Angeben von UDs in „ClusterConfig.json“ können Sie jeweils den Namen der UD wählen. Beispielsweise sind die folgenden Namen zulässig:

* „upgradeDomain“: „UD0“
* „upgradeDomain“: „UD1A“
* „upgradeDomain“: „DomainRed“
* „upgradeDomain“: „Blue“

Ausführlichere Informationen zu Upgrade- und Fehlerdomänen finden Sie im Artikel [Beschreiben eines Service Fabric-Clusters](service-fabric-cluster-resource-manager-cluster-description.md).

### <a name="step-5-download-the-service-fabric-standalone-package-for-windows-server"></a>Schritt 5: Herunterladen des eigenständigen Service Fabric-Pakets für Windows Server
[Laden Sie das eigenständige Service Fabric-Paket für Windows Server herunter](http://go.microsoft.com/fwlink/?LinkId=730690) , und entpacken Sie es entweder auf einem Bereitstellungscomputer, der nicht Teil des Clusters ist, oder auf einem der Computer, die Teil des Clusters sein werden. Sie können den entzippten Ordner `Microsoft.Azure.ServiceFabric.WindowsServer` umbenennen.

<a id="createcluster"></a>

## <a name="create-your-cluster"></a>Erstellen Ihres Clusters
Nachdem Sie die Planungs- und Vorbereitungsabschnitte durchlaufen haben, können Sie Ihren Cluster erstellen.

### <a name="step-1-modify-cluster-configuration"></a>Schritt 1: Ändern der Clusterkonfiguration
Der Cluster wird in der Datei „ClusterConfig.json“ beschrieben. Ausführliche Informationen zu den Abschnitten in dieser Datei finden Sie im Artikel [Konfigurationseinstellungen für eigenständige Windows-Cluster](service-fabric-cluster-manifest.md).
Öffnen Sie eine der „ClusterConfig.json“-Dateien aus dem Paket, das Sie heruntergeladen haben, und ändern Sie die folgenden Einstellungen:

<!--Loc Comment: Please, check that line 129 the clause has been modified to "that you use as placement constraints" instead of using "you are used as placement constraints"-->

| **Konfigurationseinstellung** | **Beschreibung** |
| --- | --- |
| **NodeTypes** |Mit Knotentypen können Sie die Clusterknoten in verschiedene Gruppen unterteilen. Ein Cluster muss über mindestens einen Knotentyp verfügen. Alle Knoten in einer Gruppe haben die folgenden gemeinsamen Merkmale: <br> **Name**: Dies ist der Knotentypname. <br>**Endpunktports**: Verschiedene benannte Endpunkte (Ports), die diesem Knotentyp zugeordnet sind. Sie können eine beliebige Portnummer verwenden, solange sie nicht mit anderen Elementen in diesem Manifest in Konflikt steht und nicht bereits von einem anderen Programm auf dem (virtuellen) Computer verwendet wird. <br> **Platzierungseigenschaften**: Eigenschaften für diesen Knotentyp, die Sie später als Platzierungseinschränkungen für Systemdienste oder eigene Dienste verwenden. Diese Eigenschaften sind benutzerdefinierte Schlüssel-Wert-Paare, mit denen für einen bestimmten Knoten zusätzliche Metadaten bereitgestellt werden. Beispiele für Knoteneigenschaften wären etwa die Angabe, ob der Knoten über eine Festplatte oder Grafikkarte verfügt, die Anzahl von Spindeln des Festplattenlaufwerks, die Anzahl von Kernen und andere physische Eigenschaften. <br> **Kapazitäten**: Knotenkapazitäten definieren Name und Menge einer bestimmten Ressource, die ein bestimmter Knoten nutzen kann. Ein Knoten definiert z. B., dass er über Kapazität für eine Metrik mit dem Namen „MemoryInMb“ verfügt und standardmäßig 2.048 MB an verfügbarem Arbeitsspeicher aufweist. Diese Kapazitäten werden zur Laufzeit verwendet, um sicherzustellen, dass Dienste, die bestimmte Ressourcenmengen beanspruchen, auf Knoten mit den verfügbaren Ressourcen in der benötigten Menge platziert werden.<br>**IsPrimary**: Wenn Sie mehr als ein NodeType-Element definiert haben, sollten Sie sicherstellen, dass nur ein Element mit dem Wert *TRUE* als primäres Element festgelegt ist. Auf diesem Element werden die Systemdienste ausgeführt. Alle anderen Knotentypen sollten auf den Wert *FALSE* festgelegt werden. |
| **Nodes** |Dies sind die Details für jeden Knoten, der Teil des Clusters ist (Knotentyp, Knotenname, IP-Adresse, Fehlerdomäne und Upgradedomäne des Knotens). Die Computer, aus denen Sie den Cluster erstellen möchten, müssen mit ihren IP-Adressen hier aufgeführt werden. <br> Wenn Sie die gleiche IP-Adresse für alle Knoten verwenden, wird ein Cluster mit einem Computer erstellt, den Sie für Tests verwenden können. Cluster mit einem Computer dürfen nicht zur Bereitstellung von Produktionsworkloads verwendet werden. |

### <a name="step-2-run-the-testconfiguration-script"></a>Schritt 2: Ausführen des TestConfiguration-Skripts
Das TestConfiguration-Skript testet Ihre Infrastruktur auf der Grundlage von „cluster.json“, um unter anderem zu überprüfen, ob die erforderlichen Berechtigungen zugewiesen sind, die Computer miteinander verbunden sind, und andere Attribute definiert sind, damit die Bereitstellung erfolgreich durchgeführt werden kann. Es ist im Grunde ein einfacher Best Practices Analyzer. Das Tool wird nach und nach mit zusätzlichen Überprüfungen erweitert, um es robuster zu machen.

Dieses Skript kann auf jedem Computer ausgeführt werden, der Administratorzugriffsrechte auf alle Computer hat, die als Knoten in der Clusterkonfigurationsdatei aufgeführt sind. Der Computer, auf dem dieses Skript ausgeführt wird, muss nicht Teil des Clusters sein.

```powershell

PS C:\temp\Microsoft.Azure.ServiceFabric.WindowsServer> .\TestConfiguration.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json
Trace folder already exists. Traces will be written to existing trace folder: C:\temp\Microsoft.Azure.ServiceFabric.WindowsServer\DeploymentTraces
Running Best Practices Analyzer...
Best Practices Analyzer completed successfully.


LocalAdminPrivilege        : True
IsJsonValid                : True
IsCabValid                 : True
RequiredPortsOpen          : True
RemoteRegistryAvailable    : True
FirewallAvailable          : True
RpcCheckPassed             : True
NoConflictingInstallations : True
FabricInstallable          : True
Passed                     : True


```

### <a name="step-3-run-the-create-cluster-script"></a>Schritt 3: Ausführen des Clustererstellungsskripts
Sobald Sie die Clusterkonfiguration im JSON-Dokument geändert und alle Knoteninformationen hinzugefügt haben, führen Sie das PowerShell-Skript *CreateServiceFabricCluster.ps1* zur Erstellung des Clusters im Paketordner aus, und übergeben Sie den Pfad zur JSON-Konfigurationsdatei. Wenn dies abgeschlossen ist, akzeptieren Sie den Endbenutzer-Lizenzvertrag.

Dieses Skript kann auf jedem Computer ausgeführt werden, der Administratorzugriffsrechte auf alle Computer hat, die als Knoten in der Clusterkonfigurationsdatei aufgeführt sind. Der Computer, auf dem dieses Skript ausgeführt wird, muss nicht Teil des Clusters sein.

```
#Create an unsecured local development cluster

.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json -AcceptEULA
```
```
#Create an unsecured multi-machine cluster

.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json -AcceptEULA
```

> [!NOTE]
> Die Bereitstellungsprotokolle sind lokal auf dem virtuellen bzw. physischen Computer verfügbar, auf dem Sie den PowerShell-Befehl „CreateServiceFabricCluster“ ausgeführt haben. Sie befinden sich im Unterordner „DeploymentTraces“ des Ordners, in dem Sie den PowerShell-Befehl ausgeführt haben. Um zu ermitteln, ob Service Fabric auf einem Computer richtig bereitgestellt wurde, können Sie die installierten Dateien im Verzeichnis „C:\ProgramData“ überprüfen. Die Ausführung der Prozesse „FabricHost.exe“ und „Fabric.exe“ können Sie im Task-Manager verfolgen.
> 
> 

### <a name="step-4-connect-to-the-cluster"></a>Schritt 4: Herstellen einer Verbindung mit dem Cluster
Weitere Informationen zum Herstellen einer Verbindung mit einem sicheren Cluster finden Sie unter [Herstellen einer Verbindung mit einem sicheren Cluster ohne AAD](service-fabric-connect-to-secure-cluster.md).

Führen Sie den folgenden PowerShell-Befehl aus, um eine Verbindung mit einem unsicheren Cluster herzustellen:

```powershell

Connect-ServiceFabricCluster -ConnectionEndpoint <*IPAddressofaMachine*>:<Client connection end point port>

Connect-ServiceFabricCluster -ConnectionEndpoint 192.13.123.2345:19000

```
### <a name="step-5-bring-up-service-fabric-explorer"></a>Schritt 5: Aufrufen von Service Fabric Explorer
Jetzt können Sie über Service Fabric Explorer eine Verbindung mit dem Cluster herstellen, und zwar entweder direkt von einem der Computer mit „http://localhost:19080/Explorer/index.html“ oder per Remotezugriff mit „http://<*IPAddressofaMachine*>:19080/Explorer/index.html“.

## <a name="add-and-remove-nodes"></a>Hinzufügen und Entfernen von Knoten
Wenn sich Ihre geschäftlichen Anforderungen ändern, können Sie dem eigenständigen Service Fabric-Cluster Knoten hinzufügen oder Knoten daraus entfernen. Ausführliche Schritte finden Sie unter [Hinzufügen oder Entfernen von Knoten für einen eigenständigen Service Fabric-Cluster unter Windows Server](service-fabric-cluster-windows-server-add-remove-nodes.md).

## <a name="remove-a-cluster"></a>Entfernen eines Clusters
Führen Sie zum Entfernen eines Clusters das PowerShell-Skript *RemoveServiceFabricCluster.ps1* aus dem Paketordner aus, und übergeben Sie den Pfad zur JSON-Konfigurationsdatei. Sie können optional einen Speicherort für das Löschprotokoll angeben.

Dieses Skript kann auf jedem Computer ausgeführt werden, der Administratorzugriffsrechte auf alle Computer hat, die als Knoten in der Clusterkonfigurationsdatei aufgeführt sind. Der Computer, auf dem dieses Skript ausgeführt wird, muss nicht Teil des Clusters sein.

```
.\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json   
```


<a id="telemetry"></a>

## <a name="telemetry-data-collected-and-how-to-opt-out-of-it"></a>Erfasste Telemetriedaten und Vorgehensweise zum Deaktivieren der Erfassung
Zur Verbesserung des Produkts werden standardmäßig Telemetriedaten zur Service Fabric-Nutzung erfasst. Der im Rahmen der Einrichtung ausgeführte Best Practices Analyzer überprüft die Verbindung mit [https://vortex.data.microsoft.com/collect/v1](https://vortex.data.microsoft.com/collect/v1). Ist die Adresse nicht erreichbar, ist die Einrichtung nicht erfolgreich – es sei denn, Sie widersprechen der Erfassung von Telemetriedaten.

1. 1) Die Telemetriepipeline versucht einmal täglich, folgende Daten an [https://vortex.data.microsoft.com/collect/v1](https://vortex.data.microsoft.com/collect/v1) hochzuladen. Der Upload basiert auf dem Best-Effort-Prinzip und hat keinen Einfluss auf die Funktionen des Clusters. Die Telemetriedaten werden nur von dem Knoten gesendet, der die primäre Komponente des Failover-Managers ausführt. Andere Knoten senden keine Telemetriedaten.
2. Die Telemetriedaten umfassen Folgendes:

* Anzahl von Diensten
* Anzahl von Diensttypen
* Anzahl von Anwendungen
* Anzahl von Anwendungsupgrades
* Anzahl von Failovereinheiten
* Anzahl von InBuild-Failovereinheiten
* Anzahl von fehlerhaften Failovereinheiten
* Anzahl von Replikaten
* Anzahl von InBuild-Replikaten
* Anzahl von StandBy-Replikaten
* Anzahl von Offlinereplikaten
* Allgemeine Warteschlangenlänge
* Länge der Abfragewarteschlange
* Länge der Warteschlange für Failovereinheiten
* Länge der Commitwarteschlange
* Anzahl von Knoten
* IsContextComplete: „true“ oder „false“
* Cluster-ID: Eine für jeden Cluster nach dem Zufallsprinzip generierte GUID.
* Service Fabric-Version
* IP-Adresse des (virtuellen) Computers, von dem die Telemetriedaten hochgeladen werden

Fügen Sie zum Deaktivieren der Telemetriedaten dem *properties*-Element Ihrer Clusterkonfiguration den folgenden Code hinzu: *enableTelemetry: false*.

<a id="previewfeatures"></a>

## <a name="preview-features-included-in-this-package"></a>Features in der Vorschauphase in diesem Paket
Keine


> [!NOTE]
> Ab der neuen [GA-Version des eigenständigen Clusters für Windows Server (Version 5.3.204.x)](https://azure.microsoft.com/blog/azure-service-fabric-for-windows-server-now-ga/) können Sie Ihren Cluster manuell oder automatisch auf zukünftige Versionen upgraden. Ausführliche Informationen finden Sie im Dokument [Aktualisieren des eigenständigen Service Fabric-Clusters unter Windows Server](service-fabric-cluster-upgrade-windows-server.md).
> 
> 

## <a name="next-steps"></a>Nächste Schritte
* [Konfigurationseinstellungen für eigenständige Windows-Cluster](service-fabric-cluster-manifest.md)
* [Hinzufügen oder Entfernen von Knoten für einen eigenständigen Service Fabric-Cluster unter Windows Server](service-fabric-cluster-windows-server-add-remove-nodes.md)
* [Aktualisieren des eigenständigen Service Fabric-Clusters unter Windows Server](service-fabric-cluster-upgrade-windows-server.md)
* [Erstellen eines eigenständigen Service Fabric-Clusters mit drei Knoten und Azure-VMs mit Windows Server](service-fabric-cluster-creation-with-windows-azure-vms.md)
* [Schützen eines eigenständigen Windows-Clusters mithilfe von Windows-Sicherheit](service-fabric-windows-cluster-windows-security.md)
* [Schützen des eigenständigen Windows-Clusters mit Zertifikaten](service-fabric-windows-cluster-x509-security.md)

<!--Image references-->
[Trusted Zone]: ./media/service-fabric-cluster-creation-for-windows-server/TrustedZone.png



<!--HONumber=Dec16_HO2-->


