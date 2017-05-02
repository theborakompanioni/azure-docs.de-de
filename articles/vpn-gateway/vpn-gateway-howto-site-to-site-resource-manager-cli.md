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
ms.date: 04/21/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 1cc1ee946d8eb2214fd05701b495bbce6d471a49
ms.openlocfilehash: c3563f3a3fa46d40ba02fe97b3b0ebe3c45caddd
ms.lasthandoff: 04/26/2017


---
# <a name="create-a-virtual-network-with-a-site-to-site-vpn-connection-using-cli"></a>Erstellen eines virtuellen Netzwerks mit einer Site-to-Site-VPN-Verbindung per CLI

In diesem Artikel wird beschrieben, wie Sie die Azure-CLI zum Erstellen einer Site-to-Site-VPN Gateway-Verbindung aus Ihrem lokalen Netzwerk mit dem VNet verwenden. Die Schritte in diesem Artikel gelten für das Resource Manager-Bereitstellungsmodell. Sie können diese Konfiguration auch erstellen, indem Sie ein anderes Bereitstellungstool oder Bereitstellungsmodell verwenden. Wählen Sie hierzu in der folgenden Liste eine andere Option:

> [!div class="op_single_selector"]
> * [Resource Manager – Azure-Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [Resource Manager – PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [Resource Manager – CLI](vpn-gateway-howto-site-to-site-resource-manager-cli.md)
> * [Klassisch – Azure-Portal](vpn-gateway-howto-site-to-site-classic-portal.md)
> * [Klassisch – Klassisches Portal](vpn-gateway-site-to-site-create.md)
> 
>


![Diagramm für die standortübergreifende Site-to-Site-VPN Gateway-Verbindung](./media/vpn-gateway-howto-site-to-site-resource-manager-cli/site-to-site-connection-diagram.png)

Eine Site-to-Site-VPN Gateway-Verbindung wird verwendet, um Ihr lokales Netzwerk über einen IPsec/IKE-VPN-Tunnel (IKEv1 oder IKEv2) mit einem virtuellen Azure-Netzwerk zu verbinden. Für diese Art von Verbindung wird ein lokales VPN-Gerät benötigt, dem eine externe öffentliche IP-Adresse zugewiesen ist. Weitere Informationen zu VPN-Gateways finden Sie unter [Informationen zu VPN Gateway](vpn-gateway-about-vpngateways.md).

## <a name="before-you-begin"></a>Voraussetzungen

Vergewissern Sie sich vor Beginn der Konfiguration, dass die folgenden Voraussetzungen erfüllt sind bzw. Folgendes vorhanden ist:

* Sie sind sicher, dass Sie das Resource Manager-Bereitstellungsmodell verwenden möchten. [!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 
* Ein kompatibles VPN-Gerät (und eine Person, die es konfigurieren kann). Weitere Informationen zu kompatiblen VPN-Geräten und zur Gerätekonfiguration finden Sie unter [Informationen zu VPN-Geräten](vpn-gateway-about-vpn-devices.md).
* Eine externe öffentliche IPv4-Adresse für Ihr VPN-Gerät. Diese IP-Adresse darf sich nicht hinter einer NAT befinden.
* Falls Sie mit den IP-Adressräumen Ihrer lokalen Netzwerkkonfiguration nicht vertraut sind, sollten Sie sich an eine Person wenden, die Ihnen diese Details geben kann. Beim Erstellen dieser Konfiguration müssen Sie die Präfixe für die IP-Adressbereiche angeben, die von Azure an Ihren lokalen Standort weitergeleitet werden. Kein Subnetz Ihres lokalen Netzwerks darf sich mit den Subnetzen des virtuellen Netzwerks überlappen, mit dem Sie eine Verbindung herstellen möchten. 
* Die aktuelle Version der CLI-Befehle (2.0 oder höher). Informationen zur Installation der CLI-Befehle finden Sie unter [Install Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (Installieren von Azure CLI 2.0).

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

## <a name="Login"></a>1. Anmelden an Azure

Melden Sie sich mit dem Befehl [az login](/cli/azure/#login) bei Ihrem Azure-Abonnement an, und befolgen Sie die Anweisungen auf dem Bildschirm.

```azurecli
az login
```

Listen Sie die Abonnements für das Konto auf, wenn Sie über mehr als ein Azure-Abonnement verfügen.

```azurecli
Az account list --all
```

Geben Sie das Abonnement an, das Sie verwenden möchten.

```azurecli
Az account set --subscription <replace_with_your_subscription_id>
```

## <a name="2-create-a-resource-group"></a>2. Erstellen einer Ressourcengruppe

Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen „TestRG1“ am Standort „eastus“ erstellt. Falls Sie in dieser Region bereits über eine Ressourcengruppe verfügen, die Sie zum Erstellen des VNet nutzen möchten, können Sie sie verwenden.

```azurecli
az group create -n TestRG1 -l eastus
```

## <a name="VNet"></a>3. Erstellen eines virtuellen Netzwerks

Falls Sie noch nicht über ein virtuelles Netzwerk verfügen, erstellen Sie eines. Stellen Sie beim Erstellen eines virtuellen Netzwerks sicher, dass sich die angegebenen Adressräume und die Adressräume im lokalen Netzwerk nicht überlappen. 

Im folgenden Beispiel werden ein virtuelles Netzwerk mit dem Namen „TestVNet1“ und ein Subnetz mit dem Namen „Subnet1“ erstellt.

```azurecli
az network vnet create -n TestVNet1 -g TestRG1 --address-prefix 10.12.0.0/16 -l eastus --subnet-name Subnet1 --subnet-prefix 10.12.0.0/24
```

## 4. <a name="gwsub"></a>Erstellen des Gatewaysubnetzes

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

Bei dieser Konfiguration benötigen Sie auch ein Gatewaysubnetz. Für das Gateway für virtuelle Netzwerke wird ein Gatewaysubnetz verwendet, das die von den VPN-Gatewaydiensten genutzten IP-Adressen enthält. Wenn Sie ein Gatewaysubnetz erstellen, muss dies den Namen „GatewaySubnet“ erhalten. Wenn Sie einen anderen Namen verwenden, erstellen Sie zwar ein Subnetz, aber es wird von Azure nicht wie ein Gatewaysubnetz behandelt.

Die Größe des von Ihnen angegebenen Gatewaysubnetzes richtet sich nach der VPN-Gatewaykonfiguration, die Sie erstellen möchten. Es ist zwar möglich, ein Gatewaysubnetz der Größe /29 zu erstellen, aber es wird empfohlen, ein größeres Subnetz mit mehr Adressen zu erstellen und /27 oder /28 auszuwählen. Die Verwendung eines größeren Gatewaysubnetzes ermöglicht die Vergabe einer ausreichenden Zahl von IP-Adressen für potenzielle zukünftige Konfigurationen.


```azurecli
az network vnet subnet create --address-prefix 10.12.255.0/27 -n GatewaySubnet -g TestRG1 --vnet-name TestVNet1
```

## <a name="localnet"></a>5. Erstellen des Gateways des lokalen Netzwerks

Mit dem Gateway des lokalen Netzwerks ist normalerweise Ihr lokaler Standort gemeint. Sie geben dem Standort einen Namen, über den Azure darauf verweisen kann, und geben dann die IP-Adresse des lokalen VPN-Geräts an, mit dem Sie eine Verbindung herstellen. Außerdem geben Sie die IP-Adresspräfixe an, die über das VPN-Gateway an das VPN-Gerät weitergeleitet werden. Die von Ihnen angegebenen Adresspräfixe befinden sich in Ihrem lokalen Netzwerk. Sie können diese Präfixe leicht aktualisieren, wenn sich Ihr lokales Netzwerk ändert.

Verwenden Sie die folgenden Werte:

* *--gateway-ip-address* ist die IP-Adresse Ihres lokalen VPN-Geräts. Das VPN-Gerät darf sich nicht hinter einer NAT befinden.
* *--local-address-prefixes* steht für Ihre lokalen Adressräume.

Im folgenden Beispiel wird veranschaulicht, wie Sie ein lokales Netzwerkgateway mit mehreren Adresspräfixen hinzufügen:

```azurecli
az network local-gateway create --gateway-ip-address 23.99.221.164 -n Site2 -g TestRG1 --local-address-prefixes 10.0.0.0/24 20.0.0.0/24
```

## <a name="PublicIP"></a>6. Anfordern einer öffentlichen IP-Adresse

Fordern Sie eine öffentliche IP-Adresse an, die Ihrem VPN-Gateway des virtuellen Netzwerks zugeordnet wird. Dies ist die IP-Adresse, die Sie für die Verbindung mit dem VPN-Gerät konfigurieren.

Das Gateway für virtuelle Netzwerke für das Resource Manager-Bereitstellungsmodell unterstützt derzeit nur öffentliche IP-Adressen. Dabei wird die Methode der dynamischen Zuordnung verwendet. Dies bedeutet aber nicht, dass sich die IP-Adresse ändert. Die IP-Adresse des VPN-Gateways ändert sich nur, wenn das Gateway gelöscht und neu erstellt wird. Die öffentliche IP-Adresse des Gateways für virtuelle Netzwerke ändert sich nicht bei Größenänderungen, beim Zurücksetzen oder bei anderen internen Wartungs-/Upgradeprozessen Ihres VPN-Gateways. 

```azurecli
az network public-ip create -n VNet1GWIP -g TestRG1 --allocation-method Dynamic
```

## <a name="CreateGateway"></a>7. Erstellen des VPN-Gateways

Erstellen Sie das VPN-Gateway für das virtuelle Netzwerk. Die Erstellung eines VPN-Gateways kann bis zu 45 Minuten dauern.

Verwenden Sie die folgenden Werte:

* *--gateway-type* für eine Site-to-Site-Konfiguration lautet *Vpn*. Der Gatewaytyp ist immer spezifisch für die Konfiguration, die Sie implementieren. Weitere Informationen finden Sie unter [Gatewaytypen](vpn-gateway-about-vpn-gateway-settings.md#gwtype).
* *--vpn-type* kann *RouteBased* (in einigen Dokumentationen als dynamisches Gateway bezeichnet) oder *PolicyBased* lauten (in einigen Dokumentationen als statisches Gateway bezeichnet). Die Einstellung richtet sich nach den Anforderungen des Geräts, mit dem Sie eine Verbindung herstellen. Weitere Informationen zu VPN-Gatewaytypen finden Sie unter [Informationen zu VPN Gateway-Einstellungen](vpn-gateway-about-vpn-gateway-settings.md#vpntype).
* *--sku* kann auf „Basic“, „Standard“ oder „HighPerformance“ festgelegt werden. Für bestimmte SKUs gelten Einschränkungen bei der Konfiguration. Weitere Informationen finden Sie unter [Gateway-SKUs](vpn-gateway-about-vpngateways.md#gateway-skus).

Nach der Ausführung dieses Befehls wird kein Feedback und keine Ausgabe angezeigt. Die Erstellung eines Gateways dauert ca. 45 Minuten.

```azurecli
az network vnet-gateway create -n VNet1GW --public-ip-address VNet1GWIP -g TestRG1 --vnet TestVNet1 --gateway-type Vpn --vpn-type RouteBased --sku Standard --no-wait 
```

## <a name="VPNDevice"></a>8. Konfigurieren des VPN-Geräts

[!INCLUDE [vpn-gateway-configure-vpn-device-rm](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]
Verwenden Sie zum Ermitteln der öffentlichen IP-Adresse Ihres Gateways für virtuelle Netzwerke das folgende Beispiel, und ersetzen Sie die Werte durch Ihre eigenen Werte. Zur besseren Lesbarkeit wird die Ausgabe formatiert, damit die Liste mit den öffentlichen IP-Adressen in Tabellenform angezeigt wird.

  ```azurecli
  az network public-ip list -g TestRG1 -o table
  ```

## <a name="CreateConnection"></a>9. Erstellen der VPN-Verbindung

Erstellen Sie die Site-to-Site-VPN-Verbindung zwischen dem Gateway Ihres virtuellen Netzwerks und Ihrem lokalen VPN-Gerät. Achten Sie besonders auf den Wert des gemeinsam verwendeten Schlüssels. Er muss mit dem konfigurierten Wert des gemeinsam verwendeten Schlüssels für Ihr VPN-Gerät übereinstimmen.

```azurecli
az network vpn-connection create -n VNet1toSite2 -g TestRG1 --vnet-gateway1 VNet1GW -l eastus --shared-key abc123 --local-gateway2 Site2
```

Die Verbindung wird nach kurzer Zeit hergestellt.

## <a name="toverify"></a>10. Überprüfen der VPN-Verbindung

[!INCLUDE [verify connection](../../includes/vpn-gateway-verify-connection-cli-rm-include.md)] 

Falls Sie ein anderes Verfahren zum Überprüfen Ihrer Verbindung verwenden möchten, helfen Ihnen die Informationen unter [Überprüfen einer VPN Gateway-Verbindung](vpn-gateway-verify-connection-resource-manager.md) weiter.

## <a name="common-tasks"></a>Häufige Aufgaben

### <a name="to-view-local-network-gateways"></a>So zeigen Sie Gateways des lokalen Netzwerks an

```azurecli
az network local-gateway list --resource-group TestRG1
```

### <a name="modify"></a>So ändern Sie die IP-Adresspräfixe für ein lokales Netzwerkgateway
Verwenden Sie die unten angegebene Anleitung, wenn Sie die Präfixe für Ihr Gateway für das lokale Netzwerk ändern müssen. Bei jeder Änderung muss die gesamte Liste mit Präfixen angegeben werden – nicht nur die Präfixe, die Sie ändern möchten.

- Verwenden Sie das Beispiel unten, **wenn Sie eine Verbindung angegeben haben**. Geben Sie die gesamte Liste mit den Präfixen an, also die vorhandenen Präfixe und die hinzuzufügenden Präfixe. In diesem Beispiel sind 10.0.0.0/24 und 20.0.0.0/24 bereits vorhanden. Wir fügen die Präfixe 30.0.0.0/24 und 40.0.0.0/24 hinzu.

  ```azurecli
  az network local-gateway update --local-address-prefixes 10.0.0.0/24 20.0.0.0/24 30.0.0.0/24 40.0.0.0/24 -n VNet1toSite2 -g TestRG1
  ```

- Verwenden Sie denselben Befehl, den Sie zum Erstellen eines Gateways für das lokale Netzwerk verwenden, **wenn Sie keine Verbindung angegeben haben**. Sie können diesen Befehl auch verwenden, um die Gateway-IP-Adresse für das VPN-Gerät zu aktualisieren. Verwenden Sie diesen Befehl nur, wenn Sie nicht bereits über eine Verbindung verfügen. In diesem Beispiel sind 10.0.0.0/24, 20.0.0.0/24, 30.0.0.0/24 und 40.0.0.0/24 vorhanden. Wir geben nur die Präfixe an, die wir beibehalten möchten. Hier sind dies 10.0.0.0/24 und 20.0.0.0/24.

  ```azurecli
  az network local-gateway create --gateway-ip-address 23.99.221.164 -n Site2 -g TestRG1 --local-address-prefixes 10.0.0.0/24 20.0.0.0/24
  ```

### <a name="modifygwipaddress"></a>So ändern Sie die Gateway-IP-Adresse für ein lokales Netzwerkgateway

Die IP-Adresse dieser Konfiguration ist die IP-Adresse des VPN-Geräts, für das Sie eine Verbindung erstellen. Wenn sich die IP-Adresse des VPN-Geräts ändert, können Sie den Wert ändern. Die IP-Adresse kann auch dann geändert werden, wenn eine Gatewayverbindung vorhanden ist.

```azurecli
az network local-gateway update --gateway-ip-address 23.99.222.170 -n Site2 -g TestRG1
```

Stellen Sie beim Anzeigen des Ergebnisses sicher, dass die Präfixe der IP-Adressen aufgeführt sind.

  ```azurecli
  "localNetworkAddressSpace": { 
    "addressPrefixes": [ 
      "10.0.0.0/24", 
      "20.0.0.0/24", 
      "30.0.0.0/24" 
    ] 
  }, 
  "location": "eastus", 
  "name": "Site2", 
  "provisioningState": "Succeeded",  
  ```

### <a name="to-view-the-virtual-network-gateway-public-ip-address"></a>So zeigen Sie die öffentliche IP-Adresse für das Gateway des virtuellen Netzwerks an

Verwenden Sie das folgende Beispiel, um die öffentliche IP-Adresse des Gateways des virtuellen Netzwerks zu ermitteln. Zur besseren Lesbarkeit wird die Ausgabe formatiert, damit die Liste mit den öffentlichen IP-Adressen in Tabellenform angezeigt wird.

```azurecli
az network public-ip list -g TestRG1 -o table
```

### <a name="to-verify-the-shared-key-values"></a>So überprüfen Sie die Werte für den gemeinsam verwendeten Schlüssel

Vergewissern Sie sich, dass der Wert des gemeinsam verwendeten Schlüssels dem Wert entspricht, den Sie für die Konfiguration Ihres VPN-Geräts verwendet haben. Wenn nicht, sollten Sie entweder die Verbindung erneut ausführen, indem Sie den Wert des Geräts verwenden, oder das Gerät mit dem zurückgegebenen Wert aktualisieren. Die Werte müssen übereinstimmen.

```azurecli
az network vpn-connection shared-key show --connection-name VNet1toSite2 -g TestRG1
```

## <a name="next-steps"></a>Nächste Schritte

*  Sobald die Verbindung hergestellt ist, können Sie Ihren virtuellen Netzwerken virtuelle Computer hinzufügen. Weitere Informationen finden Sie unter [Virtuelle Computer](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) .
* Informationen zu BGP finden Sie in der [Übersicht über BGP](vpn-gateway-bgp-overview.md) und unter [Konfigurieren von BGP](vpn-gateway-bgp-resource-manager-ps.md).
* Weitere Informationen zur Tunnelerzwingung finden Sie unter [Konfigurieren der Tunnelerzwingung](vpn-gateway-forced-tunneling-rm.md).
* Eine Liste mit Azure-CLI-Netzwerkbefehlen finden Sie unter [Azure-CLI](https://docs.microsoft.com/cli/azure/network).
