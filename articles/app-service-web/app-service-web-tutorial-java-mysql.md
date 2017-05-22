---
title: Erstellen einer Java- und MySQL-Web-App in Azure | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine Java-App erstellen, die eine Verbindung mit dem Azure-MySQL-Datenbankdienst in Azure App Service herstellt.
services: app-service\web
documentationcenter: Java
author: bbenz
manager: jeffsand
editor: jasonwhowell
ms.assetid: 
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 05/06/2017
ms.author: bbenz
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: fab7759154b38b8043a17c933f896d7af9514c85
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017

---
# <a name="build-a-java-and-mysql-web-app-in-azure"></a>Erstellen einer Java- und MySQL-Web-App in Azure
In diesem Tutorial wird gezeigt, wie Sie eine Java-Web-App in Azure erstellen und mit einer MySQL-Datenbank verbinden. Im ersten Schritt klonen Sie eine Anwendung auf Ihrem lokalen Computer und führen sie mit einer lokalen Instanz von MySQL aus.
Im nächsten Schritt richten Sie Azure-Dienste für die Java-App und MySQL ein und stellen dann die Anwendung in Azure App Service bereit.
Wenn Sie fertig sind, verfügen Sie über eine To-do-Listen-Anwendung, die in Azure ausgeführt wird und über eine Verbindung mit dem Azure-MySQL-Datenbankdienst verfügt.

![In Azure App Service ausgeführte Java-App](./media/app-service-web-tutorial-java-mysql/appservice-web-app.png)

## <a name="before-you-begin"></a>Voraussetzungen

Installieren Sie vor der Ausführung dieses Beispiels zunächst die folgenden erforderlichen Komponenten:

1. [Laden Sie Git herunter, und installieren Sie es.](https://git-scm.com/)
1. [Herunterladen und Installieren von Java 7 oder höher](http://Java.net/downloads.Java)
1. [Herunterladen und Installieren von Maven](https://maven.apache.org/download.cgi)
1. [Herunterladen, Installieren und Starten von MySQL](https://dev.mysql.com/doc/refman/5.7/en/installing.html) 
1. [Herunterladen und Installieren von Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prepare-local-mysql-database"></a>Vorbereiten der lokalen MySQL-Datenbank

In diesem Schritt erstellen Sie eine Datenbank auf einem lokalen MySQL-Server für die Verwendung in diesem Tutorial.

### <a name="connect-to-mysql-server"></a>Herstellen einer Verbindung mit dem MySQL-Server
Stellen Sie über die Befehlszeile eine Verbindung mit Ihrem lokalen MySQL-Server her:

```bash
mysql -u root -p
```

Wenn der Befehl erfolgreich ausgeführt wurde, wird der MySQL-Server bereits ausgeführt. Stellen Sie andernfalls anhand der [Nachinstallationsschritte für MySQL](https://dev.mysql.com/doc/refman/5.7/en/postinstallation.html) sicher, dass Ihr lokaler MySQL-Server gestartet wurde.

Wenn Sie zur Eingabe eines Kennworts aufgefordert werden, geben Sie das Kennwort für das `root`-Konto ein. Wenn Sie das Kennwort für Ihr Root-Konto vergessen haben, lesen Sie unter [MySQL: How to Reset the Root Password](https://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html) (MySQL: Zurücksetzen des Root-Kennworts) nach.


### <a name="create-a-database-and-table"></a>Erstellen einer Datenbank und Tabelle

Erstellen Sie über die `mysql`-Eingabeaufforderung eine Datenbank und eine Tabelle für die Aufgaben.

```sql
CREATE DATABASE todoItemDb;
USE todoItemDb;
CREATE TABLE ITEMS ( id varchar(255), name varchar(255), category varchar(255), complete bool);
```

Beenden Sie die Serververbindung durch Eingabe von `quit`.

```sql
quit
```

## <a name="create-local-java-application"></a>Erstellen einer lokalen Java-Anwendung
In diesem Schritt klonen Sie ein GitHub-Repository, konfigurieren die Verbindung mit der MySQL-Datenbank und führen die App lokal aus. 

### <a name="clone-the-sample"></a>Klonen des Beispiels

Navigieren Sie in der Befehlszeile zu einem Arbeitsverzeichnis.  

Führen Sie die folgenden Befehle aus, um das Beispielrepository zu klonen. 

```bash
git clone https://github.com/bbenz/azure-mysql-java-todo-app
```

Richten Sie als Nächstes „lombok.jar“ gemäß den Schritten in der Infodatei zum Repository ein.


### <a name="configure-mysql-connection"></a>Konfigurieren der MySQL-Verbindung

Diese Anwendung verwendet das Maven Jetty-Plug-In, um die Anwendung lokal auszuführen und eine Verbindung mit der MySQL-Datenbank herzustellen.
Um den Zugriff auf die lokale Instanz von MySQL zu aktivieren, legen Sie Ihre lokale MySQL-Benutzer-ID und Ihr Kennwort in „WebContent/WEB-INF/jetty-env.xml“ fest.

Aktualisieren Sie die Werte für Benutzer und Kennwort mit der Benutzer-ID und dem Kennwort der lokalen MySQL-Instanz:

```
<Configure id='wac' class="org.eclipse.jetty.webapp.WebAppContext">
  <New id="itemdb" class="org.eclipse.jetty.plus.jndi.Resource">
     <Arg></Arg>
     <Arg>jdbc/todoItemDb</Arg>
     <Arg>
        <New class="com.mysql.jdbc.jdbc2.optional.MysqlConnectionPoolDataSource">
           <Set name="Url">jdbc:mysql://localhost:3306/itemdb</Set>
           <Set name="User">root</Set>
           <Set name="Password"></Set>
        </New>
     </Arg>
    </New>
</Configure>

```

&gt; [!NOTE]
&gt; Informationen dazu, wie Jetty die Datei `jetty-env.xml` verwendet, finden Sie in der [Jetty-XML-Referenz](http://www.eclipse.org/jetty/documentation/9.4.x/jetty-env-xml.html).
&gt; &gt;

### <a name="run-the-sample"></a>Ausführen des Beispiels

Verwenden Sie einen Maven-Befehl, um das Beispiel auszuführen: 

```bash
mvn package jetty:run
```

Navigieren Sie anschließend in einem Browser zu `http://localhost:8080`. Fügen Sie auf der Seite einige Aufgaben hinzu.

Um die Anwendung zu beenden, können Sie jederzeit `Ctrl`+`C` an der Eingabeaufforderung eingeben. 

## <a name="create-a-mysql-database-in-azure"></a>Erstellen einer MySQL-Datenbank in Azure

In diesem Schritt erstellen Sie eine MySQL-Datenbank in [Azure-Datenbank für MySQL (Vorschau)](/azure/mysql). Später konfigurieren Sie Ihre Java-Anwendung für eine Verbindung mit dieser Datenbank.

### <a name="log-in-to-azure"></a>Anmelden an Azure

Erstellen Sie mit Azure CLI 2.0 in einem Terminalfenster die Ressourcen, die zum Hosten der Java-Anwendung in Azure App Service benötigt werden. Melden Sie sich mit dem Befehl [az login](/cli/azure/#login) bei Ihrem Azure-Abonnement an, und befolgen Sie die Anweisungen auf dem Bildschirm. 

```azurecli 
az login 
``` 

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie mit dem Befehl [az group create](/cli/azure/group#create) eine [Ressourcengruppe](../azure-resource-manager/resource-group-overview.md). Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen wie Web-Apps, Datenbanken und Speicherkonten bereitgestellt und verwaltet werden. 

Im folgenden Beispiel wird eine Ressourcengruppe in der Region „Europa, Norden“ erstellt:

```azurecli
az group create --name myResourceGroup  --location "North Europe"
```

Die verfügbaren Werte für `--location` erfahren Sie mithilfe des Befehls [az appservice list-locations](/cli/azure/appservice#list-locations).

### <a name="create-a-mysql-server"></a>Erstellen eines MySQL-Servers

Erstellen Sie mit dem Befehl [az mysql server create](/cli/azure/mysql/server#create) einen Server in Azure-Datenbank für MySQL (Vorschau).

Ersetzen Sie den Platzhalter `&lt;mysql_server_name&gt;` durch Ihren eigenen eindeutigen MySQL-Servernamen. Dieser Name ist Teil des Hostnamens des MySQL-Servers `&lt;mysql_server_name&gt;.database.windows.net` und muss daher global eindeutig sein. Ersetzen Sie auch `&lt;admin_user&gt;` und `&lt;admin_password&gt;` durch Ihre eigenen Werte.

```azurecli
az mysql server create --name &lt;mysql_server_name&gt; --resource-group myResourceGroup --location "North Europe" --user &lt;admin_user&gt; --password &lt;admin_password&gt;
```

Nach dem Erstellen des MySQL-Servers zeigt die Azure-Befehlszeilenschnittstelle Informationen wie im folgenden Beispiel an:

```json
{
  "administratorLogin": "&lt;admin_user&gt;",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "&lt;mysql_server_name&gt;.database.windows.net",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforMySQL/servers/&lt;mysql_server_name&gt;",
  "location": "northeurope",
  "name": "&lt;mysql_server_name&gt;",
  "resourceGroup": "myResourceGroup",
  ...
}
```

### <a name="configure-a-server-firewall"></a>Konfigurieren einer Serverfirewall

Erstellen Sie mit dem Befehl [az mysql server firewall-rule create](/cli/azure/mysql/server/firewall-rule#create) eine Firewallregel für Ihren MySQL-Server, um Clientverbindungen zuzulassen. 

```azurecli
az mysql server firewall-rule create --name allIPs --server &lt;mysql_server_name&gt; --resource-group myResourceGroup --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

&gt; [!NOTE]
&gt; Azure-Datenbank für MySQL (Vorschau) ermöglicht bisher keine Verbindungen ausschließlich von Azure-Diensten. Da IP-Adressen in Azure dynamisch zugewiesen werden, ist es besser, zunächst alle IP-Adressen zu aktivieren. Da sich der Dienst in der Vorschauphase befindet, werden bessere Methoden zum Schützen der Datenbank in Kürze aktiviert.
&gt; &gt;

### <a name="connect-to-the-mysql-server"></a>Herstellen einer Verbindung mit dem MySQL-Server

Stellen Sie im Terminalfenster eine Verbindung mit dem MySQL-Server in Azure her. Verwenden Sie den zuvor angegebenen Wert für `&lt;admin_user&gt;` und `&lt;mysql_server_name&gt;`.

```bash
mysql -u &lt;admin_user&gt;@&lt;mysql_server_name&gt; -h &lt;mysql_server_name&gt;.database.windows.net -P 3306 -p
```

### <a name="create-a-database-and-table-in-the-azure-mysql-service"></a>Erstellen einer Datenbank und einer Tabelle im Azure-MySQL-Dienst

Erstellen Sie über die `mysql`-Eingabeaufforderung eine Datenbank und eine Tabelle für die Aufgaben.

```sql
CREATE DATABASE todoItemDb;
USE todoItemDb;
CREATE TABLE ITEMS ( id varchar(255), name varchar(255), category varchar(255), complete bool);
```

### <a name="create-a-user-with-permissions"></a>Erstellen eines Benutzers mit Berechtigungen

Erstellen Sie einen Datenbankbenutzer, und weisen Sie ihm alle Berechtigungen in der Datenbank `todoItemDb` zu. Ersetzen Sie die Platzhalter `&lt;Javaapp_user&gt;` und `&lt;Javaapp_password&gt;` durch Ihren eigenen eindeutigen App-Namen.

```sql
CREATE USER '&lt;Javaapp_user&gt;' IDENTIFIED BY '&lt;Javaapp_password&gt;'; 
GRANT ALL PRIVILEGES ON todoItemDb.* TO '&lt;Javaapp_user&gt;';
```

Beenden Sie die Serververbindung durch Eingabe von `quit`.

```sql
quit
```

### <a name="configure-the-local-mysql-connection-with-the-new-azure-mysql-service"></a>Konfigurieren der lokalen MySQL-Verbindung mit dem neuen Azure-MySQL-Dienst

In diesem Schritt verbinden Sie Ihre Java-Anwendung mit der MySQL-Datenbank, die Sie in Azure-Datenbank für MySQL (Vorschau) erstellt haben. 

Um den Zugriff von der lokalen Anwendung auf den Azure-MySQL-Dienst zu aktivieren, legen Sie den neuen MySQL-Endpunkt, die Benutzer-ID und das Kennwort in „WebContent/WEB-INF/jetty-env.xml“ fest:

```
<Configure id='wac' class="org.eclipse.jetty.webapp.WebAppContext">
  <New id="itemdb" class="org.eclipse.jetty.plus.jndi.Resource">
     <Arg></Arg>
     <Arg>jdbc/todoItemDb</Arg>
     <Arg>
        <New class="com.mysql.jdbc.jdbc2.optional.MysqlConnectionPoolDataSource">
           <Set name="Url">jdbc:mysql:<paste the endpoint for the Azure MySQL Service>/itemdb</Set>
           <Set name="User">Azure MySQL userID</Set>
           <Set name="Password">Azure MySQL Password</Set>
        </New>
     </Arg>
    </New>
</Configure>

```

Speichern Sie die Änderungen.

## <a name="test-the-application"></a>Testen der Anwendung

Verwenden Sie denselben Maven-Befehl wie zuvor, um das Beispiel erneut lokal auszuführen, aber stellen Sie dieses Mal eine Verbindung mit dem Azure-MySQL-Dienst her: 

```bash
mvn package jetty:run
```

Navigieren Sie in einem Browser zu `http://localhost:8080`. Wenn die Seite ohne Fehler geladen wird, ist die Java-Anwendung mit der MySQL-Datenbank in Azure verbunden. 

Es sollten noch keine Aufgaben auf der Seite hinzugefügt worden sein.

Um die Anwendung zu beenden, können Sie jederzeit `Ctrl`+`C` im Terminal eingeben. 

### <a name="secure-sensitive-data"></a>Schützen sensibler Daten

Stellen Sie sicher, dass sensible Daten in `WebContent/WEB-INF/jetty-env.xml` nicht in Git committet werden.

Öffnen Sie dazu als root `.gitignore` aus dem Repository, und fügen Sie `WebContent/WEB-INF/jetty-env.xml` in einer neuen Zeile hinzu. Speichern Sie die Änderungen.

Committen Sie die Änderungen in `.gitignore`.

```bash
git add .gitignore
git commit -m "keep sensitive data in WebContent/WEB-INF/jetty-env.xml out of git"
```

## <a name="deploy-the-java-application-to-azure"></a>Bereitstellen der Java-Anwendung in Azure
Als Nächstes stellen wir die Java-Anwendung in Azure App Service bereit.

### <a name="create-an-appservice-plan"></a>Erstellen eines App Service-Plans

Erstellen Sie mit dem Befehl [az appservice plan create](/cli/azure/appservice/plan#create) einen App Service-Plan. 

&gt; [!NOTE] 
&gt; Ein App Service-Plan stellt die Sammlung physischer Ressourcen dar, die zum Hosten Ihrer Apps verwendet werden. Alle einem App Service-Plan zugewiesenen Anwendungen teilen sich die durch den Plan definierten Ressourcen. Das spart Kosten, wenn Sie mehrere Apps hosten. 
&gt; &gt; App Service-Pläne definieren Folgendes: &gt; &gt; * Region („Europa, Norden“, „USA, Osten“, „Asien, Südosten“) &gt; * Instanzgröße (Small, Medium, Large) &gt; * Skalierung (Instanzenanzahl) &gt; * SKU (Free, Shared, Basic, Standard, Premium) &gt; 

Im folgenden Beispiel wird der App Service-Plan `myAppServicePlan` mit dem Tarif **FREE** erstellt:

```azurecli
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku FREE
```

Nach dem Erstellen des App Service-Plans zeigt die Azure-Befehlszeilenschnittstelle Informationen wie im folgenden Beispiel an:

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
  &lt; JSON data removed for brevity. &gt;
  "targetWorkerSizeId": 0,
  "type": "Microsoft.Web/serverfarms",
  "workerTierName": null
} 
``` 

### <a name="create-an-azure-web-app"></a>Erstellen einer Azure-Web-App

Nachdem Sie nun einen App Service-Plan erstellt haben, können Sie innerhalb des App Service-Plans `myAppServicePlan` eine Azure-Web-App erstellen. Die Web-App bietet Ihnen eine Hostingumgebung zum Bereitstellen Ihres Codes sowie eine URL, unter der Sie sich die bereitgestellte Anwendung ansehen können. Erstellen Sie die Web-App mit dem Befehl [az appservice web create](/cli/azure/appservice/web#create). 

Ersetzen Sie im folgenden Befehl den Platzhalter `&lt;app_name&gt;` durch den eindeutigen Namen Ihrer App. Da dieser eindeutige Name als Teil des Standarddomänennamens für die Web-App verwendet wird, muss er für alle Apps in Azure eindeutig sein. Später können Sie der Web-App einen beliebigen benutzerdefinierten DNS-Eintrag zuordnen, bevor Sie sie für Ihre Benutzer verfügbar machen. 

```azurecli
az appservice web create --name &lt;app_name&gt; --resource-group myResourceGroup --plan myAppServicePlan
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
  "defaultHostName": "&lt;app_name&gt;.azurewebsites.net",
  "enabled": true,
  "enabledHostNames": [
    "&lt;app_name&gt;.azurewebsites.net",
    "&lt;app_name&gt;.scm.azurewebsites.net"
  ],
  "gatewaySiteName": null,
  "hostNameSslStates": [
    {
      "hostType": "Standard",
      "name": "&lt;app_name&gt;.azurewebsites.net",
      "sslState": "Disabled",
      "thumbprint": null,
      "toUpdate": null,
      "virtualIp": null
    }
    &lt; JSON data removed for brevity. &gt;
}
```

### <a name="set-the-java-version-the-java-application-server-type-and-the-application-server-version"></a>Festlegen von Java-Version, Java-Anwendungsservertyp und Anwendungsserverversion

Sie legen die Java-Version, den Java-App-Server (Container) und die Containerversion mithilfe des Befehls [az appservice web config update](/cli/azure/appservice/web/config#update) fest.

Der folgende Befehl legt die Java-Version auf „8“, den Java-App-Server auf „Jetty“ und die Jetty-Version auf „Newest Jetty 9.3“ fest.

```azurecli
az appservice web config update --name &lt;app_name&gt; --resource-group myResourceGroup --java-version 1.8 --java-container Jetty --java-container-version 9.3
```


### <a name="get-credentials-for-deployment-to-the-web-app-using-ftp"></a>Abrufen von Anmeldeinformationen für die Bereitstellung in der Web-App mithilfe von FTP 

Bei der Bereitstellung Ihrer Anwendung in Azure App Service haben Sie verschiedene Optionen. Hierzu zählen beispielsweise FTP, lokales Git sowie GitHub, Visual Studio Team Services und Bitbucket. In diesem Beispiel verwenden wir Maven, um eine WAR-Datei zu kompilieren, und FTP, um die WAR-Datei in der Web-App bereitzustellen.

Um zu bestimmen, welche Anmeldeinformationen in einem FTP-Befehle an die Web-App übergeben werden müssen, verwenden Sie den Befehl [az appservice web deployment list-publishing-profiles](https://docs.microsoft.com/cli/azure/appservice/web/deployment#list-publishing-profiles) wie folgt: 

```azurecli

az appservice web deployment list-publishing-profiles --name &lt;app_name&gt; --resource-group myResourceGroup --query "[?publishMethod=='FTP'].{URL:publishUrl, Username:userName,Password:userPWD}" --o table

```
### <a name="compile-the-local-application-to-deply-to-the-web-app"></a>Kompilieren der lokalen Anwendung für die Bereitstellung in der Web-App 

Kompilieren Sie alle Ressourcen in der Java-Anwendung erneut in einer einzelnen WAR-Datei, um die lokale Java-Anwendung für die Ausführung in der Azure-Web-App vorzubereiten. Navigieren Sie zu dem Verzeichnis mit der Anwendung „pom.xml“, und geben Sie Folgendes ein:

```bash 
mvn clean package
``` 
Zum Ende des Maven-Paketprozesses wird der Speicherort der WAR-Datei angezeigt.  Die Ausgabe sollte ungefähr wie folgt aussehen:

```bash

[INFO] Processing war project
[INFO] Copying webapp resources [local-location\GitHub\mysql-java-todo-app\WebContent]
[INFO] Webapp assembled in [1519 msecs]
[INFO] Building war: C:\Users\your\localGitHub\mysql-java-todo-app\target\azure-appservice-mysql-java-sample-0.0.1-SNAPSHOT.war
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------

```

Notieren Sie den Speicherort der WAR-Datei, und verwenden Sie Ihr bevorzugtes FTP-Verfahren zum Bereitstellen der WAR-Datei im Web-Apps-Ordner von Jetty.  Beachten Sie, dass sich der Web-Apps-Ordner von Jetty in einer Azure-Web-App unter „/site/wwwroot/webapps“ befindet. 

### <a name="browse-to-the-azure-web-app"></a>Navigieren Sie zur Azure-Web-App.

Browsen Sie zu `http://&lt;app_name&gt;.azurewebsites.net/&lt;app_name&gt;`, und fügen Sie der Liste einige Aufgaben hinzu. 

![In Azure App Service ausgeführte Java-App](./media/app-service-web-tutorial-java-mysql/appservice-web-app.png)


**Glückwunsch!** Sie führen eine datengesteuerte Java-App in Azure App Service aus.
Um die App zu aktualisieren, wiederholen Sie den Maven-Befehl zur Paketbereinigung, und stellen Sie die App per FTP erneut bereit.

## <a name="manage-your-azure-web-app"></a>Verwalten Ihrer Azure-Web-App

Wechseln Sie unter [https://portal.azure.com](https://portal.azure.com) zum Azure-Portal, um die Web-App anzuzeigen, die Sie erstellt haben.

Klicken Sie im linken Menü auf **App Service** und anschließend auf den Namen Ihrer Azure-Web-App.

Dadurch gelangen Sie auf das _Blatt_ Ihrer Web-App (eine Portalseite, die horizontal geöffnet wird).

Auf dem Blatt Ihrer Web-App wird standardmäßig die Seite **Übersicht** angezeigt. Diese Seite bietet einen Überblick über den Status Ihrer App. Hier können Sie auch einfache Verwaltungsaufgaben wie Durchsuchen, Beenden, Neustarten und Löschen durchführen. Die Registerkarten auf der linken Seite des Blatts zeigen die verschiedenen Konfigurationsseiten, die Sie öffnen können.

Gehen Sie auf der Seite **Anwendungseinstellungen** folgendermaßen vor: 

![Azure App Service-Web-Apps-Anwendungseinstellungen](./media/app-service-web-tutorial-java-mysql/appservice-web-app-application-settings.png)



Die Registerkarten auf dem Blatt zeigen die vielen tollen Features, mit denen Sie Ihre Web-App ausstatten können. Ein paar Beispiele:

* Zuordnen eines benutzerdefinierten DNS-Namens
* Binden eines benutzerdefinierten SSL-Zertifikats
* Konfigurieren von Continuous Deployment
* Zentrales und horizontales Hochskalieren
* Hinzufügen einer Benutzerauthentifizierung

## <a name="more-resources"></a>Weitere Ressourcen

- [Zuordnen eines vorhandenen benutzerdefinierten DNS-Namens zu Azure-Web-Apps](app-service-web-tutorial-custom-domain.md)
- [Binden eines vorhandenen benutzerdefinierten SSL-Zertifikats an Azure-Web-Apps](app-service-web-tutorial-custom-ssl.md)
- [Web-Apps-CLI-Skripts](app-service-cli-samples.md)</PRE>

