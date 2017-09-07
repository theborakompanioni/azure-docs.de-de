---
title: "Azure Functions – Twilio-Bindung | Microsoft Docs"
description: Erfahren Sie, wie Twilio-Bindungen in Azure Functions verwendet werden.
services: functions
documentationcenter: na
author: wesmc7777
manager: cfowler
editor: 
tags: 
keywords: Azure Functions, Funktionen, Ereignisverarbeitung, dynamisches Compute, serverlose Architektur
ms.assetid: a60263aa-3de9-4e1b-a2bb-0b52e70d559b
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/20/2016
ms.author: wesmc
ms.custom: H1Hack27Feb2017
ms.translationtype: HT
ms.sourcegitcommit: a0b98d400db31e9bb85611b3029616cc7b2b4b3f
ms.openlocfilehash: e8c5e8f2dfedae26486e1c8afbe0cec3f3228e86
ms.contentlocale: de-de
ms.lasthandoff: 08/29/2017

---
# <a name="send-sms-messages-from-azure-functions-using-the-twilio-output-binding"></a>Senden von SMS-Nachrichten mit Azure Functions mithilfe der Twilio-Ausgabebindung
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

In diesem Artikel wird das Konfigurieren und Codieren von Twilio-Bindungen in Azure Functions erläutert. 

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

Azure Functions unterstützt Twilio-Ausgabebindungen, mit denen Sie in Ihren Funktionen das Senden von SMS-Textnachrichten mit wenigen Codezeilen und einem [Twilio](https://www.twilio.com/)-Konto aktivieren können. 

## <a name="functionjson-for-the-twilio-output-binding"></a>function.json für die Twilio-Ausgabebindung
Die Datei „function.json“ stellt die folgenden Eigenschaften bereit:

|Eigenschaft  |Beschreibung  |
|---------|---------|
|**name**| Variablenname, der im Funktionscode für die Twilio-SMS-Textnachricht verwendet wird |
|**type**| auf `twilioSms` festgelegt werden muss.|
|**accountSid**| Dieser Wert muss auf den Namen einer App-Einstellung festgelegt werden, die Ihre Twilio-Konto-SID enthält.|
|**authToken**| Dieser Wert muss auf den Namen einer App-Einstellung festgelegt werden, die Ihr Twilio-Authentifizierungstoken enthält.|
|**to**| Dieser Wert wird auf die Telefonnummer festgelegt, an die die SMS-Textnachricht gesendet wird.|
|**from**| Dieser Wert wird auf die Telefonnummer festgelegt, von der die SMS-Textnachricht gesendet wird.|
|**direction**| auf `out` festgelegt werden muss.|
|**body**| Dieser Wert kann verwendet werden, um die SMS-Textnachricht als vordefinierten Code festzulegen und nicht dynamisch im Code für die Funktion. |

Beispiel für „function.json“:

```json
{
  "type": "twilioSms",
  "name": "message",
  "accountSid": "TwilioAccountSid",
  "authToken": "TwilioAuthToken",
  "to": "+1704XXXXXXX",
  "from": "+1425XXXXXXX",
  "direction": "out",
  "body": "Azure Functions Testing"
}
```


## <a name="example-c-queue-trigger-with-twilio-output-binding"></a>Beispiel für einen C#-Warteschlangentrigger mit Twilio Ausgabebindung
#### <a name="synchronous"></a>Synchron
Dieser synchrone Beispielcode für einen Azure Storage-Warteschlangentrigger verwendet einen out-Parameter zum Senden einer Textnachricht an einen Kunden, der eine Bestellung aufgegeben hat.

```cs
#r "Newtonsoft.Json"
#r "Twilio.Api"

using System;
using Newtonsoft.Json;
using Twilio;

public static void Run(string myQueueItem, out SMSMessage message,  TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example the queue item is a JSON string representing an order that contains the name of a 
    // customer and a mobile number to send text updates to.
    dynamic order = JsonConvert.DeserializeObject(myQueueItem);
    string msg = "Hello " + order.name + ", thank you for your order.";

    // Even if you want to use a hard coded message and number in the binding, you must at least 
    // initialize the SMSMessage variable.
    message = new SMSMessage();

    // A dynamic message can be set instead of the body in the output binding. In this example, we use 
    // the order information to personalize a text message to the mobile number provided for
    // order status updates.
    message.Body = msg;
    message.To = order.mobileNumber;
}
```

#### <a name="asynchronous"></a>Asynchron
Dieser asynchrone Beispielcode für einen Azure Storage-Warteschlangentrigger sendet eine Textnachricht an einen Kunden, der eine Bestellung aufgegeben hat.

```cs
#r "Newtonsoft.Json"
#r "Twilio.Api"

using System;
using Newtonsoft.Json;
using Twilio;

public static async Task Run(string myQueueItem, IAsyncCollector<SMSMessage> message,  TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example the queue item is a JSON string representing an order that contains the name of a 
    // customer and a mobile number to send text updates to.
    dynamic order = JsonConvert.DeserializeObject(myQueueItem);
    string msg = "Hello " + order.name + ", thank you for your order.";

    // Even if you want to use a hard coded message and number in the binding, you must at least 
    // initialize the SMSMessage variable.
    SMSMessage smsText = new SMSMessage();

    // A dynamic message can be set instead of the body in the output binding. In this example, we use 
    // the order information to personalize a text message to the mobile number provided for
    // order status updates.
    smsText.Body = msg;
    smsText.To = order.mobileNumber;

    await message.AddAsync(smsText);
}
```

## <a name="example-nodejs-queue-trigger-with-twilio-output-binding"></a>Beispiel für einen Node.js-Warteschlangentrigger mit Twilio Ausgabebindung
In diesem Node.js-Beispiel wird eine Textnachricht an einen Kunden gesendet, der eine Bestellung aufgegeben hat.

```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);

    // In this example the queue item is a JSON string representing an order that contains the name of a 
    // customer and a mobile number to send text updates to.
    var msg = "Hello " + myQueueItem.name + ", thank you for your order.";

    // Even if you want to use a hard coded message and number in the binding, you must at least 
    // initialize the message binding.
    context.bindings.message = {};

    // A dynamic message can be set instead of the body in the output binding. In this example, we use 
    // the order information to personalize a text message to the mobile number provided for
    // order status updates.
    context.bindings.message = {
        body : msg,
        to : myQueueItem.mobileNumber
    };

    context.done();
};
```

## <a name="next-steps"></a>Nächste Schritte
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]


