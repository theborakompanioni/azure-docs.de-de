---
title: Senden von Azure-Diagnoseprotokollen an Application Insights | Microsoft Docs
description: Details zum Konfigurieren der Azure Cloud Services-Protokolle, die an das Application Insights-Portal gesendet werden.
services: application-insights
documentationcenter: .net
author: sbtron
manager: douge
ms.assetid: a67dd20a-fc5d-4391-ba63-bfe164fb62f7
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/17/2015
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 2843ff25c118afe5d5421b322b529e9a9f2d5036


---
# <a name="configure-azure-diagnostic-logging-to-application-insights"></a>Konfigurieren der Azure-Diagnose zum Protokollieren in Application Insights
Wenn Sie ein Cloud Services-Projekt oder einen virtuellen Computer in Microsoft Azure einrichten, [kann in Azure ein Diagnoseprotokoll generiert werden](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md). Sie können veranlassen, dass dieses Protokoll an Application Insights gesendet wird. Danach können Sie es gemeinsam mit der Diagnose- und Nutzungstelemetrie auswerten, die das Application Insights SDK aus der App heraus sendet. Das Azure-Protokoll enthält Ereignisse beim Verwalten der App, z. B. Starten, Beenden, Abstürze sowie Leistungsindikatoren. Das Protokoll enthält außerdem Aufrufe von „System.Diagnostics.Trace“ in der App.

Dieser Artikel beschreibt im Detail, wie Sie das Erfassen der Diagnose konfigurieren.

Als Voraussetzung muss das Azure SDK 2.8 oder höher in Visual Studio installiert sein.

## <a name="get-an-application-insights-resource"></a>Abrufen einer Application Insights-Ressource
Um optimale Ergebnisse zu erhalten, [fügen Sie das Application Insights SDK jeder Rolle Ihrer Cloud Services-App hinzu](app-insights-cloudservices.md). Sie können es auch [jeder App hinzufügen, die auf einem virtuellen Computer ausgeführt wird](app-insights-overview.md). Anschließend können Sie die zu analysierenden und anzuzeigenden Diagnosedaten an dieselbe Application Insights-Ressource senden.

Wenn Sie das SDK nicht verwenden möchten, z.B. weil die App schon live geschaltet ist, können Sie alternativ im Azure-Portal einfach [eine neue Application Insights-Ressource erstellen](app-insights-create-new-resource.md). Wählen Sie dazu **Azure-Diagnose** als Anwendungstyp aus.

## <a name="send-azure-diagnostics-to-application-insights"></a>Senden von Azure-Diagnosedaten an Application Insights
Wenn Sie Ihr App-Projekt aktualisieren können, wählen Sie in Visual Studio jede einzelne Rolle aus, wählen Sie die zugehörigen Eigenschaften, und wählen Sie auf der Registerkarte „Konfiguration“ die Option **Diagnosedaten an Application Insights senden**.

Wenn Sie Ihre App mit dem Befehl „Veröffentlichen“ hochladen, können Sie alternativ die Option „Application Insights“ auf der Registerkarte „Diagnose“ auswählen.

Wenn Ihre App bereits live geschaltet ist, verwenden Sie den Server-Explorer von Visual Studio oder den Cloud Services-Explorer, um die Eigenschaften der App zu öffnen. Wählen Sie **Diagnosedaten an Application Insights senden**aus.

In beiden Fällen werden Sie aufgefordert, die Details der erstellten Application Insights-Ressource anzugeben.

Es empfiehlt sich, die Daten für separate Rollen an separate Ressourcen zu senden. Die zugehörigen Metrikdiagramme können Sie durch Erstellen eines [Dashboards](app-insights-dashboards.md) nebeneinander im Portal anzeigen.

[Erfahren Sie mehr über das Einrichten von Application Insights für eine Cloud Services-App](app-insights-cloudservices.md).

## <a name="configuring-the-azure-diagnostics-adapter"></a>Konfigurieren des Azure-Diagnoseadapters
Lesen Sie hier nur weiter, wenn Sie die Teile des Protokolls auswählen möchten, die an Application Insights gesendet werden. In der Standardeinstellung wird alles gesendet, also z. B. Microsoft Azure-Ereignisse, Leistungsindikatoren und Aufrufe der Ablaufverfolgung „System.Diagnostics.Trace“ aus der App.

Wenn Sie die Diagnoseoptionen im Editor für Rolleneigenschaften oder im Veröffentlichungs-Assistenten ändern, ändern Sie eigentlich den Inhalt von zwei Dateigruppen:

* [Diagnosekonfigurationsdateien](https://msdn.microsoft.com/library/azure/dn782207.aspx) für Rollen. Diese finden Sie im Projektmappen-Explorer unter `<Your Service>/Roles/*/diagnostics.wadcfgx`.
* Dienstkonfigurationsdateien: `ServiceConfiguration.*.cscfg`.

Bearbeiten Sie diese Dateien direkt, um spezifischere Optionen festzulegen, als dies die Assistenten ermöglichen. Im Folgenden erhalten Sie ausführlichere Informationen. 

## <a name="separate-development-and-production-resources"></a>Trennen von Entwicklungs- und Produktionsressourcen
Sie können die Telemetriedaten der Entwicklungs- und Produktionsstempel Ihrer App an unterschiedliche Application Insights-Ressourcen senden. Dadurch werden die Entwicklungstelemetriedaten nicht mit Livetelemetriedaten überladen. 

1. [Erstellen Sie neue Application Insights-Ressourcen](app-insights-create-new-resource.md) für jeden Stempel. Rufen Sie den Instrumentierungsschlüssel auf der Registerkarte „Zusammenfassung“ der einzelnen Ressourcen ab.
2. Bearbeiten Sie die beiden CSCFG-Dateien, und fügen Sie andere Instrumentierungsschlüssel ein.

## <a name="choose-the-priority-levels-to-send"></a>Auswählen der zu sendenden Prioritätsstufen
In der Diagnosekonfigurationsdatei `diagnostics.wadcfgx` für die einzelnen Rollen können Sie die Protokollmeldungen nach Stufe filtern.

### <a name="define-a-sink"></a>Definieren einer Senke
`<SinksConfig>` definiert die zusätzliche Senke, an die die Azure-Diagnosedaten gesendet werden können.  Eine Beispiel- `SinksConfig` sieht wie folgt aus:

```xml

    <SinksConfig>
     <Sink name="ApplicationInsights">
      <ApplicationInsights/>
      <Channels>
        <Channel logLevel="Error" name="MyTopDiagData"  />
        <Channel logLevel="Verbose" name="MyLogData"  />
      </Channels>
     </Sink>
    </SinksConfig>

```

`Channels` benennt einen Datenstrom, der an die Senke gesendet wird. Der Kanal verhält sich wie ein Filter. Mit dem `loglevel` -Attribut können Sie den Protokolliergrad angeben, den der Kanal übermittelt. Die verfügbaren Werte sind: `{Verbose, Information, Warning, Error, Critical}`

### <a name="send-data-to-the-sink"></a>Senden von Daten an die Senke
Sie senden Daten an die Application Insights-Senke, indem Sie das sinks-Attribut unter dem Knoten DiagnosticMonitorConfiguration oder einem ihm untergeordneten Knoten hinzufügen. Durch Hinzufügen des sinks-Elements für jeden Knoten geben Sie an, dass die von jedem Knoten und den untergeordneten Knoten gesammelten Daten an die angegebene Senke gesendet werden sollen.

Beispielsweise werden in der vom Azure SDK erstellten Standardeinstellung alle Azure-Diagnosedaten gesendet:

```xml

    <DiagnosticMonitorConfiguration overallQuotaInMB="4096" sinks="ApplicationInsights">
```

Wenn Sie jedoch nur die Fehlerprotokolle senden möchten, qualifizieren Sie den Namen der Senke durch den Namen eines Kanals:

```xml

    <DiagnosticMonitorConfiguration overallQuotaInMB="4096" sinks="ApplicationInsights.MyTopDiagdata">
```

Beachten Sie, dass hier der Namen der oben definierten Senke zusammen mit dem Namen des oben definierten Kanals verwendet wird.

Wenn Sie nur ausführliche Anwendungsprotokolle an Application Insights senden möchten, müssten Sie dem `Logs` -Knoten das sinks-Attribut hinzufügen.

```xml

    <Logs scheduledTransferPeriod="PT1M" scheduledTransferLogLevelFilter="Verbose" sinks="ApplicationInsights.MyLogData"/>
```

Sie können auch mehrere Senken in der Konfiguration auf unterschiedlichen Ebenen der Hierarchie einschließen. In diesem Fall verhält sich die auf der obersten Ebene der Hierarchie angegebene Senke wie eine globale Einstellung, und die auf der einzelnen Elementebene angegebene Senke verhält sich wie eine Korrektur der globalen Einstellung.

Hier ist ein vollständiges Beispiel für die öffentliche Konfigurationsdatei, die alle Fehler an Application Insights sendet (angegeben unter dem `DiagnosticMonitorConfiguration`-Knoten) und zusätzlich Protokolle mit dem Protokolliergrad „Ausführlich“ für die Anwendungsprotokolle (angegeben unter dem `Logs`-Knoten).

```xml

    <WadCfg>
     <DiagnosticMonitorConfiguration overallQuotaInMB="4096"
       sinks="ApplicationInsights.MyTopDiagData">
       <!-- All info below sent to this channel -->
      <DiagnosticInfrastructureLogs />
      <PerformanceCounters>
        <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" sinks="ApplicationInsights.MyLogData/>
        <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
        <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
      </PerformanceCounters>
      <WindowsEventLog scheduledTransferPeriod="PT1M">
        <DataSource name="Application!*" />
      </WindowsEventLog>
      <Logs scheduledTransferPeriod="PT1M" scheduledTransferLogLevelFilter="Verbose"
            sinks="ApplicationInsights.MyLogData"/>
       <!-- This specific info sent to this channel -->
     </DiagnosticMonitorConfiguration>

     <SinksConfig>
      <Sink name="ApplicationInsights">
        <ApplicationInsights/>
        <Channels>
          <Channel logLevel="Error" name="MyTopDiagData"  />
          <Channel logLevel="Verbose" name="MyLogData"  />
        </Channels>
      </Sink>
     </SinksConfig>
    </WadCfg>
```


Bei diesen Funktionen sind einige Einschränkungen zu beachten:

* Kanäle sind nur für die Arbeit mit Protokolltyp und nicht mit Leistungsindikatoren vorgesehen. Wenn Sie einen Kanal mit einem Leistungsindikatorelement angeben, wird es ignoriert.
* Der Protokolliergrad für einen Kanal darf den Protokolliergrad für die von der Azure-Diagnose erfassten Daten nicht überschreiten. Beispiel: Sie können keine Anwendungsprotokollfehler im Logs-Element sammeln und können nicht versuchen, ausführliche Protokolle an die Application Insight-Synchronisierung zu senden. Das scheduledTransferLogLevelFilter-Attribut muss immer mindestens genauso viele Protokolle sammeln wie die Protokolle, die Sie an eine Senke senden möchten.
* Sie können keine von der Azure-Diagnose-Erweiterung gesammelten Blobdaten an Application Insights senden. Dazu gehören z. B. alle Daten unter dem Directories-Knoten. Bei Absturzabbildern wird das tatsächliche Absturzabbild weiterhin an den Blobspeicher gesendet, und es wird nur eine Benachrichtigung, dass das Absturzabbild generiert wurde, an Application Insights gesendet.

## <a name="next-steps"></a>Nächste Schritte
* [Überwachen von Azure Cloud Services mit Application Insights](app-insights-cloudservices.md)
* [Senden von Azure-Diagnosedaten an Application Insights mit PowerShell](app-insights-powershell-azure-diagnostics.md)
* [Azure-Diagnosekonfigurationsdatei](https://msdn.microsoft.com/library/azure/dn782207.aspx)




<!--HONumber=Nov16_HO3-->


