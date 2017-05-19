---
title: Problembehandlung bei Application Insights in einem Java-Webprojekt
description: "Handbuch zur Problembehandlung – Überwachen von Live-Java-Apps mit Application Insights"
services: application-insights
documentationcenter: java
author: CFreemanwa
manager: carmonm
ms.assetid: ef602767-18f2-44d2-b7ef-42b404edd0e9
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/16/2016
ms.author: cfreeman
ms.translationtype: Human Translation
ms.sourcegitcommit: 4fc4561516490b9b285220e7ae688bf97384fe6e
ms.openlocfilehash: 9164a93308a4a2f3eed4ff6262942239df029dae
ms.contentlocale: de-de
ms.lasthandoff: 02/11/2017


---
# <a name="troubleshooting-and-q-and-a-for-application-insights-for-java"></a>Anleitung zur Problembehandlung sowie Fragen und Antworten zu Application Insights für Java
Haben Sie Fragen oder Probleme im Zusammenhang mit [Azure Application Insights in Java][java]? Hier sind einige Tipps.

## <a name="build-errors"></a>Buildfehler
**Wenn ich in Eclipse das Application Insights-SDK über Maven oder Gradle hinzufüge, erhalte ich Build- oder Prüfsummenvalidierungsfehler.**

* Wenn das Abhängigkeitselement <version> ein Muster mit Platzhalterzeichen verwendet (z. B.  (Maven) `<version>[1.0,)</version>` oder (Gradle) `version:'1.0.+'`), geben Sie stattdessen eine bestimmte Version wie `1.0.2` an. Weitere Informationen finden Sie in den [Versionshinweisen](https://github.com/Microsoft/ApplicationInsights-Java#release-notes) für die aktuelle Version.

## <a name="no-data"></a>Keine Daten
**Ich habe Application Insights erfolgreich hinzugefügt und meine Anwendung ausgeführt, sehe aber keine Daten im Portal.**

* Warten Sie eine Minute, und klicken Sie auf "Aktualisieren". Die Diagramme aktualisieren sich in regelmäßigen Abständen selbst, können aber auch manuell aktualisiert werden. Das Aktualisierungsintervall hängt vom Zeitbereich des Diagramms ab.
* Prüfen Sie, ob Sie in der Datei "ApplicationInsights.xml" (im Ordner "Ressourcen" Ihres Projekts) einen Instrumentationsschlüssel definiert haben.
* Stellen Sie sicher, dass es in der XML-Datei keinen Knoten `<DisableTelemetry>true</DisableTelemetry>` gibt.
* In Ihrer Firewall müssen Sie möglicherweise die TCP-Ports 80 und 443 für ausgehenden Datenverkehr zu „dc.services.visualstudio.com“ öffnen. Weitere Informationen finden Sie in den [hier](app-insights-ip-addresses.md)
* Schauen Sie sich auf der Startseite von Microsoft Azure die Dienststatusübersicht an. Falls es eine Warnungsanzeige gibt, warten Sie, bis sie wieder "OK" anzeigt, und schließen Sie das Application Insights-Anwendungsfenster, bevor Sie es erneut öffnen.
* Aktivieren Sie die Protokollierung im IDE-Konsolenfenster durch Hinzufügen eines `<SDKLogger />` -Elements unter dem Stammknoten in der Datei "ApplicationInsights.xml" (im Ordner "Ressourcen" Ihres Projekts), und suchen Sie nach Einträgen, denen [Fehler] vorangestellt ist.
* Stellen Sie sicher, dass die richtige Datei "ApplicationInsights.xml" vom Java-SDK geladen wurde, indem Sie die von der Konsole ausgegebenen Meldungen auf den Hinweis untersuchen, dass die Konfigurationsdatei gefunden wurde.
* Wenn die Konfigurationsdatei nicht gefunden wird, stellen Sie anhand der Ausgabemeldungen fest, wo nach der Konfigurationsdatei gesucht wird, und stellen Sie sicher, dass sich die Datei "ApplicationInsights.xml" an einem dieser durchsuchten Speicherorte befindet. Als Faustregel können Sie die Konfigurationsdatei bei den JAR-Dateien des Application Insights-SDK ablegen. Beispiel: In Tomcat wäre dies der Ordner "WEB-INF/lib".

#### <a name="i-used-to-see-data-but-it-has-stopped"></a>Zuvor wurden Daten angezeigt, jetzt jedoch nicht mehr.
* Überprüfen Sie den [Statusblog](http://blogs.msdn.com/b/applicationinsights-status/).
* Ist Ihr monatliches Kontingent an Datenpunkten erreicht? Öffnen Sie "Einstellungen – Kontingente und Preisübersicht", um es herauszufinden. Sie können in diesem Fall Ihren Plan aktualisieren oder zusätzliche Kapazität erwerben. Informationen hierzu finden Sie in der [Preisübersicht](https://azure.microsoft.com/pricing/details/application-insights/).

#### <a name="i-dont-see-all-the-data-im-expecting"></a>Nicht alle Daten werden erwartungsgemäß angezeigt.
* Öffnen Sie das Blatt „Quotas and Pricing“ (Kontingente und Preise), und überprüfen Sie, ob ein [Sampling](app-insights-sampling.md) durchgeführt wird. (Eine Übertragung von 100 % bedeutet, dass kein Sampling durchgeführt wird.) Der Application Insights-Dienst kann für die Übernahmen nur eines Bruchteils der Telemetriedaten, die von Ihrer App empfangen werden, konfiguriert werden. So können Sie sicherstellen, dass Sie Ihr monatliches Kontingent an Telemetriedaten nicht überschreiten. 

## <a name="no-usage-data"></a>Keine Nutzungsdaten
**Ich sehe Daten zu Anforderungen und Antwortzeiten, aber keine Seitenzugriffs-, Browser- oder Benutzerdaten.**

Sie haben Ihre App erfolgreich so eingerichtet, dass Telemetriedaten vom Server gesendet werden. Jetzt ist der nächste Schritt das [Einrichten Ihrer Webseiten für das Senden von Telemetrie aus dem Webbrowser][usage].

Wenn der Client eine App auf einem [Smartphone oder anderen Gerät][platforms] ist, können Sie Telemetriedaten von dort aus senden. 

Verwenden Sie den gleichen Instrumentationsschlüssel zum Einrichten der Telemetrie auf Client und Server. Die Daten werden in der gleichen Application Insights-Ressource angezeigt und können Ereignisse von Client und Server korrelieren.


## <a name="disabling-telemetry"></a>Deaktivieren der Telemetrie
**Wie kann ich die Telemetrieerfassung deaktivieren?**

Im Code:

```Java

    TelemetryConfiguration config = TelemetryConfiguration.getActive();
    config.setTrackingIsDisabled(true);
```

**Oder** 

Aktualisieren Sie "ApplicationInsights.xml" (im Ordner "Ressourcen" in Ihrem Projekt). Fügen Sie unter dem Stammknoten Folgendes hinzu:

```XML

    <DisableTelemetry>true</DisableTelemetry>
```

Mithilfe der XML-Methode müssen Sie die Anwendung neu starten, nachdem Sie den Wert geändert haben.

## <a name="changing-the-target"></a>Ändern des Ziels
**Wie kann ich ändern, an welche Azure-Ressource mein Projekt Daten sendet?**

* [Rufen Sie den Instrumentierungsschlüssel der neuen Ressource ab.][java]
* Wenn Sie Ihrem Projekt Application Insights mit dem Azure-Toolkit für Eclipse hinzugefügt haben, klicken Sie mit der rechten Maustaste auf das Webprojekt. Wählen Sie erst **Azure** und dann **Application Insights konfigurieren** aus, und ändern Sie den Schlüssel.
* Aktualisieren Sie den Schlüssel andernfalls in "ApplicationInsights.xml" im Ordner "Ressourcen" in Ihrem Projekt.

## <a name="debug-data-from-the-sdk"></a>Daten-Debug aus SDK

**Wie finde ich heraus, was das SDK ausführt?**

Um weitere Informationen zu API-Vorgängen zu erhalten, fügen Sie `<SDKLogger/>` unter dem Stammknoten der ApplicationInsights.xml-Konfigurationsdatei hinzu.

Sie können das Protokollierungstool auch anweisen, Dateien auszugeben:

```XML

    <SDKLogger type="File">
      <enabled>True</enabled>
      <uniqueprefix>JavaSDKLog</uniqueprefix>
    </SDKLogger>
```

Diese Dateien finden Sie unter `%temp%\javasdklogs`.


## <a name="the-azure-start-screen"></a>Der Azure-Startbildschirm
**Ich sehe mir das [Azure-Portal](https://portal.azure.com) an. Teilt mir die Karte etwas über meine App mit?**

Nein, sie zeigt die Integrität der Azure-Server auf der ganzen Welt.

*Wie finde ich im Azure-Startmenü (Startbildschirm) Daten über meine App?*

Wenn Sie [Ihre App für Application Insights eingerichtet haben][java], klicken Sie auf „Durchsuchen“. Wählen Sie „Application Insights“ und dann die App-Ressource aus, die Sie für Ihre App erstellt haben. Um in Zukunft schneller dorthin zu gelangen, können Sie Ihre App an die Startseite anheften.

## <a name="intranet-servers"></a>Intranetserver
**Kann ich einen Server in meinem Intranet überwachen?**

Ja, sofern Ihr Server über das öffentliche Internet Telemetriedaten an das Application Insights-Portal senden kann. 

In Ihrer Firewall müssen Sie möglicherweise die TCP-Ports 80 und 443 für ausgehenden Datenverkehr zu "dc.services.visualstudio.com" und "f5.services.visualstudio.com" öffnen.

## <a name="data-retention"></a>Beibehaltung von Daten
**Wie lange werden Daten im Portal aufbewahrt? Ist Sicherheit gewährleistet?**

Informationen hierzu finden Sie unter [Datenspeicherung und Datenschutz][data].

## <a name="next-steps"></a>Nächste Schritte
**Ich habe Application Insights für meine Java-Server-App eingerichtet. Was kann ich sonst noch tun?**

* [Überwachen der Verfügbarkeit von Webseiten][availability]
* [Überwachen der Nutzung von Webseiten][usage]
* [Nachverfolgen der Nutzung und Diagnostizieren von Problemen in Ihren Geräte-Apps][platforms]
* [Schreiben von Code zum Nachverfolgen der Verwendung Ihrer App][track]
* [Erfassen von Diagnoseprotokollen][javalogs]

## <a name="get-help"></a>Hier erhalten Sie Hilfe
* [Stapelüberlauf](http://stackoverflow.com/questions/tagged/ms-application-insights)

<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[data]: app-insights-data-retention-privacy.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[platforms]: app-insights-platforms.md
[track]: app-insights-api-custom-events-metrics.md
[usage]: app-insights-javascript.md


