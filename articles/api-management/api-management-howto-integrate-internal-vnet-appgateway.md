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
ms.translationtype: HT
ms.sourcegitcommit: cf381b43b174a104e5709ff7ce27d248a0dfdbea
ms.openlocfilehash: 8131ded6b74e9c544bf70b1a4659ed07e5def04d
ms.contentlocale: de-de
ms.lasthandoff: 08/23/2017

---
# <a name="integrate-api-management-in-an-internal-vnet-with-application-gateway"></a>Integrieren von API Management in ein internes VNET mit Application Gateway 

##<a name="overview"> </a> Übersicht
 
Der API Management-Dienst kann in einem virtuellen Netzwerk im internen Modus konfiguriert werden, sodass nur aus dem virtuellen Netzwerk darauf zugegriffen werden kann. Azure Application Gateway ist ein PaaS-Dienst, bei dem ein Schicht 7-Lastenausgleich bereitgestellt wird. Er fungiert als Reverseproxydienst und verfügt unter anderem über eine Web Application Firewall (WAF).

Die Kombination aus einer Bereitstellung von API Management in einem internen VNET und dem Application Gateway-Front-End ermöglicht die folgenden Szenarien:

* Verwenden Sie dieselbe API Management-Ressource für die Nutzung durch interne und externe Consumer.
* Verwenden Sie nur eine API Management-Ressource, und halten Sie eine Teilmenge der in API Management definierten APIs für externe Consumer bereit.
* Schaffen Sie eine einfache Möglichkeit, mit der der Zugriff auf API Management über das öffentliche Internet ein- und ausgeschaltet werden kann. 

##<a name="scenario"> </a> Szenario
In diesem Artikel wird beschrieben, wie Sie einen einzelnen API Management-Dienst sowohl für interne als auch für externe Consumer verwenden und als einzelnes Front-End für lokale und Cloud-APIs einrichten. Außerdem wird veranschaulicht, wie Sie nur eine Teilmenge Ihrer APIs (im Beispiel grün markiert) für die externe Nutzung verfügbar machen, indem Sie die PathBasedRouting-Funktionalität von Application Gateway verwenden.

Im ersten Setupbeispiel werden alle APIs ausschließlich aus Ihrem virtuellen Netzwerk verwaltet. Interne Consumer (orange gekennzeichnet) können auf alle internen und externen APIs zugreifen. Der Datenverkehr gelangt niemals ins Internet, und die ExpressRoute-Verbindungen ermöglichen eine hohe Leistung.

![URL-Route](./media/api-management-howto-integrate-internal-vnet-appgateway/api-management-howto-integrate-internal-vnet-appgateway.png)

## <a name="before-you-begin"> </a> Voraussetzungen

1. Installieren Sie mit dem Webplattform-Installer die aktuelle Version der Azure PowerShell-Cmdlets. Sie können die neueste Version aus dem Abschnitt **Windows PowerShell** der [Downloadseite](https://azure.microsoft.com/downloads/)herunterladen und installieren.
2. Erstellen Sie ein virtuelles Netzwerk und separate Subnetze für API Management und Application Gateway. 
3. Wenn Sie einen benutzerdefinierten DNS-Server für das virtuelle Netzwerk erstellen möchten, sollten Sie dies vor dem Starten der Bereitstellung durchführen. Überprüfen Sie die richtige Funktionsweise, indem Sie sicherstellen, dass ein virtueller Computer, der im virtuellen Netzwerk in einem neuen Subnetz erstellt wird, alle Azure-Dienstendpunkte auflösen und darauf zugreifen kann.

## <a name="what-is-required-to-create-an-integration-between-api-management-and-application-gateway"></a>Was ist erforderlich, um eine Integration zwischen API Management und Application Gateway zu erstellen?

* **Back-End-Serverpool:** Dies ist die interne virtuelle IP-Adresse des API Management-Diensts.
* **Einstellungen für den Back-End-Serverpool:** Jeder Pool weist Einstellungen wie Port, Protokoll und cookiebasierte Affinität auf. Diese Einstellungen werden auf alle Server innerhalb des Pools angewendet.
* **Front-End-Port:** Dies ist der öffentliche Port, der im Application Gateway geöffnet ist. Datenverkehr, der an diesem Port eintrifft, wird an einen der Back-End-Server umgeleitet.
* **Listener:** Der Listener verfügt über einen Front-End-Port, ein Protokoll („Http“ oder „Https“; jeweils unter Beachtung der Groß-/Kleinschreibung) und den Namen des SSL-Zertifikats (falls die SSL-Auslagerung konfiguriert wird).
* **Regel:** Mit der Regel wird ein Listener an einen Back-End-Serverpool gebunden.
* **Benutzerdefinierter Integritätstest:** Für Application Gateway werden standardmäßig auf IP-Adressen basierende Tests verwendet, um zu ermitteln, welche Server im BackendAddressPool aktiv sind. Der API Management-Dienst reagiert nur auf Anforderungen, die über den richtigen Hostheader verfügen, sodass die Standardtests fehlschlagen. Ein benutzerdefinierter Integritätstest muss definiert werden, damit das Application Gateway ermitteln kann, dass der Dienst aktiv ist und Anforderungen weitergeleitet werden können.
* **Benutzerdefiniertes Domänenzertifikat:** Zum Zugreifen auf API Management über das Internet müssen Sie eine CNAME-Zuordnung des Hostnamens zum DNS-Namen des Application Gateway-Front-Ends erstellen. So wird sichergestellt, dass die Daten zum Hostnamenheader und Zertifikat für das Application Gateway, die an API Management weitergeleitet werden, von APIM als gültig erkannt werden können.

## <a name="overview-steps"> </a> Erforderliche Schritte zur Integration von API Management und Application Gateway 

1. Erstellen einer Ressourcengruppe für den Ressourcen-Manager
2. Erstellen eines virtuelles Netzwerks, Subnetzes und einer öffentlichen IP-Adresse für das Application Gateway Erstellen eines anderen Subnetzes für API Management
3. Erstellen eines API Management-Diensts im oben erstellten VNET-Subnetz und Sicherstellen, dass der interne Modus verwendet wird
4. Einrichten des benutzerdefinierten Domänennamens im API Management-Dienst
5. Erstellen eines Konfigurationsobjekts für das Application Gateway
6. Erstellen einer Application Gateway-Ressource
7. Erstellen eines CNAME-Eintrags aus dem öffentlichen DNS-Namen des Application Gateway für den API Management-Proxyhostnamen

## <a name="create-a-resource-group-for-resource-manager"></a>Erstellen einer Ressourcengruppe für den Resource Manager

Stellen Sie sicher, dass Sie die neueste Version von Azure PowerShell verwenden. Weitere Informationen finden Sie unter [Verwenden von Windows PowerShell mit Resource Manager](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Schritt 1

Anmelden an Azure

```powershell
Login-AzureRmAccount
```

Authentifizieren Sie sich mit Ihren Anmeldeinformationen.<BR>

### <a name="step-2"></a>Schritt 2

Überprüfen Sie die Abonnements für das Konto, und treffen Sie die entsprechende Auswahl.

```powershell
Get-AzureRmSubscription -Subscriptionid "GUID of subscription" | Select-AzureRmSubscription
```

### <a name="step-3"></a>Schritt 3

Erstellen Sie eine Ressourcengruppe. (Überspringen Sie diesen Schritt, wenn Sie eine vorhandene Ressourcengruppe verwenden.)

```powershell
New-AzureRmResourceGroup -Name "apim-appGw-RG" -Location "West US"
```
Der Azure-Ressourcen-Manager erfordert, dass alle Ressourcengruppen einen Speicherort angeben. Dieser wird als Standardspeicherort für Ressourcen in dieser Ressourcengruppe verwendet. Stellen Sie sicher, dass alle Befehle, mit denen ein Anwendungsgateway erstellt wird, die gleiche Ressourcengruppe verwenden.

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Erstellen eines virtuellen Netzwerks und eines Subnetzes für das Application Gateway

Das folgende Beispiel zeigt, wie Sie mit dem Resource Manager ein virtuelles Netzwerk erstellen:

### <a name="step-1"></a>Schritt 1

Weisen Sie den Adressbereich 10.0.0.0/24 der Subnetzvariablen zur Verwendung für das Application Gateway beim Erstellen eines virtuellen Netzwerks zu.

```powershell
$appgatewaysubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "apim01" -AddressPrefix "10.0.0.0/24"
```

### <a name="step-2"></a>Schritt 2

Weisen Sie den Adressbereich 10.0.1.0/24 der Subnetzvariablen zur Verwendung für API Management beim Erstellen eines virtuellen Netzwerks zu.

```powershell
$apimsubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "apim02" -AddressPrefix "10.0.1.0/24"
```

### <a name="step-3"></a>Schritt 3

Erstellen Sie ein virtuelles Netzwerk mit dem Namen **appgwvnet** in der Ressourcengruppe **apim-appGw-RG** für die Region „USA, Westen“ mit dem Präfix „10.0.0.0/16“ und den Subnetzen „10.0.0.0/24“ und „10.0.1.0/24“.

```powershell
$vnet = New-AzureRmVirtualNetwork -Name "appgwvnet" -ResourceGroupName "apim-appGw-RG" -Location "West US" -AddressPrefix "10.0.0.0/16" -Subnet $appgatewaysubnet,$apimsubnet
```

### <a name="step-4"></a>Schritt 4

Zuweisen einer Subnetzvariablen für die nächsten Schritte

```powershell
$appgatewaysubnetdata=$vnet.Subnets[0]
$apimsubnetdata=$vnet.Subnets[1]
```
## <a name="create-an-api-management-service-inside-a-vnet-configured-in-internal-mode"></a>Erstellen eines API Management-Diensts in einem VNET, das im internen Modus konfiguriert ist

Im folgenden Beispiel wird veranschaulicht, wie Sie einen API Management-Dienst in einem VNET erstellen, für das nur der interne Zugriff konfiguriert ist.

### <a name="step-1"></a>Schritt 1
Erstellen Sie ein Objekt vom Typ „Virtuelles API Management-Netzwerk“, indem Sie das oben erstellte Subnetz „$apimsubnetdata“ verwenden.

```powershell
$apimVirtualNetwork = New-AzureRmApiManagementVirtualNetwork -Location "West US" -SubnetResourceId $apimsubnetdata.Id
```
### <a name="step-2"></a>Schritt 2
Erstellen Sie im virtuellen Netzwerk einen API Management-Dienst.

```powershell
$apimService = New-AzureRmApiManagement -ResourceGroupName "apim-appGw-RG" -Location "West US" -Name "ContosoApi" -Organization "Contoso" -AdminEmail "admin@contoso.com" -VirtualNetwork $apimVirtualNetwork -VpnType "Internal" -Sku "Developer"
```
Nachdem der obige Befehl erfolgreich ausgeführt wurde, können Sie die Informationen unter [DNS-Konfiguration](api-management-using-with-internal-vnet.md#apim-dns-configuration) (für den Zugriff auf den internen VNET-API Management-Dienst) nutzen, um darauf zuzugreifen.

## <a name="set-up-a-custom-domain-name-in-api-management"></a>Einrichten eines benutzerdefinierten Domänennamens in API Management

### <a name="step-1"></a>Schritt 1
Laden Sie das Zertifikat mit dem privaten Schlüssel für die Domäne hoch. In diesem Beispiel ist dies `*.contoso.net`. 

```powershell
$certUploadResult = Import-AzureRmApiManagementHostnameCertificate -ResourceGroupName "apim-appGw-RG" -Name "ContosoApi" -HostnameType "Proxy" -PfxPath <full path to .pfx file> -PfxPassword <password for certificate file> -PassThru
```

### <a name="step-2"></a>Schritt 2
Erstellen Sie nach dem Hochladen des Zertifikats ein Hostnamen-Konfigurationsobjekt für den Proxy mit dem Hostnamen `api.contoso.net`, da im Beispielzertifikat die Autorität für die Domäne `*.contoso.net` gewährt wird. 

```powershell
$proxyHostnameConfig = New-AzureRmApiManagementHostnameConfiguration -CertificateThumbprint $certUploadResult.Thumbprint -Hostname "api.contoso.net"
$result = Set-AzureRmApiManagementHostnames -Name "ContosoApi" -ResourceGroupName "apim-appGw-RG" -ProxyHostnameConfiguration $proxyHostnameConfig
```

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Erstellen der öffentlichen IP-Adresse für die Front-End-Konfiguration

Erstellen Sie eine öffentliche IP-Ressource namens **publicIP01** in der Ressourcengruppe **apim-appGw-RG** für die Region „USA, Westen“.

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName "apim-appGw-RG" -name "publicIP01" -location "West US" -AllocationMethod Dynamic
```

Dem Anwendungsgateway wird beim Starten des Diensts eine IP-Adresse zugewiesen.

## <a name="create-application-gateway-configuration"></a>Erstellen einer Application Gateway-Konfiguration

Sie müssen alle Konfigurationselemente einrichten, bevor Sie das Anwendungsgateway erstellen. Die folgenden Schritten erstellen die Konfigurationselemente, die für eine Application Gateway-Ressource benötigt werden.

### <a name="step-1"></a>Schritt 1

Erstellen Sie für das Anwendungsgateway eine IP-Konfiguration mit dem Namen **gatewayIP01**. Beim Starten des Anwendungsgateways wird eine IP-Adresse aus dem konfigurierten Subnetz ausgewählt, und der Netzwerkdatenverkehr wird an die IP-Adressen im Back-End-IP-Pool weitergeleitet. Beachten Sie, dass jede Instanz eine eigene IP-Adresse benötigt.

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name "gatewayIP01" -Subnet $appgatewaysubnetdata
```

### <a name="step-2"></a>Schritt 2

Konfigurieren Sie den Front-End-IP-Port für den öffentlichen IP-Endpunkt. Dieser Port ist der Port, mit dem Endbenutzer eine Verbindung herstellen.

```powershell
$fp01 = New-AzureRmApplicationGatewayFrontendPort -Name "port01"  -Port 443
```
### <a name="step-3"></a>Schritt 3

Konfigurieren Sie die Front-End-IP-Adresse mit dem öffentlichen IP-Endpunkt.

```powershell
$fipconfig01 = New-AzureRmApplicationGatewayFrontendIPConfig -Name "frontend1" -PublicIPAddress $publicip
```

### <a name="step-4"></a>Schritt 4

Konfigurieren Sie das Zertifikat für das Application Gateway, das zum Entschlüsseln und Verschlüsseln des durchlaufenden Datenverkehrs verwendet wird.

```powershell
$cert = New-AzureRmApplicationGatewaySslCertificate -Name "cert01" -CertificateFile <full path to .pfx file> -Password <password for certificate file>
```

### <a name="step-5"></a>Schritt 5

Erstellen Sie den HTTP-Listener für das Application Gateway. Weisen Sie die zu verwendende Front-End-IP-Konfiguration, den Port und das SSL-Zertifikat zu.

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name "listener01" -Protocol "Https" -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01 -SslCertificate $cert
```

### <a name="step-6"></a>Schritt 6

Erstellen Sie einen benutzerdefinierten Test für den `ContosoApi`-Proxy-Domänenendpunkt des API Management-Diensts. Der Pfad `/status-0123456789abcdef` ist ein Standard-Integritätsendpunkt, der von allen API Management-Diensten gehostet wird. Legen Sie `api.contoso.net` als benutzerdefinierten Testhostnamen fest, den Sie mit einem SSL-Zertifikat schützen möchten.

> [!NOTE]
> Der Hostname `contosoapi.azure-api.net` ist der Proxy-Standardhostname, der konfiguriert wird, wenn im öffentlichen Azure-Bereich der Dienst `contosoapi` erstellt wird. 
> 

```powershell
$apimprobe = New-AzureRmApplicationGatewayProbeConfig -Name "apimproxyprobe" -Protocol "Https" -HostName "api.contoso.net" -Path "/status-0123456789abcdef" -Interval 30 -Timeout 120 -UnhealthyThreshold 8
```

### <a name="step-7"></a>Schritt 7

Laden Sie das Zertifikat hoch, das auf den SSL-fähigen Ressourcen des Back-End-Pools verwendet werden soll. Dies ist das gleiche Zertifikat, das Sie in Schritt 4 bereitgestellt haben.

```powershell
$authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name "whitelistcert1" -CertificateFile <full path to .cer file>
```

### <a name="step-8"></a>Schritt 8

Konfigurieren Sie die HTTP-Back-End-Einstellungen für das Application Gateway. Dies umfasst auch das Festlegen eines zeitlichen Grenzwerts für Back-End-Anforderungen, die abgebrochen werden, wenn der Grenzwert erreicht wird. Dieser Wert unterscheidet sich vom Testtimeout.

```powershell
$apimPoolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name "apimPoolSetting" -Port 443 -Protocol "Https" -CookieBasedAffinity "Disabled" -Probe $apimprobe -AuthenticationCertificates $authcert -RequestTimeout 180
```

### <a name="step-9"></a>Schritt 9

Konfigurieren Sie einen Back-End-IP-Adresspool namens **apimbackend** mit der internen virtuellen IP-Adresse des oben erstellten API Management-Diensts.

```powershell
$apimProxyBackendPool = New-AzureRmApplicationGatewayBackendAddressPool -Name "apimbackend" -BackendIPAddresses $apimService.StaticIPs[0]
```

### <a name="step-10"></a>Schritt 10

Erstellen Sie die Einstellungen für ein (nicht existierendes) Pseudo-Back-End. Anforderungen von API-Pfaden, die nicht über Application Gateway von API Management verfügbar gemacht werden sollen, treffen auf dieses Back-End und geben 404 zurück.

Konfigurieren Sie die HTTP-Einstellungen für das Pseudo-Back-End.

```powershell
$dummyBackendSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name "dummySetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled
```

Konfigurieren Sie ein Pseudo-Back-End namens **dummyBackendPool**, das auf eine FQDN-Adresse namens **dummybackend.com** zeigt. Diese FQDN-Adresse ist im virtuellen Netzwerk nicht vorhanden.

```powershell
$dummyBackendPool = New-AzureRmApplicationGatewayBackendAddressPool -Name "dummyBackendPool" -BackendFqdns "dummybackend.com"
```

Erstellen Sie eine Regeleinstellung, die Application Gateway standardmäßig verwendet, die auf das nicht vorhandene Back-End **dummybackend.com** im virtuellen Netzwerk zeigt.

```powershell
$dummyPathRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "nonexistentapis" -Paths "/*" -BackendAddressPool $dummyBackendPool -BackendHttpSettings $dummyBackendSetting
```

### <a name="step-11"></a>Schritt 11

Konfigurieren Sie URL-Regelpfade für die Back-End-Pools. Sie haben dann die Möglichkeit, nur einige APIs von API Management auszuwählen, die öffentlich verfügbar sein sollen. Beispiel: Wenn `Echo API` (/echo/), `Calculator API` (/calc/) usw. vorhanden sind, machen Sie nur `Echo API` für den Zugriff über das Internet verfügbar. 

Im folgenden Beispiel wird eine einfache Regel für den Pfad „/echo/" erstellt, um Datenverkehr an das Back-End „apimProxyBackendPool“ zu leiten.

```powershell
$echoapiRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "externalapis" -Paths "/echo/*" -BackendAddressPool $apimProxyBackendPool -BackendHttpSettings $apimPoolSetting
```

Die Konfiguration der Regelpfadzuordnung konfiguriert auch dann einen Standard-Back-End-Adresspool namens **dummyBackendPool**, wenn der Pfad keiner der Pfadregeln entspricht, die wir für API Management anwenden möchten. Beispiel: http://api.contoso.net/calc/* wird an **dummyBackendPool** weitergeleitet, da es als Standardpool für nicht übereinstimmenden Datenverkehr definiert wurde.

```powershell
$urlPathMap = New-AzureRmApplicationGatewayUrlPathMapConfig -Name "urlpathmap" -PathRules $echoapiRule, $dummyPathRule -DefaultBackendAddressPool $dummyBackendPool -DefaultBackendHttpSettings $dummyBackendSetting
```

Mit den obigen Schritten wird sichergestellt, dass nur Anforderungen für den Pfad „/echo“ über das Application Gateway gesendet werden können. Anforderungen an andere APIs, die in API Management konfiguriert sind, lösen Fehler vom Typ 404 für das Application Gateway aus, wenn der Zugriff über das Internet erfolgt. 

### <a name="step-12"></a>Schritt 12

Erstellen Sie eine Regeleinstellung für das Application Gateway zur Verwendung des Routings basierend auf URL-Pfaden.

```powershell
$rule01 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule1" -RuleType PathBasedRouting -HttpListener $listener -UrlPathMap $urlPathMap
```

### <a name="step-13"></a>Schritt 13

Konfigurieren Sie die Anzahl von Instanzen und die Größe für das Application Gateway. Hier verwenden wir die [WAF-SKU](../application-gateway/application-gateway-webapplicationfirewall-overview.md), um eine höhere Sicherheit für die API Management-Ressource zu erzielen.

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name "WAF_Medium" -Tier "WAF" -Capacity 2
```

### <a name="step-14"></a>Schritt 14

Konfigurieren Sie für die WAF den „Präventionsmodus“.
```powershell
$config = New-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode "Prevention"
```

## <a name="create-application-gateway"></a>Erstellen eines Application Gateways

Erstellen Sie ein Application Gateway mit allen Konfigurationsobjekten aus den vorherigen Schritten.

```powershell
$appgw = New-AzureRmApplicationGateway -Name $applicationGatewayName -ResourceGroupName $resourceGroupName  -Location $location -BackendAddressPools $apimProxyBackendPool, $dummyBackendPool -BackendHttpSettingsCollection $apimPoolSetting, $dummyBackendSetting  -FrontendIpConfigurations $fipconfig01 -GatewayIpConfigurations $gipconfig -FrontendPorts $fp01 -HttpListeners $listener -UrlPathMaps $urlPathMap -RequestRoutingRules $rule01 -Sku $sku -WebApplicationFirewallConfig $config -SslCertificates $cert -AuthenticationCertificates $authcert -Probes $apimprobe
```

## <a name="cname-the-api-management-proxy-hostname-to-the-public-dns-name-of-the-application-gateway-resource"></a>Erstellen eines CNAME-Eintrags für den API Management-Proxyhostnamen zum Namen des öffentlichen DNS der Application Gateway-Ressource

Nach dem Erstellen des Gateways wird das Front-End für die Kommunikation konfiguriert. Wenn Sie eine öffentliche IP-Adresse verwenden, wird für das Application Gateway ein dynamisch zugewiesener DNS-Namen benötigt, der ggf. nicht benutzerfreundlich ist. 

Verwenden Sie den DNS-Namen des Application Gateway zum Erstellen eines CNAME-Eintrags, mit dem für den APIM-Proxyhostnamen (z.B. `api.contoso.net` in den obigen Beispielen) auf diesen DNS-Namen verwiesen wird. Rufen Sie zum Konfigurieren des CNAME-Eintrags für die Front-End-IP mithilfe des PublicIPAddress-Elements Details zum Application Gateway und zum zugeordneten IP/DNS-Namen ab. Die Verwendung von A-Einträgen wird nicht empfohlen, da sich die VIP beim Neustart des Gateways unter Umständen ändert.

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName "apim-appGw-RG" -Name "publicIP01"
```

##<a name="summary"> </a> Zusammenfassung
Bei einer Konfiguration von Azure API Management in einem VNET wird eine einzelne Gatewayschnittstelle für alle konfigurierten APIs bereitgestellt. Hierbei spielt es keine Rolle, ob sie lokal oder in der Cloud gehostet werden. Mit der Integration des Application Gateway in API Management verfügen Sie über die Flexibilität, für bestimmte APIs den Zugriff über das Internet selektiv zu aktivieren, und können eine Web Application Firewall als Front-End für Ihre API Management-Instanz bereitstellen.

##<a name="next-steps"> </a> Nächste Schritte
* Weitere Informationen zu Azure Application Gateway
  * [Übersicht über Application Gateway](../application-gateway/application-gateway-introduction.md)
  * [Web Application Firewall für Application Gateway](../application-gateway/application-gateway-webapplicationfirewall-overview.md)
  * [Anwendungsgateways mit pfadbasiertem Routing](../application-gateway/application-gateway-create-url-route-arm-ps.md)
* Weitere Informationen zu API Management und VNETs
  * [Verwenden von Azure API Management mit internen virtuellen Netzwerken](api-management-using-with-internal-vnet.md)
  * [Verwenden von API Management im VNET](api-management-using-with-vnet.md)

