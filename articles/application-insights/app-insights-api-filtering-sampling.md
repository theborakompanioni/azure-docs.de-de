<properties 
	pageTitle="Filterung und Vorverarbeitung im Application Insights-SDK | Microsoft Azure" 
	description="Schreiben Sie Telemetrieprozessoren und Telemetrieinitialisierer für das SDK, um die Daten zu filtern oder ihnen Eigenschaften hinzuzufügen, bevor die Telemetriedaten an das Application Insights-Portal gesendet werden." 
	services="application-insights"
    documentationCenter="" 
	authors="beckylino" 
	manager="douge"/>
 
<tags 
	ms.service="application-insights" 
	ms.workload="tbd" 
	ms.tgt_pltfrm="ibiza" 
	ms.devlang="multiple" 
	ms.topic="article" 
	ms.date="08/30/2016" 
	ms.author="borooji"/>

# Filterung und Vorverarbeitung von Telemetriedaten im Application Insights-SDK

*Application Insights befindet sich in der Vorschau.*

Sie können Plug-Ins für das Application Insights SDK schreiben und konfigurieren, um anzupassen, wie Telemetriedaten erfasst und verarbeitet werden, bevor sie an den Application Insights-Dienst gesendet werden.

Derzeit sind diese Features für das ASP.NET-SDK verfügbar.

* Durch das [Erstellen von Stichproben](app-insights-sampling.md) verringert sich der Umfang der Telemetriedaten, ohne Statistiken zu verfälschen. Zusammengehörige Datenpunkte werden dabei zusammengehalten, sodass Sie bei der Diagnose von Problemen zwischen diesen navigieren können. Im Portal wird die Gesamtanzahl multipliziert, um eine Kompensation der Stichproben zu erreichen.
* Durch das [Filtern mit Telemetrieprozessoren](#filtering) können Sie Telemetriedaten im SDK auswählen oder ändern, bevor sie an den Server gesendet werden. Sie können beispielsweise den Umfang der Telemetriedaten verringern, indem Sie Anforderungen von Robots ausschließen. Filtern stellt jedoch ein grundlegenderes Verfahren zur Senkung des Datenverkehrs dar als das Erstellen von Stichproben. Sie können so besser steuern, was übertragen wird. Jedoch müssen Sie beachten, dass dies Auswirkungen auf die Statistik hat – wenn Sie z. B. alle erfolgreichen Anforderungen herausfiltern.
* [Telemetrieinitialisierer fügen Eigenschaften](#add-properties) zu beliebigen von der App gesendeten Telemetriedaten hinzu, einschließlich Telemetriedaten von Standardmodulen. Sie könnten z. B. berechnete Werte hinzufügen oder Versionsnummern, nach denen Sie die Daten im Portal filtern.
* Die [SDK-API](app-insights-api-custom-events-metrics.md) wird zum Senden benutzerdefinierter Ereignisse und Metriken verwendet.


Vorbereitung:

* Installieren Sie das [Application Insights-SDK für ASP.NET v2](app-insights-asp-net.md) in Ihrer App.


<a name="filtering"></a>
## Filtern: ITelemetryProcessor

Diese Technik bietet Ihnen eine bessere Kontrolle über die Daten aus dem Telemetriedatenstrom, die ein- oder ausgeschlossen werden. Sie können sie zusammen mit der Stichprobenerstellung oder einzeln verwenden.

Zum Filtern von Telemetriedaten schreiben Sie einen Telemetrie-Prozessor und registrieren ihn beim SDK. Alle Telemetriedaten durchlaufen den Prozessor. Sie können dann auswählen, diese aus dem Datenstrom zu verwerfen, oder Eigenschaften hinzufügen. Dies schließt Telemetriedaten aus Standardmodulen wie etwa dem HTTP-Anforderungssammler und der Abhängigkeitserfassung sowie von Ihnen erstellte Telemetriedaten ein. Sie können z. B. Telemetriedaten zu Anforderungen von Robots oder erfolgreiche Abhängigkeitsaufrufe herausfiltern.

> [AZURE.WARNING] Die Filterung der vom SDK gesendeten Telemetriedaten mithilfe von Prozessoren kann die im Portal angezeigten Statistiken verfälschen und die Nachverfolgung verwandter Elemente erschweren.
> 
> Verwenden Sie stattdessen [Sampling](app-insights-sampling.md).

### Erstellen eines Telemetrieprozessors

1. Vergewissern Sie sich, dass in Ihrem Projekt das Application Insights-SDK in Version 2.0.0 oder höher verwendet wird. Klicken Sie im Visual Studio-Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und wählen Sie „NuGet-Pakete verwalten“ aus. Aktivieren Sie im NuGet-Paket-Manager „Microsoft.ApplicationInsights.Web“.

1. Implementieren Sie zum Erstellen eines Filters „ITelemetryProcessor“. Hierbei handelt es sich um einen weiteren Erweiterungspunkt wie Telemetriemodul, Telemetrieinitialisierer und Telemetriekanal.

    Beachten Sie, dass Telemetrieprozessoren eine Verarbeitungskette erstellen. Beim Instanziieren eines Telemetrieprozessors übergeben Sie einen Link an den nächsten Prozessor in der Kette. Wenn ein Telemetriedatenpunkt an die Verarbeitungsmethode übergeben wird, führt er seine Aufgaben aus und ruft dann den nächsten Telemetrieprozessor in der Kette auf.

    ``` C#

    using Microsoft.ApplicationInsights.Channel;
    using Microsoft.ApplicationInsights.Extensibility;

    public class SuccessfulDependencyFilter : ITelemetryProcessor
      {
        private ITelemetryProcessor Next { get; set; }

        // You can pass values from .config
        public string MyParamFromConfigFile { get; set; }

        // Link processors to each other in a chain.
        public SuccessfulDependencyFilter(ITelemetryProcessor next)
        {
            this.Next = next;
        }
        public void Process(ITelemetry item)
        {
            // To filter out an item, just return 
            if (!OKtoSend(item)) { return; }
            // Modify the item if required 
            ModifyItem(item);

            this.Next.Process(item);
        }

        // Example: replace with your own criteria.
        private bool OKtoSend (ITelemetry item)
        {
            var dependency = item as DependencyTelemetry;
            if (dependency == null) return true;

            return dependency.Success != true;
        }

        // Example: replace with your own modifiers.
        private void ModifyItem (ITelemetry item)
        {
            item.Context.Properties.Add("app-version", "1." + MyParamFromConfigFile);
        }
    }
    

    ```
2. Fügen Sie Folgendes in „ApplicationInsights.config“ ein:

```XML

    <TelemetryProcessors>
      <Add Type="WebApplication9.SuccessfulDependencyFilter, WebApplication9">
         <!-- Set public property -->
         <MyParamFromConfigFile>2-beta</MyParamFromConfigFile>
      </Add>
    </TelemetryProcessors>

```

(Dies ist der gleiche Abschnitt, in dem Sie einen Filter für die Stichprobe initialisieren.).

Sie können durch die Bereitstellung von öffentlich benannten Eigenschaften in Ihrer Klasse Zeichenfolgenwerte aus der .config-Datei übergeben.

> [AZURE.WARNING] Achten Sie darauf, dass die Typnamen und sonstige Eigenschaftennamen in der .config-Datei mit den Klassen- und Eigenschaftennamen im Code übereinstimmen. Wenn die .config-Datei auf einen nicht vorhandenen Typ oder eine nicht vorhandene Eigenschaft verweist, könnte das SDK möglicherweise automatisch keine Telemetriedaten mehr senden.

 
**Alternativ** können Sie den Filter im Code initialisieren. Fügen Sie in einer geeigneten Initialisierungsklasse, z. B. AppStart in „Global.asax.cs“ Ihren Prozessor in die Kette ein:

```C#

    var builder = TelemetryConfiguration.Active.TelemetryProcessorChainBuilder;
    builder.Use((next) => new SuccessfulDependencyFilter(next));

    // If you have more processors:
    builder.Use((next) => new AnotherProcessor(next));

    builder.Build();

```

Nach diesem Punkt erstellte TelemetryClients-Elemente verwenden Ihre Prozessoren.

### Beispielfilter

#### Synthetische Anforderungen

Filtern Sie Bots und Webtests heraus. Mit dem Metrik-Explorer haben Sie die Option, synthetische Quellen herauszufiltern. Dadurch wird der Datenverkehr verringert, da die Quellen im SDK gefiltert werden.

``` C#

    public void Process(ITelemetry item)
    {
      if (!string.IsNullOrEmpty(item.Context.Operation.SyntheticSource)) {return;}

      // Send everything else: 
      this.Next.Process(item);
    }

```

#### Fehler bei der Authentifizierung

Filtern Sie Abfragen mit der Antwort „401“ heraus.

```C#

public void Process(ITelemetry item)
{
    var request = item as RequestTelemetry;

    if (request != null &&
    request.ResponseCode.Equals("401", StringComparison.OrdinalIgnoreCase))
    {
        // To filter out an item, just terminate the chain: 
        return;
    }
    // Send everything else: 
    this.Next.Process(item);
}

```

#### Herausfiltern schneller Remoteabhängigkeitsaufrufe

Wenn Sie langsame Aufrufe diagnostizieren möchten, filtern Sie die schnellen Aufrufe heraus.

> [AZURE.NOTE] Dadurch wird die im Portal angezeigte Statistik verfälscht. Das Abhängigkeitsdiagramm sieht aus, als ob alle Abhängigkeitsaufrufe fehlgeschlagen wären.

``` C#

public void Process(ITelemetry item)
{
    var request = item as DependencyTelemetry;
            
    if (request != null && request.Duration.Milliseconds < 100)
    {
        return;
    }
    this.Next.Process(item);
}

```

#### Diagnostizieren von Abhängigkeitsproblemen

In [diesem Blog](https://azure.microsoft.com/blog/implement-an-application-insights-telemetry-processor/) wird ein Projekt zur Diagnose von Abhängigkeitsproblemen beim automatischen Senden regulärer Pings an Abhängigkeiten beschrieben.


<a name="add-properties"></a>
## Hinzufügen von Eigenschaften: ITelemetryInitializer

Mithilfe von Telemetrieinitialisierern definieren Sie globale Eigenschaften, die mit allen Telemetriedaten gesendet werden, und setzen das ausgewählte Verhalten der Standardtelemetriemodule außer Kraft.

Das Application Insights for Web-Paket erfasst beispielsweise Telemetriedaten über HTTP-Anforderungen. Standardmäßig kennzeichnet es jede Anforderung mit einem Antwortcode >= 400 als fehlerhaft. Wenn 400 jedoch als erfolgreich behandelt werden soll, können Sie einen Telemetrieinitialisierer angeben, der die Success-Eigenschaft festlegt.

Wenn Sie einen Telemetrieinitialisierer angeben, wird dieser immer aufgerufen, wenn eine der Track*()-Methoden aufgerufen wird. Dies umfasst auch Methoden, die von den Standardtelemetriemodulen aufgerufen werden. Nach Abmachung legen diese Module keine Eigenschaft fest, die bereits durch einen Initialisierer festgelegt wurde.

**Definieren des Initialisierers**

*C#*

```C#

    using System;
    using Microsoft.ApplicationInsights.Channel;
    using Microsoft.ApplicationInsights.DataContracts;
    using Microsoft.ApplicationInsights.Extensibility;

    namespace MvcWebRole.Telemetry
    {
      /*
       * Custom TelemetryInitializer that overrides the default SDK 
       * behavior of treating response codes >= 400 as failed requests
       * 
       */
      public class MyTelemetryInitializer : ITelemetryInitializer
      {
        public void Initialize(ITelemetry telemetry)
        {
            var requestTelemetry = telemetry as RequestTelemetry;
            // Is this a TrackRequest() ?
            if (requestTelemetry == null) return;
            int code;
            bool parsed = Int32.TryParse(requestTelemetry.ResponseCode, out code);
            if (!parsed) return;
            if (code >= 400 && code < 500)
            {
                // If we set the Success property, the SDK won't change it:
                requestTelemetry.Success = true;
                // Allow us to filter these requests in the portal:
                requestTelemetry.Context.Properties["Overridden400s"] = "true";
            }
            // else leave the SDK to set the Success property      
        }
      }
    }
```

**Laden des Initialisierers**

In "ApplicationInsights.config":

    <ApplicationInsights>
      <TelemetryInitializers>
        <!-- Fully qualified type name, assembly name: -->
        <Add Type="MvcWebRole.Telemetry.MyTelemetryInitializer, MvcWebRole"/> 
        ...
      </TelemetryInitializers>
    </ApplicationInsights>

*Alternativ* können Sie den Initialisierer im Code instanziieren, z. B. in "Global.aspx.cs":


```C#
    protected void Application_Start()
    {
        // ...
        TelemetryConfiguration.Active.TelemetryInitializers
        .Add(new MyTelemetryInitializer());
    }
```


[Weitere Informationen zu diesem Beispiel anzeigen.](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/MvcWebRole)

<a name="js-initializer"></a>
### JavaScript-Telemetrieinitialisierer

*JavaScript*

Fügen Sie einen Telemetrieinitialisierer unmittelbar nach dem Initialisierungscode ein, den Sie vom Portal abgerufen haben:

```JS

    <script type="text/javascript">
        // ... initialization code
        ...({
            instrumentationKey: "your instrumentation key"
        });
        window.appInsights = appInsights;


        // Adding telemetry initializer.
        // This is called whenever a new telemetry item
        // is created.

        appInsights.queue.push(function () {
            appInsights.context.addTelemetryInitializer(function (envelope) {
                var telemetryItem = envelope.data.baseData;

                // To check the telemetry item’s type - for example PageView:
                if (envelope.name == Microsoft.ApplicationInsights.Telemetry.PageView.envelopeType) {
                    // this statement removes url from all page view documents
                    telemetryItem.url = "URL CENSORED";
                }

                // To set custom properties:
                telemetryItem.properties = telemetryItem.properties || {};
                telemetryItem.properties["globalProperty"] = "boo";

                // To set custom metrics:
                telemetryItem.measurements = telemetryItem.measurements || {};
                telemetryItem.measurements["globalMetric"] = 100;
            });
        });

        // End of inserted code.

        appInsights.trackPageView();
    </script>
```

Eine Übersicht der für „telemetryItem“ verfügbaren nicht benutzerdefinierten Eigenschaften finden Sie im [Datenmodell](app-insights-export-data-model.md#lttelemetrytypegt).

Sie können beliebig viele Initialisierer hinzufügen.


## ITelemetryProcessor und ITelemetryInitializer

Was ist der Unterschied zwischen Telemetrieprozessoren und Telemetrieinitialisierern?

* Die Einsatzmöglichkeiten überlappen sich teilweise: Beide können verwendet werden, um einer Telemetrie Eigenschaften hinzuzufügen.
* Telemetrieinitialisierer werden immer vor Telemetrieprozessoren ausgeführt.
* Mit Telemetrieprozessoren können Sie ein Telemetrieelement vollständig ersetzen oder verwerfen.
* Telemetrieprozessoren verarbeiten keine Telemetrie von Leistungsindikatoren.



## Persistenzkanal 

Wenn Ihre Apps in einem Kontext ausgeführt werden, in dem nicht immer eine Internetverbindung oder nur eine langsame Internetverbindung verfügbar ist, erwägen Sie die Verwendung eines Persistenzkanals anstelle des standardmäßigen In-Memory-Kanals.

Der standardmäßige In-Memory-Kanal verliert alle Telemetriedaten, die nicht vor dem Schließen der App gesendet wurden. Wenngleich Sie mithilfe von `Flush()` versuchen können, alle im Puffer verbleibenden Daten zu senden, gehen dennoch Daten verloren, wenn keine Internetverbindung besteht oder wenn die App heruntergefahren wird, bevor die Übertragung abgeschlossen ist.

Im Gegensatz dazu puffert der Persistenzkanal Telemetriedaten in einer Datei, bevor diese an das Portal gesendet werden. `Flush()` stellt sicher, dass Daten in der Datei gespeichert werden. Wenn beim Schließen der App keine Daten gesendet werden, verbleiben diese in der Datei. Wenn die App erneut gestartet wird, werden die Daten gesendet, sofern eine Internetverbindung besteht. Daten werden so lange in der Datei gesammelt, bis eine Internetverbindung verfügbar ist.

### Verwenden des Persistenzkanals

1. Importieren Sie das NuGet-Paket [Microsoft.ApplicationInsights.PersistenceChannel](https://www.nuget.org/packages/Microsoft.ApplicationInsights.PersistenceChannel/1.2.3).
2. Fügen Sie diesen Code an geeigneter Stelle für die Initialisierung in Ihre App ein:
 
    ```C# 

      using Microsoft.ApplicationInsights.Channel;
      using Microsoft.ApplicationInsights.Extensibility;
      ...

      // Set up 
      TelemetryConfiguration.Active.InstrumentationKey = "YOUR INSTRUMENTATION KEY";
 
      TelemetryConfiguration.Active.TelemetryChannel = new PersistenceChannel();
    
    ``` 
3. Verwenden Sie `telemetryClient.Flush()`, bevor Ihre App geschlossen wird. So stellen Sie sicher, dass die Daten entweder an das Portal gesendet oder in der Datei gespeichert werden.

    Beachten Sie, dass Flush() synchron für den Persistenzkanal, aber asynchron für andere Kanäle ist.

 
Der Persistenzkanal ist für Geräteszenarien optimiert, in denen die Anwendung relativ wenige Ereignisse generiert und die Internetverbindung häufig unzuverlässig ist. Dieser Kanal schreibt Ereignisse zunächst in zuverlässigen Speicher auf den Datenträger zurück und versucht dann, die Daten zu senden.

#### Beispiel

Angenommen, Sie möchten Ausnahmefehler überwachen. Sie abonnieren das `UnhandledException`-Ereignis. Im Rückruf schließen Sie einen Flush-Befehl ein und stellen so sicher, dass die Telemetriedaten dauerhaft gespeichert werden.
 
```C# 

AppDomain.CurrentDomain.UnhandledException += CurrentDomain_UnhandledException; 
 
... 
 
private void CurrentDomain_UnhandledException(object sender, UnhandledExceptionEventArgs e) 
{ 
    ExceptionTelemetry excTelemetry = new ExceptionTelemetry((Exception)e.ExceptionObject); 
    excTelemetry.SeverityLevel = SeverityLevel.Critical; 
    excTelemetry.HandledAt = ExceptionHandledAt.Unhandled; 
 
    telemetryClient.TrackException(excTelemetry); 
 
    telemetryClient.Flush(); 
} 

``` 

Wenn die App geschlossen wird, sehen Sie eine Datei in `%LocalAppData%\Microsoft\ApplicationInsights`, die die komprimierten Ereignisse enthält.
 
Beim nächsten Start der Anwendung prüft der Kanal diese Datei und sendet die Telemetriedaten, sofern möglich, an Application Insights.

#### Testbeispiel

```C#

using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.Extensibility;

namespace ConsoleApplication1
{
    class Program
    {
        static void Main(string[] args)
        {
            // Send data from the last time the app ran
            System.Threading.Thread.Sleep(5 * 1000);

            // Set up persistence channel

            TelemetryConfiguration.Active.InstrumentationKey = "YOUR KEY";
            TelemetryConfiguration.Active.TelemetryChannel = new PersistenceChannel();

            // Send some data

            var telemetry = new TelemetryClient();

            for (var i = 0; i < 100; i++)
            {
                var e1 = new Microsoft.ApplicationInsights.DataContracts.EventTelemetry("persistenceTest");
                e1.Properties["i"] = "" + i;
                telemetry.TrackEvent(e1);
            }

            // Make sure it's persisted before we close
            telemetry.Flush();
        }
    }
}

```


Den Code für den Persistenzkanal finden Sie auf [GitHub](https://github.com/Microsoft/ApplicationInsights-dotnet/tree/v1.2.3/src/TelemetryChannels/PersistenceChannel).


## Referenz

* [API-Übersicht](app-insights-api-custom-events-metrics.md)

* [ASP.NET-Referenz](https://msdn.microsoft.com/library/dn817570.aspx)


## SDK-Code

* [ASP.NET Core SDK](https://github.com/Microsoft/ApplicationInsights-dotnet)
* [ASP.NET 5](https://github.com/Microsoft/ApplicationInsights-aspnet5)
* [JavaScript SDK](https://github.com/Microsoft/ApplicationInsights-JS)


## <a name="next"></a>Nächste Schritte


* [Durchsuchen von Ereignissen und Protokollen][diagnostic]
* [Stichproben](app-insights-sampling.md)
* [Problembehandlung][qna]


<!--Link references-->

[client]: app-insights-javascript.md
[config]: app-insights-configuration-with-applicationinsights-config.md
[create]: app-insights-create-new-resource.md
[data]: app-insights-data-retention-privacy.md
[diagnostic]: app-insights-diagnostic-search.md
[exceptions]: app-insights-asp-net-exceptions.md
[greenbrown]: app-insights-asp-net.md
[java]: app-insights-java-get-started.md
[metrics]: app-insights-metrics-explorer.md
[qna]: app-insights-troubleshoot-faq.md
[trace]: app-insights-search-diagnostic-logs.md
[windows]: app-insights-windows-get-started.md

 

<!---HONumber=AcomDC_0907_2016-->