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
ms.date: 06/30/2017
ms.author: pakunapa
ms.translationtype: Human Translation
ms.sourcegitcommit: 6efa2cca46c2d8e4c00150ff964f8af02397ef99
ms.openlocfilehash: dc4a362b5737bb424ca2c196c85f4c51b6ee5e30
ms.contentlocale: de-de
ms.lasthandoff: 07/01/2017


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

## <a name="service-proxy-lifetime"></a>Gültigkeitsdauer von Dienstproxys
Die Erstellung von Dienstproxys ist ein einfacher Vorgang, sodass Benutzer so viele erstellen können, wie sie benötigen. Ein Dienstproxy kann erneut verwendet werden, solange der Benutzer ihn benötigt. Der Benutzer kann den gleichen Proxy im Fall einer Ausnahme erneut verwenden. Jeder Dienstproxy enthält einen Kommunikationsclient zum Senden von Nachrichten im Netzwerk. Beim Aufrufen der API erfolgt eine interne Prüfung, ob der verwendete Kommunikationsclient gültig ist. Basierend auf diesem Ergebnis erstellen wir den Kommunikationsclient neu. Daher müssen Benutzer im Fall einer Ausnahme den Dienstproxy nicht neu erstellen.

### <a name="serviceproxyfactory-lifetime"></a>Gültigkeitsdauer von „ServiceProxyFactory“
[FabricServiceProxyFactory](https://docs.microsoft.com/en-us/java/api/microsoft.servicefabric.services.remoting.client._fabric_service_proxy_factory) ist eine Factory, die einen Proxy für verschiedene Remotingschnittstellen erstellt. Wenn Sie die API `ServiceProxyBase.create` verwenden, um Proxys zu erstellen, erstellt das Framework eine `FabricServiceProxyFactory`.
Es ist sinnvoll, eine Factory manuell zu erstellen, wenn Sie [IServiceRemotingClientFactory](https://docs.microsoft.com/en-us/java/api/microsoft.servicefabric.services.remoting.client._service_remoting_client_factory)-Eigenschaften überschreiben müssen.
„Factory“ ist ein aufwendiger Vorgang. `FabricServiceProxyFactory` verwaltet den Cache von Kommunikationsclients.
Eine bewährte Methode besteht darin, `FabricServiceProxyFactory` solange wie möglich zwischenzuspeichern.

## <a name="remoting-exception-handling"></a>Behandlung von Remotingausnahmen
Alle von der Dienstverwaltungs-API ausgelösten remoten Ausnahmen werden entweder als „RuntimeException“ oder „FabricException“ an den Client zurückgesendet.

„ServiceProxy“ verarbeitet sämtliche Failoverausnahmen für die Dienstpartition, für die seine Erstellung erfolgt ist. Dieser Proxy löst die Endpunkte erneut auf, falls Failoverausnahmen (nicht vorübergehende Ausnahmen) vorliegen, und wiederholt den Aufruf mit dem richtigen Endpunkt. Die Anzahl der Wiederholungen bei Failoverausnahmen ist unbegrenzt.
Im Falle vorübergehender Ausnahmen wird nur der Aufruf wiederholt.

Standardparameter für die Wiederholung werden von [OperationRetrySettings] angegeben. (https://docs.microsoft.com/en-us/java/api/microsoft.servicefabric.services.communication.client._operation_retry_settings) Der Benutzer kann diese Werte konfigurieren, indem er das Objekt „OperationRetrySettings“ an den Konstruktor „ServiceProxyFactory“ übergibt.

## <a name="next-steps"></a>Nächste Schritte
* [Absichern der Kommunikation für Reliable Services](service-fabric-reliable-services-secure-communication.md)

