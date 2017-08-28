---
title: "Konfigurieren der Web Application Firewall – Azure Application Gateway | Microsoft-Dokumentation"
description: "Dieser Artikel enthält Anleitungen zur Verwendung der Web Application Firewall für eine vorhandene oder neue Application Gateway-Instanz."
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
ms.sourcegitcommit: 1e6fb68d239ee3a66899f520a91702419461c02b
ms.openlocfilehash: dab489a1c9fb7d4a51b9ccbce543b209bec23575
ms.contentlocale: de-de
ms.lasthandoff: 08/16/2017

---
# <a name="configure-web-application-firewall-on-a-new-or-existing-application-gateway"></a>Konfigurieren der Web Application Firewall auf einem neuen oder vorhandenen Anwendungsgateway

> [!div class="op_single_selector"]
> * [Azure-Portal](application-gateway-web-application-firewall-portal.md)
> * [PowerShell](application-gateway-web-application-firewall-powershell.md)
> * [Azure-CLI](application-gateway-web-application-firewall-cli.md)

Hier erfahren Sie, wie Sie eine Application Gateway-Instanz mit Web Application Firewall erstellen oder einer vorhandenen Application Gateway-Instanz eine Web Application Firewall hinzufügen.

Die Web Application Firewall (WAF) in Azure Application Gateway schützt Webanwendungen vor gängigen webbasierten Angriffen wie der Einschleusung von SQL-Code, Angriffen durch websiteübergreifende Skripts und der Übernahme von Sitzungen.

Azure Application Gateway verwendet einen Load Balancer auf der Schicht 7 (Anwendungsschicht). Das Application Gateway ermöglicht ein Failover sowie schnelles Routing von HTTP-Anforderungen zwischen verschiedenen Servern in der Cloud und der lokalen Umgebung. Application Gateway bietet zahlreiche ADC-Features (Application Delivery Controller) wie HTTP-Lastenausgleich, cookiebasierte Sitzungsaffinität, SSL-Auslagerung (Secure Sockets Layer), benutzerdefinierte Integritätstests und Unterstützung mehrerer Standorte. Eine vollständige Liste der unterstützten Features finden Sie unter [Übersicht über das Application Gateway](application-gateway-introduction.md).

Der folgende Artikel zeigt, wie Sie die [Web Application Firewall einer vorhandenen Application Gateway-Instanz hinzufügen](#add-web-application-firewall-to-an-existing-application-gateway) und [eine Application Gateway-Instanz erstellen, die die Web Application Firewall verwendet](#create-an-application-gateway-with-web-application-firewall).

![Szenarioabbildung][scenario]

## <a name="waf-configuration-differences"></a>Unterschiede bei der WAF-Konfiguration

Wenn Sie [Erstellen eines Anwendungsgateway mit PowerShell](application-gateway-create-gateway-arm.md) gelesen haben, sind Sie bereits mit den SKU-Einstellungen vertraut, die beim Erstellen einer Application Gateway-Instanz konfiguriert werden müssen. Die WAF bietet zusätzliche Einstellungen, die Sie beim Konfigurieren der SKU für eine Application Gateway-Instanz definieren müssen. Für die Application Gateway-Instanz selbst sind keine zusätzlichen Änderungen erforderlich.

| **Einstellung** | **Details**
|---|---|
|**SKU** |Eine normale Application Gateway-Instanz ohne WAF unterstützt die Größen **Standard\_Small**, **Standard\_Medium** und **Standard\_Large**. Mit der Einführung der WAF gibt es zwei zusätzliche SKUs: **WAF\_Medium** und **WAF\_Large**. Die WAF wird für kleine Application Gateway-Instanzen nicht unterstützt.|
|**Tier** | Verfügbare Werte sind **Standard** oder **WAF**. Bei Verwendung der Web Application Firewall muss **WAF** ausgewählt werden.|
|**Mode** | Diese Einstellung ist der Modus der WAF. Zulässige Werte sind **Detection** (Erkennung) und **Prevention** (Schutz). Wenn die WAF im Erkennungsmodus eingerichtet wird, werden alle Bedrohungen in einer Protokolldatei gespeichert. Im Schutzmodus werden Ereignisse ebenfalls protokolliert, der Angreifer erhält von der Application Gateway-Instanz jedoch zusätzlich die Antwort 403 (nicht autorisiert).|

## <a name="add-web-application-firewall-to-an-existing-application-gateway"></a>Hinzufügen der Web Application Firewall zu einer vorhandenen Application Gateway-Instanz

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

1. Aktualisieren Sie die Application Gateway-Instanz mit den im vorherigen Schritt definierten Einstellungen.

    ```powershell
    Set-AzureRmApplicationGateway -ApplicationGateway $gw
    ```

Dieser Befehl aktualisiert die Application Gateway-Instanz mit der Web Application Firewall. Besuchen Sie [Application Gateway-Diagnose](application-gateway-diagnostics.md), um die Anzeige von Protokollen für Ihre Application Gateway-Instanz nachzuvollziehen. Aufgrund der Art der WAF-Sicherheit müssen Protokolle regelmäßig überprüft werden, um sich über die Sicherheitslage Ihrer Webanwendungen zu informieren.

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

Erstellen Sie eine Ressourcengruppe für die Application Gateway-Instanz.

```powershell
New-AzureRmResourceGroup -Name appgw-rg -Location "West US"
```

Der Azure Resource Manager erfordert, dass alle Ressourcengruppen einen Speicherort angeben. Dieser wird als Standardspeicherort für Ressourcen in dieser Ressourcengruppe verwendet. Achten Sie darauf, dass bei allen Befehlen für die Erstellung einer Application Gateway-Instanz die gleiche Ressourcengruppe verwendet wird.

Im obigen Beispiel haben wir eine Ressourcengruppe namens „appgw-RG“ mit dem Standort „USA, Westen“ erstellt.

> [!NOTE]
> Falls Sie einen benutzerdefinierten Test für Ihre Application Gateway-Instanz konfigurieren müssen, finden Sie die entsprechenden Informationen unter [Erstellen eines Anwendungsgateways mit benutzerdefinierten Tests mithilfe von PowerShell](application-gateway-create-probe-ps.md). Weitere Informationen finden Sie unter [Benutzerdefinierte Tests und Systemüberwachung](application-gateway-probe-overview.md) .

### <a name="configure-virtual-network"></a>Konfigurieren des virtuellen Netzwerks

Für Application Gateway ist ein eigenes Subnetz erforderlich. In diesem Schritt erstellen Sie ein virtuelles Netzwerk mit einem Adressbereich von 10.0.0.0/16 sowie zwei Subnetze: eines für die Application Gateway-Instanz und das andere für Back-End-Poolmitglieder.

```powershell
# Create a subnet configuration object for the Application Gateway subnet. A subnet for an application should have a minimum of 28 mask bits. This value leaves 10 available addresses in the subnet for Application Gateway instances. With a smaller subnet, you may not be able to add more instance of your Application Gateway in the future.
$gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24

# Create a subnet configuration object for the backend pool members subnet
$nicSubnet = New-AzureRmVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24

# Create the virtual network with the previous created subnets
$vnet = New-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet
```

### <a name="configure-public-ip-address"></a>Konfigurieren der öffentlichen IP-Adresse

Um externe Anforderungen verarbeiten zu können, benötigt die Application Gateway-Instanz eine öffentliche IP-Adresse. Für diese öffentliche IP-Adresse darf kein `DomainNameLabel`-Wert definiert sein, um von der Application Gateway-Instanz verwendet werden zu können.

```powershell
# Create a public IP address for use with the Application Gateway. Defining the domainnamelabel during creation is not supported for use with Application Gateway
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name 'appgwpip' -Location "West US" -AllocationMethod Dynamic
```

### <a name="configure-the-application-gateway"></a>Konfigurieren der Application Gateway-Instanz

```powershell
# Create a IP configuration. This configures what subnet the Application Gateway uses. When Application Gateway starts, it picks up an IP address from the subnet configured and routes network traffic to the IP addresses in the back-end IP pool.
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet

# Create a backend pool to hold the addresses or NICs for the application that Application Gateway is protecting.
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 1.1.1.1, 2.2.2.2, 3.3.3.3

# Upload the authenication certificate that will be used to communicate with the backend servers
$authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name 'whitelistcert1' -CertificateFile <full path to .cer file>

# Conifugre the backend HTTP settings to be used to define how traffic is routed to the backend pool. The authenication certificate used in the previous step is added to the backend http settings.
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name 'setting01' -Port 443 -Protocol Https -CookieBasedAffinity Enabled -AuthenticationCertificates $authcert

# Create a frontend port to be used by the listener.
$fp = New-AzureRmApplicationGatewayFrontendPort -Name 'port01'  -Port 443

# Create a frontend IP configuration to associate the public IP address with the Application Gateway
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip

# Configure the certificate for the Application Gateway. This certificate is used to decrypt and re-encrypt the traffic on the Application Gateway.
$cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path to .pfx file> -Password <password for certificate file>

# Create the HTTP listener for the Application Gateway. Assign the front-end ip configuration, port, and ssl certificate to use.
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert

#Create a load balancer routing rule that configures the load balancer behavior. In this example, a basic round robin rule is created.
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

# Configure the SKU of the Application Gateway
$sku = New-AzureRmApplicationGatewaySku -Name WAF_Medium -Tier WAF -Capacity 2

# Define the SSL policy to use
$policy = New-AzureRmApplicationGatewaySslPolicy -PolicyType Predefined -PolicyName AppGwSslPolicy20170401S

#Configure the waf configuration settings.
$config = New-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode "Prevention"

# Create the Application Gateway utilizing all the previously created configuration objects
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -WebApplicationFirewallConfig $config -SslCertificates $cert -AuthenticationCertificates $authcert
```

> [!NOTE]
> Application Gateway-Instanzen, die mit der Basiskonfiguration der Web Application Firewall erstellt wurden, werden zum Schutz mit CRS 3.0 konfiguriert.

## <a name="get-application-gateway-dns-name"></a>Abrufen des DNS-Namens für die Application Gateway-Instanz

Nach dem Erstellen des Gateways wird das Front-End für die Kommunikation konfiguriert. Bei Verwendung einer öffentlichen IP-Adresse wird für die Application Gateway-Instanz ein dynamisch zugewiesener DNS-Namen benötigt. Dieser Name ist nicht benutzerfreundlich. Um zu gewährleisten, dass Endbenutzer auf die Application Gateway-Instanz zugreifen können, kann mit einem CNAME-Eintrag auf den öffentlichen Endpunkt der Application Gateway-Instanz verwiesen werden. [Konfigurieren eines benutzerdefinierten Domänennamens in Azure](../cloud-services/cloud-services-custom-domain-name-portal.md) Rufen Sie zum Konfigurieren des Alias mithilfe des PublicIPAddress-Elements, das an die Application Gateway-Instanz angefügt ist, Details zur Application Gateway-Instanz sowie die zugeordnete IP-Adresse/den zugeordneten DNS-Namen ab. Verwenden Sie den DNS-Namen der Application Gateway-Instanz zum Erstellen eines CNAME-Eintrags, der die beiden Webanwendungen an diesen DNS-Namen verweist. Die Verwendung von A-Einträgen wird nicht empfohlen, da sich die VIP beim Neustart der Application Gateway-Instanz möglicherweise ändert.

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

