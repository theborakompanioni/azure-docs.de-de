---
title: Erste Schritte mit Service Fabric Reliable Actors | Microsoft Docs
description: "Dieses Tutorial führt Sie durch die Schritte zum Erstellen, Debuggen und Bereitstellen eines einfachen actorbasierten Diensts mithilfe von Service Fabric Reliable Actors."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: d4aebe72-1551-4062-b1eb-54d83297f139
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/04/2017
ms.author: vturecek
translationtype: Human Translation
ms.sourcegitcommit: 2304a9433bb382c0c7ddcf36733838ac769b9584
ms.openlocfilehash: 98e519df244e9ae934b8100ea9820a7f765b1ee3


---
# <a name="getting-started-with-reliable-actors"></a>Erste Schritte mit Reliable Actors
> [!div class="op_single_selector"]
> * [C# unter Windows](service-fabric-reliable-actors-get-started.md)
> * [Java unter Linux](service-fabric-reliable-actors-get-started-java.md)
> 
> 

Dieser Artikel erläutert die Grundlagen von Azure Service Fabric Reliable Actors und führt Sie durch das Erstellen, Debuggen und Bereitstellen einer einfachen Reliable Actors-Anwendung in Visual Studio.

## <a name="installation-and-setup"></a>Installation und Setup
Bevor Sie beginnen, vergewissern Sie sich, dass sich das Setup der Entwicklungsumgebung von Service Fabric auf Ihrem Computer befindet.
Wenn Sie die Umgebung einrichten müssen, finden Sie ausführliche Anweisungen unter [Einrichten der Entwicklungsumgebung](service-fabric-get-started.md).

## <a name="basic-concepts"></a>Grundlegende Konzepte
Für den Einstieg in Reliable Actors müssen Sie sich nur mit einigen grundlegenden Konzepten vertraut machen:

* **Actor-Dienst**. Reliable Actors sind in Reliable Services gepackt, die in der Service Fabric-Infrastruktur bereitgestellt werden können. Actor-Instanzen werden in einer benannten Dienstinstanz aktiviert.
* **Actor-Registrierung**. Wie bei Reliable Services muss ein Reliable Actor-Dienst bei der Service Fabric-Laufzeit registriert werden. Außerdem muss der Actor-Typ bei der Actor-Laufzeit registriert werden.
* **Actor-Schnittstelle**. Die Actor-Schnittstelle wird zum Definieren einer stark typisierten öffentlichen Schnittstelle eines Actors verwendet. In der Terminologie von Reliable Actor-Modellen definiert die Actor-Schnittstelle die Nachrichtentypen, die der Actor verstehen und verarbeiten kann. Die Actor-Schnittstelle wird von anderen Actors oder von Clientanwendungen zum (asynchronen) „Senden“ von Nachrichten an den Actor verwendet. Reliable Actors können mehrere Schnittstellen implementieren.
* **ActorProxy-Klasse**. Die ActorProxy-Klasse dient Clientanwendungen zum Aufrufen der Methoden, die über die Actor-Schnittstelle verfügbar gemacht werden. Die ActorProxy-Klasse bietet zwei wichtige Funktionen:
  
  * Namensauflösung: Kann zum Lokalisieren des Actors im Cluster verwendet werden (den Knoten des Clusters suchen, auf dem er gehostet wird).
  * Fehlerbehandlung: Kann Methodenaufrufe erneut versuchen und den Actor-Speicherort erneut auflösen, z.B. nach einem Fehler, der erfordert, dass der Actor auf einen anderen Knoten im Cluster verschoben wird.

Die folgenden Regeln zu Actor-Schnittstellen sind erwähnenswert:

* Actor-Schnittstellenmethoden können nicht überladen werden.
* Actor-Schnittstellenmethoden dürfen keine out-, ref- oder optionalen Parameter aufweisen.
* Generische Schnittstellen werden nicht unterstützt.

## <a name="create-a-new-project-in-visual-studio"></a>Erstellen eines neuen Projekts in Visual Studio
Nachdem Sie die Service Fabric-Tools für Visual Studio installiert haben, können Sie neue Projekttypen erstellen. Die neuen Projekttypen befinden sich in der Kategorie **Cloud** des Dialogfelds **Neues Projekt**.

![Service Fabric-Tools für Visual Studio – Neues Projekt][1]

Im nächsten Dialogfeld können Sie den Typ des Projekts auswählen, das Sie erstellen möchten.

![Service Fabric-Projektvorlagen][5]

Für das HelloWorld-Projekt wird der Service Fabric Reliable Actors-Dienst verwendet.

Nachdem die Projektmappe erstellt wurde, sollte die folgende Struktur angezeigt werden:

![Service Fabric-Projektstruktur][2]

## <a name="reliable-actors-basic-building-blocks"></a>Grundlegende Bausteine von Reliable Actors
Eine typische Reliable Actors-Projektmappe besteht aus drei Projekten:

* **Das Anwendungsprojekt (MyActorApplication)**. Dies ist das Projekt, das alle Dienste für die Bereitstellung zusammenfasst. Es enthält die Datei *ApplicationManifest.xml* und PowerShell-Skripts zum Verwalten der Anwendung.
* **Das Schnittstellenprojekt (MyActor.Interfaces)**. Dies ist das Projekt, das die Schnittstellendefinition für den Actor enthält. Im Projekt „MyActor.Interfaces“ können Sie die Schnittstellen definieren, die von den Actors in der Lösung verwendet werden. Ihre Actor-Schnittstellen können in jedem beliebigen Projekt mit jedem beliebigen Namen definiert werden. Jedoch definiert die Schnittstelle den Actor-Vertrag, der von der Actor-Implementierung und den den Actor aufrufenden Clients gemeinsam verwendet wird. In der Regel ist es also sinnvoll, diesen in einer Assembly zu definieren, die getrennt ist von der Actor-Implementierung und die von mehreren anderen Projekten gemeinsam verwendet werden kann.

```csharp
public interface IMyActor : IActor
{
    Task<string> HelloWorld();
}
```

* **Das Actordienst-Projekt (MyActor)**. Dies ist das Projekt, mit dem der Service Fabric-Dienst zum Hosten des Actors definiert wird. Es enthält die Implementierung des Actors. Eine Actor-Implementierung ist eine Klasse, die vom Basistyp `Actor` abstammt und die im Projekt „MyActor.Interfaces“ definierte(n) Schnittstelle(n) implementiert. Eine Actor-Klasse muss auch einen Konstruktor implementieren, der eine `ActorService`-Instanz und eine `ActorId` akzeptiert und diese an die `Actor`-Basisklasse übergibt. Dies ermöglicht eine Injektion von Konstruktorabhängigkeit von Plattformabhängigkeiten.

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task<string> HelloWorld()
    {
        return Task.FromResult("Hello world!");
    }
}
```

Der Actordienst muss mit einem Diensttyp in der Service Fabric-Laufzeit registriert sein. Damit der Actordienst Ihre Actor-Instanzen ausführen kann, muss auch Ihr Actortyp mit dem Actordienst registriert sein. Die `ActorRuntime` -Registrierungsmethode führt dies für Actors aus.

```csharp
internal static class Program
{
    private static void Main()
    {
        try
        {
            ActorRuntime.RegisterActorAsync<MyActor>(
                (context, actorType) => new ActorService(context, actorType, () => new MyActor())).GetAwaiter().GetResult();

            Thread.Sleep(Timeout.Infinite);
        }
        catch (Exception e)
        {
            ActorEventSource.Current.ActorHostInitializationFailed(e.ToString());
            throw;
        }
    }
}

```

Wenn Sie ein neues Projekt in Visual Studio beginnen und nur eine Actor-Definition haben, wird die Registrierung standardmäßig in den von Visual Studio generierten Code eingefügt. Wenn Sie in dem Dienst weitere Actors definieren, müssen Sie die Actor-Registrierung folgendermaßen hinzufügen:

```csharp
 ActorRuntime.RegisterActorAsync<MyOtherActor>();

```

> [!TIP]
> Die Service Fabric Actors-Laufzeit gibt einige [Ereignisse und Leistungsindikatoren im Zusammenhang mit Actor-Methoden](service-fabric-reliable-actors-diagnostics.md#actor-method-events-and-performance-counters)aus. Sie sind hilfreich bei der Diagnose und Leistungsüberwachung.
> 
> 

## <a name="debugging"></a>Debuggen
Die Service Fabric-Tools für Visual Studio unterstützen das Debuggen auf dem lokalen Computer. Sie können eine Debugsitzung starten, indem Sie F5 drücken. Visual Studio erstellt Pakete (falls erforderlich). Es stellt außerdem die Anwendung auf dem lokalen Service Fabric-Cluster bereit und fügt den Debugger an.

Während der Bereitstellung wird der Fortschritt im Fenster **Ausgabe** angezeigt.

![Service Fabric-Debugging – Ausgabefenster][3]

## <a name="next-steps"></a>Nächste Schritte
* [Verwendung der Service Fabric-Plattform durch Reliable Actors](service-fabric-reliable-actors-platform.md)
* [Actor-Zustandsverwaltung](service-fabric-reliable-actors-state-management.md)
* [Actor-Lebenszyklus und Garbage Collection](service-fabric-reliable-actors-lifecycle.md)
* [Actor-API-Referenzdokumentation](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [Beispielcode](https://github.com/Azure/servicefabric-samples)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject.PNG
[2]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-projectstructure.PNG
[3]: ./media/service-fabric-reliable-actors-get-started/debugging-output.PNG
[4]: ./media/service-fabric-reliable-actors-get-started/vs-context-menu.png
[5]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject1.PNG



<!--HONumber=Jan17_HO1-->


