<properties 
	pageTitle="Trennen von Application Insights-Ressourcen für Entwicklung, Test und Produktion" 
	description="Überwachen der Leistung und Nutzung Ihrer Anwendung in verschiedenen Entwicklungsphasen" 
	services="application-insights" 
    documentationCenter=""
	authors="alancameronwills" 
	manager="douge"/>

<tags 
	ms.service="application-insights" 
	ms.workload="tbd" 
	ms.tgt_pltfrm="ibiza" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="05/04/2016" 
	ms.author="awills"/>

# Trennen von Application Insights-Ressourcen

Sollten Telemetriedaten aus verschiedenen Komponenten und Versionen einer Anwendung an verschiedene Application Insights-Ressourcen gesendet oder in einer einzigen Ressource kombiniert werden? Dieser Artikel beschreibt bewährte Methoden und die notwendigen Techniken für beide Ansätze.

Aber betrachten wir zunächst genauer, worauf die Frage abzielt. Die von Ihrer Anwendung empfangenen Daten werden von Application Insights in einer Microsoft Azure-*Ressource* gespeichert und verarbeitet. Jede Ressource wird durch einen *Instrumentierungsschlüssel* (iKey) identifiziert. In Ihrer App wird der Schlüssel dem Application Insights-SDK bereitgestellt, sodass es die gesammelten Daten an die richtige Ressource senden kann. Der Schlüssel kann im Code oder in "ApplicationInsights.config" bereitgestellt werden. Durch Ändern des Schlüssels im SDK können Sie Daten an andere Ressourcen weiterleiten.

Ein einfacher Fall als Beispiel: Wenn Sie Code für eine neue Anwendung erstellen, erstellen Sie auch eine neue Ressource in Application Insights. In Visual Studio übernimmt der Dialog *Neues Projekt* das für Sie.

Wenn es sich um eine Website mit großem Datenvolumen handelt, wird sie unter Umständen auf mehreren Serverinstanzen bereitgestellt.

In komplexeren Szenarien verwenden Sie ein System, das aus mehreren Komponenten besteht, z.B. aus einer Website und einer Back-End-Verarbeitung.

## Wann sollten Sie separate iKeys verwenden?

Hier sind einige allgemeinen Richtlinien:

* Wenn Sie über eine unabhängig bereitstellbare Anwendungseinheit verfügen, die auf mehreren unabhängig von anderen Komponenten skalierbaren Serverinstanzen ausgeführt wird, ordnen Sie diese in der Regel einer einzigen Ressource zu. Das heißt, für die Anwendungseinheit wird ein einziger Instrumentierungsschlüssel (iKey) verwendet.
* Separate iKeys werden hingegen u.a. aus folgenden Gründen verwendet:
 - Sie möchten separate Metriken aus separaten Komponenten leicht lesen können.
 - Sie möchten geringere Aufkommen an Telemetriedaten von hohem Datenaufkommen trennen, sodass Drosselung, Kontingente und Stichproben für einen Datenstrom keine Auswirkungen auf den anderen haben.
 - Sie möchten Konfigurationen für Warnungen, Exporte und Arbeitsaufgaben voneinander trennen.
 - Sie möchten [Grenzwerte](app-insights-pricing.md#limits-summary), z.B. für Telemetriekontingente, Drosselung und Webtestanzahl, verteilen.
 - Code, der sich im Entwicklungs- oder Teststadium befindet, soll seine Telemetriedaten an einen anderen iKey senden als der Code im Produktivbetrieb.  

Viele Funktionen im Application Insights-Portal spiegeln diese Richtlinien wider. Beispielsweise wird bei der Betrachtung von Segmenten die Serverinstanz berücksichtigt. Es wird also davon ausgegangen, dass Telemetriedaten zu einer logischen Komponente von verschiedenen Serverinstanzen stammen können.

## Ein einziger iKey

Wenn Sie Telemetriedaten von mehreren Komponenten an einen einzigen iKey senden:

* Fügen Sie allen Telemetriedaten eine Eigenschaft hinzu, anhand derer Sie sie segmentieren und nach Komponenten-ID filtern können. Für Serverrolleninstanzen geschieht dies automatisch. In anderen Fällen können Sie die Eigenschaft mit einem [Telemetrieinitialisierer](app-insights-api-filtering-sampling.md#add-properties) hinzufügen.
* Aktualisieren Sie die Application Insights-SDKs in den verschiedenen Komponenten gleichzeitig. Die Telemetriedaten für einen iKey sollten aus der gleichen Version des SDKs stammen.

## Separate iKeys

Wenn Sie mehrere iKeys für verschiedene Anwendungskomponenten verwenden:

* Erstellen Sie ein [Dashboard](app-insights-dashboards.md) zum Anzeigen der wichtigsten Telemetriedaten aus Ihrer logischen Anwendung, in dem die Daten aus den verschiedenen Anwendungskomponenten kombiniert werden. Solche Dashboards können freigegeben werden, damit die einheitliche Ansicht für ein logisches System von verschiedenen Teams genutzt werden kann.
* Organisieren Sie [Ressourcengruppen](app-insights-resources-roles-access-control.md) auf Teamebene. Zugriffsberechtigungen werden nach Ressourcengruppe zugewiesen, und dazu gehören auch die Berechtigungen zum Einrichten von Warnungen. 
* Verwenden Sie [Azure Resource Manager-Vorlagen und PowerShell](app-insights-powershell.md), um Artefakte wie Warnungsregeln und Webtests zu verwalten.



## Separate iKeys für Entwicklung/Test und Produktion

Damit Sie den Schlüssel bei der Veröffentlichung Ihrer App leichter automatisch ändern können, sollten Sie den iKey im Code festlegen und nicht in „ApplicationInsights.config.“

### <a name="dynamic-ikey"></a>Dynamischer Instrumentierungsschlüssel

Legen Sie den Schlüssel in einer Initialisierungsmethode fest, wie z. B. "global.aspx.cs" in einem ASP.NET-Dienst:

*C#*

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey = 
          // - for example -
          WebConfigurationManager.AppSettings["ikey"];
      ...

In diesem Beispiel werden die iKeys für die verschiedenen Ressourcen in verschiedenen Versionen der Webkonfigurationsdatei platziert. Wenn Sie die Webkonfigurationsdatei austauschen – z.B. im Rahmen des Releaseskripts – wird die Zielressource ausgetauscht.

### Webseiten

Der iKey wird auch in Webseiten Ihrer App in dem [Skript verwendet, das Sie auf dem Blatt "Schnellstart" erhalten haben](app-insights-javascript.md). Statt ihn direkt im Skript zu programmieren, generieren Sie ihn über den Serverzustand. Beispielsweise in einer ASP.NET-App:

*JavaScript in Razor*

    <script type="text/javascript">
    // Standard Application Insights web page script:
    var appInsights = window.appInsights || function(config){ ...
    // Modify this part:
    }({instrumentationKey:  
      // Generate from server property:
      @Microsoft.ApplicationInsights.Extensibility.
         TelemetryConfiguration.Active.InstrumentationKey"
    }) // ...


## Erstellen einer weiteren Application Insights-Ressource
  
Wenn Sie sich dafür entschieden haben, die Telemetriedaten für verschiedene Anwendungskomponenten oder für verschiedene Einsatzzwecke der gleichen Komponente (Entwicklung/Test und Produktion) zu trennen, müssen Sie eine neue Application Insights-Ressource erstellen.

Fügen Sie unter [portal.azure.com](https://portal.azure.com) eine neue Application Insights-Ressource hinzu:

![Klicken Sie auf "Neu > Application Insights"](./media/app-insights-separate-resources/01-new.png)


* **Anwendungstyp** bestimmt den Inhalt des Blatts "Übersicht" und die im [Metrik-Explorer](app-insights-metrics-explorer.md) verfügbaren Eigenschaften. Wenn Ihr App-Typ nicht angezeigt wird, wählen Sie einen der Webtypen für Webseiten aus.
* Eine **Ressourcengruppe** ist eine benutzerfreundliche Möglichkeit zum Verwalten von Eigenschaften wie der [Zugriffssteuerung](app-insights-resources-roles-access-control.md). Sie können separate Ressourcengruppen für Entwicklungs-, Test- und Produktionsumgebungen verwenden.
* **Abonnement** ist Ihr Zahlungskonto in Azure.
* **Speicherort** ist der Ort, an dem Ihre Daten aufbewahrt werden. Dieser kann derzeit nicht geändert werden. 
* **Zu Dashboard hinzufügen** legt eine Schnellzugriffskachel für die Ressource auf Ihrer Azure-Startseite ab. 

Das Erstellen der Ressource dauert einige Sekunden. Wenn es abgeschlossen ist, sehen Sie eine Warnung.

(Sie können ein [PowerShell-Skript](app-insights-powershell-script-create-resource.md) schreiben, um eine Ressource automatisch zu erstellen.)


## Abrufen des Instrumentierungsschlüssels

Der Instrumentierungsschlüssel identifiziert die Ressource, die Sie erstellt haben.

![Klicken Sie auf "Essentials", klicken Sie auf den Instrumentierungsschlüssel, STRG+C.](./media/app-insights-separate-resources/02-props.png)

Sie benötigen die Instrumentierungsschlüssel aller Ressourcen, an die Ihre App Daten sendet.


 

<!---HONumber=AcomDC_0518_2016-->