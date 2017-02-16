---
title: "Azure Resource Manager-Richtlinien für Speicher | Microsoft-Dokumentation"
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
ms.date: 01/19/2017
ms.author: tomfitz
translationtype: Human Translation
ms.sourcegitcommit: ce31bbcb96a6afe19cf6c25dd9a68d6c2d4d080c
ms.openlocfilehash: f2141304f6db6d137065e06558e10c721b11f892


---
# <a name="apply-azure-resource-policies-to-storage-accounts"></a>Anwenden von Azure-Ressourcenrichtlinien auf Speicherkonten
Mithilfe von Azure Resource Manager-Richtlinien können Sie konsistente Regeln für die Bereitstellung von Ressourcen in Ihrer Organisation definieren. Durch die Erstellung benutzerdefinierter Richtlinien stellen Sie sicher, dass die Benutzer in Ihrer Organisation nicht gegen Konventionen verstoßen, die zum Verwalten der Ressourcen Ihrer Organisation erforderlich sind. Dieses Thema enthält mehrere Richtlinien, die Regeln für Azure Storage-Konten definieren. Weitere Informationen zu Richtlinien finden Sie unter [Verwenden von Richtlinien für Ressourcenverwaltung und Zugriffssteuerung](resource-manager-policy.md).

Die Beispiele in diesem Thema enthalten hartcodierte Werte in der Richtlinienregel. Sie können jedoch auch Parameter verwenden, um Werte zu übergeben, die beim Zuweisen der Richtlinie verwendet werden. Weitere Informationen finden Sie unter [Parameter](resource-manager-policy.md#parameters).

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

## <a name="create-and-assign-policies"></a>Erstellen und Zuweisen von Richtlinien

Nach dem Definieren einer Richtlinienregel (wie in den vorherigen Beispielen zu sehen) müssen Sie die Richtlinie erstellen und einem Bereich zuweisen. Bei dem Bereich kann es sich um ein Abonnement, um eine Ressourcengruppe oder um eine Ressource handeln. Beispiele zum Erstellen und Zuweisen von Richtlinien finden Sie unter [Erstellen und Zuweisen einer Richtlinie](resource-manager-policy.md#create-and-assign-a-policy). 

## <a name="next-steps"></a>Nächste Schritte
* Anleitungen dazu, wie Unternehmen Abonnements mit Resource Manager effektiv verwalten können, finden Sie unter [Azure-Unternehmensgerüst - Präskriptive Abonnementgovernance](resource-manager-subscription-governance.md).




<!--HONumber=Jan17_HO3-->


