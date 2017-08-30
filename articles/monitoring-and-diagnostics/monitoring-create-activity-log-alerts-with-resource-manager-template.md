---
title: "Erstellen einer Aktivitätsprotokollwarnung mithilfe einer Resource Manager-Vorlage | Microsoft-Dokumentation"
description: Lassen Sie sich benachrichtigen, wenn Ihre Azure-Ressourcen erstellt werden.
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2017
ms.author: ancav
ms.translationtype: HT
ms.sourcegitcommit: 25e4506cc2331ee016b8b365c2e1677424cf4992
ms.openlocfilehash: 92076c7fe1f867919b7e02abf79cf0fb74fb7eb4
ms.contentlocale: de-de
ms.lasthandoff: 08/24/2017

---
# <a name="create-an-activity-log-alert-with-a-resource-manager-template"></a>Erstellen einer Aktivitätsprotokollwarnung mithilfe einer Resource Manager-Vorlage
In diesem Artikel erfahren Sie, wie Sie mit einer [Azure Resource Manager-Vorlage](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-authoring-templates) Aktivitätsprotokollwarnungen konfigurieren können. Mithilfe von Vorlagen können Sie problemlos viele Warnungen einrichten, die basierend auf bestimmten Aktivitätsprotokollereignis-Bedingungen als Teil Ihres automatisierten Bereitstellungsprozesses aktiviert werden.

Die grundlegenden Schritte lauten wie folgt:

1. Erstellen Sie eine Vorlage als JSON-Datei, die die Erstellung der Aktivitätsprotokollwarnung beschreibt.

2. Stellen Sie die Vorlage mithilfe einer [beliebigen Bereitstellungsmethode](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy) bereit.

## <a name="resource-manager-template-for-an-activity-log-alert"></a>Resource Manager-Vorlage für eine Aktivitätsprotokollwarnung
Um eine Aktivitätsprotokollwarnung mithilfe einer Resource Manager-Vorlage zu erstellen, müssen Sie eine Ressource des Typs `microsoft.insights/activityLogAlerts` erstellen. Anschließend tragen Sie alle zugehörigen Eigenschaften ein. Hier sehen Sie eine Vorlage, mit der eine Aktivitätsprotokollwarnung erstellt wird.

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

Besuchen Sie unseren [Azure-Schnellstartkatalog](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Insights), um einige Beispiele für Aktivitätsprotokollwarnungs-Vorlagen zu erhalten.

## <a name="next-steps"></a>Nächste Schritte
- Weitere Informationen zu [Warnungen](monitoring-overview-alerts.md).
- Weitere Informationen zum [Erstellen einer Aktionsgruppe mithilfe einer Resource Manager-Vorlage](monitoring-create-action-group-with-resource-manager-template.md).
- Weitere Informationen zum [Erstellen einer Aktivitätsprotokollwarnung, um alle automatischen Modulskalierungsvorgänge für Ihr Abonnement zu überwachen](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert).
- Weitere Informationen zum [Erstellen einer Aktivitätsprotokollwarnung, um alle Autoskalierungs-Vorgänge zum horizontalen Herunterskalieren und horizontalen Hochskalieren in Ihrem Abonnement, bei denen Fehler aufgetreten sind, zu überwachen](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert).

