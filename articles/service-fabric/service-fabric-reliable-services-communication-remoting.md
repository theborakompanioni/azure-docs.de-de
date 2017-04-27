---
title: Dienstremoting in Azure Service Fabric | Microsoft-Dokumentation
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
ms.date: 02/10/2017
ms.author: vturecek
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 8e06b3f2f6347468b197f2e90912a5d0facc5404
ms.lasthandoff: 04/03/2017


---
# <a name="service-remoting-with-reliable-services"></a>Dienstremoting mit Reliable Services
> [!div class="op_single_selector"]
> * [C# unter Windows](service-fabric-reliable-services-communication-remoting.md)
> * [Java unter Linux](service-fabric-reliable-services-communication-remoting-java.md)
>
>

Für WebAPI, WCF (Windows Communication Foundation) und andere Dienste, die nicht an ein bestimmtes Kommunikationsprotokoll oder einen bestimmten Kommunikationsstapel gebunden sind, stellt das Reliable Services-Framework einen Remotingmechanismus für das schnelle, einfache Einrichten von Remoteprozeduraufrufen für Dienste bereit.

## <a name="set-up-remoting-on-a-service"></a>Einrichten von Remoting für einen Dienst
Die Einrichtung von Remoting für einen Dienst erfolgt in zwei einfachen Schritten:

1. Erstellen Sie eine Schnittstelle, die vom Dienst implementiert werden soll. Diese Schnittstelle definiert die Methoden, die für den Remoteprozeduraufruf für Ihren Dienst verfügbar sind. Bei den Methoden muss es sich um asynchrone Methoden handeln, die einen Task zurückgeben. Die Schnittstelle muss `Microsoft.ServiceFabric.Services.Remoting.IService` implementieren, um zu signalisieren, dass der Dienst über eine Remotingschnittstelle verfügt.
2. Verwenden Sie einen Remoting-Listener in Ihrem Dienst. Dies ist eine `ICommunicationListener` -Implementierung, die Remotingfunktionen bereitstellt. Der `Microsoft.ServiceFabric.Services.Remoting.Runtime`-Namespace enthält eine Erweiterungsmethode `CreateServiceRemotingListener` sowohl für zustandslose als auch für zustandsbehaftete Dienste, die zum Erstellen eines Remoting-Listener mit dem Standard-Remoting-Transportprotokoll verwendet werden kann.

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
        return new[] { new ServiceInstanceListener(context =>
            this.CreateServiceRemotingListener(context)) };
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

## <a name="next-steps"></a>Nächste Schritte
* [Web-API mit OWIN in Reliable Services](service-fabric-reliable-services-communication-webapi.md)
* [WCF-Kommunikation mit Reliable Services](service-fabric-reliable-services-communication-wcf.md)
* [Absichern der Kommunikation für Reliable Services](service-fabric-reliable-services-secure-communication.md)

