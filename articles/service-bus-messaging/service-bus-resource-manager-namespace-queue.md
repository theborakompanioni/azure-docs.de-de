<properties
    pageTitle="Erstellen eines Service Bus-Namespace mit Warteschlange anhand einer Azure Resource Manager-Vorlage | Microsoft Azure"
    description="Erstellen eines Service Bus-Namespace und einer Warteschlange mithilfe einer Azure Resource Manager-Vorlage"
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
    ms.date="07/11/2016"
    ms.author="sethm;shvija"/>


# <a name="create-a-service-bus-namespace-and-a-queue-using-an-azure-resource-manager-template"></a>Erstellen eines Service Bus-Namespace und einer Warteschlange mithilfe einer Azure Resource Manager-Vorlage

In diesem Artikel wird beschrieben, wie Sie eine Azure Resource Manager-Vorlage verwenden, die einen Service Bus-Namespace und eine Warteschlange erstellt. Sie erfahren, wie Sie definieren, welche Ressourcen bereitgestellt werden und wie Sie Parameter definieren, die angegeben werden, wenn die Bereitstellung ausgeführt wird. Sie können diese Vorlage für Ihre eigenen Bereitstellungen verwenden oder an Ihre Anforderungen anpassen.

Weitere Informationen zum Erstellen von Vorlagen finden Sie unter [Erstellen von Azure Resource Manager-Vorlagen][].

Die vollständige Vorlage finden Sie unter [Service Bus namespace and queue template][] (Service Bus-Namespace- und Warteschlangenvorlage) auf GitHub.

>[AZURE.NOTE] Die folgenden Azure Resource Manager-Vorlagen sind zum Download und zur Bereitstellung verfügbar.
>
>-    [Create a Service Bus authorization rule for namespace and queue using an Azure Resource Manager template (Erstellen einer Service Bus-Autorisierungsregel für Namespace und Warteschlange mit einer Azure Resource Manager-Vorlage)](service-bus-resource-manager-namespace-auth-rule.md)
>-    [Create a Service Bus namespace with topic and subscription using an Azure Resource Manager template (Erstellen eines Service Bus-Namespace mit Thema und Abonnement mit einer Azure Resource Manager-Vorlage)](service-bus-resource-manager-namespace-topic.md)
>-    [Erstellen eines Service Bus-Namespaces](service-bus-resource-manager-namespace.md)
>-    [Erstellen eines Event Hubs-Namespace mit Event Hub und einer Consumergruppe mithilfe einer Azure Resource Manager-Vorlage](../event-hubs/event-hubs-resource-manager-namespace-event-hub.md)
>
>Um die neuesten Vorlagen zu finden, rufen Sie den Katalog [Azure-Schnellstartvorlagen][] auf, und suchen Sie nach „Service Bus“.

## <a name="what-will-you-deploy?"></a>Was möchten Sie bereitstellen?

Mit dieser Vorlage können Sie einen Service Bus-Namespace mit einer Warteschlange bereitstellen.

[Service Bus-Warteschlangen](service-bus-queues-topics-subscriptions.md#queues) bieten eine FIFO-Nachrichtenzustellung (First In, First Out) an einen Consumer oder an mehrere konkurrierende Consumer.

Klicken Sie auf folgende Schaltfläche, um die Bereitstellung automatisch auszuführen:

[![In Azure bereitstellen](./media/service-bus-resource-manager-namespace-queue/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-queue%2Fazuredeploy.json)

## <a name="parameters"></a>Parameter

Mit dem Azure-Ressourcen-Manager definieren Sie die Parameter für Werte, die Sie bei der Bereitstellung der Vorlage angeben möchten. Die Vorlage enthält einen Abschnitt namens `Parameters` , der alle Parameterwerte enthält. Definieren Sie einen Parameter für die Werte, die basierend auf dem bereitgestellten Projekt oder der bereitgestellten Umgebung variieren. Definieren Sie keine Parameter für Werte, die sich nicht ändern. Jeder Parameterwert wird in der Vorlage verwendet, um die bereitgestellten Ressourcen zu definieren.

Die Vorlage definiert die folgenden Parameter:

### <a name="servicebusnamespacename"></a>serviceBusNamespaceName

Der Name des zu erstellenden Service Bus-Namespace.

```
"serviceBusNamespaceName": {
"type": "string",
"metadata": { 
    "description": "Name of the Service Bus namespace" 
    }
}
```

### <a name="servicebusqueuename"></a>serviceBusQueueName

Der Name der Warteschlange im Service Bus-Namespace.

```
"serviceBusQueueName": {
"type": "string"
}
```

### <a name="servicebusapiversion"></a>serviceBusApiVersion

Die Service Bus-API-Version der Vorlage.

```
"serviceBusApiVersion": {
"type": "string"
}
```

## <a name="resources-to-deploy"></a>Bereitzustellende Ressourcen

Erstellt einen standardmäßigen Service Bus-Namespace des Typs **Messaging**mit einer Warteschlange.

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
            "name": "[parameters('serviceBusQueueName')]",
            "type": "Queues",
            "dependsOn": [
                "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
            ],
            "properties": {
                "path": "[parameters('serviceBusQueueName')]",
            }
        }]
    }]
```

## <a name="commands-to-run-deployment"></a>Befehle zum Ausführen der Bereitstellung

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShell

```
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-queue/azuredeploy.json>
```

## <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-queue/azuredeploy.json>
```

## <a name="next-steps"></a>Nächste Schritte

Da Sie nun Ressourcen mit Azure Resource Manager erstellt und bereitgestellt haben, lesen Sie die folgenden Artikel, und erfahren Sie, wie Sie diese Ressourcen verwalten können:

- [Verwalten von Service Bus mit PowerShell](service-bus-powershell-how-to-provision.md)
- [Service Bus Explorer](https://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)


  [Erstellen von Azure Resource Manager-Vorlagen]: ../resource-group-authoring-templates.md
  [Service Bus namespace and queue template]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/
  [Azure-Schnellstartvorlagen]: https://azure.microsoft.com/documentation/templates/?term=service+bus
  [Weitere Informationen zu Service Bus-Warteschlangen]: service-bus-queues-topics-subscriptions.md
  [Verwenden von Windows PowerShell mit dem Azure Resource Manager]: ../powershell-azure-resource-manager.md
  [Verwalten von Azure-Ressourcen und -Ressourcengruppen mithilfe der Azure-Befehlszeilenschnittstelle]: ../xplat-cli-azure-resource-manager.md



<!--HONumber=Oct16_HO2-->


