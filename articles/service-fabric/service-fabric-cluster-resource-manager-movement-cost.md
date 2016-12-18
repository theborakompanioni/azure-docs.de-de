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
ms.date: 08/19/2016
ms.author: masnider
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 54cdfee64360543e0d1011f04c9b79400e912582


---
# <a name="service-movement-cost-for-influencing-cluster-resource-manager-choices"></a>Kosten von Dienstverschiebungen zum Beeinflussen der Optionen im Clusterressourcen-Manager
Ein wichtiger Faktor bei den Überlegungen zu Veränderungen an einem Cluster und der Bewertung einer Lösung sind die Gesamtkosten, die mit der Umsetzung dieser Lösung verbunden sind.

Das Verschieben von Dienstinstanzen oder Replikaten kostet zumindest CPU-Zeit und Netzwerkbandbreite. Für zustandsbehaftete Dienste kostet es auch den Umfang an Speicherplatz auf dem Datenträger, den Sie benötigen, um eine Kopie des Zustands vor dem Herunterfahren alter Replikate zu erstellen. Natürlich möchten Sie die Kosten einer Lösung minimieren, die vom Clusterressourcen-Manager in Azure Service Fabric erstellt wird. Aber Sie möchten auch alle Lösungen kennen, die die Zuordnung von Ressourcen im Cluster erheblich verbessern würden.

Der Clusterressourcen-Manager bietet zwei Möglichkeiten, die Kosten zu berechnen und zu beschränken und gleichzeitig den Cluster im Hinblick auf seine anderen Ziele möglichst optimal zu verwalten. Wenn der Clusterressourcen-Manager ein neues Layout für den Cluster plant, zählt er jede einzelne Verschiebung, die bei diesem Vorgang erforderlich ist. Wenn Sie zwei Lösungen mit etwa dem gleichen Endergebnis (Bewertung) zur Auswahl haben, wählen Sie diejenige mit den geringsten Kosten (Gesamtzahl der Verschiebungen).

Dies funktioniert recht gut. Aber wie bei standardmäßigen oder statischen Lasten ist es in einem komplexen System unwahrscheinlich, dass alle Verschiebungen gleich sind. Einige sind wahrscheinlich viel teurer.

## <a name="changing-a-replicas-move-cost-and-factors-to-consider"></a>Ändern der Bewegungskosten eines Replikats und zu berücksichtigende Faktoren
Genau wie beim Melden von Lasten (ein weiteres Feature des Clusterressourcen-Managers) können Sie dem Dienst ermöglichen, selbst zu melden, wie teuer es jeweils ist, den Dienst zu einem beliebigen Zeitpunkt zu verschieben.

Code:

```csharp
this.ServicePartition.ReportMoveCost(MoveCost.Medium);
```

MoveCost hat vier Stufen: Zero, Low, Medium und High. Diese stehen zueinander in einem Verhältnis, mit Ausnahme von Zero. Zero bedeutet, dass das Verschieben eines Replikats keine Kosten generiert und die Bewertung der Lösung nicht negativ beeinflussen sollte. Wenn Sie die Kosten für die Verschiebung als „High“ einstufen, garantiert dies *nicht* , dass das Replikat nicht verschoben wird, sondern nur, dass die Verschiebung nicht ohne triftigen Grund erfolgt.

![Verschiebungskosten als Faktor bei der Auswahl der zu verschiebenden Replikate][Image1]

MoveCost hilft Ihnen dabei, Lösungen zu finden, die insgesamt die geringsten Unterbrechungen verursachen, am leichtesten umzusetzen sind und dabei das beste Gleichgewicht versprechen. Die Kosten für einen Dienst können von Vielem abhängen. Die gängigsten Faktoren bei der Berechnung der Kosten von Verschiebungen sind:

* Die Menge von Zuständen oder Daten, die ein Dienst verschieben soll.
* Die Kosten der Trennung von Clients. Die Kosten für das Verschieben eines primären Replikats sind normalerweise höher als die für ein sekundäres Replikat.
* Die Kosten für Unterbrechungen einer sich in der Ausführung befindenden Operation. Einige Operationen auf Datenspeicherebene oder Operationen, die als Antwort auf einen Clientaufruf ausgeführt werden, sind kostenaufwendig. Ab einem bestimmten Punkt werden sie nicht mehr unnötigerweise freiwillig abgebrochen. Für die Dauer der Operation stufen Sie die Kosten hoch, um die Wahrscheinlichkeit zu minimieren, dass das Replikat oder die Instanz des Diensts verschoben wird. Wenn die Operation abgeschlossen ist, können Sie sie auf normal zurückstufen.

## <a name="next-steps"></a>Nächste Schritte
* Der Clusterressourcen-Manager von Service Fabric verwendet Metriken, um den Ressourcenverbrauch und die Kapazität im Cluster zu verwalten. Weitere Informationen zu Metriken und deren Konfiguration finden Sie unter [Verwalten von Ressourcenverbrauch und Auslastung in Service Fabric mit Metriken](service-fabric-cluster-resource-manager-metrics.md).
* Informationen darüber, wie der Clusterressourcen-Manager die Auslastung im Cluster verwaltet und verteilt, finden Sie unter [Lastenausgleich für Service Fabric-Cluster](service-fabric-cluster-resource-manager-balancing.md).

[Bild1]:./media/service-fabric-cluster-resource-manager-movement-cost/service-most-cost-example.png



<!--HONumber=Nov16_HO3-->


