---
title: 'Konfigurieren einer Web Application Firewall: Azure Application Gateway | Microsoft-Dokumentation'
description: "Dieser Artikel enthält Anleitungen zur Verwendung einer Web Application Firewall für ein vorhandenes oder neues Anwendungsgateway."
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
ms.sourcegitcommit: a0b98d400db31e9bb85611b3029616cc7b2b4b3f
ms.openlocfilehash: 3d01763709e58d25047a6dec4361bcf6553d54e0
ms.contentlocale: de-de
ms.lasthandoff: 08/29/2017

---
# <a name="configure-a-web-application-firewall-on-a-new-or-existing-application-gateway"></a>Konfigurieren einer Web Application Firewall auf einem neuen oder vorhandenen Anwendungsgateway

> [!div class="op_single_selector"]
> * [Azure-Portal](application-gateway-web-application-firewall-portal.md)
> * [PowerShell](application-gateway-web-application-firewall-powershell.md)
> * [Azure-CLI](application-gateway-web-application-firewall-cli.md)

In diesem Artikel wird erläutert, wie ein Anwendungsgateway, für das eine Web Application Firewall (WAF) aktiviert ist, erstellt wird. Zudem wird das Hinzufügen von WAF zu einem vorhandenen Anwendungsgateway erläutert.

WAF in Azure Application Gateway schützt Webanwendungen vor gängigen webbasierten Angriffen wie der Einschleusung von SQL-Befehlen, Angriffen durch websiteübergreifende Skripts und der Übernahme von Sitzungen.

 Application Gateway ist ein Lastenausgleich auf Schicht 7 (Anwendungsschicht). Application Gateway ermöglicht ein Failover sowie schnelles Routing von HTTP-Anforderungen zwischen verschiedenen Servern in der Cloud und der lokalen Umgebung. Application Gateway stellt viele ADC-Features (Application Delivery Controller) bereit:

 * HTTP-Lastenausgleich
 * Cookiebasierte Sitzungsaffinität
 * SSL-Auslagerung (Secure Sockets Layer)
 * Benutzerdefinierte Integritätstests
 * Unterstützung der Funktion für mehrere Standorte
 
 Eine vollständige Liste der unterstützten Features finden Sie unter [Übersicht über Application Gateway](application-gateway-introduction.md).

Dieser Artikel veranschaulicht das [Hinzufügen von WAF zu einem vorhandenen Anwendungsgateway](#add-web-application-firewall-to-an-existing-application-gateway). Er erläutert außerdem das [Erstellen eines Anwendungsgateways, das WAF nutzt](#create-an-application-gateway-with-web-application-firewall).

![Szenarioabbildung][scenario]

## <a name="waf-configuration-differences"></a>Unterschiede bei der WAF-Konfiguration

Wenn Sie [Erstellen eines Anwendungsgateways mit PowerShell](application-gateway-create-gateway-arm.md) gelesen haben, kennen Sie die SKU-Einstellungen, die beim Erstellen eines Anwendungsgateways konfiguriert werden müssen. WAF bietet zusätzliche Einstellungen, die Sie beim Konfigurieren einer SKU für ein Anwendungsgateway definieren müssen. Am Anwendungsgateway selbst sind keine zusätzlichen Änderungen erforderlich.

| **Einstellung** | **Details**
|---|---|
|**SKU** |Ein normales Anwendungsgateway ohne WAF unterstützt die Größen **Standard\_Small**, **Standard\_Medium** und **Standard\_Large**. Mit der Einführung von WAF gibt es zwei zusätzliche SKUs: **WAF\_Medium** und **WAF\_Large**. WAF wird für kleine Anwendungsgateways nicht unterstützt.|
|**Tier** | Verfügbare Werte sind **Standard** oder **WAF**. Wenn Sie WAF verwenden, müssen Sie **WAF** auswählen.|
|**Mode** | Diese Einstellung ist der WAF-Modus. Zulässige Werte sind **Detection** (Erkennung) und **Prevention** (Schutz). Wenn WAF im **Erkennungsmodus** eingerichtet wird, werden alle Bedrohungen in einer Protokolldatei gespeichert. Im **Schutzmodus** werden Ereignisse ebenfalls protokolliert, der Angreifer erhält vom Anwendungsgateway jedoch zusätzlich die Meldung 403 (nicht autorisiert).|

## <a name="add-a-web-application-firewall-to-an-existing-application-gateway"></a>Hinzufügen einer Web Application Firewall zu einem vorhandenen Anwendungsgateway

Stellen Sie sicher, dass Sie die neueste Version von Azure PowerShell verwenden. Weitere Informationen finden Sie unter [Verwenden von Windows PowerShell mit Resource Manager](../powershell-azure-resource-manager.md).

1. Melden Sie sich beim Azure-Konto an.

    ```powershell
    Login-AzureRmAccount
    ```

2. Wählen Sie das Abonnement aus, das für dieses Szenario verwendet werden soll.

    ```powershell
    Select-AzureRmSubscription -SubscriptionName "<Subscription name>"
    ```

3. Rufen Sie das Gateway ab, dem Sie WAF hinzufügen möchten.

    ```powershell
    $gw = Get-AzureRmApplicationGateway -Name "AdatumGateway" -ResourceGroupName "MyResourceGroup"
    ```

4. Konfigurieren Sie die WAF-SKU. Die verfügbaren Größen sind **WAF\_Large** und **WAF\_Medium**. Wenn Sie WAF verwenden, muss die Ebene **WAF** sein. Überprüfen Sie die Kapazität, wenn Sie die SKU festlegen.

    ```powershell
    $gw | Set-AzureRmApplicationGatewaySku -Name WAF_Large -Tier WAF -Capacity 2
    ```

5. Konfigurieren Sie die WAF-Einstellungen, wie im folgenden Beispiel definiert. Für **FirewallMode** lauten die verfügbaren Werte **Prevention** und **Detection**.

    ```powershell
    $gw | Set-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode Prevention
    ```

6. Aktualisieren Sie das Anwendungsgateway mit den im vorherigen Schritt definierten Einstellungen.

    ```powershell
    Set-AzureRmApplicationGateway -ApplicationGateway $gw
    ```

Dieser Befehl aktualisiert das Anwendungsgateway mit WAF. Informationen zur Anzeige von Protokollen für Ihr Anwendungsgateway finden Sie unter [Application Gateway-Diagnose](application-gateway-diagnostics.md). Aufgrund der Art der WAF-Sicherheit müssen Sie die Protokolle regelmäßig überprüfen, um die Sicherheitslage Ihrer Webanwendungen zu kennen.

## <a name="create-an-application-gateway-with-a-web-application-firewall"></a>Erstellen eines Anwendungsgateways mit einer Web Application Firewall

Die folgenden Schritte führen Sie durch den gesamten Prozess zum Erstellen eines Anwendungsgateways mit WAF.

Stellen Sie sicher, dass Sie die neueste Version von Azure PowerShell verwenden. Weitere Informationen finden Sie unter [Verwenden von Windows PowerShell mit Resource Manager](../powershell-azure-resource-manager.md).

1. Melden Sie sich bei Azure an, indem Sie `Login-AzureRmAccount` ausführen. Sie werden zur Authentifizierung mit Ihren Anmeldeinformationen aufgefordert.

2. Überprüfen Sie die Abonnements für das Konto, indem Sie `Get-AzureRmSubscription` ausführen.

3. Wählen Sie das zu verwendende Azure-Abonnement aus.

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
> Falls Sie einen benutzerdefinierten Test für Ihr Anwendungsgateway konfigurieren müssen, finden Sie die entsprechenden Informationen unter [Erstellen eines Anwendungsgateways mit benutzerdefinierten Tests mithilfe von PowerShell](application-gateway-create-probe-ps.md). Weitere Informationen finden Sie unter [Benutzerdefinierte Tests und Systemüberwachung](application-gateway-probe-overview.md).

### <a name="configure-a-virtual-network"></a>Konfigurieren eines virtuellen Netzwerks

Für ein Anwendungsgateway ist ein eigenes Subnetz erforderlich. In diesem Schritt erstellen Sie ein virtuelles Netzwerk mit dem Adressraum 10.0.0.0/16 sowie zwei Subnetze: eines für das Anwendungsgateway und das andere für Mitglieder des Back-End-Pools.

```powershell
# Create a subnet configuration object for the application gateway subnet. A subnet for an application should have a minimum of 28 mask bits. This value leaves 10 available addresses in the subnet for application gateway instances. With a smaller subnet, you might not be able to add more instances of your application gateway in the future.
$gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24

# Create a subnet configuration object for the back-end pool members subnet.
$nicSubnet = New-AzureRmVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24

# Create the virtual network with the previously created subnets.
$vnet = New-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet
```

### <a name="configure-the-public-ip-address"></a>Konfigurieren der öffentlichen IP-Adresse

Um externe Anforderungen verarbeiten zu können, benötigt das Anwendungsgateway eine öffentliche IP-Adresse. Diese öffentliche IP-Adresse darf nicht `DomainNameLabel` definiert haben, um vom Anwendungsgateway verwendet zu werden.

```powershell
# Create a public IP address for use with the application gateway. Defining the `DomainNameLabel` during creation is not supported for use with the application gateway.
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name 'appgwpip' -Location "West US" -AllocationMethod Dynamic
```

### <a name="configure-the-application-gateway"></a>Konfigurieren des Anwendungsgateways

```powershell
# Create an IP configuration to configure which subnet the application gateway uses. When the application gateway starts, it picks up an IP address from the configured subnet and routes network traffic to the IP addresses in the back-end IP pool.
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet

# Create a back-end pool to hold the addresses or NIC handles for the application that the application gateway is protecting.
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 1.1.1.1, 2.2.2.2, 3.3.3.3

# Upload the authentication certificate to be used to communicate with the back-end servers.
$authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name 'whitelistcert1' -CertificateFile <full path to .cer file>

# Configure the back-end HTTP settings to be used to define how traffic is routed to the back-end pool. The authentication certificate used in the previous step is added to the back-end HTTP settings.
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name 'setting01' -Port 443 -Protocol Https -CookieBasedAffinity Enabled -AuthenticationCertificates $authcert

# Create a front-end port to be used by the listener.
$fp = New-AzureRmApplicationGatewayFrontendPort -Name 'port01'  -Port 443

# Create a front-end IP configuration to associate the public IP address with the application gateway.
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip

# Configure the certificate for the application gateway. This certificate is used to decrypt and re-encrypt the traffic on the application gateway.
$cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path to .pfx file> -Password <password for certificate file>

# Create the HTTP listener for the application gateway. Assign the front-end IP configuration, port, and SSL certificate to use.
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert

# Create a load-balancer routing rule that configures the load balancer behavior. In this example, a basic round-robin rule is created.
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

# Configure the SKU of the application gateway.
$sku = New-AzureRmApplicationGatewaySku -Name WAF_Medium -Tier WAF -Capacity 2

# Define the SSL policy to use.
$policy = New-AzureRmApplicationGatewaySslPolicy -PolicyType Predefined -PolicyName AppGwSslPolicy20170401S

# Configure the WAF configuration settings.
$config = New-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode "Prevention"

# Create the application gateway by using all the previously created configuration objects.
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -WebApplicationFirewallConfig $config -SslCertificates $cert -AuthenticationCertificates $authcert
```

> [!NOTE]
> Anwendungsgateways, die mit der WAF-Basiskonfiguration erstellt wurden, werden zum Schutz mit CRS 3.0 konfiguriert.

## <a name="get-an-application-gateway-dns-name"></a>Abrufen des DNS-Namens eines Anwendungsgateways

Nach dem Erstellen des Gateways wird das Front-End für die Kommunikation konfiguriert. Wenn Sie eine öffentliche IP-Adresse verwenden, wird für das Anwendungsgateway ein dynamisch zugewiesener DNS-Name benötigt. Dieser Name ist nicht benutzerfreundlich. Um zu gewährleisten, dass Benutzer auf das Anwendungsgateway zugreifen können, verweisen Sie mit einem CNAME-Eintrag auf den öffentlichen Endpunkt des Anwendungsgateways. Weitere Informationen finden Sie unter [Konfigurieren eines benutzerdefinierten Domänennamens für einen Azure-Clouddienst](../cloud-services/cloud-services-custom-domain-name-portal.md). 

Rufen Sie zum Konfigurieren eines Alias mithilfe des PublicIPAddress-Elements, das an das Anwendungsgateway angefügt ist, Details zum Anwendungsgateway sowie die zugeordnete IP-Adresse/den zugeordneten DNS-Namen ab. Verwenden Sie den DNS-Namen des Anwendungsgateways zum Erstellen eines CNAME-Eintrags, der die beiden Webanwendungen an diesen DNS-Namen verweist. Die Verwendung von A-Einträgen wird nicht empfohlen, da sich die VIP beim Neustart des Anwendungsgateways möglicherweise ändert.

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

Informationen zum Konfigurieren der Diagnoseprotokollierung zum Protokollieren von Ereignissen, die mit WAF erkannt oder verhindert werden, finden Sie unter [Application Gateway-Diagnose](application-gateway-diagnostics.md).

[scenario]: ./media/application-gateway-web-application-firewall-powershell/scenario.png

