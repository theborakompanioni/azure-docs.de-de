---
title: Verwenden der Azure-Autoskalierung mit Gastmetriken in einer Linux-Skalierungsgruppenvorlage | Microsoft-Dokumentationen
description: "Erfahren Sie, wie Sie die Autoskalierung mit Gastmetriken in einer Linux-Vorlage für virtuelle Computer in einer VM-Skalierungsgruppe verwenden können"
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: na
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: negat
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: ac0bbb4dbfccca3f3fc31526aeff11afe55d44be
ms.contentlocale: de-de
ms.lasthandoff: 08/21/2017

---

# <a name="autoscale-using-guest-metrics-in-a-linux-scale-set-template"></a>Autoskalierung mit Gastmetriken in einer Linux-Skalierungsgruppenvorlage

Es gibt in Azure zwei Metriktypen, die aus virtuellen Computern und Skalierungsgruppen gesammelt werden: Einige stammen aus dem virtuellen Hostcomputer, andere aus dem virtuellen Gastcomputer. Hostmetriken setzen keine zusätzlichen Einstellungen voraus, weil sie vom virtuellen Hostcomputer gesammelt werden, während für Gastmetriken die [Windows Azure Diagnostics-Erweiterung](../virtual-machines/windows/extensions-diagnostics-template.md) oder die [Linux Azure Diagnostics-Erweiterung](../virtual-machines/linux/diagnostic-extension.md) im virtuellen Gastcomputer installiert werden muss. Ein häufiger Grund für die Verwendung von Gastmetriken anstatt von Hostmetriken ist, dass Gastmetriken eine größere Auswahl an Metriken bieten als Hostmetriken. Ein Beispiel dafür sind Speicherverbrauchsmetriken, die nur über Gastmetriken zur Verfügung stehen. Die unterstützten Hostmetriken finden Sie [hier](../monitoring-and-diagnostics/monitoring-supported-metrics.md), und häufig verwendete Gastmetriken finden Sie [hier](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md). In diesem Artikel wird gezeigt, wie Sie die [Vorlage für die kleinstmögliche Skalierungsgruppe](./virtual-machine-scale-sets-mvss-start.md) verändern müssen, um bei Linux-Skalierungsgruppen auf Gastmetriken basierende Autoskalierungsregeln verwenden zu können.

## <a name="change-the-template-definition"></a>Ändern der Vorlagendefinition

Unsere Vorlage für die kleinstmögliche Skalierungsgruppe finden Sie [hier](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json), und unsere Vorlage für die Bereitstellung der Linux-Skalierungsgruppe mit gastbasierter Autoskalierung finden Sie [hier](https://raw.githubusercontent.com/gatneil/mvss/guest-based-autoscale-linux/azuredeploy.json). Sehen wir uns die Diff zum Erstellen dieser Vorlage (`git diff minimum-viable-scale-set existing-vnet`) Stück für Stück an:

Zunächst fügen wir Parameter für `storageAccountName` und `storageAccountSasToken` hinzu. Der Diagnose-Agent speichert Metrikdaten in einer [Tabelle](../cosmos-db/table-storage-how-to-use-dotnet.md) in diesem Speicherkonto. Ab Version 3.0 des Linux-Diagnose-Agents wird der Speicherzugriffsschlüssel nicht mehr unterstützt. Daher müssen wir ein [SAS-Token](../storage/common/storage-dotnet-shared-access-signature-part-1.md) verwenden.

```diff
     },
     "adminPassword": {
       "type": "securestring"
+    },
+    "storageAccountName": {
+      "type": "string"
+    },
+    "storageAccountSasToken": {
+      "type": "securestring"
     }
   },
```

Im nächsten Schritt fügen wir der Skalierungsgruppe `extensionProfile` die Diagnoseerweiterung hinzu. Bei dieser Konfiguration geben wir die Ressourcen-ID der Skalierungsgruppe an, von der Metriken gesammelt werden sollen. Weiterhin geben wir das Speicherkonto und das SAS-Token an, die zum Speichern der Metriken verwendet werden sollen. Ferner geben wir an, wie häufig die Metriken aggregiert werden (hier minütlich) und welche Metriken verfolgt werden sollen (hier Prozent des verbrauchten Speichers). Weitere Informationen über diese Konfiguration und andere Metriken als den Prozentsatz des verbrauchten Speichers finden Sie in [dieser Dokumentation](../virtual-machines/linux/diagnostic-extension.md).

```diff
                 }
               }
             ]
+          },
+          "extensionProfile": {
+            "extensions": [
+              {
+                "name": "LinuxDiagnosticExtension",
+                "properties": {
+                  "publisher": "Microsoft.Azure.Diagnostics",
+                  "type": "LinuxDiagnostic",
+                  "typeHandlerVersion": "3.0",
+                  "settings": {
+                    "StorageAccount": "[parameters('storageAccountName')]",
+                    "ladCfg": {
+                      "diagnosticMonitorConfiguration": {
+                        "performanceCounters": {
+                          "sinks": "WADMetricJsonBlob",
+                          "performanceCounterConfiguration": [
+                            {
+                              "unit": "percent",
+                              "type": "builtin",
+                              "class": "memory",
+                              "counter": "percentUsedMemory",
+                              "counterSpecifier": "/builtin/memory/percentUsedMemory",
+                              "condition": "IsAggregate=TRUE"
+                            }
+                          ]
+                        },
+                        "metrics": {
+                          "metricAggregation": [
+                            {
+                              "scheduledTransferPeriod": "PT1M"
+                            }
+                          ],
+                          "resourceId": "[resourceId('Microsoft.Compute/virtualMachineScaleSets', 'myScaleSet')]"
+                        }
+                      }
+                    }
+                  },
+                  "protectedSettings": {
+                    "storageAccountName": "[parameters('storageAccountName')]",
+                    "storageAccountSasToken": "[parameters('storageAccountSasToken')]",
+                    "sinksConfig": {
+                      "sink": [
+                        {
+                          "name": "WADMetricJsonBlob",
+                          "type": "JsonBlob"
+                        }
+                      ]
+                    }
+                  }
+                }
+              }
+            ]
           }
         }
       }
```

Schließlich fügen wir eine `autoscaleSettings`-Ressource hinzu, damit wir basierend auf diesen Metriken die Autoskalierung konfigurieren können. Diese Ressource verfügt über eine `dependsOn`-Klausel, die auf die Skalierungsgruppe verweist. So wird vor dem Autoskalierungsversuch sichergestellt, dass die Skalierungsgruppe existiert. Wenn wir eine andere Metrik für die Autoskalierung auswählen, würden wir den `counterSpecifier` aus der Konfiguration der Diagnoseerweiterung als `metricName` in der Autoskalierungskonfiguration verwenden. Weitere Informationen über das Konfigurieren der Autoskalierung finden Sie in [Bewährte Methoden für die automatische Skalierung](..//monitoring-and-diagnostics/insights-autoscale-best-practices.md) und der Referenzdokumentation der [Azure Monitor REST-API](https://msdn.microsoft.com/library/azure/dn931928.aspx).

```diff
+    },
+    {
+      "type": "Microsoft.Insights/autoscaleSettings",
+      "apiVersion": "2015-04-01",
+      "name": "guestMetricsAutoscale",
+      "location": "[resourceGroup().location]",
+      "dependsOn": [
+        "Microsoft.Compute/virtualMachineScaleSets/myScaleSet"
+      ],
+      "properties": {
+        "name": "guestMetricsAutoscale",
+        "targetResourceUri": "[resourceId('Microsoft.Compute/virtualMachineScaleSets', 'myScaleSet')]",
+        "enabled": true,
+        "profiles": [
+          {
+            "name": "Profile1",
+            "capacity": {
+              "minimum": "1",
+              "maximum": "10",
+              "default": "3"
+            },
+            "rules": [
+              {
+                "metricTrigger": {
+                  "metricName": "/builtin/memory/percentUsedMemory",
+                  "metricNamespace": "",
+                  "metricResourceUri": "[resourceId('Microsoft.Compute/virtualMachineScaleSets', 'myScaleSet')]",
+                  "timeGrain": "PT1M",
+                  "statistic": "Average",
+                  "timeWindow": "PT5M",
+                  "timeAggregation": "Average",
+                  "operator": "GreaterThan",
+                  "threshold": 60
+                },
+                "scaleAction": {
+                  "direction": "Increase",
+                  "type": "ChangeCount",
+                  "value": "1",
+                  "cooldown": "PT1M"
+                }
+              },
+              {
+                "metricTrigger": {
+                  "metricName": "/builtin/memory/percentUsedMemory",
+                  "metricNamespace": "",
+                  "metricResourceUri": "[resourceId('Microsoft.Compute/virtualMachineScaleSets', 'myScaleSet')]",
+                  "timeGrain": "PT1M",
+                  "statistic": "Average",
+                  "timeWindow": "PT5M",
+                  "timeAggregation": "Average",
+                  "operator": "LessThan",
+                  "threshold": 30
+                },
+                "scaleAction": {
+                  "direction": "Decrease",
+                  "type": "ChangeCount",
+                  "value": "1",
+                  "cooldown": "PT1M"
+                }
+              }
+            ]
+          }
+        ]
+      }
     }
   ]
 }
```





## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]

