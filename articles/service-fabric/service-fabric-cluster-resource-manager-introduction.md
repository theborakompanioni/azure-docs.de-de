---
title: "Einführung in den Clusterressourcen-Manager von Service Fabric | Microsoft Docs"
description: "Eine Einführung in den Clusterressourcen-Manager von Service Fabric."
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: cfab735b-923d-4246-a2a8-220d4f4e0c64
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/05/2017
ms.author: masnider
translationtype: Human Translation
ms.sourcegitcommit: dafaf29b6827a6f1c043af3d6bfe62d480d31ad5
ms.openlocfilehash: ec470466f006265af5c4ccfddeeba975e6e648b5


---
# <a name="introducing-the-service-fabric-cluster-resource-manager"></a>Einführung in den Clusterressourcen-Manager von Service Fabric
Zur Verwaltung von IT-Systemen oder einer Gruppe von Diensten wurden bisher üblicherweise physische oder virtuelle Computer beschafft und für diese Dienste oder Systeme eingerichtet. Viele wichtige Dienste wurden in eine „Web“- und eine „Daten“- oder „Speicher“-Schicht unterteilt, ggf. mit anderen speziellen Komponenten wie einem Cache. Andere Anwendungstypen wiesen eine Nachrichtenschicht auf, in der Anforderungen ein- und ausgingen. Diese Schicht stellte eine Verbindung mit einer Arbeitsschicht her, um Analysen oder Transformationen durchzuführen, die im Rahmen des Nachrichtenversands erforderlich waren. Für jede Art von Workload gab es bestimmte fest zugeordnete Computer oder Computergruppen, so z.B. für die Datenbank und die Webserver. Wenn eine bestimmte Art von Workload eine Überlastung der Computer verursachte, auf denen sie ausgeführt wurde, hat man weitere Computer mit derselben Konfiguration hinzugefügt. Meistens wurden jedoch einige Computer durch größere Computer ersetzt. Ganz einfach. Wenn ein Computer ausfiel, wurde dieser Teil der gesamten Anwendung mit niedriger Kapazität ausgeführt, bis der Computer wiederhergestellt werden konnte. Noch immer recht einfach (wenn auch nicht unbedingt spaßig).

Lassen Sie uns nun annehmen, dass es einen Bedarf an einer horizontalen Hochskalierung gibt und Sie sich für den Ansatz mit Containern und/oder Microservices entschieden haben. Plötzlich besitzen Sie Dutzende, Hunderte oder Tausende von Computern. Sie verwenden Dutzende verschiedener Arten von Diensten (von denen keiner die vollständigen Ressourcen eines Computers beansprucht) und möglicherweise Hunderte verschiedener Instanzen dieser Dienste. Für hohe Verfügbarkeit weist jede benannte Instanz eine oder mehrere Instanzen oder Replikate auf.

Dann ist die Verwaltung Ihrer Umgebung nicht mehr so einfach wie das Verwalten einiger Computer, die einzelnen Typen von Workloads fest zugeordnet sind. Ihre Server sind virtuell und haben keine Namen mehr (denn Ihr Modell hat sich *tatsächlich* von einem [Haustierbestand in eine Rinderherde](http://www.slideshare.net/randybias/architectures-for-open-and-scalable-clouds/20) geändert). Die Konfiguration dreht sich weniger um die Computer und mehr um die Dienste selbst. Dedizierte Hardware gehört der Vergangenheit an. Dienste sind selbst zu kleinen verteilten Systemen geworden, die mehrere kleinere Standardhardwarekomponenten überspannen.

Als Folge dieser Aufteilung Ihrer zuvor monolithischen, geschichteten App in verschiedene Dienste, die auf Standardhardware ausgeführt werden, müssen Sie nun mit wesentlich mehr Kombinationen zurechtkommen. Wer entscheidet, welche Arten und wie viele Workloads auf welcher Hardware ausgeführt werden können? Welche Workloads können problemlos auf der gleichen Hardware ausgeführt werden, bei welchen treten Konflikte auf? Wenn ein Computer ausfällt, … was wurde überhaupt darauf ausgeführt? Wer ist dafür verantwortlich, dass die Workload wieder den Betrieb aufnimmt? Warten Sie, bis der (virtuelle?) Computer wieder aktiv wird, oder führen Ihre Workloads automatisch ein Failover auf andere Computer durch und werden weiter ausgeführt? Ist Benutzereingriff erforderlich? Was ist mit Upgrades in dieser Umgebung?

Als Entwickler und Operatoren, die sich hiermit beschäftigen, werden wir bei der Verwaltung dieser Komplexität Hilfe benötigen. Ihnen wird klar, dass eine Einstellungswelle und der Versuch, die Komplexität mit Personal in den Griff zu bekommen, nicht die richtige Antwort ist.

Vorgehensweise

## <a name="introducing-orchestrators"></a>Einführung in Orchestratoren
„Orchestrator“ ist der allgemeine Begriff für eine Softwarekomponente, mit deren Hilfe Administratoren diese Arten von Umgebungen verwalten können. Orchestratoren sind die Komponenten, die Anforderungen wie „Ich möchte&5; Kopien dieses Diensts in meiner Umgebung ausführen“ annehmen. Sie versuchen, die Umgebung unter allen Umständen in den gewünschten Zustand zu versetzen.

Orchestratoren (nicht Menschen) kommen zum Einsatz, wenn ein Computer ausfällt oder eine Workload aus einem unerwarteten Grund beendet wird. Die meisten Orchestratoren beschäftigen sich nicht nur mit Fehlern. Weitere Features umfassen das Verwalten neuer Bereitstellungen, das Durchführen von Upgrades sowie den Umgang mit Ressourcenverbrauch. Alle Orchestratoren sind im Grunde darauf ausgelegt, einen gewünschten Konfigurationszustand in der Umgebung zu halten. Sie möchten einem Orchestrator angeben können, was Sie wünschen, und ihm die Hauptarbeit überlassen. Chronos oder Marathon auf Basis von Mesos, Fleet, Docker Datacenter/Docker Swarm, Kubernetes und Service Fabric sind alles Beispiele für Orchestratoren (oder verfügen über integrierte Orchestratoren). Weitere werden ständig entwickelt, da die Komplexität der Verwaltung realer Bereitstellungen in verschiedenen Arten von Umgebungen und unter unterschiedlichen Bedingungen zunimmt und sich ändert.

## <a name="orchestration-as-a-service"></a>Orchestrierung als Dienst
Die Aufgabe des Orchestrators in einem Service Fabric-Cluster übernimmt hauptsächlich der Clusterressourcen-Manager. Der Clusterressourcen-Manager von Service Fabric ist einer der Systemdienste in Service Fabric, der in jedem Cluster automatisch gestartet wird. Im Allgemeinen kümmert sich der Clusterressourcen-Manager um drei Aufgaben:

1. Erzwingen von Regeln
2. Optimieren der Umgebung
3. Helfen bei anderen Prozessen

Sehen Sie sich das folgende Microsoft Virtual Academy-Video zur Funktionsweise der Clusterressourcen-Managers an: <center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=d4tka66yC_5706218965">
<img src="./media/service-fabric-cluster-resource-manager-introduction/ConceptsAndDemoVid.png" WIDTH="360" HEIGHT="244">
</a></center>

### <a name="what-it-isnt"></a>Was der Clusterressourcen-Manager nicht ist
In herkömmlichen N-Schichten-Anwendungen wurde immer das Konzept des Lastenausgleichs verfolgt. Hierbei handelte es sich normalerweise um einen Netzwerklastenausgleich (Network Load Balancer, NLB) oder einen Anwendungslastenausgleich (ALB), je nachdem, an welcher Stelle im Netzwerkstapel er stattfand. Einige Load Balancer sind hardwarebasiert, wie z.B. das BigIP-Angebot von F5, andere sind softwarebasiert, wie z.B. der Netzwerklastenausgleich von Microsoft. In anderen Umgebungen wird der Lastenausgleich möglicherweise von HAProxy oder nginx ausgeführt. In diesen Architekturen besteht die Aufgabe des Lastenausgleichs darin sicherzustellen, dass zustandslose Workloads (ungefähr) die gleiche Menge an Arbeit erhalten. Für den Lastenausgleich gab es verschiedene Strategien. Einige sendeten jeden Aufruf an einen anderen Server. Andere stellten feste/unveränderliche Sitzungen bereit. Fortgeschrittenere Load Balancer verwenden tatsächliche Schätzungen oder Berichte, um einen Aufruf basierend auf den erwarteten Kosten und der aktuellen Computerauslastung weiterzuleiten.

Netzwerklastenausgleichsmodule oder Nachrichtenrouter versuchten sicherzustellen, dass die Web-/Workerschicht einigermaßen ausgeglichen blieb. Strategien für einen Lastenausgleich auf der Datenschicht waren unterschiedlich und hingen vom Datenspeichermechanismus ab, wobei der Fokus meist auf dem Sharding und Zwischenspeichern von Daten, verwalteten Sichten, gespeicherten Prozeduren und anderen speicherspezifischen Mechanismen lag.

Während einige dieser Strategien interessant sind, ist der Clusterressourcen-Manager von Service Fabric mit einem NLB oder Cache nicht vergleichbar. Bei einem Netzwerklastenausgleich wird ein Ausgleich der Front-Ends sichergestellt, indem der Datenverkehr an die Stellen verschoben wird, an denen die Dienste ausgeführt werden. Der Clusterressourcen-Manager von Service Fabric verfolgt eine andere Strategie. Im Grunde verschiebt Service Fabric *Dienste* dorthin, wo sie am sinnvollsten erscheinen, und erwartet, dass der Datenverkehr oder die Auslastung ihnen folgt. Beispielsweise werden Dienste auf Knoten verschoben, die zurzeit unausgelastet sind, weil die dort befindlichen Dienste nicht sehr beansprucht werden. Die Knoten werden möglicherweise deshalb nicht beansprucht, weil die dort befindlichen Dienste gelöscht oder an eine andere Stelle verschoben wurden. Als weiteres Beispiel kann der Clusterressourcen-Manager auch einen Dienst von einem Computer an eine andere Stelle verschieben. Möglicherweise soll auf dem Computer ein Upgrade stattfinden, oder der Computer ist aufgrund einer intensiven Nutzung durch die darauf ausgeführten Dienste überlastet.

Da der Clusterressourcen-Manager für das Verschieben von Diensten zuständig ist (nicht dafür, den Netzwerkdatenverkehr dort bereitzustellen, wo sich die Dienste bereits befinden), umfasst er einen anderen Funktionssatz als ein Netzwerklastenausgleichsmodul. Wenn wir ihn uns im Detail betrachten, stellen wir fest, dass er grundlegend andere Strategien verfolgt, um sicherzustellen, dass die Hardwareressourcen im Cluster effizient genutzt werden.

## <a name="next-steps"></a>Nächste Schritte
* Informationen über die Architektur und den Informationsfluss innerhalb des Clusterressourcen-Managers finden Sie in [diesem Artikel](service-fabric-cluster-resource-manager-architecture.md).
* Der Clusterressourcen-Manager bietet zahlreiche Optionen zum Beschreiben des Clusters. Weitere Informationen hierzu finden Sie in diesem Artikel zum [Beschreiben eines Service Fabric-Clusters](service-fabric-cluster-resource-manager-cluster-description.md).
* Weitere Informationen zu den anderen Optionen, die für die Konfiguration von Diensten zur Verfügung stehen, finden Sie im Thema zu den anderen verfügbaren Clusterressourcen-Manager-Konfigurationen unter [Konfigurieren von Diensten](service-fabric-cluster-resource-manager-configure-services.md).
* Metriken bestimmen, wie der Clusterressourcen-Manager von Service Fabric den Ressourcenverbrauch und die Kapazität im Cluster verwaltet. Weitere Informationen zu Metriken und deren Konfiguration finden Sie in [diesem Artikel](service-fabric-cluster-resource-manager-metrics.md)
* Der Clusterressourcen-Manager arbeitet mit den Verwaltungsfunktionen von Service Fabric. Weitere Informationen hierzu finden Sie in [diesem Artikel](service-fabric-cluster-resource-manager-management-integration.md)
* Informationen darüber, wie der Clusterressourcen-Manager die Auslastung im Cluster verwaltet und verteilt, finden Sie im Artikel zum [Lastenausgleich](service-fabric-cluster-resource-manager-balancing.md)



<!--HONumber=Jan17_HO1-->


