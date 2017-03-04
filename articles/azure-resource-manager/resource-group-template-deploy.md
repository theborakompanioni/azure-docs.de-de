---
title: Bereitstellen von Ressourcen mit PowerShell und Vorlagen | Microsoft Docs
description: Verwenden Sie Azure Resource Manager und Azure PowerShell, um Ressourcen in Azure bereitzustellen. Die Ressourcen werden in einer Resource Manager-Vorlage definiert.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 55903f35-6c16-4c6d-bf52-dbf365605c3f
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/16/2016
ms.author: tomfitz
translationtype: Human Translation
ms.sourcegitcommit: 5b8b293b5b37365088a3df55581be7b7bf76691c
ms.openlocfilehash: 7bc3421e00215ca4629ea11811c98e581377b24a
ms.lasthandoff: 01/18/2017


---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-powershell"></a>Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und Azure PowerShell
> [!div class="op_single_selector"]
> * [PowerShell](resource-group-template-deploy.md)
> * [Azure-Befehlszeilenschnittstelle](resource-group-template-deploy-cli.md)
> * [Portal](resource-group-template-deploy-portal.md)
> * [REST-API](resource-group-template-deploy-rest.md)
> 
> 

In diesem Thema wird erläutert, wie Ihre Anwendung mit Azure PowerShell und Resource Manager-Vorlagen in Azure bereitgestellt wird. Ihre Vorlage kann entweder eine lokale Datei oder eine externe Datei sein, die über einen URI verfügbar ist. Wenn sich Ihre Vorlage in einem Speicherkonto befindet, können Sie den Zugriffs auf die Vorlage beschränken und ein SAS-Token (Shared Access Signature) während der Bereitstellung angeben.

> [!TIP]
> Hilfe zum Beheben eines Fehlers während der Bereitstellung finden Sie unter:
> 
> * [Anzeigen von Bereitstellungsvorgängen](resource-manager-deployment-operations.md). Erfahren Sie, wie Sie Informationen zum Beheben von Fehlern abrufen.
> * [Beheben von häufigen Fehlern beim Bereitstellen von Ressourcen in Azure mit Azure Resource Manager](resource-manager-common-deployment-errors.md). Hier erfahren Sie, wie Sie häufig auftretende Bereitstellungsfehler beheben.
> 
> 

## <a name="quick-steps-to-deployment"></a>Schnelle Schritte zur Bereitstellung
Verwenden Sie die folgenden Befehle, um schnell mit der Bereitstellung zu beginnen:

```powershell
New-AzureRmResourceGroup -Name ExampleResourceGroup -Location "West US"
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile c:\MyTemplates\example.json -TemplateParameterFile c:\MyTemplates\example.params.json
```

Diese Befehle dienen zum Erstellen einer Ressourcengruppe und Bereitstellen einer Vorlage für diese Ressourcengruppe. Die Vorlagen- und Parameterdatei sind lokale Dateien. Wenn das für Sie funktioniert, haben Sie alles, was Sie zum Bereitstellen von Ressourcen brauchen. Es stehen Ihnen allerdings mehr Optionen für das Angeben der bereitzustellenden Ressourcen zur Verfügung. Im restlichen Artikel werden alle Optionen beschrieben, die Ihnen während der Bereitstellung zur Verfügung stehen. 

[!INCLUDE [resource-manager-deployments](../../includes/resource-manager-deployments.md)]

## <a name="deploy"></a>Bereitstellen
1. Melden Sie sich beim Azure-Konto an.

   ```powershell
   Login-AzureRmAccount
   ```

    Es wird eine Zusammenfassung Ihres Kontos zurückgegeben.
    
   ```powershell
   Environment           : AzureCloud
   Account               : someone@example.com
   TenantId              : {guid}
   SubscriptionId        : {guid}
   SubscriptionName      : Example Subscription
   CurrentStorageAccount :
   ```

2. Wenn Sie über mehrere Abonnements verfügen, geben Sie die Abonnement-ID ein, die Sie mit dem Befehl **Set-AzureRmContext** für die Bereitstellung verwenden möchten. 
   
   ```powershell
   Set-AzureRmContext -SubscriptionID <YourSubscriptionId>
   ```
3. Wenn Sie eine Vorlage bereitstellen, müssen Sie eine Ressourcengruppe angeben, die die bereitgestellten Ressourcen enthält. Wenn es bereits eine Ressourcengruppe gibt, in der die Bereitstellung erfolgen soll, können Sie diesen Schritt überspringen und diese Ressourcengruppe nutzen. 
   
     Geben Sie zum Erstellen einer Ressourcengruppe einen Namen und Speicherort für diese an. Sie geben einen Standort für die Ressourcengruppe an, da diese Metadaten zu den Ressourcen speichert. Aus Compliance-Gründen sollten Sie angeben, wo diese Metadaten gespeichert werden. Im Allgemeinen wird die Angabe eines Standorts empfohlen, an dem sich der Großteil Ihrer Ressourcen befindet. Durch die Verwendung des gleichen Standorts können Sie die Vorlage vereinfachen.
   
   ```powershell
   New-AzureRmResourceGroup -Name ExampleResourceGroup -Location "West US"
   ```
   
     Es wird eine Zusammenfassung der neuen Ressourcengruppe zurückgegeben.
4. Vor dem Ausführen der Bereitstellung können Sie Ihre Bereitstellungseinstellungen überprüfen. Das Cmdlet **Test-AzureRmResourceGroupDeployment** ermöglicht Ihnen, Probleme zu finden, bevor tatsächlich Ressourcen erstellt werden. Im folgenden Beispiel wird veranschaulicht, wie eine Bereitstellung überprüft wird.
   
   ```powershell
   Test-AzureRmResourceGroupDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate>
   ```
5. Führen Sie zum Bereitstellen von Ressourcen in der Ressourcengruppe den Befehl **New-AzureRmResourceGroupDeployment** aus, und geben Sie die erforderlichen Parameter ein. Die Parameter enthalten den Namen der Bereitstellung, den Namen der Ressourcengruppe, den Pfad oder die URL der Vorlage und alle anderen für Ihr Szenario erforderlichen Parameter. Wenn der Parameter **Mode** nicht angegeben wurde, wird der Standardwert **Incremental** verwendet. Legen Sie zum Ausführen einer vollständigen Bereitstellung **Mode** auf **Complete** fest. Seien Sie bei Wahl des Modus „Complete“ vorsichtig, da Sie versehentlich Ressourcen löschen können, die nicht in Ihrer Vorlage enthalten sind.
   
     Verwenden Sie zum Bereitstellen einer lokalen Vorlage den **TemplateFile**-Parameter:
   
   ```powershell
   New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile c:\MyTemplates\example.json
   ```
   
     Verwenden Sie zum Bereitstellen einer externen Vorlage den **TemplateUri**-Parameter:
   
   ```powershell
   New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateUri https://raw.githubusercontent.com/exampleuser/MyTemplates/master/example.json
   ```
   
     Die vorherigen beiden Beispiele enthalten keine Parameterwerte. Im Abschnitt [Parameter](#parameters) erfahren Sie mehr über die Optionen zum Übergeben von Parameterwerten. Vorerst werden Sie aufgefordert, Parameterwerte mit der folgenden Syntax anzugeben:

   ```powershell  
   cmdlet New-AzureRmResourceGroupDeployment at command pipeline position 1
   Supply values for the following parameters:
   (Type !? for Help.)
   firstParameter: <type here>
   ```
     
     Nachdem die Ressourcen bereitgestellt wurden, wird eine Zusammenfassung der Bereitstellung angezeigt. Die Zusammenfassung enthält die Angabe **ProvisioningState**, die bestimmt, ob die Bereitstellung erfolgreich war.

   ```powershell   
   DeploymentName    : ExampleDeployment
   ResourceGroupName : ExampleResourceGroup
   ProvisioningState : Succeeded
   Timestamp         : 4/14/2015 7:00:27 PM
   Mode              : Incremental
   ```
      
6. Wenn Sie zusätzliche Informationen zur Bereitstellung protokollieren möchten, die Ihnen möglicherweise bei der Behebung von Bereitstellungsfehlern helfen können, verwenden Sie den Parameter **DeploymentDebugLogLevel**. Sie können angeben, dass der Anforderungsinhalt, der Antwortinhalt oder beide beim Bereitstellungsvorgang protokolliert werden.
   
   ```powershell
   New-AzureRmResourceGroupDeployment -Name ExampleDeployment -DeploymentDebugLogLevel All -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate>
   ```
   
     Weitere Informationen zur Verwendung dieses Debuginhalts zum Beheben von Bereitstellungsproblemen finden Sie unter [Anzeigen von Bereitstellungsvorgängen](resource-manager-deployment-operations.md).

## <a name="deploy-private-template-with-sas-token"></a>Bereitstellen einer privaten Vorlage mit SAS-Token
Sie können Ihre Vorlagen einem Speicherkonto hinzufügen und sie während der Bereitstellung mit einem SAS-Token verknüpfen.

> [!IMPORTANT]
> Wenn Sie die nachstehenden Schritte befolgen, hat nur der Kontobesitzer Zugriff auf das Blob mit der Vorlage. Wenn Sie jedoch ein SAS-Token für das Blob erstellen, können andere Benutzer über diesen URI auf das Blob zugreifen. Wenn ein anderer Benutzer den URI abfängt, hat dieser Benutzer Zugriff auf die Vorlage. Ein SAS-Token ist eine gute Möglichkeit zum Einschränken des Zugriffs auf Ihre Vorlagen. Sie sollten allerdings Kennwörter auf keinen Fall direkt in die Vorlage einschließen.
> 
> 

### <a name="add-private-template-to-storage-account"></a>Hinzufügen einer privaten Vorlage zum Speicherkonto
Führen Sie die folgenden Schritte aus, um ein Speicherkonto für Vorlagen einzurichten:

1. Erstellen Sie eine Ressourcengruppe.
   
   ```powershell
   New-AzureRmResourceGroup -Name ManageGroup -Location "West US"
   ```
2. Erstellen Sie ein Speicherkonto. Der Name des Speicherkontos muss innerhalb von Azure eindeutig sein. Geben Sie deshalb Ihren eigenen Namen für das Konto an.
   
   ```powershell
   New-AzureRmStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates -Type Standard_LRS -Location "West US"
   ```
3. Legen Sie das aktuelle Speicherkonto fest.
   
   ```powershell
   Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates
   ```
4. Erstellen Sie einen Container. Die Berechtigung ist auf **Off** festgelegt, was bedeutet, dass nur der Besitzer Zugriff auf den Container hat.
   
   ```powershell
   New-AzureStorageContainer -Name templates -Permission Off
   ```
5. Fügen Sie Ihre Vorlage dem Container hinzu.
   
   ```powershell
   Set-AzureStorageBlobContent -Container templates -File c:\Azure\Templates\azuredeploy.json
   ```

### <a name="provide-sas-token-during-deployment"></a>Bereitstellen eines SAS-Tokens während der Bereitstellung
Rufen Sie zum Bereitstellen einer privaten Vorlage in einem Speicherkonto ein SAS-Token ab, und fügen Sie es dem URI für die Vorlage hinzu.

1. Wenn Sie das aktuelle Speicherkonto geändert haben, legen Sie das aktuelle Speicherkonto auf dasjenige mit Ihren Vorlagen fest.
   
   ```powershell
   Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates
   ```
2. Erstellen Sie ein SAS-Token mit Leseberechtigungen und Ablaufzeit, um den Zugriff einzuschränken. Rufen Sie den vollständigen URI der Vorlage einschließlich SAS-Token ab.
   
   ```powershell
   $templateuri = New-AzureStorageBlobSASToken -Container templates -Blob azuredeploy.json -Permission r -ExpiryTime (Get-Date).AddHours(2.0) -FullUri
   ```
3. Stellen Sie die Vorlage durch Angabe des URI bereit, der das SAS-Token enthält.
   
   ```powershell
   New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleResourceGroup -TemplateUri $templateuri
   ```

Ein Beispiel der Verwendung eines SAS-Tokens mit verknüpften Vorlagen finden Sie unter [Verwenden von verknüpften Vorlagen mit Azure Resource Manager](resource-group-linked-templates.md).

## <a name="parameters"></a>Parameter

Sie haben die folgenden Möglichkeiten zum Angeben der Parameterwerte: 
   
- Inlineparameter. Beziehen Sie Namen einzelner Parameter in das Cmdlet ein (z.B. **-MeinParametername**.)

   ```powershell   
   New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate> -myParameterName "parameterValue"
   ```
- Parameterobjekt. Beziehen Sie den **-TemplateParameterObject**-Parameter ein.

   ```powershell   
   $parameters = @{"<ParameterName>"="<Parameter Value>"}
   New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate> -TemplateParameterObject $parameters
   ```
- Lokale Parameterdatei. Beziehen Sie den **-TemplateParameterFile**-Parameter ein.

   ```powershell   
   New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate> -TemplateParameterFile c:\MyTemplates\example.params.json
   ```
- Externe Parameterdatei. Beziehen Sie den **-TemplateParameterUri**-Parameter ein.

   ```powershell   
   New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateUri <LinkToTemplate> -TemplateParameterUri https://raw.githubusercontent.com/exampleuser/MyTemplates/master/example.params.json
   ```
      
[!INCLUDE [resource-manager-parameter-file](../../includes/resource-manager-parameter-file.md)] 

Sollte Ihre Vorlage einen Parameter enthalten, der den gleichen Namen besitzt wie einer der Parameter des PowerShell-Befehls, werden Sie zur Eingabe eines Werts für diesen Parameter aufgefordert. Azure PowerShell zeigt den Parameter aus Ihrer Vorlage mit dem Postfix **FromTemplate** an. Beispiel: Ein Parameter namens **ResourceGroupName** in Ihrer Vorlage verursacht einen Konflikt mit dem Parameter **ResourceGroupName** im Cmdlet [New-AzureRmResourceGroupDeployment](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.3.0/new-azurermresourcegroupdeployment). Sie werden zur Eingabe eines Werts für **ResourceGroupNameFromTemplate** aufgefordert. Im Allgemeinen sollten Sie diese Verwirrung vermeiden, indem Sie Parametern nicht dieselben Namen wie Parametern für Bereitstellungsvorgänge geben.

## <a name="parameter-precedence"></a>Parameterrangfolge

Sie können Inlineparameter und eine lokale Parameterdatei im selben Bereitstellungsvorgang verwenden. Sie können beispielsweise einige Werte in der lokalen Parameterdatei angeben und weitere Werte während der Bereitstellung inline hinzufügen. Wenn Sie Werte für einen Parameter sowohl in der lokalen Parameterdatei als auch inline bereitstellen, haben die Inlinewerte Vorrang.

Bei Verwendung einer externen Parameterdatei können Sie jedoch keine anderen Werte (weder inline noch aus einer lokalen Datei) übergeben. Wenn Sie eine Parameterdatei im Parameter **TemplateParameterUri** angeben, werden alle Inlineparameter ignoriert. Stellen Sie alle Parameterwerte in der externen Datei bereit. Wenn Ihre Vorlage einen vertraulichen Wert enthält, der nicht in die Parameterdatei aufgenommen werden kann, fügen Sie diesen Wert einem Schlüsseltresor hinzu. Alternativ können Sie alle Parameterwerte dynamisch inline bereitstellen.

## <a name="next-steps"></a>Nächste Schritte
* Ein Beispiel für die Bereitstellung von Ressourcen über die .NET-Clientbibliothek finden Sie unter [Bereitstellen von Ressourcen mithilfe von .NET-Bibliotheken und einer Vorlage](../virtual-machines/virtual-machines-windows-csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Informationen zum Definieren von Parametern in der Vorlage finden Sie unter [Erstellen von Vorlagen](resource-group-authoring-templates.md#parameters).
* Informationen zum Bereitstellen der Lösung in andere Umgebungen finden Sie unter [Entwicklungs- und Testumgebungen in Microsoft Azure](solution-dev-test-environments.md).
* Anleitungen dazu, wie Unternehmen Abonnements mit Resource Manager effektiv verwalten können, finden Sie unter [Azure-Unternehmensgerüst - Präskriptive Abonnementgovernance](resource-manager-subscription-governance.md).
* Eine vierteilige Reihe zum Automatisieren von Bereitstellungen finden Sie unter [Automatisieren von Anwendungsbereitstellungen auf virtuellen Azure-Computern](../virtual-machines/virtual-machines-windows-dotnet-core-1-landing.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Diese Reihe behandelt die Anwendungsarchitektur, Zugriff und Sicherheit, Verfügbarkeit und Skalierung sowie die Bereitstellung einer Anwendung.


