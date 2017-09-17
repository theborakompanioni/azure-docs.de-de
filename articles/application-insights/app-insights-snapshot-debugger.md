---
title: "Azure Application Insights Snapshot-Debugger für .NET-Apps | Microsoft Docs"
description: "Debugmomentaufnahmen werden automatisch beim Auslösen von Ausnahmen in .NET-Produktions-Apps erfasst."
services: application-insights
documentationcenter: 
author: qubitron
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 07/03/2017
ms.author: bwren
ms.translationtype: HT
ms.sourcegitcommit: 266b9b7eb228744075627e1e80710e63c27880cc
ms.openlocfilehash: cb0c74e7a3e3a2044262f94275110d0a55ccc19b
ms.contentlocale: de-de
ms.lasthandoff: 09/06/2017

---
# <a name="debug-snapshots-on-exceptions-in-net-apps"></a>Debugmomentaufnahmen von Ausnahmen in .NET-Apps

Wenn eine Ausnahme auftritt, können Sie automatisch eine Debugmomentaufnahme von Ihrer aktiven Webanwendung erfassen. Die Momentaufnahme zeigt den Status des Quellcodes und der Variablen in dem Moment, in dem die Ausnahme ausgelöst wurde. Der Momentaufnahmedebugger (Vorschau) in [Azure Application Insights](app-insights-overview.md) überwacht die Ausnahmetelemetrie aus Ihrer Web-App. Er erfasst Momentaufnahmen Ihrer am häufigsten ausgelösten Ausnahmen, damit Sie die erforderlichen Informationen zur Diagnose von Problemen in der Produktion erhalten. Binden Sie das [NuGet-Paket des Momentaufnahmesammlers](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) in Ihre Anwendung ein, und konfigurieren Sie optional Parameter für die Datensammlung in [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md). Momentaufnahmen finden Sie im Application Insights-Portal unter [Ausnahmen](app-insights-asp-net-exceptions.md).

Sie können Debugmomentaufnahmen im Portal anzeigen, um die Aufrufliste anzuzeigen und die Variablen in jedem Aufruflistenrahmen zu überprüfen. Öffnen Sie zum Verbessern Ihrer Debugleistung mit Quellcode die Momentaufnahmen mit Visual Studio 2017 Enterprise, indem Sie [die Momentaufnahmedebugger-Erweiterung für Visual Studio herunterladen](https://aka.ms/snapshotdebugger).

Die Momentaufnahmesammlung ist für folgende Anwendungen verfügbar:
* .NET Framework- und ASP.NET-Anwendungen, die mit .NET Framework 4.5 oder höher ausgeführt werden
* .NET Core 2.0- und ASP.NET Core 2.0-Anwendungen, die unter Windows ausgeführt werden

### <a name="configure-snapshot-collection-for-aspnet-applications"></a>Konfigurieren der Momentaufnahmesammlung für ASP.NET-Anwendungen

1. Falls noch nicht geschehen, [aktivieren Sie Application Insights für Ihre Web-App](app-insights-asp-net.md).

2. Binden Sie das NuGet-Paket [Microsoft.ApplicationInsights.SnapshotCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) in Ihre App ein. 

3. Überprüfen Sie die Standardoptionen, die zum Paket [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) hinzugefügt wurden:

    ```xml
    <TelemetryProcessors>
        <Add Type="Microsoft.ApplicationInsights.SnapshotCollector.SnapshotCollectorTelemetryProcessor, Microsoft.ApplicationInsights.SnapshotCollector">
        <!-- The default is true, but you can disable Snapshot Debugging by setting it to false -->
        <IsEnabled>true</IsEnabled>
        <!-- Snapshot Debugging is usually disabled in developer mode, but you can enable it by setting this to true. -->
        <!-- DeveloperMode is a property on the active TelemetryChannel. -->
        <IsEnabledInDeveloperMode>false</IsEnabledInDeveloperMode>
        <!-- How many times we need to see an exception before we ask for snapshots. -->
        <ThresholdForSnapshotting>5</ThresholdForSnapshotting>
        <!-- The maximum number of examples we create for a single problem. -->
        <MaximumSnapshotsRequired>3</MaximumSnapshotsRequired>
        <!-- The maximum number of problems that we can be tracking at any time. -->
        <MaximumCollectionPlanSize>50</MaximumCollectionPlanSize>
        <!-- How often to reset problem counters. -->
        <ProblemCounterResetInterval>06:00:00</ProblemCounterResetInterval>
        <!-- The maximum number of snapshots allowed in one minute. -->
        <SnapshotsPerMinuteLimit>2</SnapshotsPerMinuteLimit>
        <!-- The maximum number of snapshots allowed per day. -->
        <SnapshotsPerDayLimit>50</SnapshotsPerDayLimit>
        </Add>
    </TelemetryProcessors>
    ```

4. Momentaufnahmen werden nur zu Ausnahmen erfasst, die Application Insights gemeldet wurden. In einigen Fällen (z.B. bei älteren Versionen der .NET-Plattform) müssen Sie möglicherweise die [Ausnahmesammlung konfigurieren](app-insights-asp-net-exceptions.md#exceptions), um Ausnahmen mit Momentaufnahmen im Portal zu sehen.


### <a name="configure-snapshot-collection-for-aspnet-core-20-applications"></a>Konfigurieren der Momentaufnahmesammlung für ASP.NET Core 2.0-Anwendungen

1. Falls noch nicht geschehen, [aktivieren Sie Application Insights für Ihre ASP.NET Core-Web-App](app-insights-asp-net-core.md).

2. Binden Sie das NuGet-Paket [Microsoft.ApplicationInsights.SnapshotCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) in Ihre App ein.

3. Ändern Sie die `ConfigureServices`-Methode in der Klasse `Startup` Ihrer Anwendung, um den Telemetrieprozessor des Momentaufnahmesammlers hinzuzufügen. Der hinzuzufügende Code hängt von der referenzierten Version des Microsoft.ApplicationInsights.ASPNETCore-NuGet-Pakets ab.

   Fügen Sie für Microsoft.ApplicationInsights.AspNetCore 2.1.0 Folgendes hinzu:
   ```C#
   using Microsoft.ApplicationInsights.SnapshotCollector;
   ...
   class Startup
   {
       // This method is called by the runtime. Use it to add services to the container.
       public void ConfigureServices(IServiceCollection services)
       {
           services.AddSingleton<Func<ITelemetryProcessor, ITelemetryProcessor>>(next => new SnapshotCollectorTelemetryProcessor(next));
           // TODO: Add any other services your application needs here.
       }
   }
   ```

   Fügen Sie für Microsoft.ApplicationInsights.AspNetCore 2.1.1 Folgendes hinzu:
   ```C#
   using Microsoft.ApplicationInsights.SnapshotCollector;
   ...
   class Startup
   {
       private class SnapshotCollectorTelemetryProcessorFactory : ITelemetryProcessorFactory
       {
           public ITelemetryProcessor Create(ITelemetryProcessor next) =>
               new SnapshotCollectorTelemetryProcessor(next);
       }

       // This method is called by the runtime. Use it to add services to the container.
       public void ConfigureServices(IServiceCollection services)
       {
            services.AddSingleton<ITelemetryProcessorFactory>(new SnapshotCollectorTelemetryProcessorFactory());
           // TODO: Add any other services your application needs here.
       }
   }
   ```

### <a name="configure-snapshot-collection-for-other-net-applications"></a>Konfigurieren der Momentaufnahmesammlung für andere .NET-Anwendungen

1. Wenn Ihre Anwendung noch nicht mit Application Insights instrumentiert wurde, beginnen Sie mit dem [Aktivieren von Application Insights und Festlegen des Instrumentierungsschlüssels](app-insights-windows-desktop.md).

2. Fügen Sie das NuGet-Paket [Microsoft.ApplicationInsights.SnapshotCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) in Ihrer App hinzu.

3. Momentaufnahmen werden nur zu Ausnahmen erfasst, die Application Insights gemeldet wurden. Sie müssen möglicherweise den Code ändern, um diese melden zu können. Der Code zur Behandlung von Ausnahmen hängt von der Struktur Ihrer Anwendung ab. Im Folgenden wird jedoch ein Beispiel gezeigt:
    ```C#
   TelemetryClient _telemetryClient = new TelemetryClient();

   void ExampleRequest()
   {
        try
        {
            // TODO: Handle the request.
        }
        catch (Exception ex)
        {
            // Report the exception to Application Insights.
            _telemetryClient.TrackException(ex);

            // TODO: Rethrow the exception if desired.
        }
   }
    ```
    
## <a name="grant-permissions"></a>Erteilen von Berechtigungen

Besitzer des Azure-Abonnements können Momentaufnahmen überprüfen. Anderen Benutzern muss durch einen Besitzer eine entsprechende Berechtigung erteilt werden.

Weisen Sie hierzu den Benutzern, die Momentaufnahmen untersuchen, die Rolle `Application Insights Snapshot Debugger` zu. Abonnementbesitzer können diese Rolle einzelnen Benutzern oder Gruppen für die Application Insights-Zielressource oder für die dazugehörige Ressourcengruppe oder das dazugehörige Abonnement zuweisen.

1. Öffnen Sie das Blatt „Zugriffssteuerung (IAM)“.
1. Klicken Sie auf die Schaltfläche „+Hinzufügen“.
1. Wählen Sie in der Dropdownliste „Rollen“ die Option „Application Insights-Momentaufnahmedebugger“ aus.
1. Suchen Sie nach dem hinzuzufügenden Benutzer, und geben Sie einen Namen für ihn ein.
1. Klicken Sie auf die Schaltfläche „Speichern“, um den Benutzer der Rolle hinzuzufügen.


[!IMPORTANT]
    In Variablen- und Parameterwerten von Momentaufnahmen können persönliche und andere vertrauliche Informationen enthalten sein.

## <a name="debug-snapshots-in-the-application-insights-portal"></a>Debuggen von Momentaufnahmen im Application Insights-Portal

Wenn eine Momentaufnahme für eine bestimmte Ausnahme oder eine Problem-ID verfügbar ist, wird unter der [Ausnahme](app-insights-asp-net-exceptions.md) im Application Insights-Portal eine Schaltfläche zum **Erstellen einer Debug-Momentaufnahme angezeigt.**

![Schaltfläche zum Erstellen einer Debug-Momentaufnahme für eine Ausnahme](./media/app-insights-snapshot-debugger/snapshot-on-exception.png)

In der Ansicht der Debug-Momentaufnahme sehen Sie eine Aufrufliste und einen Variablenbereich. Wenn Sie Frames der Aufrufliste im Aufruflistenbereich auswählen, können Sie lokale Variablen und Parameter für diesen Funktionsaufruf im Variablenbereich anzeigen.

![Anzeigen der Debug-Momentaufnahme im Portal](./media/app-insights-snapshot-debugger/open-snapshot-portal.png)

Momentaufnahmen enthalten möglicherweise vertrauliche Informationen und können standardmäßig nicht angezeigt werden. Zum Anzeigen von Momentaufnahmen muss Ihnen die Rolle `Application Insights Snapshot Debugger` zugewiesen sein.

## <a name="debug-snapshots-with-visual-studio-2017-enterprise"></a>Debuggen von Momentaufnahmen mit Visual Studio 2017 Enterprise
1. Klicken Sie auf die Schaltfläche **Momentaufnahme herunterladen**, um eine Datei vom Typ `.diagsession` herunterzuladen, die von Visual Studio 2017 Enterprise geöffnet werden kann. 

2. Zum Öffnen der Datei vom Typ `.diagsession` müssen Sie zuerst [die Momentaufnahmedebugger-Erweiterung für Visual Studio herunterladen und installieren](https://aka.ms/snapshotdebugger).

3. Nach dem Öffnen der Momentaufnahmedatei erscheint in Visual Studio die Minidump-Debugging-Seite. Klicken Sie auf **Debug Managed Code** (Verwalteten Code debuggen), um mit dem Debuggen der Momentaufnahme zu beginnen. Die Momentaufnahme wird bei der Codezeile geöffnet, in der die Ausnahme ausgelöst wurde, damit Sie den aktuellen Zustand des Prozesses debuggen können.

    ![Anzeigen der Debugmomentaufnahme in Visual Studio](./media/app-insights-snapshot-debugger/open-snapshot-visualstudio.png)

Die heruntergeladene Momentaufnahme enthält alle Symboldateien, die auf Ihrem Webanwendungsserver gefunden wurden. Diese Symboldateien sind zum Zuordnen von Quellcode und Momentaufnahmedaten erforderlich. Achten Sie bei App Service-Apps darauf, dass die Symbolbereitstellung aktiviert ist, wenn Sie Ihre Web-Apps veröffentlichen.

## <a name="how-snapshots-work"></a>Funktionsweise von Momentaufnahmen

Beim Starten Ihrer Anwendung wird ein separater Uploader-Prozess für die Momentaufnahme erstellt, der Ihre Anwendung auf Momentaufnahmeanforderungen überwacht. Beim Anfordern einer Momentaufnahme wird innerhalb von etwa 10 bis 20 Millisekunden eine Schattenkopie des ausgeführten Prozesses erstellt. Der Schattenprozess wird dann analysiert, und eine Momentaufnahme wird erstellt, während der Hauptprozess weiterhin ausgeführt wird und Datenverkehr für Benutzer verarbeitet. Die Momentaufnahme wird dann mit allen relevanten Symboldateien (PDB-Dateien), die zum Anzeigen der Momentaufnahme erforderlich sind, in Application Insights hochgeladen.

## <a name="current-limitations"></a>Aktuelle Einschränkungen

### <a name="publish-symbols"></a>Veröffentlichen von Symbolen
Für den Momentaufnahmedebugger müssen Symboldateien auf dem Produktionsserver vorhanden sein, um Variablen zu decodieren und eine gute Debugleistung in Visual Studio zu erzielen. Die Version 15.2 von Visual Studio 2017 veröffentlicht Symbole für Releasebuilds standardmäßig im Rahmen der Veröffentlichung für App Service. In Vorgängerversionen müssen Sie der Veröffentlichungsprofildatei vom Typ `.pubxml` die folgende Zeile hinzufügen, um Symbole im Releasemodus zu veröffentlichen:

```xml
    <ExcludeGeneratedDebugSymbol>False</ExcludeGeneratedDebugSymbol>
```

Stellen Sie für Azure Compute und andere Typen sicher, dass die Symboldateien im selben Ordner wie die DLL-Datei der Hauptanwendung (in der Regel `wwwroot/bin`) liegen oder unter dem aktuellen Pfad verfügbar sind.

### <a name="optimized-builds"></a>Optimierte Builds
In einigen Fällen können lokale Variablen aufgrund von Optimierungen, die während des Buildprozesses angewendet werden, nicht in Releasebuilds angezeigt werden.

## <a name="troubleshooting"></a>Problembehandlung

Die folgenden Tipps unterstützen bei der Behandlung von Problemen mit dem Momentaufnahmedebugger.

### <a name="verify-the-instrumentation-key"></a>Überprüfen des Instrumentierungsschlüssels

Stellen Sie sicher, dass Sie den richtigen Instrumentierungsschlüssel in der veröffentlichten Anwendung verwenden. In der Regel liest Application Insights den Instrumentierungsschlüssel aus der Datei „ApplicationInsights.config“ aus. Stellen Sie sicher, dass der Wert mit dem Instrumentierungsschlüssel für die im Portal angezeigte Application Insights-Ressource identisch ist.

### <a name="check-the-uploader-logs"></a>Überprüfen der Uploaderprotokolle

Nachdem eine Momentaufnahme erstellt wurde, wird eine Minidumpdatei (DMP) auf dem Datenträger erstellt. Diese Minidumpdatei sowie alle zugehörigen PDB-Dateien werden in einem separaten Uploadervorgang in den Speicher des Application Insights-Momentaufnahmedebuggers hochgeladen. Nachdem die Minidumpdatei erfolgreich hochgeladen wurde, wird sie vom Datenträger gelöscht. Die Protokolldateien für den Minidumpuploader werden auf dem Datenträger beibehalten. In einer App Service-Umgebung finden Sie diese Protokolle unter `D:\Home\LogFiles\Uploader_*.log`. Verwenden Sie die Verwaltungswebsite von Kudu für App Service, um nach diesen Protokolldateien zu suchen.

1. Öffnen Sie im Azure-Portal Ihre App Service-Anwendung.

2. Wählen Sie das Blatt **Erweiterte Tools** aus, oder suchen Sie nach **Kudu**.
3. Klicken Sie auf **Start**.
4. Wählen Sie im Dropdown-Listenfeld **Debugging-Konsole** die Option **CMD** aus.
5. Klicken Sie auf **LogFiles**.

Daraufhin sollte mindestens eine Datei mit einem Namen angezeigt werden, der mit `Uploader_` beginnt und die Erweiterung `.log` aufweist. Klicken Sie auf das entsprechende Symbol, um Protokolldateien herunterzuladen oder in einem Browser zu öffnen.
Der Dateiname enthält den Computernamen. Wenn Ihre App Service-Instanz auf mehreren Computern gehostet wird, werden separate Protokolldateien für jeden Computer erstellt. Wenn der Uploader eine neue Minidumpdatei erkennt, wird diese in der Protokolldatei erfasst. Im Folgenden wird ein Beispiel für einen erfolgreichen Upload gezeigt:

```
MinidumpUploader.exe Information: 0 : Dump available 139e411a23934dc0b9ea08a626db16c5.dmp
    DateTime=2017-05-25T14:25:08.0349846Z
MinidumpUploader.exe Information: 0 : Uploading D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\139e411a23934dc0b9ea08a626db16c5.dmp, 329.12 MB
    DateTime=2017-05-25T14:25:16.0145444Z
MinidumpUploader.exe Information: 0 : Upload successful.
    DateTime=2017-05-25T14:25:42.9164120Z
MinidumpUploader.exe Information: 0 : Extracting PDB info from D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\139e411a23934dc0b9ea08a626db16c5.dmp.
    DateTime=2017-05-25T14:25:42.9164120Z
MinidumpUploader.exe Information: 0 : Matched 2 PDB(s) with local files.
    DateTime=2017-05-25T14:25:44.2310982Z
MinidumpUploader.exe Information: 0 : Stamp does not want any of our matched PDBs.
    DateTime=2017-05-25T14:25:44.5435948Z
MinidumpUploader.exe Information: 0 : Deleted D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\139e411a23934dc0b9ea08a626db16c5.dmp
    DateTime=2017-05-25T14:25:44.6095821Z
```

Im vorherigen Beispiel lautet der Instrumentierungsschlüssel `c12a605e73c44346a984e00000000000`. Dieser Wert sollte dem Instrumentierungsschlüssel für Ihre Anwendung entsprechen.
Die Minidumpdatei ist einer Momentaufnahme mit der ID `139e411a23934dc0b9ea08a626db16c5` zugeordnet. Sie können diese ID später verwenden, um nach der zugehörigen Ausnahmetelemetrie in Application Insights Analytics zu suchen.

Der Uploader sucht alle 15 Minuten nach neuen PDB-Dateien. Hier sehen Sie ein Beispiel:

```
MinidumpUploader.exe Information: 0 : PDB rescan requested.
    DateTime=2017-05-25T15:11:38.8003886Z
MinidumpUploader.exe Information: 0 : Scanning D:\home\site\wwwroot\ for local PDBs.
    DateTime=2017-05-25T15:11:38.8003886Z
MinidumpUploader.exe Information: 0 : Scanning D:\local\Temporary ASP.NET Files\root\a6554c94\e3ad6f22\assembly\dl3\81d5008b\00b93cc8_dec5d201 for local PDBs.
    DateTime=2017-05-25T15:11:38.8160276Z
MinidumpUploader.exe Information: 0 : Local PDB scan complete. Found 2 PDB(s).
    DateTime=2017-05-25T15:11:38.8316450Z
MinidumpUploader.exe Information: 0 : Deleted PDB scan marker D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\.pdbscan.
    DateTime=2017-05-25T15:11:38.8316450Z
```

Bei Anwendungen, die _nicht_ in App Service gehostet werden, befinden sich die Uploaderprotokolle im selben Ordner wie die Minidumpdateien: `%TEMP%\Dumps\<ikey>` (`<ikey>` steht hierbei für Ihren Instrumentierungsschlüssel).

### <a name="use-application-insights-search-to-find-exceptions-with-snapshots"></a>Suchen nach Ausnahmen mit Momentaufnahmen über die Application Insights-Suche

Wenn eine Momentaufnahme erstellt wird, wird die auslösende Ausnahme mit einer Momentaufnahme-ID gekennzeichnet. Wenn Application Insights die Ausnahmetelemetrie gemeldet wird, ist diese Momentaufnahme-ID als benutzerdefinierte Eigenschaft enthalten. In Application Insights können Sie auf dem Blatt „Suchen“ nach sämtlicher Telemetrie mit der benutzerdefinierten Eigenschaft `ai.snapshot.id` suchen.

1. Navigieren Sie im Azure-Portal zu Ihrer Application Insights-Ressource.
2. Klicken Sie auf **Suchen**.
3. Geben Sie in das Textfeld „Suchen“ `ai.snapshot.id` ein, und drücken Sie die Eingabetaste.

![Suchen nach Telemetrie im Portal mit einer Momentaufnahme-ID](./media/app-insights-snapshot-debugger/search-snapshot-portal.png)

Wenn bei dieser Suche keine Ergebnisse zurückgegeben werden, wurden Application Insights im ausgewählten Zeitraum keine Momentaufnahmen für Ihre Anwendung gemeldet.

Um über die Uploaderprotokolle nach einer bestimmten Momentaufnahme-ID zu suchen, geben Sie die jeweilige ID in das Suchfeld ein. Wenn Sie für eine Momentaufnahme, die Ihres Wissens nach hochgeladen wurde, keine Telemetrie finden können, gehen Sie folgendermaßen vor:

1. Vergewissern Sie sich, dass Sie die Suche in der richtigen Application Insights-Ressource durchführen, indem Sie den Instrumentierungsschlüssel überprüfen.

2. Passen Sie mithilfe des Zeitstempels aus dem Uploaderprotokoll den Filter „Zeitbereich“ an, um den jeweiligen Zeitbereich bei der Suche zu berücksichtigen.

Wenn trotzdem keine Ausnahme mit dieser Momentaufnahme-ID angezeigt wird, wurde Application Insights die Ausnahmetelemetrie nicht gemeldet. Diese Situation kann vorkommen, wenn Ihre Anwendung abgestürzt ist, nachdem die Momentaufnahme erfasst wurde, jedoch bevor die Ausnahmetelemetrie gemeldet wurde. Überprüfen Sie in diesem Fall die App Service-Protokolle unter `Diagnose and solve problems`, um festzustellen, ob unerwartete Neustarts oder Ausnahmefehler vorliegen.

## <a name="next-steps"></a>Nächste Schritte

* [Legen Sie Fangpunkte in Ihrem Code fest](https://azure.microsoft.com/blog/snapshot-debugger-for-azure/), um Momentaufnahmen abzurufen, ohne auf eine Ausnahme warten zu müssen.
* Unter [Diagnostizieren von Ausnahmen in Ihren Web-Apps](app-insights-asp-net-exceptions.md) erfahren Sie, wie Sie weitere Ausnahmen für Application Insights sichtbar machen. 
* Die [intelligente Erkennung](app-insights-proactive-diagnostics.md) ermittelt automatisch Leistungsanomalien.

