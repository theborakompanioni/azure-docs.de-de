---
title: "Übersicht über die Azure Event Hubs Dedicated-Kapazität | Microsoft-Dokumentation"
description: "Übersicht über die Microsoft Azure Event Hubs Dedicated-Kapazität"
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/29/2017
ms.author: sethm;babanisa
ms.translationtype: HT
ms.sourcegitcommit: 1c730c65194e169121e3ad1d1423963ee3ced8da
ms.openlocfilehash: db8b119178de0e565b2064e9a52d5e9989d60d38
ms.contentlocale: de-de
ms.lasthandoff: 08/30/2017

---

# <a name="overview-of-event-hubs-dedicated"></a>Übersicht über Event Hubs Dedicated

Die *Event Hubs Dedicated*-Kapazität bietet Bereitstellungen mit einem Mandanten für Kunden mit äußerst anspruchsvollen Anforderungen. In vollem Umfang kann Azure Event Hubs über zwei Millionen Eingangsereignisse pro Sekunde bzw. bis zu 2 GB an Telemetriedaten pro Sekunde mit vollständig dauerhafter Speicherung und einer Wartezeit von unter einer Sekunde empfangen. Dies ermöglicht auch integrierte Lösungen, indem auf demselben System eine Echtzeit- und Batchverarbeitung erfolgt. Da [Event Hubs Capture](event-hubs-capture-overview.md) im Angebot enthalten ist,können Sie die Komplexität Ihrer Lösung mit einem einzelnen Datenstrom verringern, der echtzeit- und batchbasierte Pipelines unterstützt.

Die folgende Tabelle enthält einen Vergleich der verfügbaren Dienstebenen von Event Hubs. Für das Event Hubs Dedicated-Angebot gilt im Gegensatz zu den nutzungsbasierten Preisen für die meisten Standard-Funktionen ein fester Monatspreis. Der Dedicated-Tarif umfasst die Funktionen des Standardplans, jedoch mit Kapazität auf Unternehmensniveau für Kunden mit anspruchsvollen Workloads. 

| Feature | Standard | Dediziert |
| --- |:---:|:---:|:---:|
| Eingangsereignisse | Bezahlung pro Million Ereignisse | Enthalten |
| Durchsatzeinheit (1 MB/s eingehend, 2 MB/s ausgehend) | Bezahlung pro Stunde | Enthalten |
| Nachrichtengröße | 256 KB | 1 MB |
| Herausgeberrichtlinien | Ja | Ja |   
| Verbrauchergruppen | 20 | 20 |
| Nachrichtenwiederholung | Ja | Ja |
| Maximale Durchsatzeinheiten | 20 (flexibel bis 100)   | 1 CU≈200 |
| Brokerverbindungen | 1.000 enthalten | 100.000 enthalten |
| Zusätzliche Brokerverbindungen | Ja | Ja |
| Nachrichtenaufbewahrung | 1 Tag enthalten | Bis zu 7 Tage enthalten |
| Erfassen | Bezahlung pro Stunde | Enthalten |

## <a name="benefits-of-event-hubs-dedicated-capacity"></a>Vorteile der Event Hubs Dedicated-Kapazität

Die Verwendung der Event Hubs Dedicated-Kapazität bietet die folgenden Vorteile:

* Hosting eines einzelnen Mandanten ohne Störungen durch andere Mandanten
* Erweiterung der Nachrichtengröße auf 1 MB im Vergleich zu 256 KB für Standard
* Jedes Mal wiederholbare Leistung
* Garantierte Kapazität, die Ihre Burstanforderungen erfüllt
* Skalierbar zwischen 1 und 8 Kapazitäteinheiten (Capacity Units, CU) – Bereitstellung von bis zu 2 Millionen Eingangsereignissen pro Sekunde
  * Kapazitätseinheiten verwalten die Staffelung für Event Hubs Dedicated, wobei jede Kapazitätseinheiten ungefähr die Entsprechung von 200 Durchsatzeinheiten (Throughput Units, TU) bereitstellen kann.
* Keine Wartung: Wir verwalten den Lastenausgleich, Betriebssystemupdates, Sicherheitspatches und Partitionierung.
* Monatlicher Festpreis

Mit Event Hubs Dedicated entfallen auch einige der Durchsatzeinschränkungen des Standard-Angebots. Durchsatzeinheiten im Standard-Tarif berechtigen Sie zu 1.000 Ereignissen pro Sekunde oder 1 MB pro Sekunde an eingehendem Datenverkehr pro TU und die zweifache Menge an ausgehendem Datenverkehr. Das Dedicated-Staffelungsangebot weist keine Einschränkungen der Anzahl von ein- und ausgehenden Ereignissen auf. Diese Grenzwerte werden nur durch die Verarbeitungskapazität der erworbenen Event Hubs geregelt.

Dieser Dienst ist auf große Telemetriebenutzer ausgerichtet und steht Kunden mit einem Enterprise Agreement zur Verfügung.

## <a name="how-to-onboard"></a>Onboardingmethoden

Die Event Hubs Dedicated-Plattform ist für über ein Enterprise Agreement mit CUs unterschiedlicher Größe verfügbar. Jede CU stellt ungefähr die Entsprechung von 200 Durchsatzeinheiten bereit. Sie können Ihre Kapazität während des gesamten Monats nach oben oder unten skalieren, um Ihre Anforderungen durch Hinzufügen oder Entfernen von CUs zu erfüllen. Der Dedicated-Plan ist einzigartig, da Sie ein äußerst praxisorientiertes Onboarding vom Event Hubs-Produktteam erhalten, um genau die richtige flexible Bereitstellung für Sie zu ermitteln. 

## <a name="next-steps"></a>Nächste Schritte
Wenden Sie sich an Ihren Microsoft-Vertriebsmitarbeiter oder den Microsoft-Support, um weitere Details zur Dedicated Event Hubs-Kapazität zu erhalten. Weitere Informationen zu Event Hubs finden Sie auch unter den folgenden Links:

Informationen zu Preisen erhalten Sie unter den folgenden Links:

- [Event Hubs Preise](https://azure.microsoft.com/pricing/details/event-hubs/) Wenden Sie sich an Ihren Microsoft-Vertriebsmitarbeiter oder den Microsoft-Support, um weitere Details zur Dedicated Event Hubs-Kapazität zu erhalten.
- Unter [Event Hubs – häufig gestellte Fragen](event-hubs-faq.md) finden Sie Preisinformationen und Antworten auf einige häufig gestellte Fragen zu Event Hubs. 

