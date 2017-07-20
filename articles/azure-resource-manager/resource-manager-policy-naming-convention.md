---
title: "Azure-Ressourcenrichtlinien für Benennungskonventionen | Microsoft-Dokumentation"
description: "Beschreibt die Azure Resource Manager-Richtlinien für Benennungskonventionen."
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
ms.date: 06/27/2017
ms.author: tomfitz
ms.translationtype: Human Translation
ms.sourcegitcommit: 1500c02fa1e6876b47e3896c40c7f3356f8f1eed
ms.openlocfilehash: 51b3519bbba8cb4c768bfdd7dadf92fced434f22
ms.contentlocale: de-de
ms.lasthandoff: 06/30/2017


---
# <a name="apply-resource-policies-for-names-and-text"></a>Anwenden von Ressourcenrichtlinien für Namen und Text
Dieses Thema zeigt mehrere [Ressourcenrichtlinien](resource-manager-policy.md), die Sie zum Erstellen von Benennungs- und Text-Konventionen anwenden können. Diese Richtlinien stellen die Konsistenz von Ressourcennamen und Tag-Werten sicher. 

## <a name="set-naming-convention-with-wildcard"></a>Festlegen der Benennungskonvention mit Platzhalter
Das folgende Beispiel veranschaulicht die Verwendung von Platzhalterzeichen, die von der Bedingung **like** unterstützt werden. Die Bedingung gibt an, dass die Anforderung verweigert wird, wenn der Name nicht mit dem angegebenen Muster (namePrefix\*nameSuffix) übereinstimmt:

```json
{
  "if": {
    "not": {
      "field": "name",
      "like": "namePrefix*nameSuffix"
    }
  },
  "then": {
    "effect": "deny"
  }
}
```

## <a name="set-naming-convention-with-pattern"></a>Festlegen der Benennungskonvention mit Muster

Verwenden Sie die match-Bedingung, um anzugeben, dass Ressourcennamen einem Muster entsprechen. Im folgenden Beispiel müssen Namen mit `contoso` beginnen und sechs zusätzliche Buchstaben enthalten:

```json
{
  "if": {
    "not": {
      "field": "name",
      "match": "contoso??????"
    }
  },
  "then": {
    "effect": "deny"
  }
}
```

## <a name="set-date-pattern-for-tag-value"></a>Festlegen des Datumsmusters für den Tagwert

Um ein Datumsmuster aus zwei Ziffern, Bindestrich, drei Buchstaben und vier Ziffern anzufordern, verwenden Sie Folgendes:

```json
{
  "if": {
    "field": "tags.date",
    "match": "##-???-####"
  },
  "then": {
    "effect": "deny"
  }
}
```

## <a name="next-steps"></a>Nächste Schritte
* Nach dem Definieren einer Richtlinienregel (wie in den vorherigen Beispielen zu sehen) müssen Sie die Richtliniendefinition erstellen und einem Bereich zuweisen. Bei dem Bereich kann es sich um ein Abonnement, um eine Ressourcengruppe oder um eine Ressource handeln. Wenn Sie Richtlinien über das Portal zuweisen möchten, siehe [Verwenden des Azure-Portals zum Zuweisen und Verwalten von Ressourcenrichtlinien](resource-manager-policy-portal.md). Wenn Sie Richtlinien über die REST-API, PowerShell oder die Azure-CLI zuweisen möchten, siehe [Zuweisen und Verwalten von Richtlinien mit Skripts](resource-manager-policy-create-assign.md). 
* Anleitungen dazu, wie Unternehmen Abonnements mit Resource Manager effektiv verwalten können, finden Sie unter [Azure-Unternehmensgerüst - Präskriptive Abonnementgovernance](resource-manager-subscription-governance.md).


