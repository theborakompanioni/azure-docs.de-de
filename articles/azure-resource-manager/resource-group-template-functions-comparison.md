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
ms.date: 08/01/2017
ms.author: tomfitz
ms.translationtype: HT
ms.sourcegitcommit: c30998a77071242d985737e55a7dc2c0bf70b947
ms.openlocfilehash: 521e5ed06c138bcd374913588f06a2e6c1e99963
ms.contentlocale: de-de
ms.lasthandoff: 08/02/2017

---
# <a name="comparison-functions-for-azure-resource-manager-templates"></a>Vergleichsfunktionen für Azure Resource Manager-Vorlagen

Resource Manager stellt mehrere Funktionen zum Durchführen von Vergleichen in Vorlagen bereit.

* [equals](#equals)
* [greater](#greater)
* [greaterOrEquals](#greaterorequals)
* [less](#less)
* [lessOrEquals](#lessorequals)

## <a name="equals"></a>equals
`equals(arg1, arg2)`

Überprüft, ob zwei Werte einander entsprechen.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |Ganze Zahl, Zeichenfolge, Array oder Objekt |Der erste auf Gleichheit zu überprüfende Wert. |
| arg2 |Ja |Ganze Zahl, Zeichenfolge, Array oder Objekt |Der zweite auf Gleichheit zu überprüfende Wert. |

### <a name="return-value"></a>Rückgabewert

Gibt **True** zurück, wenn die Werte gleich sind. Andernfalls wird **False** zurückgegeben.

### <a name="remarks"></a>Anmerkungen

Die Funktion „equals“ wird häufig mit dem Element `condition` verwendet, um zu prüfen, ob eine Ressource bereitgestellt wurde.

```json
{
    "condition": "[equals(parameters('newOrExisting'),'new')]",
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2017-06-01",
    "location": "[resourceGroup().location]",
    "sku": {
        "name": "[variables('storageAccountType')]"
    },
    "kind": "Storage",
    "properties": {}
}
```

### <a name="example"></a>Beispiel

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

Die Ausgabe aus dem vorherigen Beispiel mit den Standardwerten lautet:

| Name | Typ | Wert |
| ---- | ---- | ----- |
| checkInts | Bool | True  |
| checkStrings | Bool | True  |
| checkArrays | Bool | True  |
| checkObjects | Bool | true |


Im folgenden Beispiel wird [not](resource-group-template-functions-logical.md#not) mit **equals** verwendet.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
    ],
    "outputs": {
        "checkNotEquals": {
            "type": "bool",
            "value": "[not(equals(1, 2))]"
        }
    }
```

Die Ausgabe aus dem vorherigen Beispiel lautet wie folgt:

| Name | Typ | Wert |
| ---- | ---- | ----- |
| checkNotEquals | Bool | true |


## <a name="greater"></a>greater
`greater(arg1, arg2)`

Überprüft, ob der erste Wert größer als der zweite Wert ist.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |Ganz Zahl oder Zeichenfolge |Der erste Wert für den Vergleich vom Typ „Größer als“. |
| arg2 |Ja |Ganz Zahl oder Zeichenfolge |Der zweite Wert für den Vergleich vom Typ „Größer als“. |

### <a name="return-value"></a>Rückgabewert

Gibt **True** zurück, wenn der erste Wert größer als der zweite Wert ist. Andernfalls wird **False** zurückgegeben.

### <a name="example"></a>Beispiel

Die Beispielvorlage überprüft, ob der eine Wert größer als der andere Wert ist.

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
            "defaultValue": 2
        },
        "firstString": {
            "type": "string",
            "defaultValue": "A"
        },
        "secondString": {
            "type": "string",
            "defaultValue": "a"
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

Die Ausgabe aus dem vorherigen Beispiel mit den Standardwerten lautet:

| Name | Typ | Wert |
| ---- | ---- | ----- |
| checkInts | Bool | False |
| checkStrings | Bool | True  |


## <a name="greaterorequals"></a>greaterOrEquals
`greaterOrEquals(arg1, arg2)`

Überprüft, ob der erste Wert größer als oder gleich Wert 2 ist.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |Ganz Zahl oder Zeichenfolge |Der erste Wert für den Vergleich vom Typ „Größer als oder gleich“. |
| arg2 |Ja |Ganz Zahl oder Zeichenfolge |Der zweite Wert für den Vergleich vom Typ „Größer als oder gleich“. |

### <a name="return-value"></a>Rückgabewert

Gibt **True** zurück, wenn der erste Wert größer als oder gleich Wert 2 ist. Andernfalls wird **False** zurückgegeben.

### <a name="example"></a>Beispiel

Die Beispielvorlage überprüft, ob der eine Wert größer als der andere Wert ist oder diesem entspricht.

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
            "defaultValue": 2
        },
        "firstString": {
            "type": "string",
            "defaultValue": "A"
        },
        "secondString": {
            "type": "string",
            "defaultValue": "a"
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

Die Ausgabe aus dem vorherigen Beispiel mit den Standardwerten lautet:

| Name | Typ | Wert |
| ---- | ---- | ----- |
| checkInts | Bool | False |
| checkStrings | Bool | True  |



## <a name="less"></a>less
`less(arg1, arg2)`

Überprüft, ob der erste Wert kleiner als der zweite Wert ist.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |Ganz Zahl oder Zeichenfolge |Der erste Wert für den Vergleich vom Typ „Kleiner als“. |
| arg2 |Ja |Ganz Zahl oder Zeichenfolge |Der zweite Wert für den Vergleich vom Typ „Kleiner als“. |

### <a name="return-value"></a>Rückgabewert

Gibt **True** zurück, wenn der erste Wert kleiner als der zweite Wert ist. Andernfalls wird **False** zurückgegeben.

### <a name="example"></a>Beispiel

Die Beispielvorlage überprüft, ob der eine Wert kleiner als der andere Wert ist.

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
            "defaultValue": 2
        },
        "firstString": {
            "type": "string",
            "defaultValue": "A"
        },
        "secondString": {
            "type": "string",
            "defaultValue": "a"
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

Die Ausgabe aus dem vorherigen Beispiel mit den Standardwerten lautet:

| Name | Typ | Wert |
| ---- | ---- | ----- |
| checkInts | Bool | True  |
| checkStrings | Bool | False |


## <a name="lessorequals"></a>lessOrEquals
`lessOrEquals(arg1, arg2)`

Überprüft, ob der erste Wert kleiner als oder gleich Wert 2 ist.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |Ganz Zahl oder Zeichenfolge |Der erste Wert für den Vergleich vom Typ „Kleiner als oder gleich“. |
| arg2 |Ja |Ganz Zahl oder Zeichenfolge |Der zweite Wert für den Vergleich vom Typ „Kleiner als oder gleich“. |

### <a name="return-value"></a>Rückgabewert

Gibt **True** zurück, wenn der erste Wert kleiner als oder gleich Wert 2 ist. Andernfalls wird **False** zurückgegeben.

### <a name="example"></a>Beispiel

Die Beispielvorlage überprüft, ob der eine Wert kleiner als der andere Wert ist oder diesem entspricht.

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
            "defaultValue": 2
        },
        "firstString": {
            "type": "string",
            "defaultValue": "A"
        },
        "secondString": {
            "type": "string",
            "defaultValue": "a"
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

Die Ausgabe aus dem vorherigen Beispiel mit den Standardwerten lautet:

| Name | Typ | Wert |
| ---- | ---- | ----- |
| checkInts | Bool | True  |
| checkStrings | Bool | False |



## <a name="next-steps"></a>Nächste Schritte
* Eine Beschreibung der Abschnitte in einer Azure Resource Manager-Vorlage finden Sie unter [Erstellen von Azure Resource Manager-Vorlagen](resource-group-authoring-templates.md).
* Informationen zum Zusammenführen mehrerer Vorlagen finden Sie unter [Verwenden von verknüpften Vorlagen bei der Bereitstellung von Azure-Ressourcen](resource-group-linked-templates.md).
* Informationen dazu, wie Sie beim Erstellen eines Ressourcentyps eine bestimmte Anzahl von Durchläufen ausführen, finden Sie unter [Erstellen mehrerer Instanzen von Ressourcen im Azure-Ressourcen-Manager](resource-group-create-multiple.md).
* Informationen zum Bereitstellen der erstellten Vorlage finden Sie unter [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen ](resource-group-template-deploy.md).


