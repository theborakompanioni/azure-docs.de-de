<properties
   pageTitle="Anwendungsupgrade: Weiterführende Themen | Microsoft Azure"
   description="Dieser Artikel behandelt einige weiterführende Themen in Bezug auf Upgrades von Service Fabric-Anwendungen."
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
   ms.date="05/13/2016"
   ms.author="subramar"/>

# Service Fabric-Anwendungsupgrade: Weiterführende Themen

## Hinzufügen oder Entfernen von Diensten während eines Anwendungsupgrades

Wenn ein neuer Dienst einer Anwendung hinzugefügt wird, die bereits bereitgestellt und als Upgrade veröffentlicht ist, wird der neue Dienst der bereitgestellten Anwendung hinzugefügt (ohne dass das Upgrade Auswirkungen auf die Dienste hat, die bereits Teil der Anwendung waren). Eine Instanz des Diensts, der hinzugefügt wurde, muss jedoch (mithilfe des `New-ServiceFabricService`-Cmdlets)gestartet werden, damit der neue Dienst aktiv ist.

Dienste können auch im Rahmen eines Upgrades von einer Anwendung entfernt werden, jedoch muss sichergestellt werden, dass alle aktuelle Instanzen des Diensts (die im Rahmen des Upgrades entfernt werden sollen) gestoppt werden, bevor das Upgrade fortgesetzt wird (mithilfe des `Remove-ServiceFabricService`-Cmdlets).

## Manueller Upgrademodus

> [AZURE.NOTE]  Der nicht überwachte manuelle Modus sollte ausschließlich bei einem fehlerhaften oder angehaltenen Upgrade in Betracht gezogen werden. Der überwachte Modus ist der für Service Fabric-Anwendungen empfohlene Modus.

Azure Service Fabric umfasst mehrere Upgrademodi zur Unterstützung von Entwicklungs- und Produktionsclustern. Die einzelnen Bereitstellungsoptionen sind jeweils ideal geeignet für die verschiedenen Umgebungen.

Das überwachte parallele Anwendungsupgrade ist das typische in Produktionsumgebungen verwendete Upgrade. Wenn die Upgraderichtlinie angegeben ist, stellt Service Fabric sicher, dass die Anwendung fehlerfrei ist, bevor das Upgrade fortgesetzt wird.

 Der Anwendungsadministrator kann den manuellen parallelen Upgrademodus für Anwendungen verwenden, um über die verschiedenen Upgradedomänen hinweg eine vollständige Kontrolle über den Upgradeverlauf zu behalten. Dieser Modus eignet sich für bestimmte Situationen, in denen eine stärker angepasste oder komplexere Richtlinie zur Integritätsauswertung erforderlich ist oder in denen ein außergewöhnliches Upgrade stattfindet (z. B. in der Anwendung gehen bereits Daten verloren).

Schließlich eignet sich der automatisierte parallele Upgrademodus in Entwicklungs- oder Testumgebungen zur Bereitstellung schneller Iterationszyklen während der Entwicklung von Diensten.

## Wechseln in den manuellen Upgrademodus
**Manuell**: Das Anwendungsupgrade wird bei der aktuellen Upgradedomäne gestoppt, und es erfolgt ein Wechsel in den nicht überwachten manuellen Upgrademodus. Der Administrator muss **MoveNextApplicationUpgradeDomainAsync** manuell aufrufen, um das Upgrade fortzusetzen, oder durch Initiieren eines neuen Upgrades einen Rollback auslösen. Wenn das Upgrade in den manuellen Modus wechselt, wird dieser Modus beibehalten, bis ein neues Upgrade initiiert wird. Der **GetApplicationUpgradeProgressAsync**-Befehl gibt „FABRIC\_APPLICATION\_UPGRADE\_STATE\_ROLLING\_FORWARD\_PENDING“ zurück.

## Upgrade mit einem Diff-Paket

Eine Service Fabric-Anwendung kann durch Bereitstellen eines vollständigen und eigenständigen Anwendungspakets aktualisiert werden. Eine Anwendung kann auch mithilfe eines Diff-Pakets aktualisiert werden, das nur die aktualisierten Anwendungsdateien, das aktualisierte Anwendungsmanifest und die Dienstmanifestdateien enthält.

Ein vollständiges Anwendungspaket enthält alle zum Starten und Ausführen einer Service Fabric-Anwendung erforderlichen Dateien. Ein Diff-Paket enthält dagegen nur die Dateien, die sich zwischen der letzten Bereitstellung und dem aktuellen Upgrade geändert haben, sowie das vollständige Anwendungsmanifest und die Dienstmanifestdateien. Bei allen Verweisen im Anwendungsmanifest oder Dienstmanifest, die Service Fabric im Buildlayout nicht finden kann, sucht Service Fabric im Imagespeicher.

Vollständige Anwendungspakete werden für die Erstinstallation einer Anwendung im Cluster benötigt. Für nachfolgende Upgrades kann entweder ein vollständiges Anwendungspaket oder ein Diff-Paket verwendet werden.

Situationen, in denen sich ein Diff-Paket anbietet:

* Ein Diff-Paket wird gegenüber einem umfangreichen Anwendungspaket bevorzugt, das auf mehrere Dienstmanifestdateien und/oder mehrere Code-, Konfigurations- oder Datenpakete verweist.

* Ein Diff-Paket wird in einem Bereitstellungssystem bevorzugt, das das Buildlayout direkt aus dem Anwendungsbuildprozess generiert. In diesem Fall weisen neu erstellte Assemblys eine andere Prüfsumme auf, auch wenn sich der Code nicht geändert hat. Bei Verwendung eines vollständigen Anwendungspakets müssten Sie die Version in allen Codepaketen ändern. Bei Verwendung eines Diff-Pakets stellen Sie dagegen nur die geänderten Dateien und die Manifestdateien bereit, in denen sich die Version geändert hat.

Wenn eine Anwendung mit Visual Studio aktualisiert wird, wird das Diff-Paket automatisch veröffentlicht. Wenn Sie beabsichtigten, ein Diff-Paket (z. B. für ein Upgrade mithilfe von PowerShell) manuell zu erstellen, sollten Sie die Anwendung und Dienstmanifeste aktualisieren, aber nur die Pakete einbeziehen, die im endgültigen Anwendungspaket geändert wurden.

Beispielsweise beginnen wir mit der folgenden Anwendung (Versionsnummern werden zum einfacheren Verständnis angegeben):

```text
app1       	1.0.0
  service1 	1.0.0
    code   	1.0.0
    config 	1.0.0
  service2 	1.0.0
    code   	1.0.0
    config 	1.0.0
```

Nun nehmen wir an, dass Sie nur das Codepaket von Service1 mithilfe eines Diff-Pakets mit PowerShell aktualisieren möchten. Die aktualisierte Anwendung sieht nun wie folgt aus:

```text
app1       	2.0.0      <-- new version
  service1 	2.0.0      <-- new version
    code   	2.0.0      <-- new version
    config 	1.0.0
  service2 	1.0.0
    code   	1.0.0
    config 	1.0.0
```

In diesem Fall aktualisieren Sie das Anwendungsmanifest auf 2.0.0 und das Dienstmanifest für Service1, um die Codepaketaktualisierung widerzuspiegeln. Die Ordnerstruktur für das Anwendungspaket würde wie folgt aussehen:

```text
app1/
  service1/
    code/
```

## Nächste Schritte

Unter [Upgrade Ihrer Anwendung mit Visual Studio](service-fabric-application-upgrade-tutorial.md) werden Sie schrittweise durch das Upgrade der Anwendung mithilfe von Visual Studio geführt.

Unter [Upgrade Ihrer Anwendung mithilfe von PowerShell](service-fabric-application-upgrade-tutorial-powershell.md) werden Sie schrittweise durch das Upgrade der Anwendung mithilfe von PowerShell geführt.

Steuern Sie die Upgrades von Anwendungen mithilfe von [Upgradeparametern](service-fabric-application-upgrade-parameters.md).

Machen Sie Ihre Anwendungsupgrades kompatibel, indem Sie sich mit der [Datenserialisierung](service-fabric-application-upgrade-data-serialization.md) vertraut machen.

Informationen zum Beheben gängiger Probleme bei Anwendungsupgrades finden Sie in den Anweisungen unter [Problembehandlung bei Anwendungsupgrades](service-fabric-application-upgrade-troubleshooting.md).
 

<!---HONumber=AcomDC_0518_2016-->