---
title: "Kündigen Ihres Azure-Abonnements | Microsoft Docs"
description: "Beschreibt, wie Sie Ihr Azure-Abonnement, etwa das Abonnement für die kostenlose Testversion, kündigen."
services: 
documentationcenter: 
author: genlin
manager: narmstr
editor: 
tags: billing
ms.assetid: 3051d6b0-179f-4e3a-bda4-3fee7135eac5
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/29/2017
ms.author: genli
translationtype: Human Translation
ms.sourcegitcommit: 5cce99eff6ed75636399153a846654f56fb64a68
ms.openlocfilehash: a8751fa70bd500a29a3e86de57de4fb919405136
ms.lasthandoff: 03/31/2017


---
# <a name="cancel-your-subscription-for-azure"></a>Kündigen Ihres Abonnements für Azure
Als [Kontoadministrator](billing-subscription-transfer.md#whoisaa) können Sie Ihr Azure-Abonnement kündigen. Nach der Kündigung Ihres Abonnements haben Sie keinen Zugriff auf Azure-Dienste und -Ressourcen mehr.

Vor der Kündigung Ihres Abonnements:

* Sichern Sie Ihre Daten. Wenn Sie Ihre Daten beispielsweise im Azure-Speicher oder SQL speichern, laden Sie eine Kopie herunter. Wenn Sie einen virtuellen Computer haben, speichern lokal ein Image davon.
* Beenden Sie Ihre Dienste. Wechseln Sie zur [Ressourcenseite im Verwaltungsportal](https://ms.portal.azure.com/?flight=1#blade/HubsExtension/Resources/resourceType/Microsoft.Resources%2Fresources), und **beenden** Sie alle ausgeführten virtuellen Computer, Anwendungen oder anderen Dienste.
* Ziehen Sie die Migration Ihrer Daten in Betracht. Informationen finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](../azure-resource-manager/resource-group-move-resources.md).

Wenn Sie einen kostenpflichtigen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) kündigen, werden Ihnen die Gebühren für die Laufzeit von 6 Monaten weiterhin monatlich in Rechnung gestellt.

## <a name="cancel-subscription-via-the-azure-portal"></a>Kündigen eines Abonnements über das Azure-Portal
1. Melden Sie sich als Kontoadministrator beim [Azure-Portal](https://portal.azure.com) an.

2. Wählen Sie links im Bereich „Azure-Dienste“ **Abonnements** aus.

    ![Screenshot mit der Schaltfläche „Abonnements“](./media/billing-download-azure-invoice-daily-usage-date/submenu.png)

3. Wählen Sie das zu kündigende Abonnement aus, und klicken Sie auf **Abonnement kündigen**.

    ![Screenshot mit der Schaltfläche zum Kündigen](./media/billing-how-to-cancel-azure-subscription/cancel_ibiza.png)
4. Geben Sie den Namen des Abonnements ein, und wählen Sie einen Grund für die Kündigung aus. 
5. Klicken Sie unten auf die Schaltfläche **Abonnement kündigen**.

## <a name="cancel-subscription-by-using-the-azure-account-center"></a>Kündigen eines Abonnements über das Azure-Kontocenter
1. Melden Sie sich beim [Azure-Kontocenter](https://account.windowsazure.com/subscriptions) als Kontoadministrator an.
2. Wählen Sie unter **Klicken Sie auf ein Abonnement, um Details und Nutzung anzuzeigen.**das Abonnement aus, das Sie kündigen möchten.

    ![Screenshot mit Beispiel für ein ausgewähltes Abonnement](./media/billing-how-to-cancel-azure-subscription/Selectsub.png)
3. Wählen Sie rechts auf der Seite **Abonnement kündigen**.

    ![Screenshot mit der Schaltfläche „Abonnement kündigen“](./media/billing-how-to-cancel-azure-subscription/cancelsub.png)
4. Wählen Sie **Ja, mein Abonnement kündigen**.

    ![Screenshot mit dem Dialogfeld zum Kündigen](./media/billing-how-to-cancel-azure-subscription/cancelbox.png)
5. Klicken Sie auf  ![Schaltfläche mit Häkchen-Symbol](./media/billing-how-to-cancel-azure-subscription/checkbutton.png) , um das Dialogfenster zu schließen und zu Ihrer Abonnementseite zurückkehren.

## <a name="what-happens-after-you-cancel-your-subscription"></a>Was geschieht nach der Kündigung Ihres Abonnements?
Es kann bis zu zehn Minuten dauern, bis die Kündigung im Portal angezeigt wird. Die Kostenabrechnung endet hingegen sofort.

Nach der Kündigung Ihres Abonnements warten wir 90 Tage, bevor wir Ihre Daten endgültig löschen, für den Fall, dass Sie darauf zugreifen müssen oder Ihre Meinung ändern. Weitere Informationen finden Sie unter [Microsoft Trust Center – Sie sind im Besitz Ihrer Daten](https://go.microsoft.com/fwLink/p/?LinkID=822930&clcid=0x409).

## <a name="reactivate-subscription"></a>Erneutes Aktivieren von Abonnements
Wenn Sie Ihr Abonnement mit nutzungsbasierter Bezahlung versehentlich gekündigt haben, können Sie es [im Kontocenter erneut aktivieren](billing-subscription-become-disable.md).

## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Wenden Sie sich an den Support.
Wenn Sie weitere Fragen haben, [wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), um das Problem schnell zu lösen.

