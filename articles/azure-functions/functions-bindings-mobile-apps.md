---
title: Mobile Apps-Bindungen in Azure Functions | Microsoft-Dokumentation
description: Erfahren Sie, wie Azure Mobile Apps-Bindungen in Azure Functions verwendet werden.
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
keywords: Azure Functions, Funktionen, Ereignisverarbeitung, dynamisches Compute, serverlose Architektur
ms.assetid: faad1263-0fa5-41a9-964f-aecbc0be706a
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/31/2016
ms.author: glenga
ms.translationtype: HT
ms.sourcegitcommit: a0b98d400db31e9bb85611b3029616cc7b2b4b3f
ms.openlocfilehash: d2c0e4e233761584bad2df05a8e702e4fc77e84f
ms.contentlocale: de-de
ms.lasthandoff: 08/29/2017

---
# <a name="azure-functions-mobile-apps-bindings"></a>Mobile Apps-Bindungen in Azure Functions
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Dieser Artikel erläutert das Konfigurieren und Codieren von [Azure Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md)-Bindungen in Azure Functions. Azure Functions unterstützt Eingabe- und Ausgabebindungen für Mobile Apps.

Die Eingabe- und Ausgabebindungen von Mobile Apps ermöglichen das [Lesen aus und Schreiben in Datentabellen](../app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations) in Ihrer mobilen App.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a name="input"></a>

## <a name="mobile-apps-input-binding"></a>Mobile Apps-Eingabebindung
Die Mobile Apps-Eingabebindung lädt einen Datensatz aus einem mobilen Tabellenendpunkt und übergibt ihn an die Funktion. In C#- und F#-Funktionen werden alle Änderungen am Datensatz automatisch an die Tabelle zurückgesendet, wenn die Funktion erfolgreich beendet wird.

Die Mobile Apps-Eingabe zu einer Funktion verwendet das folgende JSON-Objekt im `bindings`-Array von „function.json“:

```json
{
    "name": "<Name of input parameter in function signature>",
    "type": "mobileTable",
    "tableName": "<Name of your mobile app's data table>",
    "id" : "<Id of the record to retrieve - see below>",
    "connection": "<Name of app setting that has your mobile app's URL - see below>",
    "apiKey": "<Name of app setting that has your mobile app's API key - see below>",
    "direction": "in"
}
```

Beachten Sie Folgendes:

* `id` kann statisch sein oder auf dem Trigger basieren, der die Funktion aufruft. Wenn Sie beispielsweise einen [Warteschlangentrigger]() für Ihre Funktion verwenden, nutzt `"id": "{queueTrigger}"` den Zeichenfolgenwert der Warteschlangennachricht als abzurufende Datensatz-ID.
* `connection` sollte den Namen einer App-Einstellung in Ihrer Funktionen-App enthalten, die wiederum die URL Ihrer mobilen App enthält. Die Funktion konstruiert anhand dieser URL die erforderlichen REST-Vorgänge für Ihre mobile App. Sie [erstellen eine App-Einstellung in Ihrer Funktionen-App](), die die URL Ihrer mobilen App enthält (etwa `http://<appname>.azurewebsites.net`). Anschließend geben Sie den Namen der App-Einstellung in der `connection`-Eigenschaft Ihrer Eingabebindung an. 
* Sie müssen `apiKey` angeben, wenn Sie [einen API-Schlüssel in Ihrem Node.js-Mobile App-Back-End implementieren](https://github.com/Azure/azure-mobile-apps-node/tree/master/samples/api-key) oder [einen API-Schlüssel in Ihrem .NET-Mobile App-Back-End implementieren](https://github.com/Azure/azure-mobile-apps-net-server/wiki/Implementing-Application-Key). Hierzu [erstellen Sie eine App-Einstellung in Ihrer Funktionen-App](), die den API-Schlüssel enthält, und fügen dann die `apiKey`-Eigenschaft in der Eingabebindung mit dem Namen der App-Einstellung hinzu. 
  
  > [!IMPORTANT]
  > Dieser API-Schlüssel darf nicht an die mobilen App-Clients weitergegeben werden. Er sollte nur sicher an dienstseitige Clients wie Azure Functions verteilt werden. 
  > 
  > [!NOTE]
  > Azure Functions speichert die Verbindungsinformationen und API-Schlüssel als App-Einstellungen, sodass sie nicht ins Repository der Quellcodeverwaltung eingecheckt werden. Dadurch werden vertrauliche Daten geschützt.
  > 
  > 

<a name="inputusage"></a>

## <a name="input-usage"></a>Eingabeverwendung
Dieser Abschnitt veranschaulicht die Verwendung Ihrer Mobile Apps-Eingabebindung in Ihrem Funktionscode. 

Wenn der Datensatz mit der angegebenen Tabelle und Datensatz-ID gefunden wird, wird er an den benannten [JObject](http://www.newtonsoft.com/json/help/html/t_newtonsoft_json_linq_jobject.htm)-Parameter übergeben (in Node.js wird er an das `context.bindings.<name>`-Objekt übergeben). Wenn der Datensatz nicht gefunden wird, ist der Parameter `null`. 

In C#- und F#-Funktionen werden alle Änderungen am Eingabedatensatz (Eingabeparameter) automatisch wieder an die Mobile Apps-Tabelle zurückgesendet, wenn die Funktion erfolgreich beendet wird. Verwenden Sie in Node.js-Funktionen `context.bindings.<name>` für den Zugriff auf den Eingabedatensatz. Ein Datensatz kann in Node.js nicht geändert werden.

<a name="inputsample"></a>

## <a name="input-sample"></a>Eingabebeispiel
Nehmen wir an, dass Sie über die folgende Datei „function.json“ verfügen, die einen Mobile App-Tabellendatensatz mit der ID der Warteschlangentriggernachricht abruft:

```json
{
"bindings": [
    {
    "name": "myQueueItem",
    "queueName": "myqueue-items",
    "connection":"",
    "type": "queueTrigger",
    "direction": "in"
    },
    {
        "name": "record",
        "type": "mobileTable",
        "tableName": "MyTable",
        "id" : "{queueTrigger}",
        "connection": "My_MobileApp_Url",
        "apiKey": "My_MobileApp_Key",
        "direction": "in"
    }
],
"disabled": false
}
```

Sehen Sie sich das sprachspezifische Beispiel an, das den Eingabedatensatz aus der Bindung verwendet. In den C#- und F#-Beispielen wird auch die `text`-Eigenschaft des Datensatzes geändert.

* [C#](#inputcsharp)
* [Node.js](#inputnodejs)

<a name="inputcsharp"></a>

### <a name="input-sample-in-c"></a>Eingabebeispiel in C# #

```cs
#r "Newtonsoft.Json"    
using Newtonsoft.Json.Linq;

public static void Run(string myQueueItem, JObject record)
{
    if (record != null)
    {
        record["Text"] = "This has changed.";
    }    
}
```

<!--
<a name="inputfsharp"></a>
### Input sample in F# ## 

```fsharp
#r "Newtonsoft.Json"    
open Newtonsoft.Json.Linq
let Run(myQueueItem: string, record: JObject) =
  inputDocument?text <- "This has changed."
```
-->

<a name="inputnodejs"></a>

### <a name="input-sample-in-nodejs"></a>Eingabebeispiel in Node.js

```javascript
module.exports = function (context, myQueueItem) {    
    context.log(context.bindings.record);
    context.done();
};
```

<a name="output"></a>

## <a name="mobile-apps-output-binding"></a>Mobile Apps-Ausgabebindung
Schreiben Sie unter Verwendung der Mobile Apps-Ausgabebindung einen neuen Datensatz in einen Mobile Apps-Tabellenendpunkt.  

Die Mobile Apps-Ausgabe für eine Funktion verwendet das folgende JSON-Objekt im `bindings`-Array von „function.json“:

```json
{
    "name": "<Name of output parameter in function signature>",
    "type": "mobileTable",
    "tableName": "<Name of your mobile app's data table>",
    "connection": "<Name of app setting that has your mobile app's URL - see below>",
    "apiKey": "<Name of app setting that has your mobile app's API key - see below>",
    "direction": "out"
}
```

Beachten Sie Folgendes:

* `connection` sollte den Namen einer App-Einstellung in Ihrer Funktionen-App enthalten, die wiederum die URL Ihrer mobilen App enthält. Die Funktion konstruiert anhand dieser URL die erforderlichen REST-Vorgänge für Ihre mobile App. Sie [erstellen eine App-Einstellung in Ihrer Funktionen-App](), die die URL Ihrer mobilen App enthält (etwa `http://<appname>.azurewebsites.net`). Anschließend geben Sie den Namen der App-Einstellung in der `connection`-Eigenschaft Ihrer Eingabebindung an. 
* Sie müssen `apiKey` angeben, wenn Sie [einen API-Schlüssel in Ihrem Node.js-Mobile App-Back-End implementieren](https://github.com/Azure/azure-mobile-apps-node/tree/master/samples/api-key) oder [einen API-Schlüssel in Ihrem .NET-Mobile App-Back-End implementieren](https://github.com/Azure/azure-mobile-apps-net-server/wiki/Implementing-Application-Key). Hierzu [erstellen Sie eine App-Einstellung in Ihrer Funktionen-App](), die den API-Schlüssel enthält, und fügen dann die `apiKey`-Eigenschaft in der Eingabebindung mit dem Namen der App-Einstellung hinzu. 
  
  > [!IMPORTANT]
  > Dieser API-Schlüssel darf nicht an die mobilen App-Clients weitergegeben werden. Er sollte nur sicher an dienstseitige Clients wie Azure Functions verteilt werden. 
  > 
  > [!NOTE]
  > Azure Functions speichert die Verbindungsinformationen und API-Schlüssel als App-Einstellungen, sodass sie nicht ins Repository der Quellcodeverwaltung eingecheckt werden. Dadurch werden vertrauliche Daten geschützt.
  > 
  > 

<a name="outputusage"></a>

## <a name="output-usage"></a>Ausgabeverwendung
Dieser Abschnitt veranschaulicht die Verwendung Ihrer Mobile Apps-Ausgabebindung in Ihrem Funktionscode. 

Verwenden Sie in C#-Funktionen einen benannten Ausgabeparameter vom Typ `out object`, um auf den Ausgabedatensatz zuzugreifen. Verwenden Sie in Node.js-Funktionen `context.bindings.<name>` für den Zugriff auf den Ausgabedatensatz.

<a name="outputsample"></a>

## <a name="output-sample"></a>Ausgabebeispiel
Nehmen wir an, dass Sie über die folgende Datei „function.json“ verfügen, die einen Warteschlangentrigger und eine Mobile Apps-Ausgabe definiert:

```json
{
"bindings": [
    {
    "name": "myQueueItem",
    "queueName": "myqueue-items",
    "connection":"",
    "type": "queueTrigger",
    "direction": "in"
    },
    {
    "name": "record",
    "type": "mobileTable",
    "tableName": "MyTable",
    "connection": "My_MobileApp_Url",
    "apiKey": "My_MobileApp_Key",
    "direction": "out"
    }
],
"disabled": false
}
```

Sehen Sie sich das sprachspezifische Beispiel an, das einen Datensatz im Mobile Apps-Tabellenendpunkt mit dem Inhalt der Warteschlangennachricht erstellt.

* [C#](#outcsharp)
* [Node.js](#outnodejs)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a>Ausgabebeispiel in C# #

```cs
public static void Run(string myQueueItem, out object record)
{
    record = new {
        Text = $"I'm running in a C# function! {myQueueItem}"
    };
}
```

<!--
<a name="outfsharp"></a>
### Output sample in F# ## 
```fsharp

```
-->
<a name="outnodejs"></a>

### <a name="output-sample-in-nodejs"></a>Ausgabebeispiel in Node.js

```javascript
module.exports = function (context, myQueueItem) {

    context.bindings.record = {
        text : "I'm running in a Node function! Data: '" + myQueueItem + "'"
    }   

    context.done();
};
```

## <a name="next-steps"></a>Nächste Schritte
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]


