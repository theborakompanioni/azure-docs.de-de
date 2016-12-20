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
ms.date: 10/19/2016
ms.author: vturecek
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 194935398809b1905ddc4100b5f09fce7f75a796


---
# <a name="reliable-actors-state-management"></a>Reliable Actors-Zustandsverwaltung
Reliable Actors sind Singlethread-Objekte, die sich zum Kapseln von Logik und Zustand eignen. Da Akteure unter Reliable Services ausgeführt werden, können Sie den Zustand zuverlässig beibehalten, indem sie die gleiche Persistenz- und Replikationsmechanismen wie Reliable Services verwenden. Daher verlieren Akteure ihren Zustand nicht nach Fehlern, nach der Reaktivierung nach einer Garbage Collection oder beim Verschieben zwischen Knoten in einem Cluster aufgrund von Ressourcenausgleich oder Upgrades.

## <a name="state-persistence-and-replication"></a>Zustandspersistenz und -replikation
Alle Reliable Actors werden als *zustandsbehaftet* betrachtet, da jeder Akteurinstanz eine eindeutige ID zugewiesen ist. Das bedeutet, dass wiederholte Aufrufe derselben Akteur-ID an dieselbe Akteurinstanz weitergeleitet werden. Hier besteht ein Unterschied zu einem zustandslosen System, bei dem Clientaufrufe nicht unbedingt jedes Mal an denselben Server weitergeleitet werden. Aus diesem Grund sind Akteurdienste immer zustandsbehaftete Dienste.

Obwohl Akteure als zustandsbehaftet gelten, bedeutet das nicht, dass sie den Zustand zuverlässig speichern müssen. Akteure können die Ebene der Zustandspersistenz und -replikation basierend auf ihren Datenspeicheranforderungen auswählen:

* **Persistenter Zustand:** Der Zustand wird persistent auf einem Datenträger gespeichert, und es werden mindestens drei Replikate davon erstellt. Dies ist die dauerhafteste Option für die Zustandsspeicherung, mit der der Zustand bei einem vollständigen Clusterausfall beibehalten werden kann.
* **Flüchtiger Zustand:** Es werden mindestens drei Replikate des Zustands erstellt, und der Zustand wird nur im Arbeitsspeicher gespeichert. Dadurch wird Ausfallsicherheit bei Knotenausfällen, Akteurausfällen, Upgrades und Ressourcenausgleich gewährleistet. Der Zustand wird jedoch nicht persistent auf einem Datenträger gespeichert. Wenn alle Replikate gleichzeitig verloren gehen, geht auch der Zustand verloren.
* **Kein persistenter Zustand:** Der Zustand wird weder repliziert noch auf den Datenträger geschrieben. Dies eignet sich für Akteure, die den Zustand nicht zuverlässig beibehalten müssen.

Jede Persistenzebene stellt einfach einen anderen *Zustandsanbieter* und eine andere *Replikationskonfiguration* Ihres Diensts dar. Ob der Zustand auf einen Datenträger geschrieben wird, ist abhängig vom *Zustandsanbieter* – der Komponente in einer Reliable Services-Instanz, die den Zustand speichert. Die Replikation hängt davon ab, mit wie vielen Replikaten ein Dienst bereitgestellt wird. Wie bei Reliable Services können der Zustandsanbieter und die Replikatanzahl einfach manuell festgelegt werden. Das Akteurframework enthält ein Attribut, das bei Verwendung für einen Akteur automatisch einen Standardzustandsanbieter auswählt und Einstellungen für die Replikatanzahl erstellt, um eine der folgenden drei Persistenzeinstellungen zu erreichen.

### <a name="persisted-state"></a>Persistenter Zustand
```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
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
Diese Einstellung verwendet einen rein speicherinternen Zustandsanbieter und legt die Replikatanzahl auf drei fest.

### <a name="no-persisted-state"></a>Kein persistenter Zustand
```csharp
[StatePersistence(StatePersistence.None)]
class MyActor : Actor, IMyActor
{
}
```
Diese Einstellung verwendet einen rein speicherinternen Zustandsanbieter und legt die Replikatanzahl auf eins fest.

### <a name="defaults-and-generated-settings"></a>Standardeinstellungen und generierte Einstellungen
Bei Verwendung des `StatePersistence`-Attributs wird zur Laufzeit automatisch ein Zustandsanbieter für Sie ausgewählt, wenn der Actordienst gestartet wird. Die Replikatanzahl wird jedoch zum Zeitpunkt der Kompilierung von den Visual Studio-Akteur-Buildtools festgelegt. Die Buildtools generieren in der Datei „ApplicationManifest.xml“ automatisch einen *Standarddienst* für den Actordienst. Für die **Mindestgröße der Replikatgruppe** und die **Zielgröße der Replikatgruppe** werden Parameter erstellt. Sie können diese Parameter natürlich manuell ändern. Bei jeder Änderung des `StatePersistence`-Attributs werden die Parameter jedoch auf die Standardwerte der Replikatgruppengröße für das ausgewählte `StatePersistence`-Attribut festgelegt. Dabei werden vorherige Werte überschrieben. Das bedeutet, dass die in „ServiceManifest.xml“ festgelegten Werte **nur** zum Erstellungszeitpunkt überschrieben werden, wenn Sie den Attributwert `StatePersistence` ändern. 

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
Jede Akteurinstanz verfügt über einen eigenen Zustands-Manager: Dabei handelt es sich um eine wörterbuchähnliche Datenstruktur, die zuverlässig Schlüssel-Wert-Paare speichert. Der Zustands-Manager ist ein Wrapper für einen Zustandsanbieter. Er kann unabhängig von der verwendeten Persistenzeinstellung zum Speichern von Daten genutzt werden. Es kann jedoch nicht garantiert werden, dass sich ein ausgeführter Akteurdienst mithilfe eines parallelen Upgrades und unter Beibehaltung der Daten von einem flüchtigen (ausschließlich speicherinternen) Zustand in einen persistenten Zustand überführen lässt. Die Replikatanzahl kann jedoch für einen ausgeführten Dienst geändert werden. 

Die Zustands-Manager-Schlüssel müssen als Zeichenfolge angegeben werden, wohingegen Werte generisch sind und von beliebiger Art (u.a. benutzerdefiniert) sein können. Im Zustands-Manager gespeicherte Werte müssen für den Datenvertrag serialisierbar sein, da sie abhängig von der Zustandspersistenzeinstellung eines Akteurs während der Replikation möglicherweise über das Netzwerk an andere Knoten übertragen und auf Datenträger geschrieben werden. 

Der Zustands-Manager macht allgemeine Wörterbuchmethoden zum Verwalten des Zustands verfügbar. Diese ähneln den Methoden in Reliable Dictionary.

### <a name="accessing-state"></a>Zugreifen auf den Zustand
Auf den Zustand kann nach Schlüssel über den Zustands-Manager zugegriffen werden. Alle Zustands-Manager-Methoden sind asynchron, da sie u.U. Datenträger-E/A erfordern, wenn Akteure über einen persistenten Zustand verfügen. Beim ersten Zugriff werden Zustandsobjekte im Arbeitsspeicher zwischengespeichert. Bei wiederholten Zugriffsvorgängen wird direkt aus dem Speicher auf Objekte zugegriffen, und die Rückgabe erfolgt synchron, ohne dass dabei Datenträger-E/A entstehen oder Aufwand für den asynchronen Kontextwechsel anfällt. In den folgenden Fällen wird ein Zustandsobjekt aus dem Cache entfernt:

* Eine Akteurmethode löst nach dem Abrufen eines Objekts aus dem Zustands-Manager eine nicht behandelte Ausnahme aus.
* Ein Akteur wird entweder nach seiner Deaktivierung oder aufgrund eines Fehlers wieder aktiviert.
* Wenn der Zustandsanbieter den Zustand an den Datenträger auslagert. Dieses Verhalten hängt von der Implementierung des Zustandsanbieters ab. Der Standardzustandsanbieter für die Einstellung `Persisted` weist dieses Verhalten auf. 

Der Zustand kann mit einem *Get*-Standardvorgang abgerufen werden, der `KeyNotFoundException` auslöst, wenn kein Eintrag für den angegebenen Schlüssel vorhanden ist: 

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

Der Zustand kann auch mit einer *TryGet* -Methode abgerufen werden, die keine Ausnahme auslöst, wenn kein Eintrag für einen bestimmten Schlüssel vorhanden ist:

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

### <a name="saving-state"></a>Speichern des Zustands
Mit den Abrufmethoden des Zustands-Managers wird ein Verweis auf ein Objekt im lokalen Speicher zurückgegeben. Eine Änderung dieses Objekts im lokalen Speicher bewirkt nicht, dass es dauerhaft gespeichert wird. Wenn ein Objekt vom Zustands-Manager abgerufen und geändert wird, muss es wieder in den Zustands-Manager eingefügt werden, damit es dauerhaft gespeichert wird.

Der Zustand kann über eine nicht bedingte *Set`dictionary["key"] = value`-Methode eingefügt werden. Dies entspricht der Syntax *:

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

Der Zustand kann mithilfe der *Add*-Methode hinzugefügt werden, die beim Versuch, einen bereits vorhandenen Schlüssel hinzuzufügen, `InvalidOperationException` auslöst:

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

Der Zustand kann auch mithilfe der *TryAdd* -Methode hinzugefügt werden, die beim Versuch, einen bereits vorhandenen Schlüssel hinzuzufügen, keine Ausnahme auslöst:

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

Am Ende einer Akteurmethode speichert der Zustands-Manager automatisch alle Werte, die in einem Einfüge- oder Aktualisierungsvorgang hinzugefügt oder geändert wurden. Je nach verwendeten Einstellungen kann ein Speichervorgang das persistente Speichern auf Datenträgern und die Replikation beinhalten. Werte, die nicht geändert wurden, werden nicht persistent gespeichert oder repliziert. Wenn keine Werte geändert wurden, geschieht beim Speichervorgang nichts. Tritt beim Speichern ein Fehler auf, wird der geänderte Zustand verworfen und der Ursprungszustand wieder geladen.

Der Zustand kann durch Aufrufen der `SaveStateAsync` -Methode für die Actor-Basis auch manuell gespeichert werden:

```csharp
async Task IMyActor.SetCountAsync(int count)
{
    await this.StateManager.AddOrUpdateStateAsync("count", count, (key, value) => count > value ? count : value);

    await this.SaveStateAsync();
}
```

### <a name="removing-state"></a>Entfernen des Zustands
Der Zustand kann durch Aufrufen der *Remove* -Methode dauerhaft aus dem Zustands-Manager eines Actors entfernt werden . Diese Methode löst beim Versuch, einen nicht vorhandenen Schlüssel zu entfernen, `KeyNotFoundException` aus:

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

Der Zustand kann auch mit der *TryRemove* -Methode dauerhaft entfernt werden, die keine Ausnahme auslöst, wenn Sie versuchen, einen nicht vorhandenen Schlüssel zu entfernen:

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

## <a name="next-steps"></a>Nächste Schritte
* [Akteurtypserialisierung](service-fabric-reliable-actors-notes-on-actor-type-serialization.md)
* [Actor-Polymorphie und objektorientierte Entwurfsmuster](service-fabric-reliable-actors-polymorphism.md)
* [Actor-Diagnose und -Leistungsüberwachung](service-fabric-reliable-actors-diagnostics.md)
* [Actor-API-Referenzdokumentation](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [Beispielcode](https://github.com/Azure/servicefabric-samples)




<!--HONumber=Nov16_HO3-->


