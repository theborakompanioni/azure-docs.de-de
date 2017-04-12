---
title: Erzwingen der Sicherheit mit Richtlinien auf virtuellen Linux-Computern in Azure | Microsoft-Dokumentation
description: 'Anwenden einer Richtlinie auf einen virtuellen Linux-Computer des Azure Resource Manager '
services: virtual-machines-linux
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 06778ab4-f8ff-4eed-ae10-26a276fc3faa
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 04/13/2016
ms.author: singhkay
translationtype: Human Translation
ms.sourcegitcommit: 303cb9950f46916fbdd58762acd1608c925c1328
ms.openlocfilehash: b48a4e2fa913b865cf4b57693ef281e446541328
ms.lasthandoff: 04/04/2017


---
# <a name="apply-security-and-policies-to-linux-vms-with-azure-resource-manager"></a>Anwenden von Sicherheit und Richtlinien auf virtuelle Linux-Computer mit Azure Resource Manager
Mithilfe von Richtlinien kann eine Organisation verschiedene Konventionen und Regeln im gesamten Unternehmen durchsetzen. Die Durchsetzung des gewünschten Verhaltens hilft dabei, Risiken zu mindern, und trägt gleichzeitig zum Erfolg des Unternehmens bei. In diesem Artikel wird beschrieben, wie Sie Azure Resource Manager-Richtlinien verwenden können, um das gewünschte Verhalten für die virtuellen Computer Ihrer Organisation zu definieren.

Nachfolgend werden die Schritte aufgelistet, die hierzu erforderlich sind:

1. Grundlagen zu Azure Resource Manager-Richtlinien
2. Definieren einer Richtlinie für den virtuellen Computer
3. Erstellen der Richtlinie
4. Anwenden der Richtlinie

## <a name="azure-resource-manager-policy-101"></a>Grundlagen zu Azure Resource Manager-Richtlinien
Für die ersten Schritte mit Azure Resource Manager-Richtlinien empfehlen wir zunächst, den folgenden Artikel zu lesen. Danach können Sie mit den Schritten in diesem Artikel fortfahren. Der unten stehende Artikel beschreibt die grundlegende Definition und die Struktur einer Richtlinie sowie die Art und Weise, wie Richtlinien ausgewertet werden. Darüber hinaus enthält er verschiedene Beispiele für Richtliniendefinitionen.

* [Verwenden von Richtlinien für Ressourcenverwaltung und Zugriffssteuerung](../../azure-resource-manager/resource-manager-policy.md)

## <a name="define-a-policy-for-your-virtual-machine"></a>Definieren einer Richtlinie für den virtuellen Computer
Ein allgemeines Szenario für ein Unternehmen besteht z. B. darin, es den Benutzern nur über bestimmte Betriebssysteme zu ermöglichen, die auf Kompatibilität mit einer LOB-Anwendung getestet wurden, virtuelle Computer zu erstellen. Mithilfe einer Azure Resource Manager-Richtlinie kann diese Aufgabe in wenigen Schritten durchgeführt werden.
In dieser Beispielrichtlinie lassen wir nur das Erstellen von virtuellen Ubuntu 14.04.2-LTS-Computern zu. Die Richtliniendefinition sieht wie folgt aus:

```
"if": {
  "allOf": [
    {
      "field": "type",
      "equals": "Microsoft.Compute/virtualMachines"
    },
    {
      "not": {
        "allOf": [
          {
            "field": "Microsoft.Compute/virtualMachines/imagePublisher",
            "equals": "Canonical"
          },
          {
            "field": "Microsoft.Compute/virtualMachines/imageOffer",
            "equals": "UbuntuServer"
          },
          {
            "field": "Microsoft.Compute/virtualMachines/imageSku",
            "equals": "14.04.2-LTS"
          }
        ]
      }
    }
  ]
},
"then": {
  "effect": "deny"
}
```

Die oben beschriebene Richtlinie kann problemlos für ein Szenario geändert werden, in dem jedes Ubuntu LTS-Image, das für die Bereitstellung eines virtuellen Computers verwendet wird, zugelassen wird. Nehmen Sie dazu die unten stehende Änderung vor:

```
{
  "field": "Microsoft.Compute/virtualMachines/imageSku",
  "like": "*LTS"
}
```

#### <a name="virtual-machine-property-fields"></a>Eigenschaftenfelder für virtuelle Computer
In der folgenden Tabelle werden die Eigenschaften des virtuellen Computers beschrieben, die als Felder in der Richtliniendefinition verwendet werden können. Weitere Informationen zu Richtlinienfeldern finden Sie unter [Verwenden von Richtlinien für Ressourcenverwaltung und Zugriffssteuerung](../../resource-manager-policy.md).

| Feldname | Beschreibung |
| --- | --- |
| imagePublisher |Gibt den Verleger des Images an. |
| imageOffer |Gibt das Angebot für den ausgewählten Image-Verleger an. |
| imageSku |Gibt die SKU für das ausgewählte Angebot an. |
| imageVersion |Gibt die Imageversion für die ausgewählte SKU an. |

## <a name="create-the-policy"></a>Erstellen der Richtlinie
Eine Richtlinie kann problemlos über die REST-API direkt oder über PowerShell-Cmdlets erstellt werden. Informieren Sie sich weiter über das [Erstellen und Zuweisen einer Richtlinie](../../resource-manager-policy.md).

## <a name="apply-the-policy"></a>Anwenden der Richtlinie
Nach dem Erstellen der Richtlinie müssen Sie sie auf einen definierten Bereich anwenden. Bei dem Bereich kann es sich um ein Abonnement, eine Ressourcengruppe oder die Ressource handeln. Informieren Sie sich weiter über das [Erstellen und Zuweisen einer Richtlinie](../../resource-manager-policy.md).

