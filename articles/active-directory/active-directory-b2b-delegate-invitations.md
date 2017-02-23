---
title: Delegieren von Einladungen zur Azure Active Directory B2B-Zusammenarbeit | Microsoft-Dokumentation
description: Die Eigenschaften von Benutzern der Azure Active Directory B2B-Zusammenarbeit sind konfigurierbar.
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 02/06/2017
ms.author: sasubram
translationtype: Human Translation
ms.sourcegitcommit: 0c05cd490ee9125f7e5182cb502db6f4e9390094
ms.openlocfilehash: 6c6d757a770613498bedca0f8f3a965241d692eb


---

# <a name="delegate-invitations-for-azure-active-directory-b2b-collaboration"></a>Delegieren von Einladungen zur Azure Active Directory B2B-Zusammenarbeit

Nach der Aktualisierung der öffentlichen Vorschau der Azure Active Directory B2B-Zusammenarbeit müssen Sie jetzt kein globaler Administrator mehr sein, um Benutzer einzuladen. Sie können mithilfe von Richtlinien steuern, welcher Benutzer Einladungen senden darf, und Sie können Einladungen an Benutzer delegieren, die Rollen mit der Berechtigung für Einladungen besitzen. Eine wichtige neue Möglichkeit, die Einladung von Gastbenutzern zu delegieren, ist die Rolle „Gasteinladender“.

## <a name="guest-inviter-role"></a>Rolle „Gasteinladender“
Sie können einem Benutzer die Rolle „Gasteinladender“ zuweisen, damit dieser Einladungen senden kann. Sie müssen kein Mitglied der Rolle der globalen Administratoren sein, um Einladungen zu senden. Standardmäßig können auch normale Benutzer die Einladungs-API aufrufen, sofern Einladungen für normale Benutzer nicht durch den globalen Administrator deaktiviert wurden. Sie können hierfür sowohl das Azure-Portal als auch PowerShell verwenden.

Dieses Beispiel zeigt das Hinzufügen eines Benutzers zur Rolle „Gasteinladender“ in PowerShell:

```
Add-MsolRoleMember -RoleObjectId 95e79109-95c0-4d8e-aee3-d01accf2d47b -RoleMemberEmailAddress <RoleMemberEmailAddress >
```

## <a name="controlling-who-can-invite"></a>Steuern der Berechtigungen zum Einladen

![Steuern der Einladungsberechtigung](media/active-directory-b2b-delegate-invitations/control-who-to-invite.png)

In der Azure AD B2B-Zusammenarbeit kann der Mandantenadministrator folgende Einladungsrichtlinien festlegen:

1. Einladungen sind deaktiviert.
2. Nur Administratoren und Benutzer in der Rolle „Gasteinladender“ dürfen einladen.
3. Administratoren, Benutzer der Rolle „Gasteinladender“ und Mitglieder dürfen einladen.
4. Alle Benutzer, einschließlich Gästen, dürfen einladen.

Standardmäßig sind Mandanten auf Richtlinie 4 festgelegt (alle Benutzer, einschließlich Gästen, dürfen B2B-Benutzer einladen).

## <a name="next-steps"></a>Nächste Schritte

Weitere Artikel zur Azure AD B2B-Zusammenarbeit:

* [Was ist die Azure AD B2B-Zusammenarbeit?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Eigenschaften von B2B-Zusammenarbeitsbenutzern](active-directory-b2b-user-properties.md)
* [Hinzufügen eines B2B-Zusammenarbeitsbenutzers zu einer Rolle](active-directory-b2b-add-guest-to-role.md)
* [Dynamische Gruppen und B2B-Zusammenarbeit](active-directory-b2b-dynamic-groups.md)
* [B2B-Zusammenarbeit: Code- und PowerShell-Beispiele](active-directory-b2b-code-samples.md)
* [Konfigurieren von SaaS-Apps für die B2B-Zusammenarbeit](active-directory-b2b-configure-saas-apps.md)
* [Benutzertoken für die B2B-Zusammenarbeit](active-directory-b2b-user-token.md)
* [Zuordnen von Benutzeransprüchen für die B2B-Zusammenarbeit](active-directory-b2b-claims-mapping.md)
* [Externe Office 365-Freigaben](active-directory-b2b-o365-external-user.md)
* [Aktuelle Einschränkungen der B2B-Zusammenarbeit](active-directory-b2b-current-limitations.md)



<!--HONumber=Feb17_HO1-->


