---
title: Grundlegendes zum Azure-Ausgabenlimit | Microsoft-Dokumentation
description: Beschreibung der Funktionsweise und zum Entfernen des Azure-Ausgabenlimits
services: 
documentationcenter: 
author: genlin
manager: jlian
editor: 
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/08/2017
ms.author: genli
ms.translationtype: HT
ms.sourcegitcommit: 2c6cf0eff812b12ad852e1434e7adf42c5eb7422
ms.openlocfilehash: 311a1af35b7fecb5f0d4fac7f38af705c945d714
ms.contentlocale: de-de
ms.lasthandoff: 09/13/2017

---
# <a name="understand-azure-spending-limit-and-how-to-remove-it"></a>Grundlegendes zum Azure-Ausgabenlimit und zu dessen Entfernung

Mit dem Ausgabenlimit in Azure wird verhindert, dass Sie Ihren Gutschriftbetrag überschreiten. Für alle neuen Kunden, die sich für die Testversion oder für Angebote registrieren, die Gutschriften für mehrere Monate beinhalten, ist das Ausgabenlimit standardmäßig aktiviert. Das Ausgabenlimit beträgt 0 $. Es kann nicht geändert werden. Für Abonnementtypen wie Abonnements mit nutzungsbasierter Bezahlung oder Pläne mit finanzieller Verpflichtung ist das Ausgabenlimit nicht verfügbar. Eine vollständige Liste mit Azure-Angeboten sowie Informationen zur Verfügbarkeit des Ausgabenlimits finden Sie [hier](https://azure.microsoft.com/support/legal/offer-details/).

**Sie möchten Abrechnungsbenachrichtigungen erhalten?** Siehe dazu [Einrichten von Abrechnungs- oder Guthabenbenachrichtigungen für Azure-Abonnements](billing-set-up-alerts.md).

## <a name="what-happens-when-i-reach-the-spending-limit"></a>Was passiert, wenn ich das Ausgabenlimit erreiche?

Wenn durch Ihre Nutzung Gebühren anfallen, die die monatlichen Gutschriften Ihres Abonnements überschreiten, werden die von Ihnen bereitgestellten Dienste für den verbleibenden Zeitraum des jeweiligen Abrechnungszeitraums deaktiviert. 

Wenn Sie beispielsweise die gesamte in Ihrem Abonnement enthaltene Gutschrift verbrauchen, werden die von Ihnen bereitgestellten Clouddienste aus der Produktion entfernt sowie Ihre virtuellen Azure-Computer angehalten und die entsprechende Zuordnung aufgehoben. Die Daten in Ihren Speicherkonten und Datenbanken sind in diesem Fall nur schreibgeschützt verfügbar.

Wenn Ihr Abonnementangebot Gutschriften für mehrere Monate beinhaltet, wird Ihr Abonnement zu Beginn des nächsten Abrechnungszeitraums automatisch erneut aktiviert. Sie können dann Ihre Cloud Services-Instanzen erneut bereitstellen und haben Vollzugriff auf Ihre Speicherkonten und Datenbanken.

Wenn Sie das Ausgabenlimit für Ihr Abonnement erreichen, senden wir entsprechende E-Mail-Benachrichtigungen. Melden Sie sich beim [Kontocenter](https://account.windowsazure.com/Subscriptions) an. Dann werden Benachrichtigungen zu Abonnements angezeigt, bei denen das Ausgabenlimit erreicht wurde.

Wenn Sie über eine kostenlose Testversion verfügen und das Ausgabenlimit erreichen, können Sie [ein Upgrade auf nutzungsbasierte Bezahlung durchführen](billing-upgrade-azure-subscription.md), damit das Ausgabenlimit entfernt und das Abonnement automatisch erneut aktiviert wird.

<a id="remove"></a>

## <a name="remove-the-spending-limit-in-account-center"></a>Entfernen des Ausgabenlimits im Kontocenter

Wenn eine gültige Zahlungsmethode für Ihr Abonnement festgelegt wurde, können Sie das Ausgabenlimit jederzeit entfernen. Bei Angeboten mit Gutschriften für mehrere Monate können Sie das Ausgabenlimit darüber hinaus zu Beginn des nächsten Abrechnungszeitraums erneut aktivieren.

Führen Sie die folgenden Schritte aus, um das Ausgabenlimit zu entfernen:

1. Melden Sie sich beim [Kontocenter](https://account.windowsazure.com/Subscriptions) an.
1. Wählen Sie ein Abonnement aus.
1. Wenn das Abonnement deaktiviert wurde, weil das Ausgabenlimit erreicht wurde, klicken Sie auf die Benachrichtigung „Abonnement hat das Ausgabenlimit erreicht und wurde deaktiviert, um zu verhindern, dass Kosten anfallen.“ Klicken Sie andernfalls im Bereich **ABONNEMENTSTATUS** auf **Ausgabenlimit entfernen**.
1. Wählen Sie die gewünschte Option aus.

|Option|Effekt|
|-------|-----|
|Ausgabenlimit auf unbestimmte Zeit deaktivieren|Entfernt das Ausgabenlimit, ohne es automatisch am Anfang des nächsten Abrechnungszeitraums wieder zu aktivieren.|
|Ausgabenlimit für den aktuellen Abrechnungszeitraum deaktivieren|Entfernt das Ausgabenlimit, sodass es automatisch am Anfang des nächsten Abrechnungszeitraums wieder aktiviert wird.|

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

### <a name="why-would-i-want-to-remove-the-spending-limit"></a>Warum sollte ich das Ausgabenlimit entfernen?

Das Ausgabenlimit könnte verhindern, dass Sie bestimmte Drittanbieterdienste und Microsoft-Dienste bereitstellen und verwenden können. Im Folgenden sind Szenarien aufgeführt, in denen Sie das Ausgabenlimit für Ihr Abonnement entfernen sollten.

* Sie planen, Images von Erstanbietern wie Oracle und Dienste wie Visual Studio Team Services bereitzustellen. Mit diesem Szenario überschreiten Sie praktisch umgehend das Ausgabenlimit, sodass Ihr Abonnement deaktiviert wird.
* Sie verfügen über Dienste, für die es keinesfalls zu einer Unterbrechung kommen darf.
* Sie verfügen über Dienste und Ressourcen mit Einstellungen wie virtuellen IP-Adressen, die nicht verloren gehen sollen. Wenn die Zuordnung der Dienste und Ressourcen aufgehoben wird, gehen diese Einstellungen verloren.

### <a name="how-do-i-turn-on-the-spending-limit-after-removing-it"></a>Wie aktiviere ich das Ausgabenlimit nach dem Entfernen?

Dieses Feature ist nur verfügbar, wenn das Ausgabenlimit auf unbestimmte Zeit entfernt wurde. Aktivieren Sie es, damit es zu Beginn des nächsten Abrechnungszeitraums automatisch gestartet wird.

1. Melden Sie sich beim [Kontocenter](https://account.windowsazure.com/Subscriptions) an.
1. Klicken Sie auf das gelbe Banner, um die Option „Ausgabenlimit“ zu ändern.
1. Wählen Sie **Ausgabenlimit im nächsten Abrechnungszeitraum aktivieren \<Startdatum des Abrechnungszeitraum\>**.

### <a name="how-do-i-set-a-custom-spending-limit"></a>Wie kann ich ein benutzerdefiniertes Ausgabenlimit festlegen?

Derzeit stehen keine benutzerdefinierten Ausgabenlimits zur Verfügung. Sie können jedoch [Abrechnungsbenachrichtigungen zum Steuern Ihrer Ausgaben verwenden](billing-set-up-alerts.md).

### <a name="does-the-spending-limit-prevent-all-charges-from-azure"></a>Können mit dem Ausgabenlimit alle Gebühren von Azure verhindert werden?

Bei einigen [Microsoft-Diensten und Diensten von Drittanbietern](billing-understand-your-azure-marketplace-charges.md) können selbst dann Gebühren anfallen und über die jeweilige Zahlungsmethode abgerechnet werden, wenn ein Ausgabenlimit festgelegt ist. Beispiele sind Visual Studio-Lizenzen, Azure Active Directory Premium, Supportpläne und die meisten Drittanbieterdienste, die über den [Azure Marketplace](https://azure.microsoft.com/marketplace/) vertrieben werden.

## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Wenden Sie sich an den Support.

[Wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), falls Sie weitere Hilfe benötigen, um das Problem schnell beheben zu lassen.

