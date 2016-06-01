<properties 
	pageTitle="Überwachen der Nutzung und Leistung bei Windows-Desktop-Apps" 
	description="Analysieren Sie die Nutzung und Leistung Ihrer Windows-Desktop-Apps mit HockeyApp und Application Insights." 
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

# Überwachen der Nutzung und Leistung von Windows-Desktop-Apps

*Application Insights befindet sich in der Vorschau.*

Mit [Visual Studio Application Insights](app-insights-get-started.md) und [HockeyApp](https://hockeyapp.net) können Sie Ihre bereitgestellte Anwendung auf Nutzung und Leistung überwachen.

> [AZURE.IMPORTANT] Wir empfehlen [HockeyApp](https://hockeyapp.net) zum Verteilen und Überwachen von Apps für Desktops und Geräte. Mit HockeyApp können Sie die Verteilung, das Durchführen von Livetests und das Feedback von Benutzern verwalten. Außerdem können Sie mit HockeyApp auch Nutzungs-und Absturzberichte überwachen.

> Von einer Desktopanwendung können zwar auch Telemetriedaten an Application Insights gesendet werden. Dies ist allerdings in erster Linie für das Debuggen und für experimentelle Zwecke nützlich.


## So senden Sie Telemetriedaten aus einer Windows-Anwendung an Application Insights

1. Erstellen Sie im [Azure-Portal](https://portal.azure.com) eine neue Application Insights-Ressource. Wählen Sie als Anwendungstyp "ASP.NET-App" aus.
2. Erstellen Sie eine Kopie des Instrumentierungsschlüssels. Diesen finden Sie in der Dropdownliste „Essentials“ der neuen Ressource, die Sie gerade erstellt haben. Schließen Sie die Anwendungszuordnung, oder führen Sie einen Bildlauf nach links zum Blatt „Übersicht“ für die Ressource aus.
3. Bearbeiten Sie die NuGet-Pakete Ihres App-Projekts in Visual Studio, und fügen Sie ihnen „Microsoft.ApplicationInsights.WindowsServer“ hinzu. (Oder wählen Sie „Microsoft.ApplicationInsights“ aus, wenn Sie nur die reine API ohne die Standardmodule zur Telemetrieerfassung verwenden möchten.)
4. Legen Sie den Instrumentierungsschlüssel im Code fest:

    `TelemetryConfiguration.Active.InstrumentationKey = "` *Ihr Schlüssel* `";`

    Oder legen Sie ihn in der Datei „ApplicationInsights.config“ fest (wenn Sie eines der Standardtelemetriepakete installiert haben):
 
    `<InstrumentationKey>`*Ihr Schlüssel*`</InstrumentationKey>`

    Wenn Sie die Datei „ApplicationInsights.config“ verwenden, stellen Sie sicher, dass ihre Eigenschaften im Projektmappen-Explorer auf **Buildvorgang = Inhalt, In Ausgabeverzeichnis kopieren = Kopieren** festgelegt sind.
5. [Verwenden Sie die API](app-insights-api-custom-events-metrics.md), um Telemetriedaten zu senden.
6. Führen Sie die App aus, und schauen Sie sich die Telemetriedaten in der Ressource an, die Sie im Azure-Portal erstellt haben.

## <a name="telemetry"></a>Beispielcode

```C#

    public partial class Form1 : Form
    {
        private TelemetryClient tc = new TelemetryClient();
        ...
        private void Form1_Load(object sender, EventArgs e)
        {
            // Alternative to setting ikey in config file:
            tc.InstrumentationKey = "key copied from portal";

            // Set session data:
            tc.Context.User.Id = Environment.UserName;
            tc.Context.Session.Id = Guid.NewGuid().ToString();
            tc.Context.Device.OperatingSystem = Environment.OSVersion.ToString();

            // Log a page view:
            tc.TrackPageView("Form1");
            ...
        }

        protected override void OnClosing(CancelEventArgs e)
        {
            stop = true;
            if (tc != null)
            {
                tc.Flush(); // only for desktop apps

                // Allow time for flushing:
                System.Threading.Thread.Sleep(1000);
            }
            base.OnClosing(e);
        }

```


 

<!---HONumber=AcomDC_0518_2016-->