<properties 
	pageTitle="Application Insights für Windows-Dienste" 
	description="Analysieren Sie die Nutzung und Leistung von Windows-Hintergrunddiensten mit Application Insights." 
	services="application-insights" 
    documentationCenter="windows"
	authors="alancameronwills" 
	manager="douge"/>

<tags 
	ms.service="application-insights" 
	ms.workload="tbd" 
	ms.tgt_pltfrm="ibiza" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="04/27/2016" 
	ms.author="awills"/>

# Application Insights für Windows-Hintergrunddienste

*Application Insights befindet sich in der Vorschau.*

Mit [Visual Studio Application Insights](app-insights-get-started.md) können Sie Ihre bereitgestellte Anwendung auf Nutzung und Leistung überwachen.

Alle Windows-Anwendungen, einschließlich Hintergrunddiensten und Workerrollen, können das Application Insights-SDK zum Senden von Telemetriedaten an Application Insights verwenden. Sie können das SDK auch einem Klassenbibliotheksprojekt hinzufügen.

Sie können die gewünschten Standarddatensammler auswählen (z. B. zum Überwachen von Leistungsindikatoren oder Abhängigkeitsaufrufen) oder einfach die Kern-API verwenden und eigene Telemetriedaten schreiben.

Überprüfen Sie zunächst, ob Sie mit einem anderen Typ von Windows-Anwendung arbeiten:

* Web-Apps: Wechseln Sie zu [ASP.NET 4](app-insights-asp-net.md), [ASP.NET 5](app-insights-asp-net-five.md).
* [Azure Cloud Services](app-insights-cloudservices.md)
* Desktop-Apps: Wir empfehlen [HockeyApp](https://hockeyapp.net). Mit HockeyApp können Sie die Verteilung, das Durchführen von Livetests und das Feedback von Benutzern verwalten. Außerdem können Sie mit HockeyApp auch Nutzungs-und Absturzberichte überwachen. Sie können auch [aus einer Desktop-App Telemetriedaten an Application Insights senden](app-insights-windows-desktop.md). 


## <a name="add"></a> Erstellen einer Application Insights-Ressource


1.  Erstellen Sie im [Azure-Portal][portal] eine neue Application Insights-Ressource. Wählen Sie als Anwendungstyp "ASP.NET-App" aus. 

    ![Klicken Sie auf "Neu > Application Insights"](./media/app-insights-windows-services/01-new.png)


2.  Erstellen Sie eine Kopie des Instrumentatierungsschlüssels. Diesen finden Sie in der Dropdownliste „Essentials“ der neuen Ressource, die Sie gerade erstellt haben. Schließen Sie die Anwendungszuordnung, oder führen Sie einen Bildlauf nach links zum Blatt „Übersicht“ für die Ressource aus.

    ![Klicken Sie auf „Eigenschaften“, wählen Sie den Schlüssel aus, und drücken Sie STRG+C.](./media/app-insights-windows-services/10.png)

## <a name="sdk"></a>Installieren des SDK in Ihrer Anwendung


1. Bearbeiten Sie die NuGet-Pakete Ihres Windows-Anwendungsprojekts in Visual Studio.

    ![Klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie "NuGet-Pakete verwalten".](./media/app-insights-windows-services/03-nuget.png)

2. Installieren Sie das Application Insights Windows Server-Paket: Microsoft.ApplicationInsights.WindowsServer

    ![Suchen Sie nach "Application Insights".](./media/app-insights-windows-services/04-ai-nuget.png)

    *Kann ich andere Pakete verwenden?*

    Ja. Wählen Sie die Kern-API (Microsoft.ApplicationInsights), wenn Sie die API nur verwenden möchten, um Ihre eigenen Telemetriedaten zu senden. Das Windows Server-Paket beinhaltet automatisch die Kern-API sowie einige weitere Pakete wie etwa die Sammlung von Leistungsindikatoren und die Abhängigkeitsüberwachung.


3. Legen Sie Ihren "InstrumentationKey" fest.

    * Wenn Sie nur das Core-API-Paket "Microsoft.ApplicationInsights" installiert haben, müssen Sie den Schlüssel im Code festlegen, beispielsweise in "main()": 

    `TelemetryConfiguration.Active.InstrumentationKey = "` *Ihr Schlüssel* `";`

    Wenn Sie eines der anderen Pakete installiert haben, können Sie den Schlüssel entweder im Code oder in "ApplicationInsights.config" festlegen:
 
    `<InstrumentationKey>`*Ihr Schlüssel*`</InstrumentationKey>`

    Wenn Sie die Datei „ApplicationInsights.config“ verwenden, stellen Sie sicher, dass ihre Eigenschaften im Projektmappen-Explorer auf **Buildvorgang = Inhalt, In Ausgabeverzeichnis kopieren = Kopieren** festgelegt sind.

## <a name="telemetry"></a>Einfügen von Telemetrieaufrufen

Verwenden Sie eine der [Application Insights-APIs][api], um Telemetriedaten zu senden. Wenn Sie die Kern-API verwenden, werden Telemetriedaten nicht automatisch gesendet. In der Regel würden Sie Folgendes verwenden:

* `TrackPageView(pageName)` für wechselnde Formulare, Seiten oder Registerkarten
* `TrackEvent(eventName)` für andere Benutzeraktionen
* `TrackMetric(name, value)` in einer Hintergrundaufgabe, um reguläre Berichte über Metriken zu senden, die nicht an bestimmte Ereignisse gekoppelt sind.
* `TrackTrace(logEvent)` für die [Diagnoseprotokollierung][diagnostic]
* `TrackException(exception)` in Catch-Klauseln
* Um sicherzustellen, dass alle Telemetriedaten vor dem Schließen der App gesendet werden, verwenden Sie `Flush()`. Verwenden Sie diese nur, wenn Sie lediglich die Core-API verwenden (Microsoft.ApplicationInsights). Die Web-SDKs implementieren dieses Verhalten automatisch. (Wenn Ihre Apps in einem Kontext ausgeführt werden, in dem nicht immer eine Internetverbindung verfügbar ist, siehe auch [Persistenzkanal](#persistence-channel).)


#### Telemetrieinitialisierer

Um die Anzahl von Benutzern und Sitzungen anzuzeigen, können Sie die Werte für jede `TelemetryClient`-Instanz festlegen. Alternativ dazu können Sie einen Telemetrieinitialisierer verwenden, um dies für alle Clients hinzuzufügen:

```C#

    class UserSessionInitializer : ITelemetryInitializer
    {
        public void Initialize(ITelemetry telemetry)
        {
            telemetry.Context.User.Id = Environment.UserName;
            telemetry.Context.Session.Id = Guid.NewGuid().ToString();
        }
        
    }

    static class Program
    {
        ...
        static void Main()
        {
            TelemetryConfiguration.Active.TelemetryInitializers.Add(
                new UserSessionInitializer());
            ...

```



## <a name="run"></a>Ausführen des Projekts

[Führen Sie die Anwendung durch Drücken von F5 aus](http://msdn.microsoft.com/library/windows/apps/bg161304.aspx), und verwenden Sie sie, um einige Telemetriedaten zu generieren.

In Visual Studio sehen Sie eine Anzahl der gesendeten Ereignisse.

![](./media/app-insights-windows-services/appinsights-09eventcount.png)

Ereignisse werden auch in den Diagnose- und Ausgabefenstern angezeigt.

## <a name="monitor"></a>Anzeigen der Überwachungsdaten

Kehren Sie zum Blatt Ihrer Anwendung im Azure-Portal zurück.

Im [Live Metrics Stream](app-insights-metrics-explorer.md#live-metrics-stream) werden die ersten Ereignisse angezeigt.


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


## <a name="usage"></a>Nächste Schritte

[Nachverfolgen der Nutzung Ihrer App][knowUsers]

[Diagnoseprotokolle anlegen und durchsuchen][diagnostic]

[Problembehandlung][qna]




<!--Link references-->

[diagnostic]: app-insights-diagnostic-search.md
[metrics]: app-insights-metrics-explorer.md
[portal]: http://portal.azure.com/
[qna]: app-insights-troubleshoot-faq.md
[knowUsers]: app-insights-overview-usage.md
[api]: app-insights-api-custom-events-metrics.md
[CoreNuGet]: https://www.nuget.org/packages/Microsoft.ApplicationInsights
 

<!---HONumber=AcomDC_0615_2016-->