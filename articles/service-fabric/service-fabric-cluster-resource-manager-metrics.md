<properties
   pageTitle="Verwalten von Metriken mit dem Clusterressourcen-Manager von Azure Service Fabric | Microsoft Azure"
   description="Enthält Informationen zum Konfigurieren und Verwenden von Metriken in Service Fabric."
   services="service-fabric"
   documentationCenter=".net"
   authors="masnider"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/19/2016"
   ms.author="masnider"/>

# Verwalten von Ressourcenverbrauch und Auslastung in Service Fabric mit Metriken
„Metriken“ sind in Service Fabric die generische Bezeichnung für die Ressourcen, die für Ihre Dienste wichtig sind und die von den Knoten im Cluster bereitgestellt werden. Im Allgemeinen ist eine Metrik jedes Element, das Sie verwalten möchten, um die Leistung Ihrer Dienste zu steuern.

Arbeitsspeicher, Festplatten, CPU-Auslastung – all dies sind Beispiele für Metriken. Es handelt sich hierbei um physische Metriken, also Ressourcen, die physischen Ressourcen auf dem Knoten entsprechen und verwaltet werden müssen. Metriken können auch logische Metriken sein (und sind dies häufig auch), die anwendungsdefiniert sind und einer bestimmten Ebene des Ressourcenverbrauchs entsprechen (wobei die Anwendung darüber aber keine eindeutigen Informationen besitzt und nicht weiß, wie diese gemessen wird). MyWorkQueueDepth ist ein Beispiel für eine solche Metrik. Die meisten verwendeten Metriken sind logische Metriken. Es gibt zahlreiche Gründe hierfür, aber der häufigste Grund ist, dass heute viele unserer Kunden ihre Dienste in verwaltetem Code schreiben, und aus einer bestimmten zustandslosen Dienstinstanz oder einem zustandsbehafteten Dienstreplikatobjekt heraus ist es tatsächlich ziemlich schwierig, die Nutzung tatsächlicher physischer Ressourcen zu messen und zu melden. Die Komplexität des Berichtens Ihrer eigenen Metriken ist auch der Grund, warum wir einige Standardmetriken sofort einsetzbar bereitstellen.

## Standardmetriken
Angenommen, Sie möchten die ersten Schritte ausführen und wissen nicht, welche Ressourcen genutzt werden bzw. welche Ressourcen wichtig sein werden. Sie führen also die Implementierung durch und erstellen die Dienste, ohne Metriken anzugeben. Das ist kein Problem. Wir wählen einige Metriken für Sie aus. Die Standardmetriken, die wir derzeit für Sie verwenden, wenn Sie keine eigenen Metriken angeben, haben die Bezeichnungen „PrimaryCount“, „ReplicaCount“ und (zugegebenermaßen etwas vage) „Count“. In der folgenden Tabelle ist angegeben, welche Auslastung für diese Metriken mit jedem Dienstobjekt verbunden ist:

| Metrik | Auslastung für zustandslose Instanz |	Zustandsbehaftete sekundäre Auslastung |	Zustandsbehaftete primäre Auslastung |
|--------|--------------------------|-------------------------|-----------------------|
| PrimaryCount | 0 |	0 |	1 |
| ReplicaCount | 0 | 1 | 1 |
| Count |	1 |	1 |	1 |

Was erhalten Sie bei diesen Standardmetriken? Für einfache Workloads erhalten Sie eine ziemlich gute Verteilung der Arbeit. Im Beispiel unten wird veranschaulicht, was passiert, wenn wir einen zustandsbehafteten Dienst mit drei Partitionen und einer Größe der Zielreplikatgruppe von „3“ erstellen. Außerdem erstellen wir einen einzelnen zustandslosen Dienst mit einer Instanzanzahl von 3. Das Ergebnis ist wie folgt:

![Clusterlayout mit Standardmetriken][Image1]

In diesem Beispiel ist Folgendes zu beobachten:
-	Primäre Replikate für den zustandsbehafteten Dienst sind nicht auf einem einzelnen Knoten gestapelt.
-	Die Replikate einer Partition befinden sich nicht auf demselben Knoten.
-	Die primären und sekundären Replikate sind im Cluster gut verteilt.
-	Die Dienstobjekte (zustandslos und zustandsbehaftet) sind gleichmäßig auf die einzelnen Knoten verteilt.

Das ist gut!

Dies funktioniert hervorragend, bis Sie beginnen, darüber nachzudenken: Wie hoch ist die Wahrscheinlichkeit, dass das Partitionierungsschema, das Sie ausgewählt haben, eine dauerhaft absolut gleichmäßige Auslastung aller Partitionen garantiert? Wie hoch ist in Verbindung hiermit die Wahrscheinlichkeit, dass die Auslastung für einen bestimmten Dienst dauerhaft, und auch in diesem Augenblick, konstant ist? Dann zeigt es sich, dass die Wahrscheinlichkeit, dass alle Replikate gleichwertig sind, für alle wichtige Workloads tatsächlich eher niedrig ist. Wenn Sie also an der optimalen Nutzung Ihres Clusters interessiert sind, sollten Sie damit beginnen, benutzerdefinierte Metriken auszuwerten.

Realistisch gesehen könnten Sie einfach die Standardmetriken verwenden. Dies bedeutet aber üblicherweise, dass Ihre Clusterauslastung geringer als gewünscht ist (da die Berichterstellung nicht adaptiv ist und davon ausgeht, dass alle Elemente gleichwertig sind). Im schlimmsten Fall kann dies auch zu überlasteten Knoten und somit zu Leistungseinbußen führen. Besser geeignet sind benutzerdefinierte Metriken und Berichte zur dynamischen Auslastung. Darum geht es in den folgenden Abschnitten.

## Benutzerdefinierte Metriken
Wir haben bereits erörtert, dass es sowohl physische als auch logische Metriken gibt, und dass Sie Ihre eigenen Metriken definieren können. Prima. Aber wie? Es ist eigentlich recht einfach! Konfigurieren Sie die Metrik und die standardmäßige Anfangsauslastung einfach beim Erstellen des Diensts. Damit sind Sie schon fertig! Wenn Sie den Dienst erstellen, können Sie für jede benannte Dienstinstanz eine beliebige Gruppe von Metriken und Standardwerten konfigurieren, die den erwarteten Verbrauch durch den Dienst repräsentieren.

Beachten Sie Folgendes: Wenn Sie mit dem Definieren von benutzerdefinierten Metriken beginnen, müssen Sie die Standardmetriken explizit wieder hinzufügen, wenn Sie möchten, dass diese von uns ebenfalls zum Ausbalancieren Ihres Diensts verwendet werden sollen. Der Grund ist, dass Sie sich über die Beziehung zwischen Standardmetriken und Ihren benutzerdefinierten Metriken im Klaren sein sollen. Es kann beispielsweise sein, dass die Arbeitsspeicherauslastung bzw. WorkQueueDepth für Sie deutlich wichtiger als die Verteilung der primären Replikate ist.

Angenommen, Sie möchten einen Dienst konfigurieren, von dem eine Metrik mit der Bezeichnung „Arbeitsspeicher“ gemeldet wird (zusätzlich zu den Standardmetriken). Sie haben einige grundlegende Messungen für den Arbeitsspeicher durchgeführt und ermittelt, dass für ein primäres Replikat des Diensts 20 MB Arbeitsspeicher verbraucht werden, während für sekundäre Replikate desselben Diensts 5 MB benötigt werden. Sie wissen, dass „Arbeitsspeicher“ die wichtigste Metrik in Bezug auf die Verwaltung der Leistung dieses Diensts ist. Sie möchten aber trotzdem erreichen, dass primäre Replikate gleichmäßig verteilt werden, damit beim Verlust eines Knotens oder einer Fehlerdomäne nicht eine übermäßig hohe Zahl von primären Replikaten ebenfalls verloren geht. Ansonsten verwenden Sie die Standardmetriken.

Gehen Sie wie folgt vor:

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
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton –Metric @("Memory,High,20,5”,"PrimaryCount,Medium,1,0”,"ReplicaCount,Low,1,1”,"Count,Low,1,1”)
```

(Zur Erinnerung: Falls Sie nur die Standardmetriken verwenden möchten, müssen Sie beim Erstellen des Diensts weder die Sammlung mit den Metriken verwenden noch sonstige Einstellungen konfigurieren.)

Nachdem Sie erfahren haben, wie Sie Ihre eigenen Metriken definieren, geht es nun um die unterschiedlichen Eigenschaften, die Metriken aufweisen können. Wir haben sie Ihnen bereits gezeigt, aber jetzt wird beschrieben, was sie genau bedeuten. Derzeit kann eine Metrik vier verschiedene Eigenschaften haben:

-	Metrikname: Dies ist der Name der Metrik. Es handelt sich hierbei um einen eindeutigen Bezeichner für die Metrik im Cluster aus Resource Manager-Sicht.
- Standardauslastung: Die Standardauslastung wird unterschiedlich repräsentiert, je nachdem, ob der Dienst zustandslos oder zustandsbehaftet ist.
  - Bei zustandslosen Diensten weist jede Metrik nur eine einzige Eigenschaft für die Standardauslastung auf.
  - Für zustandsbehaftete Dienste definieren Sie
    -	PrimaryDefaultLoad: Dies ist die standardmäßige Auslastungsmenge, die für den Dienst für diese Metrik als primäres Replikat anfällt.
    -	SecondaryDefaultLoad: Dies ist die standardmäßige Auslastungsmenge, die für den Dienst für diese Metrik als sekundäres Replikat anfällt.
-	Gewichtung: Gibt an, wie wichtig die Metrik relativ zu den anderen konfigurierten Metriken dieses Diensts ist.

## Laden
Die Last oder auch Auslastung ist eine allgemeine Angabe, welcher Anteil einer bestimmten Metrik von einer Dienstinstanz oder einem Replikat auf einem bestimmten Knoten verbraucht wird.

## Standardauslastung
Die Standardauslastung gibt an, welche Auslastung der Clusterressourcen-Manager für die einzelnen Dienstinstanzen oder Replikate des Diensts ansetzen soll, bis Updates von tatsächlichen Dienstinstanzen oder Replikaten eingehen. Für Dienste mit einfachem Aufbau wird dies zu einer statischen Definition, die niemals dynamisch aktualisiert und somit während der gesamten Lebensdauer des Diensts verwendet wird. Dies eignet sich gut für eine einfache Kapazitätsplanung, weil es genau die Vorgehensweise ist, die wir kennen: die Zuordnung bestimmter Ressourcen zu bestimmten Workloads. Der Vorteil besteht aber darin, dass wir uns jetzt im Bereich der Microservices befinden. Hierbei werden Ressourcen nicht statisch bestimmten Workloads zugewiesen, und der Mensch ist nicht am Treffen der Entscheidungen beteiligt.

Wir lassen es zu, dass zustandsbehaftete Dienste die Standardauslastung für ihre primären und sekundären Replikate angeben. In Wirklichkeit sehen diese Werte für viele Dienste aufgrund der unterschiedlichen Workloads, die von primären und sekundären Replikaten ausgeführt werden, anders aus. Und da primäre Replikate normalerweise sowohl Lese- als auch Schreibvorgänge bereitstellen (und außerdem die größte Rechenlast tragen), ist die Standardauslastung für ein primäres Replikat höher als für sekundäre Replikate.

Nehmen wir nun an, dass Sie Ihren Dienst eine Zeit lang ausgeführt und bemerkt haben, dass einige Instanzen oder Replikate Ihres Diensts deutlich mehr Ressourcen als andere verbrauchen oder dass der Verbrauch im Laufe der Zeit variiert. Es kann beispielsweise sein, dass sie einem bestimmten Kunden zugeordnet sind oder dass sie nur für bestimmte Workloads bestimmt sind, die sich im Laufe des Tages ändern, z.B. Nachrichtendatenverkehr, Telefongespräche oder Aktienhandel. Ihnen fällt jedenfalls auf, dass es keinen alleinigen Wert gibt, den Sie für die Auslastung angeben können, ohne zumindest über einen bestimmten Zeitraum relativ weit daneben zu liegen. Außerdem bemerken Sie, dass die unrealistische erste Einschätzung dazu führt, dass der Clusterressourcen-Manager Ihrem Dienst entweder zu viele oder zu wenig Ressourcen zuordnet und daher Knoten vorhanden sind, die eine zu hohe oder zu niedrige Auslastung aufweisen.

Vorgehensweise Es ist möglich, dass Ihr Dienst seine Auslastung ständig „on the fly“ meldet.

## Dynamische Auslastung
Mit Berichten zur dynamischen Auslastung können Replikate oder Instanzen die Zuordnung bzw. die gemeldete Auslastung von Metriken im Cluster über ihre Lebensdauer hinweg anpassen. Ein Dienstreplikat oder eine Dienstinstanz, das bzw. die derzeit keine Auslastung aufweist, meldet in der Regel eine geringe Nutzung einer bestimmten Metrik, während Replikate oder Instanzen mit höherer Auslastung eine höhere Nutzung melden. Aufgrund dieser allgemeinen Veränderungen im Cluster können wir die Dienstreplikate und -instanzen im Cluster im laufenden Betrieb neu organisieren, um sicherzustellen, dass die Dienste und Instanzen die benötigten Ressourcen erhalten. Höher ausgelastete Dienste können so Ressourcen von anderen Replikaten oder Instanzen nutzen, die gerade keine oder nur wenig Arbeit verrichten. Das kontinuierliche Melden der Auslastung kann über die ReportLoad-Methode erfolgen, die in ServicePartition verfügbar ist, die wiederum als Eigenschaft der StatefulService- oder StatelessService-Basisklasse über das Reliable Services-Programmiermodell zur Verfügung steht. In Ihrem Dienst würde der Code wie folgt aussehen:

Code:

```csharp
this.ServicePartition.ReportLoad(new List<LoadMetric> { new LoadMetric("Memory", 1234), new LoadMetric("metric1", 42) });
```

Dienstreplikate oder -instanzen können die Auslastung nur für die Metriken melden, für deren Verwendung sie konfiguriert wurden. Die Liste mit den Metriken wird jeweils festgelegt, wenn ein Dienst erstellt wird, und kann später aktualisiert werden. Wenn ein Dienstreplikat oder eine Instanz versucht, die Auslastung für eine Metrik zu melden, die derzeit nicht für die Verwendung konfiguriert ist, protokolliert Service Fabric den Bericht, ignoriert ihn aber. Dies bedeutet, dass er nicht verwendet wird, wenn der Zustand des Clusters berechnet oder gemeldet wird. Dies ist praktisch, weil Sie so besser experimentieren können. Der Code kann alle Dinge messen und melden, für die die Vorgehensweise bekannt ist. Der Bediener kann die Regeln für den Lastenausgleich des Diensts kontinuierlich konfigurieren, optimieren und aktualisieren, ohne jemals den Code ändern zu müssen. Beispiele hierfür sind das Deaktivieren einer Metrik mit einem fehlerhaften Bericht, das Neukonfigurieren der Gewichtungen von Metriken anhand des Verhaltens oder das Aktivieren einer neuen Metrik erst dann, wenn der Code bereits über andere Mechanismen bereitgestellt und überprüft wurde.

## Kombinieren von Standardauslastungswerten und dynamischen Auslastungsberichten
Ist es sinnvoll, eine Standardauslastung für einen Dienst anzugeben, für den die Auslastung dynamisch gemeldet wird? Auf jeden Fall. In diesem Fall dient die Standardauslastung als Schätzung, bis die richtigen Berichte vom jeweiligen Replikat oder der Instanz des Diensts vorhanden sind. Dies ist sehr nützlich, da der Clusterressourcen-Manager die Schätzung der Standardauslastung verwenden kann, um die Dienstinstanzen oder -replikate von Beginn an richtig zu platzieren. Wenn keine Informationen zur Standardauslastung bereitgestellt werden, erfolgt die Dienstplatzierung eher zufällig, und wenn sich die Auslastungen später ändern, muss der Clusterressourcen-Manager mit hoher Wahrscheinlichkeit Elemente verschieben.

Wir sehen uns das vorherige Beispiel an und prüfen, was passiert, wenn wir eine benutzerdefinierte Auslastung hinzufügen und diese nach der Erstellung des Diensts dynamisch aktualisiert wird. In diesem Beispiel verwenden wir „Memory“ (Arbeitsspeicher) und setzen voraus, dass der zustandsbehaftete Dienst zuvor mit dem folgenden Befehl erstellt wurde:

Powershell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton –Metric @("Memory,High,21,11”,"PrimaryCount,Medium,1,0”,"ReplicaCount,Low,1,1”,"Count,Low,1,1”)
```

Diese Syntax wurde bereits beschrieben (MetricName, MetricWeight, PrimaryDefaultLoad, SecondaryDefaultLoad). Weiter unten geht es dann darum, was der jeweilige Wert für „Weight“ (Gewichtung) bedeutet.

Ein mögliches Clusterlayout kann beispielsweise wie folgt aussehen:

![Ausgleich für Cluster mit Standardmetriken und benutzerdefinierten Metriken][Image2]

Beachten Sie folgende Punkte:

-	Da Replikate oder Instanzen die Standardauslastung des Diensts verwenden, bis sie ihre eigene Auslastung melden, wissen wir, dass die Replikate in Partition 1 des zustandsbehafteten Diensts noch keine eigene Auslastung gemeldet haben.
-	Sekundäre Replikate innerhalb einer Partition können über eine eigene Auslastung verfügen.
-	Insgesamt sehen die Metriken ziemlich gut aus. Die Differenz zwischen der maximalen und minimalen Auslastung eines Knotens (für den Arbeitsspeicher, der uns in diesem Fall am wichtigsten ist) weist nur einen Faktor von 1,75 auf (der Knoten mit der höchsten Auslastung für den Arbeitsspeicher ist N3, und N2 hat die geringste Auslastung, also gilt 28/16 = 1,75). Das ist ein guter Ausgleichswert!

Einige zu klärende Punkte

-	Wie wird bestimmt, ob ein Verhältnis von 1,75 gut oder nicht gut ist? Wie können wir sicher sein, dass der Wert gut genug ist und nicht noch mehr Arbeit erforderlich ist?
-	Wann wird ein Lastenausgleich durchgeführt?
-	Was bedeutet es, dass „Memory“ die Gewichtung „Hoch“ erhalten hat?

## Metrikgewichtungen
Anhand von Metrikgewichtungen können zwei unterschiedliche Dienste die gleichen Metriken melden, die Wichtigkeit eines Ausgleichs für diese Metriken aber anders bewerten. Beispiele hierfür sind ein In-Memory-Analysemodul und eine dauerhafte Datenbank. Für beide ist die Metrik „Memory“ sicherlich von Bedeutung, aber für den In-Memory-Dienst ist die Metrik „Disk“ wahrscheinlich weniger wichtig. Unter Umständen wird diese Metrik in geringem Umfang genutzt, aber sie ist für die Leistung des Diensts nicht entscheidend, und daher meldet der Dienst sie möglicherweise nicht einmal. Es ist hilfreich, die gleichen Metriken über unterschiedliche Dienste hinweg nachverfolgen zu können, da der Clusterressourcen-Manager so die tatsächliche Nutzung im Cluster überwachen, die Einhaltung der Kapazitätsobergrenze sicherstellen und vieles mehr tun kann.

Dank Metrikgewichtungen kann der Clusterressourcen-Manager auch Entscheidungen darüber treffen, wie der Ausgleich für den Cluster durchgeführt werden soll, wenn es keine perfekte Lösung gibt (was häufig der Fall ist). Metriken können vier unterschiedliche Gewichtungen haben: Null, Niedrig, Mittel und Hoch. Eine Metrik mit der Gewichtung „Null“ leistet keinen Beitrag, wenn überprüft wird, ob ein guter Ausgleich besteht. Ihre Auslastung wird aber trotzdem berücksichtigt, wenn es beispielsweise um die Kapazitätsmessung geht.

Die eigentlichen Auswirkungen von unterschiedlichen Metrikgewichtungen im Cluster sind, dass wir zu unterschiedlichen Anordnungen der Dienste gelangen, weil der Clusterressourcen-Manager darüber informiert wurde, dass bestimmte Metriken wichtiger sind als andere. Anhand dieser Informationen kann der Clusterressourcen-Manager bei Konflikten bei Metriken mit verschiedenen Gewichtungen Lösungen bevorzugen, die für einen besseren Ausgleich der höher gewichteten Metriken sorgen.

Wir sehen uns nun ein einfaches Beispiel für einige Auslastungsberichte und dafür an, wie unterschiedliche Metrikgewichtungen zu unterschiedlichen Zuordnungen im Cluster führen können. In diesem Beispiel sehen wir, dass die Umstellung der relativen Gewichtung von Metriken dazu führt, dass der Resource Manager bestimmte Lösungen vorzieht, indem unterschiedliche Anordnungen von Diensten erstellt werden.

![Beispiel für Metrikgewichtung und Auswirkung auf Ausgleichslösungen][Image3]

In diesem Beispiel werden vier verschiedene Dienste verwendet, die jeweils unterschiedliche Werte für die beiden Metriken A und B melden. In einem Fall definieren alle Dienste MetricA als wichtigste Metrik (Weight = High) und MetricB als relativ unwichtig (Weight = Low). Wir sehen, dass der Clusterressourcen-Manager die Dienste so anordnet, dass MetricA einen besseren Ausgleich (geringere Standardabweichung) als MetricB aufweist. Im zweiten Fall kehren wir die Metrikgewichtungen um und sehen, dass der Clusterressourcen-Manager ServiceA und ServiceB wahrscheinlich tauschen würde, um eine Zuordnung zu erzielen, bei der MetricB einen besseren Ausgleich als MetricA aufweist.

### Globale Metrikgewichtungen
Wenn ServiceA also MetricA als wichtigste Metrik definiert und diese für ServiceB keine Rolle spielt, welche Gewichtung wird dann letztendlich verwendet?

Für jede Metrik werden eigentlich zwei Gewichtungen nachverfolgt. Die Gewichtung, die vom Dienst selbst definiert wird, und die globale durchschnittliche Gewichtung über alle Dienste hinweg, für die diese Metrik wichtig ist. Wir nutzen beide Gewichtungen beim Berechnen der Punktzahlen von generierten Lösungen. Es ist wichtig sicherzustellen, dass ein Dienst in Bezug auf seine eigenen Prioritäten einen Lastenausgleich erfährt, aber es muss auch dafür gesorgt sein, dass der Cluster insgesamt über eine geeignete Anordnung verfügt.

Was würde passieren, wenn wir uns nicht um den globalen und lokalen Ausgleich kümmern würden? Es ist nicht schwierig, Lösungen zu erstellen, für die ein globaler Ausgleich durchgeführt wird, die für einzelne Dienste aber eine sehr schlechte Leistung und Ressourcenzuordnung ergeben. Im folgenden Beispiel sehen wir uns die Standardmetriken an, mit denen ein zustandsbehafteter Dienst konfiguriert wurde: PrimaryCount, ReplicaCount und Count. Wir können verfolgen, was passiert, wenn wir uns nur um den globalen Ausgleich kümmern:

![Auswirkung einer rein globalen Lösung][Image4]

Im oberen Beispiel, in dem wir uns nur den globalen Ausgleich angesehen haben, ist der Cluster insgesamt gut ausgeglichen. Alle Knoten weisen die gleiche Anzahl von primären Replikaten und Replikaten insgesamt auf. Wenn wir uns aber die tatsächlichen Auswirkungen dieser Zuordnung ansehen, ist das Ergebnis nicht so gut: Der Verlust eines Knotens wirkt sich unverhältnismäßig stark auf eine bestimmte Workload aus, da alle zugehörigen primären Replikate betroffen sind. Nehmen wir beispielsweise an, dass der erste Knoten ausfällt. In diesem Fall verlieren wir gleichzeitig die drei primären Replikate für die drei verschiedenen Partitionen des Diensts „Circle“. Für die anderen beiden Dienste („Triangle“ und „Hexagon“) verlieren die Partitionen ein Replikat, was nicht zu Problemen führt (bis auf die Wiederherstellung des ausgefallenen Replikats).

Im unteren Beispiel haben wir die Replikate sowohl basierend auf dem globalen Ausgleich als auch auf dem Ausgleich pro Dienst verteilt. Beim Berechnen der Punktzahl für die Lösung ordnen wir den größten Teil der Gewichtung der globalen Lösung zu, aber ein (konfigurierbarer) Teil wird für die Sicherstellung verwendet, dass auch die Dienste selbst so weit wie möglich ausgeglichen sind. Wenn hier nun derselbe erste Knoten ausfällt, sehen wir, dass der Verlust der primären (und sekundären) Replikate auf alle Partitionen aller Dienste verteilt ist und die Auswirkungen jeweils gleich sind.

Indem die Metrikgewichtungen berücksichtigt werden, wird der globale Ausgleich basierend auf dem Durchschnitt der Metrikgewichtungen berechnet, die für jeden Dienst konfiguriert wurden. Wir führen den Ausgleich für einen Dienst in Bezug auf seine eigenen definierten Metrikgewichtungen durch.

## Nächste Schritte
- Weitere Informationen zu den anderen Optionen, die für die Konfiguration von Diensten zur Verfügung stehen, finden Sie im Thema zu den anderen verfügbaren Clusterressourcen-Manager-Konfigurationen unter [Konfigurieren von Diensten](service-fabric-cluster-resource-manager-configure-services.md).
- Das Definieren von Defragmentierungsmetriken ist eine Möglichkeit, die Last auf Knoten zu konsolidieren, statt sie auszubreiten. Informationen zum Konfigurieren der Defragmentierung finden Sie in [diesem Artikel](service-fabric-cluster-resource-manager-defragmentation-metrics.md).
- Informationen darüber, wie der Clusterressourcen-Manager die Auslastung im Cluster verwaltet und verteilt, finden Sie im Artikel zum [Lastenausgleich](service-fabric-cluster-resource-manager-balancing.md).
- Starten Sie mit einer [Einführung in den Clusterressourcen-Manager von Service Fabric](service-fabric-cluster-resource-manager-introduction.md).
- Bewegungskosten sind eine Möglichkeit, dem Clusterressourcen-Manager mitzuteilen, dass bestimmte Dienste teurer zu bewegen sind als andere. Weitere Informationen zu Bewegungskosten finden Sie in [diesem Artikel](service-fabric-cluster-resource-manager-movement-cost.md).

[Image1]: ./media/service-fabric-cluster-resource-manager-metrics/cluster-resource-manager-cluster-layout-with-default-metrics.png
[Image2]: ./media/service-fabric-cluster-resource-manager-metrics/Service-Fabric-Resource-Manager-Dynamic-Load-Reports.png
[Image3]: ./media/service-fabric-cluster-resource-manager-metrics/cluster-resource-manager-metric-weights-impact.png
[Image4]: ./media/service-fabric-cluster-resource-manager-metrics/cluster-resource-manager-global-vs-local-balancing.png

<!---HONumber=AcomDC_0824_2016-->