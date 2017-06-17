---
title: Automatisieren der Azure Service Fabric-Anwendungsverwaltung | Microsoft-Dokumentation
description: Bereitstellen, Aktualisieren, Testen und Entfernen von Service Fabric-Anwendungen mithilfe von PowerShell.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: f03f4294-571d-4262-8a77-cc8b481b959d
ms.service: service-fabric
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 06/16/2017
ms.author: ryanwi
redirect_url: /azure/service-fabric/service-fabric-deploy-remove-applications
ms.translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: 4e4793d35974268eac8272f85f78132bc69f7913
ms.contentlocale: de-de
ms.lasthandoff: 04/27/2017

---
# <a name="automate-the-application-lifecycle-using-powershell"></a>Automatisieren des Anwendungslebenszyklus mithilfe von PowerShell
Viele Aspekte des [Service Fabric-Anwendungslebenszyklus](service-fabric-application-lifecycle.md) können automatisiert werden.  Dieser Artikel veranschaulicht die Automatisierung allgemeiner Aufgaben zum Bereitstellen, Aktualisieren, Entfernen und Testen von Service Fabric-Anwendungen mithilfe von PowerShell.  Verwaltete und HTTP-APIs für die App-Verwaltung sind auch verfügbar. Weitere Informationen finden Sie unter [Lebenszyklus der Service Fabric-Anwendung](service-fabric-application-lifecycle.md) .  

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie mit den Aufgaben in diesem Artikel beginnen, müssen Sie Folgendes sicherstellen:

* Machen Sie sich mit den unter [Technische Übersicht über Service Fabric](service-fabric-technical-overview.md)beschriebenen Service Fabric-Konzepten vertraut.
* [Installieren Sie die Runtime, das SDK und die Tools](service-fabric-get-started.md). Dabei wird auch das PowerShell-Modul **ServiceFabric** installiert.
* [Aktivieren Sie die PowerShell-Skriptausführung](service-fabric-get-started.md#enable-powershell-script-execution).
* Starten Sie einen lokalen Cluster.  Öffnen Sie ein neues PowerShell-Fenster als Administrator, und führen Sie das Clustersetupskript aus dem SDK-Ordner aus: `& "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1"`
* Bevor Sie die in diesem Artikel aufgeführten PowerShell-Befehle ausführen, stellen Sie zuerst mit [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) eine Verbindung mit dem lokalen Service Fabric-Cluster her: `Connect-ServiceFabricCluster localhost:19000`
* Die folgenden Aufgaben erfordern ein Anwendungspaket der Version 1 für die Bereitstellung und ein Anwendungspaket der Version 2 für das Upgrade. Laden Sie die [**WordCount**-Beispielanwendung](http://aka.ms/servicefabricsamples) (aus den Beispielen für die ersten Schritte) herunter. Erstellen und packen Sie die Anwendung in Visual Studio, indem Sie im Projektmappen-Explorer mit der rechten Maustaste auf **WordCount** klicken und **Paket** auswählen. Kopieren Sie das Paket der Version 1 in `C:\ServiceFabricSamples\Services\WordCount\WordCount\pkg\Debug` nach `C:\Temp\WordCount`. Kopieren Sie `C:\Temp\WordCount` nach `C:\Temp\WordCountV2`, um das Anwendungspaket der Version 2 für das Upgrade zu erstellen. Öffnen Sie `C:\Temp\WordCountV2\ApplicationManifest.xml` in einem Text-Editor. Ändern Sie im **ApplicationManifest**-Element das **ApplicationTypeVersion**-Attribut von „1.0.0“ in „2.0.0“, um die App-Versionsnummer zu aktualisieren. Speichern Sie die geänderte Datei „ApplicationManifest.xml“.

## <a name="task-deploy-a-service-fabric-application"></a>Aufgabe: Bereitstellen einer Service Fabric-Anwendung
Nachdem Sie die Anwendung erstellt und gepackt (oder das Anwendungspaket heruntergeladen) haben, können Sie die Anwendung in einem lokalen Service Fabric-Cluster bereitstellen. Die Bereitstellung umfasst das Hochladen des Anwendungspakets, das Registrieren des Anwendungstyps und das Erstellen der Anwendungsinstanz. Verwenden Sie die Anweisungen in diesem Abschnitt, um eine neue Anwendung in einem Cluster bereitzustellen.

### <a name="step-1-upload-the-application-package"></a>Schritt 1: Hochladen des Anwendungspakets
Beim Hochladen des Anwendungspakets in den ImageStore wird das Paket an einem Speicherort gespeichert, an dem interne Service Fabric-Komponenten auf das Paket zugreifen können.  Das Anwendungspaket enthält die erforderlichen Anwendungs- und Dienstmanifeste sowie Code-, Konfigurations- und Datenpakete zum Erstellen der Anwendungs- und Dienstinstanzen. Wenn Sie das App-Paket lokal überprüfen möchten, verwenden Sie das Cmdlet [Test-ServiceFabricApplicationPackage](/powershell/servicefabric/vlatest/test-servicefabricapplicationpackage).  Mit dem Befehl [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) wird das Paket hochgeladen. Beispiel:

```powershell
Copy-ServiceFabricApplicationPackage C:\Temp\WordCount\ -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCount
```

### <a name="step-2-register-the-application-type"></a>Schritt 2: Registrieren des Anwendungstyps
Beim Registrieren des Anwendungspakets werden der Anwendungstyp und die Version im verfügbaren Anwendungsmanifest deklariert. Das System liest das in Schritt 1 hochgeladene Paket, überprüft es (entspricht der lokalen Ausführung von [Test-ServiceFabricApplicationPackage](/powershell/servicefabric/vlatest/test-servicefabricapplicationpackage)), verarbeitet den Inhalt des Pakets und kopiert das verarbeitete Paket an einen internen Systemspeicherort.  Führen Sie das Cmdlet [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) aus:

```powershell
Register-ServiceFabricApplicationType WordCount
```
Um alle im Cluster registrierten Anwendungstypen anzuzeigen, führen Sie das Cmdlet [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) aus:

```powershell
Get-ServiceFabricApplicationType
```

### <a name="step-3-create-the-application-instance"></a>Schritt 3: Erstellen der Anwendungsinstanz
Eine Anwendung kann mit einer beliebigen Version des Anwendungstyps instanziiert werden, die mit dem Befehl [New-ServiceFabricApplication](/powershell/module/servicefabric/new-servicefabricapplication?view=azureservicefabricps) erfolgreich registriert wurde. Der Name jeder Anwendung wird bei der Bereitstellung deklariert. Er muss mit dem **fabric:**-Schema beginnen und für jede Anwendungsinstanz eindeutig sein. Der Name und die Version des Anwendungstyps werden in der Datei **ApplicationManifest.xml** des Anwendungspakets deklariert. Wenn im Anwendungsmanifest des Zielanwendungstyps Standarddienste festgelegt wurden, werden diese in diesem Schritt erstellt.

```powershell
New-ServiceFabricApplication fabric:/WordCount WordCount 1.0.0
```

Der Befehl [Get-ServiceFabricApplication](/powershell/servicefabric/vlatest/get-servicefabricapplication) listet alle erfolgreich erstellten Anwendungsinstanzen zusammen mit ihrem Gesamtzustand auf. Der Befehl [Get-ServiceFabricService](/powershell/module/servicefabric/get-servicefabricservice?view=azureservicefabricps) listet alle Dienstinstanzen auf, die innerhalb einer bestimmten Anwendungsinstanz erfolgreich erstellt wurden. Die Standarddienste (sofern vorhanden) werden aufgelistet.

```powershell
Get-ServiceFabricApplication

Get-ServiceFabricApplication | Get-ServiceFabricService
```

## <a name="task-upgrade-a-service-fabric-application"></a>Aufgabe: Aktualisieren einer Service Fabric-Anwendung
Sie können eine zuvor bereitgestellte Service Fabric-Anwendung mit einem Anwendungspaket aktualisieren. Mit dieser Aufgabe wird die WordCount -Anwendung aktualisiert, die unter „Aufgabe: Bereitstellen einer Service Fabric-Anwendung“ bereitgestellt wurde. Weitere Informationen finden Sie unter [Service Fabric-Anwendungsupgrade](service-fabric-application-upgrade.md) .

Um dieses Beispiel einfach zu halten, wurde nur die Versionsnummer der Anwendung im Anwendungspaket WordCountV2 aktualisiert, das bei den Voraussetzungen erstellt wurde. Ein realistischeres Szenario würde eine Aktualisierung der Dienstcode-, Konfigurations- oder Datendateien sowie das Packen der Anwendung mit aktualisierten Versionsnummern beinhalten.  

### <a name="step-1-upload-the-updated-application-package"></a>Schritt 1: Hochladen des aktualisierten Anwendungspakets
Die WordCount-Anwendung der Version 1 kann nun aktualisiert werden. Wenn Sie ein PowerShell-Fenster als Administrator öffnen und [**Get-ServiceFabricApplication**](/powershell/module/servicefabric/get-servicefabricapplication?view=azureservicefabricps) eingeben, wird angezeigt, dass Version 1.0.0 des WordCount-Anwendungstyps bereitgestellt wurde.  

Kopieren Sie jetzt das aktualisierte Anwendungspaket in den Service Fabric-ImageStore (in dem die Anwendungspakete von Service Fabric gespeichert werden). Der **ApplicationPackagePathInImageStore** -Parameter informiert Service Fabric darüber, wo sich das Anwendungspaket befindet. Der folgende Befehl kopiert das Anwendungspaket nach **WordCountV2** im Imagespeicher:  

```powershell
Copy-ServiceFabricApplicationPackage C:\Temp\WordCountV2\ -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCountV2

```
### <a name="step-2-register-the-updated-application-type"></a>Schritt 2: Registrieren des aktualisierten Anwendungstyps
Im nächsten Schritt wird die neue Version der Anwendung bei Service Fabric registriert. Dazu kann das Cmdlet [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) verwendet werden:

```powershell
Register-ServiceFabricApplicationType WordCountV2
```

### <a name="step-3-start-the-upgrade"></a>Schritt 3: Starten des Upgrades
Auf Anwendungsupgrades können verschiedene Upgradeparameter, Timeoutwerte und Integritätskriterien angewendet werden. Weitere Informationen finden Sie in den Dokumenten [Parameter für Anwendungsupgrades](service-fabric-application-upgrade-parameters.md) und [Service Fabric-Anwendungsupgrade](service-fabric-application-upgrade.md). Alle Dienste und Instanzen sollten nach dem Upgrade *fehlerfrei* sein.  Legen Sie **HealthCheckStableDuration** auf 60 Sekunden fest (sodass die Dienste mindestens 20 Sekunden fehlerfrei sind, bevor zur nächsten Upgradedomäne gewechselt wird).  Wir legen außerdem **UpgradeDomainTimeout** auf 1200 Sekunden und **UpgradeTimeout** auf 3000 Sekunden fest. Schließlich legen wir für **UpgradeFailureAction** den Wert **rollback** fest, sodass Service Fabric die Anwendung auf die vorherige Version zurücksetzt, wenn während des Upgrades Fehler erkannt werden.

Nun können Sie das Anwendungsupgrade mithilfe des Cmdlets [Start ServiceFabricApplicationUpgrade](/powershell/module/servicefabric/start-servicefabricapplicationupgrade?view=azureservicefabricps) starten:

```powershell
Start-ServiceFabricApplicationUpgrade -ApplicationName fabric:/WordCount -ApplicationTypeVersion 2.0.0 -HealthCheckStableDurationSec 60 -UpgradeDomainTimeoutSec 1200 -UpgradeTimeout 3000  -FailureAction Rollback -Monitored
```

Beachten Sie, dass der Name der Anwendung dem Namen der zuvor bereitgestellten v1.0.0-Anwendung entspricht (fabric:/WordCount). Service Fabric verwendet diesen Namen, um die zu aktualisierende Anwendung zu ermitteln. Wenn Sie die Timeoutwerte zu kurz festlegen, wird möglicherweise eine Fehlermeldung zu den Timeoutwerten angezeigt. Lesen Sie [Problembehandlung bei Anwendungsupgrades](service-fabric-application-upgrade-troubleshooting.md), oder erhöhen Sie die Timeoutwerte.

### <a name="step-4-check-upgrade-progress"></a>Schritt 4: Überprüfen des Upgradevorgangs
Sie können den Fortschritt des Anwendungsupgrades mit [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) oder dem Cmdlet [Get-ServiceFabricApplicationUpgrade](/powershell/module/servicefabric/get-servicefabricapplicationupgrade?view=azureservicefabricps) überwachen:

```powershell
Get-ServiceFabricApplicationUpgrade fabric:/WordCount
```

In wenigen Minuten sollte das Cmdlet [Get-ServiceFabricApplicationUpgrade](/powershell/module/servicefabric/get-servicefabricapplicationupgrade?view=azureservicefabricps) anzeigen, dass alle Upgradedomänen aktualisiert (abgeschlossen) wurden.

## <a name="task-test-a-service-fabric-application"></a>Aufgabe: Testen einer Service Fabric-Anwendung
Um hochwertige Dienste schreiben zu können, müssen Entwickler dazu in der Lage sein, Fehler in unzuverlässigen Infrastrukturen auszulösen, um die Stabilität ihrer Dienste testen zu können. Service Fabric ermöglicht Entwicklern das Auslösen von Fehleraktionen, um das Verhalten von Diensten beim Auftreten von Fehlern mit Chaos- und Failovertestszenarien zu testen.  Weitere Informationen finden Sie unter [Einführung in den Fault Analysis Service](service-fabric-testability-overview.md).

### <a name="step-1-run-the-chaos-test-scenario"></a>Schritt 1: Ausführen des Chaostestszenarios
Beim Chaosszenario werden Fehler im gesamten Service Fabric-Cluster generiert. Fehler, die normalerweise in Zeiträumen von mehreren Monaten oder Jahren auftreten, werden auf wenige Stunden komprimiert. Bei dieser Kombination aus überlappenden Fehlern mit hoher Fehlerrate werden auch Spezialfälle erkannt, die sonst nicht auffallen würden. Im folgenden Beispiel wird das Chaostestszenario für 60 Minuten ausgeführt.

```powershell
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$concurrentFaults = 3
$waitTimeBetweenIterationsSec = 60

Invoke-ServiceFabricChaosTestScenario -TimeToRunMinute $timeToRun -MaxClusterStabilizationTimeoutSec $maxStabilizationTimeSecs -MaxConcurrentFaults $concurrentFaults -EnableMoveReplicaFaults -WaitTimeBetweenIterationsSec $waitTimeBetweenIterationsSec
```

### <a name="step-2-run-the-failover-test-scenario"></a>Schritt 2: Ausführen des Failovertestszenarios
Das Failovertestszenario zielt auf eine bestimmte Dienstpartition ab, statt auf den gesamten Cluster. Andere Dienste sind davon nicht betroffen. Bei diesem Szenario werden eine Sequenz von simulierten Fehlern und die Dienstüberprüfung durchlaufen, während Ihre Geschäftslogik ausgeführt wird. Ein Fehler bei der Dienstüberprüfung weist auf ein Problem hin, das genauer untersucht werden muss. Beim Failovertest wird nur jeweils ein Fehler ausgelöst, während beim Chaostestszenario mehrere Fehler ausgelöst werden können. Im folgenden Beispiel wird der Failovertest für 60 Minuten für den fabric:/WordCount/WordCountService-Dienst ausgeführt.

```powershell
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$waitTimeBetweenFaultsSec = 10
$serviceName = "fabric:/WordCount/WordCountService"

Invoke-ServiceFabricFailoverTestScenario -TimeToRunMinute $timeToRun -MaxServiceStabilizationTimeoutSec $maxStabilizationTimeSecs -WaitTimeBetweenFaultsSec $waitTimeBetweenFaultsSec -ServiceName $serviceName -PartitionKindUniformInt64 -PartitionKey 1
```

## <a name="task-remove-a-service-fabric-application"></a>Aufgabe: Entfernen einer Service Fabric-Anwendung
Sie können eine Instanz einer bereitgestellten Anwendung löschen, den bereitgestellten Anwendungstyp aus dem Cluster entfernen und das Anwendungspaket aus dem ImageStore entfernen.

### <a name="step-1-remove-an-application-instance"></a>Schritt 1: Entfernen einer Anwendungsinstanz
Wird eine Anwendungsinstanz nicht mehr benötigt, können Sie diese mit dem Cmdlet [Remove-ServiceFabricApplication](/powershell/module/servicefabric/remove-servicefabricapplication?view=azureservicefabricps) dauerhaft entfernen. Hierbei werden auch alle Dienste automatisch entfernt, die mit der Anwendung verknüpft sind, d.h., der Dienstzustand wird vollständig und dauerhaft entfernt. Dieser Vorgang kann nicht rückgängig gemacht werden, und der Anwendungsstatus kann nicht wiederhergestellt werden.

```powershell
Remove-ServiceFabricApplication fabric:/WordCount
```

### <a name="step-2-unregister-the-application-type"></a>Schritt 2: Aufheben der Registrierung des Anwendungstyps
Wenn Sie eine bestimmte Version eines Anwendungstyps nicht mehr benötigen, heben Sie die Registrierung der Version mit dem Cmdlet [Unregister-ServiceFabricApplicationType](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) auf. Durch das Aufheben der Registrierung nicht verwendeter Typen wird Speicherplatz freigegeben, der vom Anwendungspaket im Imagespeicher verwendet wird. Die Registrierung eines Anwendungstyps kann nur aufgehoben werden, wenn keine Anwendungen für den Typ instanziiert sind und keine ausstehenden Anwendungsupgrades vorliegen, die auf den Typ verweisen.

```powershell
Unregister-ServiceFabricApplicationType WordCount 1.0.0
```

### <a name="step-3-remove-the-application-package"></a>Schritt 3: Entfernen des Anwendungspakets
Nachdem die Registrierung des Anwendungstyps aufgehoben wurde, kann das Anwendungspaket mit dem Cmdlet [Remove-ServiceFabricApplicationPackage](/powershell/module/servicefabric/remove-servicefabricapplicationpackage?view=azureservicefabricps) aus dem Imagespeicher entfernt werden.

```powershell
Remove-ServiceFabricApplicationPackage -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCount
```

## <a name="next-steps"></a>Nächste Schritte
[Service Fabric-Anwendungslebenszyklus](service-fabric-application-lifecycle.md)

[Bereitstellen von Anwendungen](service-fabric-deploy-remove-applications.md)

[Anwendungsupgrade](service-fabric-application-upgrade.md)

[Azure Service Fabric-Cmdlets](/powershell/azure/overview?view=azureservicefabricps)


