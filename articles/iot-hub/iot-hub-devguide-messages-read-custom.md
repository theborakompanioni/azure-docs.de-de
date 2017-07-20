---
title: Grundlegendes zu benutzerdefinierten Azure IoT Hub-Endpunkten | Microsoft-Dokumentation
description: 'Entwicklerhandbuch: Verwenden von Routingregeln zum Weiterleiten von D2C-Nachrichten an benutzerdefinierte Endpunkte.'
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/25/2017
ms.author: dobett
ms.translationtype: Human Translation
ms.sourcegitcommit: 5edc47e03ca9319ba2e3285600703d759963e1f3
ms.openlocfilehash: a21f1c61f344f96e2e03422e41fd8c5f7f841a0c
ms.contentlocale: de-de
ms.lasthandoff: 05/31/2017


---
# <a name="use-message-routes-and-custom-endpoints-for-device-to-cloud-messages"></a>Verwenden von Nachrichtenrouten und benutzerdefinierten Endpunkten für D2C-Nachrichten

Mit IoT Hub können Sie [D2C-Nachrichten][lnk-device-to-cloud] basierend auf den Nachrichteneigenschaften an IoT Hub-Endpunkte weiterleiten. Mit Routingregeln können Sie Nachrichten flexibel an den Bestimmungsort senden, ohne zusätzliche Dienste zum Verarbeiten von Nachrichten oder Schreiben von weiterem Code bereitstellen zu müssen. Jede Routingregel, die Sie konfigurieren, hat die folgenden Eigenschaften:

| Eigenschaft      | Beschreibung |
| ------------- | ----------- |
| **Name**      | Der eindeutige Name, mit dem die Regel identifiziert wird. |
| **Quelle**    | Der Ursprung des zu verarbeitenden Datenstroms. Beispiel: Gerätetelemetrie. |
| **Condition** | Der Abfrageausdruck für die Routingregel, die für den Header und den Text der Nachricht ausgeführt wird, um zu ermitteln, ob sich für den Endpunkt eine Übereinstimmung ergibt. Weitere Informationen zur Erstellung einer Routenbedingung finden Sie unter [Referenz – Abfragesprache für Zwillinge und Aufträge][lnk-devguide-query-language]. |
| **Endpunkt**  | Der Name des Endpunkts, an den vom IoT Hub diejenigen Nachrichten gesendet werden, für die sich eine Übereinstimmung ergeben hat. Es ist ratsam, dass sich Endpunkte in derselben Region wie der IoT Hub befinden, da sonst ggf. Kosten für regionsübergreifende Schreibvorgänge anfallen. |

Es kann vorkommen, dass sich für eine einzelne Nachricht Übereinstimmungen mit den Bedingungen mehrerer Routingregeln ergeben. In diesem Fall sendet der IoT Hub die Nachricht jeweils an alle Endpunkte, die den entsprechenden Regeln zugeordnet sind. Falls für den IoT Hub bei der Nachrichtenzustellung auch die automatische Deduplizierung verwendet wird, wird eine Nachricht nur einmal auf das Ziel geschrieben, wenn diese für mehrere Regeln mit demselben Ziel eine Übereinstimmung ergibt.

Eine IoT Hub-Instanz verfügt standardmäßig über einen [integrierten Endpunkt][lnk-built-in]. Sie können benutzerdefinierte Endpunkte für das Routing von Nachrichten erstellen, indem Sie andere Dienste Ihres Abonnements mit dem Hub verknüpfen. IoT Hub unterstützt derzeit Event Hubs, Service Bus-Warteschlangen und Service Bus-Themen als benutzerdefinierte Endpunkte.

> [!WARNING]
> Service Bus-Warteschlangen und -Themen, bei denen **Sitzungen** oder **Duplikaterkennung** aktiviert wurden, werden nicht als benutzerdefinierte Endpunkte unterstützt.

Weitere Informationen zur Erstellung von benutzerdefinierten Endpunkten auf dem IoT Hub finden Sie unter [IoT Hub-Endpunkte][lnk-devguide-endpoints].

Weitere Informationen zum Lesen von benutzerdefinierten Endpunkten finden Sie unter:

* Lesen aus [Event Hubs][lnk-getstarted-eh]
* Lesen aus [Service Bus-Warteschlangen][lnk-getstarted-queue]
* Lesen aus [Service Bus-Themen][lnk-getstarted-topic]

### <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu IoT Hub-Endpunkten finden Sie unter [IoT Hub-Endpunkte][lnk-devguide-endpoints].

Weitere Informationen zur Abfragesprache, mit der Sie Routingregeln definieren, finden Sie unter [IoT Hub-Abfragesprache für Gerätezwillinge, Aufträge und Nachrichtenrouting][lnk-devguide-query-language].

Im Tutorial [Verarbeiten von IoT Hub-D2C-Nachrichten mithilfe von Routen][lnk-d2c-tutorial] erfahren Sie, wie Sie Routingregeln und benutzerdefinierte Endpunkte verwenden.

[lnk-built-in]: iot-hub-devguide-messages-read-builtin.md
[lnk-device-to-cloud]: iot-hub-devguide-messages-d2c.md
[lnk-devguide-query-language]: iot-hub-devguide-query-language.md
[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
[lnk-getstarted-eh]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-getstarted-queue]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md
[lnk-getstarted-topic]: ../service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions.md

