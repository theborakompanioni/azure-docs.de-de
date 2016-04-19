<properties
   pageTitle="Reliable Actors-Eintrittsinvarianz | Microsoft Azure"
   description="Einführung in Eintrittsinvarianz für Service Fabric Reliable Actors"
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor="amanbha"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="03/25/2016"
   ms.author="vturecek"/>


# Reliable Actors-Eintrittsinvarianz
Die Reliable Actors-Laufzeit erlaubt bei logischen Aufrufen standardmäßig kontextbasierte Eintrittsinvarianz. Dadurch können Actors eintrittsinvariant sein, wenn sie sich in der selben Aufrufkontextkette befinden. Beispiel: Actor A sendet eine Nachricht an Actor B und dieser wiederum sendet eine Nachricht an Actor C. Wenn Actor C Actor A aufruft, wird im Rahmen der Nachrichtenverarbeitung zugelassen, dass die Nachricht eintrittsinvariant ist. Alle weiteren Nachrichten, die zu unterschiedlichen Aufrufkontexten gehören, werden auf Actor A blockiert, bis dieser die Verarbeitung abgeschlossen hat.

Actors, die kontextbasierte Eintrittsinvarianz bei logischen Aufrufen nicht zulassen möchten, können diese Option deaktivieren, indem sie der Actor-Klasse `ReentrantAttribute(ReentrancyMode.Disallowed)` hinzufügen.

```csharp
public enum ReentrancyMode
{
    LogicalCallContext = 1,
    Disallowed = 2
}
```

Der folgende Code zeigt eine Actor-Klasse, die den Eintrittsinvarianz-Modus auf `ReentrancyMode.Disallowed` festlegt. Wenn in diesem Fall ein Actor eine eintrittsinvariante Nachricht an einen anderen Actor sendet, wird eine Ausnahme vom Typ `FabricException` ausgelöst.

```csharp
[Reentrant(ReentrancyMode.Disallowed)]
class MyActor : Actor, IMyActor
{
    ...
}
```

## Nächste Schritte
 - [Actor-Diagnose und -Leistungsüberwachung](service-fabric-reliable-actors-diagnostics.md)
 - [Actor-API-Referenzdokumentation](https://msdn.microsoft.com/library/azure/dn971626.aspx)
 - [Beispielcode](https://github.com/Azure/servicefabric-samples)

<!---HONumber=AcomDC_0406_2016-->