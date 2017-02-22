## <a name="create-a-simulated-device-app"></a>Erstellen einer simulierten Geräte-App
In diesem Abschnitt führen Sie folgende Schritte aus:

* Erstellen einer Node.js-Konsolen-App, die auf eine von der Cloud aufgerufene direkte Methode antwortet
* Auslösen eines simulierten Firmwareupdates
* Ermöglichen von Gerätezwillingabfragen mit den gemeldeten Eigenschaften, um Geräte und den Zeitpunkt ihres letzten abgeschlossenen Firmwareupdates zu identifizieren

1. Erstellen Sie einen leeren Ordner mit dem Namen **manageddevice**.  Erstellen Sie im Ordner **manageddevice** die Datei „package.json“, indem Sie an der Eingabeaufforderung den folgenden Befehl ausführen. Übernehmen Sie alle Standardeinstellungen:
   
    ```
    npm init
    ```
2. Führen Sie an der Eingabeaufforderung im Ordner **manageddevice** den folgenden Befehl aus, um die Geräte-SDK-Pakete **azure-iot-device** und **azure-iot-device-mqtt** zu installieren:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. Erstellen Sie im Ordner **manageddevice** mithilfe eines Text-Editors die Datei **dmpatterns_fwupdate_device.js**.

4. Fügen Sie am Anfang der Datei **dmpatterns_fwupdate_device.js** die folgenden require-Anweisungen ein:
   
    ```
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```
5. Fügen Sie die Variable **connectionString** hinzu, und verwenden Sie sie zum Erstellen einer **Client**-Instanz. Ersetzen Sie den Platzhalter `{yourdeviceconnectionstring}` durch die Verbindungszeichenfolge, die Sie sich zuvor im Abschnitt „Erstellen einer Geräteidentität“ notiert haben:
   
    ```
    var connectionString = '{yourdeviceconnectionstring}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```
6. Fügen Sie die folgende Funktion hinzu, mit der die gemeldeten Eigenschaften aktualisiert werden:
   
    ```
    var reportFWUpdateThroughTwin = function(twin, firmwareUpdateValue) {
      var patch = {
          iothubDM : {
            firmwareUpdate : firmwareUpdateValue
          }
      };
   
      twin.properties.reported.update(patch, function(err) {
        if (err) throw err;
        console.log('twin state reported: ' + firmwareUpdateValue.status);
      });
    };
    ```
7. Fügen Sie die folgenden Funktionen hinzu, die das Herunterladen und Anwenden des Firmwareimages simulieren:
   
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
8. Fügen Sie die folgende Funktion hinzu, die den Status des Firmwareupdates über die gemeldeten Eigenschaften auf **waiting** (Warten) aktualisiert. Geräte werden üblicherweise über verfügbare Updates informiert, und eine vom Administrator definierte Richtlinie sorgt dafür, dass das Gerät das Update herunterlädt und anwendet. Diese Funktion ist die Stelle, an der die Logik zum Aktivieren dieser Richtlinie ausgeführt werden muss. Der Einfachheit halber wird das Beispiel vier Sekunden lang wiederholt, bevor mit dem Herunterladen des Firmwareimages begonnen wird:
   
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
9. Fügen Sie die folgende Funktion hinzu, die den Status des Firmwareupdates über die gemeldeten Eigenschaften auf **downloading** (Herunterladen) aktualisiert. Die Funktion simuliert dann einen Firmwaredownload und aktualisiert den Status des Firmwareupdates schließlich auf **downloadFailed** (Downloadfehler) oder auf **downloadComplete** (Download abgeschlossen):
   
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
10. Fügen Sie die folgende Funktion hinzu, die den Status des Firmwareupdates über die gemeldeten Eigenschaften auf **applying** (Anwenden) aktualisiert. Die Funktion simuliert dann das Anwenden des Firmwareimages und aktualisiert den Status des Firmwareupdates schließlich auf **applyFailed** (Fehler beim Anwenden) oder auf **applyComplete** (Anwenden abgeschlossen):
    
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
11. Fügen Sie die folgende Funktion hinzu, die die direkte Methode **firmwareUpdate** behandelt und den mehrstufigen Firmwareupdateprozess initiiert:
    
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
      var fwPackageUri = request.payload.fwPackageUri;
    
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
12. Fügen Sie abschließend den folgenden Code hinzu, der eine Verbindung mit Ihrem IoT-Hub herstellt:
    
    ```
    client.open(function(err) {
      if (err) {
        console.error('Could not connect to IotHub client');
      }  else {
        console.log('Client connected to IoT Hub.  Waiting for firmwareUpdate direct method.');
      }
    
      client.onDeviceMethod('firmwareUpdate', onFirmwareUpdate);
    });
    ```

> [!NOTE]
> Der Einfachheit halber wird in diesem Tutorial keine Wiederholungsrichtlinie implementiert. Im Produktionscode sollten Sie Wiederholungsrichtlinien implementieren (beispielsweise einen exponentiellen Backoff), wie im MSDN-Artikel zum [Behandeln vorübergehender Fehler][lnk-transient-faults] beschrieben.
> 
> 

<!--HONumber=Feb17_HO1-->


