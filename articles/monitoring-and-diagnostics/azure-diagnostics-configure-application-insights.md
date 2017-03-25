---
title: Konfigurieren der Azure-Diagnose zum Senden von Daten an Application Insights | Microsoft-Dokumentation
description: "Aktualisieren Sie die öffentliche Konfiguration von Azure-Diagnose, um Daten an Application Insights zu senden."
services: monitoring-and-diagnostics
documentationcenter: .net
author: rboucher
manager: carmonm
editor: 
ms.assetid: f9e12c3e-c307-435e-a149-ef0fef20513a
ms.service: monitoring-and-diagnostics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/19/2016
ms.author: robb
translationtype: Human Translation
ms.sourcegitcommit: 424d8654a047a28ef6e32b73952cf98d28547f4f
ms.openlocfilehash: f6848fef5b23a864496565334b22dc2e2e8d1492
ms.lasthandoff: 03/22/2017


---
# <a name="send-cloud-service-virtual-machine-or-service-fabric-diagnostic-data-to-application-insights"></a>Senden von Cloud Services-, Virtual Machines- oder Service Fabric-Diagnosedaten an Application Insights
Cloud Services, Virtual Machines, VM-Skalierungsgruppen und Service Fabric verwenden die Azure-Diagnoseerweiterung zum Sammeln von Daten.  Die Azure-Diagnose sendet die Daten an Azure Storage-Tabellen.  Sie können jedoch auch alle oder einen Teil der Daten mithilfe der Azure-Diagnoseerweiterung 1.5 oder höher an andere Speicherorte weiterleiten.

Dieser Artikel beschreibt, wie Daten der Azure-Diagnoseerweiterung an Application Insights gesendet werden.

## <a name="diagnostics-configuration-explained"></a>Erläuterung der Diagnosekonfiguration
Mit der Azure-Diagnoseerweiterung 1.5 wurden Senken eingeführt, die zusätzliche Speicherorte darstellen, an die Sie Diagnosedaten senden können.

Beispielkonfiguration einer Senke für Application Insights:

```XML
    <SinksConfig>
        <Sink name="ApplicationInsights">
          <ApplicationInsights>{Insert InstrumentationKey}</ApplicationInsights>
          <Channels>
            <Channel logLevel="Error" name="MyTopDiagData"  />
            <Channel logLevel="Verbose" name="MyLogData"  />
          </Channels>
        </Sink>
      </SinksConfig>
```

- Das Attribut **Sink** *Name* ist ein Zeichenfolgenwert, der die Senke eindeutig identifiziert.

- Das Element **ApplicationInsights** gibt den Instrumentierungsschlüssel der Application Insights-Ressource an, an die die Azure-Diagnosedaten gesendet werden.
    - Wenn Sie keine vorhandene Application Insights-Ressource besitzen, finden Sie weitere Informationen zum Erstellen einer Ressource sowie zum Abrufen des Instrumentierungsschlüssels unter [Erstellen einer neuen Application Insights-Ressource](../application-insights/app-insights-create-new-resource.md).
    - Wenn Sie einen Clouddienst mit dem Azure SDK 2.8 und höher entwickeln, wird dieser Instrumentierungsschlüssel automatisch aufgefüllt. Der Wert basiert auf der Dienstkonfigurationseinstellung **APPINSIGHTS_INSTRUMENTATIONKEY**, die beim Verpacken des Clouddienstprojekts festgelegt wird. Weitere Informationen finden Sie unter [Verwenden von Application Insights mit Azure-Diagnose zum Beheben von Clouddienstproblemen](../cloud-services/cloud-services-dotnet-diagnostics-applicationinsights.md).

- Das Element **Channels** enthält ein oder mehrere **Channel**-Elemente.
    - Das Attribut *Name* gibt diesen Kanal eindeutig an.
    - Mit dem Attribut *logLevel* können Sie den Protokolliergrad angeben, den der Kanal zulässt. Die verfügbaren Protokolliergrade in der Reihenfolge ihrer Ausführlichkeit:
        - Ausführlich
        - Information
        - Warnung
        - Fehler
        - Kritisch

Ein Kanal verhält sich wie ein Filter und ermöglicht es Ihnen, bestimmte Protokolliergrade auszuwählen, die an die Zielsenke gesendet werden. Beispielsweise könnten Sie ausführliche Protokolle sammeln und an den Speicher senden, aber nur Fehler an die Senke.

Die folgende Abbildung zeigt diese Beziehung.

![Öffentliche Diagnosekonfiguration](./media/azure-diagnostics-configure-applicationinsights/AzDiag_Channels_App_Insights.png)

Die folgende Abbildung fasst die Konfigurationswerte und deren Funktionsweise zusammen. Sie können in die Konfiguration mehrere Senken auf unterschiedlichen Ebenen der Hierarchie einschließen. Die Senke der obersten Ebene verhält sich wie eine globale Einstellung, und die für das einzelne Element angegebene Senke verhält sich wie eine Korrektur der globalen Einstellung.

![Konfiguration von Diagnosesenken mit Application Insights](./media/azure-diagnostics-configure-applicationinsights/Azure_Diagnostics_Sinks.png)

## <a name="complete-sink-configuration-example"></a>Vollständiges Beispiel für eine Senkenkonfiguration
Hier sehen Sie ein vollständiges Beispiel für eine öffentliche Konfigurationsdatei, die Folgendes ausführt:
1. Sendet alle Fehler an Application Insights (angegeben im Knoten **DiagnosticMonitorConfiguration**)
2. Sendet zudem ausführliche Protokolle für die Anwendungsprotokolle (angegeben im Knoten **Logs**).

```XML
<WadCfg>
  <DiagnosticMonitorConfiguration overallQuotaInMB="4096"
       sinks="ApplicationInsights.MyTopDiagData"> <!-- All info below sent to this channel -->
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
            sinks="ApplicationInsights.MyLogData"/> <!-- This specific info sent to this channel -->
  </DiagnosticMonitorConfiguration>

<SinksConfig>
    <Sink name="ApplicationInsights">
      <ApplicationInsights>{Insert InstrumentationKey}</ApplicationInsights>
      <Channels>
        <Channel logLevel="Error" name="MyTopDiagData"  />
        <Channel logLevel="Verbose" name="MyLogData"  />
      </Channels>
    </Sink>
  </SinksConfig>
</WadCfg>
```

In der vorherigen Konfiguration haben die folgenden Zeilen die folgende Bedeutung:

### <a name="send-all-the-data-that-is-being-collected-by-azure-diagnostics"></a>Senden aller Daten, die von der Azure-Diagnose gesammelt werden

```XML
<DiagnosticMonitorConfiguration overallQuotaInMB="4096" sinks="ApplicationInsights">
```

### <a name="send-only-error-logs-to-the-application-insights-sink"></a>Ausschließliches Senden von Fehlerprotokollen an die Application Insights-Senke

```XML
<DiagnosticMonitorConfiguration overallQuotaInMB="4096" sinks="ApplicationInsights.MyTopDiagdata">
```

### <a name="send-verbose-application-logs-to-application-insights"></a>Senden von ausführlichen Anwendungsprotokollen an Application Insights

```XML
<Logs scheduledTransferPeriod="PT1M" scheduledTransferLogLevelFilter="Verbose" sinks="ApplicationInsights.MyLogData"/>
```

## <a name="limitations"></a>Einschränkungen

- **Nur Kanäle werden protokolliert, keine Leistungsindikatoren.** Wenn Sie einen Kanal mit einem Leistungsindikatorelement angeben, wird es ignoriert.
- **Der Protokolliergrad für einen Kanal darf den Protokolliergrad für die von der Azure-Diagnose erfassten Daten nicht überschreiten.** Beispiel: Sie können keine Anwendungsprotokollfehler im Element „Logs“ sammeln und können nicht versuchen, ausführliche Protokolle an die Application Insight-Senke zu senden. Das *scheduledTransferLogLevelFilter*-Attribut muss immer mindestens genauso viele Protokolle sammeln wie die Protokolle, die Sie an eine Senke senden möchten.
- **Sie können keine von der Azure-Diagnoseerweiterung gesammelten Blobdaten an Application Insights senden.** Beispiel: Alle Daten unter dem Knoten *Directories*. Bei Absturzabbildern wird das tatsächliche Absturzabbild weiterhin an den Blobspeicher gesendet, und es wird nur eine Benachrichtigung, dass das Absturzabbild generiert wurde, an Application Insights gesendet.

## <a name="next-steps"></a>Nächste Schritte
* Verwenden Sie [PowerShell](../cloud-services/cloud-services-diagnostics-powershell.md) , um die Azure-Diagnose-Erweiterung für Ihre Anwendung zu aktivieren.
* Verwenden Sie [Visual Studio](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md) , um die Azure-Diagnose-Erweiterung für Ihre Anwendung zu aktivieren.

