---
title: Erstellen Ihrer ersten Azure Resource Manager-Vorlage | Microsoft-Dokumentation
description: "Enthält eine Schritt-für-Schritt-Anleitung für die Erstellung Ihrer ersten Azure Resource Manager-Vorlage. Es wird beschrieben, wie Sie den Vorlagenverweis für ein Speicherkonto zum Erstellen der Vorlage verwenden."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 04/18/2017
ms.topic: get-started-article
ms.author: tomfitz
ms.translationtype: Human Translation
ms.sourcegitcommit: 07584294e4ae592a026c0d5890686eaf0b99431f
ms.openlocfilehash: 80fd9d79652e4f0d9c4c524e3a762bcc3462bb53
ms.contentlocale: de-de
ms.lasthandoff: 06/01/2017

---

# <a name="create-your-first-azure-resource-manager-template"></a>Erstellen Ihrer ersten Azure Resource Manager-Vorlage
In diesem Thema werden die einzelnen Schritte zum Erstellen Ihrer ersten Azure Resource Manager-Vorlage beschrieben. Resource Manager-Vorlagen sind JSON-Dateien, mit denen die Ressourcen definiert werden, die Sie für Ihre Lösung bereitstellen müssen. Weitere Informationen zu den Konzepten der Bereitstellung und Verwaltung Ihrer Azure-Lösungen finden Sie unter [Übersicht über Azure Resource Manager](resource-group-overview.md). Wenn Sie über vorhandene Ressourcen verfügen und eine Vorlage für diese Ressourcen verwenden möchten, helfen Ihnen die Informationen unter [Exportieren einer Azure Resource Manager-Vorlage aus vorhandenen Ressourcen](resource-manager-export-template.md) weiter.

Zum Erstellen und Überarbeiten von Vorlagen benötigen Sie einen JSON-Editor. [Visual Studio Code](https://code.visualstudio.com/) ist ein einfacher Open-Source-Code-Editor für alle Plattformen. Er unterstützt die Erstellung und Bearbeitung von Resource Manager-Vorlagen mit einer Erweiterung. In diesem Thema wird vorausgesetzt, dass Sie VS Code verwenden. Sie können aber auch einen anderen JSON-Editor (z.B. Visual Studio) nutzen.

## <a name="get-vs-code-and-extension"></a>Verwenden von VS Code und der Erweiterung
1. Bei Bedarf können Sie VS Code über [https://code.visualstudio.com/](https://code.visualstudio.com/) installieren.

2. Installieren Sie die Erweiterung [Azure Resource Manager-Tools](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools), indem Sie das schnelle Öffnen (STRG+P) nutzen und Folgendes ausführen: 

   ```
   ext install msazurermtools.azurerm-vscode-tools
   ```

3. Starten Sie VS Code neu, wenn Sie dazu aufgefordert werden, um die Erweiterung zu aktivieren.

## <a name="create-blank-template"></a>Erstellen einer leeren Vorlage

Wir beginnen mit einer leeren Vorlage, die nur die grundlegenden Abschnitte einer Vorlage enthält.

1. Erstellen Sie eine Datei. 

2. Kopieren Sie die folgende JSON-Syntax, und fügen Sie sie in Ihre Datei ein:

   ```json
   {
     "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
     "contentVersion": "1.0.0.0",
     "parameters": {  },
     "variables": {  },
     "resources": [  ],
     "outputs": {  }
   }
   ```

3. Speichern Sie diese Datei als **azuredeploy.json**. 

## <a name="add-storage-account"></a>Hinzufügen von Speicherkonten
1. Zum Definieren eines Speicherkontos für die Bereitstellung fügen Sie das Speicherkonto dem Abschnitt **resources** Ihrer Vorlage hinzu. Die Werte, die für das Speicherkonto verfügbar sind, finden Sie in der [Referenz zu den Speicherkontovorlagen](/azure/templates/microsoft.storage/storageaccounts). Kopieren Sie den JSON-Code, der für das Speicherkonto angezeigt wird. 

3. Fügen Sie diesen JSON-Code in den Abschnitt **resources** Ihrer Vorlage ein. Dies ist im folgenden Beispiel dargestellt: 

   ```json
   {
     "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
     "contentVersion": "1.0.0.0",
     "parameters": {  },
     "variables": {  },
     "resources": [
       {
         "name": "string",
         "type": "Microsoft.Storage/storageAccounts",
         "apiVersion": "2016-12-01",
         "sku": {
           "name": "string"
         },
         "kind": "string",
         "location": "string",
         "tags": {},
         "properties": {
           "customDomain": {
             "name": "string",
             "useSubDomain": boolean
           },
           "encryption": {
             "services": {
               "blob": {
                 "enabled": boolean
               }
             },
             "keySource": "Microsoft.Storage"
           },
           "accessTier": "string"
         }
       }
     ],
     "outputs": {  }
   }
   ```

  VS-Code kann darauf hinweisen, dass 2016-12-01 keine gültige API-Version ist. Wenn Sie eine Versionsnummer aus der Referenzdokumentation der Vorlage verwenden, können Sie diese Warnung ignorieren. Diese Warnung wird angezeigt, wenn das Schema nicht mit der letzten Versionsnummer des Ressourcenanbieters aktualisiert wurde. 
  
  Das obige Beispiel enthält viele Platzhalterwerte und einige Eigenschaften, die Sie in Ihrem Speicherkonto unter Umständen nicht benötigen.

## <a name="set-values-for-storage-account"></a>Festlegen von Werten für das Speicherkonto

Sie können nun die Werte für Ihr Speicherkonto festlegen. 

1. Sehen Sie sich erneut die [Referenz zu den Speicherkontovorlagen](/azure/templates/microsoft.storage/storageaccounts) an, aus der Sie den JSON-Code kopiert haben. Es sind mehrere Tabellen vorhanden, in denen die Eigenschaften beschrieben und die verfügbaren Werte angegeben werden. 

2. Beachten Sie, dass im **properties**-Element die Elemente **customDomain**, **encryption** und **accessTier** als nicht erforderlich angegeben sind. Diese Werte können für Sie ggf. wichtig sein, aber hier entfernen wir sie, um das Beispiel einfach zu halten.

   ```json
   "resources": [
     {
       "name": "string",
       "type": "Microsoft.Storage/storageAccounts",
       "apiVersion": "2016-12-01",
       "sku": {
         "name": "string"
       },
       "kind": "string",
       "location": "string",
       "tags": {},
       "properties": {
       }
     }
   ],
   ```

3. Derzeit ist das **kind**-Element auf einen Platzhalterwert („string“) festgelegt. VS Code enthält viele Features, mit deren Hilfe Sie besser verstehen können, welche Werte in der Vorlage verwendet werden sollten. Beachten Sie, dass dieser Wert von VS Code als ungültig angegeben wird. Wenn Sie auf „string“ zeigen, schlägt VS Code `Storage` oder `BlobStorage` als gültige Werte für **kind** vor. 

   ![Anzeigen der von VS Code vorgeschlagenen Werte](./media/resource-manager-create-first-template/vs-code-show-values.png)

   Löschen Sie die Zeichen zwischen den doppelten Anführungszeichen, und wählen Sie **STRG+Leertaste**, um die verfügbaren Werte anzuzeigen. Wählen Sie unter den verfügbaren Optionen **Storage** aus.
  
   ![IntelliSense anzeigen](./media/resource-manager-create-first-template/intellisense.png)

   Falls Sie VS Code nicht verwenden, helfen Ihnen die Informationen auf der Seite mit der Referenz zu den Speicherkontovorlagen weiter. Beachten Sie, dass in der Beschreibung die gleichen gültigen Werte aufgeführt sind. Legen Sie das Element auf **Storage** fest.

   ```json
   "kind": "Storage",
   ```

Ihre Vorlage sieht jetzt wie folgt aus:

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {  },
  "variables": {  },
  "resources": [
    {
      "name": "string",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2016-12-01",
      "sku": {
        "name": "string"
      },
      "kind": "Storage",
      "location": "string",
      "tags": {},
      "properties": {
      }
    }
  ],
  "outputs": {  }
}
```

## <a name="add-template-function"></a>Hinzufügen einer Vorlagenfunktion

Sie verwenden in Ihrer Vorlage Funktionen, um die Syntax der Vorlage zu vereinfachen und Werte abzurufen, die nur verfügbar sind, wenn die Vorlage bereitgestellt wird. Unter [Vorlagenfunktionen in Azure Resource Manager](resource-group-template-functions.md) finden Sie eine vollständige Liste mit den Vorlagenfunktionen.

Legen Sie die **location**-Eigenschaft wie folgt fest, um anzugeben, dass das Speicherkonto an demselben Standort wie die Ressourcengruppe bereitgestellt wird.

```json
"location": "[resourceGroup().location]",
```

Hier erhalten Sie wieder Unterstützung von VS Code, indem verfügbare Funktionen vorgeschlagen werden. 

![Anzeigen von Funktionen](./media/resource-manager-create-first-template/show-functions.png)

Beachten Sie, dass die Funktion in eckige Klammern gesetzt ist. Mit der Funktion [resourceGroup](resource-group-template-functions-resource.md#resourcegroup) wird ein Objekt mit einer `location`-Eigenschaft zurückgegeben. Die Ressourcengruppe enthält alle zugehörigen Ressourcen für Ihre Lösung. Sie können für die location-Eigenschaft einen Wert wie „Central US“ (USA, Mitte) hartcodieren, aber Sie müssen die Vorlage dann manuell ändern, um die erneute Bereitstellung für einen anderen Standort durchzuführen. Die Verwendung der Funktion `resourceGroup` erleichtert Ihnen die erneute Bereitstellung dieser Vorlage in einer anderen Ressourcengruppe an einem anderen Standort.

Ihre Vorlage sieht jetzt wie folgt aus:

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {  },
  "variables": {  },
  "resources": [
    {
      "name": "string",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2016-12-01",
      "sku": {
        "name": "string"
      },
      "kind": "Storage",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {
      }
    }
  ],
  "outputs": {  }
}
```

## <a name="add-parameters-and-variables"></a>Hinzufügen von Parametern und Variablen
In der Vorlage müssen jetzt nur noch zwei Werte festgelegt werden: **name** und **sku.name**. Für diese Eigenschaften fügen Sie Parameter hinzu, mit denen Sie diese Werte bei der Bereitstellung anpassen können. 

Speicherkontonamen verfügen über mehrere Einschränkungen, die das Festlegen erschweren. Der Name muss zwischen drei und 24 Zeichen lang sein, darf nur Zahlen und Kleinbuchstaben enthalten und muss eindeutig sein. Verwenden Sie die Funktion [uniqueString](resource-group-template-functions-string.md#uniquestring), um einen Hashwert zu generieren, anstatt zu versuchen, einen eindeutigen Wert zu erraten, der die Einschränkungen erfüllt. Sie können diesen Hashwert aussagekräftiger machen, indem Sie ein Präfix hinzufügen, das nach der Bereitstellung zum Identifizieren als Speicherkonto dient. 

1. Navigieren Sie zum Abschnitt **parameters** Ihrer Vorlage, um ein Präfix für den Namen zu übergeben, der Ihren Benennungskonventionen entspricht. Fügen Sie der Vorlage einen Parameter hinzu, für den ein Präfix für den Speicherkontonamen verwendet werden kann:

   ```json
   "parameters": {
     "storageNamePrefix": {
       "type": "string",
       "maxLength": 11,
       "defaultValue": "storage",
       "metadata": {
         "description": "The value to use for starting the storage account name."
       }
     }
   },
   ```

  Das Präfix ist auf maximal elf Zeichen beschränkt, da von `uniqueString` 13 Zeichen zurückgegeben werden und der Name nicht länger als 24 Zeichen sein darf. Wenn Sie während der Bereitstellung keinen Wert für den Parameter übergeben, wird der Standardwert verwendet.

2. Navigieren Sie zum Abschnitt **variables** der Vorlage. Fügen Sie die folgende Variable hinzu, um den Namen aus dem Präfix und der eindeutigen Zeichenfolge zu erstellen:

   ```json
   "variables": {
     "storageName": "[concat(parameters('storageNamePrefix'), uniqueString(resourceGroup().id))]"
   },
   ```

3. Legen Sie den Speicherkontonamen im Abschnitt **resources** auf diese Variable fest.

   ```json
   "name": "[variables('storageName')]",
   ```

3. Navigieren Sie zum Abschnitt **parameters**, um das Übergeben von unterschiedlichen SKUs für das Speicherkonto zu ermöglichen. Fügen Sie nach dem Parameter für das Speichernamenpräfix einen Parameter hinzu, mit dem die zulässigen SKU-Werte und ein Standardwert angegeben werden. Sie finden die zulässigen Werte entweder auf der Seite mit der Vorlagenreferenz oder in VS Code. Im folgenden Beispiel binden Sie alle gültigen Werte für die SKU ein. Sie können die zulässigen Werte aber auch nur auf die Arten von SKUs beschränken, die Sie über diese Vorlage bereitstellen möchten.

   ```json
   "parameters": {
     "storageNamePrefix": {
       "type": "string",
       "maxLength": 11,
       "defaultValue": "storage",
       "metadata": {
         "description": "The value to use for starting the storage account name."
       }
     },
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
     }
   },
   ```

3. Ändern Sie die SKU-Eigenschaft so, dass der Wert aus dem Parameter verwendet wird:

   ```json
   "sku": {
     "name": "[parameters('storageSKU')]"
   },
   ```    

4. Speichern Sie die Datei.

## <a name="final-template"></a>Endgültige Vorlage

Nach Abschluss der Schritte in diesem Artikel sieht Ihre Vorlage nun wie folgt aus:

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageNamePrefix": {
      "type": "string",
      "maxLength": 11,
      "defaultValue": "storage",
      "metadata": {
        "description": "The value to use for starting the storage account name."
      }
    },
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
    }
  },
  "variables": {
    "storageName": "[concat(parameters('storageNamePrefix'), uniqueString(resourceGroup().id))]"
  },
  "resources": [
    {
      "name": "[variables('storageName')]",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2016-12-01",
      "sku": {
        "name": "[parameters('storageSKU')]"
      },
      "kind": "Storage",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {
      }
    }
  ],
  "outputs": {  }
}
```

## <a name="next-steps"></a>Nächste Schritte
* Die Vorlage ist fertig, und Sie können sie nun für Ihr Abonnement bereitstellen. Informationen zur Bereitstellung finden Sie unter [Bereitstellen von Ressourcen in Azure](resource-manager-quickstart-deploy.md).
* Weitere Informationen zur Struktur einer Vorlage finden Sie unter [Erstellen von Azure Resource Manager-Vorlagen](resource-group-authoring-templates.md).
* Komplette Vorlagen für viele verschiedene Lösungstypen finden Sie unter [Azure-Schnellstartvorlagen](https://azure.microsoft.com/documentation/templates/).

