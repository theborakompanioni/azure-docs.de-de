---
title: "Continuous Deployment mit Azure-Web-App für Container | Microsoft-Dokumentation"
description: "Informationen zum Einrichten von Continuous Deployment in Azure-Web-App für Container"
keywords: Azure App Service, Linux, OSS, ACR
services: app-service
documentationcenter: 
author: ahmedelnably
manager: erikre
editor: 
ms.assetid: a47fb43a-bbbd-4751-bdc1-cd382eae49f8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: aelnably;wesmc
ms.translationtype: HT
ms.sourcegitcommit: fda37c1cb0b66a8adb989473f627405ede36ab76
ms.openlocfilehash: 69eb021014c2ae04fc71a03cd27c0e113a2b062b
ms.contentlocale: de-de
ms.lasthandoff: 09/14/2017

---
# <a name="continuous-deployment-with-azure-web-app-for-containers"></a>Continuous Deployment mit Azure-Web-App für Container

In diesem Tutorial konfigurieren Sie Continuous Deployment für ein benutzerdefiniertes Containerimage aus verwalteten [Azure Container Registry](https://azure.microsoft.com/en-us/services/container-registry/)-Repositorys oder aus [Docker Hub](https://hub.docker.com).

## <a name="step-1---sign-in-to-azure"></a>Schritt 1: Anmelden bei Azure

Melden Sie sich unter „http://portal.azure.com“ beim Azure-Portal an.

## <a name="step-2---enable-container-continuous-deployment-feature"></a>Schritt 2: Aktivieren von Continuous Deployment für Container

Sie können das Feature Continuous Deployment mit der [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) durch Ausführen des folgenden Befehls aktivieren.

```azurecli-interactive
az webapp deployment container config -n sname -g rgname -e true
``` 

Klicken Sie im **[Azure-Portal](https://portal.azure.com/)** auf der linken Seite auf **App Service**.

Klicken Sie auf den Namen der App, für die Sie Docker Hub Continuous Deployment konfigurieren möchten.

Fügen Sie in den **App-Einstellungen** eine App-Einstellung mit dem Namen `DOCKER_ENABLE_CI` und dem Wert `true` hinzu.

![Abbildung einfügen: App-Einstellung](./media/app-service-webapp-service-linux-ci-cd/step2.png)

## <a name="step-3---prepare-webhook-url"></a>Schritt 3: Vorbereiten der Webhook-URL

Sie können die Webhook-URL mit der [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) durch Ausführen des folgenden Befehls abrufen.

```azurecli-interactive
az webapp deployment container show-cd-url -n sname1 -g rgname
``` 

Für die Webhook-URL benötigen Sie den folgenden Endpunkt: `https://<publishingusername>:<publishingpwd>@<sitename>.scm.azurewebsites.net/docker/hook`.

Sie können `publishingusername` und `publishingpwd` durch Herunterladen des Web-App-Veröffentlichungsprofils im Azure-Portal abrufen.

![Abbildung einfügen: Hinzufügen von Webhook 2](./media/app-service-webapp-service-linux-ci-cd/step3-3.png)

## <a name="step-4---add-a-web-hook"></a>Schritt 4:Hinzufügen eines Webhooks

### <a name="azure-container-registry"></a>Azure-Containerregistrierung

Klicken Sie auf dem Registrierungsportalblatt auf **Webhooks** und anschließend auf **Hinzufügen**, um einen neuen Webhook hinzuzufügen. Geben Sie dem Webhook auf dem Blatt **Webhook erstellen** einen Namen. Geben Sie als Webhook-URI die URL aus **Schritt 3** an.

Legen Sie den Bereich auf das Repository fest, das Ihr Containerimage enthält.

![Webhookbild einfügen](./media/app-service-webapp-service-linux-ci-cd/step3ACRWebhook-1.png)

Wenn das Image aktualisiert wird, wird die Web-App automatisch mit dem neuen Image aktualisiert.

### <a name="docker-hub"></a>Docker Hub

Klicken Sie auf der Docker Hub-Seite auf **Webhooks** und anschließend auf **CREATE A WEBHOOK** (WEBHOOK ERSTELLEN).

![Abbildung einfügen: Hinzufügen von Webhook 1](./media/app-service-webapp-service-linux-ci-cd/step3-1.png)

Geben Sie als Webhook-URL die URL aus **Schritt 3** an.

![Abbildung einfügen: Hinzufügen von Webhook 2](./media/app-service-webapp-service-linux-ci-cd/step3-2.png)

Wenn das Image aktualisiert wird, wird die Web-App automatisch mit dem neuen Image aktualisiert.

## <a name="next-steps"></a>Nächste Schritte

* [Was sind Azure-Web-Apps für Container?](./app-service-linux-intro.md)
* [Azure Container Registry](https://azure.microsoft.com/en-us/services/container-registry/)
* [Verwenden von .NET Core in Azure-Web-Apps für Container](quickstart-dotnetcore.md)
* [Verwenden von Ruby in Azure-Web-Apps für Container](quickstart-ruby.md)
* [Informationen zum Verwenden eines benutzerdefinierten Docker-Image für Azure-Web-Apps für Container](quickstart-custom-docker-image.md)
* [Azure App Service-Web-Apps für Container – FAQs](./app-service-linux-faq.md) 
* [Verwalten von Web-Apps für Container mithilfe der Azure CLI 2.0](./app-service-linux-cli.md)

