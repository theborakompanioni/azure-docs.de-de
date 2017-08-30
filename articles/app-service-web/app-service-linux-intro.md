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
ms.translationtype: HT
ms.sourcegitcommit: 25e4506cc2331ee016b8b365c2e1677424cf4992
ms.openlocfilehash: 67dee77dd4e46d097358d86626a859b7dc7982e7
ms.contentlocale: de-de
ms.lasthandoff: 08/24/2017

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
    * 6.10
    * 6.11
    * 8.0
    * 8.1
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

* Kunden können Web-Apps zentral hoch- und herunterskalieren, indem sie den Tarif ihres App Service-Plans ändern.
* Kunden können Anwendungen horizontal hochskalieren und mehrere App-Instanzen innerhalb der Grenzen ihrer SKU ausführen.

Für Kudu können einige grundlegende Funktionen verwendet werden:

* Umgebungen
* Bereitstellungen
* Grundlegende Konsole
* SSH

Für DevOps:

* Stagingumgebungen
* ACR und CI/CD in DockerHub

## <a name="limitations"></a>Einschränkungen
Im Azure-Portal werden nur Features angezeigt, die für Web-App unter Linux derzeit funktionieren. Die restlichen Features werden ausgeblendet. Sobald wir weitere Features aktivieren, werden sie im Portal angezeigt.

Einige Features, z.B. die Integration virtueller Netzwerke, Azure Active Directory-/Drittanbieterauthentifizierung oder Kudu-Websiteerweiterungen, sind noch nicht verfügbar. Sobald diese Features verfügbar sind, aktualisieren wir die Dokumentation entsprechend und veröffentlichen Blog-Beiträge zu den Änderungen.

Diese öffentliche Vorschau ist derzeit nur in den folgenden Regionen verfügbar:

* USA (West)
* USA (Ost)
* Westeuropa
* Europa, Norden
* USA (Mitte/Süden)
* USA Nord Mitte
* Asien, Südosten
* Asien, Osten
* Australien (Osten)
* Japan Ost
* Brasilien Süd
* Indien (Süden)

Web-Apps unter Linux wird nur in den App Service-Plänen unterstützt und verfügt nicht über einen Free- oder Shared-Tarif. Außerdem schließen sich App Service-Pläne für reguläre Web-Apps und Linux-Web-Apps gegenseitig aus, sodass es nicht möglich ist, eine Linux-Web-App in einem anderen Plan zu erstellen, bei dem es sich nicht um einen Linux-App Service-Plan handelt.

Web-Apps unter Linux müssen in einer Ressourcengruppe erstellt werden, die in derselben Region keine anderen Web-Apps als Linux-Web-Apps enthält.

## <a name="troubleshooting"></a>Problembehandlung ##

Wenn Ihre Anwendung nicht gestartet werden kann oder Sie die Protokolle Ihrer App überprüfen möchten, sehen Sie sich die Docker-Protokolle im Verzeichnis „LogFiles“ an. Sie können auf dieses Verzeichnis entweder über Ihre SCM-Website oder per FTP zugreifen.
Zum Protokollieren von `stdout` und `stderr` in Ihrem Container müssen Sie unter **Diagnoseprotokolle** die **Protokollierung von Docker-Containern** aktivieren.

![Aktivieren der Protokollierung][2]

![Verwenden von Kudu zum Anzeigen von Docker-Protokollen][1]

Sie können auf die SCM-Website über die Option **Erweiterte Tools** im Menü **Entwicklungstools** zugreifen.

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

<!--Image references-->
[1]: ./media/app-service-linux-intro/kudu-docker-logs.png
[2]: ./media/app-service-linux-intro/logging.png
