---
title: Entfernen einer Benutzer- oder Gruppenzuweisung aus einer Unternehmens-App in der Azure Active Directory-Vorschau | Microsoft Docs
description: Entfernen der Zugriffszuweisung eines Benutzers oder einer Gruppe aus einer Unternehmens-App in Azure Active Directory
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 7b2d365b-ae92-477f-9702-353cc6acc5ea
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/13/2017
ms.author: curtand
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: b38a613a1f93d9c36254f7642655f716e048e9e3


---
# <a name="remove-a-user-or-group-assignment-from-an-enterprise-app-in-azure-active-directory-preview"></a>Entfernen einer Benutzer- oder Gruppenzuweisung aus einer Unternehmens-App in der Azure Active Directory-Vorschau
In der Azure Active Directory-Vorschau (Azure AD) können Sie auf einfache Weise eine Benutzer- oder Gruppenzuweisung für den Zugriff auf eine Ihrer Unternehmensanwendungen entfernen. [Was bietet die Vorschauversion?](active-directory-preview-explainer.md)  Sie müssen über die geeigneten Berechtigungen zum Verwalten der Unternehmens-App verfügen. In der aktuellen Vorschau müssen Sie als globaler Administrator für das Verzeichnis konfiguriert sein.

## <a name="how-do-i-remove-a-user-or-group-assignment"></a>Wie entferne ich eine Benutzer- oder Gruppenzuweisung?
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) über ein Konto an, das als globaler Administrator für das Verzeichnis konfiguriert ist.
2. Wählen Sie **Weitere Dienste** aus, geben Sie **Azure Active Directory** in das Textfeld ein, und drücken Sie die **EINGABETASTE**.
3. Wählen Sie auf dem Blatt **Azure Active Directory – *Verzeichnisname*** (d.h. dem Azure AD-Blatt für das Verzeichnis, das Sie verwalten) **Unternehmensanwendungen** aus.

    ![Öffnen von Unternehmens-Apps](./media/active-directory-coreapps-remove-assignment-user-azure-portal/open-enterprise-apps.png)
4. Wählen Sie auf dem Blatt **Unternehmensanwendungen** die Einstellung **Alle Anwendungen** aus. Es wird eine Liste aller Apps angezeigt, die Sie verwalten können.
5. Wählen Sie auf dem Blatt **Unternehmensanwendungen – Alle Anwendungen** eine App aus.
6. Wählen Sie auf dem Blatt ***App-Name*** (dem Blatt mit dem Namen der ausgewählten App im Titel) die Option **Benutzer und Gruppen** aus.

    ![Auswählen von Benutzern oder Gruppen](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-app-users.png)
7. Wählen Sie auf dem Blatt ***App-Name*** **– Benutzer- und Gruppenzuweisung** mindestens einen Benutzer oder eine Gruppe aus, und klicken Sie dann auf den Befehl **Entfernen**. Bestätigen Sie Ihre Entscheidung, wenn Sie dazu aufgefordert werden.

    ![Auswählen des Befehls „Entfernen“](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-users.png)

## <a name="next-steps"></a>Nächste Schritte
* [Alle meine Gruppen anzeigen](active-directory-groups-view-azure-portal.md)
* [Zuweisen eines Benutzers oder einer Gruppe zu einer Unternehmens-App](active-directory-coreapps-assign-user-azure-portal.md)
* [Deaktivieren von Benutzeranmeldungen für eine Unternehmens-App](active-directory-coreapps-disable-app-azure-portal.md)
* [Ändern des Namens oder Logos einer Unternehmens-App](active-directory-coreapps-change-app-logo-user-azure-portal.md)



<!--HONumber=Nov16_HO3-->


