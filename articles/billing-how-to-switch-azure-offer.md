<properties
    pageTitle="Umstellen Ihres Azure-Abonnements auf ein anderes Angebot | Microsoft Azure"
    description="Erfahren Sie, wie Sie Ihr Azure-Abonnement ändern und über das Abonnementverwaltungsportal zu einem anderen Angebot wechseln."
    services=""
    documentationCenter=""
    authors="genlin"
    manager="mbaldwin"
    editor=""
    tags="billing,top-support-issue"/>

<tags
    ms.service="billing"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="genli"/>


# <a name="switch-your-azure-subscription-to-another-offer"></a>Umstellen Ihres Azure-Abonnements auf ein anderes Angebot

Als Benutzer mit [nutzungsbasierter Bezahlung](https://azure.microsoft.com/offers/ms-azr-0003p/) können Sie Ihr Abonnement im [Kontocenter](https://account.windowsazure.com/Subscriptions) unter Umständen auf ein anderes Angebot umstellen. Mithilfe dieses Features können Sie beispielsweise von den [monatlichen Gutschriften für Visual Studio-Abonnenten](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) profitieren. Wenn Sie eine [kostenlose Testversion](https://azure.microsoft.com/free/) besitzen, erfahren Sie, wie Sie ein [Upgrade auf eine nutzungsbasierte Version](billing-buy-sign-up-azure-subscription.md#upgrade-azure-free-trial-to-pay-as-you-go) durchführen.

#### <a name="what's-supported:"></a>Unterstützte Umstellungen:

| Aus                                                              | To
                                                                                      |
|-------------------------------------------------------------------|-----------------------------------------------------------------------------------------|
| [Nutzungsbasierte Bezahlung](https://azure.microsoft.com/offers/ms-azr-0003p/) | [Pay-As-You-Go Dev/Test](https://azure.microsoft.com/offers/ms-azr-0023p/)              |                  |
| [Nutzungsbasierte Bezahlung](https://azure.microsoft.com/offers/ms-azr-0003p/) | [Visual Studio Professional](https://azure.microsoft.com/offers/ms-azr-0059p/)          |
| [Nutzungsbasierte Bezahlung](https://azure.microsoft.com/offers/ms-azr-0003p/) | [Visual Studio Test Professional](https://azure.microsoft.com/offers/ms-azr-0060p/)     |
| [Nutzungsbasierte Bezahlung](https://azure.microsoft.com/offers/ms-azr-0003p/) | [MSDN-Plattformen](https://azure.microsoft.com/offers/ms-azr-0062p/)                      |
| [Nutzungsbasierte Bezahlung](https://azure.microsoft.com/offers/ms-azr-0003p/) | [Visual Studio Enterprise](https://azure.microsoft.com/offers/ms-azr-0063p/)            |
| [Nutzungsbasierte Bezahlung](https://azure.microsoft.com/offers/ms-azr-0003p/) | [Visual Studio Enterprise (Bizspark)](https://azure.microsoft.com/offers/ms-azr-0064p/) |

> [AZURE.NOTE] Um Informationen zu weiteren Angebotsänderungen zu erhalten, [wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
    
## <a name="switch-subscription-offer"></a>Umstellen des Abonnementangebots

> [AZURE.VIDEO switch-to-a-different-azure-offer]

1.  Melden Sie sich beim [Azure-Kontocenter](https://account.windowsazure.com/Subscriptions)an.

2.  Wählen Sie Ihr Abonnement mit nutzungsbasierter Bezahlung aus.

3.  Klicken Sie auf **Zu einem anderen Angebot wechseln**. Die Schaltfläche ist nur verfügbar, wenn Sie die nutzungsbasierte Bezahlung verwenden und der erste Abrechnungszeitraum abgelaufen ist.

    ![switchbutton](./media/billing-how-to-switch-azure-offer/switchbutton.png)
    
4.  **Wählen Sie das gewünschte Angebot** aus der Liste der Angebote aus, auf die Ihr Abonnement umgestellt werden kann. Diese Liste variiert je nach den Mitgliedschaften, denen Ihr Konto zugeordnet ist. Ist kein Angebot verfügbar, sehen Sie sich die [Liste mit verfügbaren Angeboten an, auf die Sie umstellen können](#whats-supported), und vergewissern Sie sich, dass Sie über die richtigen Mitgliedschaften verfügen. 

    ![selectoffer](./media/billing-how-to-switch-azure-offer/selectoffer.png)

5.  Abhängig von dem Angebot, zu dem Sie wechseln möchten, wird möglicherweise ein Hinweis zu den Auswirkungen dieses Wechsels angezeigt. Lesen Sie diese Liste aufmerksam durch, und befolgen Sie die Anweisungen, bevor Sie fortfahren.

    ![thingstonote](./media/billing-how-to-switch-azure-offer/thingstonote.png)

6.  Sie können Ihr Abonnement umbenennen. Standardmäßig verwenden wir den Namen des neuen Angebots. Klicken Sie auf **Angebot wechseln** , um den Vorgang abzuschließen.

    ![confirmpage](./media/billing-how-to-switch-azure-offer/confirmpage.png)

7.  Erfolg! Ihr Abonnement wird jetzt auf das neue Angebot umgestellt.

## <a name="why-can't-i-switch-offers?"></a>Warum kann ich nicht zwischen Angeboten wechseln?

Wenn die Schaltfläche **Zu einem anderen Angebot wechseln** nicht angezeigt wird, kommen folgende Gründe infrage:

- Sie verwenden nicht die [nutzungsbasierte Bezahlung](https://azure.microsoft.com/offers/ms-azr-0003p/). Derzeit können nur Abonnements mit nutzungsbasierter Bezahlung auf ein anderes Angebot umgestellt werden.

    - Wenn Sie eine [kostenlose Testversion](https://azure.microsoft.com/free/) besitzen, erfahren Sie, wie Sie ein [Upgrade auf eine nutzungsbasierte Version](billing-buy-sign-up-azure-subscription.md#upgrade-azure-free-trial-to-pay-as-you-go) durchführen.

    - Wenn Sie von einem anderen Abonnement wechseln möchten, [wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

- Sie befinden sich weiterhin im ersten Abrechnungszeitraum und müssen das Ende des ersten Abrechnungszeitraums abwarten, bevor Sie zwischen Angeboten wechseln können.

Die Meldung **Zurzeit sind keine Angebote in Ihrer Region bzw. Ihrem Land verfügbar.** wird unter Umständen in folgenden Fällen angezeigt:

- Sie sind nicht zu Angebotsumstellungen berechtigt. Informationen finden Sie in der Liste [Verfügbare Angebote für einen Wechsel](#whats-supported).

## <a name="what-does-switching-azure-offers-do-to-my-service-and-billing?"></a>Welche Auswirkungen hat ein Wechsel zwischen Azure-Angeboten auf meinen Dienst und meine Abrechnung?

Hier wird ausführlich erläutert, was geschieht, wenn Sie Azure-Pläne im Kontocenter umstellen.

### <a name="access-to-services"></a>Zugriff auf Dienste

Für die dem Abonnement zugeordneten Benutzer entstehen keine Ausfallzeiten. Allerdings können für das Angebot, zu dem Sie wechseln möchten, Einschränkungen gelten. Manche Angebote können beispielsweise nicht in der Produktion verwendet werden. Daher müssten Sie Produktionsressourcen in ein anderes Abonnement verschieben.

### <a name="billing"></a>Abrechnung

An dem Tag, an dem Sie das Angebot wechseln, wird für alle ausstehenden Gebühren eine Rechnung generiert. Anschließend wird Ihr Abonnement gemäß den Preisinformationen für das neue Angebot abgerechnet. Der Stichtag Ihrer Abonnementabrechnung wird in das Datum geändert, an dem Sie das Angebot gewechselt haben. Die Nutzungs- und Abrechnungsdaten von vor der Angebotsänderung werden nicht beibehalten. Daher empfiehlt es sich, vor dem Wechsel eine Kopie davon herunterzuladen.

> [AZURE.NOTE] Aufgrund abrechnungstechnischer Einschränkungen sind Angebotsumstellungen innerhalb des ersten Abrechnungszyklus nach dem Erstellen eines Abonnements nicht möglich.

## <a name="can-i-migrate-from-pay-as-you-go-to-[cloud-solution-provider](https://partner.microsoft.com/solutions/cloud-reseller-overview)-(csp)-or-[enterprise-agreement](https://azure.microsoft.com/pricing/enterprise-agreement/)-(ea)?"></a>Kann ich von der nutzungsbasierten Bezahlung zu [Cloudlösungsanbieter](https://partner.microsoft.com/Solutions/cloud-reseller-overview) (Cloud Solution Provider, CSP) oder [Enterprise Agreement](https://azure.microsoft.com/pricing/enterprise-agreement/) (EA) migrieren?

Derzeit wird die Umstellung auf CSP oder EA im Kontocenter nicht unterstützt. Um Ihr bestehendes Abonnement in EA zu verschieben, muss Ihr Registrierungsadministrator Ihr Konto in EA hinzufügen. Anschließend erhalten Sie eine Einladung per E-Mail. Wenn Sie die Anweisungen zum Annehmen der Einladung befolgen, werden Ihre Abonnements automatisch in das Enterprise Agreement verschoben. Zurzeit gibt es noch keine Option zum Verschieben eines vorhandenen Abonnements in CSP.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie für Ihr Abonnement [Administratorrollen verwalten](billing-add-change-azure-subscription-administrator.md) .

- Verfolgen Sie die Verwendung durch [Herunterladen von Verwendungsdaten und Rechnung](billing-download-azure-invoice-daily-usage-date.md)

## <a name="need-help?-contact-support."></a>Sie brauchen Hilfe? Wenden Sie sich an den Support.

Wenn Sie weitere Fragen haben, [wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) , um das Problem schnell zu lösen.



<!--HONumber=Oct16_HO2-->


