---
title: "Diagnostizieren von Leistungsproblemen auf einer ausgeführten IIS-Website | Microsoft Docs"
description: "Überwachen Sie die Leistung einer Website, ohne sie erneut bereitzustellen. Zur eigenständigen Verwendung oder in Verbindung mit dem Application Insights-SDK zum Abrufen von Telemetriedaten zu Abhängigkeiten."
services: application-insights
documentationcenter: .net
author: alancameronwills
manager: douge
ms.assetid: 769a5ea4-a8c6-4c18-b46c-657e864e24de
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/24/2016
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: 15de13cfaec7a5dcd906733727bb1f826893a31b
ms.openlocfilehash: 61a2aca74f9d422550b90d218c5e060c89e354ad


---
# <a name="instrument-web-apps-at-runtime-with-application-insights"></a>Instrumentieren von Web-Apps zur Laufzeit mit Application Insights


Sie können eine Live-Web-App mit Azure Application Insights instrumentieren, ohne dass Sie Ihren Code ändern oder erneut bereitstellen müssen. Falls Ihre Apps von einem lokalen IIS-Server gehostet werden, installieren Sie den Statusmonitor. Falls es sich um Azure-Web-Apps handelt oder die Apps auf einem virtuellen Azure-Computer ausgeführt werden, können Sie die Application Insights-Erweiterung installieren. (Es gibt auch separate Artikel zum Instrumentieren von [Live-J2EE-Web-Apps](app-insights-java-live.md) und [Azure Cloud Services](app-insights-cloudservices.md).)

![Beispieldiagramme](./media/app-insights-monitor-performance-live-website-now/10-intro.png)

Bei der Anwendung von Application Insights auf die .NET-Webanwendungen können Sie zwischen drei Möglichkeiten wählen:

* **Buildzeit:** [Fügen Sie Ihrem Web-App-Code das Application Insights-SDK hinzu][greenbrown].
* **Laufzeit:** Instrumentieren Sie Ihre Web-App auf dem Server wie unten beschrieben, ohne den Code neu zu erstellen und bereitzustellen.
* **Beides:** Erstellen Sie das SDK in Ihrem Web-App-Code, und wenden Sie auch die Laufzeiterweiterungen an. So können Sie die Vorteile beider Optionen nutzen.

Hier ist zusammengefasst, was Sie jeweils erhalten:

|  | Buildzeit | Laufzeit |
| --- | --- | --- |
| Anforderungen und Ausnahmen |Ja |Ja |
| [Weitere ausführliche Ausnahmen](app-insights-asp-net-exceptions.md) | |Ja |
| [Abhängigkeitsdiagnose](app-insights-asp-net-dependencies.md) |Bei .NET 4.6 und höheren Versionen, aber weniger Details |Ja, vollständige Details: Ergebniscodes, SQL-Befehlstext, HTTP-Verb|
| [Systemleistungsindikatoren](app-insights-performance-counters.md) |Ja |Ja |
| [API für benutzerdefinierte Telemetrie][api] |Ja | |
| [Ablaufverfolgungsprotokoll-Integration](app-insights-asp-net-trace-logs.md) |Ja | |
| [Seitenansicht und Benutzerdaten](app-insights-javascript.md) |Ja | |
| Neuerstellung des Codes nicht erforderlich |Nein | |

## <a name="instrument-your-web-app-at-run-time"></a>Instrumentieren der Web-App zur Laufzeit
Sie benötigen ein [Microsoft Azure](http://azure.com) -Abonnement.

### <a name="if-your-app-is-an-azure-web-app-or-cloud-service"></a>Wenn Ihre App eine Azure-Web-App oder ein Clouddienst ist
* Wählen Sie in der Systemsteuerung der App in Azure „Application Insights“ aus.

    [detaillierte Kapazitätsplanung](app-insights-azure.md)

### <a name="if-your-app-is-hosted-on-your-iis-server"></a>Bei auf dem IIS-Server gehosteter App
1. Melden Sie sich mit Administratorrechten auf Ihrem IIS-Webserver an.
2. Laden Sie den [Statusmonitor-Installer](http://go.microsoft.com/fwlink/?LinkId=506648)herunter, und starten Sie ihn.  
3. Wählen Sie die installierte Webanwendung oder Website aus, die Sie überwachen möchten, und konfigurieren Sie die Ressource, in der die Ergebnisse im Application Insights-Portal angezeigt werden sollen. Die Anmeldung an Microsoft Azure sollte durchgeführt werden.

    ![Wählen Sie eine App und eine Ressource.](./media/app-insights-monitor-performance-live-website-now/appinsights-036-configAIC.png)

    In der Regel konfigurieren Sie eine neue Ressource und [Ressourcengruppe][roles].

    Sie können andernfalls auch eine vorhandene Ressource verwenden, wenn Sie bereits [Webtests][availability] für Ihre Website oder die [Webclientüberwachung][client] eingerichtet haben.
4. Starten Sie IIS neu.

    ![Wählen Sie im oberen Bereich des Dialogfelds "Neustart".](./media/app-insights-monitor-performance-live-website-now/appinsights-036-restart.png)

    Der Webdienst wird für kurze Zeit unterbrochen.
5. Beachten Sie, dass die Datei "ApplicationInsights.config" in die Web-Apps eingefügt wurde, die Sie überwachen möchten.

    ![Suchen Sie die CONFIG-Datei bei den Codedateien der Web-App.](./media/app-insights-monitor-performance-live-website-now/appinsights-034-aiconfig.png)
   

#### <a name="want-to-reconfigure-later"></a>Möchten Sie die Konfiguration später ändern?
Nach Abschluss des Assistenten können Sie die Konfiguration des Agenten später jederzeit ändern. Sie können dies auch verwenden, wenn bei der ursprünglichen Einrichtung des Agenten ein Problem aufgetreten ist.

![Klicken Sie auf das Application Insights-Symbol in der Taskleiste](./media/app-insights-monitor-performance-live-website-now/appinsights-033-aicRunning.png)

## <a name="view-performance-telemetry"></a>Anzeigen von Leistungstelemetriedaten
Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an, navigieren Sie zu Application Insights, und öffnen Sie die zuvor erstellte Ressource.

![Wählen Sie "Durchsuchen", "Application Insights" und dann Ihre App aus.](./media/app-insights-monitor-performance-live-website-now/appinsights-08openApp.png)

Öffnen Sie das Blatt "Leistung", um die Anforderung, Antwortzeit, Abhängigkeiten und andere Daten anzuzeigen.

![Leistung](./media/app-insights-monitor-performance-live-website-now/21-perf.png)

Klicken Sie jeweils auf ein Diagramm, um eine ausführlichere Ansicht zu öffnen.

Sie können Diagramme [bearbeiten, neu anordnen und speichern](app-insights-metrics-explorer.md) und Diagramme oder das gesamte Blatt an ein [Dashboard](app-insights-dashboards.md) anheften.

## <a name="dependencies"></a>Abhängigkeiten
Das Diagramm mit der Abhängigkeitsdauer zeigt die Dauer, die Ihre App für Aufrufe externer Komponenten (z. B. Datenbanken, REST-APIs oder Azure-BLOB-Speicher) benötigt.

Gehen Sie wie folgt vor, um das Diagramm nach Aufrufen verschiedener Abhängigkeiten zu segmentieren: Bearbeiten Sie das Diagramm, aktivieren Sie „Gruppierung“, und wählen Sie dann die Gruppierung nach „Abhängigkeit“, „Abhängigkeitstyp“ oder „Abhängigkeitsleistung“ aus.

![Abhängigkeit](./media/app-insights-monitor-performance-live-website-now/23-dep.png)

## <a name="performance-counters"></a>Leistungsindikatoren
Klicken Sie auf dem Übersichtsblatt auf "Server", um die Diagramme von Serverleistungsindikatoren wie CPU- und Speicherauslastung anzuzeigen.

Wenn Sie über mehrere Serverinstanzen verfügen, kann es ratsam sein, die Diagramme so zu bearbeiten, dass sie nach der Rolleninstanz gruppiert werden.

![Server](./media/app-insights-monitor-performance-live-website-now/22-servers.png)

Sie können auch die Leistungsindikatoren ändern, die vom SDK gemeldet werden. 

## <a name="exceptions"></a>Ausnahmen
![Klicken Sie sich durch das Diagramm der Serverausnahmen](./media/app-insights-monitor-performance-live-website-now/appinsights-039-1exceptions.png)

Sie können Detailinformationen zu bestimmten Ausnahmen (der letzten sieben Tage) anzeigen sowie Stapelüberwachung und Kontextdaten abrufen.

## <a name="sampling"></a>Stichproben
Wenn Ihre Anwendung eine große Menge von Daten sendet und Sie das Application Insights-SDK für ASP.NET Version 2.0.0-beta3 oder höher verwenden, wird möglicherweise die adaptive Stichprobenerstellung verwendet, bei der nur ein bestimmter Prozentsatz der Telemetriedaten übermittelt wird. [Erfahren Sie mehr über das Erstellen von Stichproben.](app-insights-sampling.md)

## <a name="troubleshooting"></a>Problembehandlung
### <a name="connection-errors"></a>Verbindungsfehler
Sie müssen [einige ausgehende Ports](app-insights-ip-addresses.md#outgoing-ports) in der Firewall des Servers öffnen, damit der Statusmonitor funktioniert.

### <a name="no-telemetry"></a>Keine Telemetriedaten?
* Verwenden Sie Ihre Website, um einige Daten zu generieren.
* Warten Sie einige Minuten, bis die Daten empfangen wurden, und klicken Sie dann auf **Aktualisieren**.
* Öffnen Sie die "Diagnosesuche" (die Kachel "Search"), um einzelne Ereignisse anzuzeigen. Ereignisse werden häufig in der Diagnosesuche angezeigt, bevor Aggregatdaten in den Diagrammen dargestellt werden.
* Öffnen Sie den Statusmonitor, und wählen Sie Ihre Anwendung auf der linken Seite aus. Prüfen Sie, ob für diese Anwendung Diagnosemeldungen im Abschnitt mit den Konfigurationsbenachrichtigungen vorliegen:

  ![Öffnen Sie das Blatt „Leistung“, um die Anforderung, Antwortzeit, Abhängigkeiten und andere Daten anzuzeigen.](./media/app-insights-monitor-performance-live-website-now/appinsights-status-monitor-diagnostics-message.png)
* Stellen Sie sicher, dass die Firewall Ihres Servers ausgehenden Datenverkehr an die oben aufgeführten Ports erlaubt.
* Wenn auf dem Server eine Meldung über "unzureichende Berechtigungen" angezeigt wird, versuchen Sie Folgendes:
  * Wählen Sie in IIS-Manager Ihren Anwendungspool aus, öffnen Sie **Erweiterte Einstellungen**, und überprüfen Sie die Identität unter **Prozessmodell**.
  * Fügen Sie in der Systemsteuerung "Computerverwaltung" diese Identität der Gruppe "Systemmonitorbenutzer" hinzu.
* Wenn Sie MMA/SCOM auf Ihrem Server installiert haben, können einige Versionen in Konflikt stehen. Deinstallieren Sie sowohl SCOM als auch den Statusmonitor, und installieren Sie die neuesten Versionen.
* Informationen hierzu finden Sie unter [Problembehandlung][qna].

## <a name="system-requirements"></a>Systemanforderungen
Betriebssystemunterstützung für den Application Insights-Statusmonitor auf dem Server:

* Windows Server 2008
* Windows Server 2008 R2
* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016

mit den neuesten Service Packs und .NET Framework 4.5

Auf der Clientseite: Windows 7, 8, 8.1 und 10, ebenfalls mit .NET Framework 4.5

IIS-Unterstützung: IIS 7, 7.5, 8, 8.5 (IIS ist erforderlich)

## <a name="automation-with-powershell"></a>Automation mit PowerShell
Sie können die Überwachung mit PowerShell auf Ihrem IIS-Server starten und beenden.

Importieren Sie zunächst das Application Insights-Modul:

`Import-Module 'C:\Program Files\Microsoft Application Insights\Status Monitor\PowerShell\Microsoft.Diagnostics.Agent.StatusMonitor.PowerShell.dll'`

Ermitteln Sie, welche Apps überwacht werden:

`Get-ApplicationInsightsMonitoringStatus [-Name appName]`

* `-Name` (Optional) Dies ist der Name einer Web-App.
* Zeigt den Application Insights-Überwachungsstatus für jede Web-App (oder die benannte App) in diesem IIS-Server an.
* Gibt `ApplicationInsightsApplication` für jede App zurück:

  * `SdkState==EnabledAfterDeployment`: Die App wird überwacht und wurde zur Laufzeit instrumentiert, und zwar entweder mit dem Statusmonitor-Tool oder mit `Start-ApplicationInsightsMonitoring`.
  * `SdkState==Disabled`: Die App ist für Application Insights nicht instrumentiert. Entweder wurde sie nie instrumentiert, oder die Laufzeitüberwachung wurde mit dem Statusmonitor-Tool oder mit `Stop-ApplicationInsightsMonitoring`deaktiviert.
  * `SdkState==EnabledByCodeInstrumentation`: Die App wurde instrumentiert, indem das SDK dem Quellcode hinzugefügt wurde. Das SDK kann nicht aktualisiert oder beendet werden.
  * `SdkVersion` wird die Version angezeigt, die für die Überwachung dieser App verwendet wird.
  * Mit `LatestAvailableSdkVersion` wird die Version angezeigt, die im NuGet-Katalog derzeit verfügbar ist. Verwenden Sie zum Aktualisieren der App auf diese Version `Update-ApplicationInsightsMonitoring`.

`Start-ApplicationInsightsMonitoring -Name appName -InstrumentationKey 00000000-000-000-000-0000000`

* `-Name` Gibt den Namen der App in IIS an.
* `-InstrumentationKey` Dies ist der iKey der Application Insights-Ressource, für die die Ergebnisse angezeigt werden sollen.
* Dieses Cmdlet wirkt sich nur auf Apps aus, die nicht bereits instrumentiert wurden (SdkState==NotInstrumented).

    Das Cmdlet wirkt sich nicht auf eine App aus, die bereits instrumentiert wurde, und zwar entweder zur Erstellungszeit, indem dem Code das SDK hinzugefügt wurde, oder zur Laufzeit bei einer vorherigen Nutzung dieses Cmdlets.

    Die zum Instrumentieren der App verwendete SDK-Version ist die Version, die zuletzt auf diesen Server heruntergeladen wurde.

    Verwenden Sie zum Herunterladen der aktuellen Version „Update-ApplicationInsightsVersion“.
* Es wird `ApplicationInsightsApplication` zurückgegeben, wenn der Vorgang erfolgreich ist. Wenn nicht, wird unter stderr eine Ablaufverfolgung protokolliert.

          Name                      : Default Web Site/WebApp1
          InstrumentationKey        : 00000000-0000-0000-0000-000000000000
          ProfilerState             : ApplicationInsights
          SdkState                  : EnabledAfterDeployment
          SdkVersion                : 1.2.1
          LatestAvailableSdkVersion : 1.2.3

`Stop-ApplicationInsightsMonitoring [-Name appName | -All]`

* `-Name` Dies ist der Name einer App in IIS.
* `-All` Beendet die Überwachung aller Apps auf diesem IIS-Server, für die `SdkState==EnabledAfterDeployment` gilt.
* Beendet die Überwachung der angegebenen Apps und entfernt die Instrumentierung. Dies funktioniert nur für Apps, die zur Laufzeit mit dem Tool für die Statusüberwachung oder mit „Start-ApplicationInsightsApplication“ überwacht wurden. (`SdkState==EnabledAfterDeployment`)
* Gibt „ApplicationInsightsApplication“ zurück.

`Update-ApplicationInsightsMonitoring -Name appName [-InstrumentationKey "0000000-0000-000-000-0000"`]

* `-Name`: Dies ist der Name einer Web-App in IIS.
* `-InstrumentationKey` (Optional.) Verwenden Sie dieses Element zum Ändern der Ressource, an die die Telemetriedaten der App gesendet werden.
* Mit diesem Cmdlet wird Folgendes durchgeführt:
  * Aktualisiert die benannte App auf die Version des SDK, die zuletzt auf den Computer heruntergeladen wurde. (Funktioniert nur, wenn `SdkState==EnabledAfterDeployment`gilt.)
  * Wenn Sie einen Instrumentierungsschlüssel angeben, wird die benannte App neu konfiguriert, um Telemetriedaten an die Ressource mit diesem Schlüssel zu senden. (Funktioniert, wenn `SdkState != Disabled`gilt.)

`Update-ApplicationInsightsVersion`

* Lädt das aktuelle Application Insights SDK auf den Server herunter.

## <a name="a-namenextanext-steps"></a><a name="next"></a>Nächste Schritte
* [Erstellen Sie Webtests][availability], um sicherzustellen, dass Ihre Website live bleibt.
* [Durchsuchen Sie Ereignisse und Protokolle][diagnostic], um Probleme besser zu diagnostizieren.
* [Fügen Sie Webclienttelemetrie hinzu][usage], um Ausnahmen von Webseitencode anzuzeigen und Ablaufverfolgungsaufrufe einfügen zu können.
* [Fügen Sie Ihrem Webdienstcode das Application Insights-SDK hinzu][greenbrown], damit Sie die Ablaufverfolgungs- und Protokollaufrufe im Servercode einfügen können.

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[availability]: app-insights-monitor-web-app-availability.md
[client]: app-insights-javascript.md
[diagnostic]: app-insights-diagnostic-search.md
[greenbrown]: app-insights-asp-net.md
[qna]: app-insights-troubleshoot-faq.md
[roles]: app-insights-resources-roles-access-control.md
[usage]: app-insights-web-track-usage.md



<!--HONumber=Jan17_HO4-->


