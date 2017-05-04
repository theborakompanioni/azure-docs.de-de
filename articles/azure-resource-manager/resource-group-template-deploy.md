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
ms.date: 04/19/2017
ms.author: tomfitz
translationtype: Human Translation
ms.sourcegitcommit: abdbb9a43f6f01303844677d900d11d984150df0
ms.openlocfilehash: f5119fef1fb0a316b4109ccbc4433f8c9a18d128
ms.lasthandoff: 04/20/2017


---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-powershell"></a>Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und Azure PowerShell

In diesem Thema wird erläutert, wie Ihre Anwendung mit Azure PowerShell und Resource Manager-Vorlagen in Azure bereitgestellt wird. Ihre Vorlage kann entweder eine lokale Datei oder eine externe Datei sein, die über einen URI verfügbar ist.

Die in diesen Beispielen verwendete Vorlage („storage.json“) finden Sie im Artikel [Erstellen Ihrer ersten Azure Resource Manager-Vorlage](resource-manager-create-first-template.md#final-template). Wenn Sie die Vorlage mit diesen Beispielen verwenden möchten, erstellen Sie eine JSON-Datei, und fügen Sie den kopierten Inhalt hinzu.

## <a name="deploy-local-template"></a>Bereitstellen einer lokalen Vorlage
Verwenden Sie die folgenden Cmdlets zum Bereitstellen einer lokalen Vorlage mit Inlineparametern, um schnell mit der Bereitstellung zu beginnen. 

```powershell
Login-AzureRmAccount
 
New-AzureRmResourceGroup -Name ExampleGroup -Location "South Central US"
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\storage.json -storageNamePrefix contoso -storageSKU Standard_GRS
```

Die Bereitstellung kann einige Minuten dauern. Wenn sie abgeschlossen ist, wird eine Nachricht mit dem Ergebnis angezeigt:

```powershell
ProvisioningState       : Succeeded
```

Im vorherigen Beispiel wurde die Ressourcengruppe in Ihrem Standardabonnement erstellt. Um ein anderes Abonnement zu verwenden, fügen Sie nach der Anmeldung das Cmdlet [Set-AzureRmContext](/powershell/module/azurerm.profile/set-azurermcontext) hinzu.

## <a name="deploy-external-template"></a>Bereitstellen einer externen Vorlage

Verwenden Sie zum Bereitstellen einer externen Vorlage den **TemplateUri**-Parameter. Die Vorlage kann sich unter jedem öffentlich zugänglichen URI (z.B. einer Datei im Speicherkonto) befinden.

```powershell
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateUri https://raw.githubusercontent.com/exampleuser/MyTemplates/master/storage.json `
  -storageNamePrefix contoso -storageSKU Standard_GRS
```

Sie können die Vorlage schützen, indem Sie festlegen, dass ein SAS-Token (Shared Access Signature) für den Zugriff erforderlich ist. Informationen zum Bereitstellen einer Vorlage, die ein SAS-Token erfordert, finden Sie unter [Bereitstellen einer privaten Vorlage mit SAS-Token](resource-manager-powershell-sas-token.md).

## <a name="parameter-files"></a>Parameterdateien

Mit den vorherigen Beispielen wurde demonstriert, wie Parameter als Inlinewerte übergeben werden. Sie können Parameterwerte in einer Datei angeben und diese Datei bei der Bereitstellung übergeben. 

Die Parameterdatei muss im folgenden Format vorliegen:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
     "storageNamePrefix": {
         "value": "contoso"
     },
     "storageSKU": {
         "value": "Standard_GRS"
     }
  }
}
```

Verwenden Sie den **TemplateParameterFile**-Parameter, um eine lokale Parameterdatei zu übergeben:

```powershell
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\storage.json `
  -TemplateParameterFile c:\MyTemplates\storage.parameters.json
```

Um eine externe Parameterdatei zu übergeben, verwenden Sie den **TemplateParameterUri**-Parameter:

```powershell
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateUri https://raw.githubusercontent.com/exampleuser/MyTemplates/master/storage.json `
  -TemplateParameterUri https://raw.githubusercontent.com/exampleuser/MyTemplates/master/storage.parameters.json
```

Sie können Inlineparameter und eine lokale Parameterdatei im selben Bereitstellungsvorgang verwenden. Sie können beispielsweise einige Werte in der lokalen Parameterdatei angeben und weitere Werte während der Bereitstellung inline hinzufügen. Wenn Sie Werte für einen Parameter sowohl in der lokalen Parameterdatei als auch inline bereitstellen, haben die Inlinewerte Vorrang.

Bei Verwendung einer externen Parameterdatei können Sie jedoch keine anderen Werte (weder inline noch aus einer lokalen Datei) übergeben. Wenn Sie eine Parameterdatei im Parameter **TemplateParameterUri** angeben, werden alle Inlineparameter ignoriert. Stellen Sie alle Parameterwerte in der externen Datei bereit. Wenn Ihre Vorlage einen vertraulichen Wert enthält, der nicht in die Parameterdatei aufgenommen werden kann, fügen Sie diesen Wert einem Schlüsseltresor hinzu. Alternativ können Sie alle Parameterwerte dynamisch inline bereitstellen.

Wenn Ihre Vorlage einen Parameter enthält, der den gleichen Namen besitzt wie einer der Parameter des PowerShell-Befehls, zeigt PowerShell den Parameter der Vorlage mit dem Postfix **FromTemplate** an. Beispiel: Ein Parameter namens **ResourceGroupName** in Ihrer Vorlage verursacht einen Konflikt mit dem Parameter **ResourceGroupName** im Cmdlet [New-AzureRmResourceGroupDeployment](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.3.0/new-azurermresourcegroupdeployment). Sie werden zur Eingabe eines Werts für **ResourceGroupNameFromTemplate** aufgefordert. Im Allgemeinen sollten Sie diese Verwirrung vermeiden, indem Sie Parametern nicht dieselben Namen wie Parametern für Bereitstellungsvorgänge geben.

## <a name="test-a-deployment"></a>Testen einer Bereitstellung

Verwenden Sie [Test-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/test-azurermresourcegroupdeployment), um die Vorlage und die Parameterwerte zu testen, ohne dabei Ressourcen bereitzustellen. Es gelten jeweils die gleichen Optionen für die Verwendung von lokalen oder Remotedateien.

```powershell
Test-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\storage.json -storageNamePrefix contoso -storageSKU Standard_GRS
```

## <a name="log-deployment-content-for-debugging"></a>Protokollieren des Bereitstellungsinhalts für das Debuggen

Informationen zu Bereitstellungsvorgängen werden automatisch in den Aktivitätsprotokollen protokolliert. Wenn Sie jedoch zusätzliche Informationen zur Bereitstellung protokollieren möchten, die Ihnen möglicherweise bei der Behebung von Bereitstellungsfehlern helfen, verwenden Sie den **DeploymentDebugLogLevel**-Parameter. Sie können angeben, dass der Anforderungsinhalt, der Antwortinhalt oder beide beim Bereitstellungsvorgang protokolliert werden.
   
```powershell
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -DeploymentDebugLogLevel All `
  -ResourceGroupName ExampleGroup -TemplateFile storage.json
```

Informationen zum Anzeigen der Protokolle finden Sie unter [Anzeigen von Aktivitätsprotokollen, um Aktionen an Ressourcen zu überwachen](resource-group-audit.md).

## <a name="export-resource-manager-template"></a>Exportieren einer Resource Manager-Vorlage
Für eine vorhandene Ressourcengruppe (die über PowerShell oder eine andere Methode, z.B. das Portal, bereitgestellt wird), können Sie die Resource Manager-Vorlage für die Ressourcengruppe anzeigen. Das Exportieren der Vorlage hat zwei Vorteile:

1. Sie können problemlos zukünftige Bereitstellungen der Lösung automatisieren, da die gesamte Infrastruktur in der Vorlage definiert ist.
2. Sie können sich mit der Vorlagensyntax vertraut machen, indem Sie sich die JavaScript Object Notation (JSON) zu Ihrer Lösung ansehen.

Führen Sie zum Anzeigen der Vorlage für eine Ressourcengruppe das Cmdlet [Export-AzureRmResourceGroup](/powershell/module/azurerm.resources/export-azurermresourcegroup) aus.

```powershell
Export-AzureRmResourceGroup -ResourceGroupName ExampleResourceGroup
```

Weitere Informationen finden Sie unter [Exportieren einer Azure Resource Manager-Vorlage aus vorhandenen Ressourcen](resource-manager-export-template.md).


[!INCLUDE [resource-manager-deployments](../../includes/resource-manager-deployments.md)]

Nutzen Sie zur Verwendung des vollständigen Modus den `Mode`-Parameter:

```powershell
New-AzureRmResourceGroupDeployment -Mode Complete -Name ExampleDeployment `
  -ResourceGroupName ExampleResourceGroup -TemplateFile c:\MyTemplates\storage.json 
```


## <a name="next-steps"></a>Nächste Schritte
* Ein vollständiges Beispielskript, mit dem eine Vorlage bereitgestellt wird, finden Sie unter [Resource Manager-Vorlage – Bereitstellungsskript](resource-manager-samples-powershell-deploy.md).
* Informationen zum Definieren von Parametern in der Vorlage finden Sie unter [Erstellen von Vorlagen](resource-group-authoring-templates.md#parameters).
* Tipps zum Beheben gängiger Azure-Bereitstellungsfehler finden Sie unter [Beheben gängiger Azure-Bereitstellungsfehler mit Azure Resource Manager](resource-manager-common-deployment-errors.md).
* Informationen zum Bereitstellen einer Vorlage, die ein SAS-Token erfordert, finden Sie unter [Bereitstellen einer privaten Vorlage mit SAS-Token](resource-manager-powershell-sas-token.md).
* Anleitungen dazu, wie Unternehmen Abonnements mit Resource Manager effektiv verwalten können, finden Sie unter [Azure-Unternehmensgerüst - Präskriptive Abonnementgovernance](resource-manager-subscription-governance.md).


