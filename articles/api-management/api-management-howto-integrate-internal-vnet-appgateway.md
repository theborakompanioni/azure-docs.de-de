---
title: Verwenden von Azure API Management im virtuellen Netzwerk mit Application Gateway | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie Azure API Management im internen virtuellen Netzwerk mit Application Gateway (WAF) als Front-End einrichten und konfigurieren.
services: api-management
documentationcenter: 
author: solankisamir
manager: kjoshi
editor: antonba
ms.assetid: a8c982b2-bca5-4312-9367-4a0bbc1082b1
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2017
ms.author: sasolank
translationtype: Human Translation
ms.sourcegitcommit: a87349518f7494dda49e52ed160509a4ffeb7775
ms.openlocfilehash: a58ec5f6d62d2b48d6cf85d997b2deac95310505


---
# <a name="integrate-api-management-in-an-internal-vnet-with-application-gateway"></a>Integrieren von API Management in ein internes VNET mit Application Gateway 

##<a name="overview"> </a> Übersicht
 
Der API Management-Dienst kann in einem virtuellen Netzwerk im internen Modus konfiguriert werden, sodass nur aus dem virtuellen Netzwerk darauf zugegriffen werden kann. Azure Application Gateway ist ein PaaS-Dienst, bei dem ein Schicht&7;-Lastenausgleich bereitgestellt wird. Er fungiert als Reverseproxydienst und verfügt unter anderem über eine Web Application Firewall (WAF).

Die Kombination aus einer Bereitstellung von API Management in einem internen VNET und dem Application Gateway-Front-End ermöglicht die folgenden Szenarien:

* Verwenden Sie dieselbe API Management-Ressource für die Nutzung durch interne und externe Consumer.
* Verwenden Sie nur eine API Management-Ressource, und halten Sie eine Teilmenge der in API Management definierten APIs für externe Consumer bereit.
* Schaffen Sie eine einfache Möglichkeit, mit der der Zugriff auf API Management über das öffentliche Internet ein- und ausgeschaltet werden kann. 

##<a name="scenario"> </a> Szenario
In diesem Beitrag wird beschrieben, wie Sie einen einzelnen API Management-Dienst sowohl für interne als auch für externe Consumer verwenden und als einzelnes Front-End für lokale und Cloud-APIs einrichten. Sie können dann angeben, dass nur eine Teilmenge dieser APIs (grün gekennzeichnet) auch für den externen Verbrauch verfügbar gemacht werden soll, indem Sie die PathBasedRouting-Funktionalität von Application Gateway verwenden.

Bei diesem Ansatz werden Ihre gesamten APIs nur aus Ihrem virtuellen Netzwerk verwaltet. Interne Consumer (orange gekennzeichnet) können auf alle internen und externen APIs zugreifen. Interne Consumer profitieren davon, dass der Datenverkehr niemals ins Internet gelangt und über ExpressRoute-Verbindungen hohe Geschwindigkeiten erreicht werden.

![URL-Route](./media/api-management-using-with-vnet/api-management-howto-integrate-internal-vnet-appgateway.png)

## <a name="before-you-begin"> </a> Voraussetzungen

1. Installieren Sie mit dem Webplattform-Installer die aktuelle Version der Azure PowerShell-Cmdlets. Sie können die neueste Version aus dem Abschnitt **Windows PowerShell** der [Downloadseite](https://azure.microsoft.com/downloads/)herunterladen und installieren.
2. Sie erstellen ein virtuelles Netzwerk und separate Subnetze für API Management und Application Gateway. 
3. Wenn Sie einen benutzerdefinierten DNS-Server für das virtuelle Netzwerk erstellen möchten, führen Sie dies vor dem Start der Bereitstellung durch und stellen sicher, dass eine im neuen Subnetz des virtuellen Netzwerks erstellte VM alle Azure-Dienstendpunkte auflösen kann.

## <a name="what-is-required-to-create-an-integration-between-api-management-and-application-gateway"></a>Was ist erforderlich, um eine Integration zwischen API Management und Application Gateway zu erstellen?

* **Back-End-Serverpool:** Dies ist die interne virtuelle IP-Adresse des API Management-Diensts.
* **Einstellungen für den Back-End-Serverpool:** Jeder Pool weist Einstellungen wie Port, Protokoll und cookiebasierte Affinität auf. Diese Einstellungen sind an einen Pool gebunden und gelten für alle Server innerhalb des Pools.
* **Front-End-Port:** Dieser Port ist der öffentliche Port, der im Application Gateway geöffnet ist. Datenverkehr erreicht diesen Port und wird dann an einen der Back-End-Server umgeleitet.
* **Listener:** Der Listener verfügt über einen Front-End-Port, ein Protokoll („Http“ oder „Https“; jeweils unter Beachtung der Groß-/Kleinschreibung) und den Namen des SSL-Zertifikats (falls die SSL-Auslagerung konfiguriert wird).
* **Regel:** Mit der Regel werden der Listener und der Back-End-Serverpool verbunden, und es wird definiert, an welchen Back-End-Serverpool der Datenverkehr geleitet werden soll, wenn er einen bestimmten Listener erreicht.
* **Benutzerdefinierter Integritätstest:** Für Application Gateway werden standardmäßig auf IP-Adressen basierende Tests verwendet, um zu ermitteln, welche Server im BackendAddressPool aktiv sind. Der API Management-Dienst reagiert nur auf Anforderungen, die über den richtigen Hostheader verfügen, sodass die Standardtests fehlschlagen. Wir müssen benutzerdefinierte Integritätstest definieren, um dem Anwendungsgateway mitzuteilen, dass der Dienst verfügbar ist und die Anforderungen weitergeleitet werden können.
* **Benutzerdefiniertes Domänenzertifikat:** Zum Zugreifen auf API Management über das Internet benötigen Sie eine CNAME-Zuordnung des Hostnamens zum DNS-Namen des Application Gateway-Front-Ends. Der gleiche Hostnamenheader und das gleiche Zertifikat werden auf API Management angewendet, damit API Management die Anforderung nach dem Eintreffen über Application Gateway als gültig erkennt und antworten kann.
## <a name="overview-steps"> </a> Erforderliche Schritte zur Integration von API Management und Application Gateway 

1. Erstellen einer Ressourcengruppe für den Ressourcen-Manager
2. Erstellen eines virtuelles Netzwerks, Subnetzes und einer öffentlichen IP-Adresse für das Application Gateway Erstellen Sie ein anderes Subnetz für API Management.
3. Erstellen Sie einen API Management-Dienst im Modus „Internes VNET“ im oben erstellten VNET-Subnetz.
4. Aktualisieren Sie den API Management-Dienst mit dem benutzerdefinierten Domänennamen.
5. Erstellen eines Konfigurationsobjekts für das Anwendungsgateway
6. Erstellen Sie eine neue Application Gateway-Ressource.
7. Erstellen Sie einen CNAME-Eintrag für den API Management-Proxyhostnamen zum Namen des öffentlichen DNS der Application Gateway-Ressource.

## <a name="create-a-resource-group-for-resource-manager"></a>Erstellen einer Ressourcengruppe für den Resource Manager

Stellen Sie sicher, dass Sie die neueste Version von Azure PowerShell verwenden. Weitere Informationen finden Sie unter [Verwenden von Windows PowerShell mit Resource Manager](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Schritt 1

Anmelden an Azure

```powershell
Login-AzureRmAccount
```

Sie werden zur Authentifizierung mit Ihren Anmeldeinformationen aufgefordert.<BR>

### <a name="step-2"></a>Schritt 2

Überprüfen Sie die Abonnements für das Konto, und treffen Sie die entsprechende Auswahl.

```powershell
Get-AzureRmSubscription -Subscriptionid "GUID of subscription" | Select-AzureRmSubscription
```

### <a name="step-3"></a>Schritt 3

Erstellen Sie eine Ressourcengruppe. (Überspringen Sie diesen Schritt, wenn Sie eine vorhandene Ressourcengruppe verwenden.)

```powershell
New-AzureRmResourceGroup -Name apim-appGw-RG -Location "West US"
```
Der Azure-Ressourcen-Manager erfordert, dass alle Ressourcengruppen einen Speicherort angeben. Dieser wird als Standardspeicherort für Ressourcen in dieser Ressourcengruppe verwendet. Stellen Sie sicher, dass alle Befehle, mit denen ein Anwendungsgateway erstellt wird, die gleiche Ressourcengruppe verwenden.

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Erstellen eines virtuelles Netzwerkes und eines Subnetzes für das Application Gateway

Das folgende Beispiel zeigt, wie Sie mit dem Ressourcen-Manager ein virtuelles Netzwerk erstellen:

### <a name="step-1"></a>Schritt 1

Weisen Sie den Adressbereich 10.0.0.0/24 der Subnetzvariablen zur Verwendung für das Anwendungsgateway beim Erstellen eines virtuellen Netzwerks zu.

```powershell
$appgatewaysubnet = New-AzureRmVirtualNetworkSubnetConfig -Name appgateway01 -AddressPrefix 10.0.0.0/24
```

### <a name="step-2"></a>Schritt 2

Weisen Sie den Adressbereich 10.0.1.0/24 der Subnetzvariablen zur Verwendung für API Management beim Erstellen eines virtuellen Netzwerks zu.

```powershell
$apimsubnet = New-AzureRmVirtualNetworkSubnetConfig -Name apim01 -AddressPrefix 10.0.1.0/24
```

### <a name="step-3"></a>Schritt 3

Erstellen Sie ein virtuelles Netzwerk mit dem Namen **appgwvnet** in der Ressourcengruppe **apim-appGw-RG** für die Region „USA, Westen“ mit dem Präfix „10.0.0.0/16“ und den Subnetzen „10.0.0.0/24“ und „10.0.1.0/24“.

```powershell
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName apim-appGw-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $appgatewaysubnet,$apimsubnet
```

### <a name="step-4"></a>Schritt 4

Zuweisen einer Subnetzvariablen für die nächsten Schritte

```powershell
$appgatewaysubnetdata=$vnet.Subnets[0]
$apimsubnetdata=$vnet.Subnets[1]
```
## <a name="create-an-api-management-service-in-internal-vnet-mode"></a>Erstellen eines API Management-Diensts im Modus „Internes VNET“

Mit dem folgenden Beispiel wird veranschaulicht, wie Sie einen API Management-Dienst im internen VNET erstellen.

### <a name="step-1"></a>Schritt 1
Erstellen Sie ein Objekt vom Typ „Virtuelles API Management-Netzwerk“, indem Sie das oben erstellte Subnetz „$apimsubnetdata“ verwenden.

```powershell
$apimVirtualNetwork = New-AzureRmApiManagementVirtualNetwork -Location "West US" -SubnetResourceId $apimsubnetdata.Id
```
### <a name="step-2"></a>Schritt 2
Erstellen Sie im virtuellen Netzwerk einen API Management-Dienst.

```powershell
$apimService = New-AzureRmApiManagement -ResourceGroupName "apim-appGw-RG" -Location "West US" -Name "ContosoApi" -Organization Contoso -AdminEmail admin@contoso.com -VirtualNetwork $apimVirtualNetwork -VpnType "Internal" -Sku "Premium"
```
Nach der erfolgreichen Ausführung des obigen Befehls helfen Ihnen die Informationen unter [Verwenden von Azure API Management mit internen virtuellen Netzwerken – DNS-Konfiguration][api-management-using-with-internal-vnet.md#apim-dns-configuration] weiter, um darauf zuzugreifen.

## <a name="update-api-management-service-with-custom-domain-name"></a>Aktualisieren des API Management-Diensts mit dem benutzerdefinierten Domänennamen

Wir wenden jetzt den benutzerdefinierten Domänennamen auf den Proxy des API Management-Dienstendpunkts an, auf den wir über das Internet zugreifen möchten.

### <a name="step-1"></a>Schritt 1
Laden Sie das Zertifikat mit dem privaten Schlüssel hoch, um nachzuweisen, dass Sie zum Einrichten der benutzerdefinierten Domäne in der Domäne berechtigt sind (z.B. `*.contoso.net`). 

```powershell
$certUploadResult = Import-AzureRmApiManagementHostnameCertificate -ResourceGroupName "apim-appGw-RG" -Name "ContosoApi" -HostnameType "Proxy" -PfxPath <full path to .pfx file> -PfxPassword <password for certificate file>
```

### <a name="step-2"></a>Schritt 2
Nach dem Hochladen des Zertifikats können wir ein Hostnamen-Konfigurationsobjekt für den Proxy mit dem Hostnamen `api.contoso.net` erstellen, da wir die Berechtigung für die Domäne `*.contoso.net` mit dem Zertifikat nachgewiesen haben. 

```powershell
$proxyHostnameConfig = New-AzureRmApiManagementHostnameConfiguration -CertificateThumbprint $certUploadResult.Thumbprint -Hostname "api.contoso.net"
$result = Set-AzureRmApiManagementHostnames -Name "ContosoApi" -ResourceGroupName "apim-appGw-RG" –PortalHostnameConfiguration $proxyHostnameConfig
```

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Erstellen der öffentlichen IP-Adresse für die Front-End-Konfiguration

Erstellen Sie eine öffentliche IP-Ressource namens **publicIP01** in der Ressourcengruppe **apim-appGw-RG** für die Region „USA, Westen“.

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName apim-appGw-RG -name publicIP01 -location "West US" -AllocationMethod Dynamic
```

Dem Anwendungsgateway wird beim Starten des Diensts eine IP-Adresse zugewiesen.

## <a name="create-application-gateway-configuration"></a>Erstellen einer Application Gateway-Konfiguration

Sie müssen alle Konfigurationselemente einrichten, bevor Sie das Anwendungsgateway erstellen. Die folgenden Schritten erstellen die Konfigurationselemente, die für eine Application Gateway-Ressource benötigt werden.

### <a name="step-1"></a>Schritt 1

Erstellen Sie für das Anwendungsgateway eine IP-Konfiguration mit dem Namen **gatewayIP01**. Beim Starten des Anwendungsgateways wird eine IP-Adresse aus dem konfigurierten Subnetz ausgewählt, und der Netzwerkdatenverkehr wird an die IP-Adressen im Back-End-IP-Pool weitergeleitet. Beachten Sie, dass jede Instanz eine eigene IP-Adresse benötigt.

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $appgatewaysubnetdata
```

### <a name="step-2"></a>Schritt 2

Konfigurieren Sie den Front-End-IP-Port für den öffentlichen IP-Endpunkt. Dieser Port ist der Port, mit dem Endbenutzer eine Verbindung herstellen.

```powershell
$fp01 = New-AzureRmApplicationGatewayFrontendPort -Name 'port01'  -Port 443
```
### <a name="step-3"></a>Schritt 3

Konfigurieren Sie die Front-End-IP-Adresse mit dem öffentlichen IP-Endpunkt.

```powershell
$fipconfig01 = New-AzureRmApplicationGatewayFrontendIPConfig -Name "frontend1" -PublicIPAddress $publicip
```

### <a name="step-4"></a>Schritt 4

Konfigurieren Sie das Zertifikat für das Anwendungsgateway. Dieses Zertifikat wird zum Entschlüsseln und erneuten Verschlüsseln des Datenverkehrs auf dem Anwendungsgateway verwendet.

```powershell
$cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path to .pfx file> -Password <password for certificate file>
```

### <a name="step-5"></a>Schritt 5

Erstellen Sie den HTTP-Listener für das Anwendungsgateway. Weisen Sie die zu verwendende Front-End-IP-Konfiguration, den Port und das SSL-Zertifikat zu.

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01 -SslCertificate $cert
```

### <a name="step-6"></a>Schritt 6

Hier erstellen wir einen benutzerdefinierten Test für den `ContosoApi`-Proxy-Domänenendpunkt des API Management-Diensts. Der Pfad `/status-0123456789abcdef` ist ein Standard-Integritätsendpunkt, der von allen API Management-Diensten gehostet wird. Der Hostname `contosoapi.azure-api.net` ist der Proxy-Standardhostname, der konfiguriert wird, wenn im öffentlichen Azure-Bereich der Dienst `contosoapi` erstellt wird.

```powershell
$apimprobe = New-AzureRmApplicationGatewayProbeConfig -Name apimproxyprobe -Protocol Https -HostName "contosoapi.azure-api.net" -Path "/status-0123456789abcdef" -Interval 30 -Timeout 120 -UnhealthyThreshold 8
```

### <a name="step-7"></a>Schritt 7

Laden Sie das Zertifikat hoch, das auf den SSL-fähigen Ressourcen des Back-End-Pools verwendet werden soll.

```powershell
$authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name 'whitelistcert1' -CertificateFile <full path to .cer file>
```

### <a name="step-8"></a>Schritt 8

Konfigurieren Sie die Application Gateway-Einstellung **apimPoolSetting** für den Datenverkehr im Back-End-Pool. Dieser Schritt umfasst auch eine Timeoutkonfiguration, die für die Back-End-Pool-Antwort auf eine Application Gateway-Anforderung gilt. Wenn eine Back-End-Antwort ein Timeoutlimit erreicht, bricht das Anwendungsgateway die Anforderung ab. Dieser Wert unterscheidet sich vom Timeout des Tests, das nur für die Back-End-Antwort auf Testprüfungen gilt.

```powershell
$apimPoolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name apimPoolSetting -Port 443 -Protocol Https -CookieBasedAffinity Disabled -Probe $apimprobe -AuthenticationCertificates $authcert -RequestTimeout 180
```

### <a name="step-9"></a>Schritt 9

Konfigurieren Sie den Back-End-IP-Adresspool namens **apimbackend** mit der internen virtuellen IP-Adresse für den oben erstellten API Management-Dienst.

```powershell
$apimProxyBackendPool = New-AzureRmApplicationGatewayBackendAddressPool -Name apimbackend -BackendIPAddresses $apimService.StaticIPs[0]
```

### <a name="step-10"></a>Schritt 10
Konfigurieren Sie URL-Regelpfade für die Back-End-Pools. In API Management können mehrere APIs konfiguriert werden, z.B. `Echo API (/echo/), Calculator API (/calc/) etc.`, und wir können festlegen, dass der Zugriff auf `Echo API` nur über das Internet möglich ist. 

Im folgenden Beispiel wird eine einfache Regel für den Pfad „/echo/" erstellt, um Datenverkehr an das Back-End „apimProxyBackendPool“ zu leiten.

```powershell
$echoapiRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "externalapis" -Paths "/echo/*" -BackendAddressPool $apimProxyBackendPool -BackendHttpSettings $apimPoolSetting

$urlPathMap = New-AzureRmApplicationGatewayUrlPathMapConfig -Name "urlpathmap" -PathRules $echoapiRule -DefaultBackendAddressPool $apimProxyBackendPool -DefaultBackendHttpSettings $apimPoolSetting
```

Mit den obigen Schritten wird sichergestellt, dass nur Anforderungen für den Pfad „/echo“ über das Anwendungsgateway gesendet werden können. Anforderungen an andere APIs, die in API Management konfiguriert sind, lösen den Fehler 404 für Application Gateway aus, wenn der Zugriff über das Internet erfolgt. 

### <a name="step-11"></a>Schritt 11

Erstellen Sie eine Regeleinstellung. Dieser Schritt konfiguriert das Anwendungsgateway für die Verwendung des Routings auf URL-Pfadbasis.

```powershell
$rule01 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule1" -RuleType PathBasedRouting -HttpListener $listener -UrlPathMap $urlPathMap
```

### <a name="step-12"></a>Schritt 12

Konfigurieren Sie die Anzahl der Instanzen und die Größe für das Application Gateway. Hier verwenden wir die [WAF-SKU][../application-gateway/application-gateway-webapplicationfirewall-overview.md], um die Sicherheit für die API Management-Ressource zu erhöhen.

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name WAF_Medium -Tier WAF -Capacity 2
```

### <a name="step-13"></a>Schritt 13

Wir konfigurieren für WAF den Modus „Prävention“.
```powershell
$config = New-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode "Prevention"
```

## <a name="create-application-gateway"></a>Erstellen eines Application Gateways

Erstellen Sie ein Anwendungsgateway mit allen Konfigurationsobjekten aus den vorherigen Schritten.

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName apim-appGw-RG -Location "West US" -BackendAddressPools $apimProxyBackendPool -BackendHttpSettingsCollection $apimPoolSetting  -FrontendIpConfigurations $fipconfig01 -GatewayIpConfigurations $gipconfig -FrontendPorts $fp01 -HttpListeners $listener -UrlPathMaps $urlPathMap -RequestRoutingRules $rule01 -Sku $sku -WebApplicationFirewallConfig $config -SslCertificates $cert -AuthenticationCertificates $authcert -Probes $apimprobe
```

## <a name="cname-the-api-management-proxy-hostname-to-public-dns-name-of-application-gateway-resource"></a>Erstellen Sie einen CNAME-Eintrag für den API Management-Proxyhostnamen zum Namen des öffentlichen DNS der Application Gateway-Ressource.

Nach dem Erstellen des Gateways wird das Front-End für die Kommunikation konfiguriert. Wenn Sie eine öffentliche IP-Adresse verwenden, wird das Anwendungsgateway ein dynamisch zugewiesener DNS-Namen benötigt, der kein Anzeigename ist. Rufen Sie zum Konfigurieren des CNAME-Eintrags für die Front-End-IP mithilfe des PublicIPAddress-Elements, das an das Anwendungsgateway angefügt ist, Details zum Anwendungsgateway und dem zugeordneten IP/DNS-Namen ab. Verwenden Sie den DNS-Namen des Anwendungsgateways zum Erstellen eines CNAME-Eintrags, mit dem für den oben konfigurierten Proxyhostnamen `api.contoso.net` auf diesen DNS-Namen verwiesen wird. Die Verwendung von A-Einträgen wird nicht empfohlen, da sich die VIP beim Neustart des Anwendungsgateways möglicherweise verändert.

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName apim-appGw-RG -Name publicIP01
```

##<a name="summary"> </a> Zusammenfassung
Für den Azure API Management-Dienst wird in einem VNET eine einzelne Gatewayschnittstelle bereitgestellt, um den Zugriff auf Ihre gesamten APIs zu verwalten. Hierbei spielt es keine Rolle, ob diese lokal oder in der Cloud gehostet werden. Sie gewinnen umfassende Erkenntnisse dazu, wer auf Ihre APIs zugreift und wie dies erfolgt. Mit der Integration von Application Gateway in API Management können Sie flexibler festlegen, welche APIs Internetzugriff haben sollen, und Sie können eine Web Application Firewall als Front-End für Ihre API Management-Instanz bereitstellen.

##<a name="next-steps"> </a> Nächste Schritte
* Weitere Informationen zu Azure Application Gateway
  * [Übersicht über Application Gateway](../application-gateway/application-gateway-introduction.md)
  * [Web Application Firewall für Application Gateway](../application-gateway/application-gateway-webapplicationfirewall-overview.md)
* Weitere Informationen zu API Management im VNET
  * [Verwenden von API Management im VNET](api-management-using-with-vnet.md)





<!--HONumber=Feb17_HO1-->


