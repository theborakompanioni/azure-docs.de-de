---
title: "Definieren von Workflows per JSON-Code – Azure Logic Apps | Microsoft-Dokumentation"
description: "Es wird beschrieben, wie Sie Workflowdefinitionen per JSON-Code für Logik-Apps schreiben."
author: jeffhollan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: d565873c-6b1b-4057-9250-cf81a96180ae
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 07/25/2016
ms.author: jehollan
translationtype: Human Translation
ms.sourcegitcommit: e94837bf79e42602e2f72cda747ea629eed45a20
ms.openlocfilehash: 920940d8ebe23d24216d3e886bd8ae58be12ce34
ms.lasthandoff: 03/01/2017


---
# <a name="create-workflow-definitions-for-logic-apps-using-json"></a>Erstellen von Workflowdefinitionen für Logik-Apps per JSON-Code

Sie können Workflowdefinitionen für [Azure Logic Apps](logic-apps-what-are-logic-apps.md) erstellen, indem Sie die einfache, deklarative JSON-Sprache verwenden. Falls Sie dies noch nicht getan haben, sollten Sie sich zuerst darüber informieren, wie Sie [Ihre erste Logik-App mit dem Logik-App-Designer erstellen](logic-apps-create-a-logic-app.md). Sehen Sie sich auch die [vollständige Referenz zur Definitionssprache für Workflows](http://aka.ms/logicappsdocs) an.

## <a name="repeat-steps-over-a-list"></a>Wiederholen von Schritten für eine Liste

Verwenden Sie den [foreach-Typ](logic-apps-loops-and-scopes.md), um ein Array zu durchlaufen, das über bis zu 10.000 Elemente verfügt, und für jedes Element eine Aktion durchzuführen.

## <a name="handle-failures-if-something-goes-wrong"></a>Behandeln von Fehlern

Normalerweise ist es ratsam, einen *Korrekturschritt* einzubauen – Logik, die *ausschließlich dann ausgeführt wird*, wenn einer oder mehrere Ihrer Aufrufe fehlschlagen. In diesem Beispiel werden Daten von verschiedenen Orten abgerufen. Falls der Aufruf nicht erfolgreich ist, soll eine Nachricht hinterlegt werden (per POST), damit wir den Fehler später nachverfolgen können:  

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
    },
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "readData": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            }
        },
        "postToErrorMessageQueue": {
            "type": "ApiConnection",
            "inputs": "...",
            "runAfter": {
                "readData": ["Failed"]
            }
        }
    },
    "outputs": {}
}
```

Um anzugeben, dass `postToErrorMessageQueue` erst nach dem Eintreten von `Failed` für `readData` ausgeführt wird, verwenden Sie die `runAfter`-Eigenschaft. Beispielsweise können Sie eine Liste mit möglichen Werten angeben, sodass `runAfter` auf `["Succeeded", "Failed"]` festgelegt werden kann.

Da der Fehler in diesem Beispiel jetzt behandelt wird, kennzeichnen wir die Ausführung nicht mehr als `Failed`. Weil wir den Schritt zum Behandeln des Fehlers in diesem Beispiel hinzugefügt haben, gilt für die Ausführung `Succeeded`, auch wenn ein Schritt fehlgeschlagen ist (`Failed`).

## <a name="execute-two-or-more-steps-in-parallel"></a>Gleichzeitiges Ausführen von zwei oder mehr Schritten

Um mehrere Aktionen parallel auszuführen, muss die `runAfter`-Eigenschaft zur Laufzeit gleich festgelegt sein. 

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "readData": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            }
        },
        "branch1": {
            "type": "Http",
            "inputs": "...",
            "runAfter": {
                "readData": ["Succeeded"]
            }
        },
        "branch2": {
            "type": "Http",
            "inputs": "...",
            "runAfter": {
                "readData": ["Succeeded"]
            }
        }
    },
    "outputs": {}
}
```

In diesem Beispiel ist für `branch1` und `branch2` die Ausführung nach `readData` festgelegt. Beide Verzweigungen werden also parallel ausgeführt. Der Zeitstempel für die beiden Verzweigungen ist identisch.

![Parallel](media/logic-apps-author-definitions/parallel.png)

## <a name="join-two-parallel-branches"></a>Verknüpfen von zwei parallelen Verzweigungen

Sie können zwei Aktionen verknüpfen, die parallel ausgeführt werden sollen, indem Sie der `runAfter`-Eigenschaft wie im vorherigen Beispiel Elemente hinzufügen.

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-04-01-preview/workflowdefinition.json#",
    "actions": {
        "readData": {
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {},
            "type": "Http"
        },
        "branch1": {
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {
                "readData": [
                    "Succeeded"
                ]
            },
            "type": "Http"
        },
        "branch2": {
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {
                "readData": [
                    "Succeeded"
                ]
            },
            "type": "Http"
        },
        "join": {
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {
                "branch1": [
                    "Succeeded"
                ],
                "branch2": [
                    "Succeeded"
                ]
            },
            "type": "Http"
        }
    },
    "contentVersion": "1.0.0.0",
    "outputs": {},
    "parameters": {},
    "triggers": {
        "manual": {
            "inputs": {
                "schema": {}
            },
            "kind": "Http",
            "type": "Request"
        }
    }
}
```

![Parallel](media/logic-apps-author-definitions/join.png)

## <a name="map-list-items-to-a-different-configuration"></a>Zuordnen von Listenelementen zu einer anderen Konfiguration

Als Nächstes nehmen wir an, dass wir basierend auf dem Wert einer Eigenschaft unterschiedlichen Inhalt abrufen möchten. Wir können eine Zuordnung von Werten zu Zielen als Parameter erstellen:  

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "specialCategories": {
            "defaultValue": ["science", "google", "microsoft", "robots", "NSA"],
            "type": "Array"
        },
        "destinationMap": {
            "defaultValue": {
                "science": "http://www.nasa.gov",
                "microsoft": "https://www.microsoft.com/en-us/default.aspx",
                "google": "https://www.google.com",
                "robots": "https://en.wikipedia.org/wiki/Robot",
                "NSA": "https://www.nsa.gov/"
            },
            "type": "Object"
        }
    },
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "getArticles": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "https://ajax.googleapis.com/ajax/services/feed/load?v=1.0&q=http://feeds.wired.com/wired/index"
            },
            "conditions": []
        },
        "getSpecialPage": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "@parameters('destinationMap')[first(intersection(item().categories, parameters('specialCategories')))]"
            },
            "conditions": [{
                "expression": "@greater(length(intersection(item().categories, parameters('specialCategories'))), 0)"
            }],
            "forEach": "@body('getArticles').responseData.feed.entries"
        }
    }
}
```

In diesem Fall rufen wir zuerst eine Liste mit Artikeln ab. Basierend auf der Kategorie, die als Parameter definiert wurde, wird im zweiten Schritt eine Zuordnung verwendet, um die URL zum Abrufen des Inhalts zu ermitteln.

Beachten Sie hierbei Folgendes: 

*    Mit der Funktion [`intersection()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#intersection) wird überprüft, ob die Kategorie einer bekannten definierten Kategorie entspricht.

*    Nach dem Erhalt der Kategorie können wir das Element mithilfe von eckigen Klammern aus der Zuordnung abrufen: `parameters[...]`.

## <a name="process-strings"></a>Verarbeiten von Zeichenfolgen

Sie können verschiedene Funktionen verwenden, um Zeichenfolgen zu bearbeiten. Angenommen, wir möchten eine Zeichenfolge an ein System übergeben, sind aber nicht sicher, ob die Zeichencodierung richtig durchgeführt wird. Eine Möglichkeit besteht darin, diese Zeichenfolge mit "base64" zu codieren. Allerdings werden zur Vermeidung von Escapezeichen in einer URL einige Zeichen ersetzt. 

Außerdem möchten wir eine Teilzeichenfolge des Auftragsnamens verwenden, da die ersten fünf Zeichen nicht verwendet werden.

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "order": {
            "defaultValue": {
                "quantity": 10,
                "id": "myorder1",
                "orderer": "NAME=Stèphén__Šīçiłianö"
            },
            "type": "Object"
        }
    },
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "order": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://www.example.com/?id=@{replace(replace(base64(substring(parameters('order').orderer,5,sub(length(parameters('order').orderer), 5) )),'+','-') ,'/' ,'_' )}"
            }
        }
    },
    "outputs": {}
}
```

Wir gehen von innen nach außen vor:

1. Rufen Sie [`length()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#length) für den Namen des Auftraggebers ab, damit wir die Gesamtzahl von Zeichen erhalten.

2. Ziehen Sie fünf ab, da wir eine kürzere Zeichenfolge verwenden möchten.

3. Verwenden Sie das [`substring()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#substring)-Element. Beginnen Sie bei Index `5` , und fahren Sie mit dem Rest der Zeichenfolge fort.

4. Wandeln Sie diese Teilzeichenfolge in eine [`base64()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#base64)-Zeichenfolge um.

5. Ersetzen Sie mit [`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace) alle `+`-Zeichen durch `-`-Zeichen.

6. Ersetzen Sie mit [`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace) alle `/`-Zeichen durch `_`-Zeichen.

## <a name="work-with-date-times"></a>Verwenden von Zeitangaben

Zeitangaben sind insbesondere dann nützlich, wenn Sie versuchen, Daten aus einer Datenquelle abzurufen, die *Trigger* standardmäßig nicht unterstützt. Sie können Zeitangaben auch verwenden, um zu ermitteln, wie lange die einzelnen Schritte dauern.

```
{
  "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "order": {
      "defaultValue": {
        "quantity": 10,
        "id": "myorder1"
      },
      "type": "Object"
    }
  },
  "triggers": {
    "Request": {
      "type": "request",
      "kind": "http"
    }
  },
  "actions": {
    "order": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://www.example.com/?id=@{parameters('order').id}"
      }
    },
    "ifTimingWarning": {
      "type": "If",
      "expression": "@less(actions('order').startTime,addseconds(utcNow(),-1))",
      "actions": {
        "timingWarning": {
          "type": "Http",
          "inputs": {
            "method": "GET",
            "uri": "http://www.example.com/?recordLongOrderTime=@{parameters('order').id}&currentTime=@{utcNow('r')}"
          }
        }
      },
      "runAfter": {
        "order": [
          "Succeeded"
        ]
      }
    }
  },
  "outputs": {}
}
```

In diesem Beispiel extrahieren wir den `startTime`-Wert aus dem vorherigen Schritt. Anschließend rufen wir die aktuelle Uhrzeit ab und subtrahieren eine Sekunde:

[`addseconds(..., -1)`](https://msdn.microsoft.com/library/azure/mt643789.aspx#addseconds) 

Sie können auch andere Zeiteinheiten verwenden, z.B. `minutes` oder `hours`. Abschließend können wir diese beiden Werte vergleichen. Wenn der erste Wert kleiner als der zweite Wert ist, bedeutet dies, dass mehr als eine Sekunde verstrichen ist, seit der Auftrag erteilt wurde.

Zum Formatieren von Datumsangaben können wir Zeichenfolgenformatierer verwenden. Zum Abrufen von RFC1123 verwenden wir beispielsweise [`utcnow('r')`](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow). Informationen zur Datumsformatierung finden Sie im Artikel zur [Workflow Definition Language](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow) (Definitionssprache für Workflows).

## <a name="deployment-parameters-for-different-environments"></a>Bereitstellungsparameter für unterschiedliche Umgebungen

Bereitstellungslebenszyklen verfügen normalerweise über eine Entwicklungsumgebung, eine Stagingumgebung und eine Produktionsumgebung. In all diesen Umgebungen können Sie beispielsweise die gleiche Definition, aber unterschiedliche Datenbanken verwenden. Ebenso können Sie die gleiche Definition übergreifend in verschiedenen Regionen verwenden, um eine hohe Verfügbarkeit zu erzielen. Die einzelnen Logik-App-Instanzen sollten aber mit der Datenbank der betreffenden Region kommunizieren.
Dieses Szenario unterscheidet sich von der Verwendung von Parametern zur *Laufzeit*, wo Sie stattdessen wie im vorherigen Beispiel die Funktion `trigger()` verwenden sollten.

Sie können mit einer einfachen Definition wie im folgenden Beispiel beginnen:

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "uri": {
            "type": "string"
        }
    },
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "readData": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "@parameters('uri')"
            }
        }
    },
    "outputs": {}
}
```

In der tatsächlichen `PUT`-Anforderung für die Logik-App können Sie den Parameter `uri` angeben. Da kein Standardwert mehr vorhanden ist, ist für die Nutzlast der Logik-App dieser Parameter erforderlich:

```
{
    "properties": {},
        "definition": {
          // Use the definition from above here
        },
        "parameters": {
            "connection": {
                "value": "https://my.connection.that.is.per.enviornment"
            }
        }
    },
    "location": "westus"
}
``` 

In den einzelnen Umgebungen können Sie einen anderen Wert für den `connection`-Parameter angeben. 

In der [REST-API-Dokumentation](https://msdn.microsoft.com/library/azure/mt643787.aspx) finden Sie Informationen dazu, welche Möglichkeiten Sie zum Erstellen und Verwalten von Logik-Apps haben. 

