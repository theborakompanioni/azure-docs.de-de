---
title: Erstellen einer ASP.NET-App in Azure mit SQL-Datenbank | Microsoft-Dokumentation
description: "Informationen zum Ausführen einer ASP.NET-App in Azure mit Verbindung mit einer SQL-Datenbank."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: 03c584f1-a93c-4e3d-ac1b-c82b50c75d3e
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: csharp
ms.topic: tutorial
ms.date: 06/09/2017
ms.author: cephalin
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: 8021f8641ff3f009104082093143ec8eb087279e
ms.openlocfilehash: ee38401d2a4faa55b9736b1de45e03bde8def5bb
ms.contentlocale: de-de
ms.lasthandoff: 07/21/2017

---

# <a name="build-an-aspnet-app-in-azure-with-sql-database"></a>Erstellen einer ASP.NET-App in Azure mit SQL-Datenbank

[Azure-Web-Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) bietet einen hochgradig skalierbaren Webhosting-Dienst mit Self-Patching. In diesem Tutorial wird veranschaulicht, wie Sie eine datengesteuerte ASP.NET-Web-App in Azure bereitstellen und dafür eine Verbindung mit [Azure SQL-Datenbank](../sql-database/sql-database-technical-overview.md) herstellen. Am Ende dieses Lernprogramms verfügen Sie über eine ASP.NET-App, die in [Azure App Service](../app-service/app-service-value-prop-what-is.md) ausgeführt wird und mit SQL-Datenbank verbunden ist.

![Veröffentlichte ASP.NET-Anwendung in Azure-Web-App](./media/app-service-web-tutorial-dotnet-sqldatabase/azure-app-in-browser.png)

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer SQL-Datenbank in Azure
> * Herstellen einer Verbindung mit SQL-Datenbank für eine ASP.NET-App
> * Bereitstellen der Anwendung in Azure
> * Aktualisieren des Datenmodells und erneutes Bereitstellen der App
> * Streamen von Protokollen von Azure auf Ihr Terminal
> * Verwalten der App im Azure-Portal

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

* Installieren Sie [Visual Studio 2017](https://www.visualstudio.com/downloads/) mit den folgenden Workloads:
  - **ASP.NET und Webentwicklung**
  - **Azure-Entwicklung**

  ![ASP.NET und Webentwicklung und Azure-Entwicklung (unter „Web und Cloud“)](media/app-service-web-tutorial-dotnet-sqldatabase/workloads.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Herunterladen des Beispiels

[Laden Sie das Beispielprojekt](https://github.com/Azure-Samples/dotnet-sqldb-tutorial/archive/master.zip) herunter.

Extrahieren (entzippen) Sie die Datei *dotnet-sqldb-tutorial-master.zip*.

Das Beispielprojekt enthält eine einfache [ASP.NET MVC](https://www.asp.net/mvc)-CRUD-App (create-read-update-delete, erstellen-lesen-aktualisieren-löschen), die auf [Entity Framework Code First](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application) basiert.

### <a name="run-the-app"></a>Ausführen der App

Öffnen Sie die Datei *dotnet-sqldb-tutorial-master/DotNetAppSqlDb.sln* in Visual Studio. 

Geben Sie `Ctrl+F5` ein, um die App ohne Debuggen auszuführen. Die App wird im Standardbrowser angezeigt. Wählen Sie den Link **Neu erstellen**, und erstellen Sie einige *Aufgaben*-Elemente. 

![Dialogfeld "Neues ASP.NET-Projekt"](media/app-service-web-tutorial-dotnet-sqldatabase/local-app-in-browser.png)

Testen Sie die Links **Bearbeiten**, **Details** und **Löschen**.

Die App nutzt einen Datenbankkontext, um die Verbindung mit der Datenbank herzustellen. In diesem Beispiel wird im Datenbankkontext eine Verbindungszeichenfolge mit dem Namen `MyDbConnection` verwendet. Die Verbindungszeichenfolge wird in der Datei *Web.config* festgelegt, und die Datei *Models/MyDatabaseContext.cs* enthält einen Verweis darauf. Der Name der Verbindungszeichenfolge wird später im Tutorial verwendet, um die Azure-Web-App mit einer Azure SQL-Datenbank zu verbinden. 

## <a name="publish-to-azure-with-sql-database"></a>Veröffentlichen in Azure mit SQL-Datenbank

Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **DotNetAppSqlDb**, und wählen Sie **Veröffentlichen** aus.

![Veröffentlichen über den Projektmappen-Explorer](./media/app-service-web-tutorial-dotnet-sqldatabase/solution-explorer-publish.png)

Vergewissern Sie sich, dass **Microsoft Azure App Service** ausgewählt ist, und klicken Sie auf **Veröffentlichen**.

![Veröffentlichen über die Projektübersichtsseite](./media/app-service-web-tutorial-dotnet-sqldatabase/publish-to-app-service.png)

Beim Veröffentlichen wird das Dialogfeld **App Service erstellen** geöffnet, das Sie beim Erstellen sämtlicher Azure-Ressourcen unterstützt, die benötigt werden, um Ihre ASP.NET-Web-App in Azure auszuführen.

### <a name="sign-in-to-azure"></a>Anmelden bei Azure

Klicken Sie im Dialogfeld **App Service erstellen** auf **Konto hinzufügen**, und melden Sie sich bei Ihrem Azure-Abonnement an. Falls Sie bereits bei einem Microsoft-Konto angemeldet sind, vergewissern Sie sich, dass dieses Konto Ihr Azure-Abonnement enthält. Wenn das Microsoft-Konto, bei dem Sie angemeldet sind, nicht Ihr Azure-Abonnement enthält, klicken Sie darauf, um das korrekte Konto hinzuzufügen.
   
![Anmelden bei Azure](./media/app-service-web-tutorial-dotnet-sqldatabase/sign-in-azure.png)

Nach der Anmeldung können Sie in diesem Dialogfeld sämtliche Ressourcen erstellen, die Sie für Ihre Azure-Web-App benötigen.

### <a name="configure-the-web-app-name"></a>Konfigurieren des Web-App-Namens

Sie können den generierten Web-App-Namen beibehalten oder in einen anderen eindeutigen Namen ändern (gültige Zeichen `a-z`, `0-9` und `-`). Der Web-App-Name wird als Teil der Standard-URL für Ihre App verwendet (`<app_name>.azurewebsites.net`, wobei `<app_name>` Ihr Web-App-Name ist). Der Web-App-Name muss in Azure über alle Apps hinweg eindeutig sein. 

![Dialogfeld „App Service erstellen“](media/app-service-web-tutorial-dotnet-sqldatabase/wan.png)

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

[!INCLUDE [resource-group](../../includes/resource-group.md)]

Klicken Sie neben **Ressourcengruppe** auf **Neu**.

![Klicken Sie neben „Ressourcengruppe“ auf „Neu“.](media/app-service-web-tutorial-dotnet-sqldatabase/new_rg2.png)

Geben Sie der Ressourcengruppe den Namen **myResourceGroup**.

> [!NOTE]
> Klicken Sie nicht auf **Erstellen**. Sie müssen in einem späteren Schritt zuerst eine SQL-Datenbank einrichten.

### <a name="create-an-app-service-plan"></a>Wie erstelle ich einen Plan?

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

Klicken Sie neben **App Service-Plan** auf **Neu**. 

Konfigurieren Sie den neuen App Service-Plan im Dialogfeld **App Service-Plan konfigurieren** mit den folgenden Einstellungen:

![App Service-Plan erstellen](./media/app-service-web-tutorial-dotnet-sqldatabase/configure-app-service-plan.png)

| Einstellung  | Empfohlener Wert | Weitere Informationen |
| ----------------- | ------------ | ----|
|**App Service-Plan**| myAppServicePlan | [App Service-Pläne](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) |
|**Location**| Westeuropa | [Azure-Regionen](https://azure.microsoft.com/regions/) |
|**Größe**| Kostenlos | [Tarife](https://azure.microsoft.com/pricing/details/app-service/)|

### <a name="create-a-sql-server-instance"></a>Erstellen einer SQL Server-Instanz

Bevor Sie eine Datenbank erstellen, benötigen Sie einen [logischen Azure SQL-Datenbankserver](../sql-database/sql-database-features.md). Ein logischer Server enthält eine Gruppe von Datenbanken, die als Gruppe verwaltet werden.

Wählen Sie die Option **Weitere Azure Services durchsuchen**.

![Konfigurieren des Web-App-Namens](media/app-service-web-tutorial-dotnet-sqldatabase/web-app-name.png)

Klicken Sie auf der Registerkarte **Dienste** auf das Symbol **+** neben **SQL-Datenbank**. 

![Klicken Sie auf der Registerkarte „Dienste“ auf das Pluszeichen neben SQL-Datenbank.](media/app-service-web-tutorial-dotnet-sqldatabase/sql.png)

Klicken Sie im Dialogfeld **SQL-Datenbank konfigurieren** auf **Neu** neben **SQL Server**. 

Ein eindeutiger Servername wird generiert. Dieser Name wird als Teil der Standard-URL für Ihren logischen Server `<server_name>.database.windows.net` verwendet. Er muss in Azure über alle Instanzen logischer Server hinweg eindeutig sein. Sie können den Servernamen ändern, aber behalten Sie den generierten Wert für dieses Tutorial bei.

Fügen Sie den Benutzernamen und das Kennwort eines Administrators hinzu, und wählen Sie dann **OK**. Informationen zu den Anforderungen an die Komplexität von Kennwörtern finden Sie unter [Kennwortrichtlinie](/sql/relational-databases/security/password-policy).

Merken Sie sich diesen Benutzernamen und das Kennwort. Sie benötigen diese Angaben später zum Verwalten der Instanz des logischen Servers.

![SQL Server-Instanz erstellen](media/app-service-web-tutorial-dotnet-sqldatabase/configure-sql-database-server.png)

### <a name="create-a-sql-database"></a>Erstellen einer SQL-Datenbank

Gehen Sie im Dialogfeld **SQL-Datenbank konfigurieren** wie folgt vor: 

* Behalten Sie den standardmäßig generierten **Datenbanknamen** bei.
* Geben Sie unter **Name der Verbindungszeichenfolge** den Namen *MyDbConnection* ein. Dieser Name muss mit der Verbindungszeichenfolge übereinstimmen, auf die in *Models/MyDatabaseContext.cs* verwiesen wird.
* Klicken Sie auf **OK**.

![SQL-Datenbank konfigurieren](media/app-service-web-tutorial-dotnet-sqldatabase/configure-sql-database.png)

Im Dialogfeld **App Service erstellen** werden die von Ihnen erstellten Ressourcen angezeigt. Klicken Sie auf **Erstellen**. 

![Erstellte Ressourcen](media/app-service-web-tutorial-dotnet-sqldatabase/app_svc_plan_done.png)

Nachdem der Assistent die Erstellung der Azure-Ressourcen abgeschlossen hat, wird Ihre ASP.NET-App in Azure veröffentlicht. Ihr Standardbrowser wird mit der URL für die bereitgestellte App gestartet. 

Fügen Sie einige Aufgaben hinzu.

![Veröffentlichte ASP.NET-Anwendung in Azure-Web-App](./media/app-service-web-tutorial-dotnet-sqldatabase/azure-app-in-browser.png)

Glückwunsch! Ihre datengesteuerte ASP.NET-Anwendung wird live in Azure App Service ausgeführt.

## <a name="access-the-sql-database-locally"></a>Lokaler Zugriff auf die SQL-Datenbank

In Visual Studio können Sie Ihre neue SQL-Datenbank bequem im **SQL Server-Objekt-Explorer** überprüfen und verwalten.

### <a name="create-a-database-connection"></a>Erstellen einer Datenbankverbindung

Wählen Sie im Menü **Ansicht** die Option **SQL Server-Objekt-Explorer**.

Klicken Sie oben im **SQL Server-Objekt-Explorer** auf die Schaltfläche **SQL Server hinzufügen**.

### <a name="configure-the-database-connection"></a>Konfigurieren der Datenbankverbindung

Erweitern Sie im Dialogfeld **Verbinden** den Knoten **Azure**. Hier sind alle Ihre Instanzen von SQL-Datenbank in Azure aufgeführt.

Wählen Sie die SQL-Datenbank `DotNetAppSqlDb`. Die Verbindung, die Sie zuvor erstellt haben, wird automatisch im unteren Bereich angezeigt.

Geben Sie das Administratorkennwort für die Datenbank ein, das Sie zuvor erstellt haben, und klicken Sie auf **Verbinden**.

![Datenbankverbindung in Visual Studio konfigurieren](./media/app-service-web-tutorial-dotnet-sqldatabase/connect-to-sql-database.png)

### <a name="allow-client-connection-from-your-computer"></a>Zulassen der Clientverbindung auf dem Computer

Das Dialogfeld zum Erstellen einer neuen **Firewallregel**wird angezeigt. Standardmäßig lässt die SQL-Datenbankinstanz nur Verbindungen von Azure-Diensten zu, z.B. von der Azure-Web-App. Erstellen Sie zum Herstellen einer Verbindung mit Ihrer Datenbank in der SQL-Datenbankinstanz eine Firewallregel. Mit der Firewallregel wird die öffentliche IP-Adresse Ihres lokalen Computers zugelassen.

Das Dialogfeld ist bereits mit der öffentlichen IP-Adresse des Computers ausgefüllt.

Stellen Sie sicher, dass **Meine IP-Clientadresse hinzufügen** ausgewählt ist, und klicken Sie auf **OK**. 

![Festlegen der Firewall für die SQL-Datenbankinstanz](./media/app-service-web-tutorial-dotnet-sqldatabase/sql-set-firewall.png)

Sobald das Erstellen der Firewalleinstellung in Visual Studio für die SQL-Datenbankinstanz abgeschlossen wurde, wird die Verbindung im **SQL Server-Objekt-Explorer** angezeigt.

Hier können Sie häufige Datenbankvorgänge ausführen, und z.B. Abfragen ausführen, Ansichten und gespeicherte Prozeduren erstellen usw. 

Klicken Sie mit der rechten Maustaste auf die Tabelle `Todoes`, und wählen Sie **Daten anzeigen**. 

![SQL-Datenbank-Objekte ansehen](./media/app-service-web-tutorial-dotnet-sqldatabase/explore-sql-database.png)

## <a name="update-app-with-code-first-migrations"></a>Aktualisieren der App mit Code First-Migrationen

Sie können die vertrauten Tools in Visual Studio verwenden, um Ihre Datenbank und Web-App in Azure zu aktualisieren. In diesem Schritt verwenden Sie Code First-Migrationen in Entity Framework, um eine Änderung am Datenbankschema vorzunehmen und sie in Azure zu veröffentlichen.

Weitere Informationen zur Verwendung von Entity Framework Code First-Migrationen finden Sie unter [Getting Started with Entity Framework 6 Code First using MVC 5](https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application) (Erste Schritte mit Entity Framework 6 Code First per MVC 5).

### <a name="update-your-data-model"></a>Aktualisieren Sie des Datenmodells

Öffnen Sie _Models\Todo.cs_ im Code-Editor. Fügen Sie der `ToDo`-Klasse die folgende Eigenschaft hinzu:

```csharp
public bool Done { get; set; }
```

### <a name="run-code-first-migrations-locally"></a>Lokales Ausführen von Code First-Migrationen

Führen Sie einige Befehle aus, um Aktualisierungen an Ihrer lokalen Datenbank vorzunehmen. 

Klicken Sie im Menü **Extras** auf **NuGet-Paket-Manager** > **Paket-Manager-Konsole**.

Aktivieren Sie im Fenster „Paket-Manager-Konsole“ Code First-Migrationen:

```PowerShell
Enable-Migrations
```

Fügen Sie eine Migration hinzu:

```PowerShell
Add-Migration AddProperty
```

Aktualisieren Sie die lokale Datenbank:

```PowerShell
Update-Database
```

Drücken Sie `Ctrl+F5`, um die App auszuführen. Testen Sie die Links „Bearbeiten“, „Details“ und „Erstellen“.

Wenn die Anwendung ohne Fehler geladen wird, waren die Code First-Migrationen erfolgreich. Allerdings wird die Seite noch genauso angezeigt, weil die Anwendungslogik noch nicht die neue Eigenschaft verwendet. 

### <a name="use-the-new-property"></a>Verwenden der neuen Eigenschaft

Nehmen Sie einige Änderungen an Ihrem Code vor, um die `Done`-Eigenschaft zu verwenden. Der Einfachheit halber ändern Sie in diesem Lernprogramm nur Ansichten `Index` und `Create`, um die Eigenschaft in Aktion zu sehen.

Öffnen Sie _Controllers\TodosController.cs_.

Suchen Sie die `Create()`-Methode, und fügen Sie `Done` zur Liste der Eigenschaften im `Bind`-Attribut hinzu. Anschließend sieht Ihre `Create()`-Methodensignatur wie im folgenden Code aus:

```csharp
public ActionResult Create([Bind(Include = "id,Description,CreatedDate,Done")] Todo todo)
```

Öffnen Sie _Views\Todos\Create.cshtml_.

Im Razor-Code sollten Sie ein `<div class="form-group">`-Element sehen, das `model.Description` verwendet, sowie ein weiteres `<div class="form-group">`-Element, das `model.CreatedDate` verwendet. Fügen Sie direkt nach diesen beiden Elementen ein weiteres `<div class="form-group">`-Element hinzu, für das `model.Done` genutzt wird:

```csharp
<div class="form-group">
    @Html.LabelFor(model => model.Done, htmlAttributes: new { @class = "control-label col-md-2" })
    <div class="col-md-10">
        <div class="checkbox">
            @Html.EditorFor(model => model.Done)
            @Html.ValidationMessageFor(model => model.Done, "", new { @class = "text-danger" })
        </div>
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

Suchen Sie nach dem `<td>`-Element, das die `Html.ActionLink()`-Hilfsmethoden enthält. Fügen Sie direkt oberhalb dieses Elements den folgenden Razor-Code hinzu:

```csharp
<td>
    @Html.DisplayFor(modelItem => item.Done)
</td>
```

Mehr ist nicht erforderlich, um die Änderungen in den Ansichten `Index` und `Create` anzuzeigen. 

Drücken Sie `Ctrl+F5`, um die App auszuführen.

Sie können jetzt eine Aufgabe hinzufügen und die Option **Fertig** aktivieren. Die Aufgabe sollte dann auf der Startseite als erledigt angezeigt werden. Beachten Sie, dass in der Ansicht `Edit` das Feld `Done` nicht angezeigt wird, da Sie die Ansicht `Edit` nicht geändert haben.

### <a name="enable-code-first-migrations-in-azure"></a>Aktivieren von Code First-Migrationen in Azure

Da die Codeänderung, einschließlich der Datenbankmigration, funktioniert, können Sie sie in Ihrer Azure-Web-App veröffentlichen und die SQL-Datenbank ebenfalls mit Code First-Migrationen aktualisieren.

Klicken Sie wie zuvor mit der rechten Maustaste auf Ihr Projekt, und wählen Sie **Veröffentlichen** aus.

Klicken Sie auf **Einstellungen**, um den Veröffentlichungs-Assistenten zu öffnen.

![Veröffentlichungseinstellungen öffnen](./media/app-service-web-tutorial-dotnet-sqldatabase/publish-settings.png)

Klicken Sie im Assistenten auf **Weiter**.

Stellen Sie sicher, dass die Verbindungszeichenfolge für die SQL-Datenbank unter **MyDatabaseContext (MyDbConnection)** angezeigt wird. Möglicherweise müssen Sie die Datenbank **myToDoAppDb** in der Dropdownliste auswählen. 

Wählen Sie **Code First-Migrationen ausführen (wird beim Anwendungsstart ausgeführt)** aus, und klicken Sie auf **Speichern**.

![Code First-Migrationen in der Azure-Web-App aktivieren](./media/app-service-web-tutorial-dotnet-sqldatabase/enable-migrations.png)

### <a name="publish-your-changes"></a>Veröffentlichen der Änderungen

Nachdem Sie Code First-Migrationen in der Azure-Web-App aktiviert haben, können Sie die Änderungen am Code veröffentlichen.

Klicken Sie auf der Veröffentlichungsseite auf **Veröffentlichen**.

Versuchen Sie erneut, Aufgaben hinzuzufügen, und wählen Sie **Fertig** aus. Sie sollten auf der Startseite als abgeschlossene Aufgaben angezeigt werden.

![Azure-Web-App nach Code First-Migration](./media/app-service-web-tutorial-dotnet-sqldatabase/this-one-is-done.png)

Ihre gesamten vorhandenen Aufgaben werden weiterhin angezeigt. Wenn Sie die ASP.NET-Anwendung erneut veröffentlichen, gehen in der SQL-Datenbank vorhandene Daten nicht verloren. Außerdem wird durch Code First-Migrationen nur das Datenschema geändert, die vorhandenen Daten bleiben unverändert.


## <a name="stream-application-logs"></a>Streamen von Anwendungsprotokollen

Sie können Ablaufverfolgungsmeldungen direkt aus der Azure-Web-App in Visual Studio streamen.

Öffnen Sie _Controllers\TodosController.cs_.

Jede Aktion beginnt mit einer `Trace.WriteLine()`-Methode. Dieser Code wird hinzugefügt, um zu zeigen, wie Sie Ihrer Azure-Web-App Ablaufverfolgungsnachrichten hinzufügen können.

### <a name="open-server-explorer"></a>Öffnen des Server-Explorer

Wählen Sie im Menü **Ansicht** die Option **Server-Explorer**. Sie können die Protokollierung für die Azure-Web-App im **Server-Explorer** konfigurieren. 

### <a name="enable-log-streaming"></a>Aktivieren des Protokollstreamings

Erweitern Sie im **Server-Explorer** den Knoten **Azure** > **App Service**.

Erweitern Sie die Ressourcengruppe **myResourceGroup**, die Sie beim Erstellen der Azure-Web-App erstellt haben.

Klicken Sie mit der rechten Maustaste auf Ihre Azure-Web-App, und wählen Sie **Streamingprotokolle anzeigen** aus.

![Aktivieren des Protokollstreamings](./media/app-service-web-tutorial-dotnet-sqldatabase/stream-logs.png)

Die Protokolle werden jetzt in das **Ausgabefenster** gestreamt. 

![Protokollstreaming im Ausgabefenster](./media/app-service-web-tutorial-dotnet-sqldatabase/log-streaming-pane.png)

Es werden allerdings noch keine Ablaufverfolgungsmeldungen angezeigt. Da beim ersten Auswählen von **Streamingprotokolle anzeigen** die Azure-Web-App die Ablaufverfolgungsebene auf `Error` festlegt, womit nur Fehlerereignisse protokolliert werden (mit der `Trace.TraceError()`-Methode).

### <a name="change-trace-levels"></a>Ändern der Ablaufverfolgungsebenen

Wenn Sie die Ablaufverfolgungsebenen ändern möchten, um andere Ablaufverfolgungsmeldungen auszugeben, wechseln Sie zurück zum **Server-Explorer**.

Klicken Sie mit der rechten Maustaste auf Ihre Azure-Web-App, und wählen Sie **Einstellungen** aus.

Wählen Sie in der Dropdownliste **Anwendungsprotokollierung (Dateisystem)** die Option **Ausführlich** aus. Klicken Sie auf **Speichern**.

![Ablaufverfolgungsebene in „Ausführlich“ ändern](./media/app-service-web-tutorial-dotnet-sqldatabase/trace-level-verbose.png)

> [!TIP]
> Sie können mit verschiedenen Ablaufverfolgungsebenen experimentieren, um zu sehen, welche Nachrichten für die jeweiligen Ebenen angezeigt werden. Bei der Ebene **Informationen** werden alle von `Trace.TraceInformation()`, `Trace.TraceWarning()` und `Trace.TraceError()` erstellten Protokolle eingeschlossen, aber keine Protokolle, die von `Trace.WriteLine()` erstellt wurden.
>
>

Klicken Sie in Ihrem Browser auf die Aufgaben-App in Azure. Die Ablaufverfolgungsmeldungen werden jetzt in das **Ausgabefenster**in Visual Studio gestreamt.

```
Application: 2017-04-06T23:30:41  PID[8132] Verbose     GET /Todos/Index
Application: 2017-04-06T23:30:43  PID[8132] Verbose     GET /Todos/Create
Application: 2017-04-06T23:30:53  PID[8132] Verbose     POST /Todos/Create
Application: 2017-04-06T23:30:54  PID[8132] Verbose     GET /Todos/Index
```



### <a name="stop-log-streaming"></a>Beenden des Protokollstreamings

Klicken Sie zum Beenden des Diensts für das Protokollstreaming auf die Schaltfläche **Überwachung beenden** im **Ausgabefenster**.

![Beenden des Protokollstreamings](./media/app-service-web-tutorial-dotnet-sqldatabase/stop-streaming.png)

## <a name="manage-your-azure-web-app"></a>Verwalten Ihrer Azure-Web-App

Wechseln Sie zum [Azure-Portal](https://portal.azure.com), um die erstellte Web-App anzuzeigen. 



Klicken Sie im linken Menü auf **App Service** und anschließend auf den Namen Ihrer Azure-Web-App.

![Portalnavigation zur Azure-Web-App](./media/app-service-web-tutorial-dotnet-sqldatabase/access-portal.png)

Sie befinden sich auf der Seite Ihrer Web-App. 

Standardmäßig wird im Portal die Seite **Übersicht** angezeigt. Diese Seite bietet einen Überblick über den Status Ihrer App. Hier können Sie auch einfache Verwaltungsaufgaben wie Durchsuchen, Beenden, Neustarten und Löschen durchführen. Die Registerkarten links auf der Seite zeigen die verschiedenen Konfigurationsseiten, die Sie öffnen können. 

![App Service-Seite im Azure-Portal](./media/app-service-web-tutorial-dotnet-sqldatabase/web-app-blade.png)

[!INCLUDE [Clean up section](../../includes/clean-up-section-portal-web-app.md)]

<a name="next"></a>

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen einer SQL-Datenbank in Azure
> * Herstellen einer Verbindung mit SQL-Datenbank für eine ASP.NET-App
> * Bereitstellen der Anwendung in Azure
> * Aktualisieren des Datenmodells und erneutes Bereitstellen der App
> * Streamen von Protokollen von Azure auf Ihr Terminal
> * Verwalten der App im Azure-Portal

Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie der Web-App einen benutzerdefinierten DNS-Namen zuordnen.

> [!div class="nextstepaction"]
> [Zuordnen eines vorhandenen benutzerdefinierten DNS-Namens zu Azure-Web-Apps](app-service-web-tutorial-custom-domain.md)

