---
title: "Verwendungsanalyse für Webanwendungen mit Azure Application Insights | Microsoft-Dokumentation"
description: Verstehen Sie Ihre Benutzer und wie sie Ihre Web-App verwenden.
services: application-insights
documentationcenter: 
author: botatoes
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 05/03/2017
ms.author: bwren
ms.translationtype: HT
ms.sourcegitcommit: b6c65c53d96f4adb8719c27ed270e973b5a7ff23
ms.openlocfilehash: 80142d9f27abed181bca6f5f536b950198e66e20
ms.contentlocale: de-de
ms.lasthandoff: 08/17/2017

---

# <a name="usage-analysis-for-web-applications-with-application-insights"></a>Verwendungsanalyse für Webanwendungen mit Application Insights

Welche Funktionen Ihrer Web-App sind die beliebtesten? Erreichen die Benutzer mit Ihrer App ihre Ziele? Brechen sie an bestimmten Stellen ab, und kehren sie später zurück?  Mit [Azure Application Insights](app-insights-overview.md) erhalten Sie wertvolle Einblicke in die Nutzung Ihrer Web-App durch die Benutzer. Bei jeder Aktualisierung Ihrer App können Sie beurteilen, wie gut sie für Benutzer funktioniert. Mit diesem Wissen können Sie datengesteuerte Entscheidungen über die nächsten Entwicklungszyklen treffen.

## <a name="send-telemetry-from-your-app"></a>Senden von Telemetriedaten aus der App

Sie erzielen optimale Ergebnisse, wenn Sie Application Insights im Servercode der App und in den Webseiten installieren. Die Client- und die Serverkomponente der App senden Telemetriedaten zur Analyse an das Azure-Portal.

1. **Servercode:** Installieren Sie das entsprechende Modul für die [ASP.NET](app-insights-asp-net.md)-, [Azure](app-insights-azure.md)-, [Java](app-insights-java-get-started.md)-, [Node.js](app-insights-nodejs.md)-App oder einen [anderen Typ](app-insights-platforms.md) von App.

    * *Sie möchten keinen Servercode installieren? [Erstellen Sie einfach eine Azure Application Insights-Ressource](app-insights-create-new-resource.md).*

2. **Webseitencode:** Öffnen Sie das [Azure-Portal](https://portal.azure.com), öffnen Sie die Application Insights-Ressource für die App, und öffnen Sie dann **Erste Schritte > Überwachung und Diagnose der clientseitigen Anwendung**. 

    ![Kopieren Sie das Skript in die Kopfzeile der Masterwebseite.](./media/app-insights-usage-overview/02-monitor-web-page.png)


3. **Abrufen von Telemetriedaten:** Führen Sie das Projekt einige Minuten lang im Debugmodus aus, und suchen Sie dann im Blatt „Übersicht“ von Application Insights nach Ergebnissen.

    Veröffentlichen Sie die App, um die Leistung der App zu überwachen und zu ermitteln, was die Benutzer mit Ihrer App tun.

## <a name="include-user-and-session-id-in-your-telemetry"></a>Einschließen der Benutzer- und Sitzungs-ID in der Telemetrie
Um im Laufe der Zeit Benutzeraktionen nachzuverfolgen, erfordert Application Insights die Möglichkeit, um diese zu kennzeichnen. Das Ereignistool ist das einzige Nutzungstool, das keine Benutzer- oder Sitzungs-ID erfordert.

Beginnen Sie mit dem Senden dieser IDs, indem Sie die Anweisungen unter [diesem Link](https://docs.microsoft.com/azure/application-insights/app-insights-usage-send-user-context) befolgen.

## <a name="explore-usage-demographics-and-statistics"></a>Untersuchen von demografische Daten und Statistiken zur Nutzung
Ermitteln Sie, wann Personen Ihre App verwenden, für welche Seiten sie sich am meisten interessieren, wo sich die Benutzer befinden, welche Browser und Betriebssysteme sie verwenden. 

In Benutzer- und Sitzungsberichte werden Ihre Daten nach Seiten oder benutzerdefinierten Ereignissen gefiltert und nach Eigenschaften wie Speicherort, Umgebung und Seite segmentiert. Sie können auch eigene Filter hinzufügen.

![Benutzer](./media/app-insights-usage-overview/users.png)  

Aus den Informationen auf der rechten Seite gehen interessante Muster im Datensatz hervor.  

* Der Bericht **Benutzer** zählt die eindeutigen Benutzer, die innerhalb der ausgewählten Zeiträume auf Ihre Seiten zugreifen. (Benutzer werden mithilfe von Cookies gezählt. Wenn eine Person mit verschiedenen Browsern oder Clientcomputern auf Ihre Website zugreift oder ihre Cookies löscht, wird sie mehrfach gezählt.)
* Der Bericht **Sitzungen** zählt die Benutzersitzungen, die auf Ihre Website zugreifen. Eine Sitzung ist ein Zeitraum der Aktivität eines Benutzers, der von einem Zeitraum der Inaktivität von mehr als eine halben Stunde beendet wird.

[Weitere Informationen zu den Tools für Benutzer, Sitzungen und Ereignisse](app-insights-usage-segmentation.md)  

## <a name="page-views"></a>Seitenaufrufe

Klicken Sie auf den Blatt „Verwendung“ zur Kachel „Seitenaufrufe“, um eine Aufschlüsselung der beliebtesten Seiten zu erhalten:

![Klicken Sie auf dem Blatt "Übersicht" auf das Diagramm "Seitenaufrufe"](./media/app-insights-usage-overview/05-games.png)

Das vorherige Beispiel gehört zu einer Spielewebsite. Anhand der Diagramme lässt sich sofort Folgendes erkennen:

* Die Nutzung hat sich in der Vorwoche nicht erhöht. Vielleicht sollten wir über eine Suchmaschinenoptimierung nachdenken?
* „Tennis“ ist die beliebteste Spieleseite. Konzentrieren wir uns auf weitere Verbesserungen dieser Seite.
* Die Seite „Tennis“ wird im Durchschnitt dreimal pro Woche besucht. (Es gibt ungefähr dreimal mehr Sitzungen als Benutzer.)
* Die meisten Benutzer besuchen die Website an Arbeitstagen in den USA, und zwar während der Arbeitszeit. Vielleicht sollten wir auf der Website eine Schaltfläche für schnelles Ausblenden bereitstellen.
* Die [Anmerkungen](app-insights-annotations.md) im Diagramm geben an, wenn neue Versionen der Website bereitgestellt wurden. Keine der neuesten Bereitstellungen hatte eine spürbare Auswirkung auf die Nutzung.

Was können Sie tun, wenn Sie Datenverkehr auf Ihrer Website im detaillierter untersuchen möchten, z.B. durch eine Aufteilung nach einer benutzerdefinierten Eigenschaft, die Ihre Website in der Seitenaufruftelemetrie sendet?

1. Öffnen Sie das **Ereignis**-Tool im Application Insights-Ressourcenmenü. Mit diesem Tool können Sie basierend auf einer Vielzahl von Filter-, Kohorten- und Segmentierungsoptionen analysieren, wie viele Seitenansichten und benutzerdefinierte Ereignisse von Ihrer App gesendet wurden.
2. Wählen Sie in der Dropdownliste „Who used“ (Verwendet von) die Option „Any Page View“ (Beliebige Seitenansicht).
3. Wählen Sie in der Dropdownliste „Split by“ (Teilen nach) eine Eigenschaft, anhand deren Ihre Seitenansichtstelemetrie aufgeteilt wird.

## <a name="retention---how-many-users-come-back"></a>Vermerkdauer – wie viele Benutzer kehren zurück?

Anhand der Vermerkdauer können Sie basierend auf Kohorten von Benutzern, die während eines bestimmten Zeitrahmens eine Businessaktion durchgeführt haben, nachvollziehen, wie oft Ihre Benutzer die App erneut verwenden. 

- Ermitteln, welche bestimmten Funktionen dazu führen, dass einige Benutzer häufiger zurückkehren als andere 
- Bilden von Hypothesen basierend auf echten Benutzerdaten 
- Bestimmen, ob die Vermerkdauer ein Problem in Ihrem Produkt darstellt 

![Aufbewahrung](./media/app-insights-usage-overview/retention.png) 

Die Vermerkdauer-Steuerelemente oben ermöglichen Ihnen das Definieren bestimmter Ereignisse und des Zeitbereichs für die Berechnen der Vermerkdauer. Das Diagramm in der Mitte bietet eine visuelle Darstellung des Prozentsatzes der gesamten Vermerkdauer nach dem angegebenen Zeitbereichs. Im Diagramm unten wird eine einzelne Vermerkdauer in einem bestimmten Zeitraum dargestellt. Dank dieses Detaillierungsgrads können Sie mit höherer Granularität verstehen, was Ihre Benutzer tun und was sich auf zurückkehrende Benutzer auswirkt.  

[Weitere Informationen zum Vermerkdauer-Tool](app-insights-usage-retention.md)

## <a name="custom-business-events"></a>Benutzerdefinierte Geschäftsereignisse

Um ein genaues Verständnis davon zu erhalten, was Benutzer mit Ihrer Web-App machen, ist es hilfreich, Codezeilen einzufügen, um benutzerdefinierte Ereignisse zu protokollieren. Diese Ereignisse können alles nachverfolgen – von detaillierten Benutzeraktionen wie das Klicken auf bestimmte Schaltflächen bis hin zu aussagekräftigeren Geschäftsereignissen wie das Tätigen eines Kaufs oder das Gewinnen eines Spiels. 

Seitenaufrufe können zwar in einigen Fällen nützliche Ereignisse darstellen, im Allgemeinen ist dies jedoch nicht der Fall. Ein Benutzer kann eine Produktseite öffnen, ohne das Produkt zu kaufen. 

Mit bestimmten Geschäftsereignisse können Sie den Aufenthalt von Benutzern auf Ihrer Website in einem Diagramm darstellen. Sie können deren Einstellungen für unterschiedliche Optionen ermitteln und herausfinden, wo sie abbrechen oder Schwierigkeiten haben. Mit diesem Wissen können Sie fundierte Entscheidungen über die Prioritäten in Ihrem Entwicklungs-Backlog treffen.

Ereignisse können auf der Webseite protokolliert werden:

```JavaScript

    appInsights.trackEvent("ExpandDetailTab", {DetailTab: tabName});
```

Oder auf der Serverseite der Web-App:

```C#
    var tc = new Microsoft.ApplicationInsights.TelemetryClient();
    tc.TrackEvent("CreatedAccount", new Dictionary<string,string> {"AccountType":account.Type}, null);
    ...
    tc.TrackEvent("AddedItemToCart", new Dictionary<string,string> {"Item":item.Name}, null);
    ...
    tc.TrackEvent("CompletedPurchase");
```

Sie können diesen Ereignissen Eigenschaftswerte anfügen, damit Sie die Ereignisse beim Überprüfen im Portal filtern oder teilen können. Außerdem wird jedem Ereignis ein Standardsatz von Eigenschaften angefügt, z.B. eine anonyme Benutzer-ID, die Ihnen die Ablaufverfolgung der Sequenz von Aktivitäten eines einzelnen Benutzers ermöglicht.

Erfahren Sie mehr über [benutzerdefinierte Ereignisse](app-insights-api-custom-events-metrics.md#trackevent) und [Eigenschaften](app-insights-api-custom-events-metrics.md#properties).

### <a name="slice-and-dice-events"></a>Aufteilen von Ereignissen

In den Tools für Benutzer, Sitzungen und Ereignisse können Sie benutzerdefinierte Ereignisse nach Benutzer, Ereignisname und Eigenschaften aufteilen.
![Benutzer](./media/app-insights-usage-overview/users.png)  
  
## <a name="design-the-telemetry-with-the-app"></a>Entwerfen der Telemetrie mit der App

Berücksichtigen Sie beim Entwerfen der einzelnen Funktionen Ihrer App, wie sie deren Erfolg bei den Benutzern messen werden. Entscheiden Sie, welche Geschäftsereignisse aufgezeichnet werden müssen, und codieren Sie die Aufrufnachverfolgung für diese Ereignisse von Anfang an in die App.

## <a name="a--b-testing"></a>A-B-Tests
Wenn Sie nicht wissen, welche Variante eines Features erfolgreicher sein wird, veröffentlichten Sie beide, und ermöglichen Sie verschiedenen Benutzern den Zugriff darauf. Messen Sie den jeweiligen Erfolg, und erstellen Sie anschließend eine vereinheitlichte Version.

Für dieses Verfahren fügen Sie unterschiedliche Eigenschaftswerte an alle Telemetriedaten an, die von jeder Version Ihrer App gesendet wird. Dazu definieren Sie Eigenschaften im aktiven "TelemetryContext"-Element. Diese Standardeigenschaften werden jeder Telemetrienachricht hinzugefügt, die die Anwendung sendet, nicht nur Ihren benutzerdefinierten Nachrichten, sondern auch den Standardtelemetriedaten.

Filtern und teilen Sie im Application Insights-Portal Ihre Daten anhand der Eigenschaftswerte, um die verschiedenen Versionen zu vergleichen.

Dazu [richten Sie einen Telemetrieinitialisierer ein](app-insights-api-filtering-sampling.md##add-properties-itelemetryinitializer):

```C#


    // Telemetry initializer class
    public class MyTelemetryInitializer : ITelemetryInitializer
    {
        public void Initialize (ITelemetry telemetry)
        {
            telemetry.Properties["AppVersion"] = "v2.1";
        }
    }
```

Im Web-App-Initialisierer wie „Global.asax.cs“:

```C#

    protected void Application_Start()
    {
        // ...
        TelemetryConfiguration.Active.TelemetryInitializers
        .Add(new MyTelemetryInitializer());
    }
```

Alle neuen TelemetryClients-Elemente fügen automatisch den von Ihnen angegebenen Eigenschaftswert hinzu. Einzelne Telemetrieereignisse können die Standardwerte außer Kraft setzen.

## <a name="next-steps"></a>Nächste Schritte
   - [Benutzer, Sitzungen, Ereignisse](app-insights-usage-segmentation.md)
   - [Trichter](usage-funnels.md)
   - [Aufbewahrung](app-insights-usage-retention.md)
   - [Benutzerabläufe](app-insights-usage-flows.md)
   - [Arbeitsmappen](app-insights-usage-workbooks.md)
   - [Hinzufügen von Benutzerkontext](app-insights-usage-send-user-context.md)

