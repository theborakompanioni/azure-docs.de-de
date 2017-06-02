---
title: "Verknüpfen von Vorlagen für die Azure-Bereitstellung | Microsoft-Dokumentation"
description: "Beschreibt, wie verknüpfte Vorlagen in einer Azure-Ressourcen-Manager-Vorlage zum Erstellen einer modularen Vorlagenprojektmappe verwendet werden. Zeigt, wie Parameterwerte übergeben, eine Parameterdatei festgelegt und URLs dynamisch erstellt werden."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 27d8c4b2-1e24-45fe-88fd-8cf98a6bb2d2
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/31/2017
ms.author: tomfitz
ms.translationtype: Human Translation
ms.sourcegitcommit: c785ad8dbfa427d69501f5f142ef40a2d3530f9e
ms.openlocfilehash: 9ab6d3e5e41f155b1404cee8a555078409c09c60
ms.contentlocale: de-de
ms.lasthandoff: 05/26/2017


---
# <a name="using-linked-templates-when-deploying-azure-resources"></a>Verwenden von verknüpften Vorlagen bei der Bereitstellung von Azure-Ressourcen
Aus einer Azure Resource Manager-Vorlage heraus können Sie einen Link zu einer anderen Vorlage herstellen, sodass Sie die Bereitstellung in eine Reihe von ausgewählten und zweckgebundenen Vorlagen zerlegen können. Wie das Zerlegen einer Anwendung in mehrere Codeklassen bietet diese Zerlegung Vorteile in Bezug auf Tests, Wiederverwendung und Lesbarkeit.  

Sie können Parameter aus einer Hauptvorlage an eine verknüpfte Vorlage übergeben, und diese Parameter können direkt Parametern oder Variablen zugeordnet werden, die von der aufrufenden Vorlage verfügbar gemacht werden. Die verknüpfte Vorlage kann auch eine Ausgabevariable zurück an die Quellvorlage übergeben, wodurch ein bidirektionaler Datenaustausch zwischen Vorlagen ermöglicht wird.

## <a name="linking-to-a-template"></a>Verknüpfen mit einer Vorlage
Sie erstellen einen Link zwischen zwei Vorlagen durch Hinzufügen einer Bereitstellungsressource innerhalb der Hauptvorlage, die auf die verknüpfte Vorlage verweist. Sie legen die **templateLink** -Eigenschaft für den URI der verknüpften Vorlage fest. Für die verknüpfte Vorlage können Sie Parameterwerte direkt in der Vorlage oder in einer Parameterdatei angeben. Im folgenden Beispiel wird die **parameters** -Eigenschaft verwendet, um direkt einen Parameterwert anzugeben.

```json
"resources": [ 
  { 
      "apiVersion": "2017-05-10", 
      "name": "linkedTemplate", 
      "type": "Microsoft.Resources/deployments", 
      "properties": { 
        "mode": "incremental", 
        "templateLink": {
          "uri": "https://www.contoso.com/AzureTemplates/newStorageAccount.json",
          "contentVersion": "1.0.0.0"
        }, 
        "parameters": { 
          "StorageAccountName":{"value": "[parameters('StorageAccountName')]"} 
        } 
      } 
  } 
] 
```

Wie andere Ressourcentypen können Sie Abhängigkeiten zwischen der verknüpften Vorlage und anderen Ressourcen festlegen. Wenn also andere Ressourcen einen Ausgabewert aus der verknüpften Vorlage benötigen, können Sie sicherstellen, dass die verknüpfte Vorlage vor ihnen bereitgestellt wird. Wenn andererseits die verknüpfte Vorlage von anderen Ressourcen abhängig ist, können Sie sicherstellen, dass andere Ressourcen vor der verknüpften Vorlage bereitgestellt werden. Sie können mit der folgenden Syntax einen Wert aus einer verknüpften Vorlage abrufen:

```json
"[reference('linkedTemplate').outputs.exampleProperty.value]"
```

Der Resource Manager-Dienst muss auf die verknüpfte Vorlage zugreifen können. Sie können keine lokale Datei und keine Datei, die nur in Ihrem lokalen Netzwerk verfügbar ist, für die verknüpfte Vorlage angeben. Sie können nur einen URI-Wert bereitstellen, der entweder **http** oder **https** enthält. Eine Option besteht darin, Ihre verknüpfte Vorlage in einem Speicherkonto zu platzieren und den URI für dieses Element zu verwenden, wie im folgenden Beispiel gezeigt:

```json
"templateLink": {
    "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
    "contentVersion": "1.0.0.0",
}
```

Obwohl die verknüpfte Vorlage extern verfügbar sein muss, muss sie der Öffentlichkeit nicht allgemein zur Verfügung stehen. Sie können Ihre Vorlage einem privaten Speicherkonto hinzufügen, auf das nur der Speicherkontobesitzer Zugriff hat. Anschließend erstellen Sie ein SAS-Token (Shared Access Signature), um den Zugriff während der Bereitstellung zu ermöglichen. Sie fügen dieses SAS-Token dem URI für die verknüpfte Vorlage hinzu. Schritte zum Einrichten einer Vorlage in einem Speicherkonto und zum Generieren eines SAS-Tokens finden Sie unter [Bereitstellen von Ressourcen mit Resource Manager-Vorlagen und Azure PowerShell](resource-group-template-deploy.md) oder [Bereitstellen von Ressourcen mit Resource Manager-Vorlagen und Azure-CLI](resource-group-template-deploy-cli.md). 

Im folgenden Beispiel wird eine übergeordnete Vorlage gezeigt, die mit einer anderen Vorlage verknüpft ist. Der Zugriff auf die verknüpfte Vorlage erfolgt mithilfe eines SAS-Tokens, das als Parameter übergeben wird.

```json
"parameters": {
    "sasToken": { "type": "securestring" }
},
"resources": [
    {
        "apiVersion": "2017-05-10",
        "name": "linkedTemplate",
        "type": "Microsoft.Resources/deployments",
        "properties": {
          "mode": "incremental",
          "templateLink": {
            "uri": "[concat('https://storagecontosotemplates.blob.core.windows.net/templates/helloworld.json', parameters('sasToken'))]",
            "contentVersion": "1.0.0.0"
          }
        }
    }
],
```

Obwohl das Token als sichere Zeichenfolge übergeben wird, wird der URI der verknüpften Vorlage samt SAS-Token in den Bereitstellungsvorgängen protokolliert. Legen Sie ein Ablaufdatum für das Token fest, um den Zugriff zu beschränken.

Der Resource Manager behandelt jede verknüpfte Vorlage als separate Bereitstellung. Im Bereitstellungsverlauf für die Ressourcengruppe finden Sie separate Bereitstellungen für die übergeordneten und geschachtelten Vorlagen.

![Bereitstellungsverlauf](./media/resource-group-linked-templates/linked-deployment-history.png)

## <a name="linking-to-a-parameter-file"></a>Verknüpfen mit einer Parameterdatei
Im nächsten Beispiel wird die **parametersLink** -Eigenschaft genutzt, um eine Verknüpfung mit einer Parameterdatei herzustellen.

```json
"resources": [ 
  { 
     "apiVersion": "2017-05-10", 
     "name": "linkedTemplate", 
     "type": "Microsoft.Resources/deployments", 
     "properties": { 
       "mode": "incremental", 
       "templateLink": {
          "uri":"https://www.contoso.com/AzureTemplates/newStorageAccount.json",
          "contentVersion":"1.0.0.0"
       }, 
       "parametersLink": { 
          "uri":"https://www.contoso.com/AzureTemplates/parameters.json",
          "contentVersion":"1.0.0.0"
       } 
     } 
  } 
] 
```

Der URI-Wert für die verknüpfte Parameterdatei darf keine lokale Datei sein und muss entweder **http** oder **https** enthalten. Für die Parameterdatei kann auch die Einschränkung gelten, dass der Zugriff nur mithilfe eines SAS-Tokens möglich ist.

## <a name="using-variables-to-link-templates"></a>Verwenden von Variablen für das Verknüpfen von Vorlagen
Die vorherigen Beispiele zeigen hartcodierte URL-Werte für die Vorlagenlinks. Dieser Ansatz funktioniert u. U. für eine einfache Vorlage, aber er funktioniert nicht gut bei der Arbeit mit einer großen Anzahl von modularen Vorlagen. Stattdessen können Sie eine statische Variable erstellen, die eine Basis-URL für die Hauptvorlage speichert, und dann aus dieser Basis-URL dynamisch URLs für die verknüpften Vorlagen erstellen. Der Vorteil dieses Ansatzes besteht darin, dass Sie die Vorlage problemlos verschieben oder verzweigen können, da Sie nur die statische Variable in der Hauptvorlage ändern müssen. Die Hauptvorlage übergibt die richtigen URIs an die zerlegten Vorlagen.

Das folgende Beispiel zeigt, wie Sie eine Basis-URL verwenden können, um zwei URLs für verknüpfte Vorlagen (**sharedTemplateUrl** und **vmTemplate**) zu erstellen. 

```json
"variables": {
    "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/postgresql-on-ubuntu/",
    "sharedTemplateUrl": "[concat(variables('templateBaseUrl'), 'shared-resources.json')]",
    "vmTemplateUrl": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]"
}
```

Sie können auch [deployment()](resource-group-template-functions-deployment.md#deployment) verwenden, um die Basis-URL für die aktuelle Vorlage zu erhalten. Mit dieser können Sie die URL für die anderen Vorlagen am gleichen Speicherort abrufen. Diese Vorgehensweise ist hilfreich, wenn sich der Speicherort der Vorlage ändert (möglicherweise aufgrund einer Versionsverwaltung) oder wenn Sie es vermeiden möchten, URLs in der Vorlagendatei fest programmieren zu müssen. 

```json
"variables": {
    "sharedTemplateUrl": "[uri(deployment().properties.templateLink.uri, 'shared-resources.json')]"
}
```

## <a name="complete-example"></a>Vollständiges Beispiel
Die folgenden Beispielvorlagen zeigen eine vereinfachte Anordnung verknüpfter Vorlagen zum Erläutern verschiedener Konzepte in diesem Artikel. Es wird davon ausgegangen, dass die Vorlagen demselben Container in einem Speicherkonto mit aktiviertem öffentlichen Zugriff hinzugefügt wurden. Im Abschnitt **outputs** übergibt die verknüpfte Vorlage einen Wert zurück an die Hauptvorlage.

Die Datei **parent.json** besteht aus Folgendem:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "containerSasToken": { "type": "string" }
  },
  "resources": [
    {
      "apiVersion": "2017-05-10",
      "name": "linkedTemplate",
      "type": "Microsoft.Resources/deployments",
      "properties": {
        "mode": "incremental",
        "templateLink": {
          "uri": "[concat(uri(deployment().properties.templateLink.uri, 'helloworld.json'), parameters('containerSasToken'))]",
          "contentVersion": "1.0.0.0"
        }
      }
    }
  ],
  "outputs": {
    "result": {
      "type": "object",
      "value": "[reference('linkedTemplate').outputs.result]"
    }
  }
}
```

Die Datei **helloworld.json** besteht aus Folgendem:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "variables": {},
  "resources": [],
  "outputs": {
    "result": {
        "value": "Hello World",
        "type" : "string"
    }
  }
}
```

In PowerShell rufen Sie ein Token für den Container ab und stellen die Vorlagen mit folgendem Code bereit:

```powershell
Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates
$token = New-AzureStorageContainerSASToken -Name templates -Permission r -ExpiryTime (Get-Date).AddMinutes(30.0)
$url = (Get-AzureStorageBlob -Container templates -Blob parent.json).ICloudBlob.uri.AbsoluteUri
New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateUri ($url + $token) -containerSasToken $token
```

An der Azure CLI 2.0 rufen Sie ein Token für den Container ab und stellen die Vorlagen mit folgendem Code bereit:

```azurecli
expiretime=$(date -u -d '30 minutes' +%Y-%m-%dT%H:%MZ)
connection=$(az storage account show-connection-string \
    --resource-group ManageGroup \
    --name storagecontosotemplates \
    --query connectionString)
token=$(az storage container generate-sas \
    --name templates \
    --expiry $expiretime \
    --permissions r \
    --output tsv \
    --connection-string $connection)
url=$(az storage blob url \
    --container-name templates \
    --name parent.json \
    --output tsv \
    --connection-string $connection)
parameter='{"containerSasToken":{"value":"?'$token'"}}'
az group deployment create --resource-group ExampleGroup --template-uri $url?$token --parameters $parameter
```

## <a name="next-steps"></a>Nächste Schritte
* Informationen zum Definieren der Bereitstellungsreihenfolge Ihrer Ressourcen finden Sie unter [Definieren von Abhängigkeiten in Azure-Ressourcen-Manager-Vorlagen](resource-group-define-dependencies.md)
* Informationen, wie Sie eine Ressource definieren und von dieser viele Instanzen erstellen, finden Sie unter [Erstellen mehrerer Instanzen von Ressourcen im Azure-Ressourcen-Manager](resource-group-create-multiple.md)


