---
title: Reliable Actors-Zustandsverwaltung | Microsoft Docs
description: "Hier wird beschrieben, wie der Reliable Actors-Zustand für hohe Verfügbarkeit verwaltet, persistent gespeichert und repliziert wird."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 37cf466a-5293-44c0-a4e0-037e5d292214
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.translationtype: Human Translation
ms.sourcegitcommit: 6efa2cca46c2d8e4c00150ff964f8af02397ef99
ms.openlocfilehash: aca8cf2b94e8b746a5cac6af021c7221a29b7345
ms.contentlocale: de-de
ms.lasthandoff: 07/01/2017


---
# <a name="reliable-actors-state-management"></a>Reliable Actors-Zustandsverwaltung
Reliable Actors sind Singlethread-Objekte, die sich zum Kapseln von Logik und Zustand eignen. Da Akteure unter Reliable Services ausgeführt werden, können Sie den Zustand zuverlässig beibehalten, indem sie die gleiche Persistenz- und Replikationsmechanismen wie Reliable Services verwenden. Daher verlieren Akteure ihren Zustand nicht nach Fehlern, bei der Reaktivierung nach einer Garbage Collection oder beim Verschieben zwischen Knoten in einem Cluster aufgrund von Ressourcenausgleich oder Upgrades.

## <a name="state-persistence-and-replication"></a>Zustandspersistenz und -replikation
Alle Reliable Actors werden als *zustandsbehaftet* betrachtet, da jeder Akteurinstanz eine eindeutige ID zugewiesen ist. Das bedeutet, dass wiederholte Aufrufe derselben Akteur-ID an dieselbe Akteurinstanz weitergeleitet werden. Bei einem zustandslosen System werden Clientaufrufe dagegen nicht unbedingt jedes Mal an denselben Server weitergeleitet. Aus diesem Grund sind Akteurdienste immer zustandsbehaftete Dienste.

Obwohl Akteure als zustandsbehaftet gelten, bedeutet das nicht, dass sie den Zustand zuverlässig speichern müssen. Akteure können die Ebene der Zustandspersistenz und -replikation basierend auf ihren Datenspeicheranforderungen auswählen:

* **Persistenter Zustand**: Der Zustand wird persistent auf einem Datenträger gespeichert, und es werden mindestens drei Replikate davon erstellt. Dies ist die dauerhafteste Option für die Zustandsspeicherung, mit der der Zustand bei einem vollständigen Clusterausfall beibehalten werden kann.
* **Flüchtiger Zustand**: Es werden mindestens drei Replikate des Zustands erstellt, und der Zustand wird nur im Arbeitsspeicher gespeichert. Dadurch wird Ausfallsicherheit bei Knotenausfällen, Akteurausfällen, Upgrades und Ressourcenausgleich gewährleistet. Der Zustand wird jedoch nicht persistent auf einem Datenträger gespeichert. Wenn alle Replikate gleichzeitig verloren gehen, geht auch der Zustand verloren.
* **Kein persistenter Zustand**: Der Zustand wird weder repliziert noch auf den Datenträger geschrieben. Diese Ebene eignet sich für Akteure, die den Zustand nicht zuverlässig beibehalten müssen.

Jede Persistenzebene stellt einfach einen anderen *Zustandsanbieter* und eine andere *Replikationskonfiguration* Ihres Diensts dar. Ob der Zustand auf einen Datenträger geschrieben wird, ist abhängig vom Zustandsanbieter – der Komponente in einer Reliable Services-Instanz, die den Zustand speichert. Die Replikation hängt davon ab, mit wie vielen Replikaten ein Dienst bereitgestellt wird. Wie bei Reliable Services können der Zustandsanbieter und die Replikatanzahl einfach manuell festgelegt werden. Das Akteurframework enthält ein Attribut, das bei Verwendung für einen Akteur automatisch einen Standardzustandsanbieter auswählt und Einstellungen für die Replikatanzahl erstellt, um eine der folgenden drei Persistenzeinstellungen zu erreichen. Das StatePersistence-Attribut wird nicht von abgeleiteten Klasse geerbt. Jeder Typ „Actor“ muss seinen „StatePersistence“-Grad angeben.

### <a name="persisted-state"></a>Persistenter Zustand
```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl  extends FabricActor implements MyActor
{
}
```  
Für diese Einstellung wird ein Zustandsanbieter verwendet, der Daten auf dem Datenträger speichert und automatisch die Anzahl der Replikat auf drei festlegt.

### <a name="volatile-state"></a>Flüchtiger Zustand
```csharp
[StatePersistence(StatePersistence.Volatile)]
class MyActor : Actor, IMyActor
{
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Volatile)
class MyActorImpl extends FabricActor implements MyActor
{
}
```
Diese Einstellung verwendet einen rein speicherinternen Zustandsanbieter und legt die Replikatanzahl auf drei fest.

### <a name="no-persisted-state"></a>Kein persistenter Zustand
```csharp
[StatePersistence(StatePersistence.None)]
class MyActor : Actor, IMyActor
{
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.None)
class MyActorImpl extends FabricActor implements MyActor
{
}
```
Diese Einstellung verwendet einen rein speicherinternen Zustandsanbieter und legt die Replikatanzahl auf eins fest.

### <a name="defaults-and-generated-settings"></a>Standardeinstellungen und generierte Einstellungen
Bei Verwendung des `StatePersistence`-Attributs wird zur Laufzeit automatisch ein Zustandsanbieter für Sie ausgewählt, wenn der Akteurdienst gestartet wird. Die Replikatanzahl wird jedoch zum Zeitpunkt der Kompilierung von den Visual Studio-Akteur-Buildtools festgelegt. Die Buildtools generieren in der Datei „ApplicationManifest.xml“ automatisch einen *Standarddienst* für den Actordienst. Für die **Mindestgröße der Replikatgruppe** und die **Zielgröße der Replikatgruppe** werden Parameter erstellt.

Sie können diese Parameter manuell ändern. Bei jeder Änderung des `StatePersistence`-Attributs werden die Parameter jedoch auf die Standardwerte der Replikatgruppengröße für das ausgewählte `StatePersistence`-Attribut festgelegt. Dabei werden vorherige Werte überschrieben. Das bedeutet, dass die in „ServiceManifest.xml“ festgelegten Werte *nur* zum Erstellungszeitpunkt überschrieben werden, wenn Sie den Attributwert `StatePersistence` ändern.

```xml
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="Application12Type" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Parameters>
      <Parameter Name="MyActorService_PartitionCount" DefaultValue="10" />
      <Parameter Name="MyActorService_MinReplicaSetSize" DefaultValue="3" />
      <Parameter Name="MyActorService_TargetReplicaSetSize" DefaultValue="3" />
   </Parameters>
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyActorPkg" ServiceManifestVersion="1.0.0" />
   </ServiceManifestImport>
   <DefaultServices>
      <Service Name="MyActorService" GeneratedIdRef="77d965dc-85fb-488c-bd06-c6c1fe29d593|Persisted">
         <StatefulService ServiceTypeName="MyActorServiceType" TargetReplicaSetSize="[MyActorService_TargetReplicaSetSize]" MinReplicaSetSize="[MyActorService_MinReplicaSetSize]">
            <UniformInt64Partition PartitionCount="[MyActorService_PartitionCount]" LowKey="-9223372036854775808" HighKey="9223372036854775807" />
         </StatefulService>
      </Service>
   </DefaultServices>
</ApplicationManifest>
```

## <a name="state-manager"></a>Zustands-Manager
Jede Akteurinstanz verfügt über einen eigenen Zustands-Manager: Dabei handelt es sich um eine wörterbuchähnliche Datenstruktur, die zuverlässig Schlüssel-Wert-Paare speichert. Der Zustands-Manager ist ein Wrapper für einen Zustandsanbieter. Sie können ihn unabhängig von der verwendeten Persistenzeinstellung zum Speichern von Daten verwenden. Es kann nicht garantiert werden, dass sich ein ausgeführter Akteurdienst mithilfe eines parallelen Upgrades und unter Beibehaltung der Daten von einem flüchtigen (ausschließlich speicherinternen) Zustand in einen persistenten Zustand überführen lässt. Die Replikatanzahl kann jedoch für einen ausgeführten Dienst geändert werden.

Schlüssel des Zustands-Managers müssen Zeichenfolgen sein. Werte sind generisch und können von beliebiger Art (u. a. benutzerdefiniert) sein. Im Zustands-Manager gespeicherte Werte müssen für den Datenvertrag serialisierbar sein, da sie abhängig von der Zustandspersistenzeinstellung eines Akteurs während der Replikation möglicherweise über das Netzwerk an andere Knoten übertragen und auf Datenträger geschrieben werden.

Der Zustands-Manager macht allgemeine Wörterbuchmethoden zum Verwalten des Zustands verfügbar. Diese ähneln den Methoden in Reliable Dictionary.

### <a name="accessing-state"></a>Zugreifen auf den Zustand
Auf den Zustand kann nach Schlüssel über den Zustands-Manager zugegriffen werden. Alle Zustands-Manager-Methoden sind asynchron, da sie u.U. Datenträger-E/A erfordern, wenn Akteure über einen persistenten Zustand verfügen. Beim ersten Zugriff werden Zustandsobjekte im Arbeitsspeicher zwischengespeichert. Bei wiederholten Zugriffsvorgängen wird direkt aus dem Speicher auf Objekte zugegriffen, und die Rückgabe erfolgt synchron, ohne dass dabei Datenträger-E/A entstehen oder Aufwand für den asynchronen Kontextwechsel anfällt. In den folgenden Fällen wird ein Zustandsobjekt aus dem Cache entfernt:

* Eine Akteurmethode löst nach dem Abrufen eines Objekts aus dem Zustands-Manager eine nicht behandelte Ausnahme aus.
* Ein Akteur wird entweder nach seiner Deaktivierung oder nach einem Fehler wieder aktiviert.
* Der Zustandsanbieter lagert den Zustand an den Datenträger aus. Dieses Verhalten hängt von der Implementierung des Zustandsanbieters ab. Der Standardzustandsanbieter für die Einstellung `Persisted` weist dieses Verhalten auf.

Der Zustand kann mit einem *Get*-Standardvorgang abgerufen werden, der `KeyNotFoundException`(C#) oder `NoSuchElementException`(Java) auslöst, wenn kein Eintrag für den Schlüssel vorhanden ist:

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task<int> GetCountAsync()
    {
        return this.StateManager.GetStateAsync<int>("MyState");
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture<Integer> getCountAsync()
    {
        return this.stateManager().getStateAsync("MyState");
    }
}
```

Der Zustand kann auch mit einer *TryGet*-Methode abgerufen werden, die keine Ausnahme auslöst, wenn kein Eintrag für einen Schlüssel vorhanden ist:

```csharp
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public async Task<int> GetCountAsync()
    {
        ConditionalValue<int> result = await this.StateManager.TryGetStateAsync<int>("MyState");
        if (result.HasValue)
        {
            return result.Value;
        }

        return 0;
    }
}
```
```Java
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture<Integer> getCountAsync()
    {
        return this.stateManager().<Integer>tryGetStateAsync("MyState").thenApply(result -> {
            if (result.hasValue()) {
                return result.getValue();
            } else {
                return 0;
            });
    }
}
```

### <a name="saving-state"></a>Speichern des Zustands
Mit den Abrufmethoden des Zustands-Managers wird ein Verweis auf ein Objekt im lokalen Speicher zurückgegeben. Eine Änderung dieses Objekts im lokalen Speicher bewirkt nicht, dass es dauerhaft gespeichert wird. Wenn ein Objekt vom Zustands-Manager abgerufen und geändert wird, muss es wieder in den Zustands-Manager eingefügt werden, damit es dauerhaft gespeichert wird.

Der Zustand kann über eine nicht bedingte *Set`dictionary["key"] = value`-Methode eingefügt werden. Dies entspricht der Syntax* :

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task SetCountAsync(int value)
    {
        return this.StateManager.SetStateAsync<int>("MyState", value);
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture setCountAsync(int value)
    {
        return this.stateManager().setStateAsync("MyState", value);
    }
}
```

Sie können den Zustand mithilfe einer *Add*-Methode hinzufügen. Diese Methode löst `InvalidOperationException`(C#) oder `IllegalStateException`(Java) aus, wenn sie versucht, einen Schlüssel hinzuzufügen, die bereits vorhanden ist.

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task AddCountAsync(int value)
    {
        return this.StateManager.AddStateAsync<int>("MyState", value);
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture addCountAsync(int value)
    {
        return this.stateManager().addOrUpdateStateAsync("MyState", value, (key, old_value) -> old_value + value);
    }
}
```

Sie können den Zustand auch mithilfe einer *TryAdd*-Methode hinzufügen. Diese Methode löst keine Ausnahme aus, wenn sie versucht, einen Schlüssel hinzuzufügen, die bereits vorhanden ist.

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public async Task AddCountAsync(int value)
    {
        bool result = await this.StateManager.TryAddStateAsync<int>("MyState", value);

        if (result)
        {
            // Added successfully!
        }
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture addCountAsync(int value)
    {
        return this.stateManager().tryAddStateAsync("MyState", value).thenApply((result)->{
            if(result)
            {
                // Added successfully!
            }
        });
    }
}
```

Am Ende einer Akteurmethode speichert der Zustands-Manager automatisch alle Werte, die in einem Einfüge- oder Aktualisierungsvorgang hinzugefügt oder geändert wurden. Je nach verwendeten Einstellungen kann ein Speichervorgang das persistente Speichern auf Datenträgern und die Replikation beinhalten. Werte, die nicht geändert wurden, werden nicht persistent gespeichert oder repliziert. Wenn keine Werte geändert wurden, geschieht beim Speichervorgang nichts. Tritt beim Speichern ein Fehler auf, wird der geänderte Zustand verworfen und der Ursprungszustand wieder geladen.

Der Zustand kann durch Aufrufen der `SaveStateAsync`-Methode für die Akteurbasis auch manuell gespeichert werden:

```csharp
async Task IMyActor.SetCountAsync(int count)
{
    await this.StateManager.AddOrUpdateStateAsync("count", count, (key, value) => count > value ? count : value);

    await this.SaveStateAsync();
}
```
```Java
interface MyActor {
    CompletableFuture setCountAsync(int count)
    {
        this.stateManager().addOrUpdateStateAsync("count", count, (key, value) -> count > value ? count : value).thenApply();

        this.stateManager().saveStateAsync().thenApply();
    }
}
```

### <a name="removing-state"></a>Entfernen des Zustands
Der Zustand kann durch Aufrufen der *Remove*-Methode dauerhaft aus dem Zustands-Manager eines Akteurs entfernt werden. Diese Methode löst `KeyNotFoundException`(C#) oder `NoSuchElementException`(Java) aus, wenn sie versucht, einen Schlüssel zu entfernen, die nicht vorhanden ist.

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task RemoveCountAsync()
    {
        return this.StateManager.RemoveStateAsync("MyState");
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture removeCountAsync()
    {
        return this.stateManager().removeStateAsync("MyState");
    }
}
```

Sie können den Zustand mithilfe der *TryRemove*-Methode auch permanent entfernen. Diese Methode löst keine Ausnahme aus, wenn sie versucht, einen Schlüssel zu entfernen, der nicht vorhanden ist.

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public async Task RemoveCountAsync()
    {
        bool result = await this.StateManager.TryRemoveStateAsync("MyState");

        if (result)
        {
            // State removed!
        }
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture removeCountAsync()
    {
        return this.stateManager().tryRemoveStateAsync("MyState").thenApply((result)->{
            if(result)
            {
                // State removed!
            }
        });
    }
}
```

## <a name="next-steps"></a>Nächste Schritte

Zustand, der in Reliable Actors gespeichert wurde, muss serialisiert werden, bevor er in den Datenträger geschrieben und zur hohen Verfügbarkeit repliziert wird. Erfahren Sie mehr über [Typserialisierung von Actors](service-fabric-reliable-actors-notes-on-actor-type-serialization.md).

Und erfahren Sie mehr über [Diagnose und Leistungsüberwachung für Reliable Actors](service-fabric-reliable-actors-diagnostics.md).

