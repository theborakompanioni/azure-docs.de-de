---
title: "Einrichten der Web-App-Analyse für ASP.NET mit Azure Application Insights | Microsoft-Dokumentation"
description: "Konfigurieren Sie Leistung, Verfügbarkeit und Nutzungsanalyse für Ihre lokal oder in Azure gehostete ASP.NET-Website."
services: application-insights
documentationcenter: .net
author: alancameronwills
manager: carmonm
ms.assetid: d0eee3c0-b328-448f-8123-f478052751db
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/04/2017
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: 6ea03adaabc1cd9e62aa91d4237481d8330704a1
ms.openlocfilehash: 04bac05363f0f7468e41ccfd96a85e4bd7bf761c
ms.lasthandoff: 04/06/2017


---
# <a name="set-up-application-insights-for-your-aspnet-website"></a>Einrichten von Application Insights für Ihre ASP.NET-Website

Mit diesem Verfahren wird Ihre ASP.NET-Web-App so konfiguriert, dass sie Telemetriedaten an den [Azure Application Insights](app-insights-overview.md)-Dienst sendet. Das funktioniert für ASP.NET-Apps, die entweder auf Ihrem eigenen IIS-Server oder in der Cloud gehostet werden. Sie erhalten Diagramme und eine leistungsfähige Abfragesprache, mit deren Hilfe Sie die Leistung Ihrer App sowie deren Verwendung durch die Benutzer nachvollziehen können. Darüber hinaus erhalten Sie automatische Warnungen bei Ausfällen oder Leistungsproblemen. Für viele Entwickler sind diese Features bereits ausreichend, bei Bedarf kann die Telemetrie aber auch noch erweitert und angepasst werden.

Die Einrichtung ist mit wenigen Mausklicks in Visual Studio erledigt. Das Volumen der Telemetriedaten kann eingeschränkt werden, um Kosten zu vermeiden. Dadurch können Sie experimentieren und debuggen oder eine Website mit nur wenigen Benutzern überwachen. Und wenn Sie dann später Ihre Produktionswebsite überwachen möchten, können Sie den Grenzwert problemlos erhöhen.

## <a name="before-you-start"></a>Vorbereitung
Erforderlich:

* Visual Studio 2013, Update 3 oder höher. Eine neuere Version ist besser.
* Ein Abonnement für [Microsoft Azure](http://azure.com). Wenn Ihr Team oder Ihre Organisation über ein Azure-Abonnement verfügt, kann der Besitzer Sie mit Ihrem [Microsoft-Konto](http://live.com) hinzufügen.

Es gibt noch weitere Themen, die für Sie unter Umständen von Interesse sind:

* [Instrumentieren von Web-Apps zur Laufzeit mit Application Insights](app-insights-monitor-performance-live-website-now.md)
* [Azure Cloud Services](app-insights-cloudservices.md)

## <a name="ide"></a> Schritt 1: Hinzufügen des Application Insights SDK

Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf Ihr Web-App-Projekt, und wählen Sie **Hinzufügen** > **Application Insights-Telemetrie...** oder **Application Insights konfigurieren** aus.

![Screenshot des Projektmappen-Explorers mit Hervorhebung von „Application Insights-Telemetrie hinzufügen“](./media/app-insights-asp-net/appinsights-03-addExisting.png)

(Visual Studio 2015 enthält im Dialogfeld „Neues Projekt“ auch eine Option zum Hinzufügen von Application Insights.)

Fahren Sie mit der Seite für die Application Insights-Konfiguration fort:

![Screenshot der Seite „App bei Application Insights registrieren“](./media/app-insights-asp-net/visual-studio-register-dialog.png)

**a.** Wählen Sie das Konto und das Abonnement aus, das Sie zum Zugreifen auf Azure verwenden.

**b.** Wählen Sie die Ressource in Azure aus, über die Sie die Daten Ihrer App anzeigen möchten. Normalerweise erstellen Sie eine separate Ressource für jede App. Wenn Sie die Ressourcengruppe oder den Speicherort Ihrer Daten festlegen möchten, klicken Sie auf **Einstellungen konfigurieren**. Ressourcengruppen werden zum Steuern das Zugriffs auf die Daten verwendet. Wenn Sie über mehrere Apps verfügen, die einen Teil desselben Systems bilden, können Sie die dazugehörigen Application Insights-Daten in derselben Ressourcengruppe anordnen.

**c.** Legen Sie eine Obergrenze auf den Grenzwert für das kostenlose Datenvolumen fest, um Gebühren zu vermeiden. Bis zu einem bestimmten Volumen von Telemetrie ist Application Insights kostenlos. Nach der Erstellung der Ressource können Sie Ihre Auswahl im Portal ändern, indem Sie **Features und Preise** > **Datenverwaltung** > **Volumenbegrenzung pro Tag** öffnen.

**d.** Klicken Sie auf **Registrieren**, um den Prozess zu starten und Application Insights für Ihre Web-App zu konfigurieren. Die Telemetriedaten werden an das [Azure-Portal](https://portal.azure.com) gesendet – sowohl während des Debuggens als auch nach dem Veröffentlichen Ihrer App.

**e.** Wenn beim Debuggen keine Telemetriedaten an das Verwaltungsportal gesendet werden sollen, fügen Sie Ihrer App nur das Application Insights SDK hinzu, konfigurieren Sie aber im Portal keine Ressource. Telemetriedaten werden beim Debuggen in Visual Studio angezeigt. Sie können später zu dieser Konfigurationsseite zurückkehren oder warten, bis Sie Ihre App bereitgestellt haben, und die [Telemetrie zur Laufzeit aktivieren](app-insights-monitor-performance-live-website-now.md).


## <a name="run"></a> Schritt 2: Ausführen der App
Führen Sie Ihre App mit F5 aus. Öffnen Sie verschiedene Seiten, um Telemetriedaten zu generieren.

In Visual Studio wird die Anzahl von protokollierten Ereignissen angezeigt.

![Screenshot von Visual Studio Die Schaltfläche „Application Insights“ wird während des Debuggens angezeigt.](./media/app-insights-asp-net/54.png)

## <a name="step-3-see-your-telemetry"></a>Schritt 3: Anzeigen Ihrer Telemetriedaten
Sie können Ihre Telemetriedaten entweder in Visual Studio oder im Application Insights-Webportal anzeigen. Suchen Sie nach Telemetriedaten in Visual Studio, und nutzen Sie sie beim Debuggen Ihrer App. Überwachen Sie Leistung und Verwendung im Web-Portal, wenn Ihr System aktiv ist. 

### <a name="see-your-telemetry-in-visual-studio"></a>Anzeigen Ihrer Telemetriedaten in Visual Studio

Öffnen Sie in Visual Studio das Fenster „Application Insights“. Klicken Sie entweder auf die Schaltfläche **Application Insights**, oder klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, wählen Sie **Application Insights**, und klicken Sie dann auf **Nach Livetelemetriedaten suchen**.

Im Application Insights-Suchfenster in Visual Studio wird die Ansicht mit den **Daten aus der Debugsitzung** für die Telemetriedaten angezeigt, die auf der Serverseite Ihrer App generiert werden. Experimentieren Sie mit den Filtern, und klicken Sie auf ein beliebiges Ereignis, um weitere Details anzuzeigen.

![Screenshot der Ansicht mit den Daten aus der Debugsitzung im Application Insights-Fenster](./media/app-insights-asp-net/55.png)

> [!NOTE]
> Gehen Sie wie folgt vor, wenn keine Daten angezeigt werden: Stellen Sie sicher, dass der richtige Zeitbereich festgelegt ist, und klicken Sie auf das Suchsymbol.

[Erfahren Sie mehr zu Application Insights-Tools in Visual Studio](app-insights-visual-studio.md).

<a name="monitor"></a>
### <a name="see-telemetry-in-web-portal"></a>Anzeigen von Telemetriedaten im Webportal

Sie können Telemetriedaten auch im Application Insights-Webportal anzeigen (sofern Sie nicht nur das SDK installiert haben). Im Portal stehen mehr Diagramme, Analysetools und komponentenübergreifende Ansichten zur Verfügung als in Visual Studio. Außerdem stellt das Portal Warnungen bereit.

Öffnen Sie die Application Insights-Ressource. Melden Sie sich entweder beim [Azure-Portal](https://portal.azure.com/) an, und suchen Sie dort danach, oder klicken Sie in Visual Studio mit der rechten Maustaste auf das Projekt, um es aufzurufen.

![Screenshot von Visual Studio mit der Vorgehensweise beim Öffnen des Application Insights-Portals](./media/app-insights-asp-net/appinsights-04-openPortal.png)

> [!NOTE]
> Sollte ein Zugriffsfehler auftreten, verfügen Sie unter Umständen über mehrere Microsoft-Anmeldeinformationen und haben sich mit den falschen Informationen angemeldet. Melden Sie sich vom Portal ab und dann wieder an.

Das Portal wird mit einer Ansicht der Telemetriedaten Ihrer App geöffnet.

![Screenshot der Application Insights-Übersichtsseite](./media/app-insights-asp-net/66.png)

Klicken Sie im Portal auf eine beliebige Kachel oder auf ein beliebiges Diagramm, um weitere Details anzuzeigen.

[Erfahren Sie mehr über die Verwendung von Application Insights im Azure-Portal](app-insights-dashboards.md).

## <a name="step-4-publish-your-app"></a>Schritt 4: Veröffentlichen der App
Veröffentlichen Sie Ihre App auf Ihrem IIS-Server oder in Azure. Sehen Sie sich [Live Metrics Stream](app-insights-metrics-explorer.md#live-metrics-stream) an, um sicherzustellen, dass alles reibungslos funktioniert.

Sie können dann verfolgen, wie Ihre Telemetriedaten im Application Insights-Portal erstellt werden. Darin können Sie Metriken überwachen, die Telemetriedaten durchsuchen und [Dashboards](app-insights-dashboards.md) einrichten. Außerdem können Sie die leistungsfähige [Analytics-Abfragesprache](app-insights-analytics.md) verwenden, um die Nutzung und Leistung zu analysieren oder nach bestimmten Ereignissen zu suchen.

Sie können Ihre Telemetriedaten auch in [Visual Studio](app-insights-visual-studio.md) mit Tools wie der Diagnosesuche und [Trends](app-insights-visual-studio-trends.md) weiter analysieren.

> [!NOTE]
> Wenn Ihre App so viele Telemetriedaten sendet, dass die [Einschränkungsgrenzwerte](app-insights-pricing.md#limits-summary) bald erreicht werden, wird das automatische [Sampling](app-insights-sampling.md) aktiviert. Mit dem Sampling wird die Menge der von der App gesendeten Telemetriedaten reduziert, während gleichzeitig korrelierte Daten für Diagnosezwecke beibehalten werden.
>
>

## <a name="land"></a> Fertig

Glückwunsch! Sie haben in Ihrer App das Application Insights-Paket installiert und so konfiguriert, dass Telemetriedaten an den Application Insights-Dienst in Azure gesendet werden.

![Diagramm mit dem Weg der Telemetriedaten](./media/app-insights-asp-net/01-scheme.png)

Die Azure-Ressource, die die Telemetriedaten Ihrer App erhält, wird durch einen *Instrumentierungsschlüssel* angegeben. Diesen Schlüssel finden Sie in der Datei „ApplicationInsights.config“.


## <a name="upgrade-to-future-sdk-versions"></a>Durchführen eines Upgrades auf zukünftige SDK-Versionen
Für ein Upgrade auf eine [neue Version des SDK](https://github.com/Microsoft/ApplicationInsights-dotnet-server/releases) öffnen Sie wieder den **NuGet-Paket-Manager** und filtern die Ansicht nach installierten Paketen. Wählen Sie **Microsoft.ApplicationInsights.Web** und dann **Upgrade** aus.

Wenn Sie Anpassungen an „ApplicationInsights.config“ vorgenommen haben, sollten Sie diese vor dem Upgrade speichern. Übernehmen Sie Ihre Änderungen anschließend für die neue Version.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a>Nächste Schritte

### <a name="more-telemetry"></a>Mehr Telemetrie

* **[Browser- und Seitenladedaten](app-insights-javascript.md)**: Fügen Sie einen Codeausschnitt in Ihre Webseiten ein.
* **[Ausführlichere Abhängigkeits- und Ausnahmenüberwachung](app-insights-monitor-performance-live-website-now.md)**: Installieren Sie den Statusmonitor auf Ihrem Server.
* **[Programmieren benutzerdefinierter Ereignisse](app-insights-api-custom-events-metrics.md)**, um Benutzeraktionen zu zählen, zeitlich abzustimmen oder zu messen.
* **[Abrufen von Protokolldaten](app-insights-asp-net-trace-logs.md)**: Korrelieren Sie Protokolldaten mit Ihren Telemetriedaten.

### <a name="analysis"></a>Analyse

* **[Arbeiten mit Application Insights in Visual Studio](app-insights-visual-studio.md)**<br/>Enthält Informationen zum Debuggen per Telemetrie, zur Diagnosesuche und zum Drillthrough für Code.
* **[Navigation und Dashboards im Application Insights-Portal](app-insights-dashboards.md)**<br/> Enthält Informationen zu Dashboards, leistungsstarken Diagnose- und Analysetools, Warnungen und zum Export von Telemetriedaten sowie eine Livekarte der Abhängigkeiten Ihrer Anwendung.
* **[Analytics](app-insights-analytics-tour.md)**: Die leistungsfähige Abfragesprache.

### <a name="alerts"></a>Warnungen

* [Verfügbarkeitstests](app-insights-monitor-web-app-availability.md): Erstellen Sie Tests, um sicherzustellen, dass Ihre Website im Web sichtbar ist.
* [Intelligente Diagnose](app-insights-proactive-diagnostics.md): Diese Tests werden automatisch ausgeführt, sodass Sie keinerlei Einrichtungsschritte ausführen müssen. Sie werden darüber benachrichtigt, ob für Ihre App eine ungewöhnlich hohe Zahl von Anforderungen mit Fehlern vorliegt.
* [Metrikwarnungen](app-insights-alerts.md): Legen Sie Metrikwarnungen fest, damit Sie gewarnt werden, wenn für eine Metrik ein Schwellenwert überschritten wird. Sie können diese für benutzerdefinierte Metriken festlegen, die Sie in Ihrer App codieren.

### <a name="automation"></a>Automation

* [Automatisieren der Erstellung einer Application Insights-Ressource](app-insights-powershell.md)

