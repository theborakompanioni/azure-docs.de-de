<properties
    pageTitle="Erstellen von Service Bus-Ressourcen mithilfe von Azure Resource Manager-Vorlagen | Microsoft Azure"
    description="Verwenden von Azure Resource Manager-Vorlagen, um die Erstellung von Service Bus-Ressourcen zu automatisieren"
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
    ms.author="sethm"/>

# Erstellen von Service Bus-Ressourcen mithilfe von Azure Resource Manager-Vorlagen

Dieser Artikel erläutert das Erstellen und Bereitstellen von Service Bus- und Event Hubs-Ressourcen mithilfe von Azure Resource Manager-Vorlagen, PowerShell und dem Service Bus-Ressourcenanbieter.

Azure Resource Manager-Vorlagen helfen Ihnen dabei, die für eine Lösung bereitzustellenden Ressourcen zu definieren und die Parameter und Variablen anzugeben, die Sie zur Eingabe von Werten für verschiedene Umgebungen benötigen. Die Vorlage besteht aus JSON und Ausdrücken, mit denen Sie Werte für die Bereitstellung erstellen können. Detaillierte Informationen zum Erstellen von Azure Resource Manager-Vorlagen sowie eine Beschreibung des Vorlagenformats finden Sie unter [Erstellen von Azure Resource Manager-Vorlagen](../resource-group-authoring-templates.md).

>[AZURE.NOTE] Die Beispiele in diesem Artikel zeigen, wie Sie Azure Resource Manager verwenden, um einen Service Bus-Namespace und eine Messagingentität (Warteschlange) zu erstellen. Um Beispiele für andere Vorlagen zu finden, rufen Sie den [Katalog mit Azure-Schnellstartvorlagen][] auf, und suchen Sie nach „Service Bus“.

## Resource Manager-Vorlagen für Service Bus und Event Hubs

Diese Azure Resource Manager-Vorlagen für Service Bus und Event Hubs sind zum Download und zur Bereitstellung verfügbar. Klicken Sie auf die folgenden Links, um Details zu jeder Vorlage abzurufen (auf jeder Seite finden Sie Links zu den Vorlagen in GitHub):

- [Erstellen eines Service Bus-Namespaces](service-bus-resource-manager-namespace.md)
- [Create a Service Bus namespace and a queue using an Azure Resource Manager template (Erstellen eines Service Bus-Namespace und einer Warteschlange mit einer Azure Resource Manager-Vorlage)](service-bus-resource-manager-namespace-queue.md)
- [Create a Service Bus namespace with topic and subscription using an Azure Resource Manager template (Erstellen eines Service Bus-Namespace mit Thema und Abonnement mit einer Azure Resource Manager-Vorlage)](service-bus-resource-manager-namespace-topic.md)
- [Create a Service Bus authorization rule for namespace and queue using an Azure Resource Manager template (Erstellen einer Service Bus-Autorisierungsregel für Namespace und Warteschlange mit einer Azure Resource Manager-Vorlage)](service-bus-resource-manager-namespace-auth-rule.md)
- [Create an Event Hubs namespace with an Event Hub and consumer group (Erstellen eines Event Hubs-Namespace mit einem Event Hub und einer Consumergruppe)](../event-hubs/event-hubs-resource-manager-namespace-event-hub.md)

## Bereitstellen mit PowerShell

Das folgende Verfahren beschreibt die Verwendung von PowerShell, um eine Azure Resource Manager-Vorlage bereitzustellen, die einen Service Bus-Namespace im Tarif **Standard** sowie eine Warteschlange innerhalb dieses Namespace erstellt. Dieses Beispiel basiert auf der Vorlage [Create a Service Bus namespace with queue](https://github.com/Azure/azure-quickstart-templates/tree/master/201-servicebus-create-queue) (Erstellen eines Service Bus-Namespace mit Warteschlange). Der Workflow sieht in etwa folgendermaßen aus:

1. Installieren Sie PowerShell.
2. Erstellen Sie die Vorlage und (optional) eine Parameterdatei.
2. Melden Sie sich in PowerShell bei Ihrem Azure-Konto an.
3. Erstellen Sie eine neue Ressourcengruppe, falls noch keine vorhanden ist.
4. Testen Sie die Bereitstellung.
5. Legen Sie ggf. den Bereitstellungsmodus fest.
6. Stellen Sie die Vorlage bereit.

Vollständige Informationen zur Bereitstellung von Azure Resource Manager-Vorlagen finden Sie unter [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen][].

### Installieren von PowerShell

Installieren Sie Azure PowerShell mithilfe der Anweisungen unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).

### Erstellen einer Vorlage

Klonen oder kopieren Sie die Vorlage [201-servicebus-create-queue](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.json) aus GitHub:

```
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "serviceBusNamespaceName": {
            "type": "string",
            "metadata": {
                "description": "Name of the Service Bus namespace"
            }
        },
        "serviceBusQueueName": {
            "type": "string",
            "metadata": {
                "description": "Name of the Queue"
            }
        },
        "serviceBusApiVersion": {
            "type": "string",
            "defaultValue": "2015-08-01",
            "metadata": {
                "description": "Service Bus ApiVersion used by the template"
            }
        }
    },
    "variables": {
        "location": "[resourceGroup().location]",
        "sbVersion": "[parameters('serviceBusApiVersion')]",
        "defaultSASKeyName": "RootManageSharedAccessKey",
        "authRuleResourceId": "[resourceId('Microsoft.ServiceBus/namespaces/authorizationRules', parameters('serviceBusNamespaceName'), variables('defaultSASKeyName'))]"
    },
    "resources": [{
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
                "path": "[parameters('serviceBusQueueName')]"
            }
        }]
    }],
    "outputs": {
        "NamespaceConnectionString": {
            "type": "string",
            "value": "[listkeys(variables('authRuleResourceId'), variables('sbVersion')).primaryConnectionString]"
        },
        "SharedAccessPolicyPrimaryKey": {
            "type": "string",
            "value": "[listkeys(variables('authRuleResourceId'), variables('sbVersion')).primaryKey]"
        }
    }
}
```

### Erstellen einer Parameterdatei (optional)

Um eine optionale Parameterdatei zu verwenden, kopieren Sie die Datei [201-servicebus-create-queue](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.parameters.json). Ersetzen Sie den Wert von `serviceBusNamespaceName` durch den Namen des Service Bus-Namespace, den Sie in dieser Bereitstellung erstellen möchten, und ersetzen Sie den Wert von `serviceBusQueueName` durch den Namen der Warteschlange, die Sie erstellen möchten.

```
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "serviceBusNamespaceName": {
            "value": "<myNamespaceName>"
        },
        "serviceBusQueueName": {
            "value": "<myQueueName>"
        },
        "serviceBusApiVersion": {
            "value": "2015-08-01"
        }
    }
}
```

Weitere Informationen finden Sie im Thema [Parameterdatei](../resource-group-template-deploy.md#parameter-file).

### Anmelden bei Azure und Festlegen des Azure-Abonnements

Führen Sie an einer PowerShell-Eingabeaufforderung den folgenden Befehl aus:

```
Login-AzureRmAccount
```

Sie werden aufgefordert, sich bei Ihrem Azure-Konto anzumelden. Führen Sie nach der Anmeldung den folgenden Befehl aus, um Ihre verfügbaren Abonnements anzuzeigen.

```
Get-AzureRMSubscription
```

Dieser Befehl gibt eine Liste der verfügbaren Azure-Abonnements zurück. Wählen Sie ein Abonnement für die aktuelle Sitzung aus, indem Sie folgenden Befehl ausführen. Ersetzen Sie `<YourSubscriptionId>` durch die GUID des Azure-Abonnements, das Sie verwenden möchten.

```
Set-AzureRmContext -SubscriptionID <YourSubscriptionId>
```

### Festlegen der Ressourcengruppe

Wenn noch keine Ressourcengruppe vorhanden ist, erstellen Sie mit dem Befehl **New-AzureRmResourceGroup** eine neue Ressourcengruppe. Geben Sie den Namen der gewünschten Ressourcengruppe und den gewünschten Speicherort ein. Beispiel:

```
New-AzureRmResourceGroup -Name MyDemoRG -Location "West US"
```

Nach erfolgreicher Ausführung wird eine Zusammenfassung der neuen Ressourcengruppe angezeigt:

```
ResourceGroupName : MyDemoRG
Location          : westus
ProvisioningState : Succeeded
Tags              :
ResourceId        : /subscriptions/<GUID>/resourceGroups/MyDemoRG
```

### Testen der Bereitstellung

Überprüfen Sie Ihre Bereitstellung, indem Sie das Cmdlet `Test-AzureRmResourceGroupDeployment` ausführen. Geben Sie beim Testen der Bereitstellung die Parameter exakt so an wie beim Ausführen der Bereitstellung.

```
Test-AzureRmResourceGroupDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json
```

### Erstellen der Bereitstellung

Um die neue Bereitstellung zu erstellen, führen Sie den Befehl `New-AzureRmResourceGroupDeployment` aus, und geben Sie bei entsprechender Aufforderung die erforderlichen Parameter an. Die Parameter enthalten einen Namen für Ihre Bereitstellung, den Namen Ihrer Ressourcengruppe und den Pfad oder die URL zur Vorlagendatei. Wenn der Parameter **Mode** nicht angegeben wurde, wird der Standardwert **Incremental** verwendet. Weitere Informationen finden Sie unter [Inkrementelle und vollständige Bereitstellungen](../resource-group-template-deploy.md#incremental-and-complete-deployments).

Der folgende Befehl fordert die Angabe der drei Parameter im PowerShell-Fenster:

```
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json
```

Um stattdessen eine Parameterdatei zu verwenden, geben Sie folgenden Befehl ein.

```
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json -TemplateParameterFile <path to parameters file>\azuredeploy.parameters.json
```

Sie können beim Ausführen des Bereitstellungs-Cmdlets auch Inlineparameter verwenden. Der Befehl lautet wie folgt:

```
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json -parameterName "parameterValue"
```

Legen Sie zum Ausführen einer [vollständigen](../resource-group-template-deploy.md#incremental-and-complete-deployments) Bereitstellung den Parameter **Mode** auf **Complete** fest:

```
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -Mode Complete -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json 
```

### Überprüfen der Bereitstellung

Wenn die Ressourcen erfolgreich bereitgestellt wurden, wird im PowerShell-Fenster eine Zusammenfassung der Bereitstellung angezeigt:

```
DeploymentName    : MyDemoDeployment
ResourceGroupName : MyDemoRG
ProvisioningState : Succeeded
Timestamp         : 4/19/2016 10:38:30 PM
Mode              : Incremental
TemplateLink      :
Parameters        :
                    Name             Type                       Value
                    ===============  =========================  ==========
                    serviceBusNamespaceName  String             <namespaceName>
                    serviceBusQueueName  String                 <queueName>
                    serviceBusApiVersion  String                2015-08-01

```

## Nächste Schritte

Sie haben nun den grundlegenden Workflow und die grundlegenden Befehle für die Bereitstellung einer Azure Resource Manager-Vorlage kennengelernt. Detaillierte Informationen finden Sie unter folgenden Links:

- [Übersicht über den Azure-Ressourcen-Manager][]
- [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen][]
- [Erstellen von Vorlagen](../resource-group-authoring-templates.md)


[Übersicht über den Azure-Ressourcen-Manager]: ../resource-group-overview.md
[Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen]: ../resource-group-template-deploy.md
[Katalog mit Azure-Schnellstartvorlagen]: https://azure.microsoft.com/documentation/templates/?term=service+bus

<!---HONumber=AcomDC_0831_2016-->