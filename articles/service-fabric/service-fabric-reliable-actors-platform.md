---
title: Reliable Actors in Service Fabric | Microsoft Docs
description: Erfahren Sie, wie Reliable Actors auf den Ebenen der Reliable Services angeordnet sind und die Features der Service Fabric-Plattform verwenden.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: amanbha
ms.assetid: 45839a7f-0536-46f1-ae2b-8ba3556407fb
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/10/2017
ms.author: vturecek
translationtype: Human Translation
ms.sourcegitcommit: 6e0ad6b5bec11c5197dd7bded64168a1b8cc2fdd
ms.openlocfilehash: ba07b1bf1b49ebb24a7d5cfbaad71f5f17c49192
ms.lasthandoff: 03/28/2017


---
# <a name="how-reliable-actors-use-the-service-fabric-platform"></a>Verwendung der Service Fabric-Plattform durch Reliable Actors
In diesem Artikel wird die Funktionsweise von Reliable Actors auf der Service Fabric-Plattform beschrieben. Reliable Actors arbeiten in einem Framework, das in einer Implementierung des zustandsbehafteten zuverlässigen Diensts gehostet wird, der als *Actordienst*bezeichnet wird. Der Actordienst enthält alle Komponenten, die zum Verwalten des Lebenszyklus und zum Übermitteln von Nachrichten für Ihre Akteure erforderlich sind:

* Die Actor Runtime verwaltet den Lebenszyklus und die Garbage Collection und erzwingt den Singlethread-Zugriff.
* Ein Actordienst-Remotelistener akzeptiert Remotezugriffsaufrufe für Akteure und sendet diese an einen Verteiler, damit sie an die entsprechende Akteurinstanz weitergeleitet werden.
* Der Akteurzustandsanbieter umschließt Zustandsanbieter (z.B. den Reliable Collections-Zustandsanbieter) und stellt einen Adapter für die Akteurzustandsverwaltung bereit.

Diese Komponenten bilden zusammen das Reliable Actors-Framework. 

## <a name="service-layering"></a>Dienstebenen
Da der Actordienst selbst einer der Reliable Services ist, gelten alle Konzepte für Reliable Services wie [Anwendungsmodell](service-fabric-application-model.md), Lebenszyklus, [Verpackung](service-fabric-package-apps.md), [Bereitstellung](service-fabric-deploy-remove-applications.md), Upgrade und Skalierung auch für Actordienste. 

![Actordienstebenen][1]

Das Diagramm oben zeigt die Beziehung zwischen dem Service Fabric-Anwendungsframework und dem Benutzercode. Blaue Elemente stellen das Reliable Services-Anwendungsframework dar, Orange stellt das Reliable Actors-Framework dar, und Grün steht für den Benutzercode. 

In Reliable Services erbt der Dienst die `StatefulService`-Klasse, die selbst von `StatefulServiceBase` abgeleitet ist. Sie können in Reliable Actors den Actordienst verwenden, der eine andere Implementierung der `StatelessService` -Klasse darstellt, die das Akteurmuster dort implementiert, wo Ihre Akteure ausgeführt werden. Da der Actordienst selbst nur eine Implementierung von `StatefulServiceBase` . Da der Actordienst selbst nur eine Implementierung von `StatefulServiceBase` ist, können Sie einen eigenen Dienst schreiben, der von `ActorService` abgeleitet ist, und Funktionen auf Dienstebene auf die gleiche Weise implementieren wie bei der Vererbung von `StatefulService`. Beispiel:

* Sicherung und Wiederherstellung des Diensts
* Gemeinsame Verwendung von Funktionen für alle Akteure, z.B. ein Schaltkreisunterbrecher
* Remoteprozeduraufrufe für den Actordienst selbst sowie für einzelne Akteure 

### <a name="using-the-actor-service"></a>Verwenden des Actordiensts
Akteurinstanzen haben Zugriff auf den Actordienst, in dem sie ausgeführt werden. Über den Actordienst können Akteurinstanzen programmgesteuert den Dienstkontext mit der Partitions-ID, den Dienstnamen, den Anwendungsnamen und andere spezifische Informationen für die Service Fabric-Plattform abrufen:

```csharp
Task MyActorMethod()
{
    Guid partitionId = this.ActorService.Context.PartitionId;
    string serviceTypeName = this.ActorService.Context.ServiceTypeName;
    Uri serviceInstanceName = this.ActorService.Context.ServiceName;
    string applicationInstanceName = this.ActorService.Context.CodePackageActivationContext.ApplicationName;
}
```

Wie alle Reliable Services muss der Actordienst mit einem Diensttyp in der Service Fabric-Laufzeit registriert werden. Damit der Actordienst Ihre Akteurinstanzen ausführen kann, muss auch Ihr Akteurtyp beim Actordienst registriert werden. Die `ActorRuntime` -Registrierungsmethode führt dies für Akteure aus. Im einfachsten Fall müssen Sie nur den Akteurtyp registrieren, der Actordienst wird dann implizit mit den Standardeinstellungen verwendet:

```csharp
static class Program
{
    private static void Main()
    {
        ActorRuntime.RegisterActorAsync<MyActor>().GetAwaiter().GetResult();

        Thread.Sleep(Timeout.Infinite);
    }
}
```  

Sie können auch ein Lambda verwenden, das von der Registrierungsmethode bereitgestellt wird, um selbst den Actordienst zu erstellen. So können Sie den Actordienst konfigurieren und explizit Akteurinstanzen erstellen, in die Sie Abhängigkeiten zu Ihrem Akteur über den Konstruktor einführen können:

```csharp
static class Program
{
    private static void Main()
    {
        ActorRuntime.RegisterActorAsync<MyActor>(
            (context, actorType) => new ActorService(context, actorType, () => new MyActor()))
            .GetAwaiter().GetResult();

        Thread.Sleep(Timeout.Infinite);
    }
}
```

### <a name="actor-service-methods"></a>Actordienstmethoden
Der Actordienst implementiert `IActorService`, womit wiederum `IService` implementiert wird. Dies ist die Schnittstelle, die für das Reliable Services-Remoting verwendet wird, das Remoteprozeduraufrufe von Dienstmethoden erlaubt. Sie enthält Methoden auf Dienstebene, die mit dem Dienstremoting aufgerufen werden können.

#### <a name="enumerating-actors"></a>Aufzählen von Akteuren
Der Actordienst ermöglicht Clients das Aufzählen von Metadaten über die Akteure, die vom Dienst gehostet werden. Da der Actordienst ein partitionierter zustandsbehafteter Dienst ist, erfolgt die Enumeration für die einzelnen Partitionen. Da jede Partition eine große Anzahl von Akteuren enthalten kann, wird die Enumeration als Satz von Ergebnisseiten zurückgegeben. Die Seiten werden in einer Schleife durchlaufen, bis alle Seiten gelesen wurden. Im folgenden Beispiel wird veranschaulicht, wie eine Liste aller aktiven Akteure in einer Partition eines Actordiensts erstellt wird:

```csharp
IActorService actorServiceProxy = ActorServiceProxy.Create(
    new Uri("fabric:/MyApp/MyService"), partitionKey);

ContinuationToken continuationToken = null;
List<ActorInformation> activeActors = new List<ActorInformation>();

do
{
    PagedResult<ActorInformation> page = await actorServiceProxy.GetActorsAsync(continuationToken, cancellationToken);

    activeActors.AddRange(page.Items.Where(x => x.IsActive));

    continuationToken = page.ContinuationToken;
}
while (continuationToken != null);
```

#### <a name="deleting-actors"></a>Löschen von Akteuren
Der Actordienst bietet auch eine Funktion zum Löschen von Akteuren:

```csharp
ActorId actorToDelete = new ActorId(id);

IActorService myActorServiceProxy = ActorServiceProxy.Create(
    new Uri("fabric:/MyApp/MyService"), actorToDelete);

await myActorServiceProxy.DeleteActorAsync(actorToDelete, cancellationToken)
```

Weitere Informationen zum Löschen von Akteuren und ihren Zustand finden Sie in der [Dokumentation zum Akteurlebenszyklus](service-fabric-reliable-actors-lifecycle.md).

### <a name="custom-actor-service"></a>Benutzerdefinierter Actordienst
Mithilfe des Registrierungslambdas für den Akteur können Sie auch einen eigenen benutzerdefinierten Actordienst registrieren, der von `ActorService` abgeleitet ist und in dem Sie eigene Funktionen auf Dienstebene implementieren können. Dies erfolgt durch das Schreiben einer Dienstklasse, die von `ActorService` erbt. Ein benutzerdefinierter Actordienst erbt die gesamte Laufzeitfunktionalität des Akteurs von `ActorService`. Er kann zum Implementieren eigener Dienstmethoden verwendet werden.

```csharp
class MyActorService : ActorService
{
    public MyActorService(StatefulServiceContext context, ActorTypeInformation typeInfo, Func<ActorBase> newActor)
        : base(context, typeInfo, newActor)
    { }
}
```

```csharp
static class Program
{
    private static void Main()
    {
        ActorRuntime.RegisterActorAsync<MyActor>(
            (context, actorType) => new MyActorService(context, actorType, () => new MyActor()))
            .GetAwaiter().GetResult();

        Thread.Sleep(Timeout.Infinite);
    }
}
```


#### <a name="implementing-actor-back-up-and-restore"></a>Implementieren der Sicherung und Wiederherstellung von Akteuren
 Im folgenden Beispiel stellt der benutzerdefinierte Actordienst eine Methode zum Sichern von Akteurdaten bereit. Diese nutzt die Vorteile des Remoting-Listeners, der bereits im `ActorService` enthalten ist:

```csharp
public interface IMyActorService : IService
{
    Task BackupActorsAsync();
}

class MyActorService : ActorService, IMyActorService
{
    public MyActorService(StatefulServiceContext context, ActorTypeInformation typeInfo, Func<ActorBase> newActor)
        : base(context, typeInfo, newActor)
    { }

    public Task BackupActorsAsync()
    {
        return this.BackupAsync(new BackupDescription(PerformBackupAsync));
    }

    private async Task<bool> PerformBackupAsync(BackupInfo backupInfo, CancellationToken cancellationToken)
    {
        try
        {
           // store the contents of backupInfo.Directory
           return true;
        }
        finally
        {
           Directory.Delete(backupInfo.Directory, recursive: true);
        }
    }
}
```

In diesem Beispiel ist `IMyActorService` ein Remotingvertrag, der `IService` implementiert und dann durch `MyActorService` implementiert wird. Durch das Hinzufügen dieses Remotingvertrags stehen die Methoden in `IMyActorService` jetzt auch auf einem Client bereit, da ein Remotingproxy mit `ActorServiceProxy` erstellt wird:

```csharp
IMyActorService myActorServiceProxy = ActorServiceProxy.Create<IMyActorService>(
    new Uri("fabric:/MyApp/MyService"), ActorId.CreateRandom());

await myActorServiceProxy.BackupActorsAsync();
```


## <a name="application-model"></a>Anwendungsmodell
Actordienste sind Reliable Services, deshalb ist auch das Anwendungsmodell dasselbe. Die Buildtools im Akteurframework erstellen jedoch viele der Anwendungsmodelldateien für Sie.

### <a name="service-manifest"></a>Dienstmanifest
Der Inhalt der Datei „ServiceManifest.xml“ zu Ihrem Actordienst wird automatisch von den Buildtools des Akteurframeworks generiert. Dies umfasst:

* Den Typ des Actordiensts Der Typname wird basierend auf den Projektnamen für Ihren Akteur generiert. Basierend auf dem Persistenzattribut des Akteurs wird das Flag HasPersistedState ebenfalls entsprechend festgelegt.
* Codepaket
* Konfigurationspaket
* Ressourcen und Endpunkte

### <a name="application-manifest"></a>Anwendungsmanifest
Die Buildtools des Akteurframeworks erstellen automatisch eine Standarddienstdefinition für den Actordienst. Die Standarddiensteigenschaften werden von den Buildtools aufgefüllt:

* Die Anzahl von Replikatgruppen wird durch das Persistenzattribut für den Akteur bestimmt. Jedes Mal, wenn das Persistenzattribut für den Akteur geändert wird, wird die Anzahl der Replikatgruppen in der Standarddienstdefinition entsprechend zurückgesetzt.
* Partitionsschema und Bereich werden auf Uniform Int64 mit dem kompletten Int64-Schlüsselbereich festgelegt.

## <a name="service-fabric-partition-concepts-for-actors"></a>Service Fabric-Partitionskonzepte für Actors
Actordienste sind partitionierte zustandsbehaftete Dienste. Jede Partition eines Actordiensts enthält einen Satz von Akteuren. Die Dienstpartitionen werden automatisch über mehrere Knoten in Service Fabric verteilt. Folglich werden daher auch die Akteurinstanzen verteilt.

![Akteurpartitionierung und -verteilung][5]

Reliable Services können mit anderen Partitionsschemas und Partitionsschlüsselbereichen erstellt werden. Der Actordienst verwendet das Int64-Partitionierungsschema mit dem vollständigen Int64-Schlüsselbereich für das Zuordnen von Akteuren zu Partitionen. 

### <a name="actor-id"></a>Akteur-ID
Jedem im Dienst erstellten Akteur wird eine eindeutige ID zugeordnet, die durch die `ActorId` -Klasse dargestellt wird. Die `ActorId` ist ein nicht transparenter ID-Wert, der für die gleichmäßige Verteilung von Akteuren über die Dienstpartitionen verwendet werden kann, indem zufällige IDs generiert werden:

```csharp
ActorProxy.Create<IMyActor>(ActorId.CreateRandom());
```

Für jedes `ActorId`-Element wird ein Int64-Hashwert erstellt. Aus diesem Grund muss der Actordienst ein Int64-Partitionierungsschema mit dem vollständigen Int64-Schlüsselbereich verwenden. Es können aber benutzerdefinierte ID-Werte für ein `ActorID`-Element verwendet werden (einschließlich GUIDs, Zeichenfolgen und Int64-Werten). 

```csharp
ActorProxy.Create<IMyActor>(new ActorId(Guid.NewGuid()));
ActorProxy.Create<IMyActor>(new ActorId("myActorId"));
ActorProxy.Create<IMyActor>(new ActorId(1234));
```

Bei Verwendung von GUIDs und Zeichenfolgen werden für die Werte Int64-Hashwerte erstellt. Allerdings wird bei der expliziten Bereitstellung eines Int64-Werts für eine `ActorId`der Int64-Wert direkt einer Partition zugeordnet, ohne dass ein weiteres Hashing erfolgt. Damit kann festgelegt werden, welcher Partition die Akteure zugeordnet werden.

## <a name="next-steps"></a>Nächste Schritte
* [Actor-Zustandsverwaltung](service-fabric-reliable-actors-state-management.md)
* [Actor-Lebenszyklus und Garbage Collection](service-fabric-reliable-actors-lifecycle.md)
* [Actors API reference documentation](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [Beispielcode](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-platform/actor-service.png
[2]: ./media/service-fabric-reliable-actors-platform/app-deployment-scripts.png
[3]: ./media/service-fabric-reliable-actors-platform/actor-partition-info.png
[4]: ./media/service-fabric-reliable-actors-platform/actor-replica-role.png
[5]: ./media/service-fabric-reliable-actors-introduction/distribution.png

