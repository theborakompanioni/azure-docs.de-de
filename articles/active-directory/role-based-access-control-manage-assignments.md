---
title: "Anzeigen von Zugriffszuweisungen für Azure-Ressourcen | Microsoft Docs"
description: "Anzeigen und Verwalten aller RBAC-Zuweisungen für alle Benutzer oder Gruppen im Azure-Portal"
services: active-directory
documentationcenter: 
author: andredm7
manager: femila
editor: jeffsta
ms.assetid: e6f9e657-8ee3-4eec-a21c-78fe1b52a005
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/04/2017
ms.author: andredm
ms.translationtype: HT
ms.sourcegitcommit: 5b6c261c3439e33f4d16750e73618c72db4bcd7d
ms.openlocfilehash: 72c695d08bdf5de003d51ffb0768184e1e4d00ba
ms.contentlocale: de-de
ms.lasthandoff: 08/28/2017

---
# <a name="view-access-assignments-for-users-and-groups-in-the-azure-portal"></a>Anzeigen von Zugriffszuweisungen für Benutzer und Gruppen im Azure-Portal
> [!div class="op_single_selector"]
> * [Verwalten des Zugriffs nach Benutzer oder Gruppe](role-based-access-control-manage-assignments.md)
> * [Verwalten des Zugriffs nach Ressource](role-based-access-control-configure.md)

Mit der rollenbasierten Zugriffssteuerung (RBAC) von Azure Active Directory können Sie den Zugriff auf Ihre Azure-Ressourcen verwalten. 

Der Zugriff kann mit RBAC sehr detailliert zugewiesen werden, denn es gibt zwei Möglichkeiten zum Einschränken der Berechtigungen:

* **Bereich:** RBAC-Rollenzuweisungen gelten für ein bestimmtes Abonnement, eine Ressourcengruppe oder eine Ressource. Ein Benutzer, dem Zugriff auf eine einzelne Ressource erteilt wird, kann nicht auf andere Ressourcen im selben Abonnement zugreifen.
* **Rolle**: Im Bereich der Zuweisung wird der Zugriff weiter eingeschränkt, indem Sie eine Rolle zuweisen. Rollen können auf allgemeiner Ebene erteilt werden (z.B. Benutzer) oder spezifisch sein (z.B. Leser virtueller Computer).

Rollen können nur von dem Abonnement, der Ressourcengruppe oder der Ressource aus zugewiesen werden, das bzw. die den Bereich für die Zuweisung darstellt. Allerdings können Sie alle Zugriffszuweisungen für einen bestimmten Benutzer oder eine bestimmte Gruppe an einem einzigen Ort anzeigen. In jedem Abonnement können Sie über bis zu 2.000 Rollenzuweisungen verfügen. 

Informieren Sie sich über das [Verwenden von Rollenzuweisungen zum Verwalten des Zugriffs auf Ihre Azure-Abonnementressourcen](role-based-access-control-configure.md).

## <a name="view-access-assignments"></a>Anzeigen von Zugriffszuweisungen
Um die Zugriffszuweisungen für einen einzelnen Benutzer oder eine Gruppe zu suchen, beginnen Sie in Azure Active Directory im [Azure-Portal](http://portal.azure.com).

1. Wählen Sie **Azure Active Directory**. Wenn diese Option nicht in der Navigationsliste angezeigt wird, wählen Sie **Weitere Dienste**, und scrollen Sie nach unten, um **Azure Active Directory** zu finden.
2. Wählen Sie **Benutzer und Gruppen**, und klicken Sie dann entweder auf **Alle Benutzer** oder auf **Alle Gruppen**. In diesem Beispiel liegt der Schwerpunkt auf einzelnen Benutzer.
    ![Verwalten von Benutzern und Gruppen in Azure Active Directory – Screenshot](./media/role-based-access-control-manage-assignments/rbac_users_groups.png)
3. Suchen Sie anhand des Namens oder Benutzernamens nach dem Benutzer.
4. Wählen Sie auf dem Benutzerblatt die Option **Azure-Ressourcen** . Alle Zugriffszuweisungen für diesen Benutzer werden angezeigt.

### <a name="read-permissions-to-view-assignments"></a>Leseberechtigungen zur Anzeige von Zuweisungen
Diese Seite zeigt nur die Zugriffszuweisungen, zu deren Anzeige Sie berechtigt sind. Beispielsweise verfügen Sie über Lesezugriff für das Abonnement A und rufen das Azure-Ressourcenblatt auf, um die Zuweisungen einer Benutzerin zu überprüfen. Sie können ihre Zugriffszuweisungen für Abonnement A anzeigen, sehen jedoch nicht, dass sie auch über Zugriffszuweisungen für Abonnement B verfügt.

## <a name="delete-access-assignments"></a>Löschen von Zugriffszuweisungen
Auf diesem Blatt können Sie Zugriffszuweisungen löschen, die einem Benutzer oder einer Gruppe direkt zugewiesen wurden. Wenn die Zugriffszuweisung von einer übergeordneten Gruppe geerbt wurde, müssen Sie zu der Ressource oder dem Abonnement wechseln und die Zuweisung dort verwalten.

1. Wählen Sie aus der Liste aller Zugriffszuweisungen für einen Benutzer oder eine Gruppe diejenige aus, die Sie löschen möchten.
2. Wählen Sie **Entfernen**, und wählen Sie dann zur Bestätigung **Ja**.
    ![Entfernen der Zugriffszuweisung – Screenshot](./media/role-based-access-control-manage-assignments/delete_assignment.png)

## <a name="next-steps"></a>Nächste Schritte

* Erste Schritte mit der rollenbasierten Zugriffssteuerung finden Sie unter [Verwenden von Rollenzuweisungen zum Verwalten des Zugriffs auf Ihre Azure-Abonnementressourcen](role-based-access-control-configure.md)
* Weitere Informationen finden Sie unter [Integrierte RBAC-Rollen in Azure](role-based-access-built-in-roles.md)


