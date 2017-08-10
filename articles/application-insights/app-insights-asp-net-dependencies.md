---
title: "Abhängigkeitsnachverfolgung in Azure Application Insights | Microsoft Docs"
description: "Analysieren Sie die Auslastung, Verfügbarkeit und Leistung Ihrer lokalen oder Microsoft Azure-Webanwendung mit Application Insights."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: d15c4ca8-4c1a-47ab-a03d-c322b4bb2a9e
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: cfreeman
ms.translationtype: HT
ms.sourcegitcommit: c30998a77071242d985737e55a7dc2c0bf70b947
ms.openlocfilehash: 43733e452126c85ab9e19b6036aea96f56fc4d12
ms.contentlocale: de-de
ms.lasthandoff: 08/02/2017

---
# <a name="set-up-application-insights-dependency-tracking"></a>Einrichten von Application Insights: Abhängigkeitsüberwachung
Eine *Abhängigkeit* ist eine externe Komponente, die von Ihrer App aufgerufen wird. In der Regel handelt es sich um einen Dienst, der über HTTP oder eine Datenbank oder ein Dateisystem aufgerufen wird. [Application Insights](app-insights-overview.md) misst, wie lange die Anwendung auf Abhängigkeiten wartet, und wie oft ein Abhängigkeitsaufruf nicht funktioniert. Sie können bestimmte Aufrufe untersuchen, und diese mit Anforderungen und Ausnahmen in Verbindung bringen.

![Beispieldiagramme](./media/app-insights-asp-net-dependencies/10-intro.png)

Der standardmäßig verfügbare Abhängigkeitsmonitor meldet derzeit Aufrufe an diese Abhängigkeitstypen:

* Server
  * SQL-Datenbanken
  * ASP.NET-Web- und WCF-Dienste, die HTTP-basierte Bindungen verwenden
  * Lokale oder Remote-HTTP-Aufrufe
  * Azure Cosmos DB, Tabelle, Blob Storage und Warteschlange
* Webseiten
  * AJAX-Aufrufe

Überwachen funktioniert mit der [Bytecodeinstrumentierung](https://msdn.microsoft.com/library/z9z62c29.aspx) um ausgewählte Methoden. Der Leistungsaufwand ist minimal.

Mit [TrackDependency-API](app-insights-api-custom-events-metrics.md#trackdependency)können Sie auch eigene SDK-Aufrufe zum Überwachen anderer Abhängigkeiten schreiben, sowohl in Client- als auch Servercode.

## <a name="set-up-dependency-monitoring"></a>Einrichten der Abhängigkeitsüberwachung
Teilinformationen von Abhängigkeiten werden automatisch durch das [Application Insights-SDK](app-insights-asp-net.md) gesammelt. Installieren Sie den entsprechenden Agent für den Hostserver, um umfassende Daten zu erhalten.

| Plattform | Installieren |
| --- | --- |
| IIS-Server |[Installieren Sie den Statusmonitor auf dem Server,](app-insights-monitor-performance-live-website-now.md), oder [aktualisieren Sie Ihre Anwendung auf .NET Framework 4.6 oder höher](http://go.microsoft.com/fwlink/?LinkId=528259), und installieren Sie das [Application Insights SDK](app-insights-asp-net.md) in Ihrer App. |
| Azure-Web-App |[Öffnen Sie das Blatt „Application Insights“ in der Systemsteuerung Ihrer Web-App](app-insights-azure-web-apps.md) und wählen Sie bei Aufforderung „Installieren“ aus. |
| Azure Cloud Service |[Verwenden Sie die Startaufgabe](app-insights-cloudservices.md), oder [Installieren Sie .NET Framework 4.6 oder höher](../cloud-services/cloud-services-dotnet-install-dotnet.md) |

## <a name="where-to-find-dependency-data"></a>Hier finden Sie Abhängigkeitsdaten
* [Anwendungszuordnung](#application-map) visualisiert Abhängigkeiten zwischen Ihrer App und angrenzenden Komponenten.
* [Die Blätter „Performance“ (Leistung) „Browser“ und „Failure“ (Fehler)](#performance-and-blades) zeigen Serverabhängigkeitsdaten.
* [Das Blatt „Browser“](#ajax-calls) zeigt AJAX-Aufrufe von Browsern Ihrer Benutzer.
* [Navigieren Sie zu langsamen oder fehlgeschlagenen Aufrufen](#diagnose-slow-requests), um ihre Abhängigkeitsaufrufe zu überprüfen.
* [Analyse](#analytics) kann verwendet werden, um Abhängigkeitsdaten abzufragen.

## <a name="application-map"></a>Anwendungszuordnung
Die Anwendungszuordnung dient als visuelle Hilfe zum Erkennen von Abhängigkeiten zwischen den Komponenten Ihrer Anwendung. Sie wird automatisch aus der Telemetrie Ihrer App generiert. Dieses Beispiel zeigt die AJAX-Aufrufe aus den Browserskripts und REST-Aufrufe aus der Serveranwendung zu zwei externen Diensten.

![Anwendungszuordnung](./media/app-insights-asp-net-dependencies/08.png)

* **Navigieren Sie aus den Feldern** zur relevanten Abhängigkeit und zu anderen Diagrammen.
* **Heften Sie die Zuordnung** an das [Dashboard](app-insights-dashboards.md) an, wo sie voll funktionsfähig sein wird.

[detaillierte Kapazitätsplanung](app-insights-app-map.md)

## <a name="performance-and-failure-blades"></a>Blätter „Performance“ und „Failure“
Das Blatt „Performance“ zeigt die Dauer von Abhängigkeitsaufrufen, die durch die Serveranwendung durchgeführt werden. Es gibt ein zusammenfassendes Diagramm und eine nach Aufrufen unterteilte Tabelle.

![Abhängigkeitsdiagramme Blatt „Performance“](./media/app-insights-asp-net-dependencies/dependencies-in-performance-blade.png)

Klicken Sie sich durch die Abhängigkeitsdiagramme oder die Tabellenelemente, um nach Rohvorkommen dieser Aufrufe zu suchen.

![Instanzen Abhängigkeitsaufrufe](./media/app-insights-asp-net-dependencies/dependency-call-instance.png)

**Die Fehleranzahl** wird auf dem Blatt **Failures** angezeigt. Ein Fehler ist jeder Rückgabecode, der nicht im Bereich 200-399 liegt, oder unbekannt ist.

> [!NOTE]
> **100 % Fehler?** - Dies liegt wahrscheinlich daran, dass Sie nur teilweise Abhängigkeitsdaten erhalten. Sie müssen [die für Ihre Plattform geeignete Abhängigkeitsüberwachung einrichten](#set-up-dependency-monitoring).
>
>

## <a name="ajax-calls"></a>AJAX-Aufrufe
Das Blatt „Browser“ zeigt die Dauer und Fehlerquote von AJAX-Aufrufen durch [JavaScript auf Ihren Webseiten](app-insights-javascript.md) an. Sie werden als Abhängigkeiten angezeigt.

## <a name="diagnosis"></a>Diagnostizieren langsamer Anforderungen
Jedes Anforderungsereignis bezieht sich auf Abhängigkeitsaufrufe, Ausnahmen und andere Ereignisse, die nachverfolgt werden, während Ihre App die Anforderung verarbeitet. Wenn einige Anforderungen also eine schlechte Leistung zeigen, können Sie herausfinden, ob es an langsamen Antworten einer Abhängigkeit liegt.

Wir sehen uns nun ein Beispiel dazu an.

### <a name="tracing-from-requests-to-dependencies"></a>Ablaufverfolgung von Anforderungen bis Abhängigkeiten
Öffnen Sie das Blatt „Performance“, und betrachten Sie das Raster der Anforderungen:

![Liste der Anforderungen mit Mittelwerten und Anzahlen](./media/app-insights-asp-net-dependencies/02-reqs.png)

Die zuerst aufgeführte Instanz dauert sehr lange. Mal sehen, ob wir herausfinden, wo die Zeit beansprucht wird.

Klicken Sie auf diese Zeile, um einzelne Anforderungsereignisse anzuzeigen:

![Liste der Anforderungsinstanzen](./media/app-insights-asp-net-dependencies/03-instances.png)

Klicken Sie auf eine beliebige Instanz mit langer Ausführungsdauer, um diese näher zu überprüfen, und scrollen Sie nach unten zu den Remoteabhängigkeitsaufrufen, die im Zusammenhang mit dieser Anforderung bestehen:

![Finden von Aufrufen von Remoteabhängigkeiten, Identifizieren ungewöhnlich langer Laufzeiten](./media/app-insights-asp-net-dependencies/04-dependencies.png)

Der Großteil der Zeit zur Verarbeitung dieser Anforderung wurde für einen Aufruf eines lokalen Diensts aufgewendet.

Wählen Sie diese Zeile aus, um weitere Informationen zu erhalten:

![Klicken Sie sich durch diese Remoteabhängigkeit, um die Ursache herauszufinden](./media/app-insights-asp-net-dependencies/05-detail.png)

Anscheinend befindet sich das Problem hier. Nachdem das Problem jetzt identifiziert ist, müssen wir noch herausfinden, warum dieser Aufruf so lange dauert.

### <a name="request-timeline"></a>Anfordern der Zeitachse
In einem anderen Fall handelt es sich nicht um einen Abhängigkeitsaufruf mit langer Ausführungsdauer. Wenn wir aber zur Zeitachsenansicht wechseln, können wir sehen, wo die Verzögerung in unserer internen Verarbeitung aufgetreten ist:

![Finden von Aufrufen von Remoteabhängigkeiten, Identifizieren ungewöhnlich langer Laufzeiten](./media/app-insights-asp-net-dependencies/04-1.png)

Es scheint eine große Unterbrechung nach dem ersten Abhängigkeitsaufruf zu geben, daher sollten wir den Code betrachten, um den Grund dafür herauszufinden.

### <a name="profile-your-live-site"></a>Erstellen eines Profils Ihrer Livewebsite

Sie möchten wissen, was am längsten gedauert hat? Der [Application Insights-Profiler](app-insights-profiler.md) verfolgt HTTP-Aufrufe zu Ihrer Livewebsite zurück, und zeigt an, welche Funktionen im Code die meiste Zeit in Anspruch genommen haben.

## <a name="failed-requests"></a>Anforderungsfehler
Anforderungsfehler können auch fehlgeschlagenen Aufrufen von Abhängigkeiten zugeordnet werden. Wir können erneut bis zum Problem durchklicken.

![Klicken Sie auf das Diagramm mit Anforderungsfehlern](./media/app-insights-asp-net-dependencies/06-fail.png)

Klicken Sie auf einen Anforderungsfehler, und sehen Sie sich die zugeordneten Ereignisse an.

![Klicken Sie auf den Anforderungstyp, klicken Sie auf die Instanz, um eine andere Ansicht derselben Instanz abzurufen, und klicken Sie darauf, um Details zur Ausnahme zu erhalten.](./media/app-insights-asp-net-dependencies/07-faildetail.png)

## <a name="analytics"></a>Analyse
Sie können Abhängigkeiten in der [Log Analytics-Abfragesprache](https://docs.loganalytics.io/) nachverfolgen. Hier einige Beispiele.

* Suchen fehlgeschlagener Abhängigkeitsaufrufe:

```

    dependencies | where success != "True" | take 10
```

* Suchen von AJAX-Aufrufen:

```

    dependencies | where client_Type == "Browser" | take 10
```

* Suchen von mit Anforderungen verbundenen Abhängigkeitsaufrufen:

```

    dependencies
    | where timestamp > ago(1d) and  client_Type != "Browser"
    | join (requests | where timestamp > ago(1d))
      on operation_Id  
```


* Suchen von mit Seitenaufrufen verbundenen AJAX-Aufrufen:

```

    dependencies
    | where timestamp > ago(1d) and  client_Type == "Browser"
    | join (browserTimings | where timestamp > ago(1d))
      on operation_Id
```



## <a name="custom-dependency-tracking"></a>Benutzerdefinierte Nachverfolgung von Abhängigkeiten
Das Standardmodul für die Nachverfolgung von Abhängigkeiten ermittelt automatisch externe Abhängigkeiten wie Datenbanken und REST-APIs. Es kann jedoch gewünscht sein, einige zusätzliche Komponenten auf die gleiche Weise zu behandeln.

Sie können Code schreiben, der Abhängigkeitsinformationen unter Verwendung der gleichen [TrackDependency-API](app-insights-api-custom-events-metrics.md#trackdependency) sendet, die von den Standardmodulen verwendet wird.

Beispiel: Wenn Sie Ihren Code mit einer Assembly erstellen, die Sie nicht selbst geschrieben haben, könnten Sie die Zeit aller Aufrufe ermitteln, um herauszufinden, welchen Beitrag sie an Ihren Reaktionszeiten hat. Um diese Daten in den Abhängigkeitsdiagrammen in Application Insights anzuzeigen, senden Sie sie mit `TrackDependency`.

```C#

            var startTime = DateTime.UtcNow;
            var timer = System.Diagnostics.Stopwatch.StartNew();
            try
            {
                success = dependency.Call();
            }
            finally
            {
                timer.Stop();
                telemetry.TrackDependency("myDependency", "myCall", startTime, timer.Elapsed, success);
            }
```

Wenn Sie das Standardmodul für die Nachverfolgung von Abhängigkeiten deaktivieren möchten, entfernen Sie den Verweis auf "DependencyTrackingTelemetryModule" in [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md).

## <a name="troubleshooting"></a>Problembehandlung
*Das Erfolgsflag für die Abhängigkeit zeigt immer entweder TRUE oder FALSE.*

*SQL-Abfrage, die nicht vollständig angezeigt wird.*

* Führen Sie ein Upgrade auf die neueste Version des SDK durch. Wenn Ihre Version von .NET niedriger als 4.6. ist:
  * IIS-Host: Installieren Sie den [Application Insights-Agent](app-insights-monitor-performance-live-website-now.md) auf den Hostservern.
  * Azure-Web-App: Öffnen Sie die Schaltfläche „Application Insights“ in der Systemsteuerung der Web-App, und installieren Sie Application Insights.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a>Nächste Schritte
* [Exceptions](app-insights-asp-net-exceptions.md)
* [Daten zu Seiten und Benutzern](app-insights-javascript.md)
* [Availability](app-insights-monitor-web-app-availability.md)

