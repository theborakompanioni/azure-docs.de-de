---
title: Zuweisen eines Benutzers oder einer Gruppe zu einer Unternehmens-App in der Azure Active Directory-Vorschau | Microsoft Docs
description: "Auswählen einer Unternehmens-App zum Zuweisen eines Benutzers oder einer Gruppe in der Azure Active Directory-Vorschau"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 5817ad48-d916-492b-a8d0-2ade8c50a224
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/13/2017
ms.author: curtand
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 18330aa5e0fe8e3bbad6c266f823a4969b9b8b6c


---
# <a name="assign-a-user-or-group-to-an-enterprise-app-in-azure-active-directory-preview"></a>Zuweisen eines Benutzers oder einer Gruppe zu einer Unternehmens-App in der Azure Active Directory-Vorschau
In der Azure Active Directory-Vorschau (Azure AD) können Sie Ihren Unternehmensanwendungen auf einfache Weise einen Benutzer oder eine Gruppe zuzuweisen. [Was bietet die Vorschauversion?](active-directory-preview-explainer.md)  Sie müssen über die geeigneten Berechtigungen zum Verwalten der Unternehmens-App verfügen. In der aktuellen Vorschau müssen Sie als globaler Administrator für das Verzeichnis konfiguriert sein.

## <a name="how-do-i-assign-user-access-to-an-enterprise-app"></a>Wie weise ich Benutzerzugriff für eine Unternehmens-App zu?
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) über ein Konto an, das als globaler Administrator für das Verzeichnis konfiguriert ist.
2. Wählen Sie **Weitere Dienste** aus, geben Sie **Azure Active Directory** in das Textfeld ein, und drücken Sie die EINGABETASTE.
3. Wählen Sie auf dem Blatt **Azure Active Directory – *Verzeichnisname*** (d.h. dem Azure AD-Blatt für das Verzeichnis, das Sie verwalten) **Unternehmensanwendungen** aus.

    ![Öffnen von Unternehmens-Apps](./media/active-directory-coreapps-assign-user-azure-portal/open-enterprise-apps.png)
4. Wählen Sie auf dem Blatt **Unternehmensanwendungen** die Einstellung **Alle Anwendungen** aus. Es wird eine Liste aller Apps angezeigt, die Sie verwalten können.
5. Wählen Sie auf dem Blatt **Unternehmensanwendungen – Alle Anwendungen** eine App aus.
6. Wählen Sie auf dem Blatt ***App-Name*** (dem Blatt mit dem Namen der ausgewählten App im Titel) die Option **Benutzer und Gruppen** aus.

    ![Auswählen des Befehls „Alle Anwendungen“](./media/active-directory-coreapps-assign-user-azure-portal/select-app-users.png)
7. Wählen Sie auf dem Blatt ***App-Name*** **– Benutzer- und Gruppenzuweisung** den Befehl **Hinzufügen** aus.
8. Wählen Sie auf dem Blatt **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Zuweisen eines Benutzers oder einer Gruppe zur App](./media/active-directory-coreapps-assign-user-azure-portal/assign-users.png)
9. Wählen Sie auf dem Blatt **Benutzer und Gruppen** mindestens einen Benutzer oder eine Gruppe aus der Liste aus, und klicken Sie auf die Schaltfläche **Auswählen** im unteren Bereich des Blatts.
10. Wählen Sie auf dem Blatt **Zuweisung hinzufügen** die Option **Rolle** aus. Wählen Sie dann auf dem Blatt **Rolle auswählen** eine Rolle aus, die auf die ausgewählten Benutzer oder Gruppen angewendet werden soll, und klicken Sie anschließend im unteren Bereich des Blatts auf **OK**.
11. Klicken Sie im unteren Bereich des Blatts **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**. Die zugewiesenen Benutzer oder Gruppen erhalten die Berechtigungen, die durch die ausgewählte Rolle für diese Unternehmens-App definiert werden.

## <a name="next-steps"></a>Nächste Schritte
* [Alle meine Gruppen anzeigen](active-directory-groups-view-azure-portal.md)
* [Entfernen einer Benutzer- oder Gruppenzuweisung aus einer Unternehmens-App](active-directory-coreapps-remove-assignment-azure-portal.md)
* [Deaktivieren von Benutzeranmeldungen für eine Unternehmens-App](active-directory-coreapps-disable-app-azure-portal.md)
* [Ändern des Namens oder Logos einer Unternehmens-App](active-directory-coreapps-change-app-logo-user-azure-portal.md)



<!--HONumber=Nov16_HO3-->


