---
title: Verwalten des Zugriffs auf die Azure-Abrechnung mithilfe von Rollen | Microsoft-Dokumentation
description: 
services: 
documentationcenter: 
author: vikramdesai01
manager: vikramdesai01
editor: 
tags: billing
ms.assetid: e4c4d136-2826-4938-868f-a7e67ff6b025
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/25/2017
ms.author: vikdesai
ms.translationtype: Human Translation
ms.sourcegitcommit: 1cc1ee946d8eb2214fd05701b495bbce6d471a49
ms.openlocfilehash: 3453e80246393f15cf0ee2eb29b2fb1d53f77078
ms.contentlocale: de-de
ms.lasthandoff: 04/26/2017


---
# <a name="manage-access-to-billing-information-for-azure-using-role-based-access-control"></a>Verwalten des Zugriffs auf Abrechnungsinformationen für Azure mithilfe der rollenbasierten Zugriffssteuerung

Sie können Mitgliedern Ihres Teams den Zugriff auf Azure-Abrechnungsinformationen gewähren, indem Sie Ihrem Abonnement eine der folgenden Benutzerrollen zuweisen: Kontoadministrator, Dienstadministrator, Co-Administrator, Besitzer, Mitwirkender, Leser und Abrechnungsleser. Diese haben dann Zugriff auf die Abrechnungsinformationen im [Azure-Portal](https://portal.azure.com/) und können die [Abrechnungs-APIs](billing-usage-rate-card-overview.md) verwenden, um Rechnungen (nach erfolgter Anmeldung) und Nutzungsdetails programmgesteuert abzurufen. Weitere Informationen dazu, wer Rollen zuweisen kann und welche Vorgänge mit welchen Rollen durchgeführt werden können, finden Sie unter [Rollen in Azure RBAC](../active-directory/role-based-access-built-in-roles.md).

## <a name="opt-in"></a>Festlegen weiterer Benutzer für den Zugriff auf Rechnungen

Der Kontoadministrator muss sich über das [Azure-Portal](https://portal.azure.com/) anmelden, um den Zugriff auf Rechnungen für andere Benutzer und über die API zu gewähren.

1. Wählen Sie als Kontoadministrator auf dem Blatt [Abonnements](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) im Azure-Portal Ihr Abonnement aus.

1. Wählen Sie **Rechnungen** und dann **Access to invoices** (Zugriff auf Rechnungen) aus.

1. **Aktivieren** Sie den Zugriff.

Durch Anmeldung können Benutzer mit den Rollen „Dienstadministrator“, „Co-Administrator“, „Besitzer“, „Mitwirkender“, „Leser“ und „Abrechnungsleser“ im Abonnement PDF-Rechnungen im Azure-Portal herunterladen. Allerdings stehen Rechnungen vor Dezember 2016 vorerst nur für den Kontoadministrator zur Verfügung.

Der Kontoadministrator kann auch konfigurieren, dass ihm Rechnungen per E-Mail gesendet werden. Weitere Informationen finden Sie unter [Empfangen Ihrer Rechnung per E-Mail](billing-download-azure-invoice-daily-usage-date.md).

## <a name="adding-users-to-the-billing-reader-role"></a>Hinzufügen von Benutzern zur Rolle „Abrechnungsleser“

Die Rolle „Abrechnungsleser“ hat schreibgeschützten Zugriff auf Abrechnungsinformationen des Abonnements im Azure-Portal, jedoch keinen Zugriff auf Dienste wie virtuelle Computer und Speicherkonten. Weisen Sie die Rolle „Abrechnungsleser“ einem Benutzer zu, der Zugriff auf die Abrechnungsinformationen des Abonnements benötigt, jedoch nicht auf die Funktion zum Verwalten von Azure-Diensten. Diese Rolle eignet sich für Benutzer in einer Organisation, die ausschließlich die Finanz- und Kostenverwaltung für Azure-Abonnements durchführen.

1. Wählen Sie auf dem Blatt [Abonnements](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) im Azure-Portal Ihr Abonnement aus.

1. Wählen Sie **Zugriffssteuerung (IAM)** aus, und klicken Sie dann auf **Hinzufügen**.

    ![Screenshot: IAM auf dem Blatt „Abonnement“](./media/billing-manage-access/select-iam.PNG)

1. Wählen Sie auf der Seite **Rolle auswählen** die Rolle **Abrechnungsleser** aus.

    ![Screenshot: Abrechnungsleser in der Popupansicht](./media/billing-manage-access/select-roles.PNG)

1. Geben Sie die E-Mail-Adresse des Benutzers ein, den Sie einladen möchten, und klicken Sie dann auf **OK**, um die Einladung zu senden.

    ![Screenshot: Eingeben der E-Mail-Adresse des Benutzers für die Einladung](./media/billing-manage-access/add-user.PNG)

1. Befolgen Sie die Anweisungen in der Einladungs-E-Mail zum Anmelden als Abrechnungsleser.

    ![Screenshot: Inhalte, die der Abrechnungsleser im Azure-Portal sehen kann](./media/billing-manage-access/billing-reader-view.png)

> [!NOTE]
> Das Feature „Abrechnungsleser“ befindet sich derzeit in der Vorschau und ist nur für die globale Azure-Cloud verfügbar.

## <a name="adding-users-to-other-roles"></a>Hinzufügen von Benutzern zu anderen Rollen

Benutzer in anderen Rollen, z.B. „Besitzer“ oder „Mitwirkender“, können nicht nur auf Abrechnungsinformationen, sondern auch auf Azure-Dienste zugreifen. Informationen zum Verwalten dieser Rollen finden Sie unter [Hinzufügen oder Ändern von Azure-Administratorrollen, die das Abonnement oder die Dienste verwalten](billing-add-change-azure-subscription-administrator.md).

## <a name="who-can-access-the-account-centerhttpsaccountwindowsazurecom"></a>Wer kann auf das [Kontocenter](https://account.windowsazure.com) zugreifen?

Lediglich der Kontoadministrator kann sich beim Kontocenter anmelden. Der Kontoadministrator ist der rechtmäßige Besitzer des Abonnements. Standardmäßig ist die Person, die sich für das Azure-Abonnement registriert oder dieses erworben hat, als Kontoadministrator festgelegt, es sei denn, der [Besitz des Abonnements wurde auf einen anderen Benutzer übertragen](billing-subscription-transfer.md). Der Kontoadministrator kann Abonnements erstellen und kündigen, die Rechnungsadresse für ein Abonnement ändern und Zugriffsrichtlinien für das Abonnement verwalten.

## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Wenden Sie sich an den Support.

Bei weiteren Fragen [wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), um das Problem schnell zu lösen.
