---
title: "Übersicht über das Reliable Services-Kommunikationsmodell | Microsoft Docs"
description: "Übersicht über das Reliable Services-Kommunikationsmodell, einschließlich Öffnen von Listenern für Dienste, Auflösen von Endpunkten und Kommunikation zwischen Diensten."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: BharatNarasimman
ms.assetid: 36217988-420e-409d-b0a4-e0e875b6eac8
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 10/19/2016
ms.author: vturecek
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 288ed1efc53ad25fcdad668aaaf5e5ebdc0a131a


---
# <a name="how-to-use-the-reliable-services-communication-apis"></a>Gewusst wie: Verwenden der Reliable Services-Kommunikations-APIs
Azure Service Fabric ist als Plattform vollständig unabhängig von der Kommunikation zwischen Diensten. Alle Protokolle und Stapel von UDP bis HTTP sind zulässig. Es liegt in der Entscheidung des Entwicklers, wie Dienste kommunizieren sollen. Das Reliable Services-Anwendungsframework stellt integrierte Kommunikationsstapel und APIs bereit, die Sie zum Erstellen Ihrer benutzerdefinierten Kommunikationskomponenten verwenden können. 

## <a name="set-up-service-communication"></a>Einrichten der Dienstkommunikation
Die Reliable Services-API verwendet eine einfache Schnittstelle für die Dienstkommunikation. Um einen Endpunkt für den Dienst zu öffnen, implementieren Sie einfach die folgende Schnittstelle:

```csharp

public interface ICommunicationListener
{
    Task<string> OpenAsync(CancellationToken cancellationToken);

    Task CloseAsync(CancellationToken cancellationToken);

    void Abort();
}

```

Sie können anschließend Ihre Kommunikationslistener-Implementierung hinzufügen, indem Sie sie in der Überschreibung einer dienstbasierten Klassenmethode zurückgeben.

Zustandslose Dienste:

```csharp
class MyStatelessService : StatelessService
{
    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        ...
    }
    ...
}
```

Zustandsbehaftete Dienste:

```csharp
class MyStatefulService : StatefulService
{
    protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
    {
        ...
    }
    ...
}
```

In beiden Fällen geben Sie eine Sammlung von Listenern zurück. Dies ermöglicht dem Dienst durch Verwendung mehrerer Listener das Lauschen auf mehrere Endpunkte, die eventuell unterschiedliche Protokolle verwenden. Sie besitzen beispielsweise einen HTTP-Listener und einen separaten WebSocket-Listener. Jeder Listener erhält einen Namen, und die resultierende Sammlung von *name : address* -Paaren wird als JSON-Objekt dargestellt, wenn ein Client die Listeneradressen für eine Dienstinstanz oder eine Partition anfordert.

Bei einem zustandslosen Dienst gibt die Überschreibung eine Sammlung von ServiceInstanceListeners zurück. Ein ServiceInstanceListener enthält eine Funktion für die ICommunicationListener-Erstellung und weist diesem Listener einen Namen zu. Bei statusbehafteten Diensten gibt die Überschreibung eine ServiceReplicaListener-Sammlung zurück. Dies unterscheidet sich leicht vom zustandslosen Gegenstück, da ein ServiceReplicaListener über eine Option zum Öffnen eines ICommunicationListeners auf sekundären Replikaten verfügt. Sie können nicht nur mehrere Kommunikationslistener in einem Dienst verwenden, sondern auch angeben, welche Listener auf sekundären Replikaten Anforderungen akzeptieren und welche nur auf primären Replikaten ausgeführt werden.

Beispielsweise können Sie einen ServiceRemotingListener besitzen, der nur RPC-Aufrufe auf primären Replikaten akzeptiert, und einen zweiten, benutzerdefinierten Listener, der Leseanforderungen für sekundäre Replikate über HTTP akzeptiert:

```csharp
protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new[]
    {
        new ServiceReplicaListener(context =>
            new MyCustomHttpListener(context),
            "HTTPReadonlyEndpoint",
            true),

        new ServiceReplicaListener(context =>
            this.CreateServiceRemotingListener(context),
            "rpcPrimaryEndpoint",
            false)
    };
}
```

> [!NOTE]
> Beim Erstellen mehrerer Listener für einen Dienst **muss** jeder Listener einen eindeutigen Namen erhalten.
> 
> 

Abschließend beschreiben Sie die Endpunkte, die für den Dienst erforderlich sind, im [Dienstmanifest](service-fabric-application-model.md) im Abschnitt „Endpunkte“.

```xml
<Resources>
    <Endpoints>
      <Endpoint Name="WebServiceEndpoint" Protocol="http" Port="80" />
      <Endpoint Name="OtherServiceEndpoint" Protocol="tcp" Port="8505" />
    <Endpoints>
</Resources>

```

Der Kommunikationslistener kann die ihm zugewiesenen Endpunktressourcen über `CodePackageActivationContext` in `ServiceContext` aufrufen. Die Überwachung von Anforderungen kann nach dem Öffnen des Listeners beginnen.

```csharp
var codePackageActivationContext = serviceContext.CodePackageActivationContext;
var port = codePackageActivationContext.GetEndpoint("ServiceEndpoint").Port;

```

> [!NOTE]
> Das gesamte Dienstpaket verwendet einheitliche Endpunktressourcen, die von Service Fabric beim Aktivieren des Dienstpakets zugewiesen werden. Mehrere Dienstreplikate, die im gleichen ServiceHost gehostet werden, können den gleichen Port verwenden. Dies bedeutet, dass der Kommunikationslistener die Portfreigabe unterstützen sollte. Zu diesem Zweck wird empfohlen, dass der Kommunikationslistener zum Generieren der Überwachungsadresse die Partitions-ID und die Replikat-/Instanz-ID verwendet.
> 
> 

### <a name="service-address-registration"></a>Registrierung von Dienstadressen
Auf Service Fabric-Clustern wird ein Systemdienst mit dem Namen *Naming Service* ausgeführt. Der Naming Service ist eine Registrierungsstelle für Dienste und ihre Adressen, auf die jede Instanz oder jedes Replikat des Diensts lauscht. Wenn die `OpenAsync`-Methode eines `ICommunicationListener`-Elements abgeschlossen ist, wird ihr Rückgabewert im Naming Service registriert. Bei diesem im Naming Service veröffentlichten Rückgabewert handelt es sich um eine Zeichenfolge, deren Wert beliebig sein kann. Clients sehen diesen Zeichenfolgenwert, wenn sie eine Adresse für den Dienst vom Naming Service anfordern.

```csharp
public Task<string> OpenAsync(CancellationToken cancellationToken)
{
    EndpointResourceDescription serviceEndpoint = serviceContext.CodePackageActivationContext.GetEndpoint("ServiceEndpoint");
    int port = serviceEndpoint.Port;

    this.listeningAddress = string.Format(
                CultureInfo.InvariantCulture,
                "http://+:{0}/",
                port);

    this.publishAddress = this.listeningAddress.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);

    this.webApp = WebApp.Start(this.listeningAddress, appBuilder => this.startup.Invoke(appBuilder));

    // the string returned here will be published in the Naming Service.
    return Task.FromResult(this.publishAddress);
}
```

Über die von Service Fabric bereitgestellten APIs können Clients und andere Dienste dann diese Adresse anhand des Dienstnamens anfordern. Dies ist wichtig, weil die Dienstadresse nicht statisch ist. Dienste werden im Cluster zum Zwecke des Lastenausgleichs von Ressourcen und der Verfügbarkeit hin und her verschoben. Dies ist der Mechanismus, mit dem Clients die Überwachungsadresse für einen Dienst auflösen können.

> [!NOTE]
> Eine ausführliche Anleitung mit den Schritten zum Schreiben eines `ICommunicationListener`-Elements finden Sie unter [Web-API-Dienste von Service Fabric mit selbstgehostetem OWIN](service-fabric-reliable-services-communication-webapi.md).
> 
> 

## <a name="communicating-with-a-service"></a>Kommunizieren mit einem Dienst
Die Reliable Services-API umfasst die folgenden Bibliotheken zum Schreiben von Clients für die Kommunikation mit Diensten.

### <a name="service-endpoint-resolution"></a>Dienstendpunktauflösung
Der erste Schritt bei der Kommunikation mit einem Dienst ist die Auflösung einer Endpunktadresse der Partition oder Instanz des Diensts, mit der Sie kommunizieren möchten. Die `ServicePartitionResolver` -Hilfsklasse ist eine allgemeine Grundklasse, mit der Clients den Endpunkt eines Diensts zur Laufzeit ermitteln können. Das Ermitteln des Endpunkts eines Diensts wird in Verbindung mit Service Fabric als *Dienstendpunktauflösung*bezeichnet.

Für die Verbindung mit Diensten in einem Cluster kann `ServicePartitionResolver` mithilfe der Standardeinstellungen erstellt werden. Dies ist die empfohlene Verwendung für die meisten Szenarien:

```csharp
ServicePartitionResolver resolver = ServicePartitionResolver.GetDefault();
```

Für die Verbindung mit Diensten in unterschiedlichen Clustern kann `ServicePartitionResolver` mit einer Gruppe von Clustergatewayendpunkten erstellt werden. Dabei sind die Gatewayendpunkte lediglich verschiedene Endpunkte für die Verbindung mit dem gleichen Cluster. Beispiel:

```csharp
ServicePartitionResolver resolver = new  ServicePartitionResolver("mycluster.cloudapp.azure.com:19000", "mycluster.cloudapp.azure.com:19001");
```

Alternativ kann `ServicePartitionResolver` eine Funktion zum Erstellen eines `FabricClient`-Objekts zur internen Verwendung zugewiesen werden: 

```csharp
public delegate FabricClient CreateFabricClientDelegate();
```

`FabricClient` ist das Objekt, das für die Kommunikation mit dem Service Fabric-Cluster für verschiedene Verwaltungsvorgänge im Cluster verwendet wird. Dies ist nützlich, wenn Sie mehr Kontrolle darüber wünschen, wie `ServicePartitionResolver` mit dem Cluster interagiert. `FabricClient` führt die Zwischenspeicherung intern durch und ist in der Regel aufwendig in der Erstellung. Daher ist es wichtig, möglichst viele `FabricClient`-Instanzen wiederzuverwenden. 

```csharp
ServicePartitionResolver resolver = new  ServicePartitionResolver(() => CreateMyFabricClient());
```

Anschließend wird eine resolve-Methode verwendet, um die Adresse eines Diensts oder einer Dienstpartition (bei partitionierten Diensten) abzurufen.

```csharp
ServicePartitionResolver resolver = ServicePartitionResolver.GetDefault();

ResolvedServicePartition partition =
    await resolver.ResolveAsync(new Uri("fabric:/MyApp/MyService"), new ServicePartitionKey(), cancellationToken);
```

Die Adresse eines Diensts kann unter Verwendung von `ServicePartitionResolver` einfach aufgelöst werden. Es sind jedoch weitere Schritte erforderlich, um sicherzustellen, dass die aufgelöste Adresse richtig verwendet werden kann. Der Client muss ermitteln, ob der Verbindungsversuch aufgrund eines vorübergehenden Fehlers fehlgeschlagen ist und wiederholt werden kann (z.B. wenn der Dienst verschoben wurde oder vorübergehend nicht verfügbar ist) oder ob ein permanenter Fehler vorliegt (wenn der Dienst z.B. gelöscht wurde, oder die angeforderte Ressource nicht mehr vorhanden ist). Dienstinstanzen oder -replikate können aus unterschiedlichen Gründen jederzeit zwischen verschiedenen Knoten verschoben werden. Zum Zeitpunkt des Verbindungsversuchs durch den Clientcode kann die über `ServicePartitionResolver` aufgelöste Adresse des Diensts daher schon veraltet sein. In diesem Fall muss der Client die Adresse erneut auflösen. Dies gilt unter der Voraussetzung, dass das vorherige `ResolvedServicePartition`-Element angibt, dass der Resolver nicht nur eine zwischengespeicherte Adresse abruft, sondern einen erneuten Versuch starten muss.

In der Regel muss der Clientcode nicht direkt mit dem `ServicePartitionResolver` -Element funktionieren. Das Element wird erstellt und an Kommunikationsclientfactorys in der Reliable Services-API übergeben. Die Factorys verwenden den Resolver intern, um ein Clientobjekt zu generieren, das für die Kommunikation mit Diensten genutzt werden kann.

### <a name="communication-clients-and-factories"></a>Kommunikationsclients und -factorys
Die Kommunikationsfactorybibliothek implementiert ein typisches Wiederholungsmuster zur Fehlerbehandlung, das die Wiederholung von Verbindungsversuchen mit aufgelösten Dienstendpunkten vereinfacht. Die Factorybibliothek stellt den Wiederholungsmechanismus bereit, und Sie geben die Fehlerhandler an.

`ICommunicationClientFactory` definiert die von einer Kommunikationsclientfactory implementierte Basisschnittstelle, die Clients erstellt, die mit einem Service Fabric-Dienst kommunizieren können. Die Implementierung von CommunicationClientFactory hängt vom Kommunikationsstapel ab, den der Service Fabric-Dienst verwendet, über den der Client kommunizieren möchte. Die Reliable Services-API stellt ein `CommunicationClientFactoryBase<TCommunicationClient>`-Element bereit. Dieses Element bietet eine grundlegende Implementierung der `ICommunicationClientFactory`-Schnittstelle und führt Tasks aus, die für alle Kommunikationsstapel gelten. (Zu diesen Tasks gehört die Verwendung von `ServicePartitionResolver` zum Ermitteln des Dienstendpunkts.) Clients implementieren in der Regel die abstrakte Klasse CommunicationClientFactoryBase, um die für den Kommunikationsstapel spezifische Logik zu verarbeiten.

Der Kommunikationsclient erhält nur eine Adresse und verwendet sie zum Herstellen einer Verbindung mit einem Dienst. Der Client kann jedes beliebige Protokoll verwenden.

```csharp
class MyCommunicationClient : ICommunicationClient
{
    public ResolvedServiceEndpoint Endpoint { get; set; }

    public string ListenerName { get; set; }

    public ResolvedServicePartition ResolvedServicePartition { get; set; }
}
```

Die Clientfactory ist in erster Linie für die Herstellung der Kommunikation mit Clients zuständig. Bei Clients, die keine permanente Verbindung aufrechterhalten (z.B. HTTP-Clients), muss die Factory lediglich den Client erstellen und zurückgeben. Andere Protokolle, die eine permanente Verbindung aufrechterhalten (z.B. bestimmte binäre Protokolle), müssen von der Factory ebenfalls überprüft werden, um zu ermitteln, ob die Verbindung neu erstellt werden muss.  

```csharp
public class MyCommunicationClientFactory : CommunicationClientFactoryBase<MyCommunicationClient>
{
    protected override void AbortClient(MyCommunicationClient client)
    {
    }

    protected override Task<MyCommunicationClient> CreateClientAsync(string endpoint, CancellationToken cancellationToken)
    {
    }

    protected override bool ValidateClient(MyCommunicationClient clientChannel)
    {
    }

    protected override bool ValidateClient(string endpoint, MyCommunicationClient client)
    {
    }
}
```

Schließlich ermittelt ein Ausnahmehandler, welche Aktion beim Auftreten einer Ausnahme ausgeführt werden muss. Ausnahmen werden in **wiederholbare** und **nicht wiederholbare** Ausnahmen unterteilt. 

* **Nicht wiederholbare** Ausnahmen werden einfach an den Aufrufer zurückgeleitet. 
* **Wiederholbare** Ausnahmen werden weiter in **vorübergehende** und **nicht vorübergehende** Ausnahmen unterteilt.
  * **Vorübergehende** Ausnahmen sind Ausnahmen, die einfach wiederholt werden können, ohne dass die Dienstendpunktadresse erneut aufgelöst werden muss. Diese Ausnahmen beruhen z.B. auf vorübergehenden Netzwerkproblemen oder Dienstfehlerantworten, sofern diese nicht angeben, dass die Dienstendpunktadresse nicht vorhanden ist. 
  * **Nicht vorübergehende** Ausnahmen sind Ausnahmen, bei denen die Dienstendpunktadresse erneut aufgelöst werden muss. Dazu gehören Ausnahmen, die angeben, dass der Dienstendpunkt nicht erreicht werden konnte, was darauf zurückzuführen ist, dass der Dienst in einen anderen Knoten verschoben wurde. 

`TryHandleException` trifft eine Entscheidung im Hinblick auf eine bestimmte Ausnahme. Wenn die Entscheidungen im Hinblick auf eine Ausnahme **unklar** sind, sollte **false** zurückgegeben werden. Wenn die Entscheidung **klar** ist, sollte das Ergebnis entsprechend festgelegt und **true** zurückgegeben werden.

```csharp
class MyExceptionHandler : IExceptionHandler
{
    public bool TryHandleException(ExceptionInformation exceptionInformation, OperationRetrySettings retrySettings, out ExceptionHandlingResult result)
    {
        // if exceptionInformation.Exception is known and is transient (can be retried without re-resolving)
        result = new ExceptionHandlingRetryResult(exceptionInformation.Exception, true, retrySettings, retrySettings.DefaultMaxRetryCount);
        return true;


        // if exceptionInformation.Exception is known and is not transient (indicates a new service endpoint address must be resolved)
        result = new ExceptionHandlingRetryResult(exceptionInformation.Exception, false, retrySettings, retrySettings.DefaultMaxRetryCount);
        return true;

        // if exceptionInformation.Exception is unknown (let the next IExceptionHandler attempt to handle it)
        result = null;
        return false;
    }
}
```
### <a name="putting-it-all-together"></a>Gesamtbild
Mit `ICommunicationClient`, `ICommunicationClientFactory` und `IExceptionHandler` und einem Kommunikationsprotokoll führt `ServicePartitionClient` alle diese Komponenten zusammen und stellt die Schleife zur Fehlerbehandlung und Auflösung von Dienstpartitionsadressen für diese Komponenten bereit.

```csharp
private MyCommunicationClientFactory myCommunicationClientFactory;
private Uri myServiceUri;

var myServicePartitionClient = new ServicePartitionClient<MyCommunicationClient>(
    this.myCommunicationClientFactory,
    this.myServiceUri,
    myPartitionKey);

var result = await myServicePartitionClient.InvokeWithRetryAsync(async (client) =>
   {
      // Communicate with the service using the client.
   },
   CancellationToken.None);

```

## <a name="next-steps"></a>Nächste Schritte
* Ein Beispiel für die HTTP-Kommunikation zwischen Diensten finden Sie im [Beispielprojekt auf GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/master/Services/WordCount).
* [Remoteprozeduraufrufe mit Reliable Services-Remoting](service-fabric-reliable-services-communication-remoting.md)
* [Web-API, die OWIN in Reliable Services verwendet](service-fabric-reliable-services-communication-webapi.md)
* [WCF-Kommunikation mit Reliable Services](service-fabric-reliable-services-communication-wcf.md)




<!--HONumber=Nov16_HO3-->


