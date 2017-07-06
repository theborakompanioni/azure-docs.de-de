---

title: "Hinzufügen neuer Benutzer in Azure Active Directory | Microsoft Docs"
description: "Erklärt, wie neue Benutzer hinzugefügt oder Benutzerinformationen in Azure Active Directory geändert werden."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 0a90c3c5-4e0e-43bd-a606-6ee00f163038
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: curtand;jeffsta
ms.reviewer: jeffsta
ms.translationtype: Human Translation
ms.sourcegitcommit: 44eac1ae8676912bc0eb461e7e38569432ad3393
ms.openlocfilehash: 5f1f28b6e0135fbd76fa5f6664f457f259dd9dd4
ms.contentlocale: de-de
ms.lasthandoff: 05/17/2017



---
# <a name="add-new-users-to-azure-active-directory"></a>Hinzufügen neuer Benutzer zu Azure Active Directory
> [!div class="op_single_selector"]
> * [Azure-Portal](active-directory-users-create-azure-portal.md)
> * [klassischen Azure-Portal](active-directory-create-users.md)
>
>

In diesem Artikel wird erläutert, wie Sie Ihrer Organisation in Azure Active Directory (Azure AD) neue Benutzer hinzufügen. 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) über ein Konto an, das als globaler Administrator für das Verzeichnis konfiguriert ist.
2. Wählen Sie **Weitere Dienste** aus, geben Sie **Benutzer und Gruppen** in das Textfeld ein, und drücken Sie die **EINGABETASTE** .

   ![Öffnen von Benutzern und Gruppen](./media/active-directory-users-create-azure-portal/create-users-user-management.png)
3. Wählen Sie auf dem Blatt **Benutzer und Gruppen** die Option **Alle Gruppen** aus, und klicken Sie dann auf **Hinzufügen**.

   ![Auswählen des Befehls „Hinzufügen“](./media/active-directory-users-create-azure-portal/create-users-add-command.png)
4. Geben Sie Details zum Benutzer ein, beispielsweise **Name** und **Benutzername**. Der Domänenteil des Benutzernamens muss entweder der Name der anfänglichen Standarddomänenname „foo.onmicrosoft.com“ oder eine überprüfte, nicht im Verbund konfigurierte Domäne wie z.B. „contoso.com“ sein.
5. Kopieren oder notieren Sie sich das generierte Benutzerkennwort, damit sie es nach Abschluss des Vorgangs dem Benutzer mitteilen können.
6. Optional können Sie die Informationen auf dem Blatt **Profil**, dem Blatt **Gruppen** oder dem Blatt **Verzeichnisrolle** für den Benutzer ausfüllen. Weitere Informationen zu Benutzer- und Administratorrollen finden Sie unter [Zuweisen von Administratorrollen in Azure AD](active-directory-assign-admin-roles.md).
7. Wählen Sie auf dem Blatt **Benutzer** die Option **Erstellen** aus.
8. Sorgen Sie für eine sichere Übermittlung des generierten Kennworts an den neuen Benutzer, damit sich dieser anmelden kann.

### <a name="next-steps"></a>Nächste Schritte
* [Hinzufügen eines externen Benutzers](active-directory-users-create-external-azure-portal.md)
* [Zurücksetzen des Kennworts für einen Benutzer im neuen Azure-Portal](active-directory-users-reset-password-azure-portal.md)
* [Ändern der Arbeitsinformationen für einen Benutzer](active-directory-users-work-info-azure-portal.md)
* [Verwalten von Benutzerprofilen](active-directory-users-profile-azure-portal.md)
* [Löschen eines Benutzers in Azure AD](active-directory-users-delete-user-azure-portal.md)
* [Zuweisen eines Benutzers zu einer Rolle in Azure AD](active-directory-users-assign-role-azure-portal.md)

