
<properties
   pageTitle="Anwendungsupgrade: Upgradeparameter | Microsoft Azure"
   description="Beschreibt Parameter im Zusammenhang mit dem Upgrade einer Service Fabric-Anwendung, einschließlich durchzuführender Integritätsprüfungen und Richtlinien für das automatische Rückgängigmachen des Upgrades."
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



# Parameter für Anwendungsupgrades

In diesem Artikel werden die verschiedenen Parameter beschrieben, die während des Upgrades einer Azure Service Fabric-Anwendung gelten. Die Parameter enthalten den Namen und die Version der Anwendung. Sie sind Schalter, die die während des Upgrades angewendeten Timeouts und Integritätsprüfungen steuern, und geben die Richtlinien an, die bei einem Upgradefehler angewendet werden müssen.


<br>

| Parameter | Beschreibung |
| --- | --- |
| ApplicationName | Der Name der Anwendung, die aktualisiert wird. Beispiele: fabric:/VisualObjects, fabric:/ClusterMonitor |
| TargetApplicationTypeVersion | Die Version des Anwendungstyps, für den das Upgrade durchgeführt wird. |
| FailureAction | Die Aktion, die von Service Fabric bei einem Upgradefehler ausgeführt wird. Die Anwendung kann auf die Version vor dem Update zurückgesetzt werden, oder das Upgrade wird ggf. von der aktuellen Upgradedomäne beendet. Im letzteren Fall wird der Upgrademodus auch in „Manual“ geändert. Zulässige Werte sind "Rollback" und "Manual". |
| HealthCheckWaitDurationSec | Die Wartezeit (in Sekunden) nach dem Abschluss des Upgrades in der Upgradedomäne, bevor Service Fabric die Integrität der Anwendung evaluiert. Diese Dauer kann auch als die Zeit betrachtet werden, die eine Anwendung ausgeführt werden soll, bevor sie als fehlerfrei gilt. Wenn die Integritätsprüfung erfolgreich ist, wird der Upgradevorgang bei der nächsten Upgradedomäne fortgesetzt. Wenn die Integritätsprüfung fehlschlägt, wartet Service Fabric auf ein Intervall ("UpgradeHealthCheckInterval"), bevor die Integritätsprüfung wiederholt wird, bis das "HealthCheckRetryTimeout" erreicht ist. Der empfohlene Standardwert ist 0 Sekunden. |
| HealthCheckRetryTimeoutSec | Die Zeit (in Sekunden), in der Service Fabric die Integritätsevaluierung fortsetzt, bevor das Upgrade als fehlgeschlagen deklariert wird. Der Standardwert ist 600 Sekunden. Dieser Zeitraum beginnt, nachdem "HealthCheckWaitDuration" erreicht wurde. Innerhalb von "HealthCheckRetryTimeout" kann Service Fabric möglicherweise mehrere Integritätsprüfungen der Anwendungsintegrität durchführen. Der Standardwert ist 10 Minuten, und es wird empfohlen, diesen Wert Ihrer Anwendung entsprechend anzupassen. |
| HealthCheckStableDurationSec | Die Dauer (in Sekunden), in der überprüft wird, ob die Anwendung stabil ist, bevor mit der nächsten Upgradedomäne fortgefahren oder das Upgrade abgeschlossen wird. Diese Wartezeit wird verwendet, um nicht erkannte Integritätsänderungen direkt nach dem Durchführen der Integritätsprüfung zu verhindern. Der Standardwert ist 120 Minuten, und es wird empfohlen, diesen Wert Ihrer Anwendung entsprechend anzupassen. |
| UpgradeDomainTimeoutSec | Maximale Zeit (in Sekunden) für das Aktualisieren einer einzelnen Upgradedomäne. Bei Erreichen dieses Timeouts wird das Upgrade beendet und basierend auf der Einstellung von „UpgradeFailureAction“ fortgesetzt. Der Standardwert ist „never“ (unendlich), und es wird empfohlen, diesen Wert Ihrer Anwendung entsprechend anzupassen. |
| UpgradeTimeout | Ein Timeout (in Sekunden), das für das gesamte Upgrade gilt. Bei Erreichen dieses Timeouts wird das Upgrade beendet und UpgradeFailureAction wird ausgelöst. Der Standardwert ist „never“ (unendlich), und es wird empfohlen, diesen Wert Ihrer Anwendung entsprechend anzupassen. |
| UpgradeHealthCheckInterval | Die Häufigkeit, mit der der Integritätsstatus geprüft wird. Dieser Parameter wird im Abschnitt „ClusterManager“ des _Cluster__manifests_ angegeben und nicht als Teil des Upgrade-Cmdlets angegeben. Der Standardwert beträgt 60 Sekunden. |






<br>
## Dienstintegritätsevaluierung während des Anwendungsupgrades

<br> Die Kriterien für die Integritätsevaluierung sind optional. Wenn beim Starten eines Upgrades keine Kriterien für die Integritätsevaluierung angegeben werden, verwendet Service Fabric die Anwendungsintegritätsrichtlinien, die in der Datei „ApplicationManifest.xml“ der Anwendungsinstanz angegeben sind.


<br>

| Parameter | Beschreibung |
| --- | --- |
| ConsiderWarningAsError | Der Standardwert ist "False". Bei der Evaluierung der Integrität der Anwendung während des Upgrades werden die Warnereignisse für die Anwendung als Fehler behandelt. In der Standardeinstellung bewertet Service Fabric Warnereignisse in Bezug auf die Integrität nicht als Fehler, sodass das Upgrade auch bei Warnereignissen fortgesetzt werden kann. |
| MaxPercentUnhealthyDeployedApplications | Der empfohlene Standardwert ist 0. Geben Sie die maximale Anzahl der bereitgestellten Anwendungen an (siehe dazu den [Abschnitt zur Integrität](service-fabric-health-introduction.md)), die fehlerhaft sein können, bevor die Anwendung als fehlerhaft gilt und das Upgrade abgebrochen wird. Dieser Parameter definiert die Anwendungsintegrität für den Knoten und hilft bei der Ermittlung von Problemen während des Upgrades. Typischerweise erfolgt ein Lastenausgleich der Replikate der Anwendung auf dem anderen Knoten, sodass die Anwendung fehlerfrei zu sein scheint und das Upgrade so fortgesetzt werden kann. Durch Angeben eines strikten Werts für MaxPercentUnhealthyDeployedApplications für die Integrität kann Service Fabric ein Problem mit dem Anwendungspaket schnell erkennen und ein Fail-Fast-Upgrade erzeugen. |
| MaxPercentUnhealthyServices | Der empfohlene Standardwert ist 0. Geben Sie die maximale Anzahl der Dienste in der Anwendungsinstanz an, die fehlerhaft sein können, bevor die Anwendung als fehlerhaft gilt und das Upgrade abgebrochen wird. |
| MaxPercentUnhealthyPartitionsPerService | Der empfohlene Standardwert ist 0. Geben Sie die maximale Anzahl der Partitionen in einem Dienst an, die fehlerhaft sein können, bevor der Dienst als fehlerhaft gilt. |
| MaxPercentUnhealthyReplicasPerPartition | Der empfohlene Standardwert ist 0. Geben Sie die maximale Anzahl der Replikate in einer Partition an, die fehlerhaft sein können, bevor die Partition als fehlerhaft gilt. |
| UpgradeReplicaSetCheckTimeout | **Statusfreier Dienst**: Service Fabric versucht innerhalb einer Upgradedomäne sicherzustellen, dass zusätzliche Instanzen des Diensts verfügbar sind. Wenn die Anzahl der Zielinstanzen größer als 1 ist, wartet Service Fabric bis zu einem maximalen Timeoutwert darauf, dass mehrere Instanzen verfügbar sind. Dieses Timeout wird mithilfe der „UpgradeReplicaSetCheckTimeout“-Eigenschaft angegeben. Wenn das Timeout überschritten wird, fährt Service Fabric unabhängig von der Anzahl der Dienstinstanzen mit dem Upgrade fort. Wenn die Anzahl der Zielinstanzen gleich 1 ist, wartet Service Fabric nicht, sondern setzt das Upgrade direkt fort. **Statusbehafteter Dienst**: Service Fabric versucht innerhalb einer Upgradedomäne sicherzustellen, dass die Replikatgruppe über ein Quorum verfügt. Service Fabric wartet bis zu einem maximalen Timeoutwert (angegeben durch die UpgradeReplicaSetCheckTimeout-Eigenschaft) darauf, dass ein Quorum verfügbar ist. Wenn das Timeout überschritten wird, fährt Service Fabric unabhängig vom Quorum mit dem Upgrade fort. Diese Einstellung ist für Rollforwards auf „never“ (unbegrenzt) und für Rollbacks auf 900 Sekunden festgelegt. |
| ForceRestart | Wenn Sie ein Konfigurations- oder Datenpaket aktualisieren, ohne den Dienstcode zu aktualisieren, wird der Dienst nur neu gestartet, wenn die „ForceRestart“-Eigenschaft auf „true“ festgelegt ist. Wenn das Upgrade abgeschlossen ist, benachrichtigt Service Fabric den Dienst, dass ein neues Konfigurations- oder Datenpaket verfügbar ist. Der Dienst muss dann die Änderungen anwenden. Bei Bedarf kann sich der Dienst neu starten. |



<br> <br> Die Kriterien für MaxPercentUnhealthyServices, MaxPercentUnhealthyPartitionsPerService und MaxPercentUnhealthyReplicasPerPartition können pro Diensttyp für eine Anwendungsinstanz angegeben werden. Das dienstbezogene Festlegen dieser Parameter für eine Anwendung ermöglicht, dass diese verschiedene Diensttypen mit unterschiedlichen Auswertungsrichtlinien enthält. Ein statusfreier Gatewaydienst kann beispielsweise einen MaxPercentUnhealthyPartitionsPerService-Wert aufweisen, der sich von dem Wert für einen statusbehafteten Moduldienst für eine bestimmte Anwendungsinstanz unterscheidet.

## Nächste Schritte

[Ihre Anwendung mit Visual Studio upgraden](service-fabric-application-upgrade-tutorial.md) beschreibt das Upgraden von Anwendungen mit Visual Studio.

[Ihre Anwendung mit PowerShell upgraden](service-fabric-application-upgrade-tutorial-powershell.md) beschreibt das Upgraden von Anwendungen mit PowerShell.

Machen Sie Ihre Anwendungsupgrades kompatibel, indem Sie sich mit der [Datenserialisierung](service-fabric-application-upgrade-data-serialization.md) vertraut machen.

Informieren Sie sich in [weiterführenden Themen](service-fabric-application-upgrade-advanced.md) darüber, wie Sie erweiterte Funktionen beim Upgrade Ihrer Anwendung nutzen.

Informationen zum Beheben gängiger Probleme bei Anwendungsupgrades finden Sie in den Anweisungen unter [Problembehandlung bei Anwendungsupgrades](service-fabric-application-upgrade-troubleshooting.md).
 

<!---HONumber=AcomDC_0921_2016-->