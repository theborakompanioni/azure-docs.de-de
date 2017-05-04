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
ms.date: 04/07/2017
ms.author: vturecek
translationtype: Human Translation
ms.sourcegitcommit: c300ba45cd530e5a606786aa7b2b254c2ed32fcd
ms.openlocfilehash: 0a12da52b6e74c721cd25f89e7cde3c07153a396
ms.lasthandoff: 04/14/2017


---
# <a name="how-reliable-actors-use-the-service-fabric-platform"></a>Verwendung der Service Fabric-Plattform durch Reliable Actors
In diesem Artikel wird die Funktionsweise von Reliable Actors auf der Azure Service Fabric-Plattform beschrieben. Reliable Actors arbeiten in einem Framework, das in einer Implementierung des zustandsbehafteten zuverlässigen Diensts gehostet wird, der als *Akteurdienst* bezeichnet wird. Der Akteurdienst enthält alle Komponenten, die zum Verwalten des Lebenszyklus und zum Übermitteln von Nachrichten für Ihre Akteure erforderlich sind:

* Die Actor Runtime verwaltet den Lebenszyklus und die Garbage Collection und erzwingt den Singlethread-Zugriff.
* Ein Actordienst-Remotelistener akzeptiert Remotezugriffsaufrufe für Akteure und sendet diese an einen Verteiler, damit sie an die entsprechende Akteurinstanz weitergeleitet werden.
* Der Akteurzustandsanbieter umschließt Zustandsanbieter (z.B. den Reliable Collections-Zustandsanbieter) und stellt einen Adapter für die Akteurzustandsverwaltung bereit.

Diese Komponenten bilden zusammen das Reliable Actors-Framework.

## <a name="service-layering"></a>Dienstebenen
Da der Akteurdienst selbst einer der Reliable Services ist, gelten alle Konzepte für Reliable Services wie [Anwendungsmodell](service-fabric-application-model.md), Lebenszyklus, [Verpackung](service-fabric-package-apps.md), [Bereitstellung](service-fabric-deploy-remove-applications.md), Upgrade und Skalierung auch für Akteurdienste. 

![Akteurdienstebenen][1]

Das vorherige Diagramm zeigt die Beziehung zwischen dem Service Fabric-Anwendungsframework und dem Benutzercode. Blaue Elemente stellen das Reliable Services-Anwendungsframework dar, Orange stellt das Reliable Actors-Framework dar, und Grün steht für den Benutzercode.

In Reliable Services erbt der Dienst die `StatefulService`-Klasse. Diese Klasse ist selbst von `StatefulServiceBase` (oder `StatelessService` für zustandslose Dienste) abgeleitet. In Reliable Actors verwenden Sie den Akteurdienst. Der Akteurdienst stellt eine andere Implementierung der `StatefulServiceBase`-Klasse dar, die das Akteurmuster implementiert, wo Ihr Akteur ausgeführt wird. Da der Akteurdienst selbst nur eine Implementierung von `StatefulServiceBase` ist, können Sie einen eigenen Dienst schreiben, der von `ActorService` abgeleitet ist, und Funktionen auf Dienstebene auf die gleiche Weise implementieren wie bei der Vererbung von `StatefulService`. Beispiel:

* Dienstsicherung und -wiederherstellung
* Gemeinsame Verwendung von Funktionen für alle Akteure, z. B. ein Schaltkreisunterbrecher
* Remoteprozeduraufrufe für den Akteurdienst selbst sowie für einzelne Akteure

> [!NOTE]
> Zustandsbehaftete Dienste werden unter Java/Linux derzeit nicht unterstützt.

### <a name="using-the-actor-service"></a>Verwenden des Akteurdiensts
Akteurinstanzen haben Zugriff auf den Akteurdienst, in dem sie ausgeführt werden. Über den Akteurdienst können Akteurinstanzen den Dienstkontext programmgesteuert abrufen. Der Dienstkontext enthält die Partitions-ID, den Dienstnamen, den Anwendungsnamen und andere spezifische Informationen für die Service Fabric-Plattform:

```csharp
Task MyActorMethod()
{
    Guid partitionId = this.ActorService.Context.PartitionId;
    string serviceTypeName = this.ActorService.Context.ServiceTypeName;
    Uri serviceInstanceName = this.ActorService.Context.ServiceName;
    string applicationInstanceName = this.ActorService.Context.CodePackageActivationContext.ApplicationName;
}
```
```Java
CompletableFuture<?> MyActorMethod()
{
    UUID partitionId = this.getActorService().getServiceContext().getPartitionId();
    String serviceTypeName = this.getActorService().getServiceContext().getServiceTypeName();
    URI serviceInstanceName = this.getActorService().getServiceContext().getServiceName();
    String applicationInstanceName = this.getActorService().getServiceContext().getCodePackageActivationContext().getApplicationName();
}
```


Wie alle Reliable Services muss der Akteurdienst mit einem Diensttyp in der Service Fabric-Laufzeit registriert werden. Damit der Akteurdienst Ihre Akteurinstanzen ausführen kann, muss auch Ihr Akteurtyp beim Akteurdienst registriert werden. Die `ActorRuntime` -Registrierungsmethode führt dies für Akteure aus. Im einfachsten Fall müssen Sie nur den Akteurtyp registrieren, der Akteurdienst wird dann implizit mit den Standardeinstellungen verwendet:

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

Sie können auch ein Lambda verwenden, das von der Registrierungsmethode bereitgestellt wird, um selbst den Akteurdienst zu erstellen. So können Sie dann den Akteurdienst konfigurieren und explizit Akteurinstanzen erstellen, in die Sie Abhängigkeiten zu Ihrem Akteur über den Konstruktor einführen können:

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
```Java
static class Program
{
    private static void Main()
    {
      ActorRuntime.registerActorAsync(
              MyActor.class,
              (context, actorTypeInfo) -> new FabricActorService(context, actorTypeInfo),
              timeout);

        Thread.sleep(Long.MAX_VALUE);
    }
}
```

### <a name="actor-service-methods"></a>Akteurdienstmethoden
Der Akteurdienst implementiert `IActorService` (C#) oder `ActorService` (Java), womit wiederum `IService` (C#) oder `Service` (Java) implementiert wird. Dies ist die Schnittstelle, die für das Reliable Services-Remoting verwendet wird, das Remoteprozeduraufrufe von Dienstmethoden erlaubt. Sie enthält Methoden auf Dienstebene, die mit dem Dienstremoting aufgerufen werden können.

#### <a name="enumerating-actors"></a>Aufzählen von Akteuren
Der Akteurdienst ermöglicht Clients das Aufzählen von Metadaten über die Akteure, die vom Dienst gehostet werden. Da der Akteurdienst ein partitionierter zustandsbehafteter Dienst ist, erfolgt die Enumeration für die einzelnen Partitionen. Da jede Partition viele Akteure enthalten kann, wird die Enumeration als Satz von Ergebnisseiten zurückgegeben. Die Seiten werden in einer Schleife durchlaufen, bis alle Seiten gelesen wurden. Im folgenden Beispiel wird veranschaulicht, wie eine Liste aller aktiven Akteure in einer Partition eines Actordiensts erstellt wird:

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

```Java
ActorService actorServiceProxy = ActorServiceProxy.create(
    new URI("fabric:/MyApp/MyService"), partitionKey);

ContinuationToken continuationToken = null;
List<ActorInformation> activeActors = new ArrayList<ActorInformation>();

do
{
    PagedResult<ActorInformation> page = actorServiceProxy.getActorsAsync(continuationToken);

    while(ActorInformation x: page.getItems())
    {
         if(x.isActive()){
              activeActors.add(x);
         }
    }

    continuationToken = page.getContinuationToken();
}
while (continuationToken != null);
```

#### <a name="deleting-actors"></a>Löschen von Akteuren
Der Akteurdienst bietet auch eine Funktion zum Löschen von Akteuren:

```csharp
ActorId actorToDelete = new ActorId(id);

IActorService myActorServiceProxy = ActorServiceProxy.Create(
    new Uri("fabric:/MyApp/MyService"), actorToDelete);

await myActorServiceProxy.DeleteActorAsync(actorToDelete, cancellationToken)
```
```Java
ActorId actorToDelete = new ActorId(id);

ActorService myActorServiceProxy = ActorServiceProxy.create(
    new URI("fabric:/MyApp/MyService"), actorToDelete);

myActorServiceProxy.deleteActorAsync(actorToDelete);
```

Weitere Informationen zum Löschen von Akteuren und ihren Zustand finden Sie in der [Dokumentation zum Akteurlebenszyklus](service-fabric-reliable-actors-lifecycle.md).

### <a name="custom-actor-service"></a>Benutzerdefinierter Akteurdienst
Mit dem Lambda für die Akteurregistrierung können Sie einen eigenen benutzerdefinierten Akteurdienst registrieren, der von `ActorService` (C#) und `FabricActorService` (Java) abgeleitet wird. In diesem benutzerdefinierten Akteurdienst können Sie eigene Funktionen auf Dienstebene implementieren, indem Sie eine Dienstklasse erstellen, die `ActorService` (C#) oder `FabricActorService` (Java) erbt. Ein benutzerdefinierter Akteurdienst erbt die gesamte Laufzeitfunktionalität des Akteurs von `ActorService` (C#) oder `FabricActorService` (Java). Er kann zum Implementieren eigener Dienstmethoden verwendet werden.

```csharp
class MyActorService : ActorService
{
    public MyActorService(StatefulServiceContext context, ActorTypeInformation typeInfo, Func<ActorBase> newActor)
        : base(context, typeInfo, newActor)
    { }
}
```
```Java
class MyActorService extends FabricActorService
{
    public MyActorService(StatefulServiceContext context, ActorTypeInformation typeInfo, BiFunction<FabricActorService, ActorId, ActorBase> newActor)
    {
         super(context, typeInfo, newActor);
    }
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
```Java
public class Program
{
    public static void main(String[] args)
    {
        ActorRuntime.registerActorAsync(
                MyActor.class,
                (context, actorTypeInfo) -> new FabricActorService(context, actorTypeInfo),
                timeout);
        Thread.sleep(Long.MAX_VALUE);
    }
}
```

#### <a name="implementing-actor-backup-and-restore"></a>Implementieren der Sicherung und Wiederherstellung von Akteuren
 Im folgenden Beispiel stellt der benutzerdefinierte Akteurdienst eine Methode zum Sichern von Akteurdaten bereit. Diese nutzt die Vorteile des Remoting-Listeners, der bereits im `ActorService` enthalten ist:

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
```Java
public interface MyActorService extends Service
{
    CompletableFuture<?> backupActorsAsync();
}

class MyActorServiceImpl extends ActorService implements MyActorService
{
    public MyActorService(StatefulServiceContext context, ActorTypeInformation typeInfo, Func<FabricActorService, ActorId, ActorBase> newActor)
    {
       super(context, typeInfo, newActor);
    }

    public CompletableFuture backupActorsAsync()
    {
        return this.backupAsync(new BackupDescription((backupInfo, cancellationToken) -> performBackupAsync(backupInfo, cancellationToken)));
    }

    private CompletableFuture<Boolean> performBackupAsync(BackupInfo backupInfo, CancellationToken cancellationToken)
    {
        try
        {
           // store the contents of backupInfo.Directory
           return true;
        }
        finally
        {
           deleteDirectory(backupInfo.Directory)
        }
    }

    void deleteDirectory(File file) {
        File[] contents = file.listFiles();
        if (contents != null) {
            for (File f : contents) {
               deleteDirectory(f);
             }
        }
        file.delete();
    }
}
```


In diesem Beispiel ist `IMyActorService` ein Remotingvertrag, der `IService` (C#) und `Service` (Java) implementiert und dann durch `MyActorService` implementiert wird. Durch das Hinzufügen dieses Remotingvertrags stehen die Methoden in `IMyActorService` jetzt auch auf einem Client bereit, da ein Remotingproxy mit `ActorServiceProxy` erstellt wird:

```csharp
IMyActorService myActorServiceProxy = ActorServiceProxy.Create<IMyActorService>(
    new Uri("fabric:/MyApp/MyService"), ActorId.CreateRandom());

await myActorServiceProxy.BackupActorsAsync();
```
```Java
MyActorService myActorServiceProxy = ActorServiceProxy.create(MyActorService.class,
    new URI("fabric:/MyApp/MyService"), actorId);

myActorServiceProxy.backupActorsAsync();
```

## <a name="application-model"></a>Anwendungsmodell
Actordienste sind Reliable Services, deshalb ist auch das Anwendungsmodell dasselbe. Die Buildtools im Akteurframework erstellen jedoch einige der Anwendungsmodelldateien für Sie.

### <a name="service-manifest"></a>Dienstmanifest
Der Inhalt der Datei „ServiceManifest.xml“ zu Ihrem Akteurdienst wird automatisch von den Buildtools des Akteurframeworks generiert. Diese Datei enthält Folgendes:

* Typ des Akteurdiensts. Der Typname wird basierend auf den Projektnamen für Ihren Akteur generiert. Basierend auf dem Persistenzattribut des Akteurs wird das Flag HasPersistedState ebenfalls entsprechend festgelegt.
* Codepaket
* Konfigurationspaket
* Ressourcen und Endpunkte

### <a name="application-manifest"></a>Anwendungsmanifest
Die Buildtools des Akteurframeworks erstellen automatisch eine Standarddienstdefinition für den Actordienst. Die Standarddiensteigenschaften werden von den Buildtools aufgefüllt:

* Die Anzahl von Replikatgruppen wird durch das Persistenzattribut für den Akteur bestimmt. Jedes Mal, wenn das Persistenzattribut für den Akteur geändert wird, wird die Anzahl der Replikatgruppen in der Standarddienstdefinition entsprechend zurückgesetzt.
* Partitionsschema und Bereich werden auf Uniform Int64 mit dem kompletten Int64-Schlüsselbereich festgelegt.

## <a name="service-fabric-partition-concepts-for-actors"></a>Service Fabric-Partitionskonzepte für Actors
Actordienste sind partitionierte zustandsbehaftete Dienste. Jede Partition eines Actordiensts enthält einen Satz von Akteuren. Die Dienstpartitionen werden automatisch über mehrere Knoten in Service Fabric verteilt. Folglich werden die Akteurinstanzen verteilt.

![Akteurpartitionierung und -verteilung][5]

Reliable Services können mit anderen Partitionsschemas und Partitionsschlüsselbereichen erstellt werden. Der Akteurdienst verwendet das Int64-Partitionierungsschema mit dem vollständigen Int64-Schlüsselbereich für das Zuordnen von Akteuren zu Partitionen.

### <a name="actor-id"></a>Akteur-ID
Jedem im Dienst erstellten Akteur wird eine eindeutige ID zugeordnet, die durch die `ActorId` -Klasse dargestellt wird. Die `ActorId` ist ein nicht transparenter ID-Wert, der für die gleichmäßige Verteilung von Akteuren über die Dienstpartitionen verwendet werden kann, indem zufällige IDs generiert werden:

```csharp
ActorProxy.Create<IMyActor>(ActorId.CreateRandom());
```
```Java
ActorProxyBase.create<MyActor>(MyActor.class, ActorId.newId());
```


Für jedes `ActorId`-Element wird ein Int64-Hashwert erstellt. Aus diesem Grund muss der Akteurdienst ein Int64-Partitionierungsschema mit dem vollständigen Int64-Schlüsselbereich verwenden. Es können aber benutzerdefinierte ID-Werte für ein `ActorID`-Element verwendet werden (einschließlich GUIDs/UUIDs, Zeichenfolgen und Int64-Werten).

```csharp
ActorProxy.Create<IMyActor>(new ActorId(Guid.NewGuid()));
ActorProxy.Create<IMyActor>(new ActorId("myActorId"));
ActorProxy.Create<IMyActor>(new ActorId(1234));
```
```Java
ActorProxyBase.create(MyActor.class, new ActorId(UUID.randomUUID()));
ActorProxyBase.create(MyActor.class, new ActorId("myActorId"));
ActorProxyBase.create(MyActor.class, new ActorId(1234));
```

Bei Verwendung von GUIDs/UUIDs und Zeichenfolgen werden für die Werte Int64-Hashwerte erstellt. Allerdings wird bei der expliziten Bereitstellung eines Int64-Werts für eine `ActorId` der Int64-Wert direkt einer Partition zugeordnet, ohne dass ein weiteres Hashing erfolgt. Sie können dieses Verfahren verwenden, um zu steuern, in welcher Partition die Akteure platziert werden.

## <a name="next-steps"></a>Nächste Schritte
* [Actor-Zustandsverwaltung](service-fabric-reliable-actors-state-management.md)
* [Actor-Lebenszyklus und Garbage Collection](service-fabric-reliable-actors-lifecycle.md)
* [Actors API reference documentation](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [.NET-Beispielcode](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [Java-Beispielcode](http://github.com/Azure-Samples/service-fabric-java-getting-started)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-platform/actor-service.png
[2]: ./media/service-fabric-reliable-actors-platform/app-deployment-scripts.png
[3]: ./media/service-fabric-reliable-actors-platform/actor-partition-info.png
[4]: ./media/service-fabric-reliable-actors-platform/actor-replica-role.png
[5]: ./media/service-fabric-reliable-actors-introduction/distribution.png

