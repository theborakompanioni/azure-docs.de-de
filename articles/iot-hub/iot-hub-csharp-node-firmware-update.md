---
title: "Durchführen eines Firmwareupdates mit Azure IoT Hub (.NET/Node) | Microsoft Docs"
description: "Erfahren Sie, wie die Geräteverwaltung in Azure IoT Hub verwendet wird, um ein Firmwareupdate zu initiieren. Sie können das Azure IoT-Geräte-SDK für Node.js verwenden, um eine simulierte Geräte-App zu implementieren, und das Azure IoT-Dienst-SDK, um eine Service-App zu implementieren, die das Firmwareupdate auslöst."
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
ms.date: 08/30/2017
ms.author: juanpere
ms.translationtype: HT
ms.sourcegitcommit: 4f77c7a615aaf5f87c0b260321f45a4e7129f339
ms.openlocfilehash: 940be2b50fb08d5ed8fb709d979e609152628645
ms.contentlocale: de-de
ms.lasthandoff: 09/22/2017

---
# <a name="use-device-management-to-initiate-a-device-firmware-update-netnode"></a>Initiieren eines Firmwareupdates mithilfe der Geräteverwaltung (.NET/Node)
[!INCLUDE [iot-hub-selector-firmware-update](../../includes/iot-hub-selector-firmware-update.md)]

Im Tutorial [Get started with device management][lnk-dm-getstarted] (Erste Schritte mit der Geräteverwaltung) wurde erläutert, wie Sie mit einem [Gerätezwilling][lnk-devtwin] und [direkten Methoden][lnk-c2dmethod] Grundtypen für den Remoteneustart eines Geräts verwenden können. In diesem Tutorial werden die gleichen IoT Hub-Grundtypen verwendet, und Sie sehen, wie Sie ein simuliertes End-to-End-Firmwareupdate durchführen.  Dieses Muster wird in der Firmwareupdateimplementierung für das [Implementierungsbeispiel mit dem Raspberry Pi-Gerät][lnk-rpi-implementation] verwendet.

Dieses Tutorial veranschaulicht folgende Vorgehensweisen:

* Erstellen einer .NET-Konsolen-App, die die direkte firmwareUpdate-Methode in der simulierten Geräte-App über Ihre IoT Hub-Instanz aufruft
* Erstellen Sie eine simulierte Gerät-App, die eine direkte **FirmwareUpdate**-Methode implementiert. Diese Methode löst einen mehrstufigen Prozess aus, der auf das Herunterladen des Firmware-Images wartet, das Firmware-Image herunterlädt und schließlich das Firmware-Image anwendet. In allen Phasen des Updates nutzt das Gerät die gemeldeten Eigenschaften zum Berichten des Fortschritts.

Am Ende dieses Tutorials verfügen Sie über eine Node.js-Konsolen-Geräte-App und eine .NET-Konsolen-Back-End-App (C#):

**dmpatterns_fwupdate_service.js**, die auf der simulierten Geräte-App eine direkte Methode aufruft, die Antwort anzeigt und in regelmäßigen Abständen (alle 500 ms) die aktualisierten gemeldeten Eigenschaften anzeigt.

**TriggerFWUpdate**, die über die zuvor erstellte Geräteidentität eine Verbindung mit dem IoT Hub herstellt, eine direkte firmwareUpdate-Methode empfängt und einen mehrstufigen Prozess zum Simulieren eines Firmwareupdates durchläuft, bei dem auf das Herunterladen des Images gewartet, das Image heruntergeladen und das Image schließlich übernommen wird.

Für dieses Tutorial benötigen Sie Folgendes:

* Visual Studio 2015 oder Visual Studio 2017.
* Node.js Version 0.12.x oder höher, <br/>  Unter [Prepare your development environment][lnk-dev-setup] (Vorbereiten Ihrer Entwicklungsumgebung) wird beschrieben, wie Sie Node.js für dieses Tutorial unter Windows oder Linux installieren.
* Ein aktives Azure-Konto. (Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein [kostenloses Konto][lnk-free-trial] erstellen.)

Befolgen Sie die Schritte im Artikel [Erste Schritte mit der Geräteverwaltung](iot-hub-csharp-node-device-management-get-started.md), um Ihren IoT Hub zu erstellen und die IoT Hub-Verbindungszeichenfolge abzurufen.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="trigger-a-remote-firmware-update-on-the-device-using-a-direct-method"></a>Auslösen eines Remotefirmwareupdates auf dem Gerät über eine direkte Methode
In diesem Abschnitt erstellen Sie (mit C#) eine .NET Konsolen-App, die auf einem Gerät ein Remotefirmwareupdate auslöst. Die App verwendet eine direkte Methode zum Auslösen des Updates und Gerätzwillingsabfragen, um in regelmäßigen Abständen den Status des aktiven Firmwareupdates abzurufen.

1. Fügen Sie in Visual Studio in der aktuellen Projektmappe mithilfe der Projektvorlage **Konsolenanwendung** ein Visual C#-Projekt für den klassischen Windows-Desktop hinzu. Nennen Sie das Projekt **TriggerFWUpdate**.

    ![Neues Visual C#-Projekt für den klassischen Windows-Desktop][img-createapp]

1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt **TriggerFWUpdate**, und klicken Sie anschließend auf **NuGet-Pakete verwalten**.
1. Wählen Sie im Fenster **NuGet-Paket-Manager** die Option **Durchsuchen** aus, suchen Sie nach **microsoft.azure.devices**, wählen Sie zum Installieren des Pakets **Microsoft.Azure.Devices** die Option **Installieren** aus, und akzeptieren Sie die Nutzungsbedingungen. Bei diesem Verfahren wird das NuGet-Paket [Azure IoT-Dienst-SDK][lnk-nuget-service-sdk] heruntergeladen und installiert und ein Verweis auf das Paket und seine Abhängigkeiten hinzugefügt.

    ![Fenster „NuGet-Paket-Manager“][img-servicenuget]
1. Fügen Sie am Anfang der Datei **Program.cs** die folgenden `using`-Anweisungen hinzu:
   
        using Microsoft.Azure.Devices;
        using Microsoft.Azure.Devices.Shared;
        
1. Fügen Sie der **Program** -Klasse die folgenden Felder hinzu. Ersetzen Sie die mehrfachen Platzhalterwerte durch die IoT Hub-Verbindungszeichenfolge für den Hub, den Sie im vorherigen Abschnitt erstellt haben, und die ID Ihres Geräts.
   
        static RegistryManager registryManager;
        static string connString = "{iot hub connection string}";
        static ServiceClient client;
        static JobClient jobClient;
        static string targetDevice = "{deviceIdForTargetDevice}";
        
1. Fügen Sie der **Program** -Klasse die folgende Methode hinzu:
   
        public static async Task QueryTwinFWUpdateReported()
        {
            Twin twin = await registryManager.GetTwinAsync(targetDevice);
            Console.WriteLine(twin.Properties.Reported.ToJson());
        }
        
1. Fügen Sie der **Program** -Klasse die folgende Methode hinzu:

        public static async Task StartFirmwareUpdate()
        {
            client = ServiceClient.CreateFromConnectionString(connString);
            CloudToDeviceMethod method = new CloudToDeviceMethod("firmwareUpdate");
            method.ResponseTimeout = TimeSpan.FromSeconds(30);
            method.SetPayloadJson(
                @"{
                    fwPackageUri : 'https://someurl'
                }");

            CloudToDeviceMethodResult result = await client.InvokeDeviceMethodAsync(targetDevice, method);

            Console.WriteLine("Invoked firmware update on device.");
        }

1. Fügen Sie abschließend der **Main**-Methode die folgenden Zeilen hinzu:
   
        registryManager = RegistryManager.CreateFromConnectionString(connString);
        StartFirmwareUpdate().Wait();
        QueryTwinFWUpdateReported().Wait();
        Console.WriteLine("Press ENTER to exit.");
        Console.ReadLine();
        
1. Öffnen Sie im Projektmappen-Explorer **Startprojekte festlegen**, und vergewissern Sie sich, dass als **Aktion** für **TriggerFWUpdate** **Starten** festgelegt ist.

1. Erstellen Sie die Projektmappe.

[!INCLUDE [iot-hub-device-firmware-update](../../includes/iot-hub-device-firmware-update.md)]

## <a name="run-the-apps"></a>Ausführen der Apps
Sie können die Apps nun ausführen.

1. Führen Sie an der Eingabeaufforderung im Ordner **manageddevice** den folgenden Befehl aus, um mit dem Lauschen auf die direkte Methode zum Neustarten zu beginnen.
   
    ```
    node dmpatterns_fwupdate_device.js
    ```
2. Klicken Sie in Visual Studio mit der rechten Maustaste auf das Projekt **TriggerFWUpdate**, und wählen Sie **Debuggen** und dann **Neue Instanz starten** aus.

3. Die Reaktion des Geräts auf die direkte Methode wird in der Konsole angezeigt.

    ![Firmware wurde erfolgreich aktualisiert][img-fwupdate]

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie mit einer direkten Methode ein Remotefirmwareupdate auf einem Gerät ausgelöst und mithilfe der gemeldeten Eigenschaften den Fortschritt der Firmwareaktualisierung überprüft.

Im Tutorial [Planen und Senden von Aufträgen][lnk-tutorial-jobs] erfahren Sie, wie Sie Ihre IoT-Lösung erweitern und Methodenaufrufe für mehrere Geräte planen.

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-node-firmware-update/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-firmware-update/createnetapp.png
[img-fwupdate]: media/iot-hub-csharp-node-firmware-update/fwupdated.png

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-dm-getstarted]: iot-hub-node-node-device-management-get-started.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-rpi-implementation]: https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_client/samples/iothub_client_sample_mqtt_dm/pi_device
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
