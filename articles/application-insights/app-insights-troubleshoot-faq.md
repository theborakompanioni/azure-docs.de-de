---
title: Problembehandlung und Fragen zu Azure Application Insights | Microsoft-Dokumentation
description: Ist Ihnen irgendetwas in Azure Application Insights unklar oder funktioniert nicht wie erwartet? Versuchen Sie es hier.
services: application-insights
documentationcenter: .net
author: alancameronwills
manager: douge
ms.assetid: 0e3b103c-6e2a-4634-9e8c-8b85cf5e9c84
ms.service: application-insights
ms.workload: mobile
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 08/24/2016
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: 75b651bd3e77ac19e22dcc3442870469fe2aaca1
ms.openlocfilehash: 22d6222b8fd27eeeb562af1ba50164031c885f25


---
# <a name="questions---application-insights-for-aspnet"></a>Fragen – Application Insights für ASP.NET
## <a name="configuration-problems"></a>Konfigurationsprobleme
*Ich benötige Hilfe beim Einrichten von Folgendem:*

* [.NET-App](app-insights-asp-net-troubleshoot-no-data.md)
* [Überwachen einer bereits ausgeführten App](app-insights-monitor-performance-live-website-now.md#troubleshooting)
* [Azure-Diagnose](app-insights-azure-diagnostics.md)
* [Java-Web-Apps](app-insights-java-troubleshoot.md)
* [Andere Plattformen](app-insights-platforms.md)

*Ich erhalte keine Daten von meinem Server.*

* [Festlegen von Firewallausnahmen](app-insights-ip-addresses.md)
* [Einrichten eines ASP.NET-Servers](app-insights-monitor-performance-live-website-now.md)
* [Einrichten eines Java-Servers](app-insights-java-agent.md)

## <a name="can-i-use-application-insights-with-"></a>Kann ich Application Insights mit ... verwenden?
Informationen hierzu finden Sie unter [Plattformen][platforms].

## <a name="is-it-free"></a>Ist es kostenlos?

Ja, zur experimentellen Verwendung. Im Standardtarif kann Ihre Anwendung kostenlos ein bestimmtes monatliches Datenkontingent senden. Das kostenlose Kontingent ist groß genug, um Entwicklungsaufgaben auszuführen und eine App für eine geringe Anzahl von Benutzern zu veröffentlichen. Sie können eine Obergrenze festlegen, um sicherzustellen, dass nicht mehr als die angegebene Datenmenge verarbeitet wird.

Bestimmte Features wie etwa der fortlaufende Export stehen nur im Enterprise-Tarif zur Verfügung. Dadurch fallen täglich Gebühren an.

Preisinformationen finden Sie [hier](https://azure.microsoft.com/pricing/details/application-insights/).


## <a name="a-nameq14awhat-does-application-insights-modify-in-my-project"></a><a name="q14"></a>Welche Änderungen nimmt Application Insights in meinem Projekt vor?
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

## <a name="a-nameupdateahow-can-i-change-which-azure-resource-my-project-sends-data-to"></a><a name="update"></a>Wie kann ich ändern, an welche Azure-Ressource mein Projekt Daten sendet?
Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf `ApplicationInsights.config`. Wählen Sie dann **Application Insights aktualisieren** aus. Sie können die Daten an eine vorhandene oder neue Ressource in Azure senden. Der Assistent ändert den Instrumentationsschlüssel in ApplicationInsights.config. Dadurch wird bestimmt, wohin das Server-SDK die Daten sendet. Wenn Sie "Alle aktualisieren" deaktivieren, wird auch der Anzeigeort des Schlüssels auf Ihren Webseiten geändert.

#### <a name="a-namedataahow-long-is-data-retained-in-the-portal-is-it-secure"></a><a name="data"></a>Wie lange werden Daten im Portal aufbewahrt? Ist Sicherheit gewährleistet?
Informationen hierzu finden Sie unter [Datensammlung, -aufbewahrung und -speicherung in Application Insights][data].

## <a name="logging"></a>Protokollierung
#### <a name="a-namepostahow-do-i-see-post-data-in-diagnostic-search"></a><a name="post"></a>Wie zeige ich POST-Daten in der Diagnosesuche an?
POST-Daten werden nicht automatisch protokolliert, Sie können jedoch einen TrackTrace-Aufruf verwenden und die Daten in den Nachrichtenparameter einfügen. Die Größenbegrenzung hierfür ist höher als bei Zeichenfolgeneigenschaften, Sie können jedoch nicht danach filtern.

## <a name="security"></a>Sicherheit
#### <a name="is-my-data-secure-in-the-portal-how-long-is-it-retained"></a>Sind meine Daten im Portal sicher? Wie lange werden sie aufbewahrt?
Informationen hierzu finden Sie unter [Datenspeicherung und Datenschutz][data].

## <a name="a-nameq17a-have-i-enabled-everything-in-application-insights"></a><a name="q17"></a> Habe ich alles in Application Insights aktiviert?
| Diese Daten sollten angezeigt werden | So erhalten Sie die Daten | Deshalb benötigen Sie die Daten |
| --- | --- | --- |
| Verfügbarkeitsdiagramme |[Webtests](app-insights-monitor-web-app-availability.md) |Information, ob Ihre Web-App verfügbar ist |
| Leistung der Server-App: Antwortzeiten usw. ... |[Fügen Sie Ihrem Projekt Application Insights hinzu](app-insights-asp-net.md) oder [installieren Sie den AI-Statusmonitor auf dem Server](app-insights-monitor-performance-live-website-now.md) (oder schreiben Sie Ihren eigenen Code zum [Nachverfolgung von Aufrufen](app-insights-api-custom-events-metrics.md#track-dependency)) |Erkennen von Leistungsproblemen |
| Telemetriedaten zu Abhängigkeiten |[Installieren des AI-Statusmonitors auf dem Server](app-insights-monitor-performance-live-website-now.md) |Diagnostizieren von Problemen mit Datenbanken oder anderen externen Komponenten |
| Abrufen von Stapelüberwachungen aus Ausnahmen |[Einfügen von TrackException-Aufrufen in Ihren Code](app-insights-search-diagnostic-logs.md#exceptions) (einige werden jedoch automatisch gemeldet) |Erkennen und Diagnostizieren von Ausnahmen |
| Durchsuchen von Ablaufprotokollen |[Hinzufügen eines Protokollierungsadapters](app-insights-search-diagnostic-logs.md) |Diagnostizieren von Ausnahmen und Leistungsproblemen |
| Grundlegende Clientnutzung: Seitenansichten, Sitzungen, ... |[JavaScript-Initialisierer in Webseiten](app-insights-javascript.md) |Nutzungsanalyse |
| Benutzerdefinierte Metriken auf Clients |[Nachverfolgen von Aufrufen in Webseiten](app-insights-api-custom-events-metrics.md) |Verbessern der Benutzerfreundlichkeit |
| Benutzerdefinierte Metriken auf Servern |[Nachverfolgen von Aufrufen auf dem Server](app-insights-api-custom-events-metrics.md) |Business Intelligence |

## <a name="automation"></a>Automation
Sie können [PowerShell-Skripts schreiben](app-insights-powershell.md) , um Application Insights-Ressourcen zu erstellen und zu aktualisieren.

## <a name="more-answers"></a>Weitere Antworten
* [Application Insights-Forum](https://social.msdn.microsoft.com/Forums/vstudio/en-US/home?forum=ApplicationInsights)

<!--Link references-->

[data]: app-insights-data-retention-privacy.md
[platforms]: app-insights-platforms.md
[start]: app-insights-overview.md
[windows]: app-insights-windows-get-started.md



<!--HONumber=Dec16_HO1-->


