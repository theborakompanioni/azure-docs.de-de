---
title: Erstellen einer Windows-VM mithilfe einer Vorlage in Azure | Microsoft Docs
description: Verwenden Sie eine Resource Manager-Vorlage und PowerShell, um ganz einfach einen neuen virtuellen Windows-Computer zu erstellen.
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 19129d61-8c04-4aa9-a01f-361a09466805
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: davidmu
ms.custom: H1Hack27Feb2017
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: ddab80262fe27c1f5995858ec7de75d7c46df081
ms.contentlocale: de-de
ms.lasthandoff: 08/21/2017

---

# <a name="create-a-windows-virtual-machine-from-a-resource-manager-template"></a>Erstellen eines virtuellen Windows-Computer mit einer Resource Manager-Vorlage

In diesem Artikel wird gezeigt, wie Sie eine Azure Resource Manager-Vorlage mithilfe von PowerShell bereitstellen. Mit der Vorlage, die Sie erstellen, wird ein einzelner virtueller Computer mit Windows Server in einem neuen virtuellen Netzwerk mit einem einzelnen Subnetz bereitgestellt.

Eine ausführliche Beschreibung der Ressource des virtuellen Computers finden Sie unter [Virtuelle Computer in einer Azure Resource Manager-Vorlage](template-description.md). Weitere Informationen zu allen Ressourcen in einer Vorlage finden Sie unter [Exemplarische Vorgehensweise zu Azure Resource Manager-Vorlagen](../../azure-resource-manager/resource-manager-template-walkthrough.md).

Das Ausführen der Schritte in diesem Artikel dauert ungefähr fünf Minuten.

## <a name="install-azure-powershell"></a>Installieren von Azure Powershell

Unter [Installieren und Konfigurieren von Azure PowerShell](../../powershell-install-configure.md) erfahren Sie, wie Sie die neueste Version von Azure PowerShell installieren, Ihr Abonnement auswählen und sich bei Ihrem Konto anmelden.

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Alle Ressourcen müssen in einer [Ressourcengruppe](../../azure-resource-manager/resource-group-overview.md) bereitgestellt werden.

1. Rufen Sie eine Liste mit den verfügbaren Standorten ab, an denen Ressourcen erstellt werden können.
   
    ```powershell   
    Get-AzureRmLocation | sort DisplayName | Select DisplayName
    ```

2. Erstellen Sie die Ressourcengruppe am ausgewählten Standort. In diesem Beispiel wird eine Ressourcengruppe mit dem Namen **myResourceGroup** am Standort **USA, Westen** erstellt:

    ```powershell   
    New-AzureRmResourceGroup -Name "myResourceGroup" -Location "West US"
    ```

## <a name="create-the-files"></a>Erstellen der Dateien

In diesem Schritt erstellen Sie eine Vorlagendatei, die Ressourcen und eine Parameterdatei bereitstellt, mit deren Hilfe Parameter in die Vorlage eingegeben werden. Sie erstellen außerdem eine Autorisierungsdatei zum Ausführen von Azure Resource Manager-Vorgängen.

1. Erstellen Sie eine Datei mit dem Namen *CreateVMTemplate.json*, und fügen Sie ihr diesen JSON-Code hinzu:

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "adminUsername": { "type": "string" },
        "adminPassword": { "type": "securestring" }
      },
      "variables": {
        "vnetID": "[resourceId('Microsoft.Network/virtualNetworks','myVNet')]", 
        "subnetRef": "[concat(variables('vnetID'),'/subnets/mySubnet')]", 
      },
      "resources": [
        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/publicIPAddresses",
          "name": "myPublicIPAddress",
          "location": "[resourceGroup().location]",
          "properties": {
            "publicIPAllocationMethod": "Dynamic",
            "dnsSettings": {
              "domainNameLabel": "myresourcegroupdns1"
            }
          }
        },
        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/virtualNetworks",
          "name": "myVNet",
          "location": "[resourceGroup().location]",
          "properties": {
            "addressSpace": { "addressPrefixes": [ "10.0.0.0/16" ] },
            "subnets": [
              {
                "name": "mySubnet",
                "properties": { "addressPrefix": "10.0.0.0/24" }
              }
            ]
          }
        },
        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/networkInterfaces",
          "name": "myNic",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[resourceId('Microsoft.Network/publicIPAddresses/', 'myPublicIPAddress')]",
            "[resourceId('Microsoft.Network/virtualNetworks/', 'myVNet')]"
          ],
          "properties": {
            "ipConfigurations": [
              {
                "name": "ipconfig1",
                "properties": {
                  "privateIPAllocationMethod": "Dynamic",
                  "publicIPAddress": { "id": "[resourceId('Microsoft.Network/publicIPAddresses','myPublicIPAddress')]" },
                  "subnet": { "id": "[variables('subnetRef')]" }
                }
              }
            ]
          }
        },
        {
          "apiVersion": "2016-04-30-preview",
          "type": "Microsoft.Compute/virtualMachines",
          "name": "myVM",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[resourceId('Microsoft.Network/networkInterfaces/', 'myNic')]"
          ],
          "properties": {
            "hardwareProfile": { "vmSize": "Standard_DS1" },
            "osProfile": {
              "computerName": "myVM",
              "adminUsername": "[parameters('adminUsername')]",
              "adminPassword": "[parameters('adminPassword')]"
            },
            "storageProfile": {
              "imageReference": {
                "publisher": "MicrosoftWindowsServer",
                "offer": "WindowsServer",
                "sku": "2012-R2-Datacenter",
                "version": "latest"
              },
              "osDisk": {
                "name": "myManagedOSDisk",
                "caching": "ReadWrite",
                "createOption": "FromImage"
              }
            },
            "networkProfile": {
              "networkInterfaces": [
                {
                  "id": "[resourceId('Microsoft.Network/networkInterfaces','myNic')]"
                }
              ]
            }
          }
        }
      ]
    }
    ```

2. Erstellen Sie eine Datei mit dem Namen *Parameters.json*, und fügen Sie ihr diesen JSON-Code hinzu:

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
      "contentVersion": "1.0.0.0",
      "parameters": {
      "adminUserName": { "value": "azureuser" },
        "adminPassword": { "value": "Azure12345678" }
      }
    }
    ```

3. Erstellen Sie ein neues Speicherkonto und einen Container:

    ```powershell
    $storageName = "st" + (Get-Random)
    New-AzureRmStorageAccount -ResourceGroupName "myResourceGroup" -AccountName $storageName -Location "West US" -SkuName "Standard_LRS" -Kind Storage
    $accountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName myResourceGroup -Name $storageName).Value[0]
    $context = New-AzureStorageContext -StorageAccountName $storageName -StorageAccountKey $accountKey 
    New-AzureStorageContainer -Name "templates" -Context $context -Permission Container
    ```

4. Laden Sie die Dateien in das Speicherkonto hoch:

    ```powershell
    Set-AzureStorageBlobContent -File "C:\templates\CreateVMTemplate.json" -Context $context -Container "templates"
    Set-AzureStorageBlobContent -File "C:\templates\Parameters.json" -Context $context -Container templates
    ```

    Ändern Sie die „-File“-Pfade in den Speicherort, in dem Sie die Dateien gespeichert haben.

## <a name="create-the-resources"></a>Erstellen der Ressourcen

Stellen Sie die Vorlage mit den Parametern bereit:

```powershell
$templatePath = "https://" + $storageName + ".blob.core.windows.net/templates/CreateVMTemplate.json"
$parametersPath = "https://" + $storageName + ".blob.core.windows.net/templates/Parameters.json"
New-AzureRmResourceGroupDeployment -ResourceGroupName "myResourceGroup" -Name "myDeployment" -TemplateUri $templatePath -TemplateParameterUri $parametersPath 
```

> [!NOTE]
> Sie können auch Vorlagen und Parameter von lokalen Dateien bereitstellen. Weitere Informationen finden Sie unter [Verwenden von Azure PowerShell mit Azure Storage](../../storage/common/storage-powershell-guide-full.md).

## <a name="next-steps"></a>Nächste Schritte

- Falls bei der Bereitstellung Probleme aufgetreten sind, können Sie den Artikel [Beheben gängiger Azure-Bereitstellungsfehler mit Azure Resource Manager](../../resource-manager-common-deployment-errors.md) lesen.
- Erfahren Sie, wie Sie einen virtuellen Computer erstellen und verwalten, indem Sie [Erstellen und Verwalten von virtuellen Windows-Computern mit dem Azure PowerShell-Modul](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) durcharbeiten.


