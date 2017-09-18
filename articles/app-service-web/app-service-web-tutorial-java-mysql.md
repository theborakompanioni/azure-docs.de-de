---
title: Erstellen einer Java- und MySQL-Web-App in Azure
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
ms.topic: tutorial
ms.date: 05/22/2017
ms.author: bbenz
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: 190ca4b228434a7d1b30348011c39a979c22edbd
ms.openlocfilehash: 96f7bda79fd83bffa4ca1d64275a784004547c6d
ms.contentlocale: de-de
ms.lasthandoff: 09/09/2017

---

# <a name="build-a-java-and-mysql-web-app-in-azure"></a>Erstellen einer Java- und MySQL-Web-App in Azure

In diesem Tutorial wird gezeigt, wie Sie eine Java-Web-App in Azure erstellen und mit einer MySQL-Datenbank verbinden. Anschließend besitzen Sie eine [Spring Boot](https://projects.spring.io/spring-boot/)-Anwendung, die Daten in [Azure-Datenbank für MySQL](https://docs.microsoft.com/azure/mysql/overview) speichert und mit [Azure App Service-Web-Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) ausgeführt wird.

![In Azure App Service ausgeführte Java-App](./media/app-service-web-tutorial-java-mysql/appservice-web-app.png)

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer MySQL-Datenbank in Azure
> * Herstellen einer Verbindung zwischen einer Beispiel-App und der Datenbank
> * Bereitstellen der Anwendung in Azure
> * Aktualisieren und erneutes Bereitstellen der App
> * Streamen von Diagnoseprotokollen aus Azure
> * Überwachen der App im Azure-Portal


## <a name="prerequisites"></a>Voraussetzungen

1. [Herunterladen und Installieren von Git](https://git-scm.com/)
1. [Herunterladen und Installieren von Java 7 JDK oder höher](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
1. [Herunterladen, Installieren und Starten von MySQL](https://dev.mysql.com/doc/refman/5.7/en/installing.html) 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für dieses Thema die Azure CLI-Version 2.0 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu. 

## <a name="prepare-local-mysql"></a>Vorbereiten der lokalen MySQL-Instanz 

In diesem Schritt erstellen Sie eine Datenbank auf einem lokalen MySQL-Server für das lokale Testen der App auf Ihrem Computer.

### <a name="connect-to-mysql-server"></a>Herstellen einer Verbindung mit dem MySQL-Server

Stellen Sie in einem Terminalfenster eine Verbindung mit Ihrem lokalen MySQL-Server her. Sie können dieses Terminalfenster verwenden, um alle Befehle dieses Tutorials auszuführen.

```bash
mysql -u root -p
```

Wenn Sie zur Eingabe eines Kennworts aufgefordert werden, geben Sie das Kennwort für das `root`-Konto ein. Wenn Sie das Kennwort für Ihr Root-Konto vergessen haben, lesen Sie unter [MySQL: How to Reset the Root Password](https://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html) (MySQL: Zurücksetzen des Root-Kennworts) nach.

Wenn der Befehl erfolgreich ausgeführt wurde, wird der MySQL-Server bereits ausgeführt. Stellen Sie andernfalls anhand der [Nachinstallationsschritte für MySQL](https://dev.mysql.com/doc/refman/5.7/en/postinstallation.html) sicher, dass Ihr lokaler MySQL-Server gestartet wurde.

### <a name="create-a-database"></a>Erstellen einer Datenbank 

Erstellen Sie über die `mysql`-Eingabeaufforderung eine Datenbank und eine Tabelle für die Aufgaben.

```sql
CREATE DATABASE tododb;
```

Beenden Sie die Serververbindung durch Eingabe von `quit`.

```sql
quit
```

## <a name="create-and-run-the-sample-app"></a>Erstellen und Ausführen der Beispiel-App 

In diesem Schritt klonen Sie die Spring Boot-Beispiel-App, konfigurieren sie für die Verwendung der lokalen MySQL-Datenbank und führen sie auf dem Computer aus. 

### <a name="clone-the-sample"></a>Klonen des Beispiels

Navigieren Sie im Terminalfenster zu einem Arbeitsverzeichnis, und klonen Sie das Beispielrepository. 

```bash
git clone https://github.com/azure-samples/mysql-spring-boot-todo
```

### <a name="configure-the-app-to-use-the-mysql-database"></a>Konfigurieren der App für die Verwendung der MySQL-Datenbank

Aktualisieren Sie `spring.datasource.password` und den Wert in *spring-boot-mysql-todo/src/main/resources/application.properties* mit dem gleichen Stammkennwort, das zum Öffnen der MySQL-Aufforderung verwendet wurde:

```
spring.datasource.password=mysqlpass
```

### <a name="build-and-run-the-sample"></a>Erstellen und Ausführen des Beispiels

Verwenden Sie den im Repository enthaltenen Maven-Wrapper zum Erstellen und Ausführen des Beispiels:

```bash
cd spring-boot-mysql-todo
mvnw package spring-boot:run
```

Öffnen Sie im Browser http://localhost:8080, um das Beispiel in Aktion anzuzeigen. Verwenden Sie beim Hinzufügen von Aufgaben zur Liste die folgenden SQL-Befehle in der MySQL-Aufforderung, um die in MySQL gespeicherten Daten anzuzeigen.

```SQL
use testdb;
select * from todo_item;
```

Beenden Sie die Anwendung, indem Sie im Terminal `Ctrl`+`C` drücken. 

## <a name="create-an-azure-mysql-database"></a>Erstellen einer Azure MySQL-Datenbank

In diesem Schritt erstellen Sie eine Instanz von [Azure-Datenbank für MySQL](../mysql/quickstart-create-mysql-server-database-using-azure-cli.md) mit der [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli). Später im Tutorial konfigurieren Sie die Beispielanwendung für die Verwendung dieser Datenbank.

Erstellen Sie mit Azure CLI 2.0 in einem Terminalfenster die Ressourcen, die zum Hosten der Java-Anwendung in Azure App Service benötigt werden. Melden Sie sich mit dem Befehl [az login](/cli/azure/#login) bei Ihrem Azure-Abonnement an, und befolgen Sie die Anweisungen auf dem Bildschirm. 

```azurecli-interactive 
az login 
```   

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie mit dem Befehl [az group create](/cli/azure/group#create) eine [Ressourcengruppe](../azure-resource-manager/resource-group-overview.md). Eine Azure-Ressourcengruppe ist ein logischer Container, in dem verwandte Ressourcen wie Web-Apps, Datenbanken und Speicherkonten bereitgestellt und verwaltet werden. 

Im folgenden Beispiel wird eine Ressourcengruppe in der Region „Europa, Norden“ erstellt:

```azurecli-interactive
az group create --name myResourceGroup --location "North Europe"
```    

Welche Werte Sie für `--location` verwenden können, erfahren Sie mithilfe des Befehls [az appservice list-locations](/cli/azure/appservice#list-locations).

### <a name="create-a-mysql-server"></a>Erstellen eines MySQL-Servers

Erstellen Sie mit dem Befehl [az mysql server create](/cli/azure/mysql/server#create) einen Server in Azure-Datenbank für MySQL (Vorschau).    
Ersetzen Sie den Platzhalter `<mysql_server_name>` durch Ihren eigenen eindeutigen MySQL-Servernamen. Dieser Name ist Teil des Hostnamens des MySQL-Servers `<mysql_server_name>.mysql.database.azure.com` und muss daher global eindeutig sein. Ersetzen Sie auch `<admin_user>` und `<admin_password>` durch Ihre eigenen Werte.

```azurecli-interactive
az mysql server create --name <mysql_server_name> \ 
    --resource-group myResourceGroup \ 
    --location "North Europe" \
    --admin-user <admin_user> \ 
    --admin-password <admin_password>
```

Nach dem Erstellen des MySQL-Servers zeigt die Azure-Befehlszeilenschnittstelle Informationen wie im folgenden Beispiel an:

```json
{
  "administratorLogin": "admin_user",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "mysql_server_name.mysql.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforMySQL/servers/mysql_server_name",
  "location": "northeurope",
  "name": "mysql_server_name",
  "resourceGroup": "mysqlJavaResourceGroup",
  ...
  < Output has been truncated for readability >
}
```

### <a name="configure-server-firewall"></a>Konfigurieren der Serverfirewall

Erstellen Sie mit dem Befehl [az mysql server firewall-rule create](/cli/azure/mysql/server/firewall-rule#create) eine Firewallregel für Ihren MySQL-Server, um Clientverbindungen zuzulassen. 

```azurecli-interactive
az mysql server firewall-rule create \
    --name allIPs \
    --server <mysql_server_name>  \ 
    --resource-group myResourceGroup \ 
    --start-ip-address 0.0.0.0 \ 
    --end-ip-address 255.255.255.255
```

> [!NOTE]
> Azure-Datenbank für MySQL (Vorschauversion) ermöglicht derzeit keine automatischen Verbindungen von Azure-Diensten. Da IP-Adressen in Azure dynamisch zugewiesen werden, ist es besser, zunächst alle IP-Adressen zu aktivieren. Da sich der Dienst weiterhin in der Vorschauphase befindet, werden bessere Methoden zum Schützen der Datenbank aktiviert.

## <a name="configure-the-azure-mysql-database"></a>Konfigurieren der Azure MySQL-Datenbank

Stellen Sie im Terminalfenster auf Ihrem Computer eine Verbindung mit dem MySQL-Server in Azure her. Verwenden Sie den zuvor angegebenen Wert für `<admin_user>` und `<mysql_server_name>`.

```bash
mysql -u <admin_user>@<mysql_server_name> -h <mysql_server_name>.mysql.database.azure.com -P 3306 -p
```

### <a name="create-a-database"></a>Erstellen einer Datenbank 

Erstellen Sie über die `mysql`-Eingabeaufforderung eine Datenbank und eine Tabelle für die Aufgaben.

```sql
CREATE DATABASE tododb;
```

### <a name="create-a-user-with-permissions"></a>Erstellen eines Benutzers mit Berechtigungen

Erstellen Sie einen Datenbankbenutzer, und weisen Sie ihm alle Berechtigungen in der Datenbank `tododb` zu. Ersetzen Sie die Platzhalter `<Javaapp_user>` und `<Javaapp_password>` durch Ihren eigenen eindeutigen App-Namen.

```sql
CREATE USER '<Javaapp_user>' IDENTIFIED BY '<Javaapp_password>'; 
GRANT ALL PRIVILEGES ON tododb.* TO '<Javaapp_user>';
```

Beenden Sie die Serververbindung durch Eingabe von `quit`.

```sql
quit
```

## <a name="deploy-the-sample-to-azure-app-service"></a>Bereitstellen der App in Azure App Service

Erstellen Sie mit dem CLI-Befehl [az appservice plan create](/cli/azure/appservice/plan#create) einen Azure App Service-Plan mit dem Tarif **FREE**. Der App Service-Plan definiert die physischen Ressourcen, die zum Hosten Ihrer Apps verwendet werden. Alle einem App Service-Plan zugewiesenen Anwendungen teilen sich diese Ressourcen. Das spart Kosten, wenn Sie mehrere Apps hosten. 

```azurecli-interactive
az appservice plan create \
    --name myAppServicePlan \ 
    --resource-group myResourceGroup \
    --sku FREE
```

Wenn der Plan fertig ist, zeigt die Azure CLI eine ähnliche Ausgabe wie im folgenden Beispiel:

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

### <a name="create-an-azure-web-app"></a>Erstellen einer Azure-Web-App

 Erstellen Sie mit dem CLI-Befehl [az webapp create](/cli/azure/appservice/web#create) eine Web-App-Definition im App Service-Plan `myAppServicePlan`. Die Web-App-Definition enthält eine URL für den Zugriff auf Ihre Anwendung und konfiguriert verschiedene Optionen zum Bereitstellen Ihres Codes in Azure. 

```azurecli-interactive
az webapp create \
    --name <app_name> \ 
    --resource-group myResourceGroup \
    --plan myAppServicePlan
```

Ersetzen Sie den Platzhalter `<app_name>` durch Ihren eigenen eindeutigen App-Namen. Dieser eindeutige Name ist Teil des Standarddomänennamens für die Web-App. Daher muss er für alle Apps in Azure eindeutig sein. Sie können der Web-App einen benutzerdefinierten Domänennameneintrag zuordnen, bevor Sie sie für Ihre Benutzer verfügbar machen.

Wenn die Web-App-Definition fertig ist, zeigt die Azure CLI Informationen wie im folgenden Beispiel an: 

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

### <a name="configure-java"></a>Konfigurieren von Java 

Richten Sie mit dem Befehl [az appservice web config update](/cli/azure/appservice/web/config#update) die für Ihre App erforderliche Java-Laufzeitkonfiguration ein.

Der folgende Befehl konfiguriert die Web-App für die Ausführung mit einer aktuellen Java 8 JDK-Version und [Apache Tomcat](http://tomcat.apache.org/) 8.0.

```azurecli-interactive
az webapp config set \ 
    --name <app_name> \
    --resource-group myResourceGroup \ 
    --java-version 1.8 \ 
    --java-container Tomcat \
    --java-container-version 8.0
```

### <a name="configure-the-app-to-use-the-azure-sql-database"></a>Konfigurieren der App für die Verwendung der Azure SQL-Datenbank

Legen Sie vor dem Ausführen der Beispiel-App Anwendungseinstellungen für die Web-App fest, um die in Azure erstellte Azure MySQL-Datenbank zu verwenden. Diese Eigenschaften werden für die Webanwendung als Umgebungsvariablen verfügbar gemacht und setzen die in der verpackten Web-App in „application.properties“ festgelegten Werte außer Kraft. 

Legen Sie mithilfe von [az webapp config appsettings](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings) in der CLI Anwendungseinstellungen fest:

```azurecli-interactive
az webapp config appsettings set \
    --settings SPRING_DATASOURCE_URL="jdbc:mysql://<mysql_server_name>.mysql.database.azure.com:3306/tododb?verifyServerCertificate=true&useSSL=true&requireSSL=false" \
    --resource-group myResourceGroup \
    --name <app_name>
```

```azurecli-interactive
az webapp config appsettings set \
    --settings SPRING_DATASOURCE_USERNAME=Javaapp_user@mysql_server_name  \
    --resource-group myResourceGroup \ 
    --name <app_name>
```

```azurecli-interactive
az webapp config appsettings set \
    --settings SPRING_DATASOURCE_URL=Javaapp_password \
    --resource-group myResourceGroup \ 
    --name <app_name>
```

### <a name="get-ftp-deployment-credentials"></a>Abrufen von FTP-Anmeldeinformationen für die Bereitstellung 
Bei der Bereitstellung Ihrer Anwendung in Azure App Service haben Sie verschiedene Optionen. Hierzu zählen beispielsweise FTP, lokales Git sowie GitHub, Visual Studio Team Services und Bitbucket. Verwenden Sie in diesem Beispiel FTP, um die zuvor auf dem lokalen Computer erstellte WAR-Datei in Azure App Service bereitzustellen.

Um zu bestimmen, welche Anmeldeinformationen in einem FTP-Befehl an die Web-App übergeben werden müssen, verwenden Sie den Befehl [az appservice web deployment list-publishing-profiles](https://docs.microsoft.com/cli/azure/appservice/web/deployment#az_appservice_web_deployment_list_publishing_profiles): 

```azurecli-interactive
az webapp deployment list-publishing-profiles \ 
    --name <app_name> \ 
    --resource-group myResourceGroup \
    --query "[?publishMethod=='FTP'].{URL:publishUrl, Username:userName,Password:userPWD}" \ 
    --output json
```

```JSON
[
  {
    "Password": "aBcDeFgHiJkLmNoPqRsTuVwXyZ",
    "URL": "ftp://waws-prod-blu-069.ftp.azurewebsites.windows.net/site/wwwroot",
    "Username": "app_name\\$app_name"
  }
]
```

### <a name="upload-the-app-using-ftp"></a>Hochladen der App mithilfe von FTP

Verwenden Sie Ihr bevorzugtes FTP-Tool zum Bereitstellen der WAR-Datei im Ordner */site/wwwroot/webapps* unter der Serveradresse aus dem Feld `URL` im vorherigen Befehl. Entfernen Sie das vorhandene Anwendungsstandardverzeichnis (ROOT), und ersetzen Sie die vorhandene Datei „ROOT.war“ durch die weiter oben im Tutorial erstellte WAR-Datei.

```bash
ftp waws-prod-blu-069.ftp.azurewebsites.windows.net
Connected to waws-prod-blu-069.drip.azurewebsites.windows.net.
220 Microsoft FTP Service
Name (waws-prod-blu-069.ftp.azurewebsites.windows.net:raisa): app_name\$app_name
331 Password required
Password:
cd /site/wwwroot/webapps
mdelete -i ROOT/*
rmdir ROOT/
put target/TodoDemo-0.0.1-SNAPSHOT.war ROOT.war
```

### <a name="test-the-web-app"></a>Testen der Web-App

Browsen Sie zu `http://<app_name>.azurewebsites.net/`, und fügen Sie der Liste einige Aufgaben hinzu. 

![In Azure App Service ausgeführte Java-App](./media/app-service-web-tutorial-java-mysql/appservice-web-app.png)

**Glückwunsch!** Sie führen eine datengesteuerte Java-App in Azure App Service aus.

## <a name="update-the-app-and-redeploy"></a>Aktualisieren der App und erneutes Bereitstellen

Aktualisieren Sie die Anwendung so, dass sie in der Aufgabenliste eine zusätzliche Spalte für das Erstellungsdatum eines Elements enthält. Spring Boot übernimmt die Aktualisierung des Datenbankschemas für Sie, wenn sich das Datenmodell ändert. Bereits vorhandene Datenbankeinträge bleiben dabei unverändert.

1. Öffnen Sie auf dem lokalen System *src/main/java/com/example/fabrikam/TodoItem.java*, und fügen Sie der Klasse die folgenden Importe hinzu:   

    ```java
    import java.text.SimpleDateFormat;
    import java.util.Calendar;
    ```

2. Fügen Sie *src/main/java/com/example/fabrikam/TodoItem.java* die `String`-Eigenschaft `timeCreated` hinzu, und initialisieren Sie die Datei mit einem Zeitstempel bei Objekterstellung. Fügen Sie beim Bearbeiten dieser Datei Getter/Setter für die neue `timeCreated`-Eigenschaft hinzu.

    ```java
    private String name;
    private boolean complete;
    private String timeCreated;
    ...

    public TodoItem(String category, String name) {
       this.category = category;
       this.name = name;
       this.complete = false;
       this.timeCreated = new SimpleDateFormat("MMMM dd, YYYY").format(Calendar.getInstance().getTime());
    }
    ...
    public void setTimeCreated(String timeCreated) {
       this.timeCreated = timeCreated;
    }

    public String getTimeCreated() {
        return timeCreated;
    }
    ```

3. Aktualisieren Sie *src/main/java/com/example/fabrikam/TodoDemoController.java* mit einer Zeile in der `updateTodo`-Methode, um den Zeitstempel festzulegen:

    ```java
    item.setComplete(requestItem.isComplete());
    item.setId(requestItem.getId());
    item.setTimeCreated(requestItem.getTimeCreated());
    repository.save(item);
    ```

4. Fügen Sie Unterstützung für das neue Feld in der Thymeleaf-Vorlage hinzu. Aktualisieren Sie *src/main/resources/templates/index.html* mit einer neuen Tabellenüberschrift für den Zeitstempel und einem neuen Feld, um den Wert des Zeitstempels in jeder Tabellendatenzeile anzuzeigen.

    ```html
    <th>Name</th>
    <th>Category</th>
    <th>Time Created</th>
    <th>Complete</th>
    ...
    <td th:text="${item.category}">item_category</td><input type="hidden" th:field="*{todoList[__${i.index}__].category}"/>
    <td th:text="${item.timeCreated}">item_time_created</td><input type="hidden" th:field="*{todoList[__${i.index}__].timeCreated}"/>
    <td><input type="checkbox" th:checked="${item.complete} == true" th:field="*{todoList[__${i.index}__].complete}"/></td>
    ```

5. Erstellen Sie die Anwendung neu:

    ```bash
    mvnw clean package 
    ```

6. Verwenden Sie wie zuvor FTP für die aktualisierte WAR-Datei. Entfernen Sie dabei das vorhandene Verzeichnis *site/wwwroot/webapps/ROOT* und die Datei *ROOT.war*, und laden Sie die aktualisierte WAR-Datei dann als „ROOT.war“ hoch. 

Wenn Sie die App aktualisieren, wird nun die Spalte **Zeitpunkt der Erstellung** angezeigt. Wenn Sie eine neue Aufgabe hinzufügen, füllt die App den Zeitstempel automatisch auf. Ihre vorhandenen Aufgaben bleiben unverändert und können mit der App verwendet werden, auch wenn sich das zugrunde liegende Datenmodell geändert hat. 

![Mit neuer Spalte aktualisierte Java-App](./media/app-service-web-tutorial-java-mysql/appservice-updates-java.png)
      
## <a name="stream-diagnostic-logs"></a>Streaming des Diagnoseprotokolls 

Wenn Ihre Java-Anwendung in Azure App Service ausgeführt wird, können Sie die Konsolenprotokolle direkt auf Ihr Terminal umleiten. Auf diese Weise erhalten Sie die gleichen Diagnosemeldungen, die Ihnen beim Debuggen von Anwendungsfehlern helfen.

Verwenden Sie zum Starten des Streamings von Protokolldateien den Befehl [az webapp log tail](/cli/azure/appservice/web/log#tail).

```azurecli-interactive 
az webapp log tail \
    --name <app_name> \
    --resource-group myResourceGroup 
``` 

## <a name="manage-your-azure-web-app"></a>Verwalten Ihrer Azure-Web-App

Wechseln Sie zum Azure-Portal, um die erstellte Web-App anzuzeigen.

Melden Sie sich hierzu bei [https://portal.azure.com](https://portal.azure.com) an.

Klicken Sie im linken Menü auf **App Service** und anschließend auf den Namen Ihrer Azure-Web-App.

![Portalnavigation zur Azure-Web-App](./media/app-service-web-tutorial-java-mysql/access-portal.png)

Auf dem Blatt Ihrer Web-App wird standardmäßig die Seite **Übersicht** angezeigt. Diese Seite bietet einen Überblick über den Status Ihrer App. Hier können Sie auch Verwaltungsaufgaben wie Beenden, Starten, Neustarten und Löschen durchführen. Die Registerkarten auf der linken Seite des Blatts zeigen die verschiedenen Konfigurationsseiten, die Sie öffnen können.

![App Service-Blatt im Azure-Portal](./media/app-service-web-tutorial-java-mysql/web-app-blade.png)

Die Registerkarten auf dem Blatt zeigen die vielen tollen Features, mit denen Sie Ihre Web-App ausstatten können. Ein paar Beispiele:
* Zuordnen eines benutzerdefinierten DNS-Namens
* Binden eines benutzerdefinierten SSL-Zertifikats
* Konfigurieren von Continuous Deployment
* Zentrales und horizontales Hochskalieren
* Hinzufügen einer Benutzerauthentifizierung

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie diese Ressourcen nicht für ein anderes Tutorial benötigen (siehe [Nächste Schritte](#next)), können Sie sie löschen, indem Sie den folgenden Befehl ausführen: 
  
```azurecli-interactive
az group delete --name myResourceGroup 
``` 

<a name="next"></a>

## <a name="next-steps"></a>Nächste Schritte

> [!div class="checklist"]
> * Erstellen einer MySQL-Datenbank in Azure
> * Herstellen einer Verbindung zwischen einer Java-Beispiel-App und MySQL
> * Bereitstellen der Anwendung in Azure
> * Aktualisieren und erneutes Bereitstellen der App
> * Streamen von Diagnoseprotokollen aus Azure
> * Verwalten der App im Azure-Portal

Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie der App einen benutzerdefinierten DNS-Namen zuordnen.

> [!div class="nextstepaction"] 
> [Zuordnen eines vorhandenen benutzerdefinierten DNS-Namens zu Azure-Web-Apps](app-service-web-tutorial-custom-domain.md)
