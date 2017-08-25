---
title: "Geräteinformationen-Metadaten in der Lösung für die Remoteüberwachung | Microsoft-Dokumentation"
description: "Eine Beschreibung der vorkonfigurierten Lösung für Remoteüberwachung von Azure IoT und deren Architektur."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 1b334769-103b-4eb0-a293-184f3d1ba9a3
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: dobett
ms.translationtype: Human Translation
ms.sourcegitcommit: 9568210d4df6cfcf5b89ba8154a11ad9322fa9cc
ms.openlocfilehash: cef08b7c9a50e02948a1fa495158f3d0fab416e9
ms.contentlocale: de-de
ms.lasthandoff: 07/06/2017

---
# <a name="device-information-metadata-in-the-remote-monitoring-preconfigured-solution"></a>Geräteinformationen-Metadaten in der vorkonfigurierten Lösung für die Remoteüberwachung

Die vorkonfigurierte Lösung für die Azure IoT Suite-Remoteüberwachung ist ein Ansatz zum Verwalten von Gerätemetadaten. In diesem Artikel wird der Ansatz beschrieben, der von dieser Lösung verwendet wird, um folgende Fragen zu beantworten:

* Welche Gerätemetadaten werden von der Lösung gespeichert?
* Wie verwaltet die Lösung die Gerätemetadaten?

## <a name="context"></a>Kontext

Für die vorkonfigurierte Lösung für die Remoteüberwachung wird [Azure IoT Hub][lnk-iot-hub] verwendet, um für Ihre Geräte das Senden von Daten an die Cloud zu ermöglichen. Die Lösung speichert Informationen zu Geräten an drei verschiedenen Speicherorten:

| Ort | Gespeicherte Informationen | Implementierung |
| -------- | ------------------ | -------------- |
| Identitätsregistrierung | Geräte-ID, Authentifizierungsschlüssel, aktivierter Zustand | In IoT Hub integriert |
| Gerätezwillinge | Metadaten: gemeldete Eigenschaften, gewünschte Eigenschaften, Tags | In IoT Hub integriert |
| Cosmos DB | Befehls- und Methodenverlauf | Benutzerdefiniert für Lösung |

IoT Hub enthält eine [Geräteidentitätsregistrierung][lnk-identity-registry] zum Verwalten des Zugriffs auf einen IoT Hub und verwendet [Gerätezwillinge][lnk-device-twin] zum Verwalten von Gerätemetadaten. Außerdem gibt es auch eine spezielle *Geräteregistrierung* der Remoteüberwachungslösung zur Speicherung des Befehls- und Methodenverlaufs. Die Remoteüberwachungslösung verwendet eine [Cosmos DB][lnk-docdb]-Datenbank um einen benutzerdefinierten Speicher für den Befehls- und Methodenverlauf zu implementieren.

> [!NOTE]
> Die vorkonfigurierte Lösung für die Remoteüberwachung sorgt dafür, dass die Geräteidentitätsregistrierung mit den Informationen in der Cosmos DB-Datenbank synchron bleibt. Beide nutzen die gleiche Geräte-ID, um alle Geräte, die mit IoT Hub verbunden werden, eindeutig zu identifizieren.

## <a name="device-metadata"></a>Gerätemetadaten

IoT Hub verwaltet einen [Gerätezwilling][lnk-device-twin] für jedes simulierte und physische Gerät, das mit einer Remoteüberwachungslösung verbunden ist. Die Lösung verwendet Gerätezwillinge zum Verwalten der Metadaten, die Geräten zugeordnet sind. Ein Gerätezwilling ist ein von IoT Hub verwaltetes JSON-Dokument, und die Lösung verwendet die IoT Hub-API für die Interaktion mit Gerätezwillingen.

Ein Gerätezwilling speichert die drei Arten von Metadaten:

- *Gemeldete Eigenschaften* werden von einem Gerät an einen IoT Hub gesendet. In der Remoteüberwachungslösung senden simulierte Geräte gemeldete Eigenschaften beim Start und als Antwort auf Befehle und Methoden zum **Ändern des Gerätestatus**. Sie können gemeldete Eigenschaften im Lösungsportal in der **Geräteliste** und den **Gerätedetails** anzeigen. Gemeldete Eigenschaften sind schreibgeschützt.
- *Gewünschte Eigenschaften* werden durch Geräte vom IoT Hub abgerufen. Das Gerät ist dafür zuständig, alle erforderlichen Konfigurationsänderungen auf dem Gerät vorzunehmen. Das Gerät ist auch dafür zuständig, die Änderung als gemeldete Eigenschaft wieder an den Hub zu melden. Sie können den Wert einer gewünschten Eigenschaftswert über das Lösungsportal festlegen.
- *Tags* sind nur im Gerätezwilling vorhanden und werden nie mit einem Gerät synchronisiert. Sie können Tag-Werte im Lösungsportal festlegen und beim Filtern der Geräteliste verwenden. Die Lösung verwendet auch ein Tag zum Identifizieren des Symbols, das im Lösungsportal für ein Gerät angezeigt wird.

Beispiele für gemeldete Eigenschaften von den simulierten Geräten sind Hersteller, Modellnummer, Breiten- und Längengrad. Simulierte Geräte geben auch die Liste der unterstützten Methoden als gemeldete Eigenschaft zurück.

> [!NOTE]
> Der simulierte Gerätecode verwendet nur die gewünschten Eigenschaften **Desired.Config.TemperatureMeanValue** und **Desired.Config.TelemetryInterval** zum Aktualisieren der gemeldeten Eigenschaften, die zurück an den IoT Hub gesendet werden. Alle anderen gewünschten Eigenschaftenänderungen werden ignoriert.

Ein JSON-Dokument mit Geräteinformationen-Metadaten, das in der Cosmos DB-Datenbank der Geräteregistrierung gespeichert ist, hat die folgende Struktur:

```json
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

> [!NOTE]
> Geräteinformationen können auch Metadaten zum Beschreiben der Telemetriedaten sein, die vom Gerät an IoT Hub gesendet werden. Die Lösung für die Remoteüberwachung verwendet diese Telemetriemetadaten, um anzupassen, wie [dynamische Telemetriedaten][lnk-dynamic-telemetry] im Dashboard angezeigt werden.

## <a name="lifecycle"></a>Lebenszyklus

Wenn Sie ein Gerät erstmals im Lösungsportal erstellen, erstellt die Lösung einen Eintrag in der Cosmos DB-Datenbank zum Speichern des Befehls- und Methodenverlaufs. An diesem Punkt erstellt die Lösung außerdem einen Eintrag für das Gerät in der Geräteidentitätsregistrierung, um die Schlüssel zu erstellen, die vom Gerät für die Authentifizierung bei IoT Hub verwendet werden. Außerdem wird ein Gerätezwilling erstellt.

Wenn ein Gerät zum ersten Mal eine Verbindung mit der Lösung herstellt, sendet es gemeldete Eigenschaften und eine Geräteinformationsnachricht. Die gemeldeten Eigenschaftswerte werden automatisch im Gerätezwilling gespeichert. Zu den gemeldeten Eigenschaften zählen Gerätehersteller, Modellnummer, Seriennummer und eine Liste der unterstützten Methoden. Die Geräteinformationsnachricht enthält die Liste mit den vom Gerät unterstützten Befehlen, einschließlich Informationen zu Befehlsparametern. Wenn die Lösung diese Nachricht erhält, werden die Geräteinformationen in der Cosmos DB-Datenbank aktualisiert.

### <a name="view-and-edit-device-information-in-the-solution-portal"></a>Anzeigen und Bearbeiten von Geräteinformationen im Lösungsportal

Die Geräteliste im Lösungsportal zeigt standardmäßig die folgenden Geräteeigenschaften als Spalten an: **Status** (Status), **DeviceId** (Geräte-ID), **Manufacturer** (Hersteller), **Model Number** (Modellnummer), **Serial Number** (Seriennummer), **Firmware** (Firmware), **Platform** (Plattform), **Processor** (Prozessor) und **Installed RAM** (Installierter RAM). Sie können die Spalten anpassen, indem Sie auf **Spalten-Editor** klicken. Die Geräteeigenschaften **Latitude** (Breitengrad) und **Longitude** (Längengrad) sind die Grundlage für den Bing Map-Standort im Dashboard.

![Spalten-Editor in der Geräteliste][img-device-list]

Im Lösungsportal können Sie im Bereich **Gerätedetails** die gewünschten Eigenschaften und Tags bearbeiten (gemeldete Eigenschaften sind schreibgeschützt).

![Bereich „Gerätedetails“][img-device-edit]

Sie können das Lösungsportal verwenden, um ein Gerät aus der Lösung zu entfernen. Wenn Sie ein Gerät entfernen, entfernt die Lösung den Geräteeintrag aus der Identitätsregistrierung und löscht dann den Gerätezwilling. Die Lösung entfernt auch Informationen im Zusammenhang mit dem Gerät aus der Cosmos DB-Datenbank. Bevor Sie ein Gerät entfernen können, müssen Sie es deaktivieren.

![Entfernen des Geräts][img-device-remove]

## <a name="device-information-message-processing"></a>Verarbeitung der Geräteinformationsnachricht

Von einem Gerät gesendete Geräteinformationsnachrichten unterscheiden sich von Telemetrienachrichten. Geräteinformationsnachrichten enthalten die Befehle, auf die ein Gerät reagieren kann, und den vorhandenen Befehlsverlauf. IoT Hub selbst verfügt über keinerlei Informationen über die Metadaten, die in einer Geräteinformationsnachricht enthalten sind, und verarbeitet die Nachricht wie alle anderen D2C-Nachrichten (Device-to-Cloud, Gerät-zu-Cloud) auch. In der Lösung für die Remoteüberwachung liest ein [Azure Stream Analytics][lnk-stream-analytics]-Auftrag (ASA) die Nachrichten von IoT Hub. Der **DeviceInfo**-Stream Analytics-Auftrag filtert nach Nachrichten, die **"ObjectType": "DeviceInfo"** enthalten, und leitet sie an die **EventProcessorHost**-Hostinstanz weiter, die in einem Webauftrag ausgeführt wird. Die Logik in der **EventProcessorHost**-Instanz verwendet die Geräte-ID, um den Cosmos DB-Eintrag für das jeweilige Gerät zu ermitteln und den Eintrag zu aktualisieren.

> [!NOTE]
> Eine Geräteinformationsnachricht ist eine D2C-Standardnachricht. Die Lösung unterscheidet mithilfe von ASA-Abfragen zwischen Geräteinformationsnachrichten und Telemetrienachrichten.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie erfahren haben, wie Sie die vorkonfigurierten Lösungen anpassen, können Sie einige der anderen Features und Funktionen der vorkonfigurierter IoT Suite-Lösungen ausprobieren:

* [Übersicht über die vorkonfigurierte Lösung für vorhersagbaren Wartungsbedarf][lnk-predictive-overview]
* [Häufig gestellte Fragen zu IoT Suite][lnk-faq]
* [Sicherheit im Internet der Dinge von Anfang an][lnk-security-groundup]

<!-- Images and links -->
[img-device-list]: media/iot-suite-remote-monitoring-device-info/image1.png
[img-device-edit]: media/iot-suite-remote-monitoring-device-info/image2.png
[img-device-remove]: media/iot-suite-remote-monitoring-device-info/image3.png

[lnk-iot-hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-identity-registry]: ../iot-hub/iot-hub-devguide-identity-registry.md
[lnk-device-twin]: ../iot-hub/iot-hub-devguide-device-twins.md
[lnk-docdb]: https://azure.microsoft.com/documentation/services/documentdb/
[lnk-stream-analytics]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-dynamic-telemetry]: iot-suite-dynamic-telemetry.md

[lnk-predictive-overview]: iot-suite-predictive-overview.md
[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md

