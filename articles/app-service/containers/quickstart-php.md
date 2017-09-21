---
title: Erstellen einer PHP-Web-App in einem Linux-Container in Azure | Microsoft-Dokumentation
description: "Stellen Sie in wenigen Minuten Ihre erste „Hallo Welt“-PHP-App in einer Azure App Service-Web-App bereit."
services: app-service\web
documentationcenter: 
author: syntaxc4
manager: erikre
editor: 
ms.assetid: 6feac128-c728-4491-8b79-962da9a40788
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 08/30/2017
ms.author: cfowler
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: 12c20264b14a477643a4bbc1469a8d1c0941c6e6
ms.openlocfilehash: 1b10636848198ab0ff46e6a492373ecf42a96202
ms.contentlocale: de-de
ms.lasthandoff: 09/07/2017

---
# <a name="create-a-php-web-app-in-a-linux-container-in-azure"></a>Erstellen einer PHP-Web-App in einem Linux-Container in Azure

[Web-Apps für Container](app-service-linux-intro.md) bieten einen hochgradig skalierbaren Webhostingdienst mit Self-Patching unter dem Linux-Betriebssystem. In diesem Schnellstarttutorial wird erläutert, wie Sie eine PHP-App in Azure-Web-Apps für Container bereitstellen. Sie erstellen die Web-App mithilfe der [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) in Cloud Shell und stellen mit Git den PHP-Code für die Web-App bereit.

![In Azure ausgeführte Beispiel-App]](media/quickstart-php/hello-world-in-browser.png)

Die folgenden Schritte können unter Mac, Windows oder Linux ausgeführt werden. 

## <a name="prerequisites"></a>Voraussetzungen

So führen Sie diesen Schnellstart durch:

* [Installieren Sie Git.](https://git-scm.com/)
* [Installieren Sie PHP.](https://php.net)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Herunterladen des Beispiels

Führen Sie in einem Terminalfenster die folgenden Befehle aus, um die Beispielanwendung auf Ihren lokalen Computer zu klonen, und navigieren Sie zum Verzeichnis mit dem Beispielcode.

```bash
git clone https://github.com/Azure-Samples/php-docs-hello-world
cd php-docs-hello-world
```

## <a name="run-the-app-locally"></a>Lokales Ausführen der App

Öffnen Sie zum lokalen Ausführen der Anwendung ein Terminalfenster, und verwenden Sie den Befehl `php`, um den integrierten PHP-Webserver zu starten.

```bash
php -S localhost:8080
```

Öffnen Sie einen Webbrowser, und navigieren Sie zur Beispiel-App unter http://localhost:8080.

Die Nachricht **Hello World!** aus der Beispiel-App wird auf der Seite angezeigt.

![Lokal ausgeführte Beispiel-App](media/quickstart-php/localhost-hello-world-in-browser.png)

Drücken Sie in Ihrem Terminalfenster **STRG+C**, um den Webserver zu beenden.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Configure deployment user](../../../includes/configure-deployment-user.md)]

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group.md)]

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux.md)]

## <a name="create-a-web-app"></a>Erstellen einer Web-App

[!INCLUDE [Create web app](../../../includes/app-service-web-create-web-app-linux-php-no-h.md)] 

Navigieren Sie zu der Website, um sich Ihre neu erstellte Web-App anzusehen. Ersetzen Sie _&lt;App-Name>_ durch einen eindeutigen App-Namen.

```bash
http://<app name>.azurewebsites.net
```

![Leere Web-App-Seite](media/quickstart-php/app-service-web-service-created.png)

Sie haben eine leere neue Web-App in einem Linux-Container mit aktivierter Git-Bereitstellung erstellt.

[!INCLUDE [Push to Azure](../../../includes/app-service-web-git-push-to-azure.md)] 

```bash
Counting objects: 2, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (2/2), 352 bytes | 0 bytes/s, done.
Total 2 (delta 1), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id '25f18051e9'.
remote: Generating deployment script.
remote: Running deployment command...
remote: Handling Basic Web Site deployment.
remote: Kudu sync from: '/home/site/repository' to: '/home/site/wwwroot'
remote: Copying file: '.gitignore'
remote: Copying file: 'LICENSE'
remote: Copying file: 'README.md'
remote: Copying file: 'index.php'
remote: Ignoring: .git
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
To https://<app_name>.scm.azurewebsites.net/<app_name>.git
   cc39b1e..25f1805  master -> master
```

## <a name="browse-to-the-app"></a>Navigieren zur App

Navigieren Sie in Ihrem Webbrowser zu der bereitgestellten Anwendung.

```bash
http://<app_name>.azurewebsites.net
```

Der PHP-Beispielcode wird in einer Azure App Service-Web-App ausgeführt.

![In Azure ausgeführte Beispiel-App](media/quickstart-php/hello-world-in-browser.png)

**Glückwunsch!** Sie haben Ihre erste PHP-App für App Service bereitgestellt.

## <a name="update-locally-and-redeploy-the-code"></a>Lokales Aktualisieren und erneutes Bereitstellen des Codes

Öffnen Sie die Datei `index.php` innerhalb der PHP-App mit einem lokalen Text-Editor, und ändern Sie den Text in der Zeichenfolge neben `echo` geringfügig:

```php
echo "Hello Azure!";
```

Führen Sie für Ihre Änderungen in Git einen Commit aus, und übertragen Sie dann die Codeänderungen mithilfe von Push an Azure.

```bash
git commit -am "updated output"
git push azure master
```

Wechseln Sie nach Abschluss der Bereitstellung wieder zu dem Browserfenster, das im Schritt **Navigieren zur App** geöffnet wurde, und aktualisieren Sie die Seite.

![In Azure ausgeführte aktualisierte Beispiel-App](media/quickstart-php/hello-azure-in-browser.png)

## <a name="manage-your-new-azure-web-app"></a>Verwalten Ihrer neuen Azure-Web-App

Wechseln Sie zum <a href="https://portal.azure.com" target="_blank">Azure-Portal</a>, um die erstellte Web-App zu verwalten.

Klicken Sie im linken Menü auf **App Services** und anschließend auf den Namen Ihrer Azure-Web-App.

![Portalnavigation zur Azure-Web-App](./media/quickstart-php/php-docs-hello-world-app-service-list.png)

Die Übersichtsseite Ihrer Web-App wird angezeigt. Hier können Sie einfache Verwaltungsaufgaben wie Durchsuchen, Beenden, Neustarten und Löschen durchführen.

![App Service-Seite im Azure-Portal](media/quickstart-php/php-docs-hello-world-app-service-detail.png)

Im linken Menü werden verschiedene Seiten für die Konfiguration Ihrer App angezeigt. 

[!INCLUDE [cli-samples-clean-up](../../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [PHP mit MySQL](tutorial-php-mysql-app.md)

