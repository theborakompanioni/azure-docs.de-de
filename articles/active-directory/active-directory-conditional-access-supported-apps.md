
<properties
	pageTitle="Bedingter Zugriff – unterstützte Anwendungen | Microsoft Azure"
	description="Mit der bedingten Zugriffssteuerung überprüft Azure Active Directory die besonderen Bedingungen, die Sie beim Authentifizieren des Benutzers und vor dem Gewähren des Zugriffs auf die Anwendung auswählen. Nachdem diese Bedingungen erfüllt sind, wird der Benutzer authentifiziert und erhält Zugriff auf die Anwendung."
    services="active-directory"
	documentationCenter=""
	authors="femila"
	manager="swadhwa"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.devlang="na"
	ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
	ms.date="06/15/2016"
	ms.author="femila"/>


# Unterstützung des bedingten Zugriffs für Anwendungen
Der bedingte Zugriff funktioniert für mobile und Desktopanwendungen, die moderne Authentifizierungsverfahren verwenden. Anwendungen mit moderner Authentifizierung können Azure AD-Anmeldeseiten anzeigen. Dadurch können Benutzer inline aufgefordert werden, sich per MFA (Multi-Factor Authentication, mehrstufige Authentifizierung) zu authentifizieren, oder Endbenutzern können Meldungen angezeigt werden, wenn der Zugriff blockiert wird. Es ist wichtig zu wissen, welche Anwendungen unterstützt werden, und die Schritte zu kennen, die möglicherweise zum Sichern weiterer Zugangspunkte erforderlich sind.

## Anwendung mit moderner Authentifizierung
Die folgenden Anwendungen wurden mit MFA und einer auf den Zieldienst festgelegten Standortrichtlinie getestet.

| Anwendung | Zieldienst | Plattform |
|--------------|-----------------|----------------------------------------------------------------|
| Outlook 2016 | Exchange | Windows 10, Windows Mobile 10, Windows 8.1, Windows 7, Mac |
| Outlook 2013 (erfordert die Aktivierung der modernen Authentifizierung)| Exchange |Windows 10, Windows Mobile 10, Windows 8.1, Windows 7|
|Skype for Business (mit moderner Authentifizierung)|Exchange (Zugriff auf Exchange für den Kalender- und Unterhaltungsverlauf)| Windows 10, Windows 8.1, Windows 7 |
|Outlook Mobile-App|Exchange| iOS und Android |
|Office 2016; Word, Excel, SharePoint|SharePoint| Windows 10, Windows Mobile 10, Windows 8.1, Windows 7, Mac |
|Office 2013 (erfordert die Aktivierung der modernen Authentifizierung)|SharePoint|Windows 10, Windows Mobile 10, Windows 8.1, Windows 7|
|Dynamics CRM-App|Dynamics CRM| Windows 10, Windows 8.1, Windows 7, iOS, Android|
| Yammer-App|Yammer| Windows Mobile 10, iOS, Android|
|Azure RemoteApp|Azure Remote-App-Dienst|Windows 10, Windows 8.1, Windows 7, Mac, iOS, Android|

## Anwendungen ohne moderne Authentifizierung

Zurzeit muss für Apps, die keine moderne Authentifizierungsverfahren verwenden, der Zugriff mithilfe anderer Methoden blockiert werden, da der bedingte Zugriff für diese nicht erzwungen wird. Dieser Aspekt gilt hauptsächlich für den Zugriff auf Exchange und SharePoint, da vorherige App-Versionen unter Verwendung älterer Protokolle erstellt wurden.

## SharePoint
Bei SharePoint können ältere Protokolle mithilfe des Cmdlets Set-SPOTenant deaktiviert werden. Dieses Cmdlet verhindert, dass Office-Clients, die keine modernen Authentifizierungsprotokolle verwenden, auf SharePoint Online-Ressourcen zugreifen.

**Beispielbefehl**: `Set-SPOTenant -LegacyAuthProtocolsEnabled $false`
 
## Exchange

In Exchange gibt es zwei Hauptkategorien von Protokollen. Überprüfen und wählen Sie die richtige Richtlinie für Ihre Organisation:

1. Exchange ActiveSync. Eine Richtlinie für bedingten Zugriff für MFA und Standort wird standardmäßig für Exchange ActiveSync nicht erzwungen. Dadurch kann der Zugriff entweder durch direktes Konfigurieren der Exchange ActiveSync-Richtlinie oder durch Blockieren von Exchange ActiveSync mithilfe von AD FS-Regeln geschützt werden.
2. Ältere Protokolle. Ältere Protokolle können mit AD FS blockiert werden. Damit wird der Zugriff für ältere Office-Clients wie z.B. Office 2013 ohne moderne Authentifizierung und frühere Versionen blockiert.


### Beispiele für AD FS-Regeln
Mithilfe der folgenden Regeln kann der Zugriff über ältere Protokolle in AD FS in zwei allgemeinen Konfigurationen blockiert werden. Option 1: Exchange ActiveSync zulassen und ältere Apps nur im Intranet zulassen.

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

### Option 2: Exchange ActiveSync zulassen und ältere Apps blockieren 
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

<!---HONumber=AcomDC_0615_2016-->