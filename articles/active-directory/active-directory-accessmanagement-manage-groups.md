<properties

	pageTitle="Managing groups in Azure Active Directory | Microsoft Azure"
	description="How to create and manage groups to manage Azure users using Azure Active Directory."
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
	ms.topic="get-started-article"
	ms.date="09/22/2016"
	ms.author="curtand"/>


# Verwalten von Gruppen in Azure Active Directory

Ein Feature der Benutzerverwaltung von Azure Active Directory (Azure AD) ist die Möglichkeit, Gruppen mit Benutzern zu erstellen. Sie verwenden eine Gruppe zum Durchführen von Verwaltungsaufgaben, z.B. gleichzeitiges Zuweisen von Lizenzen oder Berechtigungen für mehrere Benutzer. Außerdem können Sie Gruppen verwenden, um folgenden Ressourcen Berechtigungen zuzuweisen:

- Ressourcen, z.B. Objekte im Verzeichnis
- Verzeichnisexterne Ressourcen, z.B. SaaS-Anwendungen, Azure-Dienste, SharePoint-Websites oder lokale Ressourcen

Darüber hinaus kann ein Ressourcenbesitzer den Zugriff auf eine Ressource auch für eine Azure AD-Gruppe zuweisen, die sich im Besitz einer anderen Person befindet. Hierbei wird den Mitgliedern dieser Gruppe Zugriff auf die Ressource gewährt. Der Besitzer der Gruppe verwaltet dann die Mitgliedschaft in der Gruppe. Der Besitzer der Ressource delegiert quasi die Berechtigung zum Zuweisen von Benutzern zu dieser Gruppe an den Besitzer der Gruppe.

## Wie erstelle ich eine Gruppe?

Je nach den Diensten, die von Ihrer Organisation abonniert wurden, können Sie auf eine der folgenden Arten eine Gruppe erstellen:
- Klassisches Azure-Portal
- Office 365-Kontoportal
- Windows Intune-Kontoportal

Hier werden die Aufgaben beschrieben, die im klassischen Azure-Portal durchgeführt werden. Weitere Informationen zur Verwendung von anderen Portalen als dem Azure-Portal zum Verwalten Ihres Azure AD-Verzeichnisses finden Sie unter [Verwalten Ihres Azure AD-Verzeichnisses](active-directory-administer.md).

1. Wählen Sie im [klassischen Azure-Portal](https://manage.windowsazure.com) die Option **Active Directory**, und wählen Sie dann den Namen des Verzeichnisses Ihrer Organisation.

2. Wählen Sie die Registerkarte **Gruppen**.

3. Wählen Sie **Gruppe hinzufügen**.

4. Geben Sie im Fenster **Gruppe hinzufügen** den Namen und die Beschreibung einer Gruppe ein.


## Wie kann ich einzelne Benutzer einer Sicherheitsgruppe hinzufügen oder diese daraus entfernen?

**So fügen Sie einer Gruppe einen einzelnen Benutzer hinzu**

1. Wählen Sie im [klassischen Azure-Portal](https://manage.windowsazure.com) die Option **Active Directory**, und wählen Sie dann den Namen des Verzeichnisses Ihrer Organisation.

2. Wählen Sie die Registerkarte **Gruppen**.

3. Öffnen Sie die Gruppe, der Sie Mitglieder hinzufügen möchten. Öffnen Sie die Registerkarte **Mitglieder** der ausgewählten Gruppe, wenn sie nicht bereits angezeigt wird.

4. Wählen Sie **Mitglieder hinzufügen**.

5. Wählen Sie auf der Seite **Mitglieder hinzufügen** den Namen des Benutzers oder der Gruppe aus, den bzw. die Sie als Mitglied der Gruppe hinzufügen möchten. Stellen Sie sicher, dass dieser Name dem Bereich **Ausgewählt** hinzugefügt wird.


**So entfernen Sie einen einzelnen Benutzer aus einer Gruppe**

1. Wählen Sie im [klassischen Azure-Portal](https://manage.windowsazure.com) die Option **Active Directory**, und wählen Sie dann den Namen des Verzeichnisses Ihrer Organisation.

2. Wählen Sie die Registerkarte **Gruppen**.

3. Öffnen Sie die Gruppe, aus der Sie Mitglieder entfernen möchten.

4. Wählen Sie die Registerkarte **Mitglieder**, klicken Sie auf der Seite dieser Gruppe auf den Namen des Mitglieds, das Sie aus dieser Gruppe entfernen möchten, und klicken Sie dann auf **Entfernen**.

6. Bestätigen Sie in der Aufforderung, dass Sie dieses Mitglied aus der Gruppe entfernen möchten.


## Wie kann ich die Mitgliedschaft einer Gruppe dynamisch verwalten?

In Azure AD können Sie sehr leicht eine einfache Regel einrichten, um zu bestimmen, welche Benutzer Mitglieder der Gruppe sein sollen. Eine einfache Regel ist eine Regel, bei der nur ein einzelner Vergleich vorgenommen wird. Wenn eine Gruppe beispielsweise einer SaaS-Anwendung zugewiesen ist, können Sie eine Regel zum Hinzufügen von Benutzern mit der Position „Vertriebsmitarbeiter“ einrichten. Diese Regel gewährt dann für alle Benutzer im Verzeichnis, die diese Position aufweisen, Zugriff auf diese SaaS-Anwendung.

Wenn sich Attribute eines Benutzers ändern, bewertet das System alle dynamischen Gruppenregel in einem Verzeichnis, um zu ermitteln, ob die Attributänderung des Benutzers Vorgänge zum Hinzufügen oder Löschen von Gruppen auslöst. Falls ein Benutzer eine Regel für eine Gruppe erfüllt, wird er als Mitglieder zu dieser Gruppe hinzugefügt. Wenn er die Regel einer Gruppe nicht mehr erfüllt, deren Mitglied er ist, wird er aus dieser Gruppe entfernt.

> [AZURE.NOTE] Sie können eine Regel für die dynamische Mitgliedschaft für Sicherheits- oder Office 365-Gruppen einrichten. Geschachtelte Gruppenmitgliedschaften werden für die gruppenbasierte Zuweisung zu Anwendungen derzeit nicht unterstützt.
>
> Für dynamische Gruppenmitgliedschaften muss eine Azure AD Premium-Lizenz folgenden Personen zugewiesen werden:
>
> - Dem Administrator, der die Regel für eine Gruppe verwaltet
> - Alle Mitglieder der Gruppe

**So aktivieren Sie die dynamische Mitgliedschaft für eine Gruppe**

1. Wählen Sie im [klassischen Azure-Portal](https://manage.windowsazure.com) die Option **Active Directory**, und wählen Sie dann den Namen des Verzeichnisses Ihrer Organisation.

2. Wählen Sie die Registerkarte **Gruppen**, und öffnen Sie die Gruppe, die Sie bearbeiten möchten.

3. Wählen Sie die Registerkarte **Konfigurieren**, und legen Sie dann **Dynamische Mitgliedschaften aktivieren** auf **Ja** fest.

4. Richten Sie eine einfache Regel für die Gruppe ein, um die Funktionsweise dynamischer Mitgliedschaften für diese Gruppe zu steuern. Stellen Sie sicher, dass die Option **Benutzer hinzufügen, wobei** ausgewählt ist, und wählen Sie dann aus der Liste eine Benutzereigenschaft (z. B. Abteilung, Position usw.) aus.

5. Wählen Sie als Nächstes eine Bedingung aus (Ungleich, Gleich, Beginnt nicht mit, Beginnt mit, Enthält nicht, Enthält, Keine Übereinstimmung, Übereinstimmung).

6. Geben Sie einen Vergleichswert für die ausgewählte Benutzereigenschaft an.

Weitere Informationen zur Erstellung *erweiterter* Regeln (mit mehreren Vergleichen) für dynamische Gruppenmitgliedschaften finden Sie unter [Verwenden von Attributen zum Erstellen erweiterter Regeln](active-directory-accessmanagement-groups-with-advanced-rules.md).

## Zusätzliche Informationen

Diese Artikel enthalten zusätzliche Informationen zu Azure Active Directory.

* [Verwalten des Zugriffs auf Ressourcen mit Azure Active Directory-Gruppen](active-directory-manage-groups.md)

* [Azure Active Directory-Cmdlets zum Konfigurieren von Gruppeneinstellungen](active-directory-accessmanagement-groups-settings-cmdlets.md)

* [Artikelindex für die Anwendungsverwaltung in Azure Active Directory](active-directory-apps-index.md)

* [Was ist Azure Active Directory?](active-directory-whatis.md)

* [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md)

<!---HONumber=AcomDC_0928_2016-->