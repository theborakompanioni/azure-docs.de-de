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
ms.date: 01/05/2017
ms.author: masnider
translationtype: Human Translation
ms.sourcegitcommit: dafaf29b6827a6f1c043af3d6bfe62d480d31ad5
ms.openlocfilehash: 67f07bad8f6f89d9e5e68326f0cc194d920e841b


---
# <a name="cluster-resource-manager-architecture-overview"></a>Übersicht über die Architektur des Clusterressourcen-Managers
Um die Ressourcen in Ihrem Cluster zu verwalten, benötigt der Clusterressourcen-Manager von Service Fabric verschiedene Informationen. Er muss wissen, welche Dienste derzeit vorhanden sind und wie viele Ressourcen diese Dienste aktuell (oder standardmäßig) belegen. Um die verfügbaren Ressourcen im Cluster nachzuverfolgen, müssen die Kapazität der Knoten im Cluster und die Menge der auf den einzelnen Knoten genutzten Ressourcen bekannt sein. Die Ressourcennutzung eines bestimmten Diensts kann sich mit der Zeit ändern, und Dienste beanspruchen meist mehr als nur einen Ressourcentyp. Dienstübergreifend können sowohl reale, physische als auch logische Ressourcen gemessen werden. Dienste können physische Metriken wie Arbeitsspeicher- oder Datenträgerspeichernutzung nachverfolgen. Häufiger benötigen Dienste logische Metriken, z.B. „WorkQueueDepth“ oder „TotalRequests“. Sowohl logische als auch physische Metriken können für eine Vielzahl verschiedener Arten von Diensten oder nur für eine Reihe von Diensten verwendet werden.

## <a name="other-considerations"></a>Weitere Überlegungen
Die Besitzer und die Operatoren des Clusters unterscheiden sich gelegentlich von den Dienstautoren, zumindest nehmen sie verschiedene Rollen ein. Wenn Sie Ihren Dienst entwickeln, kennen Sie beispielsweise einige Aspekte im Hinblick auf die benötigten Ressourcen und die ideale Bereitstellung der verschiedenen Komponenten. Die Person, die sich an einem Livestandort für diesen Dienst in der Produktion mit einem Incident befasst, hat jedoch ganz andere Aufgaben und benötigt andere Tools. Darüber hinaus werden weder der Cluster noch die Dienste statisch konfiguriert:

* Die Anzahl von Knoten im Cluster kann steigen und sinken.
* Knoten von verschiedenen Größen und Typen können hinzukommen und entfernt werden.
* Dienste können erstellt oder entfernt werden und ihre gewünschten Ressourcenzuteilungen ändern.
* Upgrades oder andere Verwaltungsvorgänge können im Cluster erfolgen, und Komponenten können jederzeit ausfallen.

## <a name="cluster-resource-manager-components-and-data-flow"></a>Komponenten und Datenfluss des Clusterressourcen-Managers
Der Clusterressourcen-Manager muss die Anforderungen einzelner Dienste und die Nutzung von Ressourcen durch die einzelnen Dienstobjekte verfolgen, aus denen diese Dienste bestehen. Um dies zu erreichen, umfasst der Clusterressourcen-Manager aus zwei konzeptionellen Teilen: aus Agents, die auf den einzelnen Knoten ausgeführt werden, und aus einem fehlertoleranten Dienst. Die Agents auf den einzelnen Knoten verfolgen Auslastungsberichte von Diensten, fassen sie zusammen und übermitteln sie in regelmäßigen Abständen. Der Clusterressourcen-Manager-Dienst aggregiert alle Informationen von den lokalen Agents und reagiert basierend auf seiner aktuellen Konfiguration.

Betrachten Sie das folgende Diagramm:

<center>
![Resource Balancer-Architektur][Image1]
</center>

Es gibt viele Änderungen, die während der Laufzeit auftreten können. Nehmen wir beispielsweise an, dass sich der Umfang der Ressourcen ändert, die von manchen Diensten genutzt werden, dass einige Dienste ausfallen und dass einige Knoten dem Cluster beitreten oder ihn verlassen. Alle Änderungen auf einem Knoten werden gesammelt und regelmäßig an den zentralen Clusterressourcen-Manager-Dienst (1,2) gesendet, wo sie erneut zusammengefasst, analysiert und gespeichert werden. Mit einem Intervall von einigen wenigen Sekunden untersucht dieser Dienst die Änderungen und ermittelt, ob Aktionen erforderlich sind (3). Er könnte z.B. feststellen, dass dem Cluster einige leere Knoten hinzugefügt wurden, und entscheiden, einige Dienste auf diese Knoten zu verschieben. Der Clusterressourcen-Manager könnte aber auch bemerken, dass ein bestimmter Knoten überlastet ist oder dass bestimmte Dienste ausgefallen sind (oder gelöscht wurden), und Ressourcen an anderer Stelle freigeben.

Betrachten Sie das folgende Diagramm, und sehen Sie sich an, was als Nächstes geschieht. Nehmen wir an, der Clusterressourcen-Manager ermittelt, dass Änderungen erforderlich sind. Er koordiniert sich mit anderen Systemdiensten (insbesondere dem Failover-Manager), um die erforderlichen Änderungen vorzunehmen. Anschließend werden die notwendigen Befehle an die entsprechenden Knoten gesendet (4). In diesem Fall nehmen wir an, dass der Ressourcen-Manager bemerkt hat, dass Knoten 5 überlastet war, und daher entschieden hat, Dienst B von N5 auf N4 zu verschieben. Am Ende der Neukonfiguration (5) sieht der Cluster folgendermaßen aus:

<center>
![Resource Balancer-Architektur][Image2]
</center>

## <a name="next-steps"></a>Nächste Schritte
* Der Clusterressourcen-Manager bietet zahlreiche Optionen zum Beschreiben des Clusters. Weitere Informationen hierzu finden Sie in diesem Artikel zum [Beschreiben eines Service Fabric-Clusters](service-fabric-cluster-resource-manager-cluster-description.md).

[Image1]:./media/service-fabric-cluster-resource-manager-architecture/Service-Fabric-Resource-Manager-Architecture-Activity-1.png
[Image2]:./media/service-fabric-cluster-resource-manager-architecture/Service-Fabric-Resource-Manager-Architecture-Activity-2.png



<!--HONumber=Feb17_HO3-->


