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
ms.sourcegitcommit: e851a3e1b0598345dc8bfdd4341eb1dfb9f6fb5d
ms.openlocfilehash: 63bfc6922de224f56186003991f4a51d8f99ed35
ms.lasthandoff: 04/15/2017

---
# <a name="monitor-app-service"></a>Überwachen des App Service
In diesem Lernprogramm wird gezeigt, wie Sie mit den integrierten Plattformtools eine in App Service gehostete App überwachen und diagnostizieren. 

## <a name="in-this-tutorial"></a>Dieses Lernprogramm umfasst folgende Punkte

1. [Prozess-Explorer](#explorer)
    - Hiermit erhalten Sie ausführliche Informationen über eine App, die über mehrere Instanzen des App Service-Plans ausgeführt wird.
1. [App Service-Metriken](#metrics) 
   - Erfahren Sie, wie Sie Ihre App mithilfe der integrierten Diagramme überwachen können.
   - Konfigurieren von Diagrammen entsprechend Ihren Anforderungen
   - Erstellen eines benutzerdefinierten Dashboards durch Anheften von benutzerdefinierten Diagrammen
1. [Konfigurieren von Warnungen](#alerts)
    - Informationen zum Konfigurieren von Warnungen für die App und den App Service-Plan
1. [App Service Companion](#Companion)
    - Überwachung und Problembehandlung für Ihre App mit einem Mobilgerät.
1. [Konfigurieren der Protokollierung](#logging)
    - Informationen zum Erfassen von Server- und Anwendungsprotokollen
    - Informationen zu den verschiedenen Speicherorten für Protokolle und wie sie aufgerufen werden
1. [Protokollstreaming](#streaming)
    - Mit Streamingprotokollen können Sie Ihre Anwendungs- und wc3-Protokolle anzuzeigen, sobald sie ausgegeben werden.
1. [Remotedebuggen](#remote)
    - Mit Visual Studio können Sie ein im App Service ausgeführtes Projekt remote debuggen.
1. [Diagnose und Problembehandlung](#diagnose)
    - Ermitteln von Problemen mit der Anwendung und Abrufen von Informationen zu deren Lösung
1. [Application Insights](#insights)
    - Erweiterte Profilerstellung und Überwachung für Ihre App

## <a name="before-you-begin"></a>Voraussetzungen

- Sie benötigen eine Web-App zum Überwachen und zum Ausführen der beschriebenen Schritte. 
    - Mit den im Lernprogramm zum [Erstellen einer ASP.NET-App in Azure mit SQL-Datenbank](app-service-web-tutorial-dotnet-sqldatabase.md) beschriebenen Schritten können Sie eine Anwendung erstellen.

- Wenn Sie das **Remotedebuggen** Ihrer Anwendung testen möchten, benötigen Sie Visual Studio. 
    - Falls Sie Visual Studio 2017 noch nicht installiert haben, können Sie die kostenlose [Visual Studio 2017 Community-Edition](https://www.visualstudio.com/downloads/) herunterladen und verwenden. 
    - Aktivieren Sie beim Setup von Visual Studio die Option **Azure-Entwicklung**.

## <a name="explorer"></a> Schritt 1:+ Prozess-Explorer

Prozess-Explorer ist ein Tool, mit dem Sie ausführliche Informationen zur Funktionsweise des App Service-Plans abrufen können.

Mit **Prozess-Explorer** haben Sie folgende Möglichkeiten:

- Auflisten aller Prozesse in verschiedenen Instanzen Ihres App Service-Plans
- Drilldown und Anzeigen der den einzelnen Prozessen zugeordneten Handles und Module 
- Anzeigen von Werten zu CPU, Arbeitssätzen und Threadanzahl auf Prozessebene zum Erkennen von unkontrollierten Prozessen
- Suchen von Handles auf offene Dateien und Beenden bestimmter Prozessinstanzen

Der Prozess-Explorer kann über **Überwachung** > **Prozess-Explorer** geöffnet werden.

![Prozess-Explorer](media/app-service-web-tutorial-monitoring/app-service-monitor-processexplorer.png)

## <a name="metrics"></a> Schritt 2: Anzeigen von App Service-Metriken
**Metriken** bieten detaillierte Informationen zu Ihrer Web-App sowie deren Interaktionen mit Benutzern und der Plattform.

Mit Metriken erhalten Sie Einblicke in:
- die Menge der von Ihrer App verwendeten Ressourcen
- das Datenverkehrsvolumen für Ihre App
- allgemeine Anforderungen/Fehler
- Volumen der ein- und ausgehenden Daten

Für jede im App Service gehostete App müssen Sie die Web-App und den App Service-Plan überwachen.

- **App**-Metriken umfassen Informationen zu HTTP-Anforderungen/-Fehlern und der durchschnittlichen Antwortzeit.
- Metriken zum **App Service-Plan** liefern Informationen Ressourcennutzung.

Das Azure-Portal verfügt mit dem **Azure Monitor** über eine schnelle Möglichkeit zum Anzeigen der Metriken für die App.

- Wechseln Sie zum Blatt **Übersicht** für die App, die Sie überwachen möchten.

![App überwachen](media/app-service-web-tutorial-monitoring/app-service-monitor.png)

- Auf der Kachel **Überwachung** können Sie die Metriken für die App anzeigen.
- Klicken Sie auf die Kachel, um zu bearbeiten und zu konfigurieren, welche Metriken und welcher Zeitbereich angezeigt werden sollen.

![Diagramm konfigurieren](media/app-service-web-tutorial-monitoring/app-service-monitor-configure.png)

- Sie können benutzerdefinierte Diagramme an das Dashboard anheften und so schnell darauf zugreifen.

![Diagramm anheften](media/app-service-web-tutorial-monitoring/app-service-monitor-pin.png)

> [!TIP]
> Unter folgenden Links finden Sie weitere Informationen zu Azure Monitor:
> - [Erste Schritte mit Azure Monitor](..\monitoring-and-diagnostics\monitoring-overview.md)
> - [Metriken in Azure](..\monitoring-and-diagnostics\monitoring-overview-metrics.md)
> - [Unterstützte Metriken von Azure Monitor](..\monitoring-and-diagnostics\monitoring-supported-metrics.md#microsoftwebsites-including-functions)
> - [Dashboards im Azure-Portal](..\azure-portal\azure-portal-dashboards.md)

## <a name="alerts"></a> Schritt 3: Konfigurieren von Warnungen

**Warnungen** ermöglichen es Ihnen, die Überwachung Ihrer Anwendung zu automatisieren.

Verwenden Sie Warnungen, damit Sie benachrichtigt werden, wenn bestimmte Ereignisse, die Auswirkungen auf Ihre App haben, erkannt werden.

So erstellen Sie eine Warnung
- Wechseln Sie zum Blatt **Übersicht** für die App, die Sie überwachen möchten.
- Navigieren Sie im Menü zu **Überwachung** > **Warnungen**.
- Wählen Sie **[+] Warnung hinzufügen** aus.
- Konfigurieren Sie die Warnung wie gewünscht.

![Warnungen](media/app-service-web-tutorial-monitoring/app-service-monitor-alerts.png)

> [!TIP]
> Unter folgenden Links finden Sie weitere Informationen zu Warnungen in Azure:
> - [Was sind Warnungen in Microsoft Azure?](..\monitoring-and-diagnostics\monitoring-overview-alerts.md)
> - [Ausführen von Aktionen für Metriken](..\monitoring-and-diagnostics\monitoring-overview.md)

## <a name="companion"></a> Schritt 4: App Service Companion
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

## <a name="logging"></a> Schritt 5: Protokollierung
Durch die Protokollierung können Sie Protokolle zu **Anwendungsdiagnosen** und **Webserverdiagnosen** für Ihre Web-App erfassen.

Mit Diagnoseprotokollen können Sie das Verhalten der Anwendung nachvollziehen, Probleme mit der Anwendung beheben und Fehlerbedingungen verstehen.

### <a name="application-diagnostics"></a>Anwendungsdiagnose
Mit der Option „Anwendungsdiagnose“ können Sie die von der Anwendung zur Laufzeit erzeugten Ablaufverfolgungen erfassen. 

So aktivieren Sie die Anwendungsprotokollierung

- Wechseln Sie zu **Überwachung** > **Diagnoseprotokolle**. 
- Aktivieren Sie die Anwendungsprotokollierung mit den entsprechenden Schaltern.

Anwendungsprotokolle können im Dateisystem der Web-App gespeichert oder per Push in Blob Storage übertragen werden.

> [!TIP]
> Für Produktionsszenarien empfiehlt sich die Verwendung von Blob Storage.

![App überwachen](media/app-service-web-tutorial-monitoring/app-service-monitor-applogs.png)

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

> [!IMPORTANT]
> Das Aktivieren der Protokollierung wirkt sich auf die Leistung der Anwendung und die Ressourcennutzung aus. Für Produktionsszenarien werden Fehlerprotokolle empfohlen. Aktivieren Sie die ausführliche Protokollierung nur, wenn Sie Fehler untersuchen.

 ### <a name="web-server-diagnostics"></a>Webserverdiagnose
In App Service können drei verschiedene Typen von Serverprotokollen erfasst werden:

- **Webserverprotokollierung** 
    - Informationen über HTTP-Transaktionen im [erweiterten W3C-Protokolldateiformat](https://msdn.microsoft.com/library/windows/desktop/aa814385.aspx). 
    - Dies ist hilfreich, wenn Sie allgemeine Website-Metriken ermitteln möchten, z.B. die Anzahl der verarbeiteten Anfragen oder wie viele Anforderungen von einer bestimmten IP-Adresse stammen.
- **Detaillierte Fehlerprotokollierung** 
    - Detaillierte Fehlerinformationen für HTTP-Statuscodes, die auf einen Fehler hinweisen (Statuscode 400 oder höher). 
- **Ablaufverfolgung von Anforderungsfehlern** 
    - Detaillierte Informationen zu fehlgeschlagenen Anforderungen, einschließlich der Verfolgung von IIS-Komponenten, die zur Verarbeitung der Anforderung verwendet wurden, sowie die in jeder Komponente benötigte Zeit. 
    - Protokolle zu Anforderungsfehlern sind hilfreich, um die Ursache eines bestimmten HTTP-Fehlers zu isolieren.

So aktivieren Sie die Serverprotokotollierung
- Wechseln Sie zu **Überwachung** > **Diagnoseprotokolle**. 
- Aktivieren Sie die verschiedenen Typen der Webserverdiagnose mithilfe der Schalter.

![App überwachen](media/app-service-web-tutorial-monitoring/app-service-monitor-serverlogs.png)

> [!IMPORTANT]
> Das Aktivieren der Protokollierung wirkt sich auf die Leistung der Anwendung und die Ressourcennutzung aus. Für Produktionsszenarien werden Fehlerprotokolle empfohlen. Aktivieren Sie die ausführliche Protokollierung nur, wenn Sie Fehler untersuchen.

### <a name="accessing-logs"></a>Zugriff auf Protokolle
Sie können auf in Blob Storage gespeicherte Protokolle mit dem Azure Storage-Explorer zugreifen.

Auf Protokolle, die im Dateisystem der Web-App gespeichert wurden, können Sie unter den folgenden Pfaden über FTP zugreifen:

- **Anwendungsprotokolle** - /LogFiles/Application/. 
    - Dieser Ordner enthält eine oder mehrere Textdateien mit Informationen, die bei der Anwendungsprotokollierung erzeugt wurden.
- **Protokolle für fehlgeschlagene Anforderungen** - /LogFiles/W3SVC#########/. 
    - Dieser Ordner enthält eine XSL-Datei und eine oder mehrere XML-Dateien. 
- **Detaillierte Fehlerprotokolle** - /LogFiles/DetailedErrors/. 
    - Dieser Ordner enthält eine oder mehrere HTM-Dateien mit umfangreichen Informationen zu HTTP-Fehlern, die von der App generiert wurden.
- **Webserverprotokolle** - /LogFiles/http/RawLogs. 
    - Dieser Ordner enthält eine oder mehrere Textdateien im erweiterten W3C-Protokolldateiformat.

## <a name="streaming"></a> Schritt 6: Protokollstreaming
App Service kann **Anwendungsprotokolle** und **Webserverprotokolle** streamen, sobald sie generiert werden. 

Die Streamingprotokolle sind beim Debuggen einer Anwendung hilfreich, da Sie gegenüber dem Zugriff auf die Protokolle über FTP oder andere Methoden Zeit sparen.

> [!TIP]
> Bevor Sie versuchen, Protokolle zu streamen, stellen Sie sicher, dass das Erfassen von Protokollen wie im Abschnitt zur [Protokollierung](#logging) beschrieben aktiviert wurde.

So streamen Sie Protokolle
- Wechseln Sie zu **Überwachung**> **Protokollstream**
- Wählen Sie **Anwendungsprotokolle** bzw. **Webserverprotokolle** aus.
- Hier können Sie auch den Puffer anhalten, neu starten und löschen.

![Streamingprotokolle](media/app-service-web-tutorial-monitoring/app-service-monitor-logstream.png)

> [!TIP]
> Protokolle werden nur generiert, wenn Datenverkehr bei der App vorhanden ist. Sie können die Ausführlichkeit der Protokolle erhöhen, um mehr Ereignisse oder Informationen abzurufen.

## <a name="remote"></a> Schritt 7: Remotedebuggen
Mit **Remotedebuggen** können Sie einen Debugger an Ihre in der Cloud ausgeführte Web-App anfügen. Im Debugmodus können Sie wie für eine lokal ausgeführte App Breakpoints setzen, den Speicher direkt manipulieren, Code schrittweise durchlaufen und sogar den Codepfad ändern.

Verwenden Sie das Remotedebuggen zusammen mit Diagnoseprotokollen, um Probleme mit der Anwendung zu ermitteln und zu beheben.

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

## <a name="diagnose"></a> Schritt 8: Diagnose und Problembehandlung
**Diagnose und Problembehandlung** ist ein integriertes Tool, das die letzten 24 Stunden Aktivität für Ihre Web-App durchsucht. Die Benutzeroberfläche enthält eine Zusammenfassung der ermittelten Probleme.

Mit dieser Funktion können Sie Anwendungsprobleme von Plattformproblemen unterscheiden und mögliche Lösungen ermitteln, um Ihre Web-App wieder fehlerfrei auszuführen.

![Diagnose und Problembehandlung](media/app-service-web-tutorial-monitoring/app-service-monitor-diagnosis.png)

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
