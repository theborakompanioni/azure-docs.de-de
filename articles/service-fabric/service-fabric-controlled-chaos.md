---
title: "Auslösen von Chaos in Service Fabric-Clustern | Microsoft Docs"
description: Verwenden von Fault Injection und Cluster Analysis Service-APIs zum Verwalten von Chaostests im Cluster.
services: service-fabric
documentationcenter: .net
author: motanv
manager: anmola
editor: motanv
ms.assetid: 2bd13443-3478-4382-9a5a-1f6c6b32bfc9
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/11/2017
ms.author: motanv
ms.translationtype: Human Translation
ms.sourcegitcommit: fc4172b27b93a49c613eb915252895e845b96892
ms.openlocfilehash: 00f703cf9e727cd5981c4f8254fc11330e41a470
ms.contentlocale: de-de
ms.lasthandoff: 05/12/2017


---
# <a name="induce-controlled-chaos-in-service-fabric-clusters"></a>Auslösen von kontrolliertem Chaos in Service Fabric-Clustern
Große verteilte Systeme wie Cloudinfrastrukturen sind grundsätzlich unzuverlässig. Azure Service Fabric ermöglicht Entwicklern, aufbauend auf einer unzuverlässigen Infrastruktur zuverlässige verteilte Dienste zu erstellen. Um robuste, verteilte Dienste in einer unzuverlässigen Infrastruktur zu schreiben, müssen Entwickler in der Lage sein, die Stabilität ihrer Dienste zu testen, während die zugrunde liegende unzuverlässige Infrastruktur komplizierte Statusübergänge aufgrund von Fehlern durchläuft.

Der [Fault Injection and Cluster Analysis Service](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-testability-overview) (auch als Fault Analysis Service (FAS) bezeichnet) gibt Entwicklern die Möglichkeit, Fehler einzuschleusen, um ihre Dienste zu testen. Solche zielgerichteten simulierten Fehler, wie z.B. das [Neustarten einer Partition](https://docs.microsoft.com/en-us/powershell/module/servicefabric/start-servicefabricpartitionrestart?view=azureservicefabricps), können dazu beitragen, die am häufigsten auftretenden Zustandsübergänge zu üben. Zielgerichtete simulierte Fehler sind jedoch definitionsgemäß nicht objektiv und können damit keine Fehler aufzeigen, die nur bei schwierig vorherzusagenden, langen und komplizierten Abfolgen von Zustandsübergängen auftreten. Für ausgewogene Tests können Sie Chaos verwenden.

Mithilfe von Chaos werden im Cluster über längere Zeiträume hinweg periodische verschachtelte Fehler simuliert (sowohl ordnungsgemäß als auch nicht ordnungsgemäß). Nachdem Sie Chaos mit der Rate und Art der Fehler für einen Test konfiguriert haben, können Sie Chaos über die C#- oder PowerShell-API starten, um Fehler im Cluster und in Ihren Diensten zu generieren. Sie können Chaos für die Ausführung über einen angegebenen Zeitraum (z.B. für eine Stunde) konfigurieren. Chaos wird danach automatisch beendet. Sie können aber auch jederzeit die StopChaos-API (C# oder PowerShell) aufrufen, um die Ausführung zu beenden.

> [!NOTE]
> In der derzeitigen Form löst Chaos nur sichere Fehler aus. Dies bedeutet, dass bei einem Nichtvorhandensein von externen Fehlern niemals ein Quorum- oder Datenverlust auftritt.
>

Während dieses Chaostests, werden verschiedene Ereignisse erzeugt, die den Status der Ausführung im jeweiligen Moment erfassen. Beispielsweise enthält ein ExecutingFaultsEvent alle Fehler, die Chaos im Rahmen der Iteration ausführt. Ein ValidationFailedEvent enthält die Details eines Überprüfungsfehlers (Integritäts- oder Stabilitätsprobleme), der während der Überprüfung des Clusters gefunden wurde. Sie können die GetChaosReport-API (C# oder PowerShell) aufrufen, um einen Bericht zu Chaos-Ausführungen zu erhalten. Diese Ereignisse werden in ein [Reliable Dictionary](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-reliable-services-reliable-collections) übernommen, für das eine Kürzungsrichtlinie mit zwei Konfigurationen gilt: **MaxStoredChaosEventCount** (der Standardwert ist 25.000) und **StoredActionCleanupIntervalInSeconds** (der Standardwert ist 3.600). Alle *StoredActionCleanupIntervalInSeconds* führt Chaos eine Überprüfung durch und entfernt alle Ereignisse bis auf die letzten *MaxStoredChaosEventCount* Ereignisse aus dem Reliable Dictionary dauerhaft.

## <a name="faults-induced-in-chaos"></a>Im Chaostest ausgelöste Fehler
Der Chaostest generiert Fehler im gesamten Service Fabric-Cluster und komprimiert in Monaten oder Jahren auftretende Fehler zu einigen wenigen Stunden. Bei dieser Kombination aus verschachtelten Fehlern mit der hohen Fehlerrate werden auch Spezialfälle erkannt, die sonst nicht auffallen. Dieser Chaostest führt zu einer erheblichen Verbesserung der Codequalität des Diensts.

Der Chaostest verursacht Fehler in den folgenden Kategorien:

* Neustart eines Knotens
* Neustart eines bereitgestellten Codepakets
* Entfernung eines Replikats
* Neustart eines Replikats
* Verschiebung eines primären Replikats (konfigurierbar)
* Verschiebung eines sekundären Replikats (konfigurierbar)

Chaos wird in mehreren Iterationen ausgeführt. Jede Iteration besteht aus Fehlern und der Clusterüberprüfung für den angegebenen Zeitraum. Sie können die Dauer der Stabilisierung des Clusters und des erfolgreichen Abschlusses der Überprüfung konfigurieren. Wenn bei der Clusterüberprüfung ein Fehler gefunden wird, wird vom Chaostest ein „ValidationFailedEvent“-Ereignis mit dem UTC-Zeitstempel und den Fehlerdetails generiert und gespeichert. Nehmen wir beispielsweise an, ein Chaostest soll eine Stunde lang ausgeführt werden und maximal drei gleichzeitige Fehler umfassen. Beim Chaostest werden drei Fehler eingeschleust, und anschließend wird die Clusterintegrität überprüft. Es erfolgt eine Iteration durch den vorherigen Schritt, bis dieser explizit mithilfe der StopChaosAsync-API beendet wird oder eine Stunde vergangen ist. Wenn der Cluster bei einer der Iterationen fehlerhaft wird (also sich nicht innerhalb des mit MaxClusterStabilizationTimeout übergebenen Zeitraums stabilisiert), generiert Chaos ein Ereignis vom Typ „ValidationFailedEvent“. Dieses Ereignis bedeutet, dass etwas schiefgelaufen ist und ggf. eine nähere Untersuchung erforderlich ist.

Wenn Sie herausfinden möchten, welche Fehler Chaos induziert hat, können Sie die GetChaosReport-API (PowerShell oder C#) verwenden. Die API ruft das jeweils nächste Segment des Chaos-Berichts basierend auf dem übergebenen Fortsetzungstoken oder dem übergebenen Zeitraum ab. Sie können entweder das ContinuationToken zum Abrufen des nächsten Segments des Chaos-Berichts angeben oder den Zeitraum mit StartTimeUtc und EndTimeUtc festlegen. Sie können jedoch nicht im gleichen Aufruf das ContinuationToken und den Zeitraum angeben. Wenn mehr als 100 Chaos-Ereignisse vorhanden sind, wird der Chaos-Bericht in Segmenten zurückgegeben, die jeweils nicht mehr als 100 Chaos-Ereignisse enthalten.

## <a name="important-configuration-options"></a>Wichtige Konfigurationsoptionen
* **TimeToRun**: Gesamtdauer der Ausführung des Chaostests, bevor er erfolgreich abgeschlossen wird. Sie können den Chaostest mit der StopChaos-API vor Ablauf des Zeitraums von „TimeToRun“ beenden.

> [!NOTE]
> Chaos wird möglicherweise weiterhin ausgeführt, nachdem die *TimeToRun* abgelaufen ist. Es kann zusätzlich bis zu (MaxClusterStabilizationTime + MaxConcurrentFaults * WaitTimeBetweenFaults + WaitTimeBetweenIterations) dauern, bis eine automatische Beendigung erfolgt.
>

* **MaxClusterStabilizationTimeout:** maximale Dauer für das Warten auf die Wiederherstellung der Integrität des Clusters, bevor ein ValidationFailedEvent ausgelöst wird. Mit diesem Wartezeitraum soll die Auslastung des Clusters während der Wiederherstellung reduziert werden. Die folgenden Prüfungen erfolgen:
  * Ob die Clusterintegrität in Ordnung ist
  * Ob die Dienstintegrität in Ordnung ist
  * Ob die Größe der Zielreplikatgruppe für die Dienstpartition erreicht wurde
  * Ob keine „InBuild“-Replikate vorhanden sind
* **MaxConcurrentFaults**: Maximale Anzahl von gleichzeitigen Fehlern, die bei jeder Iteration ausgelöst werden. Je höher die Zahl, desto aggressiver geht Chaos vor und desto komplexer sind auch die Failover und die Status-Übergang-Kombinationen, die der Cluster durchläuft. 

> [!NOTE]
> Der Chaos-Test garantiert unabhängig von der Höhe des Werts von *MaxConcurrentFaults*, dass es bei Nichtauftreten externer Fehler nicht zu einem Quorum- oder Datenverlust kommt.
>

* **EnableMoveReplicaFaults**: Aktiviert oder deaktiviert die Fehler, die zur Verschiebung der primären oder sekundären Replikate führen. Diese Fehler sind standardmäßig deaktiviert.
* **WaitTimeBetweenIterations:** die Zeitspanne, die zwischen Iterationen gewartet wird. Dies ist also die Zeitspanne, die Chaos nach einem Fehlerdurchlauf und den zugehörigen Überprüfungen der Integrität des Clusters unterbrochen wird. Je höher der Wert, desto niedriger ist die durchschnittliche Fault Injection-Rate.
* **WaitTimeBetweenFaults:** Wartezeit zwischen zwei aufeinanderfolgenden Fehlern in einer Iteration. Je höher der Wert, desto niedriger ist die Parallelität (oder Überlappung) der Fehler.
* **ClusterHealthPolicy:** Die Clusterintegritätsrichtlinie wird verwendet, um die Integrität des Clusters zwischen Iterationen von Chaos zu überprüfen. Wenn die Clusterintegrität beeinträchtigt ist oder bei der Fehlerausführung eine unerwartete Ausnahme auftritt, wartet Chaos 30 Minuten bis zur nächsten Integritätsüberprüfung, damit der Cluster sich normalisieren kann.
* **Context:** eine Sammlung von Schlüssel-Wert-Paaren vom Typ (string, string). Die Zuordnung kann verwendet werden, um Informationen zur Ausführung von Chaos aufzuzeichnen. Es kann nicht mehr als 100 solcher Paare geben, und jede Zeichenfolge (Schlüssel oder Wert) darf höchstens 4.095 Zeichen lang sein. Diese Zuordnung wird beim Start der Chaos-Ausführung festgelegt, um optional den Kontext der jeweiligen Ausführung zu speichern.

## <a name="how-to-run-chaos"></a>Ausführen des Chaostests

```csharp
using System;
using System.Collections.Generic;
using System.Threading.Tasks;
using System.Fabric;

using System.Diagnostics;
using System.Fabric.Chaos.DataStructures;

class Program
{
    private class ChaosEventComparer : IEqualityComparer<ChaosEvent>
    {
        public bool Equals(ChaosEvent x, ChaosEvent y)
        {
            return x.TimeStampUtc.Equals(y.TimeStampUtc);
        }

        public int GetHashCode(ChaosEvent obj)
        {
            return obj.TimeStampUtc.GetHashCode();
        }
    }

    static void Main(string[] args)
    {
        var clusterConnectionString = "localhost:19000";
        using (var client = new FabricClient(clusterConnectionString))
        {
            var startTimeUtc = DateTime.UtcNow;
            var stabilizationTimeout = TimeSpan.FromSeconds(30.0);
            var timeToRun = TimeSpan.FromMinutes(60.0);
            var maxConcurrentFaults = 3;

            var parameters = new ChaosParameters(
                stabilizationTimeout,
                maxConcurrentFaults,
                true, /* EnableMoveReplicaFault */
                timeToRun);

            try
            {
                client.TestManager.StartChaosAsync(parameters).GetAwaiter().GetResult();
            }
            catch (FabricChaosAlreadyRunningException)
            {
                Console.WriteLine("An instance of Chaos is already running in the cluster.");
            }

            var filter = new ChaosReportFilter(startTimeUtc, DateTime.MaxValue);

            var eventSet = new HashSet<ChaosEvent>(new ChaosEventComparer());

            while (true)
            {
                var report = client.TestManager.GetChaosReportAsync(filter).GetAwaiter().GetResult();

                foreach (var chaosEvent in report.History)
                {
                    if (eventSet.Add(chaosEvent))
                    {
                        Console.WriteLine(chaosEvent);
                    }
                }

                // When Chaos stops, a StoppedEvent is created.
                // If a StoppedEvent is found, exit the loop.
                var lastEvent = report.History.LastOrDefault();

                if (lastEvent is StoppedEvent)
                {
                    break;
                }

                Task.Delay(TimeSpan.FromSeconds(1.0)).GetAwaiter().GetResult();
            }
        }
    }
}
```

```powershell
$connection = "localhost:19000"
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$concurrentFaults = 3
$waitTimeBetweenIterationsSec = 60

Connect-ServiceFabricCluster $connection

$events = @{}
$now = [System.DateTime]::UtcNow

Start-ServiceFabricChaos -TimeToRunMinute $timeToRun -MaxConcurrentFaults $concurrentFaults -MaxClusterStabilizationTimeoutSec $maxStabilizationTimeSecs -EnableMoveReplicaFaults -WaitTimeBetweenIterationsSec $waitTimeBetweenIterationsSec

while($true)
{
    $stopped = $false
    $report = Get-ServiceFabricChaosReport -StartTimeUtc $now -EndTimeUtc ([System.DateTime]::MaxValue)

    foreach ($e in $report.History) {

        if(-Not ($events.Contains($e.TimeStampUtc.Ticks)))
        {
            $events.Add($e.TimeStampUtc.Ticks, $e)
            if($e -is [System.Fabric.Chaos.DataStructures.ValidationFailedEvent])
            {
                Write-Host -BackgroundColor White -ForegroundColor Red $e
            }
            else
            {
                if($e -is [System.Fabric.Chaos.DataStructures.StoppedEvent])
                {
                    $stopped = $true
                }

                Write-Host $e
            }
        }
    }

    if($stopped -eq $true)
    {
        break
    }

    Start-Sleep -Seconds 1
}

Stop-ServiceFabricChaos
```

