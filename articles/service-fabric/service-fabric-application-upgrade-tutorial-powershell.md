<properties
   pageTitle="Service Fabric-Anwendungsupgrade mit PowerShell | Microsoft Azure"
   description="Dieser Artikel bietet eine exemplarische Vorgehensweise für das Bereitstellen einer Service Fabric-Anwendung, Ändern des Codes und Einführen eines Upgrades mithilfe von PowerShell."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/14/2016"
   ms.author="subramar"/>




# Service Fabric-Anwendungsupgrade mithilfe von PowerShell

Die am häufigsten angewendete und empfohlene Methode für Upgrades ist das überwachte parallele Upgrade. Azure Service Fabric überwacht die Integrität der aktualisierten Anwendung basierend auf einer Reihe von Integritätsrichtlinien. Wenn für eine Updatedomäne (UD) ein Upgrade erfolgt ist, prüft Service Fabric die Anwendungsintegrität und ob mit der nächsten Updatedomäne fortgefahren wird oder ob basierend auf den Integritätsrichtlinien ein Upgradefehler vorliegt.

Ein überwachtes Anwendungsupgrade kann mithilfe der verwalteten oder systemeigenen APIs, PowerShell oder REST ausgeführt werden. Eine Anleitung zum Durchführen eines Upgrades mit Visual Studio finden Sie unter [Upgrade Ihrer Anwendung mit Visual Studio](service-fabric-application-upgrade-tutorial.md).

Mit den von Service Fabric überwachten parallelen Upgrades kann der Anwendungsadministrator die Integritätsevaluierungsrichtlinie konfigurieren, mit der Service Fabric ermittelt, ob die Anwendung fehlerfrei ist. Darüber hinaus kann der Administrator auch die Aktion (z. B. einen automatischen Rollback) konfigurieren, die ausgeführt wird, wenn bei der Integritätsevaluierung ein Fehler auftritt. In diesem Abschnitt wird ein überwachtes Upgrade für eines der SDK-Beispiele mit PowerShell Schritt für Schritt erläutert.

## Schritt 1: Erstellen und Bereitstellen des Beispiels „Visual Objects“


Erstellen und veröffentlichen Sie die Anwendung, indem Sie mit der rechten Maustaste auf das Anwendungsprojekt **VisualObjectsApplication** klicken und den Befehl **Veröffentlichen** auswählen. Weitere Informationen finden Sie im [Tutorial für Service Fabric-Anwendungsupgrades](service-fabric-application-upgrade-tutorial.md). Alternativ können Sie Ihre Anwendung mithilfe von PowerShell bereitstellen.

> [AZURE.NOTE] Um Service Fabric-Befehle in PowerShell verwenden zu können, müssen Sie zunächst mit dem Cmdlet `Connect-ServiceFabricCluster` eine Verbindung mit dem Cluster herstellen. Auch wird davon ausgegangen, dass der Cluster bereits auf dem lokalen Computer eingerichtet wurde. Siehe dazu den Artikel [Set up your Service Fabric development environment](service-fabric-get-started.md) (in englischer Sprache).

Nach dem Erstellen des Projekts in Visual Studio können Sie das Anwendungspaket über den PowerShell-Befehl **Copy-ServiceFabricApplicationPackage** in den ImageStore kopieren. Nach diesem Schritt folgt die Registrierung der Anwendung bei der Service Fabric-Laufzeit mit dem Cmdlet **Register-ServiceFabricApplicationPackage**. Im letzten Schritt wird mithilfe des Cmdlets **New-ServiceFabricApplication** eine Instanz der Anwendung gestartet. Diese drei Schritte entsprechen der Verwendung des Menüelements **Bereitstellen** in Visual Studio.

Nun können Sie mit dem [Service Fabric-Explorer den Cluster und die Anwendung anzeigen](service-fabric-visualizing-your-cluster.md). Die Anwendung verfügt über einen Webdienst, zu dem Sie wechseln können, indem Sie in der Adressleiste von Internet Explorer [http://localhost:8081/visualobjects](http://localhost:8081/visualobjects) eingeben. Sie sollten einige unverankerte visuelle Objekte sehen, die sich auf dem Bildschirm bewegen. Darüber hinaus können Sie **Get-ServiceFabricApplication** zum Überprüfen des Anwendungsstatus verwenden.

## Schritt 2: Aktualisieren des Beispiels „Visual Objects“

Sie werden feststellen, dass sich die visuellen Objekte mit der Version, die in Schritt 1 bereitgestellt wurde, nicht drehen. Wir aktualisieren diese Anwendung so, dass sich die visuellen Objekte drehen.

Wählen Sie das Projekt „VisualObjects.ActorService“ in der Projektmappe „VisualObjects“ aus, und öffnen Sie die Datei „StatefulVisualObjectActor.cs“. Navigieren Sie in der Datei zur `MoveObject`-Methode, kommentieren Sie `this.State.Move()` aus, und heben Sie die Auskommentierung von `this.State.Move(true)` auf. Diese Änderung bewirkt, dass sich die Objekte nach dem Upgrade des Diensts drehen.

Wir müssen auch die Datei *ServiceManifest.xml* (unter „PackageRoot“) des Projekts **VisualObjects.DataService** aktualisieren. Aktualisieren Sie *CodePackage* und die Dienstversion auf 2.0., und aktualisieren Sie die entsprechenden Zeilen in der Datei *ServiceManifest.xml*. Klicken Sie zum Ändern der Manifestdatei mit der rechten Maustaste auf die Projektmappe, und wählen Sie anschließend die Visual Studio-Option *Manifestdateien bearbeiten* aus.


Nach diesen Änderungen sollte das Manifest wie folgt aussehen (Änderungen sind hervorgehoben):

```xml
<ServiceManifestName="VisualObjects.ActorService" Version="2.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">

<CodePackageName="Code" Version="2.0">
```

Nun wird die Datei *ApplicationManifest.xml* (im Projekt **VisualObjects** unter der Projektmappe **VisualObjects**) auf Version 2.0 des Projekts **VisualObjects.ActorService** aktualisiert. Außerdem muss die Version der Anwendung von „1.0.0.0“ in „2.0.0.0“ geändert werden. Die Datei *ApplicationManifest.xml* sollte wie im folgenden Codeausschnitt aussehen:

```xml
<ApplicationManifestxmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="VisualObjects" ApplicationTypeVersion="2.0.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

 <ServiceManifestRefServiceManifestName="VisualObjects.ActorService" ServiceManifestVersion="2.0" />
```


Erstellen Sie jetzt das Projekt, indem Sie in Visual Studio das Projekt **ActorService** auswählen und dann mit der rechten Maustaste klicken und **Erstellen** wählen. Wenn Sie die Option **Alles neu erstellen** wählen, müssen Sie die Versionen für alle Projekte aktualisieren, da sich der Code geändert haben sollte. Packen Sie nun die aktualisierte Anwendung, indem Sie mit der rechten Maustaste auf ***VisualObjectsApplication*** klicken und anschließend im Service Fabric-Menü die Option **Paket** auswählen. Damit wird ein Anwendungspaket erstellt, das bereitgestellt werden kann. Die aktualisierte Anwendung kann nun bereitgestellt werden.


## Schritt 3: Festlegen der Integritätsrichtlinien und Upgradeparameter

Machen Sie sich mit den [Anwendungsupgradeparametern](service-fabric-application-upgrade-parameters.md) und dem [Upgradevorgang](service-fabric-application-upgrade.md) vertraut, um sich einen fundierten Überblick über die verschiedenen Upgradeparameter, Timeouts und Integritätskriterien zu verschaffen. In dieser exemplarischen Vorgehensweise ist das Evaluierungskriterium für die Dienstintegrität auf die (empfohlenen) Standardwerte festgelegt, wodurch alle Dienste und Instanzen nach dem Upgrade _fehlerfrei_ sein sollten.

Allerdings erhöhen wir *HealthCheckStableDuration* auf 60 Sekunden, sodass die Dienste mindestens 20 Sekunden fehlerfrei sind, bevor zur nächsten Updatedomäne gewechselt wird. Wir legen außerdem *UpgradeDomainTimeout* auf 1200 Sekunden und *UpgradeTimeout* auf 3000 Sekunden fest.

Schließlich legen wir *UpgradeFailureAction* auf „Rollback“ fest. Diese Option fordert von Service Fabric an, die Anwendung auf die vorherige Version zurückzusetzen, falls während des Upgrades Probleme erkannt werden. Daher werden beim Starten des Upgrades (in Schritt 4) die folgenden Parameter angegeben:

FailureAction = Rollback

HealthCheckStableDurationSec = 60

UpgradeDomainTimeoutSec = 1200

UpgradeTimeout = 3000


## Schritt 4: Vorbereiten der Anwendung für das Upgrade

Die Anwendung ist nun erstellt und kann aktualisiert werden. Wenn Sie als Administrator ein PowerShell-Fenster öffnen und **Get-ServiceFabricApplication** eingeben, sollte angezeigt werden, dass der Anwendungstyp „1.0.0.0“ von **VisualObjects** bereitgestellt wurde.

Das Anwendungspaket ist unter dem folgenden relativen Pfad gespeichert, in dem Sie das Service Fabric-SDK dekomprimiert haben: *Samples\\Services\\Stateful\\VisualObjects\\VisualObjects\\obj\\x64\\Debug*. In diesem Verzeichnis befindet sich der Ordner „Package“, in dem das Anwendungspaket gespeichert ist. Überprüfen Sie die Zeitstempel, um sicherzustellen, dass es sich um die neueste Version handelt (möglicherweise müssen Sie auch die Pfade entsprechend ändern).

Jetzt kopieren Sie das aktualisierte Anwendungspaket in den Service Fabric-ImageStore (in dem die Anwendungspakete von Service Fabric gespeichert werden). Der *ApplicationPackagePathInImageStore*-Parameter informiert Service Fabric darüber, wo sich das Anwendungspaket befindet. Wir haben die aktualisierte Anwendung mit dem folgenden Befehl in „VisualObjects\_V2“ abgelegt (möglicherweise müssen Sie die Pfade wieder entsprechend ändern).

```powershell
Copy-ServiceFabricApplicationPackage  -ApplicationPackagePath .\Samples\Services\Stateful\VisualObjects\VisualObjects\obj\x64\Debug\Package
-ImageStoreConnectionString fabric:ImageStore   -ApplicationPackagePathInImageStore "VisualObjects\_V2"
```

Im nächsten Schritt wird diese Anwendung bei Service Fabric registriert. Dies kann über den folgenden Befehl erfolgen:

```powershell
Register-ServiceFabricApplicationType -ApplicationPathInImageStore "VisualObjects\_V2"
```

Wenn der vorherige Befehl nicht erfolgreich ausgeführt wird, müssen Sie wahrscheinlich alle Dienste neu erstellen. Wie in Schritt 2 erwähnt, müssen Sie möglicherweise auch Ihre WebService-Version aktualisieren.

## Schritt 5: Starten des Anwendungsupgrades

Jetzt können wir das Anwendungsupgrade mit dem folgenden Befehl starten:

```powershell
Start-ServiceFabricApplicationUpgrade -ApplicationName fabric:/VisualObjects -ApplicationTypeVersion 2.0.0.0 -HealthCheckStableDurationSec 60 -UpgradeDomainTimeoutSec 1200 -UpgradeTimeout 3000   -FailureAction Rollback -Monitored
```


Beachten Sie, dass als Anwendungsname der in der Datei *ApplicationManifest.xml* angegebene Name verwendet wird. Service Fabric verwendet diesen Namen, um die zu aktualisierende Anwendung zu ermitteln. Wenn Sie die Timeoutwerte zu kurz festlegen, wird möglicherweise eine entsprechende Fehlermeldung angezeigt. Informationen hierzu finden Sie im Abschnitt „Problembehandlung“. Sie können die Timeoutwerte erhöhen.

Während das Anwendungsupgrade durchgeführt wird, können Sie es in Service Fabric Explorer oder mit dem folgenden PowerShell-Befehl überwachen: **Get-ServiceFabricApplicationUpgrade fabric:/VisualObjects**.

Nach wenigen Minuten sollte nach Verwendung des vorangehenden PowerShell-Befehls angezeigt werden, dass alle Updatedomänen aktualisiert (abgeschlossen) wurden. Und Sie werden feststellen, dass die visuellen Objekte in Ihrem Browserfenster sich jetzt drehen!

Versuchen Sie zur Übung ein Upgrade von Version 2 auf Version 3 oder von Version 2 auf Version 1. Der Wechsel von Version 2 zu Version 1 wird ebenfalls als Upgrade eingestuft. Experimentieren Sie mit den Timeouts und Integritätsrichtlinien, um sich schrittweise mit ihnen vertraut zu machen. Wenn Sie die Bereitstellung in einem Azure-Cluster erfolgt, müssen die Parameter entsprechend festgelegt werden. Es ist ratsam, die Timeouts konservativ festzulegen.


## Nächste Schritte

Unter [Upgrade Ihrer Anwendung mit Visual Studio](service-fabric-application-upgrade-tutorial.md) werden Sie schrittweise durch ein Anwendungsupgrade mithilfe von Visual Studio geführt.

Steuern Sie die Upgrades von Anwendungen mithilfe von [Upgradeparametern](service-fabric-application-upgrade-parameters.md).

Machen Sie sich mit der [Datenserialisierung](service-fabric-application-upgrade-data-serialization.md) vertraut, um Ihre Anwendungsupgrades kompatibel zu machen.

Informieren Sie sich in [weiterführenden Themen](service-fabric-application-upgrade-advanced.md) darüber, wie Sie erweiterte Funktionen beim Upgrade Ihrer Anwendung nutzen.

Informationen zum Beheben gängiger Probleme bei Anwendungsupgrades finden Sie in den Anweisungen unter [Problembehandlung bei Anwendungsupgrades](service-fabric-application-upgrade-troubleshooting.md).

<!---HONumber=AcomDC_0921_2016-->