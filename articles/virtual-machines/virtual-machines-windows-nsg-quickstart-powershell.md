<properties
   pageTitle="Zulassen des externen Zugriffs auf eine VM mithilfe von PowerShell | Microsoft Azure"
   description="Erfahren Sie, wie Sie einen Port öffnen oder Endpunkt erstellen, der mit dem Resource Manager-Bereitstellungsmodell und Azure PowerShell den Zugriff auf Ihren virtuellen Windows-Computer ermöglicht."
   services="virtual-machines-windows"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="05/24/2016"
   ms.author="iainfou"/>

# Zulassen des externen Zugriffs auf eine VM mithilfe von PowerShell
[AZURE.INCLUDE [virtual-machines-common-nsg-quickstart](../../includes/virtual-machines-common-nsg-quickstart.md)]

## Schnellbefehle
Zum Erstellen einer Netzwerksicherheitsgruppe und von ACL-Regeln (Access Control Lists, Zugriffssteuerungslisten) müssen Sie [die neueste Version von Azure PowerShell installieren](../powershell-install-configure.md). Sie können [diese Schritte auch über das Azure-Portal ausführen](virtual-machines-windows-nsg-quickstart-portal.md).

Zunächst müssen Sie eine Regel erstellen, die HTTP-Datenverkehr am TCP-Port 80 zulässt, wie folgt erstellen, wobei Sie Ihren eigenen Namen und eine Beschreibung eingeben:

```
$httprule = New-AzureRmNetworkSecurityRuleConfig -Name http-rule -Description "Allow HTTP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 100 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 80
```

Als Nächstes erstellen Sie die Netzwerksicherheitsgruppe und weisen die zuvor erstellte HTTP-Regel zu, wobei Sie den Namen und die Region Ihrer Ressourcengruppe eingeben:

```
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName TestRG -Location westus 
    -Name "TestNSG" -SecurityRules $httprule
```

Lassen Sie uns nun Ihre Netzwerksicherheitsgruppe einem Subnetz zuweisen. Wählen Sie zuerst das virtuelle Netzwerk wie folgt aus:

```
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
```

Ordnen Sie die Netzwerksicherheitsgruppe Ihrem Subnetz wie folgt zu:

```
Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name TestSubnet `
    -NetworkSecurityGroup $nsg
```

Aktualisieren Sie abschließend Ihr virtuelles Netzwerk, damit die Änderungen wirksam werden:

```
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```


## Weitere Informationen zu Netzwerksicherheitsgruppen
Mit diesen Schnellbefehlen können Sie den Datenverkehr zu Ihrem virtuellen Computer einrichten. Netzwerksicherheitsgruppen bieten viele herausragende Funktionen sowie Granularität zum Steuern von Ressourcenzugriffen. Weitere Informationen über die [Erstellung von Netzwerksicherheitsgruppen und ACL-Regeln erhalten Sie hier](../virtual-network/virtual-networks-create-nsg-arm-ps.md).

Netzwerksicherheitsgruppen und ACL-Regeln können auch als Teil von Azure Resource Manager-Vorlagen definiert werden. Erfahren Sie mehr über [das Erstellen von Netzwerksicherheitsgruppen mit Vorlagen](../virtual-network/virtual-networks-create-nsg-arm-template.md).

Wenn eine Portweiterleitung notwendig ist, um einem internen Port des virtuellen Computers einen eindeutigen externen Port zuzuweisen, müssen ein Lastenausgleich sowie NAT-Regeln (Network Address Translation, Netzwerkadressübersetzung) verwendet werden. Sie können beispielsweise TCP-Port 8080 extern verfügbar machen und den Datenverkehr an TCP-Port 80 auf einem virtuellen Computer weiterleiten. Hier finden Sie weitere Informationen zum [Erstellen eines Load Balancers mit Internetzugriff](../load-balancer/load-balancer-get-started-internet-arm-ps.md).

## Nächste Schritte
In diesem Beispiel haben Sie eine einfache Regel erstellt, die HTTP-Datenverkehr zulässt. Weitere Informationen zum Erstellen detaillierterer Umgebungen finden Sie in den folgenden Artikeln:

- [Übersicht über den Azure-Ressourcen-Manager](../resource-group-overview.md)
- [Was ist eine Netzwerksicherheitsgruppe (NSG)?](../virtual-network/virtual-networks-nsg.md)
- [Übersicht über den Azure Resource Manager für Load Balancer](../load-balancer/load-balancer-arm.md)

<!---HONumber=AcomDC_0601_2016-->