---
title: Sammeln von Protokollen mit der Azure-Diagnose | Microsoft Docs
description: "In diesem Artikel wird beschrieben, wie Sie die Azure-Diagnose so konfigurieren, dass Protokolle aus einem Service Fabric-Cluster unter Azure gesammelt werden."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 9f7e1fa5-6543-4efd-b53f-39510f18df56
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/17/2017
ms.author: dekapur
translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: 7c3311800af27016b7f993b375055bbc65d9a727
ms.lasthandoff: 04/27/2017


---
# <a name="collect-logs-by-using-azure-diagnostics"></a>Sammeln von Protokollen mit der Azure-Diagnose
> [!div class="op_single_selector"]
> * [Windows](service-fabric-diagnostics-how-to-setup-wad.md)
> * [Linux](service-fabric-diagnostics-how-to-setup-lad.md)
> 
> 

Bei Verwendung eines Azure Service Fabric-Clusters empfiehlt es sich, die Protokolle aller Knoten an einem zentralen Ort zu sammeln. Das Sammeln der Protokolle an einem zentralen Ort hilft Ihnen bei Analyse und Behandlung von Problemen, die ggf. in Ihrem Cluster oder in den Anwendungen und Diensten des Clusters auftreten.

Eine Möglichkeit zum Hochladen und Sammeln von Protokollen ist die Verwendung der Azure-Diagnoseerweiterung, mit der Protokolle an Azure Storage, Azure Application Insights oder Azure Event Hubs hochgeladen werden. Direkt im Speicher oder in Event Hubs sind die Protokolle allerdings nicht besonders hilfreich. Sie können jedoch einen externen Prozess verwenden, um die Ereignisse aus dem Speicher zu lesen und in einem Produkt wie [Log Analytics](../log-analytics/log-analytics-service-fabric.md) oder in einer anderen Protokollanalyselösung zu verwenden. [Azure Application Insights](https://azure.microsoft.com/services/application-insights/) bietet einen umfassenden Such- und Analysedienst für Protokolle.

## <a name="prerequisites"></a>Voraussetzungen
Einige der Vorgänge in diesem Dokument werden mithilfe folgender Tools ausgeführt:

* [Azure-Diagnose](../cloud-services/cloud-services-dotnet-diagnostics.md) (bezieht sich auf Azure Cloud Services, enthält jedoch hilfreiche Informationen und Beispiele)
* [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md)
* [Azure PowerShell](/powershell/azure/overview)
* [Azure Resource Manager-Client](https://github.com/projectkudu/ARMClient)
* [Azure Resource Manager-Vorlage](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="log-sources-that-you-might-want-to-collect"></a>Protokollquellen, die gesammelt werden können
* **Service Fabric-Protokolle**: Werden von der Plattform für standardmäßige Kanäle der Ereignisablaufverfolgung für Windows und EventSource-Kanäle ausgegeben. Protokolle können unterschiedlicher Art sein:
  * Betriebsereignisse: Protokolle für Vorgänge, die von der Service Fabric-Plattform durchgeführt werden. Beispiele hierfür wären die Erstellung von Anwendungen und Diensten, Knotenzustandsänderungen und Upgradeinformationen.
  * [Ereignisse des Reliable Actors-Programmiermodells](service-fabric-reliable-actors-diagnostics.md)
  * [Ereignisse des Reliable Services-Programmiermodells](service-fabric-reliable-services-diagnostics.md)
* **Anwendungsereignisse**: Ereignisse, die vom Code Ihres Diensts ausgegeben werden und mit der EventSource-Hilfsklasse der Visual Studio-Vorlagen ausgegeben werden. Weitere Informationen zum Schreiben von Protokollen aus Ihrer Anwendung finden Sie unter [Überwachen und Diagnostizieren von Diensten in einer Entwicklungsumgebung auf einem lokalen Computer](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).

## <a name="deploy-the-diagnostics-extension"></a>Bereitstellen der Diagnoseerweiterung
Zum Sammeln von Protokollen muss zunächst die Diagnoseerweiterung auf allen VMs des Service Fabric-Clusters bereitgestellt werden. Die Diagnoseerweiterung sammelt Protokolle auf allen VMs und lädt sie in das angegebene Speicherkonto hoch. Die auszuführenden Schritte variieren ein wenig, je nachdem, ob Sie das Azure-Portal oder Azure Resource Manager verwenden. Diese Schritte hängen auch davon ab, ob die Bereitstellung während der Clustererstellung oder für einen bereits vorhandenen Cluster erfolgt. Wir sehen uns nun die Schritte für die einzelnen Szenarien an.

### <a name="deploy-the-diagnostics-extension-as-part-of-cluster-creation-through-the-portal"></a>Bereitstellen der Diagnoseerweiterung im Rahmen der Clustererstellung über das Portal
Um die Diagnoseerweiterung im Rahmen der Clustererstellung für die im Cluster enthaltenen VMs bereitzustellen, wird verwenden Sie das in der folgenden Abbildung gezeigte Fenster „Diagnoseeinstellungen“. Stellen Sie sicher, dass für die Diagnose **Ein**(Standardeinstellung) festgelegt ist, um die Reliable Actors- oder Reliable Services-Ereignissammlung zu aktivieren. Nach der Erstellung des Clusters können Sie diese Einstellung nicht im Portal ändern.

![Azure-Diagnose-Einstellung im Portal für die Clustererstellung](./media/service-fabric-diagnostics-how-to-setup-wad/portal-cluster-creation-diagnostics-setting.png)

Das Azure-Supportteam *benötigt* die Supportprotokolle zum Bearbeiten aller von Ihnen erstellten Supportanforderungen. Diese Protokolle werden in Echtzeit erfasst und in einem der Speicherkonten gespeichert, die in der aktuellen Ressourcengruppe erstellt werden. Die Diagnoseeinstellungen konfigurieren Ereignisse auf Anwendungsebene. Zu diesen Ereignissen gehören [Reliable Actors](service-fabric-reliable-actors-diagnostics.md)-Ereignisse, [Reliable Services](service-fabric-reliable-services-diagnostics.md)-Ereignisse sowie einige Service Fabric-Ereignisse auf Systemebene, die in Azure Storage gespeichert werden sollen.

Produkte wie [Elasticsearch](https://www.elastic.co/guide/index.html) oder Ihr eigener Prozess können die Ereignisse aus dem Speicherkonto abrufen. Es gibt derzeit keine Möglichkeit, die an die Tabelle gesendeten Ereignisse zu filtern oder zu optimieren. Wenn Sie keinen Prozess zum Entfernen von Ereignissen aus der Tabelle implementieren, wächst die Tabelle weiter an.

Bei der Clustererstellung über das Portal sollten Sie unbedingt die Vorlage herunterladen, **bevor Sie auf „OK“ klicken**, um den Cluster zu erstellen. Ausführliche Informationen finden Sie unter [Erstellen eines Service Fabric-Clusters in Azure mithilfe von Azure Resource Manager](service-fabric-cluster-creation-via-arm.md). Sie benötigen die Vorlage für spätere Änderungen, da Sie einige Änderungen nicht über das Portal vornehmen können.

Sie können Vorlagen mithilfe der folgenden Schritte aus dem Portal exportieren. Diese Vorlagen sind möglicherweise etwas schwieriger zu nutzen, da sie NULL-Werte enthalten können, für die Informationen fehlen.

1. Öffnen Sie Ihre Ressourcengruppe.
2. Wählen Sie **Einstellungen**, um den Bereich mit den Einstellungen anzuzeigen.
3. Wählen Sie **Bereitstellungen**, um den Bereich mit dem Bereitstellungsverlauf anzuzeigen.
4. Wählen Sie eine Bereitstellung, um die Details der Bereitstellung anzuzeigen.
5. Wählen Sie **Vorlage exportieren**, um den Bereich mit den Vorlagen anzuzeigen.
6. Wählen Sie **In Datei speichern** aus, um eine ZIP-Datei zu exportieren, die Vorlage-, Parameter- und PowerShell-Dateien enthält.

Nach dem Export der Dateien müssen Sie eine Änderung vornehmen. Bearbeiten Sie die Datei „parameters.json“, und entfernen Sie das Element **adminPassword**. Dann wird beim Ausführen des Bereitstellungsskripts eine Aufforderung zur Kennworteingabe angezeigt. Beim Ausführen des Bereitstellungsskripts müssen Sie unter Umständen Werte für NULL-Parameterwerte angeben.

So verwenden Sie die heruntergeladene Vorlage, um eine Konfiguration zu aktualisieren:

1. Extrahieren Sie den Inhalt in einen Ordner auf dem lokalen Computer.
2. Ändern Sie den Inhalt, sodass er die neue Konfiguration widerspiegelt.
3. Starten Sie PowerShell, und wechseln Sie zu dem Ordner, in dem Sie den Inhalt extrahiert haben.
4. Führen Sie **deploy.ps1** aus, und geben Sie Abonnement-ID, Ressourcengruppenname (verwenden Sie den gleichen Namen, um die Konfiguration zu aktualisieren) und einen eindeutigen Bereitstellungsnamen an.

### <a name="deploy-the-diagnostics-extension-as-part-of-cluster-creation-by-using-azure-resource-manager"></a>Bereitstellen der Diagnoseerweiterung im Rahmen der Clustererstellung mithilfe des Azure-Ressourcen-Managers
Wenn Sie einen Cluster mithilfe von Resource Manager erstellen möchten, müssen Sie der Resource Manager-Vorlage für einen vollständigen Cluster vor der Clustererstellung den JSON-Code für die Diagnosekonfiguration hinzufügen. Die Vorlagenbeispiele für den Ressourcen-Manager enthalten eine Beispielvorlage mit hinzugefügter Diagnosekonfiguration für einen Cluster mit fünf VMs. Diese finden Sie im Azure-Beispielkatalog unter [Ressourcen-Manager-Beispielvorlage für einen Cluster mit fünf Knoten und Diagnose](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype-wad).

Öffnen Sie die Datei „azuredeploy.json“ und suchen nach **IaaSDiagnostics**, um die Diagnoseeinstellung in der Resource Manager-Vorlage anzuzeigen. Klicken Sie zum Erstellen eines Clusters mit dieser Vorlage einfach auf die Schaltfläche **In Azure bereitstellen** (unter dem oben angegebenen Link).

Alternativ können Sie das Resource Manager-Beispiel herunterladen, anpassen und den Befehl `New-AzureRmResourceGroupDeployment` in einem Azure PowerShell-Fenster ausführen, um einen Cluster mit der geänderten Vorlage zu erstellen. Im folgenden Code finden Sie die Parameter, die Sie an den Befehl übergeben. Ausführliche Informationen zum Bereitstellen einer Ressourcengruppe mit PowerShell finden Sie im Artikel [Bereitstellen einer Ressourcengruppe mit einer Azure Resource Manager-Vorlage](../azure-resource-manager/resource-group-template-deploy.md).

```powershell

New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -Name $deploymentName -TemplateFile $pathToARMConfigJsonFile -TemplateParameterFile $pathToParameterFile –Verbose
```

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

## <a name="update-diagnostics-to-collect-health-and-load-events"></a>Aktualisieren der Diagnose, um Integritäts- und Auslastungsereignisse zu sammeln

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

## <a name="update-diagnostics-to-collect-and-upload-logs-from-new-eventsource-channels"></a>Aktualisieren der Diagnose zum Sammeln und Hochladen von Protokollen aus neuen EventSource-Kanälen
Wenn Sie die Diagnose aktualisieren möchten, damit Protokolle aus neuen EventSource-Kanälen gesammelt werden, die eine neu bereitzustellende Anwendung darstellen, führen Sie einfach die gleichen Schritte aus wie im [vorherigen Abschnitt](#deploywadarm), um die Diagnose für einen vorhandenen Cluster einzurichten.

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

## <a name="next-steps"></a>Nächste Schritte
Sehen Sie sich die Diagnoseereignisse an, die für [Reliable Actors](service-fabric-reliable-actors-diagnostics.md) und [Reliable Services](service-fabric-reliable-services-diagnostics.md) ausgegeben werden, um besser zu verstehen, welche Ereignisse Sie beim Behandeln von Problemen untersuchen sollten.

## <a name="related-articles"></a>Verwandte Artikel
* [Erfahren Sie, wie Sie Leistungsindikatoren oder Protokolle mithilfe der Diagnoseerweiterung sammeln können.](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Service Fabric-Lösung in Log Analytics](../log-analytics/log-analytics-service-fabric.md)


