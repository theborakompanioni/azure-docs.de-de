---
title: Verwalten von Web-Apps unter Linux mit Azure CLI 2.0 | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Web-Apps unter Linux mit der Azure-Befehlszeilenschnittstelle verwalten.
keywords: Azure App Service, Web-App, CLI, Linux, OSS
services: app-service
documentationCenter: 
authors: ahmedelnably
manager: erikre
editor: 
ms.assetid: 
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: aelnably
ms.translationtype: HT
ms.sourcegitcommit: cf381b43b174a104e5709ff7ce27d248a0dfdbea
ms.openlocfilehash: e0c913ef50db3572940928d9f739e26994c96981
ms.contentlocale: de-de
ms.lasthandoff: 08/23/2017

---

# <a name="manage-web-app-on-linux-using-azure-cli"></a>Verwalten von Web-Apps unter Linux mit der Azure-Befehlszeilenschnittstelle

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

Mit den Befehlen in diesem Artikel können Sie eine Web-App unter Linux mit Azure CLI 2.0 erstellen und verwalten.
Sie können die neue Version der Befehlszeilenschnittstelle auf zwei Arten einrichten:

* Durch [Installieren von Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) auf Ihrem Computer
* Mithilfe der [Azure Cloud Shell (Vorschau)](../cloud-shell/overview.md)


## <a name="create-a-linux-app-service-plan"></a>Erstellen eines Linux-App Service-Plans

Um einen Linux-App Service-Plan zu erstellen, können Sie den folgenden Befehl verwenden:

```azurecli-interactive
az appservice plan create -n appname -g rgname --islinux -l "South Central US" --sku S1 --number-of-workers 1
``` 

## <a name="create-a-custom-docker-container-web-app"></a>Erstellen einer benutzerdefinierten Docker-Container-Web-App

Zum Erstellen einer Web-App und Konfigurieren dieser für die Ausführung eines benutzerdefinierten Docker-Containers können Sie den folgenden Befehl verwenden:

```azurecli-interactive
az webapp create -n sname -g rgname -p pname -i elnably/dockerimagetest
```
 
## <a name="change-the-custom-docker-container-for-an-existing-web-app-on-linux-app"></a>Ändern des benutzerdefinierten Docker-Containers für eine vorhandene Web-App unter Linux

Wenn Sie eine bereits erstellte App vom aktuellen Docker-Image auf ein neues Image umstellen möchten, können Sie den folgenden Befehl verwenden:

```azurecli-interactive
az webapp config container set -n sname -g rgname -c apurvajo/mariohtml5
``` 

## <a name="using-docker-images-from-a-private-registry"></a>Verwenden von Docker-Images aus einer privaten Registrierung

Sie können Ihre App für die Verwendung von Images aus einer privaten Registrierung konfigurieren. Sie müssen dabei die URL für Ihre Registrierung, den Benutzernamen und das Kennwort angeben. Dies kann mithilfe des folgenden Befehls durchgeführt werden:

```azurecli-interactive
az webapp config container set -n sname1 -g rgname -c <container name> -r <server url> -u <username> -p <password>
``` 

## <a name="enable-continuous-deployments-for-custom-docker-images"></a>Aktivieren von Continuous Deployment für benutzerdefinierte Docker-Images

Mit dem folgenden Befehl aktivieren Sie die CD-Funktion und rufen die Webhook-URL ab. Diese URL kann zum Konfigurieren des DockerHubs oder Repositorys der Azure Container Registry verwendet werden.

```azurecli-interactive
az webapp deployment container config -n sname -g rgname -e true
``` 

## <a name="create-a-web-app-on-linux-app-using-one-of-our-built-in-runtime-frameworks"></a>Erstellen einer Web-App unter Linux mit einem integrierten Runtime-Framework

Verwenden Sie zum Erstellen einer Web-App unter Linux mit PHP 5.6 den folgenden Befehl.

```azurecli-interactive
az webapp create -n sname -g rgname -p pname -r "php|5.6"
``` 

## <a name="change-framework-version-for-an-existing-web-app-on-linux-app"></a>Ändern der Frameworkversion für eine vorhandene Web-App unter Linux

Wenn Sie eine bereits erstellte App von der aktuellen Frameworkversion auf Node.js 6.11 umstellen möchten, können Sie den folgenden Befehl verwenden:

```azurecli-interactive
az webapp config set -n sname -g rgname --linux-fx-version "node|6.11"
``` 

## <a name="set-up-git-deployments-for-your-web-app"></a>Einrichten von Git-Bereitstellungen für Ihre Web-App

Zum Einrichten von Git-Bereitstellungen für Ihre App können Sie den folgenden Befehl verwenden:

```azurecli-interactive
az webapp deployment source config -n sname -g rgname --repo-url <gitrepo url> --branch <branch>
``` 


## <a name="next-steps"></a>Nächste Schritte
* [Was ist Azure-Web-App unter Linux?](app-service-linux-intro.md)
* [Installieren der Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli)
* [Azure Cloud Shell (Vorschau)](../cloud-shell/overview.md)
* [Erstellen von Web-Apps in Azure-Web-App unter Linux](app-service-linux-how-to-create-web-app.md)
* [Einrichten von Stagingumgebungen in Azure App Service](./web-sites-staged-publishing.md)
* [Continuous Deployment mit Azure-Web-App unter Linux](./app-service-linux-ci-cd.md)

