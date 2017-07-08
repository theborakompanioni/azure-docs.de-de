---
title: Docker Hub Continuous Deployment mit Azure-Web-App unter Linux | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Continuous Deployment in Azure-Web-App unter Linux einrichten.
keywords: Azure App Service, Linux, OSS
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
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 68e11f07520bbe8da6812a34fe95708fc9e932ac
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
# <a name="docker-hub-continuous-deployment-with-azure-web-app-on-linux"></a>Docker Hub Continuous Deployment mit Azure-Web-App unter Linux

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

In diesem Tutorial konfigurieren Sie Continuous Deployment für ein benutzerdefiniertes Containerimage in [Docker Hub](https://hub.docker.com).

## <a name="step-1---log-in-to-azure"></a>Schritt 1: Anmelden bei Azure

Melden Sie sich unter „http://portal.azure.com“ beim Azure-Portal an.

## <a name="step-2---enable-docker-hub-continuous-deployment"></a>Schritt 2: Aktivieren von Docker Hub Continuous Deployment

Klicken Sie im **[Azure-Portal](https://portal.azure.com/)** auf der linken Seite auf **App Service**.

Klicken Sie auf den Namen der App, für die Sie Docker Hub Continuous Deployment konfigurieren möchten.

Fügen Sie in den **App-Einstellungen** eine App-Einstellung mit dem Namen `DOCKER_ENABLE_CI` und dem Wert `true` hinzu.

![Abbildung einfügen: App-Einstellung](./media/app-service-webapp-service-linux-ci-cd/step2.png)

## <a name="step-3---add-a-web-hook-to-docker-hub"></a>Schritt 3: Hinzufügen eines Webhooks zu Docker Hub

Klicken Sie auf der Docker Hub-Seite auf **Webhooks** und anschließend auf **CREATE A WEBHOOK** (WEBHOOK ERSTELLEN).

![Abbildung einfügen: Hinzufügen von Webhook 1](./media/app-service-webapp-service-linux-ci-cd/step3-1.png)

Für die Webhook-URL benötigen Sie den folgenden Endpunkt: `https://<publishingusername>:<publishingpwd>@<sitename>.scm.azurewebsites.net/docker/hook`.

![Abbildung einfügen: Hinzufügen von Webhook 2](./media/app-service-webapp-service-linux-ci-cd/step3-2.png)

Sie können `publishingusername` und `publishingpwd` durch Herunterladen des Web-App-Veröffentlichungsprofils im Azure-Portal abrufen.

![Abbildung einfügen: Hinzufügen von Webhook 2](./media/app-service-webapp-service-linux-ci-cd/step3-3.png)

Wenn das Image aktualisiert wird, wird die Web-App automatisch mit dem neuen Image aktualisiert.

## <a name="next-steps"></a>Nächste Schritte
* [Was ist Azure-Web-App unter Linux?](./app-service-linux-intro.md)
* [Erstellen von Apps in Azure-Web-App unter Linux](./app-service-linux-how-to-create-web-app.md)
* [Verwenden der PM2-Konfiguration für Node.js in Azure-Web-App unter Linux](app-service-linux-using-nodejs-pm2.md)
* [Verwenden von .NET Core in Azure-Web-App unter Linux](app-service-linux-using-dotnetcore.md)
* [Verwenden von Ruby in Azure-Web-App unter Linux](app-service-linux-ruby-get-started.md)
* [Verwenden eines benutzerdefinierten Docker-Images für Azure-Web-App unter Linux](./app-service-linux-using-custom-docker-image.md)
* [Häufig gestellte Fragen zu Azure App Service-Web-App unter Linux](./app-service-linux-faq.md) 




