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
ms.sourcegitcommit: 6efa2cca46c2d8e4c00150ff964f8af02397ef99
ms.openlocfilehash: 9f2d3b57a42efb7b04566278d3267b3cdbed713a
ms.contentlocale: de-de
ms.lasthandoff: 07/01/2017

---
<a id="send-events-to-a-time-series-insights-environment-via-event-hub" class="xliff"></a>

# Senden von Ereignissen an die Azure Time Series Insights-Umgebung per Event Hub

In diesem Tutorial wird erläutert, wie Sie Event Hub erstellen und konfigurieren. Außerdem erfahren Sie, wie Sie eine Beispielanwendung ausführen, um Ereignisse mithilfe von Push zu übertragen. Wenn Sie bereits über einen Event Hub mit Ereignissen im JSON-Format verfügen, überspringen Sie dieses Tutorial, sehen Sie sich Ihre Umgebung in [Time Series Insights](https://insights.timeseries.azure.com) an.

<a id="configure-an-event-hub" class="xliff"></a>

## Konfigurieren eines Event Hubs
1. Führen Sie zum Erstellen eines Event Hubs die in der [Event Hub-Dokumentation](https://docs.microsoft.com/azure/event-hubs/event-hubs-create) beschriebenen Schritte aus.

2. Erstellen Sie eine Consumergruppe, die ausschließlich von Ihrer Time Series Insights-Ereignisquelle verwendet wird.

  > [!IMPORTANT]
  > Achten Sie darauf, dass diese Consumergruppe nicht von einem anderen Dienst (beispielsweise durch einen Stream Analytics-Auftrag oder durch eine andere Time Series Insights-Umgebung) verwendet wird. Wenn die Consumergruppe von anderen Diensten verwendet wird, wirkt sich das negativ auf Lesevorgänge für diese Umgebung und andere Dienste aus. Wenn Sie „$Default“ als Consumergruppe verwenden, wird sie unter Umständen von anderen Lesern wiederverwendet.

  ![Auswählen der Event Hub-Consumergruppe](media/send-events/consumer-group.png)

3. Erstellen Sie auf dem Event Hub die Richtlinie „MySendPolicy“. Diese wird im CSharp-Beispiel zum Senden von Ereignissen verwendet.

  ![Auswählen von „SAS-Richtlinien“ und Klicken auf „Hinzufügen“](media/send-events/shared-access-policy.png)  

  ![Hinzufügen der neuen SAS-Richtlinie](media/send-events/shared-access-policy-2.png)  

<a id="create-time-series-insights-event-source" class="xliff"></a>

## Erstellen der Time Series Insights-Ereignisquelle
1. Falls Sie noch keine Ereignisquelle erstellt haben, führen Sie die Schritte in [dieser Anleitung](time-series-insights-add-event-source.md) aus.

2. Geben Sie „deviceTimestamp“ als Name der timestamp-Eigenschaft an. Diese Eigenschaft wird im CSharp-Beispiel als tatsächlicher Zeitstempel verwendet. Bei der timestamp-Eigenschaft muss die Groß-/Kleinschreibung beachtet werden, und Werte müssen das Format __yyyy-MM-ddTHH:mm:ss.FFFFFFFK__ besitzen, wenn sie als JSON an Event Hub gesendet werden. Sollte die Eigenschaft im Ereignis nicht vorhanden sein, wird der Zeitpunkt verwendet, zu dem der Event Hub in die Warteschlange eingereiht wurde.

  ![Erstellen der Ereignisquelle](media/send-events/event-source-1.png)

<a id="sample-code-to-push-events" class="xliff"></a>

## Beispielcode zum Übertragen von Ereignissen mithilfe von Push
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
<a id="supported-json-shapes" class="xliff"></a>

## Unterstützte JSON-Formen
<a id="sample-1" class="xliff"></a>

### Beispiel 1

<a id="input" class="xliff"></a>

#### Eingabe

Ein einfaches JSON-Objekt.

```json
{
    "id":"device1",
    "timestamp":"2016-01-08T01:08:00Z"
}
```
<a id="output---1-event" class="xliff"></a>

#### Ausgabe: ein Ereignis

|id|timestamp|
|--------|---------------|
|device1|2016-01-08T01:08:00Z|

<a id="sample-2" class="xliff"></a>

### Beispiel 2

<a id="input" class="xliff"></a>

#### Eingabe
Ein JSON-Array mit zwei JSON-Objekten. Jedes JSON-Objekt wird in ein Ereignis konvertiert.
```json
[
    {
        "id":"device1",
        "timestamp":"2016-01-08T01:08:00Z"
    },
    {
        "id":"device2",
        "timestamp":"2016-01-17T01:17:00Z"
    }
]
```
<a id="output---2-events" class="xliff"></a>

#### Ausgabe: zwei Ereignisse

|id|timestamp|
|--------|---------------|
|device1|2016-01-08T01:08:00Z|
|device2|2016-01-08T01:17:00Z|
<a id="sample-3" class="xliff"></a>

### Beispiel 3

<a id="input" class="xliff"></a>

#### Eingabe

Ein JSON-Objekt mit einem geschachtelten JSON-Array, das zwei JSON-Objekte enthält.
```json
{
    "location":"WestUs",
    "events":[
        {
            "id":"device1",
            "timestamp":"2016-01-08T01:08:00Z"
        },
        {
            "id":"device2",
            "timestamp":"2016-01-17T01:17:00Z"
        }
    ]
}

```
<a id="output---2-events" class="xliff"></a>

#### Ausgabe: zwei Ereignisse
Beachten Sie, dass die location-Eigenschaft in die einzelnen Ereignisse kopiert wird.

|location|events.id|events.timestamp|
|--------|---------------|----------------------|
|WestUs|device1|2016-01-08T01:08:00Z|
|WestUs|device2|2016-01-08T01:17:00Z|

<a id="sample-4" class="xliff"></a>

### Beispiel 4

<a id="input" class="xliff"></a>

#### Eingabe

Ein JSON-Objekt mit einem geschachtelten JSON-Array, das zwei JSON-Objekte enthält. Diese Eingabe zeigt, dass die globalen Eigenschaften vom komplexen JSON-Objekt dargestellt werden können.

```json
{
    "location":"WestUs",
    "manufacturer":{
        "name":"manufacturer1",
        "location":"EastUs"
    },
    "events":[
        {
            "id":"device1",
            "timestamp":"2016-01-08T01:08:00Z",
            "data":{
                "type":"pressure",
                "units":"psi",
                "value":108.09
            }
        },
        {
            "id":"device2",
            "timestamp":"2016-01-17T01:17:00Z",
            "data":{
                "type":"vibration",
                "units":"abs G",
                "value":217.09
            }
        }
    ]
}
```
<a id="output---2-events" class="xliff"></a>

#### Ausgabe: zwei Ereignisse

|location|manufacturer.name|manufacturer.location|events.id|events.timestamp|events.data.type|events.data.units|events.data.value|
|---|---|---|---|---|---|---|---|
|WestUs|manufacturer1|EastUs|device1|2016-01-08T01:08:00Z|pressure|psi|108.09|
|WestUs|manufacturer1|EastUs|device2|2016-01-08T01:17:00Z|vibration|abs G|217.09|

<a id="next-steps" class="xliff"></a>

## Nächste Schritte

* Anzeigen Ihrer Umgebung im [Time Series Insights-Portal](https://insights.timeseries.azure.com)

