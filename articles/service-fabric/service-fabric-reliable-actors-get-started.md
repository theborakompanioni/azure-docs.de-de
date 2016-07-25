<properties
   pageTitle="Erste Schritte mit Service Fabric Reliable Actors | Microsoft Azure"
   description="Dieses Tutorial führt Sie durch die Schritte zum Erstellen, Debuggen und Bereitstellen eines einfachen actorbasierten Diensts mithilfe von Service Fabric Reliable Actors."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/06/2016"
   ms.author="vturecek"/>

# Erste Schritte mit Reliable Actors
Dieser Artikel erläutert die Grundlagen von Azure Service Fabric Reliable Actors und führt Sie durch das Erstellen, Debuggen und Bereitstellen einer einfachen Reliable Actors-Anwendung in Visual Studio.

## Installation und Setup
Bevor Sie beginnen, vergewissern Sie sich, dass sich das Setup der Entwicklungsumgebung von Service Fabric auf Ihrem Computer befindet. Wenn Sie die Umgebung einrichten müssen, finden Sie ausführliche Anweisungen unter [Einrichten der Entwicklungsumgebung](service-fabric-get-started.md).

## Grundlegende Konzepte
Um mit Reliable Actors zu beginnen, müssen Sie lediglich vier grundlegende Konzepte verstehen:

* **Actor-Dienst**. Reliable Actors sind in Reliable Services gepackt, die in der Service Fabric-Infrastruktur bereitgestellt werden können. Ein Dienst kann einen oder mehrere Actors hosten. Die Vor- und Nachteile von einem oder mehreren Actors pro Dienst werden weiter unten ausführlicher behandelt. Zunächst gehen wir davon aus, dass nur ein Actor implementiert werden soll.
* **Actor-Schnittstelle**. Die Actor-Schnittstelle wird zum Definieren einer stark typisierten öffentlichen Schnittstelle eines Actors verwendet. In der Terminologie von Reliable Actor-Modellen definiert die Actor-Schnittstelle die Nachrichtentypen, die der Actor verstehen und verarbeiten kann. Die Actor-Schnittstelle wird von anderen Actors oder von Clientanwendungen zum (asynchronen) „Senden“ von Nachrichten an den Actor verwendet. Reliable Actors können mehrere Schnittstellen implementieren. Wie noch zu sehen sein wird, kann ein HelloWorld-Actor die IHelloWorld-Schnittstelle implementieren, jedoch auch eine ILogging-Schnittstelle, die verschiedene Nachrichten und/oder Funktionalitäten definiert.
* **Actor-Registrierung**. Im Reliable Actors-Dienst muss der Actor-Typ registriert werden. Auf diese Weise ist der neue Typ in Service Fabric bekannt und kann zum Erstellen neuer Actors verwendet werden.
* **ActorProxy-Klasse**. Die ActorProxy-Klasse dient Clientanwendungen zum Aufrufen der Methoden, die über ihre Schnittstellen verfügbar gemacht werden. Die ActorProxy-Klasse bietet zwei wichtige Funktionen:
	* Namensauflösung. Kann zum Lokalisieren des Actors im Cluster verwendet werden (den Knoten des Clusters suchen, auf dem er gehostet wird).
	* Behandeln von Fehlern. Kann Methodenaufrufe erneut versuchen und den Actor-Speicherort erneut bestimmen, z. B. nach einem Fehler, der erfordert, dass der Actor auf einen anderen Knoten im Cluster verschoben wird.

Die folgenden Regeln zu Actor-Schnittstellen sind erwähnenswert:

- Actor-Schnittstellenmethoden können nicht überladen werden.
- Actor-Schnittstellenmethoden dürfen keine out-, ref- oder optionalen Parameter aufweisen.
- Generische Schnittstellen werden nicht unterstützt.

## Erstellen eines neuen Projekts in Visual Studio
Nachdem Sie die Service Fabric-Tools für Visual Studio installiert haben, können Sie neue Projekttypen erstellen. Die neuen Projekttypen befinden sich in der Kategorie **Cloud** des Dialogfelds **Neues Projekt**.


![Service Fabric-Tools für Visual Studio – Neues Projekt][1]

Im nächsten Dialogfeld können Sie den Typ des Projekts auswählen, das Sie erstellen möchten.

![Service Fabric-Projektvorlagen][5]

Für das HelloWorld-Projekt wird der Service Fabric Reliable Actors-Dienst verwendet.

Nachdem die Projektmappe erstellt wurde, sollte die folgende Struktur angezeigt werden:

![Service Fabric-Projektstruktur][2]

## Grundlegende Bausteine von Reliable Actors

Eine typische Reliable Actors-Projektmappe besteht aus drei Projekten:

* **Das Anwendungsprojekt (MyActorApplication)**. Dies ist das Projekt, das alle Dienste für die Bereitstellung zusammenfasst. Es enthält die Datei *ApplicationManifest.xml* und PowerShell-Skripts zum Verwalten der Anwendung.

* **Das Schnittstellenprojekt (MyActor.Interfaces)**. Dies ist das Projekt, das die Schnittstellendefinition für den Actor enthält. Im Projekt „MyActor.Interfaces“ können Sie die Schnittstellen definieren, die von den Actors in der Lösung verwendet werden. Ihre Actor-Schnittstellen können in jedem beliebigen Projekt mit jedem beliebigen Namen definiert werden. Jedoch definiert die Schnittstelle den Actor-Vertrag, der von der Actor-Implementierung und den den Actor aufrufenden Clients gemeinsam verwendet wird. In der Regel ist es also sinnvoll, diesen in einer Assembly zu definieren, die getrennt ist von der Actor-Implementierung und die von mehreren anderen Projekten gemeinsam verwendet werden kann.

```csharp
public interface IMyActor : IActor
{
    Task<string> HelloWorld();
}
```

* **Das Actordienst-Projekt (MyActor)**. Dies ist das Projekt, mit dem der Service Fabric-Dienst zum Hosten des Actors definiert wird. Es enthält die Implementierung des Actors. Eine Actor-Implementierung ist eine Klasse, die vom Basistyp `Actor` abstammt und die im Projekt „MyActor.Interfaces“ definierte(n) Schnittstelle(n) implementiert.

```csharp
[StatePersistence(StatePersistence.Persisted)]
internal class MyActor : Actor, IMyActor
{
    public Task<string> HelloWorld()
    {
        return Task.FromResult("Hello world!");
    }
}
```

Der Actordienst muss mit einem Diensttyp in der Service Fabric-Laufzeit registriert sein. Damit der Actordienst Ihre Actor-Instanzen ausführen kann, muss auch Ihr Actortyp mit dem Actordienst registriert sein. Die `ActorRuntime`-Registrierungsmethode führt dies für Actors aus.

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

> [AZURE.TIP] Die Service Fabric Actors-Laufzeit gibt einige [Ereignisse und Leistungsindikatoren im Zusammenhang mit Actor-Methoden](service-fabric-reliable-actors-diagnostics.md#actor-method-events-and-performance-counters) aus. Sie sind hilfreich bei der Diagnose und Leistungsüberwachung.


## Debuggen

Die Service Fabric-Tools für Visual Studio unterstützen das Debuggen auf dem lokalen Computer. Sie können eine Debugsitzung starten, indem Sie F5 drücken. Visual Studio erstellt Pakete (falls erforderlich). Es stellt außerdem die Anwendung auf dem lokalen Service Fabric-Cluster bereit und fügt den Debugger an.

Während der Bereitstellung wird der Fortschritt im Fenster **Ausgabe** angezeigt.

![Service Fabric-Debugging – Ausgabefenster][3]


## Nächste Schritte
 - [Verwendung der Service Fabric-Plattform durch Reliable Actors](service-fabric-reliable-actors-platform.md)
 - [Actor-Zustandsverwaltung](service-fabric-reliable-actors-state-management.md)
 - [Actor-Lebenszyklus und Garbage Collection](service-fabric-reliable-actors-lifecycle.md)
 - [Actor-API-Referenzdokumentation](https://msdn.microsoft.com/library/azure/dn971626.aspx)
 - [Beispielcode](https://github.com/Azure/servicefabric-samples)


<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject.PNG
[2]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-projectstructure.PNG
[3]: ./media/service-fabric-reliable-actors-get-started/debugging-output.PNG
[4]: ./media/service-fabric-reliable-actors-get-started/vs-context-menu.png
[5]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject1.PNG

<!---HONumber=AcomDC_0713_2016-->