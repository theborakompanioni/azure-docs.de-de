---
title: Der Sicherheits-Assistent von Azure AD Privileged Identity Management
description: Bei der ersten Verwendung der Erweiterung Azure Active Directory Privileged Identity Management wird ein Sicherheits-Assistent angezeigt. Dieser Artikel beschreibt die Schritte zur Verwendung des Assistenten.
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: a53a3719-8cc7-4fc7-8164-aafca192871b
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/27/2017
ms.author: billmath
ms.custom: pim ; H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: b9a3b64d9de48f17a295ca7a9ea58cf26e8f83ed
ms.openlocfilehash: ba06eb7e9f3f09e2d1d04cb4118dd81b9e66853e
ms.contentlocale: de-de
ms.lasthandoff: 02/28/2017

---
# <a name="using-the-security-wizard-in-azure-ad-privileged-identity-management"></a>Verwenden des Sicherheits-Assistenten in Azure AD Privileged Identity Management 
Wenn Sie die erste Person sind, die Azure Privileged Identity Management (PIM) ausführt, wird Ihnen ein Assistent angezeigt. Der Assistent bietet Einblick in die Sicherheitsrisiken privilegierter Identitäten und informiert darüber, wie Sie diese Risiken mithilfe von PIM reduzieren. Sie müssen die vorhandenen Rollenzuweisungen im Assistenten nicht ändern, wenn Sie dies lieber später machen möchten.

## <a name="what-to-expect"></a>Ausblick
Bevor Ihre Organisation mit der Verwendung von PIM beginnt, sind alle Rollenzuweisungen permanent, d. h. die Benutzer besitzen diese Rollen ständig, auch wenn sie die zugehörigen Berechtigungen derzeit nicht benötigen.  Der erste Schritt des Assistenten bietet eine Liste der Rollen mit hohen Berechtigungen und zeigt an, wie viele Benutzer derzeit in diesen Rollen vorhanden sind. Sie können Details zu einer bestimmten Rolle anzeigen, um weitere Informationen zu den Benutzern zu erhalten und zu ermitteln, ob ein oder mehrere Benutzer unbekannt sind.

Im zweiten Schritt des Assistenten haben Sie die Möglichkeit, die Rollenzuweisungen des Administrators zu ändern.  

> [!WARNING]
> Es müssen unbedingt mindestens ein globaler Administrator sowie mehrere Administratoren für privilegierte Rollen mit einem Organisationskonto (keinem Microsoft-Konto) vorhanden sein. Wenn nur ein Administrator für privilegierte Rollen vorhanden ist, kann die Organisation PIM nicht mehr verwalten, sollte dieses Konto gelöscht werden.
> Verwenden Sie außerdem permanente Rollenzuweisungen, wenn ein Benutzer über ein Microsoft-Konto verfügt (ein Konto zum Anmelden bei Microsoft-Diensten wie Skype und Outlook.com). Wenn Sie planen, zur Aktivierung dieser Rolle MFA als erforderlich festzulegen, kann der Benutzer sie nicht übernehmen.
> 
> 

Nachdem Sie Änderungen vorgenommen haben, wird der Assistent nicht mehr angezeigt. Wenn Sie oder ein anderer Administrator für privilegierte Rollen PIM das nächste Mal verwenden, wird das PIM-Dashboard angezeigt.  

* Wenn Sie Benutzer zu Rollen hinzufügen oder aus Rollen entfernen oder permanente in berechtigte Zuweisungen ändern möchten, finden Sie weitere Informationen unter [Hinzufügen oder Entfernen einer Benutzerrolle](active-directory-privileged-identity-management-how-to-add-role-to-user.md).
* Wenn Sie weiteren Benutzern Verwaltungszugriff auf PIM gewähren möchten, finden Sie weitere Informationen unter [Gewähren des Zugriffs zur Verwaltung von PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md)

## <a name="next-steps"></a>Nächste Schritte
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]


