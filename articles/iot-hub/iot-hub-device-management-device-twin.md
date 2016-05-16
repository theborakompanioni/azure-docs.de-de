<properties
	pageTitle="Gerätezwillinge in der IoT Hub-Geräteverwaltung | Microsoft Azure"
	description="Tutorial zur Azure IoT Hub-Geräteverwaltung, in dem die Verwendung von Gerätezwillingen beschrieben wird."
	services="iot-hub"
	documentationCenter=".net"
	authors="ellenfosborne"
	manager="timlt"
	editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="04/29/2016"
 ms.author="elfarber"/>

# Tutorial: Verwenden des Gerätezwillings (Vorschau)

Die Azure IoT Hub-Geräteverwaltung führt den Gerätezwilling ein, eine dienstseitige Darstellung eines physischen Geräts. Das folgende Diagramm zeigt die verschiedenen Komponenten des Gerätezwillings.

![][img-twin]

In diesem Tutorial konzentrieren wir uns auf die Geräteeigenschaften. Weitere Informationen zu den anderen Komponenten finden Sie unter [Überblick über die Azure IoT Hub-Geräteverwaltung][lnk-dm-overview].

Geräteeigenschaften sind ein vordefiniertes Wörterbuch von Eigenschaften, die das physische Gerät beschreiben. Das physische Gerät ist der Master jeder Geräteeigenschaft und der autoritative Speicher jedes entsprechenden Werts. Eine konsistente Darstellung dieser Eigenschaften ist im Gerätezwilling und in der Cloud gespeichert. Die Kohärenz und Aktualität unterliegen den Synchronisierungseinstellungen, wie im Folgenden beschrieben. Einige Beispiele für Geräteeigenschaften sind Firmwareversion, Akkukapazität und der Namen des Herstellers.

## Synchronisierung von Geräteeigenschaften

Das physische Gerät ist die autoritative Quelle für die Geräteeigenschaften. Ausgewählte Werte auf dem physischen Gerät werden automatisch mit dem Gerätezwilling in IoT Hub über das von LWM2M beschriebene Muster *observe/notify* synchronisiert.

Wenn das physische Gerät eine Verbindung mit IoT Hub herstellt, initiiert der Dienst *Beobachtungen* der ausgewählten Geräteeigenschaften. Anschließend *benachrichtigt* das physische Gerät IoT Hub über Änderungen der Geräteeigenschaften. Zum Implementieren der Hysterese wird **pmin** (das Mindestintervall zwischen Benachrichtigungen) auf 5 Minuten festgelegt. Dies bedeutet, dass das physische Gerät für jede Eigenschaft IoT Hub nicht öfter als einmal alle 5 Minuten benachrichtigt, selbst wenn eine Änderung vorliegt. Um die Aktualität sicherzustellen, wird **pmax** (das Höchstintervall zwischen Benachrichtigungen) auf 6 Stunden festgelegt. Dies bedeutet, dass das physische Gerät für jede Eigenschaft IoT Hub mindestens einmal alle 6 Stunden benachrichtigt, selbst wenn keine Änderung vorliegt.

Wenn das physische Gerät die Verbindung trennt, endet die Synchronisierung. Die Synchronisierung startet neu, wenn das Gerät sich erneut mit dem Dienst verbindet. Sie können den Zeitpunkt der letzten Aktualisierung für eine Eigenschaft immer überprüfen, um die Aktualität sicherzustellen.

Die vollständige Liste der Geräteeigenschaften, die automatisch beobachtet werden, ist im Folgenden aufgeführt:

![][img-observed]

## Beispiel für das Ausführen des Gerätezwillings

Das folgende Beispiel legt die Funktionalität des Tutorials [Get started with Azure IoT Hub device management using C# (preview)][lnk-get-started] (Erste Schritte mit der Azure IoT Hub-Geräteverwaltung mit C# [Vorschau]) dar. Auf der Basis, dass die verschiedenen simulierten Geräte ausgeführt werden, wird der Gerätezwilling zum Lesen und Ändern von Eigenschaften auf einem simulierten Gerät verwendet.

### Voraussetzungen 

Vor dem Ausführen dieses Beispiels müssen Sie die Schritte in [Get started with Azure IoT Hub device management using C# (preview)][lnk-get-started] (Erste Schritte mit der Azure IoT Hub-Geräteverwaltung mit C# [Vorschau]) abgeschlossen haben. Das bedeutet, dass Ihre simulierten Geräte ausgeführt werden müssen. Wenn Sie den Vorgang zuvor abgeschlossen haben, starten Sie Ihre simulierten Geräte jetzt neu.

### Starten des Beispiels

Um das Beispiel zu starten, müssen Sie den Prozess **DeviceTwin.exe** ausführen. Er liest die Geräteeigenschaften aus dem Gerätezwilling und dem physischen Gerät. Er ändert auch eine Geräteeigenschaft auf dem physischen Gerät. Folgen Sie den Schritten unten, um das Beispiel zu starten:

1.  Navigieren Sie aus dem Stammordner, in dem Sie das Repository **azure-iot-sdks** geklont haben, zum Ordner **azure-iot-sdks\\csharp\\service\\samples\\bin**.  

2.  Führen Sie `DeviceTwin.exe <IoT Hub Connection String>` aus.

Im Befehlszeilenfenster sollte die Verwendung des Gerätezwillings angezeigt werden. Das Beispiel führt folgenden Prozess aus:

1.  Ausdruck aller Geräteeigenschaften auf einem Gerätezwilling.

2.  Tiefer Lesevorgang: Lesen der Akkustand-Geräteeigenschaft aus dem physischen Gerät (3 Mal).

3.  Tiefer Schreibvorgang: Schreiben der **Timezone**-Geräteeigenschaft auf dem physischen Gerät.

4.  Tiefer Lesevorgang: Lesen der **Timezone**-Geräteeigenschaft auf dem physischen Gerät, um festzustellen, ob sich etwas geändert hat.

### Flacher Lesevorgang

Es gibt einen Unterschied zwischen *flachen* Lesevorgängen und *tiefen* Lese- und Schreibvorgängen. Ein flacher Lesevorgang gibt den Wert der angeforderten Eigenschaft aus dem in Azure IoT Hub gespeicherten Gerätezwilling zurück. Das ist der Wert aus dem vorherigen Benachrichtigungsvorgang. Sie können keinen flachen Schreibvorgang durchführen, weil das physische Gerät die autoritative Quelle für die Geräteeigenschaften ist. Ein flacher Lesevorgang besteht einfach im Lesen der Eigenschaft aus dem Gerätezwilling:

```
device.DeviceProperties[DevicePropertyNames.BatteryLevel].Value.ToString();
```

Um die Aktualität dieser Werte zu ermitteln, können Sie den Zeitpunkt der letzten Aktualisierung überprüfen:

```
device.DeviceProperties[DevicePropertyNames.BatteryLevel].LastUpdatedTime.ToString();
```

Sie können auf ähnliche Weise Diensteigenschaften lesen, die nur im Gerätezwilling gespeichert sind. Sie sind nicht mit dem Gerät synchronisiert.

### Tiefer Lesevorgang

Ein tiefer Lesevorgang startet einen Geräteauftrag, den Wert der angeforderten Eigenschaft aus dem physischen Gerät zu lesen. Geräteaufträge wurden in [Übersicht zur Azure IoT Hub-Geräteverwaltung (Vorschau)][lnk-dm-overview] eingeführt und werden in [Tutorial: Aktualisieren der Gerätefirmware mithilfe von Geräteaufträgen (Vorschau)][lnk-dm-jobs] ausführlich beschrieben. Der tiefe Lesevorgang liefert Ihnen einen aktuelleren Wert der Geräteeigenschaft, da die Aktualität des Benachrichtigungsintervalls nicht beschränkt ist. Der Auftrag sendet eine Nachricht an das physische Gerät und aktualisiert den Gerätezwilling nur für die angegebene Eigenschaft mit dem aktuellen Wert. Es wird nicht der gesamte Gerätezwilling aktualisiert.

```
JobResponse jobResponse = await deviceJobClient.ScheduleDevicePropertyReadAsync(Guid.NewGuid().ToString(), deviceId, propertyToRead);
```

Sie können keinen tiefen Lesevorgang auf Diensteigenschaften oder Tags anwenden, da sie nicht mit dem Gerät synchronisiert sind.

### Tiefer Schreibvorgang

Wenn Sie eine beschreibbare Geräteeigenschaft ändern möchten, können Sie hierzu einen tiefen Schreibvorgang anwenden, der einen Geräteauftrag zum Schreiben des Werts in das physische Gerät startet. Nicht alle Geräteeigenschaften sind beschreibbar. Eine vollständige Liste finden Sie in Anhang A von [Introducing the Azure IoT Hub device management client library][lnk-dm-library] (Einführung in die Clientbibliothek der Azure IoT Hub-Geräteverwaltung).

Der Auftrag sendet eine Nachricht an das physische Gerät, um die angegebene Eigenschaft zu aktualisieren. Der Gerätezwilling wird nach Abschluss des Auftrags nicht sofort aktualisiert. Sie müssen bis zum nächsten Benachrichtigungsintervall warten. Sobald die Synchronisierung erfolgt, können Sie die Änderung im Gerätezwilling mit einem flachen Lesevorgang erkennen.

```
JobResponse jobResponse = await deviceJobClient.ScheduleDevicePropertyWriteAsync(Guid.NewGuid().ToString(), deviceId, propertyToSet, setValue);
```

### Details zur Gerätesimulatorimplementierung

Wir untersuchen nun, was Sie auf der Geräteseite zum Implementieren des „observe/notify“-Musters und von tiefen Lese-/Schreibvorgängen tun müssen.

Da die Synchronisierung der Geräteeigenschaften vollständig über die Clientbibliothek der Azure IoT Hub-Geräteverwaltung durchgeführt wird, müssen Sie lediglich die API aufrufen, um die Geräteeigenschaft (in diesem Beispiel die Akkukapazität) in regelmäßigen Intervallen festzulegen. Wenn der Dienst einen tiefen Lesevorgang ausführt, wird der letzte von Ihnen festgelegte Wert zurückgegeben. Wenn der Dienst einen tiefen Schreibvorgang ausführt, wird diese Set-Methode aufgerufen. In **iotdm\_simple\_sample.c** sehen Sie ein Beispiel dafür:

```
int level = get_batterylevel();  // call to platform specific code 
set_device_batterylevel(0, level);
```

Anstatt die Set-Methode zu verwenden, könnten Sie einen Rückruf implementieren. Weitere Informationen zu dieser Option finden Sie unter [Introducing the Azure IoT Hub device management client library][lnk-dm-library] (Einführung in die Clientbibliothek der Azure IoT Hub-Geräteverwaltung).

## Nächste Schritte

Weitere Informationen zu den Geräteverwaltungsfeatures von Azure IoT Hub erhalten Sie in den folgenden Tutorials:

- [Suchen nach Gerätezwillingen mithilfe von Abfragen][lnk-tutorial-queries]

- [Aktualisieren der Gerätefirmware mithilfe von Geräteaufträgen][lnk-dm-jobs]

- Die Clientbibliothek der Geräteverwaltung bietet ein End-to-End-Beispiel mit einem [Intel Edison-Gerät][lnk-edison].

<!-- images and links -->
[img-twin]: media/iot-hub-device-management-device-twin/image1.png
[img-observed]: media/iot-hub-device-management-device-twin/image2.png

[lnk-dm-overview]: iot-hub-device-management-overview.md
[lnk-dm-library]: iot-hub-device-management-library.md
[lnk-get-started]: iot-hub-device-management-get-started.md
[lnk-tutorial-queries]: iot-hub-device-management-device-query.md
[lnk-dm-jobs]: iot-hub-device-management-device-jobs.md
[lnk-edison]: https://github.com/Azure/azure-iot-sdks/tree/dmpreview/c/iotdm_client/samples/iotdm_edison_sample

<!---HONumber=AcomDC_0504_2016-->