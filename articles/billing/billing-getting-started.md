---
title: "Vermeiden unerwarteter Kosten, Verwalten der Abrechnung – Azure | Microsoft-Dokumentation"
description: "Hier erwarten Sie, wie Sie unerwartete Gebühren in Ihrer Azure-Abrechnung vermeiden. Verwenden Sie Kostenüberwachungs- und Kostenverwaltungsfunktionen für ein Microsoft Azure-Abonnement."
services: 
documentationcenter: 
author: tonguyen10
manager: tonguyen
editor: 
tags: billing
ms.assetid: 482191ac-147e-4eb6-9655-c40c13846672
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/14/2017
ms.author: tonguyen
ms.translationtype: HT
ms.sourcegitcommit: 4f77c7a615aaf5f87c0b260321f45a4e7129f339
ms.openlocfilehash: d74f649a8b5f35ffe16479576959e5ebb2857bf1
ms.contentlocale: de-de
ms.lasthandoff: 09/22/2017

---
# <a name="prevent-unexpected-charges-with-azure-billing-and-cost-management"></a>Vermeiden unerwarteter Gebühren bei der Azure-Abrechnung und -Kostenverwaltung

Wenn Sie sich für Azure registrieren, sollten Sie sich einen Überblick über Ihre Ausgaben verschaffen. Der [Preisrechner](https://azure.microsoft.com/pricing/calculator/) kann eine Kostenschätzung bereitstellen, bevor Sie eine Azure-Ressource erstellen. Im [Azure-Portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) erhalten Sie den aktuellen Kostenstrukturplan und die Prognose für Ihr Abonnement. Wenn Sie Kosten für unterschiedliche Projekte oder Teams zusammenfassen und verstehen möchten, beschäftigen Sie sich mit [Tags für Ressourcen](../azure-resource-manager/resource-group-using-tags.md). Falls Sie das Berichtssystem Ihrer Organisation verwenden möchten, informieren Sie sich über die [Abrechnung-APIs](billing-usage-rate-card-overview.md). 

- Für Abonnements über Enterprise Agreement (EA) ist die öffentliche Vorschau zum Anzeigen Ihrer Kosten im Azure-Portal verfügbar. Einige der folgenden Features sind für Abonnements von CSP-Kunden (Cloud Solution Provider, Cloud-Lösungsanbieter) oder Azure Sponsorship-Kunden eventuell nicht relevant. Weitere Informationen finden Sie unter [Zusätzliche Ressourcen für EA, CSP und Sponsorship](#other-offers).

- Wenn Sie ein Abonnement für eine kostenlose Testversion, [Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), Azure in Open (AIO) oder BizSpark besitzen, wird Ihr Abonnement automatisch deaktiviert, sobald Ihr gesamtes Guthaben verbraucht ist. Erfahren Sie mehr über [Ausgabenlimits](#spending-limit), um zu vermeiden, dass Ihr Abonnement unerwartet deaktiviert wird.

## <a name="get-estimated-costs-before-adding-azure-services"></a>Abrufen der geschätzten Kosten vor dem Hinzufügen von Azure-Diensten

### <a name="estimate-cost-online-using-the-pricing-calculator"></a>Schätzen der Kosten mithilfe des Onlinepreisrechners

Verschaffen Sie sich mit dem [Preisrechner](https://azure.microsoft.com/pricing/calculator/) einen ungefähren Eindruck von den monatlichen Kosten des Diensts, für den Sie sich interessieren. Sie können eine beliebige Erstanbieter-Azure-Ressource hinzufügen, um eine Kostenschätzung zu erhalten.

![Screenshot des Preisrechnermenüs](./media/billing-getting-started/pricing-calc.png)

So fallen beispielsweise für die Computestunden eines virtuellen A1-Windows-Computers voraussichtlich 66,96 USD/Monat an, wenn der Computer durchgehend in Betrieb ist:

![Screenshot des Preisrechners, der zeigt, dass für einen virtuellen A1-Windows-Computer pro Monat voraussichtlich Kosten in Höhe von 66,96 USD anfallen](./media/billing-getting-started/pricing-calcVM.png)

Weitere Informationen zu Preisen finden Sie unter diesen [häufig gestellten Fragen](https://azure.microsoft.com/pricing/faq/). Wenn Sie lieber mit einer für den Azure-Vertrieb zuständigen Person sprechen möchten, nutzen die Rufnummer 1-800-867-1389.

### <a name="review-the-estimated-cost-in-the-azure-portal"></a>Überprüfen der geschätzten Kosten im Azure-Portal

Wenn Sie im Azure-Portal einen Dienst hinzufügen, erscheint üblicherweise eine ähnliche Schätzung der monatlichen Kosten. Wenn Sie also etwa die Größe Ihres virtuellen Windows-Computers auswählen, werden die voraussichtlichen monatlichen Kosten für die Computestunden angezeigt:

![Beispiel: Die monatlichen Kosten für einen virtuellen A1-Windows-Computer belaufen sich voraussichtlich auf 66,96 USD.](./media/billing-getting-started/vm-size-cost.PNG)

### <a name="set-up-billing-alerts"></a>Einrichten von Abrechnungswarnungen

Richten Sie Abrechnungswarnungen ein, um sich per E-Mail benachrichtigen zu lassen, wenn Ihre Nutzungskosten einen bestimmten Betrag übersteigen. Falls Sie über monatliches Guthaben verfügen, richten Sie Warnungen für den Verbrauch eines bestimmten Betrags ein. Weitere Informationen finden Sie unter [Einrichten von Abrechnungswarnungen für Microsoft Azure-Abonnements](billing-set-up-alerts.md).

![Screenshot einer E-Mail mit einer Abrechnungswarnung](./media/billing-getting-started/billing-alert.png)

> [!NOTE]
> Da sich dieses Feature noch in der Vorschauphase befindet, sollten Sie Ihre Nutzung in regelmäßigen Abständen überprüfen.

Bei Ihrer ersten Warnung können Sie sich an der Kostenschätzung des Preisrechners orientieren.

### <a name="spending-limit"></a>Überprüfen, ob ein Ausgabenlimit aktiviert ist

Bei Verwendung eines guthabenbasierten Abonnements wird das Ausgabenlimit standardmäßig für Sie aktiviert. Dadurch wird Ihre Kreditkarte nicht belastet, nachdem Sie Ihr gesamtes Guthaben verbraucht haben. Eine vollständige Liste mit Azure-Angeboten sowie Informationen zur Verfügbarkeit des Ausgabenlimits finden Sie [hier](https://azure.microsoft.com/support/legal/offer-details/).

Bei Erreichen Ihres Ausgabenlimits werden Ihre Dienste allerdings deaktiviert. Das bedeutet, dass die Zuordnung Ihrer virtuellen Computer aufgehoben wird. Um Ausfallzeiten zu vermeiden, müssen Sie das Ausgabenlimit deaktivieren. Überschreitungen werden über die hinterlegte Kreditkarte abgerechnet. 

Um zu ermitteln, ob ein Ausgabenlimit aktiviert ist, [navigieren Sie im Kontocenter zur Ansicht „Abonnements“](https://account.windowsazure.com/Subscriptions). Bei aktiviertem Ausgabenlimit wird ein Banner angezeigt:

![Screenshot mit einer Warnung aufgrund eines aktivierten Ausgabenlimits im Kontocenter](./media/billing-getting-started/spending-limit-banner.PNG)

Klicken Sie auf das Banner, und folgen Sie den Anweisungen, um das Ausgabenlimit zu entfernen. Falls Sie bei der Registrierung keine Kreditkarteninformationen eingegeben haben, müssen Sie dies nun nachholen, um das Ausgabenlimit entfernen zu können. Weitere Informationen finden Sie unter [Azure-Ausgabenlimit – Funktionsweise und Aktivierung/Deaktivierung](https://azure.microsoft.com/pricing/spending-limits/).

## <a name="ways-to-monitor-your-costs-when-using-azure-services"></a>Möglichkeiten, Ihre Kosten bei Verwendung von Azure-Diensten zu überwachen

### <a name="tags"></a> Gruppieren von Abrechnungsdaten durch Hinzufügen von Tags zu Ressourcen

Mithilfe von Tags können Sie Abrechnungsdaten für unterstützte Dienste gruppieren. Wenn Sie also beispielsweise mehrere virtuelle Computer für unterschiedliche Teams verwenden, können Sie die Kosten mithilfe von Tags nach Kostenstelle (Personal, Marketing, Finanzen) oder Umgebung (Produktion, Präproduktion, Test) kategorisieren. 

![Screenshot zur Einrichtung von Tags im Portal](./media/billing-getting-started/tags.PNG)

Die Tags werden in den unterschiedlichen Kostenberichtsansichten angezeigt. So sind sie beispielsweise sofort in der [Kostenanalyseansicht](#costs) und nach dem ersten Abrechnungszeitraum in der [CSV-Datei mit den Nutzungsdetails](#invoice-and-usage) zu sehen.

Weitere Informationen finden Sie unter [Verwenden von Tags zum Organisieren von Azure-Ressourcen](../azure-resource-manager/resource-group-using-tags.md).

### <a name="costs"></a> Regelmäßiges Überprüfen der Kostenaufschlüsselung und Verbrauchsrate im Portal

Überprüfen Sie regelmäßig die Kosten, die für Ihre aktiven Dienste anfallen. Die aktuellen Ausgaben sowie die Verbrauchsrate werden im Azure-Portal angezeigt. 

1. Besuchen Sie das [Blatt „Abonnements“ im Azure-Portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade), und wählen Sie ein Abonnement aus.

2. Die Kostenaufschlüsselung und die Verbrauchsrate werden auf dem Popupblatt angezeigt. Sollte es für Ihr Angebot nicht unterstützt werden, erscheint im oberen Bereich eine entsprechende Warnung.

    ![Screenshot der Verbrauchsrate und der Kostenaufschlüsselung im Azure-Portal](./media/billing-getting-started/burn-rate.PNG)

3. Klicken Sie in der Liste auf der linken Seite auf **Kostenanalyse**, um die Kostenaufschlüsselung nach Ressource anzuzeigen. Warten Sie nach dem Hinzufügen eines Diensts 24 Stunden, damit die Daten aufgefüllt werden können.

    ![Screenshot der Kostenanalyseansicht im Azure-Portal](./media/billing-getting-started/cost-analysis.PNG)

4. Sie können nach verschiedenen Eigenschaften wie [Tags](#tags), Ressourcengruppe und Zeitraum filtern. Klicken Sie auf **Übernehmen**, um die Filter zu bestätigen, und auf **Herunterladen**, um die Ansicht in eine CSV-Datei zu exportieren.

5. Darüber hinaus können Sie auf eine Ressource klicken, um den täglichen Ausgabenverlauf und die täglichen Kosten der Ressource anzuzeigen.

    ![Screenshot der Ausgabenverlaufsansicht im Azure-Portal](./media/billing-getting-started/costhistory.PNG)

Es empfiehlt sich, die angezeigten Kosten mit den voraussichtlichen Kosten zu vergleichen, die beim Auswählen der Dienste angezeigt wurden. Sollten die Kosten stark von den Schätzungen abweichen, überprüfen Sie noch einmal den Tarif (etwa A1 im Vergleich zu A0), den Sie für Ihre Ressourcen ausgewählt haben. 

### <a name="consider-enabling-cost-cutting-features-like-auto-shutdown-for-vms"></a>Aktivieren von Kostensenkungsfeatures wie dem automatischen Herunterfahren für virtuelle Computer

Abhängig von Ihrem Szenario können Sie im Azure-Portal ggf. das automatische Herunterfahren für Ihre virtuellen Computer konfigurieren. Weitere Informationen finden Sie unter [Announcing auto-shutdown for VMs using Azure Resource Manager](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/) (Ankündigung des automatischen Herunterfahrens für virtuelle Computer mit Azure Resource Manager).

![Screenshot der Option für automatisches Herunterfahren im Portal](./media/billing-getting-started/auto-shutdown.PNG)

Das automatische Herunterfahren ist nicht dasselbe wie das Herunterfahren des virtuellen Computers über dessen Energieoptionen. Beim automatischen Herunterfahren werden Ihre virtuellen Computer beendet, und ihre Zuordnung wird aufgehoben, um zu verhindern, dass weitere Nutzungsgebühren anfallen. Weitere Informationen finden Sie in den häufig gestellten Fragen zu Preisen für [virtuelle Linux-Computer](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) und [virtuelle Windows-Computer](https://azure.microsoft.com/pricing/details/virtual-machines/windows/).

Informationen zu weiteren Kostensenkungsfeatures für Ihre Entwicklungs- und Testumgebungen finden Sie unter [Azure DevTest Labs](https://azure.microsoft.com/services/devtest-lab/).

### <a name="turn-on-and-check-out-azure-advisor-recommendations"></a>Aktivieren und Überprüfen von Azure Advisor-Empfehlungen

[Azure Advisor](../advisor/advisor-overview.md) ist ein Vorschaufeature, das wenig genutzte Ressourcen identifiziert und Sie so bei der Senkung Ihrer Kosten unterstützt. Aktivieren Sie das Feature im Azure-Portal:

![Screenshot der Schaltfläche für Azure Advisor im Azure-Portal](./media/billing-getting-started/advisor-button.PNG)

Daraufhin stehen auf der Registerkarte **Kosten** des Advisor-Dashboards nützliche Empfehlungen zur Verfügung:

![Screenshot mit einem Beispiel für eine Advisor-Kostenempfehlung](./media/billing-getting-started/advisor-action.PNG)

Weitere Informationen finden Sie unter [Advisor-Empfehlungen zu Kosten](../advisor/advisor-cost-recommendations.md).

## <a name="reviewing-costs-at-the-end-of-your-billing-cycle"></a>Überprüfen der Kosten am Ende des Abrechnungszyklus

Nach dem Ende Ihres Abrechnungszyklus wird Ihre Rechnung zur Verfügung gestellt. Sie können auch [bisherige Rechnungen und detaillierte Verwendungsdateien herunterladen](billing-download-azure-invoice-daily-usage-date.md), um sicherzustellen, dass Sie richtige Rechnungen erhalten haben. Weitere Informationen für den Vergleich Ihrer täglichen Nutzung mit Ihrer Rechnung finden Sie unter [Erläuterungen zur Rechnung für Microsoft Azure](billing-understand-your-bill.md).

### <a name="billing-api"></a>Abrechnungs-API

Über unsere Abrechnungs-API können Sie Nutzungsdaten programmgesteuert abrufen. Verwenden Sie die RateCard-API und die Usage-API, um Ihre abgerechnete Nutzung abzurufen. Weitere Informationen finden Sie unter [Gewinnen von Einblicken in den Ressourcenverbrauch unter Microsoft Azure](billing-usage-rate-card-overview.md).

## <a name="other-offers"></a> Zusätzliche Ressourcen und Sonderfälle

### <a name="ea-csp-and-sponsorship-customers"></a>EA-, CSP- und Sponsorship-Kunden
Wenden Sie sich an Ihren Kundenbetreuer oder Azure-Partner.

| Angebot | Ressourcen |
|-------------------------------|-----------------------------------------------------------------------------------|
| Enterprise Agreement (EA) | [EA-Portal](https://ea.azure.com/), [Hilfedokumente](https://ea.azure.com/helpdocs) und [Power BI-Bericht](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-enterprise/) |
| Cloud-Lösungsanbieter (CSP) | Wenden Sie sich an Ihren Anbieter. |
| Azure Sponsorship | [Sponsorship-Portal](https://www.microsoftazuresponsorships.com/) |

Für IT-Manager großer Organisationen empfehlen wir den Artikel zum [Azure-Unternehmensgerüst](../azure-resource-manager/resource-manager-subscription-governance.md) und das [Whitepaper zu Enterprise IT](http://download.microsoft.com/download/F/F/F/FFF60E6C-DBA1-4214-BEFD-3130C340B138/Azure_Onboarding_Guide_for_IT_Organizations_EN_US.pdf) (PDF-Download, nur auf Englisch verfügbar).

#### <a name="EA"></a> Vorschau von Enterprise Agreement-Kostenansichten im Azure-Portal 

Enterprise-Kostenansichten sind derzeit als öffentliche Vorschau verfügbar. Folgende Punkte sind zu beachten:
- Abonnementkosten basieren auf der Nutzung. Bereits gezahlte Beträge, Überschreitungen, enthaltene Mengen, Anpassungen und Steuern werden nicht berücksichtigt. Die tatsächlichen Gebühren werden auf Registrierungsebene berechnet. 
- Die im Azure-Portal angezeigten Beträge werden im Vergleich zu den Werten im Enterprise-Portal möglicherweise zeitverzögert angezeigt.  
- Wenn keine Kosten angezeigt werden, kann das auf einen der folgenden Gründe zurückzuführen sein:
    - Sie sind Kontobesitzer, und der Registrierungsadministrator hat die Einstellung „Gebühren anzeigen“ für Kontobesitzer deaktiviert.  Wenden Sie sich an den Registrierungsadministrator, um Zugriff auf die Kosten zu erhalten. 
    - Sie sind Abteilungsadministrator, und der Registrierungsadministrator hat die Einstellung „Gebühren anzeigen“ für Abteilungsadministratoren deaktiviert.  Wenden Sie sich an den Registrierungsadministrator, um Zugriff zu erhalten. 
    - Sie haben Azure über einen Channelpartner erworben, und der Partner hat die Preisinformationen nicht freigegeben.  
- Wenn Einstellungen in Bezug auf den Zugriff auf die Kosten im Enterprise-Portal aktualisiert werden, werden die Änderungen erst nach einigen Minuten Verzögerung im Azure-Portal angezeigt.
- Das Ausgabenlimit, Abrechnungsbenachrichtigungen und Rechnungsanleitungen sind für EA-Abonnements nicht relevant.

### <a name="check-your-subscription-and-access"></a>Überprüfen von Abonnement und Zugriff

Zum Anzeigen der Kosten benötigen Sie [Zugriff auf Abrechnungsinformationen auf Abonnementebene](billing-manage-access.md), aber nur der Kontoadministrator kann auf das [Kontocenter](https://account.azure.com/Subscriptions) zugreifen, Abrechnungsinformationen ändern und Abonnements verwalten. Der Kontoadministrator ist die Person, die den Registrierungsprozess durchlaufen hat. Weitere Informationen finden Sie unter [Hinzufügen oder Ändern von Azure-Administratorrollen, die das Abonnement oder die Dienste verwalten](billing-add-change-azure-subscription-administrator.md).

Um zu ermitteln, ob Sie der Kontoadministrator sind, [navigieren Sie im Azure-Portal zum Blatt „Abonnements“](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade), und sehen Sie sich die Liste mit den Abonnements an, auf die Sie Zugriff haben. Ausschlaggebend ist die Angabe unter **Meine Rolle**. Wenn hier *Kontoadministrator* angegeben ist, ist alles in Ordnung. Bei einer Angabe wie *Besitzer* verfügen Sie nicht über sämtliche Berechtigungen.

![Screenshot Ihrer Rolle in der Ansicht „Abonnements“ im Azure-Portal](./media/billing-getting-started/sub-blade-view.PNG)

Falls Sie nicht der Kontoadministrator sind, wurde Ihnen wahrscheinlich von einer anderen Person über die [rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) in Azure Active Directory](../active-directory/role-based-access-control-configure.md) eingeschränkter Zugriff gewährt. Um Abonnements verwalten und Abrechnungsinformationen ändern zu können, müssen Sie [den Kontoadministrator ermitteln](billing-subscription-transfer.md#whoisaa) und ihn bitten, die Aufgaben für Sie auszuführen oder das [Abonnement auf Sie zu übertragen](billing-subscription-transfer.md).

Falls der Kontoadministrator Ihre Organisation verlassen hat und Sie die Abrechnung verwalten müssen, wenden Sie sich an den [Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). 
## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Support kontaktieren

[Wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), falls Sie weitere Hilfe benötigen, um das Problem schnell beheben zu lassen.

