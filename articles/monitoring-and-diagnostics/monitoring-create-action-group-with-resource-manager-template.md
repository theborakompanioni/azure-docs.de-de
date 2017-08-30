---
title: Erstellen von Aktionsgruppen mit Resource Manager-Vorlagen | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine Aktionsgruppe mit einer Azure Resource Manager-Vorlage erstellen.
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
ms.date: 03/31/2017
ms.author: ancav
ms.translationtype: HT
ms.sourcegitcommit: 25e4506cc2331ee016b8b365c2e1677424cf4992
ms.openlocfilehash: 76bf353cac13f1c2169380f8dd3c1e163d4f3f41
ms.contentlocale: de-de
ms.lasthandoff: 08/24/2017

---

# <a name="create-an-action-group-with-a-resource-manager-template"></a>Erstellen einer Aktionsgruppe mithilfe einer Resource Manager-Vorlage
In diesem Artikel erfahren Sie, wie Sie mit [Azure Resource Manager-Vorlagen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates) Aktionsgruppen konfigurieren können. Mithilfe von Vorlagen können Sie automatisch Aktionsgruppen festlegen, die in bestimmten Warnungstypen wiederverwendet werden können. Diese Aktionsgruppen stellen sicher, dass alle relevanten Parteien benachrichtigt werden, wenn eine Warnung ausgelöst wird.

Die grundlegenden Schritte lauten wie folgt:

1. Erstellen Sie eine Vorlage als JSON-Datei, die die Erstellung der Aktionsgruppe beschreibt.

2. Stellen Sie die Vorlage mithilfe einer [beliebigen Bereitstellungsmethode](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy) bereit.

Zunächst erfahren Sie, wie Sie eine Resource Manager-Vorlage für eine Aktionsgruppe erstellen, wobei die Aktionsdefinitionen in der Vorlage hartcodiert sind. Zweitens erfahren Sie, wie Sie eine Vorlage erstellen, die die Daten der Webhookkonfiguration als Eingabeparameter akzeptiert, wenn die Vorlage bereitgestellt wird.

## <a name="resource-manager-templates-for-an-action-group"></a>Resource Manager-Vorlagen für eine Aktionsgruppe

Um eine Aktionsgruppe mithilfe einer Resource Manager-Vorlage zu erstellen, müssen Sie eine Ressource des Typs `Microsoft.Insights/actionGroups` erstellen. Anschließend tragen Sie alle zugehörigen Eigenschaften ein. Im Folgenden finden Sie zwei Beispielvorlagen, die eine Aktionsgruppe erstellen.

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
* Weitere Informationen zu [Aktionsgruppen](monitoring-action-groups.md).
* Weitere Informationen zu [Warnungen](monitoring-overview-alerts.md).
* Weitere Informationen zum [Erstellen einer Aktivitätsprotokollwarnung mithilfe einer Resource Manager-Vorlage](monitoring-create-activity-log-alerts-with-resource-manager-template.md).

