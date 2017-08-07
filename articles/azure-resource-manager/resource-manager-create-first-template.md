---
title: Erstellen Ihrer ersten Azure Resource Manager-Vorlage | Microsoft-Dokumentation
description: "Enthält eine Schritt-für-Schritt-Anleitung für die Erstellung Ihrer ersten Azure Resource Manager-Vorlage. Es wird beschrieben, wie Sie den Vorlagenverweis für ein Speicherkonto zum Erstellen der Vorlage verwenden."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 07/27/2017
ms.topic: get-started-article
ms.author: tomfitz
ms.translationtype: HT
ms.sourcegitcommit: 6e76ac40e9da2754de1d1aa50af3cd4e04c067fe
ms.openlocfilehash: 49086b51e2db1aebed45746306ae14b6f1feb631
ms.contentlocale: de-de
ms.lasthandoff: 07/31/2017

---

# <a name="create-and-deploy-your-first-azure-resource-manager-template"></a>Erstellen und Bereitstellen Ihrer ersten Azure Resource Manager-Vorlage
In diesem Thema werden die einzelnen Schritte zum Erstellen Ihrer ersten Azure Resource Manager-Vorlage beschrieben. Resource Manager-Vorlagen sind JSON-Dateien, mit denen die Ressourcen definiert werden, die Sie für Ihre Lösung bereitstellen müssen. Weitere Informationen zu den Konzepten der Bereitstellung und Verwaltung Ihrer Azure-Lösungen finden Sie unter [Übersicht über Azure Resource Manager](resource-group-overview.md). Wenn Sie über vorhandene Ressourcen verfügen und eine Vorlage für diese Ressourcen verwenden möchten, helfen Ihnen die Informationen unter [Exportieren einer Azure Resource Manager-Vorlage aus vorhandenen Ressourcen](resource-manager-export-template.md) weiter.

Zum Erstellen und Überarbeiten von Vorlagen benötigen Sie einen JSON-Editor. [Visual Studio Code](https://code.visualstudio.com/) ist ein einfacher Open-Source-Code-Editor für alle Plattformen. Es wird dringend empfohlen, Resource Manager-Vorlagen mithilfe von Visual Studio Code zu erstellen. In diesem Thema wird vorausgesetzt, dass Sie VS Code verwenden. Sie können aber auch einen anderen JSON-Editor (z.B. Visual Studio) nutzen.

## <a name="prerequisites"></a>Voraussetzungen

* Visual Studio Code. Kann bei Bedarf über [https://code.visualstudio.com/](https://code.visualstudio.com/) installiert werden.
* Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="create-template"></a>Erstellen der Vorlage

Wir beginnen mit einer einfachen Vorlage, die ein Speicherkonto für Ihr Abonnement bereitstellt.

1. Wählen Sie **Datei** > **Neue Datei** aus. 

   ![Neue Datei](./media/resource-manager-create-first-template/new-file.png)

2. Kopieren Sie die folgende JSON-Syntax, und fügen Sie sie in Ihre Datei ein:

   ```json
   {
     "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
     "contentVersion": "1.0.0.0",
     "parameters": {
     },
     "variables": {
     },
     "resources": [
       {
         "name": "[concat('storage', uniqueString(resourceGroup().id))]",
         "type": "Microsoft.Storage/storageAccounts",
         "apiVersion": "2016-01-01",
         "sku": {
           "name": "Standard_LRS"
         },
         "kind": "Storage",
         "location": "South Central US",
         "tags": {},
         "properties": {}
       }
     ],
     "outputs": {  }
   }
   ```

   Speicherkontonamen verfügen über mehrere Einschränkungen, die das Festlegen erschweren. Der Name muss zwischen drei und 24 Zeichen lang sein, darf nur Zahlen und Kleinbuchstaben enthalten und muss eindeutig sein. Die vorherige Vorlage verwendet die Funktion [uniqueString](resource-group-template-functions-string.md#uniquestring), um einen Hashwert zu generieren. Um den Hashwert aussagekräftiger zu machen, fügt sie das Präfix *storage* hinzu. 

3. Speichern Sie diese Datei als **azuredeploy.json** in einem lokalen Ordner.

   ![Vorlage speichern](./media/resource-manager-create-first-template/save-template.png)

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

Nach Abschluss der Bereitstellung befindet sich Ihr Speicherkonto in der Ressourcengruppe.

## <a name="deploy-template-from-cloud-shell"></a>Bereitstellen der Vorlage über Cloud Shell

Mit [Cloud Shell](../cloud-shell/overview.md) können Sie Befehle der Azure-Befehlszeilenschnittstelle ausführen, um Ihre Vorlage bereitzustellen. Die Vorlage muss allerdings zuerst in die Dateifreigabe für Ihre Cloud Shell-Instanz geladen werden. Für den Fall, dass Sie Cloud Shell noch nicht verwendet haben, finden Sie unter [Übersicht über Azure Cloud Shell (Vorschau)](../cloud-shell/overview.md) Informationen zum Einrichten von Cloud Shell.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.   

2. Wählen Sie Ihre Cloud Shell-Ressourcengruppe aus. Namensmuster: `cloud-shell-storage-<region>`.

   ![Auswählen der Ressourcengruppe](./media/resource-manager-create-first-template/select-cs-resource-group.png)

3. Wählen Sie das Speicherkonto für Ihre Cloud Shell-Instanz aus.

   ![Auswählen des Speicherkontos](./media/resource-manager-create-first-template/select-storage.png)

4. Wählen Sie **Dateien** aus.

   ![Auswählen von Dateien](./media/resource-manager-create-first-template/select-files.png)

5. Wählen Sie die Dateifreigabe für Cloud Shell aus. Namensmuster: `cs-<user>-<domain>-com-<uniqueGuid>`.

   ![Auswählen der Dateifreigabe](./media/resource-manager-create-first-template/select-file-share.png)

6. Wählen Sie **Verzeichnis hinzufügen** aus.

   ![Hinzufügen des Verzeichnisses](./media/resource-manager-create-first-template/select-add-directory.png)

7. Nennen Sie es **templates**, und wählen Sie **OK** aus.

   ![Benennen des Verzeichnisses](./media/resource-manager-create-first-template/name-templates.png)

8. Wählen Sie Ihr neues Verzeichnis aus.

   ![Auswählen des Verzeichnisses](./media/resource-manager-create-first-template/select-templates.png)

9. Wählen Sie die Option **Hochladen**.

   ![Auswählen von „Hochladen“](./media/resource-manager-create-first-template/select-upload.png)

10. Suchen Sie Ihre Vorlage, und laden Sie sie hoch.

   ![Hochladen der Datei](./media/resource-manager-create-first-template/upload-files.png)

11. Öffnen Sie die Eingabeaufforderung.

   ![Öffnen von Cloud Shell](./media/resource-manager-create-first-template/start-cloud-shell.png)

12. Geben Sie in der Cloud Shell-Instanz folgende Befehle ein:

   ```azurecli
   az group create --name examplegroup --location "South Central US"
   az group deployment create --resource-group examplegroup --template-file clouddrive/templates/azuredeploy.json
   ```

Nach Abschluss der Bereitstellung befindet sich Ihr Speicherkonto in der Ressourcengruppe.

## <a name="customize-the-template"></a>Anpassen der Vorlage

Die Vorlage funktioniert zwar, ist jedoch nicht flexibel. Sie stellt immer einen lokal redundanten Speicher für „USA, Süden-Mitte“ bereit. Der Name ist immer eine Kombination aus *storage* und einem Hashwert. Der Vorlage können jedoch Parameter hinzugefügt werden, um sie in verschiedenen Szenarien verwenden zu können.

Das folgende Beispiel zeigt den Parameterabschnitt mit zwei Parametern. Der erste Parameter (`storageSKU`) ermöglicht die Angabe des Redundanztyps. Er begrenzt die übergebbaren Werte auf gültige Werte für ein Speicherkonto. Außerdem gibt er einen Standardwert an. Der zweite Parameter (`storageNamePrefix`) wird festgelegt, um maximal 11 Zeichen zuzulassen. Er gibt einen Standardwert an.

```json
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
```

Fügen Sie im Variablenabschnitt eine Variable namens `storageName` hinzu. Dadurch wird der Präfixwert aus den Parametern mit einem Hashwert aus der Funktion [uniqueString](resource-group-template-functions-string.md#uniquestring) kombiniert. Mit der Funktion [toLower](resource-group-template-functions-string.md#tolower) werden alle Zeichen in Kleinbuchstaben umgewandelt.

```json
"variables": {
  "storageName": "[concat(toLower(parameters('storageNamePrefix')), uniqueString(resourceGroup().id))]"
},
```

Um die neuen Werte für Ihr Speicherkonto verwenden zu können, muss die Ressourcendefinition geändert werden:

```json
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
    "properties": {}
  }
],
```

Wie Sie sehen, ist der Name des Speicherkontos nun auf die hinzugefügte Variable festgelegt. Der SKU-Name ist auf den Wert des Parameters festgelegt. Der Standort ist auf den gleichen Standort festgelegt wie die Ressourcengruppe.

Speichern Sie die Datei. 

Nach Abschluss der Schritte in diesem Artikel sieht Ihre Vorlage nun wie folgt aus:

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
      "properties": {}
    }
  ],
  "outputs": {  }
}
```

## <a name="redeploy-template"></a>Erneutes Bereitstellen der Vorlage

Stellen Sie die Vorlage mit anderen Werten erneut bereit.

Verwenden Sie für PowerShell Folgendes:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName examplegroup -TemplateFile azuredeploy.json -storageNamePrefix newstore -storageSKU Standard_RAGRS
```

Verwenden Sie für die Azure-Befehlszeilenschnittstelle den folgenden Befehl:

```azurecli
az group deployment create --resource-group examplegroup --template-file azuredeploy.json --parameters storageSKU=Standard_RAGRS storageNamePrefix=newstore
```

Laden Sie für Cloud Shell die geänderte Vorlage in die Dateifreigabe hoch. Überschreiben Sie die vorhandene Datei. Verwenden Sie dann den folgenden Befehl:

```azurecli
az group deployment create --resource-group examplegroup --template-file clouddrive/templates/azuredeploy.json --parameters storageSKU=Standard_RAGRS storageNamePrefix=newstore
```

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

