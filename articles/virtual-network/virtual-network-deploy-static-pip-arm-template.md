---
title: "Erstellen eines virtuellen Computers mit einer statischen öffentlichen IP-Adresse – Azure Resource Manager-Vorlage | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie mithilfe einer Azure Resource Manager-Vorlage einen virtuellen Computer mit einer statischen öffentlichen IP-Adresse erstellen."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: d551085a-c7ed-4ec6-b4c3-e9e1cebb774c
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/27/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 6d749e5182fbab04adc32521303095dab199d129
ms.openlocfilehash: 1bb49c0c03a3103194d068b511fa92ecb996caf3
ms.lasthandoff: 03/22/2017


---
# <a name="create-a-vm-with-a-static-public-ip-address-using-an-azure-resource-manager-template"></a>Erstellen eines virtuellen Computers mit einer statischen öffentlichen IP-Adresse mithilfe einer Azure Resource Manager-Vorlage

> [!div class="op_single_selector"]
> * [Azure-Portal](virtual-network-deploy-static-pip-arm-portal.md)
> * [PowerShell](virtual-network-deploy-static-pip-arm-ps.md)
> * [Azure-Befehlszeilenschnittstelle](virtual-network-deploy-static-pip-arm-cli.md)
> * [Vorlage](virtual-network-deploy-static-pip-arm-template.md)
> * [PowerShell (klassisch)](virtual-networks-reserved-public-ip.md)

[!INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

> [!NOTE]
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Resource Manager-Bereitstellung und klassische Bereitstellung](../resource-manager-deployment-model.md). Dieser Artikel befasst sich mit dem Resource Manager-Bereitstellungsmodell, das von Microsoft für die meisten neuen Bereitstellungen anstatt des klassischen Bereitstellungsmodells empfohlen wird.

[!INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name="public-ip-address-resources-in-a-template-file"></a>Öffentliche IP-Adressressourcen in einer Vorlagendatei
Sie können die [Beispielvorlage](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json)anzeigen und herunterladen.

Im folgenden Abschnitt wird die Definition der öffentlichen IP-Ressource auf Grundlage des obigen Szenarios beschrieben:

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/publicIPAddresses",
  "name": "[variables('webVMSetting').pipName]",
  "location": "[variables('location')]",
  "properties": {
    "publicIPAllocationMethod": "Static"
  },
  "tags": {
    "displayName": "PublicIPAddress - Web"
  }
},
```

Beachten Sie die **publicIPAllocationMethod** -Eigenschaft, die auf *Static*festgelegt ist. Diese Eigenschaft kann entweder *Dynamic* (Standardwert) oder *Static* lauten. Durch das Festlegen auf „Static“ wird sichergestellt, dass sich die zugewiesene öffentliche IP-Adresse nicht ändert.

Der folgende Abschnitt zeigt die Zuordnung der öffentlichen IP-Adresse zu einer Netzwerkschnittstelle:

```json
  {
    "apiVersion": "2015-06-15",
    "type": "Microsoft.Network/networkInterfaces",
    "name": "[variables('webVMSetting').nicName]",
    "location": "[variables('location')]",
    "tags": {
    "displayName": "NetworkInterface - Web"
    },
    "dependsOn": [
      "[concat('Microsoft.Network/publicIPAddresses/', variables('webVMSetting').pipName)]",
      "[concat('Microsoft.Network/virtualNetworks/', parameters('vnetName'))]"
    ],
    "properties": {
      "ipConfigurations": [
        {
          "name": "ipconfig1",
          "properties": {
          "privateIPAllocationMethod": "Static",
          "privateIPAddress": "[variables('webVMSetting').ipAddress]",
          "publicIPAddress": {
          "id": "[resourceId('Microsoft.Network/publicIPAddresses',variables('webVMSetting').pipName)]"
          },
          "subnet": {
            "id": "[variables('frontEndSubnetRef')]"
          }
        }
      }
    ]
  }
},
```

Beachten Sie, dass die **publicIPAddress**-Eigenschaft auf die **Id** einer Ressource mit dem Namen **variables('webVMSetting').pipName** zeigt. Dies ist der Name der oben angegebenen öffentlichen IP-Ressource.

Die obige Netzwerkschnittstelle wird jetzt unter der **networkProfile** -Eigenschaft der zu erstellenden VM aufgelistet.

```json
      "networkProfile": {
        "networkInterfaces": [
          {
            "id": "[resourceId('Microsoft.Network/networkInterfaces', variables('webVMSetting').nicName)]"
          }
        ]
      }
```

## <a name="deploy-the-template-by-using-click-to-deploy"></a>Bereitstellen der Vorlage per Klick

In der verfügbaren Beispielvorlage im öffentlichen Repository wird eine Parameterdatei verwendet, die die Standardwerte zum Generieren des oben beschriebenen Szenarios enthält. Klicken Sie zum Bereitstellen dieser Vorlage per Klick in der Datei „Readme.md“ für die Vorlage [VM with static PIP](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/03-Static-public-IP) (VM mit statischer PIP) auf **Deploy to Azure** (In Azure bereitstellen). Ersetzen Sie ggf. die Standardwerte für die Parameter, und geben Sie Werte für die leeren Parameter ein.  Folgen Sie den Anweisungen im Portal zum Erstellen eines virtuellen Computers mit einer statischen öffentlichen IP-Adresse.

## <a name="deploy-the-template-by-using-powershell"></a>Bereitstellen der Vorlage mithilfe von PowerShell

Führen Sie zum Bereitstellen der mithilfe von PowerShell heruntergeladenen Vorlage die unten beschriebenen Schritte aus.

1. Wenn Sie Azure PowerShell zum ersten Mal verwenden, folgen Sie den Anweisungen unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs).
2. Führen Sie in einer PowerShell-Konsole das Cmdlet `New-AzureRmResourceGroup` aus, um bei Bedarf eine neue Ressourcengruppe zu erstellen. Fahren Sie mit Schritt 3 fort, falls Sie bereits eine Ressourcengruppe erstellt haben.

    ```powershell
    New-AzureRmResourceGroup -Name PIPTEST -Location westus
    ```

    Erwartete Ausgabe:
   
        ResourceGroupName : PIPTEST
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/StaticPublicIP

3. Führen Sie auf einer PowerShell-Konsole das Cmdlet `New-AzureRmResourceGroupDeployment` aus, um die Vorlage bereitzustellen.

    ```powershell
    New-AzureRmResourceGroupDeployment -Name DeployVM -ResourceGroupName PIPTEST `
        -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json `
        -TemplateParameterUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.parameters.json
    ```

    Erwartete Ausgabe:
   
        DeploymentName    : DeployVM
        ResourceGroupName : PIPTEST
        ProvisioningState : Succeeded
        Timestamp         : [Date and time]
        Mode              : Incremental
        TemplateLink      :
                            Uri            : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/mas
                            ter/IaaS-Story/03-Static-public-IP/azuredeploy.json
                            ContentVersion : 1.0.0.0
   
        Parameters        :
                            Name                      Type                       Value     
                            ========================  =========================  ==========
                            vnetName                  String                     WTestVNet
                            vnetPrefix                String                     192.168.0.0/16
                            frontEndSubnetName        String                     FrontEnd  
                            frontEndSubnetPrefix      String                     192.168.1.0/24
                            storageAccountNamePrefix  String                     iaasestd  
                            stdStorageType            String                     Standard_LRS
                            osType                    String                     Windows   
                            adminUsername             String                     adminUser
                            adminPassword             SecureString                         
   
        Outputs           :

## <a name="deploy-the-template-by-using-the-azure-cli"></a>Bereitstellen der Vorlage mithilfe der Azure-Befehlszeilenschnittstelle
Führen Sie zum Bereitstellen der Vorlage mithilfe der Azure-Befehlszeilenschnittstelle die folgenden Schritte aus:

1. Falls Sie Azure CLI zum ersten Mal verwenden, befolgen Sie die Anleitung im Artikel [Installieren und Konfigurieren von Azure CLI](../cli-install-nodejs.md), um CLI zu installieren und zu konfigurieren.
2. Führen Sie den Befehl `azure config mode` aus, um in den Resource Manager-Modus zu wechseln, wie unten dargestellt.

    ```azurecli
    azure config mode arm
    ```

    Die erwartete Ausgabe des obigen Befehls:

        info:    New mode is arm

3. Öffnen Sie die [Parameterdatei](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.parameters.json), und speichern Sie den Inhalt dieser Datei in einer Datei auf Ihrem Computer. In diesem Beispiel werden die Parameter in einer Datei namens *parameters.json* gespeichert. Ändern Sie die Werte der Parameter in der Datei bei Bedarf. Es empfiehlt sich jedoch, dass Sie zumindest den Wert für den Parameter „adminPassword“ in ein eindeutiges, komplexes Kennwort ändern.
4. Führen Sie das Cmd `azure group deployment create` aus, um das neue VNET mithilfe der oben heruntergeladenen und geänderten Vorlage und Parameterdateien bereitzustellen. Ersetzen Sie im folgenden Befehl <path> durch den Pfad, an dem Sie die Datei gespeichert haben. 

    ```azurecli
    azure group create -n PIPTEST2 -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json -e <path>\parameters.json
    ```

    Erwartete Ausgabe (Liste der verwendeten Parameterwerte):

        info:    Executing command group create
        + Getting resource group PIPTEST2
        + Creating resource group PIPTEST2
        info:    Created resource group PIPTEST2
        + Initializing template configurations and parameters
        + Creating a deployment
        info:    Created template deployment "azuredeploy"
        data:    Id:                  /subscriptions/[Subscription ID]/resourceGroups/PIPTEST2
        data:    Name:                PIPTEST2
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK


