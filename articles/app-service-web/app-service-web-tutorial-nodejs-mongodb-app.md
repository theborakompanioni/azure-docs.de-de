---
title: Erstellen einer Node.js- und MongoDB-Web-App in Azure | Microsoft Docs
description: Erfahren Sie, wie eine Node.js-App in Azure funktioniert, die eine Verbindung mit einer Cosmos DB-Datenbank mit einer MongoDB-Verbindungszeichenfolge aufweist.
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: 0b4d7d0e-e984-49a1-a57a-3c0caa955f0e
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 05/04/2017
ms.author: cephalin
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: f7479260c7c2e10f242b6d8e77170d4abe8634ac
ms.openlocfilehash: 14e399c06b21d9cabab28bdb195b02eb422f2d00
ms.contentlocale: de-de
ms.lasthandoff: 06/21/2017

---
# <a name="build-a-nodejs-and-mongodb-web-app-in-azure"></a>Erstellen einer Node.js- und MongoDB-Web-App in Azure

Azure-Web-Apps bietet einen hochgradig skalierbaren Webhosting-Dienst mit Self-Patching. In diesem Tutorial wird gezeigt, wie Sie eine Node.js-Web-App in Azure erstellen und mit einer MongoDB-Datenbank verbinden. Wenn Sie fertig sind, wird eine MEAN-Anwendung (MongoDB, Express, AngularJS und Node.js) in [Azure App Service](app-service-web-overview.md) ausgeführt. Zur Vereinfachung verwendet die Beispielanwendung das [MEAN.js-Webframework](http://meanjs.org/).

![In Azure App Service ausgeführte MEAN.js-App](./media/app-service-web-tutorial-nodejs-mongodb-app/meanjs-in-azure.png)

Sie lernen Folgendes:

> [!div class="checklist"]
> * Erstellen einer MongoDB-Datenbank in Azure
> * Verbinden einer Node.js-App mit MongoDB
> * Bereitstellen der Anwendung in Azure
> * Aktualisieren des Datenmodells und erneutes Bereitstellen der App
> * Streamen von Diagnoseprotokollen aus Azure
> * Verwalten der App im Azure-Portal

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

1. [Installieren Sie Git.](https://git-scm.com/)
1. [Installieren Sie Node.js und NPM.](https://nodejs.org/)
1. [Installieren von Gulp.js](http://gulpjs.com/) (erforderlich für [MEAN.js](http://meanjs.org/docs/0.5.x/#getting-started))
1. [Installieren und Ausführen von MongoDB Community Edition](https://docs.mongodb.com/manual/administration/install-community/) 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für dieses Thema die Azure CLI-Version 2.0 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu. 

## <a name="test-local-mongodb"></a>Testen der lokalen MongoDB

Öffnen Sie das Terminalfenster und wechseln Sie mit `cd` zum Verzeichnis `bin` der MongoDB-Installation. Sie können dieses Terminalfenster verwenden, um alle Befehle dieses Tutorials auszuführen.

Führen Sie `mongo` im Terminal aus, um eine Verbindung mit dem lokalen MongoDB-Server herzustellen.

```bash
mongo
```

Wenn die Verbindung erfolgreich ist, wird die MongoDB-Datenbank bereits ausgeführt. Wenn dies nicht der Fall ist, stellen Sie sicher, dass die lokale MongoDB-Datenbank durch Ausführen der Schritte unter [Installieren von MongoDB Community Edition](https://docs.mongodb.com/manual/administration/install-community/) gestartet wurde. Häufig ist MongoDB installiert, Sie müssen aber dennoch eine Instanz starten, indem Sie `mongod` ausführen. 

Wenn Sie die MongoDB-Datenbank getestet haben, geben Sie im Terminal `Ctrl+C` ein. 

## <a name="create-local-nodejs-app"></a>Erstellen einer lokalen Node.js-App

In diesem Schritt richten Sie das lokale Node.js-Projekt ein.

### <a name="clone-the-sample-application"></a>Klonen der Beispielanwendung

Wechseln Sie im Terminalfenster mit `cd` in ein Arbeitsverzeichnis.  

Führen Sie den folgenden Befehl aus, um das Beispielrepository zu klonen. 

```bash
git clone https://github.com/Azure-Samples/meanjs.git
```

Dieses Beispielrepository enthält eine Kopie des [MEAN.js-Repositorys](https://github.com/meanjs/mean). Es ist für die Ausführung in App Service angepasst. (Weitere Informationen finden Sie in der [Infodatei](https://github.com/Azure-Samples/meanjs/blob/master/README.md) im Repository „MEAN.js“.)

### <a name="run-the-application"></a>Ausführen der Anwendung

Führen Sie die folgenden Befehle aus, um die erforderlichen Pakete zu installieren und die Anwendung zu starten.

```bash
cd meanjs
npm install
npm start
```

Wenn die App vollständig geladen wurde, sollte eine ähnliche Meldung wie diese angezeigt werden:

```
--
MEAN.JS - Development Environment

Environment:     development
Server:          http://0.0.0.0:3000
Database:        mongodb://localhost/mean-dev
App version:     0.5.0
MEAN.JS version: 0.5.0
--
```

Navigieren Sie in einem Browser zu http://localhost:3000. Klicken Sie oben im Menü auf **Sign Up** (Registrieren), und erstellen Sie einen Testbenutzer. 

Die MEAN.js-Beispielanwendung speichert Benutzerdaten in der Datenbank. Wenn Sie erfolgreich einen Benutzer erstellt und diesen angemeldet haben, schreibt die App Daten in die lokale MongoDB-Datenbank.

![Erfolgreiche Verbindung zwischen MEAN.js und MongoDB](./media/app-service-web-tutorial-nodejs-mongodb-app/mongodb-connect-success.png)

Wählen Sie **Admin > Manage Articles** (Admin > Artikel verwalten) aus, um Artikel hinzuzufügen.

Sie können Node.js jederzeit beenden, indem Sie im Terminal `Ctrl+C` drücken. 

## <a name="create-production-mongodb"></a>Erstellen einer Produktions-MongoDB

In diesem Schritt erstellen Sie eine MongoDB-Datenbank in Azure. Wenn Ihre App in Azure bereitgestellt ist, nutzt sie diese Clouddatenbank.

Für MongoDB verwendet dieses Tutorial [Azure Cosmos DB](/azure/documentdb/). Cosmos DB unterstützt MongoDB-Clientverbindungen.

### <a name="log-in-to-azure"></a>Anmelden an Azure

Sie verwenden die Azure CLI 2.0, um die zum Hosten Ihrer App in Azure erforderlichen Ressourcen zu erstellen. Melden Sie sich mit dem Befehl [az login](/cli/azure/#login) bei Ihrem Azure-Abonnement an, und befolgen Sie die Anweisungen auf dem Bildschirm.

```azurecli-interactive
az login
```   

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie mit dem Befehl [az group create](/cli/azure/group#create) eine Ressourcengruppe.

[!INCLUDE [Resource group intro](../../includes/resource-group.md)]

Im folgenden Beispiel wird eine Ressourcengruppe in der Region „Europa, Westen“ erstellt.

```azurecli-interactive
az group create --name myResourceGroup --location "West Europe"
```

Verwenden Sie den Azure CLI-Befehl [az appservice list-locations](/cli/azure/appservice#list-locations) zum Auflisten der verfügbaren Speicherorte. 

### <a name="create-a-cosmos-db-account"></a>Erstellen eines Cosmos DB-Kontos

Erstellen Sie mit dem Befehl [az cosmosdb create](/cli/azure/cosmosdb#create) ein Cosmos DB-Konto.

Ersetzen Sie im folgenden Befehl den Platzhalter *\<cosmosdb_name>* durch einen eindeutigen Cosmos DB-Namen. Dieser eindeutige Name wird als Teil des Cosmos DB-Endpunkts (`https://<cosmosdb_name>.documents.azure.com/`) verwendet, daher muss er für alle Cosmos DB-Konten in Azure eindeutig sein. Der Name darf nur Kleinbuchstaben, Ziffern und Bindestriche (-) enthalten, und er muss zwischen 3 und 50 Zeichen lang sein.

```azurecli-interactive
az cosmosdb create \
    --name <cosmosdb_name> \
    --resource-group myResourceGroup \
    --kind MongoDB
```

Der *--kind MongoDB*-Parameter ermöglicht MongoDB-Clientverbindungen.

Nach dem Erstellen des Cosmos DB-Kontos zeigt die Azure-Befehlszeilenschnittstelle ähnliche Informationen wie im folgenden Beispiel an:

```json
{
  "consistencyPolicy":
  {
    "defaultConsistencyLevel": "Session",
    "maxIntervalInSeconds": 5,
    "maxStalenessPrefix": 100
  },
  "databaseAccountOfferType": "Standard",
  "documentEndpoint": "https://<cosmosdb_name>.documents.azure.com:443/",
  "failoverPolicies": 
  ...
  < Output truncated for readability >
}
```

## <a name="connect-app-to-production-mongodb"></a>Verbinden der App mit der MongoDB-Produktionsinstanz

In diesem Schritt verbinden Sie die MEAN.js-Beispielanwendung mithilfe einer MongoDB-Verbindungszeichenfolge mit der soeben erstellten Cosmos DB-Datenbank. 

### <a name="retrieve-the-database-key"></a>Abrufen des Datenbankschlüssels

Um eine Verbindung mit der Cosmos DB-Datenbank herstellen zu können, benötigen Sie den Datenbankschlüssel. Rufen Sie den Primärschlüssel mit dem Befehl [az cosmosdb list-keys](/cli/azure/cosmosdb#list-keys) ab.

```azurecli-interactive
az cosmosdb list-keys --name <cosmosdb_name> --resource-group myResourceGroup
```

An der Azure-CLI werden Informationen ähnlich wie im folgenden Beispiel angezeigt:

```json
{
  "primaryMasterKey": "RS4CmUwzGRASJPMoc0kiEvdnKmxyRILC9BWisAYh3Hq4zBYKr0XQiSE4pqx3UchBeO4QRCzUt1i7w0rOkitoJw==",
  "primaryReadonlyMasterKey": "HvitsjIYz8TwRmIuPEUAALRwqgKOzJUjW22wPL2U8zoMVhGvregBkBk9LdMTxqBgDETSq7obbwZtdeFY7hElTg==",
  "secondaryMasterKey": "Lu9aeZTiXU4PjuuyGBbvS1N9IRG3oegIrIh95U6VOstf9bJiiIpw3IfwSUgQWSEYM3VeEyrhHJ4rn3Ci0vuFqA==",
  "secondaryReadonlyMasterKey": "LpsCicpVZqHRy7qbMgrzbRKjbYCwCKPQRl0QpgReAOxMcggTvxJFA94fTi0oQ7xtxpftTJcXkjTirQ0pT7QFrQ=="
}
```

Kopieren Sie den Wert von `primaryMasterKey`. Sie benötigen diese Informationen im nächsten Schritt.

<a name="devconfig"></a>
### <a name="configure-the-connection-string-in-your-nodejs-application"></a>Konfigurieren der Verbindungszeichenfolge in der Node.js-Anwendung

Öffnen Sie in Ihrem MEAN.js-Repository die Datei _config/env/production.js_.

Aktualisieren Sie im `db`-Objekt den Wert von `uri`:

* Ersetzen Sie die zwei Platzhalter *\<cosmosdb_name>* durch Ihren Cosmos DB-Datenbanknamen.
* Ersetzen Sie den Platzhalter *\<primary_master_key>* durch den Schlüssel, den Sie im vorherigen Schritt kopiert haben.

Der folgende Code zeigt das `db`-Objekt:

```javascript
db: {
  uri: 'mongodb://<cosmosdb_name>:<primary_master_key>@<cosmosdb_name>.documents.azure.com:10250/mean?ssl=true&sslverifycertificate=false',
  ...
},
```

Die Option `ssl=true` ist erforderlich, da [SSL für Cosmos DB erforderlich ist](../cosmos-db/connect-mongodb-account.md#connection-string-requirements). 

Speichern Sie die Änderungen.

### <a name="test-the-application-in-production-mode"></a>Testen der Anwendung im Produktionsmodus 

Führen Sie den folgenden Befehl aus, um Skripts für die Produktionsumgebung zu minimieren und zu bündeln. Bei diesem Vorgang werden die von der Produktionsumgebung benötigten Dateien generiert.

```bash
gulp prod
```

Führen Sie den folgenden Befehl aus, um die in _config/env/production.js_ konfigurierte Verbindungszeichenfolge zu verwenden.

```bash
NODE_ENV=production node server.js
```

`NODE_ENV=production` legt die Umgebungsvariable fest, die die Ausführung von „Node.js“ in der Produktionsumgebung anweist.  `node server.js` startet den Node.js-Server mit `server.js` im Repositorystamm. So wird die Node.js-Anwendung in Azure geladen. 

Wenn die App geladen ist, überprüfen Sie, ob sie in der Produktionsumgebung ausgeführt wird:

```
--
MEAN.JS

Environment:     production
Server:          http://0.0.0.0:8443
Database:        mongodb://<cosmosdb_name>:<primary_master_key>@<cosmosdb_name>.documents.azure.com:10250/mean?ssl=true&sslverifycertificate=false
App version:     0.5.0
MEAN.JS version: 0.5.0
```

Navigieren Sie in einem Browser zu http://localhost:8443. Klicken Sie oben im Menü auf **Sign Up** (Registrieren), und erstellen Sie einen Testbenutzer. Wenn Sie erfolgreich einen Benutzer erstellt und diesen angemeldet haben, schreibt die App Daten in die Cosmos DB-Datenbank in Azure. 

Geben Sie `Ctrl+C` im Terminal ein, um „Node.js“ zu beenden. 

## <a name="deploy-app-to-azure"></a>Bereitstellen von Apps in Azure

In diesem Schritt stellen Sie die mit MongoDB verbundene Node.js-Anwendung in Azure App Service bereit.

### <a name="create-an-app-service-plan"></a>Wie erstelle ich einen Plan?

Erstellen Sie mit dem Befehl [az appservice plan create](/cli/azure/appservice/plan#create) einen App Service-Plan. 

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

Im folgenden Beispiel wird der App Service-Plan _myAppServicePlan_ mit dem Tarif **FREE** erstellt:

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku FREE
```

Nach dem Erstellen des App Service-Plans zeigt die Azure-CLI Informationen wie im folgenden Beispiel an:

```json 
{ 
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "North Europe",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan", 
  "kind": "app",
  "location": "North Europe",
  "maximumNumberOfWorkers": 1,
  "name": "myAppServicePlan",
  ...
  < Output has been truncated for readability >
} 
```

### <a name="create-a-web-app"></a>Erstellen einer Web-App

Erstellen Sie eine Web-App im App Service-Plan `myAppServicePlan` mithilfe des Befehls [az webapp create](/cli/azure/webapp#create). 

Die Web-App bietet Ihnen eine Hostingumgebung zum Bereitstellen Ihres Codes sowie eine URL, unter der Sie sich die bereitgestellte Anwendung ansehen können. Erstellen Sie damit die Web-App. 

Ersetzen Sie im folgenden Befehl den Platzhalter *\<app_name>* durch einen eindeutigen App-Namen. Dieser Name wird als Teil der Standard-URL der Web-App verwendet. Daher muss er für alle Apps in Azure App Service eindeutig sein. 

```azurecli-interactive
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan
```

Nach dem Erstellen der Web-App zeigt die Azure-Befehlszeilenschnittstelle Informationen wie im folgenden Beispiel an: 

```json 
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "enabled": true,
  ...
  < Output has been truncated for readability >
}
```

### <a name="configure-an-environment-variable"></a>Konfigurieren einer Umgebungsvariablen

Weiter oben in diesem Tutorial wurde die Verbindungszeichenfolge für die Datenbank in _config/env/production.js_ hartcodiert. In Übereinstimmung mit den Best Practices für die Sicherheit sollten diese vertraulichen Daten nicht im Git-Repository gespeichert werden. Für Ihre App, die in Azure ausgeführt wird, verwenden Sie stattdessen eine Umgebungsvariable.

In App Service legen Sie Umgebungsvariablen als _App-Einstellungen_ fest, indem Sie den Befehl [az webapp config appsettings update](/cli/azure/webapp/config/appsettings#update) verwenden. 

Im folgenden Beispiel wird die App-Einstellung `MONGODB_URI` in der Azure-Web-App konfiguriert. Ersetzen Sie die Platzhalter *\<app_name>*, *\<cosmosdb_name>* und *\<primary_master_key>*.

```azurecli-interactive
az webapp config appsettings update \
    --name <app_name> \
    --resource-group myResourceGroup \
    --settings MONGODB_URI="mongodb://<cosmosdb_name>:<primary_master_key>@<cosmosdb_name>.documents.azure.com:10250/mean?ssl=true"
```

Im Node.js-Code greifen Sie auf diese App-Einstellung mit `process.env.MONGODB_URI` zu, genauso wie Sie auf eine Umgebungsvariable zugreifen würden. 

Machen Sie jetzt die Änderungen an _config/env/production.js_ mit dem folgenden Befehl rückgängig:

```bash
git checkout -- .
```

Öffnen Sie _config/env/production.js_ wieder. Beachten Sie, dass die MEAN.js-Standard-App bereits für die Verwendung der `MONGODB_URI`-Umgebungsvariablen konfiguriert ist, die Sie erstellt haben.

```javascript
db: {
  uri: ... || process.env.MONGODB_URI || ...,
  ...
},
```

### <a name="configure-local-git-deployment"></a>Konfigurieren der lokalen Git-Bereitstellung 

Erstellen Sie Anmeldeinformationen für die Bereitstellung mit dem Befehl [az webapp deployment user set](/cli/azure/webapp/deployment/user#set).

Bei der Bereitstellung Ihrer Anwendung im Azure App Service haben Sie verschiedene Optionen. Hierzu zählen beispielsweise FTP, lokales Git sowie GitHub, Visual Studio Team Services und Bitbucket. Für FTP und lokales Git muss zur Authentifizierung Ihrer Bereitstellung ein Bereitstellungsbenutzer auf dem Server konfiguriert werden. Dieser Bereitstellungsbenutzer ist auf der Kontoebene vorhanden und unterscheidet sich von Ihrem Azure-Abonnementkonto. Sie müssen diesen Bereitstellungsbenutzer nur einmal konfigurieren.

Ersetzen Sie im folgenden Befehl *\<user-name>* und *\<password>* durch einen neuen Benutzernamen und ein neues Kennwort. Der Benutzername muss eindeutig sein. Das Kennwort muss mindestens acht Zeichen lang sein und zwei der folgenden drei Elemente enthalten: Buchstaben, Zahlen, Symbole. Wenn Sie den Fehler ` 'Conflict'. Details: 409` erhalten, müssen Sie den Benutzernamen ändern. Wenn Sie den Fehler ` 'Bad Request'. Details: 400` erhalten, müssen Sie ein sichereres Kennwort verwenden.

```azurecli-interactive
az appservice web deployment user set --user-name <username> --password <password>
```

Notieren Sie sich den Benutzernamen und das Kennwort für die Verwendung in späteren Schritten, wenn Sie die Anwendung bereitstellen.

Konfigurieren Sie mit dem Befehl [az webapp deployment source config-local-git](/cli/azure/webapp/deployment/source#config-local-git) den lokalen Git-Zugriff auf die Azure-Web-App. 

```azurecli-interactive
az webapp deployment source config-local-git --name <app_name> --resource-group myResourceGroup
```

Wenn der Bereitstellungsbenutzer konfiguriert wurde, zeigt die Azure-CLI die Bereitstellungs-URL für Ihre Azure-Web-App im folgenden Format an:

```bash 
https://<username>@<app_name>.scm.azurewebsites.net:443/<app_name>.git 
``` 

Kopieren Sie die Terminalausgabe für den nächsten Schritt. 

### <a name="push-to-azure-from-git"></a>Übertragen von Git an Azure mithilfe von Push

Fügen Sie Ihrem lokalen Git-Repository eine Azure-Remoteinstanz hinzu. 

```bash
git remote add azure <paste_copied_url_here> 
```

Führen Sie einen Pushvorgang an die Azure-Remoteinstanz durch, um die Node.js-Anwendung bereitzustellen. Sie werden zur Angabe des Kennworts aufgefordert, das Sie zuvor bei der Erstellung des Bereitstellungsbenutzers angegeben haben. 

```bash
git push azure master
```

Während der Bereitstellung meldet Azure App Service seinen Status mit Git.

```bash
Counting objects: 5, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (5/5), done.
Writing objects: 100% (5/5), 489 bytes | 0 bytes/s, done.
Total 5 (delta 3), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id '6c7c716eee'.
remote: Running custom deployment command...
remote: Running deployment command...
remote: Handling node.js deployment.
.
.
.
remote: Deployment successful.
To https://<app_name>.scm.azurewebsites.net/<app_name>.git
 * [new branch]      master -> master
``` 

Sie werden feststellen, dass beim Bereitstellungsprozess [Gulp](http://gulpjs.com/) nach `npm install` ausgeführt wird. App Service führt während der Bereitstellung keine Gulp- oder Grunt-Aufgaben aus. Daher enthält dieses Beispielrepository zwei zusätzliche Dateien im Stammverzeichnis für die Aktivierung: 

- _.deployment:_ Diese Datei weist App Service an, `bash deploy.sh` als benutzerdefiniertes Bereitstellungsskript auszuführen.
- _deploy.sh:_ das benutzerdefinierte Bereitstellungsskript. Wenn Sie die Datei überprüfen, sehen Sie, dass `gulp prod` nach `npm install` und `bower install` ausgeführt wird. 

Mit diesem Ansatz können Sie beliebige Schritte zur Git-basierten Bereitstellung hinzufügen. Wenn Sie Ihre Azure-Web-App zu irgendeinem Zeitpunkt neu starten, werden diese Automatisierungsaufgaben von App Service nicht nochmals ausgeführt.

### <a name="browse-to-the-azure-web-app"></a>Navigieren Sie zur Azure-Web-App. 

Navigieren Sie in Ihrem Webbrowser zu der bereitgestellten Web-App. 

```bash 
http://<app_name>.azurewebsites.net 
``` 

Klicken Sie oben im Menü auf **Sign Up** (Registrieren), und erstellen Sie einen Dummy-Benutzer. 

Wenn der Vorgang erfolgreich abgeschlossen und der erstellte Benutzer automatisch in der App angemeldet wird, verfügt die MEAN.js-App in Azure über eine Verbindung mit der MongoDB-Datenbank (Cosmos DB). 

![In Azure App Service ausgeführte MEAN.js-App](./media/app-service-web-tutorial-nodejs-mongodb-app/meanjs-in-azure.png)

Wählen Sie **Admin > Manage Articles** (Admin > Artikel verwalten) aus, um Artikel hinzuzufügen. 

**Glückwunsch!** Sie führen eine datengesteuerte Node.js-app in Azure App Service aus.

## <a name="update-data-model-and-redeploy"></a>Aktualisieren und erneutes Bereitstellen des Datenmodells

In diesem Schritt ändern Sie das `article`-Datenmodell und veröffentlichen die Änderung in Azure.

### <a name="update-the-data-model"></a>Aktualisieren des Datenmodells

Öffnen Sie _modules/articles/server/models/article.server.model.js_.

Fügen Sie in `ArticleSchema` einen `String`-Typ mit dem Namen `comment` hinzu. Anschließend sollte Ihr Schemacode wie folgt aussehen:

```javascript
var ArticleSchema = new Schema({
  ...,
  user: {
    type: Schema.ObjectId,
    ref: 'User'
  },
  comment: {
    type: String,
    default: '',
    trim: true
  }
});
```

### <a name="update-the-articles-code"></a>Aktualisieren Sie des articles-Codes

Aktualisieren Sie den restlichen `articles`-Code so, dass `comment` verwendet wird.

Sie müssen fünf Dateien ändern – den Servercontroller und die vier Clientansichten. 

Öffnen Sie _modules/articles/server/controllers/articles.server.controller.js_.

Fügen Sie in der `update`-Funktion eine Zuweisung für `article.comment` hinzu. Der folgende Code zeigt die abgeschlossene `update`-Funktion:

```javascript
exports.update = function (req, res) {
  var article = req.article;

  article.title = req.body.title;
  article.content = req.body.content;
  article.comment = req.body.comment;

  ...
};
```

Öffnen Sie _modules/articles/client/views/view-article.client.view.html_.

Fügen Sie unmittelbar vor dem schließenden `</section>`-Tag die folgende Zeile hinzu, um `comment` und die verbleibenden Artikeldaten anzuzeigen:

```HTML
<p class="lead" ng-bind="vm.article.comment"></p>
```

Öffnen Sie _modules/articles/client/views/list-articles.client.view.html_.

Fügen Sie unmittelbar vor dem schließenden `</a>`-Tag die folgende Zeile hinzu, um `comment` und die verbleibenden Artikeldaten anzuzeigen:

```HTML
<p class="list-group-item-text" ng-bind="article.comment"></p>
```

Öffnen Sie _modules/articles/client/views/admin/list-articles.client.view.html_.

Fügen Sie im `<div class="list-group">`-Element und unmittelbar vor dem schließenden `</a>`-Tag die folgende Zeile hinzu, um `comment` und die verbleibenden Artikeldaten anzuzeigen:

```HTML
<p class="list-group-item-text" data-ng-bind="article.comment"></p>
```

Öffnen Sie _modules/articles/client/views/admin/form-article.client.view.html_.

Suchen Sie nach dem `<div class="form-group">`-Element mit der Schaltfläche zum Senden, die wie folgt aussieht:

```HTML
<div class="form-group">
  <button type="submit" class="btn btn-default">{{vm.article._id ? 'Update' : 'Create'}}</button>
</div>
```

Fügen Sie unmittelbar vor diesem Tag ein weiteres `<div class="form-group">`-Element ein, das es Benutzern erlaubt, das `comment`-Feld zu bearbeiten. Das neue Element sollte folgendermaßen aussehen:

```HTML
<div class="form-group">
  <label class="control-label" for="comment">Comment</label>
  <textarea name="comment" data-ng-model="vm.article.comment" id="comment" class="form-control" cols="30" rows="10" placeholder="Comment"></textarea>
</div>
```

### <a name="test-your-changes-locally"></a>Lokales Testen der Änderungen

Speichern Sie alle Änderungen.

Testen Sie Ihre Änderungen nochmals im Produktionsmodus.

```bash
gulp prod
NODE_ENV=production node server.js
```

> [!NOTE]
> Denken Sie daran, dass _config/env/production.js_ zurückgesetzt wurde und die Umgebungsvariable `MONGODB_URI` nur in der Azure-Web-App und nicht auf dem lokalen Computer festgelegt ist. In der Konfigurationsdatei können Sie sehen, dass die Produktionskonfiguration als Standard eine lokale MongoDB-Datenbank verwendet. Dadurch wird sichergestellt, dass Sie keine Produktionsdaten ändern, wenn Sie Ihre Änderungen am Code lokal testen.

Navigieren Sie in einem Browser zu `http://localhost:8443`, und vergewissern Sie sich, dass Sie angemeldet sind.

Wählen Sie **Admin > Manage Articles** (Admin > Artikel verwalten) aus, und fügen Sie dann einen Artikel hinzu, indem Sie die Schaltfläche **+** auswählen.

Nun wird das neue `Comment`-Textfeld angezeigt.

![Hinzugefügtes Kommentarfeld für Artikel](./media/app-service-web-tutorial-nodejs-mongodb-app/added-comment-field.png)

Geben Sie `Ctrl+C` im Terminal ein, um „Node.js“ zu beenden. 

### <a name="publish-changes-to-azure"></a>Veröffentlichen von Änderungen in Azure

Committen Sie Ihre Änderungen in Git, und übertragen Sie die Codeänderungen mithilfe von Push an Azure.

```bash
git commit -am "added article comment"
git push azure master
```

Wechseln Sie nach Abschluss des `git push` zu Ihrer Azure-Web-App, und testen Sie die neuen Funktionen.

![Auf Azure veröffentlichte Änderungen an Modell und Datenbank](media/app-service-web-tutorial-nodejs-mongodb-app/added-comment-field-published.png)

Alle zuvor hinzugefügten Artikel werden weiterhin angezeigt. Vorhandene Daten in der Cosmos DB sind nicht verloren gegangen. Außerdem sind Ihre Änderungen am Datenschema und Ihre vorhandenen Daten erhalten geblieben.

## <a name="stream-diagnostic-logs"></a>Streaming des Diagnoseprotokolls 

Wenn Ihre Node.js-Anwendung in Azure App Service ausgeführt wird, können Sie die Konsolenprotokolle auf Ihr Terminal umleiten. Auf diese Weise erhalten Sie die gleichen Diagnosemeldungen, die Ihnen beim Debuggen von Anwendungsfehlern helfen.

Verwenden Sie zum Starten des Streamings von Protokolldateien den Befehl [az webapp log tail](/cli/azure/webapp/log#tail).

```azurecli-interactive
az webapp log tail --name <app_name> --resource-group myResourceGroup
``` 

Nachdem das Protokollstreaming gestartet wurde, aktualisieren Sie die Azure-Web-App im Browser, um Webdatenverkehr zu generieren. Sie stellen fest, dass Konsolenprotokolle jetzt auf Ihr Terminal umgeleitet werden.

Geben Sie `Ctrl+C` ein, um das Protokollstreaming zu einem beliebigen Zeitpunkt zu beenden. 

## <a name="manage-your-azure-web-app"></a>Verwalten Ihrer Azure-Web-App

Wechseln Sie zum [Azure-Portal](https://portal.azure.com), um die erstellte Web-App anzuzeigen.

Klicken Sie im linken Menü auf **App Services** und anschließend auf den Namen Ihrer Azure-Web-App.

![Portalnavigation zur Azure-Web-App](./media/app-service-web-tutorial-nodejs-mongodb-app/access-portal.png)

Standardmäßig wird im Portal die Seite **Übersicht** Ihrer Web-App angezeigt. Diese Seite bietet einen Überblick über den Status Ihrer App. Hier können Sie auch einfache Verwaltungsaufgaben wie Durchsuchen, Beenden, Neustarten und Löschen durchführen. Die Registerkarten links auf der Seite zeigen die verschiedenen Konfigurationsseiten, die Sie öffnen können.

![App Service-Seite im Azure-Portal](./media/app-service-web-tutorial-nodejs-mongodb-app/web-app-blade.png)

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

<a name="next"></a>
## <a name="next-steps"></a>Nächste Schritte

Sie haben Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen einer MongoDB-Datenbank in Azure
> * Verbinden einer Node.js-App mit MongoDB
> * Bereitstellen der Anwendung in Azure
> * Aktualisieren des Datenmodells und erneutes Bereitstellen der App
> * Streamen von Protokollen von Azure auf Ihr Terminal
> * Verwalten der App im Azure-Portal

Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie Ihrer Web-App einen benutzerdefinierten DNS-Namen zuordnen.

> [!div class="nextstepaction"] 
> [Zuordnen eines vorhandenen benutzerdefinierten DNS-Namens zu Azure-Web-Apps](app-service-web-tutorial-custom-domain.md)

