---
title: "Ändern des Signaturhashalgorithmus für die Vertrauensstellung der vertrauenden Seite für Office 365 | Microsoft Docs"
description: "Diese Seite enthält eine Anleitung zum Ändern des SHA-Algorithmus für die Verbundvertrauensstellung mit Office 365."
keywords: SHA1,SHA256,O365,federation,aadconnect,adfs,ad fs,change sha,federation trust,relying party trust
services: active-directory
documentationcenter: 
author: anandyadavmsft
manager: samueld
editor: 
ms.assetid: cf6880e2-af78-4cc9-91bc-b64de4428bbd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/31/2016
ms.author: anandy
ms.translationtype: Human Translation
ms.sourcegitcommit: f7479260c7c2e10f242b6d8e77170d4abe8634ac
ms.openlocfilehash: c581b1468630a9f28204592c936360b72f42f0d8
ms.contentlocale: de-de
ms.lasthandoff: 06/21/2017


---
# <a name="change-signature-hash-algorithm-for-office-365-relying-party-trust"></a>Ändern des Signaturhashalgorithmus für die Vertrauensstellung der vertrauenden Seite für Office 365
## <a name="overview"></a>Übersicht
Active Directory-Verbunddienste (Active Directory Federation Services, AD FS) signieren die eigenen Token in Microsoft Azure Active Directory, um sicherzustellen, dass sie nicht manipuliert werden können. Diese Signatur kann auf SHA-1 oder SHA-256 basieren. Azure Active Directory unterstützt jetzt Token, die mit einem SHA-256-Algorithmus signiert wurden, und es wird empfohlen, den Signaturalgorithmus für Token auf SHA-256 festzulegen, um ein Höchstmaß an Sicherheit zu erzielen. Dieser Artikel erläutert, wie Sie den Tokensignaturalgorithmus auf das sicherere Level SHA-256 festlegen.

>[!NOTE]
>Microsoft empfiehlt die Verwendung von SHA256 als Algorithmus für die Signatur von Tokens, da es sicherer als SHA1 ist. SHA1 ist jedoch nach wie vor eine unterstützte Option.

## <a name="change-the-token-signing-algorithm"></a>Ändern des Tokensignaturalgorithmus
Nachdem Sie den Signaturalgorithmus mit einer der beiden unten aufgeführten Vorgehensweisen festgelegt haben, signiert AD FS die Token für die Vertrauensstellung der vertrauenden Seite für Office 365 mit SHA-256. Sie müssen keine zusätzlichen Konfigurationsänderungen vornehmen, und Sie können trotz dieser Änderung auf Office 365 oder andere Azure AD-Anwendungen zugreifen.

### <a name="ad-fs-management-console"></a>AD FS-Verwaltungskonsole
1. Öffnen Sie die AD FS-Verwaltungskonsole auf dem primären AD FS-Server.
2. Erweitern Sie den AD FS-Knoten, und klicken Sie auf **Vertrauensstellungen der vertrauenden Seite**.
3. Klicken Sie mit der rechten Maustaste auf Ihre Vertrauensstellung der vertrauenden Seite für Office 365/Azure, und wählen Sie **Eigenschaften**aus.
4. Wählen Sie die Registerkarte **Erweitert** , und wählen Sie den sicheren Hashalgorithmus SHA-256 aus.
5. Klicken Sie auf **OK**.

![SHA-256-Signaturalgorithmus – MMC](./media/active-directory-aadconnectfed-sha256guidance/mmc.png)

### <a name="ad-fs-powershell-cmdlets"></a>AD FS PowerShell-Cmdlets
1. Öffnen Sie PowerShell mit Administratorrechten auf einem beliebigen AD FS-Server.
2. Legen Sie den sicheren Hashalgorithmus mithilfe des Cmdlets **Set-AdfsRelyingPartyTrust** fest.
   
   <code>Set-AdfsRelyingPartyTrust -TargetName 'Microsoft Office 365 Identity Platform' -SignatureAlgorithm 'http://www.w3.org/2001/04/xmldsig-more#rsa-sha256'</code>

## <a name="also-read"></a>Weitere Informationen
* [Verwaltung der Active Directory-Verbunddienste und Anpassung mit Azure AD Connect](connect/active-directory-aadconnect-federation-management.md#repairthetrust)


