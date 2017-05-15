---
title: "Erstellen Ihrer ersten Python-Web-App in Azure in fünf Minuten | Microsoft-Dokumentation"
description: "Stellen Sie in wenigen Minuten Ihre erste Python-App vom Typ „Hello World“ in einer App Service-Web-App bereit."
services: app-service\web
documentationcenter: 
author: syntaxc4
manager: erikre
editor: 
ms.assetid: 928ee2e5-6143-4c0c-8546-366f5a3d80ce
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 03/17/2017
ms.author: cfowler
ms.translationtype: Human Translation
ms.sourcegitcommit: 2db2ba16c06f49fd851581a1088df21f5a87a911
ms.openlocfilehash: 2916ee6ba4753efdb8823f93c951a4f678b08ae4
ms.contentlocale: de-de
ms.lasthandoff: 05/08/2017


---
# <a name="create-a-python-application-on-web-app"></a>Erstellen einer Python-Anwendung in einer Web-App

In diesem Schnellstarttutorial erfahren Sie Schritt für Schritt, wie Sie eine Python-App entwickeln und für Azure bereitstellen. Wir führen die App mit Azure App Service aus und erstellen und konfigurieren darin eine neue Web-App über die Azure-Befehlszeilenschnittstelle. Anschließend stellen wir unsere Python-App mithilfe von Git für Azure bereit.

![hello-world-in-browser](media/app-service-web-get-started-python/hello-world-in-browser.png)

Die folgenden Schritte können auf einem Mac oder auf einem Computer unter Windows oder Linux ausgeführt werden. Der gesamte Prozess dauert etwa fünf Minuten.

## <a name="prerequisites"></a>Voraussetzungen

Installieren Sie vor der Ausführung dieses Beispiels zunächst die folgenden erforderlichen Komponenten:

1. [Laden Sie Git herunter, und installieren Sie es.](https://git-scm.com/)
1. [Laden Sie Python herunter, und installieren Sie es.](https://www.python.org/downloads/)
1. Laden Sie die [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) herunter, und installieren Sie sie.

## <a name="download-the-sample"></a>Herunterladen des Beispiels

Klonen Sie das Beispiel-App-Repository für „Hello World“ auf Ihren lokalen Computer.

```bash
git clone https://github.com/Azure-Samples/python-docs-hello-world
```

> [!TIP]
> Alternativ können Sie [das Beispiel als ZIP-Datei herunterladen](https://github.com/Azure-Samples/Python-docs-hello-world/archive/master.zip) und extrahieren.

Navigieren Sie zum Verzeichnis mit dem Beispielcode.

```bash
cd Python-docs-hello-world
```

## <a name="run-the-app-locally"></a>Lokales Ausführen der App

Öffnen Sie zum lokalen Ausführen der Anwendung ein Terminalfenster, und verwenden Sie die Befehlszeile `Python` für das Beispiel, um den integrierten Python-Webserver zu starten.

```bash
python main.py
```

Öffnen Sie einen Webbrowser, und navigieren Sie zu dem Beispiel.

```bash
http://localhost:5000
```

Auf der Seite wird die Nachricht **Hello World** aus der Beispiel-App angezeigt.

![localhost-hello-world-in-browser](media/app-service-web-get-started-python/localhost-hello-world-in-browser.png)

Drücken Sie in Ihrem Terminalfenster **STRG+C**, um den Webserver zu beenden.

## <a name="log-in-to-azure"></a>Anmelden an Azure

Als Nächstes verwenden wir die Azure CLI 2.0 in einem Terminalfenster, um die Ressourcen zu erstellen, die zum Hosten unserer Python-App in Azure benötigt werden. Melden Sie sich mit dem Befehl [az login](/cli/azure/#login) bei Ihrem Azure-Abonnement an, und befolgen Sie die Anweisungen auf dem Bildschirm.

```azurecli
az login
```

## <a name="configure-a-deployment-user"></a>Konfigurieren eines Bereitstellungsbenutzers

Für FTP und lokales Git muss zur Authentifizierung Ihrer Bereitstellung ein Bereitstellungsbenutzer auf dem Server konfiguriert werden. Bei der Erstellung eines Bereitstellungsbenutzers handelt es sich um einen einmaligen Konfigurationsschritt. Notieren Sie sich den Benutzernamen und das Kennwort, da Sie beides in einem späteren Schritt benötigen.

> [!NOTE]
> Ein Bereitstellungsbenutzer wird bei FTP- und lokalen Git-Bereitstellungen für eine Web-App benötigt.
> `username` und `password` gelten für die Kontoebene und unterscheiden sich daher von den Anmeldeinformationen für Ihr Azure-Abonnement. **Diese Anmeldeinformationen müssen nur einmal erstellt werden.**
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

Erstellen Sie mit dem Befehl [az appservice plan create](/cli/azure/appservice/plan#create) einen App Service-Plan.

> [!NOTE]
> Ein App Service-Plan stellt die Sammlung physischer Ressourcen dar, die zum Hosten Ihrer Apps verwendet werden. Alle einem App Service-Plan zugewiesenen Anwendungen teilen sich die durch den Plan definierten Ressourcen. Das spart Kosten, wenn Sie mehrere Apps hosten.
>
> In App Service-Plänen wird Folgendes definiert:
> * Region („Europa, Norden“, „USA, Osten“, „Asien, Südosten“)
> * Instanzgröße (klein, mittel, groß)
> * Skalierung (Instanzenanzahl)
> * SKU (Free, Shared, Basic, Standard, Premium)
>

Im folgenden Beispiel wird ein App Service-Plan namens `quickStartPlan` mit dem Tarif **FREE** erstellt.

```azurecli
az appservice plan create --name quickStartPlan --resource-group myResourceGroup --sku FREE
```

Nach Erstellung des App Service-Plans zeigt die Azure-Befehlszeilenschnittstelle Informationen wie im folgenden Beispiel an.

```json
{
"appServicePlanName": "quickStartPlan",
"geoRegion": "North Europe",
"id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/quickStartPlan",
"kind": "app",
"location": "North Europe",
"maximumNumberOfWorkers": 1,
"name": "quickStartPlan",
"provisioningState": "Succeeded",
"resourceGroup": "myResourceGroup",
"sku": {
  "capacity": 0,
  "family": "F",
  "name": "F1",
  "size": "F1",
  "tier": "Free"
},
"status": "Ready",
"type": "Microsoft.Web/serverfarms",
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
  "enabled": true,
  "enabledHostNames": [
    "<app_name>.azurewebsites.net",
    "<app_name>.scm.azurewebsites.net"
  ],
  "hostNames": [
    "<app_name>.azurewebsites.net"
  ],
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/sites/<app_name>",
  "kind": "app",
  "location": "North Europe",
  "outboundIpAddresses": "13.69.190.80,13.69.191.239,13.69.186.193,13.69.187.34",
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

![app-service-web-service-created](media/app-service-web-get-started-python/app-service-web-service-created.png)

Wir haben nun also eine neue leere Web-App in Azure erstellt. Als Nächstes konfigurieren wir unsere Web-App für die Verwendung von Python und stellen sie dafür bereit.

## <a name="configure-to-use-python"></a>Konfigurieren für die Verwendung von Python

Konfigurieren Sie die Web-App mithilfe des Befehls [az appservice web config update](/cli/azure/app-service/web/config#update) für die Verwendung der Python-Version `3.4`.

> [!TIP]
> Wenn Sie die Python-Version auf diese Weise festlegen, wird ein von der Plattform bereitgestellter Standardcontainer verwendet. Informationen zur Verwendung eines eigenen Containers finden Sie in der Befehlszeilenreferenz für den Befehl [az appservice web config container update](https://docs.microsoft.com/cli/azure/appservice/web/config/container#update).

```azurecli
az appservice web config update --python-version 3.4 --name <app-name> --resource-group myResourceGroup
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
Counting objects: 18, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (16/16), done.
Writing objects: 100% (18/18), 4.31 KiB | 0 bytes/s, done.
Total 18 (delta 4), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id '44e74fe7dd'.
remote: Generating deployment script.
remote: Generating deployment script for python Web Site
remote: Generated deployment script files
remote: Running deployment command...
remote: Handling python deployment.
remote: KuduSync.NET from: 'D:\home\site\repository' to: 'D:\home\site\wwwroot'
remote: Deleting file: 'hostingstart.html'
remote: Copying file: '.gitignore'
remote: Copying file: 'LICENSE'
remote: Copying file: 'main.py'
remote: Copying file: 'README.md'
remote: Copying file: 'requirements.txt'
remote: Copying file: 'virtualenv_proxy.py'
remote: Copying file: 'web.2.7.config'
remote: Copying file: 'web.3.4.config'
remote: Detected requirements.txt.  You can skip Python specific steps with a .skipPythonDeployment file.
remote: Detecting Python runtime from site configuration
remote: Detected python-3.4
remote: Creating python-3.4 virtual environment.
remote: .................................
remote: Pip install requirements.
remote: Successfully installed Flask click itsdangerous Jinja2 Werkzeug MarkupSafe
remote: Cleaning up...
remote: .
remote: Overwriting web.config with web.3.4.config
remote:         1 file(s) copied.
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
To https://<app_name>.scm.azurewebsites.net/<app_name>.git
 * [new branch]      master -> master
```

## <a name="browse-to-the-app"></a>Navigieren zur App

Navigieren Sie in Ihrem Webbrowser zu der bereitgestellten Anwendung.

```bash
http://<app_name>.azurewebsites.net
```

Diesmal wird die Seite mit der Nachricht „Hello World“ unter Verwendung unseres Python-Codes als Azure App Service-Web-App ausgeführt.

![hello-world-in-browser](media/app-service-web-get-started-python/hello-world-in-browser.png)

## <a name="updating-and-deploying-the-code"></a>Aktualisieren und Bereitstellen des Codes

Öffnen Sie die Datei `main.py` innerhalb der Python-App mit einem lokalen Text-Editor, und ändern Sie den Text in der Zeichenfolge neben der `return`-Anweisung geringfügig:

```python
return 'Hello, Azure!'
```

Committen Sie Ihre Änderungen in Git, und übertragen Sie die Codeänderungen mithilfe von Push an Azure.

```bash
git commit -am "updated output"
git push azure master
```

Wechseln Sie nach Abschluss der Bereitstellung wieder zu dem Browserfenster, das im Schritt „Navigieren zur App“ geöffnet wurde, und wählen Sie die Option „Aktualisieren“ aus.

![hello-azure-in-browser](media/app-service-web-get-started-python/hello-azure-in-browser.png)

## <a name="manage-your-new-azure-web-app"></a>Verwalten Ihrer neuen Azure-Web-App

Sehen Sie sich die soeben erstellte Web-App im Azure-Portal an.

Melden Sie sich hierzu bei [https://portal.azure.com](https://portal.azure.com) an.

Klicken Sie im linken Menü auf **App Services** und anschließend auf den Namen Ihrer Azure-Web-App.

![Portalnavigation zur Azure-Web-App](./media/app-service-web-get-started-python/app-service-list.png)

Dadurch gelangen Sie auf das _Blatt_ Ihrer Web-App (eine Portalseite, die horizontal geöffnet wird).

Auf dem Blatt Ihrer Web-App wird standardmäßig die Seite **Übersicht** angezeigt. Diese Seite bietet einen Überblick über den Status Ihrer App. Hier können Sie auch einfache Verwaltungsaufgaben wie Durchsuchen, Beenden, Neustarten und Löschen durchführen. Die Registerkarten auf der linken Seite des Blatts zeigen die verschiedenen Konfigurationsseiten, die Sie öffnen können.

![App Service-Blatt im Azure-Portal](media/app-service-web-get-started-python/app-service-detail.png)

Die Registerkarten auf dem Blatt zeigen die vielen tollen Features, mit denen Sie Ihre Web-App ausstatten können. Ein paar Beispiele:

* Zuordnen eines benutzerdefinierten DNS-Namens
* Binden eines benutzerdefinierten SSL-Zertifikats
* Konfigurieren von Continuous Deployment
* Zentrales und horizontales Hochskalieren
* Hinzufügen einer Benutzerauthentifizierung

**Glückwunsch!** Sie haben Ihre erste Python-App für App Service bereitgestellt.

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Machen Sie sich mit CLI-Skripts für Beispiel-Web-Apps vertraut.](app-service-cli-samples.md)

