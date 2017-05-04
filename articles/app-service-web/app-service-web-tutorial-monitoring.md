---
title: "Überwachen einer Web-App | Microsoft-Dokumentation"
description: "Informationen zum Einrichten der Überwachung für eine Web-App"
services: App-Service
keywords: 
author: btardif
ms.author: byvinyal
ms.date: 04/04/2017
ms.topic: article
ms.service: app-service-web
translationtype: Human Translation
ms.sourcegitcommit: abdbb9a43f6f01303844677d900d11d984150df0
ms.openlocfilehash: 8d9b4a4fa3b62659fc7e2aa1c6329fdc5e01fe39
ms.lasthandoff: 04/20/2017

---
# <a name="monitor-app-service"></a>Überwachen des App Service
Dieses Tutorial führt Sie durch die Überwachung Ihrer App und die Überwachung der integrierten Plattformtools zum Beheben etwaiger Probleme.

Jeder Abschnitt dieses Dokuments behandelt ein bestimmtes Feature. Wenn Sie die Features gemeinsam verwenden, haben Sie folgende Möglichkeiten:
- Identifizieren eines Problems in Ihrer App
- Ermitteln, ob das Problem durch Ihren Code oder die Plattform verursacht wird
- Eingrenzen der Ursache des Problems im Code
- Debuggen und Beheben des Problems

## <a name="before-you-begin"></a>Voraussetzungen
- Sie benötigen eine Web-App zum Überwachen und zum Ausführen der beschriebenen Schritte. 
    - Mit den im Lernprogramm zum [Erstellen einer ASP.NET-App in Azure mit SQL-Datenbank](app-service-web-tutorial-dotnet-sqldatabase.md) beschriebenen Schritten können Sie eine Anwendung erstellen.

- Wenn Sie das **Remotedebuggen** Ihrer Anwendung testen möchten, benötigen Sie Visual Studio. 
    - Falls Sie Visual Studio 2017 noch nicht installiert haben, können Sie die kostenlose [Visual Studio 2017 Community-Edition](https://www.visualstudio.com/downloads/) herunterladen und verwenden. 
    - Aktivieren Sie beim Setup von Visual Studio die Option **Azure-Entwicklung**.

## <a name="metrics"></a> Schritt 1 – Anzeigen von Metriken
**Metriken** sind nützlich, um Folgendes zu verstehen: 
- Anwendungsintegrität
- Anwendungsleistung
- Ressourcenverbrauch

Beim Untersuchen eines Anwendungsproblems ist die Überprüfung von Metriken ein guter Ausgangspunkt. Das Azure-Portal verfügt mit dem **Azure Monitor** über eine schnelle Möglichkeit zum Anzeigen der Metriken für die App.

Metriken stellen eine Verlaufsansicht über mehrere Hauptaggregationen für die App bereit. Für jede im App Service gehostete App sollten Sie die Web-App und den App Service-Plan überwachen.

> [!NOTE]
> Ein App Service-Plan stellt die Sammlung physischer Ressourcen dar, die zum Hosten Ihrer Apps verwendet werden. Alle einem App Service-Plan zugewiesenen Anwendungen teilen sich die durch den Plan definierten Ressourcen. Das spart Kosten, wenn Sie mehrere Apps hosten.
>
> In App Service-Plänen wird Folgendes definiert:
> * Region: „Europa, Norden“, „USA, Osten“, „Asien, Südosten“ usw.
> * Instanzgröße: klein, mittel, groß usw.
> * Skalierung: eine, zwei, drei Instanzen usw.
> * SKU: Free, Shared, Basic, Standard, Premium usw.

Um die Metriken für Ihre Web-App zu überprüfen, wechseln Sie zum Blatt **Übersicht** der App, die Sie überwachen möchten. Hier können Sie ein Diagramm für die Metriken Ihrer App als **Überwachungskachel** anzeigen. Klicken Sie auf die Kachel, um zu bearbeiten und zu konfigurieren, welche Metriken und welcher Zeitbereich angezeigt werden sollen. 

Standardmäßig bietet das Ressourcenblatt eine Ansicht für die Anwendungsanforderungen und die Fehler in der letzten Stunde.
![Überwachen der App](media/app-service-web-tutorial-monitoring/app-service-monitor.png)

Wie im Beispiel zu erkennen ist, generiert eine Anwendung viele **HTTP-Serverfehler**. Die große Anzahl von Fehlern ist das erste Anzeichen, das wir zum Untersuchen dieser Anwendung benötigen.

> [!TIP]
> Unter folgenden Links finden Sie weitere Informationen zu Azure Monitor:
> - [Erste Schritte mit Azure Monitor](..\monitoring-and-diagnostics\monitoring-overview.md)
> - [Metriken in Azure](..\monitoring-and-diagnostics\monitoring-overview-metrics.md)
> - [Unterstützte Metriken von Azure Monitor](..\monitoring-and-diagnostics\monitoring-supported-metrics.md#microsoftwebsites-including-functions)
> - [Azure-Dashboards](..\azure-portal\azure-portal-dashboards.md)

## <a name="alerts"></a> Schritt 2: Konfigurieren von Warnungen
**Warnungen** können so konfiguriert werden, dass sie unter bestimmten Bedingungen für Ihre App ausgelöst werden.

In [Schritt 1 – Anzeigen von Metriken](#metrics) haben wir gesehen, dass die Anwendung eine große Anzahl von Fehlern aufwies. 

Nun konfigurieren wir eine Warnung, damit Sie automatisch benachrichtigt werden, wenn Fehler auftreten. In diesem Fall soll die Warnung jedes Mal, wenn die Anzahl der HTTP-Fehler 50X einen bestimmten Schwellenwert überschreitet, eine E-Mail senden.

Um eine Warnung zu erstellen, navigieren Sie zu **Überwachung** > **Warnungen**, und klicken Sie auf **[+] Warnung hinzufügen**.

![Warnungen](media/app-service-web-tutorial-monitoring/app-service-monitor-alerts.png)

Geben Sie Werte für die Konfiguration der Warnung an:
- **Ressource:** die Site, die mit der Warnung überwacht werden soll 
- **Name:** ein Name für die Warnung, in diesem Fall *High HTTP 50X*
- **Beschreibung:** Klartexterläuterung dazu, was in dieser Warnung ausgewertet wird
- **Warnung bei:** Warnungen können Metriken oder Ereignisse untersuchen, in diesem Beispiel werden Metriken betrachtet.
- **Metrik:** die zu überwachende Metrik, in diesem Fall *HTTP-Serverfehler*
- **Bedingung:** die Umstände, unter denen eine Warnung ausgegeben werden soll, in diesem Fall wählen Sie die Option *größer als* aus
- **Schwellenwert:** der Wert, auf den geachtet wird, in diesem Fall *400*
- **Zeitraum:** Warnungen beziehen sich auf den Durchschnittswert der Metrik. Kleinere Zeiträume ergeben empfindlichere Warnungen. In diesem Fall werden *5 Minuten* betrachtet. 
- **E-Mail-Besitzer, Mitwirkende und Leser:** in diesem Fall *Aktiviert*

Nachdem die Warnung erstellt wurde, wird jedes Mal, wenn die App den konfigurierten Schwellenwert überschreitet, eine E-Mail gesendet. Aktive Warnungen können auch im Azure-Portal eingesehen werden.

![Ausgelöste Warnungen](media/app-service-web-tutorial-monitoring/app-service-monitor-alerts-triggered.png)


> [!TIP]
> Unter folgenden Links finden Sie weitere Informationen zu Warnungen in Azure:
> - [Was sind Warnungen in Microsoft Azure?](..\monitoring-and-diagnostics\monitoring-overview-alerts.md)
> - [Ausführen von Aktionen für Metriken](..\monitoring-and-diagnostics\monitoring-overview.md)
> - [Erstellen von Metrikwarnungen](..\monitoring-and-diagnostics\insights-alerts-portal.md)

## <a name="companion"></a> Schritt 3: App Service Companion
Mit **App Service Companion** können Sie Ihre App mit einer nativen Darstellung auf Ihrem Mobilgerät (iOS oder Android) bequem überwachen.

Mit App Service Companion haben Sie folgende Möglichkeiten:
- Überprüfen von Anwendungsmetriken
- Überprüfen von Anwendungswarnungen und Empfehlungen und Ausführen entsprechender Aktionen
- Ausführen einer grundlegenden Fehlerbehebung (Durchsuchen, Starten, Beenden, Neustart der App)
- Push-Benachrichtigungen für kritische Ereignisse

![App Service Companion](media/app-service-web-tutorial-monitoring/app-service-companion.png)

[![App Service Companion im App Store](media/app-service-web-tutorial-monitoring/app-service-companion-appStore.png)](https://itunes.apple.com/app/azure-app-service-companion/id1146659260)
[![App Service Companion in Google Play](media/app-service-web-tutorial-monitoring/app-service-companion-googlePlay.png)](https://play.google.com/store/apps/details?id=azureApps.AzureApps)

Sie können App Service Companion im [App Store](https://itunes.apple.com/app/azure-app-service-companion/id1146659260) oder auf [Google Play](https://play.google.com/store/apps/details?id=azureApps.AzureApps) herunterladen.

## <a name="diagnose"></a> Schritt 4: Diagnose und Problembehandlung
**Diagnose und Problembehandlung** helfen dabei, Anwendungsprobleme von Plattformproblemen zu unterscheiden. Es können auch mögliche Gegenmaßnahmen vorgeschlagen werden, um Ihre Web-App wieder in einen problemfreien Zustand zu überführen.
 
![Diagnose und Problembehandlung](media/app-service-web-tutorial-monitoring/app-service-monitor-diagnosis.png)

Indem wir mit dem Beispiel aus den vorherigen Schritten fortfahren, sehen wir, dass bei der Anwendung Verfügbarkeitsprobleme aufgetreten sind. Die Plattformverfügbarkeit liegt hingegen weiterhin bei 100 %.

Wenn bei der App ein Problem auftritt und die Plattform weiterhin verfügbar ist, deutet dies auf ein Anwendungsproblem hin.

## <a name="logging"></a> Schritt 5: Protokollierung
Nachdem wir die Fehler nun auf ein Anwendungsproblem eingegrenzt haben, können wir die Anwendung und die Serverprotokolle untersuchen, um weitere Informationen zu erhalten.

Durch die Protokollierung können Sie Protokolle zu **Anwendungsdiagnosen** und **Webserverdiagnosen** für Ihre Web-App erfassen.

### <a name="application-diagnostics"></a>Anwendungsdiagnose
Mit der Option „Anwendungsdiagnose“ können Sie die von der Anwendung zur Laufzeit erzeugten Ablaufverfolgungen erfassen. 

Wenn Sie Ihrer Anwendung eine Ablaufverfolgung hinzufügen, verbessern Sie Ihre Möglichkeiten zum Debuggen und zum Erkennen von Probleme deutlich.

In ASP.NET können Sie die Ablaufverfolgung für eine Anwendung mit der [System.Diagnostics.Trace-Klasse](https://msdn.microsoft.com/library/system.diagnostics.trace.aspx) protokollieren, um Ereignisse zu generieren, die von der Protokollinfrastruktur erfasst werden. Sie können auch den Schweregrad der Ablaufverfolgung angeben, um das Filtern zu vereinfachen.

```csharp
public ActionResult Delete(Guid? id)
{
    System.Diagnostics.Trace.TraceInformation("GET /Todos/Delete/" + id);
    if (id == null)
    {
        System.Diagnostics.Trace.TraceError("/Todos/Delete/ failed, ID is null");
        return new HttpStatusCodeResult(HttpStatusCode.BadRequest);
    }
    Todo todo = db.Todoes.Find(id);
    if (todo == null)
    {
        System.Diagnostics.Trace.TraceWarning("/Todos/Delete/ failed, ID: " + id + " could not be found");
        return HttpNotFound();
    }
    System.Diagnostics.Trace.TraceInformation("GET /Todos/Delete/" + id + "completed successfully");
    return View(todo);
}
```
Um die Anwendungsprotokollierung zu aktivieren, wechseln Sie zu **Überwachung** > **Diagnoseprotokolle**, und aktivieren Sie die Anwendungsprotokollierung mithilfe der Schalter.

![App überwachen](media/app-service-web-tutorial-monitoring/app-service-monitor-applogs.png)

Anwendungsprotokolle können im Dateisystem der Web-App gespeichert oder per Push in Blob Storage übertragen werden. Für Produktionsszenarien empfiehlt sich die Verwendung von Blob Storage.

> [!IMPORTANT]
> Das Aktivieren der Protokollierung wirkt sich auf die Leistung der Anwendung und die Ressourcennutzung aus. Für Produktionsszenarien werden Fehlerprotokolle empfohlen. Aktivieren Sie die ausführliche Protokollierung nur, wenn Sie Fehler untersuchen.

 ### <a name="web-server-diagnostics"></a>Webserverdiagnose
Webserverprotokolle werden auch dann generiert, wenn Ihre App nicht genutzt wird. In App Service können drei verschiedene Typen von Serverprotokollen erfasst werden:

- **Webserverprotokollierung** 
    - Informationen über HTTP-Transaktionen im [erweiterten W3C-Protokolldateiformat](https://msdn.microsoft.com/library/windows/desktop/aa814385.aspx). 
    - Dies ist hilfreich, wenn Sie allgemeine Website-Metriken ermitteln möchten, z.B. die Anzahl der verarbeiteten Anfragen oder wie viele Anforderungen von einer bestimmten IP-Adresse stammen.
- **Detaillierte Fehlerprotokollierung** 
    - Detaillierte Fehlerinformationen für HTTP-Statuscodes, die auf einen Fehler hinweisen (Statuscode 400 oder höher). 
    - [Weitere Informationen über detaillierte Fehlerprotokolle](https://www.iis.net/learn/troubleshoot/diagnosing-http-errors/how-to-use-http-detailed-errors-in-iis)
- **Ablaufverfolgung von Anforderungsfehlern** 
    - Detaillierte Informationen zu fehlgeschlagenen Anforderungen, einschließlich der Verfolgung von IIS-Komponenten, die zur Verarbeitung der Anforderung verwendet wurden, sowie die in jeder Komponente benötigte Zeit. 
    - Protokolle zu Anforderungsfehlern sind hilfreich, um die Ursache eines bestimmten HTTP-Fehlers zu isolieren.
    - [Weitere Informationen über Fehler bei der Ablaufverfolgung von Anforderungen](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis)

So aktivieren Sie die Serverprotokotollierung
- Wechseln Sie zu **Überwachung** > **Diagnoseprotokolle**. 
- Aktivieren Sie die verschiedenen Typen der Webserverdiagnose mithilfe der Schalter.

![App überwachen](media/app-service-web-tutorial-monitoring/app-service-monitor-serverlogs.png)

> [!IMPORTANT]
> Das Aktivieren der Protokollierung wirkt sich auf die Leistung der Anwendung und die Ressourcennutzung aus. Für Produktionsszenarien werden Fehlerprotokolle empfohlen. Aktivieren Sie die ausführliche Protokollierung nur, wenn Sie Fehler untersuchen.

### <a name="accessing-logs"></a>Zugriff auf Protokolle
Sie können auf in Blob Storage gespeicherte Protokolle mit dem Azure Storage-Explorer zugreifen. Auf Protokolle, die im Dateisystem der Web-App gespeichert wurden, können Sie unter den folgenden Pfaden über FTP zugreifen:

- **Anwendungsprotokolle** - `%HOME%/LogFiles/Application/`.
    - Dieser Ordner enthält eine oder mehrere Textdateien mit Informationen, die bei der Anwendungsprotokollierung erzeugt wurden.
- **Protokolle für Anforderungsfehler** - `%HOME%/LogFiles/W3SVC#########/`. 
    - Dieser Ordner enthält eine XSL-Datei und eine oder mehrere XML-Dateien. 
- **Detaillierte Fehlerprotokolle** - `%HOME%/LogFiles/DetailedErrors/`. 
    - Dieser Ordner enthält eine oder mehrere HTM-Dateien mit umfangreichen Informationen zu HTTP-Fehlern, die von der App generiert wurden.
- **Webserverprotokolle** - `%HOME%/LogFiles/http/RawLogs`. 
    - Dieser Ordner enthält eine oder mehrere Textdateien im erweiterten W3C-Protokolldateiformat.

## <a name="streaming"></a> Schritt 6: Protokollstreaming
Die Streamingprotokolle sind beim Debuggen einer Anwendung hilfreich, da Sie gegenüber dem [Zugriff auf die Protokolle](#Accessing-Logs) über FTP Zeit sparen.

App Service kann **Anwendungsprotokolle** und **Webserverprotokolle** streamen, sobald sie generiert werden. 

> [!TIP]
> Bevor Sie versuchen, Protokolle zu streamen, stellen Sie sicher, dass das Erfassen von Protokollen wie im Abschnitt zur [Protokollierung](#logging) beschrieben aktiviert wurde.

Um Protokolle zu streamen, wechseln Sie zu **Überwachung**> **Protokollstream**. Wählen Sie **Anwendungsprotokolle** bzw. **Webserverprotokolle** aus. Hier können Sie auch den Puffer anhalten, neu starten und löschen.

![Streamingprotokolle](media/app-service-web-tutorial-monitoring/app-service-monitor-logstream.png)

> [!TIP]
> Protokolle werden nur generiert, wenn Datenverkehr bei der App vorhanden ist. Sie können die Ausführlichkeit der Protokolle erhöhen, um mehr Ereignisse oder Informationen abzurufen.

## <a name="remote"></a> Schritt 7: Remotedebuggen
Nachdem Sie die Quelle der Anwendungsprobleme ausfindig gemacht haben, verwenden Sie **Remotedebuggen**, um den Code zu durchlaufen.

Remotedebuggen ermöglicht Ihnen, an Ihre in der Cloud ausgeführte Web-App einen Debugger anzufügen. Im Debugmodus können Sie wie für eine lokal ausgeführte App Breakpoints setzen, den Speicher direkt manipulieren, Code schrittweise durchlaufen und sogar den Codepfad ändern.

So fügen Sie den Debugger an Ihre in der Cloud ausgeführte App an

- Öffnen Sie mit Visual Studio 2017 die Lösung für die App, die Sie debuggen möchten. 
- Legen Sie wie bei der lokalen Entwicklung einige Breakpoints fest.
- Öffnen Sie den **Cloud-Explorer** (STRG + /, STRG + X).
- Melden Sie sich ggf. mit Ihren Azure-Anmeldeinformationen an.
- Suchen Sie die App, die Sie debuggen möchten.
- Wählen Sie im Bereich **Aktionen** die Option **Debugger anfügen** aus.

![Remotedebugging](media/app-service-web-tutorial-monitoring/app-service-monitor-vsdebug.png)

Visual Studio konfiguriert die Anwendung für das Remotedebuggen, öffnet ein Browserfenster und navigiert zur App. Sie können die App durchlaufen, um Breakpoints auszulösen und den Code durchzugehen.

> [!WARNING]
> Vermeiden Sie es, den Debugmodus in Produktion einzusetzen. Wenn Ihre Produktions-App nicht auf mehrere Serverinstanzen dezentral skaliert wird, verhindert das Debuggen, dass der Webserver auf andere Anforderungen reagiert. Die beste Möglichkeit zur Problembehandlung von Produktionsproblemen sind das [Konfigurieren der Protokollierung](#logging) und [Application Insights](#insights).



## <a name="explorer"></a> Schritt 8: Prozess-Explorer
Wenn Ihre Anwendung für mehrere Instanzen horizontal hochskaliert ist, kann **Prozess-Explorer** Ihnen dabei helfen, instanzspezifische Probleme zu identifizieren.

Mit **Prozess-Explorer** haben Sie folgende Möglichkeiten:

- Auflisten aller Prozesse in verschiedenen Instanzen Ihres App Service-Plans
- Drilldown und Anzeigen der den einzelnen Prozessen zugeordneten Handles und Module 
- Anzeigen von Werten zu CPU, Arbeitssätzen und Threadanzahl auf Prozessebene zum Erkennen von unkontrollierten Prozessen
- Suchen von Handles auf offene Dateien und Beenden bestimmter Prozessinstanzen

Prozess-Explorer ist unter **Überwachung** > **Prozess-Explorer** zu finden.

![Prozess-Explorer](media/app-service-web-tutorial-monitoring/app-service-monitor-processexplorer.png)


## <a name="insights"></a> Schritt 9: Application Insights
**Application Insights** bietet Anwendungsprofile und erweiterte Überwachungsfunktionen für Ihre App. 

Mit Application Insights können Sie Ausnahme und Leistungsprobleme bei Ihrer Web-App erkennen und diagnostizieren.

Sie können Application Insights für Ihre Web-App unter **Überwachung** > **Application Insights** aktivieren. 

> [!NOTE]
> Möglicherweise werden Sie von Application Insights aufgefordert, die Application Insights-Websiteerweiterung zu installieren, um mit dem Erfassen von Daten zu beginnen. Die Installation der Websiteerweiterung führt zu einem Neustart der Anwendung.

![Application Insights](media/app-service-web-tutorial-monitoring/app-service-monitor-appinsights.png)

Application Insights verfügt über eine Vielzahl von Funktionen. Weitere Informationen finden Sie unter den Links im Abschnitt [Nächste Schritte](#next).

## <a name="next"></a> Nächste Schritte

 - [Was ist Application Insights?](..\application-insights\app-insights-overview.md)
 - [Überwachen der Leistung der Azure-Web-App mit Application Insights](..\application-insights\app-insights-azure-web-apps.md)
 - [Überwachen der Verfügbarkeit und Reaktionsfähigkeit von Websites mit Application Insights](..\application-insights\app-insights-monitor-web-app-availability.md)
