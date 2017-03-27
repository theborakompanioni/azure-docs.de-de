---
title: "Erstellen eines virtuellen Computers mit mehreren Netzwerkkarten – Azure Resource Manager-Vorlage | Microsoft-Dokumentation"
description: Erstellen Sie mithilfe einer Azure Resource Manager-Vorlage einen virtuellen Computer mit mehreren Netzwerkkarten (NICs).
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 486f7dd5-cf2f-434c-85d1-b3e85c427def
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 0d8472cb3b0d891d2b184621d62830d1ccd5e2e7
ms.openlocfilehash: 9f9908466dbfd471bc881387c0968c924140c67b
ms.lasthandoff: 03/21/2017


---
# <a name="create-a-vm-with-multiple-nics-using-a-template"></a>Erstellen eines virtuellen Computers (VM) mit mehreren Netzwerkkarten (NICs) mithilfe einer Vorlage
[!INCLUDE [virtual-network-deploy-multinic-arm-selectors-include.md](../../includes/virtual-network-deploy-multinic-arm-selectors-include.md)]

[!INCLUDE [virtual-network-deploy-multinic-intro-include.md](../../includes/virtual-network-deploy-multinic-intro-include.md)]

> [!NOTE]
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Resource Manager-Bereitstellung und klassische Bereitstellung](../resource-manager-deployment-model.md).  Dieser Artikel befasst sich mit dem Resource Manager-Bereitstellungsmodell, das von Microsoft für die meisten neuen Bereitstellungen anstatt des [klassischen Bereitstellungsmodells](virtual-network-deploy-multinic-classic-ps.md) empfohlen wird.
> 

[!INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

In den folgenden Schritten verwenden Sie eine Ressourcengruppe mit dem Namen *IaaSStory* für die Webserver und eine Ressourcengruppe mit dem Namen *IaaSStory-BackEnd* für die DB-Server.

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie die DB-Server erstellen können, müssen Sie die Ressourcengruppe *IaaSStory* mit den erforderlichen Ressourcen für dieses Szenario erstellen. Führen Sie die folgenden Schritte aus, um diese Ressourcen zu erstellen:

1. Wechseln Sie zu [azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC)(in englischer Sprache).
2. Klicken Sie auf der Vorlagenseite rechts neben **Parent Resource group** (übergeordnete Ressourcengruppe) auf **Deploy to Azure** (In Azure bereitstellen).
3. Ändern Sie bei Bedarf die Parameterwerte, und führen Sie die folgenden Schritte im Azure-Vorschauportal aus, um die Ressourcengruppe bereitzustellen.

> [!IMPORTANT]
> Achten Sie darauf, eindeutige Speicherkontonamen zu verwenden. In Azure dürfen keine doppelten Speicherkontennamen verwendet werden.
> 

## <a name="understand-the-deployment-template"></a>Grundlegendes zur Bereitstellungsvorlage
Bevor Sie die in dieser Dokumentation enthaltene Vorlage bereitstellen, sollten Sie sich mit ihrer Funktion vertraut machen. Die folgenden Schritte bieten eine gute Übersicht zu dieser Vorlage:

1. Wechseln Sie zu [azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC)(in englischer Sprache).
2. Klicken Sie auf **azuredeploy.json** , um die Vorlagendatei zu öffnen.
3. Beachten Sie den unten aufgeführten Parameter *osType* . Dieser Parameter wird verwendet, um das VM-Image für den Datenbankserver sowie verschiedene Einstellungen im Zusammenhang mit dem Betriebssystem auszuwählen.

    ```json
    "osType": {
      "type": "string",
      "defaultValue": "Windows",
      "allowedValues": [
        "Windows",
        "Ubuntu"
      ],
      "metadata": {
      "description": "Type of OS to use for VMs: Windows or Ubuntu."
      }
    },
    ```

4. Scrollen Sie zur Liste der Variablen, und lesen Sie die Definition für die **dbVMSetting** -Variablen (unten aufgeführt). Diese Variable empfängt eins der in der Variablen **dbVMSettings** enthaltenen Arrayelemente. Wenn Sie mit der in der Softwareentwicklung gebräuchlichen Terminologie vertraut sind, können Sie die Variable **dbVMSettings** als Hashtabelle oder Wörterbuch anzeigen.

    ```json
    "dbVMSetting": "[variables('dbVMSettings')[parameters('osType')]]"
    ```

5. Angenommen, Sie möchten Windows-VMs, auf denen SQL ausgeführt wird, auf dem Back-End-Server bereitstellen. In diesem Fall würde für **osType** der Wert *Windows* verwendet, und die Variable **dbVMSetting** würde das unten aufgeführte Element enthalten, das dem ersten Wert in der Variablen **dbVMSettings** entspricht.

    ```json
    "Windows": {
      "vmSize": "Standard_DS3",
      "publisher": "MicrosoftSQLServer",
      "offer": "SQL2014SP1-WS2012R2",
      "sku": "Standard",
      "version": "latest",
      "vmName": "DB",
      "osdisk": "osdiskdb",
      "datadisk": "datadiskdb",
      "nicName": "NICDB",
      "ipAddress": "192.168.2.",
      "extensionDeployment": "",
      "avsetName": "ASDB",
      "remotePort": 3389,
      "dbPort": 1433
    },
    ```

6. Beachten Sie, dass **vmSize** den Wert *Standard_DS3* enthält. Die Verwendung mehrerer Netzwerkschnittstellenkarten ist nur bei bestimmten VM-Größen möglich. Weitere Informationen zu den VM-Größen, die mehrere NICs unterstützen, finden Sie in den Artikeln [Windows-VM-Größen](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) und [Linux-VM-Größen](../virtual-machines/virtual-machines-linux-sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

7. Scrollen Sie zu **Ressourcen** , und beachten Sie das erste Element. Dieses Element beschreibt ein Speicherkonto. Dieses Speicherkonto wird für die Datenträger der einzelnen Datenbank-VMs verwendet. In diesem Szenario verfügt jede Datenbank-VM über einen Betriebssystemdatenträger in regulärem Speicher sowie über zwei Datenträger, die sich in SSD-Storage (Premium) befinden.

    ```json
    {
      "apiVersion": "2015-05-01-preview",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[parameters('prmStorageName')]",
      "location": "[variables('location')]",
      "tags": {
        "displayName": "Storage Account - Premium"
      },
      "properties": {
      "accountType": "[parameters('prmStorageType')]"
      }
    },
    ```

8. Scrollen Sie zur nächsten Ressource (siehe unten). Diese Ressource steht für die NIC, die in den einzelnen Datenbank-VMs für den Datenbankzugriff verwendet wird. Beachten Sie die Verwendung der **copy** -Funktion in dieser Ressource. Mit dieser Vorlage können Sie basierend auf dem Parameter **dbCount** eine beliebige Anzahl von VMs bereitstellen. Daher müssen Sie dieselbe Anzahl von NICs für den Datenbankzugriff erstellen (eine NIC pro VM).

    ```json
    {
    "apiVersion": "2015-06-15",
    "type": "Microsoft.Network/networkInterfaces",
    "name": "[concat(variables('dbVMSetting').nicName,'-DA-', copyindex(1))]",
    "location": "[variables('location')]",
    "tags": {
      "displayName": "NetworkInterfaces - DB DA"
    },
    "copy": {
      "name": "dbniccount",
      "count": "[parameters('dbCount')]"
    },
    "properties": {
      "ipConfigurations": [
        {
          "name": "ipconfig1",
          "properties": {
            "privateIPAllocationMethod": "Static",
            "privateIPAddress": "[concat(variables('dbVMSetting').ipAddress,copyindex(4))]",
            "subnet": {
              "id": "[variables('backEndSubnetRef')]"
            }
          }
         }
       ] 
     }
    },
    ```

9. Scrollen Sie zur nächsten Ressource (siehe unten). Diese Ressource steht für die NIC, die in den einzelnen Datenbank-VMs für die Verwaltung verwendet wird. Auch hier wird pro Datenbank-VM eine dieser NICs benötigt. Beachten Sie das Element **networkSecurityGroup** , das eine Netzwerksicherheitsgruppe verknüpft, die nur dieser NIC den Zugriff auf RDP/SSH erlaubt.

    ```json
    {
      "apiVersion": "2015-06-15",
      "type": "Microsoft.Network/networkInterfaces",
      "name": "[concat(variables('dbVMSetting').nicName, '-RA-',copyindex(1))]",
      "location": "[variables('location')]",
      "tags": {
        "displayName": "NetworkInterfaces - DB RA"
    },
    "copy": {
      "name": "dbniccount",
      "count": "[parameters('dbCount')]"
    },
    "properties": {
      "ipConfigurations": [
        {
          "name": "ipconfig1",
          "properties": {
            "networkSecurityGroup": {
             "id": "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('remoteAccessNSGName'))]"
             },
             "privateIPAllocationMethod": "Static",
             "privateIPAddress": "[concat(variables('dbVMSetting').ipAddress,copyindex(54))]",
             "subnet": {
              "id": "[variables('backEndSubnetRef')]"
             }
           }
          }
        ]
      }
    },
```

10. Scrollen Sie zur nächsten Ressource (siehe unten). Diese Ressource steht für eine Verfügbarkeitsgruppe, die von allen Datenbank-VMs gemeinsam verwendet wird. Auf diese Weise wird sichergestellt, dass während einer Wartung stets eine VM innerhalb der Verfügbarkeitsgruppe ausgeführt wird.

    ```json
    {
      "apiVersion": "2015-06-15",
      "type": "Microsoft.Compute/availabilitySets",
      "name": "[variables('dbVMSetting').avsetName]",
      "location": "[variables('location')]",
      "tags": {
        "displayName": "AvailabilitySet - DB"
      }
    },
    ```

11. Scrollen Sie zur nächsten Ressource. Diese Ressource steht für die Datenbank-VMs (wie unten in den ersten Zeilen gezeigt). Beachten Sie die erneute Verwendung der **copy**-Funktion, mit der sichergestellt wird, dass basierend auf dem Parameter **dbCount** mehrere VMs erstellt werden. Beachten Sie außerdem die Sammlung **dependsOn** . Diese führt zwei NICs auf, die erstellt werden müssen, bevor die VM bereitgestellt wird, sowie die Verfügbarkeitsgruppe und das Speicherkonto.

    ```json
    "apiVersion": "2015-06-15",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[concat(variables('dbVMSetting').vmName,copyindex(1))]",
    "location": "[variables('location')]",
    "dependsOn": [
      "[concat('Microsoft.Network/networkInterfaces/', variables('dbVMSetting').nicName,'-DA-', copyindex(1))]",
      "[concat('Microsoft.Network/networkInterfaces/', variables('dbVMSetting').nicName,'-RA-', copyindex(1))]",
      "[concat('Microsoft.Compute/availabilitySets/', variables('dbVMSetting').avsetName)]",
      "[concat('Microsoft.Storage/storageAccounts/', parameters('prmStorageName'))]"
    ],
    "tags": {
      "displayName": "VMs - DB"
    },
    "copy": {
      "name": "dbvmcount",
      "count": "[parameters('dbCount')]"
    },
    ```

12. Scrollen Sie in der VM-Ressource zum Element **networkProfile** (siehe unten). Beachten Sie, dass für jede VM zwei NICs referenziert werden. Wenn Sie mehrere NICs für eine VM erstellen, müssen Sie die Eigenschaft **primary** für eine der NICs auf *TRUE* und für alle anderen auf *FALSE* festlegen.

    ```json
    "networkProfile": {
      "networkInterfaces": [
        {
          "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('dbVMSetting').nicName,'-DA-',copyindex(1)))]",
          "properties": { "primary": true }
        },
        {
          "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('dbVMSetting').nicName,'-RA-',copyindex(1)))]",
          "properties": { "primary": false }
        }
      ]
    }
    ```

## <a name="deploy-the-arm-template-by-using-click-to-deploy"></a>Bereitstellen der ARM-Vorlage per Klick

> [!IMPORTANT]
> Stellen Sie sicher, dass Sie die [Voraussetzungen](#Pre-requisites) erfüllt haben, bevor Sie die unten stehenden Anweisungen befolgen.
> 

In der verfügbaren Beispielvorlage im öffentlichen Repository wird eine Parameterdatei verwendet, die die Standardwerte zum Generieren des oben beschriebenen Szenarios enthält. Öffnen Sie zum Bereitstellen dieser Vorlage per Klick [diesen Link](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC) rechts neben **Backend resource group (Back-End-Ressourcengruppe) (siehe Dokumentation)**, klicken Sie auf **Deploy to Azure**, ersetzen Sie ggf. die Werte der Standardparameter, und folgen Sie den Anweisungen im Portal.

Die unten stehende Abbildung zeigt den Inhalt der neuen Ressourcengruppe nach der Bereitstellung.

![Back-End-Ressourcengruppe](./media/virtual-network-deploy-multinic-arm-template/Figure2.png)

## <a name="deploy-the-template-by-using-powershell"></a>Bereitstellen der Vorlage mithilfe von PowerShell
Stellen Sie die Vorlage bereit, die Sie mithilfe von PowerShell heruntergeladen haben. Installieren und konfigurieren Sie PowerShell dazu mithilfe der folgenden Schritte im Artikel [Install and configure PowerShell (Installieren und Konfigurieren von PowerShell)](/powershell/azureps-cmdlets-docs), und führen Sie anschließend die folgenden Schritte aus:

Führen Sie das Cmdlet **`New-AzureRmResourceGroup`** aus, um mit der Vorlage eine Ressourcengruppe zu erstellen.

```powershell
New-AzureRmResourceGroup -Name IaaSStory-Backend -Location uswest `
TemplateFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.json' `
-TemplateParameterFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.parameters.json'
```

Erwartete Ausgabe:

    ResourceGroupName : IaaSStory-Backend
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
    Permissions       :
                        Actions  NotActions
                        =======  ==========
                        *
        Resources         :
                        Name                 Type                                 Location
                        ===================  ===================================  ========
                        ASDB                 Microsoft.Compute/availabilitySets   westus  
                        DB1                  Microsoft.Compute/virtualMachines    westus  
                        DB2                  Microsoft.Compute/virtualMachines    westus  
                        NICDB-DA-1           Microsoft.Network/networkInterfaces  westus  
                        NICDB-DA-2           Microsoft.Network/networkInterfaces  westus  
                        NICDB-RA-1           Microsoft.Network/networkInterfaces  westus  
                        NICDB-RA-2           Microsoft.Network/networkInterfaces  westus  
                        wtestvnetstorageprm  Microsoft.Storage/storageAccounts    westus  
    ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend

## <a name="deploy-the-template-by-using-the-azure-cli"></a>Bereitstellen der Vorlage mithilfe der Azure-Befehlszeilenschnittstelle
Führen Sie zum Bereitstellen der Vorlage mithilfe der Azure-Befehlszeilenschnittstelle die unten beschriebenen Schritte aus.

1. Wenn Sie die Azure-Befehlszeilenschnittstelle noch nie verwendet haben, ziehen Sie [Installieren und Konfigurieren der Azure-Befehlszeilenschnittstelle](../cli-install-nodejs.md) zurate, und folgen Sie den Anweisungen bis zu dem Punkt, an dem Sie Ihr Azure-Konto und Ihr Abonnement auswählen.
2. Führen Sie den Befehl **`azure config mode`** aus, um in den Ressourcen-Manager-Modus zu wechseln, wie unten dargestellt.

    ```azurecli
    azure config mode arm
    ```

    Die erwartete Ausgabe lautet wie folgt:

        info:    New mode is arm

3. Öffnen Sie die [Parameterdatei](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.parameters.json), und speichern Sie den Inhalt dieser Datei in einer Datei auf Ihrem Computer. In diesem Beispiel haben wir die Parameterdatei in *parameters.json*gespeichert.
4. Führen Sie das Cmdlet **`azure group deployment create`** aus, um das neue VNET mithilfe der oben heruntergeladenen und geänderten Vorlage und Parameterdateien bereitzustellen. In der nach der Ausgabe angezeigten Liste werden die verwendeten Parameter erläutert.

    ```azurecli
    azure group create -n IaaSStory-Backend -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.json -e parameters.json
    ```

    Erwartete Ausgabe:
   
        info:    Executing command group create
        + Getting resource group IaaSStory-Backend
        + Creating resource group IaaSStory-Backend
        info:    Created resource group IaaSStory-Backend
        + Initializing template configurations and parameters
        + Creating a deployment
        info:    Created template deployment "azuredeploy"
        data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend
        data:    Name:                IaaSStory-Backend
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK


