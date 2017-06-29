---
title: Senden von Ereignissen an die Azure Time Series Insights-Umgebung | Microsoft-Dokumentation
description: "In diesem Tutorial erfahren Sie, wie Sie Ereignisse mithilfe von Push an die Time Series Insights-Umgebung übertragen."
keywords: 
services: time-series-insights
documentationcenter: 
author: venkatgct
manager: almineev
editor: cgronlun
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/21/2017
ms.author: venkatja
ms.translationtype: Human Translation
ms.sourcegitcommit: ef1e603ea7759af76db595d95171cdbe1c995598
ms.openlocfilehash: d7c01e18355b66670c9ab7d964f5cdb7ba72bb8f
ms.contentlocale: de-de
ms.lasthandoff: 06/16/2017

---
# <a name="send-events-to-a-time-series-insights-environment-via-event-hub"></a>Senden von Ereignissen an die Azure Time Series Insights-Umgebung per Event Hub

In diesem Tutorial wird erläutert, wie Sie Event Hub erstellen und konfigurieren. Außerdem erfahren Sie, wie Sie eine Beispielanwendung ausführen, um Ereignisse mithilfe von Push zu übertragen. Wenn Sie bereit über einen Event Hub mit Ereignissen im JSON-Format verfügen, können Sie dieses Tutorial überspringen und sich Ihre Umgebung im [Time Series-Explorer](https://insights.timeseries.azure.com) ansehen.

## <a name="configure-an-event-hub"></a>Konfigurieren eines Event Hubs
1. Führen Sie zum Erstellen eines Event Hubs die in der [Event Hub-Dokumentation](https://docs.microsoft.com/azure/event-hubs/event-hubs-create) beschriebenen Schritte aus.

2. Erstellen Sie eine Consumergruppe, die ausschließlich von Ihrer Time Series Insights-Ereignisquelle verwendet wird.

  > [!IMPORTANT]
  > Achten Sie darauf, dass diese Consumergruppe nicht von einem anderen Dienst (beispielsweise durch einen Stream Analytics-Auftrag oder durch eine andere Time Series Insights-Umgebung) verwendet wird. Wenn die Consumergruppe von anderen Diensten verwendet wird, wirkt sich das negativ auf Lesevorgänge für diese Umgebung und andere Dienste aus. Wenn Sie „$Default“ als Consumergruppe verwenden, wird sie unter Umständen von anderen Lesern wiederverwendet.

  ![Auswählen der Event Hub-Consumergruppe](media/send-events/consumer-group.png)

3. Erstellen Sie auf dem Event Hub die Richtlinie „MySendPolicy“. Diese wird im Beispiel weiter unten zum Senden von Ereignissen verwendet.

  ![Auswählen von „SAS-Richtlinien“ und Klicken auf „Hinzufügen“](media/send-events/shared-access-policy.png)  

  ![Hinzufügen der neuen SAS-Richtlinie](media/send-events/shared-access-policy-2.png)  

## <a name="create-time-series-insights-event-source"></a>Erstellen der Time Series Insights-Ereignisquelle
1. Falls Sie noch keine Ereignisquelle erstellt haben, führen Sie die Schritte in [dieser Anleitung](time-series-insights-add-event-source.md) aus.

2. Geben Sie „deviceTimestamp“ als Name der timestamp-Eigenschaft an. Diese Eigenschaft wird im Beispiel weiter unten als tatsächlicher Zeitstempel verwendet. Bei der timestamp-Eigenschaft muss die Groß-/Kleinschreibung beachtet werden, und Werte müssen das Format __yyyy-MM-ddTHH:mm:ss.FFFFFFFK__ besitzen, wenn sie als JSON an Event Hub gesendet werden. Sollte die Eigenschaft im Ereignis nicht vorhanden sein, wird der Zeitpunkt verwendet, zu dem das Ereignis der Warteschlange für Event Hub hinzugefügt wurde.

  ![Erstellen der Ereignisquelle](media/send-events/event-source-1.png)

## <a name="run-sample-code-to-push-events"></a>Ausführen von Beispielcode, um Ereignisse mithilfe von Push zu übertragen
1. Navigieren Sie zur Event Hub-Richtlinie „MySendPolicy“, und kopieren Sie die Verbindungszeichenfolge mit dem Richtlinienschlüssel.

  ![Kopieren der MySendPolicy-Verbindungszeichenfolge](media/send-events/sample-code-connection-string.png)

2. Führen Sie den folgenden Code aus. Dieser Code sendet 600 Ereignisse über jedes der drei Geräte. Aktualisieren Sie `eventHubConnectionString` mit Ihrer Verbindungszeichenfolge.

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.IO;
using Microsoft.ServiceBus.Messaging;

namespace Microsoft.Rdx.DataGenerator
{
    internal class Program
    {
        private static void Main(string[] args)
        {
            var from = new DateTime(2017, 4, 20, 15, 0, 0, DateTimeKind.Utc);
            Random r = new Random();
            const int numberOfEvents = 600;

            var deviceIds = new[] { "device1", "device2", "device3" };

            var events = new List<string>();
            for (int i = 0; i < numberOfEvents; ++i)
            {
                for (int device = 0; device < deviceIds.Length; ++device)
                {
                    // Generate event and serialize as JSON object:
                    // { "deviceTimestamp": "utc timestamp", "deviceId": "guid", "value": 123.456 }
                    events.Add(
                        String.Format(
                            CultureInfo.InvariantCulture,
                            @"{{ ""deviceTimestamp"": ""{0}"", ""deviceId"": ""{1}"", ""value"": {2} }}",
                            (from + TimeSpan.FromSeconds(i * 30)).ToString("o"),
                            deviceIds[device],
                            r.NextDouble()));
                }
            }

            // Create event hub connection.
            var eventHubConnectionString = @"Endpoint=sb://...";
            var eventHubClient = EventHubClient.CreateFromConnectionString(eventHubConnectionString);

            using (var ms = new MemoryStream())
            using (var sw = new StreamWriter(ms))
            {
                // Wrap events into JSON array:
                sw.Write("[");
                for (int i = 0; i < events.Count; ++i)
                {
                    if (i > 0)
                    {
                        sw.Write(',');
                    }
                    sw.Write(events[i]);
                }
                sw.Write("]");

                sw.Flush();
                ms.Position = 0;

                // Send JSON to event hub.
                EventData eventData = new EventData(ms);
                eventHubClient.Send(eventData);
            }
        }
    }
}

```
## <a name="supported-json-shapes"></a>Unterstützte JSON-Formen
### <a name="sample-1"></a>Beispiel 1

#### <a name="input"></a>Eingabe

Ein einfaches JSON-Objekt.

```json
{
    "deviceId":"device1",
    "deviceTimestamp":"2016-01-08T01:08:00Z"
}
```
#### <a name="output---1-event"></a>Ausgabe: ein Ereignis

|deviceId|deviceTimestamp|
|--------|---------------|
|device1|2016-01-08T01:08:00Z|

### <a name="sample-2"></a>Beispiel 2

#### <a name="input"></a>Eingabe
Ein JSON-Array mit zwei JSON-Objekten. Jedes JSON-Objekt wird in ein Ereignis konvertiert.
```json
[
    {
        "deviceId":"device1",
        "deviceTimestamp":"2016-01-08T01:08:00Z"
    },
    {
        "deviceId":"device2",
        "deviceTimestamp":"2016-01-17T01:17:00Z"
    }
]
```
#### <a name="output---2-events"></a>Ausgabe: zwei Ereignisse

|deviceId|deviceTimestamp|
|--------|---------------|
|device1|2016-01-08T01:08:00Z|
|device2|2016-01-08T01:17:00Z|
### <a name="sample-3"></a>Beispiel 3

#### <a name="input"></a>Eingabe

Ein JSON-Objekt mit einem geschachtelten JSON-Array, das zwei JSON-Objekte enthält.
```json
{
    "location":"WestUs",
    "events":[
        {
            "deviceId":"device1",
            "deviceTimestamp":"2016-01-08T01:08:00Z"
        },
        {
            "deviceId":"device2",
            "deviceTimestamp":"2016-01-17T01:17:00Z"
        }
    ]
}

```
#### <a name="output---2-events"></a>Ausgabe: zwei Ereignisse
Beachten Sie, dass die location-Eigenschaft in die einzelnen Ereignisse kopiert wird.

|location|events.deviceId|events.deviceTimestamp|
|--------|---------------|----------------------|
|WestUs|device1|2016-01-08T01:08:00Z|
|WestUs|device2|2016-01-08T01:17:00Z|

### <a name="sample-4"></a>Beispiel 4

#### <a name="input"></a>Eingabe

Ein JSON-Objekt mit einem geschachtelten JSON-Array, das zwei JSON-Objekte enthält. Diese Eingabe zeigt, dass die globalen Eigenschaften vom komplexen JSON-Objekt dargestellt werden können.

```json
{
    "location":"WestUs",
    "manufacturerInfo":{
        "name":"manufacturer1",
        "location":"EastUs"
    },
    "events":[
        {
            "deviceId":"device1",
            "deviceTimestamp":"2016-01-08T01:08:00Z",
            "deviceData":{
                "type":"pressure",
                "units":"psi",
                "value":108.09
            }
        },
        {
            "deviceId":"device2",
            "deviceTimestamp":"2016-01-17T01:17:00Z",
            "deviceData":{
                "type":"vibration",
                "units":"abs G",
                "value":217.09
            }
        }
    ]
}
```
#### <a name="output---2-events"></a>Ausgabe: zwei Ereignisse

|location|manufacturerInfo.name|manufacturerInfo.location|events.deviceId|events.deviceTimestamp|events.deviceData.type|events.deviceData.units|events.deviceData.value|
|---|---|---|---|---|---|---|---|
|WestUs|manufacturer1|EastUs|device1|2016-01-08T01:08:00Z|pressure|psi|108.09|
|WestUs|manufacturer1|EastUs|device2|2016-01-08T01:17:00Z|vibration|abs G|217.09|

## <a name="next-steps"></a>Nächste Schritte

* Anzeigen Ihrer Umgebung im [Time Series Insights-Portal](https://insights.timeseries.azure.com)

