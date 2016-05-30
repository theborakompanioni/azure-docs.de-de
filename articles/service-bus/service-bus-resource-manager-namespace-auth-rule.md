<properties
    pageTitle="Erstellen einer Service Bus-Autorisierungsregel anhand einer Azure Resource Manager-Vorlage | Microsoft Azure"
    description="Erstellen einer Service Bus-Autorisierungsregel für Namespaces und Warteschlangen mithilfe einer Azure Resource Manager-Vorlage"
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

# Erstellen einer Service Bus-Autorisierungsregel für Namespaces und Warteschlangen mithilfe einer Azure Resource Manager-Vorlage

In diesem Artikel wird beschrieben, wie Sie eine Azure Resource Manager-Vorlage verwenden, die eine Autorisierungsregel für einen Service Bus-Namespace und eine Service Bus-Warteschlange erstellt. Sie erfahren, wie Sie definieren, welche Ressourcen bereitgestellt werden und wie Sie Parameter definieren, die angegeben werden, wenn die Bereitstellung ausgeführt wird. Sie können diese Vorlage für Ihre eigenen Bereitstellungen verwenden oder an Ihre Anforderungen anpassen.

Weitere Informationen zum Erstellen von Vorlagen finden Sie unter [Erstellen von Azure-Ressourcen-Manager-Vorlagen][].

Die vollständige Vorlage finden Sie unter [Service Bus auth rule template][] (Service Bus-Autorisierungsregelvorlage) auf GitHub.

>[AZURE.NOTE] Die folgenden Azure Resource Manager-Vorlagen sind zum Download und zur Bereitstellung verfügbar.
>
>-    [Create a Service Bus namespace with an Event Hub and consumer group (Erstellen eines Service Bus-Namespace mit einem Event Hub und einer Consumergruppe)](service-bus-resource-manager-namespace-event-hub.md)
>-    [Create a Service Bus namespace and a queue using an Azure Resource Manager template (Erstellen eines Service Bus-Namespace und einer Warteschlange mit einer Azure Resource Manager-Vorlage)](service-bus-resource-manager-namespace-queue.md)
>-    [Create a Service Bus namespace with topic and subscription using an Azure Resource Manager template (Erstellen eines Service Bus-Namespace mit Thema und Abonnement mit einer Azure Resource Manager-Vorlage)](service-bus-resource-manager-namespace-topic.md)
>-    [Erstellen eines Service Bus-Namespaces](service-bus-resource-manager-namespace.md)
>
>Um die neuesten Vorlagen zu finden, rufen Sie die [Azure-Schnellstartvorlagen][] auf, und suchen Sie nach „Service Bus“.

## Was möchten Sie bereitstellen?

Mit dieser Vorlage stellen Sie eine Service Bus-Autorisierungsregel für einen Namespace und eine Messagingentität (in diesem Fall eine Warteschlange) bereit.

Diese Vorlage verwendet Shared Access Signature (SAS) für die Authentifizierung. SAS ermöglicht Anwendungen die Authentifizierung bei Service Bus mithilfe eines Zugriffsschlüssels, der für den Namespace oder für die Messagingentität (Warteschlange oder Thema) konfiguriert wird, welcher bestimmte Rechte zugeordnet sind. Sie können diesen Schlüssel zum Generieren eines SAS-Tokens verwenden, das Clients wiederum für die Authentifizierung bei Service Bus verwenden können.

Klicken Sie auf folgende Schaltfläche, um die Bereitstellung automatisch auszuführen:

[![Bereitstellen in Azure](./media/service-bus-resource-manager-namespace-auth-rule/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F301-servicebus-create-authrule-namespace-and-queue%2Fazuredeploy.json)

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

### namespaceAuthorizationRuleName 

Der Name der Autorisierungsregel für den Namespace.

```
"namespaceAuthorizationRuleName ": {
"type": "string"
}
```

### serviceBusQueueName

Der Name der Warteschlange im Service Bus-Namespace.

```
"serviceBusQueueName": {
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

Erstellt einen standardmäßigen Service Bus-Namespace des Typs **Messaging** und eine Service Bus-Autorisierungsregel für Namespace und Entität.

```
"resources": [
        {
            "apiVersion": "[variables('sbVersion')]",
            "name": "[parameters('serviceBusNamespaceName')]",
            "type": "Microsoft.ServiceBus/namespaces",
            "location": "[variables('location')]",
            "kind": "Messaging",
            "sku": {
                "name": "StandardSku",
                "tier": "Standard"
            },
            "resources": [
                {
                    "apiVersion": "[variables('sbVersion')]",
                    "name": "[parameters('serviceBusQueueName')]",
                    "type": "Queues",
                    "dependsOn": [
                        "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
                    ],
                    "properties": {
                        "path": "[parameters('serviceBusQueueName')]"
                    },
                    "resources": [
                        {
                            "apiVersion": "[variables('sbVersion')]",
                            "name": "[parameters('queueAuthorizationRuleName')]",
                            "type": "authorizationRules",
                            "dependsOn": [
                                "[parameters('serviceBusQueueName')]"
                            ],
                            "properties": {
                                "Rights": ["Listen"]
                            }
                        }
                    ]
                }
            ]
        }, {
            "apiVersion": "[variables('sbVersion')]",
            "name": "[variables('namespaceAuthRuleName')]",
            "type": "Microsoft.ServiceBus/namespaces/authorizationRules",
            "dependsOn": ["[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"],
            "location": "[resourceGroup().location]",
            "properties": {
                "Rights": ["Send"]
            }
        }
    ]
```

## Befehle zum Ausführen der Bereitstellung

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### PowerShell

```
New-AzureRmResourceGroupDeployment -ResourceGroupName <resource-group-name> -TemplateFile <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/301-servicebus-create-authrule-namespace-and-queue/azuredeploy.json>
```

## Azure-Befehlszeilenschnittstelle

```
azure config mode arm

azure group deployment create <my-resource-group> <my-deployment-name> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/301-servicebus-create-authrule-namespace-and-queue/azuredeploy.json>
```

## Nächste Schritte

Da Sie nun Ressourcen mit Azure Resource Manager erstellt und bereitgestellt haben, lesen Sie die folgenden Artikel, und erfahren Sie, wie Sie diese Ressourcen verwalten können:

- [Verwalten von Azure Service Bus mithilfe von Azure Automation](service-bus-automation-manage.md)
- [Verwalten von Service Bus mit PowerShell](service-bus-powershell-how-to-provision.md)
- [Service Bus Explorer](https://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)
- [Service Bus-Authentifizierung und -Autorisierung](service-bus-authentication-and-authorization.md)

  [Erstellen von Azure-Ressourcen-Manager-Vorlagen]: ../resource-group-authoring-templates.md
  [Azure-Schnellstartvorlagen]: https://azure.microsoft.com/documentation/templates/
  [Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
  [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
  [Service Bus auth rule template]: https://github.com/Azure/azure-quickstart-templates/blob/master/301-servicebus-create-authrule-namespace-and-queue/

<!---HONumber=AcomDC_0518_2016-->