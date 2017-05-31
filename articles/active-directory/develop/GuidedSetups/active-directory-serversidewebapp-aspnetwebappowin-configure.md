---
title: "Azure AD v2 – ASP.NET-Webserver: Erste Schritte – Konfiguration | Microsoft-Dokumentation"
description: "Informationen zum Implementieren einer Microsoft-Anmeldung in einer ASP.NET-Projektmappe mittels einer herkömmlichen webbrowserbasierten Anwendung unter Verwendung des Standards OpenID Connect"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 6f50ced708cb79a39e190657e4720c515c09990f
ms.contentlocale: de-de


---

## <a name="create-an-application-express"></a>Erstellen einer Anwendung (Express)
Nun müssen Sie Ihre Anwendung im *Microsoft-Anwendungsregistrierungsportal* registrieren:
1. Registrieren Sie Ihre Anwendung im [Microsoft-Anwendungsregistrierungsportal](https://apps.dev.microsoft.com/portal/register-app?appType=serverSideWebApp&appTech=aspNetWebAppOwin&step=configure).
2.    Geben Sie einen Namen für Ihre Anwendung und Ihre E-Mail-Adresse ein.
3.    Stellen Sie sicher, dass die Option für geführtes Setup aktiviert ist.
4.    Befolgen Sie die Anweisungen, um Ihrer Anwendung eine Umleitungs-URL hinzufügen.

## <a name="add-your-application-registration-information-to-your-solution-advanced"></a>Hinzufügen der Registrierungsinformationen Ihrer Anwendung zur Projektmappe (für Fortgeschrittene)
Nun müssen Sie Ihre Anwendung im *Microsoft-Anwendungsregistrierungsportal* registrieren:
1. Registrieren Sie Ihre Anwendung im [Microsoft-Anwendungsregistrierungsportal](https://apps.dev.microsoft.com/portal/register-app).
2. Geben Sie einen Namen für Ihre Anwendung und Ihre E-Mail-Adresse ein. 
3.    Stellen Sie sicher, dass die Option für geführtes Setup deaktiviert ist.
4.    Klicken Sie auf `Add Platforms`, und wählen Sie dann `Web` aus.
5.    Kehren Sie zurück zu Visual Studio, und wählen Sie im Projektmappen-Explorer das Projekt aus. Untersuchen Sie das Fenster „Eigenschaften“ (wenn es nicht angezeigt wird, drücken Sie F4).
6.    Ändern Sie „SSL-fähig“ in `True`:<br/><br/>![Projekteigenschaften](media/active-directory-serversidewebapp-aspnetwebappowin-configure/vsprojectproperties.png)<br />

7.    Kopieren Sie die SSL-URL, und fügen Sie diese URL zur Liste der Umleitungs-URLs im Registrierungsportal hinzu.
8.    Fügen Sie im Stammordner der Datei `web.config` im Abschnitt `configuration\appSettings` Folgendes hinzu:

```xml
<add key="ClientId" value="Enter_the_Application_Id_here" />
<add key="redirectUri" value="Enter_the_Redirect_URL_here" />
<add key="Tenant" value="common" />
<add key="Authority" value="https://login.microsoftonline.com/{0}/v2.0" /> 
```
9. Ersetzen Sie `ClientId` durch die Anwendungs-ID, die Sie gerade registriert haben.
10. Ersetzen Sie `redirectUri` durch die SSL-URL Ihres Projekts. 
<!-- End Docs -->

> Hinweis
> ### <a name="restricting-users-from-only-one-organization-to-sign-in-to-your-application"></a>Beschränken der Anmeldung bei Ihrer Anwendung auf ausschließlich Benutzer in einer Organisation
> Standardmäßig können sich sowohl persönliche Konten (z.B. outlook.com, live.com u.a.) als auch Geschäfts,- Schul- und Unikonten von Unternehmen oder Organisationen, die in Azure Active Directory integriert wurden, bei Ihrer Anwendung anmelden. Wenn Sie möchten, dass Ihre Anwendung nur Anmeldungen aus einer Organisation akzeptiert, ersetzen Sie den `Tenant`-Parameter in `web.config` `Common` durch den Mandantennamen der Organisation, z.B. `contoso.onmicrosoft.com`. Ändern Sie danach das *ValidateIssuer*-Argument in Ihrer OWIN-Klasse „Startup“ in `true`.
Um nur Benutzer in einer Liste angegebener Organisationen zuzulassen, legen Sie `ValidateIssuer` auf `true` fest, und legen Sie mithilfe des `ValidIssuers`-Parameters eine Liste mit Organisationen fest.
Eine andere Möglichkeit ist das Implementieren einer benutzerdefinierten Methode, um mit `IssuerValidator parameter` die Aussteller zu überprüfen. Weitere Informationen zu `TokenValidationParameters` finden Sie in [diesem MSDN-Artikel](https://msdn.microsoft.com/en-us/library/system.identitymodel.tokens.tokenvalidationparameters(v=vs.114).aspx).


