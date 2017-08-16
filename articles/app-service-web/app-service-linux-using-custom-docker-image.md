---
title: "Verwenden eines benutzerdefinierten Docker-Images für Azure-Web-Apps unter Linux | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie ein benutzerdefiniertes Docker-Image für Azure-Web-Apps unter Linux verwenden."
keywords: Azure App Service, Web-App, Linux, Docker, Container
services: app-service
documentationcenter: 
author: naziml
manager: erikre
editor: 
ms.assetid: b97bd4e6-dff0-4976-ac20-d5c109a559a8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/16/2017
ms.author: naziml;wesmc
ms.translationtype: HT
ms.sourcegitcommit: 1dbb1d5aae55a4c926b9d8632b416a740a375684
ms.openlocfilehash: f51cacb33251d479f48a39014cc2db60a23358d5
ms.contentlocale: de-de
ms.lasthandoff: 08/07/2017

---

# <a name="using-a-custom-docker-image-for-azure-web-app-on-linux"></a>Verwenden eines benutzerdefinierten Docker-Images für Azure-Web-Apps unter Linux #

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]


App Service stellt vordefinierte Anwendungsstapel unter Linux mit Unterstützung für bestimmte Versionen bereit, z.B. PHP 7.0 und Node.js 4.5. In App Service unter Linux werden Docker-Container verwendet, um diese vorab erstellten Anwendungsstapel zu hosten. Sie können auch ein benutzerdefiniertes Docker-Image verwenden, um Ihre Web-App in einem Anwendungsstapel bereitzustellen, der nicht bereits in Azure definiert ist. Benutzerdefinierte Docker-Images können in einem öffentlichen oder privaten Docker-Repository gehostet werden.


## <a name="how-to-set-a-custom-docker-image-for-a-web-app"></a>Festlegen eines benutzerdefinierten Docker-Images für eine Web-App
Sie können das benutzerdefinierte Docker-Image sowohl für neue als auch für vorhandene Web-Apps festlegen. Klicken Sie beim Erstellen einer Web-App unter Linux im [Azure-Portal](https://portal.azure.com/#create/Microsoft.AppSvcLinux) auf **Container konfigurieren**, um ein benutzerdefiniertes Docker-Image festzulegen:

![Benutzerdefiniertes Docker-Image für eine neue Web-App unter Linux][1]


## <a name="how-to-use-a-custom-docker-image-from-docker-hub"></a>Verwenden eines benutzerdefinierten Docker-Image über Docker Hub ##
So verwenden Sie ein benutzerdefiniertes Docker-Image über Docker Hub

1. Suchen Sie im [Azure-Portal](https://portal.azure.com) nach Ihrer Web-App unter Linux, und klicken Sie dann unter **Einstellungen** auf **Docker-Container**.

2.  Wählen Sie **Docker Hub** als **Imagequelle** aus, und klicken Sie dann entweder auf **Öffentlich** oder **Privat**, und geben Sie das **Image und optional einen Tagnamen** ein, z.B. `node:4.5`. Der **Startbefehl** wird automatisch basierend darauf festgelegt, was in der Docker-Imagedatei definiert ist, aber Sie können auch eigene Befehle festlegen.  

    ![Konfigurieren des öffentlichen Docker Hub-Repositoryimage][2]

    Wenn das Image aus einem privaten Repository stammt, müssen Sie auch die Docker Hub-Anmeldeinformationen (**Benutzername für Anmeldung** und **Kennwort**) wie für das private Docker Hub-Repository eingeben.

    ![Konfigurieren des privaten Docker Hub-Repositoryimage][3]

3. Klicken Sie nach dem Konfigurieren des Containers auf **Speichern**.

## <a name="how-to-use-a-docker-image-from-a-private-image-registry"></a>Verwenden eines Docker-Image aus einer privaten Imageregistrierung ##
So verwenden Sie ein benutzerdefiniertes Docker-Image aus einer privaten Imageregistrierung

1. Suchen Sie im [Azure-Portal](https://portal.azure.com) nach Ihrer Web-App unter Linux, und klicken Sie dann unter **Einstellungen** auf **Docker-Container**.

2.  Klicken Sie unter **Bildquelle** auf **Private Registrierung**. Geben Sie Werte unter **Bild und optional ein Tag** und **Server-URL** für die private Registrierung sowie die Anmeldeinformationen (**Benutzername für Anmeldung** und **Kennwort**) ein. Klicken Sie auf **Speichern**.

    ![Konfigurieren des Docker-Image aus der privaten Registrierung][4]


## <a name="how-to-set-the-port-used-by-your-docker-image"></a>Festlegen des vom Docker-Image verwendeten Ports ##

Wenn Sie ein benutzerdefiniertes Docker-Image für Ihre Web-App nutzen, können Sie die Umgebungsvariable `WEBSITES_PORT` in Ihrer Dockerfile-Datei verwenden. Sie wird dem generierten Container hinzugefügt. Sehen Sie sich das folgende Beispiel mit einer Docker-Datei für eine Ruby-Anwendung an:

    FROM ruby:2.2.0
    RUN mkdir /app
    WORKDIR /app
    ADD . /app
    RUN bundle install
    CMD bundle exec puma config.ru -p WEBSITES_PORT -e production

In der letzten Zeile des Befehls können Sie erkennen, dass die Umgebungsvariable „WEBSITES_PORT“ zur Laufzeit übergeben wird. Beachten Sie, dass in Befehlen die Groß-/Kleinschreibung berücksichtigt wird.

Bei der Plattform wurde in der Vergangenheit die App-Einstellung `PORT` verwendet. Es ist geplant, die Verwendung dieser App-Einstellung als veraltet zu markieren und ausschließlich `WEBSITES_PORT` zu verwenden.

Wenn Sie ein vorhandenes Docker-Image nutzen, das von einer anderen Person erstellt wurde, müssen Sie unter Umständen einen anderen Port als Port 80 für die Anwendung angeben. Um den Port zu konfigurieren, fügen Sie eine Anwendungseinstellung mit dem Namen `WEBSITES_PORT` und dem unten abgebildeten Wert hinzu:

![Konfigurieren der App-Einstellung PORT für das benutzerdefinierte Docker-Image][6]


## <a name="how-to-switch-back-to-using-a-built-in-image"></a>Zurückwechseln zur Verwendung eines integrierten Image ##

So wechseln Sie von einem benutzerdefinierten Image zurück zu einem integrierten Image

1. Suchen Sie im [Azure-Portal](https://portal.azure.com) nach Ihrer Web-App unter Linux, und klicken Sie dann unter **Einstellungen** auf **App Service**.

2. Wählen Sie den **Laufzeitstapel** aus, den Sie für das integrierte Image erstellen möchten, und klicken Sie dann auf **Speichern**. 

![Konfigurieren des integrierten Docker-Image][5]


## <a name="troubleshooting"></a>Problembehandlung ##

Wenn Ihre Anwendung nicht mit Ihrem benutzerdefinierten Docker-Image gestartet werden kann, überprüfen Sie die Docker-Protokolle im Verzeichnis „LogFiles“. Sie können auf dieses Verzeichnis entweder über Ihre SCM-Website oder per FTP zugreifen.
Zum Protokollieren von `stdout` und `stderr` in Ihrem Container müssen Sie unter **Diagnoseprotokolle** die **Protokollierung von Docker-Containern** aktivieren.

![Aktivieren der Protokollierung][8]

![Verwenden von Kudu zum Anzeigen von Docker-Protokollen][7]

Sie können auf die SCM-Website über die Option **Erweiterte Tools** im Menü **Entwicklungstools** zugreifen.

## <a name="next-steps"></a>Nächste Schritte ##

Unter den folgenden Links erhalten Sie Informationen zu den ersten Schritten mit Web-Apps unter Linux.   

* [Einführung in Azure-Web-Apps unter Linux](./app-service-linux-intro.md)
* [Erstellen von Web-Apps in Azure-Web-Apps unter Linux](./app-service-linux-how-to-create-web-app.md)
* [Verwenden der PM2-Konfiguration für Node.js in Azure-Web-Apps unter Linux](./app-service-linux-using-nodejs-pm2.md)
* [Häufig gestellte Fragen zu Azure App Service-Web-Apps unter Linux](app-service-linux-faq.md)

In [unserem Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview) können Sie Fragen stellen und Antworten auf Probleme erhalten.


<!--Image references-->
[1]: ./media/app-service-linux-using-custom-docker-image/new-configure-container.png
[2]: ./media/app-service-linux-using-custom-docker-image/existingapp-configure-dockerhub-public.png
[3]: ./media/app-service-linux-using-custom-docker-image/existingapp-configure-dockerhub-private.png
[4]: ./media/app-service-linux-using-custom-docker-image/existingapp-configure-privateregistry.png
[5]: ./media/app-service-linux-using-custom-docker-image/existingapp-configure-builtin.png
[6]: ./media/app-service-linux-using-custom-docker-image/setting-port.png
[7]: ./media/app-service-linux-using-custom-docker-image/kudu-docker-logs.png
[8]: ./media/app-service-linux-using-custom-docker-image/logging.png

