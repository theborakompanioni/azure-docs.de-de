<properties
    pageTitle="Grundlegendes zu Azure-Gebühren für externe Dienste | Microsoft Azure"
    description="Informieren Sie sich über die Gebührenabrechnung für externe Dienste in Azure (früher Marketplace)."
    services=""
    documentationCenter=""
    authors="adpick"
    manager="felixwu"
    editor=""
    tags="billing"
    />

<tags
    ms.service="billing"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="adpick"/>


# <a name="understand-your-azure-external-service-charges"></a>Grundlegendes zu Azure-Gebühren für externe Dienste

In diesem Artikel wird die Abrechnung bei externen Diensten in Azure beschrieben. Externe Dienste wurden in der Vergangenheit als Marketplace-Bestellungen bezeichnet. Externe Dienste werden von unabhängigen Dienstanbietern angeboten, sind jedoch vollständig in das Azure-Ökosystem integriert. In diesem Artikel werden folgende Themen erläutert:

- Identifizieren von externen Diensten
- Grundlegendes zu den Unterschieden bei der Abrechnung im Vergleich zu anderen Azure-Ressourcen
- Anzeigen und Nachverfolgen der antizipierten Kosten durch die Nutzung externer Dienste
- Verwalten von Bestellungen externer Dienste sowie der Bezahlung dieser Dienste

## <a name="what-are-azure-external-services?"></a>Was sind externe Azure-Dienste?

Externe Dienste wurden in der Vergangenheit als Azure Marketplace-Angebote bezeichnet. Diese Dienste werden im Allgemeinen von Drittanbietern veröffentlicht und für Azure zur Verfügung gestellt. Beispiele für externe Dienste, die Sie in Azure erwerben können, die jedoch nicht von Microsoft veröffentlicht werden, sind ClearDB und SendGrid.

### <a name="identify-external-services"></a>Identifizieren von externen Diensten

Beim Bereitstellen eines neuen externen Diensts oder einer neuen externen Ressource wird eine Warnung angezeigt:

![Warnung beim Erwerb von Marketplace-Angeboten](./media/billing-understand-your-azure-marketplace-charges/marketplace-warning.PNG)

>[AZURE.NOTE] Externe Dienste werden von Drittanbietern veröffentlicht, einige Microsoft-Produkte sind jedoch ebenfalls als externe Dienste gekennzeichnet.

### <a name="external-services-are-billed-separately"></a>Externe Dienste werden separat in Rechnung gestellt.

Externe Dienste werden als einzelne Bestellungen innerhalb Ihres Azure-Abonnements behandelt. Der Abrechnungszeitraum für die einzelnen Dienste wird beim Erwerb des Diensts festgelegt. Dieser Zeitraum darf nicht mit dem Abrechnungszeitraum des Abonnements verwechselt werden, das zum Kauf des externen Diensts verwendet wurde. Sie erhalten separate Rechnungen, und Ihre Kreditkarte wird separat belastet.

### <a name="each-external-service-has-a-different-billing-model"></a>Jeder externer Dienst verwendet ein eigenes Abrechnungsmodell.

Für einige Dienste wird die nutzungsbasierte Bezahlung verwendet, anderen liegt ein monatliches Zahlungsmodell zugrunde. Für externe Azure-Dienste wird eine Kreditkarte benötigt, eine Zahlung per Rechnung ist nicht möglich.

### <a name="you-can't-use-monthly-free-credits-for-external-services"></a>Die kostenlosen monatlichen Gutschriften können nicht für externe Dienste verwendet werden.

Wenn Sie ein Azure-Abonnement mit [kostenlosen Gutschriften](https://azure.microsoft.com/pricing/spending-limits/) verwenden, können diese Gutschriften nicht zum Begleichen von Rechnungen genutzt werden, die Sie für externe Dienste erhalten. Nutzen Sie eine Kreditkarte, um externe Dienste zu erwerben.

## <a name="view-external-service-spending-and-history"></a>Anzeigen der Ausgaben und des Verlaufs für externe Dienste

Im [Azure-Portal](https://portal.azure.com/) kann eine Liste der externen Dienste angezeigt werden, die unter den einzelnen Abonnements erworben wurden: 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an, und [wechseln Sie auf das Blatt **Abrechnung**](https://portal.azure.com/?flight=1#blade/Microsoft_Azure_Billing/BillingBlade).

    ![Auswählen der Option „Abrechnung“ im Hubmenü](./media/billing-understand-your-azure-marketplace-charges/billing-button.png) 
  
2. Wählen Sie im Abschnitt **Abonnementkosten** das Abonnement aus, das angezeigt werden soll. 
   
    ![Auswählen eines Abonnements auf dem Blatt „Abrechnung“](./media/billing-understand-your-azure-marketplace-charges/select-sub.png)

3. Klicken Sie auf **Externe Dienste**.

    ![Auswählen von „Externe Dienste“ auf dem Blatt „Abonnement“](./media/billing-understand-your-azure-marketplace-charges/external-service-blade.png)

4. In dieser Anzeige sollten die einzelnen externen Dienste, der Name des Herausgebers, der erworbene Tarif, der von Ihnen zugewiesene Ressourcenname und der aktuelle Bestellstatus angezeigt werden. Wählen Sie einen externen Dienst aus, um die Rechnungen anzuzeigen, die bereits erstellt wurden.

    ![Auswählen eines externen Diensts](./media/billing-understand-your-azure-marketplace-charges/external-service-blade2.png)

5. In dieser Ansicht können Sie die Rechnungsbeträge einschließlich Steuern anzeigen.

    ![Anzeigen des Abrechnungsverlaufs für externe Dienste](./media/billing-understand-your-azure-marketplace-charges/billing-overview-blade.png)

## <a name="manage-payment-methods-for-external-service-orders"></a>Verwalten der Zahlungsmethoden für Bestellungen externer Dienste

Ihre Zahlungsmethoden für externe Dienste können über das [Kontocenter](https://account.windowsazure.com/) aktualisiert werden.

> [AZURE.NOTE] Wenn Sie Ihr Abonnement über ein Geschäfts-, Schul- oder Unikonto erworben haben, [kontaktieren Sie den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), um Ihre Zahlungsmethode zu ändern.

1. Melden Sie sich beim [Kontocenter](https://account.windowsazure.com/) an, und [navigieren Sie auf die Registerkarte **Marketplace**](https://account.windowsazure.com/Store).

    ![Auswählen von „Marketplace“ im Kontocenter](./media/billing-understand-your-azure-marketplace-charges/select-marketplace.png)

2. Auswählen des externen Diensts, der verwaltet werden soll

    ![Auswählen des externen Diensts, der verwaltet werden soll](./media/billing-understand-your-azure-marketplace-charges/select-ext-service.png)

3. Klicken Sie rechts auf der Seite auf **Zahlungsmethode ändern**. Sie gelangen in ein anderes Portal, um Ihre Zahlungsmethode zu verwalten.
    
    ![Bestellübersicht](./media/billing-understand-your-azure-marketplace-charges/change-payment.PNG)

4. Klicken Sie auf **Informationen bearbeiten**, und befolgen Sie die Anweisungen zum Aktualisieren Ihrer Zahlungsinformationen.

    ![Auswählen von „Informationen bearbeiten“](./media/billing-understand-your-azure-marketplace-charges/edit-info.png)
    
## <a name="cancel-an-external-service-order"></a>Kündigen eines externen Diensts

Wenn Sie einen externen Dienst kündigen möchten, müssen Sie die Ressource im [Azure-Portal](https://portal.azure.com) löschen.

![Löschen der Ressource](./media/billing-understand-your-azure-marketplace-charges/deleteMarketplaceOrder.PNG)

## <a name="need-help?-contact-support."></a>Sie brauchen Hilfe? Wenden Sie sich an den Support.

Wenn Sie weitere Fragen haben, [wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) , um das Problem schnell zu lösen.



<!--HONumber=Oct16_HO2-->


