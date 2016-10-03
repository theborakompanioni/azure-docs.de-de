<properties
   pageTitle="Bereitstellen von Ressourcen mit Azure-CLI und Vorlagen | Microsoft Azure"
   description="Verwenden Sie Azure Resource Manager und Azure-CLI, um Ressourcen in Azure bereitzustellen. Die Ressourcen werden in einer Resource Manager-Vorlage definiert."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tomfitz"/>

# Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und Azure-CLI

> [AZURE.SELECTOR]
- [PowerShell](resource-group-template-deploy.md)
- [Azure-Befehlszeilenschnittstelle](resource-group-template-deploy-cli.md)
- [Portal](resource-group-template-deploy-portal.md)
- [REST-API](resource-group-template-deploy-rest.md)

In diesem Thema wird erläutert, wie Ihre Ressourcen mithilfe von Azure-CLI und Azure Resource Manager-Vorlagen in Azure bereitgestellt werden.

> [AZURE.TIP] Hilfe zum Beheben eines Fehlers während der Bereitstellung finden Sie unter:
>
> - [Anzeigen von Bereitstellungsvorgängen mit Azure-CLI](resource-manager-troubleshoot-deployments-cli.md). Hier erfahren Sie, wie Sie Informationen zum Beheben von Fehlern abrufen.
> - [Beheben von häufigen Fehlern beim Bereitstellen von Ressourcen in Azure mit Azure Resource Manager](resource-manager-common-deployment-errors.md). Hier erfahren Sie, wie Sie häufig auftretende Bereitstellungsfehler beheben.

Ihre Vorlage kann entweder eine lokale Datei oder eine externe Datei sein, die über einen URI verfügbar ist. Wenn sich Ihre Vorlage in einem Speicherkonto befindet, können Sie den Zugriffs auf die Vorlage beschränken und ein SAS-Token (Shared Access Signature) während der Bereitstellung angeben.

## Schnelle Schritte zur Bereitstellung

Dieser Artikel beschreibt alle verschiedenen Optionen, die Ihnen während der Bereitstellung zur Verfügung stehen. Allerdings benötigen Sie häufig nur zwei einfache Befehle. Verwenden Sie die folgenden Befehle, um schnell mit der Bereitstellung zu beginnen:

    azure group create -n ExampleResourceGroup -l "West US"
    azure group deployment create -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

Weitere Informationen zu Optionen für die Bereitstellung, die für Ihr Szenario besser geeignet sein könnten, erhalten Sie im weiteren Verlauf dieses Artikels.

[AZURE.INCLUDE [resource-manager-deployments](../includes/resource-manager-deployments.md)]

## Bereitstellen über die Azure-Befehlszeilenschnittstelle

Wenn Sie die Azure-Befehlszeilenschnittstelle noch nicht mit dem Ressourcen-Manager verwendet haben, finden Sie unter [Verwenden der Azure-Befehlszeilenschnittstelle für Mac, Linux und Windows mit der Azure-Ressourcenverwaltung](xplat-cli-azure-resource-manager.md) weitere Informationen.

1. Melden Sie sich beim Azure-Konto an. Nach der Eingabe Ihrer Anmeldeinformationen gibt der Befehl das Ergebnis der Anmeldung zurück.

        azure login
  
        ...
        info:    login command OK

2. Wenn Sie über mehrere Abonnements verfügen, geben Sie die Abonnement-ID ein, die Sie für die Bereitstellung verwenden möchten.

        azure account set <YourSubscriptionNameOrId>

3. Wechseln Sie zum Azure-Ressourcen-Manager-Modul. Der neue Modus wird bestätigt.

        azure config mode arm
   
        info:     New mode is arm

4. Erstellen Sie eine Ressourcengruppe, wenn noch keine vorhanden ist. Geben Sie den Namen der Ressourcengruppe und des gewünschten Speicherorts ein. Sie müssen einen Standort für die Ressourcengruppe angeben, da diese Metadaten zu den Ressourcen speichert. Aus Compliance-Gründen sollten Sie angeben, wo diese Metadaten gespeichert werden. Im Allgemeinen wird die Angabe eines Standorts empfohlen, an dem sich der Großteil Ihrer Ressourcen befindet. Durch die Verwendung des gleichen Standorts können Sie die Vorlage vereinfachen.

        azure group create -n ExampleResourceGroup -l "West US"

     Es wird eine Zusammenfassung der neuen Ressourcengruppe zurückgegeben.
   
        info:    Executing command group create
        + Getting resource group ExampleResourceGroup
        + Creating resource group ExampleResourceGroup
        info:    Created resource group ExampleResourceGroup
        data:    Id:                  /subscriptions/####/resourceGroups/ExampleResourceGroup
        data:    Name:                ExampleResourceGroup
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags:
        data:
        info:    group create command OK

5. Überprüfen Sie die Bereitstellung vor der Ausführung, indem Sie den Befehl **azure group template validate** ausführen. Geben Sie die Parameter beim Testen der Bereitstellung genauso an wie beim Ausführen der Bereitstellung (wie im nächsten Schritt zu sehen).

        azure group template validate -f <PathToTemplate> -p "{"ParameterName":{"value":"ParameterValue"}}" -g ExampleResourceGroup

5. Führen Sie zum Bereitstellen von Ressourcen in Ihrer Ressourcengruppe den folgenden Befehl aus, und geben Sie die erforderlichen Parameter ein. Die Parameter enthalten den Namen der Bereitstellung, den Namen der Ressourcengruppe, den Pfad oder die URL der Vorlage und alle anderen für Ihr Szenario erforderlichen Parameter.
   
     Sie haben die folgenden drei Möglichkeiten, die Parameterwerte anzugeben:

     1. Verwenden Sie Inlineparameter und eine lokale Vorlage. Jeder Parameter weist das Format `"ParameterName": { "value": "ParameterValue" }` auf. Das folgende Beispiel zeigt die Parameter mit Escapezeichen.

            azure group deployment create -f <PathToTemplate> -p "{"ParameterName":{"value":"ParameterValue"}}" -g ExampleResourceGroup -n ExampleDeployment

     2. Verwenden Sie Inlineparameter und einen Link zu einer Vorlage.

            azure group deployment create --template-uri <LinkToTemplate> -p "{"ParameterName":{"value":"ParameterValue"}}" -g ExampleResourceGroup -n ExampleDeployment

     3. Verwenden Sie eine Parameterdatei. Informationen über die Vorlagendatei finden Sie unter [Parameterdatei](./#parameter-file).
    
            azure group deployment create -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

     Nach der Bereitstellung der Ressourcen über eine der oben genannten drei Methoden sehen Sie eine Zusammenfassung der Bereitstellung.
  
        info:    Executing command group deployment create
        + Initializing template configurations and parameters
        + Creating a deployment
        ...
        info:    group deployment create command OK

     Legen Sie zum Ausführen einer vollständigen Bereitstellung **Mode** auf **Complete** fest. Seien Sie bei Wahl dieses Modus vorsichtig, da Sie versehentlich Ressourcen löschen können, die nicht in Ihrer Vorlage enthalten sind.

        azure group deployment create --mode Complete -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

6. Wenn Sie weitere Informationen über die Bereitstellung protokollieren möchten, die Ihnen bei der Behebung von Bereitstellungsfehlern helfen können, verwenden Sie den Parameter **debug-setting**. Sie können angeben, dass der Anforderungsinhalt, der Antwortinhalt oder beide beim Bereitstellungsvorgang protokolliert werden.

        azure group deployment create --debug-setting All -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

## Bereitstellen einer Vorlage aus Speicher mithilfe eines SAS-Tokens

Sie können Ihre Vorlagen einem Speicherkonto hinzufügen und sie während der Bereitstellung mit einem SAS-Token verknüpfen.

> [AZURE.IMPORTANT] Wenn Sie die nachstehenden Schritte befolgen, hat nur der Kontobesitzer Zugriff auf das Blob mit der Vorlage. Wenn Sie jedoch ein SAS-Token für das Blob erstellen, können andere Benutzer über diesen URI auf das Blob zugreifen. Wenn ein anderer Benutzer den URI abfängt, hat dieser Benutzer Zugriff auf die Vorlage. Ein SAS-Token ist eine gute Möglichkeit zum Einschränken des Zugriffs auf Ihre Vorlagen. Sie sollten allerdings Kennwörter auf keinen Fall direkt in die Vorlage einschließen.

### Hinzufügen einer privaten Vorlage zum Speicherkonto

Führen Sie die folgenden Schritte aus, um ein Speicherkonto für Vorlagen einzurichten:

1. Erstellen Sie eine Ressourcengruppe.

        azure group create -n "ManageGroup" -l "westus"

2. Erstellen Sie ein Speicherkonto. Der Name des Speicherkontos muss innerhalb von Azure eindeutig sein. Geben Sie deshalb Ihren eigenen Namen für das Konto an.

        azure storage account create -g ManageGroup -l "westus" --sku-name LRS --kind Storage storagecontosotemplates

3. Legen Sie Variablen für Speicherkonto und Schlüssel fest.

        export AZURE_STORAGE_ACCOUNT=storagecontosotemplates
        export AZURE_STORAGE_ACCESS_KEY={storage_account_key}

4. Erstellen Sie einen Container. Die Berechtigung ist auf **Off** festgelegt, was bedeutet, dass nur der Besitzer Zugriff auf den Container hat.

        azure storage container create --container templates -p Off 
        
4. Fügen Sie Ihre Vorlage dem Container hinzu.

        azure storage blob upload --container templates -f c:\Azure\Templates\azuredeploy.json
        
### Bereitstellen eines SAS-Tokens während der Bereitstellung

Rufen Sie zum Bereitstellen einer privaten Vorlage in einem Speicherkonto ein SAS-Token ab, und fügen Sie es dem URI für die Vorlage hinzu.

1. Erstellen Sie ein SAS-Token mit Leseberechtigungen und Ablaufzeit, um den Zugriff einzuschränken. Legen Sie die Ablaufzeit so fest, dass ausreichend Zeit für die Bereitstellung bleibt. Rufen Sie den vollständigen URI der Vorlage einschließlich SAS-Token ab.

        expiretime=$(date -I'minutes' --date "+30 minutes")
        fullurl=$(azure storage blob sas create --container templates --blob azuredeploy.json --permissions r --expiry $expiretimetime --json  | jq ".url")

2. Stellen Sie die Vorlage durch Angabe des URI bereit, der das SAS-Token enthält.

        azure group deployment create --template-uri $fullurl -g ExampleResourceGroup

Ein Beispiel der Verwendung eines SAS-Tokens mit verknüpften Vorlagen finden Sie unter [Verwenden von verknüpften Vorlagen mit Azure Resource Manager](resource-group-linked-templates.md).

[AZURE.INCLUDE [resource-manager-parameter-file](../includes/resource-manager-parameter-file.md)]

## Nächste Schritte
- Ein Beispiel für die Bereitstellung von Ressourcen über die .NET-Clientbibliothek finden Sie unter [Bereitstellen von Ressourcen mithilfe von .NET-Bibliotheken und einer Vorlage](virtual-machines/virtual-machines-windows-csharp-template.md).
- Informationen zum Definieren von Parametern in der Vorlage finden Sie unter [Erstellen von Vorlagen](resource-group-authoring-templates.md#parameters).
- Informationen zum Bereitstellen der Lösung in andere Umgebungen finden Sie unter [Entwicklungs- und Testumgebungen in Microsoft Azure](solution-dev-test-environments.md).
- Weitere Informationen zum Verwenden eines KeyVault-Verweises zum Übergeben sicherer Werte finden Sie unter [Übergeben sicherer Werte während der Bereitstellung](resource-manager-keyvault-parameter.md).

<!---HONumber=AcomDC_0921_2016-->