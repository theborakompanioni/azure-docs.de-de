---
title: "Automatisieren der Ressourcenbereitstellung für Azure Functions | Microsoft-Dokumentation"
description: "Es wird beschrieben, wie Sie eine Azure Resource Manager-Vorlage erstellen, mit der Ihre Funktionen-App für Microsoft Azure bereitgestellt wird."
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
ms.sourcegitcommit: d11ef8865d21dfe4e56bcb6fa08ce58c53f3d309
ms.openlocfilehash: 7c55a1d34df71c2c958f42f43810fbce7bd74e5d


---

# <a name="automate-the-deployment-of-resources-for-your-azure-functions-app"></a>Automatisieren der Bereitstellung von Ressourcen für Ihre Azure-Funktionen-App

In diesem Thema erfahren Sie, wie Sie eine Azure Resource Manager-Vorlage erstellen, mit der eine Funktionen-App bereitgestellt wird. Sie erfahren, wie Sie die Ressourcenbasis, die für eine Azure Function erforderlich ist, und die Parameter definieren, die beim Ausführen der Bereitstellung angegeben werden. Je nach den [Triggern und Bindungen](functions-triggers-bindings.md), die in Ihrer Funktion verwendet werden, müssen Sie ggf. weitere Ressourcen bereitstellen, um für Ihre gesamte Anwendung „Infrastruktur als Code“ festzulegen.

Weitere Informationen zum Erstellen von Vorlagen finden Sie unter [Erstellen von Azure Resource Manager-Vorlagen](../azure-resource-manager/resource-group-authoring-templates.md).

Beispiele für eine vollständige Vorlage finden Sie unter [Create consumption-based Azure Function](https://github.com/Azure/azure-quickstart-templates/blob/052db5feeba11f85d57f170d8202123511f72044/101-function-app-create-dynamic/azuredeploy.json) (Erstellen einer nutzungsbasierten Azure Function) und [Create an App Service Plan based Azure Function](https://github.com/Azure/azure-quickstart-templates/blob/master/101-function-app-create-dedicated/azuredeploy.json) (Erstellen einer Azure Function, die auf einem App Service-Plan basiert).

## <a name="what-is-deployed"></a>Umfang der Bereitstellung

Mit den Beispielen unten erstellen Sie eine einfache Azure-Funktionen-App. Für eine Funktionen-App werden die folgenden Ressourcen benötigt:

* [Azure Storage](../storage/index.md)-Konto
* Hostingplan (Verbrauchsplan oder App Service-Plan)
* Funktionen-App (Microsoft.Web/Site vom Typ **functionapp**)

## <a name="parameters"></a>Parameter

Mit dem Azure-Ressourcen-Manager definieren Sie die Parameter für Werte, die Sie bei der Bereitstellung der Vorlage angeben möchten. Die Vorlage enthält den Abschnitt „Parameters“ mit allen Parameterwerten. Definieren Sie Parameter für die Werte, die basierend auf dem bereitgestellten Projekt oder der bereitgestellten Umgebung variieren.

[Variablen](../azure-resource-manager/resource-group-authoring-templates.md#variables) sind nützlich für Werte, die sich nicht basierend auf einer einzelnen Bereitstellung ändern, oder für Parameter, für die vor der Verwendung in einer Vorlage eine Transformation erforderlich ist (z.B. zum Übergeben von Überprüfungsregeln).

Verwenden Sie beim Definieren von Parametern das Feld **allowedValues** , um anzugeben, welche Werte eine Benutzer während der Bereitstellung angeben kann. Verwenden Sie das Feld **defaultValue** , um dem Parameter einen Wert zuweisen, wenn kein Wert während der Bereitstellung angegeben wird.

Als Nächstes werden die Parameter für unsere Vorlage beschrieben.

### <a name="appname"></a>appName

Der Name der Funktion, die Sie erstellen möchten.

```json
"appName": {
    "type": "string"
}
```

### <a name="location"></a>location

Der Speicherort, an dem die Funktionen-App bereitgestellt werden soll.

> [!NOTE]
> Der Parameter **defaultValue** wird verwendet, um den Speicherort der Ressourcengruppe zu vererben, oder wenn bei einer PowerShell- oder CLI-Bereitstellung kein Parameterwert angegeben wird. Bei der Bereitstellung über das Portal wird ein Dropdownfeld angegeben, in dem Sie eine Auswahl aus **allowedValues** treffen können.

> [!TIP]
> Eine aktuelle Liste mit den Regionen, in denen Azure Functions verfügbar ist, finden Sie auf der Seite [Verfügbare Produkte nach Region](https://azure.microsoft.com/regions/services/).

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
        "description": "Sourcecode Repo branch"
      }
    }
```

### <a name="sourcecodemanualintegration-optional"></a>sourceCodeManualIntegration (optional)

```json
"sourceCodeManualIntegration": {
    "type": "bool",
    "defaultValue": false,
    "metadata": {
        "description": "Use 'true' if you are deploying from the base repo, 'false' if you are deploying from your own fork. If you're using 'false', make sure you have admin permissions to the repo. If you get an error, you should add GitHub integration to another web app manually, so that you get a GitHub access token associated with your Azure Subscription."
    }
}
```

## <a name="variables"></a>Variablen

Zusätzlich zu den Parametern verfügen Azure Resource Manager-Vorlagen auch über ein Variablenkonzept, bei dem Parameter zum Erstellen von spezifischeren Einstellungen für die Verwendung in der Vorlage genutzt werden können.

Im Beispiel unten sehen Sie, dass Variablen genutzt werden, um [Azure Resource Manager-Vorlagenfunktionen](../azure-resource-manager/resource-group-template-functions.md) anzuwenden und den angegebenen appName in Kleinbuchstaben zu konvertieren. So wird sichergestellt, dass die [Benennungsanforderungen](../storage/storage-create-storage-account.md#create-a-storage-account) für Azure Storage-Konten erfüllt werden.

```json
"variables": {
    "lowerSiteName": "[toLower(parameters('appName'))]",
    "storageAccountName": "[concat(variables('lowerSiteName'))]"
}
```

## <a name="resources-to-deploy"></a>Bereitzustellende Ressourcen

### <a name="storage-account"></a>Speicherkonto

Ein Azure Storage-Konto ist in Azure Functions eine erforderliche Ressource.

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

### <a name="hosting-plan-consumption-vs-app-service-plan"></a>Hostingplan: Verbrauchsplan oder App Service-Plan

Bei der Erstellung von Funktionen kann es Fälle geben, in denen die Funktionen eine vollständig verwaltete Skalierung aufweisen. Dies bedeutet, dass sie von der Plattform bedarfsabhängig skaliert werden (Verbrauch). Alternativ hierzu können Sie auch die vom Benutzer verwaltete Skalierung wählen, bei der Ihre Funktionen rund um die Uhr auf dedizierter Hardware (App Service-Plan) ausgeführt werden, wobei die Anzahl von Instanzen manuell oder automatisch konfiguriert werden kann. Die Entscheidung für einen Plan gegenüber einem anderen kann auf den verfügbaren Features im Plan oder auf einer Entscheidung in Bezug auf die Architektur bzw. die Kosten basieren. Weitere Informationen zu den unterschiedlichen Hostingplänen finden Sie im Artikel [Skalieren von Azure Functions](functions-scale.md).

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

#### <a name="app-service-plan"></a>App Services-Plan

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

### <a name="function-app-site"></a>Funktionen-App (Standort)

Nach dem Auswählen der Skalierungsoption wird der Container erstellt, der alle Funktionen enthält. Dies wird als Funktionen-App bezeichnet.

Eine Funktionen-App verfügt über viele untergeordnete Ressourcen, die Sie nutzen können, z.B. **App-Einstellungen** und **Optionen der Quellcodeverwaltung**. Sie können sich dafür entscheiden, die untergeordnete Ressource **sourcecontrols** zu entfernen und eine andere [Bereitstellungsoption](functions-continuous-deployment.md) zu verwenden.

> [!IMPORTANT]
> Es ist wichtig zu verstehen, wie Ressourcen in Azure bereitgestellt werden. So wird sichergestellt, dass Sie eine erfolgreiche Konfiguration vom Typ „Infrastruktur als Code“ für Ihre Anwendung erstellen, wenn Sie Azure Resource Manager verwenden. In diesem Beispiel sehen Sie, dass Konfigurationen der obersten Ebene mit **siteConfig** angewendet werden. Es ist wichtig, dies auf der obersten Ebene festzulegen, weil so der Azure Functions-Laufzeit und dem Bereitstellungsmodul Bedeutung verliehen wird. Dies ist erforderlich, bevor die untergeordnete Ressource **sourcecontrols/web** angewendet wird. Diese Einstellungen können auch in der untergeordneten Ressource **config/appSettings** konfiguriert werden. Es gibt aber Szenarien, in denen die Funktionen-App und Funktionen bereitgestellt werden müssen, *bevor* **config/appsettings** angewendet wird. Ihre Funktionen stellen nämlich eine Abhängigkeit von einer anderen Ressource dar, z.B. einer [Logik-App](../logic-apps/index.md).

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
> In dieser Vorlage wird die App-Einstellung [Project](https://github.com/projectkudu/kudu/wiki/Customizing-deployments#using-app-settings-instead-of-a-deployment-file) verwendet. Hiermit wird das Basisverzeichnis festgelegt, in dem das Modul zum Bereitstellen von Funktionen (Kudu) nach bereitstellbarem Code sucht. In diesem Beispiel haben wir diesen Wert auf `src` festgelegt, da unser GitHub-Repository den Ordner `src` enthält, dem unsere Funktionen untergeordnet sind. Wenn Sie ein Repository verwenden, bei dem sich Ihre Funktionen direkt im Repositorystamm befinden, oder wenn Sie keine Bereitstellung aus der Quellcodeverwaltung durchführen, kann diese App-Einstellung entfernt werden.

## <a name="deploying-your-template"></a>Bereitstellen der Vorlage

* [PowerShell](../azure-resource-manager/resource-group-template-deploy.md)
* [BEFEHLSZEILENSCHNITTSTELLE (CLI)](../azure-resource-manager/resource-group-template-deploy-cli.md)
* [Portal](../azure-resource-manager/resource-group-template-deploy-portal.md)
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

Da Sie eine Funktionen-App jetzt per Code bereitstellen können, sollten Sie die Gelegenheit nutzen, mehr über die Entwicklung und Konfiguration von Azure Functions zu erfahren:

* [Entwicklerreferenz zu Azure Functions](functions-reference.md)
* [Konfigurieren von Azure Functions-App-Einstellungen](functions-how-to-use-azure-function-app-settings.md)
* [Erstellen Sie Ihre erste Funktion in Azure Functions](functions-create-first-azure-function.md)


<!--HONumber=Jan17_HO4-->


