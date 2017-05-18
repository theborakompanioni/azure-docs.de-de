---
title: "Azure Resource Manager-Vorlagen – Objekte als Parameter | Microsoft-Dokumentation"
description: "Beschreibt das Erweitern der Funktionalität der Azure Resource Manager-Vorlagen für die Verwendung von Objekten als Parameter."
services: azure-resource-manager, guidance
documentationcenter: na
author: petertay
manager: christb
editor: 
ms.service: guidance
ms.topic: article
ms.date: 05/01/2017
ms.author: mspnp
ms.translationtype: Human Translation
ms.sourcegitcommit: 9568210d4df6cfcf5b89ba8154a11ad9322fa9cc
ms.openlocfilehash: 617c24ea999aef78696ff08add4b9616e3dac589
ms.contentlocale: de-de
ms.lasthandoff: 05/15/2017


---

# <a name="patterns-for-extending-the-functionality-of-azure-resource-manager-templates---objects-as-parameters"></a>Muster zum Erweitern der Funktionalität der Azure Resource Manager-Vorlagen – Objekte als Parameter

Azure Resource Manager-Vorlagen unterstützen das Angeben von Werten mit Parametern, um eine Ressourcenbereitstellung anzupassen. Dieses Feature ist nützlich und erlaubt Ihnen, komplexe Bereitstellungen zu erstellen, doch eine einzelne Vorlage ist auf 255 Parameter beschränkt. Wenn Sie bei einer großen Bereitstellung für jede Eigenschaft in einer Ressource einen Parameter verwenden, könnten Ihnen die Parameter ausgehen.

## <a name="create-object-as-parameter"></a>Erstellen von Objekten als Parameter

Eine Möglichkeit, dieses Problem zu beheben, ist die Verwendung eines Objekts als Parameter. Das Muster besteht darin, dass Sie einen Parameter als Objekt in Ihrer Vorlage angeben und dann das Objekt als Wert oder Array von Werten bereitstellen. Mit der `parameter()`-Funktion und dem Punktoperator verweisen Sie auf seine Untereigenschaften. Beispiel:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
      "VNetSettings":{"type":"object"}
  },
  "variables": {},
  "resources": [
    {
      "apiVersion": "2015-06-15",
      "type": "Microsoft.Network/virtualNetworks",
      "name": "[parameters('VNetSettings').name]",
      "location":"[resourceGroup().location]",
      "properties": {
          "addressSpace":{
              "addressPrefixes": [
                  "[parameters('VNetSettings').addressPrefixes[0].addressPrefix]"
              ]
          },
          "subnets":[
              {
                  "name":"[parameters('VNetSettings').subnets[0].name]",
                  "properties": {
                      "addressPrefix": "[parameters('VNetSettings').subnets[0].addressPrefix]"
                  }
              },
              {
                  "name":"[parameters('VNetSettings').subnets[1].name]",
                  "properties": {
                      "addressPrefix": "[parameters('VNetSettings').subnets[1].addressPrefix]"
                  }
              }
          ]
        }
    }
  ],          
  "outputs": {}
}

```

Die entsprechende Parameterdatei sieht wie folgt aus:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters":{ 
      "VNetSettings":{
          "value":{
              "name":"VNet1",
              "addressPrefixes": [
                  { 
                      "name": "firstPrefix",
                      "addressPrefix": "10.0.0.0/22"
                  }
              ],
              "subnets":[
                  {
                      "name": "firstSubnet",
                      "addressPrefix": "10.0.0.0/24"
                  },
                  {
                      "name":"secondSubnet",
                      "addressPrefix":"10.0.1.0/24"
                  }
              ]
          }
      }
  }
}
```

In diesem Beispiel stammen alle für das VNet angegebenen Werte von dem einzelnen Parameter `VNetSettings`. Dieses Muster ist hilfreich für die Verwaltung von Eigenschaftenwerten, da Sie alle Werte für eine bestimmte Ressource in einem einzelnen Objekt beibehalten. In diesem Beispiel wird ein Objekt als Parameter verwendet, doch Sie können auch ein Array von Objekten als Parameter verwenden. Sie verweisen mit einem Index für das Array auf die Objekte.

## <a name="use-object-instead-of-multiple-arrays"></a>Verwenden eines Objekts anstelle mehrerer Arrays

Sie haben vielleicht ein ähnliches Muster verwendet, um mehrere Instanzen einer Ressource durch Erstellen mehrerer Arrays von Eigenschaftenwerten und Durchlaufen jedes Arrays, um den Wert auszuwählen, zu erstellen. Dieses Muster funktioniert gut beim Erstellen mehrerer Ressourcen des gleichen Typs, kann aber problematisch sein, wenn es zum Erstellen untergeordneter Ressourcen verwendet wird. 

Es gibt zwei Gründe für dieses Problem. Erstens versucht der Resource Manager, untergeordnete Ressourcen parallel bereitzustellen, aber bei Ihrer Bereitstellung tritt ein Fehler auf, wenn zwei untergeordnete Ressourcen gleichzeitig das übergeordnete Element aktualisieren. 

Zweitens werden die Arrays von Eigenschaftenwerten parallel durchlaufen, und wenn die Formen der Arrays nicht identisch sind, tritt ein Fehler auf. Betrachten Sie beispielsweise die folgenden Arrays von Eigenschaftenwerten:

```json
"variables": {
    "firstProperty": [
        {
            "name": "A",
            "type": "typeA"
        },
        {
            "name": "B",
            "type": "typeB"
        },
        {
            "name": "C",
            "type": "typeC"
        }
    ],
    "secondProperty": [
        "one","two"
    ]
}
```

Das typische Muster der Zuweisung dieser Werte zu Eigenschaften in einer Kopierschleife ist der Zugriff auf die Eigenschaft mit der `variables()`-Funktion und die Nutzung von `copyIndex()` als Index für das Array. Beispiel:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    ...
    "copy": {
        "name": "copyLoop1",
        "count": "[length(variables('firstProperty'))]"
    },
    ...
    "properties": {
        "name": { "value": "[variables('firstProperty')[copyIndex()].name]" },
        "type": { "value": "[variables('firstProperty')[copyIndex()].type]" },
        "number": { "value": "[variables('secondProperty')[copyIndex()]]" }
    }
}
```
Beachten Sie, dass `count` der Kopierschleife von der Anzahl der Eigenschaften im `firstProperty`-Array abhängt. Es ist jedoch nicht die gleiche Anzahl von Eigenschaften im `secondProperty`-Array. Bei der Überprüfung für diese Vorlage tritt ein Fehler auf, da die Länge des `secondProperty`-Arrays nicht identisch ist.

Wenn Sie jedoch alle Eigenschaften in ein einzelnes Objekt einbeziehen, können Sie viel einfacher erkennen, dass ein Wert fehlt. Beispiel:

```json
"variables": {
    "propertyObject": [
        {
            "name": "A",
            "type": "typeA",
            "number": "one"
        },
        {
            "name": "B",
            "type": "typeB",
            "number": "two"
        },
        {
            "name": "C",
            "type": "typeC"
        }
    ]
}
```

## <a name="try-the-template"></a>Testen der Vorlage

Befolgen Sie die nachfolgenden Schritte, wenn Sie mit diesen Vorlagen experimentieren möchten:

1.    Wechseln Sie zum Azure-Portal, wählen Sie das Pluszeichen „+“ aus, und suchen Sie nach dem Ressourcentyp „Vorlagenbereitstellung“. Wenn Sie ihn in den Suchergebnissen finden, wählen Sie ihn aus.
2.    Wenn Sie zur Seite „Vorlagenbereitstellung“ gelangen, wählen Sie die Schaltfläche **Erstellen** aus. Mit dieser Schaltfläche öffnen Sie das Blatt „Benutzerdefinierte Bereitstellung“.
3.    Wählen Sie die Schaltfläche **Vorlage bearbeiten**.
4.    Löschen Sie die leere Vorlage im rechten Bereich.
5.    Kopieren Sie die Beispielvorlage, und fügen Sie sie im rechten Bereich ein.
6.    Wählen Sie die Schaltfläche **Speichern** aus.
7.    Wenn Sie in den Bereich „Benutzerdefinierte Bereitstellung“ zurückgekehrt sind, wählen Sie die Schaltfläche **Parameter bearbeiten** aus.
8.  Löschen Sie auf dem Blatt „Parameter bearbeiten“ die vorhandene Vorlage.
9.  Kopieren Sie die Beispielparametervorlage, und fügen Sie sie ein.
10. Wählen Sie die Schaltfläche **Speichern**, sodass Sie wieder zu dem Blatt „Benutzerdefinierte Bereitstellung“ gelangen.
11. Wählen Sie auf dem Blatt „Benutzerdefinierte Bereitstellung“ Ihr Abonnement aus, erstellen Sie entweder eine neue oder verwenden Sie eine vorhandene Ressourcengruppe, und wählen Sie einen Speicherort aus. Lesen Sie die Nutzungsbedingungen, und aktivieren Sie das Kontrollkästchen „Ich stimme zu“.
12.    Wählen Sie die Schaltfläche **Kaufen** aus.

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie mehr als die maximal zulässigen 255 Parameter pro Bereitstellung benötigen, verwenden Sie dieses Muster, um weniger Parameter in Ihrer Vorlage anzugeben. Sie können dieses Muster auch verwenden, um die Eigenschaften für die Ressourcen einfacher zu verwalten und sie dann ohne Konflikte mit dem Muster der sequenziellen Schleife bereitzustellen.

* Eine Einführung in die `parameter()`-Funktion und das Verwenden von Arrays finden Sie unter [Vorlagenfunktionen im Azure Resource Manager](resource-group-template-functions.md).
* Dieses Muster ist auch im [Vorlagenbaustein-Projekt](https://github.com/mspnp/template-building-blocks) und in den [Azure-Referenzarchitekturen](/azure/architecture/reference-architectures/) implementiert.
