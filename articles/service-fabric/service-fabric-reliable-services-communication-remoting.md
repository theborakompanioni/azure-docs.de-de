
---
title: Dienstremoting in Service Fabric | Microsoft Docs
description: "Service Fabric-Remoting ermöglicht Clients und Diensten die Kommunikation mit Diensten über einen Remoteprozeduraufruf."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: BharatNarasimman
ms.assetid: abfaf430-fea0-4974-afba-cfc9f9f2354b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 04/20/2017
ms.author: vturecek
ms.translationtype: Human Translation
ms.sourcegitcommit: 138f04f8e9f0a9a4f71e43e73593b03386e7e5a9
ms.openlocfilehash: 92a8894f24c234fbf38eda086531b524cceccfc1
ms.contentlocale: de-de
ms.lasthandoff: 06/29/2017


---
# <a name="service-remoting-with-reliable-services"></a>Dienstremoting mit Reliable Services
Für WebAPI, WCF (Windows Communication Foundation) und andere Dienste, die nicht an ein bestimmtes Kommunikationsprotokoll oder einen bestimmten Kommunikationsstapel gebunden sind, stellt das Reliable Services-Framework einen Remotingmechanismus für das schnelle, einfache Einrichten von Remoteprozeduraufrufen für Dienste bereit.

## <a name="set-up-remoting-on-a-service"></a>Einrichten von Remoting für einen Dienst
Die Einrichtung von Remoting für einen Dienst erfolgt in zwei einfachen Schritten:

1. Erstellen Sie eine Schnittstelle, die vom Dienst implementiert werden soll. Diese Schnittstelle definiert die Methoden, die für den Remoteprozeduraufruf für Ihren Dienst verfügbar sind. Bei den Methoden muss es sich um asynchrone Methoden handeln, die einen Task zurückgeben. Die Schnittstelle muss `Microsoft.ServiceFabric.Services.Remoting.IService` implementieren, um zu signalisieren, dass der Dienst über eine Remotingschnittstelle verfügt.
2. Verwenden Sie einen Remoting-Listener in Ihrem Dienst. Dies ist eine `ICommunicationListener` -Implementierung, die Remotingfunktionen bereitstellt. Der `Microsoft.ServiceFabric.Services.Remoting.Runtime`-Namespace enthält die Erweiterungsmethode `CreateServiceRemotingListener` sowohl für zustandslose als auch für zustandsbehaftete Dienste, die zum Erstellen eines Remotinglisteners mit dem standardmäßigen Remotingtransportprotokoll verwendet werden kann.

Hinweis: Der `Remoting`-Namespace steht als separates NuGet-Paket mit dem Namen `Microsoft.ServiceFabric.Services.Remoting` zur Verfügung. 

Der folgende zustandslose Dienst macht beispielsweise eine einzelne Methode verfügbar, um „Hello World“ per Remoteprozeduraufruf abzurufen:

```csharp
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Microsoft.ServiceFabric.Services.Remoting;
using Microsoft.ServiceFabric.Services.Remoting.Runtime;
using Microsoft.ServiceFabric.Services.Runtime;

public interface IMyService : IService
{
    Task<string> HelloWorldAsync();
}

class MyService : StatelessService, IMyService
{
    public MyService(StatelessServiceContext context)
        : base (context)
    {
    }

    public Task HelloWorldAsync()
    {
        return Task.FromResult("Hello!");
    }

    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        return new[] { new ServiceInstanceListener(context =>            this.CreateServiceRemotingListener(context)) };
    }
}
```
> [!NOTE]
> Bei den Argumenten und Rückgabetypen in der Dienstschnittstelle kann es sich um einfache, komplexe oder benutzerdefinierte Typen handeln. Sie müssen jedoch durch [DataContractSerializer](https://msdn.microsoft.com/library/ms731923.aspx) von .NET serialisierbar sein.
>
>

## <a name="call-remote-service-methods"></a>Aufrufen von Remotedienstmethoden
Methoden für einen Dienst, der den Remotingstapel nutzt, werden mithilfe eines lokalen Proxys für den Dienst über die `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxy` -Klasse abgerufen. Die `ServiceProxy` -Methode erstellt einen lokalen Proxy unter Verwendung der gleichen Schnittstelle, die auch der Dienst implementiert. Mit diesem Proxy können Sie Methoden für die Schnittstelle einfach remote aufrufen.

```csharp

IMyService helloWorldClient = ServiceProxy.Create<IMyService>(new Uri("fabric:/MyApplication/MyHelloWorldService"));

string message = await helloWorldClient.HelloWorldAsync();

```

Das Remotingframework gibt beim Dienst aufgetretene Ausnahmen an den Client weiter. Somit kann die Ausnahmebehandlungslogik auf dem Client Ausnahmen, die vom Dienst ausgegeben werden, mithilfe von `ServiceProxy` direkt behandeln.

## <a name="service-proxy-lifetime"></a>Gültigkeitsdauer von Dienstproxys
Die Erstellung von Dienstproxys ist ein einfacher Vorgang, sodass Benutzer so viele erstellen können, wie sie benötigen. Ein Dienstproxy kann erneut verwendet werden, solange der Benutzer ihn benötigt. Der Benutzer kann den gleichen Proxy im Fall einer Ausnahme erneut verwenden. Jeder Dienstproxy enthält einen Kommunikationsclient zum Senden von Nachrichten im Netzwerk. Beim Aufrufen der API erfolgt eine interne Prüfung, ob der verwendete Kommunikationsclient gültig ist. Basierend auf diesem Ergebnis erstellen wir den Kommunikationsclient neu. Daher müssen Benutzer im Fall einer Ausnahme den Dienstproxy nicht neu erstellen.

### <a name="serviceproxyfactory-lifetime"></a>Gültigkeitsdauer von „ServiceProxyFactory“
[ServiceProxyFactory](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.remoting.client.serviceproxyfactory) ist eine Factory, die einen Proxy für verschiedene Remotingschnittstellen erstellt. Bei Verwenden der API „ServiceProxy.Create“ zum Erstellen des Proxys erzeugt das Framework die ServiceProxyFactory als Singleton.
Es ist sinnvoll, eine manuell zu erstellen, wenn Sie [IServiceRemotingClientFactory](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.remoting.client.iserviceremotingclientfactory)-Eigenschaften überschreiben müssen.
„Factory“ ist ein aufwendiger Vorgang. „ServiceProxyFactory“ verwaltet den Cache des Kommunikationsclients.
Es empfiehlt sich, „ServiceProxyFactory“ so lange wie möglich im Cache zwischenzuspeichern.

## <a name="remoting-exception-handling"></a>Behandlung von Remotingausnahmen
Alle von der Dienst-API ausgelösten Ausnahmen werden als „AggregateException“ an den Client zurückgesendet. „RemoteExceptions“ muss „DataContract Serializable“ sein, da andernfalls [ServiceException](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.communication.serviceexception) für die Proxy-API mit enthaltenem Serialisierungsfehler ausgelöst wird.

„ServiceProxy“ verarbeitet sämtliche Failoverausnahmen für die Dienstpartition, für die seine Erstellung erfolgt ist. Dieser Proxy löst die Endpunkte erneut auf, falls Failoverausnahmen (nicht vorübergehende Ausnahmen) vorliegen, und wiederholt den Aufruf mit dem richtigen Endpunkt. Die Anzahl der Wiederholungen bei Failoverausnahmen ist unbegrenzt.
Im Falle vorübergehender Ausnahmen wird nur der Aufruf wiederholt.

Standardparameter für die Wiederholung werden von [OperationRetrySettings] angegeben. (https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.communication.client.operationretrysettings) Der Benutzer kann diese Werte konfigurieren, indem er das Objekt „OperationRetrySettings“ an den Konstruktor „ServiceProxyFactory“ übergibt.

## <a name="next-steps"></a>Nächste Schritte
* [Web-API mit OWIN in Reliable Services](service-fabric-reliable-services-communication-webapi.md)
* [WCF-Kommunikation mit Reliable Services](service-fabric-reliable-services-communication-wcf.md)
* [Absichern der Kommunikation für Reliable Services](service-fabric-reliable-services-secure-communication.md)

