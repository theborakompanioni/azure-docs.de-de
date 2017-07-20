---
title: Azure Service Fabric-Ereignisaggregation mit der Windows Azure-Diagnose | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie Ereignisse unter Verwendung von WAD zur Überwachung und Diagnose von Azure Service Fabric-Clustern aggregieren und sammeln."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/30/2017
ms.author: dekapur
ms.translationtype: Human Translation
ms.sourcegitcommit: b1d56fcfb472e5eae9d2f01a820f72f8eab9ef08
ms.openlocfilehash: 3337e3ad36792c1dcd0eaf183a2b695503b8f02c
ms.contentlocale: de-de
ms.lasthandoff: 07/06/2017


---

# <a name="event-aggregation-and-collection-using-windows-azure-diagnostics"></a>Ereignisaggregation und -sammlung mit der Windows Azure-Diagnose
> [!div class="op_single_selector"]
> * [Windows](service-fabric-diagnostics-event-aggregation-wad.md)
> * [Linux](service-fabric-diagnostics-event-aggregation-lad.md)
>
>

Bei Verwendung eines Azure Service Fabric-Clusters empfiehlt es sich, die Protokolle aller Knoten an einem zentralen Ort zu sammeln. Das Sammeln der Protokolle an einem zentralen Ort hilft Ihnen bei Analyse und Behandlung von Problemen, die ggf. in Ihrem Cluster oder in den Anwendungen und Diensten des Clusters auftreten.

Eine Möglichkeit zum Hochladen und Sammeln von Protokollen ist die Verwendung der Windows Azure-Diagnose (WAD)-Erweiterung, mit der Protokolle in Azure Storage hochgeladen und an Azure Application Insights oder Event Hubs gesendet werden können. Sie können zudem einen externen Prozess verwenden, um die Ereignisse aus dem Speicher zu lesen und in einem Analyseplattformprodukt wie [OMS Log Analytics](../log-analytics/log-analytics-service-fabric.md) oder in einer anderen Protokollanalyselösung zu verwenden.

## <a name="prerequisites"></a>Voraussetzungen
Diese Tools werden verwendet, um einige Vorgänge in diesem Dokument durchzuführen:

* [Azure-Diagnose](../cloud-services/cloud-services-dotnet-diagnostics.md) (bezieht sich auf Azure Cloud Services, enthält jedoch hilfreiche Informationen und Beispiele)
* [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md)
* [Azure PowerShell](/powershell/azure/overview)
* [Azure Resource Manager-Client](https://github.com/projectkudu/ARMClient)
* [Azure Resource Manager-Vorlage](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="log-and-event-sources"></a>Protokoll- und Ereignisquellen

### <a name="service-fabric-infrastructure-events"></a>Service Fabric-Infrastrukturereignisse
Wie in [diesem Artikel](service-fabric-diagnostics-event-generation-infra.md) erläutert, richtet Service Fabric einige Standardprotokollierungskanäle ein. Die folgenden dieser Kanäle lassen sich mit WAD einfach konfigurieren, um Überwachungs- und Diagnosedaten an eine Speichertabelle oder einen anderen Speicherort zu senden:
  * Betriebsereignisse: Vorgänge einer höheren Ebene, die von der Service Fabric-Plattform durchgeführt werden. Beispiele hierfür wären die Erstellung von Anwendungen und Diensten, Knotenzustandsänderungen und Upgradeinformationen. Diese werden als ETW-Protokolle (Event Tracing for Windows, Ereignisablaufverfolgung für Windows-Ereignisse) ausgegeben.
  * [Ereignisse des Reliable Actors-Programmiermodells](service-fabric-reliable-actors-diagnostics.md)
  * [Ereignisse des Reliable Services-Programmiermodells](service-fabric-reliable-services-diagnostics.md)

### <a name="application-events"></a>Anwendungsereignisse
 Ereignisse, die vom Code Ihrer Anwendungen und Dienste ausgegeben und mithilfe der EventSource-Hilfsklasse der Visual Studio-Vorlagen geschrieben werden. Weitere Informationen zum Schreiben von EventSource-Protokollen aus Ihrer Anwendung finden Sie unter [Überwachen und Diagnostizieren von Diensten in einer Entwicklungsumgebung auf einem lokalen Computer](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).

## <a name="deploy-the-diagnostics-extension"></a>Bereitstellen der Diagnoseerweiterung
Zum Sammeln von Protokollen muss zunächst die Diagnoseerweiterung auf allen VMs des Service Fabric-Clusters bereitgestellt werden. Die Diagnoseerweiterung sammelt Protokolle auf allen VMs und lädt sie in das angegebene Speicherkonto hoch. Die auszuführenden Schritte variieren ein wenig, je nachdem, ob Sie das Azure-Portal oder Azure Resource Manager verwenden. Diese Schritte hängen auch davon ab, ob die Bereitstellung während der Clustererstellung oder für einen bereits vorhandenen Cluster erfolgt. Wir sehen uns nun die Schritte für die einzelnen Szenarien an.

### <a name="deploy-the-diagnostics-extension-as-part-of-cluster-creation-through-azure-portal"></a>Bereitstellen der Diagnoseerweiterung im Rahmen der Clustererstellung über das Azure-Portal
Um die Diagnoseerweiterung im Rahmen der Clustererstellung für die im Cluster enthaltenen virtuellen Computer bereitzustellen, verwenden Sie den in der folgenden Abbildung gezeigten Bereich mit den Diagnoseeinstellungen. Stellen Sie sicher, dass für die Diagnose **Ein** (Standardeinstellung) festgelegt ist. Nach der Erstellung des Clusters können Sie diese Einstellung nicht im Portal ändern.

![Azure-Diagnose-Einstellung im Portal für die Clustererstellung](media/service-fabric-diagnostics-event-aggregation-wad/azure-enable-diagnostics.png)

Bei der Clustererstellung über das Portal sollten Sie unbedingt die Vorlage herunterladen, **bevor Sie auf „OK“ klicken**, um den Cluster zu erstellen. Ausführliche Informationen finden Sie unter [Erstellen eines Service Fabric-Clusters in Azure mithilfe von Azure Resource Manager](service-fabric-cluster-creation-via-arm.md). Sie benötigen die Vorlage für spätere Änderungen, da Sie einige Änderungen nicht über das Portal vornehmen können.

### <a name="deploy-the-diagnostics-extension-as-part-of-cluster-creation-by-using-azure-resource-manager"></a>Bereitstellen der Diagnoseerweiterung im Rahmen der Clustererstellung mithilfe des Azure-Ressourcen-Managers
Wenn Sie einen Cluster mithilfe von Resource Manager erstellen möchten, müssen Sie der Resource Manager-Vorlage für einen vollständigen Cluster vor der Clustererstellung den JSON-Code für die Diagnosekonfiguration hinzufügen. Die Vorlagenbeispiele für den Ressourcen-Manager enthalten eine Beispielvorlage mit hinzugefügter Diagnosekonfiguration für einen Cluster mit fünf VMs. Diese finden Sie im Azure-Beispielkatalog unter [Ressourcen-Manager-Beispielvorlage für einen Cluster mit fünf Knoten und Diagnose](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype-wad).

Öffnen Sie die Datei „azuredeploy.json“ und suchen nach **IaaSDiagnostics**, um die Diagnoseeinstellung in der Resource Manager-Vorlage anzuzeigen. Klicken Sie zum Erstellen eines Clusters mit dieser Vorlage einfach auf die Schaltfläche **In Azure bereitstellen** (unter dem oben angegebenen Link).

Alternativ können Sie das Resource Manager-Beispiel herunterladen, anpassen und den Befehl `New-AzureRmResourceGroupDeployment` in einem Azure PowerShell-Fenster ausführen, um einen Cluster mit der geänderten Vorlage zu erstellen. Im folgenden Code finden Sie die Parameter, die Sie an den Befehl übergeben. Ausführliche Informationen zum Bereitstellen einer Ressourcengruppe mit PowerShell finden Sie im Artikel [Bereitstellen einer Ressourcengruppe mit einer Azure Resource Manager-Vorlage](../azure-resource-manager/resource-group-template-deploy.md).

### <a name="deploy-the-diagnostics-extension-to-an-existing-cluster"></a>Bereitstellen der Diagnoseerweiterung für einen vorhandenen Cluster
Wenn Sie bereits über einen Cluster ohne Diagnosebereitstellung verfügen oder eine vorhandene Konfiguration ändern möchten, können Sie die Diagnose hinzufügen oder aktualisieren. Ändern Sie die zum Erstellen des vorhandenen Clusters verwendete Resource Manager-Vorlage, oder laden Sie die Vorlage, wie oben beschrieben, aus dem Portal herunter. Ändern Sie die Datei „template.json“ , indem Sie die folgenden Aufgaben ausführen.

Fügen Sie der Vorlage eine neue Speicherressource hinzu, indem Sie sie dem Ressourcenabschnitt hinzufügen.

```json
{
  "apiVersion": "2015-05-01-preview",
  "type": "Microsoft.Storage/storageAccounts",
  "name": "[parameters('applicationDiagnosticsStorageAccountName')]",
  "location": "[parameters('computeLocation')]",
  "properties": {
    "accountType": "[parameters('applicationDiagnosticsStorageAccountType')]"
  },
  "tags": {
    "resourceType": "Service Fabric",
    "clusterName": "[parameters('clusterName')]"
  }
},
```

 Fügen Sie als Nächstes dem Parameterabschnitt etwas hinzu, direkt nach den Speicherkontodefinitionen, zwischen `supportLogStorageAccountName` und `vmNodeType0Name`. Ersetzen Sie den Platzhaltertext *storage account name goes here* durch den Namen des Speicherkontos.

```json
    "applicationDiagnosticsStorageAccountType": {
      "type": "string",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS"
      ],
      "defaultValue": "Standard_LRS",
      "metadata": {
        "description": "Replication option for the application diagnostics storage account"
      }
    },
    "applicationDiagnosticsStorageAccountName": {
      "type": "string",
      "defaultValue": "storage account name goes here",
      "metadata": {
        "description": "Name for the storage account that contains application diagnostics data from the cluster"
      }
    },
```
Aktualisieren Sie dann den Abschnitt `VirtualMachineProfile` der template.json-Datei durch Hinzufügen des folgenden Codes innerhalb des Arrays „extensions“. Achten Sie darauf, ein Komma am Anfang oder Ende hinzuzufügen – je nach Einfügeposition.

```json
{
    "name": "[concat(parameters('vmNodeType0Name'),'_Microsoft.Insights.VMDiagnosticsSettings')]",
    "properties": {
        "type": "IaaSDiagnostics",
        "autoUpgradeMinorVersion": true,
        "protectedSettings": {
        "storageAccountName": "[parameters('applicationDiagnosticsStorageAccountName')]",
        "storageAccountKey": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('applicationDiagnosticsStorageAccountName')),'2015-05-01-preview').key1]",
        "storageAccountEndPoint": "https://core.windows.net/"
        },
        "publisher": "Microsoft.Azure.Diagnostics",
        "settings": {
        "WadCfg": {
            "DiagnosticMonitorConfiguration": {
            "overallQuotaInMB": "50000",
            "EtwProviders": {
                "EtwEventSourceProviderConfiguration": [
                {
                    "provider": "Microsoft-ServiceFabric-Actors",
                    "scheduledTransferKeywordFilter": "1",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricReliableActorEventTable"
                    }
                },
                {
                    "provider": "Microsoft-ServiceFabric-Services",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricReliableServiceEventTable"
                    }
                }
                ],
                "EtwManifestProviderConfiguration": [
                {
                    "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
                    "scheduledTransferLogLevelFilter": "Information",
                    "scheduledTransferKeywordFilter": "4611686018427387904",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricSystemEventTable"
                    }
                }
                ]
            }
            }
        },
        "StorageAccount": "[parameters('applicationDiagnosticsStorageAccountName')]"
        },
        "typeHandlerVersion": "1.5"
    }
}
```

Nachdem Sie die Datei „template.json“ wie beschrieben geändert haben, veröffentlichen Sie die Resource Manager-Vorlage erneut. Wenn die Vorlage exportiert wurde, wird sie durch Ausführen der Datei „deploy.ps1“ neu veröffentlicht. Stellen Sie nach der Bereitstellung sicher, dass **ProvisioningState** den Status **Erfolgreich** aufweist.

## <a name="collect-health-and-load-events"></a>Erfassen von Integritäts- und Auslastungsereignissen

Ab Version 5.4 von Service Fabric können Integritäts- und Auslastungsmetrikereignisse gesammelt werden. Diese Ereignisse spiegeln Ereignisse wider, die vom System oder von Ihrem Code mithilfe der APIs zum Melden der Integrität oder Auslastung (beispielsweise [ReportPartitionHealth](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportpartitionhealth.aspx) oder [ReportLoad](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportload.aspx)) generiert wurden. Dies ermöglicht eine aggregierte Betrachtung der Systemintegrität im Laufe der Zeit sowie die Generierung von Warnungen auf der Grundlage von Integritäts- oder Auslastungsereignissen. Diese Ereignisse können Sie in der Diagnoseereignisansicht von Visual Studio anzeigen, indem Sie „Microsoft-ServiceFabric:4:0x4000000000000008“ zur Liste mit den ETW-Anbietern hinzufügen.

Ändern Sie zum Sammeln der Ereignisse die Resource Manager-Vorlage, sodass sie Folgendes enthält:

```json
  "EtwManifestProviderConfiguration": [
    {
      "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
      "scheduledTransferLogLevelFilter": "Information",
      "scheduledTransferKeywordFilter": "4611686018427387912",
      "scheduledTransferPeriod": "PT5M",
      "DefaultEvents": {
        "eventDestination": "ServiceFabricSystemEventTable"
      }
    }
```

## <a name="collect-from-new-eventsource-channels"></a>Erfassen aus neuen EventSource-Kanälen

Wenn Sie die Diagnose aktualisieren möchten, um Protokolle aus neuen EventSource-Kanälen zu sammeln, die eine neu bereitzustellende Anwendung darstellen, führen Sie die Schritte aus, die zuvor für die Einrichtung der Diagnose für einen vorhandenen Cluster beschrieben wurden.

Aktualisieren Sie den Abschnitt `EtwEventSourceProviderConfiguration` in der template.json-Datei, und fügen Sie Einträge für die neuen EventSource-Kanäle hinzu, bevor Sie das Konfigurationsupdate mithilfe des PowerShell-Befehls `New-AzureRmResourceGroupDeployment` anwenden. Der Name der Ereignisquelle wird als Teil des Codes in der von Visual Studio generierten Datei „ServiceEventSource.cs“ definiert.

Wenn Ihre Ereignisquelle beispielsweise My-Eventsource heißt, fügen Sie folgenden Code hinzu, um Ereignisse aus My-Eventsource in eine Tabelle namens MyDestinationTableName zu platzieren.

```json
        {
            "provider": "My-Eventsource",
            "scheduledTransferPeriod": "PT5M",
            "DefaultEvents": {
            "eventDestination": "MyDestinationTableName"
            }
        }
```

Um Leistungsindikatoren oder Ereignisprotokolle zu sammeln, ändern Sie die Resource Manager-Vorlage anhand der Beispiele unter [Erstellen eines virtuellen Windows-Computers mit Überwachung und Diagnose mithilfe von Azure Resource Manager-Vorlagen](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Veröffentlichen Sie die Resource Manager-Vorlage dann erneut.

## <a name="collect-performance-counters"></a>Erfassen von Leistungsindikatoren

Fügen Sie zu „WadCfg“ > „DiagnosticMonitorConfiguration“ in der Resource Manager-Vorlage für Ihren Cluster die Leistungsindikatoren hinzu, um Leistungsmetriken Ihres Clusters zu erfassen. Informationen zu den Leistungsindikatoren, deren Erfassung wir empfehlen, finden Sie unter [Service Fabric Performance Counters](service-fabric-diagnostics-event-generation-perf.md) (Service Fabric-Leistungsindikatoren).

Hier wird beispielsweise ein Leistungsindikator festgelegt, der alle 15 Sekunden als Stichprobe abgerufen wird (dies kann geändert werden, und hat das Format „PT\<Zeit>\<Einheit>“; PT3M bedeutet beispielsweise, dass in Intervallen von drei Minuten Stichproben abgerufen werden) und anschließend einmal pro Minute an die entsprechende Speichertabelle übermittelt wird.

    ```json
    "PerformanceCounters": {
        "scheduledTransferPeriod": "PT1M",
        "PerformanceCounterConfiguration": [
            {
                "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
                "sampleRate": "PT15S",
                "unit": "Percent",
                "annotation": [
                ],
                "sinks": ""
            }
        ]
    }
    ```
Wenn Sie eine Application Insights-Senke verwenden (siehe dazu den folgenden Abschnitt) und möchten, dass diese Metriken in Application Insights angezeigt werden, dann fügen Sie den Namen der Senke im Abschnitt „sinks“ (s.o.) ein. Darüber hinaus sollten Sie ggf. eine separate Tabelle erstellen, an die Ihre Leistungsindikatoren gesendet werden, damit sie nicht mit den Daten aus den anderen aktivierten Protokollierungskanälen vermischt werden.


## <a name="send-logs-to-application-insights"></a>Senden von Protokollen an Application Insights

Das Senden von Überwachungs- und Diagnosedaten an Application Insights (AI) kann als Teil der WAD-Konfiguration erfolgen. Wenn Sie AI für die Ereignisanalyse und -visualisierung verwenden, finden Sie unter [Event Analysis and Visualization with Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md) (Ereignisanalyse und -visualisierung mit Application Insights) weitere Informationen zum Einrichten einer AI-Senke als Teil von „WadCfg“.

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie die Azure-Diagnose richtig konfiguriert haben, enthalten Ihre Speichertabellen Daten aus den ETW- und EventSource-Protokollen. Wenn Sie OMS, Kibana oder eine andere Plattform zur Datenanalyse und -visualisierung verwenden möchten, die in der Resource Manager-Vorlage nicht direkt konfiguriert ist, richten Sie die gewünschte Plattform so ein, dass sie die Daten aus diesen Speichertabellen liest. Bei OMS ist dies recht einfach. Informationen dazu finden Sie unter [Event analysis and visualization with OMS](service-fabric-diagnostics-event-analysis-oms.md) (Ereignis- und Protokollanalyse mithilfe von OMS). Bei Application Insights verhält sich dies ein wenig anders, da es bei der Konfiguration der Diagnoseerweiterung konfiguriert werden kann. Wenn Sie also AI verwenden möchten, lesen Sie die Informationen im [entsprechenden Artikel](service-fabric-diagnostics-event-analysis-appinsights.md).

>[!NOTE]
>Es gibt derzeit keine Möglichkeit, die an die Tabelle gesendeten Ereignisse zu filtern oder zu optimieren. Wenn Sie keinen Prozess zum Entfernen von Ereignissen aus der Tabelle implementieren, wächst die Tabelle weiter an. Im [Watchdog-Beispiel](https://github.com/Azure-Samples/service-fabric-watchdog-service) finden Sie derzeit ein Beispiel für einen ausgeführten Datenbereinigungsdienst. Die Erstellung eines solchen Diensts wird empfohlen, sofern Sie Protokolle nicht aus einem triftigen Grund länger als 30 oder 90 Tage speichern müssen.

* [Erfahren Sie, wie Sie Leistungsindikatoren oder Protokolle mithilfe der Diagnoseerweiterung sammeln können.](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Event Analysis and Visualization with Application Insights (Ereignisanalyse und -visualisierung mit Application Insights)](service-fabric-diagnostics-event-analysis-appinsights.md)
* [Event Analysis and Visualization with OMS (Ereignisanalyse und -visualisierung mit OMS)](service-fabric-diagnostics-event-analysis-oms.md)
