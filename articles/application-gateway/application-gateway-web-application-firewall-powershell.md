<properties
   pageTitle="Konfigurieren der Web Application Firewall auf einem neuen oder vorhandenen Anwendungsgateway | Microsoft Azure"
   description="Dieser Artikel enthält Anleitungen zur Verwendung der Web Application Firewall auf einem vorhandenen oder neuen Anwendungsgateway."
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/26/2016"
   ms.author="gwallace"/>

# Konfigurieren der Web Application Firewall auf einem neuen oder vorhandenen Anwendungsgateway

> [AZURE.SELECTOR]
- [Azure-Portal](application-gateway-web-application-firewall-portal.md)
- [Azure Resource Manager PowerShell](application-gateway-web-application-firewall-powershell.md)

Für das Azure Application Gateway wird ein Load Balancer auf Schicht 7 (Anwendungsschicht) verwendet. Das Application Gateway ermöglicht ein Failover sowie schnelles Routing von HTTP-Anforderungen zwischen verschiedenen Servern in der Cloud und der lokalen Umgebung. Application Gateway bietet zahlreiche Application Delivery Controller-Funktionen (ADC), u.a. HTTP-Lastenausgleich, cookiebasierte Sitzungsaffinität, SSL-Auslagerung (Secure Sockets Layer), benutzerdefinierte Integritätstests und Unterstützung für mehrere Websites. Eine vollständige Liste der unterstützten Features finden Sie unter „Übersicht über Application Gateway“.

Die Web Application Firewall (WAF) in Azure Application Gateway schützt Webanwendungen vor gängigen webbasierten Angriffen wie der Einschleusung von SQL-Code, Angriffen durch websiteübergreifende Skripts und der Übernahme von Sitzungen.

Der folgende Artikel zeigt, wie Sie die [Web Application Firewall einem vorhandenen Anwendungsgateway hinzufügen](#add-web-application-firewall-to-an-existing-application-gateway) und [ein Anwendungsgateway erstellen, das die Web Application Firewall verwendet](#create-an-application-gateway-with-web-application-firewall).

![Szenarioabbildung][scenario]

## Unterschiede bei der WAF-Konfiguration

Wenn Sie [Erstellen eines Anwendungsgateway mit PowerShell](application-gateway-create-gateway-arm.md) gelesen haben, kennen Sie die SKU-Einstellungen, die beim Erstellen eines Anwendungsgateways konfiguriert werden müssen. Die WAF bietet zusätzliche Einstellungen, die Sie beim Konfigurieren der SKU auf einem Anwendungsgateway definieren müssen. Am Anwendungsgateway selbst sind keine zusätzlichen Änderungen erforderlich.

**SKU**: Ein normales Anwendungsgateway ohne WAF unterstützt die Größen **Standard\_Small**, **Standard\_Medium** und **Standard\_Large**. Mit der Einführung der WAF gibt es zwei zusätzliche SKUs: **WAF\_Medium** und **WAF\_Large**. Die WAF wird auf kleinen Anwendungsgateways nicht unterstützt.

**Tier**: Verfügbare Werte sind **Standard** oder **WAF**. Bei Verwendung der Web Application Firewall muss **WAF** ausgewählt werden.

**Mode**: Diese Einstellung ist der Modus der WAF. Zulässige Werte sind **Detection** (Erkennung) und **Prevention** (Schutz). Wenn die WAF im Erkennungsmodus eingerichtet wird, werden alle Bedrohungen in einer Protokolldatei gespeichert. Im Schutzmodus werden Ereignisse ebenfalls protokolliert, aber der Angreifer erhält zudem die Antwort 403 (nicht autorisiert) vom Anwendungsgateway.

## Hinzufügen der Web Application Firewall zu einem vorhandenen Anwendungsgateway

Stellen Sie sicher, dass Sie die neueste Version von Azure PowerShell verwenden. Weitere Informationen finden Sie unter [Verwenden von Windows PowerShell mit Resource Manager](../powershell-azure-resource-manager.md).

### Schritt 1

Melden Sie sich beim Azure-Konto an.

    Login-AzureRmAccount

### Schritt 2

Wählen Sie das Abonnement aus, das für dieses Szenario verwendet werden soll.

    Select-AzureRmSubscription -SubscriptionName "<Subscription name>"

### Schritt 3

Rufen Sie das Gateway ab, dem Sie die Web Application Firewall hinzufügen möchten.

    $gw = Get-AzureRmApplicationGateway -Name "AdatumGateway" -ResourceGroupName "MyResourceGroup"


### Schritt 4

Konfigurieren Sie die Web Application Firewall-SKU. Die verfügbaren Größen sind **WAF\_Large** und **WAF\_Medium**. Bei Verwendung der Web Application Firewall muss der Tarif **WAF** lauten.

    $gw | Set-AzureRmApplicationGatewaySku -Name WAF_Large -Tier WAF

### Schritt 5

Konfigurieren Sie die WAF-Einstellungen, wie im folgenden Beispiel definiert:

Für **WafMode** lauten die verfügbaren Werte „Prevention“ und „Detection“.

    $config = Add-AzureRmApplicationGatewayWafConfig -Enabled $true -WafMode "Prevention" -ApplicationGateway $gw

### Schritt 6

Aktualisieren Sie das Anwendungsgateway mit den im vorherigen Schritt definierten Einstellungen.

    Set-AzureRmApplicationGateway -ApplicationGateway $gw

Dieser Befehl aktualisiert das Anwendungsgateway mit der Web Application Firewall. Es wird empfohlen, die [Application Gateway-Diagnose](application-gateway-diagnostics.md) anzuzeigen, um die Anzeige von Protokollen für Ihr Anwendungsgateway nachzuvollziehen. Aufgrund der Art der WAF-Sicherheit müssen Protokolle regelmäßig überprüft werden, um sich über die Sicherheitslage Ihrer Webanwendungen zu informieren.

## Erstellen eines Anwendungsgateways mit der Web Application Firewall

Die folgenden Schritte führen Sie durch den gesamten Prozess zum Erstellen eines Anwendungsgateways mit Web Application Firewall.

Stellen Sie sicher, dass Sie die neueste Version von Azure PowerShell verwenden. Weitere Informationen finden Sie unter [Verwenden von Windows PowerShell mit Resource Manager](../powershell-azure-resource-manager.md).

### Schritt 1

Anmelden an Azure

	Login-AzureRmAccount

Sie werden zur Authentifizierung mit Ihren Anmeldeinformationen aufgefordert.

### Schritt 2

Überprüfen Sie die Abonnements für das Konto.

	Get-AzureRmSubscription

### Schritt 3

Wählen Sie aus, welches Azure-Abonnement Sie verwenden möchten.

    Select-AzureRmsubscription -SubscriptionName "<Subscription name>"

### Schritt 4

Erstellen Sie eine Ressourcengruppe. (Überspringen Sie diesen Schritt, wenn Sie eine vorhandene Ressourcengruppe verwenden.)

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

Der Azure Resource Manager erfordert, dass alle Ressourcengruppen einen Speicherort angeben. Dieser wird als Standardspeicherort für Ressourcen in dieser Ressourcengruppe verwendet. Stellen Sie sicher, dass alle Befehle, mit denen ein Anwendungsgateway erstellt wird, die gleiche Ressourcengruppe verwenden.

Im obigen Beispiel haben wir eine Ressourcengruppe namens „appgw-RG“ mit dem Standort „USA, Westen“ erstellt.

>[AZURE.NOTE] Wenn Sie einen benutzerdefinierten Test für Ihr Anwendungsgateway konfigurieren müssen, finden Sie unter [Erstellen eines benutzerdefinierten Tests für Azure Application Gateway mithilfe von PowerShell für Azure-Ressourcen-Manager](application-gateway-create-probe-ps.md) weitere Informationen. Weitere Informationen finden Sie unter [Benutzerdefinierte Tests und Systemüberwachung](application-gateway-probe-overview.md).

### Schritt 5

Weisen Sie einen Adressbereich für das Subnetz zu, das für das Anwendungsgateway verwendet werden soll.

    $gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24

> [AZURE.NOTE] Ein Subnetz für eine Anwendung sollte mindestens 28 Maskenbits aufweisen. Bei diesem Wert verbleiben 10 verfügbare Adressen im Subnetz für Application Gateway-Instanzen. Mit einem kleineren Subnetz können Sie später möglicherweise keine weiteren Instanzen Ihres Anwendungsgateways hinzufügen.

### Schritt 6

Weisen Sie einen Adressbereich zu, der für den Back-End-Adresspool verwendet werden soll.

    $nicSubnet = New-AzureRmVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24

### Schritt 7

Erstellen Sie ein virtuelles Netzwerk mit den vorhergehenden Subnetzen in der Ressourcengruppe, die Sie im Schritt [Erstellen der Ressourcengruppe](#create-the-resource-group) erstellt haben.

    $vnet = New-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet

### Schritt 8

Rufen Sie die virtuelle Netzwerkressource und die Subnetzressourcen ab, die in den folgenden Schritten verwendet werden sollen:

    $vnet = Get-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg
    $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -VirtualNetwork $vnet
    $nicSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appsubnet' -VirtualNetwork $vnet

### Schritt 9

Erstellen Sie eine öffentliche IP-Ressource, die für das Anwendungsgateway verwendet werden soll. Diese öffentliche IP-Adresse wird in einem der folgenden Schritte verwendet:

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name 'appgwpip' -Location "West US" -AllocationMethod Dynamic

> [AZURE.IMPORTANT] Application Gateway unterstützt nicht die Verwendung einer öffentlichen IP-Adresse, die mit einer definierten Domänenbezeichnung erstellt wird. Nur eine öffentliche IP-Adresse mit einer dynamisch erstellten Domänenbezeichnung wird unterstützt. Wenn Sie einen benutzerfreundlichen DNS-Namen für das Anwendungsgateway benötigen, wird empfohlen, einen cname-Datensatz als Alias zu verwenden.

### Schritt 10

Sie müssen alle Konfigurationselemente einrichten, bevor Sie das Anwendungsgateway erstellen. Die folgenden Schritten erstellen die Konfigurationselemente, die für eine Application Gateway-Ressource benötigt werden.

Erstellen Sie eine Anwendungsgateway-IP-Konfiguration. So wird konfiguriert, welches Subnetz vom Anwendungsgateway verwendet wird. Beim Starten des Anwendungsgateways wird eine IP-Adresse aus dem konfigurierten Subnetz ausgewählt, und der Netzwerkdatenverkehr wird an die IP-Adressen im Back-End-IP-Pool weitergeleitet. Beachten Sie, dass jede Instanz eine eigene IP-Adresse benötigt.

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet

### Schritt 11

Konfigurieren Sie den Back-End-IP-Adresspool mit den IP-Adressen der Back-End-Webserver. Dies sind die IP-Adressen, die den Netzwerkdatenverkehr vom Front-End-IP-Endpunkt empfangen. Sie ersetzen die folgenden IP-Adressen durch die IP-Adressendpunkte Ihrer eigenen Anwendung.

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 1.1.1.1, 2.2.2.2, 3.3.3.3

### Schritt 12

Konfigurieren Sie die HTTP-Einstellungen auf dem Back-End des Anwendungsgateways. Weisen Sie den HTTP-Einstellungen das Zertifikat zu, das im vorherigen Schritt hochgeladen wurde.

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name 'setting01' -Port 443 -Protocol Https -CookieBasedAffinity Enabled -AuthenticationCertificates $authcert

### Schritt 13

Konfigurieren Sie den Front-End-IP-Port für den öffentlichen IP-Endpunkt. Dieser Port ist der Port, mit dem Endbenutzer eine Verbindung herstellen.

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name 'port01'  -Port 443

### Schritt 14

Erstellen Sie eine Front-End-IP-Konfiguration. Diese Einstellung ordnet dem Front-End des Anwendungsgateways eine private oder öffentliche IP-Adresse zu. Im folgenden Schritt wird die öffentliche IP-Adresse aus dem vorherigen Schritt der Front-End-IP-Konfiguration zugeordnet.

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip

### Schritt 15

Erstellen Sie den HTTP-Listener für das Anwendungsgateway. Weisen Sie die zu verwendende Front-End-IP-Konfiguration, den Port und das SSL-Zertifikat zu.

	$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert

### Schritt 16

Erstellen Sie eine Routingregel für den Lastenausgleich, durch die das Verhalten des Lastenausgleichs konfiguriert wird. In diesem Beispiel wird eine Roundrobin-Basisregel erstellt.

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
   
### Schritt 17

Konfigurieren Sie die Instanzgröße des Application Gateways.

    $sku = New-AzureRmApplicationGatewaySku -Name WAF_Medium -Tier WAF -Capacity 2

>[AZURE.NOTE]  Sie können zwischen **WAF\_Medium** und **WAF\_Large** wählen. Der Tarif lautet bei Verwendung der WAF immer **WAF**. Die Kapazität ist eine beliebige Zahl zwischen 1 und 10.

### Schritt 18

Konfigurieren Sie den Modus für die WAF. Die zulässigen Werte lauten **Prevention** und **Detection**.

    $config = New-AzureRmApplicationGatewayWafConfig -Enabled $true -WafMode "Prevention"

### Schritt 19

Erstellen Sie ein Anwendungsgateway mit allen Konfigurationselementen aus den vorherigen Schritten. In diesem Beispiel heißt das Application Gateway „appgwtest“.

	$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -WafConfig $config

## Nächste Schritte

Informationen zum Konfigurieren der Diagnoseprotokollierung zum Protokollieren von Ereignissen, die mit der Web Application Firewall erkannt oder verhindert werden, finden Sie unter [Application Gateway-Diagnose](application-gateway-diagnostics.md).


[scenario]: ./media/application-gateway-web-application-firewall-powershell/scenario.png

<!---HONumber=AcomDC_0928_2016-->