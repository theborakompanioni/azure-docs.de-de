<properties
	pageTitle="Versionshinweise für Visual Studio-Erweiterung für Developer Analytics"
	description="Neueste Updates für Visual Studio-Tools für Developer Analytics"
	services="application-insights"
    documentationCenter=""
	authors="acearun"
	manager="douge"/>
<tags
	ms.service="application-insights"
	ms.workload="tbd"
	ms.tgt_pltfrm="ibiza"
	ms.devlang="na"
	ms.topic="article"
	ms.date="06/09/2016"
	ms.author="acearun"/>

# Versionshinweise – Developer Analytics Tools
##### Application Insights- und HockeyApp-Analysen in Visual Studio
## Version 7.0
###Application Insights-Trends
Application Insights-Trends ist ein neues Tool in Visual Studio für die Analyse des Verhaltens Ihrer App im Laufe der Zeit. Wählen Sie zum Einstieg auf der Application Insights-Symbolleiste oder im Application Insights-Suchfenster die Schaltfläche „Telemetrietrends untersuchen“. Alternativ können Sie unter „Ansicht > Andere Fenster“ die Option „Application Insights-Trends“ wählen. Wählen Sie eine der fünf allgemeinen Abfragen aus, um zu beginnen. Sie können unterschiedliche Datasets basierend auf Telemetrietypen, Zeiträumen und anderen Eigenschaften analysieren. Wählen Sie zum Ermitteln von Anomalien in Ihren Daten in der Dropdownliste „Ansichtstyp“ eine Anomalieoption. Mit den Filteroptionen am unteren Rand des Fensters ist es einfach, bestimmte Teilmengen Ihrer Telemetriedaten anzuzeigen.

![Application Insights-Trends](./media/app-insights-release-notes-vsix/Trends.PNG)

###Ausnahmen in CodeLens
Telemetriedaten zu Ausnahmen werden jetzt in CodeLens angezeigt. Wenn Sie Ihr Projekt mit dem Application Insights-Dienst verknüpft haben, wird die Anzahl von Ausnahmen angezeigt, die in den letzten 24 Stunden in den einzelnen Methoden in der Produktion aufgetreten sind. Von CodeLens können Sie zur Suche oder zu den Trends wechseln, um die Ausnahmen genauer zu untersuchen.

![Ausnahmen in CodeLens](./media/app-insights-release-notes-vsix/ExceptionsCodeLens.png)

###Unterstützung für ASP.NET Core
Application Insights unterstützt jetzt ASP.NET Core RC2-Projekte in Visual Studio. Sie können Application Insights über das Dialogfeld „Neue Projekte“ einem neuen ASP.NET Core RC2-Projekt hinzufügen. Um Application Insights einem vorhandenen Projekt hinzuzufügen, klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und wählen Sie „Application Insights-Telemetrie hinzufügen...“ aus.

![Unterstützung für .NET Core](./media/app-insights-release-notes-vsix/NetCoreSupport.PNG)

Für ASP.NET 5 RC1- und ASP.NET Core RC2-Projekte steht im Fenster „Diagnosetools“ ebenfalls neue Unterstützung bereit. Während des lokalen Debuggens werden Application Insights-Ereignisse wie z.B. Anforderungen und Ausnahmen aus Ihrer ASP.NET-App angezeigt. Sie können weitere Informationen zu jedem Ereignis anzeigen, indem Sie auf „Suche“ klicken.

![Unterstützung für Diagnosetools](./media/app-insights-release-notes-vsix/DiagnosticTools.PNG)

###HockeyApp für universelle Windows-Apps
Zusätzlich zur Betaverteilung und zum Benutzerfeedback bietet HockeyApp Absturzberichte in Form von Symbolen für Ihre universellen Windows-Apps. Das Hinzufügen des HockeyApp SDK ist jetzt noch einfacher: Klicken Sie mit der rechten Maustaste auf Ihr universelles Windows-Projekt, und wählen Sie „Hockey App - Enable Crash Analytics...“ (HockeyApp > Absturzanalyse aktivieren...). Dadurch wird das SDK installiert, die Absturzsammlung eingerichtet und eine HockeyApp-Ressource in der Cloud bereitgestellt. Ihre App wird dabei nicht in den HockeyApp-Dienst hochgeladen.

Weitere neue Features:

* Wir haben die Application Insights Search-Benutzeroberfläche wurde schneller und intuitiver gestaltet – Zeiträume und Detailfilter werden bei Auswahl jetzt automatisch angewendet.
* Application Insights Search bietet jetzt eine Option, aus der Anforderungstelemetrie direkt zum Code zu wechseln.
* Wir haben die Benutzeroberfläche zur Anmeldung bei HockeyApp verbessert.
* In Diagnosetools werden jetzt Produktionstelemetriedaten für Ausnahmen angezeigt.

## Version 5.2
Wir freuen uns, die Einführung von HockeyApp-Szenarien in Visual Studio ankündigen zu können. Die erste Integration, die wir ermöglicht haben, ist die Beta-Distribution von universellen Windows-Apps und Windows Forms-Apps in VS.

Mit der Beta-Distribution können Sie frühe Versionen Ihrer Apps für die Verteilung an eine ausgewählte Gruppe von Kunden oder Testpersonen in HockeyApp hochladen. Die Beta-Distribution kann vor einer größeren Veröffentlichung in Kombination mit der HockeyApp-Absturzerfassung und den Funktionen für Benutzerfeedback wertvolle Informationen zu Ihrer App liefern. Sie können diese Informationen nutzen, um Probleme der App zu beheben, bevor diese zu einem größeren Problem werden (schlechte Bewertungen, negatives Feedback usw.).

Informieren Sie sich darüber, wie einfach es ist, Builds für die Beta-Distribution aus VS hochzuladen…
### Universelle Windows-Apps
Das Kontextmenü für einen UWP-Projektknoten enthält jetzt eine Option zum Hochladen Ihres Builds in HockeyApp.

![Kontextmenü des Projekts für universelle Apps](./media/app-insights-release-notes-vsix/UniversalContextMenu.png)

Wenn Sie die Option wählen, wird das Dialogfeld für den HockeyApp-Upload angezeigt. Sie benötigen ein HockeyApp-Konto, um den Build hochzuladen. Es macht nichts, wenn Sie ein neuer Benutzer sind. Das Erstellen eines Kontos ist ein einfacher Prozess.

Nachdem die Verbindung hergestellt wurde, wird das Formular für den Upload im Dialogfeld angezeigt.

![Dialogfeld „Upload“ für universelle Apps](./media/app-insights-release-notes-vsix/UniversalUploadDialog.png)

Wählen Sie den Inhalt für den Upload aus (appxbundle oder appx), und wählen Sie im Assistenten dann die Veröffentlichungsoptionen aus. Auf der nächsten Seite können Sie optional die Versionshinweise hinzufügen. Wählen Sie die Option „Fertig stellen“, um den Upload zu starten.

Nach Abschluss des Uploads wird ein HockeyApp-Popup mit der Bestätigung und einem Link zur App im HockeyApp-Portal angezeigt.

![Popup „Upload abgeschlossen“](./media/app-insights-release-notes-vsix/UploadComplete.png)

Das ist schon alles. Sie haben mit wenigen Klicks einen Build für die Beta-Distribution hochgeladen.

Im HockeyApp-Portal können Sie Ihre Anwendung auf verschiedene Arten verwalten (Benutzer einladen, Absturzberichte und Feedback anzeigen, Details ändern usw.).

![HockeyApp-Portal](./media/app-insights-release-notes-vsix/HockeyAppPortal.png)

Weitere Informationen zur App-Verwaltung finden Sie in der [Hockey App Knowledge Base](http://support.hockeyapp.net/kb/app-management-2).

### Windows Forms-Apps
Das Kontextmenü für einen Windows Forms-Projektknoten enthält eine Option zum Hochladen Ihres Builds in HockeyApp.

![Kontextmenü des Projekts für Windows Forms-Apps](./media/app-insights-release-notes-vsix/WinFormContextMenu.png)

Das HockeyApp-Dialogfeld für Uploads wird angezeigt. Es ähnelt dem Dialogfeld für universelle Windows-Apps.

![Dialogfeld „Upload“ für Windows Forms-Apps](./media/app-insights-release-notes-vsix/WinFormsUploadDialog.png)

Beachten Sie, dass dieser Assistent ein zusätzliches Feld zum Angeben der App-Version enthält. Für universelle Apps werden die Informationen aus dem Manifest eingefügt. Win Forms verfügen leider nicht über eine Ressource dieser Art, sodass die Informationen manuell angegeben werden müssen.

Der Rest des Ablaufs ähnelt dem Ablauf für universelle Apps. Sie können im HockeyApp-Portal den Build auswählen, Optionen für die Veröffentlichung nutzen, Versionshinweise hinzufügen, einen Upload durchführen und Verwaltungsschritte ausführen.

So einfach ist dieser Vorgang. Probieren Sie es aus, und teilen Sie uns Ihre Meinung mit.
## Version 4.3
### Suchen von Telemetriedaten aus lokalen Debugsitzungen
In dieser Version führen wir eine Möglichkeit zum Suchen nach Application Insights-Telemetriedaten ein, die in der Visual Studio-Debugsitzung generiert wurden. Die Suche war bisher nur möglich, wenn Sie Ihre App bei Application Insights registriert hatten. Ab dieser Version muss für die App nur das Application Insights-SDK installiert werden, um lokale Telemetriedaten zu suchen.

#### Bei einer ASP.NET-Anwendung mit Application Insights-SDK

- Debuggen Sie die Anwendung.
- Öffnen Sie die Suche in Application Insights mithilfe einer der folgenden Vorgehensweisen
	- Ansicht-> Weitere Fenster -> Application Insights-Suche.
	- Klicken Sie auf die Symbolleisten-Schaltfläche „Application Insights“.
	- Erweitern Sie im Projektmappen-Explorer „ApplicationInsights.config“ -> Debugsitzungstelemetrie suchen.
- Wenn Sie nicht bei Application Insights angemeldet sind, wird das Fenster „Suchen“ im Modus „Debugsitzungstelemetrie“ geöffnet.
- Klicken Sie auf das Suchsymbol, um Ihre lokale Telemetrie anzuzeigen.

![Upload abgeschlossen](./media/app-insights-release-notes-vsix/LocalSearch.png)



##Version 4.2
In dieser Version haben wir Features zum Suchen von Daten im Ereigniskontext sowie das Senden von Protokolldaten an Application Insights vereinfacht und die Möglichkeit geschaffen, aus mehr Datenereignissen in den Code zu springen. Diese Erweiterung wird monatlich aktualisiert. Wenn Sie Feedback oder Funktionsanforderungen haben, senden Sie diese an aidevtools@microsoft.com.
###- Protokollierung mit 0 Klicks
Wenn Sie NLog, Log4Net oder System.Diagnostics für die Ablaufverfolgung verwenden, dann müssen Sie nicht alle Ihre Ablaufverfolgungen nach Application Insights verschieben. Die Application Insights-Protokollierungsadapter sind nun direkt in die normale Konfigurationsoberfläche integriert. Falls Sie bereits eines dieser Protokollierungsframeworks konfiguriert haben, können Sie dies folgendermaßen erreichen:
####Wenn Sie bereits Application Insights hinzugefügt haben
- Klicken Sie mit der rechten Maustaste auf den Projektknoten, wählen Sie „Application Insights“ und dann „Application Insights konfigurieren“ aus. Vergewissern Sie sich, dass der richtige Adapter im Konfigurationsfenster hinzugefügt werden kann.
- Wenn Sie die Projektmappe erstellen, können Sie auch im Popupfenster, das oben rechts angezeigt wird, auf „Konfigurieren“ klicken. ![Anmeldungspopup](./media/app-insights-release-notes-vsix/LoggingToast.png)

Nachdem Sie den Protokollierungsadapter installiert haben, können Sie die Anwendung ausführen und überprüfen, ob die Daten wie folgt auf der Registerkarte der Diagnosetools angezeigt werden: ![Ablaufverfolgungen](./media/app-insights-release-notes-vsix/Traces.png)
###- Benutzer können den Code suchen und zu ihm springen, an den die Telemetrieereigniseigenschaft ausgegeben wurde.
In der neuen Version können Benutzer dann auf einen beliebigen Wert in den Ereignisdetails klicken, um nach einer übereinstimmenden Zeichenfolge in der aktuell geöffneten Projektmappe zu suchen. Die Ergebnisse werden in Visual Studio in der Liste „Suchergebnisse“ angezeigt, wie unten dargestellt: ![Übereinstimmung suchen](./media/app-insights-release-notes-vsix/FindMatch.png)
###- Neuer Bildschirm für nicht angemeldete Benutzer im Suchfenster
Wir haben das Aussehen des Suchfensters verbessert, um Benutzer beim Suchen ihrer Daten in der Produktion zu unterstützen. ![Fenster „Suchen“](./media/app-insights-release-notes-vsix/SearchWindow.png)
###- Benutzer können alle Telemetrieereignisse anzeigen, die dem Ereignis zugeordnet sind.
Es wurde eine neue Registerkarte neben den Ereignisdetails hinzugefügt, die vordefinierte Abfragen zum Anzeigen aller Daten zu dem Telemetrieereignis, das der Benutzer anzeigt, enthält. Beispiel: Wenn die Anforderung ein Feld für die Vorgangs-ID enthält, dann hat jedes Ereignis zu dieser Anforderung die gleiche Vorgangs-ID. Wenn also während der Verarbeitung der Anforderung eine Ausnahme auftritt, erhält sie die gleiche Vorgangs-ID wie die Anforderung, sodass sie einfacher aufzufinden ist, usw. Damit können Benutzer, die eine Anforderung anzeigen, jetzt einfach auf „Alle Telemetriedaten für diesen Vorgang“ klicken, um eine neue Registerkarte mit den neuen Suchergebnissen zu öffnen. ![Verwandte Elemente](./media/app-insights-release-notes-vsix/RelatedItems.png)
### - Vorwärts/Rückwärts-Feature im Suchverlauf hinzugefügt.
Benutzer können nun zwischen den Suchergebnissen wechseln. ![Zurück navigieren](./media/app-insights-release-notes-vsix/GoBAck.png)

##Version 4.1
Diese Version ergänzt frühere Versionen um eine Reihe neuer Funktionen und Verbesserungen. Sie müssen Update 1 auf Ihrem Computer installiert haben, um diese Version erhalten zu können.

### Springen von einer Ausnahme zu einer Methode in Quellcode
Benutzer, die sich über die Application Insights-Suche Ausnahmen von ihren Produktions-Apps anzeigen lassen, können nun direkt zu der Methode in ihrem Code springen, an der die Ausnahme auftritt. Sie müssen nur das richtige Projekt laden und wir kümmern uns um den Rest. (Weitere Informationen zum Suchfenster finden Sie in den unten stehenden Hinweisen zur Version 4.0).

#### Wie funktioniert Application Insights?

Sie müssen keine Projektmappe geöffnet haben, um AI-Search verwenden zu können. In diesem Fall wird im Bereich Stapelüberwachung eine Benachrichtigung angezeigt und viele Elemente in diesem Bereich werden abgeblendet angezeigt.


Falls Dateiinformationen verfügbar wären, könnten einige Elemente möglicherweise Links darstellen, doch das Projektinformationselement ist immer noch sichtbar.

Durch Klicken auf den Hyperlink gelangen Sie zu dem Ort, an dem die ausgewählte Methode in Ihrem Code liegt. Möglicherweise gibt es einen Unterschied in der Versionsnummer, aber diese Funktion wird in späteren Versionen folgen: wechseln Sie zur richtigen Version des Codes.

![Klicken auf die Ausnahme](./media/app-insights-release-notes-vsix/jumptocode.png)

###Neue Einstiegspunkte für Suchvorgänge im Projektmappen-Explorer

![Einstiegspunkt im Projektmappen-Explorer](./media/app-insights-release-notes-vsix/searchentry.png)


###Erscheinen eines Popupfensters nach Beenden der Veröffentlichung
Ein Popupfenster wird angezeigt, nachdem das Projekt online veröffentlicht wurde, damit Sie Ihre Application Insights-Daten in der Produktion anzeigen können.

![Popup](./media/app-insights-release-notes-vsix/publishtoast.png)

## Version 4.0

###Application Insights-Daten in Visual Studio suchen
Genau wie bei der Suchenfunktion im Application Insights-Portal können Sie filtern und nach Ereignistypen, Eigenschaftswerten und Text suchen sowie einzelne Ereignisse überprüfen.

![Fenster „Suchen“](./media/app-insights-release-notes-vsix/search.png)

###Anzeigen von Daten aus Ihrer Local Box im Fenster Diagnosetools

Zusätzlich werden im Visual Studio Diagnosehub Ihre Telemetriedaten zusammen mit anderen Debugdaten angezeigt. Dies wird nur in ASP.NET 4.5 unterstützt. Unterstützung für ASP. NET-5 wird in zukünftigen Versionen verfügbar sein.

![Diagnosehub-Fenster](./media/app-insights-release-notes-vsix/diagtools.png)

###Hinzufügen des SDK zu Ihrem Projekt, ohne bei Azure angemeldet sein zu müssen

Sie müssen sich nicht mehr bei Azure anmelden, um Application Insights-Pakete Ihrem Projekt hinzuzufügen, egal ob im Dialogfeld „Neues Projekt“ oder im Kontextmenü des Projekts. Wenn Sie sich anmelden, wird das SDK wie üblich installiert und für das Senden von Telemetriedaten konfiguriert. Wenn Sie sich nicht anmelden, wird das SDK Ihrem Projekt hinzugefügt. Es generiert die Telemetriedaten für den Diagnosehub . Sie können es später konfigurieren, wenn Sie möchten.

![Dialogfeld „Neues Projekt“](./media/app-insights-release-notes-vsix/newproject.png)

###Unterstützung von Geräten

Im Rahmen der *Connect();* 2015 haben wir [angekündigt](https://azure.microsoft.com/blog/deep-diagnostics-for-web-apps-with-application-insights/), dass HockeyApp unsere Mobile DevOps-Umgebung für Geräte ist. HockeyApp unterstützt Sie beim Verteilen der Beta-Builds an Ihre Tester, beim Sammeln und Analysieren von allen Abstürzen Ihrer App und beim Sammeln von Feedback direkt von Ihren Kunden. HockeyApp unterstützt Sie auf jeder beliebigen Plattform, auf der Sie Ihre mobile Anwendung erstellen, z. B. iOS, Android oder Windows oder eine plattformübergreifende Lösung wie Xamarin, Cordova oder Unity.

In zukünftigen Versionen der Application Insights-Erweiterung werden wir neue Funktionen einführen, die eine stärkere Integration zwischen HockeyApp und Visual Studio ermöglichen. Beginnen Sie mit HockeyApp, indem Sie einfach den NuGet-Verweis hinzufügen. Weitere Informationen finden Sie in der [Dokumentation](http://support.hockeyapp.net/kb/client-integration-windows-and-windows-phone).

<!---HONumber=AcomDC_0622_2016-->