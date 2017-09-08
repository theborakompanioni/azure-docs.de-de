---
title: Erstellen eines Service Bus-Namespace mithilfe einer Azure Resource Manager-Vorlage | Microsoft-Dokumentation
description: Verwenden einer Azure Resource Manager-Vorlage zum Erstellen eines Service Bus-Namespace
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: dc0d6482-6344-4cef-8644-d4573639f5e4
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 08/07/2017
ms.author: sethm;shvija
ms.translationtype: HT
ms.sourcegitcommit: 1c730c65194e169121e3ad1d1423963ee3ced8da
ms.openlocfilehash: 0598ee93a38c07aa7b1102cdaf228c2a4b4dcf71
ms.contentlocale: de-de
ms.lasthandoff: 08/30/2017

---

# <a name="create-a-service-bus-namespace-using-an-azure-resource-manager-template"></a>Erstellen eines Service Bus-Namespace mithilfe einer Azure Resource Manager-Vorlage

In diesem Artikel wird beschrieben, wie Sie eine Azure Resource Manager-Vorlage verwenden, die einen Service Bus-Namespace vom Typ **Messaging** mit einer Standard-SKU erstellt. Der Artikel definiert auch die Parameter, die für die Ausführung der Bereitstellung angegeben werden. Sie können diese Vorlage für Ihre eigenen Bereitstellungen verwenden oder an Ihre Anforderungen anpassen.

Weitere Informationen zum Erstellen von Vorlagen finden Sie unter [Erstellen von Azure Resource Manager-Vorlagen][Authoring Azure Resource Manager templates].

Die vollständige Vorlage finden Sie unter [Service Bus namespace template][Service Bus namespace template] (Service Bus-Namespacevorlage) auf GitHub.

> [!NOTE]
> Die folgenden Azure Resource Manager-Vorlagen sind zum Download und zur Bereitstellung verfügbar. 
> 
> * [Create a Service Bus namespace and a queue using an Azure Resource Manager template (Erstellen eines Service Bus-Namespace und einer Warteschlange mit einer Azure Resource Manager-Vorlage)](service-bus-resource-manager-namespace-queue.md)
> * [Create a Service Bus namespace with topic and subscription using an Azure Resource Manager template (Erstellen eines Service Bus-Namespace mit Thema und Abonnement mit einer Azure Resource Manager-Vorlage)](service-bus-resource-manager-namespace-topic.md)
> * [Create a Service Bus authorization rule for namespace and queue using an Azure Resource Manager template (Erstellen einer Service Bus-Autorisierungsregel für Namespace und Warteschlange mit einer Azure Resource Manager-Vorlage)](service-bus-resource-manager-namespace-auth-rule.md)
> * [Create a Service Bus namespace with topic, subscription, and rule (Erstellen eines Service Bus-Namespace mit Thema, Abonnement und Regel)](service-bus-resource-manager-namespace-topic-with-rule.md)
> 
> Die neuesten Vorlagen finden Sie, indem Sie im Katalog [Azure-Schnellstartvorlagen][Azure Quickstart Templates] nach „Service Bus“ suchen.
> 
> 

## <a name="what-will-you-deploy"></a>Was möchten Sie bereitstellen?
Mit dieser Vorlage können Sie einen Service Bus-Namespace mit einer [Standard- oder Premium-SKU](https://azure.microsoft.com/pricing/details/service-bus/) bereitstellen.

Klicken Sie auf folgende Schaltfläche, um die Bereitstellung automatisch auszuführen:

[![In Azure bereitstellen](./media/service-bus-resource-manager-namespace/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-servicebus-create-namespace%2Fazuredeploy.json)

## <a name="parameters"></a>Parameter
Mit dem Azure-Ressourcen-Manager definieren Sie die Parameter für Werte, die Sie bei der Bereitstellung der Vorlage angeben möchten. Die Vorlage enthält einen Abschnitt namens `Parameters` , der alle Parameterwerte enthält. Definieren Sie einen Parameter für die Werte, die basierend auf dem bereitgestellten Projekt oder der bereitgestellten Umgebung variieren. Definieren Sie keine Parameter für Werte, die sich nicht ändern. Jeder Parameterwert wird in der Vorlage verwendet, um die bereitgestellten Ressourcen zu definieren.

Diese Vorlage definiert die folgenden Parameter.

### <a name="servicebusnamespacename"></a>serviceBusNamespaceName
Der Name des zu erstellenden Service Bus-Namespace.

```json
"serviceBusNamespaceName": {
"type": "string",
"metadata": { 
    "description": "Name of the Service Bus namespace" 
    }
}
```

### <a name="servicebussku"></a>serviceBusSKU
Der Name der zu erstellenden Service Bus- [SKU](https://azure.microsoft.com/pricing/details/service-bus/) .

```json
"serviceBusSku": { 
    "type": "string", 
    "allowedValues": [ 
        "Standard",
        "Premium" 
    ], 
    "defaultValue": "Standard", 
    "metadata": { 
        "description": "The messaging tier for service Bus namespace" 
    } 

```

Die Vorlage definiert die Werte, die für diesen Parameter zulässig sind (Standard oder Premium), und weist einen Standardwert (Standard) zu, wenn kein Wert angegeben wird.

Weitere Informationen zu Service Bus-Preisen finden Sie unter [Service Bus – Preise und Abrechnung][Service Bus pricing and billing].

### <a name="servicebusapiversion"></a>serviceBusApiVersion
Die Service Bus-API-Version der Vorlage.

```json
"serviceBusApiVersion": { 
       "type": "string", 
       "defaultValue": "2015-08-01", 
       "metadata": { 
           "description": "Service Bus ApiVersion used by the template" 
       } 
```

## <a name="resources-to-deploy"></a>Bereitzustellende Ressourcen
### <a name="service-bus-namespace"></a>Service Bus-Namespace
Erstellt einen standardmäßigen Service Bus-Namespace des Typs **Messaging**.

```json
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

## <a name="commands-to-run-deployment"></a>Befehle zum Ausführen der Bereitstellung
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell
```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName <resource-group-name> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-servicebus-create-namespace/azuredeploy.json
```

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle
```azurecli
azure config mode arm

azure group deployment create <my-resource-group> <my-deployment-name> --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-servicebus-create-namespace/azuredeploy.json
```

## <a name="next-steps"></a>Nächste Schritte
Da Sie nun Ressourcen mit Azure Resource Manager erstellt und bereitgestellt haben, lesen Sie die folgenden Artikel, und erfahren Sie, wie Sie diese Ressourcen verwalten können:

* [Verwalten von Service Bus mit PowerShell](service-bus-manage-with-ps.md)
* [Service Bus Explorer](https://github.com/paolosalvatori/ServiceBusExplorer/releases)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Service Bus namespace template]: https://github.com/Azure/azure-quickstart-templates/blob/master/101-servicebus-create-namespace/
[Azure Quickstart Templates]: https://azure.microsoft.com/documentation/templates/?term=service+bus
[Service Bus pricing and billing]: service-bus-pricing-billing.md
[Using Azure PowerShell with Azure Resource Manager]: ../azure-resource-manager/powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md

