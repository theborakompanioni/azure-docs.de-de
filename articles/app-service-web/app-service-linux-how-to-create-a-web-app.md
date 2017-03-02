---
title: "Erstellen einer Azure-Web-App, die unter Linux ausgeführt wird | Microsoft-Dokumentation"
description: "Enthält eine Beschreibung des Workflows zur Erstellung einer Web-App für App Service unter Linux."
keywords: Azure App Service, Web-App, Linux, OSS
services: app-service
documentationcenter: 
author: naziml
manager: erikre
editor: 
ms.assetid: 3a71d10a-a0fe-4d28-af95-03b2860057d5
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/16/2017
ms.author: naziml;wesmc
translationtype: Human Translation
ms.sourcegitcommit: bb4c7ea7adfe1326ae8259782b5de2762c8c2bf5
ms.openlocfilehash: 19a42a8dc9a4026d93c10dd209e84d4e14b8f00a
ms.lasthandoff: 02/17/2017


---
# <a name="create-an-azure-web-app-running-on-linux"></a>Erstellen einer Azure-Web-App, die unter Linux ausgeführt wird
## <a name="use-the-azure-portal-to-create-your-web-app"></a>Verwenden des Azure-Portals zum Erstellen Ihrer Web-App
Sie können im [Azure-Portal](https://portal.azure.com) mit dem Erstellen Ihrer Web-App unter Linux beginnen. Dies ist in der folgenden Abbildung dargestellt:

![Beginnen mit der Erstellung einer Web-App im Azure-Portal][1]

Als Nächstes wird wie hier dargestellt das Blatt **Erstellen** geöffnet:

![Blatt „Erstellen“][2]

1. Geben Sie der Web-App einen Namen.
2. Wählen Sie eine vorhandene Ressourcengruppe aus, oder erstellen Sie eine neue Ressourcengruppe. (Die verfügbaren Regionen sind im [Abschnitt „Einschränkungen“](app-service-linux-intro.md) angegeben.)
3. Wählen Sie einen vorhandenen Azure App Service-Plan aus, oder erstellen Sie einen neuen. (Hinweise zu App Service-Plänen finden Sie im [Abschnitt „Einschränkungen“](app-service-linux-intro.md).)
4. Wählen Sie den gewünschten Anwendungsstapel aus. Sie können zwischen mehreren Versionen von Node.js, PHP, .NET Core und Ruby wählen.

Nachdem Sie die App erstellt haben, können Sie den Anwendungsstapel in den Anwendungseinstellungen wie hier gezeigt ändern:

![Anwendungseinstellungen][3]

## <a name="deploy-your-web-app"></a>Bereitstellen Ihrer Web-App
Wenn Sie **Bereitstellungsoptionen** im Verwaltungsportal wählen, können Sie ein lokales Git-Repository oder ein GitHub-Repository verwenden, um die Anwendung bereitzustellen. Die restlichen Anweisungen ähneln denen für eine nicht Linux-basierte Web-App. Zum Bereitstellen Ihrer App können Sie die Anweisungen für die [lokale Git-Bereitstellung](app-service-deploy-local-git.md) oder die [kontinuierliche Bereitstellung](app-service-continuous-deployment.md) befolgen.

Außerdem können Sie FTP verwenden, um die Anwendung auf Ihre Website hochzuladen. Sie können den FTP-Endpunkt für Ihre Web-App über den Abschnitt mit den Diagnoseprotokollen abrufen. Dies ist in der folgenden Abbildung dargestellt:

![Diagnoseprotokolle][4]

## <a name="next-steps"></a>Nächste Schritte
* [Was ist App Service unter Linux?](app-service-linux-intro.md)
* [Verwenden der PM2-Konfiguration für Node.js in Web-Apps unter Linux](app-service-linux-using-nodejs-pm2.md)
* [Verwendung von Ruby in Azure App Service-Web-Apps unter Linux](app-service-linux-using-ruby.md)
* [Häufig gestellte Fragen zu Azure App Service-Web-Apps unter Linux](app-service-linux-faq.md)

<!--Image references-->
[1]: ./media/app-service-linux-how-to-create-a-web-app/top-level-create.png
[2]: ./media/app-service-linux-how-to-create-a-web-app/create-blade.png
[3]: ./media/app-service-linux-how-to-create-a-web-app/application-settings-change-stack.png
[4]: ./media/app-service-linux-how-to-create-a-web-app/diagnostic-logs-ftp.png

