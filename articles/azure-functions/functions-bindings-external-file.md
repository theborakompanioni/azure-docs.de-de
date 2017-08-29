---
title: "Azure Functions – Bindungen zu externen Dateien (Vorschau) | Microsoft-Dokumentation"
description: Verwenden von Bindungen zu externen Dateien in Azure Functions
services: functions
documentationcenter: 
author: alexkarcher-msft
manager: cfowler
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 04/12/2017
ms.author: alkarche
ms.translationtype: HT
ms.sourcegitcommit: 0aae2acfbf30a77f57ddfbaabdb17f51b6938fd6
ms.openlocfilehash: 2082e4e9b23271be93f3e3ab43997c3243238da8
ms.contentlocale: de-de
ms.lasthandoff: 08/09/2017

---
# <a name="azure-functions-external-file-bindings-preview"></a>Azure Functions – Bindungen zu externen Dateien (Vorschau)
In diesem Artikel wird gezeigt, wie Dateien von unterschiedlichen SaaS-Anbietern (z. B. OneDrive, Dropbox) in Ihrer Funktion verarbeitet werden, indem integrierte Bindungen verwendet werden. Azure Functions unterstützt Trigger-, Eingabe- und Ausgabebindungen für externe Dateien.

Diese Bindung erstellt API-Verbindungen mit SaaS-Anbietern oder verwendet vorhandene API-Verbindungen aus der Ressourcengruppe Ihrer Funktionen-App.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="supported-file-connections"></a>Unterstützte Dateiverbindungen

|Connector|Trigger|Eingabe|Ausgabe|
|:-----|:---:|:---:|:---:|
|[Box](https://www.box.com)|x|x|x
|[Dropbox](https://www.dropbox.com)|x|x|x
|[FTP](https://docs.microsoft.com/azure/app-service-web/app-service-deploy-ftp)|x|x|x
|[OneDrive](https://onedrive.live.com)|x|x|x
|[OneDrive for Business](https://onedrive.live.com/about/business/)|x|x|x
|[SFTP](https://docs.microsoft.com/azure/connectors/connectors-create-api-sftp)|x|x|x
|[Google Drive](https://www.google.com/drive/)||x|x|

> [!NOTE]
> Verbindungen mit externen Dateien können auch in [Azure Logic Apps](https://docs.microsoft.com/azure/connectors/apis-list) verwendet werden.

## <a name="external-file-trigger-binding"></a>Triggerbindung zu externen Dateien

Der Azure-Trigger für externe Dateien ermöglicht es Ihnen, einen Remoteordner zu überwachen und Ihren Funktionscode auszuführen, wenn Änderungen erkannt werden.

Der Trigger für externe Dateien verwendet die folgenden JSON-Objekte im `bindings`-Array von „function.json“:

```json
{
  "type": "apiHubFileTrigger",
  "name": "<Name of input parameter in function signature>",
  "direction": "in",
  "path": "<folder to monitor, and optionally a name pattern - see below>",
  "connection": "<name of external file connection - see above>"
}
```
<!---
See one of the following subheadings for more information:

* [Name patterns](#pattern)
* [File receipts](#receipts)
* [Handling poison files](#poison)
--->

<a name="pattern"></a>

### <a name="name-patterns"></a>Namensmuster
Sie können in der `path`-Eigenschaft ein Dateinamensmuster angeben. Der Ordner, auf den verwiesen wird, muss im SaaS-Anbieter vorhanden sein.
Beispiele:

```json
"path": "input/original-{name}",
```

Mit diesem Pfad wird eine Datei namens *original-File1.txt* im Ordner *input* gefunden, und der Wert der Variablen `name` im Funktionscode lautet `File1.txt`.

Ein weiteres Beispiel:

```json
"path": "input/{filename}.{fileextension}",
```

Mit diesem Pfad wird ebenfalls eine Datei namens *original-File1.txt* gefunden, und die Werte der Variablen `filename` und `fileextension` im Funktionscode lauten *original-File1* und *txt*.

Sie können den Dateityp von Dateien beschränken, indem Sie einen festen Wert als Dateierweiterung verwenden. Beispiel:

```json
"path": "samples/{name}.png",
```

In diesem Fall wird die Funktion nur für *.png*-Dateien im Ordner *samples* ausgelöst.

Geschweifte Klammern werden in Namensmustern als Sonderzeichen angesehen. Sie können die geschweiften Klammern verdoppeln, um einen Dateinamen anzugeben, der geschweifte Klammern enthält.
Beispiel:

```json
"path": "images/{{20140101}}-{name}",
```

Mit diesem Pfad wird eine Datei namens *{20140101}-soundfile.mp3* im Ordner *images* gefunden, und der Wert der Variablen `name` im Funktionscode lautet *soundfile.mp3*.

<a name="receipts"></a>

<!--- ### File receipts
The Azure Functions runtime makes sure that no external file trigger function gets called more than once for the same new or updated file.
It does so by maintaining *file receipts* to determine if a given file version has been processed.

File receipts are stored in a folder named *azure-webjobs-hosts* in the Azure storage account for your function app
(specified by the `AzureWebJobsStorage` app setting). A file receipt has the following information:

* The triggered function ("*&lt;function app name>*.Functions.*&lt;function name>*", for example: "functionsf74b96f7.Functions.CopyFile")
* The folder name
* The file type ("BlockFile" or "PageFile")
* The file name
* The ETag (a file version identifier, for example: "0x8D1DC6E70A277EF")

To force reprocessing of a file, delete the file receipt for that file from the *azure-webjobs-hosts* folder manually.
--->
<a name="poison"></a>

### <a name="handling-poison-files"></a>Verarbeiten von nicht verarbeitbaren Dateien
Wenn beim Ausführen einer Triggerfunktion für externe Dateien ein Fehler auftritt, versucht Azure Functions standardmäßig bis zu fünf Mal (einschließlich des ersten Versuchs), diese Funktion für die jeweilige Datei auszuführen.
Wenn alle fünf Versuche fehlschlagen, fügt Functions eine Nachricht zu einer Storage-Warteschlange namens *webjobs-apihubtrigger-poison* hinzu. Die Warteschlangennachricht für nicht verarbeitbare Dateien ist ein JSON-Objekt, das die folgenden Eigenschaften enthält:

* FunctionId (im Format *&lt;Funktionen-App-Name>*.Functions.*&lt;Funktionsname>*)
* FileType
* FolderName
* FileName
* ETag (eine Dateiversions-ID. Beispiel: „0x8D1DC6E70A277EF“)


<a name="triggerusage"></a>

## <a name="trigger-usage"></a>Triggerverwendung
In C#-Funktionen stellen Sie mit einem benannten Parameter in Ihrer Funktionssignatur, z.B. `<T> <name>`, eine Bindung zu Eingabedateidaten her.
Dabei ist `T` der Datentyp, in den Sie die Daten deserialisieren möchten, und `paramName` ist der Name, den Sie im [JSON-Code des Triggers](#trigger) angegeben haben. In Node.js-Funktionen greifen Sie mit `context.bindings.<name>` auf die Eingabedateidaten zu.

Die Datei kann in jeden der folgenden Typen deserialisiert werden:

* Beliebiges [Objekt](https://msdn.microsoft.com/library/system.object.aspx) – nützlich für JSON-serialisierte Dateidaten.
  Wenn Sie einen benutzerdefinierten Eingabetyp deklarieren (z.B. `FooType`), versucht Azure Functions, die JSON-Daten in den angegebenen Typ zu deserialisieren.
* Zeichenfolge – nützlich für Textdateidaten.

In C#-Funktionen können Sie auch eine Bindung zu jedem der folgenden Typen erstellen. Die Functions-Runtime versucht dann, die Dateidaten mithilfe dieses Typs zu deserialisieren:

* `string`
* `byte[]`
* `Stream`
* `StreamReader`
* `TextReader`

## <a name="trigger-sample"></a>Triggerbeispiel
Angenommen, Sie haben die folgende „function.json“, die einen Trigger für externe Dateien definiert:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myFile",
            "type": "apiHubFileTrigger",
            "direction": "in",
            "path": "samples-workitems",
            "connection": "<name of external file connection>"
        }
    ]
}
```

Sehen Sie sich das sprachspezifische Beispiel an, in dem der Inhalte jeder Datei protokolliert wird, die dem überwachten Ordner hinzugefügt wurde.

* [C#](#triggercsharp)
* [Node.js](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-usage-in-c"></a>Triggerverwendung in C# #

```cs
public static void Run(string myFile, TraceWriter log)
{
    log.Info($"C# File trigger function processed: {myFile}");
}
```

<!--
<a name="triggerfsharp"></a>
### Trigger usage in F# ##
```fsharp

```
-->

<a name="triggernodejs"></a>

### <a name="trigger-usage-in-nodejs"></a>Triggerverwendung in Node.js

```javascript
module.exports = function(context) {
    context.log('Node.js File trigger function processed', context.bindings.myFile);
    context.done();
};
```

<a name="input"></a>

## <a name="external-file-input-binding"></a>Eingabebindung zu externen Dateien
Die Azure-Eingabebindung zu externen Dateien ermöglicht es Ihnen, in Ihrer Funktion eine Datei aus einem externen Ordner zu verwenden.

Für die aus einer externen Datei stammende Eingabe in eine Funktion werden die folgenden JSON-Objekte im `bindings`-Array von „function.json“ verwendet:

```json
{
  "name": "<Name of input parameter in function signature>",
  "type": "apiHubFile",
  "direction": "in",
  "path": "<Path of input file - see below>",
  "connection": "<name of external file connection>"
},
```

Beachten Sie Folgendes:

* `path` muss den Ordnernamen und den Dateinamen enthalten. Wenn Sie in Ihrer Funktion beispielsweise über einen [Warteschlangentrigger](functions-bindings-storage-queue.md) verfügen, können Sie mit `"path": "samples-workitems/{queueTrigger}"` auf eine Datei im Ordner `samples-workitems` mit einem Namen verweisen, der mit dem in der Triggernachricht angegebenen Dateinamen übereinstimmt.   

<a name="inputusage"></a>

## <a name="input-usage"></a>Eingabeverwendung
In C#-Funktionen stellen Sie mit einem benannten Parameter in Ihrer Funktionssignatur, z.B. `<T> <name>`, eine Bindung zu Eingabedateidaten her.
Dabei ist `T` der Datentyp, in den Sie die Daten deserialisieren möchten, und `paramName` ist der Name, den Sie in der [Eingabebindung](#input) angegeben haben. In Node.js-Funktionen greifen Sie mit `context.bindings.<name>` auf die Eingabedateidaten zu.

Die Datei kann in jeden der folgenden Typen deserialisiert werden:

* Beliebiges [Objekt](https://msdn.microsoft.com/library/system.object.aspx) – nützlich für JSON-serialisierte Dateidaten.
  Wenn Sie einen benutzerdefinierten Eingabetyp deklarieren (z.B. `InputType`), versucht Azure Functions, die JSON-Daten in den angegebenen Typ zu deserialisieren.
* Zeichenfolge – nützlich für Textdateidaten.

In C#-Funktionen können Sie auch eine Bindung zu jedem der folgenden Typen erstellen. Die Functions-Runtime versucht dann, die Dateidaten mithilfe dieses Typs zu deserialisieren:

* `string`
* `byte[]`
* `Stream`
* `StreamReader`
* `TextReader`


<a name="output"></a>

## <a name="external-file-output-binding"></a>Ausgabebindung zu externen Dateien
Die Azure-Ausgabebindung zu externen Dateien ermöglicht es Ihnen, in Ihrer Funktion Dateien in einen externen Ordner zu schreiben.

Für die für eine externe Datei vorgesehene Ausgabe für eine Funktion werden die folgenden JSON-Objekte im `bindings`-Array von „function.json“ verwendet:

```json
{
  "name": "<Name of output parameter in function signature>",
  "type": "apiHubFile",
  "direction": "out",
  "path": "<Path of input file - see below>",
  "connection": "<name of external file connection>"
}
```

Beachten Sie Folgendes:

* `path` muss den Ordnernamen und den Dateinamen der Datei enthalten, in die geschrieben werden soll. Wenn Sie in Ihrer Funktion beispielsweise über einen [Warteschlangentrigger](functions-bindings-storage-queue.md) verfügen, können Sie mit `"path": "samples-workitems/{queueTrigger}"` auf eine Datei im Ordner `samples-workitems` mit einem Namen verweisen, der mit dem in der Triggernachricht angegebenen Dateinamen übereinstimmt.   

<a name="outputusage"></a>

## <a name="output-usage"></a>Ausgabeverwendung
In C#-Funktionen erstellen Sie eine Bindung zu einer Ausgabedatei, indem Sie den benannten `out`-Parameter in Ihrer Funktionssignatur verwenden, etwa `out <T> <name>`. Hierbei ist `T` der Datentyp, in den Sie die Daten serialisieren möchten, und `paramName` ist der Name, den Sie in der [Ausgabebindung](#output) angegeben haben. In Node.js-Funktionen greifen Sie mit `context.bindings.<name>` auf die Ausgabedatei zu.

Sie können in die Ausgabedatei schreiben, indem Sie einen der folgenden Typen verwenden:

* Beliebiges [Objekt](https://msdn.microsoft.com/library/system.object.aspx) – nützlich für JSON-Serialisierung.
  Wenn Sie einen benutzerdefinierten Ausgabetyp deklarieren (z.B. `out OutputType paramName`), versucht Azure Functions, das Objekt in JSON zu serialisieren. Wenn der Ausgabeparameter bei Beendigung der Funktion einen Nullwert hat, erstellt die Functions-Runtime eine Datei als NULL-Objekt.
* Zeichenfolge – (`out string paramName`) nützlich für Textdateidaten. Die Functions-Runtime erstellt nur dann eine Datei, wenn der Zeichenfolgenparameter bei Rückgabe durch die Funktion keinen Nullwert enthält.

In C#-Funktionen können Sie auch eine Ausgabe in einen der folgenden Typen erstellen:

* `TextWriter`
* `Stream`
* `CloudFileStream`
* `ICloudFile`
* `CloudBlockFile`
* `CloudPageFile`

<a name="outputsample"></a>

<a name="sample"></a>

## <a name="input--output-sample"></a>Beispiel für Eingabe und Ausgabe
Angenommen, Sie haben die folgende „function.json“-Datei, in der ein [Storage-Warteschlangentrigger](functions-bindings-storage-queue.md), eine Eingabe aus einer externen Datei und eine Ausgabe in eine externe Datei definiert sind:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myInputFile",
      "type": "apiHubFile",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "<name of external file connection>",
      "direction": "in"
    },
    {
      "name": "myOutputFile",
      "type": "apiHubFile",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "<name of external file connection>",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

Sehen Sie sich das sprachspezifische Beispiel an, in dem die Eingabedatei in die Ausgabedatei kopiert wird.

* [C#](#incsharp)
* [Node.js](#innodejs)

<a name="incsharp"></a>

### <a name="usage-in-c"></a>Verwendung in C# #

```cs
public static void Run(string myQueueItem, string myInputFile, out string myOutputFile, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    myOutputFile = myInputFile;
}
```

<!--
<a name="infsharp"></a>
### Input usage in F# ##
```fsharp

```
-->

<a name="innodejs"></a>

### <a name="usage-in-nodejs"></a>Verwendung in „Node.js“

```javascript
module.exports = function(context) {
    context.log('Node.js Queue trigger function processed', context.bindings.myQueueItem);
    context.bindings.myOutputFile = context.bindings.myInputFile;
    context.done();
};
```

## <a name="next-steps"></a>Nächste Schritte
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

