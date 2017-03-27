---
title: "Erstellen Ihrer ersten Node.js-Web-App in Azure in fünf Minuten | Microsoft-Dokumentation"
description: "Erfahren Sie, wie einfach die Ausführung von Web-Apps in App Service ist, indem Sie eine Beispiel-Node.js-App bereitstellen."
services: app-service\web
documentationcenter: 
author: cephalin
manager: wpickett
editor: 
ms.assetid: 412cc786-5bf3-4e1b-b696-6a08cf46501e
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 03/17/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: 0d8472cb3b0d891d2b184621d62830d1ccd5e2e7
ms.openlocfilehash: 89a9e29261e338aceb4ff6feb55cf344afeeb3d4
ms.lasthandoff: 03/21/2017


---
# <a name="create-your-first-nodejs-web-app-in-azure-in-five-minutes"></a>Erstellen Ihrer ersten Node.js-Web-App in Azure in fünf Minuten
[!INCLUDE [app-service-web-selector-get-started](../../includes/app-service-web-selector-get-started.md)] 

Dieser Schnellstart hilft Ihnen bei der Bereitstellung Ihrer ersten Node.js-Web-App in [Azure App Service](../app-service/app-service-value-prop-what-is.md) in nur wenigen Minuten.

Bevor Sie beginnen, stellen Sie sicher, dass die Azure CLI installiert wurde. Weitere Informationen finden Sie im [Azure CLI-Installationshandbuch](https://docs.microsoft.com/cli/azure/install-azure-cli).

## <a name="log-in-to-azure"></a>Anmelden an Azure
Melden Sie sich bei Azure an, indem Sie `az login` ausführen und den Anweisungen auf dem Bildschirm folgen.
   
```azurecli
az login
```
   
## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe   
Erstellen Sie eine [Ressourcengruppe](../azure-resource-manager/resource-group-overview.md). Dieser fügen Sie alle Azure-Ressourcen hinzu, die Sie zusammen verwalten möchten, z.B. die Web-App und das zugehörige SQL-Datenbank-Back-End.

```azurecli
az group create --location "West Europe" --name myResourceGroup
```

Welche Werte Sie für `---location` verwenden können, erfahren Sie mithilfe des Azure CLI-Befehls `az appservice list-locations`.

## <a name="create-an-app-service-plan"></a>Wie erstelle ich einen Plan?
Erstellen Sie einen [App Service-Standardplan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md), für den ein Linux-Container ausgeführt wird. 

```azurecli
az appservice plan create --name my-free-appservice-plan --resource-group myResourceGroup --is-linux --sku S1
```

## <a name="create-a-web-app"></a>Erstellen einer Web-App
Erstellen Sie in `<app_name>` eine Web-App mit einem eindeutigen Namen.

```azurecli
az appservice web create --name <app_name> --resource-group myResourceGroup --plan my-free-appservice-plan
```

## <a name="configure-the-linux-container"></a>Konfigurieren des Linux-Containers
Konfigurieren Sie den Linux-Container für die Verwendung des standardmäßigen Node.js 6.9.3-Images.

```azurecli
az appservice web config update --node-version 6.9.3 --name <app_name> --resource-group myResourceGroup
```

## <a name="deploy-sample-application"></a>Bereitstellen der Beispielanwendung
Stellen Sie eine Node.js-Beispiel-App in GitHub bereit.

```azurecli
az appservice web source-control config --name <app_name> --resource-group myResourceGroup \
--repo-url "https://github.com/Azure-Samples/app-service-web-nodejs-get-started.git" --branch master --manual-integration 
```

## <a name="browse-to-web-app"></a>Navigieren zur Web-App
Führen Sie diesen Befehl aus, um die Liveausführung der App in Azure zu verfolgen.

```azurecli
az appservice web browse --name <app_name> --resource-group myResourceGroup
```

Herzlichen Glückwunsch, Ihre erste Node.js-Web-App wird live in Azure App Service ausgeführt.

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Nächste Schritte

Erkunden Sie vorab erstellte [CLI-Skripts für Web-Apps](app-service-cli-samples.md).

