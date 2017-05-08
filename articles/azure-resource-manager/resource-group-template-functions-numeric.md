---
title: "Azure Resource Manager-Vorlagenfunktionen – Numerische Funktionen | Microsoft Docs"
description: "Hier werden die Funktionen beschrieben, die in einer Azure Resource Manager-Vorlage zum Arbeiten mit Zahlen verwendet werden können."
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
ms.date: 04/26/2017
ms.author: tomfitz
ms.translationtype: Human Translation
ms.sourcegitcommit: 54b5b8d0040dc30651a98b3f0d02f5374bf2f873
ms.openlocfilehash: 5844540801a6f0ff593b3f404f6815473c65a52e
ms.contentlocale: de-de
ms.lasthandoff: 04/28/2017


---
# <a name="numeric-functions-for-azure-resource-manager-templates"></a>Numerische Funktionen für Azure Resource Manager-Vorlagen

Der Ressourcen-Manager stellt die folgenden Funktionen für das Arbeiten mit ganzen Zahlen bereit:

* [Hinzufügen](#add)
* [copyIndex](#copyindex)
* [div](#div)
* [float](#float)
* [int](#int)
* [min](#min)
* [max](#max)
* [mod](#mod)
* [mul](#mul)
* [sub](#sub)

<a id="add" />

## <a name="add"></a>Hinzufügen
`add(operand1, operand2)`

Gibt die Summe der beiden angegebenen ganzen Zahlen zurück.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- | 
|operand1 |Ja |int |Erste zu addierende Zahl. |
|operand2 |Ja |int |Zweite zu addierende Zahl. |

### <a name="examples"></a>Beispiele

Im folgenden Beispiel werden zwei Parameter hinzugefügt.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
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
    "resources": [
    ],
    "outputs": {
        "addResult": {
            "type": "int",
            "value": "[add(parameters('first'), parameters('second'))]"
        }
    }
}
```

### <a name="return-value"></a>Rückgabewert

Eine ganze Zahl, die die Summe der Parameter enthält.

<a id="copyindex" />

## <a name="copyindex"></a>copyIndex
`copyIndex(offset)`

Gibt den Index einer Iterationsschleife zurück. 

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| offset |Nein |int |Die Zahl, die dem nullbasierten (0) Iterationswert hinzugefügt werden soll. |

### <a name="remarks"></a>Anmerkungen

Diese Funktion wird immer mit einem **copy** -Objekt verwendet. Wenn kein Wert für **offset** angegeben wird, wird der aktuelle Iterationswert zurückgegeben. Der Iterationswert beginnt bei 0 (null). Eine vollständige Beschreibung der Nutzung von **copyIndex**finden Sie unter [Erstellen mehrerer Instanzen von Ressourcen im Azure-Ressourcen-Manager](resource-group-create-multiple.md).

### <a name="examples"></a>Beispiele

Das folgende Beispiel enthält eine Kopierschleife und den Indexwert im Namen. 

```json
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
```

### <a name="return-value"></a>Rückgabewert

Eine ganze Zahl, die den aktuellen Index der Iteration darstellt.

<a id="div" />

## <a name="div"></a>div
`div(operand1, operand2)`

Gibt die Ganzzahldivision der beiden angegebenen ganzen Zahlen zurück.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| operand1 |Ja |int |Die zu teilende Zahl (Dividend). |
| operand2 |Ja |int |Die Zahl, durch die geteilt wird (Divisor). Diese Zahl darf nicht 0 sein. |

### <a name="examples"></a>Beispiele

Im folgenden Beispiel wird ein Parameter durch einen anderen Parameter dividiert.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
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
    "resources": [
    ],
    "outputs": {
        "divResult": {
            "type": "int",
            "value": "[div(parameters('first'), parameters('second'))]"
        }
    }
}
```

### <a name="return-value"></a>Rückgabewert

Eine ganze Zahl, die die Division darstellt.

<a id="float" />

## <a name="float"></a>float
`float(arg1)`

Konvertiert den Wert in eine Gleitkommazahl. Diese Funktion wird nur beim Übergeben von benutzerdefinierten Parametern an eine Anwendung (z. B. eine Logik-App) verwendet.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |Zeichenfolge oder ganze Zahl |Der in eine Gleitkommazahl zu konvertierende Wert. |

### <a name="examples"></a>Beispiele

Das folgende Beispiel zeigt, wie „float“ zum Übergeben von Parametern an eine Logik-App verwendet wird:

```json
{
    "type": "Microsoft.Logic/workflows",
    "properties": {
        ...
        "parameters": {
        "custom1": {
            "value": "[float('3.0')]"
        },
        "custom2": {
            "value": "[float(3)]"
        },
```

### <a name="return-value"></a>Rückgabewert
Eine Gleitkommazahl.

<a id="int" />

## <a name="int"></a>int
`int(valueToConvert)`

Konvertiert den angegebenen Wert in eine ganze Zahl (Integer).

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| valueToConvert |Ja |Zeichenfolge oder ganze Zahl |Der Wert, der in eine ganze Zahl (Integer) konvertiert werden soll. |

### <a name="examples"></a>Beispiele

Im folgenden Beispiel wird der vom Benutzer angegebene Parameterwert in eine ganze Zahl konvertiert.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "appId": { "type": "string" }
    },
    "variables": { 
        "intValue": "[int(parameters('appId'))]"
    },
    "resources": [
    ],
    "outputs": {
        "divResult": {
            "type": "int",
            "value": "[variables('intValue')]"
        }
    }
}
```

### <a name="return-value"></a>Rückgabewert

Eine ganze Zahl.

<a id="min" />

## <a name="min"></a>Min
`min (arg1)`

Gibt den kleinsten Wert aus einem Array mit ganzen Zahlen oder einer durch Trennzeichen getrennten Liste mit ganzen Zahlen zurück.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |Array mit ganzen Zahlen oder durch Trennzeichen getrennte Liste mit ganzen Zahlen |Die Auflistung, aus der der kleinste Wert abgerufen werden soll. |

### <a name="examples"></a>Beispiele

Im folgenden Beispiel wird gezeigt, wie „min“ mit einem Array und einer Liste mit ganzen Zahlen verwendet wird:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": [0,3,2,5,4]
        }
    },
    "resources": [],
    "outputs": {
        "arrayOutput": {
            "type": "int",
            "value": "[min(parameters('arrayToTest'))]"
        },
        "intOutput": {
            "type": "int",
            "value": "[min(0,3,2,5,4)]"
        }
    }
}
```

### <a name="return-value"></a>Rückgabewert

Eine ganze Zahl, die den kleinsten Wert aus der Auflistung darstellt.

<a id="max" />

## <a name="max"></a>max
`max (arg1)`

Gibt den größten Wert aus einem Array mit ganzen Zahlen oder einer durch Trennzeichen getrennten Liste mit ganzen Zahlen zurück.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |Array mit ganzen Zahlen oder durch Trennzeichen getrennte Liste mit ganzen Zahlen |Die Auflistung, aus der der größte Wert abgerufen werden soll. |

### <a name="examples"></a>Beispiele

Im folgenden Beispiel wird gezeigt, wie „max“ mit einem Array und einer Liste mit ganzen Zahlen verwendet wird:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": [0,3,2,5,4]
        }
    },
    "resources": [],
    "outputs": {
        "arrayOutput": {
            "type": "int",
            "value": "[max(parameters('arrayToTest'))]"
        },
        "intOutput": {
            "type": "int",
            "value": "[max(0,3,2,5,4)]"
        }
    }
}
```

### <a name="return-value"></a>Rückgabewert

Eine ganze Zahl, die den größten Wert aus der Auflistung darstellt.

<a id="mod" />

## <a name="mod"></a>mod
`mod(operand1, operand2)`

Gibt den Rest der Ganzzahldivision mit den beiden angegebenen ganzen Zahlen zurück.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| operand1 |Ja |int |Die zu teilende Zahl (Dividend). |
| operand2 |Ja |int |Die Zahl, durch die dividiert wird (Divisor), darf nicht null (0) sein. |

### <a name="examples"></a>Beispiele

Im folgenden Beispiel wird der Restbetrag der Division von einem Parameter durch einen anderen Parameter zurückgegeben.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
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
    "resources": [
    ],
    "outputs": {
        "modResult": {
            "type": "int",
            "value": "[mod(parameters('first'), parameters('second'))]"
        }
    }
}
```

### <a name="return-value"></a>Rückgabewert
Eine ganze Zahl, die den Rest darstellt.

<a id="mul" />

## <a name="mul"></a>mul
`mul(operand1, operand2)`

Gibt die Multiplikation der beiden angegebenen ganzen Zahlen zurück.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| operand1 |Ja |int |Erste zu multiplizierende Zahl. |
| operand2 |Ja |int |Zweite zu multiplizierende Zahl. |

### <a name="examples"></a>Beispiele

Im folgenden Beispiel wird ein Parameter mit einem anderen Parameter multipliziert.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
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
    "resources": [
    ],
    "outputs": {
        "mulResult": {
            "type": "int",
            "value": "[mul(parameters('first'), parameters('second'))]"
        }
    }
}
```

### <a name="return-value"></a>Rückgabewert

Eine ganze Zahl, die die Multiplikation darstellt.

<a id="sub" />

## <a name="sub"></a>sub
`sub(operand1, operand2)`

Gibt die Differenz der beiden angegebenen ganzen Zahlen zurück.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| operand1 |Ja |int |Zahl, von der subtrahiert wird. |
| operand2 |Ja |int |Zahl, die subtrahiert wird. |

### <a name="examples"></a>Beispiele

Im folgenden Beispiel wird ein Parameter von einem anderen Parameter subtrahiert.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
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
    "resources": [
    ],
    "outputs": {
        "subResult": {
            "type": "int",
            "value": "[sub(parameters('first'), parameters('second'))]"
        }
    }
}
```

### <a name="return-value"></a>Rückgabewert
Eine ganze Zahl, die die Subtraktion darstellt.

## <a name="next-steps"></a>Nächste Schritte
* Eine Beschreibung der Abschnitte in einer Azure Resource Manager-Vorlage finden Sie unter [Erstellen von Azure Resource Manager-Vorlagen](resource-group-authoring-templates.md).
* Informationen zum Zusammenführen mehrerer Vorlagen finden Sie unter [Verwenden von verknüpften Vorlagen bei der Bereitstellung von Azure-Ressourcen](resource-group-linked-templates.md).
* Informationen dazu, wie Sie beim Erstellen eines Ressourcentyps eine bestimmte Anzahl von Durchläufen ausführen, finden Sie unter [Erstellen mehrerer Instanzen von Ressourcen im Azure-Ressourcen-Manager](resource-group-create-multiple.md).
* Informationen zum Bereitstellen der erstellten Vorlage finden Sie unter [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen ](resource-group-template-deploy.md).


