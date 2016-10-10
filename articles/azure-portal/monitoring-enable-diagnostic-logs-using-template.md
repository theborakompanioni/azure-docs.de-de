<properties
	pageTitle="Automatisches Aktivieren von Diagnoseeinstellungen mithilfe einer Resource Manager-Vorlage | Microsoft Azure"
	description="Hier erfahren Sie, wie Sie mithilfe einer Resource Manager-Vorlage Diagnoseeinstellungen erstellen, mit denen Sie Ihre Diagnoseprotokolle an Event Hubs streamen oder in einem Speicherkonto speichern können."
	authors="johnkemnetz"
	manager="rboucher"
	editor=""
	services="monitoring-and-diagnostics"
	documentationCenter="monitoring-and-diagnostics"/>

<tags
	ms.service="monitoring-and-diagnostics"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="09/26/2016"
	ms.author="johnkem"/>

# Automatisches Aktivieren von Diagnoseeinstellungen bei der Ressourcenerstellung mithilfe einer Resource Manager-Vorlage
In diesem Artikel erfahren Sie, wie Sie mithilfe einer [Azure Resource Manager-Vorlage](../resource-group-authoring-templates.md) Diagnoseeinstellungen für eine Ressource konfigurieren, wenn die Ressource erstellt wird. Dadurch können Sie automatisch mit dem Streamen Ihrer Diagnoseprotokolle und Metriken an Event Hubs beginnen, sie in einem Speicherkonto archivieren oder sie bei der Erstellung einer Ressource an Log Analytics senden.

Die Methode zum Aktivieren von Diagnoseprotokollen mithilfe einer Resource Manager-Vorlage hängt vom Ressourcentyp ab.

- Die von **computefremden Ressourcen** (Netzwerksicherheitsgruppen, Logik-Apps, Automation oder Ähnliches) verwendeten Diagnoseeinstellungen finden Sie in [diesem Artikel](./monitoring-overview-of-diagnostic-logs.md#diagnostic-settings).
- Die von **Computeressourcen** (WAD-LAD-basierte Ressourcen) verwendete WAD-/LAD-Konfigurationsdatei wird in [diesem Artikel](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md) beschrieben.

In diesem Artikel erfahren Sie, wie Sie die Diagnose mit diesen Methoden konfigurieren.

Die grundlegenden Schritte lauten wie folgt:

1. Erstellen Sie eine Vorlage als JSON-Datei, die die Ressourcenerstellung und die Diagnoseaktivierung beschreibt.
2. [Stellen Sie die Vorlage mithilfe einer beliebigen Bereitstellungsmethode bereit.](../resource-group-template-deploy.md)

Im Anschluss finden Sie ein Beispiel für die JSON-Vorlagendatei, die für computefremde Ressourcen und für Computeressourcen generiert werden muss.

## Vorlage für computefremde Ressourcen
Für computefremde Ressourcen müssen zwei Schritte ausgeführt werden:

1. Fügen Sie Parameter für den Speicherkontonamen, die Service Bus-Regel-ID und/oder die OMS Log Analytics-Arbeitsbereichs-ID zum Parameterblob hinzu (um die Archivierung von Diagnoseprotokollen in einem Speicherkonto, das Streamen von Protokollen an Event Hubs und/oder das Senden von Protokollen an Log Analytics zu ermöglichen).

    ```json
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Storage Account in which Diagnostic Logs should be saved."
      }
    },
    "serviceBusRuleId": {
      "type": "string",
      "metadata": {
        "description": "Service Bus Rule Id for the Service Bus Namespace in which the Event Hub should be created or streamed to."
      }
    },
    "workspaceId":{
      "type": "string",
      "metadata": {
        "description": "Log Analytics workspace ID for the Log Analytics workspace to which logs will be sent."
      }
    }
    ```
2. Fügen Sie im Ressourcenarray der Ressource, für die Sie Diagnoseprotokolle aktivieren möchten, eine Ressource vom Typ `[resource namespace]/providers/diagnosticSettings` hinzu.

    ```json
    "resources": [
      {
        "type": "providers/diagnosticSettings",
        "name": "Microsoft.Insights/service",
        "dependsOn": [
          "[/*resource Id for which Diagnostic Logs will be enabled>*/]"
        ],
        "apiVersion": "2015-07-01",
        "properties": {
          "storageAccountId": "[resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName'))]",
          "serviceBusRuleId": "[parameters('serviceBusRuleId')]",
          "workspaceId": "[parameters('workspaceId')]",
          "logs": [ 
            {
              "category": "/* log category name */",
              "enabled": true,
              "retentionPolicy": {
                "days": 0,
                "enabled": false
              }
            }
          ]
        }
      }
    ]
    ```

Das Eigenschaftenblob für die Diagnoseeinstellung verwendet das in [diesem Artikel](https://msdn.microsoft.com/library/azure/dn931931.aspx) beschriebene Format.

Im folgenden vollständigen Beispiel wird eine Netzwerksicherheitsgruppe erstellt und das Streamen an Event Hubs sowie das Speichern in einem Speicherkonto aktiviert:

```json

{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "nsgName": {
      "type": "string",
      "metadata": {
        "description": "Name of the NSG that will be created."
      }
    },
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Storage Account in which Diagnostic Logs should be saved."
      }
    },
    "serviceBusRuleId": {
      "type": "string",
      "metadata": {
        "description": "Service Bus Rule Id for the Service Bus Namespace in which the Event Hub should be created or streamed to."
      }
    },
    "workspaceId": {
      "type": "string",
      "metadata": {
        "description": "Log Analytics workspace ID for the Log Analytics workspace to which logs will be sent."
      }
    }
  },
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Network/networkSecurityGroups",
      "name": "[parameters('nsgName')]",
      "apiVersion": "2016-03-30",
      "location": "westus",
      "properties": {
        "securityRules": []
      },
      "resources": [
        {
          "type": "providers/diagnosticSettings",
          "name": "Microsoft.Insights/service",
          "dependsOn": [
            "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('nsgName'))]"
          ],
          "apiVersion": "2015-07-01",
          "properties": {
            "storageAccountId": "[resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName'))]",
            "serviceBusRuleId": "[parameters('serviceBusRuleId')]",
            "workspaceId": "[parameters('workspaceId')]",
            "logs": [
              {
                "category": "NetworkSecurityGroupEvent",
                "enabled": true,
                "retentionPolicy": {
                  "days": 0,
                  "enabled": false
                }
              },
              {
                "category": "NetworkSecurityGroupRuleCounter",
                "enabled": true,
                "retentionPolicy": {
                  "days": 0,
                  "enabled": false
                }
              }
            ]
          }
        }
      ],
      "dependsOn": []
    }
  ]
}

```

## Vorlage für Computeressourcen
Führen Sie die folgenden Schritte aus, um die Diagnose für eine Computeressource (beispielsweise für einen virtuellen Computer oder für einen Service Fabric-Cluster) zu aktivieren:

1. Fügen Sie die Azure-Diagnoseerweiterung der VM-Ressourcendefinition hinzu.
2. Geben Sie ein Speicherkonto und/oder eine Event Hub-Instanz als Parameter an.
3. Fügen Sie den Inhalt der WADCfg-XML-Datei in die XMLCfg-Eigenschaft ein, und achten Sie dabei darauf, dass alle XML-Zeichen ordnungsgemäß mit Escapezeichen versehen sind.

> [AZURE.WARNING] Der letzte Schritt ist unter Umständen nicht ganz einfach. In [diesem Artikel](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md#diagnostics-configuration-variables) finden Sie ein Beispiel, in dem das Diagnosekonfigurationsschema in Variablen aufgeteilt wird, die korrekt mit Escapezeichen versehen und ordnungsgemäß formatiert sind.

Der gesamte Prozess wird in [diesem Dokument](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md) beschrieben (einschließlich Beispielen).


## Nächste Schritte
- [Informieren Sie sich ausführlicher über Azure-Diagnoseprotokolle.](./monitoring-overview-of-diagnostic-logs.md)
- [Streamen Sie Azure-Diagnoseprotokolle an Event Hubs.](./monitoring-stream-diagnostic-logs-to-event-hubs.md)

<!---HONumber=AcomDC_0928_2016-->