---
title: Erste Schritte mit .NET-Web-Apps in Azure AD | Microsoft-Dokumentation
description: "In diesem Thema erfahren Sie, wie eine .NET-MVC-Web-App erstellt wird, die sich für die Anmeldung in Azure AD integriert."
services: active-directory
documentationcenter: .net
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: e15a41a4-dc5d-4c90-b3fe-5dc33b9a1e96
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: dastrock
translationtype: Human Translation
ms.sourcegitcommit: 4094759caba015b9d609b616d5099a6e109bf1d4
ms.openlocfilehash: 6ac0c3b2893b96f93bf2aeadd61b263654957477
ms.lasthandoff: 02/17/2017


---
# <a name="aspnet-web-app-sign-in-and-sign-out-with-azure-ad"></a>An- und Abmeldung bei ASP.NET-Web-Apps mit Azure AD
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Azure Active Directory (Azure AD) erleichtert die Auslagerung der Identitätsverwaltung für Web-Apps, indem eine einmalige An- und Abmeldung (Single Sign-on und Single Sign-out) mit nur wenigen Codezeilen ermöglicht wird. Mithilfe der Microsoft-Implementierung der Middleware OWIN (Open Web Interface for .NET) können Sie Benutzern das An- und Abmelden bei ASP.NET-Web-Apps ermöglichen. .NET Framework 4.5 enthält die von der Community gestützte Middleware OWIN. Dieser Artikel beschreibt, wie OWIN für die folgenden Zwecke genutzt werden kann:

* Anmelden von Benutzern bei Web-Apps mithilfe von Azure AD als Identitätsanbieter
* Anzeigen von Benutzerinformationen
* Abmelden von Benutzer von den Apps

## <a name="before-you-get-started"></a>Bevor Sie beginnen
* Laden Sie das [Anwendungsgerüst](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) oder [vollständige Beispiel](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip) herunter.
* Außerdem benötigen Sie einen Azure AD-Mandanten, bei dem Sie die App registrieren. Wenn Sie noch nicht über einen Mandanten verfügen, [erfahren Sie hier, wie Sie einen erhalten](active-directory-howto-tenant.md).

Wenn Sie bereit sind, führen Sie die Verfahren in den folgenden vier Abschnitten aus.

## <a name="step-1-register-the-new-app-with-azure-ad"></a>Schritt 1: Registrieren der neuen App bei Azure AD
Um die App für die Authentifizierung von Benutzern einzurichten, registrieren Sie sie zunächst bei Ihrem Mandanten, indem Sie wie folgt vorgehen:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Klicken Sie auf der oberen Leiste auf den Namen Ihres Kontos. Wählen Sie in der Liste **Verzeichnis** den Active Directory-Mandanten aus, bei dem Sie die App registrieren möchten.
3. Klicken Sie im linken Bereich auf **Weitere Dienste**, und wählen Sie **Azure Active Directory** aus.
4. Klicken Sie auf **App-Registrierungen**, und wählen Sie **Hinzufügen** aus.
5. Folgen Sie den Bildschirmaufforderungen zum Erstellen einer neuen **Webanwendung und/oder Web-API**.
  * **Name** enthält eine Beschreibung der App für Benutzer.
  * **Anmelde-URL** ist die Basis-URL der App. Die Standard-URL des Gerüsts lautet „https://localhost:44320/“.
  * **App-ID-URI** ist ein eindeutiger Bezeichner der App. Die Benennungskonvention lautet `https://<tenant-domain>/<app-name>` (z.B. `https://contoso.onmicrosoft.com/my-first-aad-app`).
6. Nach Abschluss der Registrierung weist Azure AD der App eine eindeutige Anwendungs-ID zu. Kopieren Sie den Wert von der App-Seite, um ihn in den nächsten Abschnitten zu verwenden.

## <a name="step-2-set-up-the-app-to-use-the-owin-authentication-pipeline"></a>Schritt 2: Konfigurieren der App für die Verwendung der OWIN-Authentifizierungspipeline
In diesem Schritt konfigurieren Sie die OWIN-Middleware für die Verwendung des Authentifizierungsprotokolls OpenID Connect. Mithilfe von OWIN können Sie An- und Abmeldeanforderungen erstellen, Benutzersitzungen verwalten, Benutzerinformationen abrufen usw.

1. Fügen Sie dem Projekt zunächst über die Paket-Manager-Konsole die NuGet-Pakete der OWIN-Middleware hinzu.

     ```
     PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
     PM> Install-Package Microsoft.Owin.Security.Cookies
     PM> Install-Package Microsoft.Owin.Host.SystemWeb
     ```

2. Fügen Sie dem Projekt eine OWIN-Startklasse mit dem Namen `Startup.cs` hinzu. Klicken Sie dazu mit der rechten Maustaste auf das Projekt, wählen Sie **Hinzufügen** --> **Neues Element** aus, und suchen Sie nach **OWIN**. Die OWIN-Middleware ruft die **Configuration(...)**-Methode auf, wenn die App gestartet wird.
3. Ändern Sie die Klassendeklaration in `public partial class Startup`. Wir haben für Sie einen Teil dieser Klasse in einer anderen Datei bereits implementiert. Rufen Sie in der **Configuration(...)**-Methode **ConfgureAuth(...)** auf, um die Authentifizierung für die App einzurichten.  

     ```C#
     public partial class Startup
     {
         public void Configuration(IAppBuilder app)
         {
             ConfigureAuth(app);
         }
     }
     ```

4. Öffnen Sie die Datei „App_Start\Startup.Auth.cs“, und implementieren Sie anschließend die **ConfigureAuth(...) **-Methode. Die Parameter, die Sie in *OpenIDConnectAuthenticationOptions* bereitstellen, dienen als Koordinaten für die App zur Kommunikation mit Azure AD. Außerdem müssen Sie die Cookie-Authentifizierung einrichten, da die Middleware OpenID Connect im Hintergrund Cookies verwendet.

     ```C#
     public void ConfigureAuth(IAppBuilder app)
     {
         app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

         app.UseCookieAuthentication(new CookieAuthenticationOptions());

         app.UseOpenIdConnectAuthentication(
             new OpenIdConnectAuthenticationOptions
             {
                 ClientId = clientId,
                 Authority = authority,
                 PostLogoutRedirectUri = postLogoutRedirectUri,
             });
     }
     ```

5. Öffnen Sie im Stammverzeichnis des Projekts die Datei „web.config“, und geben Sie die Konfigurationswerte im Abschnitt `<appSettings>` ein.
  * `ida:ClientId`: Die GUID, die Sie in „Schritt 1: Registrieren der neuen App bei Azure AD“ aus dem Azure-Portal kopiert haben.
  * `ida:Tenant`: Der Name Ihres Azure AD-Mandanten, z. B. „contoso.onmicrosoft.com“.
  * `ida:PostLogoutRedirectUri`: Der Indikator, der Azure AD anweist, wohin ein Benutzer nach einer erfolgreich ausgeführten Abmeldeanforderung umgeleitet werden soll.

## <a name="step-3-use-owin-to-issue-sign-in-and-sign-out-requests-to-azure-ad"></a>Schritt 3: Erstellen von An- und Abmeldeanforderungen an Azure AD mit OWIN
Die App ist nun ordnungsgemäß für die Kommunikation mit Azure AD über das Authentifizierungsprotokoll OpenID Connect konfiguriert. OWIN hat sich um alle Details der Erstellung von Authentifizierungsnachrichten, der Überprüfung der Azure AD-Token und der Verwaltung der Benutzersitzung gekümmert. Sie müssen lediglich dafür sorgen, dass sich Ihre Benutzer an- und abmelden können.

1. Über Autorisierungstags in Ihren Controllern können Sie die Benutzer vor dem Zugriff auf bestimmte Seiten zur Anmeldung zwingen. Zu diesem Zweck öffnen Sie „Controllers\HomeController.cs“ und fügen dann das `[Authorize]`-Tag dem „About“-Controller hinzu.

     ```C#
     [Authorize]
     public ActionResult About()
     {
       ...
     ```

2. Mit OWIN können Sie Authentifizierungsanforderungen auch direkt aus Ihrem Code ausgeben. Öffnen Sie hierzu „Controllers\AccountController.cs“. Geben Sie in den Aktionen „SignIn()“ und „SignOut()“ OpenID Connect-Challenge- und Abmeldeanforderungen ein.

     ```C#
     public void SignIn()
     {
         // Send an OpenID Connect sign-in request.
         if (!Request.IsAuthenticated)
         {
             HttpContext.GetOwinContext().Authentication.Challenge(new AuthenticationProperties { RedirectUri = "/" }, OpenIdConnectAuthenticationDefaults.AuthenticationType);
         }
     }
     public void SignOut()
     {
         // Send an OpenID Connect sign-out request.
         HttpContext.GetOwinContext().Authentication.SignOut(
              OpenIdConnectAuthenticationDefaults.AuthenticationType, CookieAuthenticationDefaults.AuthenticationType);
     }
     ```

3. Öffnen Sie „Views\Shared\_LoginPartial.cshtml“, um dem Benutzer die Links für An- und Abmeldung bei der App zu zeigen und den Namen des Benutzers in einer Ansicht auszugeben.

    ```HTML
    @if (Request.IsAuthenticated)
    {
     <text>
         <ul class="nav navbar-nav navbar-right">
             <li class="navbar-text">
                 Hello, @User.Identity.Name!
             </li>
             <li>
                 @Html.ActionLink("Sign out", "SignOut", "Account")
             </li>
         </ul>
     </text>
    }
    else
    {
     <ul class="nav navbar-nav navbar-right">
         <li>@Html.ActionLink("Sign in", "SignIn", "Account", routeValues: null, htmlAttributes: new { id = "loginLink" })</li>
     </ul>
    }
    ```

## <a name="step-4-display-user-information"></a>Schritt 4: Anzeigen der Benutzerinformationen
Bei der Authentifizierung von Benutzern mit OpenID Connect gibt Azure AD ein ID-Token an die App zurück, das die Ansprüche bzw. Assertionen des Benutzers enthält. Mithilfe dieser Ansprüche können Sie die App wie folgt personalisieren:

1. Öffnen Sie die Datei Controllers\HomeController.cs. Auf die Ansprüche des Benutzers können Sie in Ihren Controllern über das Sicherheitsprinzipalobjekt `ClaimsPrincipal.Current` zugreifen.

 ```C#
 public ActionResult About()
 {
     ViewBag.Name = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Name).Value;
     ViewBag.ObjectId = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
     ViewBag.GivenName = ClaimsPrincipal.Current.FindFirst(ClaimTypes.GivenName).Value;
     ViewBag.Surname = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Surname).Value;
     ViewBag.UPN = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Upn).Value;

     return View();
 }
 ```

2. Erstellen Sie die App, und führen Sie sie aus. Falls noch nicht geschehen, ist es nun Zeit, in Ihrem Mandanten einen neuen Benutzer mit einer „onmicrosoft.com“-Domäne zu erstellen. Das geht so:

  a. Melden Sie sich als dieser Benutzer an, und achten Sie darauf, wie dessen Identität auf der oberen Leiste angezeigt wird.

  b. Melden Sie sich ab, und melden Sie sich unter dem Namen eines neuen Benutzers wieder bei Ihrem Mandanten an.

  c. Wenn Sie besonders ambitioniert sind und noch mehr ausprobieren möchten, können Sie auch eine weitere Instanz dieser App (mit eigener Client-ID) registrieren und ausführen. Sie erleben dann die einmalige Anmeldung (SSO) in Aktion.

## <a name="next-steps"></a>Nächste Schritte
Als Referenz stellen wir [ das vollständige Beispiel](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip) (ohne Ihre Konfigurationswerte) bereit.

Sie können nun mit den Themen für fortgeschrittenere Benutzer fortfahren. Sehen Sie sich z.B. das [Schützen einer Web-API mit Azure AD](active-directory-devquickstarts-webapi-dotnet.md) an.

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]

