---
title: Anpassen der exportierten Resource Manager-Vorlage | Microsoft Docs
description: Fügen Sie einer Azure Resource Manager-Vorlage Parameter hinzu, und stellen Sie sie über Azure PowerShell oder über die Azure-Befehlszeilenschnittstelle erneut bereit.
services: azure-resource-manager
documentationcenter: ''
author: tfitzmac
manager: timlt
editor: tysonn

ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/01/2016
ms.author: tomfitz

---
# Anpassen einer exportierten Azure Resource Manager-Vorlage
In diesem Thema erfahren Sie, wie Sie eine exportierte Vorlage ändern, um zusätzliche Werte als Parameter übergeben zu können. Das Thema baut zwar auf den Schritten des Artikels [Export Resource Manager template](resource-manager-export-template.md) (Exportieren der Resource Manager-Vorlage) auf, diese müssen jedoch nicht zwingend vorher ausgeführt werden. Die erforderliche Vorlage und die entsprechenden Skripts finden Sie in diesem Artikel.

## Anzeigen einer exportierten Vorlage
Falls Sie das Thema [Export Resource Manager template](resource-manager-export-template.md) (Exportieren der Resource Manager-Vorlage) bearbeitet haben, öffnen Sie die Vorlage, die Sie heruntergeladen haben. Der Name der Vorlage lautet **template.json**. Wenn Sie über Visual Studio oder Visual Code verfügen, können Sie die Vorlage mit einer dieser Lösungen bearbeiten. Andernfalls können Sie einen beliebigen JSON- oder Text-Editor verwenden.

Falls Sie die vorherige exemplarische Vorgehensweise nicht durchgeführt haben, erstellen Sie eine Datei namens **template.json**, und fügen Sie den folgenden Inhalt aus der exportierten Vorlage in die Datei ein.

```
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "virtualNetworks_VNET_name": {
            "defaultValue": "VNET",
            "type": "String"
        },
        "storageAccounts_storagetf05092016_name": {
            "defaultValue": "storagetf05092016",
            "type": "String"
        }
    },
    "variables": {},
    "resources": [
        {
            "comments": "Generalized from resource in walkthrough.",
            "type": "Microsoft.Network/virtualNetworks",
            "name": "[parameters('virtualNetworks_VNET_name')]",
            "apiVersion": "2015-06-15",
            "location": "northeurope",
            "properties": {
                "addressSpace": {
                    "addressPrefixes": [
                        "10.0.0.0/16"
                    ]
                },
                "subnets": [
                    {
                        "name": "default",
                        "properties": {
                            "addressPrefix": "10.0.0.0/24"
                        }
                    }
                ]
            },
            "dependsOn": []
        },
        {
            "comments": "Generalized from resource in walkthrough.",
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[parameters('storageAccounts_storagetf05092016_name')]",
            "apiVersion": "2015-06-15",
            "location": "northeurope",
            "tags": {},
            "properties": {
                "accountType": "Standard_RAGRS"
            },
            "dependsOn": []
        }
    ]
}
```

Die JSON-Vorlage eignet sich sehr gut, wenn Sie die gleiche Art von Speicherkonto in der gleichen Region mit einem virtuellen Netzwerk erstellen möchten, das für jede Bereitstellung das gleiche Adresspräfix und das gleiche Subnetzpräfix verwendet. Mit den in Resource Manager verfügbaren Optionen können Vorlagen jedoch erheblich flexibler bereitgestellt werden. So können Sie etwa bei der Bereitstellung die Art des zu erstellenden Speicherkontos oder die Werte angeben, die für Adresspräfix und Subnetzpräfix des virtuellen Netzwerks verwendet werden sollen.

## Anpassen der Vorlage
In diesem Abschnitt werden der exportierten Vorlage Parameter hinzugefügt, um die Vorlage wieder verwenden zu können, wenn Sie die Ressourcen in anderen Umgebungen bereitstellen. Außerdem werden der Vorlage einige Features hinzugefügt, die das Fehlerrisiko beim Bereitstellen der Vorlage verringern. Sie müssen sich keinen eindeutigen Namen für das Speicherkonto mehr ausdenken. Dieser wird stattdessen von der Vorlage erstellt. Darüber hinaus werden die Werte, die für den Speicherkontotyp angegeben werden können, auf gültige Optionen eingeschränkt.

1. Um die Übergabe der Werte zu ermöglichen, die Sie unter Umständen bei der Bereitstellung angeben möchten, muss der Abschnitt **parameters** durch die im Anschluss angegebenen Parameterdefinitionen ersetzt werden. Beachten Sie die Werte von **allowedValues** für **storageAccount\_accountType**. Falls Sie versehentlich einen ungültigen Wert angeben, wird dieser Fehler vor dem Start der Bereitstellung erkannt. Beachten Sie außerdem, dass Sie nur ein Präfix für den Namen des Speicherkontos angeben und das Präfix auf 11 Zeichen beschränkt ist. Durch die Beschränkung auf 11 Zeichen stellen Sie sicher, dass der vollständige Name die maximal zulässige Zeichenanzahl für ein Speicherkonto nicht überschreitet. Das Präfix ermöglicht die Verwendung einer Benennungskonvention für Speicherkonten. Die Vorgehensweise zum Erstellen eines eindeutigen Namens wird im nächsten Schritt erläutert.
   
        "parameters": {
          "storageAccount_prefix": {
            "type": "string",
            "maxLength": 11
          },
          "storageAccount_accountType": {
            "defaultValue": "Standard_RAGRS",
            "type": "string",
            "allowedValues": [
              "Standard_LRS",
              "Standard_ZRS",
              "Standard_GRS",
              "Standard_RAGRS",
              "Premium_LRS"
            ]
          },
          "virtualNetwork_name": {
            "type": "string"
          },
          "addressPrefix": {
            "defaultValue": "10.0.0.0/16",
            "type": "string"
          },
          "subnetName": {
            "defaultValue": "subnet-1",
            "type": "string"
          },
          "subnetAddressPrefix": {
            "defaultValue": "10.0.0.0/24",
            "type": "string"
          }
        },
2. Der Abschnitt **variables** der Vorlage ist momentan noch leer. Ersetzen Sie den Abschnitt durch den weiter unten angegebenen Code. Mithilfe des Abschnitts **variables** können Sie als Vorlagenersteller Werte erstellen, die die Syntax der restlichen Vorlage vereinfachen. Die Variable **storageAccount\_name** verkettet das Präfix aus dem Parameter zu einer eindeutigen Zeichenfolge, die auf der Grundlage des Bezeichners der Ressourcengruppe generiert wird.
   
        "variables": {
          "storageAccount_name": "[concat(parameters('storageAccount_prefix'), uniqueString(resourceGroup().id))]"
        },
3. Zur Verwendung der Parameter und Variablen in den Ressourcendefinitionen muss der Abschnitt **resources** durch die weiter unten angegebenen Definitionen ersetzt werden. Abgesehen von dem Wert, der der Ressourceneigenschaft zugewiesen wird, hat sich an den Ressourcendefinitionen eigentlich nicht viel geändert. Die Eigenschaften entsprechen exakt den Eigenschaften aus der exportierten Vorlage. Anstelle von hartcodierten Werten werden den Parameterwerten allerdings Eigenschaften zugewiesen. Mithilfe des Ausdrucks **resourceGroup().location** wird der Ort der Ressourcen auf den Ort der Ressourcengruppe festgelegt. Als Referenz für die Variable, die für den Namen des Speicherkontos erstellt wurde, wird der Ausdruck **variables** verwendet.
   
        "resources": [
          {
            "type": "Microsoft.Network/virtualNetworks",
            "name": "[parameters('virtualNetwork_name')]",
            "apiVersion": "2015-06-15",
            "location": "[resourceGroup().location]",
            "properties": {
              "addressSpace": {
                "addressPrefixes": [
                  "[parameters('addressPrefix')]"
                ]
              },
              "subnets": [
                {
                  "name": "[parameters('subnetName')]",
                  "properties": {
                    "addressPrefix": "[parameters('subnetAddressPrefix')]"
                  }
                }
              ]
            },
            "dependsOn": []
          },
          {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[variables('storageAccount_name')]",
            "apiVersion": "2015-06-15",
            "location": "[resourceGroup().location]",
            "tags": {},
            "properties": {
                "accountType": "[parameters('storageAccount_accountType')]"
            },
            "dependsOn": []
          }
        ]

## Anpassen der Parameterdatei
Die heruntergeladene Parameterdatei entspricht nicht mehr den Parametern in Ihrer Vorlage. Die Verwendung einer Parameterdatei ist zwar nicht zwingend notwendig, kann aber die erneute Bereitstellung einer Umgebung vereinfachen. Für die meisten Parameter werden die in der Vorlage definierten Standardwerte verwendet, sodass für die Parameterdatei lediglich zwei Werte benötigt werden.

Ersetzen Sie den Inhalt der JSON-Parameterdatei durch Folgendes:

```
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccount_prefix": {
      "value": "storage"
    },
    "virtualNetwork_name": {
      "value": "VNET"
    }
  }
}
```

Die aktualisierte Parameterdatei stellt nur Werte für Parameter bereit, die über keinen Standardwert verfügen. Sie können auch Werte für andere Parameter angeben, wenn Sie nicht den Standardwert verwenden möchten.

## Bereitstellen der Vorlage
Die angepasste Vorlage und die Parameterdateien können mit Azure PowerShell oder mithilfe der Azure-Befehlszeilenschnittstelle (Command-Line-Interface, CLI) bereitgestellt werden. Installieren Sie ggf. entweder [Azure PowerShell](powershell-install-configure.md) oder die [Azure-Befehlszeilenschnittstelle](xplat-cli-install.md). Sie können entweder die Skripts verwenden, die Sie beim Exportieren der ursprünglichen Vorlage zusammen mit Ihrer Vorlage heruntergeladen haben, oder ein eigenes Skript zum Bereitstellen der Vorlage schreiben. Beide Optionen werden in diesem Artikel gezeigt.

1. Wenn Sie die Bereitstellung mit einem eigenen Skript durchführen möchten, verwenden Sie eines der folgenden Verfahren:
   
     Führen Sie bei Verwendung von PowerShell Folgendes aus:
   
        # login
        Add-AzureRmAccount
   
        # create a new resource group
        New-AzureRmResourceGroup -Name ExportGroup -Location "West Europe"
   
        # deploy the template to the resource group
        New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExportGroup -TemplateFile {path-to-file}\template.json -TemplateParameterFile {path-to-file}\parameters.json
   
     Führen Sie bei Verwendung der Azure-Befehlszeilenschnittstelle Folgendes aus:
   
        azure login
   
        azure group create -n ExportGroup -l "West Europe"
   
        azure group deployment create -f {path-to-file}\azuredeploy.json -e {path-to-file}\parameters.json -g ExportGroup -n ExampleDeployment
2. Wenn Sie die exportierte Vorlage und die Skripts heruntergeladen haben, suchen Sie die Datei **deploy.ps1** (PowerShell) bzw. die Datei **deploy.sh** (Azure-Befehlszeilenschnittstelle).
   
     Führen Sie bei Verwendung von PowerShell Folgendes aus:
   
        Get-Item deploy.ps1 | Unblock-File
   
        .\deploy.ps1
   
     Führen Sie bei Verwendung der Azure-Befehlszeilenschnittstelle Folgendes aus:
   
        .\deploy.sh

## Nächste Schritte
* Das Thema [Resource Manager-Vorlage – Exemplarische Vorgehensweise](resource-manager-template-walkthrough.md) baut auf den Kenntnissen aus diesem Artikel auf und veranschaulicht die Erstellung einer Vorlage für eine komplexere Lösung. Hier erhalten Sie ausführlichere Informationen zu den verfügbaren Ressourcen sowie zu den anzugebenden Werten.
* Informationen zum Exportieren einer Vorlage mithilfe von PowerShell finden Sie unter [Verwenden von Azure PowerShell mit dem Azure-Ressourcen-Manager](powershell-azure-resource-manager.md).
* Informationen zum Exportieren einer Vorlage mithilfe der Azure-Befehlszeilenschnittstelle finden Sie unter [Verwenden der plattformübergreifenden Azure-Befehlszeilenschnittstelle mit dem Azure Resource Manager](xplat-cli-azure-resource-manager.md).
* Informationen zur Struktur von Vorlagen finden Sie unter [Erstellen von Azure-Ressourcen-Manager-Vorlagen](resource-group-authoring-templates.md).

<!---HONumber=AcomDC_0803_2016-->