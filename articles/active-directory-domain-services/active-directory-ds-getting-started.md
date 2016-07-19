<properties
	pageTitle="Azure AD-Domänendienste: Erstellen der Administratorengruppe für Azure AD-Domänencontroller | Microsoft Azure"
	description="Erste Schritte mit Azure Active Directory-Domänendiensten (Vorschau)"
	services="active-directory-ds"
	documentationCenter=""
	authors="mahesh-unnikrishnan"
	manager="stevenpo"
	editor="curtand"/>

<tags
	ms.service="active-directory-ds"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/06/2016"
	ms.author="maheshu"/>

# Azure AD-Domänendienste *(Vorschau)* – Erstellen der Administratorengruppe für Azure AD-Domänencontroller

Dieser Artikel erläutert die erforderlichen Konfigurationsaufgaben zum Aktivieren der Azure AD-Domänendienste für Ihren Azure AD-Mandanten.

## Aufgabe 1: Erstellen der Administratorengruppe für Azure AD-Domänencontroller
Die erste Aufgabe besteht darin, eine administrative Gruppe in Ihrem Azure Active Directory-Mandanten zu erstellen. Diese spezielle administrative Gruppe heißt **AAD DC Administrators**. Mitgliedern dieser Gruppe werden Administratorrechte auf Computern erteilt, die in die Domäne eingebunden sind, die Sie mit den Azure Active Directory-Domänendiensten einrichten. Nach dem Beitritt zu einer Domäne wird diese Gruppe der Gruppe "Administratoren" auf diesen Computern der Domäne hinzugefügt. Mitglieder dieser Gruppe können darüber hinaus auch mithilfe von Remotedesktop Remoteverbindungen mit den in die Domäne eingebundenen Computern herstellen.

> [AZURE.NOTE] Sie verfügen nicht über Domänenadministrator- oder Unternehmensadministratorberechtigungen in der verwalteten Domäne, die mithilfe der Azure AD-Domänendienste erstellt wurde. Da dies eine verwaltete Domäne ist, sind diese Berechtigungen durch den Dienst reserviert und stehen Benutzern innerhalb des Verzeichnisses nicht zur Verfügung. Sie können jedoch die in dieser Konfigurationsaufgabe verwendete spezielle Administratorgruppe verwenden, um einige privilegierte Vorgänge auszuführen. Zu diesen Vorgängen gehört das Einbinden von Computern in die Domäne, die Mitgliedschaft in der Administratorgruppe auf der Domäne beigetretenen Computern, das Konfigurieren von Gruppenrichtlinien usw.

In dieser Konfigurationsaufgabe erstellen Sie die administrative Gruppe und fügen ihr einen oder mehrere Benutzer in Ihrem Verzeichnis hinzu. Führen Sie die folgenden Schritte durch, um die Administratorgruppe für Azure Active Directory-Domänendienste zu erstellen:

1. Navigieren Sie zum **klassischen Azure-Portal** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).

2. Wählen Sie den Knoten **Active Directory** im linken Bereich aus.

3. Wählen Sie den Azure AD-Mandanten (Verzeichnis) aus, für den Sie Azure AD-Domänendienste aktivieren möchten. Beachten Sie, dass Sie für jedes Azure AD-Verzeichnis nur eine Domäne erstellen können.

    ![Auswählen des Azure AD-Verzeichnisses](./media/active-directory-domain-services-getting-started/select-aad-directory.png)

4. Klicken Sie auf die Registerkarte **Gruppen**.

5. Klicken Sie im Aufgabenbereich am unteren Rand der Seite auf **Gruppe hinzufügen**, um Ihrem Verzeichnis eine Gruppe hinzuzufügen.

6. Erstellen Sie eine Gruppe mit dem Namen **AAD DC Administrators**.

    > [AZURE.WARNING] Sie müssen eine Gruppe mit exakt diesem Namen erstellen, um den Zugriff innerhalb von Azure Active Directory-Domänendiensten zu aktivieren.

	![Erstellen der Administratorgruppe](./media/active-directory-domain-services-getting-started/create-admin-group.png)

7. Sie können eine Beschreibung für diese Gruppe hinzufügen, damit andere Benutzer innerhalb Ihres Azure AD-Mandanten wissen, dass diese Gruppe zum Erteilen von Administratorberechtigungen innerhalb der Azure Active Directory-Domänendienste verwendet wird.

8. Nachdem die Gruppe erstellt wurde, klicken Sie auf den Namen der Gruppe, um deren Eigenschaften anzuzeigen. Klicken Sie im unteren Bereich auf die Schaltfläche **Mitglieder hinzufügen**, um Benutzer als Mitglieder dieser Gruppe hinzuzufügen.

9. Wählen Sie im Dialogfeld **Mitglieder hinzufügen** die Benutzer aus, die Mitglieder dieser Gruppe werden sollen, und wählen Sie das Häkchen aus, wenn Sie fertig sind.

    ![Hinzufügen von Benutzern zur Administratorgruppe](./media/active-directory-domain-services-getting-started/add-group-members.png)

<br>

## Aufgabe 2: Erstellen oder Auswählen eines virtuellen Azure-Netzwerks
Die nächste Konfigurationsaufgabe besteht darin, [ein virtuelles Azure-Netzwerk zu erstellen oder auszuwählen](active-directory-ds-getting-started-vnet.md).

<!---HONumber=AcomDC_0706_2016-->