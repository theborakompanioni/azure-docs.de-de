---
title: "Azure Resource Manager-Vorlagenfunktionen – logisch | Microsoft-Dokumentation"
description: "Hier werden die Funktionen beschrieben, die in einer Azure Resource Manager-Vorlage zum Bestimmen von logischen Werten verwendet werden können."
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
ms.openlocfilehash: 313601ad99cdc12c4b50f5469959d37a9fa70d35
ms.contentlocale: de-de
ms.lasthandoff: 08/02/2017

---
# <a name="logical-functions-for-azure-resource-manager-templates"></a>Logische Funktionen für Azure Resource Manager-Vorlagen

Resource Manager stellt mehrere Funktionen zum Durchführen von Vergleichen in Vorlagen bereit.

* [and](#and)
* [bool](#bool)
* [if](#if)
* [not](#not)
* [or](#or)

## <a name="and"></a>and
`and(arg1, arg2)`

Überprüft, ob beide Parameterwerte zutreffen.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |Boolescher Wert |Der erste Wert, für den überprüft wird, ob er zutrifft. |
| arg2 |Ja |Boolescher Wert |Der zweite Wert, für den überprüft wird, ob er zutrifft. |

### <a name="return-value"></a>Rückgabewert

Gibt **True** zurück, wenn beide Werte zutreffen. Andernfalls wird **False** zurückgegeben.

### <a name="examples"></a>Beispiele

Das folgende Beispiel zeigt die Verwendung logischer Funktionen.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [ ],
    "outputs": {
        "andExampleOutput": {
            "value": "[and(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "orExampleOutput": {
            "value": "[or(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "notExampleOutput": {
            "value": "[not(bool('true'))]",
            "type": "bool"
        }
    }
}
```

Die Ausgabe aus dem vorherigen Beispiel lautet wie folgt:

| Name | Typ | Wert |
| ---- | ---- | ----- |
| andExampleOutput | Bool | False |
| orExampleOutput | Bool | true |
| notExampleOutput | Bool | False |


## <a name="bool"></a>bool
`bool(arg1)`

Konvertiert den Parameter in einen booleschen Wert.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |Zeichenfolge oder ganze Zahl |Der Wert, der in einen booleschen Wert konvertiert werden soll. |

### <a name="return-value"></a>Rückgabewert
Ein boolescher Wert des konvertierten Werts.

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

Die Ausgabe aus dem vorherigen Beispiel mit den Standardwerten lautet:

| Name | Typ | Wert |
| ---- | ---- | ----- |
| trueString | Bool | True  |
| falseString | Bool | False |
| trueInt | Bool | True  |
| falseInt | Bool | False |

## <a name="if"></a>if
`if(condition, trueValue, falseValue)`

Gibt einen Wert abhängig davon zurück, ob eine Bedingung zutrifft oder nicht.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| condition |Ja |Boolescher Wert |Der Wert, für den überprüft wird, ob er zutrifft. |
| trueValue |Ja | Zeichenfolge, Integer, Objekt oder Array |Der zurückzugebende Wert, wenn die Bedingung zutrifft. |
| falseValue |Ja | Zeichenfolge, Integer, Objekt oder Array |Der zurückzugebende Wert, wenn die Bedingung nicht zutrifft. |

### <a name="return-value"></a>Rückgabewert

Gibt den zweiten Parameter zurück, wenn der erste Parameter **True** ist, andernfalls wird der dritte Parameter zurückgegeben.

### <a name="remarks"></a>Anmerkungen

Mit dieser Funktion können Sie eine Ressourceneigenschaft „bedingt“ festlegen. Das folgende Beispiel ist keine vollständige Vorlage, aber es zeigt die relevanten Abschnitte für das bedingte Festlegen der Verfügbarkeitsgruppe.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        ...
        "availabilitySet": {
            "type": "string",
            "allowedValues": [
                "yes",
                "no"
            ]
        }
    },
    "variables": {
        ...
        "availabilitySetName": "availabilitySet1",
        "availabilitySet": {
            "id": "[resourceId('Microsoft.Compute/availabilitySets',variables('availabilitySetName'))]"
        }
    },
    "resources": [
        {
            "condition": "[equals(parameters('availabilitySet'),'yes')]",
            "type": "Microsoft.Compute/availabilitySets",
            "name": "[variables('availabilitySetName')]",
            ...
        },
        {
            "apiVersion": "2016-03-30",
            "type": "Microsoft.Compute/virtualMachines",
            "properties": {
                "availabilitySet": "[if(equals(parameters('availabilitySet'),'yes'), variables('availabilitySet'), json('null'))]",
                ...
            }
        },
        ...
    ],
    ...
}
```

### <a name="examples"></a>Beispiele

Das folgende Beispiel zeigt die Verwendung der Funktion `if`.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
    ],
    "outputs": {
        "yesOutput": {
            "type": "string",
            "value": "[if(equals('a', 'a'), 'yes', 'no')]"
        },
        "noOutput": {
            "type": "string",
            "value": "[if(equals('a', 'b'), 'yes', 'no')]"
        }
    }
}
```

Die Ausgabe aus dem vorherigen Beispiel lautet wie folgt:

| Name | Typ | Wert |
| ---- | ---- | ----- |
| yesOutput | String | Ja |
| noOutput | String | no |


## <a name="not"></a>not
`not(arg1)`

Konvertiert den booleschen Wert in seinen gegenteiligen Wert.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |Boolescher Wert |Der zu konvertierende Wert. |


### <a name="return-value"></a>Rückgabewert

Gibt **True** zurück, wenn der Parameter **False** ist. Gibt **False** zurück, wenn der Parameter **True** ist.

### <a name="examples"></a>Beispiele

Das folgende Beispiel zeigt die Verwendung logischer Funktionen.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [ ],
    "outputs": {
        "andExampleOutput": {
            "value": "[and(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "orExampleOutput": {
            "value": "[or(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "notExampleOutput": {
            "value": "[not(bool('true'))]",
            "type": "bool"
        }
    }
}
```

Die Ausgabe aus dem vorherigen Beispiel lautet wie folgt:

| Name | Typ | Wert |
| ---- | ---- | ----- |
| andExampleOutput | Bool | False |
| orExampleOutput | Bool | true |
| notExampleOutput | Bool | False |

Im folgenden Beispiel wird **not** mit [equals](resource-group-template-functions-comparison.md#equals) verwendet.

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


## <a name="or"></a>oder
`or(arg1, arg2)`

Überprüft, ob einer der Parameterwerte zutrifft.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |Boolescher Wert |Der erste Wert, für den überprüft wird, ob er zutrifft. |
| arg2 |Ja |Boolescher Wert |Der zweite Wert, für den überprüft wird, ob er zutrifft. |

### <a name="return-value"></a>Rückgabewert

Gibt **True** zurück, wenn einer der Werte zutrifft. Andernfalls wird **False** zurückgegeben.

### <a name="examples"></a>Beispiele

Das folgende Beispiel zeigt die Verwendung logischer Funktionen.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [ ],
    "outputs": {
        "andExampleOutput": {
            "value": "[and(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "orExampleOutput": {
            "value": "[or(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "notExampleOutput": {
            "value": "[not(bool('true'))]",
            "type": "bool"
        }
    }
}
```

Die Ausgabe aus dem vorherigen Beispiel lautet wie folgt:

| Name | Typ | Wert |
| ---- | ---- | ----- |
| andExampleOutput | Bool | False |
| orExampleOutput | Bool | true |
| notExampleOutput | Bool | False |


## <a name="next-steps"></a>Nächste Schritte
* Eine Beschreibung der Abschnitte in einer Azure Resource Manager-Vorlage finden Sie unter [Erstellen von Azure Resource Manager-Vorlagen](resource-group-authoring-templates.md).
* Informationen zum Zusammenführen mehrerer Vorlagen finden Sie unter [Verwenden von verknüpften Vorlagen bei der Bereitstellung von Azure-Ressourcen](resource-group-linked-templates.md).
* Informationen dazu, wie Sie beim Erstellen eines Ressourcentyps eine bestimmte Anzahl von Durchläufen ausführen, finden Sie unter [Erstellen mehrerer Instanzen von Ressourcen im Azure-Ressourcen-Manager](resource-group-create-multiple.md).
* Informationen zum Bereitstellen der erstellten Vorlage finden Sie unter [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen ](resource-group-template-deploy.md).


