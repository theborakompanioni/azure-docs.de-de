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
ms.date: 03/22/2017
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 432752c895fca3721e78fb6eb17b5a3e5c4ca495
ms.openlocfilehash: 52b7728c3fc702e37f5c5fe3d6544117a11464e8
ms.lasthandoff: 03/30/2017


---
# <a name="configure-web-application-firewall-on-a-new-or-existing-application-gateway"></a>Konfigurieren der Web Application Firewall auf einem neuen oder vorhandenen Anwendungsgateway

> [!div class="op_single_selector"]
> * [Azure-Portal](application-gateway-web-application-firewall-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-web-application-firewall-powershell.md)

Die Web Application Firewall (WAF) in Azure Application Gateway schützt Webanwendungen vor gängigen webbasierten Angriffen wie der Einschleusung von SQL-Code, Angriffen durch websiteübergreifende Skripts und der Übernahme von Sitzungen.

Azure Application Gateway verwendet einen Load Balancer auf der Schicht 7 (Anwendungsschicht). Das Application Gateway ermöglicht ein Failover sowie schnelles Routing von HTTP-Anforderungen zwischen verschiedenen Servern in der Cloud und der lokalen Umgebung. Application Gateway bietet zahlreiche Application Delivery Controller-Funktionen (ADC), u.a. HTTP-Lastenausgleich, cookiebasierte Sitzungsaffinität, SSL-Auslagerung (Secure Sockets Layer), benutzerdefinierte Integritätstests und Unterstützung für mehrere Websites. Eine vollständige Liste der unterstützten Features finden Sie unter „Übersicht über Application Gateway“.

Der folgende Artikel zeigt, wie Sie die [Web Application Firewall einem vorhandenen Anwendungsgateway hinzufügen](#add-web-application-firewall-to-an-existing-application-gateway) und [ein Anwendungsgateway erstellen](#create-an-application-gateway-with-web-application-firewall), das die Web Application Firewall verwendet.

![Szenarioabbildung][scenario]

## <a name="waf-configuration-differences"></a>Unterschiede bei der WAF-Konfiguration

Wenn Sie [Erstellen eines Anwendungsgateway mit PowerShell](application-gateway-create-gateway-arm.md)gelesen haben, kennen Sie die SKU-Einstellungen, die beim Erstellen eines Anwendungsgateways konfiguriert werden müssen. Die WAF bietet zusätzliche Einstellungen, die Sie beim Konfigurieren der SKU auf einem Anwendungsgateway definieren müssen. Am Anwendungsgateway selbst sind keine zusätzlichen Änderungen erforderlich.

**SKU**: Ein normales Anwendungsgateway ohne WAF unterstützt die Größen **Standard\_Small**, **Standard\_Medium** und **Standard\_Large**. Mit der Einführung der WAF gibt es zwei zusätzliche SKUs: **WAF\_Medium** und **WAF\_Large**. Die WAF wird auf kleinen Anwendungsgateways nicht unterstützt.

**Tier**: Verfügbare Werte sind **Standard** oder **WAF**. Bei Verwendung der Web Application Firewall muss **WAF** ausgewählt werden.

**Mode** : Diese Einstellung ist der Modus der WAF. Zulässige Werte sind **Detection** (Erkennung) und **Prevention** (Schutz). Wenn die WAF im Erkennungsmodus eingerichtet wird, werden alle Bedrohungen in einer Protokolldatei gespeichert. Im Schutzmodus werden Ereignisse ebenfalls protokolliert, aber der Angreifer erhält zudem die Antwort 403 (nicht autorisiert) vom Anwendungsgateway.

## <a name="add-web-application-firewall-to-an-existing-application-gateway"></a>Hinzufügen der Web Application Firewall zu einem vorhandenen Anwendungsgateway

Stellen Sie sicher, dass Sie die neueste Version von Azure PowerShell verwenden. Weitere Informationen finden Sie unter [Verwenden von Windows PowerShell mit Resource Manager](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Schritt 1

Melden Sie sich beim Azure-Konto an.

```powershell
Login-AzureRmAccount
```

### <a name="step-2"></a>Schritt 2

Wählen Sie das Abonnement aus, das für dieses Szenario verwendet werden soll.

```powershell
Select-AzureRmSubscription -SubscriptionName "<Subscription name>"
```

### <a name="step-3"></a>Schritt 3

Rufen Sie das Gateway ab, dem Sie die Web Application Firewall hinzufügen möchten.

```powershell
$gw = Get-AzureRmApplicationGateway -Name "AdatumGateway" -ResourceGroupName "MyResourceGroup"
```

### <a name="step-4"></a>Schritt 4

Konfigurieren Sie die Web Application Firewall-SKU. Die verfügbaren Größen sind **WAF\_Large** und **WAF\_Medium**. Bei Verwendung von Web Application Firewall muss die Ebene **WAF** ausgewählt sein, und beim Einrichten der SKU muss die Kapazität bestätigt werden.

```powershell
$gw | Set-AzureRmApplicationGatewaySku -Name WAF_Large -Tier WAF -Capacity 2
```

### <a name="step-5"></a>Schritt 5

Konfigurieren Sie die WAF-Einstellungen, wie im folgenden Beispiel definiert:

Für **WafMode** lauten die verfügbaren Werte „Prevention“ und „Detection“.

```powershell
$gw | Set-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode Prevention
```

### <a name="step-6"></a>Schritt 6

Aktualisieren Sie das Anwendungsgateway mit den im vorherigen Schritt definierten Einstellungen.

```powershell
Set-AzureRmApplicationGateway -ApplicationGateway $gw
```

Dieser Befehl aktualisiert das Anwendungsgateway mit der Web Application Firewall. Es wird empfohlen, die [Application Gateway-Diagnose](application-gateway-diagnostics.md) anzuzeigen, um die Anzeige von Protokollen für Ihr Anwendungsgateway nachzuvollziehen. Aufgrund der Art der WAF-Sicherheit müssen Protokolle regelmäßig überprüft werden, um sich über die Sicherheitslage Ihrer Webanwendungen zu informieren.

## <a name="create-an-application-gateway-with-web-application-firewall"></a>Erstellen eines Anwendungsgateways mit der Web Application Firewall

Die folgenden Schritte führen Sie durch den gesamten Prozess zum Erstellen eines Anwendungsgateways mit Web Application Firewall.

Stellen Sie sicher, dass Sie die neueste Version von Azure PowerShell verwenden. Weitere Informationen finden Sie unter [Verwenden von Windows PowerShell mit Resource Manager](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Schritt 1

Anmelden an Azure

```powershell
Login-AzureRmAccount
```

Sie werden zur Authentifizierung mit Ihren Anmeldeinformationen aufgefordert.

### <a name="step-2"></a>Schritt 2

Überprüfen Sie die Abonnements für das Konto.

```powershell
Get-AzureRmSubscription
```

### <a name="step-3"></a>Schritt 3

Wählen Sie aus, welches Azure-Abonnement Sie verwenden möchten.

```powershell
Select-AzureRmsubscription -SubscriptionName "<Subscription name>"
```

### <a name="step-4"></a>Schritt 4

Erstellen Sie eine Ressourcengruppe. (Überspringen Sie diesen Schritt, wenn Sie eine vorhandene Ressourcengruppe verwenden.)

```powershell
New-AzureRmResourceGroup -Name appgw-rg -Location "West US"
```

Der Azure Resource Manager erfordert, dass alle Ressourcengruppen einen Speicherort angeben. Dieser wird als Standardspeicherort für Ressourcen in dieser Ressourcengruppe verwendet. Stellen Sie sicher, dass alle Befehle, mit denen ein Anwendungsgateway erstellt wird, die gleiche Ressourcengruppe verwenden.

Im obigen Beispiel haben wir eine Ressourcengruppe namens „appgw-RG“ mit dem Standort „USA, Westen“ erstellt.

> [!NOTE]
> Wenn Sie einen benutzerdefinierten Test für Ihr Anwendungsgateway konfigurieren müssen, finden Sie unter [Erstellen eines benutzerdefinierten Tests für Azure Application Gateway mithilfe von PowerShell für Azure-Ressourcen-Manager](application-gateway-create-probe-ps.md)weitere Informationen. Weitere Informationen finden Sie unter [Benutzerdefinierte Tests und Systemüberwachung](application-gateway-probe-overview.md) .

### <a name="step-5"></a>Schritt 5

Weisen Sie einen Adressbereich für das Subnetz zu, das für das Anwendungsgateway verwendet werden soll.

```powershell
$gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24
```

> [!NOTE]
> Ein Subnetz für eine Anwendung sollte mindestens 28 Maskenbits aufweisen. Bei diesem Wert verbleiben 10 verfügbare Adressen im Subnetz für Application Gateway-Instanzen. Mit einem kleineren Subnetz können Sie später möglicherweise keine weiteren Instanzen Ihres Anwendungsgateways hinzufügen.


### <a name="step-6"></a>Schritt 6

Weisen Sie einen Adressbereich zu, der für den Back-End-Adresspool verwendet werden soll.

```powershell
$nicSubnet = New-AzureRmVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24
```

### <a name="step-7"></a>Schritt 7

Erstellen Sie ein virtuelles Netzwerk mit den vorhergehenden Subnetzen in der Ressourcengruppe, die Sie im Schritt [Erstellen der Ressourcengruppe](#create-the-resource-group)

```powershell
$vnet = New-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet
```

### <a name="step-8"></a>Schritt 8

Rufen Sie die virtuelle Netzwerkressource und die Subnetzressourcen ab, die in den folgenden Schritten verwendet werden sollen:

```powershell
$vnet = Get-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg
$gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -VirtualNetwork $vnet
$nicSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appsubnet' -VirtualNetwork $vnet
```

### <a name="step-9"></a>Schritt 9

Erstellen Sie eine öffentliche IP-Ressource, die für das Anwendungsgateway verwendet werden soll. Diese öffentliche IP-Adresse wird in einem der folgenden Schritte verwendet:

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name 'appgwpip' -Location "West US" -AllocationMethod Dynamic
```

> [!IMPORTANT]
> Application Gateway unterstützt nicht die Verwendung einer öffentlichen IP-Adresse, die mit einer definierten Domänenbezeichnung erstellt wird. Nur eine öffentliche IP-Adresse mit einer dynamisch erstellten Domänenbezeichnung wird unterstützt. Wenn Sie einen benutzerfreundlichen DNS-Namen für das Anwendungsgateway benötigen, wird empfohlen, einen cname-Datensatz als Alias zu verwenden.


### <a name="step-10"></a>Schritt 10

Sie müssen alle Konfigurationselemente einrichten, bevor Sie das Anwendungsgateway erstellen. Die folgenden Schritten erstellen die Konfigurationselemente, die für eine Application Gateway-Ressource benötigt werden.

Erstellen Sie eine Anwendungsgateway-IP-Konfiguration. So wird konfiguriert, welches Subnetz vom Anwendungsgateway verwendet wird. Beim Starten des Anwendungsgateways wird eine IP-Adresse aus dem konfigurierten Subnetz ausgewählt, und der Netzwerkdatenverkehr wird an die IP-Adressen im Back-End-IP-Pool weitergeleitet. Beachten Sie, dass jede Instanz eine eigene IP-Adresse benötigt.

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet
```

### <a name="step-11"></a>Schritt 11

Konfigurieren Sie den Back-End-IP-Adresspool mit den IP-Adressen der Back-End-Webserver. Dies sind die IP-Adressen, die den Netzwerkdatenverkehr vom Front-End-IP-Endpunkt empfangen. Sie ersetzen die folgenden IP-Adressen durch die IP-Adressendpunkte Ihrer eigenen Anwendung.

```powershell
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 1.1.1.1, 2.2.2.2, 3.3.3.3
```

### <a name="step-12"></a>Schritt 12

Laden Sie das Zertifikat hoch, das auf den SSL-fähigen Ressourcen des Back-End-Pools verwendet werden soll.

```powershell
$authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name 'whitelistcert1' -CertificateFile <full path to .cer file>
```

### <a name="step-13"></a>Schritt 13

Konfigurieren Sie die HTTP-Einstellungen auf dem Back-End des Anwendungsgateways. Weisen Sie den HTTP-Einstellungen das Zertifikat zu, das im vorherigen Schritt hochgeladen wurde.

```powershell
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name 'setting01' -Port 443 -Protocol Https -CookieBasedAffinity Enabled -AuthenticationCertificates $authcert
```

### <a name="step-14"></a>Schritt 14

Konfigurieren Sie den Front-End-IP-Port für den öffentlichen IP-Endpunkt. Dieser Port ist der Port, mit dem Endbenutzer eine Verbindung herstellen.

```powershell
$fp = New-AzureRmApplicationGatewayFrontendPort -Name 'port01'  -Port 443
```

### <a name="step-15"></a>Schritt 15

Erstellen Sie eine Front-End-IP-Konfiguration. Diese Einstellung ordnet dem Front-End des Anwendungsgateways eine private oder öffentliche IP-Adresse zu. Im folgenden Schritt wird die öffentliche IP-Adresse aus dem vorherigen Schritt der Front-End-IP-Konfiguration zugeordnet.

```powershell
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip
```

### <a name="step-16"></a>Schritt 16

Konfigurieren Sie das Zertifikat für das Anwendungsgateway. Dieses Zertifikat wird zum Entschlüsseln und erneuten Verschlüsseln des Datenverkehrs auf dem Anwendungsgateway verwendet.

```powershell
$cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path to .pfx file> -Password <password for certificate file>
```

### <a name="step-17"></a>Schritt 17

Erstellen Sie den HTTP-Listener für das Anwendungsgateway. Weisen Sie die zu verwendende Front-End-IP-Konfiguration, den Port und das SSL-Zertifikat zu.

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert
```

### <a name="step-18"></a>Schritt 18

Erstellen Sie eine Routingregel für den Lastenausgleich, durch die das Verhalten des Lastenausgleichs konfiguriert wird. In diesem Beispiel wird eine Roundrobin-Basisregel erstellt.

```powershell
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
```

### <a name="step-19"></a>Schritt 19

Konfigurieren Sie die Instanzgröße des Anwendungsgateways.

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name WAF_Medium -Tier WAF -Capacity 2
```

> [!NOTE]
> Sie können zwischen **WAF\_Medium** und **WAF\_Large** wählen. Der Tarif lautet bei Verwendung der WAF immer **WAF**. Die Kapazität ist eine beliebige Zahl zwischen 1 und 10.

### <a name="step-20"></a>Schritt 20

Konfigurieren Sie den Modus für die WAF. Die zulässigen Werte lauten **Prevention** (Schutz) und **Detection** (Erkennung).

```powershell
$config = New-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode "Prevention"
```

### <a name="step-21"></a>Schritt 21

Erstellen Sie ein Anwendungsgateway mit allen Konfigurationselementen aus den vorherigen Schritten. In diesem Beispiel heißt das Anwendungsgateway „appgwtest“.

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -WebApplicationFirewallConfig $config -SslCertificates $cert -AuthenticationCertificates $authcert
```

> [!NOTE]
> Anwendungsgateways, die mit der Basiskonfiguration der Web Application Firewall erstellt wurden, werden zum Schutz mit CRS 3.0 konfiguriert.

## <a name="get-application-gateway-dns-name"></a>Abrufen des DNS-Namens des Anwendungsgateways

Nach dem Erstellen des Gateways wird das Front-End für die Kommunikation konfiguriert. Wenn Sie eine öffentliche IP-Adresse verwenden, wird das Anwendungsgateway ein dynamisch zugewiesener DNS-Namen benötigt, der kein Anzeigename ist. Um sicherzustellen, dass die Endbenutzer auf das Anwendungsgateway zugreifen können, kann mit einem CNAME-Eintrag auf den öffentlichen Endpunkt des Anwendungsgateways verwiesen werden. [Konfigurieren eines benutzerdefinierten Domänennamens in Azure](../cloud-services/cloud-services-custom-domain-name-portal.md) Rufen Sie hierzu mithilfe des PublicIPAddress-Elements, das an das Anwendungsgateway angefügt ist, Details zum Anwendungsgateway und den zugeordneten IP/DNS-Namen ab. Verwenden Sie den DNS-Namen des Anwendungsgateways zum Erstellen eines CNAME-Eintrags, der die beiden Webanwendungen an diesen DNS-Namen verweist. Die Verwendung von A-Einträgen wird nicht empfohlen, da sich die VIP beim Neustart des Anwendungsgateways möglicherweise verändert.

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

