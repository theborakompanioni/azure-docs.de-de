---
title: Erstellen eines virtuellen Netzwerks mithilfe von Azure CLI 2.0 | Microsoft Docs
description: Informationen zum Erstellen eines virtuellen Netzwerks mithilfe von Azure CLI 2.0 | Resource Manager
services: virtual-network
documentationcenter: 
author: jimdial
manager: carmonm
editor: 
tags: azure-resource-manager
ms.assetid: 75966bcc-0056-4667-8482-6f08ca38e77a
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
translationtype: Human Translation
ms.sourcegitcommit: 617ac4672b24d339c5d4c0b671de7fb19cd9af91
ms.openlocfilehash: 3cbb679048a0cc1121b221bda8fc1e3df0e307c3
ms.lasthandoff: 02/17/2017


---
# <a name="create-a-virtual-network-using-the-azure-cli"></a>Erstellen eines virtuellen Netzwerks über die Azure-Befehlszeilenschnittstelle

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnet-intro-include.md)]

Azure verfügt über zwei Bereitstellungsmodelle: Azure Resource Manager und klassisch. Microsoft empfiehlt, Ressourcen mit dem Resource Manager-Bereitstellungsmodell zu erstellen. Weitere Informationen zu den Unterschieden zwischen den beiden Modellen finden Sie im Artikel zum Thema [Understand Azure deployment models](../azure-resource-manager/resource-manager-deployment-model.md) (Grundlegendes zu Azure-Bereitstellungsmodellen).

## <a name="cli-versions-to-complete-the-task"></a>CLI-Versionen zum Durchführen dieser Aufgabe
Führen Sie die Aufgabe mit einer der folgenden CLI-Versionen durch:

- [Azure CLI 1.0:](virtual-networks-create-vnet-arm-cli-nodejs.md) Unsere CLI für das klassische Bereitstellungsmodell und das Resource Manager-Bereitstellungsmodell
- [Azure CLI 2.0 (Vorschau):](#create-a-virtual-network) Unsere Befehlszeilenschnittstelle der nächsten Generation für das Resource Manager-Bereitstellungsmodell (dieser Artikel)
 
    Sie können ein VNet per Resource Manager auch mit anderen Tools erstellen oder dafür das klassische Bereitstellungsmodell verwenden, indem Sie in der folgenden Liste eine andere Option wählen:

> [!div class="op_single_selector"]
- [Portal](virtual-networks-create-vnet-arm-pportal.md)
- [PowerShell](virtual-networks-create-vnet-arm-ps.md)
- [BEFEHLSZEILENSCHNITTSTELLE (CLI)](virtual-networks-create-vnet-arm-cli.md)
- [Vorlage](virtual-networks-create-vnet-arm-template-click.md)
- [Portal (klassisch)](virtual-networks-create-vnet-classic-pportal.md)
- [PowerShell (klassisch)](virtual-networks-create-vnet-classic-netcfg-ps.md)
- [CLI (klassisch)](virtual-networks-create-vnet-classic-cli.md)

[!INCLUDE [virtual-networks-create-vnet-scenario-include](../../includes/virtual-networks-create-vnet-scenario-include.md)]


## <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks

Führen Sie die folgenden Schritte aus, um mithilfe von Azure CLI 2.0 ein virtuelles Netzwerk zu erstellen:

1. Installieren und konfigurieren Sie die neueste [Azure CLI 2.0 (Vorschau)](/cli/azure/install-az-cli2), und melden Sie sich mit [az login](/cli/azure/#login) bei einem Azure-Konto an.

2. Erstellen Sie eine Ressourcengruppe für Ihr VNET unter Verwendung des Befehls [az group create](/cli/azure/group#create) mit den Argumenten `--name` und `--location`:

    ```azurecli
    az group create --name myVNet --location centralus
    ```

3. Erstellen Sie ein VNET und ein Subnetz:

    ```azurecli
    az network vnet create \
        --name TestVNet \
        --resource-group myVNet \
        --location centralus \
        --address-prefix 192.168.0.0/16 \
        --subnet-name FrontEnd \
        --subnet-prefix 192.168.1.0/24
    ```

    Erwartete Ausgabe:
   
    ```json
    {
        "newVNet": {
            "addressSpace": {
            "addressPrefixes": [
                "192.168.0.0/16"
            ]
            },
            "dhcpOptions": {
            "dnsServers": []
            },
            "provisioningState": "Succeeded",
            "resourceGuid": "<guid>",
            "subnets": [
            {
                "etag": "W/\"<guid>\"",
                "id": "/subscriptions/<guid>/resourceGroups/myVNet/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                "name": "FrontEnd",
                "properties": {
                "addressPrefix": "192.168.1.0/24",
                "provisioningState": "Succeeded"
                },
                "resourceGroup": "myVNet"
            }
            ]
        }
    }
    ```

    Verwendete Parameter:

    - `--name TestVNet`: Name des zu erstellenden VNET
    - `--resource-group myVNet`: Name der Ressourcengruppe, die die Ressource steuert 
    - `--location centralus`: Speicherort für die Bereitstellung
    - `--address-prefix 192.168.0.0/16`: Adresspräfix und Adressblock  
    - `--subnet-name FrontEnd`: Name des Subnetzes
    - `--subnet-prefix 192.168.1.0/24`: Adresspräfix und Adressblock

    Um die grundlegenden Informationen zur Verwendung im nächsten Befehl aufzulisten, können Sie das VNET mit einem [Abfragefilter](/cli/azure/query-az-cli2) abfragen:

    ```azurecli
    az network vnet list --query '[?name==`TestVNet`].{Where:location,Name:name,Group:resourceGroup}' -o table
    ```

    Die folgende Ausgabe wird erzeugt:

        Where      Name      Group
        ---------  --------  -------
        centralus  TestVNet  myVNet

4. Erstellen Sie ein Subnetz:

    ```azurecli
    az network vnet subnet create \
        --address-prefix 192.168.2.0/24 \
        --name BackEnd \
        --resource-group myVNet \
        --vnet-name TestVNet
    ```
   
    Erwartete Ausgabe:

    ```json
    {
    "addressPrefix": "192.168.2.0/24",
    "etag": "W/\"<guid> \"",
    "id": "/subscriptions/<guid>/resourceGroups/myVNet/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/BackEnd",
    "ipConfigurations": null,
    "name": "BackEnd",
    "networkSecurityGroup": null,
    "provisioningState": "Succeeded",
    "resourceGroup": "myVNet",
    "resourceNavigationLinks": null,
    "routeTable": null
    }
    ```

    Verwendete Parameter:

    - `--address-prefix 192.168.2.0/24`: Subnetz-CIDR-Block
    - `--name BackEnd`: Name des neuen Subnetzes
    - `--resource-group myVNet`: Ressourcengruppe
    - `--vnet-name TestVNet`: Name des besitzenden VNET

5. Fragen Sie die Eigenschaften des neuen VNET ab:

    ```azurecli
    az network vnet show \
    -g myVNET \
    -n TestVNet \
    --query '{Name:name,Where:location,Group:resourceGroup,Status:provisioningState,SubnetCount:subnets | length(@)}' \
    -o table
    ```
   
    Erwartete Ausgabe:
   
        Name      Where      Group    Status       SubnetCount
        --------  ---------  -------  ---------  -------------
        TestVNet  centralus  myVNet   Succeeded              2

6. Fragen Sie die Eigenschaften des Subnetzes ab:

    ```azurecli
    az network vnet subnet list \
    -g myvnet \
    --vnet-name testvnet \
    --query '[].{Name:name,CIDR:addressPrefix,Status:provisioningState}' \
    -o table
    ```

    Erwartete Ausgabe:

        Name      CIDR            Status
        --------  --------------  ---------
        FrontEnd  192.168.1.0/24  Succeeded
        BackEnd   192.168.2.0/24  Succeeded

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich, wie folgende Verbindungen hergestellt werden:

- Eine Verbindung eines virtuellen Computers (VM) mit einem virtuellen Netzwerk. Lesen Sie hierzu den Artikel [Erstellen eines virtuellen Linux-Computers](../virtual-machines/virtual-machines-linux-quick-create-cli.md). Anstatt gemäß den Schritten in den Artikeln ein VNet und ein Subnetz zu erstellen, können Sie für die Verbindung mit der VM auch ein vorhandenes VNet und Subnetz auswählen.
- Eine Verbindung des virtuellen Netzwerks mit anderen virtuellen Netzwerken. Lesen Sie hierzu den Artikel [Konfigurieren einer VNet-zu-VNet-Verbindung über das Azure-Portal](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md).
- Eine Verbindung des virtuellen Netzwerks mit einem lokalen Netzwerk. Verwenden Sie hierfür ein Site-to-Site-VPN oder eine ExpressRoute-Verbindung. Informationen zur Vorgehensweise finden Sie unter [Herstellen einer Verbindung von einem VNET mit einem lokalen Netzwerk per Site-to-Site-VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) und [Verknüpfen eines VNET mit einer ExpressRoute-Verbindung](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md).
