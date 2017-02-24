---
title: "Übersicht über Azure Service Fabric-Überwachung und -Diagnose | Microsoft-Dokumentation"
description: "Erfahren Sie, wie die Überwachung und Diagnose von Microsoft Azure Service Fabric-Anwendungen, die in Azure, einer Entwicklungsumgebung oder lokal gehostet werden, durchgeführt wird."
services: service-fabric
documentationcenter: .net
author: ms-toddabel
manager: mfussell
editor: 
ms.assetid: edcc0631-ed2d-45a3-851d-2c4fa0f4a326
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/9/2017
ms.author: toddabel
translationtype: Human Translation
ms.sourcegitcommit: a83fe451ec8b77b8b84575c5fd46f3661fc261c0
ms.openlocfilehash: 77e5872654aa6f50b5a6564fb8ee8c7a6a4c29a5


---
# <a name="monitor-and-diagnose-azure-service-fabric-applications"></a>Überwachen und Diagnostizieren von Azure Service Fabric-Anwendungen

Durch Überwachung, Erkennung, Diagnose und Problembehandlung können Dienste mit minimalen Störungen für die Benutzer ausgeführt, Geschäftsinformationen bereitgestellt, die Ressourcenverwendung überwacht, Hardware- und Softwarefehler oder Leistungsprobleme erkannt und potenzielle Probleme beim Dienstverhalten diagnostiziert werden. Die Überwachung und Diagnose sind in einer tatsächlichen bereitgestellten Produktionsumgebung zwar wichtig, die Wirksamkeit hängt aber davon ab, ob bei der Entwicklung von Diensten ein ähnliches Modell verwendet wird. Nur so ist sichergestellt, dass die Dienste später tatsächlich funktionieren. Service Fabric erleichtert Dienstentwicklern das Implementieren von Diagnosen, die sowohl in einer lokalen Umgebung auf einem einzelnen Computer als auch in der tatsächlichen Konfiguration in einem Produktionscluster nahtlos verwendet werden können. 

Überwachung ist ein umfassender Begriff, der Folgendes abdeckt: Instrumentierung von Code, Erfassung der Instrumentierungsprotokolle, Analyse der Protokolle, Visualisierung der Erkenntnisse basierend auf den Protokolldaten, Warnungen anhand der Protokollwerte und Erkenntnisse, Überwachung der Infrastruktur und die Möglichkeit, Probleme zu erkennen und zu diagnostizieren, die Kunden betreffen. Dieser Artikel soll eine Übersicht über die Überwachung für Service Fabric-Cluster bieten, die in Azure oder lokal gehostet werden und unter Windows oder Linux mit .NET bereitgestellt werden. Beginnen wir, indem wir das Problem in drei Teile unterteilen.
- Instrumentierung von Code oder der Infrastruktur
- Sammlung von generierten Ereignissen
- Speicherung, Aggregation, Visualisierung und Analyse

Zwar gibt es einige Produkte, die alle drei Aspekte abdecken, viele Kunden wählen jedoch unterschiedliche Technologien für jeden Bereich aus. Es ist wichtig, dass diese jeweils zusammen passen, um eine komplette Überwachungslösung für die Anwendung bereitzustellen. 

## <a name="monitoring-infrastructure"></a>Überwachen der Infrastruktur

Service Fabric hilft dabei, dass eine Anwendung bei Infrastrukturausfällen weiter ausgeführt wird. Die Betreiber der Anwendung müssen allerdings verstehen, ob ein Fehler in der Anwendung oder in der zugrunde liegenden Infrastruktur auftritt. Die Überwachung der Infrastruktur ist auch für die Kapazitätsplanung erforderlich, um zu wissen, wann die Infrastruktur erweitert oder verkleinert werden muss. Die Infrastruktur und die Anwendung, die eine Service Fabric-Bereitstellung bilden, müssen überwacht und bei Problemen entsprechend behandelt werden. Sofern die Anwendung den Kunden weiterhin zur Verfügung steht, können Teile der Infrastruktur Probleme aufweisen.

### <a name="azure-monitoring"></a>Azure-Überwachung

Für in Azure bereitgestellte Cluster bietet die [Azure-Überwachung](../monitoring-and-diagnostics/monitoring-overview.md) die Möglichkeit, viele der Azure-Ressourcen zu überwachen, auf denen ein Service Fabric-Cluster basiert. Ein Satz von Metriken wird automatisch im Azure-Portal für die [VM-Skalierungsgruppe (Virtual Machine Scale Set, VMSS)](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftcomputevirtualmachinescalesets) und einzelne [VMs](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftcomputevirtualmachinescalesetsvirtualmachines) gesammelt und angezeigt. Diese Informationen können im Azure-Portal angezeigt werden, indem die Ressourcengruppe ausgewählt wird, in der der Service Fabric-Cluster enthalten ist, und dann die anzuzeigende VMSS ausgewählt wird. Wählen Sie anschließend „Metriken“ im Navigationsbereich „Überwachung“ aus, um die Werte in einem Diagramm anzuzeigen.

![Ansicht des Azure-Portals mit gesammelten Metrikinformationen](./media/service-fabric-diagnostics-overview/azure-monitoring-metrics.png)

Die Diagramme können mithilfe der Anweisungen im Artikel [Überblick über Metriken in Microsoft Azure](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) angepasst werden. Zudem können Warnungen basierend auf diesen Metriken erstellt werden. Dies ist im Artikel [Verwenden des Azure-Portals zum Erstellen von Warnungen für Azure-Dienste](../monitoring-and-diagnostics/insights-alerts-portal.md) beschrieben. Warnungen können mithilfe von Webhooks an einen Benachrichtigungsdienst gesendet werden. Dies ist im Artikel [Konfigurieren eines Webhooks für eine Azure-Metrikwarnung(../monitoring-and-diagnostics/insights-webhooks-alerts.md) beschrieben. Die Azure-Überwachung unterstützt ein einzelnes Abonnement. Wenn Unterstützung für mehrere Abonnements oder weitere Features benötigt wird, bietet [Log Analytics](https://azure.microsoft.com/documentation/services/log-analytics/) als Teil der Operations Management Suite eine umfassende IT-Verwaltungslösung für lokale und cloudbasierte Infrastrukturen. Daten der Azure-Überwachung können direkt an Log Analytics weitergeleitet werden, sodass Sie die Metriken und Protokolle für Ihre gesamte Umgebung an einem Ort finden.

Die Operations Management Suite ist die empfohlene Methode für die Überwachung Ihrer lokalen Infrastruktur, es kann aber jede vorhandene Lösung, die Ihr Unternehmen für die Infrastrukturüberwachung nutzt, verwendet werden.

### <a name="service-fabric-support-logs"></a>Service Fabric-Supportprotokolle

Wenn Sie vom Microsoft-Support Hilfe zu Ihrem Azure Service Fabric-Cluster benötigen, sind fast immer Supportprotokolle erforderlich. Wenn Ihr Cluster in Azure gehostet wird, werden diese Protokolle beim Erstellen eines Clusters automatisch konfiguriert und gesammelt. Die Protokolle werden in einem dedizierten Speicherkonto gespeichert, das in der Ressourcengruppe des Clusters angezeigt werden kann. Es gibt keinen festen Namen für das Speicherkonto, jedoch werden im Konto Blobcontainer und Tabellen angezeigt, die mit „fabric“ beginnen. Wenn Ihr Cluster ein eigenständiger Cluster ist, sollten Sie die Sammlung dieser Protokolle gemäß der Anleitungen unter [Erstellen und Verwalten eines eigenständigen Azure Service Fabric-Clusters](service-fabric-cluster-creation-for-windows-server.md) und [Konfigurationseinstellungen für eigenständige Windows-Cluster](service-fabric-cluster-manifest.md) konfigurieren. Für eigenständige Service Fabric-Cluster sollten die Protokolle an eine lokale Dateifreigabe gesendet werden. Sie **benötigen** diese Protokolle für den Support, allerdings sind sie für Personen außerhalb des Microsoft-Supportteams nicht nützlich. Es sind wahrscheinlich auch nicht die Protokolle, an denen Sie interessiert sind.

## <a name="instrument-your-code"></a>Instrumentieren des Codes

Das Instrumentieren des Codes ist die Grundlage für die meisten anderen Aspekte bei der Überwachung Ihrer Dienste. Es ist häufig überraschend, wie viel Instrumentierung erforderlich ist. Bedenken Sie jedoch, dass diese Instrumentierung Ihre einzige Möglichkeit ist festzustellen, dass es Probleme gibt, und zu diagnostizieren, was korrigiert werden muss. Zwar ist es technisch möglich, einen Debugger mit einem Produktionsdienst zu verbinden, jedoch ist es nicht üblich. Daher sind ausführliche Instrumentierungsdaten wichtig. Wenn diese Menge an Informationen erzeugt wird, kann die Übertragung aller Ereignisse vom lokalen Knoten ressourcenintensiv sein. Viele Dienste verwenden eine zweiteilige Strategie für den Umgang mit der Menge an Instrumentierungsdaten:
* Alle Ereignisse werden für wenige Tage in einer lokalen rollierenden Protokolldatei gespeichert und nur erfasst, wenn dies für das Debuggen erforderlich ist. In der Regel bleiben die Ereignisse, die für eine ausführliche Diagnose erforderlich sind, auf den Knoten, um die Kosten und die Ressourcenverwendung reduzieren.
* Alle Ereignisse zur Dienstintegrität wie Fehlerereignisse, Taktereignisse oder Leistungsereignisse werden an ein zentrales Repository gesendet und können zum Auslösen von Warnungen zu fehlerhaften Diensten verwendet werden.

Es gibt Lösungen, die den Code automatisch instrumentieren. Zwar können diese Produkte gut funktionieren, eine manuelle Instrumentierung ist aber fast immer erforderlich. Letztlich benötigen Sie ausreichend Informationen für das genaue Debuggen der Anwendung. Die nächsten Abschnitte beschreiben die verschiedenen Verfahren für das Instrumentieren des Codes und geben an, wann welches gewählt werden sollte.

### <a name="eventsource"></a>EventSource

Wenn Sie eine Azure Service Fabric-Lösung aus einer Vorlage in Visual Studio erstellen, wird eine von EventSource abgeleitete Klasse (*ServiceEventSource* oder *ActorEventSource*) generiert. Dadurch erhalten Sie eine Vorlage, der Sie zusätzliche Ereignisse hinzufügen können, die für die Anwendung oder den Dienst geeignet sind. Der EventSource-Name **muss** eindeutig sein. Er sollte auf der Ausgangszeichenfolge „MyCompany-&lt;Lösung&gt;-&lt;Projekt&gt;“ basieren. Mehrere EventSource-Definitionen mit dem gleichen Namen führen zu Problemen zur Laufzeit. Jedes definierte Ereignis muss über einen eindeutigen Bezeichner verfügen. Wenn ein Bezeichner nicht eindeutig ist, tritt ein Laufzeitfehler auf. Häufig werden Wertebereiche vorab zu Bezeichnern zugeordnet, um Konflikte zwischen separaten Entwicklungsteams zu vermeiden. Weitere Informationen zu EventSource finden Sie im [Blog von Vance](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/) oder in der [MSDN-Dokumentation](https://msdn.microsoft.com/library/dn774985(v=pandp.20).aspx).

#### <a name="using-structured-eventsource-events"></a>Verwenden strukturierter EventSource-Ereignisse

Jedes der folgenden Ereignisse wurde für einen bestimmten Anwendungsfall definiert, beispielsweise wurde ein Diensttyp registriert. Wenn Meldungen auf diese Weise definiert werden, können Daten zusammen mit dem Text des Fehlers gepackt werden. Dies ermöglicht Verbesserungen beim Suchen und Filtern basierend auf den Namen oder Werten der angegebenen Eigenschaften. Durch Strukturieren der Instrumentierungsausgabe wird eine Nutzung einfacher, jedoch sind für das Definieren eines neuen Ereignisses für jeden Verwendungsfall mehr Überlegungen und Zeit notwendig. Einige Ereignisdefinitionen können für die gesamte Anwendung genutzt werden, z.B. könnte ein Start- oder Stoppereignis einer Methode für viele Dienste in einer Anwendung wiederverwendet werden. Ein domänenspezifischer Dienst, z.B. ein Auftragssystem, kann ein CreateOrder-Ereignis mit einem eigenen eindeutigen Ereignis aufweisen. Häufig werden bei diesem Ansatz viele Ereignisse generiert, sodass möglicherweise die Koordinierung von Bezeichnern in verschiedenen Projektteams erforderlich wird. Ein vollständiges Beispiel strukturierter EventSources in Service Fabric ist [PartyCluster.ApplicationDeployService.ServiceEventSource](https://github.com/Azure-Samples/service-fabric-dotnet-management-party-cluster/blob/master/src/PartyCluster.ApplicationDeployService/ServiceEventSource.cs) im Party Cluster-Beispiel.

```csharp
    [EventSource(Name = "MyCompany-VotingState-VotingStateService")]
    internal sealed class ServiceEventSource : EventSource
    {
        public static readonly ServiceEventSource Current = new ServiceEventSource();

        // Instance constructor is private to enforce singleton semantics
        private ServiceEventSource() : base() { }

        ...

        // ServiceTypeRegistered event contains a unique identifier, an event attribute that defined the event and the code implementation of the event.
        private const int ServiceTypeRegisteredEventId = 3;
        [Event(ServiceTypeRegisteredEventId, Level = EventLevel.Informational, Message = "Service host process {0} registered service type {1}", Keywords = Keywords.ServiceInitialization)]
        public void ServiceTypeRegistered(int hostProcessId, string serviceType)
        {
            WriteEvent(ServiceTypeRegisteredEventId, hostProcessId, serviceType);
        }

        // ServiceHostInitializationFailed event contains a unique identifier, an event attribute that defined the event and the code implementation of the event.
        private const int ServiceHostInitializationFailedEventId = 4;
        [Event(ServiceHostInitializationFailedEventId, Level = EventLevel.Error, Message = "Service host initialization failed", Keywords = Keywords.ServiceInitialization)]
        public void ServiceHostInitializationFailed(string exception)
        {
            WriteEvent(ServiceHostInitializationFailedEventId, exception);
        }
```
#### <a name="using-eventsource-generically"></a>Generisches Verwenden von EventSource

Da das Definieren bestimmter Ereignisse schwierig sein kann, werden häufig nur wenige Ereignisse mit einem gemeinsamen Satz von Parametern definiert, die in der Regel ihre Informationen als Zeichenfolge ausgegeben. Dadurch geht ein großer Teil der Strukturierung verloren, weshalb es schwieriger ist, die Ergebnisse zu durchsuchen und zu filtern. Bei diesem Ansatz werden einige Ereignisse definiert, die in der Regel den Protokolliergraden entsprechen. Der folgende Codeausschnitt definiert eine Debug- und Fehlermeldung.
```csharp
    [EventSource(Name = "MyCompany-VotingState-VotingStateService")]
    internal sealed class ServiceEventSource : EventSource
    {
        public static readonly ServiceEventSource Current = new ServiceEventSource();

        // Instance constructor is private to enforce singleton semantics
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
Die Nutzung eines Hybridansatzes aus strukturierter und generischer Instrumentierung kann auch gut funktionieren. Dabei wird die strukturierte Instrumentierung für Berichte zu Fehlern und Metriken verwendet, während generische Ereignisse für die ausführliche Protokollierung eingesetzt werden können, die zur Problembehandlung genutzt werden.

### <a name="aspnet-core-logging"></a>ASP.NET Core-Protokollierung

Die Auswahl des Verfahrens zum Instrumentieren des Codes kann schwierig sein. Wenn Sie Ihre Entscheidung revidieren und die Instrumentierung wiederholen müssen, ändern Sie wieder die Codebasis und destabilisieren sie dadurch möglicherweise. Um das Risiko zu reduzieren, können Entwickler eine Instrumentierungsbibliothek wie z.B. [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/) von ASP.NET Core wählen. Diese bietet eine [ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger)-Schnittstelle, mit der ein Anbieter Ihrer Wahl verwendet werden kann und gleichzeitig die Auswirkungen auf den vorhandenen Code minimiert werden. Eine weiterer vorteilhafter Aspekt ist, dass der Code nicht nur in .NET Core unter Windows und Linux, sondern auch im vollständigen Framework verwendet werden kann. Dadurch haben Sie die Möglichkeit, Instrumentierungscode in .NET und .NET Core zu standardisieren.

#### <a name="how-to-use-microsoftextensionslogging-within-service-fabric"></a>Verwenden von Microsoft.Extensions.Logging in Service Fabric

1. Fügen Sie dem Projekt, das Sie instrumentieren möchten, das NuGet-Paket **Microsoft.Extensions.Logging** hinzu. Sie sollten auch alle Anbieterpakete hinzufügen. Im Folgenden machen wir dies für ein Drittanbieterpaket. Weitere Informationen finden Sie unter [Logging in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging) (Protokollierung in ASP.NET Core).

2. Fügen Sie der Dienstdatei eine **using**-Direktive für Microsoft.Extensions.Logging hinzu.

3. Definieren Sie eine private Variable innerhalb Ihrer Dienstklasse.

    ```csharp
        private ILogger _logger = null;
    ```

4. Fügen Sie im Konstruktor der Dienstklasse Folgendes hinzu:
    
    ```csharp
        _logger = new LoggerFactory().CreateLogger<Stateless>();
    ```

5. Beginnen Sie in den Methoden, den Code zu instrumentieren. Es folgen einige Beispiele:
    
    ```csharp

        _logger.LogDebug("Debug level event from Microsoft.Logging");
        _logger.LogInformation("Informational level event from Microsoft.Logging");

        // In this variant, we're adding structured properties RequestName and Duration that has values MyRequest and the duration of the request.
        // More on why you'll want to do this later.
        _logger.LogInformation("{RequestName} {Duration}", "MyRequest", requestDuration);

    ```

#### <a name="using-other-logging-providers"></a>Verwenden anderer Protokollierungsanbieter

Einige Anbieter von Drittanbietern funktionieren mit diesem Ansatz. [SeriLog](https://serilog.net/), [NLog](http://nlog-project.org/) und [loggr](https://github.com/imobile3/Loggr.Extensions.Logging) sind drei Beispiele. Jeder dieser Anbieter kann in die ASP.Net Core-Protokollierung integriert und auch separat verwendet werden. SeriLog verfügt über eine Funktion, die eine Anreicherung aller von einem Protokollierungstool gesendeten Meldungen ermöglicht. Dies kann hilfreich sein, um den Dienstnamen, den Typ und Partitionsinformationen auszugeben. Um diese Funktion in der ASP.NET Core-Infrastruktur zu verwenden, gehen Sie wie folgt vor:

1. Fügen Sie die NuGet-Pakete **Serilog**, **Serilog.Extensions.Logging**, **Serilog.Sinks.Observable** zum Projekt hinzu. Fügen Sie in diesem Beispiel auch **SeriLog.Sinks.Literate** hinzu. Ein besserer Ansatz wird weiter unten in diesem Artikel gezeigt.
2. Erstellen Sie eine LoggerConfiguration und die Protokollierungsinstanz in SeriLog.

    ```csharp

        Log.Logger = new LoggerConfiguration().WriteTo.LiterateConsole().CreateLogger();

    ```

3. Fügen Sie dem Dienstkonstruktor ein SeriLog.ILogger-Argument hinzu, und übergeben Sie die neu erstellte Protokollierung.
    
    ```csharp

        ServiceRuntime.RegisterServiceAsync("StatelessType", context => new Stateless(context, Log.Logger)).GetAwaiter().GetResult();

    ```

4. Fügen Sie im Dienstkonstruktor Folgendes ein. Damit werden die Eigenschaften „ServiceTypeName“, „ServiceName“, „PartitionId“ und „InstanceId“ des Diensts erweitert. Auch die ASP.NET Core-Protokollierungsfactory wird entsprechend erweitert, sodass „Microsoft.Extensions.Logging.ILogger“ in Ihrem Code verwendet werden kann.
    
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
> Es ist nicht empfehlenswert, den statischen Log.Logger bei diesem Ansatz zu verwenden, da Service Fabric mehrere Instanzen des gleichen Diensttyps innerhalb eines einzelnen Prozesses hosten kann. Dies würde bedeuten, dass die Werte des letzten Writers der erweiterten Eigenschaften für alle aktiven Instanzen angezeigt werden. Dies ist ein Grund dafür, dass die _logger-Variable eine private Membervariable der Dienstklasse ist. Dies bedeutet auch, dass „_logger“ für gemeinsam genutzten Code verfügbar gemacht werden muss, der dienstübergreifend verwendet werden kann. 

### <a name="which-one-should-i-use"></a>Was sollte ich verwenden?

Wenn für Ihre Anwendung Leistung sehr wichtig ist, sollten Sie EventSource verwenden, da damit **im Allgemeinen** weniger Ressourcen beansprucht werden und eine bessere Leistung als mit der ASP.NET Core-Protokollierung oder einer Lösung eines Drittanbieters möglich ist.  Dies ist für zahlreiche Dienste kein Problem, aber wenn Ihr Dienst leistungsorientiert ist, kann EventSource die bessere Wahl sein. Um die gleichen Vorteile der strukturierten Protokollierung zu erhalten, erfordert EventSource einen großen Aufwand der Entwickler. Um zu entscheiden, was Sie für das Projekt auswählen sollten, können Sie einen schnellen Prototyp der Verfahren für die einzelnen Ansätze durchführen. Wählen Sie dann den Ansatz aus, der am besten zu Ihren Anforderungen passt.

## <a name="event-and-log-collection"></a>Ereignis- und Protokollsammlung

### <a name="azure-diagnostics"></a>Azure-Diagnose

Zusätzlich zu den Funktionen der Azure-Überwachung bietet Azure auch eine Möglichkeit zum Sammeln von Ereignissen aus den einzelnen Dienste an einem zentralen Ort. Es gibt zwei Artikeln, die das Konfigurieren der Ereignissammlung für [Windows](service-fabric-diagnostics-how-to-setup-wad.md) und [Linux](service-fabric-diagnostics-how-to-setup-lad.md) veranschaulichen. Diese Artikel zeigen das Sammeln von Ereignisdaten und das Senden der Daten an Azure Storage. Dies kann problemlos im Portal oder mit einer Resource Manager-Vorlage durch Aktivieren der Diagnose erfolgen. Durch das Aktivieren dieser Funktion werden einige Ereignisquellen gesammelt, die von Service Fabric automatisch erstellt werden:

- EventSource-Ereignisse und -Leistungsindikatoren, wenn das Reliable Actors-Programmiermodell verwendet wird. Die Ereignisse werden im Artikel zu [Diagnose und Leistungsüberwachung für Reliable Actors](service-fabric-reliable-actors-diagnostics.md) aufgeführt.
- EventSource-Ereignisse, wenn das Reliable Services-Programmiermodell verwendet wird. Die Ereignisse werden im Artikel zu [Diagnosefunktionen für zustandsbehaftete Reliable Services](service-fabric-reliable-services-diagnostics.md) aufgeführt.
- Systemereignisse werden als ETW-Ereignisse ausgegeben. Es gibt viele Ereignisse, die von Service Fabric als Teil dieser Kategorie ausgegeben werden. Dazu gehören Dienstplatzierungs-, Start- und Stoppereignisse. Die beste Möglichkeit, die ausgegebenen Ereignisse anzuzeigen, ist die Verwendung des [Diagnose-Viewers von Visual Studio](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md), wenn die Ausführung auf dem lokalen Computer erfolgt. Da diese Ereignisse native ETW-Ereignisse sind, bestehen einige Einschränkungen für deren Sammlung.
- Ab Version 5.4 von Service Fabric können Integritäts- und Auslastungsmetrikereignisse verfügbar gemacht werden. Dadurch kann die Sammlung dieser Ereignisse für Verlaufsberichte und Warnungen verwendet werden. Diese Ereignisse sind ebenfalls native ETW-Ereignisse und weisen einige Einschränkungen für ihre Sammlung auf.

Bei entsprechender Konfiguration werden diese Ereignisse in einem der Azure Storage-Konten angezeigt, die beim Erstellen des Clusters erstellt wurden, vorausgesetzt, die Diagnose wurde aktiviert. Die Tabellen heißen *WADServiceFabricReliableActorEventTable*, *WADServiceFabricReliableServiceEventTable* und *WADServiceFabricSystemEventTable*. Die Integritätsereignisse werden nicht standardmäßig hinzugefügt und erfordern, dass die Resource Manager-Vorlage geändert wird. Details finden Sie unter [Sammeln von Protokollen mit der Azure-Diagnose](service-fabric-diagnostics-how-to-setup-wad.md).

Diese Artikel zeigen auch, wie benutzerdefinierte Ereignisse in Azure Storage eingebunden werden. Alle vorhandenen Artikel zur Azure-Diagnose, die sich mit dem Konfigurieren von Leistungsindikatoren oder anderen Überwachungsinformationen von einem virtuellen Computer für die Azure-Diagnose beschäftigen, funktionieren auch für ein Service Fabric-Cluster. Wenn z.B. Azure Table Storage als Ziel nicht Ihren Vorstellungen entspricht, gibt es einen Artikel zum [Streamen von Azure-Diagnosedaten im heißen Pfad mithilfe von Event Hubs](../event-hubs/event-hubs-streaming-azure-diags-data.md). Sobald sich die Ereignisse in Event Hub befinden, können sie gelesen und an den Speicherort Ihrer Wahl gesendet werden. Es gibt auch einen Artikel zur Integration von [Azure Diagnoseinformationen in Application Insights](https://azure.microsoft.com/blog/azure-diagnostics-integration-with-application-insights/).

Einer der Nachteile der Verwendung der Azure-Diagnose besteht darin, dass die Konfiguration mithilfe einer Resource Manager-Vorlage erfolgt und daher nur auf VMSS-Ebene vorgenommen werden kann. Eine VMSS entspricht einem Knotentyp in Service Fabric. Dies bedeutet, dass Sie jeden Knotentyp für alle Anwendungen und Dienste konfigurieren müssen, die auf einem Knoten dieses Typs ausgeführt werden können. Dies ist möglicherweise eine große Anzahl von EventSources, abhängig von der Anzahl der konfigurierten Anwendungen und Dienste. Darüber hinaus muss eine Resource Manager-Bereitstellung immer durchgeführt werden, wenn die Konfiguration einer Anwendung geändert wurde. Im Idealfall wird die Überwachungskonfiguration zusammen mit der Dienstkonfiguration übertragen.

Die Azure-Diagnose funktioniert nur für Service Fabric-Cluster, die in Azure bereitgestellt wurden, aber Windows- und Linux-Cluster können verwendet werden.

### <a name="eventflow"></a>EventFlow

[EventFlow wurde vom Visual Studio-Team veröffentlicht](https://github.com/Azure/diagnostics-eventflow) und bietet einen Mechanismus zum Weiterleiten von Ereignissen von einem Knoten zu einem oder mehreren Überwachungszielen. Da EventFlow als NuGet-Paket in Ihrem Dienstprojekt enthalten ist, werden der Code und die Konfiguration für EventFlow mit dem Dienst übertragen, sodass das für die Azure-Diagnose erwähnte Problem mit der Konfiguration pro Knoten nicht besteht. EventFlow wird im Dienstprozess ausgeführt und direkt mit den konfigurierten Ausgaben verbunden. Aufgrund dieser direkten Verbindung funktioniert EventFlow für Azure-, Container- oder lokale Bereitstellungen eines Diensts. Wenn viele Replikate auf demselben Knoten ausgeführt werden, müssen Sie umsichtig vorgehen, da jede EventFlow-Pipeline eine externe Verbindung herstellt. Wenn Sie viele Prozesse hosten, verfügen Sie am Ende über sehr viele ausgehende Verbindungen! Dies ist für Service Fabric-Anwendungen kein großes Problem, da alle ServiceType-Replikate innerhalb desselben Prozesses ausgeführt werden, sodass die Anzahl der ausgehenden Verbindungen beschränkt wird. EventFlow ermöglicht auch das Filtern von Ereignissen, sodass nur die Ereignisse gesendet werden, die dem angegebenen Filter entsprechen. Ausführliche Informationen zur Verwendung von EventFlow mit Service Fabric finden Sie unter [Sammeln von Protokollen direkt aus einem Azure Service Fabric-Dienstprozess](service-fabric-diagnostic-collect-logs-without-an-agent.md).

> [!NOTE]
> In einer zukünftigen Version von Service Fabric wird eine EventSource-Hostanwendung zur Verfügung gestellt, die das Lauschen auf ETW-basierte Eingaben, die Sammlung von Metriken auf Knotenebene und Unterstützung für rollierende Protokolldateien ermöglicht.

Die Verwendung von EventFlow ist ziemlich einfach.
1. Fügen Sie Ihrem Dienstprojekt das NuGet-Paket hinzu.
2. Erstellen Sie innerhalb der **Main**-Funktion des Diensts die EventFlow-Pipeline, und konfigurieren Sie die Ausgaben. In diesem Fall wird die Verwendung von SeriLog als Ausgabe veranschaulicht.
    ```csharp

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

                    // The ServiceManifest.XML file defines one or more service type names.
                    // Registering a service maps a service type name to a .NET type.
                    // When Service Fabric creates an instance of this service type,
                    // an instance of the class is created in this host process.

                    ServiceRuntime.RegisterServiceAsync("StatelessType", context => new Stateless(context, Log.Logger)).GetAwaiter().GetResult();
                    ServiceEventSource.Current.ServiceTypeRegistered(Process.GetCurrentProcess().Id, typeof(Stateless).Name);
                    
                    // Prevents this host process from terminating so services keep running.
                    Thread.Sleep(Timeout.Infinite);
                }
            }
            catch (Exception e)
            {
                ServiceEventSource.Current.ServiceHostInitializationFailed(e.ToString());
                throw;
            }
        }

    ```
3. Erstellen Sie eine Datei im Konfigurationsordner „PackageRoot“ des Diensts mit dem Namen *eventFlowConfig.json*. In der Datei sieht die Konfiguration wie folgt aus:
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
In der Konfiguration sind zwei Eingaben definiert: die zwei EventSource-basierten Quellen, die von Service Fabric erstellt wurden, und die EventSource für den Dienst. Beachten Sie, dass die System- und Integritätsereignisse, die ETW nutzen, für EventFlow nicht verfügbar sind. Die Ursache ist, dass erhöhte Rechte erforderlich sind, um auf eine ETW-Quelle zu lauschen, und Dienste sollten nie mit erhöhten Rechten ausgeführt. Die andere Eingabe ist SeriLog. Die Konfiguration erfolgt in der **Main**-Methode.  Es werden auch einige Filter angewendet. Der erste weist EventFlow an, alle Ereignisse zu löschen, deren Ereignisebene „Verbose“ ist. Auf die andere Filterdefinition kommen wir gleich zurück. Es sind auch zwei Ausgaben konfiguriert: die Standardausgabe, die in das Ausgabefenster in Visual Studio geschrieben wird. Die andere Ausgabe ist ApplicationInsights. Achten Sie darauf, den Instrumentierungsschlüssels hinzuzufügen.

4. Der letzte Schritt besteht darin, den Code zu instrumentieren. In diesem Beispiel instrumentieren wir RunAsync beispielhaft auf unterschiedliche Arten. Im folgenden Code verwenden wir weiterhin SeriLog, und ein Teil der verwendeten Syntax ist spezifisch für SeriLog. Achten Sie auf die besonderen Funktionen für die Protokollierungslösung, die Sie auswählen. Es werden drei Ereignisse generiert: ein Debugereignis und zwei Informationsereignisse. Das zweite verfolgt die Anforderungsdauer. Mit der obigen EventFlow-Konfiguration sollte das Debugereignis nicht an die Ausgabe übergeben werden.

    ```csharp
        Stopwatch sw = Stopwatch.StartNew();

        while (true)
        {
            cancellationToken.ThrowIfCancellationRequested();

            sw.Restart();

            // Delay a random interval to provide a more interesting request duration.
            await Task.Delay(TimeSpan.FromMilliseconds(DateTime.Now.Millisecond), cancellationToken);

            ServiceEventSource.Current.ServiceMessage(this.Context, "Working-{0}", ++iterations);
            _logger.LogDebug("Debug level event from Microsoft.Logging");
            _logger.LogInformation("Informational level event from Microsoft.Logging");
            _logger.LogInformation("{RequestName} {Duration}", "MyRequest", sw.ElapsedMilliseconds);
        }
    ```

Öffnen Sie das Azure-Portal, und navigieren Sie zu Ihrer ApplicationInsights-Ressource, um die Ereignisse in Application Insights anzuzeigen. Klicken Sie dann oben links auf „Suchen“, und die Ereignisse sollten angezeigt werden.

![Anzeigen von Ereignissen in der Application Insights-Suche](./media/service-fabric-diagnostics-overview/ai-search-events.png)

Die Ablaufverfolgungen befinden sich unten in der Abbildung. Sie sehen, dass es nur zwei Ereignisse gibt. Das Debugereignis wurde von EventFlow gelöscht. Und was ist der Anforderungseintrag über der Ablaufverfolgung? Es ist die dritte _logger-Instrumentierungszeile, die anzeigt, dass das Ereignis in Application Insights in eine Anforderungsmetrik übersetzt wurde. Kehren wir jetzt zur Filterdefinition mit dem Typ „metadata“ zurück. Damit wird deklariert, dass ein Ereignis die Eigenschaft „RequestName“ mit dem Wert „MyRequest“ aufweist und eine andere Eigenschaft, „Duration“, die Dauer der Anforderung in Millisekunden enthält. Dies wird im Anforderungsereignis in Application Insights angezeigt. Der gleiche Ansatz funktioniert für alle unterstützten EventFlow-Eingaben, einschließlich EventSource.

Wenn der Cluster ein eigenständiger Cluster ist, der aufgrund von Richtlinien nicht mit einer cloudbasierten Lösung verbunden werden kann, unterstützt EventFlow eine elastische Suche als Ausgabe. Es können aber andere Ausgaben geschrieben werden, und Pull Requests werden empfohlen. Einige Drittanbieter für die ASP.NET Core-Protokollierung bieten auch Lösungen, die lokale Installationen unterstützen.

## <a name="azure-service-fabric-health-and-load-reporting"></a>Integritäts- und Auslastungsberichte für Azure Service Fabric

Service Fabric weist ein eigenes Integritätsmodell auf, das in einigen Artikeln detailliert beschrieben wird:
- [Einführung in die Service Fabric-Integritätsüberwachung](service-fabric-health-introduction.md)
- [Melden und Überprüfen der Dienstintegrität](service-fabric-diagnostics-how-to-report-and-check-service-health.md)
- [Hinzufügen von benutzerdefinierten Service Fabric-Integritätsberichten](service-fabric-report-health.md)
- [Anzeigen von Service Fabric-Integritätsberichten](service-fabric-view-entities-aggregated-health.md)

Die Systemüberwachung ist entscheidend für verschiedene Aspekte beim Betrieb eines Diensts. Dies gilt vor allem, wenn Service Fabric ein Upgrade für eine bestimmte Anwendung ausführt. Nachdem eine Upgradedomäne des Diensts aktualisiert und Ihren Kunden verfügbar gemacht wurde, muss eine Systemdiagnose durchgeführt werden, bevor mit der nächsten Upgradedomäne fortgefahren wird. Wenn keine Integrität erreicht werden kann, wird die Bereitstellung zurückgesetzt, und die Anwendung hat wieder einen bekannten guten Zustand. Während dies Auswirkungen auf einige Kunden haben kann, bevor die Dienste zurückgesetzt werden konnten, tritt bei den meisten Kunden kein Problem auf. Darüber hinaus erfolgt die Auflösung relativ schnell, ohne dass auf eine Aktion von einem menschlichen Operator gewartet werden muss. Je mehr Integritätsprüfungen in den Code eingebunden sind, desto widerstandsfähiger ist der Dienst gegenüber Problemen bei der Bereitstellung.

Ein weiterer Aspekt der Dienstintegrität sind Berichte zu Metriken des Diensts. Metriken sind wichtig in Service Fabric, da sie zum Ausgleichen der Ressourcenverwendung genutzt werden und als Indikator der Systemintegrität eingesetzt werden können. Nehmen wir an, die Anwendung enthält zahlreiche Dienste, und jede Instanz meldet eine Metrik für die Anforderungen pro Sekunde (Requests Per Second, RPS). Wenn einer der Dienste mehr Ressourcen als ein anderer Dienst nutzt, verschiebt Service Fabric Dienstinstanzen im Cluster, um eine gleichmäßige Ressourcenverwendung zu erreichen. Der Artikel [Verwalten von Ressourcenverbrauch und Auslastung in Service Fabric mit Metriken](service-fabric-cluster-resource-manager-metrics.md) enthält eine detaillierte Erläuterung der Funktionsweise.

Metriken gewähren auch Einblicke in die Leistung Ihres Diensts und können über einen Zeitraum verwendet werden, um zu überprüfen, ob der Dienst innerhalb der erwarteten Parameter ausgeführt wird. Wenn z.B. basierend auf Trends um 9:00 Uhr am Montagmorgen der RPS-Durchschnitt 1000 beträgt, könnten Sie einen Integritätsbericht einrichten, der Warnungen ausgibt, wenn der RPS-Wert unter 500 oder über 1500 liegt. Möglicherweise ist alles in Ordnung, aber eine Überprüfung könnte nicht schaden, um sicherzustellen, dass die Benutzererfahrung für Ihre Kunden optimal ist. Ihr Dienst kann einen Satz von Metriken definieren, die für die Integrität gemeldet werden können, die sich jedoch nicht auf den Lastenausgleich von Ressourcen des Clusters auswirken. Legen Sie die Metrikgewichtung auf null fest. Es wird empfohlen, bei allen Metriken mit einer Gewichtung von null zu beginnen und die Gewichtung erst zu erhöhen, wenn Sie sicher sind, dass Sie die Auswirkungen auf den Lastenausgleich von Ressourcen für den Cluster verstehen.

> [!TIP]
> Sie sollten nicht zu viele gewichtete Metriken nutzen. Es kann schwierig sein zu verstehen, warum Dienstinstanzen verschoben werden. Wenige Metriken bieten enorme Verwendungsmöglichkeiten!

Kandidaten für Metriken und Integritätsberichte sind alle Elemente, die Hinweise auf die Integrität und die Leistung der Anwendung liefern können. Ein CPU-Leistungsindikator kann darauf hinweisen, wie ausgelastet der Knoten ist, aber er informiert nicht unbedingt darüber, ob ein bestimmter Dienst fehlerfrei ist, da mehrere Dienste auf einem einzelnen Knoten ausgeführt werden können. Dagegen kann eine Metrik wie RPS oder verarbeitete Elemente oder die Latenz von Anforderungen die Integrität eines bestimmten Diensts angeben.

Fügen Sie für Integritätsberichte Code wie den folgenden hinzu:
```csharp
        if (!result.HasValue)
        {
            HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
            this.Partition.ReportInstanceHealth(healthInformation);
        }
```

Fügen Sie für Metrikberichte Code wie den folgenden zum Dienst hinzu:
```csharp
        this.ServicePartition.ReportLoad(new List<LoadMetric> { new LoadMetric("MemoryInMb", 1234), new LoadMetric("metric1", 42) });
```

## <a name="watchdogs"></a>Watchdogs

Ein Watchdog ist ein separater Dienst, der die Integrität und die Auslastung von Diensten überwachen und die Integrität für alle Elemente in der Hierarchie des Integritätsmodells melden kann. So können Fehler verhindert werden, die nicht erkannt werden, wenn nur ein einzelner Dienst betrachtet wird. Watchdogs sind auch ein guter Platz zum Hosten von Code, der Aktionen zur Problembehebung bei bekannten Bedingungen ohne menschliche Interaktion ausführen kann.

## <a name="visualization-analysis-and-alerting"></a>Visualisierung, Analyse und Warnungen

Zum letzten Teil der Überwachung gehören die Visualisierung des Ereignisdatenstroms, Berichte zur Leistung des Diensts und Warnungen bei Problemen. Es gibt zahlreiche mögliche Lösungen, die für diesen Aspekt der Überwachung verwendet werden. Application Insights und OMS können genutzt werden, um Warnungen basierend auf dem Ereignisdatenstrom zu erzeugen. Power BI oder eine Lösung eines Drittanbieters, wie z.B. [Kibana](https://www.elastic.co/products/kibana) oder [Splunk](https://www.splunk.com/), können für die visuelle Darstellung der Daten verwendet werden können.

## <a name="next-steps"></a>Nächste Schritte

* [Sammeln von Protokollen mit Azure-Diagnose](service-fabric-diagnostics-how-to-setup-wad.md)
* [Sammeln von Protokollen direkt aus einem Azure Service Fabric-Dienstprozess](service-fabric-diagnostic-collect-logs-without-an-agent.md)
*  [Verwalten von Ressourcenverbrauch und Auslastung in Service Fabric mit Metriken](service-fabric-cluster-resource-manager-metrics.md)




<!--HONumber=Feb17_HO3-->


