---
title: "Upgraden eines eigenständigen Azure Service Fabric-Clusters unter Windows Server | Microsoft-Dokumentation"
description: "Upgraden des Azure Service Fabric-Codes und/oder Konfiguration für die Ausführung eines eigenständigen Service Fabric-Clusters und Festlegen des Clusteraktualisierungsmodus"
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
ms.sourcegitcommit: 094729399070a64abc1aa05a9f585a0782142cbf
ms.openlocfilehash: df15775f8e93c1dfad82c69ee4caa7838a39a545
ms.lasthandoff: 03/07/2017


---
# <a name="upgrade-your-standalone-azure-service-fabric-cluster-on-windows-server"></a>Upgraden des eigenständigen Azure Service Fabric-Clusters unter Windows Server
> [!div class="op_single_selector"]
> * [Azure-Cluster](service-fabric-cluster-upgrade.md)
> * [Eigenständiger Cluster](service-fabric-cluster-upgrade-windows-server.md)
>
>

Moderner Systeme müssen upgradefähig sein, um den langfristigen Erfolg Ihres Produkts zu gewährleisten. Ein Azure Service Fabric-Cluster ist eine Ressource, die sich in Ihrem Besitz befindet. In diesem Artikel wird beschrieben, wie Sie sicherstellen, dass der Cluster immer unterstützte Versionen des Service Fabric-Codes und von Konfigurationen ausführt.

## <a name="control-the-fabric-version-that-runs-on-your-cluster"></a>Steuern der in Ihrem Cluster ausgeführten Fabric-Version
Sie können Ihren Cluster festlegen, um die Service Fabric-Updates herunterzuladen, wenn Microsoft eine neue Version veröffentlicht. Eine andere Möglichkeit ist die Auswahl einer unterstützten Fabric-Version für Ihr Cluster.

Um die Fabric-Version zu steuern, müssen Sie die Clusterkonfiguration „fabricClusterAutoupgradeEnabled“ auf „TRUE“ oder „FALSE“ festlegen.

> [!NOTE]
> Achten Sie immer darauf, dass in Ihrem Cluster eine unterstützte Service Fabric-Version ausgeführt wird. Nach der Ankündigung einer neuen Service Fabric-Version beträgt die verbleibende Supportdauer der vorherigen Version noch mindestens 60 Tage. Neue Versionen werden im [Blog des Service Fabric-Teams](https://blogs.msdn.microsoft.com/azureservicefabric/) angekündigt.
>
>

Sie können Ihren Cluster nur auf die neue Version upgraden, wenn Sie eine Knotenkonfiguration im Produktionsformat verwenden, bei der jeder Service Fabric-Knoten einem separaten physischen oder virtuellen Computer zugeordnet ist. Wenn Sie einen Entwicklungscluster mit mehreren Service Fabric-Knoten auf einem physischen oder virtuellen Computer haben, müssen Sie Ihren Cluster auflösen und anschließend mit der neuen Version neu erstellen.

Die folgenden beiden Workflows sind verfügbar, um ein Upgrade für Ihren Cluster auf die neueste Version oder auf eine unterstützte Service Fabric-Version:


*   Clusters, die über Konnektivität verfügen, um die neuesten Versionen automatisch herunterzuladen
*   Clusters, die über Konnektivität verfügen, um die neueste Service Fabric-Version herunterzuladen

### <a name="upgrade-the-clusters-with-connectivity-to-download-the-latest-code-and-configuration"></a>Aktualisieren der Cluster mit Verbindung zum Herunterladen des aktuellen Codes und der Konfiguration
Führen Sie diese Schritte aus, um Ihren Cluster auf eine unterstützte Version upzugraden, wenn Ihre Clusterknoten eine Internetverbindung mit dem [Microsoft Download Center](http://download.microsoft.com) haben.

Für Cluster mit Verbindung zum [Microsoft Download Center](http://download.microsoft.com) überprüfen wir in regelmäßigen Abständen die Verfügbarkeit neuer Service Fabric-Versionen.

Wenn eine neue Service Fabric-Version verfügbar ist, wird das Paket lokal in den Cluster heruntergeladen und für das Upgrade bereitgestellt. Um den Kunden außerdem über diese neue Version zu informieren, zeigt das System eine explizite Clusterintegritätswarnung ähnlich der folgenden an:

„Der Support für die aktuelle Clusterversion [Version#] endet am [Datum].“

Wenn der Cluster die neueste Version ausführt, wird diese Warnung nicht mehr angezeigt.

#### <a name="cluster-upgrade-workflow"></a>Workflow für das Upgrade des Clusters
Nachdem die Clusterintegritätswarnung angezeigt wird, müssen Sie folgende Schritte ausführen:

1. Stellen Sie auf einem Computer mit Administratorzugriffsrechten auf alle Computer, die als Knoten in der Clusterkonfigurationsdatei aufgeführt sind, eine Verbindung mit dem Cluster her. Der Computer, auf dem folgendes Skript ausgeführt wird, muss nicht Teil des Clusters sein.

    ```powershell

    ###### Connect to the secure cluster using certs
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

    ###### Get the list of available Service Fabric versions
    Get-ServiceFabricRegisteredClusterCodeVersion
    ```

    Die Ausgabe sollte in etwa wie folgt aussehen:

    ![Abrufen von Service Fabric-Versionen][getfabversions]

3. Starten Sie mithilfe des Windows PowerShell-Befehls [Start-ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt125872.aspx) ein Clusterupgrade auf eine der verfügbaren Versionen.

    ```powershell

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Cluster upgrade example

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback

    ```
   Um den Fortschritt des Upgrades zu verfolgen, können Sie Service Fabric Explorer nutzen oder den folgenden PowerShell-Befehl verwenden.

    ```powershell

    Get-ServiceFabricClusterUpgrade
    ```

Wenn die Integritätsrichtlinien des Clusters nicht erfüllt sind, wird das Upgrade zurückgesetzt. Sie können benutzerdefinierte Integritätsrichtlinien zum Zeitpunkt für den Befehl „start-serviceFabricClusterUpgrade“ angeben. Weitere Informationen finden Sie unter [Start-ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt125872.aspx).

Nachdem Sie die Probleme behoben haben, die zu dem Rollback geführt haben, müssen Sie das Upgrade erneut initiieren, indem Sie die gleichen Schritte ausführen wie zuvor.

### <a name="upgrade-the-clusters-with-no-connectivity-to-download-the-latest-code-and-configuration"></a>Upgrade der Cluster ohne Verbindung zum Herunterladen des aktuellen Codes und der Konfiguration
Verwenden Sie diese Prozedur, um Ihren Cluster auf eine unterstützte Version upzugraden, wenn Ihre Clusterknoten keine Internetverbindung mit dem [Microsoft Download Center](http://download.microsoft.com) haben.

> [!NOTE]
> Wenn Sie einen Cluster ohne Internetverbindung ausführen, müssen Sie auf dem [Service Fabric-Teamblog](https://blogs.msdn.microsoft.com/azureservicefabric/) überprüfen, ob Benachrichtigung über eine neue Version verfügbar sind. Das System zeigt *keine* Clusterintegritätswarnung an, um Sie über die Version zu benachrichtigen.  
>
>

1. Ändern Sie die Clusterkonfiguration, um die folgende Eigenschaft auf „false“ festzulegen,

        "fabricClusterAutoupgradeEnabled": false,

2.      Starten Sie ein Konfigurationsupgrade Weitere Informationen finden Sie unter [Start-ServiceFabricClusterConfigurationUpgrade](https://msdn.microsoft.com/en-us/library/mt788302.aspx). Bevor Sie das Konfigurationsupgrade beginnen, upgraden Sie den „clusterConfigurationVersion“-Wert in der JavaScript Object Notation-Datei (JSON).

```powershell

    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>

```

#### <a name="cluster-upgrade-workflow"></a>Workflow für das Upgrade des Clusters
1. Laden Sie die aktuelle Version des Pakets aus dem Dokument [Service Fabric-Cluster für Windows Server erstellen](service-fabric-cluster-creation-for-windows-server.md) herunter.
2. Stellen Sie auf einem Computer mit Administratorzugriffsrechten auf alle Computer, die als Knoten in der Clusterkonfigurationsdatei aufgeführt sind, eine Verbindung mit dem Cluster her. Der Computer, auf dem dieses Skript ausgeführt wird, muss nicht Teil des Clusters sein.

    ```powershell

    ###### Connect to the cluster
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
3. Kopieren Sie das heruntergeladene Paket in den Clusterimagespeicher.

    ```powershell

   ###### Get the list of available Service Fabric versions
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath <name of the .cab file including the path to it> -ImageStoreConnectionString "fabric:ImageStore"

   ###### Code example
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath .\MicrosoftAzureServiceFabric.5.3.301.9590.cab -ImageStoreConnectionString "fabric:ImageStore"

    ```

4. Registrieren Sie das kopierte Paket.

    ```powershell

    ###### Get the list of available Service Fabric versions
    Register-ServiceFabricClusterPackage -Code -CodePackagePath <name of the .cab file>

    ###### Code example
    Register-ServiceFabricClusterPackage -Code -CodePackagePath MicrosoftAzureServiceFabric.5.3.301.9590.cab

     ```
5. Starten Sie ein Clusterupgrade auf eine der verfügbaren Versionen.

    ```Powershell

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Code example
    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback

    ```
   Um den Fortschritt des Upgrades zu verfolgen, können Sie Service Fabric Explorer nutzen oder den folgenden PowerShell-Befehl verwenden.

    ```powershell

    Get-ServiceFabricClusterUpgrade
    ```

    Wenn die Integritätsrichtlinien des Clusters nicht erfüllt sind, wird das Upgrade zurückgesetzt. Sie können benutzerdefinierte Integritätsrichtlinien zum Zeitpunkt für den Befehl „start-serviceFabricClusterUpgrade“ angeben. Weitere Informationen finden Sie unter [Start-ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt125872.aspx).

Nachdem Sie die Probleme behoben haben, die zu dem Rollback geführt haben, müssen Sie das Upgrade erneut initiieren, indem Sie die gleichen Schritte ausführen wie zuvor.


## <a name="upgrade-the-cluster-configuration"></a>Upgraden der Clusterkonfiguration
Um die Clusterkonfiguration upzugraden, führen Sie den Befehl „Start-ServiceFabricClusterConfigurationUpgrade“ aus. Das Konfigurationsupgrade wird durch die Upgradedomäne verarbeitet.

```powershell

    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>

```


## <a name="next-steps"></a>Nächste Schritte
* Informieren Sie sich über die Vorgehensweise zum [Anpassen von Service Fabric-Clustereinstellungen und der Fabric-Upgraderichtlinie](service-fabric-cluster-fabric-settings.md).
* Machen Sie sich mit der Vorgehensweise zum [Skalieren Ihres Clusters](service-fabric-cluster-scale-up-down.md) vertraut.
* Machen Sie sich mit [Anwendungsupgrades](service-fabric-application-upgrade.md) vertraut.

<!--Image references-->
[getfabversions]: ./media/service-fabric-cluster-upgrade-windows-server/getfabversions.PNG

