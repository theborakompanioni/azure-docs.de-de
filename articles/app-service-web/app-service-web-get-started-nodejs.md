---
title: Erstellen einer Node.js-Anwendung in einer Web-App | Microsoft-Dokumentation
description: "Stellen Sie in wenigen Minuten Ihre erste Node.js-App vom Typ „Hello World“ in einer App Service-Web-App bereit."
services: app-service\web
documentationcenter: 
author: syntaxc4
manager: erikre
editor: 
ms.assetid: 582bb3c2-164b-42f5-b081-95bfcb7a502a
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 03/28/2017
ms.author: cfowler
translationtype: Human Translation
ms.sourcegitcommit: b0c27ca561567ff002bbb864846b7a3ea95d7fa3
ms.openlocfilehash: c32cb52e4bb7bacde20e21820f277b4e86877e74
ms.lasthandoff: 04/25/2017

---
# <a name="create-a-nodejs-application-on-web-app"></a>Erstellen einer Node.js-Anwendung in einer Web-App

In diesem Schnellstarttutorial erfahren Sie Schritt für Schritt, wie Sie eine Node.js-App entwickeln und für Azure bereitstellen. Wir führen die App mithilfe einer Linux-basierten Azure App Service-Instanz aus und konfigurieren darin eine neue Web-App über die Azure-Befehlszeilenschnittstelle. Anschließend stellen wir unsere Node.js-App mithilfe von Git für Azure bereit.

![hello-world-in-browser](media/app-service-web-get-started-nodejs-poc/hello-world-in-browser.png)

Die folgenden Schritte können auf einem Mac oder auf einem Computer unter Windows oder Linux ausgeführt werden. Der gesamte Prozess dauert etwa fünf Minuten.

## <a name="before-you-begin"></a>Voraussetzungen

Installieren Sie vor der Ausführung dieses Beispiels zunächst die folgenden erforderlichen Komponenten:

1. [Laden Sie Git herunter, und installieren Sie es.](https://git-scm.com/)
1. [Laden Sie Node.js und NPM herunter, und installieren Sie sie.](https://nodejs.org/)
1. Laden Sie die [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) herunter, und installieren Sie sie.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Herunterladen des Beispiels

Klonen Sie das Beispiel-App-Repository für „Hello World“ auf Ihren lokalen Computer.

```bash
git clone https://github.com/Azure-Samples/nodejs-docs-hello-world
```

> [!TIP]
> Alternativ können Sie [das Beispiel als ZIP-Datei herunterladen](https://github.com/Azure-Samples/nodejs-docs-hello-world/archive/master.zip) und extrahieren.

Navigieren Sie zum Verzeichnis mit dem Beispielcode.

```bash
cd nodejs-docs-hello-world
```

## <a name="run-the-app-locally"></a>Lokales Ausführen der App

Öffnen Sie zum lokalen Ausführen der Anwendung ein Terminalfenster, und verwenden Sie das Skript `npm start` für das Beispiel, um den integrierten Node.js-HTTP-Server zu starten.

```bash
npm start
```

Öffnen Sie einen Webbrowser, und navigieren Sie zu dem Beispiel.

```bash
http://localhost:1337
```

Auf der Seite wird die Nachricht **Hello World** aus der Beispiel-App angezeigt.

![localhost-hello-world-in-browser](media/app-service-web-get-started-nodejs-poc/localhost-hello-world-in-browser.png)

Drücken Sie in Ihrem Terminalfenster **STRG+C**, um den Webserver zu beenden.

## <a name="log-in-to-azure"></a>Anmelden an Azure

Als Nächstes verwenden wir die Azure CLI 2.0 in einem Terminalfenster, um die Ressourcen zu erstellen, die zum Hosten unserer Node.js-App in Azure benötigt werden. Melden Sie sich mit dem Befehl [az login](/cli/azure/#login) bei Ihrem Azure-Abonnement an, und befolgen Sie die Anweisungen auf dem Bildschirm.

```azurecli
az login
```

## <a name="configure-a-deployment-user"></a>Konfigurieren eines Bereitstellungsbenutzers

Für FTP und lokales Git muss zur Authentifizierung Ihrer Bereitstellung ein Bereitstellungsbenutzer auf dem Server konfiguriert werden. Bei der Erstellung eines Bereitstellungsbenutzers handelt es sich um einen einmaligen Konfigurationsschritt. Notieren Sie sich den Benutzernamen und das Kennwort, da Sie beides in einem späteren Schritt benötigen.

> [!NOTE]
> Ein Bereitstellungsbenutzer wird bei FTP- und lokalen Git-Bereitstellungen für eine Web-App benötigt.
> `username` und `password` gelten für die Kontoebene und unterscheiden sich daher von den Anmeldeinformationen für Ihr Azure-Abonnement. Diese Anmeldeinformationen müssen nur einmal erstellt werden.
>

Verwenden Sie den Befehl [az appservice web deployment user set](/cli/azure/appservice/web/deployment/user#set), um Ihre Anmeldeinformationen auf der Kontoebene zu erstellen.

```azurecli
az appservice web deployment user set --user-name <username> --password <password>
```

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie mit dem Befehl [az group create](/cli/azure/group#create) eine Ressourcengruppe. Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen wie Web-Apps, Datenbanken und Speicherkonten bereitgestellt und verwaltet werden.

```azurecli
az group create --name myResourceGroup --location westeurope
```

## <a name="create-an-azure-app-service"></a>Erstellen einer Azure App Service-Instanz

Erstellen Sie mit dem Befehl [az appservice plan create](/cli/azure/appservice/plan#create) einen Linux-basierten App Service-Plan.

> [!NOTE]
> Ein App Service-Plan stellt die Sammlung physischer Ressourcen dar, die zum Hosten Ihrer Apps verwendet werden. Alle einem App Service-Plan zugewiesenen Anwendungen teilen sich die durch den Plan definierten Ressourcen. Das spart Kosten, wenn Sie mehrere Apps hosten.
>
> In App Service-Plänen wird Folgendes definiert:
> * Region („Europa, Norden“, „USA, Osten“, „Asien, Südosten“)
> * Instanzgröße (klein, mittel, groß)
> * Skalierung (Instanzenanzahl)
> * SKU (Free, Shared, Basic, Standard, Premium)
>

Im folgenden Beispiel wird ein App Service-Plan für Linux-Worker namens `quickStartPlan` mit dem Tarif **Standard** erstellt.

```azurecli
az appservice plan create --name quickStartPlan --resource-group myResourceGroup --sku S1 --is-linux
```

Nach Erstellung des App Service-Plans zeigt die Azure-Befehlszeilenschnittstelle Informationen wie im folgenden Beispiel an.

```json
{
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/quickStartPlan",
    "kind": "linux",
    "location": "West Europe",
    "sku": {
    "capacity": 1,
    "family": "S",
    "name": "S1",
    "tier": "Standard"
    },
    "status": "Ready",
    "type": "Microsoft.Web/serverfarms"
}
```

## <a name="create-a-web-app"></a>Erstellen einer Web-App

Nachdem Sie nun einen App Service-Plan erstellt haben, können Sie innerhalb des App Service-Plans `quickStartPlan` eine Web-App erstellen. Die Web-App bietet eine Hostingumgebung zum Bereitstellen unseres Codes sowie eine URL, unter der wir uns die bereitgestellte Anwendung ansehen können. Erstellen Sie die Web-App mit dem Befehl [az appservice web create](/cli/azure/appservice/web#create).

Ersetzen Sie im unten stehenden Befehl den Platzhalter `<app_name>` durch Ihren eigenen eindeutigen App-Namen. Da `<app_name>` als DNS-Standardwebsite für die Web-App verwendet wird, muss er für alle Apps in Azure eindeutig sein. Später können Sie der Web-App einen beliebigen benutzerdefinierten DNS-Eintrag zuordnen, bevor Sie sie für Ihre Benutzer verfügbar machen.

```azurecli
az appservice web create --name <app_name> --resource-group myResourceGroup --plan quickStartPlan
```

Nach Erstellung der Web-App zeigt die Azure-Befehlszeilenschnittstelle Informationen wie im folgenden Beispiel an.

```json
{
    "clientAffinityEnabled": true,
    "defaultHostName": "<app_name>.azurewebsites.net",
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/sites/<app_name>",
    "isDefaultContainer": null,
    "kind": "app",
    "location": "West Europe",
    "name": "<app_name>",
    "repositorySiteName": "<app_name>",
    "reserved": true,
    "resourceGroup": "myResourceGroup",
    "serverFarmId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/quickStartPlan",
    "state": "Running",
    "type": "Microsoft.Web/sites",
}
```

Navigieren Sie zu der Website, um sich Ihre neu erstellte Web-App anzusehen.

```bash
http://<app_name>.azurewebsites.net
```

![app-service-web-service-created](media/app-service-web-get-started-nodejs-poc/app-service-web-service-created.png)

Wir haben nun also eine neue leere Web-App in Azure erstellt. Als Nächstes konfigurieren wir unsere Web-App für die Verwendung von Node.js und stellen sie dafür bereit.

## <a name="configure-to-use-nodejs"></a>Konfigurieren für die Verwendung von Node.js

Konfigurieren Sie die Web-App mithilfe des Befehls [az appservice web config update](/cli/azure/app-service/web/config#update) für die Verwendung der Node.js-Version `6.9.3`.

> [!TIP]
> Wenn Sie die Node.js-Version auf diese Weise festlegen, wird ein von der Plattform bereitgestellter Standardcontainer verwendet. Informationen zur Verwendung eines eigenen Containers finden Sie in der Befehlszeilenreferenz für den Befehl [az appservice web config container update](/cli/azure/appservice/web/config/container#update).

```azurecli
az appservice web config update --linux-fx-version "NODE|6.9.3" --startup-file process.json --name <app_name> --resource-group myResourceGroup
```

## <a name="configure-local-git-deployment"></a>Konfigurieren der lokalen Git-Bereitstellung

Bei der Bereitstellung für Ihre Web-App haben Sie verschiedene Optionen. Hierzu zählen beispielsweise FTP, lokales Git sowie GitHub, Visual Studio Team Services und Bitbucket.

Konfigurieren Sie mit dem Befehl [az appservice web source-control config-local-git](/cli/azure/appservice/web/source-control#config-local-git) den lokalen Git-Zugriff auf die Web-App.

```azurecli
az appservice web source-control config-local-git --name <app_name> --resource-group myResourceGroup --query url --output tsv
```

Kopieren Sie die Terminalausgabe für den nächsten Schritt.

```bash
https://<username>@<app_name>.scm.azurewebsites.net:443/<app_name>.git
```

## <a name="push-to-azure-from-git"></a>Übertragen von Git an Azure mithilfe von Push

Fügen Sie Ihrem lokalen Git-Repository eine Azure-Remoteinstanz hinzu.

```bash
git remote add azure <paste-previous-command-output-here>
```

Führen Sie einen Pushvorgang an die Azure-Remoteinstanz durch, um Ihre Anwendung bereitzustellen. Sie werden zur Angabe des Kennworts aufgefordert, das Sie zuvor bei der Erstellung des Bereitstellungsbenutzers angegeben haben.

```azurecli
git push azure master
```

Während der Bereitstellung meldet Azure App Service seinen Status mit Git.

```bash
Counting objects: 23, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (21/21), done.
Writing objects: 100% (23/23), 3.71 KiB | 0 bytes/s, done.
Total 23 (delta 8), reused 7 (delta 1)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id 'bf114df591'.
remote: Generating deployment script.
remote: Generating deployment script for node.js Web Site
remote: Generated deployment script files
remote: Running deployment command...
remote: Handling node.js deployment.
remote: Kudu sync from: '/home/site/repository' to: '/home/site/wwwroot'
remote: Copying file: '.gitignore'
remote: Copying file: 'LICENSE'
remote: Copying file: 'README.md'
remote: Copying file: 'index.js'
remote: Copying file: 'package.json'
remote: Copying file: 'process.json'
remote: Deleting file: 'hostingstart.html'
remote: Ignoring: .git
remote: Using start-up script index.js from package.json.
remote: Node.js versions available on the platform are: 4.4.7, 4.5.0, 6.2.2, 6.6.0, 6.9.1.
remote: Selected node.js version 6.9.1. Use package.json file to choose a different version.
remote: Selected npm version 3.10.8
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
To https://<app_name>.scm.azurewebsites.net:443/<app_name>.git
 * [new branch]      master -> master
```

## <a name="browse-to-the-app"></a>Navigieren zur App

Navigieren Sie in Ihrem Webbrowser zu der bereitgestellten Anwendung.

```bash
http://<app_name>.azurewebsites.net
```

Diesmal wird die Seite mit der Nachricht „Hello World“ unter Verwendung unseres Node.js-Codes als Azure App Service-Web-App ausgeführt.

## <a name="updating-and-deploying-the-code"></a>Aktualisieren und Bereitstellen des Codes

Öffnen Sie die Datei `index.js` innerhalb der Node.js-App mit einem lokalen Text-Editor, und ändern Sie den Text im Aufruf von `response.end` geringfügig:

```nodejs
response.end("Hello Azure!");
```

Committen Sie Ihre Änderungen in Git, und übertragen Sie die Codeänderungen mithilfe von Push an Azure.

```bash
git commit -am "updated output"
git push azure master
```

Wechseln Sie nach Abschluss der Bereitstellung wieder zu dem Browserfenster, das im Schritt „Navigieren zur App“ geöffnet wurde, und wählen Sie die Option „Aktualisieren“ aus.

![hello-world-in-browser](media/app-service-web-get-started-nodejs-poc/hello-world-in-browser.png)

## <a name="manage-your-new-azure-web-app"></a>Verwalten Ihrer neuen Azure-Web-App

Sehen Sie sich die soeben erstellte Web-App im Azure-Portal an.

Melden Sie sich hierzu bei [https://portal.azure.com](https://portal.azure.com) an.

Klicken Sie im linken Menü auf **App Services** und anschließend auf den Namen Ihrer Azure-Web-App.

![Portalnavigation zur Azure-Web-App](./media/app-service-web-get-started-nodejs-poc/nodejs-docs-hello-world-app-service-list.png)

Dadurch gelangen Sie auf das _Blatt_ Ihrer Web-App (eine Portalseite, die horizontal geöffnet wird).

Auf dem Blatt Ihrer Web-App wird standardmäßig die Seite **Übersicht** angezeigt. Diese Seite bietet einen Überblick über den Status Ihrer App. Hier können Sie auch einfache Verwaltungsaufgaben wie Durchsuchen, Beenden, Neustarten und Löschen durchführen. Die Registerkarten auf der linken Seite des Blatts zeigen die verschiedenen Konfigurationsseiten, die Sie öffnen können.

![App Service-Blatt im Azure-Portal](media/app-service-web-get-started-nodejs-poc/nodejs-docs-hello-world-app-service-detail.png)

Die Registerkarten auf dem Blatt zeigen die vielen tollen Features, mit denen Sie Ihre Web-App ausstatten können. Ein paar Beispiele:

* Zuordnen eines benutzerdefinierten DNS-Namens
* Binden eines benutzerdefinierten SSL-Zertifikats
* Konfigurieren von Continuous Deployment
* Zentrales und horizontales Hochskalieren
* Hinzufügen einer Benutzerauthentifizierung

**Glückwunsch!** Sie haben Ihre erste Node.js-App für App Service bereitgestellt.

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Nächste Schritte

Machen Sie sich mit vorgefertigten [Azure CLI-Beispielen](app-service-cli-samples.md) vertraut.

