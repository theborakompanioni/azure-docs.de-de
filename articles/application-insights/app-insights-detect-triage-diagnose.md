---
title: "Überblick über Azure Application Insights für DevOps | Microsoft-Dokumentation"
description: In diesem Artikel erfahren Sie, wie Sie Application Insights in einer DevOps-Umgebung verwenden.
author: CFreemanwa
services: application-insights
documentationcenter: 
manager: carmonm
ms.assetid: 6ccab5d4-34c4-4303-9d3b-a0f1b11e6651
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: sewhee
ms.translationtype: Human Translation
ms.sourcegitcommit: 857267f46f6a2d545fc402ebf3a12f21c62ecd21
ms.openlocfilehash: c6bfa094f5f06483a9c59a1e0167e5fa7f8f053e
ms.contentlocale: de-de
ms.lasthandoff: 06/28/2017

---
# <a name="overview-of-application-insights-for-devops"></a>Überblick über Application Insights für DevOps

Mithilfe von [Application Insights](app-insights-overview.md) können Sie schnell die Leistung und Verwendung Ihrer App nach ihrer Liveschaltung ermitteln. Wenn ein Problem vorliegt, informiert Application Insights Sie darüber, sodass Sie die Auswirkungen bewerten und die Ursache ermitteln können.

Hier beschreibt ein Team, das Webanwendungen entwickelt, seine Erfahrungen:

* *"Vor ein paar Tagen haben wir einen 'kleineren' Hotfix bereitgestellt. Wir haben keinen umfassenden Testdurchlauf ausgeführt, doch leider gerieten einige unerwartete Änderungen in die Nutzlast, die eine Inkompatibilität zwischen dem Front-End und dem Back-End verursacht haben. Sofort nahmen die Serverausnahmen zu, unsere Warnungen wurden ausgelöst, und wir wurden auf die Situation aufmerksam gemacht. Nach ein paar Klicks im Application Insights-Portal hatten wir genügend Informationen über die Aufruflisten für Ausnahmen gesammelt, um das Problem einzugrenzen. Wir haben die Änderung umgehend rückgängig gemacht und dadurch den Schaden begrenzt. Dank Application Insights war dieser Teil des DevOps-Zyklus sehr einfach umsetzbar."*

In diesem Artikel begleiten wir ein Team der Fabrikam-Bank bei, das für die Entwicklung des Onlinebankingsystems (OBS) zuständig ist, um herauszufinden, wie sie mithilfe von Application Insights schnell auf Anforderungen von Kunden reagieren und Updates bereitstellen.  

Das Team arbeitet in einem DevOps-Zyklus, das in der folgenden Abbildung dargestellt wird:

![DevOps-Zyklus](./media/app-insights-detect-triage-diagnose/00-devcycle.png)

Anforderungen werden in den Entwicklungs-Backlog (Aufgabenliste) geladen. Sie arbeiten mit kurzen Sprints, in denen häufig Software – in der Regel in Form von Verbesserungen und Erweiterungen der vorhandenen Anwendung – bereitgestellt wird. Die Live-App wird regelmäßig mit neuen Features aktualisiert. Während die App live geschaltet ist, überwacht das Team sie mithilfe von Application Insights in Hinblick auf Leistung und Verwendung. Diese APM-Daten werden zurück in ihr Entwicklungs-Backlog geladen.

Das Team verwendet Application Insights zum genauen Überwachen der Live-Webanwendung in Bezug auf:

* Leistung. Es soll nachvollziehbar sein, wie die Antwortzeiten in Abhängigkeit von der Anforderungsanzahl variieren, wie viele CPU-, Netzwerk-, Datenträgerressourcen und andere Ressourcen verwendet werden und wo Engpässe auftreten.
* Fehler. Wenn Ausnahmen oder Fehler bei Anforderungen auftreten oder ein Leistungsindikator den zulässigen Bereich überschreitet, muss das Team sofort darüber informiert werden, um entsprechende Maßnahmen ergreifen zu können.
* Verwendung. Sobald eine neue Funktion veröffentlicht wird, möchte das Team wissen, inwieweit sie genutzt wird und ob Benutzer Probleme mit dieser Funktion haben.

Konzentrieren wir uns auf den Feedbackteil des Zyklus:

![Erkennung-Eingrenzung-Diagnose](./media/app-insights-detect-triage-diagnose/01-pipe1.png)

## <a name="detect-poor-availability"></a>Erkennen schlechter Verfügbarkeit
Marcela Markova ist leitende Entwicklerin im OBS-Team, und ist für Tests sowie für die Überwachung der Onlineleistung zuständig. Sie richtet mehrere [Verfügbarkeitstests](app-insights-monitor-web-app-availability.md) ein:

* Einen einzelnen URL-Test für die Hauptangebotsseite der App: http://fabrikambank.com/onlinebanking/. Sie legt Kriterien für den HTTP-Code 200 und den Text "Willkommen!" fest. Wenn dieser Test misslingt, ist etwas mit dem Netzwerk oder den Servern ernsthaft nicht in Ordnung, oder es liegt ggf. ein Bereitstellungsproblem vor. (Oder jemand hat ohne ihr Wissen die Begrüßungsmeldung auf der Seite geändert.)
* Einen umfassenden mehrstufigen Test, der eine Anmeldung ausführt, eine aktuelle Kontoauflistung abruft und einige wichtige Details auf jeder Seite überprüft. Dieser Test bestätigt, dass die Verbindung mit der Kontendatenbank funktioniert. Sie verwendet eine fiktive Kunden-ID, von denen einige für Testzwecke vorgehalten werden.

Nach dem Einrichten dieser Tests ist Marcela sicher, dass das Team schnell über Systemausfälle informiert wird.  

Ausfälle bzw. Fehler werden im Diagramm des Webtests als rote Punkte angezeigt:

![Anzeigen von Webtests, die im vorherigen Zeitraum erfolgt sind](./media/app-insights-detect-triage-diagnose/04-webtests.png)

Aber noch wichtiger ist, dass eine Warnung zu allen Fehlern per E-Mail an das Entwicklungsteam gesendet wird. Auf diese Weise weiß es vor fast allen Kunden von dem Problem.

## <a name="monitor-performance"></a>Überwachen der Leistung
Die Übersichtsseite in Application Insights enthält ein Diagramm, das eine Vielzahl [wichtiger Metriken](app-insights-web-monitor-performance.md) zeigt.

![Verschiedene Metriken](./media/app-insights-detect-triage-diagnose/05-perfMetrics.png)

Die Browser-Seitenladezeit wird von Telemetriedaten abgeleitet, die direkt von Webseiten gesendet werden. Die Serverantwortzeit, die Anzahl von Serveranforderungen und die Anzahl der Anforderungsfehler werden auf dem Webserver gemessen und von dort an Application Insights gesendet.

Marcela ist etwas besorgt über das Serverantwortdiagramm. Dieses Diagramm zeigt die Durchschnittszeit, in der ein Server eine HTTP-Anforderung vom Browser eines Benutzers erhält und in der der Server die Antwort zurückgibt. Es ist nicht ungewöhnlich, dass eine Abweichung in diesem Diagramm auftritt, da die Systemauslastung variiert. In diesem Fall existiert aber scheinbar eine Korrelation zwischen geringfügigen Anstiegen in der Anzahl der Anforderungen, und großen Anstiegen in der Antwortzeit. Das könnte darauf hinweisen, dass das System genau an seiner Grenze arbeitet.

Sie öffnet die Serverdiagramme:

![Verschiedene Metriken](./media/app-insights-detect-triage-diagnose/06.png)

Es scheinen keine Anzeichen einer Ressourceneinschränkung vorhanden zu sein, also sind die Unregelmäßigkeiten in den Serverantwort-Diagrammen möglicherweise nur ein Zufall.

## <a name="set-alerts-to-meet-goals"></a>Festlegen von Benachrichtigungen zur Erfüllung von Zielen
Dennoch möchte sie die Antwortzeiten im Auge behalten. Wenn diese zu lang werden, möchte sie sofort darüber informiert sein.

Also richtet sie eine [Warnung](app-insights-metrics-explorer.md) für Antwortzeiten ein, die länger sind als der typische Schwellenwert. Dies gibt ihr Zuversicht, dass Bescheid weiß, wenn Antwortzeiten zu langsam sind.

![Blatt "Warnung hinzufügen"](./media/app-insights-detect-triage-diagnose/07-alerts.png)

Warnungen können für eine Vielzahl anderer Metriken eingerichtet werden. Beispielsweise können Sie E-Mails empfangen, wenn die Anzahl der Ausnahmen hoch wird oder der verfügbare Arbeitsspeicher zur Neige geht oder es zu einer Spitze bei den Clientanforderungen kommt.

## <a name="stay-informed-with-smart-detection-alerts"></a>Benachrichtigung durch Warnungen der intelligenten Erkennung
Am nächsten Tag kommt eine E-Mail mit einer Warnung von Application Insights. Als sie die E-Mail öffnet, erkennt sie, dass dies nicht die Warnung für Antwortzeiten ist, die sie eingerichtet hat. Stattdessen wird ihr mitgeteilt, dass es einen plötzlichen Anstieg an gescheiterten Anforderungen gab. Dies sind Anforderungen, die mindestens 500 Fehlercodes oder mehr zurückgegeben haben.

Anforderungsfehler sind Fälle, bei denen Benutzern ein Fehler angezeigt wurde, meist im Anschluss an eine im Code ausgelöste Ausnahme. Möglicherweise wird eine Meldung wie „Leider konnten wir Ihre Details gerade nicht aktualisieren“ angezeigt. Im absolut schlimmsten Fall kann auch ein Stapeldump auf dem Bildschirm des Benutzers angezeigt werden, der vom Webserver ausgegeben wurde.

Diese Warnung ist eine Überraschung, denn das letzte Mal, als sie die Situation prüfte, war die Anzahl der Anforderungsfehler ermutigend niedrig. Eine kleine Anzahl von Fehlern werden in einem ausgelasteten Server erwartet.

Dies war ebenso eine Überraschung für sie, denn sie musste diese Warnung nicht konfigurieren. Application Insights beinhaltet intelligente Erkennung. Sie passt sich automatisch dem üblichen Fehlermuster Ihrer App an, und „gewöhnt sich“ an Fehler auf einer bestimmten Seite, unter hoher Last oder verknüpft mit anderen Metriken. Es wird nur dann ein Alarm ausgelöst, wenn der Anstieg über einen erwarteten Punkt hinausgeht.

![Proaktive Diagnose-E-Mail](./media/app-insights-detect-triage-diagnose/21.png)

Dies ist eine sehr nützliche E-Mail. Sie löst nicht nur einen Alarm aus. Sie führt auch einen großen Teil der Eingrenzung und Diagnose durch.

Es wird gezeigt, wie viele Kunden, Webseiten oder Vorgänge betroffen sind. Marcela kann entscheiden, ob das gesamte Team als Übung für den Erstfall daran arbeiten soll, oder ob es reicht, diese Arbeit bis zur nächsten Woche aufzuschieben.

Die E-Mail zeigt auch, dass eine bestimmte Ausnahme aufgetreten ist, und dass der Fehler den fehlerhaften Aufrufen auf eine bestimmte Datenbank zugeordnet werden kann, was sogar noch interessanter ist. Dies erklärt, warum der Fehler plötzlich aufgetreten ist, obwohl Marcelas Team in letzter Zeit keine Updates bereitgestellt hat.

Marcela sendet basierend auf dieser E-Mail ein Pingsignal an den Leiter des Datenbankteams. Sie erfährt, dass ein Hotfix in der letzten halben Stunde herausgegeben wurde, und womöglich versehentlich eine geringfügige Schemaänderung durchgeführt worden sein könnte.

Das Problem wird also behoben, noch vor der Untersuchung von Protokollen, und innerhalb von 15 Minuten vor dessen Erscheinen. Marcela klickt trotzdem auf den Link, um Application Insights zu öffnen. Sie wird direkt auf eine fehlgeschlagene Anforderung geleitet und sie kann die fehlerhafte Datenbankanforderung in der Liste der Abhängigkeitsaufrufe sehen.

![Fehlgeschlagene Anforderung](./media/app-insights-detect-triage-diagnose/23.png)

## <a name="detect-exceptions"></a>Erkennen von Ausnahmen
Mit nur wenigen Einrichtungsschritten werden [Ausnahmen](app-insights-asp-net-exceptions.md) automatisch an Application Insights gemeldet. Die Ausnahmen können auch explizit aufgezeichnet werden, indem Aufrufe von [TrackException()](app-insights-api-custom-events-metrics.md#trackexception) in den Code eingefügt werden:  

    var telemetry = new TelemetryClient();
    ...
    try
    { ...
    }
    catch (Exception ex)
    {
       // Set up some properties:
       var properties = new Dictionary <string, string>
         {{"Game", currentGame.Name}};

       var measurements = new Dictionary <string, double>
         {{"Users", currentGame.Users.Count}};

       // Send the exception telemetry:
       telemetry.TrackException(ex, properties, measurements);
    }


Das Team der Fabrikam-Bank hat lässt sich bei einer Ausnahme stets Telemetriedaten senden, es sei denn, es gibt eine offensichtliche Problemlösung.  

Die Strategie des Teams ist sogar noch umfassender: es lässt sich Telemetriedaten in allen Fällen senden, in denen der Kunde nicht den gewünschten Schritt ausführen kann, ob aufgrund einer Ausnahme im Code oder nicht. Wenn z. B. das externe Interbank-Überweisungssystem die Meldung "Diese Transaktion kann nicht abgeschlossen werden" aus bestimmten betrieblichen Gründen (ohne Fehlverhalten des Kunden) zurückgibt, wird auch dieses Ereignis nachverfolgt.

    var successCode = AttemptTransfer(transferAmount, ...);
    if (successCode < 0)
    {
       var properties = new Dictionary <string, string>
            {{ "Code", returnCode, ... }};
       var measurements = new Dictionary <string, double>
         {{"Value", transferAmount}};
       telemetry.TrackEvent("transfer failed", properties, measurements);
    }

TrackException wird verwendet, um Ausnahmen zu melden, da eine Kopie des Stapels gesendet wird. Mit TrackEvent werden andere Ereignisse gemeldet. Sie können alle Eigenschaften anfügen, die für die Diagnose hilfreich sein können.

Ausnahmen und Ereignisse werden auf dem Blatt [Diagnosesuche](app-insights-diagnostic-search.md) angezeigt. Sie können eine Detailsuche durchführen, um die weiteren Eigenschaften und die Stapelüberwachung anzuzeigen.

![Verwenden Sie bei der Diagnosesuche Filter, um bestimmte Datentypen anzeigen](./media/app-insights-detect-triage-diagnose/appinsights-333facets.png)


## <a name="monitor-proactively"></a>Proaktive Überwachung
Marcela sitzt nicht bloß herum und wartet auf Warnungen. Schon bald nach jeder erneuten Bereitstellung befasst sie sich mit [Antwortzeiten](app-insights-web-monitor-performance.md) – sowohl mit der Gesamtanzahl und der Tabelle mit den langsamsten Anforderungen als auch mit der Anzahl der Ausnahmen.  

![Antwortzeitdiagramm und Raster von Serverantwortzeiten.](./media/app-insights-detect-triage-diagnose/09-dependencies.png)

Sie kann die Auswirkung jeder Bereitstellung auf die Leistung bewerten, indem üblicherweise jede Woche mit der Vorwoche verglichen wird. Sobald es eine plötzliche Verschlechterung gibt, wendet sie sich an die entsprechenden Entwickler.

## <a name="triage-issues"></a>Selektieren von Problemen
Die Eingrenzung, d. h. das Bewerten des Schweregrads und Ausmaßes eines Problems, ist der erste Schritt nach der Erkennung. Müssen wir das Team um Mitternacht herbeirufen? Oder kann es bis zur nächsten praktischen Lücke im Rückstand unbehandelt bleiben? Es gibt einige wichtigen Fragen bei der Eingrenzung.

Wie oft tritt das Problem auf? Die Diagramme auf dem Blatt "Übersicht" setzen das Problem in Perspektive. Beispielsweise hat die Fabrikam-Anwendung vier Webtestwarnungen in einer Nacht generiert. Als sich das Team das Diagramm am Morgen ansah, konnte es einige rote Punkte erkennen, wenngleich für die meisten Tests die Zeichen auf grün standen. Nach einer Detailsuche im Verfügbarkeitsdiagramm war klar, dass all diese zwischenzeitlichen Probleme von einem bestimmten Test herrührten. Es handelte sich offensichtlich um ein Netzwerkproblem, das sich auf nur eine Route auswirkte und sich wahrscheinlich von selbst beheben sollte.  

Im Gegensatz dazu ist ein drastischer und stabiler Anstieg im Diagramm der Anzahl von Ausnahmen oder Dauer von Antwortzeiten natürlich etwas, das Grund zur Panik gibt.

Eine nützliche Eingrenzungstaktik ist das eigenständige Ausprobieren. Wenn bei Ihnen das gleiche Problem auftritt, wissen Sie, dass es real ist.

Welcher Anteil der Benutzer ist betroffen? Um eine grobe Antwort zu erhalten, teilen Sie die Fehlerrate durch die Anzahl der Sitzungen.

![Diagramme fehlerhafter Anforderungen und Sitzungen](./media/app-insights-detect-triage-diagnose/10-failureRate.png)

Vergleichen Sie bei langen Antwortzeiten die Tabelle mit den Anforderungen, die die längsten Antwortzeiten aufweisen, mit der Nutzungsfrequenz jeder Seite.

Wie wichtig ist das Szenario einer Blockierung? Wenn es sich um ein funktionales Problem handelt, das eine bestimmte User Story blockiert, wie wichtig ist es wirklich? Wenn Kunden ihre Rechnungen nicht zahlen können, ist es ernsthaft. Wenn sie die Farbeinstellungen ihres Bildschirms nicht ändern können, kann es möglicherweise warten. Die Details eines Ereignisses oder einer Ausnahme bzw. die Identität der langsamen Seite sagen Ihnen, wo Kunden Probleme haben.

## <a name="diagnose-issues"></a>Diagnostizieren von Problemen
Eine Diagnose ist nicht ganz dasselbe wie ein Debugging. Bevor Sie die Ablaufverfolgung im Code starten, müssen Sie eine grobe Vorstellung haben, warum, wo und wann das Problem auftritt.

**Wann tritt es auf?** Die von den Ereignis- und Metrikdiagrammen bereitgestellte Verlaufsansicht vereinfacht das Korrelieren von Auswirkungen mit möglichen Ursachen. Wenn es bei den Antwortzeiten oder Ausnahmeraten zwischenzeitliche Spitzen gibt, untersuchen Sie die Anzahl der Anforderungen. Wenn die Spitze zur selben Zeit auftritt, liegt wahrscheinlich ein Ressourcenproblem vor. Müssen Sie mehr CPU oder Arbeitsspeicher zuweisen? Oder gibt es eine Abhängigkeit, die die Last nicht bewältigen kann?

**Sind wir es?**  Wenn es bei einem bestimmten Typ von Anforderung einen plötzlichen Leistungsabfall gibt, z. B. wenn der Kunde einen Kontoauszug wünscht, besteht die Möglichkeit, dass die Ursache ein externes Subsystem und nicht Ihre Webanwendung ist. Wählen Sie im Metrik-Explorer die Raten "Abhängigkeitsfehler" und "Abhängigkeitsdauer" aus, und vergleichen Sie deren Verlauf in den letzten Stunden oder Tagen mit dem erkannten Problem. Wenn es korrelierende Änderungen gibt, ist vielleicht ein externes Subsystem Schuld.  

![Diagramme von Abhängigkeitsfehlern und Dauer von Aufrufen von Abhängigkeiten](./media/app-insights-detect-triage-diagnose/11-dependencies.png)

Bei einigen Problemen mit langsamen Abhängigkeiten ist die Geolocation die Ursache. Die Fabrikam Bank nutzt virtuelle Computer in Azure und hat festgestellt, dass sich ihre Web- und Kontoserver aus Versehen in unterschiedlichen Ländern befinden. Eine deutliche Verbesserung wurde erreicht, indem einer davon in die richtige Zone verlagert wurde.

**Was haben wir gemacht?** Wenn das Problem nicht einer Abhängigkeit zuzuschreiben ist und nicht immer vorhanden war, wird es wahrscheinlich durch eine zuletzt erfolgte Änderung verursacht. Die von den Metrik- und Ereignisdiagrammen zur Verfügung gestellte Verlaufsansicht erleichtert das Korrelieren plötzlicher Änderungen mit Bereitstellungen Dadurch kann die Suche nach der Problemursache eingeengt werden.

**Was passiert?** Einige Probleme treten nur selten auf und können schwierig aufzuspüren sein, wenn Tests offline erfolgen. Alles, was wir tun können, ist versuchen, den Fehler zu erfassen, wenn er online auftritt. Sie können das Stapelabbild in den Ausnahmeberichten prüfen. Darüber hinaus können Sie Ablaufverfolgungsaufrufe mit Ihrem bevorzugten Protokollierungsframework oder mit TrackTrace() oder TrackEvent() schreiben.  

Bei Fabrikam war ein zwischenzeitliches Problem bei Überweisungen zwischen Konten, jedoch nur bei bestimmten Kontotypen aufgetreten. Um besser zu verstehen, was passiert ist, hat das Team "TrackTrace()"-Aufrufe an wichtigen Punkten im Code eingefügt, wobei der Kontotyp jedem Aufruf als Eigenschaft angefügt wird. Dadurch wurde das Herausfiltern bloß dieser Ablaufverfolgungen bei der Diagnosesuche erleichtert. Außerdem hat das Team Parameterwerte als Eigenschaften und Kennzahlen an die Ablaufverfolgungsaufrufe angefügt.

## <a name="respond-to-discovered-issues"></a>Reagieren auf erkannte Probleme
Nachdem Sie das Problem diagnostiziert haben, arbeiten Sie einen Plan zu seiner Korrektur aus. Vielleicht müssen Sie eine zuletzt erfolgte Änderung rückgängig machen oder können das Probleme ohne diesen Schritt angehen. Nachdem die Korrektur erfolgt ist, informiert Application Insights Sie, ob Sie erfolgreich waren.  

Das Entwicklungsteam der Fabrikam Bank befolgt nun einen strukturierteren Ansatz bei der Leistungsmessung als vor dem Einsatz von Application Insights.

* Es legt auf der Übersichtsseite von Application Insights Leistungsziele in Bezug auf bestimmte Kennzahlen fest.
* Das Team integriert von Anfang an Leistungsmessungen in die Anwendung, z. B. in Form von Metriken, die den Benutzerfortschritt in "Trichtern" messen.  


## <a name="monitor-user-activity"></a>Überwachen der Benutzeraktivität
Wenn die Antwortzeit konsistent gut ist und wenige Ausnahmen auftreten, kann das Entwicklungsteam zur Verwendbarkeit übergehen. Es kann über die Verbesserung der Benutzeroberfläche nachdenken oder darüber, wie die Benutzer besser zum Erreichen der gewünschten Ziele ermuntert werden können.

Application Insights kann auch genutzt werden, um zu erfahren, welche Aktionen Benutzer in einer Anwendung ausführen. Sobald sie reibungslos läuft, möchte das Team wissen, welche Funktionen am häufigsten genutzt werden, was Benutzer mögen, womit sie Schwierigkeiten haben und wie oft sie zurückkehren. Dadurch können die anstehenden Aufgaben mit Prioritäten versehen werden. Und außerdem kann das Team das Messen des Erfolg der einzelnen Features im Rahmen des Entwicklungszyklus planen. 

Der normale Weg eines Benutzers durch die Website weist beispielsweise einen deutlichen „Trichtereffekt“ auf. Viele Kunden sehen sich die Zinsen für verschiedene Arten von Darlehen an. Eine kleinere Anzahl fährt mit dem Ausfüllen des Angebotsformulars fort. Von denen, die ein Angebot anfordern, enden nur wenige dabei, das Darlehen aufzunehmen.

![Anzahl von Seitenaufrufen](./media/app-insights-detect-triage-diagnose/12-funnel.png)

Ein Unternehmen kann nun untersuchen, an welcher Stelle die meisten Kunden den Vorgang abbrechen, und so ermitteln, wie sich mehr Benutzer durch den gesamten Trichter schleusen lassen. In manchen Fällen tritt ein Fehler auf der Benutzeroberfläche auf. Beispielsweise ist die Schaltfläche „Weiter“ schwer zu finden, oder die Anweisungen sind nicht eindeutig. Wahrscheinlicher sind jedoch geschäftliche Gründe (wie etwa zu hohe Zinssätze für Darlehen).

Unabhängig von den Gründen kann das Team mithilfe der Daten ermitteln, welche Aktionen Benutzer ausführen. Es kann weitere Aufrufe zur Nachverfolgung einfügen, um zusätzliche Einzelheiten zu ermitteln. Mithilfe von TrackEvent() können alle Benutzeraktionen (angefangen von einzelnen Klicks auf Schaltflächen bis hin zu bedeutenderen Ereignissen wie etwa dem Abzahlen eines Darlehens) nachverfolgt werden.

Das Team gewöhnt sich daran, Informationen zur Benutzeraktivität zu erhalten. Heute entwickelt das Team beim Entwerfen eines neuen Features ein Feedbackkonzept, um Informationen zur Nutzung zu erhalten. Es arbeitet von Anfang an Aufrufe zur Nachverfolgung in das Feature ein. Es verwendet das Feedback dazu, das Feature in jedem Entwicklungszyklus zu verbessern.

[Weitere Informationen hierzu finden Sie im Abschnitt zum Nachverfolgen der Nutzung](app-insights-usage-overview.md).

## <a name="apply-the-devops-cycle"></a>Anwenden des DevOps-Zyklus
Ein Team kann also Application Insights nicht nur nutzen, um einzelne Probleme zu beheben, sondern auch um seine Softwareentwicklung zu verbessern. Ich hoffe, dass ich Ihnen einige Ideen vermitteln konnte, wie Application Insights Sie bei der Verwaltung der Leistung Ihrer Anwendungen unterstützen kann.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a>Nächste Schritte
Der Einstieg kann auf verschiedene Weise je nach den Eigenschaften Ihrer Anwendung erfolgen. Treffen Sie Ihre Auswahl:

* [ASP.NET-Webanwendung](app-insights-asp-net.md)
* [Java-Webanwendung](app-insights-java-get-started.md)
* [Node.js-Webanwendung](app-insights-nodejs.md)
* Bereits bereitgestellte Apps, die über [IIS](app-insights-monitor-web-app-availability.md), [J2EE](app-insights-java-live.md) oder [Azure](app-insights-azure.md) gehostet werden.
* [Webseiten:](app-insights-javascript.md) Einzelseiten-App oder gewöhnliche Webseite – eigenständig genutzt oder als Ergänzung zu den Serveroptionen.
* [Verfügbarkeitstests:](app-insights-monitor-web-app-availability.md) zum Testen Ihrer App über das öffentliche Internet.

