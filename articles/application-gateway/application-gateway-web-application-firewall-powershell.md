---
title: "Konfigurieren der Web Application Firewall – Azure Application Gateway | Microsoft-Dokumentation"
description: "Dieser Artikel enthält Anleitungen zur Verwendung der Web Application Firewall auf einem vorhandenen oder neuen Anwendungsgateway."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 670b9732-874b-43e6-843b-d2585c160982
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/03/2017
ms.author: gwallace
ms.translationtype: HT
ms.sourcegitcommit: 137671152878e6e1ee5ba398dd5267feefc435b7
ms.openlocfilehash: 8a2281cea551092be4b5c628c1e541b04021523d
ms.contentlocale: de-de
ms.lasthandoff: 07/28/2017

---
# <a name="configure-web-application-firewall-on-a-new-or-existing-application-gateway"></a>Konfigurieren der Web Application Firewall auf einem neuen oder vorhandenen Anwendungsgateway

> [!div class="op_single_selector"]
> * [Azure-Portal](application-gateway-web-application-firewall-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-web-application-firewall-powershell.md)

Erfahren Sie, wie Sie ein durch eine Web Application Firewall aktiviertes Anwendungsgateway erstellen oder einem vorhandenen Anwendungsgateway eine Web Application Firewall hinzufügen.

Die Web Application Firewall (WAF) in Azure Application Gateway schützt Webanwendungen vor gängigen webbasierten Angriffen wie der Einschleusung von SQL-Code, Angriffen durch websiteübergreifende Skripts und der Übernahme von Sitzungen.

Azure Application Gateway verwendet einen Load Balancer auf der Schicht 7 (Anwendungsschicht). Das Application Gateway ermöglicht ein Failover sowie schnelles Routing von HTTP-Anforderungen zwischen verschiedenen Servern in der Cloud und der lokalen Umgebung. Application Gateway bietet zahlreiche Application Delivery Controller-Funktionen (ADC), u.a. HTTP-Lastenausgleich, cookiebasierte Sitzungsaffinität, SSL-Auslagerung (Secure Sockets Layer), benutzerdefinierte Integritätstests und Unterstützung für mehrere Websites. Eine vollständige Liste der unterstützten Features finden Sie unter „Übersicht über Application Gateway“.

Der folgende Artikel zeigt, wie Sie die [Web Application Firewall einem vorhandenen Anwendungsgateway hinzufügen](#add-web-application-firewall-to-an-existing-application-gateway) und [ein Anwendungsgateway erstellen](#create-an-application-gateway-with-web-application-firewall), das die Web Application Firewall verwendet.

![Szenarioabbildung][scenario]

## <a name="waf-configuration-differences"></a>Unterschiede bei der WAF-Konfiguration

Wenn Sie [Erstellen eines Anwendungsgateway mit PowerShell](application-gateway-create-gateway-arm.md)gelesen haben, kennen Sie die SKU-Einstellungen, die beim Erstellen eines Anwendungsgateways konfiguriert werden müssen. Die WAF bietet zusätzliche Einstellungen, die Sie beim Konfigurieren der SKU auf einem Anwendungsgateway definieren müssen. Am Anwendungsgateway selbst sind keine zusätzlichen Änderungen erforderlich.

| **Einstellung** | **Details**
|---|---|
|**SKU** |Ein normales Anwendungsgateway ohne WAF unterstützt die Größen **Standard\_Small**, **Standard\_Medium** und **Standard\_Large**. Mit der Einführung der WAF gibt es zwei zusätzliche SKUs: **WAF\_Medium** und **WAF\_Large**. Die WAF wird auf kleinen Anwendungsgateways nicht unterstützt.|
|**Tier** | Verfügbare Werte sind **Standard** oder **WAF**. Bei Verwendung der Web Application Firewall muss **WAF** ausgewählt werden.|
|**Mode** | Diese Einstellung ist der Modus der WAF. Zulässige Werte sind **Detection** (Erkennung) und **Prevention** (Schutz). Wenn die WAF im Erkennungsmodus eingerichtet wird, werden alle Bedrohungen in einer Protokolldatei gespeichert. Im Schutzmodus werden Ereignisse ebenfalls protokolliert, aber der Angreifer erhält zudem die Antwort 403 (nicht autorisiert) vom Anwendungsgateway.|

## <a name="add-web-application-firewall-to-an-existing-application-gateway"></a>Hinzufügen der Web Application Firewall zu einem vorhandenen Anwendungsgateway

Stellen Sie sicher, dass Sie die neueste Version von Azure PowerShell verwenden. Weitere Informationen finden Sie unter [Verwenden von Windows PowerShell mit Resource Manager](../powershell-azure-resource-manager.md).

1. Melden Sie sich beim Azure-Konto an.

    ```powershell
    Login-AzureRmAccount
    ```

2. Wählen Sie das Abonnement aus, das für dieses Szenario verwendet werden soll.

    ```powershell
    Select-AzureRmSubscription -SubscriptionName "<Subscription name>"
    ```

3. Rufen Sie das Gateway ab, dem Sie die Web Application Firewall hinzufügen möchten.

    ```powershell
    $gw = Get-AzureRmApplicationGateway -Name "AdatumGateway" -ResourceGroupName "MyResourceGroup"
    ```

1. Konfigurieren Sie die Web Application Firewall-SKU. Die verfügbaren Größen sind **WAF\_Large** und **WAF\_Medium**. Bei Verwendung von Web Application Firewall muss die Ebene **WAF** ausgewählt sein, und beim Einrichten der SKU muss die Kapazität bestätigt werden.

    ```powershell
    $gw | Set-AzureRmApplicationGatewaySku -Name WAF_Large -Tier WAF -Capacity 2
    ```

1. Konfigurieren Sie die WAF-Einstellungen, wie im folgenden Beispiel definiert:

   Für **FirewallMode** lauten die verfügbaren Werte „Prevention“ und „Detection“.

    ```powershell
    $gw | Set-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode Prevention
    ```

1. Aktualisieren Sie das Anwendungsgateway mit den im vorherigen Schritt definierten Einstellungen.

    ```powershell
    Set-AzureRmApplicationGateway -ApplicationGateway $gw
    ```

Dieser Befehl aktualisiert das Anwendungsgateway mit der Web Application Firewall. Es wird empfohlen, die [Application Gateway-Diagnose](application-gateway-diagnostics.md) anzuzeigen, um die Anzeige von Protokollen für Ihr Anwendungsgateway nachzuvollziehen. Aufgrund der Art der WAF-Sicherheit müssen Protokolle regelmäßig überprüft werden, um sich über die Sicherheitslage Ihrer Webanwendungen zu informieren.

## <a name="create-an-application-gateway-with-web-application-firewall"></a>Erstellen eines Anwendungsgateways mit der Web Application Firewall

Die folgenden Schritte führen Sie durch den gesamten Prozess zum Erstellen eines Anwendungsgateways mit Web Application Firewall.

Stellen Sie sicher, dass Sie die neueste Version von Azure PowerShell verwenden. Weitere Informationen finden Sie unter [Verwenden von Windows PowerShell mit Resource Manager](../powershell-azure-resource-manager.md).

1. Melden Sie sich bei Azure an, indem Sie `Login-AzureRmAccount` ausführen. Sie werden zur Authentifizierung mit Ihren Anmeldeinformationen aufgefordert.

1. Überprüfen Sie die Abonnements für das Konto, indem Sie `Get-AzureRmSubscription` ausführen.

1. Wählen Sie aus, welches Azure-Abonnement Sie verwenden möchten.

    ```powershell
    Select-AzureRmsubscription -SubscriptionName "<Subscription name>"
    ```

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie eine Ressourcengruppe für das Anwendungsgateway.

```powershell
New-AzureRmResourceGroup -Name appgw-rg -Location "West US"
```

Der Azure Resource Manager erfordert, dass alle Ressourcengruppen einen Speicherort angeben. Dieser wird als Standardspeicherort für Ressourcen in dieser Ressourcengruppe verwendet. Stellen Sie sicher, dass alle Befehle, mit denen ein Anwendungsgateway erstellt wird, die gleiche Ressourcengruppe verwenden.

Im obigen Beispiel haben wir eine Ressourcengruppe namens „appgw-RG“ mit dem Standort „USA, Westen“ erstellt.

> [!NOTE]
> Wenn Sie einen benutzerdefinierten Test für Ihr Anwendungsgateway konfigurieren müssen, finden Sie unter [Erstellen eines benutzerdefinierten Tests für Azure Application Gateway mithilfe von PowerShell für Azure-Ressourcen-Manager](application-gateway-create-probe-ps.md)weitere Informationen. Weitere Informationen finden Sie unter [Benutzerdefinierte Tests und Systemüberwachung](application-gateway-probe-overview.md) .

### <a name="configure-virtual-network"></a>Konfigurieren des virtuellen Netzwerks

Für Application Gateway ist ein eigenes Subnetz erforderlich. In diesem Schritt erstellen Sie ein virtuelles Netzwerk mit einem Adressbereich von 10.0.0.0/16 sowie zwei Subnetze: eines für das Anwendungsgateway und das andere für die Back-End-Poolmitglieder.

```powershell
# Create a subnet configuration object for the application gateway subnet. A subnet for an application should have a minimum of 28 mask bits. This value leaves 10 available addresses in the subnet for Application Gateway instances. With a smaller subnet, you may not be able to add more instance of your application gateway in the future.
$gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24

# Create a subnet configuration object for the backend pool members subnet
$nicSubnet = New-AzureRmVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24

# Create the virtual network with the previous created subnets
$vnet = New-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet
```

### <a name="configure-public-ip-address"></a>Konfigurieren der öffentlichen IP-Adresse

Um externe Anforderungen zu verarbeiten, erfordert das Anwendungsgateway eine öffentliche IP-Adresse. Diese öffentliche IP-Adresse darf nicht `DomainNameLabel` definiert haben, um vom Anwendungsgateway verwendet zu werden.

```powershell
# Create a public IP address for use with the application gateway. Defining the domainnamelabel during creation is not supported for use with application gateway
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name 'appgwpip' -Location "West US" -AllocationMethod Dynamic
```

### <a name="configure-the-application-gateway"></a>Konfigurieren des Anwendungsgateways

```powershell
# Create a IP configuration. This configures what subnet the Application Gateway uses. When Application Gateway starts, it picks up an IP address from the subnet configured and routes network traffic to the IP addresses in the back-end IP pool.
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet

# Create a backend pool to hold the addresses or NICs for the application that application gateway is protecting.
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 1.1.1.1, 2.2.2.2, 3.3.3.3

# Upload the authenication certificate that will be used to communicate with the backend servers
$authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name 'whitelistcert1' -CertificateFile <full path to .cer file>

# Conifugre the backend HTTP settings to be used to define how traffic is routed to the backend pool. The authenication certificate used in the previous step is added to the backend http settings.
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name 'setting01' -Port 443 -Protocol Https -CookieBasedAffinity Enabled -AuthenticationCertificates $authcert

# Create a frontend port to be used by the listener.
$fp = New-AzureRmApplicationGatewayFrontendPort -Name 'port01'  -Port 443

# Create a frontend IP configuration to associate the public IP address with the application gateway
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip

# Configure the certificate for the application gateway. This certificate is used to decrypt and re-encrypt the traffic on the application gateway.
$cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path to .pfx file> -Password <password for certificate file>

# Create the HTTP listener for the application gateway. Assign the front-end ip configuration, port, and ssl certificate to use.
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert

#Create a load balancer routing rule that configures the load balancer behavior. In this example, a basic round robin rule is created.
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

# Configure the SKU of the application gateway
$sku = New-AzureRmApplicationGatewaySku -Name WAF_Medium -Tier WAF -Capacity 2

#Configure the waf configuration settings.
$config = New-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode "Prevention"

# Create the application gateway utilizing all the previously created configuration objects
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -WebApplicationFirewallConfig $config -SslCertificates $cert -AuthenticationCertificates $authcert
```

> [!NOTE]
> Anwendungsgateways, die mit der Basiskonfiguration der Web Application Firewall erstellt wurden, werden zum Schutz mit CRS 3.0 konfiguriert.

## <a name="get-application-gateway-dns-name"></a>Abrufen des DNS-Namens des Anwendungsgateways

Nach dem Erstellen des Gateways wird das Front-End für die Kommunikation konfiguriert. Wenn Sie eine öffentliche IP-Adresse verwenden, wird das Anwendungsgateway ein dynamisch zugewiesener DNS-Namen benötigt, der kein Anzeigename ist. Um sicherzustellen, dass die Endbenutzer auf das Anwendungsgateway zugreifen können, kann mit einem CNAME-Eintrag auf den öffentlichen Endpunkt des Anwendungsgateways verwiesen werden. Rufen Sie hierzu mithilfe des PublicIPAddress-Elements, das an das Anwendungsgateway angefügt ist, Details zum Anwendungsgateway und den zugeordneten IP/DNS-Namen ab. Verwenden Sie hierzu Azure DNS oder einen anderen DNS-Anbieter, und erstellen Sie einen CNAME-Eintrag, der auf die [öffentliche IP-Adresse](../dns/dns-custom-domain.md#public-ip-address) verweist. Die Verwendung von A-Einträgen wird nicht empfohlen, da sich die VIP beim Neustart des Anwendungsgateways möglicherweise verändert.

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -Name publicIP01
```

```
Name                     : publicIP01
ResourceGroupName        : appgw-RG
Location                 : westus
Id                       : /subscriptions/<subscription_id>/resourceGroups/appgw-RG/providers/Microsoft.Network/publicIPAddresses/publicIP01
Etag                     : W/"00000d5b-54ed-4907-bae8-99bd5766d0e5"
ResourceGuid             : 00000000-0000-0000-0000-000000000000
ProvisioningState        : Succeeded
Tags                     : 
PublicIpAllocationMethod : Dynamic
IpAddress                : xx.xx.xxx.xx
PublicIpAddressVersion   : IPv4
IdleTimeoutInMinutes     : 4
IpConfiguration          : {
                                "Id": "/subscriptions/<subscription_id>/resourceGroups/appgw-RG/providers/Microsoft.Network/applicationGateways/appgwtest/frontendIP
                            Configurations/frontend1"
                            }
DnsSettings              : {
                                "Fqdn": "00000000-0000-xxxx-xxxx-xxxxxxxxxxxx.cloudapp.net"
                            }
```

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Konfigurieren der Diagnoseprotokollierung zum Protokollieren von Ereignissen, die mit der Web Application Firewall erkannt oder verhindert werden, finden Sie unter [Application Gateway-Diagnose](application-gateway-diagnostics.md).

[scenario]: ./media/application-gateway-web-application-firewall-powershell/scenario.png

