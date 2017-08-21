---
title: "Verwenden von Azure IoT Hub-Gerätezwillingseigenschaften (.NET/.NET) | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie mithilfe von Azure IoT Hub-Gerätezwillingen Geräte konfigurieren. Sie verwenden das Azure IoT-Geräte-SDK für .NET, um eine simulierte Geräte-App zu implementieren, und das Azure IoT-Dienst-SDK für .NET, um eine Dienst-App zu implementieren, die eine Gerätekonfiguration mithilfe eines Gerätezwillings ändert."
services: iot-hub
documentationcenter: .net
author: dsk-2015
manager: timlt
editor: 
ms.assetid: 3c627476-f982-43c9-bd17-e0698c5d236d
ms.service: iot-hub
ms.devlang: csharp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/10/2017
ms.author: dkshir
ms.translationtype: HT
ms.sourcegitcommit: 2ad539c85e01bc132a8171490a27fd807c8823a4
ms.openlocfilehash: 679cda28bf3ce9fb207fe3693a3453b355f1de15
ms.contentlocale: de-de
ms.lasthandoff: 07/12/2017

---
# <a name="use-desired-properties-to-configure-devices"></a>Verwenden von gewünschten Eigenschaften zum Konfigurieren von Geräten
[!INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

Am Ende dieses Tutorials verfügen Sie über zwei .NET-Konsolen-Apps:

* **SimulateDeviceConfiguration**, eine simulierte Geräte-App, die auf eine gewünschte Konfigurationsaktualisierung wartet und den Status eines simulierten Konfigurationsaktualisierungsvorgangs meldet.
* **SetDesiredConfigurationAndQuery**, eine Back-End-App, die die gewünschte Konfiguration für ein Gerät festlegt und den Konfigurationsaktualisierungsprozess abfragt.

> [!NOTE]
> Im Artikel [Azure IoT SDKs][lnk-hub-sdks] finden Sie Informationen zu den verschiedenen Azure IoT SDKs, mit denen Sie sowohl Geräte- als auch Back-End-Apps erstellen können.
> 
> 

Für dieses Tutorial benötigen Sie Folgendes:

* Visual Studio 2015 oder Visual Studio 2017
* Ein aktives Azure-Konto. Wenn Sie nicht über ein Konto verfügen, können Sie in nur wenigen Minuten ein [kostenloses Konto][lnk-free-trial] erstellen.

Wenn Sie das Tutorial [Erste Schritte mit Gerätezwillingen][lnk-twin-tutorial] ausgeführt haben, haben Sie bereits einen IoT Hub und die Geräteidentität **myDeviceId**. In diesem Fall können Sie den Abschnitt [Erstellen der simulierten Geräte-App][lnk-how-to-configure-createapp] überspringen.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

<a id="#create-the-simulated-device-app"></a>
## <a name="create-the-simulated-device-app"></a>Erstellen der simulierten Geräte-App
In diesem Abschnitt erstellen Sie eine .NET-Konsolen-App, die als **myDeviceId** eine Verbindung mit dem Hub herstellt, auf eine gewünschte Konfigurationsaktualisierung wartet und dann Aktualisierungen für den simulierten Konfigurationsaktualisierungsvorgang meldet.

1. Erstellen Sie in Visual Studio mithilfe der Projektvorlage **Konsolenanwendung** ein neues Visual C#-Projekt für den klassischen Windows-Desktop. Nennen Sie das Projekt **SimulateDeviceConfiguration**.
   
    ![Neue klassische Visual C#-App für Windows][img-createdeviceapp]

1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt **SimulateDeviceConfiguration**, und klicken Sie dann auf **NuGet-Pakete verwalten...**.
1. Wählen Sie im Fenster **NuGet-Paket-Manager** die Option **Durchsuchen** aus, und suchen Sie dann nach **microsoft.azure.devices.client**. Wählen Sie **Installieren** aus, um das Paket **Microsoft.Azure.Devices.Client** zu installieren, und akzeptieren Sie die Nutzungsbedingungen. Bei diesem Verfahren wird das [NuGet-Paket mit dem Azure IoT-Geräte-SDK][lnk-nuget-client-sdk] heruntergeladen und installiert und ein Verweis auf das Paket und seine Abhängigkeiten hinzugefügt.
   
    ![Fenster „NuGet-Paket-Manager“ mit Client-App][img-clientnuget]
1. Fügen Sie am Anfang der Datei **Program.cs** die folgenden `using`-Anweisungen hinzu:
   
        using Microsoft.Azure.Devices.Client;
        using Microsoft.Azure.Devices.Shared;
        using Newtonsoft.Json;

1. Fügen Sie der **Program** -Klasse die folgenden Felder hinzu. Ersetzen Sie den Platzhalterwert durch die Geräteverbindungszeichenfolge, die Sie sich im vorherigen Abschnitt notiert haben.
   
        static string DeviceConnectionString = "HostName=<yourIotHubName>.azure-devices.net;DeviceId=<yourIotDeviceName>;SharedAccessKey=<yourIotDeviceAccessKey>";
        static DeviceClient Client = null;
        static TwinCollection reportedProperties = new TwinCollection();

1. Fügen Sie der **Program**-Klasse die folgende Methode hinzu:
 
        public static void InitClient()
        {
            try
            {
                Console.WriteLine("Connecting to hub");
                Client = DeviceClient.CreateFromConnectionString(DeviceConnectionString, TransportType.Mqtt);
            }
            catch (Exception ex)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", ex.Message);
            }
        }
    Das **Client**-Objekt macht alle Methoden verfügbar, die für die Interaktion mit Gerätezwillingen des Geräts erforderlich sind. Der oben gezeigte Code initialisiert das **Client**-Objekt und ruft dann den Gerätezwilling für **myDeviceId** ab.

1. Fügen Sie der **Program**-Klasse die folgende Methode hinzu. Diese Methode legt die Anfangswerte von Telemetriedaten auf dem lokalen Gerät fest und aktualisiert dann den Gerätezwilling.

        public static async void InitTelemetry()
        {
            try
            {
                Console.WriteLine("Report initial telemetry config:");
                TwinCollection telemetryConfig = new TwinCollection();
                
                telemetryConfig["configId"] = "0";
                telemetryConfig["sendFrequency"] = "24h";
                reportedProperties["telemetryConfig"] = telemetryConfig;
                Console.WriteLine(JsonConvert.SerializeObject(reportedProperties));

                await Client.UpdateReportedPropertiesAsync(reportedProperties);
            }
            catch (AggregateException ex)
            {
                foreach (Exception exception in ex.InnerExceptions)
                {
                    Console.WriteLine();
                    Console.WriteLine("Error in sample: {0}", exception);
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", ex.Message);
            }
        }

1. Fügen Sie der **Program**-Klasse die folgende Methode hinzu. Dies ist ein Rückruf, der eine Änderung der *gewünschten Eigenschaften* im Gerätezwilling erkennt.

        private static async Task OnDesiredPropertyChanged(TwinCollection desiredProperties, object userContext)
        {
            try
            {
                Console.WriteLine("Desired property change:");
                Console.WriteLine(JsonConvert.SerializeObject(desiredProperties));

                var currentTelemetryConfig = reportedProperties["telemetryConfig"];
                var desiredTelemetryConfig = desiredProperties["telemetryConfig"];

                if ((desiredTelemetryConfig != null) && (desiredTelemetryConfig["configId"] != currentTelemetryConfig["configId"]))
                {
                    Console.WriteLine("\nInitiating config change");
                    currentTelemetryConfig["status"] = "Pending";
                    currentTelemetryConfig["pendingConfig"] = desiredTelemetryConfig;

                    await Client.UpdateReportedPropertiesAsync(reportedProperties);

                    CompleteConfigChange();
                }
            }
            catch (AggregateException ex)
            {
                foreach (Exception exception in ex.InnerExceptions)
                {
                    Console.WriteLine();
                    Console.WriteLine("Error in sample: {0}", exception);
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", ex.Message);
            }
        }

    Diese Methode aktualisiert die gemeldeten Eigenschaften des lokalen Gerätezwillingsobjekts mit der Anforderung zur Konfigurationsaktualisierung, legt den Status auf **Ausstehend** fest und aktualisiert dann den Gerätezwilling für den Dienst. Nach der erfolgreichen Aktualisierung des Gerätezwillings führt sie die Konfigurationsänderung durch Aufrufen der im nächsten Punkt beschriebenen Methode `CompleteConfigChange` durch.

1. Fügen Sie der **Program**-Klasse die folgende Methode hinzu. Diese Methode simuliert eine Geräterücksetzung und aktualisiert dann die lokale Einstellung der gemeldeten Eigenschaften, indem sie den Status auf **Success** festgelegt und das **pendingConfig**-Element entfernt. Anschließend wird der Gerätezwilling für den Dienst aktualisiert. 

        public static async void CompleteConfigChange()
        {
            try
            {
                var currentTelemetryConfig = reportedProperties["telemetryConfig"];

                Console.WriteLine("\nSimulating device reset");
                await Task.Delay(30000); 

                Console.WriteLine("\nCompleting config change");
                currentTelemetryConfig["configId"] = currentTelemetryConfig["pendingConfig"]["configId"];
                currentTelemetryConfig["sendFrequency"] = currentTelemetryConfig["pendingConfig"]["sendFrequency"];
                currentTelemetryConfig["status"] = "Success";
                currentTelemetryConfig["pendingConfig"] = null;

                await Client.UpdateReportedPropertiesAsync(reportedProperties);
                Console.WriteLine("Config change complete \nPress any key to exit.");
            }
            catch (AggregateException ex)
            {
                foreach (Exception exception in ex.InnerExceptions)
                {
                    Console.WriteLine();
                    Console.WriteLine("Error in sample: {0}", exception);
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", ex.Message);
            }
        }

1. Fügen Sie abschließend der **Main**-Methode die folgenden Zeilen hinzu:

        try
        {
            InitClient();
            InitTelemetry();

            Console.WriteLine("Wait for desired telemetry...");
            Client.SetDesiredPropertyUpdateCallback(OnDesiredPropertyChanged, null).Wait();
            Console.ReadKey();
        }
        catch (AggregateException ex)
        {
            foreach (Exception exception in ex.InnerExceptions)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", exception);
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
        }

   > [!NOTE]
   > In diesem Tutorial wird nicht das Verhalten für gleichzeitige Konfigurationsaktualisierungen simuliert. Bei einigen Konfigurationsaktualisierungsvorgängen können Änderungen der Zielkonfiguration möglicherweise während der Ausführung der Aktualisierung erfolgen, und bei einigen müssen diese eventuell in die Warteschlange gestellt werden. Bei anderen wiederum können sie ohne eine Fehlerbedingung abgelehnt werden. Stellen Sie sicher, dass Sie das gewünschte Verhalten für den spezifischen Konfigurationsvorgang berücksichtigen, und fügen Sie vor dem Initiieren der Konfigurationsänderung die entsprechende Logik hinzu.
   > 
   > 
1. Erstellen Sie die Projektmappe, und führen Sie dann die Geräte-App durch Klicken auf **F5** von Visual Studio aus. In der Ausgabekonsole sollten die Nachrichten angezeigt werden, die angeben, dass das simulierte Gerät den Gerätezwilling abruft, die Telemetrie einrichtet und auf die Änderung der gewünschten Eigenschaft wartet. Führen Sie die App weiter aus.

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
   > Diese Anwendung fragt IoT Hub zur Veranschaulichung alle 10 Sekunden ab. Verwenden Sie Abfragen, um Berichte für Benutzer für viele Geräte zu generieren, und nicht, um Änderungen zu erkennen. Wenn für Ihre Lösung Echtzeitbenachrichtigungen von Geräteereignissen erforderlich sind, verwenden Sie [twin notifications][lnk-twin-notifications].
   > 
   > 
1. Fügen Sie abschließend der **Main**-Methode die folgenden Zeilen hinzu:
   
        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        SetDesiredConfigurationAndQuery();
        Console.WriteLine("Press any key to quit.");
        Console.ReadLine();
1. Öffnen Sie im Projektmappen-Explorer **Startprojekte festlegen**, und vergewissern Sie sich, dass **Starten** als **Aktion** für das Projekt **SetDesiredConfigurationAndQuery** festgelegt ist. Erstellen Sie die Projektmappe.
1. Während die Geräte-App **SimulateDeviceConfiguration** ausgeführt wird, führen Sie von Visual Studio aus mit **F5** die Dienst-App aus. Mit der neuen aktiven Sendehäufigkeit von fünf Minuten anstatt 24 Stunden sollte die gemeldete Konfigurationsänderung von **Pending** zu **Success** wechseln.

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
[img-servicenuget]: media/iot-hub-csharp-csharp-twin-how-to-configure/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-csharp-twin-how-to-configure/createnetapp.png
[img-deviceconfigured]: media/iot-hub-csharp-csharp-twin-how-to-configure/deviceconfigured.png
[img-createdeviceapp]: media/iot-hub-csharp-csharp-twin-how-to-configure/createdeviceapp.png
[img-clientnuget]: media/iot-hub-csharp-csharp-twin-how-to-configure/devicesdknuget.png
[img-deviceconfigured]: media/iot-hub-csharp-csharp-twin-how-to-configure/deviceconfigured.png


<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-nuget-client-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/1.1.0/

[lnk-query]: iot-hub-devguide-query-language.md
[lnk-twin-notifications]: iot-hub-devguide-device-twins.md#back-end-operations
[lnk-twin-tutorial]: iot-hub-csharp-csharp-twin-getstarted.md
[lnk-schedule-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-iothub-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md
[lnk-how-to-configure-createapp]: iot-hub-csharp-csharp-twin-how-to-configure.md#create-the-simulated-device-app

