---
title: Azure Container Instances und Containerorchestrierung
description: Verstehen Sie, wie Azure Container Instances mit Containerorchestratoren interagiert
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/24/2017
ms.author: seanmck
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: a678700884b612cad6281eb8f3b74ce63a0ebb69
ms.openlocfilehash: 6f0a1e5263a034e825d4c12860fd6f97a4c556c6
ms.contentlocale: de-de
ms.lasthandoff: 07/26/2017

---

# <a name="azure-container-instances-and-container-orchestrators"></a>Azure Container Instances und Containerorchestratoren

Aufgrund ihrer kleinen Größe und Anwendungsorientierung sind Container gut für agile Delivery-Umgebungen und auf Microservice basierende Architekturen geeignet. Die Automatisierung und Verwaltung einer großen Anzahl von Containern und deren Interaktion wird als *Orchestrierung* bezeichnet. Zu den beliebten Containerorchestratoren zählen Kubernetes, DC/OS und Docker Swarm, die alle in [Azure Container Service](https://docs.microsoft.com/azure/container-service/) verfügbar sind.

Azure Container Instances bietet einige der grundlegenden Planungsfunktionen von Orchestrierungsplattformen, aber nicht die höherwertigen Dienste solcher Plattformen, und kann als Ergänzung dazu dienen. In diesem Artikel werden die Funktionen von Azure Container Instances beschrieben und wie vollständige Containerorchestratoren damit interagieren können.

## <a name="traditional-orchestration"></a>Herkömmliche Orchestrierung

Die Standarddefinition der Orchestrierung umfasst die folgenden Aufgaben:

- **Planen**: Findet für ein Containerimage und eine Ressourcenanforderung einen passenden Computer, auf dem der Container ausgeführt werden kann.
- **Affinität/Anti-Affinität**: Gibt an, dass ein Satz von Containern in der Nähe voneinander (für Leistung) oder ausreichend weit voneinander (für Verfügbarkeit) ausgeführt werden soll.
- **Systemüberwachung**: Bemerkt Containerfehler, und plant diese automatisch neu.
- **Failover**: Verfolgt nach, was auf jedem Computer ausgeführt wird, und plant Container von Computern mit Fehlern auf intakte Knoten um.
- **Skalierung**: Fügt Containerinstanzen nach Bedarf manuell oder automatisch hinzu oder entfernt sie.
- **Netzwerk**: Bietet ein Überlagerungsnetzwerk für die Koordinierung der Kommunikation von Containern in mehreren Hostcomputer.
- **Dienstermittlung**: Ermöglicht, dass sich Container automatisch finden, auch wenn sie den Hostcomputer wechseln oder die IP-Adresse ändern.
- **Koordinierte Anwendungsupgrades**: Verwaltet Containerupgrades zur Vermeidung von Ausfallzeiten der Anwendung und ermöglicht Rollbacks, falls etwas schief geht.

## <a name="orchestration-with-azure-container-instances-a-layered-approach"></a>Orchestrierung mit Azure Container Instances: Ein mehrstufiger Ansatz

Azure Container Instances ermöglicht einen mehrstufigen Ansatz für die Orchestrierung, bietet alle Planungs- und Verwaltungsfunktionen für das Ausführen eines einzelnen Containers, und lässt außerdem zu, dass Orchestrierungsplattformen Aufgaben mit mehreren Containern verwalten.

Da die gesamte zugrundeliegende Infrastruktur von Container Instances von Azure verwaltet wird, muss sich eine Orchestrierungsplattform nicht darum kümmern, einen geeigneten Hostcomputer zu finden, auf dem ein einzelner Container ausgeführt werden kann. Die Flexibilität der Cloud stellt sicher, dass immer einer verfügbar ist. Stattdessen kann sich der Orchestrator auf die Aufgaben konzentrieren, die die Entwicklung von Architekturen mit mehreren Containern vereinfachen, darunter das Skalieren und Koordinieren von Upgrades.



## <a name="potential-scenarios"></a>Mögliche Szenarios

Die Orchestratorintegration mit Azure Container Instances ist zwar noch am Entstehen, aber wir gehen davon aus, dass einige andere Umgebungen entstehen können:

### <a name="orchestration-of-container-instances-exclusively"></a>Ausschließliche Orchestrierung von Container Instances

Aufgrund des schnellen Starts und der sekundengenauen Abrechnung bietet eine ausschließlich auf Azure Container Instances basierende Umgebung die schnellste Möglichkeit, um einzusteigen und mit sehr variablen Workloads umzugehen.

### <a name="combination-of-container-instances-and-containers-in-virtual-machines"></a>Kombination aus Container Instances und Containern in virtuellen Computern

Für stabile Workloads mit langer Ausführungszeit ist es normalerweise günstiger, Container in einem Cluster dedizierter virtueller Computer auszuführen als dieselben Container mit Container Instances auszuführen. Allerdings bietet Container Instances eine tolle Lösung für die schnelle Erweiterung oder Verringerung Ihrer Gesamtkapazität, um auf unerwartete oder vorübergehende Spitzen bei der Auslastung zu reagieren. Anstatt die Anzahl der virtuellen Computer in Ihrem Cluster horizontal zu skalieren und dann zusätzliche Container auf diesen Computern bereitzustellen, kann der Orchestrator einfach die zusätzlichen Container mithilfe von Container Instances einplanen und sie wieder entfernen, wenn sie nicht mehr gebraucht werden.

## <a name="sample-implementation-azure-container-instances-connector-for-kubernetes"></a>Beispiel für die Implementierung: Azure Container Instances-Connector für Kubernetes

Um zu zeigen, wie Containerorchestrierungsplattformen in Azure Container Instances integriert werden können, haben wir damit begonnen, einen [Beispiel-Connector für Kubernetes][aci-connector-k8s] zu erstellen. 

Der Connector für Kubernetes imitiert das [Kubelet][kubelet-doc], indem er sich als Knoten mit unbegrenzter Kapazität registriert und die Erstellung von [Pods][pod-doc] als Containergruppen in Azure Container Instances erledigt. 

<!-- ![ACI Connector for Kubernetes][aci-connector-k8s-gif] -->

Connectors für andere Orchestratoren könnten auf ähnliche Weise gebaut und in Plattformgrundtypen integriert werden, um die Leistung der Orchestrator-API mit der Geschwindigkeit und einfachen Verwaltung von Containern in Azure Container Instances zu kombinieren.

> [!WARNING]
> Der ACI-Connector für Kubernetes ist *experimentell* und sollte nicht in der Produktion verwendet werden.

## <a name="next-steps"></a>Nächste Schritte

Erstellen Sie Ihren ersten Container mit Azure Container Instances mithilfe der [Kurzanleitung](container-instances-quickstart.md).

<!-- IMAGES -->
[aci-connector-k8s-gif]: ./media/container-instances-orchestrator-relationship/aci-connector-k8s.gif

<!-- LINKS -->
[aci-connector-k8s]: https://github.com/azure/aci-connector-k8s
[kubelet-doc]: https://kubernetes.io/docs/admin/kubelet/
[pod-doc]: https://kubernetes.io/docs/concepts/workloads/pods/pod/
