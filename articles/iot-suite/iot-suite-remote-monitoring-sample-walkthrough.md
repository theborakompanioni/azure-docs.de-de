<properties
 pageTitle="Exemplarische Vorgehensweise zur vorkonfigurierten Lösung für Remoteüberwachung | Microsoft Azure"
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
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/17/2016"
 ms.author="dobett"/>

# Exemplarische Vorgehensweise zur vorkonfigurierten Lösung für Remoteüberwachung

## Einführung

Die [vorkonfigurierte Lösung][lnk-preconfigured-solutions] für die IoT Suite-Remoteüberwachung ist eine Implementierung einer End-to-End-Überwachungslösung für mehrere Computer, die an Remotestandorten ausgeführt werden. In der Lösung sind wichtige Azure-Dienste kombiniert, um eine generische Implementierung des Geschäftsszenarios zu erzielen. Sie können sie als Ausgangspunkt für Ihre Implementierung verwenden. Sie können die Lösung [anpassen][lnk-customize], um Ihre eigenen speziellen Geschäftsanforderungen zu erfüllen.

In diesem Artikel werden einige wichtige Elemente der Lösung für die Remoteüberwachung beschrieben, um die Funktionsweise zu verdeutlichen. Dieses Wissen ist für folgende Zwecke hilfreich:

- Behandeln von Problemen in der Lösung
- Planen der Lösungsanpassung zur Erfüllung besonderer Anforderungen
- Entwerfen einer eigenen IoT-Lösung mit Verwendung von Azure-Diensten

## Logische Architektur

Das folgende Diagramm beschreibt die logischen Komponenten der vorkonfigurierten Lösung:

![Logische Architektur](media/iot-suite-remote-monitoring-sample-walkthrough/remote-monitoring-architecture.png)


## Simulierte Geräte

In der vorkonfigurierten Lösung ist das simulierte Gerät ein Kühlgerät (z. B. die Klimaanlage eines Gebäudes oder die Lüftungsanlage einer Anlage). Wenn Sie die vorkonfigurierte Lösung bereitstellen, werden automatisch auch vier simulierte Geräte bereitgestellt, die in einem [Azure WebJob][lnk-webjobs] ausgeführt werden. Die simulierten Geräte erleichtern Ihnen das Untersuchen des Verhaltens einer Lösung, ohne dass Sie physische Geräte bereitstellen müssen. Informationen zum Bereitstellen eines echten physischen Geräts finden Sie im Tutorial [Verbinden Ihres Geräts mit der vorkonfigurierten Remoteüberwachungslösung][lnk-connect-rm].

Jedes simulierte Gerät kann die folgenden Nachrichtentypen an den IoT Hub senden:

| Nachricht | Beschreibung |
|----------|-------------|
| Starten | Wenn das Gerät gestartet wird, sendet es eine Nachricht vom Typ **Geräteinformationen** (device-info) mit Daten über sich selbst an das Back-End. Hierzu gehören die Geräte-ID, Gerätemetadaten, eine Liste mit Befehlen, die das Gerät unterstützt, und die aktuelle Konfiguration des Geräts. |
| Anwesenheit | Ein Gerät sendet regelmäßig eine **Anwesenheitsnachricht**, um zu melden, ob das Gerät das Vorhandensein („Anwesenheit“) eines Sensors erkennen kann. |
| Telemetrie | Ein Gerät sendet in regelmäßigen Abständen eine **Telemetrienachricht**, mit der simulierte Werte für die Temperatur und Luftfeuchtigkeit gemeldet werden, die über die simulierten Sensoren des Geräts erfasst werden. |


Die simulierten Geräte senden die folgenden Geräteeigenschaften als Nachricht vom Typ **Geräteinformationen**:

| Eigenschaft | Zweck |
|------------------------|--------- |
| Geräte-ID | Die ID, die entweder bereitgestellt oder zugewiesen wird, wenn ein Gerät in der Lösung erstellt wird. |
| Hersteller | Gerätehersteller |
| Modellnummer | Modellnummer des Geräts |
| Seriennummer | Seriennummer des Geräts |
| Firmware | Aktuelle Version der Firmware auf dem Gerät |
| Plattform | Plattformarchitektur des Geräts |
| Prozessor | Prozessor des Geräts |
| Installierter RAM | Größe des auf dem Gerät installierten RAM |
| Für Hub aktivierter Zustand | IoT Hub-Zustandseigenschaft des Geräts |
| Erstellungszeit | Uhrzeit der Erstellung des Geräts in der Lösung |
| Aktualisierungszeit | Letzter Zeitpunkt, zu dem die Eigenschaften für das Gerät aktualisiert wurden |
| Breitengrad | Breitengrad des Standorts des Geräts |
| Längengrad | Längengrad des Standorts des Geräts |

Der Simulator füllt diese Eigenschaften in simulierten Geräten mit Beispielwerten. Jedes Mal, wenn der Simulator ein simuliertes Gerät initialisiert, sendet das Gerät vordefinierte Metadaten an IoT Hub. Beachten Sie, wie hierbei alle Aktualisierungen von Metadaten, die im Geräteportal vorgenommen werden, überschrieben werden.


Außerdem können die simulierten Geräte auch die folgenden Befehle verarbeiten, die vom Lösungsdashboard über den IoT Hub gesendet werden:

| Befehl | Beschreibung |
|------------------------|-----------------------------------------------------|
| PingDevice | Sendet einen _Ping_ an das Gerät, um zu überprüfen, ob es aktiv ist. |
| StartTelemetry | Startet das Senden von Telemetriedaten auf dem Gerät |
| StopTelemetry | Beendet das Senden von Telemetriedaten auf dem Gerät |
| ChangeSetPointTemp | Ändert den festgelegten Punktwert, um den die zufälligen Daten generiert werden |
| DiagnosticTelemetry | Löst aus, dass der Gerätesimulator einen zusätzlichen Telemetriewert (externalTemp) sendet |
| ChangeDeviceState | Ändert eine erweiterte Zustandseigenschaft für das Gerät und sendet die Meldung mit Geräteinformationen vom Gerät |

Die Bestätigung des Gerätebefehls für das Back-End der Lösung wird über den IoT Hub bereitgestellt.

## IoT Hub

Der [IoT Hub][lnk-iothub] erfasst Daten, die von den Geräten in die Cloud gesendet werden, und stellt sie für die Azure Stream Analytics-Aufträge (ASA) zur Verfügung. Der IoT Hub sendet im Namen des Geräteportals auch Befehle an Ihre Geräte. Für jeden ASA-Datenstromauftrag wird eine separate IoT Hub-Consumergruppe verwendet, um den Nachrichtendatenstrom von Ihren Geräten zu lesen.

## Azure Stream Analytics

In der Lösung für die Remoteüberwachung übermittelt [Azure Stream Analytics][lnk-asa] \(ASA) Gerätenachrichten, die über den IoT Hub eingehen, zur Verarbeitung oder Speicherung an andere Back-End-Komponenten. Unterschiedliche ASA-Aufträge führen bestimmte Funktionen basierend auf dem Inhalt der Nachrichten durch.

**Auftrag 1: Geräteinformationen** filtert Meldungen mit Geräteinformationen aus dem eingehenden Meldungsdatenstrom und sendet diese an einen Event Hub-Endpunkt. Ein Gerät sendet Meldungen mit Geräteinformationen beim Start und als Antwort auf den Befehl **SendDeviceInfo**. Bei diesem Auftrag wird die folgende Abfragedefinition verwendet, um Nachrichten vom Typ **Geräteinformationen** zu identifizieren:

```
SELECT * FROM DeviceDataStream Partition By PartitionId WHERE  ObjectType = 'DeviceInfo'
```

Dieser Auftrag sendet seine Ausgabe zur weiteren Verarbeitung an einen Event Hub.

**Auftrag 2: Regeln** wertet eingehende Telemetriewerte zu Temperatur und Feuchtigkeit anhand von Schwellenwerten pro Gerät aus. Schwellenwerte werden im Regel-Editor festgelegt, der im Lösungsdashboard verfügbar ist. Jedes Gerät-Wert-Paar wird nach dem Zeitstempel in einem Blob gespeichert, das von Stream Analytics als **Verweisdaten** eingelesen wird. Der Auftrag vergleicht alle nicht leeren Werte mit dem für das Gerät festgelegten Schwellenwert. Wenn er die Bedingung „>“ überschreitet, gibt der Auftrag ein **Alarm**-Ereignis aus. Damit wird angezeigt, dass der Schwellenwert überschritten wurde. Zudem werden das Gerät, der Wert und Zeitstempelwerte bereitgestellt. Für diesen Auftrag wird die folgende Abfragedefinition verwendet, um Telemetrienachrichten zu identifizieren, die einen Alarm auslösen:

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

Der Auftrag sendet seine Ausgabe zur weiteren Verarbeitung an einen Event Hub und speichert die Details der einzelnen Warnungen im Blobspeicher, wo über das Lösungsdashboard die Warnungsinformationen gelesen werden können.

**Auftrag 3: Telemetrie** verarbeitet den eingehenden Datenstrom mit Gerätetelemetriedaten auf zwei Arten. Bei der ersten Methode werden alle Telemetrienachrichten von den Geräten zur langfristigen Speicherung an den permanenten Blobspeicher gesendet. Bei der zweiten Methode werden durchschnittliche, minimale und maximale Luftfeuchtigkeitswerte für ein gleitendes Fenster von fünf Minuten berechnet und die Daten an den Blobspeicher gesendet. Über das Lösungsdashboard werden die Telemetriedaten aus dem Blobspeicher gelesen, um die Diagramme aufzufüllen. Dieser Auftrag verwendet die folgende Abfragedefinition:

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

## Event Hubs

Die Daten der ASA-Aufträge vom Typ **Geräteinformationen** und **Regeln** werden an Event Hubs ausgegeben, um die zuverlässige Weiterleitung zum **Ereignisprozessor** sicherzustellen, der im WebJob ausgeführt wird.

## Azure-Speicher

Die Lösung nutzt Azure-Blobspeicher, um alle Rohdaten und zusammengefassten Telemetriedaten von den Geräten dauerhaft in der Lösung zu speichern. Über das Dashboard werden die Telemetriedaten aus dem Blobspeicher gelesen, um die Diagramme aufzufüllen. Zum Anzeigen von Warnungen liest das Dashboard die Daten aus dem Blobspeicher aus, in dem aufgezeichnet wird, wenn die Telemetriewerte die konfigurierten Schwellenwerte überschreiten. In der Lösung wird Blobspeicher auch eingesetzt, um die Schwellenwerte aufzuzeichnen, die Sie im Dashboard festlegen.

## WebJobs

Zusätzlich zum Hosten der Gerätesimulatoren hosten die WebJobs in der Lösung auch den **Ereignisprozessor** in einem Azure WebJob, mit dem die Geräteinformationsnachrichten und Befehlsantworten verarbeitet werden. Er verwendet Folgendes:

- Meldungen mit Geräteinformationen, die die Registrierung des Geräts (gespeichert in der DocumentDB-Datenbank) mit den aktuellen Geräteinformationen aktualisieren.
- Meldungen mit Antworten auf Befehle, um den Befehlsverlauf des Geräts zu aktualisieren (gespeichert in der DocumentDB-Datenbank).

## DocumentDB

Die Lösung verwendet eine DocumentDB-Datenbank zum Speichern von Informationen zu den Geräten, die mit der Lösung verbunden sind. Dies sind beispielsweise Gerätemetadaten und der Verlauf der Befehle, die vom Dashboard an die Geräte gesendet werden.

## Web-Apps

### Dashboard für die Remoteüberwachung
Diese Seite in der Webanwendung verwendet PowerBI-JavaScript-Steuerelemente (siehe [PowerBI-visuals repo](https://www.github.com/Microsoft/PowerBI-visuals) (Repository mit PowerBI-Visualisierungen)), um die Telemetriedaten der Geräte zu visualisieren. Die Lösung nutzt den ASA-Telemetrieauftrag, um die Telemetriedaten in den Blobspeicher zu schreiben.


### Geräteverwaltungsportal

Diese Web-App ermöglicht Folgendes:

- Bereitstellen eines neuen Geräts. Diese Aktion legt die eindeutige Geräte-ID fest und generiert den Authentifizierungsschlüssel. Sie schreibt Informationen zum Gerät sowohl in die IoT Hub-Identitätsregistrierung als auch in die DocumentDB-Datenbank der Lösung.
- Verwalten von Geräteeigenschaften. Diese Aktion umfasst das Anzeigen von vorhandenen Eigenschaften und das Aktualisieren mit neuen Eigenschaften.
- Senden von Befehlen an ein Gerät.
- Anzeigen des Befehlsverlaufs für ein Gerät.
- Aktivieren und Deaktivieren von Geräten.

## Nächste Schritte

In den folgenden TechNet-Blogbeiträgen finden Sie weitere Informationen zur vorkonfigurierten Lösung für die Remoteüberwachung:

- [IoT Suite - Under The Hood - Remote Monitoring (IoT Suite – Weitere Informationen – Remoteüberwachung)](http://social.technet.microsoft.com/wiki/contents/articles/32941.iot-suite-under-the-hood-remote-monitoring.aspx)
- [IoT Suite - Remote Monitoring - Adding Live and Simulated Devices (IoT Suite – Remoteüberwachung – Hinzufügen von simulierten und Live-Geräten)](http://social.technet.microsoft.com/wiki/contents/articles/32975.iot-suite-remote-monitoring-adding-live-and-simulated-devices.aspx)

Sie können mit den ersten Schritten mit IoT Suite fortfahren. Lesen Sie dazu die folgenden Artikel:

- [Verbinden Ihres Geräts mit der vorkonfigurierten Remoteüberwachungslösung (Windows)][lnk-connect-rm]
- [Berechtigungen für die Website „azureiotsuite.com“][lnk-permissions]

[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-iothub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-webjobs]: https://azure.microsoft.com/documentation/articles/websites-webjobs-resources/
[lnk-connect-rm]: iot-suite-connecting-devices.md
[lnk-permissions]: iot-suite-permissions.md

<!---HONumber=AcomDC_0817_2016-->