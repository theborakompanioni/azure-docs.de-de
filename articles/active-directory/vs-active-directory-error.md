---
title: "Fehler während der Authentifizierungserkennung"
description: "Der Verbindungs-Assistent für Active Directory hat einen inkompatiblen Authentifizierungstyp erkannt."
services: active-directory
documentationcenter: 
author: TomArcher
manager: douge
editor: 
ms.assetid: dd89ea63-4e45-4da1-9642-645b9309670a
ms.service: active-directory
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 11/18/2016
ms.author: tarcher
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 7c031d6e04c26151b9e6e25fbca8acb6bbbdb12d


---
# <a name="error-during-authentication-detection"></a>Fehler während der Authentifizierungserkennung
Beim Erkennen des vorherigen Authentifizierungscodes hat der Assistent einen nicht kompatiblen Authentifizierungstyp erkannt.   

## <a name="what-is-being-checked"></a>Was wird überprüft?
**Hinweis:** Um vorherigen Authentifizierungscode in einem Projekt ordnungsgemäß erkennen zu können, muss das Projekt erstellt werden.  Wenn dieser Fehler auftritt und in Ihrem Projekt kein vorheriger Authentifizierungscode enthalten ist, erstellen Sie Ihr Projekt neu, und versuchen Sie es nochmals.

### <a name="project-types"></a>Projekttypen
Der Assistent überprüft, welche Art von Projekt Sie entwickeln, um die richtige Authentifizierungslogik in das Projekt einzufügen.  Wenn ein Controller im Projekt von `ApiController` abgeleitet wird, gilt das Projekt als WebAPI-Projekt.  Wenn alle Controller im Projekt von `MVC.Controller` abgeleitet werden, gilt das Projekt als MVC-Projekt.  Alles andere wird vom Assistenten nicht unterstützt.  WebForms-Projekte werden derzeit nicht unterstützt.

### <a name="compatible-authentication-code"></a>Kompatibler Authentifizierungscode
Der Assistent überprüft auch Authentifizierungseinstellungen, die zuvor mit dem Assistenten konfiguriert wurden oder mit dem Assistenten kompatibel sind.  Wenn alle Einstellungen vorhanden sind, gilt dies als eintrittsinvarianter Fall, und der Assistent wird geöffnet und zeigt die Einstellungen an.  Wenn nur einige der Einstellungen vorhanden sind, wird es als Fehlerfall betrachtet.

In einem MVC-Projekt überprüft der Assistent die folgenden Einstellungen, die aus der vorherigen Verwendung des Assistenten resultieren:

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:AADInstance" value="" />
    <add key="ida:PostLogoutRedirectUri" value="" />

Außerdem überprüft der Assistent die folgenden Einstellungen in einem Web-API-Projekt, die aus der vorherigen Verwendung des Assistenten resultieren:

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:Audience" value="" />

### <a name="incompatible-authentication-code"></a>Nicht kompatibler Authentifizierungscode
Der Assistent versucht schließlich, Versionen von Authentifizierungscode zu erkennen, die mit früheren Versionen von Visual Studio konfiguriert wurden. Wenn Sie diesen Fehler erhalten, ist ein nicht kompatibler Authentifizierungstyp in Ihrem Projekt vorhanden. Der Assistent erkennt die folgenden Authentifizierungstypen aus früheren Versionen von Visual Studio:

* Windows-Authentifizierung 
* Einzelne Benutzerkonten 
* Organisationskonten 

Zum Erkennen der Windows-Authentifizierung in einem MVC-Projekt sucht der Assistent nach dem `authentication` -Element aus Ihrer Datei **web.config** .

<pre>
    &lt;configuration&gt;
        &lt;system.web&gt;
            <span style="background-color: yellow">&lt;authentication mode="Windows" /&gt;</span>
        &lt;/system.web&gt;
    &lt;/configuration&gt;
</pre>

Zum Erkennen der Windows-Authentifizierung in einem Web-API-Projekt sucht der Assistent nach dem `IISExpressWindowsAuthentication` -Element aus der **CSPROJ** -Datei Ihres Projekts:

<pre>
    &lt;Project&gt;
        &lt;PropertyGroup&gt;
            <span style="background-color: yellow">&lt;IISExpressWindowsAuthentication&gt;enabled&lt;/IISExpressWindowsAuthentication&gt;</span>
        &lt;/PropertyGroup> &lt;/Project&gt;
</pre>

Zum Erkennen der Authentifizierung einzelner Benutzerkonten sucht der Assistent nach dem Paketelement aus Ihrer Datei **Packages.config** .

<pre>
    &lt;packages&gt;
        <span style="background-color: yellow">&lt;package id="Microsoft.AspNet.Identity.EntityFramework" version="2.1.0" targetFramework="net45" /&gt;</span>
    &lt;/packages&gt;
</pre>

Zum Erkennen der alten Form von Organisationskontoauthentifizierung sucht der Assistent nach dem folgenden Element aus der Datei **web.config**:

<pre>
    &lt;configuration&gt;
        &lt;appSettings&gt;
            <span style="background-color: yellow">&lt;add key="ida:Realm" value="***" /&gt;</span>
        &lt;/appSettings&gt;
    &lt;/configuration&gt;
</pre>

Wenn Sie den Authentifizierungstyp ändern möchten, entfernen Sie den inkompatiblen Authentifizierungstyp, und führen Sie den Assistenten dann erneut aus.

Weitere Informationen finden Sie unter [Authentifizierungsszenarien für Azure AD](active-directory-authentication-scenarios.md).




<!--HONumber=Nov16_HO3-->


