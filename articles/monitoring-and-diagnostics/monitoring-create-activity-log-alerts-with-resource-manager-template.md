---
title: "Erstellen einer Aktivitätsprotokollwarnung mithilfe einer Resource Manager-Vorlage | Microsoft-Dokumentation"
description: Lassen Sie sich benachrichtigen, wenn Ihre Azure-Ressourcen erstellt werden.
author: anirudhcavale
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: ancav
ms.translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: 239b8fe2a7724bb34e7060c90af73825e61d8398
ms.contentlocale: de-de
ms.lasthandoff: 04/12/2017


---
# <a name="create-an-activity-log-alert-with-a-resource-manager-template"></a>Erstellen einer Aktivitätsprotokollwarnung mithilfe einer Resource Manager-Vorlage
In diesem Artikel erfahren Sie, wie Sie mit einer [Azure Resource Manager-Vorlage](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-authoring-templates) Aktivitätsprotokollwarnungen konfigurieren können. So können Sie im Rahmen des automatisierten Bereitstellungsprozesses Warnungen für Ihre Ressourcen gleich bei der Erstellung automatisch einrichten.

Die grundlegenden Schritte lauten wie folgt:

1.    Erstellen Sie eine Vorlage als JSON-Datei, die die Erstellung der Aktivitätsprotokollwarnung beschreibt.
2.    [Stellen Sie die Vorlage mithilfe einer beliebigen Bereitstellungsmethode bereit.](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy)

Im Folgenden wird beschrieben, wie Sie eine Resource Manager-Vorlage zunächst für eine Aktivitätsprotokollwarnung allein und dann im Rahmen der Erstellung einer weiteren Ressource erstellen.

## <a name="resource-manager-template-for-an-activity-log-alert"></a>Resource Manager-Vorlage für eine Aktivitätsprotokollwarnung
Um eine Aktivitätsprotokollwarnung mithilfe einer Resource Manager-Vorlage zu erstellen, müssen Sie eine Ressource des Typs `microsoft.insights/activityLogAlerts` erstellen und alle zugehörigen Eigenschaften eingeben. Unten sehen Sie eine Vorlage, mit der eine Aktivitätsprotokollwarnung erstellt wird.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "activityLogAlertName": {
      "type": "string",
      "metadata": {
        "description": "Unique name (within the Resource Group) for the Activity log alert."
      }
    },
    "activityLogAlertEnabled": {
      "type": "bool",
      "defaultValue": true,
      "metadata": {
        "description": "Indicates whether or not the alert is enabled."
      }
    },
    "actionGroupResourceId": {
      "type": "string",
      "metadata": {
        "description": "Resource Id for the Action group."
      }
    }
  },
  "resources": [   
    {
      "type": "Microsoft.Insights/activityLogAlerts",
      "apiVersion": "2017-04-01",
      "name": "[parameters('activityLogAlertName')]",      
      "location": "Global",
      "properties": {
        "enabled": "[parameters('activityLogAlertEnabled')]",
        "scopes": [
            "[subscription().id]"
        ],        
        "condition": {
          "allOf": [
            {
              "field": "category",
              "equals": "Administrative"
            },
            {
              "field": "operationName",
              "equals": "Microsoft.Resources/deployments/write"
            },
            {
              "field": "resourceType",
              "equals": "Microsoft.Resources/deployments"
            }
          ] 
        },
        "actions": {
          "actionGroups": 
          [
            {
              "actionGroupId": "[parameters('actionGroupResourceId')]"
            }
          ]
        }
      }
    }
  ]
}
```

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu [Warnungen](monitoring-overview-alerts.md)  
Hinzufügen von [Aktivitätsgruppen mithilfe einer Resource Manager-Vorlage](monitoring-create-action-group-with-resource-manager-template.md)

