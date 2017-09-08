---
title: Freigeben von Dashboards im Azure-Portal mit RBAC | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie ein Dashboard im Azure-Portal mithilfe der rollenbasierten Zugriffssteuerung (Role Based Access Control, RBAC) freigeben.
services: azure-portal
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 8908a6ce-ae0c-4f60-a0c9-b3acfe823365
ms.service: multiple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/01/2016
ms.author: tomfitz
ms.translationtype: HT
ms.sourcegitcommit: 646886ad82d47162a62835e343fcaa7dadfaa311
ms.openlocfilehash: ea0cf7ad074f95c2b49a92f9a8e32270a1d39b3a
ms.contentlocale: de-de
ms.lasthandoff: 08/24/2017

---
# <a name="share-azure-dashboards-by-using-role-based-access-control"></a>Freigeben von Azure-Dashboards mithilfe der rollenbasierter Zugriffssteuerung
Nach der Konfiguration eines Dashboards können Sie es veröffentlichen und für andere Benutzer in Ihrer Organisation freigeben. Sie gewähren anderen Benutzern den Zugriff auf Ihr Dashboard über die [rollenbasierte Zugriffssteuerung](../active-directory/role-based-access-control-configure.md) von Azure. Sie weisen einem Benutzer oder einer Gruppe von Benutzern einer Rolle zu, und mit dieser Rolle wird dann definiert, ob diese Benutzer das veröffentlichte Dashboard anzeigen oder ändern können. 

Alle veröffentlichten Dashboards werden als Azure-Ressourcen implementiert, d.h., sie sind als verwaltbare Elemente in Ihrem Abonnement vorhanden und in einer Ressourcengruppe enthalten.  Aus Sicht der Zugriffssteuerung unterscheiden sich Dashboards nicht von anderen Ressourcen, z.B. einem virtuellen Computer oder Speicherkonto.

> [!TIP]
> Einzelne Kacheln im Dashboard erzwingen basierend auf den von ihnen gezeigten Ressourcen eigene Anforderungen an die Zugriffssteuerung.  Sie können deshalb ein Dashboard entwerfen, das umfassend freigegeben werden kann, während die Daten auf den einzelnen Kacheln trotzdem geschützt sind.
> 
> 

## <a name="understanding-access-control-for-dashboards"></a>Grundlagen der Zugriffssteuerung für Dashboards
Bei der rollenbasierten Zugriffssteuerung können Sie Benutzer auf drei verschiedenen Bereichsebenen Rollen zuweisen:

* Abonnement
* Ressourcengruppe
* Ressource

Die von Ihnen zugewiesenen Berechtigungen werden vom Abonnement an die Ressource vererbt. Das veröffentlichte Dashboard ist eine Ressource. Daher kann es sein, dass Sie Benutzer bereits Rollen für das Abonnement zugewiesen haben, die auch für das veröffentlichte Dashboard funktionieren. 

Beispiel:  Ein Beispiel: Sie verfügen über ein Azure-Abonnement, und verschiedenen Mitgliedern Ihres Teams wurden die Rollen **Besitzer**, **Mitwirkender** oder **Leser** für das Abonnement zugewiesen. Benutzer, die als Leser festgelegt wurden, können Dashboards auflisten und anzeigen, aber sie können diese nicht ändern oder löschen.  Benutzer mit Lesezugriff können lokale Änderungen an einem veröffentlichten Dashboard vornehmen (z.B.  beim Behandeln eines Problems), haben aber nicht die Möglichkeit, diese Änderungen zur Veröffentlichung zurück an den Server zu übergeben.  Diese Benutzer können eine private Kopie des Dashboards zur eigenen Nutzung erstellen.

Sie können aber der Ressourcengruppe mit mehreren Dashboards oder einem einzelnen Dashboard Berechtigungen zuweisen. Beispielsweise können Sie entscheiden, dass eine Gruppe von Benutzern über begrenzte Berechtigungen in Bezug auf das Abonnement und gleichzeitig über einen besseren Zugriff auf ein bestimmtes Dashboard verfügen soll. Sie weisen diese Benutzer einer Rolle für das Dashboard zu. 

## <a name="publish-dashboard"></a>Veröffentlichen des Dashboards
Angenommen, Sie haben die Konfiguration eines Dashboards abgeschlossen, das Sie für eine Gruppe von Benutzern im Abonnement freigeben möchten. In den Schritten unten wird eine angepasste Gruppe mit dem Namen „Storage Managers“ verwendet, aber Sie können der Gruppe einen beliebigen Namen geben. Informationen zum Erstellen einer Active Directory-Gruppe und Hinzufügen von Benutzern zu dieser Gruppe finden Sie unter [Verwalten von Gruppen in Azure Active Directory](../active-directory/active-directory-accessmanagement-manage-groups.md).

1. Wählen Sie im Dashboard die Option **Freigeben**.
   
     ![„Freigeben“ wählen](./media/azure-portal-dashboard-share-access/select-share.png)
2. Vor dem Zuweisen des Zugriffs müssen Sie das Dashboard veröffentlichen. Standardmäßig wird das Dashboard in einer Ressourcengruppe mit dem Namen **Dashboards**veröffentlicht. Wählen Sie **Veröffentlichen**.
   
     ![Veröffentlichen](./media/azure-portal-dashboard-share-access/publish.png)

Das Dashboard wurde veröffentlicht. Wenn die aus dem Abonnement geerbten Berechtigungen geeignet sind, sind keine weiteren Schritte erforderlich. Andere Benutzer in Ihrer Organisation können je nach Abonnementebenenrolle auf das Dashboard zugreifen und Änderungen vornehmen. Im Rahmen dieses Tutorials weisen wir aber eine Gruppe von Benutzern einer Rolle für das Dashboard zu.

## <a name="assign-access-to-a-dashboard"></a>Zuweisen des Zugriffs auf ein Dashboard
1. Wählen Sie nach dem Veröffentlichen des Dashboards die Option **Benutzer verwalten**.
   
     ![Benutzer verwalten](./media/azure-portal-dashboard-share-access/manage-users.png)
2. Eine Liste mit den vorhandenen Benutzern wird angezeigt, die bereits einer Rolle für dieses Dashboard zugewiesen sind. Ihre Liste mit den vorhandenen Benutzern unterscheidet sich von der Abbildung unten. Normalerweise werden die Zuweisungen aus dem Abonnement geerbt. Wählen Sie **Hinzufügen**, um einen neuen Benutzer oder eine neue Gruppe hinzuzufügen.
   
     ![Benutzer hinzufügen](./media/azure-portal-dashboard-share-access/existing-users.png)
3. Wählen Sie die Rolle aus, die für die zu gewährenden Berechtigungen steht. Wählen Sie für dieses Beispiel die Option **Mitwirkender**.
   
     ![Rolle wählen](./media/azure-portal-dashboard-share-access/select-role.png)
4. Wählen Sie den Benutzer bzw. die Gruppe zum Zuweisen zur Rolle aus. Verwenden Sie das Suchfeld, wenn der gesuchte Benutzer oder die Gruppe in der Liste nicht angezeigt wird. Die Liste mit den verfügbaren Gruppen richtet sich nach den Gruppen, die Sie in Active Directory erstellt haben.
   
     ![Benutzer auswählen](./media/azure-portal-dashboard-share-access/select-user.png) 
5. Wählen Sie nach dem Hinzufügen von Benutzern oder Gruppen die Option **OK**. 
6. Die neue Zuweisung wird der Liste mit den Benutzern hinzugefügt. Beachten Sie, dass der **Zugriff** als **Zugewiesen** und nicht als **Geerbt** angezeigt wird.
   
     ![Zugewiesene Rollen](./media/azure-portal-dashboard-share-access/assigned-roles.png)

## <a name="next-steps"></a>Nächste Schritte
* Eine Liste mit den Rollen finden Sie unter [RBAC: Integrierte Rollen](../active-directory/role-based-access-built-in-roles.md).
* Informationen zum Verwalten von Ressourcen finden Sie unter [Verwalten von Azure-Ressourcen über das Portal](resource-group-portal.md).


