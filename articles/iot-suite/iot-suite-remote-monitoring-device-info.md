<properties
 pageTitle="Geräteinformationen-Metadaten in der Lösung für die Remoteüberwachung | Microsoft Azure"
 description="Eine Beschreibung der vorkonfigurierten Lösung für Remoteüberwachung von Azure IoT und deren Architektur."
 services=""
 suite="iot-suite"
 documentationCenter=""
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-suite"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/12/2016"
 ms.author="dobett"/>

# Geräteinformationen-Metadaten in der vorkonfigurierten Lösung für die Remoteüberwachung

Die vorkonfigurierte Lösung für die Azure IoT Suite-Remoteüberwachung ist ein Ansatz zum Verwalten von Gerätemetadaten. In diesem Artikel wird der Ansatz beschrieben, der von dieser Lösung verwendet wird, um folgende Fragen zu beantworten:

- Welche Gerätemetadaten werden von der Lösung gespeichert?
- Wie verwaltet die Lösung die Gerätemetadaten?

## Kontext

Für die vorkonfigurierte Lösung für die Remoteüberwachung wird [Azure IoT Hub][lnk-iot-hub] verwendet, um für Ihre Geräte das Senden von Daten an die Cloud zu ermöglichen. IoT Hub enthält eine [Geräteidentitätsregistrierung][lnk-identity-registry], um den Zugriff auf IoT Hub zu steuern. Die IoT Hub-Geräteidentitätsregistrierung ist von der speziellen *Geräteregistrierung* der Remoteüberwachungslösung getrennt, in der Geräteinformationen-Metadaten gespeichert werden. Für die Remoteüberwachungslösung wird eine [DocumentDB][lnk-docdb]-Datenbank verwendet, um die Geräteregistrierung zum Speichern von Geräteinformationen-Metadaten zu implementieren. Unter [Microsoft Azure IoT Reference Architecture][lnk-ref-arch] \(Microsoft Azure IoT-Referenzarchitektur) wird die Rolle der Geräteregistrierung in einer typischen IoT-Lösung beschrieben.

> [AZURE.NOTE] Die vorkonfigurierte Lösung für die Remoteüberwachung sorgt dafür, dass die Geräteidentitätsregistrierung mit der Geräteregistrierung synchron bleibt. Beide nutzen die gleiche Geräte-ID, um alle Geräte, die mit IoT Hub verbunden werden, eindeutig zu identifizieren.

Unter [IoT Hub-Geräteverwaltung (Vorschau)][lnk-dm-preview] werden IoT Hub Features hinzugefügt, die den in diesem Artikel beschriebenen Geräteinformationen-Verwaltungsfeatures ähneln. Für die Remoteüberwachungslösung werden derzeit aber nur die allgemein verfügbaren Features in IoT Hub genutzt.

## Geräteinformationen-Metadaten

Ein JSON-Dokument mit Geräteinformationen-Metadaten, das in der DocumentDB-Datenbank der Geräteregistrierung gespeichert ist, hat die folgende Struktur:

```
{
  "DeviceProperties": {
    "DeviceID": "deviceid1",
    "HubEnabledState": null,
    "CreatedTime": "2016-04-25T23:54:01.313802Z",
    "DeviceState": "normal",
    "UpdatedTime": null
    },
  "SystemProperties": {
    "ICCID": null
  },
  "Commands": [],
  "CommandHistory": [],
  "IsSimulatedDevice": false,
  "id": "fe81a81c-bcbc-4970-81f4-7f12f2d8bda8"
}
```

- **DeviceProperties**: Das Gerät selbst schreibt diese Eigenschaften, und das Gerät ist die Autorität für diese Daten. Andere Beispiele für Geräteeigenschaften sind Hersteller, Modellnummer und Seriennummer.
- **DeviceID**: Die eindeutige Geräte-ID. Dieser Wert entspricht dem Wert in der IoT Hub-Geräteidentitätsregistrierung.
- **HubEnabledState**: Der Status des Geräts im IoT Hub. Dieser Wert wird anfänglich auf **null** festgelegt, bis das Gerät die erste Verbindung herstellt. Im Lösungsportal wird ein **null**-Wert so dargestellt, dass das Gerät „registriert, aber nicht vorhanden“ ist.
- **CreatedTime**: Der Zeitpunkt, zu dem das Gerät erstellt wurde.
- **DeviceState**: Der vom Gerät gemeldete Zustand.
- **UpdatedTime**: Der Zeitpunkt, zu dem das Gerät im Lösungsportal zum letzten Mal aktualisiert wurde.
- **SystemProperties**: Das Lösungsportal schreibt die Systemeigenschaften, und das Gerät besitzt keinerlei Informationen zu diesen Eigenschaften. Ein Beispiel für eine Systemeigenschaft ist die **ICCID**, wenn die Lösung für einen Dienst autorisiert ist, der SIM-fähige Geräte verwaltet, und mit diesem Dienst verbunden ist.
- **Commands**: Eine Liste mit den vom Gerät unterstützten Befehlen. Das Gerät stellt diese Informationen für die Lösung bereit.
- **CommandHistory**: Eine Liste mit den Befehlen, die von der Lösung für die Remoteüberwachung an das Gerät gesendet werden, und mit dem Status dieser Befehle.
- **IsSimulatedDevice**: Ein Flag, mit dem ein Gerät als simuliertes Gerät identifiziert wird.
- **id**: Der eindeutige DocumentDB-Bezeichner für dieses Gerätedokument.

> [AZURE.NOTE] Geräteinformationen können auch Metadaten zum Beschreiben der Telemetriedaten sein, die vom Gerät an IoT Hub gesendet werden. Die Lösung für die Remoteüberwachung verwendet diese Telemetriemetadaten, um anzupassen, wie im Dashboard [dynamische Telemetriedaten][lnk-dynamic-telemetry] angezeigt werden.

## Lebenszyklus

Wenn Sie ein Gerät zum ersten Mal im Lösungsportal erstellen, erstellt die Lösung wie früher gezeigt einen Eintrag in der Geräteregistrierung. Für den Großteil dieser Informationen wird anfänglich ein Stub ausgeführt, und **HubEnabledState** wird auf **null** festgelegt. An diesem Punkt erstellt die Lösung außerdem einen Eintrag für das Gerät in der Geräteidentitätsregistrierung, um die Schlüssel zu erstellen, die vom Gerät für die Authentifizierung bei IoT Hub verwendet werden.

Wenn ein Gerät zum ersten Mal eine Verbindung mit der Lösung herstellt, sendet es eine Geräteinformationsnachricht. Diese Geräteinformationsnachricht enthält Geräteeigenschaften, z.B. den Gerätehersteller, die Modellnummer und die Seriennummer. Außerdem enthält eine Geräteinformationsnachricht eine Liste mit den vom Gerät unterstützten Befehlen, z.B. Informationen zu Befehlsparametern. Wenn die Lösung diese Nachricht erhält, werden die Geräteinformationen-Metadaten in der Geräteregistrierung aktualisiert.

### Anzeigen und Bearbeiten von Geräteinformationen im Lösungsportal

Die Geräteliste im Lösungsportal zeigt die folgenden Geräteeigenschaften als Spalten an: **Status** (Status), **DeviceId** (Geräte-ID), **Manufacturer** (Hersteller), **Model Number** (Modellnummer), **Serial Number** (Seriennummer), **Firmware** (Firmware), **Platform** (Plattform), **Processor** (Prozessor) und **Installed RAM** (Installierter RAM). Die Geräteeigenschaften **Latitude** (Breitengrad) und **Longitude** (Längengrad) sind die Grundlage für den Bing Map-Standort im Dashboard.

![Geräteliste][img-device-list]

Wenn Sie im Lösungsportal unter **Gerätedetails** auf **Bearbeiten** klicken, können Sie diese Eigenschaften bearbeiten. Die Bearbeitung dieser Eigenschaften aktualisiert den Datensatz für das Gerät in der DocumentDB-Datenbank. Wenn ein Gerät jedoch eine aktualisierte Geräteinformationsnachricht sendet, überschreibt es alle im Lösungsportal vorgenommenen Änderungen. Sie können die Eigenschaften **DeviceId**, **Hostname**, **HubEnabledState**, **CreatedTime**, **DeviceState** und **UpdatedTime** im Lösungsportal nicht bearbeiten, da nur das Gerät über die Autorität für diese Eigenschaften verfügt.

![Gerät bearbeiten][img-device-edit]

Sie können das Lösungsportal verwenden, um ein Gerät aus der Lösung zu entfernen. Wenn Sie ein Gerät entfernen, entfernt die Lösung die Geräteinformationen-Metadaten aus der Lösungsgerätregistrierung und außerdem den Geräteeintrag in der IoT Hub-Geräteidentitätsregistrierung. Bevor Sie ein Gerät entfernen können, müssen Sie es deaktivieren.

![Gerät entfernen][img-device-remove]

## Verarbeitung der Geräteinformationsnachricht

Von einem Gerät gesendete Geräteinformationsnachrichten unterscheiden sich von Telemetrienachrichten. Geräteinformationsnachrichten enthalten Informationen wie Geräteeigenschaften, die Befehle, auf die ein Gerät reagieren kann, und einen beliebigen Befehlsverlauf. IoT Hub selbst verfügt über keinerlei Informationen über die Metadaten, die in einer Geräteinformationsnachricht enthalten sind, und verarbeitet die Nachricht wie alle anderen D2C-Nachrichten (Device-to-Cloud, Gerät-zu-Cloud) auch. In der Lösung für die Remoteüberwachung liest ein [Azure Stream Analytics][lnk-stream-analytics]-Auftrag (ASA) die Nachrichten aus IoT Hub. Der **DeviceInfo**-Stream Analytics-Auftrag filtert nach Nachrichten, die **"ObjectType": "DeviceInfo"** enthalten, und leitet sie an die **EventProcessorHost**-Hostinstanz weiter, die in einem Webauftrag ausgeführt wird. Die Logik in der **EventProcessorHost**-Instanz verwendet die Geräte-ID, um den DocumentDB-Eintrag für das jeweilige Gerät zu ermitteln und den Eintrag zu aktualisieren. Der Geräteregistrierungseintrag enthält jetzt Informationen, z.B. Geräteeigenschaften, Befehle und den Befehlsverlauf.

> [AZURE.NOTE] Eine Geräteinformationsnachricht ist eine D2C-Standardnachricht. Die Lösung unterscheidet mithilfe von ASA-Abfragen zwischen Geräteinformationsnachrichten und Telemetrienachrichten.

## Beispiele für Geräteinformationseinträge

Die vorkonfigurierte Lösung für die Remoteüberwachung verwendet zwei Arten von Geräteinformationseinträgen: Einträge für die simulierten Geräte, die mit der Lösung bereitgestellt werden, und Einträge für die benutzerdefinierten Geräte, für die Sie eine Verbindung mit der Lösung herstellen.

### Simuliertes Gerät

Im folgenden Beispiel wird der JSON-Geräteinformationseintrag für ein simuliertes Gerät veranschaulicht. Dieser Eintrag verfügt über einen Wert, der für **UpdatedTime** festgelegt ist. So wird angegeben, dass das Gerät eine **DeviceInfo**-Nachricht an IoT Hub gesendet hat. Der Eintrag enthält einige allgemeine Geräteeigenschaften und definiert die sechs von den simulierten Geräten unterstützten Befehle. Das Flag **IsSimulatedDevice** ist auf **1** festgelegt.

```
{
  "DeviceProperties": {
    "DeviceID": "SampleDevice001_455",
    "HubEnabledState": true,
    "CreatedTime": "2016-01-26T19:02:01.4550695Z",
    "DeviceState": "normal",
    "UpdatedTime": "2016-06-01T15:28:41.8105157Z",
    "Manufacturer": "Contoso Inc.",
    "ModelNumber": "MD-369",
    "SerialNumber": "SER9009",
    "FirmwareVersion": "1.39",
    "Platform": "Plat-34",
    "Processor": "i3-2191",
    "InstalledRAM": "3 MB",
    "Latitude": 47.583582,
    "Longitude": -122.130622
  },
  "Commands": [
    {
      "Name": "PingDevice",
      "Parameters": null
    },
    {
      "Name": "StartTelemetry",
      "Parameters": null
    },
    {
      "Name": "StopTelemetry",
      "Parameters": null
    },
    {
      "Name": "ChangeSetPointTemp",
      "Parameters": [
        {
          "Name": "SetPointTemp",
          "Type": "double"
        }
      ]
    },
    {
      "Name": "DiagnosticTelemetry",
      "Parameters": [
        {
          "Name": "Active",
          "Type": "boolean"
        }
      ]
    },
    {
      "Name": "ChangeDeviceState",
      "Parameters": [
        {
          "Name": "DeviceState",
          "Type": "string"
        }
      ]
    }
  ],
  "CommandHistory": [],
  "IsSimulatedDevice": 1,
  "Version": "1.0",
  "ObjectType": "DeviceInfo",
  "IoTHub": {
    "MessageId": null,
    "CorrelationId": null,
    "ConnectionDeviceId": "SampleDevice001_455",
    "ConnectionDeviceGenerationId": "635894317219942540",
    "EnqueuedTime": "0001-01-01T00:00:00",
    "StreamId": null
  },
  "SystemProperties": {
    "ICCID": null
  },
  "id": "7101c002-085f-4954-b9aa-7466980a2aaf"
}
```

### Benutzerdefiniertes Gerät

Das folgende Beispiel enthält den JSON-Geräteinformationseintrag für ein benutzerdefiniertes Gerät, und das Flag **IsSimulatedDevice** ist auf **0** festgelegt. Sie sehen, dass dieses benutzerdefinierte Gerät zwei Befehle unterstützt, und dass das Lösungsportal den Befehl **SetTemperature** an das Gerät gesendet hat:

```
{
  "DeviceProperties": {
    "DeviceID": "mydevice01",
    "HubEnabledState": true,
    "CreatedTime": "2016-03-28T21:05:06.6061104Z",
    "DeviceState": "normal",
    "UpdatedTime": "2016-06-07T22:05:34.2802549Z"
  },
  "SystemProperties": {
    "ICCID": null
  },
  "Commands": [
    {
      "Name": "SetHumidity",
      "Parameters": [
        {
          "Name": "humidity",
          "Type": "int"
        }
      ]
    },
    {
      "Name": "SetTemperature",
      "Parameters": [
        {
          "Name": "temperature",
          "Type": "int"
        }
      ]
    }
  ],
  "CommandHistory": [
    {
      "Name": "SetTemperature",
      "MessageId": "2a0cec61-5eca-4de7-92dc-9c0bc4211c46",
      "CreatedTime": "2016-06-07T21:05:18.140796Z",
      "Parameters": {
        "temperature": 20
      },
      "UpdatedTime": "2016-06-07T21:05:18.716076Z",
      "Result": "Expired"
    }
  ],
  "IsSimulatedDevice": 0,
  "id": "6184ae0f-2d94-4fbd-91cd-4b193aecc9d1",
  "ObjectType": "DeviceInfo",
  "Version": "1.0",
  "IoTHub": {
    "MessageId": null,
    "CorrelationId": null,
    "ConnectionDeviceId": "SampleCustom",
    "ConnectionDeviceGenerationId": "635947959068246845",
    "EnqueuedTime": "0001-01-01T00:00:00",
    "StreamId": null
  }
}
```

Unten ist die **DeviceInfo**-JSON-Nachricht angegeben, die vom Gerät zum Aktualisieren der Geräteinformationen-Metadaten gesendet wurde:

```
{ "ObjectType":"DeviceInfo",
  "Version":"1.0",
  "IsSimulatedDevice":false,
  "DeviceProperties": { "DeviceID":"mydevice01", "HubEnabledState":true },
  "Commands": [
    {"Name":"SetHumidity", "Parameters":[{"Name":"humidity","Type":"double"}]},
    {"Name":"SetTemperature", "Parameters":[{"Name":"temperature","Type":"double"}]}
  ]
}
```

## Nächste Schritte

Nachdem Sie erfahren haben, wie Sie die vorkonfigurierten Lösungen anpassen, können Sie einige der anderen Features und Funktionen der vorkonfigurierter IoT Suite-Lösungen ausprobieren:

- [Übersicht über die vorkonfigurierte Lösung für vorhersagbaren Wartungsbedarf][lnk-predictive-overview]
- [Häufig gestellte Fragen zu IoT Suite][lnk-faq]
- [IoT-Sicherheit von Grund auf][lnk-security-groundup]



<!-- Images and links -->
[img-device-list]: media/iot-suite-remote-monitoring-device-info/image1.png
[img-device-edit]: media/iot-suite-remote-monitoring-device-info/image2.png
[img-device-remove]: media/iot-suite-remote-monitoring-device-info/image3.png

[lnk-iot-hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-identity-registry]: ../iot-hub/iot-hub-devguide.md#device-identity-registry
[lnk-docdb]: https://azure.microsoft.com/documentation/services/documentdb/
[lnk-ref-arch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
[lnk-stream-analytics]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-dm-preview]: ../iot-hub/iot-hub-device-management-overview.md
[lnk-dynamic-telemetry]: iot-suite-dynamic-telemetry.md

[lnk-predictive-overview]: iot-suite-predictive-overview.md
[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md

<!---HONumber=AcomDC_0914_2016-->