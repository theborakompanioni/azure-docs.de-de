---
title: "Ausführen eines benutzerdefinierten Image von Docker-Hubs in Azure-Web-Apps für Container | Microsoft-Dokumentation"
description: "Informationen zum Verwenden eines benutzerdefinierten Docker-Image für Azure-Web-Apps für Container"
keywords: Azure App Service, Web-App, Linux, Docker, Container
services: app-service
documentationcenter: 
author: naziml
manager: cfowler
editor: 
ms.assetid: b97bd4e6-dff0-4976-ac20-d5c109a559a8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/05/2017
ms.author: naziml;wesmc
ms.translationtype: HT
ms.sourcegitcommit: 12c20264b14a477643a4bbc1469a8d1c0941c6e6
ms.openlocfilehash: 47eec572272eb22cd6cd881874d7ecb87d7e08ad
ms.contentlocale: de-de
ms.lasthandoff: 09/07/2017

---

# <a name="run-a-custom-docker-hub-image-in-azure-web-apps-for-containers"></a>Ausführen eines benutzerdefinierten Image von Docker-Hubs in Azure-Web-Apps für Container #

App Service stellt vordefinierte Anwendungsstapel unter Linux mit Unterstützung für bestimmte Versionen bereit, z.B. PHP 7.0 und Node.js 4.5. Sie können auch ein benutzerdefiniertes Docker-Image verwenden, um Ihre Web-App in einem Anwendungsstapel bereitzustellen, der nicht bereits in Azure definiert ist. In diesem Schnellstartartikel wird erläutert, wie eine Web-App erstellt und ein Python-Image für diese bereitgestellt wird. Sie erstellen die Web-App mithilfe der [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli).

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Configure deployment user](../../../includes/configure-deployment-user.md)] 

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group.md)] 

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux.md)] 

## <a name="create-a-web-app"></a>Erstellen einer Web-App

Erstellen Sie eine [Web-App](../../app-service-web/app-service-web-overview.md) im App Service-Plan `myAppServicePlan` mit dem Befehl [az webapp create](/cli/azure/webapp#create). Vergessen Sie nicht, `<app name>` durch einen eindeutigen App-Namen zu ersetzen.

```azurecli-interactive
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app name> --deployment-container-image-name elnably/dockerimagetest
```

Im vorhergehenden Befehl verweist `--deployment-container-image-name` auf das öffentliche Image des Docker-Hubs [https://hub.docker.com/r/elnably/dockerimagetest/](https://hub.docker.com/r/elnably/dockerimagetest/). Den jeweiligen Inhalt können Sie unter [https://github.com/rachelappel/docker-image](https://github.com/rachelappel/docker-image) prüfen.

Nach Erstellung der Web-App zeigt die Azure CLI eine Ausgabe wie im folgenden Beispiel an:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app name>.azurewebsites.net",
  "deploymentLocalGitUrl": "https://<username>@<app name>.scm.azurewebsites.net/<app name>.git",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

## <a name="browse-to-the-app"></a>Navigieren zur App

Rufen Sie folgende URL mit Ihrem Webbrowser auf:

```bash
http://<app_name>.azurewebsites.net
```

![In Azure ausgeführte Beispiel-App](media/quickstart-custom-docker-image/hello-world-in-browser.png)

**Glückwunsch!** Sie haben ein benutzerdefiniertes Docker-Image für Web-Apps für Container bereitgestellt.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen einer Docker Python- und PostgreSQL-Web-App in Azure](tutorial-docker-python-postgresql-app.md)

