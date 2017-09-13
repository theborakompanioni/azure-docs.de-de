---
title: Azure Resource Manager-Vorlage mit VS Code-Erweiterung | Microsoft-Dokumentation
description: "Verwenden Sie die Azure Resource Manager-Tools-Erweiterung für Resource Manager-Vorlagen."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 09/06/2017
ms.topic: get-started-article
ms.author: tomfitz
ms.translationtype: HT
ms.sourcegitcommit: eeed445631885093a8e1799a8a5e1bcc69214fe6
ms.openlocfilehash: fbc0fd12999c9085c3c364f0d7115eb1ab1ddd74
ms.contentlocale: de-de
ms.lasthandoff: 09/07/2017

---

# <a name="use-visual-studio-code-extension-to-create-azure-resource-manager-template"></a>Verwenden der Visual Studio Code-Erweiterung für die Erstellung einer Azure Resource Manager-Vorlage
Dieser Artikel enthält Informationen zu den Vorteilen, die sich durch das Installieren und Nutzen der Azure Resource Manager-Tools-Erweiterung in Visual Studio Code ergeben. Sie können Resource Manager-Vorlagen in VS Code auch ohne die Erweiterung erstellen, aber die Erweiterung verfügt über Optionen für die automatische Vervollständigung, die Ihnen die Entwicklung von Vorlagen vereinfachen. Hierbei werden Funktionen, Parameter und Variablen vorgeschlagen, die in der Vorlage verfügbar sind.

Zum Durcharbeiten dieses Artikels benötigen Sie [Visual Studio Code](https://code.visualstudio.com/).

Weitere Informationen zu den Konzepten der Bereitstellung und Verwaltung Ihrer Azure-Lösungen finden Sie unter [Übersicht über Azure Resource Manager](resource-group-overview.md).

## <a name="create-the-template"></a>Erstellen der Vorlage

Dieser Artikel baut auf der Vorlage auf, die Sie unter [Erstellen und Bereitstellen Ihrer ersten Azure Resource Manager-Vorlage](resource-manager-create-first-template.md) erstellt haben. Sie können diesen Abschnitt überspringen, falls Sie bereits über diese Vorlage verfügen.

1. Starten Sie VS Code, falls Sie die Vorlage erstellen möchten. Wählen Sie **Datei** > **Neue Datei** aus. 

   ![Neue Datei](./media/resource-manager-vscode-extension/new-file.png)

2. Kopieren Sie die folgende JSON-Syntax, und fügen Sie sie in Ihre Datei ein:

   ```json
   {
     "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
     "contentVersion": "1.0.0.0",
     "parameters": {
       "storageSKU": {
         "type": "string",
         "allowedValues": [
           "Standard_LRS",
           "Standard_ZRS",
           "Standard_GRS",
           "Standard_RAGRS",
           "Premium_LRS"
         ],
         "defaultValue": "Standard_LRS",
         "metadata": {
           "description": "The type of replication to use for the storage account."
         }
       },   
       "storageNamePrefix": {
         "type": "string",
         "maxLength": 11,
         "defaultValue": "storage",
         "metadata": {
           "description": "The value to use for starting the storage account name. Use only lowercase letters and numbers."
         }
       }
     },
     "variables": {
       "storageName": "[concat(toLower(parameters('storageNamePrefix')), uniqueString(resourceGroup().id))]"
     },
     "resources": [
       {
         "name": "[variables('storageName')]",
         "type": "Microsoft.Storage/storageAccounts",
         "apiVersion": "2016-01-01",
         "sku": {
           "name": "[parameters('storageSKU')]"
         },
         "kind": "Storage",
         "location": "[resourceGroup().location]",
         "tags": {},
         "properties": {
           "encryption":{
             "services":{
               "blob":{
                 "enabled":true
               }
             },
             "keySource":"Microsoft.Storage"
           }
         }
       }
     ],
     "outputs": {  }
   }
   ```

3. Speichern Sie diese Datei als **azuredeploy.json** in einem lokalen Ordner.

   ![Vorlage speichern](./media/resource-manager-vscode-extension/save-template.png)

## <a name="install-the-extension"></a>Installieren der Erweiterung

1. Wählen Sie in VS Code die Option **Erweiterungen**.

   ![Auswählen von „Erweiterungen“](./media/resource-manager-vscode-extension/select-extensions.png)

2. Suchen Sie nach **Azure Resource Manager-Tools**, und wählen Sie die Option **Installieren**.

   ![Installieren der Erweiterung](./media/resource-manager-vscode-extension/install-extension.png)

3. Wählen Sie die Option **Erneut laden**, um die Installation der Erweiterung abzuschließen.

## <a name="edit-the-template"></a>Bearbeiten der Vorlage

1. Öffnen Sie die Datei „azuredeploy.json“.

2. Die Erweiterung ruft alle verfügbaren [Vorlagenfunktionen](resource-group-template-functions.md) ab. Außerdem liest sie die Parameter und Variablen, die Sie in der Vorlage definiert haben. Zum Anzeigen dieser Funktionalität fügen Sie dem Abschnitt „output“ zwei Werte hinzu. Ersetzen Sie den Abschnitt „outputs“ in der Vorlage wie folgt:

   ```json
   "outputs": { 
       "groupLocation": {
         "type": "string",
         "value": ""
       },
       "storageUri": {
         "type": "string",
         "value": ""
       }
   }
   ```

3. Platzieren Sie den Cursor innerhalb der Anführungszeichen für den Wert von **groupLocation**. Geben Sie die öffnende Klammer (`[`) ein. Beachten Sie, dass von der Erweiterung sofort verfügbare Vorlagenfunktionen vorgeschlagen werden.

   ![Anzeigen von verfügbaren Funktionen](./media/resource-manager-vscode-extension/available-functions.png)

4. Beginnen Sie mit der Eingabe von **resourceGroup**. Wenn die Funktion `resourceGroup()` angezeigt wird, können Sie die TAB- oder EINGABETASTE drücken.

   ![Auswählen von resourceGroup-Funktionen](./media/resource-manager-vscode-extension/show-resourcegroup.png)

5. Die Erweiterung fügt die Funktionssyntax ein. Die Funktion [resourceGroup](resource-group-template-functions-resource.md#resourcegroup) akzeptiert keine Parameter. Fügen Sie nach der schließenden Klammer einen Punkt hinzu. Die Erweiterung stellt die Eigenschaften bereit, die für das von der Funktion `resourceGroup()` zurückgegebene Objekt verfügbar sind. Wählen Sie `location` aus.

   ![Eigenschaften auswählen](./media/resource-manager-vscode-extension/resourcegroup-properties.png)

6. Fügen Sie nach **location** die schließende Klammer hinzu.

   ```json
   "outputs": { 
       "groupLocation": {
         "type": "string",
         "value": "[resourceGroup().location]"
       },
       "storageUri": {
         "type": "string",
         "value": ""
       }
   }
   ```

7. Platzieren Sie den Cursor nun innerhalb der Anführungszeichen für **storageUri**. Geben Sie wieder die öffnende Klammer ein. Beginnen Sie mit der Eingabe von **reference**. Wenn diese Funktion ausgewählt ist, können Sie die TAB- oder EINGABETASTE drücken.

   ![Auswählen von „reference“](./media/resource-manager-vscode-extension/add-reference.png)

8. [reference](resource-group-template-functions-resource.md#reference) akzeptiert die Ressourcen-ID oder den Ressourcennamen als Parameter. Sie verfügen in einer Variablen bereits über den Namen des Speicherkontos. Geben Sie **var** ein, und drücken Sie anschließend STRG+LEERTASTE. Die Erweiterung schlägt die Funktion „variables“ vor.

   ![Auswählen der Variablen](./media/resource-manager-vscode-extension/add-variable.png)

   Drücken Sie die TAB- oder EINGABETASTE.

9. Für die Funktion [variables](resource-group-template-functions-deployment.md#variables) ist der Name der Variablen erforderlich. Fügen Sie innerhalb der Klammern ein einfaches Anführungszeichen hinzu. Die Erweiterung stellt die Namen der Variablen bereit, die Sie in der Vorlage definiert haben.

   ![Anzeigen von Variablen](./media/resource-manager-vscode-extension/show-variables.png) 

10. Wählen Sie die Variable **storageName** aus. Fügen Sie die schließende Klammer hinzu. Im folgenden Beispiel ist der Abschnitt „outputs“ dargestellt:

   ```json
   "outputs": { 
       "groupLocation": {
         "type": "string",
         "value": "[resourceGroup().location]"
       },
       "storageUri": {
         "type": "string",
         "value": "[reference(variables('storageName'))]"
       }
   }
   ```

Die endgültige Vorlage sieht wie folgt aus:

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageSKU": {
      "type": "string",
      "allowedValues": [
        "Standard_LRS",
        "Standard_ZRS",
        "Standard_GRS",
        "Standard_RAGRS",
        "Premium_LRS"
      ],
      "defaultValue": "Standard_LRS",
      "metadata": {
        "description": "The type of replication to use for the storage account."
      }
    },   
    "storageNamePrefix": {
      "type": "string",
      "maxLength": 11,
      "defaultValue": "storage",
      "metadata": {
        "description": "The value to use for starting the storage account name. Use only lowercase letters and numbers."
      }
    }
  },
  "variables": {
    "storageName": "[concat(toLower(parameters('storageNamePrefix')), uniqueString(resourceGroup().id))]"
  },
  "resources": [
    {
      "name": "[variables('storageName')]",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2016-01-01",
      "sku": {
        "name": "[parameters('storageSKU')]"
      },
      "kind": "Storage",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {
        "encryption":{
          "services":{
            "blob":{
              "enabled":true
            }
          },
          "keySource":"Microsoft.Storage"
        }
      }
    }
  ],
  "outputs": { 
    "groupLocation": {
      "type": "string",
      "value": "[resourceGroup().location]"
    },
    "storageUri": {
      "type": "string",
      "value": "[reference(variables('storageName'))]"
    }
  }
}
```

## <a name="deploy-template"></a>Bereitstellen der Vorlage

Nun können Sie die Vorlage bereitstellen. Verwenden Sie PowerShell oder die Azure-Befehlszeilenschnittstelle, um eine Ressourcengruppe zu erstellen. Stellen Sie anschließend ein Speicherkonto für diese Ressourcengruppe bereit.

* Führen Sie bei Verwendung von PowerShell die folgenden Befehle in dem Ordner mit der Vorlage aus:

   ```powershell
   Login-AzureRmAccount
   
   New-AzureRmResourceGroup -Name examplegroup -Location "South Central US"
   New-AzureRmResourceGroupDeployment -ResourceGroupName examplegroup -TemplateFile azuredeploy.json
   ```

* Führen Sie bei Verwendung einer lokalen Installation der Azure-Befehlszeilenschnittstelle die folgenden Befehle in dem Ordner mit der Vorlage aus:

   ```azurecli
   az login

   az group create --name examplegroup --location "South Central US"
   az group deployment create --resource-group examplegroup --template-file azuredeploy.json
   ```

Nachdem die Bereitstellung abgeschlossen ist, werden die Ausgabewerte zurückgegeben.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die bereitgestellten Ressourcen nicht mehr benötigen, löschen Sie die Ressourcengruppe, um die Ressourcen zu bereinigen.

Verwenden Sie für PowerShell Folgendes:

```powershell
Remove-AzureRmResourceGroup -Name examplegroup
```

Verwenden Sie für die Azure-Befehlszeilenschnittstelle den folgenden Befehl:

```azurecli
az group delete --name examplegroup
```

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zur Struktur einer Vorlage finden Sie unter [Erstellen von Azure Resource Manager-Vorlagen](resource-group-authoring-templates.md).
* Weitere Informationen zu den Eigenschaften für ein Speicherkonto finden Sie in der [Referenz zu Speicherkontenvorlagen](/azure/templates/microsoft.storage/storageaccounts).
* Komplette Vorlagen für viele verschiedene Lösungstypen finden Sie unter [Azure-Schnellstartvorlagen](https://azure.microsoft.com/documentation/templates/).

