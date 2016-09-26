<properties
	pageTitle="Erstellen einer neuen Gruppe in der Azure Active Directory-Vorschau | Microsoft Azure"
	description="Erstellen einer Gruppe in Azure Active Directory und Hinzufügen von Benutzern (Mitgliedern) zur Gruppe"
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


# Erstellen einer neuen Gruppe in der Azure Active Directory-Vorschau

In diesem Artikel wird erläutert, wie Sie eine neue Gruppe in der Azure Active Directory-Vorschau (Azure AD) erstellen und auffüllen. [Was bietet die Vorschauversion?](active-directory-preview-explainer.md) Sie verwenden eine Gruppe zum Durchführen von Verwaltungsaufgaben, z.B. für das Zuweisen von Lizenzen oder Berechtigungen für mehrere Benutzer oder Geräte in einem Arbeitsschritt.

## Wie erstelle ich eine Gruppe?

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) über ein Konto an, das als globaler Administrator für das Verzeichnis konfiguriert ist.

2. Wählen Sie **Weitere Dienste** aus, geben Sie **Benutzer und Gruppen** in das Textfeld ein, und drücken Sie die **EINGABETASTE**.

  ![Öffnen der Benutzerverwaltung](./media/active-directory-groups-create-azure-portal/search-user-management.png)

3. Wählen Sie auf dem Blatt **Benutzer und Gruppen** die Option **Alle Gruppen** aus.

  ![Öffnen des Blatts „Gruppen“](./media/active-directory-groups-create-azure-portal/view-groups-blade.png)

4. Wählen Sie auf dem Blatt **Benutzer und Gruppen** den Befehl **Hinzufügen** aus.

  ![Auswählen des Befehls „Hinzufügen“](./media/active-directory-groups-create-azure-portal/add-group-command.png)

5. Fügen Sie auf dem Blatt **Gruppe** einen Namen und eine Beschreibung für die Gruppe hinzu.

6. Um der Gruppe Mitglieder hinzuzufügen, wählen Sie im Textfeld **Mitgliedschaftstyp** die Einstellung **Zugewiesen** aus und klicken dann auf **Mitglieder**. Weitere Informationen zum dynamischen Verwalten der Gruppenmitgliedschaften finden Sie unter [Verwalten dynamischer Regeln für Benutzer in einer Gruppe](active-directory-groups-dynamic-users-azure-portal.md) oder unter [Verwalten dynamischer Regeln für Geräte in einer Gruppe](active-directory-groups-dynamic-devices-azure-portal.md).

  ![Auswählen der hinzuzufügenden Mitglieder](./media/active-directory-groups-create-azure-portal/select-members.png)

5. Wählen Sie auf dem Blatt **Mitglieder** mindestens einen Benutzer oder ein Gerät aus, den/das Sie der Gruppe hinzufügen möchten, und klicken Sie auf die Schaltfläche **Auswählen** im unteren Bereich des Blatts, um das Element zur Gruppe hinzuzufügen. Das Feld **Benutzer** filtert die Anzeige basierend auf einem Abgleich Ihrer Eingabe mit einem beliebigen Teil eines Benutzer- oder Gerätenamens. In diesem Feld können keine Platzhalterzeichen verwendet werden.

6. Wenn Sie alle gewünschten Mitglieder zur Gruppe hinzugefügt haben, wählen Sie auf dem Blatt **Gruppe** die Option **Erstellen** aus.

  ![Bestätigen der Gruppenerstellung](./media/active-directory-groups-create-azure-portal/create-group-confirmation.png)




## Zusätzliche Informationen

Diese Artikel enthalten zusätzliche Informationen zu Azure Active Directory.

* [Anzeigen vorhandener Gruppen](active-directory-groups-view-azure-portal.md)
* [Verwalten der Einstellungen einer Gruppe](active-directory-groups-settings-azure-portal.md)
* [Verwalten der Mitglieder einer Gruppe](active-directory-groups-members-azure-portal.md)
* [Verwalten der Mitgliedschaften einer Gruppe](active-directory-groups-membership-azure-portal.md)
* [Verwalten dynamischer Regeln für Benutzer in einer Gruppe](active-directory-groups-dynamic-membership-azure-portal.md)

<!---HONumber=AcomDC_0914_2016-->