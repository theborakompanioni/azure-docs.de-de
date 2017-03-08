---
title: "Bereitstellen einer Web-App, die mit einem GitHub-Repository verknüpft ist | Microsoft Docs"
description: "Verwenden Sie eine Azure-Ressourcen-Manager-Vorlage, um eine Web-App bereitzustellen, die ein Projekt aus einem GitHub-Repository enthält."
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: 32739607-85fe-43c8-a4dc-1feb46d93a4d
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2016
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: 0921b01bc930f633f39aba07b7899ad60bd6a234
ms.openlocfilehash: 92408d6358516cdb2cea068553757c036143e955
ms.lasthandoff: 03/01/2017


---
# <a name="deploy-a-web-app-linked-to-a-github-repository"></a>Bereitstellen einer Web-App in einem GitHub-Repository
In diesem Thema erfahren Sie, wie Sie eine Azure-Ressourcen-Manager-Vorlage erstellen, die eine Web-App bereitstellt, die mit einem Projekt in einem GitHub-Repository verknüpft ist. Sie erfahren, wie Sie definieren, welche Ressourcen bereitgestellt werden und wie Sie Parameter definieren, die angegeben werden, wenn die Bereitstellung ausgeführt wird. Sie können diese Vorlage für Ihre eigenen Bereitstellungen verwenden oder an Ihre Anforderungen anpassen.

Weitere Informationen zum Erstellen von Vorlagen finden Sie unter [Erstellen von Azure-Ressourcen-Manager-Vorlagen](../azure-resource-manager/resource-group-authoring-templates.md).

Die vollständige Vorlage finden Sie unter [Web-App verknüpft mit GitHub-Vorlage](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-github-deploy/azuredeploy.json).

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="what-you-will-deploy"></a>Was Sie bereitstellen
Mit dieser Vorlage stellen Sie eine Web-App mit dem Code aus einem Projekt in GitHub bereit.

Klicken Sie auf folgende Schaltfläche, um die Bereitstellung automatisch auszuführen:

[![In Azure bereitstellen](./media/app-service-web-arm-from-github-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-github-deploy%2Fazuredeploy.json)

## <a name="parameters"></a>Parameter
[!INCLUDE [app-service-web-deploy-web-parameters](../../includes/app-service-web-deploy-web-parameters.md)]

### <a name="repourl"></a>repoURL
URL des GitHub-Repositorys, das das bereitzustellende Projekt enthält. Dieser Parameter enthält einen Standardwert, der aber nur den Zweck hat, Ihnen zu zeigen, wie Sie die URL für das Repository angeben. Sie können diesen Wert verwenden, wenn Sie die Vorlage testen. Sie müssen allerdings die URL Ihres eigenen Repositorys angeben, wenn Sie mit der Vorlage arbeiten.

    "repoURL": {
        "type": "string",
        "defaultValue": "https://github.com/davidebbo-test/Mvc52Application.git"
    }

### <a name="branch"></a>Verzweigung
Die beim Bereitstellen der Anwendung zu verwendende Verzweigung des Repositorys. Der Standardwert ist "master", doch Sie können den Namen einer beliebigen Verzweigung im Repository angeben, die Sie bereitstellen möchten.

    "branch": {
        "type": "string",
        "defaultValue": "master"
    }

## <a name="resources-to-deploy"></a>Bereitzustellende Ressourcen
[!INCLUDE [app-service-web-deploy-web-host](../../includes/app-service-web-deploy-web-host.md)]

### <a name="web-app"></a>Web-App
Erstellt die Web-App, die mit dem Projekt in GitHub verknüpft ist. 

Sie geben den Namen der Web-App über den **siteName**-Parameter und den Speicherort der Web-App über den **siteLocation**-Parameter an. Im **dependsOn** -Element definiert die Vorlage die Web-App als abhängig vom Diensthostingplan. Da sie vom Hostingplan abhängig ist, wird die Web-App erst erstellt, wenn der Hostingplan vollständig erstellt wurde. Das **dependsOn** -Element wird nur verwendet, um die Bereitstellungsreihenfolge anzugeben. Wenn Sie die Webanwendung nicht als abhängig vom Webhostingplan markieren, versucht der Azure-Ressourcen-Manager, beide Ressourcen zur gleichen Zeit zu erstellen, und Sie erhalten möglicherweise einen Fehler, wenn die Web-App vor dem Hostingplan erstellt wird.

Die Web-App verfügt auch über eine untergeordnete Ressource, die unter **Ressourcen** weiter unten definiert wird. Diese untergeordnete Ressource definiert die Quellcodeverwaltung für das Projekt, das mit der Web-App bereitgestellt wird. In dieser Vorlage wird die Quellcodeverwaltung mit einem bestimmten GitHub-Repository verknüpft. Das GitHub-Repository wird durch den Code **"RepoUrl": "https://github.com/davidebbo-test/Mvc52Application.git"** definiert. Sie können die Repository-URL hart codieren, wenn Sie eine Vorlage erstellen möchten, die ein einzelnes Projekt wiederholt bereitstellt, während die minimale Anzahl von Parametern erforderlich ist.
Anstelle einer hart codierten Repository-URL können Sie auch einen Parameter für die Repository-URL hinzufügen und diesen Wert für die **RepoUrl**-Eigenschaft verwenden.

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('siteName')]",
      "type": "Microsoft.Web/sites",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[resourceId('Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
      ],
      "properties": {
        "serverFarmId": "[parameters('hostingPlanName')]"
      },
      "resources": [
        {
          "apiVersion": "2015-08-01",
          "name": "web",
          "type": "sourcecontrols",
          "dependsOn": [
            "[resourceId('Microsoft.Web/Sites', parameters('siteName'))]"
          ],
          "properties": {
            "RepoUrl": "[parameters('repoURL')]",
            "branch": "[parameters('branch')]",
            "IsManualIntegration": true
          }
        }
      ]
    }

## <a name="commands-to-run-deployment"></a>Befehle zum Ausführen der Bereitstellung
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell
    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-github-deploy/azuredeploy.json -siteName ExampleSite -hostingPlanName ExamplePlan -siteLocation "West US" -ResourceGroupName ExampleDeployGroup

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

    azure group deployment create -g {resource-group-name} --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-github-deploy/azuredeploy.json

### <a name="azure-cli-20"></a>Azure CLI 2.0

    az group deployment create -g {resource-group-name} --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-github-deploy/azuredeploy.json --parameters '@azuredeploy.parameters.json'

> [!NOTE] 
> Den Inhalt der JSON-Parameterdatei finden Sie in [azuredeploy.parameters.json](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-github-deploy/azuredeploy.parameters.json).
>
>


