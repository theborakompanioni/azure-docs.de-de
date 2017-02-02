---
title: "Hinzufügen oder Ändern von Azure-Administratorrollen | Microsoft-Dokumentation"
description: "Informationen zum Hinzufügen oder Ändern des Co-Administrators, Dienstadministrators und Kontoadministrators in Azure"
services: 
documentationcenter: 
author: genlin
manager: vikdesai
editor: 
tags: billing
ms.assetid: 13a72d76-e043-4212-bcac-a35f4a27ee26
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/8/2016
ms.author: genli
translationtype: Human Translation
ms.sourcegitcommit: 9083872545675cb700f4cfae675331484aed933b
ms.openlocfilehash: 9a3c67d88fc1da6582e07d948cf5be5a2d6db3d2


---
# <a name="how-to-add-or-change-azure-administrator-roles"></a>Hinzufügen oder Ändern von Azure-Administratorrollen
 Kontoadministrator, Dienstadministrator und Co-Administrator sind die drei Arten von Administratorrollen in Microsoft Azure. Um die Abrechnungsinformationen anzuzeigen und die Abonnements zu verwalten, müssen Sie sich im [Kontocenter](https://account.windowsazure.com/Home/Index) als Kontoadministrator anmelden. Die folgende Tabelle beschreibt den Unterschied zwischen diesen drei Administratorrollen.

| Administratorrolle | Begrenzung | Beschreibung |
| --- | --- | --- |
| Kontoadministrator (AA) |1 pro Azure-Konto |Dies ist die Person, die sich für Azure-Abonnements angemeldet oder diese erworben hat und die für den Zugriff auf das [Kontocenter](https://account.windowsazure.com/Home/Index) und zum Ausführen verschiedener Verwaltungsaufgaben berechtigt ist. Dazu gehört z. B. die Möglichkeit zum Erstellen von Abonnements, Kündigen von Abonnements, Ändern der Abrechnung für ein Abonnement und Ändern des Dienstadministrators. |
| Dienstadministrator (SA) |1 pro Azure-Abonnement |Diese Rolle ist zum Verwalten von Diensten im [Azure-Portal](https://portal.azure.com)berechtigt. Standardmäßig ist der Kontoadministrator für ein neues Abonnement gleichzeitig auch der Dienstadministrator. |
| Co-Administrator (CA) im [klassischen Azure-Portal](https://manage.windowsazure.com) |200 pro Abonnement |Diese Rolle verfügt über die gleichen Zugriffsrechte wie der Dienstadministrator, kann jedoch die Zuordnung von Abonnements zu Azure-Verzeichnissen nicht ändern. |

Die rollenbasierte Zugriffssteuerung in Azure Active Directory (RBAC, Role-based Access Control) ermöglicht es, Benutzer mehreren Rollen hinzuzufügen. Weitere Informationen finden Sie unter [Rollenbasierte Zugriffssteuerung in Azure Active Directory](active-directory/role-based-access-control-configure.md).


## <a name="how-to-add-an-admin-for-a-subscription"></a>Hinzufügen eines Administrators für ein Abonnement
**Azure-Portal**

Um eine Person als Administrator für ein Abonnement im Azure-Portal hinzuzufügen, geben Sie ihr die Rolle „Besitzer“. Die Rolle „Besitzer“ kann nur die Ressourcen im Abonnement verwalten, die ihr zugewiesen sind. Diese Rolle besitzt keine Zugriffsberechtigung auf andere Abonnements. Die Besitzer, die Sie über das [Azure-Portal](https://portal.azure.com) hinzufügen, können im [klassischen Azure-Portal](https://manage.windowsazure.com) keine Ressourcen verwalten. 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Wählen Sie im Hub-Menü die Option **Abonnement** > *das Abonnement aus, auf das der Administrator zugreifen soll*berechtigt.

    ![newselectsub](./media/billing-add-change-azure-subscription-administrator/newselectsub.png)
3. Wählen Sie auf dem Blatt „Abonnement“ **Zugriffssteuerung (IAM)**> **Hinzufügen** aus.

    ![newsettings](./media/billing-add-change-azure-subscription-administrator/newsettings.png)
4. Wählen Sie **Rolle auswählen** > **Besitzer** aus.

    ![newselectrole](./media/billing-add-change-azure-subscription-administrator/newselectrole.png)

5. Geben Sie die E-Mail-Adresse des Benutzers ein, den Sie als Besitzer hinzufügen möchten, klicken Sie auf den Benutzer, und klicken Sie dann auf **Auswählen**.

    ![newadduser](./media/billing-add-change-azure-subscription-administrator/newadduser.png)

**Klassisches Azure-Portal**

1. Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com/)an.
2. Wählen Sie im Navigationsbereich die Optionen **Einstellungen**> **Administratoren**> **Hinzufügen**. </br>

    ![addcodmin](./media/billing-add-change-azure-subscription-administrator/addcoadmin.png)
3. Geben Sie die E-Mail-Adresse der Person ein, die Sie als Co-Administrator hinzufügen möchten, und wählen Sie dann das Abonnement aus, auf das der Co-Administrator zugreifen soll.</br>

    ![addcoadmin2](./media/billing-add-change-azure-subscription-administrator/addcoadmin2.png)</br>

Die folgende E-Mail-Adresse kann als Co-Administrator hinzugefügt werden:

* **Microsoft-Konto** (ehemals Windows Live ID) </br>
  Sie können ein Microsoft-Konto verwenden, um sich bei allen verbraucherorientierten Microsoft-Produkten und Clouddiensten anzumelden, z.B. Outlook (Hotmail), Skype (MSN), OneDrive, Windows Phone und Xbox LIVE.
* **Organizational account**</br>
  Ein Organisationskonto ist ein Konto, das unter Azure Active Directory erstellt wird. Die Adresse des Organisationskontos weist folgendes Format auf:
  
    user@&lt;Ihre Domäne&gt;.onmicrosoft.com

## <a name="limitations-and-restrictions-to-administrator-accounts"></a>Begrenzungen und Einschränkungen für Administratorkonten
* Jedes Azure-Abonnement ist einem Azure AD-Verzeichnis zugeordnet (das auch als Standardverzeichnis bezeichnet wird). Um das Standardverzeichnis zu finden, dem das Abonnement zugeordnet ist, navigieren Sie im [klassischen Azure-Portal](https://manage.windowsazure.com/) zu **Einstellungen** > **Abonnements**. Überprüfen Sie dort die Abonnement-ID, und suchen Sie dann das Standardverzeichnis.
* Wenn Sie mit einem Microsoft-Konto angemeldet sind, können Sie nur andere Microsoft-Konten oder Benutzer innerhalb des Standardverzeichnisses als Co-Administrator hinzufügen.
* Wenn Sie mit einem Organisationskonto angemeldet sind, können Sie andere Organisationskonten in Ihrer Organisation als Co-Administrator hinzufügen. Beispielsweise kann abby@contoso.com das Konto bob@contoso.com als Dienstadministrator oder Co-Administrator hinzufügen, aber nicht john@notcontoso.com, es sei denn, john@noncontoso.com ist Benutzer im Standardverzeichnis. Mit einem Organisationskonto angemeldete Benutzer können Benutzer mit Microsoft-Konten weiterhin als Dienstadministrator oder Co-Administrator hinzufügen.
* Nachdem es jetzt möglich ist, sich bei Azure mit einem Organisationskonto anzumelden, ändern sich die folgenden Kontoanforderungen für Dienstadministratoren und Co-Administratoren:

  | Anmeldemethode | Microsoft-Konto oder Benutzer im Standardverzeichnis als Co-Administrator oder Dienstadministrator hinzufügen? | Organisationskonto in der gleichen Organisation als Co-Administrator oder Dienstadministrator hinzufügen? | Organisationskonto in einer anderen Organisation als Co-Administrator oder Dienstadministrator hinzufügen? |
  | --- | --- | --- | --- |
  |  Microsoft Account |Ja |Nein |Nein |
  |  Organisationskonto |Ja |Ja |Nein |

## <a name="how-to-change-service-administrator-for-a-subscription"></a>Ändern des Dienstadministrators für ein Abonnement
Nur der Kontoadministrator kann den Dienstadministrator für ein Abonnement ändern.

1. Melden Sie sich als Kontoadministrator beim [Azure-Kontocenter](https://account.windowsazure.com/subscriptions) an.
2. Wählen Sie das Abonnement aus, das Sie ändern möchten.
3. Klicken Sie auf der rechten Seite auf **Abonnementdetails bearbeiten** . </br>

    ![editsub](./media/billing-add-change-azure-subscription-administrator/editsub.png)
4. Geben Sie im Feld **DIENSTADMINISTRATOR** die E-Mail-Adresse des neuen Dienstadministrators ein. </br>

    ![changeSA](./media/billing-add-change-azure-subscription-administrator/changeSA.png)

## <a name="how-to-change-the-account-administrator"></a>Ändern des Kontoadministrators
Informationen zum Übertragen des Besitzes des Azure-Kontos auf ein anderes Konto finden Sie unter [Übertragen eines Azure-Abonnements](billing-subscription-transfer.md).

## <a name="how-to-check-the-account-administrator-of-the-subscription"></a>Überprüfen des Kontoadministrators des Abonnements
Gehen Sie folgendermaßen vor, wenn Sie nicht sicher sind, wer der Kontoadministrator für ein Abonnement ist.

  1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
  2. Wählen Sie im Menü „Hub“ die Option **Abonnement**aus.
  3. Wählen Sie das zu überprüfende Abonnement aus, und sehen Sie unter **Einstellungen** nach.
  4. Wählen Sie **Eigenschaften**aus. Der Kontoadministrator des Abonnements wird im Feld **Kontoadministrator** angezeigt.  

## <a name="learn-more-about-resource-access-control-and-active-directory"></a>Weitere Informationen zu Ressourcenzugriffssteuerung und Active Directory
* Informationen dazu, wie der Zugriff auf Ressourcen in Microsoft Azure gesteuert wird, finden Sie unter [Grundlegendes zum Zugriff auf Ressourcen in Azure](active-directory/active-directory-understanding-resource-access.md).
* Weitere Informationen zur Beziehung von Azure Active Directory zu Ihrem Azure-Abonnement finden Sie unter [Beziehung zwischen Azure-Abonnements und Azure Active Directory](active-directory/active-directory-how-subscriptions-associated-directory.md) und [Zuweisen von Administratorrollen in Azure Active Directory](active-directory/active-directory-assign-admin-roles.md).

## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Wenden Sie sich an den Support.
[Wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), falls Sie weitere Hilfe benötigen, um das Problem schnell beheben zu lassen. 




<!--HONumber=Dec16_HO2-->


