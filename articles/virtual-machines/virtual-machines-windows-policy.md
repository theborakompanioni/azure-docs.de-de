---
title: Erzwingen der Sicherheit mit Richtlinien auf virtuellen Windows-Computern in Azure | Microsoft-Dokumentation
description: Anwenden einer Richtlinie auf einen virtuellen Windows-Computer des Azure Resource Manager
services: virtual-machines-windows
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 0b71ba54-01db-43ad-9bca-8ab358ae141b
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 04/13/2016
ms.author: kasing
translationtype: Human Translation
ms.sourcegitcommit: 1cb57e5156dab976599ddfa9a58f26ca8ef1ee0e
ms.openlocfilehash: 54afb2f55ef9ff48448c63bc8ee8e745765f61e6
ms.lasthandoff: 02/22/2017


---
# <a name="apply-security-and-policies-to-windows-vms-with-azure-resource-manager"></a>Anwenden von Sicherheit und Richtlinien auf virtuelle Windows-Computer mit Azure Resource Manager
Mithilfe von Richtlinien kann eine Organisation verschiedene Konventionen und Regeln im gesamten Unternehmen durchsetzen. Die Durchsetzung des gewünschten Verhaltens hilft dabei, Risiken zu mindern, und trägt gleichzeitig zum Erfolg des Unternehmens bei. In diesem Artikel wird beschrieben, wie Sie Azure Resource Manager-Richtlinien verwenden können, um das gewünschte Verhalten für die virtuellen Computer Ihrer Organisation zu definieren.

Nachfolgend werden die Schritte aufgelistet, die hierzu erforderlich sind:

1. Grundlagen zu Azure Resource Manager-Richtlinien
2. Definieren einer Richtlinie für den virtuellen Computer
3. Erstellen der Richtlinie
4. Anwenden der Richtlinie

## <a name="azure-resource-manager-policy-101"></a>Grundlagen zu Azure Resource Manager-Richtlinien
Für die ersten Schritte mit Azure Resource Manager-Richtlinien empfehlen wir zunächst, den folgenden Artikel zu lesen. Danach können Sie mit den Schritten in diesem Artikel fortfahren. Der unten stehende Artikel beschreibt die grundlegende Definition und die Struktur einer Richtlinie sowie die Art und Weise, wie Richtlinien ausgewertet werden. Darüber hinaus enthält er verschiedene Beispiele für Richtliniendefinitionen.

* [Verwenden von Richtlinien für Ressourcenverwaltung und Zugriffssteuerung](../azure-resource-manager/resource-manager-policy.md)

## <a name="define-a-policy-for-your-virtual-machine"></a>Definieren einer Richtlinie für den virtuellen Computer
Ein allgemeines Szenario für ein Unternehmen besteht z. B. darin, es den Benutzern nur über bestimmte Betriebssysteme zu ermöglichen, die auf Kompatibilität mit einer LOB-Anwendung getestet wurden, virtuelle Computer zu erstellen. Mithilfe einer Azure Resource Manager-Richtlinie kann diese Aufgabe in wenigen Schritten durchgeführt werden.
In dieser Beispielrichtlinie lassen wir nur das Erstellen von virtuellen Windows Server 2012 R2 Datacenter-Computern zu. Die Richtliniendefinition sieht wie folgt aus:

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
            "equals": "MicrosoftWindowsServer"
          },
          {
            "field": "Microsoft.Compute/virtualMachines/imageOffer",
            "equals": "WindowsServer"
          },
          {
            "field": "Microsoft.Compute/virtualMachines/imageSku",
            "equals": "2012-R2-Datacenter"
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

Die oben beschriebene Richtlinie kann problemlos für ein Szenario geändert werden, in dem jedes Windows Server Datacenter-Image zugelassen wird, das für die Bereitstellung eines virtuellen Computers verwendet wird. Nehmen Sie dazu die unten stehende Änderung vor:

```
{
  "field": "Microsoft.Compute/virtualMachines/imageSku",
  "like": "*Datacenter"
}
```

#### <a name="virtual-machine-property-fields"></a>Eigenschaftenfelder für virtuelle Computer
In der folgenden Tabelle werden die Eigenschaften des virtuellen Computers beschrieben, die als Felder in der Richtliniendefinition verwendet werden können. Weitere Informationen zu Richtlinienfeldern finden Sie im folgenden Artikel:

* [Felder und Quellen](../azure-resource-manager/resource-manager-policy.md#conditions)

| Feldname | Beschreibung |
| --- | --- |
| imagePublisher |Gibt den Verleger des Images an. |
| imageOffer |Gibt das Angebot für den ausgewählten Image-Verleger an. |
| imageSku |Gibt die SKU für das ausgewählte Angebot an. |
| imageVersion |Gibt die Imageversion für die ausgewählte SKU an. |

## <a name="create-the-policy"></a>Erstellen der Richtlinie
Eine Richtlinie kann problemlos über die REST-API direkt oder über PowerShell-Cmdlets erstellt werden. Informationen zum Erstellen der Richtlinie finden Sie im folgenden Artikel:

* [Erstellen einer Richtlinie](../azure-resource-manager/resource-manager-policy.md)

## <a name="apply-the-policy"></a>Anwenden der Richtlinie
Nach dem Erstellen der Richtlinie müssen Sie sie auf einen definierten Bereich anwenden. Bei dem Bereich kann es sich um ein Abonnement, eine Ressourcengruppe oder die Ressource handeln. Informationen zum Anwenden der Richtlinie finden Sie im folgenden Artikel:

* [Erstellen einer Richtlinie](../azure-resource-manager/resource-manager-policy.md)

