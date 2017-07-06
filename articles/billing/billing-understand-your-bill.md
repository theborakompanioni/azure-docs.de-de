---
title: "Informationen zu Ihrer Rechnung für Azure | Microsoft-Dokumentation"
description: Erfahren Sie, wie Sie die Nutzung und Abrechnung Ihres Azure-Abonnements anzeigen und verstehen.
services: 
documentationcenter: 
author: genlin
manager: tonguyen
editor: 
tags: billing
ms.assetid: 32eea268-161c-4b93-8774-bc435d78a8c9
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2017
ms.author: erihur;genli
ms.translationtype: Human Translation
ms.sourcegitcommit: 9568210d4df6cfcf5b89ba8154a11ad9322fa9cc
ms.openlocfilehash: b5268239fb4c50e6bdea77323267a42e267268db
ms.contentlocale: de-de
ms.lasthandoff: 05/15/2017


---
# <a name="understand-your-bill-for-microsoft-azure"></a>Informationen zu Ihrer Rechnung für Microsoft Azure
Um Ihre Azure-Rechnung zu verstehen, schauen Sie sich die Rechnung mit der Zusammenfassung der Gebühren und die separate Datei mit den detaillierten Informationen zur täglichen Nutzung an. In diesem Artikel werden die meisten der in Rechnung und täglichen Nutzungsdaten verwendeten Begriffe beschrieben. Informationen zum Abrufen dieser Dateien finden Sie unter [Abrufen von Azure-Rechnungen und täglichen Nutzungsdaten](billing-download-azure-invoice-daily-usage-date.md). Wenn Sie ein kostenloses Testabonnement verwenden, können Sie Ihre täglichen Nutzungsinformationen abrufen, aber es ist keine Rechnung verfügbar.

Die Gebühren für Microsoft Azure-Abonnements variieren je nach Preisliste. Einige Tarife, z.B. Visual Studio Enterprise (MPN), umfassen monatliche Guthaben für Abonnenten, die Sie Ihren Anforderungen entsprechend für jeden Azure-Dienst verwenden können.

In der aktuellen Rechnung können bis zu 24 Stunden der Nutzung am Ende des vorherigen Abrechnungszeitraums angezeigt werden. Bei Abrechnungen für internationale Kunden sind die aufgeführten Gebühren zudem nur Schätzwerte, da Banken unterschiedliche Umrechnungsgebühren erheben.

## <a name="pdf"></a>Informationen zu Ihrer Rechnung (PDF)
Die Rechnung stellt eine Zusammenfassung der Gebühren bereit. Die Rechnung steht im [Azure-Portal](https://portal.azure.com) im PDF-Format (Portable Document Format) zum Download bereit. Weitere Informationen finden Sie unter [Abrufen von Azure-Rechnungen und täglichen Nutzungsdaten](billing-download-azure-invoice-daily-usage-date.md). 

In den folgenden Abschnitten werden die meisten Begriffe, die Sie auf Ihrer Rechnung finden, sowie die Beschreibungen für jeden Begriff aufgeführt.

### <a name="account-information"></a>Kontoinformationen
Der Abschnitt mit den Kontoinformationen enthält Angaben zu Ihrer Nutzung und Ihrem Profil.

![Header](./media/billing-understand-your-bill/Header.png)

| Begriff | Beschreibung |
| --- | --- |
| Invoice No. |Die eindeutige Rechnungsnummer für die Nachverfolgung |
| Billing Cycle |Datumsbereich, den diese Rechnung abdeckt |
| Invoice Date |Das Datum, an dem die Rechnung generiert wurde. |
| Payment Method |Die Zahlungsart, die für das Konto verwendet wird (Rechnung oder Kreditkarte). Weitere Informationen zum Aktualisieren einer Kreditkarte finden Sie unter [Ändern der für die Zahlung eines Azure-Abonnements verwendeten Kreditkarte](billing-how-to-change-credit-card.md). Wenn Sie Mitarbeiter eines Unternehmens sind, können Sie die Rechnungen für das Azure-Abonnement auch per Scheck, Express-Scheck und Überweisung begleichen. Informationen finden Sie unter [Azure-Abrechnung – Bezahlen per Rechnung](https://azure.microsoft.com/pricing/invoicing/). |
| Bill to |Anschrift für Microsoft Azure-Rechnungen. Weitere Informationen zum Aktualisieren der Abrechnungsinformationen finden Sie unter [Ändern der für die Zahlung eines Azure-Abonnements verwendeten Kreditkarte](billing-how-to-change-credit-card.md). |
| Abonnementangebot |Die Art des erworbenen Abonnementangebots (nutzungsbasierte Bezahlung, BizSpark Plus, Azure Pass usw.). Wie Sie von einem nutzungsbasierten Tarif zu einem anderen Abonnementangebot wechseln können, erfahren Sie unter [Umstellen Ihres Azure-Abonnements auf ein anderes Angebot](billing-how-to-switch-azure-offer.md) . |
| Account Owner Email |Die E-Mail-Adresse, unter der das Microsoft Azure-Konto registriert ist. Weitere Informationen zum Ändern der E-Mail-Adresse finden Sie unter [Ändern von Profilinformationen für Ihr Azure-Konto, beispielsweise Kontakt-E-Mail-Adresse, Adresse und Telefonnummer](billing-how-to-change-azure-account-profile.md). |

### <a name="understand-the-invoice-summary"></a>Grundlegendes zur Rechnungsübersicht
Die **Rechnungsübersicht** fasst die Transaktionen seit der letzten Rechnung und die aktuellen Nutzungsgebühren zusammen.

![Rechnungsübersicht](./media/billing-understand-your-bill/InvoiceSummary.png)

In den Abschnitten „Previous Balances“, „Payments“ und „Outstanding Balance“ der Rechnung werden die Transaktionen seit Ihrer letzten Rechnung zusammengefasst.

| Begriff | Beschreibung |
| --- | --- |
| Previous Balance |Der fällige Gesamtbetrag Ihrer letzten Rechnung. |
| Payments |Sämtliche mit Ihrer letzten Rechnung verrechneten Zahlungen. |
| Outstanding balance (from previous billing cycle) |Alle Beträge (Gutschriften oder Restbeträge), die seit Ihrer letzten Rechnung mit Ihrem Konto verrechnet wurden. |

### <a name="understand-the-current-charges"></a>Informationen zu aktuellen Gebühren
Der Abschnitt „Current Charges“ der Rechnung enthält Details zu Ihren monatlichen Gebühren. 

| Begriff | Beschreibung |
| --- | --- |
| Usage Charges |Dies sind die monatlichen Gesamtgebühren für ein Abonnement. Der Verbrauch für den vergangenen Monat wird Ihnen in Rechnung gestellt. |
| Discounts |Auf Ihre aktuelle Rechnung angewendete Dienstrabatte. |
| Adjustments |Verschiedene Gutschriften oder ausstehende Gebühren für Ihre aktuelle Rechnung. Wenn Sie beispielsweise den Tarif „Visual Studio Enterprise mit MSDN“ nutzen, wird hier ein monatliches Guthaben angezeigt. Wenn Sie Ihr Abonnement kündigen, werden hier alle monatlichen Nutzungsgebühren angezeigt, die die monatliche Gutschrift überschreiten, die Sie mit Ihrem Abonnementangebot erhalten. Die Gebühren beziehen sich auf den Zeitraum vom Anfang des aktuellen Abrechnungszeitraums bis zum Kündigungsdatum des Abonnements. |


### <a name="sold-to-and-payment-instructions"></a>Käufer und Zahlungsanweisungen
In der folgenden Tabelle werden Angaben zu Käufer und Zahlungsanweisungen auf Ihrer Rechnung beschrieben.

| Begriff | Beschreibung |
| --- | --- |
| Sold To | Die Profiladresse im Konto. Weitere Informationen zum Ändern dieser Adresse finden Sie unter [Ändern von Profilinformationen für Ihr Azure-Konto, beispielsweise Kontakt-E-Mail-Adresse, Adresse und Telefonnummer](billing-how-to-change-azure-account-profile.md). |
| Payment Instructions | Wenn Sie per Rechnung bezahlen, erfahren Sie in diesen Anweisungen, wie Sie per Scheck, Überweisung oder Scheckzahlung über Nacht zahlen. Weitere Informationen finden Sie unter [Azure-Abrechnung – Bezahlen per Rechnung](https://azure.microsoft.com/pricing/invoicing/). |

## <a name="csv"></a> Erläuterungen zu den detaillierten Nutzungsgebühren (CSV)
Die Nutzungsdatei zeigt, welche Menge der einzelnen Ressourcen innerhalb des aktuellen Abrechnungszeitraums verwendet wurde. Sie ist als Datei mit kommagetrennten Werten (CSV) verfügbar, die Sie in einem Tabellenkalkulationsprogramm öffnen können. Wenn zwei Versionen verfügbar sind, laden Sie Version 2 herunter. Das ist das aktuellste Dateiformat. Weitere Informationen finden Sie unter [Abrufen von Azure-Rechnungen und täglichen Nutzungsdaten](billing-download-azure-invoice-daily-usage-date.md).

Dies sind die **monatlichen** Gesamtgebühren für ein Abonnement nach Abzug von Gutschriften oder Rabatten. Der Verbrauch für den vergangenen Monat wird Ihnen in Rechnung gestellt.  

In den folgenden Abschnitten werden die meisten der in Version 2 der Datei mit den detaillierten Informationen zur Nutzung verwendeten Begriffe beschrieben.

### <a name="statement"></a>Anweisung 
Im obersten Abschnitt der Datei werden die während des Abrechnungszeitraums des vorherigen Monats verwendeten Dienste angezeigt. Die folgende Tabelle enthält die in diesem Abschnitt aufgeführten Begriffe und Beschreibungen.

| Begriff | Beschreibung |
| --- | --- |
|Billing Period |Der Abrechnungszeitraum, in dem die Ressource verbraucht bzw. der Dienst genutzt wurde. |
|Meter Category |Gibt den Dienst an, zu dem dieser Verbrauch gehört. |
|Meter Sub-Category |Definiert den Typ des Azure-Diensts und kann sich auf den Tarif auswirken. |
|Meter Name |Gibt die Maßeinheit für die Ressource an, für die der Verbrauch gemessen wird. |
|Meter Region |Gibt den Standort des Rechenzentrums für bestimmte Dienste an, die basierend auf dem Standort des Rechenzentrums berechnet werden. |
|SKU |Gibt den eindeutigen Systembezeichner für jede Azure-Ressource an. |
|Unit |Gibt die Einheit an, in der der Dienst in Rechnung gestellt wird. Beispiele sind GB, Stunden oder 10.000 s. |
|Consumed Quantity |Die Menge der Ressource, die während des Abrechnungszeitraums verbraucht wurde. |
|Included Quantity |Die Menge der Ressource, die kostenlos in Ihrem Abrechnungszeitraum enthalten ist. |
|Overage Quantity |Zeigt den Unterschied zwischen der verbrauchten und der enthaltenen Menge an. Dieser Betrag wird Ihnen in Rechnung gestellt. Für nutzungsbasierte Tarife ohne im Tarif enthaltene Menge entspricht der Wert hier dem Wert unter „Consumed Quantity“. |
|Within Commitment |Zeigt die Gebühren für Ressourcen an, die von Ihrem Verpflichtungsbetrag im Rahmen Ihres 6- oder 12-monatigen Angebots abgezogen werden. Beachten Sie, dass die Gebühren für Ressourcen in chronologischer Reihenfolge von Ihrem Verpflichtungsbetrag abgezogen werden. |
|Currency |Die für den aktuellen Abrechnungszeitraum verwendete Währung. |
|Überschreitung |Zeigt die Gebühren für Ressourcen an, die Ihren Verpflichtungsbetrag im Rahmen Ihres 6- oder 12-monatigen Angebots überschreiten. |
|Commitment Rate |Zeigt den Verpflichtungssatz basierend auf dem Gesamtverpflichtungsbetrag im Rahmen Ihres 6- oder 12-monatigen Angebots an. |
|Rate |Der Satz, der Ihnen pro abrechenbarer Einheit in Rechnung gestellt wird. |
|Wert |Zeigt das Ergebnis der Multiplikation der Spalte „Overage Quantity“ mit der Spalte „Rate“ an. Wenn die verbrauchte Menge die enthaltene Menge nicht überschreitet, fallen in dieser Spalte keine Gebühren an. |

### <a name="daily-usage"></a>Tägliche Nutzung 

Im Abschnitt „Daily Usage“ der Datei werden Nutzungsdetails angezeigt, die sich auf die Abrechnung auswirken. Die folgende Tabelle enthält die in diesem Abschnitt aufgeführten Begriffe und Beschreibungen. 

| Begriff| Beschreibung |
| --- | --- |
|Usage Date |Das Datum, an dem die Ressource verwendet wurde. |
|Meter Category |Gibt den Dienst an, zu dem dieser Verbrauch gehört. |
|Meter ID |Der Bezeichner der berechneten Messung, der für die Preisfeststellung für die Nutzung verwendet wird. |
|Meter Sub-Category |Definiert den Typ des Azure-Diensts, der sich auf den Tarif auswirken kann. |
|Meter Name |Gibt die Maßeinheit für die Ressource an, für die der Verbrauch gemessen wird. |
|Meter Region|Gibt den Standort des Rechenzentrums für bestimmte Dienste an, die basierend auf dem Standort des Rechenzentrums berechnet werden. |
|Unit |Gibt die Einheit an, in der der Dienst in Rechnung gestellt wird. Beispiele sind GB, Stunden oder 10.000 s. |
|Consumed Quantity |Die Menge der Ressource, die an diesem Tag verbraucht wurde. |
|Resource Location |Gibt das Rechenzentrum an, in dem die Ressource ausgeführt wird. |
|Consumed Service |Der verwendete Azure-Plattformdienst. |
|Ressourcengruppe |Die Ressourcengruppe, in der die bereitgestellte Ressource ausgeführt wird. Weitere Informationen finden Sie unter [Übersicht über den Azure Resource Manager](../azure-resource-manager/resource-group-overview.md). |
|Instance ID |Der Bezeichner für die Ressource. Der Bezeichner enthält den Namen, den Sie für die Ressource bei der Erstellung angegeben haben. Es ist entweder der Name der Ressource oder die vollqualifizierte Ressourcen-ID. Weitere Informationen finden Sie unter [Azure Resource Manager-API](/rest/api/resources/resources). |
|Tags |Tag, den Sie der Ressource zuweisen. Verwenden Sie Tags zum Gruppieren von Abrechnungsdatensätzen. Beispielsweise können Sie Tags verwenden, um Kosten nach den Abteilungen zu unterteilen, die die Ressource nutzen. Dienste, die das Ausgeben von Tags unterstützen, sind virtuelle Computer, Speicher und Netzwerkdienste, die mit der [Azure Resource Manager-API](/rest/api/resources/resources) bereitgestellt werden. Weitere Informationen finden Sie unter [Organisieren von Azure-Ressourcen mit Tags](http://azure.microsoft.com/updates/organize-your-azure-resources-with-tags/). |
|Additional Info |Dienstspezifische Metadaten. Dies kann beispielsweise ein Imagetyp für einen virtuellen Computer sein. |
|Service Info 1 |Der Name des Projekts, zu dem der Dienst in Ihrem Abonnement gehört. |
|Service Info 2 |Ein Legacyfeld, in dem optionale dienstspezifische Metadaten erfasst werden. |

## <a name="tips-for-cost-management"></a>Tipps für das Kostenmanagement
- Schätzen Sie Kosten mit dem [Preisrechner](https://azure.microsoft.com/pricing/calculator/), dem [Gesamtbetriebskosten-Rechner](https://aka.ms/azure-tco-calculator) und wenn Sie einen Dienst hinzufügen.
- [Einrichten von Abrechnungswarnungen](billing-set-up-alerts.md)
- [Regelmäßiges Überprüfen Ihrer Nutzung und Kosten im Azure-Portal](billing-getting-started.md#costs)

Weitere Informationen finden Sie unter [Erste Schritte mit der Abrechnungs- und Kostenverwaltung in Azure](billing-getting-started.md).

## <a name="how-do-i-make-a-payment"></a>Wie kann ich bezahlen?
Wenn Sie eine Kredit- oder Debitkarte als Zahlungsmethode festlegen, erfolgt die Zahlung automatisch. Auf Ihrem Kreditkartenbeleg lautet die Postenzeile dann **MSFT Azure**.

Wenn Sie [auf Rechnung zahlen](https://azure.microsoft.com/pricing/invoicing/), senden Sie Ihre Zahlung an den Empfänger, der unten auf der Rechnung angegeben ist. Sie können den [Support kontaktieren](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), falls Sie weitere Unterstützung benötigen.

## <a name="how-do-i-check-the-status-of-a-payment-made-by-credit-card"></a>Wie überprüfe ich den Status einer per Kreditkarte erfolgten Zahlung?
[Erstellen Sie ein Supportticket](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), um den Status Ihrer Zahlung zu erfragen. 

## <a name="what-about-marketplace-orders-or-external-service-charges"></a>Was ist mit Marketplace-Bestellungen oder Gebühren für externe Dienste?
Externe Dienste wurden in der Vergangenheit als Marketplace-Bestellungen bezeichnet. Externe Dienste werden von unabhängigen Dienstanbietern angeboten, sind jedoch in Azure integriert. Weitere Informationen finden Sie unter [Grundlegendes zu Azure-Gebühren für externe Dienste](billing-understand-your-azure-marketplace-charges.md).

## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Wenden Sie sich an den Support. 
[Wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), falls Sie weitere Hilfe benötigen, um das Problem schnell beheben zu lassen.
 




