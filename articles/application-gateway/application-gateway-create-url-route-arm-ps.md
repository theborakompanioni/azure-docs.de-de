---
title: Erstellen eines Anwendungsgateways mit URL-Routingregeln | Microsoft-Dokumentation
description: "Diese Seite enthält Anweisungen zum Erstellen und Konfigurieren eines Azure-Anwendungsgateways mit URL-Routingregeln."
documentationcenter: na
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.assetid: d141cfbb-320a-4fc9-9125-10001c6fa4cf
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/03/2017
ms.author: davidmu
ms.translationtype: HT
ms.sourcegitcommit: d24c6777cc6922d5d0d9519e720962e1026b1096
ms.openlocfilehash: b1ed7d5693ff7e6730255462411d462694b730e1
ms.contentlocale: de-de
ms.lasthandoff: 09/14/2017

---
# <a name="create-an-application-gateway-by-using-path-based-routing"></a>Erstellen eines Anwendungsgateways mit pfadbasiertem Routing

> [!div class="op_single_selector"]
> * [Azure-Portal](application-gateway-create-url-route-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-url-route-arm-ps.md)
> * [Azure CLI 2.0](application-gateway-create-url-route-cli.md)

Mit pfadbasiertem Routing können Sie Routen basierend auf dem URL-Pfad einer HTTP-Anforderung zuordnen. Es wird überprüft, ob eine Route zu einem Back-End-Pool für die URL konfiguriert ist, die im Anwendungsgateway angezeigt wird, und der Netzwerkverkehr wird dann an den definierten Back-End-Pool gesendet. Ein gängiges Szenario für URL-basiertes Routing ist der Lastenausgleich von Anforderungen für verschiedene Inhaltstypen auf verschiedene Back-End-Serverpools.

Azure Application Gateway verfügt über zwei Regeltypen: Basisrouting und pfadbasiertes Routing. Beim Basisrouting wird ein Roundrobin-Dienst für die Back-End-Pools bereitgestellt. Pfadbasiertes Routing nutzt für die Auswahl des Back-End-Pools zusätzlich zur Roundrobin-Verteilung das Pfadmuster der Anforderungs-URL.

## <a name="scenario"></a>Szenario

Im folgenden Beispiel verarbeitet Application Gateway Datenverkehr für „contoso.com“ mit zwei Back-End-Serverpools: einem Videoserverpool und einem Imageserverpool.

Anforderungen für „http://contoso.com/image*“ werden an den Imageserverpool (**pool1**) geleitet, Anforderungen für „http://contoso.com/video*“ an den Videoserverpool (**pool2**). Wenn keines der Pfadmuster zutrifft, wird ein Standardserverpool (**pool1**) ausgewählt.

![URL-Route](./media/application-gateway-create-url-route-arm-ps/figure1.png)

## <a name="before-you-begin"></a>Voraussetzungen

1. Installieren Sie mit dem Webplattform-Installer die aktuelle Version der Azure PowerShell-Cmdlets. Sie können die neueste Version aus dem Abschnitt **Windows PowerShell** der [Downloadseite](https://azure.microsoft.com/downloads/)herunterladen und installieren.
2. Erstellen Sie ein virtuelles Netzwerk und ein Subnetz für ein Anwendungsgateway. Stellen Sie sicher, dass keine virtuellen Computer oder Cloudbereitstellungen das Subnetz verwenden. Das Application Gateway muss sich allein im Subnetz eines virtuellen Netzwerks befinden.
3. Vergewissern Sie sich, dass die Server, die Sie dem Back-End-Pool für das Anwendungsgateway hinzugefügt haben, vorhanden sind oder dass ihre Endpunkte im virtuellen Netzwerk erstellt wurden bzw. ihren Endpunkten eine öffentliche IP-Adresse/VIP zugewiesen wurde.

## <a name="requirements-to-create-an-application-gateway"></a>Anforderungen zum Erstellen eines Anwendungsgateways

* **Back-End-Serverpool:** Die Liste der IP-Adressen der Back-End-Server. Die aufgelisteten IP-Adressen müssen entweder dem Subnetz des virtuellen Netzwerks angehören oder eine öffentliche IP-Adresse/VIP sein.
* **Einstellungen für den Back-End-Serverpool:** Etwa Port, Protokoll und cookiebasierte Affinität. Diese Einstellungen sind an einen Pool gebunden und gelten für alle Server innerhalb des Pools.
* **Front-End-Port:** Der öffentliche Port, der im Anwendungsgateway geöffnet ist. Datenverkehr erreicht diesen Port und wird dann an einen der Back-End-Server umgeleitet.
* **Listener:** Der Listener verfügt über einen Front-End-Port, ein Protokoll (Http oder Https, jeweils mit Beachtung der Groß-/Kleinschreibung) und den Namen des SSL-Zertifikats (falls die SSL-Auslagerung konfiguriert wird).
* **Regel:** Mit der Regel werden der Listener und der Back-End-Serverpool gebunden, und es wird definiert, an welchen Pool der Datenverkehr gesendet werden soll, wenn er einen Listener erreicht.

## <a name="create-an-application-gateway"></a>Erstellen eines Anwendungsgateways

Der Unterschied zwischen dem klassischen Azure-Portal und Azure Resource Manager besteht in der Reihenfolge, in der Sie das Anwendungsgateway und die Elemente erstellen, die konfiguriert werden müssen.

Beim Resource Manager werden alle Elemente, die ein Anwendungsgateway bilden, einzeln konfiguriert und anschließend zusammengesetzt, um die Anwendungsgatewayressource zu erstellen.

Führen Sie zum Erstellen eines Anwendungsgateways die folgenden Schritte aus:

1. Erstellen einer Ressourcengruppe für den Ressourcen-Manager
2. Erstellen eines virtuelles Netzwerks, Subnetzes und einer öffentlichen IP-Adresse für das Application Gateway
3. Erstellen eines Konfigurationsobjekts für das Anwendungsgateway
4. Erstellen einer Application Gateway-Ressource

## <a name="create-a-resource-group-for-resource-manager"></a>Erstellen einer Ressourcengruppe für den Resource Manager

Stellen Sie sicher, dass Sie die neueste Version von Azure PowerShell verwenden. Weitere Informationen finden Sie unter [Verwenden von Windows PowerShell mit Resource Manager](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Schritt 1

Melden Sie sich bei Azure an.

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

Alternativ können Sie Tags für eine Ressourcengruppe für ein Anwendungsgateway erstellen:

```powershell
$resourceGroup = New-AzureRmResourceGroup -Name appgw-RG -Location "West US" -Tags @{Name = "testtag"; Value = "Application Gateway URL routing"} 
```

Azure Resource Manager erfordert, dass Ressourcengruppen einen Standardspeicherort angeben, der für alle Ressourcen in dieser Gruppe verwendet wird. Stellen Sie sicher, dass alle Befehle, mit denen ein Anwendungsgateway erstellt wird, die gleiche Ressourcengruppe verwenden.

Im obigen Beispiel haben wir eine Ressourcengruppe namens „appgw-RG“ erstellt und den Standort „USA, Westen“ verwendet.

> [!NOTE]
> Falls Sie einen benutzerdefinierten Test für Ihr Anwendungsgateway konfigurieren müssen, finden Sie die entsprechenden Informationen unter [Erstellen eines benutzerdefinierten Tests für Azure Application Gateway mithilfe von PowerShell für Azure Resource Manager](application-gateway-create-probe-ps.md). Weitere Informationen finden Sie unter [Systemüberwachung des Application Gateways – Übersicht](application-gateway-probe-overview.md).
> 
> 

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Erstellen eines virtuelles Netzwerkes und eines Subnetzes für das Application Gateway

Das folgende Beispiel zeigt, wie Sie mit dem Ressourcen-Manager ein virtuelles Netzwerk erstellen: In diesem Beispiel wird ein virtuelles Netzwerk für das Anwendungsgateway erstellt. Für Application Gateway ist ein eigenes Subnetz erforderlich. Daher ist das für das Anwendungsgateway erstellte Subnetz kleiner als der Adressraum des virtuellen Netzwerks. Dadurch können andere Ressourcen – einschließlich Webserver – im gleichen virtuellen Netzwerk konfiguriert werden.

### <a name="step-1"></a>Schritt 1

Weisen Sie den Adressbereich 10.0.0.0/24 der Subnetzvariablen zu, die zum Erstellen eines virtuelles Netzwerks verwendet wird.  Dadurch wird das im nächsten Beispiel verwendete Subnetzkonfigurationsobjekt für das Anwendungsgateway erstellt.

```powershell
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24
```

### <a name="step-2"></a>Schritt 2

Erstellen Sie ein virtuelles Netzwerk mit dem Namen **appgwvnet** in der Ressourcengruppe **appgw-rg** für die Region „USA, Westen“ mit dem Präfix 10.0.0.0/16 und dem Subnetz 10.0.0.0/24. Damit ist die Konfiguration des virtuellen Netzwerks mit einem einzelnen Subnetz für das Anwendungsgateway abgeschlossen.

```powershell
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet
```

### <a name="step-3"></a>Schritt 3

Weisen Sie die Subnetzvariable für die nächsten Schritte zu. Diese Variable wird in einem späteren Schritt an das Cmdlet `New-AzureRMApplicationGateway` übergeben.

```powershell
$subnet=$vnet.Subnets[0]
```

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Erstellen der öffentlichen IP-Adresse für die Front-End-Konfiguration

Erstellen Sie eine öffentliche IP-Ressource namens **publicIP01** in der Ressourcengruppe **appgw-rg** für die Region „USA, Westen“. Application Gateway kann eine öffentliche IP-Adresse und/oder eine interne IP-Adresse verwenden, um Anforderungen für den Lastenausgleich zu empfangen.  In diesem Beispiel wird nur eine öffentliche IP-Adresse verwendet. Im folgenden Beispiel wird für die Erstellung der öffentlichen IP-Adresse kein DNS-Name konfiguriert, da Application Gateway keine benutzerdefinierten DNS-Namen für öffentliche IP-Adressen unterstützt.  Falls für den öffentlichen Endpunkt ein benutzerdefinierter Name erforderlich ist, erstellen Sie einen CNAME-Eintrag, um auf den automatisch generierten DNS-Namen für die öffentliche IP-Adresse zu verweisen.

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -name publicIP01 -location "West US" -AllocationMethod Dynamic
```

Dem Anwendungsgateway wird beim Starten des Diensts eine IP-Adresse zugewiesen.

## <a name="create-the-application-gateway-configuration"></a>Erstellen der Anwendungsgatewaykonfiguration

Sie müssen alle Konfigurationselemente einrichten, bevor Sie das Anwendungsgateway erstellen. Die folgenden Schritten erstellen die Konfigurationselemente, die für eine Anwendungsgatewayressource benötigt werden.

### <a name="step-1"></a>Schritt 1

Erstellen Sie für das Anwendungsgateway eine IP-Konfiguration mit dem Namen **gatewayIP01**. Beim Starten von Application Gateway wird eine IP-Adresse aus dem konfigurierten Subnetz ausgewählt, und der Netzwerkdatenverkehr wird an die IP-Adressen im Back-End-IP-Pool weitergeleitet. Beachten Sie, dass jede Instanz eine eigene IP-Adresse benötigt.

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet
```

### <a name="step-2"></a>Schritt 2

Konfigurieren Sie die Back-End-IP-Adresspools **pool1** und **pool2** mit den IP-Adressen für **pool1** und **pool2**. Dabei handelt es sich um die IP-Adressen der Ressourcen, die die Webanwendung hosten, die durch das Anwendungsgateway geschützt werden soll. Die Integrität dieser Mitglieder des Back-End-Pools wurde jeweils durch Basistests oder benutzerdefinierte Tests überprüft. An sie wird Datenverkehr weitergeleitet, wenn Anforderungen beim Anwendungsgateway eingehen. Back-End-Pools können von mehreren Regeln innerhalb des Anwendungsgateways verwendet werden. Das bedeutet, dass ein Back-End-Pool für mehrere Webanwendungen verwendet werden kann, die sich auf dem gleichen Host befinden.

```powershell
$pool1 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221, 134.170.185.50

$pool2 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool02 -BackendIPAddresses 134.170.186.47, 134.170.189.222, 134.170.186.51
```

In diesem Beispiel leiten zwei Back-End-Pools Netzwerkdatenverkehr basierend auf dem URL-Pfad weiter. Ein Pool empfängt Datenverkehr aus dem URL-Pfad „/video“, der andere Pool empfängt Datenverkehr aus dem Pfad „/image“. Ersetzen Sie die obigen IP-Adressen durch die IP-Adressendpunkte Ihrer eigenen Anwendung. 

### <a name="step-3"></a>Schritt 3

Konfigurieren Sie die Anwendungsgatewayeinstellungen **poolsetting01** und **poolsetting02** für den Netzwerkdatenverkehr mit Lastenausgleich im Back-End-Pool. In diesem Beispiel konfigurieren Sie verschiedene Back-End-Pool-Einstellungen für die Back-End-Pools. Jeder Back-End-Pool kann eigene Einstellung aufweisen.  Regeln verwenden Back-End-HTTP-Einstellungen, um Datenverkehr an die richtigen Mitglieder des Back-End-Pools weiterzuleiten. Die Einstellungen bestimmen das Protokoll und den Port, das bzw. der beim Senden von Datenverkehr an die Mitglieder des Back-End-Pools verwendet wird. Cookiebasierte Sitzungen werden ebenfalls durch die Back-End-HTTP-Einstellungen gesteuert. Im aktivierten Zustand sendet die cookiebasierte Sitzungsaffinität Datenverkehr an das gleiche Back-End wie vorherige Anforderungen für das jeweilige Paket.

```powershell
$poolSetting01 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled -RequestTimeout 120

$poolSetting02 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting02" -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 240
```

### <a name="step-4"></a>Schritt 4

Konfigurieren Sie die Front-End-IP-Adresse mit öffentlichen IP-Endpunkten. Ein Listener verwendet das Front-End-IP-Konfigurationsobjekt, um die von außen erreichbare IP-Adresse mit dem Listener zu verknüpfen.

```powershell
$fipconfig01 = New-AzureRmApplicationGatewayFrontendIPConfig -Name "frontend1" -PublicIPAddress $publicip
```

### <a name="step-5"></a>Schritt 5

Konfigurieren Sie den Front-End-Port für ein Application Gateway. Der Listener verwendet das Front-End-Port-Konfigurationsobjekt, um den Port zu definieren, an dem das Anwendungsgateway auf Datenverkehr des Listeners lauscht.

```powershell
$fp01 = New-AzureRmApplicationGatewayFrontendPort -Name "fep01" -Port 80
```

### <a name="step-6"></a>Schritt 6

Konfigurieren Sie den Listener für die öffentliche IP-Adresse und den Port zum Empfangen von eingehendem Netzwerkdatenverkehr. Im folgenden Beispiel wird der Listener mit der zuvor konfigurierten Front-End-IP- und Front-End-Portkonfiguration sowie mit einem Protokoll (Http oder Https, jeweils mit Beachtung der Groß-/Kleinschreibung) konfiguriert. In diesem Beispiel lauscht der Listener an Port 80 auf HTTP-Datenverkehr für die zuvor erstellte öffentliche IP-Adresse.

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name "listener01" -Protocol Http -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01
```

### <a name="step-7"></a>Schritt 7

Konfigurieren Sie URL-Regelpfade für die Back-End-Pools. Dieser Schritt konfiguriert den relativen Pfad, der von Application Gateway verwendet wird, und definiert die Zuordnung zwischen dem URL-Pfad und dem Back-End-Pool, der zum Verarbeiten des eingehenden Datenverkehrs zugewiesen wurde.

> [!IMPORTANT]
> Jeder Pfad muss mit einem Schrägstrich (/) beginnen, und ein Sternchen ist nur am Ende zulässig. Gültige Beispiele sind „/xyz“, „/xyz*“ oder „/xyz/“*. Die Zeichenfolge, die in den Pfadabgleicher eingegeben wird, enthält keinen Text nach dem ersten Fragezeichen (?) oder der ersten Raute (#), und diese Zeichen sind nicht zulässig. 

Im folgenden Beispiel werden zwei Regeln erstellt: eine für den Pfad „/image/“, der Datenverkehr an das Back-End **pool1** leitet, und eine andere für den Pfad „/video/“, der Datenverkehr an das Back-End **pool2** leitet. Diese Regeln stellen sicher, dass der Datenverkehr für jeden Satz URLs an das Back-End weitergeleitet wird. Beispiel: „http://contoso.com/image/figure1.jpg“ wird an **pool1** weitergeleitet, „http://contoso.com/video/example.mp4“ an **pool2**.

```powershell
$imagePathRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "pathrule1" -Paths "/image/*" -BackendAddressPool $pool1 -BackendHttpSettings $poolSetting01

$videoPathRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "pathrule2" -Paths "/video/*" -BackendAddressPool $pool2 -BackendHttpSettings $poolSetting02
```

Die Konfiguration der Regelpfadzuordnung konfiguriert auch dann einen Standard-Back-End-Adresspool, wenn der Pfad keiner der vordefinierten Pfadregeln entspricht. Beispiel: „http://contoso.com/shoppingcart/test.html“ wird an **pool1** weitergeleitet, da dieser als Standardpool für nicht übereinstimmenden Datenverkehr definiert wurde.

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

## <a name="create-an-application-gateway"></a>Erstellen eines Anwendungsgateways

Erstellen Sie ein Anwendungsgateway mit allen Konfigurationsobjekten aus den vorherigen Schritten.

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-RG -Location "West US" -BackendAddressPools $pool1,$pool2 -BackendHttpSettingsCollection $poolSetting01, $poolSetting02 -FrontendIpConfigurations $fipconfig01 -GatewayIpConfigurations $gipconfig -FrontendPorts $fp01 -HttpListeners $listener -UrlPathMaps $urlPathMap -RequestRoutingRules $rule01 -Sku $sku
```

## <a name="get-an-application-gateway-dns-name"></a>Abrufen des DNS-Namens eines Anwendungsgateways

Nach dem Erstellen des Gateways konfigurieren Sie das Front-End für die Kommunikation. Bei Verwendung einer öffentlichen IP-Adresse wird für die Application Gateway-Instanz ein dynamisch zugewiesener DNS-Namen benötigt. Dieser Name ist nicht benutzerfreundlich. Um zu gewährleisten, dass Kunden auf das Anwendungsgateway zugreifen können, können Sie mit einem CNAME-Eintrag auf den öffentlichen Endpunkt des Anwendungsgateways verweisen. Weitere Informationen finden Sie unter [Konfigurieren eines benutzerdefinierten Domänennamens für einen Azure-Clouddienst](../cloud-services/cloud-services-custom-domain-name-portal.md).

Rufen Sie zum Konfigurieren des CNAME-Eintrags für die Front-End-IP mithilfe des PublicIPAddress-Elements, das an das Anwendungsgateway angefügt ist, Details zum Anwendungsgateway und dem zugeordneten IP/DNS-Namen ab. Verwenden Sie den DNS-Namen des Anwendungsgateways zum Erstellen eines CNAME-Eintrags. Die Verwendung von A-Einträgen wird nicht empfohlen, da sich die VIP beim Neustart von Application Gateway möglicherweise ändert.

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

Informationen zur SSL-Auslagerung (Secure Sockets Layer) finden Sie unter [Konfigurieren eines Anwendungsgateways für die SSL-Auslagerung mit Azure Resource Manager](application-gateway-ssl-arm.md).


