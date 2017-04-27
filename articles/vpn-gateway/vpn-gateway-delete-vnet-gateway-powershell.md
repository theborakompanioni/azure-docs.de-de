---
title: "Löschen eines Gateways des virtuellen Netzwerks: PowerShell: Azure Resource Manager | Microsoft Docs"
description: "Löschen Sie ein Gateway des virtuellen Netzwerks mit PowerShell im Resource Manager-Bereitstellungsmodell."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: 
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/12/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 197ebd6e37066cb4463d540284ec3f3b074d95e1
ms.openlocfilehash: d2bb2422a8458f1577c14bad8d24d8c9cb3ead1b
ms.lasthandoff: 03/31/2017


---
# <a name="delete-a-virtual-network-gateway-using-powershell"></a>Löschen eines Gateways des virtuellen Netzwerks mit PowerShell
> [!div class="op_single_selector"]
> * [Resource Manager – Azure-Portal](vpn-gateway-delete-vnet-gateway-portal.md)
> * [Resource Manager – PowerShell](vpn-gateway-delete-vnet-gateway-powershell.md)
> * [Klassisch – PowerShell](vpn-gateway-delete-vnet-gateway-classic-powershell.md)
>
>

Sie können zwischen ein paar unterschiedlichen Ansätzen wählen, wenn Sie ein Gateway des virtuellen Netzwerks für eine VPN-Gatewaykonfiguration löschen möchten.

- Wenn Sie alles löschen und von Neuem anfangen möchten, wie im Fall einer Testumgebung, können Sie die Ressourcengruppe löschen. Wenn Sie eine Ressourcengruppe löschen, werden alle Ressourcen innerhalb der Gruppe gelöscht. Diese Vorgehensweise wird nur empfohlen, wenn Sie keine der Ressourcen aus der Ressourcengruppe beibehalten möchten. Sie können mit diesem Ansatz nicht selektiv nur ein paar Ressourcen löschen.

- Wenn Sie einige der Ressourcen aus der Ressourcengruppe beibehalten möchten, ist das Löschen eines Gateways des virtuellen Netzwerks etwas komplizierter. Bevor Sie das Gateway des virtuellen Netzwerks löschen können, müssen Sie zuerst alle Ressourcen löschen, die von dem Gateway abhängig sind. Welche Schritte Sie ausführen, hängt vom Typ der Verbindungen ab, die Sie erstellt haben, und den abhängigen Ressourcen für jede Verbindung.

## <a name="before-beginning"></a>Vorbereitungen

### <a name="1-download-the-latest-azure-resource-manager-powershell-cmdlets"></a>1. Laden Sie die aktuellen Azure Resource Manager-PowerShell-Cmdlets herunter.
Laden Sie die aktuelle Version der Azure Resource Manager-PowerShell-Cmdlets herunter, und installieren Sie sie. Weitere Informationen zum Herunterladen und Installieren der PowerShell-Cmdlets finden Sie unter [Get started with Azure PowerShell cmdlets](/powershell/azureps-cmdlets-docs) (Erste Schritte mit Azure PowerShell-Cmdlets).

### <a name="2-connect-to-your-azure-account"></a>2. Stellen Sie eine Verbindung mit Ihrem Azure-Konto her. 
Öffnen Sie die PowerShell-Konsole, und stellen Sie eine Verbindung mit Ihrem Konto her. Verwenden Sie das folgende Beispiel, um eine Verbindung herzustellen:

```powershell
Login-AzureRmAccount
```

Überprüfen Sie die Abonnements für das Konto.

```powershell
Get-AzureRmSubscription
```

Wenn Sie über mehrere Abonnements verfügen, geben Sie das Abonnement an, das Sie verwenden möchten.

```powershell
Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
```

## <a name="S2S"></a>Löschen eines Site-to-Site-VPN-Gateways

Um ein Gateway des virtuellen Netzwerks für eine S2S-Konfiguration zu löschen, müssen Sie zunächst jede Ressource löschen, die zu dem Gateway des virtuellen Netzwerks gehört. Ressourcen müssen aufgrund von Abhängigkeiten in einer bestimmten Reihenfolge gelöscht werden. Bei der Arbeit mit den folgenden Beispielen müssen einige der Werte angegeben werden, während andere Werte ein Ergebnis der Ausgabe sind. Wir verwenden die folgenden spezifischen Werte in den Beispielen zu Demonstrationszwecken:

VNet-Name: VNet1<br>
Ressourcengruppenname: RG1<br>
Name des Gateways des virtuellen Netzwerks: GW1<br>

Die folgenden Schritte gelten für das Resource Manager-Bereitstellungsmodell.

### <a name="1-get-the-virtual-network-gateway-that-you-want-to-delete"></a>1. Rufen Sie das Gateway des virtuellen Netzwerks ab, das Sie löschen möchten.

```powershell
$Gateway=get-azurermvirtualnetworkgateway -Name "GW1" -ResourceGroupName "RG1"
```

### <a name="2-check-to-see-if-the-virtual-network-gateway-has-any-connections"></a>2. Überprüfen Sie, ob das Gateway des virtuellen Netzwerks über Verbindungen verfügt.

```powershell
get-azurermvirtualnetworkgatewayconnection -ResourceGroupName "RG1" | where-object {$_.VirtualNetworkGateway1.Id -eq $GW.Id}
$Conns=get-azurermvirtualnetworkgatewayconnection -ResourceGroupName "RG1" | where-object {$_.VirtualNetworkGateway1.Id -eq $GW.Id}
```

### <a name="3-delete-all-connections"></a>3. Löschen Sie alle Verbindungen.
Sie werden möglicherweise aufgefordert, das Löschen der einzelnen Verbindungen zu bestätigen.

```powershell
$Conns | ForEach-Object {Remove-AzureRmVirtualNetworkGatewayConnection -Name $_.name -ResourceGroupName $_.ResourceGroupName}
```

### <a name="4-get-the-list-of-the-corresponding-local-network-gateways"></a>4. Rufen Sie die Liste der entsprechenden lokalen Netzwerkgateways ab.

```powershell
$LNG=Get-AzureRmLocalNetworkGateway -ResourceGroupName "RG1" | where-object {$_.Id -In $Conns.LocalNetworkGateway2.Id}
```
    
### <a name="5-delete-the-local-network-gateways"></a>5. Löschen Sie die lokalen Netzwerkgateways.
Sie werden möglicherweise aufgefordert, das Löschen der einzelnen lokalen Netzwerkgateways zu bestätigen.

```powershell
$LNG | ForEach-Object {Remove-AzureRmLocalNetworkGateway -Name $_.Name -ResourceGroupName $_.ResourceGroupName}
```

### <a name="6-delete-the-virtual-network-gateway"></a>6. Löschen Sie das Gateway des virtuellen Netzwerks.
Sie werden möglicherweise aufgefordert, das Löschen des Gateways zu bestätigen.

>[!NOTE]
> Wenn Sie neben der S2S-Konfiguration noch über eine P2S-Konfiguration für dieses VNET verfügen, werden automatisch alle P2S-Clients ohne Warnung getrennt, wenn Sie das Gateway des virtuellen Netzwerks löschen.
>
>

```powershell
Remove-AzureRmVirtualNetworkGateway -Name "GW1" -ResourceGroupName "RG1"
```

### <a name="7-get-the-ip-configurations-of-the-virtual-network-gateway"></a>7. Rufen Sie die IP-Konfigurationen des Gateways des virtuellen Netzwerks ab.

```powershell
$GWIpConfigs = $Gateway.IpConfigurations
```

### <a name="8-get-the-list-of-public-ip-addresses-used-for-this-virtual-network-gateway"></a>8. Rufen Sie die Liste der öffentlichen IP-Adressen ab, die für dieses Gateway des virtuellen Netzwerks verwendet werden. 
Wenn das Gateway des virtuellen Netzwerks aktiv/aktiv war, sehen Sie zwei öffentliche IP-Adressen.

```powershell
$PubIP=Get-AzureRmPublicIpAddress | where-object {$_.Id -In $GWIpConfigs.PublicIpAddress.Id}
```

### <a name="9-delete-the-public-ips"></a>9. Löschen Sie die öffentlichen IP-Adressen.

```powershell
$PubIP | foreach-object {remove-azurermpublicIpAddress -Name $_.Name -ResourceGroupName "RG1"}
```

### <a name="10-delete-the-gateway-subnet-and-set-the-configuration"></a>10. Löschen Sie das Gatewaysubnetz, und legen Sie die Konfiguration fest.

```powershell
$GWSub = Get-AzureRmVirtualNetwork -ResourceGroupName "RG1" -Name "VNet1" | Remove-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet"
Set-AzureRmVirtualNetwork -VirtualNetwork $GWSub
```

## <a name="v2v"></a>Löschen eines VNet-to-VNet-VPN-Gateways

Um ein Gateway des virtuellen Netzwerks für eine V2V-Konfiguration zu löschen, müssen Sie zunächst jede Ressource löschen, die zu dem Gateway des virtuellen Netzwerks gehört. Ressourcen müssen aufgrund von Abhängigkeiten in einer bestimmten Reihenfolge gelöscht werden. Bei der Arbeit mit den folgenden Beispielen müssen einige der Werte angegeben werden, während andere Werte ein Ergebnis der Ausgabe sind. Wir verwenden die folgenden spezifischen Werte in den Beispielen zu Demonstrationszwecken:

VNet-Name: VNet1<br>
Ressourcengruppenname: RG1<br>
Name des Gateways des virtuellen Netzwerks: GW1<br>

Die folgenden Schritte gelten für das Resource Manager-Bereitstellungsmodell.

### <a name="1-get-the-virtual-network-gateway-that-you-want-to-delete"></a>1. Rufen Sie das Gateway des virtuellen Netzwerks ab, das Sie löschen möchten.

```powershell
$Gateway=get-azurermvirtualnetworkgateway -Name "GW1" -ResourceGroupName "RG1"
```

### <a name="2-check-to-see-if-the-virtual-network-gateway-has-any-connections"></a>2. Überprüfen Sie, ob das Gateway des virtuellen Netzwerks über Verbindungen verfügt.

```powershell
get-azurermvirtualnetworkgatewayconnection -ResourceGroupName "RG1" | where-object {$_.VirtualNetworkGateway1.Id -eq $GW.Id}
```
 
Es gibt möglicherweise andere Verbindungen mit dem Gateway des virtuellen Netzwerks, die Teil einer anderen Ressourcengruppe sind. Überprüfen Sie in jeder zusätzlichen Ressourcengruppe, ob weitere Verbindungen bestehen. In diesem Beispiel überprüfen wir, ob Verbindungen mit RG2 bestehen. Führen Sie dies für jede Ihrer Ressourcengruppen durch, die möglicherweise mit dem Gateway des virtuellen Netzwerks verbunden ist.

```powershell
get-azurermvirtualnetworkgatewayconnection -ResourceGroupName "RG2" | where-object {$_.VirtualNetworkGateway2.Id -eq $GW.Id}
```

### <a name="3-get-the-list-of-connections-in-both-directions"></a>3. Rufen Sie die Liste der Verbindungen in beiden Richtungen ab.
Da dies eine VNet-to-VNet-Konfiguration ist, benötigen Sie die Liste der Verbindungen in beiden Richtungen.

```powershell
$ConnsL=get-azurermvirtualnetworkgatewayconnection -ResourceGroupName "RG1" | where-object {$_.VirtualNetworkGateway1.Id -eq $GW.Id}
```
 
In diesem Beispiel überprüfen wir, ob Verbindungen mit RG2 bestehen. Führen Sie dies für jede Ihrer Ressourcengruppen durch, die möglicherweise mit dem Gateway des virtuellen Netzwerks verbunden ist.

```powershell
 $ConnsR=get-azurermvirtualnetworkgatewayconnection -ResourceGroupName "<NameOfResourceGroup2>" | where-object {$_.VirtualNetworkGateway2.Id -eq $GW.Id}
 ```

### <a name="4-delete-all-connections"></a>4. Löschen Sie alle Verbindungen.
Sie werden möglicherweise aufgefordert, das Löschen der einzelnen Verbindungen zu bestätigen.

```powershell
$ConnsL | ForEach-Object {Remove-AzureRmVirtualNetworkGatewayConnection -Name $_.name -ResourceGroupName $_.ResourceGroupName}
$ConnsR | ForEach-Object {Remove-AzureRmVirtualNetworkGatewayConnection -Name $_.name -ResourceGroupName $_.ResourceGroupName}
```

### <a name="5-delete-the-virtual-network-gateway"></a>5. Löschen Sie das Gateway des virtuellen Netzwerks.
Sie werden möglicherweise aufgefordert, das Löschen des Gateways des virtuellen Netzwerks zu bestätigen.

>[!NOTE]
> Wenn Sie neben der V2V-Konfiguration noch über P2S-Konfigurationen für Ihre VNETs verfügen, werden automatisch alle P2S-Clients ohne Warnung getrennt, wenn Sie das Gateway des virtuellen Netzwerks löschen.
>
>


```powershell
Remove-AzureRmVirtualNetworkGateway -Name "GW1" -ResourceGroupName "RG1"
```

### <a name="6-get-the-ip-configurations-of-the-virtual-network-gateway"></a>6. Rufen Sie die IP-Konfigurationen des Gateways des virtuellen Netzwerks ab.

```powershell
$GWIpConfigs = $Gateway.IpConfigurations
```

### <a name="7-get-the-list-of-public-ip-addresses-used-for-this-virtual-network-gateway"></a>7. Rufen Sie die Liste der öffentlichen IP-Adressen ab, die für dieses Gateway des virtuellen Netzwerks verwendet werden. 
Wenn das Gateway des virtuellen Netzwerks aktiv/aktiv war, sehen Sie zwei öffentliche IP-Adressen.

```powershell
$PubIP=Get-AzureRmPublicIpAddress | where-object {$_.Id -In $GWIpConfigs.PublicIpAddress.Id}
```

### <a name="8-delete-the-public-ips"></a>8. Löschen Sie die öffentlichen IP-Adressen.
Sie werden möglicherweise aufgefordert, das Löschen der öffentlichen IP-Adresse zu bestätigen.

```powershell
$PubIP | foreach-object {remove-azurermpublicIpAddress -Name $_.Name -ResourceGroupName "<NameOfResourceGroup1>"}
```

### <a name="9-delete-the-gateway-subnet-and-set-the-configuration"></a>9. Löschen Sie das Gatewaysubnetz, und legen Sie die Konfiguration fest.

```powershell
$GWSub = Get-AzureRmVirtualNetwork -ResourceGroupName "RG1" -Name "VNet1" | Remove-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet"
Set-AzureRmVirtualNetwork -VirtualNetwork $GWSub
```

## <a name="deletep2s"></a>Löschen eines Punkt-zu-Site-VPN-Gateways

Um ein Gateway des virtuellen Netzwerks für eine P2S-Konfiguration zu löschen, müssen Sie zunächst jede Ressource löschen, die zu dem Gateway des virtuellen Netzwerks gehört. Ressourcen müssen aufgrund von Abhängigkeiten in einer bestimmten Reihenfolge gelöscht werden. Bei der Arbeit mit den folgenden Beispielen müssen einige der Werte angegeben werden, während andere Werte ein Ergebnis der Ausgabe sind. Wir verwenden die folgenden spezifischen Werte in den Beispielen zu Demonstrationszwecken:

VNet-Name: VNet1<br>
Ressourcengruppenname: RG1<br>
Name des Gateways des virtuellen Netzwerks: GW1<br>

Die folgenden Schritte gelten für das Resource Manager-Bereitstellungsmodell.


>[!NOTE]
> Wenn Sie das VPN-Gateway löschen, werden alle verbundenen Clients ohne Warnung vom VNET getrennt.
>
>

### <a name="1-get-the-virtual-network-gateway-that-you-want-to-delete"></a>1. Rufen Sie das Gateway des virtuellen Netzwerks ab, das Sie löschen möchten.

```powershell
$Gateway=get-azurermvirtualnetworkgateway -Name "GW1" -ResourceGroupName "RG1"
```

### <a name="2-delete-the-virtual-network-gateway"></a>2. Löschen Sie das Gateway des virtuellen Netzwerks.
Sie werden möglicherweise aufgefordert, das Löschen des Gateways des virtuellen Netzwerks zu bestätigen.

```powershell
Remove-AzureRmVirtualNetworkGateway -Name "GW1" -ResourceGroupName "RG1"
```

### <a name="3-get-the-ip-configurations-of-the-virtual-network-gateway"></a>3. Rufen Sie die IP-Konfigurationen des Gateways des virtuellen Netzwerks ab.

```powershell
$GWIpConfigs = $Gateway.IpConfigurations
```

### <a name="4-get-the-list-of-public-ip-addresses-used-for-this-virtual-network-gateway"></a>4. Rufen Sie die Liste der öffentlichen IP-Adressen ab, die für dieses Gateway des virtuellen Netzwerks verwendet werden. 
Wenn das Gateway des virtuellen Netzwerks aktiv/aktiv war, sehen Sie zwei öffentliche IP-Adressen.

```powershell
$PubIP=Get-AzureRmPublicIpAddress | where-object {$_.Id -In $GWIpConfigs.PublicIpAddress.Id}
```

### <a name="5-delete-the-public-ips"></a>5. Löschen Sie die öffentlichen IP-Adressen.
Sie werden möglicherweise aufgefordert, das Löschen der öffentlichen IP-Adresse zu bestätigen.

```powershell
$PubIP | foreach-object {remove-azurermpublicIpAddress -Name $_.Name -ResourceGroupName "<NameOfResourceGroup1>"}
```

### <a name="6-delete-the-gateway-subnet-and-set-the-configuration"></a>6. Löschen Sie das Gatewaysubnetz, und legen Sie die Konfiguration fest.

```powershell
$GWSub = Get-AzureRmVirtualNetwork -ResourceGroupName "RG1" -Name "VNet1" | Remove-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet"
Set-AzureRmVirtualNetwork -VirtualNetwork $GWSub
```

## <a name="delete"></a>Löschen eines VPN-Gateways durch Löschen der Ressourcengruppe

Wenn Sie Ihre bestehenden Ressourcen in der Ressourcengruppe nicht behalten müssen und neu beginnen möchten, können Sie eine gesamte Ressourcengruppe löschen. Sie lernen hier eine schnelle Möglichkeit kennen, alles zu entfernen. Die folgenden Schritte gelten für das Resource Manager-Bereitstellungsmodell.

### <a name="1-get-a-list-of-all-the-resource-groups-in-your-subscription"></a>1. Rufen Sie eine Liste aller Ressourcengruppen in Ihrem Abonnement auf.

```powershell
Get-AzureRmResourceGroup
```

### <a name="2-locate-the-resource-group-that-you-want-to-delete"></a>2. Wählen Sie als Nächstes die Ressourcengruppe aus, die Sie löschen möchten.
Wählen Sie die Ressourcengruppe aus, die Sie löschen möchten, und zeigen Sie eine Liste der in der Gruppe enthaltenen Ressourcen an. Die Ressourcengruppe im Beispiel hat den Namen „RG1“. Ändern Sie das Beispiel zum Abrufen einer Liste aller Ressourcen.

```powershell
Find-AzureRmResource -ResourceGroupNameContains RG1
```

### <a name="3-verify-the-resources-in-the-list"></a>3. Überprüfen Sie die Ressourcen in der Liste.
Wenn die Liste zurückgegeben wird, sehen Sie sie durch, um sicherzustellen, dass Sie sowohl alle Ressourcen in der Ressourcengruppe als auch die Ressourcengruppe selbst löschen möchten. Wenn Sie einige der Ressourcen in der Ressourcengruppe beibehalten möchten, führen Sie die Schritte in den früheren Abschnitten dieses Artikels aus, um Ihr Gateway zu löschen.

### <a name="4-delete-the-resource-group-and-resources"></a>4. Löschen Sie die Ressourcengruppe und die Ressourcen.
Um die Ressourcengruppe und alle darin enthaltenen Ressourcen zu löschen, ändern Sie das Beispiel, und führen Sie es aus.

```powershell
Remove-AzureRmResourceGroup -Name RG1
```

### <a name="5-check-the-status"></a>5. Überprüfen Sie den Status.
Azure benötigt etwas Zeit, um alle Ressourcen zu löschen. Sie können den Status der Ressourcengruppe mit diesem Cmdlet überprüfen.

```powershell
Get-AzureRmResourceGroup -ResourceGroupName RG1
```

Als Ergebnis wird „Erfolgreich“ angezeigt.

    ResourceGroupName : RG1
    Location          : eastus
    ProvisioningState : Succeeded
