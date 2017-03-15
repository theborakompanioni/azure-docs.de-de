---
title: "Verwalten von Netzwerksicherheitsgruppen – Azure PowerShell | Microsoft-Dokumentation"
description: Erfahren Sie, wie Sie Netzwerksicherheitsgruppen mithilfe von PowerShell verwalten.
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 3706ce6c-d9ae-46cb-a048-f0a4e84dc5cc
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/14/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 63f2f6dde56c1b5c4b3ad2591700f43f6542874d
ms.openlocfilehash: edb23ae41e175061607d3a191c839e1194fa862b
ms.lasthandoff: 02/28/2017


---
# <a name="manage-network-security-groups-using-powershell"></a>Verwalten von Netzwerksicherheitsgruppen mithilfe von PowerShell

[!INCLUDE [virtual-network-manage-arm-selectors-include.md](../../includes/virtual-network-manage-nsg-arm-selectors-include.md)]

[!INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

> [!NOTE]
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Resource Manager-Bereitstellung und klassische Bereitstellung](../resource-manager-deployment-model.md). Dieser Artikel befasst sich mit dem Resource Manager-Bereitstellungsmodell, das von Microsoft für die meisten neuen Bereitstellungen anstatt des klassischen Bereitstellungsmodells empfohlen wird.
>

[!INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="retrieve-information"></a>Abrufen von Informationen
Sie können Ihre vorhandenen NSGs sehen, Regeln für eine vorhandene NSG abrufen und herausfinden, welchen Ressourcen eine NSG zugeordnet ist.

### <a name="view-existing-nsgs"></a>Anzeigen lassen vorhandener NSGs
Um alle vorhandenen NSGs in einem Abonnement anzuzeigen, führen Sie das `Get-AzureRmNetworkSecurityGroup`-Cmdlet aus.

Erwartetes Ergebnis:

    Name                 : NSG-BackEnd
    ResourceGroupName    : RG-NSG
    Location             : westus
    Id                   : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-BackEnd
    Etag                 : W/"[Id]"
    ResourceGuid         : [Id]
    ProvisioningState    : Succeeded
    Tags                 :                            
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]

    Name                 : NSG-FrontEnd
    ResourceGroupName    : RG-NSG
    Location             : eastus
    Id                   : /subscriptions/[Subscription Id]/resourceGroups/NRP-RG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
    Etag                 : W/"[Id]"
    ResourceGuid         : [Id]
    ProvisioningState    : Succeeded
    Tags                 : 
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]

    Name                 : WEB1
    ResourceGroupName    : RG101
    Location             : eastus2
    Id                   : /subscriptions/[Subscription Id]/resourceGroups/RG101/providers/M
                           icrosoft.Network/networkSecurityGroups/WEB1
    Etag                 : W/"[Id]"
    ResourceGuid         : [Id]
    ProvisioningState    : Succeeded
    Tags                 : 
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]


Um die Liste der vorhandenen NSGs in einer bestimmten Ressourcengruppe anzuzeigen, führen Sie das `Get-AzureRmNetworkSecurityGroup`-Cmdlet aus.

Erwartete Ausgabe:

    Name                 : NSG-BackEnd
    ResourceGroupName    : RG-NSG
    Location             : westus
    Id                   : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-BackEnd
    Etag                 : W/"[Id]"
    ResourceGuid         : [Id]
    ProvisioningState    : Succeeded
    Tags                 :                            
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]

    Name                 : NSG-FrontEnd
    ResourceGroupName    : RG-NSG
    Location             : eastus
    Id                   : /subscriptions/[Subscription Id]/resourceGroups/NRP-RG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
    Etag                 : W/"[Id]"
    ResourceGuid         : [Id]
    ProvisioningState    : Succeeded
    Tags                 : 
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]

### <a name="list-all-rules-for-an-nsg"></a>Auflisten aller Regeln für eine NSG
Geben Sie den folgenden Befehl ein, um die Regeln für eine NSG namens **NSG-FrontEnd** anzuzeigen:

```powershell
Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd | Select SecurityRules -ExpandProperty SecurityRules
```

Erwartete Ausgabe:

    Name                     : rdp-rule
    Id                       : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/                           Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/rdp-rule
    Etag                     : W/"[Id]"
    ProvisioningState        : Succeeded
    Description              : Allow RDP
    Protocol                 : Tcp
    SourcePortRange          : *
    DestinationPortRange     : 3389
    SourceAddressPrefix      : Internet
    DestinationAddressPrefix : *
    Access                   : Allow
    Priority                 : 100
    Direction                : Inbound

    Name                     : web-rule
    Id                       : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/                           Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/web-rule
    Etag                     : W/"[Id]"
    ProvisioningState        : Succeeded
    Description              : Allow HTTP
    Protocol                 : Tcp
    SourcePortRange          : *
    DestinationPortRange     : 80
    SourceAddressPrefix      : Internet
    DestinationAddressPrefix : *
    Access                   : Allow
    Priority                 : 101
    Direction                : Inbound

> [!NOTE]
> Sie können auch `Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name "NSG-FrontEnd" | Select DefaultSecurityRules -ExpandProperty DefaultSecurityRules` verwenden, um die Standardregeln von der **NSG-FrontEnd**-NSG aus aufzulisten.
> 

### <a name="view-nsgs-associations"></a>Anzeigen von NSG-Zuordnungen
Führen Sie den folgenden Befehl aus, um anzuzeigen, welchen Ressourcen die NSG **NSG-FrontEnd** zugeordnet ist:

```powershell
Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
```

Suchen Sie, wie unten gezeigt, nach den Eigenschaften **NetworkInterfaces** und **Subnets**:

    NetworkInterfaces    : []
    Subnets              : [
                             {
                               "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                               "IpConfigurations": []
                             }
                           ]

Im vorherigen Beispiel ist die NSG keinen Netzwerkschnittstellen (NICs) zugeordnet. Sie ist einem Subnetz namens **FrontEnd** zugeordnet.

## <a name="manage-rules"></a>Verwalten von Regeln
Sie können einer vorhandenen NSG Regeln hinzufügen, vorhandene Regeln bearbeiten und Regeln entfernen.

### <a name="add-a-rule"></a>Hinzufügen einer Regel
Um eine Regel hinzuzufügen, die **eingehenden** Verkehr an Port **443** von jedem Computer aus an die **NSG-FrontEnd**-NSG zulässt, führen Sie die folgenden Schritte aus:

1. Führen Sie den folgenden Befehl aus, um die vorhandene NSG abzurufen und in einer Variable zu speichern:

    ```powershell   
    $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
    ```

2. Führen Sie den folgenden Befehl aus, um eine Regel zur NSG hinzuzufügen:

    ```powershell
    Add-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
    -Name https-rule `
    -Description "Allow HTTPS" `
    -Access Allow `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 102 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 443
    ```

3. Um die an der NSG vorgenommenen Änderungen zu speichern, führen Sie den folgenden Befehl aus:

    ```powershell
    Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
    ```
    Die erwartete Ausgabe zeigt nur die Sicherheitsregeln:
   
        Name                 : NSG-FrontEnd
        ...
        SecurityRules        : [
                                 {
                                   "Name": "rdp-rule",
                                   ...
                                 },
                                 {
                                   "Name": "web-rule",
                                   ...
                                 },
                                 {
                                   "Name": "https-rule",
                                   "Etag": "W/\"[Id]\"",
                                   "Id": "/subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/https-rule",
                                   "Description": "Allow HTTPS",
                                   "Protocol": "Tcp",
                                   "SourcePortRange": "*",
                                   "DestinationPortRange": "443",
                                   "SourceAddressPrefix": "*",
                                   "DestinationAddressPrefix": "*",
                                   "Access": "Allow",
                                   "Priority": 102,
                                   "Direction": "Inbound",
                                   "ProvisioningState": "Succeeded"
                                 }
                               ]

### <a name="change-a-rule"></a>Ändern einer Regel
Um die oben erstellte Regel zu ändern, sodass eingehender Verkehr nur aus dem **Internet** zugelassen wird, führen Sie die folgenden Schritte aus.

1. Führen Sie den folgenden Befehl aus, um die vorhandene NSG abzurufen und in einer Variable zu speichern:

    ```powershell 
    $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
    ```

2. Führen Sie den folgenden Befehl mit den neuen Regeleinstellungen aus:

    ```powershell
    Set-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
    -Name https-rule `
    -Description "Allow HTTPS" `
    -Access Allow `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 102 `
    -SourceAddressPrefix * `
    -SourcePortRange Internet `
    -DestinationAddressPrefix * `
    -DestinationPortRange 443
    ```

3. Um die an der NSG vorgenommenen Änderungen zu speichern, führen Sie den folgenden Befehl aus:

    ```powershell
    Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
    ```

    Die erwartete Ausgabe zeigt nur die Sicherheitsregeln:
   
        Name                 : NSG-FrontEnd
        ...
        SecurityRules        : [
                                 {
                                   "Name": "rdp-rule",
                                   ...
                                 },
                                 {
                                   "Name": "web-rule",
                                   ...
                                 },
                                 {
                                   "Name": "https-rule",
                                   "Etag": "W/\"[Id]\"",
                                   "Id": "/subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/https-rule",
                                   "Description": "Allow HTTPS",
                                   "Protocol": "Tcp",
                                   "SourcePortRange": "*",
                                   "DestinationPortRange": "443",
                                   "SourceAddressPrefix": "Internet",
                                   "DestinationAddressPrefix": "*",
                                   "Access": "Allow",
                                   "Priority": 102,
                                   "Direction": "Inbound",
                                   "ProvisioningState": "Succeeded"
                                 }
                               ]

### <a name="delete-a-rule"></a>Löschen einer Regel
1. Führen Sie den folgenden Befehl aus, um die vorhandene NSG abzurufen und in einer Variable zu speichern:

    ```powershell
    $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
    ```

2. Führen Sie den folgenden Befehl aus, um die Regel aus der NSG zu entfernen:

    ```powershell
    Remove-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg -Name https-rule
    ```

3. Um die Änderungen an der NSG zu speichern, führen Sie den folgenden Befehl aus:

    ```powershell
    Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
    ```

    Die erwartete Ausgabe zeigt nur die Sicherheitsregeln. Beachten Sie, dass **https-rule** nicht mehr aufgelistet ist:
   
        Name                 : NSG-FrontEnd
        ...
        SecurityRules        : [
                                 {
                                   "Name": "rdp-rule",
                                   ...
                                 },
                                 {
                                   "Name": "web-rule",
                                   ...
                                 }
                               ]

## <a name="manage-associations"></a>Verwalten von Zuordnungen
Sie können eine NSG Subnetzen und NICs zuordnen. Sie können auch die Zuordnung einer NSG zu jeder beliebigen Ressource, der sie zugeordnet ist, aufheben.

### <a name="associate-an-nsg-to-a-nic"></a>Zuordnen einer NSG zu einer NIC
Führen Sie die folgenden Schritte aus, um die **NSG-FrontEnd**-NSG der **TestNICWeb1**-NIC zuzuordnen:

1. Führen Sie den folgenden Befehl aus, um die vorhandene NSG abzurufen und in einer Variable zu speichern:

    ```powershell
    $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
    ```

2. Führen Sie den folgenden Befehl aus, um die vorhandene NIC abzurufen und in einer Variable zu speichern:

    ```powershell
    $nic = Get-AzureRmNetworkInterface -ResourceGroupName RG-NSG -Name TestNICWeb1
    ```

3. Setzen Sie mit dem folgenden Befehl die Eigenschaft **NetworkSecurityGroup** der Variablen **NIC** auf den Wert der **NSG**-Variablen, wie unten dargestellt:

    ```powershell
    $nic.NetworkSecurityGroup = $nsg
    ```

4. Um die an der NIC vorgenommenen Änderungen zu speichern, führen Sie den folgenden Befehl aus:

    ```powershell
    Set-AzureRmNetworkInterface -NetworkInterface $nic
    ```
   
    Die erwartete Ausgabe zeigt nur die Eigenschaft **NetworkSecurityGroup** :
   
        NetworkSecurityGroup : {
                                 "SecurityRules": [],
                                 "DefaultSecurityRules": [],
                                 "NetworkInterfaces": [],
                                 "Subnets": [],
                                 "Id": "/subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd"
                               }

### <a name="dissociate-an-nsg-from-a-nic"></a>Aufheben der Zuordnung einer NSG zu einer NIC
Führen Sie die folgenden Schritte aus, um die Zuordnung der **NSG-FrontEnd**-NSG zur **TestNICWeb1**-NIC aufzuheben:

1. Führen Sie den folgenden Befehl aus, um die vorhandene NIC abzurufen und in einer Variable zu speichern:

    ```powershell
    $nic = Get-AzureRmNetworkInterface -ResourceGroupName RG-NSG -Name TestNICWeb1
    ```

2. Setzen Sie mit dem folgenden Befehl die Eigenschaft **NetworkSecurityGroup** der Variablen **NIC** auf **$null**:

    ```powershell
    $nic.NetworkSecurityGroup = $null
    ```

3. Um die an der NIC vorgenommenen Änderungen zu speichern, führen Sie den folgenden Befehl aus:

    ```powershell
    Set-AzureRmNetworkInterface -NetworkInterface $nic
    ```
   
    Die erwartete Ausgabe zeigt nur die Eigenschaft **NetworkSecurityGroup** :
   
        NetworkSecurityGroup : null

### <a name="dissociate-an-nsg-from-a-subnet"></a>Aufheben der Zuordnung einer NSG zu einem Subnetz
Führen Sie die folgenden Schritte aus, um die Zuordnung der **NSG-FrontEnd**-NSG zum **FrontEnd**-Subnetz aufzuheben:

1. Führen Sie den folgenden Befehl aus, um das vorhandene VNet abzurufen und in einer Variable zu speichern:

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName RG-NSG -Name TestVNet
    ```

2. Führen Sie den folgenden Befehl aus, um das vorhandene **FrontEnd**-Subnetz abzurufen und in einer Variable zu speichern:

    ```powershell
    $subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name FrontEnd
    ```
 
3. Setzen Sie mit dem folgenden Befehl die Eigenschaft **NetworkSecurityGroup** der **Subnetzvariable** auf **$null**:

    ```powershell
    $subnet.NetworkSecurityGroup = $null
    ```

4. Um die am Subnetz vorgenommenen Änderungen zu speichern, führen Sie den folgenden Befehl aus:

    ```powershell
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
    ```

    Die erwartete Ausgabe zeigt nur die Eigenschaften des **FrontEnd** -Subnetzes. Beachten Sie, dass keine Eigenschaft für **NetworkSecurityGroup**vorhanden ist:
   
            ...
            Subnets           : [
                                  {
                                    "Name": "FrontEnd",
                                    "Etag": "W/\"[Id]\"",
                                    "Id": "/subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                                    "AddressPrefix": "192.168.1.0/24",
                                    "IpConfigurations": [
                                      {
                                        "Id": "/subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb2/ipConfigurations/ipconfig1"
                                      },
                                      {
                                        "Id": "/subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1/ipConfigurations/ipconfig1"
                                      }
                                    ],
                                    "ProvisioningState": "Succeeded"
                                  },
                                    ...
                                ]

### <a name="associate-an-nsg-to-a-subnet"></a>Zuordnen einer NSG zu einem Subnetz
Führen Sie die folgenden Schritte aus, um die **NSG-FrontEnd**-NSG erneut dem **FrontEnd**-Subnetz zuzuordnen:

1. Führen Sie den folgenden Befehl aus, um das vorhandene VNet abzurufen und in einer Variable zu speichern:

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName RG-NSG -Name TestVNet
    ```

2. Führen Sie den folgenden Befehl aus, um das vorhandene **FrontEnd**-Subnetz abzurufen und in einer Variable zu speichern:

    ```powershell
    $subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name FrontEnd
    ```
 
3. Führen Sie den folgenden Befehl aus, um die vorhandene NSG abzurufen und in einer Variable zu speichern:

    ```powershell
    $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
    ```

4. Setzen Sie mit dem folgenden Befehl die Eigenschaft **NetworkSecurityGroup** der **Subnetzvariable** auf **$null**:

    ```powershell
    $subnet.NetworkSecurityGroup = $nsg
    ```

5. Um die am Subnetz vorgenommenen Änderungen zu speichern, führen Sie den folgenden Befehl aus:

    ```powershell
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
    ```

    Die erwartete Ausgabe zeigt nur die Eigenschaft **NetworkSecurityGroup** des **FrontEnd**-Subnetzes:
   
        ...
        "NetworkSecurityGroup": {
                                  "SecurityRules": [],
                                  "DefaultSecurityRules": [],
                                  "NetworkInterfaces": [],
                                  "Subnets": [],
                                  "Id": "/subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd"
                                }
        ...

## <a name="delete-an-nsg"></a>Löschen einer NSG
Sie können eine NSG nur löschen, wenn sie keiner Ressource zugeordnet ist. Um eine NSG zu löschen, gehen Sie folgendermaßen vor.

1. Um die Ressourcen, die einer NSG zugeordnet sind, zu überprüfen, führen Sie `azure network nsg show` aus, wie in [Anzeigen von NSG-Zuordnungen](#View-NSGs-associations)gezeigt.
2. Falls die NSG einer NIC zugeordnet ist, führen Sie `azure network nic set` , wie in [Aufheben der Zuordnung einer NSG zu einer NIC](#Dissociate-an-NSG-from-a-NIC) gezeigt, für jede NIC aus. 
3. Falls die NSG einem Subnetz zugeordnet ist, führen Sie `azure network vnet subnet set` , wie in [Aufheben der Zuordnung einer NSG zu einem Subnetz](#Dissociate-an-NSG-from-a-subnet) gezeigt, für jedes Subnetz aus.
4. Führen Sie den folgenden Befehl aus, um die NSG zu löschen:

    ```powershell
    Remove-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd -Force
    ```
   
   > [!NOTE]
   > Der Parameter `-Force` stellt sicher, dass Sie den Löschvorgang nicht bestätigen müssen.
   > 

## <a name="next-steps"></a>Nächste Schritte
* [Aktivieren der Protokollierung](virtual-network-nsg-manage-log.md) für NSGs.


