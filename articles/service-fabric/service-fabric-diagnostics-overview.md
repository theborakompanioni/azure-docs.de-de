---
title: "Übersicht über Azure Service Fabric-Überwachung und -Diagnose | Microsoft-Dokumentation"
description: "Erfahren Sie, wie die Überwachung und Diagnose von Microsoft Azure Service Fabric-Anwendungen, die in Azure, einer Entwicklungsumgebung oder lokal gehostet werden, durchgeführt wird."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: mfussell
editor: 
ms.assetid: edcc0631-ed2d-45a3-851d-2c4fa0f4a326
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/10/2017
ms.author: dekapur
ms.translationtype: Human Translation
ms.sourcegitcommit: 97fa1d1d4dd81b055d5d3a10b6d812eaa9b86214
ms.openlocfilehash: 9c1a3bb6de8756c37903e5f1b9dcf3fdc1ef6a11
ms.contentlocale: de-de
ms.lasthandoff: 05/11/2017


---
# <a name="monitoring-and-diagnostics-in-azure-service-fabric"></a>Überwachung und Diagnose in Azure Service Fabric

Überwachung und Diagnose sind wichtig für Entwicklung und Tests sowie die erfolgreiche Bereitstellung in Produktionsumgebungen. Service Fabric-Lösungen funktionieren am besten, wenn Sie die Überwachung und Diagnose von Beginn an implementieren. So stellen Sie sicher, dass die Dienste nahtlos in den lokalen Entwicklungsumgebungen und in realen Produktionsclusterinstallationen funktionieren.

Die Hauptziele der Überwachung und Diagnose sind:
* Erkennen und Diagnostizieren von Hardware- und Infrastrukturproblemen
* Erkennen von Software- und App-Problemen, Verringern von Dienstausfallzeiten
* Verstehen des Ressourcenverbrauchs und Hilfe bei betrieblichen Entscheidungen
* Optimieren der Leistung von Anwendung, Dienst und Infrastruktur
* Generieren von geschäftlichen Einblicken und Identifizieren von Verbesserungspotenzialen


Überwachung und Diagnose sind wichtig, um sicherzustellen, dass alles wie erwartet funktioniert. Außerdem können Sie so auf Situationen mit lediglich minimalen Unterbrechungen des Diensts reagieren. 

Der allgemeine Workflow für die Überwachung und Diagnose besteht aus drei Schritten: 
1. **Ereignisgenerierung:** Dies schließt Ereignisse (Protokolle, Ablaufverfolgungen, benutzerdefinierte Ereignisse) auf Infrastruktur- (Cluster-) und Anwendungs-/Dienstebene ein. 
2. **Ereignisaggregation:** Die generierten Ereignisse müssen gesammelt und aggregiert werden, bevor sie angezeigt werden können. Dies kann über Speichertabellen wie bei Microsoft Azure-Diagnose oder durch das Erstellen einer EventFlow-Pipeline erfolgen.
3. **Analyse:** Die Ereignisse müssen visualisiert und in einem Format verfügbar sein, das die Analyse und Anzeige je nach Bedarf ermöglicht. Sie können auch Tools wie ApplicationInsights, Operations Management Suite, Kibana und andere verwenden.

Es sind zwar mehrere Produkte erhältlich, die alle drei Bereiche abdecken, aber viele Kunden wählen für die einzelnen Aspekte unterschiedliche Technologien. Es ist wichtig, dass diese jeweils zusammenpassen, um eine umfassende Überwachungslösung für die Anwendung bereitzustellen.

Je nach Ihren Anforderungen können Sie die Diagnose und Überwachung erweitern, um automatisierte Warnungen und eine Problemumgehung hinzuzufügen. Beide Features sind häufig in Analysetools integriert, die Sie möglicherweise verwenden. 

## <a name="event-generation"></a>Ereignisgenerierung

Ereignisse, Protokolle und Ablaufverfolgungen können auf Infrastrukturebene (von Clustern, Computern oder Service Fabric-Aktionen) oder auf Anwendungsebene (Instrumentierungen in Apps und Diensten, die im Cluster bereitgestellt wurden) generiert werden.

### <a name="infrastructure-monitoring-the-cluster"></a>Infrastruktur: Überwachen des Clusters

Service Fabric kann dazu beitragen, dass eine Anwendung bei Infrastrukturausfällen weiter ausgeführt werden kann. Sie müssen aber über Informationen dazu verfügen, ob ein Fehler in der Anwendung oder in der zugrunde liegenden Infrastruktur auftritt. Außerdem müssen Sie die Infrastruktur in Bezug auf die Kapazitätsplanung überwachen, damit Sie wissen, wann Infrastrukturelemente hinzugefügt oder entfernt werden müssen. Es ist wichtig, sowohl für die Infrastruktur als auch für die Anwendung, aus denen eine Service Fabric-Bereitstellung besteht, die Überwachung und Problembehandlung durchzuführen. Auch wenn eine Anwendung für Kunden verfügbar ist, kann die Infrastruktur eine potenzielle Problemquelle bleiben. Um sicherzustellen, dass der Cluster wie erwartet arbeitet, richtet Service Fabric standardmäßig fünf verschiedene Protokollierungskanäle ein:

1. Vorgangskanal: von Service Fabric und im Cluster ausgeführten Vorgänge auf höchster Ebene, einschließlich Ereignissen für einen gestarteten Knoten, eine neu bereitgestellte Anwendung oder ein Rollback für ein SF-Upgrade usw. 
2. Kundeninformationskanal: Integritätsberichte und Entscheidungen zum Lastenausgleich
3. Reliable Services-Ereignisse: spezifische Ereignisse für das Programmierungsmodell
4. Reliable Actors-Ereignisse: spezifische Ereignisse für das Programmierungsmodell und Leistungsindikatoren
5. Supportprotokolle: Systemprotokolle, die von Service Fabric nur für unseren Support generiert werden

Es wird dringend empfohlen, dass bei der Clustererstellung „Diagnose“ aktiviert wird. Dies kann wie unten gezeigt im Portal erfolgen oder mithilfe einer Azure Resource Manager-Vorlage, die die Diagnose enthält. 

![Clustererstellung im Azure-Portal mit aktivierter Diagnose](./media/service-fabric-diagnostics-overview/azure-enable-diagnostics.png)

Wie oben zu sehen, gibt es außerdem ein optionales Feld für das Hinzufügen eines Instrumentierungsschlüssels von Application Insights (AppInsights). Wenn Sie AppInsights für eine Ereignisanalyse verwenden (AppInsights ist eine der empfohlenen Lösungen), fügen Sie hier den Instrumentierungsschlüssel für die AppInsights-Ressource (GUID) ein.

#### <a name="service-fabric-support-logs"></a>Service Fabric-Supportprotokolle

Wenn Sie vom Microsoft-Support Hilfe zu Ihrem Azure Service Fabric-Cluster benötigen, sind fast immer Supportprotokolle erforderlich. Wenn Ihr Cluster in Azure gehostet wird, werden Supportprotokolle beim Erstellen eines Clusters automatisch konfiguriert und gesammelt. Die Protokolle werden in einem dedizierten Speicherkonto in der Ressourcengruppe des Clusters gespeichert. Das Speicherkonto hat keinen feststehenden Namen, aber im Konto werden Blobcontainer und -tabellen angezeigt, deren Namen mit *fabric* beginnen. Informationen zum Einrichten von Protokollsammlungen für einen eigenständigen Cluster finden Sie unter [Erstellen und Verwalten eines eigenständigen Azure Service Fabric-Clusters](service-fabric-cluster-creation-for-windows-server.md) und [Konfigurationseinstellungen für eigenständige Windows-Cluster](service-fabric-cluster-manifest.md). Für eigenständige Service Fabric-Instanzen sollten die Protokolle an eine lokale Dateifreigabe gesendet werden. Sie **benötigen** diese Protokolle für den Support, allerdings sind sie für Personen außerhalb des Microsoft-Supportteams nicht nützlich.

#### <a name="azure-service-fabric-health-and-load-reporting"></a>Integritäts- und Auslastungsberichte für Azure Service Fabric

Service Fabric weist ein eigenes Integritätsmodell auf, das in diesen Artikeln ausführlich beschrieben wird:
- [Einführung in die Service Fabric-Integritätsüberwachung](service-fabric-health-introduction.md)
- [Melden und Überprüfen der Dienstintegrität](service-fabric-diagnostics-how-to-report-and-check-service-health.md)
- [Hinzufügen von benutzerdefinierten Service Fabric-Integritätsberichten](service-fabric-report-health.md)
- [Anzeigen von Service Fabric-Integritätsberichten](service-fabric-view-entities-aggregated-health.md)

Die Systemüberwachung ist entscheidend für mehrere Aspekte beim Betrieb eines Diensts. Die Systemüberwachung ist besonders wichtig, wenn Service Fabric ein Upgrade für eine benannte Anwendung durchführt. Nachdem jede Upgradedomäne des Diensts aktualisiert wurde und für Ihre Kunden verfügbar ist, muss die Upgradedomäne Integritätsprüfungen bestehen, bevor die Bereitstellung mit der nächsten Upgradedomäne fortfährt. Wenn kein ausreichender Integritätsstatus erreicht werden kann, wird für die Bereitstellung ein Rollback durchgeführt, damit sich die Anwendung wieder in einem bekannten guten Zustand befindet. Es können sich zwar Auswirkungen für einige Kunden ergeben, bis das Rollback für die Dienste abgeschlossen ist, aber die meisten Kunden bemerken keine Fehler. Darüber hinaus wird relativ schnell für eine Lösung gesorgt, ohne dass auf eine Aktion von einem menschlichen Bediener gewartet werden muss. Je mehr Integritätsprüfungen in Ihren Code eingebunden sind, desto widerstandsfähiger ist Ihr Dienst gegenüber Problemen bei der Bereitstellung.

Ein weiterer Aspekt der Dienstintegrität sind Berichte zu Metriken des Diensts. Metriken sind in Service Fabric wichtig, da sie zum Ausgleichen der Ressourcenverwendung genutzt werden. Außerdem können Metriken ein Indikator für die Systemintegrität sein. Sie können beispielsweise über eine Anwendung mit vielen Diensten verfügen, und jede Instanz meldet eine RPS-Metrik (Requests per Second, Anforderungen pro Sekunde). Wenn ein Dienst mehr Ressourcen als ein anderer Dienst nutzt, verschiebt Service Fabric Dienstinstanzen im Cluster, um eine gleichmäßige Ressourcenverwendung zu erreichen. Eine ausführlichere Beschreibung der Ressourcenverwendung finden Sie unter [Verwalten von Ressourcenverbrauch und Auslastung in Service Fabric mit Metriken](service-fabric-cluster-resource-manager-metrics.md).

Mit Metriken können Sie auch Erkenntnisse zur Leistung Ihres Diensts gewinnen. Im Laufe der Zeit können Sie Metriken verwenden, um zu überprüfen, ob der Dienst innerhalb der erwarteten Parameter betrieben wird. Wenn Trends beispielsweise anzeigen, dass der RPS-Durchschnittswert am Montag um 9:00 Uhr bei 1.000 liegt, können Sie einen Integritätsbericht einrichten, mit dem Sie gewarnt werden, wenn der RPS-Wert unter 500 oder über 1.500 liegt. Unter Umständen ist alles in Ordnung, aber eine Überprüfung könnte nicht schaden, um sicherzustellen, dass die Benutzererfahrung für Ihre Kunden optimal ist. Ihr Dienst kann eine Gruppe von Metriken definieren, die für Integritätsprüfungen gemeldet werden können, sich aber nicht auf den Lastenausgleich von Ressourcen des Clusters auswirken. Legen Sie die Metrikgewichtung hierfür auf null fest. Es wird empfohlen, bei allen Metriken mit einer Gewichtung von null zu beginnen und die Gewichtung erst zu erhöhen, wenn Sie sicher sind, wie sich die Gewichtung der Metriken auf den Lastenausgleich von Ressourcen des Clusters auswirkt.

> [!TIP]
> Verwenden Sie nicht zu viele gewichtete Metriken. Es kann schwierig zu verstehen sein, warum Dienstinstanzen aus Gründen des Lastenausgleichs verschoben werden. Wenige Metriken können eine große Auswirkung haben!

Alle Informationen, mit denen die Integrität und Leistung Ihrer Anwendung angegeben wird, sind Kandidaten für Metriken und Integritätsberichte. Ein CPU-Leistungsindikator kann darauf hinweisen, wie ausgelastet der Knoten ist. Er informiert Sie aber nicht unbedingt darüber, ob ein bestimmter Dienst fehlerfrei ist, da ggf. mehrere Dienste auf einem einzelnen Knoten ausgeführt werden können. Aber mit Metriken wie „RPS“, „Verarbeitete Elemente“ und „Anforderungswartezeit“ kann die Integrität eines bestimmten Diensts angegeben werden.

Verwenden Sie Code wie diesen, um die Integrität zu melden:

  ```csharp
    if (!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportInstanceHealth(healthInformation);
    }
  ```

Verwenden Sie Code wie diesen, um eine Metrik zu melden:

  ```csharp
    this.ServicePartition.ReportLoad(new List<LoadMetric> { new LoadMetric("MemoryInMb", 1234), new LoadMetric("metric1", 42) });
  ```


### <a name="application-instrumenting-code-for-custom-events"></a>Anwendung: Instrumentierungscode für benutzerdefinierte Ereignisse

Das Instrumentieren des Codes ist die Grundlage für die meisten anderen Aspekte bei der Überwachung Ihrer Dienste. Die Instrumentierung ist die einzige Möglichkeit, wie Sie Fehler erkennen und die Diagnose zur Fehlerbehebung stellen können. Es ist zwar technisch möglich, einen Debugger mit einem Produktionsdienst zu verbinden, aber dies ist keine gängige Vorgehensweise. Es ist also wichtig, dass Sie über ausführliche Instrumentierungsdaten verfügen. 

Bei einigen Produkten wird Ihr Code automatisch instrumentiert. Obwohl diese Lösungen ggf. gut funktionieren, ist fast immer eine manuelle Instrumentierung erforderlich. Letztlich benötigen Sie ausreichend Informationen für das genaue Debuggen der Anwendung. In den nächsten Abschnitten werden die verschiedenen Verfahren für das Instrumentieren des Codes und die jeweiligen Vor- und Nachteile für bestimmte Fälle beschrieben.

#### <a name="eventsource"></a>EventSource

Wenn Sie eine Service Fabric-Lösung aus einer Vorlage in Visual Studio erstellen, wird eine von **EventSource** abgeleitete Klasse (**ServiceEventSource** oder **ActorEventSource**) generiert. Es wird eine Vorlage erstellt, in der Sie Ereignisse für Ihre Anwendung oder Ihren Dienst hinzufügen können. Der **EventSource**-Name **muss** eindeutig sein. Er sollte auf der Standardvorlage „MyCompany-&lt;Projektmappe&gt;-&lt;Projekt&gt;“ basieren und entsprechend umbenannt werden. Mehrere **EventSource**-Definitionen mit dem gleichen Namen führen zur Laufzeit zu Problemen. Jedes definierte Ereignis muss über einen eindeutigen Bezeichner verfügen. Wenn ein Bezeichner nicht eindeutig ist, tritt ein Laufzeitfehler auf. In einigen Organisationen werden Wertebereiche vorab Bezeichnern zugewiesen, um Konflikte zwischen separaten Entwicklungsteams zu vermeiden. Weitere Informationen finden Sie im [Blog von Vance](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/) oder in der [MSDN-Dokumentation](https://msdn.microsoft.com/library/dn774985(v=pandp.20).aspx).

##### <a name="using-structured-eventsource-events"></a>Verwenden strukturierter EventSource-Ereignisse

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

##### <a name="using-eventsource-generically"></a>Generisches Verwenden von EventSource

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

#### <a name="aspnet-core-logging"></a>ASP.NET Core-Protokollierung

Es ist wichtig, dass Sie sorgfältig planen, wie Sie Ihren Code instrumentieren. Mit dem richtigen Instrumentierungsplan können Sie ggf. verhindern, dass Ihre Codebasis destabilisiert wird und eine erneute Instrumentierung des Codes erforderlich wird. Zur Reduzierung des Risikos können Sie eine Instrumentierungsbibliothek wie [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/) wählen, die Teil von Microsoft ASP.NET Core ist. ASP.NET Core verfügt über eine [ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger)-Schnittstelle, die Sie zusammen mit dem Anbieter Ihrer Wahl nutzen können, während gleichzeitig die Auswirkungen auf den vorhandenen Code minimiert werden. Sie können den Code in ASP.NET Core unter Windows und Linux und im vollständigen .NET Framework verwenden, damit Ihr Instrumentierungscode standardisiert ist.

##### <a name="using-microsoftextensionslogging-in-service-fabric"></a>Verwenden von Microsoft.Extensions.Logging in Service Fabric

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

#### <a name="using-other-logging-providers"></a>Verwenden anderer Protokollierungsanbieter

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

4. Fügen Sie im Dienstkonstruktor den folgenden Code hinzu. Der Code erstellt die Eigenschaftserweiterungen („Property Enrichers“) für die Eigenschaften **ServiceTypeName**, **ServiceName**, **PartitionId** und **InstanceId** des Diensts. Außerdem wird der ASP.NET Core-Protokollierungsfactory eine Eigenschaftserweiterung hinzugefügt, sodass Sie Microsoft.Extensions.Logging.ILogger in Ihrem Code verwenden können.

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

  > [!NOTE]
  > Wir empfehlen Ihnen, den statischen Log.Logger nicht für das vorherige Beispiel zu verwenden. Service Fabric kann mehrere Instanzen mit dem gleichen Diensttyp innerhalb eines Prozesses hosten. Wenn Sie den statischen Log.Logger nutzen, zeigt der letzte Writer der Eigenschaftserweiterungen Werte für alle ausgeführten Instanzen an. Dies ist ein Grund dafür, dass die _logger-Variable eine private Membervariable der Dienstklasse ist. Darüber hinaus müssen Sie „_logger“ für den allgemeinen Code verfügbar machen, der ggf. dienstübergreifend verwendet wird.

#### <a name="choosing-a-logging-provider"></a>Auswählen eines Protokollierungsanbieters

Wenn Sie für Ihre Anwendung eine hohe Leistung benötigen, ist **EventSource** der am besten geeignete Ansatz. Für **EventSource** werden *im Allgemeinen* weniger Ressourcen verwendet, und es wird eine bessere Leistung als mit der ASP.NET Core-Protokollierung oder den erhältlichen Drittanbieterlösungen erzielt.  Dies ist für zahlreiche Dienste kein Problem, aber wenn Ihr Dienst leistungsorientiert ist, kann **EventSource** die bessere Wahl sein. Um die gleichen Vorteile wie bei der strukturierten Protokollierung zu erhalten, fällt für Ihre Entwickler bei **EventSource** ein höherer Aufwand an. Gehen Sie wie folgt vor, um den passenden Ansatz für Ihr Projekt zu wählen: Führen Sie eine schnelle „Prototyperstellung“ für jede Option durch, und wählen Sie dann die Option, die für Ihre Anforderungen am besten geeignet ist.

## <a name="event-aggregation-and-collection"></a>Ereignisaggregation und -sammlung

### <a name="azure-diagnostics"></a>Azure-Diagnose

Zusätzlich zu den Informationen, die von Azure Monitor bereitgestellt werden, sammelt Azure die Ereignisse der einzelnen Dienste an einem zentralen Ort. Informieren Sie sich, wie Sie die Ereignissammlung für [Windows](service-fabric-diagnostics-how-to-setup-wad.md) und [Linux](service-fabric-diagnostics-how-to-setup-lad.md) konfigurieren. In diesen Artikeln wird veranschaulicht, wie Sie die Ereignisdaten sammeln und an den Azure-Speicher senden können. Sie können dies im Azure-Portal oder in Ihrer Azure Resource Manager-Vorlage durchführen, indem Sie die Diagnose aktivieren. Mit der Azure-Diagnose werden die Daten einiger Ereignisquellen gesammelt, die von Service Fabric automatisch erstellt werden:

- **ETW**-Ereignisse werden im Vorgangskanal ausgegeben.
- **EventSource**-Ereignisse und -Leistungsindikatoren, wenn Sie das Reliable Actors-Programmiermodell verwenden. Die Ereignisse sind unter [Diagnose und Leistungsüberwachung für Reliable Actors](service-fabric-reliable-actors-diagnostics.md) aufgeführt.
- **EventSource**-Ereignisse, wenn Sie das Reliable Services-Programmiermodell verwenden. Die Ereignisse sind unter [Diagnosefunktionen für zustandsbehaftete Reliable Services](service-fabric-reliable-services-diagnostics.md) aufgeführt.


Wenn dies konfiguriert ist, werden die Ereignisse in einem Azure-Speicherkonto angezeigt, das während der Erstellung des Clusters erstellt wurde (vorausgesetzt, Sie haben die Diagnose aktiviert). Die Tabellen heißen WADServiceFabricReliableActorEventTable, WADServiceFabricReliableServiceEventTable und WADServiceFabricSystemEventTable. Integritätsereignisse werden nicht standardmäßig hinzugefügt. Sie müssen die Resource Manager-Vorlage ändern, um sie hinzuzufügen. Weitere Informationen finden Sie unter [Sammeln von Protokollen mit der Azure-Diagnose](service-fabric-diagnostics-how-to-setup-wad.md).

Die in diesem Abschnitt angegebenen Artikel enthalten auch Informationen dazu, wie Sie benutzerdefinierte Ereignisse in Azure-Speicher ablegen. Andere Artikel zur Azure-Diagnose, in denen es um die Konfiguration von Leistungsindikatoren geht, oder Artikel mit anderen Informationen zur Überwachung von virtuellen Computern zur Azure-Diagnose gelten auch für einen Service Fabric-Cluster. Wenn Sie beispielsweise nicht Azure Table Storage als Ziel verwenden möchten, helfen Ihnen die Informationen unter [Streamen von Azure-Diagnosedaten im heißen Pfad mithilfe von Event Hubs](../event-hubs/event-hubs-streaming-azure-diags-data.md) weiter. Wenn sich die Ereignisse in Azure Event Hubs befinden, können Sie sie lesen und an den Ort Ihrer Wahl senden. Sie können sich auch über [Azure Diagnostics integration with Application Insights](https://azure.microsoft.com/blog/azure-diagnostics-integration-with-application-insights/) (Integration der Azure-Diagnose in Application Insights) informieren.

Ein Nachteil bei der Verwendung der Azure-Diagnose ist, dass Sie sie mithilfe einer Resource Manager-Vorlage einrichten. Die Diagnose wird dann nur auf Ebene der VM-Skalierungsgruppe durchgeführt. In Service Fabric entspricht eine VM-Skalierungsgruppe einem Knotentypen. Sie konfigurieren jeden Knotentyp für alle Anwendungen und Dienste, die auf einem Knoten dieses Typs ausgeführt werden können. Dies können, je nach konfigurierter Anzahl von Anwendungen und Diensten, viele **EventSource**-Ereignisse sein. Außerdem müssen Sie Resource Manager jedes Mal bereitstellen, wenn sich eine Anwendungskonfiguration ändert. Im Idealfall ist die Überwachungskonfiguration mit der Dienstkonfiguration verknüpft.

Die Azure-Diagnose funktioniert nur für Service Fabric-Cluster, die in Azure bereitgestellt wurden. Dies gilt sowohl für Windows- als auch für Linux-Cluster.

### <a name="eventflow"></a>EventFlow

Mit dem [EventFlow-Element der Microsoft-Diagnose](https://github.com/Azure/diagnostics-eventflow) können Ereignisse von einem Knoten an mindestens ein Überwachungsziel geleitet werden. Da die entsprechenden Daten als NuGet-Paket in Ihrem Dienstprojekt enthalten sind, werden der EventFlow-Code und die -Konfiguration mit dem Dienst übertragen, sodass das oben erwähnte Problem mit der Konfiguration pro Knoten für die Azure-Diagnose nicht besteht. EventFlow wird im Dienstprozess ausgeführt und direkt mit den konfigurierten Ausgaben verbunden. Aufgrund der direkten Verbindung funktioniert EventFlow für Azure-, Container- und lokale Bereitstellungen von Diensten. Gehen Sie mit Bedacht vor, wenn Sie EventFlow in Szenarien mit hoher Dichte ausführen, z.B. in einem Container, da für jede EventFlow-Pipeline eine externe Verbindung hergestellt wird. Wenn Sie viele Prozesse hosten, ergibt sich so eine hohe Zahl von ausgehenden Verbindungen! Dies ist für Service Fabric-Anwendungen kein größeres Problem, da alle Replikate eines `ServiceType`-Elements in demselben Prozess ausgeführt werden, sodass die Anzahl von ausgehenden Verbindungen begrenzt wird. EventFlow ermöglicht auch die Ereignisfilterung, sodass nur die Ereignisse gesendet werden, die dem angegebenen Filter entsprechen. Ausführliche Informationen zur Verwendung von EventFlow mit Service Fabric finden Sie unter [Sammeln von Protokollen direkt aus einem Azure Service Fabric-Dienstprozess](service-fabric-diagnostic-collect-logs-without-an-agent.md).

Gehen Sie wie folgt vor, um EventFlow zu verwenden:

1. Fügen Sie Ihrem Dienstprojekt das NuGet-Paket hinzu.
2. Erstellen Sie in der **Main**-Funktion des Diensts die EventFlow-Pipeline, und konfigurieren Sie anschließend die Ausgaben. Im folgenden Beispiel verwenden wir Serilog als Ausgabe.

  ```csharp
  internal static class Program
  {
      /// <summary>
      /// This is the entry point of the service host process.
      /// </summary>
      private static void Main()
      {
          try
          {
              using (var pipeline = ServiceFabricDiagnosticPipelineFactory.CreatePipeline("MonitoringE2E-Stateless-Pipeline"))
              {
                  Log.Logger = new LoggerConfiguration().WriteTo.EventFlow(pipeline).CreateLogger();

                  // The ServiceManifest.xml file defines one or more service type names.
                  // Registering a service maps a service type name to a .NET type.
                  // When Service Fabric creates an instance of this service type,
                  // an instance of the class is created in this host process.

                  ServiceRuntime.RegisterServiceAsync("StatelessType", context => new Stateless(context, Log.Logger)).GetAwaiter().GetResult();
                  ServiceEventSource.Current.ServiceTypeRegistered(Process.GetCurrentProcess().Id, typeof(Stateless).Name);

                  // Prevents this host process from terminating, so services keep running.
                  Thread.Sleep(Timeout.Infinite);
              }
          }
          catch (Exception e)
          {
              ServiceEventSource.Current.ServiceHostInitializationFailed(e.ToString());
              throw;
          }
      }
  }
  ```

3. Erstellen Sie die Datei „eventFlowConfig.json“ im Ordner „\\PackageRoot\\Config“ des Diensts. In der Datei sieht die Konfiguration wie folgt aus:

  ```json
      {
      "inputs": [
          {
          "type": "EventSource",
          "sources": [
              { "providerName": "Microsoft-ServiceFabric-Services" },
              { "providerName": "Microsoft-ServiceFabric-Actors" },
              { "providerName": "MyCompany-MonitoringE2E-Stateless" }
          ]
          },
          {
          "type": "Serilog"
          }
      ],
      "filters": [
          {
          "type": "drop",
          "include": "Level == Verbose"
          },
          {
          "type": "metadata",
          "metadata": "request",
          "requestNameProperty": "RequestName",
          "include":  "RequestName==MyRequest",
          "durationProperty": "Duration",
          "durationUnit": "milliseconds"
          }
      ],
      "outputs": [
          {
          "type": "StdOutput"
          },
          {
          "type": "ApplicationInsights",
          "instrumentationKey": "== instrumentation key here =="
          }
      ],
      "schemaVersion": "2016-08-11",
      "extensions": []
      }
  ```
    In der Konfiguration sind zwei Eingaben definiert: die beiden **EventSource**-basierten Quellen, die von Service Fabric erstellt werden, und das **EventSource**-Element für den Dienst. Die System- und Integritätsereignisse, die ETW nutzen, sind für EventFlow nicht verfügbar. Die Ursache ist, dass erhöhte Rechte erforderlich sind, um auf eine ETW-Quelle zu lauschen, und Dienste sollten nie mit erhöhten Rechten ausgeführt werden. Die andere Eingabe ist Serilog. Die Serilog-Konfiguration erfolgt in der **Main**-Methode.  Es werden einige Filter angewendet. Mit dem ersten Filter wird EventFlow angewiesen, alle Ereignisse mit der Ereignisebene „Verbose“ (Ausführlich) zu verwerfen. Zwei Ausgaben werden konfiguriert: die Standardausgabe, die im Ausgabefenster in Visual Studio erfolgt, und Application Insights. Achten Sie darauf, dass Sie Ihren Instrumentierungsschlüssel hinzufügen.

4. Instrumentieren Sie den Code. Im nächsten Beispiel instrumentieren wir `RunAsync` auf unterschiedliche Arten, um einige Beispiele anzuzeigen. Im folgenden Code verwenden wir noch Serilog. Einige der verwendeten Syntaxelemente gelten spezifisch für Serilog. Achten Sie auf die besonderen Funktionen für die Protokollierungslösung, die Sie auswählen. Drei Ereignisse werden generiert: ein Ereignis für die Debugebene und zwei Ereignisse zu Informationszwecken. Mit dem zweiten Informationsereignis wird die Anforderungsdauer nachverfolgt. In der oben beschriebenen Konfiguration von EventFlow sollte das Ereignis der Debugebene nicht in die Ausgabe fließen.

  ```csharp
      Stopwatch sw = Stopwatch.StartNew();

      while (true)
      {
          cancellationToken.ThrowIfCancellationRequested();

          sw.Restart();

          // Delay a random interval, to provide a more interesting request duration.
          await Task.Delay(TimeSpan.FromMilliseconds(DateTime.Now.Millisecond), cancellationToken);

          ServiceEventSource.Current.ServiceMessage(this.Context, "Working-{0}", ++iterations);
          _logger.LogDebug("Debug level event from Microsoft.Logging");
          _logger.LogInformation("Informational level event from Microsoft.Logging");
          _logger.LogInformation("{RequestName} {Duration}", "MyRequest", sw.ElapsedMilliseconds);
      }
  ```

Navigieren Sie im Azure-Portal zu Ihrer Application Insights-Ressource, um die Ereignisse in Application Insights anzuzeigen. Wählen Sie das Feld **Suche**, um die Ereignisse anzuzeigen.

![Anzeigen von Ereignissen in der Application Insights-Suche](./media/service-fabric-diagnostics-overview/ai-search-events.png)

Die Ablaufverfolgungen sind im obigen Screenshot im unteren Bereich zu sehen. Es werden nur zwei Ereignisse angezeigt, und es wird darauf hingewiesen, dass das Ereignis der Debugebene von EventFlow verworfen wurde. Der Anforderungseintrag vor der Ablaufverfolgung ist die dritte `_logger`-Instrumentierungszeile. In der Zeile wird angezeigt, dass das Ereignis in Application Insights in eine Anforderungsmetrik übersetzt wurde.

In der Filterdefinition lautet der Typ **metadata**. Hiermit wird deklariert, dass ein Ereignis die `RequestName`-Eigenschaft mit dem Wert `MyRequest` aufweist, und die `Duration`-Eigenschaft enthält die Dauer der Anforderung in Millisekunden. Dies wird im Anforderungsereignis in Application Insights angezeigt. Der gleiche Ansatz funktioniert für alle unterstützten EventFlow-Eingaben, einschließlich **EventSource**.

Wenn Sie über einen eigenständigen Cluster verfügen, der aus Richtliniengründen nicht mit einer cloudbasierten Lösung verbunden werden kann, können Sie Elasticsearch als Ausgabe verwenden. Es können aber andere Ausgaben geschrieben werden, und die Verwendung von Pull Requests wird empfohlen. Einige Drittanbieter für die ASP.NET Core-Protokollierung bieten auch Lösungen, die lokale Installationen unterstützen.

## <a name="visualization-analysis-and-alerts"></a>Visualisierung, Analyse und Warnungen

Zum letzten Teil der Überwachung gehören die Visualisierung des Ereignisdatenstroms, Berichte zur Leistung des Diensts und Warnungen bei Problemen. Sie können für diesen Aspekt der Überwachung verschiedene Lösungen verwenden. Zum Erzeugen von Warnungen basierend auf dem Ereignisdatenstrom können Sie Azure AppInsights und Operations Management Suite (OMS) verwenden. Zum Visualisieren von Daten können Sie Microsoft Power BI oder eine Drittanbieterlösung wie [Kibana](https://www.elastic.co/products/kibana) oder [Splunk](https://www.splunk.com/) verwenden.

### <a name="appinsights"></a>AppInsights

AppInsights ist eines der empfohlenen Tools zum Überwachen von Anwendungen und Diensten. Das aktualisierte AI.SDK eignet sich gut für Service Fabric-Ereignisse. Neben der Bereitstellung guter Datenvisualisierungen und eines Abfragetools (über AppInsights Analytics) kann es auch eine präzise AppMap erstellen, die Ihnen helfen kann, Abhängigkeiten zwischen Prozessen in einer Anwendung oder einem Cluster zu verfolgen.

Sie richten eine AppInsights-Ressource ein, indem Sie im Azure Marketplace nach „Application Insights“ suchen. Wechseln Sie nach der Erstellung zu den *Eigenschaften*, um den AI-Instrumentierungsschlüssel (in Form einer GUID) zu suchen. Dieser wird für Folgendes verwendet:
* Integrieren von AppInsights zum Empfangen von Ereignissen auf Infrastrukturebene von einem Service Fabric-Cluster direkt über eine Azure Resource Manager-Vorlage oder über das Azure-Portal bei der Erstellung eines Clusters, sofern die Diagnose aktiviert wurde
* Konfigurieren von EventFlow („eventFlowConfig.json“) für die Ausgabe von Daten an Application Insights, wie im Abschnitt oben gezeigt

### <a name="oms"></a>OMS

OMS ist ein weiteres empfohlenes Tool für die Diagnose und Überwachung von Service Fabric-Clustern. Die Service Fabric-Lösung kann einem beliebigen Arbeitsbereich hinzugefügt werden. Sie bietet ein Dashboard, auf dem die verschiedenen Typen von Service Fabric-Ereignissen angezeigt werden. OMS-Arbeitsbereiche verfügen außerdem mit Log Analytics über ein leistungsfähiges Protokollabfragetool.

Um einen OMS-Arbeitsbereich zu konfigurieren, vergewissern Sie sich, dass die Diagnose für Ihren Cluster aktiviert wurde. Fügen Sie „Service Fabric-Analyse“ aus dem Azure Marketplace einem vorhandenen OMS-Arbeitsbereich hinzu, oder erstellen Sie einen neuen. Konfigurieren Sie für die Datenquellen des Arbeitsbereichs Verbindungen mit den Azure Storage-Tabellen, in die Ihr Cluster Ereignisse schreibt. 

Damit OMS benutzerdefinierte Ereignissen annehmen kann, müssen Sie auch sicherstellen, dass die Instrumentierung, die Sie Ihren Anwendungen hinzufügen, auch in dieselben Storage-Tabellen schreibt oder in andere, die ebenfalls als Datenquellen für den Arbeitsbereich konfiguriert wurden. 

OMS ist derzeit auch das empfohlene Tool für die Datenvisualisierung und -analyse, wenn Sie eine Überwachung und Diagnose für Container benötigen, da Sie Ihrem Arbeitsbereich eine Containerlösung hinzufügen können, die gut bei Containern funktioniert, die in Service Fabric orchestriert werden. Eine kurze Anleitung für die Einrichtung finden Sie [hier](service-fabric-diagnostics-containers-windowsserver.md).

### <a name="azure-monitor"></a>Azure Monitor

Sie können [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview.md) zum Überwachen von vielen Azure-Ressourcen verwenden, auf denen ein Service Fabric-Cluster erstellt wird. Ein Satz von Metriken für die [VM-Skalierungsgruppe](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftcomputevirtualmachinescalesets) (Virtual Machine Scale Set, VMSS) und einzelne [virtuelle Computer](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftcomputevirtualmachinescalesetsvirtualmachines) wird automatisch gesammelt und im Azure-Portal angezeigt. Wählen Sie zum Anzeigen der gesammelten Informationen im Azure-Portal die Ressourcengruppe aus, die den Service Fabric-Cluster enthält. Wählen Sie anschließend die VM-Skalierungsgruppe aus, die Sie anzeigen möchten. Wählen Sie im Abschnitt **Überwachung** die Option **Metriken**, um einen Graphen mit den Werten anzuzeigen.

![Ansicht des Azure-Portals mit gesammelten Metrikinformationen](./media/service-fabric-diagnostics-overview/azure-monitoring-metrics.png)

Führen Sie die Schritte der Anleitung unter [Überblick über Metriken in Microsoft Azure](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) aus, um die Diagramme anzupassen. Sie können basierend auf diesen Metriken auch Warnungen erstellen, wie unter [Erstellen von Warnungen in Azure Monitor für Azure-Dienste](../monitoring-and-diagnostics/insights-alerts-portal.md) beschrieben. Sie können Warnungen an einen Benachrichtigungsdienst senden, indem Sie Webhooks verwenden. Dies ist unter [Konfigurieren eines Webhooks für eine Azure-Metrikwarnung](../monitoring-and-diagnostics/insights-webhooks-alerts.md) beschrieben. Für Azure Monitor wird nur ein Abonnement unterstützt. Wenn Sie mehrere Abonnements überwachen müssen oder zusätzliche Features benötigen, können Sie [Log Analytics](https://azure.microsoft.com/documentation/services/log-analytics/) verwenden. Log Analytics ist Teil der Microsoft Operations Management Suite und stellt eine umfassende IT-Verwaltungslösung für lokale und cloudbasierte Infrastrukturen bereit. Sie können Daten von Azure Monitor direkt an Log Analytics weiterleiten, damit sich die Metriken und Protokolle für Ihre gesamte Umgebung an einem Ort befinden.

Wir empfehlen Ihnen, die Operations Management Suite zum Überwachen Ihrer lokalen Infrastruktur zu verwenden, aber Sie können auch andere verfügbare Lösungen nutzen, die von Ihrer Organisation zur Überwachung der Infrastruktur eingesetzt werden.

## <a name="additional-steps"></a>Zusätzliche Schritte

### <a name="watchdogs"></a>Watchdogs

Ein Watchdog ist ein separater Dienst, der die Integrität und die Auslastung von Diensten überwachen und die Integrität für alle Elemente in der Hierarchie des Integritätsmodells melden kann. So können Fehler verhindert werden, die nicht erkannt werden, wenn nur ein einzelner Dienst betrachtet wird. Watchdogs sind auch ein guter Platz zum Hosten von Code, der Aktionen zur Problembehebung bei bekannten Bedingungen ohne Interaktion von Benutzern ausführen kann. Ein Beispiel für eine Watchdog-Dienstimplementierung finden Sie [hier](https://github.com/Azure-Samples/service-fabric-watchdog-service).


## <a name="next-steps"></a>Nächste Schritte

* [Sammeln von Protokollen mit Azure-Diagnose](service-fabric-diagnostics-how-to-setup-wad.md)
* [Sammeln von Protokollen direkt aus einem Azure Service Fabric-Dienstprozess](service-fabric-diagnostic-collect-logs-without-an-agent.md)
*  [Verwalten von Ressourcenverbrauch und Auslastung in Service Fabric mit Metriken](service-fabric-cluster-resource-manager-metrics.md)

