<properties 
	pageTitle="Erstellen einer Logik-App mit Azure-Ressourcen-Manager-Vorlagen in Azure App Service | Microsoft Azure" 
	description="Verwenden Sie eine Azure-Ressourcen-Manager-Vorlage, um eine leere Logik-App zum Definieren von Workflows bereitzustellen." 
	services="logic-apps" 
	documentationCenter="" 
	authors="MSFTMan" 
	manager="erikre" 
	editor=""/>

<tags 
	ms.service="logic-apps" 
	ms.workload="integration" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="07/25/2016" 
	ms.author="deonhe"/>

# Erstellen einer Logik-App mithilfe einer Vorlage

Erstellen Sie mit einer Azure-Ressourcen-Manager-Vorlage eine leere Logik-App, die zum Definieren von Workflows verwendet werden kann. Sie können definieren, welche Ressourcen bereitgestellt werden, und wie Sie Parameter definieren, die angegeben werden, wenn die Bereitstellung ausgeführt wird. Sie können diese Vorlage für Ihre eigenen Bereitstellungen verwenden oder an Ihre Anforderungen anpassen.

Weitere Informationen zu den Eigenschaften der Logik-App finden Sie unter [API für die Workflowverwaltung einer Logik-App](https://msdn.microsoft.com/library/azure/mt643788.aspx).

Beispiele für die Definition selbst finden Sie unter [Schreiben von Logik-App-Definitionen](app-service-logic-author-definitions.md).

Weitere Informationen zum Erstellen von Vorlagen finden Sie unter [Erstellen von Azure-Ressourcen-Manager-Vorlagen](../resource-group-authoring-templates.md).

Die vollständige Vorlage finden Sie unter [Logik-App-Vorlage](https://github.com/Azure/azure-quickstart-templates/blob/master/101-logic-app-create/azuredeploy.json).

## Was Sie bereitstellen

Mit dieser Vorlage stellen Sie eine Logik-App bereit.

Wählen Sie die folgende Schaltfläche, um die Bereitstellung automatisch auszuführen:

[![Bereitstellen in Azure](media/app-service-logic-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-logic-app-create%2Fazuredeploy.json)

## Parameter

[AZURE.INCLUDE [app-service-logic-deploy-parameters](../../includes/app-service-logic-deploy-parameters.md)]

### testUri

     "testUri": {
        "type": "string",
        "defaultValue": "http://azure.microsoft.com/status/feed/"
      }
    
## Bereitzustellende Ressourcen

### Logik-App

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


## Befehle zum Ausführen der Bereitstellung

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### PowerShell

    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -ResourceGroupName ExampleDeployGroup

### Azure-Befehlszeilenschnittstelle

    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -g ExampleDeployGroup


 

<!---HONumber=AcomDC_0803_2016-->