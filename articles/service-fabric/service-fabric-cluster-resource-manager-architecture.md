---
title: "Clusterressourcen-Manager von Service Fabric – Architektur | Microsoft Docs"
description: "Übersicht über die Architektur des Clusterressourcen-Managers von Service Fabric"
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 6c4421f9-834b-450c-939f-1cb4ff456b9b
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/19/2016
ms.author: masnider
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 3ec8800b947c227d4d1087283b0964ce22eca4df


---
# <a name="cluster-resource-manager-architecture-overview"></a>Übersicht über die Architektur des Clusterressourcen-Managers
Um die Ressourcen in Ihrem Cluster zu verwalten, benötigt der Clusterressourcen-Manager von Service Fabric verschiedene Informationen. Er muss wissen, welche Dienste derzeit vorhanden sind und wie viele Ressourcen diese Dienste aktuell (oder standardmäßig) belegen. Eine weitere Information ist die tatsächliche Kapazität der Knoten im Cluster und somit die Anzahl der verfügbaren Ressourcen, sowohl im Cluster als Ganzes und als auch auf einem bestimmten Knoten. Die Ressourcennutzung eines bestimmten Diensts kann sich mit der Zeit ändern, und Dienste benötigen meist mehr als nur eine Ressource. Bei vielen Diensten können sowohl echte physische Ressourcen (z.B. die Nutzung von Arbeitsspeicher und Datenträgern) als auch logische Metriken (z.B. WorkQueueDepth oder TotalRequests) erfasst und als Metriken angezeigt werden, wobei die Erfassung logischer Metriken tatsächlich gängiger ist. Sowohl logische als auch physische Metriken können für eine Vielzahl verschiedener Arten von Diensten oder nur für eine Reihe von Diensten verwendet werden.

## <a name="other-considerations"></a>Weitere Überlegungen
Gelegentlich sind die Besitzer und Betreiber des Clusters nicht auch die Ersteller der Dienste. Möglich ist auch, dass es sich um dieselben Personen mit unterschiedlichen Rollen handelt. Wenn Sie Ihren Dienst z.B. entwickeln, verfügen Sie über Informationen zu den Ressourcenanforderungen und zur idealen Bereitstellung der Komponenten. Wenn Sie jedoch für Incidents auf einer Live-Website für diesen Dienst in der Produktion verantwortlich sind, haben Sie andere Aufgaben, für die Sie andere Tools benötigen. Darüber hinaus sind weder der Cluster noch die Dienste selbst statisch konfiguriert: die Anzahl von Knoten im Cluster kann erhöht oder verringert werden, Knoten verschiedener Größen können hinzukommen oder entfernt werden, und Dienste können eine andere Ressourcenzuteilung erhalten oder erstellt und entfernt werden. Upgrades oder andere Verwaltungsvorgänge können im Cluster erfolgen, und natürlich können Komponenten jederzeit ausfallen.

## <a name="cluster-resource-manager-components-and-data-flow"></a>Komponenten und Datenfluss des Clusterressourcen-Managers
Der Clusterressourcen-Manager benötigt eine Vielzahl von Informationen zum Cluster sowie zu den Anforderungen der einzelnen Dienste und zustandslosen Instanzen bzw. zustandsbehafteten Replikate, aus denen sich der Cluster zusammensetzt. Um dies zu erreichen, werden einerseits Agents des Clusterressourcen-Managers auf einzelnen Knoten ausgeführt, um Informationen zur lokalen Ressourcennutzung zusammenzutragen, sowie andererseits ein zentraler, fehlertoleranter Clusterressourcen-Manager-Dienst, der alle Informationen zu den Diensten und zum Cluster sammelt und basierend auf seiner aktuellen Konfiguration reagiert. Die Fehlertoleranz des Clusterressourcen-Manager-Diensts (und aller anderen Systemdienste) wird über denselben Mechanismus erreicht, der auch für Ihre Dienste verwendet wird: Der Zustand eines Diensts wird auf einen Quorumdatenträger mit einer bestimmten Anzahl von Replikaten innerhalb des Clusters repliziert (üblicherweise 7).

![Resource Balancer-Architektur][Image1]

Werfen Sie als Beispiel einen Blick auf das obige Diagramm. Zur Laufzeit können viele verschiedene Änderungen auftreten. Beispielsweise können Änderungen an der Menge der Ressourcen vorkommen, die Dienste nutzen, Dienstausfälle können passieren, einige Knoten können dem Cluster hinzugefügt oder daraus entfernt werden usw. Alle Änderungen auf einem bestimmten Knoten werden gesammelt und regelmäßig an den zentralen Clusterressourcen-Manager-Dienst (1,2) gesendet, wo sie aggregiert, analysiert und gespeichert werden. Mit einem Intervall von einigen wenigen Sekunden untersucht dieser Dienst alle Änderungen und ermittelt, ob Aktionen erforderlich sind (3). Er könnte z. B. feststellen, dass Knoten dem Cluster hinzugefügt wurden und leer sind und entscheiden, einige Dienste auf diese Knoten zu verschieben. Er könnte aber auch bemerken, dass ein bestimmter Knoten überlastet ist oder dass bestimmte Dienste ausgefallen sind (oder gelöscht wurden), und Ressourcen auf anderen Knoten freigeben.

Betrachten Sie das folgende Diagramm, und sehen Sie sich an, was als Nächstes geschieht. Nehmen wir an, der Clusterressourcen-Manager ermittelt, dass Änderungen erforderlich sind. Er koordiniert sich mit anderen Systemdiensten (insbesondere dem Failover-Manager), um die erforderlichen Änderungen vorzunehmen. Anschließend werden die Änderungsanforderungen an die entsprechenden Knoten gesendet (4). In diesem Fall nehmen wir an, dass der Ressourcen-Manager bemerkt hat, dass Knoten 5 überlastet war, und daher entschieden hat, Dienst B von N5 auf N4 zu verschieben. Am Ende der Neukonfiguration (5) sieht der Cluster folgendermaßen aus:

![Resource Balancer-Architektur][Image2]

## <a name="next-steps"></a>Nächste Schritte
* Der Clusterressourcen-Manager bietet viele Optionen für die Beschreibung des Clusters. Weitere Informationen hierzu finden Sie in diesem Artikel zum [Beschreiben eines Service Fabric-Clusters](service-fabric-cluster-resource-manager-cluster-description.md)

[Image1]:./media/service-fabric-cluster-resource-manager-architecture/Service-Fabric-Resource-Manager-Architecture-Activity-1.png
[Image2]:./media/service-fabric-cluster-resource-manager-architecture/Service-Fabric-Resource-Manager-Architecture-Activity-2.png



<!--HONumber=Dec16_HO2-->


