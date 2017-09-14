---
title: "Informationen zu Ihrer Rechnung für Azure | Microsoft-Dokumentation"
description: "Es wird beschrieben, wie Sie die Nutzung und Abrechnung Ihres Azure-Abonnements anzeigen und besser verstehen können."
services: 
documentationcenter: 
author: tonguyen10
manager: tonguyen
editor: 
tags: billing
ms.assetid: 32eea268-161c-4b93-8774-bc435d78a8c9
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/31/2017
ms.author: tonguyen
ms.translationtype: HT
ms.sourcegitcommit: 4eb426b14ec72aaa79268840f23a39b15fee8982
ms.openlocfilehash: 668b32e99ba9a3bdf8e8f16ac51c35c609444cd9
ms.contentlocale: de-de
ms.lasthandoff: 09/06/2017

---
# <a name="understand-your-bill-for-microsoft-azure"></a>Informationen zu Ihrer Rechnung für Microsoft Azure
Um Ihre Azure-Rechnung besser zu verstehen, können Sie Ihre Rechnung mit der Datei mit den ausführlichen Daten zur täglichen Nutzung und den Kostenverwaltungsberichten im Azure-Portal vergleichen.

>[!NOTE]
>Dieser Artikel gilt nicht für Enterprise Agreement-Kunden (EA). Wenn Sie EA-Kunde sind, [finden Sie die Dokumentation zu Rechnungen im Enterprise Portal](https://ea.azure.com/helpdocs/understandingYourInvoice).  

Eine PDF-Datei mit Ihrer Rechnung und eine Kopie Ihrer Datei zur ausführlichen täglichen Nutzung (CSV-Download) finden Sie unter [Herunterladen oder Anzeigen Ihrer Azure-Rechnungen und täglichen Nutzungsdaten](billing-download-azure-invoice-daily-usage-date.md). 

Detaillierte Erläuterungen zu den Benennungen und Beschreibungen Ihrer Rechnung und der Datei mit ausführlichen Daten zur täglichen Nutzung finden Sie unter [Grundlegendes über Benennungen in Ihrer Microsoft Azure-Rechnung](billing-understand-your-invoice.md) und [Grundlegendes über Benennungen in der Datei mit ausführlichen Nutzungsdaten zu Microsoft Azure](billing-understand-your-usage.md). 

Einzelheiten zu Kostenverwaltungsberichten finden Sie unter [Verwaltung der Kosten für das Azure-Portal](https://docs.microsoft.com/en-us/azure/billing/billing-getting-started).

## <a name="charges"></a>Wie stelle ich sicher, dass die in meiner Rechnung aufgeführten Gebühren richtig sind?
<div style="padding-top: 56.25%; position: relative; width: 100%;">
<iframe style="position: absolute;top: 0;left: 0;right: 0;bottom: 0;" width="100%" height="100%" src="https://www.youtube.com/embed/3YegFD769Pk" frameborder="0" allowfullscreen></iframe>
</div>

Wenn Sie zu einer Gebühr in Ihrer Rechnung weitere Informationen erhalten möchten, haben Sie folgende Optionen:

### <a name="option-1-review-your-invoice-and-compare-the-usage-and-costs-with-the-detailed-usage-csv-file"></a>Option 1: Überprüfen der Rechnung und Vergleichen der Nutzung und der Kosten mithilfe der CSV-Datei mit ausführlichen täglichen Nutzungsdaten

Die CSV-Datei mit ausführlichen Nutzungsdaten enthält Ihre Gebühren nach Abrechnungszeitraum und Daten zur täglichen Nutzung. Informationen zum Abrufen der CSV-Datei mit ausführlichen Nutzungsdaten finden Sie unter [Herunterladen oder Anzeigen Ihrer Azure-Rechnungen und täglichen Nutzungsdaten](https://docs.microsoft.com/en-us/azure/billing/billing-download-azure-invoice-daily-usage-date).

Ihre Nutzungsgebühren werden auf der Ebene der Verbrauchseinheiten angezeigt. Die Bedeutung der folgenden Benennungen ist in der Rechnung und in der Datei mit ausführlichen Nutzungsdaten identisch. Beispielsweise entspricht der „Abrechnungszyklus“ in der Rechnung dem „Abrechnungszeitraum“ in der Datei mit ausführlichen Nutzungsdaten.

 | Rechnung (PDF) | Detaillierte Nutzungsdaten (CSV)|
 | --- | --- |
|Billing Cycle | Billing Period |
 |Name |Meter Category |
 |Typ |Meter Subcategory |
 |Ressource |Meter Name |
 |Region |Meter Region |
 |Consumed |Consumed Quantity |
 |Included |Included Quantity |
 |Billable |Overage Quantity |

Der Abschnitt zu den **Nutzungsgebühren** Ihrer Rechnung enthält den Gesamtwert aller Verbrauchseinheiten, die während des Abrechnungszeitraums genutzt wurden. Im folgenden Screenshot wird beispielsweise eine Nutzungsgebühr für den Azure Scheduler-Dienst gezeigt.

![Nutzungsgebühren (Rechnung)](./media/billing-understand-your-bill/1.png)

Dieselbe Gebühr wird im Abschnitt **Statement** (Auszug) der CSV-Datei mit ausführlichen Nutzungsdaten angezeigt. Sowohl der verbrauchte Betrag (*Verbraucht*) als auch der *Wert* stimmt mit der Rechnung überein.

![Nutzungsgebühren (CSV-Datei)](./media/billing-understand-your-bill/2.png)

Eine Aufstellung dieser Gebühren pro Tag finden Sie in der CSV-Datei im Abschnitt **Tägliche Nutzung**. Wenn Sie unter *Kategorie für Messung* nach „Scheduler“ filtern, wird angezeigt, an welchen Tagen die Messung durchgeführt wurde und wie hoch der Verbrauch war. Zu Vergleichszwecken sind auch Informationen zu *Ressource* und *Ressourcengruppe* aufgeführt. Die Werte unter *Verbraucht* sollten zusammen die in der Rechnung enthaltene Summe ergeben.

![Abschnitt zur täglichen Nutzung in der CSV-Datei](./media/billing-understand-your-bill/3.png)

Sie erhalten die Kosten pro Tag, indem Sie die Beträge von *Verbraucht* mit dem Wert *Rate* aus dem Abschnitt **Statement** (Auszug) multiplizieren.

Weitere Informationen zur Rechnung finden Sie unter [Grundlegendes über Ihre Azure-Rechnung](billing-understand-your-invoice.md).

Weitere Informationen zu den einzelnen Spalten in der CSV-Datei finden Sie unter [Grundlegendes über die detaillierte Nutzung in Azure](billing-understand-your-invoice.md).

### <a name="option-2-review-your-invoice-and-compare-with-the-usage-and-costs-in-the-azure-portal"></a>Option 2: Überprüfen der Rechnung und Vergleichen der Nutzung und Kosten im Azure-Portal

Mithilfe des Azure-Portals können Sie auch Ihre Gebühren überprüfen. Das Azure-Portal enthält Kostenverwaltungsdiagramme, um Ihnen einen schnellen Überblick über Ihre Nutzung und die Gebühren in Ihrer Rechnung zu verschaffen.

Gehen Sie wie folgt vor, wenn Sie mit dem obigen Beispiel fortfahren möchten: Navigieren Sie zur [Seite „Abonnements“](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade), und wählen Sie Ihr Abonnement und dann **Kostenanalyse** aus. Sie können nun die Zeitspanne angeben und die Nutzungsgebühren für den Azure Scheduler-Dienst anzeigen.

![Kostenanalyseansicht im Azure-Portal](./media/billing-understand-your-bill/4.png)

Klicken Sie auf die entsprechende Zeile, um unter **Kostenverlauf** die Aufstellung der täglichen Kosten anzuzeigen.

![Ansicht „Kostenverlauf“ im Azure-Portal](./media/billing-understand-your-bill/5.png)

Weitere Informationen finden Sie unter [Vermeiden unerwarteter Kosten bei der Azure-Abrechnung und -Kostenverwaltung](billing-getting-started.md#costs).

## <a name="external"></a>Wie verhält es sich mit Gebühren für externe Dienste?
Externe Dienste (auch als „Azure Marketplace-Bestellungen“ bezeichnet) werden von unabhängigen Dienstanbietern bereitgestellt und separat in Rechnung gestellt. Die Gebühren werden nicht in Ihrer Azure-Rechnung aufgeführt. Weitere Informationen finden Sie unter [Grundlegendes zu Azure-Gebühren für externe Dienste](billing-understand-your-azure-marketplace-charges.md).

## <a name="payment"></a>Wie tätige ich eine Zahlung?

Wenn Sie eine Kreditkarte oder Debitkarte als Zahlungsmethode eingerichtet haben, wird die Zahlung zehn Tage nach Abschluss des Abrechnungszeitraums automatisch berechnet. Auf Ihrem Kreditkartenbeleg lautet die Postenzeile dann **MSFT Azure**.

Wenn Sie [auf Rechnung zahlen](billing-how-to-pay-by-invoice.md), senden Sie Ihre Zahlung an den Empfänger, der unten auf der Rechnung angegeben ist. Sie können den [Support kontaktieren](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), falls Sie weitere Unterstützung benötigen.

## <a name="how-do-i-check-the-status-of-a-payment-made-by-credit-card"></a>Wie überprüfe ich den Status einer per Kreditkarte erfolgten Zahlung?

[Erstellen Sie ein Supportticket](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), um den Status Ihrer Zahlung zu erfragen. 

## <a name="tips-for-cost-management"></a>Tipps für das Kostenmanagement
- Schätzen Sie die Kosten, indem Sie den [Preisrechner](https://azure.microsoft.com/pricing/calculator/) und den [Gesamtkosten-Rechner](https://aka.ms/azure-tco-calculator) nutzen, und rufen Sie die [ausführlichen Preisinformationen zu den einzelnen Diensten](https://azure.microsoft.com/en-us/pricing/) ab.
- [Richten Sie Abrechnungswarnungen ein](billing-set-up-alerts.md).
- [Überprüfen Sie Nutzung und Kosten regelmäßig im Azure-Portal](billing-getting-started.md#costs).

## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Wenden Sie sich an den Support.

[Wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), falls Sie weitere Hilfe benötigen, um das Problem schnell beheben zu lassen.

