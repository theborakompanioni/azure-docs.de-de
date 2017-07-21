---
title: Azure Application Insights-Trichter
description: "In diesem Artikel wird erläutert, wie Sie Trichter dazu verwenden können, sich über die Interaktionen der Kunden mit Ihrer Anwendung zu informieren."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: cfreeman
ms.translationtype: HT
ms.sourcegitcommit: c3ea7cfba9fbf1064e2bd58344a7a00dc81eb148
ms.openlocfilehash: 8d20995ebb63e8cf1e4d15c57628b80aaf9e036b
ms.contentlocale: de-de
ms.lasthandoff: 07/19/2017

---

# <a name="discover-how-customers-are-using-your-application-with-the-application-insights-funnels"></a>Informieren Sie sich mit den Application Insights-Trichtern darüber, wie die Kunden Ihre Anwendung verwenden

Es ist für Ihr Unternehmen von essenzieller Bedeutung, ein Gefühl für die Kundenerfahrung zu entwickeln. Wenn in Ihrer Anwendung mehrere Schritte durchlaufen werden, müssen Sie wissen, ob die meisten Kunden den gesamten Prozess durchlaufen oder diesen an einem bestimmten Punkt beenden. Der Fortschritt beim Ausführen einer Reihe von Schritten in einer Webanwendung wird als „Trichter“ bezeichnet. Sie können die Application Insights-Trichter dazu verwenden, Erkenntnisse über Ihre Benutzer zu erhalten und die schrittweisen Konvertierungsraten zu überwachen. 

## <a name="get-started-with-the-funnels-blade"></a>Erste Schritte mit dem Blatt „Trichter“
Die einfachste Möglichkeit, sich mit den Trichtern vertraut zu machen, ist ein Beispiel zu betrachten. Die folgenden Abbildungen zeigen die Schritte, die die Besitzer eines E-Commerce-Geschäfts unternehmen würden, um herauszufinden, wie ihre Kunden mit der Webanwendung interagieren.  

### <a name="create-your-funnel"></a>Erstellen Ihres Trichters
Bevor Sie Ihren Trichter erstellen, müssen Sie eine Entscheidung darüber treffen, welche Frage Sie mit dem Trichter beantworten möchten. Beispielsweise möchten Sie vielleicht herausfinden, wie viele Besucher Ihrer Homepage eine Werbung anklicken. Im vorliegenden Beispiel möchten die Besitzer des Unternehmens „Fabrikam Fiber“ herausfinden, welcher Prozentsatz der Kunden im letzten Monat einen Kauf getätigt haben, nachdem sie Artikel zu ihrem Einkaufswagen hinzugefügt hatten.

Im Folgenden werden die Schritte zum Erstellen ihres Trichters beschrieben.

1. Klicken Sie im Blatt „Trichter“ auf die Schaltfläche „Neu“.
1. Wählen Sie als **Zeitraum** den letzten Monat aus der Dropdownliste aus. 
1. Wählen Sie das Ereignis **Produktseite** aus der Dropdownliste aus **Schritt 1** aus. 
1. Wählen Sie das Ereignis **Zum Einkaufswagen hinzufügen** aus der Dropdownliste aus **Schritt 2** aus.
1. Wählen Sie das Ereignis **Auf Bestellung klicken** aus der Dropdownliste aus **Schritt 3** aus.
1. Fügen Sie einen Namen zu dem Trichter hinzu, und klicken Sie auf **Speichern**.

Die folgende Abbildung zeigt die vom Blatt „Trichter“ generierten Daten. Hier können die Fabrikam-Besitzer sehen, dass in der letzten Woche 22,7 Prozent derjenigen Kunden, die einen Artikel zu ihrem Einkaufswagen hinzugefügt haben, den Kauf auch abgeschlossen haben. Weiterhin ist zu sehen, dass ein Prozent der Kunden vor dem Besuch der Produktseite auf eine Werbung geklickt und 20 Prozent der Kunden sich nach Abschluss des Einkaufs abgemeldet haben.


![Blatt „Trichter“ mit Daten](./media/app-insights-understand-usage-patterns/funnel1.png)

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr über [Nutzungsanalyse](app-insights-usage-overview.md).
