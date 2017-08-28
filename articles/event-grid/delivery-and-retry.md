---
title: "Azure Event Grid – Übermittlung und Wiederholung"
description: "Beschreibt, wie Azure Event Grid Ereignisse übermittelt und wie nicht übermittelte Nachrichten verarbeitet werden."
services: event-grid
author: djrosanova
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 08/11/2017
ms.author: darosa
ms.translationtype: HT
ms.sourcegitcommit: 540180e7d6cd02dfa1f3cac8ccd343e965ded91b
ms.openlocfilehash: e0f8afdfd84ea3c0c061459c27da285f6ae8957e
ms.contentlocale: de-de
ms.lasthandoff: 08/16/2017

---

# <a name="event-grid-message-delivery-and-retry"></a>Event Grid – Übermittlung und Wiederholung von Nachrichten 

In diesem Artikel wird beschrieben, wie Azure Event Grid Ereignisse verarbeitet, wenn die Übermittlung nicht bestätigt wird.

Event Grid bietet permanente Übermittlung. Jede Nachricht wird für jedes Abonnement mindestens einmal übermittelt. Ereignisse werden sofort an den registrierten Webhook des jeweiligen Abonnements gesendet. Wenn ein Webhook den Eingang eines Ereignisses nicht innerhalb von 60 Sekunden nach dem ersten Übermittlungsversuch bestätigt, wiederholt Event Grid die Übermittlung des Ereignisses.

## <a name="message-delivery-status"></a>Nachrichtenübermittlungsstatus

Event Grid verwendet HTTP-Antwortcodes zum Bestätigen des Eingangs von Ereignissen. 

### <a name="success-codes"></a>Erfolgscodes

Die folgenden HTTP-Antwortcodes geben an, dass ein Ereignis erfolgreich an den Webhook übermittelt wurde. Die Übermittlung gilt dann in Event Grid als abgeschlossen.

- 200 – OK
- 202 – Akzeptiert

### <a name="failure-codes"></a>Fehlercodes

Die folgenden HTTP-Antwortcodes geben an, dass bei der Übermittlung eines Ereignisses ein Fehler aufgetreten ist. Event Grid versucht erneut, das Ereignis zu senden. 

- 400 – Ungültige Anforderung
- 401 – Nicht autorisiert
- 404 – Nicht gefunden
- 408 – Anforderungstimeout
- 414 – URI zu lang
- 500 Interner Serverfehler
- 503 Dienst nicht verfügbar
- 504 Gateway-Timeout

Alle anderen Antwortcodes oder eine fehlende Antwort geben einen Fehler an. Event Grid wiederholt die Übermittlung. 

## <a name="retry-intervals"></a>Wiederholungsintervalle

Event Grid verwendet exponentiell ansteigende Wartezeiten für Wiederholungsversuche für die Ereignisübermittlung. Wenn der Webhook nicht antwortet oder einen Fehlercode zurückgibt, wiederholt Event Grid die Übermittlung nach folgendem Zeitplan:

1. 10 Sekunden
2. 30 Sekunden
3. 1 Minute
4. 5 Minuten
5. 10 Minuten
6. 30 Minuten
7. 1 Stunde

Event Grid fügt allen Wiederholungsintervallen eine geringfügige zufällige Anordnung hinzu.

## <a name="retry-duration"></a>Wiederholungszeitraum

Während der Vorschauphase laufen alle Ereignisse in Azure Event Grid ab, die nicht innerhalb von zwei Stunden übermittelt werden. Vor der allgemeinen Verfügbarkeit wird dieser Zeitraum auf 24 Stunden erhöht. 

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung zu Event Grid finden Sie unter [Informationen zu Event Grid](overview.md).
* Einen schnellen Einstieg in die Verwendung von Event Grid finden Sie unter [Erstellen und Weiterleiten benutzerdefinierter Ereignisse mit Azure Event Grid](custom-event-quickstart.md).
