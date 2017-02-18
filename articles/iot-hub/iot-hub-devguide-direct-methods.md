---
title: Informationen zu direkten Azure IoT Hub-Methoden | Microsoft Docs
description: "Entwicklerhandbuch – Verwenden von direkten Methoden zum Aufrufen von Code auf Ihren Geräten von einer Service-App"
services: iot-hub
documentationcenter: .net
author: nberdy
manager: timlt
editor: 
ms.assetid: 9f0535f1-02e6-467a-9fc4-c0950702102d
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/11/2017
ms.author: nberdy
translationtype: Human Translation
ms.sourcegitcommit: 9c2817129162ab17faadf3c5ecf8ef7dcb370c3c
ms.openlocfilehash: 2c9c4b59077ea7d31677a5e1c690160bf63633a6


---
# <a name="direct-methods"></a>Direkte Methoden
## <a name="overview"></a>Übersicht
IoT Hub gibt Ihnen die Möglichkeit, direkte Methoden auf Geräten von der Cloud aus aufzurufen. Direkte Methoden stellen eine Anforderung-Antwort-Interaktion mit einem Gerät dar, die einem HTTP-Aufruf darin ähnelt, dass sie unverzüglich (nach einem vom Benutzer angegebenen Timeout) zu einem Erfolg oder Fehler führt. Dies ist hilfreich für Szenarios, in denen die unmittelbare Vorgehensweise davon abhängt, ob das Gerät antworten konnte, z.B. beim Senden eines SMS-Weckrufs an ein Gerät, wenn es offline ist (da eine SMS teurer als ein Methodenaufruf ist).

Jedes Gerätemethode hat ein einzelnes Gerät als Ziel. [Aufträge][lnk-devguide-jobs] bieten eine Möglichkeit zum Aufrufen von direkten Methoden auf mehreren Geräten und zum Planen von Methodenaufrufen für nicht verbundene Geräte.

Jeder Benutzer mit der Berichtigung **Dienstverbindung** für IoT Hub kann eine Methode auf einem Gerät aufrufen.

### <a name="when-to-use"></a>Einsatzgebiete
Direkte Methoden entsprechen einem Anforderung-Antwort-Schema und sind für Kommunikation bestimmt, die sofortige Bestätigung ihres Ergebnisses erfordert, in der Regel über interaktive Steuerung des Geräts, z.B. Einschalten eines Lüfters.

Falls Sie weitere Informationen zur Verwendung von gewünschten Eigenschaften, direkten Methoden oder C2D-Nachrichten benötigen, hilft Ihnen das Thema [Leitfaden zur C2D-Kommunikation][lnk-c2d-guidance] weiter.

## <a name="method-lifecycle"></a>Methodenlebenszyklus
Direkte Methoden werden auf dem Gerät implementiert und können für eine ordnungsgemäße Instanziierung null oder mehr Eingaben in der Methodennutzlast erfordern. Sie rufen eine direkte Methode über einen dienstseitigen URI (`{iot hub}/twins/{device id}/methods/`) auf. Ein Gerät empfängt direkte Methoden in einem gerätespezifischen MQTT-Thema (`$iothub/methods/POST/{method name}/`). Möglicherweise wird in Zukunft eine Unterstützung für direkte Methoden über weitere geräteseitige Netzwerkprotokolle eingeführt.

> [!NOTE]
> Wenn Sie eine direkte Methode auf einem Gerät aufrufen, können Eigenschaftennamen und -werte nur druckbare alphanumerische US-ASCII-Zeichen mit Ausnahme der folgenden enthalten: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.
> 
> 

Direkte Methoden sind synchron und werden nach der Wartezeit (Standardeinstellung: 30 Sekunden, einstellbar bis 3.600 Sekunden) entweder mit einem Erfolg oder Fehler abgeschlossen. Direkte Methoden sind hilfreich bei interaktiven Szenarios, in denen ein Gerät genau dann agieren soll, wenn es online ist und Befehle empfängt, z.B. Einschalten eines Lichts von einem Telefon aus. In diesen Szenarios soll der Erfolg oder Misserfolg unmittelbar erkennbar sein, damit der Clouddienst so schnell wie möglich auf das Ergebnis reagieren kann. Das Gerät kann einen Nachrichtentext als Ergebnis der Methode zurückgeben, dies ist für die Methode aber nicht erforderlich. Es gibt keine Garantie für die Sortierung oder eine Parallelitätssemantik für Methodenaufrufe.

Direkte Methodenaufrufe erfolgen von der Cloudseite nur über HTTP und von der Geräteseite nur über MQTT.

Die Nutzlast für Methodenanforderungen und -antworten ist ein JSON-Dokument bis zu 8KB.

## <a name="reference-topics"></a>Referenzthemen:
Die folgenden Referenzthemen enthalten weitere Informationen zur Verwendung direkter Methoden.

## <a name="invoke-a-direct-method-from-a-back-end-app"></a>Aufrufen einer direkten Methode aus einer Back-End-App
### <a name="method-invocation"></a>Methodenaufruf
Direkte Methodenaufrufe auf einem Gerät sind HTTP-Aufrufe, die Folgendes umfassen:

* Den spezifischen *URI* für das Gerät (`{iot hub}/twins/{device id}/methods/`)
* Die POST-*Methode*
* *Header*, die Autorisierung, Anforderungs-ID, Inhaltstyp und Inhaltscodierung enthalten
* Einen transparenten JSON-*Haupttext* im folgenden Format:

```
{
    "methodName": "reboot",
    "responseTimeoutInSeconds": 200,
    "payload": {
        "input1": "someInput",
        "input2": "anotherInput"
    }
}
```

Timeout in Sekunden. Wenn kein Timeout festgelegt ist, lautet der Standardwert 30 Sekunden.

### <a name="response"></a>Antwort
Die Back-End-App empfängt eine Antwort, die Folgendes umfasst:

* *HTTP-Statuscode*, der für Fehler von IoT Hub verwendet wird. Dazu gehören z.B. 404-Fehler für Geräte, die derzeit nicht verbunden sind
* *Header*, die ETag, Anforderungs-ID, Inhaltstyp und Inhaltscodierung enthalten
* Einen JSON-*Haupttext* im folgenden Format:

```
{
    "status" : 201,
    "payload" : {...}
}
```

   `status` und `body` werden vom Gerät bereitgestellt und für die Antwort mit dem Statuscode und/oder der Beschreibung des Geräts verwendet.

## <a name="handle-a-direct-method-on-a-device"></a>Behandeln einer direkten Methode auf einem Gerät
### <a name="method-invocation"></a>Methodenaufruf
Geräte empfangen direkte Methodenanforderungen zum MQTT-Thema: `$iothub/methods/POST/{method name}/?$rid={request id}`

Der vom Gerät empfangene Haupttext weist das folgende Format auf:

```
{
    "input1": "someInput",
    "input2": "anotherInput"
}
```

Methodenanforderungen sind QoS 0.

### <a name="response"></a>Antwort
Das Gerät sendet Antworten an `$iothub/methods/res/{status}/?$rid={request id}`, wobei Folgendes gilt:

* Die `status`-Eigenschaft ist der vom Gerät bereitgestellte Status der Methodenausführung.
* Die `$rid`-Eigenschaft ist die Anforderungs-ID des von IoT Hub empfangenen Methodenaufrufs.

Der Haupttext wird durch das Gerät festgelegt und kann jeden beliebigen Status aufweisen.

## <a name="additional-reference-material"></a>Weiteres Referenzmaterial
Weitere Referenzthemen im IoT Hub-Entwicklerhandbuch:

* Unter [IoT Hub-Endpunkte][lnk-endpoints] werden die verschiedenen Endpunkte beschrieben, die jeder IoT-Hub für Laufzeit- und Verwaltungsvorgänge bereitstellt.
* Unter [Drosselung und Kontingente][lnk-quotas] werden die Kontingente für den IoT Hub-Dienst und das Drosselungsverhalten beschrieben, die bei Verwendung des Diensts zu erwarten sind.
* Unter [Azure IoT SDKs für Geräte und Dienste][lnk-sdks] werden die verschiedenen Sprach-SDKs aufgelistet, die Sie bei der Entwicklung von Geräte- und Dienst-Apps für die Interaktion mit IoT Hub verwenden können.
* Unter [Referenz – Abfragesprache für Zwillinge und Aufträge][lnk-query] wird die IoT Hub-Abfragesprache beschrieben, mit der Sie von IoT Hub Informationen zu Gerätezwillingen und Aufträgen abrufen können.
* [IoT Hub-MQTT-Unterstützung][lnk-devguide-mqtt] enthält weitere Informationen zur Unterstützung für das MQTT-Protokoll in IoT Hub.

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie erfahren haben, wie Sie direkte Methoden verwenden, sind möglicherweise die folgenden Themen im IoT Hub-Entwicklerhandbuch für Sie interessant:

* [Planen von Aufträgen auf mehreren Geräten][lnk-devguide-jobs]

Wenn Sie einige der in diesem Artikel beschriebenen Konzepte ausprobieren möchten, ist möglicherweise das folgende IoT Hub-Tutorial für Sie interessant:

* [Verwenden von direkten Methoden][lnk-methods-tutorial]

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md
[lnk-devguide-messages]: iot-hub-devguide-messaging.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md



<!--HONumber=Jan17_HO4-->


