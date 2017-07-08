---
title: Sammeln von Protokollen direkt aus einem Azure Service Fabric-Dienstprozess | Microsoft Azure
description: "Es wird beschrieben, dass Service Fabric-Anwendungen Protokolle direkt an einen zentralen Speicherort, z.B. Azure Application Insights oder Elasticsearch, senden können, ohne dass der Azure-Diagnose-Agent erforderlich ist."
services: service-fabric
documentationcenter: .net
author: karolz-ms
manager: rwike77
editor: 
ms.assetid: ab92c99b-1edd-4677-8c28-4e591d909b47
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/18/2017
ms.author: karolz
ms.translationtype: Human Translation
ms.sourcegitcommit: db18dd24a1d10a836d07c3ab1925a8e59371051f
ms.openlocfilehash: 08f7b57985382f2abbb90ba1e13a30f40b38917f
ms.contentlocale: de-de
ms.lasthandoff: 06/15/2017


---
# <a name="collect-logs-directly-from-an-azure-service-fabric-service-process"></a>Sammeln von Protokollen direkt aus einem Azure Service Fabric-Dienstprozess
## <a name="in-process-log-collection"></a>In-Process-Protokollsammlung
Das Sammeln von Anwendungsprotokollen per [Azure-Diagnose-Erweiterung](service-fabric-diagnostics-how-to-setup-wad.md) ist eine gute Option für **Azure Service Fabric**-Dienste, wenn die Gruppe von Protokollquellen und -zielen klein ist, sich nicht häufig ändert und eine einfache Zuordnung zwischen den Quellen und ihren Zielen besteht. Falls nicht, besteht eine Alternative darin, dass die Dienste ihre Protokolle direkt an einen zentralen Speicherort senden. Dieser Prozess wird als **In-Process-Protokollsammlung** bezeichnet und hat mehrere potenzielle Vorteile:

* *Einfache Konfiguration und Bereitstellung*

    * Die Konfiguration der Erfassung von Diagnosedaten ist Teil der Dienstkonfiguration. Es ist einfach, sie stets mit dem Rest der Anwendung „synchron“ zu halten.
    * Eine anwendungs- oder dienstspezifische Konfiguration ist problemlos möglich.
        * Für die Agent-basierte Protokollsammlung ist in der Regel eine separate Bereitstellung und Konfiguration des Diagnose-Agents erforderlich. Dies bedeutet eine zusätzliche Verwaltungsaufgabe und eine potenzielle Fehlerquelle. Häufig ist nur eine Instanz des Agents pro virtuellem Computer (Knoten) vorhanden, und die Agent-Konfiguration wird von allen Anwendungen und Diensten gemeinsam genutzt, die auf diesem Knoten ausgeführt werden. 

* *Flexibilität*
   
    * Die Anwendung kann die Daten an ein beliebiges Ziel senden – vorausgesetzt, es ist eine Clientbibliothek vorhanden, die das gewünschte Datenspeichersystem unterstützt. Neue Ziele können je nach Bedarf hinzugefügt werden.
    * Komplexe Erfassungs-, Filter- und Datenaggregierungsregeln können implementiert werden.
    * Die Agent-basierte Protokollsammlung ist häufig durch die Datensenken beschränkt, die vom Agent unterstützt werden. Einige Agents sind erweiterbar.

* *Zugriff auf interne Anwendungsdaten und Kontext*
   
    * Das Diagnosesubsystem innerhalb des Anwendungs-/Dienstprozesses kann die Ablaufverfolgungen problemlos mit Kontextinformationen ergänzen.
    * Bei der Agent-basierten Protokollsammlung müssen die Daten über einen prozessübergreifenden Kommunikationsmechanismus, z.B. die Ereignisablaufverfolgung für Windows, an einen Agent gesendet werden. Dieser Mechanismus hat unter Umständen weitere Einschränkungen zur Folge.

Es ist möglich, beide Sammlungsmethoden zu kombinieren und von Ihnen zu profitieren. In der Tat ist dies möglicherweise die beste Lösung für viele Anwendungen. Die Agent-basierte Sammlung ist eine natürliche Lösung zum Sammeln von Protokollen für den gesamten Cluster und einzelne Clusterknoten. Es ist eine deutlich zuverlässigere Möglichkeit als die In-Process-Protokollsammlung, um Probleme und Abstürze beim Starten von Diensten zu diagnostizieren. Wenn in einem Service Fabric-Cluster viele Dienste ausgeführt werden, kommt es für den Cluster zu einer hohen Zahl von ausgehenden Verbindungen, sofern jeder Dienst eine eigene In-Process-Protokollsammlung durchführt. Eine hohe Zahl von ausgehenden Verbindungen stellt sowohl für das Netzwerksubsystem als auch für das Protokollziel eine große Belastung dar. Ein Agent, z.B. [**Azure-Diagnose**](../cloud-services/cloud-services-dotnet-diagnostics.md), kann Daten von mehreren Diensten sammeln und alle Daten über eine geringe Zahl von Verbindungen senden, um den Durchsatz zu verbessern. 

In diesem Artikel wird veranschaulicht, wie Sie eine In-Process-Protokollsammlung per [**EventFlow-Open-Source-Bibliothek**](https://github.com/Azure/diagnostics-eventflow) einrichten. Für den gleichen Zweck können auch andere Bibliotheken verwendet werden, aber EventFlow hat den Vorteil, dass die Anwendung speziell für die In-Process-Protokollsammlung und die Unterstützung von Service Fabric-Diensten konzipiert wurde. Wir verwenden [**Azure Application Insights**](https://azure.microsoft.com/services/application-insights/) als Protokollziel. Andere Ziele, z.B. [**Event Hubs**](https://azure.microsoft.com/services/event-hubs/) oder [**Elasticsearch**](https://www.elastic.co/products/elasticsearch), werden ebenfalls unterstützt. Es müssen lediglich das richtige NuGet-Paket installiert und das Ziel in der EventFlow-Konfigurationsdatei konfiguriert werden. Weitere Informationen zu anderen Protokollzielen als Application Insights finden Sie in der [EventFlow-Dokumentation](https://github.com/Azure/diagnostics-eventflow).

## <a name="adding-eventflow-library-to-a-service-fabric-service-project"></a>Hinzufügen der EventFlow-Bibliothek zu einem Service Fabric-Dienstprojekt
EventFlow-Binärdateien sind als Gruppe von NuGet-Paketen verfügbar. Klicken Sie zum Hinzufügen von EventFlow zu einem Service Fabric-Dienstprojekt im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und wählen Sie die Option „NuGet-Pakete verwalten“. Wechseln Sie zur Registerkarte „Durchsuchen“, und suchen Sie nach „`Diagnostics.EventFlow`“:

![EventFlow-NuGet-Pakete in der NuGet-Paket-Manager-UI von Visual Studio][1]

Der Dienst, von dem EventFlow gehostet wird, sollte – je nach Quelle und Ziel für die Anwendungsprotokolle – die richtigen Pakete enthalten. Fügen Sie die folgenden Pakete hinzu: 

* `Microsoft.Diagnostics.EventFlow.Inputs.EventSource` 
    * (zur Erfassung von Daten über die EventSource-Klasse des Diensts und über standardmäßige EventSources, z.B. *Microsoft-ServiceFabric-Services* und *Microsoft-ServiceFabric-Actors*)
* `Microsoft.Diagnostics.EventFlow.Outputs.ApplicationInsights` 
    * (Wir senden die Protokolle an eine Azure Application Insights-Ressource.)  
* `Microsoft.Diagnostics.EventFlow.ServiceFabric` 
    * (Ermöglicht die Initialisierung der EventFlow-Pipeline über die Service Fabric-Dienstkonfiguration und meldet Probleme in Bezug auf das Senden von Diagnosedaten als Service Fabric-Integritätsberichte.)

> [!NOTE]
> Für das `Microsoft.Diagnostics.EventFlow.Inputs.EventSource`-Paket muss das Dienstprojekt auf .NET Framework 4.6 oder höher ausgerichtet sein. Achten Sie darauf, in den Projekteigenschaften das richtige Zielframework festzulegen, bevor Sie dieses Paket installieren. 

Nachdem alle Pakete installiert wurde, ist der nächste Schritt das Konfigurieren und Aktivieren von EventFlow im Dienst.

## <a name="configuring-and-enabling-log-collection"></a>Konfigurieren und Aktivieren der Protokollsammlung
Die EventFlow-Pipeline, die für das Senden der Protokolle zuständig ist, wird aus einer Spezifikation erstellt, die in einer Konfigurationsdatei gespeichert wird. Mit dem `Microsoft.Diagnostics.EventFlow.ServiceFabric`-Paket wird eine anfängliche EventFlow-Konfigurationsdatei im Projektmappenordner `PackageRoot\Config` installiert. Der Dateiname lautet `eventFlowConfig.json`. Diese Konfigurationsdatei muss geändert werden, um Daten aus der `EventSource`-Standarddienstklasse zu erfassen und Daten an den Application Insights-Dienst zu senden.

> [!NOTE]
> Wir gehen davon aus, dass sie mit dem **Azure Application Insights**-Dienst vertraut sind und über eine Application Insights-Ressource verfügen, die Sie zum Überwachen Ihres Service Fabric-Diensts verwenden möchten. Weitere Informationen finden Sie unter [Erstellen einer Application Insights-Ressource](../application-insights/app-insights-create-new-resource.md).

Öffnen Sie die Datei `eventFlowConfig.json` im Editor, und ändern Sie ihren Inhalt wie unten dargestellt. Stellen Sie sicher, dass Sie den ServiceEventSource-Namen und den Application Insights-Instrumentierungsschlüssel gemäß den Kommentaren ersetzen. 

```json
{
  "inputs": [
    {
      "type": "EventSource",
      "sources": [
        { "providerName": "Microsoft-ServiceFabric-Services" },
        { "providerName": "Microsoft-ServiceFabric-Actors" },
        // (replace the following value with your service's ServiceEventSource name)
        { "providerName": "your-service-EventSource-name" }
      ]
    }
  ],
  "filters": [
    {
      "type": "drop",
      "include": "Level == Verbose"
    }
  ],
  "outputs": [
    {
      "type": "ApplicationInsights",
      // (replace the following value with your AI resource's instrumentation key)
      "instrumentationKey": "00000000-0000-0000-0000-000000000000"
    }
  ],
  "schemaVersion": "2016-08-11"
}
```

> [!NOTE]
> Der Name des ServiceEventSource-Elements für den Dienst ist der Wert der Name-Eigenschaft des `EventSourceAttribute`-Elements, das auf die ServiceEventSource-Klasse angewendet wird. Alle Angaben sind in der Datei `ServiceEventSource.cs` enthalten, die Teil des Dienstcodes ist. Im folgenden Codeausschnitt lautet der Name des ServiceEventSource-Elements *MyCompany-Application1-Stateless1*:
> ```csharp
> [EventSource(Name = "MyCompany-Application1-Stateless1")]
> internal sealed class ServiceEventSource : EventSource
> {
>    // (rest of ServiceEventSource implementation)
>} 
> ```

Beachten Sie, dass die Datei `eventFlowConfig.json` Teil des Dienstkonfigurationspakets ist. Änderungen an dieser Datei können in vollständigen oder auf die Konfiguration beschränkten Upgrades des Diensts enthalten sein. Dies ist von Integritätsprüfungen des Service Fabric-Upgrades und dem automatischen Rollback, falls ein Upgradefehler auftritt, abhängig. Weitere Informationen finden Sie unter [Service Fabric-Anwendungsupgrade](service-fabric-application-upgrade.md).

Der letzte Schritt ist die Instanziierung der EventFlow-Pipeline im Startcode Ihres Diensts in der Datei `Program.cs`. Im folgenden Beispiel sind auf EventFlow bezogene Hinzufügungen mit Kommentaren gekennzeichnet, die mit `****` beginnen:

```csharp
using System;
using System.Diagnostics;
using System.Threading;
using Microsoft.ServiceFabric;
using Microsoft.ServiceFabric.Services.Runtime;

// **** EventFlow namespace
using Microsoft.Diagnostics.EventFlow.ServiceFabric;

namespace Stateless1
{
    internal static class Program
    {
        /// <summary>
        /// This is the entry point of the service host process.
        /// </summary>
        private static void Main()
        {
            try
            {
                // **** Instantiate log collection via EventFlow
                using (var diagnosticsPipeline = ServiceFabricDiagnosticPipelineFactory.CreatePipeline("MyApplication-MyService-DiagnosticsPipeline"))
                {

                    
                    ServiceRuntime.RegisterServiceAsync("Stateless1Type",
                    context => new Stateless1(context)).GetAwaiter().GetResult();

                    ServiceEventSource.Current.ServiceTypeRegistered(Process.GetCurrentProcess().Id, typeof(Stateless1).Name);

                    Thread.Sleep(Timeout.Infinite);
                }
            }
            catch (Exception e)
            {
                ServiceEventSource.Current.ServiceHostInitializationFailed(e.ToString());
                throw;
            }
        }
    }
}
```

Der Name, der als Parameter der `CreatePipeline`-Methode von `ServiceFabricDiagnosticsPipelineFactory` übergeben wird, ist der Name der *Integritätsentität*, der für die Pipeline der EventFlow-Protokollsammlung steht. Dieser Name wird verwendet, wenn für EventFlow ein Fehler auftritt, der über das Service Fabric-Integritätssubsystem gemeldet wird.

## <a name="verification"></a>Überprüfung
Starten Sie Ihren Dienst, und sehen Sie sich die Informationen im Fenster für die Debugausgabe von Visual Studio an. Nachdem der Dienst gestartet wurde, sollte angezeigt werden, dass der Dienst Datensätze vom Typ „Application Insights-Telemetrie“ sendet. Öffnen Sie einen Webbrowser, und navigieren Sie zu Ihrer Application Insights-Ressource. Öffnen Sie die Registerkarte „Suche“ (oben auf dem Standardblatt „Übersicht“). Nach einer kurzen Verzögerung sollten Ihre Ablaufverfolgungen im Application Insights-Portal angezeigt werden:

![Application Insights-Portal mit Protokollen einer Service Fabric-Anwendung][2]

## <a name="next-steps"></a>Nächste Schritte
* [Weitere Informationen zur Diagnose und Überwachung eines Service Fabric-Diensts](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
* [EventFlow-Dokumentation](https://github.com/Azure/diagnostics-eventflow)


<!--Image references-->
[1]: ./media/service-fabric-diagnostics-collect-logs-without-an-agent/eventflow-nugets.png
[2]: ./media/service-fabric-diagnostics-collect-logs-without-an-agent/ai-traces.png

