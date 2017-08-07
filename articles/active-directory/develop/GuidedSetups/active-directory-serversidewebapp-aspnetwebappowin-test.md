---
title: "Azure AD v2 – ASP.NET-Webserver: Erste Schritte – Test | Microsoft-Dokumentation"
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
ms.custom: aaddev
ms.translationtype: Human Translation
ms.sourcegitcommit: ef74361c7a15b0eb7dad1f6ee03f8df707a7c05e
ms.openlocfilehash: 00cb963e85111274c36c3a84489894811ad2dabd
ms.contentlocale: de-de
ms.lasthandoff: 05/25/2017


---
## <a name="test-your-code"></a>Testen Ihres Codes

Drücken Sie `F5`, um das Projekt in Visual Studio auszuführen. Der Browser wird geöffnet und leitet Sie zu *http://localhost:{port}*, wo Sie die Schaltfläche *Mit Microsoft anmelden* sehen. Fahren Sie fort, indem Sie darauf klicken, um sich anzumelden.

Wenn Sie zum Testen bereit sind, verwenden Sie ein Geschäfts-, Schul- oder Unikonto (Azure Active Directory) oder ein persönliches Konto (live.com, outlook.com), um sich anzumelden. 

![Browserfenster „Mit Microsoft anmelden“](media/active-directory-serversidewebapp-aspnetwebappowin-test/aspnetbrowsersignin.png)

![Browserfenster „Mit Microsoft anmelden“](media/active-directory-serversidewebapp-aspnetwebappowin-test/aspnetbrowsersignin2.png)

#### <a name="expected-results"></a>Erwartete Ergebnisse
Nach der Anmeldung wird der Benutzer zur Startseite Ihrer Website umgeleitet. Dabei handelt es sich um die HTTPS-URL, die im Microsoft-Anwendungsregistrierungsportal in Ihre Anwendungsregistrierungsinformationen eingegeben wurde. Diese Seite zeigt jetzt *Hello {Benutzer}*, einen Link zum Abmelden und einen Link zum Anzeigen der Ansprüche des Benutzers. Hierbei handelt es sich um einen Link zum „Authorize“-Controller, den Sie zuvor erstellt haben.

### <a name="see-users-claims"></a>Anzeigen der Ansprüche des Benutzers
Klicken Sie auf den Link, um die Ansprüche des Benutzers anzuzeigen. Dadurch gelangen Sie zum Controller und zur Ansicht, die nur Benutzern zur Verfügung steht, die authentifiziert wurden.

#### <a name="expected-results"></a>Erwartete Ergebnisse
 Es sollte eine Tabelle angezeigt werden, die die Standardeigenschaften des angemeldeten Benutzers enthält:

| Eigenschaft | Wert | Beschreibung|
|---|---|---|
| Name | {Vollständiger Name des Benutzers} | Vor- und Nachname des Benutzers
|Benutzername | <span>user@domain.com</span>| Benutzername zur Identifizierung des angemeldeten Benutzers
| Antragsteller| {Antragsteller}|Zeichenfolge zur eindeutigen Identifizierung der Benutzeranmeldung über das Web|
| Mandanten-ID| {GUID}| Eine *GUID*, die die Azure Active Directory-Organisation des Benutzers eindeutig darstellt.|

Darüber hinaus sehen Sie eine Tabelle mit allen Benutzeransprüchen, die in der Authentifizierungsanforderung enthalten sind. Eine Liste aller Ansprüche in einem ID-Token und eine Erläuterung finden Sie in [diesem Artikel](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims "Liste der Ansprüche im ID-Token").


### <a name="test-accessing-a-method-that-has-an-authorize-attribute-optional"></a>Testen des Zugriffs auf eine Methode mit einem *[Authorize]*-Attribut (optional)
In diesem Schritt testen Sie den Zugriff auf den „Authenticated“-Controller als anonymer Benutzer:<br/>
Klicken Sie auf den Link zum Abmelden des Benutzers, und schließen Sie den Abmeldevorgang ab.<br/>
Geben Sie nun http://localhost:{port}/authenticated in Ihren Browser ein, um auf Ihren Controller zuzugreifen, der durch das `[Authorize]`-Attribut geschützt ist.

#### <a name="expected-results"></a>Erwartete Ergebnisse
Sie sollten zum Anmelden aufgefordert werden, um die Ansicht anzuzeigen.

## <a name="additional-information"></a>Zusätzliche Informationen

<!--start-collapse-->
### <a name="protect-your-entire-web-site"></a>Schützen der gesamten Website
Zum Schutz der gesamten Website fügen Sie in der Methode `Global.asax` `Application_Start` `AuthorizeAttribute` zu `GlobalFilters` hinzu:

```csharp
GlobalFilters.Filters.Add(new AuthorizeAttribute());
```
<!--end-collapse-->

<div></div>
<br/>

> [!NOTE]
> **Beschränken der Anmeldung bei Ihrer Anwendung auf ausschließlich Benutzer in einer Organisation**

> Standardmäßig können sich sowohl persönliche Konten (z.B. outlook.com, live.com u.a.) als auch Geschäfts,- Schul- und Unikonten von Unternehmen oder Organisationen, die in Azure Active Directory integriert wurden, bei Ihrer Anwendung anmelden. 

> Wenn Sie möchten, dass Ihre Anwendung nur Anmeldungen aus einer Azure Active Directory-Organisation akzeptiert, ersetzen Sie den `Tenant`-Parameter `Common` in *web.config* durch den Mandantennamen der Organisation, z.B. *contoso.onmicrosoft.com*. Ändern Sie danach das `ValidateIssuer`-Argument in Ihrer *OWIN-Klasse „Startup“* in `true`.

> Um nur Benutzer in einer Liste angegebener Organisationen zuzulassen, legen Sie `ValidateIssuer` auf TRUE fest, und legen Sie mithilfe des `ValidIssuers`-Parameters eine Liste mit Organisationen fest.

> Eine andere Möglichkeit ist das Implementieren einer benutzerdefinierten Methode, um mit dem „IssuerValidator“-Parameter Aussteller zu überprüfen. Weitere Informationen zu `TokenValidationParameters` finden Sie in [diesem](https://msdn.microsoft.com/library/system.identitymodel.tokens.tokenvalidationparameters.aspx "MSDN-Artikel zu TokenValidationParameters") MSDN-Artikel.


