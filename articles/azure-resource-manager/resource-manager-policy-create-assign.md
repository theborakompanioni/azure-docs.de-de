---
title: Zuweisen und Verwalten von Azure-Ressourcenrichtlinien | Microsoft-Dokumentation
description: Beschreibt, wie Azure-Ressourcenrichtlinien auf Abonnements und Ressourcengruppen angewendet und wie Ressourcenrichtlinien angezeigt werden.
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
ms.date: 06/29/2017
ms.author: tomfitz
ms.translationtype: Human Translation
ms.sourcegitcommit: 1500c02fa1e6876b47e3896c40c7f3356f8f1eed
ms.openlocfilehash: e9a858addb768ce051fccce0eaf83e49a83da21b
ms.contentlocale: de-de
ms.lasthandoff: 06/30/2017


---
# <a name="assign-and-manage-resource-policies"></a>Zuweisen und Verwalten von Ressourcenrichtlinien

Um eine Richtlinie zu implementieren, müssen Sie diese drei Schritte ausführen:

1. Überprüfen Sie Richtliniendefinitionen (einschließlich den von Azure bereitgestellten integrierten Richtlinien), um zu überprüfen, ob in Ihrem Abonnement bereits eine vorhanden ist, die Ihre Anforderungen erfüllt.
2. Wenn eine vorhanden ist, suchen Sie deren Namen.
3. Wenn noch keine vorhanden ist, definieren Sie die Richtlinienregel mit JSON, und fügen Sie sie als eine Richtliniendefinition Ihrem Abonnement hinzu. Dieser Schritt stellt die Richtlinie für die Zuweisung zur Verfügung, wendet die Regeln aber nicht auf Ihr Abonnement an.
4. Weisen Sie die Richtlinie in beiden Fällen einem Bereich zu (z.B. einem Abonnement oder einer Ressourcengruppe). Jetzt werden die Regeln der Richtlinie erzwungen.

In diesem Artikel werden hauptsächlich die Schritte zum Erstellen einer Richtliniendefinition und Zuweisen dieser Definition zu einem Bereich über REST-API, PowerShell oder Azure-CLI beschrieben. Wenn Sie es vorziehen, Richtlinien über das Portal zuzuweisen, finden Sie entsprechende Informationen in [Verwenden des Azure-Portals zum Zuweisen und Verwalten von Ressourcenrichtlinien](resource-manager-policy-portal.md). Der Schwerpunkt dieses Artikels liegt nicht auf der Syntax zum Erstellen der Richtliniendefinition. Weitere Informationen zur Richtliniensyntax finden Sie unter [Verwenden von Richtlinien für Ressourcenverwaltung und Zugriffssteuerung](resource-manager-policy.md).

## <a name="rest-api"></a>REST-API

### <a name="create-policy-definition"></a>Erstellen einer Richtliniendefinition

Sie können eine Richtlinie mit der REST-API erstellen, wie unter [Policy Definitions](/rest/api/resources/policydefinitions)(in englischer Sprache) beschrieben. Die REST-API ermöglicht es Ihnen, Richtliniendefinitionen zu erstellen und zu löschen sowie Informationen zu vorhandenen Definitionen abzurufen.

Um eine Richtliniendefinition zu erstellen, führen Sie folgenden Befehl aus:

```HTTP
PUT https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.authorization/policydefinitions/{policyDefinitionName}?api-version={api-version}
```

Nehmen Sie einen Anforderungstext auf, der dem im folgenden Beispiel dargestellten ähnelt:

```json
{
  "properties": {
    "parameters": {
      "allowedLocations": {
        "type": "array",
        "metadata": {
          "description": "The list of locations that can be specified when deploying resources",
          "strongType": "location",
          "displayName": "Allowed locations"
        }
      }
    },
    "displayName": "Allowed locations",
    "description": "This policy enables you to restrict the locations your organization can specify when deploying resources.",
    "policyRule": {
      "if": {
        "not": {
          "field": "location",
          "in": "[parameters('allowedLocations')]"
        }
      },
      "then": {
        "effect": "deny"
      }
    }
  }
}
```

### <a name="assign-policy"></a>Zuweisen der Richtlinie

Sie können die Richtliniendefinition mithilfe von [Policy Assignments](/rest/api/resources/policyassignments)(Richtlinienzuweisungen, in englischer Sprache) auf den gewünschten Bereich anwenden. Die REST-API ermöglicht es Ihnen, Richtlinienzuweisungen zu erstellen und zu löschen sowie Informationen zu vorhandenen Zuweisungen abzurufen.

Führen Sie zum Erstellen einer neuen Richtlinienzuweisung folgenden Befehl aus:

```HTTP
PUT https://management.azure.com /subscriptions/{subscription-id}/providers/Microsoft.authorization/policyassignments/{policyAssignmentName}?api-version={api-version}
```

"{policy-assignment}" ist der Name der Richtlinienzuweisung.

Nehmen Sie einen Anforderungstext auf, der dem im folgenden Beispiel dargestellten ähnelt:

```json
{
  "properties":{
    "displayName":"West US only policy assignment on the subscription ",
    "description":"Resources can only be provisioned in West US regions",
    "parameters": {
      "allowedLocations": { "value": ["northeurope", "westus"] }
     },
    "policyDefinitionId":"/subscriptions/{subscription-id}/providers/Microsoft.Authorization/policyDefinitions/{definition-name}",
      "scope":"/subscriptions/{subscription-id}"
  },
}
```

### <a name="view-policy"></a>Anzeigen der Richtlinie
Um eine Richtlinie abzurufen, verwenden Sie den Vorgang [Richtliniendefinition abrufen](https://docs.microsoft.com/rest/api/resources/policydefinitions#PolicyDefinitions_Get).

### <a name="get-aliases"></a>Abrufen von Aliasen
Sie können die Aliase über die REST-API abrufen:

```HTTP
GET /subscriptions/{id}/providers?$expand=resourceTypes/aliases&api-version=2015-11-01
```

Das folgende Beispiel zeigt eine Definition eines Alias. Wie Sie sehen können, definiert ein Alias Pfade in verschiedenen API-Versionen, auch wenn sich der Name einer Eigenschaft ändert. 

```json
"aliases": [
    {
      "name": "Microsoft.Storage/storageAccounts/sku.name",
      "paths": [
        {
          "path": "properties.accountType",
          "apiVersions": [
            "2015-06-15",
            "2015-05-01-preview"
          ]
        },
        {
          "path": "sku.name",
          "apiVersions": [
            "2016-01-01"
          ]
        }
      ]
    }
]
```

## <a name="powershell"></a>PowerShell

Vergewissern Sie sich, dass Sie [die neueste Version von Azure PowerShell installiert](/powershell/azure/install-azurerm-ps) haben, bevor Sie mit den PowerShell-Beispielen fortfahren. In Version 3.6.0 wurden Richtlinienparameter hinzugefügt. Bei Verwendung einer älteren Version wird bei den Beispielen ein Fehler mit dem Hinweis zurückgegeben, dass der Parameter nicht gefunden wurde.

### <a name="view-policy-definitions"></a>Anzeigen von Richtliniendefinitionen
Verwenden Sie den folgenden Befehl, um alle Richtliniendefinitionen in Ihrem Abonnement anzuzeigen:

```powershell
Get-AzureRmPolicyDefinition
```

Es gibt alle verfügbaren Richtliniendefinitionen, einschließlich integrierten Richtlinien, zurück. Jede Richtlinie wird im folgenden Format zurückgeben:

```powershell
Name               : e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceId         : /providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceName       : e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceType       : Microsoft.Authorization/policyDefinitions
Properties         : @{displayName=Allowed locations; policyType=BuiltIn; description=This policy enables you to
                     restrict the locations your organization can specify when deploying resources. Use to enforce
                     your geo-compliance requirements.; parameters=; policyRule=}
PolicyDefinitionId : /providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c
```

Betrachten Sie vor dem Erstellen einer Richtliniendefinition die integrierten Richtlinien. Wenn Sie eine integrierte Richtlinie finden, welche die von Ihnen benötigten Beschränkungen anwendet, können Sie das Erstellen einer Richtliniendefinition überspringen. Weisen Sie stattdessen die integrierte Richtlinie dem gewünschten Bereich zu.

### <a name="create-policy-definition"></a>Erstellen einer Richtliniendefinition
Sie können eine Richtliniendefinition über das Cmdlet `New-AzureRmPolicyDefinition` erstellen.

```powershell
$policy = New-AzureRmPolicyDefinition -Name coolAccessTier -Description "Policy to specify access tier." -Policy '{
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
}'
```            

Die Ausgabe wird in einem `$policy`-Objekt gespeichert, das während der Richtlinienzuweisung verwendet wird. 

Statt die JSON als Parameter anzugeben, können Sie den Pfad zu einer JSON-Datei bereitstellen, in der die Richtlinienregel enthalten ist.

```powershell
$policy = New-AzureRmPolicyDefinition -Name coolAccessTier -Description "Policy to specify access tier." -Policy "c:\policies\coolAccessTier.json"
```

### <a name="assign-policy"></a>Zuweisen der Richtlinie

Sie wenden die Richtlinie über das `New-AzureRmPolicyAssignment` Cmdlet auf den gewünschten Bereich an. Im folgenden Beispiel wird die Richtlinie einer Ressourcengruppe zugewiesen.

```powershell
$rg = Get-AzureRmResourceGroup -Name "ExampleGroup"
New-AzureRMPolicyAssignment -Name accessTierAssignment -Scope $rg.ResourceId -PolicyDefinition $policy
```

Um eine Richtlinie zuzuweisen, die Parameter erfordert, erstellen Sie ein Objekt mit diesen Werten. Im folgenden Beispiel wird eine integrierte Richtlinie abgerufen und in Parameterwerten übergeben:

```powershell
$rg = Get-AzureRmResourceGroup -Name "ExampleGroup"
$policy = Get-AzureRmPolicyDefinition -Id /providers/Microsoft.Authorization/policyDefinitions/e5662a6-4747-49cd-b67b-bf8b01975c4c
$array = @("West US", "West US 2")
$param = @{"listOfAllowedLocations"=$array}
New-AzureRMPolicyAssignment -Name locationAssignment -Scope $rg.ResourceId -PolicyDefinition $policy -PolicyParameterObject $param
```

### <a name="view-policy-assignment"></a>Anzeigen der Richtlinienzuweisung

Verwenden Sie für den Erhalt einer bestimmte Richtlinienzuweisung Folgendes:

```powershell
$rg = Get-AzureRmResourceGroup -Name "ExampleGroup"
(Get-AzureRmPolicyAssignment -Name accessTierAssignment -Scope $rg.ResourceId
```

Verwenden Sie zum Anzeigen der Richtlinienregel für eine Richtliniendefinition Folgendes:

```powershell
(Get-AzureRmPolicyDefinition -Name coolAccessTier).Properties.policyRule | ConvertTo-Json
```

### <a name="remove-policy-assignment"></a>Entfernen der Richtlinienzuweisung 

Um eine Richtlinienzuweisung zu entfernen, verwenden Sie Folgendes:

```powershell
Remove-AzureRmPolicyAssignment -Name regionPolicyAssignment -Scope /subscriptions/{subscription-id}/resourceGroups/{resource-group-name}
```

## <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

### <a name="view-policy-definitions"></a>Anzeigen von Richtliniendefinitionen
Verwenden Sie den folgenden Befehl, um alle Richtliniendefinitionen in Ihrem Abonnement anzuzeigen:

```azurecli
az policy definition list
```

Es gibt alle verfügbaren Richtliniendefinitionen, einschließlich integrierten Richtlinien, zurück. Jede Richtlinie wird im folgenden Format zurückgeben:

```azurecli
{                                                            
  "description": "This policy enables you to restrict the locations your organization can specify when deploying resources. Use to enforce your geo-compliance requirements.",                      
  "displayName": "Allowed locations",                                                                                                                "id": "/providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c",                                                 "name": "e56962a6-4747-49cd-b67b-bf8b01975c4c",                                                                                                    "policyRule": {                                                                                                                                      "if": {                                                                                                                                              "not": {                                                                                                                                             "field": "location",                                                                                                                               "in": "[parameters('listOfAllowedLocations')]"                                                                                                   }                                                                                                                                                },                                                                                                                                                 "then": {                                                                                                                                            "effect": "Deny"                                                                                                                                 }                                                                                                                                                },                                                                                                                                                 "policyType": "BuiltIn"
}
```

Betrachten Sie vor dem Erstellen einer Richtliniendefinition die integrierten Richtlinien. Wenn Sie eine integrierte Richtlinie finden, welche die von Ihnen benötigten Beschränkungen anwendet, können Sie das Erstellen einer Richtliniendefinition überspringen. Weisen Sie stattdessen die integrierte Richtlinie dem gewünschten Bereich zu.

### <a name="create-policy-definition"></a>Erstellen einer Richtliniendefinition

Sie können eine Richtliniendefinition mit dem entsprechenden Befehl über Azure CLI erstellen.

```azurecli
az policy definition create --name coolAccessTier --description "Policy to specify access tier." --rules '{
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
}'    
```

### <a name="assign-policy"></a>Zuweisen der Richtlinie

Sie können die Richtlinie auf den gewünschten Bereich anwenden, indem Sie den Befehl zur Richtlinienzuweisung verwenden. Im folgenden Beispiel wird eine Richtlinie einer Ressourcengruppe zugewiesen.

```azurecli
az policy assignment create --name coolAccessTierAssignment --policy coolAccessTier --scope /subscriptions/{subscription-id}/resourceGroups/{resource-group-name}
```

### <a name="view-policy-assignment"></a>Anzeigen der Richtlinienzuweisung

Geben Sie zum Anzeigen einer Richtlinienzuweisung den Namen der Richtlinienzuweisung und den Bereich an:

```azurecli
az policy assignment show --name coolAccessTierAssignment --scope "/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}"
```

### <a name="remove-policy-assignment"></a>Entfernen der Richtlinienzuweisung 

Um eine Richtlinienzuweisung zu entfernen, verwenden Sie Folgendes:

```azurecli
az policy assignment delete --name coolAccessTier --scope /subscriptions/{subscription-id}/resourceGroups/{resource-group-name}
```

## <a name="next-steps"></a>Nächste Schritte
* Anleitungen dazu, wie Unternehmen Abonnements mit Resource Manager effektiv verwalten können, finden Sie unter [Azure-Unternehmensgerüst - Präskriptive Abonnementgovernance](resource-manager-subscription-governance.md).


