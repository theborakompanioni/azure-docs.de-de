---
title: "Hinzufügen einer Java-Anwendung in Azure App Service-Web-Apps"
description: "In diesem Lernprogramm erfahren Sie, wie Sie eine Seite oder Anwendung zu Ihrer Instanz von Azure-App Service Web-Apps hinzufügen, die bereits für die Verwendung von Java konfiguriert ist."
services: app-service\web
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 9b46528b-e2d0-4f26-b8d7-af94bd8c31ef
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 12/22/2016
ms.author: robmcm
translationtype: Human Translation
ms.sourcegitcommit: 627930ca68a94ecc56e7ef9ac9435f4b5f3f41c7
ms.openlocfilehash: 61466be17a52f1f230207b71bb94e10f88ee075c


---
# <a name="add-a-java-application-to-azure-app-service-web-apps"></a>Hinzufügen einer Java-Anwendung in Azure App Service-Web-Apps
Nachdem Sie Ihre Java Web-App in [Azure App Service][Azure App Service] wie unter [Erstellen einer Java-Web-App in Azure App Service](web-sites-java-get-started.md) beschrieben initialisiert haben, können Sie die Anwendung hochladen, indem Sie Ihre WAR-Datei im Ordner **webapps** speichern.

iiDer Navigationspfad zum Ordner **webapps** hängt davon ab, wie Sie Ihre Web-Apps-Instanz einrichten.

* Wenn Sie Ihre Web-App über den Azure Marketplace einrichten, hat der Pfad zum Ordner **webapps** das Format **D:\home\site\wwwroot\bin\application\_server\webapps**, wobei **application\_server** für den Namen des Anwendungsservers steht, der für Ihre Web-Apps-Instanz verwendet wird. 
* Wenn Sie Ihre Web-App über die Azure-Konfigurationsbenutzeroberfläche einrichten, hat der Pfad zum Ordner **webapps** das Format **D:\home\site\wwwroot\webapps**. 

Beachten Sie, dass Sie die Quellcodeverwaltung zum Hochladen Ihrer Anwendung oder Webseiten verwenden können, auch in [Szenarien mit fortlaufender Integration](app-service-continuous-deployment.md). FTP ist auch eine Option für das Hochladen Ihrer Anwendung oder Webseiten. Weitere Informationen zum Bereitstellen von Anwendungen über FTP finden Sie unter [Bereitstellen der App in Azure App Service].

Hinweis für Tomcat-Web-Apps: Nachdem Sie Ihre WAR-Datei in den Ordner **webapps** hochgeladen haben, erkennt der Tomcat-Anwendungsserver die neu hinzugefügte Datei und lädt diese automatisch. Beachten Sie, dass Sie beim Kopieren von Dateien in das ROOT-Verzeichnis den Anwendungsserver neu starten müssen, damit diese Dateien verwendet werden. Dies gilt nicht für WAR-Dateien. Die Funktion für automatisches Laden für in Azure ausgeführte Tomcat-Java-Web-Apps basiert auf einer neuen, hinzugefügten WAR-Datei oder neuen Dateien oder Verzeichnissen, die dem Ordner **webapps** hinzugefügt wurden. 

<a name="see-also"></a>

## <a name="see-also"></a>Weitere Informationen
Weitere Informationen zum Verwenden von Azure mit Java finden Sie im [Azure Java Developer Center].

[!INCLUDE [application-insights-app-insights-java-get-started](../application-insights/app-insights-java-get-started.md)]

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
[Bereitstellen der App in Azure App Service]: ./web-sites-deploy.md



<!--HONumber=Jan17_HO2-->


