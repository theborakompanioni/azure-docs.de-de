> [!div class="op_single_selector"]
> * [Node.js](../articles/iot-hub/iot-hub-node-node-twin-getstarted.md)
> * [C#](../articles/iot-hub/iot-hub-csharp-node-twin-getstarted.md)
> 
> 

## <a name="introduction"></a>Einführung
Gerätezwillinge sind JSON-Dokumente, in denen Gerätestatusinformationen (Metadaten, Konfigurationen und Bedingungen) gespeichert werden. IoT Hub speichert einen Gerätezwilling für jedes Gerät, das Sie mit IoT Hub verbinden.

Verwenden Sie Gerätezwillinge für Folgendes:

* Speichern von Gerätemetadaten von Ihrem Lösungs-Back-End
* Melden von aktuellen Zustandsinformationen, z.B. verfügbare Funktionen und Bedingungen (wie die verwendete Verbindungsmethode), von Ihrer Geräte-App
* Synchronisieren des Zustands von Workflows mit langer Ausführungsdauer (z.B. Firmware- und Konfigurationsupdates) zwischen einer Geräte-App und einer Back-End-App
* Abfragen von Metadaten, Konfiguration oder Status des Geräts

> [!NOTE]
> Gerätezwillinge sind für die Synchronisierung und zum Abfragen von Gerätekonfigurationen und -bedingungen ausgelegt. Weitere Informationen dazu, wann Sie Gerätezwillinge verwenden sollten, finden Sie unter [Grundlegendes zu Gerätezwillingen][lnk-twins].
> 
> 

Gerätezwillinge werden in einem IoT Hub gespeichert und enthalten Folgendes:

* *Tags:* Gerätemetadaten, auf die nur vom Lösungs-Back-End zugegriffen werden kann
* *Gewünschte Eigenschaften:* JSON-Objekte, die vom Lösungs-Back-End geändert und von der Geräte-App überwacht werden können
* *Gemeldete Eigenschaften:* JSON-Objekte, die von der Geräte-App geändert und vom Lösungs-Back-End gelesen werden können. Tags und Eigenschaften können keine Arrays enthalten, aber Objekte können geschachtelt werden.

![][img-twin]

Außerdem können mit dem Lösungs-Back-End Gerätezwillinge basierend auf allen obigen Daten abgefragt werden.
Weitere Informationen zu Gerätezwillingen finden Sie unter [Grundlegendes zu Gerätezwillingen][lnk-twins], und eine Referenz zu Abfragen finden Sie unter [IoT Hub-Abfragesprache][lnk-query].

> [!NOTE]
> Zurzeit ist der Zugriff auf Gerätezwillinge nur von Geräten aus möglich, die über das MQTT-Protokoll Verbindungen mit IoT Hub herstellen. Anweisungen zum Konvertieren vorhandener Geräte-Apps für die Verwendung von MQTT finden Sie im Artikel [MQTT-Unterstützung][lnk-devguide-mqtt].
> 
> 

Dieses Tutorial veranschaulicht folgende Vorgehensweisen:

* Erstellen Sie eine Back-End-App, mit der einem Gerätezwilling *Tags* hinzufügt werden, sowie eine simulierte Geräte-App, die ihren Konnektivitätskanal als *gemeldete Eigenschaft* auf dem Gerätezwilling meldet.
* Fragen Sie Geräte von Ihrer Back-End-App ab, indem Sie Filter für die zuvor erstellten Tags und Eigenschaften verwenden.

<!-- images -->
[img-twin]: media/iot-hub-selector-twin-get-started/twin.png

<!-- links -->
[lnk-query]: ../articles/iot-hub/iot-hub-devguide-query-language.md
[lnk-twins]: ../articles/iot-hub/iot-hub-devguide-device-twins.md
[lnk-d2c]: ../articles/iot-hub/iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: ../articles/iot-hub/iot-hub-mqtt-support.md

<!--HONumber=Feb17_HO1-->


