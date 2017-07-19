---
title: Erstellen einer PHP- und MySQL-Web-App in Azure | Microsoft-Dokumentation
description: "Erfahren Sie etwas über das Ausführen einer PHP-App in Azure mit Verbindung mit einer MySQL-Datenbank in Azure."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: 14feb4f3-5095-496e-9a40-690e1414bd73
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 06/19/2017
ms.author: cephalin
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: f7479260c7c2e10f242b6d8e77170d4abe8634ac
ms.openlocfilehash: 8a818a63409d914f82d2f0f8c894ba74ad8fa89d
ms.contentlocale: de-de
ms.lasthandoff: 06/21/2017

---
# <a name="build-a-php-and-mysql-web-app-in-azure"></a>Erstellen einer PHP- und MySQL-Web-App in Azure
[Azure-Web-Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) bietet einen hochgradig skalierbaren Webhosting-Dienst mit Self-Patching. In diesem Tutorial wird gezeigt, wie Sie eine PHP-Web-App in Azure erstellen und mit einer MySQL-Datenbank verbinden. Wenn Sie fertig sind, verfügen Sie über eine [Laravel](https://laravel.com/)-App, die in Azure App Service-Web-Apps ausgeführt wird.

![In Azure App Service ausgeführte PHP-App](./media/app-service-web-tutorial-php-mysql/complete-checkbox-published.png)

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer MySQL-Datenbank in Azure
> * Verbinden einer PHP-App mit MySQL
> * Bereitstellen der Anwendung in Azure
> * Aktualisieren des Datenmodells und erneutes Bereitstellen der App
> * Streamen von Diagnoseprotokollen aus Azure
> * Verwalten der App im Azure-Portal

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

* [Installation von Git](https://git-scm.com/)
* [Installation von PHP 5.6.4 oder höher](http://php.net/downloads.php)
* [Installation von Composer](https://getcomposer.org/doc/00-intro.md)
* Aktivieren der folgenden PHP-Erweiterungen für Laravel: OpenSSL, PDO-MySQL, Mbstring, Tokenizer, XML
* [Installieren und Starten von MySQL](https://dev.mysql.com/doc/refman/5.7/en/installing.html) 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für dieses Thema die Azure CLI-Version 2.0 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu. 

## <a name="prepare-local-mysql"></a>Vorbereiten der lokalen MySQL-Instanz

In diesem Schritt erstellen Sie eine Datenbank auf einem lokalen MySQL-Server für die Verwendung in diesem Tutorial.

### <a name="connect-to-mysql-server"></a>Herstellen einer Verbindung mit dem MySQL-Server
Stellen Sie in einem Terminalfenster eine Verbindung mit Ihrem lokalen MySQL-Server her. Sie können dieses Terminalfenster verwenden, um alle Befehle dieses Tutorials auszuführen.

```bash
mysql -u root -p
```

Wenn Sie zur Eingabe eines Kennworts aufgefordert werden, geben Sie das Kennwort für das `root`-Konto ein. Wenn Sie das Kennwort für Ihr Root-Konto vergessen haben, lesen Sie unter [MySQL: How to Reset the Root Password](https://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html) (MySQL: Zurücksetzen des Root-Kennworts) nach.

Wenn der Befehl erfolgreich ausgeführt wurde, wird der MySQL-Server ausgeführt. Stellen Sie andernfalls anhand der [Nachinstallationsschritte für MySQL](https://dev.mysql.com/doc/refman/5.7/en/postinstallation.html) sicher, dass Ihr lokaler MySQL-Server gestartet wurde.

### <a name="create-a-database"></a>Erstellen einer Datenbank

Erstellen Sie an der Eingabeaufforderung `mysql` eine Datenbank.

```sql 
CREATE DATABASE sampledb;
```

Beenden Sie die Serververbindung durch Eingabe von `quit`.

```sql
quit
```

<a name="step2"></a>

## <a name="create-local-php-app"></a>Erstellen einer lokalen PHP-App
In diesem Schritt rufen Sie eine Laravel-Beispielanwendung ab, konfigurieren ihre Datenbankverbindung und führen sie lokal aus. 

### <a name="clone-the-sample"></a>Klonen des Beispiels

Wechseln Sie im Terminalfenster mit `cd` in ein Arbeitsverzeichnis.  

Führen Sie den folgenden Befehl aus, um das Beispielrepository zu klonen. 

```bash
git clone https://github.com/Azure-Samples/laravel-tasks
```

Wechseln Sie mit `cd` in das geklonte Verzeichnis. Installieren Sie die erforderlichen Pakete.

```bash
cd laravel-tasks
composer install
```

### <a name="configure-mysql-connection"></a>Konfigurieren der MySQL-Verbindung

Erstellen Sie im Repositorystamm eine Datei mit dem Namen *.env*. Kopieren Sie die folgenden Variablen in die Datei *.env*. Ersetzen Sie den Platzhalter _&lt;root_password>_ durch das Kennwort des MySQL-Root-Benutzers.

```
APP_ENV=local
APP_DEBUG=true
APP_KEY=SomeRandomString

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_DATABASE=sampledb
DB_USERNAME=root
DB_PASSWORD=<root_password>
```

Informationen dazu, wie Laravel die _ENV_-Datei verwendet, finden Sie unter [Laravel Environment Configuration](https://laravel.com/docs/5.4/configuration#environment-configuration) (Konfigurieren der Laravel-Umgebung).

### <a name="run-the-sample"></a>Ausführen des Beispiels

Führen Sie [Laravel-Datenbankmigrationen](https://laravel.com/docs/5.4/migrations) durch, um die von der Anwendung benötigten Tabellen zu erstellen. Im Verzeichnis _database/migrations_ im Git-Repository können Sie sehen, welche Tabellen bei den Migrationen erstellt werden.

```bash
php artisan migrate
```

Generieren Sie einen neuen Anwendungsschlüssel für Laravel.

```bash
php artisan key:generate 
```

Führen Sie die Anwendung aus.

```bash
php artisan serve
```

Navigieren Sie in einem Browser zu `http://localhost:8000`. Fügen Sie auf der Seite einige Aufgaben hinzu.

![Erfolgreiche Verbindung zwischen PHP und MySQL](./media/app-service-web-tutorial-php-mysql/mysql-connect-success.png)

Geben Sie zum Beenden von PHP im Terminal `Ctrl + C` ein. 

## <a name="create-mysql-in-azure"></a>Erstellen von MySQL in Azure

In diesem Schritt erstellen Sie eine MySQL-Datenbank in [Azure-Datenbank für MySQL (Vorschau)](/azure/mysql). Später konfigurieren Sie die PHP-Anwendung für eine Verbindung mit dieser Datenbank.

### <a name="log-in-to-azure"></a>Anmelden an Azure

Melden Sie sich mit dem Befehl [az login](/cli/azure/#login) bei Ihrem Azure-Abonnement an, und befolgen Sie die Anweisungen auf dem Bildschirm. 

```azurecli
az login 
```
### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group-no-h.md)] 

### <a name="create-a-mysql-server"></a>Erstellen eines MySQL-Servers

Erstellen Sie mit dem Befehl [az mysql server create](/cli/azure/mysql/server#create) einen Server in Azure-Datenbank für MySQL (Vorschau).

Ersetzen Sie im folgenden Befehl den Platzhalter _&lt;mysql_server_name>_ durch Ihren MySQL-Servernamen (gültige Zeichen sind `a-z`, `0-9` und `-`). Dieser Name ist Teil des Hostnamens des MySQL-Servers (`<mysql_server_name>.database.windows.net`) und muss global eindeutig sein. 

```azurecli-interactive
az mysql server create \
    --name <mysql_server_name> \
    --resource-group myResourceGroup \
    --location "North Europe" \
    --user adminuser \
    --password MySQLAzure2017
```

Nach dem Erstellen des MySQL-Servers zeigt die Azure-Befehlszeilenschnittstelle Informationen wie im folgenden Beispiel an:

```json
{
  "administratorLogin": "adminuser",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "<mysql_server_name>.database.windows.net",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforMySQL/servers/<mysql_server_name>",
  "location": "northeurope",
  "name": "<mysql_server_name>",
  "resourceGroup": "myResourceGroup",
  ...
}
```

### <a name="configure-server-firewall"></a>Konfigurieren der Serverfirewall

Erstellen Sie mit dem Befehl [az mysql server firewall-rule create](/cli/azure/mysql/server/firewall-rule#create) eine Firewallregel für Ihren MySQL-Server, um Clientverbindungen zuzulassen. 

```azurecli-interactive
az mysql server firewall-rule create \
    --name allIPs \
    --server <mysql_server_name> \
    --resource-group myResourceGroup \
    --start-ip-address 0.0.0.0 \
    --end-ip-address 255.255.255.255
```

> [!NOTE]
> Für Azure-Datenbank für MySQL (Vorschauversion) besteht derzeit keine Beschränkung für ausschließliche Verbindungen mit Azure-Diensten. Da IP-Adressen in Azure dynamisch zugewiesen werden, ist es besser, alle IP-Adressen zu aktivieren. Der Dienst befindet sich in der Vorschauphase. Bessere Methoden zum Schützen Ihrer Datenbank sind geplant.
>
>

### <a name="connect-to-production-mysql-server"></a>Verbinden mit dem MySQL-Produktionsserver

Stellen Sie im Terminalfenster eine Verbindung mit dem MySQL-Server in Azure her. Verwenden Sie den Wert, den Sie zuvor für _&lt;mysql_server_name>_ angegeben haben.

```bash
mysql -u adminuser@<mysql_server_name> -h <mysql_server_name>.database.windows.net -P 3306 -p
```

Verwenden Sie bei der Aufforderung zum Eingeben das Kennwort _$tr0ngPa$w0rd!_, das Sie beim Erstellen der Datenbank angegeben haben.

### <a name="create-a-production-database"></a>Erstellen einer Produktionsdatenbank

Erstellen Sie an der Eingabeaufforderung `mysql` eine Datenbank.

```sql
CREATE DATABASE sampledb;
```

### <a name="create-a-user-with-permissions"></a>Erstellen eines Benutzers mit Berechtigungen

Erstellen Sie einen Datenbankbenutzer mit dem Namen _phpappuser_, und weisen Sie ihm alle Berechtigungen in der Datenbank `sampledb` zu.

```sql
CREATE USER 'phpappuser' IDENTIFIED BY 'MySQLAzure2017'; 
GRANT ALL PRIVILEGES ON sampledb.* TO 'phpappuser';
```

Beenden Sie die Serververbindung durch Eingabe von `quit`.

```sql
quit
```

## <a name="connect-app-to-azure-mysql"></a>Verbinden der App mit Azure MySQL

In diesem Schritt verbinden Sie die PHP-Anwendung mit der MySQL-Datenbank, die Sie in Azure-Datenbank für MySQL (Vorschauversion) erstellt haben. 

<a name="devconfig"></a>
### <a name="configure-the-connection"></a>Konfigurieren der Verbindung 

Erstellen Sie im Stammverzeichnis des Repositorys eine Datei _.env.production_, und kopieren Sie die folgenden Variablen in die Datei. Ersetzen Sie den Platzhalter _&lt;mysql_server_name>_.

```
APP_ENV=production
APP_DEBUG=true
APP_KEY=SomeRandomString

DB_CONNECTION=mysql
DB_HOST=<mysql_server_name>.database.windows.net
DB_DATABASE=sampledb
DB_USERNAME=phpappuser@<mysql_server_name>
DB_PASSWORD=MySQLAzure2017
MYSQL_SSL=true
```

Speichern Sie die Änderungen.

> [!TIP]
> Um Ihre MySQL-Verbindungsinformationen zu schützen, ist diese Datei bereits aus dem Git-Repository ausgeschlossen (siehe _.gitignore_ im Repositorystamm). Später erfahren Sie, wie Sie die Umgebungsvariablen in App Service so konfigurieren, dass eine Verbindung mit Ihrer Datenbank in Azure-Datenbank für MySQL (Vorschauversion) hergestellt wird. Bei Umgebungsvariablen benötigen Sie die *ENV*-Datei in App Service nicht. 
>

### <a name="configure-ssl-certificate"></a>Konfigurieren des SSL-Zertifikats

Standardmäßig erzwingt Azure-Datenbank für MySQL SSL-Verbindungen von Clients. Sie müssen ein _PEM_-SSL-Zertifikat verwenden, um eine Verbindung mit Ihrer MySQL-Datenbank in Azure herzustellen.

Öffnen Sie _config/database.php_, und fügen Sie `connections.mysql` die Parameter _sslmode_ und _options_ hinzu. Dies wird im folgenden Code veranschaulicht.

```php
'mysql' => [
    ...
    'sslmode' => env('DB_SSLMODE', 'prefer'),
    'options' => (env('MYSQL_SSL')) ? [
        PDO::MYSQL_ATTR_SSL_KEY    => '/ssl/certificate.pem', 
    ] : []
],
```

Informationen zum Generieren der Datei _certificate.pem_ finden Sie unter [Konfigurieren von SSL-Verbindungen in der Anwendung für eine sichere Verbindung mit der Azure-Datenbank für MySQL](../mysql/howto-configure-ssl.md).

> [!TIP]
> Der Pfad _/ssl/certificate.pem_ verweist auf eine vorhandene Datei _certificate.pem_ im Git-Repository. In diesem Tutorial wird die Datei der Einfachheit halber für Sie bereitgestellt. Die bewährte Methode besteht darin, für Ihre _PEM_-Zertifikate keinen Commit für die Quellcodeverwaltung durchzuführen. 
>
>

### <a name="test-the-application"></a>Testen der Anwendung

Führen Sie die Laravel-Datenbankmigrationen mit der Datei _.env.production_ als Umgebungsdatei aus, um die Tabellen in der MySQL-Datenbank in Azure-Datenbank für MySQL (Vorschau) zu erstellen. Beachten Sie, dass _.env.production_ die Verbindungsinformationen für Ihre MySQL-Datenbank in Azure enthält.

```bash
php artisan migrate --env=production --force
```

_.env.production_ verfügt noch nicht über einen gültigen Anwendungsschlüssel. Generieren Sie im Terminal einen neuen. 

```bash
php artisan key:generate --env=production --force
```

Führen Sie die Beispielanwendung mit _.env.production_ als Umgebungsdatei aus.

```bash
php artisan serve --env=production
```

Navigieren Sie zu `http://localhost:8000`. Wenn die Seite ohne Fehler geladen wird, ist die PHP-Anwendung mit der MySQL-Datenbank in Azure verbunden. 

Fügen Sie auf der Seite einige Aufgaben hinzu.

![Erfolgreiche Verbindung zwischen PHP und Azure-Datenbank für MySQL (Vorschau)](./media/app-service-web-tutorial-php-mysql/mysql-connect-success.png)

Geben Sie zum Beenden von PHP im Terminal `Ctrl + C` ein. 

### <a name="commit-your-changes"></a>Committen Ihrer Änderungen

Führen Sie die folgenden Git-Befehle aus, um für Ihre Änderungen einen Commit durchzuführen:

```bash
git add .
git commit -m "database.php updates"
```

Ihre App kann jetzt bereitgestellt werden.

## <a name="deploy-to-azure"></a>Bereitstellen in Azure
In diesem Schritt stellen Sie die mit MySQL verbundene PHP-Anwendung in Azure App Service bereit.

### <a name="create-an-app-service-plan"></a>Wie erstelle ich einen Plan?

[!INCLUDE [Create app service plan no h](../../includes/app-service-web-create-app-service-plan-no-h.md)] 

### <a name="create-a-web-app"></a>Erstellen einer Web-App

[!INCLUDE [Create web app no h](../../includes/app-service-web-create-web-app-no-h.md)] 

### <a name="set-the-php-version"></a>Festlegen der PHP-Version

Legen Sie die für die Anwendung erforderliche PHP-Version fest, indem Sie den Befehl [az webapp config set](/cli/azure/webapp/config#set) verwenden.

Der folgende Befehl legt die PHP-Version auf _7.0_ fest.

```azurecli-interactive
az webapp config set \
    --name <app_name> \
    --resource-group myResourceGroup \
    --php-version 7.0
```

### <a name="configure-database-settings"></a>Konfigurieren der Datenbankeinstellungen

Wie bereits erwähnt, können Sie mithilfe von Umgebungsvariablen in App Service eine Verbindung mit der Azure-MySQL-Datenbank herstellen.

In App Service legen Sie Umgebungsvariablen als _App-Einstellungen_ fest, indem Sie den Befehl [az webapp config appsettings set](/cli/azure/webapp/config/appsettings#set) verwenden. 

Mit dem folgenden Befehl werden die App-Einstellungen `DB_HOST`, `DB_DATABASE`, `DB_USERNAME` und `DB_PASSWORD` konfiguriert. Ersetzen Sie die Platzhalter _&lt;appname>_ und _&lt;mysql_server_name>_.

```azurecli-interactive
az webapp config appsettings set \
    --name <app_name> \
    --resource-group myResourceGroup \
    --settings DB_HOST="<mysql_server_name>.database.windows.net" DB_DATABASE="sampledb" DB_USERNAME="phpappuser@<mysql_server_name>" DB_PASSWORD="MySQLAzure2017" MYSQL_SSL="true"
```

Sie können die PHP-Methode [getenv](http://www.php.net/manual/function.getenv.php) verwenden, um auf die Einstellungen zuzugreifen. Im Laravel-Code wird ein [env](https://laravel.com/docs/5.4/helpers#method-env)-Wrapper für die PHP-Methode `getenv` verwendet. Die MySQL-Konfiguration in _config/database.php_ sieht beispielsweise wie im folgenden Code aus:

```php
'mysql' => [
    'driver'    => 'mysql',
    'host'      => env('DB_HOST', 'localhost'),
    'database'  => env('DB_DATABASE', 'forge'),
    'username'  => env('DB_USERNAME', 'forge'),
    'password'  => env('DB_PASSWORD', ''),
    ...
],
```

### <a name="configure-laravel-environment-variables"></a>Konfigurieren der Laravel-Umgebungsvariablen

Für Laravel ist in App Service ein Anwendungsschlüssel erforderlich. Sie können ihn mit App-Einstellungen konfigurieren.

Verwenden Sie `php artisan`, um einen neuen Anwendungsschlüssel zu generieren, ohne ihn in der Datei _.env_ zu speichern.

```bash
php artisan key:generate --show
```

Legen Sie den Anwendungsschlüssel in der App Service-Web-App fest, indem Sie den Befehl [az webapp config appsettings set](/cli/azure/webapp/config/appsettings#set) verwenden. Ersetzen Sie die Platzhalter _&lt;appname>_ und _&lt;outputofphpartisankey:generate>_.

```azurecli-interactive
az webapp config appsettings set \
    --name <app_name> \
    --resource-group myResourceGroup \
    --settings APP_KEY="<output_of_php_artisan_key:generate>" APP_DEBUG="true"
```

`APP_DEBUG="true"` weist Laravel an, Debuginformationen zurückzugeben, wenn die bereitgestellte Web-App Fehler feststellt. Legen Sie beim Ausführen einer Produktionsanwendung `false` fest, weil dies sicherer ist.

### <a name="set-the-virtual-application-path"></a>Festlegen des virtuellen Anwendungspfads

Legen Sie den virtuellen Anwendungspfad für die Web-App fest. Dieser Schritt ist nur erforderlich, da der [Lebenszyklus der Laravel-Anwendung](https://laravel.com/docs/5.4/lifecycle) im _öffentlichen_ Verzeichnis anstatt im Stammverzeichnis der Anwendung beginnt. Andere PHP-Frameworks, deren Lebenszyklus im Stammverzeichnis startet, können ohne manuelle Konfiguration des virtuellen Anwendungspfads ausgeführt werden.

Legen Sie den virtuellen Anwendungspfad mit dem Befehl [az resource update](/cli/azure/resource#update) fest. Ersetzen Sie den Platzhalter _&lt;appname>_.

```azurecli-interactive
az resource update \
    --name web \
    --resource-group myResourceGroup \
    --namespace Microsoft.Web \
    --resource-type config \
    --parent sites/<app_name> \
    --set properties.virtualApplications[0].physicalPath="site\wwwroot\public" \
    --api-version 2015-06-01
```

Standardmäßig verweist Azure App Service im Stammverzeichnis des virtuellen Anwendungspfads (_/_) auf das Stammverzeichnis der bereitgestellten Anwendungsdateien (_sites\wwwroot_). 

### <a name="configure-a-deployment-user"></a>Konfigurieren eines Bereitstellungsbenutzers

[!INCLUDE [Configure deployment user](../../includes/configure-deployment-user-no-h.md)] 

### <a name="configure-local-git-deployment"></a>Konfigurieren der lokalen Git-Bereitstellung 

[!INCLUDE [Configure local git](../../includes/app-service-web-configure-local-git-no-h.md)] 

### <a name="push-to-azure-from-git"></a>Übertragen von Git an Azure mithilfe von Push

Fügen Sie Ihrem lokalen Git-Repository eine Azure-Remoteinstanz hinzu. 

```bash
git remote add azure <paste_copied_url_here> 
```

Führen Sie einen Pushvorgang an die Azure-Remoteinstanz durch, um die PHP-Anwendung bereitzustellen. Sie werden zur Angabe des Kennworts aufgefordert, das Sie zuvor bei der Erstellung des Bereitstellungsbenutzers angegeben haben. 

```bash
git push azure master
```

Während der Bereitstellung meldet Azure App Service seinen Status mit Git.

```bash
Counting objects: 3, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 291 bytes | 0 bytes/s, done.
Total 3 (delta 2), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id 'a5e076db9c'.
remote: Running custom deployment command...
remote: Running deployment command...
...
< Output has been truncated for readability >
``` 

> [!NOTE]
> Sie werden feststellen, dass am Ende der Bereitstellung [Composer](https://getcomposer.org/)-Pakete installiert werden. App Service führt diese Automatisierungen nicht während der Bereitstellung aus. Daher enthält dieses Beispielrepository drei zusätzliche Dateien im Stammverzeichnis für die Aktivierung: 
>
> - `.deployment`: Diese Datei weist App Service an, `bash deploy.sh` als benutzerdefiniertes Bereitstellungsskript auszuführen.
> - `deploy.sh`: Das Der benutzerdefinierte Bereitstellungsskript. Wenn Sie die Datei überprüfen, sehen Sie, dass `php composer.phar install` nach `npm install` ausgeführt wird. 
> - `composer.phar`: Der Composer-Paket-Manager.
>
> Mit diesem Ansatz können Sie der Git-basierten Bereitstellung in App Service beliebige Schritte hinzufügen. Weitere Informationen finden Sie unter [Custom Deployment Script](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script) (Benutzerdefiniertes Bereitstellungsskript).
>
>


### <a name="browse-to-the-azure-web-app"></a>Navigieren Sie zur Azure-Web-App.

Browsen Sie zu `http://<app_name>.azurewebsites.net`, und fügen Sie der Liste einige Aufgaben hinzu. 

![In Azure App Service ausgeführte PHP-App](./media/app-service-web-tutorial-php-mysql/php-mysql-in-azure.png)

Glückwunsch! Sie führen eine datengesteuerte PHP-App in Azure App Service aus.

## <a name="update-model-and-redeploy"></a>Aktualisierungsmodell und erneute Bereitstellung

In diesem Schritt nehmen Sie eine einfache Änderung am `task`-Datenmodell und der Web-App vor und veröffentlichen das Update dann in Azure.

Für das tasks-Szenario ändern Sie die Anwendung, damit Sie eine Aufgabe als abgeschlossen kennzeichnen können. 

### <a name="add-a-column"></a>Hinzufügen eines Spaltennamens

Navigieren Sie im Terminal zum Stamm des Git-Repositorys.

Generieren Sie eine neue Datenbankmigration für die Tabelle `tasks`:

```bash
php artisan make:migration add_complete_column --table=tasks
```

Dieser Befehl zeigt den Namen der Migrationsdatei an, die generiert wird. Suchen Sie unter _database/migrations_ nach dieser Datei, und öffnen Sie sie.

Ersetzen Sie die `up` -Methode durch den folgenden Code:

```php
public function up()
{
    Schema::table('tasks', function (Blueprint $table) {
        $table->boolean('complete')->default(False);
    });
}
```

Mit dem obigen Code wird der Tabelle `tasks` die boolesche Spalte `complete` hinzugefügt.

Ersetzen Sie die `down`-Methode durch den folgenden Code für die Rollbackaktion:

```php
public function down()
{
    Schema::table('tasks', function (Blueprint $table) {
        $table->dropColumn('complete');
    });
}
```

Führen Sie im Terminal Laravel-Datenbankmigrationen aus, um die Änderung in der lokalen Datenbank vorzunehmen.

```bash
php artisan migrate
```

Das Modell `Task` (siehe _app/Task.php_) wird basierend auf der [Laravel-Namenskonvention](https://laravel.com/docs/5.4/eloquent#defining-models) standardmäßig der Tabelle `tasks` zugeordnet.

### <a name="update-application-logic"></a>Aktualisieren der Anwendungslogik

Öffnen Sie die Datei *routes/web.php*. Die Anwendung definiert hier die Routen und Geschäftslogik.

Fügen Sie mit folgendem Code am Ende der Datei eine Route hinzu:

```php
/**
 * Toggle Task completeness
 */
Route::post('/task/{id}', function ($id) {
    error_log('INFO: post /task/'.$id);
    $task = Task::findOrFail($id);

    $task->complete = !$task->complete;
    $task->save();

    return redirect('/');
});
```

Der obige Code führt eine einfache Aktualisierung des Datenmodells durch, indem der Wert von `complete` geändert wird.

### <a name="update-the-view"></a>Aktualisieren der Ansicht

Öffnen Sie die Datei *resources/views/tasks.blade.php*. Suchen Sie das öffnende Tag `<tr>`, und ersetzen Sie es durch Folgendes:

```html
<tr class="{{ $task->complete ? 'success' : 'active' }}" >
```

Mit dem obigen Code wird die Zeilenfarbe abhängig davon geändert, ob die Aufgabe abgeschlossen ist.

Die nächste Zeile enthält folgenden Code:

```html
<td class="table-text"><div>{{ $task->name }}</div></td>
```

Ersetzen Sie die gesamte Zeile durch den folgenden Code:

```html
<td>
    <form action="{{ url('task/'.$task->id) }}" method="POST">
        {{ csrf_field() }}

        <button type="submit" class="btn btn-xs">
            <i class="fa {{$task->complete ? 'fa-check-square-o' : 'fa-square-o'}}"></i>
        </button>
        {{ $task->name }}
    </form>
</td>
```

Der obige Code fügt die Schaltfläche zum Übermitteln hinzu, die auf die zuvor definierte Route verweist.

### <a name="test-the-changes-locally"></a>Lokales Testen der Änderungen

Führen Sie den Development Server aus dem Stammverzeichnis des Git-Repositorys aus.

```bash
php artisan serve
```

Navigieren Sie zu `http://localhost:8000`, um die Änderung des Aufgabenstatus anzuzeigen, und aktivieren Sie das Kontrollkästchen.

![Hinzugefügtes Kontrollkästchen in der Aufgabe](./media/app-service-web-tutorial-php-mysql/complete-checkbox.png)

Geben Sie zum Beenden von PHP im Terminal `Ctrl + C` ein. 

### <a name="publish-changes-to-azure"></a>Veröffentlichen von Änderungen in Azure

Führen Sie im Terminal Laravel-Datenbankmigrationen mit der Produktions-Verbindungszeichenfolge aus, um die Änderung in der Azure-Datenbank vorzunehmen.

```bash
php artisan migrate --env=production --force
```

Führen Sie für alle Änderungen in Git einen Commit aus, und übertragen Sie dann die Codeänderungen per Pushvorgang an Azure.

```bash
git add .
git commit -m "added complete checkbox"
git push azure master
```

Wechseln Sie nach Abschluss des `git push`-Vorgangs zur Azure-Web-App, und testen Sie die neuen Funktionen.

![Auf Azure veröffentlichte Änderungen an Modell und Datenbank](media/app-service-web-tutorial-php-mysql/complete-checkbox-published.png)

Wenn Sie Aufgaben hinzugefügt haben, werden sie in der Datenbank beibehalten. Bei Updates des Datenschemas bleiben vorhandene Daten erhalten.

## <a name="stream-diagnostic-logs"></a>Streaming des Diagnoseprotokolls 

Wenn die PHP-Anwendung in Azure App Service ausgeführt wird, können Sie die Konsolenprotokolle auf Ihr Terminal umleiten. Auf diese Weise erhalten Sie die gleichen Diagnosemeldungen, die Ihnen beim Debuggen von Anwendungsfehlern helfen.

Verwenden Sie zum Starten des Streamings von Protokolldateien den Befehl [az webapp log tail](/cli/azure/webapp/log#tail).

```azurecli-interactive 
az webapp log tail \
    --name <app_name> \
    --resource-group myResourceGroup
``` 

Nachdem das Protokollstreaming gestartet wurde, aktualisieren Sie die Azure-Web-App im Browser, um Webdatenverkehr zu generieren. Sie können jetzt Konsolenprotokolle sehen, die auf das Terminal umgeleitet werden. Falls Sie nicht sofort Konsolenprotokolle sehen, können Sie es nach 30 Sekunden noch einmal versuchen.

Zum Beenden des Protokollstreamings geben Sie `Ctrl`+`C` ein. 

> [!TIP]
> Eine PHP-Anwendung kann die Standardmethode [error_log()](http://php.net/manual/function.error-log.php) für die Ausgabe an die Konsole verwenden. Die Beispielanwendung verwendet diesen Ansatz in _app/Http/routes.php_.
>
> Als ein Webframework verwendet [Laravel Monolog](https://laravel.com/docs/5.4/errors) als Protokollierungsanbieter. Informationen dazu, wie Sie Monolog-Meldungen an die Konsole ausgeben, finden Sie unter [PHP: How to use monolog to log to console (php://out)](http://stackoverflow.com/questions/25787258/php-how-to-use-monolog-to-log-to-console-php-out) (PHP: Verwenden von Monolog für die Konsolenprotokollierung (php://out)).
>
>

## <a name="manage-the-azure-web-app"></a>Verwalten der Azure-Web-App

Wechseln Sie zum [Azure-Portal](https://portal.azure.com), um die erstellte Web-App zu verwalten.

Klicken Sie im linken Menü auf **App Services** und anschließend auf den Namen Ihrer Azure-Web-App.

![Portalnavigation zur Azure-Web-App](./media/app-service-web-tutorial-php-mysql/access-portal.png)

Die Übersichtsseite Ihrer Web-App wird angezeigt. Hier können Sie einfache Verwaltungsaufgaben wie Beenden, Starten, Neustarten, Durchsuchen und Löschen durchführen. 

Im linken Menü werden Seiten für die Konfiguration Ihrer App angezeigt. 

![App Service-Seite im Azure-Portal](./media/app-service-web-tutorial-php-mysql/web-app-blade.png)

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

<a name="next"></a>

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen einer MySQL-Datenbank in Azure
> * Verbinden einer PHP-App mit MySQL
> * Bereitstellen der Anwendung in Azure
> * Aktualisieren des Datenmodells und erneutes Bereitstellen der App
> * Streamen von Diagnoseprotokollen aus Azure
> * Verwalten der App im Azure-Portal

Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie einer Web-App einen benutzerdefinierten DNS-Namen zuordnen.

> [!div class="nextstepaction"] 
> [Zuordnen eines vorhandenen benutzerdefinierten DNS-Namens zu Azure-Web-Apps](app-service-web-tutorial-custom-domain.md)

