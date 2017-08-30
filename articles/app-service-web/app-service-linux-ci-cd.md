---
title: Continuous Deployment mit Azure-Web-App unter Linux | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Continuous Deployment in Azure-Web-App unter Linux einrichten.
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
ms.sourcegitcommit: cf381b43b174a104e5709ff7ce27d248a0dfdbea
ms.openlocfilehash: 026c4491818c8719c68a759ee9595ad9c765d526
ms.contentlocale: de-de
ms.lasthandoff: 08/23/2017

---
# <a name="continuous-deployment-with-azure-web-app-on-linux"></a>Continuous Deployment mit Azure-Web-App unter Linux

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

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
az webapp deployment container -n sname1 -g rgname -e true --show-cd-url
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
* [Was ist Azure-Web-App unter Linux?](./app-service-linux-intro.md)
* [Erstellen von Apps in Azure-Web-Apps unter Linux](./app-service-linux-how-to-create-web-app.md)
* [Azure Container Registry](https://azure.microsoft.com/en-us/services/container-registry/)
* [Verwenden der PM2-Konfiguration für Node.js in Azure-Web-App unter Linux](app-service-linux-using-nodejs-pm2.md)
* [Verwenden von .NET Core in Azure-Web-Apps unter Linux](app-service-linux-using-dotnetcore.md)
* [Verwenden von Ruby in Azure-Web-App unter Linux](app-service-linux-ruby-get-started.md)
* [Verwenden eines benutzerdefinierten Docker-Images für Azure-Web-App unter Linux](./app-service-linux-using-custom-docker-image.md)
* [Häufig gestellte Fragen zu Azure App Service-Web-App unter Linux](./app-service-linux-faq.md) 
* [Verwalten von Web-Apps unter Linux mit Azure CLI 2.0](./app-service-linux-cli.md)




