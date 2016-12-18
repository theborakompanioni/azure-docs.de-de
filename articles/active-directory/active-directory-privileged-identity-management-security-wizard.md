---
title: Der Sicherheits-Assistent von Azure AD Privileged Identity Management
description: Bei der ersten Verwendung der Erweiterung Azure Active Directory Privileged Identity Management wird ein Sicherheits-Assistent angezeigt. Dieser Artikel beschreibt die Schritte zur Verwendung des Assistenten.
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: 
ms.assetid: a53a3719-8cc7-4fc7-8164-aafca192871b
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/01/2016
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 03adc1873e8187a9104f42ba9b50df2ac0d015b5


---
# <a name="the-azure-ad-privileged-identity-management-security-wizard"></a>Der Sicherheits-Assistent von Azure AD Privileged Identity Management
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




<!--HONumber=Nov16_HO3-->


