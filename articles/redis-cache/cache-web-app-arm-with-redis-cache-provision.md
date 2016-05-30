<properties 
	pageTitle="Bereitstellen einer Web-App mit Redis Cache" 
	description="Verwenden Sie eine Azure-Ressourcen-Manager-Vorlage, um eine Web-App mit Redis Cache bereitzustellen." 
	services="app-service" 
	documentationCenter="" 
	authors="steved0x" 
	manager="erickson-doug" 
	editor=""/>

<tags 
	ms.service="app-service" 
	ms.workload="web" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="03/04/2016" 
	ms.author="sdanie"/>

# Erstellen einer Web-App und einer Redis Cache-Instanz mithilfe einer Vorlage

In diesem Thema erfahren Sie, wie Sie eine Azure-Ressourcen-Manager-Vorlage erstellen, die eine Azure Web-App mit einer Redis Cache-Instanz bereitstellt. Sie erfahren, wie Sie definieren, welche Ressourcen bereitgestellt werden und wie Sie Parameter definieren, die angegeben werden, wenn die Bereitstellung ausgeführt wird. Sie können diese Vorlage für Ihre eigenen Bereitstellungen verwenden oder an Ihre Anforderungen anpassen.

Weitere Informationen zum Erstellen von Vorlagen finden Sie unter [Erstellen von Azure-Ressourcen-Manager-Vorlagen](../resource-group-authoring-templates.md).

Die vollständige Vorlage finden Sie unter [Web-App mit Redis Cache-Vorlage](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-with-redis-cache/azuredeploy.json).

## Was Sie bereitstellen

In dieser Vorlage stellen Sie Folgendes bereit:

- Azure-Web-App
- Azure Redis Cache.

Klicken Sie auf folgende Schaltfläche, um die Bereitstellung automatisch auszuführen:

[![Bereitstellen in Azure](./media/cache-web-app-arm-with-redis-cache-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-with-redis-cache%2Fazuredeploy.json)

## Anzugebende Parameter

[AZURE.INCLUDE [app-service-web-deploy-web-parameters](../../includes/app-service-web-deploy-web-parameters.md)]

[AZURE.INCLUDE [cache-deploy-parameters](../../includes/cache-deploy-parameters.md)]

## Variablen für Namen

Diese Vorlage verwendet Variablen für die Erstellung von Namen für die Ressourcen. Sie nutzt die [uniqueString](../resource-group-template-functions/#uniquestring)-Funktion, um einen Wert basierend auf der Ressourcengruppen-ID zu erstellen.

    "variables": {
      "hostingPlanName": "[concat('hostingplan', uniqueString(resourceGroup().id))]",
      "webSiteName": "[concat('webSite', uniqueString(resourceGroup().id))]",
      "cacheName": "[concat('cache', uniqueString(resourceGroup().id))]"
    },


## Bereitzustellende Ressourcen

[AZURE.INCLUDE [app-service-web-deploy-web-host](../../includes/app-service-web-deploy-web-host.md)]

### Redis-Cache

Erstellt den Azure Redis Cache, der mit der Web-App verwendet werden soll. Der Name des Cache wird in der **cacheName**-Variablen angegeben.

Die Vorlage erstellt den Cache am gleichen Speicherort wie die Ressourcengruppe.

    {
      "name": "[variables('cacheName')]",
      "type": "Microsoft.Cache/Redis",
      "location": "[resourceGroup().location]",
      "apiVersion": "2015-08-01",
      "dependsOn": [ ],
      "tags": {
        "displayName": "cache"
      },
      "properties": {
        "sku": {
          "name": "[parameters('cacheSKUName')]",
          "family": "[parameters('cacheSKUFamily')]",
          "capacity": "[parameters('cacheSKUCapacity')]"
        }
      }
    }


### Web-App

Erstellt die Web-App mit dem Namen, der in der **webSiteName**-Variablen angegeben ist.

Beachten Sie, dass die Web-App mit App-Einstellungseigenschaften konfiguriert ist, die es der App ermöglichen, den Redis Cache zu verwenden. Diese App-Einstellungen werden dynamisch anhand der Werte erstellt, die während der Bereitstellung angegeben wurden.
        
    {
      "apiVersion": "2015-08-01",
      "name": "[variables('webSiteName')]",
      "type": "Microsoft.Web/sites",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Web/serverFarms/', variables('hostingPlanName'))]",
        "[concat('Microsoft.Cache/Redis/', variables('cacheName'))]"
      ],
      "tags": {
        "[concat('hidden-related:', resourceGroup().id, '/providers/Microsoft.Web/serverfarms/', variables('hostingPlanName'))]": "empty",
        "displayName": "Website"
      },
      "properties": {
        "name": "[variables('webSiteName')]",
        "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]"
      },
      "resources": [
        {
          "apiVersion": "2015-08-01",
          "type": "config",
          "name": "appsettings",
          "dependsOn": [
            "[concat('Microsoft.Web/Sites/', variables('webSiteName'))]",
            "[concat('Microsoft.Cache/Redis/', variables('cacheName'))]"
          ],
          "properties": {
            "CacheConnection": "[concat(variables('cacheName'),'.redis.cache.windows.net,abortConnect=false,ssl=true,password=', listKeys(resourceId('Microsoft.Cache/Redis', variables('cacheName')), '2015-08-01').primaryKey)]"
          }
        }
      ]
    }

## Befehle zum Ausführen der Bereitstellung

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### PowerShell

    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-with-redis-cache/azuredeploy.json -ResourceGroupName ExampleDeployGroup

### Azure-Befehlszeilenschnittstelle

    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-with-redis-cache/azuredeploy.json -g ExampleDeployGroup

<!---HONumber=AcomDC_0518_2016-->