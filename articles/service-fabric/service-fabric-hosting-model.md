---
title: Azure Service Fabric-Hostingmodell | Microsoft-Dokumentation
description: Beschreibt die Beziehung zwischen Replikaten (oder Instanzen) eines bereitgestellten Service Fabric-Diensts und dem service-host-Prozess.
services: service-fabric
documentationcenter: .net
author: harahma
manager: timlt
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/15/2017
ms.author: harahma
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: ca7092a06a9ffce8383ca8bc9f70ce312cdf9de4
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
# <a name="service-fabric-hosting-model"></a>Service Fabric-Hostingmodell
Dieser Artikel bietet einen Überblick über von Service Fabric bereitgestellte Anwendungshostingmodelle, und beschreibt die Unterschiede zwischen dem Modell mit einem **gemeinsam genutzten Prozess** und dem Modell mit einem **exklusiven Prozess**. Er beschreibt die Darstellung einer bereitgestellten Anwendung auf einem Service Fabric-Knoten und die Beziehung zwischen Replikaten (oder Instanzen) des Diensts und dem service-host-Prozess.

Bevor Sie fortfahren, sollten Sie mit dem [Service Fabric-Anwendungsmodell][a1] vertraut sein und die verschiedenen Konzepte und Beziehungen zwischen diesen kennen. 

> [!NOTE]
> In diesem Artikel gilt (sofern nicht explizit erwähnt) aus Gründen der Einfachheit Folgendes:
>
> - Das Wort *Replikat* bezieht sich auf ein Replikat eines zustandsbehafteten Diensts oder eine Instanz eines zustandslosen Diensts.
> - *CodePackage* wird gleichbedeutend mit dem Prozess *ServiceHost* verwendet, der einen *ServiceType* registriert und Replikate von Diensten dieses *ServiceType* hostet.
>

Um das Hostingmodell zu verstehen, betrachten wir ein Beispiel im Detail. Angenommen, wir verfügen über einen *ApplicationType* „MyAppType“ mit einem *ServiceType* „MyServiceType“, der vom *ServicePackage* „MyServicePackage“ mit dem *CodePackage* „MyCodePackage“ bereitgestellt wird, das den *ServiceType* „MyServiceType“ bei der Ausführung registriert.

Weiterhin angenommen, wir verfügen über einen Cluster mit 3 Knoten und erstellen eine *Anwendung* **fabric:/App1** vom Typ „MyAppType“. In dieser *Anwendung* **fabric:/App1** erstellen wir einen Dienst **fabric:/App1/ServiceA** vom Typ „MyServiceType“ mit 2 Partitionen (**P1** & **P2**) und 3 Replikaten pro Partition. Das folgende Diagramm zeigt die Sicht dieser Anwendung, wie sie auf einem Knoten bereitgestellt wird.

<center>
![Sicht der bereitgestellten Anwendung auf dem Knoten][node-view-one]
</center>

Von Service Fabric wurde „MyServicePackage“ aktiviert, von dem „MyCodePackage“ gestartet wurde, das Replikate aus beiden Partitionen (**P1** & **P2**) hostet. Beachten Sie, dass alle Knoten im Cluster dieselbe Sicht aufweisen, da eine Anzahl von Replikaten pro Partition ausgewählt wurde, die der Anzahl der Knoten im Cluster entspricht. Erstellen wir in der Anwendung **fabric:/App1** nun einen weiteren Dienst **fabric:/App1/ServiceB** mit einer Partition (**P3**) und 3 Replikaten pro Partition. Das folgende Diagramm zeigt die neue Sicht auf dem Knoten:

<center>
![Sicht der bereitgestellten Anwendung auf dem Knoten][node-view-two]
</center>

Wie Sie sehen, hat Service Fabric das neue Replikat für die Partition **P3** des Diensts **fabric:/App1/ServiceB** in die vorhandene Aktivierung von „MyServicePackage“ platziert. Erstellen Sie nun eine weitere *Anwendung* (**fabric:/App2**) vom Typ „MyAppType“, und erstellen Sie in **fabric:/App2** den Dienst **fabric:/App2/ServiceA** mit 2 Partitionen (**P4** & **P5**) und 3 Replikaten pro Partition. Die folgenden Diagramme zeigen die neue Knotensicht:

<center>
![Sicht der bereitgestellten Anwendung auf dem Knoten][node-view-three]
</center>

Dieses Mal hat Service Fabric eine neue Kopie von „MyServicePackage“ aktiviert, die eine neue Kopie von „MyCodePackage“ startet, und Replikate von beiden Partitionen des Diensts **fabric:/App2/ServiceA** (d. h.**P4** & **P5**) werden in diese neue „MyCodePackage“-Kopie platziert.

## <a name="shared-process-model"></a>Modell mit einem gemeinsam genutzten Prozess
Im oben stehenden Beispiel wird das von Service Fabric bereitgestellte Standardhostingmodell dargestellt, das als Modell mit einem **gemeinsam genutzten Prozess** bezeichnet wird. In diesem Modell wird für eine bestimmte *Anwendung* nur eine Kopie eines bestimmten *ServicePackage* auf einem *Knoten* aktiviert (der alle darin enthaltenen *CodePackages* startet), und alle Replikate aller Dienste eines bestimmten *ServiceType* werden in das *CodePackage* platziert, das den *ServiceType* registriert. Das heißt, alle Replikate aller Dienste von eines bestimmten *ServiceType* nutzen gemeinsam denselben Prozess.

## <a name="exclusive-process-model"></a>Modell mit einem exklusiven Prozess
Das andere von Service Fabric bereitgestellte Hostingmodell ist das Modell mit einem **exklusiven Prozess**. In diesem Modell aktiviert Service Fabric auf einem bestimmten *Knoten* zum Platzieren jedes einzelnen Replikats eine neue Kopie des *ServicePackage* (das alle darin enthaltenen *CodePackages* startet), und das Replikat wird in das *CodePackage* platziert, das den *ServiceType* des Diensts registriert hat, dem das Replikat angehört. Das heißt, jedes Replikat wird in seinem eigenen dedizierten Prozess ausgeführt. 

Dieses Modell wird ab Version 5.6 von Service Fabric unterstützt. Das Modell mit einem **exklusiven Prozess** kann zum Zeitpunkt der Erstellung des Diensts ausgewählt werden (mit [PowerShell][p1], [REST][r1] oder [FabricClient][c1]), indem **ServicePackageActivationMode** auf „ExclusiveProcess“ festgelegt wird.

```powershell
PS C:\>New-ServiceFabricService -ApplicationName "fabric:/App1" -ServiceName "fabric:/App1/ServiceA" -ServiceTypeName "MyServiceType" -Stateless -PartitionSchemeSingleton -InstanceCount -1 -ServicePackageActivationMode "ExclusiveProcess"
```

```csharp
var serviceDescription = new StatelessServiceDescription
{
    ApplicationName = new Uri("fabric:/App1"),
    ServiceName = new Uri("fabric:/App1/ServiceA"),
    ServiceTypeName = "MyServiceType",
    PartitionSchemeDescription = new SingletonPartitionSchemeDescription(),
    InstanceCount = -1,
    ServicePackageActivationMode = ServicePackageActivationMode.ExclusiveProcess
};

var fabricClient = new FabricClient(clusterEndpoints);
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

Wenn Sie über einen Standarddienst im Anwendungsmanifest verfügen, können Sie das Modell mit einem **exklusiven Prozess** auswählen, indem Sie das Attribut **ServicePackageActivationMode** wie unten dargestellt festlegen:

```xml
<DefaultServices>
  <Service Name="MyService" ServicePackageActivationMode="ExclusiveProcess">
    <StatelessService ServiceTypeName="MyServiceType" InstanceCount="1">
      <SingletonPartition/>
    </StatelessService>
  </Service>
</DefaultServices>
```
Fahren wir mit unserem Beispiel oben fort: Erstellen Sie einen weiteren Dienst **fabric:/App1/ServiceC** in der Anwendung **fabric:/App1** mit 2 Partitionen (**P6** & **P7**) und 3 Replikaten pro Partition, und legen Sie **ServicePackageActivationMode** auf „ExclusiveProcess“ fest. Das folgende Diagramm zeigt die neue Sicht auf dem Knoten:

<center>
![Sicht der bereitgestellten Anwendung auf dem Knoten][node-view-four]
</center>

Wie Sie sehen können, hat Service Fabric zwei neue Kopien von „MyServicePackage“ aktiviert (eine für jedes Replikat aus den Partitionen **P6** & **P7**) und jedes Replikat in eine dedizierte Kopie von *CodePackage* platziert. Ein weiterer Aspekt ist hierbei zu beachten: Bei Verwendung des Modells mit einem **exklusiven Prozess** können für eine bestimmte *Anwendung* mehrere Kopien eines bestimmten *ServicePackage* auf einem *Knoten* aktiv sein. Im oben stehenden Beispiel sehen Sie, dass drei Kopien von „MyServicePackage“ für **fabric:/App1** aktiv sind. Jeder dieser aktiven Kopien von „MyServicePackage“ ist eine **ServicePackageAtivationId** zugeordnet, die diese Kopie innerhalb der *Anwendung* **fabric:/App1** identifiziert.

Wenn nur das Modell mit einem **gemeinsam genutzten Prozess** für eine *Anwendung* wie **fabric:/App2** im oben stehenden Beispiel verwendet wird, gibt es nur eine aktive Kopie von *ServicePackage* auf einem *Knoten*, und die **ServicePackageAtivationId** für diese Aktivierung von *ServicePackage* ist eine leere Zeichenfolge.

> [!NOTE]
>- Das Hostingmodell mit einem **gemeinsam genutzten Prozess** entspricht **ServicePackageAtivationMode** = **SharedProcess**. Dies ist das Standardhostingmodell, und **ServicePackageAtivationMode** muss zum Zeitpunkt der Erstellung des Diensts nicht festgelegt werden.
>
>- Das Hostingmodell mit einem **exklusiven Prozess** entspricht **ServicePackageAtivationMode** = **ExclusiveProcess**, und muss zum Zeitpunkt der Erstellung des Diensts explizit festgelegt werden. 
>
>- Das Hostingmodell eines Diensts können Sie ermitteln, indem Sie die [Dienstbeschreibung][p2] abfragen und den Wert von **ServicePackageAtivationMode** anschauen.
>
>

## <a name="working-with-deployed-service-package"></a>Arbeiten mit einem bereitgestellten Dienstpaket
Eine aktive Kopie eines *ServicePackage* auf einem Knoten wird als [bereitgestelltes Dienstpaket][p3] bezeichnet. Wenn Sie das Modells mit einem **exklusiven Prozess** zum Erstellen von Diensten für eine bestimmte *Anwendung* verwenden, können, wie bereits erwähnt, mehrere bereitgestellte Dienstpakete für dasselbe *ServicePackage* vorliegen. Beim Ausführen von Vorgängen, die für ein bereitgestelltes Dienstpaket spezifisch sind, wie z.B. die [Erstellung eines Berichts zur Integrität eines bereitgestellten Dienstpakets][p4] oder das [Neustarten des Codepakets eines bereitgestellten Dienstpakets][p5] usw., muss die **ServicePackageActivationId** angegeben werden, um ein bestimmtes bereitgestelltes Dienstpaket zu identifizieren.

 Die **ServicePackageActivationId** eines bereitgestellten Dienstpakets kann abgerufen werden, indem Sie die Liste der auf einem Knoten [bereitgestellten Dienstpakete] [p3] abfragen. Beim Abfragen von [bereitgestellten Diensttypen][p6], [bereitgestellten Replikaten][p7] und [bereitgestellten Codepaketen][p8] auf einem Knoten enthält das Ergebnis der Abfrage auch die **ServicePackageActivationId** des übergeordneten bereitgestellten Dienstpakets.

> [!NOTE]
>- Unter dem Hostingmodell mit einem **gemeinsam genutzten Prozess** wird auf einem bestimmten *Knoten* für eine bestimmte *Anwendung* nur eine Kopie eines *ServicePackage* aktiviert. Die **ServicePackageActivationId** ist eine *leere Zeichenfolge* und muss bei der Ausführung von Vorgängen im Zusammenhang mit bereitgestellten Dienstpaketen nicht angegeben werden. 
>
> - Unter dem Hostingmodell mit einem **exklusiven Prozess** können auf einem bestimmten *Knoten* für eine bestimmte *Anwendung* eine oder mehrere Kopien eines *ServicePackage* aktiv sein. Für jede Aktivierung ist **ServicePackageActivationId** *nicht leer* und muss bei der Ausführung von Vorgängen im Zusammenhang mit bereitgestellten Dienstpaketen angegeben werden. 
>
> - Wird **ServicePackageActivationId** nicht angegeben, wird der Wert standardmäßig auf eine leere Zeichenfolge festgelegt. Ist ein bereitgestelltes Dienstpaket vorhanden, das unter dem Modell mit einem **gemeinsam genutzten Prozess** aktiviert wurde, wird der Vorgang ausgeführt, andernfalls schlägt der Vorgang fehl.
>
> - Es wird nicht empfohlen, die **ServicePackageActivationId** einmalig abzufragen und zwischenzuspeichern, da sie dynamisch generiert wird und sich aus verschiedenen Gründen ändern kann. Vor dem Ausführen eines Vorgangs, bei dem die **ServicePackageActivationId** benötigt wird, sollten Sie zuerst die Liste der auf einen Knoten [bereitgestellten Dienstpakete][p3] abfragen und dann mit *ServicePackageActivationId** aus den Ergebnissen der Abfrage den ursprünglichen Vorgang ausführen.
>
>

## <a name="guest-executable-and-container-applications"></a>Ausführbare Gastanwendungsdatei- und Containeranwendungen
Service Fabric behandelt [ausführbare Gastanwendungsdatei-][a2] und [Containeranwendungen][a3] als zustandslose Dienste, die eigenständig sind, d. h., dass es keine Service Fabric-Laufzeit in *ServiceHost* (Prozess oder Container) gibt. Da diese Dienste eigenständig sind, gilt die Anzahl der Replikate pro *ServiceHost* für diese Dienste nicht. Die mit diesen Diensten am häufigsten verwendete Konfiguration ist eine einzelne Partition mit [InstanceCount][c2] = -1 (d. h., auf jedem Knoten des Clusters wird eine Kopie des Dienstcodes ausgeführt). 

Der standardmäßige **ServicePackageActivationMode** für diese Dienste ist **SharedProcess**. In diesem Fall aktiviert Service Fabric nur eine Kopie des *ServicePackage* auf einem *Knoten* für eine bestimmte *Anwendung*, was bedeutet, dass auf einem *Knoten* nur eine Kopie des Dienstcodes ausgeführt wird. Wenn Sie aber auf einem *Knoten* mehrere Kopien Ihres Dienstcodes ausführen möchten, dann sollten Sie, beim Erstellen mehrerer Dienste (*Service1* bis *ServiceN*) des (im *ServiceManifest* angegebenen) *ServiceType* bzw. wenn Ihr Dienst mehrere Partitionen enthält, zum Zeitpunkt der Erstellung des Diensts als **ServicePackageActivationMode** die Option **ExclusiveProcess** angeben.

## <a name="changing-hosting-model-of-an-existing-service"></a>Ändern des Hostingmodells eines vorhandenen Diensts
Das Ändern des Hostingmodells eines vorhandenen Diensts von **SharedProcess** zu **ExclusiveProcess** und umgekehrt durch Upgrade- oder Updatemechanismen (bzw. in der Standarddienstspezifikation im Anwendungsmanifest) wird derzeit nicht unterstützt. Die Unterstützung für dieses Feature ist für zukünftige Versionen geplant.

## <a name="choosing-between-shared-process-and-exclusive-process-model"></a>Auswählen zwischen einem Modell mit einem gemeinsam genutzten Prozess und einem Modell mit einem exklusiven Prozess
Beide Hostingmodelle weisen Vor- und Nachteile auf, und der Benutzer muss abwägen, welches Modell seine Anforderungen am besten erfüllt. Das Modell mit einem **gemeinsam genutzten Prozess** ermöglicht eine bessere Nutzung von Betriebssystemressourcen, da weniger Prozesse erzeugt werden, mehrere Replikate im selben Prozess Ports gemeinsam verwenden können usw. Wenn eines der Replikate jedoch auf einen Fehler stößt, durch den der Diensthost beendet werden muss, hat dies Auswirkungen auf alle anderen Replikate im selben Prozess.

 Das Modell mit einem **exklusiven Prozess** bietet eine bessere Isolierung, bei der jedes Replikat in seinem eigenen Prozess ausgeführt wird und ein fehlerhaftes Replikat sich nicht auf die anderen Replikate auswirkt. Dies ist beispielsweise praktisch, wenn die gemeinsame Portnutzung durch das Kommunikationsprotokoll nicht unterstützt wird. Es erleichtert die Ressourcenkontrolle auf Replikatebene. Andererseits verbrauchen **exklusive Prozesse** mehr Betriebssystemressourcen, da ein Prozess für jedes Replikat auf dem Knoten erzeugt wird.

## <a name="exclusive-process-model-and-application-model-considerations"></a>Überlegungen zum Modell mit einem exklusiven Prozess und zur Anwendungsmodellierung
Die empfohlene Methode für die Anwendungsmodellierung in Service Fabric besteht darin, einen *ServiceType* pro *ServicePackage* zu verwalten, und dieses Modell eignet sich gut für die meisten Anwendungen. 

Um jedoch spezielle Szenarien zu ermöglichen, bei denen ein *ServiceType* pro *ServicePackage* möglicherweise nicht wünschenswert ist, ist es in Service Fabric funktionell zulässig, mehr als einen *ServiceType* pro *ServicePackage* zu verwenden (und eine *CodePackage* kann mehr als einen *ServiceType* registrieren). Es folgen einige Szenarien, in denen diese Konfigurationen nützlich sein können:

- Sie möchten die Verwendung von Betriebssystemressourcen dadurch optimieren, dass weniger Prozesse mit einer erhöhten Replikatdichte pro Prozess erzeugt werden.
- Replikate aus verschiedenen ServiceTypes müssen einige allgemeine Daten mit hohen Initialisierungs- oder Arbeitsspeicherkosten gemeinsam nutzen.
- Sie verfügen über ein Angebot kostenloser Dienste, und Sie möchten einen Grenzwert für die Ressourcenverwendung festlegen, indem Sie alle Replikate des Diensts im den gleichen Prozess platzieren.

Das Hostingmodell mit einem **exklusiven Prozess** ist nicht kohärent mit einem Anwendungsmodell, das über mehrere *ServiceTypes* pro *ServicePackage* verfügt. Grund hierfür ist, dass mehrere *ServiceTypes* pro *ServicePackage* dazu ausgelegt sind, eine höhere gemeinsame Ressourcennutzung zwischen Replikaten zu erreichen und eine höhere Replikatdichte pro Prozess ermöglichen. Dies steht im Gegensatz zu den Zielen des Modells mit einem **exklusiven Prozess**.

Betrachten Sie den Fall mehrerer *ServiceTypes* pro *ServicePackage*, wobei jeder *ServiceType* von einem anderen *CodePackage* registriert wird. Angenommen, wir haben ein *ServicePackage* „MultiTypeServicePackge“ mit zwei *CodePackages*:

- „MyCodePackageA“ registriert *ServiceType* „MyServiceTypeA“.
- „MyCodePackageB“ registriert *ServiceType* „MyServiceTypeB“.

Angenommen, wir erstellen jetzt eine *Anwendung* **fabric:/SpecialApp**, und innerhalb von **fabric:/SpecialApp** erstellen wir die folgenden beiden Dienste mit dem Modell mit einem **exklusiven Prozess**:

- Den Dienst **fabric:/SpecialApp/ServiceA** vom Typ „MyServiceTypeA“ mit zwei Partitionen (**P1** und **P2**) und 3 Replikaten pro Partition.
- Den Dienst **fabric:/SpecialApp/ServiceB** vom Typ „MyServiceTypeB“ mit zwei Partitionen (**P3** und **P4**) und 3 Replikaten pro Partition.

Beide Dienste verfügen auf einem bestimmten Knoten über je zwei Replikate. Da wir verwendet das Modell mit einem **exklusiven Prozess** Erstellen von der Dienste verwendet haben, aktiviert Service Fabric eine neue Kopie von „MyServicePackage“ für jedes Replikat. Jede Aktivierung von „MultiTypeServicePackage“ startet eine Kopie von „MyCodePackageA“ und „MyCodePackageB“. Allerdings wird das Replikat, für das „MultiTypeServicePackage“ aktiviert wurde, nur entweder von „MyCodePackageA“ oder „MyCodePackageB“ gehostet. Das folgende Diagramm zeigt die Knotensicht:

<center>
![Sicht der bereitgestellten Anwendung auf dem Knoten][node-view-five]
</center>

 Wie zu sehen ist, wird das Replikat der Partition **P1** des Diensts **fabric:/SpecialApp/ServiceA** bei der Aktivierung von „MultiTypeServicePackage“ von „MyCodePackageA“ gehostet, und „MyCodePackageB“ wird gerade ausgeführt. Gleichermaßen wird das Replikat der Partition **P3** des Diensts **fabric:/SpecialApp/ServiceB** bei der Aktivierung von „MultiTypeServicePackage“ von „MyCodePackageB“ gehostet, und „MyCodePackageA“ wird gerade ausgeführt usw. Daher gilt: Je höher die Zahl der *CodePackages* (die verschiedene *ServiceTypes* registrieren) pro *ServicePackage* ist, desto höher wird die redundante Ressourcenverwendung. 
 
 Wenn wird andererseits die Dienste **fabric:/SpecialApp/ServiceA** und **fabric:/SpecialApp/ServiceB** mit dem Modell mit einem **gemeinsam genutzten Prozess** erstellen, aktiviert Service Fabric nur eine Kopie von „MultiTypeServicePackage“ für die *Anwendung* **fabric:/SpecialApp** (wie zuvor beschrieben). „MyCodePackageA“ hostet alle Replikate für den Dienst **fabric:/SpecialApp/ServiceA** (bzw. eines beliebigen Diensts vom Typ „MyServiceTypeA“) und „MyCodePackageB“ hostet alle Replikate für den Dienst **fabric:/SpecialApp/ServiceB** (bzw. eines beliebigen Diensts vom Typ „MyServiceTypeB“). Das folgende Diagramm zeigt die Knotensicht in dieser Einstellung: 

<center>
![Sicht der bereitgestellten Anwendung auf dem Knoten][node-view-six]
</center>

Sie könnten einwenden, dass im oben stehenden Beispiel keine redundante Ausführung von *CodePackage* erfolgt, wenn „MyCodePackageA“ sowohl „MyServiceTypeA“ als auch „MyServiceTypeB“ registriert, aber kein „MyCodePackageB“ existiert. Dies ist richtig, aber wie bereits erwähnt, steht dieses Anwendungsmodell nicht in Einklang mit dem Histingmodell mit einem **exklusive Prozess**. Wenn das Ziel darin besteht, jedes Replikat in einem eigenen dedizierten Prozess zu verarbeitet, ist die Registrierung beider *ServiceTypes* aus demselben *CodePackage* nicht erforderlich, und es ist sinnvoller, jeden *ServiceType* in ein eigenes *ServicePackage* zu platzieren.

## <a name="next-steps"></a>Nächste Schritte
[Packen][a4] und Vorbereiten einer Anwendung für die Bereitstellung.

[Bereitstellen und Entfernen von Anwendungen][a5] beschreibt, wie Sie mit PowerShell Anwendungsinstanzen verwalten.

<!--Image references-->
[node-view-one]: ./media/service-fabric-hosting-model/node-view-one.png
[node-view-two]: ./media/service-fabric-hosting-model/node-view-two.png
[node-view-three]: ./media/service-fabric-hosting-model/node-view-three.png
[node-view-four]: ./media/service-fabric-hosting-model/node-view-four.png
[node-view-five]: ./media/service-fabric-hosting-model/node-view-five.png
[node-view-six]: ./media/service-fabric-hosting-model/node-view-six.png

<!--Link references--In actual articles, you only need a single period before the slash-->
[a1]: service-fabric-application-model.md
[a2]: service-fabric-deploy-existing-app.md
[a3]: service-fabric-containers-overview.md
[a4]: service-fabric-package-apps.md
[a5]: service-fabric-deploy-remove-applications.md

[r1]: https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-createservice

[c1]: https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync
[c2]: https://docs.microsoft.com/dotnet/api/system.fabric.description.statelessservicedescription.instancecount

[p1]: https://docs.microsoft.com/powershell/servicefabric/vlatest/new-servicefabricservice
[p2]: https://docs.microsoft.com/powershell/servicefabric/vlatest/get-servicefabricservicedescription
[p3]: https://docs.microsoft.com/powershell/servicefabric/vlatest/get-servicefabricdeployedservicePackage
[p4]: https://docs.microsoft.com/powershell/servicefabric/vlatest/send-servicefabricdeployedservicepackagehealthreport
[p5]: https://docs.microsoft.com/powershell/servicefabric/vlatest/restart-servicefabricdeployedcodepackage
[p6]: https://docs.microsoft.com/powershell/servicefabric/vlatest/get-servicefabricdeployedservicetype
[p7]: https://docs.microsoft.com/powershell/servicefabric/vlatest/get-servicefabricdeployedreplica
[p8]: https://docs.microsoft.com/powershell/servicefabric/vlatest/get-servicefabricdeployedcodepackage
