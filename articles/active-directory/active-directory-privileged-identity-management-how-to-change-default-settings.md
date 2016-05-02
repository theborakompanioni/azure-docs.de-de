<properties
   pageTitle="Verwalten der Einstellungen für die Rollenaktivierung | Microsoft Azure"
   description="Erfahren Sie, wie Sie mit der Erweiterung Azure Active Directory Privileged Identity Management die Standardeinstellungen für privilegierte Identitäten ändern."
   services="active-directory"
   documentationCenter=""
   authors="kgremban"
   manager="stevenpo"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="04/15/2016"
   ms.author="kgremban"/>

# Verwalten der Einstellungen für die Rollenaktivierung in Azure AD Privileged Identity Management

Ein Sicherheitsadministrator kann Azure AD Privileged Identity Management (PIM) in der Organisation anpassen und hierbei auch die Art und Weise ändern, in der ein Benutzer eine temporäre Rollenzuweisung aktiviert.

## Verwalten der Rollenaktivierungseinstellungen

1. Wechseln Sie zum [Azure-Portal](https://portal.azure.com), und wählen Sie auf dem Dashboard die App **Azure AD Privileged Identity Management** aus.
2. Wählen Sie in der Rollenliste die Rolle aus, die Sie verwalten möchten.
3. Klicken Sie auf **Einstellungen**.
4. Legen Sie Standardaktivierungsdauer in Stunden fest. Verwenden Sie hierzu den Schieberegler, oder geben Sie im Textfeld die Anzahl der Stunden ein. Die maximale Dauer beträgt 72 Stunden.
5. Klicken Sie auf **Aktivieren** oder **Deaktivieren**, um festzulegen, ob Aktivierungsbenachrichtigungen an Administratoren gesendet werden sollen oder nicht. (Das Aktivieren der Benachrichtigungen kann dabei helfen, nicht autorisierte Administratoraktivitäten zu erkennen.)
6. Klicken Sie auf **Aktivieren**, um festzulegen, dass Administratoren in der Aktivierungsanforderung Ticketinformationen eingeben dürfen. (Diese Informationen können später beim Überwachen des Rollenzugriffs hilfreich sein.)
7. Klicken Sie auf **Aktivieren** oder **Deaktivieren**, um festzulegen, ob eine Aktivierungsanforderung Multi-Factor Authentication erfordert oder nicht.
8. Klicken Sie auf **Speichern**.

Sie können MFA für hoch privilegierte Rollen für Azure AD und Office 365 nicht deaktivieren. Dazu gehören folgende Rollen:
- Globaler Administrator  
- Benutzerkontoadministrator  
- Verzeichnisautor  
- Partnersupport der Ebene 1  
- Partnersupport der Ebene 2  
- Abrechnungsadministrator  
- Sicherheitsadministrator  
- Exchange-Administrator  
- Postfachadministrator  
- Skype for Business-Administrator  
- SharePoint-Administrator  
- Complianceadministrator  

Weitere Informationen zum Verwenden von MFA mit PIM finden Sie unter [Erfordern von MFA](active-directory-privileged-identity-management-how-to-require-mfa.md).

<!--PLACEHOLDER: Need an explanation of what the temporary Global Administrator setting is for.-->

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## Nächste Schritte
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!---HONumber=AcomDC_0420_2016-->