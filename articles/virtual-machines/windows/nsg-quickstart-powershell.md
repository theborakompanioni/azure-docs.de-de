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
ms.date: 08/21/2017
ms.author: iainfou
ms.translationtype: HT
ms.sourcegitcommit: cf381b43b174a104e5709ff7ce27d248a0dfdbea
ms.openlocfilehash: e818e3b3c707e1471d6f580f8379a277d3575b89
ms.contentlocale: de-de
ms.lasthandoff: 08/23/2017

---
# <a name="how-to-open-ports-and-endpoints-to-a-vm-in-azure-using-powershell"></a>Öffnen von Ports und Endpunkten für einen virtuellen Computer in Azure mit PowerShell
[!INCLUDE [virtual-machines-common-nsg-quickstart](../../../includes/virtual-machines-common-nsg-quickstart.md)]

## <a name="quick-commands"></a>Schnellbefehle
Zum Erstellen einer Netzwerksicherheitsgruppe und von ACL-Regeln (Access Control Lists, Zugriffssteuerungslisten) müssen Sie [die neueste Version von Azure PowerShell installieren](/powershell/azureps-cmdlets-docs). Sie können [diese Schritte auch über das Azure-Portal ausführen](nsg-quickstart-portal.md).

Melden Sie sich bei Ihrem Azure-Konto an:

```powershell
Login-AzureRmAccount
```

Ersetzen Sie in den folgenden Beispielen die Beispielparameternamen durch Ihre eigenen Werte. Beispielparameternamen sind etwa *myResourceGroup*, *myNetworkSecurityGroup* oder *myVnet*.

Erstellen Sie eine Regel mit [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig). Im folgenden Beispiel wird die Regel *myNetworkSecurityGroupRule* erstellt, um *TCP*-Datenverkehr an Port *80* zuzulassen:

```powershell
$httprule = New-AzureRmNetworkSecurityRuleConfig `
    -Name "myNetworkSecurityGroupRule" `
    -Description "Allow HTTP" `
    -Access "Allow" `
    -Protocol "Tcp" `
    -Direction "Inbound" `
    -Priority "100" `
    -SourceAddressPrefix "Internet" `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 80
```

Als Nächstes erstellen Sie die Netzwerksicherheitsgruppe mit [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) und weisen die zuvor erstellte HTTP-Regel wie folgt zu. Im folgenden Beispiel wird die Netzwerksicherheitsgruppe *myNetworkSecurityGroup* erstellt:

```powershell
$nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName "myResourceGroup" `
    -Location "EastUS" `
    -Name "myNetworkSecurityGroup" `
    -SecurityRules $httprule
```

Nun weisen Sie Ihre Netzwerksicherheitsgruppe einem Subnetz zu. Das folgende Beispiel weist der Variablen *$vnet* ein vorhandenes virtuelles Netzwerk namens *myVnet* mit [Get-AzureRmVirtualNetwork](/powershell/module/azurerm.network/get-azurermvirtualnetwork) zu:

```powershell
$vnet = Get-AzureRmVirtualNetwork `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVnet"
```

Weisen Sie mit [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) die Netzwerksicherheitsgruppe Ihrem Subnetz zu. Im folgenden Beispiel wird das Subnetz *mySubnet* Ihrer Netzwerksicherheitsgruppe zugewiesen:

```powershell
$subnetPrefix = $vnet.Subnets|?{$_.Name -eq 'mySubnet'}

Set-AzureRmVirtualNetworkSubnetConfig `
    -VirtualNetwork $vnet `
    -Name "mySubnet" `
    -AddressPrefix $subnetPrefix.AddressPrefix `
    -NetworkSecurityGroup $nsg
```

Aktualisieren Sie abschließend Ihr virtuelles Netzwerk mit [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork), damit die Änderungen wirksam werden:

```powershell
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```


## <a name="more-information-on-network-security-groups"></a>Weitere Informationen zu Netzwerksicherheitsgruppen
Mit diesen Schnellbefehlen können Sie den Datenverkehr zu Ihrem virtuellen Computer einrichten. Netzwerksicherheitsgruppen bieten eine Vielzahl erstklassiger Funktionen sowie eine differenzierte Steuerung des Ressourcenzugriffs. Weitere Informationen über die [Erstellung von Netzwerksicherheitsgruppen und ACL-Regeln erhalten Sie hier](tutorial-virtual-network.md#manage-internal-traffic).

Bei hoch verfügbaren Webanwendungen sollten Sie die virtuellen Computer hinter einem Azure Load Balancer anordnen. Der Load Balancer verteilt den Datenverkehr auf virtuelle Computer, mit einer Netzwerksicherheitsgruppe, die den Datenverkehr filtert. Weitere Informationen finden Sie unter [Lastenausgleich für virtuelle Linux-Computer in Azure zum Erstellen einer hoch verfügbaren Anwendung](tutorial-load-balancer.md).

## <a name="next-steps"></a>Nächste Schritte
In diesem Beispiel haben Sie eine einfache Regel erstellt, die HTTP-Datenverkehr zulässt. Informationen zum Erstellen von detaillierteren Umgebungen finden Sie in den folgenden Artikeln:

* [Übersicht über den Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md)
* [Was ist eine Netzwerksicherheitsgruppe (NSG)?](../../virtual-network/virtual-networks-nsg.md)
* [Übersicht über Azure Resource Manager für Load Balancer](../../load-balancer/load-balancer-arm.md)


