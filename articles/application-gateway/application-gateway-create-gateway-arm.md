---
title: "Erstellen und Verwalten einer Azure Application Gateway-Instanz – PowerShell | Microsoft-Dokumentation"
description: "Diese Seite enthält Anweisungen zum Erstellen, Konfigurieren, Starten und Löschen eines Azure-Anwendungsgateways mit Azure Resource Manager."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: gwallace
ms.translationtype: HT
ms.sourcegitcommit: fff84ee45818e4699df380e1536f71b2a4003c71
ms.openlocfilehash: 5f1713365406764998de505ff62309bab9fa2567
ms.contentlocale: de-de
ms.lasthandoff: 08/01/2017

---
# <a name="create-start-or-delete-an-application-gateway-by-using-azure-resource-manager"></a>Erstellen, Starten oder Löschen eines Anwendungsgateways mit Azure Resource Manager

> [!div class="op_single_selector"]
> * [Azure-Portal](application-gateway-create-gateway-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-gateway-arm.md)
> * [Klassische Azure PowerShell](application-gateway-create-gateway.md)
> * [Azure Resource Manager-Vorlage](application-gateway-create-gateway-arm-template.md)
> * [Azure-Befehlszeilenschnittstelle](application-gateway-create-gateway-cli.md)

Azure Application Gateway verwendet einen Load Balancer auf der Schicht 7 (Anwendungsschicht). Das Application Gateway ermöglicht Failover sowie schnelles Routing von HTTP-Anforderungen zwischen verschiedenen Servern in der Cloud und der lokalen Umgebung. Application Gateway bietet zahlreiche ADC-Features (Application Delivery Controller) wie HTTP-Lastenausgleich, cookiebasierte Sitzungsaffinität, SSL-Auslagerung (Secure Sockets Layer), benutzerdefinierte Integritätstests und Unterstützung mehrerer Standorte. Eine Liste mit allen unterstützten Features finden Sie unter [Übersicht über Application Gateway](application-gateway-introduction.md).

In diesem Artikel werden Sie durch die Schritte zum Erstellen, Konfigurieren, Starten und Löschen eines Anwendungsgateways geführt.

> [!IMPORTANT]
> Vor der Verwendung von Azure-Ressourcen sollten Sie wissen, dass Azure derzeit über zwei Bereitstellungsmodelle verfügt: Resource Manager-Bereitstellung und klassische Bereitstellung. Machen Sie sich vor der Verwendung von Azure-Ressourcen unbedingt mit den [Bereitstellungsmodellen und -tools](../azure-classic-rm.md) vertraut. Zum Anzeigen der Dokumentation für verschiedene Tools klicken Sie auf die Registerkarten oben in diesem Artikel. In diesem Dokument erfahren Sie alles über das Erstellen eines Anwendungsgateways mit Azure Resource Manager. Um die klassische Version zu verwenden, wechseln Sie zu [Erstellen, Starten oder Löschen eines Anwendungsgateways](application-gateway-create-gateway.md).

## <a name="before-you-begin"></a>Voraussetzungen

1. Installieren Sie mit dem Webplattform-Installer die aktuelle Version der Azure PowerShell-Cmdlets. Sie können die neueste Version aus dem Abschnitt **Windows PowerShell** der [Downloadseite](https://azure.microsoft.com/downloads/)herunterladen und installieren.
1. Wenn Sie über ein vorhandenes Gateway verfügen, können Sie entweder ein vorhandenes leeres Subnetz auswählen oder im vorhandenen virtuellen Netzwerk ein Subnetz erstellen, das ausschließlich vom Anwendungsgateway verwendet wird. Sie können das Anwendungsgateway nicht in einem anderen virtuellen Netzwerk als dem Netzwerk mit den Ressourcen bereitstellen, die Sie hinter dem Anwendungsgateway bereitstellen möchten.
1. Die Server, die Sie für die Verwendung des Anwendungsgateways konfigurieren, müssen vorhanden sein oder Endpunkte aufweisen, die im virtuellen Netzwerk erstellt wurden oder denen eine öffentliche IP-Adresse/VIP zugewiesen wurde.

## <a name="what-is-required-to-create-an-application-gateway"></a>Was ist zum Erstellen eines Application Gateways erforderlich?

* **Back-End-Serverpool:** Die Liste mit IP-Adressen, FQDNs oder NICs des Back-End-Servers. Bei Verwendung von IP-Adressen müssen diese entweder dem Subnetz des virtuellen Netzwerks angehören oder eine öffentliche IP-Adresse/VIP sein.
* **Einstellungen für den Back-End-Serverpool:** Für jeden Pool sind Einstellungen wie Port, Protokoll und cookiebasierte Affinität festgelegt. Diese Einstellungen sind an einen Pool gebunden und gelten für alle Server innerhalb des Pools.
* **Front-End-Port:** Der öffentliche Port, der im Application Gateway geöffnet ist. Datenverkehr erreicht diesen Port und wird dann an einen der Back-End-Server umgeleitet.
* **Listener:** Der Listener verfügt über einen Front-End-Port, ein Protokoll („Http“ oder „Https“; jeweils unter Beachtung der Groß-/Kleinschreibung) und den Namen des SSL-Zertifikats (falls die SSL-Auslagerung konfiguriert wird).
* **Regel:** Mit der Regel werden der Listener und der Back-End-Serverpool gebunden, und es wird definiert, an welchen Back-End-Serverpool der Datenverkehr gesendet werden soll, wenn er einen bestimmten Listener erreicht.

## <a name="create-a-resource-group-for-resource-manager"></a>Erstellen einer Ressourcengruppe für den Ressourcen-Manager

Stellen Sie sicher, dass Sie die neueste Version von Azure PowerShell verwenden. Weitere Informationen finden Sie unter [Verwenden von Windows PowerShell mit Resource Manager](../powershell-azure-resource-manager.md).

1. Melden Sie sich bei Azure an, und geben Sie Ihre Anmeldeinformationen ein.

  ```powershell
  Login-AzureRmAccount
  ```

2. Überprüfen Sie die Abonnements für das Konto.

  ```powershell
  Get-AzureRmSubscription
  ```

3. Wählen Sie aus, welches Azure-Abonnement Sie verwenden möchten.

  ```powershell
  Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
  ```

4. Erstellen Sie eine Ressourcengruppe. (Überspringen Sie diesen Schritt, wenn Sie eine vorhandene Ressourcengruppe verwenden.)

  ```powershell
  New-AzureRmResourceGroup -Name ContosoRG -Location "West US"
  ```

Der Azure Resource Manager erfordert, dass alle Ressourcengruppen einen Speicherort angeben. Dieser wird als Standardspeicherort für Ressourcen in dieser Ressourcengruppe verwendet. Stellen Sie sicher, dass alle Befehle, mit denen ein Anwendungsgateway erstellt wird, die gleiche Ressourcengruppe verwenden.

Im obigen Beispiel wurde eine Ressourcengruppe mit dem Namen **ContosoRG** und dem Standort **USA, Osten** erstellt.

> [!NOTE]
> Wenn Sie einen benutzerdefinierten Test für Ihr Anwendungsgateway konfigurieren müssen, sehen Sie sich den folgenden Artikel an: [Erstellen eines benutzerdefinierten Tests für Azure Application Gateway mithilfe von PowerShell für Azure Resource Manager](application-gateway-create-probe-ps.md). Weitere Informationen finden Sie unter [Benutzerdefinierte Tests und Systemüberwachung](application-gateway-probe-overview.md) .


## <a name="create-the-application-gateway-configuration-objects"></a>Erstellen der Konfigurationsobjekte für das Anwendungsgateway

Sie müssen alle Konfigurationselemente einrichten, bevor Sie das Anwendungsgateway erstellen. Die folgenden Schritten erstellen die Konfigurationselemente, die für eine Application Gateway-Ressource benötigt werden.

```powershell
# Create a subnet and assign the address space of 10.0.0.0/24
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

# Create a virtual network with the address space of 10.0.0.0/16 and add the subnet
$vnet = New-AzureRmVirtualNetwork -Name ContosoVNET -ResourceGroupName ContosoRG -Location "East US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet

# Retrieve the newly created subnet
$subnet=$vnet.Subnets[0]

# Create a public IP address that is used to connect to the application gateway. Application Gateway does not support custom DNS names on public IP addresses.  If a custom name is required for the public endpoint, a CNAME record should be created to point to the automatically generated DNS name for the public IP address.
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName ContosoRG -name publicIP01 -location "East US" -AllocationMethod Dynamic

# Create a gateway IP configuration. The gateway picks up an IP addressfrom the configured subnet and routes network traffic to the IP addresses in the backend IP pool. Keep in mind that each instance takes one IP address.
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

# Configure a backend pool with the addresses of your web servers. These backend pool members are all validated to be healthy by probes, whether they are basic probes or custom probes.  Traffic is then routed to them when requests come into the application gateway. Backend pools can be used by multiple rules within the application gateway, which means one backend pool could be used for multiple web applications that reside on the same host.
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221, 134.170.185.50

# Configure backend http settings to determine the protocol and port that is used when sending traffic to the backend servers. Cookie-based sessions are also determined by the backend HTTP settings.  If enabled, cookie-based session affinity sends traffic to the same backend as previous requests for each packet.
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled -RequestTimeout 120

# Configure a frontend port that is used to connect to the application gateway through the public IP address
$fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80

# Configure the frontend IP configuration with the public IP address created earlier.
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

# Configure the listener.  The listener is a combination of the front end IP configuration, protocol, and port and is used to receive incoming network traffic. 
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

# Configure a basic rule that is used to route traffic to the backend servers. The backend pool settings, listener, and backend pool created in the previous steps make up the rule. Based on the criteria defined traffic is routed to the appropriate backend.
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

# Configure the SKU for the application gateway, this determines the size and whether or not WAF is used.
$sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

# Create the application gateway
$appgw = New-AzureRmApplicationGateway -Name ContosoAppGateway -ResourceGroupName ContosoRG -Location "East US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku
```

Rufen Sie anschließend die DNS- und VIP-Details des Anwendungsgateways aus der öffentlichen IP-Ressource ab, die an das Anwendungsgateway angefügt ist.

```powershell
Get-AzureRmPublicIpAddress -Name publicIP01 -ResourceGroupName ContosoRG
```

## <a name="delete-the-application-gateway"></a>Löschen des Anwendungsgateways

Das folgende Beispiel löscht das Anwendungsgateway.

```powershell
# Retrieve the application gateway
$gw = Get-AzureRmApplicationGateway -Name ContosoAppGateway -ResourceGroupName ContosoRG

# Stops the application gateway
Stop-AzureRmApplicationGateway -ApplicationGateway $gw

# Once the application gateway is in a stopped state, use the `Remove-AzureRmApplicationGateway` cmdlet to remove the service.
Remove-AzureRmApplicationGateway -Name ContosoAppGateway -ResourceGroupName ContosoRG -Force
```

> [!NOTE]
> Mit dem optionalen Switch **-force** kann diese Bestätigungsmeldung unterdrückt werden.

Mithilfe des Cmdlets `Get-AzureRmApplicationGateway` können Sie sicherstellen, dass der Dienst entfernt wurde. Dieser Schritt ist nicht erforderlich.

```powershell
Get-AzureRmApplicationGateway -Name ContosoAppGateway -ResourceGroupName ContosoRG
```

## <a name="get-application-gateway-dns-name"></a>Abrufen des DNS-Namens des Anwendungsgateways

Nach dem Erstellen des Gateways wird das Front-End für die Kommunikation konfiguriert. Wenn Sie eine öffentliche IP-Adresse verwenden, wird das Anwendungsgateway ein dynamisch zugewiesener DNS-Namen benötigt, der kein Anzeigename ist. Um sicherzustellen, dass die Endbenutzer auf das Anwendungsgateway zugreifen können, kann mit einem CNAME-Eintrag auf den öffentlichen Endpunkt des Anwendungsgateways verwiesen werden. Rufen Sie hierzu mithilfe des PublicIPAddress-Elements, das an das Anwendungsgateway angefügt ist, Details zum Anwendungsgateway und den zugeordneten IP/DNS-Namen ab. Verwenden Sie hierzu Azure DNS oder einen anderen DNS-Anbieter, und erstellen Sie einen CNAME-Eintrag, der auf die [öffentliche IP-Adresse](../dns/dns-custom-domain.md#public-ip-address) verweist. Die Verwendung von A-Einträgen wird nicht empfohlen, da sich die VIP beim Neustart des Anwendungsgateways möglicherweise verändert.

> [!NOTE]
> Dem Anwendungsgateway wird beim Starten des Diensts eine IP-Adresse zugewiesen.

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName ContosoRG -Name publicIP01
```

```
Name                     : publicIP01
ResourceGroupName        : ContosoRG
Location                 : westus
Id                       : /subscriptions/<subscription_id>/resourceGroups/ContosoRG/providers/Microsoft.Network/publicIPAddresses/publicIP01
Etag                     : W/"00000d5b-54ed-4907-bae8-99bd5766d0e5"
ResourceGuid             : 00000000-0000-0000-0000-000000000000
ProvisioningState        : Succeeded
Tags                     : 
PublicIpAllocationMethod : Dynamic
IpAddress                : xx.xx.xxx.xx
PublicIpAddressVersion   : IPv4
IdleTimeoutInMinutes     : 4
IpConfiguration          : {
                                "Id": "/subscriptions/<subscription_id>/resourceGroups/ContosoRG/providers/Microsoft.Network/applicationGateways/ContosoAppGateway/frontendIP
                            Configurations/frontend1"
                            }
DnsSettings              : {
                                "Fqdn": "00000000-0000-xxxx-xxxx-xxxxxxxxxxxx.cloudapp.net"
                            }
```

## <a name="delete-all-resources"></a>Löschen aller Ressourcen

Führen Sie den folgenden Schritt aus, um alle Ressourcen zu löschen, die in diesem Artikel erstellt wurden:

```powershell
Remove-AzureRmResourceGroup -Name ContosoRG
```

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Konfigurieren der SSL-Auslagerung finden Sie unter [Konfigurieren eines Anwendungsgateways für SSL-Auslagerung mit klassischem Bereitstellungsmodell](application-gateway-ssl.md).

Informationen zum Konfigurieren eines Anwendungsgateways für die Verwendung mit einem internen Lastenausgleich finden Sie unter [Erstellen eines Anwendungsgateways mit einem internen Lastenausgleich (ILB)](application-gateway-ilb.md).

Weitere grundsätzliche Informationen zu Lastenausgleichsoptionen finden Sie unter:

* [Azure-Lastenausgleich](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)

