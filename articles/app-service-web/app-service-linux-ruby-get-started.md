---
title: Erstellen einer Ruby-App in Azure App Service-Web-Apps unter Linux | Microsoft Docs
description: Erfahren Sie, wie Sie Ruby-Apps mit Azure App Service-Web-Apps unter Linux erstellen.
keywords: Azure App Service, Linux, OSS
services: app-service
documentationcenter: 
author: wesmc7777
manager: erikre
editor: 
ms.assetid: 6d00c73c-13cb-446f-8926-923db4101afa
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/05/2017
ms.author: wesmc
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: c63bf790a50b894ea1732ae3142e6c1ff9ccbd4c
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
# <a name="create-a-ruby-app-with-azure-web-app-on-linux---preview"></a>Erstellen einer Ruby-App mit Azure-Web-Apps unter Linux – Vorschauversion

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

## <a name="overview"></a>Übersicht

In diesem Tutorial erfahren Sie, wie Sie eine grundlegende Ruby on Rails-Anwendung lokal erstellen und unter Linux auf Azure-Web-App bereitstellen.

## <a name="prerequisites"></a>Voraussetzungen

* [Ruby 2.3.3](https://www.ruby-lang.org/en/documentation/installation/#rubyinstaller) auf dem Entwicklungscomputer
* [Git](https://git-scm.com/downloads) auf dem Entwicklungscomputer
* Ein [aktives Azure-Abonnement](https://azure.microsoft.com/pricing/free-trial/)
* Dieses Tutorial wurde im Kontext einer Ubuntu-Umgebung geschrieben. Alle Systembefehle sind Bash-spezifisch.


## <a name="create-a-new-local-rails-application"></a>Erstellen einer neuen lokalen Rails-Anwendung

1. Erstellen Sie ein Verzeichnis für die neue App, und wechseln Sie in dieses Verzeichnis.

        mkdir ~/workspace
        cd ~/workspace

2. Initialisieren Sie Ruby, und überprüfen Sie die Version mit dem Befehl `ruby -v`.

    ![Ruby init](./media/app-service-linux-ruby-get-started/ruby-version.png)

3. Installieren Sie Rails mit dem Befehl `gem install rails`.

    ![Installieren von Rails](./media/app-service-linux-ruby-get-started/install-rails.png)

4. Erstellen Sie eine Rails-Anwendung mit dem Namen **hello-world** mit dem folgenden Befehl:

    Wenn Sie Rails 5.1.0 oder höher verwenden, binden Sie den Befehl `--skip-yarn` wie im folgenden Befehl gezeigt ein:

        rails new hello-world --skip-yarn

    Verwenden Sie für ältere Rails-Versionen als 5.1.0 den folgenden Befehl:

        rails new hello-world 

    ![New Hello-world](./media/app-service-linux-ruby-get-started/rails-new-hello-world.png)

    ![New Hello-world](./media/app-service-linux-ruby-get-started/rails-new-hello-world-2.png)

    Wenn Sie Rails 5.1 + verwenden, wird eine Datei vom Typ „package.json“ erstellt, wenn die `--skip-yarn` Option nicht verwendet wird. Wir möchten nicht, dass sie in unsere Bereitstellung eingebunden wird. Als Alternative können Sie die Datei „package.json“ löschen oder sie der Datei *.git-ignore* wie folgt im Verzeichnis hinzufügen: 

        # Ignore package.json
        /package.json

5. Wechseln Sie zum Verzeichnis *hello-world*, und starten Sie den Server.

        cd hello-world
        rails server

    ![Start Hello-world](./media/app-service-linux-ruby-get-started/start-hello-world-server.png)
    
6. Navigieren Sie in Ihrem Webbrowser zu `http://localhost:3000`, um die App lokal zu testen.    

    ![Hello-world](./media/app-service-linux-ruby-get-started/hello-world.png)

## <a name="prepare-the-app-for-azure"></a>Vorbereiten der App für Azure

Das Ruby-Image führt standardmäßig den Server mit dem Flag `-e production` aus. Diese Umgebung erfordert eine Einrichtung der Azure-Web-App unter Linux. Der Container übernimmt einen Teil dieser Einrichtung (z.B. die Einstellung einer `SECRET_KEY_BASE`). Eine Standardroute muss konfiguriert werden. Andernfalls erhalten Sie einen 404-Fehler beim Durchsuchen der Website.

So konfigurieren Sie eine Standardroute:

1. Öffnen Sie *~/workspace/hello-world/config/routes.rb* zur Bearbeitung. Fügen Sie wie im Screenshot dargestellt die folgende Zeile hinzu. 

        root 'application#hello'

    ![routes.rb](./media/app-service-linux-ruby-get-started/routes-rb.png)


2. Öffnen Sie *~/workspace/hello-world/app/controllers/application_controller.rb* zur Bearbeitung. Fügen Sie wie im Screenshot dargestellt die folgenden Zeilen hinzu.

        def hello
            render html: "Hello, world from Azure Web App on Linux!"
        end

    ![routes.rb](./media/app-service-linux-ruby-get-started/application-controller-rb.png)


3. Ihre App ist jetzt konfiguriert. Navigieren Sie in Ihrem Webbrowser zu `http://localhost:3000`, um die Stamm-Startseite zu bestätigen.

    ![Hallo Welt (konfiguriert)](./media/app-service-linux-ruby-get-started/hello-world-configured.png)

## <a name="create-a-ruby-website-on-azure"></a>Erstellen einer Ruby-Website in Azure

1. Navigieren Sie zum [Azure-Portal](http://portal.azure.com), und melden Sie sich mit Ihren Abonnementdaten an. Fügen Sie wie im folgenden Screenshot dargestellt eine **Web-App unter Linux** hinzu:

    ![Erstellen einer Web-App unter Linux](./media/app-service-linux-ruby-get-started/top-level-create.png)

2. Das Blatt **Erstellen** wird wie im folgenden Screenshot geöffnet:

    ![Blatt „Erstellen“](./media/app-service-linux-ruby-get-started/create-blade.png)


    1. Geben Sie der Web-App einen Namen.
    2. Wählen Sie eine vorhandene Ressourcengruppe aus, oder erstellen Sie eine neue Ressourcengruppe. (Die verfügbaren Regionen sind im [Abschnitt „Einschränkungen“](app-service-linux-intro.md) angegeben.)
    3. Wählen Sie einen vorhandenen Azure App Service-Plan aus, oder erstellen Sie einen neuen. (Hinweise zu App Service-Plänen finden Sie im [Abschnitt „Einschränkungen“](app-service-linux-intro.md).)
    4. Wählen Sie den in **Ruby 2.3** integrierten Laufzeitstapel für die Konfiguration Ihres Containers aus.
    5. Klicken Sie für die Web-App auf **An Dashboard anheften**.
    6. Klicken Sie auf **Erstellen**.

3. Nach dem Erstellen der Web-App wird das Blatt **Übersicht** angezeigt. Kopieren Sie die **URL** für die neue Web-App, und öffnen Sie sie in Ihrem Browser. Die folgende Begrüßungsseite wird angezeigt.

    ![Begrüßungsseite](./media/app-service-linux-ruby-get-started/splash-page.png)


## <a name="deploy-your-application"></a>Bereitstellen der Anwendung

In diesem Tutorial verwenden wir Git, um die lokale Ruby-Anwendung in Azure bereitzustellen.

1. Auf der neuen Azure-Website wurde bereits eine Git-Bereitstellung konfiguriert. Sie finden die URL der Git-Bereitstellung nach dem Einfügen des Namens Ihrer Web-App unter folgender URL:

        https://{your web app name}.scm.azurewebsites.net/api/scm/info

    Die Git-URL weist basierend auf dem Namen Ihrer Web-App das folgende Format auf:

        https://{your web app name}.scm.azurewebsites.net/{your web app name}.git

2. Führen Sie die folgenden Befehle aus, um die lokale Anwendung auf Ihrer Azure-Website bereitzustellen.

        cd ~/workspace/hello-world
        git init
        git remote add azure <Git deployment URL from above>
        git add -A
        git commit -m "Initial deployment commit"
        git push master

    Vergewissern Sie sich, dass die Remotebereitstellungsvorgänge erfolgreich waren.

    ![Bereitstellen der Web-App](./media/app-service-linux-ruby-get-started/deployment-success.png)

    Wenn Sie eine Fehlermeldung sehen, dass die Remoteverbindung nicht reagiert, wird die Bereitstellung wahrscheinlich noch ausgeführt. Navigieren Sie in diesem Fall zu folgender URL in Ihrem Browser:

        https://{your web app name}.scm.azurewebsites.net/api/deployments

3. Starten Sie Ihre Web-App nach Abschluss der Bereitstellung neu, damit die Bereitstellung wirksam wird. Navigieren Sie im [Azure-Portal](http://portal.azure.com) zum Blatt **Übersicht** Ihrer Web-App.

    Klicken Sie auf der Symbolleiste auf **Neu starten**.

    ![Neustarten einer Web-App](./media/app-service-linux-ruby-get-started/restart-web-app.png)

4. Navigieren Sie zu Ihrer Website, und prüfen Sie, ob Ihre Updates aktiv sind. 

    Wenn Sie während des Neustarts der App versuchen, die Website zu durchsuchen, wird der HTTP-Statuscodefehler 503 (Server nicht verfügbar) ausgegeben. Der vollständige Neustart kann einige Minuten in Anspruch nehmen.

        http://{your web app name}.azurewebsites.net

    ![Aktualisierte Web-App](./media/app-service-linux-ruby-get-started/hello-world-updated.png)
    

## <a name="next-steps"></a>Nächste Schritte
Unter den folgenden Links finden Sie weitere Informationen zu Azure App Service-Web-Apps unter Linux. In [unserem Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview) können Sie Fragen stellen und Antworten auf Probleme erhalten.

* [Erstellen von Web-Apps in App Service unter Linux](app-service-linux-how-to-create-web-app.md)
* [Verwenden eines benutzerdefinierten Docker-Image für App Service unter Linux](app-service-linux-using-custom-docker-image.md)
* [Verwenden der PM2-Konfiguration für Node.js in Web-Apps unter Linux](app-service-linux-using-nodejs-pm2.md)
* [Verwendung von .NET Core in Azure App Service-Web-Apps unter Linux](app-service-linux-using-dotnetcore.md)
* [Häufig gestellte Fragen zu Azure App Service-Web-Apps unter Linux](app-service-linux-faq.md)


