---
title: "Anpassen von vorkonfigurierten Lösungen | Microsoft Docs"
description: "Dieses Dokument bietet eine Anleitung zum Anpassen vorkonfigurierter Azure IoT Suite-Lösungen."
services: 
suite: iot-suite
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 4653ae53-4110-4a10-bd6c-7dc034c293a8
ms.service: iot-suite
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/15/2017
ms.author: corywink
ms.translationtype: Human Translation
ms.sourcegitcommit: 9568210d4df6cfcf5b89ba8154a11ad9322fa9cc
ms.openlocfilehash: bdf4cd89d5ad0392337dfe761108608d506adf18
ms.contentlocale: de-de
ms.lasthandoff: 05/15/2017

---
# <a name="customize-a-preconfigured-solution"></a>Anpassen vorkonfigurierter Lösungen

Die vorkonfigurierten Lösungen in der Azure IoT Suite veranschaulichen das Bereitstellen einer umfassenden Lösung basierend auf dem Zusammenwirken der Dienste in der Suite. Von hier ausgehend gibt es verschiedene Möglichkeiten zur Erweiterung und Anpassung der Lösung für bestimmte Szenarios. Diese allgemeinen Anpassungsmöglichkeiten werden in den folgenden Abschnitten beschrieben.

## <a name="find-the-source-code"></a>Suchen des Quellcodes

Der Quellcode für die vorkonfigurierten Lösungen ist auf GitHub in den folgenden Repositorys verfügbar:

* Remoteüberwachung: [https://www.github.com/Azure/azure-iot-remote-monitoring](https://github.com/Azure/azure-iot-remote-monitoring)
* Vorhersagbarer Wartungsbedarf: [https://github.com/Azure/azure-iot-predictive-maintenance](https://github.com/Azure/azure-iot-predictive-maintenance)
* Verbundene Factory: [https://github.com/Azure/azure-iot-connected-factory](https://github.com/Azure/azure-iot-connected-factory)

Der Quellcode für die vorkonfigurierten Lösungen wird bereitgestellt, um Muster und Verfahren zum Implementieren der umfassenden Funktionalität von IoT-Lösungen mithilfe von Azure IoT Suite zu veranschaulichen. Weitere Informationen zur Erstellung und Bereitstellung von Lösungen finden Sie in den GitHub-Repositorys.

## <a name="change-the-preconfigured-rules"></a>Ändern der vorkonfigurierten Regeln

Die Remoteüberwachungslösung enthält drei [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/)-Aufträge, um Geräteinformationen, Telemetriedaten und Regellogik in der Lösung zu behandeln.

Die drei Stream Analytics-Aufträge und ihre Syntax werden ausführlich unter [Exemplarische Vorgehensweise zur vorkonfigurierten Lösung für Remoteüberwachung](iot-suite-remote-monitoring-sample-walkthrough.md) beschrieben. 

Sie können diese Aufträge direkt bearbeiten, um die Logik zu ändern oder spezifische Logik für das Szenario hinzuzufügen. Sie finden die Stream Analytics-Aufträge wie folgt:

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com).
2. Navigieren Sie zu der Ressourcengruppe mit demselben Namen wie Ihre IoT-Lösung. 
3. Wählen Sie den Azure Stream Analytics-Auftrag aus, den Sie ändern möchten. 
4. Beenden Sie den Auftrag, indem Sie aus den Befehlen **Beenden**auswählen. 
5. Bearbeiten Sie die Eingaben, die Abfrage und die Ausgaben.
   
    Eine einfache Änderung besteht darin, die Abfrage für den Auftrag **Regeln** so zu ändern, dass **„<“** anstelle von **„>“** verwendet wird. Das Lösungsportal zeigt weiterhin **„>“** an, wenn Sie eine Regel bearbeiten, aber beachten Sie, wie sich das Verhalten aufgrund der Änderung am zugrunde liegenden Auftrag umkehrt.
6. Starten des Auftrags

> [!NOTE]
> Das Remoteüberwachungs-Dashboard hängt von bestimmten Daten ab. Deshalb kann das Ändern der Aufträge dazu führen, dass das Dashboard fehlschlägt.

## <a name="add-your-own-rules"></a>Hinzufügen eigener Regeln

Sie können nicht nur die vorkonfigurierten Azure Stream Analytics-Aufträge ändern, sondern auch im Azure-Portal neue Aufträge hinzufügen oder vorhandenen Aufträgen neue Abfragen hinzufügen.

## <a name="customize-devices"></a>Anpassen von Geräten

Eine der am häufigsten Erweiterungsaktivitäten ist das Arbeiten mit speziellen Geräten für Ihr Szenario. Es gibt mehrere Methoden zum Arbeiten mit Geräten. Dazu gehören das Anpassen eines simulierten Geräts an Ihr Szenario und das Verknüpfen des physischen Geräts mit der Lösung mithilfe des [IoT-Geräte-SDK][IoT Device SDK].

Unter [Verbinden Ihres Geräts mit der vorkonfigurierten Remoteüberwachungslösung](iot-suite-connecting-devices.md) und im [Beispiel für ein Remoteüberwachungs-SDK für C](https://github.com/Azure/azure-iot-sdk-c/tree/master/serializer/samples/remote_monitoring) finden Sie eine schrittweise Anleitung zum Hinzufügen von Geräten. Das Beispiel ist für die Verwendung mit der vorkonfigurierten Remoteüberwachungslösung ausgelegt.

### <a name="create-your-own-simulated-device"></a>Erstellen eines eigenen simulierten Geräts

Der [Quellcode der Remoteüberwachungslösung](https://github.com/Azure/azure-iot-remote-monitoring) enthält einen .NET-Simulator. Dieser Simulator wird als Teil der Lösung bereitgestellt, und Sie können ihn ändern, um andere Metadaten oder Telemetriedaten zu senden und auf andere Befehle und Methoden zu reagieren.

Der vorkonfigurierte Simulator in der vorkonfigurierten Remoteüberwachungslösung simuliert ein kühleres Gerät, das Temperatur- und Luftfeuchtigkeits-Telemetriedaten ausgibt. Sie können den Simulator im [Simulator.WebJob](https://github.com/Azure/azure-iot-remote-monitoring/tree/master/Simulator/Simulator.WebJob)-Projekt ändern, wenn Sie das GitHub-Repository aufgesucht haben.

### <a name="available-locations-for-simulated-devices"></a>Verfügbare Standorte für simulierte Geräte

Die Standorte befinden sich standardmäßig in Seattle/Redmond, Washington, USA. Diese Standorte können in [SampleDeviceFactory.cs][lnk-sample-device-factory] geändert werden.

### <a name="add-a-desired-property-update-handler-to-the-simulator"></a>Hinzufügen eines Updatehandlers für gewünschte Eigenschaften zum Simulator

Sie können einen Wert für eine gewünschte Eigenschaft für ein Gerät im Lösungsportal festlegen. Es ist die Zuständigkeit des Geräts, die Änderungsanforderung für die Eigenschaft zu verarbeiten, wenn das Gerät den Wert der gewünschten Eigenschaft abruft. Sie müssen dem Simulator einen Handler hinzufügen, um Unterstützung für eine Änderung eines Eigenschaftswerts über eine gewünschte Eigenschaft hinzuzufügen.

Der Simulator beinhaltet Handler für die Eigenschaften **SetPointTemp** und **TelemetryInterval**, die Sie aktualisieren können, indem Sie im Lösungsportal gewünschte Wert festlegen.

Das folgende Beispiel zeigt den Handler für die gewünschte Eigenschaft **SetPointTemp** in der Klasse **CoolerDevice**:

```csharp
protected async Task OnSetPointTempUpdate(object value)
{
    var telemetry = _telemetryController as ITelemetryWithSetPointTemperature;
    telemetry.SetPointTemperature = Convert.ToDouble(value);

    await SetReportedPropertyAsync(SetPointTempPropertyName, telemetry.SetPointTemperature);
}
```

Diese Methode aktualisiert die Telemetriepunkttemperatur und meldet die Änderung anschließend an IoT Hub, indem eine gemeldete Eigenschaft festgelegt wird.

Sie können eigene Handler für Ihre eigenen Eigenschaften hinzufügen, indem Sie wie im vorherigen Beispiel beschrieben vorgehen.

Sie müssen die gewünschte Eigenschaft an den Handler binden, was im folgenden Beispiel anhand des **CoolerDevice**-Konstruktors gezeigt wird:

```csharp
_desiredPropertyUpdateHandlers.Add(SetPointTempPropertyName, OnSetPointTempUpdate);
```

**SetPointTempPropertyName** ist eine Konstante, die als „Config.SetPointTemp“ definiert ist.

### <a name="add-support-for-a-new-method-to-the-simulator"></a>Hinzufügen von Unterstützung für eine neue Methode zum Simulator

Sie können den Simulator anpassen, um Unterstützung für eine neue [Methode (direkte Methode)][lnk-direct-methods] hinzuzufügen. Zwei Schritte sind hierfür erforderlich:

- Der Simulator muss IoT Hub in der vorkonfigurierten Lösung über die Details der Methode benachrichtigen.
- Der Simulator muss Code zum Verarbeiten des Methodenaufrufs umfassen, wenn Sie ihn über den Bereich **Gerätedetails** im Projektmappen-Explorer oder über einen Auftrag aufrufen.

Die vorkonfigurierte Remoteüberwachungslösung verwendet *gemeldete Eigenschaften*, um Details zu unterstützten Methoden an IoT Hub zu senden. Das Lösungs-Back-End enthält eine Liste aller Methoden, die von den einzelnen Geräten unterstützt werden, zusammen mit einem Verlauf der Methodenaufrufe. Sie können die Informationen zu Geräten und Aufrufmethoden im Lösungsportal anzeigen.

Damit der IoT Hub darüber benachrichtigt wird, dass ein Gerät eine Methode unterstützt, muss das Gerät Details der Methode zum Knoten **SupportedMethods** in den gemeldeten Eigenschaften hinzufügen:

```json
"SupportedMethods": {
  "<method signature>": "<method description>",
  "<method signature>": "<method description>"
}
```

Die Methodensignatur weist das folgende Format auf: `<method name>--<parameter #0 name>-<parameter #1 type>-...-<parameter #n name>-<parameter #n type>`. Verwenden Sie z.B. die folgende Methodensignatur, um festzulegen, dass die Methode **InitiateFirmwareUpdate** einen Zeichenfolgenparameter namens **FwPackageURI** erwartet:

```json
InitiateFirmwareUpate--FwPackageURI-string: "description of method"
```

Eine Liste der unterstützten Parametertypen finden Sie unter der Klasse **CommandTypes** im Projekt „Infrastructure“.

Legen Sie für die Methodensignatur in den gemeldeten Eigenschaften `null` fest, um eine Methode zu löschen.

> [!NOTE]
> Das Lösungs-Back-End aktualisiert nur Informationen zu unterstützten Methoden, wenn es vom Gerät eine Meldung zu *Geräteinformationen* erhält.

Das folgende Codebeispiel aus der Klasse **SampleDeviceFactory** im Projekt „Common“ zeigt, wie der Liste **SupportedMethods** in den vom Gerät gesendeten Eigenschaften eine Methode hinzugefügt wird:

```csharp
device.Commands.Add(new Command(
    "InitiateFirmwareUpdate",
    DeliveryType.Method,
    "Updates device Firmware. Use parameter 'FwPackageUri' to specifiy the URI of the firmware file, e.g. https://iotrmassets.blob.core.windows.net/firmwares/FW20.bin",
    new[] { new Parameter("FwPackageUri", "string") }
));
```

Dieser Codeausschnitt fügt Details der Methode **InitiateFirmwareUpdate** hinzu, einschließlich des im Lösungsportal anzuzeigenden Texts sowie Details der erforderlichen Methodenparameter.

Der Simulator sendet beim Start des Simulators gemeldete Eigenschaften, einschließlich der Liste unterstützter Methoden, an den IoT Hub.

Fügen Sie dem Simulatorcode für jede unterstützte Methode einen Handler hinzu. Sie können die vorhandenen Handler in der **CoolerDevice**-Klasse im Projekt „Simulator.WebJob“ sehen. Das folgende Beispiel zeigt den Handler für die Methode **InitiateFirmwareUpdate**:

```csharp
public async Task<MethodResponse> OnInitiateFirmwareUpdate(MethodRequest methodRequest, object userContext)
{
    if (_deviceManagementTask != null && !_deviceManagementTask.IsCompleted)
    {
        return await Task.FromResult(BuildMethodRespose(new
        {
            Message = "Device is busy"
        }, 409));
    }

    try
    {
        var operation = new FirmwareUpdate(methodRequest);
        _deviceManagementTask = operation.Run(Transport).ContinueWith(async task =>
        {
            // after firmware completed, we reset telemetry
            var telemetry = _telemetryController as ITelemetryWithTemperatureMeanValue;
            if (telemetry != null)
            {
                telemetry.TemperatureMeanValue = 34.5;
            }

            await UpdateReportedTemperatureMeanValue();
        });

        return await Task.FromResult(BuildMethodRespose(new
        {
            Message = "FirmwareUpdate accepted",
            Uri = operation.Uri
        }));
    }
    catch (Exception ex)
    {
        return await Task.FromResult(BuildMethodRespose(new
        {
            Message = ex.Message
        }, 400));
    }
}
```

Methodenhandlernamen müssen mit `On` beginnen, gefolgt vom Namen der Methode. Der Parameter **methodRequest** enthält Parameter, die beim Methodenaufruf vom Lösungs-Back-End übergeben werden. Der Rückgabewert muss vom Typ **Task&lt;MethodResponse&gt;** sein. Die Hilfsmethode **BuildMethodResponse** hilft Ihnen beim Erstellen des Rückgabewerts.

Im Methodenhandler können Sie folgende Aktionen ausführen:

- Starten einer asynchronen Aufgabe
- Abrufen gewünschter Eigenschaften vom *Gerätezwilling* in IoT Hub
- Aktualisieren einer einzelnen gemeldeten Eigenschaft mithilfe der Methode **SetReportedPropertyAsync** in der Klasse **CoolerDevice**.
- Aktualisieren mehrerer gemeldeter Eigenschaften durch Erstellen einer **TwinCollection**-Instanz und Aufrufen der Methode **Transport.UpdateReportedPropertiesAsync**

Im o.g. Beispiel für Firmwareupdates werden die folgenden Schritte ausgeführt:

- Überprüfen, ob das Gerät die Anforderung für ein Firmwareupdate akzeptieren kann
- Asynchrones Initiieren des Firmwareupdatevorgangs und Zurücksetzen der Telemetrie nach Abschluss des Vorgangs
- Sofortiges Ausgeben der Meldung „FirmwareUpdate accepted“ (Firmwareupdate akzeptiert), um anzugeben, dass die Anforderung vom Gerät akzeptiert wurde

### <a name="build-and-use-your-own-physical-device"></a>Erstellen und Verwenden eines eigenen (physischen) Geräts

Die [Azure IoT-SDKs](https://github.com/Azure/azure-iot-sdks) bieten Bibliotheken zum Verbinden zahlreicher Gerätetypen (Sprachen und Betriebssysteme) mit IoT-Lösungen.

## <a name="modify-dashboard-limits"></a>Ändern von Dashboardgrenzwerten

### <a name="number-of-devices-displayed-in-dashboard-dropdown"></a>Anzahl angezeigter Geräte im Dropdownfeld des Dashboards

Standardwert: 200. Diese Zahl kann in [DashboardController.cs][lnk-dashboard-controller] geändert werden.

### <a name="number-of-pins-to-display-in-bing-map-control"></a>Anzahl angezeigter Pins im Bing Karten-Steuerelement

Standardwert: 200. Diese Zahl kann in [TelemetryApiController.cs][lnk-telemetry-api-controller-01] geändert werden.

### <a name="time-period-of-telemetry-graph"></a>Zeitraum der Telemetriegrafik

Standardwert: 10 Minuten. Diesen Wert können Sie in [TelemetryApiController.cs][lnk-telemetry-api-controller-02] ändern.

## <a name="manually-set-up-application-roles"></a>Manuelles Einrichten der Anwendungsrollen

Das folgende Verfahren beschreibt das Hinzufügen von **Admin**- und **ReadOnly**-Anwendungsrollen zu einer vorkonfigurierten Lösung. In von „azureiotsuite.com“ bereitgestellten, vorkonfigurierten Lösungen sind die **Admin**- und die **ReadOnly**-Rolle bereits enthalten.

Mitglieder der **ReadOnly** -Rolle können das Dashboard und die Geräteliste anzeigen, dürfen jedoch keine Geräte hinzufügen, Geräteattribute ändern oder Befehle senden.  Mitglieder der **Admin** -Rolle haben uneingeschränkten Zugriff auf alle Funktionen in der Lösung.

1. Wechseln Sie zum [klassischen Azure-Portal][lnk-classic-portal].
2. Wählen Sie **Active Directory**aus.
3. Klicken Sie auf den Namen des AAD-Mandanten, den Sie bei der Bereitstellung Ihrer Lösung verwendet haben.
4. Klicken Sie auf **Anwendungen**.
5. Klicken Sie auf den Namen der Anwendung, der mit dem Namen der vorkonfigurierten Lösung übereinstimmt. Sollte Ihre Anwendung nicht in der Liste angezeigt werden, wählen Sie in der Dropdownliste **Anzeigen** die Option **Anwendungen im Besitz meines Unternehmens** aus, und klicken Sie auf das Häkchen.
6. Klicken Sie unten auf der Seite auf **Manifest verwalten** und dann auf **Manifest herunterladen**.
7. Durch dieses Verfahren wird eine JSON-Datei auf Ihren lokalen Computer heruntergeladen. Öffnen Sie diese Datei zur Bearbeitung in einem Text-Editor Ihrer Wahl.
8. In der dritten Zeile der JSON-Datei lesen Sie:

   ```json
   "appRoles" : [],
   ```
   Ersetzen Sie diese Zeile durch den folgenden Code:

   ```json
   "appRoles": [
   {
   "allowedMemberTypes": [
   "User"
   ],
   "description": "Administrator access to the application",
   "displayName": "Admin",
   "id": "a400a00b-f67c-42b7-ba9a-f73d8c67e433",
   "isEnabled": true,
   "value": "Admin"
   },
   {
   "allowedMemberTypes": [
   "User"
   ],
   "description": "Read only access to device information",
   "displayName": "Read Only",
   "id": "e5bbd0f5-128e-4362-9dd1-8f253c6082d7",
   "isEnabled": true,
   "value": "ReadOnly"
   } ],
   ```

9. Speichern Sie die aktualisierte JSON-Datei (Sie können die vorhandene Datei überschreiben).
10. Wählen Sie im klassischen Azure-Portal am unteren Seitenrand die Option **Manifest verwalten** und anschließend **Manifest hochladen** aus, um die zuvor gespeicherte JSON-Datei hochzuladen.
11. Sie haben nun die **Admin**- und die **ReadOnly**-Rolle Ihrer Anwendung hinzugefügt.
12. Unter [Berechtigungen für die Website „azureiotsuite.com“][lnk-permissions] erfahren Sie, wie Sie einem Benutzer in Ihrem Verzeichnis eine dieser Rollen zuweisen.

## <a name="feedback"></a>Feedback

Möchten Sie Anpassungsvorschläge zu diesem Dokument machen? Nutzen Sie [UserVoice](https://feedback.azure.com/forums/321918-azure-iot), um neue Features vorzuschlagen, oder verfassen Sie einen Kommentar zu diesem Artikel. 

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den Optionen zum Anpassen der vorkonfigurierten Lösungen finden Sie unter:

* [Verbinden einer Logik-App mit der vorkonfigurierten Remoteüberwachungslösung von Azure IoT Suite][lnk-logicapp]
* [Verwenden dynamischer Telemetriedaten mit der vorkonfigurierten Lösung für die Remoteüberwachung][lnk-dynamic]
* [Geräteinformationen-Metadaten in der vorkonfigurierten Lösung für die Remoteüberwachung][lnk-devinfo]
* [Anpassen der Anzeige von Daten von Ihren OPC UA-Servern durch die verbundene Werksprojektmappe][lnk-cf-customize]

[lnk-logicapp]: iot-suite-logic-apps-tutorial.md
[lnk-dynamic]: iot-suite-dynamic-telemetry.md
[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[IoT Device SDK]: https://azure.microsoft.com/documentation/articles/iot-hub-sdks-summary/
[lnk-permissions]: iot-suite-permissions.md
[lnk-dashboard-controller]: https://github.com/Azure/azure-iot-remote-monitoring/blob/3fd43b8a9f7e0f2774d73f3569439063705cebe4/DeviceAdministration/Web/Controllers/DashboardController.cs#L27
[lnk-telemetry-api-controller-01]: https://github.com/Azure/azure-iot-remote-monitoring/blob/3fd43b8a9f7e0f2774d73f3569439063705cebe4/DeviceAdministration/Web/WebApiControllers/TelemetryApiController.cs#L27
[lnk-telemetry-api-controller-02]: https://github.com/Azure/azure-iot-remote-monitoring/blob/e7003339f73e21d3930f71ceba1e74fb5c0d9ea0/DeviceAdministration/Web/WebApiControllers/TelemetryApiController.cs#L25 
[lnk-sample-device-factory]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Common/Factory/SampleDeviceFactory.cs#L40
[lnk-classic-portal]: https://manage.windowsazure.com
[lnk-direct-methods]: ../iot-hub/iot-hub-devguide-direct-methods.md
[lnk-cf-customize]: iot-suite-connected-factory-customize.md
