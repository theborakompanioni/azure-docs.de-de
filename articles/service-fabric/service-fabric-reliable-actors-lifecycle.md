---
title: "Übersicht über den Lebenszyklus von Actor-basierten Azure-Microservices | Microsoft-Dokumentation"
description: "Erläutert den Service Fabric Reliable Actor-Lebenszyklus, Garbage Collection und das manuelle Löschen von Actors und ihren Zuständen"
services: service-fabric
documentationcenter: .net
author: amanbha
manager: timlt
editor: vturecek
ms.assetid: b91384cc-804c-49d6-a6cb-f3f3d7d65a8e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/02/2017
ms.author: amanbha
translationtype: Human Translation
ms.sourcegitcommit: 7033955fa9c18b2fa1a28d488ad5268d598de287
ms.openlocfilehash: 22f906de37ad7ae2a48acf26be26f2af1e3bde7a
ms.lasthandoff: 01/24/2017


---
# <a name="actor-lifecycle-automatic-garbage-collection-and-manual-delete"></a>Actor-Lebenszyklus, automatische Garbage Collection und manuelles Löschen
Ein Actor wird aktiviert, wenn zum ersten Mal eine seiner Methoden aufgerufen wird. Ein Actor wird deaktiviert (Garbage Collection durch die Actors-Laufzeit), wenn er innerhalb eines konfigurierbaren Zeitraums nicht verwendet wird. Ein Actor und sein Zustand können jederzeit auch manuell gelöscht werden.

## <a name="actor-activation"></a>Actor-Aktivierung
Wenn ein Actor aktiviert wird, geschieht Folgendes:

* Wenn ein Aufruf für einen Actor eingeht und er nicht bereits aktiv ist, wird ein neuer Actor erstellt.
* Der Zustand des Actors wird geladen, wenn zuvor ein Zustand gespeichert wurde.
* Die Methode `OnActivateAsync` (die bei der Actor-Implementierung überschrieben werden kann) wird aufgerufen.
* Der Actor gilt jetzt als aktiv.

## <a name="actor-deactivation"></a>Actor-Deaktivierung
Wenn ein Actor deaktiviert wird, geschieht Folgendes:

* Wenn ein Actor einige Zeit lang nicht verwendet wird, wird er aus der Tabelle der aktiven Actors entfernt.
* Die Methode `OnDeactivateAsync` (die bei der Actor-Implementierung überschrieben werden kann) wird aufgerufen. Dadurch werden alle Timer für den Actor gelöscht. Actor-Vorgänge wie Statusänderungen dürfen von dieser Methode nicht aufgerufen werden.

> [!TIP]
> Die Fabric Actors-Laufzeit gibt einige [Ereignisse im Zusammenhang mit der Actor-Aktivierung und -Deaktivierung](service-fabric-reliable-actors-diagnostics.md#list-of-events-and-performance-counters) aus. Sie sind hilfreich bei der Diagnose und Leistungsüberwachung.
>
>

### <a name="actor-garbage-collection"></a>Actor-Garbage Collection
Wenn ein Actor deaktiviert wird, werden Referenzen zum Actor-Objekt veröffentlicht und es kann eine normale Garbage Collection durch den CLR-Garbage Collector (Common Language Runtime) durchgeführt werden. Die Garbage Collection bereinigt nur das Actor-Objekt und entfernt **nicht** den Zustand im Zustands-Manager des Actors. Wenn der Actor das nächste Mal aktiviert ist, wird ein neues Actor-Objekt erstellt und sein Zustand wiederhergestellt.

Was zählt im Sinne der Deaktivierung und Garbage Collection als „wird verwendet“?

* Empfangen eines Aufrufs
* `IRemindable.ReceiveReminderAsync` (gilt nur, wenn der Actor Erinnerungen verwendet).

> [!NOTE]
> Falls der Actor Timer verwendet und der Timer-Rückruf aufgerufen wird, zählt er **nicht** als "wird verwendet".
>
>

Bevor das Thema „Deaktivierung“ ausführlicher behandelt wird, ist es wichtig, die folgenden Begriffe zu definieren:

* *Scanintervall*. Das Intervall, in dem die Actors-Laufzeit ihre Tabelle der aktiven Actors nach Actors durchsucht, die deaktiviert werden können und für die eine Garbage Collection durchgeführt werden kann. Der Standardwert dafür ist 1 Minute.
* *Leerlauftimeout*. Die Zeitspanne, in der ein Actor inaktiv (im Leerlauf) sein muss, ehe er deaktiviert und eine Garbage Collection ausgeführt werden kann. Der Standardwert hierfür sind 60 Minuten.

In der Regel müssen Sie diese Standardeinstellungen nicht ändern. Falls erforderlich, können diese Intervalle jedoch durch `ActorServiceSettings` geändert werden, wenn Sie Ihren [Actordienst](service-fabric-reliable-actors-platform.md)registrieren.

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        ActorRuntime.RegisterActorAsync<MyActor>((context, actorType) =>
                new ActorService(context, actorType,
                    settings:
                        new ActorServiceSettings()
                        {
                            ActorGarbageCollectionSettings =
                                new ActorGarbageCollectionSettings(10, 2)
                        }))
            .GetAwaiter()
            .GetResult();
    }
}
```

Für jeden aktiven Actor verfolgt die Actors-Laufzeit die Zeitspanne, während der er sich im Leerlauf befand (d.h. nicht verwendet wurde). Die Actors-Laufzeit überprüft jeden Actor in einem festgelegten Abstand (`ScanIntervalInSeconds`), um festzustellen, ob eine Garbage Collection für ihn durchgeführt werden kann, und sammelt ihn auf, wenn er sich seit einem gewissen Zeitraum (`IdleTimeoutInSeconds`) im Leerlauf befindet.

Bei jeder Verwendung eines Actors wird seine Leerlaufzeit auf 0 zurückgesetzt. Danach kann für den Actor nur dann eine Garbage Collection ausgeführt werden, wenn er erneut `IdleTimeoutInSeconds`lang im Leerlauf bleibt. Denken Sie daran, dass ein Actor als verwendet gilt, wenn entweder eine Actor-Schnittstellenmethode oder ein Actor-Erinnerungsrückruf ausgeführt wird. Ein Actor gilt **nicht** als verwendet, wenn der Timer-Rückruf ausgeführt wird.

Das folgende Diagramm zeigt den Lebenszyklus eines einzelnen Actors, um diese Konzepte zu verdeutlichen.

![Beispiel für Leerlaufzeit][1]

Das Beispiel zeigt die Auswirkung von Actor-Methodenaufrufen, Erinnerungen und Timern auf die Lebensdauer dieses Actors. Die folgenden Punkte im Hinblick auf das Beispiel sind besonders erwähnenswert:

* „ScanInterval“ und „IdleTimeout“ werden auf 5 und 10 festgelegt. (Einheiten sind hier nicht wichtig, da nur das Konzept veranschaulicht werden soll.)
* Der Scanvorgang für Actors, für die eine Garbage Collection ausgeführt werden soll, erfolgt in den Intervallen T = 0, 5, 10, 15, 20, 25, wie durch den Scanintervallwert 5 definiert.
* Ein periodischer Timer wird bei T =&4;,&8;,&12;,&16;,&20;,&24; ausgelöst, und der Rückruf wird ausgeführt. Auf die Leerlaufzeit des Actors hat dies keine Auswirkungen.
* Ein Actor-Methodenaufruf bei T = 7 setzt die Leerlaufzeit auf 0 zurück und verzögert die Ausführung der Garbage Collection für den Actor.
* Ein Actor-Erinnerungs-Rückruf wird bei T =&14; ausgeführt und verzögert die Garbage Collection des Actors noch weiter.
* Während des Garbage Collection-Scanvorgangs bei T = 25 überschreitet die Leerlaufzeit des Actors schließlich den Leerlauftimeout-Wert von 10, und die Garbage Collection wird für den Actor ausgeführt.

Für einen Actor wird nie eine Garbage Collection durchgeführt, während er eine seiner Methoden ausführt, unabhängig davon, wie viel Zeit die Ausführung dieser Methode beansprucht. Wie bereits erwähnt, verhindert die Ausführung von Actor-Schnittstellenmethoden und Erinnerungs-Rückrufen die Durchführung der Garbage Collection, indem die Leerlaufzeit des Actors auf 0 zurückgesetzt wird. Durch die Ausführung von Timer-Rückrufen wird die Leerlaufzeit nicht auf 0 zurückgesetzt. Allerdings wird die Garbage Collection des Actors verzögert, bis der Timer-Rückruf abgeschlossen ist.

## <a name="deleting-actors-and-their-state"></a>Löschen von Actors und deren Zustände
Eine Garbage Collection von deaktivierten Actors bereinigt nur das Actor-Objekt, entfernt jedoch keine Daten, die im Zustands-Manager eines Actors gespeichert sind. Wenn ein Actor wieder aktiviert wird, werden ihm seine Daten durch den Zustands-Manager wieder zur Verfügung gestellt. In Fällen, in denen Actors Daten im Zustands-Manager speichern, dann deaktiviert aber nie wieder aktiviert werden, kann die Bereinigung Ihrer Daten nötig sein.

Der [Actordienst](service-fabric-reliable-actors-platform.md) bietet auch eine Funktion zum Löschen von Actors über einen Remoteanrufer:

```csharp
ActorId actorToDelete = new ActorId(id);

IActorService myActorServiceProxy = ActorServiceProxy.Create(
    new Uri("fabric:/MyApp/MyService"), actorToDelete);

await myActorServiceProxy.DeleteActorAsync(actorToDelete, cancellationToken)
```

Das Löschen eines Actors hat die folgenden Auswirkungen, abhängig davon, ob der Actor derzeit aktiv ist oder nicht:

* **Aktiver Actor**
  * Der Actor wird aus der Liste der aktiven Actors entfernt und deaktiviert.
  * Sein Zustand wird dauerhaft gelöscht.
* **Inaktiver Actor**
  * Sein Zustand wird dauerhaft gelöscht.

Beachten Sie, dass ein Actor für sich selbst durch eine seiner Methoden „delete“ aufrufen kann. Der Actor kann nämlich während einer Ausführung innerhalb eines Actor-Aufrufkontexts, in dem die Laufzeit eine Sperre um den Actor-Aufruf erhalten hat, um den Singlethread-Zugang zu erzwingen, nicht gelöscht werden.

## <a name="next-steps"></a>Nächste Schritte
* [Actor-Timer und -Erinnerungen](service-fabric-reliable-actors-timers-reminders.md)
* [Actor-Ereignisse](service-fabric-reliable-actors-events.md)
* [Actor-Eintrittsinvarianz](service-fabric-reliable-actors-reentrancy.md)
* [Actor-Diagnose und -Leistungsüberwachung](service-fabric-reliable-actors-diagnostics.md)
* [Actor-API-Referenzdokumentation](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [Beispielcode](https://github.com/Azure/servicefabric-samples)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-lifecycle/garbage-collection.png

