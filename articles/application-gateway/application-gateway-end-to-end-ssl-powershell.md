---
title: Konfigurieren der SSL-Richtlinie und von End-to-End-SSL mit Application Gateway | Microsoft Docs
description: "Dieser Artikel beschreibt das Konfigurieren von End-to-End-SSL mit Application Gateway über Azure Resource Manager-PowerShell"
services: application-gateway
documentationcenter: na
author: georgewallace
manager: carmonm
editor: tysonn
ms.assetid: e6d80a33-4047-4538-8c83-e88876c8834e
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/16/2016
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: ee8cfffdbf054b4251ed269745f6b9ee5a5e6c64
ms.openlocfilehash: 6f061810b865e7855877bf08b02e758351f3e63c


---
# <a name="configure-ssl-policy-and-end-to-end-ssl-with-application-gateway-using-powershell"></a>Konfigurieren der SSL-Richtlinie und von End-to-End-SSL mit Application Gateway über PowerShell

## <a name="overview"></a>Übersicht

Application Gateway unterstützt die End-to-End-Verschlüsselung des Datenverkehrs. Hierfür wird in Application Gateway die SSL-Verbindung am Anwendungsgateway beendet. Das Gateway wendet dann die Routingregeln auf den Datenverkehr an, verschlüsselt das Paket erneut und leitet das Paket basierend auf den definierten Routingregeln an das entsprechende Back-End weiter. Antworten vom Webserver durchlaufen denselben Prozess zurück an den Endbenutzer.

Eine weitere Funktion, die von Application Gateway unterstützt wird, ist das Deaktivieren bestimmter Versionen des SSL-Protokolls. Application Gateway unterstützt das Deaktivieren der folgenden Protokollversionen: **TLSv1.0**, **TLSv1.1** und **TLSv1.2**.

> [!NOTE]
> SSL 2.0 und SSL 3.0 sind standardmäßig deaktiviert und können nicht aktiviert werden. Sie werden als unsicher eingestuft und können mit Application Gateway nicht verwendet werden.
> 
> 

![Szenarioabbildung][scenario]

## <a name="scenario"></a>Szenario

In diesem Szenario erfahren Sie, wie Sie mithilfe von PowerShell ein Anwendungsgateway über End-to-End-SSL erstellen.

Dieses Szenario umfasst Folgendes:

* Erstellen einer Ressourcengruppe namens **appgw-rg**
* Erstellen eines virtuellen Netzwerks namens **appgwvnet** mit dem reservierten CIDR-Block 10.0.0.0/16.
* Erstellen zweier Subnetze namens **appgwsubnet** und **appsubnet**.
* Erstellen eines kleinen Anwendungsgateways mit Unterstützung der End-to-End-SSL-Verschlüsselung, das bestimmte SSL-Protokolle deaktiviert.

## <a name="before-you-begin"></a>Voraussetzungen

Zum Konfigurieren von End-to-End-SSL mit einem Anwendungsgateway wird ein Zertifikat für das Gateway benötigt, und weitere Zertifikate sind für die Back-End-Server erforderlich. Das Gatewayzertifikat wird zum Verschlüsseln und Entschlüsseln des Datenverkehrs verwendet, der über SSL an das Gateway gesendet wird. Das Gatewayzertifikat muss im PFX-Format (Personal Information Exchange, privater Informationsaustausch) vorliegen. In diesem Dateiformat kann der private Schlüssel exportiert werden, was erforderlich ist, damit das Anwendungsgateway die Ver- und Entschlüsselung des Datenverkehrs durchführen kann.

Für die End-to-End-SSL-Verschlüsselung muss das Back-End beim Anwendungsgateway auf der Whitelist stehen. Dies geschieht durch Hochladen des öffentlichen Zertifikats der Back-Ends auf das Anwendungsgateway. Dadurch wird sichergestellt, dass das Anwendungsgateway nur mit bekannten Back-End-Instanzen kommuniziert. Außerdem wird auf diese Weise die End-to-End-Kommunikation gesichert.

Dieser Prozess wird in den folgenden Schritten beschrieben:

## <a name="create-the-resource-group"></a>Erstellen der Ressourcengruppe

Dieser Abschnitt führt Sie durch die Erstellung einer Ressourcengruppe, die das Anwendungsgateway enthält.

### <a name="step-1"></a>Schritt 1

Melden Sie sich beim Azure-Konto an.

```powershell
Login-AzureRmAccount
```

### <a name="step-2"></a>Schritt 2

Wählen Sie das Abonnement aus, das für dieses Szenario verwendet werden soll.

```powershell
Select-AzureRmsubscription -SubscriptionName "<Subscription name>"
```

### <a name="step-3"></a>Schritt 3

Erstellen Sie eine Ressourcengruppe. (Überspringen Sie diesen Schritt, wenn Sie eine vorhandene Ressourcengruppe verwenden.)

```powershell
New-AzureRmResourceGroup -Name appgw-rg -Location "West US"
```

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Erstellen eines virtuelles Netzwerkes und eines Subnetzes für das Application Gateway

Im folgenden Beispiel werden ein virtuelles Netzwerk und zwei Subnetze erstellt. In einem Subnetz befindet sich das Anwendungsgateway. Das andere Subnetz wird für die Back-Ends verwendet, die die Webanwendung hosten.

### <a name="step-1"></a>Schritt 1

Weisen Sie einen Adressbereich für das Subnetz zu, das für das Anwendungsgateway verwendet werden soll.

```powershell
$gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24
```

> [!NOTE]
> Die Größen der für das Anwendungsgateway konfigurierten Subnetze sollten richtig bemessen sein. Ein Anwendungsgateway kann für bis zu 10 Instanzen konfiguriert werden. Jede Instanz erhält dabei eine IP-Adresse vom Subnetz. Ein zu kleines Subnetz kann sich negativ auf die Erweiterungsmöglichkeiten eines Anwendungsgateways auswirken.
> 
> 

### <a name="step-2"></a>Schritt 2

Weisen Sie einen Adressbereich zu, der für den Back-End-Adresspool verwendet werden soll.

```powershell
$nicSubnet = New-AzureRmVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24
```

### <a name="step-3"></a>Schritt 3

Erstellen Sie ein virtuelles Netzwerk mit den Subnetzen, die in den vorhergehenden Schritten definiert wurden.

```powershell
$vnet = New-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet
```

### <a name="step-4"></a>Schritt 4

Rufen Sie die virtuelle Netzwerkressource und die Subnetzressourcen ab, die in den folgenden Schritten verwendet werden sollen:

```powershell
$vnet = Get-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg
$gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -VirtualNetwork $vnet
$nicSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appsubnet' -VirtualNetwork $vnet
```

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Erstellen der öffentlichen IP-Adresse für die Front-End-Konfiguration

Erstellen Sie eine öffentliche IP-Ressource, die für das Anwendungsgateway verwendet werden soll. Diese öffentliche IP-Adresse wird in einem folgenden Schritt verwendet.

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -Name 'publicIP01' -Location "West US" -AllocationMethod Dynamic
```

> [!IMPORTANT]
> Application Gateway unterstützt nicht die Verwendung einer öffentlichen IP-Adresse, die mit einer definierten Domänenbezeichnung erstellt wird. Nur eine öffentliche IP-Adresse mit einer dynamisch erstellten Domänenbezeichnung wird unterstützt. Wenn Sie einen benutzerfreundlichen DNS-Namen für das Anwendungsgateway benötigen, wird empfohlen, einen cname-Datensatz als Alias zu verwenden.
> 
> 

## <a name="create-an-application-gateway-configuration-object"></a>Erstellen eines Konfigurationsobjekts für das Anwendungsgateway

Sie müssen alle Konfigurationselemente einrichten, bevor Sie das Anwendungsgateway erstellen. Die folgenden Schritten erstellen die Konfigurationselemente, die für eine Application Gateway-Ressource benötigt werden.

### <a name="step-1"></a>Schritt 1

Erstellen Sie eine Anwendungsgateway-IP-Konfiguration. Diese Einstellung konfiguriert, welches Subnetz vom Anwendungsgateway verwendet wird. Beim Starten des Anwendungsgateways wird eine IP-Adresse aus dem konfigurierten Subnetz ausgewählt, und der Netzwerkdatenverkehr wird an die IP-Adressen im Back-End-IP-Pool weitergeleitet. Beachten Sie, dass jede Instanz eine eigene IP-Adresse benötigt.

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet
```

### <a name="step-2"></a>Schritt 2

Erstellen Sie eine Front-End-IP-Konfiguration. Diese Einstellung ordnet dem Front-End des Anwendungsgateways eine private oder öffentliche IP-Adresse zu. Im folgenden Schritt wird die öffentliche IP-Adresse aus dem vorherigen Schritt der Front-End-IP-Konfiguration zugeordnet.

```powershell
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip
```

### <a name="step-3"></a>Schritt 3

Konfigurieren Sie den Back-End-IP-Adresspool mit den IP-Adressen der Back-End-Webserver. Dies sind die IP-Adressen, die den Netzwerkdatenverkehr vom Front-End-IP-Endpunkt empfangen. Sie ersetzen die folgenden IP-Adressen durch die IP-Adressendpunkte Ihrer eigenen Anwendung.

```powershell
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 1.1.1.1, 2.2.2.2, 3.3.3.3
```

> [!NOTE]
> Ein vollqualifizierter Domänenname (FQDN) ist ebenfalls ein gültiger Wert anstelle einer IP-Adresse für die Back-End-Server. Verwenden Sie hierfür die Option „-BackendFqdns“.
> 
> 

### <a name="step-4"></a>Schritt 4

Konfigurieren Sie den Front-End-IP-Port für den öffentlichen IP-Endpunkt. Dieser Port ist der Port, mit dem Endbenutzer eine Verbindung herstellen.

```powershell
$fp = New-AzureRmApplicationGatewayFrontendPort -Name 'port01'  -Port 443
```

### <a name="step-5"></a>Schritt 5

Konfigurieren Sie das Zertifikat für das Anwendungsgateway. Dieses Zertifikat wird zum Entschlüsseln und erneuten Verschlüsseln des Datenverkehrs auf dem Anwendungsgateway verwendet.

```powershell
$cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path to .pfx file> -Password <password for certificate file>
```

> [!NOTE]
> In diesem Beispiel wird das Zertifikat für SSL-Verbindungen konfiguriert. Das Zertifikat muss im PFX-Format vorliegen, und das Kennwort muss zwischen 4 und 12 Zeichen umfassen.
> 
> 

### <a name="step-6"></a>Schritt 6

Erstellen Sie den HTTP-Listener für das Anwendungsgateway. Weisen Sie die zu verwendende Front-End-IP-Konfiguration, den Port und das SSL-Zertifikat zu.

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert
```

### <a name="step-7"></a>Schritt 7

Laden Sie das Zertifikat hoch, das auf den SSL-fähigen Ressourcen des Back-End-Pools verwendet werden soll.

> [!NOTE]
> Der Standardtest ruft den öffentlichen Schlüssel aus der **standardmäßigen** SSL-Bindung in der IP-Adresse des Back-Ends ab und vergleicht den Wert dieses öffentlichen Schlüssels mit dem Wert des öffentlichen Schlüssels, den Sie hier bereitstellen. Der abgerufene öffentliche Schlüssel ist nicht notwendigerweise der Zielort, zu dem der Datenverkehr fließen wird, **wenn** Sie Hostheader und SNI auf dem Back-End verwenden. Öffnen Sie im Zweifelsfall auf den Back-Ends die Seite „https://127.0.0.1/“, um sich zu vergewissern, welches Zertifikat für die **standardmäßige** SSL-Bindung verwendet wird. Verwenden Sie den öffentlichen Schlüssel aus der Aufforderung in diesem Abschnitt. Wenn Sie Hostheader und SNI in HTTPS-Bindungen verwenden und durch eine manuelle Browseranforderung in „https://127.0.0.1/“ auf den Back-Ends keine Antwort und kein Zertifikat erhalten, müssen Sie eine Standard-SSL-Bindung auf den Back-Ends einrichten. Andernfalls werden die Tests nicht erfolgreich sein, und das Back-End wird nicht in die Whitelist aufgenommen.
> 
> 

```powershell
$authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name 'whitelistcert1' -CertificateFile C:\users\gwallace\Desktop\cert.cer
```

> [!NOTE]
> Das in diesem Schritt bereitgestellte Zertifikat sollte dem öffentlichen Schlüssel des PFX-Zertifikats entsprechen, das auf dem Back-End vorliegt. Exportieren Sie das auf dem Back-End-Server im CER-Format installierte Zertifikat (nicht das Stammzertifikat), und verwenden Sie es für diesen Schritt. Durch diesen Schritt wird der Back-End beim Anwendungsgateway auf die Whitelist gesetzt.
> 
> 

### <a name="step-8"></a>Schritt 8

Konfigurieren Sie die HTTP-Einstellungen auf dem Back-End des Anwendungsgateways. Weisen Sie den HTTP-Einstellungen das Zertifikat zu, das im vorherigen Schritt hochgeladen wurde.

```powershell
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name 'setting01' -Port 443 -Protocol Https -CookieBasedAffinity Enabled -AuthenticationCertificates $authcert
```

### <a name="step-9"></a>Schritt 9

Erstellen Sie eine Routingregel für den Lastenausgleich, durch die das Verhalten des Lastenausgleichs konfiguriert wird. In diesem Beispiel wird eine Roundrobin-Basisregel erstellt.

```powershell
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
```

### <a name="step-10"></a>Schritt 10

Konfigurieren Sie die Instanzgröße des Application Gateways.  Die verfügbaren Größen lauten **Standard\_Small**, **Standard\_Medium** und **Standard\_Large**.  Für die Kapazität stehen die Werte 1 bis 10 zur Verfügung.

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2
```

> [!NOTE]
> Zu Testzwecken kann für die Anzahl von Instanzen der Wert 1 ausgewählt werden. Beachten Sie, dass eine Anzahl von weniger als zwei Instanzen nicht durch die SLA abgedeckt ist und daher nicht empfohlen wird. Gateways mit niedriger Nutzungsdauer sind für Dev/Test-Umgebungen vorgesehen und sollten nicht zu Produktionszwecken verwendet werden.
> 
> 

### <a name="step-11"></a>Schritt 11

Konfigurieren Sie die SSL-Richtlinie, die auf dem Anwendungsgateway verwendet werden soll. Application Gateway unterstützt die Möglichkeit, bestimmte Versionen des SSL-Protokolls zu deaktivieren.

Die folgenden Werte sind eine Liste der Protokollversionen, die deaktiviert werden können.

* **TLSv1_0**
* **TLSv1_1**
* **TLSv1_2**

Im folgenden Beispiel wird **TLSv1\_0** deaktiviert.

```powershell
$sslPolicy = New-AzureRmApplicationGatewaySslPolicy -DisabledSslProtocols TLSv1_0
```

## <a name="create-the-application-gateway"></a>Erstellen des Anwendungsgateways

Erstellen Sie das Anwendungsgateway mithilfe der oben aufgeführten Schritte. Die Erstellung des Gateways ist ein langwieriger Prozess.

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgateway -SslCertificates $cert -ResourceGroupName "appgw-rg" -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SslPolicy $sslPolicy -AuthenticationCertificates $authcert -Verbose
```

## <a name="disable-ssl-protocol-versions-on-an-existing-application-gateway"></a>Deaktivieren von SSL-Protokollversionen auf einem vorhandenen Anwendungsgateway

Die vorangehenden Schritte führen Sie durch die Erstellung einer Anwendung mit End-to-End-SSL und durch das Deaktivieren bestimmter Versionen des SSL-Protokolls. Im folgenden Beispiel werden bestimmte SSL-Richtlinien auf einem vorhandenen Anwendungsgateway deaktiviert.

### <a name="step-1"></a>Schritt 1

Rufen Sie das Anwendungsgateway ab, das aktualisiert werden soll.

```powershell
$gw = Get-AzureRmApplicationGateway -Name AdatumAppGateway -ResourceGroupName AdatumAppGatewayRG
```

### <a name="step-2"></a>Schritt 2

Definieren Sie eine SSL-Richtlinie. Im folgenden Beispiel werden TLSv1.0 und TLSv1.1 deaktiviert.

```powershell
Set-AzureRmApplicationGatewaySslPolicy -DisabledSslProtocols TLSv1_0, TLSv1_1 -ApplicationGateway $gw
```

### <a name="step-3"></a>Schritt 3

Zum Schluss aktualisieren Sie das Gateway. Es ist wichtig zu beachten, dass dieser letzte Schritt einen längeren Zeitraum in Anspruch nimmt. Wenn dies abgeschlossen ist, ist End-to-End-SSL auf dem Anwendungsgateway konfiguriert.

```powershell
$gw | Set-AzureRmApplicationGateway
```

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

Informationen über das Verstärken der Sicherheit Ihrer Webanwendungen mit Web Application Firewall über Application Gateway finden Sie unter [Web Application Firewall – Übersicht](application-gateway-webapplicationfirewall-overview.md)

[scenario]: ./media/application-gateway-end-to-end-ssl-powershell/scenario.png



<!--HONumber=Nov16_HO3-->


