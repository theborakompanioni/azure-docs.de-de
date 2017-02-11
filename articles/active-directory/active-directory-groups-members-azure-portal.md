---
title: "Verwalten der Mitglieder für eine Gruppe in der Azure Active Directory-Vorschau | Microsoft Docs"
description: "Verwalten von Benutzern und Geräten, bei denen es sich um Mitglieder einer Gruppe in Azure Active Directory handelt"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: d399a97d-fd2a-4b2d-b73d-0975db83f41b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2017
ms.author: curtand
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 7c0c411f6e2f51fa2d55d46ff92153dc8882bb38


---
# <a name="manage-the-members-for-a-group-in-azure-active-directory-preview"></a>Verwalten der Mitglieder für eine Gruppe in der Azure Active Directory-Vorschau
In diesem Artikel wird erläutert, wie Sie die Mitglieder für eine Gruppe in der Azure Active Directory-Vorschau (Azure AD) verwalten. [Was bietet die Vorschauversion?](active-directory-preview-explainer.md)

## <a name="how-do-i-find-the-members-and-manage-them"></a>Wie finde ich Mitglieder und verwalte sie?
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) über ein Konto an, das als globaler Administrator für das Verzeichnis konfiguriert ist.
2. Wählen Sie **Weitere Dienste** aus, geben Sie **Benutzer und Gruppen** in das Textfeld ein, und drücken Sie die EINGABETASTE****.

   ![Öffnen der Benutzerverwaltung](./media/active-directory-groups-members-azure-portal/search-user-management.png)
3. Wählen Sie auf dem Blatt **Benutzer und Gruppen** die Option **Alle Gruppen** aus.

   ![Öffnen des Blatts „Gruppen“](./media/active-directory-groups-members-azure-portal/view-groups-blade.png)
4. Wählen Sie auf dem Blatt **Benutzer und Gruppen** eine Gruppe aus.
5. Wählen Sie auf dem Blatt**Gruppe – *Gruppenname*** die Option **Mitglieder** aus.

   ![Öffnen des Blatts „Mitglieder“](./media/active-directory-groups-members-azure-portal/view-group-members.png)
6. Wählen Sie zum Hinzufügen von Mitgliedern zur Gruppe auf dem Blatt **Gruppe – Mitglieder** die Option **Mitglieder hinzufügen** aus.

   ![Befehl zum Hinzufügen von Mitgliedern](./media/active-directory-groups-members-azure-portal/add-group-members-command.png)
7. Wählen Sie auf dem Blatt **Mitglieder** mindestens einen Benutzer oder ein Gerät aus, den/das Sie der Gruppe hinzufügen möchten, und klicken Sie auf die Schaltfläche **Auswählen** im unteren Bereich des Blatts, um das Element zur Gruppe hinzuzufügen. Das Feld **Benutzer** filtert die Anzeige basierend auf einem Abgleich Ihrer Eingabe mit einem beliebigen Teil eines Benutzer- oder Gerätenamens. In diesem Feld können keine Platzhalterzeichen verwendet werden.
8. Um Mitglieder aus der Gruppe zu entfernen, wählen Sie auf dem Blatt **Gruppe – Mitglieder** ein Mitglied aus.
9. Wählen Sie auf dem Blatt ***Mitgliedsname*** den Befehl **Entfernen** aus, und bestätigen Sie die Auswahl, wenn Sie dazu aufgefordert werden.

   ![Befehl zum Entfernen von Mitgliedern](./media/active-directory-groups-members-azure-portal/remove-group-members-command.png)
10. Klicken Sie auf **Speichern**, wenn Sie die Bearbeitung der Mitglieder für die Gruppe abgeschlossen haben.

## <a name="additional-information"></a>Zusätzliche Informationen
Diese Artikel enthalten zusätzliche Informationen zu Azure Active Directory.

* [Anzeigen vorhandener Gruppen](active-directory-groups-view-azure-portal.md)
* [Erstellen einer neuen Gruppe und Hinzufügen von Mitgliedern](active-directory-groups-create-azure-portal.md)
* [Verwalten der Einstellungen einer Gruppe](active-directory-groups-settings-azure-portal.md)
* [Verwalten der Mitgliedschaften einer Gruppe](active-directory-groups-membership-azure-portal.md)
* [Verwalten dynamischer Regeln für Benutzer in einer Gruppe](active-directory-groups-dynamic-membership-azure-portal.md)



<!--HONumber=Nov16_HO3-->


