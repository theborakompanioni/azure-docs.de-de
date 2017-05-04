---
title: "Azure Application Insights – häufig gestellte Fragen | Microsoft-Dokumentation"
description: "Häufig gestellte Fragen zu Application Insights."
services: application-insights
documentationcenter: .net
author: alancameronwills
manager: carmonm
ms.assetid: 0e3b103c-6e2a-4634-9e8c-8b85cf5e9c84
ms.service: application-insights
ms.workload: mobile
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 04/12/2017
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: 0c4554d6289fb0050998765485d965d1fbc6ab3e
ms.openlocfilehash: 191d9e1197c3526d2f72b972b7fada76dee84447
ms.lasthandoff: 04/13/2017


---
# <a name="application-insights-frequently-asked-questions"></a>Application Insights: Häufig gestellte Fragen

## <a name="configuration-problems"></a>Konfigurationsprobleme
*Ich benötige Hilfe beim Einrichten von Folgendem:*

* [.NET-App](app-insights-asp-net-troubleshoot-no-data.md)
* [Überwachen einer bereits ausgeführten App](app-insights-monitor-performance-live-website-now.md#troubleshooting-runtime-configuration-of-application-insights)
* [Azure-Diagnose](app-insights-azure-diagnostics.md)
* [Java-Web-Apps](app-insights-java-troubleshoot.md)

*Ich erhalte keine Daten von meinem Server.*

* [Festlegen von Firewallausnahmen](app-insights-ip-addresses.md)
* [Einrichten eines ASP.NET-Servers](app-insights-monitor-performance-live-website-now.md)
* [Einrichten eines Java-Servers](app-insights-java-agent.md)

## <a name="can-i-use-application-insights-with-"></a>Kann ich Application Insights mit ... verwenden?

* [Web-Apps auf einem IIS-Server – lokal oder auf einem virtuellen Computer](app-insights-asp-net.md)
* [Java-Web-Apps](app-insights-java-get-started.md)
* [Node.js-Apps](app-insights-nodejs.md)
* [Web-Apps in Azure](app-insights-azure-web-apps.md)
* [Cloud Services in Azure](app-insights-cloudservices.md)
* [In Docker ausgeführte App-Server](app-insights-docker.md)
* [Einzelseiten-Web-Apps](app-insights-javascript.md)
* [SharePoint](app-insights-sharepoint.md)
* [Windows-Desktop-App](app-insights-windows-desktop.md)
* [Andere Plattformen](app-insights-platforms.md)

## <a name="is-it-free"></a>Ist es kostenlos?

Ja, zur experimentellen Verwendung. Im Basic-Tarif kann Ihre Anwendung kostenlos ein bestimmtes monatliches Datenkontingent senden. Das kostenlose Kontingent ist groß genug, um Entwicklungsaufgaben auszuführen und eine App für eine geringe Anzahl von Benutzern zu veröffentlichen. Sie können eine Obergrenze festlegen, um sicherzustellen, dass nicht mehr als die angegebene Datenmenge verarbeitet wird.

Größere Mengen von Telemetriedaten werden nach GB in Rechnung gestellt. Wir bieten einige Tipps zum [Begrenzen der Gebühren](app-insights-pricing.md).

Im Enterprise-Plan fallen für jeden Tag, an dem die einzelnen Web-Serverknoten Telemetriedaten senden, Gebühren an. Er ist geeignet, wenn Sie in großem Umfang fortlaufenden Export verwenden möchten.

Preisinformationen finden Sie [hier](https://azure.microsoft.com/pricing/details/application-insights/).

## <a name="how-much-is-it-costing"></a>Wie hoch sind die Kosten?

* Öffnen Sie in einer Application Insights-Ressource die Seite **Features und Preise**. Dort finden Sie ein Diagramm der kürzlichen Nutzung. Sie können ggf. eine Obergrenze für das Datenvolumen festlegen.
* Öffnen Sie das Blatt für die [Azure-Abrechnung](https://portal.azure.com/#blade/Microsoft_Azure_Billing/BillingBlade/Overview), um Ihre Rechnungen für alle Ressourcen anzuzeigen.

## <a name="q14"></a>Welche Änderungen nimmt Application Insights in meinem Projekt vor?
Die Details hängen von der Art des Projekts ab. Für eine Webanwendung:

* Fügen Sie folgende Dateien zu Ihrem Projekt hinzu:

  * ApplicationInsights.config
  * ai.js
* Diese NuGet-Pakete werden installiert:

  * *Application Insights API* - die Haupt-API
  * *Application Insights API for Web Applications* - zum Senden von Telemetrie vom Server
  * *Application Insights API for JavaScript Applications* - zum Senden von Telemetrie vom Client

    Die Pakete umfassen folgende Assemblys:
  * Microsoft.ApplicationInsights
  * Microsoft.ApplicationInsights.Platform
* Fügt Elemente ein in:

  * Web.config
  * packages.config
* (Nur neue Projekte – wenn Sie [Application Insights zu einem vorhandenen Webprojekt hinzufügen][start], müssen Sie diesen Schritt manuell ausführen.) Fügen Sie Codeausschnitte in den Client- und Servercode ein, um diese mit der Application Insights-Ressourcen-ID zu initialisieren. Beispielsweise wird in einer MVC-App Code auf der Masterseite "Views/Shared/_Layout.cshtml" eingefügt.

## <a name="how-do-i-upgrade-from-older-sdk-versions"></a>Wie aktualisiere ich von älteren SDK-Versionen?
Informationen hierzu finden Sie in den [Versionshinweisen](app-insights-release-notes.md) für das SDK, das für Ihren Anwendungstyp geeignet ist.

## <a name="update"></a>Wie kann ich ändern, an welche Azure-Ressource mein Projekt Daten sendet?
Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf `ApplicationInsights.config`. Wählen Sie dann **Application Insights aktualisieren** aus. Sie können die Daten an eine vorhandene oder neue Ressource in Azure senden. Der Assistent ändert den Instrumentationsschlüssel in ApplicationInsights.config. Dadurch wird bestimmt, wohin das Server-SDK die Daten sendet. Wenn Sie "Alle aktualisieren" deaktivieren, wird auch der Anzeigeort des Schlüssels auf Ihren Webseiten geändert.

## <a name="what-is-status-monitor"></a>Was ist der Statusmonitor?

Eine Desktop-App, die Sie auf Ihrem IIS-Webserver zum Konfigurieren von Application Insights in Web-Apps verwenden können. Sie sammelt keine Telemetriedaten; Sie können sie beenden, wenn Sie keine App konfigurieren. 

[Weitere Informationen](app-insights-monitor-performance-live-website-now.md#questions).

## <a name="what-telemetry-is-collected-by-application-insights"></a>Welche Telemetriedaten werden von Application Insights gesammelt?

Von Server-Web-Apps:

* HTTP-Anforderungen
* [Abhängigkeiten](app-insights-asp-net-dependencies.md) Aufrufe von SQL-Datenbanken, HTTP-Aufrufe externer Dienste, Aufrufe von Azure DocumentDB, Tabellen, Blob Storage und Warteschlangen. 
* [Ausnahmen](app-insights-asp-net-exceptions.md) und Stapelüberwachungen.
* [Leistungsindikatoren](app-insights-performance-counters.md) – bei Verwendung von [Statusmonitor](app-insights-monitor-performance-live-website-now.md), Azure-Überwachung („app-insights-azure-web-apps.md“) oder [collectd-Writer für Application Insights](app-insights-java-collectd.md).
* [Benutzerdefinierte Ereignisse und Metriken](app-insights-api-custom-events-metrics.md), die Sie codieren.
* [Ablaufverfolgungsprotokolle](app-insights-asp-net-trace-logs.md), wenn Sie den entsprechenden Sammler konfigurieren.

Über [Clientwebseiten](app-insights-javascript.md):

* [Anzahl von Seitenaufrufen](app-insights-web-track-usage.md)
* [AJAX-Aufrufe](app-insights-asp-net-dependencies.md) – Anforderungen aus einem derzeit ausgeführten Skript.
* Daten zum Laden von Seitenansichten
* Anzahl von Benutzern und Sitzungen
* [IDs authentifizierter Benutzer](app-insights-api-custom-events-metrics.md#authenticated-users)

Aus anderen Quellen, sofern Sie sie konfigurieren:

* [Azure-Diagnose](app-insights-azure-diagnostics.md)
* [Docker-Container](app-insights-docker.md)
* [In Analytics importierte Tabellen](app-insights-analytics-import.md)
* [OMS (Log Analytics)](https://azure.microsoft.com/blog/omssolutionforappinsightspublicpreview/)
* [Logstash](app-insights-analytics-import.md)

## <a name="can-i-filter-out-or-modify-some-telemetry"></a>Kann ich Telemetriedaten filtern oder ändern?

Ja, auf dem Server können Sie Folgendes schreiben:

* Einen Telemetrieprozessor, um Eigenschaften zu filtern oder ausgewählten Telemetrieelementen hinzuzufügen, bevor sie von der App gesendet werden
* Eine Telemetrieinitialisierer, um allen Telemetrieelementen Eigenschaften hinzuzufügen

Weitere Informationen über [ASP.NET](app-insights-api-filtering-sampling.md) und [Java](app-insights-java-filter-telemetry.md).

## <a name="how-are-city-country-and-other-geo-location-data-calculated"></a>Wie werden Daten zu Ort, Land und andere Geolocation-Daten berechnet?

Mit [GeoLite2](http://dev.maxmind.com/geoip/geoip2/geolite2/) wird die IP-Adresse (IPv4 oder IPv6) des Webclients gesucht.

* Browsertelemetrie: Die IP-Adresse des Absenders wird gesammelt.
* Servertelemetrie: Das Application Insights-Modul sammelt die Client-IP-Adresse. Sie wird nicht gesammelt, wenn `X-Forwarded-For` festgelegt ist.

Sie können in der Konfiguration festlegen, dass `ClientIpHeaderTelemetryInitializer` die IP-Adresse aus einem anderen Header akzeptiert. In manchen Systemen wird sie z.B. von einem Proxy, durch den Lastenausgleich oder das CDN nach `X-Originating-IP` verschoben. [Weitere Informationen](http://apmtips.com/blog/2016/07/05/client-ip-address/).

Sie können [mit Power BI](app-insights-export-power-bi.md) die Anforderungstelemetriedaten auf einer Karte anzeigen.


## <a name="data"></a>Wie lange werden Daten im Portal aufbewahrt? Ist Sicherheit gewährleistet?
Informationen hierzu finden Sie unter [Datensammlung, -aufbewahrung und -speicherung in Application Insights][data].

## <a name="might-personally-identifiable-information-pii-be-sent-in-the-telemetry"></a>Können in den Telemetriedaten personenbezogene Informationen gesendet werden?

Dies ist möglich, wenn der Code solche Daten sendet. Es kann auch vorkommen, wenn Variablen in den Stapelüberwachungen personenbezogene Informationen enthalten. Ihr Entwicklungsteam sollte Risikobewertungen durchführen, um sicherzustellen, dass personenbezogene Informationen ordnungsgemäß behandelt werden. [Weitere Informationen zu Datenaufbewahrung und Datenschutz](app-insights-data-retention-privacy.md).

Das letzte Oktett der Clientwebadresse wird nach der Erfassung durch das Portal immer auf 0 festgelegt.

## <a name="my-ikey-is-visible-in-my-web-page-source"></a>Mein iKey ist in meiner Webseitenquelle sichtbar. 

* Dies ist in Überwachungslösungen üblich.
* Er kann nicht zum Diebstahl Ihrer Daten verwendet werden.
* Er kann verwendet werden, um Datenschiefe zu bewirken oder Warnungen auszulösen.
* Wir wissen von keinem Kunden, bei dem solche Probleme aufgetreten sind.

Sie haben folgende Möglichkeiten, um das Problem auszuschließen:

* Verwenden Sie zwei verschiedene iKeys (unterschiedliche Application Insights-Ressourcen) für Client- und Serverdaten. oder
* Schreiben Sie einen Proxy, der auf dem Server ausgeführt wird, und lassen Sie den Webclient Daten über diesen Proxy senden.

## <a name="post"></a>Wie zeige ich POST-Daten in der Diagnosesuche an?
POST-Daten werden nicht automatisch protokolliert, Sie können jedoch einen TrackTrace-Aufruf verwenden und die Daten in den Nachrichtenparameter einfügen. Die Größenbegrenzung hierfür ist höher als bei Zeichenfolgeneigenschaften, Sie können jedoch nicht danach filtern.

## <a name="should-i-use-single-or-multiple-application-insights-resources"></a>Sollte ich eine einzelne oder mehrere Application Insights-Ressourcen verwenden?

* [Konsultieren Sie die Erörterung hier](app-insights-separate-resources.md).
* [Beispiel – Clouddienst mit Worker- und Webrolle](app-insights-cloudservices.md)

## <a name="how-do-i-dynamically-change-the-instrumentation-key"></a>Wie kann ich den Instrumentationsschlüssel dynamisch ändern?

* [Die Erläuterung finden Sie hier](app-insights-separate-resources.md).
* [Beispiel – Clouddienst mit Worker- und Webrolle](app-insights-cloudservices.md)

## <a name="what-are-the-user-and-session-counts"></a>Was sind Benutzer- und Sitzungszähler?

* Das JavaScript-SDK legt im Webclient ein Benutzercookie zum Identifizieren wiederkehrender Benutzer und ein Sitzungscookie zum Gruppieren von Aktivitäten fest.
* Wenn kein clientseitiges Skript vorhanden ist, können Sie [Cookies auf dem Server festlegen](http://apmtips.com/blog/2016/07/09/tracking-users-in-api-apps/).
* Wenn ein realer Benutzer Ihre Website in verschiedenen Browsern, beim InPrivate- oder Inkognito-Browsing oder auf unterschiedlichen Computern verwendet, wird er mehrmals gezählt.
* Um einen angemeldeten Benutzer auf verschiedenen Computern und in verschiedenen Browser zu identifizieren, legen Sie [setAuthenticatedUserContext()](app-insights-api-custom-events-metrics.md#authenticated-users) fest.

## <a name="q17"></a> Habe ich alles in Application Insights aktiviert?
| Diese Daten sollten angezeigt werden | So erhalten Sie die Daten | Deshalb benötigen Sie die Daten |
| --- | --- | --- |
| Verfügbarkeitsdiagramme |[Webtests](app-insights-monitor-web-app-availability.md) |Information, ob Ihre Web-App verfügbar ist |
| Leistung der Server-App: Antwortzeiten usw. ... |[Fügen Sie Ihrem Projekt Application Insights hinzu](app-insights-asp-net.md) oder [installieren Sie den AI-Statusmonitor auf dem Server](app-insights-monitor-performance-live-website-now.md) (oder schreiben Sie Ihren eigenen Code zum [Nachverfolgung von Aufrufen](app-insights-api-custom-events-metrics.md#trackdependency)) |Erkennen von Leistungsproblemen |
| Telemetriedaten zu Abhängigkeiten |[Installieren des AI-Statusmonitors auf dem Server](app-insights-monitor-performance-live-website-now.md) |Diagnostizieren von Problemen mit Datenbanken oder anderen externen Komponenten |
| Abrufen von Stapelüberwachungen aus Ausnahmen |[Einfügen von TrackException-Aufrufen in Ihren Code](app-insights-asp-net-exceptions.md) (einige werden jedoch automatisch gemeldet) |Erkennen und Diagnostizieren von Ausnahmen |
| Durchsuchen von Ablaufprotokollen |[Hinzufügen eines Protokollierungsadapters](app-insights-asp-net-trace-logs.md) |Diagnostizieren von Ausnahmen und Leistungsproblemen |
| Grundlegende Clientnutzung: Seitenansichten, Sitzungen, ... |[JavaScript-Initialisierer in Webseiten](app-insights-javascript.md) |Nutzungsanalyse |
| Benutzerdefinierte Metriken auf Clients |[Nachverfolgen von Aufrufen in Webseiten](app-insights-api-custom-events-metrics.md) |Verbessern der Benutzerfreundlichkeit |
| Benutzerdefinierte Metriken auf Servern |[Nachverfolgen von Aufrufen auf dem Server](app-insights-api-custom-events-metrics.md) |Business Intelligence |

## <a name="why-are-the-counts-in-search-and-metrics-charts-unequal"></a>Warum sind die Zahlen in den Diagrammen „Suchen“ und „Metriken“ unterschiedlich?

Durch [Sampling](app-insights-sampling.md) wird die Anzahl der Telemetrieelemente (Anforderungen, benutzerdefinierte Ereignisse usw.), die tatsächlich von der App an das Portal gesendet werden, verringert. In „Suchen“ wir die Anzahl der tatsächlich empfangenen Elemente angezeigt. In Metrikdiagrammen, in denen die Anzahl der Ereignisse angezeigt wird, wird die Anzahl der ursprünglich eingetretenen Ereignisse angezeigt. 

Jedes übertragene Element verfügt über die `itemCount`-Eigenschaft, die angibt, wie viele ursprüngliche Ereignisse das Element darstellt. Sie können die folgende Abfrage in Analytics ausführen, um zu sehen, wie Sampling erfolgt:

```
    requests | summarize original_events = sum(itemCount), transmitted_events = count()
```


## <a name="automation"></a>Automation

### <a name="configuring-application-insights"></a>Konfigurieren von Application Insights

Sie können mit dem Azure-Ressourcenmonitor [PowerShell-Skripts schreiben](app-insights-powershell.md), um folgende Aufgaben auszuführen:

* Erstellen und Aktualisieren von Application Insights-Ressourcen
* Festlegen des Tarifs
* Abrufen des Instrumentierungsschlüssels
* Hinzufügen einer Metrikwarnung
* Hinzufügen eines Verfügbarkeitstests

Sie können keinen Metrik-Explorer-Bericht oder fortlaufende Exporte einrichten.

### <a name="querying-the-telemetry"></a>Abfragen der Telemetriedaten

Verwenden Sie die [REST-API](https://dev.applicationinsights.io/) zum Ausführen von [Analytics](app-insights-analytics.md)-Abfragen.

## <a name="how-can-i-set-an-alert-on-an-event"></a>Wie kann ich eine Warnung für ein Ereignis festlegen?

Azure-Warnungen gelten nur für Metriken. Erstellen Sie eine benutzerdefinierte Metrik, die immer einen Schwellenwert überschreitet, wenn das Ereignis eintritt. Legen Sie dann eine Warnung für die Metrik fest. Beachten Sie Folgendes: Sie erhalten immer eine Benachrichtigung, wenn die Metrik den Schwellenwert in einer der beiden Richtungen überschreitet. Vor der ersten Überschreitung erhalten Sie keine Benachrichtigung, unabhängig von der Höhe des Anfangswerts. Vor der Benachrichtigung liegt immer eine Wartezeit von einigen Minuten.

## <a name="are-there-data-transfer-charges-between-an-azure-web-app-and-application-insights"></a>Fallen Gebühren für die Datenübertragung zwischen einer Azure-Web-App und Application Insights an?

* Wenn Ihre Azure-Web-App in einem Rechenzentrum gehostet wird, die einen Application Insights-Sammlungsendpunkt enthält, fallen keine Gebühren an. 
* Wenn das Hostrechenzentrum keinen Sammlungsendpunkt enthält, fallen für die Telemetriedaten [Azure-Gebühren für ausgehende Daten](https://azure.microsoft.com/pricing/details/bandwidth/) an.

Dies gilt unabhängig davon, wo Ihre Application Insights-Ressource gehostet wird. Es hängt nur von der Verteilung unserer Endpunkte ab.

## <a name="can-i-send-telemetry-to-the-application-insights-portal"></a>Kann ich Telemetriedaten an das Application Insights-Portal senden?

Es wird empfohlen, unsere SDKs und die SDK-API („app-insights-api-custom-events-metrics.md“) zu verwenden. Es gibt Varianten des SDK für verschiedene [Plattformen](app-insights-platforms.md). Diese SDKs behandeln Pufferung, Komprimierung, Drosselung, Wiederholungen usw. Das [Erfassungsschema](https://github.com/Microsoft/ApplicationInsights-dotnet/tree/develop/Schema/PublicSchema) und das [Endpunktprotokoll](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/EndpointSpecs/ENDPOINT-PROTOCOL.md) sind jedoch öffentlich.

## <a name="can-i-monitor-an-intranet-web-server"></a>Kann ich einen Intranetwebserver überwachen?

Hier sind zwei Methoden:

### <a name="firewall-door"></a>Tür in der Firewall

Lassen Sie zu, dass Ihr Webserver Telemetriedaten an unsere Endpunkte https://dc.services.visualstudio.com:443 und https://rt.services.visualstudio.com:443 sendet. 

### <a name="proxy"></a>Proxy

Leiten Sie Datenverkehr von Ihrem Server zu einem Gateway im Intranet weiter, indem Sie in „ApplicationInsights.config“ Folgendes festlegen:

```XML
<TelemetryChannel>
    <EndpointAddress>your gateway endpoint</EndpointAddress>
</TelemetryChannel>
```

Ihr Gateway leitet dann den Datenverkehr an https://dc.services.visualstudio.com:443/v2/track weiter.

## <a name="can-i-run-availability-web-tests-on-an-intranet-server"></a>Kann ich auf einem Intranetserver Verfügbarkeitswebtests ausführen?

Unsere [Webtests](app-insights-monitor-web-app-availability.md) werden auf Points of Presence aufgeführt, die über den gesamten Globus verteilt sind. Es gibt zwei Lösungen:

* Tür in der Firewall – Lassen Sie Anforderungen von der [umfangreichen und änderbaren Liste von Webtest-Agents](app-insights-ip-addresses.md) an Ihren Server zu.
* Schreiben Sie eigenen Code, um aus dem Intranet regelmäßig Anforderungen an den Server zu senden. Sie können zu diesem Zweck Visual Studio-Webtests ausführen. Der Tester kann die Ergebnisse mithilfe der TrackAvailability()-API an Application Insights senden.

## <a name="more-answers"></a>Weitere Antworten
* [Application Insights-Forum](https://social.msdn.microsoft.com/Forums/vstudio/en-US/home?forum=ApplicationInsights)

<!--Link references-->

[data]: app-insights-data-retention-privacy.md
[platforms]: app-insights-platforms.md
[start]: app-insights-overview.md
[windows]: app-insights-windows-get-started.md

