---
title: Erste Schritte mit .NET-Web-APIs in Azure AD | Microsoft-Dokumentation
description: "In diesem Thema erfahren Sie, wie eine .NET-MVC-Web-API erstellt wird, die sich für die Authentifizierung und Autorisierung in Azure AD integriert."
services: active-directory
documentationcenter: .net
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 67e74774-1748-43ea-8130-55275a18320f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: dastrock
translationtype: Human Translation
ms.sourcegitcommit: bb1ca3189e6c39b46eaa5151bf0c74dbf4a35228
ms.openlocfilehash: b028a75836f7c762431bfb9e3fc30822b7dee885
ms.lasthandoff: 03/18/2017


---
# <a name="help-protect-a-web-api-by-using-bearer-tokens-from-azure-ad"></a>Schützen einer Web-API mit Bearertoken aus Azure AD
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Wenn Sie eine Anwendung erstellen, die Zugriff auf geschützte Ressourcen bietet, müssen Sie wissen, wie Sie diese Ressourcen vor unbefugtem Zugriff schützen.
Mit Azure Active Directory (Azure AD) lässt sich eine Web-API mit nur wenigen Codezeilen unkompliziert und einfach mithilfe von OAuth 2.0-Bearerzugriffstoken schützen.

Für ASP.NET-Web-Apps erreichen Sie diesen Schutz mithilfe der Microsoft-Implementierung der von der Community gestützten und in .NET Framework 4.5 enthaltenen Middleware OWIN. Hier verwenden wir OWIN zur Erstellung einer Web-API mit einer Aufgabenliste, die folgende Aufgaben ausführt:

* Festlegen, welche APIs geschützt sind.
* Prüfen, ob die Web-API-Aufrufe ein gültiges Zugriffstoken enthalten.

Zum Erstellen der „ToDoList“-API müssen Sie zunächst so vorgehen:

1. Registrieren Sie eine Anwendung in Azure AD.
2. Konfigurieren Sie die App für die Verwendung der OWIN-Authentifizierungspipeline.
3. Konfigurieren Sie eine Clientanwendung für den Aufruf der Web-API.

Beginnen Sie, indem Sie das [Anwendungsgerüst](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/skeleton.zip) oder [das vollständige Beispiel herunterladen](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip). Bei beidem handelt es sich um eine Visual Studio 2013-Lösung. Außerdem benötigen Sie einen Azure AD-Mandanten, bei dem Sie Ihre Anwendung registrieren. Wenn Sie noch keinen Mandanten haben, [erfahren Sie hier, wie Sie einen erhalten](active-directory-howto-tenant.md).

## <a name="step-1-register-an-application-with-azure-ad"></a>Schritt 1: Registrieren einer Anwendung in Azure AD
Zum Schutz Ihrer Anwendung müssen Sie in Ihrem Mandanten zunächst eine Anwendung erstellen und Azure AD einige wichtige Informationen bereitstellen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.

2. Klicken Sie auf der oberen Leiste auf Ihr Konto. Wählen Sie in der **Verzeichnis** den Azure AD-Mandanten aus, bei dem Sie Ihre Anwendung registrieren möchten.

3. Klicken Sie im linken Bereich auf **Weitere Dienste**, und wählen Sie **Azure Active Directory** aus.

4. Klicken Sie auf **App-Registrierungen**, und wählen Sie **Hinzufügen** aus.

5. Folgen Sie den Bildschirmaufforderungen, und erstellen Sie eine neue **Webanwendung und/oder Web-API**.
  * **Name**: Beschreibt die Anwendung für Benutzer. Geben Sie **TodoListService** ein.
  * Der **Umleitungs-URI** ist eine Kombination aus einem Schema und einer Zeichenfolge, die Azure AD für die Rückgabe der von der Anforderung angeforderten Token verwendet. Geben Sie hier `https://localhost:44321/` ein.

6. Aktualisieren Sie auf der Seite **Einstellungen** -> **Eigenschaften** für Ihre Anwendung den App-ID-URI. Geben Sie einen mandantenspezifischen Bezeichner ein. Geben Sie z. B. Folgendes ein: `https://contoso.onmicrosoft.com/TodoListService`.

7. Speichern Sie die Konfiguration. Lassen Sie das Portal geöffnet, da Sie in Kürze auch Ihre Clientanwendung registrieren müssen.

## <a name="step-2-set-up-the-app-to-use-the-owin-authentication-pipeline"></a>Schritt 2: Konfigurieren der App für die Verwendung der OWIN-Authentifizierungspipeline
Um eingehende Anforderungen und Token zu überprüfen, müssen Sie Ihre Anwendung für die Kommunikation mit Azure AD einrichten.

1. Öffnen Sie dazu zunächst die Projektmappe, und fügen Sie dem Projekt „TodoListService“ über die Paket-Manager-Konsole die NuGet-Pakete der OWIN-Middleware hinzu.

    ```
    PM> Install-Package Microsoft.Owin.Security.ActiveDirectory -ProjectName TodoListService
    PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
    ```

2. Fügen Sie dem Projekt „TodoListService“ eine OWIN-Startklasse mit dem Namen `Startup.cs`hinzu.  Klicken Sie mit der rechten Maustaste auf das Projekt, wählen Sie **Hinzufügen** > **Neues Element** aus, und suchen Sie dann nach **OWIN**. Die OWIN-Middleware ruft beim Starten Ihrer Anwendung die Methode `Configuration(…)` auf.

3. Ändern Sie die Klassendeklaration in `public partial class Startup`. Wir haben für Sie einen Teil dieser Klasse in einer anderen Datei bereits implementiert. Rufen Sie in der `Configuration(…)`-Methode `ConfgureAuth(…)` auf, um die Authentifizierung für Ihre Web-App einzurichten.

    ```C#
    public partial class Startup
    {
        public void Configuration(IAppBuilder app)
        {
            ConfigureAuth(app);
        }
    }
    ```

4. Öffnen Sie die Datei `App_Start\Startup.Auth.cs`, und implementieren Sie die Methode `ConfigureAuth(…)`. Die Parameter, die Sie in `WindowsAzureActiveDirectoryBearerAuthenticationOptions` bereitstellen, dienen als Koordinaten für Ihre App zur Kommunikation mit Azure AD.

    ```C#
    public void ConfigureAuth(IAppBuilder app)
    {
        app.UseWindowsAzureActiveDirectoryBearerAuthentication(
            new WindowsAzureActiveDirectoryBearerAuthenticationOptions
            {
                Audience = ConfigurationManager.AppSettings["ida:Audience"],
                Tenant = ConfigurationManager.AppSettings["ida:Tenant"]
            });
    }
    ```

5. Nun können Sie Ihre Controller und Aktionen mit `[Authorize]`-Attributen durch eine JWT-Bearerauthentifizierung (JSON Web Token) schützen. Geben Sie für die Klasse `Controllers\TodoListController.cs` ein Autorisierungstag an. Dadurch wird der Benutzer gezwungen, sich vor dem Zugriff auf diese Seite anzumelden.

    ```C#
    [Authorize]
    public class TodoListController : ApiController
    {
    ```

    Wenn ein autorisierter Aufrufer eine der `TodoListController` -APIs erfolgreich aufruft, muss die Aktion möglicherweise auf Informationen über den Aufrufer zugreifen können. OWIN stellt über das Objekt `ClaimsPrincpal` Zugriff auf die Ansprüche innerhalb des Bearertokens bereit.  

6. Eine häufige Anforderung an Web-APIs ist das Überprüfen der im Token vorhandenen „Bereiche“. Dadurch wird sichergestellt, dass der Benutzer den für den Zugriff auf den „TodoListService“ erforderlichen Berechtigungen zugestimmt hat.

    ```C#
    public IEnumerable<TodoItem> Get()
    {
        // user_impersonation is the default permission exposed by applications in Azure AD
        if (ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/scope").Value != "user_impersonation")
        {
            throw new HttpResponseException(new HttpResponseMessage {
              StatusCode = HttpStatusCode.Unauthorized,
              ReasonPhrase = "The Scope claim does not contain 'user_impersonation' or scope claim not found"
            });
        }
        ...
    }
    ```

7. Öffnen Sie im Stammverzeichnis des Projekts „TodoListService“ die Datei `web.config`, und geben Sie Ihre Konfigurationswerte im Abschnitt `<appSettings>` ein.
  * `ida:Tenant`: Der Name Ihres Azure AD-Mandanten, z.B. „contoso.onmicrosoft.com“.
  * `ida:Audience`: Der App-ID-URI der Anwendung, den Sie in das Azure-Portal eingegeben haben.

## <a name="step-3-configure-a-client-application-and-run-the-service"></a>Schritt 3: Konfigurieren einer Clientanwendung und Ausführen des Diensts
Um „TodoListService“ in Aktion sehen zu können, müssen Sie den TodoList-Client so konfigurieren, dass er Token von Azure AD erhält und Aufrufe an den Dienst richten kann.

1. Wechseln Sie zurück zum [Azure-Portal](https://portal.azure.com).

2. Erstellen Sie in Ihrem Azure AD-Mandanten eine neue Anwendung, und wählen Sie an der Eingabeaufforderung **Systemeigene Clientanwendung** aus.
  * **Name**: Beschreibt die Anwendung für Benutzer.
  * Geben Sie als **Umleitungs-URI** `http://TodoListClient/` ein.

3. Nach Abschluss der Registrierung weist Azure AD Ihrer App eine eindeutige Anwendungs-ID zu. Diesen Wert benötigen Sie in den nächsten Schritten. Daher sollten Sie ihn von der Anwendungsseite kopieren.

4. Wählen Sie auf der Seite **Einstellungen** zunächst **Erforderliche Berechtigungen** und dann **Hinzufügen** aus. Suchen Sie den „TodoListService“, wählen Sie ihn aus, und fügen Sie unter **Delegierte Berechtigungen** die Berechtigung **Auf TodoListService zugreifen** hinzu. Klicken Sie anschließend auf **Fertig**.

5. Öffnen Sie in Visual Studio im Projekt „TodoListClient“ die Datei `App.config`, und geben Sie dann Ihre Konfigurationswerte im Abschnitt `<appSettings>` ein.

  * `ida:Tenant`: Der Name Ihres Azure AD-Mandanten, z.B. „contoso.onmicrosoft.com“.
  * `ida:ClientId`: Die App-ID, die Sie aus dem Azure-Portal kopiert haben.
  * `todo:TodoListResourceId`: Der App-ID-URI der Anwendung „TodoListService“, den Sie im Azure-Portal eingegeben haben.

## <a name="next-steps"></a>Nächste Schritte
Bereinigen, erstellen und führen Sie zum Schluss alle Projekte aus. Falls noch nicht geschehen, ist es nun Zeit, in Ihrem Mandanten einen neuen Benutzer mit einer *.onmicrosoft.com-Domäne zu erstellen. Melden Sie sich unter diesem Benutzer beim TodoList-Client an, und fügen Sie der Aufgabenliste des Benutzers einige Aufgaben hinzu.

Das vollständige Beispiel (ohne Ihre Konfigurationswerte) steht in [GitHub](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip) zur Verfügung. Sie können sich nun weiteren Identitätsszenarien zuwenden.

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]

