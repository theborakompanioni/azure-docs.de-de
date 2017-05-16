---
title: Datenaufbewahrung und -speicherung in Azure Application Insights | Microsoft-Dokumentation
description: Hinweis zur Datenaufbewahrung und Datenschutzrichtlinie
services: application-insights
documentationcenter: 
author: alancameronwills
manager: carmonm
ms.assetid: a6268811-c8df-42b5-8b1b-1d5a7e94cbca
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 04/07/2017
ms.author: cfreeman
ms.translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: d82f27c9a657a97f23b60ade352e4d2a87166233
ms.contentlocale: de-de
ms.lasthandoff: 04/12/2017


---
# <a name="data-collection-retention-and-storage-in-application-insights"></a>Datensammlung, -aufbewahrung und -speicherung in Application Insights


Wenn Sie das [Visual Studio Application Insights-SDK][start] in Ihrer App installieren, werden Telemetriedaten für Ihre App an die Cloud gesendet. Als Entwickler möchten Sie natürlich genau wissen, welche Daten gesendet werden, was mit den Daten geschieht und wie Sie die Kontrolle darüber behalten. Dabei ist insbesondere interessant, ob womöglich sensible Daten übermittelt werden, wo sie gespeichert werden und wie sicher die Daten sind. 

Zunächst die kurze Antwort:

* Bei Verwendung der Standardtelemetriemodule in der vorkonfigurierten Form ist es äußerst unwahrscheinlich, dass sensible Daten an den Dienst übermittelt werden. Bei der Telemetrie dreht sich alles um Lade-, Leistungs- und Nutzungsmetriken sowie um Ausnahmeberichte und andere Diagnosedaten. In den Diagnoseberichten sind als Benutzerdaten hauptsächlich URLs sichtbar. Ihre App sollte jedoch in keinem Fall sensible Daten im Nur-Text-Format in eine URL einfügen.
* Sie können Code schreiben, der zusätzliche benutzerdefinierte Telemetriedaten sendet, die Sie bei der Diagnose und Überwachung der Nutzung unterstützen. (Diese Erweiterbarkeit ist ein großartiges Feature von Application Insights.) Der Code kann theoretisch so geschrieben werden, dass versehentlich auch persönliche und andere sensible Daten einbezogen werden. Falls Ihre Anwendung mit solchen Daten arbeitet, müssen Sie sämtlichen Code, den Sie schreiben, einer gründlichen Prüfung unterziehen.
* Beim Entwickeln und Testen Ihrer App können Sie problemlos überprüfen, was vom SDK gesendet wird. Die Daten erscheinen in den Debugging-Ausgabefenstern von IDE und Browser. 
* Die Daten werden auf [Microsoft Azure](http://azure.com)-Servern in den USA oder in Europa gespeichert. (Ihre App kann jedoch überall ausgeführt werden.) Azure verfügt über [ausgefeilte Sicherheitsprozesse und erfüllt eine breite Palette von Kompatibilitätsstandards](https://azure.microsoft.com/support/trust-center/). Auf die Daten haben nur Sie selbst und Ihr Team Zugriff. Microsoft-Mitarbeiter können nur unter ganz bestimmten Bedingungen und mit Ihrem Wissen eingeschränkten Zugriff auf die Daten erlangen. Die Daten sind bei der Übertragung verschlüsselt, auf den Servern allerdings nicht.

Im Rest dieses Artikels werden die obigen Antworten näher erläutert. Der Artikel ist in sich geschlossen, sodass Sie ihn auch Kollegen zeigen können, die nicht direkt Ihrem Team angehören.

## <a name="what-is-application-insights"></a>Was ist Application Insights?
[Visual Studio Application Insights][start] ist ein Dienst von Microsoft, der Sie beim Optimieren der Leistung und Benutzerfreundlichkeit Ihrer aktiven Anwendung unterstützt. Er überwacht Ihre Anwendung während der gesamten Ausführung – sowohl in der Testphase als auch nach der Veröffentlichung oder Bereitstellung. Application Insights erstellt Diagramme und Tabellen, die beispielsweise Aufschluss darüber geben, zu welchen Tageszeiten das Benutzerinteresse besonders groß ist, wie gut die App reagiert und wie gut sie von externen Diensten versorgt wird, von denen sie unter Umständen abhängig ist. Im Falle von Abstürzen, Fehlern oder Leistungsproblemen können Sie die Telemetriedaten im Detail durchsuchen, um die Fehlerursache zu ermitteln. Darüber hinaus informiert Sie der Dienst per E-Mail, falls sich die Verfügbarkeit oder Leistung Ihrer App ändert.

Diese Funktionen erhalten Sie, indem Sie das Application Insights-SDK in Ihrer Anwendung installieren und es so in den Code integrieren. Während der App-Ausführung überwacht das SDK den Betrieb und sendet Telemetriedaten an den Application Insights-Dienst. Hierbei handelt es sich um einen von [Microsoft Azure](http://azure.com)gehosteten Clouddienst. (Application Insights kann aber nicht nur für in Azure gehostete Anwendungen, sondern für jede beliebige Anwendung verwendet werden.)

![Das SDK in Ihrer App sendet Telemetriedaten an den Application Insights-Dienst.](./media/app-insights-data-retention-privacy/01-scheme.png)

Der Application Insights-Dienst speichert und analysiert die Telemetriedaten. Wenn Sie sich die Analyse ansehen oder die gespeicherten Telemetriedaten durchsuchen möchten, melden Sie sich bei Ihrem Azure-Konto an, und öffnen Sie die Application Insights-Ressource für Ihre Anwendung. Sie können auch anderen Mitgliedern Ihres Teams oder bestimmten Azure-Abonnenten Zugriff auf die Daten gewähren.

Die Daten können vom Application Insights-Dienst exportiert werden (etwa in eine Datenbank oder in externe Tools). Für jedes Tool muss ein spezieller Schlüssel angegeben werden, den Sie über den Dienst erhalten. Der Schlüssel kann bei Bedarf widerrufen werden. 

Application Insights-SDKs stehen für eine Reihe von Anwendungstypen zur Verfügung. Hierzu zählen auf eigenen J2EE- oder ASP.NET-Servern oder in Azure gehostete Webdienste, Webclients (also der innerhalb einer Webseite ausgeführte Code), Desktop-Apps und -Dienste sowie Geräte-Apps (etwa für Windows Phone, iOS und Android). Alle diese Anwendungen senden Telemetriedaten an den gleichen Dienst.

## <a name="what-data-does-it-collect"></a>Welche Daten werden dabei gesammelt?
### <a name="how-is-the-data-is-collected"></a>Wie werden die Daten gesammelt?
Es werden drei Datenquellen verwendet:

* Das SDK, das Sie entweder [bei der Entwicklung](app-insights-asp-net.md) oder [zur Laufzeit](app-insights-monitor-performance-live-website-now.md) in Ihre App integrieren. Für die unterschiedlichen Anwendungstypen stehen jeweils unterschiedliche SDKs zur Verfügung. Darunter ist auch ein [SDK für Webseiten](app-insights-javascript.md), das zusammen mit der Seite in den Browser des Endbenutzers geladen wird.
  
  * Jedes SDK verfügt über eine Reihe von [Modulen](app-insights-configuration-with-applicationinsights-config.md), die mithilfe unterschiedlicher Techniken verschiedene Arten von Telemetriedaten erfassen.
  * Wenn Sie das SDK während der Entwicklung installieren, können Sie zusätzlich zu den Standardmodulen die API des SDK verwenden, um Ihre eigenen Telemetriedaten zu senden. Diese benutzerdefinierten Telemetriedaten können beliebige Daten enthalten, die Sie senden möchten.
* Einige Webserver verfügen auch über Agents, die gemeinsam mit der App ausgeführt werden und Telemetriedaten zu CPU-, Arbeitsspeicher- und Netzwerkauslastung senden. Solche Agents können etwa bei Azure-VMs, Docker-Hosts und [J2EE-Servern](app-insights-java-agent.md) vorhanden sein.
* [Verfügbarkeitstests](app-insights-monitor-web-app-availability.md) werden von Microsoft in regelmäßigen Abständen Anforderungen an Ihre Web-App gesendet. Die Ergebnisse werden dann an den Application Insights-Dienst gesendet.

### <a name="what-kinds-of-data-are-collected"></a>Welche Arten von Daten werden gesammelt?
Die gesammelten Daten lassen sich in folgende Hauptkategorien unterteilen:

* [Webserver-Telemetriedaten](app-insights-asp-net.md) : HTTP-Anforderungen.  URI, Anforderungsverarbeitungsdauer, Antwortcode, Client-IP-Adresse. Sitzungs-ID.
* [Webseiten](app-insights-javascript.md) : Seiten-, Benutzer- und Sitzungszähler. Seitenladezeiten. Ausnahmen. AJAX-Aufrufe.
* Leistungsindikatoren: Arbeitsspeicher-, CPU-, E/A-, Netzwerkauslastung.
* Client- und Serverkontext: Betriebssystem, Gebietsschema, Gerätetyp, Browser, Bildschirmauflösung.
* [Ausnahmen](app-insights-asp-net-exceptions.md) und Abstürze: **Stapelabbilder**, Build-ID, CPU-Typ. 
* [Abhängigkeiten](app-insights-asp-net-dependencies.md) : Aufrufe an externe Dienste wie etwa REST, SQL und AJAX. URI oder Verbindungszeichenfolge, Dauer, Erfolg, Befehl.
* [Verfügbarkeitstests](app-insights-monitor-web-app-availability.md) : Testdauer und Schritte, Antworten.
* [Ablaufverfolgungsprotokolle](app-insights-asp-net-trace-logs.md) und [benutzerdefinierte Telemetriedaten](app-insights-api-custom-events-metrics.md) - **alles, was Sie als Code in Ihre Protokolle oder Telemetrie integrieren**.

[Weitere Details](#data-sent-by-application-insights)

## <a name="how-can-i-verify-whats-being-collected"></a>Wie kann ich überprüfen, was gesammelt wird?
Wenn Sie die App mit Visual Studio entwickeln, führen Sie sie im Debugmodus (F5) aus. Die Telemetriedaten werden im Ausgabefenster angezeigt. Dort können Sie sie kopieren und zur einfachen Überprüfung als JSON formatieren. 

![](./media/app-insights-data-retention-privacy/06-vs.png)

Es gibt auch eine lesbarere Ansicht im Fenster „Diagnose“.

Öffnen Sie bei einer Webseite das Debuggingfenster Ihres Browsers.

![Drücken Sie F12, und öffnen Sie die Registerkarte „Netzwerk“.](./media/app-insights-data-retention-privacy/08-browser.png)

### <a name="can-i-write-code-to-filter-the-telemetry-before-it-is-sent"></a>Kann ich Code schreiben, um die Telemetriedaten vor dem Senden zu filtern?
Hierzu können Sie ein [Telemetriedaten-Prozessor-Plug-In](app-insights-api-filtering-sampling.md)schreiben.

## <a name="how-long-is-the-data-kept"></a>Wie lange werden Daten aufbewahrt?
Rohdatenpunkte (also Elemente, die Sie in Analytics abfragen und in Search überprüfen können) werden bis zu 90 Tage lang aufbewahrt. Wenn Sie Daten länger beibehalten möchten, können Sie sie mit dem [fortlaufenden Export](app-insights-export-telemetry.md) in ein Speicherkonto kopieren.

Aggregierte Daten (d.h. Zählungen, Mittelwerte und andere statistischen Daten, die im Metrik-Explorer angezeigt werden) werden im Maß von 1 Minute für 90 Tage aufbewahrt.

## <a name="who-can-access-the-data"></a>Wer kann auf die Daten zugreifen?
Die Daten sind für Sie und, wenn Sie über ein Unternehmenskonto verfügen, für die Teammitglieder sichtbar. 

Sie und Ihre Teammitglieder können die Daten exportieren und an andere Speicherorte kopieren oder Sie an andere Personen weiterleiten.

#### <a name="what-does-microsoft-do-with-the-information-my-app-sends-to-application-insights"></a>Wie verwendet Microsoft die Daten, die meine App an Application Insights sendet?
Microsoft verwendet die Daten nur dazu, Ihnen den Dienst bereitstellen zu können.

## <a name="where-is-the-data-held"></a>Wo werden die Daten gespeichert?
* In den USA oder in Europa. Sie können den Speicherort auswählen, wenn Sie eine neue Application Insights-Ressource erstellen. 


#### <a name="does-that-mean-my-app-has-to-be-hosted-in-the-usa-or-europe"></a>Bedeutet dies, dass meine App in den USA oder in Europa gehostet werden muss?
* Nein. Die Anwendung kann überall ausgeführt werden – auf Ihren eigenen lokalen Hosts oder in der Cloud.

## <a name="how-secure-is-my-data"></a>Wie sicher sind meine Daten?
Application Insights ist ein Azure-Dienst. Sicherheitsrichtlinien werden im Whitepaper [Azure Security, Privacy, and Compliance (Azure-Sicherheit, -Datenschutz und -Kompatibilität)](http://go.microsoft.com/fwlink/?linkid=392408) beschrieben.

Die Daten werden auf Microsoft Azure-Servern gespeichert. Für Konten im Azure-Portal sind die Kontobeschränkungen im Dokument [Trusted Cloud: Microsoft Azure Security, Privacy, and Compliance](http://go.microsoft.com/fwlink/?linkid=392408)(in englischer Sprache) beschrieben.

Der Zugriff auf Ihre Daten durch Microsoft-Mitarbeiter ist eingeschränkt. Wir werden nur mit Ihrer Erlaubnis und ausschließlich dann auf Ihre Daten zugreifen, wenn es erforderlich ist, Sie bei der Verwendung von Application Insights zu unterstützen. 

Aggregierte Daten in den Anwendungen unserer Kunden (z. B. Datenraten und die durchschnittliche Größe von Ablaufverfolgungen) werden zur Verbesserung von Application Insights verwendet.

#### <a name="could-someone-elses-telemetry-interfere-with-my-application-insights-data"></a>Könnte die Telemetrie eines anderen Benutzers meine Application Insights-Daten beeinträchtigen?
Sie könnten zusätzliche Telemetrie mit dem Instrumentationsschlüssel im Code Ihrer Webseiten an Ihr Konto senden. Bei genügend zusätzlichen Daten würden die Metriken die Leistung und Nutzung Ihrer App nicht ordnungsgemäß darstellen.

Wenn Sie Code für andere Projekte freigeben, sollten Sie Ihren Instrumentationsschlüssel entfernen.

## <a name="is-the-data-encrypted"></a>Werden die Daten verschlüsselt?
Derzeit nicht innerhalb der Server.

Alle Daten werden bei der Übertragung zwischen Datencentern verschlüsselt.

#### <a name="is-the-data-encrypted-in-transit-from-my-application-to-application-insights-servers"></a>Werden die Daten während der Übertragung von meiner Anwendung zu Application Insights-Servern verschlüsselt?
Ja, wir verwenden HTTPS zum Senden von Daten an das Portal aus nahezu allen SDKs, darunter Webserver, Geräte und HTTPS-Webseiten. Die einzige Ausnahme sind Daten, die von einfachen HTTP-Webseiten gesendet werden. 

## <a name="personally-identifiable-information"></a>Personenbezogene Informationen
#### <a name="could-personally-identifiable-information-pii-be-sent-to-application-insights"></a>Können personenbezogene Informationen an Application Insights gesendet werden?
Ja, das ist möglich. 

Allgemeine Hinweise:

* Der Großteil der standardmäßigen Telemetrie (d. h. Telemetrie, die ohne das Schreiben von Code gesendet wird) enthält keine expliziten personenbezogenen Informationen. Allerdings können möglicherweise Rückschlüsse aus einer Ereignisauflistung auf einzelne Personen gezogen werden.
* Ausnahme- und Ablaufverfolgungsmeldungen können personenbezogene Informationen enthalten.
* Benutzerdefinierte Telemetrie, z. B. Aufrufe wie "TrackEvent", die Sie mithilfe der API oder von Protokollnachverfolgungen in Code schreiben, kann alle von Ihnen ausgewählten Daten enthalten.

Die Tabelle am Ende dieses Dokuments enthält detaillierte Beschreibungen der gesammelten Daten.

#### <a name="am-i-responsible-for-complying-with-laws-and-regulations-in-regard-to-pii"></a>Bin ich für die Einhaltung von Gesetzen und Vorschriften in Bezug auf personenbezogene Informationen verantwortlich?
Ja. Sie müssen sicherstellen, dass bei der Sammlung und Nutzung der Daten die Gesetze und Vorschriften sowie die Microsoft Online Services-Bedingungen eingehalten werden.

Informieren Sie Ihre Kunden angemessen darüber, welche Daten Ihre Anwendung erfasst und wie die Daten verwendet werden.

#### <a name="can-my-users-turn-off-application-insights"></a>Können meine Benutzer Application Insights abschalten?
Nicht direkt. Wir bieten keinen Schalter, mit dem Ihre Benutzer Application Insights abschalten können.

Sie können jedoch eine solche Funktion in Ihrer Anwendung implementieren. Alle SDKs enthalten eine API-Einstellung, mit der die Telemetrie-Erfassung deaktiviert wird. 

#### <a name="my-application-is-unintentionally-collecting-sensitive-information-can-application-insights-scrub-this-data-so-it-isnt-retained"></a>Meine Anwendung erfasst unbeabsichtigt vertrauliche Informationen. Kann Application Insights ein Scrubbing für diese Daten durchführen, damit sie nicht aufbewahrt werden?
Die Daten werden von Application Insights weder gefiltert noch gelöscht. Verwalten Sie die Daten entsprechend, und vermeiden Sie es, solche Daten an Application Insights zu senden.

## <a name="data-sent-by-application-insights"></a>Von Application Insights gesendete Daten
Die SDKs sind je nach Plattform unterschiedlich, und es gibt verschiedene Komponenten, die Sie installieren können. (Weitere Informationen finden Sie unter [Application Insights – Übersicht][start].) Jede Komponente sendet unterschiedliche Daten.

#### <a name="classes-of-data-sent-in-different-scenarios"></a>Klassen von in verschiedenen Szenarien gesendeten Daten
| Aktion | Gesammelte Datenklassen (siehe nächste Tabelle) |
| --- | --- |
| [Hinzufügen des Application Insights SDK zu Ihrem .NET-Webprojekt][greenbrown] |ServerContext<br/>Inferred<br/>Perf counters<br/>Anforderungen<br/>**Exceptions**<br/>Sitzung (Session)<br/>users |
| [Installieren des Statusmonitors auf IIS][redfield] |Abhängigkeiten<br/>ServerContext<br/>Inferred<br/>Perf counters |
| [Hinzufügen des Application Insights SDK zu einer Java-Web-App][java] |ServerContext<br/>Inferred<br/>Anforderung<br/>Sitzung<br/>users |
| [Hinzufügen des JavaScript SDK zur Webseite][client] |ClientContext  <br/>Inferred<br/>Seite<br/>ClientPerf<br/>Ajax |
| [Definieren von Standardeigenschaften][apiproperties] |**Properties** für alle standardmäßigen und benutzerdefinierten Ereignisse |
| [Aufrufen von TrackMetric][api] |Numerische Werte<br/>**Eigenschaften** |
| [Aufrufen von Track*][api] |Ereignisname<br/>**Eigenschaften** |
| [Aufrufen von TrackException][api] |**Exceptions**<br/>Stapelabbild<br/>**Eigenschaften** |
| SDK kann keine Daten sammeln. Beispiel: <br/> – auf Leistungsindikatoren kann nicht zugegriffen werden<br/> – Ausnahme beim Telemetrieinitialisierer |SDK-Diagnose |

Weitere Informationen zu [SDKs für andere Plattformen][platforms] finden Sie in den entsprechenden Dokumenten.

#### <a name="the-classes-of-collected-data"></a>Die Klassen der gesammelten Daten
| Gesammelte Datenklasse | Umfasst (keine vollständige Liste) |
| --- | --- |
| **Properties** |**Alle Daten – bestimmt durch Code** |
| DeviceContext |ID, IP, Gebietsschema, Gerätemodell, Netzwerk, Netzwerktyp, OEM-Name, Bildschirmauflösung, Rolleninstanz, Rollenname, Gerätetyp |
| ClientContext  |Betriebssystem, Gebietsschema, Sprache, Netzwerk, Fensterauflösung |
| Sitzung (Session) |Sitzungs-ID |
| ServerContext |Computername, Gebietsschema, Betriebssystem, Gerät, Benutzersitzung, Benutzerkontext, Vorgang |
| Inferred |Geolocation anhand IP-Adresse, Zeitstempel, Betriebssystem, Browser |
| Metriken |Metrikname und -wert |
| Ereignisse |Ereignisname und -wert |
| PageViews |URL und Seitenname oder Bildschirmname |
| Client perf |URL-/Seitenname, Browserladezeit |
| Ajax |HTTP-Aufrufe von der Webseite an den Server |
| Anforderungen |URL, Dauer, Antwortcode |
| Abhängigkeiten |Typ (SQL, HTTP,...), Verbindungszeichenfolge oder URI Sync/Async, Dauer, Erfolg, SQL-Anweisung (mit Statusmonitor) |
| **Exceptions** |Typ, **Meldung**, Aufrufliste, Quelldatei und Zeilennummer, Thread-ID |
| Crashes |Prozess-ID, übergeordnete Vorgangs-ID, Thread-ID des Absturzes, Anwendungs-Patch, ID, Build, Typ der Ausnahme, Adresse, Ursache; abgeblendete Symbole und Registerkarten, binäre Start- und Endadressen, binärer Name und Pfad, CPU-Typ |
| Trace |**Meldung** und Schweregrad |
| Perf counters |Prozessorzeit, verfügbarer Speicher, Anforderungsrate, Ausnahmerate, private Bytes verarbeiten, E/A-Rate, Anforderungsdauer, Länge der Anforderungswarteschlange |
| Verfügbarkeit |Webtestantwortcode, Dauer der einzelnen Testschritte, Testname, Zeitstempel, Erfolg, Antwortzeit, Testverzeichnis |
| SDK-Diagnose |Ablaufverfolgungsmeldung oder Ausnahme |

Sie können [einige der Daten durch Bearbeiten von ApplicationInsights.config abschalten][config].

## <a name="credits"></a>Guthaben
Dieses Produkt enthält GeoLite2-Daten, die von MaxMind erstellt wurden und unter [http://www.maxmind.com](http://www.maxmind.com)verfügbar sind.



<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiproperties]: app-insights-api-custom-events-metrics.md#properties
[client]: app-insights-javascript.md
[config]: app-insights-configuration-with-applicationinsights-config.md
[greenbrown]: app-insights-asp-net.md
[java]: app-insights-java-get-started.md
[platforms]: app-insights-platforms.md
[pricing]: http://azure.microsoft.com/pricing/details/application-insights/
[redfield]: app-insights-monitor-performance-live-website-now.md
[start]: app-insights-overview.md


