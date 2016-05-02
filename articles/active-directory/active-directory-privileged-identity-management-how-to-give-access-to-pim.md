<properties
   pageTitle="Gewähren des Zugriffs auf PIM | Microsoft Azure"
   description="Erfahren Sie, wie Sie mit der Erweiterung Azure Active Directory Privileged Identity Management Rollen zu Benutzern hinzufügen, sodass diese PIM verwalten können."
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

# Gewähren des Zugriffs zur Verwaltung von Azure AD Privileged Identity Management

Der erste Benutzer, der Azure AD Privileged Identity Management (PIM) für eine Organisation aktiviert, muss ein globaler Administrator sein. Andere globale Administratoren haben jedoch nicht standardmäßig Zugriff auf PIM und können daher temporäre Zuweisungen nicht verwalten. Um Zugriff auf PIM zu gewähren, kann der erste Benutzer den anderen Administratoren die Rolle des Sicherheitsadministrators zuweisen. Diese Zuweisung muss innerhalb von PIM erfolgen und kann nicht über PowerShell oder andere Portale geändert werden.

> [AZURE.NOTE] Für die Verwaltung von Azure AD PIM ist Azure MFA erforderlich. Da Microsoft-Konten nicht für Azure MFA registriert werden können, kann ein Benutzer, der sich mit einem Microsoft-Konto anmeldet, nicht auf Azure AD PIM zugreifen.

Stellen Sie sicher, dass immer mindestens zwei Benutzer mit der Rolle „Sicherheitsadministrator“ vorhanden sind, falls ein Benutzer gesperrt oder ein Konto gelöscht wird.

## Gewähren des Zugriffs für einen anderen Benutzer zur Verwaltung von PIM

1. Melden Sie sich am [Azure-Portal](https://portal.azure.com/) an, und wählen Sie auf dem Dashboard die App **Azure AD Privileged Identity Management** aus.
2. Wählen Sie die Rolle **Sicherheitsadministrator** aus. Es wird eine Liste der Benutzer angezeigt, die diese Rolle derzeit innehaben.
3. Klicken Sie auf **Hinzufügen**. Ein Assistenten-Blatt wird geöffnet. Die Rolle ist bereits ausgewählt.
4. Klicken Sie auf **Benutzer auswählen**. Das Blatt mit der Benutzerliste wird geöffnet.
5. Geben Sie den Namen des Benutzers in das Suchfeld ein. Wenn der Benutzer im Verzeichnis enthalten ist, wird sein Konto während der Eingabe angezeigt.
6. Wählen Sie den Benutzer aus den Suchergebnissen aus, und klicken Sie auf **Fertig**.
7. Klicken Sie zum Speichern der Auswahl auf **OK**. Der von Ihnen ausgewählte Benutzer wird in der Liste angezeigt, und die Rolle ist standardmäßig als temporär festgelegt.

  - Wenn die Rolle permanent sein soll, klicken Sie auf den Benutzer in der Liste. Die Informationen des Benutzers werden auf einem neuen Blatt angezeigt. Wählen Sie im Menü mit den Benutzerinformationen **Als permanent festlegen** aus.

8. Senden Sie dem Benutzer einen Link zur Azure-Dokumentation unter [Erste Schritte mit Azure AD Privileged Identity Management](active-directory-privileged-identity-management-getting-started.md).


## Entfernen der Zugriffsrechte eines Benutzers für die Verwaltung von PIM

Bevor Sie einen Benutzer aus der Rolle „Sicherheitsadministrator“ entfernen, stellen Sie sicher, dass dieser Rolle danach noch mindestens zwei Benutzer zugewiesen sind.

1. Klicken Sie auf dem PIM-Dashboard auf die Rolle **Sicherheitsadministrator**. Es wird eine Liste der Benutzer angezeigt, die diese Rolle derzeit innehaben.
2. Klicken Sie auf den Benutzer in der Benutzerliste.
3. Klicken Sie auf **Entfernen**. Eine Bestätigungsmeldung wird angezeigt.
4. Klicken Sie auf **Ja**, um die Rolle des Benutzers zu entfernen.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## Nächste Schritte
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!---HONumber=AcomDC_0420_2016-->