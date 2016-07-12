<properties
	pageTitle="Azure AD Connect-Synchronisierung: Verwalten des Azure AD-Dienstkontos | Microsoft Azure"
	description="In diesem Thema wird beschrieben, wie Sie das Azure AD-Dienstkonto wiederherstellen."
	services="active-directory"
    keywords="AADSTS70002, AADSTS50054, Zurücksetzen des Kennworts für das Dienstkonto des Azure AD Connect-Synchronisierungsconnectors"
	documentationCenter=""
	authors="andkjell"
	manager="stevenpo"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="06/27/2016"
	ms.author="andkjell"/>

# Azure AD Connect-Synchronisierung: Verwalten des Azure AD-Dienstkontos
Das vom Azure AD-Connector verwendete Dienstkonto sollte keine Verwaltung erfordern. Wenn Sie die Anmeldeinformationen des Kontos zurückzusetzen müssen, finden Sie die notwendigen Informationen in diesem Thema. Dies kann beispielsweise passieren, wenn ein globaler Administrator versehentlich über PowerShell das Kennwort des Dienstkontos zurückgesetzt hat.

## Zurücksetzen der Anmeldeinformationen
Wenn das im Azure AD-Connector definierte Dienstkonto aufgrund von Authentifizierungsproblemen keine Verbindung mit Azure AD herstellen kann, kann das Kennwort zurückgesetzt werden.

1. Melden Sie sich am Azure AD Connect-Synchronisierungsserver an, und starten Sie PowerShell.
2. Führen Sie `Add-ADSyncAADServiceAccount` aus. ![PowerShell-Cmdlet addadsyncaadserviceaccount](./media/active-directory-aadconnectsync-howto-azureadaccount/addadsyncaadserviceaccount.png)
3. Stellen Sie Anmeldeinformationen eines globalen Azure AD-Administrators bereit.

Dieses Cmdlet setzt das Kennwort für das Dienstkonto zurück und aktualisiert es sowohl in Azure AD als auch im Synchronisierungsmodul.

## Bekannte Probleme, die sich mit diesen Schritten beheben lassen
Hier finden Sie eine Liste mit Fehlern, die von Kunden gemeldet und mithilfe der genannten Schritte behoben wurden.

-----------
Event 6900 The server encountered an unexpected error while processing a password change notification: AADSTS70002: Error validating credentials. AADSTS50054: Old password is used for authentication. (Ereignis 6900: Unerwarteter Serverfehler beim Verarbeiten einer Kennwortänderungsbenachrichtigung. AADSTS70002: Fehler beim Überprüfen der Anmeldeinformationen. AADSTS50054: Für die Authentifizierung wurde ein altes Kennwort verwendet.)

----------
Event 659 Error while retrieving password policy sync configuration. Microsoft.IdentityModel.Clients.ActiveDirectory.AdalServiceException: AADSTS70002: Error validating credentials. AADSTS50054: Old password is used for authentication. (Ereignis 659: Fehler beim Abrufen der Konfiguration für die Kennwortrichtliniensynchronisierung. Microsoft.IdentityModel.Clients.ActiveDirectory.AdalServiceException: AADSTS70002: Fehler beim Überprüfen der Anmeldeinformationen. AADSTS50054: Für die Authentifizierung wurde ein altes Kennwort verwendet.)

## Nächste Schritte
Weitere Informationen zum [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md).

<!---HONumber=AcomDC_0629_2016-->