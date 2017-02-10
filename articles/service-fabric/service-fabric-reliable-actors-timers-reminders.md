---
title: "Timer und Erinnerungen für Reliable Actors | Microsoft Docs"
description: "Einführung in Timer und Erinnerungen für Service Fabric Reliable Actors."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: amanbha
ms.assetid: 00c48716-569e-4a64-bd6c-25234c85ff4f
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/19/2016
ms.author: vturecek
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 56b2e13c3bf053175e357a627d45a91d9a9a4ba9


---
# <a name="actor-timers-and-reminders"></a>Actor-Timer und -Erinnerungen
Akteure können regelmäßige Arbeit an sich selbst einplanen, indem sie entweder Timer oder Erinnerungen registrieren. In diesem Artikel wird gezeigt, wie Timer und Erinnerungen verwendet werden können, und es werden die Unterschiede zwischen ihnen erklärt.

## <a name="actor-timers"></a>Actor-Timer
Actor-Timer bieten einen einfachen Wrapper um .NET-Timer, um sicherzustellen, dass die Rückrufmethoden die von der Actors-Laufzeit bereitgestellten wechselseitigen Parallelitätsgarantien respektieren.

Actor können sich mithilfe der Methoden `RegisterTimer` und `UnregisterTimer` ihrer Basisklasse bei ihren Timern an- und abmelden. Das folgende Beispiel zeigt die Verwendung von Timer-APIs. Die APIs sind dem .NET-Timer sehr ähnlich. In diesem Beispiel wird die `MoveObject` -Methode von der Actors-Laufzeit aufgerufen, wenn der Timer abgelaufen ist. Es ist sichergestellt, dass die Methode die wechselseitige Parallelität respektiert. Dies bedeutet, dass keine anderen Actor-Methoden oder Timer-/Erinnerungsrückrufe ausgeführt werden, bis dieser Rückruf die Ausführung abgeschlossen hat.

```csharp
class VisualObjectActor : Actor, IVisualObject
{
    private IActorTimer _updateTimer;

    public VisualObjectActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    protected override Task OnActivateAsync()
    {
        ...

        _updateTimer = RegisterTimer(
            MoveObject,                     // Callback method
            null,                           // Parameter to pass to the callback method
            TimeSpan.FromMilliseconds(15),  // Amount of time to delay before the callback is invoked
            TimeSpan.FromMilliseconds(15)); // Time interval between invocations of the callback method

        return base.OnActivateAsync();
    }

    protected override Task OnDeactivateAsync()
    {
        if (_updateTimer != null)
        {
            UnregisterTimer(_updateTimer);
        }

        return base.OnDeactivateAsync();
    }

    private Task MoveObject(object state)
    {
        ...
        return Task.FromResult(true);
    }
}
```

Der nächste Timerzeitraum beginnt nach Abschluss des Rückrufs. Dies bedeutet, dass der Timer während des Rückrufs angehalten und nach Abschluss des Rückrufs erneut gestartet wird.

Die Actors-Laufzeit speichert die Änderungen, die im Zustands-Manager des Actors vorgenommen wurden, wenn der Rückruf endet. Tritt ein Fehler beim Speichern des Zustands auf, wird das Actor-Objekt deaktiviert und eine neue Instanz aktiviert. 

Alle Timer werden angehalten, wenn der Actor im Rahmen einer Garbage Collection deaktiviert wird. Danach werden keine Timerrückrufe mehr aufgerufen. Zur Actor-Laufzeit werden zudem keine Informationen über die Timer gespeichert, die vor der Deaktivierung ausgeführt wurden. Es ist die Aufgabe des Actors, alle für zukünftige Reaktivierungen erforderlichen Timer zu registrieren. Weitere Informationen finden Sie im Abschnitt zur [Actor Garbage Collection](service-fabric-reliable-actors-lifecycle.md).

## <a name="actor-reminders"></a>Actor-Erinnerungen
Erinnerungen sind ein Mechanismus zum Auslösen persistenter Rückrufe für einen Actor zu bestimmten Zeiten. Die Funktionalität ähnelt dabei Timern. Im Gegensatz zu Timern werden Erinnerungen aber unter allen Umständen ausgelöst, bis ihre Registrierung vom Actor explizit aufgehoben oder der Actor explizit gelöscht wird. Erinnerungen werden insbesondere über Actor-Deaktivierungen und Failover ausgelöst, da zur Actor-Laufzeit Informationen über die Erinnerungen des Actors gespeichert werden.

Zum Registrieren einer Erinnerung ruft ein Actor die in der Basisklasse bereitgestellte `RegisterReminderAsync` -Methode auf, wie im folgenden Beispiel gezeigt:

```csharp
protected override async Task OnActivateAsync()
{
    string reminderName = "Pay cell phone bill";
    int amountInDollars = 100;

    IActorReminder reminderRegistration = await this.RegisterReminderAsync(
        reminderName,
        BitConverter.GetBytes(amountInDollars),
        TimeSpan.FromDays(3),
        TimeSpan.FromDays(1));
}
```

In diesem Beispiel ist `"Pay cell phone bill"` der Name der Erinnerung. Dies ist eine Zeichenfolge, die der Actor zur eindeutigen Identifizierung einer Erinnerung verwendet. `BitConverter.GetBytes(amountInDollars)` ist der mit der Erinnerung verknüpfte Kontext. Sie wird an den Actor als Argument für den Erinnerungsrückruf zurückgegeben, d. h. `IRemindable.ReceiveReminderAsync`.

Actors, die Erinnerungen verwenden, müssen die `IRemindable` -Schnittstelle implementieren, wie im folgenden Beispiel gezeigt.

```csharp
public class ToDoListActor : Actor, IToDoListActor, IRemindable
{
    public ToDoListActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task ReceiveReminderAsync(string reminderName, byte[] context, TimeSpan dueTime, TimeSpan period)
    {
        if (reminderName.Equals("Pay cell phone bill"))
        {
            int amountToPay = BitConverter.ToInt32(context, 0);
            System.Console.WriteLine("Please pay your cell phone bill of ${0}!", amountToPay);
        }
        return Task.FromResult(true);
    }
}
```

Wenn eine Erinnerung ausgelöst wird, ruft die Reliable Actors-Laufzeit die `ReceiveReminderAsync`-Methode auf dem Actor auf. Ein Actor kann mehrere Erinnerungen registrieren. Die `ReceiveReminderAsync`-Methode wird aufgerufen, sobald eine dieser Erinnerungen ausgelöst wurde. Der Actor kann mithilfe des an die Methode `ReceiveReminderAsync` übergebenen Erinnerungsnamens ermitteln, welche Erinnerung ausgelöst wurde.

Die Actors-Laufzeit speichert den Actor-Zustand nach Abschluss des Aufrufs `ReceiveReminderAsync` . Tritt ein Fehler beim Speichern des Zustands auf, wird das Actor-Objekt deaktiviert und eine neue Instanz aktiviert. 

Zum Aufheben der Registrierung einer Erinnerung ruft ein Actor die `UnregisterReminderAsync`-Methode auf, wie im folgenden Beispiel gezeigt.

```csharp
IActorReminder reminder = GetReminder("Pay cell phone bill");
Task reminderUnregistration = UnregisterReminderAsync(reminder);
```

Wie oben gezeigt, akzeptiert die Methode `UnregisterReminderAsync` die Schnittstelle `IActorReminder`. Die Basisklasse des Actors unterstützt die Methode `GetReminder`, die zum Abrufen der Schnittstelle `IActorReminder` durch Übergabe des Erinnerungsnamens verwendet werden kann. Dies ist praktisch, da der Actor die Schnittstelle `IActorReminder` nicht beibehalten muss, die bei Aufruf der Methode `RegisterReminder` zurückgegeben wurde.

## <a name="next-steps"></a>Nächste Schritte
* [Actor-Ereignisse](service-fabric-reliable-actors-events.md)
* [Actor-Eintrittsinvarianz](service-fabric-reliable-actors-reentrancy.md)
* [Actor-Diagnose und -Leistungsüberwachung](service-fabric-reliable-actors-diagnostics.md)
* [Actor-API-Referenzdokumentation](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [Beispielcode](https://github.com/Azure/servicefabric-samples)




<!--HONumber=Nov16_HO3-->


