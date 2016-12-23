---
title: "Entwicklerhandbuch – Preisbeispiel | Microsoft-Dokumentation"
description: "Entwicklungsleitfaden für Azure IoT Hub: Informationen und Links zu verschiedenen Geräte- und Dienst-SDKs für Azure IoT."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 1ac90923-1edf-4134-bbd4-77fee9b68d24
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/30/2016
ms.author: elioda
translationtype: Human Translation
ms.sourcegitcommit: c18a1b16cb561edabd69f17ecebedf686732ac34
ms.openlocfilehash: 8b025872acd715878dab82e3fa02f0367f71420d


---

# <a name="pricing-information"></a>Preisinformationen

[Azure IoT Hub – Preise][lnk-pricing] enthält die allgemeinen Informationen zu den verschiedenen SKUs und die Preise für IoT Hub. Dieser Artikel enthält weitere Informationen dazu, wie die verschiedenen IoT Hub-Funktionen als Nachrichten von IoT Hub getaktet werden.

## <a name="charges-per-operation"></a>Gebühren pro Vorgang

| Vorgang | Abrechnungsinformationen | 
| --------- | ------------------- |
| Identitätsregistrierungsvorgänge <br/> (Erstellen, Abrufen, Auflisten, Aktualisieren, Löschen) | Keine Gebühren. |
| D2C-Nachrichten | Erfolgreich gesendete Nachrichten werden in Blöcken von 4 KB berechnet, sodass eine Nachricht mit 6 KB beispielsweise wie zwei Nachrichten berechnet wird. |
| C2D-Nachrichten | Erfolgreich gesendete Nachrichten werden in Blöcken von 4 KB berechnet, sodass eine Nachricht mit 6 KB beispielsweise wie zwei Nachrichten berechnet wird. |
| Dateiuploads | Die Dateiübertragung an Azure Storage wird von IoT Hub nicht getaktet. Nachrichten für die Initiierung und den Abschluss der Dateiübertragung werden als Nachrichten mit 4-KB-Taktung berechnet. Für die Übertragung einer Datei mit 10 MB werden zusätzlich zu den Azure Storage-Kosten beispielsweise zwei Nachrichten berechnet. |
| Direkte Methoden | Erfolgreiche Methodenanforderungen werden in Blöcken von 4 KB berechnet, und Antworten mit nicht leerem Textbereich werden als zusätzliche Nachrichten in Blöcken von 4 KB berechnet. Anforderungen an nicht verbundene Geräte werden als Nachrichten in Blöcken von 4 KB berechnet. Eine Methode mit einem Text von 6 KB, die zu einer Antwort des Geräts ohne Text führt, wird als zwei Nachrichten berechnet. Eine Methode mit einem Text von 6 KB, die zu einer Antwort des Geräts mit 1 KB führt, wird als zwei Nachrichten für die Anforderung und eine weitere Nachricht für die Antwort berechnet. |
| Gerätezwilling-Lesevorgänge | Gerätezwilling-Lesevorgänge vom Gerät und vom Back-End werden als Nachrichten in Blöcken von 512 Byte berechnet. Das Lesen von einem 6-KB-Gerätezwilling wird beispielsweise wie zwölf Nachrichten berechnet. |
| Gerätezwillingsaktualisierungen (Tags und Eigenschaften) | Gerätezwillingsaktualisierungen vom Gerät sowie das Gerät werden als Nachrichten in Blöcken von 512 Byte berechnet. Das Lesen von einem 6-KB-Gerätezwilling wird beispielsweise wie zwölf Nachrichten berechnet. |
| Gerätezwillingsabfragen | Abfragen werden als Nachrichten je nach Ergebnisgröße in Blöcken von 512 Byte berechnet. |
| Auftragsvorgänge <br/> (Erstellen, Aktualisieren, Auflisten, Löschen) | Keine Gebühren. |
| Vorgänge vom Typ „Aufträge pro Gerät“ | Auftragsvorgänge (z.B. Gerätezwillingsaktualisierungen und Methoden) werden normal berechnet. Ein Auftrag, der zu 1.000 Methodenaufrufen mit 1-KB-Anforderungen und Antworten mit leerem Textbereich führt, wird als 1.000 Nachrichten berechnet. |

> AZURE.NOTE Bei allen Berechnungen der Größen wird die Nutzlastgröße in Byte berücksichtigt (Protokollframing wird ignoriert). Bei Nachrichten (mit Eigenschaften und Text) wird die Größe auf protokollagnostische Weise berechnet, wie im [Entwicklerleitfaden zum IoT Hub-Messaging][lnk-message-size] beschrieben.

## <a name="example-1--"></a>Beispiel 1: 

Ein Gerät sendet eine 1-KB-D2C-Nachricht pro Minute an IoT Hub, die dann von Azure Stream Analytics gelesen wird. Das Back-End ruft alle zehn Minuten eine Methode (mit Nutzlast von 512 Byte) auf dem Gerät auf, um eine bestimmte Aktion auszulösen. Das Gerät antwortet auf die Methode mit einem Ergebnis von 200 Byte.

Das Gerät verbraucht 1 Nachricht · 60 Minuten · 24 Stunden = 1440 Nachrichten pro Tag für die D2C-Nachrichten und 2 Anforderungen plus Antwort · 6-mal pro Stunde · 24 Stunden = 288 Nachrichten für die Methoden. Dies ergibt eine Gesamtsumme von 1.728 Nachrichten pro Tag.

## <a name="example-2"></a>Beispiel 2:

Ein Gerät sendet jede Stunde eine 100-KB-D2C-Nachricht. Außerdem aktualisiert es alle vier Stunden seinen Gerätezwilling mit 1-KB-Nutzlasten. Das Back-End liest einmal pro Tag den 14-KB-Gerätezwilling und aktualisiert ihn mit 512-Byte-Nutzlasten, um die Konfigurationen zu ändern.

Das Gerät verbraucht 25 (100 KB/4KB) Nachrichten · 24 Stunden für D2C-Nachrichten sowie 1 Nachricht · 6-mal pro Tag für Gerätezwillingsaktualisierungen. Dies ergibt eine Gesamtsumme von 156 Nachrichten pro Tag.
Das Back-End verbraucht 28 Nachrichten (14 KB/0,5 KB) für das Lesen vom Gerätezwilling sowie 1 Nachricht für die Aktualisierung. Dies ergibt insgesamt 29 Nachrichten.

Zusammen verbrauchen das Gerät und das Back-End 185 Nachrichten pro Tag.


[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub
[lnk-message-size]: iot-hub-devguide-messaging.md#message-size


<!--HONumber=Nov16_HO5-->


