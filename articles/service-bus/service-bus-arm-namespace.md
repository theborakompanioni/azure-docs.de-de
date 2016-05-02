<properties
    pageTitle="Erstellen eines Service Bus-Namespace anhand einer ARM-Vorlage | Microsoft Azure"
    description="Verwenden einer Azure Resource Manager-Vorlage zum Erstellen eines Service Bus-Namespace"
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

# Erstellen eines Service Bus-Namespace anhand einer ARM-Vorlage

In diesem Artikel wird beschrieben, wie Sie eine Vorlage von Azure Resource Manager (ARM) verwenden, die einen Service Bus-„Messaging“-Namespace mit einer Standard/Basic-SKU erstellt. Der Artikel definiert auch die Parameter, die für die Ausführung der Bereitstellung angegeben werden. Sie können diese Vorlage für Ihre eigenen Bereitstellungen verwenden oder an Ihre Anforderungen anpassen.

Weitere Informationen zum Erstellen von Vorlagen finden Sie unter [Erstellen von Azure-Ressourcen-Manager-Vorlagen][].

Die vollständige Vorlage finden Sie unter [Service Bus namespace template][] \(Service Bus-Namespace-Vorlage) auf GitHub.

>[AZURE.NOTE] Die folgenden ARM-Vorlagen stehen zum Herunterladen und Bereitstellen zur Verfügung.
>
>-    [Create a Service Bus namespace with an Event Hub and consumer group](service-bus-arm-namespace-event-hub.md) (Erstellen eines Service Bus-Namespace mit einem Event Hub und einer Consumergruppe)
>-    [Create a Service Bus namespace and a queue using an Azure Resource Manager template](service-bus-arm-namespace-queue.md) (Erstellen eines Service Bus-Namespace und einer Warteschlange mit einer Azure Resource Manager-Vorlage)
>-    [Create a Service Bus namespace with topic and subscription using an Azure Resource Manager template](service-bus-arm-namespace-topic.md) (Erstellen eines Service Bus-Namespace mit Thema und Abonnement mit einer Azure Resource Manager-Vorlage)
>-    [Create a Service Bus authorization rule for namespace and queue using an Azure Resource Manager template](service-bus-arm-namespace-auth-rule.md) (Erstellen einer Service Bus-Autorisierungsregel für Namespace und Warteschlange mit einer Azure Resource Manager-Vorlage)
>
>Um die neuesten Vorlagen zu finden, rufen Sie die [Azure-Schnellstartvorlagen][] auf, und suchen Sie nach „Service Bus“.

## Was möchten Sie bereitstellen?

Mit dieser Vorlage können Sie einen Service Bus-Namespace mit einer [Basic- oder Standard](https://azure.microsoft.com/pricing/details/service-bus/)-SKU bereitstellen.

Klicken Sie auf folgende Schaltfläche, um die Bereitstellung automatisch auszuführen:

[![Bereitstellen in Azure](./media/service-bus-arm-namespace/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-servicebus-create-namespace%2Fazuredeploy.json)

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

### serviceBusSKU

Der Name der zu erstellenden Service Bus-[SKU](https://azure.microsoft.com/pricing/details/service-bus/).

```
"serviceBusSKU": {
"type": "string",
"allowedValues": ["Basic","Standard"],
"defaultValue": "Standard"
}
```

Die Vorlage definiert die Werte, die für diesen Parameter zulässig sind (Basic oder Standard), und weist einen Standardwert (Standard) zu, wenn kein Wert angegeben wird.

Es wird eine Basisgebühr von 10 US-Dollar pro Monat für die Ebene "Standard" erhoben, die Ihnen ermöglicht, bis zu 12,5 Millionen Vorgänge pro Monat ohne zusätzliche Kosten auszuführen. Die Basic-Stufe kostet 0,05 $ pro Million Vorgänge.

Weitere Informationen zu Service Bus-Preisen finden Sie unter [Service Bus – Preise und Abrechnung][].

### serviceBusApiVersion

Die Service Bus-API-Version der Vorlage.

```
"serviceBusApiVersion": {

"type": "string"

}
```

## Bereitzustellende Ressourcen

### Service Bus-Namespace

Erstellt einen standardmäßigen Service Bus-Namespace des Typs **Messaging**.

```
"resources": [
    {
        "apiVersion": "[parameters('serviceBusApiVersion')]",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/Namespaces",
        "location": "[variables('location')]",
        "kind": "Messaging",
        "sku": {
            "name": "StandardSku",
            "tier": "Standard"
        },
        "properties": {
        }
    }
]
```

## Befehle zum Ausführen der Bereitstellung

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### PowerShell

```
New-AzureRmResourceGroupDeployment -ResourceGroupName <resource-group-name> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-servicebus-create-namespace/azuredeploy.json
```

### Azure-Befehlszeilenschnittstelle

```
azure config mode arm

azure group deployment create <my-resource-group> <my-deployment-name> --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-servicebus-create-namespace/azuredeploy.json
```

## Nächste Schritte

Da Sie nun Ressourcen mit ARM erstellt und bereitgestellt haben, erfahren Sie, wie Sie diese Ressourcen verwalten können, in diesen Artikeln:

- [Verwalten von Azure Service Bus mithilfe von Azure Automation](service-bus-automation-manage.md)
- [Verwalten von Service Bus mit PowerShell](service-bus-powershell-how-to-provision.md)
- [Service Bus Explorer](https://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)

  [Erstellen von Azure-Ressourcen-Manager-Vorlagen]: ../resource-group-authoring-templates.md
  [Service Bus namespace template]: https://github.com/Azure/azure-quickstart-templates/blob/master/101-servicebus-create-namespace/
  [Azure-Schnellstartvorlagen]: https://azure.microsoft.com/documentation/templates/
  [Service Bus – Preise und Abrechnung]: https://azure.microsoft.com/documentation/articles/service-bus-pricing-billing/
  [Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
  [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md

<!---HONumber=AcomDC_0420_2016-->