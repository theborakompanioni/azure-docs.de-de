---
title: "Änderungen an einem MVC-Projekt, die vorgenommen werden, wenn Sie eine Verbindung mit Azure AD herstellen | Microsoft-Dokumentation"
description: "Beschreibt, was mit dem MVC-Projekt geschieht, wenn Sie mithilfe von verbundenen Visual Studio-Diensten eine Verbindung mit Azure AD herstellen "
services: active-directory
documentationcenter: na
author: TomArcher
manager: douge
editor: 
ms.assetid: 8b24adde-547e-4ffe-824a-2029ba210216
ms.service: active-directory
ms.workload: web
ms.tgt_pltfrm: vs-what-happened
ms.devlang: na
ms.topic: article
ms.date: 03/01/2017
ms.author: tarcher
ms.custom: aaddev
ms.translationtype: Human Translation
ms.sourcegitcommit: 0d8472cb3b0d891d2b184621d62830d1ccd5e2e7
ms.openlocfilehash: 72cd94ba16cb4fe234c898b093c7de6a08f71239
ms.contentlocale: de-de
ms.lasthandoff: 03/21/2017


---
# <a name="what-happened-to-my-mvc-project-visual-studio-azure-active-directory-connected-service"></a>Was ist mit dem MVC-Projekt passiert (verbundene Visual Studio-Dienste für Azure Active Directory)?
> [!div class="op_single_selector"]
> * [Erste Schritte](vs-active-directory-dotnet-getting-started.md)
> * [Was ist passiert?](vs-active-directory-dotnet-what-happened.md)
> 
> 

## <a name="references-have-been-added"></a>Verweise wurden hinzugefügt
### <a name="nuget-package-references"></a>NuGet-Paketverweise
* **Microsoft.IdentityModel.Protocol.Extensions**
* **Microsoft.Owin**
* **Microsoft.Owin.Host.SystemWeb**
* **Microsoft.Owin.Security**
* **Microsoft.Owin.Security.Cookies**
* **Microsoft.Owin.Security.OpenIdConnect**
* **Owin**
* **System.IdentityModel.Tokens.Jwt**

### <a name="net-references"></a>.NET-Verweise
* **Microsoft.IdentityModel.Protocol.Extensions**
* **Microsoft.Owin**
* **Microsoft.Owin.Host.SystemWeb**
* **Microsoft.Owin.Security**
* **Microsoft.Owin.Security.Cookies**
* **Microsoft.Owin.Security.OpenIdConnect**
* **Owin**
* **System.IdentityModel**
* **System.IdentityModel.Tokens.Jwt**
* **System.Runtime.Serialization**

## <a name="code-has-been-added"></a>Code wurde hinzugefügt
### <a name="code-files-were-added-to-your-project"></a>Ihrem Projekt wurden Codedateien hinzugefügt
Eine Authentifizierungsstartklasse (**App_Start/Startup.Auth.cs**) wurde Ihrem Projekt hinzugefügt. Sie enthält Startlogik für die Azure AD-Authentifizierung. Außerdem wurde eine Controllerklasse (Controllers/AccountController.cs) hinzugefügt, die **SignIn()**- und **SignOut()**-Methoden enthält. Schließlich wurde noch eine Teilansicht (**Views/Shared/_LoginPartial.cshtml**) hinzugefügt, die einen Aktionslink für „SignIn/SignOut“ enthält.

### <a name="startup-code-was-added-to-your-project"></a>Ihrem Projekt wurde Startcode hinzugefügt
Wenn Sie bereits eine Startklasse in Ihrem Projekt verwendet haben, wurde die **Configuration**-Methode so aktualisiert, dass sie einen Aufruf von **ConfigureAuth(app)** enthält. Andernfalls wurde Ihrem Projekt eine Startklasse hinzugefügt.

### <a name="your-appconfig-or-webconfig-has-new-configuration-values"></a>Ihre Datei app.config oder web.config weist neue Konfigurationswerte auf
Die folgenden Konfigurationseinträge wurden hinzugefügt.

    <appSettings>
        <add key="ida:ClientId" value="ClientId from the new Azure AD App" />
        <add key="ida:AADInstance" value="https://login.microsoftonline.com/" />
        <add key="ida:Domain" value="The selected Azure AD Domain" />
        <add key="ida:TenantId" value="The Id of your selected Azure AD Tenant" />
        <add key="ida:PostLogoutRedirectUri" value="Your project start page" />
    </appSettings>

### <a name="an-azure-active-directory-ad-app-was-created"></a>Eine Azure Active Directory-App (AD) wurde erstellt
Eine Azure AD-Anwendung wurde in dem Verzeichnis erstellt, das Sie im Assistenten ausgewählt haben.

## <a name="if-i-checked-disable-individual-user-accounts-authentication-what-additional-changes-were-made-to-my-project"></a>Wenn ich *das Deaktivieren der Authentifizierung für einzelne Benutzerkonten*ausgewählt habe, welche zusätzlichen Änderungen wurden an meinem Projekt vorgenommen?
NuGet-Paketverweise wurden entfernt, und die Dateien wurden entfernt und gesichert. Abhängig vom Status des Projekts müssen Sie möglicherweise manuell zusätzliche Verweise oder Dateien entfernen oder Code entsprechend ändern.

### <a name="nuget-package-references-removed-for-those-present"></a>NuGet-Paketverweise entfernt (die vorhanden waren)
* **Microsoft.AspNet.Identity.Core**
* **Microsoft.AspNet.Identity.EntityFramework**
* **Microsoft.AspNet.Identity.Owin**

### <a name="code-files-backed-up-and-removed-for-those-present"></a>Codedateien gesichert und entfernt (die vorhanden waren)
Jede der folgenden Dateien wurde gesichert und aus dem Projekt entfernt. Sicherungsdateien befinden sich in einem Ordner "Backup" im Stammverzeichnis des Projektverzeichnisses.

* **App_Start\IdentityConfig.cs**
* **Controllers\ManageController.cs**
* **Models\IdentityModels.cs**
* **Models\ManageViewModels.cs**

### <a name="code-files-backed-up-for-those-present"></a>Codedateien gesichert (die vorhanden waren)
Jede der folgenden Dateien wurde gesichert, bevor sie ersetzt wurde. Sicherungsdateien befinden sich in einem Ordner "Backup" im Stammverzeichnis des Projektverzeichnisses.

* **Startup.cs**
* **App_Start\Startup.Auth.cs**
* **Controllers\AccountController.cs**
* **Views\Shared\_LoginPartial.cshtml**

## <a name="if-i-checked-read-directory-data-what-additional-changes-were-made-to-my-project"></a>Wenn ich *Verzeichnisdaten lesen*aktiviert habe, welche zusätzlichen Änderungen wurden an meinem Projekt vorgenommen?
Zusätzliche Verweise wurden hinzugefügt.

### <a name="additional-nuget-package-references"></a>Zusätzliche NuGet-Paketverweise
* **EntityFramework**
* **Microsoft.Azure.ActiveDirectory.GraphClient**
* **Microsoft.Data.Edm**
* **Microsoft.Data.OData**
* **Microsoft.Data.Services.Client**
* **Microsoft.IdentityModel.Clients.ActiveDirectory**
* **System.Spatial**

### <a name="additional-net-references"></a>Zusätzliche .NET-Verweise
* **EntityFramework**
* **EntityFramework.SqlServer**
* **Microsoft.Azure.ActiveDirectory.GraphClient**
* **Microsoft.Data.Edm**
* **Microsoft.Data.OData**
* **Microsoft.Data.Services.Client**
* **Microsoft.IdentityModel.Clients.ActiveDirectory**
* **Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms**
* **System.Spatial**

### <a name="additional-code-files-were-added-to-your-project"></a>Ihrem Projekt wurden zusätzliche Codedateien hinzugefügt
Zur Unterstützung der Zwischenspeicherung von Token wurden zwei Dateien hinzugefügt: **Models\ADALTokenCache.cs** und **Models\ApplicationDbContext.cs**.  Ein zusätzlicher Controller und eine Ansicht wurden hinzugefügt, um den Zugriff auf Benutzerprofilinformationen mithilfe von Azure Graph-APIs zu veranschaulichen.  Bei diesen Dateien handelt es sich um **Controllers\UserProfileController.cs** und **Views\UserProfile\Index.cshtml**.

### <a name="additional-startup-code-was-added-to-your-project"></a>Ihrem Projekt wurde zusätzlicher Startcode hinzugefügt
In der Datei **startup.auth.cs** wurde dem Mitglied **Notifications** von **OpenIdConnectAuthenticationOptions** ein neues **OpenIdConnectAuthenticationNotifications**-Objekt hinzugefügt.  Auf diese Weise wird der Empfang des OAuth-Codes und dessen Austausch gegen ein Zugriffstoken aktiviert.

### <a name="additional-changes-were-made-to-your-appconfig-or-webconfig"></a>An "app.config" oder "web.config" wurden zusätzliche Änderungen vorgenommen
Die folgenden zusätzlichen Konfigurationseinträge wurden hinzugefügt.

    <appSettings>
        <add key="ida:ClientSecret" value="Your Azure AD App's new client secret" />
    </appSettings>

Die folgenden Konfigurationsabschnitte und eine Verbindungszeichenfolge wurden hinzugefügt.

    <configSections>
        <!-- For more information on Entity Framework configuration, visit http://go.microsoft.com/fwlink/?LinkID=237468 -->
        <section name="entityFramework" type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=6.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" requirePermission="false" />
    </configSections>
    <connectionStrings>
        <add name="DefaultConnection" connectionString="Data Source=(localdb)\MSSQLLocalDB;AttachDbFilename=|DataDirectory|\aspnet-[AppName + Generated Id].mdf;Initial Catalog=aspnet-[AppName + Generated Id];Integrated Security=True" providerName="System.Data.SqlClient" />
    </connectionStrings>
    <entityFramework>
        <defaultConnectionFactory type="System.Data.Entity.Infrastructure.LocalDbConnectionFactory, EntityFramework">
          <parameters>
            <parameter value="mssqllocaldb" />
          </parameters>
        </defaultConnectionFactory>
        <providers>
          <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
        </providers>
    </entityFramework>


### <a name="your-azure-active-directory-app-was-updated"></a>Ihre Azure Active Directory-App (AD) wurde aktualisiert
Ihre Azure Active Directory-App wurde aktualisiert und enthält nun die Berechtigung *Verzeichnis lesen*. Außerdem wurde ein zusätzlicher Schlüssel erstellt, der als *ida:ClientSecret* in der Datei **web.config** verwendet wird.

## <a name="next-steps"></a>Nächste Schritte
- [Weitere Informationen zu Azure Active Directory](https://azure.microsoft.com/services/active-directory/)


