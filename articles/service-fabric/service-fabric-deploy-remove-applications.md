---
title: Service Fabric-Anwendungsbereitstellung | Microsoft Docs
description: Bereitstellen und Entfernen von Anwendungen in Service Fabric
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: b120ffbf-f1e3-4b26-a492-347c29f8f66b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/16/2016
ms.author: ryanwi
translationtype: Human Translation
ms.sourcegitcommit: 6626747c501b01aa5e53657309ec79c75213483c
ms.openlocfilehash: 2e96e978c56ef2b9c901c952a6e96055a6ab91cf


---
# <a name="deploy-and-remove-applications-using-powershell"></a>Bereitstellen und Entfernen von Anwendungen mit PowerShell
> [!div class="op_single_selector"]
> * [PowerShell](service-fabric-deploy-remove-applications.md)
> * [Visual Studio](service-fabric-publish-app-remote-cluster.md)
> 
> 

<br/>

Sobald der [Anwendungstyp gepackt][10] wurde, ist die Anwendung für die Bereitstellung in einem Azure Service Fabric-Cluster bereit. Die Bereitstellung umfasst die folgenden drei Schritte:

1. Hochladen des Anwendungspakets
2. Registrierung des Anwendungstyps
3. Erstellen der Anwendungsinstanz

> [!NOTE]
> Wenn Sie Visual Studio zum Bereitstellen und Debuggen von Anwendungen in Ihrem lokalen Entwicklungscluster verwenden, werden alle folgenden Schritte automatisch über ein PowerShell-Skript ausgeführt.  Dieses Skript befindet sich im Ordner „Scripts“ des Anwendungsprojekts. In diesem Artikel wird die grundlegende Funktionsweise der Skripts erläutert, sodass Sie die gleichen Vorgänge außerhalb von Visual Studio ausführen können.
> 
> 

## <a name="upload-the-application-package"></a>Hochladen des Anwendungspakets
Beim Hochladen des Anwendungspakets wird das Paket an einem Speicherort gespeichert, an dem interne Service Fabric-Komponenten auf das Paket zugreifen können. Zum Hochladen können Sie PowerShell verwenden. Bevor Sie die in diesem Artikel aufgeführten PowerShell-Befehle ausführen, stellen Sie immer zuerst über [Connect-ServiceFabricCluster](https://docs.microsoft.com/powershell/servicefabric/vlatest/connect-servicefabriccluster) eine Verbindung mit dem Service Fabric-Cluster her.

Angenommen, Sie haben einen Ordner namens *MyApplicationType* , der die erforderlichen Anwendungs- und Dienstmanifeste sowie Code-/Konfigurations-/Datenpakete enthält. Mit dem Befehl [Copy-ServiceFabricApplicationPackage](https://docs.microsoft.com/powershell/servicefabric/vlatest/copy-servicefabricapplicationpackage) wird das Paket in den Clusterimagespeicher hochgeladen. Das Cmdlet **Get-ImageStoreConnectionStringFromClusterManifest** ist Teil des Service Fabric-SDK-PowerShell-Moduls und wird verwendet, um die Imagespeicher-Verbindungszeichenfolge abzurufen.  Um das SDK-Modul zu importieren, führen Sie Folgendes aus:

```
Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
```

Sie können ein Anwendungspaket aus *C:\users\ryanwi\Documents\Visual Studio 2015\Projects\MyApplication\myapplication\pkg\debug* in *c:\temp\MyApplicationType* kopieren (benennen Sie das Verzeichnis „debug“ in „MyApplicationType“ um). Im folgenden Beispiel wird das Paket hochgeladen:

```powershell
PS C:\temp> dir

    Directory: c:\temp


Mode                LastWriteTime         Length Name                                                                                   
----                -------------         ------ ----                                                                                   
d-----        8/12/2016  10:23 AM                MyApplicationType                                                                          

PS C:\temp> tree /f .\Stateless1Pkg

C:\TEMP\MyApplicationType
│   ApplicationManifest.xml
|
└───Stateless1Pkg
    |   ServiceManifest.xml
    │
    └───Code
    │   │  Microsoft.ServiceFabric.Data.dll
    │   │  Microsoft.ServiceFabric.Data.Interfaces.dll
    │   │  Microsoft.ServiceFabric.Internal.dll
    │   │  Microsoft.ServiceFabric.Internal.Strings.dll
    │   │  Microsoft.ServiceFabric.Services.dll
    │   │  ServiceFabricServiceModel.dll
    │   │  MyService.exe
    │   │  MyService.exe.config
    │   │  MyService.pdb
    │   │  System.Fabric.dll
    │   │  System.Fabric.Strings.dll
    │   │
    │   └───en-us
    |         Microsoft.ServiceFabric.Internal.Strings.resources.dll
    |         System.Fabric.Strings.resources.dll
    |
    ├───Config
    │     Settings.xml
    │
    └───Data
          init.dat

PS C:\temp> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath MyApplicationType -ImageStoreConnectionString (Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest))
Copy application package succeeded

PS D:\temp>
```

Weitere Informationen zum Imagespeicher und zu ImageStoreConnectionString finden Sie unter [Grundlegendes zur Imagespeicher-Verbindungszeichenfolge](service-fabric-image-store-connection-string.md).

## <a name="register-the-application-package"></a>Registrieren des Anwendungspakets
Der im Anwendungsmanifest deklarierte Anwendungstyp und die Version werden beim Registrieren des Anwendungspakets verfügbar. Das System liest das im vorherigen Schritt hochgeladene Paket, überprüft es, verarbeitet den Inhalt des Pakets und kopiert das verarbeitete Paket an einen internen Systemspeicherort.  Wenn Sie das App-Paket lokal überprüfen möchten, verwenden Sie das Cmdlet [Test-ServiceFabricApplicationPackage](https://docs.microsoft.com/powershell/servicefabric/vlatest/test-servicefabricapplicationpackage).

```powershell
PS D:\temp> Register-ServiceFabricApplicationType MyApplicationType
Register application type succeeded

PS D:\temp> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : AppManifestVersion1
DefaultParameters      : {}

PS D:\temp>
```

Der Befehl [Register-ServiceFabricApplicationType](https://docs.microsoft.com/powershell/servicefabric/vlatest/register-servicefabricapplicationtype) wird erst zurückgegeben, wenn das Anwendungspaket vom System erfolgreich kopiert wurde. Die Dauer des Registriervorgangs hängt von der Größe und dem Inhalt des Anwendungspakets ab. Verwenden Sie bei Bedarf den Parameter **-TimeoutSec**, wenn ein längeres Zeitlimit erforderlich ist (das Standardzeitlimit beträgt 60 Sekunden).  Wenn es sich um ein großes App-Paket handelt und Timeouts auftreten, verwenden Sie den Parameter **-Async**.

Der Befehl [Get-ServiceFabricApplicationType](https://docs.microsoft.com/powershell/servicefabric/vlatest/get-servicefabricapplicationtype) listet alle erfolgreich registrierten Anwendungstypversionen auf.

## <a name="create-the-application"></a>Erstellen der Anwendung
Sie können eine Anwendung mit einer beliebigen Version des Anwendungstyps instanziieren, die mit dem Befehl [New-ServiceFabricApplication](https://docs.microsoft.com/powershell/servicefabric/vlatest/new-servicefabricapplication) erfolgreich registriert wurde. Der Name jeder Anwendung muss mit dem *fabric:* -Schema beginnen und für jede Anwendungsinstanz eindeutig sein. Wenn im Anwendungsmanifest des Zielanwendungstyps Standarddienste festgelegt wurden, werden diese ebenfalls erstellt.

```powershell
PS D:\temp> New-ServiceFabricApplication fabric:/MyApp MyApplicationType AppManifestVersion1

ApplicationName        : fabric:/MyApp
ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : AppManifestVersion1
ApplicationParameters  : {}

PS D:\temp> Get-ServiceFabricApplication  

ApplicationName        : fabric:/MyApp
ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : AppManifestVersion1
ApplicationStatus      : Ready
HealthState            : OK
ApplicationParameters  : {}

PS D:\temp> Get-ServiceFabricApplication | Get-ServiceFabricService

ServiceName            : fabric:/MyApp/MyService
ServiceKind            : Stateless
ServiceTypeName        : MyServiceType
IsServiceGroup         : False
ServiceManifestVersion : SvcManifestVersion1
ServiceStatus          : Active
HealthState            : Ok

PS D:\temp>
```

Der Befehl [Get-ServiceFabricApplication](https://docs.microsoft.com/powershell/servicefabric/vlatest/get-servicefabricapplication) listet alle erfolgreich erstellten Anwendungsinstanzen zusammen mit ihrem Gesamtzustand auf.

Der Befehl [Get-ServiceFabricService](https://docs.microsoft.com/powershell/servicefabric/vlatest/get-servicefabricservice) listet alle Dienstinstanzen auf, die innerhalb einer bestimmten Anwendungsinstanz erfolgreich erstellt wurden. Die Standarddienste (sofern vorhanden) werden hier aufgelistet.

Für jede Version des registrierten Anwendungstyps können mehrere Anwendungsinstanzen erstellt werden. Jede Anwendungsinstanz wird isoliert mit einem eigenen Arbeitsverzeichnis und Prozess ausgeführt.

## <a name="remove-an-application"></a>Entfernen einer Anwendung
Wird eine Anwendungsinstanz nicht mehr benötigt, kann diese mit dem Befehl [Remove-ServiceFabricApplication](https://docs.microsoft.com/powershell/servicefabric/vlatest/remove-servicefabricapplication) dauerhaft entfernt werden. Mit diesem Befehl werden auch alle Dienste automatisch entfernt, die mit der Anwendung verknüpft sind, d.h., der Dienstzustand wird vollständig und dauerhaft entfernt. Dieser Vorgang kann nicht rückgängig gemacht werden, und der Anwendungsstatus kann nicht wiederhergestellt werden.

```powershell
PS D:\temp> Remove-ServiceFabricApplication fabric:/MyApp

Confirm
Continue with this operation?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
Remove application instance succeeded

PS D:\temp> Get-ServiceFabricApplication
PS D:\temp>
```

Wird eine bestimmte Version eines Anwendungstyps nicht mehr benötigt, heben Sie die Registrierung der Version mit dem Befehl [Unregister-ServiceFabricApplicationType](https://docs.microsoft.com/powershell/servicefabric/vlatest/unregister-servicefabricapplicationtype) auf. Durch das Aufheben der Registrierung nicht verwendeter Typen wird Speicherplatz freigegeben, der von dem Inhalt des Anwendungspakets dieses Typs im Image Store verwendet wird. Die Registrierung eines Anwendungstyps kann nur aufgehoben werden, wenn keine Anwendungen für den Typ instanziiert sind und keine ausstehenden Anwendungsupgrades vorliegen, die auf den Typ verweisen.

```powershell
PS D:\temp> Get-ServiceFabricApplicationType

ApplicationTypeName    : DemoAppType
ApplicationTypeVersion : v1
DefaultParameters      : {}

ApplicationTypeName    : DemoAppType
ApplicationTypeVersion : v2
DefaultParameters      : {}

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : AppManifestVersion1
DefaultParameters      : {}

PS D:\temp> Unregister-ServiceFabricApplicationType MyApplicationType AppManifestVersion1
Unregister application type succeeded

PS D:\temp> Get-ServiceFabricApplicationType

ApplicationTypeName    : DemoAppType
ApplicationTypeVersion : v1
DefaultParameters      : {}

ApplicationTypeName    : DemoAppType
ApplicationTypeVersion : v2
DefaultParameters      : {}

PS D:\temp>
```

## <a name="troubleshooting"></a>Problembehandlung
### <a name="copy-servicefabricapplicationpackage-asks-for-an-imagestoreconnectionstring"></a>Copy-ServiceFabricApplicationPackage fordert einen ImageStoreConnectionString an
Die Service Fabric-SDK-Umgebung sollte bereits mit den richtigen Standardeinstellungen eingerichtet sein. Wichtig ist, dass der ImageStoreConnectionString für alle Befehle mit dem vom Service Fabric-Cluster verwendeten Wert übereinstimmt. Sie finden ImageStoreConnectionString im Clustermanifest, das über den Befehl [Get-ServiceFabricClusterManifest](https://docs.microsoft.com/powershell/servicefabric/vlatest/get-servicefabricclustermanifest) abgerufen wird:

```powershell
PS D:\temp> Get-ServiceFabricClusterManifest
```

ImageStoreConnectionString ist im Clustermanifest zu finden:

```xml
<ClusterManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Name="Server-Default-SingleNode" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

    [...]

    <Section Name="Management">
      <Parameter Name="ImageStoreConnectionString" Value="file:D:\ServiceFabric\Data\ImageStore" />
    </Section>

    [...]
```

## <a name="next-steps"></a>Nächste Schritte
[Service Fabric-Anwendungsupgrade](service-fabric-application-upgrade.md)

[Einführung in Service Fabric-Integrität](service-fabric-health-introduction.md)

[Diagnose und Problembehandlung von Service Fabric-Diensten](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Modellieren einer Anwendung in Service Fabric](service-fabric-application-model.md)

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-application-model.md
[11]: service-fabric-application-upgrade.md



<!--HONumber=Feb17_HO2-->


