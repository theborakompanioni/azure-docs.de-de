---
title: Bereitstellen eines Redis Cache mit Azure Resource Manager | Microsoft-Dokumentation
description: Verwenden Sie eine Azure-Ressourcen-Manager-Vorlage, um einen Azure Redis Cache bereitzustellen.
services: app-service
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: ce6f5372-7038-4655-b1c5-108f7c148282
ms.service: cache
ms.workload: web
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: sdanie
translationtype: Human Translation
ms.sourcegitcommit: c885edd7064f7ee3c9907e6b938d826e181dcae5
ms.openlocfilehash: cce5d63e8bad2dd066cb4c28e2a8a9cb16c47953


---
# <a name="create-a-redis-cache-using-a-template"></a>Erstellen einer Redis Cache-Instanz mithilfe einer Vorlage
In diesem Thema erfahren Sie, wie Sie eine Azure Resource Manager-Vorlage erstellen, die einen Azure Redis Cache bereitstellt. Der Cache kann mit einem vorhandenen Speicherkonto verwendet werden, um Diagnosedaten aufzunehmen. Sie erfahren darüber hinaus, wie Sie definieren, welche Ressourcen bereitgestellt werden und wie Sie Parameter definieren, die angegeben werden, wenn die Bereitstellung ausgeführt wird. Sie können diese Vorlage für Ihre eigenen Bereitstellungen verwenden oder an Ihre Anforderungen anpassen.

Derzeit werden für alle Caches in derselben Region für ein Abonnement dieselben Diagnoseeinstellungen verwendet. Ein Aktualisieren eines Cache in der Region wirkt sich auf alle anderen Caches in der Region aus.

Weitere Informationen zum Erstellen von Vorlagen finden Sie unter [Erstellen von Azure-Ressourcen-Manager-Vorlagen](../azure-resource-manager/resource-group-authoring-templates.md).

Die vollständige Vorlage finden Sie unter [Redis Cache-Vorlage](https://github.com/Azure/azure-quickstart-templates/blob/master/101-redis-cache/azuredeploy.json).

> [!NOTE]
> Resource Manager-Vorlagen für den neuen [Tarif „Premium“](cache-premium-tier-intro.md) sind verfügbar. 
> 
> * [Erstellen eines Premium-Redis Caches mit Clustering](https://azure.microsoft.com/documentation/templates/201-redis-premium-cluster-diagnostics/)
> * [Erstellen eines Premium-Redis Caches mit Datenpersistenz](https://azure.microsoft.com/documentation/templates/201-redis-premium-persistence/)
> * [Erstellen eines Premium-Redis Caches mithilfe von VNet und optionalem Clustering](https://azure.microsoft.com/documentation/templates/201-redis-premium-vnet-cluster-diagnostics/)
> 
> Um die neuesten Vorlagen zu ermitteln, suchen Sie in [Azure-Schnellstartvorlagen](https://azure.microsoft.com/documentation/templates/) nach `Redis Cache`.
> 
> 

## <a name="what-you-will-deploy"></a>Was Sie bereitstellen
In dieser Vorlage stellen Sie einen Azure Redis Cache bereit, der ein vorhandenes Speicherkonto für Diagnosedaten verwendet.

Klicken Sie auf folgende Schaltfläche, um die Bereitstellung automatisch auszuführen:

[![In Azure bereitstellen](./media/cache-redis-cache-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-redis-cache%2Fazuredeploy.json)

## <a name="parameters"></a>Parameter
Mit dem Azure-Ressourcen-Manager definieren Sie die Parameter für Werte, die Sie bei der Bereitstellung der Vorlage angeben möchten. Die Vorlage enthält einen Abschnitt namens "Parameters", der alle Parameterwerte enthält.
Definieren Sie einen Parameter für die Werte, die basierend auf dem bereitgestellten Projekt oder der bereitgestellten Umgebung variieren. Definieren Sie keine Parameter für Werte, die sich nicht ändern. Jeder Parameterwert wird in der Vorlage verwendet, um die bereitgestellten Ressourcen zu definieren. 

[!INCLUDE [app-service-web-deploy-redis-parameters](../../includes/cache-deploy-parameters.md)]

### <a name="rediscachelocation"></a>redisCacheLocation
Der Speicherort der Redis Cache-Instanz. Um eine optimale Leistung zu erzielen, verwenden Sie den gleichen Speicherort wie für die App, die den Cache nutzen wird.

    "redisCacheLocation": {
      "type": "string"
    }

### <a name="existingdiagnosticsstorageaccountname"></a>existingDiagnosticsStorageAccountName
Der Name des vorhandenen Speicherkontos, das für Diagnosen verwendet werden soll. 

    "existingDiagnosticsStorageAccountName": {
      "type": "string"
    }

### <a name="enablenonsslport"></a>enableNonSslPort
Ein boolescher Wert, der angibt, ob Zugriff über Nicht-SSL-Ports zugelassen werden soll.

    "enableNonSslPort": {
      "type": "bool"
    }

### <a name="diagnosticsstatus"></a>diagnosticsStatus
Ein Wert, der angibt, ob Diagnosen aktiviert sind. Verwenden Sie ON oder OFF.

    "diagnosticsStatus": {
      "type": "string",
      "defaultValue": "ON",
      "allowedValues": [
            "ON",
            "OFF"
        ]
    }

## <a name="resources-to-deploy"></a>Bereitzustellende Ressourcen
### <a name="redis-cache"></a>Redis-Cache
Erstellt den Azure Redis Cache.

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('redisCacheName')]",
      "type": "Microsoft.Cache/Redis",
      "location": "[parameters('redisCacheLocation')]",
      "properties": {
        "enableNonSslPort": "[parameters('enableNonSslPort')]",
        "sku": {
          "capacity": "[parameters('redisCacheCapacity')]",
          "family": "[parameters('redisCacheFamily')]",
          "name": "[parameters('redisCacheSKU')]"
        }
      },
      "resources": [
        {
          "apiVersion": "2015-07-01",
          "type": "Microsoft.Cache/redis/providers/diagnosticsettings",
          "name": "[concat(parameters('redisCacheName'), '/Microsoft.Insights/service')]",
          "location": "[parameters('redisCacheLocation')]",
          "dependsOn": [
            "[concat('Microsoft.Cache/Redis/', parameters('redisCacheName'))]"
          ],
          "properties": {
            "status": "[parameters('diagnosticsStatus')]",
            "storageAccountName": "[parameters('existingDiagnosticsStorageAccountName')]"
          }
        }
      ]
    }



## <a name="commands-to-run-deployment"></a>Befehle zum Ausführen der Bereitstellung
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell
    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-redis-cache/azuredeploy.json -ResourceGroupName ExampleDeployGroup -redisCacheName ExampleCache

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle
    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-redis-cache/azuredeploy.json -g ExampleDeployGroup





<!--HONumber=Jan17_HO4-->


