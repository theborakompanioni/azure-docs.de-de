---
title: "Verwalten der Einstellungen für die Rollenaktivierung | Microsoft Docs"
description: "Erfahren Sie, wie Sie mit der Erweiterung Azure Active Directory Privileged Identity Management die Standardeinstellungen für privilegierte Identitäten ändern."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: 
ms.assetid: f6cbcb6a-8a89-4077-afd8-06c94a64f4aa
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/30/2016
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 5108b83d3291d3b934a402a0a7978c68bda0cf1b


---
# <a name="how-to-manage-role-activation-settings-in-azure-ad-privileged-identity-management"></a>Verwalten der Einstellungen für die Rollenaktivierung in Azure AD Privileged Identity Management
Ein Administrator für privilegierte Rollen kann Azure AD Privileged Identity Management (PIM) in der Organisation anpassen und hierbei auch die Art und Weise ändern, in der ein Benutzer eine berechtigte Rollenzuweisung aktiviert.

## <a name="manage-the-role-activation-settings"></a>Verwalten der Rollenaktivierungseinstellungen
1. Wechseln Sie zum [Azure-Portal](https://portal.azure.com) , und wählen Sie auf dem Dashboard die App **Azure AD Privileged Identity Management** aus.
2. Wählen Sie **Privilegierte Rollen verwalten** > **Einstellungen** > **Privilegierte Rollen**.
3. Wählen Sie die Rolle, deren Einstellungen Sie verwalten möchten.

Auf der Einstellungsseite für jede Rolle finden Sie eine Reihe von Einstellungen, die Sie konfigurieren können. Diese Einstellungen wirken sich nur auf Benutzer aus, die berechtigte Administratoren und keine permanenten Administratoren sind.

**Aktivierungen**: Die Zeit in Stunden, die eine Rolle aktiv bleibt, bevor sie abläuft. Diese Dauer kann zwischen 1 und 72 Stunden betragen.

**Benachrichtigungen**: Sie können entscheiden, ob das System E-Mails an Administratoren sendet, um zu bestätigen, dass sie eine Rolle aktiviert haben. Dies kann für die Erkennung von nicht autorisierten oder unzulässigem Aktivierungen nützlich sein.

**Incident/Ticket anfordern**: Sie können auswählen, ob berechtigte Administratoren eine Ticketnummer angeben müssen, wenn sie ihre Rolle aktivieren. Dies kann nützlich sein, wenn Sie Rollenzugriffe überprüfen.

**Multi-Factor Authentication**: Sie können auswählen, ob Benutzer ihre Identität mit MFA verifizieren müssen, bevor sie ihre Rollen aktivieren können. Sie müssen dies nur einmal pro Sitzung verifizieren, nicht jedes Mal, wenn sie eine Rolle aktivieren. Beachten Sie zwei Tipps, wenn Sie die MFA aktivieren:

* Benutzer, die Microsoft-Konten als E-Mail-Adressen verwenden (in der Regel @outlook.com,, aber nicht immer), können sich nicht für Azure MFA registrieren. Wenn Sie Benutzern mit Microsoft-Konten Rollen zuweisen möchten, sollten Sie sie zu permanenten Administratoren machen oder MFA für diese Rolle deaktivieren.
* Sie können MFA für sehr privilegierte Rollen für Azure AD und Office 365 nicht deaktivieren. Dies ist ein Sicherheitsfeature, da diese Rollen sorgfältig geschützt werden sollten:  
  
  * Anwendungsadministrator
  * Serveradministrator des Anwendungsproxys
  * Abrechnungsadministrator  
  * Complianceadministrator  
  * CRM-Dienstadministrator
  * Genehmigende Person für den LockBox-Kundenzugriff
  * Verzeichnisautor  
  * Exchange-Administrator  
  * Globaler Administrator
  * Intune-Dienstadministrator
  * Postfachadministrator  
  * Partnersupport der Ebene 1  
  * Partnersupport der Ebene 2  
  * Administrator für privilegierte Rollen   
  * Sicherheitsadministrator  
  * SharePoint-Administrator  
  * Skype for Business-Administrator  
  * Benutzerkontoadministrator  

Weitere Informationen zum Verwenden von MFA mit PIM finden Sie unter [Erfordern von MFA](active-directory-privileged-identity-management-how-to-require-mfa.md).

<!--PLACEHOLDER: Need an explanation of what the temporary Global Administrator setting is for.-->

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Nächste Schritte
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]




<!--HONumber=Nov16_HO3-->


