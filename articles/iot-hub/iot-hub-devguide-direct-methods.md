<properties
 pageTitle="Entwicklerhandbuch – direkte Methoden | Microsoft Azure"
 description="Azure IoT Hub-Entwicklerhandbuch – Verwenden von direkten Methoden zum Aufrufen von Code auf Ihren Geräten"
 services="iot-hub"
 documentationCenter=".net"
 authors="nberdy"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="nberdy"/>


# <a name="invoke-a-direct-method-on-a-device-(preview)"></a>Aufrufen einer direkten Methode auf einem Gerät (Vorschau)

## <a name="overview"></a>Übersicht

IoT Hub gibt Ihnen die Möglichkeit, Methoden auf Geräten von der Cloud aus aufzurufen. Methoden stellen eine Anforderung-Antwort-Interaktion mit einem Gerät dar, die einem HTTP-Aufruf darin ähnelt, dass sie unverzüglich (nach einem vom Benutzer angegebenen Timeout) zu einem Erfolg oder Fehler führt, um den Benutzer über den Status des Aufrufs zu informieren. Dies ist hilfreich für Szenarios, in denen die unmittelbare Vorgehensweise davon abhängt, ob das Gerät antworten konnte, z.B. beim Senden eines SMS-Weckrufs an ein Gerät, wenn es offline ist (da eine SMS teurer als ein Methodenaufruf ist).

Sie können sich eine Methode als Remoteprozeduraufruf direkt an das Gerät vorstellen. Nur Methoden, die auf einem Gerät implementiert wurden, können aus der Cloud aufgerufen werden. Wenn die Cloud versucht, eine Methode auf einem Gerät aufzurufen, auf dem diese Methode nicht definiert ist, führt der Methodenaufruf zu einem Fehler.

Jedes Gerätemethode hat ein einzelnes Gerät als Ziel. [Aufträge][lnk-devguide-jobs] bieten eine Möglichkeit zum Aufrufen von Methoden auf mehreren Geräten und zum Verschieben von Methodenaufrufen für nicht verbundene Geräte in die Warteschlange.

Jeder Benutzer mit der Berichtigung **Dienstverbindung** für IoT Hub kann eine Methode auf einem Gerät aufrufen.

### <a name="when-to-use"></a>Einsatzgebiete

Gerätemethoden ähneln [Cloud-zu-Gerät-Nachrichten][lnk-devguide-messages], da beide dem Cloud-Back-End ermöglichen, Informationen an ein Gerät zu übergeben. Es gibt jedoch grundlegende Unterschiede zwischen ihnen. Konzeptionell sind Methoden synchron und nicht dauerhaft, während Cloud-zu-Gerät-Nachrichten asynchron mit einer Lebensdauer von bis zu 48 Stunden sind.

Methoden folgen einem Anforderung-Antwort-Muster und sind nicht dauerhaft. Die fehlende Dauerhaftigkeit bietet zwei unmittelbare Vorteile, wenn Sie Befehle an Geräte senden:

- **Sofortiges Feedback bei der Methodenausführung** bedeutet, dass keine Notwendigkeit für Sie besteht, den Zusammenhang zwischen Anforderung und Antwort zu verwalten.
- **Höherer Durchsatz** bedeutet, dass die Vorgänge schneller durchgeführt werden können, da IoT Hub keine Dauerhaftigkeit bereitstellt. IoT Hub lässt pro Einheit mehr Methodenaufrufe als Cloud-zu-Gerät-Nachrichten zu.

Cloud-zu-Gerät-Nachrichten sind nicht unbedingt Befehle an das Gerät. Sie stellen vielmehr die Übergabe von Informationen durch einen Clouddienst an das Gerät dar, die dieses nach eigenem Ermessen abrufen kann. Das Gerät kann selbst entscheiden, ob es auf die Informationsübergabe reagiert. Cloud-zu-Gerät-Nachrichten haben ein längeres Timeout (bis zu 48 Stunden), während Methoden sehr viel schneller ablaufen.

Verwenden Sie Gerätemethoden für sofortige Befehlsaufrufe auf einem Gerät und Aufträge für geplante Aufrufe von Befehlen auf einem Gerät.

## <a name="method-lifecycle"></a>Methodenlebenszyklus

Methoden werden auf dem Gerät implementiert und können für eine ordnungsgemäße Instanziierung null oder mehr Eingaben in der Methodennutzlast erfordern. Sie rufen eine direkte Methode über einen dienstseitigen URI (`{iot hub}/twins/{device id}/methods/`) auf. Ein Gerät empfängt direkte Methoden in einem gerätespezifischen MQTT-Thema (`$iothub/methods/POST/{method name}/`). Möglicherweise wird in Zukunft eine Unterstützung für Methoden über weitere geräteseitige Netzwerkprotokolle eingeführt.

> [AZURE.NOTE] Wenn Sie eine direkte Methode auf einem Gerät aufrufen, können Eigenschaftennamen und -werte nur druckbare alphanumerische US-ASCII-Zeichen mit Ausnahme der folgenden enthalten: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.

Methoden sind synchron und werden nach der Wartezeit (Standardeinstellung: 30 Sekunden, einstellbar bis 3.600 Sekunden) entweder mit einem Erfolg oder Fehler abgeschlossen. Methoden sind hilfreich bei interaktiven Szenarios, in denen ein Gerät genau dann agieren soll, wenn es online ist und Befehle empfängt, z.B. Einschalten eines Lichts von einem Telefon aus. In diesen Szenarios soll der Erfolg oder Misserfolg unmittelbar erkennbar sein, damit der Clouddienst so schnell wie möglich auf das Ergebnis reagieren kann. Das Gerät kann einen Nachrichtentext als Ergebnis der Methode zurückgeben, dies ist für die Methode aber nicht erforderlich. Es gibt keine Garantie für die Sortierung oder eine Parallelitätssemantik für Methodenaufrufe.

Gerätemethodenaufrufe erfolgen von der Cloudseite nur über HTTP und von der Geräteseite nur über MQTT.

## <a name="reference"></a>Referenz

### <a name="service-facing"></a>Dienstseitig

#### <a name="method-invocation"></a>Methodenaufruf

Direkte Methodenaufrufe auf einem Gerät sind HTTP-Aufrufe, die Folgendes umfassen:

- Den spezifischen *URI* für das Gerät (`{iot hub}/twins/{device id}/methods/`)
- Die POST-*Methode*
- *Header*, die Autorisierung, Anforderungs-ID, Inhaltstyp und Inhaltscodierung enthalten
- Einen transparenten JSON-*Haupttext* im folgenden Format:

```
{
    "methodName": "reboot",
    "timeoutInSeconds": 200,
    "payload": {
        "input1": "someInput",
        "input2": "anotherInput"
    }
}
```

  Timeout in Sekunden. Wenn kein Timeout festgelegt ist, lautet der Standardwert 30 Sekunden.
  
#### <a name="response"></a>Antwort

Das Back-End empfängt eine Antwort, die Folgendes umfasst:

- *HTTP-Statuscode*, der für Fehler von IoT Hub verwendet wird. Dazu gehören z.B. 404-Fehler für Geräte, die derzeit nicht verbunden sind
- *Header*, die ETag, Anforderungs-ID, Inhaltstyp und Inhaltscodierung enthalten
- Einen JSON-*Haupttext* im folgenden Format:

```
{
    "status" : "OK",
    "payload" : {...}
}
```
  
   `status` und `body` werden vom Gerät bereitgestellt und für die Antwort mit dem Statuscode und/oder der Beschreibung des Geräts verwendet.

### <a name="device-facing"></a>Geräteseitig

#### <a name="method-invocation"></a>Methodenaufruf

Geräte empfangen direkte Methodenanforderungen zum MQTT-Thema: `$iothub/methods/POST/{method name}/?$rid={request id}`

Der vom Gerät empfangene Haupttext weist das folgende Format auf:

```
{
    "input1": "someInput",
    "input2": "anotherInput"
}
```

Methodenanforderungen sind QoS 0.

#### <a name="response"></a>Antwort

Das Gerät sendet Antworten an `$iothub/methods/res/{status}/?$rid={request id}`, wobei Folgendes gilt:

 - Die `status`-Eigenschaft ist der vom Gerät bereitgestellte Status der Methodenausführung.
 - Die `$rid`-Eigenschaft ist die Anforderungs-ID des von IoT Hub empfangenen Methodenaufrufs.

Der Haupttext wird durch das Gerät festgelegt und kann jeden beliebigen Status aufweisen.

### <a name="additional-reference-material"></a>Weiteres Referenzmaterial

Weitere Referenzthemen im Entwicklerhandbuch:

- Unter [IoT Hub-Endpunkte][lnk-endpoints] werden die verschiedenen Endpunkte beschrieben, die jeder IoT Hub für Laufzeit- und Verwaltungsvorgänge bereitstellt.
- Unter [Drosselung und Kontingente][lnk-quotas] werden die Kontingente für den IoT Hub-Dienst und das Drosselungsverhalten beschrieben, die bei Verwendung des Diensts zu erwarten sind.
- Unter [Geräte- und Dienst-SDKs für IoT Hub][lnk-sdks] werden die verschiedenen Sprach-SDKs aufgelistet, die Sie bei der Entwicklung von Geräte- und Dienstanwendungen für die Interaktion mit IoT Hub verwenden können.
- Unter [Abfragesprache für Zwillinge, Methoden und Aufträge][lnk-query] wird die Abfragesprache beschrieben, mit der Sie von IoT Hub Informationen über Gerätezwillinge, Methoden und Aufträge abrufen können.
- [IoT Hub-MQTT-Unterstützung][lnk-devguide-mqtt] enthält weitere Informationen zur Unterstützung für das MQTT-Protokoll in IoT Hub.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie gelernt haben, wie Sie direkte Methoden verwenden, sind möglicherweise die folgenden Themen im Entwicklerhandbuch für Sie interessant:

- [Planen von Aufträgen auf mehreren Geräten][lnk-devguide-jobs]

Wenn Sie einige der in diesem Artikel beschriebenen Konzepte ausprobieren möchten, ist möglicherweise das folgende IoT Hub-Tutorial für Sie interessant:

- [Verwenden von Cloud-zu-Gerät-Methoden][lnk-methods-tutorial]

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-methods-tutorial]: iot-hub-c2d-methods.md
[lnk-devguide-messages]: iot-hub-devguide-messaging.md



<!--HONumber=Oct16_HO2-->


