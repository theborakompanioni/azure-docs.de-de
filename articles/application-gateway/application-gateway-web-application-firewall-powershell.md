---
title: Konfigurieren der Web Application Firewall auf einem neuen oder vorhandenen Anwendungsgateway | Microsoft Docs
description: Dieser Artikel enthält Anleitungen zur Verwendung der Web Application Firewall auf einem vorhandenen oder neuen Anwendungsgateway.
documentationcenter: na
services: application-gateway
author: georgewallace
manager: carmonm
editor: tysonn

ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/26/2016
ms.author: gwallace

---
# <a name="configure-web-application-firewall-on-a-new-or-existing-application-gateway"></a>Konfigurieren der Web Application Firewall auf einem neuen oder vorhandenen Anwendungsgateway
> [!div class="op_single_selector"]
> * [Azure-Portal](application-gateway-web-application-firewall-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-web-application-firewall-powershell.md)
> 
> 

Das Azure-Anwendungsgateway verwendet einen Load Balancer auf der Schicht 7 (Anwendungsschicht). Das Application Gateway ermöglicht ein Failover sowie schnelles Routing von HTTP-Anforderungen zwischen verschiedenen Servern in der Cloud und der lokalen Umgebung. Application Gateway bietet zahlreiche Application Delivery Controller-Funktionen (ADC), u.a. HTTP-Lastenausgleich, cookiebasierte Sitzungsaffinität, SSL-Auslagerung (Secure Sockets Layer), benutzerdefinierte Integritätstests und Unterstützung für mehrere Websites. Eine vollständige Liste der unterstützten Features finden Sie unter „Übersicht über Application Gateway“.

Die Web Application Firewall (WAF) in Azure Application Gateway schützt Webanwendungen vor gängigen webbasierten Angriffen wie der Einschleusung von SQL-Code, Angriffen durch websiteübergreifende Skripts und der Übernahme von Sitzungen.

Der folgende Artikel zeigt, wie Sie die [Web Application Firewall einem vorhandenen Anwendungsgateway hinzufügen](#add-web-application-firewall-to-an-existing-application-gateway) und [ein Anwendungsgateway erstellen](#create-an-application-gateway-with-web-application-firewall), das die Web Application Firewall verwendet.

![Szenarioabbildung][scenario]

## <a name="waf-configuration-differences"></a>Unterschiede bei der WAF-Konfiguration
Wenn Sie [Erstellen eines Anwendungsgateway mit PowerShell](application-gateway-create-gateway-arm.md)gelesen haben, kennen Sie die SKU-Einstellungen, die beim Erstellen eines Anwendungsgateways konfiguriert werden müssen. Die WAF bietet zusätzliche Einstellungen, die Sie beim Konfigurieren der SKU auf einem Anwendungsgateway definieren müssen. Am Anwendungsgateway selbst sind keine zusätzlichen Änderungen erforderlich.

**SKU**: Ein normales Anwendungsgateway ohne WAF unterstützt die Größen **Standard\_Small**, **Standard\_Medium** und **Standard\_Large**. Mit der Einführung der WAF gibt es zwei zusätzliche SKUs: **WAF\_Medium** und **WAF\_Large**. Die WAF wird auf kleinen Anwendungsgateways nicht unterstützt.

**Tier**: Verfügbare Werte sind **Standard** oder **WAF**. Bei Verwendung der Web Application Firewall muss **WAF** ausgewählt werden.

**Mode** : Diese Einstellung ist der Modus der WAF. Zulässige Werte sind **Detection** (Erkennung) und **Prevention** (Schutz). Wenn die WAF im Erkennungsmodus eingerichtet wird, werden alle Bedrohungen in einer Protokolldatei gespeichert. Im Schutzmodus werden Ereignisse ebenfalls protokolliert, aber der Angreifer erhält zudem die Antwort 403 (nicht autorisiert) vom Anwendungsgateway.

## <a name="add-web-application-firewall-to-an-existing-application-gateway"></a>Hinzufügen der Web Application Firewall zu einem vorhandenen Anwendungsgateway
Stellen Sie sicher, dass Sie die neueste Version von Azure PowerShell verwenden. Weitere Informationen finden Sie unter [Verwenden von Windows PowerShell mit Resource Manager](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Schritt 1
Melden Sie sich beim Azure-Konto an.

    Login-AzureRmAccount

### <a name="step-2"></a>Schritt 2
Wählen Sie das Abonnement aus, das für dieses Szenario verwendet werden soll.

    Select-AzureRmSubscription -SubscriptionName "<Subscription name>"

### <a name="step-3"></a>Schritt 3
Rufen Sie das Gateway ab, dem Sie die Web Application Firewall hinzufügen möchten.

    $gw = Get-AzureRmApplicationGateway -Name "AdatumGateway" -ResourceGroupName "MyResourceGroup"


### <a name="step-4"></a>Schritt 4
Konfigurieren Sie die Web Application Firewall-SKU. Die verfügbaren Größen sind **WAF\_Large** und **WAF\_Medium**. Bei Verwendung der Web Application Firewall muss der Tarif **WAF**lauten.

    $gw | Set-AzureRmApplicationGatewaySku -Name WAF_Large -Tier WAF

### <a name="step-5"></a>Schritt 5
Konfigurieren Sie die WAF-Einstellungen, wie im folgenden Beispiel definiert:

Für **WafMode** lauten die verfügbaren Werte „Prevention“ und „Detection“.

    $config = Add-AzureRmApplicationGatewayWafConfig -Enabled $true -WafMode "Prevention" -ApplicationGateway $gw

### <a name="step-6"></a>Schritt 6
Aktualisieren Sie das Anwendungsgateway mit den im vorherigen Schritt definierten Einstellungen.

    Set-AzureRmApplicationGateway -ApplicationGateway $gw

Dieser Befehl aktualisiert das Anwendungsgateway mit der Web Application Firewall. Es wird empfohlen, die [Application Gateway-Diagnose](application-gateway-diagnostics.md) anzuzeigen, um die Anzeige von Protokollen für Ihr Anwendungsgateway nachzuvollziehen. Aufgrund der Art der WAF-Sicherheit müssen Protokolle regelmäßig überprüft werden, um sich über die Sicherheitslage Ihrer Webanwendungen zu informieren.

## <a name="create-an-application-gateway-with-web-application-firewall"></a>Erstellen eines Anwendungsgateways mit der Web Application Firewall
Die folgenden Schritte führen Sie durch den gesamten Prozess zum Erstellen eines Anwendungsgateways mit Web Application Firewall.

Stellen Sie sicher, dass Sie die neueste Version von Azure PowerShell verwenden. Weitere Informationen finden Sie unter [Verwenden von Windows PowerShell mit Resource Manager](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Schritt 1
Anmelden an Azure

    Login-AzureRmAccount

Sie werden zur Authentifizierung mit Ihren Anmeldeinformationen aufgefordert.

### <a name="step-2"></a>Schritt 2
Überprüfen Sie die Abonnements für das Konto.

    Get-AzureRmSubscription

### <a name="step-3"></a>Schritt 3
Wählen Sie aus, welches Azure-Abonnement Sie verwenden möchten.

    Select-AzureRmsubscription -SubscriptionName "<Subscription name>"

### <a name="step-4"></a>Schritt 4
Erstellen Sie eine Ressourcengruppe. (Überspringen Sie diesen Schritt, wenn Sie eine vorhandene Ressourcengruppe verwenden.)

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

Der Azure Resource Manager erfordert, dass alle Ressourcengruppen einen Speicherort angeben. Dieser wird als Standardspeicherort für Ressourcen in dieser Ressourcengruppe verwendet. Stellen Sie sicher, dass alle Befehle, mit denen ein Anwendungsgateway erstellt wird, die gleiche Ressourcengruppe verwenden.

Im obigen Beispiel haben wir eine Ressourcengruppe namens „appgw-RG“ mit dem Standort „USA, Westen“ erstellt.

> [!NOTE]
> Wenn Sie einen benutzerdefinierten Test für Ihr Anwendungsgateway konfigurieren müssen, finden Sie unter [Erstellen eines benutzerdefinierten Tests für Azure Application Gateway mithilfe von PowerShell für Azure-Ressourcen-Manager](application-gateway-create-probe-ps.md)weitere Informationen. Weitere Informationen finden Sie unter [Benutzerdefinierte Tests und Systemüberwachung](application-gateway-probe-overview.md) .
> 
> 

### <a name="step-5"></a>Schritt 5
Weisen Sie einen Adressbereich für das Subnetz zu, das für das Anwendungsgateway verwendet werden soll.

    $gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24

> [!NOTE]
> Ein Subnetz für eine Anwendung sollte mindestens 28 Maskenbits aufweisen. Bei diesem Wert verbleiben 10 verfügbare Adressen im Subnetz für Application Gateway-Instanzen. Mit einem kleineren Subnetz können Sie später möglicherweise keine weiteren Instanzen Ihres Anwendungsgateways hinzufügen.
> 
> 

### <a name="step-6"></a>Schritt 6
Weisen Sie einen Adressbereich zu, der für den Back-End-Adresspool verwendet werden soll.

    $nicSubnet = New-AzureRmVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24

### <a name="step-7"></a>Schritt 7
Erstellen Sie ein virtuelles Netzwerk mit den vorhergehenden Subnetzen in der Ressourcengruppe, die Sie im Schritt [Erstellen der Ressourcengruppe](#create-the-resource-group)

    $vnet = New-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet

### <a name="step-8"></a>Schritt 8
Rufen Sie die virtuelle Netzwerkressource und die Subnetzressourcen ab, die in den folgenden Schritten verwendet werden sollen:

    $vnet = Get-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg
    $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -VirtualNetwork $vnet
    $nicSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appsubnet' -VirtualNetwork $vnet

### <a name="step-9"></a>Schritt 9
Erstellen Sie eine öffentliche IP-Ressource, die für das Anwendungsgateway verwendet werden soll. Diese öffentliche IP-Adresse wird in einem der folgenden Schritte verwendet:

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name 'appgwpip' -Location "West US" -AllocationMethod Dynamic

> [!IMPORTANT]
> Application Gateway unterstützt nicht die Verwendung einer öffentlichen IP-Adresse, die mit einer definierten Domänenbezeichnung erstellt wird. Nur eine öffentliche IP-Adresse mit einer dynamisch erstellten Domänenbezeichnung wird unterstützt. Wenn Sie einen benutzerfreundlichen DNS-Namen für das Anwendungsgateway benötigen, wird empfohlen, einen cname-Datensatz als Alias zu verwenden.
> 
> 

### <a name="step-10"></a>Schritt 10
Sie müssen alle Konfigurationselemente einrichten, bevor Sie das Anwendungsgateway erstellen. Die folgenden Schritten erstellen die Konfigurationselemente, die für eine Application Gateway-Ressource benötigt werden.

Erstellen Sie eine Anwendungsgateway-IP-Konfiguration. So wird konfiguriert, welches Subnetz vom Anwendungsgateway verwendet wird. Beim Starten des Anwendungsgateways wird eine IP-Adresse aus dem konfigurierten Subnetz ausgewählt, und der Netzwerkdatenverkehr wird an die IP-Adressen im Back-End-IP-Pool weitergeleitet. Beachten Sie, dass jede Instanz eine eigene IP-Adresse benötigt.

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet

### <a name="step-11"></a>Schritt 11
Konfigurieren Sie den Back-End-IP-Adresspool mit den IP-Adressen der Back-End-Webserver. Dies sind die IP-Adressen, die den Netzwerkdatenverkehr vom Front-End-IP-Endpunkt empfangen. Sie ersetzen die folgenden IP-Adressen durch die IP-Adressendpunkte Ihrer eigenen Anwendung.

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 1.1.1.1, 2.2.2.2, 3.3.3.3

### <a name="step-12"></a>Schritt 12
Konfigurieren Sie die HTTP-Einstellungen auf dem Back-End des Anwendungsgateways. Weisen Sie den HTTP-Einstellungen das Zertifikat zu, das im vorherigen Schritt hochgeladen wurde.

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name 'setting01' -Port 443 -Protocol Https -CookieBasedAffinity Enabled -AuthenticationCertificates $authcert

### <a name="step-13"></a>Schritt 13
Konfigurieren Sie den Front-End-IP-Port für den öffentlichen IP-Endpunkt. Dieser Port ist der Port, mit dem Endbenutzer eine Verbindung herstellen.

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name 'port01'  -Port 443

### <a name="step-14"></a>Schritt 14
Erstellen Sie eine Front-End-IP-Konfiguration. Diese Einstellung ordnet dem Front-End des Anwendungsgateways eine private oder öffentliche IP-Adresse zu. Im folgenden Schritt wird die öffentliche IP-Adresse aus dem vorherigen Schritt der Front-End-IP-Konfiguration zugeordnet.

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip

### <a name="step-15"></a>Schritt 15
Erstellen Sie den HTTP-Listener für das Anwendungsgateway. Weisen Sie die zu verwendende Front-End-IP-Konfiguration, den Port und das SSL-Zertifikat zu.

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert

### <a name="step-16"></a>Schritt 16
Erstellen Sie eine Routingregel für den Lastenausgleich, durch die das Verhalten des Lastenausgleichs konfiguriert wird. In diesem Beispiel wird eine Roundrobin-Basisregel erstellt.

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

### <a name="step-17"></a>Schritt 17
Konfigurieren Sie die Instanzgröße des Application Gateways.

    $sku = New-AzureRmApplicationGatewaySku -Name WAF_Medium -Tier WAF -Capacity 2

> [!NOTE]
> Sie können zwischen **WAF\_Medium** und **WAF\_Large** wählen. Der Tarif lautet bei Verwendung der WAF immer **WAF**. Die Kapazität ist eine beliebige Zahl zwischen 1 und 10.
> 
> 

### <a name="step-18"></a>Schritt 18
Konfigurieren Sie den Modus für die WAF. Die zulässigen Werte lauten **Prevention** (Schutz) und **Detection** (Erkennung).

    $config = New-AzureRmApplicationGatewayWafConfig -Enabled $true -WafMode "Prevention"

### <a name="step-19"></a>Schritt 19
Erstellen Sie ein Anwendungsgateway mit allen Konfigurationselementen aus den vorherigen Schritten. In diesem Beispiel heißt das Application Gateway „appgwtest“.

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -WafConfig $config

## <a name="next-steps"></a>Nächste Schritte
Informationen zum Konfigurieren der Diagnoseprotokollierung zum Protokollieren von Ereignissen, die mit der Web Application Firewall erkannt oder verhindert werden, finden Sie unter [Application Gateway-Diagnose](application-gateway-diagnostics.md)

[Szenario]: ./media/application-gateway-web-application-firewall-powershell/scenario.png


<!--HONumber=Oct16_HO2-->


