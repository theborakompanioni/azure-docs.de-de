---
title: "Hinzufügen oder Ändern von Azure-Abonnementadministratorrollen | Microsoft Docs"
description: "Informationen zum Hinzufügen oder Ändern des Co-Administrators, Dienstadministrators und Kontoadministrators in Azure"
services: 
documentationcenter: 
author: genlin
manager: jlian
editor: 
tags: billing
ms.assetid: 13a72d76-e043-4212-bcac-a35f4a27ee26
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/12/2017
ms.author: genli
ms.translationtype: HT
ms.sourcegitcommit: 2c6cf0eff812b12ad852e1434e7adf42c5eb7422
ms.openlocfilehash: 29fee079bd8b2775d7b45fa37cadb73635a2eb60
ms.contentlocale: de-de
ms.lasthandoff: 09/13/2017

---
# <a name="add-or-change-azure-administrator-roles-that-manage-the-subscription-or-services"></a>Hinzufügen oder Ändern von Azure-Administratorrollen, die das Abonnement oder die Dienste verwalten

Sie können den Azure-Administrator ändern, der Ihr Azure-Abonnement oder die in Ihrem Abonnement verwendeten Azure-Dienste verwaltet. Um die Azure-Abrechnungsinformationen anzuzeigen und die Abonnements zu verwalten, müssen Sie sich im [Kontocenter](https://account.windowsazure.com/Home/Index) als Kontoadministrator anmelden. 

<a name="add-an-admin-for-a-subscription"></a>

## <a name="add-an-rbac-owner-admin-for-a-subscription-in-azure-portal"></a>Hinzufügen eines Administrators mit der RBAC-Rolle „Besitzer“ für ein Abonnement im Azure-Portal 

Um eine Person als Administrator für ein Abonnement im Azure-Portal hinzuzufügen, empfiehlt es sich, ihr die [RBAC](../active-directory/role-based-access-control-configure.md)-Rolle „Besitzer“ zuzuweisen. Benutzer mit der Rolle „Besitzer“ können die Ressourcen in dem von Ihnen zugewiesenen Abonnement verwalten, verfügen aber über keine Zugriffsrechte für andere Abonnements. Die Besitzer, die Sie über das [Azure-Portal](https://portal.azure.com) hinzufügen, können im [klassischen Azure-Portal](https://manage.windowsazure.com) keine Ressourcen verwalten.

1. Melden Sie sich bei der [Ansicht „Abonnements“ im Azure-Portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) an.
1. Wählen Sie das Abonnement aus, auf das der Administrator zugreifen soll.
1. Wählen Sie im Menü die Option **Zugriffssteuerung (IAM)** aus.
1. Wählen Sie **Hinzufügen** > **Rolle** > **Besitzer**. Geben Sie die E-Mail-Adresse des Benutzers ein, den Sie als Besitzer hinzufügen möchten. Wählen Sie den Benutzer und dann **Speichern** aus.

    ![Screenshot mit ausgewählter Besitzerrolle](./media/billing-add-change-azure-subscription-administrator/add-role.png)

### <a name="add-or-change-co-administrator"></a>Hinzufügen oder Ändern eines Co-Administrators

Nur ein Besitzer kann als Co-Administrator hinzugefügt werden. Andere Benutzer mit Rollen wie „Mitwirkender“ oder „Leser“ können nicht als Co-Administratoren hinzugefügt werden.

1. Wenn noch nicht erfolgt, fügen Sie einen Benutzer entsprechend den Anweisungen oben als Besitzer hinzu.
2. **Klicken Sie mit der rechten Maustaste** auf den soeben hinzugefügten Benutzer mit der Rolle „Besitzer“, und wählen Sie dann **Als Co-Administrator hinzufügen** aus. Wenn die Option **Als Co-Administrator hinzufügen** nicht angezeigt wird, aktualisieren Sie die Seite, oder versuchen Sie es mit einem anderen Internetbrowser. 

     ![Screenshot, auf dem der Co-Administrator hinzugefügt wird](./media/billing-add-change-azure-subscription-administrator/add-coadmin.png)

    >[!TIP]
    >Sie müssen das Konto „Besitzer“ als Co-Administrator hinzufügen, wenn der Benutzer die Azure-Dienste im [klassischen Azure-Portal](https://manage.windowsazure.com/) verwalten soll.

    Klicken Sie zum Entfernen der Co-Administratorberechtigung **mit der rechten Maustaste** auf den Benutzer „Co-Administrator“, und wählen Sie dann **Co-Administrator entfernen** aus.

    ![Screenshot, auf dem der Co-Administrator entfernt wird](./media/billing-add-change-azure-subscription-administrator/remove-coadmin.png)

<a name="change-service-administrator-for-a-subscription"></a>

## <a name="change-the-service-administrator-for-an-azure-subscription"></a>Ändern des Dienstadministrators für ein Azure-Abonnement

Nur der Kontoadministrator kann den Dienstadministrator für ein Abonnement ändern. Standardmäßig ist der Dienstadministrator bei der Registrierung identisch mit dem Kontoadministrator.

1. Stellen Sie sicher, dass Ihr Szenario unterstützt wird, indem Sie die [Einschränkungen beim Ändern von Dienstadministratoren](#limits) überprüfen.
1. Melden Sie sich beim [Kontocenter](https://account.windowsazure.com/subscriptions) als Kontoadministrator an.
1. Wählen Sie ein Abonnement aus.
1. Wählen Sie auf der rechten Seite die Option **Abonnementdetails bearbeiten** aus.

    ![Screenshot mit der Schaltfläche „Abonnementdetails bearbeiten“ im Kontocenter](./media/billing-add-change-azure-subscription-administrator/editsub.png)
1. Geben Sie im Feld **DIENSTADMINISTRATOR** die E-Mail-Adresse des neuen Dienstadministrators ein.

    ![Screenshot mit dem Feld zum Ändern der E-Mail-Adresse des Dienstadministrators](./media/billing-add-change-azure-subscription-administrator/changeSA.png)

<a name="limits"></a>

### <a name="limitations-for-changing-service-administrators"></a>Einschränkungen beim Ändern von Dienstadministratoren

* Jedes Abonnement ist einem Azure AD-Verzeichnis zugeordnet. Um das Verzeichnis zu finden, dem das Abonnement zugeordnet ist, navigieren Sie im [klassischen Azure-Portal](https://manage.windowsazure.com/) zu **Einstellungen** > **Abonnements**. Überprüfen Sie dort die Abonnement-ID, und suchen Sie dann das Verzeichnis.
* Wenn Sie mit einem Geschäfts-, Schul- oder Unikonto angemeldet sind, können Sie andere Konten in Ihrer Organisation als Dienstadministrator hinzufügen. Beispielsweise kann abby@contoso.com das Konto bob@contoso.com als Dienstadministrator hinzufügen, aber nicht john@notcontoso.com, es sei denn, john@notcontoso.com ist im Verzeichnis „contoso.com“ enthalten. Mit einem Geschäfts-, Schul- oder Unikonto angemeldete Benutzer können Benutzer mit Microsoft-Konten weiterhin als Dienstadministrator hinzufügen.

  | Anmeldemethode | Hinzufügen eines Microsoft-Kontobenutzers als Dienstadministrator? | Hinzufügen eines Geschäfts-, Schul- oder Unikontos in der gleichen Organisation als Dienstadministrator? | Hinzufügen eines Geschäfts-, Schul- oder Unikontos in einer anderen Organisation als Dienstadministrator? |
  | --- | --- | --- | --- |
  |  Microsoft Account |Ja |Nein |Nein |
  |  Geschäfts-, Schul- oder Unikonto |Ja |Ja |Nein |

## <a name="change-the-account-administrator-for-an-azure-subscription"></a>Ändern des Kontoadministrators für ein Azure-Abonnement

Informationen zum Ändern des Kontoadministrators für ein Abonnement finden Sie unter [Übertragen des Besitzes eines Azure-Abonnements auf ein anderes Konto](billing-subscription-transfer.md).

<a name="check-the-account-administrator-of-the-subscription"></a>

**Sie sind nicht sicher, wer der Kontoadministrator ist?** Folgen Sie diesen Schritten:

1. Melden Sie sich bei der [Ansicht „Abonnements“ im Azure-Portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) an.
1. Wählen Sie das zu überprüfende Abonnement aus, und sehen Sie unter **Einstellungen** nach.
1. Wählen Sie **Eigenschaften** aus. Der Kontoadministrator des Abonnements wird im Feld **Kontoadministrator** angezeigt.  

## <a name="types-of-azure-admin-accounts"></a>Arten von Azure-Administratorkonten

 Kontoadministrator, Dienstadministrator und Co-Administrator sind die drei Arten von Administratorrollen in Microsoft Azure. Die folgende Tabelle beschreibt den Unterschied zwischen diesen drei Administratorrollen.

| Administratorrolle | Begrenzung | Beschreibung |
| --- | --- | --- |
| Kontoadministrator (AA) |1 pro Azure-Konto |Dies ist die Person, die sich für Azure-Abonnements angemeldet oder diese erworben hat und die für den Zugriff auf das [Kontocenter](https://account.windowsazure.com/Home/Index) und zum Ausführen verschiedener Verwaltungsaufgaben berechtigt ist. Dazu gehört z. B. die Möglichkeit zum Erstellen von Abonnements, Kündigen von Abonnements, Ändern der Abrechnung für ein Abonnement und Ändern des Dienstadministrators. |
| Dienstadministrator (SA) |1 pro Azure-Abonnement |Diese Rolle ist zum Verwalten von Diensten im [Azure-Portal](https://portal.azure.com)berechtigt. Standardmäßig ist der Kontoadministrator für ein neues Abonnement gleichzeitig auch der Dienstadministrator. |
| Co-Administrator (CA) im [klassischen Azure-Portal](https://manage.windowsazure.com) |200 pro Abonnement |Diese Rolle verfügt über die gleichen Zugriffsrechte wie der Dienstadministrator, kann jedoch die Zuordnung von Abonnements zu Azure-Verzeichnissen nicht ändern. |

Die rollenbasierte Zugriffssteuerung in Azure Active Directory (RBAC, Role-based Access Control) ermöglicht es, Benutzer mehreren Rollen hinzuzufügen. Weitere Informationen finden Sie unter [Rollenbasierte Zugriffssteuerung in Azure Active Directory](../active-directory/role-based-access-control-configure.md).


## <a name="learn-more-about-resource-access-control-and-active-directory"></a>Weitere Informationen zu Ressourcenzugriffssteuerung und Active Directory

* Informationen dazu, wie der Zugriff auf Ressourcen in Microsoft Azure gesteuert wird, finden Sie unter [Grundlegendes zum Zugriff auf Ressourcen in Azure](../active-directory/active-directory-understanding-resource-access.md).
* Weitere Informationen zu Azure Active Directory finden Sie unter [Beziehung zwischen Azure-Abonnements und Azure Active Directory](../active-directory/active-directory-how-subscriptions-associated-directory.md) und [Zuweisen von Administratorrollen in Azure Active Directory](../active-directory/active-directory-assign-admin-roles.md).

## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Wenden Sie sich an den Support.

[Wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), falls Sie weitere Hilfe benötigen, um das Problem schnell beheben zu lassen.

