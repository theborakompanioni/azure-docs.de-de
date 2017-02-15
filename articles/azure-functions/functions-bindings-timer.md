---
title: Azure Functions-Trigger mit Timer | Microsoft Docs
description: Erfahren Sie, wie Trigger mit Timer in Azure Functions verwendet werden.
services: functions
documentationcenter: na
author: christopheranderson
manager: erikre
editor: 
tags: 
keywords: Azure Functions, Funktionen, Ereignisverarbeitung, dynamisches Compute, serverlose Architektur
ms.assetid: d2f013d1-f458-42ae-baf8-1810138118ac
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/31/2016
ms.author: chrande; glenga
translationtype: Human Translation
ms.sourcegitcommit: b41a5aacec6748af5ee05b01487310cc339af1f9
ms.openlocfilehash: 542e5378aff893741a68c979bc2c5e8bfe58ba26


---
# <a name="azure-functions-timer-trigger"></a>Azure Functions-Trigger mit Timer
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Dieser Artikel erläutert das Konfigurieren und Codieren von Triggern mit Timer in Azure Functions. Azure Functions unterstützt die Trigger für Timer. Trigger mit Timer rufen Funktionen anhand eines Zeitplans einmalig oder wiederholt auf. 

Der Trigger mit Timer unterstützt das horizontale Hochskalieren mit mehreren Instanzen. Eine einzelne Instanz einer bestimmten Timerfunktion wird auf allen Instanzen ausgeführt.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a id="trigger"></a>

## <a name="timer-trigger"></a>Trigger mit Timer
Der Trigger mit Timer zu einer Funktion verwendet das folgende JSON-Objekte im `bindings`-Array von „function.json“:

```json
{
    "schedule": "<CRON expression - see below>",
    "name": "<Name of trigger parameter in function signature>",
    "type": "timerTrigger",
    "direction": "in"
}
```

Der Wert von `schedule` ist ein [CRON-Ausdruck](http://en.wikipedia.org/wiki/Cron#CRON_expression) mit sechs Feldern: `{second} {minute} {hour} {day} {month} {day of the week}`. In vielen der CRON-Ausdrücke, die Sie online finden, ist das `{second}`-Feld weggelassen. Wenn Sie aus einem von ihnen kopieren, müssen Sie eine Anpassung für das zusätzliche `{second}`-Feld vornehmen. Spezifische Beispiele finden Sie unten unter [Beispiele für Zeitpläne](#examples).

Als Standardzeitzone wird in Verbindung mit den CRON-Ausdrücken die Coordinated Universal Time (UTC) verwendet. Wenn Sie möchten, dass Ihr CRON-Ausdruck auf einer anderen Zeitzone basiert, erstellen Sie eine neue App-Einstellung für die Funktions-App mit dem Namen `WEBSITE_TIME_ZONE`. Legen Sie den Wert auf den Namen der gewünschten Zeitzone gemäß [Microsoft Time Zone Index](https://msdn.microsoft.com/library/ms912391.aspx) (Microsoft-Zeitzonenindex) fest. 

Beispiel: *Eastern Normalzeit* ist UTC-05:00. Wenn Sie Ihren Trigger mit Timer täglich um 10:00 Uhr EST auslösen möchten, könnten Sie den folgenden CRON-Ausdruck verwenden, der die UTC-Zeitzone berücksichtigt:

```json
"schedule": "0 0 15 * * *",
``` 

Sie könnten auch eine neue App-Einstellung für die Funktions-App mit dem Namen `WEBSITE_TIME_ZONE` hinzufügen und den Wert auf **Eastern Standard Time** (Eastern Normalzeit) festlegen.  Dann könnte der folgende CRON-Ausdruck für 10:00 Uhr EST verwendet werden: 

```json
"schedule": "0 0 10 * * *",
``` 


<a name="examples"></a>

## <a name="schedule-examples"></a>Beispiele für Zeitpläne
Hier sind einige Beispiele für CRON-Ausdrücke, die Sie für die `schedule`-Eigenschaft verwenden können. 

Alle 5 Minuten auslösen:

```json
"schedule": "0 */5 * * * *"
```

Einmal zu jeder vollen Stunde auslösen:

```json
"schedule": "0 0 * * * *",
```

Einmal alle zwei Stunden auslösen:

```json
"schedule": "0 0 */2 * * *",
```

Zwischen 9:00 und 17:00 Uhr jeweils einmal pro Stunde auslösen:

```json
"schedule": "0 0 9-17 * * *",
```

Täglich um 9:30 Uhr auslösen:

```json
"schedule": "0 30 9 * * *",
```

Werktags um 9:30 Uhr auslösen:

```json
"schedule": "0 30 9 * * 1-5",
```

<a name="usage"></a>

## <a name="trigger-usage"></a>Triggerverwendung
Bei Aufruf einer Trigger-mit-Timer-Funktion wird das [Timerobjekt](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Timers/TimerInfo.cs) an die Funktion übergeben. Der folgende JSON-Code ist eine Beispieldarstellung des Timerobjekts. 

```json
{
    "Schedule":{
    },
    "ScheduleStatus": {
        "Last":"2016-10-04T10:15:00.012699+00:00",
        "Next":"2016-10-04T10:20:00+00:00"
    },
    "IsPastDue":false
}
```

<a name="sample"></a>

## <a name="trigger-sample"></a>Triggerbeispiel
Angenommen, der folgende Trigger mit Timer befindet sich im `bindings`-Array von „function.json“:

```json
{
    "schedule": "0 */5 * * * *",
    "name": "myTimer",
    "type": "timerTrigger",
    "direction": "in"
}
```

Beachten Sie das sprachspezifische Beispiel für das Lesen des Timerobjekts, um festzustellen, ob es verzögert ist.

* [C#](#triggercsharp)
* [F#](#triggerfsharp)
* [Node.js](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-sample-in-c"></a>Triggerbeispiel in C# #
```csharp
public static void Run(TimerInfo myTimer, TraceWriter log)
{
    if(myTimer.IsPastDue)
    {
        log.Info("Timer is running late!");
    }
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}" );  
}
```

<a name="triggerfsharp"></a>

### <a name="trigger-sample-in-f"></a>Triggerbeispiel in F# #
```fsharp
let Run(myTimer: TimerInfo, log: TraceWriter ) =
    if (myTimer.IsPastDue) then
        log.Info("F# function is running late.")
    let now = DateTime.Now.ToLongTimeString()
    log.Info(sprintf "F# function executed at %s!" now)
```

<a name="triggernodejs"></a>

### <a name="trigger-sample-in-nodejs"></a>Triggerbeispiel in Node.js
```JavaScript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();

    if(myTimer.isPastDue)
    {
        context.log('Node.js is running late!');
    }
    context.log('Node.js timer trigger function ran!', timeStamp);   

    context.done();
};
```

## <a name="next-steps"></a>Nächste Schritte
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]




<!--HONumber=Nov16_HO3-->


