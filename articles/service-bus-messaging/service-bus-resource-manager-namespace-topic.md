<properties
    pageTitle="Erstellen eines Service Bus-Namespace mit einem Thema und einem Abonnement anhand einer Azure Resource Manager-Vorlage | Microsoft Azure"
    description="Erstellen eines Service Bus-Namespace mit einem Thema und einem Abonnement anhand einer Azure Resource Manager-Vorlage"
    services="service-bus-messaging"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus-messaging"
    ms.devlang="tbd"
    ms.topic="article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="07/11/2016"
    ms.author="sethm;shvija"/>

# Erstellen eines Service Bus-Namespace mit einem Thema und einem Abonnement anhand einer Azure Resource Manager-Vorlage

In diesem Artikel wird beschrieben, wie Sie eine Azure Resource Manager-Vorlage verwenden, die einen Service Bus-Namespace mit einem Thema und einem Abonnement erstellt. Sie erfahren, wie Sie definieren, welche Ressourcen bereitgestellt werden und wie Sie Parameter definieren, die angegeben werden, wenn die Bereitstellung ausgeführt wird. Sie können diese Vorlage für Ihre eigenen Bereitstellungen verwenden oder Ihren Anforderungen anpassen.

Weitere Informationen zum Erstellen von Vorlagen finden Sie unter [Erstellen von Azure Resource Manager-Vorlagen][].

Die vollständige Vorlage finden Sie unter [Service Bus namespace with topic and subscription][] (Service Bus-Namespace mit Thema und Abonnement).

>[AZURE.NOTE] Die folgenden Azure Resource Manager-Vorlagen sind zum Download und zur Bereitstellung verfügbar.
>
>-    [Create a Service Bus authorization rule for namespace and queue using an Azure Resource Manager template (Erstellen einer Service Bus-Autorisierungsregel für Namespace und Warteschlange mit einer Azure Resource Manager-Vorlage)](service-bus-resource-manager-namespace-auth-rule.md)
>-    [Create a Service Bus namespace and a queue using an Azure Resource Manager template (Erstellen eines Service Bus-Namespace und einer Warteschlange mit einer Azure Resource Manager-Vorlage)](service-bus-resource-manager-namespace-queue.md)
>-    [Erstellen eines Service Bus-Namespaces](service-bus-resource-manager-namespace.md)
>-    [Create an Event Hubs namespace with an Event Hub and consumer group (Erstellen eines Event Hubs-Namespace mit einem Event Hub und einer Consumergruppe)](../event-hubs/event-hubs-resource-manager-namespace-event-hub.md)
>
>Um die neuesten Vorlagen zu finden, rufen Sie den Katalog [Azure-Schnellstartvorlagen][] auf, und suchen Sie nach „Service Bus“.

## Was möchten Sie bereitstellen?

Mit dieser Vorlage können Sie einen Service Bus-Namespace mit Thema und Abonnement bereitstellen.

[Service Bus-Topics und -Subscriptions](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions) bieten eine 1:n-Kommunikation in einem Muster vom Typ *Veröffentlichen/Abonnieren*.

Klicken Sie auf folgende Schaltfläche, um die Bereitstellung automatisch auszuführen:

[![Bereitstellen in Azure](./media/service-bus-resource-manager-namespace-topic/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-topic-and-subscription%2Fazuredeploy.json)

## Parameter

Mit dem Azure-Ressourcen-Manager definieren Sie die Parameter für Werte, die Sie bei der Bereitstellung der Vorlage angeben möchten. Die Vorlage enthält einen Abschnitt namens `Parameters`, der alle Parameterwerte enthält. Definieren Sie einen Parameter für die Werte, die basierend auf dem bereitgestellten Projekt oder der bereitgestellten Umgebung variieren. Definieren Sie keine Parameter für Werte, die sich nicht ändern. Jeder Parameterwert wird in der Vorlage verwendet, um die bereitgestellten Ressourcen zu definieren.

Die Vorlage definiert die folgenden Parameter:

### serviceBusNamespaceName

Der Name des zu erstellenden Service Bus-Namespace.

```
"serviceBusNamespaceName": {
"type": "string"
}
```

### serviceBusTopicName

Der Name des Themas, das im Service Bus-Namespace erstellt wird.

```
"serviceBusTopicName": {
"type": "string"
}
```

### serviceBusSubscriptionName

Der Name des Abonnements, das im Service Bus-Namespace erstellt wird.

```
"serviceBusSubscriptionName": {
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

Erstellt einen standardmäßigen Service Bus-Namespace des Typs **Messaging** mit Topic und Subscription.

```
"resources ": [{
        "apiVersion": "[variables('sbVersion')]",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/Namespaces",
        "location": "[variables('location')]",
        "kind": "Messaging",
        "sku": {
            "name": "StandardSku",
            "tier": "Standard"
        },
        "resources": [{
            "apiVersion": "[variables('sbVersion')]",
            "name": "[parameters('serviceBusTopicName')]",
            "type": "Topics",
            "dependsOn": [
                "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
            ],
            "properties": {
                "path": "[parameters('serviceBusTopicName')]",
            },
            "resources": [{
                "apiVersion": "[variables('sbVersion')]",
                "name": "[parameters('serviceBusSubscriptionName')]",
                "type": "Subscriptions",
                "dependsOn": [
                    "[parameters('serviceBusTopicName')]"
                ],
                "properties": {}
            }]
        }]
    }]
```

## Befehle zum Ausführen der Bereitstellung

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## PowerShell

```
New-AzureResourceGroupDeployment -Name <deployment-name> -ResourceGroupName <resource-group-name> -TemplateUri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-and-subscription/azuredeploy.json>
```

## Azure-Befehlszeilenschnittstelle

```
azure config mode arm

azure group deployment create <my-resource-group> <my-deployment-name> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-and-subscription/azuredeploy.json>
```

## Nächste Schritte

Da Sie nun Ressourcen mit Azure Resource Manager erstellt und bereitgestellt haben, lesen Sie die folgenden Artikel, und erfahren Sie, wie Sie diese Ressourcen verwalten können:

- [Verwalten von Azure Service Bus mithilfe von Azure Automation](../service-bus/service-bus-automation-manage.md)
- [Verwalten von Service Bus mit PowerShell](../service-bus/service-bus-powershell-how-to-provision.md)
- [Service Bus Explorer](https://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)


  [Erstellen von Azure Resource Manager-Vorlagen]: ../resource-group-authoring-templates.md
  [Azure-Schnellstartvorlagen]: https://azure.microsoft.com/documentation/templates/?term=service+bus
  [Learn more about Service Bus topics and subscriptions]: service-bus-queues-topics-subscriptions.md
  [Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
  [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
  [Service Bus namespace with topic and subscription]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-topic-and-subscription/

<!---HONumber=AcomDC_0928_2016-->