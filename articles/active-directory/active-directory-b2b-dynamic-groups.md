---
title: Dynamische Gruppen und Azure Active Directory B2B-Zusammenarbeit | Microsoft-Dokumentation
description: Die Azure Active Directory B2B-Zusammenarbeit kann mit dynamischen Azure AD-Gruppen verwendet werden.
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
ms.date: 06/27/2017
ms.author: curtand
ms.reviewer: sasubram
ms.translationtype: Human Translation
ms.sourcegitcommit: 857267f46f6a2d545fc402ebf3a12f21c62ecd21
ms.openlocfilehash: 5818c41610c8c5df89abcb0dcd058bcbe9579ce7
ms.contentlocale: de-de
ms.lasthandoff: 06/28/2017


---

# <a name="dynamic-groups-and-azure-active-directory-b2b-collaboration"></a>Dynamische Gruppen und Azure Active Directory B2B-Zusammenarbeit

## <a name="what-are-dynamic-groups"></a>Was sind dynamische Gruppen?
Die dynamische Konfiguration der Mitgliedschaft in Sicherheitsgruppen für Azure Active Directory (Azure AD) ist im [Azure-Portal](https://portal.azure.com) verfügbar. Administratoren können Regeln einrichten, um in Azure Active Directory erstellte Gruppen anhand von Benutzerattributen (z.B. Benutzertyp, Abteilung oder Land) aufzufüllen. Mitglieder können auf der Grundlage ihrer Attribute automatisch zu einer Sicherheitsgruppe hinzugefügt oder daraus entfernt werden. Diese Gruppen können Zugriff auf Anwendungen oder Cloudressourcen (SharePoint-Websites, Dokumente) gewähren oder den Mitgliedern Lizenzen zuweisen. Weitere Informationen zu dynamischen Gruppen erhalten Sie unter [Dedizierte Gruppen in Azure Active Directory](active-directory-accessmanagement-dedicated-groups.md).

Für die Erstellung und Verwendung dynamischer Gruppen wird die passende [Azure AD-Lizenzierung (Premium P1 oder Premium P2)](https://azure.microsoft.com/pricing/details/active-directory/) benötigt. Weitere Informationen finden Sie im Artikel [Erstellen attributbasierter Regeln für dynamische Gruppenmitgliedschaft in Azure Active Directory](active-directory-groups-dynamic-membership-azure-portal.md).

## <a name="what-are-the-built-in-dynamic-groups"></a>Was sind die integrierten dynamischen Gruppen?
Die dynamische Gruppe **Alle Benutzer** ermöglicht es Mandantenadministratoren, mit nur einem Klick eine Gruppe zu erstellen, die alle Benutzer des Mandanten umfasst. Die Gruppe **Alle Benutzer** umfasst standardmäßig alle Benutzer im Verzeichnis, einschließlich Mitgliedern und Gästen.
Im neuen Azure Active Directory-Verwaltungsportal können Sie die Gruppe **Alle Benutzer** in der Ansicht „Gruppeneinstellungen“ aktivieren.

![Integrierte Gruppen](media/active-directory-b2b-dynamic-groups/built-in-groups.png)

## <a name="hardening-the-all-users-dynamic-group"></a>Härten der dynamischen Gruppe „Alle Benutzer“
Die Gruppe **Alle Benutzer** umfasst standardmäßig auch Ihre (Gast-)Benutzer für die B2B-Zusammenarbeit. Mit einer Regel zum Entfernen von Gastbenutzern kann die Gruppe **Alle Benutzer** noch besser geschützt werden. Die folgende Abbildung zeigt die Gruppe **Alle Benutzer** nach der Anpassung zum Ausschließen von Gästen.

![Aktivieren der Gruppe „Alle Benutzer“](media/active-directory-b2b-dynamic-groups/enable-all-users-group.png)

Unter Umständen empfiehlt es sich auch, eine neue dynamische Gruppe zu erstellen, die nur Gastbenutzer enthält, um Richtlinien auf sie anwenden zu können (etwa Azure AD-Richtlinien für den bedingten Zugriff).
Eine solche Gruppe könnte beispielsweise wie folgt aussehen:

![Ausschließen von Gastbenutzern](media/active-directory-b2b-dynamic-groups/exclude-guest-users.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Artikel zur Azure AD B2B-Kollaboration:

* [Was ist die Azure AD B2B-Zusammenarbeit?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Eigenschaften von B2B-Zusammenarbeitsbenutzern](active-directory-b2b-user-properties.md)
* [Hinzufügen eines B2B-Zusammenarbeitsbenutzers zu einer Rolle](active-directory-b2b-add-guest-to-role.md)
* [Delegieren von Einladungen zur B2B-Zusammenarbeit](active-directory-b2b-delegate-invitations.md)
* [B2B-Zusammenarbeit: Code- und PowerShell-Beispiele](active-directory-b2b-code-samples.md)
* [Konfigurieren von SaaS-Apps für die B2B-Zusammenarbeit](active-directory-b2b-configure-saas-apps.md)
* [Benutzertoken für die B2B-Zusammenarbeit](active-directory-b2b-user-token.md)
* [Zuordnen von Benutzeransprüchen für die B2B-Zusammenarbeit](active-directory-b2b-claims-mapping.md)
* [Externe Office 365-Freigabe](active-directory-b2b-o365-external-user.md)
* [Aktuelle Einschränkungen der B2B-Zusammenarbeit](active-directory-b2b-current-limitations.md)

