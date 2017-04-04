---
title: "Erstellen Ihres ersten zuverlässigen Azure-Microservice in Java | Microsoft-Dokumentation"
description: "Einführung in das Erstellen einer Microsoft Azure Service Fabric-Anwendung mit zustandslosen und zustandsbehafteten Diensten."
services: service-fabric
documentationcenter: java
author: vturecek
manager: timlt
editor: 
ms.assetid: 7831886f-7ec4-4aef-95c5-b2469a5b7b5d
ms.service: service-fabric
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/10/2017
ms.author: vturecek
translationtype: Human Translation
ms.sourcegitcommit: cf8f717d5343ae27faefdc10f81b4feaccaa53b9
ms.openlocfilehash: 5a29d6838af7f3952ad96158e5962b17c0f4cb6b
ms.lasthandoff: 01/24/2017


---
# <a name="get-started-with-reliable-services"></a>Erste Schritte mit Reliable Services
> [!div class="op_single_selector"]
> * [C# unter Windows](service-fabric-reliable-services-quick-start.md)
> * [Java unter Linux](service-fabric-reliable-services-quick-start-java.md)
>
>

Dieser Artikel erläutert die Grundlagen von Azure Service Fabric Reliable Services und führt Sie durch das Erstellen und Bereitstellen einer einfachen in Java geschriebenen Reliable Services-Anwendung. Dieses Microsoft Virtual Academy-Video zeigt auch die Vorgehensweise zum Erstellen von zustandslosen Reliable Services: <center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=DOX8K86yC_206218965">  
<img src="./media/service-fabric-reliable-services-quick-start-java/ReliableServicesJavaVid.png" WIDTH="360" HEIGHT="244">  
</a></center>

## <a name="installation-and-setup"></a>Installation und Setup
Bevor Sie beginnen, vergewissern Sie sich, dass die Entwicklungsumgebung von Service Fabric auf Ihrem Computer eingerichtet wurde.
Wenn Sie sie einrichten müssen, wechseln Sie zu [Einrichten Ihrer Entwicklungsumgebung unter Mac OS X](service-fabric-get-started-mac.md) bzw. [Vorbereiten Ihrer Entwicklungsumgebung unter Linux](service-fabric-get-started-linux.md).

## <a name="basic-concepts"></a>Grundlegende Konzepte
Für den Einstieg in Reliable Services müssen Sie sich nur mit einigen grundlegenden Konzepten vertraut machen:

* **Diensttyp**: Dies ist Ihre Dienstimplementierung. Sie wird durch die von Ihnen geschriebene Klasse definiert, mit der `StatelessService` und alle anderen darin verwendeten Codeelemente oder Abhängigkeiten erweitert werden, einschließlich eines Namens und der Versionsnummer.
* **Instanz des benannten Diensts**: Zum Ausführen des Diensts erstellen Sie benannte Instanzen Ihres Diensttyps. Dies ähnelt der Erstellung von Objektinstanzen eines Klassentyps. Bei Dienstinstanzen handelt es sich eigentlich um Objektinstanziierungen der von Ihnen geschriebenen Dienstklasse.
* **Diensthost**: Die benannten Dienstinstanzen, die Sie erstellen, müssen in einem Host ausgeführt werden. Der Diensthost ist nur ein Prozess, für den Instanzen Ihres Diensts ausgeführt werden können.
* **Dienstregistrierung**: Bei der Registrierung werden alle Elemente zusammengeführt. Der Diensttyp muss bei der Service Fabric-Laufzeit in einem Diensthost registriert werden, damit von Service Fabric Instanzen davon für die Ausführung erstellt werden können.  

## <a name="create-a-stateless-service"></a>Erstellen eines zustandslosen Diensts
Beginnen Sie, indem Sie eine Service Fabric-Anwendung erstellen. Das Service Fabric SDK für Linux enthält einen Yeoman-Generator, um das Gerüst für eine Service Fabric-Anwendung mit einem zustandslosen Dienst bereitzustellen. Beginnen Sie, indem Sie den folgenden Yeoman Befehl ausführen:

```bash
$ yo azuresfjava
```

Befolgen Sie die Anweisungen zum Erstellen eines **zustandslosen Reliable Service**. Für dieses Tutorial nennen Sie die Anwendung „HelloWorldApplication“ und den Dienst „HelloWorld“. Das Ergebnis enthält Verzeichnisse für `HelloWorldApplication` und `HelloWorld`.

```bash
HelloWorldApplication/
├── build.gradle
├── HelloWorld
│   ├── build.gradle
│   └── src
│       └── statelessservice
│           ├── HelloWorldServiceHost.java
│           └── HelloWorldService.java
├── HelloWorldApplication
│   ├── ApplicationManifest.xml
│   └── HelloWorldPkg
│       ├── Code
│       │   ├── entryPoint.sh
│       │   └── _readme.txt
│       ├── Config
│       │   └── _readme.txt
│       ├── Data
│       │   └── _readme.txt
│       └── ServiceManifest.xml
├── install.sh
├── settings.gradle
└── uninstall.sh
```

## <a name="implement-the-service"></a>Implementieren des Diensts
Öffnen Sie **HelloWorldApplication/HelloWorld/src/statelessservice/HelloWorldService.java**. Diese Klasse definiert den Diensttyp und kann jeden Code ausführen. Die Dienst-API bietet zwei Einstiegspunkte für den Code:

* Eine Einstiegspunktmethode mit offenem Ende namens `runAsync()`, mit der Sie die Ausführung beliebiger Workloads starten können, inklusive Computeworkloads mit langer Ausführungsdauer.

```java
@Override
protected CompletableFuture<?> runAsync(CancellationToken cancellationToken) {
    ...
}
```

* Einen Einstiegspunkt für die Kommunikation, mit dem Sie mit einem beliebigen Kommunikationsstapel verbinden können. Dies ist der Punkt, an dem Sie mit dem Empfangen der Anforderungen von Benutzern und anderen Diensten beginnen können.

```java
@Override
protected List<ServiceInstanceListener> createServiceInstanceListeners() {
    ...
}
```

In diesem Tutorial geht es um die Einstiegspunktmethode `runAsync()`. Hiermit können Sie sofort mit der Ausführung des Codes beginnen.

### <a name="runasync"></a>RunAsync
Die Plattform ruft diese Methode auf, wenn eine Instanz des Diensts platziert wurde und zur Ausführung bereit ist. Bei zustandslosen Diensten ist dies einfach der Fall, wenn die Dienstinstanz geöffnet wird. Ein Abbruchtoken koordiniert, wann die Dienstinstanz geschlossen werden muss. In Service Fabric kann dieser Offen/Geschlossen-Zyklus einer Dienstinstanz über die gesamte Lebensdauer des Diensts häufig auftreten. Dies kann aus unterschiedlichen Gründen geschehen, z. B.:

* Das System verschiebt Ihre Dienstinstanzen, um einen Lastenausgleich für Ressourcen durchzuführen.
* In Ihrem Code treten Fehler auf.
* Die Anwendung oder das System werden aktualisiert.
* Die zugrunde liegende Hardware fällt aus.

Diese Orchestrierung wird von Service Fabric verwaltet, um sicherzustellen, dass der Dienst hoch verfügbar bleibt und die Lasten richtig verteilt sind.

Mit `runAsync()` sollte nicht synchron blockiert werden. Ihre Implementierung von runAsync sollte ein CompletableFuture-Element zurückgeben, damit die Laufzeit fortgesetzt werden kann. Wenn Ihre Workload eine Aufgabe mit langer Ausführungsdauer implementieren muss, sollte dies innerhalb des CompletableFuture-Elements erfolgen.

#### <a name="cancellation"></a>Abbruch
Zum Abbrechen der Arbeitsauslastung ist das Zusammenspiel verschiedener Aktionen erforderlich, die vom bereitgestellten Abbruchtoken orchestriert werden. Das System wartet, bis Ihre Aufgabe beendet wurde (erfolgreicher Abschluss, Abbruch oder Fehler), bevor der Vorgang fortgesetzt wird. Es ist wichtig, das Abbruchtoken zu berücksichtigen, etwaige Arbeiten abzuschließen und `runAsync()` so schnell wie möglich zu beenden, wenn vom System ein Abbruch angefordert wird. Das folgende Beispiel zeigt, wie ein Abbruchereignis verarbeitet wird:

```java
    @Override
    protected CompletableFuture<?> runAsync(CancellationToken cancellationToken) {

        // TODO: Replace the following sample code with your own logic
        // or remove this runAsync override if it's not needed in your service.

        CompletableFuture.runAsync(() -> {
          long iterations = 0;
          while(true)
          {
            cancellationToken.throwIfCancellationRequested();
            logger.log(Level.INFO, "Working-{0}", ++iterations);

            try
            {
              Thread.sleep(1000);
            }
            catch (IOException ex) {}
          }
        });
    }
```

### <a name="service-registration"></a>Dienstregistrierung
Diensttypen müssen bei der Service Fabric-Laufzeit registriert sein. Der Diensttyp wird in der Datei `ServiceManifest.xml` und in der Dienstklasse definiert, die `StatelessService` implementiert. Die Dienstregistrierung erfolgt im Haupteinstiegspunkt des Prozesses. In diesem Beispiel lautet der Haupteinstiegspunkt des Prozesses `HelloWorldServiceHost.java`:

```java
public static void main(String[] args) throws Exception {
    try {
        ServiceRuntime.registerStatelessServiceAsync("HelloWorldType", (context) -> new HelloWorldService(), Duration.ofSeconds(10));
        logger.log(Level.INFO, "Registered stateless service type HelloWorldType.");
        Thread.sleep(Long.MAX_VALUE);
    }
    catch (Exception ex) {
        logger.log(Level.SEVERE, "Exception in registration:", ex);
        throw ex;
    }
}
```

## <a name="run-the-application"></a>Ausführen der Anwendung
Das Yeoman-Gerüst enthält ein Gradle-Skript zum Erstellen der Anwendung sowie Bash-Skripts zum Bereitstellen und Aufheben der Bereitstellung der Anwendung. Um die Anwendung auszuführen, erstellen Sie diese zunächst mit Gradle:

```bash
$ gradle
```

Dadurch wird ein Service Fabric-Anwendungspaket generiert, das mithilfe der Service Fabric-Azure CLI bereitgestellt werden kann. Das Skript „install.sh“ enthält die erforderlichen Befehle der Azure-Befehlszeilenschnittstelle zum Bereitstellen des Anwendungspakets. Führen Sie für die Bereitstellung das Skript „install.sh“ aus:

```bash
$ ./install.sh
```

