<properties
   pageTitle="Defragmentierung von Metriken in Azure Service Fabric | Microsoft Azure"
   description="Eine Übersicht über den Einsatz von Defragmentierung oder Überlastung als Strategie für Metriken in Service Fabric"
   services="service-fabric"
   documentationCenter=".net"
   authors="masnider"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/19/2016"
   ms.author="masnider"/>

# Defragmentierung von Metriken und Lasten in Service Fabric
Der Clusterressourcen-Manager von Service Fabric wird hauptsächlich für den Lastenausgleich in Bezug auf die Verteilung der Last eingesetzt und stellt sicher, dass alle Knoten im Cluster in gleichem Umfang genutzt werden. Dies ist in der Regel das sicherste und intelligenteste Konzept im Hinblick auf das Überstehen von Ausfällen, da es sicherstellt, dass bei einem Ausfall nicht ein hoher Prozentsatz einer Workload verloren geht. Der Clusterressourcen-Manager von Service Fabric unterstützt auch eine andere Strategie, die Defragmentierung genannt wird. Defragmentierung bedeutet im Allgemeinen, dass anstatt zu versuchen, die Nutzung einer Metrik auf den Cluster zu verteilen, wir tatsächlich versuchen sollten, sie zu konsolidieren. Das ist eine Umkehrung unserer normalen Strategie – anstatt den Cluster basierend auf einer möglichst minimalen Standardabweichung der Metriklast für eine bestimmte Metrik zu optimieren, beginnen wir damit, im Hinblick auf eine größere Abweichung zu optimieren. Doch was spricht für eine solche Strategie?

Wenn Sie die Last gleichmäßig auf die Knoten im Cluster verteilen, haben Sie einige der Ressourcen belegt, die die Knoten zu bieten haben. Normalerweise ist das kein Problem, aber manchmal erstellen Workloads außergewöhnlich umfangreiche Dienste, die den Großteil der Kapazität eines Knotens in Anspruch nehmen, sodass z.B. 75–95% der Ressourcen eines Knotens einer einzelnen Dienstinstanz oder einem einzelnen Dienstreplikat fest zugeordnet sein können. Auch das ist im Grunde kein Problem. Der Clusterressourcen-Manager erkennt beim Erstellen des Diensts, dass der Cluster reorganisiert werden muss, um Platz für diese umfangreiche Workload zu schaffen, und beginnt mit der Reorganisation. In der Zwischenzeit muss die Workload allerdings warten, bis sie im Cluster geplant wird.

Da bei der Planung neuer Workloads meist ein wenig Wartezeit anfällt, können wir, wenn wir nichts verändern, zuweilen die dazugehörigen SLAs unterschreiten, sobald viele Dienste und Zustandsdaten verschoben werden müssen. Dies gilt insbesondere dann, wenn die Workloads im Cluster im Allgemeinen umfangreich sind (und das Verschieben im Cluster daher sowieso schon länger dauert). Beim Messen der Erstellungszeiten in auf tatsächlichen Clusterdaten basierenden Simulationen haben wir festgestellt, dass sich die Erstellung großer Dienste verlangsamte, wenn die Dienste umfangreich waren und der Cluster bereits gut ausgelastet war. Diese Situation lässt sich durch Richtlinien für Defragmentierungsmetriken verbessern.

Dateierstellung und -zugriff können sich verlangsamen, wenn die Festplatte eines Computers fragmentiert ist. Durch eine Defragmentierung, die größere zusammenhängende Datenblöcke verfügbar macht, lässt sich die Leistung wieder beschleunigen. Auf die gleiche Weise können Sie Defragmentierungsmetriken konfigurieren, sodass der Clusterressourcen-Manager proaktiv versucht, die Last der Dienste auf weniger Knoten zu komprimieren, damit (fast) immer genügend Platz für sehr umfangreiche Dienste vorhanden ist und diese schnell erstellt werden können. In den meisten Fällen ist dies nicht erforderlich, da Dienste zumeist nicht sehr umfangreich sind und es daher nicht besonders schwierig ist, sie unterzubringen. Wenn Sie jedoch umfangreiche Dienste schnell bereitstellen möchten (und einige Nachteile in Kauf nehmen, wie z.B. die höheren Auswirkungen von Ausfällen und nicht ausgelastete Ressourcen, die darauf warten, dass Workloads geplant werden), ist die Defragmentierungsstrategie gut für Sie geeignet.

Das folgende Diagramm ist eine visuelle Darstellung von zwei verschiedenen Clustern – einer ist defragmentiert, der andere nicht. Berücksichtigen Sie im Ausgleichsfall die Verschiebungen, die erforderlich wären, um eines der größten Dienstobjekte zu platzieren, wenn ein neues erstellt werden müsste, im Vergleich zum defragmentierten Cluster, in dem es sofort auf Knoten 4 oder 5 platziert werden könnte.

![Vergleich zwischen ausgeglichenen und defragmentierten Clustern][Image1]

## Vor- und Nachteile der Defragmentierung
Was sind die anderen grundlegenden Kompromisse? Es wird empfohlen, vor dem Aktivieren von Defragmentierungsmetriken Ihre Workloads gründlich zu messen. Es folgt eine Kurzübersicht der zu beachtenden Aspekte:

| Vorteile der Defragmentierung | Nachteile der Defragmentierung |
|----------------------|----------------------|
|Ermöglicht eine schnellere Erstellung großer Dienste |	Konzentriert die Last auf weniger Knoten, wodurch sich Konflikte vermehren
|Ermöglicht eine langsamere Datenverschiebung während der Erstellung | Ausfälle können sich auf mehr Dienste auswirken und mehr Wechsel verursachen
|Ermöglicht eine umfassende Beschreibung der Anforderungen und Freigabe von Speicherplatz |	Komplexere allgemeine Konfiguration der Ressourcenverwaltung

Sie können defragmentierte und normale Metriken im selben Cluster kombinieren. Der Ressourcen-Manager versucht sein Möglichstes, Ihnen ein Layout zu bieten, dass die Defragmentierungsmetriken umfassend konsolidiert, während versucht wird, den Rest zu verteilen. Die genauen Ergebnisse, die Sie erzielen, richten sich nach der Anzahl der Lastenausgleichsmetriken im Vergleich mit der Anzahl und Gewichtung der Defragmentierungsmetriken, der aktuellen Auslastung usw.

## Konfigurieren von Defragmentierungsmetriken
Das Konfigurieren von Defragmentierungsmetriken ist eine globale Entscheidung im Cluster, und einzelne Metriken können für die Defragmentierung ausgewählt werden:

ClusterManifest.xml

```xml
<Section Name="DefragmentationMetrics">
    <Parameter Name="Disk" Value="true" />
    <Parameter Name="CPU" Value="false" />
</Section>
```

## Nächste Schritte
- Der Clusterressourcen-Manager bietet viele Optionen für die Beschreibung des Clusters. Weitere Informationen hierzu finden Sie in diesem Artikel zum [Beschreiben eines Service Fabric-Clusters](service-fabric-cluster-resource-manager-cluster-description.md).
- Metriken bestimmen, wie der Clusterressourcen-Manager von Service Fabric den Ressourcenverbrauch und die Kapazität im Cluster verwaltet. Weitere Informationen zu Metriken und deren Konfiguration finden Sie in [diesem Artikel](service-fabric-cluster-resource-manager-metrics.md).

[Image1]: ./media/service-fabric-cluster-resource-manager-defragmentation-metrics/balancing-defrag-compared.png

<!---HONumber=AcomDC_0824_2016-->