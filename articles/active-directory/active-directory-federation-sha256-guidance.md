---
title: Ändern des Signaturhashalgorithmus für die Vertrauensstellung der vertrauenden Seite für Office 365 | Microsoft Docs
description: Diese Seite enthält eine Anleitung zum Ändern des SHA-Algorithmus für die Verbundvertrauensstellung mit Office 365.
keywords: SHA1,SHA256,O365,federation,aadconnect,adfs,ad fs,change sha,federation trust,relying party trust
services: active-directory
documentationcenter: ''
author: anandyadavmsft
manager: samueld
editor: ''

ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2016
ms.author: anandy

---
# Ändern des Signaturhashalgorithmus für die Vertrauensstellung der vertrauenden Seite für Office 365
## Übersicht
Azure Active Directory-Verbunddienste (Active Directory Federation Services, AD FS) signiert seine Token in Microsoft Azure Active Directory, um sicherzustellen, dass sie nicht manipuliert werden können. Diese Signatur kann auf SHA-1 oder SHA-256 basieren. Azure Active Directory unterstützt jetzt Token, die mit einem SHA-256-Algorithmus signiert wurden, und es wird empfohlen, den Signaturalgorithmus für Token auf SHA-256 festzulegen, um ein Höchstmaß an Sicherheit zu erzielen. Dieser Artikel erläutert, wie Sie den Tokensignaturalgorithmus auf das sicherere Level SHA-256 festlegen.

## Ändern des Tokensignaturalgorithmus
Nachdem Sie den Signaturalgorithmus mit einer der beiden unten aufgeführten Vorgehensweisen festgelegt haben, signiert AD FS die Token für die Vertrauensstellung der vertrauenden Seite für Office 365 mit SHA-256. Sie müssen keine zusätzlichen Konfigurationsänderungen vornehmen, und Sie können trotz dieser Änderung auf Office 365 oder andere Azure AD-Anwendungen zugreifen.

### AD FS-Verwaltungskonsole
1. Öffnen Sie die AD FS-Verwaltungskonsole auf dem primären AD FS-Server.
2. Erweitern Sie den AD FS-Knoten, und klicken Sie auf **Vertrauensstellungen der vertrauenden Seite**.
3. Klicken Sie mit der rechten Maustaste auf Ihre Vertrauensstellung der vertrauenden Seite für Office 365/Azure, und wählen Sie **Eigenschaften** aus.
4. Wählen Sie die Registerkarte **Erweitert**, und wählen Sie den sicheren Hashalgorithmus SHA-256 aus.
5. Klicken Sie auf **OK**.

![SHA-256-Signaturalgorithmus – MMC](./media/active-directory-aadconnectfed-sha256guidance/mmc.png)

### AD FS PowerShell-Cmdlets
1. Öffnen Sie PowerShell mit Administratorrechten auf einem beliebigen AD FS-Server.
2. Legen Sie den sicheren Hashalgorithmus mithilfe des Cmdlets **Set-AdfsRelyingPartyTrust** fest.
   
   <code>Set-AdfsRelyingPartyTrust -TargetName 'Microsoft Office 365 Identity Platform' -SignatureAlgorithm 'http://www.w3.org/2001/04/xmldsig-more#rsa-sha256'</code>

## Weitere Informationen
* [Verwaltung der Active Directory-Verbunddienste und Anpassung mit Azure AD Connect](active-directory-aadconnect-federation-management.md#repairing-the-trust)

<!---HONumber=AcomDC_0817_2016-->