<properties
   pageTitle="Hinzufügen oder Entfernen einer Benutzerrolle | Microsoft Azure"
   description="Erfahren Sie, wie Sie mit der Anwendung Azure Active Directory Privileged Identity Management Rollen zu privilegierten Identitäten hinzufügen."
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
   ms.date="04/18/2016"
   ms.author="kgremban"/>

# Azure AD Privileged Identity Management: Hinzufügen oder Entfernen einer Benutzerrolle

In Azure Active Directory (AD) kann ein globaler Administrator (oder ein Unternehmensadministrator) festlegen, welchen Benutzern Rollen in Azure AD **permanent** zugewiesen werden. Dies erfolgt über PowerShell-Cmdlets wie `Add-MsolRoleMember` und `Remove-MsolRoleMember`. Administratoren können auch das klassische Azure-Portal verwenden, wie unter [Zuweisen von Administratorrollen in Azure Active Directory (Azure AD)](active-directory-assign-admin-roles.md) beschrieben.

Die Anwendung Azure AD Privileged Identity Management ermöglicht Sicherheitsadministratoren auch das Festlegen von permanenten Rollenzuweisungen. Administratoren können ebenfalls Kandidaten für die **temporäre** Zuweisung zu diesen Rollen hinzufügen oder entfernen. Ein Kandidat kann eine Rolle bei Bedarf aktivieren, und die entsprechenden Berechtigungen laufen nach einem bestimmten Zeitraum ab.

## Verwalten von Rollen mit PIM im Azure-Portal

Sie können Benutzern Ihrer Organisation in Azure AD verschiedene Rollen zuweisen. Mit diesen Rollenzuweisungen wird gesteuert, welche Aufgaben (beispielsweise das Hinzufügen oder Entfernen von Benutzern oder das Ändern von Diensteinstellungen) Benutzer in Azure AD, Office 365 und weiteren Microsoft-Diensten und -Anwendungen ausführen können. Weitere Informationen zu den verfügbaren Rollen finden Sie unter [Rollen in Azure AD Privileged Identity Management](active-directory-privileged-identity-management-roles.md).

Um einen Benutzer mithilfe von Privileged Identity Management zu einer Rolle hinzuzufügen oder daraus zu entfernen, öffnen Sie das PIM-Dashboard, und klicken Sie auf die Schaltfläche **Benutzer in Administratorrollen** oder wählen aus der Rollentabelle eine bestimmte Rolle aus (beispielsweise den globalen Administrator).

> [AZURE.NOTE] Wenn PIM im Azure-Portal noch nicht aktiviert ist, finden Sie die erforderlichen Informationen unter [Erste Schritte mit Azure AD Privileged Identity Management](active-directory-privileged-identity-management-getting-started.md).

Wenn Sie einem anderen Benutzer Zugriff zu PIM gewähren möchten, werden die Rollen, die ein Benutzer für PIM benötigt, unter [Gewähren von Zugriff auf PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md) beschrieben.

## Hinzufügen eines Benutzers zu einer Rolle
Wechseln Sie zum Blatt „Rolle“, indem Sie eine Rolle im Azure AD PIM-Dashboard auswählen oder auf die Schaltfläche **Benutzer in Administratorrollen** klicken.

1. Klicken Sie auf **Hinzufügen**.
  - Wenn Sie hierher gelangt sind, indem Sie in der Rollentabelle auf eine Benutzerrolle geklickt haben, ist die Rolle bereits ausgewählt.  
  - Klicken Sie auf **Rolle auswählen**, und wählen Sie aus der Rollenliste eine Rolle aus. Beispiel: **Kennwortadministrator**.
2. Suchen Sie auf dem Blatt **Benutzer auswählen** nach dem Benutzer. Wenn der Benutzer im Verzeichnis enthalten ist, wird sein Konto während der Eingabe angezeigt.
3. Wählen Sie den Benutzer aus der Liste der Suchergebnisse aus, und klicken Sie auf **Fertig**.
4. Klicken Sie zum Speichern der Auswahl auf **OK**. Der von Ihnen ausgewählte Benutzer wird in der Liste angezeigt, und die Rolle ist standardmäßig als temporär festgelegt.

  >[AZURE.NOTE] Wenn die Rolle permanent sein soll, klicken Sie auf den Benutzer in der Liste. Die Informationen des Benutzers werden auf einem neuen Blatt angezeigt. Wählen Sie im Menü mit den Benutzerinformationen **Als permanent festlegen** aus. Dies ist erforderlich, wenn sich der Benutzer nicht für Azure Multi-Factor Authentication (MFA) registrieren kann oder wenn er ein Microsoft-Konto verwendet. Temporäre Administratoren werden während der Aktivierung aufgefordert, sich für MFA zu registrieren.

Nachdem dem Benutzer eine temporäre Rolle zugewiesen wurde, informieren Sie ihn darüber, dass er die Rolle entsprechend den Anweisungen unter [Aktivieren oder Deaktivieren einer Rolle](active-directory-privileged-identity-management-how-to-activate-role.md) aktivieren kann.

## Entfernen eines Benutzers aus einer Rolle

Sie können Benutzer aus temporären Rollenzuweisungen entfernen. Stellen Sie dabei jedoch sicher, dass immer mindestens ein Benutzer als permanenter globaler Administrator vorhanden ist.

Führen Sie die folgenden Schritte aus, um einen bestimmten Benutzer aus einer Rolle zu entfernen:

1. Wechseln Sie zu der Rolle in der Rollenliste, indem Sie eine Rolle im Azure AD PIM-Dashboard auswählen oder auf die Schaltfläche **Benutzer in Administratorrollen** klicken.
2. Klicken Sie auf den Benutzer in der Benutzerliste.
3. Klicken Sie auf **Entfernen**. Sie werden in einer Meldung aufgefordert, den Vorgang zu bestätigen.
4. Klicken Sie auf **Ja**, um die Rolle des Benutzers zu entfernen.

Wenn Sie nicht sicher sind, welche Benutzer ihre Rollenzuweisungen noch benötigen, können Sie [eine Sicherheitsüberprüfung für die Rolle starten](active-directory-privileged-identity-management-how-to-start-security-review.md).


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## Nächste Schritte
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!---HONumber=AcomDC_0420_2016-->