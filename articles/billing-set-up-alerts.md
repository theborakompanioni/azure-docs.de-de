---
title: "Einrichten von Abrechnungswarnungen für Microsoft Azure-Abonnements | Microsoft-Dokumentation"
description: "Hier wird beschrieben, wie Sie Warnungen für Ihre Azure-Rechnung einrichten können, um Überraschungen bei der Abrechnung zu vermeiden."
services: 
documentationcenter: 
author: vikdesai
manager: mbaldwin
editor: 
tags: billing
ms.assetid: 9b7b3eeb-cd9d-4690-86a3-51b1e2a8974f
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/18/2016
ms.author: vikdesai
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 76e72abdb4876af7a3ca9be082ff125a2c70c6c7


---
# <a name="set-up-billing-alerts-for-your-microsoft-azure-subscriptions"></a>Einrichten von Abrechnungswarnungen für Microsoft Azure-Abonnements
Machen Sie sich Gedanken darum, wie viel Sie jeden Monat für Ihr Azure-Abonnement ausgeben? Wenn Sie der Kontoadministrator für ein Azure-Abonnement sind, können Sie den Dienst für Azure-Abrechnungswarnungen zum Erstellen von benutzerdefinierten Abrechnungswarnungen verwenden, mit dem Sie Abrechnungsaktivitäten für Ihre Azure-Konten überwachen und verwalten können.

Dieser Dienst ist ein Vorschaudienst, daher müssen Sie sich zunächst für den Dienst registrieren. Besuchen Sie die [Seite mit den Vorschaufeatures](https://account.windowsazure.com/PreviewFeatures) im Azure-Kontoverwaltungsportal, um dieses Feature zu aktivieren.

## <a name="set-the-alert-threshold-and-email-recipients"></a>Festlegen des Schwellenwerts für Warnungen und von E-Mail-Empfängern
Nachdem Sie die E-Mail-Bestätigung erhalten haben, dass der Abrechnungsdienst für Ihr Abonnement aktiviert ist, besuchen Sie die [Abonnementseite](https://account.windowsazure.com/Subscriptions) im Kontoportal. Klicken Sie auf das zu überwachende Abonnement, und klicken Sie dann auf **Benachrichtigungen**.

![][Image1]

Klicken Sie dann auf **Benachrichtigung hinzufügen**, um die erste Benachrichtigung zu erstellen. Sie können bis zu fünf Abrechnungswarnungen pro Abonnement mit unterschiedlichen Schwellenwerten und mit jeweils bis zu zwei E-Mail-Empfängern einrichten.

![][Image2]

Wenn Sie eine Benachrichtigung hinzufügen, weisen Sie ihr einen eindeutigen Namen zu, wählen einen Ausgabenschwellenwert und dann die E-Mail-Adressen aus, an die Benachrichtigungen gesendet werden. Beim Einrichten des Schwellenwerts können Sie entweder **Abrechnung gesamt** oder **Monetäre Gutschriften** aus der Liste **Warnung für** auswählen. Für "Abrechnung gesamt" wird eine Warnung gesendet, wenn die Abonnementausgaben den Schwellenwert überschreiten. Für "Monetäre Gutschriften" wird eine Warnung gesendet, wenn das Guthaben unter den Grenzwert fällt. "Monetäre Gutschriften" gelten in der Regel für kostenlose Testversionen und Abonnements, die MSDN-Konten zugeordnet sind.

![][Image3]

Azure unterstützt beliebige E-Mail-Adressen, überprüft dabei jedoch nicht, ob die E-Mail-Adresse funktioniert. Vermeiden Sie daher Tippfehler.

## <a name="check-on-your-alerts"></a>Überprüfen Ihrer Warnungen
Nach dem Einrichten von Warnungen werden diese im Account Center aufgelistet. Zudem wird angezeigt, wie viele zusätzliche Warnungen eingerichtet werden können. Für jede Warnung werden Datum und Uhrzeit des Sendevorgangs angezeigt. Außerdem wird das festgelegte Limit und die Information angegeben, ob es sich um eine Abrechnungssumme oder monetäre Gutschrift handelt. Die Uhrzeit wird als UTC-Zeit (Universal Time Coordinate) im 24-Stunden-Format und das Datum im Format JJJJ-MM-TT angegeben. Klicken Sie in der Liste auf das Pluszeichen einer Warnung, um diese zu bearbeiten, oder klicken Sie auf den Papierkorb, um sie zu löschen.

[Bild1]: ./media/azure-billing-set-up-alerts/billingalert1.png
[Bild2]: ./media/azure-billing-set-up-alerts/billingalert2.png
[Bild3]: ./media/azure-billing-set-up-alerts/billingalerts3.png



<!--HONumber=Nov16_HO3-->


