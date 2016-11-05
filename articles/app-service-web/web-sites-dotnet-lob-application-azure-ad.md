---
title: Erstellen einer Azure-Branchen-App mit Azure Active Directory-Authentifizierung | Microsoft Docs
description: Erfahren Sie, wie Sie eine ASP.NET MVC-Branchen-App in Azure App Service erstellen, die mit Azure Active Directory authentifiziert wird.
services: app-service\web, active-directory
documentationcenter: .net
author: cephalin
manager: wpickett
editor: ''

ms.service: app-service-web
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 09/01/2016
ms.author: cephalin

---
# Erstellen einer Azure-Branchen-App mit Azure Active Directory-Authentifizierung
In diesem Artikel erfahren Sie, wie Sie eine .NET-Branchen-App in [Azure App Service-Web-Apps](http://go.microsoft.com/fwlink/?LinkId=529714) mithilfe der Funktion [Authentifizierung/Autorisierung](../app-service/app-service-authentication-overview.md) erstellen. Außerdem lernen Sie, wie Sie die [Azure Active Directory Graph-API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) zum Abfragen von Verzeichnisdaten in der Anwendung verwenden.

Der Azure Active Directory-Mandant, den Sie verwenden, kann ein reines Azure-Verzeichnis sein. Er kann aber auch [mit Ihrem lokalen Active Directory synchronisiert sein](../active-directory/active-directory-aadconnect.md), um das einmalige Anmelden für lokale und Remote-Worker zu ermöglichen. In diesem Artikel wird das Standardverzeichnis für Ihr Azure-Konto verwendet.

<a name="bkmk_build"></a>

## Was Sie erstellen
Sie erstellen eine einfache Branchenanwendung für Create-Read-Update-Delete (CRUD) in App Service-Web-Apps, die Arbeitsaufgaben mit folgenden Features erfasst:

* Authentifizieren von Benutzern mit Azure Active Directory
* Abfragen von Verzeichnisbenutzern und -gruppen mit der [Azure Active Directory Graph-API](http://msdn.microsoft.com/library/azure/hh974476.aspx)
* Verwenden der ASP.NET MVC-Vorlage *Keine Authentifizierung*

Wenn Sie rollenbasierte Zugriffskontrolle (RBAC) für Ihre Branchen-App in Azure benötigen, finden Sie unter [Nächster Schritt](#next) weitere Informationen.

<a name="bkmk_need"></a>

## Erforderliches Element
[!INCLUDE [free-trial-note](../../includes/free-trial-note.md)]

Sie benötigen Folgendes zum Bearbeiten dieses Lernprogramms:

* Einen Azure Active Directory-Mandanten mit Benutzern in verschiedenen Gruppen
* Berechtigungen zum Erstellen von Anwendungen auf dem Azure Active Directory-Mandanten
* Visual Studio 2013, Update 4 (oder höher)
* [Azure SDK 2.8.1 oder höher](https://azure.microsoft.com/downloads/)

<a name="bkmk_deploy"></a>

## Erstellen und Bereitstellen einer Web-App in Azure
1. Klicken Sie in Visual Studio auf **Datei** > **Neu** > **Projekt**.
2. Wählen Sie **ASP.NET-Webanwendung**, benennen Sie das Projekt, und klicken Sie auf **OK**.
3. Wählen Sie die Vorlage **MVC** aus, und ändern Sie die Authentifizierung in **Keine Authentifizierung**. Vergewissern Sie sich, dass **In der Cloud hosten** ausgewählt ist, und klicken Sie auf **OK**.
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/1-create-mvc-no-authentication.png)
4. Klicken Sie im Dialogfeld **App Service erstellen** auf **Konto hinzufügen** (wählen Sie anschließend **Konto hinzufügen** in der Dropdownliste), und melden Sie sich bei Ihrem Azure-Konto an.
5. Nach der Anmeldung können Sie Ihre Web-App konfigurieren. Erstellen Sie eine Ressourcengruppe und einen neuen App Service-Plan, indem Sie jeweils auf die Schaltfläche **Neu** klicken. Klicken Sie auf **Explore additional services** (Zusätzliche Dienste erkunden), um fortzufahren.
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/2-create-app-service.png)
6. Klicken Sie auf der Registerkarte **Dienste** auf **+**, um Ihrer App eine SQL-Datenbank hinzuzufügen.
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/3-add-sql-database.png)
7. Klicken Sie unter **SQL-Datenbank konfigurieren** auf **Neu**, um eine SQL Server-Instanz zu erstellen.
8. Konfigurieren Sie unter **SQL Server konfigurieren** Ihre SQL Server-Instanz. Klicken Sie dann auf **OK**, **OK** und **Erstellen**, um mit der App-Erstellung in Azure zu beginnen.
9. Unter **Aktivität von Azure App Service**, sehen Sie, wenn die App-Erstellung abgeschlossen ist. Klicken Sie auf **Publish &lt;*appname*> to this Web App now** (<App-Name> jetzt in dieser Web-App veröffentlichen) und anschließend auf **Veröffentlichen**.
   
    Wenn Visual Studio dies abgeschlossen hat, wird die veröffentlichte App im Browser geöffnet.
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/4-published-shown-in-browser.png)

<a name="bkmk_auth"></a>

## Konfigurieren von Authentifizierung und Verzeichniszugriff
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Klicken Sie im linken Menü auf **App Services** > **& Lt;*App-Name*>** > **Authentifizierung/Autorisierung**.
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/5-app-service-authentication.png)
3. Aktivieren Sie die Azure Active Directory-Authentifizierung, indem Sie auf **Ein** > **Azure Active Directory** > **Express** > **OK** klicken.
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/6-authentication-express.png)
4. Klicken Sie in der Befehlsleiste auf **Speichern**.
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/7-authentication-save.png)
   
    Nachdem die Authentifizierungseinstellungen erfolgreich gespeichert wurden, versuchen Sie erneut, im Browser zu Ihrer App zu navigieren. Die Standardeinstellungen erzwingen die Authentifizierung für die gesamte App. Wenn Sie noch nicht angemeldet sind, werden Sie zu einer Anmeldeseite umgeleitet. Nach der Anmeldung sehen Sie, dass Ihre App durch HTTPS geschützt ist. Als Nächstes müssen Sie den Zugriff auf Verzeichnisdaten aktivieren.
5. Navigieren Sie zum [klassischen Portal](https://manage.windowsazure.com).
6. Klicken Sie im linken Menü auf **Active Directory** > **Standardverzeichnis** > **Anwendungen** > **&lt;*App-Name*>**.
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/8-find-aad-application.png)
   
    Dies ist die Azure Active Directory-Anwendung, die App Service für Sie erstellt hat, um die Funktion „Autorisierung /Authentifizierung“ zu aktivieren.
7. Klicken Sie auf **Benutzer** und **Gruppen**, um sicherzustellen, dass im Verzeichnis mehrere Benutzer und Gruppen vorhanden sind. Erstellen Sie andernfalls einige Testbenutzer und -gruppen.
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/9-create-users-groups.png)
8. Klicken Sie auf **Konfigurieren**, um diese Anwendung zu konfigurieren.
9. Scrollen Sie nach unten zum Abschnitt **Schlüssel**, und fügen Sie einen Schlüssel hinzu, indem Sie eine Dauer auswählen. Klicken Sie anschließend auf **Delegierte Berechtigungen**, und wählen Sie **Verzeichnisdaten lesen**. Klicken Sie auf **Speichern**.
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/10-configure-aad-application.png)
10. Wenn Sie Ihre Einstellungen gespeichert haben, scrollen Sie wieder nach oben zum Abschnitt **Schlüssel**, und klicken Sie auf die Schaltfläche **Kopieren**, um den Clientschlüssel zu kopieren.
    
     ![](./media/web-sites-dotnet-lob-application-azure-ad/11-get-app-key.png)
    
    > [!IMPORTANT]
    > Wenn Sie diese Seite jetzt verlassen, können Sie auf diesen Clientschlüssel nie wieder zugreifen.
    > 
    > 
11. Als Nächstes müssen Sie Ihre Web-App mit diesem Schlüssel konfigurieren. Melden Sie sich mit Ihrem Azure-Konto beim [Azure-Ressourcen-Explorer](https://resources.azure.com) an.
12. Klicken Sie oben auf der Seite auf **Lese-/Schreibberechtigung**, um im Azure-Ressourcen-Explorer Änderungen vorzunehmen.
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/12-resource-manager-writable.png)
13. Suchen Sie die Authentifizierungseinstellungen für Ihre App unter subscriptions > **&lt;*subscriptionname*>** > **resourceGroups** > **&lt;*resourcegroupname*>** > **providers** > **Microsoft.Web** > **sites** > **&lt;*appname*>** > **config** > **authsettings** auf.
14. Klicken Sie auf **Bearbeiten**.
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/13-edit-authsettings.png)
15. Legen Sie im Bearbeitungsbereich die Eigenschaften `clientSecret` und `additionalLoginParams` wie folgt fest.
    
        ...
        "clientSecret": "<client key from the Azure Active Directory application>",
        ...
        "additionalLoginParams": ["response_type=code id_token", "resource=https://graph.windows.net"],
        ...
16. Klicken Sie oben auf **Put**, um Ihre Änderungen zu übermitteln.
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/14-edit-parameters.png)
17. Um zu überprüfen, ob Sie das Autorisierungstoken für den Zugriff auf die Azure Active Directory Graph-API besitzen, navigieren Sie einfach zu **https://&lt;*appname*>.azurewebsites.net/.auth/me** in Ihrem Browser. Wenn Sie alles richtig konfiguriert haben, sollten Sie die `access_token`-Eigenschaft in der JSON-Antwort sehen.
    
    Der `~/.auth/me`-URL-Pfad wird über die App-Authentifizierung/Autorisierung verwaltet und enthält alle Informationen im Zusammenhang mit der authentifizierten Sitzung. Weitere Informationen finden Sie unter [Authentifizierung und Autorisierung in Azure App Service](../app-service/app-service-authentication-overview.md).
    
    > [!NOTE]
    > Für `access_token` ist eine Ablaufdauer festgelegt. Allerdings bietet die App-Authentifizierung/Autorisierung eine Funktion zur Aktualisierung von Token mit `~/.auth/refresh`. Weitere Informationen zur Verwendung finden Sie im Abschnitt zum [App Service-Tokenspeicher](https://cgillum.tech/2016/03/07/app-service-token-store/).
    > 
    > 

Nun können Sie Ihre Verzeichnisdaten für weitere Aktionen nutzen.

<a name="bkmk_crud"></a>

## Hinzufügen von Branchenfunktionen zur Anwendung
Jetzt erstellen Sie einen einfachen CRUD-Arbeitsaufgaben-Tracker.

1. Erstellen Sie im Ordner „~\\Models“ eine neue Klassendatei namens „WorkItem.cs“, und ersetzen Sie `public class WorkItem {...}` durch den folgenden Code:
   
     using System.ComponentModel.DataAnnotations;
   
     public class WorkItem
     {
   
         [Key]
         public int ItemID { get; set; }
         public string AssignedToID { get; set; }
         public string AssignedToName { get; set; }
         public string Description { get; set; }
         public WorkItemStatus Status { get; set; }
     }
   
     public enum WorkItemStatus
     {
   
         Open,
         Investigating,
         Resolved,
         Closed
     }
2. Erstellen Sie das Projekt, damit das neue Modell für die Gerüstlogik in Visual Studio zugänglich ist.
3. Fügen Sie das neue Gerüstelement `WorkItemsController` zum Ordner „~\\Controllers“ hinzu (klicken Sie mit der rechten Maustaste auf **Controller**, zeigen Sie auf **Hinzufügen**, und wählen Sie **Neues Gerüstelement**).
4. Wählen Sie **MVC 5-Controller mit Ansichten per Entity Framework**, und klicken Sie auf **Hinzufügen**.
5. Wählen Sie das Modell, das Sie erstellt haben, klicken Sie auf **+** und **Hinzufügen**, um einen Datenkontext hinzuzufügen, und klicken Sie anschließend auf **Hinzufügen**.
   
   ![](./media/web-sites-dotnet-lob-application-azure-ad/16-add-scaffolded-controller.png)
6. Suchen Sie in „~Views\\WorkItems\\Create.cshtml“ (einem automatisch erstellten Gerüstelement) die Hilfsmethode `Html.BeginForm`, und nehmen Sie die hervorgehobenen Änderungen vor:
   
   <pre class="prettyprint">
   @model WebApplication1.Models.WorkItem
   
   @{
    ViewBag.Title = "Create";
   }
   
   &lt;h2>Create&lt;/h2>
   
   @using (Html.BeginForm(<mark>"Create", "WorkItems", FormMethod.Post, new { id = "main-form" }</mark>)) 
   {
    @Html.AntiForgeryToken()
   
    &lt;div class="form-horizontal">
        &lt;h4>WorkItem&lt;/h4>
        &lt;hr />
        @Html.ValidationSummary(true, "", new { @class = "text-danger" })
        &lt;div class="form-group">
            @Html.LabelFor(model => model.AssignedToID, htmlAttributes: new { @class = "control-label col-md-2" })
            &lt;div class="col-md-10">
                @Html.EditorFor(model => model.AssignedToID, new { htmlAttributes = new { @class = "form-control"<mark>, @type = "hidden"</mark> } })
                @Html.ValidationMessageFor(model => model.AssignedToID, "", new { @class = "text-danger" })
            &lt;/div>
        &lt;/div>
   
        &lt;div class="form-group">
            @Html.LabelFor(model => model.AssignedToName, htmlAttributes: new { @class = "control-label col-md-2" })
            &lt;div class="col-md-10">
                @Html.EditorFor(model => model.AssignedToName, new { htmlAttributes = new { @class = "form-control" } })
                @Html.ValidationMessageFor(model => model.AssignedToName, "", new { @class = "text-danger" })
            &lt;/div>
        &lt;/div>
   
        &lt;div class="form-group">
            @Html.LabelFor(model => model.Description, htmlAttributes: new { @class = "control-label col-md-2" })
            &lt;div class="col-md-10">
                @Html.EditorFor(model => model.Description, new { htmlAttributes = new { @class = "form-control" } })
                @Html.ValidationMessageFor(model => model.Description, "", new { @class = "text-danger" })
            &lt;/div>
        &lt;/div>
   
        &lt;div class="form-group">
            @Html.LabelFor(model => model.Status, htmlAttributes: new { @class = "control-label col-md-2" })
            &lt;div class="col-md-10">
                @Html.EnumDropDownListFor(model => model.Status, htmlAttributes: new { @class = "form-control" })
                @Html.ValidationMessageFor(model => model.Status, "", new { @class = "text-danger" })
            &lt;/div>
        &lt;/div>
   
        &lt;div class="form-group">
            &lt;div class="col-md-offset-2 col-md-10">
                &lt;input type="submit" value="Create" class="btn btn-default"<mark> id="submit-button"</mark> />
            &lt;/div>
        &lt;/div>
    &lt;/div>
   }
   
   &lt;div>
    @Html.ActionLink("Back to List", "Index")
   &lt;/div>
   
   @section Scripts {
    @Scripts.Render("~/bundles/jqueryval")
    <mark>&lt;script>
        // Code für Personen-/Gruppenauswahl
        var maxResultsPerPage = 14;
        var input = document.getElementById("AssignedToName");
   
        // Zugriffstoken aus Anforderungsheader und Mandanten-ID aus Anspruchsidentität
        var token = "@Request.Headers["X-MS-TOKEN-AAD-ACCESS-TOKEN"]";
        var tenant ="@(System.Security.Claims.ClaimsPrincipal.Current.Claims
                        .Where(c => c.Type == "http://schemas.microsoft.com/identity/claims/tenantid")
                        .Select(c => c.Value).SingleOrDefault())";
   
        var picker = new AadPicker(maxResultsPerPage, input, token, tenant);
   
        // Übermitteln Sie den ausgewählten Benutzer bzw. die Gruppe für die Zuweisung.
        $("#submit-button").click({ picker: picker }, function () {
            if (!picker.Selected())
                return;
            $("#main-form").get()[0].elements["AssignedToID"].value = picker.Selected().objectId;
        });
    &lt;/script></mark>
   }
   </pre>
   
   Beachten Sie, dass `token` und `tenant` vom Objekt `AadPicker` für Azure Active Directory Graph-API verwendet werden. `AadPicker` fügen Sie später hinzu.
   
   > [!NOTE]
   > Sie können auch `token` und `tenant` mit `~/.auth/me` vom Client abrufen, aber dazu wäre ein weiterer Serveraufruf erforderlich. Beispiel:
   > 
   > $.ajax({
   > dataType: "json",
   > url: "/.auth/me",
   > success: function (data) {
   > var token = data[0].access_token;
   > var tenant = data[0].user_claims
   > .find(c => c.typ === 'http://schemas.microsoft.com/identity/claims/tenantid')
   > .val;
   > }
   > });
   > 
   > 
7. Nehmen Sie die gleichen Änderungen bei „~\\Views\\WorkItems\\Edit.cshtml“ vor.
8. Das Objekt `AadPicker` wird in einem Skript definiert, das Sie Ihrem Projekt hinzufügen müssen. Klicken Sie mit der rechten Maustaste auf den Ordner „~\\Scripts“, zeigen Sie auf **Hinzufügen**, und klicken Sie auf die **JavaScript-Datei**. Geben Sie `AadPickerLibrary` als Dateinamen ein, und klicken Sie auf **OK**.
9. Kopieren Sie den Inhalt von [hier](https://raw.githubusercontent.com/cephalin/active-directory-dotnet-webapp-roleclaims/master/WebApp-RoleClaims-DotNet/Scripts/AadPickerLibrary.js) in „~\\Scripts\\AadPickerLibrary.js“.
   
   Im Skript ruft das `AadPicker`-Objekt die Azure Active Directory Graph-[API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) auf, um nach Benutzern und Gruppen zu suchen, die für die Eingabe eine Übereinstimmung ergeben.
10. „~\\Scripts\\AadPickerLibrary.js“ verwendet auch das [jQuery UI Autocomplete Widget](https://jqueryui.com/autocomplete/). Sie müssen Ihrem Projekt jetzt jQuery UI hinzufügen. Klicken Sie mit der rechten Maustaste auf das Projekt, und klicken Sie auf **NuGet-Pakete verwalten**.
11. Klicken Sie im NuGet-Paket-Manager auf „Durchsuchen“, geben Sie in der Suchleiste **Jquery Ui** ein, und klicken Sie auf **jQuery.UI.Combined**.
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/17-add-jquery-ui-nuget.png)
12. Klicken Sie im rechten Bereich auf **Installieren** und anschließend auf **OK**, um den Vorgang fortzusetzen.
13. Öffnen Sie „~\\App\_Start\\BundleConfig.cs“, und nehmen Sie die folgenden hervorgehobenen Änderungen vor:
    
    <pre class="prettyprint">
    public static void RegisterBundles(BundleCollection bundles)
    {
        bundles.Add(new ScriptBundle("~/bundles/jquery").Include(
                    "~/Scripts/jquery-{version}.js"<mark>,
                    "~/Scripts/jquery-ui-{version}.js",
                    "~/Scripts/AadPickerLibrary.js"</mark>));
    
        bundles.Add(new ScriptBundle("~/bundles/jqueryval").Include(
                    "~/Scripts/jquery.validate*"));
    
        // Use the development version of Modernizr to develop with and learn from. Then, when you're
        // ready for production, use the build tool at http://modernizr.com to pick only the tests you need.
        bundles.Add(new ScriptBundle("~/bundles/modernizr").Include(
                    "~/Scripts/modernizr-*"));
    
        bundles.Add(new ScriptBundle("~/bundles/bootstrap").Include(
                    "~/Scripts/bootstrap.js",
                    "~/Scripts/respond.js"));
    
        bundles.Add(new StyleBundle("~/Content/css").Include(
                    "~/Content/bootstrap.css",
                    "~/Content/site.css"<mark>,
                    "~/Content/themes/base/jquery-ui.css"</mark>));
    }
    </pre>
    
    Es gibt leistungsfähigere Möglichkeiten, mit denen Sie JavaScript und CSS-Dateien in Ihrer App verwalten können. Verwenden Sie der Einfachheit halber aber nur die Bündel, die mit jeder Ansicht geladen werden.
14. Fügen Sie dann in „~\\Global.asax“ die folgende Codezeile in der Methode `Application_Start()` hinzu. `Ctrl`+`.` dient zur Behebung von Fehlern bei der Namensauflösung.
    
        AntiForgeryConfig.UniqueClaimTypeIdentifier = ClaimTypes.NameIdentifier;
    
    > [!NOTE]
    > Sie benötigen diese Codezeile, weil die Standard-MVC-Vorlage bei einigen der Aktionen die Dekoration <code>[ValidateAntiForgeryToken]</code> verwendet. Aufgrund des Verhaltens, das von [Brock Allen](https://twitter.com/BrockLAllen) unter [MVC 4, AntiForgeryToken and Claims](http://brockallen.com/2012/07/08/mvc-4-antiforgerytoken-and-claims/) (MVC 4, AntiForgeryToken und Ansprüche) beschrieben wurde, ist aus folgenden Gründen eventuell keine Fälschungsschutz-Überprüfung für Ihren HTTP POST möglich:
    > 
    > * Azure Active Directory sendet kein vom Fälschungsschutztoken standardmäßig erforderliches http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider.
    > * Wenn Azure Active Directory mit AD FS synchronisiert ist, sendet die AD FS-Vertrauensstellung standardmäßig ebenfalls nicht den http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider-Anspruch, Sie können AD FS jedoch manuell zum Senden dieses Anspruchs konfigurieren.
    > 
    > Mit `ClaimTypes.NameIdentifies` wird der Anspruch `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier` angegeben, der von Azure Active Directory nicht bereitgestellt wird.
    > 
    > 
15. Veröffentlichen Sie nun Ihre Änderungen. Klicken Sie mit der rechten Maustaste auf Ihr Projekt, und wählen Sie **Veröffentlichen**.
16. Klicken Sie auf **Einstellungen**, stellen Sie sicher, dass eine Verbindungszeichenfolge für die SQL-Datenbank vorhanden ist, wählen Sie **Datenbank aktualisieren**, um das Schema Ihres Modells zu ändern, und klicken Sie auf **Veröffentlichen**.
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/18-publish-crud-changes.png)
17. Navigieren Sie im Browser zu „https://&lt;*appname*>.azurewebsites.net/workitems“, und klicken Sie auf **Neu erstellen**.
18. Klicken Sie in das Feld **AssignedToName**. Nun werden Ihnen Benutzer und Gruppen Ihres Azure Active Directory-Mandanten in einer Dropdownliste angezeigt. Sie können zum Filtern etwas eingeben, oder mithilfe der Tasten `Up` oder `Down` bzw. durch Klicken den Benutzer oder die Gruppe auswählen.
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/19-use-aadpicker.png)
19. Klicken Sie zum Speichern der Änderungen auf **Erstellen**. Klicken Sie anschließend auf **Bearbeiten**, und Sie beobachten das gleiche Verhalten bei der erstellten Arbeitsaufgabe.

Herzlichen Glückwunsch, Sie führen nun eine Branchen-App in Azure mit Verzeichniszugriff aus! Graph-API bietet Ihnen noch viele weitere Möglichkeiten. Weitere Informationen finden Sie unter [Azure AD Graph API reference](https://msdn.microsoft.com/library/azure/ad/graph/api/api-catalog) (Azure AD Graph-API-Referenz).

<a name="next"></a>

## Nächster Schritt
Wenn Sie für Ihre Branchen-App rollenbasierte Zugriffskontrolle (RBAC, role-based aceess control) benötigen, hat das Azure Active Directory-Team unter [Azure AD Graph API reference](https://github.com/Azure-Samples/active-directory-dotnet-webapp-roleclaims) (Azure AD Graph-API-Referenz) ein Beispiel für Sie bereitgestellt. Dort erfahren Sie, wie Sie Rollen für Ihre Azure Active Directory-Anwendung aktivieren, und Benutzer mit der Dekoration `[Authorize]` autorisieren.

Wenn Ihre Branchen-App Zugriff auf lokale Daten benötigt, finden Sie unter [Zugreifen auf lokale Ressourcen über Hybridverbindungen in Azure App Service](web-sites-hybrid-connection-get-started.md) weitere Informationen.

<a name="bkmk_resources"></a>

## Weitere Ressourcen
* [Authentifizierung und Autorisierung in Azure App Service](../app-service/app-service-authentication-overview.md)
* [Authentifizieren mit lokaler Active Directory-Instanz in Ihrer Azure-App](web-sites-authentication-authorization.md)
* [Erstellen einer .NET MVC-Web-App in Azure App Service mit Azure Active Directory-Authentifizierung](web-sites-dotnet-lob-application-adfs.md)
* [App Service Auth and the Azure AD Graph API (App Service-Authentifizierung und Azure AD Graph-API)](https://cgillum.tech/2016/03/25/app-service-auth-aad-graph-api/)
* [Microsoft Azure Active Directory – Beispiele und Dokumentation](https://github.com/AzureADSamples)
* [Unterstützte Token- und Anspruchstypen](http://msdn.microsoft.com/library/azure/dn195587.aspx)

[Protect the Application with SSL and the Authorize Attribute]: web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md#protect-the-application-with-ssl-and-the-authorize-attribute

<!---HONumber=AcomDC_0914_2016-->