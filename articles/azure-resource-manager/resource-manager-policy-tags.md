---
title: "Azure-Ressourcenrichtlinien für Tags | Microsoft-Dokumentation"
description: "Bietet Beispiele für Ressourcenrichtlinien zum Verwalten von Tags für Ressourcen."
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
ms.date: 02/09/2017
ms.author: tomfitz
translationtype: Human Translation
ms.sourcegitcommit: 72d398c529fc7dd5eef450da0e134dcdab534ac5
ms.openlocfilehash: 375a8df763eb6b4b8f7349e0061ab39c076ebfc6


---
# <a name="apply-resource-policies-for-tags"></a>Anwenden von Ressourcenrichtlinien für Tags

Dieses Thema enthält allgemeine Richtlinienregeln, durch deren Anwendung Sie eine konsistente Verwendung von Tags für Ressourcen sicherstellen können.

Durch das Anwenden einer Tagrichtlinie auf eine Ressourcengruppe oder ein Abonnement mit bestehenden Ressourcen wird die Richtlinie nicht rückwirkend auf diese Ressourcen angewendet. Um die Richtlinien für diese Ressourcen zu erzwingen, lösen Sie ein Update der vorhandenen Ressourcen aus, wie unter [Auslösen von Updates für vorhandene Ressourcen](#trigger-updates-to-existing-resources) gezeigt.

## <a name="ensure-all-resources-in-a-resource-group-have-a-tagvalue"></a>Sicherstellen, dass alle Ressourcen in einer Ressourcengruppe einen Tag/Wert aufweisen

Eine häufige Anforderung besteht darin, dass alle Ressourcen in einer Ressourcengruppe ein bestimmtes Tag und einen bestimmten Wert aufweisen müssen. Diese Anforderung wird oft benötigt, um die Kosten pro Abteilung zu verfolgen. Die folgenden Bedingungen müssen erfüllt sein:

* Das erforderliche Tag und der entsprechende Wert werden an neue und aktualisierte Ressourcen angefügt, die keine vorhandenen Tags aufweisen.
* Das erforderliche Tag und der entsprechende Wert werden an neue und aktualisierte Ressourcen angefügt, die zwar andere Tags, aber nicht das erforderliche Tag mit dem entsprechenden Wert aufweisen.
* Das erforderliche Tag und der entsprechende Wert können nicht aus vorhandenen Ressourcen entfernt werden.

Sie erfüllen diese Anforderung durch Anwenden der folgenden drei Richtlinien auf eine Ressourcengruppe:

* [Tag anfügen](#append-tag) 
* [Tag mit anderen Tags anfügen](#append-tag-with-other-tags)
* [Tag und Wert anfordern](#require-tag-and-value)

### <a name="append-tag"></a>Tag anfügen

Die folgende Richtlinienregel fügt ein costCenter-Tag mit einem vordefinierten Wert an, wenn keine Tags vorhanden sind:

```json
{
  "if": {
    "field": "tags",
    "exists": "false"
  },
  "then": {
    "effect": "append",
    "details": [
      {
        "field": "tags",
        "value": {"costCenter":"myDepartment" }
      }
    ]
  }
}
```

### <a name="append-tag-with-other-tags"></a>Tag mit anderen Tags anfügen

Die folgende Richtlinienregel fügt ein costCenter-Tag mit einem vordefinierten Wert an, wenn Tags vorhanden sind, das costCenter-Tag jedoch nicht definiert ist:

```json
{
  "if": {
    "allOf": [
      {
        "field": "tags",
        "exists": "true"
      },
      {
        "field": "tags.costCenter",
        "exists": "false"
      }
    ]
  },
  "then": {
    "effect": "append",
    "details": [
      {
        "field": "tags.costCenter",
        "value": "myDepartment"
      }
    ]
  }
}
```

### <a name="require-tag-and-value"></a>Tag und Wert anfordern

Die folgenden Richtlinienregel verweigert die Aktualisierung oder Erstellung von Ressourcen, deren costCenter-Tag nicht der vordefinierte Wert zugewiesen ist.

```json
{
  "if": {
    "not": {
      "field": "tags.costCenter",
      "equals": "myDepartment"
    }
  },
  "then": {
    "effect": "deny"
  }
}
```

## <a name="require-tags-for-a-resource-type"></a>Tags für einen Ressourcentyp anfordern
Das folgende Beispiel zeigt, wie logische Operatoren so verschachtelt werden, dass ein Anwendungstag nur für einen bestimmten Ressourcentyp (in diesem Fall Speicherkonten) erforderlich ist.

```json
{
    "if": {
        "allOf": [
          {
            "not": {
              "field": "tags",
              "containsKey": "application"
            }
          },
          {
            "field": "type",
            "equals": "Microsoft.Storage/storageAccounts"
          }
        ]
    },
    "then": {
        "effect": "audit"
    }
}
```

## <a name="require-tag"></a>Tag anfordern
Die folgende Richtlinie verweigert Anforderungen, die kein Tag mit dem Schlüssel „costCenter“ enthalten (beliebiger Wert kann zugewiesen werden):

```json
{
  "if": {
    "not" : {
      "field" : "tags",
      "containsKey" : "costCenter"
    }
  },
  "then" : {
    "effect" : "deny"
  }
}
```

## <a name="trigger-updates-to-existing-resources"></a>Updates für vorhandene Ressourcen auslösen

Das folgende PowerShell-Skript löst ein Update für vorhandene Ressourcen aus, um die hinzugefügten Tagrichtlinien zu erzwingen.

```powershell
$group = Get-AzureRmResourceGroup -Name "ExampleGroup" 

$resources = Find-AzureRmResource -ResourceGroupName $group.ResourceGroupName 

foreach($r in $resources)
{
    try{
        $r | Set-AzureRmResource -Tags ($a=if($_.Tags -eq $NULL) { @{}} else {$_.Tags}) -Force -UsePatchSemantics
    }
    catch{
        Write-Host  $r.ResourceId + "can't be updated"
    }
}
```

## <a name="next-steps"></a>Nächste Schritte
* Nach dem Definieren einer Richtlinienregel (wie in den vorherigen Beispielen zu sehen) müssen Sie die Richtliniendefinition erstellen und einem Bereich zuweisen. Bei dem Bereich kann es sich um ein Abonnement, um eine Ressourcengruppe oder um eine Ressource handeln. Beispiele zum Erstellen und Zuweisen von Richtlinien finden Sie unter [Zuweisen und Verwalten von Richtlinien](resource-manager-policy-create-assign.md). 
* Eine Einführung in Ressourcenrichtlinien finden Sie unter [Verwenden von Richtlinien für Ressourcenverwaltung und Zugriffssteuerung](resource-manager-policy.md).
* Anleitungen dazu, wie Unternehmen Abonnements mit Resource Manager effektiv verwalten können, finden Sie unter [Azure-Unternehmensgerüst - Präskriptive Abonnementgovernance](resource-manager-subscription-governance.md).




<!--HONumber=Feb17_HO3-->


