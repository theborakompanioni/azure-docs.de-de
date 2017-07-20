---
title: Grundlegendes zum Azure IoT Hub-Nachrichtenformat | Microsoft-Dokumentation
description: 'Entwicklerhandbuch: Beschreibung des Formats und des erwarteten Inhalts von IoT Hub-Nachrichten.'
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 3fc5f1a3-3711-4611-9897-d4db079b4250
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/25/2017
ms.author: dobett
ms.translationtype: Human Translation
ms.sourcegitcommit: 5edc47e03ca9319ba2e3285600703d759963e1f3
ms.openlocfilehash: be467cf7d3ada41f110f4f6aea686b3d40e01ffa
ms.contentlocale: de-de
ms.lasthandoff: 05/31/2017


---
# <a name="create-and-read-iot-hub-messages"></a>Erstellen und Lesen von IoT Hub-Nachrichten

Um eine nahtlose Interoperabilität zwischen Protokollen zu gewährleisten, definiert IoT Hub ein gemeinsames Nachrichtenformat für alle geräteseitigen Protokolle. Dieses Nachrichtenformat wird sowohl für [D2C][lnk-d2c]- als auch für [C2D][lnk-c2d]-Nachrichten verwendet. Eine [IoT Hub-Nachricht][lnk-messaging] enthält Folgendes:

* Einen Satz an *Systemeigenschaften*. Eigenschaften, die von IoT Hub interpretiert oder festgelegt werden. Dieser Satz ist vordefiniert.
* Einen Satz an *Anwendungseigenschaften*. Ein Wörterbuch mit Zeichenfolgeneigenschaften. Die Anwendung kann diese definieren und darauf zugreifen, ohne den Nachrichtentext deserialisieren zu müssen. IoT Hub ändert diese Eigenschaften nie.
* Ein nicht lesbarer binärer Textkörper.

Eigenschaftennamen und Eigenschaftswerte dürfen nur alphanumerische ASCII-Zeichen sowie die Zeichen ``{'!', '#', '$', '%, '&', "'", '*', '*', '+', '-', '.', '^', '_', '`', '|', '~'}`` enthalten. Dies gilt für folgende Situationen:

* Senden von D2C-Nachrichten über HTTP
* Senden von Cloud-an-Gerät-Nachrichten.

Weitere Informationen zum Codieren und Decodieren von Nachrichten, die über verschiedene Protokollen gesendet werden, finden Sie unter [Azure IoT-SDKs][lnk-sdks].

In der folgenden Tabelle werden die Systemeigenschaften in IoT Hub-Nachrichten aufgeführt.

| Eigenschaft | Beschreibung |
| --- | --- |
| MessageId |Eine vom Benutzer festgelegte Kennung für die Nachricht; wird für Anforderung-Antwort-Muster verwendet. Format: Eine Zeichenfolge mit Berücksichtigung von Klein-/Großschreibung (bis zu 128 Zeichen lang), die aus alphanumerischen ASCII-Zeichen (7 Bit) + `{'-', ':',’.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}`besteht. |
| Sequenznummer |Eine Nummer (für jede Gerätewarteschlange eindeutig), die jeder C2D-Nachricht von IoT Hub zugewiesen wird |
| To |Ein Ziel, das in [C2D][lnk-c2d]-Nachrichten angegeben wird. |
| ExpiryTimeUtc |Datum und Uhrzeit des Nachrichtenablaufs. |
| EnqueuedTime |Datum und Uhrzeit des Empfangs der [C2D][lnk-c2d]-Nachricht durch IoT Hub. |
| CorrelationId |Eine Zeichenfolgeneigenschaft in einer Antwortnachricht, die normalerweise die Nachrichten-ID der Anforderung im Anforderung-Antwort-Muster enthält. |
| UserId |Eine ID zum Festlegen des Ursprungs von Nachrichten. Wenn IoT Hub Nachrichten generiert, wird diese Eigenschaft auf `{iot hub name}`festgelegt. |
| Ack |Ein Feedbacknachrichtengenerator. Diese Eigenschaft wird in C2D-Nachrichten verwendet, um IoT Hub anzuweisen, als Ergebnis der Nachrichtenverarbeitung durch das Gerät Feedbacknachrichten zu generieren. Mögliche Werte: **Kein** (Standardeinstellung): Es wird keine Feedbacknachricht generiert. **Positiv**: Es wird eine Feedbacknachricht empfangen, wenn die Nachricht abgeschlossen wurde. **Negativ**: Es wird eine Feedbacknachricht empfangen, wenn die Nachricht ohne vollständige Verarbeitung durch das Gerät abgelaufen ist (oder die maximale Anzahl von Zustellversuchen erreicht wurde). **Voll**: Feedback wird sowohl bei erfolgreicher als auch nicht erfolgreicher Nachrichtenverarbeitung generiert. Weitere Informationen finden Sie unter [Nachrichtenfeedback][lnk-feedback]. |
| ConnectionDeviceId |Eine ID, die von IoT Hub für D2C-Nachrichten festgelegt wird. Diese Eigenschaft enthält die **deviceId** des Geräts, das die Nachricht sendet. |
| ConnectionDeviceGenerationId |Eine ID, die von IoT Hub für D2C-Nachrichten festgelegt wird. Diese Eigenschaft enthält die **generationId** (gemäß [Geräteidentitätseigenschaften][lnk-device-properties]) des Geräts, das die Nachricht gesendet hat. |
| ConnectionAuthMethod |Eine von IoT Hub für D2C-Nachrichten festgelegte Authentifizierungsmethode. Diese Eigenschaft enthält Informationen zu der Methode, die zum Authentifizieren des Geräts verwendet wird, das die Nachricht sendet. Weitere Informationen finden Sie unter [D2C-Anti-Spoofing][lnk-antispoofing]. |

## <a name="message-size"></a>Nachrichtengröße

IoT Hub misst die Nachrichtengröße auf „protokollagnostische“ Weise, indem nur die tatsächliche Nutzlast berücksichtigt wird. Die Summe für die Größe in Byte wird wie folgt berechnet:

* Text in Byte
* Größe aller Werte der Nachrichtensystemeigenschaften in Byte
* Größe aller Benutzereigenschaftsnamen und -werte in Byte

Die Eigenschaftennamen und -werte sind auf ASCII-Zeichen beschränkt, sodass die Länge der Zeichenfolgen der Größe in Byte entspricht.

## <a name="next-steps"></a>Nächste Schritte

Informationen zu Größenbeschränkungen für Nachricht in IoT Hub finden Sie unter [IoT Hub-Kontingente und -Drosselung][lnk-quotas].

Informationen zum Erstellen und Lesen von IoT Hub-Nachrichten in verschiedenen Programmiersprachen finden Sie in den Tutorials mit [Ersten Schritten][lnk-get-started].

[lnk-messaging]: iot-hub-devguide-messaging.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-get-started]: iot-hub-get-started.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-c2d]: iot-hub-devguide-messages-c2d.md
[lnk-d2c]: iot-hub-devguide-messages-d2c.md
[[lnk-feedback]: iot-hub-devguide-messages-c2d.md#message-feedback [lnk-device-properties]: iot-hub-devguide-identity-registry.md#device-identity-properties [lnk-antispoofing]: iot-hub-devguide-messages-d2c.md#anti-spoofing-propertiess
