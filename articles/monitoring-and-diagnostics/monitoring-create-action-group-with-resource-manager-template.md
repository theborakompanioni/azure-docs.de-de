---
title: Erstellen von Aktionsgruppen mit Resource Manager-Vorlagen | Microsoft-Dokumentation
description: "Aktivitätsgruppen ermöglichen Benachrichtigungen per E-Mail oder SMS oder den Anruf von Webhooks, wenn bestimmte Ereignisse eintreten."
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
ms.openlocfilehash: 2dd7b14f1466fa7244a2af2c030d8b794658aaad
ms.contentlocale: de-de
ms.lasthandoff: 04/12/2017


---

# <a name="create-an-action-group-with-a-resource-manager-template"></a>Erstellen einer Aktionsgruppe mithilfe einer Resource Manager-Vorlage
In diesem Artikel erfahren Sie, wie Sie mit [Azure Resource Manager-Vorlagen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates) Aktionsgruppen konfigurieren können. Mithilfe von Vorlagen können Sie Aktionsgruppen für Ihre Ressourcen beim Erstellen automatisch einrichten, um sicherzustellen, dass die richtigen Beteiligten benachrichtigt werden, wenn eine Warnung ausgelöst wird.

Die grundlegenden Schritte lauten wie folgt:

1.    Erstellen Sie eine Vorlage als JSON-Datei, die die Erstellung der Aktionsgruppe beschreibt.
2.    [Stellen Sie die Vorlage mithilfe einer beliebigen Bereitstellungsmethode bereit.](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy)

Im Folgenden wird beschrieben, wie Sie eine Resource Manager-Vorlage zunächst für eine Aktionsgruppe allein und dann im Rahmen der Erstellung einer weiteren Ressource erstellen.

## <a name="resource-manager-template-for-an-action-group"></a>Resource Manager-Vorlage für eine Aktionsgruppe

Um eine Aktionsgruppe mithilfe einer Resource Manager-Vorlage zu erstellen, müssen Sie eine Ressource des Typs `Microsoft.Insights/actionGroups` erstellen und alle zugehörigen Eigenschaften eingeben. Im Folgenden finden Sie einige Beispielvorlagen, die eine Aktivitätsgruppe erstellen.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "actionGroupName": {
      "type": "string",
      "metadata": {
        "description": "Unique name (within the Resource Group) for the Action group."
      }
    },
    "actionGroupShortName": {
      "type": "string",
      "metadata": {
        "description": "Short name (maximum 12 characters) for the Action group."
      }
    }
  },
  "resources": [
    {
      "type": "Microsoft.Insights/actionGroups",
      "apiVersion": "2017-04-01",
      "name": "[parameters('actionGroupName')]",
      "location": "Global",
      "properties": {
        "groupShortName": "[parameters('actionGroupShortName')]",
        "enabled": true,
        "smsReceivers": [
          {
            "name": "contosoSMS",
            "countryCode": "1",
            "phoneNumber": "5555551212"
          },
          {
            "name": "contosoSMS2",
            "countryCode": "1",
            "phoneNumber": "5555552121"
          }
        ],
        "emailReceivers": [
          {
            "name": "contosoEmail",
            "emailAddress": "devops@contoso.com"
          },
          {
            "name": "contosoEmail2",
            "emailAddress": "devops2@contoso.com"
          }
        ],
        "webhookReceivers": [
          {
            "name": "contosoHook",
            "serviceUri": "http://requestb.in/1bq62iu1"
          },
          {
            "name": "contosoHook2",
            "serviceUri": "http://requestb.in/1bq62iu2"
          }
        ]
      }
    }
  ],
  "outputs":{
      "actionGroupId":{
          "type":"string",
          "value":"[resourceId('Microsoft.Insights/actionGroups',parameters('actionGroupName'))]"
      }
  }
}
```

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "actionGroupName": {
      "type": "string",
      "metadata": {
        "description": "Unique name (within the Resource Group) for the Action group."
      }
    },
    "actionGroupShortName": {
      "type": "string",
      "metadata": {
        "description": "Short name (maximum 12 characters) for the Action group."
      }
    },
    "webhookReceiverName": {
      "type": "string",
      "metadata": {
        "description": "Webhook receiver service URI."
      }
    },    
    "webhookServiceUri": {
      "type": "string",
      "metadata": {
        "description": "Webhook receiver service URI."
      }
    }    
  },
  "resources": [
    {
      "type": "Microsoft.Insights/actionGroups",
      "apiVersion": "2017-04-01",
      "name": "[parameters('actionGroupName')]",
      "location": "Global",
      "properties": {
        "groupShortName": "[parameters('actionGroupShortName')]",
        "enabled": true,
        "smsReceivers": [
        ],
        "emailReceivers": [
        ],
        "webhookReceivers": [
          {
            "name": "[parameters('webhookReceiverName')]",
            "serviceUri": "[parameters('webhookServiceUri')]"
          }
        ]
      }
    }
  ],
  "outputs":{
      "actionGroupResourceId":{
          "type":"string",
          "value":"[resourceId('Microsoft.Insights/actionGroups',parameters('actionGroupName'))]"
      }
  }
}
```


## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu [Aktionsgruppen](monitoring-action-groups.md)  
Weitere Informationen zu [Warnungen](monitoring-overview-alerts.md)  
Hinzufügen von [Warnungen mithilfe einer Resource Manager-Vorlage](monitoring-create-activity-log-alerts-with-resource-manager-template.md)

