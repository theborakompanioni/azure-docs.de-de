---
title: "Einführung in Azure-Web-App unter Linux | Microsoft-Dokumentation"
description: "Erfahren Sie mehr über Azure-Web-App unter Linux."
keywords: Azure App Service, Linux, OSS
services: app-service
documentationcenter: 
author: naziml
manager: erikre
editor: 
ms.assetid: bc85eff6-bbdf-410a-93dc-0f1222796676
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/16/2017
ms.author: naziml;wesmc
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 036e5691ecc435da54f381563b5d798f065bfb7f
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
# <a name="introduction-to-azure-web-app-on-linux"></a>Einführung in Azure-Web-App unter Linux

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

## <a name="overview"></a>Übersicht
Kunden können Web-App unter Linux verwenden, um Web-Apps für unterstützte Anwendungsstapel nativ unter Linux zu hosten. Im folgenden Abschnitt sind die Anwendungsstapel aufgeführt, die derzeit unterstützt werden. 

## <a name="features"></a>Features
Web-App unter Linux unterstützt derzeit die folgenden Anwendungsstapel:

* Node.js
    * 4.4
    * 4,5
    * 6.2
    * 6.6
    * 6.9
* PHP
    * 5.6
    * 7.0
* .NET Core
    * 1,0
    * 1.1
* Ruby
    * 2.3

Kunden haben folgende Möglichkeiten, um ihre Anwendungen bereitzustellen:

* FTP
* Lokales Git
* GitHub
* Bitbucket

Für die Anwendungsskalierung:

* Kunden können Web-Apps zentral hoch- und herunterskalieren, indem sie die Ebene in ihrem App Service-Plan ändern.
* Kunden können Anwendungen horizontal hochskalieren und mehrere App-Instanzen innerhalb der Grenzen ihrer SKU ausführen.

Für Kudu können einige grundlegende Funktionen verwendet werden:

* Umgebungen
* Bereitstellungen
* Grundlegende Konsolen
* SSH

Für DevOps:

* Stagingumgebungen
* CI/CD in DockerHub

## <a name="limitations"></a>Einschränkungen
Im Azure-Portal werden nur Features angezeigt, die für Web-App unter Linux derzeit funktionieren. Die restlichen Features werden ausgeblendet. Sobald wir weitere Features aktivieren, werden sie im Portal angezeigt.

Einige Features, z.B. die Integration virtueller Netzwerke, Azure Active Directory-/Drittanbieterauthentifizierung oder Kudu-Websiteerweiterungen, sind noch nicht verfügbar. Sobald diese Features verfügbar sind, aktualisieren wir die Dokumentation entsprechend und veröffentlichen Blog-Beiträge zu den Änderungen.

Diese öffentliche Vorschau ist derzeit nur in den folgenden Regionen verfügbar:

* USA (West)
* Westeuropa 
* Südostasien
* Australien (Osten)

Web-Apps unter Linux wird nur in den App Service-Plänen unterstützt und verfügt nicht über einen Free- oder Shared-Tarif. Außerdem schließen sich App Service-Pläne für reguläre Web-Apps und Linux-Web-Apps gegenseitig aus, sodass es nicht möglich ist, eine Linux-Web-App in einem anderen Plan zu erstellen, bei dem es sich nicht um einen Linux-App Service-Plan handelt.

Web-Apps unter Linux müssen in einer Ressourcengruppe erstellt werden, die in derselben Region keine anderen Web-Apps als Linux-Web-Apps enthält.

## <a name="next-steps"></a>Nächste Schritte
Unter den folgenden Links erhalten Sie Informationen zu den ersten Schritten mit App Service unter Linux. In [unserem Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview) können Sie Fragen stellen und Antworten auf Probleme erhalten.

* [Erstellen von Web-Apps in Azure-Web-App unter Linux](app-service-linux-how-to-create-web-app.md)
* [Verwenden eines benutzerdefinierten Docker-Images für Azure-Web-App unter Linux](app-service-linux-using-custom-docker-image.md)
* [Verwenden der PM2-Konfiguration für Node.js in Azure-Web-App unter Linux](app-service-linux-using-nodejs-pm2.md)
* [Verwenden von .NET Core in Azure App Service-Web-App unter Linux](app-service-linux-using-dotnetcore.md)
* [Verwenden von Ruby in Azure App Service-Web-App unter Linux](app-service-linux-ruby-get-started.md)
* [Häufig gestellte Fragen zu Azure App Service-Web-App unter Linux](app-service-linux-faq.md)
* [SSH-Unterstützung für Azure-Web-App unter Linux](./app-service-linux-ssh-support.md)
* [Einrichten von Stagingumgebungen in Azure App Service](./web-sites-staged-publishing.md)
* [Docker Hub Continuous Deployment mit Azure-Web-App unter Linux](./app-service-linux-ci-cd.md)


