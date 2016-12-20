---
title: Apps, Berechtigungen und Zustimmung in Azure Active Directory | Microsoft-Dokumentation
description: "Azure AD Connect integriert Ihre lokalen Verzeichnisse in Azure Active Directory. Dadurch können Sie eine einzige Identität für in Azure AD integrierte in Office 365-, Azure- und SaaS-Anwendungen bereitstellen."
keywords: "Einführung in Azure AD, Apps, Was ist Azure AD Connect?, Active Directory installieren"
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 25897cc4-7687-49b6-b0d5-71f51302b6b1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/17/2016
ms.author: billmath
translationtype: Human Translation
ms.sourcegitcommit: 095497656a1c6ed1fe5322b6015de750d8fd6f15
ms.openlocfilehash: 3706ed729a525526a415d3a3367e6b8245ead812


---

# <a name="apps-permissions-and-consent-in-azure-active-directory"></a>Apps, Berechtigungen und Zustimmung in Azure Active Directory
In Azure Active Directory können Sie Ihrem Verzeichnis Anwendungen hinzufügen.  Die Anwendungen können je nach Art der Anwendung variieren.  Wählen Sie ein Verzeichnis und dann die Anwendungen aus, um sie im klassischen Portal anzuzeigen.

![](media/active-directory-apps-permissions-consent/apps1.png)


## <a name="types-of-apps"></a>App-Typen

1. **Apps mit nur einem Mandanten** </br>
    - **Apps mit nur einem Mandanten**: Apps dieser Art werden häufig als Branchen-Apps bezeichnet. Diese Bezeichnung trifft zu, wenn eine Person in Ihrer Organisation eine eigene App entwickelt und erreichen möchte, dass sich Benutzer der Organisation an der App anmelden können.
    ![](media/active-directory-apps-permissions-consent/apps2.png)
    - **Anwendungsproxy-Apps**: Wenn Sie eine lokale Anwendung mit einem Azure AD-Anwendungsproxy verfügbar machen, wird in Ihrem Mandanten eine App mit nur einem Mandanten registriert (zusätzlich zum Anwendungsproxydienst). Diese App stellt Ihre lokale Anwendung bei allen Cloudinteraktionen dar (z.B. bei der Authentifizierung). (Für den Anwendungsproxy ist Azure AD Basic oder höher erforderlich.)


2. **Mehrinstanzenfähige Apps**
    - **Mehrinstanzenfähige Apps, zu denen andere Benutzer ihre Zustimmung geben können**: Diese Apps ähneln den „von Ihrer Organisation entwickelten Apps für einzelne Mandanten“. Der Hauptunterschied besteht (neben der Logik in der App selbst) darin, dass Benutzer von anderen Mandanten auch ihre Zustimmung geben und sich an der App anmelden können.</br>
    ![](media/active-directory-apps-permissions-consent/apps4.png)
    - **Von anderen Benutzern entwickelte mehrinstanzenfähige Apps, zu denen Contoso seine Zustimmung geben kann** (kurz: „mit Zustimmung versehene Apps“). Dies ist die andere Seite der „von Ihrer Organisation entwickelten mehrinstanzenfähigen Apps“. Wenn eine mehrinstanzenfähige App von einer anderen Organisation entwickelt wird, können Benutzer Ihrer Organisation ihre Zustimmung zur App geben und sich an der App anmelden.
    - **Erstanbieter-Apps von Microsoft**: Dies sind Apps, die Microsoft-Dienste darstellen. Die Zustimmung wird erreicht, indem Sie sich für den Dienst anmelden. Für bestimmte Erstanbieter-Apps wird in einigen Fällen eine spezielle Benutzeroberfläche und Logik verwendet. Diese Elemente werden dann häufig genutzt, wenn Richtlinien zum Zugreifen auf die App eingerichtet werden.</br>
    ![](media/active-directory-apps-permissions-consent/apps3.png)
    - **Vorab integrierte Apps**: Dies sind Apps, die im Azure AD-App-Katalog verfügbar sind. Sie können sie Ihrem Verzeichnis hinzufügen, um das einmalige Anmelden (und ggf. die Bereitstellung) für beliebte SaaS-Apps zu ermöglichen.
    - **Azure AD – einmaliges Anmelden**: „Echtes“ einmaliges Anmelden für Apps, die in Azure AD integriert werden können, über ein unterstütztes Anmeldeprotokoll, z.B. SAML 2.0 oder OpenID Connect. Sie werden vom Assistenten durch die Einrichtung geführt.
    - **Einmaliges Anmelden per Kennwort**: In Azure AD werden die Anmeldeinformationen des Benutzers für die App sicher gespeichert, und die Anmeldeinformationen werden über die Browsererweiterung für den Azure AD-App-Zugriff in das Formular für die Anmeldung eingefügt. Dieser Vorgang wird auf Englisch auch als „Password Vaulting“ bezeichnet.

## <a name="permissions"></a>Berechtigungen

Wenn eine App registriert wird, definiert der Benutzer, von dem die App-Registrierung durchgeführt wird (also der Entwickler), auf welche Berechtigungen und welche Ressourcen die App Zugriff benötigt. (Die Ressourcen selbst werden als andere Apps definiert.) Wenn beispielsweise eine App zum Lesen von E-Mails erstellt wird, wird angegeben, dass für die App die Berechtigung „Access mailboxes as the signed-in user“ (Als angemeldeter Benutzer auf Postfächer zugreifen) unter der Ressource „Office 365 Exchange Online“ erforderlich ist:
    
![](media/active-directory-apps-permissions-consent/apps6.png)

Damit eine App (Client) eine bestimmte Berechtigung von einer anderen App (Ressource) anfordern kann, definiert der Entwickler der Ressourcen-App die vorhandenen Berechtigungen. In unserem Beispiel wurde von Microsoft, dem Besitzer der Ressourcen-App „Office 365 Exchange Online“, die Berechtigung „Access mailboxes as the signed-in user“ (Als angemeldeter Benutzer auf Postfächer zugreifen) definiert.

Beim Definieren von Berechtigungen muss der App-Entwickler festlegen, ob für die Berechtigung die Zustimmung erteilt werden kann oder ob die Zustimmung des Administrators erforderlich ist. So wird ermöglicht, dass Entwickler für Benutzer das eigenmächtige Erteilen der Zustimmung für Apps zulassen, bei denen es um weniger kritische Berechtigungen geht. Bei kritischen Berechtigungen müssen weiterhin Administratoren ihre Zustimmung erteilen. Beispielsweise wurde die Ressourcen-App „Azure Active Directory“ definiert, damit Benutzer ihre Zustimmung für Apps erteilen können, wenn eingeschränkte Leseberechtigungen angefordert werden.  Die Zustimmung durch den Administrator wird aber für vollständige Leseberechtigungen und alle Schreibberechtigungen benötigt.

Da native Clients nicht authentifiziert werden, können für eine App, die als native Client-App definiert wird, nur delegierte Berechtigungen angefordert werden. Dies bedeutet, dass beim Abrufen eines Tokens immer ein Benutzer beteiligt sein muss. Für Web-Apps und Web-APIs (vertrauliche Clients) muss die Authentifizierung beim Abrufen eines Zugriffstokens immer mit Azure AD durchgeführt werden. Es besteht also auch die Möglichkeit, dass nur App-Berechtigungen angefordert werden. Beispiel: Ein Back-End-Dienst muss sich gegenüber einem anderen Back-End-Dienst authentifizieren. Für Anwendungen, die Berechtigungen nur für die App anfordern, muss die Zustimmung immer durch den Administrator erteilt werden.

Zusammenfassung:



- Eine App (Client) gibt die Berechtigungen an, die sie für andere Apps (Ressourcen) benötigt.
- Eine App (Ressource) gibt an, welche Berechtigungen für andere Apps (Clients) verfügbar gemacht werden.
- Eine Berechtigung kann nur für die App gelten oder eine delegierte Berechtigung sein.
- Für eine delegierte Berechtigung kann „Zustimmung durch Benutzer zulässig“ oder „Zustimmung durch Administrator erforderlich“ angegeben werden.
- Eine App kann sich wie ein Client (deklariert, dass sie Berechtigungen für eine Ressource benötigt), eine Ressource (deklariert, welche Berechtigungen sie verfügbar macht) oder wie beides verhalten.

## <a name="controls"></a>Steuerelemente

Die folgende Liste enthält die verschiedenen Administratorsteuerelemente, die für diese Fälle verfügbar sind. Auf die Administratorsteuerelemente kann im klassischen Portal mit der Option „Konfigurieren“ unterhalb des Verzeichnisses zugegriffen werden.

![](media/active-directory-apps-permissions-consent/apps7.png)

Verwenden Sie im Azure-Portal die Option **Verwalten** > **Benutzereinstellungen**.

![](media/active-directory-apps-permissions-consent/apps11.png)



- Sie können steuern, ob Benutzer ihre Zustimmung für Apps erteilen können:

Wählen Sie im klassischen Portal die Option **Benutzer können Anwendungen die Berechtigung für den Zugriff auf ihre Daten geben**.
![](media/active-directory-apps-permissions-consent/apps8.png)

Wählen Sie im Azure-Portal die Option **Benutzer können Apps Zugriff auf ihre Daten gewähren**.
![](media/active-directory-apps-permissions-consent/apps12.png)



- Sie können steuern, ob Benutzer ihre eigenen Branchen-Apps mit nur einem Mandanten registrieren können: Wählen Sie im klassischen Portal die Option **Benutzer können integrierte Anwendungen hinzufügen**.
![](media/active-directory-apps-permissions-consent/apps9.png)

Wählen Sie im Azure-Portal die Option **Benutzer können Apps Zugriff auf ihre Daten gewähren**.
![](media/active-directory-apps-permissions-consent/apps13.png)

>[!NOTE]
>Auch wenn Sie für Benutzer das Registrieren von Branchen-Apps mit nur einem Mandanten zulassen, ist der Umfang der möglichen Registrierungen begrenzt.  
>Beispiel: Entwickler, die keine Verzeichnisadministratoren sind.
>
>- Benutzer können eine App mit nur einem Mandanten nicht in eine mehrinstanzenfähige App ändern.
>- Beim Registrieren von Branchen-Apps mit nur einem Mandanten können Benutzer keine App-Berechtigungen für einzelne andere Apps anfordern.
>- Beim Registrieren von Branchen-Apps mit nur einem Mandanten können Benutzer keine delegierten Berechtigungen für andere Apps anfordern, wenn für diese Berechtigungen die Zustimmung durch einen Administrator erforderlich ist.
>- Benutzer können keine Änderungen an Apps vornehmen, deren Besitzer sie nicht sind.



- Sie können steuern, ob Benutzer eigenmächtig vorab integrierte Apps hinzufügen können, für die das einmalige Anmelden per Kennwort („Password Vaulting“) verwendet wird. ![](media/active-directory-apps-permissions-consent/apps10.png)



- Sie können steuern, unter welchen Bedingungen auf Anwendungen zugegriffen werden kann (bedingter Zugriff). Beachten Sie, dass dies sowohl für die Client-App als auch für die Ressourcen-App gilt. Angenommen, Sie legen eine Richtlinie für bedingten Zugriff fest, die besagt, dass auf die App „Office 365 Exchange Online“ nur von Computern zugegriffen werden kann, die kompatibel sind.  Diese Richtlinie gilt auch, wenn ein Benutzer versucht, eine Client-App zu verwenden, die Berechtigungen für Exchange Online anfordert.



- Sie können verfolgen, für welche Apps die Zustimmung erteilt wurde und welche Apps verwendet werden.

1.  Wenn ein Benutzer die Zustimmung für eine App erteilt, wird im Mandanten ein ServicePrincipal-Objekt erstellt. Die ServicePrincipal-Erstellung ist im Überwachungsbericht enthalten.
2.  In den Berichten zur Anmeldeaktivität der Benutzer ist angegeben, an welcher App sich ein Benutzer anmeldet. 

## <a name="example"></a>Beispiel

Wir nehmen beispielsweise an, dass Sie verfolgt haben, dass sich Benutzer unter Ihrem Mandanten an der App „FabrikamMail für Office 365“ anmelden. „FabrikamMail“ ist eine App zum Lesen von E-Mails für Android, die von „Fabrikam, Inc.“ veröffentlicht wurde. Dies fällt in die Kategorie „Von anderen Benutzern entwickelte mehrinstanzenfähige Apps, zu denen Contoso seine Zustimmung geben kann“.

Falls für Benutzer das Erteilen der Zustimmung zulässig ist, wird nach dem ersten Anmelden eine Aufforderung zum Erteilen der Zustimmung angezeigt: ![](media/active-directory-apps-permissions-consent/apps14.png)

„Access your mailboxes“ (Auf Postfächer zugreifen) ist die für den Benutzer geltende Zustimmungszeichenfolge für die Berechtigung „Access mailboxes as the signed-in user“ (Als angemeldeter Benutzer auf Postfächer zugreifen), die von „Office 365 Exchange Online“ (also Exchange) verfügbar gemacht wird.

Sie können die Berechtigungen anzeigen, indem Sie das ServicePrincipal-Objekt für Exchange (die Ressource) nachschlagen, das beim Registrieren für Office 365 hinzugefügt wurde. Sie können sich das ServicePrincipal-Objekt als „Instanz“ der App auf Ihrem Mandanten vorstellen, die zum Aufzeichnen unterschiedlicher Optionen und Konfigurationen verwendet wird.  Hierfür können Sie `Get-AzureADServicePrincipal` in PowerShell verwenden.

    PS C:\> Get-AzureADServicePrincipal -ObjectId 383f7b97-6754-4d3d-9474-3908ebcba1c6 | fl *
    
    DeletionTimeStamp         : 
    ObjectId                  : 383f7b97-6754-4d3d-9474-3908ebcba1c6
    ObjectType                : ServicePrincipal
    AccountEnabled            : True
    AppDisplayName            : Office 365 Exchange Online
    AppId                     : 00000002-0000-0ff1-ce00-000000000000
    AppOwnerTenantId          : 
    AppRoleAssignmentRequired : False
    AppRoles                  : {...}
    DisplayName               : Microsoft.Exchange
    ErrorUrl                  : 
    Homepage                  : 
    KeyCredentials            : {}
    LogoutUrl                 : 
    Oauth2Permissions         : {...
                                , class OAuth2Permission {
                                  AdminConsentDescription : Allows the app to have the same access to mailboxes as the signed-in user via Exchange Web Services.
                                  AdminConsentDisplayName : Access mailboxes as the signed-in user via Exchange Web Services
                                  Id                      : 3b5f3d61-589b-4a3c-a359-5dd4b5ee5bd5
                                  IsEnabled               : True
                                  Type                    : User
                                  UserConsentDescription  : Allows the app full access to your mailboxes on your behalf.
                                  UserConsentDisplayName  : Access your mailboxes
                                  Value                   : full_access_as_user
                                },
                                ...}
    PasswordCredentials       : {}
    PublisherName             : 
    ReplyUrl                  : 
    SamlMetadataUrl           : 
    ServicePrincipalNames     : {00000002-0000-0ff1-ce00-000000000000/outlook.office365.com, 00000002-0000-0ff1-ce00-000000000000/mail.office365.com, 00000002-0000-0ff1-ce00-000000000000/outlook.com, 
                                00000002-0000-0ff1-ce00-000000000000/*.outlook.com...}
    Tags                      : {}

Die Zustimmung wird initiiert, wenn der Benutzer auf „Akzeptieren“ klickt. Zuerst wird ein ServicePrincipal-Objekt für „FabrikamMail für Office 365“ unter dem Mandanten erstellt. ServicePrincipal sieht etwa wie folgt aus:

    PS C:\> Get-AzureADServicePrincipal -SearchString "FabrikamMail for Office 365" | fl *
    
    DeletionTimeStamp         : 
    ObjectId                  : a8b16333-851d-42e8-acd2-eac155849b37
    ObjectType                : ServicePrincipal
    AccountEnabled            : True
    AppDisplayName            : FabrikamMail for Office 365
    AppId                     : aba7c072-2267-4031-8960-e7a2db6e0590
    AppOwnerTenantId          : 4a4076e0-a70f-41c6-b819-6f9c4a86df89
    AppRoleAssignmentRequired : False
    AppRoles                  : {}
    DisplayName               : FabrikamMail for Office 365
    ErrorUrl                  : 
    Homepage                  : 
    KeyCredentials            : {}
    LogoutUrl                 : 
    Oauth2Permissions         : {}
    PasswordCredentials       : {}
    PublisherName             : Fabrikam, Inc.
    ReplyUrl                  : 
    SamlMetadataUrl           : 
    ServicePrincipalNames     : {aba7c072-2267-4031-8960-e7a2db6e0590}
    Tags                      : {WindowsAzureActiveDirectoryIntegratedApp}

Beim Erteilen der Zustimmung für eine App wird eine Oauth2PermissionGrant-Verknüpfung zwischen den folgenden Elementen erstellt:
  
- Benutzerobjekt
- ServicePrincipalName (SPN) von Client-Apps
- ServicePrincipalName (SPN) von Ressourcen-Apps
- Berechtigungen in der Ressourcen-App  

Für FabrikamMail sieht dies wie folgt aus:

    PS C:\> Get-AzureADUserOAuth2PermissionGrant -ObjectId ddiggle@aadpremiumlab.onmicrosoft.com | fl *
    
    ClientId    : a8b16333-851d-42e8-acd2-eac155849b37
    ConsentType : Principal
    ExpiryTime  : 05/15/2017 07:02:39 AM
    ObjectId    : M2OxqB2F6EKs0urBVYSbN5d7PzhUZz1NlH25COvLocbJjoxkUFfRQauryBKwBWet
    PrincipalId : 648c8ec9-5750-41d1-abab-c812b00567ad
    ResourceId  : 383f7b97-6754-4d3d-9474-3908ebcba1c6
    Scope       : full_access_as_user
    StartTime   : 01/01/0001 12:00:00 AM

(**ClientId** ist die Dienstprinzipalobjekt-ID von FabrikamMail (gerade erstellt), **PrincipalId** ist die Benutzerobjekt-ID (des Benutzers, der die Zustimmung erteilt hat), **ResourceId** ist die Dienstprinzipalobjekt-ID von Exchange, und „Scope“ ist die Berechtigung in Exchange, für die die Zustimmung erteilt wurde.)

Falls das Erteilen der Zustimmung für Benutzer nicht zulässig ist, wird ein Hinweis angezeigt, dass die Berechtigung erforderlich ist.




<!--HONumber=Dec16_HO1-->


