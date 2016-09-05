<properties
   pageTitle="Der Clusterressourcen-Manager von Service Fabric – Bewegungskosten | Microsoft Azure"
   description="Übersicht über die Bewegungskosten für Service Fabric-Dienste"
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

# Dienstbewegungskosten zum Beeinflussen der Ressourcen-Manager-Optionen
Ein weiterer wichtiger Faktor bei den Überlegungen zu Veränderungen an einem Cluster, und der Bewertung einer bestimmten Lösung sind die Kosten, die mit der Umsetzung dieser Lösung verbunden sind.

Dienstinstanzen oder Replikate zu verschieben kostet zumindest CPU-Zeit und Netzwerkbandbreite. Für zustandsbehaftete Dienste wird darüber hinaus Platz auf dem Laufwerk benötigt, um den Zustand vor dem Herunterfahren alter Replikate zu kopieren. Sie legen sicherlich Wert darauf, die Kosten einer Lösung zu minimieren, die der Clusterressourcen-Manager bereithält. Gleichzeitig möchten Sie aber natürlich Lösungen nutzen, die die Zuordnung von Ressourcen im Cluster erheblich verbessern.

Der Clusterressourcen-Manager bietet zwei Möglichkeiten, die Kosten zu berechnen und zu beschränken und gleichzeitig den Cluster im Hinblick auf seine anderen Ziele möglichst optimal zu verwalten. Wenn der Clusterressourcen-Manager ein neues Layout für den Cluster plant, berechnet er jede einzelne Verschiebung, die bei diesem Vorgang erforderlich ist. Wenn Sie zwei Lösungen mit etwa dem gleichen Endergebnis zur Auswahl haben, wählen Sie diejenige mit den geringsten Kosten (Gesamtzahl der Datenverschiebungen).

Das funktioniert recht gut, bis dasselbe Problem wie bei den Standard- oder statischen Lasten auftritt: In einem komplexen System sind Verschiebevorgänge nicht alle gleich; einige sind mit sehr viel höheren Kosten verbunden als andere.

## Ändern der Bewegungskosten eines Replikats und zu berücksichtigende Faktoren
Genau wie bei der Berichterstattung von Lasten (ein weiteres Feature des Clusterressourcen-Managers) kann der Dienst selbst ermitteln, wie teuer es jeweils ist, den Dienst zu einem beliebigen Zeitpunkt zu verschieben.

Code

```csharp
this.ServicePartition.ReportMoveCost(MoveCost.Medium);
```

MoveCost hat vier Stufen: Zero, Low, Medium, und High. Diese stehen in einem Verhältnis zueinander, mit Ausnahme von Zero. Zero bedeutet, dass das Verschieben eines Replikats keine Kosten generiert und die Bewertung der Lösung nicht negativ beeinflussen sollte. Wenn Sie die Kosten für die Verschiebung als „High“ einstufen, garantiert dies *nicht*, dass das Replikat nicht verschoben wird, sondern nur, dass die Verschiebung nicht ohne triftigen Grund erfolgt.

![MoveCost als Faktor bei der Auswahl der zu verschiebenden Replikate][Image1]

MoveCost hilft dabei, Lösungen zu finden, die insgesamt die geringsten Unterbrechungen verursachen, am leichtesten umzusetzen sind und dabei das beste Gleichgewicht versprechen. Die Kosten eines Dienstes können von Vielem abhängen, doch die gängigsten Faktoren in der Berechnung Ihrer Verschiebekosten sind:

1.	der Menge von Zuständen oder Daten, die ein Dienst verschieben soll
2.	Die Kosten für die Trennung von Clients (die Kosten für das Verschieben eines primären Replikats sind üblicherweise höher als die Kosten für das Verschieben eines sekundären Replikats).
3.	Die Kosten für die Unterbrechung eines Vorgangs, der gerade ausgeführt wird (einige Vorgänge auf Datenspeicherebene oder Vorgänge, die eine Antwort auf einen Clientaufruf darstellen, sind kostenaufwendig und sollten ab einem bestimmten Punkt nicht mehr abgebrochen werden, wenn es nicht unbedingt erforderlich ist). Für die Dauer der Operation stufen Sie die Kosten hoch, um die Wahrscheinlichkeit, dass der Dienst das Replikat oder die Instanz verschiebt, zu minimieren. Wenn die Operation abgeschlossen ist, können Sie sie auf normal zurückstufen.

## Nächste Schritte
- Metriken bestimmen, wie der Clusterressourcen-Manager von Service Fabric den Ressourcenverbrauch und die Kapazität im Cluster verwaltet. Weitere Informationen zu Metriken und deren Konfiguration finden Sie in [diesem Artikel](service-fabric-cluster-resource-manager-metrics.md).
- Informationen darüber, wie der Clusterressourcen-Manager die Auslastung im Cluster verwaltet und verteilt, finden Sie im Artikel zum [Lastenausgleich](service-fabric-cluster-resource-manager-balancing.md).

[Image1]: ./media/service-fabric-cluster-resource-manager-movement-cost/service-most-cost-example.png

<!---HONumber=AcomDC_0824_2016-->