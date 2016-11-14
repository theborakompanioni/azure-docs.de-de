---
title: "Erstellen, Starten oder Löschen eines Anwendungsgateways mit Azure Resource Manager | Microsoft Docs"
description: "Diese Seite enthält Anweisungen zum Erstellen, Konfigurieren, Starten und Löschen eines Azure-Anwendungsgateways mit Azure Resource Manager."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: carmonm
editor: tysonn
ms.assetid: 866e9b5f-0222-4b6a-a95f-77bc3d31d17b
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/10/2016
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: d4da5dd65b39011ed0abb8bbbd47542d66e73ad6


---
# <a name="create-start-or-delete-an-application-gateway-by-using-azure-resource-manager"></a>Erstellen, Starten oder Löschen eines Anwendungsgateways mit Azure Resource Manager
> [!div class="op_single_selector"]
> * [Azure-Portal](application-gateway-create-gateway-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-gateway-arm.md)
> * [Klassische Azure PowerShell](application-gateway-create-gateway.md)
> * [Azure Resource Manager-Vorlage](application-gateway-create-gateway-arm-template.md)
> * [Azure-Befehlszeilenschnittstelle](application-gateway-create-gateway-cli.md)
> 
> 

Azure Application Gateway verwendet einen Load Balancer auf der Schicht 7 (Anwendungsschicht). Das Application Gateway ermöglicht ein Failover sowie schnelles Routing von HTTP-Anforderungen zwischen verschiedenen Servern in der Cloud und der lokalen Umgebung. Application Gateway bietet zahlreiche Application Delivery Controller-Funktionen (ADC), u.a. HTTP-Lastenausgleich, cookiebasierte Sitzungsaffinität, SSL-Auslagerung (Secure Sockets Layer), benutzerdefinierte Integritätstests und Unterstützung für mehrere Websites. Eine vollständige Liste der unterstützten Features finden Sie unter [Übersicht über Application Gateway](application-gateway-introduction.md).

In diesem Artikel werden Sie durch die Schritte zum Erstellen, Konfigurieren, Starten und Löschen eines Anwendungsgateways geführt.

> [!IMPORTANT]
> Bevor Sie mit Azure-Ressourcen arbeiten, sollten Sie wissen, dass Azure derzeit über zwei Bereitstellungsmodelle verfügt: die Bereitstellung mit dem Resource Manager und die klassische Bereitstellung. Machen Sie sich vor der Verwendung von Azure-Ressourcen unbedingt mit den [Bereitstellungsmodellen und -tools](../azure-classic-rm.md) vertraut. Zum Anzeigen der Dokumentation für verschiedene Tools klicken Sie auf die Registerkarten oben in diesem Artikel. In diesem Dokument erfahren Sie alles über das Erstellen eines Anwendungsgateways mit Azure Resource Manager. Um die klassische Version zu verwenden, wechseln Sie zu [Erstellen, Starten oder Löschen eines Anwendungsgateways](application-gateway-create-gateway.md).
> 
> 

## <a name="before-you-begin"></a>Voraussetzungen
1. Installieren Sie mit dem Webplattform-Installer die aktuelle Version der Azure PowerShell-Cmdlets. Sie können die neueste Version aus dem Abschnitt **Windows PowerShell** der [Downloadseite](https://azure.microsoft.com/downloads/)herunterladen und installieren.
2. Wenn Sie über ein vorhandenes Gateway verfügen, können Sie entweder ein vorhandenes leeres Subnetz auswählen oder im vorhandenen virtuellen Netzwerk ein Subnetz erstellen, das ausschließlich vom Anwendungsgateway verwendet wird. Sie können das Anwendungsgateway nicht in einem anderen virtuellen Netzwerk als dem Netzwerk mit den Ressourcen bereitstellen, die Sie hinter dem Anwendungsgateway bereitstellen möchten.
3. Die Server, die Sie für die Verwendung des Anwendungsgateways konfigurieren, müssen vorhanden sein oder Endpunkte aufweisen, die im virtuellen Netzwerk erstellt wurden oder denen eine öffentliche IP-Adresse/VIP zugewiesen wurde.

## <a name="what-is-required-to-create-an-application-gateway"></a>Was ist zum Erstellen eines Anwendungsgateways erforderlich?
* **Back-End-Serverpool:** Die Liste der IP-Adressen der Back-End-Server. Die aufgelisteten IP-Adressen sollten entweder dem Subnetz des virtuellen Netzwerks angehören oder eine öffentliche IP-Adresse/VIP sein.
* **Einstellungen für den Back-End-Serverpool:** Jeder Pool weist Einstellungen wie Port, Protokoll und cookiebasierte Affinität auf. Diese Einstellungen sind an einen Pool gebunden und gelten für alle Server innerhalb des Pools.
* **Front-End-Port:** Dieser Port ist der öffentliche Port, der im Application Gateway geöffnet ist. Datenverkehr erreicht diesen Port und wird dann an einen der Back-End-Server umgeleitet.
* **Listener:** Der Listener verfügt über einen Front-End-Port, ein Protokoll („Http“ oder „Https“; jeweils unter Beachtung der Groß-/Kleinschreibung) und den Namen des SSL-Zertifikats (falls die SSL-Auslagerung konfiguriert wird).
* **Regel:** Mit der Regel werden der Listener und der Back-End-Serverpool verbunden, und es wird definiert, an welchen Back-End-Serverpool der Datenverkehr geleitet werden soll, wenn er einen bestimmten Listener erreicht.

## <a name="create-an-application-gateway"></a>Erstellen eines Anwendungsgateways
Der Unterschied zwischen dem klassischen Azure-Modell und Azure Resource Manager besteht in der Reihenfolge, in der Sie ein Anwendungsgateway und die Elemente erstellen, die konfiguriert werden müssen.

Beim Resource Manager werden alle Elemente, die ein Anwendungsgateway bilden, einzeln konfiguriert und anschließend zusammengesetzt, um die Anwendungsgatewayressource zu erstellen.

Im Anschluss sind die Schritte zum Erstellen eines Anwendungsgateways aufgeführt:

## <a name="create-a-resource-group-for-resource-manager"></a>Erstellen einer Ressourcengruppe für den Ressourcen-Manager
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
Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
```

### <a name="step-4"></a>Schritt 4
Erstellen Sie eine Ressourcengruppe. (Überspringen Sie diesen Schritt, wenn Sie eine vorhandene Ressourcengruppe verwenden.)

```powershell
New-AzureRmResourceGroup -Name appgw-rg -Location "West US"
```

Der Azure Resource Manager erfordert, dass alle Ressourcengruppen einen Speicherort angeben. Dieser wird als Standardspeicherort für Ressourcen in dieser Ressourcengruppe verwendet. Stellen Sie sicher, dass alle Befehle, mit denen ein Anwendungsgateway erstellt wird, die gleiche Ressourcengruppe verwenden.

Im obigen Beispiel haben wir eine Ressourcengruppe namens „appgw-RG“ mit dem Standort „USA, Westen“ erstellt.

> [!NOTE]
> Falls Sie einen benutzerdefinierten Test für Ihr Anwendungsgateway konfigurieren müssen, finden Sie die entsprechenden Informationen unter [Erstellen eines Anwendungsgateways mit benutzerdefinierten Tests mithilfe von PowerShell](application-gateway-create-probe-ps.md). Weitere Informationen finden Sie unter [Benutzerdefinierte Tests und Systemüberwachung](application-gateway-probe-overview.md) .
> 
> 

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Erstellen eines virtuelles Netzwerkes und eines Subnetzes für das Application Gateway
Das folgende Beispiel zeigt, wie Sie mit dem Ressourcen-Manager ein virtuelles Netzwerk erstellen:

### <a name="step-1"></a>Schritt 1
Weisen Sie den Adressbereich 10.0.0.0/24 der Subnetzvariablen zu, die zum Erstellen eines virtuelles Netzwerks verwendet wird.

```powershell
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24
```

### <a name="step-2"></a>Schritt 2
Erstellen Sie ein virtuelles Netzwerk mit dem Namen „appgwvnet“ in der Ressourcengruppe „appgw-rg“ für die Region „USA, Westen“ mit dem Präfix „10.0.0.0/16“ und dem Subnetz „10.0.0.0/24“.

```powershell
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet
```

### <a name="step-3"></a>Schritt 3
Weisen Sie eine Subnetzvariable für die nächsten Schritte zu. Damit wird ein Application Gateway erstellt.

```powershell
$subnet=$vnet.Subnets[0]
```

## <a name="create-a-public-ip-address-for-the-frontend-configuration"></a>Erstellen der öffentlichen IP-Adresse für die Front-End-Konfiguration
Erstellen Sie eine öffentliche IP-Ressource namens „publicIP01“ in der Ressourcengruppe „appgw-rg“ für die Region „USA, Westen“.

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name publicIP01 -location "West US" -AllocationMethod Dynamic
```

## <a name="create-an-application-gateway-configuration-object"></a>Erstellen eines Konfigurationsobjekts für das Anwendungsgateway
Sie müssen alle Konfigurationselemente einrichten, bevor Sie das Anwendungsgateway erstellen. Die folgenden Schritten erstellen die Konfigurationselemente, die für eine Application Gateway-Ressource benötigt werden.

### <a name="step-1"></a>Schritt 1
Erstellen Sie eine IP-Konfiguration für das Application Gateway mit dem Namen „gatewayIP01“. Beim Starten des Anwendungsgateways wird eine IP-Adresse aus dem konfigurierten Subnetz ausgewählt, und der Netzwerkdatenverkehr wird an die IP-Adressen im Back-End-IP-Pool weitergeleitet. Beachten Sie, dass jede Instanz eine eigene IP-Adresse benötigt.

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet
```

### <a name="step-2"></a>Schritt 2
Konfigurieren Sie den Back-End-IP-Adresspool „pool01“ mit den IP-Adressen „134.170.185.46“, „134.170.188.221“ und „134.170.185.50“. Dies sind die IP-Adressen, die den Netzwerkdatenverkehr vom Front-End-IP-Endpunkt empfangen. Ersetzen Sie die obigen IP-Adressen durch die IP-Adressendpunkte Ihrer eigenen Anwendung.

```powershell
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50
```

### <a name="step-3"></a>Schritt 3
Konfigurieren Sie die Anwendungsgatewayeinstellung „poolsetting01“ für den Lastenausgleich des Netzwerkdatenverkehrs im Back-End-Pool.

```powershell
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled
```

### <a name="step-4"></a>Schritt 4
Konfigurieren Sie den Front-End-IP-Port mit dem Namen „frontendport01“ für den öffentlichen IP-Adressendpunkt.

```powershell
$fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80
```

### <a name="step-5"></a>Schritt 5
Erstellen Sie die Front-End-IP-Adresskonfiguration namens „fipconfig01“ und ordnen Sie die öffentliche IP-Adresse der Front-End-IP-Konfiguration zu.

```powershell
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip
```

### <a name="step-6"></a>Schritt 6
Erstellen Sie den Listener „listener01“ und ordnen Sie den Front-End-Port der Front-End-IP-Konfiguration zu.

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp
```

### <a name="step-7"></a>Schritt 7
Erstellen Sie die Load Balancer-Routingregel „rule01“, welche das Load Balancer-Verhalten konfiguriert.

```powershell
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
```

### <a name="step-8"></a>Schritt 8
Konfigurieren Sie die Instanzgröße des Anwendungsgateways.

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2
```

> [!NOTE]
> Der Standardwert für *InstanceCount* ist 2, der Maximalwert ist 10. Der Standardwert für *GatewaySize* ist "Medium". Sie können zwischen „Standard_Small“, „Standard_Medium“ und „Standard_Large“ wählen.
> 
> 

## <a name="create-an-application-gateway-by-using-newazurermapplicationgateway"></a>Erstellen eines Application Gateways mit dem Cmdlet „New-AzureRmApplicationGateway“
Erstellen Sie ein Anwendungsgateway mit allen Konfigurationselementen aus den vorherigen Schritten. In diesem Beispiel heißt das Anwendungsgateway „appgwtest“.

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku
```

### <a name="step-9"></a>Schritt 9
Rufen Sie die DNS- und VIP-Details des Anwendungsgateways aus der öffentlichen IP-Ressource ab, die an das Anwendungsgateway angefügt ist.

```powershell
Get-AzureRmPublicIpAddress -Name publicIP01 -ResourceGroupName appgw-rg  
```

## <a name="delete-an-application-gateway"></a>Löschen eines Anwendungsgateways
So löschen Sie ein Anwendungsgateway:

### <a name="step-1"></a>Schritt 1
Rufen Sie das Anwendungsgatewayobjekt ab, und ordnen Sie es einer „$getgw“-Variablen zu.

```powershell
$getgw = Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg
```

### <a name="step-2"></a>Schritt 2
Verwenden Sie das **Stop-AzureRmApplicationGateway** -Cmdlet, um das Application Gateway zu beenden.

```powershell
Stop-AzureRmApplicationGateway -ApplicationGateway $getgw  
```

Verwenden Sie das **Remove-AzureRmApplicationGateway** -Cmdlet, um den Dienst zu entfernen, sobald das Application Gateway beendet wurde.

```powershell
Remove-AzureRmApplicationGateway -Name $appgwtest -ResourceGroupName appgw-rg -Force
```

> [!NOTE]
> Mit dem optionalen Switch **-force** kann diese Bestätigungsmeldung unterdrückt werden.
> 
> 

Um sicherzustellen, dass der Dienst entfernt wurde, können Sie das **Get-AzureRmApplicationGateway** -Cmdlet verwenden. Dieser Schritt ist nicht erforderlich.

```powershell
Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg
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
Wenn Sie die SSL-Auslagerung konfigurieren möchten, finden Sie weitere Informationen im Abschnitt [Konfigurieren eines Application Gateways für die SSL-Auslagerung](application-gateway-ssl.md).

Wenn Sie ein Anwendungsgateway für die Verwendung mit einem internen Load Balancer konfigurieren möchten, ist es ratsam, den Abschnitt [Erstellen eines Application Gateways mit einem internen Lastenausgleich (ILB)](application-gateway-ilb.md)zu lesen.

Weitere Informationen zu Lastenausgleichsoptionen im Allgemeinen finden Sie unter:

* [Azure-Lastenausgleich](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)




<!--HONumber=Nov16_HO2-->


