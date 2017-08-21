---
title: "Planen von Aufträgen mit Azure IoT Hub (.NET/Node) | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie einen Azure IoT Hub-Auftrag planen, um eine direkte Methode auf mehreren Geräten aufzurufen. Sie verwenden das Azure IoT-Geräte-SDK für Node.js, um die simulierten Geräte-Apps zu implementieren, und das Azure IoT-Dienst-SDK für .NET, um eine Dienst-App für die Auftragsausführung zu implementieren."
services: iot-hub
documentationcenter: .net
author: juanjperez
manager: timlt
editor: 
ms.assetid: 2233356e-b005-4765-ae41-3a4872bda943
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/10/2017
ms.author: juanpere
ms.translationtype: HT
ms.sourcegitcommit: f76de4efe3d4328a37f86f986287092c808ea537
ms.openlocfilehash: a8f4f34aa99c4a9966957cac213ec9170de80a46
ms.contentlocale: de-de
ms.lasthandoff: 07/10/2017


---
# <a name="schedule-and-broadcast-jobs-netnodejs"></a>Planen und Übertragen von Aufträgen für (.NET/Node.js)

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

Verwenden Sie Azure IoT Hub zum Planen und Nachverfolgen von Aufträgen, die Millionen von Geräte aktualisieren. Verwenden Sie Aufträge zum:

* Aktualisieren gewünschter Eigenschaften
* Aktualisieren von Tags
* Aufrufen direkter Methoden

Ein Auftrag umschließt eine dieser Aktionen und verfolgt die Ausführung für eine Gruppe von Geräten nach, die anhand einer Gerätezwillingsabfrage definiert wird. Eine Back-End-App kann z.B. einen Auftrag verwenden, um eine direkte Methode auf 10.000 Geräten aufzurufen, die die Geräte neu startet. Sie geben die Gruppe von Geräten mit einer Gerätezwillingsabfrage ein und planen die Ausführung des Auftrags zu einem späteren Zeitpunkt. Der Auftrag verfolgt den Fortschritt nach, während jedes der Geräte die Neustart-Direktmethode empfängt und ausführt.

Weitere Informationen zu diesen Funktionen finden Sie unter:

* Gerätezwilling und -eigenschaften: [Tutorial: Erste Schritte mit Gerätezwillingen (Vorschau)][lnk-get-started-twin] und [Tutorial: Verwenden der Eigenschaften von Gerätezwillingen][lnk-twin-props]
* Direkte Methoden: [IoT Hub-Entwicklerhandbuch – direkte Methoden][lnk-dev-methods] und [Tutorial: Verwenden von direkten Methoden][lnk-c2d-methods]

Dieses Tutorial veranschaulicht folgende Vorgehensweisen:

* Erstellen Sie eine Geräte-App, die eine direkte Methode namens **lockDoor** implementiert, die von der Back-End-App aufgerufen werden kann. Die Geräte-App empfängt auch die Änderungen der gewünschten Eigenschaft über die Back-End-App.
* Erstellen Sie eine Back-End-App, die einen Auftrag zum Aufrufen der direkten Methode **lockDoor** auf mehreren Geräten erstellt. Ein anderer Auftrag sendet die Updates der gewünschten Eigenschaft an mehrere Geräte.

Am Ende dieses Tutorials verfügen Sie über eine Node.js-Konsolen-Geräte-App und eine .NET-Konsolen-Back-End-App (C#):

**simDevice.js**, die eine Verbindung mit Ihrem IoT Hub herstellt, implementiert die direkte **lockDoor**-Methode und führt die Änderungen der gewünschten Eigenschaft durch.

**ScheduleJob**, die Aufträge zum Aufrufen der direkten **lockDoor**-Methode und Aktualisieren der gewünschten Gerätezwillingseigenschaften auf mehreren Geräten verwendet.

Für dieses Lernprogramm benötigen Sie Folgendes:

* Visual Studio 2015 oder Visual Studio 2017
* Node.js Version 0.12.x oder höher. Im Artikel [Vorbereiten Ihrer Entwicklungsumgebung][lnk-dev-setup] wird beschrieben, wie Sie Node.js für dieses Tutorial unter Windows oder Linux installieren.
* Ein aktives Azure-Konto. Wenn Sie nicht über ein Konto verfügen, können Sie in nur wenigen Minuten ein [kostenloses Konto][lnk-free-trial] erstellen.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="schedule-jobs-for-calling-a-direct-method-and-sending-device-twin-updates"></a>Planen von Aufträgen zum Aufrufen einer direkten Methode und Senden der Gerätezwillingsupdates

In diesem Abschnitt erstellen Sie eine .NET-Konsolen-App (mithilfe von C#), die Aufträge zum Aufrufen der direkten **lockDoor**-Methode und Senden der Updates der gewünschten Eigenschaft an mehrere Geräte sendet.

1. Fügen Sie in Visual Studio in der aktuellen Projektmappe mithilfe der Projektvorlage **Konsolenanwendung** ein Visual C#-Projekt für den klassischen Windows-Desktop hinzu. Geben Sie dem Projekt den Namen **ScheduleJob**.

    ![Neues Visual C#-Projekt für den klassischen Windows-Desktop][img-createapp]

1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt **ScheduleJob**, und klicken Sie anschließend auf **NuGet-Pakete verwalten**.
1. Wählen Sie im Fenster **NuGet-Paket-Manager** die Option **Durchsuchen** aus, suchen Sie nach **microsoft.azure.devices**, wählen Sie zum Installieren des Pakets **Microsoft.Azure.Devices** die Option **Installieren** aus, und akzeptieren Sie die Nutzungsbedingungen. In diesem Schritt wird das NuGet-Paket [Azure IoT-Dienst-SDK][lnk-nuget-service-sdk] heruntergeladen und installiert und ein Verweis auf das Paket und seine Abhängigkeiten hinzugefügt.

    ![Fenster „NuGet-Paket-Manager“][img-servicenuget]
1. Fügen Sie am Anfang der Datei **Program.cs** die folgenden `using`-Anweisungen hinzu:
    
    ```csharp
    using Microsoft.Azure.Devices;
    using Microsoft.Azure.Devices.Shared;
    ```

1. Fügen Sie die folgende `using`-Anweisung in den Standardanweisungen hinzu, sofern sie noch nicht vorhanden ist.

    ```csharp
    using System.Threading.Tasks;
    ```

1. Fügen Sie der **Program** -Klasse die folgenden Felder hinzu. Ersetzen Sie den Platzhalter durch die IoT Hub-Verbindungszeichenfolge für den Hub, den Sie im vorherigen Abschnitt erstellt haben.

    ```csharp
    static string connString = "{iot hub connection string}";
    static ServiceClient client;
    static JobClient jobClient;
    ```

1. Fügen Sie der **Program** -Klasse die folgende Methode hinzu:

    ```csharp
    public static async Task MonitorJob(string jobId)
    {
        JobResponse result;
        do
        {
            result = await jobClient.GetJobAsync(jobId);
            Console.WriteLine("Job Status : " + result.Status.ToString());
            Thread.Sleep(2000);
        } while ((result.Status != JobStatus.Completed) && (result.Status != JobStatus.Failed));
    }
    ```

1. Fügen Sie der **Program** -Klasse die folgende Methode hinzu:

    ```csharp
    public static async Task StartMethodJob(string jobId)
    {
        CloudToDeviceMethod directMethod = new CloudToDeviceMethod("lockDoor", TimeSpan.FromSeconds(5), TimeSpan.FromSeconds(5));

        JobResponse result = await jobClient.ScheduleDeviceMethodAsync(jobId,
            "deviceId='myDeviceId'",
            directMethod,
            DateTime.Now,
            10);

        Console.WriteLine("Started Method Job");
    }
    ```

1. Fügen Sie der **Program** -Klasse die folgende Methode hinzu:

    ```csharp
    public static async Task StartTwinUpdateJob(string jobId)
    {
        var twin = new Twin();
        twin.Properties.Desired["Building"] = "43";
        twin.Properties.Desired["Floor"] = "3";
        twin.ETag = "*";

        JobResponse result = await jobClient.ScheduleTwinUpdateAsync(jobId,
            "deviceId='myDeviceId'",
            twin,
            DateTime.Now,
            10);

        Console.WriteLine("Started Twin Update Job");
    }
    ```

1. Fügen Sie abschließend der **Main**-Methode die folgenden Zeilen hinzu:

    ```csharp
    jobClient = JobClient.CreateFromConnectionString(connString);

    string methodJobId = Guid.NewGuid().ToString();

    StartMethodJob(methodJobId);
    MonitorJob(methodJobId).Wait();
    Console.WriteLine("Press ENTER to run the next job.");
    Console.ReadLine();

    string twinUpdateJobId = Guid.NewGuid().ToString();

    StartTwinUpdateJob(twinUpdateJobId);
    MonitorJob(twinUpdateJobId).Wait();
    Console.WriteLine("Press ENTER to exit.");
    Console.ReadLine();
    ```

1. Öffnen Sie im Projektmappen-Explorer **Startprojekte festlegen**, und vergewissern Sie sich, dass als **Aktion** für das Projekt **ScheduleJob** **Starten** festgelegt ist. Erstellen Sie die Projektmappe.

## <a name="create-a-simulated-device-app"></a>Erstellen einer simulierten Geräte-App

In diesem Abschnitt erstellen Sie eine Node.js-Konsolen-App, die auf eine von der Cloud aufgerufene direkte Methode antwortet. Diese Methode löst einen Neustart eines simulierten Geräts aus und ermöglicht mithilfe der gemeldeten Eigenschaften Abfragen des Gerätezwillings zum Identifizieren von Geräten und deren letztem Neustart.

1. Erstellen Sie einen leeren Ordner mit dem Namen **simDevice**.  Erstellen Sie im Ordner **simDevice** die Datei „package.json“, indem Sie an der Eingabeaufforderung den unten angegebenen Befehl ausführen.  Übernehmen Sie alle Standardeinstellungen:

    ```cmd/sh
    npm init
    ```

1. Führen Sie an der Eingabeaufforderung im Ordner **simDevice** den folgenden Befehl aus, um die Pakete **azure-iot-device** und **azure-iot-device-mqtt** zu installieren:

    ```cmd/sh
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

1. Erstellen Sie mit einem Text-Editor im Ordner **simDevice** die neue Datei **simDevice.js**.

1. Fügen Sie am Anfang der Datei **simDevice.js** die folgenden require-Anweisungen hinzu:

    ```nodejs
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```

1. Fügen Sie die Variable **connectionString** hinzu, und verwenden Sie sie zum Erstellen einer **Client**-Instanz. Vergewissern Sie sich, dass Sie die Platzhalter durch die entsprechenden Werte für Ihre Konfiguration ersetzt haben.

    ```nodejs
    var connectionString = 'HostName={youriothostname};DeviceId={yourdeviceid};SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

1. Fügen Sie die folgende Funktion zur Behandlung der **lockDoor**-Methode hinzu.

    ```nodejs
    var onLockDoor = function(request, response) {
   
        // Respond the cloud app for the direct method
        response.send(200, function(err) {
            if (!err) {
                console.error('An error occured when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.');
            }
        });
   
        console.log('Locking Door!');
    };
    ```

1. Fügen Sie den folgenden Code hinzu, um den Handler für die **lockDoor**-Methode zu registrieren.

    ```nodejs
    client.open(function(err) {
        if (err) {
            console.error('Could not connect to IotHub client.');
        }  else {
            console.log('Client connected to IoT Hub.  Waiting for lockDoor direct method.');
            client.onDeviceMethod('lockDoor', onLockDoor);
        }
    });
    ```

1. Speichern und schließen Sie die Datei **simDevice.js**.

> [!NOTE]
> Der Einfachheit halber wird in diesem Tutorial keine Wiederholungsrichtlinie implementiert. Im Produktionscode sollten Sie Wiederholungsrichtlinien implementieren (z.B. einen exponentiellen Backoff), wie im MSDN-Artikel zum [Transient Fault Handling (Behandeln vorübergehender Fehler)][lnk-transient-faults] beschrieben.

## <a name="run-the-apps"></a>Ausführen der Apps

Sie können die Apps nun ausführen.

1. Führen Sie an der Eingabeaufforderung im Ordner **simDevice** den folgenden Befehl aus, um mit dem Lauschen auf die direkte Methode zum Neustarten zu beginnen.

    ```cmd/sh
    node simDevice.js
    ```

1. Führen Sie die C#-Konsolen-App **ScheduleJob** aus, indem Sie mit der rechten Maustaste auf das **ScheduleJob**-Projekt klicken, **Debuggen** und dann **Neue Instanz starten** auswählen.

1. Sie sehen die Ausgabe von Geräte- und Back-End-App.

    ![Führen Sie die Apps aus, um Aufträge zu planen.][img-schedulejobs]

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie einen Auftrag zum Planen einer direkten Methode für ein Gerät und eines Updates der Eigenschaften eines Gerätezwillings verwendet.

Informationen zu den weiteren Schritten mit IoT Hub und Geräteverwaltungsmustern, z.B. drahtloses Firmware-Remoteupdate, finden Sie unter [Tutorial: Durchführen eines Firmwareupdates][lnk-fwupdate].

Informationen zu den weiteren ersten Schritten mit IoT Hub finden Sie unter [Erste Schritte mit IoT Edge][lnk-iot-edge].

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-node-schedule-jobs/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-schedule-jobs/createnetapp.png
[img-schedulejobs]: media/iot-hub-csharp-node-schedule-jobs/schedulejobs.png

[lnk-get-started-twin]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-props]: iot-hub-node-node-twin-how-to-configure.md
[lnk-c2d-methods]: iot-hub-node-node-direct-methods.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-fwupdate]: iot-hub-node-node-firmware-update.md
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/

