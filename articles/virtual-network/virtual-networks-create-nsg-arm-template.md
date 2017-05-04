---
title: "Erstellen von Netzwerksicherheitsgruppen – Azure Resource Manager-Vorlage | Microsoft-Dokumentation"
description: Erfahren Sie, wie Sie Netzwerksicherheitsgruppen mithilfe einer Azure Resource Manager-Vorlage erstellen und bereitstellen.
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: f3e7385d-717c-44ff-be20-f9aa450aa99b
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: 87246de343f5c810bf765b467ba0d131934f8526
ms.lasthandoff: 04/27/2017


---
# <a name="create-network-security-groups-using-an-azure-resource-manager-template"></a>Erstellen von Netzwerksicherheitsgruppen mithilfe einer Azure Resource Manager-Vorlage

[!INCLUDE [virtual-networks-create-nsg-selectors-arm-include](../../includes/virtual-networks-create-nsg-selectors-arm-include.md)]

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Dieser Artikel gilt für das Ressourcen-Manager-Bereitstellungsmodell. Sie haben auch die Möglichkeit, [NSGs im klassischen Bereitstellungsmodell zu erstellen](virtual-networks-create-nsg-classic-ps.md).

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

## <a name="nsg-resources-in-a-template-file"></a>NSG Ressourcen in einer Vorlagendatei
Sie können die [Beispielvorlage](https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/NSGs.json)anzeigen und herunterladen.

Im folgenden Abschnitt wird die Definition der Front-End-NSG auf Grundlage des Szenarios beschrieben.

```json
"apiVersion": "2015-06-15",
"type": "Microsoft.Network/networkSecurityGroups",
"name": "[parameters('frontEndNSGName')]",
"location": "[resourceGroup().location]",
"tags": {
  "displayName": "NSG - Front End"
},
"properties": {
  "securityRules": [
    {
      "name": "rdp-rule",
      "properties": {
        "description": "Allow RDP",
        "protocol": "Tcp",
        "sourcePortRange": "*",
        "destinationPortRange": "3389",
        "sourceAddressPrefix": "Internet",
        "destinationAddressPrefix": "*",
        "access": "Allow",
        "priority": 100,
        "direction": "Inbound"
      }
    },
    {
      "name": "web-rule",
      "properties": {
        "description": "Allow WEB",
        "protocol": "Tcp",
        "sourcePortRange": "*",
        "destinationPortRange": "80",
        "sourceAddressPrefix": "Internet",
        "destinationAddressPrefix": "*",
        "access": "Allow",
        "priority": 101,
        "direction": "Inbound"
      }
    }
  ]
}
```
Um die NSG dem Front-End-Subnetz zuzuordnen, müssen Sie die Subnetzdefinition in der Vorlage ändern und die Referenz-ID für die NSG verwenden.

```json
"subnets": [
  {
    "name": "[parameters('frontEndSubnetName')]",
    "properties": {
      "addressPrefix": "[parameters('frontEndSubnetPrefix')]",
      "networkSecurityGroup": {
      "id": "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('frontEndNSGName'))]"
      }
    }
  }, 
```

Beachten Sie, dass für die Back-End-NSG und das Back-End-Subnetz in der Vorlage die gleichen Schritte ausgeführt werden.

## <a name="deploy-the-arm-template-by-using-click-to-deploy"></a>Bereitstellen der ARM-Vorlage per Klick
In der verfügbaren Beispielvorlage im öffentlichen Repository wird eine Parameterdatei verwendet, die die Standardwerte zum Generieren des oben beschriebenen Szenarios enthält. Öffnen Sie zum Bereitstellen dieser Vorlage per Klick [diesen Link](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd-NSG), klicken Sie auf **In Azure bereitstellen**, ersetzen Sie ggf. die Standardwerte der Parameter, und führen Sie die Schritte im Portal aus.

## <a name="deploy-the-arm-template-by-using-powershell"></a>Bereitstellen der ARM-Vorlage mithilfe von PowerShell
Führen Sie zum Bereitstellen der mithilfe von PowerShell heruntergeladenen ARM-Vorlage die folgenden Schritte aus.

1. Falls Sie Azure PowerShell noch nicht verwendet haben, befolgen Sie die Anleitung unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/overview), um PowerShell zu installieren und zu konfigurieren.
2. Führen Sie das Cmdlet **`New-AzureRmResourceGroup`** aus, um mit der Vorlage eine Ressourcengruppe zu erstellen.

    ```powershell
    New-AzureRmResourceGroup -Name TestRG -Location uswest `
    -TemplateFile 'https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/azuredeploy.json' `
    -TemplateParameterFile 'https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/azuredeploy.parameters.json'
    ```

    Erwartete Ausgabe:

        ResourceGroupName : TestRG
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                            Actions  NotActions
                            =======  ==========
                            *                  
   
        Resources         :
                            Name                Type                                     Location
                            ==================  =======================================  ========
                            sqlAvSet            Microsoft.Compute/availabilitySets       westus  
                            webAvSet            Microsoft.Compute/availabilitySets       westus  
                            SQL1                Microsoft.Compute/virtualMachines        westus  
                            SQL2                Microsoft.Compute/virtualMachines        westus  
                            Web1                Microsoft.Compute/virtualMachines        westus  
                            Web2                Microsoft.Compute/virtualMachines        westus  
                            TestNICSQL1         Microsoft.Network/networkInterfaces      westus  
                            TestNICSQL2         Microsoft.Network/networkInterfaces      westus  
                            TestNICWeb1         Microsoft.Network/networkInterfaces      westus  
                            TestNICWeb2         Microsoft.Network/networkInterfaces      westus  
                            NSG-BackEnd         Microsoft.Network/networkSecurityGroups  westus  
                            NSG-FrontEnd        Microsoft.Network/networkSecurityGroups  westus  
                            TestPIPSQL1         Microsoft.Network/publicIPAddresses      westus  
                            TestPIPSQL2         Microsoft.Network/publicIPAddresses      westus  
                            TestPIPWeb1         Microsoft.Network/publicIPAddresses      westus  
                            TestPIPWeb2         Microsoft.Network/publicIPAddresses      westus  
                            TestVNet            Microsoft.Network/virtualNetworks        westus  
                            testvnetstorageprm  Microsoft.Storage/storageAccounts        westus  
                            testvnetstoragestd  Microsoft.Storage/storageAccounts        westus  
   
        ResourceId        : /subscriptions/[Subscription Id]/resourceGroups/TestRG

## <a name="deploy-the-arm-template-by-using-the-azure-cli"></a>Bereitstellen der ARM-Vorlage mithilfe der Azure-Befehlszeilenschnittstelle
Führen Sie zum Bereitstellen der ARM-Vorlage mithilfe der Azure-Befehlszeilenschnittstelle die unten beschriebenen Schritte aus.

1. Wenn Sie die Azure-Befehlszeilenschnittstelle noch nie verwendet haben, ziehen Sie [Installieren und Konfigurieren der Azure-Befehlszeilenschnittstelle](../cli-install-nodejs.md) zurate, und folgen Sie den Anweisungen bis zu dem Punkt, an dem Sie Ihr Azure-Konto und Ihr Abonnement auswählen.
2. Führen Sie den Befehl **`azure config mode`** aus, um in den Ressourcen-Manager-Modus zu wechseln, wie unten dargestellt.

    ```azurecli
    azure config mode arm
    ```

    Nachstehend finden Sie die erwartete Ausgabe des Befehls:

        info:    New mode is arm

3. Führen Sie das Cmdlet **`azure group deployment create`** aus, um das neue VNet mithilfe der oben heruntergeladenen und geänderten Vorlage und Parameterdateien bereitzustellen. In der nach der Ausgabe angezeigten Liste werden die verwendeten Parameter erläutert.

    ```azurecli
    azure group create -n TestRG -l westus -f 'https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/azuredeploy.json' -e 'https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/azuredeploy.parameters.json'
    ```

    Erwartete Ausgabe:
   
        info:    Executing command group create
        info:    Getting resource group TestRG
        info:    Creating resource group TestRG
        info:    Created resource group TestRG
        info:    Initializing template configurations and parameters
        info:    Creating a deployment
        info:    Created template deployment "azuredeploy"
        data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG
        data:    Name:                TestRG
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:    
        info:    group create command OK
   
   * **-n (oder --name)**. Der Name der zu erstellenden Ressourcengruppe.
   * **-l (oder --location)**. Azure-Region, in der die Ressourcengruppe erstellt wird.
   * **-f (oder --template-file)**. Pfad zu Ihrer ARM-Vorlagendatei.
   * **-e (oder --parameters-file)**. Pfad zu Ihrer ARM-Parameterdatei.


