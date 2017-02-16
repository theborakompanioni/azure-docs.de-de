---
title: "Übersicht über Azure Event Hubs Dedicated | Microsoft-Dokumentation"
description: Microsoft Azure Event Hubs
services: event-hubs
author: banisadr
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: babanisa
translationtype: Human Translation
ms.sourcegitcommit: 6aebaeccc502d20c8a80297972b8f3b5e8e363c6
ms.openlocfilehash: 3cd3d3fa7a540f9608ac07172bb78ee719d6b0a6


---


# <a name="dedicated-event-hubs--an-overview"></a>Dedicated Event Hubs – Übersicht

Die Event Hubs Dedicated-Kapazität bietet Bereitstellungen mit einem Mandanten für Kunden mit den anspruchsvollsten Anforderungen. In vollem Umfang kann Azure Event Hubs über zwei Millionen Eingangsereignisse pro Sekunde bzw. bis zu 2 GB an Telemetriedaten pro Sekunde mit vollständig dauerhafter Speicherung und einer Latenz von unter einer Sekunde empfangen. Dies ermöglicht auch integrierte Lösungen, indem auf demselben System eine Echtzeit- und Batchverarbeitung erfolgt. Da Event Hubs Archive im Angebot enthalten ist, kann ein einzelner Stream batch- und echtzeitbasierte Pipelines unterstützen, wodurch die Komplexität Ihrer Lösung reduziert wird.

Die folgende Tabelle enthält einen Vergleich der verfügbaren Dienstebenen von Event Hubs. Für das Event Hubs Dedicated-Angebot gilt im Gegensatz zu den nutzungsbasierten Preisen für die meisten Standard- und Basic-Funktionen ein fester Monatspreis. Der Dedicated-Tarif umfasst die Funktionen des Standardplans, jedoch mit Kapazität auf Unternehmensniveau für Kunden mit anspruchsvollen Workloads.

|  | Basic | Standard | Dediziert |
| --- |:---:|:---:|:---:|
| Eingangsereignisse | Bezahlung pro Million Ereignisse | Bezahlung pro Million Ereignisse | Enthalten |
| Durchsatzeinheit (1 MB/s eingehend, 2 MB/s ausgehend) | Bezahlung pro Stunde | Bezahlung pro Stunde | Enthalten |
| Nachrichtengröße | 256 KB | 256 KB | 1 MB |
| Herausgeberrichtlinien | – | ✓ | ✓ |     
| Verbrauchergruppen | 1 – Standardwert | 20 | 20 |
| Nachrichtenwiederholung | ✓ | ✓  | ✓ |
| Maximale Durchsatzeinheiten | 20 | 20 (flexibel bis 100)  | 1 CU≈200 |
| Brokerverbindungen | 100 enthalten | 1.000 enthalten | 100.000 enthalten |
| Zusätzliche Brokerverbindungen | N/V | ✓ | ✓ |
| Nachrichtenaufbewahrung | 1 Tag enthalten | 1 Tag enthalten | Bis zu 7 Tage enthalten |
| Archiv <sup>Vorschau</sup> | –  | Bezahlung pro Stunde | Enthalten |

## <a name="benefits-of-event-hubs-at-dedicated-capacity-include"></a>Vorteile von Event Hubs mit Dedicated-Kapazität:

* Hosting eines einzelnen Mandanten ohne Störungen durch andere Mandanten
* Erweiterung der Nachrichtengröße auf 1 MB im Vergleich zu 256 KB für Standard und Basic
* Jedes Mal wiederholbare Leistung
* Garantierte Kapazität, die Ihre Burstanforderungen erfüllt
* Skalierbar zwischen 1 und 8 Kapazitäteinheiten (Capacity Units, CU) – Bereitstellung von bis zu 2 Millionen Eingangsereignissen pro Sekunde
  * Kapazitätseinheiten (CU) verwalten die Staffelung für Dedicated Event Hubs, wobei jede CU ungefähr die Entsprechung von 200 Durchsatzeinheiten (Throughput Units, TU) bereitstellen kann.
* Keine Wartung – wir verwalten den Lastenausgleich, Betriebssystemupdates, Sicherheitspatches und Partitionierung
* Monatlicher Festpreis

Mit Dedicated Event Hubs entfallen auch einige der Durchsatzeinschränkungen des Standard-Angebots. Durchsatzeinheiten (TU) in den Basic- und Standard-Tarifen berechtigen den Kunden zu 1.000 Ereignissen pro Sekunde oder 1 Mbit/s an eingehendem Datenverkehr pro TU und die zweifache Menge an ausgehendem Datenverkehr. Das Dedicated-Staffelungsangebot weist keine Einschränkungen der Anzahl von ein- und ausgehenden Ereignissen auf. Diese Grenzwerte werden nur durch die Verarbeitungskapazität der erworbenen Dedicated Event Hubs geregelt.

Dieser Dienst ist auf die größten Telemetriebenutzer ausgerichtet und ist für Kunden mit einem Enterprise Agreement verfügbar.

## <a name="how-to-onboard"></a>Onboardingmethoden

Die Dedicated Event Hubs-Plattform wird für die Öffentlichkeit über ein Enterprise Agreement mit Kapazitätsinheiten (CU) unterschiedlicher Größe angeboten. Jede CU stellt ungefähr die Entsprechung von 200 Durchsatzeinheiten bereit und wird mit 31 $/Std. berechnet. Sie können Ihre Kapazität während des gesamten Monats nach oben oder unten skalieren, um Ihre Anforderungen durch Hinzufügen oder Entfernen von CUs zu erfüllen. Der dedizierte Plan ist einzigartig, da Sie ein äußerst praxisorientiertes Onboarding vom Event Hubs-Produktteam erhalten, um genau die richtige flexible Bereitstellung für Sie zu ermitteln. 


## <a name="next-steps"></a>Nächste Schritte

[Dedicated Event Hubs – Preise](https://azure.microsoft.com/en-us/pricing/details/event-hubs/)

Wenden Sie sich an Ihren Microsoft-Vertriebsmitarbeiter oder den Microsoft-Support, um weitere Details zur Dedicated Event Hubs-Kapazität zu erhalten.


<!--HONumber=Jan17_HO4-->


