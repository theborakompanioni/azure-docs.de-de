---
title: "Azure-Ressourcenrichtlinien für Netzwerkressourcen | Microsoft-Dokumentation"
description: Beschreibt Azure Resource Manager-Richtlinien zur Verwaltung der Bereitstellung von Netzwerkressourcen.
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
ms.date: 07/05/2017
ms.author: tomfitz
ms.translationtype: Human Translation
ms.sourcegitcommit: b1d56fcfb472e5eae9d2f01a820f72f8eab9ef08
ms.openlocfilehash: bca66bbdd9da9b3e4099d0d961f42c9368a17f5e
ms.contentlocale: de-de
ms.lasthandoff: 07/06/2017


---
# <a name="apply-resource-policies-to-network-resources"></a>Anwenden von Ressourcenrichtlinien auf Netzwerkressourcen
Dieser Artikel zeigt ein Beispiel für eine [Ressourcenrichtlinie](resource-manager-policy.md), die auf virtuellen Azure-Netzwerkgateways angewendet werden kann. Diese Richtlinie stellt die Konsistenz für Gateways, die in Ihrer Organisation bereitgestellt werden, sicher. 

## <a name="define-permitted-virtual-network-gateway-sku"></a>Definieren der zulässigen SKU des Gateways des virtuellen Netzwerks

Die folgende Richtlinie schränkt ein, welche SKUs für Gateways des virtuellen Netzwerks bereitgestellt werden können:

```json
{
  "if": {
    "allOf": [
      {
        "field": "type",
        "equals": "Microsoft.Network/virtualNetworkGateways"
      },
      {
        "not": {
          "field": "Microsoft.Network/virtualNetworkGateways/sku.name",
          "in": [
            "Basic",
            "VpnGw1"
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

## <a name="next-steps"></a>Nächste Schritte
* Nach dem Definieren einer Richtlinienregel (wie in den vorherigen Beispielen zu sehen) müssen Sie die Richtliniendefinition erstellen und einem Bereich zuweisen. Bei dem Bereich kann es sich um ein Abonnement, um eine Ressourcengruppe oder um eine Ressource handeln. Wenn Sie Richtlinien über das Portal zuweisen möchten, siehe [Verwenden des Azure-Portals zum Zuweisen und Verwalten von Ressourcenrichtlinien](resource-manager-policy-portal.md). Wenn Sie Richtlinien über die REST-API, PowerShell oder die Azure-CLI zuweisen möchten, siehe [Zuweisen und Verwalten von Richtlinien mit Skripts](resource-manager-policy-create-assign.md). 
* Anleitungen dazu, wie Unternehmen Abonnements mit Resource Manager effektiv verwalten können, finden Sie unter [Azure-Unternehmensgerüst - Präskriptive Abonnementgovernance](resource-manager-subscription-governance.md).


