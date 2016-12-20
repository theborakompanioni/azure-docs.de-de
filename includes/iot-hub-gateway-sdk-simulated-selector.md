> [!div class="op_single_selector"]
> * [Linux](../articles/iot-hub/iot-hub-linux-gateway-sdk-simulated-device.md)
> * [Windows](../articles/iot-hub/iot-hub-windows-gateway-sdk-simulated-device.md)
> 
> 

Diese exemplarische Vorgehensweise für das [Beispiel für Clouduploads von simulierten Geräten] zeigt, wie Sie das [Azure IoT-Gateway SDK][lnk-sdk] verwenden, um Gerät-zu-Cloud-Telemetriedaten von simulierten Geräten an IoT Hub zu senden.

Diese Anleitung umfasst:

1. **Architektur**: wichtige Informationen zur Architektur des Beispiels für Clouduploads von simulierten Geräten.
2. **Erstellen und Ausführen**: die zum Erstellen und Ausführen des Beispiels erforderlichen Schritte.

## <a name="architecture"></a>Architektur
Das Beispiel für Clouduploads von simulierten Geräten zeigt, wie Sie das SDK zum Erstellen eines Gateways verwenden, das Telemetriedaten von simulierten Geräten an einen IoT Hub sendet. Die simulierten Geräte können aus folgenden Gründen nicht direkt mit IoT Hub verbunden werden:

* Die Geräte verwenden kein Kommunikationsprotokoll, das von IoT Hub gelesen werden kann.
* Die Geräte verfügen nicht über die notwendigen intelligenten Funktionen, um sich die ihnen von IoT Hub zugewiesene Identität zu merken.

Das Gateway löst diese Probleme für die simulierten Geräte auf folgende Weise:

* Das Gateway liest das von den simulierten Geräten verwendete Protokoll, empfängt D2C-Telemetriedaten von den Geräten und leitet diese Nachrichten an IoT Hub über ein Protokoll weiter, das vom IoT-Hub gelesen werden kann.
* Das Gateway speichert IoT Hub-Identitäten für die simulierten Geräte und fungiert als Proxy, wenn die simulierten Geräte Nachrichten an IoT Hub senden.

Das folgende Diagramm zeigt die Hauptkomponenten des Beispiels, einschließlich der Gatewaymodule:

![][1]

> [!NOTE]
> Die Module übergeben untereinander Nachrichten nicht direkt. Die Module veröffentlichen Nachrichten in einem internen Broker, der die Nachrichten mithilfe eines Abonnementmechanismus an die anderen Module übermittelt, wie im unten stehenden Diagramm gezeigt. Weitere Informationen finden Sie unter [Erste Schritte mit dem Gateway SDK][lnk-gw-getstarted].
> 
> 

### <a name="protocol-ingestion-module"></a>Protokollerfassungsmodul
Dieses Modul ist der Ausgangspunkt für das Übermitteln von Daten von Geräten über das Gateway in die Cloud. Im Beispiel führt das Modul vier Aufgaben aus:

1. Es erstellt simulierte Temperaturdaten. Beachten Sie, dass wenn Sie reale Geräte verwenden, das Modul die Daten aus diesen physischen Geräten liest.
2. Es platziert die simulierten Temperaturdaten in den Inhalt einer Nachricht.
3. Es fügt der Nachricht mit den simulierten Temperaturdaten eine Eigenschaft mit einer MAC-Pseudoadresse hinzu.
4. Es stellt dem nächsten Modul in der Kette die Nachricht zur Verfügung.

> [!NOTE]
> Das im Diagramm oben als **Protocol X ingestion** bezeichnete Modul ist ein **simuliertes Gerät** im Quellcode.
> 
> 

### <a name="mac-lt-gt-iot-hub-id-module"></a>Modul zur Zuordnung zwischen MAC-Adressen und IoT Hub-IDs&lt;-&gt;
Dieses Modul sucht nach Nachrichten mit einer Eigenschaft, die die vom Protokollerfassungsmodul hinzugefügte MAC-Adresse der simulierten Geräte-App enthält. Wenn das Modul eine solche Eigenschaft findet, fügt es der Nachricht eine weitere Eigenschaft mit einem IoT Hub-Geräteschlüssel hinzu und stellt die Nachricht dann dem nächsten Modul in der Kette zur Verfügung. So verknüpft das Beispiel IoT Hub-Geräteeigenschaften mit simulierten Geräten. Der Entwickler richtet die Zuordnung zwischen MAC-Adressen und IoT Hub-Identitäten manuell im Rahmen der Modulkonfiguration ein. 

> [!NOTE]
> Dieses Beispiel verwendet eine MAC-Adresse als eindeutige Geräte-ID und korreliert sie mit einer IoT Hub-Geräteidentität. Sie können jedoch auch selbst ein Modul schreiben, das eine andere eindeutige ID verwendet. Sie können z.B. Geräte mit eindeutigen Seriennummern oder Telemetriedaten mit eingebetteten eindeutigen Gerätenamen verwenden, um die IoT Hub-Geräteidentität zu bestimmen.
> 
> 

### <a name="iot-hub-communication-module"></a>IoT Hub-Kommunikationsmodul
Dieses Modul akzeptiert Nachrichten mit einer vom vorherigen Modul zugewiesenen IoT Hub-Geräteidentität und sendet den Nachrichteninhalt über HTTP an IoT Hub. HTTP ist eines der drei Protokolle, die von IoT Hub gelesen werden können.

Anstatt für jedes simulierte Gerät eine Verbindung mit IoT Hub zu öffnen, öffnet dieses Modul eine einzelne HTTP-Verbindung zwischen Gateway und dem IoT-Hub und stellt über diese Verbindung im Multiplexverfahren Verbindungen von allen simulierten Geräte-Apps her. So kann ein einzelnes Gateway wesentlich mehr (simulierte oder nicht simulierte) Geräte miteinander verbinden, als es durch Öffnen einer eindeutigen Verbindung für jedes Gerät möglich wäre.

![][2]

<!-- Images -->
[1]: media/iot-hub-gateway-sdk-simulated-selector/image1.png
[2]: media/iot-hub-gateway-sdk-simulated-selector/image2.png

<!-- Links -->
[Beispiel für Clouduploads von simulierten Geräten]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/sample_simulated_device_cloud_upload.md
[lnk-sdk]: https://github.com/Azure/azure-iot-gateway-sdk
[lnk-gw-getstarted]: ../articles/iot-hub/iot-hub-linux-gateway-sdk-get-started.md

<!--HONumber=Nov16_HO5-->


