<properties
   pageTitle="Dienstremoting in Service Fabric | Microsoft Azure"
   description="Service Fabric-Remoting ermöglicht Clients und Diensten die Kommunikation mit Diensten über einen Remoteprozeduraufruf."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor="BharatNarasimman"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="07/06/2016"
   ms.author="vturecek"/>

# Dienstremoting mit Reliable Services
Für WebAPI, WCF (Windows Communication Foundation) und andere Dienste, die nicht an ein bestimmtes Kommunikationsprotokoll oder einen bestimmten Kommunikationsstapel gebunden sind, stellt das Reliable Services-Framework einen Remotingmechanismus für das schnelle, einfache Einrichten von Remoteprozeduraufrufen für Dienste bereit.

## Einrichten von Remoting für einen Dienst
Die Einrichtung von Remoting für einen Dienst erfolgt in zwei einfachen Schritten:

1. Erstellen Sie eine Schnittstelle, die vom Dienst implementiert werden soll. Diese Schnittstelle definiert die Methoden, die für den Remoteprozeduraufruf für Ihren Dienst verfügbar sind. Bei den Methoden muss es sich um asynchrone Methoden handeln, die einen Task zurückgeben. Die Schnittstelle muss `Microsoft.ServiceFabric.Services.Remoting.IService` implementieren, um zu signalisieren, dass der Dienst über eine Remotingschnittstelle verfügt.
2. Verwenden Sie einen Remoting-Listener in Ihrem Dienst. Dies ist eine `ICommunicationListener`-Implementierung, die Remotingfunktionen bereitstellt. Der `Microsoft.ServiceFabric.Services.Remoting.Runtime`-Namespace enthält eine Erweiterungsmethode `CreateServiceRemotingListener` sowohl für zustandslose als auch für zustandsbehaftete Dienste, die zum Erstellen eines Remoting-Listener mit dem Standard-Remoting-Transportprotokoll verwendet werden kann.

Der folgende zustandslose Dienst macht beispielsweise eine einzelne Methode verfügbar, um „Hello World“ per Remoteprozeduraufruf abzurufen:

```csharp
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Microsoft.ServiceFabric.Services.Remoting;
using Microsoft.ServiceFabric.Services.Remoting.Runtime;
using Microsoft.ServiceFabric.Services.Runtime;

public interface IMyService : IService
{
    Task<string> GetHelloWorld();
}

class MyService : StatelessService, IMyService
{
    public MyService(StatelessServiceContext context)
        : base (context)
    {
    }

    public Task HelloWorld()
    {
        return Task.FromResult("Hello!");
    }

    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        return new[] { new ServiceInstanceListener(context => 
            this.CreateServiceRemotingListener(context)) };
    }
}
```
> [AZURE.NOTE] Bei den Argumenten und Rückgabetypen in der Dienstschnittstelle kann es sich um einfache, komplexe oder benutzerdefinierte Typen handeln. Sie müssen jedoch durch den .NET-[DataContractSerializer](https://msdn.microsoft.com/library/ms731923.aspx) serialisierbar sein.


## Aufrufen von Remotedienstmethoden
Methoden für einen Dienst, der den Remotingstapel nutzt, werden mithilfe eines lokalen Proxys für den Dienst über die `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxy`-Klasse abgerufen. Die `ServiceProxy`-Methode erstellt einen lokalen Proxy unter Verwendung der gleichen Schnittstelle, die auch der Dienst implementiert. Mit diesem Proxy können Sie Methoden für die Schnittstelle einfach remote aufrufen.


```csharp

IMyService helloWorldClient = ServiceProxy.Create<IMyService>(new Uri("fabric:/MyApplication/MyHelloWorldService"));

string message = await helloWorldClient.GetHelloWorld();

```

Das Remotingframework gibt beim Dienst aufgetretene Ausnahmen an den Client weiter. Somit kann die Ausnahmebehandlungslogik auf dem Client Ausnahmen, die vom Dienst ausgegeben werden, mithilfe von `ServiceProxy` direkt behandeln.

## Nächste Schritte

* [Web-API mit OWIN in Reliable Services](service-fabric-reliable-services-communication-webapi.md)

* [WCF-Kommunikation mit Reliable Services](service-fabric-reliable-services-communication-wcf.md)

* [Absichern der Kommunikation für Reliable Services](service-fabric-reliable-services-secure-communication.md)

<!---HONumber=AcomDC_0713_2016-->