<properties
   pageTitle="Service Fabric-Anwendungsbereitstellung | Microsoft Azure"
   description="Bereitstellen und Entfernen von Anwendungen in Service Fabric"
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/25/2016"
   ms.author="ryanwi"/>

# Bereitstellen und Entfernen von Anwendungen mit PowerShell

> [AZURE.SELECTOR]
- [PowerShell](service-fabric-deploy-remove-applications.md)
- [Visual Studio](service-fabric-publish-app-remote-cluster.md)

<br/>

Sobald der [Anwendungstyp gepackt][10] wurde, ist die Anwendung für die Bereitstellung in einem Azure Service Fabric-Cluster bereit. Die Bereitstellung umfasst die folgenden drei Schritte:

1. Hochladen des Anwendungspakets
2. Registrierung des Anwendungstyps
3. Erstellen der Anwendungsinstanz

>[AZURE.NOTE] Wenn Sie Visual Studio zum Bereitstellen und Debuggen von Anwendungen in Ihrem lokalen Entwicklungscluster verwenden, werden alle folgenden Schritte automatisch über ein PowerShell-Skript im Ordner „Scripts“ des Anwendungsprojekts ausgeführt. In diesem Artikel wird die grundlegende Funktionsweise der Skripts erläutert, sodass Sie die gleichen Vorgänge außerhalb von Visual Studio ausführen können.

## Hochladen des Anwendungspakets

Beim Hochladen des Anwendungspakets wird das Paket an einem Speicherort gespeichert, an dem interne Service Fabric-Komponenten auf das Paket zugreifen können. Zum Hochladen können Sie PowerShell verwenden. Bevor Sie die in diesem Artikel aufgeführten Befehle ausführen, stellen Sie immer zuerst eine Verbindung zum Service Fabric-Cluster mit [Connect-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) her.

Angenommen, Sie haben einen Ordner namens *MyApplicationType*, der die erforderlichen Anwendungs- und Dienstmanifeste sowie Code-/Konfigurations-/Datenpakete enthält. Mit dem Befehl [Copy-ServiceFabricApplicationPackage](https://msdn.microsoft.com/library/mt125905.aspx) wird das Paket an den Clusterimagespeicher hochgeladen. Das Cmdlet **Get-ImageStoreConnectionStringFromClusterManifest** ist Teil des Service Fabric-SDK-PowerShell-Moduls und wird verwendet, um die Imagespeicher-Verbindungszeichenfolge abzurufen. Um das SDK-Modul zu importieren, führen Sie Folgendes aus:

```
Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
```

Sie können ein Anwendungspaket aus *C:\\users\\ryanwi\\Documents\\Visual Studio 2015\\Projects\\MyApplication\\myapplication\\pkg\\debug* in *c:\\temp\\MyApplicationType* kopieren (benennen Sie das Verzeichnis „debug“ in „MyApplicationType“ um). Im folgenden Beispiel wird das Paket hochgeladen:

~~~
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
    │   └───de-DE
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
~~~

## Registrieren des Anwendungspakets

Beim Registrieren des Anwendungspakets werden der Anwendungstyp und die Version im verfügbaren Anwendungsmanifest deklariert. Das System liest das im vorherigen Schritt hochgeladene Paket, überprüft es (entspricht der lokalen Ausführung von [Test-ServiceFabricApplicationPackage](https://msdn.microsoft.com/library/mt125950.aspx)), verarbeitet den Inhalt des Pakets und kopiert das verarbeitete Paket in einen internen Systemspeicherort.

~~~
PS D:\temp> Register-ServiceFabricApplicationType MyApplicationType
Register application type succeeded

PS D:\temp> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : AppManifestVersion1
DefaultParameters      : {}

PS D:\temp>
~~~

Der Befehl [Register-ServiceFabricApplicationType](https://msdn.microsoft.com/library/mt125958.aspx) wird erst zurückgegeben, wenn das Paket vom System erfolgreich kopiert wurde. Die Dauer des Kopiervorgangs hängt vom Inhalt des Anwendungspakets ab. Ggf. können Sie den Parameter **-TimeoutSec** verwenden, wenn ein längeres Zeitlimit erforderlich ist. (Das Standardzeitlimit beträgt 60 Sekunden.)

Der Befehl [Get-ServiceFabricApplicationType](https://msdn.microsoft.com/library/mt125871.aspx) listet alle erfolgreich registrierten Anwendungstypversionen auf.

## Erstellen der Anwendung

Sie können eine Anwendung mit einer beliebigen Version des Anwendungstyps instanziieren, die mit dem Befehl [New-ServiceFabricApplication](https://msdn.microsoft.com/library/mt125913.aspx) erfolgreich registriert wurde. Der Name jeder Anwendung muss mit dem *fabric:*-Schema beginnen und für jede Anwendungsinstanz eindeutig sein. Wenn im Anwendungsmanifest des Zielanwendungstyps Standarddienste festgelegt wurden, werden sie in diesem Schritt erstellt.

~~~
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
~~~

Der Befehl [Get-ServiceFabricApplication](https://msdn.microsoft.com/library/mt163515.aspx) listet alle erfolgreich erstellten Anwendungsinstanzen zusammen mit ihrem Gesamtzustand auf.

Der Befehl [Get-ServiceFabricService](https://msdn.microsoft.com/library/mt125889.aspx) listet alle Dienstinstanzen auf, die innerhalb einer bestimmten Anwendungsinstanz erfolgreich erstellt wurden. Die Standarddienste (sofern vorhanden) werden hier aufgelistet.

Für jede Version des registrierten Anwendungstyps können mehrere Anwendungsinstanzen erstellt werden. Jede Anwendungsinstanz wird isoliert mit einem eigenen Arbeitsverzeichnis und Prozess ausgeführt.

## Entfernen einer Anwendung

Wird eine Anwendungsinstanz nicht mehr benötigt, kann diese mit dem Befehl [Remove-ServiceFabricApplication](https://msdn.microsoft.com/library/mt125914.aspx) dauerhaft entfernt werden. Mit diesem Befehl werden auch alle Dienste automatisch entfernt, die mit der Anwendung verknüpft sind, d.h., der Dienstzustand wird vollständig und dauerhaft entfernt. Dieser Vorgang kann nicht rückgängig gemacht werden, und der Anwendungsstatus kann nicht wiederhergestellt werden.

~~~
PS D:\temp> Remove-ServiceFabricApplication fabric:/MyApp

Confirm
Continue with this operation?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
Remove application instance succeeded

PS D:\temp> Get-ServiceFabricApplication
PS D:\temp>
~~~

Wird eine bestimmte Version eines Anwendungstyps nicht mehr benötigt, heben Sie die Registrierung der Version mit dem Befehl [Unregister-ServiceFabricApplicationType](https://msdn.microsoft.com/library/mt125885.aspx) auf. Durch das Aufheben der Registrierung nicht verwendeter Typen wird Speicherplatz freigegeben, der von dem Inhalt des Anwendungspakets dieses Typs im Image Store verwendet wird. Die Registrierung eines Anwendungstyps kann nur aufgehoben werden, wenn keine Anwendungen für den Typ instanziiert sind und keine ausstehenden Anwendungsupgrades vorliegen, die auf den Typ verweisen.

~~~
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
~~~

## Problembehandlung

### Copy-ServiceFabricApplicationPackage fordert einen ImageStoreConnectionString an

Die Service Fabric-SDK-Umgebung sollte bereits mit den richtigen Standardeinstellungen eingerichtet sein. Wichtig ist, dass der ImageStoreConnectionString für alle Befehle mit dem vom Service Fabric-Cluster verwendeten Wert übereinstimmt. Der Wert ist im Clustermanifest enthalten, das mit dem Befehl [Get-ServiceFabricClusterManifest](https://msdn.microsoft.com/library/mt126024.aspx) abgerufen wird:

~~~
PS D:\temp> Copy-ServiceFabricApplicationPackage .\MyApplicationType

cmdlet Copy-ServiceFabricApplicationPackage at command pipeline position 1
Supply values for the following parameters:
ImageStoreConnectionString:

PS D:\temp> Get-ServiceFabricClusterManifest
<ClusterManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Name="Server-Default-SingleNode" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

    [...]

    <Section Name="Management">
      <Parameter Name="ImageStoreConnectionString" Value="file:D:\ServiceFabric\Data\ImageStore" />
    </Section>

    [...]

PS D:\temp> Copy-ServiceFabricApplicationPackage .\MyApplicationType -ImageStoreConnectionString file:D:\ServiceFabric\Data\ImageStore
Copy application package succeeded

PS D:\temp>
~~~

## Nächste Schritte

[Service Fabric-Anwendungsupgrade](service-fabric-application-upgrade.md)

[Einführung in Service Fabric-Integrität](service-fabric-health-introduction.md)

[Diagnose und Problembehandlung von Service Fabric-Diensten](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Modellieren einer Anwendung in Service Fabric](service-fabric-application-model.md)

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-application-model.md
[11]: service-fabric-application-upgrade.md

<!---HONumber=AcomDC_0928_2016-->