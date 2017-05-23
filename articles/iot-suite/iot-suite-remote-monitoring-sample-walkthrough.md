---
title: "Exemplarische Vorgehensweise zur vorkonfigurierten Lösung für Remoteüberwachung | Microsoft Docs"
description: "Eine Beschreibung der vorkonfigurierten Lösung für Remoteüberwachung von Azure IoT und deren Architektur."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 31fe13af-0482-47be-b4c8-e98e36625855
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/15/2017
ms.author: dobett
ms.translationtype: Human Translation
ms.sourcegitcommit: 9568210d4df6cfcf5b89ba8154a11ad9322fa9cc
ms.openlocfilehash: c6d76cc741a6d932a506017781e45bc9b8f8c640
ms.contentlocale: de-de
ms.lasthandoff: 05/15/2017


---
# <a name="remote-monitoring-preconfigured-solution-walkthrough"></a>Exemplarische Vorgehensweise zur vorkonfigurierten Lösung für Remoteüberwachung

Die [vorkonfigurierte Lösung][lnk-preconfigured-solutions] für die IoT Suite-Remoteüberwachung ist eine Implementierung einer End-to-End-Überwachungslösung für mehrere Computer, die an Remotestandorten ausgeführt werden. In der Lösung sind wichtige Azure-Dienste kombiniert, um eine generische Implementierung des Geschäftsszenarios zu erzielen. Sie können sie als Ausgangspunkt für Ihre Implementierung verwenden und [anpassen][lnk-customize], um Ihre eigenen speziellen Geschäftsanforderungen zu erfüllen.

In diesem Artikel werden einige wichtige Elemente der Lösung für die Remoteüberwachung beschrieben, um die Funktionsweise zu verdeutlichen. Dieses Wissen ist für folgende Zwecke hilfreich:

* Behandeln von Problemen in der Lösung
* Planen der Lösungsanpassung zur Erfüllung besonderer Anforderungen 
* Entwerfen einer eigenen IoT-Lösung mit Verwendung von Azure-Diensten

## <a name="logical-architecture"></a>Logische Architektur

Das folgende Diagramm beschreibt die logischen Komponenten der vorkonfigurierten Lösung:

![Logische Architektur](media/iot-suite-remote-monitoring-sample-walkthrough/remote-monitoring-architecture.png)

## <a name="simulated-devices"></a>Simulierte Geräte

In der vorkonfigurierten Lösung ist das simulierte Gerät ein Kühlgerät (z. B. die Klimaanlage eines Gebäudes oder die Lüftungsanlage einer Anlage). Wenn Sie die vorkonfigurierte Lösung bereitstellen, werden automatisch auch vier simulierte Geräte bereitgestellt, die in einem [Azure WebJob][lnk-webjobs] ausgeführt werden. Die simulierten Geräte erleichtern Ihnen das Untersuchen des Verhaltens einer Lösung, ohne dass Sie physische Geräte bereitstellen müssen. Informationen zum Bereitstellen eines echten physischen Geräts finden Sie im Tutorial [Verbinden Ihres Geräts mit der vorkonfigurierten Remoteüberwachungslösung][lnk-connect-rm].

### <a name="device-to-cloud-messages"></a>D2C-Nachrichten

Jedes simulierte Gerät kann die folgenden Nachrichtentypen an den IoT Hub senden:

| Nachricht | Beschreibung |
| --- | --- |
| Starten |Wenn das Gerät gestartet wird, sendet es eine Nachricht vom Typ **Geräteinformationen** (device-info) mit Daten über sich selbst an das Back-End. Diese Daten enthalten die Geräte-ID und eine Liste der vom Gerät unterstützten Befehle und Methoden. |
| Anwesenheit |Ein Gerät sendet regelmäßig eine **Anwesenheitsnachricht** , um zu melden, ob das Gerät das Vorhandensein („Anwesenheit“) eines Sensors erkennen kann. |
| Telemetrie |Ein Gerät sendet in regelmäßigen Abständen eine **Telemetrienachricht** , mit der simulierte Werte für die Temperatur und Luftfeuchtigkeit gemeldet werden, die über die simulierten Sensoren des Geräts erfasst werden. |

> [!NOTE]
> Die Lösung speichert die Liste der vom Gerät unterstützten Befehle in einer Cosmos DB-Datenbank und nicht im Gerätezwilling.

### <a name="properties-and-device-twins"></a>Eigenschaften und Gerätezwillinge

Die simulierten Geräte senden die Geräteeigenschaften an den [Zwilling][lnk-device-twins] im IoT Hub als *gemeldete Eigenschaften*. Das Gerät sendet gemeldete Eigenschaften beim Start und als Antwort auf einen Befehl oder eine Methode zum **Ändern des Gerätestatus**.

| Eigenschaft | Zweck |
| --- | --- |
| Config.TelemetryInterval | Frequenz (Sekunden), in der das Gerät Telemetriedaten sendet |
| Config.TemperatureMeanValue | Gibt den Mittelwert der simulierten Telemetriedaten für die Temperatur an |
| Device.DeviceID |ID, die bereitgestellt oder zugewiesen wird, wenn ein Gerät in der Lösung erstellt wird |
| Device.DeviceState | Der vom Gerät gemeldete Zustand |
| Device.CreatedTime |Uhrzeit der Erstellung des Geräts in der Lösung |
| Device.StartupTime |Uhrzeit, zu das Gerät gestartet wurde |
| Device.LastDesiredPropertyChange |Versionsnummer der letzten gewünschten Eigenschaftsänderung |
| Device.Location.Latitude |Breitengrad des Standorts des Geräts |
| Device.Location.Longitude |Längengrad des Standorts des Geräts |
| System.Manufacturer |Gerätehersteller |
| System.ModelNumber |Modellnummer des Geräts |
| System.SerialNumber |Seriennummer des Geräts |
| System.FirmwareVersion |Aktuelle Version der Firmware auf dem Gerät |
| System.Platform |Plattformarchitektur des Geräts |
| System.Processor |Prozessor des Geräts |
| System.InstalledRAM |Größe des auf dem Gerät installierten RAM |

Der Simulator füllt diese Eigenschaften in simulierten Geräten mit Beispielwerten. Jedes Mal, wenn der Simulator ein simuliertes Gerät initialisiert, sendet das Gerät vordefinierte Metadaten als gemeldete Eigenschaften an IoT Hub. Gemeldete Eigenschaften können nur vom Gerät aktualisiert werden. Um eine gemeldete Eigenschaft zu ändern, legen Sie eine gewünschte Eigenschaft im Lösungsportal fest. Das Gerät hat folgende Aufgaben:

1. Regelmäßiges Abrufen der gewünschten Eigenschaften aus dem IoT Hub
2. Aktualisieren der Konfiguration mit dem gewünschten Eigenschaftswert
3. Zurücksenden des neuen Werts an den Hub als gemeldete Eigenschaft

Über das Lösungsdashboard können Sie *gewünschte Eigenschaften* verwenden, um mit dem [Gerätezwilling][lnk-device-twins] Eigenschaften auf einem Gerät festzulegen. In der Regel liest ein Gerät einen gewünschten Eigenschaftswert vom Hub, um seinen internen Status zu aktualisieren und die Änderung wieder als gemeldete Eigenschaft zu melden.

> [!NOTE]
> Das simulierte Gerätecode verwendet nur die gewünschten Eigenschaften **Desired.Config.TemperatureMeanValue** und **Desired.Config.TelemetryInterval** zum Aktualisieren der gemeldeten Eigenschaften, die zurück an den IoT Hub gesendet werden. Alle anderen gewünschten Eigenschaftsänderungen werden im simulierten Gerät ignoriert.

### <a name="methods"></a>Methoden

Die simulierten Geräte können die folgenden Methoden ([direkte Methoden][lnk-direct-methods]) verarbeiten, die vom Lösungsportal über den IoT Hub gesendet werden:

| Methode | Beschreibung |
| --- | --- |
| InitiateFirmwareUpdate |Weist das Gerät an, ein Firmwareupdate auszuführen |
| Neustart |Weist das Gerät an, neu zu starten |
| FactoryReset |Weist das Gerät an, ein Zurücksetzen auf Werkseinstellungen vorzunehmen |

Manche Methoden verwenden gemeldete Eigenschaften zum Berichten des Fortschritts. Die **InitiateFirmwareUpdate**-Methode simuliert beispielsweise das asynchrone Ausführen des Updates auf dem Gerät. Die Methode wird sofort auf dem Gerät zurückgegeben, während die asynchrone Aufgabe weiterhin über gemeldete Eigenschaften Statusaktualisierungen an das Lösungsdashboard zurücksendet.

### <a name="commands"></a>Befehle

Außerdem können die simulierten Geräte auch die folgenden Befehle (C2D-Nachrichten) verarbeiten, die vom Lösungsportal über den IoT Hub gesendet werden:

| Befehl | Beschreibung |
| --- | --- |
| PingDevice |Sendet einen *Ping* an das Gerät, um zu überprüfen, ob es aktiv ist. |
| StartTelemetry |Startet das Senden von Telemetriedaten auf dem Gerät |
| StopTelemetry |Beendet das Senden von Telemetriedaten auf dem Gerät |
| ChangeSetPointTemp |Ändert den festgelegten Punktwert, um den die zufälligen Daten generiert werden |
| DiagnosticTelemetry |Löst aus, dass der Gerätesimulator einen zusätzlichen Telemetriewert (externalTemp) sendet |
| ChangeDeviceState |Ändert eine erweiterte Zustandseigenschaft für das Gerät und sendet die Meldung mit Geräteinformationen vom Gerät |

> [!NOTE]
> Einen Vergleich dieser Befehle (C2D-Nachrichten) und Methoden (direkte Methoden) finden Sie im [Leitfaden zur C2D-Kommunikation][lnk-c2d-guidance].

## <a name="iot-hub"></a>IoT Hub

Der [IoT Hub][lnk-iothub] erfasst Daten, die von den Geräten in die Cloud gesendet werden, und stellt sie für die Azure Stream Analytics-Aufträge (ASA) zur Verfügung. Für jeden ASA-Datenstromauftrag wird eine separate IoT Hub-Consumergruppe verwendet, um den Nachrichtendatenstrom von Ihren Geräten zu lesen.

Zu den Aufgaben des IoT Hub in der Lösung zählen auch:

- Verwalten einer Identitätsregistrierung, die die IDs und Authentifizierungsschlüssel aller Geräte speichert, die Verbindungen zum Portal herstellen dürfen. Sie können Geräte über die Identitätsregistrierung aktivieren und deaktivieren.
- Senden von Befehlen an Ihre Geräte im Namen des Geräteportals
- Aufrufen von Methoden auf Ihren Geräten im Namen des Geräteportals
- Verwalte von Gerätezwillingen für alle registrierten Geräte. Ein Gerätezwilling speichert die von einem Gerät gemeldeten Eigenschaftswerte. Ein Gerätezwilling speichert außerdem die im Lösungsportal gespeicherten gewünschten Eigenschaften, damit das Gerät diese beim nächsten Verbinden abrufen kann.
- Planen von Aufträgen, um Eigenschaften für mehrere Geräte festzulegen oder Methoden auf mehreren Geräte aufzurufen.

## <a name="azure-stream-analytics"></a>Azure Stream Analytics

In der Lösung für die Remoteüberwachung übermittelt [Azure Stream Analytics][lnk-asa] (ASA) Gerätenachrichten, die über den IoT Hub eingehen, zur Verarbeitung oder Speicherung an andere Back-End-Komponenten. Unterschiedliche ASA-Aufträge führen bestimmte Funktionen basierend auf dem Inhalt der Nachrichten durch.

**Auftrag 1: Geräteinformationen** filtert Meldungen mit Geräteinformationen aus dem eingehenden Meldungsdatenstrom und sendet diese an einen Event Hub-Endpunkt. Ein Gerät sendet Meldungen mit Geräteinformationen beim Start und als Antwort auf den Befehl **SendDeviceInfo** . Bei diesem Auftrag wird die folgende Abfragedefinition verwendet, um Nachrichten vom Typ **Geräteinformationen** zu identifizieren:

```
SELECT * FROM DeviceDataStream Partition By PartitionId WHERE  ObjectType = 'DeviceInfo'
```

Dieser Auftrag sendet seine Ausgabe zur weiteren Verarbeitung an einen Event Hub.

**Auftrag 2: Regeln** wertet eingehende Telemetriewerte zu Temperatur und Feuchtigkeit anhand von Schwellenwerten pro Gerät aus. Schwellenwerte werden im Regel-Editor festgelegt, der im Lösungsportal verfügbar ist. Jedes Gerät-Wert-Paar wird nach dem Zeitstempel in einem Blob gespeichert, das von Stream Analytics als **Verweisdaten** eingelesen wird. Der Auftrag vergleicht alle nicht leeren Werte mit dem für das Gerät festgelegten Schwellenwert. Wenn er die Bedingung „>“ überschreitet, gibt der Auftrag ein **Alarm**-Ereignis aus. Damit wird angezeigt, dass der Schwellenwert überschritten wurde. Zudem werden das Gerät, der Wert und Zeitstempelwerte bereitgestellt. Für diesen Auftrag wird die folgende Abfragedefinition verwendet, um Telemetrienachrichten zu identifizieren, die einen Alarm auslösen:

```
WITH AlarmsData AS 
(
SELECT
     Stream.IoTHub.ConnectionDeviceId AS DeviceId,
     'Temperature' as ReadingType,
     Stream.Temperature as Reading,
     Ref.Temperature as Threshold,
     Ref.TemperatureRuleOutput as RuleOutput,
     Stream.EventEnqueuedUtcTime AS [Time]
FROM IoTTelemetryStream Stream
JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
WHERE
     Ref.Temperature IS NOT null AND Stream.Temperature > Ref.Temperature

UNION ALL

SELECT
     Stream.IoTHub.ConnectionDeviceId AS DeviceId,
     'Humidity' as ReadingType,
     Stream.Humidity as Reading,
     Ref.Humidity as Threshold,
     Ref.HumidityRuleOutput as RuleOutput,
     Stream.EventEnqueuedUtcTime AS [Time]
FROM IoTTelemetryStream Stream
JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
WHERE
     Ref.Humidity IS NOT null AND Stream.Humidity > Ref.Humidity
)

SELECT *
INTO DeviceRulesMonitoring
FROM AlarmsData

SELECT *
INTO DeviceRulesHub
FROM AlarmsData
```

Der Auftrag sendet seine Ausgabe zur weiteren Verarbeitung an einen Event Hub und speichert die Details der einzelnen Warnungen im Blobspeicher, wo über das Lösungsportal die Warnungsinformationen gelesen werden können.

**Auftrag 3: Telemetrie** verarbeitet den eingehenden Datenstrom mit Gerätetelemetriedaten auf zwei Arten. Bei der ersten Methode werden alle Telemetrienachrichten von den Geräten zur langfristigen Speicherung an den permanenten Blobspeicher gesendet. Bei der zweiten Methode werden durchschnittliche, minimale und maximale Luftfeuchtigkeitswerte für ein gleitendes Fenster von fünf Minuten berechnet und die Daten an den Blobspeicher gesendet. Über das Lösungsportal werden die Telemetriedaten aus dem Blobspeicher gelesen, um die Diagramme aufzufüllen. Dieser Auftrag verwendet die folgende Abfragedefinition:

```
WITH 
    [StreamData]
AS (
    SELECT
        *
    FROM [IoTHubStream]
    WHERE
        [ObjectType] IS NULL -- Filter out device info and command responses
) 

SELECT
    IoTHub.ConnectionDeviceId AS DeviceId,
    Temperature,
    Humidity,
    ExternalTemperature,
    EventProcessedUtcTime,
    PartitionId,
    EventEnqueuedUtcTime,
    * 
INTO
    [Telemetry]
FROM
    [StreamData]

SELECT
    IoTHub.ConnectionDeviceId AS DeviceId,
    AVG (Humidity) AS [AverageHumidity],
    MIN(Humidity) AS [MinimumHumidity],
    MAX(Humidity) AS [MaxHumidity],
    5.0 AS TimeframeMinutes 
INTO
    [TelemetrySummary]
FROM [StreamData]
WHERE
    [Humidity] IS NOT NULL
GROUP BY
    IoTHub.ConnectionDeviceId,
    SlidingWindow (mi, 5)
```

## <a name="event-hubs"></a>Event Hubs

Die Daten der ASA-Aufträge vom Typ **Geräteinformationen** und **Regeln** werden an Event Hubs ausgegeben, um die zuverlässige Weiterleitung zum **Ereignisprozessor** sicherzustellen, der im WebJob ausgeführt wird.

## <a name="azure-storage"></a>Azure-Speicher

Die Lösung nutzt Azure-Blobspeicher, um alle Rohdaten und zusammengefassten Telemetriedaten von den Geräten dauerhaft in der Lösung zu speichern. Über das Portal werden die Telemetriedaten aus dem Blobspeicher gelesen, um die Diagramme aufzufüllen. Zum Anzeigen von Warnungen liest das Lösungsportal die Daten aus dem Blobspeicher aus, in dem aufgezeichnet wird, wenn die Telemetriewerte die konfigurierten Schwellenwerte überschreiten. In der Lösung wird Blobspeicher auch eingesetzt, um die Schwellenwerte aufzuzeichnen, die Sie im Lösungsportal festlegen.

## <a name="webjobs"></a>WebJobs

Zusätzlich zum Hosten der Gerätesimulatoren hosten die WebJobs in der Lösung auch den **Ereignisprozessor** in einem Azure WebJob, mit dem Befehlsantworten verarbeitet werden. Meldungen mit Antworten auf Befehle werden verwendet, um den Befehlsverlauf des Geräts zu aktualisieren (gespeichert in der Cosmos DB-Datenbank).

## <a name="cosmos-db"></a>Cosmos DB

Die Lösung verwendet eine Cosmos DB-Datenbank zum Speichern von Informationen zu den Geräten, die mit der Lösung verbunden sind. Diese Informationen umfassen den Verlauf von Befehlen, die vom Lösungsportal an die Geräte gesendet werden, und von Methoden, die aus dem Lösungsportal aufgerufen werden.

## <a name="solution-portal"></a>Lösungsportal

Das Lösungsportal ist eine Web-App, die als Teil der vorkonfigurierten Lösung bereitgestellt wird. Die wichtigsten Seiten im Lösungsportal sind das Dashboard und die Geräteliste.

### <a name="dashboard"></a>Dashboard

Diese Seite in der Web-App verwendet PowerBI-JavaScript-Steuerelemente (siehe [PowerBI-visuals repo](https://www.github.com/Microsoft/PowerBI-visuals)(Repository mit PowerBI-Visualisierungen)), um die Telemetriedaten der Geräte zu visualisieren. Die Lösung nutzt den ASA-Telemetrieauftrag, um die Telemetriedaten in den Blobspeicher zu schreiben.

### <a name="device-list"></a>Geräteliste

Auf dieser Seite des Lösungsportals können Sie folgende Aktionen ausführen:

* Bereitstellen eines neuen Geräts. Diese Aktion legt die eindeutige Geräte-ID fest und generiert den Authentifizierungsschlüssel. Sie schreibt Informationen zum Gerät sowohl in die IoT Hub-Identitätsregistrierung als auch in die Cosmos DB-Datenbank der Lösung.
* Verwalten von Geräteeigenschaften. Diese Aktion umfasst das Anzeigen von vorhandenen Eigenschaften und das Aktualisieren mit neuen Eigenschaften.
* Senden von Befehlen an ein Gerät.
* Anzeigen des Befehlsverlaufs für ein Gerät.
* Aktivieren und Deaktivieren von Geräten.

## <a name="next-steps"></a>Nächste Schritte

In den folgenden TechNet-Blogbeiträgen finden Sie weitere Informationen zur vorkonfigurierten Lösung für die Remoteüberwachung:

* [IoT Suite - Under The Hood - Remote Monitoring (IoT Suite – Weitere Informationen – Remoteüberwachung)](http://social.technet.microsoft.com/wiki/contents/articles/32941.iot-suite-under-the-hood-remote-monitoring.aspx)
* [IoT Suite - Remote Monitoring - Adding Live and Simulated Devices (IoT Suite – Remoteüberwachung – Hinzufügen von simulierten und Live-Geräten)](http://social.technet.microsoft.com/wiki/contents/articles/32975.iot-suite-remote-monitoring-adding-live-and-simulated-devices.aspx)

Sie können mit den ersten Schritten mit IoT Suite fortfahren. Lesen Sie dazu die folgenden Artikel:

* [Verbinden Ihres Geräts mit der vorkonfigurierten Remoteüberwachungslösung (Windows)][lnk-connect-rm]
* [Berechtigungen für die Website „azureiotsuite.com“][lnk-permissions]

[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-iothub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-webjobs]: https://azure.microsoft.com/documentation/articles/websites-webjobs-resources/
[lnk-connect-rm]: iot-suite-connecting-devices.md
[lnk-permissions]: iot-suite-permissions.md
[lnk-c2d-guidance]: ../iot-hub/iot-hub-devguide-c2d-guidance.md
[lnk-device-twins]:  ../iot-hub/iot-hub-devguide-device-twins.md
[lnk-direct-methods]: ../iot-hub/iot-hub-devguide-direct-methods.md

