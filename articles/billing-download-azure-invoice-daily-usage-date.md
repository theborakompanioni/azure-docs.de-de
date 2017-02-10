---
title: "Herunterladen von Azure-Rechnungen und täglichen Nutzungsdaten | Microsoft Docs"
description: "Hier erfahren Sie, wie Sie Azure-Rechnungen und tägliche Nutzungsdaten herunterladen."
services: 
documentationcenter: 
author: genlin
manager: ruchic
editor: 
tags: billing
ms.assetid: 6d568d1d-3bd6-4348-97d0-1098b5fe0661
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/2/2016
ms.author: genli
translationtype: Human Translation
ms.sourcegitcommit: 9b864a55d413ec7775e96fced4770b414f379a23
ms.openlocfilehash: ffc5cae2c4559dd8c7a9042dc676485966b976f4


---
# <a name="how-to-download-your-azure-billing-invoice-and-daily-usage-data"></a>Herunterladen von Azure-Rechnungen und täglichen Nutzungsdaten
Die Abrechnung und die tägliche Nutzung können im [Azure-Kontocenter](https://account.windowsazure.com/subscriptions) heruntergeladen werden. Nur der Kontoadministrator hat die Berechtigung für die Abrechnung und Nutzungsinformationen. Den Kontoadministrator des Abonnements finden Sie unter [Übertragen eines Azure-Abonnements – Häufig gestellte Fragen](billing-subscription-transfer.md#faq).

## <a name="get-invoice-and-usage-from-azure-account-center"></a>Abrufen von Rechnung und Nutzung aus dem Azure-Kontocenter
1. Melden Sie sich im [Azure-Kontocenter](https://account.windowsazure.com/subscriptions) als Kontoadministrator an.
2. Wählen Sie das Abonnement, für das Sie die Rechnung und Nutzungsinformationen abrufen möchten.
3. Klicken Sie auf **ABRECHNUNGSVERLAUF**. 

    ![Screenshot mit Abrechnungsverlaufsoption](./media/billing-download-azure-invoice-daily-usage-date/Billinghisotry.png)

4. Sie können Ihre Aufstellungen für die letzten sechs Abrechnungszeiträume sowie den aktuellen, nicht berechneten Zeitraum anzeigen. 

    ![Screenshot, der Abrechnungszeiträume, Optionen zum Herunterladen von Rechnungen und Daten zur täglichen Nutzung sowie Gesamtgebühren für jeden Abrechnungszeitraum anzeigt](./media/billing-download-azure-invoice-daily-usage-date/billingSum.png)

5. Wählen Sie **Aktuelle Abrechnung anzeigen** , um eine Schätzung der Kosten zum Zeitpunkt der Erstellung der Schätzung anzuzeigen. Diese Informationen werden nur einmal täglich aktualisiert und umfassen möglicherweise nicht die gesamte Nutzung. Ihre monatliche Abrechnung kann von dieser Schätzung abweichen.

    ![Screenshot mit der Option „Aktuelle Erklärung anzeigen“](./media/billing-download-azure-invoice-daily-usage-date/billingSum2.png)

    ![Screenshot mit der Schätzung der aktuellen Gebühren](./media/billing-download-azure-invoice-daily-usage-date/billingSum3.png)

6. Wählen Sie **Rechnung herunterladen** aus, um eine Kopie Ihrer letzten PDF-Rechnung anzuzeigen. 

    ![Screenshot mit der Option „Rechnung herunterladen“](./media/billing-download-azure-invoice-daily-usage-date/DLInvoice1.png)

7. Wählen Sie **Nutzung herunterladen**, um die täglichen Nutzungsdaten als CSV-Datei herunterzuladen. Wenn zwei Versionen verfügbar sind, laden Sie Version 2 herunter.

    ![Screenshot mit der Option „Nutzung herunterladen“](./media/billing-download-azure-invoice-daily-usage-date/DLusage.png)

Weitere Informationen über Ihre Rechnung und Daten zur täglichen Nutzung finden Sie unter [Erläuterungen zur Rechnung für Microsoft Azure](./billing/billing-understand-your-bill.md).

## <a name="get-invoice-from-azure-portal"></a>Abrufen der Rechnung aus dem Azure-Portal
Sie können die Daten zur täglichen Nutzung im Azure-Portal anzeigen, jedoch nur die Rechnung herunterladen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als Kontoadministrator an. 
2. Wählen Sie im Menü „Hub“ die Option **Abonnements** aus. 

    ![Screenshot mit der Abonnementoption](./media/billing-download-azure-invoice-daily-usage-date/submenu.png) 

3. Wählen Sie auf dem Blatt **Abonnements** das anzuzeigende Abonnement aus, und wählen Sie dann **Abrechnung und Nutzung**. 

    ![Screenshot mit der Option „Abrechnung und Nutzung“](./media/billing-download-azure-invoice-daily-usage-date/billingandusage.png) 

4. Klicken Sie auf dem Blatt **Abrechnung und Nutzung** auf **Rechnung herunterladen**, um eine Kopie Ihrer PDF-Rechnung anzuzeigen. 

    ![Screenshot mit Abrechnungszeiträumen, der Option „Herunterladen“ und den Gesamtgebühren für jeden Abrechnungszeitraum](./media/billing-download-azure-invoice-daily-usage-date/billing4.png)

5. Sie können Ihre Daten zur tägliche Nutzung durch Klicken auf den Abrechnungszeitraum anzeigen. 

Weitere Informationen über Ihre Rechnung und Daten zur täglichen Nutzung finden Sie unter [Erläuterungen zur Rechnung für Microsoft Azure](./billing/billing-understand-your-bill.md).

## <a name="a-namenoinvoicea-why-dont-i-see-an-invoice-for-the-last-billing-period"></a><a name="noinvoice"></a> Warum sehe ich keine Rechnung für den letzten Abrechnungszeitraum?
Mehrere Gründe können dafür ausschlaggebend sein, dass Sie keine Rechnung sehen:
- Sie haben einen monatlichen Gutschriftenbetrag in Ihrem Abonnement, den Sie nicht überschritten haben, oder eine kostenlose Testversion. Eine Rechnung wird nur generiert, wenn Sie Geld schuldig sind.
- Es sind weniger als 30 Tage seit dem Abschluss Ihres Azure-Abonnements vergangen.
- Die Rechnung ist nicht noch erstellt. Warten Sie bis zum Ende des Abrechnungszeitraums.

## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Wenden Sie sich an den Support.
Bei weiteren Fragen [wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), um das Problem schnell zu lösen.




<!--HONumber=Dec16_HO2-->


