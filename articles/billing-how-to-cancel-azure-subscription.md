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
ms.date: 12/6/2016
ms.author: genli
translationtype: Human Translation
ms.sourcegitcommit: c23c92234b38eeb911bd316fffe5127bbad4de91
ms.openlocfilehash: e64e4a3baf48ecd5b16bf5a141455152b7a486f5


---
# <a name="cancel-your-azure-subscription"></a>Kündigen Ihres Azure-Abonnements
Als Kontoadministrator können Sie Ihr Azure-Abonnement kündigen. Nach der Kündigung Ihres Abonnements haben Sie keinen Zugriff auf Azure-Dienste und -Ressourcen mehr.

Vor der Kündigung Ihres Abonnements:

* Sichern Sie Ihre Daten. Wenn Sie Ihre Daten beispielsweise im Azure-Speicher oder SQL speichern, laden Sie eine Kopie herunter. Wenn Sie einen virtuellen Computer haben, speichern lokal ein Image davon.
* Beenden Sie Ihre Dienste. Wechseln Sie zur [Ressourcenseite im Verwaltungsportal](https://ms.portal.azure.com/?flight=1#blade/HubsExtension/Resources/resourceType/Microsoft.Resources%2Fresources), und **beenden** Sie alle ausgeführten virtuellen Computer, Anwendungen oder anderen Dienste.
* Ziehen Sie die Migration Ihrer Daten in Betracht. Informationen finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](./azure-resource-manager/resource-group-move-resources.md).

Wenn Sie einen kostenpflichtigen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) kündigen, werden Ihnen die Gebühren für die Laufzeit von 6 Monaten weiterhin monatlich in Rechnung gestellt.

## <a name="cancel-subscription-via-the-azure-portal"></a>Kündigen eines Abonnements über das Azure-Portal
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als Kontoadministrator an.

   > [!NOTE]
   > Nur der Kontoadministrator kann Abonnements kündigen. Wie Sie herausfinden, wer der Kontoadministrator des Abonnements ist, erfahren Sie unter [Häufig gestellte Fragen (FAQ)](billing-subscription-transfer.md#faq).
   >
   >

2. Wählen Sie im Menü „Hub“ die Option **Abonnements** aus.

    ![Schaltfläche „Abonnement“](./media/billing-download-azure-invoice-daily-usage-date/submenu.png)

3. Wählen Sie auf dem Blatt „Abonnements“ das Abonnement aus, das Sie kündigen möchten, und klicken Sie auf **Cancel Sub** (Abo kündigen), um das Blatt für die Kündigung zu öffnen.

    ![cancel_ibiza](./media/billing-how-to-cancel-azure-subscription/cancel_ibiza.png)
4. Geben Sie den Abonnementnamen ein, wählen Sie einen Kündigungsgrund aus, und klicken Sie unten auf dem Kündigungsblatt auf die Schaltfläche **Cancel Sub** (Abo kündigen).

> [!NOTE]
> Es dauert bis zu zehn Minuten, bis die Kündigung im Portal angezeigt wird. Die Kostenabrechnung endet hingegen sofort.
>
>

## <a name="cancel-subscription-via-the-azure-account-center"></a>Kündigen eines Abonnements über das Azure-Kontocenter
1. Melden Sie sich im [Azure-Kontocenter](https://account.windowsazure.com/subscriptions) als Kontoadministrator an.
2. Wählen Sie unter **Klicken Sie auf ein Abonnement, um Details und Nutzung anzuzeigen.**das Abonnement aus, das Sie kündigen möchten.

    ![selectsub](./media/billing-how-to-cancel-azure-subscription/Selectsub.png)
3. Wählen Sie rechts auf der Seite **Abonnement kündigen**.

    ![cancelsub](./media/billing-how-to-cancel-azure-subscription/cancelsub.png)
4. Wählen Sie **Ja, mein Abonnement kündigen**.

    ![cancelbox](./media/billing-how-to-cancel-azure-subscription/cancelbox.png)
5. Klicken Sie auf  ![checkbutton](./media/billing-how-to-cancel-azure-subscription/checkbutton.png) , um das Dialogfenster zu schließen und zu Ihrer Abonnementseite zurückkehren.

Nach der Kündigung Ihres Abonnements warten wir 90 Tage, bevor wir Ihre Daten endgültig löschen, für den Fall, dass Sie darauf zugreifen müssen oder Ihre Meinung ändern. Weitere Informationen finden Sie unter [Microsoft Trust Center – Sie sind im Besitz Ihrer Daten](https://go.microsoft.com/fwLink/p/?LinkID=822930&clcid=0x409).

## <a name="reactivate-subscription"></a>Erneutes Aktivieren von Abonnements
Wenn Sie Ihr Abonnement mit nutzungsbasierter Bezahlung versehentlich gekündigt haben, können Sie es [im Kontocenter erneut aktivieren](billing-subscription-become-disable.md).

## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Wenden Sie sich an den Support.
Wenn Sie weitere Fragen haben, [wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) , um das Problem schnell zu lösen.



<!--HONumber=Dec16_HO2-->


