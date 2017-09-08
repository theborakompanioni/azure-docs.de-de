---
title: "Erstellen und Veröffentlichen einer verwalteten Azure-Dienstkataloganwendung | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie eine verwaltete Azure-Anwendung erstellen, die für Mitglieder Ihrer Organisation vorgesehen ist."
services: azure-resource-manager
author: ravbhatnagar
manager: rjmax
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 08/23/2017
ms.author: gauravbh; tomfitz
ms.translationtype: HT
ms.sourcegitcommit: 646886ad82d47162a62835e343fcaa7dadfaa311
ms.openlocfilehash: 39b74984ec2f89ed39753963de7fe3ff79577c9e
ms.contentlocale: de-de
ms.lasthandoff: 08/24/2017

---
# <a name="publish-a-managed-application-for-internal-consumption"></a>Veröffentlichen einer verwalteten Anwendung für die interne Nutzung

Sie können [verwaltete Azure-Anwendungen](managed-application-overview.md) erstellen und veröffentlichen, die für Mitglieder Ihrer Organisation vorgesehen sind. Die IT-Abteilung kann beispielsweise verwaltete Anwendungen veröffentlichen, die die Konformität mit Ihren Unternehmensstandards gewährleisten. Diese verwalteten Anwendungen stehen über den Dienstkatalog, aber nicht über den Azure Marketplace zur Verfügung.

Um eine verwaltete Anwendung für den Dienstkatalog zu veröffentlichen, gehen Sie folgendermaßen vor:

* Erstellen Sie ein ZIP-Paket, das die drei erforderlichen Vorlagendateien enthält.
* Entscheiden Sie, welche Benutzer, Gruppen oder Anwendungen Zugriff auf die Ressourcengruppe im Benutzerabonnement benötigen.
* Erstellen Sie die verwaltete Anwendungsdefinition, die auf das ZIP-Paket verweist und Zugriff auf die Identität anfordert.

## <a name="create-a-managed-application-package"></a>Erstellen eines Pakets für eine verwaltete Anwendung

Im ersten Schritt werden die drei erforderlichen Vorlagendateien erstellt. Komprimieren Sie alle drei Dateien zu einer ZIP-Datei, und laden Sie sie an einen zugänglichen Speicherort hoch, z.B. in ein Speicherkonto. Sie übergeben einen Link zu dieser ZIP-Datei, wenn Sie die Definition der verwalteten Anwendung erstellen.

* **applianceMainTemplate.json:** Diese Datei definiert die Azure-Ressourcen, die als Teil der verwalteten Anwendung bereitgestellt werden. Die Vorlage unterscheidet sich nicht von anderen regulären Resource Manager-Vorlagen. Beispielsweise enthält die Datei „applianceMainTemplate.json“ zum Erstellen eines Speicherkontos mithilfe einer verwalteten Anwendung Folgendes:

  ```json
  {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageAccountNamePrefix": {
            "type": "string"
        }
    },
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[concat(parameters('storageAccountNamePrefix'), uniqueString(resourceGroup().id))]",
            "apiVersion": "2016-01-01",
            "location": "[resourceGroup().location]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {}
        }
    ],
    "outputs": {}
  }
  ```

* **mainTemplate.json:** Benutzer stellen diese Vorlage beim Erstellen der verwalteten Anwendung bereit. Sie definiert die Ressource der verwalteten Anwendung. Dabei handelt es sich um den Ressourcentyp „Microsoft.Solutions/appliances“. Die Datei enthält alle erforderlichen Parameter für die Ressourcen in der Datei „applianceMainTemplate.json“.

  Sie legen zwei wichtige Eigenschaften in dieser Vorlage fest. Die **applianceDefinitionId**-Eigenschaft ist die ID der Definition der verwalteten Anwendung. Sie erstellen die Definition weiter unten in diesem Thema. Beim Festlegen dieses Werts müssen Sie entscheiden, welches Abonnement und welche Ressourcengruppe Sie für das Speichern der Definitionen verwalteter Anwendungen verwenden möchten. Außerdem müssen Sie einen Namen für die Definition festlegen. Das Format der ID ist:

  `/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Solutions/applianceDefinitions/<definition-name>`

  Die **managedResourceGroupId**-Eigenschaft ist die ID der Ressourcengruppe, in der die Azure-Ressourcen erstellt werden. Sie können diesem Ressourcengruppennamen einen Wert zuweisen oder die Benutzer einen Namen angeben lassen. Das Format der ID ist:

  `/subscriptions/<subscription-id>/resourceGroups/<resoure-group-name>`.

  Das folgende Beispiel zeigt die Datei „mainTemplate.json“. Sie gibt eine Ressourcengruppe für die bereitgestellten Ressourcen an. Für die Definitions-ID wurde die Verwendung einer Definition mit dem Namen **storageApp** in einer Ressourcengruppe mit dem Namen **managedApplicationGroup** festgelegt. Sie können diese Werte ändern, wenn Sie andere Namen verwenden möchten. Geben Sie eine eigene Abonnement-ID für die Definitions-ID an.

  ```json
  {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageAccountNamePrefix": {
            "type": "string"
        }
    },
    "variables": {
        "managedRGId": "[concat(resourceGroup().id,'-application-resources')]",
        "managedAppName": "[concat('managedStorage', uniqueString(resourceGroup().id))]"
    },
    "resources": [
        {
            "type": "Microsoft.Solutions/appliances",
            "name": "[variables('managedAppName')]",
            "apiVersion": "2016-09-01-preview",
            "location": "[resourceGroup().location]",
            "kind": "ServiceCatalog",
            "properties": {
                "managedResourceGroupId": "[variables('managedRGId')]",
                "applianceDefinitionId": "/subscriptions/<subscription-id>/resourceGroups/managedApplicationGroup/providers/Microsoft.Solutions/applianceDefinitions/storageApp",
                "parameters": {
                    "storageAccountNamePrefix": {
                        "value": "[parameters('storageAccountNamePrefix')]"
                    }
                }
            }
        }
    ]
  }
  ```

* **applianceCreateUiDefinition.json:** Das Azure-Portal verwendet diese Datei zum Generieren der Benutzeroberfläche für Benutzer, die die verwaltete Anwendung erstellen. Sie legen fest, wie Benutzer die Eingaben für die einzelnen Parameter bereitstellen. Sie können Optionen wie eine Dropdownliste, ein Textfeld, ein Kennwortfeld und andere Eingabetools verwenden. Informationen zum Erstellen einer UI-Definitionsdatei für eine verwaltete Anwendung finden Sie unter [Erste Schritte mit „CreateUiDefinition“](managed-application-createuidefinition-overview.md).

  Das folgende Beispiel zeigt eine Datei „applianceCreateUiDefinition.json“, die es Benutzern ermöglicht, das Namenspräfix für das Speicherkonto über ein Textfeld einzugeben.

  ```json
  {
    "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json",
    "handler": "Microsoft.Compute.MultiVm",
    "version": "0.1.2-preview",
    "parameters": {
        "basics": [
            {
                "name": "storageAccounts",
                "type": "Microsoft.Common.TextBox",
                "label": "Storage account name prefix",
                "defaultValue": "storage",
                "toolTip": "Provide a value that is used for the prefix of your storage account. Limit to 11 characters.",
                "constraints": {
                    "required": true,
                    "regex": "^[a-z0-9A-Z]{1,11}$",
                    "validationMessage": "Only alphanumeric characters are allowed, and the value must be 1-11 characters long."
                },
                "visible": true
            }
        ],
        "steps": [],
        "outputs": {
            "storageAccountNamePrefix": "[basics('storageAccounts')]"
        }
    }
  }
  ```

Nachdem alle erforderlichen Dateien bereit sind, packen Sie sie als eine ZIP-Datei. Die drei Dateien müssen sich auf der Stammebene der ZIP-Datei befinden. Wenn Sie sie in einem Ordner speichern, erhalten Sie, wenn Sie die Definition der verwalteten Anwendung erstellen, eine Fehlermeldung, dass die erforderlichen Dateien nicht vorhanden sind. Laden Sie das Paket an einen zugänglichen Speicherort hoch, wo es verwendet werden kann. In diesem Artikel wird davon ausgegangen, dass die ZIP-Datei in einem öffentlich zugänglichen Storage-Blobcontainer verfügbar ist.

## <a name="create-an-azure-active-directory-user-group-or-application"></a>Erstellen einer Azure Active Directory-Benutzergruppe oder -Anwendung

Der zweite Schritt besteht darin, eine Benutzergruppe oder Anwendung für das Verwalten der Ressourcen für den Kunden zu erstellen. Diese Benutzergruppe oder Anwendung verfügt über Berechtigungen für die verwaltete Ressourcengruppe, die durch die zugewiesene Rolle festgelegt sind. Es kann sich um eine beliebige integrierte rollenbasierte Zugriffssteuerungsrolle (Role-Based Access Control, RBAC) wie „Besitzer“ oder „Mitwirkender“ handeln. Sie können einem einzelnen Benutzer auch Berechtigungen zum Verwalten der Ressourcen zuweisen, in der Regel weisen Sie diese Berechtigungen jedoch einer Benutzergruppe zu. Weitere Informationen zum Erstellen einer neuen Active Directory-Benutzergruppe finden Sie unter [Erstellen einer Gruppe und Hinzufügen von Mitgliedern in Azure Active Directory](../active-directory/active-directory-groups-create-azure-portal.md).

Sie benötigen zum Verwalten der Ressourcen die Objekt-ID der Benutzergruppe. Das folgende Beispiel zeigt, wie die Objekt-ID vom Anzeigenamen der Gruppe abgerufen wird:

```azurecli-interactive
az ad group show --group exampleGroupName
```

Der Beispielbefehl gibt die folgende Ausgabe zurück:

```azurecli
{
    "displayName": "exampleGroupName",
    "mail": null,
    "objectId": "9aabd3ad-3716-4242-9d8e-a85df479d5d9",
    "objectType": "Group",
    "securityEnabled": true
}
```

Verwenden Sie zum Abrufen der Objekt-ID Folgendes:

```azurecli-interactive
groupid=$(az ad group show --group exampleGroupName --query objectId --output tsv)
```

## <a name="get-the-role-definition-id"></a>Abrufen der Rollendefinitions-ID

Als Nächstes benötigen Sie die Rollendefinitions-ID der integrierten RBAC-Rolle, der Sie Zugriff auf den Benutzer, die Benutzergruppe oder Anwendung gewähren möchten. In der Regel verwenden Sie die Rolle „Besitzer, „Mitwirkender“ oder „Leser“. Der folgende Befehl zeigt, wie die Rollendefinitions-ID für die Rolle „Besitzer“ abgerufen wird:


```azurecli-interactive
az role definition list --name owner
```

Der Befehl gibt die folgende Ausgabe zurück:

```azurecli
{
    "id": "/subscriptions/<subscription-id>/providers/Microsoft.Authorization/roleDefinitions/8e3af657-a8ff-443c-a75c-2fe8c4bcb635",
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

Sie benötigen den Wert der Eigenschaft „name“ aus dem vorhergehenden Beispiel. Sie können nur diese Eigenschaft wie folgt abrufen:

```azurecli-interactive
roleid=$(az role definition list --name Owner --query [].name --output tsv)
```

## <a name="create-the-managed-application-definition"></a>Erstellen der Definition für die verwaltete Anwendung

Wenn Sie noch über keine Ressourcengruppe für das Speichern der Definition Ihrer verwalteten Anwendung verfügen, erstellen Sie sie jetzt:

```azurecli-interactive
az group create --name managedApplicationGroup --location westcentralus
```

Erstellen Sie nun die Definitionsressource für die verwaltete Anwendung.

```azurecli-interactive
az managedapp definition create \
  --name storageApp \
  --location "westcentralus" \
  --resource-group managedApplicationGroup \
  --lock-level ReadOnly \
  --display-name myteststorageapp \
  --description storageapp \
  --authorizations "$groupid:$roleid" \
  --package-file-uri <uri-path-to-zip-file>
```

Die im vorhergehenden Beispiel verwendeten Parameter sind:

* **resource-group:** Der Name der Ressourcengruppe, in der die Definition für die verwaltete Anwendung erstellt wird.
* **lock-level**: Der Typ der Sperre, der auf die verwaltete Ressourcengruppe angewendet wird. Er verhindert, dass der Kunde unerwünschte Vorgänge für diese Ressourcengruppe durchführt. Derzeit ist „ReadOnly“ die einzige unterstützte Sperrebene. Wenn „ReadOnly“ festgelegt wird, sind die in der verwalteten Ressourcengruppe vorhandenen Ressourcen für den Kunden schreibgeschützt.
* **authorizations**: Beschreibt die Prinzipal-ID und die Rollendefinitions-ID, die zum Erteilen von Berechtigungen für die verwaltete Ressourcengruppe verwendet werden. Diese ist im Format `<principalId>:<roleDefinitionId>` angegeben. Für diese Eigenschaft können auch mehrere Werte angegeben werden. Wenn mehrere Werte erforderlich sind, sollten sie in diesem Format angegeben werden: `<principalId1>:<roleDefinitionId1> <principalId2>:<roleDefinitionId2>`. Mehrere Werte werden durch ein Leerzeichen voneinander getrennt.
* **package-file-uri:** Der Speicherort des Pakets für die verwaltete Anwendung mit den Vorlagendateien. Dabei kann es sich um Azure Storage Blob handeln.

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung in verwaltete Anwendungen finden Sie in der [Übersicht über verwaltete Anwendungen](managed-application-overview.md).
* Beispiele für die Dateien finden Sie unter [Beispiele für verwaltete Anwendungen](https://github.com/Azure/azure-managedapp-samples/tree/master/samples).
* Informationen zum Nutzen einer verwalteten Dienstkataloganwendung, finden Sie unter [Nutzen einer verwalteten Dienstkataloganwendung](managed-application-consumption.md).
* Informationen zum Veröffentlichen von verwalteten Anwendungen im Azure Marketplace finden Sie unter [Verwaltete Azure-Anwendungen im Marketplace](managed-application-author-marketplace.md).
* Informationen zur Nutzung einer verwalteten Anwendung aus dem Marketplace finden Sie unter [Nutzen verwalteter Azure-Anwendungen im Marketplace](managed-application-consume-marketplace.md).
* Informationen zum Erstellen einer UI-Definitionsdatei für eine verwaltete Anwendung finden Sie unter [Erste Schritte mit „CreateUiDefinition“](managed-application-createuidefinition-overview.md).

