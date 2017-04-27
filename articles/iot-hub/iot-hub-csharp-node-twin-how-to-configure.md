---
title: "Verwenden von Azure IoT Hub-Gerätezwillingseigenschaften (.NET/Node) | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie mithilfe von Azure IoT Hub-Gerätezwillingen Geräte konfigurieren. Sie verwenden das Azure IoT-Geräte-SDK für Node.js, um eine simulierte Geräte-App zu implementieren, und das Azure IoT-Dienst-SDK, um eine Dienst-App zu implementieren, die eine Gerätekonfiguration mithilfe eines Gerätezwillings ändert."
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: 
ms.assetid: 3c627476-f982-43c9-bd17-e0698c5d236d
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: elioda
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: e42ad1b62d4f953e23624841ddec70b1fac28058
ms.lasthandoff: 04/03/2017


---
# <a name="use-desired-properties-to-configure-devices"></a>Verwenden von gewünschten Eigenschaften zum Konfigurieren von Geräten
[!INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

Am Ende dieses Tutorials verfügen Sie über zwei Konsolen-Apps:

* **SimulateDeviceConfiguration.js**, eine simulierte Geräte-App, die auf eine gewünschte Konfigurationsaktualisierung wartet und den Status eines simulierten Konfigurationsaktualisierungsvorgangs meldet.
* **SetDesiredConfigurationAndQuery**, eine .NET-Back-End-App, die die gewünschte Konfiguration für ein Gerät festlegt und den Konfigurationsaktualisierungsprozess abfragt.

> [!NOTE]
> Im Artikel [Azure IoT SDKs][lnk-hub-sdks] finden Sie Informationen zu den verschiedenen Azure IoT SDKs, mit denen Sie sowohl Geräte- als auch Back-End-Apps erstellen können.
> 
> 

Für dieses Tutorial benötigen Sie Folgendes:

* Visual Studio 2015 oder Visual Studio 2017
* Node.js, Version 0.10.x oder höher.
* Ein aktives Azure-Konto. Wenn Sie nicht über ein Konto verfügen, können Sie in nur wenigen Minuten ein [kostenloses Konto][lnk-free-trial] erstellen.

Wenn Sie das Tutorial [Erste Schritte mit Gerätezwillingen][lnk-twin-tutorial] ausgeführt haben, haben Sie bereits einen IoT Hub und die Geräteidentität **myDeviceId**. In diesem Fall können Sie den Abschnitt [Erstellen der simulierten Geräte-App][lnk-how-to-configure-createapp] überspringen.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-the-simulated-device-app"></a>Erstellen der simulierten Geräte-App
In diesem Abschnitt erstellen Sie eine Node.js-Konsolenanwendung, die als **myDeviceId** eine Verbindung mit dem Hub herstellt, auf eine gewünschte Konfigurationsaktualisierung wartet und dann Aktualisierungen für den simulierten Konfigurationsaktualisierungsvorgang meldet.

1. Erstellen Sie einen neuen leeren Ordner mit dem Namen **simulatedeviceconfiguration**. Erstellen Sie im Ordner **simulatedeviceconfiguration** die neue Datei „package.json“, indem Sie an der Eingabeaufforderung den unten angegebenen Befehl ausführen. Übernehmen Sie alle Standardeinstellungen.
   
    ```
    npm init
    ```
1. Führen Sie an der Eingabeaufforderung im Ordner **simulatedeviceconfiguration** den folgenden Befehl aus, um das Paket **azure-iot-device** und das Paket **azure-iot-device-mqtt** zu installieren:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
1. Erstellen Sie mithilfe eines Text-Editors die neue Datei **SimulateDeviceConfiguration.js** im Ordner **simulatedeviceconfiguration**.
1. Fügen Sie in der Datei **SimulateDeviceConfiguration.js** den folgenden Code ein, und ersetzen Sie den Platzhalter **{device connection string}** durch die Geräteverbindungszeichenfolge, die Sie beim Erstellen der Geräteidentität **myDeviceId** kopiert haben:
   
        'use strict';
        var Client = require('azure-iot-device').Client;
        var Protocol = require('azure-iot-device-mqtt').Mqtt;
   
        var connectionString = '{device connection string}';
        var client = Client.fromConnectionString(connectionString, Protocol);
   
        client.open(function(err) {
            if (err) {
                console.error('could not open IotHub client');
            } else {
                client.getTwin(function(err, twin) {
                    if (err) {
                        console.error('could not get twin');
                    } else {
                        console.log('retrieved device twin');
                        twin.properties.reported.telemetryConfig = {
                            configId: "0",
                            sendFrequency: "24h"
                        }
                        twin.on('properties.desired', function(desiredChange) {
                            console.log("received change: "+JSON.stringify(desiredChange));
                            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
                            if (desiredChange.telemetryConfig &&desiredChange.telemetryConfig.configId !== currentTelemetryConfig.configId) {
                                initConfigChange(twin);
                            }
                        });
                    }
                });
            }
        });
   
    Das **Client**-Objekt macht alle Methoden verfügbar, die für die Interaktion mit Gerätezwillingen des Geräts erforderlich sind. Mit diesem Code wird das **Client**-Objekt initialisiert, der Gerätezwilling für **myDeviceId** abgerufen und dann ein Handler für die Aktualisierung der *gewünschten Eigenschaften* angefügt. Der Handler überprüft durch einen Vergleich der configIds, ob eine tatsächliche Anforderung für eine Konfigurationsänderung vorliegt, und ruft dann eine Methode zum Starten der Konfigurationsänderung auf.
   
    Beachten Sie, dass bei diesem Code der Einfachheit halber ein fest codierter Standardwert für die anfängliche Konfiguration verwendet wird. Mit einer realen App wird diese Konfiguration wahrscheinlich aus einem lokalen Speicher geladen.
   
   > [!IMPORTANT]
   > Änderungsereignisse der gewünschten Eigenschaften werden bei der Geräteverbindung immer einmal ausgegeben. Stellen Sie vor dem Ausführen einer Aktion sicher, dass bei den gewünschten Eigenschaften eine tatsächliche Änderung vorliegt.
   > 
   > 
1. Fügen Sie vor dem `client.open()`-Aufruf die folgenden Methoden hinzu:
   
        var initConfigChange = function(twin) {
            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
            currentTelemetryConfig.pendingConfig = twin.properties.desired.telemetryConfig;
            currentTelemetryConfig.status = "Pending";
   
            var patch = {
            telemetryConfig: currentTelemetryConfig
            };
            twin.properties.reported.update(patch, function(err) {
                if (err) {
                    console.log('Could not report properties');
                } else {
                    console.log('Reported pending config change: ' + JSON.stringify(patch));
                    setTimeout(function() {completeConfigChange(twin);}, 60000);
                }
            });
        }
   
        var completeConfigChange =  function(twin) {
            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
            currentTelemetryConfig.configId = currentTelemetryConfig.pendingConfig.configId;
            currentTelemetryConfig.sendFrequency = currentTelemetryConfig.pendingConfig.sendFrequency;
            currentTelemetryConfig.status = "Success";
            delete currentTelemetryConfig.pendingConfig;
   
            var patch = {
                telemetryConfig: currentTelemetryConfig
            };
            patch.telemetryConfig.pendingConfig = null;
   
            twin.properties.reported.update(patch, function(err) {
                if (err) {
                    console.error('Error reporting properties: ' + err);
                } else {
                    console.log('Reported completed config change: ' + JSON.stringify(patch));
                }
            });
        };
   
    Die **initConfigChange**-Methode aktualisiert die gemeldeten Eigenschaften des lokalen Gerätezwillingsobjekts mit der Anforderung zur Konfigurationsaktualisierung, legt den Status auf **Ausstehend** fest und aktualisiert dann den Gerätezwilling für den Dienst. Nach dem erfolgreichen Aktualisieren des Gerätezwillings wird ein lang ausgeführter Vorgang simuliert, der mit der Ausführung von **completeConfigChange** endet. Diese Methode aktualisiert die lokale Einstellung der gemeldeten Eigenschaften, indem sie den Status auf **Success** festgelegt und das **pendingConfig**-Objekt entfernt. Anschließend wird der Gerätezwilling für den Dienst aktualisiert.
   
    Beachten Sie, dass zum Einsparen von Bandbreite beim Aktualisieren der gemeldeten Eigenschaften nicht das gesamte Dokument ersetzt wird, sondern nur die zu ändernden Eigenschaften (mit dem Namen **patch** im Code oben) angegeben werden.
   
   > [!NOTE]
   > In diesem Tutorial wird nicht das Verhalten für gleichzeitige Konfigurationsaktualisierungen simuliert. Bei einigen Konfigurationsaktualisierungsvorgängen können Änderungen der Zielkonfiguration möglicherweise während der Ausführung der Aktualisierung erfolgen, und bei einigen müssen diese eventuell in die Warteschlange gestellt werden. Bei anderen wiederum können sie ohne eine Fehlerbedingung abgelehnt werden. Stellen Sie sicher, dass Sie das gewünschte Verhalten für den spezifischen Konfigurationsvorgang berücksichtigen, und fügen Sie vor dem Initiieren der Konfigurationsänderung die entsprechende Logik hinzu.
   > 
   > 
1. Führen Sie die Geräte-App aus:
   
        node SimulateDeviceConfiguration.js
   
    Die Meldung `retrieved device twin` sollte angezeigt werden. Führen Sie die App weiter aus.

## <a name="create-the-service-app"></a>Erstellen der Dienst-App
In diesem Abschnitt erstellen Sie eine .NET-Konsolen-App, die die *gewünschten Eigenschaften* für den Gerätezwilling, der **myDeviceId** zugeordnet ist, mit einem neuen Telemetriekonfigurationsobjekt aktualisiert. Anschließend werden die im IoT Hub gespeicherten Gerätezwillinge abgefragt und die Unterschiede zwischen den gewünschten und den gemeldeten Konfigurationen des Geräts angezeigt.

1. Fügen Sie in Visual Studio in der aktuellen Projektmappe mithilfe der Projektvorlage **Konsolenanwendung** ein Visual C#-Projekt für den klassischen Windows-Desktop hinzu. Nennen Sie das Projekt **SetDesiredConfigurationAndQuery**.
   
    ![Neues Visual C#-Projekt für den klassischen Windows-Desktop][img-createapp]
1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt **SetDesiredConfigurationAndQuery**, und klicken Sie dann auf **NuGet-Pakete verwalten**.
1. Wählen Sie im Fenster **NuGet-Paket-Manager** die Option **Durchsuchen** aus, suchen Sie nach **microsoft.azure.devices**, wählen Sie zum Installieren des Pakets **Microsoft.Azure.Devices** die Option **Installieren** aus, und akzeptieren Sie die Nutzungsbedingungen. Bei diesem Verfahren wird das NuGet-Paket [Azure IoT-Dienst-SDK][lnk-nuget-service-sdk] heruntergeladen und installiert und ein Verweis auf das Paket und seine Abhängigkeiten hinzugefügt.
   
    ![Fenster „NuGet-Paket-Manager“][img-servicenuget]
1. Fügen Sie am Anfang der Datei **Program.cs** die folgenden `using`-Anweisungen hinzu:
   
        using Microsoft.Azure.Devices;
        using System.Threading;
        using Newtonsoft.Json;
1. Fügen Sie der **Program** -Klasse die folgenden Felder hinzu. Ersetzen Sie den Platzhalterwert durch die IoT Hub-Verbindungszeichenfolge für den Hub, den Sie im vorherigen Abschnitt erstellt haben.
   
        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";
1. Fügen Sie der **Program** -Klasse die folgende Methode hinzu:
   
        static private async Task SetDesiredConfigurationAndQuery()
        {
            var twin = await registryManager.GetTwinAsync("myDeviceId");
            var patch = new {
                    properties = new {
                        desired = new {
                            telemetryConfig = new {
                                configId = Guid.NewGuid().ToString(),
                                sendFrequency = "5m"
                            }
                        }
                    }
                };
   
            await registryManager.UpdateTwinAsync(twin.DeviceId, JsonConvert.SerializeObject(patch), twin.ETag);
            Console.WriteLine("Updated desired configuration");
   
            while (true)
            {
                var query = registryManager.CreateQuery("SELECT * FROM devices WHERE deviceId = 'myDeviceId'");
                var results = await query.GetNextAsTwinAsync();
                foreach (var result in results)
                {
                    Console.WriteLine("Config report for: {0}", result.DeviceId);
                    Console.WriteLine("Desired telemetryConfig: {0}", JsonConvert.SerializeObject(result.Properties.Desired["telemetryConfig"], Formatting.Indented));
                    Console.WriteLine("Reported telemetryConfig: {0}", JsonConvert.SerializeObject(result.Properties.Reported["telemetryConfig"], Formatting.Indented));
                    Console.WriteLine();
                }
                Thread.Sleep(10000);
            }
        }
   
    Das **Registry**-Objekt macht alle Methoden verfügbar, die für die Interaktion mit Gerätezwillingen des Diensts erforderlich sind. Mit diesem Code wird das **Registry**-Objekt initialisiert, der Gerätezwilling für **myDeviceId** wird abgerufen, und dann werden die zugehörigen gewünschten Eigenschaften mit einem neuen Telemetriekonfigurationsobjekt aktualisiert.
    Danach werden die im IoT Hub gespeicherten Gerätezwillinge alle 10 Sekunden abgefragt und die gewünschten und gemeldeten Telemetriekonfigurationen gedruckt. Informationen zum Generieren von umfassenden Berichten für alle Ihre Geräte finden Sie im Artikel zur [IoT Hub-Abfragesprache][lnk-query].
   
   > [!IMPORTANT]
   > Diese Anwendung fragt IoT Hub zur Veranschaulichung alle 10 Sekunden ab. Verwenden Sie Abfragen, um Berichte für Benutzer für viele Geräte zu generieren, und nicht, um Änderungen zu erkennen. Wenn für Ihre Lösung Echtzeitbenachrichtigungen von Geräteereignissen erforderlich sind, verwenden Sie [Gerät-zu-Cloud-Nachrichten][lnk-d2c].
   > 
   > 
1. Fügen Sie abschließend der **Main**-Methode die folgenden Zeilen hinzu:
   
        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        SetDesiredConfigurationAndQuery();
        Console.WriteLine("Press any key to quit.");
        Console.ReadLine();
1. Öffnen Sie im Projektmappen-Explorer **Startprojekte festlegen**, und vergewissern Sie sich, dass **Starten** als **Aktion** für das Projekt **SetDesiredConfigurationAndQuery** festgelegt ist. Erstellen Sie die Projektmappe.
1. Führen sie gleichzeitig mit **SimulateDeviceConfiguration.js** in Visual Studio mit **F5** die .NET-Anwendung aus. Mit der neuen aktiven Sendehäufigkeit von fünf Minuten anstatt 24 Stunden sollte sich die gemeldete Konfigurationsänderung von **Success** in **Pending** und wieder in **Success** ändern.

 ![Gerät wurde erfolgreich konfiguriert][img-deviceconfigured]
   
   > [!IMPORTANT]
   > Es gibt eine Verzögerung von bis zu einer Minute zwischen dem Vorgang für den Gerätebericht und dem Ergebnis der Abfrage. Dies ermöglicht die Abfrageinfrastruktur mit umfangreicher Skalierung. Zum Abrufen einheitlicher Ansichten eines einzelnen Gerätezwillings verwenden Sie die **getDeviceTwin**-Methode in der **Registry**-Klasse.
   > 
   > 

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie eine gewünschte Konfiguration als *gewünschte Eigenschaften* im Lösungs-Back-End festgelegt und eine Geräte-App geschrieben, die diese Änderung erkennt und einen Aktualisierungsvorgang in mehreren Schritten simuliert, bei dem der entsprechende Status als gemeldete Eigenschaften gemeldet wird.

Weitere Informationen finden Sie in den folgenden Ressourcen:

* Senden von Telemetriedaten von Geräten im Tutorial [Erste Schritte mit IoT Hub][lnk-iothub-getstarted]
* Planen oder Durchführen von Vorgängen für eine Vielzahl von Geräten im Tutorial [Schedule and broadcast jobs][lnk-schedule-jobs] (Planen und Übertragen von Aufträgen)
* Interaktive Steuerung von Geräten (z.B. Einschalten eines Lüfters über eine benutzergesteuerte App) im Tutorial [Use direct methods][lnk-methods-tutorial] (Verwenden von direkten Methoden)

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-node-twin-how-to-configure/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-twin-how-to-configure/createnetapp.png
[img-deviceconfigured]: media/iot-hub-csharp-node-twin-how-to-configure/deviceconfigured.png

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/1.1.0/

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-dm-overview]: iot-hub-device-management-overview.md
[lnk-twin-tutorial]: iot-hub-node-node-twin-getstarted.md
[lnk-schedule-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md

[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier

[lnk-how-to-configure-createapp]: iot-hub-node-node-twin-how-to-configure.md#create-the-simulated-device-app

