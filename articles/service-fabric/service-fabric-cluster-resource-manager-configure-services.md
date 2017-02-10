---
title: Konfigurieren von Diensten mit dem Clusterressourcen-Manager von Service Fabric | Microsoft Docs
description: "Beschreibt einen Service Fabric-Dienst durch Angabe der Metriken, Platzierungseinschränkungen und weiterer Platzierungsrichtlinien"
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
ms.date: 08/19/2016
ms.author: masnider
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: e21127ab5c59e0acb3ffb28c44518e5baa5fe868


---
# <a name="configuring-cluster-resource-manager-settings-for-service-fabric-services"></a>Konfigurieren von Einstellungen des Clusterressourcen-Managers für Service Fabric-Dienste
Der Clusterressourcen-Manager von Service Fabric ermöglicht eine sehr detaillierte Steuerung der Regeln, die jeden einzelnen benannten Dienst lenken. Jede benannte Dienstinstanz kann Regeln dafür angeben, wie die Instanz im Cluster zugewiesen werden soll, und den Satz von Metriken definieren, den sie melden will, inklusive der Wichtigkeit dieser Metriken für den Dienst. Das Konfigurieren von Diensten besteht im Allgemeinen aus drei verschiedenen Aufgaben:

1. Konfigurieren von Platzierungseinschränkungen
2. Konfigurieren von Metriken
3. Konfigurieren von erweiterten Platzierungsrichtlinien (seltener)

Die einzelnen Punkte im Detail:

## <a name="placement-constraints"></a>Platzierungseinschränkungen
Platzierungseinschränkungen steuern, an welchen Knoten im Cluster ein Dienst tatsächlich ausgeführt werden kann. Typischerweise werden Sie eine bestimmte benannte Dienstinstanz oder alle Dienste eines bestimmten Typs sehen, die auf die Ausführung auf einem bestimmten Knotentyp beschränkt sind. Platzierungseinschränkungen sind allerdings erweiterbar – Sie können einen beliebigen Satz Eigenschaften pro Knotentyp definieren und beim Erstellen des Diensts die gewünschten Einschränkungen aus diesen auswählen. Platzierungseinschränkungen sind über die Lebensdauer des Diensts dynamisch aktualisierbar, sodass Sie auf Veränderungen im Cluster reagieren können. Die Eigenschaften eines Knotens können auch dynamisch im Cluster aktualisiert werden. Weitere Informationen zu Platzierungseinschränkungen und deren Konfiguration finden Sie in [diesem Artikel](service-fabric-cluster-resource-manager-cluster-description.md#placement-constraints-and-node-properties)

## <a name="metrics"></a>Metriken
Metriken stellen den Satz von Ressourcen dar, die eine bestimmte benannte Dienstinstanz benötigt, und enthalten Informationen darüber, wie viel dieser Ressourcen jedes zustandsbehaftete Replikat oder jede zustandslose Instanz des Diensts standardmäßig konsumiert. Eine Metrik enthält auch eine Gewichtung, die darauf hinweist, wie wichtig der Ausgleich dieser Metrik für den Dienst ist, falls Kompromisse erforderlich sind.

## <a name="other-placement-rules"></a>Weitere Platzierungsregeln
Es gibt weitere Platzierungsregeln, die in erster Linie in geografisch verstreuten Clustern hilfreich sind, oder in anderen, weniger üblichen Szenarien Anwendung finden. Sie werden mithilfe von Korrelationen oder Richtlinien konfiguriert. Obwohl sie nur in einer geringen Anzahl von Szenarien Verwendung finden, werden sie aus Gründen der Vollständigkeit beschrieben.

## <a name="next-steps"></a>Nächste Schritte
* Metriken bestimmen, wie der Clusterressourcen-Manager von Service Fabric den Ressourcenverbrauch und die Kapazität im Cluster verwaltet. Weitere Informationen zu Metriken und deren Konfiguration finden Sie in [diesem Artikel](service-fabric-cluster-resource-manager-metrics.md)
* Affinität ist ein Modus, den Sie für Ihre Dienste konfigurieren können. Er ist nicht üblich, aber bei Bedarf erhalten Sie [hier](service-fabric-cluster-resource-manager-advanced-placement-rules-affinity.md)
* Es gibt viele verschiedene Platzierungsregeln, die für Ihren Dienst konfiguriert werden können, um zusätzliche Szenarien zu verarbeiten. Informationen zu diesen verschiedenen Platzierungsrichtlinien finden Sie [hier](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md)
* Starten Sie mit einer [Einführung in den Clusterressourcen-Manager von Service Fabric](service-fabric-cluster-resource-manager-introduction.md)
* Informationen darüber, wie der Clusterressourcen-Manager die Auslastung im Cluster verwaltet und verteilt, finden Sie im Artikel zum [Lastenausgleich](service-fabric-cluster-resource-manager-balancing.md)
* Der Clusterressourcen-Manager bietet viele Optionen für die Beschreibung des Clusters. Weitere Informationen hierzu finden Sie in diesem Artikel zum [Beschreiben eines Service Fabric-Clusters](service-fabric-cluster-resource-manager-cluster-description.md)




<!--HONumber=Nov16_HO3-->


