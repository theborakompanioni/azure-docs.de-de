---
title: "Durchführen eines Firmwareupdates mit Azure IoT Hub | Microsoft Docs"
description: "In diesem Tutorial erfahren Sie, wie Sie eine Aktualisierung der Firmware durchführen."
services: iot-hub
documentationcenter: .net
author: juanjperez
manager: timlt
editor: 
ms.assetid: 70b84258-bc9f-43b1-b7cf-de1bb715f2cf
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/17/2016
ms.author: juanpere
translationtype: Human Translation
ms.sourcegitcommit: 00746fa67292fa6858980e364c88921d60b29460
ms.openlocfilehash: 7cbb823f2d8b9a337bd987ae3fc41a85ddb6ae80


---
# <a name="tutorial-how-to-do-a-firmware-update"></a>Tutorial: Durchführen eines Firmwareupdates
[!INCLUDE [iot-hub-selector-firmware-update](../../includes/iot-hub-selector-firmware-update.md)]

## <a name="introduction"></a>Einführung
Im Tutorial [Get started with device management][lnk-dm-getstarted] (Erste Schritte mit der Geräteverwaltung) wurde erläutert, wie Sie mit einem [Gerätezwilling][lnk-devtwin] und [direkten Methoden][lnk-c2dmethod] Grundtypen für den Remoteneustart eines Geräts verwenden können. In diesem Tutorial werden die gleichen IoT Hub-Grundtypen verwendet, und Sie sehen, wie Sie ein simuliertes End-to-End-Firmwareupdate durchführen.  Dieses Muster wird in der Firmwareupdateimplementierung für das [Implementierungsbeispiel mit dem Raspberry Pi-Gerät][lnk-rpi-implementation] verwendet.

Dieses Tutorial veranschaulicht folgende Vorgehensweisen:

* Erstellen einer Konsolen-App, die die direkte firmwareUpdate-Methode auf der simulierten Geräte-App über Ihren IoT-Hub aufruft.
* Erstellen einer simulierten Geräte-App, die eine direkte firmwareUpdate-Methode implementiert, die einen mehrstufigen Prozess durchläuft und über diesen auf das Herunterladen des Firmwareimages wartet, das Firmwareimage herunterlädt und schließlich das Firmwareimage übernimmt.  Während der gesamten Ausführung aller Phasen aktualisiert das Gerät den Fortschritt über die gemeldeten Eigenschaften.

Am Ende dieses Tutorials verfügen Sie über eine Node.js-Konsolen-Geräte-App und eine .NET-Konsolen-Back-End-App (C#):

**dmpatterns_fwupdate_service.js**, die auf der simulierten Geräte-App eine direkte Methode aufruft, die Antwort anzeigt und in regelmäßigen Abständen (alle 500 ms) die aktualisierten gemeldeten Eigenschaften anzeigt.

**TriggerFWUpdate**, die über die zuvor erstellte Geräteidentität eine Verbindung mit dem IoT Hub herstellt, eine direkte firmwareUpdate-Methode empfängt und einen mehrstufigen Prozess zum Simulieren eines Firmwareupdates durchläuft, bei dem auf das Herunterladen des Images gewartet, das Image heruntergeladen und das Image schließlich übernommen wird.

Für dieses Tutorial benötigen Sie Folgendes:

* Microsoft Visual Studio 2015.
* Node.js Version 0.12.x oder höher, <br/>  Unter [Prepare your development environment][lnk-dev-setup] (Vorbereiten Ihrer Entwicklungsumgebung) wird beschrieben, wie Sie Node.js für dieses Tutorial unter Windows oder Linux installieren.
* Ein aktives Azure-Konto. (Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein [kostenloses Konto][lnk-free-trial] erstellen.)

Befolgen Sie die Schritte im Artikel [Erste Schritte mit der Geräteverwaltung](iot-hub-csharp-node-device-management-get-started.md), um Ihren IoT Hub zu erstellen und die Verbindungszeichenfolge abzurufen.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="trigger-a-remote-firmware-update-on-the-device-using-a-direct-method"></a>Auslösen eines Remotefirmwareupdates auf dem Gerät über eine direkte Methode
In diesem Abschnitt erstellen Sie eine .NET-Konsolen-App (mit C#), die mit einer direkten Methode ein Remotefirmwareupdate auf einem Gerät initiiert und mithilfe von Gerätezwillingsabfragen in regelmäßigen Abständen den Status des aktiven Firmwareupdates auf diesem Gerät abruft.

1. Fügen Sie in Visual Studio in der aktuellen Projektmappe mithilfe der Projektvorlage **Konsolenanwendung** ein Visual C#-Projekt für den klassischen Windows-Desktop hinzu. Nennen Sie das Projekt **TriggerFWUpdate**.

    ![Neues Visual C#-Projekt für den klassischen Windows-Desktop][img-createapp]

2. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt **TriggerFWUpdate**, und klicken Sie anschließend auf **NuGet-Pakete verwalten**.
3. Wählen Sie im Fenster **NuGet-Paket-Manager** die Option **Durchsuchen**, suchen Sie nach **microsoft.azure.devices**, wählen Sie zum Installieren des Pakets **Microsoft.Azure.Devices** die Option **Installieren**, und akzeptieren Sie die Nutzungsbedingungen. Bei diesem Verfahren werden das NuGet-Paket für das [Dienst SDK für Microsoft Azure IoT][lnk-nuget-service-sdk] sowie die dazugehörigen Abhängigkeiten heruntergeladen, installiert und mit einem Verweis versehen.

    ![Fenster „NuGet-Paket-Manager“][img-servicenuget]
4. Fügen Sie am Anfang der Datei **Program.cs** die folgenden `using`-Anweisungen hinzu:
   
        using Microsoft.Azure.Devices;
        
5. Fügen Sie der **Program** -Klasse die folgenden Felder hinzu. Ersetzen Sie den mehrfachen Platzhalterwert durch die Verbindungszeichenfolge für den IoT Hub, den Sie im vorherigen Abschnitt erstellt haben.
   
        static RegistryManager registryManager;
        static string connString = "{iot hub connection string}";
        static ServiceClient client;
        static JobClient jobClient;
        static string targetDevice = "{deviceIdForTargetDevice}";
        
6. Fügen Sie der **Program** -Klasse die folgende Methode hinzu:
   
        public static async Task QueryTwinFWUpdateReported()
        {
            Twin twin = await registryManager.GetTwinAsync(targetDevice);
            Console.WriteLine(twin.Properties.Reported.ToJson());
        }
        
7. Fügen Sie der **Program** -Klasse die folgende Methode hinzu:

        public static async Task StartFirmwareUpdate()
        {
            client = ServiceClient.CreateFromConnectionString(connString);
            CloudToDeviceMethod method = new CloudToDeviceMethod("firmwareUpdate");
            method.ResponseTimeout = TimeSpan.FromSeconds(30);
            method.SetPayloadJson(
                @"{
                    fwPackageUri : 'https://someurl'
                }");

            CloudToDeviceMethodResult result = await client.InvokeDeviceMethodAsync(targetDevice, method);

            Console.WriteLine("Invoked firmware update on device.");
        }

7. Fügen Sie abschließend der **Main** -Methode die folgenden Zeilen hinzu:
   
        registryManager = RegistryManager.CreateFromConnectionString(connString);
        StartFirmwareUpdate().Wait();
        QueryTwinFWUpdateReported().Wait();
        Console.WriteLine("Press ENTER to exit.");
        Console.ReadLine();
        
8. Erstellen Sie die Projektmappe.

## <a name="create-a-simulated-device-app"></a>Erstellen einer simulierten Geräte-App
In diesem Abschnitt werden Sie Folgendes durchführen:

* Eine Node.js-Konsolen-App erstellen, die auf eine von der Cloud aufgerufene direkte Methode antwortet
* Ein simuliertes Firmwareupdate auslösen
* Mit den gemeldeten Eigenschaften Gerätezwillingabfragen ermöglichen, um Geräte und den Zeitpunkt ihres letzten abgeschlossenen Firmwareupdates zu identifizieren

1. Erstellen Sie einen neuen leeren Ordner mit dem Namen **manageddevice**.  Erstellen Sie im Ordner **manageddevice** die Datei „package.json“, indem Sie an der Eingabeaufforderung den folgenden Befehl verwenden.  Übernehmen Sie alle Standardeinstellungen:
   
    ```
    npm init
    ```
2. Führen Sie an der Eingabeaufforderung im Ordner **manageddevice** den folgenden Befehl aus, um das Geräte-SDK-Paket **azure-iot-device** und das Paket **azure-iot-device-mqtt** zu installieren:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. Erstellen Sie mit einem Text-Editor im Ordner **manageddevice** die neue Datei **dmpatterns_fwupdate_device.js**.
4. Fügen Sie am Anfang der Datei **dmpatterns_fwupdate_device.js** die folgenden require-Anweisungen ein:
   
    ```
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```
5. Fügen Sie die Variable **connectionString** hinzu, und verwenden Sie sie zum Erstellen eines Geräteclients.  
   
    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myDeviceId;SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```
6. Fügen Sie die folgende Funktion hinzu, mit der die gemeldeten Eigenschaften aktualisiert werden.
   
    ```
    var reportFWUpdateThroughTwin = function(twin, firmwareUpdateValue) {
      var patch = {
          iothubDM : {
            firmwareUpdate : firmwareUpdateValue
          }
      };
   
      twin.properties.reported.update(patch, function(err) {
        if (err) throw err;
        console.log('twin state reported')
      });
    };
    ```
7. Fügen Sie die folgende Funktionen hinzu, die das Herunterladen und Übernehmen des Firmwareimages simulieren.
   
    ```
    var simulateDownloadImage = function(imageUrl, callback) {
      var error = null;
      var image = "[fake image data]";
   
      console.log("Downloading image from " + imageUrl);
   
      callback(error, image);
    }
   
    var simulateApplyImage = function(imageData, callback) {
      var error = null;
   
      if (!imageData) {
        error = {message: 'Apply image failed because of missing image data.'};
      }
   
      callback(error);
    }
    ```
8. Fügen Sie die folgende Funktion hinzu, die den Status des Firmwareupdates über die gemeldeten Eigenschaften für das Warten auf den Download aktualisiert.  Normalerweise werden Geräte über verfügbare Updates informiert. Außerdem sorgt eine vom Administrator definierte Richtlinie für das Herunterladen und Übernehmen des Updates durch das Gerät.  An dieser Stelle wird dann die Logik zum Aktivieren dieser Richtlinie ausgeführt.  Der Einfachheit halber führen wir eine Verzögerung von 4 Sekunden aus und laden dann das Firmwareimage herunter. 
   
    ```
    var waitToDownload = function(twin, fwPackageUriVal, callback) {
      var now = new Date();
   
      reportFWUpdateThroughTwin(twin, {
        fwPackageUri: fwPackageUriVal,
        status: 'waiting',
        error : null,
        startedWaitingTime : now.toISOString()
      });
      setTimeout(callback, 4000);
    };
    ```
9. Fügen Sie die folgende Funktion hinzu, die den Status des Firmwareupdates über die gemeldeten Eigenschaften für den Download des Firmwareimages aktualisiert.  Anschließend wird ein Firmwaredownload simuliert, und schließlich wird der Status des Firmwareupdates aktualisiert, um über Erfolg oder Fehler des Downloads zu informieren.
   
    ```
    var downloadImage = function(twin, fwPackageUriVal, callback) {
      var now = new Date();   
   
      reportFWUpdateThroughTwin(twin, {
        status: 'downloading',
      });
   
      setTimeout(function() {
        // Simulate download
        simulateDownloadImage(fwPackageUriVal, function(err, image) {
   
          if (err)
          {
            reportFWUpdateThroughTwin(twin, {
              status: 'downloadfailed',
              error: {
                code: error_code,
                message: error_message,
              }
            });
          }
          else {        
            reportFWUpdateThroughTwin(twin, {
              status: 'downloadComplete',
              downloadCompleteTime: now.toISOString(),
            });
   
            setTimeout(function() { callback(image); }, 4000);   
          }
        });
   
      }, 4000);
    }
    ```
10. Fügen Sie die folgende Funktion hinzu, die den Status des Firmwareupdates über die gemeldeten Eigenschaften für die Übernahme des Firmwareimages aktualisiert.  Anschließend wird ein Anwenden des Firmwareimages simuliert, und schließlich wird der Status des Firmwareupdates aktualisiert, um über den Erfolg oder Fehler des Übernehmens zu informieren.
    
    ```
    var applyImage = function(twin, imageData, callback) {
      var now = new Date();   
    
      reportFWUpdateThroughTwin(twin, {
        status: 'applying',
        startedApplyingImage : now.toISOString()
      });
    
      setTimeout(function() {
    
        // Simulate apply firmware image
        simulateApplyImage(imageData, function(err) {
          if (err) {
            reportFWUpdateThroughTwin(twin, {
              status: 'applyFailed',
              error: {
                code: err.error_code,
                message: err.error_message,
              }
            });
          } else { 
            reportFWUpdateThroughTwin(twin, {
              status: 'applyComplete',
              lastFirmwareUpdate: now.toISOString()
            });    
    
          }
        });
    
        setTimeout(callback, 4000);
    
      }, 4000);
    }
    ```
11. Fügen Sie die folgende Funktion hinzu, mit der die **firmwareUpdate**-Methode verwaltet und der mehrstufige Firmwareupdatevorgang initiiert wird.
    
    ```
    var onFirmwareUpdate = function(request, response) {
    
      // Respond the cloud app for the direct method
      response.send(200, 'FirmwareUpdate started', function(err) {
        if (!err) {
          console.error('An error occured when sending a method response:\n' + err.toString());
        } else {
          console.log('Response to method \'' + request.methodName + '\' sent successfully.');
        }
      });
    
      // Get the parameter from the body of the method request
      var fwPackageUri = JSON.parse(request.payload).fwPackageUri;
    
      // Obtain the device twin
      client.getTwin(function(err, twin) {
        if (err) {
          console.error('Could not get device twin.');
        } else {
          console.log('Device twin acquired.');
    
          // Start the multi-stage firmware update
          waitToDownload(twin, fwPackageUri, function() {
            downloadImage(twin, fwPackageUri, function(imageData) {
              applyImage(twin, imageData, function() {});    
            });  
          });
    
        }
      });
    }
    ```
12. Fügen Sie abschließend den folgenden Code hinzu, der eine Verbindung mit dem IoT Hub als Gerät herstellt. 
    
    ```
    client.open(function(err) {
      if (err) {
        console.error('Could not connect to IotHub client');
      }  else {
        console.log('Client connected to IoT Hub.  Waiting for firmwareUpdate direct method.');
      }
    
      client.onDeviceMethod('firmwareUpdate', onFirmwareUpdate(request, response));
    });
    ```

> [!NOTE]
> Der Einfachheit halber wird in diesem Tutorial keine Wiederholungsrichtlinie implementiert. Im Produktionscode sollten Sie Wiederholungsrichtlinien implementieren (z.B. einen exponentiellen Backoff), wie im MSDN-Artikel zum [Transient Fault Handling (Behandeln vorübergehender Fehler)][lnk-transient-faults] beschrieben.
> 
> 

## <a name="run-the-apps"></a>Ausführen der Apps
Sie können die Apps nun ausführen.

1. Führen Sie in der Befehlszeile im Ordner **manageddevice** den folgenden Befehl aus, um mit dem Lauschen auf die direkte Methode zum Neustarten zu beginnen.
   
    ```
    node dmpatterns_fwupdate_device.js
    ```
2. Führen Sie die C#-Konsolen-App **TriggerFWUpdate** aus – klicken Sie mit der rechten Maustaste auf das **TriggerFWUpdate**-Projekt, wählen Sie **Debuggen** und **Neue Instanz starten**.

3. Die Reaktion des Geräts auf die direkte Methode wird in der Konsole angezeigt.

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie mit einer direkten Methode ein Remotefirmwareupdate auf einem Gerät ausgelöst und mithilfe der gemeldeten Eigenschaften in regelmäßigen Abständen den Fortschritt des Firmwareaktualisierungsvorgangs überprüft.  

Im Tutorial [Planen und Senden von Aufträgen][lnk-tutorial-jobs] erfahren Sie, wie Sie Ihre IoT-Lösung erweitern und Methodenaufrufe für mehrere Geräte planen.

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-node-firmware-update/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-firmware-update/createnetapp.png

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-dm-getstarted]: iot-hub-node-node-device-management-get-started.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-rpi-implementation]: https://github.com/Azure/azure-iot-sdks/tree/master/c/iothub_client/samples/iothub_client_sample_mqtt_dm/pi_device
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/


<!--HONumber=Nov16_HO5-->


