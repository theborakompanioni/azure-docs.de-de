---
title: Defragmentierung von Metriken in Azure Service Fabric | Microsoft Docs
description: "Eine Übersicht über den Einsatz von Defragmentierung oder Überlastung als Strategie für Metriken in Service Fabric"
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: e5ebfae5-c8f7-4d6c-9173-3e22a9730552
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/05/2017
ms.author: masnider
translationtype: Human Translation
ms.sourcegitcommit: dafaf29b6827a6f1c043af3d6bfe62d480d31ad5
ms.openlocfilehash: 5ef6381f7d182c818171eca3e3d32a00bc30268e


---
# <a name="defragmentation-of-metrics-and-load-in-service-fabric"></a>Defragmentierung von Metriken und Lasten in Service Fabric
Der Clusterressourcen-Manager von Service Fabric wird hauptsächlich für den Lastenausgleich in Bezug auf die Verteilung der Last eingesetzt und stellt sicher, dass die Knoten im Cluster gleichmäßig genutzt werden. Die Verteilung von Workloads ist das sicherste Konzept im Hinblick auf das Überstehen von Ausfällen, da es sicherstellt, dass bei einem Ausfall kein hoher Prozentsatz einer Workload verloren geht. Der Clusterressourcen-Manager von Service Fabric unterstützt auch eine andere Strategie, die Defragmentierung genannt wird. Defragmentierung bedeutet im Allgemeinen, dass anstatt zu versuchen, die Nutzung einer Metrik auf den Cluster zu verteilen, wir tatsächlich versuchen sollten, sie zu konsolidieren. Die Konsolidierung ist eine Umkehrung unserer normalen Strategie – anstatt die Standardabweichung der Metriklast zu minimieren, zielt der Clusterressourcen-Manager auf eine größere Abweichung ab. Doch was spricht für eine solche Strategie?

Wenn Sie die Last gleichmäßig auf die Knoten im Cluster verteilen, haben Sie einige der Ressourcen belegt, die die Knoten zu bieten haben. Einige Workloads erstellen jedoch Dienste, die außergewöhnlich groß sind und den größten Teil eines Knotens nutzen. In diesen Fällen kann es vorkommen, dass 75 bis 95% der Ressourcen eines Knotens dediziert für ein einzelnes Dienstobjekt benötigt werden. Große Workloads sind kein Problem. Der Clusterressourcen-Manager legt zur Diensterstellungszeit fest, dass der Cluster neu organisiert werden muss, um Platz für diese große Workload zu schaffen. In der Zwischenzeit muss diese Workload jedoch warten, bis sie im Cluster geplant werden kann.

Wenn viele Dienste und Status zu verschieben sind, kann es lange dauern, bis die große Workload im Cluster platziert wird. Dies ist wahrscheinlich, wenn andere Workloads im Cluster groß sind und das Verschieben daher viel Zeit in Anspruch nimmt. Das Service Fabric-Team hat die Erstellungszeiten in Simulationen dieses Szenarios gemessen. Dabei haben wir herausgefunden, dass die Erstellung von Diensten sehr langsam erfolgt, wenn diese Dienste sehr groß sind und der Cluster einen hohen Auslastungsgrad aufweist. Um dieses Szenario zu verarbeiten, haben wir die Defragmentierung als Strategie für den Lastenausgleich eingeführt. Wir haben festgestellt, dass bei großen Workloads – insbesondere solchen, bei denen die Erstellungsdauer eine große Rolle spielt – die Defragmentierung in hohem Maß dazu beiträgt, dass diese neuen Workloads im Cluster geplant werden können.

Sie können Defragmentierungsmetriken konfigurieren, damit der Clusterressourcen-Manager proaktiv versucht, die Last der Dienste auf eine geringere Anzahl von Knoten zu verteilen. So lässt sich sicherstellen, dass (nahezu) immer genügend Platz auch für sehr große Dienste vorhanden ist. So können solche Dienste bei Bedarf sehr schnell erstellt werden.

Die meisten Benutzer benötigen keine Defragmentierung. Dienste sind normalerweise klein, daher ist es nicht schwierig, genügend Platz für sie im Cluster zu finden. Wenn Sie jedoch mit großen Diensten arbeiten und diese schnell erstellen müssen (und die anderen Nachteile akzeptieren können), ist die Defragmentierung die richtige Strategie für Sie.

Welche Nachteile gibt es? Die Defragmentierung kann die Auswirkungen von Ausfällen verstärken, da mehr Dienste auf einem Knoten ausgeführt werden. Darüber hinaus sorgt die Defragmentierung dafür, dass einige Ressourcen im Cluster nicht genutzt werden, während sie darauf warten, dass Workloads geplant werden.

Das folgende Diagramm ist eine visuelle Darstellung von zwei verschiedenen Clustern – einer ist defragmentiert, der andere nicht. Beachten Sie im Cluster mit Lastenausgleich die Anzahl von Verschiebungen, die notwendig wären, um eines der größten Dienstobjekte zu platzieren. Vergleichen Sie dies mit dem defragmentierten Cluster, in dem eine große Workload sofort in den Knoten&4; oder&5; platziert werden könnte.

<center>
![Vergleich zwischen Clustern mit Lastenausgleich und defragmentierten Clustern][Image1]
</center>

## <a name="defragmentation-pros-and-cons"></a>Vor- und Nachteile der Defragmentierung
Was sind die anderen grundlegenden Kompromisse? Es wird empfohlen, vor dem Aktivieren von Defragmentierungsmetriken Ihre Workloads gründlich zu messen. Es folgt eine Kurzübersicht der zu beachtenden Aspekte:

| Vorteile der Defragmentierung | Nachteile der Defragmentierung |
| --- | --- |
| Ermöglicht eine schnellere Erstellung großer Dienste |Konzentriert die Last auf weniger Knoten, wodurch sich Konflikte vermehren |
| Ermöglicht eine langsamere Datenverschiebung während der Erstellung |Ausfälle können sich auf mehr Dienste auswirken und mehr Wechsel verursachen |
| Ermöglicht eine umfassende Beschreibung der Anforderungen und Freigabe von Speicherplatz |Komplexere allgemeine Konfiguration der Ressourcenverwaltung |

Sie können defragmentierte und normale Metriken im gleichen Cluster kombinieren. Der Clusterressourcen-Manager versucht, die Defragmentierungsmetriken so weit wie möglich zu konsolidieren und die anderen Metriken zu verteilen. Wenn keine Dienste vorhanden sind, die diese Metriken gemeinsam nutzen, können die Ergebnisse gut sein. Die genauen Ergebnisse richten sich nach der Anzahl der Lastenausgleichsmetriken im Vergleich mit der Anzahl und Gewichtung der Defragmentierungsmetriken, dem Maß an Überschneidung, der aktuellen Auslastung und anderen Faktoren. Sie werden ein wenig experimentieren müssen, um die exakte Konfiguration zu ermitteln, die Sie benötigen.

## <a name="configuring-defragmentation-metrics"></a>Konfigurieren von Defragmentierungsmetriken
Das Konfigurieren von Defragmentierungsmetriken ist eine globale Entscheidung im Cluster, und einzelne Metriken können für die Defragmentierung ausgewählt werden:

ClusterManifest.xml

```xml
<Section Name="DefragmentationMetrics">
    <Parameter Name="Disk" Value="true" />
    <Parameter Name="CPU" Value="false" />
</Section>
```

Über „ClusterConfig.json“ für eigenständige Bereitstellungen bzw. über „Template.json“ für in Azure gehostete Cluster:

```json
"fabricSettings": [
  {
    "name": "DefragmentationMetrics",
    "parameters": [
      {
          "name": "Disk",
          "value": "true"
      },
      {
          "name": "CPU",
          "value": "false"
      }
    ]
  }
]
```


## <a name="next-steps"></a>Nächste Schritte
* Der Clusterressourcen-Manager bietet zahlreiche Optionen zum Beschreiben des Clusters. Weitere Informationen hierzu finden Sie in diesem Artikel zum [Beschreiben eines Service Fabric-Clusters](service-fabric-cluster-resource-manager-cluster-description.md).
* Metriken bestimmen, wie der Clusterressourcen-Manager von Service Fabric den Ressourcenverbrauch und die Kapazität im Cluster verwaltet. Weitere Informationen zu Metriken und deren Konfiguration finden Sie in [diesem Artikel](service-fabric-cluster-resource-manager-metrics.md).

[Image1]:./media/service-fabric-cluster-resource-manager-defragmentation-metrics/balancing-defrag-compared.png



<!--HONumber=Jan17_HO1-->


