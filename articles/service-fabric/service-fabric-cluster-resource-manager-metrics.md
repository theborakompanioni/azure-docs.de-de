---
title: Verwalten der Auslastung von Azure-Microservices mithilfe von Metriken | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Metriken in Service Fabric konfigurieren und verwenden, um den Ressourcenverbrauch von Diensten zu verwalten.
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 0d622ea6-a7c7-4bef-886b-06e6b85a97fb
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/05/2017
ms.author: masnider
translationtype: Human Translation
ms.sourcegitcommit: bb27d279396aa7b670187560cebe2ed074576bad
ms.openlocfilehash: c0e5418574920495bf277f4127b97ff5b07b56ef


---
# <a name="managing-resource-consumption-and-load-in-service-fabric-with-metrics"></a>Verwalten von Ressourcenverbrauch und Auslastung in Service Fabric mit Metriken
„Metriken“ sind in Service Fabric die generische Bezeichnung für die Ressourcen, die für Ihre Dienste wichtig sind und die von den Knoten im Cluster bereitgestellt werden. Generell ist eine Metrik ein beliebiges Element, das Sie verwalten möchten, um die Leistung Ihrer Dienste zu steuern.

Arbeitsspeicher, Festplatte und CPU-Auslastung sind Beispiele für Metriken. Es handelt sich hierbei um physische Metriken, also Ressourcen, die physischen Ressourcen auf dem Knoten entsprechen und verwaltet werden müssen. In vielen Fällen handelt es sich bei Metriken aber um logische Metriken. Beispiele für logische Metriken wären „MyWorkQueueDepth“, „MessagesToProcess“ und „TotalRecords“. Logische Metriken werden von der Anwendung definiert und entsprechen dem Verbrauch einer physischen Ressource, bei dem die Anwendung jedoch nicht genau weiß, wie sie ihn messen soll. Dies ist recht häufig der Fall. Die meisten verwendeten Metriken sind logische Metriken. Das liegt häufig daran, dass viele Dienste heutzutage in verwaltetem Code geschrieben sind. Verwalteter Code bedeutet, dass es innerhalb eines Hostprozesses schwierig sein kann, den Verbrauch physischer Ressourcen eines einzelnen Dienstobjekts zu messen und zu melden. Die Komplexität der Messung und Meldung Ihrer eigenen Metriken ist auch der Grund, warum wir einige sofort einsetzbare Standardmetriken bereitstellen.

## <a name="default-metrics"></a>Standardmetriken
Angenommen, Sie wissen zunächst noch nicht, welche Ressourcen verbraucht werden bzw. welche Ressourcen für Sie von Bedeutung sind. Sie führen also die Implementierung durch und erstellen die Dienste, ohne Metriken anzugeben. Das ist kein Problem. Der Clusterressourcen-Manager von Service Fabric wählt einige einfache Metriken für Sie aus. Wenn Sie keine Metriken angeben, verwenden wir aktuell folgende Standardmetriken: „PrimaryCount“, „ReplicaCount“ und „Count“. In der folgenden Tabelle ist angegeben, welche Auslastung für diese Metriken mit den einzelnen Dienstobjekten verbunden ist:

| Metrik | Auslastung für zustandslose Instanz | Zustandsbehaftete sekundäre Auslastung | Zustandsbehaftete primäre Auslastung |
| --- | --- | --- | --- |
| PrimaryCount |0 |0 |1 |
| ReplicaCount |0 |1 |1 |
| Count |1 |1 |1 |

Was erhalten Sie bei diesen Standardmetriken? Bei einfachen Workloads erhalten Sie eine gute Arbeitsaufteilung. Im folgenden Beispiel werden zwei Dienste erstellt. Der erste ist ein zustandsbehafteter Dienst mit drei Partitionen und der Zielreplikatgruppen-Größe „3“. Der zweite ist ein zustandsloser Dienst mit einer einzelnen Partition und drei Instanzen:

<center>
![Clusterlayout mit Standardmetriken][Image1]
</center>

Ergebnis:

* Primäre Replikate für den zustandsbehafteten Dienst sind nicht auf einem einzelnen Knoten gestapelt.
* Die Replikate einer Partition befinden sich nicht auf demselben Knoten.
* Die Gesamtanzahl primärer und sekundärer Replikate ist über den Cluster verteilt.
* Die Gesamtanzahl von Dienstobjekten ist auf den einzelnen Knoten gleichmäßig zugewiesen.

So weit, so gut.

Das funktioniert hervorragend, bis Sie damit beginnen, große Mengen von echten Workloads auszuführen: Wie hoch ist die Wahrscheinlichkeit, dass sich mit dem gewählten Partitionierungsschema eine absolut gleichmäßige Auslastung für alle Partitionen erreichen lässt? Wie hoch ist die Wahrscheinlichkeit, dass die Auslastung für einen bestimmten Dienst dauerhaft konstant (oder auch nur zum aktuellen Zeitpunkt gleich) ist?

Sie könnten nun zwar einfach die Standardmetriken verwenden, dies führt aber in der Regel dazu, dass Ihre Clusterauslastung geringer ausfällt als gewünscht. Das liegt daran, dass die Berichterstellung für die Standardmetriken nicht adaptiv ist und davon ausgeht, dass alles gleichwertig ist. Im schlimmsten Fall kann dies auch zu überlasteten Knoten und damit zu Leistungseinbußen führen. Besser geeignet sind benutzerdefinierte Metriken und Berichte zur dynamischen Auslastung. Darum geht es in den folgenden Abschnitten. In der Praxis ist die Wahrscheinlichkeit, dass die Workloads aller Dienste dauerhaft gleich sind, äußerst gering. Wenn Sie also Ihren Cluster optimal nutzen und Leistungsprobleme vermeiden möchten, sollten Sie sich mit benutzerdefinierten Metriken befassen.

## <a name="custom-metrics"></a>Benutzerdefinierte Metriken
Metriken werden beim Erstellen des Diensts individuell für benannte Dienstinstanzen konfiguriert.

Jede Metrik verfügt über beschreibende Eigenschaften wie Name, Standardauslastung und Gewichtung.

* Metrikname: Der Name der Metrik. Der Metrikname ist ein eindeutiger Bezeichner für die Metrik (innerhalb des Clusters aus der Perspektive von Resource Manager).
* Standardauslastung: Die Standardauslastung wird unterschiedlich repräsentiert, je nachdem, ob der Dienst zustandslos oder zustandsbehaftet ist.
  * Bei zustandslosen Diensten besitzt jede Metrik eine einzelne Eigenschaft namens „DefaultLoad“.
  * Bei zustandsbehafteten Diensten definieren Sie Folgendes:
    * PrimaryDefaultLoad: Die Standardmenge dieser Metrik, die der Dienst verbraucht, wenn es sich dabei um ein primäres Replikat handelt.
    * SecondaryDefaultLoad: Die Standardmenge dieser Metrik, die der Dienst verbraucht, wenn es sich dabei um ein sekundäres Replikat handelt.
* Weight: Die Metrikgewichtung gibt die Bedeutung der Metrik in Relation zu den anderen Metriken für diesen Dienst an.

Wenn Sie benutzerdefinierte Metriken definieren und auch die Standardmetriken verwenden möchten, müssen Sie sie explizit wieder hinzufügen. Dies ist erforderlich, um die Beziehung zwischen den Standardmetriken und Ihren benutzerdefinierten Metriken deutlich zu machen. Vielleicht ist Ihnen ja die Arbeitsspeichernutzung oder die Tiefe der Warteschlange deutlich wichtiger als die primäre Verteilung.

### <a name="defining-metrics-for-your-service---an-example"></a>Definieren von Metriken für Ihren Dienst – ein Beispiel
Angenommen, Sie möchten einen Dienst konfigurieren, der eine Metrik namens „MemoryInMb“ meldet, und Sie möchten auch die Standardmetriken verwenden. Nehmen wir weiter an, Sie haben im Zuge einiger Messungen ermittelt, dass für ein primäres Replikat des Diensts in der Regel 20 Einheiten von „MemoryInMb“ verbraucht werden, während für sekundäre Replikate fünf Einheiten anfallen. Sie wissen, dass der Arbeitsspeicher die wichtigste Metrik für die Verwaltung der Leistung dieses speziellen Diensts ist, legen aber trotzdem Wert auf ausgewogene primäre Replikate. Die Verwendung ausgewogener primärer Replikate empfiehlt sich, damit der Verlust eines Knotens oder einer Fehlerdomäne nicht gleichzeitig die Mehrheit der primären Replikate beeinträchtigt. Abgesehen von diesen geringfügigen Anpassungen möchten Sie jedoch die Standardmetriken verwenden.

Hier sehen Sie den Code, den Sie schreiben würden, um einen Dienst mit dieser Metrikkonfiguration zu erstellen:

Code:

```csharp
StatefulServiceDescription serviceDescription = new StatefulServiceDescription();
StatefulServiceLoadMetricDescription memoryMetric = new StatefulServiceLoadMetricDescription();
memoryMetric.Name = "MemoryInMb";
memoryMetric.PrimaryDefaultLoad = 20;
memoryMetric.SecondaryDefaultLoad = 5;
memoryMetric.Weight = ServiceLoadMetricWeight.High;

StatefulServiceLoadMetricDescription primaryCountMetric = new StatefulServiceLoadMetricDescription();
primaryCountMetric.Name = "PrimaryCount";
primaryCountMetric.PrimaryDefaultLoad = 1;
primaryCountMetric.SecondaryDefaultLoad = 0;
primaryCountMetric.Weight = ServiceLoadMetricWeight.Medium;

StatefulServiceLoadMetricDescription replicaCountMetric = new StatefulServiceLoadMetricDescription();
replicaCountMetric.Name = "ReplicaCount";
replicaCountMetric.PrimaryDefaultLoad = 1;
replicaCountMetric.SecondaryDefaultLoad = 1;
replicaCountMetric.Weight = ServiceLoadMetricWeight.Low;

StatefulServiceLoadMetricDescription totalCountMetric = new StatefulServiceLoadMetricDescription();
totalCountMetric.Name = "Count";
totalCountMetric.PrimaryDefaultLoad = 1;
totalCountMetric.SecondaryDefaultLoad = 1;
totalCountMetric.Weight = ServiceLoadMetricWeight.Low;

serviceDescription.Metrics.Add(memoryMetric);
serviceDescription.Metrics.Add(primaryCountMetric);
serviceDescription.Metrics.Add(replicaCountMetric);
serviceDescription.Metrics.Add(totalCountMetric);

await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

Powershell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton –Metric @("MemoryInMb,High,20,5”,"PrimaryCount,Medium,1,0”,"ReplicaCount,Low,1,1”,"Count,Low,1,1”)
```

(Zur Erinnerung: Falls Sie nur die Standardmetriken verwenden möchten, müssen Sie beim Erstellen des Diensts weder die Sammlung mit den Metriken verwenden noch sonstige Einstellungen konfigurieren.)

Nach der Einführung und dem Beispiel beschäftigen wir uns nun ausführlicher mit den einzelnen Einstellungen und mit dem Verhalten, das Sie erhalten.

## <a name="load"></a>Auslastung
Metriken werden definiert, um eine Auslastung darzustellen. Die Auslastung gibt an, welcher Anteil einer bestimmten Metrik von einer Dienstinstanz oder von einem Replikat auf einem bestimmten Knoten verbraucht wird. Die erwartete Auslastung kann bei der Diensterstellung konfiguriert, nach der Diensterstellung aktualisiert und/oder für einzelne Dienstobjekte gemeldet werden.

## <a name="default-load"></a>Standardauslastung
Die Standardauslastung gibt die Auslastung an, die standardmäßig bei den einzelnen Dienstobjekten (Instanz oder Replikat) des Diensts anfällt. Bei einfacheren Diensten ist die Standardauslastung eine statische Definition, die niemals aktualisiert und für die gesamte Lebensdauer des Diensts verwendet wird. Die Standardauslastung eignet sich gut für einfache Kapazitätsplanungsszenarien, in denen verschiedenen Workloads bestimmte Ressourcenmengen zugewiesen werden.

Der Clusterressourcen-Manager ermöglicht zustandsbehafteten Diensten die Angabe unterschiedlicher Standardauslastungen für primäre und sekundäre Replikate. Bei zustandslosen Diensten kann hingegen nur ein einzelner Wert angegeben werden. Bei zustandsbehafteten Diensten unterscheidet sich die Auslastung für primäre Replikate üblicherweise von der Auslastung für sekundäre Replikate, da Replikate in jeder Rolle verschiedene Arten von Aufgaben übernehmen. So übernehmen primäre Replikate im Gegensatz zu sekundären Replikaten in der Regel sowohl Lese- als auch Schreibvorgänge (sowie den Großteil der Rechenlast). Zwar ist davon auszugehen, dass die Standardauslastung für ein primäres Replikat höher ausfällt als bei sekundären Replikaten, die tatsächlichen Werte hängen jedoch von Ihren eigenen Messungen ab.

## <a name="dynamic-load"></a>Dynamische Auslastung
Nehmen wir an, Sie haben den Dienst bereits eine Zeitlang ausgeführt. Dabei haben Sie aufgrund der Überwachung Folgendes festgestellt:

1. Einige Partitionen oder Instanzen eines bestimmten Diensts verbrauchen mehr Ressourcen als andere.
2. Die Auslastung einiger Dienste verändert sich im Laufe der Zeit.

Diese Auslastungsschwankungen können verschiedenste Ursachen haben. Möglicherweise ist der Dienst oder die Partition einem bestimmten Kunden zugeordnet, oder der Dienst/die Partition hängt mit Workloads zusammen, die im Laufe des Tages variieren. Was auch immer die Ursache ist: Sie können keinen einzelnen Wert als Standardauslastung verwenden. Jeder Wert, den Sie für die Standardauslastung festlegen, ist für einen gewissen Zeitraum unzutreffend. Das ist problematisch, da der Clusterressourcen-Manager Ihrem Dienst im Falle einer falschen Standardauslastung entweder zu viel oder zu wenig Ressourcen zuweist. Dies führt zu einer übermäßigen oder unzureichenden Auslastung von Knoten, auch wenn der Cluster für den Clusterressourcen-Manager ausgewogen erscheint. Standardauslastungen haben einen gewissen Informationsgehalt und sind daher nicht schlecht, vermitteln in der Praxis aber zumeist kein umfassendes Bild von den Workloads. Aus diesem Grund ermöglicht der Clusterressourcen-Manager den einzelnen Dienstobjekten die Aktualisierung der eigenen Auslastung zur Laufzeit. Dies wird als „dynamisches Melden der Auslastung“ bezeichnet.

Mit Berichten zur dynamischen Auslastung können Replikate oder Instanzen die Zuordnung bzw. die gemeldete Auslastung von Metriken im Laufe ihrer Lebensdauer anpassen. Dienstreplikate oder -instanzen, die gerade nicht aktiv sind und nichts zu tun haben, melden in der Regel eine geringe Nutzung einer bestimmten Metrik. Aktive Replikate oder Instanzen melden eine höhere Auslastung.

Auf der Grundlage replikat- oder instanzspezifischer Berichte kann der Clusterressourcen-Manager die einzelnen Dienstobjekte im Cluster so organisieren, dass den Diensten die benötigten Ressourcen zur Verfügung stehen. Dadurch können aktive Dienste Ressourcen von anderen Replikaten oder Instanzen abziehen, die gerade nicht aktiv sind oder weniger zu tun haben.

Innerhalb Ihres zuverlässigen Diensts sieht der Code zum dynamischen Melden der Auslastung wie folgt aus:

Code:

```csharp
this.ServicePartition.ReportLoad(new List<LoadMetric> { new LoadMetric("MemoryInMb", 1234), new LoadMetric("metric1", 42) });
```

Dienstreplikate oder -instanzen können die Auslastung nur für die Metriken melden, für deren Verwendung sie bei der Erstellung konfiguriert wurden. Bei der Liste mit den Metriken, die von einem Dienst gemeldet werden können, handelt es sich um die gleichen Metriken, die im Zuge der Diensterstellung angegeben wurden. Die Liste mit den Metriken, die dem Dienst zugeordneten sind, kann auch dynamisch aktualisiert werden. Wenn ein Dienstreplikat oder eine Instanz versucht, die Auslastung für eine Metrik zu melden, für deren Verwendung das Replikat oder die Instanz derzeit nicht konfiguriert ist, protokolliert Service Fabric die Meldung, ignoriert sie aber. Falls im Rahmen des gleichen API-Aufrufs auch noch andere (gültige) Metriken gemeldet werden, werden diese akzeptiert und verwendet. Dadurch haben Sie mehr Spielraum bei Experimenten. Der Code kann sämtliche ihm bekannte Metriken ermitteln und melden, und der Bediener kann die Metrikkonfiguration für den Dienst angeben und aktualisieren, ohne den Code zu ändern. So kann etwa der Administrator oder das Betriebsteam eine Metrik mit einem fehlerhaften Bericht für einen bestimmten Dienst deaktivieren, die Gewichtungskonfiguration von Metriken auf der Grundlage des Verhaltens anpassen oder eine neue Metrik erst aktivieren, nachdem der Code über andere Mechanismen bereitgestellt und überprüft wurde.

## <a name="mixing-default-load-values-and-dynamic-load-reports"></a>Kombinieren von Standardauslastungswerten und dynamischen Auslastungsberichten
Wenn die Standardauslastung nicht ausreicht und dynamische Auslastungsberichte empfohlen werden, kann ich dann beides verwenden? Ja. Dies ist sogar die empfohlene Konfiguration. Wenn eine Standardauslastung festgelegt wird und gleichzeitig dynamische Auslastungsberichte verwendet werden, fungiert die Standardauslastung als Schätzwert, bis dynamische Berichte vorliegen. So hat der Clusterressourcen-Manager etwas, womit er arbeiten kann. Auf der Grundlage der Standardauslastung kann der Clusterressourcen-Manager die Dienstobjekte bei deren Erstellung sinnvoll platzieren. Ohne Angabe einer Standardauslastung werden die Dienste nach dem Zufallsprinzip platziert. Wenn dann später Auslastungsberichte eingehen, muss der Clusterressourcen-Manager fast immer Dienste verschieben.

Im Anschluss sehen wir uns anhand des vorherigen Beispiels an, was passiert, wenn wir einige benutzerdefinierte Metriken sowie dynamische Auslastungsberichte hinzufügen. In diesem Beispiel verwenden wir den Arbeitsspeicher (Memory) als Beispielmetrik. Außerdem nehmen wir an, dass der zustandsbehaftete Dienst ursprünglich mit dem folgenden Befehl erstellt wurde:

Powershell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton –Metric @("MemoryInMb,High,21,11”,"PrimaryCount,Medium,1,0”,"ReplicaCount,Low,1,1”,"Count,Low,1,1”)
```

Syntax: ("Metrikname, Metrikgewichtung, Standardauslastung des primären Replikats, Standardauslastung des sekundären Replikats").

Ein mögliches Clusterlayout kann beispielsweise wie folgt aussehen:

<center>
![Ausgeglichener Cluster mit Standardmetriken und benutzerdefinierten Metriken][Image2]
</center>

Beachten Sie folgende Punkte:

* Da Replikate oder Instanzen die Standardauslastung des Diensts verwenden, bis sie ihre eigene Auslastung melden, wissen wir, dass die Replikate in Partition 1 des zustandsbehafteten Diensts noch keine dynamische Auslastung gemeldet haben.
* Sekundäre Replikate innerhalb einer Partition können über eine eigene Auslastung verfügen.
* Insgesamt machen die Metriken einen ausgeglichenen Eindruck. Für den Arbeitsspeicher liegt das Verhältnis zwischen maximaler und minimaler Auslastung bei 1,75. (Der Knoten mit der höchsten Auslastung ist N3, der Knoten mit der geringsten Auslastung ist N2, also: 28/16 = 1,75.)

Einige Punkte müssen noch erläutert werden:

* Wie wird bestimmt, ob ein Verhältnis von 1,75 gut oder nicht gut ist? Woher weiß der Clusterressourcen-Manager, ob dieses Ergebnis ausreicht oder ob noch weitere Optimierungen erforderlich sind?
* Wann wird ein Lastenausgleich durchgeführt?
* Was bedeutet es, dass „Memory“ die Gewichtung „Hoch“ erhalten hat?

## <a name="metric-weights"></a>Metrikgewichtungen
Die gleichen Metriken müssen über verschiedene Dienste hinweg nachverfolgbar sein. Dadurch kann der Clusterressourcen-Manager den Verbrauch im Cluster überwachen, knotenübergreifend ausgleichen und sicherstellen, dass die Kapazität von Knoten nicht überschritten wird. Die gleiche Metrik hat jedoch bei unterschiedlichen Diensten unter Umständen nicht den gleichen Stellenwert. Außerdem sind perfekt ausgewogene Lösungen in einem Cluster mit zahlreichen Metriken und einer Vielzahl von Diensten unter Umständen gar nicht für alle Metriken möglich. Wie soll der Clusterressourcen-Manager mit solchen Situationen umgehen?

Dank Metrikgewichtungen kann der Clusterressourcen-Manager entscheiden, wie der Cluster ausgeglichen werden soll, falls keine perfekte Lösung verfügbar ist. Auf der Grundlage von Metrikgewichtungen kann der Clusterressourcen-Manager außerdem bestimmte Dienste unterschiedlich ausgleichen. Metriken können vier unterschiedliche Gewichtungen haben: Null, Niedrig, Mittel und Hoch. Eine Metrik mit der Gewichtung „Null“ hat zwar keine Auswirkung auf die Frage, ob ein guter Ausgleich besteht, ihre Auslastung wird jedoch für bestimmte Aspekte (etwa die Kapazität) berücksichtigt.

Der tatsächliche Nutzen von Metrikgewichtungen im Cluster besteht darin, dass der Clusterressourcen-Manager unterschiedliche Lösungen generiert. Mithilfe von Metrikgewichtungen wird dem Clusterressourcen-Manager vermittelt, dass bestimmte Metriken einen höheren Stellenwert haben als andere. Sollte keine perfekte Lösung verfügbar sein, kann der Clusterressourcen-Manager Lösungen bevorzugen, bei denen die höher gewichteten Metriken besser ausgeglichen werden. Falls ein Dienst eine Metrik als unwichtig betrachtet, entsteht für ihn unter Umständen der Eindruck, dass die Verwendung dieser Metrik zu einer Unausgewogenheit führt. Dadurch kann bei einem anderen Dienst, bei dem dies wichtig ist, eine ausgewogene Verteilung erzielt werden.

Im Anschluss sehen wir uns ein Beispiel für einige Auslastungsberichte an und beschäftigen uns damit, wie unterschiedliche Metrikgewichtungen zu unterschiedlichen Zuordnungen im Cluster führen können. In diesem Beispiel sehen Sie, dass die Umstellung der relativen Metrikgewichtung dazu führt, dass der Clusterressourcen-Manager unterschiedliche Lösungen vorzieht und unterschiedliche Dienstanordnungen erstellt.

<center>
![Beispiel für die Metrikgewichtung und deren Auswirkungen auf Ausgleichslösungen][Image3]
</center>

In diesem Beispiel gibt es vier Dienste, die alle unterschiedliche Werte für zwei Metriken (A und B) melden. In einem Fall definieren alle Dienste „MetricA“ als wichtigste Metrik (hohe Gewichtung) und „MetricB“ als unwichtig (niedrige Gewichtung). In diesem Fall platziert der Clusterressourcen-Manager die Dienste so, dass „MetricA“ im Vergleich zu „MetricB“ besser ausgeglichen ist (also eine geringere Standardabweichung aufweist). Im zweiten Fall kehren wir die Metrikgewichtungen um. Daraufhin vertauscht der Clusterressourcen-Manager wahrscheinlich die Dienste A und B, um eine Zuordnung zu erzielen, bei der „MetricB“ im Vergleich zu „MetricA“ einen besseren Ausgleich aufweist.

### <a name="global-metric-weights"></a>Globale Metrikgewichtungen
Wenn ServiceA also MetricA als wichtigste Metrik definiert und diese für ServiceB keine Rolle spielt, welche Gewichtung wird dann letztendlich verwendet?

Für jede Metrik werden eigentlich mehrere Gewichtungen nachverfolgt. Bei der ersten Gruppe handelt es sich um die Gewichtungen, die jeder Dienst für die Metrik definiert hat. Die andere Gewichtung ist eine globale Gewichtung. Hierbei handelt es sich um den Durchschnitt aller Dienste, die diese Metrik melden. Bei der Berechnung der Lösungspunktzahlen werden beide Gewichtungen vom Clusterressourcen-Manager berücksichtigt. Der Grund: Es kommt nicht nur darauf an, dass ein Dienst auf der Grundlage seiner eigenen Prioritäten ausgeglichen ist, sondern auch, dass die Zuordnung des gesamten Clusters korrekt ist.

Was würde passieren, wenn sich der Clusterressourcen-Manager nicht um die globale und lokale Ausgewogenheit kümmert? Es ist keine große Herausforderung, global ausgewogene Lösungen zu erstellen, die aber ansonsten eine unzureichende Ressourcenzuordnung für einzelne Dienste bieten. Im folgenden Beispiel betrachten wir die Standardmetriken, mit denen ein zustandsbehafteter Dienst konfiguriert ist, und sehen uns an, was passiert, wenn lediglich die globale Ausgewogenheit berücksichtigt wird:

<center>
![Auswirkung einer rein globalen Lösung][Image4]
</center>

Im oberen Beispiel, in dem wir uns nur den globalen Ausgleich angesehen haben, ist der Cluster insgesamt tatsächlich ausgeglichen. Alle Knoten verfügen über die gleiche Anzahl von primären Replikaten sowie über die gleiche Gesamtanzahl von Replikaten. Wenn wir uns aber die tatsächlichen Auswirkungen dieser Zuordnung ansehen, ist das Ergebnis nicht so gut: Der Verlust eines Knotens wirkt sich unverhältnismäßig stark auf eine bestimmte Workload aus, da alle zugehörigen primären Replikate davon betroffen sind. Wenn also beispielsweise der erste Knoten ausfällt, gehen die drei primären Replikate für die drei verschiedenen Partitionen des Diensts „Circle“ verloren. Für die anderen beiden Dienste („Triangle“ und „Hexagon“) verlieren die Partitionen ein Replikat, was nicht zu Problemen führt (bis auf die Wiederherstellung des ausgefallenen Replikats).

Im unteren Beispiel hat der Clusterressourcen-Manager die Replikate sowohl auf der Grundlage des globalen Ausgleichs als auch auf der Grundlage des dienstspezifischen Ausgleichs verteilt. Beim Berechnen der Lösungspunktzahl entfällt der Großteil der Gewichtung auf die globale Lösung und ein (konfigurierbar) Teil auf einzelne Dienste. Der globale Ausgleich wird auf der Grundlage des Durchschnitts der Metrikgewichtungen berechnet, die für die einzelnen Dienste konfiguriert wurden. Jeder Dienst wird gemäß seiner eigenen definierten Metrikgewichtungen ausgeglichen. Dadurch wird sichergestellt, dass die Dienste gemäß ihren jeweiligen Anforderungen bestmöglich untereinander ausgeglichen sind. Wenn nun der erste Knoten ausfällt, verteilt sich der Verlust der primären (und sekundären) Replikate auf alle Partitionen aller Dienste. Die Auswirkungen sind also für alle gleich.

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zu den anderen Optionen, die für die Konfiguration von Diensten zur Verfügung stehen, finden Sie im Thema zu den anderen verfügbaren Clusterressourcen-Manager-Konfigurationen unter [Konfigurieren von Diensten](service-fabric-cluster-resource-manager-configure-services.md).
* Das Definieren von Defragmentierungsmetriken ist eine Möglichkeit, die Last auf Knoten zu konsolidieren, statt sie auszubreiten. Informationen zum Konfigurieren der Defragmentierung finden Sie in [diesem Artikel](service-fabric-cluster-resource-manager-defragmentation-metrics.md)
* Informationen darüber, wie der Clusterressourcen-Manager die Auslastung im Cluster verwaltet und verteilt, finden Sie im Artikel zum [Lastenausgleich](service-fabric-cluster-resource-manager-balancing.md)
* Starten Sie mit einer [Einführung in den Clusterressourcen-Manager von Service Fabric](service-fabric-cluster-resource-manager-introduction.md)
* Bewegungskosten sind eine Möglichkeit, dem Clusterressourcen-Manager mitzuteilen, dass bestimmte Dienste teurer zu bewegen sind als andere. Weitere Informationen zu Bewegungskosten finden Sie in [diesem Artikel](service-fabric-cluster-resource-manager-movement-cost.md)

[Image1]:./media/service-fabric-cluster-resource-manager-metrics/cluster-resource-manager-cluster-layout-with-default-metrics.png
[Image2]:./media/service-fabric-cluster-resource-manager-metrics/Service-Fabric-Resource-Manager-Dynamic-Load-Reports.png
[Image3]:./media/service-fabric-cluster-resource-manager-metrics/cluster-resource-manager-metric-weights-impact.png
[Image4]:./media/service-fabric-cluster-resource-manager-metrics/cluster-resource-manager-global-vs-local-balancing.png



<!--HONumber=Jan17_HO4-->


