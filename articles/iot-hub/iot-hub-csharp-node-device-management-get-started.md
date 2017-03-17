---
title: "Erste Schritte mit der Azure IoT Hub-Geräteverwaltung (.NET/Node) | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie mithilfe der Azure IoT Hub-Geräteverwaltung den Neustart eines Remotegeräts initiieren. Sie verwenden das Azure IoT-Geräte-SDK für Node.js, um eine simulierte Geräte-App zu implementieren, die eine direkte Methode enthält, und das Azure IoT-Dienst-SDK, um eine Dienst-App zu implementieren, die die direkte Methode aufruft."
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
ms.sourcegitcommit: d9dad6cff80c1f6ac206e7fa3184ce037900fc6b
ms.openlocfilehash: 0a4dc9c39d864dcfd52121ed4e4fa2393b56f872
ms.lasthandoff: 03/06/2017


---
# <a name="get-started-with-device-management-netnode"></a>Erste Schritte mit der Geräteverwaltung (.NET/Node)

[!INCLUDE [iot-hub-selector-dm-getstarted](../../includes/iot-hub-selector-dm-getstarted.md)]
## <a name="introduction"></a>Einführung
Back-End-Apps können Grundtypen (genauer: die Gerätezwillingsmethode und die direkte Methode) in Azure IoT Hub verwenden, um Geräteverwaltungsaktionen auf Geräten remote zu starten und zu überwachen.  Dieser Artikel enthält Informationen dazu, wie Back-End-Apps und Geräte zusammen verwendet werden können, um mit IoT Hub einen Remoteneustart der Geräte zu initiieren und zu überwachen.

Verwenden Sie für den Remotestart und die Remoteüberwachung von Geräteverwaltungsaktionen auf Ihren Geräten von einer cloudbasierten Back-End-App aus IoT Hub-Grundtypen, z.B. die [Gerätezwillingsmethode][lnk-devtwin] und [direkte Methoden][lnk-c2dmethod]. In diesem Tutorial wird veranschaulicht, wie eine Back-End-App und ein Gerät zusammen verwendet werden können, damit Sie einen Remoteneustart des Geräts von IoT Hub aus initiieren und überwachen können.

Sie verwenden eine direkte Methode, um Geräteverwaltungsaktionen (wie Neustarts, Zurücksetzen auf Werkseinstellungen und Firmwareaktualisierung) von einer Back-End-App aus in der Cloud zu initiieren. Das Gerät ist für Folgendes verantwortlich:

* Verarbeiten der von IoT Hub gesendeten Methodenanforderung
* Initiieren der entsprechenden gerätespezifischen Aktion auf dem Gerät
* Senden von Statusupdates über die gemeldeten Eigenschaften an IoT Hub

Sie können eine Back-End-App in der Cloud verwenden, um Gerätezwillingsabfragen auszuführen und Berichte zum Status der Geräteverwaltungsaktionen zu erstellen.

Dieses Tutorial veranschaulicht folgende Vorgehensweisen:

* Verwenden des Azure-Portals zum Erstellen einer IoT Hub-Instanz und einer Geräteidentität im IoT Hub
* Erstellen einer simulierten Geräte-App, die eine direkte Methode aufweist, die den Neustart ermöglicht und von der Cloud aufgerufen werden kann
* Erstellen einer .NET-Konsolenanwendung, die über Ihre IoT Hub-Instanz eine direkte Neustartmethode in der simulierten Geräte-App aufruft

Am Ende dieses Tutorials verfügen Sie über eine Node.js-Konsolen-Geräte-App und eine .NET-Konsolen-Back-End-App (C#):

**dmpatterns_getstarted_device.js**: Stellt mit der zuvor erstellten Geräteidentität eine Verbindung mit dem IoT Hub her, empfängt eine direkte Methode zum Neustart, simuliert einen physischen Neustart und meldet den Zeitpunkt des letzten Neustarts.

**TriggerReboot**: Ruft eine direkte Methode auf der simulierten Geräte-App auf und zeigt die Antwort sowie die aktualisierten Eigenschaften an.

Für dieses Tutorial benötigen Sie Folgendes:

* Visual Studio 2015 oder Visual Studio 2017.
* Node.js Version 0.12.x oder höher, <br/>  Unter [Prepare your development environment][lnk-dev-setup] (Vorbereiten Ihrer Entwicklungsumgebung) wird beschrieben, wie Sie Node.js für dieses Tutorial unter Windows oder Linux installieren.
* Ein aktives Azure-Konto. (Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein [kostenloses Konto][lnk-free-trial] erstellen.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="trigger-a-remote-reboot-on-the-device-using-a-direct-method"></a>Auslösen eines Remoteneustarts auf dem Gerät über eine direkte Methode
In diesem Abschnitt erstellen Sie eine .NET-Konsolen-App (mit C#), die mit einer direkten Methode einen Remoteneustart auf einem Gerät initiiert und mithilfe von Gerätezwillingsabfragen den letzten Neustart-Zeitpunkt für dieses Gerät abruft.

1. Fügen Sie in Visual Studio einer neuen Projektmappe mithilfe der Projektvorlage **Konsolen-App (.NET Framework)** ein Visual C#-Projekt für den klassischen Windows-Desktop hinzu. Stellen Sie sicher, dass .NET-Framework-Version 4.5.1 oder höher verwendet wird. Nennen Sie das Projekt **TriggerReboot**.

    ![Neues Visual C#-Projekt für den klassischen Windows-Desktop][img-createapp]

2. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt **TriggerReboot**, und klicken Sie anschließend auf **NuGet-Pakete verwalten**.
3. Wählen Sie im Fenster **NuGet-Paket-Manager** die Option **Durchsuchen** aus, suchen Sie nach **microsoft.azure.devices**, wählen Sie zum Installieren des Pakets **Microsoft.Azure.Devices** die Option **Installieren** aus, und akzeptieren Sie die Nutzungsbedingungen. Bei diesem Verfahren wird das NuGet-Paket [Azure IoT-Dienst-SDK][lnk-nuget-service-sdk] heruntergeladen und installiert und ein Verweis auf das Paket und seine Abhängigkeiten hinzugefügt.

    ![Fenster „NuGet-Paket-Manager“][img-servicenuget]
4. Fügen Sie am Anfang der Datei **Program.cs** die folgenden `using`-Anweisungen hinzu:
   
        using Microsoft.Azure.Devices;
        
5. Fügen Sie der **Program** -Klasse die folgenden Felder hinzu. Ersetzen Sie den Platzhalterwert durch die IoT Hub-Verbindungszeichenfolge für den Hub, den Sie im vorherigen Abschnitt erstellt haben, und das Zielgerät.
   
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
In diesem Abschnitt werden Sie folgende Schritte ausführen:

* Erstellen einer Node.js-Konsolen-App, die auf eine von der Cloud aufgerufene direkte Methode antwortet
* Auslösen eines simulierten Geräteneustarts
* Verwenden der gemeldeten Eigenschaften, um Gerätezwillingsabfragen zu ermöglichen, die Geräte und den Zeitpunkt ihres letzten Neustarts ermitteln

1. Erstellen Sie einen neuen leeren Ordner mit dem Namen **manageddevice**.  Erstellen Sie im Ordner **manageddevice** die Datei „package.json“, indem Sie an der Eingabeaufforderung den folgenden Befehl ausführen.  Übernehmen Sie alle Standardeinstellungen:
   
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
5. Fügen Sie die Variable **connectionString** hinzu, und verwenden Sie sie zum Erstellen einer **Client**-Instanz.  Ersetzen Sie die Verbindungszeichenfolge durch Ihre Geräteverbindungszeichenfolge.  
   
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

1. Führen Sie an der Eingabeaufforderung im Ordner **manageddevice** den folgenden Befehl aus, um mit dem Lauschen auf die direkte Methode zum Neustarten zu beginnen.
   
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
In diesem Tutorial haben Sie eine direkte Methode zum Auslösen eines Remoteneustarts auf einem Gerät und die gemeldeten Eigenschaften zum Melden des letzten Neustartzeitpunkts des Geräts verwendet. Darüber hinaus haben Sie den Gerätezwilling abgefragt, um den letzten Neustartzeitpunkt des Geräts aus der Cloud zu ermitteln.

Informationen zu den weiteren ersten Schritten mit IoT Hub und Geräteverwaltungsmustern, z.B. drahtloses Firmware-Remoteupdate, finden Sie unter:

[Tutorial: Durchführen eines Firmwareupdates][lnk-fwupdate]

Im Tutorial [Planen und Senden von Aufträgen][lnk-tutorial-jobs] erfahren Sie, wie Sie Ihre IoT-Lösung erweitern und Methodenaufrufe für mehrere Geräte planen.

Informationen zu den weiteren ersten Schritten mit IoT Hub finden Sie unter [Erste Schritte mit dem Azure IoT Gateway SDK (Linux)][lnk-gateway-SDK].

<!-- images and links -->
[img-output]: media/iot-hub-get-started-with-dm/image6.png
[img-dm-ui]: media/iot-hub-get-started-with-dm/dmui.png
[img-servicenuget]: media/iot-hub-csharp-node-device-management-get-started/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-device-management-get-started/createnetapp.png

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md

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
