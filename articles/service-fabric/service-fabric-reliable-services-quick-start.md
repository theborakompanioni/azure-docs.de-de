---
title: Erste Schritte mit Reliable Services | Microsoft Docs
description: Einführung in das Erstellen einer Microsoft Azure Service Fabric-Anwendung mit zustandslosen und zustandsbehafteten Diensten.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: ''

ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/06/2016
ms.author: vturecek

---
# Erste Schritte mit Service Fabric Reliable Services
Eine Azure Service Fabric-Anwendung enthält einen oder mehrere Dienste zum Ausführen von Code. Dieses Handbuch veranschaulicht das Erstellen sowohl zustandsloser als auch zustandsbehafteter Service Fabric-Anwendungen mit [Reliable Services](service-fabric-reliable-services-introduction.md).

## Erstellen eines zustandslosen Diensts
Ein zustandsloser Dienst ist eine Art von Dienst, der in Cloudanwendungen derzeit die Norm ist. Er wird als zustandslos angesehen, weil der Dienst selbst keine Daten enthält, die zuverlässig gespeichert werden oder hoch verfügbar sein müssen. Wenn eine Instanz eines zustandslosen Diensts heruntergefahren wird, geht sein gesamter interner Zustand verloren. Damit der Zustand dieser Dienste hoch verfügbar und zuverlässig ist, muss er extern gespeichert werden, z. B. in Azure-Tabellen oder in einer SQL-Datenbank.

Starten Sie Visual Studio 2015 als Administrator, und erstellen Sie ein neues Projekt mit einer Service Fabric-Anwendung, das den Namen *HelloWorld* trägt:

![Erstellen Sie über das Dialogfeld „Neues Projekt“ eine neue Service Fabric-Anwendung.](media/service-fabric-reliable-services-quick-start/hello-stateless-NewProject.png)

Erstellen Sie anschließend ein Projekt für einen zustandslosen Dienst mit dem Namen *HelloWorldStateless*:

![Erstellen Sie im zweiten Dialogfeld ein Projekt für einen zustandslosen Dienst.](media/service-fabric-reliable-services-quick-start/hello-stateless-NewProject2.png)

Die Projektmappe enthält jetzt zwei Projekte:

* *HelloWorld*: Dies ist das *Anwendung* sprojekt, das Ihre *Dienste* enthält. Darüber hinaus enthält es das Anwendungsmanifest zum Beschreiben der Anwendung sowie eine Reihe von PowerShell-Skripts, mit deren Hilfe Sie die Anwendung bereitstellen.
* *HelloWorldStateless*: Dies ist das Dienstprojekt. Es enthält die Implementierung des zustandslosen Diensts.

## Implementieren des Diensts
Öffnen Sie im Dienstprojekt die Datei **HelloWorld.cs**. In Service Fabric kann mit einem Dienst jegliche Art von Geschäftslogik ausgeführt werden. Die Dienst-API bietet zwei Einstiegspunkte für den Code:

* Eine Einstiegspunktmethode mit offenem Ende namens *RunAsync*, mit der Sie die Ausführung beliebiger Workloads startet können, inklusive lang andauernder Compute-Workloads.

```csharp
protected override async Task RunAsync(CancellationToken cancellationToken)
{
    ...
}
```

* Einen Einstiegspunkt für die Kommunikation, den Sie mit einem beliebigen Kommunikationsstapel verbinden können, z. B. der ASP.NET Web-API. Dies ist der Punkt, an dem Sie mit dem Empfangen der Anforderungen von Benutzern und anderen Diensten beginnen können.

```csharp
protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    ...
}
```

In diesem Tutorial geht es um die Einstiegspunktmethode `RunAsync()`. Hiermit können Sie sofort mit der Ausführung des Codes beginnen. Die Projektvorlage enthält ein Beispiel für eine Implementierung von `RunAsync()`, die einen rollierenden Zähler schrittweise erhöht.

> [!NOTE]
> Ausführliche Informationen zur Verwendung eines Kommunikationsstapels finden Sie unter [Web-API-Dienste von Service Fabric mit selbstgehostetem OWIN](service-fabric-reliable-services-communication-webapi.md).
> 
> 

### RunAsync
```csharp
protected override async Task RunAsync(CancellationToken cancellationToken)
{
    // TODO: Replace the following sample code with your own logic 
    //       or remove this RunAsync override if it's not needed in your service.

    long iterations = 0;

    while (true)
    {
        cancellationToken.ThrowIfCancellationRequested();

        ServiceEventSource.Current.ServiceMessage(this, "Working-{0}", ++iterations);

        await Task.Delay(TimeSpan.FromSeconds(1), cancellationToken);
    }
}
```

Die Plattform ruft diese Methode auf, wenn eine Instanz des Diensts platziert wurde und zur Ausführung bereit ist. Bei zustandslosen Diensten ist dies einfach der Fall, wenn die Dienstinstanz geöffnet wird. Ein Abbruchtoken koordiniert, wann die Dienstinstanz geschlossen werden muss. In Service Fabric kann dieser Offen/Geschlossen-Zyklus einer Dienstinstanz über die gesamte Lebensdauer des Diensts häufig auftreten. Dies kann aus unterschiedlichen Gründen geschehen, z. B.:

* Das System verschiebt Ihre Dienstinstanzen, um einen Lastenausgleich für Ressourcen durchzuführen.
* In Ihrem Code treten Fehler auf.
* Die Anwendung oder das System werden aktualisiert.
* Die zugrunde liegende Hardware fällt aus.

Diese Orchestrierung wird vom System verwaltet, um sicherzustellen, dass der Dienst hoch verfügbar bleibt und die Lasten richtig verteilt sind.

`RunAsync()` wird in einer eigenen Aufgabe ausgeführt. Beachten Sie, dass wir im obigen Codeausschnitt direkt in eine *while*-Schleife gesprungen sind. Es ist nicht erforderlich, für Ihre Workload eine separate Aufgabe einzuplanen. Zum Abbrechen der Arbeitsauslastung ist das Zusammenspiel verschiedener Aktionen erforderlich, die vom bereitgestellten Abbruchtoken orchestriert werden. Das System wartet, bis Ihre Aufgabe beendet wurde (erfolgreicher Abschluss, Abbruch oder Fehler), bevor der Vorgang fortgesetzt wird. Es ist wichtig, das Abbruchtoken zu berücksichtigen, etwaige Arbeiten abzuschließen und `RunAsync()` so schnell wie möglich zu beenden, wenn vom System ein Abbruch angefordert wird.

In diesem Beispiel eines zustandslosen Diensts wird die Anzahl in einer lokalen Variablen gespeichert. Da es sich aber um einen zustandslosen Dienst handelt, existiert der gespeicherte Wert nur für den aktuellen Lebenszyklus der Dienstinstanz. Wenn der Dienst verschoben oder neu gestartet wird, geht der Wert verloren.

## Erstellen eines zustandsbehafteten Diensts
Mit Service Fabric wird eine neue Art von zustandsbehaftetem Dienst eingeführt. Bei einem zustandsbehafteten Dienst kann der Zustand zuverlässig innerhalb des Diensts selbst verwaltet und dem Code zugeordnet werden, in dem er verwendet wird. Service Fabric stellt die hohe Verfügbarkeit des Zustands sicher, ohne dass dieser extern gespeichert werden muss.

Um einen Zählerwert selbst bei einer Verschiebung oder einem Neustart des Diensts von zustandslos zu hoch verfügbar und persistent zu konvertieren, benötigen Sie einen zustandsbehafteten Dienst.

In der gleichen *HelloWorld*-Anwendung können Sie einen neuen Dienst hinzufügen, indem Sie mit der rechten Maustaste auf die Services-Referenzen im Anwendungsprojekt klicken und **Hinzufügen -> Neuer Service Fabric-Dienst** wählen.

![Fügen Sie der Service Fabric-Anwendung einen Dienst hinzu.](media/service-fabric-reliable-services-quick-start/hello-stateful-NewService.png)

Wählen Sie **Zustandsbehafteter Dienst**, und geben Sie ihm den Namen *HelloWorldStateful*. Klicken Sie auf **OK**.

![Verwenden Sie das Dialogfeld „Neues Projekt“, um einen zustandsbehafteten Service Fabric-Dienst zu erstellen.](media/service-fabric-reliable-services-quick-start/hello-stateful-NewProject.png)

Ihre Anwendung sollte nun über zwei Dienste verfügen: den zustandslosen Dienst *HelloWorldStateless* und den zustandsbehafteten Dienst *HelloWorldStateful*.

Ein zustandsbehafteter Dienst hat die gleichen Einstiegspunkte wie ein zustandsloser Dienst. Der Hauptunterschied liegt in der Verfügbarkeit eines *Zustandsanbieters*, der den Zustand zuverlässig speichern kann. Service Fabric beinhaltet eine Zustandsanbieterimplementation namens [Reliable Collections](service-fabric-reliable-services-reliable-collections.md), die es Ihnen ermöglicht, replizierte Datenstrukturen mittels Reliable State Manager zu erstellen. Ein zustandsbehafteter Reliable Service verwendet standardmäßig diesen Zustandsanbieter.

Öffnen Sie **HelloWorldStateful.cs** in *HelloWorldStateful*. Darin ist die folgende RunAsync-Methode enthalten:

```csharp
protected override async Task RunAsync(CancellationToken cancellationToken)
{
    // TODO: Replace the following sample code with your own logic 
    //       or remove this RunAsync override if it's not needed in your service.

    var myDictionary = await this.StateManager.GetOrAddAsync<IReliableDictionary<string, long>>("myDictionary");

    while (true)
    {
        cancellationToken.ThrowIfCancellationRequested();

        using (var tx = this.StateManager.CreateTransaction())
        {
            var result = await myDictionary.TryGetValueAsync(tx, "Counter");

            ServiceEventSource.Current.ServiceMessage(this, "Current Counter Value: {0}",
                result.HasValue ? result.Value.ToString() : "Value does not exist.");

            await myDictionary.AddOrUpdateAsync(tx, "Counter", 0, (key, value) => ++value);

            // If an exception is thrown before calling CommitAsync, the transaction aborts, all changes are
            // discarded, and nothing is saved to the secondary replicas.
            await tx.CommitAsync();
        }

        await Task.Delay(TimeSpan.FromSeconds(1), cancellationToken);
    }
```

### RunAsync
`RunAsync()` funktioniert in zustandsbehafteten und zustandslosen Diensten ähnlich. Bei einem zustandsbehafteten Dienst werden von der Plattform aber noch weitere Schritte in Ihrem Namen ausgeführt, bevor `RunAsync()` ausgeführt wird. Hierzu kann auch die Sicherstellung dessen gehören, dass der Reliable State Manager und Reliable Collections für die Verwendung bereit sind.

### Reliable Collections und der Reliable State Manager
```csharp
var myDictionary = await this.StateManager.GetOrAddAsync<IReliableDictionary<string, long>>("myDictionary");
```

[IReliableDictionary](https://msdn.microsoft.com/library/dn971511.aspx) ist eine Wörterbuchimplementierung, die Sie nutzen können, um den Zustand im Dienst zuverlässig zu speichern. Mit Service Fabric und Reliable Collections können Sie Daten direkt in Ihrem Dienst speichern. Ein externer persistenter Speicher ist nicht erforderlich. Reliable Collections stellen die hohe Verfügbarkeit Ihrer Daten her. Service Fabric erreicht dies, indem mehrere *Replikate* Ihres Diensts für Sie erstellt und verwaltet werden. Außerdem wird eine API bereitgestellt, mit der die komplexen Verwaltungsanforderungen dieser Replikate und der damit verbundenen Zustandsübergänge beseitigt werden.

Reliable Collections können mit gewissen Einschränkungen beliebige .NET-Typen – einschließlich benutzerdefinierten Typen – speichern:

* Service Fabric macht Ihren Zustand hoch verfügbar, indem der Zustand über verschiedene Knoten *repliziert* wird und Reliable Collections Ihre Daten an jedem Replikat auf einen lokalen Datenträger speichert. Dies bedeutet, dass alle Elemente, die in Reliable Collections gespeichert werden, *serialisierbar* sein müssen. Reliable Collections verwenden standardmäßig [DataContract](https://msdn.microsoft.com/library/system.runtime.serialization.datacontractattribute%28v=vs.110%29.aspx) für die Serialisierung. Stellen Sie daher bei Verwendung des Standardserialisierers unbedingt sicher, dass die Typen [vom Datenverwaltungsserialisierer unterstützt](https://msdn.microsoft.com/library/ms731923%28v=vs.110%29.aspx) werden.
* Objekte werden zum Zweck einer hohen Verfügbarkeit repliziert, wenn Sie Transaktionen auf Reliable Collections anwenden. In Reliable Collections gespeicherte Objekte werden in Ihrem Dienst im lokalen Speicher vorgehalten. Dies bedeutet, dass Sie über einen lokalen Verweis auf das Objekt verfügen.
  
   Es ist wichtig, dass Sie lokale Instanzen dieser Objekte nicht ändern, ohne ein Update für die Reliable Collection in einer Transaktion durchzuführen. Das liegt daran, dass Änderungen an lokalen Instanzen von Objekten nicht automatisch repliziert werden. Sie müssen das Objekt wieder zurück in das Wörterbuch einfügen oder eine der Methoden zur *Aktualisierung* auf das Wörterbuch anwenden.

Reliable State Manager verwaltet Reliable Collections für Sie. Sie können über Reliable State Manager jederzeit und von jedem Ort in Ihrem Dienst aus anhand des Namens eine zuverlässige Auflistung anfordern. Der Reliable State Manager stellt sicher, dass Sie einen Verweis zurückerhalten. Es ist nicht ratsam, Verweise auf Reliable Collection-Instanzen in Klassenmembervariablen oder -eigenschaften zu speichern. Achten Sie besonders darauf sicherzustellen, dass der Verweis während des Dienstlebenszyklus jederzeit auf eine Instanz festgelegt ist. Der Reliable State Manager übernimmt diesen Schritt für Sie. Er ist für wiederholte Besuche optimiert.

### Transaktionale und asynchrone Vorgänge
```C#
using (ITransaction tx = this.StateManager.CreateTransaction())
{
    var result = await myDictionary.TryGetValueAsync(tx, "Counter-1");

    await myDictionary.AddOrUpdateAsync(tx, "Counter-1", 0, (k, v) => ++v);

    await tx.CommitAsync();
}
```

Reliable Collections verfügen über viele der gleichen Vorgänge, die auch für ihre Gegenstücke `System.Collections.Generic` und `System.Collections.Concurrent` möglich sind, außer LINQ. Vorgänge für Reliable Collections sind asynchron. Das liegt daran, dass Schreibvorgänge mit Reliable Collections E/A-Vorgänge ausführen, um Replikationen der Daten und ihre persistente Speicherung auf dem Datenträger vorzunehmen.

Reliable Collection-Vorgänge sind *transaktional*, damit Sie den Zustand über mehrere Reliable Collections und Vorgänge hinweg beibehalten können. Sie können beispielsweise eine Arbeitsaufgabe aus einer zuverlässigen Warteschlange entfernen, einen Vorgang daran ausführen und das Ergebnis in einem zuverlässigen Wörterbuch speichern – alles in einer Transaktion. Dies wird als atomischer Vorgang behandelt und es wird sichergestellt, dass entweder der gesamte Vorgang erfolgreich ist oder ein Rollback für den gesamten Vorgang ausgeführt wird. Wenn nach dem Entfernen des Elements aus der Warteschlange und vor dem Speichern des Ergebnisses ein Fehler auftritt, wird für die gesamte Transaktion ein Rollback ausgeführt, und das Element bleibt zur Verarbeitung in der Warteschlange enthalten.

## Ausführen der Anwendung
Wir kehren nun zur Anwendung *HelloWorld* zurück. Sie können jetzt Dienste erstellen und bereitstellen. Wenn Sie **F5** drücken, wird die Anwendung erstellt und im lokalen Cluster bereitgestellt.

Nach dem Beginn der Dienstausführung können Sie die generierten ETW-Ereignisse (Ereignisablaufverfolgung für Windows) in einem Fenster für **Diagnoseereignisse** ansehen. Beachten Sie, dass sowohl Ereignisse des zustandslosen Diensts als auch des zustandsbehafteten Diensts in der Anwendung angezeigt werden. Sie können den Datenstrom anhalten, indem Sie auf die Schaltfläche **Anhalten** klicken. Sie können die Details einer Nachricht dann prüfen, indem Sie sie erweitern.

> [!NOTE]
> Stellen Sie vor dem Ausführen der Anwendung sicher, dass ein lokaler Entwicklungscluster ausgeführt wird. Informationen zum Einrichten Ihrer lokalen Umgebung finden Sie im [Leitfaden zu den ersten Schritten](service-fabric-get-started.md).
> 
> 

![Zeigen Sie Diagnoseereignisse in Visual Studio an.](media/service-fabric-reliable-services-quick-start/hello-stateful-Output.png)

## Nächste Schritte
[Debuggen einer Service Fabric-Anwendung in Visual Studio](service-fabric-debugging-your-application.md)

[Erste Schritte: Web-API-Dienste von Service Fabric mit selbstgehostetem OWIN](service-fabric-reliable-services-communication-webapi.md)

[Erfahren Sie mehr über zuverlässige Auflistungen](service-fabric-reliable-services-reliable-collections.md)

[Bereitstellen von Anwendungen](service-fabric-deploy-remove-applications.md)

[Anwendungsupgrade](service-fabric-application-upgrade.md)

[Entwicklerreferenz für zuverlässige Dienste](https://msdn.microsoft.com/library/azure/dn706529.aspx)

<!---HONumber=AcomDC_0713_2016-->