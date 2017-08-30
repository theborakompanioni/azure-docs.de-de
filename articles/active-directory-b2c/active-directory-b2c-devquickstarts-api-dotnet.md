---
title: Azure AD B2C | Microsoft Docs
description: "Es wird beschrieben, wie Sie eine .NET-Web-API mithilfe von Azure Active Directory B2C erstellen, die mit OAuth 2.0-Zugriffstoken für die Authentifizierung geschützt ist."
services: active-directory-b2c
documentationcenter: .net
author: parakhj
manager: krassk
editor: 
ms.assetid: 7146ed7f-2eb5-49e9-8d8b-ea1a895e1966
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 03/17/2017
ms.author: parakhj
ms.translationtype: Human Translation
ms.sourcegitcommit: f6006d5e83ad74f386ca23fe52879bfbc9394c0f
ms.openlocfilehash: 48749bfa2ab54a0e766a4aad4f39073cc4e90818
ms.contentlocale: de-de
ms.lasthandoff: 05/03/2017

---
# <a name="azure-active-directory-b2c-build-a-net-web-api"></a>Azure Active Directory B2C: Erstellen einer .NET-Web-API

Mit Azure Active Directory (Azure AD) B2C können Sie eine Web-API mithilfe von OAuth 2.0-Zugriffstoken sichern. Mit diesen Token können sich Ihre Client-Apps gegenüber der API authentifizieren. In diesem Artikel erfahren Sie, wie Sie eine .NET MVC-API für eine Aufgabenliste erstellen, mit der Benutzer Ihrer Clientanwendung CRUD-Vorgänge für Aufgaben durchführen können. Die Web-API wird mit Azure AD B2C geschützt, und nur authentifizierte Benutzer können ihre Aufgabenliste verwalten.

## <a name="create-an-azure-ad-b2c-directory"></a>Erstellen eines Azure AD B2C-Verzeichnisses

Bevor Sie Azure AD B2C verwenden können, müssen Sie ein Verzeichnis oder einen Mandanten erstellen. Ein Verzeichnis ist ein Container für all Ihre Benutzer, Apps, Gruppen usw. Wenn Sie noch keines verwenden, sollten Sie [ein B2C-Verzeichnis erstellen](active-directory-b2c-get-started.md) , bevor Sie die weiteren Schritte in diesem Leitfaden ausführen.

> [!NOTE]
> Die Clientanwendung und die Web-API müssen dasselbe Azure AD B2C-Verzeichnis verwenden.
>

## <a name="create-a-web-api"></a>Erstellen einer Web-API

Als Nächstes müssen Sie in Ihrem B2C-Verzeichnis eine Web-API-App erstellen. Dadurch werden Azure AD die Informationen bereitgestellt, die für die sichere Kommunikation mit Ihrer App erforderlich sind. Befolgen Sie zum Erstellen einer App [diese Anweisungen](active-directory-b2c-app-registration.md). Führen Sie folgende Schritte aus:

* Fügen Sie der Anwendung eine **Web-App** oder **Web-API** hinzu.
* Verwenden Sie den **Umleitungs-URI** `https://localhost:44332/` für die Web-App. Dies ist der Standardspeicherort des Web-App-Clients für dieses Codebeispiel.
* Kopieren Sie die **Anwendungs-ID** , die Ihrer App zugewiesen ist. Sie benötigen sie später.
* Geben Sie einen App-Bezeichner in das Feld **App-ID-URI** ein.
* Fügen Sie über das Menü **Published scopes** (Veröffentlichte Bereiche) Berechtigungen hinzu.

  [!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Erstellen der Richtlinien

In Azure AD B2C wird jede Benutzererfahrung durch eine [Richtlinie](active-directory-b2c-reference-policies.md)definiert. Es ist erforderlich, eine Richtlinie für die Kommunikation mit Azure AD B2C zu erstellen. Wir empfehlen Ihnen die Verwendung einer kombinierten Richtlinie für die Registrierung und Anmeldung, wie im [Artikel zu Richtlinienreferenzen](active-directory-b2c-reference-policies.md) beschrieben. Achten Sie beim Erstellen der Richtlinie auf Folgendes:

* Wählen Sie den **Anzeigenamen** und andere Registrierungsattribute in der Richtlinie aus.
* Wählen Sie den **Anzeigenamen** und die **Objekt-ID** als Anwendungsansprüche für jede Richtlinie aus. Sie können auch andere Ansprüche auswählen.
* Notieren Sie sich die **Namen** der einzelnen Richtlinien nach ihrer Erstellung. Sie benötigen den Richtliniennamen später noch.

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Nachdem Sie die Richtlinie erfolgreich erstellt haben, können Sie Ihre App erstellen.

## <a name="download-the-code"></a>Herunterladen des Codes

Der Code für dieses Tutorial wird auf [GitHub](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi) verwaltet. Sie können das Beispiel klonen, indem Sie Folgendes ausführen:

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
```

Nachdem Sie den Beispielcode heruntergeladen haben, öffnen Sie zum Einstieg die Visual Studio-SLN-Datei. Die Projektmappe enthält jetzt zwei Projekte: `TaskWebApp` und `TaskService`. `TaskWebApp` ist eine MVC-Webanwendung, mit der der Benutzer interagiert. `TaskService` ist die Back-End-Web-API der App, in der die Aufgabenlisten der einzelnen Benutzer gespeichert werden. In diesem Artikel wird nur die Anwendung `TaskService` beschrieben. Informationen zum Erstellen einer `TaskWebApp` mit Azure AD B2C finden Sie in unserem [.NET-Web-App-Tutorial](active-directory-b2c-devquickstarts-web-dotnet-susi.md).

### <a name="update-the-azure-ad-b2c-configuration"></a>Aktualisieren der Azure AD B2C-Konfiguration

Unser Beispiel ist so konfiguriert, dass die Richtlinien und die Client-ID unseres Demomandanten verwendet werden. Gehen Sie wie folgt vor, wenn Sie Ihren eigenen Mandanten verwenden möchten:

1. Öffnen Sie `web.config` im Projekt `TaskService`, und ersetzen Sie die Werte wie folgt:
    * `ida:Tenant` durch Ihren Mandantennamen
    * `ida:ClientId` durch Ihre Anwendungs-ID der Web-API
    * `ida:SignUpSignInPolicyId` durch Ihren Namen für die Richtlinie zur Registrierung bzw. Anmeldung

2. Öffnen Sie `web.config` im Projekt `TaskWebApp` und ersetzen Sie die Werte wie folgt:
    * `ida:Tenant` durch Ihren Mandantennamen
    * `ida:ClientId` durch Ihre Anwendungs-ID der Web-App
    * `ida:ClientSecret` durch Ihren geheimen Schlüssel der Web-App
    * `ida:SignUpSignInPolicyId` durch Ihren Namen für die Richtlinie zur Registrierung bzw. Anmeldung
    * `ida:EditProfilePolicyId` durch den Namen der Richtlinie für „Profil bearbeiten“
    * `ida:ResetPasswordPolicyId` durch den Namen der Richtlinie für „Kennwort zurücksetzen“


## <a name="secure-the-api"></a>Sichern der API

Wenn Sie über einen Client verfügen, mit dem Ihre API aufgerufen wird, können Sie die API (z.B. `TaskService`) mit OAuth 2.0-Bearertoken schützen. So wird sichergestellt, dass jede an die API gesendete Anforderung nur gültig ist, wenn sie über ein Bearertoken verfügt. Die API kann Bearertoken mit der OWIN-Bibliothek von Microsoft (Open Web Interface for .NET) akzeptieren und überprüfen.

### <a name="install-owin"></a>Installieren von OWIN

Installieren Sie zunächst die OWIN OAuth-Authentifizierungspipeline über die Visual Studio-Paket-Manager-Konsole.

```Console
PM> Install-Package Microsoft.Owin.Security.OAuth -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Security.Jwt -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TaskService
```

Hierbei wird die OWIN-Middleware installiert, die Bearertoken akzeptieren und überprüfen kann.

### <a name="add-an-owin-startup-class"></a>Hinzufügen einer OWIN-Startklasse

Fügen Sie der API eine OWIN-Startklasse mit dem Namen `Startup.cs` hinzu.  Klicken Sie mit der rechten Maustaste auf das Projekt, wählen Sie **Hinzufügen** und **Neues Element** aus, und suchen Sie dann nach „OWIN“. Die OWIN-Middleware ruft beim Starten Ihrer Anwendung die Methode `Configuration(…)` auf.

In unserem Beispiel haben wir die Klassendeklaration in `public partial class Startup` geändert und den anderen Teil der Klasse in `App_Start\Startup.Auth.cs` implementiert. Innerhalb der `Configuration`-Methode haben wir einen Aufruf von `ConfigureAuth` hinzugefügt, der in `Startup.Auth.cs` definiert ist. Nach den Änderungen sieht `Startup.cs` wie folgt aus:

```CSharp
// Startup.cs

public partial class Startup
{
    // The OWIN middleware will invoke this method when the app starts
    public void Configuration(IAppBuilder app)
    {
        // ConfigureAuth defined in other part of the class
        ConfigureAuth(app);
    }
}
```

### <a name="configure-oauth-20-authentication"></a>Konfigurieren der OAuth 2.0-Authentifizierung

Öffnen Sie die Datei `App_Start\Startup.Auth.cs`, und implementieren Sie die `ConfigureAuth(...)`-Methode. Sie kann beispielsweise wie folgt aussehen:

```CSharp
// App_Start\Startup.Auth.cs

 public partial class Startup
    {
        // These values are pulled from web.config
        public static string AadInstance = ConfigurationManager.AppSettings["ida:AadInstance"];
        public static string Tenant = ConfigurationManager.AppSettings["ida:Tenant"];
        public static string ClientId = ConfigurationManager.AppSettings["ida:ClientId"];
        public static string SignUpSignInPolicy = ConfigurationManager.AppSettings["ida:SignUpSignInPolicyId"];
        public static string DefaultPolicy = SignUpSignInPolicy;

        /*
         * Configure the authorization OWIN middleware.
         */
        public void ConfigureAuth(IAppBuilder app)
        {
            TokenValidationParameters tvps = new TokenValidationParameters
            {
                // Accept only those tokens where the audience of the token is equal to the client ID of this app
                ValidAudience = ClientId,
                AuthenticationType = Startup.DefaultPolicy
            };

            app.UseOAuthBearerAuthentication(new OAuthBearerAuthenticationOptions
            {
                // This SecurityTokenProvider fetches the Azure AD B2C metadata & signing keys from the OpenIDConnect metadata endpoint
                AccessTokenFormat = new JwtFormat(tvps, new OpenIdConnectCachingSecurityTokenProvider(String.Format(AadInstance, Tenant, DefaultPolicy)))
            });
        }
    }
```

### <a name="secure-the-task-controller"></a>Sichern des Aufgabencontrollers

Nachdem Sie die App für die OAuth 2.0-Authentifizierung konfiguriert haben, können Sie Ihre Web-API sichern, indem Sie dem Aufgabencontroller ein `[Authorize]`-Tag hinzufügen. Dies ist der Controller, in dem die gesamte Bearbeitung der Aufgabenliste durchgeführt wird. Daher wird der gesamte Controller auf Klassenebene gesichert. Zur präziseren Steuerung können Sie das `[Authorize]`-Tag auch einzelnen Aktionen hinzufügen.

```CSharp
// Controllers\TasksController.cs

[Authorize]
public class TasksController : ApiController
{
    ...
}
```

### <a name="get-user-information-from-the-token"></a>Abrufen von Benutzerinformationen aus dem Token

`TasksController` speichert Aufgaben in einer Datenbank, wobei jeder Aufgabe ein Benutzer zugeordnet ist, der die Aufgabe „besitzt“. Der Besitzer wird durch die **Objekt-ID**des Benutzers identifiziert. (Daher mussten Sie die Objekt-ID als Anwendungsanspruch in all Ihren Richtlinien hinzufügen.)

```CSharp
// Controllers\TasksController.cs

public IEnumerable<Models.Task> Get()
{
    string owner = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
    IEnumerable<Models.Task> userTasks = db.Tasks.Where(t => t.owner == owner);
    return userTasks;
}
```

### <a name="validate-the-permissions-in-the-token"></a>Überprüfen der Berechtigungen im Token

Eine häufige Anforderung an Web-APIs ist das Überprüfen der im Token vorhandenen „Bereiche“. Hierdurch wird sichergestellt, dass der Benutzer den für den Zugriff auf den „TodoListService“ erforderlichen Berechtigungen zugestimmt hat.

```CSharp
public IEnumerable<Models.Task> Get()
{
    if (ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/scope").Value != "read")
    {
        throw new HttpResponseException(new HttpResponseMessage {
            StatusCode = HttpStatusCode.Unauthorized,
            ReasonPhrase = "The Scope claim does not contain 'read' or scope claim not found"
        });
    }
    ...
}
```

## <a name="run-the-sample-app"></a>Ausführen der Beispiel-App

Zum Schluss erstellen Sie `TaskWebApp` und `TaskService` und führen diese Elemente aus. Erstellen Sie einige Aufgaben in der Aufgabenliste des Benutzers, und beachten Sie, wie sie in der API beibehalten werden, auch nachdem Sie den Client beendet und neu gestartet haben.

## <a name="edit-your-policies"></a>Bearbeiten der Richtlinien

Nachdem Sie eine API mit Azure AD B2C gesichert haben, können Sie mit Ihrer Richtlinie für die Registrierung bzw. Anmeldung experimentieren und die Auswirkungen (sofern vorhanden) auf die API beobachten. Sie können die Anwendungsansprüche in den Richtlinien bearbeiten und die Benutzerinformationen ändern, die in der Web-API verfügbar sind. Ansprüche, die Sie hinzufügen, stehen Ihrer .NET MVC-Web-API im `ClaimsPrincipal` -Objekt zur Verfügung, wie oben in diesem Artikel beschrieben.

