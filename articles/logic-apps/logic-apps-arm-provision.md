---
title: Erstellen einer Logik-App mithilfe einer Vorlage in Azure | Microsoft-Dokumentation
description: Verwenden Sie eine Azure Resource Manager-Vorlage, um eine Logik-App zum Definieren von Workflows bereitzustellen.
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 7574cc7c-e5a1-4b7c-97f6-0cffb1a5d536
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2016
ms.author: mandia
translationtype: Human Translation
ms.sourcegitcommit: 2f407a428aa176cc5c2a3b6bb236b522bda5ab64
ms.openlocfilehash: 1305fdc4cf87905eef701ec5471d6329006c2b5f


---
# <a name="create-a-logic-app-using-a-template"></a>Erstellen einer Logik-App mithilfe einer Vorlage
Mit Vorlagen können Sie einfach verschiedene Connectors in einer Logik-App verwenden. Logic Apps enthält Azure Resource Manager-Vorlagen, mit denen Sie eine Logik-App erstellen können, die zum Definieren von Geschäftsworkflows verwendet werden kann. Sie können festlegen, welche Ressourcen bereitgestellt werden und wie Sie Parameter beim Bereitstellen der Logik-App definieren. Sie können diese Vorlage für Ihre eigenen Unternehmensszenarien verwenden oder Ihren Anforderungen entsprechend anpassen.

Weitere Informationen zu den Eigenschaften der Logik-App finden Sie unter [API für die Workflowverwaltung einer Logik-App](https://msdn.microsoft.com/library/azure/mt643788.aspx). 

Beispiele für die Definition selbst finden Sie unter [Schreiben von Logik-App-Definitionen](logic-apps-author-definitions.md). 

Weitere Informationen zum Erstellen von Vorlagen finden Sie unter [Erstellen von Azure-Ressourcen-Manager-Vorlagen](../azure-resource-manager/resource-group-authoring-templates.md).

Die vollständige Vorlage finden Sie unter [Logik-App-Vorlage](https://github.com/Azure/azure-quickstart-templates/blob/master/101-logic-app-create/azuredeploy.json).

## <a name="what-you-deploy"></a>Was wird bereitgestellt?
Mit dieser Vorlage stellen Sie eine Logik-App bereit.

Wählen Sie die folgende Schaltfläche, um die Bereitstellung automatisch auszuführen:  

[![Bereitstellen in Azure](media/logic-apps-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-logic-app-create%2Fazuredeploy.json)

## <a name="parameters"></a>Parameter
[!INCLUDE [app-service-logic-deploy-parameters](../../includes/app-service-logic-deploy-parameters.md)]

### <a name="testuri"></a>testUri
     "testUri": {
        "type": "string",
        "defaultValue": "http://azure.microsoft.com/en-us/status/feed/"
      }

## <a name="resources-to-deploy"></a>Bereitzustellende Ressourcen
### <a name="logic-app"></a>Logik-App
Erstellt die Logik-App.

Die Vorlage erstellt einen Parameterwert für den Namen der Logik-App. Sie legt den Speicherort der Logik-App auf den Speicherort für die Ressourcengruppe fest. 

Diese spezifische Definition wird einmal pro Stunde ausgeführt und pingt den im Parameter **testUri** angegebenen Speicherort. 

    {
      "type": "Microsoft.Logic/workflows",
      "apiVersion": "2016-06-01",
      "name": "[parameters('logicAppName')]",
      "location": "[resourceGroup().location]",
      "tags": {
        "displayName": "LogicApp"
      },
      "properties": {
        "definition": {
          "$schema": "http://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "testURI": {
              "type": "string",
              "defaultValue": "[parameters('testUri')]"
            }
          },
          "triggers": {
            "recurrence": {
              "type": "recurrence",
              "recurrence": {
                "frequency": "Hour",
                "interval": 1
              }
            }
          },
          "actions": {
            "http": {
              "type": "Http",
              "inputs": {
                "method": "GET",
                "uri": "@parameters('testUri')"
              },
              "runAfter": {}
            }
          },
          "outputs": {}
        },
        "parameters": {}
      }
    }


## <a name="commands-to-run-deployment"></a>Befehle zum Ausführen der Bereitstellung
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell
    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -ResourceGroupName ExampleDeployGroup

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle
    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -g ExampleDeployGroup






<!--HONumber=Jan17_HO5-->


