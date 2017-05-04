---
title: Sequenzielle Schleifen in Azure-Vorlagen | Microsoft-Dokumentation
description: "Beschreibt die Erweiterung der Funktionalität von Azure Resource Manager-Vorlagen, um bei der Bereitstellung mehrerer Instanzen eines Ressourcentyps sequenzielle Schleifen zu implementieren."
services: guidance, azure-resource-manager
documentationcenter: na
author: petertaylor9999
manager: christb
editor: 
ms.service: guidance
ms.topic: article
ms.date: 04/18/2017
ms.author: petertay
translationtype: Human Translation
ms.sourcegitcommit: db7cb109a0131beee9beae4958232e1ec5a1d730
ms.openlocfilehash: 44d59488600e05d6ae80a169b5d682e8fa94ccc5
ms.lasthandoff: 04/18/2017


---

# <a name="patterns-for-extending-the-functionality-of-azure-resource-manager-templates---sequential-looping"></a>Muster zum Erweitern der Funktionalität von Azure Resource Manager-Vorlagen – Sequenzielle Schleifen

Azure Resource Manager-Vorlagen unterstützen die Bereitstellung einer Gruppe ähnlicher Ressourcen mithilfe einer copy-Schleife. Eine copy-Schleife in einem Ressourcenobjekt kann dazu verwendet werden, um ein Array von Zeichenfolgen zu durchlaufen, mit denen dann eindeutige Namen für die Ressourcen generiert werden. Eine copy-Schleife im Ressourcenobjekt kann auch dazu verwendet werden, um ein Array von Variablen zu durchlaufen, die die Ressource beschreiben.

Diese Muster funktionieren alle gut, aber das ist nur der Fall, wenn zwischen den einzelnen Mitgliedern der Gruppe keine Abhängigkeiten vorhanden sind. Während einer Iterationsschleife versucht der Resource Manager gleichzeitig entsprechende Ressourcen bereitzustellen. Wenn die erste Ressource von einer zweiten abhängig ist, kann bei der Bereitstellung ein Fehler auftreten, wenn der Resource Manager die zweite Ressource vor der ersten Ressource bereitstellt.

Das eigentliche Problem ist, dass der Resource Manager `dependsOn` innerhalb einer Iterationsschleife derzeit nicht unterstützt. Diese Funktionalität kann jedoch mithilfe vorhandener Resource Manager-Funktionen und einer kreativen Ressourcenbennenung implementiert werden. 

## <a name="sequential-looping-pattern"></a>Sequenzielle Schleifenmuster

Das Muster sieht wie folgt aus: eine erste Ressource wird durch die Verkettung eines Namenpräfixes mit `0` (oder was auch immer dem ersten Index der Schleife entspricht) benannt. Eine zweite Ressource enthält eine copy-Schleife. In der copy-Schleife besteht der nächste Ressourcenname aus einer Verkettung des Namenpräfixes mit dem Ergebnis der `copyIndex(1)`-Funktion, die 1 zum aktuellen `copyIndex()` hinzufügt. Die zweite Ressource enthält auch ein `dependsOn`-Element, das auf die Verkettung des Namenpräfixes mit dem Ergebnis der `copyIndex()`-Funktion verweist. Dieser Ansatz erstellt eine `dependsOn`-Beziehung von der nächsten Ressource zurück zur vorherigen Ressource. Resource Manager wartet mit der Bereitstellung der nächsten Ressource, bis die vorherige Ressource bereitgestellt wurde.

Die folgende Vorlage veranschaulicht dieses Muster. Der Ressourcentyp „Microsoft.Resources/deployments“ ist einfach eine geschachtelte Vorlage, die eigentlich keine Ressourcen bereitstellt.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "numberToDeploy": {
      "type": "int",
      "minValue": 2,
      "defaultValue": 5
    }
  },
  "variables": {
    "count": "[sub(parameters('numberToDeploy'), 1)]"
  },
  "resources": [
    {
      "apiVersion": "2015-01-01",
      "type": "Microsoft.Resources/deployments",
      "name": "loop-0",
      "properties": {
        "mode": "Incremental",
        "parameters": {},
        "template": {
          "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {},
          "variables": {},
          "resources": [],
          "outputs": {
            "collection": {
              "type": "string",
              "value": "loop-0 "
            }
          }
        }
      }
    },
    {
      "apiVersion": "2015-01-01",
      "type": "Microsoft.Resources/deployments",
      "name": "[concat('loop-', copyIndex(1))]",
      "dependsOn": [
        "[concat('loop-', copyIndex())]"
      ],
      "copy": {
        "name": "iterator",
        "count": "[variables('count')]"
      },
      "properties": {
        "mode": "Incremental",
        "template": {
          "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {},
          "variables": {},
          "resources": [],
          "outputs": {
            "collection": {
              "type": "string",
              "value": "[concat(reference(concat('loop-',copyIndex())).outputs.collection.value,'loop-',copyIndex(1), ' ')]"
            }
          }
        }
      }
    }
  ],
  "outputs": {
    "result": {
      "type": "string",
      "value": "[reference(concat('loop-',variables('count'))).outputs.collection.value]"
    }
  }
}

```
In dieser Vorlage weist das erste Ressourcenobjekt den Namen `loop-0` auf. Im zweiten Ressourcenobjekt besteht der nächste Ressourcenname aus einer Verkettung des Worts `loop-` mit dem Ergebnis der `copyIndex(1)`-Funktion: `loop-1`. Das `dependsOn`-Element verweist auf die vorherige Ressource, da es sich um eine Verkettung des Worts `loop-` mit dem Ergebnis der `copyIndex()`-Funktion handelt: `loop-0`. Das Muster im zweiten Ressourcenobjekt wird wiederholt, bis `count` erreicht wurde, was zu einer Ressource namens `loop-4` führt, die `dependsOn` `loop-3`. Beachten Sie, dass `count` eine Variable ist, die `1` vom `numberToDeploy`-Parameter subtrahiert, damit der nullbasierte Zähler weiterhin richtig ist.

## <a name="try-the-template"></a>Testen der Vorlage

Befolgen Sie die nachfolgenden Schritte, wenn Sie diese Vorlage testen möchten:

1.    Wechseln Sie zum Azure-Portal, wählen Sie das Pluszeichen „+“ aus, und suchen Sie nach dem Ressourcentyp „Vorlagenbereitstellung“. Wenn Sie ihn in den Suchergebnissen finden, wählen Sie ihn aus.
2.    Wenn Sie zur Seite „Vorlagenbereitstellung“ gelangen, wählen Sie die Schaltfläche „Erstellen“ aus. Sie wechseln zum Blatt „benutzerdefinierte Bereitstellung“ und sehen, dass die Vorlage keine Ressourcen aufweist.
3.    Wählen Sie das Symbol „Bearbeiten“ aus.
4.    Löschen Sie die leere Vorlage im rechten Bereich.
5.    Kopieren Sie die vorherige Beispielvorlage, und fügen Sie sie im rechten Bereich ein.
6.    Wählen Sie die Schaltfläche „Speichern“ aus.
7.    Sie gelangen zurück zum Bereich „benutzerdefinierte Bereitstellung“, aber dieses Mal sind einige Dropdownfelder verfügbar. Wählen Sie Ihr Abonnement aus, erstellen Sie eine neue oder vorhandene Ressourcengruppe, und wählen Sie dann einen Speicherort aus. Überprüfen Sie die Nutzungsbedingungen, und klicken Sie auf die Schaltfläche „Ich stimme zu“.
8.    Wählen Sie die Schaltfläche „Kaufen“ aus.

Um sicherzustellen, dass die Ressourcen sequenziell bereitgestellt werden, wählen Sie „Ressourcengruppen“ und dann die Ressourcengruppe aus, die Sie zuvor ausgewählt haben. Wählen die Schaltfläche „Bereitstellungen“ auf dem Blatt „Ressourcengruppe“ aus, um die bereitgestellten Ressourcen in sequenzieller Reihenfolge mit entsprechendem Zeitstempel anzuzeigen.

![Bereitstellung der Vorlage für sequenzielle Schleifen im Azure Resource Manager](./media/resource-manager-sequential-loop/deployments.png)

## <a name="next-steps"></a>Nächste Schritte

Verwenden Sie dieses Muster in Ihren Vorlagen, indem Sie Ihre Ressourcen zur geschachtelten Vorlage hinzufügen. Sie können sie entweder direkt im Vorlagenelement der `Microsoft.Resources/deployments`-Ressource erstellen oder mit dem `templateLink`-Element eine Verknüpfung mit ihnen herstellen. Der Ressourcentyp im Beispiel ist eine geschachtelte Vorlage, aber es kann ein beliebiger Ressourcentyp bereitgestellt werden. Die einzige Ausnahme ist, dass innerhalb einer Iterationsschleife nicht auf die untergeordneten Ressourcen verwiesen werden kann.

* Eine Einführung zum Erstellen mehrerer Instanzen einer Ressource finden Sie unter [Bereitstellen mehrerer Instanzen von Ressourcen in Azure Resource Manager-Vorlagen](resource-group-create-multiple.md).
* Dieses Muster ist auch im [Vorlagenbaustein-Projekt](https://github.com/mspnp/template-building-blocks) und in den [Azure-Referenzarchitekturen](/azure/architecture/reference-architectures/) implementiert.
