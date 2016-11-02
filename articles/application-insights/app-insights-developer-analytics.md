<properties
	pageTitle="Developer Analytics"
	description="DevOps mit Visual Studio, Application Insights und HockeyApp"
	authors="alancameronwills"
	services="application-insights"
    documentationCenter=""
	manager="douge"/>

<tags
	ms.service="application-insights"
	ms.workload="tbd"
	ms.tgt_pltfrm="ibiza"
	ms.devlang="na"
	ms.topic="article" 
	ms.date="05/18/2016"
	ms.author="awills"/>

# Developer Analytics mit Application Insights und HockeyApp

*Application Insights befindet sich in der Vorschau.*

Viele Projekte werden in einem schnellen [DevOps](https://en.wikipedia.org/wiki/DevOps)-Zyklus ausgeführt. Sie erstellen und verteilen ihre Anwendungen, erhalten Feedback zur Leistung und den Arbeiten, die Benutzer damit erledigen. Dieses Wissen wird dann für die weiteren Entwicklungszyklen verwendet.

Zum Überwachen von Nutzung und Leistung ist es wichtig, Telemetriedaten aus der Liveanwendung sowie Feedback von den Benutzern selbst zu erhalten.

Viele Systeme bestehen aus mehreren Komponenten: einem Webdienst, Back-End-Prozessoren oder -Datenspeichern und Clientsoftware, die im Browser oder als App auf einem Telefon oder einem anderen Gerät des Benutzers ausgeführt wird. Die Telemetriedaten dieser unterschiedlichen Komponenten müssen zusammengeführt werden.

Einige Versionen werden nur an festgelegte Tester verteilt. Wir haben auch Test-Flighting (Tests des neuen Features mit eingeschränkter Zielgruppe) und A|B-Tests (parallele Tests mit unterschiedlichen Benutzeroberflächen) organisiert.

Das Verwalten der Verteilung und das Integrieren der Überwachung über mehrere Client- und Serverkomponenten ist keine einfache Aufgabe. Dieser Prozess ist ein wesentlicher Bestandteil der Architektur der Anwendung: Wir können kein System dieser Art ohne einen iterativen Entwicklungszyklus und gute Tools zur Überwachung erstellen.

In diesem Artikel untersuchen wir, wie die Aspekte zur Überwachung des DevOps-Zyklus mit anderen Teilen des Prozesses zusammenpassen.

Wenn Sie ein konkretes Beispiel betrachten möchten, steht Ihnen [eine interessante Fallstudie](http://aka.ms/mydrivingdocs) mit mehreren Client- und Serverkomponenten zur Verfügung.

## Ein DevOps-Zyklus

Visual Studio und Developer Analytics-Tools bieten alles für einen guten integrierten DevOps-Zyklus. Hier ist z.B. ein typischer Zyklus für eine Webanwendung (Java, Node.js oder ASP.NET):

![Web-App-DevOps-Zyklus](./media/app-insights-developer-analytics/040.png)

* Ein Entwickler checkt Code im Repository ein oder führt diesen im Hauptzweig zusammen. Das Repository in dieser Abbildung ist Git, es könnte aber auch die [Team Foundation-Versionskontrolle](https://www.visualstudio.com/docs/tfvc/overview) sein.
* Die Änderungen lösen einen Build und einen Komponententest aus. Der Builddienst kann Teil von [Visual Studio Team Services oder der lokalen Entsprechung, Team Foundation Server](https://www.visualstudio.com/docs/vsts-tfs-overview) sein.
* Bei dem Build und einem erfolgreichen Komponententest kann eine [automatische Bereitstellung](https://www.visualstudio.com/docs/release/author-release-definition/more-release-definition) ausgelöst werden. Der Web-App-Host kann ein eigener Webserver- oder Microsoft Azure sein.
* Telemetriedaten aus der Live-App wird sowohl vom Server als auch von den [Clientbrowsern](app-insights-javascript.md) an [Application Insights](app-insights-overview.md) gesendet. Dort können Sie die Leistung der App und die Nutzungsmuster analysieren. Leistungsstarke [Suchtools](app-insights-analytics.md) helfen bei der Diagnose von Problemen. [Warnungen](app-insights-alerts.md) stellen sicher, dass Sie von einem Problem erfahren, sowie es auftritt.
* Der nächste Entwicklungszyklus wird über Ihre Analyse der Livetelemetriedaten informiert.

### Geräte- und Desktop-Apps

Für Geräte- und Desktop-Apps ist der Verteilungsaspekt im Zyklus geringfügig anders, da wir nicht nur auf einen oder zwei Server hochladen. Stattdessen kann nach Build und erfolgreichem Komponententest das [Hochladen nach HockeyApp](https://support.hockeyapp.net/kb/third-party-bug-trackers-services-and-webhooks/how-to-use-hockeyapp-with-visual-studio-team-services-vsts-or-team-foundation-server-tfs) ausgelöst werden. HockeyApp überwacht die Verteilung an das Team von Testbenutzern (oder die Öffentlichkeit, falls gewünscht).


![Geräte-App-DevOps-Zyklus](./media/app-insights-developer-analytics/030.png)

HockeyApp sammelt außerdem Leistungs- und Nutzungsdaten in folgender Form:

* Direktes Benutzerfeedback mit Screenshots
* Absturzberichte
* Benutzerdefinierte Telemetriedaten, die von Ihnen codiert wurden.

Wieder ist der DevOps-Zyklus abgeschlossen, wenn Sie Ihre zukünftigen Entwicklungspläne anhand des erhaltenen Feedbacks anpassen.


## Einrichten von Developer Analytics

Für jede Komponente der Anwendung – mobil, Web oder Desktop – sind die Schritte im Grunde identisch. Für viele Arten von Apps führt Visual Studio automatisch einige dieser Schritte durch.

1. Fügen Sie Ihrer App das entsprechende SDK hinzu. Bei Geräte-Apps ist dies HockeyApp, und bei Webdiensten ist es Application Insights. Jedes SDK verfügt über mehrere Varianten für unterschiedliche Plattformen. (Sie können beide SDKs auch für Desktop-Apps verwenden, wir empfehlen jedoch HockeyApp.)
2. Registrieren Sie Ihre App je nach verwendetem SDK bei Application Insights oder im HockeyApp-Portal. Dort werden die Analysen von Ihrer Live-App angezeigt. Sie erhalten einen Instrumentierungsschlüssel oder eine ID für die Konfiguration Ihrer App, damit das SDK weiß, wohin es die Telemetriedaten senden soll.
3. Fügen Sie nach Wunsch benutzerdefinierten Code hinzu, um Ereignisse oder Metriken zu protokollieren, die Ihnen bei der Diagnose oder Analyse der Leistung oder Nutzung helfen. Es sind zahlreiche Überwachungsfunktionen integriert, sodass Sie dies nicht beim ersten Zyklus benötigen.
3. Für Geräte-Apps:
 * Laden Sie ein Debugbuild auf HockeyApp hoch. Von dort aus können Sie es an ein Team von Testbenutzern verteilen. Wenn Sie nachfolgende Builds hochladen, wird das Team jedes Mal benachrichtigt.
 * Beim Einrichten dieses kontinuierlichen Builddiensts erstellen Sie eine Versionsdefinition, die den Plug-In-Schritt zum Hochladen nach HockeyApp verwendet.

### Analytics und Export für HockeyApp-Telemetriedaten

Sie können benutzerdefinierte Telemetriedaten und Protokolltelemetriedaten von HockeyApp mithilfe der Funktionen für Analytics und fortlaufenden Export von Application Insights untersuchen. Dazu müssen Sie eine [Brücke einrichten](app-insights-hockeyapp-bridge-app.md).



## Nächste Schritte
 
Hier finden Sie die ausführliche Anleitung für verschiedene Arten von Apps:

* [ASP.NET-Web-Apps](app-insights-asp-net.md)
* [Java-Web-Apps](app-insights-java-get-started.md)
* [Node.js-Web-Apps](https://github.com/Microsoft/ApplicationInsights-node.js)
* [iOS-Apps](https://support.hockeyapp.net/kb/client-integration-ios-mac-os-x-tvos/hockeyapp-for-ios)
* [Mac OS X-Apps](https://support.hockeyapp.net/kb/client-integration-ios-mac-os-x-tvos/hockeyapp-for-mac-os-x)
* [Android-Apps](https://support.hockeyapp.net/kb/client-integration-android/hockeyapp-for-android-sdk)
* [Universal Windows-Apps](https://support.hockeyapp.net/kb/client-integration-windows-and-windows-phone/how-to-create-an-app-for-uwp)
* [Windows Phone 8- und 8.1-Apps](https://support.hockeyapp.net/kb/client-integration-windows-and-windows-phone/hockeyapp-for-windows-phone-silverlight-apps-80-and-81)
* [Windows Presentation Foundation-Apps](https://support.hockeyapp.net/kb/client-integration-windows-and-windows-phone/hockeyapp-for-windows-wpf-apps)

<!---HONumber=AcomDC_0907_2016-->