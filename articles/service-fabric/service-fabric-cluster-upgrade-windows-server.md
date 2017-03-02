---
title: "Aktualisieren eines eigenständigen Service Fabric-Clusters unter Windows Server | Microsoft-Dokumentation"
description: "Aktualisieren des Service Fabric-Codes und/oder Konfiguration für die Ausführung eines eigenständigen Service Fabric-Clusters und Festlegen des Clusteraktualisierungsmodus"
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: 
ms.assetid: 66296cc6-9524-4c6a-b0a6-57c253bdf67e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2017
ms.author: chackdan
translationtype: Human Translation
ms.sourcegitcommit: e9d7e1b5976719c07de78b01408b2546b4fec297
ms.openlocfilehash: 217715ad1657582eb35008b765de6d19bd2a8b0b
ms.lasthandoff: 02/16/2017


---
# <a name="upgrade-your-standalone-service-fabric-cluster-on-windows-server"></a>Aktualisieren des eigenständigen Service Fabric-Clusters unter Windows Server
> [!div class="op_single_selector"]
> * [Azure-Cluster](service-fabric-cluster-upgrade.md)
> * [Eigenständiger Cluster](service-fabric-cluster-upgrade-windows-server.md)
> 
> 

Moderner Systeme müssen upgradefähig sein, um den langfristigen Erfolg Ihres Produkts zu gewährleisten. Ein Service Fabric-Cluster ist eine Ressource, die sich in Ihrem Besitz befindet. In diesem Artikel wird beschrieben, wie Sie sicherstellen, dass der Cluster immer unterstützte Versionen des Service Fabric-Codes und von Konfigurationen ausführt.

## <a name="controlling-the-fabric-version-that-runs-on-your-cluster"></a>Steuern der in Ihrem Cluster ausgeführten Fabric-Version
Sie können Ihren Cluster so konfigurieren, dass er die Service Fabric-Updates herunterlädt, wenn Microsoft eine neue Version veröffentlicht. Alternativ können Sie eine unterstützte Fabric-Version auswählen, die für Ihren Cluster verwendet werden soll. 

Dazu müssen Sie die Clusterkonfiguration „fabricClusterAutoupgradeEnabled“ auf „true“ oder „false“ festlegen.

> [!NOTE]
> Achten Sie immer darauf, dass in Ihrem Cluster eine unterstützte Service Fabric-Version ausgeführt wird. Nach der Ankündigung einer neuen Service Fabric-Version beträgt die verbleibende Supportdauer der vorherigen Version noch mindestens 60 Tage. Neue Versionen werden im [Blog des Service Fabric-Teams](https://blogs.msdn.microsoft.com/azureservicefabric/)angekündigt. Ab dann kann die neue Version ausgewählt werden. 
> 
> 

Sie können Ihren Cluster nur auf die neue Version aktualisieren, wenn Sie eine Knotenkonfiguration im Produktionsformat verwenden, bei der jeder Service Fabric-Knoten einem separaten physischen oder virtuellen Computer zugeordnet ist. Wenn Sie einen Entwicklungscluster mit mehreren Service Fabric-Knoten auf einem physischen oder virtuellen Computer haben, müssen Sie Ihren Cluster auflösen und anschließend mit der neuen Version neu erstellen.

Für das Upgrade Ihres Clusters auf die aktuelle oder eine unterstützte Service Fabric-Version gibt es zwei unterschiedliche Workflows. Einen für Cluster, die über eine Verbindung zum automatischen Herunterladen der aktuellen Version verfügen, und der zweite für Cluster, die nicht über eine Verbindung zum Herunterladen der aktuellen Service Fabric-Version verfügen.

### <a name="upgrade-the-clusters-with-connectivity-to-download-the-latest-code-and-configuration"></a>Aktualisieren der Cluster mit Verbindung zum Herunterladen des aktuellen Codes und der Konfiguration
Führen Sie diese Schritte aus, um Ihren Cluster auf eine unterstützte Version zu aktualisieren, wenn Ihre Clusterknoten eine Internetverbindung mit [http://download.microsoft.com](http://download.microsoft.com) haben. 

Für Cluster mit Verbindung zu [http://download.microsoft.com](http://download.microsoft.com) überprüfen wir in regelmäßigen Abständen die Verfügbarkeit neuer Service Fabric-Versionen.

Wenn eine neue Service Fabric-Version verfügbar ist, wird das Paket lokal in den Cluster heruntergeladen und für das Upgrade bereitgestellt. Um den Kunden außerdem über diese neue Version zu informieren, zeigt das System eine explizite Clusterintegritätswarnung ähnlich der folgenden an:

„Die Unterstützung der aktuellen Clusterversion [Versionsnummer] [Datum]“. Sobald auf dem Cluster die aktuelle Version ausgeführt wird, verschwindet die Warnung.

#### <a name="cluster-upgrade-workflow"></a>Workflow für das Upgrade des Clusters
Wenn die Clusterintegritätswarnung angezeigt wird, müssen Sie folgende Schritte ausführen:

1. Stellen Sie auf einem Computer mit Administratorzugriffsrechten auf alle Computer, die als Knoten in der Clusterkonfigurationsdatei aufgeführt sind, eine Verbindung mit dem Cluster her. Der Computer, auf dem dieses Skript ausgeführt wird, muss nicht Teil des Clusters sein.
   
    ```powershell
   
    ###### connect to the secure cluster using certs
    $ClusterName= "mysecurecluster.something.com:19000"
    $CertThumbprint= "70EF5E22ADB649799DA3C8B6A6BF7FG2D630F8F3" 
    Connect-serviceFabricCluster -ConnectionEndpoint $ClusterName -KeepAliveIntervalInSec 10 `
        -X509Credential `
        -ServerCertThumbprint $CertThumbprint  `
        -FindType FindByThumbprint `
        -FindValue $CertThumbprint `
        -StoreLocation CurrentUser `
        -StoreName My
    ```
2. Rufen Sie die Liste der Service Fabric-Versionen ab, auf die Sie ein Upgrade durchführen können.
   
    ```powershell
   
    ###### Get the list of available service fabric versions 
    Get-ServiceFabricRegisteredClusterCodeVersion
    ```
   
    Die Ausgabe sollte in etwa wie folgt aussehen:
   
    ![Abrufen von Fabric-Versionen][getfabversions]
3. Starten Sie mithilfe des [PowerShell-Befehls „Start-ServiceFabricClusterUpgrade“](https://msdn.microsoft.com/library/mt125872.aspx) ein Clusterupgrade auf eine der verfügbaren Versionen.
   
    ```Powershell
   
    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback
   
    ###### Here is a filled out example
   
    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback
   
    ```
   Sie können den Fortschritt des Upgrades im Service Fabric Explorer oder durch Ausführen des folgenden PowerShell-Befehls verfolgen
   
    ```powershell
   
    Get-ServiceFabricClusterUpgrade
    ```
   
    Wenn die Integritätsrichtlinien des Clusters nicht erfüllt sind, wird das Upgrade zurückgesetzt. Sie können benutzerdefinierte Integritätsrichtlinien zum Zeitpunkt für den Befehl „Start-ServiceFabricClusterUpgrade“ angeben. Details finden Sie in [diesem Dokument](https://msdn.microsoft.com/library/mt125872.aspx). 

Beheben Sie die Probleme, die zu dem Rollback geführt haben, und initiieren Sie das Upgrade erneut, indem Sie die gleichen Schritte ausführen wie zuvor.

### <a name="upgrade-the-clusters-with-uno-connectivityu-to-download-the-latest-code-and-configuration"></a>Aktualisieren der Cluster <U>ohne Verbindung</u> zum Herunterladen des aktuellen Codes und der Konfiguration
Führen Sie diese Schritte aus, um Ihren Cluster auf eine unterstützte Version zu aktualisieren, wenn Ihre Clusterknoten **keine** Internetverbindung mit [http://download.microsoft.com](http://download.microsoft.com) haben. 

> [!NOTE]
> Wenn Sie einen Cluster ohne Internetverbindung ausführen, müssen Sie auf dem Service Fabric-Teamblog überprüfen, ob eine neue Version verfügbar ist. Das System zeigt **keine** Clusterintegritätswarnung an, um Sie darüber zu benachrichtigen.  
> 
> 

1. Ändern Sie die Clusterkonfiguration, um die folgende Eigenschaft auf „false“ festzulegen,
   
        "fabricClusterAutoupgradeEnabled": false,

und starten Sie ein Konfigurationsupgrade. Nutzungsdetails finden Sie in den Informationen zum PowerShell-Befwehl [Start-ServiceFabricClusterConfigurationUpgrade](https://msdn.microsoft.com/en-us/library/mt788302.aspx). Achten Sie darauf, den Wert für „clusterConfigurationVersion“ in der JSON-Datei zu aktualisieren, bevor Sie das Konfigurationsupgrade starten.

```powershell

    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File> 

```

#### <a name="cluster-upgrade-workflow"></a>Workflow für das Upgrade des Clusters
1. Führen Sie Get-ServiceFabricClusterUpgrade auf einem der Knoten im Cluster aus, und notieren Sie die TargetCodeVersion.
2. Führen Sie Folgendes auf einem mit dem Internet verbundenen Computer aus, um alle mit der aktuellen Version für das Upgrade kompatiblen Versionen aufzulisten und das entsprechende Paket über die zugehörigen Downloadlinks herunterzuladen.
   ```powershell
   
    ###### Get list of all upgrade compatible packages
    Get-ServiceFabricRuntimeUpgradeVersion -BaseVersion <TargetCodeVersion as noted in Step 1>
    ```
3. Stellen Sie auf einem Computer mit Administratorzugriffsrechten auf alle Computer, die als Knoten in der Clusterkonfigurationsdatei aufgeführt sind, eine Verbindung mit dem Cluster her. Der Computer, auf dem dieses Skript ausgeführt wird, muss nicht Teil des Clusters sein. 
   
    ```powershell
   
    ###### connect to the cluster
    $ClusterName= "mysecurecluster.something.com:19000"
    $CertThumbprint= "70EF5E22ADB649799DA3C8B6A6BF7FG2D630F8F3" 
    Connect-serviceFabricCluster -ConnectionEndpoint $ClusterName -KeepAliveIntervalInSec 10 `
        -X509Credential `
        -ServerCertThumbprint $CertThumbprint  `
        -FindType FindByThumbprint `
        -FindValue $CertThumbprint `
        -StoreLocation CurrentUser `
        -StoreName My
    ```
4. Kopieren Sie das heruntergeladene Paket in den Clusterimagespeicher.
   
    ```powershell
   
   ###### Get the list of available service fabric versions
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath <name of the .cab file including the path to it> -ImageStoreConnectionString "fabric:ImageStore"
   
   ###### Here is a filled out example
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath .\MicrosoftAzureServiceFabric.5.3.301.9590.cab -ImageStoreConnectionString "fabric:ImageStore"

    ```

5. Registrieren Sie das kopierte Paket. 
   
    ```powershell
   
    ###### Get the list of available service fabric versions 
    Register-ServiceFabricClusterPackage -Code -CodePackagePath <name of the .cab file> 
   
    ###### Here is a filled out example
    Register-ServiceFabricClusterPackage -Code -CodePackagePath MicrosoftAzureServiceFabric.5.3.301.9590.cab
   
     ```
6. Starten Sie ein Clusterupgrade auf eine der verfügbaren Versionen. 
   
    ```Powershell
   
    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback
   
    ###### Here is a filled out example
    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback
   
    ```
   Sie können den Fortschritt des Upgrades im Service Fabric Explorer oder durch Ausführen des folgenden PowerShell-Befehls verfolgen
   
    ```powershell
   
    Get-ServiceFabricClusterUpgrade
    ```
   
    Wenn die Integritätsrichtlinien des Clusters nicht erfüllt sind, wird das Upgrade zurückgesetzt. Sie können benutzerdefinierte Integritätsrichtlinien zum Zeitpunkt für den Befehl „start-serviceFabricClusterUpgrade“ angeben. Details finden Sie in [diesem Dokument](https://msdn.microsoft.com/library/mt125872.aspx). 

Beheben Sie die Probleme, die zu dem Rollback geführt haben, und initiieren Sie das Upgrade erneut, indem Sie die gleichen Schritte ausführen wie zuvor.


## <a name="cluster-configuration-upgrade"></a>Clusterkonfigurationsupgrade
Führen Sie für ein Upgrade der Clusterkonfiguration Start-ServiceFabricClusterConfigurationUpgrade aus. Das Konfigurationsupgrade wird Upgradedomäne für Upgradedomäne verarbeitet.

```powershell

    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File> 

```

### <a name="cluster-certificate-config-upgrade-pls-hold-on-till-v55-is-released-because-cluster-cert-upgrade-doesnt-work-till-v55"></a>Clusterzertifikatkonfigurations-Upgrade (PLS HOLD ON TILL v5.5 wurde veröffentlicht, da das Clusterzertifikatupgrade erst ab Version&5;.5 funktioniert)
Das Clusterzertifikat wird für die Authentifizierung zwischen Clusterknoten verwendet. Daher sollte der Zertifikatrollover mit besonderer Vorsicht ausgeführt werden, da Fehler die Kommunikation zwischen den Clusterknoten blockieren könnten.
Technisch gesehen werden zwei Optionen unterstützt:

1. Einzelnes Zertifikatupgrade: Der Upgradepfad lautet „Zertifikat A (primär) > Zertifikat B (primär) > Zertifikat C (primär) > ...“. 
2. Doppeltes Zertifikatupgrade: Der Upgradepfad lautet „Zertifikat A (primär) > Zertifikat A (primär) und B (sekundär) > Zertifikat B (primär) > Zertifikat B (primär) und C (sekundär) > Zertifikat C (primär) > ...“.


## <a name="next-steps"></a>Nächste Schritte
* Informieren Sie sich über die Vorgehensweise zum Anpassen einiger [Customize Service Fabric cluster settings and Fabric Upgrade policy](service-fabric-cluster-fabric-settings.md)
* Machen Sie sich mit der Vorgehensweise zum [Skalieren Ihres Clusters](service-fabric-cluster-scale-up-down.md)
* Machen Sie sich mit [Anwendungsupgrades](service-fabric-application-upgrade.md)

<!--Image references-->
[getfabversions]: ./media/service-fabric-cluster-upgrade-windows-server/getfabversions.PNG

