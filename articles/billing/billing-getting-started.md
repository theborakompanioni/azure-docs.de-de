---
title: "Vermeiden unerwarteter Kosten, Verwalten der Abrechnung – Azure | Microsoft-Dokumentation"
description: "Hier erwarten Sie, wie Sie unerwartete Gebühren in Ihrer Azure-Abrechnung vermeiden. Verwenden Sie Kostenüberwachungs- und Kostenverwaltungsfunktionen für ein Microsoft Azure-Abonnement."
services: 
documentationcenter: 
author: jlian
manager: tonguyen
editor: 
tags: billing
ms.assetid: 482191ac-147e-4eb6-9655-c40c13846672
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/20/2017
ms.author: jlian
ms.translationtype: Human Translation
ms.sourcegitcommit: 1cc1ee946d8eb2214fd05701b495bbce6d471a49
ms.openlocfilehash: 539a3e628dfec1088f683652bc9b743b87875d36
ms.contentlocale: de-de
ms.lasthandoff: 04/26/2017


---
# <a name="prevent-unexpected-costs-with-azure-billing-and-cost-management"></a>Vermeiden unerwarteter Kosten bei der Azure-Abrechnung und -Kostenverwaltung

Wenn Sie sich für Azure registrieren, sollten Sie sich einen Überblick über Ihre Ausgaben verschaffen. Im [Azure-Portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) finden Sie nach Auswahl Ihres Abonnements Ihre aktuelle Kostenaufschlüsselung und Verbrauchsrate. Außerdem können Sie [ältere Rechnungen sowie Dateien mit detaillierten Nutzungsinformationen herunterladen](billing-download-azure-invoice-daily-usage-date.md). Wenn Sie Kosten für Ressourcen zusammenfassen möchten, die für unterschiedliche Projekte oder Teams verwendet werden, beschäftigen Sie sich mit [Tags für Ressourcen](../azure-resource-manager/resource-group-using-tags.md). Falls Sie das Berichtssystem Ihrer Organisation verwenden möchten, informieren Sie sich über die [Abrechnung-APIs](billing-usage-rate-card-overview.md). 

Weitere Informationen über Ihre Daten zur täglichen Nutzung finden Sie unter [Erläuterungen zur Rechnung für Microsoft Azure](billing-understand-your-bill.md).

Viele Features in diesem Artikel sind für Abonnements von EA-Kunden (Enterprise Agreement), CSP-Kunden (Cloud Solution Provider, Cloudlösungsanbieter) und Azure Sponsorship-Kunden nicht relevant. Wir stellen dafür einen anderen Satz von Tools bereit, die Sie für die Kostenverwaltung verwenden können. Weitere Informationen finden Sie unter [Zusätzliche Ressourcen für EA, CSP und Sponsorship](#other-offers).

Wenn Ihr Abonnement eine kostenlose Testversion ist oder für [Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), Azure in Open (AIO) oder BizSpark gilt, sollten Sie sich über [Ausgabenlimits](#spending-limit) informieren, um zu vermeiden, dass Ihr Abonnement unerwartet deaktiviert wird. 

## <a name="day-0-before-you-add-azure-services"></a>Tag 0: Vor dem Hinzufügen von Azure-Diensten

### <a name="estimate-cost-online-using-the-pricing-calculator"></a>Schätzen der Kosten mithilfe des Onlinepreisrechners

Verschaffen Sie sich mit dem [Preisrechner](https://azure.microsoft.com/pricing/calculator/) und dem [Gesamtbetriebskosten-Rechner](https://aka.ms/azure-tco-calculator) einen ungefähren Eindruck von den monatlichen Kosten des Diensts, für den Sie sich interessieren. So fallen beispielsweise für die Computestunden eines virtuellen A1-Windows-Computers voraussichtlich 66,96 USD/Monat an, wenn der Computer durchgehend in Betrieb ist:

![Screenshot des Preisrechners, der zeigt, dass für einen virtuellen A1-Windows-Computer pro Monat voraussichtlich Kosten in Höhe von 66,96 USD anfallen](./media/billing-getting-started/pricing-calc.PNG)

Weitere Informationen finden Sie in den [häufig gestellten Fragen zu Preisen](https://azure.microsoft.com/pricing/faq/). Wenn Sie stattdessen lieber mit einer Person sprechen möchten, können Sie sich unter 1 800 867 1389 auch telefonisch informieren.

### <a name="check-your-subscription-and-access"></a>Überprüfen von Abonnement und Zugriff

Zum Anzeigen der Kosten benötigen Sie [Zugriff auf Abrechnungsinformationen auf Abonnementebene](billing-manage-access.md), aber nur der Kontoadministrator kann auf das [Kontocenter](https://account.windowsazure.com/Home/Index) zugreifen, Abrechnungsinformationen ändern und Abonnements verwalten. Der Kontoadministrator ist die Person, die den Registrierungsprozess durchlaufen hat. Weitere Informationen finden Sie unter [Hinzufügen oder Ändern von Azure-Administratorrollen, die das Abonnement oder die Dienste verwalten](billing-add-change-azure-subscription-administrator.md).

Um zu ermitteln, ob Sie der Kontoadministrator sind, [navigieren Sie im Azure-Portal zum Blatt „Abonnements“](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade), und sehen Sie sich die Liste mit den Abonnements an, auf die Sie Zugriff haben. Ausschlaggebend ist die Angabe unter **Meine Rolle**. Wenn hier *Kontoadministrator* angegeben ist, ist alles in Ordnung. Bei einer Angabe wie *Besitzer* verfügen Sie nicht über sämtliche Berechtigungen.

![Screenshot Ihrer Rolle in der Ansicht „Abonnements“ im Azure-Portal](./media/billing-getting-started/sub-blade-view.PNG)

Falls Sie nicht der Kontoadministrator sind, wurde Ihnen wahrscheinlich von einer anderen Person über die [rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) in Azure Active Directory](../active-directory/role-based-access-control-configure.md) eingeschränkter Zugriff gewährt. Um Abonnements verwalten und Abrechnungsinformationen ändern zu können, müssen Sie [den Kontoadministrator ermitteln](billing-subscription-transfer.md#whoisaa) und ihn bitten, die Aufgaben für Sie auszuführen oder das [Abonnement auf Sie zu übertragen](billing-subscription-transfer.md).

Falls der Kontoadministrator Ihre Organisation verlassen hat und Sie die Abrechnung verwalten müssen, wenden Sie sich an den [Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). 

### <a name="spending-limit"></a>Überprüfen, ob ein Ausgabenlimit aktiviert ist

Bei Verwendung eines guthabenbasierten Abonnements wird das Ausgabenlimit standardmäßig für Sie aktiviert. Dadurch wird Ihre Kreditkarte nicht belastet, nachdem Sie Ihr gesamtes Guthaben verbraucht haben. Eine vollständige Liste mit Azure-Angeboten sowie Informationen zur Verfügbarkeit des Ausgabenlimits finden Sie [hier](https://azure.microsoft.com/support/legal/offer-details/).

Bei Erreichen Ihres Ausgabenlimits werden Ihre Dienste allerdings deaktiviert. Das bedeutet, dass die Zuordnung Ihrer virtuellen Computer aufgehoben wird. Um Ausfallzeiten zu vermeiden, müssen Sie das Ausgabenlimit deaktivieren. Überschreitungen werden über die hinterlegte Kreditkarte abgerechnet. 

Um zu ermitteln, ob ein Ausgabenlimit aktiviert ist, [navigieren Sie im Kontocenter zur Ansicht „Abonnements“](https://account.windowsazure.com/Subscriptions). Bei aktiviertem Ausgabenlimit wird ein Banner angezeigt:

![Screenshot mit einer Warnung aufgrund eines aktivierten Ausgabenlimits im Kontocenter](./media/billing-getting-started/spending-limit-banner.PNG)

Klicken Sie auf das Banner, und folgen Sie den Anweisungen, um das Ausgabenlimit zu entfernen. Falls Sie bei der Registrierung keine Kreditkarteninformationen eingegeben haben, müssen Sie dies nun nachholen, um das Ausgabenlimit entfernen zu können. Weitere Informationen finden Sie unter [Azure-Ausgabenlimit – Funktionsweise und Aktivierung/Deaktivierung](https://azure.microsoft.com/pricing/spending-limits/).

### <a name="set-up-billing-alerts"></a>Einrichten von Abrechnungswarnungen

Richten Sie Abrechnungswarnungen ein, um sich per E-Mail benachrichtigen zu lassen, wenn Ihre Nutzungskosten einen bestimmten Betrag übersteigen. Falls Sie über monatliches Guthaben verfügen, richten Sie Warnungen für den Verbrauch eines bestimmten Betrags ein. Weitere Informationen finden Sie unter [Einrichten von Abrechnungswarnungen für Microsoft Azure-Abonnements](billing-set-up-alerts.md).

![Screenshot einer E-Mail mit einer Abrechnungswarnung](./media/billing-getting-started/billing-alert.png)

> [!NOTE]
> Da sich dieses Feature noch in der Vorschauphase befindet, sollten Sie Ihre Nutzung in regelmäßigen Abständen überprüfen.

Bei Ihrer ersten Warnung können Sie sich an der Kostenschätzung des Preisrechners orientieren.

### <a name="understand-limits-and-quotas-for-your-subscription"></a>Grundlegendes zu Grenzwerten und Kontingenten für Ihr Abonnement

Bei jedem Abonnement gelten standardmäßig bestimmte Einschränkungen für Aspekte wie die Anzahl von CPU-Kernen und die IP-Adresse. Berücksichtigen Sie diese Grenzwerte. Weitere Informationen finden Sie unter [Grenzwerte für Azure-Abonnements, -Dienste und -Kontingente sowie allgemeine Beschränkungen](../azure-subscription-service-limits.md). Falls Sie einen höheren Grenzwert oder ein höheres Kontingent benötigen, [wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

## <a name="day-1-as-you-add-services"></a>Tag 1: Hinzufügen von Diensten

### <a name="review-the-estimated-cost-in-the-portal"></a>Überprüfen der voraussichtlichen Kosten im Portal

Wenn Sie im Azure-Portal einen Dienst hinzufügen, erscheint üblicherweise eine ähnliche Schätzung der monatlichen Kosten. Wenn Sie also etwa die Größe Ihres virtuellen Windows-Computers auswählen, werden die voraussichtlichen monatlichen Kosten für die Computestunden angezeigt:

![Beispiel: Die monatlichen Kosten für einen virtuellen A1-Windows-Computer belaufen sich voraussichtlich auf 66,96 USD.](./media/billing-getting-started/vm-size-cost.PNG)

### <a name="tags"></a> Gruppieren von Abrechnungsdaten durch Hinzufügen von Tags zu Ressourcen

Mithilfe von Tags können Sie Abrechnungsdaten für unterstützte Dienste gruppieren. Wenn Sie also beispielsweise mehrere virtuelle Computer für unterschiedliche Teams verwenden, können Sie die Kosten mithilfe von Tags nach Kostenstelle (Personal, Marketing, Finanzen) oder Umgebung (Produktion, Präproduktion, Test) kategorisieren. 

![Screenshot zur Einrichtung von Tags im Portal](./media/billing-getting-started/tags.PNG)

Die Tags werden in den unterschiedlichen Kostenberichtsansichten angezeigt. So sind sie beispielsweise sofort in der [Kostenanalyseansicht](#costs) und nach dem ersten Abrechnungszeitraum in der [CSV-Datei mit den Nutzungsdetails](#invoice-and-usage) zu sehen.

Weitere Informationen finden Sie unter [Verwenden von Tags zum Organisieren von Azure-Ressourcen](../azure-resource-manager/resource-group-using-tags.md).

### <a name="consider-enabling-cost-cutting-features-like-auto-shutdown-for-vms"></a>Aktivieren von Kostensenkungsfeatures wie dem automatischen Herunterfahren für virtuelle Computer

Abhängig von Ihrem Szenario können Sie im Azure-Portal ggf. das automatische Herunterfahren für Ihre virtuellen Computer konfigurieren. Weitere Informationen finden Sie unter [Announcing auto-shutdown for VMs using Azure Resource Manager](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/) (Ankündigung des automatischen Herunterfahrens für virtuelle Computer mit Azure Resource Manager).

![Screenshot der Option für automatisches Herunterfahren im Portal](./media/billing-getting-started/auto-shutdown.PNG)

Das automatische Herunterfahren ist nicht dasselbe wie das Herunterfahren des virtuellen Computers über dessen Energieoptionen. Beim automatischen Herunterfahren werden Ihre virtuellen Computer beendet, und ihre Zuordnung wird aufgehoben, um zu verhindern, dass weitere Nutzungsgebühren anfallen. Weitere Informationen finden Sie in den häufig gestellten Fragen zu Preisen für [virtuelle Linux-Computer](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) und [virtuelle Windows-Computer](https://azure.microsoft.com/pricing/details/virtual-machines/windows/).

Informationen zu weiteren Kostensenkungsfeatures für Ihre Entwicklungs- und Testumgebungen finden Sie unter [Azure DevTest Labs](https://azure.microsoft.com/services/devtest-lab/).

## <a name="cost-reporting"></a> Ab Tag 2: Anzeigen der Nutzung nach der Verwendung von Diensten

### <a name="costs"></a> Regelmäßiges Überprüfen der Kostenaufschlüsselung und Verbrauchsrate im Portal

Überprüfen Sie regelmäßig die Kosten, die für Ihre aktiven Dienste anfallen. Die aktuellen Ausgaben sowie die Verbrauchsrate werden im Azure-Portal angezeigt. 

1. [Navigieren Sie im Azure-Portal zum Blatt „Abonnements“.](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)

2. Wählen Sie das gewünschte Abonnement aus. Möglicherweise ist nur ein einzelnes Abonnement verfügbar.

3. Die Kostenaufschlüsselung und die Verbrauchsrate werden auf dem Popupblatt angezeigt. Sollte es für Ihr Angebot nicht unterstützt werden, erscheint im oberen Bereich eine entsprechende Warnung. Warten Sie nach dem Hinzufügen eines Diensts 24 Stunden, damit die Daten aufgefüllt werden können.
    
    ![Screenshot der Verbrauchsrate und der Kostenaufschlüsselung im Azure-Portal](./media/billing-getting-started/burn-rate.PNG)

4. Bei Bedarf können Sie die Ansicht an Ihr Dashboard anheften.

    ![Screenshot zum Anheften einer Ansicht an das Dashboard](./media/billing-getting-started/pin.PNG)

5. Klicken Sie in der Liste auf der linken Seite auf **Kostenanalyse**, um die Kostenaufschlüsselung nach Ressource anzuzeigen.

    ![Screenshot der Kostenanalyseansicht im Azure-Portal](./media/billing-getting-started/cost-analysis.PNG)

6. Sie können nach verschiedenen Eigenschaften wie [Tags](#tags), Ressourcengruppe und Zeitraum filtern. Klicken Sie auf **Übernehmen**, um die Filter zu bestätigen, und auf **Herunterladen**, um die Ansicht in eine CSV-Datei zu exportieren.

7. Klicken Sie auf eine Ressource, um den Ausgabenverlauf und die Kosten pro Tag anzuzeigen.

    ![Screenshot der Ausgabenverlaufsansicht im Azure-Portal](./media/billing-getting-started/spend-history.PNG)

Es empfiehlt sich, die angezeigten Kosten mit den voraussichtlichen Kosten zu vergleichen, die beim Auswählen der Dienste angezeigt wurden. Sollten die Kosten stark von den Schätzungen abweichen, überprüfen Sie noch einmal den Tarif (etwa A1 im Vergleich zu A0), den Sie für Ihre Ressourcen ausgewählt haben. 

#### <a name="view-costs-for-all-your-subscriptions-in-the-billing-blade"></a>Anzeigen der Kosten für alle Abonnements auf dem Blatt „Abrechnung“

Falls Sie als Kontoadministrator mehrere Abonnements verwalten, werden auf dem Blatt [Abrechnung](https://portal.azure.com/#blade/Microsoft_Azure_Billing/BillingBlade) der aggregierte Rechnungsbetrag und die Aufschlüsselung für alle Ihre Abonnements angezeigt. 

<!-- Add screenshots of multiple subs each with billed usage -->

### <a name="turn-on-and-check-out-azure-advisor-recommendations"></a>Aktivieren und Überprüfen von Azure Advisor-Empfehlungen

[Azure Advisor](../advisor/advisor-overview.md) ist ein Vorschaufeature, das wenig genutzte Ressourcen identifiziert und Sie so bei der Senkung Ihrer Kosten unterstützt. Aktivieren Sie das Feature im Azure-Portal:

![Screenshot der Schaltfläche für Azure Advisor im Azure-Portal](./media/billing-getting-started/advisor-button.PNG)

Daraufhin stehen auf der Registerkarte **Kosten** des Advisor-Dashboards nützliche Empfehlungen zur Verfügung:

![Screenshot mit einem Beispiel für eine Advisor-Kostenempfehlung](./media/billing-getting-started/advisor-action.PNG)

Weitere Informationen finden Sie unter [Advisor-Empfehlungen zu Kosten](../advisor/advisor-cost-recommendations.md).

### <a name="invoice-and-usage"></a> Abrufen der Rechnung und der Nutzungsdetails nach dem ersten Abrechnungszeitraum

Nach dem ersten Abrechnungszeitraum können Sie Ihre Rechnung als PDF-Datei und die Nutzungsdetails als CSV-Datei herunterladen. Wahlweise kann Ihnen Ihre Rechnung auch per E-Mail gesendet werden. Diese Dateien geben Aufschluss darüber, welche Kosten letztendlich nach Steuern, Rabatten und Gutschriften abgerechnet werden. Falls Sie keine Zahlungsmethode mit Ihrem Abonnement verknüpft haben, stehen diese Dateien unter Umständen nicht zur Verfügung. Weitere Informationen finden Sie unter [Abrufen von Azure-Rechnungen und täglichen Nutzungsdaten](billing-download-azure-invoice-daily-usage-date.md) sowie unter [Informationen zu Ihrer Rechnung für Microsoft Azure](billing-understand-your-bill.md).

![Screenshot einer PDF-Rechnung](./media/billing-getting-started/invoice.png)

Die zuvor festgelegten Tags werden in den CSV-Dateien mit den Nutzungsdetails angezeigt:

![Screenshot mit Tags in der CSV-Datei zur Nutzung](./media/billing-getting-started/csv.png)

### <a name="billing-api"></a>Abrechnungs-API

Über unsere Abrechnungs-API können Sie Nutzungsdaten programmgesteuert abrufen. Verwenden Sie die RateCard-API und die Usage-API, um Ihre abgerechnete Nutzung abzurufen. Weitere Informationen finden Sie unter [Gewinnen von Einblicken in den Ressourcenverbrauch unter Microsoft Azure](billing-usage-rate-card-overview.md).

## <a name="other-offers"></a> Zusätzliche Ressourcen für EA, CSP und Sponsorship

Wenden Sie sich an Ihren Kundenbetreuer oder Azure-Partner.

| Angebot | Ressourcen |
|-------------------------------|-----------------------------------------------------------------------------------|
| Enterprise Agreement (EA) | [EA-Portal](https://ea.azure.com/), [Hilfedokumente](https://ea.azure.com/helpdocs) und [Power BI-Bericht](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-enterprise/) |
| Cloud-Lösungsanbieter (CSP) | Wenden Sie sich an Ihren Anbieter. |
| Azure Sponsorship | [Sponsorship-Portal](https://www.microsoftazuresponsorships.com/) |

Für IT-Manager großer Organisationen empfehlen wir den Artikel zum [Azure-Unternehmensgerüst](../azure-resource-manager/resource-manager-subscription-governance.md) und das [Whitepaper zu Enterprise IT](http://download.microsoft.com/download/F/F/F/FFF60E6C-DBA1-4214-BEFD-3130C340B138/Azure_Onboarding_Guide_for_IT_Organizations_EN_US.pdf) (PDF-Download, nur auf Englisch verfügbar).

## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Support kontaktieren

[Wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), falls Sie weitere Hilfe benötigen, um das Problem schnell beheben zu lassen.
