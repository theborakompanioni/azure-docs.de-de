---
title: "Erstellen von Netzwerksicherheitsgruppen – Azure CLI 2.0 | Microsoft-Dokumentation"
description: Hier erfahren Sie, wie Sie Netzwerksicherheitsgruppen mit der Azure CLI 2.0 erstellen und bereitstellen.
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 9ea82c09-f4a6-4268-88bc-fc439db40c48
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/17/2017
ms.author: jdial
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 63f2f6dde56c1b5c4b3ad2591700f43f6542874d
ms.openlocfilehash: e7a8f4cff0889a02ef1f7a4d32fda4e57c5d35b0
ms.lasthandoff: 02/28/2017


---
# <a name="create-network-security-groups-using-the-azure-cli-20"></a>Erstellen von Netzwerksicherheitsgruppen mit der Azure CLI 2.0

[!INCLUDE [virtual-networks-create-nsg-selectors-arm-include](../../includes/virtual-networks-create-nsg-selectors-arm-include.md)]

## <a name="cli-versions-to-complete-the-task"></a>CLI-Versionen zum Durchführen dieser Aufgabe 

Führen Sie die Aufgabe mit einer der folgenden CLI-Versionen durch: 

- [Azure CLI 1.0:](virtual-networks-create-nsg-cli-nodejs.md) Unsere CLI für das klassische Bereitstellungsmodell und das Resource Manager-Bereitstellungsmodell 
- [Azure CLI 2.0](#Create-the-nsg-for-the-front-end-subnet): Unsere CLI der nächsten Generation für das Resource Manager-Bereitstellungsmodell (dieser Artikel)

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

Die folgenden Azure CLI 2.0-Beispielbefehle setzen voraus, dass bereits eine einfache Umgebung erstellt wurde, die auf dem obigen Szenario basiert. 

## <a name="create-the-nsg-for-the-frontend-subnet"></a>Erstellen der NSG für das Subnetz `FrontEnd`

Führen Sie zum Erstellen einer NSG mit dem Namen *NSG-FrontEnd* basierend auf dem oben beschriebenen Szenario die folgenden Schritte aus.

1. Falls noch nicht geschehen, installieren und konfigurieren Sie die neueste [Azure CLI 2.0](/cli/azure/install-az-cli2), und melden Sie sich mit [az login](/cli/azure/#login) bei einem Azure-Konto an. 

2. Erstellen Sie eine NSG mit dem Befehl [az network nsg create](/cli/azure/network/nsg#create). 

    ```azurecli
    az network nsg create \
    --resource-group testrg \
    --name NSG-FrontEnd \
    --location centralus 
    ```

    Parameter:
   
   * `--resource-group`: Name der Ressourcengruppe, in der die NSG erstellt wird. In diesem Szenario *TestRG*.
   * `--location`: Azure-Region, in der die neue NSG erstellt wird. In diesem Szenario *westus*.
   * `--name`: Name der neuen NSG. In diesem Szenario *NSG-FrontEnd*.

    Die erwartete Ausgabe enthält relativ viele Informationen, einschließlich einer Liste aller Standardregeln. Das folgende Beispiel zeigt die Standardregeln mithilfe eines JMESPATH-Abfragefilters mit dem Ausgabeformat `table`:

    ```azurecli
    az network nsg show \
    -g testrg \
    -n nsg-frontend \
    --query 'defaultSecurityRules[].{Access:access,Desc:description,DestPortRange:destinationPortRange,Direction:direction,Priority:priority}' \
    -o table
    ```
   
   Ausgabe:

        Access    Desc                                                    DestPortRange    Direction      Priority
        --------  ------------------------------------------------------  ---------------  -----------  ----------
        Allow     Allow inbound traffic from all VMs in VNET              *                Inbound           65000
        Allow     Allow inbound traffic from azure load balancer          *                Inbound           65001
        Deny      Deny all inbound traffic                                *                Inbound           65500
        Allow     Allow outbound traffic from all VMs to all VMs in VNET  *                Outbound          65000
        Allow     Allow outbound traffic from all VMs to Internet         *                Outbound          65001
        Deny      Deny all outbound traffic                               *                Outbound          65500



3. Erstellen Sie mit dem Befehl [az network nsg rule create](/cli/azure/network/nsg/rule#create) eine Regel, die den Zugriff auf Port 3389 (RDP) über das Internet zulässt.

    > [!NOTE]
    > Abhängig von der verwendeten Shell müssen Sie möglicherweise das Zeichen `*` in den folgenden Argumenten ändern, damit das Argument vor der Ausführung nicht erweitert wird.
   
    ```azurecli
    az network nsg rule create \
    --resource-group testrg \
    --nsg-name NSG-FrontEnd \
    --name rdp-rule \
    --access Allow \
    --protocol Tcp \
    --direction Inbound \
    --priority 100 \
    --source-address-prefix Internet \
    --source-port-range "*" \
    --destination-address-prefix "*" \
    --destination-port-range 3389
    ```
   
    Erwartete Ausgabe:
   
    ```json
    {
        "access": "Allow",
        "description": null,
        "destinationAddressPrefix": "*",
        "destinationPortRange": "3389",
        "direction": "Inbound",
        "etag": "W/\"<guid>\"",
        "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/rdp-rule",
        "name": "rdp-rule",
        "priority": 100,
        "protocol": "Tcp",
        "provisioningState": "Succeeded",
        "resourceGroup": "testrg",
        "sourceAddressPrefix": "Internet",
        "sourcePortRange": "*"
    }
    ```

    Parameter:

    * `--resource-group testrg`: Die zu verwendende Ressourcengruppe. Groß-/Kleinschreibung muss beachtet werden.
    * `--nsg-name NSG-FrontEnd`: Name der NSG, in der die Regel erstellt wird.
    * `--name rdp-rule`: Name der neuen Regel.
    * `--access Allow`: Zugriffsebene für die Regel (Deny oder Allow).
    * `--protocol Tcp`: Protokoll (TCP, UDP oder *).
    * `--direction Inbound`: Richtung der Verbindung (Inbound oder Outbound).
    * `--priority 100`: Priorität für die Regel.
    * `--source-address-prefix Internet`: Quelladresspräfix in CIDR oder Verwendung von Standardtags.
    * `--source-port-range "*"`: Quellport oder Portbereich. Port, der die Verbindung geöffnet hat.
    * `--destination-address-prefix "*"`: Zieladresspräfix in CIDR oder Verwendung von Standardtags.
    * `--destination-port-range 3389`: Zielport oder Portbereich. Port, der die Verbindungsanforderung empfängt.



4. Erstellen Sie mit dem Befehl **az network nsg rule create** eine Regel, die den Zugriff auf Port 80 (HTTP) über das Internet zulässt.
   
    ```azurecli
    az network nsg rule create \
    --resource-group testrg \
    --nsg-name NSG-FrontEnd \
    --name web-rule \
    --access Allow \
    --protocol Tcp \
    --direction Inbound \
    --priority 200 \
    --source-address-prefix Internet \
    --source-port-range "*" \
    --destination-address-prefix "*" \
    --destination-port-range 80
    ```
   
    Erwartete Ausgabe:
   
    ```json
    {
        "access": "Allow",
        "description": null,
        "destinationAddressPrefix": "*",
        "destinationPortRange": "80",
        "direction": "Inbound",
        "etag": "W/\"<guid>\"",
        "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/web-rule",
        "name": "web-rule",
        "priority": 200,
        "protocol": "Tcp",
        "provisioningState": "Succeeded",
        "resourceGroup": "testrg",
        "sourceAddressPrefix": "Internet",
        "sourcePortRange": "*"
    }
    ```

5. Binden Sie die NSG mit dem Befehl [az network vnet subnet update](/cli/azure/network/vnet/subnet#update) an das Subnetz **FrontEnd**.
        
    ```azurecli
    az network vnet subnet update \
    > --vnet-name TestVNET \
    > --name FrontEnd \
    > --resource-group testrg \
    > --network-security-group NSG-FrontEnd
    ```
   
    Erwartete Ausgabe:
   
    ```json
    {
        "addressPrefix": "192.168.1.0/24",
        "etag": "W/\"<guid>\"",
        "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/TestVNET/subnets/FrontEnd",
        "ipConfigurations": [
            {
            "etag": null,
            "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC/ipConfigurations/ipconfig1",
            "name": null,
            "privateIpAddress": null,
            "privateIpAllocationMethod": null,
            "provisioningState": null,
            "publicIpAddress": null,
            "resourceGroup": "TestRG",
            "subnet": null
            }
        ],
        "name": "FrontEnd",
        "networkSecurityGroup": {
            "defaultSecurityRules": null,
            "etag": null,
            "id": "/subscriptions/<guid>f/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd",
            "location": null,
            "name": null,
            "networkInterfaces": null,
            "provisioningState": null,
            "resourceGroup": "testrg",
            "resourceGuid": null,
            "securityRules": null,
            "subnets": null,
            "tags": null,
            "type": null
        },
        "provisioningState": "Succeeded",
        "resourceGroup": "testrg",
        "resourceNavigationLinks": null,
        "routeTable": null
    }
    ```

## <a name="create-the-nsg-for-the-backend-subnet"></a>Erstellen der NSG für das Subnetz `BackEnd`
Führen Sie zum Erstellen einer NSG mit dem Namen *NSG-BackEnd* basierend auf dem oben beschriebenen Szenario die folgenden Schritte aus.

1. Erstellen Sie mit **az network nsg create** die NSG `NSG-BackEnd`.
   
    ```azurecli
    az network nsg create \
    --resource-group testrg \
    --name NSG-BackEnd \
    --location centralus
    ```
   
    Wie in Schritt 2 oben ist die erwartete Ausgabe sehr umfangreich und enthält auch Standardregeln.
   
2. Erstellen Sie mit dem Befehl **az network nsg rule create** eine Regel, die den Zugriff auf Port 1433 (SQL) über das Subnetz `FrontEnd` zulässt.
   
    ```azurecli
    az network nsg rule create \
    --resource-group testrg \
    --nsg-name NSG-BackEnd \
    --name sql-rule \
    --access Allow \
    --protocol Tcp \
    --direction Inbound \
    --priority 100 \
    --source-address-prefix 192.168.1.0/24 \
    --source-port-range "*" \
    --destination-address-prefix "*" \
    --destination-port-range 1433
    ```
   
    Erwartete Ausgabe:

    ```json  
    {
    "access": "Allow",
    "description": null,
    "destinationAddressPrefix": "*",
    "destinationPortRange": "1433",
    "direction": "Inbound",
    "etag": "W/\"<guid>\"",
    "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd/securityRules/sql-rule",
    "name": "sql-rule",
    "priority": 100,
    "protocol": "Tcp",
    "provisioningState": "Succeeded",
    "resourceGroup": "testrg",
    "sourceAddressPrefix": "192.168.1.0/24",
    "sourcePortRange": "*"
    }
    ```

3. Erstellen Sie mit dem Befehl **az network nsg rule create** eine Regel, die den Zugriff auf das Internet verweigert.
   
    ```azurecli
    az network nsg rule create \
    --resource-group testrg \
    --nsg-name NSG-BackEnd \
    --name web-rule \
    --access Deny \
    --protocol Tcp  \
    --direction Outbound  \
    --priority 200 \
    --source-address-prefix "*" \
    --source-port-range "*" \
    --destination-address-prefix "*" \
    --destination-port-range "*"
    ```
   
    Erwartete Ausgabe:
   
    ```json
    {
    "access": "Deny",
    "description": null,
    "destinationAddressPrefix": "*",
    "destinationPortRange": "*",
    "direction": "Outbound",
    "etag": "W/\"<guid>\"",
    "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd/securityRules/web-rule",
    "name": "web-rule",
    "priority": 200,
    "protocol": "Tcp",
    "provisioningState": "Succeeded",
    "resourceGroup": "testrg",
    "sourceAddressPrefix": "*",
    "sourcePortRange": "*"
    }
    ```

4. Binden Sie die NSG mit dem Befehl **az network vnet subnet set** an das Subnetz `BackEnd`.
   
    ```azurecli
    az network vnet subnet update \
    --vnet-name TestVNET \
    --name BackEnd \
    --resource-group testrg \
    --network-security-group NSG-BackEnd
    ```
   
    Erwartete Ausgabe:
   
    ```json
    {
    "addressPrefix": "192.168.2.0/24",
    "etag": "W/\"<guid>\"",
    "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/TestVNET/subnets/BackEnd",
    "ipConfigurations": null,
    "name": "BackEnd",
    "networkSecurityGroup": {
        "defaultSecurityRules": null,
        "etag": null,
        "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd",
        "location": null,
        "name": null,
        "networkInterfaces": null,
        "provisioningState": null,
        "resourceGroup": "testrg",
        "resourceGuid": null,
        "securityRules": null,
        "subnets": null,
        "tags": null,
        "type": null
    },
    "provisioningState": "Succeeded",
    "resourceGroup": "testrg",
    "resourceNavigationLinks": null,
    "routeTable": null
    }
    ```

