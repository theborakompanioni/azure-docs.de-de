---
title: Behandeln des Inhaltstyps von Logik-Apps | Microsoft Docs
description: Erfahren Sie, wie Logik-Apps zur Entwurfs- und Laufzeit mit Inhaltstypen umgehen.
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: dwrede
editor: ''

ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 05/03/2016
ms.author: jehollan

---
# Behandeln des Inhaltstyps von Logik-Apps
Es gibt viele verschiedene Arten von Inhalten für Logik-Apps – einschließlich JSON, XML, Flatfiles und binären Daten. Zwar werden alle Inhaltstypen unterstützt, einige werden jedoch nativ vom Logic Apps-Modul verstanden, während andere möglicherweise eine Umwandlung oder Konvertierung erfordern. Im folgende Artikel wird beschrieben, wie das Modul die verschiedenen Inhaltstypen behandelt und wie sie bei Bedarf ordnungsgemäß verarbeitet werden können.

## Header „Content-Type“
Sehen Sie sich für den einfachen Einstieg zunächst die beiden `Content-Types` an, die keine Konvertierung oder Umwandlung innerhalb einer Logik-App erfordern: `application/json` und `text/plain`.

### application/json
Das Workflowmodul verlässt sich auf den `Content-Type`-Header aus HTTP-Aufrufen, um die richtige Verarbeitung zu ermitteln. Jede Anforderung mit dem Inhaltstyp `application/json` wird als JSON-Objekt gespeichert und behandelt. Darüber hinaus kann JSON-Inhalt standardmäßig ohne Umwandlung analysiert werden. Eine Anforderung mit dem Inhaltstypheader `application/json ` wie z.B.:

```
{
    "data": "a",
    "foo": [
        "bar"
    ]
}
```

kann in einem Workflow mit einem Ausdruck wie `@body('myAction')['foo'][0]` analysiert werden, um einen Wert abzurufen (in diesem Fall `bar`). Es ist keine zusätzliche Umwandlung erforderlich. Wenn Sie mit JSON-Daten arbeiten, für die kein Header angegeben wurde, können Sie diese mithilfe der `@json()`-Funktion manuell in JSON umwandeln (Beispiel: `@json(triggerBody())['foo']`).

### text/plain
Ähnlich wie bei `application/json` werden eingehende HTTP-Nachrichten mit dem `Content-Type`-Header `text/plain` im Rohformat gespeichert. Darüber hinaus wird die Anforderung bei nachfolgenden Aktionen ohne Umwandlung mit dem Header `Content-Type`: `text/plain` übermittelt. Angenommen, Sie erhalten bei der Arbeit mit einer Flatfile den folgenden HTTP-Inhalt:

```
Date,Name,Address
Oct-1,Frank,123 Ave.
```

als `text/plain`. Wenn Sie in der nächsten Aktion diesen Inhalt als Text einer anderen Anforderung senden (`@body('flatfile')`), hat die Anforderung den Inhaltstypheader `text/plain`. Wenn Sie mit Daten im Textformat arbeiten, für die kein Header angegeben wurde, können Sie diese mithilfe der `@string()`-Funktion manuell umwandeln (Beispiel: `@string(triggerBody())`).

### application/xml und application/octet-stream und Konvertierungsfunktionen
Das Logic Apps-Modul behält immer den `Content-Type` bei, der in der HTTP-Anforderung oder der Antwort empfangen wurde. Das heißt, wenn ein Inhalt mit dem `Content-Type` `application/octet-stream` eingeht, führt das Einfügen in eine nachfolgende Aktion ohne eine Umwandlung zu einer ausgehenden Anforderung mit `Content-Type`: `application/octet-stream`. Auf diese Weise kann das Modul garantieren, dass die Daten nicht beim Austausch im gesamten Workflow verloren gehen. Der Aktionszustand (Eingaben und Ausgaben) wird jedoch in einem JSON-Objekt gespeichert, während er den Workflow durchläuft. Wenn Sie also einige Datentypen erhalten möchten, konvertiert das Modul den Inhalt in eine Base64-codierte Binärzeichenfolge mit entsprechenden Metadaten, in der `$content` und `$content-type` beibehalten werden – wobei eine automatische Konvertierung erfolgt. Sie können die Konvertierung zwischen den verschiedenen Inhaltstypen auch manuell mit dem integrierten Konvertierungsfunktionen konvertieren:

* `@json()`: wandelt Daten in `application/json` um
* `@xml()`: wandelt Daten in `application/xml` um
* `@binary()`: wandelt Daten in `application/octet-stream` um
* `@string()`: wandelt Daten in `text/plain` um
* `@base64()`: konvertiert Inhalt in eine Base64-Zeichenfolge
* `@base64toString()`: konvertiert eine Base64-codierte Zeichenfolge in `text/plain`
* `@base64toBinary()`: konvertiert eine Base64-codierte Zeichenfolge in `application/octet-stream`
* `@encodeDataUri()`: codiert eine Zeichenfolge als dataUri-Bytearray
* `@decodeDataUri()`: decodiert einen dataUri in ein Bytearray

Wenn Sie z.B. eine HTTP-Anforderung mit `Content-Type`: `application/xml` empfangen:

```
<?xml version="1.0" encoding="UTF-8" ?>
<CustomerName>Frank</CustomerName>
```

können Sie mit `@xml(triggerBody())` eine Umwandlung durchführen und sie später verwenden, oder Sie verwenden dazu eine Funktion wie `@xpath(xml(triggerBody()), '/CustomerName')`.

### Andere Inhaltstypen
Andere Inhaltstypen werden ebenfalls unterstützt und können in einer Logik-App verwendet werden. Sie müssen aber möglicherweise den Nachrichtentext manuell abrufen, indem Sie den `$content` decodieren. Angenommen, Sie verwenden als Trigger eine `application/x-www-url-formencoded`-Anforderung wie die folgende:

```
CustomerName=Frank&Address=123+Avenue
```

Da dies weder Nur-Text noch JSON ist, erfolgt die Speicherung in der Aktion als:

```
...
"body": {
    "$content-type": "application/x-www-url-formencoded",
    "$content": "AAB1241BACDFA=="
}
```

Dabei ist `$content` die Nutzlast, die als Base64-Zeichenfolge codiert ist, um alle Daten zu erhalten. Obwohl es derzeit keine native Funktion für Formulardaten gibt, können Sie diese Daten innerhalb eines Workflows verwenden. Greifen Sie dazu manuell mit einer Funktion wie `@string(body('formdataAction'))` auf die Daten zu. Wenn die ausgehende Anforderung auch den Inhaltstypheader `application/x-www-url-formencoded` haben soll, können Sie ihn einfach im Aktionstext hinzufügen, ohne eine Umwandlung wie `@body('formdataAction')` durchzuführen. Dies funktioniert jedoch nur, wenn der Text der einzige Parameter der `body`-Eingabe ist. Wenn Sie versuchen, `@body('formdataAction')` in einer `application/json`-Anforderung auszuführen, erhalten Sie einen Laufzeitfehler, da die Übermittlung als codierter Text erfolgt.

<!---HONumber=AcomDC_0803_2016-->