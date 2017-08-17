---
title: "Überwachen der Verfügbarkeit und Reaktionsfähigkeit von Websites | Microsoft Docs"
description: "Richten Sie Webtests in Application Insights ein. Erhalten Sie Benachrichtigungen, wenn eine Website nicht mehr zur Verfügung steht oder langsam reagiert."
services: application-insights
documentationcenter: 
author: SoubhagyaDash
manager: carmonm
ms.assetid: 46dc13b4-eb2e-4142-a21c-94a156f760ee
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/25/2017
ms.author: bwren
ms.translationtype: Human Translation
ms.sourcegitcommit: c785ad8dbfa427d69501f5f142ef40a2d3530f9e
ms.openlocfilehash: 74cd4dbf74afbf5e7e0e7d0b7aca49b5aa214b83
ms.contentlocale: de-de
ms.lasthandoff: 05/26/2017

---
# <a name="monitor-availability-and-responsiveness-of-any-web-site"></a>Überwachen der Verfügbarkeit und Reaktionsfähigkeit von Websites
Nachdem Sie die Web-App oder Website an einen beliebigen Server bereitgestellt haben, können Sie Tests einrichten, um die Verfügbarkeit und Reaktionsfähigkeit zu überwachen. [Azure Application Insights](app-insights-overview.md) sendet regelmäßig Webanforderungen von verschiedenen Punkten auf der ganzen Welt an Ihre Anwendung. Sie werden benachrichtigt, wenn Ihre Anwendung langsam oder gar nicht reagiert.

Sie können für jeden HTTP- oder HTTPS-Endpunkt, der über das öffentliche Internet zugänglich ist, Verfügbarkeitstests einrichten. Der zu testenden Website muss nichts hinzugefügt werden. Es muss sich nicht einmal um Ihre Website handeln: Sie können auch einen benötigten REST-API-Dienst testen.

Es gibt zwei Arten von Verfügbarkeitstests:

* [URL-Pingtest](#create): Dies ist ein einfacher Test, den Sie im Azure-Portal erstellen können.
* [Mehrstufiger Webtest](#multi-step-web-tests): Diesen Test erstellen Sie in Visual Studio Enterprise und laden ihn in das Portal hoch.

Sie können bis zu 25 Verfügbarkeitstests pro Anwendungsressource erstellen.

## <a name="create"></a>1. Öffnen einer Ressource für Ihre Verfügbarkeitstestberichte

**Falls Sie Application Insights bereits für Ihre Web-App konfiguriert haben**, können Sie die dazugehörige Application Insights-Ressource im [Azure-Portal](https://portal.azure.com) öffnen.

**Wenn Sie Ihre Berichte in einer neuen Ressource anzeigen möchten**, können Sie sich an [Microsoft Azure](http://azure.com) anmelden, zum [Azure-Portal](https://portal.azure.com) navigieren und eine Application Insights-Ressource erstellen.

![Neu > Application Insights](./media/app-insights-monitor-web-app-availability/11-new-app.png)

Klicken Sie auf **All resources** (Alle Ressourcen), um das Blatt „Übersicht“ für die neue Ressource zu öffnen.

## <a name="setup"></a>2. Erstellen eines URL-Pingtests
Öffnen Sie das Blatt „Verfügbarkeit“, und fügen Sie einen Test hinzu.

![Mindestens die URL der Website eintragen](./media/app-insights-monitor-web-app-availability/13-availability.png)

* **Die URL** kann eine beliebige Webseite sein, die Sie testen möchten, aber sie muss über das öffentliche Internet sichtbar sein. Die URL kann eine Abfragezeichenfolge enthalten. So können Sie beispielsweise Ihre Datenbank abfragen. Wenn die URL in eine Umleitung aufgelöst wird, werden bis zu 10 Umleitungen verfolgt.
* **Abhängige Anforderungen analysieren**: Wenn diese Option aktiviert ist, werden beim Test Bilder, Skripts, Formatdateien und andere Dateien angefordert, die Teil der zu testenden Webseite sind. Die aufgezeichnete Antwortzeit enthält auch die Zeit, die zum Abrufen dieser Dateien erforderlich ist. Der Test schlägt fehl, wenn alle diese Ressourcen innerhalb des Zeitlimits für den gesamten Test nicht erfolgreich heruntergeladen werden können. 

    Wenn die Option nicht aktiviert ist, wird beim Test nur die Datei unter der von Ihnen angegebenen URL angefordert.
* **Enable retries** (Wiederholungen aktivieren): Wenn diese Option aktiviert ist, wird der Test nach kurzer Zeit wiederholt, falls er fehlschlägt. Nur wenn drei aufeinander folgende Versuche scheitern, wird ein Fehler gemeldet. Nachfolgende Tests werden dann in der üblichen Häufigkeit ausgeführt. Die Wiederholung wird bis zum nächsten Erfolg vorübergehend eingestellt. Diese Regel wird an jedem Teststandort unabhängig angewendet. Wir empfehlen Ihnen, diese Option zu verwenden. Im Durchschnitt treten ca. 80% der Fehler bei einer Wiederholung nicht mehr auf.
* **Testhäufigkeit**: Legt fest, wie oft der Test von jedem Teststandort aus ausgeführt wird. Mit einer Frequenz von fünf Minuten und fünf Teststandorten wird Ihre Website im Durchschnitt jede Minute getestet.
* **Teststandorte** sind die Orte, von denen aus unsere Server Webanforderungen an Ihre URL senden. Wählen Sie mehrere aus, damit Sie Probleme mit der Website von Netzwerkproblemen unterscheiden können. Sie können bis zu 16 Standorte auswählen.
* **Erfolgskriterien**:

    **Timeout für Tests**: Reduzieren Sie diesen Wert, um über langsame Antworten benachrichtigt zu werden. Der Test wird als ein Fehler gezählt, wenn die Antworten von Ihrer Website nicht innerhalb dieses Zeitraums empfangen wurden. Bei Auswahl von **Abhängige Anforderungen analysieren**müssen alle Bilder, Styledateien, Skripts und anderen abhängigen Ressourcen innerhalb dieses Zeitraums empfangen werden.

    **HTTP-Antwort**: Der zurückgegebene Statuscode, der als Erfolg gezählt wird. 200 ist der Code, der angibt, dass eine normale Webseite zurückgegeben wurde.

    **Inhaltsübereinstimmung**: Eine Zeichenfolge, zum Beispiel „Willkommen!“ Wir vergewissern uns, dass in jeder Antwort eine exakte Übereinstimmung unter Berücksichtigung der Groß-und Kleinschreibung vorkommt. Dies muss eine Zeichenfolge in Klartext, ohne Platzhalter sein. Vergessen Sie nicht, diese zu aktualisieren, wenn sich der Seiteninhalt ändert.
* **Warnungen** werden standardmäßig an Sie gesendet, wenn innerhalb von fünf Minuten an drei Standorten Fehler auftreten. Bei einem Fehler an einem Standort handelt es sich wahrscheinlich um ein Netzwerkproblem und nicht um ein Problem mit Ihrer Website. Sie können den Schwellenwert auf eine engere oder weitere Überwachung einstellen oder den Empfänger der E-Mails ändern.

    Sie können einen [Webhook](../monitoring-and-diagnostics/insights-webhooks-alerts.md) einrichten, der bei einer Warnung aufgerufen wird. (Beachten Sie aber, dass Abfrageparameter derzeit nicht als Eigenschaften übergeben werden.)

### <a name="test-more-urls"></a>Testen weiterer URLs
Fügen Sie weitere Tests hinzu. Neben dem Testen der Startseite können Sie zum Beispiel auch sicherstellen, dass die Datenbank ausgeführt wird, indem Sie eine Such-URL testen.


## <a name="monitor"></a>3. Anzeigen der Verfügbarkeitstestergebnisse

Klicken Sie nach einigen Minuten auf **Aktualisieren**, um die Testergebnisse anzuzeigen. 

![Ergebnisübersicht im Startblatt](./media/app-insights-monitor-web-app-availability/14-availSummary-3.png)

Das Punktdiagramm zeigt Stichproben der Testergebnisse an, die Diagnosedetails zu Testschritten enthalten. Das Testmodul speichert Diagnosedetails für Tests mit Fehlern. Für erfolgreiche Tests werden Diagnosedetails für eine Teilmenge der Ausführungen gespeichert. Bewegen Sie den Mauszeiger über einen der grünen oder roten Punkte, um Zeitstempel, Dauer, Standort und Name des Tests anzuzeigen. Klicken Sie auf einen Punkt im Punktdiagramm, um die Details des Testergebnisses anzuzeigen.  

Wählen Sie einen bestimmten Test oder Standort aus, oder verringern Sie den Zeitraum, um weitere Ergebnisse um den gewünschten Zeitraum anzuzeigen. Verwenden Sie den Suchexplorer, um Ergebnisse von allen Ausführungen anzuzeigen, oder Analytics-Abfragen, um benutzerdefinierte Berichte für diese Daten auszuführen.

Zusätzlich zu den reinen Ergebnissen gibt es im Metrik-Explorer zwei Verfügbarkeitsmetriken: 

1. Verfügbarkeit: Prozentsatz der erfolgreichen Tests für alle Testausführungen 
2. Testdauer: Durchschnittliche Testdauer für alle Ausführungen

Sie können Filter auf den Testnamen oder Standort anwenden, um Trends eines bestimmten Tests und/oder Standorts zu analysieren.

## <a name="edit"></a> Überprüfen und Bearbeiten der Tests

Wählen Sie auf der Seite „Zusammenfassung“ einen bestimmten Test aus. Auf dieser Seite werden die jeweiligen Ergebnisse angezeigt, und Sie können den Test bearbeiten oder vorübergehend deaktivieren.

![Webtest bearbeiten oder deaktivieren](./media/app-insights-monitor-web-app-availability/19-availEdit-3.png)

Eventuell möchten Sie Verfügbarkeitstests oder die damit verknüpften Warnungsregeln deaktivieren, während Sie Ihren Dienst warten. 

## <a name="failures"></a>Wenn Sie Fehler finden ...
Klicken Sie auf einen roten Punkt.

![Auf einen roten Punkt klicken](./media/app-insights-monitor-web-app-availability/open-instance-3.png)


Mit einem Verfügbarkeitstestergebnis können Sie folgende Aktionen ausführen:

* Untersuchen Sie die vom Server erhaltene Antwort.
* Öffnen Sie die von der Server-App gesendeten Telemetriedaten, während die Instanz der fehlgeschlagenen Anforderung verarbeitet wird.
* Erstellen Sie in Git oder VSTS ein Problem oder eine Arbeitsaufgabe, um das Problem nachzuverfolgen. Der Fehler enthält einen Link zu diesem Ereignis.
* Öffnen Sie das Webtestergebnis in Visual Studio.


*Sieht gut aus, wird jedoch als fehlerhaft gemeldet?* Überprüfen Sie alle Bilder, Skripts, Stylesheets und anderen Dateien, die von der Seite geladen werden. Wenn eines dieser Elemente einen Fehler verursacht, wird der Test auch dann als fehlerhaft gemeldet, wenn die HTML-Hauptseite problemlos geladen wird.

*Keine verwandten Elemente vorhanden?* Wenn Sie Application Insights für Ihre serverseitige Anwendung eingerichtet haben, liegt dies möglicherweise daran, dass [Sampling](app-insights-sampling.md) in Betrieb ist. 

## <a name="multi-step-web-tests"></a>Webtests mit mehreren Schritten
Sie können ein Szenario überwachen, das eine Sequenz mit mehreren URLs umfasst. Wenn Sie zum Beispiel eine Verkaufswebsite überwachen, können Sie testen, ob das Hinzufügen von Artikeln zum Einkaufswagen ordnungsgemäß funktioniert.

> [!NOTE] 
> Für Webtests mit mehreren Schritten wird eine Gebühr erhoben. Informationen hierzu finden Sie in der [Preisübersicht](http://azure.microsoft.com/pricing/details/application-insights/).
> 

Um einen mehrstufigen Test zu erstellen, zeichnen das Szenario mit Visual Studio Enterprise auf. Laden Sie dann die Aufzeichnung in Application Insights hoch. Application Insights wiederholt das Szenario in bestimmten Abständen und überprüft die Antworten.

> [!NOTE]
> Es ist nicht möglich, in Ihren Tests codierte Funktionen oder Schleifen zu verwenden. Der Test muss vollständig im WEBTEST-Skript enthalten sein. Sie können aber Standard-Plug-Ins nutzen.
>

#### <a name="1-record-a-scenario"></a>1. Aufzeichnen eines Szenarios
Verwenden Sie Visual Studio Enterprise, um eine Websitzung aufzuzeichnen.

1. Erstellen Sie ein Webleistungstest-Projekt.

    ![Erstellen Sie in der Visual Studio Enterprise Edition ein Projekt aus der Vorlage für Webleistungs- und Auslastungstests.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-create.png)

 * *Wird die Vorlage für Webleistungs- und Auslastungstests nicht angezeigt?* - Schließen Sie Visual Studio Enterprise. Öffnen Sie den **Visual Studio-Installer**, um Ihre Visual Studio Enterprise-Installation zu ändern. Wählen Sie unter **Einzelne Komponenten** die Option **Tools für Webleistung und Auslastungstests**.

2. Öffnen Sie die .webtest-Datei und starten Sie die Aufzeichnung.

    ![Öffnen Sie die WEBTEST-Datei, und klicken Sie auf "Aufzeichnen".](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-start.png)
3. Führen Sie die Benutzeraktionen durch, die Sie im Test simulieren möchten: Öffnen Sie Ihre Website, fügen Sie dem Warenkorb ein Produkt hinzu usw. Beenden Sie dann den Test.

    ![Die Webtestaufzeichnung wird im Internet Explorer ausgeführt.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-record.png)

    Erstellen Sie kein langes Szenario. Es gibt eine Beschränkung von 100 Schritten und 2 Minuten.
4. Bearbeiten Sie den Test:

   * Fügen Sie Validierungen zum Überprüfen der empfangenen Text- und Antwortcodes hinzu.
   * Entfernen Sie alle überflüssigen Interaktionen. Sie können auch abhängige Anforderungen für Bilder oder für Werbe- oder Nachverfolgungswebsites entfernen.

     Denken Sie daran, dass Sie nur das Testskript bearbeiten können. Sie können keinen benutzerdefinierten Code hinzufügen oder andere Webtests aufrufen. Fügen Sie keine Schleifen in den Test ein. Sie können standardmäßige Webtest-Plug-Ins verwenden.
5. Führen Sie den Test in Visual Studio aus, um sicherzustellen, dass er funktioniert.

    Das Webtest-Ausführungsprogramm öffnet einen Webbrowser und wiederholt die aufgezeichneten Aktionen. Stellen Sie sicher, dass der Test wie erwartet funktioniert.

    ![Öffnen Sie in Visual Studio die WEBTEST-Datei, und klicken Sie auf "Ausführen".](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-run.png)

#### <a name="2-upload-the-web-test-to-application-insights"></a>2. Hochladen des Webtests in Application Insights
1. Erstellen Sie im Application Insights-Portal einen Webtest.

    ![Wählen Sie auf dem Blatt "Webtests" die Option "Hinzufügen".](./media/app-insights-monitor-web-app-availability/16-another-test.png)
2. Wählen Sie einen mehrstufigen Test aus und laden Sie die .webtest-Datei hoch.

    ![Wählen Sie einen mehrstufigen Webtest aus.](./media/app-insights-monitor-web-app-availability/appinsights-71webtestUpload.png)

    Legen Sie die Teststandorte, Häufigkeit und Warnungsparameter auf die gleiche Weise wie für Ping-Tests fest.

#### <a name="3-see-the-results"></a>3. Anzeigen der Ergebnisse

Zeigen Sie die Testergebnisse und alle Fehler auf die gleiche Weise wie für die Tests mit einer einzelnen URL an.

Außerdem können Sie die Testergebnisse herunterladen, um sie in Visual Studio anzuzeigen.

#### <a name="too-many-failures"></a>Treten zu viele Fehler auf?

* Eine häufige Ursache für Fehler ist, dass der Test zu lange ausgeführt wird. Er darf nicht länger als zwei Minuten ausgeführt werden.

* Denken Sie daran, dass alle Ressourcen einer Seite richtig geladen werden müssen, damit der Test erfolgreich ist. Dazu gehören auch Skripts, Stylesheets, Bilder usw.

* Der Webtest muss vollständig im WEBTEST-Skript enthalten sein: Sie können keine codierten Funktionen im Test verwenden.

### <a name="plugging-time-and-random-numbers-into-your-multi-step-test"></a>Einfügen von Zeiten und Zufallszahlen in einen mehrstufigen Test
Angenommen, Sie testen ein Tool, das zeitabhängige Daten, wie z. B. Bestände, aus einem externen Feed abruft. Beim Aufzeichnen eines Webtests müssen Sie bestimmte Zeiten verwenden, aber als Parameter des Tests – StartTime und EndTime – festlegen.

![Ein Webtest mit Parametern.](./media/app-insights-monitor-web-app-availability/appinsights-72webtest-parameters.png)

Wenn Sie den Test ausführen, sollte "EndTime" immer die aktuelle Uhrzeit widerspiegeln, und "StartTime" sollte der Uhrzeit vor 15 Minuten entsprechen.

Webtest-Plug-Ins sind eine Möglichkeit zum Parametrisieren von Zeiten.

1. Fügen Sie ein Webtest-Plug-In für jeden gewünschten Variablenparameterwert hinzu. Wählen Sie in der Webtest-Symbolleiste **Webtest-Plug-In hinzufügen**aus.

    ![Wählen Sie "Webtest-Plug-In hinzufügen", und wählen Sie einen Typ aus.](./media/app-insights-monitor-web-app-availability/appinsights-72webtest-plugins.png)

    In diesem Beispiel verwenden wir zwei Instanzen des Datums-/Uhrzeit-Plug-Ins. Eine Instanz gilt für „vor 15 Minuten“ und eine weitere für „jetzt“.
2. Öffnen Sie die Eigenschaften der einzelnen Plug-Ins. Geben Sie einen Namen ein, und legen Sie es so fest, dass die aktuelle Uhrzeit verwendet wird. Legen Sie für eines der Plug-Ins "Add Minutes = -15" fest.

    ![Legen Sie den Name fest, wählen Sie "Aktuelle Uhrzeit verwenden" und "Minuten hinzufügen".](./media/app-insights-monitor-web-app-availability/appinsights-72webtest-plugin-parameters.png)
3. Verwenden Sie im Webtestparameter {{Plug-In-Name}} zum Verweisen auf einen Plug-In-Namen.

    ![Verwenden Sie im Testparameter "{{Plug-In-Name}}".](./media/app-insights-monitor-web-app-availability/appinsights-72webtest-plugin-name.png)

Laden Sie nun den Test in das Portal hoch. Bei jeder Ausführung des Tests werden die dynamischen Werte verwendet.

## <a name="dealing-with-sign-in"></a>Umgang mit der Anmeldung
Wenn sich Benutzer bei Ihrer App anmelden, stehen Ihnen verschiedene Optionen für die Anmeldungssimulation zur Verfügung, damit Sie Seiten testen können, die auf die Anmeldung folgen. Der verwendete Ansatz hängt vom Typ der von der App bereitgestellten Sicherheit ab.

In allen Fällen sollten Sie in der Anwendung ein Konto erstellen, das nur Testzwecken dient. Schränken Sie die Berechtigungen dieses Testkontos nach Möglichkeit ein, damit ausgeschlossen ist, dass sich die Webtests auf echte Benutzer auswirken.

### <a name="simple-username-and-password"></a>Einfacher Benutzername und Kennwort
Zeichnen Sie einen Webtest auf normale Weise auf. Löschen Sie zuerst Cookies.

### <a name="saml-authentication"></a>SAML-Authentifizierung
Verwenden Sie für Webtests das verfügbare SAML-Plug-In.

### <a name="client-secret"></a>Geheimer Clientschlüssel
Verwenden Sie diese Route, wenn die Anmelderoute Ihrer App einen geheimen Clientschlüssel umfasst. Azure Active Directory (AAD) ist ein Beispiel für einen Dienst, bei dem eine Anmeldung mit geheimem Clientschlüssel bereitgestellt wird. In AAD ist der geheime Clientschlüssel der App-Schlüssel.

Hier ist ein Beispiel für einen Webtest einer Azure-Web-App mit App-Schlüssel angegeben:

![Beispiel für geheimen Clientschlüssel](./media/app-insights-monitor-web-app-availability/110.png)

1. Rufen Sie das Token aus AAD mit dem geheimen Clientschlüssel (AppKey) ab.
2. Extrahieren Sie das Bearertoken aus der Antwort.
3. Rufen Sie die API mit dem Bearertoken im Autorisierungsheader auf.

Stellen Sie sicher, dass der Webtest ein richtiger Client ist – also über eine eigene App in AAD verfügt –, und verwenden Sie dessen Client-ID und App-Schlüssel (clientId und appkey). Ihr getesteter Dienst verfügt ebenfalls über eine eigene App in AAD: Der appID-URI dieser App wird im Webtest im Feld „resource“ angegeben.

### <a name="open-authentication"></a>Offene Authentifizierung
Ein Beispiel für die offene Authentifizierung ist die Anmeldung mit Ihrem Microsoft- oder Google-Konto. Viele Apps, die OAuth verwenden, stellen die Alternative mit dem geheimen Clientschlüssel bereit. Ihre erste Taktik sollte also darin bestehen, diese Möglichkeit zu untersuchen.

Wenn bei Ihrem Test die Anmeldung mit OAuth erforderlich ist, lautet die allgemeine Vorgehensweise wie folgt:

* Verwenden Sie ein Tool wie Fiddler, um den Datenverkehr zwischen Ihrem Webbrowser, der Authentifizierungswebsite und Ihrer App zu untersuchen.
* Führen Sie mindestens zwei Anmeldungen auf verschiedenen Computern bzw. in verschiedenen Browsern oder mit großen zeitlichen Abständen durch (damit Token ablaufen können).
* Vergleichen Sie verschiedene Sitzungen, um das von der authentifizierenden Website zurückgegebene Token zu identifizieren, das nach der Anmeldung dann an Ihren App-Server übergeben wird.
* Zeichnen Sie einen Webtests mit Visual Studio auf.
* Parametrisieren Sie die Token, indem Sie den Parameter festlegen, wenn das Token vom Authentifikator zurückgegeben wird, und ihn in der Abfrage an die Website verwenden.
  (Visual Studio versucht, den Test zu parametrisieren, aber die Token werden nicht richtig parametrisiert.)


## <a name="performance-tests"></a>Leistungstests
Sie können für Ihre Website einen Auslastungstest durchführen. Wie beim Verfügbarkeitstest auch, können Sie entweder einfache Anforderungen oder Anforderungen mit mehreren Schritten von unseren weltweit vorhandenen Punkten senden. Im Gegensatz zu einem Verfügbarkeitstest werden viele Anforderungen gesendet, um mehrere gleichzeitige Benutzer zu simulieren.

Öffnen Sie auf dem Blatt „Übersicht“ die Option **Einstellungen** > **Leistungstests**. Wenn Sie einen Test erstellen, können Sie eine Verbindung mit einem Visual Studio Team Services-Konto herstellen bzw. ein Konto erstellen.

Nach Abschluss des Tests werden die Antwortzeiten und Erfolgsraten angezeigt.


![Leistungstest](./media/app-insights-monitor-web-app-availability/perf-test.png)

> [!TIP]
> Verwenden Sie zum Beobachten der Auswirkungen eines Leistungstests [Livestream](app-insights-live-stream.md) und [Profiler](app-insights-profiler.md).
>

## <a name="automation"></a>Automation
* [Verwenden Sie PowerShell-Skripts zum automatischen Einrichten eines Verfügbarkeitstests](app-insights-powershell.md#add-an-availability-test).
* Richten Sie einen [Webhook](../monitoring-and-diagnostics/insights-webhooks-alerts.md) ein, der bei einer Warnung aufgerufen wird.

## <a name="qna"></a>Fragen? Probleme?
* *Kann ich Code aus meinem Webtest aufrufen?*

    Nein. Die Schritte des Tests müssen in der Webtest-Datei enthalten sein. Und Sie können keine anderen Webtests aufrufen oder Schleifen verwenden. Aber es gibt mehrere hilfreiche Plug-Ins.
* *Wird HTTPS unterstützt?*

    Wir unterstützen TLS 1.1 und TLS 1.2.
* *Gibt es einen Unterschied zwischen „Webtests“ und „Verfügbarkeitstests“?*

    Die beiden Begriffe sind austauschbar. „Verfügbarkeitstests“ ist ein allgemeinerer Begriff, der neben den mehrstufigen Webtests auch die einzelnen URL-Pingtests enthält.
* *Ich möchte Verfügbarkeitstests auf unserem internen Server verwenden, der hinter einer Firewall ausgeführt wird.*

    Es gibt zwei mögliche Lösungen:
    
    * Konfigurieren Sie die Firewall so, dass eingehende Anforderungen von den [IP-Adressen der Webtest-Agents](app-insights-ip-addresses.md) zugelassen werden.
    * Schreiben Sie eigenen Code zum regelmäßigen Testen Ihres internen Servers. Führen Sie den Code als Hintergrundprozess auf einem Testserver hinter Ihrer Firewall aus. Die Ergebnisse des Testvorgangs können an Application Insights gesendet werden, indem die [TrackAvailability()](https://docs.microsoft.com/dotnet/api/microsoft.applicationinsights.telemetryclient.trackavailability)-API im Core SDK-Paket verwendet wird. Hierfür ist es erforderlich, dass Ihr Testserver Zugriff in ausgehender Richtung auf den Application Insights-Erfassungsendpunkt hat. Dies ist aber ein deutlich geringeres Sicherheitsrisiko als bei der Alternativlösung, bei der eingehende Anforderungen zugelassen werden. Die Ergebnisse werden nicht auf den Blättern der Verfügbarkeitswebtests angezeigt, sondern als Verfügbarkeitsergebnisse in Analytics, Search und Metrik-Explorer.
* *Fehler beim Hochladen eines Webtests mit mehreren Schritten*

    Die Größenbeschränkung beträgt 300 K.

    Schleifen werden nicht unterstützt.

    Verweise auf andere Webtests werden nicht unterstützt.

    Datenquellen werden nicht unterstützt.
* *Test mit mehreren Schritten wird nicht abgeschlossen*

    Pro Test können maximal 100 Anforderungen verwendet werden.

    Der Test wird beendet, wenn die Ausführung länger als zwei Minuten dauert.
* *Wie kann ich einen Test mit Clientzertifikaten durchführen?*

    Dies wird leider nicht unterstützt.


## <a name="next"></a>Nächste Schritte
[Durchsuchen der Diagnoseprotokolle][diagnostic]

[Behandeln von Problemen][qna]

[IP-Adressen von Webtest-Agents](app-insights-ip-addresses.md)

<!--Link references-->

[azure-availability]: ../insights-create-web-tests.md
[diagnostic]: app-insights-diagnostic-search.md
[qna]: app-insights-troubleshoot-faq.md
[start]: app-insights-overview.md

