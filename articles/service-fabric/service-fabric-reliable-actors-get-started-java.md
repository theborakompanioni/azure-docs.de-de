---
title: Erstellen Ihres ersten Actor-basierten Azure-Microservice in Java | Microsoft-Dokumentation
description: "Dieses Tutorial führt Sie durch die Schritte zum Erstellen, Debuggen und Bereitstellen eines einfachen actorbasierten Diensts mithilfe von Service Fabric Reliable Actors."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: d31dc8ab-9760-4619-a641-facb8324c759
ms.service: service-fabric
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/04/2017
ms.author: vturecek
ms.translationtype: HT
ms.sourcegitcommit: 25e4506cc2331ee016b8b365c2e1677424cf4992
ms.openlocfilehash: 288f1ed1016f50031065e66444d2562427194dc7
ms.contentlocale: de-de
ms.lasthandoff: 08/24/2017

---
# <a name="getting-started-with-reliable-actors"></a>Erste Schritte mit Reliable Actors
> [!div class="op_single_selector"]
> * [C# unter Windows](service-fabric-reliable-actors-get-started.md)
> * [Java unter Linux](service-fabric-reliable-actors-get-started-java.md)
> 
> 

Dieser Artikel erläutert die Grundlagen von Azure Service Fabric Reliable Actors und führt Sie durch das Erstellen und Bereitstellen einer einfachen Reliable Actors-Anwendung in Java.

## <a name="installation-and-setup"></a>Installation und Setup
Bevor Sie beginnen, vergewissern Sie sich, dass die Entwicklungsumgebung von Service Fabric auf Ihrem Computer eingerichtet wurde.
Wenn Sie sie einrichten müssen, wechseln Sie zu [Einrichten Ihrer Entwicklungsumgebung unter Mac OS X](service-fabric-get-started-mac.md) bzw. [Vorbereiten Ihrer Entwicklungsumgebung unter Linux](service-fabric-get-started-linux.md).

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

## <a name="create-an-actor-service"></a>Erstellen eines Actor-Diensts
Beginnen Sie, indem Sie eine neue Service Fabric-Anwendung erstellen. Das Service Fabric SDK für Linux enthält einen Yeoman-Generator, um das Gerüst für eine Service Fabric-Anwendung mit einem zustandslosen Dienst bereitzustellen. Beginnen Sie, indem Sie den folgenden Yeoman Befehl ausführen:

```bash
$ yo azuresfjava
```

Befolgen Sie die Anweisungen zum Erstellen einer **Reliable Actor-Diensts**. Für dieses Tutorial nennen Sie die Anwendung „HelloWorldActorApplication“ und den Actor „HelloWorldActor“. Das folgende Gerüst wird erstellt:

```bash
HelloWorldActorApplication/
├── build.gradle
├── HelloWorldActor
│   ├── build.gradle
│   ├── settings.gradle
│   └── src
│       └── reliableactor
│           ├── HelloWorldActorHost.java
│           └── HelloWorldActorImpl.java
├── HelloWorldActorApplication
│   ├── ApplicationManifest.xml
│   └── HelloWorldActorPkg
│       ├── Code
│       │   ├── entryPoint.sh
│       │   └── _readme.txt
│       ├── Config
│       │   ├── _readme.txt
│       │   └── Settings.xml
│       ├── Data
│       │   └── _readme.txt
│       └── ServiceManifest.xml
├── HelloWorldActorInterface
│   ├── build.gradle
│   └── src
│       └── reliableactor
│           └── HelloWorldActor.java
├── HelloWorldActorTestClient
│   ├── build.gradle
│   ├── settings.gradle
│   ├── src
│   │   └── reliableactor
│   │       └── test
│   │           └── HelloWorldActorTestClient.java
│   └── testclient.sh
├── install.sh
├── settings.gradle
└── uninstall.sh
```

## <a name="reliable-actors-basic-building-blocks"></a>Grundlegende Bausteine von Reliable Actors
Die zuvor beschriebenen Konzepte bilden die Grundbausteine eines Reliable Actor-Diensts.

### <a name="actor-interface"></a>Actor-Schnittstelle
Diese enthält die Schnittstellendefinition für den Actor. Diese Schnittstelle definiert den Actor-Vertrag, der von der Actor-Implementierung und den den Actor aufrufenden Clients gemeinsam verwendet wird. In der Regel ist es also sinnvoll, diesen an einem Ort zu definieren, der getrennt ist von der Actor-Implementierung und von mehreren anderen Diensten oder Clientanwendungen gemeinsam verwendet werden kann.

`HelloWorldActorInterface/src/reliableactor/HelloWorldActor.java`:

```java
public interface HelloWorldActor extends Actor {
    @Readonly   
    CompletableFuture<Integer> getCountAsync();

    CompletableFuture<?> setCountAsync(int count);
}
```

### <a name="actor-service"></a>Actordienst
Dieser Dienst enthält Ihre Actor-Implementierung und den Actor-Registrierungscode. Die Actor-Klasse implementiert die Actor-Schnittstelle. Hier führt der Actor seine Aufgaben aus.

`HelloWorldActor/src/reliableactor/HelloWorldActorImpl`:

```java
@ActorServiceAttribute(name = "HelloWorldActor.HelloWorldActorService")
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
public class HelloWorldActorImpl extends ReliableActor implements HelloWorldActor {
    Logger logger = Logger.getLogger(this.getClass().getName());

    protected CompletableFuture<?> onActivateAsync() {
        logger.log(Level.INFO, "onActivateAsync");

        return this.stateManager().tryAddStateAsync("count", 0);
    }

    @Override
    public CompletableFuture<Integer> getCountAsync() {
        logger.log(Level.INFO, "Getting current count value");
        return this.stateManager().getStateAsync("count");
    }

    @Override
    public CompletableFuture<?> setCountAsync(int count) {
        logger.log(Level.INFO, "Setting current count value {0}", count);
        return this.stateManager().addOrUpdateStateAsync("count", count, (key, value) -> count > value ? count : value);
    }
}
```

### <a name="actor-registration"></a>Actor-Registrierung
Der Actordienst muss mit einem Diensttyp in der Service Fabric-Laufzeit registriert sein. Damit der Actordienst Ihre Actor-Instanzen ausführen kann, muss auch Ihr Actortyp mit dem Actordienst registriert sein. Die `ActorRuntime` -Registrierungsmethode führt dies für Akteure aus.

`HelloWorldActor/src/reliableactor/HelloWorldActorHost`:

```java
public class HelloWorldActorHost {

    public static void main(String[] args) throws Exception {

        try {
            ActorRuntime.registerActorAsync(HelloWorldActorImpl.class, (context, actorType) -> new ActorServiceImpl(context, actorType, ()-> new HelloWorldActorImpl()), Duration.ofSeconds(10));

            Thread.sleep(Long.MAX_VALUE);

        } catch (Exception e) {
            e.printStackTrace();
            throw e;
        }
    }
}
```

### <a name="test-client"></a>Testen des Clients
Hierbei handelt es sich um eine einfache Testclientanwendung, die separat von der Service Fabric-Anwendung zum Testen des Actor-Diensts ausgeführt werden kann. Dies ist ein Beispiel für den Einsatz von „ActorProxy“ zum Aktivieren und Kommunizieren mit Actor-Instanzen. Diese Klasse wird nicht mit Ihrem Dienst bereitgestellt.

### <a name="the-application"></a>Anwendung
Abschließend fasst die Anwendung den Actor-Dienst und alle ggf. später hinzugefügten Dienste in einem Paket für die Bereitstellung zusammen. Das Paket enthält die Datei *ApplicationManifest.xml* und Platzhalter für das Paket des Actor-Diensts.

## <a name="run-the-application"></a>Ausführen der Anwendung

Das Yeoman-Gerüst enthält ein Gradle-Skript zum Erstellen der Anwendung sowie Bash-Skripts zum Bereitstellen und Entfernen der Anwendung. Um die Anwendung bereitzustellen, erstellen Sie diese zunächst mit Gradle:

```bash
$ gradle
```

Dadurch wird ein Service Fabric-Anwendungspaket generiert, das mithilfe der Service Fabric-CLI-Tools bereitgestellt werden kann.

### <a name="deploy-service-fabric-cli"></a>Bereitstellen der Service Fabric-Befehlszeilenschnittstelle

Das Skript „install.sh“ enthält die erforderlichen Befehle der Service Fabric-Befehlszeilenschnittstelle (sfctl) zum Bereitstellen des Anwendungspakets.
Führen Sie das Skript „install.sh“ aus, um die Anwendung bereitzustellen.

```bash
$ ./install.sh
```

## <a name="next-steps"></a>Nächste Schritte

* [Erste Schritte mit der Service Fabric-Befehlszeilenschnittstelle](service-fabric-cli.md)

