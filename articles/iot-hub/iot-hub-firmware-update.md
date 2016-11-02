<properties
 pageTitle="Gewusst wie: Durchführen eines Firmwareupdates | Microsoft Azure"
 description="In diesem Tutorial erfahren Sie, wie Sie eine Aktualisierung der Firmware durchführen."
 services="iot-hub"
 documentationCenter=".net"
 authors="juanjperez"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="juanpere"/>


# <a name="tutorial:-how-to-do-a-firmware-update-(preview)"></a>Tutorial: Durchführen eines Firmwareupdates (Vorschau)

## <a name="introduction"></a>Einführung
Im Tutorial [Erste Schritte mit der Geräteverwaltung][lnk-dm-getstarted] haben Sie erfahren, wie Sie mit den Grundtypen [Gerätezwilling][lnk-devtwin] und [Cloud-zu-Gerät-Methoden (C2D)][lnk-c2dmethod] ein Geräte remote neu starten können. In diesem Tutorial werden dieselben IoT Hub-Grundtypen verwendet. Sie erhalten Hilfestellung dazu, wie Sie ein simuliertes End-to-End-Firmwareupdate durchführen.  Dieses Muster wird in der Firmwareupdateimplementierung für das Intel Edison-Gerätebeispiel verwendet.

Dieses Lernprogramm beschreibt Folgendes:

- Erstellen einer Konsolenanwendung, die die direkte firmwareUpdate-Methode auf dem simulierten Gerät über den IoT Hub aufruft
- Erstellen eines simulierten Geräts, das eine direkte firmwareUpdate-Methode implementiert, das einen mehrstufigen Prozess durchläuft und über diesen auf das Herunterladen des Firmwareimages wartet, das Firmwareimage herunterlädt und schließlich das Firmwareimage übernimmt.  Während der gesamten Ausführung aller Phasen aktualisiert das Gerät den Fortschritt über die vom Gerätezwilling gemeldeten Eigenschaften.

Am Ende dieses Tutorials verfügen Sie über zwei Node.js-Konsolenanwendungen:

**dmpatterns_fwupdate_service.js**, die auf dem simulierten Gerät eine direkte Methode aufruft, die Antwort anzeigt und in regelmäßigen Abständen (alle 500 ms) die aktualisierten über die vom Gerätezwilling gemeldeten Eigenschaften anzeigt.

**dmpatterns_fwupdate_device.js**, die über die zuvor erstellte Geräteidentität eine Verbindung mit dem IoT Hub herstellt, eine direkte firmwareUpdate-Methode empfängt und einen mehrstufigen Prozess zum Simulieren eines Firmwareupdates durchläuft, bei dem auf das Herunterladen des Images gewartet, das Image heruntergeladen und das Image schließlich übernommen wird.


Für dieses Lernprogramm benötigen Sie Folgendes:

Node.js Version 0.12.x oder höher, <br/>  Unter [Vorbereiten Ihrer Entwicklungsumgebung][lnk-dev-setup] wird beschrieben, wie Sie Node.js für dieses Tutorial unter Windows oder Linux installieren.

Ein aktives Azure-Konto. (Falls Sie nicht über ein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Weitere Informationen finden Sie unter [Azure – Kostenlose Testversion][lnk-free-trial].)

Befolgen Sie die Schritte im Artikel [Erste Schritte mit der Geräteverwaltung](iot-hub-device-management-get-started.md), um Ihren IoT Hub zu erstellen und die Verbindungszeichenfolge abzurufen.

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-a-simulated-device-app"></a>Erstellen einer simulierten Geräte-App

In diesem Abschnitt erstellen Sie eine Node.js-Konsolen-App, die auf eine von der Cloud aufgerufene direkte Methode antwortet. Diese Methode löst ein simuliertes Gerätefirmwareupdate aus und ermöglicht mithilfe der vom Gerätezwilling gemeldeten Eigenschaften Abfragen des Gerätezwillings zum Identifizieren von Geräten und deren letzten Neustart.

1. Erstellen Sie einen neuen leeren Ordner mit dem Namen **manageddevice**.  Erstellen Sie im Ordner **manageddevice** die Datei „package.json“, indem Sie an der Eingabeaufforderung den folgenden Befehl verwenden.  Übernehmen Sie alle Standardeinstellungen:

    ```
    npm init
    ```
    
2. Führen Sie an der Eingabeaufforderung im Ordner **manageddevice** den folgenden Befehl aus, um das Geräte-SDK-Paket **azure-iot-device@dtpreview** und das Paket **azure-iot-device-mqtt@dtpreview** zu installieren:

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
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

6. Fügen Sie die folgende Funktion hinzu, mit der die vom Gerätezwilling gemeldeten Eigenschaften aktualisiert werden.

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
    
7. Fügen Sie die folgenden Funktionen hinzu, die das Herunterladen und Übernehmen des Firmwareimages simulieren.

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

8. Fügen Sie die folgende Funktion hinzu, die den Status des Firmwareupdates über die vom Gerätezwilling gemeldeten Eigenschaften für das Warten auf den Download aktualisiert.  Normalerweise werden Geräte über verfügbare Updates informiert. Außerdem sorgt eine vom Administrator definierte Richtlinie für das Herunterladen und Übernehmen des Updates durch das Gerät.  An dieser Stelle wird dann die Logik zum Aktivieren dieser Richtlinie ausgeführt.  Der Einfachheit halber führen wir eine Verzögerung von 4 Sekunden aus und laden dann das Firmwareimage herunter. 

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
    
9. Fügen Sie die folgende Funktion hinzu, die den Status des Firmwareupdates über die vom Gerätezwilling gemeldeten Eigenschaften für das Herunterladen der Firmware aktualisiert.  Anschließend wird ein Firmwaredownload simuliert, und schließlich wird der Status des Firmwareupdates aktualisiert, um über Erfolg oder Fehler des Downloads zu informieren.

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
    
10. Fügen Sie die folgende Funktion hinzu, die den Status des Firmwareupdates über die vom Gerätezwilling gemeldeten Eigenschaften für das Übernehmen der Firmware aktualisiert.  Anschließend wird ein Anwenden des Firmwareimages simuliert, und schließlich wird der Status des Firmwareupdates aktualisiert, um über den Erfolg oder Fehler des Übernehmens zu informieren.

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

11. Fügen Sie die folgenden Funktion hinzu, mit der die firmwareUpdate-Methode verwaltet und der mehrstufige Firmwareupdatevorgang initiiert wird.

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

> [AZURE.NOTE] Der Einfachheit halber wird in diesem Tutorial keine Wiederholungsrichtlinie implementiert. Im Produktionscode sollten Sie Wiederholungsrichtlinien implementieren (z.B. einen exponentiellen Backoff), wie im MSDN-Artikel zum [Behandeln vorübergehender Fehler][lnk-transient-faults] beschrieben.

## <a name="trigger-a-remote-firmware-update-on-the-device-using-a-direct-method"></a>Auslösen eines Remotefirmwareupdates auf dem Gerät über eine direkte Methode 

In diesem Abschnitt erstellen Sie eine Node.js-Konsolen-App, die mit einer direkten Methode ein Remotefirmwareupdate auf einem Gerät initiiert und mithilfe von Gerätezwillingsabfragen in regelmäßigen Abständen den Status des aktiven Firmwareupdates auf diesem Gerät abruft.


1. Erstellen Sie einen neuen leeren Ordner mit dem Namen **triggerfwupdateondevice**.  Erstellen Sie im Ordner **triggerfwupdateondevice** die Datei „package.json“, indem Sie an der Eingabeaufforderung den folgenden Befehl eingeben.  Übernehmen Sie alle Standardeinstellungen:

    ```
    npm init
    ```
    
2. Führen Sie an der Eingabeaufforderung im Ordner **triggerfwupdateondevice** den folgenden Befehl aus, um das Geräte-SDK-Paket **azure-iothub** und das Paket **azure-iot-device-mqtt** zu installieren:

    ```
    npm install azure-iot-hub@dtpreview --save
    ```
    
3. Erstellen Sie mit einem Text-Editor im Ordner **triggerfwupdateondevice** die neue Datei **dmpatterns_getstarted_service.js**.

4. Fügen Sie am Anfang der Datei **dmpatterns_getstarted_service.js** die folgenden require-Anweisungen ein:

    ```
    'use strict';

    var Registry = require('azure-iothub').Registry;
    var Client = require('azure-iothub').Client;
    ```

5. Fügen Sie die folgenden Variablendeklarationen hinzu, und ersetzen Sie die Platzhalterwerte:

    ```
    var connectionString = '{device_connectionstring}';
    var registry = Registry.fromConnectionString(connectionString);
    var client = Client.fromConnectionString(connectionString);
    var deviceToUpdate = 'myDeviceId';
    ```
    
6. Fügen Sie die folgende Funktion hinzu, um den Wert der gemeldeten firmwareUpdate-Eigenschaft zu suchen und anzuzeigen.

    ```
    var queryTwinFWUpdateReported = function() {
        registry.getTwin(deviceToUpdate, function(err, twin){
            if (err) {
              console.error('Could not query twins: ' + err.constructor.name + ': ' + err.message);
            } else {
              console.log((JSON.stringify(twin.properties.reported.iothubDM.firmwareUpdate)) + "\n");
            }
        });
    };
    ```

7. Fügen Sie die folgende Funktion hinzu, um durch Aufrufen der firmwareUpdate-Methode das Gerät neu zu starten:

    ```
    var startFirmwareUpdateDevice = function() {
      var params = {
          fwPackageUri: 'https://secureurl'
      };
      
      var methodName = "firmwareUpdate";
      var payloadData =  JSON.stringify(params);
      
      var methodParams = {
        methodName: methodName,
        payload: payloadData,
        timeoutInSeconds: 30
      };
      
      client.invokeDeviceMethod(deviceToUpdate, methodParams, function(err, result) {
        if (err) {
          console.error('Could not start the firmware update on the device: ' + err.message)
        } 
      });
    };
    ```

8. Fügen Sie dem Code abschließend die folgende Funktion hinzu, die die Firmwareupdatesequenz startet und in regelmäßigen Abständen die vom Gerätezwilling gemeldeten Eigenschaften anzeigt:

    ```
    startFirmwareUpdateDevice();
    setInterval(queryTwinFWUpdateReported, 500);
    ```
    
9. Speichern und schließen Sie die Datei **dmpatterns_fwupdate_service.js**.

## <a name="run-the-applications"></a>Ausführen der Anwendungen

Sie können nun die Anwendungen ausführen.

1. Führen Sie in der Befehlszeile im Ordner **manageddevice** den folgenden Befehl aus, um mit dem Lauschen auf die direkte Methode zum Neustarten zu beginnen.

    ```
    node dmpatterns_fwupdate_device.js
    ```

2. Führen Sie in der Befehlszeile im Ordner **triggerfwupdateondevice** den folgenden Befehl aus, um den Remoteneustart und die Abfrage an den Gerätezwilling zum Suchen des letzten Neustartzeitpunkts auszulösen.

    ```
    node dmpatterns_fwupdate_service.js
    ```

3. Sie können die Antwort auf die direkte Methode durch Ausgeben der Meldung anzeigen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie mit einer direkten Methode ein Remotefirmwareupdate auf einem Gerät ausgelöst und mithilfe der vom Gerätezwilling gemeldeten Eigenschaften in regelmäßigen Abständen den Fortschritt des Firmwareaktualisierungsvorgangs überprüft.  

Im Tutorial [Planen und Senden von Aufträgen][lnk-tutorial-jobs] erfahren Sie, wie Sie Ihre IoT-Lösung erweitern und Methodenaufrufe für mehrere Geräte planen.

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-dm-getstarted]: iot-hub-device-management-get-started.md
[lnk-tutorial-jobs]: iot-hub-schedule-jobs.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx



<!--HONumber=Oct16_HO2-->


