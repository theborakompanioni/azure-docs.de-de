---
title: "Hinzufügen oder Entfernen einer Benutzerrolle | Microsoft Docs"
description: "Erfahren Sie, wie Sie mit der Anwendung Azure Active Directory Privileged Identity Management Rollen zu privilegierten Identitäten hinzufügen."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: 
ms.assetid: 6a47ced8-cf34-4ce8-bea2-e4fc548cfe22
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/24/2016
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 5dde6b98f72306d6fe78f3c12377d3ce2ec5ddea


---
# <a name="azure-ad-privileged-identity-management-how-to-add-or-remove-a-user-role"></a>Azure AD Privileged Identity Management: Hinzufügen oder Entfernen einer Benutzerrolle
In Azure Active Directory (AD) kann ein globaler Administrator (oder ein Unternehmensadministrator) festlegen, welchen Benutzern Rollen in Azure AD **permanent** zugewiesen werden. Dies erfolgt über PowerShell-Cmdlets wie `Add-MsolRoleMember` und `Remove-MsolRoleMember`. Administratoren können auch das klassische Azure-Portal verwenden, wie unter [Zuweisen von Administratorrollen in Azure Active Directory (Azure AD)](active-directory-assign-admin-roles.md)beschrieben.

Die Anwendung Azure AD Privileged Identity Management ermöglicht Administratoren für privilegierte Rollen auch das Festlegen von permanenten Rollenzuweisungen. Außerdem können Administratoren für privilegierte Rollen Benutzer als für Administratorrollen **geeignet** festlegen. Ein berechtigter Administrator kann die Rolle bei Bedarf aktivieren, und die entsprechenden Berechtigungen laufen nach einem bestimmten Zeitraum ab.

## <a name="manage-roles-with-pim-in-the-azure-portal"></a>Verwalten von Rollen mit PIM im Azure-Portal
Sie können Benutzern Ihrer Organisation in Azure AD, Office 365 und anderen Microsoft-Diensten und -Anwendungen verschiedene Administratorrollen zuweisen.  Weitere Informationen zu den verfügbaren Rollen finden Sie unter [Rollen in Azure AD Privileged Identity Management](active-directory-privileged-identity-management-roles.md).

Um einen Benutzer mithilfe von PIM (Privileged Identity Management) einer Rolle hinzuzufügen oder den Benutzer aus einer Rolle zu entfernen, öffnen Sie das PIM-Dashboard. Klicken Sie dann auf die Schaltfläche **Benutzer in Administratorrollen**, oder wählen Sie in der Tabelle mit den Rollen eine bestimmte Rolle aus (z.B. den globalen Administrator).

> [!NOTE]
> Wenn PIM im Azure-Portal noch nicht aktiviert ist, finden Sie die erforderlichen Informationen unter [Erste Schritte mit Azure AD Privileged Identity Management](active-directory-privileged-identity-management-getting-started.md) .
> 
> 

Wenn Sie einem anderen Benutzer Zugriff zu PIM gewähren möchten, werden die Rollen, die ein Benutzer für PIM benötigt, unter [Gewähren von Zugriff auf PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md)beschrieben.

## <a name="add-a-user-to-a-role"></a>Hinzufügen eines Benutzers zu einer Rolle
1. Wählen Sie im [Azure-Portal](https://portal.azure.com/)auf dem Dashboard die Kachel **Azure AD Privileged Identity Management** aus.
2. Wählen Sie **Privilegierte Rollen verwalten**aus.
3. Wählen Sie in der Tabelle **Rollenzusammenfassung** die Rolle aus, die Sie verwalten möchten.
4. Wählen Sie auf dem Blatt der Rolle **Hinzufügen**aus.
5. Klicken Sie auf **Benutzer auswählen**, und suchen Sie auf dem Blatt **Benutzer auswählen** nach dem Benutzer.  
6. Wählen Sie den Benutzer aus der Liste der Suchergebnisse aus, und klicken Sie auf **Fertig**.
7. Klicken Sie zum Speichern der Auswahl auf **OK** . Der von Ihnen ausgewählte Benutzer wird in der Liste als berechtigt für die Rolle angezeigt.

> [!NOTE]
> Neue Benutzer in einer Rolle sind standardmäßig für die Rolle nur berechtigt. Wenn die Rolle permanent sein soll, klicken Sie auf den Benutzer in der Liste. Die Informationen des Benutzers werden auf einem neuen Blatt angezeigt. Wählen Sie im Menü mit den Benutzerinformationen **Als permanent festlegen** aus.  
> Wenn sich ein Benutzer nicht für Azure Multi-Factor Authentication (MFA) registrieren kann oder ein Microsoft-Konto nutzt (in der Regel @outlook.com),, müssen Sie ihn in allen Rollen als permanent einrichten. Geeignete Administratoren werden während der Aktivierung aufgefordert, sich für MFA zu registrieren.
> 
> 

Nachdem der Benutzer für eine Rolle geeignet ist, informieren Sie ihn darüber, dass er die Rolle entsprechend den Anweisungen unter [Gewusst wie: Aktivieren oder Deaktivieren einer Rolle](active-directory-privileged-identity-management-how-to-activate-role.md)aktivieren kann.

## <a name="remove-a-user-from-a-role"></a>Entfernen eines Benutzers aus einer Rolle
Sie können Benutzer aus berechtigten Rollenzuweisungen entfernen. Stellen Sie dabei jedoch sicher, dass immer mindestens ein Benutzer als permanenter globaler Administrator vorhanden ist.

Führen Sie die folgenden Schritte aus, um einen bestimmten Benutzer aus einer Rolle zu entfernen:

1. Wechseln Sie zu der Rolle in der Rollenliste, indem Sie eine Rolle im Azure AD PIM-Dashboard auswählen oder auf die Schaltfläche **Benutzer in Administratorrollen** klicken.
2. Klicken Sie auf den Benutzer in der Benutzerliste.
3. Klicken Sie auf **Entfernen**. Sie werden in einer Meldung aufgefordert, den Vorgang zu bestätigen.
4. Klicken Sie auf **Ja** , um die Rolle vom Benutzer zu entfernen.

Wenn Sie nicht sicher sind, welche Benutzer ihre Rollenzuweisungen noch benötigen, können Sie [eine Zugriffsüberprüfung für die Rolle starten](active-directory-privileged-identity-management-how-to-start-security-review.md).

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Nächste Schritte
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]




<!--HONumber=Dec16_HO4-->


