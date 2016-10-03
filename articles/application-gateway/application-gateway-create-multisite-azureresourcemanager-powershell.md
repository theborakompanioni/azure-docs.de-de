<properties
   pageTitle="Erstellen eines Anwendungsgateways zum Hosten mehrerer Websites | Microsoft Azure"
   description="Auf dieser Seite finden Sie Anweisungen zum Erstellen und Konfigurieren eines Azure-Anwendungsgateways zum Hosten mehrerer Webanwendungen über dasselbe Gateway."
   documentationCenter="na"
   services="application-gateway"
   authors="amsriva"
   manager="rossort"
   editor="amsriva"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/07/2016"
   ms.author="amsriva"/>


# Erstellen eines Anwendungsgateways zum Hosten mehrerer Webanwendungen

Das Hosten mehrerer Websites ermöglicht es Ihnen, mehr als eine Webanwendung über dasselbe Anwendungsgateway bereitzustellen. Das Prinzip basiert auf einem Hostheader in der eingehenden HTTP-Anforderung, anhand dessen bestimmt wird, welcher Listener den Datenverkehr empfängt. Der Listener leitet dann den Datenverkehr an den in der Regeldefinition des Gateways konfigurierten Back-End-Pool. Bei SSL-aktivierten Webanwendungen wählt das Anwendungsgateway den passenden Listener für den Webdatenverkehr anhand der SNI-Erweiterung (Servernamensanzeige) aus.

Ein gängiges Szenario für das Hosten mehrerer Websites ist der Lastenausgleich von Anforderungen für verschiedene Webdomänen auf verschiedene Back-End-Serverpools. Auf ähnliche Weise können auch mehrere Unterdomänen derselben Stammdomäne im selben Anwendungsgateway gehostet werden.

## Szenario
Im folgenden Beispiel verarbeitet das Anwendungsgateway Datenverkehr für „contoso.com“ und „fabrikam.com“ mit zwei Back-End-Serverpools: dem Contoso-Serverpool und dem Fabrikam-Serverpool. Mit einem ähnlichen Setup können auch Unterdomänen wie „app.contoso.com“ und „blog.contoso.com“ gehostet werden.


## Voraussetzungen

1. Installieren Sie mit dem Webplattform-Installer die aktuelle Version der Azure PowerShell-Cmdlets. Sie können die neueste Version aus dem Abschnitt **Windows PowerShell** der [Downloadseite](https://azure.microsoft.com/downloads/) herunterladen und installieren.
2. Erstellen Sie ein virtuelles Netzwerk und ein Subnetz für das Anwendungsgateway. Stellen Sie sicher, dass keine virtuellen Maschinen oder Cloudbereitstellungen das Subnetz verwenden. Das Application Gateway muss sich allein im Subnetz eines virtuellen Netzwerks befinden.
3. Die Server, die Sie dem Back-End-Pool zur Verwendung des Application Gateways hinzugefügt haben, müssen vorhanden sein oder Endpunkte aufweisen, die im virtuellen Netzwerk erstellt wurden, oder denen eine öffentliche IP-Adresse/VIP zugewiesen wurde.



## Was ist zum Erstellen eines Application Gateways erforderlich?


- **Back-End-Serverpool:** Die Liste der IP-Adressen der Back-End-Server. Die aufgelisteten IP-Adressen sollten entweder dem Subnetz des virtuellen Netzwerks angehören oder eine öffentliche IP-Adresse/VIP sein. Der FQDN kann ebenfalls verwendet werden.
- **Einstellungen für den Back-End-Serverpool:** Jeder Pool weist Einstellungen wie Port, Protokoll und cookiebasierte Affinität auf. Diese Einstellungen sind an einen Pool gebunden und gelten für alle Server innerhalb des Pools.
- **Front-End-Port:** Dieser Port ist der öffentliche Port, der im Application Gateway geöffnet ist. Datenverkehr erreicht diesen Port und wird dann an einen der Back-End-Server umgeleitet.
- **Listener:** Der Listener verfügt über einen Front-End-Port, ein Protokoll („Http“ oder „Https“; jeweils unter Beachtung der Groß-/Kleinschreibung) und den Namen des SSL-Zertifikats (falls die SSL-Auslagerung konfiguriert wird). Bei Anwendungsgateways, die mehrere Websites hosten können, werden zudem der Hostname und SNI-Indikatoren hinzugefügt.
- **Regel:** Mit der Regel werden der Listener und der Back-End-Serverpool verbunden, und es wird definiert, an welchen Back-End-Serverpool der Datenverkehr geleitet werden soll, wenn er einen bestimmten Listener erreicht.

## Erstellen eines Anwendungsgateways

Hier sind die erforderlichen Schritte zum Erstellen eines Anwendungsgateways angegeben:

1. Erstellen einer Ressourcengruppe für den Ressourcen-Manager
2. Erstellen eines virtuelles Netzwerks, Subnetzes und einer öffentlichen IP-Adresse für das Application Gateway
3. Erstellen eines Konfigurationsobjekts für das Anwendungsgateway
4. Erstellen einer Application Gateway-Ressource

## Erstellen einer Ressourcengruppe für den Ressourcen-Manager

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

	Select-AzureRmSubscription -SubscriptionName "Name of subscription"

### Schritt 4

Erstellen Sie eine Ressourcengruppe. (Überspringen Sie diesen Schritt, wenn Sie eine vorhandene Ressourcengruppe verwenden.)

    New-AzureRmResourceGroup -Name appgw-RG -location "East Asia"

Alternativ können Sie auch Tags für eine Ressourcengruppe für das Application Gateway erstellen:
	
	$resourceGroup = New-AzureRmResourceGroup -Name appgw-RG -Location "East Asia" -Tags @{Name = "testtag"; Value = "Application Gateway multiple site"} 

Der Azure Resource Manager erfordert, dass alle Ressourcengruppen einen Speicherort angeben. Dieser wird als Standardspeicherort für Ressourcen in dieser Ressourcengruppe verwendet. Stellen Sie sicher, dass alle Befehle, mit denen ein Anwendungsgateway erstellt wird, die gleiche Ressourcengruppe verwenden.

Im obigen Beispiel wurde eine Ressourcengruppe „appgw-RG“ mit dem Standort „Asien, Osten“ erstellt.

>[AZURE.NOTE] Wenn Sie einen benutzerdefinierten Test für Ihr Anwendungsgateway konfigurieren müssen, finden Sie unter [Erstellen eines benutzerdefinierten Tests für Azure Application Gateway mithilfe von PowerShell für Azure-Ressourcen-Manager](application-gateway-create-probe-ps.md) weitere Informationen. Weitere Informationen finden Sie unter [Benutzerdefinierte Tests und Systemüberwachung](application-gateway-probe-overview.md).

## Erstellen eines virtuelles Netzwerkes und eines Subnetzes für das Application Gateway

Das folgende Beispiel zeigt, wie Sie mit dem Ressourcen-Manager ein virtuelles Netzwerk erstellen:

### Schritt 1

Weisen Sie den Adressbereich 10.0.0.0/24 der Subnetzvariablen zu, die zum Erstellen eines virtuelles Netzwerks verwendet wird.

	$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24


### Schritt 2

Erstellen Sie ein virtuelles Netzwerk mit dem Namen „appgwvnet“ in der Ressourcengruppe „appgw-rg“ für die Region „USA, Westen“ mit dem Präfix „10.0.0.0/16“ und dem Subnetz „10.0.0.0/24“.

	$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-RG -Location "East Asia" -AddressPrefix 10.0.0.0/16 -Subnet $subnet


### Schritt 3

Weisen Sie eine Subnetzvariable für die nächsten Schritte zu. Damit wird ein Application Gateway erstellt.

	$subnet=Get-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -VirtualNetwork $vnet

## Erstellen der öffentlichen IP-Adresse für die Front-End-Konfiguration

Erstellen Sie eine öffentliche IP-Ressource namens „publicIP01“ in der Ressourcengruppe „appgw-rg“ für die Region „USA, Westen“.

	$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -name publicIP01 -location "East Asia" -AllocationMethod Dynamic

Dem Anwendungsgateway wird beim Starten des Diensts eine IP-Adresse zugewiesen.

## Erstellen einer Application Gateway-Konfiguration

Sie müssen alle Konfigurationselemente einrichten, bevor Sie das Anwendungsgateway erstellen. Die folgenden Schritten erstellen die Konfigurationselemente, die für eine Application Gateway-Ressource benötigt werden.

### Schritt 1

Erstellen Sie eine IP-Konfiguration für das Application Gateway mit dem Namen „gatewayIP01“. Beim Starten des Anwendungsgateways wird eine IP-Adresse aus dem konfigurierten Subnetz ausgewählt, und der Netzwerkdatenverkehr wird an die IP-Adressen im Back-End-IP-Pool weitergeleitet. Beachten Sie, dass jede Instanz eine eigene IP-Adresse benötigt.


	$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet


### Schritt 2

Konfigurieren Sie die Back-End-IP-Adresspools „pool01“ und „pool02“ mit den IP-Adressen „10.0.0.100, 10.0.0.101,10.0.0.102“ für „pool01“ und „10.0.0.103, 10.0.0.104, 10.0.0.105“ für „pool02“.


	$pool1 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 10.0.0.100, 10.0.0.101,10.0.0.102 
	$pool2 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool02 -BackendIPAddresses 10.0.0.103, 10.0.0.104, 10.0.0.105

Dieses Beispiel enthält zwei Back-End-Pools zum Weiterleiten von Netzwerkverkehr basierend auf der angeforderten Site. Ein Pool empfängt Datenverkehr von der Website „contoso.com“, und der andere Pool empfängt Datenverkehr von der Website „fabrikam.com“. Ersetzen Sie die obigen IP-Adressen durch die IP-Adressendpunkte Ihrer eigenen Anwendung. Anstelle interner IP-Adressen können Sie auch öffentliche IP-Adressen, den FQDN oder eine VM-NIC für Back-End-Instanzen verwenden. Verwenden Sie den Parameter „-BackendFQDNs“ in PowerShell, um FQDNs anstelle von IPs anzugeben.

### Schritt 3

Konfigurieren Sie die Application Gateway-Einstellungen „poolsetting01“ und „poolsetting02“ für den Lastenausgleich des Netzwerkdatenverkehrs im Back-End-Pool. In diesem Beispiel konfigurieren Sie verschiedene Back-End-Pool-Einstellungen für die Back-End-Pools. Jeder Back-End-Pool kann eine eigene Back-End-Pool-Einstellung aufweisen.


	$poolSetting01 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled -RequestTimeout 120
	$poolSetting02 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting02" -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 240

### Schritt 4

Konfigurieren Sie die Front-End-IP-Adresse mit dem öffentlichen IP-Endpunkt.

	$fipconfig01 = New-AzureRmApplicationGatewayFrontendIPConfig -Name "frontend1" -PublicIPAddress $publicip

### Schritt 5

Konfigurieren Sie den Front-End-Port für ein Application Gateway.

	$fp01 = New-AzureRmApplicationGatewayFrontendPort -Name "fep01" -Port 443

### Schritt 6

Konfigurieren Sie zwei SSL-Zertifikate für die beiden Websites, die wir in diesem Beispiel unterstützen. Ein Zertifikat ist für Datenverkehr von „contoso.com“, das andere für Datenverkehr von „fabrikam.com“. Hierbei sollte es sich um Zertifikate handeln, die von einer Zertifizierungsstelle für Ihre Websites ausgestellt wurden. Selbstsignierte Zertifikate werden zwar unterstützt, werden für Produktionsdatenverkehr jedoch nicht empfohlen.

	$cert01 = New-AzureRmApplicationGatewaySslCertificate -Name  contosocert -CertificateFile <file path> -Password <password>
	$cert02 = New-AzureRmApplicationGatewaySslCertificate -Name fabrikamcert -CertificateFile <file path> -Password <password>


### Schritt 7

Konfigurieren Sie zwei Listener für die beiden Websites in diesem Beispiel. In diesem Schritt werden die Listener für die öffentliche IP-Adresse sowie den Port und den Host zum Empfangen von eingehendem Datenverkehr konfiguriert. Der Parameter „HostName“ ist zur Unterstützung mehrerer Websites erforderlich und muss für die entsprechende Website festgelegt werden, für die der Datenverkehr empfangen wird. Der Parameter „RequireServerNameIndication“ muss für Websites, die SSL-Unterstützung für Szenarien mit mehreren Hosts benötigen, auf „True“ festgelegt werden. Wenn SSL-Unterstützung erforderlich ist, müssen Sie auch das SSL-Zertifikat angeben, das für die Sicherheit des Datenverkehrs für diese Webanwendung verwendet wird. Die Kombination aus „FrontendIPConfiguration“, „FrontendPort“ und „HostName“ muss für einen Listener eindeutig sein. Jeder Listener kann ein Zertifikat unterstützen.

	$listener01 = New-AzureRmApplicationGatewayHttpListener -Name "listener01" -Protocol Https -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01 -HostName "contoso11.com" -RequireServerNameIndication true  -SslCertificate $cert01
	$listener02 = New-AzureRmApplicationGatewayHttpListener -Name "listener02" -Protocol Https -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01 -HostName "fabrikam11.com" -RequireServerNameIndication true -SslCertificate $cert02

### Schritt 8

Erstellen Sie zwei Regeleinstellungen für die beiden Webanwendungen in diesem Beispiel. Eine Regel verbindet Listener, Back-End-Pools und HTTP-Einstellungen. In diesem Schritt wird das Anwendungsgateway für die Verwendung der Routingregel „Basic“ konfiguriert (eine Regel für jede Website). Der eingehende Datenverkehr jeder Website wird vom konfigurierten Listener der Website empfangen und anschließend unter Verwendung der in den BackendHttpSettings festgelegten Eigenschaften an den jeweils konfigurierten Back-End-Pool geleitet.

	$rule01 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule01" -RuleType Basic -HttpListener $listener01 -BackendHttpSettings $poolSetting01 -BackendAddressPool $pool1
	$rule02 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule02" -RuleType Basic -HttpListener $listener02 -BackendHttpSettings $poolSetting02 -BackendAddressPool $pool2

### Schritt 9

Konfigurieren Sie die Anzahl der Instanzen und die Größe für das Application Gateway.

	$sku = New-AzureRmApplicationGatewaySku -Name "Standard_Medium" -Tier Standard -Capacity 2

## Erstellen eines Anwendungsgateways

Erstellen Sie ein Anwendungsgateway mit allen Konfigurationsobjekten aus den vorherigen Schritten.

	$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-RG -Location "East Asia" -BackendAddressPools $pool1,$pool2 -BackendHttpSettingsCollection $poolSetting01, $poolSetting02 -FrontendIpConfigurations $fipconfig01 -GatewayIpConfigurations $gipconfig -FrontendPorts $fp01 -HttpListeners $listener01, $listener02 -RequestRoutingRules $rule01, $rule02 -Sku $sku -SslCertificates $cert01, $cert02 	


>[AZURE.IMPORTANT] Die Bereitstellung des Anwendungsgateways ist ein Vorgang mit langer Ausführungsdauer, und es kann einige Zeit dauern, bis er abgeschlossen ist.

## Abrufen des DNS-Namens des Anwendungsgateways

Rufen Sie mithilfe des Elements PublicIPAddress, das an das Anwendungsgateway angefügt ist, Details zum Anwendungsgateway und den zugeordneten IP/DNS-Namen ab. Verwenden Sie den DNS-Namen des Anwendungsgateways zum Erstellen eines CNAME-Eintrags, der die beiden Webanwendungen an diesen DNS-Namen verweist. Die Verwendung von A-Einträgen wird nicht empfohlen, da sich die VIP beim Neustart des Anwendungsgateways möglicherweise verändert.
	
	Get-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -name publicIP01
		
	Name                     : publicIP01
	ResourceGroupName        : appgw-RG
	Location                 : eastasia
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

<!---HONumber=AcomDC_0921_2016-->