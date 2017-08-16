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
ms.date: 08/02/2017
ms.author: singhkay
ms.translationtype: HT
ms.sourcegitcommit: 8b857b4a629618d84f66da28d46f79c2b74171df
ms.openlocfilehash: 58eaab4fa03afc1e6a5e38bef691cce62a921ea9
ms.contentlocale: de-de
ms.lasthandoff: 08/04/2017

---
# <a name="apply-policies-to-linux-vms-with-azure-resource-manager"></a>Anwenden von Richtlinien auf virtuelle Linux-Computer mit Azure Resource Manager
Mithilfe von Richtlinien kann eine Organisation verschiedene Konventionen und Regeln im gesamten Unternehmen durchsetzen. Die Durchsetzung des gewünschten Verhaltens hilft dabei, Risiken zu mindern, und trägt gleichzeitig zum Erfolg des Unternehmens bei. In diesem Artikel wird beschrieben, wie Sie Azure Resource Manager-Richtlinien verwenden können, um das gewünschte Verhalten für die virtuellen Computer Ihrer Organisation zu definieren.

Eine Einführung in die Richtlinien finden Sie unter [Verwenden von Richtlinien für Ressourcenverwaltung und Zugriffssteuerung](../../azure-resource-manager/resource-manager-policy.md).

## <a name="permitted-virtual-machines"></a>Zugelassene virtuelle Computer
Um sicherzustellen, dass virtuelle Computer für Ihre Organisation mit einer Anwendung kompatibel sind, können Sie die zulässigen Betriebssysteme einschränken. In der folgenden Beispielrichtlinie lassen Sie nur das Erstellen von virtuellen Ubuntu 14.04.2-LTS-Computern zu.

```json
{
  "if": {
    "allOf": [
      {
        "field": "type",
        "in": [
          "Microsoft.Compute/disks",
          "Microsoft.Compute/virtualMachines",
          "Microsoft.Compute/VirtualMachineScaleSets"
        ]
      },
      {
        "not": {
          "allOf": [
            {
              "field": "Microsoft.Compute/imagePublisher",
              "in": [
                "Canonical"
              ]
            },
            {
              "field": "Microsoft.Compute/imageOffer",
              "in": [
                "UbuntuServer"
              ]
            },
            {
              "field": "Microsoft.Compute/imageSku",
              "in": [
                "14.04.2-LTS"
              ]
            },
            {
              "field": "Microsoft.Compute/imageVersion",
              "in": [
                "latest"
              ]
            }
          ]
        }
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}
```

Verwenden Sie einen Platzhalter, um die vorhergehende Richtlinie so zu ändern, dass Ubuntu LTS-Images zugelassen werden: 

```json
{
  "field": "Microsoft.Compute/virtualMachines/imageSku",
  "like": "*LTS"
}
```

Informationen zu Richtlinienfeldern finden Sie unter [Richtlinienaliase](../../azure-resource-manager/resource-manager-policy.md#aliases).

## <a name="managed-disks"></a>Verwaltete Datenträger

Um die Verwendung verwalteter Datenträger erforderlich zu machen, verwenden Sie die folgende Richtlinie:

```json
{
  "if": {
    "anyOf": [
      {
        "allOf": [
          {
            "field": "type",
            "equals": "Microsoft.Compute/virtualMachines"
          },
          {
            "field": "Microsoft.Compute/virtualMachines/osDisk.uri",
            "exists": true
          }
        ]
      },
      {
        "allOf": [
          {
            "field": "type",
            "equals": "Microsoft.Compute/VirtualMachineScaleSets"
          },
          {
            "anyOf": [
              {
                "field": "Microsoft.Compute/VirtualMachineScaleSets/osDisk.vhdContainers",
                "exists": true
              },
              {
                "field": "Microsoft.Compute/VirtualMachineScaleSets/osdisk.imageUrl",
                "exists": true
              }
            ]
          }
        ]
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}
```

## <a name="images-for-virtual-machines"></a>Images für virtuelle Computer

Aus Gründen der Sicherheit können Sie festlegen, dass nur genehmigte benutzerdefinierte Images in Ihrer Umgebung bereitgestellt werden. Sie können entweder die Ressourcengruppe mit den genehmigten Images oder die spezifischen genehmigten Images angeben.

Für das folgende Beispiel sind Images aus einer genehmigten Ressourcengruppe erforderlich:

```json
{
    "if": {
        "allOf": [
            {
                "field": "type",
                "in": [
                    "Microsoft.Compute/virtualMachines",
                    "Microsoft.Compute/VirtualMachineScaleSets"
                ]
            },
            {
                "not": {
                    "field": "Microsoft.Compute/imageId",
                    "contains": "resourceGroups/CustomImage"
                }
            }
        ]
    },
    "then": {
        "effect": "deny"
    }
} 
```

Im folgenden Beispiel werden die IDs der genehmigten Images angegeben:

```json
{
    "field": "Microsoft.Compute/imageId",
    "in": ["{imageId1}","{imageId2}"]
}
```

## <a name="virtual-machine-extensions"></a>VM-Erweiterungen

Möglicherweise möchten Sie die Verwendung bestimmter Erweiterungstypen verbieten. Beispielsweise ist eine Erweiterung unter Umständen nicht mit benutzerdefinierten Images für virtuelle Computer kompatibel. Im folgenden Beispiel wird gezeigt, wie eine bestimmte Erweiterung blockiert wird. Dabei wird mithilfe des Verlegers und des Typs festgelegt, welche Erweiterung blockiert werden soll.

```json
{
    "if": {
        "allOf": [
            {
                "field": "type",
                "equals": "Microsoft.Compute/virtualMachines/extensions"
            },
            {
                "field": "Microsoft.Compute/virtualMachines/extensions/publisher",
                "equals": "Microsoft.Compute"
            },
            {
                "field": "Microsoft.Compute/virtualMachines/extensions/type",
                "equals": "{extension-type}"

      }
        ]
    },
    "then": {
        "effect": "deny"
    }
}
```


## <a name="next-steps"></a>Nächste Schritte
* Nach dem Definieren einer Richtlinienregel (wie in den vorherigen Beispielen zu sehen) müssen Sie die Richtliniendefinition erstellen und einem Bereich zuweisen. Bei dem Bereich kann es sich um ein Abonnement, um eine Ressourcengruppe oder um eine Ressource handeln. Wenn Sie Richtlinien über das Portal zuweisen möchten, siehe [Verwenden des Azure-Portals zum Zuweisen und Verwalten von Ressourcenrichtlinien](../../azure-resource-manager/resource-manager-policy-portal.md). Wenn Sie Richtlinien über die REST-API, PowerShell oder die Azure-CLI zuweisen möchten, siehe [Zuweisen und Verwalten von Richtlinien mit Skripts](../../azure-resource-manager/resource-manager-policy-create-assign.md).
* Eine Einführung in Ressourcenrichtlinien finden Sie unter [Verwenden von Richtlinien für Ressourcenverwaltung und Zugriffssteuerung](../../azure-resource-manager/resource-manager-policy.md).
* Anleitungen dazu, wie Unternehmen Abonnements mit Resource Manager effektiv verwalten können, finden Sie unter [Azure-Unternehmensgerüst - Präskriptive Abonnementgovernance](../../azure-resource-manager/resource-manager-subscription-governance.md).

