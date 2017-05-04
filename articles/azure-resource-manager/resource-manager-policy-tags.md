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
ms.date: 04/20/2017
ms.author: tomfitz
translationtype: Human Translation
ms.sourcegitcommit: 2c33e75a7d2cb28f8dc6b314e663a530b7b7fdb4
ms.openlocfilehash: 04338b62d942774368149b27e8b35713b77f8d7c
ms.lasthandoff: 04/21/2017


---
# <a name="apply-resource-policies-for-tags"></a>Anwenden von Ressourcenrichtlinien für Tags

Dieses Thema enthält allgemeine Richtlinienregeln, durch deren Anwendung Sie eine konsistente Verwendung von Tags für Ressourcen sicherstellen können.

Durch das Anwenden einer Tagrichtlinie auf eine Ressourcengruppe oder ein Abonnement mit bestehenden Ressourcen wird die Richtlinie nicht rückwirkend auf diese Ressourcen angewendet. Um die Richtlinien für diese Ressourcen zu erzwingen, lösen Sie ein Update der vorhandenen Ressourcen aus, wie unter [Auslösen von Updates für vorhandene Ressourcen](#trigger-updates-to-existing-resources) gezeigt.

## <a name="ensure-all-resources-in-a-resource-group-have-a-tagvalue"></a>Sicherstellen, dass alle Ressourcen in einer Ressourcengruppe einen Tag/Wert aufweisen

Eine häufige Anforderung besteht darin, dass alle Ressourcen in einer Ressourcengruppe ein bestimmtes Tag und einen bestimmten Wert aufweisen müssen. Diese Anforderung wird oft benötigt, um die Kosten pro Abteilung zu verfolgen. Die folgenden Bedingungen müssen erfüllt sein:

* Das erforderliche Tag und der entsprechende Wert werden an neue und aktualisierte Ressourcen angefügt, die nicht über das Tag verfügen.
* Das erforderliche Tag und der entsprechende Wert können nicht aus vorhandenen Ressourcen entfernt werden.

Sie erfüllen diese Anforderung, indem Sie zwei integrierte Richtlinien auf eine Ressourcengruppe anwenden.

| ID | Beschreibung |
| ---- | ---- |
| 2a0e14a6-b0a6-4fab-991a-187a4f81c498 | Wendet ein erforderliches Tag und dessen Standardwert an (falls nicht vom Benutzer angegeben). |
| 1e30110a-5ceb-460c-a204-c1c3969c6d62 | Erzwingt ein erforderliches Tag und dessen Wert. |

### <a name="powershell"></a>PowerShell

Das folgende PowerShell-Skript weist die beiden integrierten Richtliniendefinitionen einer Ressourcengruppe zu. Weisen Sie der Ressourcengruppe vor dem Ausführen des Skripts alle erforderlichen Tags zu. Jedes Tag für die Ressourcengruppe wird für die Ressourcen in der Gruppe benötigt. Wenn die Zuweisung für alle Ressourcengruppen in Ihrem Abonnement erfolgen soll, lassen Sie beim Abrufen der Ressourcengruppen den Parameter `-Name` weg.

```powershell
$appendpolicy = Get-AzureRmPolicyDefinition | Where-Object {$_.Name -eq '2a0e14a6-b0a6-4fab-991a-187a4f81c498'}
$denypolicy = Get-AzureRmPolicyDefinition | Where-Object {$_.Name -eq '1e30110a-5ceb-460c-a204-c1c3969c6d62'}

$rgs = Get-AzureRMResourceGroup -Name ExampleGroup

foreach($rg in $rgs)
{
    $tags = $rg.Tags
    foreach($key in $tags.Keys){
        $key 
        $tags[$key]
        New-AzureRmPolicyAssignment -Name ("append"+$key+"tag") -PolicyDefinition $appendpolicy -Scope $rg.ResourceId -tagName $key -tagValue  $tags[$key]
        New-AzureRmPolicyAssignment -Name ("denywithout"+$key+"tag") -PolicyDefinition $denypolicy -Scope $rg.ResourceId -tagName $key -tagValue  $tags[$key]
    }
}
```

Nach dem Zuweisen der Richtlinien können Sie ein Update für alle vorhandenen Ressourcen auslösen, um die hinzugefügten Tagrichtlinien zu erzwingen. Das folgende Skript behält alle anderen Tags bei, die für die Ressourcen vorhanden waren:

```powershell
$group = Get-AzureRmResourceGroup -Name "ExampleGroup" 

$resources = Find-AzureRmResource -ResourceGroupName $group.ResourceGroupName 

foreach($r in $resources)
{
    try{
        $r | Set-AzureRmResource -Tags ($a=if($r.Tags -eq $NULL) { @{}} else {$r.Tags}) -Force -UsePatchSemantics
    }
    catch{
        Write-Host  $r.ResourceId + "can't be updated"
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

## <a name="next-steps"></a>Nächste Schritte
* Nach dem Definieren einer Richtlinienregel (wie in den vorherigen Beispielen zu sehen) müssen Sie die Richtliniendefinition erstellen und einem Bereich zuweisen. Bei dem Bereich kann es sich um ein Abonnement, um eine Ressourcengruppe oder um eine Ressource handeln. Wenn Sie Richtlinien über das Portal zuweisen möchten, siehe [Verwenden des Azure-Portals zum Zuweisen und Verwalten von Ressourcenrichtlinien](resource-manager-policy-portal.md). Wenn Sie Richtlinien über die REST-API, PowerShell oder die Azure-CLI zuweisen möchten, siehe [Zuweisen und Verwalten von Richtlinien mit Skripts](resource-manager-policy-create-assign.md).
* Eine Einführung in Ressourcenrichtlinien finden Sie unter [Verwenden von Richtlinien für Ressourcenverwaltung und Zugriffssteuerung](resource-manager-policy.md).
* Anleitungen dazu, wie Unternehmen Abonnements mit Resource Manager effektiv verwalten können, finden Sie unter [Azure-Unternehmensgerüst - Präskriptive Abonnementgovernance](resource-manager-subscription-governance.md).


