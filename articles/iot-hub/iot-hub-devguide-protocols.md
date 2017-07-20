---
title: Azure IoT Hub-Kommunikationsprotokolle und -Ports | Microsoft-Dokumentation
description: "Entwicklerhandbuch: Beschreibung der unterstützten Kommunikationsprotokolle für D2C- und C2D-Kommunikationen sowie der Portnummern, die geöffnet sein müssen."
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
ms.openlocfilehash: 98004a48734e33f85eebf8f6213d9f0751dea843
ms.contentlocale: de-de
ms.lasthandoff: 05/31/2017


---
# <a name="reference---choose-a-communication-protocol"></a>Referenz – Auswählen eines Kommunikationsprotokolls

IoT Hub erlaubt Geräten das Verwenden der Protokolle [MQTT][lnk-mqtt], MQTT über WebSockets, [AMQP][lnk-amqp], AMQP über WebSockets und HTTP für die geräteseitige Kommunikation. Informationen dazu, wie diese Protokolle bestimmte IoT Hub-Features unterstützen, finden Sie im [Leitfaden zur D2C-Kommunikation][lnk-d2c-guidance] und im [Leitfaden zur C2D-Kommunikation][lnk-c2d-guidance].

Die folgende Tabelle enthält allgemeine Ratschläge für Ihre Protokollauswahl:

| Protocol | Wann Sie dieses Protokoll auswählen sollten |
| --- | --- |
| MQTT <br> MQTT über WebSockets |Verwenden Sie dieses Protokoll auf allen Geräten, die sich nicht über dieselbe TLS-Verbindung mit mehreren Geräten verbinden müssen (von denen jedes eigene gerätebezogene Anmeldeinformationen hat). |
| AMQP <br> AMQP über WebSockets |Verwenden Sie es für Feld- und Cloudgateways, um die Vorteile geräteübergreifender Multiplexingverbindungen zu nutzen. |
| HTTP |Verwenden Sie es für Geräte, die keine anderen Protokolle unterstützen können. |

Beachten Sie bei der Auswahl des Protokolls für die geräteseitige Kommunikation folgende Punkte:

* **C2D-Muster**. HTTP bietet keine effiziente Methode zum Implementieren von Serverpushvorgängen. Daher fragen Geräte bei Verwendung von HTTP IoT Hub nach C2D-Nachrichten ab. Dieser Ansatz ist für das Gerät und auch für IoT Hub sehr ineffizient. Gemäß den aktuellen HTTP-Richtlinien muss jedes Gerät mindestens alle 25 Minuten eine Abfrage nach Nachrichten durchführen. Auf der anderen Seite unterstützen MQTT und AMQP Serverpush beim Empfangen von C2D-Nachrichten. Sie ermöglichen sofortiges Nachrichtenpushen von IoT Hub zum Gerät. Wenn die Übermittlungslatenz eine wichtige Rolle spielt, sind MQTT oder AMQP die zu bevorzugenden Protokolle. Bei nur selten verbundenen Geräten funktioniert auch HTTP.
* **Bereichsgateways**. Bei Verwendung von MQTT und HTTP ist es nicht möglich, mehrere Geräte (jedes mit eigenen gerätebezogenen Anmeldeinformationen) mithilfe der gleichen TLS-Verbindung zu verbinden. Darum sind diese Protokolle für [Bereichsgatewayszenarien][lnk-azure-gateway-guidance] nicht optimal, da sie eine TLS-Verbindung zwischen dem Bereichsgateway und IoT Hub für jedes Gerät benötigen, das mit dem Bereichsgateway verbunden ist.
* **Geräte mit eingeschränkten Ressourcen**. Die MQTT- und HTTP-Bibliotheken haben weniger Speicherbedarf als die AMQP-Bibliotheken. Wenn daher das Gerät über limitierte Ressourcen verfügt (beispielsweise weniger als 1MB RAM), stehen möglicherweise nur diese Protokolle als Protokollimplementierung zur Verfügung.
* **Netzwerkausnahme**. Das Standardprotokoll AMQP verwendet Port 5671, während MQTT standardmäßig an Port 8883 lauscht. Dies kann Probleme in Netzwerken verursachen, die für Nicht-HTTP-Protokolle geschlossen sind. MQTT über WebSockets, AMQP über WebSockets und HTTP können in diesem Szenario verwendet werden.
* **Größe der Nutzlast**. MQTT und AMQP sind binäre Protokolle, was zu kompakteren Nutzlasten als bei HTTP führt.

> [!WARNING]
> Bei Verwendung von HTTP muss jedes Gerät mindestens alle 25 Minuten eine Abfrage nach C2D-Nachrichten durchführen. In der Entwicklungsphase darf freilich häufiger als alle 25 Minuten eine Abfrage erfolgen.

## <a name="port-numbers"></a>Portnummern

Geräte können mit IoT Hub in Azure über verschiedene Protokolle kommunizieren. In der Regel richtet sich die Wahl des Protokolls nach den spezifischen Anforderungen der Lösung. Die folgende Tabelle enthält die ausgehenden Ports, die geöffnet sein müssen, damit ein Gerät ein bestimmtes Protokoll verwenden kann:

| Protocol | Port |
| --- | --- |
| MQTT |8883 |
| MQTT über WebSockets |443 |
| AMQP |5671 |
| AMQP über WebSockets |443 |
| HTTP |443 |

Nachdem Sie einen IoT Hub in einer Azure-Region erstellt haben, behält er seine IP-Adresse für die gesamte Lebensdauer bei. Wenn der IoT Hub von Microsoft jedoch in eine andere Skalierungseinheit verschoben wird, wird ihm eine neue IP-Adresse zugewiesen, um die Dienstqualität zu gewährleisten.


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen dazu, wie IoT Hub das MQTT-Protokoll implementiert, finden Sie unter [Kommunikation mit Ihrem IoT Hub mithilfe des Protokolls MQTT][lnk-mqtt-support].

[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-mqtt-support]: iot-hub-mqtt-support.md
[lnk-amqp]: http://docs.oasis-open.org/amqp/core/v1.0/os/amqp-core-complete-v1.0-os.pdf
[lnk-mqtt]: http://docs.oasis-open.org/mqtt/mqtt/v3.1.1/mqtt-v3.1.1.pdf
[lnk-azure-gateway-guidance]: iot-hub-devguide-endpoints.md#field-gateways

