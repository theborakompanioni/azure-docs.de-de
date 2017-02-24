---
title: "Öffnen von Ports für einen virtuellen Computer mithilfe von Azure PowerShell | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie mit dem Azure Resource Manager-Bereitstellungsmodell und Azure PowerShell für Ihren virtuellen Windows-Computer einen Port öffnen oder einen Endpunkt erstellen."
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: cf45f7d8-451a-48ab-8419-730366d54f1e
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 02/09/2017
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: 91c2e6f53f8ba490d2943a60c7ad519d856707b9
ms.openlocfilehash: 0d57a27c0862b09e2628520a1e50a0cc56a55881


---
# <a name="opening-ports-and-endpoints-to-a-vm-in-azure-using-powershell"></a>Öffnen von Ports und Endpunkten für einen virtuellen Computer in Azure mithilfe von PowerShell
[!INCLUDE [virtual-machines-common-nsg-quickstart](../../includes/virtual-machines-common-nsg-quickstart.md)]

## <a name="quick-commands"></a>Schnellbefehle
Zum Erstellen einer Netzwerksicherheitsgruppe und von ACL-Regeln (Access Control Lists, Zugriffssteuerungslisten) müssen Sie [die neueste Version von Azure PowerShell installieren](/powershell/azureps-cmdlets-docs). Sie können [diese Schritte auch über das Azure-Portal ausführen](virtual-machines-windows-nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Melden Sie sich bei Ihrem Azure-Konto an:

```powershell
Login-AzureRmAccount
```

Ersetzen Sie in den folgenden Beispielen die Beispielparameternamen durch Ihre eigenen Werte. Zu den Beispielparameternamen zählen `myResourceGroup`, `myNetworkSecurityGroup` und `myVnet`.

Erstellen Sie eine Regel. Im folgenden Beispiel wird eine Regel namens `myNetworkSecurityGroupRule` erstellt, um TCP-Datenverkehr an Port 80 zuzulassen:

```powershell
$httprule = New-AzureRmNetworkSecurityRuleConfig -Name "myNetworkSecurityGroupRule" `
    -Description "Allow HTTP" -Access "Allow" -Protocol "Tcp" -Direction "Inbound" `
    -Priority "100" -SourceAddressPrefix "Internet" -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 80
```

Als Nächstes erstellen Sie die Netzwerksicherheitsgruppe und weisen die zuvor erstellte HTTP-Regel zu: Im folgenden Beispiel wird eine Netzwerksicherheitsgruppe namens `myNetworkSecurityGroup` erstellt:

```powershell
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "myNetworkSecurityGroup" -SecurityRules $httprule
```

Nun weisen Sie Ihre Netzwerksicherheitsgruppe einem Subnetz zu. Das folgende Beispiel weist der Variablen `$vnet` ein vorhandenes virtuelles Netzwerk namens `myVnet` zu:

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName "myResourceGroup" `
    -Name "myVnet"
```

Ordnen Sie die Netzwerksicherheitsgruppe Ihrem Subnetz zu. Im folgenden Beispiel wird das Subnetz mit dem Namen `mySubnet` Ihrer Netzwerksicherheitsgruppe zugewiesen:

```powershell
$subnetPrefix = $vnet.Subnets|?{$_.Name -eq 'mySubnet'}

Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "mySubnet" `
    -AddressPrefix $subnetPrefix.AddressPrefix `
    -NetworkSecurityGroup $nsg
```

Aktualisieren Sie abschließend Ihr virtuelles Netzwerk, damit die Änderungen wirksam werden:

```powershell
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```


## <a name="more-information-on-network-security-groups"></a>Weitere Informationen zu Netzwerksicherheitsgruppen
Mit diesen Schnellbefehlen können Sie den Datenverkehr zu Ihrem virtuellen Computer einrichten. Netzwerksicherheitsgruppen bieten eine Vielzahl erstklassiger Funktionen sowie eine differenzierte Steuerung des Ressourcenzugriffs. Weitere Informationen über die [Erstellung von Netzwerksicherheitsgruppen und ACL-Regeln erhalten Sie hier](../virtual-network/virtual-networks-create-nsg-arm-ps.md).

Sie können Netzwerksicherheitsgruppen und ACL-Regeln auch als Teil von Azure Resource Manager-Vorlagen definieren. Erfahren Sie mehr über [das Erstellen von Netzwerksicherheitsgruppen mit Vorlagen](../virtual-network/virtual-networks-create-nsg-arm-template.md).

Wenn eine Portweiterleitung notwendig ist, um einem internen Port des virtuellen Computers einen eindeutigen externen Port zuzuweisen, verwenden Sie einen Lastenausgleich sowie NAT-Regeln (Network Address Translation, Netzwerkadressübersetzung). Sie können beispielsweise TCP-Port 8080 extern verfügbar machen und den Datenverkehr an TCP-Port 80 auf einem virtuellen Computer weiterleiten. Hier finden Sie weitere Informationen zum [Erstellen eines Load Balancers mit Internetzugriff](../load-balancer/load-balancer-get-started-internet-arm-ps.md).

## <a name="next-steps"></a>Nächste Schritte
In diesem Beispiel haben Sie eine einfache Regel erstellt, die HTTP-Datenverkehr zulässt. Informationen zum Erstellen von detaillierteren Umgebungen finden Sie in den folgenden Artikeln:

* [Übersicht über den Azure Resource Manager](../azure-resource-manager/resource-group-overview.md)
* [Was ist eine Netzwerksicherheitsgruppe (NSG)?](../virtual-network/virtual-networks-nsg.md)
* [Übersicht über Azure Resource Manager für Load Balancer](../load-balancer/load-balancer-arm.md)




<!--HONumber=Feb17_HO2-->


