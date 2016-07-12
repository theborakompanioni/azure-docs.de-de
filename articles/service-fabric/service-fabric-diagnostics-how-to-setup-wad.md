<properties
   pageTitle="Sammeln von Protokollen mit Azure-Diagnose | Microsoft Azure"
   description="In diesem Artikel wird beschrieben, wie Sie die Azure-Diagnose so konfigurieren, dass Protokolle aus einem Service Fabric-Cluster unter Azure gesammelt werden."
   services="service-fabric"
   documentationCenter=".net"
   authors="ms-toddabel"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="06/24/2016"
   ms.author="toddabel"/>


# Sammeln von Protokollen mit Azure-Diagnose

Bei Verwendung eines Azure Service Fabric-Clusters empfiehlt es sich, die Protokolle aller Knoten an einem zentralen Ort zu sammeln. Das Sammeln der Protokolle an einem zentralen Ort vereinfacht die Analyse und Behandlung von Problemen, die ggf. in Ihrem Cluster oder in den Anwendungen und Diensten des Clusters auftreten. Eine Möglichkeit zum Hochladen und Sammeln von Protokollen ist die Verwendung der Erweiterung „Azure-Diagnose“, mit der Protokolle an Azure Storage hochgeladen werden. Die Protokolle sind direkt im Speicher nicht sehr nützlich, jedoch kann ein externer Prozess verwendet werden, um die Ereignisse aus dem Speicher zu lesen und in einem Produkt wie z.B. [Elasticsearch](service-fabric-diagnostic-how-to-use-elasticsearch.md) oder einer anderen Protokollanalyselösung zu platzieren.

## Voraussetzungen
Diese Tools werden verwendet, um einige Vorgänge in diesem Dokument durchzuführen:

* [Azure-Diagnose](../cloud-services/cloud-services-dotnet-diagnostics.md) (bezieht sich auf Azure Cloud Services, enthält jedoch hilfreiche Informationen und Beispiele)
* [Azure Resource Manager](../resource-group-overview.md)
* [Azure PowerShell](../powershell-install-configure.md)
* [Azure Resource Manager-Client](https://github.com/projectkudu/ARMClient)
* [Erstellen eines virtuellen Windows-Computers mit Überwachung und Diagnose mithilfe von Azure-Ressourcen-Manager-Vorlagen](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md)


## Andere Protokollquellen, die gesammelt werden können
1. **Service Fabric-Protokolle:** Werden von der Plattform für standardmäßige ETW- und EventSource-Kanäle ausgegeben. Protokolle können unterschiedlicher Art sein:
  - Betriebsereignisse: Protokolle für Vorgänge, die von der Service Fabric-Plattform durchgeführt werden. Beispiele hierfür wären die Erstellung von Anwendungen und Diensten, Knotenzustandsänderungen und Upgradeinformationen.
  - [Ereignisse des Actor-Programmiermodells](service-fabric-reliable-actors-diagnostics.md)
  - [Ereignisse des Reliable Services-Programmiermodells](service-fabric-reliable-services-diagnostics.md)
2. **Anwendungsereignisse:** Ereignisse, die von Ihrem Dienstcode ausgegeben werden und mit der EventSource-Hilfsklasse der Visual Studio-Vorlagen ausgegeben werden. Weitere Informationen zum Schreiben von Protokollen aus Ihrer Anwendung finden Sie in [diesem Artikel zur Überwachung und Diagnose von Diensten in einer lokalen Installation](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).


## Bereitstellen der Diagnoseerweiterungen
Zum Sammeln von Protokollen muss zunächst die Diagnoseerweiterung auf allen VMs des Service Fabric-Clusters bereitgestellt werden. Die Diagnoseerweiterung sammelt Protokolle auf allen VMs und lädt sie an das angegebene Speicherkonto hoch. Je nachdem, ob Sie das Azure-Portal oder den Azure-Ressourcen-Manager verwenden und ob die Bereitstellung im Rahmen der Clustererstellung oder für einen bereits vorhandenen Cluster erfolgt, variieren die Schritte etwas. Wir sehen uns nun die Schritte für die einzelnen Szenarien an.

### Bereitstellen der Diagnoseerweiterung im Rahmen der Clustererstellung über das Portal
Um die Diagnoseerweiterung im Rahmen der Clustererstellung für die im Cluster enthaltenen VMs bereitzustellen, wird das in der folgenden Abbildung gezeigte Fenster „Diagnoseeinstellungen“ verwendet. Stellen Sie sicher, dass für die Diagnose **Ein** (Standardeinstellung) festgelegt ist, um die Actor- oder Reliable Service-Ereignissammlung zu aktivieren. Nach der Erstellung des Clusters kann diese Einstellung nicht über das Portal geändert werden.

![Azure-Diagnose-Einstellung im Portal für die Clustererstellung](./media/service-fabric-diagnostics-how-to-setup-wad/portal-cluster-creation-diagnostics-setting.png)

Für das Azure-Supportteam sind die Supportprotokolle **erforderlich**, um jegliche Supportanforderungen abzuwickeln, die Sie erstellen. Diese Protokolle werden in Echtzeit erfasst und in einem der Speicherkonten gespeichert, die in der aktuellen Ressourcengruppe erstellt werden. Mit der Diagnoseeinstellung werden Ereignisse auf Anwendungsebene konfiguriert, z.B. [Actor](service-fabric-reliable-actors-diagnostics.md)- und [Reliable Service](service-fabric-reliable-services-diagnostics.md)-Ereignisse sowie einige Service Fabric-Ereignisse auf Systemebene, die im Azure-Speicher gespeichert werden. Produkte wie z.B. [Elasticsearch](service-fabric-diagnostic-how-to-use-elasticsearch.md) oder Ihr eigener Prozess können die Ereignisse aus dem Speicherkonto übernehmen. Es gibt derzeit keine Möglichkeit, die an die Tabelle gesendeten Ereignisse zu filtern oder zu optimieren. Wenn kein Prozess zum Entfernen von Ereignissen aus der Tabelle implementiert ist, wächst die Tabelle weiter an.

Bei der Clustererstellung über das Portal wird dringend empfohlen, erst *nach dem Herunterladen der Vorlage* auf „OK“ zu klicken, um den Cluster zu erstellen. Ausführliche Informationen finden Sie unter [Einrichten eines Service Fabric-Clusters mit einer Azure Resource Manager-Vorlage](service-fabric-cluster-creation-via-arm.md). So erhalten Sie eine verwendbare ARM-Vorlage für den zu erstellenden Cluster. Diese wird für spätere Änderungen benötigt. Nicht alle Änderungen können über das Portal vorgenommen werden. Vorlagen können im Portal zwar mithilfe der folgenden Schritte exportiert werden, diese Vorlagen sind jedoch unter Umständen nicht ohne Weiteres verwendbar, da sie möglicherweise einige NULL-Werte besitzen, für die Werte angegeben werden müssen, oder gar keine der benötigten Informationen enthalten:

1. Öffnen Ihrer Ressourcengruppe
2. Wählen von „Einstellungen“ zum Anzeigen des Fensters „Einstellungen“
3. Wählen von „Bereitstellungen“ zum Anzeigen des Fensters mit dem Bereitstellungsverlaufs
4. Wählen einer Bereitstellung zum Anzeigen der Details der Bereitstellung
5. Wählen von „Vorlage exportieren“ zum Anzeigen des Fensters „Vorlage“
6. Wählen Sie „In Datei speichern“ aus, um eine ZIP-Datei zu exportieren, die Vorlage-, Parameter- und PowerShell-Dateien enthält.

Nach dem Exportieren der Dateien ist eine Änderung erforderlich. Bearbeiten Sie die Datei **parameters.json**, und entfernen Sie das Element **adminPassword**. Dann wird beim Ausführen des Bereitstellungsskripts eine Aufforderung zur Kennworteingabe angezeigt. Beim Ausführen des Bereitstellungsskripts müssen unter Umständen Werte für NULL-Parameterwerte angegeben werden. So verwenden Sie die heruntergeladene Vorlage, um eine Konfiguration zu aktualisieren

1. Extrahieren Sie den Inhalt in einen Ordner auf dem lokalen Computer.
2. Ändern Sie den Inhalt, sodass er die neue Konfiguration widerspiegelt.
3. Starten Sie PowerShell, und wechseln Sie zu dem Ordner, in dem Sie den Inhalt extrahiert haben.
4. Führen Sie **deploy.ps1** aus, und geben Sie Abonnement-ID, Ressourcengruppenname (verwenden Sie den gleichen Namen, um die Konfiguration zu aktualisieren) und einen eindeutigen Bereitstellungsnamen an.


### Bereitstellen der Diagnoseerweiterung im Rahmen der Clustererstellung mithilfe des Azure Resource Managers
Wenn Sie einen Cluster mithilfe des Ressourcen-Managers erstellen möchten, müssen Sie der Ressourcen-Manager-Vorlage vom Typ „Vollständiger Cluster“ vor der Clustererstellung den JSON-Code für die Diagnosekonfiguration hinzufügen. Die Vorlagenbeispiele für den Ressourcen-Manager enthalten eine Beispielvorlage mit hinzugefügter Diagnosekonfiguration für einen Cluster mit fünf VMs. Diese finden Sie im Azure-Beispielkatalog unter [Ressourcen-Manager-Beispielvorlage für einen Cluster mit fünf Knoten und Diagnose](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype-wad). Öffnen Sie die Datei **azuredeploy.json** und suchen nach **IaaSDiagnostics**, um die Diagnoseeinstellung in der Resource Manager-Vorlage anzuzeigen. Klicken Sie zum Erstellen eines Clusters mit dieser Vorlage einfach auf die Schaltfläche **In Azure bereitstellen** (unter dem oben angegebenen Link).

Alternativ können Sie das Ressourcen-Manager-Beispiel herunterladen, anpassen und den Befehl `New-AzureRmResourceGroupDeployment` in einem Azure PowerShell-Fenster ausführen, um einen Cluster mit der geänderten Vorlage zu erstellen. Informationen zu den Parametern, die an den Befehl übergeben werden müssen, finden Sie weiter unten. Ausführliche Informationen zum Bereitstellen einer Ressourcengruppe mit PowerShell finden Sie im Artikel [Bereitstellen einer Ressourcengruppe mit einer Azure Resource Manager-Vorlage](../resource-group-template-deploy.md).

```powershell

New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -Name $deploymentName -TemplateFile $pathToARMConfigJsonFile -TemplateParameterFile $pathToParameterFile –Verbose
```

### Bereitstellen der Diagnoseerweiterung für einen vorhandenen Cluster
Wenn Sie bereits über einen Cluster ohne Diagnosebereitstellung verfügen oder eine vorhandene Konfiguration ändern möchten, können Sie die Diagnose mit den folgenden Schritten hinzufügen oder aktualisieren. Ändern Sie die zum Erstellen des vorhandenen Clusters verwendete ARM-Vorlage, oder laden Sie die Vorlage, wie oben beschrieben, aus dem Portal herunter. Ändern Sie die Datei **template.json**, indem Sie die folgenden Aufgaben ausführen:

Fügen Sie der Vorlage eine neue Speicherressource hinzu, indem Sie sie dem Ressourcenabschnitt hinzufügen.

##### Aktualisieren des Ressourcenabschnitts
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

 Fügen Sie als Nächstes dem Parameterabschnitt etwas hinzu, direkt nach den Speicherkontodefinitionen, zwischen „supportLogStorageAccountName“ und „vmNodeType0Name“. Ersetzen Sie den Platzhaltertext *storage account name goes here* durch den Namen des gewünschten Speicherkontos.

##### Aktualisieren des Parameterabschnitts
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
Aktualisieren Sie dann den Abschnitt *VirtualMachineProfile* von **template.json** durch Hinzufügen des folgenden Codes innerhalb des Arrays „extensions“. Achten Sie darauf, ein Komma am Anfang oder Ende hinzuzufügen – je nach Einfügeposition.

##### Hinzufügen zum Array „extensions“ von „VirtualMachineProfile“
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

Nachdem Sie die Datei **template.json** wie beschrieben geändert haben, veröffentlichen Sie die ARM-Vorlage erneut. Wenn die Vorlage exportiert wurde, wird sie durch Ausführen der Datei **deploy.ps1** neu veröffentlicht. Stellen Sie nach der Bereitstellung sicher, dass *ProvisioningState* den Status *Erfolgreich* hat.


## Aktualisieren der Diagnose zum Sammeln und Hochladen von Protokollen aus neuen EventSource-Kanälen
Wenn Sie die Diagnose für das Sammeln von Protokollen aus neuen EventSource-Kanälen aktualisieren möchten, die eine neu bereitzustellende Anwendung darstellen, müssen Sie einfach nur die gleichen Schritte wie im [obigen Abschnitt](#deploywadarm) ausführen, in denen das Einrichten der Diagnose für einen vorhandenen Cluster beschrieben wird. Sie müssen den Abschnitt *EtwEventSourceProviderConfiguration* in der Datei **template.json** aktualisieren, um Einträge für die neuen EventSource-Elemente hinzuzufügen, bevor Sie das Konfigurationsupdate mit dem PowerShell-Befehl *New-AzureRmResourceGroupDeployment* ausführen. Der Name der Ereignisquelle wird als Teil des Codes in der von Visual Studio generierten Datei **ServiceEventSource.cs** definiert.


## Nächste Schritte
Sehen Sie sich die Diagnoseereignisse an, die für [Reliable Actors](service-fabric-reliable-actors-diagnostics.md) und [Reliable Services](service-fabric-reliable-services-diagnostics.md) ausgegeben werden, um besser zu verstehen, welche Ereignisse Sie beim Behandeln von Problemen untersuchen sollten.


## Verwandte Artikel
* [Erfahren Sie, wie Sie Leistungsindikatoren oder Protokolle mit diagnostischen Erweiterungen erfassen können.](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md)
* [Service Fabric-Lösung in Log Analytics](../log-analytics/log-analytics-service-fabric.md)

<!---HONumber=AcomDC_0629_2016-->