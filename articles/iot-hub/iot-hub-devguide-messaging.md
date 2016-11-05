---
title: Entwicklerhandbuch – Messaging | Microsoft Docs
description: Azure IoT Hub-Entwicklerhandbuch – Messaging zwischen Geräten und Cloud
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: ''

ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/30/2016
ms.author: dobett

---
# <a name="send-and-receive-messages-with-iot-hub"></a>Senden und Empfangen von Nachrichten mit IoT Hub
## <a name="overview"></a>Übersicht
IoT Hub bietet die folgenden Messaginggrundtypen für die Kommunikation mit einem Gerät:

* [Gerät-zu-Cloud][lnk-d2c] von einem Gerät zu einem Anwendungs-Back-End.
* [Cloud-zu-Gerät][lnk-c2d] von einem Anwendungs-Back-End (*Dienst* oder *Cloud*).

Die wichtigsten Eigenschaften beim IoT Hub-Messaging sind eine zuverlässige und stabile Übermittlung von Nachrichten. Diese Eigenschaften ermöglichen Ausfallsicherheit bei zeitweiligen Verbindungsproblemen auf Geräteseite und Lastspitzen bei der Ereignisverarbeitung auf Cloudseite. IoT Hub implementiert *mindestens einmal* Übermittlungsgarantien für das D2C- und C2D-Messaging.

IoT Hub unterstützt mehrere [geräteseitige Protokolle][lnk-protocols] (z.B. MQTT, AMQP und HTTP). Um eine nahtlose Interoperabilität zwischen Protokollen zu gewährleisten, definiert IoT Hub ein gemeinsames [Nachrichtenformat][lnk-message-format], das von allen geräteseitigen Protokollen unterstützt wird.

IoT Hub stellt einen [Event Hubs-kompatiblen Endpunkt][lnk-compatible-endpoint] zur Verfügung, über den Back-End-Anwendungen vom Hub empfangene Gerät-zu-Cloud-Nachrichten lesen können.

### <a name="when-to-use"></a>Einsatzgebiete
Messaging ist eine Kernfunktion von IoT Hub. Verwenden Sie sie, wenn Sie Nachrichten von Ihrem Gerät an Ihr Back-End oder von Ihrem Back-End an ein Gerät senden müssen.

Einen Vergleich der Dienste IoT Hub und Event Hubs finden Sie unter [Vergleich von IoT Hub und Event Hubs][lnk-compare].

## <a name="device-to-cloud-messages"></a>D2C-Nachrichten
Sie senden Gerät-zu-Cloud-Nachrichten (Device-to-Cloud, D2C) von einem geräteseitigen Endpunkt (**/devices/{Geräte-ID}/messages/events**). Ihr Back-End-Dienst empfängt Gerät-zu-Cloud-Nachrichten über einen dienstseitigen Endpunkt (**/messages/events**), der mit [Event Hubs][lnk-event-hubs] kompatibel ist. Auf diese Weise können Sie standardmäßige [Event Hubs-Integration und SDKs][lnk-compatible-endpoint] zum Empfangen von D2C-Nachrichten verwenden.

IoT Hub implementiert D2C-Messaging auf ähnliche Weise wie [Event Hubs][lnk-event-hubs]. Die D2C-Nachrichten von IoT Hub ähneln eher Event Hubs-*Ereignissen* als [Service Bus][lnk-servicebus]-*Nachrichten*.

Diese Implementierung hat folgende Auswirkungen:

* Ähnlich wie Event Hubs-Ereignisse sind D2C-Nachrichten dauerhaft und werden in einem IoT Hub bis zu sieben Tage lang aufbewahrt (siehe [Optionen für die D2C-Konfiguration][lnk-d2c-configuration]).
* D2C-Nachrichten werden für einen festen Satz an Partitionen partitioniert, der zum Zeitpunkt der Erstellung festgelegt wird (siehe [Optionen für die D2C-Konfiguration][lnk-d2c-configuration]).
* Analog zu Event Hubs müssen Clients, die D2C-Nachrichten lesen, Partitionen und Prüfpunkte verarbeiten können. Weitere Informationen finden Sie unter [Event Hubs – Nutzen von Ereignissen][lnk-event-hubs-consuming-events].
* D2C-Nachrichten dürfen wie Event Hubs-Ereignisse maximal 256 KB groß sein. Sie können in Batches gruppiert werden, um den Sendevorgang zu optimieren. Batches können maximal 256 KB groß sein und maximal 500 Nachrichten enthalten.

Es gibt jedoch einige wichtige Unterschiede zwischen dem D2C-Messaging von IoT Hub und Event Hubs:

* Wie im Abschnitt [Verwalten des Zugriffs auf IoT Hub][lnk-devguide-security] erläutert, ermöglicht IoT Hub Authentifizierung und Access Control auf Gerätebasis.
* IoT Hub unterstützt Millionen von gleichzeitig verbundenen Geräten (siehe [Kontingente und Drosselung][lnk-quotas]), während Event Hubs auf 5.000 AMQP-Verbindungen pro Namespace beschränkt ist.
* IoT Hub ermöglicht keine zufällige Partitionierung mit einem **PartitionKey**. D2C-Nachrichten werden gemäß ihrer ursprünglichen **deviceId**partitioniert.
* Die Skalierung funktioniert in IoT Hub etwas anders als in Event Hubs. Weitere Informationen finden Sie unter [Skalieren von IoT Hub][lnk-guidance-scale].

> [!NOTE]
> Sie können Event Hubs nicht in allen Szenarios durch IoT Hub ersetzen. In einigen Szenarien mit Ereignisverarbeitung kann es beispielsweise erforderlich sein, Ereignisse in Bezug auf eine andere Eigenschaft oder ein anderes Feld neu zu partitionieren, bevor die Datenströme analysiert werden können. In diesem Szenario können Sie mit einem Event Hub zwei Bestandteile der Pipeline zur Datenstromverarbeitung entkoppeln. Weitere Informationen finden Sie unter *Partitionen* in [Übersicht über Azure Event Hubs][lnk-eventhub-partitions].
> 
> 

Ausführliche Informationen zum Verwenden des D2C-Messaging finden Sie unter [IoT Hub-APIs und -SDKs][lnk-sdks].

> [!NOTE]
> Bei der Verwendung von HTTP zum Senden von D2C-Nachrichten dürfen Eigenschaftennamen und Eigenschaftswerte nur alphanumerische ASCII-Zeichen sowie die Zeichen ``{'!', '#', '$', '%, '&', "'", '*', '*', '+', '-', '.', '^', '_', '`', '|', '~'}`` enthalten.
> 
> 

### <a name="non-telemetry-traffic"></a>Datenverkehr ohne Telemetrie
Oftmals senden Geräte nicht nur Telemetriedatenpunkte, sondern auch Nachrichten und Anforderungen, die eine Ausführung und Verarbeitung auf der Anwendungsschicht mit der Geschäftslogik erfordern. Beispiele: Kritische Warnungen, die eine bestimmte Aktion im Back-End auslösen müssen, oder Geräteantworten auf vom Back-End gesendete Befehle.

Weitere Informationen über die beste Möglichkeit zum Verarbeiten dieser Art von Nachricht finden Sie im Tutorial [Gewusst wie: Verarbeiten von D2C-Nachrichten mit IoT Hub][lnk-d2c-tutorial].

### <a name="device-to-cloud-configuration-options"></a>Optionen für die D2C-Konfiguration
Ein IoT Hub macht die folgenden Eigenschaften zum Steuern des D2C-Messaging verfügbar:

* **Anzahl von Partitionen**. Legen Sie diese Eigenschaft bei der Erstellung fest, um die Anzahl von Partitionen für die D2C-Ereigniserfassung zu definieren.
* **Aufbewahrungsdauer**. Diese Eigenschaft legt die Aufbewahrungszeit für D2C-Nachrichten fest. Als Standardwert ist ein Tag festgelegt, dieser Wert kann jedoch auf sieben Tage erhöht werden.

Analog zu Event Hubs ermöglicht IoT Hub die Verwaltung von Consumergruppen am empfangenden D2C-Endpunkt.

Sie können alle diese Eigenschaften sowohl programmgesteuert über die [Azure IoT Hub-Ressourcenanbieter-APIs][lnk-resource-provider-apis] als auch über das [Azure-Portal][lnk-management-portal] ändern.

### <a name="anti-spoofing-properties"></a>Eigenschaften zum Schutz vor Spoofing
Um ein Gerätespoofing beim D2C-Messaging zu verhindern, versieht IoT Hub alle Nachrichten mit den folgenden Eigenschaften:

* **ConnectionDeviceId**
* **ConnectionDeviceGenerationId**
* **ConnectionAuthMethod**

Die ersten beiden Eigenschaften enthalten die Werte für **deviceId** und **generationId** des ursprünglichen Geräts. Eine Beschreibung finden Sie unter [Geräteidentitätseigenschaften][lnk-device-properties].

Die Eigenschaft **ConnectionAuthMethod** enthält ein serialisiertes JSON-Objekt mit folgenden Eigenschaften:

```
{
  "scope": "{ hub | device}",
  "type": "{ symkey | sas}",
  "issuer": "iothub"
}
```

## <a name="cloud-to-device-messages"></a>C2D-Nachrichten
Sie senden Cloud-zu-Gerät-Nachrichten (Cloud-to-Device, C2D) über einen dienstseitigen Endpunkt (**/messages/devicebound**). Ein Gerät empfängt sie über einen gerätespezifischen Endpunkt (**/devices/{Geräte-ID}/messages/devicebound**).

Jede C2D-Nachricht wird einem bestimmten Gerät zugeordnet, indem die **to**-Eigenschaft auf **/devices/{Geräte-ID}/messages/devicebound** festgelegt wird.

> [!IMPORTANT]
> Jede Gerätewarteschlange kann maximal 50 C2D-Nachrichten enthalten. Der Versuch, eine größere Anzahl von Nachrichten an das gleiche Gerät zu senden, führt zu einem Fehler.
> 
> [!NOTE]
> Beim Senden von C2D-Nachrichten dürfen Eigenschaftennamen und Eigenschaftswerte nur alphanumerische ASCII-Zeichen sowie die Zeichen ``{'!', '#', '$', '%, '&', "'", '*', '*', '+', '-', '.', '^', '_', '`', '|', '~'}`` enthalten.
> 
> 

### <a name="message-lifecycle"></a>Nachrichtenlebenszyklus
Um die Nachrichtenübermittlung mindestens einmal zu gewährleisten, werden C2D-Nachrichten in IoT Hub in Warteschlangen auf Gerätebasis beibehalten. Geräte müssen explizit den *Abschluss* bestätigen, damit IoT Hub sie aus der Warteschlange entfernen kann. Auf diese Weise wird Ausfallsicherheit bei Verbindungs- und Gerätefehlern gewährleistet.

Die folgende Abbildung zeigt den Lebenszyklus einer C2D-Nachricht und ihren jeweiligen Status.

![Lebenszyklus von C2D-Nachrichten][img-lifecycle]

Wenn der Dienst eine Nachricht sendet, wird diese als *zur Warteschlange hinzugefügt*betrachtet. Wenn ein Gerät eine Nachricht *empfangen* möchte, *sperrt* IoT Hub die Nachricht (Status wird auf **Nicht sichtbar** gesetzt), um anderen Threads auf dem gleichen Gerät das Empfangen anderer Nachrichten zu ermöglichen. Wenn ein Gerätethread die Verarbeitung einer Nachricht abschließt, wird IoT Hub hierüber durch den *Abschluss* der Nachricht benachrichtigt.

Ein Gerät kann auch Folgendes durchführen:

* *Ablehnen* der Nachricht: IoT Hub versetzt sie in den Status **Unzustellbar** . Hinweis: Geräte, die eine Verbindung mit MQTT herstellen, können C2D-Nachrichten nicht ablehnen.
* *Verwerfen* der Nachricht: IoT Hub platziert die Nachricht wieder in der Warteschlange mit dem Status **Zur Warteschlange hinzugefügt**.

Bei der Nachrichtenverarbeitung durch den Thread könnte ein Fehler auftreten, ohne dass IoT Hub hierüber benachrichtigt wird. In diesem Fall werden Nachrichten automatisch vom Status **Nicht sichtbar** zurück in den Status **Enqueued** (Zur Warteschlange hinzugefügt) versetzt, wenn ein *Timeout für die Sichtbarkeit (oder Sperrung)* abgelaufen ist. Der Standardwert dieses Timeouts ist eine Minute.

Eine Nachricht kann zwischen den Statuswerten **Enqueued** (Zur Warteschlange hinzugefügt) und **Nicht sichtbar** maximal so oft wechseln, wie in der Eigenschaft **Anzahl maximaler Zustellungen** in IoT Hub festgelegt wurde. Nachdem diese Anzahl überschritten wurde, legt IoT Hub den Status der Nachricht auf **Unzustellbar**fest. Ebenso kennzeichnet IoT Hub eine Nachricht als **Deadlettered** (Unzustellbar), wenn ihre Gültigkeitsdauer abgelaufen ist (siehe [Gültigkeitsdauer][lnk-ttl]).

Ein Tutorial für C2D-Nachrichten finden Sie unter [Gewusst wie: Senden von Cloud-zu-Gerät-Nachrichten mit IoT Hub][lnk-c2d-tutorial]. Informationen dazu, wie verschiedene APIs und SDKs die C2D-Funktionalität verfügbar machen, finden Sie unter [IoT Hub-APIs und -SDKs][lnk-sdks].

> [!NOTE]
> Typischerweise werden C2D-Nachrichten immer dann abgeschlossen, wenn der Verlust der Nachricht keine Auswirkung auf die Anwendungslogik hat. Das ist z.B. der Fall, wenn der Inhalt der Nachricht erfolgreich im lokalen Speicher beibehalten oder ein Vorgang erfolgreich ausgeführt wurde. Die Nachricht könnte auch vorübergehende Informationen übermitteln, deren Verlust der Funktionalität der Anwendung nicht beeinträchtigen würde. In einigen Fällen können Sie für Tasks mit langer Ausführungsdauer die C2D-Nachricht nach Beibehalten der Beschreibung des Tasks im lokalen Speicher abschließen. Dann können Sie das Anwendungs-Back-End mit einer oder mehreren D2C-Nachrichten in verschiedenen Phasen des Taskverlaufs benachrichtigen.
> 
> 

### <a name="message-expiration-(time-to-live)"></a>Nachrichtenablauf (Gültigkeitsdauer)
Jede C2D-Nachricht verfügt über eine Gültigkeitsdauer. Diese Zeit wird (in der **ExpiryTimeUtc**-Eigenschaft) ausdrücklich durch den Dienst oder durch IoT Hub mithilfe der standardmäßig als IoT Hub-Eigenschaft angegebenen *Gültigkeitsdauer* festgelegt. Weitere Informationen finden Sie unter [Optionen für die C2D-Konfiguration][lnk-c2d-configuration].

> [!NOTE]
> Eine gängige Methode, den Vorteil des Nachrichtenablaufs zu nutzen und zu vermeiden, dass Nachrichten an getrennte Geräte gesendet werden, ist die Festlegung einer kurzen Gültigkeitsdauer für Werte. Bei diesem Ansatz wird das gleiche Ergebnis erzielt wie beim Aufrechterhalten des Geräteverbindungsstatus, er ist aber effizienter. Wenn Sie Nachrichtenbestätigungen anfordern, benachrichtigt IoT Hub Sie darüber, welche der Geräte in der Lage sind, Nachrichten zu empfangen, und welche nicht online sind bzw. bei welchen bei der Zustellung ein Fehler auftrat.
> 
> 

### <a name="message-feedback"></a>Nachrichtenfeedback
Beim Senden einer C2D-Nachricht kann der Dienst das Übermitteln von Feedback auf Nachrichtenbasis anfordern, um über den finalen Status dieser Nachricht informiert zu werden.

* Wenn Sie die **Ack**-Eigenschaft auf **Positiv** festlegen, generiert IoT Hub nur dann eine Feedbacknachricht, wenn die C2D-Nachricht den Status **Abgeschlossen** erreicht hat.
* Wenn Sie die **Ack**Eigenschaft auf **Negativ** festlegen, generiert IoT Hub nur dann eine Feedbacknachricht, wenn die C2D-Nachricht den Status **Deadlettered** (Unzustellbar) erreicht.
* Bei Festlegung der **Ack**-Eigenschaft auf **Voll** generiert IoT Hub in beiden Fällen eine Feedbacknachricht.

> [!NOTE]
> Wenn **Ack** auf **Voll** festgelegt ist und Sie keine Feedbacknachricht erhalten, bedeutet dies, dass die Feedbacknachricht abgelaufen ist. Der Dienst kann nicht wissen, was mit der ursprünglichen Nachricht geschehen ist. In der Praxis sollte ein Dienst sicherstellen, dass Feedback verarbeitet werden kann, bevor es abläuft. Die maximale Ablaufzeit beträgt zwei Tage, daher sollte ausreichend Zeit verbleiben, um den Dienst zu starten, wenn ein Fehler auftritt.
> 
> 

Wie im Abschnitt [Endpunkte][lnk-endpoints] erläutert, übermittelt IoT Hub Feedback in Form von Nachrichten über einen dienstseitigen Endpunkt (**/messages/servicebound/feedback**). Die Semantik für den Empfang von Feedback stimmt mit der für C2D-Nachrichten überein und weist den gleichen [Nachrichtenlebenszyklus][lnk-lifecycle] auf. Nachrichtenfeedback wird nach Möglichkeit in einer einzigen Nachricht zusammengefasst, die das folgende Format aufweist:

| Eigenschaft | Beschreibung |
| --- | --- |
| EnqueuedTime |Zeitstempel, der die Erstellung der Nachricht angibt. |
| UserId |`{iot hub name}` |
| ContentType |`application/vnd.microsoft.iothub.feedback.json` |

Der Nachrichtenkörper ist ein serialisiertes JSON-Array aus Datensätzen, von denen jeder die folgenden Eigenschaften aufweist:

| Eigenschaft | Beschreibung |
| --- | --- |
| EnqueuedTimeUtc |Zeitstempel, der den Zeitpunkt des Nachrichtenergebnisses angibt. Diese Eigenschaft kann beispielsweise angeben, wann das Gerät abgeschlossen wurde oder die Nachricht abgelaufen ist. |
| OriginalMessageId |**MessageId** der C2D-Nachricht, auf die sich das Feedback bezieht. |
| StatusCode |Erforderlicher Ganzzahlwert. In von IoT Hub erzeugten Feedbacknachrichten verwendet. <br/> 0 = erfolgreich <br/> 1 = Nachricht abgelaufen <br/> 2 = maximale Übermittlungsanzahl überschritten <br/> 3 = Nachricht abgelehnt |
| Beschreibung |Zeichenfolgenwerte für **StatusCode**. |
| deviceId |**DeviceId** des Zielgeräts für die C2D-Nachricht, auf die sich das Feedback bezieht. |
| DeviceGenerationId |**DeviceGenerationId** des Zielgeräts für die C2D-Nachricht, auf die sich das Feedback bezieht. |

> [!IMPORTANT]
> Der Dienst muss eine **MessageId** für die C2D-Nachricht angeben, damit das Feedback der ursprünglichen Nachricht zugeordnet werden kann.
> 
> 

Das folgende Beispiel zeigt den Text einer Feedbacknachricht.

```
[
  {
    "OriginalMessageId": "0987654321",
    "EnqueuedTimeUtc": "2015-07-28T16:24:48.789Z",
    "StatusCode": 0
    "Description": "Success",
    "DeviceId": "123",
    "DeviceGenerationId": "abcdefghijklmnopqrstuvwxyz"
  },
  {
    ...
  },
  ...
]
```

### <a name="cloud-to-device-configuration-options"></a>Optionen für die C2D-Konfiguration
Jeder IoT Hub legt die folgenden Konfigurationsoptionen für das C2D-Messaging offen:

| Eigenschaft | Beschreibung | Bereich und Standardwert |
| --- | --- | --- |
| defaultTtlAsIso8601 |Standardmäßige Gültigkeitsdauer für C2D-Nachrichten. |ISO_8601-Intervall bis 2D (mindestens 1 Minute). Standardwert: 1 Stunde. |
| maxDeliveryCount |Maximale Zustellungsanzahl für C2D-Gerätewarteschlangen pro Gerät. |1 bis 100. Standardwert: 10 |
| feedback.ttlAsIso8601 |Aufbewahrungsdauer für dienstgebundene Feedbacknachrichten. |ISO_8601-Intervall bis 2D (mindestens 1 Minute). Standardwert: 1 Stunde. |
| feedback.maxDeliveryCount |Maximale Zustellungsanzahl für Feedbackwarteschlangen. |1 bis 100. Standardwert: 100. |

Weitere Informationen finden Sie unter [Erstellen von IoT-Hubs][lnk-portal].

## <a name="read-device-to-cloud-messages"></a>Lesen von D2C-Nachrichten
IoT Hub stellt einen Endpunkt für Ihre Back-End-Dienste zum Lesen der Gerät-zu-Cloud-Nachrichten, die von Ihrem Hub empfangen werden, bereit. Der Endpunkt ist Event Hubs-kompatibel, sodass Sie zum Lesen von Nachrichten alle Mechanismen verwenden können, die der Event Hubs-Dienst unterstützt.

Wenn Sie das [Azure Service Bus-SDK für .NET][lnk-servicebus-sdk] oder den [Event Hubs-Ereignisprozessorhost][lnk-eventprocessorhost] verwenden, können Sie eine beliebige IoT Hub-Verbindungszeichenfolge mit den richtigen Berechtigungen verwenden. Anschließend verwenden Sie **messages/events** als Event Hub-Name.

Wenn Sie SDKs (oder Produktintegrationen) verwenden, die nicht IoT Hub-fähig sind, müssen Sie einen Event Hubs-kompatiblen Endpunkt und den Event Hub-Namen aus den IoT Hub-Einstellungen im [Azure-Portal][lnk-management-portal] abrufen:

1. Klicken Sie auf dem IoT Hub-Blatt auf **Messaging**.
2. Im Abschnitt **Gerät-zu-Cloud-Einstellungen** finden Sie die folgenden Werte: **Event Hub-kompatibler Endpunkt**, **Event Hub-kompatibler Name** und **Partitionen**.
   
    ![D2C-Einstellungen][img-eventhubcompatible]

> [!NOTE]
> Wenn das SDK einen Wert für **Hostname** oder **Namespace** benötigt, entfernen Sie das Schema aus dem **Event Hub-kompatiblen Endpunkt**. Wenn es sich bei Ihrem Event Hub-kompatiblen Endpunkt beispielsweise um **sb://iothub-ns-myiothub-1234.servicebus.windows.net/** handelt, lautet der **Hostname** **iothub-ns-myiothub-1234.servicebus.windows.net**, und der **Namespace** lautet **iothub-ns-myiothub-1234**.
> 
> 

Sie können in diesem Fall eine beliebige Sicherheitsrichtlinie für den gemeinsamen Zugriff mit den **ServiceConnect** -Berechtigungen zur Verbindungsherstellung mit dem angegebenen Event Hub verwenden.

Wenn Sie eine Event Hub-Verbindungszeichenfolge mit den zuvor angegebenen Informationen erstellen müssen, verwenden Sie das folgende Muster:

```
Endpoint={Event Hub-compatible endpoint};SharedAccessKeyName={iot hub policy name};SharedAccessKey={iot hub policy key}
```

Nachfolgend finden Sie eine Liste der SDKs und Integrationen, die Sie mit Event Hub-kompatiblen Endpunkten verwenden können, die IoT Hub verfügbar macht:

* [Java-Event Hubs-Client](https://github.com/hdinsight/eventhubs-client)
* [Apache Storm-Spout](../hdinsight/hdinsight-storm-develop-csharp-event-hub-topology.md). Sie können sich die [Spoutquelle](https://github.com/apache/storm/tree/master/external/storm-eventhubs) bei GitHub ansehen.
* [Apache Spark-Integration](../hdinsight/hdinsight-apache-spark-eventhub-streaming.md)

## <a name="reference"></a>Referenz
### <a name="message-format"></a>Nachrichtenformat
IoT Hub-Nachrichten umfassen:

* Einen Satz an *Systemeigenschaften*. Eigenschaften, die von IoT Hub interpretiert oder festgelegt werden. Dieser Satz ist vordefiniert.
* Einen Satz an *Anwendungseigenschaften*. Ein Wörterbuch mit Zeichenfolgeneigenschaften. Die Anwendung kann diese definieren und darauf zugreifen, ohne den Nachrichtentext deserialisieren zu müssen. IoT Hub ändert diese Eigenschaften nie.
* Ein nicht lesbarer binärer Textkörper.

Weitere Informationen dazu, wie die Nachricht in verschiedenen Protokollen codiert wird, finden Sie unter [IoT Hub-APIs und -SDKs][lnk-sdks].

In der folgenden Tabelle werden die Systemeigenschaften in IoT Hub-Nachrichten aufgeführt.

| Eigenschaft | Beschreibung |
| --- | --- |
| MessageId |Eine vom Benutzer festgelegte Kennung für die Nachricht; wird für Anforderung-Antwort-Muster verwendet. Format: Eine Zeichenfolge mit Berücksichtigung von Klein-/Großschreibung (bis zu 128 Zeichen lang), die aus alphanumerischen ASCII-Zeichen (7 Bit) + `{'-', ':',’.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}`besteht. |
| Sequenznummer |Eine Nummer (für jede Gerätewarteschlange eindeutig), die jeder C2D-Nachricht von IoT Hub zugewiesen wird |
| To | |
| Ein Ziel, das in [C2D][lnk-c2d]-Nachrichten angegeben wird. | |
| ExpiryTimeUtc |Datum und Uhrzeit des Nachrichtenablaufs. |
| EnqueuedTime |Datum und Uhrzeit des Empfangs der Nachricht durch IoT Hub. |
| CorrelationId |Eine Zeichenfolgeneigenschaft in einer Antwortnachricht, die normalerweise die Nachrichten-ID der Anforderung im Anforderung-Antwort-Muster enthält. |
| UserId |Eine ID zum Festlegen des Ursprungs von Nachrichten. Wenn IoT Hub Nachrichten generiert, wird diese Eigenschaft auf `{iot hub name}`festgelegt. |
| Ack |Ein Feedbacknachrichtengenerator. Diese Eigenschaft wird in C2D-Nachrichten verwendet, um IoT Hub anzuweisen, als Ergebnis der Nachrichtenverarbeitung durch das Gerät Feedbacknachrichten zu generieren. Mögliche Werte: **Kein** (Standardeinstellung): Es wird keine Feedbacknachricht generiert. **Positiv**: Es wird eine Feedbacknachricht empfangen, wenn die Nachricht abgeschlossen wurde. **Negativ**: Es wird eine Feedbacknachricht empfangen, wenn die Nachricht ohne vollständige Verarbeitung durch das Gerät abgelaufen ist (oder die maximale Anzahl von Zustellversuchen erreicht wurde). **Voll**: Feedback wird sowohl bei erfolgreicher als auch nicht erfolgreicher Nachrichtenverarbeitung generiert. Weitere Informationen finden Sie unter [Nachrichtenfeedback][lnk-feedback]. |
| ConnectionDeviceId |Eine ID, die von IoT Hub für D2C-Nachrichten festgelegt wird. Diese Eigenschaft enthält die **deviceId** des Geräts, das die Nachricht sendet. |
| ConnectionDeviceGenerationId |Eine ID, die von IoT Hub für D2C-Nachrichten festgelegt wird. Diese Eigenschaft enthält die **generationId** (gemäß [Geräteidentitätseigenschaften][lnk-device-properties]) des Geräts, das die Nachricht gesendet hat. |
| ConnectionAuthMethod |Eine von IoT Hub für D2C-Nachrichten festgelegte Authentifizierungsmethode. Diese Eigenschaft enthält Informationen zu der Methode, die zum Authentifizieren des Geräts verwendet wird, das die Nachricht sendet. Weitere Informationen finden Sie unter [D2C-Anti-Spoofing][lnk-antispoofing]. |

### <a name="communication-protocols"></a>Kommunikationsprotokolle
IoT Hub unterstützt für die geräteseitige Kommunikation die Protokolle MQTT, [AMQP][lnk-amqp], AMQP über WebSockets und HTTP/1. Die folgende Tabelle enthält allgemeine Ratschläge für Ihre Protokollauswahl:

| Protocol | Wann Sie dieses Protokoll auswählen sollten |
| --- | --- |
| MQTT |Verwenden Sie es auf allen Geräten, die nicht die Verwendung von WebSockets erfordern. |
| AMQPS |Verwenden Sie es für Feld- und Cloudgateways, um die Vorteile geräteübergreifender Multiplexingverbindungen zu nutzen. <br/>  Verwenden Sie es, wenn Sie die Verbindung über Port 443 herstellen müssen. |
| HTTPS |Verwenden Sie es für Geräte, die keine anderen Protokolle unterstützen können. |

Beachten Sie bei der Auswahl des Protokolls für die geräteseitige Kommunikation folgende Punkte:

* **C2D-Muster**. HTTP/1 verfügt über keine effiziente Methode zum Implementieren von Serverpushvorgängen. Daher fragen Geräte bei Verwendung von HTTP/1 IoT Hub nach C2D-Nachrichten ab. Dieser Ansatz ist für das Gerät und auch für IoT Hub sehr ineffizient. Unter den aktuellen HTTP/1-Richtlinien sollte jedes Gerät mindestens alle 25 Minuten eine Abfrage nach Nachrichten durchführen. Auf der anderen Seite unterstützen MQTT und AMQP Serverpush beim Empfangen von C2D-Nachrichten. Sie ermöglichen sofortiges Nachrichtenpushen von IoT Hub zum Gerät. Wenn die Übermittlungslatenz eine wichtige Rolle spielt, sind AMQP oder MQTT die zu bevorzugenden Protokolle. Bei nur selten verbundenen Geräten funktioniert auch HTTP/1.
* **Bereichsgateways**. Bei Verwendung von HTTP/1 und MQTT ist es nicht möglich, mehrere Geräte (jedes mit eigenen Anmeldeinformationen pro Gerät) mithilfe der gleichen TLS-Verbindung zu verbinden. Darum sind diese Protokolle für [Bereichsgatewayszenarios][lnk-azure-gateway-guidance] nicht optimal, da sie eine TLS-Verbindung zwischen dem Bereichsgateway und IoT Hub für jedes Gerät benötigen, das mit dem Bereichsgateway verbunden ist.
* **Geräte mit eingeschränkten Ressourcen**. MQTT- und HTTP/1-Bibliotheken haben weniger Speicherbedarf als die AMQP-Bibliotheken. Wenn daher das Gerät über limitierte Ressourcen verfügt (beispielsweise weniger als 1MB RAM), stehen möglicherweise nur diese Protokolle als Protokollimplementierung zur Verfügung.
* **Netzwerkausnahme**. MQTT lauscht standardmäßig an Port 8883. Dies kann Probleme in Netzwerken verursachen, die für Nicht-HTTP-Protokolle geschlossen sind. HTTP und AMQP (über WebSockets) können in diesem Szenario verwendet werden.
* **Größe der Nutzlast**. AMQP und MQTT sind binäre Protokolle, was zu kompakteren Nutzlasten als bei HTTP/1 führt.

> [!NOTE]
> Bei Verwendung von HTTP/1 muss jedes Gerät mindestens alle 25 Minuten eine Abfrage auf C2D-Nachrichten durchführen. In der Entwicklungsphase darf freilich häufiger als alle 25 Minuten eine Abfrage erfolgen.
> 
> 

### <a name="port-numbers"></a>Portnummern
Geräte können mit IoT Hub in Azure über verschiedene Protokolle kommunizieren. In der Regel richtet sich die Wahl des Protokolls nach den spezifischen Anforderungen der Lösung. Die folgende Tabelle enthält die ausgehenden Ports, die geöffnet sein müssen, damit ein Gerät ein bestimmtes Protokoll verwenden kann:

| Protocol | Port(s) |
| --- | --- |
| MQTT |8883 |
| AMQP |5671 |
| AMQP über WebSockets |443 |
| HTTPS |443 |
| LWM2M (Geräteverwaltung) |5684 |

Nachdem Sie einen IoT Hub in einer Azure-Region erstellt haben, behält er seine IP-Adresse für die gesamte Lebensdauer. Wenn der IoT Hub von Microsoft jedoch in eine andere Skalierungseinheit verschoben wird, wird ihm eine neue IP-Adresse zugewiesen, um die Dienstqualität zu gewährleisten.

### <a name="notes-on-mqtt-support"></a>Hinweise zur MQTT-Unterstützung
IoT Hub implementiert die MQTT-Protokollversion 3.1.1 mit den folgenden Einschränkungen und einem bestimmten Verhalten:

* **QoS 2 wird nicht unterstützt**. Wenn ein Geräteclient eine Nachricht mit **QoS 2**veröffentlicht, schließt IoT Hub die Netzwerkverbindung. Wenn ein Geräteclient ein Thema mit **QoS 2** abonniert, gewährt IoT Hub im **SUBACK**-Paket maximal die QoS-Ebene 1.
* **Beibehaltungsnachrichten werden nicht beständig speichert**. Wenn ein Geräteclient eine Nachricht mit auf 1 festgelegtem RETAIN-Flag veröffentlicht, fügt IoT Hub der Nachricht die Anwendungseigenschaft **x-opt-retain** hinzu. In diesem Fall speichert IoT Hub die Beibehaltungsnachricht nicht beständig, sondern übergibt sie an die Back-End-Anwendung.

Weitere Informationen finden Sie unter [IoT Hub MQTT-Unterstützung][lnk-devguide-mqtt].

Schließlich sollte noch der Einsatz eines [Azure IoT-Protokollgateways][lnk-azure-protocol-gateway] erwogen werden, das die Bereitstellung eines hochleistungsfähigen benutzerdefinierten Protokollgateways mit einer direkten Schnittstelle zu IoT Hub ermöglicht. Das Azure IoT-Protokollgateway dient zum Anpassen des Geräteprotokolls zum Unterstützen von Brownfield MQTT-Bereitstellungen oder anderer benutzerdefinierter Protokolle. Dieser Ansatz setzt jedoch voraus, dass Sie ein benutzerdefiniertes Protokollgateway selfhosten und betreiben.

### <a name="additional-reference-material"></a>Weiteres Referenzmaterial
Weitere Referenzthemen im Entwicklerhandbuch:

* Unter [IoT Hub-Endpunkte][lnk-endpoints] werden die verschiedenen Endpunkte beschrieben, die jeder IoT Hub für Laufzeit- und Verwaltungsvorgänge bereitstellt.
* Unter [Drosselung und Kontingente][lnk-quotas] werden die Kontingente für den IoT Hub-Dienst und das Drosselungsverhalten beschrieben, die bei Verwendung des Diensts zu erwarten sind.
* Unter [Geräte- und Dienst-SDKs für IoT Hub][lnk-sdks] werden die verschiedenen Sprach-SDKs aufgelistet, die Sie bei der Entwicklung von Geräte- und Dienstanwendungen für die Interaktion mit IoT Hub verwenden können.
* Unter [Abfragesprache für Zwillinge, Methoden und Aufträge][lnk-query] wird die Abfragesprache beschrieben, mit der Sie von IoT Hub Informationen über Gerätezwillinge, Methoden und Aufträge abrufen können.
* [IoT Hub-MQTT-Unterstützung][lnk-devguide-mqtt] enthält weitere Informationen zur Unterstützung für das MQTT-Protokoll in IoT Hub.

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie nun gelernt haben, wie Sie Nachrichten mit IoT Hub senden und empfangen, sind möglicherweise die folgenden Themen im Entwicklerhandbuch für Sie interessant:

* [Hochladen von Dateien von einem Gerät][lnk-devguide-upload]
* [Verwalten von Geräteidentitäten in IoT Hub][lnk-devguide-identities]
* [Verwalten des Zugriffs auf IoT Hub][lnk-devguide-security]
* [Verwenden von Gerätezwillingen zum Synchronisieren von Status und Konfigurationen][lnk-devguide-device-twins]
* [Aufrufen einer direkten Methode auf einem Gerät][lnk-devguide-directmethods]
* [Planen von Aufträgen auf mehreren Geräten][lnk-devguide-jobs]

Wenn Sie einige der in diesem Artikel beschriebenen Konzepte ausprobieren möchten, sind möglicherweise die folgenden IoT Hub-Tutorials für Sie interessant:

* [Erste Schritte mit Azure IoT Hub][lnk-getstarted-tutorial]
* [Gewusst wie: Senden von Cloud-zu-Gerät-Nachrichten mit IoT Hub][lnk-c2d-tutorial]
* [Gewusst wie: Verarbeiten von D2C-Nachrichten mit IoT Hub][lnk-d2c-tutorial]

[img-lifecycle]: ./media/iot-hub-devguide-messaging/lifecycle.png
[img-eventhubcompatible]: ./media/iot-hub-devguide-messaging/eventhubcompatible.png

[lnk-resource-provider-apis]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-azure-gateway-guidance]: iot-hub-devguide-endpoints.md#field-gateways
[lnk-guidance-scale]: iot-hub-scaling.md
[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md
[lnk-amqp]: https://www.amqp.org/
[lnk-event-hubs]: http://azure.microsoft.com/documentation/services/event-hubs/
[lnk-event-hubs-consuming-events]: ../event-hubs/event-hubs-programming-guide.md#event-consumers
[lnk-management-portal]: https://portal.azure.com
[lnk-servicebus]: http://azure.microsoft.com/documentation/services/service-bus/
[lnk-eventhub-partitions]: ../event-hubs/event-hubs-overview.md#partitions
[lnk-portal]: iot-hub-create-through-portal.md

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-c2d]: iot-hub-devguide-messaging.md#cloud-to-device-messages
[lnk-compatible-endpoint]: iot-hub-devguide-messaging.md#read-device-to-cloud-messages
[lnk-protocols]: iot-hub-devguide-messaging.md#communication-protocols
[lnk-message-format]: iot-hub-devguide-messaging.md#message-format
[lnk-d2c-configuration]: iot-hub-devguide-messaging.md#device-to-cloud-configuration-options
[lnk-device-properties]: iot-hub-devguide-identity-registry.md#device-identity-properties
[lnk-ttl]: iot-hub-devguide-messaging.md#message-expiration-time-to-live
[lnk-c2d-configuration]: iot-hub-devguide-messaging.md#cloud-to-device-configuration-options
[lnk-lifecycle]: iot-hub-devguide-messaging.md#message-lifecycle
[lnk-feedback]: iot-hub-devguide-messaging.md#message-feedback
[lnk-antispoofing]: iot-hub-devguide-messaging.md#anti-spoofing-properties
[lnk-compare]: iot-hub-compare-event-hubs.md

[lnk-devguide-upload]: iot-hub-devguide-file-upload.md
[lnk-devguide-identities]: iot-hub-devguide-identity-registry.md
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-servicebus-sdk]: https://www.nuget.org/packages/WindowsAzure.ServiceBus
[lnk-eventprocessorhost]: http://blogs.msdn.com/b/servicebus/archive/2015/01/16/event-processor-host-best-practices-part-1.aspx


[lnk-getstarted-tutorial]: iot-hub-csharp-csharp-getstarted.md
[lnk-c2d-tutorial]: iot-hub-csharp-csharp-c2d.md
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md


<!--HONumber=Oct16_HO2-->


