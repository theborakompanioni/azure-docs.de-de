---
title: 'Azure AD Connect-Synchronisierung: Verwalten des Azure AD-Dienstkontos | Microsoft Docs'
description: In diesem Thema wird beschrieben, wie Sie das Azure AD-Dienstkonto wiederherstellen.
services: active-directory
keywords: "AADSTS70002, AADSTS50054, Zurücksetzen des Kennworts für das Dienstkonto des Azure AD Connect-Synchronisierungsconnectors"
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 6077043a-27f1-4304-a44b-81dc46620f24
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2017
ms.author: billmath
translationtype: Human Translation
ms.sourcegitcommit: aa20b20c86763791eb579883b5273ea79cc714b5
ms.openlocfilehash: 0854cc7f23a427fc3fdb16e14af2f9e89bd1627f


---
# <a name="azure-ad-connect-sync-how-to-manage-the-azure-ad-service-account"></a>Azure AD Connect-Synchronisierung: Verwalten des Azure AD-Dienstkontos
Das vom Azure AD-Connector verwendete Dienstkonto sollte keine Verwaltung erfordern. Wenn Sie die Anmeldeinformationen des Kontos zurückzusetzen müssen, finden Sie die notwendigen Informationen in diesem Thema. Dies ist beispielsweise der Fall, wenn ein globaler Administrator versehentlich über PowerShell das Kennwort des Dienstkontos zurückgesetzt hat.

## <a name="reset-the-credentials"></a>Zurücksetzen der Anmeldeinformationen
Wenn das im Azure AD-Connector definierte Dienstkonto aufgrund von Authentifizierungsproblemen keine Verbindung mit Azure AD herstellen kann, kann das Kennwort zurückgesetzt werden.

1. Melden Sie sich beim Azure AD Connect-Synchronisierungsserver an, und starten Sie PowerShell.
2. Führen Sie `Add-ADSyncAADServiceAccount`aus.  
   ![PowerShell-Cmdlet addadsyncaadserviceaccount](./media/active-directory-aadconnectsync-howto-azureadaccount/addadsyncaadserviceaccount.png)
3. Stellen Sie Anmeldeinformationen eines globalen Azure AD-Administrators bereit.

Dieses Cmdlet setzt das Kennwort für das Dienstkonto zurück und aktualisiert es sowohl in Azure AD als auch im Synchronisierungsmodul.

## <a name="known-issues-these-steps-can-solve"></a>Bekannte Probleme, die sich mit diesen Schritten beheben lassen
Dieser Abschnitt enthält eine Liste mit Fehlern, die von Kunden gemeldet und durch das Zurücksetzen der Anmeldeinformationen für das Azure AD-Dienstkonto behoben wurden.

- - -
Ereignis 6900  
Der Server hat einen unerwarteten Fehler beim Verarbeiten einer Benachrichtigung über eine Kennwortänderung erkannt:  
AADSTS70002: Fehler beim Überprüfen der Anmeldeinformationen. AADSTS50054: Old password is used for authentication. (Ereignis 6900: Unerwarteter Serverfehler beim Verarbeiten einer Kennwortänderungsbenachrichtigung. AADSTS70002: Fehler beim Überprüfen der Anmeldeinformationen. AADSTS50054: Für die Authentifizierung wurde ein altes Kennwort verwendet.)

- - -
Ereignis 659  
Fehler beim Abrufen der Konfiguration für die Kennwortrichtliniensynchronisierung. Microsoft.IdentityModel.Clients.ActiveDirectory.AdalServiceException:  
AADSTS70002: Fehler beim Überprüfen der Anmeldeinformationen. AADSTS50054: Old password is used for authentication. (Ereignis 6900: Unerwarteter Serverfehler beim Verarbeiten einer Kennwortänderungsbenachrichtigung. AADSTS70002: Fehler beim Überprüfen der Anmeldeinformationen. AADSTS50054: Für die Authentifizierung wurde ein altes Kennwort verwendet.)

## <a name="next-steps"></a>Nächste Schritte
**Übersichtsthemen**

* [Azure AD Connect-Synchronisierung: Grundlagen und Anpassung der Synchronisierung](active-directory-aadconnectsync-whatis.md)
* [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md)




<!--HONumber=Nov16_HO3-->


