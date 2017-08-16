---
title: Erstellen eines Application Gateways mit URL-Routingregeln | Microsoft Docs
description: "Diese Seite enthält Anweisungen zum Erstellen und Konfigurieren eines Azure Application Gateways mit URL-Routingregeln."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: d141cfbb-320a-4fc9-9125-10001c6fa4cf
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/03/2017
ms.author: gwallace
ms.translationtype: HT
ms.sourcegitcommit: 8b857b4a629618d84f66da28d46f79c2b74171df
ms.openlocfilehash: ba756d3262b9780c5701e69faad860ba32bba08b
ms.contentlocale: de-de
ms.lasthandoff: 08/04/2017

---
# <a name="create-an-application-gateway-using-path-based-routing"></a>Erstellen eines Anwendungsgateways mit pfadbasiertem Routing

> [!div class="op_single_selector"]
> * [Azure-Portal](application-gateway-create-url-route-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-url-route-arm-ps.md)
> * [Azure CLI 2.0](application-gateway-create-url-route-cli.md)

Mit Routing auf URL-Pfadbasis können Sie Routen basierend auf dem URL-Pfad einer HTTP-Anforderung zuordnen. Es wird überprüft, ob eine Route zu einem Back-End-Pool für die URL konfiguriert ist, die im Anwendungsgateway angezeigt wird. Dann wird der Netzwerkdatenverkehr an den definierten Back-End-Pool gesendet. Ein gängiges Szenario für URL-basiertes Routing ist der Lastenausgleich von Anforderungen für verschiedene Inhaltstypen auf verschiedene Back-End-Serverpools.

Mit dem URL-basierten Routing wird ein neuer Regeltyp für Anwendungsgateways eingeführt. Anwendungsgateways verfügen über zwei Regeltypen: Basisrouting und PathBasedRouting. Der Basisregeltyp bietet einen Roundrobin-Dienst für die Back-End-Pools, während PathBasedRouting neben der Roundrobin-Verteilung auch Pfadmuster der Anforderungs-URL beim Auswählen des Back-End-Pools berücksichtigt.

## <a name="scenario"></a>Szenario

Im folgenden Beispiel verarbeitet das Application Gateway Datenverkehr für „contoso.com“ mit zwei Back-End-Pools: Video-Serverpool und Image-Serverpool.

Anforderungen für „http://contoso.com/image*“ werden an den Imageserverpool (pool1) geleitet, Anforderungen für „http://contoso.com/video*“ an den Videoserverpool (pool2). Wenn keines der Pfadmuster zutrifft, wird ein Standardserverpool (pool1) ausgewählt.

![URL-Route](./media/application-gateway-create-url-route-arm-ps/figure1.png)

## <a name="before-you-begin"></a>Voraussetzungen

1. Installieren Sie mit dem Webplattform-Installer die aktuelle Version der Azure PowerShell-Cmdlets. Sie können die neueste Version aus dem Abschnitt **Windows PowerShell** der [Downloadseite](https://azure.microsoft.com/downloads/)herunterladen und installieren.
2. Sie erstellen ein virtuelles Netzwerk und ein Subnetz für das Anwendungsgateway. Stellen Sie sicher, dass keine virtuellen Maschinen oder Cloudbereitstellungen das Subnetz verwenden. Das Application Gateway muss sich allein im Subnetz eines virtuellen Netzwerks befinden.
3. Die Server, die Sie dem Back-End-Pool zur Verwendung des Application Gateways hinzugefügt haben, müssen vorhanden sein oder Endpunkte aufweisen, die im virtuellen Netzwerk erstellt wurden, oder denen eine öffentliche IP-Adresse/VIP zugewiesen wurde.

## <a name="what-is-required-to-create-an-application-gateway"></a>Was ist zum Erstellen eines Application Gateways erforderlich?

* **Back-End-Serverpool:** Die Liste der IP-Adressen der Back-End-Server. Die aufgelisteten IP-Adressen sollten entweder dem Subnetz des virtuellen Netzwerks angehören oder eine öffentliche IP-Adresse/VIP sein.
* **Einstellungen für den Back-End-Serverpool:** Jeder Pool weist Einstellungen wie Port, Protokoll und cookiebasierte Affinität auf. Diese Einstellungen sind an einen Pool gebunden und gelten für alle Server innerhalb des Pools.
* **Front-End-Port:** Dieser Port ist der öffentliche Port, der im Application Gateway geöffnet ist. Datenverkehr erreicht diesen Port und wird dann an einen der Back-End-Server umgeleitet.
* **Listener:** Der Listener verfügt über einen Front-End-Port, ein Protokoll („Http“ oder „Https“; jeweils unter Beachtung der Groß-/Kleinschreibung) und den Namen des SSL-Zertifikats (falls die SSL-Auslagerung konfiguriert wird).
* **Regel:** Mit der Regel werden der Listener und der Back-End-Serverpool verbunden, und es wird definiert, an welchen Back-End-Serverpool der Datenverkehr geleitet werden soll, wenn er einen bestimmten Listener erreicht.

## <a name="create-an-application-gateway"></a>Erstellen eines Anwendungsgateways

Der Unterschied zwischen dem klassischen Azure-Modell und Azure Resource Manager besteht in der Reihenfolge, in der Sie ein Anwendungsgateway und die Elemente erstellen, die konfiguriert werden müssen.

Beim Resource Manager werden alle Elemente, die ein Anwendungsgateway bilden, einzeln konfiguriert und anschließend zusammengesetzt, um die Anwendungsgatewayressource zu erstellen.

Hier sind die erforderlichen Schritte zum Erstellen eines Anwendungsgateways angegeben:

1. Erstellen einer Ressourcengruppe für den Ressourcen-Manager
2. Erstellen eines virtuelles Netzwerks, Subnetzes und einer öffentlichen IP-Adresse für das Application Gateway
3. Erstellen eines Konfigurationsobjekts für das Anwendungsgateway
4. Erstellen einer Application Gateway-Ressource

## <a name="create-a-resource-group-for-resource-manager"></a>Erstellen einer Ressourcengruppe für den Ressourcen-Manager

Stellen Sie sicher, dass Sie die neueste Version von Azure PowerShell verwenden. Weitere Informationen finden Sie unter [Verwenden von Windows PowerShell mit Resource Manager](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Schritt 1

Anmelden an Azure

```powershell
Login-AzureRmAccount
```

Sie werden zur Authentifizierung mit Ihren Anmeldeinformationen aufgefordert.<BR>

### <a name="step-2"></a>Schritt 2

Überprüfen Sie die Abonnements für das Konto.

```powershell
Get-AzureRmSubscription
```

### <a name="step-3"></a>Schritt 3

Wählen Sie aus, welches Azure-Abonnement Sie verwenden möchten. <BR>

```powershell
Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
```

### <a name="step-4"></a>Schritt 4

Erstellen Sie eine Ressourcengruppe. (Überspringen Sie diesen Schritt, wenn Sie eine vorhandene Ressourcengruppe verwenden.)

```powershell
$resourceGroup = New-AzureRmResourceGroup -Name appgw-RG -Location "West US"
```

Alternativ können Sie auch Tags für eine Ressourcengruppe für das Application Gateway erstellen:

```powershell
$resourceGroup = New-AzureRmResourceGroup -Name appgw-RG -Location "West US" -Tags @{Name = "testtag"; Value = "Application Gateway URL routing"} 
```

Der Azure Resource Manager erfordert, dass alle Ressourcengruppen einen Speicherort angeben. Diese Ressourcengruppe wird als Standardspeicherort für Ressourcen in dieser Ressourcengruppe verwendet. Stellen Sie sicher, dass alle Befehle, mit denen ein Anwendungsgateway erstellt wird, die gleiche Ressourcengruppe verwenden.

Im obigen Beispiel haben wir eine Ressourcengruppe namens „appgw-RG“ mit dem Standort „USA, Westen“ erstellt.

> [!NOTE]
> Falls Sie einen benutzerdefinierten Test für Ihr Anwendungsgateway konfigurieren müssen, finden Sie die entsprechenden Informationen unter [Erstellen eines Anwendungsgateways mit benutzerdefinierten Tests mithilfe von PowerShell](application-gateway-create-probe-ps.md). Weitere Informationen finden Sie unter [Benutzerdefinierte Tests und Systemüberwachung](application-gateway-probe-overview.md) .
> 
> 

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Erstellen eines virtuelles Netzwerkes und eines Subnetzes für das Application Gateway

Das folgende Beispiel zeigt, wie Sie mit dem Ressourcen-Manager ein virtuelles Netzwerk erstellen: In diesem Beispiel wird ein VNET für das Anwendungsgateway erstellt. Da das Anwendungsgateway ein eigenes Subnetz benötigt, ist das für das Anwendungsgateway erstellte Subnetz kleiner als der VNET-Adressraum. Dadurch können andere Ressourcen – einschließlich Webserver – im gleichen VNET konfiguriert werden.

### <a name="step-1"></a>Schritt 1

Weisen Sie den Adressbereich 10.0.0.0/24 der Subnetzvariablen zu, die zum Erstellen eines virtuelles Netzwerks verwendet wird.  Dadurch wird das im nächsten Beispiel verwendete Subnetzkonfigurationsobjekt für das Anwendungsgateway erstellt.

```powershell
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24
```

### <a name="step-2"></a>Schritt 2

Erstellen Sie ein virtuelles Netzwerk mit dem Namen **appgwvnet** in der Ressourcengruppe **appgw-rg** für die Region „USA, Westen“ mit dem Präfix „10.0.0.0/16“ und dem Subnetz „10.0.0.0/24“. Damit ist die Konfiguration des VNETs mit einem einzelnen Subnetz für das Anwendungsgateway abgeschlossen.

```powershell
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet
```

### <a name="step-3"></a>Schritt 3

Weisen Sie die Subnetzvariable für die nächsten Schritte zu. Diese Variable wird in einem späteren Schritt an das `New-AzureRMApplicationGateway`-Cmdlet übergeben.

```powershell
$subnet=$vnet.Subnets[0]
```

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Erstellen der öffentlichen IP-Adresse für die Front-End-Konfiguration

Erstellen Sie eine öffentliche IP-Ressource namens **publicIP01** in der Ressourcengruppe **appgw-rg** für die Region „USA, Westen“. Das Anwendungsgateway kann eine öffentliche IP-Adresse und/oder eine interne IP-Adresse verwenden, um Anforderungen für den Lastenausgleich zu empfangen.  In diesem Beispiel wird nur eine öffentliche IP-Adresse verwendet. Im folgenden Beispiel wird für die Erstellung der öffentlichen IP-Adresse kein DNS-Name konfiguriert.  Das Anwendungsgateway unterstützt keine benutzerdefinierten DNS-Namen für öffentliche IP-Adressen.  Falls für einen öffentlichen Endpunkt ein benutzerdefinierter Name erforderlich ist, muss ein CNAME-Eintrag erstellt werden, um auf den automatisch generierten DNS-Namen für die öffentliche IP-Adresse zu verweisen.

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -name publicIP01 -location "West US" -AllocationMethod Dynamic
```

Dem Anwendungsgateway wird beim Starten des Diensts eine IP-Adresse zugewiesen.

## <a name="create-application-gateway-configuration"></a>Erstellen einer Application Gateway-Konfiguration

Sie müssen alle Konfigurationselemente einrichten, bevor Sie das Anwendungsgateway erstellen. Die folgenden Schritten erstellen die Konfigurationselemente, die für eine Application Gateway-Ressource benötigt werden.

### <a name="step-1"></a>Schritt 1

Erstellen Sie für das Anwendungsgateway eine IP-Konfiguration mit dem Namen **gatewayIP01**. Beim Starten des Anwendungsgateways wird eine IP-Adresse aus dem konfigurierten Subnetz ausgewählt, und der Netzwerkdatenverkehr wird an die IP-Adressen im Back-End-IP-Pool weitergeleitet. Beachten Sie, dass jede Instanz eine eigene IP-Adresse benötigt.

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet
```

### <a name="step-2"></a>Schritt 2

Konfigurieren Sie die Back-End-IP-Adresspools **pool1** und **pool2** mit den IP-Adressen für **pool1** und **pool2**. Bei diesen IP-Adressen handelt es sich um die IP-Adressen der Ressourcen, auf denen die Webanwendung gehostet wird, die durch das Anwendungsgateway geschützt werden soll. Die Integrität dieser Mitglieder des Back-End-Pools wurde jeweils durch Basistests oder benutzerdefinierte Tests überprüft.  An sie wird Datenverkehr weitergeleitet, wenn Anforderungen beim Anwendungsgateway eingehen. Back-End-Pools können von mehreren Regeln innerhalb des Anwendungsgateways verwendet werden. Ein Back-End-Pool kann also für mehrere Webanwendungen verwendet werden, die sich auf dem gleichen Host befinden.

```powershell
$pool1 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221, 134.170.185.50

$pool2 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool02 -BackendIPAddresses 134.170.186.47, 134.170.189.222, 134.170.186.51
```

Dieses Beispiel enthält zwei Back-End-Pools zum Weiterleiten von Netzwerkverkehr basierend auf dem URL-Pfad. Ein Pool empfängt Datenverkehr aus dem URL-Pfad „/video“, der andere Pool empfängt Datenverkehr aus dem Pfad „/image“. Ersetzen Sie die obigen IP-Adressen durch die IP-Adressendpunkte Ihrer eigenen Anwendung. 

### <a name="step-3"></a>Schritt 3

Konfigurieren Sie die Application Gateway-Einstellungen **poolsetting01** und **poolsetting02** für den Lastenausgleich des Netzwerkdatenverkehrs im Back-End-Pool. In diesem Beispiel konfigurieren Sie verschiedene Back-End-Pool-Einstellungen für die Back-End-Pools. Jeder Back-End-Pool kann eine eigene Back-End-Pool-Einstellung aufweisen.  Back-End-HTTP-Einstellungen werden von Regeln verwendet, um Datenverkehr an die richtigen Mitglieder des Back-End-Pools weiterzuleiten. Die Einstellungen bestimmen das Protokoll und den Port, das bzw. der beim Senden von Datenverkehr an die Mitglieder des Back-End-Pools verwendet wird. Cookiebasierte Sitzungen werden ebenfalls durch die Back-End-HTTP-Einstellungen gesteuert.  Im aktivierten Zustand sendet die cookiebasierte Sitzungsaffinität Datenverkehr an das gleiche Back-End wie vorherige Anforderungen für das jeweilige Paket.

```powershell
$poolSetting01 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled -RequestTimeout 120

$poolSetting02 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting02" -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 240
```

### <a name="step-4"></a>Schritt 4

Konfigurieren Sie die Front-End-IP-Adresse mit dem öffentlichen IP-Endpunkt. Das Front-End-IP-Konfigurationsobjekt wird von einem Listener verwendet, um die von außen erreichbare IP-Adresse mit dem Listener zu verknüpfen.

```powershell
$fipconfig01 = New-AzureRmApplicationGatewayFrontendIPConfig -Name "frontend1" -PublicIPAddress $publicip
```

### <a name="step-5"></a>Schritt 5

Konfigurieren Sie den Front-End-Port für ein Application Gateway. Das Front-End-Port-Konfigurationsobjekt wird von einen Listener verwendet, um den Port zu definieren, an dem das Anwendungsgateway auf Datenverkehr des Listeners lauscht.

```powershell
$fp01 = New-AzureRmApplicationGatewayFrontendPort -Name "fep01" -Port 80
```

### <a name="step-6"></a>Schritt 6

Konfigurieren Sie den Listener. Dieser Schritt konfiguriert den Listener für die öffentliche IP-Adresse und den Port zum Empfangen von eingehendem Netzwerkverkehr. Im folgenden Beispiel wird der Listener mit der zuvor konfigurierten Front-End-IP- und Front-End-Portkonfiguration sowie mit einem Protokoll (HTTP oder HTTPS) konfiguriert. In diesem Beispiel lauscht der Listener an Port 80 auf HTTP-Datenverkehr für die zuvor erstellte öffentliche IP-Adresse.

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name "listener01" -Protocol Http -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01
```

### <a name="step-7"></a>Schritt 7

Konfigurieren Sie URL-Regelpfade für die Back-End-Pools. Dieser Schritt konfiguriert den relativen Pfad, der vom Anwendungsgateway verwendet wird, und definiert die Zuordnung zwischen dem URL-Pfad und dem Back-End-Pool, der zum Verarbeiten des eingehenden Datenverkehrs zugewiesen wurde.

> [!IMPORTANT]
> Jeder Pfad muss mit „/“ beginnen, und ein „\*“ ist nur am Ende zulässig. Gültige Beispiele sind „/xyz“, „/xyz*“ oder „/xyz/*“. Die Zeichenfolge, die in den Pfadabgleicher eingegeben wird, enthält keinen Text nach dem ersten „?“ oder „#“, und diese Zeichen sind nicht zulässig. 

Im folgenden Beispiel werden zwei Regeln erstellt: eine für den Pfad „/image/“, der Datenverkehr an Back-End „pool1“ leitet, und eine andere für den Pfad „/video/“, der Datenverkehr an Back-End „pool2“ leitet. Diese Regeln stellen sicher, dass der Datenverkehr für jeden Satz URLs an das Back-End weitergeleitet wird. Beispiel: „http://contoso.com/image/figure1.jpg“ wird an „pool1“ weitergeleitet, „http://contoso.com/video/example.mp4“ an „pool2“.

```powershell
$imagePathRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "pathrule1" -Paths "/image/*" -BackendAddressPool $pool1 -BackendHttpSettings $poolSetting01

$videoPathRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "pathrule2" -Paths "/video/*" -BackendAddressPool $pool2 -BackendHttpSettings $poolSetting02
```

Die Konfiguration der Regelpfadzuordnung konfiguriert auch dann einen Standard-Back-End-Adresspool, wenn der Pfad keiner der vordefinierten Pfadregeln entspricht. Beispiel: „http://contoso.com/shoppingcart/test.html“ wird an „pool1“ weitergeleitet, da dieser als Standardpool für nicht übereinstimmenden Datenverkehr definiert wurde.

```powershell
$urlPathMap = New-AzureRmApplicationGatewayUrlPathMapConfig -Name "urlpathmap" -PathRules $videoPathRule, $imagePathRule -DefaultBackendAddressPool $pool1 -DefaultBackendHttpSettings $poolSetting02
```

### <a name="step-8"></a>Schritt 8

Erstellen Sie eine Regeleinstellung. Dieser Schritt konfiguriert das Anwendungsgateway für die Verwendung des Routings auf URL-Pfadbasis. Die in einem vorherigen Schritt definierte Variable `$urlPathMap` wird jetzt verwendet, um die pfadbasierte Regel zu erstellen. In diesem Schritt wird die Regel mit einem Listener und der zuvor erstellten URL-Pfadzuordnung verknüpft.

```powershell
$rule01 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule1" -RuleType PathBasedRouting -HttpListener $listener -UrlPathMap $urlPathMap
```

### <a name="step-9"></a>Schritt 9

Konfigurieren Sie die Anzahl der Instanzen und die Größe für das Application Gateway.

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name "Standard_Small" -Tier Standard -Capacity 2
```

## <a name="create-application-gateway"></a>Erstellen eines Application Gateways

Erstellen Sie ein Anwendungsgateway mit allen Konfigurationsobjekten aus den vorherigen Schritten.

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-RG -Location "West US" -BackendAddressPools $pool1,$pool2 -BackendHttpSettingsCollection $poolSetting01, $poolSetting02 -FrontendIpConfigurations $fipconfig01 -GatewayIpConfigurations $gipconfig -FrontendPorts $fp01 -HttpListeners $listener -UrlPathMaps $urlPathMap -RequestRoutingRules $rule01 -Sku $sku
```

## <a name="get-application-gateway-dns-name"></a>Abrufen des DNS-Namens des Anwendungsgateways

Nach dem Erstellen des Gateways wird das Front-End für die Kommunikation konfiguriert. Wenn Sie eine öffentliche IP-Adresse verwenden, wird das Anwendungsgateway ein dynamisch zugewiesener DNS-Namen benötigt, der kein Anzeigename ist. Um sicherzustellen, dass die Endbenutzer auf das Anwendungsgateway zugreifen können, kann mit einem CNAME-Eintrag auf den öffentlichen Endpunkt des Anwendungsgateways verwiesen werden. [Konfigurieren eines benutzerdefinierten Domänennamens in Azure](../cloud-services/cloud-services-custom-domain-name-portal.md) Rufen Sie zum Konfigurieren des CNAME-Eintrags für die Front-End-IP mithilfe des PublicIPAddress-Elements, das an das Anwendungsgateway angefügt ist, Details zum Anwendungsgateway und dem zugeordneten IP/DNS-Namen ab. Der DNS-Name des Anwendungsgateways sollte verwendet werden, um einen CNAME-Eintrag zu erstellen. Die Verwendung von A-Einträgen wird nicht empfohlen, da sich die VIP beim Neustart des Anwendungsgateways möglicherweise verändert.

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

Informationen zur SSL-Auslagerung (Secure Sockets Layer) finden Sie unter [Konfigurieren eines Anwendungsgateways für die SSL-Auslagerung](application-gateway-ssl-arm.md).


