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
ms.sourcegitcommit: 6e0ad6b5bec11c5197dd7bded64168a1b8cc2fdd
ms.openlocfilehash: 4e3840f68c93998a52fa2956c2ea9d0976e0f627
ms.lasthandoff: 03/28/2017


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

Nachdem eine App bereitgestellt wurde und eine Instanz im Cluster ausgeführt wird, können Sie die App-Instanz und ihren Anwendungstyp löschen. Das vollständige Entfernen einer App aus dem Cluster umfasst die folgenden Schritte:

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
Beim Hochladen des Anwendungspakets wird das Paket an einem Speicherort gespeichert, an dem interne Service Fabric-Komponenten auf das Paket zugreifen können.
Wenn Sie das Anwendungspaket lokal überprüfen möchten, verwenden Sie das Cmdlet [Test-ServiceFabricApplicationPackage](/powershell/servicefabric/vlatest/test-servicefabricapplicationpackage).

Mit dem Befehl [Copy-ServiceFabricApplicationPackage](/powershell/servicefabric/vlatest/copy-servicefabricapplicationpackage) wird das Anwendungspaket in den Clusterimagespeicher hochgeladen.
Das Cmdlet **Get-ImageStoreConnectionStringFromClusterManifest** ist Teil des Service Fabric-SDK-PowerShell-Moduls und wird verwendet, um die Imagespeicher-Verbindungszeichenfolge abzurufen.  Um das SDK-Modul zu importieren, führen Sie Folgendes aus:

```powershell
Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
```

Angenommen, Sie erstellen und verpacken eine App namens *MyApplication* in Visual Studio. Der Name des Anwendungstyps ist in der Datei „ApplicationManifest.xml“ standardmäßig als „MyApplicationType“ aufgeführt.  Das Anwendungspaket mit den erforderlichen Anwendungs- und Dienstmanifesten sowie Code-/Konfigurations-/Datenpaketen befindet sich in *C:\Benutzer\Benutzername\Eigene Dokumente\Visual Studio 2015\Projects\MyApplication\MyApplication\pkg\Debug*. 

Mit dem folgenden Befehl werden die Inhalte des Anwendungspakets aufgelistet:

```powershell
PS C:\> $path = 'C:\Users\user\Documents\Visual Studio 2015\Projects\MyApplication\MyApplication\pkg\Debug'
PS C:\> tree /f $path
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

Wenn das Anwendungspaket groß ist und/oder viele Dateien enthält, können Sie es [komprimieren](service-fabric-package-apps.md#compress-a-package). Durch eine Komprimierung werden Größe und Anzahl der Dateien verringert.
Ein positiver Nebeneffekt dabei ist, dass die Registrierung und die Aufhebung der Registrierung des Anwendungstyps schneller erfolgen. Der Upload kann sich in diesem Zeitraum eventuell verlangsamen, insbesondere, wenn Sie die Zeit zur Komprimierung des Pakets berücksichtigen. 

Mit demselben Befehl [Copy-ServiceFabricApplicationPackage](/powershell/servicefabric/vlatest/copy-servicefabricapplicationpackage) wird das Paket komprimiert. Die Komprimierung kann getrennt vom Upload mithilfe des Flags `SkipCopy` oder beim Uploadvorgang durchgeführt werden. Die Komprimierung eines komprimierten Pakets ist nicht möglich.
Mit demselben Befehl [Copy-ServiceFabricApplicationPackage](/powershell/servicefabric/vlatest/copy-servicefabricapplicationpackage) mit dem Schalter `UncompressPackage` wird ein komprimiertes Paket dekomprimiert.

Mit dem folgenden Cmdlet wird das Paket komprimiert, ohne dass es im Abbildspeicher kopiert wird. Das Paket enthält nun ZIP-Dateien für die Pakete `Code` und `Config`. Die Anwendungs- und Dienstmanifeste werden nicht komprimiert, da sie für zahlreiche interne Vorgänge (z.B. Paketfreigabe, Extraktion des Namen und der Version des Anwendungstyps für bestimmte Überprüfungen) benötigt werden. Eine Komprimierung der Manifeste würde dazu führen, dass diese Vorgänge unwirksam werden.

```
PS C:\> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath $path -CompressPackage -SkipCopy
PS C:\> tree /f $path
Folder PATH listing for volume OSDisk
Volume serial number is 0459-2393
C:\USERS\USER\DOCUMENTS\VISUAL STUDIO 2015\PROJECTS\MYAPPLICATION\MYAPPLICATION\PKG\DEBUG
|   ApplicationManifest.xml
|
└───Stateless1Pkg
       Code.zip
       Config.zip
       ServiceManifest.xml
```

Die Komprimierung erfordert bei großen Anwendungspaketen Zeit. Um optimale Ergebnisse zu erzielen, sollten Sie ein schnelles SSD-Laufwerk verwenden. Die Komprimierungszeit und die Größe des komprimierten Pakets variieren zudem je nach dem Inhalt des Pakets.
Im Folgenden werden beispielhaft Komprimierungsstatistiken für einige Pakete aufgeführt, die die ursprüngliche und die komprimierte Paketgröße mit der Komprimierungszeit angeben.

|Ursprüngliche Größe (MB)|Dateianzahl|Komprimierungszeit|Größe des komprimierten Pakets (MB)|
|----------------:|---------:|---------------:|---------------------------:|
|100|100|00:00:03.3547592|60|
|512|100|00:00:16.3850303|307|
|1.024|500|00:00:32.5907950|615|
|2048|1000|00:01:04.3775554|1231|
|5012|100|00:02:45.2951288|3074|

Nachdem ein Paket komprimiert wurde, kann es bei Bedarf in einen oder in mehrere Service Fabric-Cluster hochgeladen werden. Für komprimierte und nicht komprimierte Pakete gilt dasselbe Bereitstellungsverfahren. Wenn das Paket komprimiert ist, wird es als solches im Clusterabbildspeicher gespeichert und vor Ausführung der Anwendung auf dem Knoten dekomprimiert.


Im folgenden Beispiel wird das Paket in den Imagespeicher in einen Ordner mit dem Namen „MyApplicationV1“ hochgeladen:

```powershell
PS C:\> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath $path -ApplicationPackagePathInImageStore MyApplicationV1 -ImageStoreConnectionString (Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest)) -TimeoutSec 1800
```

Wenn Sie den Parameter *-ApplicationPackagePathInImageStore* nicht angeben, wird das App-Paket in den Ordner „Debug“ im Imagespeicher kopiert.

Die Zeit zum Hochladen eines Pakets hängt von verschiedenen Faktoren ab. Zu einigen dieser Faktoren zählen die Anzahl der Dateien im Paket, die Paketgröße und die Dateigrößen. Die Netzwerkgeschwindigkeit zwischen dem Quellcomputer und dem Service Fabric-Cluster wirkt sich auch auf die Uploadzeit aus. Das Standardtimeout für [Copy-ServiceFabricApplicationPackage](/powershell/servicefabric/vlatest/copy-servicefabricapplicationpackage) beträgt 30 Minuten.
Abhängig von den beschriebenen Faktoren müssen Sie eventuell das Timeout erhöhen. Wenn Sie das Paket beim Aufrufen der Kopierfunktion komprimieren, müssen Sie auch die Zeit für die Komprimierung in Betracht ziehen.

Weitere Informationen zum Imagespeicher und zur Imagespeicher-Verbindungszeichenfolge finden Sie unter [Grundlegendes zur Imagespeicher-Verbindungszeichenfolge](service-fabric-image-store-connection-string.md).

## <a name="register-the-application-package"></a>Registrieren des Anwendungspakets
Der Anwendungstyp und die Version, der bzw. die im Anwendungsmanifest deklariert sind, werden beim Registrieren des Anwendungspakets verfügbar. Das System liest das im vorherigen Schritt hochgeladene Paket, überprüft es, verarbeitet den Inhalt des Pakets und kopiert das verarbeitete Paket an einen internen Systemspeicherort.  

Führen Sie das Cmdlet [Register ServiceFabricApplicationType](/powershell/servicefabric/vlatest/register-servicefabricapplicationtype) aus, um den Anwendungstyp im Cluster zu registrieren, und für die Bereitstellung verfügbar zu machen:

```powershell
PS C:\> Register-ServiceFabricApplicationType MyApplicationV1
Register application type succeeded
```

„MyApplicationV1“ ist der Ordner im Imagespeicher, in dem sich das App-Paket befindet. Der Anwendungstyp mit dem Namen „MyApplicationType“ und Version „1.0.0“ (beides befindet sich im Anwendungsmanifest) ist jetzt im Cluster registriert.

Der Befehl [Register-ServiceFabricApplicationType](/powershell/servicefabric/vlatest/register-servicefabricapplicationtype) wird erst zurückgegeben, wenn das Anwendungspaket vom System erfolgreich registriert wurde. Die Dauer des Registriervorgangs hängt von der Größe und dem Inhalt des Anwendungspakets ab. Verwenden Sie bei Bedarf den Parameter **-TimeoutSec**, wenn ein längeres Zeitlimit erforderlich ist (das Standardzeitlimit beträgt 60 Sekunden).

Wenn es sich um ein großes App-Paket handelt oder Timeouts auftreten, verwenden Sie den Parameter **-Async**. Der Befehl wird zurückgegeben, wenn der Cluster den Registrierungsbefehl akzeptiert. Die Verarbeitung wird bei Bedarf fortgesetzt.
Der Befehl [Get-ServiceFabricApplicationType](/powershell/servicefabric/vlatest/get-servicefabricapplicationtype) listet alle erfolgreich registrierten Anwendungstypversionen und deren Registrierungsstatus auf. Sie können mithilfe dieses Befehls ermitteln, wann die Registrierung abgeschlossen ist.

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

Weitere Informationen zum Imagespeicher und zur Imagespeicher-Verbindungszeichenfolge finden Sie unter [Grundlegendes zur Imagespeicher-Verbindungszeichenfolge](service-fabric-image-store-connection-string.md).

### <a name="deploy-large-application-package"></a>Bereitstellen eines großen Anwendungspakets
Problem: Bei einem großen Anwendungspaket (im GB-Bereich) tritt bei Verwendung von [Copy-ServiceFabricApplicationPackage](/powershell/servicefabric/vlatest/copy-servicefabricapplicationpackage) ein Timeout auf.
Versuchen Sie Folgendes:
- Geben Sie bei dem Befehl [Copy-ServiceFabricApplicationPackage](/powershell/servicefabric/vlatest/copy-servicefabricapplicationpackage) mit dem Parameter `TimeoutSec` einen höheren Timeoutwert an. Das Timeout beträgt standardmäßig 30 Minuten.
- Überprüfen Sie die Netzwerkverbindung zwischen Ihrem Quellcomputer und dem Cluster. Ziehen Sie bei einer langsamen Verbindung die Verwendung eines Computers mit einer besseren Netzwerkverbindung in Betracht.
Wenn sich der Clientcomputer in einem anderen Bereich als der Cluster befindet, sollten Sie einen Clientcomputer in einem näher gelegenen oder im selben Bereich wie den des Clusters verwenden.
- Überprüfen Sie, ob Sie von einer externen Drosselung betroffen sind. Wenn der Abbildspeicher beispielsweise für die Verwendung des Azure-Speichers konfiguriert ist, wird der Upload eventuell gedrosselt.

Problem: Das Paket wurde erfolgreich hochgeladen, allerdings tritt bei Verwendung von [Register-ServiceFabricApplicationType](/powershell/servicefabric/vlatest/register-servicefabricapplicationtype) ein Timeout auf.
Versuchen Sie Folgendes:
- [Komprimieren Sie das Paket](service-fabric-package-apps.md#compress-a-package), bevor Sie es in den Abbildspeicher kopieren.
Durch eine Komprimierung werden Größe und Anzahl der Dateien verringert, wodurch wiederum die Menge des Datenverkehrs und der Aufgaben, die das Service Fabric durchführen muss, reduziert wird. Der Upload kann sich eventuell verlangsamen (insbesondere, wenn Sie die Komprimierungszeit berücksichtigen), allerdings werden die Registrierung und die Aufhebung der Registrierung des Anwendungstyps schneller durchgeführt.
- Geben Sie bei dem Befehl [Register-ServiceFabricApplicationType](/powershell/servicefabric/vlatest/register-servicefabricapplicationtype) mit dem Parameter `TimeoutSec` einen höheren Timeoutwert an.
- Legen Sie den Schalter `Async` für [Register-ServiceFabricApplicationType](/powershell/servicefabric/vlatest/register-servicefabricapplicationtype) fest. Der Befehl wird zurückgegeben, wenn der Cluster den Befehl akzeptiert, und die Bereitstellung wird asynchron fortgesetzt.
Aus diesem Grund muss in diesem Fall kein höherer Timeoutwert angegeben werden.

### <a name="deploy-application-package-with-many-files"></a>Bereitstellen eines Anwendungspakets mit vielen Dateien
Problem: Bei einem Anwendungspaket mit vielen Dateien (im Tausenderbereich) tritt bei Verwendung von [Register-ServiceFabricApplicationType](/powershell/servicefabric/vlatest/register-servicefabricapplicationtype) ein Timeout auf.
Versuchen Sie Folgendes:
- [Komprimieren Sie das Paket](service-fabric-package-apps.md#compress-a-package), bevor Sie es in den Abbildspeicher kopieren. Durch eine Komprimierung wird die Anzahl der Dateien verringert.
- Geben Sie bei dem Befehl [Register-ServiceFabricApplicationType](/powershell/servicefabric/vlatest/register-servicefabricapplicationtype) mit dem Parameter `TimeoutSec` einen höheren Timeoutwert an.
- Legen Sie den Schalter `Async` für [Register-ServiceFabricApplicationType](/powershell/servicefabric/vlatest/register-servicefabricapplicationtype) fest. Der Befehl wird zurückgegeben, wenn der Cluster den Befehl akzeptiert, und die Bereitstellung wird asynchron fortgesetzt.
Aus diesem Grund muss in diesem Fall kein höherer Timeoutwert angegeben werden. 

## <a name="next-steps"></a>Nächste Schritte
[Service Fabric-Anwendungsupgrade](service-fabric-application-upgrade.md)

[Einführung in Service Fabric-Integrität](service-fabric-health-introduction.md)

[Diagnose und Problembehandlung von Service Fabric-Diensten](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Modellieren einer Anwendung in Service Fabric](service-fabric-application-model.md)

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-application-model.md
[11]: service-fabric-application-upgrade.md

