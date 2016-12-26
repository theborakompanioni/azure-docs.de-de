---
title: Erstellen eines Event Hubs-Namespace mit Event Hub und einer Consumergruppe mithilfe einer Azure Resource Manager-Vorlage | Microsoft Docs
description: Erstellen eines Event Hubs-Namespace mit Event Hub und einer Consumergruppe mithilfe einer Azure Resource Manager-Vorlage
services: event-hubs
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 28bb4591-1fd7-444f-a327-4e67e8878798
ms.service: event-hubs
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 11/21/2016
ms.author: sethm;shvija
translationtype: Human Translation
ms.sourcegitcommit: a9e31954983fa673258917785a5bd828f1970fa1
ms.openlocfilehash: 92ec109c6cf9e3a2792ed68dfd96f86f5f9ae339


---
# <a name="create-an-event-hubs-namespace-with-event-hub-and-consumer-group-using-an-azure-resource-manager-template"></a>Erstellen eines Event Hubs-Namespace mit Event Hub und einer Consumergruppe mithilfe einer Azure Resource Manager-Vorlage
In diesem Artikel wird beschrieben, wie Sie eine Azure Resource Manager-Vorlage verwenden, die einen Event Hub-Namespace mit einem Event Hub und einer Consumergruppe erstellt. Sie erfahren, wie Sie definieren, welche Ressourcen bereitgestellt werden und wie Sie Parameter definieren, die angegeben werden, wenn die Bereitstellung ausgeführt wird. Sie können diese Vorlage für Ihre eigenen Bereitstellungen verwenden oder Ihren Anforderungen anpassen.

Weitere Informationen zum Erstellen von Vorlagen finden Sie unter [Erstellen von Azure Resource Manager-Vorlagen][Erstellen von Azure Resource Manager-Vorlagen].

Die vollständige Vorlage finden Sie auf GitHub unter [Event Hub- und Consumergruppenvorlage][Event Hub- und Consumergruppenvorlage].

> [!NOTE]
> Um die neuesten Vorlagen zu finden, rufen Sie den Katalog [Azure-Schnellstartvorlagen][Azure-Schnellstartvorlagen] auf und suchen nach „Event Hubs“.
> 
> 

## <a name="what-will-you-deploy"></a>Was möchten Sie bereitstellen?
Mit dieser Vorlage können Sie einen Event Hubs-Namespace mit einem Event Hub und einer Consumergruppe bereitstellen.

[Event Hubs](event-hubs-what-is-event-hubs.md) ist ein Dienst zur Ereignisverarbeitung, der riesige Mengen an Ereignis- und Telemetriedaten in Azure erfassen kann und gleichzeitig eine niedrige Latenz und hohe Zuverlässigkeit bietet.

Klicken Sie auf folgende Schaltfläche, um die Bereitstellung automatisch auszuführen:

[![In Azure bereitstellen](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-event-hubs-create-event-hub-and-consumer-group%2Fazuredeploy.json)

## <a name="parameters"></a>Parameter
Mit dem Azure-Ressourcen-Manager definieren Sie die Parameter für Werte, die Sie bei der Bereitstellung der Vorlage angeben möchten. Die Vorlage enthält einen Abschnitt namens `Parameters` , der alle Parameterwerte enthält. Definieren Sie einen Parameter für die Werte, die basierend auf dem bereitgestellten Projekt oder der bereitgestellten Umgebung variieren. Definieren Sie keine Parameter für Werte, die sich nicht ändern. Jeder Parameterwert in der Vorlage definiert die bereitgestellten Ressourcen.

Die Vorlage definiert die folgenden Parameter:

### <a name="eventhubnamespacename"></a>eventHubNamespaceName
Der Name des zu erstellenden Event Hubs-Namespace.

```json
"eventHubNamespaceName": {
"type": "string"
}
```

### <a name="eventhubname"></a>eventHubName
Der Name des im Event Hubs-Namespace erstellten Event Hubs.

```json
"eventHubName": {
"type": "string"
}
```

### <a name="eventhubconsumergroupname"></a>eventHubConsumerGroupName
Der Name der Consumergruppe, die für den Event Hub erstellt wurde.

```json
"eventHubConsumerGroupName": {
"type": "string"
}
```

### <a name="apiversion"></a>apiVersion
Die API-Version der Vorlage.

```json
"apiVersion": {
"type": "string"
}
```

## <a name="resources-to-deploy"></a>Bereitzustellende Ressourcen
Erstellt einen Namespace des Typs **EventHubs**mit einem Event Hub und einer Consumergruppe.

```json
"resources":[  
      {  
         "apiVersion":"[variables('ehVersion')]",
         "name":"[parameters('namespaceName')]",
         "type":"Microsoft.EventHub/namespaces",
         "location":"[variables('location')]",
         "sku":{  
            "name":"Standard",
            "tier":"Standard"
         },
         "resources":[  
            {  
               "apiVersion":"[variables('ehVersion')]",
               "name":"[parameters('eventHubName')]",
               "type":"EventHubs",
               "dependsOn":[  
                  "[concat('Microsoft.EventHub/namespaces/', parameters('namespaceName'))]"
               ],
               "properties":{  
                  "path":"[parameters('eventHubName')]"
               },
               "resources":[  
                  {  
                     "apiVersion":"[variables('ehVersion')]",
                     "name":"[parameters('consumerGroupName')]",
                     "type":"ConsumerGroups",
                     "dependsOn":[  
                        "[parameters('eventHubName')]"
                     ],
                     "properties":{  

                     }
                  }
               ]
            }
         ]
      }
   ],
```

## <a name="commands-to-run-deployment"></a>Befehle zum Ausführen der Bereitstellung
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShell
```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-event-hubs-create-event-hub-and-consumer-group/azuredeploy.json
```

## <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle
```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri [https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-event-hubs-create-event-hub-and-consumer-group/azuredeploy.json][]
```

[Erstellen von Azure Resource Manager-Vorlagen]: ../resource-group-authoring-templates.md
[Azure-Schnellstartvorlagen]:  https://azure.microsoft.com/documentation/templates/?term=event+hubs
[Verwenden von Windows PowerShell mit dem Azure Resource Manager]: ../powershell-azure-resource-manager.md
[Verwenden der Azure-Befehlszeilenschnittstelle für Mac, Linux und Windows mit der Azure-Ressourcenverwaltung]: ../xplat-cli-azure-resource-manager.md
[Event Hub- und Consumergruppenvorlage]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-event-hubs-create-event-hub-and-consumer-group/



<!--HONumber=Nov16_HO5-->


