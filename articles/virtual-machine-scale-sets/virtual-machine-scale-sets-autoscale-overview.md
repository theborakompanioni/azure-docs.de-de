<properties
	pageTitle="Automatische Skalierung und Skalierungsgruppen für virtuelle Computer | Microsoft Azure"
	description="Hier erfahren Sie, wie Sie virtuelle Computer in einer Skalierungsgruppe mithilfe der Diagnose und mit Ressourcen für die automatische Skalierung automatisch skalieren."
    services="virtual-machine-scale-sets"
	documentationCenter=""
	authors="davidmu1"
	manager="timlt"
	editor=""
	tags="azure-resource-manager"/>

<tags
	ms.service="virtual-machine-scale-sets"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="06/10/2016"
	ms.author="davidmu"/>

# Automatische Skalierung und Skalierungsgruppen für virtuelle Computer

Automatische Skalierung virtueller Computer in einer Skalierungsgruppe bedeutet, dass Computer in der Gruppe erstellt oder daraus gelöscht werden, wenn dies für eine Anwendung erforderlich ist, damit Leistungsanforderungen erfüllt und Vereinbarungen zum Servicelevel (SLAs) eingehalten werden. Wenn das Arbeitsvolumen zunimmt, benötigt eine Anwendung unter Umständen zusätzliche Ressourcen, um ihre Aufgaben effizient auszuführen.

Die automatische Skalierung ist ein automatisierter Prozess, der den Verwaltungsaufwand erleichtert. Aufgrund der geringeren Aufwands ist es nicht erforderlich, die Systemleistung durchgehend zu überwachen oder Entscheidungen in Bezug auf das Hinzufügen oder Entfernen von Ressourcen zu treffen. Skalieren ist ein flexibler Prozess. Bei steigender Auslastung des Systems können weitere Ressourcen bereitgestellt werden, bei geringerer Nachfrage ist es jedoch auch möglich, Ressourcen freizugeben, um Kosten zu verringern und gleichzeitig die Leistung beizubehalten und die Einhaltung der SLAs sicherzustellen.

Richten Sie die automatische Skalierung für eine Skalierungsgruppe mit einer Azure Resource Manager-Vorlage, Azure PowerShell oder der Azure-CLI ein.

## Einrichten der Skalierung mithilfe von Resource Manager-Vorlagen

Anstatt jede Ressource Ihrer Anwendung gesondert bereitzustellen und zu verwalten, können Sie eine Vorlage verwenden, die die Bereitstellung aller Ressourcen in einem einzigen koordinierten Vorgang ermöglicht. In der Vorlage werden Anwendungsressource definiert und Bereitstellungsparameter für verschiedene Umgebungen festgelegt. Die Vorlage besteht aus JSON und Ausdrücken, mit denen Sie Werte für die Bereitstellung erstellen können. Weitere Informationen finden Sie unter [Erstellen von Azure Resource Manager-Vorlagen](../resource-group-authoring-templates.md).

Legen Sie in der Vorlage das Kapazitätselement fest:

    "sku": {
      "name": "Standard_A0",
      "tier": "Standard",
      "capacity": 3
    },

Die Kapazität gibt die Anzahl von virtuellen Computern in der Gruppe an. Sie können die Kapazität manuell ändern, indem Sie eine Vorlage mit einem anderen Wert bereitstellen. Wenn Sie eine Vorlage bereitstellen, um die Kapazität zu ändern, können Sie nur das SKU-Element mit der aktualisierten Kapazität einbeziehen.

Die Kapazität der Skalierungsgruppe wird automatisch geändert, indem eine Kombination der von Azure Resource Manager bereitgestellten Ressource „autoscaleSettings“ und der auf den Computern in der Skalierungsgruppe installierten Azure-Diagnoseerweiterung verwendet wird.

### Konfigurieren der Azure-Diagnoseerweiterung

Die automatische Skalierung ist nur möglich, wenn die Metrikauflistung auf jedem virtuellen Computer in der Skalierungsgruppe erfolgreich ist. Die Azure-Diagnoseerweiterung bietet Überwachungs- und Diagnosefunktionen, die die Anforderungen der Ressource für die automatische Skalierung in Bezug auf die Metrikauflistung erfüllen. Sie können die Erweiterung als Teil der Resource Manager-Vorlage installieren.

Dieses Beispiel zeigt die Variablen, die in der Vorlage zum Konfigurieren der Diagnoseerweiterung verwendet werden:

	"diagnosticsStorageAccountName": "[concat(parameters('resourcePrefix'), 'saa')]",
	"accountid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/', 'Microsoft.Storage/storageAccounts/', variables('diagnosticsStorageAccountName'))]",
	"wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB="4096" xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter="Error"/> <WindowsEventLog scheduledTransferPeriod="PT1M" > <DataSource name="Application!*[System[(Level = 1 or Level = 2)]]" /> <DataSource name="Security!*[System[(Level = 1 or Level = 2)]]" /> <DataSource name="System!*[System[(Level = 1 or Level = 2)]]" /></WindowsEventLog>",
	"wadperfcounter": "<PerformanceCounters scheduledTransferPeriod="PT1M"><PerformanceCounterConfiguration counterSpecifier="\\Processor(_Total)\\Thread Count" sampleRate="PT15S" unit="Percent"><annotation displayName="Thread Count" locale="de-DE"/></PerformanceCounterConfiguration></PerformanceCounters>",
	"wadcfgxstart": "[concat(variables('wadlogs'),variables('wadperfcounter'),'<Metrics resourceId="')]",
	"wadmetricsresourceid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name ,'/providers/','Microsoft.Compute/virtualMachineScaleSets/',parameters('vmssName'))]",
	"wadcfgxend": "[concat('"><MetricAggregation scheduledTransferPeriod="PT1H"/><MetricAggregation scheduledTransferPeriod="PT1M"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>')]"

Parameter werden angegeben, wenn die Vorlage für Werte wie etwa den Namen des Speicherkontos, in dem Daten gespeichert werden, und den Namen der Skalierungsgruppe, aus der Daten gesammelt werden, bereitgestellt wird. In diesem Beispiel zu Windows Server wird nur der Leistungsindikator „Threadanzahl“ erfasst, alle in Windows oder Linux verfügbaren Leistungsindikatoren können jedoch zum Erfassen von Diagnoseinformationen verwendet und in die Erweiterungskonfiguration einbezogen werden.

Dieses Beispiel zeigt die Definition der Erweiterung in der Vorlage:

    "extensionProfile": {
      "extensions": [
        {
          "name": "Microsoft.Insights.VMDiagnosticsSettings",
          "properties": {
            "publisher": "Microsoft.Azure.Diagnostics",
            "type": "IaaSDiagnostics",
            "typeHandlerVersion": "1.5",
            "autoUpgradeMinorVersion": true,
            "settings": {
              "xmlCfg": "[base64(concat(variables('wadcfgxstart'),variables('wadmetricsresourceid'),variables('wadcfgxend')))]",
              "storageAccount": "[variables('diagnosticsStorageAccountName')]"
            },
            "protectedSettings": {
              "storageAccountName": "[variables('diagnosticsStorageAccountName')]",
              "storageAccountKey": "[listkeys(variables('accountid'), variables('apiVersion')).key1]",
              "storageAccountEndPoint": "https://core.windows.net"
            }
          }
        }
      ]
    }

Wenn die Diagnoseerweiterung ausgeführt wird, werden die Daten in einer Tabelle erfasst, die sich im von Ihnen festgelegten Speicherkonto befindet. Die erfassten Daten befinden sich in der Tabelle „WADPerformanceCounters“. Dies ist z.B. die Threadanzahl, die beim Erstellen der virtuellen Computer in einer Skalierungsgruppe erfasst wurde:

![](./media/virtual-machine-scale-sets-autoscale-overview/ThreadCountBefore2.png)

### Konfigurieren der Ressource „autoScaleSettings“

Die Ressource „autoscaleSettings“ verwendet die Informationen, die von der Diagnoseerweiterung erfasst wurden, um Entscheidungen in Bezug darauf zu treffen, ob die Anzahl von virtuellen Computern in der Skalierungsgruppe vergrößert oder verkleinert werden soll.

Dieses Beispiel zeigt die Konfiguration der Ressource in der Vorlage:

    {
      "type": "Microsoft.Insights/autoscaleSettings",
      "apiVersion": "2015-04-01",
      "name": "[concat(parameters('resourcePrefix'),'as1')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]"
      ],
      "properties": {
        "enabled": true,
        "name": "[concat(parameters('resourcePrefix'),'as1')]",
        "profiles": [
          {
            "name": "Profile1",
            "capacity": {
              "minimum": "1",
              "maximum": "10",
              "default": "1"
            },
            "rules": [
              {
                "metricTrigger": {
                  "metricName": "\\Process(_Total)\\Thread Count",
                  "metricNamespace": "",
                  "metricResourceUri": "[concat('/subscriptions/',subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]",
                  "timeGrain": "PT1M",
                  "statistic": "Average",
                  "timeWindow": "PT5M",
                  "timeAggregation": "Average",
                  "operator": "GreaterThan",
                  "threshold": 650
                },
                "scaleAction": {
                  "direction": "Increase",
                  "type": "ChangeCount",
                  "value": "1",
                  "cooldown": "PT5M"
                }
              },
              {
                "metricTrigger": {
                  "metricName": "\\Process(_Total)\\Thread Count",
                  "metricNamespace": "",
                  "metricResourceUri": "[concat('/subscriptions/',subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]",
                  "timeGrain": "PT1M",
                  "statistic": "Average",
                  "timeWindow": "PT5M",
                  "timeAggregation": "Average",
                  "operator": "LessThan",
                  "threshold": 550
                },
                "scaleAction": {
                  "direction": "Decrease",
                  "type": "ChangeCount",
                  "value": "1",
                  "cooldown": "PT5M"
                }
              }
            ]
          }
        ],
        "targetResourceUri": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]"
      }
    }

Im obigen Beispiel werden zwei Regeln erstellt, um Aktionen für die automatische Skalierung zu definieren: Die erste Regel definiert das Hochskalieren, die zweite das Herunterskalieren. Diese Werte werden in den Regeln bereitgestellt:

- **metricName**: Entspricht dem Leistungsindikator, den wir in der Variablen „wadperfcounter“ für die Diagnoseerweiterung definiert haben. Im obigen Beispiel wird der Zähler für die Threadanzahl verwendet.
- **metricResourceUri**: Dies ist der Ressourcenbezeichner der VM-Skalierungsgruppe. Dieser Bezeichner enthält den Namen der Ressourcengruppe, den Namen des Ressourcenanbieters und den Namen der zu skalierenden Skalierungsgruppe.
- **timeGrain**: Dies ist die Granularität der erfassten Metriken. Im obigen Beispiel werden Daten in einem Intervall von einer Minute erfasst. Dieser Wert wird in Kombination mit „timeWindow“ verwendet.
- **statistic**: Hiermit wird bestimmt, wie die Metriken für die Durchführung der automatischen Skalierungsaktion kombiniert werden. Mögliche Werte sind: Average, Min, Max.
- **timeWindow**: Der Zeitbereich, in dem Instanzdaten gesammelt werden. Der Wert muss zwischen fünf Minuten und zwölf Stunden liegen.
- **timeAggregation**: Hiermit wird bestimmt, wie die gesammelten Daten im Laufe der Zeit kombiniert werden sollen. Der Standardwert ist "Average". Mögliche Werte sind: Average, Minimum, Maximum, Last, Total, Count.
- **operator**: Dies ist der Operator, der zum Vergleichen der Metrikdaten und des Schwellenwerts verwendet wird. Mögliche Werte sind: Equals, NotEquals, GreaterThan, GreaterThanOrEqual, LessThan, LessThanOrEqual.
- **threshold**: Dies ist der Wert, mit dem die Skalierungsaktion ausgelöst wird. Daher müssen Sie einen ausreichenden Unterschied zwischen den Schwellenwerten für das Hochskalieren und das Herunterskalieren festlegen. Wenn Sie für beides die gleichen Werte festlegen (z.B. 600 Threads für beide wie im obigen Beispiel), erwartet das System, dass sich die Größe der Gruppe ständig vergrößert und verkleinert und implementiert daher keine Skalierungsaktion.
- **direction**: Hiermit wird bestimmt, welche Aktion ausgeführt wird, wenn der Schwellenwert erreicht ist. Mögliche Werte sind „Increase“ oder „Decrease“.
- **type**: Dies ist die Art von Aktion, die eintreten soll. Diese Option muss auf „ChangeCount“ festgelegt werden.
- **value**: Die Anzahl von virtuellen Computern, die der Skalierungsgruppe hinzugefügt bzw. daraus entfernt werden. Dieser Wert muss 1 oder höher lauten.
- **cooldown**: Gibt an, wie lange nach der letzten Skalierungsaktion gewartet wird, bevor die nächste Aktion durchgeführt wird. Dieser Wert muss zwischen einer Minute und einer Woche liegen.

Je nach dem verwendeten Leistungsindikator werden einige der Elemente in der Vorlagenkonfiguration unterschiedlich verwendet. Im gezeigten Beispiel wird die Threadanzahl als Leistungsindikator verwendet, und der Schwellenwert ist auf 650 für das Hochaskalieren bzw. auf 550 für das Herunterskalieren festgelegt. Wenn Sie einen Indikator wie „%Processor Time“ verwenden, wird der Wert auf den Prozentsatz der CPU-Auslastung festgesetzt, der eine Skalierungsaktion bedingt.

Wenn auf den virtuellen Computern in der Gruppe, die eine Skalierungsaktion auslöst, eine Last entsteht, wird die Anzahl von Computern in der Gruppe ausgehend vom Wertelement in der Vorlage vergrößert. Dieses Beispiel zeigt eine Skalierungsgruppe, in der die Kapazität auf 3 und der Wert für die Skalierungsaktion auf 1 festgelegt ist:

![](./media/virtual-machine-scale-sets-autoscale-overview/ResourceExplorerBefore.png)

Bei einer Last, bei der die durchschnittliche Threadanzahl den Schwellenwert von 650 übersteigt:

![](./media/virtual-machine-scale-sets-autoscale-overview/ThreadCountAfter.png)

Ein Hochskalieren wird ausgelöst, wodurch sich die Kapazität der Gruppe um den Wert 1 erhöht.

    "sku": {
      "name": "Standard_A0",
      "tier": "Standard",
      "capacity": 4
    },

Der Skalierungsgruppe wird ein virtueller Computer hinzugefügt:

![](./media/virtual-machine-scale-sets-autoscale-overview/ResourceExplorerAfter.png)

Wenn die durchschnittliche Threadanzahl nach einer Abkühlperiode von fünf Minuten noch immer größer ist als 600, wird der Gruppe ein weiterer Computer hinzugefügt. Wenn die durchschnittliche Threadanzahl kleiner als 550 bleibt, wird die Kapazität der Skalierungsgruppe um den Wert 1 verringert, und ein Computer wird aus der Gruppe entfernt.

## Einrichten der Skalierung mithilfe von Azure PowerShell

Azure PowerShell kann zum Einrichten der automatischen Skalierung für eine Skalierungsgruppe verwendet werden. Hier sehen Sie ein Beispiel für die Erstellung der weiter oben in diesem Artikel beschriebenen Regeln mithilfe von PowerShell:

    $rule1 = New-AutoscaleRule -MetricName "\Processor(_Total)\Thread Count" -MetricResourceId "/subscriptions/{subscription-id}/resourceGroups/myrg1/providers/Microsoft.Compute/virtualMachineScaleSets/myvmss1" -Operator GreaterThan -MetricStatistic Average -Threshold 650 -TimeGrain 00:01:00 -ScaleActionCooldown 00:05:00 -ScaleActionDirection Increase -ScaleActionScaleType ChangeCount -ScaleActionValue "1"  
 
    $rule2 = New-AutoscaleRule -MetricName "\Processor(_Total)\% Processor Time" -MetricResourceId "/subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/rainvmss/providers/Microsoft.Compute/virtualMachineScaleSets/rainvmss1" -Operator LessThan -MetricStatistic Average -Threshold 10 -TimeGrain 00:01:00 -ScaleActionCooldown 00:10:00 -ScaleActionDirection Decrease -ScaleActionScaleType ChangeCount -ScaleActionValue "2" 
 
    $profile1 = New-AutoscaleProfile -DefaultCapacity "2" -MaximumCapacity "10" -MinimumCapacity "2" -Rules $rule1, $rule2 -Name "ScalePuppy" 
 
    Add-AutoscaleSetting -Location "East US" -Name 'MyScaleVMSSSetting' -ResourceGroup rainvmss -TargetResourceId "/subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/rainvmss/providers/Microsoft.Compute/virtualMachineScaleSets/rainvmss1" -AutoscaleProfiles $profile1 

## Einrichten der Skalierung mithilfe der Azure-Befehlszeilenschnittstelle

(Informationen noch nicht verfügbar.)

## Untersuchen von Skalierungsaktionen

- [Azure-Portal](): Das Portal liefert aktuell eine begrenzte Menge an Informationen.
- [Azure-Ressourcen-Explorer](): Dies ist das Tool, das am besten zum Untersuchen des aktuellen Zustands Ihrer Skalierungsgruppe geeignet ist. Über den folgenden Pfad gelangen Sie zur Instanzansicht für die von Ihnen erstellte Skalierungsgruppe: Abonnements > {Ihr Abonnement} > resourceGroups > {Ihre Ressourcengruppe} > Anbieter > Microsoft.Compute > virtualMachineScaleSets > {Ihre Skalierungsgruppe} > virtualMachines
- Azure PowerShell: Verwenden Sie den folgenden Befehl, um Informationen zu erhalten:

        Get-AzureRmResource -name vmsstest1 -ResourceGroupName vmsstestrg1 -ResourceType Microsoft.Compute/virtualMachineScaleSets -ApiVersion 2015-06-15
        Get-Autoscalesetting -ResourceGroup rainvmss -DetailedOutput
        
- Stellen Sie eine Verbindung mit dem virtuellen Jumpbox-Computer her, wie Sie dies auch für jeden anderen Computer tun würden. Sie können dann per Remoteverbindung auf die virtuellen Computer der Skalierungsgruppe zugreifen, um die einzelnen Prozesse zu überwachen.

## Nächste Schritte

- Unter [Automatisches Skalieren von Computern in einer VM-Skalierungsgruppe](virtual-machine-scale-sets-windows-autoscale.md) finden Sie ein Beispiel für die Erstellung einer Skalierungsgruppe mit automatischer Skalierung.
- Beispiele für Überwachungsfeatures von Azure Insights finden Sie unter [Azure Insights – PowerShell-Schnellstartbeispiele](../azure-portal/insights-powershell-samples.md).
- Informationen zu Benachrichtigungsfeatures finden Sie unter [Verwenden von automatischen Skalierungsvorgängen zum Senden von E-Mail- und Webhook-Warnbenachrichtigungen in Azure Insights](../azure-portal/insights-autoscale-to-webhook-email.md) sowie unter [Verwenden von Überwachungsprotokollen zum Senden von E-Mail- und Webhook-Warnbenachrichtigungen in Azure Insights](../azure-portal/insights-auditlog-to-webhook-email.md).
- Erfahren Sie mehr über [erweiterte Szenarien für die automatische Skalierung](./virtual-machine-scale-sets-advanced-autoscale.md).

<!---HONumber=AcomDC_0907_2016-->