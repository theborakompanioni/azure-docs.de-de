---
title: 'Logic Apps: Ausnahmebehandlung | Microsoft-Dokumentation'
description: "Enthält eine Beschreibung der Muster für die Fehler- und Ausnahmebehandlung mit Azure Logic Apps."
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: erikre
editor: 
ms.assetid: e50ab2f2-1fdc-4d2a-be40-995a6cc5a0d4
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/18/2016
ms.author: jehollan
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: ec09eb465628ac3c78b9883cbc84bbbf6c0e875e


---
# <a name="logic-apps-error-and-exception-handling"></a>Logik-Apps: Fehler- und Ausnahmebehandlung
Logik-Apps verfügen über ein umfassendes Angebot an Tools und Mustern, damit Sie sicherstellen können, dass Ihre Integrationen robust und widerstandsfähig gegen Fehler sind.  Eine Anforderung in Bezug auf eine Integrationsarchitektur ist die Sicherstellung, dass Ausfallzeiten oder Probleme abhängiger Systeme richtig behandelt werden.  Mit Logik-Apps wird die Behandlung von Fehlern deutlich vereinfacht, da Sie über die Tools verfügen, die Sie zum Reagieren auf Ausnahmen und Fehler in Ihren Workflows benötigen.

## <a name="retry-policies"></a>Wiederholungsrichtlinien
Die einfachste Form der Ausnahme- und Fehlerbehandlung ist eine Wiederholungsrichtlinie.  Mit dieser Richtlinie wird definiert, ob für die Aktion ein Wiederholungsversuch durchgeführt werden soll, wenn bei der ersten Anforderung ein Timeout oder ein Fehler aufgetreten ist (Anforderung mit der Antwort 429 oder 5xx).  Standardmäßig wird versucht, alle Aktionen in Intervallen von jeweils 20 Sekunden vier weitere Male zu wiederholen.  Wenn für die erste Anforderung die Antwort `500 Internal Server Error` empfangen wurde, wartet das Workflowmodul also 20 Sekunden und startet dann einen Wiederholungsversuch für die Anforderung.  Wenn für die Anforderung nach allen Wiederholungsversuchen immer noch eine Ausnahme oder ein Fehler vorliegt, wird der Workflow fortgesetzt, und der Status der Aktion lautet `Failed`.

Sie können Wiederholungsrichtlinien in den **Eingaben** einer bestimmten Aktion konfigurieren.  Eine Wiederholungsrichtlinie kann so konfiguriert werden, dass bis zu vier Wiederholungsversuche im Abstand einer Stunde durchgeführt werden.  Ausführliche Informationen zu den Eingabeeigenschaften finden Sie auf der [MSDN-Website][retryPolicyMSDN].

```json
"retryPolicy" : {
      "type": "<type-of-retry-policy>",
      "interval": <retry-interval>,
      "count": <number-of-retry-attempts>
    }
```

Wenn für die HTTP-Aktion vier Wiederholungsversuche in einem Intervall von 10 Minuten durchgeführt werden sollen, lautet die Definition wie folgt:

```json
"HTTP": 
{
    "inputs": {
        "method": "GET",
        "uri": "http://myAPIendpoint/api/action",
        "retryPolicy" : {
            "type": "fixed",
            "interval": "PT10M",
            "count": 4
        }
    },
    "runAfter": {},
    "type": "Http"
}
```

Weitere Informationen zur unterstützten Syntax finden Sie im [retryPolicy-Abschnitt auf der MSDN-Website][retryPolicyMSDN].

## <a name="runafter-property-to-catch-failures"></a>RunAfter-Eigenschaft zum Abfangen von Fehlern
Für jede Logik-App-Aktion wird deklariert, welche Aktionen abgeschlossen werden müssen, bevor die Aktion gestartet wird.  Sie können sich dies wie die Sortierung von Schritten in Ihrem Workflow vorstellen.  Diese Sortierung wird in der Aktionsdefinition als `runAfter` -Eigenschaft bezeichnet.  Mit diesem Objekt wird beschrieben, für welche Aktionen und Aktionsstatus die Aktion ausgeführt wird.  Standardmäßig werden alle Aktionen, die mit dem Designer hinzugefügt werden, auf die Ausführung nach dem vorherigen Schritt (`runAfter`) festgelegt, wenn der vorherige Schritt `Succeeded` lautet.  Sie können diesen Wert aber so anpassen, dass Aktionen ausgelöst werden, wenn die vorherigen Aktionen `Failed` oder `Skipped` lauten oder ein möglicher Satz mit diesen Werten gilt.  Wenn Sie ein Element einem angegebenen Service Bus-Thema hinzufügen möchten, nachdem eine bestimmte Aktion `Insert_Row` fehlschlägt, verwenden Sie die folgende `runAfter`-Konfiguration:

```json
"Send_message": {
    "inputs": {
        "body": {
            "ContentData": "@{encodeBase64(body('Insert_Row'))}",
            "ContentType": "{ \"content-type\" : \"application/json\" }"
        },
        "host": {
            "api": {
                "runtimeUrl": "https://logic-apis-westus.azure-apim.net/apim/servicebus"
            },
            "connection": {
                "name": "@parameters('$connections')['servicebus']['connectionId']"
            }
        },
        "method": "post",
        "path": "/@{encodeURIComponent('failures')}/messages"
    },
    "runAfter": {
        "Insert_Row": [
            "Failed"
        ]
    }
}
```

Beachten Sie, dass die `runAfter`-Eigenschaft für die Auslösung festgelegt wird, wenn die Aktion `Insert_Row` den Status `Failed` hat.  Die Syntax zum Ausführen der Aktion, wenn der Aktionsstatus `Succeeded`, `Failed` oder `Skipped` ist, lautet wie folgt:

```json
"runAfter": {
        "Insert_Row": [
            "Failed", "Succeeded", "Skipped"
        ]
    }
```

> [!TIP]
> Aktionen, die nach dem Fehlschlagen einer vorhergehenden Aktion ausgeführt werden und erfolgreich sind, werden als `Succeeded` gekennzeichnet.  Dies bedeutet Folgendes: Wenn das Abfangen aller Fehler eines Workflows erfolgreich ist, wird die gesamte Ausführung als `Succeeded`gekennzeichnet.
> 
> 

## <a name="scopes-and-results-to-evaluate-actions"></a>Bereiche und Ergebnisse zum Auswerten von Aktionen
Ähnlich wie bei der Ausführung nach einzelnen Aktionen, können Sie Aktionen auch in einem [Bereich](app-service-logic-loops-and-scopes.md) gruppieren. Ein Bereich dient also als logische Gruppierung von Aktionen.  Bereich sind nützlich, um Logik-App-Aktionen zu organisieren und Gesamtauswertungen zum Status eines Bereichs durchzuführen.  Der Bereich selbst erhält einen Status, nachdem alle Aktionen innerhalb eines Bereichs abgeschlossen wurden.  Der Bereichsstatus wird anhand der gleichen Kriterien wie bei einer Ausführung bestimmt. Wenn die letzte Aktion einer Ausführungsverzweigung `Failed` oder `Aborted` lautet, ergibt sich der Status `Failed`.

Sie können `runAfter` verwenden, wenn ein Bereich als `Failed` gekennzeichnet wurde, um bestimmte Aktionen für Fehler auszulösen, die innerhalb des Bereichs aufgetreten sind.  Durch die Ausführung nach dem Fehlschlagen eines Bereichs können Sie ein einzelne Aktion erstellen, um Fehler abzufangen, wenn für *beliebige* Aktionen im Bereich Fehler auftreten.

### <a name="getting-the-context-of-failures-with-results"></a>Abrufen des Kontexts von Fehlern mit Ergebnissen
Das Abfangen der Fehler eines Bereichs ist sehr nützlich. Aber häufig ist auch der Kontext hilfreich, um genau zu verstehen, für welche Aktionen Fehler aufgetreten sind und welche Fehler oder Statuscodes zurückgegeben wurden.  Die Workflowfunktion `@result()` liefert Kontext zum Ergebnis aller Aktionen innerhalb eines Bereichs.

Für `@result()` wird ein einzelner Parameter (Bereichsname) verwendet und ein Array mit allen Aktionsergebnissen des Bereichs zurückgegeben.  Diese Aktionsobjekte enthalten die gleichen Attribute wie das `@actions()`-Objekt, z.B. Aktionsstartzeit, Aktionsendzeit, Aktionsstatus, Aktionseingaben, Korrelations-IDs der Aktion und Aktionsausgaben.  Sie können eine `@result()`-Funktion problemlos mit `runAfter` koppeln, um Kontext zu allen Aktionen zu senden, für die im Bereich ein Fehler aufgetreten ist.

Wenn Sie für *jede Aktion* eines Bereichs, für die der Status `Failed` lautet, eine Aktion ausführen möchten, können Sie wie folgt vorgehen: Sie können `@result()` mit einer Aktion vom Typ **[Array filtern](../connectors/connectors-native-query.md)** und einer **[ForEach](app-service-logic-loops-and-scopes.md)**-Schleife koppeln.  So können Sie das Array mit den Ergebnissen nach Aktionen mit Fehlern filtern.  Sie können für das Array mit den gefilterten Ergebnissen eine Aktion für jeden Fehler durchführen, indem Sie die **ForEach** -Schleife verwenden.  Unten ist ein Beispiel angegeben, gefolgt von einer ausführlichen Erklärung.  In diesem Beispiel wird eine HTTP POST-Anforderung mit dem Anforderungstext von Aktionen gesendet, die im Bereich `My_Scope`einen Fehler aufweisen.

```json
"Filter_array": {
    "inputs": {
        "from": "@result('My_Scope')",
        "where": "@equals(item()['status'], 'Failed')"
    },
    "runAfter": {
        "My_Scope": [
            "Failed"
        ]
    },
    "type": "Query"
},
"For_each": {
    "actions": {
        "Log_Exception": {
            "inputs": {
                "body": "@item()['outputs']['body']",
                "method": "POST",
                "headers": {
                    "x-failed-action-name": "@item()['name']",
                    "x-failed-tracking-id": "@item()['clientTrackingId']"
                },
                "uri": "http://requestb.in/"
            },
            "runAfter": {},
            "type": "Http"
        }
    },
    "foreach": "@body('Filter_array')",
    "runAfter": {
        "Filter_array": [
            "Succeeded"
        ]
    },
    "type": "Foreach"
}
```

Hier ist eine ausführliche exemplarische Vorgehensweise des Ablaufs angegeben:

1. Durchführen der Aktion **Array filtern** zum Filtern von `@result('My_Scope')`, um das Ergebnis aller Aktionen in `My_Scope` zu erhalten.
2. Die Bedingung von **Array filtern** ist ein beliebiges `@result()`-Element mit dem Status `Failed`.  Das Array mit allen Aktionsergebnissen aus `My_Scope` wird gefiltert, um ein Array nur mit Aktionen mit Fehlerergebnissen zu erhalten.
3. Durchführen einer **For Each**-Aktion für Ausgaben vom Typ **Array gefiltert**.  Hierdurch wird eine Aktion für jede ** Aktion mit Fehlerergebnis durchgeführt, die wir oben herausgefiltert haben.
   * Falls der Bereich nur eine fehlgeschlagene Aktion umfasst, werden die Aktionen in `foreach` nur einmal ausgeführt.  Viele fehlgeschlagene Aktionen führen zu einer Aktion pro Fehler.
4. Senden von HTTP POST für den Antworttext des `foreach`-Elements oder `@item()['outputs']['body']`.  Die `@result()`-Elementform ist mit der `@actions()`-Form identisch und kann auch genauso analysiert werden.
5. Außerdem sind zwei benutzerdefinierte Header mit dem Namen der Fehleraktion `@item()['name']` und der Clientnachverfolgungs-ID der Fehlerausführung `@item()['clientTrackingId']` enthalten.

Zu Referenzzwecken ist hier ein Beispiel für ein einzelnes `@result()` -Element angegeben.  Sie sehen die Eigenschaften `name`, `body` und `clientTrackingId`, die im obigen Beispiel analysiert wurden.  Beachten Sie auch, dass außerhalb eines `foreach`-Elements für `@result()` ein Array mit diesen Objekten zurückgegeben wird.

```json
{
    "name": "Example_Action_That_Failed",
    "inputs": {
        "uri": "https://myfailedaction.azurewebsites.net",
        "method": "POST"
    },
    "outputs": {
        "statusCode": 404,
        "headers": {
            "Date": "Thu, 11 Aug 2016 03:18:18 GMT",
            "Server": "Microsoft-IIS/8.0",
            "X-Powered-By": "ASP.NET",
            "Content-Length": "68",
            "Content-Type": "application/json"
        },
        "body": {
            "code": "ResourceNotFound",
            "message": "/docs/foo/bar does not exist"
        }
    },
    "startTime": "2016-08-11T03:18:19.7755341Z",
    "endTime": "2016-08-11T03:18:20.2598835Z",
    "trackingId": "bdd82e28-ba2c-4160-a700-e3a8f1a38e22",
    "clientTrackingId": "08587307213861835591296330354",
    "code": "NotFound",
    "status": "Failed"
}
```

Sie können die obigen Ausdrücke verwenden, um unterschiedliche Muster für die Ausnahmebehandlung zu nutzen.  Sie können auch die Entscheidung treffen, eine einzelne Aktion für die Ausnahmebehandlung außerhalb des Bereichs durchzuführen, bei der das gesamte gefilterte Array mit Fehlern akzeptiert wird, und das `foreach`-Element entfernen.  Außerdem können Sie andere nützliche Eigenschaften aus der obigen `@result()` -Antwort einfügen.

## <a name="azure-diagnostics-and-telemetry"></a>Azure-Diagnose und Telemetrie
Die oben beschriebenen Muster eignen sich gut zum Behandeln von Fehlern und Ausnahmen in einer Ausführung, aber Sie können auch unabhängig von der eigentlichen Ausführung Fehler identifizieren und darauf reagieren.  [Azure-Diagnose](app-service-logic-monitor-your-logic-apps.md) ist eine einfache Möglichkeit zum Senden aller Workflowereignisse (einschließlich aller Ausführungs- und Aktionsstatus) an ein Azure Storage-Konto oder einen Azure Event Hub.  Sie können die Protokolle und Metriken überwachen oder mit einem von Ihnen bevorzugten Überwachungstool veröffentlichen, um jeweils den Ausführungsstatus auszuwerten.  Eine potenzielle Option ist das Streamen aller Ereignisse über den Azure Event Hub in [Stream Analytics](https://azure.microsoft.com/services/stream-analytics/).  In Stream Analytics können Sie Liveabfragen für Anomalien, Mittelwerte oder Fehler aus den Diagnoseprotokollen schreiben.  Für Stream Analytics ist die Ausgabe in andere Datenquellen wie Warteschlangen, Themen, SQL, DocumentDB und Power BI problemlos möglich.

## <a name="next-steps"></a>Nächste Schritte
* [Protokollierung und Fehlerbehandlung in Logik-Apps (Beispiel eines Kunden)](app-service-logic-scenario-error-and-exception-handling.md)
* [Beispiele und häufige Szenarios für Logik-Apps](app-service-logic-examples-and-scenarios.md)
* [Erstellen einer Bereitstellungsvorlage für Logik-Apps](app-service-logic-create-deploy-template.md)
* [Erstellen und Bereitstellen von Logik-Apps in Visual Studio](app-service-logic-deploy-from-vs.md)

<!-- References -->
[retryPolicyMSDN]: https://msdn.microsoft.com/library/azure/mt643939.aspx#Anchor_9



<!--HONumber=Nov16_HO3-->


