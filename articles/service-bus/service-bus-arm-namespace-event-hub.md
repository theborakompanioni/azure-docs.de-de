<properties
    pageTitle="Erstellen eines Service Bus-Namespace mit einem Event Hub und einer Consumergruppe | Microsoft Azure"
    description="Erstellen eines Service Bus-Namespace mit einem Event Hub und einer Consumergruppe unter Verwendung einer ARM-Vorlage"
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="tbd"
    ms.topic="article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="04/15/2016"
    ms.author="sethm;shvija"/>

# Erstellen eines Service Bus-Namespace mit einem Event Hub und einer Consumergruppe unter Verwendung einer ARM-Vorlage

In diesem Artikel wird beschrieben, wie Sie eine Vorlage von Azure Resource Manager (ARM) verwenden, die einen Service Bus-Namespace mit einem Event Hub und einer Consumergruppe erstellt. Sie erfahren, wie Sie definieren, welche Ressourcen bereitgestellt werden und wie Sie Parameter definieren, die angegeben werden, wenn die Bereitstellung ausgeführt wird. Sie können diese Vorlage für Ihre eigenen Bereitstellungen verwenden oder Ihren Anforderungen anpassen.

Weitere Informationen zum Erstellen von Vorlagen finden Sie unter [Erstellen von Azure-Ressourcen-Manager-Vorlagen][].

Die vollständige Vorlage finden Sie unter [Service Bus Event Hub and consumer group template][] (Service Bus-Event Hub- und Consumergruppenvorlage) auf GitHub.

>[AZURE.NOTE] Die folgenden ARM-Vorlagen stehen zum Herunterladen und Bereitstellen zur Verfügung.
>
>-    [Create a Service Bus authorization rule for namespace and queue using an Azure Resource Manager template](service-bus-arm-namespace-auth-rule.md) (Erstellen einer Service Bus-Autorisierungsregel für Namespace und Warteschlange mit einer Azure Resource Manager-Vorlage)
>-    [Create a Service Bus namespace and a queue using an Azure Resource Manager template](service-bus-arm-namespace-queue.md) (Erstellen eines Service Bus-Namespace und einer Warteschlange mit einer Azure Resource Manager-Vorlage)
>-    [Create a Service Bus namespace with topic and subscription using an Azure Resource Manager template](service-bus-arm-namespace-topic.md) (Erstellen eines Service Bus-Namespace mit Thema und Abonnement mit einer Azure Resource Manager-Vorlage)
>-    [Erstellen eines Service Bus-Namespaces](service-bus-arm-namespace.md)
>
>Um die neuesten Vorlagen zu finden, rufen Sie die [Azure-Schnellstartvorlagen][] auf, und suchen Sie nach „Service Bus“.

## Was möchten Sie bereitstellen?

Mit dieser Vorlage können Sie einen Service Bus-Namespace mit einem Event Hub und einer Consumergruppe bereitstellen.

Event Hubs ist ein Dienst zur Ereignisverarbeitung, der riesige Mengen an Ereignis- und Telemetriedaten in Azure erfassen kann und gleichzeitig eine niedrige Latenz und hohe Zuverlässigkeit bietet.

Weitere Informationen zu [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md).

Klicken Sie auf folgende Schaltfläche, um die Bereitstellung automatisch auszuführen:

[![Bereitstellen in Azure](./media/service-bus-arm-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-eventhub-and-consumergroup%2Fazuredeploy.json)

## Parameter

Mit dem Azure-Ressourcen-Manager definieren Sie die Parameter für Werte, die Sie bei der Bereitstellung der Vorlage angeben möchten. Die Vorlage enthält einen Abschnitt namens `Parameters`, der alle Parameterwerte enthält. Definieren Sie einen Parameter für die Werte, die basierend auf dem bereitgestellten Projekt oder der bereitgestellten Umgebung variieren. Definieren Sie keine Parameter für Werte, die sich nicht ändern. Jeder Parameterwert wird in der Vorlage verwendet, um die bereitgestellten Ressourcen zu definieren.

Jeder Parameter wird in der Vorlage beschrieben.

### serviceBusNamespaceName

Der Name des zu erstellenden Service Bus-Namespace.

```
"serviceBusNamespaceName": {
"type": "string"
}
```

### serviceBusEventHubName

Der Name des im Service Bus-Namespace erstellten Event Hubs.

```
" serviceBusEventHubName": {
"type": "string"
}
```

### serviceBusConsumerGroupName

Der Name der Consumergruppe, die für den Event Hub im Service Bus-Namespace erstellt wurde.

```
" serviceBusConsumerGroupName": {
"type": "string"
}
```

### serviceBusApiVersion

Die Service Bus-API-Version der Vorlage.

```
"serviceBusApiVersion": {
"type": "string"
}
```

## Bereitzustellende Ressourcen

Erstellt einen Service Bus-Namespace des Typs **Event Hub** mit einem Event Hub und einer Consumergruppe.

```
"resources": [
        {
            "apiVersion": "[variables('ehVersion')]",
            "name": "[parameters('serviceBusNamespaceName')]",
            "type": "Microsoft.ServiceBus/Namespaces",
            "location": "[variables('location')]",
            "kind": "EventHub",
            "sku": {
                "name": "StandardSku",
                "tier": "Standard"
            },
            "resources": [
                {
                    "apiVersion": "[variables('ehVersion')]",
                    "name": "[parameters('serviceBusEventHubName')]",
                    "type": "EventHubs",
                    "dependsOn": [
                        "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
                    ],
                    "properties": {
                        "path": "[parameters('serviceBusEventHubName')]"
                    },
                    "resources": [
                        {
                            "apiVersion": "[variables('ehVersion')]",
                            "name": "[parameters('serviceBusConsumerGroupName')]",
                            "type": "ConsumerGroups",
                            "dependsOn": [
                                "[parameters('serviceBusEventHubName')]"
                            ],
                            "properties": {
                            }
                        }
                    ]
                }
            ]
        }
    ]
```

## Befehle zum Ausführen der Bereitstellung

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## PowerShell

```
New-AzureRmResourceGroupDeployment -ResourceGroupName <resource-group-name> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-eventhub-and-consumergroup/azuredeploy.json
```

## Azure-Befehlszeilenschnittstelle

```
azure config mode arm

azure group deployment create <my-resource-group> <my-deployment-name> --template-uri [https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-eventhub-and-consumergroup/azuredeploy.json][]
```

## Nächste Schritte

Da Sie nun Ressourcen mit ARM erstellt und bereitgestellt haben, erfahren Sie, wie Sie diese Ressourcen verwalten können, in diesen Artikeln:

- [Verwalten von Azure Service Bus mithilfe von Azure Automation](service-bus-automation-manage.md)
- [Verwalten von Service Bus mit PowerShell](service-bus-powershell-how-to-provision.md)
- [Service Bus Explorer](https://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)

  [Erstellen von Azure-Ressourcen-Manager-Vorlagen]: ../resource-group-authoring-templates.md
  [Azure-Schnellstartvorlagen]: https://azure.microsoft.com/documentation/templates/
  [Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
  [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
  [Service Bus Event Hub and consumer group template]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-eventhub-and-consumerGroup/

<!---HONumber=AcomDC_0420_2016-->