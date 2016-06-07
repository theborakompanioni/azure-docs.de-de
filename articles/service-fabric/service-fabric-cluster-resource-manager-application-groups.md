<properties
   pageTitle="Clusterressourcen-Manager von Service Fabric – Anwendungsgruppen | Microsoft Azure"
   description="Übersicht über die Anwendungsgruppenfunktionalität im Clusterressourcen-Manager von Service Fabric"
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
   ms.date="05/20/2016"
   ms.author="masnider"/>

# Einführung in Anwendungsgruppen
Der Clusterressourcen-Manager von Service Fabric verwaltet die Clusterressourcen in der Regel durch Verteilen der Last (über Metriken dargestellt) gleichmäßig im gesamten Cluster. Service Fabric verwaltet außerdem die Kapazität der Knoten im Cluster und den Cluster als Ganzes über das Konzept der Kapazität. Dies funktioniert hervorragend für eine Vielzahl verschiedener Arten von Workloads, aber die Muster, die intensiven Gebrauch von anderen Service Fabric-Anwendungsinstanzen machen, bringen manchmal zusätzliche Anforderungen ein. Einige zusätzlichen Anforderungen sind in der Regel:

- Möglichkeit zum Reservieren von Kapazität für Dienste einer Anwendungsinstanz auf einer bestimmten Anzahl von Knoten
- Möglichkeit zum Beschränken der Gesamtzahl der Knoten, die ein bestimmter Satz von Diensten in einer Anwendung ausführen darf
- Definition der Kapazitäten auf der Anwendungsinstanz selbst, um die Anzahl oder Summe der Ressourcennutzung der Dienste darin zu beschränken

Um diese Anforderungen zu erfüllen, entwickelten wir Unterstützung für das, was wir Anwendungsgruppen nennen.

## Verwalten der Anwendungskapazität
Anwendungskapazität kann verwendet werden, um die Anzahl der Knoten zu beschränken, über die sich eine Anwendung erstreckt, sowie die gesamte Metriklast der Instanzen der Anwendung auf den einzelnen Knoten. Sie kann auch verwendet werden, um im Cluster Ressourcen für die Anwendung zu reservieren.

Anwendungskapazität kann für neue Anwendungen festgelegt werden, wenn diese erstellt werden. Sie kann auch für vorhandene Anwendungen aktualisiert werden, die ohne Angabe der Anwendungskapazität erstellt wurden.

### Begrenzung der maximalen Anzahl von Knoten
Der einfachste Anwendungsfall für Anwendungskapazität ist, wenn eine Anwendungsinstanziierung auf eine bestimmte maximale Anzahl von Knoten beschränkt werden muss. Wenn keine Anwendungskapazität angegeben wird, instanziiert der Clusterressourcen-Manager von Service Fabric Replikate entsprechend den normalen Regeln (Ausgleich oder Defragmentierung), was in der Regel bedeutet, dass seine Dienste auf alle verfügbaren Knoten im Cluster verteilt werden, oder – wenn die Defragmentierung aktiviert ist – auf eine beliebige, jedoch kleinere Anzahl von Knoten.

Die folgende Abbildung zeigt die mögliche Platzierung einer Anwendungsinstanz ohne Definition der maximalen Anzahl von Knoten, und dann die gleiche Anwendung mit festgelegter maximaler Anzahl von Knoten. Beachten Sie, dass nicht garantiert wird, welche Replikate oder Instanzen welcher Dienste zusammen platziert werden.

![Anwendungsinstanzen – Definieren der maximalen Anzahl von Knoten][Image1]

Im linken Beispiel ist für die Anwendung keine Anwendungskapazität festgelegt, und sie verfügt über drei Dienste. Der Clusterressourcen-Manager hat eine logische Entscheidung getroffen, alle Replikate über sechs verfügbare Knoten zu verteilen, um das optimale Gleichgewicht im Cluster zu erreichen. Im rechten Beispiel sehen wir die gleiche Anwendung auf drei Knoten beschränkt, und dort hat der Clusterressourcen-Manager von Service Fabric das optimale Gleichgewicht für die Replikate der Dienste der Anwendung erreicht.

Der Parameter MaximumNodes steuert dieses Verhalten. Dieser Parameter kann während der Anwendungserstellung festgelegt oder für eine bereits ausgeführte Anwendungsinstanz aktualisiert werden – in diesem Fall beschränkt der Clusterressourcen-Manager von Service Fabric die Replikate der Anwendungsdienste auf die definierte maximale Anzahl von Knoten.

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

## Metriken, Last und Kapazität der Anwendung
Mit Anwendungsgruppen können Sie auch sowohl Metriken für eine bestimmte Anwendungsinstanz als auch die Kapazität der Anwendung im Hinblick auf diese Metriken definieren. Beispielsweise könnten Sie definieren, dass so viele Dienste wie Sie wünschen, erstellt werden können in

Für jede Metrik können 2 Werte festgelegt werden, um die Kapazität für diese Anwendungsinstanz zu beschreiben:

-	Gesamte Anwendungskapazität – Stellt die Gesamtkapazität der Anwendung für eine bestimmte Metrik dar. Der Clusterressourcen-Manager von Service Fabric versucht, die Summe der Metriklasten der Dienste dieser Anwendung auf den angegebenen Wert zu beschränken. Darüber hinaus lässt er – wenn die Auslastung durch die Dienste der Anwendung bereits diesen Grenzwert erreicht – die Erstellung von neuen Diensten oder Partitionen nicht zu, wenn die Gesamtlast damit diesen Grenzwert überschreiten würde.
-	Maximale Knotenkapazität – Gibt die maximale Gesamtlast für Replikate der Dienste der Anwendung auf einem einzelnen Knoten an. Wenn die Gesamtlast auf dem Knoten diese Kapazität überschreitet, versucht der Clusterressourcen-Manager von Service Fabric, Replikate auf andere Knoten zu verschieben, sodass die Einschränkung der Kapazität berücksichtigt wird.

## Reservieren von Kapazität
Eine weitere häufige Verwendung für Anwendungsgruppen ist, sicherzustellen, dass Ressourcen innerhalb des Clusters für eine bestimmte Anwendungsinstanz reserviert werden, selbst wenn die Anwendungsinstanz die Dienste noch nicht enthält, oder selbst dann, wenn die Ressourcen noch nicht genutzt werden. Wir betrachten nun die Funktionsweise.

### Angeben einer minimalen Anzahl von Knoten und Reservierung von Ressourcen
Reservieren von Ressourcen für eine Anwendungsinstanz erfordert die Angabe einiger zusätzlicher Parameter: *MinimumNodes* und *NodeReservationCapacity*.

- MinimumNodes – So wie Sie eine maximale Anzahl von Knoten angeben können, auf denen die Dienste innerhalb einer Anwendung ausgeführt werden, können Sie auch die minimale Anzahl von Knoten angeben, auf denen eine Anwendung ausgeführt werden soll. Diese Einstellung definiert die Anzahl der Knoten, auf denen die Ressourcen mindestens reserviert werden sollten, und gewährleistet damit beim Erstellen der Anwendungsinstanz die Kapazitätsbereitstellung im Cluster.
- NodeReservationCapacity - Die NodeReservationCapacity kann für jede Metrik innerhalb der Anwendung definiert werden. Damit wird definiert, welche Metriklast für die Anwendung auf einem beliebigen Knoten reserviert wird, auf dem die Replikate oder Instanzen der darin enthaltenen Dienste platziert werden.

Betrachten wir nun ein Beispiel der Kapazitätsreservierung:

![Anwendungsinstanzen – Definieren der reservierten Kapazität][Image2]

Im linken Beispiel ist für Anwendungen keine Anwendungskapazität definiert. Der Clusterressourcen-Manager von Service Fabric gleicht die Replikate und Instanzen der untergeordneten Dienste der Anwendung und solche anderer Dienste (außerhalb der Anwendung) aus, um das Gleichgewicht im Cluster sicherzustellen.

Wenn im Beispiel auf der rechten Seite beim Erstellen der Anwendung MinimumNodes auf 2 festgelegt wurde, MaximumNodes auf 3 und eine Anwendungsmetrik mit einer Reservierung von 20, einer Maximalkapazität auf einem Knoten von 50 und einer gesamten Anwendungskapazität von 100 definiert wurde, reserviert Service Fabric auf zwei Knoten Kapazität für die blaue Anwendung und lässt nicht zu, dass andere Replikate im Cluster diese Kapazität verbrauchen. Diese reservierte Anwendungskapazität wird als verbraucht betrachtet und von der verfügbaren Clusterkapazität abgezogen.

Wenn eine Anwendung mit Reservierung erstellt wird, reserviert der Clusterressourcen-Manager im Cluster Kapazität gemäß der Formel „MinimumNodes * NodeReservationCapacity“, jedoch erst dann Kapazität auf bestimmten Knoten, wenn die Replikate der Dienste der Anwendung erstellt und platziert werden. Dies ermöglicht Flexibilität, da Knoten erst dann für neue Replikate ausgewählt werden, wenn diese erstellt werden. Kapazität wird auf einem bestimmten Knoten reserviert, wenn mindestens ein Replikat darauf platziert wird.

## Abrufen von Anwendungslastinformationen
Für jede Anwendung, für die Anwendungskapazität definiert wurde, können Sie Informationen über die aggregierte Last abrufen, die von Replikaten ihrer Dienste gemeldet werden. Für diesen Zweck können Sie in Service Fabric PowerShell- und Managed API-Abfragen durchführen.

Beispielsweise können Sie die Last mit folgendem PowerShell-Cmdlet abrufen:

``` posh
Get-ServiceFabricApplicationLoad –ApplicationName fabric:/MyApplication1

```

Die Ausgabe dieser Abfrage enthält die grundlegenden Informationen zur Anwendungskapazität, die für die Anwendung angegeben wurde, z.B. die minimale und maximale Anzahl Knoten. Sie erhalten ebenfalls Informationen über die Anzahl der Knoten, die die Anwendung derzeit verwendet. Folglich erhalten Sie für jede Lastmetrik Informationen zu:
- Metrikname: Name der Metrik.
-	Reservierungskapazität: für diese Anwendung reservierte Clusterkapazität.
-	Anwendungslast: Gesamtlast der untergeordneten Replikate dieser Anwendung.
-	Anwendungskapazität: maximal zulässiger Wert der Anwendungslast.

## Entfernen der Anwendungskapazität
Sobald die Anwendungskapazitätsparameter für eine Anwendung festgelegt sind, können sie mit den APIs zur Anwendungsaktualisierung oder PowerShell-Cmdlets entfernt werden. Zum Beispiel:

``` posh
Update-ServiceFabricApplication –Name fabric:/MyApplication1 –RemoveApplicationCapacity

```

Dieser Befehl entfernt alle Anwendungskapazitätsparameter aus der Anwendung, und der Clusterressourcen-Manager von Service Fabric behandelt diese Anwendung jetzt wie jede andere Anwendung im Cluster, für die diese Parameter nicht definiert sind. Der Befehl ist sofort wirksam, und der Clusterressourcen-Manager löscht alle Anwendungskapazitätsparameter für diese Anwendung. Um sie erneut anzugeben, müssen Sie APIs zur Anwendungsaktualisierung mit den entsprechenden Parametern aufrufen.

## Einschränkungen der Anwendungskapazität
Es gibt mehrere Einschränkungen für Anwendungskapazitätsparameter, die berücksichtigt werden müssen. Bei Prüfungsfehlern wird die Erstellung oder Aktualisierung der Anwendung mit einer Fehlermeldung abgelehnt. Alle Integerparameter müssen positive Zahlen sein. Darüber hinaus gelten für einzelne Parameter folgende Einschränkungen:

-	MinimumNodes darf nie größer sein als MaximumNodes.
-	Wenn Kapazitäten für eine Lastmetrik definiert sind, müssen sie die folgenden Regeln einhalten:
  - Die Knotenreservierungskapazität darf nie größer sein als die maximale Knotenkapazität. Sie können z.B. nicht die Kapazität für die Metrik „CPU“ auf dem Knoten auf 2 Einheiten beschränken und versuchen, 3 Einheiten auf jedem Knoten zu reservieren.
  - Wenn MaximumNodes angegeben wird, darf das Produkt aus MaximumNodes und maximaler Knotenkapazität nicht größer sein als die Gesamtkapazität der Anwendung. Wenn Sie beispielsweise für die Lastmetrik „CPU“ für die maximale Knotenkapazität 8 und für die maximale Anzahl von Knoten 10 festlegen, muss die Gesamtkapazität der Anwendung für diese Lastmetrik größer als 80 sein.

Die Einschränkungen werden sowohl während der Anwendungserstellung (auf der Clientseite) als auch während der Anwendungsaktualisierung (auf der Serverseite) erzwungen. Dies ist ein Beispiel für einen klaren Verstoß gegen die Anforderungen während der Erstellung, da MaximumNodes < MinimumNodes ist, und bei der Ausführung des Befehls im Client tritt ein Fehler auf, noch bevor die Anforderung an den Service Fabric-Cluster gesendet wird:

``` posh
New-ServiceFabricApplication –Name fabric:/MyApplication1 –MinimumNodes 6 –MaximumNodes 2
```

Hier folgt ein Beispiel für ungültige Aktualisierung. Wenn wir in einer bestehenden Anwendung die maximale Anzahl von Knoten auf einen anderen Wert aktualisieren, wird die Aktualisierung durchgeführt:

``` posh
Update-ServiceFabricApplication –Name fabric:/MyApplication1 6 –MaximumNodes 2
```

Danach können wir versuchen, die minimale Anzahl von Knoten zu aktualisieren:

``` posh
Update-ServiceFabricApplication –Name fabric:/MyApplication1 6 –MinimumNodes 6
```

Der Client besitzt nicht genügend Kontextinformationen über die Anwendung, darum übergibt er das Update an den Service Fabric-Cluster. Allerdings überprüft Service Fabric im Cluster den neuen Parameter anhand der vorhandenen Parameter und lässt das Update nicht zu, da der Wert für die minimale Anzahl von Knoten größer ist als der Wert für die maximale Anzahl. In diesem Fall bleiben die Anwendungskapazitätsparameter unverändert.

Diese Einschränkungen gelten, damit der Clusterressourcen-Manager die optimale Platzierung für Replikate der Anwendungsdienste durchführen kann.

## Wie Sie die Anwendungskapazität nicht verwenden sollten

-	Verwenden Sie die Anwendungskapazität nicht, um die Anwendung auf eine bestimmte Teilmenge von Knoten einzuschränken: Obwohl Service Fabric sicherstellt, dass die maximale Anzahl von Knoten für jede Anwendung berücksichtigt wird, für die Anwendungskapazität angegeben ist, können Benutzer nicht entscheiden, auf welchen Knoten sie instanziiert wird. Dies kann mit Platzierungseinschränkungen für Dienste erreicht werden.
-	Verwenden Sie die Anwendungskapazität nicht, um sicherzustellen, dass zwei Dienste aus der gleichen Anwendung immer nebeneinander platziert werden. Dies kann über die Affinitätsbeziehung zwischen Diensten erreicht werden, und Affinität kann ausschließlich auf die Dienste beschränkt werden, die tatsächlich zusammen platziert werden sollten.

## Nächste Schritte
- Weitere Informationen zu den anderen Optionen, die für die Konfiguration von Diensten zur Verfügung stehen, finden Sie im Thema zu den anderen verfügbaren Clusterressourcen-Manager-Konfigurationen, [Konfigurieren von Diensten](service-fabric-cluster-resource-manager-configure-services.md).
- Informationen darüber, wie der Clusterressourcen-Manager die Auslastung im Cluster verwaltet und verteilt, finden Sie im Artikel zum [Lastenausgleich](service-fabric-cluster-resource-manager-balancing.md).
- Starten Sie mit einer [Einführung in den Clusterressourcen-Manager von Service Fabric](service-fabric-cluster-resource-manager-introduction.md).
- Weitere Informationen zur Funktionsweise von Metriken im Allgemeinen finden Sie unter [Verwalten von Ressourcenverbrauch und Auslastung in Service Fabric mit Metriken](service-fabric-cluster-resource-manager-metrics.md).
- Der Clusterressourcen-Manager bietet viele Optionen für die Beschreibung des Clusters. Weitere Informationen hierzu finden Sie in diesem Artikel zum [Beschreiben eines Service Fabric-Clusters](service-fabric-cluster-resource-manager-cluster-description.md).


[Image1]: ./media/service-fabric-cluster-resource-manager-application-groups/application-groups-max-nodes.png
[Image2]: ./media/service-fabric-cluster-resource-manager-application-groups/application-groups-reserved-capacity.png

<!---HONumber=AcomDC_0525_2016-->