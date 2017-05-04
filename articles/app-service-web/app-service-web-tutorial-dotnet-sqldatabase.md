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
ms.devlang: nodejs
ms.topic: article
ms.date: 04/07/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: db7cb109a0131beee9beae4958232e1ec5a1d730
ms.openlocfilehash: 566a8d7fc595975e065c5a8a42c7f7c94997821b
ms.lasthandoff: 04/18/2017


---
# <a name="create-an-aspnet-app-in-azure-with-sql-database"></a>Erstellen einer ASP.NET-App in Azure mit SQL-Datenbank

In diesem Lernprogramm wird gezeigt, wie Sie eine datengesteuerte ASP.NET-Web-App in Azure entwickeln, mit Azure SQL-Datenbank verbinden und datengestützte Funktionen aktivieren. Am Ende dieses Lernprogramms haben Sie eine ASP.NET-Anwendung, die in [Azure App Service](../app-service/app-service-value-prop-what-is.md) ausgeführt wird und mit SQL-Datenbank verbunden ist.

![Veröffentlichte ASP.NET-Anwendung in Azure-Web-App](./media/app-service-web-tutorial-dotnet-sqldatabase/azure-app-in-browser.png)

## <a name="before-you-begin"></a>Voraussetzungen

Vor dem Ausführen dieses Beispiels [laden Sie die kostenlose Visual Studio 2017 Community Edition herunter, und installieren Sie sie](https://www.visualstudio.com/downloads/). Aktivieren Sie beim Setup von Visual Studio die Option **Azure-Entwicklung**.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="step-1---download-the-sample"></a>Schritt 1: Herunterladen des Beispiels
In diesem Schritt laden Sie eine ASP.NET-Beispielanwendung herunter.

### <a name="get-the-sample-project"></a>Herunterladen des Beispielprojekts

Laden Sie das Beispielprojekt herunter, indem Sie [hier](https://github.com/Azure-Samples/dotnet-sqldb-tutorial/archive/master.zip) klicken.

Extrahieren Sie die heruntergeladene Datei `dotnet-sqldb-tutorial-master.zip` in ein Arbeitsverzeichnis.

> [!TIP]
> Sie können das gleiche Beispielprojekt auch abrufen, indem Sie das GitHub-Repository klonen:
>
> ```bash
> git clone https://github.com/Azure-Samples/dotnet-sqldb-tutorial.git
> ```
>
>

Dieses Beispielprojekt enthält eine einfache [ASP.NET MVC](https://www.asp.net/mvc)-CRUD-Anwendung (create-read-update-delete, erstellen-lesen-aktualisieren-löschen), die auf [Entity Framework Code First](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application) basiert.

### <a name="run-the-application"></a>Ausführen der Anwendung

Starten Sie aus dem extrahierten Verzeichnis `dotnet-sqldb-tutorial-master\DotNetAppSqlDb.sln` in Visual Studio 2017.

Wenn die Projektmappe geöffnet ist, geben Sie zum Ausführen im Browser `F5` ein.

Auf der Startseite sollte eine einfache Aufgabenliste angezeigt werden. Versuchen Sie, der leeren Liste einige Aufgaben hinzuzufügen.

![Dialogfeld "Neues ASP.NET-Projekt"](./media/app-service-web-tutorial-dotnet-sqldatabase/local-app-in-browser.png)

Der Datenbankkontext verwendet eine Verbindungszeichenfolge namens `MyDbConnection`. Diese Verbindungszeichenfolge wird in `Web.config` definiert, und in `Models\MyDatabaseContext.cs` wird auf sie verwiesen. Der Name der Verbindungszeichenfolge ist alles, was, die Sie später benötigen, wenn Sie Ihre Azure-Web-App mit Azure SQL-Datenbank verbinden. 

## <a name="step-2---publish-to-azure-with-sql-database"></a>Schritt 2: Veröffentlichen in Azure mit SQL-Datenbank

Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **DotNetAppSqlDb**, und wählen Sie **Veröffentlichen** aus.

![Veröffentlichen über den Projektmappen-Explorer](./media/app-service-web-tutorial-dotnet-sqldatabase/solution-explorer-publish.png)

Vergewissern Sie sich, dass **Microsoft Azure App Service** ausgewählt ist, und klicken Sie auf **Veröffentlichen**.

![Veröffentlichen über die Projektübersichtsseite](./media/app-service-web-tutorial-dotnet-sqldatabase/publish-to-app-service.png)

Daraufhin öffnet sich das Dialogfeld **App Service erstellen**, das Sie beim Erstellen sämtlicher Azure-Ressourcen unterstützt, die benötigt werden, um Ihre ASP.NET Web-App in Azure auszuführen.

### <a name="sign-in-to-azure"></a>Anmelden bei Azure

Klicken Sie im Dialogfeld **App Service erstellen** auf **Konto hinzufügen**, und melden Sie sich bei Ihrem Azure-Abonnement an. Falls Sie bereits bei einem Microsoft-Konto angemeldet sind, vergewissern Sie sich, dass dieses Konto Ihr Azure-Abonnement enthält. Wenn das Microsoft-Konto, bei dem Sie angemeldet sind, nicht Ihr Azure-Abonnement enthält, klicken Sie darauf, um das korrekte Konto hinzuzufügen.
   
![Anmelden bei Azure](./media/app-service-web-tutorial-dotnet-sqldatabase/sign-in-azure.png)

Nach der Anmeldung können Sie in diesem Dialogfeld sämtliche Ressourcen erstellen, die Sie für Ihre Azure-Web-App benötigen.

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Als erstes benötigen Sie eine _Ressourcengruppe_. 

> [!NOTE] 
> Eine Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen wie Web-Apps, Datenbanken und Speicherkonten bereitgestellt und verwaltet werden.
>
>

Klicken Sie neben **Ressourcengruppe** auf **Neu**.

Nennen Sie Ihre Ressourcengruppe **myResourceGroup**, und klicken Sie anschließend auf **OK**.

### <a name="create-an-app-service-plan"></a>Wie erstelle ich einen Plan?

Ihre Azure Web-App benötigt außerdem einen _App Service-Plan_. 

> [!NOTE]
> Ein App Service-Plan stellt die Sammlung physischer Ressourcen dar, die zum Hosten Ihrer Apps verwendet werden. Alle einem App Service-Plan zugewiesenen Apps teilen sich die durch den Plan definierten Ressourcen. Das spart Kosten, wenn Sie mehrere Apps hosten. 
>
> In App Service-Plänen wird Folgendes definiert:
>
> - Region („Europa, Norden“, „USA, Osten“, „Asien, Südosten“)
> - Instanzgröße (klein, mittel, groß)
> - Skalierung (Instanzenanzahl) 
> - SKU (Free, Shared, Basic, Standard, Premium)
>
>

Klicken Sie neben **App Service-Plan** auf **Neu**. 

Konfigurieren Sie den neuen App Service-Plan im Dialogfeld **App Service-Plan konfigurieren** mit den folgenden Einstellungen:

- **App Service-Plan**: Geben Sie **myAppServicePlan** ein. 
- **Standort**: Wählen Sie **Europa, Westen** oder eine beliebige andere Region aus.
- **Größe**: Wählen Sie **Free** oder einen beliebigen anderen [Tarif](https://azure.microsoft.com/pricing/details/app-service/) aus.

Klicken Sie auf **OK**.

![App Service-Plan erstellen](./media/app-service-web-tutorial-dotnet-sqldatabase/configure-app-service-plan.png)

### <a name="configure-the-web-app-name"></a>Konfigurieren des Web-App-Namens

Geben Sie unter **Web-App-Name** einen eindeutigen Web-App-Namen ein. Da dieser Name als Teil des DNS-Standardnamens für Ihre App (`<app_name>.azurewebsites.net`) verwendet wird, muss er für alle Apps in Azure eindeutig sein. Sie können Ihrer App später einen benutzerdefinierten Domänennamen zuordnen, bevor Sie sie für Ihre Benutzer verfügbar machen.

Sie können auch den automatisch generierten Namen übernehmen. Dieser ist bereits eindeutig.

Klicken Sie zur Vorbereitung des nächsten Schritts auf **Weitere Azure Services durchsuchen**.

![Konfigurieren des Web-App-Namens](./media/app-service-web-tutorial-dotnet-sqldatabase/web-app-name.png)

### <a name="create-a-sql-server-instance"></a>Erstellen einer SQL Server-Instanz

Klicken Sie auf der Registerkarte **Dienste** auf das Symbol **+** neben **SQL-Datenbank**. 

Klicken Sie im Dialogfeld **SQL-Datenbank konfigurieren** auf **Neu** neben **SQL Server**. 

Geben Sie unter **Servername** einen eindeutigen Namen ein. Da dieser Name als Teil des DNS-Standardnamens für Ihren Datenbankserver (`<server_name>.database.windows.net`) verwendet wird, muss er für alle SQL Server-Instanzen in Azure eindeutig sein. 

Konfigurieren Sie die restlichen Felder, und klicken Sie auf **OK**.

![SQL Server-Instanz erstellen](./media/app-service-web-tutorial-dotnet-sqldatabase/configure-sql-database-server.png)

### <a name="configure-the-sql-database"></a>Konfigurieren der SQL-Datenbank

Geben Sie unter **Datenbankname** `myToDoAppDb` oder einen beliebigen Namen ein.

Geben Sie unter **Name der Verbindungszeichenfolge** den Namen `MyDbConnection` ein. Dieser Name muss mit der Verbindungszeichenfolge übereinstimmen, auf die in `Models\MyDatabaseContext.cs` verwiesen wird.

![SQL-Datenbank konfigurieren](./media/app-service-web-tutorial-dotnet-sqldatabase/configure-sql-database.png)

### <a name="create-and-publish-the-web-app"></a>Erstellen und Veröffentlichen der Web-App

Klicken Sie auf **Erstellen**. 

Nachdem der Assistent die Azure-Ressourcen erstellt hat, veröffentlicht er Ihre ASP.NET-Anwendung erstmals für Azure und startet die veröffentlichte Azure-Web-App anschließend in Ihrem Standardbrowser.

Versuchen Sie, der leeren Liste einige Aufgaben hinzuzufügen.

![Veröffentlichte ASP.NET-Anwendung in Azure-Web-App](./media/app-service-web-tutorial-dotnet-sqldatabase/azure-app-in-browser.png)

Glückwunsch! Ihre datengesteuerte ASP.NET-Anwendung wird live in Azure App Service ausgeführt.

## <a name="step-3---access-the-sql-database-locally"></a>Schritt 3: Lokaler Zugriff auf die SQL-Datenbank

In Visual Studio können Sie Ihre neue SQL-Datenbank bequem im **SQL Server-Objekt-Explorer** überprüfen und verwalten.

### <a name="create-a-database-connection"></a>Erstellen einer Datenbankverbindung

Öffnen Sie den **SQL Server-Objekt-Explorer**, indem Sie `Ctrl`+`\`, `Ctrl`+`S` drücken.

Klicken Sie oben im **SQL Server-Objekt-Explorer** auf die Schaltfläche **SQL Server hinzufügen**.

### <a name="configure-the-database-connection"></a>Konfigurieren der Datenbankverbindung

Erweitern Sie im Dialogfeld **Verbinden** den Knoten **Azure**. Alle SQL-Datenbanken in Azure werden hier aufgelistet.

Wählen Sie die SQL-Datenbank aus, die Sie zuvor erstellt haben. Die Verbindung, die Sie zuvor verwendet haben, wird automatisch im unteren Bereich angezeigt.

Geben Sie das Administratorkennwort für die Datenbank ein, das Sie zuvor verwendet haben, und klicken Sie auf **Verbinden**.

![Datenbankverbindung in Visual Studio konfigurieren](./media/app-service-web-tutorial-dotnet-sqldatabase/connect-to-sql-database.png)

### <a name="allow-client-connection-from-your-computer"></a>Zulassen der Clientverbindung auf dem Computer

Das Dialogfeld zum Erstellen einer neuen Firewallregel**** wird angezeigt. Standardmäßig lässt die SQL Server-Instanz nur Verbindungen von Azure-Diensten zu, z.B. von der Azure-Web-App. Damit Sie die Datenbank direkt in Visual Studio verbinden können, müssen Sie eine Firewallregel in der SQL Server-Instanz erstellen, um die öffentliche IP-Adresse des lokalen Computers zuzulassen.

In Visual Studio ist dies leicht zu bewerkstelligen. Das Dialogfeld ist bereits mit der öffentlichen IP-Adresse des Computers ausgefüllt.

Stellen Sie sicher, dass **Meine IP-Clientadresse hinzufügen** ausgewählt ist, und klicken Sie auf **OK**. 

![Firewall für SQL Server-Instanz einrichten](./media/app-service-web-tutorial-dotnet-sqldatabase/sql-set-firewall.png)

Sobald das Einrichten der Firewall in Visual Studio für die SQL Server-Instanz abgeschlossen wurde, wird die Verbindung im **SQL Server-Objekt-Explorer** angezeigt.

Hier können Sie häufige Datenbankvorgänge ausführen, und z.B. Abfragen ausführen, Ansichten und gespeicherte Prozeduren erstellen usw. Das folgende Beispiel zeigt, wie Sie Datenbankdaten anzeigen. 

![SQL-Datenbank-Objekte ansehen](./media/app-service-web-tutorial-dotnet-sqldatabase/explore-sql-database.png)

## <a name="step-4---update-app-with-code-first-migrations"></a>Schritt 4: Aktualisieren der App mit Code First-Migrationen

In diesem Schritt verwenden Sie Code First-Migrationen in Entity Framework, um eine Änderung am Datenbankschema vorzunehmen und sie in Azure zu veröffentlichen.

### <a name="update-your-data-model"></a>Aktualisieren Sie des Datenmodells

Öffnen Sie `Models\Todo.cs` im Code-Editor. Fügen Sie der `ToDo`-Klasse die folgende Eigenschaft hinzu:

```csharp
public bool Done { get; set; }
```

### <a name="run-code-first-migrations-locally"></a>Lokales Ausführen von Code First-Migrationen

Führen Sie anschließend einige Befehle aus, um Aktualisierungen an der Datenbank „localdb“ vorzunehmen. 

Klicken Sie im Menü **Extras** auf **NuGet-Paket-Manager** > **Paket-Manager-Konsole**. Die Konsole wird in der Regel im unteren Fenster geöffnet.

Aktivieren Sie Code First-Migrationen wie folgt:

```PowerShell
Enable-Migrations
```

Fügen Sie wie folgt eine Migration hinzu:

```PowerShell
Add-Migration AddProperty
```

Aktualisieren Sie die Datenbank „localdb“ wie folgt:

```PowerShell
Update-Database
```

Testen Sie die Änderungen, indem Sie die Anwendung mit `F5` ausführen.

Wenn die Anwendung ohne Fehler geladen wird, waren die Code First-Migrationen erfolgreich. Allerdings wird die Seite noch genauso angezeigt, weil die Anwendungslogik noch nicht die neue Eigenschaft verwendet. 

### <a name="use-the-new-property"></a>Verwenden der neuen Eigenschaft

Jetzt nehmen Sie Änderungen in Ihrem Code vor, um die `Done`-Eigenschaft zu verwenden. Der Einfachheit halber ändern Sie in diesem Lernprogramm nur Ansichten `Index` und `Create`, um die Eigenschaft in Aktion zu sehen.

Öffnen Sie `Controllers\TodosController.cs`.

Suchen Sie die `Create()`-Methode, und fügen Sie `Done` zur Liste der Eigenschaften im `Bind`-Attribut hinzu. Wenn Sie hiermit fertig sind, sollte die Signatur der `Create()`-Methode wie folgt aussehen:

```csharp
public ActionResult Create([Bind(Include = "id,Description,CreatedDate,Done")] Todo todo)
```

Öffnen Sie `Views\Todos\Create.cshtml`.

Im Razor-Code sollten Sie ein `<div class="form-group">`-Tag sehen, das `model.Description` verwendet, sowie ein weiteres `<div class="form-group">`-Tag, das `model.CreatedDate` verwendet. Fügen Sie unmittelbar nach diesen beiden Tags ein weiteres `<div class="form-group">`-Tag hinzu, das `model.Done` verwendet:

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

Öffnen Sie `Views\Todos\Index.cshtml`.

Suchen Sie das leere `<th></th>`-Tag. Fügen Sie direkt oberhalb dieses Tags folgenden Razor-Code hinzu:

```csharp
<th>
    @Html.DisplayNameFor(model => model.Done)
</th>
```

Suchen Sie das `<td>`-Tag, das die `Html.ActionLink()`-Hilfsmethoden enthält. Fügen Sie direkt oberhalb dieses Tags folgenden Razor-Code hinzu:

```csharp
<td>
    @Html.DisplayFor(modelItem => item.Done)
</td>
```

Mehr ist nicht erforderlich, um die Änderungen in den Ansichten `Index` und `Create` anzuzeigen. 

Drücken Sie erneut `F5`, um die Anwendung auszuführen.

Sie können jetzt eine Aufgabe hinzufügen und **Fertig** aktivieren. Die Aufgabe sollte dann auf der Startseite als erledigt angezeigt werden. Mehr können Sie nicht tun, da Sie die Ansicht `Edit` nicht geändert haben.

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

Nachdem Sie Code First-Migrationen in der Azure-Web-App aktiviert haben, veröffentlichen Sie einfach die Änderungen am Code.

Klicken Sie auf der Veröffentlichungsseite auf **Veröffentlichen**.

Versuchen Sie erneut, neue Aufgaben zu erstellen, und wählen Sie **Fertig** aus. Sie sollten auf der Startseite als abgeschlossene Aufgaben angezeigt werden.

![Azure-Web-App nach Code First-Migration](./media/app-service-web-tutorial-dotnet-sqldatabase/this-one-is-done.png)

> [!NOTE]
> Beachten Sie, dass alle vorhandenen Aufgaben weiterhin angezeigt werden. Wenn Sie die ASP.NET-Anwendung erneut veröffentlichen, gehen in der SQL-Datenbank vorhandene Daten nicht verloren. Außerdem wird durch Code First-Migrationen nur das Datenschema geändert, die vorhandenen Daten bleiben unverändert.
>
>

## <a name="step-6---stream-application-logs"></a>Schritt 6: Streamen von Anwendungsprotokollen

Sie können Ablaufverfolgungsmeldungen direkt aus der Azure-Web-App in Visual Studio streamen.

Öffnen Sie `Controllers\TodosController.cs`.

Beachten Sie, dass jede Aktion mit einer `Trace.WriteLine()`-Methode beginnt. Dieser Code wird hinzugefügt, um zu zeigen, wie leicht Sie Ablaufverfolgungsnachrichten zu Ihrer Azure-Web-App hinzufügen können.

### <a name="open-server-explorer"></a>Öffnen des Server-Explorer

Sie können die Protokollierung für die Azure-Web-App im **Server-Explorer** konfigurieren. 

Geben Sie zum Öffnen `Ctrl`+`Alt`+`S` ein.

### <a name="enable-log-streaming"></a>Aktivieren des Protokollstreamings

Erweitern Sie im **Server-Explorer** den Knoten **Azure** > **App Service**.

Erweitern Sie die Ressourcengruppe **myResourceGroup**, die Sie beim Erstellen der Azure-Web-App erstellt haben.

Klicken Sie mit der rechten Maustaste auf Ihre Azure-Web-App, und wählen Sie **Streamingprotokolle anzeigen** aus.

![Aktivieren des Protokollstreamings](./media/app-service-web-tutorial-dotnet-sqldatabase/stream-logs.png)

Die Protokolle werden jetzt in das Ausgabefenster**** gestreamt. 

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

Klicken Sie in Ihrem Browser auf die Aufgaben-App in Azure. Die Ablaufverfolgungsmeldungen werden jetzt in das Ausgabefenster**** in Visual Studio gestreamt.

```
Application: 2017-04-06T23:30:41  PID[8132] Verbose     GET /Todos/Index
Application: 2017-04-06T23:30:43  PID[8132] Verbose     GET /Todos/Create
Application: 2017-04-06T23:30:53  PID[8132] Verbose     POST /Todos/Create
Application: 2017-04-06T23:30:54  PID[8132] Verbose     GET /Todos/Index
```

### <a name="stop-log-streaming"></a>Beenden des Protokollstreamings

Klicken Sie zum Beenden des Diensts für das Protokollstreaming auf die Schaltfläche **Überwachung beenden** im****  Ausgabefenster.

![Beenden des Protokollstreamings](./media/app-service-web-tutorial-dotnet-sqldatabase/stop-streaming.png)

## <a name="step-7---manage-your-azure-web-app"></a>Schritt 7: Verwalten Ihrer Azure-Web-App

Wechseln Sie zum Azure-Portal, um die erstellte Web-App anzuzeigen. 

Melden Sie sich hierzu bei [https://portal.azure.com](https://portal.azure.com) an.

Klicken Sie im linken Menü auf **App Service** und anschließend auf den Namen Ihrer Azure-Web-App.

![Portalnavigation zur Azure-Web-App](./media/app-service-web-tutorial-dotnet-sqldatabase/access-portal.png)

Dadurch gelangen Sie auf das _Blatt_ Ihrer Web-App (eine Portalseite, die horizontal geöffnet wird). 

Auf dem Blatt Ihrer Web-App wird standardmäßig die Seite **Übersicht** angezeigt. Diese Seite bietet einen Überblick über den Status Ihrer App. Hier können Sie auch einfache Verwaltungsaufgaben wie Durchsuchen, Beenden, Neustarten und Löschen durchführen. Die Registerkarten auf der linken Seite des Blatts zeigen die verschiedenen Konfigurationsseiten, die Sie öffnen können. 

![App Service-Blatt im Azure-Portal](./media/app-service-web-tutorial-dotnet-sqldatabase/web-app-blade.png)

Die Registerkarten auf dem Blatt zeigen die vielen tollen Features, mit denen Sie Ihre Web-App ausstatten können. Ein paar Beispiele:

- Zuordnen eines benutzerdefinierten DNS-Namens
- Binden eines benutzerdefinierten SSL-Zertifikats
- Konfigurieren von Continuous Deployment
- Zentrales und horizontales Hochskalieren
- Hinzufügen einer Benutzerauthentifizierung

## <a name="next-steps"></a>Nächste Schritte

Machen Sie sich mit vorgefertigten [Azure PowerShell-Beispielen](app-service-powershell-samples.md) vertraut.
