<properties 
	pageTitle="Erstellen von Logik-App-Definitionen | Microsoft Azure" 
	description="Erfahren Sie, wie die JSON-Definition für Logik-Apps geschrieben wird." 
	authors="jeffhollan" 
	manager="erikre" 
	editor="" 
	services="logic-apps" 
	documentationCenter=""/>

<tags
	ms.service="logic-apps"
	ms.workload="integration"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/25/2016"
	ms.author="jehollan"/>
	
# Erstellen von Logik-App-Definitionen
In diesem Thema wird die Verwendung von Definitionen für [Azure Logic Apps](app-service-logic-what-are-logic-apps.md) erläutert. Diese sind in einer einfachen, deklarativen JSON-Sprache verfasst. Machen Sie sich zunächst [mit dem Erstellen neuer Logik-Apps](app-service-logic-create-a-logic-app.md) vertraut, falls Sie dies noch nicht getan haben. Sie können auch [das umfassende MSDN-Referenzmaterial zur Definitionssprache](http://aka.ms/logicappsdocs) lesen.

## Wiederholt ausgeführte Schritte in einer Liste

Nutzen Sie den [Foreach-Typ](app-service-logic-loops-and-scopes.md), um ein Array für bis zu 10.000 Elemente zu wiederholen und für jedes eine Aktion auszuführen.

## Ein Schritt zur Fehlerbehandlung, falls Fehler auftreten

Üblicherweise möchten Sie einen *Korrekturschritt* schreiben können – Logik, die **ausschließlich dann ausgeführt wird**, wenn einer oder mehrere Ihrer Aufrufe fehlschlagen. In diesem Beispiel rufen wir Daten von unterschiedlichen Stellen ab, aber wenn der Aufruf fehlschlägt, soll irgendwo eine Nachricht hinterlassen werden, um den Fehler später ermitteln zu können:

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

Sie können mit der Eigenschaft `runAfter` festlegen, dass `postToErrorMessageQueue` erst ausgeführt wird, wenn `readData` als **fehlerhaft** gekennzeichnet wird. Dies könnte auch eine Liste von möglichen Werten sein. `runAfter` könnte `["Succeeded", "Failed"]` lauten.

Nachdem Sie den Fehler nun behoben haben, wird der Lauf nicht länger als **fehlerhaft** gekennzeichnet. Wie Sie hier sehen können, ist dieser Lauf als **Erfolgreich** gekennzeichnet, obwohl ein Schritt fehlgeschlagen ist, da ich den Schritt zur Behebung dieses Fehlers geschrieben habe.

## Zwei (oder mehr) Schritte, die parallel ausgeführt werden

Um mehrere Aktionen parallel auszuführen, muss die Eigenschaft `runAfter` der Laufzeit entsprechen.

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

Wie Sie im obigen Beispiel sehen können, sollen `branch1` und `branch2` nach `readData` ausgeführt werden. Daher werden beide dieser Verzweigungen parallel ausgeführt:

![Parallel](./media/app-service-logic-author-definitions/parallel.png)

Wie Sie sehen, ist der Zeitstempel für die beiden Verzweigungen identisch.

## Verknüpfen von zwei parallelen Verzweigungen

Sie können zwei Aktionen, die parallel ausgeführt werden sollen, verknüpfen, indem Sie der Eigenschaft `runAfter` wie oben gezeigt Elemente hinzufügen.

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

![Parallel](./media/app-service-logic-author-definitions/join.png)

## Zuordnen von Elementen in einer Liste zu einer anderen Konfiguration

Nehmen wir als Nächstes an, dass abhängig vom Wert einer Eigenschaft völlig unterschiedliche Inhalte abgerufen werden sollen. Wir können eine Zuordnung von Werten zu Zielen als Parameter erstellen:

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
				"microsoft": "https://www.microsoft.com/de-DE/default.aspx",
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

In diesem Fall rufen wir zunächst eine Liste von Artikeln ab. Im zweiten Schritt wird dann in einer Zuordnung abgefragt, von welcher URL der Inhalt abgerufen werden soll, und zwar basierend auf der Kategorie, die als Parameter definiert wurde.

Hierbei sollten Sie auf zwei Dinge achten: Erstens dient die [`intersection()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#intersection)-Funktion dazu, zu überprüfen, ob die Kategorie mit einer der bekannten definierten Kategorien übereinstimmt. Zweitens können wir, sobald wir die Kategorie haben, das Element mithilfe eckiger Klammern aus der Zuordnung abrufen: `parameters[...]`.

## Arbeiten mit Zeichenfolgen

Es gibt verschiedene Funktionen, die zum Ändern von Zeichenfolgen verwendet werden können. Sehen wir uns ein Beispiel mit einer Zeichenfolge an, die an ein System übergeben werden soll. In diesem Fall sind wir aber nicht sicher, ob die Zeichencodierung ordnungsgemäß verarbeitet wird. Eine Möglichkeit besteht darin, diese Zeichenfolge mit "base64" zu codieren. Allerdings werden zur Vermeidung von Escapezeichen in einer URL einige Zeichen ersetzt.

Wir wollen außerdem eine Teilzeichenfolge des Namens des Auftrag, da die ersten fünf Zeichen nicht verwendet werden.

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

Gehen Sie von innen nach außen vor:

1. Rufen Sie die [`length()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#length) des Namens des Auftraggebers ab. Damit wird die Gesamtanzahl der Zeichen zurückgegeben.

2. Ziehen Sie fünf ab (da wir eine kürzere Zeichenfolge wollen).

3. Verwenden Sie die [`substring()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#substring) . Beginnen Sie bei Index `5`, und fahren Sie mit dem Rest der Zeichenfolge fort.

4. Wandeln Sie diese Teilzeichenfolge in eine [`base64()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#base64)-Zeichenfolge um.

5. [`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace) Sie alle der `+`-Zeichen durch `-`.

6. [`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace) Sie alle der `/`-Zeichen durch `_`.

## Arbeiten mit Zeitangaben

Zeitangaben sind insbesondere dann nützlich, wenn Sie versuchen, Daten aus einer Datenquelle abzurufen, die **Trigger** grundsätzlich nicht unterstützt. Sie können Zeitangaben auch verwenden, um zu ermitteln, wie lange die einzelnen Schritte dauern.

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
		"manual": {
			"type": "manual"
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
		"timingWarning": {
			"actions" {
				"type": "Http",
				"inputs": {
					"method": "GET",
					"uri": "http://www.example.com/?recordLongOrderTime=@{parameters('order').id}&currentTime=@{utcNow('r')}"
				},
				"runAfter": {}
			}
			"expression": "@less(actions('order').startTime,addseconds(utcNow(),-1))"
		}
	},
	"outputs": {}
}
```

In diesem Fall extrahieren wir die `startTime` des vorherigen Schritts. Dann rufen wir die aktuelle Uhrzeit ab und ziehen eine Sekunde ab :[`addseconds(..., -1)`](https://msdn.microsoft.com/library/azure/mt643789.aspx#addseconds) (Sie können auch andere Zeiteinheiten als `minutes` oder `hours` verwenden). Abschließend können wir diese beiden Werte vergleichen. Wenn der erste kleiner ist als der zweite, bedeutet dies, dass mehr als eine Sekunde verstrichen ist, seit der Auftrag erteilt wurde.

Beachten Sie außerdem, dass Zeichenfolgenformatierer zum Formatieren von Datumsangaben verwendet werden können: In der Abfragezeichenfolge verwende ich [`utcnow('r')`](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow), um das RFC1123-Format zu erhalten. Alle Datumsformate [sind im MSDN dokumentiert](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow).

## Verwenden von Parametern zur Bereitstellungszeit für unterschiedliche Umgebungen

Üblicherweise verfügen Sie über einen Bereitstellungslebenszyklus mit einer Entwicklungsumgebung, einer Stagingumgebung und einer Produktionsumgebung. In all diesen Umgebungen können Sie dieselbe Definition, aber beispielsweise unterschiedliche Datenbanken verwenden. Ebenso sollten Sie dieselbe Definition übergreifend in vielen verschiedenen Regionen verwenden, um eine hohe Verfügbarkeit zu erzielen. Die einzelnen Logik-App-Instanzen sollten jedoch mit der Datenbank der betreffenden Region kommunizieren.

Beachten Sie, dass sich dies von der Verwendung anderer Parameter zur *Laufzeit* unterscheidet. Dazu sollten Sie die `trigger()`-Funktion verwenden, wie oben beschrieben.

Sie können mit einer sehr einfachen Definition wie dieser beginnen:

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

In der tatsächlichen `PUT`-Anforderung für die Logik-App können Sie dann den `uri`-Parameter angeben. Da es keinen Standardwert mehr gibt, ist dieser Parameter für die Logik-App-Nutzlast erforderlich:

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

In den einzelnen Umgebungen können Sie dann einen anderen Wert für den `connection`-Parameter angeben.

In der [REST-API-Dokumentation](https://msdn.microsoft.com/library/azure/mt643787.aspx) finden Sie Informationen dazu, welche Möglichkeiten Sie zum Erstellen und Verwalten von Logik-Apps haben.

<!---HONumber=AcomDC_0803_2016-->