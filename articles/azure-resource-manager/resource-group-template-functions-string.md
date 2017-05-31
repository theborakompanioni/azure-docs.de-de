---
title: "Azure Resource Manager-Vorlagenfunktionen – Zeichenfolge | Microsoft-Dokumentation"
description: "Hier werden die Funktionen beschrieben, die in einer Azure Resource Manager-Vorlage zum Arbeiten mit Zeichenfolgen verwendet werden können."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/15/2017
ms.author: tomfitz
ms.translationtype: Human Translation
ms.sourcegitcommit: 17c4dc6a72328b613f31407aff8b6c9eacd70d9a
ms.openlocfilehash: 9932ac04699f49b7a3ea3dabe4d380fdc4d05ec1
ms.contentlocale: de-de
ms.lasthandoff: 05/16/2017


---
# <a name="string-functions-for-azure-resource-manager-templates"></a>Zeichenfolgenfunktionen für Azure Resource Manager-Vorlagen

Der Ressourcen-Manager stellt die folgenden Funktionen für das Arbeiten mit Zeichenfolgen bereit:

* [base64](#base64)
* [base64ToJson](#base64tojson)
* [base64ToString](#base64tostring)
* [bool](#bool)
* [concat](#concat)
* [contains](#contains)
* [dataUri](#datauri)
* [dataUriToString](#datauritostring)
* [empty](#empty)
* [endsWith](#endswith)
* [first](#first)
* [indexOf](#indexof)
* [last](#last)
* [lastIndexOf](#lastindexof)
* [Länge](#length)
* [padLeft](#padleft)
* [replace](#replace)
* [skip](#skip)
* [split](#split)
* [startsWith](resource-group-template-functions-string.md#startswith)
* [string](#string)
* [substring](#substring)
* [take](#take)
* [toLower](#tolower)
* [toUpper](#toupper)
* [trim](#trim)
* [uniqueString](#uniquestring)
* [uri](#uri)
* [uriComponent](resource-group-template-functions-string.md#uricomponent)
* [uriComponentToString](resource-group-template-functions-string.md#uricomponenttostring)

<a id="base64" />

## <a name="base64"></a>base64
`base64(inputString)`

Rückkehr zur base64-Darstellung der Eingabezeichenfolge.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| inputString |Ja |string |Der Wert, der als base64-Darstellung zurückgegeben wird. |

### <a name="examples"></a>Beispiele

Das folgende Beispiel zeigt die Funktionsweise der base64-Funktion.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringData": {
            "type": "string",
            "defaultValue": "one, two, three"
        },
        "jsonFormattedData": {
            "type": "string",
            "defaultValue": "{'one': 'a', 'two': 'b'}"
        }
    },
    "variables": {
        "base64String": "[base64(parameters('stringData'))]",
        "base64Object": "[base64(parameters('jsonFormattedData'))]"
    },
    "resources": [
    ],
    "outputs": {
        "base64Output": {
            "type": "string",
            "value": "[variables('base64String')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[base64ToString(variables('base64String'))]"
        },
        "toJsonOutput": {
            "type": "object",
            "value": "[base64ToJson(variables('base64Object'))]"
        }
    }
}
```

### <a name="return-value"></a>Rückgabewert

Eine Zeichenfolge mit der base64-Darstellung.

<a id="base64tojson" />

## <a name="base64tojson"></a>base64ToJson
`base64tojson`

Konvertiert eine base64-Darstellung in ein JSON-Objekt.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| base64Value |Ja |string |Die in ein JSON-Objekt zu konvertierende base64-Darstellung. |

### <a name="examples"></a>Beispiele

Im folgenden Beispiel wird die base64ToJson-Funktion zum Konvertieren eines base64-Werts verwendet:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringData": {
            "type": "string",
            "defaultValue": "one, two, three"
        },
        "jsonFormattedData": {
            "type": "string",
            "defaultValue": "{'one': 'a', 'two': 'b'}"
        }
    },
    "variables": {
        "base64String": "[base64(parameters('stringData'))]",
        "base64Object": "[base64(parameters('jsonFormattedData'))]"
    },
    "resources": [
    ],
    "outputs": {
        "base64Output": {
            "type": "string",
            "value": "[variables('base64String')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[base64ToString(variables('base64String'))]"
        },
        "toJsonOutput": {
            "type": "object",
            "value": "[base64ToJson(variables('base64Object'))]"
        }
    }
}
```

### <a name="return-value"></a>Rückgabewert

Ein JSON-Objekt.

<a id="base64tostring" />

## <a name="base64tostring"></a>base64ToString
`base64ToString(base64Value)`

Konvertiert eine base64-Darstellung in eine Zeichenfolge.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| base64Value |Ja |string |Die in eine Zeichenfolge zu konvertierende base64-Darstellung. |

### <a name="examples"></a>Beispiele

Im folgenden Beispiel wird die base64ToString-Funktion verwendet, um einen base64-Wert zu konvertieren:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringData": {
            "type": "string",
            "defaultValue": "one, two, three"
        },
        "jsonFormattedData": {
            "type": "string",
            "defaultValue": "{'one': 'a', 'two': 'b'}"
        }
    },
    "variables": {
        "base64String": "[base64(parameters('stringData'))]",
        "base64Object": "[base64(parameters('jsonFormattedData'))]"
    },
    "resources": [
    ],
    "outputs": {
        "base64Output": {
            "type": "string",
            "value": "[variables('base64String')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[base64ToString(variables('base64String'))]"
        },
        "toJsonOutput": {
            "type": "object",
            "value": "[base64ToJson(variables('base64Object'))]"
        }
    }
}
```

### <a name="return-value"></a>Rückgabewert

Eine Zeichenfolge des konvertierten base64-Werts.

<a id="bool" />

## <a name="bool"></a>bool
`bool(arg1)`

Konvertiert den Parameter in einen booleschen Wert.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |Zeichenfolge oder ganze Zahl |Der Wert, der in einen booleschen Wert konvertiert werden soll. |

### <a name="examples"></a>Beispiele

Im folgenden Beispiel wird gezeigt, wie „bool“ mit einer Zeichenfolge oder ganzen Zahl verwendet wird.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "trueString": {
            "value": "[bool('true')]",
            "type" : "bool"
        },
        "falseString": {
            "value": "[bool('false')]",
            "type" : "bool"
        },
        "trueInt": {
            "value": "[bool(1)]",
            "type" : "bool"
        },
        "falseInt": {
            "value": "[bool(0)]",
            "type" : "bool"
        }
    }
}
```

### <a name="return-value"></a>Rückgabewert
Ein boolescher Wert.

<a id="concat" />

## <a name="concat"></a>concat
`concat (arg1, arg2, arg3, ...)`

Kombiniert mehrere Zeichenfolgenwerte und gibt die verkettete Zeichenfolge zurück oder kombiniert mehrere Arrays und gibt das verkettete Array zurück.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |Zeichenfolge oder Array |Der erste Wert für die Verkettung. |
| zusätzliche Argumente |Nein |string |Weitere Werte in sequenzieller Reihenfolge für die Verkettung. |

### <a name="examples"></a>Beispiele

Das folgende Beispiel zeigt, wie zwei Zeichenfolgenwerte kombiniert werden und eine verkettete Zeichenfolge zurückgeben.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "prefix": {
            "type": "string",
            "defaultValue": "prefix"
        }
    },
    "resources": [],
    "outputs": {
        "concatOutput": {
            "value": "[concat(parameters('prefix'), uniqueString(resourceGroup().id))]",
            "type" : "string"
        }
    }
}
```

Im folgenden Beispiel wird veranschaulicht, wie zwei Arrays kombiniert werden.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": { 
        "firstArray": { 
            "type": "array", 
            "defaultValue": [ 
                "1-1", 
                "1-2", 
                "1-3" 
            ] 
        },
        "secondArray": {
            "type": "array", 
            "defaultValue": [ 
                "2-1", 
                "2-2",
                "2-3" 
            ] 
        }
    },
    "resources": [
    ],
    "outputs": {
        "return": {
            "type": "array",
            "value": "[concat(parameters('firstArray'), parameters('secondArray'))]"
        }
    }
}
```

### <a name="return-value"></a>Rückgabewert
Eine Zeichenfolge oder ein Array aus verketteten Werten.

<a id="contains" />

## <a name="contains"></a>contains
`contains (container, itemToFind)`

Überprüft, ob ein Array einen Wert enthält, ein Objekt einen Schlüssel enthält oder eine Zeichenfolge eine Teilzeichenfolge enthält.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| container |Ja |Array, Objekt oder Zeichenfolge |Der Wert, der den zu suchenden Wert enthält. |
| itemToFind |Ja |Zeichenfolge oder ganze Zahl |Der zu suchende Wert. |

### <a name="examples"></a>Beispiele

Das folgende Beispiel zeigt die Verwendung von „contains“ mit unterschiedlichen Typen:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringToTest": {
            "type": "string",
            "defaultValue": "OneTwoThree"
        },
        "objectToTest": {
            "type": "object",
            "defaultValue": {"one": "a", "two": "b", "three": "c"}
        },
        "arrayToTest": {
            "type": "array",
            "defaultValue": ["one", "two", "three"]
        }
    },
    "resources": [
    ],
    "outputs": {
        "stringTrue": {
            "type": "bool",
            "value": "[contains(parameters('stringToTest'), 'e')]"
        },
        "stringFalse": {
            "type": "bool",
            "value": "[contains(parameters('stringToTest'), 'z')]"
        },
        "objectTrue": {
            "type": "bool",
            "value": "[contains(parameters('objectToTest'), 'one')]"
        },
        "objectFalse": {
            "type": "bool",
            "value": "[contains(parameters('objectToTest'), 'a')]"
        },
        "arrayTrue": {
            "type": "bool",
            "value": "[contains(parameters('arrayToTest'), 'three')]"
        },
        "arrayFalse": {
            "type": "bool",
            "value": "[contains(parameters('arrayToTest'), 'four')]"
        }
    }
}
```

### <a name="return-value"></a>Rückgabewert

**True**, wenn das Element gefunden wurde; andernfalls **False**.

<a id="datauri" />

## <a name="datauri"></a>dataUri
`dataUri(stringToConvert)`

Konvertiert einen Wert in einen Daten-URI.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| stringToConvert |Ja |string |Der Wert, der in einen Daten-URI konvertiert werden soll. |

### <a name="examples"></a>Beispiele

Im folgenden Beispiel wird ein Wert in einen Daten-URI und ein Daten-URI in eine Zeichenfolge konvertiert:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringToTest": {
            "type": "string",
            "defaultValue": "Hello"
        },
        "dataFormattedString": {
            "type": "string",
            "defaultValue": "data:;base64,SGVsbG8sIFdvcmxkIQ=="
        }
    },
    "resources": [],
    "outputs": {
        "dataUriOutput": {
            "value": "[dataUri(parameters('stringToTest'))]",
            "type" : "string"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[dataUriToString(parameters('dataFormattedString'))]"
        }
    }
}
```

### <a name="return-value"></a>Rückgabewert

Eine als Daten-URI formatierte Zeichenfolge.

<a id="datauritostring" />

## <a name="datauritostring"></a>dataUriToString
`dataUriToString(dataUriToConvert)`

Konvertiert einen als Daten-URI formatierten Wert in eine Zeichenfolge.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| dataUriToConvert |Ja |string |Der zu konvertierende Daten-URI-Wert. |

### <a name="examples"></a>Beispiele

Im folgenden Beispiel wird ein Wert in einen Daten-URI und ein Daten-URI in eine Zeichenfolge konvertiert:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringToTest": {
            "type": "string",
            "defaultValue": "Hello"
        },
        "dataFormattedString": {
            "type": "string",
            "defaultValue": "data:;base64,SGVsbG8sIFdvcmxkIQ=="
        }
    },
    "resources": [],
    "outputs": {
        "dataUriOutput": {
            "value": "[dataUri(parameters('stringToTest'))]",
            "type" : "string"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[dataUriToString(parameters('dataFormattedString'))]"
        }
    }
}
```

### <a name="return-value"></a>Rückgabewert

Eine Zeichenfolge, die den konvertierten Wert enthält.

<a id="empty" /> 

## <a name="empty"></a>empty
`empty(itemToTest)`

Bestimmt, ob ein Array, Objekt oder eine Zeichenfolge leer ist.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| itemToTest |Ja |Array, Objekt oder Zeichenfolge |Der Wert, für den geprüft werden soll, ob er leer ist. |

### <a name="examples"></a>Beispiele

Im folgenden Beispiel wird überprüft, ob ein Array, Objekt und eine Zeichenfolge leer sind.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testArray": {
            "type": "array",
            "defaultValue": []
        },
        "testObject": {
            "type": "object",
            "defaultValue": {}
        },
        "testString": {
            "type": "string",
            "defaultValue": ""
        }
    },
    "resources": [
    ],
    "outputs": {
        "arrayEmpty": {
            "type": "bool",
            "value": "[empty(parameters('testArray'))]"
        },
        "objectEmpty": {
            "type": "bool",
            "value": "[empty(parameters('testObject'))]"
        },
        "stringEmpty": {
            "type": "bool",
            "value": "[empty(parameters('testString'))]"
        }
    }
}
```

### <a name="return-value"></a>Rückgabewert

Gibt **True** zurück, wenn der Werte leer ist. Andernfalls wird **False** zurückgegeben.

<a id="endswith" />

## <a name="endswith"></a>endsWith
`endsWith(stringToSearch, stringToFind)`

Bestimmt, ob eine Zeichenfolge mit einem Wert endet. Bei dem Vergleich wird Groß- und Kleinschreibung nicht unterschieden.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| stringToSearch |Ja |string |Der Wert, der das zu suchende Element enthält. |
| stringToFind |Ja |string |Der zu suchende Wert. |

### <a name="examples"></a>Beispiele

Das folgende Beispiel zeigt die Verwendung der Funktionen „startsWith“ und „endsWith“:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "startsTrue": {
            "value": "[startsWith('abcdef', 'ab')]",
            "type" : "bool"
        },
        "startsCapTrue": {
            "value": "[startsWith('abcdef', 'A')]",
            "type" : "bool"
        },
        "startsFalse": {
            "value": "[startsWith('abcdef', 'e')]",
            "type" : "bool"
        },
        "endsTrue": {
            "value": "[endsWith('abcdef', 'ef')]",
            "type" : "bool"
        },
        "endsCapTrue": {
            "value": "[endsWith('abcdef', 'F')]",
            "type" : "bool"
        },
        "endsFalse": {
            "value": "[endsWith('abcdef', 'e')]",
            "type" : "bool"
        }
    }
}
```

### <a name="return-value"></a>Rückgabewert

**True**, wenn das letzte Zeichen oder die letzten Zeichen der Zeichenfolge mit dem Wert übereinstimmen, andernfalls **False**.

<a id="first" />

## <a name="first"></a>first
`first(arg1)`

Gibt das erste Zeichen der Zeichenfolge oder das erste Element des Arrays zurück.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |Array oder Zeichenfolge |Der Wert, dessen erstes Element oder Zeichen abgerufen wird. |

### <a name="examples"></a>Beispiele

Im folgenden Beispiel wird die Verwendung der first-Funktion mit einem Array und einer Zeichenfolge gezeigt.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": ["one", "two", "three"]
        }
    },
    "resources": [
    ],
    "outputs": {
        "arrayOutput": {
            "type": "string",
            "value": "[first(parameters('arrayToTest'))]"
        },
        "stringOutput": {
            "type": "string",
            "value": "[first('One Two Three')]"
        }
    }
}
```

### <a name="return-value"></a>Rückgabewert

Ein Zeichenfolgenwert, der dem letzten Zeichen entspricht, bzw. der Typ (Zeichenfolge, ganze Zahl, Array oder Objekt) des ersten Elements in einem Array.

<a id="indexof" />

## <a name="indexof"></a>indexOf
`indexOf(stringToSearch, stringToFind)`

Gibt die erste Position eines Werts innerhalb einer Zeichenfolge zurück. Bei dem Vergleich wird Groß- und Kleinschreibung nicht unterschieden.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| stringToSearch |Ja |string |Der Wert, der das zu suchende Element enthält. |
| stringToFind |Ja |string |Der zu suchende Wert. |

### <a name="examples"></a>Beispiele

Das folgende Beispiel zeigt die Verwendung der Funktionen „indexOf“ und „lastIndexOf“:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "firstT": {
            "value": "[indexOf('test', 't')]",
            "type" : "int"
        },
        "lastT": {
            "value": "[lastIndexOf('test', 't')]",
            "type" : "int"
        },
        "firstString": {
            "value": "[indexOf('abcdef', 'CD')]",
            "type" : "int"
        },
        "lastString": {
            "value": "[lastIndexOf('abcdef', 'AB')]",
            "type" : "int"
        },
        "notFound": {
            "value": "[indexOf('abcdef', 'z')]",
            "type" : "int"
        }
    }
}
```

### <a name="return-value"></a>Rückgabewert

Eine ganze Zahl, die die Position des zu suchenden Elements darstellt. Der Wert ist nullbasiert. Wenn das Element nicht gefunden wird, wird -1 zurückgegeben.


<a id="last" />

## <a name="last"></a>last
`last (arg1)`

Gibt das letzte Zeichen der Zeichenfolge bzw. das letzte Element des Arrays zurück.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |Array oder Zeichenfolge |Der Wert, dessen letztes Element oder Zeichen abgerufen wird. |

### <a name="examples"></a>Beispiele

Im folgenden Beispiel wird die Verwendung der last-Funktion mit einem Array und einer Zeichenfolge gezeigt.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": ["one", "two", "three"]
        }
    },
    "resources": [
    ],
    "outputs": {
        "arrayOutput": {
            "type": "string",
            "value": "[last(parameters('arrayToTest'))]"
        },
        "stringOutput": {
            "type": "string",
            "value": "[last('One Two Three')]"
        }
    }
}
```

### <a name="return-value"></a>Rückgabewert

Ein Zeichenfolgenwert, der dem letzten Zeichen entspricht, bzw. der Typ (Zeichenfolge, ganze Zahl, Array oder Objekt) des letzten Elements in einem Array.

<a id="lastindexof" />

## <a name="lastindexof"></a>lastIndexOf
`lastIndexOf(stringToSearch, stringToFind)`

Gibt die letzte Position eines Werts innerhalb einer Zeichenfolge zurück. Bei dem Vergleich wird Groß- und Kleinschreibung nicht unterschieden.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| stringToSearch |Ja |string |Der Wert, der das zu suchende Element enthält. |
| stringToFind |Ja |string |Der zu suchende Wert. |

### <a name="examples"></a>Beispiele

Das folgende Beispiel zeigt die Verwendung der Funktionen „indexOf“ und „lastIndexOf“:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "firstT": {
            "value": "[indexOf('test', 't')]",
            "type" : "int"
        },
        "lastT": {
            "value": "[lastIndexOf('test', 't')]",
            "type" : "int"
        },
        "firstString": {
            "value": "[indexOf('abcdef', 'CD')]",
            "type" : "int"
        },
        "lastString": {
            "value": "[lastIndexOf('abcdef', 'AB')]",
            "type" : "int"
        },
        "notFound": {
            "value": "[indexOf('abcdef', 'z')]",
            "type" : "int"
        }
    }
}
```

### <a name="return-value"></a>Rückgabewert

Eine ganze Zahl, die die letzte Position des zu suchenden Elements darstellt. Der Wert ist nullbasiert. Wenn das Element nicht gefunden wird, wird -1 zurückgegeben.


<a id="length" />

## <a name="length"></a>Länge
`length(string)`

Gibt die Anzahl von Zeichen in einer Zeichenfolge bzw. von Elementen in einem Array zurück.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |Array oder Zeichenfolge |Das Array, von dem die Anzahl der Elemente, bzw. die Zeichenfolge, von der die Anzahl der Zeichen ermittelt werden soll. |

### <a name="examples"></a>Beispiele

Im folgenden Beispiel wird die Verwendung von „length“ mit einem Array und einer Zeichenfolge gezeigt:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": [
                "one",
                "two",
                "three"
            ]
        },
        "stringToTest": {
            "type": "string",
            "defaultValue": "One Two Three"
        }
    },
    "resources": [],
    "outputs": {
        "arrayLength": {
            "type": "int",
            "value": "[length(parameters('arrayToTest'))]"
        },
        "stringLength": {
            "type": "int",
            "value": "[length(parameters('stringToTest'))]"
        }
    }
}
```

### <a name="return-value"></a>Rückgabewert

Eine ganze Zahl. 

<a id="padleft" />

## <a name="padleft"></a>padLeft
`padLeft(valueToPad, totalLength, paddingCharacter)`

Gibt eine rechtsbündig ausgerichtete Zeichenfolge zurück, indem links Zeichen hinzugefügt werden, bis die angegebene Gesamtlänge erreicht ist.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| valueToPad |Ja |Zeichenfolge oder ganze Zahl |Der Wert, der rechtsbündig ausgerichtet werden soll. |
| totalLength |Ja |int |Die Gesamtzahl der Zeichen in der zurückgegebenen Zeichenfolge. |
| paddingCharacter |Nein |einzelnes Zeichen |Das Zeichen, das für das Auffüllen auf der linken Seite verwendet werden soll, bis die Gesamtlänge erreicht ist. Der Standardwert ist ein Leerzeichen. |

Wenn die Länge der ursprünglichen Zeichenfolge die Anzahl der aufzufüllenden Zeichen überschreitet, werden keine Zeichen hinzugefügt.

### <a name="examples"></a>Beispiele

Das folgende Beispiel zeigt, wie Sie den vom Benutzer angegebenen Parameterwert auffüllen, indem Sie das Nullzeichen hinzufügen, bis er die Gesamtzahl der Zeichen erreicht. 

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "123"
        },
        "totalCharacters": {
            "type": "int",
            "defaultValue": 10
        }
    },
    "resources": [],
    "outputs": {
        "stringOutput": {
            "type": "string",
            "value": "[padLeft(parameters('testString'),parameters('totalCharacters'),'0')]"
        }
    }
}
```

### <a name="return-value"></a>Rückgabewert

Eine Zeichenfolge mit mindestens der angegebenen Zeichenanzahl.

<a id="replace" />

## <a name="replace"></a>replace
`replace(originalString, oldString, newString)`

Gibt eine neue Zeichenfolge zurück, in der alle Instanzen einer Zeichenfolge durch eine andere Zeichenfolge ersetzt wurden.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| originalString |Ja |string |Der Wert, für den alle Instanzen einer Zeichenfolge durch eine andere Zeichenfolge ersetzt wurden. |
| oldString |Ja |string |Die Zeichenfolge, die aus der ursprünglichen Zeichenfolge entfernt werden soll. |
| newString |Ja |string |Die Zeichenfolge, die anstelle der entfernten Zeichenfolge eingefügt werden soll. |

### <a name="examples"></a>Beispiele

Das folgende Beispiel zeigt, wie Sie aus einer von einem Benutzer bereitgestellten Zeichenfolge alle Gedankenstriche entfernen und einen Teil der Zeichenfolge durch eine andere Zeichenfolge ersetzen.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "123-123-1234"
        }
    },
    "resources": [],
    "outputs": {
        "firstOutput": {
            "type": "string",
            "value": "[replace(parameters('testString'),'-', '')]"
        },
        "secodeOutput": {
            "type": "string",
            "value": "[replace(parameters('testString'),'1234', 'xxxx')]"
        }
    }
}
```

### <a name="return-value"></a>Rückgabewert

Eine Zeichenfolge mit den ersetzten Zeichen.

<a id="skip" />

## <a name="skip"></a>skip
`skip(originalValue, numberToSkip)`

Gibt eine Zeichenfolge mit allen Zeichen gemäß der angegebenen Anzahl von Zeichen oder ein Array mit allen Elementen gemäß der angegebenen Anzahl von Elementen zurück.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| originalValue |Ja |Array oder Zeichenfolge |Array oder Zeichenfolge, wo Elemente übersprungen werden sollen. |
| numberToSkip |Ja |int |Die Anzahl der zu überspringenden Elemente bzw. Zeichen. Wenn dieser Wert 0 (null) oder kleiner ist, werden alle Elemente oder Zeichen in dem Wert zurückgegeben. Ist der Wert größer als die Länge des Arrays bzw. der Zeichenfolge, wird ein leeres Array bzw. eine leere Zeichenfolge zurückgegeben. |

### <a name="examples"></a>Beispiele

Im folgenden Beispiel wird die angegebene Anzahl von Elementen im Array und Zeichen in der Zeichenfolge übersprungen.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testArray": {
            "type": "array",
            "defaultValue": [
                "one",
                "two",
                "three"
            ]
        },
        "elementsToSkip": {
            "type": "int",
            "defaultValue": 2
        },
        "testString": {
            "type": "string",
            "defaultValue": "one two three"
        },
        "charactersToSkip": {
            "type": "int",
            "defaultValue": 4
        }
    },
    "resources": [],
    "outputs": {
        "arrayOutput": {
            "type": "array",
            "value": "[skip(parameters('testArray'),parameters('elementsToSkip'))]"
        },
        "stringOutput": {
            "type": "string",
            "value": "[skip(parameters('testString'),parameters('charactersToSkip'))]"
        }
    }
}
```

### <a name="return-value"></a>Rückgabewert

Ein Array oder eine Zeichenfolge.

<a id="split" />

## <a name="split"></a>split
`split(inputString, delimiter)`

Gibt ein Array mit Zeichenfolgen zurück, das die Teilzeichenfolgen der Eingabezeichenfolge getrennt durch die angegebenen Trennzeichen enthält.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| inputString |Ja |string |Die zu teilende Zeichenfolge. |
| Trennzeichen |Ja |Zeichenfolge oder Array von Zeichenfolgen |Das Trennzeichen, das zum Teilen der Zeichenfolge verwendet werden soll. |

### <a name="examples"></a>Beispiele

Im nächsten Beispiel wird die Eingabezeichenfolge mit einem Komma sowie entweder mit einem Komma oder Semikolon getrennt.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstString": {
            "type": "string",
            "defaultValue": "one,two,three"
        },
        "secondString": {
            "type": "string",
            "defaultValue": "one;two,three"
        }
    },
    "variables": {
        "delimiters": [ ",", ";" ]
    },
    "resources": [],
    "outputs": {
        "firstOutput": {
            "type": "array",
            "value": "[split(parameters('firstString'),',')]"
        },
        "secondOutput": {
            "type": "array",
            "value": "[split(parameters('secondString'),variables('delimiters'))]"
        }
    }
}
```

### <a name="return-value"></a>Rückgabewert

Ein Array der Zeichenfolgen.

<a id="startswith" />

## <a name="startswith"></a>startsWith
`startsWith(stringToSearch, stringToFind)`

Stellt fest, ob eine Zeichenfolge mit einem bestimmten Wert beginnt. Bei dem Vergleich wird Groß- und Kleinschreibung nicht unterschieden.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| stringToSearch |Ja |string |Der Wert, der das zu suchende Element enthält. |
| stringToFind |Ja |string |Der zu suchende Wert. |

### <a name="examples"></a>Beispiele

Das folgende Beispiel zeigt die Verwendung der Funktionen „startsWith“ und „endsWith“:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "startsTrue": {
            "value": "[startsWith('abcdef', 'ab')]",
            "type" : "bool"
        },
        "startsCapTrue": {
            "value": "[startsWith('abcdef', 'A')]",
            "type" : "bool"
        },
        "startsFalse": {
            "value": "[startsWith('abcdef', 'e')]",
            "type" : "bool"
        },
        "endsTrue": {
            "value": "[endsWith('abcdef', 'ef')]",
            "type" : "bool"
        },
        "endsCapTrue": {
            "value": "[endsWith('abcdef', 'F')]",
            "type" : "bool"
        },
        "endsFalse": {
            "value": "[endsWith('abcdef', 'e')]",
            "type" : "bool"
        }
    }
}
```

### <a name="return-value"></a>Rückgabewert

**True**, wenn das Zeichen oder die ersten Zeichen der Zeichenfolge mit dem Wert übereinstimmen, andernfalls **False**.


<a id="string" />

## <a name="string"></a>string
`string(valueToConvert)`

Konvertiert den angegebenen Wert in eine Zeichenfolge.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| valueToConvert |Ja | Beliebig |Der Wert, der in eine Zeichenfolge konvertiert werden soll. Werte aller Typen können konvertiert werden, auch Objekte und Arrays. |

### <a name="examples"></a>Beispiele

Das folgende Beispiel zeigt das Konvertieren verschiedener Typen von Werten in Zeichenfolgen:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testObject": {
            "type": "object",
            "defaultValue": {
                "valueA": 10,
                "valueB": "Example Text"
            }
        },
        "testArray": {
            "type": "array",
            "defaultValue": [
                "a",
                "b",
                "c"
            ]
        },
        "testInt": {
            "type": "int",
            "defaultValue": 5
        }
    },
    "resources": [],
    "outputs": {
        "objectOutput": {
            "type": "string",
            "value": "[string(parameters('testObject'))]"
        },
        "arrayOutput": {
            "type": "string",
            "value": "[string(parameters('testArray'))]"
        },
        "intOutput": {
            "type": "string",
            "value": "[string(parameters('testInt'))]"
        }
    }
}
```

### <a name="return-value"></a>Rückgabewert

Eine Zeichenfolge.

<a id="substring" />

## <a name="substring"></a>substring
`substring(stringToParse, startIndex, length)`

Gibt eine Teilzeichenfolge zurück, die an der angegebenen Zeichenposition beginnt und die angegebene Anzahl von Zeichen enthält.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| stringToParse |Ja |string |Die ursprüngliche Zeichenfolge, aus der die Teilzeichenfolge extrahiert wird. |
| startIndex |Nein |int |Die nullbasierte Anfangsposition für die Teilzeichenfolge. |
| Länge |Nein |int |Die Anzahl der Zeichen der Teilzeichenfolge. Muss auf eine Position innerhalb der Zeichenfolge verweisen. |

### <a name="examples"></a>Beispiele

Im folgenden Beispiel wird eine Teilzeichenfolge aus einem Parameter extrahiert.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "one two three"
        }
    },
    "resources": [],
    "outputs": {
        "substringOutput": {
            "value": "[substring(parameters('testString'), 4, 3)]",
            "type": "string"
        }
    }
}
```

Das folgende Beispiel führt zu dem Fehler „Die Parameter "index" und "length" müssen auf eine Position innerhalb der Zeichenfolge verweisen. Parameter "index": '0', Parameter "length": '11', Länge des Parameters "string": '10'.“.

```json
"parameters": {
    "inputString": { "type": "string", "value": "1234567890" }
},
"variables": { 
    "prefix": "[substring(parameters('inputString'), 0, 11)]"
}
```

<a id="take" />

## <a name="take"></a>take
`take(originalValue, numberToTake)`

Gibt eine Zeichenfolge mit der angegebenen Anzahl von Zeichen ab dem Anfang der Zeichenfolge bzw. ein Array mit der angegebenen Anzahl von Elementen ab dem Anfang des Arrays zurück.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| originalValue |Ja |Array oder Zeichenfolge |Das Array bzw. die Zeichenfolge, wo die Elemente entnommen werden sollen. |
| numberToTake |Ja |int |Die Anzahl der zu entnehmenden Elemente bzw. Zeichen. Ist dieser Wert 0 oder kleiner, wird ein leeres Array bzw. eine leere Zeichenfolge zurückgegeben. Ist der Wert größer als die Länge des entsprechenden Arrays bzw. der Zeichenfolge, werden alle Elemente des Arrays bzw. der Zeichenfolge zurückgegeben. |

### <a name="examples"></a>Beispiele

Im folgenden Beispiel wird die angegebene Anzahl von Elementen aus dem Array und Zeichen aus der Zeichenfolge entnommen.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testArray": {
            "type": "array",
            "defaultValue": [
                "one",
                "two",
                "three"
            ]
        },
        "elementsToTake": {
            "type": "int",
            "defaultValue": 2
        },
        "testString": {
            "type": "string",
            "defaultValue": "one two three"
        },
        "charactersToTake": {
            "type": "int",
            "defaultValue": 2
        }
    },
    "resources": [],
    "outputs": {
        "arrayOutput": {
            "type": "array",
            "value": "[take(parameters('testArray'),parameters('elementsToTake'))]"
        },
        "stringOutput": {
            "type": "string",
            "value": "[take(parameters('testString'),parameters('charactersToTake'))]"
        }
    }
}
```

### <a name="return-value"></a>Rückgabewert

Ein Array oder eine Zeichenfolge.

<a id="tolower" />

## <a name="tolower"></a>toLower
`toLower(stringToChange)`

Konvertiert die angegebene Zeichenfolge in Kleinbuchstaben.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| stringToChange |Ja |string |Der Wert, der in Kleinbuchstaben konvertiert werden soll. |

### <a name="examples"></a>Beispiele

Im folgenden Beispiel wird ein Parameterwert in Klein- und Großbuchstaben konvertiert.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "One Two Three"
        }
    },
    "resources": [],
    "outputs": {
        "toLowerOutput": {
            "value": "[toLower(parameters('testString'))]",
            "type": "string"
        },
        "toUpperOutput": {
            "type": "string",
            "value": "[toUpper(parameters('testString'))]"
        }
    }
}
```

<a id="toupper" />

## <a name="toupper"></a>toUpper
`toUpper(stringToChange)`

Konvertiert die angegebene Zeichenfolge in Großbuchstaben.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| stringToChange |Ja |string |Der Wert, der in Großbuchstaben konvertiert werden soll. |

### <a name="examples"></a>Beispiele

Im folgenden Beispiel wird ein Parameterwert in Klein- und Großbuchstaben konvertiert.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "One Two Three"
        }
    },
    "resources": [],
    "outputs": {
        "toLowerOutput": {
            "value": "[toLower(parameters('testString'))]",
            "type": "string"
        },
        "toUpperOutput": {
            "type": "string",
            "value": "[toUpper(parameters('testString'))]"
        }
    }
}
```

<a id="trim" />

## <a name="trim"></a>trim
`trim (stringToTrim)`

Entfernt alle führenden und nachgestellten Leerzeichen aus der angegebenen Zeichenfolge.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| stringToTrim |Ja |string |Der zu kürzende Wert. |

### <a name="examples"></a>Beispiele

Im folgenden Beispiel werden die Leerzeichen aus dem Parameter entfernt.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "    one two three   "
        }
    },
    "resources": [],
    "outputs": {
        "return": {
            "type": "string",
            "value": "[trim(parameters('testString'))]"
        }
    }
}
```

<a id="uniquestring" />

## <a name="uniquestring"></a>uniqueString
`uniqueString (baseString, ...)`

Erstellt auf der Grundlage der als Parameter angegebenen Werte eine deterministische Hashzeichenfolge. 

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| baseString |Ja |string |Der Wert, der in der Hashfunktion verwendet wird, um eine eindeutige Zeichenfolge zu erstellen. |
| Zusätzliche Parameter nach Bedarf. |Nein |string |Sie können beliebig viele Zeichenfolgen hinzufügen, ganz wie sie zum Erstellen des Werts benötigt werden, der die Ebene der Eindeutigkeit angibt. |

### <a name="remarks"></a>Anmerkungen

Diese Funktion ist hilfreich, wenn Sie einen eindeutigen Namen für eine Ressource erstellen müssen. Sie geben Parameterwerte an, die den Eindeutigkeitsbereich für das Ergebnis einschränken. Sie können angeben, ob der Name bis hinunter zum Abonnement, zur Ressourcengruppe oder zur Bereitstellung eindeutig ist. 

Der zurückgegebene Wert ist keine zufällige Zeichenfolge, sondern das Ergebnis einer Hashfunktion. Der zurückgegebene Wert ist 13 Zeichen lang. Er ist nicht global eindeutig. Es empfiehlt sich, den Wert mit einem Präfix aus Ihrer Benennungskonvention zu kombinieren, um einen aussagekräftigen Namen zu erstellen. Im folgenden Beispiel wird das Format des zurückgegebenen Werts veranschaulicht. Der tatsächliche Wert variiert je nach den angegebenen Parametern.

    tcvhiyu5h2o5o

### <a name="examples"></a>Beispiele

Die folgenden Beispiele zeigen, wie Sie mithilfe von uniqueString einen eindeutigen Wert für häufig verwendete Ebenen erstellen können.

Eindeutige Zuordnung zum Abonnement

```json
"[uniqueString(subscription().subscriptionId)]"
```

Eindeutige Zuordnung zur Ressourcengruppe

```json
"[uniqueString(resourceGroup().id)]"
```

Eindeutige Zuordnung zur Bereitstellung für eine Ressourcengruppe

```json
"[uniqueString(resourceGroup().id, deployment().name)]"
```

Das folgende Beispiel zeigt, wie Sie einen eindeutigen Namen für ein Speicherkonto auf Grundlage seiner Ressourcengruppe erstellen. Innerhalb der Ressourcengruppe ist der Name nicht eindeutig, wenn er auf die gleiche Weise erstellt wird.

```json
"resources": [{ 
    "name": "[concat('storage', uniqueString(resourceGroup().id))]", 
    "type": "Microsoft.Storage/storageAccounts", 
    ...
```

### <a name="return-value"></a>Rückgabewert

Eine Zeichenfolge mit 13 Zeichen

<a id="uri" />

## <a name="uri"></a>uri
`uri (baseUri, relativeUri)`

Erstellt einen absoluten URI durch Kombinieren der baseUri- und der relativeUri-Zeichenfolge.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| baseUri |Ja |string |Die Zeichenfolge mit dem Basis-URI. |
| relativeUri |Ja |string |Der Zeichenfolge mit dem relativen URI, die der Zeichenfolge mit dem Basis-URI hinzugefügt werden soll. |

Der Wert für den **baseUri** -Parameter kann eine bestimmte Datei enthalten, beim Erstellen des URI wird jedoch nur der Basispfad verwendet. Beispielsweise führt das Übergeben von `http://contoso.com/resources/azuredeploy.json` als baseUri-Parameter zu einem Basis-URI von `http://contoso.com/resources/`.

### <a name="examples"></a>Beispiele

Im folgenden Beispiel wird veranschaulicht, wie basierend auf dem Wert der übergeordneten Vorlage eine Verknüpfung zu einer geschachtelten Vorlage erstellt wird.

```json
"templateLink": "[uri(deployment().properties.templateLink.uri, 'nested/azuredeploy.json')]"
```

Im folgenden Beispiel wird die Verwendung von „uri“, „uriComponent“ und „uriComponentToString“ gezeigt:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "variables": {
        "uriFormat": "[uri('http://contoso.com/resources/', 'nested/azuredeploy.json')]",
        "uriEncoded": "[uriComponent(variables('uriFormat'))]" 
    },
    "resources": [
    ],
    "outputs": {
        "uriOutput": {
            "type": "string",
            "value": "[variables('uriFormat')]"
        },
        "componentOutput": {
            "type": "string",
            "value": "[variables('uriEncoded')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[uriComponentToString(variables('uriEncoded'))]"
        }
    }
}
```

### <a name="return-value"></a>Rückgabewert

Eine Zeichenfolge, die den absoluten URI für den Basis- und relativen URI-Wert darstellt.

<a id="uricomponent" />

## <a name="uricomponent"></a>uriComponent
`uricomponent(stringToEncode)`

Codiert einen URI.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| stringToEncode |Ja |string |Der zu codierende Wert. |

### <a name="examples"></a>Beispiele

Im folgenden Beispiel wird die Verwendung von „uri“, „uriComponent“ und „uriComponentToString“ gezeigt:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "variables": {
        "uriFormat": "[uri('http://contoso.com/resources/', 'nested/azuredeploy.json')]",
        "uriEncoded": "[uriComponent(variables('uriFormat'))]" 
    },
    "resources": [
    ],
    "outputs": {
        "uriOutput": {
            "type": "string",
            "value": "[variables('uriFormat')]"
        },
        "componentOutput": {
            "type": "string",
            "value": "[variables('uriEncoded')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[uriComponentToString(variables('uriEncoded'))]"
        }
    }
}
```

### <a name="return-value"></a>Rückgabewert

Eine Zeichenfolge des als URI codierten Werts.

<a id="uricomponenttostring" />

## <a name="uricomponenttostring"></a>uriComponentToString
`uriComponentToString(uriEncodedString)`

Gibt eine Zeichenfolge des als URI codierten Werts zurück.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| uriEncodedString |Ja |string |Der als URI codierte Wert, der in eine Zeichenfolge konvertiert werden soll. |

### <a name="examples"></a>Beispiele

Im folgenden Beispiel wird die Verwendung von „uri“, „uriComponent“ und „uriComponentToString“ gezeigt:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "variables": {
        "uriFormat": "[uri('http://contoso.com/resources/', 'nested/azuredeploy.json')]",
        "uriEncoded": "[uriComponent(variables('uriFormat'))]" 
    },
    "resources": [
    ],
    "outputs": {
        "uriOutput": {
            "type": "string",
            "value": "[variables('uriFormat')]"
        },
        "componentOutput": {
            "type": "string",
            "value": "[variables('uriEncoded')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[uriComponentToString(variables('uriEncoded'))]"
        }
    }
}
```

### <a name="return-value"></a>Rückgabewert

Eine decodierte Zeichenfolge des als URI codierten Werts.

## <a name="next-steps"></a>Nächste Schritte
* Eine Beschreibung der Abschnitte in einer Azure Resource Manager-Vorlage finden Sie unter [Erstellen von Azure Resource Manager-Vorlagen](resource-group-authoring-templates.md).
* Informationen zum Zusammenführen mehrerer Vorlagen finden Sie unter [Verwenden von verknüpften Vorlagen bei der Bereitstellung von Azure-Ressourcen](resource-group-linked-templates.md).
* Informationen dazu, wie Sie beim Erstellen eines Ressourcentyps eine bestimmte Anzahl von Durchläufen ausführen, finden Sie unter [Erstellen mehrerer Instanzen von Ressourcen im Azure-Ressourcen-Manager](resource-group-create-multiple.md).
* Informationen zum Bereitstellen der erstellten Vorlage finden Sie unter [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen ](resource-group-template-deploy.md).


