---
title: "Steuern von Routing und virtuellen Geräten mithilfe der Azure CLI 2.0 | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Routing und virtuelle Geräte mit Azure CLI 2.0 steuern können."
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: 
tags: azure-resource-manager
ms.assetid: 5452a0b8-21a6-4699-8d6a-e2d8faf32c25
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/12/2017
ms.author: jdial
translationtype: Human Translation
ms.sourcegitcommit: 6d749e5182fbab04adc32521303095dab199d129
ms.openlocfilehash: 3a85fa624dc55f31822f00910b6d124c1d37323f
ms.lasthandoff: 03/22/2017


---
# <a name="create-user-defined-routes-udr-using-the-azure-cli-20"></a>Erstellen benutzerdefinierter Routen (UDR) mit Azure CLI 2.0

> [!div class="op_single_selector"]
> * [PowerShell](virtual-network-create-udr-arm-ps.md)
> * [Azure-Befehlszeilenschnittstelle](virtual-network-create-udr-arm-cli.md)
> * [Vorlage](virtual-network-create-udr-arm-template.md)
> * [PowerShell (klassische Bereitstellung)](virtual-network-create-udr-classic-ps.md)
> * [CLI (klassische Bereitstellung)](virtual-network-create-udr-classic-cli.md)

## <a name="cli-versions-to-complete-the-task"></a>CLI-Versionen zum Durchführen dieser Aufgabe 

Führen Sie die Aufgabe mit einer der folgenden CLI-Versionen durch: 

- [Azure CLI 1.0:](virtual-network-create-udr-arm-cli-nodejs.md) Unsere CLI für das klassische Bereitstellungsmodell und das Resource Manager-Bereitstellungsmodell 
- [Azure CLI 2.0](#Create-the-UDR-for-the-front-end-subnet): Unsere CLI der nächsten Generation für das Resource Manager-Bereitstellungsmodell (dieser Artikel)

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

[!INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[!INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

Die folgenden Beispielbefehle für die Azure-Befehlszeilenschnittstelle setzen voraus, dass bereits eine einfache Umgebung erstellt wurde, die auf dem obigen Szenario basiert. Wenn Sie die Befehle so ausführen möchten, wie sie in diesem Dokument angezeigt werden, erstellen Sie zunächst die Testumgebung durch Bereitstellen [dieser Vorlage](http://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR-Before). Klicken Sie auf **In Azure bereitstellen**, ersetzen Sie bei Bedarf die Standardparameterwerte, und befolgen Sie dann die Anweisungen im Portal.


## <a name="create-the-udr-for-the-front-end-subnet"></a>Erstellen der benutzerdefinierten Route für das Front-End-Subnetz
Führen Sie zum Erstellen der Routingtabelle und der für das Front-End-Subnetz erforderlichen Route anhand des oben beschriebenen Szenarios die folgenden Schritte aus.

1. Erstellen Sie eine Routingtabelle für das Front-End-Subnetz mit dem Befehl [az network route-table create](/cli/azure/network/route-table#create):

    ```azurecli
    az network route-table create \
    --resource-group testrg \
    --location centralus \
    --name UDR-FrontEnd
    ```

    Ausgabe:

    ```json
    {
    "etag": "W/\"<guid>\"",
    "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/routeTables/UDR-FrontEnd",
    "location": "centralus",
    "name": "UDR-FrontEnd",
    "provisioningState": "Succeeded",
    "resourceGroup": "testrg",
    "routes": [],
    "subnets": null,
    "tags": null,
    "type": "Microsoft.Network/routeTables"
    }
    ```

2. Erstellen Sie eine Route, die sämtlichen an das Back-End-Subnetz (192.168.2.0/24) gerichteten Datenverkehr mit dem Befehl [az network route-table route create](/cli/azure/network/route-table/route#create) an den virtuellen Computer **FW1** (192.168.0.4) umleitet:

    ```azurecli 
    az network route-table route create \
    --resource-group testrg \
    --name RouteToBackEnd \
    --route-table-name UDR-FrontEnd \
    --address-prefix 192.168.2.0/24 \
    --next-hop-type VirtualAppliance \
    --next-hop-ip-address 192.168.0.4
    ```

    Ausgabe:

    ```json
    {
    "addressPrefix": "192.168.2.0/24",
    "etag": "W/\"<guid>\"",
    "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/routeTables/UDR-FrontEnd/routes/RouteToBackEnd",
    "name": "RouteToBackEnd",
    "nextHopIpAddress": "192.168.0.4",
    "nextHopType": "VirtualAppliance",
    "provisioningState": "Succeeded",
    "resourceGroup": "testrg"
    }
    ```
    Parameter:

    * **--route-table-name**. Der Name der Routingtabelle, der die Route hinzugefügt wird. In diesem Szenario *UDR-FrontEnd*.
    * **--address-prefix**. Das Adresspräfix für das Zielsubnetz der Pakete. In diesem Szenario *192.168.2.0/24*.
    * **--next-hop-type**. Der Typ des Zielobjekts für den Datenverkehr. Mögliche Werte sind *VirtualAppliance*, *VirtualNetworkGateway*, *VNETLocal*, *Internet* oder *None*.
    * **--next-hop-ip-address**. Die IP-Adresse für den nächsten Hop. In diesem Szenario *192.168.0.4*.

3. Führen Sie den Befehl [az network vnet subnet update](/cli/azure/network/vnet/subnet#update) aus, um die oben erstellte Routingtabelle dem Subnetz **FrontEnd** zuzuordnen:

    ```azurecli
    az network vnet subnet update \
    --resource-group testrg \
    --vnet-name testvnet \
    --name FrontEnd \
    --route-table UDR-FrontEnd
    ```

    Ausgabe:

    ```json
    {
    "addressPrefix": "192.168.1.0/24",
    "etag": "W/\"<guid>\"",
    "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testvnet/subnets/FrontEnd",
    "ipConfigurations": null,
    "name": "FrontEnd",
    "networkSecurityGroup": null,
    "provisioningState": "Succeeded",
    "resourceGroup": "testrg",
    "resourceNavigationLinks": null,
    "routeTable": {
        "etag": null,
        "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/routeTables/UDR-FrontEnd",
        "location": null,
        "name": null,
        "provisioningState": null,
        "resourceGroup": "testrg",
        "routes": null,
        "subnets": null,
        "tags": null,
        "type": null
        }
    }
    ```

    Parameter:
    
    * **--vnet-name**. Name des VNets mit dem Subnetz. In diesem Szenario *TestVNet*.

## <a name="create-the-udr-for-the-back-end-subnet"></a>Erstellen der benutzerdefinierten Route für das Back-End-Subnetz

Führen Sie die folgenden Schritte aus, um die Routingtabelle und die für das Back-End-Subnetz erforderliche Route anhand des oben beschriebenen Szenarios zu erstellen:

1. Führen Sie den folgenden Befehl aus, um eine Routingtabelle für das Back-End-Subnetz zu erstellen:

    ```azurecli
    az network route-table create \
    --resource-group testrg \
    --name UDR-BackEnd \
    --location centralus
    ```

2. Führen Sie den folgenden Befehl aus, um in der Routingtabelle eine Route zu erstellen, die sämtlichen an das Front-End-Subnetz (192.168.1.0/24) gerichteten Datenverkehr an den virtuellen Computer **FW1** (192.168.0.4) umleitet:

    ```azurecli
    az network route-table route create \
    --resource-group testrg \
    --name RouteToFrontEnd \
    --route-table-name UDR-BackEnd \
    --address-prefix 192.168.1.0/24 \
    --next-hop-type VirtualAppliance \
    --next-hop-ip-address 192.168.0.4
    ```

3. Führen Sie den folgenden Befehl aus, um die oben erstellte Routingtabelle dem Subnetz **BackEnd** zuzuordnen:

    ```azurecli
    az network vnet subnet update \
    --resource-group testrg \
    --vnet-name testvnet \
    --name BackEnd \
    --route-table UDR-BackEnd
    ```

## <a name="enable-ip-forwarding-on-fw1"></a>Aktivieren der IP-Weiterleitung auf "FW1"

Führen Sie die folgenden Schritte aus, um die IP-Weiterleitung in der vom virtuellen Computer **FW1** verwendeten NIC zu aktivieren:

1. Führen Sie den Befehl [az network nic show](/cli/az/network/nic#show) mit einem JMESPATH-Filter aus, um den aktuellen **enable-ip-forwarding**-Wert für **Enable IP forwarding** (IP-Weiterleitung aktivieren) anzuzeigen. Er sollte auf *false*festgelegt sein.

    ```azurecli
    az network nic show \
    --resource-group testrg \
    --nname nicfw1 \
    --query 'enableIpForwarding' -o tsv
    ```

    Ausgabe:

        false

2. Führen Sie den folgenden Befehl aus, um die IP-Weiterleitung zu aktivieren:

    ```azurecli
    az network nic update \
    --resource-group testrg \
    --name nicfw1 \
    --ip-forwarding true
    ```

    Sie können die an die Konsole gestreamte Ausgabe untersuchen oder einfach für den jeweiligen **enableIpForwarding**-Wert neu testen:

    ```azurecli
    az network nic show -g testrg -n nicfw1 --query 'enableIpForwarding' -o tsv
    ```

    Ausgabe:

        true

    Parameter:

    **--ip-forwarding:** *true* oder *false*.


