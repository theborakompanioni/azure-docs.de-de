---
title: "Aktivieren von Azure Application Insights Profiler für eine Cloud Services-Ressource | Microsoft-Dokumentation"
description: Erfahren Sie, wie Sie den Profiler in einer ASP.NET-Anwendung einrichten, die von einer Azure Cloud Services-Ressource gehostet wird.
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
ms.author: bwren
ms.translationtype: HT
ms.sourcegitcommit: 4c2be7c35f678430d0ad83a3374ef25f68fd2509
ms.openlocfilehash: c2cae6129386260f2bf35f75d44fa001f7541d40
ms.contentlocale: de-de
ms.lasthandoff: 09/20/2017

---

# <a name="enable-application-insights-profiler-on-an-azure-cloud-services-resource"></a>Aktivieren von Application Insights Profiler für eine Azure Cloud Services-Ressource

Diese exemplarische Vorgehensweise veranschaulicht, wie Sie Azure Application Insights Profiler für eine von einer Azure Cloud Services-Ressource gehostete ASP.NET-Anwendung aktivieren. Die Beispiele umfassen Unterstützung für Azure Virtual Machines, VM-Skalierungsgruppen und Azure Service Fabric. Alle Beispiele basieren auf Vorlagen, die das Azure Resource Manager-Bereitstellungsmodell unterstützen. Weitere Informationen zum Bereitstellungsmodell finden Sie unter [Azure Resource Manager-Bereitstellung im Vergleich zur klassischen Bereitstellung: Grundlegendes zu Bereitstellungsmodellen und zum Status von Ressourcen](/azure-resource-manager/resource-manager-deployment-model).

## <a name="overview"></a>Übersicht

Das folgende Diagramm veranschaulicht die Funktionsweise des Profilers für Azure Cloud Services-Ressourcen. Als Beispiel dient ein virtueller Azure-Computer.

![Übersicht:](./media/enable-profiler-compute/overview.png) Für die Azure Cloud Services-Ressourcen muss der Diagnose-Agent installiert werden, um Informationen zu sammeln, die verarbeitet und im Azure-Portal angezeigt werden können. Im Rest der exemplarischen Vorgehensweise erfahren Sie, wie Sie den Diagnose-Agent installieren und konfigurieren, um Application Insights Profiler zu aktivieren.

## <a name="prerequisites-for-the-walkthrough"></a>Voraussetzungen für die exemplarische Vorgehensweise

* Eine Resource Manager-Bereitstellungsvorlage, mit der die Profiler-Agents auf den virtuellen Computern ([WindowsVirtualMachine.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachine.json)) oder in den Skalierungsgruppen ([WindowsVirtualMachineScaleSet.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachineScaleSet.json)) installiert werden.

* Eine für die Profilerstellung geeignete Application Insights-Instanz. Anweisungen hierzu finden Sie unter [Aktivieren des Profilers](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-profiler#enable-the-profiler).

* Installiertes .NET Framework 4.6.1 oder höher auf der Azure Cloud Services-Zielressource.

## <a name="create-a-resource-group-in-your-azure-subscription"></a>Erstellen einer Ressourcengruppe in Ihrem Azure-Abonnement
Das folgende Beispiel veranschaulicht die Erstellung einer Ressourcengruppe unter Verwendung eines PowerShell-Skripts:

```
New-AzureRmResourceGroup -Name "Replace_With_Resource_Group_Name" -Location "Replace_With_Resource_Group_Location"
```

## <a name="create-an-application-insights-resource-in-the-resource-group"></a>Erstellen einer Application Insights-Ressource in der Ressourcengruppe
Geben Sie auf dem Blatt **Application Insights** die Informationen für Ihre Ressource ein, wie im folgenden Beispiel gezeigt: 

![Blatt „Application Insights“](./media/enable-profiler-compute/createai.png)

## <a name="apply-an-application-insights-instrumentation-key-in-the-azure-resource-manager-template"></a>Anwenden eines Application Insights-Instrumentierungsschlüssels in der Azure Resource Manager-Vorlage

1. Wenn Sie die Vorlage noch nicht heruntergeladen haben, laden Sie sie von [GitHub](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachine.json) herunter.

2. Suchen Sie den Application Insights-Schlüssel.
   
   ![Speicherort des Schlüssels](./media/enable-profiler-compute/copyaikey.png)

3. Ersetzen Sie den Vorlagenwert.
   
   ![Ersetzter Wert in der Vorlage](./media/enable-profiler-compute/copyaikeytotemplate.png)

## <a name="create-an-azure-vm-to-host-the-web-application"></a>Erstellen eines virtuellen Azure-Computers zum Hosten der Webanwendung
1. Erstellen Sie eine sichere Zeichenfolge zum Speichern des Kennworts.

   ```
   $password = ConvertTo-SecureString -String "Replace_With_Your_Password" -AsPlainText -Force
   ```

2. Stellen Sie die Azure Resource Manager-Vorlage bereit.

   Legen Sie das Verzeichnis in der PowerShell-Konsole auf den Ordner fest, der Ihre Resource Manager-Vorlage enthält. Führen Sie den folgenden Befehl aus, um die Vorlage bereitzustellen:

   ```
   New-AzureRmResourceGroupDeployment -ResourceGroupName "Replace_With_Resource_Group_Name" -TemplateFile .\WindowsVirtualMachine.json -adminUsername "Replace_With_your_user_name" -adminPassword $password -dnsNameForPublicIP "Replace_WIth_your_DNS_Name" -Verbose
   ```

Nach erfolgreicher Ausführung des Skripts enthält Ihre Ressourcengruppe einen virtuellen Computer namens **MyWindowsVM**.

## <a name="configure-web-deploy-on-the-vm"></a>Konfigurieren von Web Deploy auf dem virtuellen Computer
Stellen Sie sicher, dass Web Deploy auf Ihrem virtuellen Computer aktiviert ist, um Ihre Webanwendung über Visual Studio veröffentlichen zu können.

Informationen zum manuellen Installieren von Web Deploy über WebPI auf einem virtuellen Computer finden Sie unter [Installing and Configuring Web Deploy on IIS 8.0 or Later](https://docs.microsoft.com/en-us/iis/install/installing-publishing-technologies/installing-and-configuring-web-deploy-on-iis-80-or-later) (Installieren und Konfigurieren von Web Deploy unter IIS 8.0 oder einer höheren Version). Ein Beispiel für die Automatisierung der Web Deploy-Installation mithilfe einer Azure Resource Manager-Vorlage finden Sie unter [Create, configure, and deploy a web application to an Azure VM](https://azure.microsoft.com/en-us/resources/templates/201-web-app-vm-dsc/) (Erstellen, Konfigurieren und Bereitstellen einer Webanwendung für einen virtuellen Azure-Computer).

Wenn Sie eine ASP.NET MVC-Anwendung bereitstellen möchten, aktivieren Sie im Server-Manager unter **Rollen und Features hinzufügen** > **Webserver (IIS)** > **Webserver** > **Anwendungsentwicklung** ASP.NET 4.5 auf Ihrem Server.

![Hinzufügen von ASP.NET](./media/enable-profiler-compute/addaspnet45.png)

## <a name="install-the-azure-application-insights-sdk-for-your-project"></a>Installieren des Azure Application Insights SDKs für Ihr Projekt
1. Öffnen Sie Ihre ASP.NET-Webanwendung in Visual Studio.

2. Klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie **Hinzufügen** > **Verbundene Dienste** aus.

3. Wählen Sie **Application Insights**.

4. Befolgen Sie die Anweisungen auf der Seite. Wählen Sie die zuvor erstellte Application Insights-Ressource aus.

5. Wählen Sie die Schaltfläche **Registrieren** aus.


## <a name="publish-the-project-to-an-azure-vm"></a>Veröffentlichen des Projekts auf einem virtuellen Azure-Computer
Eine Anwendung kann auf unterschiedliche Weise für einen virtuellen Azure-Computer veröffentlicht werden. Eine Möglichkeit ist die Verwendung von Visual Studio 2017.

1. Klicken Sie mit der rechten Maustaste auf Ihr Projekt, und wählen Sie **Veröffentlichen** aus.

2. Wählen Sie **Microsoft Azure Virtual Machines** als Veröffentlichungsziel aus, und führen Sie die entsprechenden Schritte aus.

   ![Publish-FromVS](./media/enable-profiler-compute/publishtoVM.png)

3. Führen Sie einen Auslastungstest für Ihre Anwendung durch. Die Ergebnisse werden auf der Portalwebseite der Application Insights-Instanz angezeigt.


## <a name="enable-the-profiler"></a>Aktivieren des Profilers
1. Navigieren Sie zum Application Insights-Blatt **Leistung**, und wählen Sie **Konfigurieren** aus.
   
   ![Symbol „Konfigurieren“](./media/enable-profiler-compute/enableprofiler1.png)
 
2. Wählen Sie **Profiler aktivieren** aus.
   
   ![Symbol „Profiler aktivieren“](./media/enable-profiler-compute/enableprofiler2.png)

## <a name="add-a-performance-test-to-your-application"></a>Hinzufügen eines Leistungstests zu der Anwendung
Führen Sie die folgenden Schritte aus, um einige Beispieldaten zu erhalten, die in Application Insights Profiler angezeigt werden:

1. Navigieren Sie zu der zuvor erstellten Application Insights-Ressource. 

2. Fügen Sie auf dem Blatt **Verfügbarkeit** einen Leistungstest hinzu, der Webanforderungen an die URL Ihrer Anwendung sendet. 

   ![Hinzufügen eines Leistungstests](./media/enable-profiler-compute/AvailabilityTest.png)

## <a name="view-your-performance-data"></a>Anzeigen der Leistungsdaten

1. Geben Sie dem Profiler 10 bis 15 Minuten Zeit für die Sammlung und Analyse der Daten. 

2. Sehen Sie sich anschließend auf dem Blatt **Leistung** für Ihre Application Insights-Ressource an, wie Ihre Anwendung unter Last ausgeführt wird.

   ![Anzeigen der Leistung](./media/enable-profiler-compute/aiperformance.png)

3. Wählen Sie das Symbol unter **Beispiele** aus, um das Blatt **Ablaufverfolgungsansicht** zu öffnen.

   ![Öffnen des Blatts „Ablaufverfolgungsansicht“](./media/enable-profiler-compute/traceview.png)


## <a name="work-with-an-existing-template"></a>Verwenden einer vorhandenen Vorlage

1. Suchen Sie in Ihrer Bereitstellungsvorlage nach der Azure-Diagnose-Ressourcendeklaration.
   
   Wenn keine Deklaration vorhanden ist, können Sie eine erstellen, die der Deklaration im folgenden Beispiel ähnelt. Sie können die Vorlage auf der [Website des Azure-Ressourcen-Explorers](https://resources.azure.com) aktualisieren.

2. Ändern Sie den Herausgeber von `Microsoft.Azure.Diagnostics` in `AIP.Diagnostics.Test`.

3. Verwenden Sie `0.0` für `typeHandlerVersion`.

4. Stellen Sie sicher, dass `autoUpgradeMinorVersion` auf `true` festgelegt ist.

5. Fügen Sie im `WadCfg`-Einstellungsobjekt die neue `ApplicationInsightsProfiler`-Senkeninstanz hinzu, wie im folgenden Beispiel zu sehen:

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
Laden Sie die Vorlage [WindowsVirtualMachineScaleSet.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachineScaleSet.json) herunter, um zu sehen, wie der Profiler aktiviert wird. Wenden Sie die gleichen Änderungen in einer VM-Vorlage auf die Diagnoseerweiterungsressource der VM-Skalierungsgruppe an.

Stellen Sie sicher, dass jede Instanz in der Skalierungsgruppe Zugriff auf das Internet hat. Der Profiler-Agent kann dann die gesammelten Beispiele zur Anzeige und Analyse an Application Insights senden.

## <a name="enable-the-profiler-on-service-fabric-applications"></a>Aktivieren des Profilers für Service Fabric-Anwendungen
1. Stellen Sie den Service Fabric-Cluster mit der Azure-Diagnose-Erweiterung bereit, mit der der Profiler-Agent installiert wird.

2. Installieren Sie das Application Insights SDK im Projekt, und konfigurieren Sie den Application Insights-Schlüssel.

3. Fügen Sie den Anwendungscode zum Instrumentieren der Telemetrie hinzu.

### <a name="provision-the-service-fabric-cluster-to-have-the-azure-diagnostics-extension-that-installs-the-profiler-agent"></a>Bereitstellen des Service Fabric-Clusters mit der Azure-Diagnose-Erweiterung, mit der der Profiler-Agent installiert wird
Ein Service Fabric-Cluster kann sicher oder unsicher sein. Sie können einen Gatewaycluster als unsicheren Cluster festlegen, damit für den Zugriff kein Zertifikat erforderlich ist. Cluster, die Geschäftslogik und -daten hosten, müssen sicher sein. Der Profiler kann sowohl für sichere als auch für unsichere Service Fabric-Cluster aktiviert werden. In dieser exemplarischen Vorgehensweise wird anhand eines unsicheren Beispielclusters erläutert, welche Änderungen erforderlich sind, um den Profiler zu aktivieren. Sie können einen sicheren Cluster auf die gleiche Weise bereitstellen.

1. Laden Sie [ServiceFabricCluster.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/ServiceFabricCluster.json) herunter. Ersetzen Sie genau wie bei virtuellen Computern und VM-Skalierungsgruppen `Application_Insights_Key` durch Ihren Application Insights-Schlüssel:

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

2. Stellen Sie die Vorlage mithilfe eines PowerShell-Skripts bereit:

   ```
   Login-AzureRmAccount
   New-AzureRmResourceGroup -Name [Your_Resource_Group_Name] -Location [Your_Resource_Group_Location] -Verbose -Force
   New-AzureRmResourceGroupDeployment -Name [Choose_An_Arbitrary_Name] -ResourceGroupName [Your_Resource_Group_Name] -TemplateFile [Path_To_Your_Template]

   ```

### <a name="install-the-application-insights-sdk-in-the-project-and-configure-the-application-insights-key"></a>Installieren des Application Insights SDK im Projekt und Konfigurieren des Application Insights-Schlüssels
Installieren Sie das Application Insights SDK aus dem [NuGet-Paket](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/). Installieren Sie eine stabile Version ab Version 2.3. 

Informationen zum Konfigurieren von Application Insights in Ihren Projekten finden Sie unter [Using Service Fabric with Application Insights](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/blob/dev/appinsights/ApplicationInsights.md) (Verwenden von Service Fabric mit Application Insights).

### <a name="add-application-code-to-instrument-telemetry"></a>Hinzufügen von Anwendungscode zum Instrumentieren der Telemetrie
1. Schließen Sie zu instrumentierende Codepassagen in eine using-Anweisung ein. 

   Im folgenden Beispiel führt die `RunAsync`-Methode Aktionen aus, und die `telemetryClient`-Klasse erfasst die Telemetrie, nachdem die Methode gestartet wurde. Der Name des Ereignisses muss in der gesamten Anwendung eindeutig sein.

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

2. Stellen Sie Ihre Anwendung im Service Fabric-Cluster bereit. Warten Sie, bis die App zehn Minuten ausgeführt wurde. Sie können auch einen Auslastungstest für die App ausführen, um noch bessere Ergebnisse zu erzielen. Auf dem Blatt **Leistung** des Application Insights-Portals werden Beispiele für Profilablaufverfolgungen angezeigt.

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

- Weitere Informationen zum Profiler finden Sie unter [Application Insights Profiler](app-insights-profiler.md).

