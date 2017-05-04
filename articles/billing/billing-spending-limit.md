---
title: Grundlegendes zum Azure-Ausgabenlimit | Microsoft-Dokumentation
description: Beschreibung der Funktionsweise und zum Entfernen des Azure-Ausgabenlimits
services: 
documentationcenter: 
author: genlin
manager: vikdesai
editor: 
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2017
ms.author: genli
translationtype: Human Translation
ms.sourcegitcommit: abdbb9a43f6f01303844677d900d11d984150df0
ms.openlocfilehash: 39ae134d8927f5123667b64bbd0c659cd5f62ffc
ms.lasthandoff: 04/20/2017


---
# <a name="understand-azure-spending-limit-and-how-to-remove-it"></a>Grundlegendes zum Azure-Ausgabenlimit und zu dessen Entfernung

Mit dem Azure-Ausgabenlimit werden die Ausgaben für Ihr Azure-Abonnement begrenzt. Für alle neuen Kunden, die sich für das Testangebot oder für Angebote registrieren, die Gutschriften für mehrere Monate beinhalten, ist das Ausgabenlimit standardmäßig aktiviert. Das Ausgabenlimit beträgt 0 $. Es kann nicht geändert werden. Für Abonnementtypen wie Abonnements mit nutzungsbasierter Bezahlung oder Pläne mit finanzieller Verpflichtung ist das Ausgabenlimit nicht verfügbar. Eine vollständige Liste mit Azure-Angeboten sowie Informationen zur Verfügbarkeit des Ausgabenlimits finden Sie [hier](https://azure.microsoft.com/support/legal/offer-details/).

## <a name="what-happens-when-i-reach-the-spending-limit"></a>Was passiert, wenn ich das Ausgabenlimit erreiche?

Wenn durch Ihre Nutzung Gebühren anfallen, die die monatlichen Gutschriften Ihres Angebots überschreiten, werden die von Ihnen bereitgestellten Dienste für den verbleibenden Zeitraum des jeweiligen Abrechnungsmonats deaktiviert. Von Ihnen bereitgestellte Cloud Services werden beispielsweise aus der Produktion entfernt, Ihre Azure-VMs werden angehalten, und die Zuordnung wird aufgehoben. Um zu verhindern, dass Ihre Dienste deaktiviert werden, können Sie Ihr Ausgabenlimit entfernen. Wenn Ihre Dienste deaktiviert werden, werden die Daten in Ihren Speicherkonten und Datenbanken schreibgeschützt für Administratoren zur Verfügung gestellt. Wenn Ihr Angebot Gutschriften für mehrere Monate beinhaltet, wird Ihr Abonnement zu Beginn des nächsten Abrechnungsmonats erneut aktiviert. Sie können dann Ihre Cloud Services-Instanzen erneut bereitstellen und haben Vollzugriff auf Ihre Speicherkonten und Datenbanken.

Wenn Sie bei Verwendung der kostenlosen Testversion das Ausgabenlimit erreichen, können Sie Ihr Abonnement innerhalb von 90 Tagen erneut aktivieren und damit automatisch zum [Standardangebot mit nutzungsbasierter Bezahlung wechseln](billing-upgrade-azure-subscription.md).

Wenn Sie das Ausgabenlimit für das Angebot erreichen, erhalten Sie entsprechende Benachrichtigungen. Melden Sie sich beim [Azure-Kontocenter](https://account.windowsazure.com) an, und wählen Sie **KONTO** und dann **Abonnements** aus. Es werden Benachrichtigungen zu Abonnements angezeigt, bei denen das Ausgabenlimit erreicht wurde.

## <a name="things-you-are-charged-for-even-if-you-have-a-spending-limit-enabled"></a>Mögliche Gebühren, selbst wenn ein Ausgabenlimit aktiviert wurde

Bei einigen Azure-Diensten und [Marketplace-Angeboten](https://azure.microsoft.com/marketplace/) können selbst dann Gebühren anfallen und über die jeweilige Zahlungsmethode (Kreditkarte) abgerechnet werden, wenn ein Ausgabenlimit festgelegt ist. Beispiele sind Visual Studio-Lizenzen, Azure Active Directory Premium, Supportpläne und die meisten Drittanbieterdienste, die über den Marketplace vertrieben werden.


## <a name="when-not-to-use-the-spending-limit"></a>Szenarien, in denen das Ausgabenlimit nicht verwendet werden sollte

Das Ausgabenlimit könnte verhindern, dass Sie bestimmte Marketplace- und Microsoft-Dienste bereitstellen und verwenden können. Im Folgenden sind Szenarien aufgeführt, in denen Sie das Ausgabenlimit für Ihr Abonnement entfernen sollten.

- Sie planen, Images von Erstanbietern wie Oracle und Dienste wie Visual Studio Team Services bereitzustellen. Mit diesem Szenario überschreiten Sie praktisch umgehend das Ausgabenlimit, sodass Ihr Abonnement deaktiviert wird.

- Sie verfügen über Dienste, für die es keinesfalls zu einer Unterbrechung kommen darf.

- Sie verfügen über Dienste und Ressourcen mit Einstellungen wie virtuellen IP-Adressen, die nicht verloren gehen sollen. Wenn die Zuordnung der Dienste und Ressourcen aufgehoben wird, gehen diese Einstellungen verloren.


## <a name="remove-the-spending-limit"></a>Entfernen des Ausgabenlimits

Wenn eine gültige Zahlungsmethode für Ihr Abonnement festgelegt wurde, können Sie das Ausgabenlimit jederzeit entfernen. Bei Angeboten mit Gutschriften für mehrere Monate können Sie das Ausgabenlimit darüber hinaus zu Beginn des nächsten Abrechnungszyklus erneut aktivieren.

Führen Sie die folgenden Schritte aus, um das Ausgabenlimit zu entfernen:

1. Melden Sie sich beim [Azure-Kontocenter](https://account.windowsazure.com) an.

2. Wählen Sie ein Abonnement aus.

3. Wenn das Abonnement deaktiviert wurde, weil das Ausgabenlimit erreicht wurde, klicken Sie auf die Benachrichtigung „Das Abonnement wurde deaktiviert, um zu verhindern, dass Gebühren anfallen.“ Klicken Sie andernfalls im Bereich **ABONNEMENTSTATUS** auf **Ausgabenlimit entfernen**.

4. Wählen Sie die gewünschte Option aus.

|Option|Effekt|
|-------|-----|
|Ausgabenlimit auf unbestimmte Zeit deaktivieren|Entfernt das Ausgabenlimit, ohne es automatisch am Anfang des nächsten Abrechnungszeitraums wieder zu aktivieren.|
|Ausgabenlimit für den aktuellen Abrechnungszeitraum deaktivieren|Entfernt das Ausgabenlimit, sodass es automatisch am Anfang des nächsten Abrechnungszeitraums wieder aktiviert wird.|

## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Wenden Sie sich an den Support.
[Wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), falls Sie weitere Hilfe benötigen, um das Problem schnell beheben zu lassen.
