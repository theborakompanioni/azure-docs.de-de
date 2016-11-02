
<properties
    pageTitle="Nächste Schritte für die Zugriffsverwaltung mithilfe von Gruppen | Microsoft Azure"
    description="Erweiterte Vorgehensweisen zum Verwalten von Sicherheitsgruppen und Verwenden dieser Gruppen zum Verwalten des Zugriffs auf eine Ressource."
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
    ms.date="09/22/2016"
    ms.author="curtand"/>


# <a name="managing-owners-for-a-group"></a>Verwalten von Besitzern einer Gruppe
Wenn ein Ressourcenbesitzer einer Ressource den Zugriff auf eine Azure AD-Gruppe zugewiesen hat, wird die Mitgliedschaft der Gruppe durch den Besitzer der Gruppe verwaltet. Tatsächlich delegiert der Besitzer der Ressource die Berechtigung, dieser Ressource Benutzer zuzuweisen, an den Besitzer der Gruppe.

## <a name="assigning-group-ownership"></a>Zuweisen des Gruppenbesitzes

**So fügen Sie einer Gruppe einen Besitzer hinzu**

1. Wählen Sie im [klassischen Azure-Portal](https://manage.windowsazure.com)die Option **Active Directory**aus, und öffnen Sie dann das Verzeichnis Ihrer Organisation.

2. Wählen Sie die Registerkarte **Gruppen** aus, und öffnen Sie dann die Gruppe, der Besitzer hinzugefügt werden sollen.

3. Wählen Sie **Besitzer hinzufügen**aus.

4. Wählen Sie auf der Seite **Besitzer hinzufügen** den Benutzer aus, den Sie als Besitzer dieser Gruppe hinzufügen möchten, und stellen Sie sicher, dass dieser Name dem Bereich **Ausgewählt** hinzugefügt wird.


**So entfernen Sie einen Besitzer aus einer Gruppe**

1. Wählen Sie im [klassischen Azure-Portal](https://manage.windowsazure.com)die Option **Active Directory**aus, und öffnen Sie dann das Verzeichnis Ihrer Organisation.

2. Wählen Sie die Registerkarte **Gruppen** aus, und öffnen Sie dann die Gruppe, aus der ein Besitzer entfernt werden soll.

4. Wählen Sie die Registerkarte **Besitzer** aus.

5. Wählen Sie den Besitzer aus, der aus der Gruppe entfernt werden soll, und wählen Sie dann **Entfernen**aus.

## <a name="additional-information"></a>Zusätzliche Informationen

Diese Artikel enthalten zusätzliche Informationen zu Azure Active Directory.

* [Verwalten des Zugriffs auf Ressourcen mit Azure Active Directory-Gruppen](active-directory-manage-groups.md)
* [Azure Active Directory-Cmdlets zum Konfigurieren von Gruppeneinstellungen](active-directory-accessmanagement-groups-settings-cmdlets.md)
* [Artikelindex für die Anwendungsverwaltung in Azure Active Directory](active-directory-apps-index.md)
* [Was ist Azure Active Directory?](active-directory-whatis.md)
* [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md)



<!--HONumber=Oct16_HO2-->


