---
title: "Erste Schritte mit Azure IoT Hub-Gerätezwillingen (.NET/Node) | Microsoft Docs"
description: "Erfahren Sie, wie Sie mithilfe von Azure IoT Hub-Gerätezwillingen Tags hinzufügen und eine IoT Hub-Abfrage verwenden. Sie verwenden das Azure IoT-Geräte-SDK für Node.js, um eine simulierte Geräte-App zu implementieren, und das Azure IoT-Dienst-SDK für .NET, um eine Dienst-App zu implementieren, die die Tags hinzufügt und die IoT Hub-Abfrage ausführt."
services: iot-hub
documentationcenter: node
author: fsautomata
manager: timlt
editor: 
ms.assetid: f7e23b6e-bfde-4fba-a6ec-dbb0f0e005f4
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/29/2017
ms.author: elioda
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 07797b9159c9b926e9eb47d8864c63048951931a
ms.lasthandoff: 04/03/2017


---
# <a name="get-started-with-device-twins-netnode"></a>Erste Schritte mit Gerätezwillingen (.NET/Node)
[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

Am Ende dieses Tutorials verfügen Sie über eine .NET- und eine Node.js-Konsolen-App:

* **AddTagsAndQuery.sln**, eine .NET-Back-End-App, die Tags hinzufügt und Gerätezwillinge abfragt.
* **TwinSimulatedDevice.js**, eine Node.js-App, die ein Gerät simuliert, das eine Verbindung mit dem IoT Hub mit der zuvor erstellten Geräteidentität herstellt und seine Verbindungsbedingung meldet.

> [!NOTE]
> Im Artikel [Azure IoT SDKs][lnk-hub-sdks] finden Sie Informationen zu den verschiedenen Azure IoT SDKs, mit denen Sie sowohl Geräte- als auch Back-End-Apps erstellen können.
> 
> 

Für dieses Tutorial benötigen Sie Folgendes:

* Visual Studio 2015 oder Visual Studio 2017
* Node.js, Version 0.10.x oder höher.
* Ein aktives Azure-Konto. (Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein [kostenloses Konto][lnk-free-trial] erstellen.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-the-service-app"></a>Erstellen der Dienst-App
In diesem Abschnitt erstellen Sie eine .NET-Konsolen-App (in C#), mit der dem Gerätezwilling, der **myDeviceId** zugeordnet ist, Standortmetadaten hinzugefügt werden. Anschließend werden die in dem IoT Hub, der die Geräte in den USA auswählt, gespeicherten Gerätezwillinge abgefragt und dann diejenigen, die eine Mobilfunkverbindung gemeldet haben.

1. Fügen Sie in Visual Studio in der aktuellen Projektmappe mithilfe der Projektvorlage **Konsolenanwendung** ein Visual C#-Projekt für den klassischen Windows-Desktop hinzu. Nennen Sie das Projekt **AddTagsAndQuery**.
   
    ![Neues Visual C#-Projekt für den klassischen Windows-Desktop][img-createapp]
1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt **AddTagsAndQuery**, und klicken Sie dann auf **NuGet-Pakete verwalten**.
1. Wählen Sie im Fenster **NuGet-Paket-Manager** die Option **Durchsuchen** aus, und suchen Sie dann nach **microsoft.azure.devices**. Wählen Sie **Installieren** aus, um das Paket **Microsoft.Azure.Devices** zu installieren, und akzeptieren Sie die Nutzungsbedingungen. Bei diesem Verfahren wird das NuGet-Paket [Azure IoT-Dienst-SDK][lnk-nuget-service-sdk] heruntergeladen und installiert und ein Verweis auf das Paket und seine Abhängigkeiten hinzugefügt.
   
    ![Fenster „NuGet-Paket-Manager“][img-servicenuget]
1. Fügen Sie am Anfang der Datei **Program.cs** die folgenden `using`-Anweisungen hinzu:
   
        using Microsoft.Azure.Devices;
1. Fügen Sie der **Program** -Klasse die folgenden Felder hinzu. Ersetzen Sie den Platzhalterwert durch die IoT Hub-Verbindungszeichenfolge für den Hub, den Sie im vorherigen Abschnitt erstellt haben.
   
        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";
1. Fügen Sie der **Program** -Klasse die folgende Methode hinzu:
   
        public static async Task AddTagsAndQuery()
        {
            var twin = await registryManager.GetTwinAsync("myDeviceId");
            var patch =
                @"{
                    tags: {
                        location: {
                            region: 'US',
                            plant: 'Redmond43'
                        }
                    }
                }";
            await registryManager.UpdateTwinAsync(twin.DeviceId, patch, twin.ETag);
   
            var query = registryManager.CreateQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43'", 100);
            var twinsInRedmond43 = await query.GetNextAsTwinAsync();
            Console.WriteLine("Devices in Redmond43: {0}", string.Join(", ", twinsInRedmond43.Select(t => t.DeviceId)));
   
            query = registryManager.CreateQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43' AND properties.reported.connectivity.type = 'cellular'", 100);
            var twinsInRedmond43UsingCellular = await query.GetNextAsTwinAsync();
            Console.WriteLine("Devices in Redmond43 using cellular network: {0}", string.Join(", ", twinsInRedmond43UsingCellular.Select(t => t.DeviceId)));
        }
   
    Die **Registry**-Klasse macht alle Methoden verfügbar, die für die Interaktion mit Gerätezwillingen des Diensts erforderlich sind. Mit dem vorherigen Code werden zunächst das **registryManager**-Objekt initialisiert, dann der Gerätezwilling für **myDeviceId** abgerufen und schließlich die zugehörigen Tags mit den gewünschten Standortinformationen aktualisiert.
   
    Nach der Aktualisierung werden zwei Abfragen ausgeführt: Mit der ersten werden nur die Gerätezwillinge von Geräten in der Anlage **Redmond43** ausgewählt. Mit der zweiten wird die Abfrage so angepasst, dass nur die Geräte ausgewählt werden, die über ein Mobilfunknetz verbunden sind.
   
    Beachten Sie, dass mit diesem Code beim Erstellen des **query**-Objekts eine maximale Anzahl von zurückgegebenen Dokumenten angegeben wird. Das **query**-Objekt enthält die boolesche Eigenschaft **HasMoreResults**, mit der Sie die **GetNextAsTwinAsync**-Methoden mehrmals aufrufen können, um alle Ergebnisse abzurufen. Die **GetNextAsJson**-Methode ist für Ergebnisse verfügbar, die keine Gerätezwillinge sind, z.B. Ergebnisse von Aggregationsabfragen.
1. Fügen Sie abschließend der **Main**-Methode die folgenden Zeilen hinzu:
   
        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        AddTagsAndQuery().Wait();
        Console.WriteLine("Press Enter to exit.");
        Console.ReadLine();

1. Öffnen Sie im Projektmappen-Explorer **Startprojekte festlegen**, und vergewissern Sie sich, dass als **Aktion** für **AddTagsAndQuery** **Starten** festgelegt ist. Erstellen Sie die Projektmappe.
1. Führen Sie diese Anwendung aus, indem Sie mit der rechten Maustaste auf das Projekt **AddTagsAndQuery** klicken und **Debuggen** und anschließend **Neue Instanz starten** auswählen. In den Ergebnissen für die Abfrage, mit der alle Geräte in der Anlage **Redmond43** abgefragt werden, sollte ein Gerät angezeigt werden und keines für die Abfrage, mit der die Ergebnisse auf die über ein Mobilfunknetz verbundenen Geräte beschränkt werden.
   
    ![Abfrageergebnisse im Fenster][img-addtagapp]

Im nächsten Abschnitt erstellen Sie eine Geräte-App, mit der die Verbindungsinformationen gemeldet und das Ergebnis der im vorherigen Abschnitt ausgeführten Abfrage geändert wird.

## <a name="create-the-device-app"></a>Erstellen der Geräte-App
In diesem Abschnitt erstellen Sie eine Node.js-Konsolen-App, die als **myDeviceId** eine Verbindung mit dem Hub herstellt und dann die gemeldeten Eigenschaften so aktualisiert, dass sie die Informationen über die Verbindung mit einem Mobilfunknetz enthalten.

1. Erstellen Sie einen neuen leeren Ordner mit dem Namen **reportconnectivity**. Erstellen Sie im Ordner **reportconnectivity** die neue Datei „package.json“, indem Sie an der Eingabeaufforderung den unten angegebenen Befehl ausführen. Übernehmen Sie alle Standardeinstellungen.
   
    ```
    npm init
    ```
1. Führen Sie an der Eingabeaufforderung im Ordner **reportconnectivity** den folgenden Befehl aus, um das Paket **azure-iot-device** und das Paket **azure-iot-device-mqtt** zu installieren:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
1. Erstellen Sie mithilfe eines Text-Editors die neue Datei **ReportConnectivity.js** im Ordner **reportconnectivity**.
1. Fügen Sie in der Datei **ReportConnectivity.js** den folgenden Code ein, und ersetzen Sie den Platzhalter für die Geräteverbindungszeichenfolge durch die beim Erstellen der Geräteidentität **myDeviceId** kopierten Verbindungszeichenfolge:
   
        'use strict';
        var Client = require('azure-iot-device').Client;
        var Protocol = require('azure-iot-device-mqtt').Mqtt;
   
        var connectionString = '{device connection string}';
        var client = Client.fromConnectionString(connectionString, Protocol);
   
        client.open(function(err) {
        if (err) {
            console.error('could not open IotHub client');
        }  else {
            console.log('client opened');
   
            client.getTwin(function(err, twin) {
            if (err) {
                console.error('could not get twin');
            } else {
                var patch = {
                    connectivity: {
                        type: 'cellular'
                    }
                };
   
                twin.properties.reported.update(patch, function(err) {
                    if (err) {
                        console.error('could not update twin');
                    } else {
                        console.log('twin state reported');
                        process.exit();
                    }
                });
            }
            });
        }
        });
   
    Das **Client**-Objekt macht alle Methoden verfügbar, die für die Interaktion mit Gerätezwillingen des Geräts erforderlich sind. Mit diesem Code wird nach dem Initialisieren des **Client**-Objekts der Gerätezwilling für **myDeviceId** abgerufen und die zugehörige gemeldete Eigenschaft mit den Verbindungsinformationen aktualisiert.
1. Führen Sie die Geräte-App aus.
   
        node ReportConnectivity.js
   
    Die Meldung `twin state reported` sollte angezeigt werden.
1. Da das Gerät nun die Verbindungsinformationen gemeldet hat, sollten diese in beiden Abfragen angezeigt werden. Führen Sie die .NET-App **AddTagsAndQuery** aus, um die Abfragen erneut durchzuführen. Nun sollte **myDeviceId** in beiden Abfrageergebnisse angezeigt werden.
   
    ![][img-addtagapp2]

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie im Azure-Portal einen neuen IoT-Hub konfiguriert und anschließend in der Identitätsregistrierung des IoT-Hubs eine Geräteidentität erstellt. Sie haben Gerätemetadaten aus einer Back-End-App als Tags hinzugefügt und eine simulierte Geräte-App zum Melden von Geräteverbindungsinformationen im Gerätezwilling geschrieben. Sie haben zudem erfahren, wie diese Informationen mithilfe der SQL-ähnlichen IoT Hub-Abfragesprache abgefragt werden können.

Weitere Informationen finden Sie in den folgenden Ressourcen:

* Senden von Telemetriedaten von Geräten im Tutorial [Erste Schritte mit IoT Hub][lnk-iothub-getstarted]
* Informationen zum Konfigurieren von Geräten mit den gewünschten Eigenschaften des Gerätezwillings im Tutorial [Verwenden von gewünschten Eigenschaften zum Konfigurieren von Geräten][lnk-twin-how-to-configure]
* Informationen zur interaktiven Steuerung von Geräten (z.B. Einschalten eines Lüfters über eine benutzergesteuerte App) im Tutorial [Verwenden von direkten Methoden][lnk-methods-tutorial]

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-node-twin-getstarted/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-twin-getstarted/createnetapp.png
[img-addtagapp]: media/iot-hub-csharp-node-twin-getstarted/addtagapp.png
[img-addtagapp2]: media/iot-hub-csharp-node-twin-getstarted/addtagapp2.png

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/

[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-identity]: iot-hub-devguide-identity-registry.md

[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md
[lnk-twin-how-to-configure]: iot-hub-csharp-node-twin-how-to-configure.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md


