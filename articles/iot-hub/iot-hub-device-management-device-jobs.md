---
title: 'IoT Hub-Geräteverwaltung: Geräteaufträge | Microsoft Docs'
description: Im Tutorial zur Azure IoT Hub-Geräteverwaltung wird beschrieben, wie Geräteaufträge verwendet werden, um Vorgänge wie Remotefirmwareupdates auszuführen.
services: iot-hub
documentationcenter: .net
author: juanjperez
manager: timlt
editor: ''

ms.service: iot-hub
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/29/2016
ms.author: juanpere

---
# Tutorial: Aktualisieren der Gerätefirmware mithilfe von Geräteaufträgen (Vorschau)
[!INCLUDE [iot-hub-device-management-job-selector](../../includes/iot-hub-device-management-jobs-selector.md)]

## Einführung
Die Azure IoT-Geräteverwaltung ermöglicht Ihnen, mithilfe von Geräteaufträgen mit physischen Geräten zu interagieren. Nachdem Sie den Gerätezwilling (die Dienstdarstellung eines physischen Geräts) identifiziert haben, können Sie mithilfe von Geräteaufträgen mit dem entsprechenden physischen Gerät interagieren. Geräteaufträge aktivieren die Koordination von komplexen Prozessen auf mehreren Geräten. Dieser Prozess kann mehrere Schritte und Vorgänge mit langer Laufzeit enthalten.

Es gibt sechs Typen von Geräteaufträgen, die zurzeit von der Azure IoT Hub-Geräteverwaltung bereitgestellt werden (es werden zusätzliche Aufträge hinzugefügt, sobald Kunden sie benötigen):

* **Firmwareupdate**: Dient zum Aktualisieren der Firmware (bzw. des Betriebssystemimage) auf dem physischen Gerät.
* **Neustart**: Startet das physische Gerät neu.
* **Zurücksetzung auf Werkseinstellungen**: Setzt die Firmware (bzw. das Betriebssystemimage) des physischen Geräts auf ein ab Werk bereitgestelltes Sicherungsimage zurück, das auf dem Gerät gespeichert ist.
* **Konfigurationsaktualisierung**: Konfiguriert den IoT Hub-Client-Agent, der auf dem physischen Gerät ausgeführt wird.
* **Geräteeigenschaft lesen**: Dient zum Abrufen des aktuellsten Werts einer Geräteeigenschaft auf dem physischen Gerät.
* **Geräteeigenschaft schreiben**: Ändert eine Geräteeigenschaft auf dem physischen Gerät.

Weitere Informationen zur Verwendung der einzelnen Aufträge finden Sie in der [API-Dokumentation][lnk-apidocs].

Sie können den Auftragsverlauf abfragen, um den Status von Aufträgen zu erkennen, die Sie gestartet haben. Einige Beispiele für Abfragen finden Sie in [unserer Bibliothek mit Abfrageausdrücken][lnk-query-samples].

## Verwenden von Geräteaufträgen zum Ausführen von Firmwareupdates: Architektur
Das folgende Diagramm zeigt eine Interaktion eines Firmwareupdate-Geräteauftrags mit einem einzigen physischen Gerät.

![][img-architecture]

Schritte für den Firmwareupdate-Geräteauftrag:

1. Die cloudbasierte IoT-Lösung startet den Firmwareupdate-Geräteauftrag und stellt den URI für den Speicherort des Firmwarepakets bereit. Es liegt in der Verantwortung der IoT-Lösung, das Firmwarepaket an einem Speicherort zu platzieren, von dem Sie das Gerät herunterladen können.
2. Wenn das physische Gerät diesen URI empfängt, beginnt es automatisch, vom angegebenen URI aus herunterzuladen.
3. Der Code auf dem Gerät empfängt die Anforderung vom IoT Hub und lädt das Firmwarepaket aus dem angegebenen Speicherort-URI.
4. Nachdem der Geräteauftrag die Gerätestatusnachricht über den abgeschlossenen Download erhalten hat, weist er das Gerät an, das heruntergeladene Firmwareimage anzuwenden.
5. Nachdem das Gerät das Firmwareimage angewendet hat, startet es neu, verbindet sich erneut mit IoT Hub und teilt IoT Hub mit, dass die neue Firmware angewendet wurde und der Geräteauftrag nun abgeschlossen ist.

## Ausführen des Beispiels des Firmwareupdates
Das folgende Beispiel erweitert die Funktionalität des Tutorials [Erste Schritte mit der Azure IoT Hub-Geräteverwaltung mithilfe von C# (Vorschau)][lnk-get-started]. Auf der Basis, dass die verschiedenen simulierten Geräte ausgeführt werden, wird ein Auftrag zum Aktualisieren der Firmware auf allen Geräten gestartet.

### Voraussetzungen
Vor dem Ausführen dieses Beispiels müssen Sie die Schritte in [Erste Schritte mit der Azure IoT Hub-Geräteverwaltung mithilfe von C# (Vorschau)][lnk-get-started] abgeschlossen haben. Das bedeutet, dass Ihre simulierten Geräte ausgeführt werden müssen. Wenn Sie den Vorgang zuvor abgeschlossen haben, starten Sie Ihre simulierten Geräte jetzt neu.

### Starten des Beispiels
Um das Beispiel zu starten, müssen Sie den Prozess **FirmwareUpdate.exe** ausführen. Damit wird die Aktualisierung der Firmware auf allen simulierten Geräten gestartet. Folgen Sie den Schritten unten, um das Beispiel zu starten:

1. Navigieren Sie aus dem Stammordner, in dem Sie das Repository **azure-iot-sdks** geklont haben, zum Ordner **azure-iot-sdks\\csharp\\service\\samples\\bin**.
2. Führen Sie `FirmwareUpdate.exe <IoT Hub Connection String>` aus.

Im Befehlszeilenfenster sollten sieben Geräteaufträge angezeigt werden, die das simulierte Firmwareupdate für die sechs simulierten Geräte nachverfolgen.

### Starten eines Geräteauftrags
Im Allgemeinen werden Geräteaufträge mit einer Reihe von **Schedule&lt;JobName&gt;Async**-Methoden auf der **JobClient**-Instanz gestartet. Im Firmwareupdatebeispiel rufen wir die **ScheduleFirmwareUpdateAsync**-Methode auf. Da wir ein Array mit 6 Geräte-IDs übergeben, werden 7 Geräteaufträge gestartet, einer für jedes Gerät, das aktualisiert wird, und einer für ein übergeordnetes Gerät, das zum Nachverfolgen der anderen 6 verwendet wird.

Im folgenden Codeausschnitt aus **Program.cs** im **FirmwareUpdate**-Projekt wird ein Firmwareupdateauftrag gestartet. Der Aufruf verwendet ein Zeichenfolgenarray von **deviceId**-Werten als Parameter, die die Geräte darstellen, die wir aktualisieren möchten.

```
var jobResponse = await deviceJobClient.ScheduleFirmwareUpdateAsync(Guid.NewGuid().ToString(), deviceIds, packageURI, TimeSpan.FromMinutes(25));
```

### Abfragen des Auftragsverlaufs
Im Beispiel wird eine Liste der Geräte, die aktiv einen Geräteauftrag ausführen, in regelmäßigen Abständen angezeigt. Wenn ein Geräteauftrag abgeschlossen ist, wird das betreffende Gerät aus der angezeigten Liste entfernt. Die folgende Abbildung ist ein Screenshot der Ausführung von **FirmwareUpdate.exe**:

![][img-output1]

Die obige Liste wird durch Abfragen für alle aktiven Aufträge generiert, wie im nachstehenden Codeausschnitt aus **Program.cs** des **FirmwareUpdate**-Projekts gezeigt:

```
private static async Task OutputRunningJobs()
{
  JobClient deviceJobClient = JobClient.CreateFromConnectionString(connectionString);

  string json = JsonConvert.SerializeObject(
    new
    {
      filter = new
      {
        property = new
        {
          name = "Status",
          type = "default"
        },
        value = "Running",
        comparisonOperator = "eq",
        type = "comparison"
      }
    }
  );

  JobQueryResult result = await deviceJobClient.QueryJobHistoryJsonAsync(json);

  // List query result
  foreach (JobResponse job in result.Result)
  {
    Console.WriteLine("DeviceID: {0}\t\tJobId: {1}",
      job.DeviceId,
      job.JobId
      );
  }
}
```

Wenn der übergeordnete Geräteauftrag abgeschlossen ist, gibt das Beispiel eine Liste aller Geräteaufträge aus. Dies wird in der Abbildung unten dargestellt. Der übergeordnete Auftrag wird nur abgeschlossen, nachdem alle zugehörigen untergeordneten Aufträge abgeschlossen sind. Im folgenden Screenshot ist der übergeordnete Auftrag der letzte Auftrag in der Liste, was Sie daran erkennen, dass **ParentJobId** gleich **''** lautet. Im übergeordneten Auftrag ist das Feld **Reason** auch auf eine Zeichenfolge festgelegt, die das aggregierte Ergebnis der Abfrage angibt. In diesem Fall gibt sie an, dass alle 6 Geräte erfolgreich aktualisiert wurden.

![][img-output2]

Die obige Liste wird durch Abfragen für alle Aufträge und Sortieren nach Startzeit generiert. Jede Eigenschaft des unten dargestellten **JobResponse**-Objekts kann zum Abfragen des Geräteauftragsverlaufs verwendet werden.

![][img-properties]

Weitere Beispiele für das Abfragen des Geräteauftragsverlaufs finden Sie in [unserer Bibliothek mit Abfrageausdrücken][lnk-query-samples].

### Details zur Gerätesimulatorimplementierung
In den vorherigen Abschnitten zeigten wir die Details der Implementierung des Firmwareupdates vom Dienststandpunkt aus (Starten eines Geräteauftrags und Abfragen seines Status). Nun erörtern wir die entsprechende Implementierung von der Geräteseite aus betrachtet.

Die Clientbibliothek der Azure IoT Hub-Geräteverwaltung übernimmt die Kommunikation zwischen dem Gerät und dem Dienst, sodass nur noch die Implementierung der gerätespezifischen Logik durchgeführt werden muss. Diese besteht aus zwei Teilen:

* Implementieren des gerätespezifischen Firmwareupdateprozesses: Umfasst das Schreiben der gerätespezifischen Logik, um das Firmwarepaket herunterzuladen, und Anwenden des Updates in den entsprechenden unten aufgeführten Rückrufen. Im simulierten Beispiel ist dies im [Beispiel][lnk-github-firmware] implementiert:
  
  ```
  object_firmwareupdate *obj = get_firmwareupdate_object(0);
  obj->firmwareupdate_packageuri_write_callback = start_firmware_download;
  // platform specific code
  obj->firmwareupdate_update_execute_callback = start_firmware_update;
  //platform specific code
  ```
* Informieren Sie den Dienst des Firmwareupdateprozesses: Dies schließt die Änderung des Firmwareupdatestatus und der Firmwareupdate-Ergebnisfelder ein. Sie erreichen dies durch Aufrufen der **set\_firmwareupdate\_state**- und **set\_firmwareupdate\_updateresult**-API. Im simulierten Beispiel ist dies im [Beispiel][lnk-github-firmware] implementiert:

## Nächste Schritte
Weitere Informationen zu den Geräteverwaltungsfeatures von Azure IoT Hub erhalten Sie in den folgenden Tutorials:

* [Aktivieren verwalteter Geräte hinter einem IoT-Gateway][lnk-dm-gateway]
* [Einführung in die Clientbibliothek der Azure IoT Hub-Geräteverwaltung][lnk-library-c]
* Die Clientbibliothek der Azure IoT Hub-Geräteverwaltung bietet ein End-to-End-Beispiel mit einem [Intel Edison-Gerät][lnk-edison].

Weitere Informationen zu den Funktionen von IoT Hub finden Sie unter:

* [Entwerfen Ihrer Lösung][lnk-design]
* [Entwicklerhandbuch][lnk-devguide]
* [Simulieren eines Geräts mit dem Gateway SDK][lnk-gateway]
* [Verwenden des Azure-Portals zur Verwaltung von IoT Hub][lnk-portal]

<!-- Images and links -->

[img-architecture]: media/iot-hub-device-management-device-jobs/image1.png
[img-output1]: media/iot-hub-device-management-device-jobs/image2.png
[img-output2]: media/iot-hub-device-management-device-jobs/image3.png
[img-properties]: media/iot-hub-device-management-device-jobs/image4.png

[lnk-apidocs]: iot-hub-sdks-summary.md
[lnk-twin-tutorial]: iot-hub-device-management-device-twin.md
[lnk-tutorial-queries]: iot-hub-device-management-device-query.md
[lnk-edison]: https://github.com/Azure/azure-iot-sdks/tree/dmpreview/c/iotdm_client/samples/iotdm_edison_sample
[lnk-get-started]: iot-hub-device-management-get-started.md
[lnk-github-firmware]: https://github.com/Azure/azure-iot-sdks/blob/dmpreview/c/iotdm_client/samples/iotdm_simple_sample/iotdm_simple_sample.c
[lnk-query-samples]: https://github.com/Azure/azure-iot-sdks/blob/dmpreview/doc/get_started/dm_queries/query-samples.md

[lnk-dm-gateway]: iot-hub-gateway-device-management.md
[lnk-library-c]: iot-hub-device-management-library.md

[lnk-design]: iot-hub-guidance.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
[lnk-portal]: iot-hub-manage-through-portal.md

<!---HONumber=AcomDC_0713_2016-->