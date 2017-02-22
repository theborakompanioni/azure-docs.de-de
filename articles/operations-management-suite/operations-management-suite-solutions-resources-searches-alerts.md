---
title: "Gespeicherte Suchen und Warnungen in OMS-Lösungen | Microsoft-Dokumentation"
description: "Lösungen in OMS enthalten in der Regel gespeicherte Suchen in Log Analytics zum Analysieren der von der Lösung erfassten Daten.  Sie können auch Warnungen zur Benachrichtigung des Benutzers definieren oder als Reaktion auf ein schwerwiegendes Problem automatisch Maßnahmen ergreifen.  Dieser Artikel beschreibt das Definieren von in Log Analytics gespeicherten Suchen und Warnungen in einer ARM-Vorlage, damit sie in Verwaltungslösungen aufgenommen werden können."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/24/2017
ms.author: bwren
translationtype: Human Translation
ms.sourcegitcommit: aaf5c442ef85edbc498aa2fd7815171f4701f960
ms.openlocfilehash: 262beba30c760335aafdf903d9f5cac6b0dd9669

---

# <a name="log-analytics-saved-searches-and-alerts-in-oms-solutions-preview"></a>In Log Analytics gespeicherte Suchen und Warnungen in OMS-Lösungen (Vorschau)

> [!NOTE]
> Dies ist die vorläufige Dokumentation für das Erstellen von Verwaltungslösungen in der OMS, die sich derzeit in der Vorschau befinden. Jedes unten beschriebene Schema kann sich ändern.   


[Verwaltungslösungen in OMS](operations-management-suite-solutions.md) enthalten in der Regel [gespeicherte Suchen](../log-analytics/log-analytics-log-searches.md) in Log Analytics zum Analysieren der von der Lösung erfassten Daten.  Sie können auch [Warnungen](../log-analytics/log-analytics-alerts.md) zur Benachrichtigung des Benutzers definieren oder als Reaktion auf ein schwerwiegendes Problem automatisch Maßnahmen ergreifen.  Dieser Artikel beschreibt das Definieren von in Log Analytics gespeicherten Suchen und Warnungen in einer [Ressourcenverwaltungsvorlage](../resource-manager-template-walkthrough.md), damit sie in [Verwaltungslösungen](operations-management-suite-solutions-creating.md) aufgenommen werden können.

> [!NOTE]
> Die Beispiele in diesem Artikel verwenden Parameter und Variablen, die entweder erforderlich sind oder für Verwaltungslösungen gelten und unter [Creating management solutions in Operations Management Suite (OMS) (Erstellen von Verwaltungslösungen in der Operations Management Suite (OMS))](operations-management-suite-solutions-creating.md) beschrieben sind.  

## <a name="prerequisites"></a>Voraussetzungen
In diesem Artikel wird davon ausgegangen, dass Sie schon mit der [Erstellung einer Verwaltungslösung](operations-management-suite-solutions-creating.md) und der Struktur einer [ARM-Vorlage](../resource-group-authoring-templates.md) und Lösungsdatei vertraut sind.


## <a name="log-analytics-workspace"></a>Log Analytics-Arbeitsbereich
Alle Ressourcen in Log Analytics befinden sich in einem [Arbeitsbereich](../log-analytics/log-analytics-manage-access.md).  Wie unter [OMS-Arbeitsbereich und Automation-Konto](operations-management-suite-solutions-creating.md#oms-workspace-and-automation-account) beschrieben, ist der Arbeitsbereich nicht in der Verwaltungslösung enthalten, muss aber vor der Installation der Lösung vorhanden sein.  Ist es nicht verfügbar, schlägt die Installation der Lösung fehl.

Der Name des Arbeitsbereichs ist im Namen jeder Log Analytics-Ressource enthalten.  Dafür sorgt wie in diesem Beispiel einer savedsearch-Ressource der Parameter **Arbeitsbereich** in der Lösung.

    "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearchId'))]"


## <a name="saved-searches"></a>Gespeicherte Suchen
Schließen Sie [gespeicherte Suchen](../log-analytics/log-analytics-log-searches.md) in eine Lösung ein, um Benutzern das Abfragen der von der Lösung erfassten Daten zu ermöglichen.  Gespeicherte Suchen werden im OMS-Portal unter **Favoriten** und im Azure-Portal unter **Gespeicherte Suchen** angezeigt.  Eine gespeicherte Suche ist zudem für jede Warnung erforderlich.   

Ressourcen für [Gespeicherte Suchen in Log Analytics](../log-analytics/log-analytics-log-searches.md) weisen den Typ `Microsoft.OperationalInsights/workspaces/savedSearches` und folgende Struktur auf. 

    {
        "name": "<name-of-savedsearch>"
        "type": "Microsoft.OperationalInsights/workspaces/savedSearches",
        "apiVersion": "<api-version-of-resource>",
        "dependsOn": []
        "tags": {},
        "properties": {
            "etag": "*",
            "query": "<query-to-run>",
            "displayName": "<saved-search-display-name>",
            "category": ""<saved-search-category>"
        }
    }

Die Eigenschaften einer gespeicherten Suche sind in der folgenden Tabelle beschrieben: 

| Eigenschaft | Beschreibung |
|:--- |:--- |
| category | Die Kategorie für die gespeicherte Suche.  Alle gespeicherten Suchen in derselben Lösung verwenden häufig gemeinsam eine einzige Kategorie, sodass sie gemeinsam in der Konsole gruppiert werden. |
| displayname | Name, der für die gespeicherte Suche im Portal angezeigt wird |
| query | Die auszuführende Abfrage. |

> [!NOTE]
> Sie müssen möglicherweise Escape-Zeichen in der Abfrage verwenden, wenn diese Zeichen enthält, die als JSON interpretiert werden könnten.  Falls Ihre Abfrage **Typ:AzureActivity OperationName:"Microsoft.Compute/virtualMachines/write"** lautete, sollte in der Lösungsdatei Folgendes stehen: **Typ:AzureActivity OperationName:\"Microsoft.Compute/virtualMachines/write\"**.

## <a name="alerts"></a>Warnungen
[Log Analytics-Warnungen](../log-analytics/log-analytics-alerts.md) werden anhand von Warnungsregeln erstellt, die in regelmäßigen Abständen eine gespeicherte Suche ausführen.  Wenn die Ergebnisse der Abfrage mit den angegebenen Kriterien übereinstimmen, wird eine Warnung erstellt und mindestens eine Aktion ausgeführt.  

Warnungsregeln in einer Verwaltungslösung bestehen aus den folgenden drei verschiedenen Ressourcen:

- **Gespeicherte Suche.**  Definiert die Protokollsuche, die ausgeführt wird.  Mehrere Warnungsregeln können gemeinsam eine einzelne gespeicherte Suche verwenden.
- **Zeitplan.**  Definiert, wie oft die Protokollsuche ausgeführt wird.  Jede Warnungsregel weist nur einen einzigen Zeitplan auf.
- **Warnungsaktion.**  Jede Warnregel weist eine Aktionsressource mit dem Typ **Warnung** auf, die die Details der Warnung, etwa die Kriterien dafür, wann ein Warnungsdatensatz erstellt wird, und den Schweregrad der Warnung definiert.  Die Aktionsressource definiert optional eine Antwort per E-Mail und Runbook.
- **Webhook-Aktion (optional).**  Wenn die Warnungsregel einen Webhook aufruft, ist eine zusätzliche Aktionsressource vom Typ **Webhook** erforderlich.    

Gespeicherte Suchressourcen sind oben beschrieben.  Die anderen Ressourcen sind nachfolgend beschrieben.


### <a name="schedule-resource"></a>Zeitplanressource

Eine gespeicherte Suche kann einen oder mehrere Zeitpläne aufweisen, wobei jeder Zeitplan eine separate Warnungsregel darstellt. Der Zeitplan definiert, wie oft die Suche durchgeführt wird und welches Zeitintervall für das Abrufen der Daten verwendet wird.  Zeitplanressourcen weisen den Typ `Microsoft.OperationalInsights/workspaces/savedSearches/schedules/` und die folgende Struktur auf. 

    {
      "name": "<name-of-schedule>",
      "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/",
      "apiVersion": "<api-version-of-resource>",
      "dependsOn": [
        "<name-of-saved-search>"
      ],
      "properties": {  
        "etag": "*",               
        "interval": <schedule-interval-in-minutes>,
        "queryTimeSpan": <query-timespan-in-minutes>,
        "enabled": <schedule-interval-in-minutes>       
      }
    }

Die Eigenschaften für Zeitplanressourcen werden in der folgenden Tabelle beschrieben.

| Elementname | Erforderlich | Beschreibung |
|:--|:--|:--|
| Aktiviert       | Ja | Gibt an, ob die Warnung beim Erstellen aktiviert wird. |
| interval      | Ja | Abfrageintervall in Minuten |
| queryTimeSpan | Ja | Länge in Minuten für die Auswertung der Ergebnisse |

Die Zeitplanressource muss von der gespeicherten Suche abhängig sein, damit sie vor dem Zeitplan erstellt wird.


### <a name="actions"></a>Actions
Es gibt zwei Arten von Aktionsressourcen, die von der **Typ**-Eigenschaft festgelegt werden.  Für einen Zeitplan ist eine Aktion vom Typ **Warnung** erforderlich, die die Details der Warnungsregel und die Aktionen definiert, die beim Erstellen einer Warnung ausgeführt werden.  Er kann auch eine **Webhook**-Aktion enthalten, wenn ein Webhook über die Warnung aufgerufen werden soll.  

Aktionsressourcen weisen den Typ `Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions` auf.  

#### <a name="alert-actions"></a>Warnungsaktionen

Jeder Zeitplan weist eine Aktion vom Typ **Warnung** auf.  Dadurch werden die Details der Warnung und optional die Aktionen zur Benachrichtigung und Wartung definiert.  Eine Benachrichtigung sendet eine E-Mail an mindestens eine Adresse.  Eine Wartung startet ein Runbook in Azure Automation und versucht, das erkannte Problem zu beheben.

Warnungsaktionen weisen die folgende Struktur auf:

    {
        "name": "<name-of-the-action>",
        "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions",
        "apiVersion": "<api-version-of-resource>",
        "dependsOn": [
            <name-of-schedule>
        ],
        "properties": {
            "etag": "*",
            "type": "Alert",
            "name": "<display-name-of-alert>",
            "description": "<description-of-alert>",
            "severity": "<severity-of-alert>",
            "threshold": {
                "operator": "<threshold-operator>",
                "value": "<threshold-value>"
                "metricsTrigger": {
                    "triggerCondition": "<trigger-condition>",
                    "operator": "<trigger-operator>",
                    "value": "<trigger-value>"
                },
            },
            "throttling": {
                "durationInMinutes": "<throttling-duration-in-minutes>"
            },
            "emailNotification": {
                "recipients": [
                    <mail-recipients>
                ],
                "subject": "<mail-subject>",
                "attachment": "None"
            },
            "remediation": {
                "runbookName": "<name-of-runbook>",
                "webhookUri": "<runbook-uri>"
            }
        }
    }

Die Eigenschaften für Warnungsaktionsressourcen werden in den folgenden Tabellen beschrieben:

| Elementname | Erforderlich | Beschreibung |
|:--|:--|:--|
| Typ | Ja | Der Typ der Aktion.  Dieser lautet bei Warnungsaktionen **Warnung**. |
| Name | Ja | Der Anzeigename für die Warnung.  Dies ist der Name, der in der Konsole für die Warnungsregel angezeigt wird. |
| Beschreibung | Nein | Eine optionale Beschreibung der Warnung |
| Schweregrad | Ja | Schweregrad des Warnungsdatensatzes aus den folgenden Werten:<br><br> **Critical** (Kritisch)<br>**Warning**<br>**Informational** (Nur zu Informationszwecken) |


##### <a name="threshold"></a>Schwellenwert
Dieser Abschnitt ist ein Pflichtabschnitt.  Er definiert die Eigenschaften für den Warnungsschwellenwert.

| Elementname | Erforderlich | Beschreibung |
|:--|:--|:--|
| Operator | Ja | Operator für den Vergleich der folgenden Werte:<br><br>**gt = Greater Than (Größer als)<br>lt = Less Than (Kleiner als)** |
| Wert | Ja | Der Wert zum Vergleich der Ergebnisse |


##### <a name="metricstrigger"></a>MetricsTrigger
Dieser Abschnitt ist optional.  Fügen Sie ihn für eine Warnung aufgrund metrischer Messungen ein.

> [!NOTE]
> Warnungen aufgrund metrischer Messungen befinden sich derzeit in der öffentlichen Vorschau. 

| Elementname | Erforderlich | Beschreibung |
|:--|:--|:--|
| TriggerCondition | Ja | Gibt an, ob der Schwellenwert für die Gesamtanzahl der Verstöße oder für aufeinander folgende Verstöße gegen folgende Werte steht:<br><br>**Insgesamt<br>Aufeinander folgende** |
| Operator | Ja | Operator für den Vergleich der folgenden Werte:<br><br>**gt = Greater Than (Größer als)<br>lt = Less Than (Kleiner als)** |
| Wert | Ja | Häufigkeit, mit der die Kriterien erfüllt werden müssen, um die Warnung auszulösen. |

##### <a name="throttling"></a>Drosselung
Dieser Abschnitt ist optional.  Beziehen Sie diesen Abschnitt mit ein, wenn nach dem Erstellen einer Warnung Warnungen von derselben Regel eine bestimmte Zeit lang unterdrückt werden sollen.

| Elementname | Erforderlich | Beschreibung |
|:--|:--|:--|
| DurationInMinutes | Ja, wenn das Drosselungselement enthalten ist | Dauer der Unterdrückung von Warnungen in Minuten, wenn eine Warnung aufgrund derselben Regel erstellt wird. |

##### <a name="emailnotification"></a>EmailNotification
 Dieser Abschnitt ist optional. Schließen Sie ihn ein, wenn bei der Warnung eine E-Mail an mindestens einen Empfänger gesendet werden soll.

| Elementname | Erforderlich | Beschreibung |
|:--|:--|:--|
| Empfänger | Ja | Durch Kommas getrennte Liste der E-Mail-Adressen, an die eine Benachrichtigung gesendet wird, wenn eine Warnung wie im folgenden Beispiel erstellt wird:<br><br>**[ "recipient1@contoso.com", "recipient2@contoso.com" ]** |
| Betreff | Ja | Die Betreffzeile der E-Mail |
| Anhang | Nein | Anlagen werden derzeit nicht unterstützt.  Wenn dieses Element enthalten ist, muss es **Keine** lauten. |


##### <a name="remediation"></a>Wiederherstellung
Dieser Abschnitt ist optional. Schließen Sie ihn ein, wenn als Reaktion auf die Warnung ein Runbook gestartet werden soll. |

| Elementname | Erforderlich | Beschreibung |
|:--|:--|:--|
| RunbookName | Ja | Der Name des Runbooks, das gestartet werden soll |
| WebhookUri | Ja | Der URI des Webhooks für das Runbook |
| Expiry | Nein | Datum und Uhrzeit des Ablaufs der Wiederherstellung |

#### <a name="webhook-actions"></a>Webhookaktionen

Bei Webhookaktionen wird ein Prozess gestartet, indem eine URL aufgerufen und optional eine zu sendende Nutzlast bereitgestellt wird. Diese Aktionen sind mit Aktionen zur Problembehebung vergleichbar. Sie sind aber für Webhooks bestimmt, mit denen andere Prozesse als Azure Automation-Runbooks aufgerufen werden können. Außerdem verfügen sie über die zusätzliche Option zum Angeben einer Nutzlast, die für den Remoteprozess bereitgestellt wird.

Wenn die Warnung einen Webhook aufruft, benötigt sie eine Aktionsressource mit einem **Webhook**-Typ zusätzlich zur Aktionsressource vom Typ **Warnung**.  

    {
        "name": "<name-of-the-action>",
        "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions",
        "apiVersion": "<api-version-of-resource>",
        "dependsOn": [
            <name-of-schedule>
            <name-of-alert-action>
        ],
        "properties": {
            "etag": "*",
            "type": "Webhook",
            "name": "<display-name-of-action>",
            "severity": "<severity-of-alert>",
            "customPayload": "<payload-to-send>"
        }
    }

Die Eigenschaften für Warnungsaktionsressourcen werden in den folgenden Tabellen beschrieben:

| Elementname | Erforderlich | Beschreibung |
|:--|:--|:--|
| Typ | Ja | Der Typ der Aktion.  Dieser lautet für Webhookaktionen **Webhook**. |
| Name | Ja | Der Anzeigename für die Aktion.  Dieser wird nicht in der Konsole angezeigt. |
| wehookUri | Ja | URI für den Webhook |
| customPayload | Nein | Benutzerdefinierte Nutzlast, die an den Webhook gesendet wird. Das Format hängt davon ab, was vom Webhook erwartet wird. |




### <a name="sample"></a>Beispiel

Es folgt ein Beispiel für eine Lösung, die die folgenden Ressourcen enthält:

- Gespeicherte Suche
- Schedule
- Warnungsaktion
- Webhookaktion

Das Beispiel verwendet Variablen für [Standardlösungsparameter](operations-management-suite-solutions-creating.md#parameters), die im Gegensatz zu hartcodierten Werten in Ressourcendefinitionen häufig in einer Lösung verwendet werden.

    {
        "$schema": "http://schemas.microsoft.org/azure/deploymentTemplate?api-version=2015-01-01#",
        "contentVersion": "1.0",
        "parameters": {
          "workspaceName": {
            "type": "string",
            "metadata": {
              "Description": "Name of Log Analytics workspace"
            }
          },
          "accountName": {
            "type": "string",
            "metadata": {
              "Description": "Name of Automation account"
            }
          },
          "workspaceregionId": {
            "type": "string",
            "metadata": {
              "Description": "Region of Log Analytics workspace"
            }
          },
          "regionId": {
            "type": "string",
            "metadata": {
              "Description": "Region of Automation account"
            }
          },
          "pricingTier": {
            "type": "string",
            "metadata": {
              "Description": "Pricing tier of both Log Analytics workspace and Azure Automation account"
            }
          },
          "recipients": {
            "type": "string",
            "metadata": {
              "Description": "List of recipients for the email alert separated by semicolon"
            }
          }
        },
        "variables": {
          "SolutionName": "MySolution",
          "SolutionVersion": "1.0",
          "SolutionPublisher": "Contoso",
          "ProductName": "SampleSolution",
    
          "LogAnalyticsApiVersion": "2015-11-01-preview",
    
          "MySearch": {
            "displayName": "Error records by hour",
            "query": "Type=MyRecord_CL | measure avg(Rating_d) by Instance_s interval 60minutes",
            "category": "Samples",
            "name": "Samples-Count of data"
          },
          "MyAlert": {
            "Name": "[toLower(concat('myalert-',uniqueString(resourceGroup().id, deployment().name)))]",
            "DisplayName": "My alert rule",
            "Description": "Sample alert.  Fires when 3 error records found over hour interval.",
            "Severity": "Critical",
            "ThresholdOperator": "gt",
            "ThresholdValue": 3,
            "Schedule": {
              "Name": "[toLower(concat('myschedule-',uniqueString(resourceGroup().id, deployment().name)))]",
              "Interval": 15,
              "TimeSpan": 60
            },
            "MetricsTrigger": {
              "TriggerCondition": "Consecutive",
              "Operator": "gt",
              "Value": 3
            },
            "ThrottleMinutes": 60,
            "Notification": {
              "Recipients": [
                "[parameters('recipients')]"
              ],
              "Subject": "Sample alert"
            },
            "Remediation": {
              "RunbookName": "MyRemediationRunbook",
              "WebhookUri": "https://s1events.azure-automation.net/webhooks?token=TluBFH3GpX4IEAnFoImoAWLTULkjD%2bTS0yscyrr7ogw%3d"
            },
            "Webhook": {
              "Name": "MyWebhook",
              "Uri": "https://MyService.com/webhook",
              "Payload": "{\"field1\":\"value1\",\"field2\":\"value2\"}"
            }
          }
        },
        "resources": [
          {
            "name": "[concat(variables('SolutionName'), '[' ,parameters('workspacename'), ']')]",
            "location": "[parameters('workspaceRegionId')]",
            "tags": { },
            "type": "Microsoft.OperationsManagement/solutions",
            "apiVersion": "[variables('LogAnalyticsApiVersion')]",
            "dependsOn": [
              "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches', parameters('workspacename'), variables('MySearch').Name)]",
              "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name)]",
              "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name, variables('MyAlert').Name)]",
              "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name, variables('MyAlert').Webhook.Name)]"
            ],
            "properties": {
              "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspacename'))]",
              "referencedResources": [
              ],
              "containedResources": [
                "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches', parameters('workspacename'), variables('MySearch').Name)]",
                "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name)]",
                "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name, variables('MyAlert').Name)]",
                "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name, variables('MyAlert').Webhook.Name)]"
              ]
            },
            "plan": {
              "name": "[concat(variables('SolutionName'), '[' ,parameters('workspaceName'), ']')]",
              "Version": "[variables('SolutionVersion')]",
              "product": "[variables('ProductName')]",
              "publisher": "[variables('SolutionPublisher')]",
              "promotionCode": ""
            }
          },
          {
            "name": "[concat(parameters('workspaceName'), '/', variables('MySearch').Name)]",
            "type": "Microsoft.OperationalInsights/workspaces/savedSearches",
            "apiVersion": "[variables('LogAnalyticsApiVersion')]",
            "dependsOn": [ ],
            "tags": { },
            "properties": {
              "etag": "*",
              "query": "[variables('MySearch').query]",
              "displayName": "[variables('MySearch').displayName]",
              "category": "[variables('MySearch').category]"
            }
          },
          {
            "name": "[concat(parameters('workspaceName'), '/', variables('MySearch').Name, '/', variables('MyAlert').Schedule.Name)]",
            "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/",
            "apiVersion": "[variables('LogAnalyticsApiVersion')]",
            "dependsOn": [
              "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('MySearch').Name)]"
            ],
            "properties": {
              "etag": "*",
              "interval": "[variables('MyAlert').Schedule.Interval]",
              "queryTimeSpan": "[variables('MyAlert').Schedule.TimeSpan]",
              "enabled": true
            }
          },
          {
            "name": "[concat(parameters('workspaceName'), '/', variables('MySearch').Name, '/',  variables('MyAlert').Schedule.Name, '/',  variables('MyAlert').Name)]",
            "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions",
            "apiVersion": "[variables('LogAnalyticsApiVersion')]",
            "dependsOn": [
              "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/',  variables('MySearch').Name, '/schedules/', variables('MyAlert').Schedule.Name)]"
            ],
            "properties": {
              "etag": "*",
              "Type": "Alert",
              "Name": "[variables('MyAlert').DisplayName]",
              "Description": "[variables('MyAlert').Description]",
              "Severity": "[variables('MyAlert').Severity]",
              "Threshold": {
                "Operator": "[variables('MyAlert').ThresholdOperator]",
                "Value": "[variables('MyAlert').ThresholdValue]",
                "MetricsTrigger": {
                  "TriggerCondition": "[variables('MyAlert').MetricsTrigger.TriggerCondition]",
                  "Operator": "[variables('MyAlert').MetricsTrigger.Operator]",
                  "Value": "[variables('MyAlert').MetricsTrigger.Value]"
                }
              },
              "Throttling": {
                "DurationInMinutes": "[variables('MyAlert').ThrottleMinutes]"
              },
              "EmailNotification": {
                "Recipients": "[variables('MyAlert').Notification.Recipients]",
                "Subject": "[variables('MyAlert').Notification.Subject]",
                "Attachment": "None"
              },
              "Remediation": {
                "RunbookName": "[variables('MyAlert').Remediation.RunbookName]",
                "WebhookUri": "[variables('MyAlert').Remediation.WebhookUri]"
              }
            }
          },
          {
            "name": "[concat(parameters('workspaceName'), '/', variables('MySearch').Name, '/', variables('MyAlert').Schedule.Name, '/', variables('MyAlert').Webhook.Name)]",
            "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions",
            "apiVersion": "[variables('LogAnalyticsApiVersion')]",
            "dependsOn": [
              "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('MySearch').Name, '/schedules/', variables('MyAlert').Schedule.Name)]",
              "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('MySearch').Name, '/schedules/', variables('MyAlert').Schedule.Name, '/actions/',variables('MyAlert').Name)]"
            ],
            "properties": {
              "etag": "*",
              "Type": "Webhook",
              "Name": "[variables('MyAlert').Webhook.Name]",
              "WebhookUri": "[variables('MyAlert').Webhook.Uri]",
              "CustomPayload": "[variables('MyAlert').Webhook.Payload]"
            }
          }
        ]
    }


Die folgende Parameterdatei stellt Beispielwerte für diese Lösung bereit:

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "workspacename": {
                "value": "myWorkspace"
            },
            "accountName": {
                "value": "myAccount"
            },
            "workspaceregionId": {
                "value": "East US"
            },
            "regionId": {
                "value": "East US 2"
            },
            "pricingTier": {
                "value": "Free"
            },
            "recipients": {
                "value": "recipient1@contoso.com;recipient2@contoso.com"
            }
        }
    }


## <a name="next-steps"></a>Nächste Schritte
* [Hinzufügen von Ansichten](operations-management-suite-solutions-resources-views.md) zu Ihrer Verwaltungslösung
* [Hinzufügen von Automation-Runbooks und anderen Ressourcen](operations-management-suite-solutions-resources-automation.md) zu Ihrer Verwaltungslösung




<!--HONumber=Jan17_HO4-->


