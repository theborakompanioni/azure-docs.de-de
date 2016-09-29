<properties
	pageTitle="Verwalten der Gruppen, denen Ihre Gruppe angehört, in der Azure Active Directory-Vorschau | Microsoft Azure"
	description="Gruppen können in Azure Active Directory andere Gruppen enthalten. Nachfolgend wird erläutert, wie Sie diese Mitgliedschaften verwalten."
	services="active-directory"
	documentationCenter=""
	authors="curtand"
	manager="femila"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="09/12/2016"
	ms.author="curtand"/>


# Verwalten der Gruppen, denen Ihre Gruppe angehört, in der Azure Active Directory-Vorschau

Gruppen können in der Azure Active Directory-Vorschau andere Gruppen enthalten. [Was bietet die Vorschauversion?](active-directory-preview-explainer.md) Nachfolgend wird erläutert, wie Sie diese Mitgliedschaften verwalten.

## Wie finde ich die Gruppen, in denen meine Gruppe Mitglied ist?

1.  Melden Sie sich beim [Azure-Portal](https://portal.azure.com) über ein Konto an, das als globaler Administrator für das Verzeichnis konfiguriert ist.

2.  Wählen Sie **Weitere Dienste** aus, geben Sie **Benutzer und Gruppen** in das Textfeld ein, und drücken Sie die **EINGABETASTE**.

  ![Öffnen der Benutzerverwaltung](./media/active-directory-groups-membership-azure-portal/search-user-management.png)

3.  Wählen Sie auf dem Blatt **Benutzer und Gruppen** die Option **Alle Gruppen** aus.

  ![Öffnen des Blatts „Gruppen“](./media/active-directory-groups-membership-azure-portal/view-groups-blade.png)

4. Wählen Sie auf dem Blatt **Benutzer und Gruppen** eine Gruppe aus.

5. Wählen Sie auf dem Blatt **Gruppe – *Gruppenname*** die Option **Gruppenmitgliedschaften** aus.

  ![Öffnen des Blatts „Gruppenmitgliedschaften“](./media/active-directory-groups-membership-azure-portal/group-membership-blade.png)

6. Um Ihre Gruppe als Mitglied einer anderen Gruppe hinzuzufügen, klicken Sie auf dem Blatt **Gruppe – Gruppenmitgliedschaften** auf den Befehl **Hinzufügen**.

7. Wählen Sie auf dem Blatt **Gruppe auswählen** eine Gruppe aus, und klicken Sie dann im unteren Bereich des Blatts auf den Befehl **Auswählen**. Sie können Ihre Gruppe in einem Arbeitsschritt immer nur einer Gruppe hinzufügen. Das Feld **Benutzer** filtert die Anzeige basierend auf einem Abgleich Ihrer Eingabe mit einem beliebigen Teil eines Benutzer- oder Gerätenamens. In diesem Feld können keine Platzhalterzeichen verwendet werden.

  ![Hinzufügen einer Gruppenmitgliedschaft](./media/active-directory-groups-membership-azure-portal/add-group-membership.png)

8. Um Ihre Gruppe als Mitglied einer anderen Gruppe zu entfernen, wählen Sie auf dem Blatt **Gruppe – Gruppenmitgliedschaften** eine Gruppe aus.

9. Klicken Sie auf dem Blatt ***Gruppenname*** auf den Befehl **Entfernen**, und bestätigen Sie Ihre Auswahl, wenn Sie dazu aufgefordert werden.

  ![Befehl zum Entfernen der Mitgliedschaft](./media/active-directory-groups-membership-azure-portal/remove-group-membership.png)

9. Klicken Sie auf **Speichern**, wenn Sie die Bearbeitung der Gruppenmitgliedschaften für Ihre Gruppe abgeschlossen haben.


## Zusätzliche Informationen

Diese Artikel enthalten zusätzliche Informationen zu Azure Active Directory.

* [Anzeigen vorhandener Gruppen](active-directory-groups-view-azure-portal.md)
* [Erstellen einer neuen Gruppe und Hinzufügen von Mitgliedern](active-directory-groups-create-azure-portal.md)
* [Verwalten der Einstellungen einer Gruppe](active-directory-groups-settings-azure-portal.md)
* [Verwalten der Mitglieder einer Gruppe](active-directory-groups-members-azure-portal.md)
* [Verwalten dynamischer Regeln für Benutzer in einer Gruppe](active-directory-groups-dynamic-membership-azure-portal.md)

<!---HONumber=AcomDC_0914_2016-->