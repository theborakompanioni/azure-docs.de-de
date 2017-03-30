---
title: "Workflowaktionen und -trigger – Azure Logic Apps | Microsoft-Dokumentation"
description: 
services: logic-apps
author: MandiOhlinger
manager: anneta
editor: 
documentationcenter: 
ms.assetid: 86a53bb3-01ba-4e83-89b7-c9a7074cb159
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 11/17/2016
ms.author: mandia
translationtype: Human Translation
ms.sourcegitcommit: 424d8654a047a28ef6e32b73952cf98d28547f4f
ms.openlocfilehash: 3a240ff317e1b3ea450703965629c08053668856
ms.lasthandoff: 03/22/2017

---

# <a name="workflow-actions-and-triggers-for-azure-logic-apps"></a>Workflowaktionen und -trigger für Azure Logic Apps

Logik-Apps bestehen aus Triggern und Aktionen. Es gibt sechs Arten von Triggern. Diese unterscheiden sich jeweils durch ihre Schnittstelle und ihr Verhalten. Weitere ausführliche Informationen finden Sie unter [Schema der Definitionssprache für Workflows für Azure Logic Apps](logic-apps-workflow-definition-language.md).  
  
Lesen Sie weiter, um sich ausführlicher über Trigger und Aktionen zu informieren, und erfahren Sie, wie Sie Logik-Apps mit Triggern und Aktionen erstellen, um Ihre Geschäftsprozesse und Workflows zu verbessern.  
  
### <a name="triggers"></a>Trigger  

Ein Trigger gibt die Aufrufe an, die eine Ausführung Ihres Logik-App-Workflows initiieren können. Die Ausführung Ihres Workflows kann auf zwei Arten initiiert werden:  
  
-   Durch einen Abfragetrigger  

-   Durch einen Pushtrigger (mittels Aufruf der [Workflow Service-REST-API](https://docs.microsoft.com/rest/api/logic/workflows))  
  
Alle Trigger enthalten folgende allgemeine Elemente:  
  
```json
"<name-of-the-trigger>" : {
    "type": "<type-of-trigger>",
    "inputs": { <settings-for-the-call> },
    "recurrence": {  
        "frequency": "Second|Minute|Hour|Week|Month|Year",
        "interval": "<recurrence interval in units of frequency>"
    },
    "conditions": [ <array-of-required-conditions > ],
    "splitOn" : "<property to create runs for>",
    "operationOptions": "<operation options on the trigger>"
}
```

### <a name="trigger-types-and-their-inputs"></a>Triggertypen und -eingaben  

Folgende Arten von Triggern stehen zur Verfügung:
  
-   **Request:** Macht die Logik-App zu einem aufrufbaren Endpunkt.  
  
-   **Recurrence:** Wird auf der Grundlage eines definierten Zeitplans ausgelöst.  
  
-   **HTTP:** Fragt einen HTTP-Webendpunkt ab. Der HTTP-Endpunkt muss einem bestimmten auslösenden Vertrag entsprechen – entweder durch Verwendung eines asynchronen Musters (202) oder durch Rückgabe eines Arrays.  
  
-   **ApiConnection:** Führt genau wie der HTTP-Trigger eine Abfrage aus, nutzt dabei aber die [von Microsoft verwalteten APIs](https://docs.microsoft.com/azure/connectors/apis-list).  
  
-   **HTTPWebhook:** Öffnet ähnlich wie der manuelle Trigger einen Endpunkt, ruft aber auch eine angegebene URL für die Registrierung/Aufhebung der Registrierung auf.  
  
-   **ApiConnectionWebhook:** Nutzt genau wie der HTTPWebhook-Trigger die von Microsoft-verwalteten APIs.       
    Jeder Triggertyp verfügt über einen anderen Satz von **Eingaben**, der sein Verhalten definiert.  
  
## <a name="request-trigger"></a>Anforderungstrigger  

Dieser Trigger fungiert als Endpunkt, den Sie über eine HTTP-Anforderung aufrufen, um Ihre Logik-App aufzurufen. Das folgende Beispiel zeigt einen Anforderungstrigger:  
  
```json
"<name-of-the-trigger>" : {
    "type" : "request",
    "kind": "http",
    "inputs" : {
        "schema" : {
            "properties" : {
                "myInputProperty1" : { "type" : "string" },
                "myInputProperty2" : { "type" : "number" }
            },
        "required" : [ "myInputProperty1" ],
        "type" : "object"
        }
    }
} 
```

Zur Verfügung steht auch eine optionale Eigenschaft namens **schema**:  
  
|Elementname|Erforderlich|Beschreibung|  
|----------------|------------|---------------|  
|schema|Nein|Ein JSON-Schema zur Validierung der eingehenden Anforderung. Hilfreich, um nachfolgende Workflowschritte darüber zu informieren, auf welche Eigenschaften verwiesen werden soll.|

Zum Aufrufen dieses Endpunkts muss die API *listCallbackUrl* aufgerufen werden. Weitere Informationen finden Sie unter [Workflows](https://docs.microsoft.com/rest/api/logic/workflows).  
  
## <a name="recurrence-trigger"></a>Recurrence-Trigger  

Recurrence-Trigger werden auf der Grundlage eines definierten Zeitplans ausgeführt. Das folgende Beispiel zeigt einen solchen Trigger:  

```json
"dailyReport" : {
    "type": "recurrence",
    "recurrence": {
        "frequency": "Day",
        "interval": "1"
    }
}
```

Wie Sie sehen, handelt es sich hierbei um eine einfache Möglichkeit zur Ausführung eines Workflows.  
  
|Elementname|Erforderlich|Beschreibung|  
|----------------|------------|---------------|  
|frequency|Ja|Gibt an, wie oft der Trigger ausgeführt wird. Verwenden Sie ausschließlich folgende mögliche Werte: „second“, „minute“, „hour“, „day“, „week“, „month“ oder „year“|  
|interval|Ja|Intervall mit der angegebenen Häufigkeit für die Wiederholung|  
|startTime|Nein|Bei Angabe einer Startzeit ohne UTC-Abweichung wird die angegebene Zeitzone verwendet.|  
|timeZone|no|Bei Angabe einer Startzeit ohne UTC-Abweichung wird die angegebene Zeitzone verwendet.|  
  
Sie können die Triggerausführung auch für einen Zeitpunkt in der Zukunft planen. Wenn Sie also beispielsweise jeden Montag einen wöchentlichen Bericht starten möchten, können Sie die Logik-App mithilfe des folgenden Triggers so planen, dass sie jeden Montag startet:  

```json
"dailyReport" : {
    "type": "recurrence",
    "recurrence": {
        "frequency": "Week",
        "interval": "1",
        "startTime" : "2015-06-22T00:00:00Z"
    }
}
```

## <a name="http-trigger"></a>HTTP-Trigger  

HTTP-Trigger fragen einen angegebenen Endpunkt ab und überprüfen anhand der Antwort, ob der Workflow ausgeführt werden soll. Das Eingabeobjekt nimmt die Parameter an, die zum Erstellen eines HTTP-Aufrufs benötigt werden:  
  
|Elementname|Erforderlich|Beschreibung|Typ|  
|----------------|------------|---------------|--------|  
|method|Ja|Mögliche HTTP-Methoden: GET, POST, PUT, DELETE, PATCH und HEAD|String|  
|uri|Ja|Der HTTP- oder HTTPS-Endpunkt, der aufgerufen wird. Maximal 2 KB.|String|  
|Abfragen|Nein|Ein Objekt, das die Abfrageparameter darstellt, die der URL hinzugefügt werden sollen. `"queries" : { "api-version": "2015-02-01" }` fügt der URL beispielsweise `?api-version=2015-02-01` hinzu.|Objekt|  
|headers|Nein|Ein Objekt, das die einzelnen Header darstellt, die an die Anforderung gesendet werden. Verwenden Sie beispielsweise Folgendes, um Sprache und Typ für eine Anforderung festzulegen: `"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`|Objekt|  
|body|Nein|Ein Objekt, das die an den Endpunkt gesendete Nutzlast darstellt.|Objekt|  
|retryPolicy|Nein|Ein Objekt zum Anpassen des Wiederholungsverhaltens bei Fehlern vom Typ „4xx“ oder „5xx“.|Objekt|  
|Authentifizierung|Nein|Stellt die Methode dar, mit der die Anforderung authentifiziert werden soll. Ausführliche Informationen zu diesem Objekt finden Sie unter [Ausgehende Authentifizierung von Scheduler](https://docs.microsoft.com/azure/scheduler/scheduler-outbound-authentication). Neben Scheduler wird noch die Eigenschaft `authority` unterstützt. Ohne Angabe wird standardmäßig der Wert `https://login.windows.net` verwendet. Sie können aber eine andere Zielgruppe verwenden (beispielsweise `https://login.windows\-ppe.net`).|Objekt|  
  
Für den HTTP-Trigger muss die HTTP-API einem bestimmten Muster entsprechen, damit sie problemlos mit Ihrer Logik-App zusammenarbeitet. Folgende Felder werden benötigt:  
  
|Antwort|Beschreibung|  
|------------|---------------|  
|Statuscode|Statuscode 200 (\(OK\)), um eine Ausführung zu bewirken. Alle anderen Statuscodes bewirken keine Ausführung.|  
|Retry\-After-Header|Anzahl von Sekunden bis zur erneuten Abfrage des Endpunkts durch die Logik-App.|  
|Adressheader|Die URL, die im nächsten Abfrageintervall aufgerufen werden soll. Ohne Angabe wird die ursprüngliche URL verwendet.|  
  
Im Anschluss finden Sie einige Beispiele für die unterschiedlichen Verhaltensweisen verschiedener Anforderungsarten:  
  
|Antwortcode|Retry\-After|Verhalten|  
|-----------------|----------------|------------|  
|200|\(keine\)|Kein gültiger Trigger. „Retry\-After“ muss angegeben werden. Andernfalls führt das Modul bei der nächsten Anforderung keine Abfrage aus.|  
|202|60|Der Workflow wird nicht ausgelöst. Der nächste Versuch erfolgt in einer Minute.|  
|200|10|Der Workflow wird ausgeführt, und in zehn Sekunden wird auf weitere Inhalte geprüft.|  
|400|\(keine\)|Ungültige Anforderung. Der Workflow wird nicht ausgeführt. Falls keine **Wiederholungsrichtlinie** definiert ist, wird die Standardrichtlinie verwendet. Bei Erreichen der Anzahl von Wiederholungen wird der Trigger ungültig.|  
|500|\(keine\)|Serverfehler. Der Workflow wird nicht ausgeführt.  Falls keine **Wiederholungsrichtlinie** definiert ist, wird die Standardrichtlinie verwendet. Bei Erreichen der Anzahl von Wiederholungen wird der Trigger ungültig.|  
  
Die Ausgabe eines HTTP-Triggers kann beispielsweise wie folgt aussehen:  
  
|Elementname|Beschreibung|Typ|  
|----------------|---------------|--------|  
|headers|Die Header der HTTP-Antwort.|Objekt|  
|body|Der Text der HTTP-Antwort.|Objekt|  
  
## <a name="api-connection-trigger"></a>APIConnection-Trigger  

Die Grundfunktion des APIConnection-Triggers ist mit der des HTTP-Triggers vergleichbar. Die Parameter zum Identifizieren der Aktion sind jedoch unterschiedlich. Beispiel:  
  
```json
"dailyReport" : {
    "type": "ApiConnection",
    "inputs": {
        "host": {
            "api": {
                "runtimeUrl": "https://myarticles.example.com/"
            },
        }
        "connection": {
            "name": "@parameters('$connections')['myconnection'].name"
        }
    },  
    "method": "POST",
    "body": {
        "category": "awesomest"
    }
}
```

|Elementname|Erforderlich|Typ|Beschreibung|  
|----------------|------------|--------|---------------|  
|host|Ja||Das von „ApiApp“ gehostete Gateway und die ID.|  
|method|Ja|string|Mögliche HTTP-Methoden: **GET**, **POST**, **PUT**, **DELETE**, **PATCH** und **HEAD**|  
|Abfragen|Nein|Objekt|Stellt die Abfrageparameter dar, die der URL hinzugefügt werden sollen. `"queries" : { "api-version": "2015-02-01" }` fügt der URL beispielsweise `?api-version=2015-02-01` hinzu.|  
|headers|Nein|Objekt|Stellt die einzelnen Header dar, die an die Anforderung gesendet werden. Verwenden Sie beispielsweise Folgendes, um Sprache und Typ für eine Anforderung festzulegen: `"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`|  
|body|Nein|Objekt|Stellt die an den Endpunkt gesendete Nutzlast dar.|  
|retryPolicy|Nein|Objekt|Ermöglicht die Anpassung des Wiederholungsverhaltens bei Fehlern vom Typ „4xx“ oder „5xx“.|  
|Authentifizierung|Nein|Objekt|Stellt die Methode dar, mit der die Anforderung authentifiziert werden soll. Ausführliche Informationen zu diesem Objekt finden Sie unter [Ausgehende Authentifizierung von Scheduler](https://docs.microsoft.com/azure/scheduler/scheduler-outbound-authentication).|  
  
Eigenschaften für „host“:  
  
|Elementname|Erforderlich|Beschreibung|  
|----------------|------------|---------------|  
|api runtimeUrl|Ja|Der Endpunkt der verwalteten API.|  
|connection name||Muss ein Verweis auf einen Parameter namens `$connection` sein und ist der Namen der vom Workflow verwendeten Verbindung mit der verwalteten API.|
  
Ausgaben eines APIConnection-Triggers:
  
|Elementname|Typ|Beschreibung|  
|----------------|--------|---------------|  
|headers|Objekt|Die Header der HTTP-Antwort.|  
|body|Objekt|Der Text der HTTP-Antwort.|  
  
## <a name="httpwebhook-trigger"></a>HTTPWebhook-Trigger  

Der HTTPWebhook-Trigger öffnet ähnlich wie der manuelle Trigger einen Endpunkt, ruft aber auch eine angegebene URL für die Registrierung/Aufhebung der Registrierung auf. Das folgende Beispiel zeigt einen HTTPWebhook-Trigger:  

```json
"myappspottrigger": {
    "type": "httpWebhook",
    "inputs": {
        "subscribe": {
            "method": "POST",
            "uri": "https://pubsubhubbub.appspot.com/subscribe",
            "headers": { },
            "body": {
                "hub.callback": "@{listCallbackUrl()}",
                "hub.mode": "subscribe",
                "hub.topic": "https://pubsubhubbub.appspot.com/articleCategories/technology"
            },
            "authentication": { },
            "retryPolicy": { }
        },
        "unsubscribe": {
            "url": "https://pubsubhubbub.appspot.com/subscribe",
            "body": {
                "hub.callback": "@{workflow().endpoint}@{listCallbackUrl()}",
                "hub.mode": "unsubscribe",
                "hub.topic": "https://pubsubhubbub.appspot.com/articleCategories/technology"
            },
            "method": "POST",
            "authentication": { }
        }
    },
    "conditions": [ ]
    }
```

Viele dieser Abschnitte sind optional, und das Verhalten des Webhooks hängt davon ab, welche Abschnitte angegeben bzw. nicht angegeben werden.  
Eigenschaften eines Webhooks:  
  
|Elementname|Erforderlich|Beschreibung|  
|----------------|------------|---------------|  
|subscribe|Nein|Die ausgehende Anforderung, die bei der Triggererstellung aufgerufen wird und die anfängliche Registrierung ausführt.|  
|unsubscribe|Nein|Die ausgehende Anforderung beim Löschen des Triggers.|  
  
-   **subscribe** ist der ausgehende Aufruf, der ausgeführt wird, um auf Ereignisse zu lauschen. Der Aufruf beginnt mit dem gleichen Parametersatz wie normale HTTP-Aktionen. Dieser ausgehende Aufruf wird bei jeder Veränderung des Workflows ausgeführt – beispielsweise bei einer Erneuerung der Anmeldeinformationen oder bei einer Änderung der Eingabeparameter des Triggers.
  
    Zur Unterstützung dieses Aufrufs steht eine neue Funktion zur Verfügung: `@listCallbackUrl()`. Diese Funktion gibt eine eindeutige URL für diesen spezifischen Trigger in diesem Workflow zurück. Er stellt den eindeutigen Bezeichner für die Endpunkte dar, die Dienst-REST verwenden.  
  
-   **unsubscribe** wird aufgerufen, wenn ein Vorgang dazu führt, dass der Trigger ungültig wird. Hierzu zählen:  
  
    -   Löschen oder Deaktivieren des Triggers  
  
    -   Löschen oder Deaktivieren des Workflows  
  
    -   Löschen oder Deaktivieren des Abonnements  
  
    Die Aktion „unsubscribe“ wird von der Logik-App automatisch aufgerufen. Für diese Funktion werden die gleichen Parameter verwendet wie beim HTTP-Trigger.  
  
    Bei den Ausgaben des HTTPWebhook-Triggers handelt es sich um die Inhalte der eingehenden Anforderung:  
  
|Elementname|Typ|Beschreibung|  
|-----------------|--------|---------------|  
|headers|Objekt|Die Header der HTTP-Anforderung.|  
|body|Objekt|Der Text der HTTP-Anforderung.|  

Grenzwerte für eine Webhookaktion können auf die gleiche Weise angegeben werden wie [asynchrone Grenzwerte für HTTP](#asynchronous-limits).
  

## <a name="conditions"></a>Bedingungen  

Bei jedem Trigger können Sie mithilfe einzelner oder mehrerer Bedingungen bestimmen, ob der Workflow ausgeführt werden soll. Beispiel:  

```json
"dailyReport" : {
    "type": "recurrence",
    "conditions": [ {
        "expression": "@parameters('sendReports')"
    } ],
    "recurrence": {
        "frequency": "Day",
        "interval": "1"
    }
}
```

In diesem Fall wird der Bericht nur ausgelöst, wenn der Parameter `sendReports` des Workflows auf „true“ festgelegt ist. Bedingungen können außerdem auf den Statuscode des Triggers verweisen. So können Sie beispielsweise festlegen, dass ein Workflow nur gestartet werden soll, wenn Ihre Website den Statuscode 500 zurückgibt:
  
```  
"conditions": [  
        {  
          "expression": "@equals(triggers().code, 'InternalServerError')"  
        }  
      ]  
```  
  
> [!NOTE]  
> Sobald ein Ausdruck \(auf beliebige Weise\) auf den Statuscode des Triggers verweist, wird das Standardverhalten \(nur bei 200 \(OK\) auslösen\) ersetzt. Wenn die Auslösung also beispielsweise sowohl für den Statuscode 200 als auch für den Statuscode 201 erfolgen soll, müssen Sie als Bedingung Folgendes einfügen: `@or(equals(triggers().code, 200),equals(triggers().code,201))`  
  
## <a name="start-multiple-runs-for-a-request"></a>Starten mehrerer Ausführungen für eine Anforderung

Wenn Sie mehrere Ausführungen für eine einzelne Anforderung starten möchten, können Sie `splitOn` verwenden. Dies ist beispielsweise hilfreich, wenn Sie einen Endpunkt abfragen möchten, der zwischen Abfrageintervallen über mehrere neue Elemente verfügen kann.
  
Bei `splitOn` geben Sie die Eigenschaft innerhalb der Antwortnutzlast mit dem Elementarray an, dessen Elemente Sie jeweils verwenden möchten, um eine Ausführung des Triggers zu starten. Angenommen, Sie verfügen über eine API, die folgende Antwort zurückgibt:  
  
```json
{
    "Status" : "success",
    "Rows" : [
        {  
            "id" : 938109380,
            "name" : "mycoolrow"
        },
        {
            "id" : 938109381,
            "name" : "another row"
        }
    ]
}
```
  
Ihre Logik-App benötigt lediglich den Inhalt von „Rows“. Somit können Sie Ihren Trigger wie im folgenden Beispiel erstellen:  
  
```json
"mysplitter" : {
    "type" : "http",
    "recurrence": {
        "frequency": "Minute",
        "interval": "1"
    },
    "intputs" : {
        "uri" : "https://mydomain.com/myAPI",
        "method" : "GET"
    },
    "splitOn" : "@triggerBody()?.Rows"
}
```
  
In der Workflowdefinition gibt `@triggerBody().name` dann für die erste Ausführung `mycoolrow` und für die zweite Ausführung `another row` zurück. Das folgende Beispiel zeigt die Triggerausgaben:  
  
```json
{
    "body" : {
        "id" : 938109381,
        "name" : "another row"
    }
}
```

Bei Verwendung von `SplitOn` können Sie also keine Eigenschaften außerhalb des Arrays abrufen (in diesem Fall das Feld `Status`).  
  
> [!NOTE]  
> In diesem Beispiel verwenden wir den `?`-Operator, um einen Fehler zu vermeiden, wenn die `Rows`-Eigenschaft nicht vorhanden ist. 
  
## <a name="single-run-instance"></a>Instanz mit einzelner Ausführung

Sie können Trigger mit einer Wiederholungseigenschaft so konfigurieren, dass sie nur ausgelöst werden, wenn alle aktiven Ausführungen abgeschlossen sind. Falls eine geplante Wiederholung vorgesehen ist, während bereits eine Ausführung stattfindet, wird der Trigger übersprungen und im nächsten Wiederholungsintervall eine erneute Prüfung ausgeführt.

Sie können diese Einstellung über die Vorgangsoptionen konfigurieren:

```json
"triggers": {
    "mytrigger": {
        "type": "http",
        "inputs": { ... },
        "recurrence": { ... },
        "operationOptions": "singleInstance"
    }
}
```

## <a name="types-and-inputs"></a>Typen und Eingaben  

Es gibt viele Arten von Aktionen, die jeweils über ein eigenes Verhalten verfügen. Auflistungsaktionen können zahlreiche weitere Aktionen enthalten.

### <a name="standard-actions"></a>Standardaktionen  

-   **HTTP:** Diese Aktion ruft einen HTTP-Webendpunkt auf.  
  
-   **ApiConnection:** Diese Aktion verhält sich wie die HTTP-Aktion, verwendet aber die von Microsoft verwalteten APIs.  
  
-   **ApiConnectionWebhook:** Diese Aktion ist mit „HTTPWebhook“ vergleichbar, verwendet aber die von Microsoft verwalteten APIs.  
  
-   **Response:** Diese Aktion definiert eine Antwort für einen eingehenden Aufruf.  
  
-   **Wait:** Diese einfache Aktion wartet einen festen Zeitraum oder bis zu einer bestimmten Zeit.  
  
-   **Workflow:** Diese Aktion stellt einen geschachtelten Workflow dar.  

### <a name="collection-actions"></a>Auflistungsaktionen

-   **Scope:** Bei dieser Aktion handelt es sich um eine logische Gruppierung anderer Aktionen.

-   **Condition:** Diese Aktion wertet einen Ausdruck aus und führt die entsprechende Ergebnisverzweigung aus.

-   **ForEach:** Diese Schleifenaktion durchläuft ein Array und führt interne Aktionen für die einzelnen Elemente aus.

-   **Until:** Diese Schleifenaktion führt interne Aktionen aus, bis eine Bedingung erfüllt ist.
  
Jede Art von Aktion verfügt über einen anderen Satz von **Eingaben**, die das Verhalten einer Aktion definieren.  
  
## <a name="http-action"></a>HTTP-Aktion  

HTTP-Aktionen rufen einen angegebenen Endpunkt auf und überprüfen anhand der Antwort, ob der Workflow ausgeführt werden soll. Das Objekt **inputs** nimmt die Parameter an, die zum Erstellen des HTTP-Aufrufs benötigt werden:  
  
|Elementname|Erforderlich|Typ|Beschreibung|  
|----------------|------------|--------|---------------|  
|method|Ja|String|Mögliche HTTP-Methoden: **GET**, **POST**, **PUT**, **DELETE**, **PATCH** und **HEAD**|  
|uri|Ja|string|Der HTTP- oder HTTPS-Endpunkt, der aufgerufen wird. Die maximale Länge beträgt 2 KB.|  
|Abfragen|Nein|Objekt|Stellt die Abfrageparameter dar, die der URL hinzugefügt werden sollen. `"queries" : { "api-version": "2015-02-01" }` fügt der URL beispielsweise `?api-version=2015-02-01` hinzu.|  
|headers|Nein|Objekt|Stellt die einzelnen Header dar, die an die Anforderung gesendet werden. Verwenden Sie beispielsweise Folgendes, um Sprache und Typ für eine Anforderung festzulegen: `"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`|  
|body|Nein|Objekt|Stellt die an den Endpunkt gesendete Nutzlast dar.|  
|retryPolicy|Nein|Objekt|Ermöglicht die Anpassung des Wiederholungsverhaltens bei Fehlern vom Typ „4xx“ oder „5xx“.|  
|operationsOptions|Nein|String|Definiert den Satz spezieller Verhaltensweisen, die überschrieben werden sollen.|  
|Authentifizierung|Nein|Objekt|Stellt die Methode dar, mit der die Anforderung authentifiziert werden soll. Ausführliche Informationen zu diesem Objekt finden Sie unter [Ausgehende Authentifizierung von Scheduler](https://docs.microsoft.com/azure/scheduler/scheduler-outbound-authentication). Neben Scheduler wird noch die Eigenschaft `authority` unterstützt. Ohne Angabe wird standardmäßig der Wert `https://login.windows.net` verwendet. Sie können aber eine andere Zielgruppe verwenden (beispielsweise `https://login.windows\-ppe.net`).|  
  
HTTP-Aktionen \(und APIConnection-Aktionen\) unterstützen Wiederholungsrichtlinien. Eine Wiederholungsrichtlinie gilt für vorübergehende Fehler (HTTP-Statuscodes 408, 429 und 5xx) sowie für Verbindungsausnahmen. Diese Richtlinie wird mithilfe des Objekts *retryPolicy* beschrieben, das wie folgt definiert wird:
  
```json
"retryPolicy" : {
    "type": "<type-of-retry-policy>",
    "interval": <retry-interval>,
    "count": <number-of-retry-attempts>
}
```
  
Das Wiederholungsintervall wird im ISO 8601-Format angegeben. Der Mindestwert für dieses Intervall beträgt 20 Sekunden und ist gleichzeitig der Standardwert. Der Maximalwert ist eine Stunde. Die standardmäßige und maximale Wiederholungsanzahl beträgt vier Stunden. Ohne Angabe der Wiederholungsrichtliniendefinition wird eine `fixed`-Strategie mit Standardwerten für Wiederholungsanzahl und Intervall verwendet. Wenn Sie die Wiederholungsrichtlinie deaktivieren möchten, legen Sie den Typ auf `None` fest.  
  
Im folgenden Beispiel wiederholt die Aktion den Abruf der neuesten Nachrichten zweimal, falls ein vorübergehender Fehler vorliegt. Dabei werden insgesamt drei Ausführungen mit einer jeweils 30-sekündigen Verzögerung zwischen den einzelnen Versuchen verwendet:  
  
```json
"latestNews" : {
    "type": "http",
    "inputs": {
        "method": "GET",
        "uri": "uri": "https://mynews.example.com/latest",
        "retryPolicy" : {
            "type": "fixed",
            "interval": "PT30S",
            "count": 2
        }
    }
}
```
### <a name="asynchronous-patterns"></a>Asynchrone Muster

Standardmäßig unterstützen alle HTTP-basierten Aktionen das Standardmuster für asynchrone Vorgänge. Wenn der Remoteserver also durch eine Antwort vom Typ „202 \(Akzeptiert\)“ angibt, dass die Anforderung zur Verarbeitung akzeptiert wird, fragt das Logic Apps-Modul weiterhin die im Adressheader der Antwort angegebene URL ab, bis ein Endzustand \(eine 202\-fremde Antwort\) erreicht ist.  
  
Wenn Sie dieses asynchrone Verhalten deaktivieren möchten, legen Sie in den Aktionseingaben eine `DisableAsyncPattern`-Option fest. In diesem Fall basiert die Ausgabe der Aktion auf der ersten 202-Antwort des Servers.  
  
```json
"invokeLongRunningOperation" : {
    "type": "http",
    "inputs": {
        "method": "POST",
        "uri": "https://host.example.com/resources"
    },
    "operationOptions": "DisableAsyncPattern"
}
```

#### <a name="asynchronous-limits"></a>Asynchrone Grenzwerte

Die Dauer eines asynchronen Musters kann auf ein bestimmtes Zeitintervall begrenzt werden.  Falls das Zeitintervall abläuft, ohne dass ein Endzustand erreicht wurde, wird der Status der Aktion als `Cancelled` mit dem Code `ActionTimedOut` markiert.  Der Grenzwert für das Timeout wird im ISO 8601-Format angegeben.  Grenzwerte können mit folgender Syntax angegeben werden:

``` json
"<action-name>": {
    "type": "workflow|webhook|http|apiconnectionwebhook|apiconnection",
    "inputs": { },
    "limit": {
        "timeout": "PT10S"
    }
}
```
  
## <a name="api-connection"></a>APIConnection  

Bei „APIConnection“ handelt es sich um eine Aktion, die auf einen von Microsoft verwalteten Connector verweist.
Diese Aktion erfordert einen Verweis auf eine gültige Verbindung sowie Informationen zur benötigten API und zu den benötigten Parametern.

|Elementname|Erforderlich|Typ|Beschreibung|  
|----------------|------------|--------|---------------|  
|host|Ja|Objekt|Stellt die Connectorinformationen wie Laufzeit-URL und Verweis auf das Verbindungsobjekt dar.|
|method|Ja|String|Mögliche HTTP-Methoden: **GET**, **POST**, **PUT**, **DELETE**, **PATCH** und **HEAD**|  
|path|Ja|string|Der Pfad des API-Vorgangs.|  
|Abfragen|Nein|Objekt|Stellt die Abfrageparameter dar, die der URL hinzugefügt werden sollen. `"queries" : { "api-version": "2015-02-01" }` fügt der URL beispielsweise `?api-version=2015-02-01` hinzu.|  
|headers|Nein|Objekt|Stellt die einzelnen Header dar, die an die Anforderung gesendet werden. Verwenden Sie beispielsweise Folgendes, um Sprache und Typ für eine Anforderung festzulegen: `"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`|  
|body|Nein|Objekt|Stellt die an den Endpunkt gesendete Nutzlast dar.|  
|retryPolicy|Nein|Objekt|Ermöglicht die Anpassung des Wiederholungsverhaltens bei Fehlern vom Typ „4xx“ oder „5xx“.|  
|operationsOptions|Nein|string|Definiert den Satz spezieller Verhaltensweisen, die überschrieben werden sollen.|  

```json
"Send_Email": {
    "type": "apiconnection",
    "inputs": {
        "host": {
            "api": {
                "runtimeUrl": "https://logic-apis-df.azure-apim.net/apim/office365"
            },
            "connection": {
                "name": "@parameters('$connections')['office365']['connectionId']"
            }
        },
        "method": "post",
        "body": {
            "Subject": "New Tweet from @{triggerBody()['TweetedBy']}",
            "Body": "@{triggerBody()['TweetText']}",
            "To": "me@example.com"
        },
        "path": "/Mail"
    },
    "runAfter": {}
    }
```

## <a name="api-connection-webhook-action"></a>APIConnectionWebhook-Aktion

```json
"Send_approval_email": {
    "type": "apiconnectionwebhook",
    "inputs": {
        "host": {
            "api": {
                "runtimeUrl": "https://logic-apis-df.azure-apim.net/apim/office365"
            },
            "connection": {
                "name": "@parameters('$connections')['office365']['connectionId']"
            }
        },
        "body": {
            "Message": {
                "Subject": "Approval Request",
                "Options": "Approve, Reject",
                "Importance": "Normal",
                "To": "me@email.com"
            }
        },
        "path": "/approvalmail",
        "authentication": "@parameters('$authentication')"
    },
    "runAfter": {}
}
```

Grenzwerte für eine Webhookaktion können auf die gleiche Weise angegeben werden wie [asynchrone Grenzwerte für HTTP](#asynchronous-limits).
  
## <a name="response-action"></a>Antwortaktion  

Dieser Aktionstyp enthält die gesamte Antwortnutzlast aus einer HTTP-Anforderung sowie einen Statuscode, Text und Header:  
  
```json
"myresponse" : {
    "type" : "response",
    "inputs" : {
        "statusCode" : 200,
        "body" : {
            "contentFieldOne" : "value100",
            "anotherField" : 10.001
        },
        "headers" : {
            "x-ms-date" : "@utcnow()",
            "Content-type" : "application/json"
        }
    },
    "runAfter": {}
}
```
  
Bei der Response-Aktion sind besondere Einschränkungen zu berücksichtigen, die für die anderen Aktionen nicht relevant sind. Dies gilt insbesondere in folgenden Fällen:  
  
-   Response-Aktionen können in einer Definition nicht parallel sein, da eine deterministische Reaktion auf die eingehende Anforderung erforderlich ist.  
  
-   Falls eine Response-Aktion erreicht wird, nachdem die eingehende Anforderung eine Antwort empfangen hat, wird die Aktion als fehlerhaft \(Konflikt\) betrachtet und die Ausführung folglich als `Failed` gekennzeichnet.  
  
-   Der Trigger eines Workflows mit Response-Aktionen darf nicht `splitOn` enthalten, da ein einzelner Aufruf mehrere Ausführungen zur Folge hat. Dies muss beim Ausführen des PUT-Vorgangs für den Flow überprüft und einen Fehler aufgrund einer ungültigen Anforderung auslösen.  
  
## <a name="wait-action"></a>Wait-Aktion  

Die Aktion `wait` hält die Ausführung des Workflows für das angegebene Intervall an. Bei Verwendung des folgenden Ausschnitts wird beispielsweise 15 Minuten gewartet:  
  
```json
"waitForFifteenMinutes" : {
    "type": "wait",
    "inputs": {
        "interval": {
            "unit" : "minute",
            "count" : 15
        }
    }
}
```  
  
Alternativ kann bis zu einem bestimmten Zeitpunkt gewartet werden, wie im folgenden Beispiel zu sehen:  
  
```json
"waitUntilOctober" : {
    "type": "wait",
    "inputs": {
        "until": {
            "timestamp" : "2016-10-01T00:00:00Z"
        }
    }
}
```
  
> [!NOTE]  
> Die Wartezeit kann entweder mit dem Objekt **interval** oder mit dem Objekt **until** angegeben werden, aber nicht mit beiden.  
  
|Name|Erforderlich|Typ|Beschreibung|  
|--------|------------|--------|---------------|  
|interval|Nein|Objekt|Die Wartezeit auf der Grundlage des Zeitraums.|  
|interval unit|Ja|String|Eine der folgenden Intervalloptionen: „second“, „minute“, „hour“, „day“, „week“, „month“ oder „year“.|  
|interval count|Ja|String|Die Dauer auf der Grundlage den angegebenen internen Einheit.|  
|until|Nein|Objekt|Die Wartezeit auf der Grundlage eines Zeitpunkts.|  
|Zeitstempel für „until“|Ja|String|Der Zeitpunkt (UTC), zu dem die Wartezeit abläuft.|  

## <a name="query-action"></a>Abfrageaktion

Mit der Aktion `query` können Sie ein Array auf der Grundlage einer Bedingung filtern. Wenn Sie also beispielsweise Zahlen auswählen möchten, die größer 2 sind, können Sie Folgendes verwenden:

```json
"FilterNumbers" : {
    "type": "query",
    "inputs": {
        "from": [ 1, 3, 0, 5, 4, 2 ],
        "where": "@greater(item(), 2)"
    }
}
```

Die Ausgabe der Aktion `query` ist ein Array mit Elementen aus dem Eingabearray, die die Bedingung erfüllen.

> [!NOTE]
> Sollten keine Werte die Bedingung von `where` erfüllen, ist das Ergebnis ein leeres Array.

|Name|Erforderlich|Typ|Beschreibung|
|--------|------------|--------|---------------|
|Aus|Ja|Array|Das Quellarray.|
|Hierbei gilt:|Ja|String|Die Bedingung, die auf die einzelnen Elemente des Quellarrays angewendet werden soll.|

## <a name="terminate-action"></a>Terminate-Aktion

Die Terminate-Aktion beendet die Ausführung des Workflows. Dabei werden alle aktiven Aktionen abgebrochen und alle restlichen Aktionen übersprungen. Mit dem folgenden Codeausschnitt wird beispielsweise eine Ausführung mit dem Status **Fehler** beendet:

```json
"HandleUnexpectedResponse" : {
    "type": "terminate",
    "inputs": {
        "runStatus" : "failed",
        "runError": {
            "code": "UnexpectedResponse",
            "message": "Received an unexpected response.",
        }
    }
}
```

> [!NOTE]
> Bereits abgeschlossene Aktionen sind von der Terminate-Aktion nicht betroffen.

|Name|Erforderlich|Typ|Beschreibung|
|--------|------------|--------|---------------|
|runStatus|Ja|String|Der Zielstatus der Ausführung. Entweder **failed** oder **cancelled**.|
|runError|Nein|Objekt|Die Fehlerdetails. Wird nur unterstützt, wenn **runStatus** auf **failed** festgelegt ist.|
|Code für „runError“|Nein|String|Der Fehlercode für die Ausführung.|
|runError message|Nein|String|Die Fehlermeldung für die Ausführung.|

## <a name="compose-action"></a>Compose-Aktion

Mit der Compose-Aktion können Sie ein beliebiges Objekt erstellen. Die Ausgabe der Compose-Aktion ist das Ergebnis der Eingabenauswertung. Mithilfe der Compose-Aktion können Sie beispielsweise Ausgaben mehrerer Aktionen zusammenführen:

```json
"composeUserRecord" : {
    "type": "compose",
    "inputs": {
        "firstName": "@actions('getUser').firstName",
        "alias": "@actions('getUser').alias",
        "thumbnailLink": "@actions('lookupThumbnail').url"
        }
    }
}
```

> [!NOTE]
> Mit der Aktion **Compose** können Sie eine beliebige Ausgabe erstellen. Hierzu zählen unter anderem Objekte und Arrays sowie jede andere Art von Ausgabe, die nativ von Logik-Apps unterstützt wird (etwa XML- und Binärelemente).

## <a name="workflow-action"></a>Workflow-Aktion   

|Name|Erforderlich|Typ|Beschreibung|  
|--------|------------|--------|---------------|  
|host id|Ja|String|Die Ressourcen-ID des Workflows, den Sie aufrufen möchten.|  
|host triggerName|Ja|String|Der Name des Triggers, den Sie aufrufen möchten.|  
|Abfragen|Nein|Objekt|Stellt die Abfrageparameter dar, die der URL hinzugefügt werden sollen. `"queries" : { "api-version": "2015-02-01" }` fügt der URL beispielsweise `?api-version=2015-02-01` hinzu.|  
|headers|Nein|Objekt|Stellt die einzelnen Header dar, die an die Anforderung gesendet werden. Verwenden Sie beispielsweise Folgendes, um Sprache und Typ für eine Anforderung festzulegen: `"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`|  
|body|Nein|Objekt|Stellt die an den Endpunkt gesendete Nutzlast dar.|  
  
```json
"mynestedwf" : {
    "type" : "workflow",
    "inputs" : {
        "host" : {
            "id" : "/subscriptions/xxxxyyyyzzz/resourceGroups/rg001/providers/Microsoft.Logic/mywf001",
            "triggerName " : "mytrigger001"
        },
        "queries" : {
            "extrafield" : "specialValue"
        },  
        "headers" : {
            "x-ms-date" : "@utcnow()",
            "Content-type" : "application/json"
        },
        "body" : {
            "contentFieldOne" : "value100",
            "anotherField" : 10.001
        }
    },
    "runAfter": {}
    }
```
  
Für den Workflow \(genauer gesagt: für den Trigger\) wird eine Zugriffsprüfung ausgeführt. Sie benötigen daher Zugriff auf den Workflow.  
  
Die Ausgaben der Aktion `workflow` basieren auf der Definition in der Aktion `response` im untergeordneten Workflow. Falls Sie keine Aktion vom Typ `response` definiert haben, sind die Ausgaben leer.  

## <a name="collection-actions-scopes-and-loops"></a>Auflistungsaktionen (Bereiche und Schleifen)

Einige Aktionstypen können andere Aktionen enthalten. Auf Verweisaktionen innerhalb einer Auflistung kann direkt außerhalb der Auflistung verwiesen werden. Wenn Sie `http` in einem Bereich definiert haben, ist `@body('http')` weiterhin überall in einem Workflow gültig. Aktionen innerhalb einer Auflistung können nur nach anderen Aktionen innerhalb der gleichen Auflistung ausgeführt werden (`runAfter`).

## <a name="scope-action"></a>Scope-Aktion

Mit der Aktion `scope` können Sie Aktionen in einem Workflow logisch gruppieren.

|Name|Erforderlich|Typ|Beschreibung|  
|--------|------------|--------|---------------|  
|Aktionen|Ja|Objekt|Interne Aktionen, die innerhalb des Bereichs ausgeführt werden sollen.|

```json
{
    "myScope": {
        "type": "scope",
        "actions": {
            "call_bing": {
                "type": "http",
                "inputs": {
                    "url": "http://www.bing.com"
                }
            }
        }
    }
}
```

## <a name="foreach-action"></a>ForEach-Aktion

Diese Schleifenaktion durchläuft ein Array und führt interne Aktionen für die einzelnen Elemente aus. Die foreach-Schleife wird standardmäßig parallel (20 gleichzeitige Ausführungen) ausgeführt. Ausführungsregeln können mithilfe des `operationOptions`-Parameters festgelegt werden.

|Name|Erforderlich|Typ|Beschreibung|  
|--------|------------|--------|---------------|  
|Aktionen|Ja|Objekt|Interne Aktionen, die innerhalb der Schleife ausgeführt werden sollen.|
|foreach|Ja|string|Das zu durchlaufende Array.|
|operationOptions|no|string|Vorgangsoptionen für das Verhalten. Unterstützt derzeit nur `sequential` für eine sequenzielle Ausführung der Iterationen. (Standardverhalten: parallel)|

```json
"forEach_email": {
    "type": "foreach",
    "foreach": "@body('email_filter')",
    "actions": {
        "send_email": {
            "type": "ApiConnection",
            "inputs": {
                "body": {
                    "to": "@item()",
                    "from": "me@contoso.com",
                    "message": "Hello, thank you for ordering"
                }
                "host": {
                    "connection": {
                        "id": "@parameters('$connections')['office365']['connection']['id']"
                    }
                }
            }
        }
    },
    "runAfter":{
        "email_filter": [ "Succeeded" ]
    }
}
```

## <a name="until-action"></a>Until-Aktion

Diese Schleifenaktion führt interne Aktionen aus, bis eine Bedingung erfüllt ist.

|Name|Erforderlich|Typ|Beschreibung|  
|--------|------------|--------|---------------|  
|Aktionen|Ja|Objekt|Interne Aktionen, die innerhalb der Schleife ausgeführt werden sollen.|
|expression|Ja|string|Der Ausdruck, der nach jeder Iteration ausgewertet werden soll.|
|limit|Ja|Objekt|Die Grenzwerte für die Schleife. Es muss mindestens ein Grenzwert definiert werden.|
|count|no|int|Der Grenzwert für die Anzahl von Iterationen, die ausgeführt werden können.|
|timeout|no|string|Das Timeout für Schleifenausführung.  Angabe im ISO 8601-Format.|


```json
 "Until_succeeded": {
    "actions": {
        "Http": {
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {},
            "type": "Http"
        }
    },
    "expression": "@equals(outputs('Http')['statusCode', 200)",
    "limit": {
        "count": 1000,
        "timeout": "PT1H"
    },
    "runAfter": {},
    "type": "Until"
}
```

## <a name="conditions---if-action"></a>Bedingungen: If-Aktion

Mit der `If`-Aktion können Sie eine Bedingung auswerten und die Ausführung einer Verzweigung davon abhängig machen, ob der Ausdruck als `true` ausgewertet wird.

|Name|Erforderlich|Typ|Beschreibung|  
|--------|------------|--------|---------------|  
|Aktionen|Ja|Objekt|Interne Aktionen, die ausgeführt werden sollen, wenn der Ausdruck als `true` ausgewertet wird.|
|expression|Ja|string|Der auszuwertende Ausdruck.|
|else|no|Objekt|Interne Aktionen, die ausgeführt werden sollen, wenn der Ausdruck als `false` ausgewertet wird.|
  
```json
"My_condition": {
    "actions": {
        "If_true": {
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {},
            "type": "Http"
        }
    },
    "else": {
        "actions": {
            "if_false": {
                "inputs": {
                    "method": "GET",
                    "uri": "http://myurl"
                },
                "runAfter": {},
                "type": "Http"
            }
        }
    },
    "expression": "@equals(triggerBody(), json(true))",
    "runAfter": {},
    "type": "If"
}
```  
  
Die Beispiele in der folgenden Tabelle veranschaulichen, wie Bedingungen Ausdrücke in einer Aktion verwenden können:  
  
|JSON-Wert|Ergebnis|  
|--------------|----------|  
|`"expression": "@parameters('hasSpecialAction')"`|Jeder Wert, der als „true“ ausgewertet wird, bewirkt, dass diese Bedingung als erfüllt betrachtet wird. Nur boolesche Ausdrücke werden unterstützt. Verwenden Sie die Funktionen `empty` und `equals`, um andere Typen in boolesche Werte zu konvertieren.|  
|`"expression": "@greater(actions('act1').output.value, parameters('threshold'))"`|Vergleichsfunktionen werden unterstützt. In dem vorliegenden Beispiel wird die Aktion nur ausgeführt, wenn die Größe der Ausgabe von „act1“ den Schwellenwert übersteigt.|  
|`"expression": "@or(greater(actions('act1').output.value, parameters('threshold')), less(actions('act1').output.value, 100))"`|Logikfunktionen werden ebenfalls unterstützt, um geschachtelte boolesche Ausdrücke zu erstellen. In diesem Fall wird die Aktion ausgeführt, wenn die Ausgabe von „act1“ über dem Schwellenwert oder unter 100 liegt.|  
|`"expression": "@equals(length(actions('act1').outputs.errors), 0))"`|Mithilfe von Arrayfunktionen können Sie prüfen, ob ein Array Elemente enthält. In diesem Fall wird die Aktion ausgeführt, wenn das Fehlerarray leer ist.| 
|`"expression": "parameters('hasSpecialAction')"`|Fehler: Keine gültige Bedingung, da „@“ für Bedingungen erforderlich ist.|  
  
Erfolgreich ausgewertete Bedingungen werden als `Succeeded` markiert. Aktionen innerhalb des Objekts `actions` oder `else` werden wie folgt ausgewertet: als `Succeeded`, wenn die Ausführung erfolgreich war, als `Failed`, wenn bei der Ausführung ein Fehler aufgetreten ist, oder als `Skipped`, wenn die Verzweigung nicht ausgeführt wird.

## <a name="next-steps"></a>Nächste Schritte

[Workflow Service-REST-API](https://docs.microsoft.com/rest/api/logic/workflows)
