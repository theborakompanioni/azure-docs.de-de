---
title: "Erste Schritte mit der Azure IoT Hub-Geräteverwaltung | Microsoft Docs"
description: "In diesem Tutorial sind die ersten Schritte mit der Geräteverwaltung in Azure IoT Hub erläutert."
services: iot-hub
documentationcenter: .net
author: juanjperez
manager: timlt
editor: 
ms.assetid: e044006d-ffd6-469b-bc63-c182ad066e31
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/17/2016
ms.author: juanpere
translationtype: Human Translation
ms.sourcegitcommit: 00746fa67292fa6858980e364c88921d60b29460
ms.openlocfilehash: cf9741e7bc30ccb5e6b8f79dad7c8ef725cd683a


---
# <a name="tutorial-get-started-with-device-management"></a>Tutorial: Erste Schritte mit der Geräteverwaltung

[!INCLUDE [iot-hub-selector-dm-getstarted](../../includes/iot-hub-selector-dm-getstarted.md)]
## <a name="introduction"></a>Einführung
IoT-Back-End-Apps können Grundtypen (d.h. die Gerätezwillingsmethode und die direkte Methode) in Azure IoT Hub verwenden, um Geräteverwaltungsaktionen auf Geräten per Remotezugriff zu starten und zu überwachen.  Dieser Artikel enthält Informationen dazu, wie eine IoT-Back-End-App und ein Gerät zusammen verwendet werden können, um einen Remoteneustart des Geräts mit IoT Hub zu initiieren und zu überwachen.

Verwenden Sie für den Remotestart und die Remoteüberwachung von Geräteverwaltungsaktionen auf Ihren Geräten von einer cloudbasierten Back-End-App aus IoT Hub-Grundtypen, z.B. die [Gerätezwillingsmethode][lnk-devtwin] und [direkte Methoden][lnk-c2dmethod]. In diesem Tutorial wird veranschaulicht, wie eine Back-End-App und ein Gerät zusammen verwendet werden können, damit Sie einen Remoteneustart des Geräts von IoT Hub aus initiieren und überwachen können.

Sie verwenden eine direkte Methode, um Geräteverwaltungsaktionen (wie Neustarts, Zurücksetzen auf Werkseinstellungen und Firmwareaktualisierung) von einer Back-End-App aus in der Cloud zu initiieren. Das Gerät ist für Folgendes verantwortlich:

* Verarbeiten der von IoT Hub gesendeten Methodenanforderung
* Initiieren der entsprechenden gerätespezifischen Aktion auf dem Gerät
* Senden von Statusupdates über die an IoT Hub gemeldeten Eigenschaften.

Sie können eine Back-End-App in der Cloud verwenden, um Gerätezwillingsabfragen auszuführen und Berichte zum Status der Geräteverwaltungsaktionen zu erstellen.

Dieses Tutorial veranschaulicht folgende Vorgehensweisen:

* Verwenden des Azure-Portals zum Erstellen einer IoT Hub-Instanz und einer Geräteidentität im IoT Hub
* Erstellen einer simulierten Geräte-App, die eine direkte Methode aufweist, die den Neustart ermöglicht und von der Cloud aufgerufen werden kann
* Erstellen einer Konsolenanwendung, die eine direkte Neustartmethode auf der simulierten Geräte-App über Ihren IoT-Hub aufruft

Am Ende dieses Tutorials verfügen Sie über eine Node.js-Konsolen-Geräte-App und eine .NET-Konsolen-Back-End-App (C#):

**dmpatterns_getstarted_device.js**: Stellt mit der zuvor erstellten Geräteidentität eine Verbindung mit dem IoT Hub her, empfängt eine direkte Methode zum Neustart, simuliert einen physischen Neustart und meldet den Zeitpunkt des letzten Neustarts.

**TriggerReboot**: Ruft eine direkte Methode auf der simulierten Geräte-App auf und zeigt die Antwort sowie die aktualisierten Eigenschaften an.

Für dieses Tutorial benötigen Sie Folgendes:

* Microsoft Visual Studio 2015.
* Node.js Version 0.12.x oder höher, <br/>  Unter [Prepare your development environment][lnk-dev-setup] (Vorbereiten Ihrer Entwicklungsumgebung) wird beschrieben, wie Sie Node.js für dieses Tutorial unter Windows oder Linux installieren.
* Ein aktives Azure-Konto. (Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein [kostenloses Konto][lnk-free-trial] erstellen.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="trigger-a-remote-reboot-on-the-device-using-a-direct-method"></a>Auslösen eines Remoteneustarts auf dem Gerät über eine direkte Methode
In diesem Abschnitt erstellen Sie eine .NET-Konsolen-App (mit C#), die mit einer direkten Methode einen Remoteneustart auf einem Gerät initiiert und mithilfe von Gerätezwillingsabfragen den letzten Neustart-Zeitpunkt für dieses Gerät abruft.

1. Fügen Sie in Visual Studio in der aktuellen Projektmappe mithilfe der Projektvorlage **Konsolenanwendung** ein Visual C#-Projekt für den klassischen Windows-Desktop hinzu. Nennen Sie das Projekt **TriggerReboot**.

    ![Neues Visual C#-Projekt für den klassischen Windows-Desktop][img-createapp]

2. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt **TriggerReboot**, und klicken Sie anschließend auf **NuGet-Pakete verwalten**.
3. Wählen Sie im Fenster **NuGet-Paket-Manager** die Option **Durchsuchen**, suchen Sie nach **microsoft.azure.devices**, wählen Sie zum Installieren des Pakets **Microsoft.Azure.Devices** die Option **Installieren**, und akzeptieren Sie die Nutzungsbedingungen. Bei diesem Verfahren werden das NuGet-Paket für das [Dienst SDK für Microsoft Azure IoT][lnk-nuget-service-sdk] sowie die dazugehörigen Abhängigkeiten heruntergeladen, installiert und mit einem Verweis versehen.

    ![Fenster „NuGet-Paket-Manager“][img-servicenuget]
4. Fügen Sie am Anfang der Datei **Program.cs** die folgenden `using`-Anweisungen hinzu:
   
        using Microsoft.Azure.Devices;
        
5. Fügen Sie der **Program** -Klasse die folgenden Felder hinzu. Ersetzen Sie den Platzhalterwert durch die Verbindungszeichenfolge für den IoT Hub, den Sie im vorherigen Abschnitt erstellt haben, und das Zielgerät.
   
        static RegistryManager registryManager;
        static string connString = "{iot hub connection string}";
        static ServiceClient client;
        static JobClient jobClient;
        static string targetDevice = "{deviceIdForTargetDevice}";
        
6. Fügen Sie der **Program**-Klasse die folgende Methode hinzu.  Dieser Code ruft den Gerätezwilling für das neu startende Gerät auf und gibt die gemeldeten Eigenschaften aus.
   
        public static async Task QueryTwinRebootReported()
        {
            Twin twin = await registryManager.GetTwinAsync(targetDevice);
            Console.WriteLine(twin.Properties.Reported.ToJson());
        }
        
7. Fügen Sie der **Program**-Klasse die folgende Methode hinzu.  Dieser Code löst den Neustart auf dem Gerät mit einer direkten Methode aus.

        public static async Task StartReboot()
        {
            client = ServiceClient.CreateFromConnectionString(connString);
            CloudToDeviceMethod method = new CloudToDeviceMethod("reboot");
            method.ResponseTimeout = TimeSpan.FromSeconds(30);

            CloudToDeviceMethodResult result = await client.InvokeDeviceMethodAsync(targetDevice, method);

            Console.WriteLine("Invoked firmware update on device.");
        }

7. Fügen Sie abschließend der **Main** -Methode die folgenden Zeilen hinzu:
   
        registryManager = RegistryManager.CreateFromConnectionString(connString);
        StartReboot().Wait();
        QueryTwinRebootReported().Wait();
        Console.WriteLine("Press ENTER to exit.");
        Console.ReadLine();
        
8. Erstellen Sie die Projektmappe.

## <a name="create-a-simulated-device-app"></a>Erstellen einer simulierten Geräte-App
In diesem Abschnitt werden Sie Folgendes durchführen:

* Eine Node.js-Konsolen-App erstellen, die auf eine von der Cloud aufgerufene direkte Methode antwortet
* Einen simulierten Gerätneustart auslösen
* Mit den gemeldeten Eigenschaften Gerätezwillingabfragen ermöglichen, um Geräte und den Zeitpunkt ihres letzten Neustarts zu identifizieren

1. Erstellen Sie einen neuen leeren Ordner mit dem Namen **manageddevice**.  Erstellen Sie im Ordner **manageddevice** die Datei „package.json“, indem Sie an der Eingabeaufforderung den folgenden Befehl verwenden.  Übernehmen Sie alle Standardeinstellungen:
   
    ```
    npm init
    ```
2. Führen Sie an der Eingabeaufforderung im Ordner **manageddevice** den folgenden Befehl aus, um das Geräte-SDK-Paket **azure-iot-device** und das Paket **azure-iot-device-mqtt** zu installieren:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. Erstellen Sie mit einem Text-Editor im Ordner **manageddevice** die neue Datei **dmpatterns_getstarted_device.js**.
4. Fügen Sie am Anfang der Datei **dmpatterns_getstarted_device.js** die folgenden require-Anweisungen ein:
   
    ```
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```
5. Fügen Sie die Variable **connectionString** hinzu, und verwenden Sie sie zum Erstellen eines Geräteclients.  Ersetzen Sie die Verbindungszeichenfolge durch Ihre Geräteverbindungszeichenfolge.  
   
    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myDeviceId;SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```
6. Fügen Sie die folgende Funktion hinzu, um die direkte Methode auf dem Gerät zu implementieren
   
    ```
    var onReboot = function(request, response) {
   
        // Respond the cloud app for the direct method
        response.send(200, 'Reboot started', function(err) {
            if (!err) {
                console.error('An error occured when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.');
            }
        });
   
        // Report the reboot before the physical restart
        var date = new Date();
        var patch = {
            iothubDM : {
                reboot : {
                    lastReboot : date.toISOString(),
                }
            }
        };
   
        // Get device Twin
        client.getTwin(function(err, twin) {
            if (err) {
                console.error('could not get twin');
            } else {
                console.log('twin acquired');
                twin.properties.reported.update(patch, function(err) {
                    if (err) throw err;
                    console.log('Device reboot twin state reported')
                });  
            }
        });
   
        // Add your device's reboot API for physical restart.
        console.log('Rebooting!');
    };
    ```
7. Fügen Sie folgenden Code hinzu, um die Verbindung mit dem IoT Hub zu öffnen und den Listener der direkten Methode zu starten:
   
    ```
    client.open(function(err) {
        if (err) {
            console.error('Could not open IotHub client');
        }  else {
            console.log('Client opened.  Waiting for reboot method.');
            client.onDeviceMethod('reboot', onReboot);
        }
    });
    ```
8. Speichern und schließen Sie die Datei **dmpatterns_getstarted_device.js**.
   
   [AZURE.NOTE] Der Einfachheit halber wird in diesem Tutorial keine Wiederholungsrichtlinie implementiert. Im Produktionscode sollten Sie Wiederholungsrichtlinien implementieren (z.B. einen exponentiellen Backoff), wie im MSDN-Artikel zum [Transient Fault Handling (Behandeln vorübergehender Fehler)][lnk-transient-faults] beschrieben.


## <a name="run-the-apps"></a>Ausführen der Apps
Sie können die Apps nun ausführen.

1. Führen Sie in der Befehlszeile im Ordner **manageddevice** den folgenden Befehl aus, um mit dem Lauschen auf die direkte Methode zum Neustarten zu beginnen.
   
    ```
    node dmpatterns_getstarted_device.js
    ```
2. Führen Sie die C#-Konsolen-App **TriggerReboot** aus – klicken Sie mit der rechten Maustaste auf das **TriggerReboot**-Projekt, wählen Sie **Debuggen** und **Neue Instanz starten**.

3. Die Reaktion des Geräts auf die direkte Methode wird in der Konsole angezeigt.

## <a name="customize-and-extend-the-device-management-actions"></a>Anpassen und Erweitern der Geräteverwaltungsaktionen
Ihre IoT-Lösungen können die festgelegten Geräteverwaltungsmuster erweitern oder benutzerdefinierte Muster ermöglichen. Dazu werden die Grundtypen für die Gerätezwillings- oder die C2D-Methode verwendet. Andere Beispiele für Geräteverwaltungsaktionen sind das Zurücksetzen auf die Werkseinstellungen, Firmware- und Softwareaktualisierungen, Energieverwaltung, Netzwerk- und Konnektivitätsverwaltung und Datenverschlüsselung.

## <a name="device-maintenance-windows"></a>Gerätewartungsfenster
In der Regel konfigurieren Sie die Ausführung von Aktionen für Geräte so, dass Unterbrechungen und Ausfallzeiten auf ein Minimum beschränkt sind.  Bei Gerätewartungsfenstern handelt es sich um ein häufig verwendetes Muster zum Festlegen des Zeitpunkts, zu dem ein Gerät seine Konfiguration aktualisieren soll. Ihre Back-End-Lösungen können die gewünschten Eigenschaften des Gerätezwillings verwenden, um auf Ihrem Gerät eine Richtlinie zur Aktivierung eines Wartungsfensters festzulegen und zu aktivieren. Wenn ein Gerät die Wartungsfensterrichtlinie erhält, kann es mithilfe der gemeldeten Eigenschaft des Gerätezwillings den Richtlinienstatus melden. Die Back-End-App kann dann mithilfe von Gerätezwillingsabfragen die Konformität von Geräten und den einzelnen Richtlinien sicherstellen.

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie eine direkte Methode zum Auslösen eines Remoteneustarts auf einem Gerät und die gemeldeten Eigenschaften zum Melden des letzten Neustart-Zeitpunkts des Geräts verwendet. Darüber hinaus haben Sie den Gerätezwilling abgefragt, um den letzten Neustart-Zeitpunkt des Geräts aus der Cloud zu ermitteln.

Informationen zu den weiteren ersten Schritten mit IoT Hub und Geräteverwaltungsmustern, z.B. drahtloses Firmware-Remoteupdate, finden Sie unter:

[Tutorial: Durchführen eines Firmwareupdates][lnk-fwupdate]

Im Tutorial [Planen und Senden von Aufträgen][lnk-tutorial-jobs] erfahren Sie, wie Sie Ihre IoT-Lösung erweitern und Methodenaufrufe für mehrere Geräte planen.

Informationen zu den weiteren ersten Schritten mit IoT Hub finden Sie unter [Erste Schritte mit dem Gateway-SDK][lnk-gateway-SDK].

<!-- images and links -->
[img-output]: media/iot-hub-get-started-with-dm/image6.png
[img-dm-ui]: media/iot-hub-get-started-with-dm/dmui.png
[img-servicenuget]: media/iot-hub-csharp-node-device-management-get-started/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-device-management-get-started/createnetapp.png

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md

[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-fwupdate]: iot-hub-node-node-firmware-update.md
[Azure portal]: https://portal.azure.com/
[Using resource groups to manage your Azure resources]: ../azure-portal/resource-group-portal.md
[lnk-dm-github]: https://github.com/Azure/azure-iot-device-management
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/


<!--HONumber=Nov16_HO5-->


