---
title: "Automatisieren der Ressourcenbereitstellung für eine Funktions-App in Azure Functions | Microsoft-Dokumentation"
description: Hier erfahren Sie, wie Sie eine Azure Resource Manager-Vorlage erstellen, die Ihre Funktions-App bereitstellt.
services: Functions
documtationcenter: na
author: lindydonna
manager: erikre
editor: 
tags: 
keywords: Azure Functions, Funktionen, serverlose Architektur, Infrastruktur als Code, Azure Resource Manager
ms.assetid: d20743e3-aab6-442c-a836-9bcea09bfd32
ms.server: functions
ms.devlang: multiple
ms.topic: 
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/25/2017
ms.author: donnam;glenga
ms.translationtype: Human Translation
ms.sourcegitcommit: 532ff423ff53567b6ce40c0ea7ec09a689cee1e7
ms.openlocfilehash: 9458b3b619649d094ddab1638e146571d9268fb0
ms.contentlocale: de-de
ms.lasthandoff: 06/05/2017


---

# <a name="automate-resource-deployment-for-your-function-app-in-azure-functions"></a>Automatisieren der Ressourcenbereitstellung für Ihre Funktions-App in Azure Functions

Sie können mithilfe einer Azure Resource Manager-Vorlage eine Funktions-App bereitstellen. In diesem Artikel werden die erforderlichen Ressourcen und die Parameter hierfür beschrieben. Abhängig von den [Triggern und Bindungen](functions-triggers-bindings.md) in Ihrer Funktions-App müssen Sie möglicherweise weitere Ressourcen bereitstellen.

Weitere Informationen zum Erstellen von Vorlagen finden Sie unter [Erstellen von Azure Resource Manager-Vorlagen](../azure-resource-manager/resource-group-authoring-templates.md).

Beispielvorlagen finden Sie unter:
- [Funktions-App im Verbrauchsplan]
- [Funktions-App im Azure App Service-Plan]

## <a name="required-resources"></a>Erforderliche Ressourcen

Eine Funktions-App benötigt diese Ressourcen:

* Ein [Azure Storage](../storage/index.md)-Konto
* Ein Hostingplan (Verbrauchsplan oder App Service-Plan)
* Eine Funktions-App 

### <a name="storage-account"></a>Speicherkonto

Für eine Funktions-App wird ein Azure Storage-Konto benötigt. Sie benötigen ein Konto für allgemeine Zwecke, das Blobs, Tabellen, Warteschlangen und Dateien unterstützt. Weitere Informationen finden Sie unter [Anforderungen an das Speicherkonto](functions-create-function-app-portal.md#storage-account-requirements).

```json
{
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2015-06-15",
    "location": "[resourceGroup().location]",
    "properties": {
        "accountType": "[parameters('storageAccountType')]"
    }
}
```

Darüber hinaus müssen die Eigenschaften `AzureWebJobsStorage` und `AzureWebJobsDashboard` als App-Einstellungen in der Standortkonfiguration angegeben werden. Die Azure Functions-Laufzeit verwendet die Verbindungszeichenfolge `AzureWebJobsStorage` zum Erstellen interner Warteschlangen. Die Verbindungszeichenfolge `AzureWebJobsDashboard` wird zur Anmeldung beim Azure-Tabellenspeicher und zur Nutzung der Registerkarte **Überwachen** im Verwaltungsportal verwendet.

Diese Eigenschaften werden in der `appSettings`-Sammlung im `siteConfig`-Objekt angegeben:

```json
"appSettings": [
    {
        "name": "AzureWebJobsStorage",
        "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
    },
    {
        "name": "AzureWebJobsDashboard",
        "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
    }
```    

### <a name="hosting-plan"></a>Hostingplan

Die Definition des Hostingplans variiert abhängig davon, ob Sie einen Verbrauchs- oder App Service-Plan verwenden. Weitere Informationen finden Sie unter [Bereitstellen einer Funktions-App im Verbrauchsplan](#consumption) und [Bereitstellen einer Funktions-App im App Service-Plan](#app-service-plan).

### <a name="function-app"></a>Funktionen-App

Die Ressource für die Funktions-App wird mithilfe einer Ressource vom Typ **Microsoft.Web/Site** und Art **functionapp** definiert:

```json
{
    "apiVersion": "2015-08-01",
    "type": "Microsoft.Web/sites",
    "name": "[variables('functionAppName')]",
    "location": "[resourceGroup().location]",
    "kind": "functionapp",            
    "dependsOn": [
        "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
        "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]"
    ]
```

<a name="consumption"></a>

## <a name="deploy-a-function-app-on-the-consumption-plan"></a>Bereitstellen einer Funktions-App im Verbrauchsplan

Sie können eine Funktions-App in zwei verschiedenen Modi ausführen: dem Verbrauchsplan und dem App Service-Plan. Der Verbrauchsplan weist automatisch Computeleistung zu, wenn Ihr Code ausgeführt wird, und skaliert diese bei Bedarf horizontal hoch, um die Last zu verarbeiten. Wird der Code nicht mehr ausgeführt, wird die Leistung wieder herunterskaliert. Deshalb müssen Sie für VMs im Leerlauf nicht bezahlen und auch keine Kapazitäten im Voraus reservieren. Weitere Informationen zu Hostingplänen finden Sie unter [Verbrauchs- und App Service-Pläne für Azure Functions](functions-scale.md).

Eine Beispielvorlage für den Azure Resource Manager finden Sie unter [Funktions-App im Verbrauchsplan].

### <a name="create-a-consumption-plan"></a>Erstellen eines Verbrauchsplans

Ein Verbrauchsplan ist eine besondere Art von „Serverfarm“-Ressource. Geben Sie sie mithilfe des `Dynamic`-Werts für die Eigenschaften `computeMode` und `sku` an:

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

### <a name="create-a-function-app"></a>Erstellen einer Funktionen-App

Darüber hinaus erfordert ein Verbrauchsplan zwei zusätzliche Einstellungen in der Standortkonfiguration: `WEBSITE_CONTENTAZUREFILECONNECTIONSTRING` und `WEBSITE_CONTENTSHARE`. Diese Eigenschaften konfigurieren das Speicherkonto und den Dateipfad zu dem Speicherort von Funktions-App-Code und Konfiguration.

```json
{
    "apiVersion": "2015-08-01",
    "type": "Microsoft.Web/sites",
    "name": "[variables('functionAppName')]",
    "location": "[resourceGroup().location]",
    "kind": "functionapp",            
    "dependsOn": [
        "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
        "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]"
    ],
    "properties": {
        "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
        "siteConfig": {
            "appSettings": [
                {
                    "name": "AzureWebJobsDashboard",
                    "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
                },
                {
                    "name": "AzureWebJobsStorage",
                    "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
                },
                {
                    "name": "WEBSITE_CONTENTAZUREFILECONNECTIONSTRING",
                    "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
                },
                {
                    "name": "WEBSITE_CONTENTSHARE",
                    "value": "[toLower(variables('functionAppName'))]"
                },
                {
                    "name": "FUNCTIONS_EXTENSION_VERSION",
                    "value": "~1"
                }
            ]
        }
    }
}
```                    

<a name="app-service-plan"></a> 

## <a name="deploy-a-function-app-on-the-app-service-plan"></a>Bereitstellen einer Funktions-App im App Service-Plan

In einem App Service-Plan wird Ihre Funktions-App ähnlich wie Web-Apps auf dedizierten virtuellen Computern für Basic-, Standard- und Premium-SKUs ausgeführt. Weitere Informationen zur Funktionsweise von App Service-Plänen finden Sie unter [Azure App Service-Pläne – Detaillierte Übersicht](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md). 

Eine Beispielvorlage für den Azure Resource Manager finden Sie unter [Funktions-App im Azure App Service-Plan].

### <a name="create-an-app-service-plan"></a>Wie erstelle ich einen Plan?

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

### <a name="create-a-function-app"></a>Erstellen einer Funktionen-App 

Nachdem Sie eine Skalierungsoption ausgewählt haben, können Sie eine Funktions-App erstellen. Bei der App handelt es sich um den Container, der alle Ihre Funktionen enthält.

Eine Funktions-App verfügt über viele untergeordnete Ressourcen, die Sie in Ihrer Bereitstellung verwenden können. Hierzu zählen beispielsweise App-Einstellungen und Quellcodeverwaltungsoptionen. Sie können die untergeordnete Ressource **sourcecontrols** auch entfernen und stattdessen eine andere [Bereitstellungsoption](functions-continuous-deployment.md) verwenden.

> [!IMPORTANT]
> Um Ihre Anwendung erfolgreich mithilfe von Azure Resource Manager bereitzustellen, müssen Sie mit der Bereitstellung von Ressourcen in Azure vertraut sein. Im folgenden Beispiel werden mithilfe von **siteConfig** Konfigurationen auf oberster Ebene angewendet. Diese Konfigurationen müssen auf der obersten Ebene festgelegt werden, da sie Informationen für das Laufzeit- und Bereitstellungsmodul von Functions bereitstellen. Informationen auf oberster Ebene werden benötigt, bevor die untergeordnete Ressource **sourcecontrols/web** angewendet wird. Die Einstellungen könnten zwar auch in der untergeordneten Ressource **config/appSettings** angewendet werden, in bestimmten Fällen müssen Ihre Funktions-App und die Funktionen jedoch bereitgestellt werden, *bevor* **config/appSettings** angewendet wird. Wenn Sie z.B. Funktionen mit [Logic Apps](../logic-apps/index.md) verwenden, handelt es sich bei Ihren Funktionen um eine Abhängigkeit einer anderen Ressource.

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

Sie können Ihre Vorlage mit einer der folgenden Methoden bereitstellen:

* [PowerShell](../azure-resource-manager/resource-group-template-deploy.md)
* [Azure-Befehlszeilenschnittstelle](../azure-resource-manager/resource-group-template-deploy-cli.md)
* [Azure-Portal](../azure-resource-manager/resource-group-template-deploy-portal.md)
* [REST-API](../azure-resource-manager/resource-group-template-deploy-rest.md)

### <a name="deploy-to-azure-button"></a>Schaltfläche zum Bereitstellen in Azure

Ersetzen Sie ```<url-encoded-path-to-azuredeploy-json>``` durch eine [URL-codierte](https://www.bing.com/search?q=url+encode) Version des Rohpfads Ihrer Datei `azuredeploy.json` in GitHub.

Hier ist ein Beispiel unter Verwendung von Markdown:

```markdown
[![Deploy to Azure](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/<url-encoded-path-to-azuredeploy-json>)
```

Hier ist ein Beispiel unter Verwendung von HTML:

```html
<a href="https://portal.azure.com/#create/Microsoft.Template/uri/<url-encoded-path-to-azuredeploy-json>" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"></a>
```

## <a name="next-steps"></a>Nächste Schritte

Machen Sie sich näher mit dem Entwickeln und Konfigurieren von Azure Functions vertraut.

* [Entwicklerreferenz zu Azure Functions](functions-reference.md)
* [Konfigurieren von Azure-Funktions-App-Einstellungen](functions-how-to-use-azure-function-app-settings.md)
* [Erstellen Sie Ihre erste Funktion in Azure Functions](functions-create-first-azure-function.md)

<!-- LINKS -->

[Funktions-App im Verbrauchsplan]: https://github.com/Azure/azure-quickstart-templates/blob/master/101-function-app-create-dynamic/azuredeploy.json
[Funktions-App im Azure App Service-Plan]: https://github.com/Azure/azure-quickstart-templates/blob/master/101-function-app-create-dedicated/azuredeploy.json

