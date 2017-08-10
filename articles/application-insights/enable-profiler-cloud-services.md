---
title: "Aktivieren von Azure Application Insights Profiler für eine Compute-Ressource | Microsoft-Dokumentation"
description: Hier erfahren Sie, wie Sie den Profiler einrichten.
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: sewhee
ms.translationtype: HT
ms.sourcegitcommit: 137671152878e6e1ee5ba398dd5267feefc435b7
ms.openlocfilehash: cde0b0bce2e332243e555a72088b8e9eeb680bdb
ms.contentlocale: de-de
ms.lasthandoff: 07/28/2017

---

# <a name="how-to-enable-application-insights-profiler-on-azure-compute-resources"></a>Aktivieren von Application Insights Profiler für Azure Compute-Ressourcen

Diese exemplarische Vorgehensweise veranschaulicht, wie Sie Application Insights Profiler für eine von Azure Compute-Ressourcen gehostete ASP.NET-Anwendung aktivieren. Die Beispiele umfassen Unterstützung für Virtual Machines, VM-Skalierungsgruppen und Service Fabric. Alle Beispiele basieren auf Vorlagen, die das Azure Resource Manager-Bereitstellungsmodell unterstützen. Weitere Informationen zum Bereitstellungsmodell finden Sie unter [Azure Resource Manager-Bereitstellung im Vergleich zur klassischen Bereitstellung: Grundlegendes zu Bereitstellungsmodellen und zum Status von Ressourcen](/azure-resource-manager/resource-manager-deployment-model).

## <a name="overview"></a>Übersicht

Das folgende Diagramm veranschaulicht die Funktionsweise des Profilers für Azure Compute-Ressourcen. Als Beispiel dient ein virtueller Azure-Computer.

![Übersicht](./media/enable-profiler-compute/overview.png) Für die Azure Compute-Ressourcen muss der Diagnose-Agent installiert werden, um Informationen zu sammeln, die verarbeitet und im Azure-Portal angezeigt werden können. Im Rest der exemplarischen Vorgehensweise erfahren Sie, wie Sie den Diagnose-Agent installieren und konfigurieren, um Application Insights Profiler zu aktivieren.

## <a name="prerequisites-for-the-walkthrough"></a>Voraussetzungen für die exemplarische Vorgehensweise

* Laden Sie die Resource Manager-Bereitstellungsvorlagen herunter, die die Profiler-Agents für die virtuellen Computer oder VM-Skalierungsgruppen installieren.

    [WindowsVirtualMachine.json](https://github.com/CFreemanwa/samples/blob/master/WindowsVirtualMachine.json) | [WindowsVirtualMachineScaleSet.json](https://github.com/CFreemanwa/samples/blob/master/WindowsVirtualMachineScaleSet.json)
* Eine für die Profilerstellung geeignete Application Insights-Instanz. Eine entsprechende Anleitung finden Sie unter „https://docs.microsoft.com/de-de/azure/application-insights/app-insights-profiler#enable-the-profiler“.
* Mindestens .NET Framework 4.6.1 (installiert in der Azure Compute-Zielressource).

## <a name="create-a-resource-group-in-your-azure-subscription"></a>Erstellen einer Ressourcengruppe in Ihrem Azure-Abonnement
Das folgende Beispiel veranschaulicht die Erstellung einer Ressourcengruppe unter Verwendung eines PowerShell-Skripts:

```
New-AzureRmResourceGroup -Name "Replace_With_Resource_Group_Name" -Location "Replace_With_Resource_Group_Location"
```

## <a name="create-an-application-insights-resource-in-the-resource-group"></a>Erstellen einer Application Insights-Ressource in der Ressourcengruppe

![Erstellen von Application Insights](./media/enable-profiler-compute/createai.png)

## <a name="apply-application-insights-instrumentation-key-in-the-azure-resource-manager-template"></a>Anwenden des Application Insights-Instrumentierungsschlüssels in der Azure Resource Manager-Vorlage
Falls Sie die Vorlage noch nicht heruntergeladen haben, laden Sie sie über den folgenden Link herunter: [WindowsVirtualMachine.json](https://github.com/CFreemanwa/samples/blob/master/WindowsVirtualMachine.json)

![Suchen nach dem AI-Schlüssel](./media/enable-profiler-compute/copyaikey.png)

![Ersetzen des Vorlagenwerts](./media/enable-profiler-compute/copyaikeytotemplate.png)

## <a name="create-azure-vm-to-host-the-web-application"></a>Erstellen eines virtuellen Azure-Computers zum Hosten der Webanwendung
* Erstellen Sie eine sichere Zeichenfolge zum Speichern des Kennworts.
```
$password = ConvertTo-SecureString -String "Replace_With_Your_Password" -AsPlainText -Force
```

* Stellen Sie die Azure Resource Manager-Vorlage bereit.

Legen Sie das Verzeichnis in der PowerShell-Konsole auf den Ordner fest, der Ihre Resource Manager-Vorlage enthält. Führen Sie den folgenden Befehl aus, um die Vorlage bereitzustellen:

```
New-AzureRmResourceGroupDeployment -ResourceGroupName "Replace_With_Resource_Group_Name" -TemplateFile .\WindowsVirtualMachine.json -adminUsername "Replace_With_your_user_name" -adminPassword $password -dnsNameForPublicIP "Replace_WIth_your_DNS_Name" -Verbose
```

Nach erfolgreicher Ausführung des Skripts enthält Ihre Ressourcengruppe einen virtuellen Computer namens *MyWindowsVM*.

## <a name="configure-web-deploy-on-the-vm"></a>Konfigurieren von Web Deploy auf dem virtuellen Computer
Stellen Sie sicher, dass **Web Deploy** auf Ihrem virtuellen Computer aktiviert ist, um Ihre Webanwendung über Visual Studio veröffentlichen zu können.

Web Deploy kann manuell über WebPI auf einem virtuellen Computer installiert werden: [Installing and Configuring Web Deploy on IIS 8.0 or Later](https://docs.microsoft.com/en-us/iis/install/installing-publishing-technologies/installing-and-configuring-web-deploy-on-iis-80-or-later) (Installieren und Konfigurieren von Web Deploy unter IIS 8.0 oder einer höheren Version)

Unter dem folgenden Link finden Sie ein Beispiel für die Automatisierung der Web Deploy-Installation mithilfe einer Azure Resource Manager-Vorlage: [Create, configure and deploy Web Application to an Azure VM](https://azure.microsoft.com/en-us/resources/templates/201-web-app-vm-dsc/) (Erstellen, Konfigurieren und Bereitstellen einer Webanwendung für einen virtuellen Azure-Computer)

Wenn Sie eine ASP.NET MVC-Anwendung bereitstellen möchten, aktivieren Sie im Server-Manager unter **Rollen und Features hinzufügen > Webserver (IIS) > Webserver > Anwendungsentwicklung** ASP.NET 4.5 auf Ihrem Server.
![Hinzufügen von ASP.NET](./media/enable-profiler-compute/addaspnet45.png)

## <a name="install-azure-application-insights-sdk-to-your-project"></a>Installieren des Azure Application Insights SDKs für Ihr Projekt
* Öffnen Sie Ihre ASP.NET-Webanwendung in Visual Studio.
* Klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie **hinzufügen > Verbundene Dienste** aus.
* Wählen Sie „Application Insights“ aus.
* Befolgen Sie die Anweisungen auf der Seite. Wählen Sie die zuvor erstellte Application Insights-Ressource aus.
* Klicken Sie auf die Schaltfläche **Registrieren**.


## <a name="publish-the-project-to-azure-vm"></a>Veröffentlichen des Projekts für den virtuellen Azure-Computer
Eine Anwendung kann auf unterschiedliche Weise für einen virtuellen Azure-Computer veröffentlicht werden. Eine Möglichkeit ist die Verwendung von Visual Studio 2017.
Klicken Sie zum Abschließen des Veröffentlichungsprozesses mit der rechten Maustaste auf das Projekt, und wählen Sie „Veröffentlichen...“ aus. Wählen Sie „Virtueller Azure-Computer“ als Veröffentlichungsziel aus, und führen Sie die entsprechenden Schritte durch.

![Publish-FromVS](./media/enable-profiler-compute/publishtoVM.png)

Führen Sie einen Auslastungstest für Ihre Anwendung durch. Die Ergebnisse werden auf der Portalwebseite der Application Insights-Instanz angezeigt.


## <a name="enable-the-profiler-in-application-insights"></a>Aktivieren des Profilers in Application Insights
Navigieren Sie zum Application Insights-Blatt „Leistung“. Klicken Sie auf das Symbol „Konfigurieren“, und aktivieren Sie den Profiler.

![Aktivieren des Profilers: Schritt 1](./media/enable-profiler-compute/enableprofiler1.png)

![Aktivieren des Profilers: Schritt 2](./media/enable-profiler-compute/enableprofiler2.png)

## <a name="add-an-availability-test-to-your-application"></a>Hinzufügen eines Verfügbarkeitstests zu Ihrer Anwendung
Navigieren Sie zu der zuvor erstellten Application Insights-Ressource. Fügen Sie auf dem Blatt „Verfügbarkeit“ einen Leistungstest hinzu, der Webanforderungen an die URL Ihrer Anwendung sendet. Dadurch erhalten Sie einige Beispieldaten, die in Application Insights Profiler angezeigt werden können.

![Hinzufügen eines Leistungstests][./media/enable-profiler-compute/add-test.png]

## <a name="view-your-performance-data"></a>Anzeigen der Leistungsdaten

Geben Sie dem Profiler zehn bis 15 Minuten Zeit für die Datensammlung und -analyse. Sehen Sie sich anschließend auf dem Blatt „Leistung“ für Ihre AI-Ressource an, wie sich Ihre Anwendung unter Last schlägt.

![Anzeigen der Leistung][./media/enable-profiler-compute/view-aiperformance.png]

Wenn Sie auf das Symbol unter „Beispiele“ klicken, öffnet sich das Blatt „Ablaufverfolgungsansicht“.

![Ablaufverfolgungsansicht][./media/enable-profiler-compute/traceview.png]


## <a name="work-with-an-existing-template"></a>Verwenden einer vorhandenen Vorlage

1. Suchen Sie in Ihrer Bereitstellungsvorlage nach der WAD-Ressourcendeklaration (Windows Azure Diagnostics).
  * Sollte noch keine vorhanden sein, erstellen Sie eine. (Eine entsprechende Anleitung finden sie im vollständigen Beispiel.)
  * Sie können die Vorlage von der Azure-Ressourcenwebsite (https://resources.azure.com) aktualisieren.
2. Ändern Sie den Herausgeber von „Microsoft.Azure.Diagnostics“ in „AIP.Diagnostics.Test“.
3. Legen Sie „typeHandlerVersion“ auf „0.0“ fest.
4. Vergewissern Sie sich, dass „autoUpgradeMinorVersion“ auf „true“ festgelegt ist.
5. Fügen Sie im WadCfg-Einstellungsobjekt die neue ApplicationInsightsProfiler-Senkeninstanz hinzu, wie im folgenden Beispiel zu sehen.

```
"resources": [
        {
          "type": "extensions",
          "name": "Microsoft.Insights.VMDiagnosticsSettings",
          "apiVersion": "2016-03-30",
          "properties": {
            "publisher": "AIP.Diagnostics.Test",
            "type": "IaaSDiagnostics",
            "typeHandlerVersion": "0.0",
            "autoUpgradeMinorVersion": true,
            "settings": {
              "WadCfg": {
                "SinksConfig": {
                  "Sink": [
                    {
                      "name": "Give a descriptive short name. E.g.: MyApplicationInsightsProfilerSink",
                      "ApplicationInsightsProfiler": "Enter the Application Insights instance instrumentation key guid here"
                    }
                  ]
                },
                "DiagnosticMonitorConfiguration": {
                    ...
                }
                ...
              }
              ...
            }
            ...
          }
          ...
]
```

## <a name="enable-the-profiler-on-virtual-machine-scale-sets"></a>Aktivieren des Profilers für VM-Skalierungsgruppen
Laden Sie die Vorlage [WindowsVirtualMachineScaleSet.json](https://github.com/CFreemanwa/samples/blob/master/WindowsVirtualMachineScaleSet.json) herunter, um zu sehen, wie der Profiler aktiviert wird. Wenden Sie die gleichen Änderungen in einer VM-Vorlage auf die Diagnoseerweiterungsressource der VM-Skalierungsgruppe an.
Vergewissern Sie sich, dass jede Instanz in der Skalierungsgruppe über Internetzugriff verfügt, damit der Profiler-Agent die gesammelten Stichproben zur Analyse und Anzeige an Application Insights senden kann.

## <a name="enable-the-profiler-on-service-fabric-applications"></a>Aktivieren des Profilers für Service Fabric-Anwendungen
Derzeit müssen folgende Schritte ausgeführt werden, um den Profiler für Service Fabric-Anwendungen zu aktivieren:
1. Bereitstellen des Service Fabric-Clusters mit der WAD-Erweiterung, die den Profiler-Agent installiert
2. Installieren des Application Insights SDKs im Projekt und Konfigurieren des AI-Schlüssels
3. Hinzufügen von Anwendungscode zum Instrumentieren der Telemetrie

## <a name="provision-the-service-fabric-cluster-have-the-wad-extension-that-installs-the-profiler-agent"></a>Bereitstellen des Service Fabric-Clusters mit der WAD-Erweiterung, die den Profiler-Agent installiert
Ein Service Fabric-Cluster kann sicher oder unsicher sein. Sie können einen Gatewaycluster als unsicheren Cluster festlegen, damit für den Zugriff kein Zertifikat erforderlich ist. Cluster, die Geschäftslogik und -daten hosten, müssen sicher sein. Der Profiler kann sowohl für sicherer als auch für unsichere Service Fabric-Cluster aktiviert werden. In dieser exemplarischen Vorgehensweise wird anhand eines unsicheren Beispielclusters erläutert, welche Änderungen erforderlich sind, um den Profiler zu aktivieren. Sie können einen sicheren Cluster auf die gleiche Weise bereitstellen.

Laden Sie [ServiceFabricCluster.json](https://github.com/CFreemanwa/samples/blob/master/ServiceFabricCluster.json) herunter. Ersetzen Sie genau wie bei virtuellen Computern und VM-Skalierungsgruppen den Application Insights-Schlüssel durch Ihren AI-Schlüssel:

```
"publisher": "AIP.Diagnostics.Test",
                 "settings": {
                   "WadCfg": {
                     "SinksConfig": {
                       "Sink": [
                         {
                           "name": "MyApplicationInsightsProfilerSinkVMSS",
                           "ApplicationInsightsProfiler": "[Application_Insights_Key]"
                         }
                       ]
                     },
```

Stellen Sie die Vorlage mithilfe eines PowerShell-Skripts bereit:
```
Login-AzureRmAccount
New-AzureRmResourceGroup -Name [Your_Resource_Group_Name] -Location [Your_Resource_Group_Location] -Verbose -Force
New-AzureRmResourceGroupDeployment -Name [Choose_An_Arbitrary_Name] -ResourceGroupName [Your_Resource_Group_Name] -TemplateFile [Path_To_Your_Template]

```

## <a name="install-application-insights-sdk-in-the-project-and-configure-ai-key"></a>Installieren des Application Insights SDKs im Projekt und Konfigurieren des AI-Schlüssels
Installieren Sie das Application Insights SDK aus dem NuGet-Paket. Installieren Sie eine stabile Version ab Version 2.3. [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) Informationen zum Konfigurieren von Application Insights in Ihren Projekten finden Sie unter [Using Service Fabric with Application Insights](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/blob/dev/appinsights/ApplicationInsights.md) (Verwenden von Service Fabric mit Application Insights).

## <a name="add-application-code-to-instrument-telemetry"></a>Hinzufügen von Anwendungscode zum Instrumentieren der Telemetrie
Schließen Sie zu instrumentierende Codepassagen in eine using-Anweisung ein. Im folgenden Beispiel führt die RunAsync-Methode Aktionen aus, und die telemetryClient-Klasse erfasst die Telemetrie, sobald die Methode gestartet wird. Der Name des Ereignisses muss in der gesamten Anwendung eindeutig sein.

```
protected override async Task RunAsync(CancellationToken cancellationToken)
       {
           // TODO: Replace the following sample code with your own logic
           //       or remove this RunAsync override if it's not needed in your service.

           while (true)
           {
               using( var operation = telemetryClient.StartOperation<RequestTelemetry>("[Insert_Event_Unique_Name]"))
               {
                   cancellationToken.ThrowIfCancellationRequested();

                   ++this.iterations;

                   ServiceEventSource.Current.ServiceMessage(this.Context, "Working-{0}", this.iterations);

                   await Task.Delay(TimeSpan.FromSeconds(1), cancellationToken);
               }

           }
       }
```

Stellen Sie Ihre Anwendung im Service Fabric-Cluster bereit. Warten Sie, bis die App zehn Minuten ausgeführt wurde. Sie können auch einen Auslastungstest für die App ausführen, um noch bessere Ergebnisse zu erzielen. Auf dem Blatt „Leistung“ des Application Insights-Portals werden Beispiele für Profilablaufverfolgungen angezeigt.

<!---
Commenting out these sections for now
## Enable the Profiler on Cloud Services applications
[TODO]
## Enable the Profiler on classic Azure Virtual Machines
[TODO]
## Enable the Profiler on on-premise servers
[TODO]
--->

## <a name="next-steps"></a>Nächste Schritte

- Hilfe beim Behandeln von Problemen mit dem Profiler finden Sie unter [Problembehandlung](app-insights-profiler.md#troubleshooting).

- Weitere Informationen zum Profiler finden Sie unter [Profilerstellung für Live-Azure-Web-Apps mit Application Insights](app-insights-profiler.md).

