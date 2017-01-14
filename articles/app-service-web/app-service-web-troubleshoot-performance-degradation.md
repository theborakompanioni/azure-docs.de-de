---
title: Schwache Leistung der Web-App in App Service | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie Web-App-Leistungsprobleme in Azure App Service beheben.
services: app-service\web
documentationcenter: 
author: cephalin
manager: wpickett
editor: 
tags: top-support-issue
keywords: Web-App-Leistung, langsame App, App langsam
ms.assetid: b8783c10-3a4a-4dd6-af8c-856baafbdde5
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2016
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: b0f1954b44e42b406aa5ffa71eee74a3bd7be4cc


---
# <a name="troubleshoot-slow-web-app-performance-issues-in-azure-app-service"></a>Problembehandlung bei niedriger Web-App-Leistung in Azure App Service
In diesem Artikel erfahren Sie, wie Sie Web-App-Leistungsprobleme in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714)beheben.

Wenn Sie beim Lesen dieses Artikels feststellen, dass Sie weitere Hilfe benötigen, können Sie Ihre Frage im [MSDN Azure-Forum oder im Stack Overflow-Forum](https://azure.microsoft.com/support/forums/)stellen, um dort Hilfe von Azure-Experten zu erhalten. Alternativ dazu haben Sie die Möglichkeit, einen Azure-Supportfall zu erstellen. Rufen Sie die [Azure-Support-Website](https://azure.microsoft.com/support/options/) auf, und klicken Sie auf **Support erhalten**.

## <a name="symptom"></a>Symptom
Beim Navigieren in der Web-App werden die Seiten langsam geladen, und manchmal tritt beim Laden der Seiten ein Timeout auf.

## <a name="cause"></a>Ursache
Diese Fehler sind häufig auf Probleme auf der Anwendungsebene zurückzuführen, z. B.:

* Anforderungen, die sehr lange dauern
* Eine Anwendung mit hoher Speicher-/CPU-Auslastung
* Eine Anwendung, die aufgrund einer Ausnahme abstürzt

## <a name="troubleshooting-steps"></a>Schritte zur Problembehandlung
Die Problembehandlung lässt sich in diesem Fall in drei unterschiedliche Aufgaben unterteilen, die nacheinander ausgeführt werden:

1. [Beobachten und Überwachen des Anwendungsverhaltens](#observe)
2. [Sammeln von Daten](#collect)
3. [Minimieren der Auswirkungen des Problems](#mitigate)

[App Service-Web-Apps](/services/app-service/web/) bietet Ihnen bei jedem Schritt verschiedene Optionen.

<a name="observe" />

### <a name="1-observe-and-monitor-application-behavior"></a>1. Beobachten und Überwachen des Anwendungsverhaltens
#### <a name="track-service-health"></a>Nachverfolgen der Dienstintegrität
Microsoft Azure informiert jeweils aktuell über Dienstunterbrechungen oder Leistungsbeeinträchtigungen. Sie können die Integrität des Diensts im [Azure-Portal](https://portal.azure.com/)nachverfolgen. Weitere Informationen finden Sie unter [Nachverfolgen der Dienstintegrität](../monitoring-and-diagnostics/insights-service-health.md).

#### <a name="monitor-your-web-app"></a>Überwachen Ihrer Web-App
Durch das Überwachen der Web-App können Sie herausfinden, ob in Ihrer Anwendung Probleme vorliegen. Klicken Sie auf dem Blatt Ihrer Web-App auf die Kachel **Anforderungen und Fehler** . Auf dem Blatt **Metrik** werden alle Metriken angezeigt, die Sie hinzufügen können.

Folgende Metriken können Sie z. B. für Ihre Web-App überwachen:

* Durchschnittlicher Arbeitssatz für Arbeitsspeicher
* Durchschnittliche Reaktionszeit
* CPU-Zeit
* Arbeitssatz für Arbeitsspeicher
* Anforderungen

![Überwachen der Web-App-Leistung](./media/app-service-web-troubleshoot-performance-degradation/1-monitor-metrics.png)

Weitere Informationen finden Sie unter:

* [Überwachen von Web-Apps in Azure App Service](web-sites-monitor.md)
* [Empfangen von Warnbenachrichtigungen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

#### <a name="monitor-web-endpoint-status"></a>Überwachen des Web-Endpunktstatus
Wenn Sie Ihre Web-App im Tarif **Standard** ausführen, können Sie mit Web-Apps 2 Endpunkte von 3 geografischen Standorten aus überwachen.

Endpunktüberwachung konfiguriert Webtests von geografisch verteilten Standorten zum Testen der Reaktionszeit und der Verfügbarkeit von Web-URLs. Der Test führt einen HTTP GET-Aufruf für die Web-URL durch, um die Reaktionszeit und Verfügbarkeit der einzelnen Standorte zu ermitteln. Jeder konfigurierte Standort führt alle fünf Minuten einen Test aus.

Die Verfügbarkeit wird mit HTTP-Antwortcodes überwacht, und die Reaktionszeit wird in Millisekunden gemessen. Der Überwachungstest schlägt fehl, wenn der HTTP-Antwortcode größer als oder gleich 400 ist und die Antwort länger als 30 Sekunden benötigt. Der Endpunkt wird als verfügbar betrachtet, wenn die Überwachungstests von allen angegebenen Standorten aus erfolgreich waren.

Informationen zur Einrichtungen finden Sie unter [Gewusst wie: Überwachen von Apps in Azure App Service](web-sites-monitor.md).

Ein Video zur Endgeräteüberwachung finden Sie unter [Pflegen von Azure-Websites plus Endgeräteüberwachung - mit Stefan Schackow](/documentation/videos/azure-web-sites-endpoint-monitoring-and-staying-up/) .

#### <a name="application-performance-monitoring-using-extensions"></a>Überwachung der Anwendungsleistung mithilfe von Erweiterungen
Sie können die Anwendungsleistung auch mit *Websiteerweiterungen*überwachen.

Jede App Service-Web-App bietet einen erweiterbaren Verwaltungsendpunkt, mit dem Sie eine Reihe leistungsstarker Tools nutzen können, die als Websiteerweiterungen bereitgestellt werden. Diese Tools reichen von Quellcode-Editoren wie [Visual Studio Team Services](https://www.visualstudio.com/products/what-is-visual-studio-online-vs.aspx) bis zu Verwaltungstools für verbundene Ressourcen (beispielsweise eine mit einer Web-App verbundene MySQL-Datenbank).

[Azure Application Insights](/services/application-insights/) und [New Relic](/marketplace/partners/newrelic/newrelic/) sind zwei der verfügbaren Websiteerweiterungen für die Leistungsüberwachung. Um New Relic zu verwenden, müssen Sie einen Agent zur Laufzeit installieren. Um Azure Application Insights zu verwenden, erstellen Sie Ihren Code mit einem SDK neu. Sie können auch eine Erweiterung installieren, die Zugriff auf zusätzliche Daten bietet. Mit dem SDK können Sie Code schreiben, um die Auslastung und Leistung Ihrer Anwendung genauer zu überwachen.

Informationen zum Verwenden von Application Insights finden Sie unter [Leistung in Webanwendungen überwachen](../application-insights/app-insights-web-monitor-performance.md).

Informationen zum Verwenden von New Relic finden Sie unter [Verwaltung der Anwendungsleistung mit New Relic unter Azure](../store-new-relic-cloud-services-dotnet-application-performance-management.md).

<a name="collect" />

### <a name="2-collect-data"></a>2. Sammeln von Daten
#### <a name="enable-diagnostics-logging-for-your-web-app"></a>Aktivieren des Diagnoseprotokolls für Ihre Web-App
Die Web-Apps-Umgebung bietet Diagnosefunktionen zum Protokollieren von Informationen – sowohl über den Webserver als auch über die Webanwendung. Diese sind logisch in Webserverdiagnose und Anwendungsdiagnose unterteilt.

##### <a name="web-server-diagnostics"></a>Webserverdiagnose
Sie können die folgenden Protokollarten aktivieren oder deaktivieren:

* **Detaillierte Fehlerprotokollierung** - Detaillierte Fehlerinformationen für HTTP-Statuscodes, die auf einen Fehler hinweisen (Statuscode 400 oder höher). Diese können Informationen enthalten, mit deren Hilfe sich bestimmen lässt, warum der Server den Fehlercode zurückgegeben hat.
* **Ablaufverfolgung von Anforderungsfehlern** - Detaillierte Informationen zu fehlgeschlagenen Anforderungen, einschließlich der Verfolgung von IIS-Komponenten, die zur Verarbeitung der Anforderung verwendet wurden, sowie die in jeder Komponente benötigte Zeit. Dies kann hilfreich sein, wenn Sie versuchen, die Leistung von Web-Apps zu verbessern oder die Ursache eines bestimmten HTTP-Fehlers zu ermitteln.
* **Webserverprotokollierung** : Informationen zu HTTP-Transaktionen im erweiterten W3C-Protokolldateiformat. Dies ist hilfreich, wenn Sie allgemeine Web-App-Kennzahlen ermitteln möchten (etwa die Anzahl der verarbeiteten Anfragen oder die Anzahl der Anforderungen von einer bestimmten IP-Adresse).

##### <a name="application-diagnostics"></a>Anwendungsdiagnose
Mit der Option „Anwendungsdiagnose“ können Sie die von einer Webanwendung generierten Informationen erfassen. ASP.NET-Anwendungen können die Klasse `System.Diagnostics.Trace` verwenden, um Informationen im Anwendungsdiagnoseprotokoll aufzuzeichnen.

Ausführlichen Anweisungen zum Konfigurieren der Anwendung für die Protokollierung finden Sie unter [Aktivieren der Diagnoseprotokollierung für Web-Apps in Azure App Service](web-sites-enable-diagnostic-log.md).

#### <a name="use-remote-profiling"></a>Verwenden von Remoteprofilerstellung
In Azure App Service kann eine Remoteprofilerstellung von Web-Apps, API-Apps und WebJobs ausgeführt werden. Wenn der Prozess langsamer als erwartet ausgeführt wird oder wenn die Latenz von HTTP-Anforderungen außergewöhnlich hoch und die CPU-Auslastung des Prozesses ebenfalls hoch ist, können Sie eine Remoteprofilerstellung des Prozesses ausführen und die CPU-Samplingaufruflisten abrufen, um die Prozessaktivität sowie die Hot Paths im Code zu analysieren.

Weitere Informationen finden Sie unter [Remote Profiling Support in Azure App Service](https://azure.microsoft.com/blog/remote-profiling-support-in-azure-app-service)(Unterstützung von Remoteprofilerstellung in Azure App Service (in englischer Sprache)).

#### <a name="use-the-azure-app-service-support-portal"></a>Verwenden des Azure App Service-Support-Portals
Web-Apps bietet Ihnen die Möglichkeit, Probleme im Zusammenhang mit Ihrer Web-App anhand von HTTP-Protokollen, Ereignisprotokollen, Prozesssicherungen usw. zu behandeln. Auf diese Informationen können Sie in unserem Supportportal unter „**http://&lt;Name_Ihrer_App>.scm.azurewebsites.net/Support**“ zugreifen.

Im Azure App Service-Support-Portal stehen Ihnen drei separate Registerkarten für die drei Schritte eines typischen Problembehandlungsszenarios zur Verfügung:

1. Aktuelles Verhalten beobachten
2. Diagnoseinformationen sammeln und integrierte Analysen ausführen, um das Problem zu analysieren
3. Auswirkungen des Problems minimieren

Wenn das Problem gerade auftritt, klicken Sie auf **Analysieren** > **Diagnose** > **Jetzt diagnostizieren**, um eine Diagnosesitzung zu erstellen, die HTTP-Protokolle, Protokolle der Ereignisanzeige, Speicherabbilddateien, PHP-Fehlerprotokolle und einen PHP-Prozessbericht sammelt.

Nachdem die Daten gesammelt wurden, wird auch eine Analyse der Daten ausgeführt, und es wird ein HTML-Bericht angezeigt.

Falls Sie die Daten herunterladen, werden diese standardmäßig im Ordner „D:\home\data\DaaS“ gespeichert.

Weitere Informationen zum Azure App Service-Support-Portal finden Sie unter [Neue Updates zur Unterstützung von Websiteerweiterungen für Azure-Websites](https://azure.microsoft.com/blog/new-updates-to-support-site-extension-for-azure-websites).

#### <a name="use-the-kudu-debug-console"></a>Verwenden der Kudu-Debugkonsole
Web-Apps enthält eine Debugkonsole, die Sie zum Debuggen, Untersuchen und Hochladen von Dateien verwenden können, sowie JSON-Endpunkte zum Abrufen von Informationen über Ihre Umgebung. Dieses Feature wird als *Kudu-Konsole* oder *SCM-Dashboard* für Ihre Web-App bezeichnet.

Sie können über den Link **https://&lt;Name_Ihrer_App>.scm.azurewebsites.net/** auf das Dashboard zugreifen.

Die Kudu-Konsole bietet z. B. folgende Funktionen:

* Umgebungseinstellungen für Ihre Anwendung
* Protokolldatenstrom
* Speicherabbild zu Diagnosezwecken
* Debugkonsole zum Ausführen von Powershell-Cmdlets und grundlegenden DOS-Befehlen

Eine weitere nützliche Funktion von Kudu ist die Möglichkeit, mithilfe von Kudu und dem SysInternals-Tool „Procdump“ Speicherabbilddateien zu erstellen, wenn Ihre Anwendung Ausnahmen (erste Chance) auslöst. Bei diesen Speicherabbilddateien handelt es sich um Momentaufnahmen des Prozesses, die beim Beheben komplizierterer Probleme mit Ihrer Web-App hilfreich sein können.

Weitere Informationen zu den verfügbaren Funktionen in Kudu finden Sie unter [Team Services-Tools für Azure-Websites, die Sie kennen sollten](https://azure.microsoft.com/blog/windows-azure-websites-online-tools-you-should-know-about/).

<a name="mitigate" />

### <a name="3-mitigate-the-issue"></a>3. Minimieren der Auswirkungen des Problems
#### <a name="scale-the-web-app"></a>Skalieren der Web-App
In Azure App Service können Sie zum Steigern der Leistung und des Durchsatzes die Skalierung anpassen, mit der Sie Ihre Anwendung ausführen. Zum Hochskalieren einer Web-App müssen Sie zwei zusammenhängende Schritte ausführen: Für den App Service-Plan muss ein höherer Tarif verwendet und nach dem Tarifwechsel müssen bestimmte Einstellungen konfiguriert werden.

Weitere Informationen zur Skalierung finden Sie unter [Skalieren einer Web-App in Azure App Service](web-sites-scale.md).

Außerdem haben Sie die Möglichkeit, Ihre Anwendung auf mehreren Instanzen auszuführen. Dies erhöht nicht nur die Verarbeitungskapazität, sondern bietet auch ein gewisses Maß an Fehlertoleranz. Wenn der Prozess in einer Instanz ausfällt, kann die andere Instanz weiterhin Anfragen verarbeiten.

Sie können die Skalierung auf „Manuell“ oder „Automatisch“ festlegen.

#### <a name="use-autoheal"></a>Verwenden von „AutoHeal“
„AutoHeal“ startet den Arbeitsprozess für Ihre App basierend auf von Ihnen ausgewählten Einstellungen neu (z. B. Konfigurationsänderungen, Anforderungen, speicherbasierte Grenzwerte oder zum Ausführen einer Anforderung benötigte Zeit). In den meisten Fällen ist das Neustarten des Prozesses die schnellste Methode zum Beheben eines Problems. Sie können die Web-App zwar immer direkt im Azure-Portal neu starten, „AutoHeal“ führt diesen Schritt jedoch automatisch für Sie aus. Sie müssen lediglich einige Trigger in der Stammdatei „web.config“ Ihrer Web-App hinzufügen. Beachten Sie, dass diese Einstellungen immer auf die gleiche Weise angewendet werden, auch dann, wenn es sich bei Ihrer Anwendung nicht um eine .Net-Anwendung handelt.

Weitere Informationen finden Sie unter [Automatische Reparatur von Azure-Websites](https://azure.microsoft.com/blog/auto-healing-windows-azure-web-sites/).

#### <a name="restart-the-web-app"></a>Neustarten der Web-App
Dies ist oft die einfachste Methode zum Beheben einmaliger Probleme. Im [Azure-Portal](https://portal.azure.com/)stehen auf dem Blatt Ihrer Web-App Optionen zum Beenden oder Neustarten der App zur Verfügung.

 ![Neustart der Web-App zum Beheben von Leistungsproblemen](./media/app-service-web-troubleshoot-performance-degradation/2-restart.png)

Sie können Ihre Web-App auch mit Azure PowerShell verwalten. Weitere Informationen finden Sie unter [Verwenden von Azure PowerShell mit dem Azure-Ressourcen-Manager](../powershell-azure-resource-manager.md).



<!--HONumber=Dec16_HO2-->


