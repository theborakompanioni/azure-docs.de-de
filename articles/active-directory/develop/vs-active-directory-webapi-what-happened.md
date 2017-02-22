---
title: "Änderungen an einem WebApi-Projekt, die vorgenommen werden, wenn Sie eine Verbindung mit Azure AD herstellen | Microsoft-Dokumentation"
description: "Beschreibt, was mit dem WebApi-Projekt geschieht, wenn Sie mithilfe von Visual Studio eine Verbindung mit Azure AD herstellen."
services: active-directory
documentationcenter: 
author: TomArcher
manager: douge
editor: 
ms.assetid: 57630aee-26a2-4326-9dbb-ea2a66daa8b0
ms.service: active-directory
ms.workload: web
ms.tgt_pltfrm: vs-what-happened
ms.devlang: na
ms.topic: article
ms.date: 11/18/2016
ms.author: tarcher
translationtype: Human Translation
ms.sourcegitcommit: f3f8292eb505c73b5fda86499581fe85ad3f8e47
ms.openlocfilehash: aa055ead3dc039745f48f268fe456ae5dab5bf36


---
# <a name="what-happened-to-my-webapi-project-visual-studio-azure-active-directory-connected-service"></a>Was ist mit dem WebApi-Projekt passiert (verbundene Visual Studio-Dienste für Azure Active Directory)?
> [!div class="op_single_selector"]
> * [Erste Schritte](vs-active-directory-webapi-getting-started.md)
> * [Was ist passiert?](vs-active-directory-webapi-what-happened.md)
> 
> 

## <a name="references-have-been-added"></a>Verweise wurden hinzugefügt
### <a name="nuget-package-references"></a>NuGet-Paketverweise
* `Microsoft.Owin`
* `Microsoft.Owin.Host.SystemWeb`
* `Microsoft.Owin.Security`
* `Microsoft.Owin.Security.ActiveDirectory`
* `Microsoft.Owin.Security.Jwt`
* `Microsoft.Owin.Security.OAuth`
* `Owin`
* `System.IdentityModel.Tokens.Jwt`

### <a name="net-references"></a>.NET-Verweise
* `Microsoft.Owin`
* `Microsoft.Owin.Host.SystemWeb`
* `Microsoft.Owin.Security`
* `Microsoft.Owin.Security.ActiveDirectory`
* `Microsoft.Owin.Security.Jwt`
* `Microsoft.Owin.Security.OAuth`
* `Owin`
* `System.IdentityModel.Tokens.Jwt`

## <a name="code-changes"></a>Änderungen am Code
### <a name="code-files-were-added-to-your-project"></a>Ihrem Projekt wurden Codedateien hinzugefügt
Eine Authentifizierungsstartklasse (**App_Start/Startup.Auth.cs**) wurde Ihrem Projekt hinzugefügt. Sie enthält Startlogik für die Azure AD-Authentifizierung.

### <a name="startup-code-was-added-to-your-project"></a>Ihrem Projekt wurde Startcode hinzugefügt
Wenn Sie bereits eine Startklasse in Ihrem Projekt verwendet haben, wurde die **Configuration**-Methode so aktualisiert, dass sie einen Aufruf von `ConfigureAuth(app)` enthält. Andernfalls wurde Ihrem Projekt eine Startklasse hinzugefügt.

### <a name="your-appconfig-or-webconfig-file-has-new-configuration-values"></a>Ihre Datei app.config oder web.config weist neue Konfigurationswerte auf.
Die folgenden Konfigurationseinträge wurden hinzugefügt.

```
    <appSettings>
            <add key="ida:ClientId" value="ClientId from the new Azure AD App" />
            <add key="ida:Tenant" value="Your selected Azure AD Tenant" />
            <add key="ida:Audience" value="The App ID Uri from the wizard" />
    </appSettings>`
```

### <a name="an-azure-ad-app-was-created"></a>Eine Azure AD-App wurde erstellt
Eine Azure AD-Anwendung wurde in dem Verzeichnis erstellt, das Sie im Assistenten ausgewählt haben.

[Weitere Informationen zu Azure Active Directory](https://azure.microsoft.com/services/active-directory/)

## <a name="if-i-checked-disable-individual-user-accounts-authentication-what-additional-changes-were-made-to-my-project"></a>Wenn ich *das Deaktivieren der Authentifizierung für einzelne Benutzerkonten*ausgewählt habe, welche zusätzlichen Änderungen wurden an meinem Projekt vorgenommen?
NuGet-Paketverweise wurden entfernt, und die Dateien wurden entfernt und gesichert. Abhängig vom Status des Projekts müssen Sie möglicherweise manuell zusätzliche Verweise oder Dateien entfernen oder Code entsprechend ändern.

### <a name="nuget-package-references-removed-for-those-present"></a>NuGet-Paketverweise entfernt (die vorhanden waren)
* `Microsoft.AspNet.Identity.Core`
* `Microsoft.AspNet.Identity.EntityFramework`
* `Microsoft.AspNet.Identity.Owin`

### <a name="code-files-backed-up-and-removed-for-those-present"></a>Codedateien gesichert und entfernt (die vorhanden waren)
Jede der folgenden Dateien wurde gesichert und aus dem Projekt entfernt. Sicherungsdateien befinden sich in einem Ordner "Backup" im Stammverzeichnis des Projektverzeichnisses.

* `App_Start\IdentityConfig.cs`
* `Controllers\AccountController.cs`
* `Controllers\ManageController.cs`
* `Models\IdentityModels.cs`
* `Providers\ApplicationOAuthProvider.cs`

### <a name="code-files-backed-up-for-those-present"></a>Codedateien gesichert (die vorhanden waren)
Jede der folgenden Dateien wurde gesichert, bevor sie ersetzt wurde. Sicherungsdateien befinden sich in einem Ordner "Backup" im Stammverzeichnis des Projektverzeichnisses.

* `Startup.cs`
* `App_Start\Startup.Auth.cs`

## <a name="if-i-checked-read-directory-data-what-additional-changes-were-made-to-my-project"></a>Wenn ich *Verzeichnisdaten lesen*aktiviert habe, welche zusätzlichen Änderungen wurden an meinem Projekt vorgenommen?
### <a name="additional-changes-were-made-to-your-appconfig-or-webconfig"></a>An "app.config" oder "web.config" wurden zusätzliche Änderungen vorgenommen
Die folgenden zusätzlichen Konfigurationseinträge wurden hinzugefügt.

```
    <appSettings>
        <add key="ida:Password" value="Your Azure AD App's new password" />
    </appSettings>`
```

### <a name="your-azure-active-directory-app-was-updated"></a>Ihre Azure Active Directory-App (AD) wurde aktualisiert
Ihre Azure Active Directory-App wurde aktualisiert und enthält nun die Berechtigung *Verzeichnisdaten lesen*. Außerdem wurde ein zusätzlicher Schlüssel erstellt, der als *ida:Password* in der Datei `web.config` verwendet wird.

[Weitere Informationen zu Azure Active Directory](https://azure.microsoft.com/services/active-directory/)




<!--HONumber=Jan17_HO5-->


