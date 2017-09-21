---
title: "Erstellen einer .NET Core- und SQL-Datenbank-Web-App in Azure-Web-Apps für Container | Microsoft-Dokumentation"
description: "Informationen zum Ausführen einer .NET Core-App in Azure-Web-Apps für Container mit Verbindung mit einer SQL-Datenbank."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: syntaxc4
editor: 
ms.assetid: 0b4d7d0e-e984-49a1-a57a-3c0caa955f0e
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 08/31/2017
ms.author: cephalin
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: 12c20264b14a477643a4bbc1469a8d1c0941c6e6
ms.openlocfilehash: 7683856e5ad7bf19b38d51f019e4eef106f1800c
ms.contentlocale: de-de
ms.lasthandoff: 09/07/2017

---
# <a name="build-a-net-core-and-sql-database-web-app-in-azure-web-apps-for-containers"></a>Erstellen einer .NET Core- und SQL-Datenbank-Web-App in Azure-Web-Apps für Container

[Web-Apps für Container](app-service-linux-intro.md) bieten einen hochgradig skalierbaren Webhostingdienst mit Self-Patching unter dem Linux-Betriebssystem. In diesem Tutorial wird gezeigt, wie Sie eine .NET Core-Web-App erstellen und mit einer SQL-Datenbank verbinden. Wenn Sie fertig sind, besitzen Sie eine .NET Core-MVC-App, die in Web-Apps für Container ausgeführt wird. 

![In Web-Apps für Container ausgeführte App](./media/tutorial-dotnetcore-sqldb-app/azure-app-in-browser.png)

Folgendes wird vermittelt:

> [!div class="checklist"]
> * Erstellen einer SQL-Datenbank in Azure
> * Herstellen einer Verbindung einer .NET Core-App mit SQL-Datenbank
> * Bereitstellen der Anwendung in Azure
> * Aktualisieren des Datenmodells und erneutes Bereitstellen der App
> * Streamen von Diagnoseprotokollen aus Azure
> * Verwalten der App im Azure-Portal

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

1. [Installation von Git](https://git-scm.com/)
1. [Installieren von .NET Core SDK 1.1.2](https://github.com/dotnet/core/blob/master/release-notes/download-archives/1.1.2-download.md)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="create-local-net-core-app"></a>Erstellen einer lokalen .NET Core-App

In diesem Schritt richten Sie das lokale .NET Core-Projekt ein.

### <a name="clone-the-sample-application"></a>Klonen der Beispielanwendung

Wechseln Sie im Terminalfenster mit `cd` in ein Arbeitsverzeichnis.  

Führen Sie die folgenden Befehle aus, um das Beispielrepository zu klonen und in seinen Stamm zu wechseln.

```bash
git clone https://github.com/azure-samples/dotnetcore-sqldb-tutorial
cd dotnetcore-sqldb-tutorial
```

Das Beispielprojekt enthält eine einfache CRUD-App (create-read-update-delete, erstellen-lesen-aktualisieren-löschen), die auf [Entity Framework Core](https://docs.microsoft.com/en-us/ef/core/) basiert.

### <a name="run-the-application"></a>Ausführen der Anwendung

Führen Sie die folgenden Befehle aus, um die erforderlichen Pakete zu installieren, Datenbankmigrationen auszuführen und die Anwendung zu starten.

```bash
dotnet restore
dotnet ef database update
dotnet run
```

Navigieren Sie in einem Browser zu „http://localhost:5000“. Wählen Sie den Link **Neu erstellen**, und erstellen Sie einige _Aufgaben_-Elemente.

![Erfolgreiche Verbindung mit SQL-Datenbank](./media/tutorial-dotnetcore-sqldb-app/local-app-in-browser.png)

Sie können .NET Core jederzeit beenden, indem Sie im Terminal `Ctrl+C` drücken. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-production-sql-database"></a>Erstellen der SQL-Datenbank für die Produktion

In diesem Schritt erstellen Sie eine SQL-Datenbank in Azure. Wenn Ihre App in Azure bereitgestellt ist, nutzt sie diese Clouddatenbank.

Für SQL-Datenbank wird in diesem Tutorial [Dokumentation zu Azure SQL-Datenbank](/azure/sql-database/) verwendet. 

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-no-h.md)] 

### <a name="create-a-sql-database-logical-server"></a>Erstellen eines logischen SQL-Datenbankservers

Erstellen Sie in der Cloud Shell mit dem Befehl [az sql server create](/cli/azure/sql/server#create) einen logischen SQL-Datenbankserver.

Ersetzen Sie den *\<server_name>*-Platzhalter durch einen eindeutigen Namen für die SQL-Datenbank. Dieser Name wird als Teil des SQL-Datenbank-Endpunkts (`<server_name>.database.windows.net`) verwendet, daher muss er für alle logischen Server in Azure eindeutig sein. Der Name darf nur Kleinbuchstaben, Ziffern und Bindestriche (-) enthalten, und er muss zwischen 3 und 50 Zeichen lang sein. Ersetzen Sie auch *\<db_username>* und *\<db_password>* mit einem Benutzernamen und einem Kennwort Ihrer Wahl. 


```azurecli-interactive
az sql server create --name <server_name> --resource-group myResourceGroup --location "West Europe" --admin-user <db_username> --admin-password <db_password>
```

Nach dem Erstellen des logischen SQL-Datenbankservers zeigt die Azure-Befehlszeilenschnittstelle Informationen wie im folgenden Beispiel an:

```json
{
  "administratorLogin": "sqladmin",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "<server_name>.database.windows.net",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Sql/servers/<server_name>",
  "identity": null,
  "kind": "v12.0",
  "location": "westeurope",
  "name": "<server_name>",
  "resourceGroup": "myResourceGroup",
  "state": "Ready",
  "tags": null,
  "type": "Microsoft.Sql/servers",
  "version": "12.0"
}
```

### <a name="configure-a-server-firewall-rule"></a>Konfigurieren einer Serverfirewallregel

Erstellen Sie mit dem Befehl [az sql server firewall create](/cli/azure/sql/server#create) eine [Azure SQL-Datenbank-Firewallregel auf Serverebene](../../sql-database/sql-database-firewall-configure.md). Wenn sowohl Start- als auch End-IP auf 0.0.0.0 festgelegt ist, wird die Firewall nur für andere Azure-Ressourcen geöffnet. 

```azurecli-interactive
az sql server firewall-rule create --resource-group myResourceGroup --server <server_name> --name AllowYourIp --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
```

### <a name="create-a-database"></a>Erstellen einer Datenbank

Erstellen Sie mit dem Befehl [az sql db create](/cli/azure/sql/db#create) auf dem Server eine Datenbank mit der [Leistungsstufe „S0“](../../sql-database/sql-database-service-tiers.md). 

```azurecli-interactive
az sql db create --resource-group myResourceGroup --server <server_name> --name coreDB --service-objective S0
```

### <a name="create-connection-string"></a>Erstellen der Verbindungszeichenfolge

Ersetzen Sie die folgende Zeichenfolge mit *\<server_name>*, *\<db_username>* und *\<db_password>*, die Sie zuvor verwendet haben.

```
Server=tcp:<server_name>.database.windows.net,1433;Initial Catalog=coreDB;Persist Security Info=False;User ID=<db_username>;Password=<db_password>;MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
```

Dies ist die Verbindungszeichenfolge für Ihre .NET Core-App. Kopieren Sie sie für die spätere Verwendung.

## <a name="deploy-app-to-azure"></a>Bereitstellen von Apps in Azure

In diesem Schritt stellen Sie die mit der SQL-Datenbank verbundene .NET Core-Anwendung für Web-Apps für Container bereit.

### <a name="configure-local-git-deployment"></a>Konfigurieren der lokalen Git-Bereitstellung 

[!INCLUDE [Configure a deployment user](../../../includes/configure-deployment-user-no-h.md)]

### <a name="create-an-app-service-plan"></a>Wie erstelle ich einen Plan?

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux-no-h.md)] 

### <a name="create-a-web-app"></a>Erstellen einer Web-App

[!INCLUDE [Create web app](../../../includes/app-service-web-create-web-app-linux-dotnetcore-no-h.md)] 

### <a name="configure-an-environment-variable"></a>Konfigurieren einer Umgebungsvariablen

Verwenden Sie zum Festlegen von Verbindungszeichenfolgen für Ihre Azure-App den [az webapp config appsettings update](/cli/azure/webapp/config/appsettings#update)-Befehl in der Cloud Shell. Ersetzen Sie im folgenden Befehl sowohl *\<app name>* als auch den *\<connection_string>*-Parameter mit der Verbindungszeichenfolge, die Sie zuvor erstellt haben.

```azurecli-interactive
az webapp config connection-string set --resource-group myResourceGroup --name <app name> --settings MyDbConnection='<connection_string>' --connection-string-type SQLServer
```

Legen Sie als Nächstes für die `ASPNETCORE_ENVIRONMENT`-App-Einstellung _Production_ fest. Diese Einstellung teilt Ihnen mit, ob die Ausführung in Azure stattfindet, da Sie SQLLite für Ihre lokale Entwicklungsumgebung und die SQL-Datenbank für Ihre Azure-Umgebung verwenden. 

Im folgenden Beispiel wird die App-Einstellung `ASPNETCORE_ENVIRONMENT` in der Azure-Web-App konfiguriert. Ersetzen Sie den Platzhalter *\<app_name>*.

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings ASPNETCORE_ENVIRONMENT="Production"
```

### <a name="connect-to-sql-database-in-production"></a>Herstellen der Verbindung mit SQL-Datenbank in der Produktion

Öffnen Sie in Ihrem lokalen Repository „Startup.cs“, und suchen Sie den folgenden Code:

```csharp
services.AddDbContext<MyDatabaseContext>(options =>
        options.UseSqlite("Data Source=localdatabase.db"));
```

Ersetzen sie ihn durch den folgenden Code, der die Umgebungsvariablen verwendet, die Sie zuvor konfiguriert haben.

```csharp
// Use SQL Database if in Azure, otherwise, use SQLLite
if(Environment.GetEnvironmentVariable("ASPNETCORE_ENVIRONMENT") == "Production")
    services.AddDbContext<DotNetCoreSqlDbContext>(options =>
            options.UseSqlServer(Configuration.GetConnectionString("MyDbConnection")));
else
    services.AddDbContext<DotNetCoreSqlDbContext>(options =>
            options.UseSqlite("Data Source=MvcMovie.db"));

// Automatically perform database migration
services.BuildServiceProvider().GetService<DotNetCoreSqlDbContext>().Database.Migrate();
```

Wenn dieser Code erkennt, dass er in der Produktion ausgeführt wird (was die Azure-Umgebung angibt), verwendet er die Verbindungszeichenfolge, die Sie zum Herstellen der Verbindung mit der SQL-Datenbank konfiguriert haben. 

Der `Database.Migration()`-Aufruf hilft Ihnen, wenn er in Azure ausgeführt wird, da er automatisch die Datenbanken erstellt, die Ihre .NET Core-App basierend auf ihrer Migrationskonfiguration benötigt. 

Speichern Sie die Änderungen.

### <a name="push-to-azure-from-git"></a>Übertragen von Git an Azure mithilfe von Push

[!INCLUDE [app-service-plan-no-h](../../../includes/app-service-web-git-push-to-azure-no-h.md)]

```bash
Counting objects: 98, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (92/92), done.
Writing objects: 100% (98/98), 524.98 KiB | 5.58 MiB/s, done.
Total 98 (delta 8), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: .
remote: Updating submodules.
remote: Preparing deployment for commit id '0c497633b8'.
remote: Generating deployment script.
remote: Project file path: ./DotNetCoreSqlDb.csproj
remote: Generated deployment script files
remote: Running deployment command...
remote: Handling ASP.NET Core Web Application deployment.
remote: .
remote: .
remote: .
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
remote: App container will begin restart within 10 seconds.
To https://<app_name>.scm.azurewebsites.net/<app_name>.git
 * [new branch]      master -> master
``` 

### <a name="browse-to-the-azure-web-app"></a>Navigieren Sie zur Azure-Web-App. 

Navigieren Sie in Ihrem Webbrowser zu der bereitgestellten Web-App. 

```bash 
http://<app_name>.azurewebsites.net 
``` 

Fügen Sie einige Aufgaben hinzu.

![In Web-Apps für Container ausgeführte App](./media/tutorial-dotnetcore-sqldb-app/azure-app-in-browser.png)

**Glückwunsch!** Sie führen eine datengesteuerte .NET Core-App in Web-Apps für Container aus.

## <a name="update-locally-and-redeploy"></a>Lokales Aktualisieren und erneutes Bereitstellen

In diesem Schritt nehmen Sie eine Änderung am Datenbankschema vor und veröffentlichen sie in Azure.

### <a name="update-your-data-model"></a>Aktualisieren Sie des Datenmodells

Öffnen Sie _Models\Todo.cs_ im Code-Editor. Fügen Sie der `ToDo`-Klasse die folgende Eigenschaft hinzu:

```csharp
public bool Done { get; set; }
```

### <a name="run-code-first-migrations-locally"></a>Lokales Ausführen von Code First-Migrationen

Führen Sie einige Befehle aus, um Aktualisierungen an Ihrer lokalen Datenbank vorzunehmen. 

```bash
dotnet ef migrations add AddProperty
```

Aktualisieren Sie die lokale Datenbank:

```bash
dotnet ef database update
```

### <a name="use-the-new-property"></a>Verwenden der neuen Eigenschaft

Nehmen Sie einige Änderungen an Ihrem Code vor, um die `Done`-Eigenschaft zu verwenden. Der Einfachheit halber ändern Sie in diesem Lernprogramm nur Ansichten `Index` und `Create`, um die Eigenschaft in Aktion zu sehen.

Öffnen Sie _Controllers\TodosController.cs_.

Suchen Sie die `Create()`-Methode, und fügen Sie `Done` zur Liste der Eigenschaften im `Bind`-Attribut hinzu. Anschließend sieht Ihre `Create()`-Methodensignatur wie im folgenden Code aus:

```csharp
public async Task<IActionResult> Create([Bind("ID,Description,CreatedDate,Done")] Todo todo)
```

Öffnen Sie _Views\Todos\Create.cshtml_.

Im Razor-Code sollten Sie ein `<div class="form-group">`-Element für `Description` sowie ein weiteres `<div class="form-group">`-Element für `CreatedDate` sehen. Fügen Sie direkt nach diesen beiden Elementen ein weiteres `<div class="form-group">`-Element für `Done` hinzu:

```csharp
<div class="form-group">
    <label asp-for="Done" class="col-md-2 control-label"></label>
    <div class="col-md-10">
        <input asp-for="Done" class="form-control" />
        <span asp-validation-for="Done" class="text-danger"></span>
    </div>
</div>
```

Öffnen Sie _Views\Todos\Index.cshtml_.

Suchen Sie nach dem leeren `<th></th>`-Element. Fügen Sie direkt oberhalb dieses Elements den folgenden Razor-Code hinzu:

```csharp
<th>
    @Html.DisplayNameFor(model => model.Done)
</th>
```

Suchen Sie nach dem `<td>`-Element, das die `asp-action`-Taghilfsprogramme enthält. Fügen Sie direkt oberhalb dieses Elements den folgenden Razor-Code hinzu:

```csharp
<td>
    @Html.DisplayFor(modelItem => item.CreatedDate)
</td>
```

Mehr ist nicht erforderlich, um die Änderungen in den Ansichten `Index` und `Create` anzuzeigen. 

### <a name="test-your-changes-locally"></a>Lokales Testen der Änderungen

Führen Sie die App lokal aus.

```
dotnet run
```

Navigieren Sie in Ihrem Browser zu „http://local:5000“. Sie können jetzt eine Aufgabe hinzufügen und die Option **Fertig** aktivieren. Die Aufgabe sollte dann auf der Startseite als erledigt angezeigt werden. Beachten Sie, dass in der Ansicht `Edit` das Feld `Done` nicht angezeigt wird, da Sie die Ansicht `Edit` nicht geändert haben.

### <a name="publish-changes-to-azure"></a>Veröffentlichen von Änderungen in Azure

```bash

git commit -am "added done field"
git push azure master
```

Wechseln Sie nach Abschluss des `git push` zu Ihrer Azure-Web-App, und testen Sie die neuen Funktionen.

![Azure-Web-App nach Code First-Migration](./media/tutorial-dotnetcore-sqldb-app/this-one-is-done.png)

Ihre gesamten vorhandenen Aufgaben werden weiterhin angezeigt. Wenn Sie die .NET Core-App erneut veröffentlichen, gehen in der SQL-Datenbank vorhandene Daten nicht verloren. Außerdem wird durch Entity Framework Core-Migrationen nur das Datenschema geändert, die vorhandenen Daten bleiben unverändert.

## <a name="manage-your-azure-web-app"></a>Verwalten Ihrer Azure-Web-App

Wechseln Sie zum [Azure-Portal](https://portal.azure.com), um die erstellte Web-App anzuzeigen.

Klicken Sie im linken Menü auf **App Services** und anschließend auf den Namen Ihrer Azure-Web-App.

![Portalnavigation zur Azure-Web-App](./media/tutorial-dotnetcore-sqldb-app/access-portal.png)

Standardmäßig wird im Portal die Seite **Übersicht** Ihrer Web-App angezeigt. Diese Seite bietet einen Überblick über den Status Ihrer App. Hier können Sie auch einfache Verwaltungsaufgaben wie Durchsuchen, Beenden, Neustarten und Löschen durchführen. Die Registerkarten links auf der Seite zeigen die verschiedenen Konfigurationsseiten, die Sie öffnen können.

![App Service-Seite im Azure-Portal](./media/tutorial-dotnetcore-sqldb-app/web-app-blade.png)

[!INCLUDE [cli-samples-clean-up](../../../includes/cli-samples-clean-up.md)]

<a name="next"></a>
## <a name="next-steps"></a>Nächste Schritte

Sie haben Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen einer SQL-Datenbank in Azure
> * Herstellen einer Verbindung einer .NET Core-App mit SQL-Datenbank
> * Bereitstellen der Anwendung in Azure
> * Aktualisieren des Datenmodells und erneutes Bereitstellen der App
> * Streamen von Protokollen von Azure auf Ihr Terminal
> * Verwalten der App im Azure-Portal

Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie Ihrer Web-App einen benutzerdefinierten DNS-Namen zuordnen.

> [!div class="nextstepaction"] 
> [Zuordnen eines vorhandenen benutzerdefinierten DNS-Namens zu Azure-Web-Apps](../../app-service-web/app-service-web-tutorial-custom-domain.md)

