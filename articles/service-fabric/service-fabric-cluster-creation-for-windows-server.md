---
title: "Erstellen eines eigenständigen Azure Service Fabric-Clusters | Microsoft-Dokumentation"
description: "Erstellen Sie einen Azure Service Fabric-Cluster auf einem beliebigen Computer (physisch oder virtuell) unter Windows Server – lokal oder in einer Cloud."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 31349169-de19-4be6-8742-ca20ac41eb9e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/24/2017
ms.author: chackdan;maburlik;dekapur
ms.translationtype: Human Translation
ms.sourcegitcommit: 09f24fa2b55d298cfbbf3de71334de579fbf2ecd
ms.openlocfilehash: 7543d98030ea143d537e0028801fd3efe3e4ff32
ms.contentlocale: de-de
ms.lasthandoff: 06/07/2017

---
# <a name="create-a-standalone-cluster-running-on-windows-server"></a>Erstellen eines eigenständigen Clusters unter Windows Server
Mithilfe von Azure Service Fabric können Sie Service Fabric-Cluster auf beliebigen virtuellen oder physischen Computern erstellen, auf denen Windows Server ausgeführt wird. So können Sie Service Fabric-Anwendungen in jeder Umgebung bereitstellen und ausführen, in der sich miteinander verbundene Windows Server-Computer befinden – lokal oder bei einem Cloudanbieter. Service Fabric stellt ein Setuppaket zum Erstellen von Service Fabric-Clustern bereit. Es wird als eigenständiges Paket für Windows Server bezeichnet.

Dieser Artikel führt Sie durch den Vorgang zum Erstellen eines eigenständigen Service Fabric-Clusters.

> [!NOTE]
> Dieses eigenständige Windows Server-Paket ist im Handel erhältlich und für Produktionsbereitstellungen geeignet. Das Paket enthält unter Umständen neue Service Fabric-Features, die sich in der Vorschauphase befinden. Scrollen Sie nach unten zum Abschnitt [Features in der Vorschauphase in diesem Paket](#previewfeatures_anchor), um zur Liste mit den Vorschaufeatures zu gelangen. Sie können jetzt [eine Kopie des Endbenutzer-Lizenzvertrags herunterladen](http://go.microsoft.com/fwlink/?LinkID=733084).
> 
> 

<a id="getsupport"></a>

## <a name="get-support-for-the-service-fabric-for-windows-server-package"></a>Support für das Service Fabric-Paket für Windows Server
* Befragen Sie im [Azure Service Fabric-Forum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=AzureServiceFabric?) die Community zum eigenständigen Service Fabric-Paket für Windows Server.
* Erstellen Sie ein Ticket für [Professional Support für Service Fabric](http://support.microsoft.com/oas/default.aspx?prid=16146).  Weitere Informationen zu Professional Support von Microsoft finden Sie [hier](https://support.microsoft.com/en-us/gp/offerprophone?wa=wsignin1.0).
* Support für dieses Paket erhalten Sie auch im Rahmen von [Microsoft Premier Support](https://support.microsoft.com/en-us/premier).
* Weitere Informationen finden Sie unter [Supportoptionen für Azure Service Fabric](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-support).
* Führen Sie zum Erfassen der Protokolle für den Support den [eigenständigen Service Fabric-Protokollsammler](service-fabric-cluster-standalone-package-contents.md) aus.

<a id="downloadpackage"></a>

## <a name="download-the-service-fabric-for-windows-server-package"></a>Herunterladen des Service Fabric-Pakets für Windows Server
Verwenden Sie zum Erstellen des Clusters das Service Fabric-Paket für Windows Server (ab Windows Server 2012 R2): <br>
[Downloadlink: Service Fabric Standalone Package – Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690)

Informationen zum Inhalt des Pakets finden Sie [hier](service-fabric-cluster-standalone-package-contents.md).

Das Service Fabric-Laufzeitpaket wird zum Zeitpunkt der Clustererstellung automatisch heruntergeladen. Wenn die Bereitstellung über einen Computer erfolgt, der nicht mit dem Internet verbunden ist, laden Sie das Laufzeitpaket vorab von hier herunter: <br>
[Downloadlink: Service Fabric Runtime – Windows Server](https://go.microsoft.com/fwlink/?linkid=839354)

Hier finden Sie Konfigurationsbeispiele für eigenständige Cluster: <br>
[Standalone Cluster Configuration Samples](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples)

<a id="createcluster"></a>

## <a name="create-the-cluster"></a>Cluster erstellen
Service Fabric kann mithilfe der Datei *ClusterConfig.Unsecure.DevCluster.json*, die in den [Beispielen](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples) enthalten ist, in einem Entwicklungscluster mit einem einzigen Computer bereitgestellt werden.

Entpacken Sie das eigenständige Paket auf Ihrem Computer, und kopieren Sie die Beispielkonfigurationsdatei auf den lokalen Computer. Führen Sie anschließend in einer PowerShell-Administratorsitzung das Skript *CreateServiceFabricCluster.ps1* aus dem Ordner des eigenständigen Pakets aus:
### <a name="step-1a-create-an-unsecured-local-development-cluster"></a>Schritt 1A: Erstellen eines nicht gesicherten lokalen Entwicklungsclusters
```powershell
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json -AcceptEULA
```

Informationen zur Problembehandlung finden Sie unter [Planen und Vorbereiten der Clusterbereitstellung](service-fabric-cluster-standalone-deployment-preparation.md) im Abschnitt zum Einrichten der Umgebung.

Wenn Sie die Entwicklungsszenarien ausgeführt haben, können Sie den Service Fabric-Cluster anhand der Schritte unter [Entfernen eines Clusters](#removecluster_anchor) vom Computer entfernen. 

### <a name="step-1b-create-a-multi-machine-cluster"></a>Schritt 1B: Erstellen eines Clusters mit mehreren Computern
Wenn Sie die im nachstehenden Link beschriebenen Planungs- und Vorbereitungsschritte ausgeführt haben, können Sie mithilfe der Clusterkonfigurationsdatei Ihren Produktionscluster erstellen. <br>
[Planen und Vorbereiten der Clusterbereitstellung](service-fabric-cluster-standalone-deployment-preparation.md)

1. Überprüfen Sie Ihre Konfigurationsdatei, indem Sie das Skript *TestConfiguration.ps1* aus dem Ordner des eigenständigen Pakets ausführen:  

    ```powershell
    .\TestConfiguration.ps1 -ClusterConfigFilePath .\ClusterConfig.json
    ```

    Es sollte eine Ausgabe wie die folgende angezeigt werden. Wenn das untere Feld „Passed“ als „True“ angezeigt wird, wurden die Integritätsprüfungen erfolgreich abgeschlossen, und der Cluster ist basierend auf der Eingabekonfiguration bereitstellbar.

    ```
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

2. Erstellen Sie den Cluster: Führen Sie das Skript *CreateServiceFabricCluster.ps1* aus, um den Service Fabric-Cluster auf allen Computern in der Konfiguration bereitzustellen. 
    ```powershell
    .\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.json -AcceptEULA
    ```

> [!NOTE]
> Auf dem (virtuellen) Computer, auf dem Sie das PowerShell-Skript „CreateServiceFabricCluster.ps1“ ausgeführt haben, werden Ablaufverfolgungen der Bereitstellung geschrieben. Diese finden Sie im Unterordner „DeploymentTraces“ des Verzeichnisses, in dem das Skript ausgeführt wurde. Um zu ermitteln, ob Service Fabric ordnungsgemäß auf einem Computer bereitgestellt wurde, suchen Sie die installierten Dateien im Verzeichnis „FabricDataRoot“, wie im Abschnitt „FabricSettings“ der Clusterkonfigurationsdatei beschrieben (standardmäßig „C:\ProgramData\SF“). Im Task Manager wird auch die Ausführung der Prozesse „FabricHost.exe“ und „Fabric.exe“ angezeigt.
> 
> 

### <a name="step-1c-create-an-offline-internet-disconnected-cluster"></a>Schritt 1C: Erstellen eines Clusters ohne Internetverbindung (Offlinecluster)
Das Service Fabric-Laufzeitpaket wird bei der Clustererstellung automatisch heruntergeladen. Wenn Sie einen Cluster auf Computern ohne Internetverbindung bereitstellen, müssen Sie das Service Fabric-Laufzeitpaket separat herunterladen und bei der Clustererstellung den Pfad angeben, an dem sich das Paket befindet.
Das Laufzeitpaket kann über einen anderen Computer, der über eine Internetverbindung verfügt, heruntergeladen werden (unter [Downloadlink: Service Fabric Runtime – Windows Server](https://go.microsoft.com/fwlink/?linkid=839354)). Kopieren Sie das Laufzeitpaket an den Ort, von dem aus Sie den Offlinecluster bereitstellen, und erstellen Sie den Cluster, indem Sie `CreateServiceFabricCluster.ps1` mit dem Parameter `-FabricRuntimePackagePath` ausführen, wie hier dargestellt: 

```powershell
CreateServiceFabricCluster.ps1 -ClusterConfigurationFilePath <path to ClusterConfig.json> -FabricRuntimePackagePath <path to MicrosoftAzureServiceFabric.cab>
```
`<path to ClusterConfig.json>` und `<path to MicrosoftAzureServiceFabric.cab>` sind hierbei die Pfade zur Clusterkonfiguration bzw. zur CAB-Datei der Laufzeit.


### <a name="step-2-connect-to-the-cluster"></a>Schritt 2: Herstellen einer Verbindung mit dem Cluster
Weitere Informationen zum Herstellen einer Verbindung mit einem sicheren Cluster finden Sie unter [Herstellen einer Verbindung mit einem sicheren Cluster ohne AAD](service-fabric-connect-to-secure-cluster.md).

Führen Sie den folgenden PowerShell-Befehl aus, um eine Verbindung mit einem unsicheren Cluster herzustellen:

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <*IPAddressofaMachine*>:<Client connection end point port>

Connect-ServiceFabricCluster -ConnectionEndpoint 192.13.123.2345:19000
```
### <a name="step-3-bring-up-service-fabric-explorer"></a>Schritt 3: Aufrufen von Service Fabric Explorer
Jetzt können Sie über Service Fabric Explorer eine Verbindung mit dem Cluster herstellen, und zwar entweder direkt von einem der Computer mit „http://localhost:19080/Explorer/index.html“ oder per Remotezugriff mit „http://<*IPAddressofaMachine*>:19080/Explorer/index.html“.

## <a name="add-and-remove-nodes"></a>Hinzufügen und Entfernen von Knoten
Wenn sich Ihre geschäftlichen Anforderungen ändern, können Sie dem eigenständigen Service Fabric-Cluster Knoten hinzufügen oder Knoten daraus entfernen. Ausführliche Schritte finden Sie unter [Hinzufügen oder Entfernen von Knoten für einen eigenständigen Service Fabric-Cluster unter Windows Server](service-fabric-cluster-windows-server-add-remove-nodes.md).

<a id="removecluster" name="removecluster_anchor"></a>
## <a name="remove-a-cluster"></a>Entfernen eines Clusters
Führen Sie zum Entfernen eines Clusters das PowerShell-Skript *RemoveServiceFabricCluster.ps1* aus dem Paketordner aus, und übergeben Sie den Pfad zur JSON-Konfigurationsdatei. Sie können optional einen Speicherort für das Löschprotokoll angeben.

Dieses Skript kann auf jedem Computer ausgeführt werden, der Administratorzugriffsrechte auf alle Computer hat, die als Knoten in der Clusterkonfigurationsdatei aufgeführt sind. Der Computer, auf dem dieses Skript ausgeführt wird, muss nicht Teil des Clusters sein.

```
# Removes Service Fabric from each machine in the configuration
.\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.json -Force
```

```
# Removes Service Fabric from the current machine
.\CleanFabric.ps1
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

<a id="previewfeatures" name="previewfeatures_anchor"></a>

## <a name="preview-features-included-in-this-package"></a>Features in der Vorschauphase in diesem Paket
Keine


> [!NOTE]
> Ab der neuen [GA-Version des eigenständigen Clusters für Windows Server (Version 5.3.204.x)](https://azure.microsoft.com/blog/azure-service-fabric-for-windows-server-now-ga/) können Sie Ihren Cluster manuell oder automatisch auf zukünftige Versionen upgraden. Ausführliche Informationen finden Sie im Dokument [Aktualisieren des eigenständigen Service Fabric-Clusters unter Windows Server](service-fabric-cluster-upgrade-windows-server.md).
> 
> 

## <a name="next-steps"></a>Nächste Schritte
* [Bereitstellen und Entfernen von Anwendungen mit PowerShell](service-fabric-deploy-remove-applications.md)
* [Konfigurationseinstellungen für eigenständige Windows-Cluster](service-fabric-cluster-manifest.md)
* [Hinzufügen oder Entfernen von Knoten für einen eigenständigen Service Fabric-Cluster unter Windows Server](service-fabric-cluster-windows-server-add-remove-nodes.md)
* [Aktualisieren des eigenständigen Service Fabric-Clusters unter Windows Server](service-fabric-cluster-upgrade-windows-server.md)
* [Erstellen eines eigenständigen Service Fabric-Clusters mit drei Knoten und Azure-VMs mit Windows Server](service-fabric-cluster-creation-with-windows-azure-vms.md)
* [Schützen eines eigenständigen Windows-Clusters mithilfe von Windows-Sicherheit](service-fabric-windows-cluster-windows-security.md)
* [Schützen des eigenständigen Windows-Clusters mit Zertifikaten](service-fabric-windows-cluster-x509-security.md)

<!--Image references-->
[Trusted Zone]: ./media/service-fabric-cluster-creation-for-windows-server/TrustedZone.png

