---
title: "Verwalten von Web-Apps für Container mithilfe der Azure CLI 2.0 | Microsoft-Dokumentation"
description: "Verwalten von Web-Apps für Container mithilfe der Azure CLI"
keywords: Azure App Service, Web-App, CLI, Linux, OSS
services: app-service
documentationCenter: 
author: ahmedelnably
manager: cfowler
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
ms.sourcegitcommit: 12c20264b14a477643a4bbc1469a8d1c0941c6e6
ms.openlocfilehash: d58fab0b423b7bc1382a82f4bf308b6ad7286296
ms.contentlocale: de-de
ms.lasthandoff: 09/07/2017

---
# <a name="manage-web-apps-for-containers-using-azure-cli"></a>Verwalten von Web-Apps für Container mithilfe der Azure CLI

Mit den Befehlen in diesem Artikel können Sie eine Web-App für Container mit der Azure CLI 2.0 erstellen und verwalten.
Sie können die neue Version der Befehlszeilenschnittstelle auf zwei Arten einrichten:

* Durch [Installieren von Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) auf Ihrem Computer
* Mithilfe der [Azure Cloud Shell (Vorschau)](../../cloud-shell/overview.md)

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

## <a name="activate-the-docker-container-logging"></a>Aktivieren der Protokollierung von Docker-Containern

Um die Protokollierung von Docker-Containern zu aktivieren, können Sie den folgenden Befehl verwenden:

```azurecli-interactive
az webapp log config -n sname -g rgname --web-server-logging filesystem
```

## <a name="change-the-custom-docker-container-for-an-existing-web-apps-for-containers-app"></a>Ändern des benutzerdefinierten Docker-Containers für eine vorhandene Web-App für Container

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

## <a name="create-a-web-apps-for-containers-app-using-one-of-our-built-in-runtime-frameworks"></a>Erstellen einer Web-App für Container mit einem integrierten Laufzeitframework

Verwenden Sie zum Erstellen einer Web-App für Container mit PHP 5.6 den folgenden Befehl.

```azurecli-interactive
az webapp create -n sname -g rgname -p pname -r "php|5.6"
```

## <a name="change-framework-version-for-an-existing-web-apps-for-containers-app"></a>Ändern der Frameworkversion für eine vorhandene Web-App für Container

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

* [Was sind Azure-Web-Apps für Container?](app-service-linux-intro.md)
* [Installieren der Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli)
* [Azure Cloud Shell (Vorschau)](../../cloud-shell/overview.md)
* [Einrichten von Stagingumgebungen in Azure App Service](../../app-service-web/web-sites-staged-publishing.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
* [Continuous Deployment mit Azure-Web-Apps für Container](app-service-linux-ci-cd.md)

