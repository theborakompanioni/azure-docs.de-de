> [!div class="op_single_selector"]
> * [Device: Node.js Service: Node.js](../articles/iot-hub/iot-hub-node-node-device-management-get-started.md)
> * [Device: Node.js Service: C#](../articles/iot-hub/iot-hub-csharp-node-device-management-get-started.md)
> * [Gerät: Java Dienst: Java](../articles/iot-hub/iot-hub-java-java-device-management-getstarted.md)

Back-End-Apps können Azure IoT Hub-Grundtypen (etwa [Gerätezwillingsmethode][lnk-devtwin] und [direkte Methode][lnk-c2dmethod]) verwenden, um Geräteverwaltungsaktionen auf Geräten remote zu starten und zu überwachen. In diesem Tutorial wird veranschaulicht, wie eine Back-End-App und eine Gerät-App zusammen verwendet werden können, um einen Remoteneustart des Geräts mit IoT Hub zu initiieren und zu überwachen.

Verwenden Sie eine direkte Methode, um Geräteverwaltungsaktionen (wie Neustarts, Zurücksetzen auf Werkseinstellungen und Firmwareaktualisierung) von einer Back-End-App aus in der Cloud zu initiieren. Das Gerät ist für Folgendes verantwortlich:

* Verarbeiten der von IoT Hub gesendeten Methodenanforderung
* Initiieren der entsprechenden gerätespezifischen Aktion auf dem Gerät.
* Senden von Statusupdates über die *gemeldeten Eigenschaften* an IoT Hub

Sie können eine Back-End-App in der Cloud verwenden, um Gerätezwillingsabfragen auszuführen und Berichte zum Status der Geräteverwaltungsaktionen zu erstellen.

[lnk-devtwin]: ../articles/iot-hub/iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
