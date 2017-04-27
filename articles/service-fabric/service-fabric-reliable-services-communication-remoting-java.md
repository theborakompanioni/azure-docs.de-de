---
title: Dienstremoting in Azure Service Fabric | Microsoft-Dokumentation
description: "Service Fabric-Remoting ermöglicht Clients und Diensten die Kommunikation mit Diensten über einen Remoteprozeduraufruf."
services: service-fabric
documentationcenter: java
author: PavanKunapareddyMSFT
manager: timlt
ms.assetid: 
ms.service: service-fabric
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 03/09/2017
ms.author: pakunapa
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 11e300b3b1d0433bd4790332593ada2d3eede883
ms.lasthandoff: 04/03/2017


---
# <a name="service-remoting-with-reliable-services"></a>Dienstremoting mit Reliable Services
> [!div class="op_single_selector"]
> * [C# unter Windows](service-fabric-reliable-services-communication-remoting.md)
> * [Java unter Linux](service-fabric-reliable-services-communication-remoting-java.md)
>
>

Das Reliable Services-Framework stellt einen Remotingmechanismus bereit, mit dem schnell und einfach ein Remoteprozeduraufruf für Dienste eingerichtet werden kann.

## <a name="set-up-remoting-on-a-service"></a>Einrichten von Remoting für einen Dienst
Die Einrichtung von Remoting für einen Dienst erfolgt in zwei einfachen Schritten:

1. Erstellen Sie eine Schnittstelle, die vom Dienst implementiert werden soll. Diese Schnittstelle definiert die Methoden, die für den Remoteprozeduraufruf für Ihren Dienst verfügbar sind. Bei den Methoden muss es sich um asynchrone Methoden handeln, die einen Task zurückgeben. Die Schnittstelle muss `microsoft.serviceFabric.services.remoting.Service` implementieren, um zu signalisieren, dass der Dienst über eine Remotingschnittstelle verfügt.
2. Verwenden Sie einen Remoting-Listener in Ihrem Dienst. Dies ist eine `CommunicationListener` -Implementierung, die Remotingfunktionen bereitstellt. `FabricTransportServiceRemotingListener` kann verwendet werden, um einen Remotinglistener zu erstellen, der das standardmäßige Remoting-Transportprotokoll nutzt.

Der folgende zustandslose Dienst macht beispielsweise eine einzelne Methode verfügbar, um „Hello World“ per Remoteprozeduraufruf abzurufen:

```java
import java.util.ArrayList;
import java.util.concurrent.CompletableFuture;
import java.util.List;
import microsoft.servicefabric.services.communication.runtime.ServiceInstanceListener;
import microsoft.servicefabric.services.remoting.Service;
import microsoft.servicefabric.services.runtime.StatelessService;

public interface MyService extends Service {
    CompletableFuture<String> helloWorldAsync();
}

class MyServiceImpl extends StatelessService implements MyService {
    public MyServiceImpl(StatelessServiceContext context) {
       super(context);
    }

    public CompletableFuture<String> helloWorldAsync() {
        return CompletableFuture.completedFuture("Hello!");
    }

    @Override
    protected List<ServiceInstanceListener> createServiceInstanceListeners() {
        ArrayList<ServiceInstanceListener> listeners = new ArrayList<>();
        listeners.add(new ServiceInstanceListener((context) -> {
            return new FabricTransportServiceRemotingListener(context,this);
        }));
        return listeners;
    }
}
```

> [!NOTE]
> Bei den Argumenten und Rückgabetypen in der Dienstschnittstelle kann es sich um einfache, komplexe oder benutzerdefinierte Typen handeln. Sie müssen jedoch serialisierbar sein.
>
>

## <a name="call-remote-service-methods"></a>Aufrufen von Remotedienstmethoden
Methoden für einen Dienst, der den Remotingstapel nutzt, werden mithilfe eines lokalen Proxys für den Dienst über die `microsoft.serviceFabric.services.remoting.client.ServiceProxyBase` -Klasse abgerufen. Die `ServiceProxyBase` -Methode erstellt einen lokalen Proxy unter Verwendung der gleichen Schnittstelle, die auch der Dienst implementiert. Mit diesem Proxy können Sie Methoden für die Schnittstelle einfach remote aufrufen.

```java

MyService helloWorldClient = ServiceProxyBase.create(MyService.class, new URI("fabric:/MyApplication/MyHelloWorldService"));

CompletableFuture<String> message = helloWorldClient.helloWorldAsync();

```

Das Remotingframework gibt beim Dienst aufgetretene Ausnahmen an den Client weiter. Somit kann die Ausnahmebehandlungslogik auf dem Client Ausnahmen, die vom Dienst ausgegeben werden, mithilfe von `ServiceProxyBase` direkt behandeln.

## <a name="next-steps"></a>Nächste Schritte
* [Absichern der Kommunikation für Reliable Services](service-fabric-reliable-services-secure-communication.md)

