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
ms.date: 08/19/2016
ms.author: masnider
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: cbc87779f2248e3429e3b1bc9d311c5d9175dddf


---
# <a name="introducing-the-service-fabric-cluster-resource-manager"></a>Einführung in den Clusterressourcen-Manager von Service Fabric
Zur Verwaltung von IT-Systemen oder einer Gruppe von Diensten wurden bisher üblicherweise physische oder virtuelle Computer beschafft und für diese Dienste oder Systeme eingerichtet. Viele wichtige Dienste wurden in eine „Web“- und eine „Daten“- oder „Speicher“-Schicht unterteilt, ggf. mit anderen speziellen Komponenten wie einem Cache. Andere Anwendungstypen verfügten über eine Messagingschicht, auf der Anforderungen ein- und ausgehen, und waren mit einer Verarbeitungsschicht verbunden, auf der die für das Messaging erforderlichen Analysen und Transformationen erfolgen. Für jede Art von Workload gab es bestimmte fest zugeordnete Computer oder Computergruppen, so z.B. für die Datenbank und die Webserver. Wenn eine bestimmte Workload eine Überlastung der entsprechenden Computer verursachte, wurden weitere für die Ausführung dieser Workloads konfigurierte Computer hinzugefügt oder einige der vorhandenen Computer durch größere Computer ersetzt. Ganz einfach. Wenn ein Computer ausfiel, wurde dieser Teil der gesamten Anwendung mit niedriger Kapazität ausgeführt, bis der Computer wiederhergestellt werden konnte. Noch immer recht einfach (wenn auch nicht unbedingt spaßig).

Lassen Sie uns nun annehmen, dass es einen Bedarf an einer horizontalen Hochskalierung gibt und Sie sich für den Ansatz mit Containern und/oder Microservices entschieden haben. Plötzlich stehen Sie vor Hunderten oder sogar Tausenden Computern, Dutzenden von verschiedenen Typen von Diensten, womöglich Hunderten von verschiedenen Instanzen dieser Dienste, von denen jeder zum Zweck der Hochverfügbarkeit über eine oder mehrere Instanzen oder Replikate verfügt.

Dann ist die Verwaltung Ihrer Umgebung nicht mehr so einfach wie das Verwalten einiger Computer, die einzelnen Typen von Workloads fest zugeordnet sind. Ihre Server sind virtuell und haben keine Namen mehr (denn Ihr Modell hat sich *tatsächlich* von einem [Haustierbestand in eine Rinderherde](http://www.slideshare.net/randybias/architectures-for-open-and-scalable-clouds/20) geändert). Die Konfiguration dreht sich weniger um die Computer und mehr um die Dienste selbst. Dedizierte Hardware gehört der Vergangenheit an. Dienste sind zu kleinen verteilten Systemen geworden, die mehrere kleinere Standardhardwarekomponenten überspannen.

Als Folge dieser Aufteilung Ihrer zuvor monolithischen, geschichteten App in verschiedene Dienste, die auf Standardhardware ausgeführt werden, müssen Sie nun mit wesentlich mehr Kombinationen zurechtkommen. Wer entscheidet, welche Arten und wie viele Workloads auf welcher Hardware ausgeführt werden können? Welche Workloads können problemlos auf der gleichen Hardware ausgeführt werden, bei welchen treten Konflikte auf? Wenn ein Computer ausfällt, … was wurde überhaupt darauf ausgeführt? Wer ist dafür verantwortlich, dass die Workload wieder den Betrieb aufnimmt? Warten Sie, bis der (virtuelle?) Computer wieder aktiv wird, oder führen Ihre Workloads automatisch ein Failover auf andere Computer durch und werden weiter ausgeführt? Ist Benutzereingriff erforderlich? Was ist mit Upgrades in dieser Art von Umgebung?

Als Entwickler und Operator brauchen wir Hilfe bei der Verwaltung dieser Komplexität, und es kristallisiert sich schnell heraus, dass eine Einstellungswelle und der Versuch, die Komplexität mit Personal in den Griff zu bekommen, nicht die richtige Antwort ist.

Vorgehensweise

## <a name="introducing-orchestrators"></a>Einführung in Orchestratoren
„Orchestrator“ ist der allgemeine Begriff für eine Softwarekomponente, mit deren Hilfe Administratoren diese Arten von Umgebungen verwalten können. Orchestratoren sind die Komponenten, die Anforderungen wie „Ich möchte 5 Kopien dieses Diensts in meiner Umgebung ausführen“ erfüllen und dann versuchen, diesen Zustand zu erhalten.

Orchestratoren (nicht Menschen) kommen zum Einsatz, wenn ein Computer ausfällt oder eine Workload aus einem unerwarteten Grund beendet wird. Die meisten Orchestratoren sind nicht nur mit Ausfällen beschäftigt, sondern helfen auch bei Neubereitstellungen, Upgrades und Optimierung der Ressourcennutzung. Doch grundlegend geht es stets um das Beibehalten eines gewünschten Konfigurationszustands in der Umgebung. Sie möchten einem Orchestrator angeben können, was Sie wünschen, und ihm die Hauptarbeit überlassen. Chronos oder Marathon auf Basis von Mesos, Fleet, Swarm, Kubernetes und Service Fabric sind alles Beispiele für Orchestratoren (oder verfügen über integrierte Orchestratoren). Weitere werden ständig entwickelt, da die Komplexität der Verwaltung realer Bereitstellungen in verschiedenen Arten von Umgebungen und unter unterschiedlichen Bedingungen zunimmt und sich ändert.

## <a name="orchestration-as-a-service"></a>Orchestrierung als Dienst
Die Aufgabe des Orchestrators in einem Service Fabric-Cluster übernimmt hauptsächlich der Clusterressourcen-Manager. Der Clusterressourcen-Manager von Service Fabric ist einer der Systemdienste in Service Fabric, der in jedem Cluster automatisch gestartet wird.  Im Allgemeinen kümmert sich der Clusterressourcen-Manager um drei Aufgaben:

1. Erzwingen von Regeln
2. Optimieren der Umgebung
3. Unterstützen anderer Prozesse

Sehen Sie sich das folgende Microsoft Virtual Academy-Video zur Funktionsweise der Clusterressourcen-Managers an: <center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=d4tka66yC_5706218965">
<img src="./media/service-fabric-cluster-resource-manager-introduction/ConceptsAndDemoVid.png" WIDTH="360" HEIGHT="244">
</a></center>

### <a name="what-it-isnt"></a>Was der Clusterressourcen-Manager nicht ist
Bei herkömmlichen Web-Apps mit „n“ Schichten gab es stets einen bestimmten „Lastenausgleich“ (auch Netzwerk- oder Anwendungslastenausgleich [NLB/ALB]), und zwar abhängig von dessen Position im Netzwerkstapel. Einige Load Balancer sind hardwarebasiert, wie z. B. das Big-IP-Angebot von F5, während andere softwarebasiert sind, wie z. B. Microsoft-Netzwerklastenausgleich (NLB). In anderen Umgebungen wird der Lastenausgleich möglicherweise von HAProxy oder einer ähnlichen Software ausgeführt. In diesen Architekturen ist es die Aufgabe des Lastenausgleichs sicherzustellen, dass alle zustandslosen Front-End-Computer bzw. die verschiedenen Computer im Cluster (ungefähr) dieselbe Verarbeitungslast erhalten. Hierfür gab es verschiedene Strategien, wie z. B. das Senden jedes unterschiedlichen Aufrufs an einen anderen Server, die Sitzungsbindung oder die tatsächliche Schätzung und Aufrufzuteilung basierend auf den erwarteten Kosten und der aktuellen Computerlast.

Beachten Sie, dass dies bestenfalls ein Mechanismus war, um sicherzustellen, dass die Webschicht ausgewogen ausgelastet war. Strategien für einen Lastenausgleich auf der Datenschicht waren vollkommen anders und hingen vom Datenspeichermechanismus ab, wobei der Fokus meist auf dem Sharding und Zwischenspeichern von Daten, von der Datenbank verwalteten Sichten und gespeicherten Prozeduren usw. lag.

Während einige dieser Strategien interessant sind, ist der Clusterressourcen-Manager von Service Fabric mit einem NLB oder Cache nicht vergleichbar. Ein Netzwerklastenausgleich stellt sicher, dass die Front-Ends gleichmäßig ausgelastet sind, indem Datenverkehr dorthin verlagert wird, wo die Dienste ausgeführt werden. Der Clusterressourcen-Manager von Service Fabric verfolgt eine vollkommen andere Strategie: Service Fabric verschiebt *Dienste* dorthin, wo es am sinnvollsten ist (und erwartet, dass Datenverkehr bzw. Last dorthin folgen). Dies können z.B. Knoten sein, die nicht viel zu tun haben, da die vorhandenen Dienste gerade nicht viel Arbeit verrichten oder da die Dienste gelöscht oder woandershin verschoben wurden. In einem anderen Beispiel könnte der Clusterressourcen-Manager auch einen Dienst aus einem Computer verschieben, der kurz vor einem Upgrade steht oder aufgrund einer Nutzungsspitze der darauf ausgeführten Dienste überlastet ist. Da der Clusterressourcen-Manager für das Verschieben von Diensten zuständig ist (nicht dafür, den Netzwerkdatenverkehr dort bereitzustellen, wo sich die Dienste bereits befinden), umfasst er einen wesentlich Funktionssatz als ein Netzwerklastenausgleichsmodul und verfolgt grundlegend andere Strategien, um sicherzustellen, dass die Hardwareressourcen im Cluster gut genutzt werden.

## <a name="next-steps"></a>Nächste Schritte
* Informationen über die Architektur und den Informationsfluss innerhalb des Clusterressourcen-Managers finden Sie in [diesem Artikel ](service-fabric-cluster-resource-manager-architecture.md)
* Der Clusterressourcen-Manager bietet viele Optionen für die Beschreibung des Clusters. Weitere Informationen hierzu finden Sie in diesem Artikel zum [Beschreiben eines Service Fabric-Clusters](service-fabric-cluster-resource-manager-cluster-description.md)
* Weitere Informationen zu den anderen Optionen, die für die Konfiguration von Diensten zur Verfügung stehen, finden Sie im Thema zu den anderen verfügbaren Clusterressourcen-Manager-Konfigurationen unter [Konfigurieren von Diensten](service-fabric-cluster-resource-manager-configure-services.md)
* Metriken bestimmen, wie der Clusterressourcen-Manager von Service Fabric den Ressourcenverbrauch und die Kapazität im Cluster verwaltet. Weitere Informationen zu Metriken und deren Konfiguration finden Sie in [diesem Artikel](service-fabric-cluster-resource-manager-metrics.md)
* Der Clusterressourcen-Manager arbeitet mit den Verwaltungsfunktionen von Service Fabric. Weitere Informationen hierzu finden Sie in [diesem Artikel](service-fabric-cluster-resource-manager-management-integration.md)
* Informationen darüber, wie der Clusterressourcen-Manager die Auslastung im Cluster verwaltet und verteilt, finden Sie im Artikel zum [Lastenausgleich](service-fabric-cluster-resource-manager-balancing.md)




<!--HONumber=Dec16_HO2-->


