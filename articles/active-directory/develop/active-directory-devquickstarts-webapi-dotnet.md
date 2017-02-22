---
title: Erste Schritte in Azure AD .NET | Microsoft Docs
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
ms.date: 01/07/2017
ms.author: dastrock
translationtype: Human Translation
ms.sourcegitcommit: c579135f798ea0c2a5461fdd7c88244d2d6d78c6
ms.openlocfilehash: c401d473d3951ed5853170e8761a09a915329164


---
# <a name="protect-a-web-api-using-bearer-tokens-from-azure-ad"></a>Schützen einer Web-API mit Bearer-Tokens aus Azure AD
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Wenn Sie eine Anwendung erstellen, die Zugriff auf geschützte Ressourcen bietet, müssen Sie wissen, wie Sie diese Ressourcen vor unbefugtem Zugriff schützen.
Mit Azure AD lässt sich eine Web-API mit nur wenigen Codezeilen unkompliziert und einfach mithilfe von OAuth Bearer 2.0-Zugriffstokens schützen.

Für Asp.NET-Webanwendungen erreichen Sie das Gleiche durch die Microsoft-Implementierung der Community-gestützten und in .NET Framework 4.5 enthaltenen OWIN-Middleware.  Hier verwenden wir OWIN zur Erstellung einer Web-API mit einer Aufgabenliste, die folgende Aufgaben ausführt:

* Sie legt fest, welche APIs geschützt sind.
* Sie stellt sicher, dass die Web-API-Aufrufe ein gültiges Zugriffstoken enthalten.

Dazu müssen Sie folgende Schritte ausführen:

1. Registrieren einer Anwendung in Azure AD
2. Konfigurieren Ihrer Anwendung für die Verwendung der OWIN-Authentifizierungspipeline
3. Konfigurieren einer Clientanwendung für den Aufruf der Web-API „To Do List“

Beginnen Sie, indem Sie das [Anwendungsgerüst](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/skeleton.zip) oder [das vollständige Beispiel herunterladen](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip).  Bei beidem handelt es sich um eine Visual Studio 2013-Lösung.  Außerdem benötigen Sie einen Azure AD-Mandanten, bei dem Sie Ihre Anwendung registrieren.  Wenn Sie noch keinen Mandanten haben, [erfahren Sie hier, wie Sie einen erhalten](active-directory-howto-tenant.md).

## <a name="1----register-an-application-with-azure-ad"></a>1.    Registrieren einer Anwendung in Azure AD
Zum Schutz Ihrer Anwendung müssen Sie in Ihrem Mandanten zunächst eine Anwendung erstellen und Azure AD einige wichtige Informationen bereitstellen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Klicken Sie oben auf der Leiste auf Ihr Konto, und wählen Sie in der Liste **Verzeichnis** den Active Directory-Mandanten aus, für den Sie Ihre Anwendungen registrieren möchten.
3. Klicken Sie im linken Navigationsbereich auf **Weitere Dienste**, und wählen Sie **Azure Active Directory** aus.
4. Klicken Sie auf **App-Registrierungen**, und wählen Sie **Hinzufügen**.
5. Folgen Sie den Bildschirmaufforderungen, und erstellen Sie eine neue **Webanwendung und/oder Web-API**.
  * Am **Namen** der Anwendung sollten die Endbenutzer die Funktion der Anwendung ablesen können.  Geben Sie „To Do List Service“ ein.
  * Die **Umleitungs-URI** ist eine Kombination aus einem Schema und einer Zeichenfolge, die Azure AD für die Rückgabe der von der Anforderung angeforderten Tokens verwendet. Geben Sie hier `https://localhost:44321/` ein.
  * Geben Sie im Feld **AppID-URI** einen mandantenspezifischen Bezeichner ein, z.B. `https://contoso.onmicrosoft.com/TodoListService`.
6. Speichern Sie die Konfiguration.  Lassen Sie das Portal geöffnet – schließlich müssen Sie gleich auch Ihre Clientanwendung registrieren.

## <a name="2-set-up-your-app-to-use-the-owin-authentication-pipeline"></a>2. Konfigurieren Ihrer Anwendung für die Verwendung der OWIN-Authentifizierungspipeline
Nachdem Sie eine Anwendung in Azure AD registriert haben, müssen Sie die Anwendung für die Kommunikation mit Azure AD konfigurieren, damit eingehende Anforderungen und Tokens überprüft werden können.

* Öffnen Sie dazu zunächst die Projektmappe, und fügen Sie dem Projekt „TodoListService“ über die Paket-Manager-Konsole die NuGet-Pakete der OWIN-Middleware hinzu.

```
PM> Install-Package Microsoft.Owin.Security.ActiveDirectory -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
```

* Fügen Sie dem Projekt „TodoListService“ eine OWIN-Startklasse mit dem Namen `Startup.cs`hinzu.  Klicken Sie mit der rechten Maustaste auf das Projekt, wählen Sie **Hinzufügen ** --> **Neues Element**, und suchen Sie nach „OWIN“.  Die OWIN-Middleware ruft beim Starten Ihrer Anwendung die Methode `Configuration(…)` auf.
* Ändern Sie die Klassendeklaration in `public partial class Startup` – einen Teil dieser Klasse haben wir bereits für Sie in einer anderen Datei implementiert.  Rufen Sie in der Methode `Configuration(…)` „ConfigureAuth(...)“ auf, um die Authentifizierung für Ihre Webanwendung einzurichten.

```C#
public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

* Öffnen Sie die Datei `App_Start\Startup.Auth.cs`, und implementieren Sie die Methode `ConfigureAuth(…)`.  Die Parameter, die Sie in `WindowsAzureActiveDirectoryBearerAuthenticationOptions` bereitstellen, dienen als Koordinaten für Ihre Anwendung zur Kommunikation mit Azure AD.

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

* Nun können Sie Ihre Controller und Aktionen mit `[Authorize]` -Attributen durch eine JWT-Bearer-Authentifizierung schützen.  Geben Sie für die Klasse `Controllers\TodoListController.cs` ein Autorisierungstag an.  Dadurch wird der Benutzer gezwungen, sich vor dem Zugriff auf diese Seite anzumelden.

```C#
[Authorize]
public class TodoListController : ApiController
{
```

* Wenn ein autorisierter Aufrufer eine der `TodoListController` -APIs erfolgreich aufruft, muss die Aktion möglicherweise auf Informationen über den Aufrufer zugreifen können.  OWIN stellt über das Objekt `ClaimsPrincpal` Zugriff auf die Ansprüche innerhalb des Bearer-Tokens bereit.  
* Im Allgemeinen müssen Web-APIs jedoch die „Bereiche“ innerhalb des Tokens überprüfen – dadurch wird sichergestellt, dass der Endbenutzer den für den Zugriff auf den Dienst „TodoListService“ erforderlichen Berechtigungen zugestimmt hat:

```C#
public IEnumerable<TodoItem> Get()
{
    // user_impersonation is the default permission exposed by applications in AAD
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

* Öffnen Sie abschließend die Datei `web.config` aus dem Stammverzeichnis des Projekts „TodoListService“, und geben Sie Ihre Konfigurationswerte im Abschnitt `<appSettings>` ein.
  * `ida:Tenant` ist der Name Ihres Azure AD-Mandanten, z. B. „contoso.onmicrosoft.com“.
  * `ida:Audience` ist die App-ID-URI der Anwendung, die Sie in das Azure-Verwaltungsportal eingegeben haben.

## <a name="3-configure-a-client-application--run-the-service"></a>3. Konfigurieren einer Clientanwendung und Ausführen des Diensts
Um den Dienst „TodoListService“ in Aktion sehen zu können, müssen Sie den TodoList-Client so konfigurieren, dass er Tokens von AAD erhält und Aufrufe an den Dienst stellen kann.

* Navigieren Sie zurück zum [Azure-Portal](https://portal.azure.com).
* Erstellen Sie in Ihrem Azure AD-Mandanten eine neue Anwendung, und wählen Sie an der Eingabeaufforderung **Systemeigene Clientanwendung** aus.
  * Am **Namen** der Anwendung sollten die Endbenutzer die Funktion der Anwendung ablesen können.
  * Geben Sie als **Umleitungs-URI** `http://TodoListClient/` ein.
* Nach Abschluss der Registrierung weist AAD Ihrer App eine eindeutige **Anwendungs-ID** zu. Diesen Wert benötigen Sie in den nächsten Schritten. Daher sollten Sie ihn von der Anwendungsseite kopieren.
* Wählen Sie auf der Seite **Einstellungen** zunächst **Erforderliche Berechtigungen** und dann **Hinzufügen** aus.  Suchen Sie TodoListService, wählen Sie ihn aus, und fügen Sie unter **Delegierte Berechtigungen** die Berechtigung **Auf TodoListService zugreifen** hinzu. Wählen Sie anschließend **Fertig** aus.

* Öffnen Sie in Visual Studio im Projekt „TodoListClient“ die Datei `App.config`, und geben Sie Ihre Konfigurationswerte im Abschnitt `<appSettings>` ein.
  
  * `ida:Tenant` ist der Name Ihres Azure AD-Mandanten, z. B. „contoso.onmicrosoft.com“.
  * `ida:ClientId` ist die App-ID, die Sie aus dem Azure-Portal kopiert haben.
  * `todo:TodoListResourceId` ist die App-ID-URI der Anwendung „TodoListService“, die Sie im Azure-Verwaltungsportal eingegeben haben.

Bereinigen, erstellen und führen Sie zum Schluss alle Projekte aus!  Falls noch nicht geschehen, ist es nun Zeit, in Ihrem Mandanten einen neuen Benutzer mit einer *.onmicrosoft.com-Domäne zu erstellen.  Melden Sie sich unter diesem Benutzer beim TodoList-Client an, und fügen Sie der Aufgabenliste des Benutzers einige Aufgaben hinzu.

Als Referenz stellen wir [hier](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip)das vollständige Beispiel (ohne Ihre Konfigurationswerte) bereit.  Sie können sich nun weiteren Identitätsszenarien zuwenden.

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]




<!--HONumber=Jan17_HO3-->


