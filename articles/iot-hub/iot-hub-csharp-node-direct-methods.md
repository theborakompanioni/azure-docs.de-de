---
title: Verwenden von direkten Azure IoT Hub-Methoden (C#) | Microsoft Docs
description: In diesem Tutorial wird die Verwendung direkter Methoden beschrieben.
services: iot-hub
documentationcenter: 
author: nberdy
manager: timlt
editor: 
ms.assetid: ab035b8e-bff8-4e12-9536-f31d6b6fe425
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/05/2016
ms.author: nberdy
translationtype: Human Translation
ms.sourcegitcommit: 00746fa67292fa6858980e364c88921d60b29460
ms.openlocfilehash: f2af421140f4f6640578a372993c2553f8c590a4


---
# <a name="tutorial-use-direct-methods-c"></a>Tutorial: Verwenden von direkten Methoden
[!INCLUDE [iot-hub-selector-c2d-methods](../../includes/iot-hub-selector-c2d-methods.md)]

Am Ende dieses Tutorials verfügen Sie über eine .NET- und eine Node.js-Konsolenanwendung:

* **CallMethodOnDevice.sln**, eine .NET-App, die auf dem Back-End ausgeführt wird, eine Methode auf der simulierten Geräte-App aufruft und die Antwort anzeigt.
* **SimulatedDevice.js**, eine Node.js-App, die ein Gerät simuliert, das eine Verbindung mit Ihrem IoT Hub mithilfe der zuvor erstellten Geräteidentität herstellt und auf die von der Cloud aufgerufene Methode antwortet.

> [!NOTE]
> Im Artikel [IoT Hub SDKs][lnk-hub-sdks] finden Sie Informationen über die verschiedenen Azure IoT SDKs, mit denen Sie sowohl Anwendungen für Geräte als auch das zugehörige Lösungs-Back-End erstellen können.
> 
> 

Für dieses Tutorial benötigen Sie Folgendes:

* Microsoft Visual Studio 2015.
* Node.js, Version 0.10.x oder höher.
* Ein aktives Azure-Konto. (Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein [kostenloses Konto][lnk-free-trial] erstellen.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a>Erstellen einer simulierten Geräte-App
In diesem Abschnitt erstellen Sie eine Node.js-Konsolen-App, die auf eine vom Back-End aufgerufene Methode antwortet.

1. Erstellen Sie einen neuen leeren Ordner mit dem Namen **simulateddevice**. Erstellen Sie im Ordner **simulateddevice** die Datei „package.json“, indem Sie an der Eingabeaufforderung den unten angegebenen Befehl verwenden. Übernehmen Sie alle Standardeinstellungen:
   
    ```
    npm init
    ```
2. Führen Sie an der Eingabeaufforderung im Ordner **simulateddevice** den folgenden Befehl aus, um die Pakete **azure-iot-device** und **azure-iot-device-mqtt** zu installieren:
   
    ```
        npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. Erstellen Sie mit einem Text-Editor im Ordner **simulateddevice** die neue Datei **SimulatedDevice.js**.
4. Fügen Sie am Anfang der Datei **SimulatedDevice.js** die folgenden `require`-Anweisungen hinzu:
   
    ```
    'use strict';
   
    var Mqtt = require('azure-iot-device-mqtt').Mqtt;
    var DeviceClient = require('azure-iot-device').Client;
    ```
5. Fügen Sie die Variable **connectionString** hinzu, und verwenden Sie sie zum Erstellen eines Geräteclients. Ersetzen Sie **{device connection string}** durch die Verbindungszeichenfolge, die Sie im Abschnitt *Erstellen einer Geräteidentität* erstellt haben:
   
    ```
    var connectionString = '{device connection string}';
    var client = DeviceClient.fromConnectionString(connectionString, Mqtt);
    ```
6. Fügen Sie die folgende Funktion hinzu, um die direkte Methode auf dem Gerät zu implementieren:
   
    ```
    function onWriteLine(request, response) {
        console.log(request.payload);
   
        response.send(200, 'Input was written to log.', function(err) {
            if(err) {
                console.error('An error ocurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.' );
            }
        });
    }
    ```
7. Öffnen Sie die Verbindung mit dem IoT Hub, und initialisieren Sie den Methodenlistener:
   
    ```
    client.open(function(err) {
        if (err) {
            console.error('could not open IotHub client');
        }  else {
            console.log('client opened');
            client.onDeviceMethod('writeLine', onWriteLine);
        }
    });
    ```
8. Speichern und schließen Sie die Datei **SimulatedDevice.js** .

> [!NOTE]
> Der Einfachheit halber wird in diesem Tutorial keine Wiederholungsrichtlinie implementiert. Im Produktionscode sollten Sie Wiederholungsrichtlinien implementieren (z.B. einen erneuten Verbindungsversuch), wie im MSDN-Artikel zum [Behandeln vorübergehender Fehler][lnk-transient-faults] beschrieben.
> 
> 

## <a name="call-a-direct-method-on-a-device"></a>Aufrufen einer direkten Methode auf einem Gerät
In diesem Abschnitt erstellen Sie eine .NET-Konsolen-App, die eine Methode in der simulierten Geräte-App aufruft und dann die Antwort anzeigt.

1. Fügen Sie in Visual Studio in der aktuellen Projektmappe mithilfe der Projektvorlage **Konsolenanwendung** ein Visual C#-Projekt für den klassischen Windows-Desktop hinzu. Stellen Sie sicher, dass .NET-Framework-Version 4.5.1 oder höher verwendet wird. Nennen Sie das Projekt **CallMethodOnDevice**.
   
    ![Neues Visual C#-Projekt für den klassischen Windows-Desktop][10]
2. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt **CallMethodOnDevice**, und klicken Sie dann auf **NuGet-Pakete verwalten**.
3. Wählen Sie im Fenster **NuGet-Paket-Manager** die Option **Durchsuchen**, suchen Sie nach **microsoft.azure.devices**, wählen Sie zum Installieren des Pakets **Microsoft.Azure.Devices** die Option **Installieren**, und akzeptieren Sie die Nutzungsbedingungen. Bei diesem Verfahren werden das NuGet-Paket für das [Dienst SDK für Microsoft Azure IoT][lnk-nuget-service-sdk] sowie die dazugehörigen Abhängigkeiten heruntergeladen, installiert und mit einem Verweis versehen.
   
    ![Fenster „NuGet-Paket-Manager“][11]

4. Fügen Sie am Anfang der Datei **Program.cs** die folgenden `using`-Anweisungen hinzu:
   
        using System.Threading.Tasks;
        using Microsoft.Azure.Devices;
5. Fügen Sie der **Program** -Klasse die folgenden Felder hinzu. Ersetzen Sie den Platzhalterwert durch die Verbindungszeichenfolge für den IoT Hub, den Sie im vorherigen Abschnitt erstellt haben.
   
        static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";
6. Fügen Sie der **Program** -Klasse die folgende Methode hinzu:
   
        private static async Task InvokeMethod()
        {
            var methodInvocation = new CloudToDeviceMethod("writeLine") { ResponseTimeout = TimeSpan.FromSeconds(30) };
            methodInvocation.SetPayloadJson("'a line to be written'");

            var response = await serviceClient.InvokeDeviceMethodAsync("myDeviceId", methodInvocation);

            Console.WriteLine("Response status: {0}, payload:", response.Status);
            Console.WriteLine(response.GetPayloadAsJson());
        }
   
    Diese Methode ruft eine direkte Methode mit dem Namen `writeLine` auf dem `myDeviceId`-Gerät auf. Dann gibt sie die Antwort des Geräts an der Konsole aus. Beachten Sie, wie es möglich ist, einen Timeoutwert für die Antwort des Geräts anzugeben.
7. Fügen Sie abschließend der **Main** -Methode die folgenden Zeilen hinzu:
   
        serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
        InvokeMethod().Wait();
        Console.WriteLine("Press Enter to exit.");
        Console.ReadLine();

## <a name="run-the-applications"></a>Ausführen der Anwendungen
Sie können nun die Anwendungen ausführen.

1. Führen Sie an einer Befehlszeile im Ordner **simulateddevice** den folgenden Befehl aus, um mit dem Lauschen auf Methodenaufrufe von Ihrem IoT Hub zu beginnen:
   
    ```
    node SimulatedDevice.js
    ```
   
    ![][7]
2. Jetzt ist das Gerät verbunden und wartet auf Methodenaufrufe, und Sie können die .NET-App **CallMethodOnDevice** ausführen, um die Methode in der simulierten Geräte-App aufzurufen. Daraufhin sollte die Antwort des Geräts an der Konsole ausgegeben werden.
   
    ![][8]
3. Durch das Ausgeben der Meldung sehen Sie das Gerät, das auf die Methode reagiert. Außerdem können Sie die Anwendung, die die Methode aufgerufen hat, mit der angezeigten Antwort vom Gerät anzeigen:
   
    ![][9]

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie im Azure-Portal einen neuen IoT-Hub konfiguriert und anschließend in der Identitätsregistrierung des IoT-Hubs eine Geräteidentität erstellt. Sie haben diese Geräteidentität verwendet, um die simulierte Geräte-App für das Reagieren auf von der Cloud aufgerufene Methoden zu aktivieren. Sie haben außerdem eine App erstellt, die Methoden auf dem Gerät aufruft und die Antwort vom Gerät angezeigt. 

Informationen zu den weiteren ersten Schritten mit IoT Hub und zum Kennenlernen anderer IoT-Szenarien finden Sie in den folgenden Artikeln:

* [Erste Schritte mit IoT Hub]
* [Planen von Aufträgen auf mehreren Geräten][lnk-devguide-jobs]

Im Tutorial [Planen und Senden von Aufträgen][lnk-tutorial-jobs] erfahren Sie, wie Sie Ihre IoT-Lösung erweitern und Methodenaufrufe für mehrere Geräte planen.

<!-- Images. -->
[7]: ./media/iot-hub-csharp-node-direct-methods/run-simulated-device.png
[8]: ./media/iot-hub-csharp-node-direct-methods/netserviceapp.png
[9]: ./media/iot-hub-csharp-node-direct-methods/methods-output.png

[10]: ./media/iot-hub-csharp-node-direct-methods/create-identity-csharp1.png
[11]: ./media/iot-hub-csharp-node-direct-methods/create-identity-csharp2.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-devguide-methods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[Send Cloud-to-Device messages with IoT Hub]: iot-hub-csharp-csharp-c2d.md
[Process Device-to-Cloud messages]: iot-hub-csharp-csharp-process-d2c.md
[Erste Schritte mit IoT Hub]: iot-hub-node-node-getstarted.md



<!--HONumber=Nov16_HO5-->


