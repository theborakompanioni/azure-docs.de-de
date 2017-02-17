---
title: "Der Clusterressourcen-Manager von Service Fabric – Verschiebungskosten | Microsoft Docs"
description: "Übersicht über die Verschiebungskosten für Service Fabric-Dienste"
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: f022f258-7bc0-4db4-aa85-8c6c8344da32
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/05/2017
ms.author: masnider
translationtype: Human Translation
ms.sourcegitcommit: dafaf29b6827a6f1c043af3d6bfe62d480d31ad5
ms.openlocfilehash: f85365a36aea39b4179805e728c7ddafa140f08b


---
# <a name="service-movement-cost-for-influencing-cluster-resource-manager-choices"></a>Kosten von Dienstverschiebungen zum Beeinflussen der Optionen im Clusterressourcen-Manager
Ein wichtiger Faktor bei den Überlegungen im Clusterressourcen-Manager für Service Fabric zu Veränderungen an einem Cluster sind die Gesamtkosten, die mit der Umsetzung dieser Lösung verbunden sind. Die „Kosten“ werden dabei gegen den Lastenausgleich abgewogen, der erreicht werden kann.

Das Verschieben von Dienstinstanzen oder Replikaten kostet zumindest CPU-Zeit und Netzwerkbandbreite. Für zustandsbehaftete Dienste kostet es auch den Umfang an Speicherplatz auf dem Datenträger und im Arbeitsspeicher, den Sie benötigen, um eine Kopie des Zustands vor dem Herunterfahren alter Replikate zu erstellen. Natürlich möchten Sie die Kosten einer Lösung minimieren, die vom Clusterressourcen-Manager in Azure Service Fabric erstellt wird. Aber Sie möchten auch alle Lösungen kennen, die die Zuordnung von Ressourcen im Cluster erheblich verbessern würden.

Der Clusterressourcen-Manager bietet zwei Möglichkeiten, die Kosten zu berechnen und zu beschränken und gleichzeitig den Cluster im Hinblick auf seine anderen Ziele möglichst optimal zu verwalten. Wenn der Clusterressourcen-Manager ein neues Layout für den Cluster plant, zählt er jede einzelne Verschiebung, die bei diesem Vorgang erforderlich ist. Wenn zwei Lösungen mit nahezu identischem Lastenausgleich (Bewertung) generiert werden, sollten Sie sich für den mit den geringsten Kosten entscheiden (Gesamtzahl der Verschiebungen).

Diese Strategie funktioniert sehr gut. Aber wie bei standardmäßigen oder statischen Lasten ist es in einem komplexen System unwahrscheinlich, dass alle Verschiebungen gleich sind. Einige sind wahrscheinlich viel teurer.

## <a name="changing-a-replicas-move-cost-and-factors-to-consider"></a>Ändern der Bewegungskosten eines Replikats und zu berücksichtigende Faktoren
Wie beim Lastenausgleich für die Berichterstellung (ein weiteres Feature des Clusterressourcen-Managers) können auch Dienste dynamisch und selbstständig Berichte dazu erstellen, wie hoch die Kosten für das Verschieben zu einem beliebigen Zeitpunkt sind.

Code:

```csharp
this.ServicePartition.ReportMoveCost(MoveCost.Medium);
```

Eine Standardverschiebung kann auch angegeben werden, wenn ein Dienst erstellt wird.

MoveCost hat vier Stufen: Zero, Low, Medium und High. Diese MoveCosts stehen zueinander in einem Verhältnis, mit Ausnahme von Zero. Zero bedeutet, dass das Verschieben keine Kosten generiert und die Bewertung der Lösung nicht negativ beeinflussen sollte. Das Festlegen der Verschiebung auf High stellt *keine* Garantie dafür dar, dass das Replikat an einem Ort verbleibt.

<center>
![Verschiebungskosten als Faktor bei der Auswahl der zu verschiebenden Replikate][Image1]
</center>

MoveCost hilft Ihnen dabei, Lösungen zu finden, die insgesamt die geringsten Unterbrechungen verursachen, am leichtesten umzusetzen sind und dabei das beste Gleichgewicht versprechen. Die Kosten für einen Dienst können von Vielem abhängen. Die gängigsten Faktoren bei der Berechnung der Kosten von Verschiebungen sind:

* Die Menge von Zuständen oder Daten, die ein Dienst verschieben soll.
* Die Kosten der Trennung von Clients. Die Kosten für das Verschieben eines primären Replikats sind normalerweise höher als die für ein sekundäres Replikat.
* Die Kosten für Unterbrechungen einer sich in der Ausführung befindenden Operation. Einige Operationen auf Datenspeicherebene oder Operationen, die als Antwort auf einen Clientaufruf ausgeführt werden, sind kostenaufwendig. Ab einem bestimmten Punkt werden sie nicht mehr unnötigerweise freiwillig abgebrochen. Sie erhöhen daher während des Vorgangs die Kosten für das Verschieben dieses Dienstobjekts, um die Wahrscheinlichkeit zu verringern, dass es verschoben wird. Wenn der Vorgang abgeschlossen ist, können Sie die Kosten wieder auf den normalen Wert zurückstufen.

## <a name="next-steps"></a>Nächste Schritte
* Der Clusterressourcen-Manager von Service Fabric verwendet Metriken, um den Ressourcenverbrauch und die Kapazität im Cluster zu verwalten. Weitere Informationen zu Metriken und deren Konfiguration finden Sie unter [Verwalten von Ressourcenverbrauch und Auslastung in Service Fabric mit Metriken](service-fabric-cluster-resource-manager-metrics.md).
* Informationen darüber, wie der Clusterressourcen-Manager die Auslastung im Cluster verwaltet und verteilt, finden Sie unter [Lastenausgleich für Service Fabric-Cluster](service-fabric-cluster-resource-manager-balancing.md).

[Image1]:./media/service-fabric-cluster-resource-manager-movement-cost/service-most-cost-example.png



<!--HONumber=Feb17_HO3-->


