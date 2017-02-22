---
title: Angeben von Metriken und Platzierungseinstellungen in Azure-Microservices | Microsoft-Dokumentation
description: "Beschreiben eines Service Fabric-Diensts durch Angeben von Metriken, Platzierungseinschränkungen und anderen Platzierungsrichtlinien."
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 16e135c1-a00a-4c6f-9302-6651a090571a
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/05/2017
ms.author: masnider
translationtype: Human Translation
ms.sourcegitcommit: bb27d279396aa7b670187560cebe2ed074576bad
ms.openlocfilehash: c1df3b77f3fa621a60d6ab73f6dc2c24abbc3366


---
# <a name="configuring-cluster-resource-manager-settings-for-service-fabric-services"></a>Konfigurieren von Einstellungen des Clusterressourcen-Managers für Service Fabric-Dienste
Der Clusterressourcen-Manager von Service Fabric ermöglicht eine detaillierte Steuerung der Regeln, die für jeden einzelnen benannten Dienst gelten. Jede Instanz eines benannten Diensts kann Regeln für seine Zuordnung im Cluster angeben. Darüber hinaus kann jeder benannte Dienst die zu meldenden Metriken und deren jeweilige Bedeutung für den Dienst definieren. Zum Konfigurieren von Diensten müssen drei Aufgaben ausgeführt werden:

1. Konfigurieren von Platzierungseinschränkungen
2. Konfigurieren von Metriken
3. Konfigurieren von erweiterten Platzierungsrichtlinien und anderen Regeln (eher selten)

## <a name="placement-constraints"></a>Platzierungseinschränkungen
Platzierungseinschränkungen steuern, an welchen Knoten im Cluster ein Dienst tatsächlich ausgeführt werden kann. Hierbei handelt es sich in der Regel um eine bestimmte benannte Dienstinstanz oder um alle Dienste eines bestimmten Typs, die auf die Ausführung auf einem bestimmten Knotentyp beschränkt sind. Platzierungseinschränkungen sind allerdings erweiterbar – Sie können einen beliebigen Satz Eigenschaften pro Knotentyp definieren und beim Erstellen des Diensts die gewünschten Einschränkungen aus diesen auswählen. Platzierungseinschränkungen sind über die Lebensdauer des Diensts dynamisch aktualisierbar, sodass Sie auf Veränderungen im Cluster reagieren können. Die Eigenschaften eines Knotens können auch dynamisch im Cluster aktualisiert werden. Weitere Informationen zu Platzierungseinschränkungen und deren Konfiguration finden Sie in [diesem Artikel](service-fabric-cluster-resource-manager-cluster-description.md#placement-constraints-and-node-properties)

## <a name="metrics"></a>Metriken
Bei Metriken handelt es sich um einen Satz von Ressourcen, den eine bestimmte benannte Dienstinstanz benötigt. Die Metrikkonfiguration eines Diensts gibt unter anderem den Anteil der Ressource an, den jedes zustandsbehaftete Replikat und jede zustandslose Instanz dieses Diensts standardmäßig verbraucht. Metriken enthalten auch eine Gewichtung, die angibt, wie wichtig der Ausgleich dieser Metrik für den Dienst ist, falls Kompromisse erforderlich sind.

## <a name="other-placement-rules"></a>Weitere Platzierungsregeln
Es gibt auch noch andere Arten von Platzierungsregeln, die in geografisch verteilten Clustern oder in anderen, weniger gängigen Szenarien hilfreich sind. Weitere Platzierungsregeln werden entweder über Korrelationen oder über Richtlinien konfiguriert.

## <a name="next-steps"></a>Nächste Schritte
* Metriken bestimmen, wie der Clusterressourcen-Manager von Service Fabric den Ressourcenverbrauch und die Kapazität im Cluster verwaltet. Weitere Informationen zu Metriken und deren Konfiguration finden Sie in [diesem Artikel](service-fabric-cluster-resource-manager-metrics.md).
* Affinität ist ein Modus, den Sie für Ihre Dienste konfigurieren können. Er ist nicht üblich, aber bei Bedarf erhalten Sie [hier](service-fabric-cluster-resource-manager-advanced-placement-rules-affinity.md)
* Es gibt viele verschiedene Platzierungsregeln, die für Ihren Dienst konfiguriert werden können, um zusätzliche Szenarien zu verarbeiten. Informationen zu diesen verschiedenen Platzierungsrichtlinien finden Sie [hier](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md)
* Starten Sie mit einer [Einführung in den Clusterressourcen-Manager von Service Fabric](service-fabric-cluster-resource-manager-introduction.md)
* Informationen darüber, wie der Clusterressourcen-Manager die Auslastung im Cluster verwaltet und verteilt, finden Sie im Artikel zum [Lastenausgleich](service-fabric-cluster-resource-manager-balancing.md)
* Der Clusterressourcen-Manager bietet zahlreiche Optionen zum Beschreiben des Clusters. Weitere Informationen hierzu finden Sie in [diesem Artikel](service-fabric-cluster-resource-manager-cluster-description.md).



<!--HONumber=Jan17_HO4-->


