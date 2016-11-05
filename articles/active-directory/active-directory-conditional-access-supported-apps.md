
---
title: Bedingter Zugriff – unterstützte Anwendungen | Microsoft Docs
description: Mit der bedingten Zugriffssteuerung überprüft Azure Active Directory die besonderen Bedingungen, die Sie beim Authentifizieren des Benutzers und vor dem Gewähren des Zugriffs auf die Anwendung auswählen. Nachdem diese Bedingungen erfüllt sind, wird der Benutzer authentifiziert und erhält Zugriff auf die Anwendung.
services: active-directory
documentationcenter: ''
author: markusvi
manager: femila
editor: ''

ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/26/2016
ms.author: markvi

---
# <a name="conditional-access-support-for-applications"></a>Unterstützung des bedingten Zugriffs für Anwendungen
Regeln für bedingten Zugriff werden übergreifend für verbundene Azure Active Directory-Anwendungen, für vorab integrierte SaaS-Verbundanwendungen, für Anwendungen, die einmaliges Anmelden mit Kennwort verwenden, sowie für Branchenanwendungen und für den Azure AD-Anwendungsproxy unterstützt. Eine detaillierte Liste mit Anwendungen, für die der bedingte Zugriff aktiviert werden kann, finden Sie unter [Dienste mit aktiviertem bedingtem Zugriff](active-directory-conditional-access-technical-reference.md#Services-enabled-with-conditional-access). Der bedingte Zugriff kann sowohl für mobile Anwendungen als auch für Desktopanwendungen mit modernen Authentifizierungsverfahren verwendet werden. In diesem Thema erfahren Sie, was hinsichtlich der mobilen und der desktopbasierten Version dieser Apps unterstützt wird.

Anwendungen mit moderner Authentifizierung können Azure AD-Anmeldeseiten anzeigen. Dadurch können Benutzer inline aufgefordert werden, sich per MFA (Multi-Factor Authentication, mehrstufige Authentifizierung) zu authentifizieren, oder Endbenutzern können Meldungen angezeigt werden, wenn der Zugriff blockiert wird. Eine moderne Authentifizierung ist ebenfalls für das Gerät erforderlich, um eine Authentifizierung bei Azure AD zu ermöglichen, sodass gerätebasierte Richtlinien für bedingten Zugriff ausgewertet werden können.

Es ist wichtig zu wissen, welche Anwendungen unterstützt werden, und die Schritte zu kennen, die möglicherweise zum Sichern weiterer Zugangspunkte erforderlich sind.

## <a name="applications-using-modern-authentication"></a>Anwendung mit moderner Authentifizierung
Die folgenden Anwendungen unterstützen den bedingten Zugriff beim Zugreifen auf Office 365 und andere mit Azure AD verbundene Dienstanwendungen:

| Zieldienst | Plattform | Anwendung |
| --- | --- | --- |
| Microsoft Office 365 Exchange Online |Windows 10 |Mail-/Kalender-/Kontakte-App, Outlook 2016, Outlook 2013 (mit aktivierter moderner Authentifizierung), Skype for Business (mit moderner Authentifizierung) |
| Microsoft Office 365 Exchange Online |Windows 7, Windows 8.1 |Outlook 2016, Outlook 2013 (mit aktivierter moderner Authentifizierung), Skype for Business (mit moderner Authentifizierung) |
| Microsoft Office 365 Exchange Online |iOS, Android |Outlook Mobile-App |
| Microsoft Office 365 Exchange Online |Mac OSX |Outlook 2016 nur für MFA/Standort, geplante Unterstützung von gerätebasierten Richtlinien, geplante Skype for Business-Unterstützung |
| Office 365 SharePoint Online |Windows 10 |Office 2016-Apps, Office Universal-Apps, Office 2013 (mit aktivierter moderner Authentifizierung), geplante Unterstützung für OneDrive for Business-App (NGSC oder Synchronisierungsclient der nächsten Generation), geplante Unterstützung für Office-Gruppen, geplante Unterstützung der SharePoint-App |
| Office 365 SharePoint Online |Windows 7, Windows 8.1 |Office 2016-Apps, Office 2013 (mit aktivierter moderner Authentifizierung), OneDrive for Business-App (Groove-Synchronisierungsclient) |
| Office 365 SharePoint Online |iOS, Android |Office Mobile-Apps |
| Office 365 SharePoint Online |Mac OSX |Office 2016-Apps nur für MFA/Standort, geplante Unterstützung für gerätebasierte Richtlinien |
| Office 365 Yammer |Windows 10, iOS und Android |Office Yammer-App |
| Dynamics CRM |Windows 10, 7, 8.1, iOS und Android |Dynamics CRM-App |
| PowerBI-Dienst |Windows 10, 7, 8.1, iOS und Android |Power BI-App |
| Azure Remote-App-Dienst |Windows 10, 7, 8.1, iOS und Android, Mac OSX |Azure RemoteApp |
| Alle Meine Apps-App-Dienste |Android und iOS |Alle Meine Apps-App-Dienste |

## <a name="applications-that-do-not-use-modern-authentication"></a>Anwendungen ohne moderne Authentifizierung
Zurzeit muss für Apps, die keine moderne Authentifizierungsverfahren verwenden, der Zugriff mithilfe anderer Methoden blockiert werden, da der bedingte Zugriff für diese nicht erzwungen wird. Dieser Aspekt gilt hauptsächlich für den Zugriff auf Exchange und SharePoint, da vorherige App-Versionen unter Verwendung älterer Protokolle erstellt wurden.

## <a name="office-365-sharepoint-online"></a>Office 365 SharePoint Online
Bei SharePoint können ältere Protokolle mithilfe des Cmdlets Set-SPOTenant deaktiviert werden. Dieses Cmdlet verhindert, dass Office-Clients, die keine modernen Authentifizierungsprotokolle verwenden, auf SharePoint Online-Ressourcen zugreifen. 

**Beispielbefehl:**    `Set-SPOTenant -LegacyAuthProtocolsEnabled $false`

## <a name="office-365-exchange-online"></a>Microsoft Office 365 Exchange Online
In Exchange gibt es zwei Hauptkategorien von Protokollen. Überprüfen und wählen Sie die richtige Richtlinie für Ihre Organisation:

1. Exchange ActiveSync. Eine Richtlinie für bedingten Zugriff für MFA und Standort wird standardmäßig für Exchange ActiveSync nicht erzwungen. Dadurch kann der Zugriff entweder durch direktes Konfigurieren der Exchange ActiveSync-Richtlinie oder durch Blockieren von Exchange ActiveSync mithilfe von AD FS-Regeln geschützt werden.
2. Ältere Protokolle. Ältere Protokolle können mit AD FS blockiert werden. Damit wird der Zugriff für ältere Office-Clients wie z.B. Office 2013 ohne moderne Authentifizierung und frühere Versionen blockiert.

### <a name="example-ad-fs-rules"></a>Beispiele für AD FS-Regeln
Mithilfe der folgenden Regeln kann der Zugriff über ältere Protokolle in AD FS in zwei allgemeinen Konfigurationen blockiert werden.

### <a name="option-1:-allow-exchange-activesync-and-only-allow-legacy-apps-on-the-intranet"></a>Option 1: Exchange ActiveSync zulassen und ältere Apps nur im Intranet zulassen
Durch Anwenden der folgenden drei Regeln in der AD FS-Vertrauensstellung der vertrauenden Seite für die Microsoft Office 365 Identity Platform wird Exchange ActiveSync-Datenverkehr zugelassen, ebenso wie Browserdatenverkehr und mit moderner Authentifizierung geschützter Datenverkehr. Der Zugriff älterer Apps auf das Extranet wird blockiert. 

Regel 1

    `@RuleName = “Allow all intranet traffic”
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "true"] 
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");`

Regel 2

    @RuleName = “Allow EAS”
    c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value == "Microsoft.Exchange.ActiveSync"] 
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

Regel 3

    @RuleName = “Allow Extranet browser or browser dialog traffic”
    c1:[Type == " http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] && 
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"] 
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

### <a name="option-2:-allow-exchange-activesync-and-block-legacy-apps"></a>Option 2: Exchange ActiveSync zulassen und ältere Apps blockieren
Durch Anwenden der folgenden drei Regeln in der AD FS-Vertrauensstellung der vertrauenden Seite für die Microsoft Office 365 Identity Platform wird Exchange ActiveSync-Datenverkehr zugelassen, ebenso wie Browserdatenverkehr und mit moderner Authentifizierung geschützter Datenverkehr. Der Zugriff älterer Apps wird an jedem Standort blockiert. 

Regel 1

    @RuleName = “Allow all intranet traffic only for browser and modern authentication clients”
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "true"] && 
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"] 
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");


Regel 2 

    @RuleName = “Allow EAS”
    c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value == "Microsoft.Exchange.ActiveSync"] 
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");


Regel 3 

    @RuleName = “Allow Extranet browser or browser dialog traffic”
    c1:[Type == " http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] && 
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"] 
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");











<!--HONumber=Oct16_HO2-->


