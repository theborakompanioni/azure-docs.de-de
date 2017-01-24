---
title: "Automatische Skalierung und Skalierungsgruppen für virtuelle Computer | Microsoft Docs"
description: "Hier erfahren Sie, wie Sie virtuelle Computer in einer Skalierungsgruppe mithilfe der Diagnose und mit Ressourcen für die automatische Skalierung automatisch skalieren."
services: virtual-machine-scale-sets
documentationcenter: 
author: Thraka
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: d29a3385-179e-4331-a315-daa7ea5701df
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/18/2016
ms.author: adegeo
translationtype: Human Translation
ms.sourcegitcommit: 090374b057a62251e40ccc41f60f61e84e08a03f
ms.openlocfilehash: ff137ead5e3490a129b36c959040d3571bff7669


---
# <a name="automatic-scaling-and-virtual-machine-scale-sets"></a>Automatische Skalierung und Skalierungsgruppen für virtuelle Computer
Bei der automatischen Skalierung virtueller Computer in einer Skalierungsgruppe werden je nach Leistungsbedarf Computer in der Gruppe erstellt oder wieder gelöscht. Wenn das Arbeitsvolumen zunimmt, benötigt eine Anwendung unter Umständen zusätzliche Ressourcen, um ihre Aufgaben effizient auszuführen.

Die automatische Skalierung ist ein automatisierter Prozess, der den Verwaltungsaufwand erleichtert. Dank des geringeren Aufwands müssen Sie nicht ständig die Systemleistung überwachen oder Entscheidungen für die Ressourcenverwaltung treffen. Die Skalierung ist ein elastischer Prozess. Bei zunehmender Auslastung können weitere Ressourcen hinzugefügt und bei geringerem Bedarf wieder entfernt werden, um die Kosten gering zu halten, ohne die Leistung zu beeinträchtigen.

Richten Sie die automatische Skalierung für eine Skalierungsgruppe mit einer Azure Resource Manager-Vorlage, Azure PowerShell, Azure-CLI oder im Azure-Portal ein.

## <a name="set-up-scaling-by-using-resource-manager-templates"></a>Einrichten der Skalierung mithilfe von Resource Manager-Vorlagen
Anstatt jede Ressource Ihrer Anwendung gesondert bereitzustellen und zu verwalten, können Sie eine Vorlage verwenden, die alle Ressourcen in einem einzelnen koordinierten Vorgang bereitstellt. In der Vorlage werden Anwendungsressource definiert und Bereitstellungsparameter für verschiedene Umgebungen festgelegt. Die Vorlage besteht aus JSON-Code und Ausdrücken, mit denen Sie Werte für Ihre Bereitstellung erstellen können. Weitere Informationen finden Sie unter [Erstellen von Azure Resource Manager-Vorlagen](../azure-resource-manager/resource-group-authoring-templates.md).

Legen Sie in der Vorlage das Kapazitätselement fest:

    "sku": {
      "name": "Standard_A0",
      "tier": "Standard",
      "capacity": 3
    },

Die Kapazität gibt die Anzahl von virtuellen Computern in der Gruppe an. Sie können die Kapazität manuell ändern, indem Sie eine Vorlage mit einem anderen Wert bereitstellen. Wenn Sie eine Vorlage bereitstellen, um lediglich die Kapazität zu ändern, müssen Sie nur das SKU-Element mit der aktualisierten Kapazität einschließen.

Wenn die Kapazität der Skalierungsgruppe automatisch geändert werden soll, verwenden Sie eine Kombination aus der Ressource „autoscaleSettings“ und der Diagnoseerweiterung.

### <a name="configure-the-azure-diagnostics-extension"></a>Konfigurieren der Azure-Diagnoseerweiterung
Die automatische Skalierung ist nur möglich, wenn die Metrikauflistung auf jedem virtuellen Computer in der Skalierungsgruppe erfolgreich ist. Die Azure-Diagnoseerweiterung bietet Überwachungs- und Diagnosefunktionen, die die Anforderungen der Ressource für die automatische Skalierung in Bezug auf die Metrikauflistung erfüllen. Sie können die Erweiterung als Teil der Resource Manager-Vorlage installieren.

Dieses Beispiel zeigt die Variablen, die in der Vorlage zum Konfigurieren der Diagnoseerweiterung verwendet werden:

    "diagnosticsStorageAccountName": "[concat(parameters('resourcePrefix'), 'saa')]",
    "accountid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/', 'Microsoft.Storage/storageAccounts/', variables('diagnosticsStorageAccountName'))]",
    "wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
    "wadperfcounter": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\Thread Count\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Thread Count\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
    "wadcfgxstart": "[concat(variables('wadlogs'),variables('wadperfcounter'),'<Metrics resourceId=\"')]",
    "wadmetricsresourceid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name ,'/providers/','Microsoft.Compute/virtualMachineScaleSets/',parameters('vmssName'))]",
    "wadcfgxend": "[concat('\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>')]"

Parameter werden beim Bereitstellen der Vorlage angegeben. In diesem Beispiel werden der Name des Speicherkontos, in dem Daten gespeichert werden, und der Name der Skalierungsgruppe, in der Daten erfasst werden, angegeben. Außerdem wird in diesem Windows Server-Beispiel nur der Leistungsindikator für die Threadanzahl erfasst. Zum Erfassen von Diagnoseinformationen können alle in Windows oder Linux verfügbaren Leistungsindikatoren verwendet und in die Erweiterungskonfiguration einbezogen werden.

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

Wenn die Diagnoseerweiterung ausgeführt wird, werden die Daten in einer Tabelle erfasst, die sich im von Ihnen festgelegten Speicherkonto befindet. Die erfassten Daten befinden sich in der Tabelle „WADPerformanceCounters“.

![](./media/virtual-machine-scale-sets-autoscale-overview/ThreadCountBefore2.png)

### <a name="configure-the-autoscalesettings-resource"></a>Konfigurieren der Ressource „autoScaleSettings“
Die Ressource „autoscaleSettings“ verwendet die Informationen aus der Diagnoseerweiterung, um zu entscheiden, ob die Anzahl virtueller Computer in der Skalierungsgruppe erhöht oder verringert werden muss.

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

Im obigen Beispiel werden zwei Regeln erstellt, um Aktionen für die automatische Skalierung zu definieren: Die erste Regel definiert das horizontale Hochskalieren, die zweite das horizontale Herunterskalieren. Diese Werte werden in den Regeln bereitgestellt:

* **metricName** : Der Leistungsindikator, den wir in der Variablen „wadperfcounter“ für die Diagnoseerweiterung definiert haben. Im obigen Beispiel wird der Zähler für die Threadanzahl verwendet.  
* **metricResourceUri** : Der Ressourcenbezeichner der VM-Skalierungsgruppe. Dieser Bezeichner enthält den Namen der Ressourcengruppe, den Namen des Ressourcenanbieters und den Namen der zu skalierenden Skalierungsgruppe.
* **timeGrain** : Die Granularität der erfassten Metriken. Im vorherigen Beispiel werden Daten in einem Intervall von einer Minute erfasst. Dieser Wert wird zusammen mit „timeWindow“ verwendet.
* **statistic** : Bestimmt, wie die Metriken für die Durchführung der automatischen Skalierungsaktion kombiniert werden sollen. Mögliche Werte sind: Average, Min, Max.
* **timeWindow** : Der Zeitbereich, in dem Instanzdaten gesammelt werden. Der Wert muss zwischen fünf Minuten und zwölf Stunden liegen.
* **timeAggregation** : Bestimmt, wie die gesammelten Daten im Laufe der Zeit kombiniert werden sollen. Der Standardwert ist "Average". Mögliche Werte sind: Average, Minimum, Maximum, Last, Total, Count.
* **operator** : Der Operator, der zum Vergleichen der Metrikdaten und des Schwellenwerts verwendet wird. Mögliche Werte sind: Equals, NotEquals, GreaterThan, GreaterThanOrEqual, LessThan, LessThanOrEqual.
* **threshold** : Der Wert, der die Skalierungsaktion auslöst. Achten Sie auf eine ausreichende Differenz zwischen den Schwellenwerten für das horizontale Hochskalieren und für das horizontale Herunterskalieren. Ist beides auf den gleichen Wert festgelegt, erwartet das System kontinuierlich eine Veränderung und kann daher keine Skalierungsaktion implementieren. Wenn Sie also beispielsweise beide Werte wie im vorherigen Beispiel auf 600 Threads festlegen, funktioniert die Skalierung nicht.
* **direction** : Bestimmt, welche Aktion bei Erreichen des Schwellenwerts ausgeführt werden soll. Mögliche Werte sind „Increase“ oder „Decrease“.
* **type**: Die Art der auszuführenden Aktion. Dieser Wert muss auf „ChangeCount“ festgelegt werden.
* **value**: Die Anzahl virtueller Computer, die der Skalierungsgruppe hinzugefügt bzw. daraus entfernt werden. Dieser Wert muss 1 oder höher lauten.
* **cooldown** : Gibt an, wie lange zwischen der letzten Skalierungsaktion und der nächsten Aktion gewartet werden soll. Dieser Wert muss zwischen einer Minute und einer Woche liegen.

Je nach dem verwendeten Leistungsindikator werden einige der Elemente in der Vorlagenkonfiguration unterschiedlich verwendet. Im vorherigen Beispiel wird die Threadanzahl als Leistungsindikator verwendet, und der Schwellenwert wird auf 650 (horizontales Hochskalieren) bzw. auf 550 (horizontales Herunterskalieren) festgelegt. Wenn Sie einen Indikator wie „Prozessorzeit (%)“ verwenden, wird der Wert auf den Prozentsatz der CPU-Auslastung festgelegt, der eine Skalierungsaktion bedingt.

Wenn auf den virtuellen Computern eine Last generiert wird, die eine Skalierungsaktion zur Folge hat, wird die Kapazität der Gruppe auf der Grundlage des Werts in der Vorlage erhöht. Dieses Beispiel zeigt eine Skalierungsgruppe, in der die Kapazität auf 3 und der Wert für die Skalierungsaktion auf 1 festgelegt ist:

![](./media/virtual-machine-scale-sets-autoscale-overview/ResourceExplorerBefore.png)

Bei einer Last, bei der die durchschnittliche Threadanzahl den Schwellenwert von 650 übersteigt:

![](./media/virtual-machine-scale-sets-autoscale-overview/ThreadCountAfter.png)

Eine Aktion für horizontales Hochskalieren wird ausgelöst, wodurch sich die Kapazität der Gruppe um eins erhöht:

    "sku": {
      "name": "Standard_A0",
      "tier": "Standard",
      "capacity": 4
    },

Der Skalierungsgruppe wird ein virtueller Computer hinzugefügt:

![](./media/virtual-machine-scale-sets-autoscale-overview/ResourceExplorerAfter.png)

Wenn die durchschnittliche Threadanzahl nach einer Abkühlperiode von fünf Minuten noch immer größer ist als 600, wird der Gruppe ein weiterer Computer hinzugefügt. Wenn die durchschnittliche Threadanzahl kleiner als 550 bleibt, wird die Kapazität der Skalierungsgruppe um den Wert 1 verringert, und ein Computer wird aus der Gruppe entfernt.

## <a name="set-up-scaling-using-azure-powershell"></a>Einrichten der Skalierung mithilfe von Azure PowerShell
Beispiele für die Einrichtung der automatischen Skalierung mithilfe von PowerShell finden Sie unter [Azure Monitor – PowerShell-Schnellstartbeispiele](../monitoring-and-diagnostics/insights-powershell-samples.md).

## <a name="set-up-scaling-using-azure-cli"></a>Einrichten der Skalierung mithilfe der Azure-Befehlszeilenschnittstelle
Beispiele für die Einrichtung der automatischen Skalierung mithilfe der Azure-Befehlszeilenschnittstelle finden Sie unter [Azure Monitor – Schnellstartbeispiele für plattformübergreifende Befehlszeilenschnittstelle](../monitoring-and-diagnostics/insights-cli-samples.md).

## <a name="set-up-scaling-using-the-azure-portal"></a>Einrichten der Skalierung im Azure-Portal
Ein Beispiel der Einrichtung der automatischen Skalierung im Azure-Portal finden Sie unter [Erstellen einer VM-Skalierungsgruppe mit dem Azure-Portal](virtual-machine-scale-sets-portal-create.md).

## <a name="investigate-scaling-actions"></a>Untersuchen von Skalierungsaktionen
* Azure-Portal: Das Portal liefert aktuell eine begrenzte Menge an Informationen.
* Azure-Ressourcen-Explorer: Dieses Tool eignet sich am besten zum Untersuchen des aktuellen Zustands Ihrer Skalierungsgruppe. Über den folgenden Pfad gelangen Sie zur Instanzansicht für die von Ihnen erstellte Skalierungsgruppe: Abonnements > {Ihr Abonnement} > resourceGroups > {Ihre Ressourcengruppe} > Anbieter > Microsoft.Compute > virtualMachineScaleSets > {Ihre Skalierungsgruppe} > virtualMachines
* Azure PowerShell: Verwenden Sie den folgenden Befehl, um Informationen zu erhalten:
  
        Get-AzureRmResource -name vmsstest1 -ResourceGroupName vmsstestrg1 -ResourceType Microsoft.Compute/virtualMachineScaleSets -ApiVersion 2015-06-15
        Get-Autoscalesetting -ResourceGroup rainvmss -DetailedOutput
* Stellen Sie eine Verbindung mit dem virtuellen Jumpbox-Computer her, wie Sie dies auch für jeden anderen Computer tun würden. Sie können dann per Remoteverbindung auf die virtuellen Computer der Skalierungsgruppe zugreifen, um die einzelnen Prozesse zu überwachen.

## <a name="next-steps"></a>Nächste Schritte
* Unter [Automatisches Skalieren von Computern in einer VM-Skalierungsgruppe](virtual-machine-scale-sets-windows-autoscale.md) finden Sie ein Beispiel für die Erstellung einer Skalierungsgruppe mit automatischer Skalierung.
* Beispiele für Überwachungsfeatures von Azure Monitor finden Sie unter [Azure Monitor – PowerShell-Schnellstartbeispiele](../monitoring-and-diagnostics/insights-powershell-samples.md)
* Informationen zu Benachrichtigungsfeatures finden Sie unter [Verwenden von automatischen Skalierungsvorgängen zum Senden von E-Mail- und Webhook-Warnbenachrichtigungen in Azure Monitor](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md).
* Informationen zur Verwendung von Überwachungsprotokollen zum Senden von E-Mail- und Webhook-Warnbenachrichtigungen in Azure Monitor finden Sie [hier](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md).
* Erfahren Sie mehr über [erweiterte Szenarien für die automatische Skalierung](virtual-machine-scale-sets-advanced-autoscale.md).




<!--HONumber=Jan17_HO2-->


