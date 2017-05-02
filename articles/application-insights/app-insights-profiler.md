---
title: "Profilerstellung für Live-Web-Apps in Azure mit Application Insights | Microsoft-Dokumentation"
description: Identifizieren Sie den langsamsten Pfad in Ihrem Webservercode mithilfe eines kompakten Profilers.
services: application-insights
documentationcenter: 
author: alancameronwills
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 04/03/2017
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: ed685f0a4ed26fb8d1c766f87210a99d2b5270ac
ms.lasthandoff: 04/12/2017


---
# <a name="profiling-live-azure-web-apps-with-application-insights-preview"></a>Profilerstellung für Live-Azure-Web-Apps mit Application Insights (Vorschau)

*Dieses Application Insights-Feature befindet sich in der Vorschauphase.*

Ermitteln Sie den Zeitaufwand für die einzelnen Methoden in Ihrer Live-Webanwendung mithilfe eines Profilerstellungstools von [Azure Application Insights](app-insights-overview.md). Das Tool zeigt detaillierte Profile der aktiven Anforderungen, die von Ihrer App verarbeitet wurden, und hebt den langsamsten Pfad hervor, der die meiste Zeit beansprucht. Es wählt automatisch Beispiele mit unterschiedlichen Reaktionszeiten aus. Der Profiler verwendet verschiedene Techniken, um den Mehraufwand möglichst gering zu halten. 

Der Profiler kann momentan für ASP.NET-Web-Apps verwendet werden, die unter Azure App Services und mindestens unter dem Basic-Tarif ausgeführt werden. (Bei Verwendung von ASP.NET Core muss `.NetCoreApp` als Zielframework verwendet werden.)

<a id="installation"></a>
## <a name="enable-the-profiler"></a>Aktivieren des Profilers

[Installieren Sie Application Insights](app-insights-asp-net.md) in Ihrem Code. Sollte es bereits installiert sein, vergewissern Sie sich, dass Sie über die neueste Version verfügen. (Klicken Sie hierzu im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und wählen Sie „NuGet-Pakete verwalten“ aus. Wählen Sie „Updates“ aus, und aktualisieren Sie alle Pakete.) Stellen Sie Ihre App erneut bereit.

*Sie verwenden ASP.NET Core? Dann lesen Sie [hier](#aspnetcore) weiter.*

Öffnen Sie in [https://portal.azure.com](https://portal.azure.com) die Application Insights-Ressource für Ihre Web-App. Öffnen Sie **Leistung**, und klicken Sie auf **Konfigurieren**. Wählen Sie Ihre App aus, und durchlaufen Sie den Assistenten.

![Klicken Sie auf dem Blatt „Leistung“ auf „Konfigurieren“.][performance-blade]

* *Die Schaltfläche „Konfigurieren“ steht nicht zur Verfügung? Verwenden Sie das [manuelle Verfahren](#manual-installation).*

Falls Sie den Profiler beenden oder neu starten müssen, finden Sie ihn **in der App Service-Ressource** unter **Webaufträge**. Wenn Sie ihn löschen möchten, suchen Sie unter **Erweiterungen**.

Wenn Sie Änderungen an Ihrer Webanwendung mithilfe von WebDeploy bereitstellen, achten Sie darauf, dass der Ordner **App_Data** bei der Bereitstellung nicht gelöscht wird. Andernfalls werden die Dateien der Profiler-Erweiterung gelöscht, wenn Sie die Webanwendung das nächste Mal für Azure bereitstellen.

## <a name="viewing-profiler-data"></a>Anzeigen von Profiler-Daten

Öffnen Sie das Blatt „Leistung“, und scrollen Sie zur Vorgangsliste.




![Application Insights-Blatt „Leistung“, Spalte „Beispiele“][performance-blade-examples]

Die Tabelle enthält folgende Spalten:

* **Anzahl**: Die Anzahl von Anforderungen im Zeitbereich des Blatts.
* **Median**: Die typische Reaktionszeit Ihrer App bei einer Anforderung. Die Hälfte aller Reaktionen war schneller als dieser Wert.
* **95. Quantil**: 95 Prozent der Reaktionen waren schneller als dieser Wert. Sollte sich dieser Wert vom Median-Wert unterscheiden, liegt bei Ihrer App unter Umständen ein vorübergehendes Problem vor. (Eine andere denkbare Erklärung wäre ein Designfeature wie etwa Caching.)
* **Beispiele**: Ein Symbol gibt an, dass der Profiler Stapelüberwachungen für diesen Vorgang erfasst hat.

Klicken Sie auf das Symbol „Beispiele“, um den Ablaufverfolgungsexplorer zu öffnen. Der Explorer zeigt mehrere vom Profiler erfasste Beispiele, klassifiziert nach Reaktionszeit.

Wählen Sie ein Beispiel aus, um eine Aufschlüsselung des Zeitaufwands für die Anforderung auf Codeebene anzuzeigen.

![Ablaufverfolgungsexplorer von Application Insights][trace-explorer]

**Langsamsten Pfad anzeigen** öffnet den größten Blattknoten (oder etwas in der Nähe). In den meisten Fällen befindet sich dieser Knoten in der Nähe eines Leistungsengpasses.



* **Bezeichnung**: Der Name der Funktion oder des Ereignisses. Die Struktur enthält eine Mischung aus Code und aufgetretenen Ereignissen (wie etwa SQL- und HTTP-Ereignisse). Das oberste Ereignis stellt die Gesamtdauer der Anforderung dar.
* **Metrik**: Die verstrichene Zeit.
* **Wann**: Gibt an, wann die Funktion/das Ereignis aktiv war (relativ zu anderen Funktionen). 

## <a name="how-to-read-performance-data"></a>Lesen von Leistungsdaten

Der Dienst-Profiler von Microsoft analysiert die Leistung Ihrer Anwendung mithilfe einer Kombination aus Samplingmethode und Instrumentation. Während der ausführlichen Erfassung nimmt der Dienst-Profiler jede Millisekunde Stichproben des Anweisungszeigers der einzelnen CPUs des Computers. Jede Stichprobe erfasst die vollständige Aufrufliste des aktuell ausgeführten Threads und liefert ausführliche und nützliche Informationen zu den Abläufen des Threads – sowohl mit hoher als auch mit geringer Abstraktion. Darüber hinaus erfasst der Dienst-Profiler auch andere Ereignisse wie etwa Kontextwechsel-, TPL- und Threadpool-Ereignisse zur Nachverfolgung von Aktivitätskorrelation und Kausalität. 

Die Aufrufliste in der Zeitachsenansicht ist das Ergebnis des oben erwähnten Samplings und der Instrumentation. Da jede Stichprobe die vollständige Aufrufliste des Threads erfasst, enthält sie auch Code aus .NET Framework sowie aus anderen Frameworks, auf die Sie verweisen.

### <a id="jitnewobj"></a>Objektzuordnung (`clr!JIT\_New or clr!JIT\_Newarr1`)
`clr!JIT\_New and clr!JIT\_Newarr1` sind Hilfsfunktionen in .NET Framework, die Arbeitsspeicher aus dem verwalteten Heap zuteilen. `clr!JIT\_New` wird aufgerufen, wenn ein Objekt zugeordnet wird. `clr!JIT\_Newarr1` wird aufgerufen, wenn ein Objektarray zugeordnet wird. Diese beiden Funktionen sind in der Regel sehr schnell und sollten nicht lange dauern. Sollte `clr!JIT\_New` oder `clr!JIT\_Newarr1` auf Ihrer Zeitachse viel Zeit beanspruchen, deutet das darauf hin, dass der Code viele Objekte zuordnet und eine erhebliche Menge an Arbeitsspeicher beansprucht. 

### <a id="theprestub"></a>Laden von Code (`clr!ThePreStub`)
`clr!ThePreStub` ist eine Hilfsfunktion in .NET Framework, die den Code für die erstmalige Ausführung vorbereitet. Das schließt in der Regel auch die JIT-Kompilierung (Just In Time) ein. Während der Lebensdauer eines Prozesses sollte `clr!ThePreStub` für jede C#-Methode höchstens einmal aufgerufen werden.

Sollte `clr!ThePreStub` sehr viel Zeit für eine Anforderung beanspruchen, deutet das darauf hin, dass diese Anforderung die erste Anforderung ist, die diese Methode ausführt, und dass die .NET Framework-Laufzeit viel Zeit zum Laden dieser Methode benötigt. Verwenden Sie ggf. einen Vorbereitungsprozess, der diesen Teil des Codes ausführt, bevor Ihre Benutzer darauf zugreifen, oder führen Sie NGen für Ihre Assemblys aus. 

### <a id="lockcontention"></a>Sperrkonflikt (`clr!JITutil\_MonContention` oder `clr!JITutil\_MonEnterWorker`)
`clr!JITutil\_MonContention`oder `clr!JITutil\_MonEnterWorker` deutet darauf hin, dass der aktuelle Thread auf die Aufhebung einer Sperre wartet. Das ist üblicherweise der Fall, wenn eine C#-Sperranweisung ausgeführt, eine Monitor.Enter-Methode aufgerufen oder eine Methode mit MethodImplOptions.Synchronized-Attribut aufgerufen wird. Ein Sperrkonflikt ist in der Regel darauf zurückzuführen, dass Thread A eine Sperre abruft und Thread B versucht, die gleiche Sperre abzurufen, bevor sie von Thread A wieder freigegeben wurde. 

### <a id="ngencold"></a>Laden von Code (`[COLD]`)
Falls der Methodenname `[COLD]` enthält (Beispiel: `mscorlib.ni![COLD]System.Reflection.CustomAttribute.IsDefined`), bedeutet das, dass die .NET Framework-Laufzeit nicht durch die <a href="https://msdn.microsoft.com/library/e7k32f4k.aspx">profilgesteuerte Optimierung</a> optimierten Code zum ersten Mal ausführt. Während der Lebensdauer des Prozesses sollte dies für jede Methode höchstens einmal erscheinen. 

Falls das Laden von Code bei einer Anforderung sehr lange dauert, deutet das darauf hin, dass die Anforderung die erste Anforderung ist, die den nicht optimierten Teil der Methode ausführt. Verwenden Sie ggf. einen Vorbereitungsprozess, der diesen Teil des Codes ausführt, bevor Ihre Benutzer darauf zugreifen. 

### <a id="httpclientsend"></a>HTTP-Sendeanforderung
Methoden wie `HttpClient.Send` deuten darauf hin, dass der Code auf den Abschluss einer HTTP-Anforderung wartet.

### <a id="sqlcommand"></a>Datenbankvorgang
Methoden wie „SqlCommand.Execute“ deuten darauf hin, dass der Code auf den Abschluss eines Datenbankvorgangs wartet.

### <a id="await"></a>Warten (`AWAIT\_TIME`)
`AWAIT\_TIME` deutet darauf hin, dass der Code auf den Abschluss einer anderen Aufgabe wartet. Das ist in der Regel bei C#-Anweisungen vom Typ „await“ der Fall. Wenn der Code eine C#-Anweisung vom Typ „await“ ausführt, wird der Thread entladen und übergibt die Steuerung wieder an den Threadpool, sodass kein Thread auf den Abschluss der await-Anweisung warten muss und dadurch blockiert wird. Logisch betrachtet ist allerdings der Thread, der die await-Anweisung ausgeführt hat, blockiert, während er auf den Abschluss des Vorgangs wartet. `AWAIT\_TIME` gibt die blockierte Zeit an, für die auf den Abschluss der Aufgabe gewartet wird.

### <a id="block"></a>Blockierte Zeit
`BLOCKED_TIME` deutet darauf hin, dass der Code darauf wartet, dass eine andere Ressource verfügbar wird. Das ist beispielsweise beim Warten auf ein Synchronisierungsobjekt, beim Warten auf die Verfügbarkeit eines Threads oder beim Warten auf den Abschluss einer Anforderung der Fall. 

### <a id="cpu"></a>CPU-Zeit
Die CPU ist mit der Ausführung der Anweisungen beschäftigt.

### <a id="disk"></a>Datenträgerzeit
Die Anwendung führt Datenträgervorgänge aus.

### <a id="network"></a>Netzwerkzeit
Die Anwendung führt Netzwerkvorgänge aus.

### <a id="when"></a>Wann-Spalte
Hierbei handelt es sich um eine Visualisierung der Abweichungen INKLUSIVER Stichproben für einen Knoten im Zeitverlauf. Der Gesamtbereich der Anforderung wird in 32 Zeitrahmen unterteilt, und die inklusiven Stichproben für den Knoten werden in diesen 32 Zeitrahmen gesammelt. Die einzelnen Zeitrahmen werden dann als Balken dargestellt, deren Höhe jeweils einen skalierten Wert darstellt. Bei Knoten, die mit `CPU_TIME` oder `BLOCKED_TIME` gekennzeichnet sind oder bei denen ein offensichtlicher Zusammenhang mit der Nutzung einer Ressource (CPU, Datenträger, Thread) besteht, stellt der Balken die Nutzung einer dieser Ressourcen für den Zeitraum des entsprechenden Zeitrahmens dar. Durch die Nutzung mehrerer Ressourcen kann bei diesen Metriken die 100-Prozent-Marke überschritten werden. Wenn Sie also beispielsweise innerhalb eines Intervalls im Schnitt zwei CPUs verwenden, erhalten Sie 200 Prozent.


## <a id="troubleshooting"></a>Problembehandlung

### <a name="how-can-i-know-whether-application-insights-profiler-is-running"></a>Wie kann ich feststellen, ob Application Insights Profiler ausgeführt wird?

Der Profiler wird als fortlaufender Webauftrag in der Web-App ausgeführt. Sie können die Web-App-Ressource in „https://portal.azure.com“ öffnen und den Status von „ApplicationInsightsProfiler“ auf dem Blatt „WebJobs“ prüfen. Sollte er nicht ausgeführt werden, öffnen Sie **Protokolle**, um mehr zu erfahren. 

### <a name="why-cant-i-find-any-stack-examples-even-though-the-profiler-is-running"></a>Warum finde ich keine Stapelbeispiele, obwohl der Profiler ausgeführt wird?

Überprüfen Sie Folgendes:

1. Vergewissern Sie sich, dass Sie für Ihre Web-App mindestens einen Service-Plan mit Basic-Tarif verwenden.
2. Vergewissern Sie sich, dass für Ihre Web-App mindestens Application Insights SDK 2.2 Beta aktiviert ist.
3. Vergewissern Sie sich, dass für die APPINSIGHTS_INSTRUMENTATIONKEY-Einstellung Ihrer Web-App der gleiche Instrumentationsschlüssel verwendet wird wie vom Application Insights SDK.
4. Vergewissern Sie sich, dass Ihre Web-App unter .NET Framework 4.6 ausgeführt wird.
5. Falls es sich um eine ASP.NET Core-Anwendung handelt, prüfen Sie auch die [erforderlichen Abhängigkeiten](#aspnetcore).

Nach dem Start des Profilers folgt eine kurze Vorbereitungsphase, in der der Profiler aktiv mehrere Leistungsnachverfolgungen erfasst. Danach werden Leistungsnachverfolgungen vom Profiler jeweils stündlich für zwei Minuten erfasst.  

### <a name="i-was-using-azure-service-profiler-what-happened-to-it"></a>Ich habe Azure Service Profiler verwendet. Was ist damit passiert?  

Wenn Sie Application Insights Profiler aktivieren, wird der Azure Service Profiler-Agent deaktiviert.

### <a id="double-counting"></a>Doppelte Erfassung in parallelen Threads

Manchmal übersteigt die Gesamtzeitmetrik im Stapel-Viewer die tatsächliche Dauer der Anforderung. 

Dieser Fall kann eintreten, wenn einer Anforderung mehrere parallel ausgeführte Threads zugeordnet sind. Die Gesamtzeit der Threads übersteigt dann die verstrichene Zeit. In vielen Fällen wartet ein Thread möglicherweise auf den Abschluss eines anderen Threads. Der Viewer versucht, solche Fälle zu erkennen und die nicht relevante Wartezeit zu ignorieren. Im Zweifelsfall wird jedoch lieber zu viel angezeigt, um zu vermeiden, dass womöglich entscheidende Informationen ignoriert werden.  

Wenn Ihnen in Ihren Ablaufverfolgungen parallele Threads begegnen, müssen Sie die wartenden Threads bestimmen, um den kritischen Pfad für die Anforderung ermitteln zu können. In den meisten Fällen wartet der Thread, der schnell in einen Wartezustand wechselt, einfach auf den Abschluss der anderen Threads. Konzentrieren Sie sich auf die anderen Threads, und ignorieren Sie die Zeit in den wartenden Threads.

### <a id="issue-loading-trace-in-viewer"></a>Keine Daten für die Profilerstellung

1. Falls die Daten, die Sie anzeigen möchten, bereits mehrere Wochen alt sind, schränken Sie Ihren Zeitfilter ein, und versuchen Sie es noch mal.

2. Vergewissern Sie sich, dass der Zugriff auf „https://gateway.azureserviceprofiler.net“ nicht durch Proxys oder durch eine Firewall blockiert wird. 

3. Vergewissern Sie sich, dass der in der App verwendete Application Insights-Instrumentierungsschlüssel dem Instrumentierungsschlüssel der Application Insights-Ressource entspricht, mit der Sie die Profilerstellung aktiviert haben. Der Schlüssel befindet sich für gewöhnlich in „ApplicationInsights.config“, ist aber auch in „web.config“ oder „app.config“ zu finden.

### <a name="error-report-in-the-profiling-viewer"></a>Fehlerbericht im Profilerstellungs-Viewer

Erstellen Sie ein Supportticket über das Portal. Geben Sie dabei die Korrelations-ID aus der Fehlermeldung an.


## <a name="manual-installation"></a>Manuelle Installation

Wenn Sie den Profiler konfigurieren, werden an den Einstellungen der Web-App folgende Aktualisierungen vorgenommen. Diese können auch manuell vorgenommen werden:

1. Öffnen Sie auf dem Steuerungsblatt der Web-App die Option „Einstellungen“.
2. Legen Sie die .NET Framework-Version auf „v4.6“ fest.
3. Legen Sie „Immer bereit“ auf „Ein“ fest.
4. Fügen Sie die App-Einstellung __APPINSIGHTS_INSTRUMENTATIONKEY__ hinzu, und legen Sie den Wert auf den Instrumentationsschlüssel des SDKs fest.
5. Fügen Sie unter **Erweiterungen** den Profiler „Application Insights Profiler“ hinzu. Die Installation dauert zwei bis drei Minuten.

## <a id="aspnetcore"></a>Unterstützung von ASP.NET Core

Die ASP.NET Core-Anwendung wird derzeit unter der .NET Core-Laufzeit unterstützt.

Für die Profilerstellung muss die Anwendung zusätzlich über folgende Komponenten verfügen:

1. [Application Insights für ASP.NET Core 2.0](https://github.com/Microsoft/ApplicationInsights-aspnetcore/releases/tag/v2.0.0)
2. [System.Diagnostics.DiagnosticSource 4.4.0-beta-25022-02](https://dotnet.myget.org/feed/dotnet-core/package/nuget/System.Diagnostics.DiagnosticSource/4.4.0-beta-25022-02)
    * Wählen Sie in Visual Studio „Extras“ > „NuGet-Paket-Manager“ > „Paket-Manager-Einstellungen“ aus.
    * Wählen Sie im Dialogfeld „Optionen“ die Option „NuGet-Paket-Manager“ und anschließend „Paketquellen“ aus.
    * Klicken Sie auf Schaltfläche „+“, um eine neue Paketquelle mit dem Namen „DotNet-Core-MyGet“ und dem Wert „https://dotnet.myget.org/F/dotnet-core/api/v3/index.json“ hinzuzufügen.
    * Klicken Sie auf die Schaltfläche „Aktualisieren“, und schließen Sie das Dialogfeld „Optionen“.
    * Öffnen Sie den Projektmappen-Explorer, klicken Sie mit der rechten Maustaste auf das ASP.NET Core-Projekt, und wählen Sie „NuGet-Pakete verwalten...“ aus.
    * Klicken Sie auf die Registerkarte „Durchsuchen“, wählen Sie „Paketquelle: DotNet-Core-MyGet“ aus, und aktivieren Sie das Kontrollkästchen „Vorabversion einbeziehen“.
    * Suchen Sie nach „System.Diagnostics.DiagnosticSource“, und wählen Sie __4.4.0-beta-25022-02__ aus, um die Komponente zu installieren.


## <a name="next-steps"></a>Nächste Schritte

* [Verwenden von Azure Application Insights in Visual Studio](https://docs.microsoft.com/azure/application-insights/app-insights-visual-studio)

[performance-blade]: ./media/app-insights-profiler/performance-blade.png
[performance-blade-examples]: ./media/app-insights-profiler/performance-blade-examples.png
[trace-explorer]: ./media/app-insights-profiler/trace-explorer.png
[trace-explorer-toolbar]: ./media/app-insights-profiler/trace-explorer-toolbar.png
[trace-explorer-hint-tip]: ./media/app-insights-profiler/trace-explorer-hint-tip.png
[trace-explorer-hot-path]: ./media/app-insights-profiler/trace-explorer-hot-path.png

