---
title: 'Behandeln von Inhaltstypen: Azure Logic Apps | Microsoft-Dokumentation'
description: Umgang von Azure Logic Apps mit Inhaltstypen zur Entwurfs- und Laufzeit
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: cd1f08fd-8cde-4afc-86ff-2e5738cc8288
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/18/2016
ms.author: jehollan
translationtype: Human Translation
ms.sourcegitcommit: 8a531f70f0d9e173d6ea9fb72b9c997f73c23244
ms.openlocfilehash: 3206c076350c2105e92d320ce30ce73448ccd3bd
ms.lasthandoff: 03/10/2017


---
# <a name="handle-content-types-in-logic-apps"></a>Behandeln von Inhaltstypen in Logik-Apps

Viele verschiedene Arten von Inhalten können durch Logik-Apps fließen – einschließlich JSON, XML, Flatfiles und binären Daten. Zwar werden alle Inhaltstypen unterstützt, einige werden jedoch nativ vom Logic Apps-Modul verstanden, während andere möglicherweise eine Umwandlung oder Konvertierung erfordern. In diesem Artikel wird beschrieben, wie das Modul verschiedene Inhaltstypen behandelt und wie diese Typen bei Bedarf ordnungsgemäß behandelt werden.

## <a name="content-type-header"></a>Header „Content-Type“

Sehen Sie sich zum Einstieg zunächst die beiden `Content-Types` an, die keine Konvertierung oder Umwandlung innerhalb einer Logik-App erfordern: `application/json` und `text/plain`.

## <a name="applicationjson"></a>Anwendung/JSON

Das Workflowmodul verlässt sich auf den `Content-Type` -Header aus HTTP-Aufrufen, um die richtige Verarbeitung zu ermitteln. Jede Anforderung mit dem Inhaltstyp `application/json` wird als JSON-Objekt gespeichert und behandelt. Zudem kann JSON-Inhalt standardmäßig ohne Umwandlung analysiert werden. 

Beispielsweise können Sie eine Anforderung mit dem Header `application/json ` in einem Workflow analysieren, indem Sie mithilfe eines Ausdruck wie `@body('myAction')['foo'][0]` den Wert `bar` in diesem Fall abrufen:

```
{
    "data": "a",
    "foo": [
        "bar"
    ]
}
```

Es ist keine zusätzliche Umwandlung erforderlich. Wenn Sie mit JSON-Daten arbeiten, für die kein Header angegeben wurde, können Sie diese mithilfe der `@json()`-Funktion manuell in JSON umwandeln, z.B.: `@json(triggerBody())['foo']`.

### <a name="schema-and-schema-generator"></a>Schema und Schemagenerator

Der Anforderungstrigger ermöglicht Ihnen, ein JSON-Schema für die Nutzlast eingeben, deren Empfang Sie erwarten. Dieses Schema ermöglicht dem Designer, Token zu generieren, damit Sie den Inhalt der Anforderung nutzen können. Wenn Sie nicht bereits über ein Schema verfügen, wählen Sie `Use sample payload to generate schema` aus, um ein JSON-Schema aus einer Beispielnutzlast zu generieren.

![Schema](./media/logic-apps-http-endpoint/manualtrigger.png)

### <a name="parse-json-action"></a>JSON-Analyseaktion

Mit der Aktion vom Typ `Parse JSON` können Sie JSON-Inhalt analysieren, um anzeigbare Token zur Nutzung in der Logik-App zu erhalten. Ähnlich wie beim Anforderungstrigger können Sie mit dieser Aktion ein JSON-Schema für den Inhalt, den Sie analysieren möchten, eingeben oder generieren. Dieses Tool erleichtert die Nutzung von Daten von Service Bus, DocumentDB usw. wesentlich.

![JSON-Analyse](./media/logic-apps-content-type/ParseJSON.png)

## <a name="textplain"></a>text/plain

Ähnlich wie bei `application/json` werden eingehende HTTP-Nachrichten mit dem `Content-Type`-Header `text/plain` im Rohformat gespeichert. Darüber hinaus wird die Anforderung bei nachfolgenden Aktionen ohne Umwandlung mit dem Header `Content-Type`: `text/plain` übermittelt. Bei der Arbeit mit einer Flatfile erhalten Sie beispielsweise den folgenden HTTP-Inhalt als `text/plain`:

```
Date,Name,Address
Oct-1,Frank,123 Ave.
```

Wenn Sie in der nächsten Aktion diese Anforderung als Text einer anderen Anforderung senden (`@body('flatfile')`), hat die Anforderung den Inhaltstypheader `text/plain`. Wenn Sie mit Daten im Textformat arbeiten, für die kein Header angegeben wurde, können Sie diese mithilfe der `@string()`-Funktion manuell umwandeln , z.B. `@string(triggerBody())`.

## <a name="applicationxml-and-applicationoctet-stream-and-converter-functions"></a>application/xml und application/octet-stream und Konvertierungsfunktionen

Das Logic Apps-Modul behält immer den `Content-Type` bei, der in der HTTP-Anforderung oder der Antwort empfangen wurde. Wenn ein Inhalt mit dem `Content-Type` `application/octet-stream` eingeht, führt das Einfügen in eine nachfolgende Aktion ohne Umwandlung zu einer ausgehenden Anforderung mit `Content-Type`: `application/octet-stream`. Auf diese Weise kann das Modul garantieren, dass die Daten beim Austausch im gesamten Workflow nicht verloren gehen. Der Aktionszustand (Eingaben und Ausgaben) wird jedoch in einem JSON-Objekt gespeichert, während er den Workflow durchläuft. Zum Erhalten einiger Datentypen konvertiert das Modul den Inhalt in eine Base64-codierte Binärzeichenfolge mit entsprechenden Metadaten, in der `$content` und `$content-type` beibehalten werden, wobei eine automatische Konvertierung erfolgt. 

* `@json()`: wandelt Daten in `application/json` um
* `@xml()`: wandelt Daten in `application/xml` um
* `@binary()`: wandelt Daten in `application/octet-stream` um
* `@string()`: wandelt Daten in `text/plain` um
* `@base64()` : konvertiert Inhalt in eine Base64-Zeichenfolge
* `@base64toString()`: konvertiert eine Base64-codierte Zeichenfolge in `text/plain`
* `@base64toBinary()`: konvertiert eine Base64-codierte Zeichenfolge in `application/octet-stream`
* `@encodeDataUri()` : codiert eine Zeichenfolge als dataUri-Bytearray
* `@decodeDataUri()` : decodiert einen dataUri in ein Bytearray

Wenn Sie z.B. eine HTTP-Anforderung mit `Content-Type`: `application/xml` empfangen:

```
<?xml version="1.0" encoding="UTF-8" ?>
<CustomerName>Frank</CustomerName>
```

können Sie mit `@xml(triggerBody())` eine Umwandlung durchführen und sie später verwenden, oder Sie verwenden dazu eine Funktion wie `@xpath(xml(triggerBody()), '/CustomerName')`.

## <a name="other-content-types"></a>Andere Inhaltstypen

Andere Inhaltstypen werden ebenfalls unterstützt und können in einer Logik-App verwendet werden. Sie müssen aber möglicherweise den Nachrichtentext manuell abrufen, indem Sie den `$content` decodieren. Lösen Sie beispielsweise eine `application/x-www-url-formencoded`-Anforderung aus, bei der `$content` die Nutzlast ist, die als Base64-Zeichenfolge codiert ist, um alle Daten zu erhalten:

```
CustomerName=Frank&Address=123+Avenue
```

Da die Eingabe der Anforderung nicht im Nur-Text- oder JSON-Format erfolgt ist, wird die Anforderung wie folgt in der Aktion gespeichert:

```
...
"body": {
    "$content-type": "application/x-www-url-formencoded",
    "$content": "AAB1241BACDFA=="
}
```

Derzeit gibt es keine native Funktion für Formulardaten. Daher können Sie diese Daten innerhalb eines Workflows verwenden. Greifen Sie dazu manuell mit einer Funktion wie `@string(body('formdataAction'))` auf die Daten zu. Wenn die ausgehende Anforderung auch den Inhaltstypheader `application/x-www-url-formencoded` haben soll, können Sie die Anforderung einfach im Aktionstext hinzufügen, ohne eine Umwandlung wie `@body('formdataAction')` durchzuführen. Diese Methode funktioniert jedoch nur, wenn der Text der einzige Parameter der `body`-Eingabe ist. Wenn Sie versuchen, `@body('formdataAction')` in einer `application/json`-Anforderung zu verwenden, erhalten Sie einen Laufzeitfehler, da der codierte Text gesendet wird.


