---
title: "Service Bus – Preise und Abrechnung | Microsoft Docs"
description: "Übersicht über die Service Bus-Preisstruktur."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 7c45b112-e911-45ab-9203-a2e5abccd6e0
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/28/2017
ms.author: sethm
ms.translationtype: HT
ms.sourcegitcommit: 1c730c65194e169121e3ad1d1423963ee3ced8da
ms.openlocfilehash: 8f693bc51fc9635fae4376137e7e573bf74da7cb
ms.contentlocale: de-de
ms.lasthandoff: 08/30/2017

---
# <a name="service-bus-pricing-and-billing"></a>Service Bus – Preise und Abrechnung
Service Bus wird in den Tarifen Standard und [Premium](service-bus-premium-messaging.md) angeboten. Sie können einen Diensttarif für jeden von Ihnen erstellten Service Bus-Dienstnamespace auswählen, und diese Tarifauswahl gilt für alle Warteschlangen, Entitäten, die in diesem Namespace erstellt werden.

> [!NOTE]
> Ausführliche Informationen zu den aktuellen Service Bus-Preisen finden Sie auf der [Azure Service Bus-Preisseite](https://azure.microsoft.com/pricing/details/service-bus/) und in den [häufig gestellten Fragen zu Service Bus](service-bus-faq.md#pricing).
>
>

Service Bus verwendet die zwei folgenden Verbrauchseinheiten für Warteschlangen und Themen/Abonnements:

1. **Messagingvorgänge**: Definiert als API-Aufrufe für Warteschlangen- oder Themen-/Abonnementdienstendpunkte. Diese Verbrauchseinheit ersetzt gesendete oder empfangene Nachrichten als primäre Einheit der abrechenbaren Nutzung für Warteschlangen und Themen/Abonnements.
2. **Brokerverbindungen**: Definiert als die Höchstanzahl dauerhafter Verbindungen, die für Warteschlangen, Themen oder Abonnements während eines angegebenen einstündigen Stichprobenzeitraums geöffnet sind. Diese Verbrauchseinheit gilt nur im Tarif Standard, in dem Sie zusätzliche Verbindungen für eine nominale Gebühr pro Verbindung öffnen können (zuvor waren Verbindungen auf 100 Verbindungen pro Warteschlange/Thema/Abonnement begrenzt).

Mit dem Tarif **Standard** werden gestaffelte Preise für Vorgänge eingeführt, die mit Warteschlangen und Themen/Abonnements ausgeführt werden. Dies führt zu volumenbasierten Rabatten von bis zu 80 Prozent in den höchsten Nutzungsebenen. Außerdem wird eine Basisgebühr von 10 US-Dollar pro Monat für den Tarif „Standard“ erhoben, die es Ihnen ermöglicht, bis zu 12,5 Millionen Vorgänge pro Monat ohne zusätzliche Kosten auszuführen.

Der Tarif **Premium** bietet Ressourcenisolierung auf CPU- und Arbeitsspeicherebene, sodass die Workloads der einzelnen Kunden voneinander isoliert ausgeführt werden. Dieser Ressourcencontainer wird als *Messaging-Einheit* bezeichnet. Jedem Premium-Namespace wird mindestens eine Messaging-Einheit zugeordnet. Sie können 1, 2 oder 4 Messaging-Einheiten für jeden Service Bus Premium-Namespace erwerben. Eine einzelne Workload oder Entität kann mehrere Messaging-Einheiten umfassen, und die Anzahl der Einheiten kann beliebig geändert werden. Gebühren werden jedoch im 24-Stunden- bzw. Tagesrhythmus berechnet. Das Ergebnis ist eine vorhersehbare und wiederholbare Leistung Ihrer Service Bus-basierten Lösung. Diese Leistung ist nicht nur besser vorhersehbar und verfügbar, sondern auch schneller.

Beachten Sie, dass die Basisgebühr für den Standard-Tarif nur einmal pro Monat pro Azure-Abonnement berechnet wird. Dies bedeutet, dass Sie nach Erstellung eines einzigen Service Bus-Namespace im Standard-Tarif unter demselben Azure-Abonnement beliebig viele weitere Standardnamespaces erstellen können, ohne dass zusätzliche Grundgebühren anfallen.

In der Tabelle [Service Bus-Preise](https://azure.microsoft.com/pricing/details/service-bus/) sind die Funktionsunterschiede zwischen den Tarifen Standard und Premium zusammengefasst.

## <a name="messaging-operations"></a>Messagingvorgänge
Als Teil des neuen Preismodells ändert sich die Abrechnung für Warteschlangen und Themen/Abonnements. Diese Entitäten wechseln aus einer Abrechnung pro Nachricht zu einer Abrechnung pro Vorgang. Ein „Vorgang“ bezieht sich auf jeden API-Aufruf für einen Warteschlangen- oder Themen-/Abonnementdienstendpunkt. Dies umfasst Verwaltungs-, Sende-/Empfangs- und Sitzungsstatusvorgänge.

| Vorgangstyp | Beschreibung |
| --- | --- |
| Verwaltung |Erstellen, Lesen, Aktualisieren und Löschen (CRUD) von Warteschlangen oder Themen/Abonnements. |
| Nachrichten |Senden und Empfangen von Nachrichten mit Warteschlangen oder Themen/Abonnements. |
| Sitzungsstatus |Abrufen oder Festlegen des Sitzungsstatus für eine Warteschlange oder ein Thema/Abonnement. |

Details zu den Kosten finden Sie auf der Seite [Service Bus-Preise](https://azure.microsoft.com/pricing/details/service-bus/).

## <a name="brokered-connections"></a>Brokerverbindungen
*Brokerverbindungen* ermöglichen Nutzungsmuster von Kunden, die eine große Anzahl „dauerhaft verbundener“ Absender/Empfänger für Warteschlangen, Themen oder Abonnements umfassen. Dauerhaft verbundene Absender/Empfänger sind die Absender/Empfänger, die mithilfe von AMQP oder HTTP mit einem Empfangstimeout ungleich null eine Verbindung herstellen (z. B. langer HTTP-Abruf). HTTP-Absender und -Empfänger mit einem sofortigen Timeout generieren keine Brokerverbindungen.

Informationen zu Verbindungskontingenten und anderen Diensteinschränkungen finden Sie im Artikel [Service Bus-Kontingente](service-bus-quotas.md). Weitere Informationen zu Brokerverbindungen finden Sie weiter unten im Abschnitt [Häufig gestellte Fragen](#faq) weiter unten in diesem Artikel.

Im Tarif „Standard“ entfällt der Grenzwert für die Brokerverbindungen pro Namespace. Es zählt die aggregierte Verwendung von Brokerverbindungen im Azure-Abonnement. Weitere Informationen finden Sie in der Tabelle [Brokerverbindungen](https://azure.microsoft.com/pricing/details/service-bus/).

> [!NOTE]
> 1.000 Brokerverbindungen sind im Messagingtarif „Standard“ (über die Basisgebühr) enthalten und können für alle Warteschlangen, Themen und Abonnements im zugehörigen Azure-Abonnement gemeinsam verwendet werden.
>
>

<br />

> [!NOTE]
> Die Abrechnung erfolgt anhand der höchsten Anzahl gleichzeitiger Verbindungen und wird anteilsmäßig auf Basis von 744 Stunden pro Monat berechnet.
>
>

### <a name="premium-tier"></a>Tarif „Premium“

Brokerverbindungen werden im Tarif „Premium“ nicht in Rechnung gestellt.

## <a name="faq"></a>Häufig gestellte Fragen

### <a name="what-are-brokered-connections-and-how-do-i-get-charged-for-them"></a>Was sind Brokerverbindungen, und wie erfolgt ihre Abrechnung?
Eine Brokerverbindung ist als eine der folgenden Verbindungen definiert:

1. Eine AMQP-Verbindung von einem Client mit einer Service Bus-Warteschlange oder einem Service Bus-Thema bzw. -Abonnement.
2. Ein HTTP-Aufruf zum Empfangen einer Nachricht von einem Service Bus-Thema oder einer Service Bus-Warteschlange mit einem Empfangstimeout größer null.

Für Service Bus wird die höchste Anzahl gleichzeitiger Brokerverbindungen in Rechnung gestellt, die die enthaltene Menge (1.000 Verbindungen im Tarif Standard“) überschreiten. Spitzenwerte werden stündlich gemessen, anteilig (dividiert durch 744 Stunden pro Monat) ermittelt und für den monatlichen Abrechnungszeitraum addiert. Die enthaltene Menge (1.000 Brokerverbindungen pro Monat) wird am Ende des Abrechnungszeitraums auf die Summe der anteiligen stündlichen Spitzenwerte angewendet.

Beispiel:

1. Alle 10.000 Geräte stellen eine Verbindung über eine einzelne AMQP-Verbindung her und empfangen Befehle von einem Service Bus-Thema. Die Geräte senden Telemetrieereignisse an einen Event Hub. Wenn alle Geräte jeden Tag 12 Stunden verbunden sind, entstehen die folgenden Verbindungsgebühren (zusätzlich zu eventuellen weiteren Gebühren für das Service Bus-Thema): 10.000 Verbindungen * 12 Stunden * 31 Tage / 744 = 5.000 Brokerverbindungen. Nach Abzug der monatlich zulässigen Menge von 1.000 Brokerverbindungen werden Ihnen 4.000 Brokerverbindungen zu einem Tarif von 0,03 US-Dollar pro Brokerverbindung berechnet. Dies ergibt eine Summe von 120 US-Dollar.
2. 10.000 Geräte empfangen Nachrichten von einer Service Bus-Warteschlange über HTTP mit einem festgelegten Timeout ungleich null. Wenn alle Geräte 12 Stunden täglich verbunden sind, entstehen die folgenden Verbindungsgebühren (zusätzlich zu eventuellen weiteren Service Bus-Gebühren): 10.000 HTTP Empfangsverbindungen * 12 Stunden pro Tag * 31 Tage/744 Stunden = 5.000 Brokerverbindungen.

### <a name="do-brokered-connection-charges-apply-to-queues-and-topicssubscriptions"></a>Gelten für Warteschlangen und Themen/Abonnements Gebühren für Brokerverbindungen?
Ja. Es gibt keine Verbindungsgebühren für das Senden von Ereignissen über HTTP, unabhängig von der Anzahl der sendenden Systeme oder Geräte. Durch das Empfangen von Ereignissen mit HTTP mithilfe eines Timeouts größer als null (manchmal als "langer Abruf" bezeichnet) entstehen Gebühren für Brokerverbindungen. Durch AMQP-Verbindungen entstehen Gebühren für Brokerverbindungen, unabhängig davon, ob die Verbindungen zum Senden oder Empfangen verwendet werden. Die ersten 1.000 Brokerverbindungen in allen Standard-Namespaces in einem Azure-Abonnement sind ohne zusätzliche Kosten (über die Basisgebühr hinaus) enthalten. Da diese Kontingente für viele Dienst-zu-Dienst-Messaging-Szenarios ausreichend sind, werden Gebühren für Brokerverbindungen normalerweise nur dann relevant, wenn geplant ist, lange AMQP-oder HTTP-Abrufe mit einer großen Anzahl von Clients zu verwenden (z. B., um effizienteres Ereignisstreaming zu erreichen oder die bidirektionale Kommunikation mit zahlreichen Geräten oder Anwendungsinstanzen zu ermöglichen).

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zu Service Bus-Preisen finden Sie auf der Seite [Service Bus-Preise](https://azure.microsoft.com/pricing/details/service-bus/).
* In den [häufig gestellten Fragen zu Service Bus](service-bus-faq.md#pricing) werden einige allgemeine Fragen zur Preisgestaltung und Abrechnung für Service Bus beantwortet.

[Azure portal]: https://portal.azure.com

