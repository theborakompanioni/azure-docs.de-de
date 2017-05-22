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
ms.date: 05/05/2017
ms.author: cephalin
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: ae0b63bc338cb3e96eae4593b96265aafbcbc029
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
# <a name="build-a-php-and-mysql-web-app-in-azure"></a>Erstellen einer PHP- und MySQL-Web-App in Azure
In diesem Tutorial wird gezeigt, wie Sie eine PHP-Web-App in Azure erstellen und mit einer MySQL-Datenbank verbinden. Wenn Sie fertig sind, verfügen Sie über eine [Laravel](https://laravel.com/)-Anwendung, die in [Azure App Service-Web-Apps](app-service-web-overview.md) ausgeführt wird.

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

Installieren Sie vor der Ausführung dieses Beispiels zunächst die folgenden erforderlichen Komponenten:

1. [Laden Sie Git herunter, und installieren Sie es.](https://git-scm.com/)
1. [Herunterladen und Installieren von PHP 5.6.4 oder höher](http://php.net/downloads.php)
1. [Herunterladen und Installieren von Composer](https://getcomposer.org/doc/00-intro.md)
1. Aktivieren der folgenden PHP-Erweiterungen für Laravel: OpenSSL, PDO-MySQL, Mbstring, Tokenizer, XML
1. [Herunterladen, Installieren und Starten von MySQL](https://dev.mysql.com/doc/refman/5.7/en/installing.html) 
1. [Herunterladen und Installieren von Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prepare-local-mysql"></a>Vorbereiten der lokalen MySQL-Instanz

In diesem Schritt erstellen Sie eine Datenbank auf einem lokalen MySQL-Server für die Verwendung in diesem Tutorial.

### <a name="connect-to-mysql-server"></a>Herstellen einer Verbindung mit dem MySQL-Server
Stellen Sie in einem Terminalfenster eine Verbindung mit Ihrem lokalen MySQL-Server her.

```bash
mysql -u root -p
```

Wenn Sie zur Eingabe eines Kennworts aufgefordert werden, geben Sie das Kennwort für das `root`-Konto ein. Wenn Sie das Kennwort für Ihr Root-Konto vergessen haben, lesen Sie unter [MySQL: How to Reset the Root Password](https://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html) (MySQL: Zurücksetzen des Root-Kennworts) nach.

Wenn der Befehl erfolgreich ausgeführt wurde, wird der MySQL-Server bereits ausgeführt. Stellen Sie andernfalls anhand der [Nachinstallationsschritte für MySQL](https://dev.mysql.com/doc/refman/5.7/en/postinstallation.html) sicher, dass Ihr lokaler MySQL-Server gestartet wurde.

### <a name="create-a-database"></a>Erstellen einer Datenbank

Erstellen Sie in der Eingabeaufforderung `mysql` eine Datenbank.

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

Öffnen Sie das Terminalfenster, und wechseln Sie mit `cd` in ein Arbeitsverzeichnis.  

Führen Sie die folgenden Befehle aus, um das Beispielrepository zu klonen. 

```bash
git clone https://github.com/Azure-Samples/laravel-tasks
```

Wechseln Sie mit `cd` in das geklonte Verzeichnis, und installieren Sie die erforderlichen Pakete.

```bash
cd laravel-tasks
composer install
```

### <a name="configure-mysql-connection"></a>Konfigurieren der MySQL-Verbindung

Erstellen Sie im Stammverzeichnis des Repositorys eine Datei _.env_, und kopieren Sie die folgenden Variablen in diese. Ersetzen Sie den Platzhalter _&lt;root_password>_ durch das Kennwort des Root-Benutzers.

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

> [!NOTE]
> Informationen dazu, wie Laravel diese Datei _.env_ verwendet, finden Sie unter [Laravel Environment Configuration](https://laravel.com/docs/5.4/configuration#environment-configuration) (Konfigurieren der Laravel-Umgebung).
>
>

### <a name="run-the-sample"></a>Ausführen des Beispiels

Führen Sie [Laravel-Datenbankmigrationen](https://laravel.com/docs/5.4/migrations) durch, um die von Ihrer Anwendung benötigten Tabellen zu erstellen. Im Verzeichnis _database/migration_ im Git-Repository können Sie sehen, welche Tabellen bei den Migrationen erstellt werden.

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

Sie können PHP jederzeit beenden, indem Sie im Terminal `Ctrl`+`C` eingeben. 

## <a name="create-production-mysql-in-azure"></a>Erstellen der MySQL-Produktionsinstanz in Azure

In diesem Schritt erstellen Sie eine MySQL-Datenbank in [Azure-Datenbank für MySQL (Vorschau)](/azure/mysql). Später konfigurieren Sie Ihre PHP-Anwendung für eine Verbindung mit dieser Datenbank.

### <a name="log-in-to-azure"></a>Anmelden an Azure

Als Nächstes werden mit Azure CLI 2.0 in einem Terminalfenster die Ressourcen erstellt, die zum Hosten der PHP-Anwendung in Azure App Service benötigt werden. Melden Sie sich mit dem Befehl [az login](/cli/azure/#login) bei Ihrem Azure-Abonnement an, und befolgen Sie die Anweisungen auf dem Bildschirm. 

```azurecli 
az login 
``` 

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie mit dem Befehl [az group create](/cli/azure/group#create) eine [Ressourcengruppe](../azure-resource-manager/resource-group-overview.md). Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen wie Web-Apps, Datenbanken und Speicherkonten bereitgestellt und verwaltet werden. 

Im folgenden Beispiel wird eine Ressourcengruppe in der Region „Europa, Norden“ erstellt:

```azurecli
az group create --name myResourceGroup --location "North Europe"
```

Welche Werte Sie für `--location` verwenden können, erfahren Sie mithilfe des Befehls [az appservice list-locations](/cli/azure/appservice#list-locations).

### <a name="create-a-mysql-server"></a>Erstellen eines MySQL-Servers

Erstellen Sie mit dem Befehl [az mysql server create](/cli/azure/mysql/server#create) einen Server in Azure-Datenbank für MySQL (Vorschau).

Ersetzen Sie im folgenden Befehl den Platzhalter _&lt;mysql_server_name>_ durch Ihren eigenen eindeutigen MySQL-Servernamen. Dieser Name ist Teil des Hostnamens des MySQL-Servers `<mysql_server_name>.database.windows.net` und muss daher global eindeutig sein. Ersetzen Sie auf gleiche Weise _&lt;admin_user>_ und _&lt;admin_password>_ durch Ihre eigenen Werte.

```azurecli
az mysql server create \
    --name <mysql_server_name> \
    --resource-group myResourceGroup \
    --location "North Europe" \
    --user <admin_user> \
    --password <admin_password>
```

Nach dem Erstellen des MySQL-Servers zeigt die Azure-Befehlszeilenschnittstelle Informationen wie im folgenden Beispiel an:

```json
{
  "administratorLogin": "<admin_user>",
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

```azurecli
az mysql server firewall-rule create \
    --name allIPs \
    --server <mysql_server_name> \
    --resource-group myResourceGroup \
    --start-ip-address 0.0.0.0 \
    --end-ip-address 255.255.255.255
```

> [!NOTE]
> Azure-Datenbank für MySQL (Vorschau) ermöglicht bisher keine Verbindungen ausschließlich von Azure-Diensten. Da IP-Adressen in Azure dynamisch zugewiesen werden, ist es besser, zunächst alle IP-Adressen zu aktivieren. Da sich der Dienst in der Vorschauphase befindet, werden bessere Methoden zum Schützen der Datenbank in Kürze aktiviert.
>
>

### <a name="connect-to-production-mysql-server"></a>Verbinden mit dem MySQL-Produktionsserver

Stellen Sie im Terminalfenster eine Verbindung mit dem MySQL-Server in Azure her. Verwenden Sie die Werte, die Sie zuvor für _&lt;admin_user>_ und _&lt;mysql_server_name>_ angegeben haben.

```bash
mysql -u <admin_user>@<mysql_server_name> -h <mysql_server_name>.database.windows.net -P 3306 -p
```

### <a name="create-a-production-database"></a>Erstellen einer Produktionsdatenbank

Erstellen Sie in der Eingabeaufforderung `mysql` eine Datenbank.

```sql
CREATE DATABASE sampledb;
```

### <a name="create-a-user-with-permissions"></a>Erstellen eines Benutzers mit Berechtigungen

Erstellen Sie einen Datenbankbenutzer, und weisen Sie ihm alle Berechtigungen in der Datenbank `sampledb` zu. Ersetzen Sie die Platzhalter _&lt;phpapp_user>_ und _&lt;phpapp_password>_ durch Ihren eigenen eindeutigen App-Namen.

```sql
CREATE USER '<phpapp_user>' IDENTIFIED BY '<phpapp_password>'; 
GRANT ALL PRIVILEGES ON sampledb.* TO '<phpapp_user>';
```

Beenden Sie die Serververbindung durch Eingabe von `quit`.

```sql
quit
```

## <a name="connect-app-to-production-mysql"></a>Verbinden der App mit der MySQL-Produktionsinstanz

In diesem Schritt verbinden Sie Ihre PHP-Anwendung mit der MySQL-Datenbank, die Sie gerade in Azure-Datenbank für MySQL (Vorschau) erstellt haben. 

<a name="devconfig"></a>
### <a name="configure-the-connection"></a>Konfigurieren der Verbindung 

Erstellen Sie im Stammverzeichnis des Repositorys eine Datei _.env.production_, und kopieren Sie die folgenden Variablen in diese. Ersetzen Sie die Platzhalter _&lt;mysql_server_name>_, _&lt;phpapp_user>_ und _&lt;phpapp_password>_.

```
APP_ENV=production
APP_DEBUG=true
APP_KEY=SomeRandomString

DB_CONNECTION=mysql
DB_HOST=<mysql_server_name>.database.windows.net
DB_DATABASE=sampledb
DB_USERNAME=<phpapp_user>@<mysql_server_name>
DB_PASSWORD=<phpapp_password>
```

Speichern Sie die Änderungen.

### <a name="test-the-application"></a>Testen der Anwendung

Führen Sie die Laravel-Datenbankmigrationen mit der Datei _.env.production_ als Umgebungsdatei aus, um die Tabellen in der MySQL-Datenbank in Azure-Datenbank für MySQL (Vorschau) zu erstellen.

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

Navigieren Sie in einem Browser zu `http://localhost:8000`. Wenn die Seite ohne Fehler geladen wird, ist die PHP-Anwendung mit der MySQL-Datenbank in Azure verbunden. 

Fügen Sie auf der Seite einige Aufgaben hinzu.

![Erfolgreiche Verbindung zwischen PHP und Azure-Datenbank für MySQL (Vorschau)](./media/app-service-web-tutorial-php-mysql/mysql-connect-success.png)

Sie können PHP jederzeit beenden, indem Sie im Terminal `Ctrl`+`C` eingeben. 

### <a name="secure-sensitive-data"></a>Schützen sensibler Daten

Stellen Sie sicher, dass sensible Daten in _.env.production_ nicht in Git committet werden.

Öffnen Sie dazu als root _.gitignore_ aus dem Repository, und fügen Sie den Dateinamen in einer neuen Zeile hinzu:

```
.env.production
```

Speichern Sie die Änderungen zu, und committen Sie die Änderungen in Git.

```bash
git add .gitignore
git commit -m "keep sensitive data out of git"
```

Später erfahren Sie, wie Sie die Umgebungsvariablen in App Service so konfigurieren, dass eine Verbindung mit Ihrer Datenbank in Azure-Datenbank für MySQL (Vorschau) hergestellt wird, sodass Sie keine Datei `.env` in App Service mehr benötigen. 

## <a name="deploy-php-app-to-azure"></a>Bereitstellen der PHP-App in Azure
In diesem Schritt stellen Sie die mit MySQL verbundene PHP-Anwendung in Azure App Service bereit.

### <a name="create-an-app-service-plan"></a>Wie erstelle ich einen Plan?

Erstellen Sie mit dem Befehl [az appservice plan create](/cli/azure/appservice/plan#create) einen App Service-Plan. 

> [!NOTE] 
> Ein App Service-Plan stellt die Sammlung physischer Ressourcen dar, die zum Hosten Ihrer Apps verwendet werden. Alle einem App Service-Plan zugewiesenen Anwendungen teilen sich die durch den Plan definierten Ressourcen. Das spart Kosten, wenn Sie mehrere Apps hosten. 
> 
> In App Service-Plänen wird Folgendes definiert: 
> 
> * Region („Europa, Norden“, „USA, Osten“, „Asien, Südosten“) 
> * Instanzgröße (klein, mittel, groß) 
> * Skalierung (Instanzenanzahl) 
> * SKU (Free, Shared, Basic, Standard, Premium) 
> 

Im folgenden Beispiel wird der App Service-Plan _myAppServicePlan_ mit dem Tarif **FREE** erstellt:

```azurecli
az appservice plan create \
    --name myAppServicePlan \
    --resource-group myResourceGroup \
    --sku FREE
```

Nach dem Erstellen des App Service-Plans zeigt die Azure-CLI Informationen wie im folgenden Beispiel an:

```json 
{ 
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "North Europe",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/0000-0000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan",
  "kind": "app",
  "location": "North Europe",
  "maximumNumberOfWorkers": 1,
  "name": "myAppServicePlan",
  ...
  < Output has been truncated for readability >
} 
``` 

### <a name="create-a-web-app"></a>Erstellen einer Web-App

Nachdem Sie nun einen App Service-Plan erstellt haben, können Sie innerhalb des App Service-Plans _myAppServicePlan_ eine Web-App erstellen. Die Web-App bietet Ihnen eine Hostingumgebung zum Bereitstellen Ihres Codes sowie eine URL, unter der Sie sich die bereitgestellte Anwendung ansehen können. Erstellen Sie die Web-App mit dem Befehl [az appservice web create](/cli/azure/appservice/web#create). 

Ersetzen Sie im folgenden Befehl den Platzhalter _&lt;appname>_ durch den eindeutigen Namen Ihrer App. Da dieser eindeutige Name als Teil des Standarddomänennamens für die Web-App verwendet wird, muss er für alle Apps in Azure eindeutig sein. Später können Sie der Web-App einen beliebigen benutzerdefinierten DNS-Eintrag zuordnen, bevor Sie sie für Ihre Benutzer verfügbar machen. 

```azurecli
az appservice web create \
    --name <app_name> \
    --resource-group myResourceGroup \
    --plan myAppServicePlan
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

### <a name="set-the-php-version"></a>Festlegen der PHP-Version

Sie legen die von der Anwendung benötigte PHP-Version mit dem Befehl [az appservice web config update](/cli/azure/appservice/web/config#update) fest.

Der folgende Befehl legt die PHP-Version auf _7.0_ fest.

```azurecli
az appservice web config update \
    --name <app_name> \
    --resource-group myResourceGroup \
    --php-version 7.0
```

### <a name="configure-database-settings"></a>Konfigurieren der Datenbankeinstellungen

Wie bereits erwähnt, können Sie mithilfe von Umgebungsvariablen in App Service eine Verbindung mit der Azure-MySQL-Datenbank herstellen.

In App Service werden Umgebungsvariablen als _App-Einstellungen_ mithilfe des Befehls [az appservice web config appsettings update](/cli/azure/appservice/web/config/appsettings#update) festgelegt. 

Mit dem folgenden Befehl können Sie die App-Einstellungen `DB_HOST`, `DB_DATABASE`, `DB_USERNAME` und `DB_PASSWORD` konfigurieren. Ersetzen Sie die Platzhalter _&lt;appname>_, _&lt;mysql_server_name>_, _&lt;phpapp_user>_ und _&lt;phpapp_password>_.

```azurecli
az appservice web config appsettings update \
    --name <app_name> \
    --resource-group myResourceGroup \
    --settings DB_HOST="<mysql_server_name>.database.windows.net" DB_DATABASE="sampledb" DB_USERNAME="<phpapp_user>@<mysql_server_name>" DB_PASSWORD="<phpapp_password>"
```

Sie können die PHP-Methode [getenv()](http://www.php.net/manual/function.getenv.php) verwenden, um auf die Einstellungen zuzugreifen. Im Laravel-Code wird ein [env()](https://laravel.com/docs/5.4/helpers#method-env)-Wrapper für die PHP-Methode `getenv()` verwendet. Die MySQL-Konfiguration in _config/database.php_ könnte beispielsweise wie folgt aussehen:

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

Laravel benötigt wie auf einem lokalen Computer einen Anwendungsschlüssel in App Service. Sie können auch diesen mit App-Einstellungen konfigurieren.

Verwenden Sie `php artisan`, um einen neuen Anwendungsschlüssel zu generieren, ohne ihn in der Datei _.env_ zu speichern.

```bash
php artisan key:generate --show
```

Legen Sie den Anwendungsschlüssel in Ihrer App Service-Web-App mit dem Befehl [az appservice web config appsettings update](/cli/azure/appservice/web/config/appsettings#update) fest. Ersetzen Sie die Platzhalter _&lt;appname>_ und _&lt;outputofphpartisankey:generate>_.

```azurecli
az appservice web config appsettings update \
    --name <app_name> \
    --resource-group myResourceGroup \
    --settings APP_KEY="<output_of_php_artisan_key:generate>" APP_DEBUG="true"
```

> [!NOTE]
> `APP_DEBUG="true"` weist Laravel an, Debuginformationen zurückzugeben, wenn die bereitgestellte Web-App Fehler feststellt. Bei der Ausführung einer Produktionsanwendung sollten Sie für mehr Sicherheit stattdessen `false` festlegen.
>
>

### <a name="set-the-virtual-application-path"></a>Festlegen des virtuellen Anwendungspfads

Legen Sie den virtuellen Anwendungspfad für Ihre Web-App fest. Dieser Schritt ist nur erforderlich, da der [Lebenszyklus der Laravel-Anwendung](https://laravel.com/docs/5.4/lifecycle) im _öffentlichen_ Verzeichnis anstatt im Stammverzeichnis Ihrer Anwendung beginnt. Andere PHP-Frameworks, deren Lebenszyklus im Stammverzeichnis startet, können ohne manuelle Konfiguration des virtuellen Anwendungspfads ausgeführt werden.

Sie legen den virtuellen Anwendungspfad mit dem Befehl [az resource update](/cli/azure/resource#update) fest. Ersetzen Sie den Platzhalter _&lt;appname>_.

```bash
az resource update \
    --name web \
    --resource-group myResourceGroup \
    --namespace Microsoft.Web \
    --resource-type config \
    --parent sites/<app_name> \
    --set properties.virtualApplications[0].physicalPath="site\wwwroot\public" \
    --api-version 2015-06-01
```

> [!NOTE]
> Standardmäßig verweist Azure App Service im Stammverzeichnis des virtuellen Anwendungspfads (_/_) auf das Stammverzeichnis der bereitgestellten Anwendungsdateien (_sites\wwwroot_). 
>
>

### <a name="configure-a-deployment-user"></a>Konfigurieren eines Bereitstellungsbenutzers

Für FTP und lokales Git muss zur Authentifizierung Ihrer Bereitstellung ein Bereitstellungsbenutzer auf dem Server konfiguriert werden. 

> [!NOTE] 
> Ein Bereitstellungsbenutzer wird bei FTP- und lokalen Git-Bereitstellungen für eine Web-App benötigt. Benutzername und Kennwort gelten für die Kontoebene und unterscheiden sich daher von den Anmeldeinformationen für Ihr Azure-Abonnement.

Wenn Sie zuvor bereits einen Benutzernamen und ein Kennwort für die Bereitstellung erstellt haben, können Sie den folgenden Befehl verwenden, um den Benutzernamen anzuzeigen:

```azurecli
az appservice web deployment user show
```

Wenn Sie noch nicht über einen Bereitstellungsbenutzer verfügen, führen Sie den Befehl [az appservice web deployment user set](/cli/azure/appservice/web/deployment/user#set) aus, um Ihre Anmeldeinformationen für die Bereitstellung zu erstellen. 

```azurecli
az appservice web deployment user set \
    --user-name <username> \
    --password <minimum-8-char-capital-lowercase-number>
```

Der Benutzername muss eindeutig und das Kennwort sicher sein. Wenn Sie den Fehler `'Conflict'. Details: 409` erhalten, müssen Sie den Benutzernamen ändern. Wenn Sie den Fehler `'Bad Request'. Details: 400` erhalten, müssen Sie ein sichereres Kennwort verwenden.

Sie müssen diesen Bereitstellungsbenutzer nur einmal erstellen und können ihn dann für alle Azure-Bereitstellungen verwenden.

Notieren Sie sich den Benutzernamen und das Kennwort für die spätere Bereitstellung der Anwendung.

### <a name="configure-local-git-deployment"></a>Konfigurieren der lokalen Git-Bereitstellung 

Bei der Bereitstellung Ihrer Anwendung im Azure App Service haben Sie verschiedene Optionen. Hierzu zählen beispielsweise FTP, lokales Git sowie GitHub, Visual Studio Team Services und Bitbucket. 

Konfigurieren Sie mit dem Befehl [az appservice web source-control config-local-git](/cli/azure/appservice/web/source-control#config-local-git) den lokalen Git-Zugriff auf die Azure-Web-App. 

```azurecli
az appservice web source-control config-local-git \
    --name <app_name> \
    --resource-group myResourceGroup
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

Führen Sie einen Pushvorgang an die Azure-Remoteinstanz durch, um Ihre PHP-Anwendung bereitzustellen. Sie werden zur Angabe des Kennworts aufgefordert, das Sie zuvor bei der Erstellung des Bereitstellungsbenutzers angegeben haben. 

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

**Glückwunsch!** Sie führen eine datengesteuerte PHP-App in Azure App Service aus.

## <a name="update-data-model-and-redeploy"></a>Aktualisieren und erneutes Bereitstellen des Datenmodells

In diesem Schritt nehmen Sie einige Änderungen am `task`Datenmodell vor und veröffentlichen die Änderungen in Azure.

In diesem Szenario möchten Sie Ihre Anwendung ändern, damit Sie eine Aufgabe als abgeschlossen markieren können. 

### <a name="add-a-column"></a>Hinzufügen eines Spaltennamens

Stellen Sie im Terminal sicher, dass Sie sich im Stammverzeichnis des Git-Repositorys befinden, und generieren Sie dann eine neue Datenbankmigration für die Tabelle `tasks`.

```bash
php artisan make:migration add_complete_column --table=tasks
```

Dieser Befehl zeigt den Namen der Migrationsdatei an, die generiert wird. Suchen Sie diese Datei in _database/migrations_, und öffnen Sie sie in einem Text-Editor.

Ersetzen Sie die up()-Methode durch den folgenden Code:

```php
public function up()
{
    Schema::table('tasks', function (Blueprint $table) {
        $table->boolean('complete')->default(False);
    });
}
```

Dieser Code fügt der Tabelle `tasks` die boolesche Spalte `complete` hinzu.

Ersetzen Sie die down()-Methode durch den folgenden Code für die Zurücksetzungsaktion:

```php
public function down()
{
    Schema::table('tasks', function (Blueprint $table) {
        $table->dropColumn('complete');
    });
}
```

Führen Sie im Terminal Laravel-Datenbankmigrationen lokal aus, um die Änderung in der lokalen Datenbank vorzunehmen.

```bash
php artisan migrate
```

Basierend auf der [Laravel-Namenskonvention](https://laravel.com/docs/5.4/eloquent#defining-models) wird das Modell `Task` (siehe _app/Task.php_) standardmäßig der Tabelle `tasks` zugeordnet, sodass Sie mit der Aktualisierung des Datenmodells fertig sind.

### <a name="update-application-logic"></a>Aktualisieren der Anwendungslogik

Öffnen Sie _routes/web.php_. Die Beispielanwendung definiert hier die Routen und Geschäftslogik.

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

Dieser Code führt eine einfache Aktualisierung Ihres Datenmodells durch, indem der Wert von `complete` geändert wird.

### <a name="update-the-view"></a>Aktualisieren der Ansicht

Öffnen Sie _resources/views/tasks.blade.php_. Suchen Sie das öffnende Tag `<tr>`, und ersetzen Sie es durch Folgendes:

```html
<tr class="{{ $task->complete ? 'success' : 'active' }}" >
```

Dies ändert die Zeilenfarbe abhängig davon, ob die Aufgabe abgeschlossen ist.

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

Dieser Code fügt die Schaltfläche zum Übermitteln hinzu, die auf die zuvor definierte Route verweist.

### <a name="test-your-changes-locally"></a>Lokales Testen der Änderungen

Führen Sie den Development Server aus dem Stammverzeichnis Ihres Git-Repositorys erneut aus.

```bash
php artisan serve
```

Navigieren Sie in einem Browser zu `http://localhost:8000`, und klicken Sie auf das Kontrollkästchen, um die Änderung des Aufgabenstatus anzuzeigen.

![Hinzugefügtes Kontrollkästchen in der Aufgabe](./media/app-service-web-tutorial-php-mysql/complete-checkbox.png)

### <a name="publish-changes-to-azure"></a>Veröffentlichen von Änderungen in Azure

Führen Sie im Terminal Laravel-Datenbankmigrationen mit der Produktions-Verbindungszeichenfolge aus, um die Änderung in der Produktionsdatenbank in Azure vorzunehmen.

```bash
php artisan migrate --env=production --force
```

Committen Sie alle Ihre Änderungen in Git, und übertragen Sie die Codeänderungen mithilfe von Push an Azure.

```bash
git add .
git commit -m "added complete checkbox"
git push azure master
```

Wechseln Sie nach Abschluss von `git push` zu Ihrer Azure-Web-App, und testen Sie die neuen Funktionen.

![Auf Azure veröffentlichte Änderungen an Modell und Datenbank](media/app-service-web-tutorial-php-mysql/complete-checkbox-published.png)

> [!NOTE]
> Alle zuvor hinzugefügten Aufgaben werden weiterhin angezeigt. Ihre Änderungen am Datenschema und Ihre vorhandenen Daten bleiben erhalten.
>
>

## <a name="stream-diagnostic-logs"></a>Streaming des Diagnoseprotokolls 

Wenn Ihre PHP-Anwendung in Azure App Service ausgeführt wird, können Sie die Konsolenprotokolle direkt auf Ihr Terminal umleiten. Auf diese Weise erhalten Sie die gleichen Diagnosemeldungen, die Ihnen beim Debuggen von Anwendungsfehlern helfen.

Verwenden Sie zum Starten des Streamings von Protokolldateien den Befehl [az appservice web log tail](/cli/azure/appservice/web/log#tail).

```azurecli 
az appservice web log tail \
    --name <app_name> \
    --resource-group myResourceGroup 
``` 

Nachdem das Protokollstreaming gestartet wurde, aktualisieren Sie die Azure-Web-App im Browser, um Webdatenverkehr zu generieren. Die Konsolenprotokolle sollten jetzt auf Ihr Terminal umgeleitet werden.

Zum Beenden des Protokollstreamings geben Sie `Ctrl`+`C` ein. 

> [!TIP]
> Eine PHP-Anwendung kann die Standardmethode [error_log()](http://php.net/manual/function.error-log.php) für die Ausgabe an die Konsole verwenden. Die Beispielanwendung verwendet diesen Ansatz in _app/Http/routes.php_.
>
> Als ein Webframework verwendet [Laravel Monolog](https://laravel.com/docs/5.4/errors) als Protokollierungsanbieter. Informationen dazu, wie Sie Monolog-Meldungen an die Konsole ausgeben, finden Sie unter [PHP: How to use monolog to log to console (php://out)](http://stackoverflow.com/questions/25787258/php-how-to-use-monolog-to-log-to-console-php-out) (PHP: Verwenden von Monolog für die Konsolenprotokollierung (php://out)).
>
>

## <a name="manage-your-azure-web-app"></a>Verwalten Ihrer Azure-Web-App

Wechseln Sie zum Azure-Portal, um die erstellte Web-App anzuzeigen.

Melden Sie sich hierzu bei [https://portal.azure.com](https://portal.azure.com) an.

Klicken Sie im linken Menü auf **App Service** und anschließend auf den Namen Ihrer Azure-Web-App.

![Portalnavigation zur Azure-Web-App](./media/app-service-web-tutorial-php-mysql/access-portal.png)

Dadurch gelangen Sie auf das _Blatt_ Ihrer Web-App (eine Portalseite, die horizontal geöffnet wird).

Auf dem Blatt Ihrer Web-App wird standardmäßig die Seite **Übersicht** angezeigt. Diese Seite bietet einen Überblick über den Status Ihrer App. Hier können Sie auch einfache Verwaltungsaufgaben wie Durchsuchen, Beenden, Neustarten und Löschen durchführen. Die Registerkarten auf der linken Seite des Blatts zeigen die verschiedenen Konfigurationsseiten, die Sie öffnen können.

![App Service-Blatt im Azure-Portal](./media/app-service-web-tutorial-php-mysql/web-app-blade.png)

Die Registerkarten auf dem Blatt zeigen die vielen tollen Features, mit denen Sie Ihre Web-App ausstatten können. Ein paar Beispiele:

* Zuordnen eines benutzerdefinierten DNS-Namens
* Binden eines benutzerdefinierten SSL-Zertifikats
* Konfigurieren von Continuous Deployment
* Zentrales und horizontales Hochskalieren
* Hinzufügen einer Benutzerauthentifizierung

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
 
Wenn Sie diese Ressourcen nicht für ein anderes Tutorial benötigen (siehe [Nächste Schritte](#next)), können Sie sie löschen, indem Sie den folgenden Befehl ausführen: 
  
```azurecli 
az group delete --name myResourceGroup 
``` 

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

Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie hierfür einen benutzerdefinierten DNS-Namen zuordnen.

> [!div class="nextstepaction"] 
> [Zuordnen eines vorhandenen benutzerdefinierten DNS-Namens zu Azure-Web-Apps](app-service-web-tutorial-custom-domain.md)

