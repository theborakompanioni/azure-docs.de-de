---
title: Paketieren einer Azure Service Fabric-Anwendung | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie eine Service Fabric-Anwendung vor der Bereitstellung in einem Cluster paketieren.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: mani-ramaswamy
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 7/27/2017
ms.author: ryanwi
ms.translationtype: Human Translation
ms.sourcegitcommit: 3716c7699732ad31970778fdfa116f8aee3da70b
ms.openlocfilehash: 48e4ad774164b87d0cacb42f709e54af1d6f07b9
ms.contentlocale: de-de
ms.lasthandoff: 06/30/2017

---
# <a name="package-an-application"></a>Packen einer Anwendung
In diesem Artikel wird beschrieben, wie eine Service Fabric-Anwendung paketiert und für die Bereitstellung vorbereitet wird.

## <a name="package-layout"></a>Paketlayout
Das Anwendungsmanifest, ein oder mehrere Dienstmanifeste und andere erforderliche Paketdateien müssen in einem bestimmten Layout für die Bereitstellung in einem Service Fabric-Cluster angeordnet werden. Die Beispielmanifeste in diesem Artikel müssen in der folgenden Verzeichnisstruktur angeordnet werden:

```
PS D:\temp> tree /f .\MyApplicationType

D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
    │   ServiceManifest.xml
    │
    ├───MyCode
    │       MyServiceHost.exe
    │
    ├───MyConfig
    │       Settings.xml
    │
    └───MyData
            init.dat
```

Die Ordner werden nach den **Name** -Attributen der jeweils zugehörigen Elemente benannt. Wenn das Dienstmanifest beispielsweise zwei Codepakete mit den Namen **MyCodeA** und **MyCodeB** umfasst, enthalten zwei Ordner mit denselben Namen die jeweils erforderlichen Binärdateien für jedes Codepaket.

## <a name="use-setupentrypoint"></a>Verwenden von SetupEntryPoint
Gängige Szenarios für die Verwendung von **SetupEntryPoint** sind die Anforderung, eine ausführbare Datei vor dem Starten des Diensts auszuführen, sowie die Anforderung, einen Vorgang mit erhöhten Rechten auszuführen. Beispiel:

* Einrichten und Initialisieren von Umgebungsvariablen, die die ausführbare Datei des Diensts benötigt. Dies ist nicht auf Dateien beschränkt, die mit den Service Fabric-Programmiermodellen geschrieben wurden. „npm.exe“ benötigt beispielsweise einige Umgebungsvariablen, die zum Bereitstellen einer node.js-Anwendung konfiguriert wurden.
* Einrichten einer Zugriffssteuerung durch Installieren von Sicherheitszertifikaten.

Weitere Informationen zum Konfigurieren von **SetupEntryPoint** finden Sie unter [Konfigurieren der Richtlinie für einen Setupeinstiegspunkt für Dienste](service-fabric-application-runas-security.md).

<a id="Package-App"></a>
## <a name="configure"></a>Konfigurieren
### <a name="build-a-package-by-using-visual-studio"></a>Erstellen eines Pakets mit Visual Studio
Wenn Sie Ihre Anwendung mithilfe von Visual Studio 2015 erstellen, können Sie über den Befehl "Paket" automatisch ein Paket erstellen, das dem oben beschriebenen Layout entspricht.

Klicken Sie zum Erstellen eines Pakets im Projektmappen-Explorer mit der rechten Maustaste auf das Anwendungsprojekt, und wählen Sie wie unten gezeigt den Befehl „Paket“ aus:

![Paketieren einer Anwendung mit Visual Studio][vs-package-command]

Nach Abschluss der Paketerstellung wird der Speicherort des Pakets im **Ausgabefenster** angezeigt. Die Paketerstellung erfolgt automatisch, wenn Sie die Anwendung in Visual Studio bereitstellen oder debuggen.

### <a name="build-a-package-by-command-line"></a>Erstellen eines Pakets über die Befehlszeile
Es ist auch möglich, Ihre Anwendung mit `msbuild.exe` programmgesteuert zu packen. Diese Datei wird auch von Visual Studio im Hintergrund ausgeführt, sodass die Ausgabe identisch ist.

```shell
D:\Temp> msbuild HelloWorld.sfproj /t:Package
```

## <a name="test-the-package"></a>Testen des Pakets
Sie können die Paketstruktur lokal über PowerShell mit dem [Test-ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) -Befehl überprüfen.
Dieser Befehl sucht nach Problemen bei der Manifestanalyse und überprüft alle Verweise. Mit diesem Befehl wird nur die strukturelle Richtigkeit der Verzeichnisse und Dateien im Paket überprüft.
Es werden keine Inhalte der Code- oder Datenpakete überprüft, mit Ausnahme der Prüfung, ob alle erforderlichen Dateien vorhanden sind.

```
PS D:\temp> Test-ServiceFabricApplicationPackage .\MyApplicationType
False
Test-ServiceFabricApplicationPackage : The EntryPoint MySetup.bat is not found.
FileName: C:\Users\servicefabric\AppData\Local\Temp\TestApplicationPackage_7195781181\nrri205a.e2h\MyApplicationType\MyServiceManifest\ServiceManifest.xml
```

Dieser Fehler gibt an, dass die Datei *MySetup.bat* , auf die im Dienstmanifest **SetupEntryPoint** verwiesen wird, im Codepaket fehlt. Nach dem Hinzufügen der fehlenden Datei wird die Überprüfung der Anwendung erfolgreich abgeschlossen:

```
PS D:\temp> tree /f .\MyApplicationType

D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
    │   ServiceManifest.xml
    │
    ├───MyCode
    │       MyServiceHost.exe
    │       MySetup.bat
    │
    ├───MyConfig
    │       Settings.xml
    │
    └───MyData
            init.dat

PS D:\temp> Test-ServiceFabricApplicationPackage .\MyApplicationType
True
PS D:\temp>
```

Wenn [Anwendungsparameter](service-fabric-manage-multiple-environment-app-configuration.md) in Ihrer Anwendung definiert sind, können Sie diese zwecks ordnungsgemäßer Überprüfung an [Test ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) übergeben.

Wenn Sie wissen, in welchem Cluster die Anwendung bereitgestellt wird, empfiehlt es sich, diese Angabe im Parameter `ImageStoreConnectionString` zu übergeben. In diesem Fall wird das Paket auch anhand älterer Versionen der Anwendung, die bereits im Cluster ausgeführt werden, überprüft. Beispielsweise kann bei der Überprüfung festgestellt werden, ob bereits ein Paket mit derselben Version, jedoch mit einem anderen Inhalt bereitgestellt wurde.  

Sobald die Anwendung korrekt paketiert wurde und die Überprüfung bestanden hat, beurteilen Sie basierend auf der Größe und Anzahl der Dateien, ob eine Komprimierung erforderlich ist.

## <a name="compress-a-package"></a>Komprimieren eines Pakets
Wenn ein Paket groß ist oder viele Dateien enthält, können Sie es zur schnelleren Bereitstellung komprimieren. Durch eine Komprimierung werden die Anzahl der Dateien und die Größe des Pakets verringert.
Das [Hochladen komprimierter Anwendungspakete](service-fabric-deploy-remove-applications.md#upload-the-application-package) kann länger dauern als das Hochladen nicht komprimierter Pakete (besonders, wenn die für die Komprimierung benötigte Zeit mit eingerechnet wird), aber das [Registrieren](service-fabric-deploy-remove-applications.md#register-the-application-package) und das [Aufheben der Registrierung des Anwendungstyps](service-fabric-deploy-remove-applications.md#unregister-an-application-type) erfolgen bei einem komprimierten Paket schneller.

Für komprimierte und nicht komprimierte Pakete gilt dasselbe Bereitstellungsverfahren. Wenn das Paket komprimiert ist, wird es als solches im Clusterabbildspeicher gespeichert und vor Ausführung der Anwendung auf dem Knoten dekomprimiert.
Bei der Komprimierung wird das gültige Service Fabric-Paket durch die komprimierte Version ersetzt. Der Ordner muss Schreibberechtigungen zulassen. Wird ein bereits komprimiertes Paket komprimiert, werden keine Änderungen vorgenommen.

Sie können ein Paket komprimieren, indem Sie den Powershell-Befehl [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) mit dem Schalter `CompressPackage` ausführen. Das Paket kann anhand desselben Befehls mit dem Schalter `UncompressPackage` dekomprimiert werden.

Mit dem folgenden Befehl wird das Paket komprimiert, ohne dass es im Abbildspeicher kopiert wird. Mit [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) ohne `SkipCopy`-Flag können Sie ein komprimiertes Paket bei Bedarf in ein oder mehrere Service Fabric-Cluster kopieren.
Das Paket enthält nun ZIP-Dateien für die Pakete `code`, `config` und `data` . Das Anwendungsmanifest und die Dienstmanifeste werden nicht komprimiert, da sie für zahlreiche interne Vorgänge (z.B. Paketfreigabe, Extraktion des Namen und der Version des Anwendungstyps für bestimmte Überprüfungen) benötigt werden.
Eine Komprimierung der Manifeste würde dazu führen, dass diese Vorgänge unwirksam werden.

```
PS D:\temp> tree /f .\MyApplicationType

D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
    │   ServiceManifest.xml
    │
    ├───MyCode
    │       MyServiceHost.exe
    │       MySetup.bat
    │
    ├───MyConfig
    │       Settings.xml
    │
    └───MyData
            init.dat
PS D:\temp> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath .\MyApplicationType -CompressPackage -SkipCopy

PS D:\temp> tree /f .\MyApplicationType

D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
       ServiceManifest.xml
       MyCode.zip
       MyConfig.zip
       MyData.zip

```

Alternativ können Sie das Paket in einem Schritt mit [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) komprimieren und kopieren.
Wenn es sich um ein großes Paket handelt, legen Sie ein ausreichend langes Timeout fest, um Zeit für die Komprimierung des Pakets und den Upload in das Cluster bereitzustellen.
```
PS D:\temp> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath .\MyApplicationType -ApplicationPackagePathInImageStore MyApplicationType -ImageStoreConnectionString fabric:ImageStore -CompressPackage -TimeoutSec 5400
```

Intern berechnet das Service Fabric-Paket zur Überprüfung Prüfsummen für die Anwendungspakete. Bei der Komprimierung werden die Prüfsummen für die komprimierten Versionen der einzelnen Pakete berechnet.
Wenn Sie eine nicht komprimierte Version des Anwendungspakets kopiert haben und dieses Paket komprimieren möchten, müssen Sie die Versionen der Pakete `code`, `config` und `data` ändern, um einen Prüfsummenkonflikt zu vermeiden. Wenn die Pakete unverändert sind, können Sie [diff provisioning](service-fabric-application-upgrade-advanced.md) verwenden, anstatt die Version zu ändern. Beziehen Sie bei dieser Option nicht das unveränderte Paket ein, sondern verweisen Sie vom Dienstmanifest aus darauf.

Gleichermaßen gilt: Wenn Sie eine komprimierte Version des Pakets hochgeladen haben und ein unkomprimiertes Paket verwenden möchten, müssen Sie die Versionen aktualisieren, um den Prüfsummenkonflikt zu vermeiden.

Das Paket ist nun ordnungsgemäß verpackt, überprüft und ggf. komprimiert. Somit ist es für die [Bereitstellung](service-fabric-deploy-remove-applications.md) in einem oder in mehreren Service Fabric-Clustern bereit.

### <a name="compress-packages-when-deploying-using-visual-studio"></a>Komprimieren Sie Pakete, wenn Sie mit Visual Studio bereitstellen
Sie können Visual Studio anweisen, Pakete bei der Bereitstellung zu komprimieren, indem Sie das `CopyPackageParameters`-Element an ihr Veröffentlichungsprofil senden und das `CompressPackage`-Attribut auf `true` festlegen.

``` xml
    <PublishProfile xmlns="http://schemas.microsoft.com/2015/05/fabrictools">
        <ClusterConnectionParameters ConnectionEndpoint="mycluster.westus.cloudapp.azure.com" />
        <ApplicationParameterFile Path="..\ApplicationParameters\Cloud.xml" />
        <CopyPackageParameters CompressPackage="true"/>
    </PublishProfile>
```

## <a name="next-steps"></a>Nächste Schritte
[Bereitstellen und Entfernen von Anwendungen][10] beschreibt, wie Sie mit PowerShell Anwendungsinstanzen verwalten.

[Verwalten von Anwendungsparametern für mehrere Umgebungen][11] beschreibt, wie Parameter und Umgebungsvariablen für verschiedene Anwendungsinstanzen konfiguriert werden.

[Konfigurieren von Sicherheitsrichtlinien für Ihre Anwendung][12] beschreibt, wie Sie Dienste unter Sicherheitsrichtlinien ausführen, um den Zugriff einzuschränken.

<!--Image references-->
[vs-package-command]: ./media/service-fabric-package-apps/vs-package-command.png

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-deploy-remove-applications.md
[11]: service-fabric-manage-multiple-environment-app-configuration.md
[12]: service-fabric-application-runas-security.md

