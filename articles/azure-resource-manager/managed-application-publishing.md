---
title: "Erstellen und Veröffentlichen von verwalteten Azure-Anwendungen | Microsoft-Dokumentation"
description: Zeigt, wie ein ISV oder Partner eine verwaltete Azure-Anwendung erstellt
services: azure-resource-manager
author: ravbhatnagar
manager: rjmax
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 05/08/2017
ms.author: gauravbh; tomfitz
ms.translationtype: Human Translation
ms.sourcegitcommit: 97fa1d1d4dd81b055d5d3a10b6d812eaa9b86214
ms.openlocfilehash: 56dd68e328abd6c1dacdf7a8e051ca6b3cd07083
ms.contentlocale: de-de
ms.lasthandoff: 05/11/2017


---
# <a name="create-and-publish-an-azure-managed-application"></a>Erstellen und Veröffentlichen von verwalteten Azure-Anwendungen 

Wie im Artikel mit der [Übersicht über verwaltete Anwendungen](managed-application-overview.md) beschrieben umfasst der gesamte Vorgang zwei Szenarien. Das eine ist der Herausgeber oder ISV, der eine verwaltete Anwendung zur Verwendung durch Kunden erstellen möchte. Das zweite ist der Kunde oder der Verbraucher der verwalteten Anwendung. Der Schwerpunkt dieses Artikels liegt auf dem ersten Szenario erläutert, wie ein ISV eine verwaltete Anwendung erstellen und veröffentlichen kann. 

Zum Erstellen einer verwalteten Anwendung müssen Sie zuerst Folgendes erstellen:

* ein Paket, das die Vorlagendateien enthält
* den Benutzer, die Gruppe oder die Anwendung mit Zugriff auf die Ressourcengruppe im Abonnement des Kunden
* die Gerätedefinition

Beispiele für die Dateien, finden Sie unter [Managed Application samples](https://github.com/Azure/azure-managedapp-samples/tree/master/samples) (Beispiele für verwaltete Anwendungen).

## <a name="create-managed-application-package"></a>Paket für verwaltete Anwendung erstellen

Der erste Schritt ist das Erstellen des Pakets für die verwaltete Anwendung, das die wichtigsten Vorlagendateien enthält. Der Herausgeber oder ISV erstellt drei Dateien. 

* Die erste Datei heißt **applianceMainTemplate.json**. Diese Vorlagendatei definiert die tatsächlichen Ressourcen, die als Teil der verwalteten Anwendung bereitgestellt werden. Beispiel: Für das Erstellen eines Speicherkontos mithilfe einer verwalteten Anwendung enthält die Datei „applianceMainTemplate.json“: 

  ```json
  {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
            "storageAccountName": {
                "type": "String"
          }
      },
      "resources": [{
          "type": "Microsoft.Storage/storageAccounts",
          "name": "[parameters('storageAccountName')]",
          "apiVersion": "2016-01-01",
          "location": "westus",
          "sku": {
              "name": "Standard_LRS"
          },
          "kind": "Storage",
          "properties": {        
          }
      }],
      "outputs": {        
      }
  }
  ```

* Die zweite Datei, die der Herausgeber erstellen muss, ist **mainTemplate.json**. Die Vorlagendatei enthält nur die Geräteressource (Microsoft.Solutions/appliances). Außerdem enthält sie alle erforderlichen Parameter für die Ressourcen in der Datei „applianceMainTemplate.json“. 

  Zwei wichtige Eigenschaften sind beim Erstellen der verwalteten Anwendung als Eingabe erforderlich. Die Eigenschaft **ManagedResourceGroupId** ist die ID der Ressourcengruppe, in der die in der Datei „applianceMainTemplate.json“ definierten Ressourcen erstellt werden. Das Format der ID ist: `/subscriptions/{subscriptionId}/resourceGroups/{resoureGroupName}`

  Die Eigenschaft **ApplianceDefinitionId** ist die ID der Definitionsressource der verwalteten Anwendung. Das Format der ID ist: `/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Solutions/applianceDefinitions/{applianceDefinitionName}`

  Geben Sie Parameter für die diese beiden Werte an, damit der Verbraucher diese beim Erstellen einer verwalteten Anwendung festlegen kann. Im folgenden Beispiel sind die beiden Parameter, die diesen Eigenschaften entsprechen, „ManagedByResourceGroup“ und „ApplianceDefinitonId“.

  ```json
  {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
          "storageAccountName": {
              "type": "String"
          },
          "applianceDefinitionId": {
              "type": "String"
          },
          "managedByResourceGroup": {
              "type": "String"
          },
          "applianceName": {
              "type": "String"
          },
      },
      "variables": {            
      },
      "resources": [{
          "type": "Microsoft.Solutions/appliances",
          "name": "[parameters('applianceName')]",
          "apiVersion": "2016-09-01-preview",
          "location": "[resourceGroup().location]",
          "kind": "ServiceCatalog",
          "properties": {
              "ManagedResourceGroupId": "[parameters('managedByResourceGroup')]",
              "applianceDefinitionId": "[parameters('applianceDefinitionId')]",
              "Parameters": {
                  "storageAccountName": {
                      "value": "[parameters('storageAccountName')]"
                  }                
              }
          }
      }]
  }
  ```

* Die dritte im Paket benötigte Datei ist **createUiDefinition.json**. Das Azure-Portal verwendet diese Datei zum Generieren der Benutzeroberfläche für Verbraucher, die die verwaltete Anwendung erstellen. Sie definieren die Parameter für die verwaltete Anwendung und wie Verbraucher die Eingabe für jeden Parameter abrufen können. Sie können Optionen wie eine Dropdown-Auswahl, ein Textfeld, ein Kennwortfeld und andere Eingabetools verwenden. Informationen zum Erstellen einer Benutzeroberflächen-Definitionsdatei finden Sie unter [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md) (Erste Schritte mit „CreateUiDefinition“).

Sobald alle benötigten Dateien bereitstehen, laden Sie das Paket in einen zugänglichen Speicherort hoch, wo es verwendet werden kann.


## <a name="create-azure-ad-user-group-or-application"></a>Erstellen einer Azure AD-Benutzergruppe oder -Anwendung
Als Nächstes erstellen Sie eine Benutzergruppe oder Anwendung, die Sie zum Verwalten der Ressourcen für den Kunden verwenden. Diese Benutzergruppe oder Anwendung hat Berechtigungen für die verwaltete Ressourcengruppe wie von der Rolle beschrieben. Die Rolle kann eine beliebige integrierte RBAC-Rolle wie **Besitzer** oder **Mitwirkender** sein. Einem einzelnen Benutzer können auch Berechtigungen zum Verwalten der Ressourcen zugewiesen werden, in der Regel weisen Sie diese Berechtigungen jedoch einer Benutzergruppe zu. Zum Erstellen einer neuen Active Directory-Benutzergruppe verwenden Sie:

```azurecli
az ad group create –display-name "name" –mail-nickname "nickname"
```

Sie können auch eine vorhandene Gruppe verwenden. Sie benötigen die Objekt-ID der neu erstellten oder einer vorhandenen Benutzergruppe. Das folgende Beispiel zeigt, wie die Objekt-ID vom Anzeigenamen abgerufen wird, der zum Erstellen der Gruppe verwendet wurde.

```azurecli
az ad group show –group "groupName"
```

Beispiel:

```azurecli
az ad group show --group ravAppliancetestADgroup
```

Damit wird die folgende Ausgabe zurückgegeben:

```json 
{
    "displayName": "ravAppliancetestADgroup",
    "mail": null,
    "objectId": "9aabd3ad-3716-4242-9d8e-a85df479d5d9",
    "objectType": "Group",
    "securityEnabled": true
}
```
    
Sie benötigen den objectId-Wert von oben. 

## <a name="get-the-role-definition-id"></a>Abrufen der Rollendefinitions-ID

Als Nächstes benötigen Sie die Rollendefinitions-ID der integrierten RBAC-Rolle, der Sie Zugriff auf den Benutzer, die Benutzergruppe oder Anwendung gewähren möchten. In der Regel sollten Sie die Rolle „Besitzer, „Mitwirkender“ oder „Leser“ verwenden. Der folgende Befehl zeigt, wie die Rollendefinitions-ID für die Rolle „Besitzer“ abgerufen wird:

```azurecli
az role definition list --name owner
```

Damit wird die folgende Ausgabe zurückgegeben:

```json
{
    "id": "/subscriptions/{subscription-id}/providers/Microsoft.Authorization/roleDefinitions/8e3af657-a8ff-443c-a75c-2fe8c4bcb635",
    "name": "8e3af657-a8ff-443c-a75c-2fe8c4bcb635",
    "properties": {
      "assignableScopes": [
    "/"
      ],
      "description": "Lets you manage everything, including access to resources.",
      "permissions": [
    {
      "actions": [
    "*"
      ],
      "notActions": []
    }
      ],
      "roleName": "Owner",
      "type": "BuiltInRole"
    },
    "type": "Microsoft.Authorization/roleDefinitions"
}
```

Sie benötigen den Wert der Eigenschaft „name“ aus dem vorhergehenden Beispiel.


## <a name="create-the-managed-application-definition"></a>Erstellen der Definition für die verwaltete Anwendung

Im letzten Schritt wird die Definitionsressource der verwalteten Anwendung erstellt. Nachdem Sie das Gerätepaket und die Autorisierungen erstellt haben, erstellen Sie die Gerätedefinition mit dem folgenden Befehl: 

```azurecli
az managedapp definition create -n ravtestAppDef4 -l "westcentralus" 
    --resource-group ravApplianceDefRG3 --lock-level ReadOnly 
    --display-name ravtestappdef --description ravtestdescription  
    --authorizations "9aabd3ad-3716-4242-9d8e-a85df479d5d9:8e3af657-a8ff-443c-a75c-2fe8c4bcb635" 
    --package-file-uri "{path to package}" --debug 
```

Die im vorhergehenden Beispiel verwendeten Parameter sind:

- „resource-group“: Name der Ressourcengruppe, in dem die Gerätedefinition erstellt wird.
- „lock-level“: Sperrentyp, der auf die verwaltete Ressourcengruppe angewendet wird. Er verhindert, dass der Kunde unerwünschte Vorgänge für diese Ressourcengruppe durchführt. Derzeit ist **ReadOnly** die einzige unterstützte Sperrebene. Wenn „ReadOnly“ festgelegt wird, sind die in der verwalteten Ressourcengruppe vorhandenen Ressourcen für den Kunden schreibgeschützt.
- „authorizations“: Beschreibt die Prinzipal-ID und die Rollendefinitions-ID, die zum Erteilen von Berechtigungen für die verwaltete Ressourcengruppe verwendet werden. Dieser Parameter ist im Format `<principalId>:<roleDefinitionId>` angegeben. Für diese Eigenschaft können auch mehrere Werte angegeben werden. Wenn mehrere Werte erforderlich sind, sollten sie in diesem Format angegeben werden: `<principalId1>:<roleDefinitionId1> <principalId2>:<roleDefinitionId2>`. Mehrere Werte werden durch ein Leerzeichen voneinander getrennt.
- „package-file-uri“: Speicherort des Gerätepakets mit den Vorlagendateien. Dabei kann es sich um einen Azure Storage Blob handeln. 


## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung in verwaltete Anwendungen finden Sie in der [Übersicht über verwaltete Azure-Anwendungen](managed-application-overview.md).
* Beispiele für die Dateien, finden Sie unter [Managed Application samples](https://github.com/Azure/azure-managedapp-samples/tree/master/samples) (Beispiele für verwaltete Anwendungen).
* Grundlegendes zur Kundenerfahrung erfahren Sie unter [Nutzen einer verwalteten Azure-Anwendung](managed-application-consumption.md).
* Informationen zum Erstellen einer Benutzeroberflächen-Definitionsdatei für eine verwaltete Anwendung finden Sie unter [Erste Schritte mit „CreateUiDefinition“](managed-application-createuidefinition-overview.md).
