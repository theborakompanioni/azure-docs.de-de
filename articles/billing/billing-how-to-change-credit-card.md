---
title: "Ändern der Kreditkarte für Azure | Microsoft-Dokumentation"
description: "Enthält Informationen zum Ändern der für die Zahlung eines Azure-Abonnements verwendeten Kreditkarte."
services: 
documentationcenter: 
author: genlin
manager: jlian
editor: 
tags: billing
ms.assetid: 15252ced-1841-4a66-ae79-2e58af1d3370
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/30/2017
ms.author: genli
ms.translationtype: HT
ms.sourcegitcommit: 9569f94d736049f8a0bb61beef0734050ecf2738
ms.openlocfilehash: 03764377b3ea0e17d4a192a7e05bb495ec56f331
ms.contentlocale: de-de
ms.lasthandoff: 08/31/2017

---
# <a name="add-update-or-remove-a-credit-or-debit-card-for-azure"></a>Hinzufügen, Aktualisieren oder Entfernen einer Kredit- oder Debitkarte für Azure

Im Kontocenter können Sie eine neue Kreditkarte hinzufügen, eine vorhandene Kreditkarte aktualisieren oder eine nicht verwendete Kreditkarte löschen. Sie können diese Änderungen nur als [Kontoadministrator](billing-subscription-transfer.md#whoisaa) vornehmen.

**Sie möchten auf die Bezahlung per Rechnung umsteigen?** Informationen dazu finden Sie unter [Zahlen für Azure-Abonnements auf Rechnung](billing-how-to-pay-by-invoice.md).
 
<a id="addcard"></a>

## <a name="add-a-new-credit-or-debit-card"></a>Hinzufügen einer neuen Kredit- oder Debitkarte

1. Melden Sie sich beim [Kontocenter](https://account.windowsazure.com/Subscriptions) als Kontoadministrator an.
1. Wählen Sie ein Abonnement aus.
1. Wählen Sie rechts auf der Seite die Option **Manage payment methods**(Zahlungsmethoden verwalten).

    ![Screenshot mit ausgewählter Option „Zahlungsmethoden verwalten“.](./media/billing-how-to-change-credit-card/changesub_new.png)
1. Wählen Sie das Pluszeichen („+“) aus, um eine Karte hinzuzufügen.

    ![Screenshot der Option „Bearbeiten“ neben der Zahlungsmethode](./media/billing-how-to-change-credit-card/editcard_new.png)
1. Geben Sie die Kredit- oder Debitkartendetails ein.
1. Wählen Sie **Speichern** aus. 

Wenn nach dem Hinzufügen der Kreditkarte eine Fehlermeldung ausgegeben wird, befolgen Sie die Anleitung unter [Ablehnung der Kreditkarte bei der Azure-Registrierung](billing-credit-card-fails-during-azure-sign-up.md).

## <a name="update-existing-credit-or-debit-card"></a>Aktualisieren einer vorhandenen Kredit- oder Debitkarte

Wenn Ihre Kreditkarte verlängert wird und die Kreditkartennummer gleich bleibt, aktualisieren Sie die bereits vorhandenen Kreditkartendetails (etwa das Ablaufdatum). Wenn sich die Kreditkartennummer aufgrund von Verlust, Diebstahl oder Ablauf ändert, führen Sie die Schritte im Abschnitt [Hinzufügen einer Kreditkarte als Zahlungsmethode](#addcard) aus. Die Kartenprüfnummer müssen Sie nicht aktualisieren.

1. Melden Sie sich beim [Azure-Kontocenter](https://account.windowsazure.com/Subscriptions) als Kontoadministrator an.
1. Wählen Sie das Abonnement aus, das mit der Karte verknüpft ist.
1. Wählen Sie **Zahlungsmethoden verwalten** aus.
1. Wählen Sie neben der Karte, die aktualisiert werden soll, **Bearbeiten** aus.
1. Aktualisieren Sie die Kredit- oder Debitkartendetails.
1. Wählen Sie **Speichern** aus.

## <a name="use-a-different-credit-card-for-the-azure-subscription"></a>Verwenden einer anderen Kreditkarte für das Azure-Abonnement

1. Melden Sie sich beim [Azure-Kontocenter](https://account.windowsazure.com/Subscriptions) als Kontoadministrator an.
1. Wählen Sie das Abonnement aus, das mit der Karte verknüpft ist.
1. Wählen Sie rechts auf der Seite die Option **Manage payment methods**(Zahlungsmethoden verwalten).
1. Klicken Sie neben der zu verwendenden Kreditkarte auf **Stattdessen verwenden**. Dadurch werden auch alle anderen Abonnements aktualisiert, die aktuell mit dieser Karte verknüpft sind. 

## <a name="remove-a-credit-or-debit-card-from-the-account"></a>Entfernen einer Kredit- oder Debitkarte aus dem Konto

1. Melden Sie sich beim [Azure-Kontocenter](https://account.windowsazure.com/Subscriptions) als Kontoadministrator an.
1. Wählen Sie das Abonnement aus, das mit der Karte verknüpft ist.
3. Wählen Sie rechts auf der Seite die Option **Manage payment methods**(Zahlungsmethoden verwalten).
4. Klicken Sie für die zu löschende Kreditkarte auf **Löschen**.

Wenn Ihre Kreditkarte anderen aktiven Microsoft-Abonnements zugeordnet ist, können Sie sie nicht aus Ihrem Azure-Konto entfernen. Entfernen Sie die Kreditkarte aus allen aktiven Abonnements, über die Sie bei Microsoft verfügen, und wiederholen Sie den Vorgang.

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

### <a name="my-subscription-is-disabled-why-cant-i-remove-my-credit-card-now"></a>Das Abonnement ist deaktiviert. Warum kann ich jetzt meine Kreditkarte nicht entfernen?

Nach der Deaktivierung oder Kündigung Ihres Abonnements warten wir 90 Tage, bevor wir Ihr Abonnement endgültig löschen. Während der Aufbewahrungsdauer wird die Zahlungsmethode beibehalten, für den Fall, dass Sie das Abonnement reaktivieren möchten. Anschließend wird das Abonnement vollständig gelöscht.

Wenn Sie Ihre Kredit- oder Debitkarte vor Ablauf des Beibehaltungszeitraums von 90 Tagen entfernen möchten, [wenden Sie sich an den Azure-Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

### <a name="why-do-i-keep-getting-your-login-session-has-expired-please-click-here-to-log-back-in"></a>Warum wird beständig die Meldung „Ihre Anmeldesitzung ist abgelaufen. Klicken Sie hier, um sich wieder anzumelden.“ angezeigt?

Wenn diese Fehlermeldung beständig angezeigt wird, selbst wenn Sie sich bereits abgemeldet und dann wieder angemeldet haben, versuchen Sie es erneut mit einer privaten Browsersitzung.

### <a name="how-do-i-use-a-different-card-for-each-subscription-i-have"></a>Wie kann ich für jedes meiner Abonnements eine andere Karte verwenden?

Wenn für Ihre Abonnements bereits ein und dieselbe Karte verwendet wird, ist es leider nicht möglich, sie zu trennen, um verschiedene Karten zu verwenden. Wenn Sie sich jedoch für ein neues Abonnement registrieren, können Sie eine neue Zahlungsmethode für dieses Abonnement auswählen.

### <a name="how-do-i-make-payments"></a>Wie tätige ich Zahlungen?

Wenn Sie eine Kreditkarte oder Debitkarte als Zahlungsmethode eingerichtet haben, wird diese Karte automatisch nach jedem Abrechnungszeitraum belastet. Sie müssen nichts weiter tun.

Wenn Sie [auf Rechnung zahlen](billing-how-to-pay-by-invoice.md), senden Sie Ihre Zahlung an den Empfänger, der unten auf der Rechnung angegeben ist.

### <a name="how-do-i-make-a-one-time-payment"></a>Wie tätige ich eine einmalige Zahlung?

Leider unterstützt Azure derzeit keine einmaligen Zahlungen für Kredit- oder Debitkarten. 

## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Support kontaktieren

[Wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), falls Sie weitere Hilfe benötigen, um das Problem schnell beheben zu lassen.

