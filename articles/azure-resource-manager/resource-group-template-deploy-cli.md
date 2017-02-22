---
title: Bereitstellen von Ressourcen mit Azure-CLI und Vorlagen | Microsoft Docs
description: Verwenden Sie Azure Resource Manager und Azure-CLI, um Ressourcen in Azure bereitzustellen. Die Ressourcen werden in einer Resource Manager-Vorlage definiert.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 493b7932-8d1e-4499-912c-26098282ec95
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/16/2016
ms.author: tomfitz
translationtype: Human Translation
ms.sourcegitcommit: e4851e872349fa6483e1f1a340d0968e845a3518
ms.openlocfilehash: ccbb918a3377094395a27a9b7a63f213c5085027


---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-cli"></a>Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und Azure-CLI
> [!div class="op_single_selector"]
> * [PowerShell](resource-group-template-deploy.md)
> * [Azure-Befehlszeilenschnittstelle](resource-group-template-deploy-cli.md)
> * [Portal](resource-group-template-deploy-portal.md)
> * [REST-API](resource-group-template-deploy-rest.md)
> 
> 

In diesem Thema wird erläutert, wie Ihre Ressourcen mithilfe von Azure-CLI und Azure Resource Manager-Vorlagen in Azure bereitgestellt werden.  Ihre Vorlage kann entweder eine lokale Datei oder eine externe Datei sein, die über einen URI verfügbar ist. Wenn sich Ihre Vorlage in einem Speicherkonto befindet, können Sie den Zugriffs auf die Vorlage beschränken und ein SAS-Token (Shared Access Signature) während der Bereitstellung angeben.

> [!TIP]
> Hilfe zum Beheben eines Fehlers während der Bereitstellung finden Sie unter:
> 
> * [Anzeigen von Bereitstellungsvorgängen](resource-manager-deployment-operations.md). Erfahren Sie, wie Sie Informationen zum Beheben von Fehlern abrufen.
> * [Beheben von häufigen Fehlern beim Bereitstellen von Ressourcen in Azure mit Azure Resource Manager](resource-manager-common-deployment-errors.md). Hier erfahren Sie, wie Sie häufig auftretende Bereitstellungsfehler beheben.
> 
> 

## <a name="quick-steps-to-deployment"></a>Schnelle Schritte zur Bereitstellung
Verwenden Sie die folgenden Befehle, um schnell mit der Bereitstellung zu beginnen:

```
azure group create -n examplegroup -l "West US"
azure group deployment create -f "c:\MyTemplates\example.json" -e "c:\MyTemplates\example.params.json" -g examplegroup -n exampledeployment
```

Diese Befehle dienen zum Erstellen einer Ressourcengruppe und Bereitstellen einer Vorlage für diese Ressourcengruppe. Die Vorlagen- und Parameterdatei sind lokale Dateien. Wenn das für Sie funktioniert, haben Sie alles, was Sie zum Bereitstellen von Ressourcen brauchen. Es stehen Ihnen allerdings mehr Optionen für das Angeben der bereitzustellenden Ressourcen zur Verfügung. Im restlichen Artikel werden alle Optionen beschrieben, die Ihnen während der Bereitstellung zur Verfügung stehen. 

[!INCLUDE [resource-manager-deployments](../../includes/resource-manager-deployments.md)]

## <a name="deploy"></a>Bereitstellen
Wenn Sie die Azure-Befehlszeilenschnittstelle noch nicht mit dem Ressourcen-Manager verwendet haben, finden Sie unter [Verwenden der Azure-Befehlszeilenschnittstelle für Mac, Linux und Windows mit der Azure-Ressourcenverwaltung](xplat-cli-azure-resource-manager.md)weitere Informationen.

1. Melden Sie sich beim Azure-Konto an. Nach der Eingabe Ihrer Anmeldeinformationen gibt der Befehl das Ergebnis der Anmeldung zurück.
   
   ```
   azure login
   ```
2. Wenn Sie über mehrere Abonnements verfügen, geben Sie die Abonnement-ID ein, die Sie für die Bereitstellung verwenden möchten.
   
   ```
   azure account set <YourSubscriptionNameOrId>
   ```
4. Wenn Sie eine Vorlage bereitstellen, müssen Sie eine Ressourcengruppe angeben, die die bereitgestellten Ressourcen enthält. Wenn es bereits eine Ressourcengruppe gibt, in der die Bereitstellung erfolgen soll, können Sie diesen Schritt überspringen und diese Ressourcengruppe nutzen. 
   
     Geben Sie zum Erstellen einer Ressourcengruppe einen Namen und Speicherort für diese an. Sie geben einen Standort für die Ressourcengruppe an, da diese Metadaten zu den Ressourcen speichert. Aus Compliance-Gründen sollten Sie angeben, wo diese Metadaten gespeichert werden. Im Allgemeinen wird die Angabe eines Standorts empfohlen, an dem sich der Großteil Ihrer Ressourcen befindet. Durch die Verwendung des gleichen Standorts können Sie die Vorlage vereinfachen.
   
   ```
   azure group create -n ExampleResourceGroup -l "West US"
   ```

     Es wird eine Zusammenfassung der neuen Ressourcengruppe zurückgegeben.
   
5. Überprüfen Sie die Bereitstellung vor der Ausführung, indem Sie den Befehl **azure group template validate** ausführen. Geben Sie die Parameter beim Testen der Bereitstellung genauso an wie beim Ausführen der Bereitstellung (wie im nächsten Schritt zu sehen).
   
   ```
   azure group template validate -f <PathToTemplate> -p "{\"ParameterName\":{\"value\":\"ParameterValue\"}}" -g ExampleResourceGroup
   ```
6. Führen Sie zum Bereitstellen in der Ressourcengruppe den Befehl**azure group deployment create** aus, und geben Sie die erforderlichen Parameter ein. Die Parameter enthalten den Namen der Bereitstellung, den Namen der Ressourcengruppe, den Pfad oder die URL der Vorlage und alle anderen für Ihr Szenario erforderlichen Parameter. Wenn der Parameter **mode** nicht angegeben wurde, wird der Standardwert **Incremental** verwendet. Legen Sie zum Ausführen einer vollständigen Bereitstellung **mode** auf **Complete** fest. Seien Sie bei Wahl des Modus „Complete“ vorsichtig, da Sie versehentlich Ressourcen löschen können, die nicht in Ihrer Vorlage enthalten sind.
   
     Verwenden Sie zum Bereitstellen einer lokalen Vorlage den **template-file**-Parameter:
   
   ```
   azure group deployment create --resource-group examplegroup --template-file "c:\MyTemplates\example.json"
   ```
   
     Verwenden Sie zum Bereitstellen einer externen Vorlage den **template-uri**-Parameter:
   
   ```
   azure group deployment create --resource-group examplegroup --template-uri "https://raw.githubusercontent.com/exampleuser/MyTemplates/master/example.json"
   ```
   
     Die vorherigen beiden Beispiele enthalten keine Parameterwerte. Im Abschnitt [Parameter](#parameters) erfahren Sie mehr über die Optionen zum Übergeben von Parameterwerten. Vorerst werden Sie aufgefordert, Parameterwerte mit der folgenden Syntax anzugeben:

   ```
   info:    Executing command group deployment create
   info:    Supply values for the following parameters
   firstParameters:  <type here>
   ```

      Nachdem die Ressourcen bereitgestellt wurden, wird eine Zusammenfassung der Bereitstellung angezeigt. Die Zusammenfassung enthält die Angabe **ProvisioningState**, die bestimmt, ob die Bereitstellung erfolgreich war.

   ```
   + Initializing template configurations and parameters
   + Creating a deployment
   info:    Created template deployment "example"
   + Waiting for deployment to complete
   +
   +
   data:    DeploymentName     : example
   data:    ResourceGroupName  : examplegroup
   data:    ProvisioningState  : Succeeded
   ```
7. Wenn Sie weitere Informationen über die Bereitstellung protokollieren möchten, die Ihnen bei der Behebung von Bereitstellungsfehlern helfen können, verwenden Sie den Parameter **debug-setting** . Sie können angeben, dass der Anforderungsinhalt, der Antwortinhalt oder beide beim Bereitstellungsvorgang protokolliert werden.
   
   ```
   azure group deployment create --debug-setting All -f <PathToTemplate> -e <PathToParameterFile> -g examplegroup -n exampleDeployment
   ```

## <a name="deploy-template-from-storage-with-sas-token"></a>Bereitstellen einer Vorlage aus Speicher mithilfe eines SAS-Tokens
Sie können Ihre Vorlagen einem Speicherkonto hinzufügen und sie während der Bereitstellung mit einem SAS-Token verknüpfen.

> [!IMPORTANT]
> Wenn Sie die nachstehenden Schritte befolgen, hat nur der Kontobesitzer Zugriff auf das Blob mit der Vorlage. Wenn Sie jedoch ein SAS-Token für das Blob erstellen, können andere Benutzer über diesen URI auf das Blob zugreifen. Wenn ein anderer Benutzer den URI abfängt, hat dieser Benutzer Zugriff auf die Vorlage. Ein SAS-Token ist eine gute Möglichkeit zum Einschränken des Zugriffs auf Ihre Vorlagen. Sie sollten allerdings Kennwörter auf keinen Fall direkt in die Vorlage einschließen.
> 
> 

### <a name="add-private-template-to-storage-account"></a>Hinzufügen einer privaten Vorlage zum Speicherkonto
Führen Sie die folgenden Schritte aus, um ein Speicherkonto für Vorlagen einzurichten:

1. Erstellen Sie eine Ressourcengruppe.
   
   ```
   azure group create -n "ManageGroup" -l "westus"
   ```
2. Erstellen Sie ein Speicherkonto. Der Name des Speicherkontos muss innerhalb von Azure eindeutig sein. Geben Sie deshalb Ihren eigenen Namen für das Konto an.
   
   ```
   azure storage account create -g ManageGroup -l "westus" --sku-name LRS --kind Storage storagecontosotemplates
   ```
3. Legen Sie Variablen für Speicherkonto und Schlüssel fest.
   
   ```
   export AZURE_STORAGE_ACCOUNT=storagecontosotemplates
   export AZURE_STORAGE_ACCESS_KEY={storage_account_key}
   ```
4. Erstellen Sie einen Container. Die Berechtigung ist auf **Off** festgelegt, was bedeutet, dass nur der Besitzer Zugriff auf den Container hat.
   
   ```
   azure storage container create --container templates -p Off 
   ```
5. Fügen Sie Ihre Vorlage dem Container hinzu.
   
   ```
   azure storage blob upload --container templates -f c:\MyTemplates\azuredeploy.json
   ```

### <a name="provide-sas-token-during-deployment"></a>Bereitstellen eines SAS-Tokens während der Bereitstellung
Rufen Sie zum Bereitstellen einer privaten Vorlage in einem Speicherkonto ein SAS-Token ab, und fügen Sie es dem URI für die Vorlage hinzu.

1. Erstellen Sie ein SAS-Token mit Leseberechtigungen und Ablaufzeit, um den Zugriff einzuschränken. Legen Sie die Ablaufzeit so fest, dass ausreichend Zeit für die Bereitstellung bleibt. Rufen Sie den vollständigen URI der Vorlage einschließlich SAS-Token ab.
   
   ```
   expiretime=$(date -I'minutes' --date "+30 minutes")
   fullurl=$(azure storage blob sas create --container templates --blob azuredeploy.json --permissions r --expiry $expiretimetime --json  | jq ".url")
   ```
2. Stellen Sie die Vorlage durch Angabe des URI bereit, der das SAS-Token enthält.
   
   ```
   azure group deployment create --template-uri $fullurl -g ExampleResourceGroup
   ```

Ein Beispiel der Verwendung eines SAS-Tokens mit verknüpften Vorlagen finden Sie unter [Verwenden von verknüpften Vorlagen mit Azure Resource Manager](resource-group-linked-templates.md).

## <a name="parameters"></a>Parameter

Sie haben die folgenden Möglichkeiten zum Angeben der Parameterwerte: 
   
- Verwenden Sie Inlineparameter. Jeder Parameter weist das Format `"ParameterName": { "value": "ParameterValue" }`auf. Das folgende Beispiel zeigt die Parameter mit Escapezeichen.

   ```   
   azure group deployment create -f <PathToTemplate> -p "{\"ParameterName\":{\"value\":\"ParameterValue\"}}" -g ExampleResourceGroup -n ExampleDeployment
   ```
- Verwenden Sie eine Parameterdatei.

  ```    
  azure group deployment create -f "c:\MyTemplates\example.json" -e "c:\MyTemplates\example.params.json" -g ExampleResourceGroup -n ExampleDeployment
  ```
      

[!INCLUDE [resource-manager-parameter-file](../../includes/resource-manager-parameter-file.md)]

## <a name="next-steps"></a>Nächste Schritte
* Ein Beispiel für die Bereitstellung von Ressourcen über die .NET-Clientbibliothek finden Sie unter [Bereitstellen von Ressourcen mithilfe von .NET-Bibliotheken und einer Vorlage](../virtual-machines/virtual-machines-windows-csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Informationen zum Definieren von Parametern in der Vorlage finden Sie unter [Erstellen von Vorlagen](resource-group-authoring-templates.md#parameters).
* Informationen zum Bereitstellen der Lösung in andere Umgebungen finden Sie unter [Entwicklungs- und Testumgebungen in Microsoft Azure](solution-dev-test-environments.md).
* Weitere Informationen zum Verwenden eines KeyVault-Verweises zum Übergeben sicherer Werte finden Sie unter [Übergeben sicherer Werte während der Bereitstellung](resource-manager-keyvault-parameter.md).
* Anleitungen dazu, wie Unternehmen Abonnements mit Resource Manager effektiv verwalten können, finden Sie unter [Azure-Unternehmensgerüst - Präskriptive Abonnementgovernance](resource-manager-subscription-governance.md).
* Eine vierteilige Reihe zum Automatisieren von Bereitstellungen finden Sie unter [Automatisieren von Anwendungsbereitstellungen auf virtuellen Azure-Computern](../virtual-machines/virtual-machines-windows-dotnet-core-1-landing.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Diese Reihe behandelt die Anwendungsarchitektur, Zugriff und Sicherheit, Verfügbarkeit und Skalierung sowie die Bereitstellung einer Anwendung.




<!--HONumber=Jan17_HO2-->


