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
ms.date: 02/23/2017
ms.author: ryanwi
translationtype: Human Translation
ms.sourcegitcommit: dbd4dd3cadf162ea18d58639d31589f7b9b8efc3
ms.openlocfilehash: 2dfdcd08501a63d62ec6ba565d1abc7d42c8c680
ms.lasthandoff: 02/27/2017


---
# <a name="deploy-and-remove-applications-using-powershell"></a>Bereitstellen und Entfernen von Anwendungen mit PowerShell
> [!div class="op_single_selector"]
> * [PowerShell](service-fabric-deploy-remove-applications.md)
> * [Visual Studio](service-fabric-publish-app-remote-cluster.md)
> 
> 

<br/>

Sobald der [Anwendungstyp gepackt][10] wurde, ist die Anwendung für die Bereitstellung in einem Azure Service Fabric-Cluster bereit. Die Bereitstellung umfasst die folgenden drei Schritte:

1. Hochladen des Anwendungspakets in den Imagespeicher
2. Registrierung des Anwendungstyps
3. Erstellen der Anwendungsinstanz

Nachdem eine Anwendung bereitgestellt wurde und eine Instanz im Cluster ausgeführt wird, können Sie die App-Instanz und ihren Anwendungstyp löschen. Das vollständige Entfernen einer App aus dem Cluster umfasst die folgenden Schritte:

1. Entfernen (oder Löschen) der ausgeführten Anwendungsinstanz
2. Aufheben der Registrierung des Anwendungstyps, wenn er nicht mehr benötigt wird
3. Entfernen des Anwendungspakets aus dem Image-Speicher

Wenn Sie [Visual Studio zum Bereitstellen und Debuggen von Anwendungen](service-fabric-publish-app-remote-cluster.md) in Ihrem lokalen Entwicklungscluster verwenden, werden alle vorherigen Schritte automatisch über ein PowerShell-Skript ausgeführt.  Dieses Skript befindet sich im Ordner *Skripts* des Anwendungsprojekts. In diesem Artikel wird die grundlegende Funktionsweise dieses Skripts erläutert, sodass Sie die gleichen Vorgänge außerhalb von Visual Studio ausführen können. 
 
## <a name="connect-to-the-cluster"></a>Verbinden mit dem Cluster
Bevor Sie die in diesem Artikel aufgeführten PowerShell-Befehle ausführen, stellen Sie immer zuerst über [Connect-ServiceFabricCluster](/powershell/servicefabric/vlatest/connect-servicefabriccluster) eine Verbindung mit dem Service Fabric-Cluster her. Führen Sie zum Herstellen der Verbindung mit dem lokalen Entwicklungscluster den folgenden Befehl aus:

```powershell
PS C:\>Connect-ServiceFabricCluster
```

Beispiele für das Herstellen einer Verbindung mit einem Remotecluster bzw. einem mit Azure Active Directory, X509-Zertifikaten oder Windows Active Directory gesicherten Cluster finden Sie unter [Herstellen einer Verbindung mit einem sicheren Cluster](service-fabric-connect-to-secure-cluster.md).

## <a name="upload-the-application-package"></a>Hochladen des Anwendungspakets
Beim Hochladen des Anwendungspakets wird das Paket an einem Speicherort gespeichert, an dem interne Service Fabric-Komponenten auf das Paket zugreifen können. Wenn Sie das App-Paket lokal überprüfen möchten, verwenden Sie das Cmdlet [Test-ServiceFabricApplicationPackage](/powershell/servicefabric/vlatest/test-servicefabricapplicationpackage).  Mit dem Befehl [Copy-ServiceFabricApplicationPackage](/powershell/servicefabric/vlatest/copy-servicefabricapplicationpackage) wird das Anwendungspaket in den Clusterimagespeicher hochgeladen. Das Cmdlet **Get-ImageStoreConnectionStringFromClusterManifest** ist Teil des Service Fabric-SDK-PowerShell-Moduls und wird verwendet, um die Imagespeicher-Verbindungszeichenfolge abzurufen.  Um das SDK-Modul zu importieren, führen Sie Folgendes aus:

```powershell
Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
```

Angenommen, Sie erstellen und verpacken eine App namens *MyApplication* in Visual Studio. Der Name des Anwendungstyps ist in der Datei „ApplicationManifest.xml“ standardmäßig als „MyApplicationType“ aufgeführt.  Das Anwendungspaket mit den erforderlichen Anwendungs- und Dienstmanifesten sowie Code-/Konfigurations-/Datenpaketen befindet sich in *C:\Benutzer\Benutzername\Eigene Dokumente\Visual Studio 2015\Projects\MyApplication\MyApplication\pkg\Debug*. 

Mit dem folgenden Befehl werden die Inhalte des Anwendungspakets aufgelistet:

```powershell
PS C:\> tree /f 'C:\Users\user\Documents\Visual Studio 2015\Projects\MyApplication\MyApplication\pkg\Debug'
Folder PATH listing for volume OSDisk
Volume serial number is 0459-2393
C:\USERS\USER\DOCUMENTS\VISUAL STUDIO 2015\PROJECTS\MYAPPLICATION\MYAPPLICATION\PKG\DEBUG
│   ApplicationManifest.xml
│
└───Stateless1Pkg
    │   ServiceManifest.xml
    │
    ├───Code
    │       Microsoft.ServiceFabric.Data.dll
    │       Microsoft.ServiceFabric.Data.Interfaces.dll
    │       Microsoft.ServiceFabric.Internal.dll
    │       Microsoft.ServiceFabric.Internal.Strings.dll
    │       Microsoft.ServiceFabric.Services.dll
    │       ServiceFabricServiceModel.dll
    │       Stateless1.exe
    │       Stateless1.exe.config
    │       Stateless1.pdb
    │       System.Fabric.dll
    │       System.Fabric.Strings.dll
    │
    └───Config
            Settings.xml
```

Im folgenden Beispiel wird das Paket in den Imagespeicher in einen Ordner mit dem Namen „MyApplicationV1“ hochgeladen:

```powershell
PS C:\> $path = 'C:\Users\user\Documents\Visual Studio 2015\Projects\MyApplication\MyApplication\pkg\Debug'
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath $path -ApplicationPackagePathInImageStore MyApplicationV1 -ImageStoreConnectionString (Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest))
```

Wenn Sie den Parameter *-ApplicationPackagePathInImageStore* nicht angeben, wird das App-Paket in den Ordner „Debug“ im Imagespeicher kopiert.

Weitere Informationen zum Imagespeicher und zur Imagespeicher-Verbindungszeichenfolge finden Sie unter [Grundlegendes zur Imagespeicher-Verbindungszeichenfolge](service-fabric-image-store-connection-string.md).

## <a name="register-the-application-package"></a>Registrieren des Anwendungspakets
Der im Anwendungsmanifest deklarierte Anwendungstyp und die Version werden beim Registrieren des Anwendungspakets verfügbar. Das System liest das im vorherigen Schritt hochgeladene Paket, überprüft es, verarbeitet den Inhalt des Pakets und kopiert das verarbeitete Paket an einen internen Systemspeicherort.  

Führen Sie das Cmdlet [Register ServiceFabricApplicationType](/powershell/servicefabric/vlatest/register-servicefabricapplicationtype) aus, um den Anwendungstyp im Cluster zu registrieren, und für die Bereitstellung verfügbar zu machen:

```powershell
PS C:\> Register-ServiceFabricApplicationType MyApplicationV1
Register application type succeeded
```

„MyApplicationV1“ ist der Ordner im Imagespeicher, in dem sich das App-Paket befindet. Der Anwendungstyp mit dem Namen „MyApplicationType“ und Version „1.0.0“ (beides befindet sich im Anwendungsmanifest) ist jetzt im Cluster registriert.

Der Befehl [Register-ServiceFabricApplicationType](/powershell/servicefabric/vlatest/register-servicefabricapplicationtype) wird erst zurückgegeben, wenn das Anwendungspaket vom System erfolgreich registriert wurde. Die Dauer des Registriervorgangs hängt von der Größe und dem Inhalt des Anwendungspakets ab. Verwenden Sie bei Bedarf den Parameter **-TimeoutSec**, wenn ein längeres Zeitlimit erforderlich ist (das Standardzeitlimit beträgt 60 Sekunden).  Wenn es sich um ein großes App-Paket handelt und Timeouts auftreten, verwenden Sie den Parameter **-Async**.

Der Befehl [Get-ServiceFabricApplicationType](/powershell/servicefabric/vlatest/get-servicefabricapplicationtype) listet alle erfolgreich registrierten Anwendungstypversionen und deren Registrierungsstatus auf.

```powershell
PS C:\> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
Status                 : Available
DefaultParameters      : { "Stateless1_InstanceCount" = "-1" }
```

## <a name="create-the-application"></a>Erstellen der Anwendung
Sie können eine Anwendung mit einer beliebigen Version des Anwendungstyps instanziieren, die mit dem Cmdlet [New-ServiceFabricApplication](/powershell/servicefabric/vlatest/new-servicefabricapplication) erfolgreich registriert wurde. Der Name jeder Anwendung muss mit dem *fabric:* -Schema beginnen und für jede Anwendungsinstanz eindeutig sein. Wenn im Anwendungsmanifest des Zielanwendungstyps Standarddienste festgelegt wurden, werden diese ebenfalls erstellt.

```powershell
PS C:\> New-ServiceFabricApplication fabric:/MyApp MyApplicationType 1.0.0

ApplicationName        : fabric:/MyApp
ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
ApplicationParameters  : {}
```
Für jede Version des registrierten Anwendungstyps können mehrere Anwendungsinstanzen erstellt werden. Jede Anwendungsinstanz wird isoliert mit einem eigenen Arbeitsverzeichnis und Prozess ausgeführt.

Zum Anzeigen welche benannten Apps und Dienste im Cluster ausgeführt werden, führen Sie die Cmdlets [Get-ServiceFabricApplication](/powershell/servicefabric/vlatest/get-servicefabricapplication) und [Get ServiceFabricService](/powershell/servicefabric/vlatest/get-servicefabricservice) aus:

```powershell
PS C:\> Get-ServiceFabricApplication  

ApplicationName        : fabric:/MyApp
ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
ApplicationStatus      : Ready
HealthState            : Ok
ApplicationParameters  : {}

PS C:\> Get-ServiceFabricApplication | Get-ServiceFabricService

ServiceName            : fabric:/MyApp/Stateless1
ServiceKind            : Stateless
ServiceTypeName        : Stateless1Type
IsServiceGroup         : False
ServiceManifestVersion : 1.0.0
ServiceStatus          : Active
HealthState            : Ok
```

## <a name="remove-an-application"></a>Entfernen einer Anwendung
Wird eine Anwendungsinstanz nicht mehr benötigt, können Sie diese anhand des Namens mit dem Cmdlet [Remove-ServiceFabricApplication](/powershell/servicefabric/vlatest/remove-servicefabricapplication) endgültig entfernen. Mit [Remove-ServiceFabricApplication](/powershell/servicefabric/vlatest/remove-servicefabricapplication) werden auch alle Dienste automatisch entfernt, die mit der Anwendung verknüpft sind, d.h. der Dienstzustand wird vollständig und dauerhaft entfernt. Dieser Vorgang kann nicht rückgängig gemacht werden, und der Anwendungsstatus kann nicht wiederhergestellt werden.

```powershell
PS C:\> Remove-ServiceFabricApplication fabric:/MyApp

Confirm
Continue with this operation?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
Remove application instance succeeded

PS C:\> Get-ServiceFabricApplication
```

## <a name="unregister-an-application-type"></a>Aufheben der Registrierung eines Anwendungstyps
Wird eine bestimmte Version eines Anwendungstyps nicht mehr benötigt, sollten Sie die Registrierung des Anwendungstyps mit dem Cmdlet [Unregister-ServiceFabricApplicationType](/powershell/servicefabric/vlatest/unregister-servicefabricapplicationtype) aufheben. Durch das Aufheben der Registrierung nicht verwendeter Anwendungstypen wird Speicherplatz freigegeben, der vom Imagespeicher verwendet wird. Die Registrierung eines Anwendungstyps kann nur aufgehoben werden, wenn keine Anwendungen für den Typ instanziiert sind und keine ausstehenden Anwendungsupgrades vorliegen, die auf den Typ verweisen.

Führen Sie [Get-ServiceFabricApplicationType](/powershell/servicefabric/vlatest/get-servicefabricapplicationtype) aus, um die derzeit im Cluster registrierten Anwendungstypen anzuzeigen:

```powershell
PS C:\> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
Status                 : Available
DefaultParameters      : { "Stateless1_InstanceCount" = "-1" }
```

Führen Sie [Unregister-ServiceFabricApplicationType](/powershell/servicefabric/vlatest/unregister-servicefabricapplicationtype) aus, um die Registrierung eines bestimmten Anwendungstyps aufzuheben:

```powershell
PS C:\> Unregister-ServiceFabricApplicationType MyApplicationType 1.0.0
```
## <a name="remove-an-application-package-from-the-image-store"></a>Entfernen eines Anwendungspakets aus dem Imagespeicher
Wenn ein Anwendungspaket nicht mehr benötigt wird, können Sie es aus dem Imagespeicher löschen, um Systemressourcen freizugeben.

```powershell
PS C:\>Remove-ServiceFabricApplicationPackage -ApplicationPackagePathInImageStore MyApplicationV1 -ImageStoreConnectionString (Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest))
```

## <a name="troubleshooting"></a>Problembehandlung
### <a name="copy-servicefabricapplicationpackage-asks-for-an-imagestoreconnectionstring"></a>Copy-ServiceFabricApplicationPackage fordert einen ImageStoreConnectionString an
Die Service Fabric-SDK-Umgebung sollte bereits mit den richtigen Standardeinstellungen eingerichtet sein. Wichtig ist, dass der ImageStoreConnectionString für alle Befehle mit dem vom Service Fabric-Cluster verwendeten Wert übereinstimmt. Sie finden die Imagespeicher-Verbindungszeichenfolge im Clustermanifest, das über den Befehl [Get-ServiceFabricClusterManifest](/powershell/servicefabric/vlatest/get-servicefabricclustermanifest) abgerufen wird:

```powershell
PS C:\> Get-ServiceFabricClusterManifest
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

