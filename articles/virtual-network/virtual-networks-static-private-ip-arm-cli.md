---
title: "Konfigurieren von privaten IP-Adressen für virtuelle Computer – Azure-CLI 2.0 | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie private IP-Adressen für virtuelle Computer mithilfe der Azure-Befehlszeilenschnittstelle (Azure CLI 2.0) konfigurieren."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 40b03a1a-ea00-454c-b716-7574cea49ac0
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/16/2017
ms.author: jdial
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 63f2f6dde56c1b5c4b3ad2591700f43f6542874d
ms.openlocfilehash: ea54b413b92a4d4e312b741ce42090c77de0e6f5
ms.lasthandoff: 02/28/2017


---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-using-the-azure-cli-20"></a>Konfigurieren von privaten IP-Adressen für einen virtuellen Computer mithilfe der Azure CLI 2.0

[!INCLUDE [virtual-networks-static-private-ip-selectors-arm-include](../../includes/virtual-networks-static-private-ip-selectors-arm-include.md)]


## <a name="cli-versions-to-complete-the-task"></a>CLI-Versionen zum Durchführen dieser Aufgabe 

Führen Sie die Aufgabe mit einer der folgenden CLI-Versionen durch: 

- [Azure CLI 1.0:](virtual-networks-static-private-ip-cli-nodejs.md) Unsere CLI für das klassische Bereitstellungsmodell und das Resource Manager-Bereitstellungsmodell 
- [Azure CLI 2.0](#specify-a-static-private-ip-address-when-creating-a-vm): Unsere CLI der nächsten Generation für das Resource Manager-Bereitstellungsmodell (dieser Artikel)

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Dieser Artikel gilt für das Ressourcen-Manager-Bereitstellungsmodell. Sie können [eine statische private IP-Adresse auch im klassischen Bereitstellungsmodell verwalten](virtual-networks-static-private-ip-classic-cli.md).

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

> [!NOTE]
> Die folgenden Azure CLI 2.0-Beispielbefehle setzen voraus, dass bereits eine einfache Umgebung erstellt wurde. Wenn Sie die in diesem Dokument aufgeführten Befehle ohne Veränderungen ausführen möchten, erstellen Sie zunächst eine Testumgebung, wie unter [Erstellen eines VNets](virtual-networks-create-vnet-arm-cli.md)beschrieben.

## <a name="specify-a-static-private-ip-address-when-creating-a-vm"></a>Angeben einer statischen privaten IP-Adresse beim Erstellen eines virtuellen Computers

Erstellen Sie in einem VNet mit dem Namen *TestVNet* im Subnetz *FrontEnd* einen virtuellen Computer mit dem Namen *DNS01* und der statischen privaten IP-Adresse *192.168.1.101*. Gehen Sie dabei wie folgt vor:

1. Falls noch nicht geschehen, installieren und konfigurieren Sie die neueste [Azure CLI 2.0](/cli/azure/install-az-cli2), und melden Sie sich mit [az login](/cli/azure/#login) bei einem Azure-Konto an. 

2. Erstellen Sie mit dem Befehl [az network public-ip create](/cli/azure/network/public-ip#create) eine öffentliche IP-Adresse für den virtuellen Computer. In der nach der Ausgabe angezeigten Liste werden die verwendeten Parameter erläutert.

    > [!NOTE]
    > Möglicherweise benötigen Sie andere Werte für Ihre Argumente in diesem und den nachfolgenden Schritten. Dies ist von Ihrer Umgebung abhängig.
   
    ```azurecli
    az network public-ip create \
    --name TestPIP \
    --resource-group TestRG \
    --location centralus \
    --allocation-method Static
    ```

    Erwartete Ausgabe:
   
   ```json
   {
        "publicIp": {
            "idleTimeoutInMinutes": 4,
            "ipAddress": "52.176.43.167",
            "provisioningState": "Succeeded",
            "publicIPAllocationMethod": "Static",
            "resourceGuid": "79e8baa3-33ce-466a-846c-37af3c721ce1"
        }
    }
    ```

   * `--resource-group`: Name der Ressourcengruppe, in der die öffentliche IP-Adresse erstellt werden soll.
   * `--name`: Name der öffentlichen IP-Adresse.
   * `--location`: Azure-Region, in der die öffentliche IP-Adresse erstellt werden soll.

3. Führen Sie den Befehl [az network nic create](/cli/azure/network/nic#create) aus, um eine NIC mit statischer privater IP-Adresse zu erstellen. In der nach der Ausgabe angezeigten Liste werden die verwendeten Parameter erläutert. 
   
    ```azurecli
    az network nic create \
    --resource-group TestRG \
    --name TestNIC \
    --location centralus \
    --subnet FrontEnd \
    --private-ip-address 192.168.1.101 \
    --vnet-name TestVNet
    ```

    Erwartete Ausgabe:
   
    ```json
    {
        "newNIC": {
            "dnsSettings": {
            "appliedDnsServers": [],
            "dnsServers": []
            },
            "enableIPForwarding": false,
            "ipConfigurations": [
            {
                "etag": "W/\"<guid>\"",
                "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC/ipConfigurations/ipconfig1",
                "name": "ipconfig1",
                "properties": {
                "primary": true,
                "privateIPAddress": "192.168.1.101",
                "privateIPAllocationMethod": "Static",
                "provisioningState": "Succeeded",
                "subnet": {
                    "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                    "resourceGroup": "TestRG"
                }
                },
                "resourceGroup": "TestRG"
            }
            ],
            "provisioningState": "Succeeded",
            "resourceGuid": "<guid>"
        }
    }
        ```
    
    Parameters:

    * `--private-ip-address`: Static private IP address for the NIC.
    * `--vnet-name`: Name of the VNet in wihch to create the NIC.
    * `--subnet`: Name of the subnet in which to create the NIC.

4. Run the [azure vm create](/cli/azure/vm/nic#create) command to create the VM using the public IP and NIC created above. The list shown after the output explains the parameters used.
   
    ```azurecli
    az vm create \
    --resource-group TestRG \
    --name DNS01 \
    --location centralus \
    --image Debian \
    --admin-username adminuser \
    --ssh-key-value ~/.ssh/id_rsa.pub \
    --nics TestNIC
    ```

    Erwartete Ausgabe:
   
    ```json
    {
        "fqdns": "",
        "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/DNS01",
        "location": "centralus",
        "macAddress": "00-0D-3A-92-C1-66",
        "powerState": "VM running",
        "privateIpAddress": "192.168.1.101",
        "publicIpAddress": "",
        "resourceGroup": "TestRG"
    }
    ```
   
   Andere Parameter als die grundlegenden Parameter von [az vm create](/cli/azure/vm#create).

   * `--nics`: Name der NIC, mit der der virtuelle Computer verknüpft wird.
   

## <a name="retrieve-static-private-ip-address-information-for-a-vm"></a>Abrufen der Informationen zur statischen privaten IP-Adresse für einen virtuellen Computer

Führen Sie den folgenden Befehl der Azure-Befehlszeilenschnittstelle aus, und sehen Sie sich die Werte für *Private IP alloc-method* und *Private IP address* an, um die statische private IP-Adresse zu erhalten, die Sie erstellt haben:

```azurecli
az vm show -g TestRG -n DNS01 --show-details --query 'privateIps'
```

Erwartete Ausgabe:

```json
"192.168.1.101"
```

Um die spezifischen IP-Informationen der NIC für diesen virtuellen Computer anzuzeigen, fragen Sie speziell die NIC ab:

```azurecli
az network nic show \
-g testrg \
-n testnic \
--query 'ipConfigurations[0].{PrivateAddress:privateIpAddress,IPVer:privateIpAddressVersion,IpAllocMethod:p
rivateIpAllocationMethod,PublicAddress:publicIpAddress}'
```

Die Ausgabe sieht etwa wie folgt aus:

```json
{
    "IPVer": "IPv4",
    "IpAllocMethod": "Static",
    "PrivateAddress": "192.168.1.101",
    "PublicAddress": null
}
```

## <a name="remove-a-static-private-ip-address-from-a-vm"></a>Entfernen einer statischen privaten IP-Adresse von einem virtuellen Computer

Es ist nicht möglich, über die Azure-Befehlszeilenschnittstelle für Resource Manager-Bereitstellungen eine statische private IP-Adresse von einer NIC zu entfernen. Die Voraussetzungen lauten wie folgt:
- Erstellen einer neuen NIC, die eine dynamische IP-Adresse verwendet
- Festlegen der NIC auf dem virtuellen Computer auf die neu erstellte NIC 

Zum Ändern der NIC für den in den obigen Befehlen verwendeten virtuellen Computer führen Sie die folgenden Schritte aus.

1. Führen Sie den Befehl **azure network nic create** aus, um eine neue NIC mit dynamischer IP-Zuordnung mit einer neuen IP-Adresse zu erstellen. Beachten Sie, dass die Zuordnungsmethode **Dynamisch** ist, da keine IP-Adresse angegeben wird.

    ```azurecli
    az network nic create     \
    --resource-group TestRG     \
    --name TestNIC2     \
    --location centralus     \
    --subnet FrontEnd    \
    --vnet-name TestVNet
    ```        
   
    Erwartete Ausgabe:

    ```json
    {
        "newNIC": {
            "dnsSettings": {
            "appliedDnsServers": [],
            "dnsServers": []
            },
            "enableIPForwarding": false,
            "ipConfigurations": [
            {
                "etag": "W/\"<guid>\"",
                "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC2/ipConfigurations/ipconfig1",
                "name": "ipconfig1",
                "properties": {
                "primary": true,
                "privateIPAddress": "192.168.1.4",
                "privateIPAllocationMethod": "Dynamic",
                "provisioningState": "Succeeded",
                "subnet": {
                    "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                    "resourceGroup": "TestRG"
                }
                },
                "resourceGroup": "TestRG"
            }
            ],
            "provisioningState": "Succeeded",
            "resourceGuid": "0808a61c-476f-4d08-98ee-0fa83671b010"
        }
    }
    ```

2. Führen Sie den Befehl **azure vm set** aus, um die vom virtuellen Computer verwendete Netzwerkkarte zu ändern.
   
    ```azurecli
    azure vm set -g TestRG -n DNS01 -N TestNIC2
    ```

    Erwartete Ausgabe:
   
    ```json
    [
        {
            "id": "/subscriptions/0e220bf6-5caa-4e9f-8383-51f16b6c109f/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC3",
            "primary": true,
            "resourceGroup": "TestRG"
        }
    ]
    ```

    > [!NOTE]
    > Wenn der virtuelle Computer groß genug ist, um mehr als eine NIC zu enthalten, führen Sie den Befehl **azure network nic delete** zum Löschen der alten NIC aus.
   
## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie mehr über [reservierte öffentliche IP-Adressen](virtual-networks-reserved-public-ip.md) .
* Erfahren Sie mehr über [öffentliche IP-Adressen auf Instanzebene (ILPIP)](virtual-networks-instance-level-public-ip.md) .
* Lesen Sie die Informationen zu [REST-APIs für reservierte IP-Adressen](https://msdn.microsoft.com/library/azure/dn722420.aspx).


