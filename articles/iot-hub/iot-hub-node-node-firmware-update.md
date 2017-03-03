---
title: "Gerätefirmwareupdate mit Azure IoT Hub (Node) | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie mithilfe der Geräteverwaltung in Azure IoT Hub ein Gerätefirmwareupdate initiieren. Sie verwenden die Azure IoT SDKs für Node.js, um eine simulierte Geräte-App und eine Dienst-App zu implementieren, die das Firmwareupdate auslöst."
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
ms.date: 02/06/2017
ms.author: juanpere
translationtype: Human Translation
ms.sourcegitcommit: 4ba60cee8848079935111ed3de480081a4aa58f6
ms.openlocfilehash: 30a707ec15d592c8a10905e13a75ea2f6e52cccc
ms.lasthandoff: 02/06/2017


---
# <a name="use-device-management-to-initiate-a-device-firmware-update-nodenode"></a>Initiieren eines Gerätefirmwareupdates mithilfe der Geräteverwaltung (Node/Node)
[!INCLUDE [iot-hub-selector-firmware-update](../../includes/iot-hub-selector-firmware-update.md)]

## <a name="introduction"></a>Einführung
Im Tutorial [Get started with device management][lnk-dm-getstarted] (Erste Schritte mit der Geräteverwaltung) wurde erläutert, wie Sie mit einem [Gerätezwilling][lnk-devtwin] und [direkten Methoden][lnk-c2dmethod] Grundtypen für den Remoteneustart eines Geräts verwenden können. In diesem Tutorial werden dieselben IoT Hub-Grundtypen verwendet. Sie erhalten Hilfestellung dazu, wie Sie ein simuliertes End-to-End-Firmwareupdate durchführen.  Dieses Muster wird in der Firmwareupdateimplementierung für das Intel Edison-Gerätebeispiel verwendet.

Dieses Tutorial veranschaulicht folgende Vorgehensweisen:

* Erstellen einer Node.js-Konsolen-App, die über Ihre IoT Hub-Instanz die direkte firmwareUpdate-Methode in der simulierten Geräte-App aufruft.
* Erstellen Sie eine simulierte Gerät-App, die eine direkte **FirmwareUpdate**-Methode implementiert. Diese Methode löst einen mehrstufigen Prozess aus, der auf das Herunterladen des Firmware-Images wartet, das Firmware-Image herunterlädt und schließlich das Firmware-Image anwendet. In allen Phasen des Updates nutzt das Gerät die gemeldeten Eigenschaften zum Berichten des Fortschritts.

Am Ende dieses Tutorials verfügen Sie über zwei Node.js-Konsolen-Apps:

**dmpatterns_fwupdate_service.js**, die in der simulierten Geräte-App eine direkte Methode aufruft, die Antwort anzeigt und in regelmäßigen Abständen (alle 500 ms) die aktualisierten gemeldeten Eigenschaften anzeigt.

**dmpatterns_fwupdate_device.js**, die über die zuvor erstellte Geräteidentität eine Verbindung mit dem IoT Hub herstellt, eine direkte firmwareUpdate-Methode empfängt und einen mehrstufigen Prozess zum Simulieren eines Firmwareupdates durchläuft, bei dem auf das Herunterladen des Images gewartet, das Image heruntergeladen und das Image schließlich übernommen wird.

Für dieses Lernprogramm benötigen Sie Folgendes:

* Node.js Version 0.12.x oder höher, <br/>  Unter [Prepare your development environment][lnk-dev-setup] (Vorbereiten Ihrer Entwicklungsumgebung) wird beschrieben, wie Sie Node.js für dieses Tutorial unter Windows oder Linux installieren.
* Ein aktives Azure-Konto. (Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein [kostenloses Konto][lnk-free-trial] erstellen.)

Befolgen Sie die Schritte im Artikel [Erste Schritte mit der Geräteverwaltung](iot-hub-node-node-device-management-get-started.md), um Ihren IoT Hub zu erstellen und die IoT Hub-Verbindungszeichenfolge abzurufen.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="trigger-a-remote-firmware-update-on-the-device-using-a-direct-method"></a>Auslösen eines Remotefirmwareupdates auf dem Gerät über eine direkte Methode
In diesem Abschnitt erstellen Sie eine Node.js-Konsolen-App, die auf einem Gerät ein Remotefirmwareupdate auslöst. Die App verwendet eine direkte Methode zum Auslösen des Updates und Gerätzwillingsabfragen, um in regelmäßigen Abständen den Status des aktiven Firmwareupdates abzurufen.

1. Erstellen Sie einen leeren Ordner mit dem Namen **triggerfwupdateondevice**.  Erstellen Sie im Ordner **triggerfwupdateondevice** die Datei „package.json“, indem Sie an der Eingabeaufforderung den folgenden Befehl ausführen.  Übernehmen Sie alle Standardeinstellungen:
   
    ```
    npm init
    ```
2. Führen Sie an der Eingabeaufforderung im Ordner **triggerfwupdateondevice** den folgenden Befehl aus, um die Geräte-SDK-Pakete **azure-iothub** und **azure-iot-device-mqtt** zu installieren:
   
    ```
    npm install azure-iot-hub --save
    ```
3. Erstellen Sie mit einem Text-Editor im Ordner **triggerfwupdateondevice** die Datei **dmpatterns_getstarted_service.js**.
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
8. Fügen Sie dem Code abschließend die folgende Funktion hinzu, die die Firmwareupdatesequenz startet und in regelmäßigen Abständen die gemeldeten Eigenschaften anzeigt:
   
    ```
    startFirmwareUpdateDevice();
    setInterval(queryTwinFWUpdateReported, 500);
    ```
9. Speichern und schließen Sie die Datei **dmpatterns_fwupdate_service.js**.

[!INCLUDE [iot-hub-device-firmware-update](../../includes/iot-hub-device-firmware-update.md)]

## <a name="run-the-apps"></a>Ausführen der Apps
Sie können die Apps nun ausführen.

1. Führen Sie an der Eingabeaufforderung im Ordner **manageddevice** den folgenden Befehl aus, um mit dem Lauschen auf die direkte Methode zum Neustarten zu beginnen.
   
    ```
    node dmpatterns_fwupdate_device.js
    ```
2. Führen Sie an der Eingabeaufforderung im Ordner **triggerfwupdateondevice** den folgenden Befehl aus, um den Remoteneustart und die Abfrage an den Gerätezwilling zum Suchen des letzten Neustartzeitpunkts auszulösen.
   
    ```
    node dmpatterns_fwupdate_service.js
    ```
3. Die Reaktion des Geräts auf die direkte Methode wird in der Konsole angezeigt.

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie mit einer direkten Methode ein Remotefirmwareupdate auf einem Gerät ausgelöst und mithilfe der gemeldeten Eigenschaften den Fortschritt der Firmwareaktualisierung überprüft.

Im Tutorial [Planen und Senden von Aufträgen][lnk-tutorial-jobs] erfahren Sie, wie Sie Ihre IoT-Lösung erweitern und Methodenaufrufe für mehrere Geräte planen.

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-dm-getstarted]: iot-hub-node-node-device-management-get-started.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

