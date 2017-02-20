---
title: "Automatisieren der Ressourcenbereitstellung für eine Azure Functions-App | Microsoft-Dokumentation"
description: Hier erfahren Sie, wie Sie eine Azure Resource Manager-Vorlage erstellen, die Ihre Azure Functions-App bereitstellt.
services: Functions
documtationcenter: na
author: mattchenderson
manager: erikre
editor: 
tags: 
keywords: Azure Functions, Funktionen, serverlose Architektur, Infrastruktur als Code, Azure Resource Manager
ms.assetid: 
ms.server: functions
ms.devlang: multiple
ms.topic: 
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 01/23/2017
ms.author: cfowler;glenga
translationtype: Human Translation
ms.sourcegitcommit: 360abaa575e473e18e55d0784730f4bd5635f3eb
ms.openlocfilehash: 979537bfe6b0e14a9208871fc9862661d2fb2e6c


---

# <a name="automate-resource-deployment-for-your-azure-functions-app"></a>Automatisieren der Ressourcenbereitstellung für Ihre Azure Functions-App

Sie können mithilfe einer Azure Resource Manager-Vorlage eine Azure Functions-App bereitstellen. Hier erfahren Sie, wie Sie die Grundressourcen definieren, die für eine Azure Functions-App erforderlich sind, und wie Sie die Parameter definieren, die während der Bereitstellung angegeben werden. Abhängig von den [Triggern und Bindungen](functions-triggers-bindings.md) in Ihrer Functions-App müssen Sie für eine erfolgreiche Konfiguration vom Typ „Infrastruktur als Code“ Ihrer Anwendung unter Umständen zusätzliche Ressourcen bereitstellen.

Weitere Informationen zum Erstellen von Vorlagen finden Sie unter [Erstellen von Azure Resource Manager-Vorlagen](../azure-resource-manager/resource-group-authoring-templates.md).

Beispiele für vollständige Vorlagen finden Sie unter [Erstellen einer verbrauchsplanbasierten Azure Functions-App](https://github.com/Azure/azure-quickstart-templates/blob/052db5feeba11f85d57f170d8202123511f72044/101-function-app-create-dynamic/azuredeploy.json) sowie unter [Erstellen einer auf einem App Service-Plan basierenden Azure Functions-App](https://github.com/Azure/azure-quickstart-templates/blob/master/101-function-app-create-dedicated/azuredeploy.json).

## <a name="required-resources"></a>Erforderliche Ressourcen

Mithilfe der Beispiele in diesem Artikel können Sie eine grundlegende Azure Functions-App erstellen. Für Ihre App benötigen Sie folgende Ressourcen:

* [Azure Storage](../storage/index.md)-Konto
* Hostingplan (Verbrauchsplan oder Azure App Service-Plan)
* Functions-App (`type`: **Microsoft.Web/Site**, `kind`: **functionapp**)

## <a name="parameters"></a>Parameter

Mit Azure Resource Manager können Sie die Parameter für Werte definieren, die Sie beim Bereitstellen der Vorlage angeben möchten. Der Vorlagenabschnitt **Parameter** enthält sämtliche Parameterwerte. Definieren Sie Parameter für Werte, die abhängig vom bereitzustellenden Projekt oder abhängig von der Zielumgebung für die Bereitstellung variieren.

[Variablen](../azure-resource-manager/resource-group-authoring-templates.md#variables) sind nützlich für Werte, die sich nicht basierend auf einer einzelnen Bereitstellung ändern, sowie für Parameter, die vor der Verwendung in einer Vorlage transformiert werden müssen (beispielsweise, um Überprüfungsregeln zu erfüllen).

Verwenden Sie beim Definieren von Parametern das Feld **allowedValues**, um anzugeben, welche Werte ein Benutzer im Rahmen der Bereitstellung angeben kann. Verwenden Sie das Feld **defaultValue**, um dem Parameter einen Wert zuweisen, falls im Rahmen der Bereitstellung kein Wert angegeben wurde.

In einer Azure Resource Manager-Vorlage werden folgende Parameter verwendet:

### <a name="appname"></a>appName

Der Name der Azure Functions-App, die Sie erstellen möchten.

```json
"appName": {
    "type": "string"
}
```

### <a name="location"></a>location

Der Ort, an dem die Functions-App bereitgestellt werden soll.

> [!NOTE]
> Verwenden Sie den Parameter **defaultValue**, um den Speicherort der Ressourcengruppe zu vererben oder für den Fall vorzusorgen, dass bei einer PowerShell- oder Azure CLI-Bereitstellung kein Parameterwert angegeben wird. Wenn Sie Ihre App über das Azure-Portal bereitstellen, wählen Sie im Dropdown-Parameterfeld **allowedValues** einen Wert aus.

> [!TIP]
> Eine aktuelle Liste mit Regionen, in denen Sie Azure Functions verwenden können, finden Sie unter [Verfügbare Produkte nach Region](https://azure.microsoft.com/regions/services/).

```json
"location": {
    "type": "string",
    "allowedValues": [
        "Brazil South",
        "East US",
        "East US 2",
        "Central US",
        "North Central US",
        "South Central US",
        "West US",
        "West US 2"
    ],
    "defaultValue": "[resourceGroup().location]"
}
```

### <a name="sourcecoderepositoryurl-optional"></a>sourceCodeRepositoryURL (optional)

```json
"sourceCodeRepositoryURL": {
    "type": "string",
    "defaultValue": "",
    "metadata": {
    "description": "Source code repository URL"
}
```

### <a name="sourcecodebranch-optional"></a>sourceCodeBranch (optional)

```json
    "sourceCodeBranch": {
      "type": "string",
      "defaultValue": "master",
      "metadata": {
        "description": "Source code repository branch"
      }
    }
```

### <a name="sourcecodemanualintegration-optional"></a>sourceCodeManualIntegration (optional)

```json
"sourceCodeManualIntegration": {
    "type": "bool",
    "defaultValue": false,
    "metadata": {
        "description": "Use 'true' if you are deploying from the base repo. Use 'false' if you are deploying from your own fork. If you use 'false', make sure that you have Administrator rights in the repo. If you get an error, manually add GitHub integration to another web app, to associate a GitHub access token with your Azure subscription."
    }
}
```

## <a name="variables"></a>Variablen

Parameter werden bei Azure Resource Manager-Vorlagen mithilfe von Variablen integriert, was die Verwendung spezifischerer Einstellungen in Ihrer Vorlage ermöglicht.

Im nächsten Beispiel wenden wir mithilfe von Variablen [Azure Resource Manager-Vorlagenfunktionen](../azure-resource-manager/resource-group-template-functions.md) an, um den eingegebenen Wert **appName** in Kleinbuchstaben umzuwandeln und so die [Benennungsanforderungen](../storage/storage-create-storage-account.md#create-a-storage-account) des Azure-Speicherkontos zu erfüllen.

```json
"variables": {
    "lowerSiteName": "[toLower(parameters('appName'))]",
    "storageAccountName": "[concat(variables('lowerSiteName'))]"
}
```

## <a name="resources-to-deploy"></a>Bereitzustellende Ressourcen

### <a name="storage-account"></a>Speicherkonto

Für eine Azure Functions-App wird ein Azure-Speicherkonto benötigt.

```json
{
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2015-05-01-preview",
    "location": "[variables('storageLocation')]",
    "properties": {
        "accountType": "[variables('storageAccountType')]"
    }
}
```

### <a name="hosting-plan-consumption-vs-app-service"></a>Hostingplan: Verbrauch im Vergleich zu App Service

In einigen Szenarien sollen Ihre Funktionen unter Umständen von der Plattform nach Bedarf skaliert werden. Dies wird auch als vollständig verwaltete Skalierung (unter Verwendung eines verbrauchsbasierten Hostingplans) bezeichnet. Alternativ können Sie eine benutzerverwaltete Skalierung für Ihre Funktionen verwenden. Bei der benutzerverwalteten Skalierung werden Ihre Funktionen rund um die Uhr auf dedizierter Hardware ausgeführt (unter Verwendung eines App Service-Hostingplans). Die Anzahl von Instanzen kann manuell oder automatisch festgelegt werden. Die Wahl des Hostingplans kann von den verfügbaren Features des Plans oder von der Architektur des Hosts abhängig gemacht werden. Weitere Informationen zu Hostingplänen finden Sie unter [Skalieren von Azure Functions](functions-scale.md).

#### <a name="consumption-plan"></a>Verbrauchsplan

```json
{
    "type": "Microsoft.Web/serverfarms",
    "apiVersion": "2015-04-01",
    "name": "[variables('hostingPlanName')]",
    "location": "[resourceGroup().location]",
    "properties": {
        "name": "[variables('hostingPlanName')]",
        "computeMode": "Dynamic",
        "sku": "Dynamic"
    }
}
```

#### <a name="app-service-plan"></a>App Service-Plan

```json
{
    "type": "Microsoft.Web/serverfarms",
    "apiVersion": "2015-04-01",
    "name": "[variables('hostingPlanName')]",
    "location": "[resourceGroup().location]",
    "properties": {
        "name": "[variables('hostingPlanName')]",
        "sku": "[parameters('sku')]",
        "workerSize": "[parameters('workerSize')]",
        "hostingEnvironment": "",
        "numberOfWorkers": 1
    }
}
```

### <a name="functions-app-a-site"></a>Functions-App (eine Website)

Nachdem Sie eine Skalierungsoption ausgewählt haben, können Sie eine Functions-App erstellen. Bei der App handelt es sich um den Container, der alle Ihre Funktionen enthält.

Eine Functions-App verfügt über viele untergeordnete Ressourcen, die Sie in Ihrer Bereitstellung verwenden können. Hierzu zählen beispielsweise App-Einstellungen und Quellcodeverwaltungsoptionen. Sie können die untergeordnete Ressource **sourcecontrols** auch entfernen und stattdessen eine andere [Bereitstellungsoption](functions-continuous-deployment.md) verwenden.

> [!IMPORTANT]
> Um mithilfe von Azure Resource Manager erfolgreich eine Konfiguration vom Typ „Infrastruktur als Code“ für Ihre Anwendung erstellen zu können, müssen Sie mit der Bereitstellung von Ressourcen in Azure vertraut sein. Im folgenden Beispiel werden mithilfe von **siteConfig** Konfigurationen auf oberster Ebene angewendet. Diese Konfigurationen müssen auf der obersten Ebene festgelegt werden, da sie Informationen für das Laufzeit- und Bereitstellungsmodul von Azure Functions bereitstellen. Informationen auf oberster Ebene werden benötigt, bevor die untergeordnete Ressource **sourcecontrols/web** angewendet wird. Die Einstellungen könnten zwar auch in der untergeordneten Ressource **config/appSettings** angewendet werden, in bestimmten Szenarien müssen Ihre Functions-App und die Funktionen jedoch bereitgestellt werden, *bevor* **config/appSettings** angewendet wird. In diesen Fällen (etwa in [Logic Apps](../logic-apps/index.md)) handelt es sich bei Ihren Funktionen um eine Abhängigkeit einer anderen Ressource.

```json
{
  "apiVersion": "2015-08-01",
  "name": "[parameters('appName')]",
  "type": "Microsoft.Web/sites",
  "kind": "functionapp",
  "location": "[parameters('location')]",
  "dependsOn": [
    "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]",
    "[resourceId('Microsoft.Web/serverfarms', parameters('appName'))]"
  ],
  "properties": {
     "serverFarmId": "[variables('appServicePlanName')]",
     "siteConfig": {
        "alwaysOn": true,
        "appSettings": [
            { "name": "FUNCTIONS_EXTENSION_VERSION", "value": "~1" },
            { "name": "Project", "value": "src" }
        ]
     }
  },
  "resources": [
     {
        "apiVersion": "2015-08-01",
        "name": "appsettings",
        "type": "config",
        "dependsOn": [
          "[resourceId('Microsoft.Web/Sites', parameters('appName'))]",
          "[resourceId('Microsoft.Web/Sites/sourcecontrols', parameters('appName'), 'web')]",
          "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]"
        ],
        "properties": {
          "AzureWebJobsStorage": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]",
          "AzureWebJobsDashboard": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
        }
     },
     {
          "apiVersion": "2015-08-01",
          "name": "web",
          "type": "sourcecontrols",
          "dependsOn": [
            "[resourceId('Microsoft.Web/sites/', parameters('appName'))]"
          ],
          "properties": {
            "RepoUrl": "[parameters('sourceCodeRepositoryURL')]",
            "branch": "[parameters('sourceCodeBranch')]",
            "IsManualIntegration": "[parameters('sourceCodeManualIntegration')]"
          }
     }
  ]
}
```
> [!TIP]
> In dieser Vorlage wird der App-Einstellungswert [Project](https://github.com/projectkudu/kudu/wiki/Customizing-deployments#using-app-settings-instead-of-a-deployment-file) verwendet. Dieser Wert legt das Basisverzeichnis fest, in dem das Modul zum Bereitstellen von Funktionen (Kudu) nach bereitstellbarem Code sucht. In unserem Repository befinden sich die Funktionen in einem Unterordner des Ordners **src**. Daher legen wir den App-Einstellungswert im vorherigen Beispiel auf `src` fest. Falls sich Ihre Funktionen im Stammverzeichnis des Repositorys befinden oder Sie Ihre Bereitstellung nicht über die Quellcodeverwaltung durchführen, können Sie diesen App-Einstellungswert entfernen.

## <a name="deploy-your-template"></a>Bereitstellen der Vorlage

* [PowerShell](../azure-resource-manager/resource-group-template-deploy.md)
* [Azure-Befehlszeilenschnittstelle](../azure-resource-manager/resource-group-template-deploy-cli.md)
* [Azure-Portal](../azure-resource-manager/resource-group-template-deploy-portal.md)
* [REST-API](../azure-resource-manager/resource-group-template-deploy-rest.md)

### <a name="deploy-to-azure-button"></a>Schaltfläche zum Bereitstellen in Azure

Ersetzen Sie ```<url-encoded-path-to-azuredeploy-json>``` durch eine [URL-codierte](https://www.bing.com/search?q=url+encode) Version des Rohpfads Ihrer Datei `azuredeploy.json` in GitHub.

#### <a name="markdown"></a>Markdown

```markdown
[![Deploy to Azure](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/<url-encoded-path-to-azuredeploy-json>)
```

#### <a name="html"></a>HTML

```html
<a href="https://portal.azure.com/#create/Microsoft.Template/uri/<url-encoded-path-to-azuredeploy-json>" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"></a>
```

## <a name="next-steps"></a>Nächste Schritte

Machen Sie sich näher mit dem Entwickeln und Konfigurieren von Azure Functions vertraut.

* [Entwicklerreferenz zu Azure Functions](functions-reference.md)
* [Konfigurieren von Azure Functions-App-Einstellungen](functions-how-to-use-azure-function-app-settings.md)
* [Erstellen Sie Ihre erste Funktion in Azure Functions](functions-create-first-azure-function.md)



<!--HONumber=Feb17_HO1-->


