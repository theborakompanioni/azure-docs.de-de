---
title: "Anwendungen, die Regeln für den bedingten Zugriff in Azure Active Directory verwenden | Microsoft Docs"
description: "Mithilfe der bedingten Zugriffssteuerung führt Azure Active Directory bei der Benutzerauthentifizierung und vor dem Gewähren von Anwendungszugriff eine Überprüfung auf besondere Bedingungen durch."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 62349fba-3cc0-4ab5-babe-372b3389eff6
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/24/2017
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: b9605eb10f87f107dffd9f631d043bf550cf030b
ms.openlocfilehash: 5e293d7d3fa6b0a763663a5428878944660fc03e


---
# <a name="applications-that-use-conditional-access-rules-in-azure-active-directory"></a>Anwendungen, die Regeln für den bedingten Zugriff in Azure Active Directory verwenden
Regeln für den bedingten Zugriff werden in mit Azure Active Directory (Azure AD) verbundenen Anwendungen, in vorab integrierten SaaS-Verbundanwendungen (Software-as-a-Service), in Anwendungen mit einmaliger Anmeldung (SSO) per Kennwort sowie für Branchenanwendungen und für Anwendungen unterstützt, die den Azure AD-Anwendungsproxy verwenden. Eine detaillierte Liste der Anwendungen, für die der bedingte Zugriff verwendet werden kann, finden Sie unter [Dienste mit aktiviertem bedingtem Zugriff](active-directory-conditional-access-technical-reference.md). Der bedingte Zugriff kann sowohl für mobile Anwendungen als auch für Desktopanwendungen mit modernen Authentifizierungsverfahren verwendet werden. In diesem Artikel wird erläutert, wie der bedingte Zugriff in mobilen Apps und in Desktop-Apps funktioniert.

Sie können in Anwendungen mit moderner Authentifizierung Azure AD-Anmeldeseiten verwenden. Bei Verwendung einer Anmeldeseite wird der Benutzer zur mehrstufigen Authentifizierung aufgefordert. Es wird eine Meldung angezeigt, wenn der Benutzerzugriff blockiert wird. Eine moderne Authentifizierung für das Gerät ist für die Authentifizierung bei Azure AD erforderlich, damit die gerätebasierten Richtlinien für den bedingten Zugriff ausgewertet werden.

Es ist wichtig zu wissen, welche Anwendungen Regeln für den bedingten Zugriff verwenden können, und die Schritte zu kennen, die möglicherweise zum Schutz anderer Anwendungseinstiegspunkte durchgeführt werden müssen.

## <a name="applications-that-use-modern-authentication"></a>Anwendungen mit moderner Authentifizierung
> [!NOTE]
> Wenn Sie in Azure AD über eine Richtlinie für den bedingten Zugriff verfügen, für die eine Entsprechung in Office 365 vorhanden ist, konfigurieren Sie beide Richtlinien für den bedingten Zugriff. Dies gilt beispielsweise für Richtlinien für den bedingten Zugriff für Exchange Online oder SharePoint Online.
>
>

Die folgenden Anwendungen unterstützen den bedingten Zugriff für Office 365 und andere mit Azure AD verbundene Dienstanwendungen:

| Zieldienst | Plattform | Anwendung |
| --- | --- | --- |
| Microsoft Office 365 Exchange Online |Windows 10 |Mail-/Kalender-/Kontakte-App, Outlook 2016, Outlook 2013 (mit moderner Authentifizierung), Skype for Business (mit moderner Authentifizierung) |
| Microsoft Office 365 Exchange Online |Windows 8.1, Windows 7 |Outlook 2016, Outlook 2013 (mit moderner Authentifizierung), Skype for Business (mit moderner Authentifizierung) |
| Microsoft Office 365 Exchange Online |iOS, Android |Outlook Mobile-App |
| Microsoft Office 365 Exchange Online |Mac OS X |Outlook 2016 nur für mehrstufige Authentifizierung und Standort; geplante Unterstützung für gerätebasierte Richtlinien, geplante Unterstützung für Skype for Business |
| Office 365 SharePoint Online |Windows 10 |Office 2016-Apps, universelle Office-Apps, Office 2013 (mit moderner Authentifizierung), OneDrive-Synchronisierungsclient (siehe [Hinweise](https://support.office.com/en-US/article/Azure-Active-Directory-conditional-access-with-the-OneDrive-sync-client-on-Windows-028d73d7-4b86-4ee0-8fb7-9a209434b04e)), geplante Unterstützung für Office-Gruppen, geplante Unterstützung der SharePoint-App |
| Office 365 SharePoint Online |Windows 8.1, Windows 7 |Office 2016-Apps, Office 2013 (mit moderner Authentifizierung), OneDrive-Synchronisierungsclient (siehe [Hinweise](https://support.office.com/en-US/article/Azure-Active-Directory-conditional-access-with-the-OneDrive-sync-client-on-Windows-028d73d7-4b86-4ee0-8fb7-9a209434b04e)) |
| Office 365 SharePoint Online |iOS, Android |Office Mobile-Apps |
| Office 365 SharePoint Online |Mac OS X |Outlook 2016 nur für mehrstufige Authentifizierung und Standort; geplante Unterstützung für gerätebasierte Richtlinien |
| Office 365 Yammer |Windows 10, iOS und Android |Office Yammer-App |
| Dynamics CRM |Windows 10, Windows 8.1, Windows 7, iOS, Android |Dynamics CRM-App |
| PowerBI-Dienst |Windows 10, Windows 8.1, Windows 7, iOS, Android |Power BI-App |
| Azure Remote-App-Dienst |Windows 10, Windows 8.1, Windows 7, iOS, Android, Mac OS X |Azure RemoteApp |
| Alle Meine Apps-App-Dienste |Android und iOS |Alle Meine Apps-App-Dienste |

## <a name="applications-that-do-not-use-modern-authentication"></a>Anwendungen ohne moderne Authentifizierung
Aktuell müssen Sie andere Methoden verwenden, um den Zugriff auf Apps ohne moderne Authentifizierung zu blockieren. Zugriffsregeln für Apps ohne moderne Authentifizierung werden nicht durch den bedingten Zugriff erzwungen. Dies ist vor allem eine Überlegung für den Exchange- und SharePoint-Zugriff. Die meisten früheren App-Versionen verwenden ältere Protokolle für die Zugriffssteuerung.

### <a name="control-access-in-office-365-sharepoint-online"></a>Zugriffssteuerung in Office 365 SharePoint Online
Ältere Protokolle für den SharePoint-Zugriff können mithilfe des Cmdlets „Set-SPOTenant“ deaktiviert werden. Verwenden Sie dieses Cmdlet, um Office-Clients ohne modernen Authentifizierungsprotokolle daran zu hindern, auf SharePoint Online-Ressourcen zuzugreifen.

**Beispielbefehl:**    `Set-SPOTenant -LegacyAuthProtocolsEnabled $false`

### <a name="control-access-in-office-365-exchange-online"></a>Zugriffssteuerung in Office 365 Exchange Online
Exchange stellt zwei Hauptkategorien von Protokollen bereit. Sehen Sie sich die folgenden Optionen an, und wählen Sie dann die für Ihre Organisation geeignete Richtlinie aus.

* **Exchange ActiveSync**: Für Exchange ActiveSync werden per Voreinstellung keine Richtlinien für den bedingten Zugriff für MFA und Standort erzwungen. Sie müssen den Zugriff auf diese Dienste entweder durch direktes Konfigurieren einer Exchange ActiveSync-Richtlinie oder durch das Blockieren von Exchange ActiveSync mithilfe von Regeln für die Active Directory-Verbunddienste (AD FS) schützen.
* **Ältere Protokolle**: Sie können ältere Protokolle mit AD FS blockieren. Damit wird der Zugriff für ältere Office-Clients wie z.B. Office 2013 ohne moderne Authentifizierung und frühere Versionen von Office blockiert.

### <a name="use-ad-fs-to-block-legacy-protocol"></a>Einsatz von AD FS zum Blockieren von älteren Protokollen
Sie können die folgenden Beispielregeln zum Blockieren des Zugriffs über ältere Protokolle auf AD FS-Ebene verwenden. Wählen Sie aus zwei gängigen Konfigurationen.

#### <a name="option-1-allow-exchange-activesync-and-allow-legacy-apps-but-only-on-the-intranet"></a>Option 1: Exchange ActiveSync zulassen und ältere Apps nur im Intranet zulassen
Durch Anwenden der folgenden drei Regeln auf die AD FS-Vertrauensstellung der vertrauenden Seite für die Microsoft Office 365 Identity Platform wird Exchange ActiveSync-Datenverkehr zugelassen, ebenso wie Browserdatenverkehr und mit moderner Authentifizierung geschützter Datenverkehr. Der Zugriff älterer Apps auf das Extranet wird blockiert.

##### <a name="rule-1"></a>Regel 1
    @RuleName = "Allow all intranet traffic"
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "true"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

##### <a name="rule-2"></a>Regel 2
    @RuleName = "Allow Exchange ActiveSync"
    c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value == "Microsoft.Exchange.ActiveSync"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

##### <a name="rule-3"></a>Regel 3
    @RuleName = "Allow extranet browser and browser dialog traffic"
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

#### <a name="option-2-allow-exchange-activesync-and-block-legacy-apps"></a>Option 2: Exchange ActiveSync zulassen und ältere Apps blockieren
Durch Anwenden der folgenden drei Regeln auf die AD FS-Vertrauensstellung der vertrauenden Seite für die Microsoft Office 365 Identity Platform wird Exchange ActiveSync-Datenverkehr zugelassen, ebenso wie Browserdatenverkehr und mit moderner Authentifizierung geschützter Datenverkehr. Der Zugriff älterer Apps wird von jedem Standort blockiert.

##### <a name="rule-1"></a>Regel 1
    @RuleName = "Allow all intranet traffic only for browser and modern authentication clients"
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "true"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");


##### <a name="rule-2"></a>Regel 2
    @RuleName = "Allow Exchange ActiveSync"
    c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value == "Microsoft.Exchange.ActiveSync"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");


##### <a name="rule-3"></a>Regel 3
    @RuleName = "Allow extranet browser and browser dialog traffic"
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");



<!--HONumber=Jan17_HO2-->


