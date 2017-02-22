---
title: Skalierbarkeit von Service Fabric-Diensten | Microsoft Docs
description: Beschreibt, wie Sie die Service Fabric-Dienste skalieren.
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: ed324f23-242f-47b7-af1a-e55c839e7d5d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/30/2016
ms.author: masnider
translationtype: Human Translation
ms.sourcegitcommit: dafaf29b6827a6f1c043af3d6bfe62d480d31ad5
ms.openlocfilehash: 52dcf341a34478bf4e800d8f1b9d44867b5feaf6


---
# <a name="scaling-service-fabric-applications"></a>Skalieren von Service Fabric-Anwendungen
Azure Service Fabric erleichtert das Erstellen skalierbarer Anwendungen durch Verwalten der Dienste, Partitionen und Replikate auf allen Knoten in einem Cluster. Dies ermöglicht eine maximale Ressourcenverwendung.

Hohe Skalierbarkeit für Service Fabric-Anwendungen kann auf zwei Arten erreicht werden:

1. Skalieren auf Dienstpartitionsebene
2. Skalierung auf der Ebene benannter Dienstinstanzen

## <a name="scaling-at-the-partition-level"></a>Skalieren auf Partitionsebene
Service Fabric unterstützt die Partitionierung. Durch die Partitionierung kann ein einzelner Dienst auf mehrere unabhängige Partitionen aufgeteilt werden, jeweils mit einem Teil des Gesamtstatus des Diensts. Die [Übersicht über die Partitionierung](service-fabric-concepts-partitioning.md) enthält Informationen zu den Typen von Partitionierungsschemas, die unterstützt werden. Die Replikate der einzelnen Partitionen sind auf den Knoten im Cluster verteilt. Angenommen, ein Dienst verwendet das Bereichspartitionierungsschema mit einem niedrigen Schlüssel 0, einem hohen Schlüssel 99 und 4 Partitionen. In einem Drei-Knoten-Cluster kann der Dienst mit vier Replikaten, welche die Ressourcen auf den einzelnen Knoten gemeinsam nutzen, wie in der folgenden Abbildung gezeigt ausgelegt werden.

<center>
![Partitionslayout mit drei Knoten](./media/service-fabric-concepts-scalability/layout-three-nodes.png)
</center>

Wenn Sie die Anzahl der Knoten erhöhen, nutzt Service Fabric die Ressourcen auf den neuen Knoten, indem einige der vorhandenen Replikate dorthin verschoben werden. Wenn die Anzahl der Knoten auf vier erhöht wird, werden auf jedem Knoten (die jeweils verschiedenen Partitionen angehören) jetzt&3; Replikate ausgeführt, wodurch Ressourcennutzung und Leistung verbessert werden.

<center>
![Partitionslayout mit vier Knoten](./media/service-fabric-concepts-scalability/layout-four-nodes.png)
</center>

## <a name="scaling-at-the-service-name-level"></a>Skalierung auf Dienstnamensebene
Eine Dienstinstanz ist eine bestimmte Instanz eines Anwendungsnamens und eines Diensttypnamens (siehe [Service Fabric-Anwendungslebenszyklus](service-fabric-application-lifecycle.md)). Während der Erstellung eines Dienstes geben Sie das zu verwendende Partitionierungsschema an ( [Partitionieren von Service Fabric-Diensten](service-fabric-concepts-partitioning.md)).

Die erste Ebene der Skalierung erfolgt nach Dienstnamen. Sie können Instanzen eines Diensts optional mit unterschiedlichen Partitionierungsebenen erstellen, wenn die älteren Instanzen ausgelastet sind. So können neue Dienstconsumer die Instanzen mit geringerer Auslastung anstelle der ausgelasteten Instanzen verwenden.

Eine Option zum Erhöhen der Kapazität besteht darin, eine neue Dienstinstanz mit einem neuen Partitionsschema zu erstellen. Dadurch wird jedoch die Komplexität gesteigert. Consumerclients müssen wissen, wann und wie sie einen Dienst mit einem anderen Namen verwenden sollen. Als weitere Alternative müsste ein Verwaltungs- oder Zwischendienst eine Entscheidung treffen, welcher Dienst und welche Partition die einzelnen Anforderungen verarbeiten sollen.

### <a name="example-scenario-embedded-dates"></a>Beispielszenario: Eingebettete Daten
Ein mögliches Szenario ist die Verwendung von Datumsinformationen im Dienstnamen. Sie können beispielsweise eine Dienstinstanz mit einem bestimmten Namen für Kunden verwenden, die in 2013 beigetreten sind, und eine Dienstinstanz mit einem anderen Namen für Kunden, die in 2014 beigetreten sind. Dieses Benennungsschema ermöglicht eine programmgesteuerte Erhöhung der Namen in Abhängigkeit des Datums (die Dienstinstanz für 2014 kann nach Bedarf erstellt werden, bevor 2014 beginnt).

Dieser Ansatz basiert jedoch auf Clients, die anwendungsspezifische Namensinformationen verwenden, die nicht zum Kenntnisbereich von Service Fabric gehören.

* *Mit einer Namenskonvention*: Erstellen Sie bei Bereitstellung Ihrer Anwendung in 2013 einen Dienst mit dem Namen „fabric:/app/service2013“. Erstellen Sie im zweiten Quartal 2013 einen weiteren Dienst mit dem Namen „fabric:/app/service2014“. Beide Dienste sind vom gleichen Diensttyp. Bei diesem Ansatz muss der Client Logik zum Erstellen des entsprechenden Dienstnamens basierend auf dem Jahr einsetzen.
* *Mit einem Suchdienst*: Ein anderer Ansatz ist die Bereitstellung eines sekundären „Suchdiensts“, der den Dienstnamen für einen gewünschten Schlüssel bereitstellen kann. Die neuen Dienstinstanzen können dann vom Suchdienst erstellt werden. Der Suchdienst selbst behält keine Anwendungsdaten bei, sondern nur Daten zu den Namen der Dienste, die der Suchdienst erstellt. Daher kontaktiert der Client im obigen Beispiel zuerst den Suchdienst, um den Namen des Diensts zu erhalten, der die Daten für ein bestimmtes Jahr verarbeitet. Anschließend verwendet der Client diesen Dienstnamen, um den eigentlichen Vorgang auszuführen. Das Ergebnis der ersten Suche kann zwischengespeichert werden.

## <a name="putting-it-all-together"></a>Gesamtbild
Sehen wir uns alle Ideen, die wir hier besprochenen haben, in einem anderen Szenario an.

Betrachten Sie das folgende Beispiel: Sie möchten einen Dienst erstellen, der als Adressbuch fungiert und Namen und Kontaktinformationen speichert. Wie viele Benutzer werden ihn verwenden? Wie viele Kontakte werden von den einzelnen Benutzern gespeichert? All diese Informationen gleich bei der ersten Erstellung des Diensts herauszufinden, ist tatsächlich schwierig. Wenn Sie die falsche Partitionsanzahl wählen, könnte dies später zu Skalierungsproblemen führen. Aber warum sollte man überhaupt versuchen, für alle Benutzer ein einzelnes Partitionsschema auszuwählen?

Erwägen Sie in diesen Situationen stattdessen das folgende Muster:
1. Statt zu versuchen, im Vorfeld für jeden ein Partitionierungsschema auswählen, erstellen Sie einen „Manager-Dienst“.
2. Der Auftrag des Manager-Diensts besteht darin, die Informationen von Kunden zu prüfen, wenn diese sich für Ihren Dienst registrieren. Abhängig von diesen Informationen wird eine Instanz Ihres _eigentlichen_ Kontaktspeicherdiensts _nur für diesen Kunden_ erstellt. Diese Art des Musters zur dynamischen Diensterstellung bringt viele Vorteile mit sich:

    * Sie müssen nicht die richtige Partitionsanzahl für alle Benutzer vorab erraten.
    * Es findet Datensegmentierung statt, weil jeder Kunde eine eigene Kopie des Diensts besitzt.
    * Die Dienste bei den einzelnen Kunden können nach Bedarf entsprechend der erwarteten Skalierung anders konfiguriert werden, mit mehr oder weniger Partitionen oder Replikaten.
      * Nehmen Sie beispielsweise an, der Kunde hätte für den Gold-Tarif bezahlt – in diesem Fall könnte er mehr Replikate oder eine größere Anzahl von Partitionen erhalten.
      * Alternativ hat der Kunde vielleicht Informationen angegeben, aus denen hervorgeht, dass nur eine geringe Anzahl von Kontakten benötigt wird. In diesem Fall würden ihm nur wenige Partitionen zugeteilt.
    * Sie führen nicht schon eine Reihe von Dienstinstanzen oder Replikaten aus, während Sie auf Kunden warten.
    * Wenn ein Kunde Sie verlässt, können dessen Informationen aus Ihrem Dienst ganz einfach von dem Manager entfernt werden, der den Dienst erstellt hat.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu den Service Fabric-Konzepten finden Sie in den folgenden Artikeln:

* [Verfügbarkeit der Service Fabric-Dienste](service-fabric-availability-services.md)
* [Partitionieren von Service Fabric-Diensten](service-fabric-concepts-partitioning.md)
* [Definieren und Verwalten von Zuständen](service-fabric-concepts-state.md)



<!--HONumber=Jan17_HO1-->


