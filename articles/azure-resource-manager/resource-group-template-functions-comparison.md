---
title: "Azure Resource Manager-Vorlagenfunktionen – Vergleich | Microsoft Docs"
description: "Hier werden die Funktionen beschrieben, die in einer Azure Resource Manager-Vorlage zum Vergleichen von Werten verwendet werden können."
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
ms.openlocfilehash: 7f19efa7e09b0dce43851019f94285b2887c46d5
ms.contentlocale: de-de
ms.lasthandoff: 04/28/2017


---
# <a name="comparison-functions-for-azure-resource-manager-templates"></a>Vergleichsfunktionen für Azure Resource Manager-Vorlagen

Resource Manager stellt mehrere Funktionen zum Durchführen von Vergleichen in Vorlagen bereit.

* [equals](#equals)
* [less](#less)
* [lessOrEquals](#lessorequals)
* [greater](#greater)
* [greaterOrEquals](#greaterorequals)

<a id="equals" />

## <a name="equals"></a>equals
`equals(arg1, arg2)`

Überprüft, ob zwei Werte einander entsprechen.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |Ganze Zahl, Zeichenfolge, Array oder Objekt |Der erste auf Gleichheit zu überprüfende Wert. |
| arg2 |Ja |Ganze Zahl, Zeichenfolge, Array oder Objekt |Der zweite auf Gleichheit zu überprüfende Wert. |

### <a name="examples"></a>Beispiele

Die Beispielvorlage überprüft verschiedene Werttypen auf Gleichheit. Alle Standardwerte geben „True“ zurück.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstInt": {
            "type": "int",
            "defaultValue": 1
        },
        "secondInt": {
            "type": "int",
            "defaultValue": 1
        },
        "firstString": {
            "type": "string",
            "defaultValue": "a"
        },
        "secondString": {
            "type": "string",
            "defaultValue": "a"
        },
        "firstArray": {
            "type": "array",
            "defaultValue": ["a", "b"]
        },
        "secondArray": {
            "type": "array",
            "defaultValue": ["a", "b"]
        },
        "firstObject": {
            "type": "object",
            "defaultValue": {"a": "b"}
        },
        "secondObject": {
            "type": "object",
            "defaultValue": {"a": "b"}
        }
    },
    "resources": [
    ],
    "outputs": {
        "checkInts": {
            "type": "bool",
            "value": "[equals(parameters('firstInt'), parameters('secondInt') )]"
        },
        "checkStrings": {
            "type": "bool",
            "value": "[equals(parameters('firstString'), parameters('secondString'))]"
        },
        "checkArrays": {
            "type": "bool",
            "value": "[equals(parameters('firstArray'), parameters('secondArray'))]"
        },
        "checkObjects": {
            "type": "bool",
            "value": "[equals(parameters('firstObject'), parameters('secondObject'))]"
        }
    }
}
```

### <a name="return-value"></a>Rückgabewert

Gibt **True** zurück, wenn die Werte gleich sind. Andernfalls wird **False** zurückgegeben.

<a id="less" />

## <a name="less"></a>less
`less(arg1, arg2)`

Überprüft, ob der erste Wert kleiner als der zweite Wert ist.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |Ganz Zahl oder Zeichenfolge |Der erste Wert für den Vergleich vom Typ „Kleiner als“. |
| arg2 |Ja |Ganz Zahl oder Zeichenfolge |Der zweite Wert für den Vergleich vom Typ „Kleiner als“. |

### <a name="examples"></a>Beispiele

Die Beispielvorlage überprüft, ob der eine Wert kleiner als der andere Wert ist.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstInt": {
            "type": "int"
        },
        "secondInt": {
            "type": "int"
        },
        "firstString": {
            "type": "string"
        },
        "secondString": {
            "type": "string"
        }
    },
    "resources": [
    ],
    "outputs": {
        "checkInts": {
            "type": "bool",
            "value": "[less(parameters('firstInt'), parameters('secondInt') )]"
        },
        "checkStrings": {
            "type": "bool",
            "value": "[less(parameters('firstString'), parameters('secondString'))]"
        }
    }
}
```

### <a name="return-value"></a>Rückgabewert

Gibt **True** zurück, wenn der erste Wert kleiner als der zweite Wert ist. Andernfalls wird **False** zurückgegeben.

<a id="lessorequals" />

## <a name="lessorequals"></a>lessOrEquals
`lessOrEquals(arg1, arg2)`

Überprüft, ob der erste Wert kleiner als oder gleich Wert 2 ist.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |Ganz Zahl oder Zeichenfolge |Der erste Wert für den Vergleich vom Typ „Kleiner als oder gleich“. |
| arg2 |Ja |Ganz Zahl oder Zeichenfolge |Der zweite Wert für den Vergleich vom Typ „Kleiner als oder gleich“. |

### <a name="examples"></a>Beispiele

Die Beispielvorlage überprüft, ob der eine Wert kleiner als der andere Wert ist oder diesem entspricht.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstInt": {
            "type": "int"
        },
        "secondInt": {
            "type": "int"
        },
        "firstString": {
            "type": "string"
        },
        "secondString": {
            "type": "string"
        }
    },
    "resources": [
    ],
    "outputs": {
        "checkInts": {
            "type": "bool",
            "value": "[lessOrEquals(parameters('firstInt'), parameters('secondInt') )]"
        },
        "checkStrings": {
            "type": "bool",
            "value": "[lessOrEquals(parameters('firstString'), parameters('secondString'))]"
        }
    }
}
```

### <a name="return-value"></a>Rückgabewert

Gibt **True** zurück, wenn der erste Wert kleiner als oder gleich Wert 2 ist. Andernfalls wird **False** zurückgegeben.

<a id="greater" />

## <a name="greater"></a>greater
`greater(arg1, arg2)`

Überprüft, ob der erste Wert größer als der zweite Wert ist.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |Ganz Zahl oder Zeichenfolge |Der erste Wert für den Vergleich vom Typ „Größer als“. |
| arg2 |Ja |Ganz Zahl oder Zeichenfolge |Der zweite Wert für den Vergleich vom Typ „Größer als“. |

### <a name="examples"></a>Beispiele

Die Beispielvorlage überprüft, ob der eine Wert größer als der andere Wert ist.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstInt": {
            "type": "int"
        },
        "secondInt": {
            "type": "int"
        },
        "firstString": {
            "type": "string"
        },
        "secondString": {
            "type": "string"
        }
    },
    "resources": [
    ],
    "outputs": {
        "checkInts": {
            "type": "bool",
            "value": "[greater(parameters('firstInt'), parameters('secondInt') )]"
        },
        "checkStrings": {
            "type": "bool",
            "value": "[greater(parameters('firstString'), parameters('secondString'))]"
        }
    }
}
```

### <a name="return-value"></a>Rückgabewert

Gibt **True** zurück, wenn der erste Wert größer als der zweite Wert ist. Andernfalls wird **False** zurückgegeben.

<a id="greaterorequals" />

## <a name="greaterorequals"></a>greaterOrEquals
`greaterOrEquals(arg1, arg2)`

Überprüft, ob der erste Wert größer als oder gleich Wert 2 ist.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |Ganz Zahl oder Zeichenfolge |Der erste Wert für den Vergleich vom Typ „Größer als oder gleich“. |
| arg2 |Ja |Ganz Zahl oder Zeichenfolge |Der zweite Wert für den Vergleich vom Typ „Größer als oder gleich“. |

### <a name="examples"></a>Beispiele

Die Beispielvorlage überprüft, ob der eine Wert größer als der andere Wert ist oder diesem entspricht.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstInt": {
            "type": "int"
        },
        "secondInt": {
            "type": "int"
        },
        "firstString": {
            "type": "string"
        },
        "secondString": {
            "type": "string"
        }
    },
    "resources": [
    ],
    "outputs": {
        "checkInts": {
            "type": "bool",
            "value": "[greaterOrEquals(parameters('firstInt'), parameters('secondInt') )]"
        },
        "checkStrings": {
            "type": "bool",
            "value": "[greaterOrEquals(parameters('firstString'), parameters('secondString'))]"
        }
    }
}
```

### <a name="return-value"></a>Rückgabewert

Gibt **True** zurück, wenn der erste Wert größer als oder gleich Wert 2 ist. Andernfalls wird **False** zurückgegeben.

## <a name="next-steps"></a>Nächste Schritte
* Eine Beschreibung der Abschnitte in einer Azure Resource Manager-Vorlage finden Sie unter [Erstellen von Azure Resource Manager-Vorlagen](resource-group-authoring-templates.md).
* Informationen zum Zusammenführen mehrerer Vorlagen finden Sie unter [Verwenden von verknüpften Vorlagen bei der Bereitstellung von Azure-Ressourcen](resource-group-linked-templates.md).
* Informationen dazu, wie Sie beim Erstellen eines Ressourcentyps eine bestimmte Anzahl von Durchläufen ausführen, finden Sie unter [Erstellen mehrerer Instanzen von Ressourcen im Azure-Ressourcen-Manager](resource-group-create-multiple.md).
* Informationen zum Bereitstellen der erstellten Vorlage finden Sie unter [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen ](resource-group-template-deploy.md).


