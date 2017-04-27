---
title: "Azure-Ressourcenrichtlinien für Speicherkonten | Microsoft-Dokumentation"
description: Beschreibt Azure Resource Manager-Richtlinien zur Verwaltung der Bereitstellung von Speicherkonten.
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
ms.date: 03/30/2017
ms.author: tomfitz
translationtype: Human Translation
ms.sourcegitcommit: 197ebd6e37066cb4463d540284ec3f3b074d95e1
ms.openlocfilehash: 75685a21ce4a212638016be62640badd4870454a
ms.lasthandoff: 03/31/2017


---
# <a name="apply-resource-policies-to-storage-accounts"></a>Anwenden von Ressourcenrichtlinien auf Speicherkonten
Dieses Thema zeigt mehrere [Ressourcenrichtlinien](resource-manager-policy.md), die Sie auf Azure-Speicherkonten anwenden können. Diese Richtlinien sorgen bei den in Ihrer Organisation bereitgestellten Speicherkonten für Konsistenz. 

## <a name="define-permitted-storage-account-types"></a>Definieren zulässiger Speicherkontotypen

Die folgende Richtlinie schränkt die bereitstellbaren [Speicherkontotypen](../storage/storage-redundancy.md) ein:

```json
{
  "if": {
    "allOf": [
      {
        "field": "type",
        "equals": "Microsoft.Storage/storageAccounts"
      },
      {
        "not": {
          "field": "Microsoft.Storage/storageAccounts/sku.name",
          "in": [
            "Standard_LRS",
            "Standard_GRS"
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

Eine ähnliche Richtlinienregel mit einem Parameter zum Akzeptieren zulässiger SKUs steht als integrierte Richtliniendefinition zur Verfügung. Die integrierte Richtlinie besitzt die Ressourcen-ID `/providers/Microsoft.Authorization/policyDefinitions/7433c107-6db4-4ad1-b57a-a76dce0154a1`. 

## <a name="define-permitted-access-tier"></a>Definieren der zulässigen Zugriffsebene

Die folgende Richtlinie gibt die Art von [Zugriffsebene](../storage/storage-blob-storage-tiers.md) an, die für Speicherkonten angegeben werden kann:

```json
{
  "if": {
    "allOf": [
      {
        "field": "type",
        "equals": "Microsoft.Storage/storageAccounts"
      },
      {
        "field": "kind",
        "equals": "BlobStorage"
      },
      {
        "not": {
          "field": "Microsoft.Storage/storageAccounts/accessTier",
          "equals": "cool"
        }
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}
```

## <a name="ensure-encryption-is-enabled"></a>Sicherstellen, dass die Verschlüsselung aktiviert ist

Die folgende Richtlinie gibt vor, dass die [Speicherdienstverschlüsselung](../storage/storage-service-encryption.md) bei allen Speicherkonten aktiviert sein muss:

```json
{
  "if": {
    "allOf": [
      {
        "field": "type",
        "equals": "Microsoft.Storage/storageAccounts"
      },
      {
        "not": {
          "field": "Microsoft.Storage/storageAccounts/enableBlobEncryption",
          "equals": "true"
        }
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}
```

Diese Richtlinienregel steht auch als integrierte Richtliniendefinition mit der Ressourcen-ID `/providers/Microsoft.Authorization/policyDefinitions/7c5a74bf-ae94-4a74-8fcf-644d1e0e6e6f` zur Verfügung.

## <a name="next-steps"></a>Nächste Schritte
* Nach dem Definieren einer Richtlinienregel (wie in den vorherigen Beispielen zu sehen) müssen Sie die Richtliniendefinition erstellen und einem Bereich zuweisen. Bei dem Bereich kann es sich um ein Abonnement, um eine Ressourcengruppe oder um eine Ressource handeln. Wenn Sie Richtlinien über das Portal zuweisen möchten, siehe [Verwenden des Azure-Portals zum Zuweisen und Verwalten von Ressourcenrichtlinien](resource-manager-policy-portal.md). Wenn Sie Richtlinien über die REST-API, PowerShell oder die Azure-CLI zuweisen möchten, siehe [Zuweisen und Verwalten von Richtlinien mit Skripts](resource-manager-policy-create-assign.md). 
* Anleitungen dazu, wie Unternehmen Abonnements mit Resource Manager effektiv verwalten können, finden Sie unter [Azure-Unternehmensgerüst - Präskriptive Abonnementgovernance](resource-manager-subscription-governance.md).


