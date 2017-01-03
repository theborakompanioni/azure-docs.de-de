---
title: Vorlagenfunktionen im Resource Manager | Microsoft Docs
description: Es werden die Funktionen beschrieben, die in einer Azure-Ressourcen-Manager-Vorlage zum Abrufen von Werten, Arbeiten mit Zeichenfolgen und numerischen Werten sowie Abrufen von Bereitstellungsinformationen verwendet werden.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 0644abe1-abaa-443d-820d-1966d7d26bfd
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/22/2016
ms.author: tomfitz
translationtype: Human Translation
ms.sourcegitcommit: 33e6b9ba880f56d967b49d0b89e61d1b531e8376
ms.openlocfilehash: 1d8cb6894399a7863392a7f11bde69d75d4685c3


---
# <a name="azure-resource-manager-template-functions"></a>Vorlagenfunktionen im Azure-Ressourcen-Manager
In diesem Thema werden alle Funktionen beschrieben, die Sie in einer Azure Resource Manager-Vorlage verwenden können.

Bei Vorlagenfunktionen und ihren Parametern wird Groß-und Kleinschreibung nicht unterschieden. Der Resource Manager löst beispielsweise **variables('var1')** und **VARIABLES('VAR1')** identisch auf. Bei der Auswertung wird die Groß-/Kleinschreibung beibehalten, sofern diese nicht ausdrücklich durch die Funktion geändert wird (z.B. mit „toUpper“ oder „toLower“). Für spezielle Ressourcentypen gelten möglicherweise Vorgaben zur Schreibweise ungeachtet der Auswertungsweise von Funktionen.

## <a name="numeric-functions"></a>Numerische Funktionen
Der Ressourcen-Manager stellt die folgenden Funktionen für das Arbeiten mit ganzen Zahlen bereit:

* [Hinzufügen](#add)
* [copyIndex](#copyindex)
* [div](#div)
* [int](#int)
* [mod](#mod)
* [mul](#mul)
* [sub](#sub)

<a id="add" />

### <a name="add"></a>Hinzufügen
`add(operand1, operand2)`

Gibt die Summe der beiden angegebenen ganzen Zahlen zurück.

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- | 
|operand1 |Ja |Integer |Erste zu addierende Zahl. |
|operand2 |Ja |Integer |Zweite zu addierende Zahl. |

Im folgenden Beispiel werden zwei Parameter hinzugefügt.

    "parameters": {
      "first": {
        "type": "int",
        "metadata": {
          "description": "First integer to add"
        }
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Second integer to add"
        }
      }
    },
    ...
    "outputs": {
      "addResult": {
        "type": "int",
        "value": "[add(parameters('first'), parameters('second'))]"
      }
    }

<a id="copyindex" />

### <a name="copyindex"></a>copyIndex
`copyIndex(offset)`

Gibt den Index einer Iterationsschleife zurück. 

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| offset |Nein |Integer |Die Zahl, die dem nullbasierten (0) Iterationswert hinzugefügt werden soll. |

Diese Funktion wird immer mit einem **copy** -Objekt verwendet. Wenn kein Wert für **offset** angegeben wird, wird der aktuelle Iterationswert zurückgegeben. Der Iterationswert beginnt bei 0 (null). Eine vollständige Beschreibung der Nutzung von **copyIndex**finden Sie unter [Erstellen mehrerer Instanzen von Ressourcen im Azure-Ressourcen-Manager](resource-group-create-multiple.md).

Das folgende Beispiel enthält eine Kopierschleife und den Indexwert im Namen. 

    "resources": [ 
      { 
        "name": "[concat('examplecopy-', copyIndex())]", 
        "type": "Microsoft.Web/sites", 
        "copy": { 
          "name": "websitescopy", 
          "count": "[parameters('count')]" 
        }, 
        ...
      }
    ]


<a id="div" />

### <a name="div"></a>div
`div(operand1, operand2)`

Gibt die Ganzzahldivision der beiden angegebenen ganzen Zahlen zurück.

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| operand1 |Ja |Integer |Die zu teilende Zahl (Dividend). |
| operand2 |Ja |Integer |Die Zahl, durch die geteilt wird (Divisor). Diese Zahl darf nicht 0 sein. |

Im folgenden Beispiel wird ein Parameter durch einen anderen Parameter dividiert.

    "parameters": {
      "first": {
        "type": "int",
        "metadata": {
          "description": "Integer being divided"
        }
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Integer used to divide"
        }
      }
    },
    ...
    "outputs": {
      "divResult": {
        "type": "int",
        "value": "[div(parameters('first'), parameters('second'))]"
      }
    }

<a id="int" />

### <a name="int"></a>int
`int(valueToConvert)`

Konvertiert den angegebenen Wert in eine ganze Zahl (Integer).

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| valueToConvert |Ja |String oder Integer |Der Wert, der in eine ganze Zahl (Integer) konvertiert werden soll. |

Im folgenden Beispiel wird der vom Benutzer angegebene Parameterwert in eine ganze Zahl konvertiert.

    "parameters": {
        "appId": { "type": "string" }
    },
    "variables": { 
        "intValue": "[int(parameters('appId'))]"
    }


<a id="mod" />

### <a name="mod"></a>mod
`mod(operand1, operand2)`

Gibt den Rest der Ganzzahldivision mit den beiden angegebenen ganzen Zahlen zurück.

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| operand1 |Ja |Integer |Die zu teilende Zahl (Dividend). |
| operand2 |Ja |Integer |Die Zahl, durch die dividiert wird (Divisor), darf nicht null (0) sein. |

Im folgenden Beispiel wird der Restbetrag der Division von einem Parameter durch einen anderen Parameter zurückgegeben.

    "parameters": {
      "first": {
        "type": "int",
        "metadata": {
          "description": "Integer being divided"
        }
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Integer used to divide"
        }
      }
    },
    ...
    "outputs": {
      "modResult": {
        "type": "int",
        "value": "[mod(parameters('first'), parameters('second'))]"
      }
    }

<a id="mul" />

### <a name="mul"></a>mul
`mul(operand1, operand2)`

Gibt die Multiplikation der beiden angegebenen ganzen Zahlen zurück.

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| operand1 |Ja |Integer |Erste zu multiplizierende Zahl. |
| operand2 |Ja |Integer |Zweite zu multiplizierende Zahl. |

Im folgenden Beispiel wird ein Parameter mit einem anderen Parameter multipliziert.

    "parameters": {
      "first": {
        "type": "int",
        "metadata": {
          "description": "First integer to multiply"
        }
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Second integer to multiply"
        }
      }
    },
    ...
    "outputs": {
      "mulResult": {
        "type": "int",
        "value": "[mul(parameters('first'), parameters('second'))]"
      }
    }

<a id="sub" />

### <a name="sub"></a>sub
`sub(operand1, operand2)`

Gibt die Differenz der beiden angegebenen ganzen Zahlen zurück.

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| operand1 |Ja |Integer |Zahl, von der subtrahiert wird. |
| operand2 |Ja |Integer |Zahl, die subtrahiert wird. |

Im folgenden Beispiel wird ein Parameter von einem anderen Parameter subtrahiert.

    "parameters": {
      "first": {
        "type": "int",
        "metadata": {
          "description": "Integer subtracted from"
        }
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Integer to subtract"
        }
      }
    },
    ...
    "outputs": {
      "subResult": {
        "type": "int",
        "value": "[sub(parameters('first'), parameters('second'))]"
      }
    }

## <a name="string-functions"></a>Zeichenfolgenfunktionen
Der Ressourcen-Manager stellt die folgenden Funktionen für das Arbeiten mit Zeichenfolgen bereit:

* [base64](#base64)
* [concat](#concat)
* [Länge](#lengthstring)
* [padLeft](#padleft)
* [replace](#replace)
* [skip](#skipstring)
* [split](#split)
* [string](#string)
* [substring](#substring)
* [take](#takestring)
* [toLower](#tolower)
* [toUpper](#toupper)
* [trim](#trim)
* [uniqueString](#uniquestring)
* [uri](#uri)

<a id="base64" />

### <a name="base64"></a>base64
`base64 (inputString)`

Rückkehr zur base64-Darstellung der Eingabezeichenfolge.

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| inputString |Ja |String |Der Wert, der als base64-Darstellung zurückgegeben wird. |

Das folgende Beispiel zeigt die Funktionsweise der base64-Funktion.

    "variables": {
      "usernameAndPassword": "[concat(parameters('username'), ':', parameters('password'))]",
      "authorizationHeader": "[concat('Basic ', base64(variables('usernameAndPassword')))]"
    }

<a id="concat" />

### <a name="concat---string"></a>concat – Zeichenfolge
`concat (string1, string2, string3, ...)`

Kombiniert mehrere Zeichenfolgenwerte und gibt die verkettete Zeichenfolge zurück. 

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| string1 |Ja |String |Der erste Wert für die Verkettung. |
| Weitere Zeichenfolgen |Nein |String |Weitere Werte in sequenzieller Reihenfolge für die Verkettung. |

Diese Funktion akzeptiert eine beliebige Anzahl von Argumenten und Zeichenfolgen oder Arrays für die Parameter. Ein Beispiel für die Verkettung von Arrays finden Sie unter [concat – Array](#concatarray).

Das folgende Beispiel zeigt, wie mehrere Zeichenfolgenwerte kombiniert werden, um eine verkettete Zeichenfolge zu erhalten.

    "outputs": {
        "siteUri": {
          "type": "string",
          "value": "[concat('http://', reference(resourceId('Microsoft.Web/sites', parameters('siteName'))).hostNames[0])]"
        }
    }


<a id="lengthstring" />

### <a name="length---string"></a>length – Zeichenfolge
`length(string)`

Gibt die Anzahl von Zeichen in einer Zeichenfolge zurück.

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| string |Ja |String |Der Wert, der zum Abrufen der Anzahl von Zeichen verwendet werden soll. |

Ein Beispiel für die Verwendung von „length“ mit einem Array finden Sie unter [length – Array](#length).

Im folgenden Beispiel wird die Anzahl von Zeichen in einer Zeichenfolge zurückgegeben. 

    "parameters": {
        "appName": { "type": "string" }
    },
    "variables": { 
        "nameLength": "[length(parameters('appName'))]"
    }


<a id="padleft" />

### <a name="padleft"></a>padLeft
`padLeft(valueToPad, totalLength, paddingCharacter)`

Gibt eine rechtsbündig ausgerichtete Zeichenfolge zurück, indem links Zeichen hinzugefügt werden, bis die angegebene Gesamtlänge erreicht ist.

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| valueToPad |Ja |String oder Integer |Der Wert, der rechtsbündig ausgerichtet werden soll. |
| totalLength |Ja |Integer |Die Gesamtzahl der Zeichen in der zurückgegebenen Zeichenfolge. |
| paddingCharacter |Nein |Ein einzelnes Zeichen |Das Zeichen, das für das Auffüllen auf der linken Seite verwendet werden soll, bis die Gesamtlänge erreicht ist. Der Standardwert ist ein Leerzeichen. |

Im folgenden Beispiel wird veranschaulicht, wie Sie den vom Benutzer angegebenen Parameterwert auffüllen, indem Sie das Nullzeichen hinzufügen, bis die Zeichenfolge zehn Zeichen lang ist. Wenn der ursprüngliche Parameterwert länger als zehn Zeichen ist, werden keine Zeichen hinzugefügt.

    "parameters": {
        "appName": { "type": "string" }
    },
    "variables": { 
        "paddedAppName": "[padLeft(parameters('appName'),10,'0')]"
    }

<a id="replace" />

### <a name="replace"></a>replace
`replace(originalString, oldCharacter, newCharacter)`

Gibt eine neue Zeichenfolge zurück, in der alle Instanzen eines Zeichens in der angegebenen Zeichenfolge durch ein anderes Zeichen ersetzt wurden.

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| originalString |Ja |String |Der Wert, für den alle Instanzen eines Zeichens durch ein anderes Zeichen ersetzt werden sollen. |
| oldCharacter |Ja |String |Das Zeichen, das aus der ursprünglichen Zeichenfolge entfernt werden soll. |
| newCharacter |Ja |String |Das Zeichen, das für das entfernte Zeichen eingefügt werden soll. |

Im folgenden Beispiel wird veranschaulicht, wie Sie alle Bindestriche aus einer vom Benutzer angegebenen Zeichenfolge entfernen.

    "parameters": {
        "identifier": { "type": "string" }
    },
    "variables": { 
        "newidentifier": "[replace(parameters('identifier'),'-','')]"
    }

<a id="skipstring" />

### <a name="skip---string"></a>skip – Zeichenfolge
`skip(originalValue, numberToSkip)`

Gibt eine Zeichenfolge mit allen Zeichen nach der angegebenen Anzahl von Stellen in der Zeichenfolge zurück.

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| originalValue |Ja |String |Die Zeichenfolge, die für das Überspringen verwendet werden soll. |
| numberToSkip |Ja |Integer |Die Anzahl von zu überspringenden Zeichen. Wenn dieser Wert 0 oder kleiner ist, werden alle Zeichen der Zeichenfolge zurückgegeben. Ist der Wert größer als die Länge der Zeichenfolge, wird eine leere Zeichenfolge zurückgegeben. |

Ein Beispiel für die Verwendung von „skip“ mit einem Array finden Sie unter [skip – Array](#skip).

Im folgenden Beispiel wird die angegebene Anzahl von Zeichen in der Zeichenfolge übersprungen.

    "parameters": {
      "first": {
        "type": "string",
        "metadata": {
          "description": "Value to use for skipping"
        }
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Number of characters to skip"
        }
      }
    },
    "resources": [
    ],
    "outputs": {
      "return": {
        "type": "string",
        "value": "[skip(parameters('first'),parameters('second'))]"
      }
    }


<a id="split" />

### <a name="split"></a>split
`split(inputString, delimiterString)`

`split(inputString, delimiterArray)`

Gibt ein Array mit Zeichenfolgen zurück, das die Teilzeichenfolgen der Eingabezeichenfolge getrennt durch die angegebenen Trennzeichen enthält.

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| inputString |Ja |String |Die zu teilende Zeichenfolge. |
| Trennzeichen |Ja |Zeichenfolge oder Array von Zeichenfolgen |Das Trennzeichen, das zum Teilen der Zeichenfolge verwendet werden soll. |

Im folgenden Beispiel wird die Eingabezeichenfolge durch ein Komma unterteilt.

    "parameters": {
        "inputString": { "type": "string" }
    },
    "variables": { 
        "stringPieces": "[split(parameters('inputString'), ',')]"
    }

Im nächsten Beispiel wird die Eingabezeichenfolge mit einem Komma oder Semikolon getrennt.

    "variables": {
      "stringToSplit": "test1,test2;test3",
      "delimiters": [ ",", ";" ]
    },
    "resources": [ ],
    "outputs": {
      "exampleOutput": {
        "value": "[split(variables('stringToSplit'), variables('delimiters'))]",
        "type": "array"
      }
    }

<a id="string" />

### <a name="string"></a>string
`string(valueToConvert)`

Konvertiert den angegebenen Wert in eine Zeichenfolge.

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| valueToConvert |Ja | Beliebig |Der Wert, der in eine Zeichenfolge konvertiert werden soll. Werte aller Typen können konvertiert werden, auch Objekte und Arrays. |

Im folgenden Beispiel werden die vom Benutzer angegebenen Parameterwerte in Zeichenfolgen konvertiert.

    "parameters": {
      "jsonObject": {
        "type": "object",
        "defaultValue": {
          "valueA": 10,
          "valueB": "Example Text"
        }
      },
      "jsonArray": {
        "type": "array",
        "defaultValue": [ "a", "b", "c" ]
      },
      "jsonInt": {
        "type": "int",
        "defaultValue": 5
      }
    },
    "variables": { 
      "objectString": "[string(parameters('jsonObject'))]",
      "arrayString": "[string(parameters('jsonArray'))]",
      "intString": "[string(parameters('jsonInt'))]"
    }

<a id="substring" />

### <a name="substring"></a>substring
`substring(stringToParse, startIndex, length)`

Gibt eine Teilzeichenfolge zurück, die an der angegebenen Zeichenposition beginnt und die angegebene Anzahl von Zeichen enthält.

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| stringToParse |Ja |String |Die ursprüngliche Zeichenfolge, aus der die Teilzeichenfolge extrahiert wird. |
| startIndex |Nein |Integer |Die nullbasierte Anfangsposition für die Teilzeichenfolge. |
| Länge |Nein |Integer |Die Anzahl der Zeichen der Teilzeichenfolge. |

Im folgenden Beispiel werden die ersten drei Zeichen aus einem Parameter extrahiert.

    "parameters": {
        "inputString": { "type": "string" }
    },
    "variables": { 
        "prefix": "[substring(parameters('inputString'), 0, 3)]"
    }

<a id="takestring" />

### <a name="take---string"></a>take – Zeichenfolge
`take(originalValue, numberToTake)`

Gibt eine Zeichenfolge mit der angegebenen Anzahl von Zeichen ab dem Anfang der Zeichenfolge zurück.

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| originalValue |Ja |String |Der Wert, aus dem die Zeichen entnommen werden. |
| numberToTake |Ja |Integer |Die Anzahl von zu entnehmenden Zeichen. Ist dieser Wert 0 oder kleiner, wird eine leere Zeichenfolge zurückgegeben. Wenn er größer als die Länge der jeweiligen Zeichenfolge ist, werden alle Zeichen der Zeichenfolge zurückgegeben. |

Ein Beispiel für die Verwendung mit einem Array finden Sie unter [take – Array](#take).

Im folgenden Beispiel wird die angegebene Anzahl von Zeichen aus der Zeichenfolge entnommen.

    "parameters": {
      "first": {
        "type": "string",
        "metadata": {
          "description": "Value to use for taking"
        }
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Number of characters to take"
        }
      }
    },
    "resources": [
    ],
    "outputs": {
      "return": {
        "type": "string",
        "value": "[take(parameters('first'), parameters('second'))]"
      }
    }

<a id="tolower" />

### <a name="tolower"></a>toLower
`toLower(stringToChange)`

Konvertiert die angegebene Zeichenfolge in Kleinbuchstaben.

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| stringToChange |Ja |String |Der Wert, der in Kleinbuchstaben konvertiert werden soll. |

Im folgenden Beispiel wird der vom Benutzer angegebene Parameterwert in Kleinbuchstaben konvertiert.

    "parameters": {
        "appName": { "type": "string" }
    },
    "variables": { 
        "lowerCaseAppName": "[toLower(parameters('appName'))]"
    }

<a id="toupper" />

### <a name="toupper"></a>toUpper
`toUpper(stringToChange)`

Konvertiert die angegebene Zeichenfolge in Großbuchstaben.

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| stringToChange |Ja |String |Der Wert, der in Großbuchstaben konvertiert werden soll. |

Im folgenden Beispiel wird der vom Benutzer angegebene Parameterwert in Großbuchstaben konvertiert.

    "parameters": {
        "appName": { "type": "string" }
    },
    "variables": { 
        "upperCaseAppName": "[toUpper(parameters('appName'))]"
    }

<a id="trim" />

### <a name="trim"></a>trim
`trim (stringToTrim)`

Entfernt alle führenden und nachgestellten Leerzeichen aus der angegebenen Zeichenfolge.

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| stringToTrim |Ja |String |Der zu kürzende Wert. |

Im folgenden Beispiel werden die Leerzeichen aus dem vom Benutzer bereitgestellten Parameterwert entfernt.

    "parameters": {
        "appName": { "type": "string" }
    },
    "variables": { 
        "trimAppName": "[trim(parameters('appName'))]"
    }

<a id="uniquestring" />

### <a name="uniquestring"></a>uniqueString
`uniqueString (baseString, ...)`

Erstellt auf der Grundlage der als Parameter angegebenen Werte eine deterministische Hashzeichenfolge. 

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| baseString |Ja |String |Der Wert, der in der Hashfunktion verwendet wird, um eine eindeutige Zeichenfolge zu erstellen. |
| Zusätzliche Parameter nach Bedarf. |Nein |String |Sie können beliebig viele Zeichenfolgen hinzufügen, ganz wie sie zum Erstellen des Werts benötigt werden, der die Ebene der Eindeutigkeit angibt. |

Diese Funktion ist hilfreich, wenn Sie einen eindeutigen Namen für eine Ressource erstellen müssen. Sie geben Parameterwerte an, die den Eindeutigkeitsbereich für das Ergebnis einschränken. Sie können angeben, ob der Name bis hinunter zum Abonnement, zur Ressourcengruppe oder zur Bereitstellung eindeutig ist. 

Der zurückgegebene Wert ist keine zufällige Zeichenfolge, sondern das Ergebnis einer Hashfunktion. Der zurückgegebene Wert ist 13 Zeichen lang. Er ist nicht global eindeutig. Es empfiehlt sich, den Wert mit einem Präfix aus Ihrer Benennungskonvention zu kombinieren, um einen aussagekräftigen Namen zu erstellen. Im folgenden Beispiel wird das Format des zurückgegebenen Werts veranschaulicht. Der tatsächliche Wert variiert je nach den angegebenen Parametern.

    tcvhiyu5h2o5o

Die folgenden Beispiele zeigen, wie Sie mithilfe von uniqueString einen eindeutigen Wert für häufig verwendete Ebenen erstellen können.

Eindeutige Zuordnung zum Abonnement

    "[uniqueString(subscription().subscriptionId)]"

Eindeutige Zuordnung zur Ressourcengruppe

    "[uniqueString(resourceGroup().id)]"

Eindeutige Zuordnung zur Bereitstellung für eine Ressourcengruppe

    "[uniqueString(resourceGroup().id, deployment().name)]"

Das folgende Beispiel zeigt, wie Sie einen eindeutigen Namen für ein Speicherkonto auf Grundlage seiner Ressourcengruppe erstellen. Innerhalb der Ressourcengruppe ist der Name nicht eindeutig, wenn er auf die gleiche Weise erstellt wird.

    "resources": [{ 
        "name": "[concat('storage', uniqueString(resourceGroup().id))]", 
        "type": "Microsoft.Storage/storageAccounts", 
        ...



<a id="uri" />

### <a name="uri"></a>uri
`uri (baseUri, relativeUri)`

Erstellt einen absoluten URI durch Kombinieren der baseUri- und der relativeUri-Zeichenfolge.

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| baseUri |Ja |String |Die Zeichenfolge mit dem Basis-URI. |
| relativeUri |Ja |String |Der Zeichenfolge mit dem relativen URI, die der Zeichenfolge mit dem Basis-URI hinzugefügt werden soll. |

Der Wert für den **baseUri** -Parameter kann eine bestimmte Datei enthalten, beim Erstellen des URI wird jedoch nur der Basispfad verwendet. Beispielsweise führt das Übergeben von `http://contoso.com/resources/azuredeploy.json` als baseUri-Parameter zu einem Basis-URI von `http://contoso.com/resources/`.

Im folgenden Beispiel wird veranschaulicht, wie basierend auf dem Wert der übergeordneten Vorlage eine Verknüpfung zu einer geschachtelten Vorlage erstellt wird.

    "templateLink": "[uri(deployment().properties.templateLink.uri, 'nested/azuredeploy.json')]"

## <a name="array-functions"></a>Arrayfunktionen
Der Ressourcen-Manager stellt die folgenden Funktionen für das Arbeiten mit Arraywerten bereit:

* [concat](#concatarray)
* [Länge](#length)
* [skip](#skip)
* [take](#take)

Informationen zu einem Array mit Zeichenfolgenwerten, die durch einen Wert getrennt sind, finden Sie unter [split](#split).

<a id="concatarray" />

### <a name="concat---array"></a>concat – Array
`concat (array1, array2, array3, ...)`

Kombiniert mehrere Arrays und gibt das verkettete Array zurück. 

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| array1 |Ja |Array |Das erste Array für die Verkettung. |
| Weitere Arrays |Nein |Array |Weitere Arrays in sequenzieller Reihenfolge für die Verkettung. |

Diese Funktion akzeptiert eine beliebige Anzahl von Argumenten und Zeichenfolgen oder Arrays für die Parameter. Ein Beispiel für das Verketten von Zeichenfolgenwerten finden Sie unter [concat – Zeichenfolge](#concat).

Im folgenden Beispiel wird veranschaulicht, wie zwei Arrays kombiniert werden.

    "parameters": {
        "firstarray": {
            type: "array"
        }
        "secondarray": {
            type: "array"
        }
     },
     "variables": {
         "combinedarray": "[concat(parameters('firstarray'), parameters('secondarray'))]
     }


<a id="length" />

### <a name="length---array"></a>length – Array
`length(array)`

Gibt die Anzahl der Elemente in einem Array zurück.

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| array |Ja |Array |Das Array, mit dem die Anzahl von Elementen abgerufen wird. |

Sie können diese Funktion mit einem Array verwenden, um bei der Erstellung von Ressourcen die Anzahl der Iterationen anzugeben. Im folgenden Beispiel bezieht sich der Parameter **siteNames** auf ein Array von Namen, die bei der Erstellung der Websites verwendet werden.

    "copy": {
        "name": "websitescopy",
        "count": "[length(parameters('siteNames'))]"
    }

Weitere Informationen zur Verwendung dieser Funktion mit einem Array finden Sie unter [Erstellen mehrerer Instanzen von Ressourcen im Azure-Ressourcen-Manager](resource-group-create-multiple.md). 

Ein Beispiel für die Verwendung von „length“ mit einem Zeichenfolgenwert finden Sie unter [length – Zeichenfolge](#lengthstring).

<a id="skip" />

### <a name="skip---array"></a>skip – Array
`skip(originalValue, numberToSkip)`

Gibt ein Array mit allen Elementen nach der angegebenen Anzahl im Array zurück.

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| originalValue |Ja |Array |Das Array, das für das Überspringen verwendet werden soll. |
| numberToSkip |Ja |Integer |Die Anzahl von zu überspringenden Elementen. Wenn dieser Wert 0 oder kleiner ist, werden alle Elemente im Array zurückgegeben. Wenn er größer als die Länge des Arrays ist, wird ein leeres Array zurückgegeben. |

Ein Beispiel für die Verwendung von „skip“ mit einer Zeichenfolge finden Sie unter [skip – Zeichenfolge](#skipstring).

Im folgenden Beispiel wird die angegebene Anzahl von Elementen im Array übersprungen.

    "parameters": {
      "first": {
        "type": "array",
        "metadata": {
          "description": "Values to use for skipping"
        },
        "defaultValue": [ "one", "two", "three" ]
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Number of elements to skip"
        }
      }
    },
    "resources": [
    ],
    "outputs": {
      "return": {
        "type": "array",
        "value": "[skip(parameters('first'), parameters('second'))]"
      }
    }

<a id="take" />

### <a name="take---array"></a>take – Array
`take(originalValue, numberToTake)`

Gibt ein Array mit der angegebenen Anzahl von Elementen ab dem Anfang des Arrays zurück.

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| originalValue |Ja |Array |Das Array, aus dem die Elemente entnommen werden sollen. |
| numberToTake |Ja |Integer |Die Anzahl von zu entnehmenden Elementen. Ist dieser Wert 0 oder kleiner, wird ein leeres Array zurückgegeben. Ist der Wert größer als die Länge des entsprechenden Arrays, werden alle Elemente des Arrays zurückgegeben. |

Ein Beispiel für die Verwendung von „take“ mit einer Zeichenfolge finden Sie unter [take – Zeichenfolge](#takestring).

Im folgenden Beispiel wird die angegebene Anzahl von Elementen aus dem Array verwendet.

    "parameters": {
      "first": {
        "type": "array",
        "metadata": {
          "description": "Values to use for taking"
        },
        "defaultValue": [ "one", "two", "three" ]
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Number of elements to take"
        }
      }
    },
    "resources": [
    ],
    "outputs": {
      "return": {
        "type": "array",
        "value": "[take(parameters('first'),parameters('second'))]"
      }
    }

## <a name="deployment-value-functions"></a>Funktionen für Bereitstellungswerte
Der Ressourcen-Manager stellt die folgenden Funktionen zum Abrufen von Werten aus Vorlagenabschnitten sowie von bereitstellungsbezogenen Werten bereit:

* [deployment](#deployment)
* [parameters](#parameters)
* [Variablen](#variables)

Informationen zum Abrufen von Werten aus Ressourcen, Ressourcengruppen oder Abonnements finden Sie unter [Ressourcenfunktionen](#resource-functions).

<a id="deployment" />

### <a name="deployment"></a>Bereitstellung
`deployment()`

Gibt Informationen zum aktuellen Bereitstellungsvorgang zurück.

Diese Funktion gibt das Objekt zurück, das während der Bereitstellung übergeben wird. Die Eigenschaften im zurückgegebenen Objekt hängen davon ab, ob das Bereitstellungsobjekt als Link oder als Inlineobjekt übergeben wird. 

Wenn das Bereitstellungsobjekt als Inlineobjekt übergeben wird, z.B. bei Verwendung des **-TemplateFile**-Parameters in Azure PowerShell zum Verweisen auf eine lokale Datei, hat das zurückgegebene Objekt das folgende Format:

    {
        "name": "",
        "properties": {
            "template": {
                "$schema": "",
                "contentVersion": "",
                "parameters": {},
                "variables": {},
                "resources": [
                ],
                "outputs": {}
            },
            "parameters": {},
            "mode": "",
            "provisioningState": ""
        }
    }

Wenn das Objekt als Link übergeben wird, z.B. bei Verwendung des **-TemplateUri**-Parameters zum Verweisen auf ein Remoteobjekt, hat das zurückgegebene Objekt folgendes Format: 

    {
        "name": "",
        "properties": {
            "templateLink": {
                "uri": ""
            },
            "template": {
                "$schema": "",
                "contentVersion": "",
                "parameters": {},
                "variables": {},
                "resources": [],
                "outputs": {}
            },
            "parameters": {},
            "mode": "",
            "provisioningState": ""
        }
    }

Im folgenden Beispiel wird veranschaulicht, wie die Bereitstellung() verwendet wird, um zu einer anderen Vorlage basierend auf dem URI der übergeordneten Vorlage eine Verknüpfung erstellt wird.

    "variables": {  
        "sharedTemplateUrl": "[uri(deployment().properties.templateLink.uri, 'shared-resources.json')]"  
    }  

<a id="parameters" />

### <a name="parameters"></a>parameters
`parameters (parameterName)`

Gibt einen Parameterwert zurück. Der spezifizierte Parametername muss im Parameterabschnitt der Vorlage definiert werden.

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| parameterName |Ja |String |Der Name des zurückzugebenden Parameter. |

Die folgenden Beispiele zeigen eine vereinfachte Nutzungsweise der Parameterfunktion.

    "parameters": { 
      "siteName": {
          "type": "string"
      }
    },
    "resources": [
       {
          "apiVersion": "2014-06-01",
          "name": "[parameters('siteName')]",
          "type": "Microsoft.Web/Sites",
          ...
       }
    ]

<a id="variables" />

### <a name="variables"></a>Variablen
`variables (variableName)`

Gibt den Wert der Variablen zurück. Der angegebene Variablenname muss im Variablenabschnitt der Vorlage definiert werden.

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| variableName |Ja |String |Der Name der zurückzugebenden Variable. |

Im folgenden Beispiel wird ein Variablenwert verwendet.

    "variables": {
      "storageName": "[concat('storage', uniqueString(resourceGroup().id))]"
    },
    "resources": [
      {
        "type": "Microsoft.Storage/storageAccounts",
        "name": "[variables('storageName')]",
        ...
      }
    ],

## <a name="resource-functions"></a>Ressourcenfunktionen
Der Ressourcen-Manager stellt die folgenden Funktionen zum Abrufen von Ressourcenwerten bereit:

* [listKeys und list{Value}](#listkeys)
* [providers](#providers)
* [Referenz](#reference)
* [Ressourcengruppe](#resourcegroup)
* [Ressourcen-ID](#resourceid)
* [Abonnement](#subscription)

Informationen zum Abrufen von Werten aus Parametern, Variablen oder der aktuellen Bereitstellung finden Sie unter [Funktionen für Bereitstellungswerte](#deployment-value-functions).

<a id="listkeys" />
<a id="list" />

### <a name="listkeys-and-listvalue"></a>listKeys und list{Value}
`listKeys (resourceName or resourceIdentifier, apiVersion)`

`list{Value} (resourceName or resourceIdentifier, apiVersion)`

Gibt die Werte für einen beliebigen Ressourcentyp zurück, der den list-Vorgang unterstützt. Am häufigsten wird **listKeys**verwendet. 

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| resourceName oder resourceIdentifier |Ja |String |Eindeutiger Bezeichner für die Ressource. |
| apiVersion |Ja |String |API-Version eines Ressourcen-Laufzeitstatus. In der Regel im Format **jjjj-mm-tt**. |

Jeder Vorgang, der mit **list** beginnt, kann als Funktion in der Vorlage verwendet werden. Zu den verfügbaren Vorgängen gehört nicht nur **listKeys**, sondern auch Vorgänge wie **list**, **listAdminKeys** und **listStatus**. Um zu bestimmen, welche Ressourcentypen einen list-Vorgang aufweisen, verwenden Sie den folgenden PowerShell-Befehl:

    Get-AzureRmProviderOperation -OperationSearchString *  | where {$_.Operation -like "*list*"} | FT Operation

Stattdessen können Sie die Liste auch über die Azure-Befehlszeilenschnittstelle abrufen. Das folgende Beispiel ruft alle Vorgänge für **apiapps**ab und verwendet das JSON-Hilfsprogramm [jq](http://stedolan.github.io/jq/download/) , um nur die list-Vorgänge zu filtern.

    azure provider operations show --operationSearchString */apiapps/* --json | jq ".[] | select (.operation | contains(\"list\"))"

Die Ressourcen-ID kann mithilfe der [Ressourcen-ID-Funktion](#resourceid) oder mithilfe des Formats **{providerNamespace}/{resourceType}/{resourceName}** angegeben werden.

Das folgende Beispiel zeigt, wie die Primär- und Sekundärschlüssel von einem Speicherkonto im Abschnitt „outputs“ zurückgegeben werden können:

    "outputs": { 
      "listKeysOutput": { 
        "value": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName')), '2016-01-01')]", 
        "type" : "object" 
      } 
    } 

Das zurückgegebene Objekt von „listKeys“ hat das folgende Format:

    {
      "keys": [
        {
          "keyName": "key1",
          "permissions": "Full",
          "value": "{value}"
        },
        {
          "keyName": "key2",
          "permissions": "Full",
          "value": "{value}"
        }
      ]
    }

<a id="providers" />

### <a name="providers"></a>providers
`providers (providerNamespace, [resourceType])`

Gibt Informationen zu einem Ressourcenanbieter und den von ihm unterstützten Ressourcentypen zurück. Wenn Sie keinen Ressourcentyp angeben, gibt die Funktion alle unterstützten Typen für den Ressourcenanbieter zurück.

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| providerNamespace |Ja |String |Namespace des Anbieters |
| resourceType |Nein |String |Der Ressourcentyp innerhalb des angegebenen Namespace. |

Jeder unterstützte Typ wird im unten angegebenen Format zurückgegeben. Die Arraysortierung ist dabei nicht garantiert.

    {
        "resourceType": "",
        "locations": [ ],
        "apiVersions": [ ]
    }

Das folgende Beispiel zeigt die Nutzungsweise der Anbieterfunktion:

    "outputs": {
        "exampleOutput": {
            "value": "[providers('Microsoft.Storage', 'storageAccounts')]",
            "type" : "object"
        }
    }

<a id="reference" />

### <a name="reference"></a>Referenz
`reference (resourceName or resourceIdentifier, [apiVersion])`

Gibt ein Objekt zurück, das den Laufzeitstatus einer anderen Ressource darstellt.

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| resourceName oder resourceIdentifier |Ja |String |Name oder eindeutiger Bezeichner einer Ressource |
| apiVersion |Nein |String |API-Version der angegebenen Ressource. Schließen Sie diesen Parameter ein, wenn die Ressource nicht innerhalb der gleichen Vorlage bereitgestellt wird. In der Regel im Format **jjjj-mm-tt**. |

Die **Referenz** -Funktion leitet ihren Wert von einem Laufzeitstatus ab und kann somit nicht im Variablen-Abschnitt verwendet werden. Sie kann in Ausgabeabschnitten einer Vorlage verwendet werden.

Mithilfe der Referenzfunktion können Sie implizit deklarieren, dass eine Ressource von einer anderen abhängt, wenn die referenzierte Ressource innerhalb der gleichen Vorlage zur Verfügung gestellt wird. Sie müssen nicht zusätzlich die Eigenschaft **dependsOn** verwenden. Die Funktion wird nicht ausgewertet, bis die Ressource, auf die verwiesen wird, die Bereitstellung abgeschlossen hat.

Im folgenden Beispiel wird auf ein Speicherkonto verwiesen, das in der gleichen Vorlage bereitgestellt wird.

    "outputs": {
        "NewStorage": {
            "value": "[reference(parameters('storageAccountName'))]",
            "type" : "object"
        }
    }

Im folgenden Beispiel wird auf ein Speicherkonto verwiesen, das nicht in dieser Vorlage bereitgestellt wird, aber innerhalb der gleichen Ressourcengruppe wie die bereitgestellten Ressourcen enthalten ist.

    "outputs": {
        "ExistingStorage": {
            "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName')), '2016-01-01')]",
            "type" : "object"
        }
    }

Sie können wie im folgenden Beispiel über das zurückgegebene Objekt, beispielsweise den Blobendpunkt-URI, einen bestimmten Wert abrufen:

    "outputs": {
        "BlobUri": {
            "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName')), '2016-01-01').primaryEndpoints.blob]",
            "type" : "string"
        }
    }

Im folgenden Beispiel wird auf ein Speicherkonto in einer anderen Ressourcengruppe verwiesen:

    "outputs": {
        "BlobUri": {
            "value": "[reference(resourceId(parameters('relatedGroup'), 'Microsoft.Storage/storageAccounts/', parameters('storageAccountName')), '2016-01-01').primaryEndpoints.blob]",
            "type" : "string"
        }
    }

Die Eigenschaften für das Objekt, die von der **reference** -Funktion zurückgegeben werden, variieren je nach Ressourcentyp. Um die Eigenschaftennamen und Werte für einen Ressourcentyp anzuzeigen, erstellen Sie eine einfache Vorlage, die das Objekt im Abschnitt **outputs** zurückgibt. Wenn Sie über eine Ressource dieses Typs verfügen, gibt Ihre Vorlage lediglich das Objekt zurück, ohne neue Ressourcen bereitzustellen. Wenn Sie nicht über eine vorhandene Ressource dieses Typs verfügen, stellt Ihre Vorlage nur diesen Typ bereit und gibt das Objekt zurück. Fügen Sie anschließend diese Eigenschaften anderen Vorlagen hinzu, die während der Bereitstellung die Werte dynamisch abrufen müssen. 

<a id="resourcegroup" />

### <a name="resourcegroup"></a>Ressourcengruppe
`resourceGroup()`

Gibt ein Objekt zurück, das die aktuelle Ressourcengruppe darstellt. 

Das zurückgegebene Objekt hat das folgende Format:

    {
      "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}",
      "name": "{resourceGroupName}",
      "location": "{resourceGroupLocation}",
      "tags": {
      },
      "properties": {
        "provisioningState": "{status}"
      }
    }

Das folgende Beispiel nutzt den Speicherort der Ressourcengruppe, um einer Website den Speicherort zuzuweisen.

    "resources": [
       {
          "apiVersion": "2014-06-01",
          "type": "Microsoft.Web/sites",
          "name": "[parameters('siteName')]",
          "location": "[resourceGroup().location]",
          ...
       }
    ]

<a id="resourceid" />

### <a name="resourceid"></a>resourceId
`resourceId ([subscriptionId], [resourceGroupName], resourceType, resourceName1, [resourceName2]...)`

Gibt den eindeutigen Bezeichner einer Ressource zurück. 

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| subscriptionId |Nein |Zeichenfolge (im GUID-Format) |Der Standardwert ist das aktuelle Abonnement. Geben Sie diesen Wert an, wenn Sie eine Ressource in einem anderen Abonnement abrufen möchten. |
| ResourceGroupName |Nein |String |Der Standardwert ist die aktuelle Ressourcengruppe. Geben Sie diesen Wert an, wenn Sie eine Ressource in einer anderen Ressourcengruppe abrufen möchten. |
| resourceType |Ja |String |Ressourcentyp einschließlich Namespace von Ressourcenanbieter. |
| resourceName1 |Ja |String |Name der Ressource. |
| resourceName2 |Nein |String |Nächstes Ressourcen-Namensegment, wenn die Ressource geschachtelt ist. |

Diese Funktion wird verwendet, wenn der Ressourcenname zweideutig ist oder nicht innerhalb der gleichen Vorlage zur Verfügung gestellt wird. Der Bezeichner wird im folgenden Format zurückgeben:

    /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/{resourceProviderNamespace}/{resourceType}/{resourceName}

Das folgende Beispiel zeigt, wie die Ressourcen-IDs für eine Website und eine Datenbank abgerufen werden können. Die Website besteht in einer Ressourcengruppe namens **myWebsitesGroup** und die Datenbank in der aktuellen Ressourcengruppe für diese Vorlage.

    [resourceId('myWebsitesGroup', 'Microsoft.Web/sites', parameters('siteName'))]
    [resourceId('Microsoft.SQL/servers/databases', parameters('serverName'), parameters('databaseName'))]

Sie müssen diese Funktion oft nutzen, wenn Sie ein Speicherkonto oder einen virtuellen Computer in einer alternativen Ressourcengruppe verwenden. Das Speicherkonto oder der virtuelle Computer können über mehrere Ressourcengruppen hinweg genutzt werden. Daher sollten Sie diese beim Löschvorgang für eine einzelne Ressourcengruppe nicht löschen. Das folgende Beispiel zeigt, wie eine Ressource von einer externen Ressourcengruppe leicht verwendet werden kann:

    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
          "virtualNetworkName": {
              "type": "string"
          },
          "virtualNetworkResourceGroup": {
              "type": "string"
          },
          "subnet1Name": {
              "type": "string"
          },
          "nicName": {
              "type": "string"
          }
      },
      "variables": {
          "vnetID": "[resourceId(parameters('virtualNetworkResourceGroup'), 'Microsoft.Network/virtualNetworks', parameters('virtualNetworkName'))]",
          "subnet1Ref": "[concat(variables('vnetID'),'/subnets/', parameters('subnet1Name'))]"
      },
      "resources": [
      {
          "apiVersion": "2015-05-01-preview",
          "type": "Microsoft.Network/networkInterfaces",
          "name": "[parameters('nicName')]",
          "location": "[parameters('location')]",
          "properties": {
              "ipConfigurations": [{
                  "name": "ipconfig1",
                  "properties": {
                      "privateIPAllocationMethod": "Dynamic",
                      "subnet": {
                          "id": "[variables('subnet1Ref')]"
                      }
                  }
              }]
           }
      }]
    }

<a id="subscription" />

### <a name="subscription"></a>Abonnement
`subscription()`

Gibt Details über das Abonnement im folgenden Format zurück:

    {
        "id": "/subscriptions/#####",
        "subscriptionId": "#####",
        "tenantId": "#####"
    }

Das folgende Beispiel zeigt ein Abrufen der subscription-Funktion im Abschnitt „outputs“. 

    "outputs": { 
      "exampleOutput": { 
          "value": "[subscription()]", 
          "type" : "object" 
      } 
    } 


## <a name="next-steps"></a>Nächste Schritte
* Eine Beschreibung der Abschnitte in einer Azure-Ressourcen-Manager-Vorlage finden Sie unter [Erstellen von Azure-Ressourcen-Manager-Vorlagen](resource-group-authoring-templates.md)
* Informationen zum Zusammenführen mehrerer Vorlagen finden Sie unter [Verwenden von verknüpften Vorlagen mit Azure-Ressourcen-Manager](resource-group-linked-templates.md)
* Informationen dazu, wie Sie beim Erstellen eines Ressourcentyps eine bestimmte Anzahl von Durchläufen ausführen, finden Sie unter [Erstellen mehrerer Instanzen von Ressourcen im Azure-Ressourcen-Manager](resource-group-create-multiple.md)
* Informationen zum Bereitstellen der erstellten Vorlage finden Sie unter [Bereitstellen einer Anwendung mit einer Azure-Ressourcen-Manager-Vorlage](resource-group-template-deploy.md)




<!--HONumber=Jan17_HO1-->


