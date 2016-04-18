<properties
   pageTitle="Polymorphie im Reliable Actors-Framework | Microsoft Azure"
   description="Erstellen Sie Hierarchien von .NET-Schnittstellen und -Typen im Reliable Actors-Framework zur Wiederverwendung von Funktionen und API-Definitionen."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="03/25/2016"
   ms.author="seanmck"/>

# Polymorphie im Reliable Actors-Framework

Das Reliable Actors-Framework ermöglicht es Ihnen, Actors mit vielen der Vorgehensweisen zu erstellen, die Sie auch beim objektorientierten Entwerfen verwenden würden. Eine dieser Techniken ist Polymorphie, bei der Typen und Schnittstellen von allgemeineren übergeordneten Elementen erben. Die Vererbung im Reliable Actors-Framework folgt normalerweise dem .NET-Modell mit einigen zusätzlichen Einschränkungen.

## Schnittstellen

Das Reliable Actors-Framework erfordert die Definition mindestens einer Schnittstelle, die von Ihrem Akteurtyp implementiert wird. Diese Schnittstelle wird verwendet, um eine Proxyklasse zu generieren, die von den Clients zur Kommunikation mit dem Akteur verwendet werden kann. Schnittstellen können von anderen Schnittstellen erben, solange jede durch einen Akteurtyp implementierte sowie alle übergeordneten Schnittstellen letztendlich von IActor abgeleitet werden. Dabei handelt es sich um die durch die Plattform definierte Basisschnittstelle für Akteure. Folglich kann das klassische Polymorphie-Beispiel mithilfe von Formen wie folgt aussehen:

![Schnittstellenhierarchie für Formakteure][shapes-interface-hierarchy]


## Typen

Sie können auch eine Hierarchie von Akteurtypen erstellen, die von der durch die Plattform bereitgestellten Akteurbasisklasse abgeleitet werden. Im Fall von Formen haben Sie möglicherweise einen Basistyp `Shape`:

```csharp
public abstract class Shape : Actor, IShape
{
    public abstract Task<int> GetVerticeCount();
    
    public abstract Task<double> GetAreaAsync();
}
```

Untertypen von `Shape` können Methoden von der Basis außer Kraft setzen.

```csharp
[ActorService(Name = "Circle")]
[StatePersistence(StatePersistence.Persisted)]
public class Circle : Shape, ICircle
{
    public override Task<int> GetVerticeCount()
    {
        return Task.FromResult(0);
    }

    public override async Task<double> GetAreaAsync()
    {
        CircleState state = await this.StateManager.GetStateAsync<CircleState>("circle");

        return Math.PI *
            state.Radius *
            state.Radius;
    }
}
```

Beachten Sie das `ActorService`-Attribut des Akteurtyps. Dieses Attribut teilt dem Reliable Actor-Framework mit, dass es automatisch einen Dienst zum Hosten von Actors dieses Typs erstellen soll. In einigen Fällen sollten Sie möglicherweise einen Basistyp erstellen, der ausschließlich für die Freigabe von Funktionen für Untertypen vorgesehen ist und nicht verwendet wird, um tatsächliche Akteure zu instanziieren. In diesen Fällen sollten Sie das Schlüsselwort `abstract` verwenden, um anzugeben, dass Sie nie einen Akteur basierend auf diesem Typ erstellen werden.


## Nächste Schritte

- Hier finden Sie weitere Informationen darüber, [wie das Reliable Actors-Framework die Service Fabric-Plattform nutzt](service-fabric-reliable-actors-platform.md), um Zuverlässigkeit, Skalierbarkeit und einen konsistenten Zustand zu gewährleisten.
- Erfahren Sie mehr über den [Actor-Lebenszyklus](service-fabric-reliable-actors-lifecycle.md).

<!-- Image references -->

[shapes-interface-hierarchy]: ./media/service-fabric-reliable-actors-polymorphism/Shapes-Interface-Hierarchy.png

## Nächste Schritte
 - [Actor-Zustandsverwaltung](service-fabric-reliable-actors-state-management.md)
 - [Actor-Lebenszyklus und Garbage Collection](service-fabric-reliable-actors-lifecycle.md)
 - [Actor-Timer und -Erinnerungen](service-fabric-reliable-actors-timers-reminders.md)
 - [Actor-Ereignisse](service-fabric-reliable-actors-events.md)
 - [Actor-Eintrittsinvarianz](service-fabric-reliable-actors-reentrancy.md)
 - [Actor-Diagnose und -Leistungsüberwachung](service-fabric-reliable-actors-diagnostics.md)

<!---HONumber=AcomDC_0406_2016-->