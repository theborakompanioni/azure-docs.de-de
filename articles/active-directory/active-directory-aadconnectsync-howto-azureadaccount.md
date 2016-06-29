<properties
	pageTitle="Azure AD Connect-Synchronisierung: Verwalten des Azure AD-Dienstkontos | Microsoft Azure"
	description="In diesem Thema wird beschrieben, wie Sie das Azure AD-Dienstkonto wiederherstellen."
	services="active-directory"
    keywords="Zurücksetzen des Kennworts für das Dienstkonto des Azure AD Connect-Synchronisierungsconnectors"
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
	ms.date="06/10/2016"
	ms.author="andkjell"/>

# Azure AD Connect-Synchronisierung: Verwalten des Azure AD-Dienstkontos
Das vom Azure AD-Connector verwendete Dienstkonto sollte keine Verwaltung erfordern. Wenn Sie die Anmeldeinformationen des Kontos zurückzusetzen müssen, finden Sie die notwendigen Informationen in diesem Thema. Dies kann beispielsweise passieren, wenn ein globaler Administrator versehentlich über PowerShell das Kennwort des Dienstkontos zurückgesetzt hat.

## Zurücksetzen der Anmeldeinformationen
Wenn das im Azure AD-Connector definierte Dienstkonto aufgrund von Authentifizierungsproblemen keine Verbindung mit Azure AD herstellen kann, kann das Kennwort zurückgesetzt werden.

1. Melden Sie sich am Azure AD Connect-Synchronisierungsserver an, und starten Sie PowerShell.
2. Führen Sie `Add-ADSyncAADServiceAccount` aus. ![PowerShell-Cmdlet addadsyncaadserviceaccount](./media/active-directory-aadconnectsync-howto-azureadaccount/addadsyncaadserviceaccount.png)
3. Stellen Sie Anmeldeinformationen eines globalen Azure AD-Administrators bereit.

Dieses Cmdlet setzt das Kennwort für das Dienstkonto zurück und aktualisiert es sowohl in Azure AD als auch im Synchronisierungsmodul.

## Nächste Schritte
Weitere Informationen zum [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md).

<!---HONumber=AcomDC_0615_2016-->