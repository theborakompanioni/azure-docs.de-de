---
title: "Clusterressourcen-Manager von Service Fabric – Anwendungsgruppen | Microsoft Docs"
description: "Übersicht über die Anwendungsgruppenfunktionalität im Clusterressourcen-Manager von Service Fabric"
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 4cae2370-77b3-49ce-bf40-030400c4260d
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/05/2017
ms.author: masnider
translationtype: Human Translation
ms.sourcegitcommit: eef9cbca0479801cbac53ae1ae5713d4dac6a4bf
ms.openlocfilehash: 57525cd41d518b12f3f465d5dea7247659e3c8b6


---
# <a name="introduction-to-application-groups"></a>Einführung in Anwendungsgruppen
Der Clusterressourcen-Manager von Service Fabric verwaltet die Clusterressourcen in der Regel durch gleichmäßiges Verteilen der Last (durch [Metriken](service-fabric-cluster-resource-manager-metrics.md) dargestellt) über den gesamten Cluster. Service Fabric verwaltet außerdem die Kapazität der Knoten im Cluster und den Cluster als Ganzes über das Konzept der [Kapazität](service-fabric-cluster-resource-manager-cluster-description.md). Metriken und Kapazität funktionieren hervorragend für eine Vielzahl von Workloads, aber die Muster, die intensiven Gebrauch von anderen Service Fabric-Anwendungsinstanzen machen, bringen manchmal zusätzliche Anforderungen ein. Einige zusätzlichen Anforderungen sind in der Regel:

* Möglichkeit zum Reservieren von Kapazität für Dienste einer Anwendungsinstanz im Cluster
* Möglichkeit zum Beschränken der Gesamtzahl der Knoten, auf denen die Dienste innerhalb einer Anwendung ausgeführt werden
* Definition der Kapazitäten in der Anwendungsinstanz selbst, um die Anzahl oder den Gesamtressourcenverbrauch der Dienste darin zu beschränken

Um diese Anforderungen zu erfüllen, werden Anwendungsgruppen vom Service Fabric-Clusterressourcen-Manager unterstützt.

## <a name="managing-application-capacity"></a>Verwalten der Anwendungskapazität
Mithilfe der Anwendungskapazität kann die Anzahl von Knoten eingeschränkt werden, über die sich eine Anwendung erstreckt. Sie kann auch die Metriklast der Anwendungsdienste auf einzelnen Knoten und insgesamt einschränken. Sie kann auch verwendet werden, um im Cluster Ressourcen für die Anwendung zu reservieren.

Anwendungskapazität kann für neue Anwendungen festgelegt werden, wenn diese erstellt werden. Sie kann auch für vorhandene Anwendungen aktualisiert werden.

### <a name="limiting-the-maximum-number-of-nodes"></a>Begrenzung der maximalen Anzahl von Knoten
Der einfachste Anwendungsfall für Anwendungskapazität ist, wenn eine Anwendungsinstanziierung auf eine bestimmte maximale Anzahl von Knoten beschränkt werden muss. Wenn keine Anwendungskapazität angegeben wird, erstellt und platziert der Service Fabric-Clusterressourcen-Manager Dienste entsprechend den üblichen Regeln (Lastenausgleich oder Defragmentierung).

Die folgende Abbildung zeigt eine Anwendungsinstanz mit und ohne eine definierte maximale Anzahl von Knoten. Es gibt keine Garantie im Hinblick darauf, welche Replikate oder Instanzen welcher Dienste zusammen platziert oder welche Knoten verwendet werden.

<center>
![Anwendungsinstanzen – Definieren der maximalen Anzahl von Knoten][Image1]
</center>

Im linken Beispiel ist für die Anwendung keine Anwendungskapazität festgelegt, und sie verfügt über drei Dienste. Der Clusterressourcen-Manager hat alle Replikate über sechs verfügbare Knoten verteilt, um das optimale Gleichgewicht im Cluster zu erreichen. Im rechten Beispiel sehen wir die gleiche Anwendung, die auf drei Knoten beschränkt wurde.

Der Parameter MaximumNodes steuert dieses Verhalten. Dieser Parameter kann während der Anwendungserstellung festgelegt oder für eine bereits ausgeführte Anwendungsinstanz aktualisiert werden.

PowerShell

``` posh
New-ServiceFabricApplication -ApplicationName fabric:/AppName -ApplicationTypeName AppType1 -ApplicationTypeVersion 1.0.0.0 -MaximumNodes 3
Update-ServiceFabricApplication –Name fabric:/AppName –MaximumNodes 5
```

C#

``` csharp
ApplicationDescription ad = new ApplicationDescription();
ad.ApplicationName = new Uri("fabric:/AppName");
ad.ApplicationTypeName = "AppType1";
ad.ApplicationTypeVersion = "1.0.0.0";
ad.MaximumNodes = 3;
fc.ApplicationManager.CreateApplicationAsync(ad);

ApplicationUpdateDescription adUpdate = new ApplicationUpdateDescription(new Uri("fabric:/AppName"));
adUpdate.MaximumNodes = 5;
fc.ApplicationManager.UpdateApplicationAsync(adUpdate);

var appMetric = new ApplicationMetricDescription();
appMetric.Name = "Metric1";
appMetric.TotalApplicationCapacity = 1000;

adUpdate.Metrics.Add(appMetric);
```

## <a name="application-metrics-load-and-capacity"></a>Metriken, Last und Kapazität der Anwendung
Mit Anwendungsgruppen können Sie auch Metriken für eine bestimmte Anwendungsinstanz sowie die Anwendungskapazität für diese Metriken definieren. So können Sie den Ressourcenverbrauch der Dienste innerhalb dieser Anwendungsinstanz verfolgen, reservieren und begrenzen.

Für jede Anwendungsmetrik gibt es zwei Werte, die festgelegt werden können:

* **Gesamte Anwendungskapazität**: Diese Einstellung stellt die Gesamtkapazität der Anwendung für eine bestimmte Metrik dar. Der Clusterressourcen-Manager lässt die Erstellung neuer Dienste innerhalb dieser Anwendungsinstanz nicht zu, wenn die Gesamtlast dadurch diesen Wert überschreiten würde. Nehmen wir z.B. an, die Anwendungsinstanz weist eine Kapazität von zehn und bereits eine Last von fünf auf. In diesem Fall wäre die Erstellung eines Diensts mit einer Standardgesamtlast von 10 nicht zulässig.
* **Maximale Knotenkapazität**: Diese Einstellung gibt die maximale Gesamtlast für Replikate der Dienste innerhalb der Anwendung auf einem einzelnen Knoten an. Wenn die Gesamtlast auf dem Knoten diese Kapazität überschreitet, versucht der Clusterressourcen-Manager, Replikate auf andere Knoten zu verschieben, sodass die Einschränkung der Kapazität berücksichtigt wird.

## <a name="reserving-capacity"></a>Reservieren von Kapazität
Ein weiterer üblicher Verwendungszweck für Anwendungsgruppen besteht darin sicherzustellen, dass Ressourcen innerhalb des Clusters für eine bestimmte Anwendungsinstanz reserviert werden. Dies geschieht selbst dann, wenn die Anwendungsinstanz noch keine Dienste enthält oder wenn diese die Ressourcen noch nicht nutzen. Sehen wir uns nun die Funktionsweise an.

### <a name="specifying-a-minimum-number-of-nodes-and-resource-reservation"></a>Angeben einer minimalen Anzahl von Knoten und Reservierung von Ressourcen
Das Reservieren von Ressourcen für eine Anwendungsinstanz erfordert die Angabe einiger zusätzlicher Parameter: *MinimumNodes* und *NodeReservationCapacity*.

* **MinimumNodes**: So wie Sie eine maximale Anzahl von Knoten angeben können, auf denen die Dienste innerhalb einer Anwendung ausgeführt werden, können Sie auch die Mindestanzahl von Knoten angeben. Diese Einstellung definiert die Anzahl von Knoten, auf denen die Ressourcen reserviert werden sollen, und gewährleistet damit beim Erstellen der Anwendungsinstanz die Kapazitätsbereitstellung im Cluster.
* **NodeReservationCapacity**: Die NodeReservationCapacity kann für jede Metrik innerhalb der Anwendung definiert werden. Mit dieser Einstellung wird definiert, welche Metriklast für die Anwendung auf einem beliebigen Knoten reserviert wird, auf dem die Replikate oder Instanzen der darin enthaltenen Dienste platziert werden.

Betrachten wir nun ein Beispiel für die Kapazitätsreservierung:

<center>
![Anwendungsinstanzen – Definieren der reservierten Kapazität][Image2]
</center>

Im linken Beispiel ist für Anwendungen keine Anwendungskapazität definiert. Der Clusterressourcen-Manager gleicht alle Komponenten den normalen Regeln entsprechend aus.

Im Beispiel auf der rechten Seite gehen wir davon aus, dass die Anwendung mit den folgenden Einstellungen erstellt wurde:

* Festlegung von MinimumNodes auf zwei
* Festlegung von MaximumNodes auf drei
* Definition einer Anwendungsmetrik mit
  * NodeReservationCapacity von 20
  * MaximumNodeCapacity von 50
  * TotalApplicationCapacity von 100

Service Fabric reserviert für die blaue Anwendung Kapazität auf zwei Knoten und lässt nicht zu, dass Dienste von anderen Anwendungsinstanzen im Cluster diese Kapazität nutzen. Diese reservierte Anwendungskapazität wird als verbraucht betrachtet und von der verbleibenden Kapazität auf diesem Knoten und innerhalb des Clusters abgezogen.

Beim Erstellen einer Anwendung mit Reservierung wird eine Kapazität vom Clusterressourcen-Manager reserviert, die der Gleichung MinimumNodes * NodeReservationCapacity entspricht (für jede Metrik). Die Kapazität wird jedoch nur dann auf einem bestimmten Knoten reserviert, wenn mindestens ein Replikat darauf platziert wird. Diese spätere Reservierung ermöglicht Flexibilität und eine bessere Ressourcennutzung, da Ressourcen nur bei Bedarf auf Knoten reserviert werden.

## <a name="obtaining-the-application-load-information"></a>Abrufen von Anwendungslastinformationen
Für jede Anwendung, für die Anwendungskapazität definiert wurde, können Sie Informationen über die aggregierte Last abrufen, die von Replikaten ihrer Dienste gemeldet werden.

Beispielsweise können Sie die Last mit folgendem PowerShell-Cmdlet abrufen:

``` posh
Get-ServiceFabricApplicationLoad –ApplicationName fabric:/MyApplication1

```

Die ApplicationLoad-Abfrage gibt grundlegende Informationen zu der Anwendungskapazität zurück, die für die Anwendung angegeben wurde. Zu diesen Informationen gehören die Mindest- und Höchstanzahl von Knoten und die Anzahl, die derzeit von der Anwendung belegt wird. Zudem sind darin Informationen zu den einzelnen Anwendungslastmetriken enthalten:

* Metrikname: Name der Metrik.
* Reservierungskapazität: für diese Anwendung reservierte Clusterkapazität.
* Anwendungslast: Gesamtlast der untergeordneten Replikate dieser Anwendung.
* Anwendungskapazität: maximal zulässiger Wert der Anwendungslast.

## <a name="removing-application-capacity"></a>Entfernen der Anwendungskapazität
Sobald die Anwendungskapazitätsparameter für eine Anwendung festgelegt sind, können sie mit den APIs zur Anwendungsaktualisierung oder PowerShell-Cmdlets entfernt werden. Beispiel:

``` posh
Update-ServiceFabricApplication –Name fabric:/MyApplication1 –RemoveApplicationCapacity

```

Dieser Befehl entfernt alle Parameter zur Anwendungskapazität aus der Anwendung. Der Befehl zeigt eine sofortige Wirkung. Nach Abschluss dieses Befehls kehrt der Clusterressourcen-Manager zum Standardverhalten für die Verwaltung von Anwendungen zurück. Parameter zur Anwendungskapazität können über Update-ServiceFabricApplication erneut angegeben werden.

### <a name="restrictions-on-application-capacity"></a>Einschränkungen der Anwendungskapazität 
Es gibt mehrere Einschränkungen für Anwendungskapazitätsparameter, die berücksichtigt werden müssen. Bei Prüfungsfehlern wird die Erstellung oder Aktualisierung der Anwendung mit einer Fehlermeldung abgelehnt, und es werden keine Änderungen vorgenommen.

* Alle Integerparameter müssen positive Zahlen sein.
* MinimumNodes darf nie größer sein als MaximumNodes.
* Wenn Kapazitäten für eine Lastmetrik definiert sind, müssen sie die folgenden Regeln einhalten:
  * Die Knotenreservierungskapazität darf nie größer sein als die maximale Knotenkapazität. Sie können z.B. nicht die Kapazität für die Metrik „CPU“ auf dem Knoten auf zwei Einheiten beschränken und versuchen, drei Einheiten auf jedem Knoten zu reservieren.
  * Wenn MaximumNodes angegeben wird, darf das Produkt aus MaximumNodes und maximaler Knotenkapazität nicht größer sein als die Gesamtkapazität der Anwendung. Nehmen wir beispielsweise an, dass die maximale Knotenkapazität für die Lastmetrik „CPU“ auf acht festgelegt ist. Außerdem nehmen wir an, dass Sie die maximale Anzahl von Knoten auf zehn festlegen. In diesem Fall muss die Gesamtanwendungskapazität für diese Lastmetrik größer als 80 sein.

Die Einschränkungen werden sowohl während der Anwendungserstellung (auf der Clientseite) als auch während der Anwendungsaktualisierung (auf der Serverseite) erzwungen.

## <a name="how-not-to-use-application-capacity"></a>Wie Sie die Anwendungskapazität nicht verwenden sollten
* Versuchen Sie nicht, die Anwendungsgruppenfunktionen zum Einschränken der Anwendung auf eine _bestimmte_ Teilmenge von Knoten zu verwenden. Das heißt, Sie können angeben, dass die Anwendung auf höchstens fünf Knoten ausgeführt wird, aber nicht, auf welchen fünf Knoten im Cluster. Das Einschränken einer Anwendung auf bestimmte Knoten kann mithilfe von Platzierungseinschränkungen für Dienste erreicht werden.
* Verwenden Sie die Anwendungskapazität nicht, um sicherzustellen, dass zwei Dienste aus der gleichen Anwendung nebeneinander platziert werden. Mithilfe von Affinität oder Platzierungseinschränkungen können Sie je nach den jeweiligen Anforderungen sicherstellen, dass Dienste auf demselben Knoten ausgeführt werden.

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zu den anderen Optionen, die für die Konfiguration von Diensten zur Verfügung stehen, finden Sie im Thema zu den anderen verfügbaren Clusterressourcen-Manager-Konfigurationen unter [Konfigurieren von Diensten](service-fabric-cluster-resource-manager-configure-services.md).
* Informationen darüber, wie der Clusterressourcen-Manager die Auslastung im Cluster verwaltet und verteilt, finden Sie im Artikel zum [Lastenausgleich](service-fabric-cluster-resource-manager-balancing.md)
* Starten Sie mit einer [Einführung in den Clusterressourcen-Manager von Service Fabric](service-fabric-cluster-resource-manager-introduction.md)
* Weitere Informationen zur Funktionsweise von Metriken im Allgemeinen finden Sie unter [Verwalten von Ressourcenverbrauch und Auslastung in Service Fabric mit Metriken](service-fabric-cluster-resource-manager-metrics.md)
* Der Clusterressourcen-Manager bietet zahlreiche Optionen zum Beschreiben des Clusters. Weitere Informationen hierzu finden Sie in diesem Artikel zum [Beschreiben eines Service Fabric-Clusters](service-fabric-cluster-resource-manager-cluster-description.md).

[Image1]:./media/service-fabric-cluster-resource-manager-application-groups/application-groups-max-nodes.png
[Image2]:./media/service-fabric-cluster-resource-manager-application-groups/application-groups-reserved-capacity.png



<!--HONumber=Jan17_HO1-->


