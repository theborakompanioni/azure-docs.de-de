---
title: "Entwicklungsleitfaden – Aufträge | Microsoft-Dokumentation"
description: "Entwicklungsleitfaden für Azure IoT Hub: Planen der Ausführung von Aufträgen auf mehreren mit Ihrem Hub verbundenen Geräten"
services: iot-hub
documentationcenter: .net
author: juanjperez
manager: timlt
editor: 
ms.assetid: fe78458f-4f14-4358-ac83-4f7bd14ee8da
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/30/2016
ms.author: juanpere
translationtype: Human Translation
ms.sourcegitcommit: c18a1b16cb561edabd69f17ecebedf686732ac34
ms.openlocfilehash: d57e52d2b97d226b62a356798a9e0b5fcabd1a00


---
# <a name="schedule-jobs-on-multiple-devices"></a>Planen von Aufträgen auf mehreren Geräten
## <a name="overview"></a>Übersicht
Wie in den vorherigen Artikeln beschrieben, bietet Azure IoT Hub eine Reihe von Bausteinen ([Eigenschaften und Tags von Gerätezwillingen][lnk-twin-devguide] und [Direkte Methoden][lnk-dev-methods]).  IoT-Back-End-Anwendungen ermöglichen Geräteadministratoren und -bedienern in der Regel die Aktualisierung von und Interaktion mit IoT-Geräten in einem Massenvorgang und zu einem geplanten Zeitpunkt.  Aufträge kapseln die Ausführung von Updates von Gerätezwillingen und direkten Methoden für eine Gruppe von Geräten zu einem geplanten Zeitpunkt.  Beispiel: Ein Bediener nutzt eine Back-End-Anwendung, mit der ein Auftrag zum Neustarten einer Gruppe von Geräten in Gebäude 43 im 3. Stock nacheinander neu gestartet und nachverfolgt wird, der für die Betriebsabläufe im Gebäude nicht störend ist.

### <a name="when-to-use"></a>Einsatzgebiete
Erwägen Sie das Arbeiten mit Aufträgen, wenn ein Lösungs-Back-End den Fortschritt der folgenden Aktivitäten auf einer Gruppe von Geräten planen und nachverfolgen muss:

* Aktualisieren gewünschter Eigenschaften
* Aktualisieren von Tags
* Aufrufen direkter Methoden

## <a name="job-lifecycle"></a>Auftragslebenszyklus
Aufträge werden vom Lösungs-Back-End eingeleitet und vom IoT Hub verwaltet.  Sie können einen Auftrag über einen dienstseitigen URI (`{iot hub}/jobs/v2/{device id}/methods/<jobID>?api-version=2016-09-30-preview`) auslösen und den Fortschritt eines in der Ausführung befindlichen Auftrags über einen dienstseitigen URI (`{iot hub}/jobs/v2/<jobId>?api-version=2016-09-30-preview`) abfragen.  Nachdem ein Auftrag eingeleitet wurde, ermöglicht das Abfragen von Aufträgen der Back-End-Anwendung das Aktualisieren des Status in der Ausführung befindlicher Aufträge.

> [!NOTE]
> Wenn Sie einen Auftrag initiieren, dürfen Eigenschaftennamen und -werte nur druckbare alphanumerische US-ASCII-Zeichen mit Ausnahme der folgenden enthalten: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.
> 
> 

## <a name="reference-topics"></a>Referenzthemen:
Die folgenden Referenzthemen enthalten weitere Informationen zur Verwendung von Aufträgen.

## <a name="jobs-to-execute-direct-methods"></a>Aufträge zum Ausführen direkter Methoden
Es folgen die HTTP 1.1-Anforderungsdetails zum Ausführen einer [direkten Methode][lnk-dev-methods] auf einer Gruppe von Geräten mithilfe eines Auftrags:

    ```
    PUT /jobs/v2/<jobId>?api-version=2016-09-30-preview

    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>

    {
        jobId: '<jobId>',
        type: 'scheduleDirectRequest', 
        cloudToDeviceMethod: {
            methodName: '<methodName>',
            payload: <payload>,                 
            timeoutInSeconds: methodTimeoutInSeconds 
        },
        queryCondition: '<queryOrDevices>', // if the queryOrDevices parameter is a string
        deviceIds: '<queryOrDevices>',      // if the queryOrDevices parameter is an array
        startTime: <jobStartTime>,          // as an ISO-8601 date string
        maxExecutionTimeInSeconds: <maxExecutionTimeInSeconds>        
    }
    ```

## <a name="jobs-to-update-device-twin-properties"></a>Aufträge zum Aktualisieren der Eigenschaften von Gerätezwillingen
Es folgen die HTTP 1.1-Anforderungsdetails zum Aktualisieren von Eigenschaften von Gerätezwillingen mithilfe eines Auftrags:

    ```
    PUT /jobs/v2/<jobId>?api-version=2016-09-30-preview
    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>

    {
        jobId: '<jobId>',
        type: 'scheduleTwinUpdate', 
        updateTwin: <patch>                 // Valid JSON object
        queryCondition: '<queryOrDevices>', // if the queryOrDevices parameter is a string
        deviceIds: '<queryOrDevices>',      // if the queryOrDevices parameter is an array
        startTime: <jobStartTime>,          // as an ISO-8601 date string
        maxExecutionTimeInSeconds: <maxExecutionTimeInSeconds>        // format TBD
    }
    ```

## <a name="querying-for-progress-on-jobs"></a>Abfragen des Fortschritts von Aufträgen
Es folgen die HTTP 1.1-Anforderungsdetails für das [Abfragen von Aufträgen][lnk-query]:

    ```
    GET /jobs/v2/query?api-version=2016-09-30-preview[&jobType=<jobType>][&jobStatus=<jobStatus>][&pageSize=<pageSize>][&continuationToken=<continuationToken>]

    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>
    ```

Das „continuationToken“ wird aus der Antwort bereitgestellt.  

## <a name="jobs-properties"></a>Auftragseigenschaften
Es folgt eine Liste von Eigenschaften und entsprechenden Beschreibungen, die beim Abfragen von Aufträgen oder Auftragsergebnissen verwendet werden können.

| Eigenschaft | Beschreibung |
| --- | --- |
| **jobId** |Für den Auftrag von der Anwendung angegebene ID. |
| **startTime** |Von der Anwendung angegebene Startzeit (ISO-8601) für den Auftrag. |
| **endTime** |Von IoT Hub angegebenes Datum (ISO-8601) für den Abschluss des Auftrags. Gilt nur, nachdem der Auftrag den Zustand „Abgeschlossen“ erreicht hat. |
| **type** |Auftragstypen: |
| **scheduledUpdateTwin**: Ein Auftrag zum Aktualisieren einer Gruppe gewünschter Eigenschaften oder Tags. | |
| **scheduledDeviceMethod**: Ein Auftrag zum Aufrufen einer Gerätemethode für eine Gruppe von Gerätezwillingen. | |
| **status** |Aktueller Status des Auftrags. Mögliche Werte für den Status: |
| **pending**: Geplant und auf Auswahl durch den Auftragsdiensts wartend. | |
| **scheduled**: Für einen Zeitpunkt in der Zukunft geplant. | |
| **running**: Derzeit aktiver Auftrag. | |
| **cancelled**: Auftrag wurde abgebrochen. | |
| **failed**: Fehler beim Auftrag. | |
| **completed**: Auftrag wurde abgeschlossen. | |
| **deviceJobStatistics** |Statistiken zur Ausführung des Auftrags. |

In der Vorschauphase steht das Objekt „deviceJobStatistics“ erst zur Verfügung, nachdem der Auftrag abgeschlossen wurde.

| Eigenschaft | Beschreibung |
| --- | --- |
| **deviceJobStatistics.deviceCount** |Anzahl der Geräte im Auftrag. |
| **deviceJobStatistics.failedCount** |Anzahl der Geräte, auf denen der Auftrag fehlgeschlagen ist. |
| **deviceJobStatistics.succeededCount** |Anzahl der Geräte, auf denen der Auftrag erfolgreich war. |
| **deviceJobStatistics.runningCount** |Anzahl der Geräte, auf denen der Auftrag derzeit ausgeführt wird. |
| **deviceJobStatistics.pendingCount** |Anzahl der Geräte, auf denen die Ausführung des Auftrags aussteht. |

### <a name="additional-reference-material"></a>Weiteres Referenzmaterial
Weitere Referenzthemen im Entwicklerhandbuch:

* Unter [IoT Hub-Endpunkte][lnk-endpoints] werden die verschiedenen Endpunkte beschrieben, die jeder IoT-Hub für Laufzeit- und Verwaltungsvorgänge bereitstellt.
* Unter [Drosselung und Kontingente][lnk-quotas] werden die Kontingente für den IoT Hub-Dienst und das Drosselungsverhalten beschrieben, die bei Verwendung des Diensts zu erwarten sind.
* Unter [Azure IoT device and service SDKs][lnk-sdks] (Azure IoT SDKs für Geräte und Dienste) werden die verschiedenen Sprach-SDKs aufgelistet, die Sie bei der Entwicklung von Geräte- und Dienstanwendungen für die Interaktion mit IoT Hub verwenden können.
* Unter [Referenz – Abfragesprache für Zwillinge und Aufträge][lnk-query] wird die IoT Hub-Abfragesprache beschrieben, mit der Sie von IoT Hub Informationen über Gerätezwillinge und Aufträge abrufen können.
* [IoT Hub-MQTT-Unterstützung][lnk-devguide-mqtt] enthält weitere Informationen zur Unterstützung für das MQTT-Protokoll in IoT Hub.

## <a name="next-steps"></a>Nächste Schritte
Wenn Sie einige der in diesem Artikel beschriebenen Konzepte ausprobieren möchten, ist möglicherweise das folgende IoT Hub-Tutorial für Sie interessant:

* [Planen und Übertragen von Aufträgen][lnk-jobs-tutorial]

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-jobs-tutorial]: iot-hub-node-node-schedule-jobs.md
[lnk-c2d-methods]: iot-hub-node-node-direct-methods.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-get-started-twin]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-devguide]: iot-hub-devguide-device-twins.md



<!--HONumber=Nov16_HO5-->


