---
title: 'Herstellen einer Verbindung zwischen Ihrem lokalen Netzwerk und einem virtuellen Azure-Netzwerk: Site-to-Site-VPN: CLI | Microsoft-Dokumentation'
description: "Schritte zum Erstellen einer IPsec-Verbindung zwischen Ihrem lokalen Netzwerk und einem virtuellen Azure-Netzwerk über das öffentliche Internet. Anhand dieser Schritte können Sie mithilfe einer CLI eine standortübergreifende Site-to-Site-VPN Gateway-Verbindung erstellen."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/24/2017
ms.author: cherylmc
ms.translationtype: Human Translation
ms.sourcegitcommit: 54b5b8d0040dc30651a98b3f0d02f5374bf2f873
ms.openlocfilehash: af85e4921a2b81c71f1d132c6df591acbe5d3764
ms.contentlocale: de-de
ms.lasthandoff: 04/28/2017


---
# <a name="create-a-virtual-network-with-a-site-to-site-vpn-connection-using-cli"></a>Erstellen eines virtuellen Netzwerks mit einer Site-to-Site-VPN-Verbindung per CLI

In diesem Artikel wird beschrieben, wie Sie die Azure-CLI zum Erstellen einer Site-to-Site-VPN Gateway-Verbindung aus Ihrem lokalen Netzwerk mit dem VNet verwenden. Die Schritte in diesem Artikel gelten für das Resource Manager-Bereitstellungsmodell. Sie können diese Konfiguration auch mit einem anderen Bereitstellungstool oder -modell erstellen. Wählen Sie hierzu in der folgenden Liste eine andere Option:<br>

> [!div class="op_single_selector"]
> * [Resource Manager – Azure-Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [Resource Manager – PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [Resource Manager – Befehlszeilenschnittstelle](vpn-gateway-howto-site-to-site-resource-manager-cli.md)
> * [Klassisch – Azure-Portal](vpn-gateway-howto-site-to-site-classic-portal.md)
> * [Klassisch – Klassisches Portal](vpn-gateway-site-to-site-create.md)
> 
>


![Diagramm für die standortübergreifende Site-to-Site-VPN Gateway-Verbindung](./media/vpn-gateway-howto-site-to-site-resource-manager-cli/site-to-site-connection-diagram.png)

Eine Site-to-Site-VPN-Gateway-Verbindung wird verwendet, um Ihr lokales Netzwerk über einen IPsec/IKE-VPN-Tunnel (IKEv1 oder IKEv2) mit einem virtuellen Azure-Netzwerk zu verbinden. Für diese Art von Verbindung wird ein lokales VPN-Gerät benötigt, dem eine extern zugängliche, öffentliche IP-Adresse zugewiesen ist. Weitere Informationen zu VPN-Gateways finden Sie unter [Informationen zu VPN Gateway](vpn-gateway-about-vpngateways.md).

## <a name="before-you-begin"></a>Voraussetzungen

Vergewissern Sie sich vor Beginn der Konfiguration, dass die folgenden Voraussetzungen erfüllt sind bzw. Folgendes vorhanden ist:

* Sie sind sicher, dass Sie das Resource Manager-Bereitstellungsmodell verwenden möchten. [!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 
* Ein kompatibles VPN-Gerät (und eine Person, die es konfigurieren kann). Weitere Informationen zu kompatiblen VPN-Geräten und zur Gerätekonfiguration finden Sie unter [Informationen zu VPN-Geräten](vpn-gateway-about-vpn-devices.md).
* Eine extern zugängliche, öffentliche IPv4-Adresse für Ihr VPN-Gerät. Diese IP-Adresse darf sich nicht hinter einer NAT befinden.
* Falls Sie nicht mit den IP-Adressbereichen in Ihrer lokalen Netzwerkkonfiguration vertraut sind, wenden Sie sich an eine Person, die Ihnen diese Informationen zur Verfügung stellen kann. Beim Erstellen dieser Konfiguration müssen Sie die Präfixe für die IP-Adressbereiche angeben, die Azure an Ihren lokalen Standort weiterleitet. Kein Subnetz Ihres lokalen Netzwerks darf sich mit den Subnetzen des virtuellen Netzwerks überlappen, mit dem Sie eine Verbindung herstellen möchten. 
* Die aktuelle Version der CLI-Befehle (2.0 oder höher). Informationen zum Installieren der CLI-Befehle finden Sie unter [Install Azure CLI 2.0](/cli/azure/install-azure-cli) (Installieren der Azure CLI 2.) und [Get Started with Azure CLI 2.0](/cli/azure/get-started-with-azure-cli) (Erste Schritte mit der Azure CLI 2.0).

### <a name="example-values"></a>Beispielwerte

Sie können die folgenden Werte zum Erstellen einer Testumgebung oder zum besseren Verständnis der Beispiele in diesem Artikel nutzen:

```
#Example values

VnetName                = TestVNet1 
ResourceGroup           = TestRG1 
Location                = eastus 
AddressSpace            = 10.12.0.0/16 
SubnetName              = Subnet1 
Subnet                  = 10.12.0.0/24 
GatewaySubnet           = 10.12.255.0/27 
LocalNetworkGatewayName = Site2 
LNG Public IP           = <VPN device IP address>
LocalAddrPrefix1        = 10.0.0.0/24 
LocalAddrPrefix2        = 20.0.0.0/24   
GatewayName             = VNet1GW 
PublicIP                = VNet1GWIP 
VPNType                 = RouteBased 
GatewayType             = Vpn 
ConnectionName          = VNet1toSite2
```

## <a name="Login"></a>1. Verbinden mit Ihrem Abonnement

[!INCLUDE [CLI login](../../includes/vpn-gateway-cli-login-include.md)]

## <a name="2-create-a-resource-group"></a>2. Erstellen einer Ressourcengruppe

Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen „TestRG1“ am Standort „eastus“ erstellt. Falls Sie in dieser Region bereits über eine Ressourcengruppe verfügen, die Sie zum Erstellen des VNet nutzen möchten, können Sie sie verwenden.

```azurecli
az group create --name TestRG1 --location eastus
```

## <a name="VNet"></a>3. Erstellen eines virtuellen Netzwerks

Falls Sie noch kein virtuelles Netzwerk besitzen, erstellen Sie eins mit dem Befehl [az network vnet create](/cli/azure/network/vnet#create). Stellen Sie beim Erstellen eines virtuellen Netzwerks sicher, dass sich die angegebenen Adressräume und die Adressräume im lokalen Netzwerk nicht überlappen. 

Im folgenden Beispiel werden ein virtuelles Netzwerk mit dem Namen „TestVNet1“ und ein Subnetz mit dem Namen „Subnet1“ erstellt.

```azurecli
az network vnet create --name TestVNet1 --resource-group TestRG1 --address-prefix 10.12.0.0/16 --location eastus --subnet-name Subnet1 --subnet-prefix 10.12.0.0/24
```

## 4. <a name="gwsub"></a>Erstellen des Gatewaysubnetzes

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

Bei dieser Konfiguration benötigen Sie auch ein Gatewaysubnetz. Für das virtuelle Netzwerkgateway wird ein Gatewaysubnetz verwendet, das die von den VPN-Gatewaydiensten genutzten IP-Adressen enthält. Wenn Sie ein Gatewaysubnetz erstellen, muss es den Namen „GatewaySubnet“ erhalten. Wenn Sie einen anderen Namen verwenden, erstellen Sie zwar ein Subnetz, aber es wird von Azure nicht wie ein Gatewaysubnetz behandelt.

Die Größe des von Ihnen angegebenen Gatewaysubnetzes richtet sich nach der VPN-Gatewaykonfiguration, die Sie erstellen möchten. Es ist zwar möglich, ein Gatewaysubnetz mit einer Größe von nur /29 zu erstellen, es wird jedoch empfohlen, /27 oder /28 auszuwählen, um ein größeres Subnetz mit mehr Adressen zu erstellen. Die Verwendung eines größeren Gatewaysubnetzes ermöglicht die Vergabe einer ausreichenden Zahl von IP-Adressen für potenzielle zukünftige Konfigurationen.

Erstellen Sie das Gatewaysubnetz mit dem Befehl [az network vnet subnet create](/cli/azure/network/vnet/subnet#create).


```azurecli
az network vnet subnet create --address-prefix 10.12.255.0/27 --name GatewaySubnet --resource-group TestRG1 --vnet-name TestVNet1
```

## <a name="localnet"></a>5. Erstellen des Gateways des lokalen Netzwerks

Mit dem Gateway des lokalen Netzwerks ist normalerweise Ihr lokaler Standort gemeint. Sie geben dem Standort einen Namen, über den Azure darauf verweisen kann, und geben dann die IP-Adresse des lokalen VPN-Geräts an, mit dem Sie eine Verbindung herstellen. Außerdem geben Sie die IP-Adresspräfixe an, die über das VPN-Gateway an das VPN-Gerät weitergeleitet werden. Die von Ihnen angegebenen Adresspräfixe befinden sich in Ihrem lokalen Netzwerk. Sie können diese Präfixe leicht aktualisieren, wenn sich Ihr lokales Netzwerk ändert.

Verwenden Sie die folgenden Werte:

* *--gateway-ip-address* ist die IP-Adresse Ihres lokalen VPN-Geräts. Das VPN-Gerät darf sich nicht hinter einer NAT befinden.
* *--local-address-prefixes* steht für Ihre lokalen Adressräume.

Fügen Sie mit dem Befehl [az network local-gateway create](/cli/azure/network/local-gateway#create) ein Gateway des lokalen Netzwerks mit mehreren Adresspräfixen hinzu:

```azurecli
az network local-gateway create --gateway-ip-address 23.99.221.164 --name Site2 --resource-group TestRG1 --local-address-prefixes 10.0.0.0/24 20.0.0.0/24
```

## <a name="PublicIP"></a>6. Anfordern einer öffentlichen IP-Adresse

Ein VPN-Gateway muss über eine öffentliche IP-Adresse verfügen. Sie fordern zuerst die IP-Adressressource an und verweisen dann beim Erstellen des Gateways des virtuellen Netzwerks darauf. Die IP-Adresse wird bei der Erstellung des VPN-Gateways der Ressource dynamisch zugewiesen. VPN Gateway unterstützt derzeit nur die *dynamische* Zuweisung öffentlicher IP-Adressen. Die Zuweisung einer statischen öffentlichen IP-Adresse kann nicht angefordert werden. Das bedeutet jedoch nicht, dass sich die IP-Adresse ändert, nachdem sie Ihrem VPN-Gateway zugewiesen wurde. Die öffentliche IP-Adresse ändert sich nur, wenn das Gateway gelöscht und neu erstellt wird. Sie ändert sich nicht, wenn die Größe geändert wird, das VPN-Gateway zurückgesetzt wird oder andere interne Wartungs-/Upgradevorgänge für das VPN-Gateway durchgeführt werden.

Fordern Sie mit dem Befehl [az network public-ip create](/cli/azure/network/public-ip#create) eine dynamische öffentliche IP-Adresse an.

```azurecli
az network public-ip create --name VNet1GWIP --resource-group TestRG1 --allocation-method Dynamic
```

## <a name="CreateGateway"></a>7. Erstellen des VPN-Gateways

Erstellen Sie das VPN-Gateway für das virtuelle Netzwerk. Die Erstellung eines VPN-Gateways kann bis zu 45 Minuten dauern.

Verwenden Sie die folgenden Werte:

* *--gateway-type* für eine Site-to-Site-Konfiguration lautet *Vpn*. Der Gatewaytyp ist immer spezifisch für die Konfiguration, die Sie implementieren. Weitere Informationen finden Sie unter [Gatewaytypen](vpn-gateway-about-vpn-gateway-settings.md#gwtype).
* *--vpn-type* kann *RouteBased* (in einigen Dokumentationen als dynamisches Gateway bezeichnet) oder *PolicyBased* lauten (in einigen Dokumentationen als statisches Gateway bezeichnet). Die Einstellung richtet sich nach den Anforderungen des Geräts, mit dem Sie eine Verbindung herstellen. Weitere Informationen zu VPN-Gatewaytypen finden Sie unter [Informationen zu VPN Gateway-Einstellungen](vpn-gateway-about-vpn-gateway-settings.md#vpntype).
* *--sku* kann auf „Basic“, „Standard“ oder „HighPerformance“ festgelegt werden. Für bestimmte SKUs gelten Einschränkungen bei der Konfiguration. Weitere Informationen finden Sie unter [Gateway-SKUs](vpn-gateway-about-vpngateways.md#gateway-skus).

Erstellen Sie das VPN-Gateway mit dem Befehl [az network vnet-gateway create](/cli/azure/network/vnet-gateway#create). Wenn Sie diesen Befehl mit dem Parameter „--no-wait“ ausführen, wird kein Feedback bzw. keine Ausgabe angezeigt. Dieser Parameter ermöglicht die Erstellung des Gateways im Hintergrund. Die Erstellung eines Gateways dauert ca. 45 Minuten.

```azurecli
az network vnet-gateway create --name VNet1GW --public-ip-address VNet1GWIP --resource-group TestRG1 --vnet TestVNet1 --gateway-type Vpn --vpn-type RouteBased --sku Standard --no-wait 
```

## <a name="VPNDevice"></a>8. Konfigurieren des VPN-Geräts

[!INCLUDE [Configure VPN device](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]
 Verwenden Sie den Befehl [az network public-ip list](/cli/azure/network/public-ip#list), um die öffentliche IP-Adresse des Gateways des virtuellen Netzwerks zu ermitteln. Zur besseren Lesbarkeit wird die Ausgabe formatiert, damit die Liste mit den öffentlichen IP-Adressen in Tabellenform angezeigt wird.

```azurecli
az network public-ip list --resource-group TestRG1 --output table
```

## <a name="CreateConnection"></a>9. Erstellen der VPN-Verbindung

Erstellen Sie die Site-to-Site-VPN-Verbindung zwischen dem Gateway Ihres virtuellen Netzwerks und Ihrem lokalen VPN-Gerät. Achten Sie besonders auf den Wert des gemeinsam verwendeten Schlüssels. Er muss mit dem konfigurierten Wert des gemeinsam verwendeten Schlüssels für Ihr VPN-Gerät übereinstimmen.

Erstellen Sie die Verbindung mit dem Befehl [az network vpn-connection create](/cli/azure/network/vpn-connection#create).

```azurecli
az network vpn-connection create --name VNet1toSite2 -resource-group TestRG1 --vnet-gateway1 VNet1GW -l eastus --shared-key abc123 --local-gateway2 Site2
```

Die Verbindung wird nach kurzer Zeit hergestellt.

## <a name="toverify"></a>10. Überprüfen der VPN-Verbindung

[!INCLUDE [verify connection](../../includes/vpn-gateway-verify-connection-cli-rm-include.md)] 

Falls Sie ein anderes Verfahren zum Überprüfen Ihrer Verbindung verwenden möchten, helfen Ihnen die Informationen unter [Überprüfen einer VPN Gateway-Verbindung](vpn-gateway-verify-connection-resource-manager.md) weiter.

## <a name="common-tasks"></a>Häufige Aufgaben

Dieser Abschnitt enthält allgemeine Befehle, die beim Arbeiten mit Site-to-Site-Konfigurationen hilfreich sind. Die vollständige Liste der CLI-Netzwerkbefehle finden Sie unter [Networking - az network](/cli/azure/network) (Netzwerke – az network).

[!INCLUDE [local network gateway common tasks](../../includes/vpn-gateway-common-tasks-cli-include.md)] 

## <a name="next-steps"></a>Nächste Schritte

*  Sobald die Verbindung hergestellt ist, können Sie Ihren virtuellen Netzwerken virtuelle Computer hinzufügen. Weitere Informationen finden Sie unter [Virtuelle Computer](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) .
* Informationen zu BGP finden Sie in der [Übersicht über BGP](vpn-gateway-bgp-overview.md) und unter [Konfigurieren von BGP](vpn-gateway-bgp-resource-manager-ps.md).
* Weitere Informationen zur Tunnelerzwingung finden Sie unter [Konfigurieren der Tunnelerzwingung](vpn-gateway-forced-tunneling-rm.md).
* Eine Liste mit Azure-CLI-Netzwerkbefehlen finden Sie unter [Azure-CLI](https://docs.microsoft.com/cli/azure/network).
