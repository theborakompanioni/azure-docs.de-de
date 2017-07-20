---
title: "Überwachung auf Azure Service Fabric-Anwendungsebene | Microsoft-Dokumentation"
description: "In diesem Artikel erfahren Sie mehr über Ereignisse und Protokolle auf Anwendungs- und Dienstebene, die zum Überwachen und Diagnostizieren von Azure Service Fabric-Clustern verwendet werden."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/26/2017
ms.author: dekapur
ms.translationtype: Human Translation
ms.sourcegitcommit: 5edc47e03ca9319ba2e3285600703d759963e1f3
ms.openlocfilehash: 3c472904641108b7383cd0f1416c47460f8de11a
ms.contentlocale: de-de
ms.lasthandoff: 05/31/2017


---

# <a name="application-and-service-level-event-and-log-generation"></a>Ereignis- und Protokollgenerierung auf Anwendungs- und Dienstebene

## <a name="instrumenting-the-code-with-custom-events"></a>Instrumentieren des Codes mit benutzerdefinierten Ereignissen

Das Instrumentieren des Codes ist die Grundlage für die meisten anderen Aspekte bei der Überwachung Ihrer Dienste. Die Instrumentierung ist die einzige Möglichkeit, wie Sie Fehler erkennen und die Diagnose zur Fehlerbehebung stellen können. Es ist zwar technisch möglich, einen Debugger mit einem Produktionsdienst zu verbinden, aber dies ist keine gängige Vorgehensweise. Es ist also wichtig, dass Sie über ausführliche Instrumentierungsdaten verfügen.

Bei einigen Produkten wird Ihr Code automatisch instrumentiert. Obwohl diese Lösungen ggf. gut funktionieren, ist fast immer eine manuelle Instrumentierung erforderlich. Letztlich benötigen Sie ausreichend Informationen für das genaue Debuggen der Anwendung. In diesem Dokument werden die verschiedenen Verfahren für das Instrumentieren des Codes und die jeweiligen Vor- und Nachteile für bestimmte Fälle beschrieben.

## <a name="eventsource"></a>EventSource
Wenn Sie eine Service Fabric-Lösung aus einer Vorlage in Visual Studio erstellen, wird eine von **EventSource** abgeleitete Klasse (**ServiceEventSource** oder **ActorEventSource**) generiert. Es wird eine Vorlage erstellt, in der Sie Ereignisse für Ihre Anwendung oder Ihren Dienst hinzufügen können. Der **EventSource**-Name **muss** eindeutig sein. Er sollte auf der Standardvorlage „MyCompany-&lt;Projektmappe&gt;-&lt;Projekt&gt;“ basieren und entsprechend umbenannt werden. Mehrere **EventSource**-Definitionen mit dem gleichen Namen führen zur Laufzeit zu Problemen. Jedes definierte Ereignis muss über einen eindeutigen Bezeichner verfügen. Wenn ein Bezeichner nicht eindeutig ist, tritt ein Laufzeitfehler auf. In einigen Organisationen werden Wertebereiche vorab Bezeichnern zugewiesen, um Konflikte zwischen separaten Entwicklungsteams zu vermeiden. Weitere Informationen finden Sie im [Blog von Vance](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/) oder in der [MSDN-Dokumentation](https://msdn.microsoft.com/library/dn774985(v=pandp.20).aspx).

### <a name="using-structured-eventsource-events"></a>Verwenden strukturierter EventSource-Ereignisse

Alle Ereignisse in den Codebeispielen in diesem Abschnitt wurden für einen bestimmten Fall definiert, z.B. für die Registrierung eines Diensttyps. Wenn Sie Meldungen nach Anwendungsfall definieren, können Daten zusammen mit dem Text des Fehlers verpackt werden, und Sie können anhand der Namen oder Werte der angegebenen Eigenschaften leichter Such- und Filtervorgänge durchführen. Durch Strukturieren der Instrumentierungsausgabe wird eine Nutzung einfacher, jedoch sind für das Definieren eines neuen Ereignisses für jeden Verwendungsfall mehr Überlegungen und Zeit notwendig. Einige Ereignisdefinitionen können über die gesamte Anwendung hinweg gemeinsam genutzt werden. Beispielweise kann ein Start- oder Stoppereignis einer Methode für viele Dienste in einer Anwendung wiederverwendet werden. Ein domänenspezifischer Dienst, z.B. ein Auftragssystem, kann ein **CreateOrder**-Ereignis mit einem eigenen eindeutigen Ereignis aufweisen. Bei diesem Ansatz werden ggf. viele Ereignisse generiert, sodass unter Umständen die Koordinierung von Bezeichnern über Projektteams hinweg erforderlich wird. 

```csharp
    [EventSource(Name = "MyCompany-VotingState-VotingStateService")]
    internal sealed class ServiceEventSource : EventSource
    {
        public static readonly ServiceEventSource Current = new ServiceEventSource();

        // The instance constructor is private to enforce singleton semantics.
        private ServiceEventSource() : base() { }

        ...

        // The ServiceTypeRegistered event contains a unique identifier, an event attribute that defined the event, and the code implementation of the event.
        private const int ServiceTypeRegisteredEventId = 3;
        [Event(ServiceTypeRegisteredEventId, Level = EventLevel.Informational, Message = "Service host process {0} registered service type {1}", Keywords = Keywords.ServiceInitialization)]
        public void ServiceTypeRegistered(int hostProcessId, string serviceType)
        {
            WriteEvent(ServiceTypeRegisteredEventId, hostProcessId, serviceType);
        }

        // The ServiceHostInitializationFailed event contains a unique identifier, an event attribute that defined the event, and the code implementation of the event.
        private const int ServiceHostInitializationFailedEventId = 4;
        [Event(ServiceHostInitializationFailedEventId, Level = EventLevel.Error, Message = "Service host initialization failed", Keywords = Keywords.ServiceInitialization)]
        public void ServiceHostInitializationFailed(string exception)
        {
            WriteEvent(ServiceHostInitializationFailedEventId, exception);
        }
```

### <a name="using-eventsource-generically"></a>Generisches Verwenden von EventSource

Da das Definieren bestimmter Ereignisse schwierig sein kann, werden häufig nur einige Ereignisse mit einem gemeinsamen Satz von Parametern definiert, die ihre Informationen in der Regel als Zeichenfolge ausgeben. Dadurch geht ein großer Teil der Strukturierung verloren, sodass es schwieriger ist, die Ergebnisse zu durchsuchen und zu filtern. Bei diesem Ansatz werden einige Ereignisse definiert, die in der Regel den Protokolliergraden entsprechen. Mit dem folgenden Codeausschnitt wird eine Debug- und Fehlermeldung definiert:

```csharp
    [EventSource(Name = "MyCompany-VotingState-VotingStateService")]
    internal sealed class ServiceEventSource : EventSource
    {
        public static readonly ServiceEventSource Current = new ServiceEventSource();

        // The Instance constructor is private, to enforce singleton semantics.
        private ServiceEventSource() : base() { }

        ...

        private const int DebugEventId = 10;
        [Event(DebugEventId, Level = EventLevel.Verbose, Message = "{0}")]
        public void Debug(string msg)
        {
            WriteEvent(DebugEventId, msg);
        }

        private const int ErrorEventId = 11;
        [Event(ErrorEventId, Level = EventLevel.Error, Message = "Error: {0} - {1}")]
        public void Error(string error, string msg)
        {
            WriteEvent(ErrorEventId, error, msg);
        }
```

Auch die Nutzung einer Hybridlösung mit strukturierter und generischer Instrumentierung kann gut funktionieren. Die strukturierte Instrumentierung wird zum Melden von Fehlern und Metriken verwendet. Generische Ereignisse können für die ausführliche Protokollierung verwendet werden, die von Technikern für die Problembehandlung genutzt wird.

## <a name="aspnet-core-logging"></a>ASP.NET Core-Protokollierung

Es ist wichtig, dass Sie sorgfältig planen, wie Sie Ihren Code instrumentieren. Mit dem richtigen Instrumentierungsplan können Sie ggf. verhindern, dass Ihre Codebasis destabilisiert wird und eine erneute Instrumentierung des Codes erforderlich wird. Zur Reduzierung des Risikos können Sie eine Instrumentierungsbibliothek wie [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/) wählen, die Teil von Microsoft ASP.NET Core ist. ASP.NET Core verfügt über eine [ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger)-Schnittstelle, die Sie zusammen mit dem Anbieter Ihrer Wahl nutzen können, während gleichzeitig die Auswirkungen auf den vorhandenen Code minimiert werden. Sie können den Code in ASP.NET Core unter Windows und Linux und im vollständigen .NET Framework verwenden, damit Ihr Instrumentierungscode standardisiert ist. Dies wird im Folgenden weiter erörtert:

### <a name="using-microsoftextensionslogging-in-service-fabric"></a>Verwenden von Microsoft.Extensions.Logging in Service Fabric

1. Fügen Sie das NuGet-Paket „Microsoft.Extensions.Logging“ dem Projekt hinzu, das Sie instrumentieren möchten. Fügen Sie außerdem alle Anbieterpakete hinzu (ein Drittanbieterpaket finden Sie im folgenden Beispiel). Weitere Informationen finden Sie unter [Logging in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging) (Protokollierung in ASP.NET Core).
2. Fügen Sie der Dienstdatei eine **using**-Direktive für Microsoft.Extensions.Logging hinzu.
3. Definieren Sie eine private Variable innerhalb Ihrer Dienstklasse.

  ```csharp
  private ILogger _logger = null;

  ```
4. Fügen Sie im Konstruktor der Dienstklasse diesen Code hinzu:

  ```csharp
  _logger = new LoggerFactory().CreateLogger<Stateless>();

  ```
5. Beginnen Sie in den Methoden, den Code zu instrumentieren. Es folgen einige Beispiele:

  ```csharp
  _logger.LogDebug("Debug-level event from Microsoft.Logging");
  _logger.LogInformation("Informational-level event from Microsoft.Logging");

  // In this variant, we're adding structured properties RequestName and Duration, which have values MyRequest and the duration of the request.
  // Later in the article, we discuss why this step is useful.
  _logger.LogInformation("{RequestName} {Duration}", "MyRequest", requestDuration);

  ```

## <a name="using-other-logging-providers"></a>Verwenden anderer Protokollierungsanbieter

Einige Drittanbieter verwenden den im obigen Abschnitt beschriebenen Ansatz, einschließlich [Serilog](https://serilog.net/), [NLog](http://nlog-project.org/) und [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging). Sie können diese Elemente in die ASP.NET Core-Protokollierung einbetten oder separat verwenden. Serilog enthält ein Feature zur Erweiterung aller Nachrichten, die von einem Protokollierungstool gesendet werden. Dieses Feature kann nützlich sein, um den Dienstnamen, den Typ und die Partitionsinformationen auszugeben. Führen Sie diese Schritte aus, um diese Funktion in der ASP.NET Core-Infrastruktur zu verwenden:

1. Fügen Sie dem Projekt die NuGet-Pakete Serilog, Serilog.Extensions.Logging und Serilog.Sinks.Observable hinzu. Fügen Sie für das nächste Beispiel außerdem Serilog.Sinks.Literate hinzu. Ein besserer Ansatz ist weiter unten in diesem Artikel dargestellt.
2. Erstellen Sie in Serilog ein LoggerConfiguration-Element und die Instanz des Protokollierungstools.

  ```csharp
  Log.Logger = new LoggerConfiguration().WriteTo.LiterateConsole().CreateLogger();
  ```

3. Fügen Sie dem Dienstkonstruktor ein Serilog.ILogger-Argument hinzu, und übergeben Sie das neu erstellte Protokollierungstool.

  ```csharp
  ServiceRuntime.RegisterServiceAsync("StatelessType", context => new Stateless(context, Log.Logger)).GetAwaiter().GetResult();
  ```

4. Fügen Sie im Dienstkonstruktor folgenden Code ein, mit dem die Eigenschaften **ServiceTypeName**, **ServiceName**, **PartitionId** und **InstanceId** des Diensts erweitert werden. Außerdem wird der ASP.NET Core-Protokollierungsfactory eine Eigenschaftserweiterung hinzugefügt, sodass Sie Microsoft.Extensions.Logging.ILogger in Ihrem Code verwenden können.

  ```csharp
  public Stateless(StatelessServiceContext context, Serilog.ILogger serilog)
      : base(context)
  {
      PropertyEnricher[] properties = new PropertyEnricher[]
      {
          new PropertyEnricher("ServiceTypeName", context.ServiceTypeName),
          new PropertyEnricher("ServiceName", context.ServiceName),
          new PropertyEnricher("PartitionId", context.PartitionId),
          new PropertyEnricher("InstanceId", context.ReplicaOrInstanceId),
      };

      serilog.ForContext(properties);

      _logger = new LoggerFactory().AddSerilog(serilog.ForContext(properties)).CreateLogger<Stateless>();
  }
  ```

5. Instrumentieren Sie den Code so wie bei der Verwendung von ASP.NET Core ohne SeriLog.

  >[!NOTE]
  >Wir empfehlen Ihnen, den statischen Log.Logger nicht für das vorherige Beispiel zu verwenden. Service Fabric kann mehrere Instanzen mit dem gleichen Diensttyp innerhalb eines Prozesses hosten. Wenn Sie den statischen Log.Logger nutzen, zeigt der letzte Writer der Eigenschaftserweiterungen Werte für alle ausgeführten Instanzen an. Dies ist ein Grund dafür, dass die _logger-Variable eine private Membervariable der Dienstklasse ist. Darüber hinaus müssen Sie „_logger“ für den allgemeinen Code verfügbar machen, der ggf. dienstübergreifend verwendet wird.

## <a name="choosing-a-logging-provider"></a>Auswählen eines Protokollierungsanbieters

Wenn Sie für Ihre Anwendung eine hohe Leistung benötigen, ist **EventSource** normalerweise ein geeigneter Ansatz. Für **EventSource** werden *im Allgemeinen* weniger Ressourcen verwendet, und es wird eine bessere Leistung als mit der ASP.NET Core-Protokollierung oder den erhältlichen Drittanbieterlösungen erzielt.  Dies ist für zahlreiche Dienste kein Problem, aber wenn Ihr Dienst leistungsorientiert ist, kann **EventSource** die bessere Wahl sein. Um die gleichen Vorteile wie bei der strukturierten Protokollierung zu erhalten, fällt für Ihre Entwickler bei **EventSource** jedoch ein höherer Aufwand an. Wenn möglich, führen Sie eine schnelle „Prototyperstellung“ für einige Protokollierungsoptionen durch, und wählen Sie dann die Option aus, die für Ihre Anforderungen am besten geeignet ist.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie den Protokollanbieter zum Instrumentieren Ihrer Anwendungen und Dienste ausgewählt haben, müssen Ihre Protokolle und Ereignisse aggregiert werden, bevor sie an eine Analyseplattform gesendet werden können. Lesen Sie die Artikel zu [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md) und [WAD](service-fabric-diagnostics-event-aggregation-wad.md), um einige der empfohlenen Optionen besser zu verstehen.

