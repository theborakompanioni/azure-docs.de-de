---
title: "Verwalten von Netzwerksicherheitsgruppen – Azure CLI 2.0 | Microsoft-Dokumentation"
description: Hier erfahren Sie, wie Sie Netzwerksicherheitsgruppen mithilfe der Azure-Befehlszeilenschnittstelle (Azure CLI 2.0) verwalten.
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: ed17d314-07e6-4c7f-bcf1-a8a2535d7c14
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/21/2017
ms.author: jdial
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 63f2f6dde56c1b5c4b3ad2591700f43f6542874d
ms.openlocfilehash: dcb0455fe223e99b4f1b0035b1d1109ecf5ee268
ms.lasthandoff: 02/28/2017


---
# <a name="manage-network-security-groups-using-the-azure-cli-20"></a>Verwalten von Netzwerksicherheitsgruppen mit der Azure CLI 2.0

[!INCLUDE [virtual-network-manage-arm-selectors-include.md](../../includes/virtual-network-manage-nsg-arm-selectors-include.md)]

## <a name="cli-versions-to-complete-the-task"></a>CLI-Versionen zum Durchführen dieser Aufgabe 

Führen Sie die Aufgabe mit einer der folgenden CLI-Versionen durch: 

- [Azure CLI 1.0:](virtual-network-manage-nsg-cli-nodejs.md) Unsere CLI für das klassische Bereitstellungsmodell und das Resource Manager-Bereitstellungsmodell 
- [Azure CLI 2.0](#View-existing-NSGs): Unsere CLI der nächsten Generation für das Resource Manager-Bereitstellungsmodell (dieser Artikel)


[!INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

> [!NOTE]
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Resource Manager-Bereitstellung und klassische Bereitstellung](../resource-manager-deployment-model.md). Dieser Artikel befasst sich mit dem Resource Manager-Bereitstellungsmodell, das von Microsoft für die meisten neuen Bereitstellungen anstatt des klassischen Bereitstellungsmodells empfohlen wird.
> 

[!INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

## <a name="prerequisite"></a>Voraussetzung
Falls noch nicht geschehen, installieren und konfigurieren Sie die neueste [Azure CLI 2.0](/cli/azure/install-az-cli2), und melden Sie sich mit [az login](/cli/azure/#login) bei einem Azure-Konto an. 


## <a name="view-existing-nsgs"></a>Anzeigen lassen vorhandener NSGs
Führen Sie zum Anzeigen der Liste von NSGs in einer bestimmten Ressourcengruppe den Befehl [az network nsg list](/cli/azure/network/nsg#list) mit dem Ausgabeformat `-o table` aus:

```azurecli
az network nsg list -g RG-NSG -o table
```

Erwartete Ausgabe:

    Location    Name          ProvisioningState    ResourceGroup    ResourceGuid
    ----------  ------------  -------------------  ---------------  ------------------------------------
    centralus   NSG-BackEnd   Succeeded            RG-NSG           <guid>
    centralus   NSG-FrontEnd  Succeeded            RG-NSG           <guid>

## <a name="list-all-rules-for-an-nsg"></a>Auflisten aller Regeln für eine NSG
Führen Sie zum Anzeigen der Regeln einer NSG mit dem Namen **NSG-FrontEnd** den Befehl [az network nsg show](/cli/azure/network/nsg#show) mit einem [JMESPATH-Abfragefilter](/cli/azure/query-az-cli2) und dem Ausgabeformat `-o table` aus:

```azurecli
    az network nsg show \
    --resource-group RG-NSG \
    --name NSG-FrontEnd \
    --query '[defaultSecurityRules[],securityRules[]][].{Name:name,Desc:description,Access:access,Direction:direction,DestPortRange:destinationPortRange,DestAddrPrefix:destinationAddressPrefix,SrcPortRange:sourcePortRange,SrcAddrPrefix:sourceAddressPrefix}' \
    -o table
```

Erwartete Ausgabe:

    Name                           Desc                                                    Access    Direction    DestPortRange    DestAddrPrefix    SrcPortRange    SrcAddrPrefix
    -----------------------------  ------------------------------------------------------  --------  -----------  ---------------  ----------------  --------------  -----------------
    AllowVnetInBound               Allow inbound traffic from all VMs in VNET              Allow     Inbound      *                VirtualNetwork    *               VirtualNetwork
    AllowAzureLoadBalancerInBound  Allow inbound traffic from azure load balancer          Allow     Inbound      *                *                 *               AzureLoadBalancer
    DenyAllInBound                 Deny all inbound traffic                                Deny      Inbound      *                *                 *               *
    AllowVnetOutBound              Allow outbound traffic from all VMs to all VMs in VNET  Allow     Outbound     *                VirtualNetwork    *               VirtualNetwork
    AllowInternetOutBound          Allow outbound traffic from all VMs to Internet         Allow     Outbound     *                Internet          *               *
    DenyAllOutBound                Deny all outbound traffic                               Deny      Outbound     *                *                 *               *
    rdp-rule                                                                               Allow     Inbound      3389             *                 *               Internet
    web-rule                                                                               Allow     Inbound      80               *                 *               Internet
> [!NOTE]
> Sie können auch [az network nsg rule list](/cli/azure/network/nsg/rule#list) verwenden, um nur die benutzerdefinierten Regeln einer NSG aufzulisten.
>

## <a name="view-nsg-associations"></a>Anzeigen lassen von NSG-Zuordnungen

Um sich anzeigen zu lassen, welchen Ressourcen die **NSG-FrontEnd**-NSG zugeordnet ist, führen Sie den Befehl `az network nsg show`, wie unten gezeigt, aus. 

```azurecli
az network nsg show -g RG-NSG -n nsg-frontend --query '[subnets,networkInterfaces]'
```

Suchen Sie, wie unten gezeigt, nach den Eigenschaften **NetworkInterfaces** und **Subnets**:

```json
[
  [
    {
      "addressPrefix": null,
      "etag": null,
      "id": "/subscriptions/<guid>/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNET/subnets/FrontEnd",
      "ipConfigurations": null,
      "name": null,
      "networkSecurityGroup": null,
      "provisioningState": null,
      "resourceGroup": "RG-NSG",
      "resourceNavigationLinks": null,
      "routeTable": null
    }
  ],
  null
]
```

Im Beispiel oben ist die NSG keinen Netzwerkschnittstellen (NICs) zugeordnet. Sie ist einem Subnetz namens **FrontEnd** zugeordnet.

## <a name="add-a-rule"></a>Hinzufügen einer Regel
Um eine Regel hinzuzufügen, die **eingehenden** Verkehr an Port **443** von jedem Computer aus an die **NSG-FrontEnd**-NSG zulässt, geben Sie den folgenden Befehl ein:

```azurecli
az network nsg rule create  \
--resource-group RG-NSG \
--nsg-name NSG-FrontEnd  \
--name allow-https \
--description "Allow access to port 443 for HTTPS" \
--access Allow \
--protocol Tcp  \
--direction Inbound \
--priority 102 \
--source-address-prefix "*"  \
--source-port-range "*"  \
--destination-address-prefix "*" \
--destination-port-range "443"
```

Erwartete Ausgabe:

```json
{
  "access": "Allow",
  "description": "Allow access to port 443 for HTTPS",
  "destinationAddressPrefix": "*",
  "destinationPortRange": "443",
  "direction": "Inbound",
  "etag": "W/\"<guid>\"",
  "id": "/subscriptions/<guid>/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/allow-https",
  "name": "allow-https",
  "priority": 102,
  "protocol": "Tcp",
  "provisioningState": "Succeeded",
  "resourceGroup": "RG-NSG",
  "sourceAddressPrefix": "*",
  "sourcePortRange": "*"
}
```

## <a name="change-a-rule"></a>Ändern einer Regel
Um die oben erstellte Regel so zu ändern, dass eingehender Verkehr nur aus dem **Internet** zugelassen wird, führen Sie den Befehl [az network nsg rule update](/cli/azure/network/nsg/rule#update) aus:

```azurecli
az network nsg rule update \
--resource-group RG-NSG \
--nsg-name NSG-FrontEnd \
--name allow-https \
--source-address-prefix Internet
```

Erwartete Ausgabe:

```json
{
"access": "Allow",
"description": "Allow access to port 443 for HTTPS",
"destinationAddressPrefix": "*",
"destinationPortRange": "443",
"direction": "Inbound",
"etag": "W/\"<guid>\"",
"id": "/subscriptions/<guid>/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/allow-https",
"name": "allow-https",
"priority": 102,
"protocol": "Tcp",
"provisioningState": "Succeeded",
"resourceGroup": "RG-NSG",
"sourceAddressPrefix": "Internet",
"sourcePortRange": "*"
}
```

## <a name="delete-a-rule"></a>Löschen einer Regel
Um die oben erstellte Regel zu löschen, führen Sie den folgenden Befehl aus:

```azurecli
az network nsg rule delete \
--resource-group RG-NSG \
--nsg-name NSG-FrontEnd \
--name allow-https
```


## <a name="associate-an-nsg-to-a-nic"></a>Zuordnen einer NSG zu einer NIC
Um die NSG **NSG-FrontEnd** der NIC **TestNICWeb1** zuzuordnen, verwenden Sie den Befehl [az network nic update](/cli/azure/network/nic#update):

```azurecli
az network nic update \
--resource-group RG-NSG \
--name TestNICWeb1 \
--network-security-group NSG-FrontEnd    
```

Erwartete Ausgabe:

```json
{
  "dnsSettings": {
    "appliedDnsServers": [],
    "dnsServers": [],
    "internalDnsNameLabel": null,
    "internalDomainNameSuffix": "k0wkaguidnqrh0ud.gx.internal.cloudapp.net",
    "internalFqdn": null
  },
  "enableAcceleratedNetworking": false,
  "enableIpForwarding": false,
  "etag": "W/\"<guid>\"",
  "id": "/subscriptions/<guid>/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1",
  "ipConfigurations": [
    {
      "applicationGatewayBackendAddressPools": null,
      "etag": "W/\"<guid>\"",
      "id": "/subscriptions/<guid>/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1/ipConfigurations/ipconfig1",
      "loadBalancerBackendAddressPools": null,
      "loadBalancerInboundNatRules": null,
      "name": "ipconfig1",
      "primary": true,
      "privateIpAddress": "192.168.1.6",
      "privateIpAddressVersion": "IPv4",
      "privateIpAllocationMethod": "Static",
      "provisioningState": "Succeeded",
      "publicIpAddress": null,
      "resourceGroup": "RG-NSG",
      "subnet": {
        "addressPrefix": null,
        "etag": null,
        "id": "/subscriptions/<guid>/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
        "ipConfigurations": null,
        "name": null,
        "networkSecurityGroup": null,
        "provisioningState": null,
        "resourceGroup": "RG-NSG",
        "resourceNavigationLinks": null,
        "routeTable": null
      }
    }
  ],
  "location": "centralus",
  "macAddress": "00-0D-3A-91-A9-60",
  "name": "TestNICWeb1",
  "networkSecurityGroup": {
    "defaultSecurityRules": null,
    "etag": null,
    "id": "/subscriptions/<guid>/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd",
    "location": null,
    "name": null,
    "networkInterfaces": null,
    "provisioningState": null,
    "resourceGroup": "RG-NSG",
    "resourceGuid": null,
    "securityRules": null,
    "subnets": null,
    "tags": null,
    "type": null
  },
  "primary": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "RG-NSG",
  "resourceGuid": "<guid>",
  "tags": {},
  "type": "Microsoft.Network/networkInterfaces",
  "virtualMachine": null
}
```

## <a name="dissociate-an-nsg-from-a-nic"></a>Aufheben der Zuordnung einer NSG zu einer NIC

Um die Zuordnung der NSG **NSG-FrontEnd** zur NIC **TestNICWeb1** aufzuheben, führen Sie den Befehl [az network nsg rule update](/cli/azure/network/nsg/rule#update) erneut aus, ersetzen Sie dabei jedoch das Argument `--network-security-group` durch eine leere Zeichenfolge (`""`).

```azurecli
az network nic update --resource-group RG-NSG --name TestNICWeb3 --network-security-group ""
```

In der Ausgabe wird der Schlüssel `networkSecurityGroup` auf null festgelegt.

## <a name="dissociate-an-nsg-from-a-subnet"></a>Aufheben der Zuordnung einer NSG zu einem Subnetz
Um die Zuordnung der NSG **NSG-FrontEnd** zum Subnetz **FrontEnd** aufzuheben, führen Sie den Befehl [az network nsg rule update](/cli/azure/network/nsg/rule#update) erneut aus, ersetzen Sie dabei jedoch das Argument `--network-security-group` durch eine leere Zeichenfolge (`""`).

```azurecli
az network vnet subnet update \
--resource-group RG-NSG \
--vnet-name testvnet \
--name FrontEnd \
--network-security-group ""
```

In der Ausgabe wird der Schlüssel `networkSecurityGroup` auf null festgelegt.

## <a name="associate-an-nsg-to-a-subnet"></a>Zuordnen einer NSG zu einem Subnetz
Um die NSG **NSG-FrontEnd** dem Subnetz **FrontEnd** erneut zuzuordnen, führen Sie folgenden Befehl aus:

```azurecli
az network vnet subnet update \
--resource-group RG-NSG \
--vnet-name testvnet \
--name FrontEnd \
--network-security-group NSG-FrontEnd
```

In der Ausgabe hat der Schlüssel `networkSecurityGroup` etwa den folgenden Wert:

```json
"networkSecurityGroup": {
    "defaultSecurityRules": null,
    "etag": null,
    "id": "/subscriptions/0e220bf6-5caa-4e9f-8383-51f16b6c109f/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd",
    "location": null,
    "name": null,
    "networkInterfaces": null,
    "provisioningState": null,
    "resourceGroup": "RG-NSG",
    "resourceGuid": null,
    "securityRules": null,
    "subnets": null,
    "tags": null,
    "type": null
  }
  ```

## <a name="delete-an-nsg"></a>Löschen einer NSG
Sie können eine NSG nur löschen, wenn sie keiner Ressource zugeordnet ist. Um eine NSG zu löschen, gehen Sie folgendermaßen vor.

1. Um die Ressourcen, die einer NSG zugeordnet sind, zu überprüfen, führen Sie `azure network nsg show` aus, wie in [Anzeigen von NSG-Zuordnungen](#View-NSGs-associations)gezeigt.
2. Falls die NSG einer NIC zugeordnet ist, führen Sie `azure network nic set` , wie in [Aufheben der Zuordnung einer NSG zu einer NIC](#Dissociate-an-NSG-from-a-NIC) gezeigt, für jede NIC aus. 
3. Falls die NSG einem Subnetz zugeordnet ist, führen Sie `azure network vnet subnet set` , wie in [Aufheben der Zuordnung einer NSG zu einem Subnetz](#Dissociate-an-NSG-from-a-subnet) gezeigt, für jedes Subnetz aus.
4. Führen Sie den folgenden Befehl aus, um die NSG zu löschen:

    ```azurecli
    az network nsg delete --resource-group RG-NSG --name NSG-FrontEnd
    ```
## <a name="next-steps"></a>Nächste Schritte
* [Aktivieren der Protokollierung](virtual-network-nsg-manage-log.md) für NSGs.


