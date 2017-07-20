---
title: "Kündigen Ihres Azure-Abonnements | Microsoft Docs"
description: "Beschreibt, wie Sie Ihr Azure-Abonnement, etwa das Abonnement für die kostenlose Testversion, kündigen."
services: 
documentationcenter: 
author: genlin
manager: jlian
editor: 
tags: billing
ms.assetid: 3051d6b0-179f-4e3a-bda4-3fee7135eac5
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: genli
ms.translationtype: Human Translation
ms.sourcegitcommit: b1d56fcfb472e5eae9d2f01a820f72f8eab9ef08
ms.openlocfilehash: c415fada30aa0b0bd9b9d1e416bc37ef30653f68
ms.contentlocale: de-de
ms.lasthandoff: 07/06/2017


---
# <a name="cancel-your-subscription-for-azure"></a>Kündigen Ihres Abonnements für Azure

Als [Kontoadministrator](billing-subscription-transfer.md#whoisaa) können Sie Ihr Azure-Abonnement kündigen. Nach der Kündigung Ihres Abonnements haben Sie keinen Zugriff auf Azure-Dienste und -Ressourcen mehr.

Vor der Kündigung Ihres Abonnements:

* Sichern Sie Ihre Daten. Wenn Sie Ihre Daten beispielsweise im Azure-Speicher oder SQL speichern, laden Sie eine Kopie herunter. Wenn Sie einen virtuellen Computer haben, speichern lokal ein Image davon.
* Beenden Sie Ihre Dienste. Wechseln Sie zur [Ressourcenseite im Verwaltungsportal](https://ms.portal.azure.com/?flight=1#blade/HubsExtension/Resources/resourceType/Microsoft.Resources%2Fresources), und **beenden** Sie alle ausgeführten virtuellen Computer, Anwendungen oder anderen Dienste.
* Ziehen Sie die Migration Ihrer Daten in Betracht. Informationen finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](../azure-resource-manager/resource-group-move-resources.md).

Wenn Sie einen kostenpflichtigen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) kündigen, werden Ihnen die Gebühren für die Laufzeit von 6 Monaten weiterhin monatlich in Rechnung gestellt.

## <a name="cancel-subscription-using-the-azure-portal"></a>Kündigen eines Abonnements unter Verwendung des Azure-Portals

1. Wählen Sie auf der Seite [Abonnements](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) Ihr Abonnement aus.

1. Wählen Sie das zu kündigende Abonnement aus, und klicken Sie auf **Abonnement kündigen**.

    ![Screenshot mit der Schaltfläche zum Kündigen](./media/billing-how-to-cancel-azure-subscription/cancel_ibiza.png)

1. Befolgen Sie die Anweisungen, und schließen Sie die Kündigung ab.

## <a name="cancel-subscription-using-the-azure-account-center"></a>Kündigen eines Abonnements unter Verwendung von Azure Account Center

1. Melden Sie sich beim [Azure-Kontocenter](https://account.windowsazure.com/subscriptions) als Kontoadministrator an.

1. Wählen Sie unter **Klicken Sie auf ein Abonnement, um Details und Nutzung anzuzeigen.**das Abonnement aus, das Sie kündigen möchten.

    ![Screenshot mit Beispiel für ein ausgewähltes Abonnement](./media/billing-how-to-cancel-azure-subscription/Selectsub.png)

1. Wählen Sie rechts auf der Seite **Abonnement kündigen**.

    ![Screenshot mit der Schaltfläche „Abonnement kündigen“](./media/billing-how-to-cancel-azure-subscription/cancelsub.png)

1. Wählen Sie **Ja, mein Abonnement kündigen**.

    ![Screenshot mit dem Dialogfeld zum Kündigen](./media/billing-how-to-cancel-azure-subscription/cancelbox.png)

1. Klicken Sie auf  ![Schaltfläche mit Häkchen-Symbol](./media/billing-how-to-cancel-azure-subscription/checkbutton.png) , um das Dialogfenster zu schließen und zu Ihrer Abonnementseite zurückkehren.

## <a name="what-happens-after-i-cancel-my-subscription"></a>Was geschieht, nachdem ich mein Abonnement gekündigt habe?

Nach Ihrer Kündigung wird die Berechnung sofort beendet. Bis die Kündigung im Portal angezeigt wird, kann es allerdings bis zu 10 Minuten dauern.

Danach werden Ihre Dienste deaktiviert. Dies bedeutet, dass die Zuordnung Ihrer virtuellen Computer aufgehoben wird, ihre vorübergehenden IP-Adressen freigegeben werden und der Speicher nur noch im Lesezugriff verfügbar ist.

Sofern Sie nicht die kostenlose Testversion nutzen oder noch über Gutschriften verfügen, werden alle offenen Nutzungsgebühren abgerechnet, die zwischen Ihrem letzten Abrechnungszyklus und dem Kündigungsdatum angefallen sind. Sie erhalten Ihre letzte Rechnung am Ende des Abrechnungszyklus.

Nach der Kündigung Ihres Abonnements warten wir 90 Tage, bevor wir Ihre Daten endgültig löschen, für den Fall, dass Sie darauf zugreifen müssen oder Ihre Meinung ändern. Für die Aufbewahrung der Daten rechnen wir keine Gebühr ab. Weitere Informationen finden Sie in [Microsoft Trust Center – Wie wir Ihre Daten verwalten](https://go.microsoft.com/fwLink/p/?LinkID=822930&clcid=0x409).

## <a name="reactivate-subscription"></a>Erneutes Aktivieren von Abonnements

Wenn Sie Ihr Abonnement mit nutzungsbasierter Bezahlung versehentlich gekündigt haben, können Sie es [im Kontocenter erneut aktivieren](billing-subscription-become-disable.md).

Wenn es sich bei Ihrem Abonnement nicht um ein Abonnement mit nutzungsbasierter Bezahlung handelt, nehmen Sie bitte innerhalb von 90 Tagen nach der Kündigung Kontakt zum Support auf, um Ihr Abonnement erneut zu aktivieren.

## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Wenden Sie sich an den Support.

Wenn Sie weitere Fragen haben, [wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), um das Problem schnell zu lösen.

