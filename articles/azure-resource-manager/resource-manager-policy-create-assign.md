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
ms.date: 02/10/2017
ms.author: tomfitz
translationtype: Human Translation
ms.sourcegitcommit: 138197bab775ec854f2761a64c52a9a9c8baca60
ms.openlocfilehash: dec7ebd834514a10d02d739264b03b01a65a4375


---
# <a name="assign-and-manage-resource-policies"></a>Zuweisen und Verwalten von Ressourcenrichtlinien

Um eine Richtlinie zu implementieren, müssen Sie drei Schritte ausführen:

1. Definieren Sie die Richtlinienregel mit JSON.
2. Erstellen Sie eine Richtliniendefinition in Ihrem Abonnement über die JSON, die Sie im vorherigen Schritt erstellt haben. Dieser Schritt stellt die Richtlinie für die Zuweisung zur Verfügung, wendet die Regeln aber nicht auf Ihr Abonnement an.
3. Weisen Sie die Richtlinie einem Bereich zu (z.B. einem Abonnement oder einer Ressourcengruppe). Jetzt werden die Regeln der Richtlinie erzwungen.

Azure stellt einige vordefinierte Richtlinien bereit, durch die die Anzahl der zu definierenden Richtlinien reduziert werden kann. Wenn eine vordefinierte Richtlinie für Ihr Szenario funktioniert, überspringen Sie die ersten beiden Schritte, und weisen Sie die vordefinierte Richtlinie einem Bereich zu.

Dieses Thema befasst sich hauptsächlich mit den Schritten zum Erstellen einer Richtliniendefinition und zum Zuweisen dieser Definition zu einem Bereich. Der Schwerpunkt liegt nicht auf der Syntax zum Erstellen der Richtliniendefinition. Weitere Informationen zur Richtliniensyntax finden Sie unter [Verwenden von Richtlinien für Ressourcenverwaltung und Zugriffssteuerung](resource-manager-policy.md).

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
      "listOfAllowedLocations": { "value": ["West US", "West US 2"] }
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

### <a name="create-policy-definition"></a>Erstellen einer Richtliniendefinition
Sie können eine Richtliniendefinition über das Cmdlet `New-AzureRmPolicyDefinition` erstellen. Im folgenden Beispiel wird eine Richtliniendefinition erstellt, um nur Ressourcen in Nord- und Westeuropa zuzulassen.

```powershell
$policy = New-AzureRmPolicyDefinition -Name regionPolicyDefinition -Description "Policy to allow resource creation only in certain regions" -Policy '{    
  "if" : {
    "not" : {
      "field" : "location",
      "in" : ["northeurope" , "westeurope"]
    }
  },
  "then" : {
    "effect" : "deny"
  }
}'
```            

Die Ausgabe wird in einem `$policy`-Objekt gespeichert, das während der Richtlinienzuweisung verwendet wird. 

Statt die JSON als Parameter anzugeben, können Sie den Pfad zu einer JSON-Datei bereitstellen, in der die Richtlinienregel enthalten ist.

```powershell
$policy = New-AzureRmPolicyDefinition -Name regionPolicyDefinition -Description "Policy to allow resource creation only in certain regions" -Policy "c:\policies\storageskupolicy.json"
```

### <a name="assign-policy"></a>Zuweisen der Richtlinie

Sie wenden die Richtlinie über das Cmdlet `New-AzureRmPolicyAssignment` auf den gewünschten Bereich an:

```powershell
New-AzureRmPolicyAssignment -Name regionPolicyAssignment -PolicyDefinition $policy -Scope /subscriptions/{subscription-id}/resourceGroups/{resource-group-name}
```

### <a name="view-policy-assignment"></a>Anzeigen der Richtlinienzuweisung

Um eine Richtlinie abzurufen, verwenden Sie das folgende Cmdlet:

```powershell
(Get-AzureRmPolicyAssignment -Id "/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/{definition-name}").Properties.policyRule | ConvertTo-Json
```

Damit wird die JSON für die Richtliniendefinition zurückgegeben.

### <a name="remove-policy-assignment"></a>Entfernen der Richtlinienzuweisung 

Um eine Richtlinienzuweisung zu entfernen, verwenden Sie Folgendes:

```powershell
Remove-AzureRmPolicyAssignment -Name regionPolicyAssignment -Scope /subscriptions/{subscription-id}/resourceGroups/{resource-group-name}
```

## <a name="azure-cli-20-preview"></a>Azure CLI 2.0 (Vorschau)

### <a name="create-policy-definition"></a>Erstellen einer Richtliniendefinition

Sie können eine Richtliniendefinition mit dem entsprechenden Befehl über die Azure CLI 2.0 (Vorschau) erstellen. Im folgenden Beispiel wird eine Richtlinie erstellt, um nur Ressourcen in Nord- und Westeuropa zuzulassen.

```azurecli
az policy definition create --name regionPolicyDefinition --description "Policy to allow resource creation only in certain regions" --rules '{    
  "if" : {
    "not" : {
      "field" : "location",
      "in" : ["northeurope" , "westeurope"]
    }
  },
  "then" : {
    "effect" : "deny"
  }
}'    
```

### <a name="assign-policy"></a>Zuweisen der Richtlinie

Sie können die Richtlinie auf den gewünschten Bereich anwenden, indem Sie den Befehl zur Richtlinienzuweisung verwenden:

```azurecli
az policy assignment create --name regionPolicyAssignment --policy regionPolicyDefinition --scope /subscriptions/{subscription-id}/resourceGroups/{resource-group-name}
```

### <a name="view-policy-definition"></a>Anzeigen einer Richtliniendefinition
Um eine Richtliniendefinition abzurufen, verwenden Sie den folgenden Befehl:

```azurecli
az policy definition show --name regionPolicyAssignment
```

### <a name="remove-policy-assignment"></a>Entfernen der Richtlinienzuweisung 

Um eine Richtlinienzuweisung zu entfernen, verwenden Sie Folgendes:

```azurecli
az policy assignment delete --name regionPolicyAssignment --scope /subscriptions/{subscription-id}/resourceGroups/{resource-group-name}
```

## <a name="azure-cli-10"></a>Azure-Befehlszeilenschnittstelle 1.0

### <a name="create-policy-definition"></a>Erstellen einer Richtliniendefinition

Sie können eine Richtliniendefinition mit dem entsprechenden Befehl in der Azure-Befehlszeilenschnittstelle erstellen. Im folgenden Beispiel wird eine Richtlinie erstellt, um nur Ressourcen in Nord- und Westeuropa zuzulassen.

```azurecli
azure policy definition create --name regionPolicyDefinition --description "Policy to allow resource creation only in certain regions" --policy-string '{    
  "if" : {
    "not" : {
      "field" : "location",
      "in" : ["northeurope" , "westeurope"]
    }
  },
  "then" : {
    "effect" : "deny"
  }
}'    
```

Sie können auch den Pfad zu einer JSON-Datei angeben, die die Richtlinie enthält, anstatt die Richtlinie inline anzugeben.

```azurecli
azure policy definition create --name regionPolicyDefinition --description "Policy to allow resource creation only in certain regions" --policy "path-to-policy-json-on-disk"
```

### <a name="assign-policy"></a>Zuweisen der Richtlinie

Sie können die Richtlinie auf den gewünschten Bereich anwenden, indem Sie den Befehl zur Richtlinienzuweisung verwenden:

```azurecli
azure policy assignment create --name regionPolicyAssignment --policy-definition-id /subscriptions/{subscription-id}/providers/Microsoft.Authorization/policyDefinitions/{policy-name} --scope    /subscriptions/{subscription-id}/resourceGroups/{resource-group-name}
```

Der Bereich ist in diesem Fall der Name der von Ihnen angegebenen Ressourcengruppe. Wenn der Wert des Parameters „policy-definition-id“ unbekannt ist, können Sie ihn über die Azure-Befehlszeilenschnittstelle abrufen. 

```azurecli
azure policy definition show {policy-name}
```

### <a name="view-policy"></a>Anzeigen der Richtlinie
Um eine Richtlinie abzurufen, verwenden Sie den folgenden Befehl:

```azurecli
azure policy definition show {definition-name} --json
```

### <a name="remove-policy-assignment"></a>Entfernen der Richtlinienzuweisung 

Um eine Richtlinienzuweisung zu entfernen, verwenden Sie Folgendes:

```azurecli
azure policy assignment delete --name regionPolicyAssignment --scope /subscriptions/{subscription-id}/resourceGroups/{resource-group-name}
```

## <a name="next-steps"></a>Nächste Schritte
* Anleitungen dazu, wie Unternehmen Abonnements mit Resource Manager effektiv verwalten können, finden Sie unter [Azure-Unternehmensgerüst - Präskriptive Abonnementgovernance](resource-manager-subscription-governance.md).




<!--HONumber=Feb17_HO3-->


