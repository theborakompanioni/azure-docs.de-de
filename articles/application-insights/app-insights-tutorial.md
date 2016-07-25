<properties 
	pageTitle="Application Insights – Tutorial | Microsoft Azure" 
	description="Verfolgen Sie die Nutzung und Leistung Ihrer Live-Webanwendung. Erkennen, Selektieren und Diagnostizieren Sie Probleme. Überwachen und verbessern Sie fortlaufend die Beliebtheit bei den Benutzern." 
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
	ms.date="07/11/2016" 
	ms.author="awills"/>
 
# Application Insights – Tutorial

[Visual Studio Application Insights](app-insights-get-started.md) ist ein erweiterbarer Analysedienst, der Ihre Live-Webanwendungen überwacht. Mit diesem Dienst können Sie Leistungsprobleme erkennen und diagnostizieren, und Sie können nachvollziehen, wie die Benutzer Ihre App nutzen. Der Dienst ist für Entwickler konzipiert und unterstützt Sie dabei, die Leistung und Benutzerfreundlichkeit Ihrer App kontinuierlich zu verbessern. Er lässt sich für Apps auf einer Vielzahl von Plattformen einsetzen, wie z.B. .NET, Node.js oder J2EE – lokal gehostet oder in der Cloud.

![Erstellen Sie ein Diagramm mit der Statistik zur Benutzeraktivität, oder führen Sie eine Detailsuche bei bestimmten Ereignissen aus.](./media/app-insights-overview/00-sample.png)


Starten Sie, indem Sie entweder [das SDK während der Entwicklung zu Ihrem Code hinzufügen](app-insights-asp-net.md) oder [Ihre App zur Laufzeit instrumentieren](app-insights-monitor-performance-live-website-now.md).

Von der Anwendung gesammelte Telemetriedaten werden im [Azure-Portal](https://portal.azure.com/) gespeichert und analysiert, wo intuitive Ansichten und leistungsstarke Tools für eine schnelle Diagnose und Analyse zur Verfügung stehen.


[Sehen Sie sich die Animation zur Einführung an](https://www.youtube.com/watch?v=fX2NtGrh-Y0).


## So funktioniert's

Sie installieren ein kleines Instrumentierungspaket in Ihrer Anwendung und richten eine Application Insights-Ressource im Microsoft Azure-Portal ein. Die Instrumentierung überwacht Ihre App und sendet Telemetriedaten an das Portal. Das Portal zeigt Ihnen statistische Diagramme und bietet leistungsfähige Suchtools, mit denen Sie Probleme diagnostizieren können.

![Die Application Insights-Instrumentierung in Ihrer App sendet Telemetriedaten an Ihre Application Insights-Ressource im Azure-Portal.](./media/app-insights-overview/01-scheme.png)


Application Insights bietet mehrere [standardmäßige Instrumentierungsmodule](app-insights-configuration-with-applicationinsights-config.md), die verschiedene Arten von Telemetriedaten sammeln. Es gibt z.B. ein Modul, das die Antwort Ihrer Web-App auf HTTP-Anforderungen überwacht, ein anderes, das Monitorausnahmen überwacht, und viele weitere zur Überwachung von Abhängigkeiten, Leistungsindikatoren usw.

Wenn Sie Ihren Code zur Entwicklungszeit instrumentieren, können Sie auch [Ihren eigenen benutzerdefinierten Code](app-insights-api-custom-events-metrics.md) zum Senden von Telemetriedaten an das Portal schreiben. Benutzerdefinierte Telemetrie ist besonders nützlich für die Ablaufverfolgung von User Storys: Sie können Ereignisse zählen wie z. B. Anfragen, Verkaufsereignisse oder das Erreichen bestimmter Ziele.

### Clientseitige Überwachung

Darüber hinaus gibt es die [clientseitige Instrumentierung](app-insights-javascript.md), die in Form eines kleinen Skripts in jede Webseite eingefügt wird. Telemetriedaten werden direkt vom Browser des Endbenutzers an das Azure-Portal gesendet. Diese Form der Instrumentierung überwacht Seitenladezeiten, AJAX-Aufrufe und Skriptausnahmen. Sie können auch benutzerdefinierten Code schreiben, um Benutzeraktionen wie Schaltflächenklicks und andere Ereignisse nachzuverfolgen.

Die clientseitige Überwachung kann zusätzlich zur serverseitigen Überwachung oder unabhängig davon eingesetzt werden.


### Wie sieht der Aufwand aus?

Die Auswirkungen auf die Leistung Ihrer App sind sehr gering. Aufrufe zur Nachverfolgung sind nicht blockierend und werden zusammengefasst und in einem separaten Thread gesendet.

## Erhalten eines Azure-Abonnements

Application Insights ist einer der vielen in Microsoft Azure gehosteten Dienste, und Telemetriedaten werden zur Analyse und Darstellung an Azure gesendet. Als Erstes benötigen Sie also ein Abonnement für [Microsoft Azure](http://azure.com). Die Registrierung ist kostenlos, und Sie können den kostenlosen [Tarif](https://azure.microsoft.com/pricing/details/application-insights/)von Application Insights auswählen.


## Erste Schritte

Es gibt zwei primäre Methoden, um mit der Überwachung Ihrer Web-App zu beginnen. Wählen Sie die Methode aus, die sich am besten für Sie eignet.

|Wenn|Vorgehensweise|Ergebnis
|---|---|---
| Entwicklung |Fügen Sie das SDK zum Code hinzu (auf dem Server und/oder dem Client).| Sie überwachen Antwortzeiten und Fehlerraten und diagnostizieren Probleme während der Entwicklungs-, Test- und Produktionsphase.<br/>Schreiben Sie Code, um benutzerdefinierte Ablaufverfolgungs- und Diagnosefunktionen hinzuzufügen. 
|Laufzeit |Führen Sie den Agent auf einem Host aus (lokal oder in Azure; J2EE oder ASP.NET). | Sie überwachen Antwortzeiten und Fehlerraten und diagnostizieren Problemen während der Laufzeit. <br/>Zugriff auf den Code ist nicht erforderlich.

Die beiden Methoden ergänzen sich gegenseitig. Verwenden Sie beide, um eine vollständige Telemetrielösung zu erhalten.

### Beginnen mit der Laufzeitmethode

In vielerlei Hinsicht ist das Instrumentieren der App zur Laufzeit die einfachste Option. Sie ändern den Code nicht, und Sie können die Methode auch einrichten, nachdem die App bereitgestellt wurde. (Sie können zu einem späteren Zeitpunkt zur Option „Entwicklungszeit“ wechseln.)

Diese Option eignet sich hervorragend für folgende Szenarien:

* Ihre App erfordert keine benutzerdefinierte Telemetrie. Sie sind mit den standardmäßigen Antwortzeiten für Anforderungen und Abhängigkeiten sowie mit Fehlerraten, Ausnahmeberichten und Leistungsindikatoren zufrieden.
* Eine ausgeführte App weist einige Problem auf, und Sie benötigen schnell Diagnosedaten.

**Auf welchem Server wird Ihre App ausgeführt?**

* **IIS lokal oder auf einem virtuellen Computer**: [Installieren Sie den Application Insights-Statusmonitor](app-insights-monitor-performance-live-website-now.md) auf Ihrem Server. Sie benötigen Administratorzugriff.
* **Azure-Web-App oder virtueller Azure-Computer**: Wählen Sie in der Systemsteuerung für die App die [Application Insights-Leistungsüberwachung](app-insights-monitor-performance-live-website-now.md#if-your-app-runs-as-an-azure-web-app) aus.
* **Java**: [Installieren Sie den Java-Agent](app-insights-java-live.md) auf Ihrem Server.

Mit Ausnahme der Java-Option richten diese Tools die erforderliche Application Insights-Ressource ein, in der Ihre Telemetriedaten angezeigt werden.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, wechseln Sie zur Application Insights-Übersicht für Ihre App, und Sie sehen, wie die Daten eintreffen.

Wenn Sie zu einem späteren Zeitpunkt [benutzerdefinierte Telemetrie](app-insights-api-custom-events-metrics.md) hinzufügen möchten, können Sie problemlos die Methode „Beginnen während der Entwicklung“ verwenden.

### Beginnen während der Entwicklung

Um Application Insights optimal zu nutzen, fügen Sie Ihrem Code während der Entwicklung das geeignete Application Insights SDK hinzu.

Das Hinzufügen des SDK ermöglicht Folgendes:

* Abrufen von Diagnosedaten währen der Entwicklungs- und Testphase
* Abrufen von Stapelüberwachungsdaten in Ausnahmeberichten
* Anpassen Ihrer Telemetrie: Schreiben Sie eigenen Code, um Ereignisse und Metriken zu zählen und nachzuverfolgen – sowohl zu Geschäfts- als auch zu Diagnosezwecken.

**Welche Entwicklungstools verwenden Sie?**

* **ASP.NET on Visual Studio** 2013 Update 2 oder höher: Verwenden Sie den Menübefehl [zum Hinzufügen von Application Insights zu Ihrem Webprojekt](app-insights-asp-net.md). Wenn Sie ein neues Projekt erstellen, können Sie auch einfach die Option „Application Insights“ aktivieren.
* **Java in Eclipse**: [Verwenden Sie das Add-In für Eclipse](app-insights-java-eclipse.md).
* **Java** mit anderen Tools: [Installieren Sie das Java SDK](app-insights-java-get-started.md).
* **Andere Plattformen**: [Installieren Sie das entsprechende SDK](app-insights-platforms.md).


![Hinzufügen von Application Insights zu einem vorhandenen Projekt in Visual Studio](./media/app-insights-tutorial/appinsights-03-addExisting.png)

In den meisten Fällen fügen die Tools das SDK zu Ihrem Projekt hinzu, erstellen eine Ressource im Application Insights-Portal und verknüpfen SDK und Ressource miteinander. Bei Bedarf können Sie jedoch [manuell eine neue Ressource in Azure erstellen](app-insights-create-new-resource.md).

### Instrumentieren der Webseiten

Sie können Ihre Webseiten jederzeit instrumentieren, auch wenn eine Instrumentierung des Servercodes nicht möglich ist.

Damit erhalten Sie Informationen zur Anzahl von Benutzern, Sitzungen und Seitenaufrufen, zu Leistungsdaten beim Laden von Seiten, zu Antwortzeiten für AJAX-Aufrufe und zu Fehlerraten. Sie können auch Code schreiben, um andere Ereignisse wie Schaltflächenklicks und Auswahlereignisse nachzuverfolgen. Dies ist besonders nützlich für Einzelseiten-Web-Apps.

* **Webseiten**: [Fügen Sie Ihren Codeausschnitt in Ihre Webseiten ein](app-insights-javascript.md).

## Erkunden von Metriken

Führen Sie Ihre App aus – entweder im Debugmodus auf dem Entwicklungscomputer oder durch Bereitstellen auf einem Server –, und verwenden Sie sie eine Weile. Melden Sie sich dann beim [Azure-Portal](https://portal.azure.com) an.

Wechseln Sie zum Application Insights-Übersichtsblatt Ihrer App:

![Das Blatt „Übersicht“](./media/app-insights-tutorial/01-find.png)

In dieser Übersicht erhalten Sie sofort Informationen zur Leistung Ihrer App. Sie können die Last (in Form der Anforderungsrate) mit der Zeit vergleichen, die Ihre App für die Antwort auf die Anforderungen benötigt hat. Wenn die Antwortzeit bei Ansteigen der Last unverhältnismäßig zunimmt, sollten Sie Ihrer App mehr Ressourcen zuweisen. Wenn direkt nach der Bereitstellung eines neuen Builds mehr fehlerhafte Antworten angezeigt werden, sollten Sie ein Rollback durchführen.


#### Abrufen weiterer Details

Klicken Sie auf ein beliebiges Diagramm, um einen Satz ausführlicherer Diagramme anzuzeigen. Beispiel: Das Diagramm für die Serverantwortzeit führt zu Diagrammen, die Anforderungsraten, Antwortzeiten und Antwortzeiten von Abhängigkeiten (also Dienste, die von Ihrer App aufgerufen werden) anzeigen.

![Das Blatt „Server“](./media/app-insights-tutorial/04.png)

Das Diagramm mit den Abhängigkeiten ist nützlich, da Sie darin sehen können, ob die von Ihrer App verwendeten Datenbanken und REST-APIs schnell reagieren oder Verzögerungen verursachen.

#### Anpassen eines Diagramms

Versuchen Sie, eines dieser Diagramme bearbeiten. Wenn beispielsweise Ihre Web-App auf einer Sammlung von Serverinstanzen ausgeführt wird, können Sie die Antwortzeiten der verschiedenen Serverinstanzen vergleichen:

![Bearbeiten des Diagramms](./media/app-insights-tutorial/05.png)

1. Zeigen Sie auf ein Diagramm, und klicken Sie auf „Bearbeiten“.
2. Wählen Sie eine Metrik aus. In einem Diagramm können mehrere Metriken angezeigt werden, allerdings nur in bestimmten Kombinationen – möglicherweise müssen Sie die Auswahl einer Metrik aufheben, bevor Sie die gewünschte Metrik auswählen können.
3. Verwenden Sie die Option „Gruppieren nach“, um eine Metrik anhand einer Eigenschaft zu segmentieren. In diesem Beispiel werden verschiedene Antwortzeiten durch verschiedene Linien angezeigt.

    Beachten Sie, dass Sie eine gültige Eigenschaft für die Metrik auswählen müssen, sonst zeigt das Diagramm gar keine Daten an.
4. Wählen Sie einen Diagrammtyp aus. Flächen- und Balkendiagramme zeigen eine gestapelte Ansicht, die sich für den Aggregationstyp „Summe“ eignet.

[Weitere Informationen zum Erkunden von Metriken](app-insights-metrics-explorer.md).

## Durchsuchen von Instanzdaten

Um ein Problem zu untersuchen, ist es nützlich, bestimmte Ereignisinstanzen zu überprüfen.

Klicken Sie auf ein Metrikdiagramm, um mithilfe von relevanten Filtern und basierend auf verschiedenen Zeiträumen die Instanzdaten zu durchsuchen. Klicken Sie beispielsweise auf die Anzahl von Serveranforderungen, um die einzelnen Anforderungsberichte anzuzeigen.

Sie können auch direkt über die Übersichtsseite des Search-Diensts auf die Instanzdaten zugreifen:

![Search-Instanzen](./media/app-insights-tutorial/06.png)

Verwenden Sie Filter, um bestimmte Arten von Ereignissen und ausgewählte Eigenschaftswerte anzuzeigen:

![Filtern nach Eigenschaften](./media/app-insights-tutorial/07.png)

Klicken Sie auf „...“, um eine vollständige Liste der Eigenschaften anzuzeigen, oder öffnen Sie andere Ereignisse, die mit diesem Ereignis verknüpft sind. In diesem Beispiel weist die fehlerhafte Anforderung einen Bericht mit zugehörigen Ausnahmen auf:

![Zugehörige Elemente und Eigenschaftsdetails](./media/app-insights-tutorial/08.png)

Öffnen Sie ein Ereignis – in diesem Fall die zugehörige Ausnahme. Nun können Sie eine Arbeitsaufgabe erstellen (wenn Sie Visual Studio Team Services zum Nachverfolgen von Aufgaben verwenden).
 
![Erstellen einer Arbeitsaufgabe](./media/app-insights-tutorial/09.png)

## Analyse

[Analytics](app-insights-analytics.md) ist eine noch leistungsfähigere Such- und Analysefunktion, in dem Sie SQL-ähnliche Abfragen für Ihre Telemetriedaten schreiben können, um nach bestimmten Problemen zu suchen oder statistische Informationen zusammenzustellen.

![Analyse](./media/app-insights-tutorial/10.png)

Öffnen Sie das Tutorialfenster, um Beispiele zu Abfragen Ihrer Daten anzuzeigen und auszuführen, oder lesen Sie die längere [Einführung in Analytics in Application Insights](app-insights-analytics-tour.md). IntelliSense zeigt die Abfragen an, die Sie verwenden können, und es ist eine [vollständige Sprachreferenz](app-insights-analytics-reference.md) vorhanden.

Abfragen beginnen in der Regel mit dem Namen eines Telemetriedatenstroms – z.B. Anforderungen, Ausnahmen oder Abhängigkeiten. Öffnen Sie die Popup-Schemaleiste auf der linken Seite, um eine Liste der verfügbaren Telemetriedatenströme anzuzeigen. Die Abfrage ist eine Pipeline aus [Abfragevorgängen](app-insights-analytics-reference.md#queries-and-operators) wie z.B. `where` – ein boolescher Filter – oder `project`, der neue Eigenschaften berechnet. `summarize` [aggregiert Instanzen](app-insights-analytics-tour.md#aggregate-groups-of-rows), gruppiert sie nach von Ihnen definierten Funktionen und wendet dann Aggregationsfunktionen auf die gruppierten Daten an.

Die Ergebnisse können [in Tabellen oder verschiedenen Arten von Diagrammen dargestellt werden](app-insights-analytics-tour.md#charting-the-results).

## Benutzerdefinierte Telemetrie

Die integrierte Telemetrie, die Sie bereits durch die Installation von Application Insights abrufen, ermöglicht die Analyse von Zählern, Erfolgsraten und Antwortzeiten sowohl für die Webanforderungen an Ihre App als auch für Abhängigkeiten, also Aufrufe von SQL aus Ihrer App oder REST-APIs. Sie rufen auch Ausnahmeablaufverfolgungen und – über den Statusmonitor auf Ihrem Server – Systemleistungsindikatoren ab. Wenn Sie den Clientcodeausschnitt zu Ihren Webseiten hinzufügen, rufen Sie Zähler und Ladezeiten für Seitenaufrufe, Clientausnahmen und Erfolgs- und Antwortraten von AJAX-Aufrufen ab.

Durch die Analyse all dieser Telemetriedaten erfahren Sie sehr viel über die Leistung und Nutzung Ihrer App. Manchmal ist dies jedoch nicht ausreichend. Möglicherweise möchten Sie die Länge einer Warteschlange überwachen, um die Leistung zu optimieren, oder Sie möchten Verkaufsereignisse zählen und diese nach Standort segmentieren. Vielleicht möchten Sie auch für Clients herausfinden, wie häufig Benutzer auf eine bestimmte Schaltfläche klicken, sodass Sie die Benutzeroberfläche optimieren können.

Die [Application Insights-API](app-insights-api-custom-events-metrics.md) bietet die Aufrufe `TrackEvent(name)` und `TrackMetric(name, value)`, sodass Sie Ihre eigenen benutzerdefinierten Ereignisse und Metriken senden können. Es gibt entsprechende Aufrufe für die Clientseite.

Wenn es sich bei Ihrer Webseite beispielsweise um eine Einzelseiten-Spiele-App handelt, können Sie an geeigneten Stellen Zeilen einfügen, um einen Protokolleintrag zu erstellen, wenn ein Spieler gewinnt oder verliert:

    
    appInsights.trackEvent("WinGame");
    ...
    appInsights.trackEvent("LoseGame");

Dann können Sie benutzerdefinierte Ereigniszähler als Diagramm darstellen und sie nach Ereignisnamen segmentieren:

![Analyse](./media/app-insights-tutorial/11.png)

### Protokollablaufverfolgungen

Zu Diagnosezwecken gibt es das benutzerdefinierte Ereignis `TrackTrace(message)`, das Sie für Ausführungsablaufverfolgungen verwenden können. In den Features Search und Analytics können Sie im Inhalt der Meldung suchen, der länger als ein Ereignisname sein kann.

Wenn Sie bereits ein Protokollierungsframework wie z.B. Log4Net, NLog, Log4J oder System.Diagnostic.Trace verwenden, können diese Ablaufverfolgungsaufrufe von Application Insights erfasst werden und werden neben den weiteren Telemetriedaten angezeigt. Die Visual Studio-Tools fügen automatisch das geeignete SDK-Modul hinzu.

## Dashboards

Viele Anwendungen bestehen aus verschiedenen Komponenten, z.B. einem Webdienst und mehreren Back-End-Prozessoren. Jede Komponente wird durch eine separate Application Insights-Ressource überwacht. Wenn Ihr System in Azure ausgeführt wird, verwenden – und überwachen – Sie möglicherweise auch Dienste wie Event Hubs und Machine Learning.

Um das gesamte System zu überwachen, können Sie die interessantesten Diagramme aus verschiedenen Apps auswählen und sie an ein Azure-[Dashboard](app-insights-dashboards.md) anheften, sodass Sie das gesamte System ununterbrochen im Blick behalten können.

![Dashboards](./media/app-insights-tutorial/12.png)

Sie können mehrere Dashboards erstellen: z.B. ein Dashboard für den Teamraum, um die allgemeine Systemintegrität zu überwachen; ein Entwurfsdashboard, das die Nutzung verschiedener Features anzeigt; ein separates Dashboard für Komponenten im Test; und viele weitere mehr.

Dashboards können ebenso wie Ressourcen zur gemeinsamen Nutzung durch mehrere Teammitglieder freigegeben werden.

## Entwicklung in Visual Studio

Wenn Sie Visual Studio zum Entwickeln Ihrer App verwenden, stehen Ihnen verschiedene integrierte Application Insights-Tools zur Verfügung.



### Diagnosesuche

Im Suchfenster werden die protokollierten Ereignisse angezeigt. (Wenn Sie sich bei der Einrichtung von Application Insights bei Azure angemeldet haben, können Sie die gleichen Ereignisse im Portal durchsuchen.)

![Klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie „Application Insights > Durchsuchen“ aus.](./media/app-insights-visual-studio/34.png)

Die Freitextsuche funktioniert in allen Feldern in den Ereignissen. Suchen Sie z. B. einen Teil der URL einer Seite, den Wert einer Eigenschaft, z. B. Ort des Kunden, oder bestimmte Wörter in einem Ablaufverfolgungsprotokoll.

Klicken Sie auf ein beliebiges Ereignis, um seine detaillierten Eigenschaften anzuzeigen.

Sie können zum Diagnostizieren von fehlerhaften Anforderungen oder von Ausnahmen auch die Registerkarte „Verwandte Elemente“ öffnen.


![](./media/app-insights-visual-studio/41.png)



### Diagnosehub

Der Diagnosehub zeigt (ab Visual Studio 2015) die von Application Insights generierten Servertelemetriedaten. Dies funktioniert auch, wenn Sie sich entschieden haben, nur das SDK zu installieren, ohne es mit einer Ressource im Azure-Portal zu verbinden.

![Öffnen Sie das Fenster „Diagnosetools“, und überprüfen Sie die Application Insights-Ereignisse.](./media/app-insights-visual-studio/31.png)


### Ausnahmen

Wenn Sie die [Ausnahmeüberwachung eingerichtet haben](app-insights-asp-net-exceptions.md), werden im Search-Fenster Ausnahmeberichte angezeigt.

Klicken Sie auf eine Ausnahme, um eine Stapelüberwachung zu erhalten. Wenn der Code der App in Visual Studio geöffnet ist, können Sie sich durch die Stapelüberwachung bis zur entsprechenden Zeile im Code klicken.


![Ausnahmestapelüberwachung](./media/app-insights-visual-studio/17.png)

Außerdem wird in der Codebereichszeile oberhalb jeder Methode die Anzahl von Ausnahmen angezeigt, die von Application Insights innerhalb der letzten 24 Stunden protokolliert wurden.

![Ausnahmestapelüberwachung](./media/app-insights-visual-studio/21.png)


### Lokale Überwachung



(In Visual Studio 2015 Update 2) Falls Sie das SDK nicht zum Senden von Telemetriedaten an das Application Insights-Portal konfiguriert haben (sodass kein Instrumentationsschlüssel in „ApplicationInsights.config“ enthalten ist), werden im Diagnosefenster Telemetriedaten aus der letzten Debugsitzung angezeigt.

Dies ist wünschenswert, wenn Sie bereits eine frühere Version der Anwendung veröffentlicht haben. Die Telemetriedaten aus den Debugsitzungen und die Telemetriedaten der veröffentlichten App im Application Insights-Portal müssen getrennt behandelt werden.

Dies ist ebenfalls hilfreich, wenn Sie über [benutzerdefinierte Telemetriedaten](app-insights-api-custom-events-metrics.md) verfügen, die Sie vor dem Senden an das Portal debuggen möchten.


* *Zunächst habe ich Application Insights vollständig für das Senden von Telemetriedaten an das Portal konfiguriert. Aber nun möchte ich die Telemetriedaten nur in Visual Studio anzeigen.*

 * In den Einstellungen des Suchfensters steht eine Option zum Durchsuchen der lokalen Diagnosen zur Verfügung, auch wenn Ihre App Telemetriedaten an das Portal sendet.
 * Damit keine Telemetriedaten mehr an das Portal gesendet werden, kommentieren Sie die Zeile `<instrumentationkey>...` in „ApplicationInsights.config“ aus. Wenn Sie bereit sind, Telemetriedaten erneut an das Portal zu senden, heben Sie die Auskommentierung auf.

## Trends

Mit dem Tool Trends können Sie visualisieren, wie sich Ihre App im Laufe der Zeit verhält.

Wählen Sie auf der Application Insights-Symbolleiste oder im Application Insights-Suchfenster die Schaltfläche **Telemetrietrends untersuchen**. Wählen Sie eine der fünf allgemeinen Abfragen aus, um zu beginnen. Sie können unterschiedliche Datasets basierend auf Telemetrietypen, Zeiträumen und anderen Eigenschaften analysieren.

Wählen Sie zum Ermitteln von Anomalien in Ihren Daten in der Dropdownliste „Ansichtstyp“ eine Anomalieoption. Mit den Filteroptionen am unteren Rand des Fensters ist es einfach, bestimmte Teilmengen Ihrer Telemetriedaten anzuzeigen.

![Trends](./media/app-insights-visual-studio/51.png)


## Freigeben eines neuen Builds

### Live Metrics Stream

Live Metrics Stream zeigt Ihnen Ihre Anwendungsmetriken für den aktuellen Zeitpunkt an – nahezu in Echtzeit, mit einer Latenz von 1 Sekunde. Dies ist äußerst nützlich, wenn Sie einen neuen Build freigeben und sicherstellen möchten, dass alles wie erwartet funktioniert, oder wenn Sie einen Incident in Echtzeit untersuchen möchten.

![Klicken Sie auf dem Blatt „Übersicht“ auf „Livedatenstrom“.](./media/app-insights-metrics-explorer/live-stream.png)

Im Gegensatz zum Metrik-Explorer zeigt Live Metrics Stream einen festen Satz von Metriken an. Die Daten werden nur so lange beibehalten, wie sie im Diagramm angezeigt werden. Anschließend werden sie verworfen.

### Anmerkungen

Die [Versionsanmerkungen](app-insights-annotations.md) in den Metrikdiagrammen geben an, wo Sie einen neuen Build bereitgestellt haben. Dank dieser Anmerkungen sehen Sie auf einen Blick, ob Ihre Änderungen Auswirkungen auf die Leistung Ihrer Anwendung hatten. Die Anmerkungen können automatisch vom [Buildsystem von Visual Studio Team Services](https://www.visualstudio.com/de-DE/get-started/build/build-your-app-vs) erstellt werden, und Sie können sie auch [mit PowerShell erstellen](#create-annotations-from-powershell).

![Beispiel für Anmerkungen mit sichtbarer Korrelation mit Serverantwortzeit](./media/app-insights-annotations/00.png)

Versionsanmerkungen sind ein Feature des cloudbasierten Build- und Versionsdiensts von Visual Studio Team Services.

## Warnungen

Wenn bei Ihrer App ein Problem auftritt, sollten Sie sofort darüber informiert werden.

Application Insights bietet drei Arten von per E-Mail übermittelten Warnungen.

### Proaktive Diagnose 

[Proaktive Diagnosen](app-insights-nrt-proactive-diagnostics.md)** werden automatisch eingerichtet. Sie müssen diese Art der Warnung nicht konfigurieren. Sofern Ihre Website genügend Datenverkehr aufweist, erhalten Sie eine E-Mail, wenn die Anzahl von fehlerhaften Anforderungen in einer Weise steigt, die für die Tageszeit oder Anforderungsrate ungewöhnlich ist. Die Warnung enthält Diagnoseinformationen.

Hier sehen Sie eine Beispielwarnung:

![Beispiel für eine intelligente Warnung mit Clusteranalyse im Umfeld des Fehlers](./media/app-insights-nrt-proactive-diagnostics/010.png)

Eine zweite Art der proaktiven Erkennung ermittelt Korrelationen zwischen Fehlern und Faktoren wie beispielsweise Standort, Clientbetriebssystem oder Browsertyp.

### Metrikwarnungen

Sie können [Metrikwarnungen](app-insights-alerts.md) konfigurieren, die Sie informieren, wenn eine Metrik für einen bestimmten Zeitraum einen Schwellenwert überschreitet – z.B. Fehleranzahl, Arbeitsspeicher oder Seitenaufrufe.

![Wählen Sie im Metrik-Explorer "Warnungsregeln", "Warnung hinzufügen"](./media/app-insights-metrics-explorer/appinsights-413setMetricAlert.png)

### Availability

[Verfügbarkeitswebtests](app-insights-monitor-web-app-availability.md) senden von unseren Servern an verschiedenen Standorten weltweit Anforderungen an Ihre Website. Sie informieren Sie, wenn Ihre Website im Internet nicht verfügbar ist oder langsam reagiert.

![Beispiel für Webtest](./media/app-insights-monitor-web-app-availability/appinsights-10webtestresult.png)

## Export

Es gibt verschiedene Methoden, um Ihre Telemetriedaten aus dem Application Insights-Portal abzurufen:

* Der [fortlaufende Export](app-insights-export-telemetry.md) ist ideal, wenn Sie große Teile Ihrer Telemetriedaten länger beibehalten möchten als in der standardmäßigen Aufbewahrungsdauer vorgesehen.
* Mit der [Export-Schaltfläche](app-insights-metrics-explorer.md#export-to-excel) im oberen Bereich eines Blatts mit Metriken oder Suchergebnissen können Sie Tabellen und Diagramme in ein Excel-Arbeitsblatt übertragen.
* [Analytics](app-insights-analytics.md) bietet eine leistungsstarke Abfragesprache für Telemetriedaten sowie die Möglichkeit, Ergebnisse zu exportieren.
* Die Data Access-REST-API kann verwendet werden, um Daten zu suchen und zu extrahieren. Die API kann Analytics-Abfragen ausführen.
* Wenn Sie [Ihre Daten in Power BI untersuchen](http://blogs.msdn.com/b/powerbi/archive/2015/11/04/explore-your-application-insights-data-with-power-bi.aspx) möchten, benötigen Sie dazu keinen fortlaufenden Export.
 
## Datenverwaltung

Die Verwendung von Application Insights weist einige Einschränkungen auf, die zu einem gewissen Teil von dem von Ihnen gewählten Tarif abhängen. Die sind die wichtigsten Einschränkungen:

* Telemetrierate pro Minute
* Anzahl der Datenpunkte pro Monat
* Aufbewahrungsdauer für Daten

[Sampling](app-insights-sampling.md) ist ein Mechanismus, mit dem sich Kosten senken und Drosselungen vermeiden lassen. Bei diesem Mechanismus wird eine repräsentative Stichprobe behalten und der Rest der Telemetrie verworfen. Zugehörige Elemente (wie Ausnahmen und die Anforderungen, die diese verursacht haben) werden entweder beibehalten oder ebenfalls verworfen. Bei ASP.NET-Anwendungen erfolgt das Sampling automatisch und wird in der App angewendet. Sie können den Mechanismus auch so einrichten, dass er während der Erfassung der Daten im Portal angewendet wird.

## Nächste Schritte

Beginnen mit der Laufzeitmethode mit:

* [IIS-Server](app-insights-monitor-performance-live-website-now.md)
* [J2EE-Server](app-insights-java-live.md)

Beginnen mit der Entwicklungszeitmethode mit:

* [ASP.NET](app-insights-asp-net.md)
* [Java](app-insights-java-get-started.md)
* [Node.js](app-insights-nodejs.md)

## Support und Feedback

* Fragen und Probleme:
 * [Problembehandlung][qna]
 * [MSDN-Forum](https://social.msdn.microsoft.com/Forums/vstudio/home?forum=ApplicationInsights)
 * [StackOverflow](http://stackoverflow.com/questions/tagged/ms-application-insights)
* Vorschläge:
 * [UserVoice](https://visualstudio.uservoice.com/forums/357324)
* Blog:
 * [Application Insights-Blog](https://azure.microsoft.com/blog/tag/application-insights)




## Videos

[![Animierte Einführung](./media/app-insights-overview/video-front-1.png)](https://www.youtube.com/watch?v=fX2NtGrh-Y0)

> [AZURE.VIDEO 218]

> [AZURE.VIDEO usage-monitoring-application-insights]

> [AZURE.VIDEO performance-monitoring-application-insights]

> [Einführungsanimation](https://www.youtube.com/watch?v=fX2NtGrh-Y0)


<!--Link references-->

[android]: https://github.com/Microsoft/ApplicationInsights-Android
[azure]: ../insights-perf-analytics.md
[client]: app-insights-javascript.md
[desktop]: app-insights-windows-desktop.md
[detect]: app-insights-detect-triage-diagnose.md
[greenbrown]: app-insights-asp-net.md
[ios]: https://github.com/Microsoft/ApplicationInsights-iOS
[java]: app-insights-java-get-started.md
[knowUsers]: app-insights-overview-usage.md
[platforms]: app-insights-platforms.md
[portal]: http://portal.azure.com/
[qna]: app-insights-troubleshoot-faq.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[windows]: app-insights-windows-get-started.md

 

<!---HONumber=AcomDC_0713_2016-->