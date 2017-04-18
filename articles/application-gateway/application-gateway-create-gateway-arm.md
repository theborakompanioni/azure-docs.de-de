---
title: "Erstellen und Verwalten einer Azure Application Gateway-Instanz – PowerShell | Microsoft-Dokumentation"
description: "Diese Seite enthält Anweisungen zum Erstellen, Konfigurieren, Starten und Löschen eines Azure-Anwendungsgateways mit Azure Resource Manager."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 866e9b5f-0222-4b6a-a95f-77bc3d31d17b
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/04/2017
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 73ee330c276263a21931a7b9a16cc33f86c58a26
ms.openlocfilehash: 11ecfc993f17c89d4ac4431e9a835000d30afe76
ms.lasthandoff: 04/05/2017


---
# <a name="create-start-or-delete-an-application-gateway-by-using-azure-resource-manager"></a>Erstellen, Starten oder Löschen eines Anwendungsgateways mit Azure Resource Manager

> [!div class="op_single_selector"]
> * [Azure-Portal](application-gateway-create-gateway-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-gateway-arm.md)
> * [Klassische Azure PowerShell](application-gateway-create-gateway.md)
> * [Azure Resource Manager-Vorlage](application-gateway-create-gateway-arm-template.md)
> * [Azure-Befehlszeilenschnittstelle](application-gateway-create-gateway-cli.md)

Azure Application Gateway verwendet einen Load Balancer auf der Schicht 7 (Anwendungsschicht). Das Application Gateway ermöglicht ein Failover sowie schnelles Routing von HTTP-Anforderungen zwischen verschiedenen Servern in der Cloud und der lokalen Umgebung.
Application Gateway bietet zahlreiche Application Delivery Controller-Funktionen (ADC), u.a. HTTP-Lastenausgleich, cookiebasierte Sitzungsaffinität, SSL-Auslagerung (Secure Sockets Layer), benutzerdefinierte Integritätstests und Unterstützung für mehrere Websites.

Eine vollständige Liste der unterstützten Features finden Sie unter [Übersicht über Application Gateway](application-gateway-introduction.md).

In diesem Artikel werden Sie durch die Schritte zum Erstellen, Konfigurieren, Starten und Löschen eines Anwendungsgateways geführt.

> [!IMPORTANT]
> Vor der Verwendung von Azure-Ressourcen sollten Sie wissen, dass Azure derzeit über zwei Bereitstellungsmodelle verfügt: Resource Manager-Bereitstellung und klassische Bereitstellung. Machen Sie sich vor der Verwendung von Azure-Ressourcen unbedingt mit den [Bereitstellungsmodellen und -tools](../azure-classic-rm.md) vertraut. Zum Anzeigen der Dokumentation für verschiedene Tools klicken Sie auf die Registerkarten oben in diesem Artikel. In diesem Dokument erfahren Sie alles über das Erstellen eines Anwendungsgateways mit Azure Resource Manager. Um die klassische Version zu verwenden, wechseln Sie zu [Erstellen, Starten oder Löschen eines Anwendungsgateways](application-gateway-create-gateway.md).

## <a name="before-you-begin"></a>Voraussetzungen

1. Installieren Sie mit dem Webplattform-Installer die aktuelle Version der Azure PowerShell-Cmdlets. Sie können die neueste Version aus dem Abschnitt **Windows PowerShell** der [Downloadseite](https://azure.microsoft.com/downloads/)herunterladen und installieren.
1. Wenn Sie über ein vorhandenes Gateway verfügen, können Sie entweder ein vorhandenes leeres Subnetz auswählen oder im vorhandenen virtuellen Netzwerk ein Subnetz erstellen, das ausschließlich vom Anwendungsgateway verwendet wird. Sie können das Anwendungsgateway nicht in einem anderen virtuellen Netzwerk als dem Netzwerk mit den Ressourcen bereitstellen, die Sie hinter dem Anwendungsgateway bereitstellen möchten.
1. Die Server, die Sie für die Verwendung des Anwendungsgateways konfigurieren, müssen vorhanden sein oder Endpunkte aufweisen, die im virtuellen Netzwerk erstellt wurden oder denen eine öffentliche IP-Adresse/VIP zugewiesen wurde.

## <a name="what-is-required-to-create-an-application-gateway"></a>Was ist zum Erstellen eines Application Gateways erforderlich?

* **Back-End-Serverpool:** Die Liste mit IP-Adressen, FQDNs oder NICs des Back-End-Servers. Bei Verwendung von IP-Adressen müssen diese entweder dem Subnetz des virtuellen Netzwerks angehören oder eine öffentliche IP-Adresse/VIP sein.
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

Im obigen Beispiel haben wir eine Ressourcengruppe namens **appgw-RG** mit dem Standort **USA, Westen** erstellt.

> [!NOTE]
> Wenn Sie einen benutzerdefinierten Test für Ihr Anwendungsgateway konfigurieren müssen, sehen Sie sich den folgenden Artikel an: [Erstellen eines benutzerdefinierten Tests für Azure Application Gateway mithilfe von PowerShell für Azure Resource Manager](application-gateway-create-probe-ps.md). Weitere Informationen finden Sie unter [Benutzerdefinierte Tests und Systemüberwachung](application-gateway-probe-overview.md) .

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Erstellen eines virtuelles Netzwerkes und eines Subnetzes für das Application Gateway

Das folgende Beispiel zeigt, wie Sie mit dem Ressourcen-Manager ein virtuelles Netzwerk erstellen: In diesem Beispiel wird ein VNET für das Anwendungsgateway erstellt. Da das Anwendungsgateway ein eigenes Subnetz benötigt, ist das für das Anwendungsgateway erstellte Subnetz kleiner als der VNET-Adressraum. Die Verwendung eines kleineren Subnetzes ermöglicht es, andere Ressourcen (einschließlich Webserver) im gleichen VNET zu konfigurieren.

### <a name="step-1"></a>Schritt 1

Weisen Sie den Adressbereich 10.0.0.0/24 der Subnetzvariablen zu, die zum Erstellen eines virtuelles Netzwerks verwendet wird. Dieser Schritt erstellt das im nächsten Beispiel verwendete Subnetz-Konfigurationsobjekt für das Anwendungsgateway.

```powershell
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24
```

### <a name="step-2"></a>Schritt 2

Erstellen Sie ein virtuelles Netzwerk mit dem Namen **appgwvnet** in der Ressourcengruppe **appgw-rg** für die Region „USA, Westen“ mit dem Präfix „10.0.0.0/16“ und dem Subnetz „10.0.0.0/24“. Mit diesem Schritt wird die Konfiguration des VNETs mit einem einzelnen Subnetz für das Anwendungsgateway abgeschlossen.

```powershell
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet
```

### <a name="step-3"></a>Schritt 3

Weisen Sie die Subnetzvariable für die nächsten Schritte zu. Diese Variable wird in einem späteren Schritt an das `New-AzureRMApplicationGateway`-Cmdlet übergeben.

```powershell
$subnet=$vnet.Subnets[0]
```

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Erstellen der öffentlichen IP-Adresse für die Front-End-Konfiguration

Erstellen Sie eine öffentliche IP-Ressource namens **publicIP01** in der Ressourcengruppe **appgw-rg** für die Region „USA, Westen“. Das Anwendungsgateway kann eine öffentliche IP-Adresse und/oder eine interne IP-Adresse verwenden, um Anforderungen für den Lastenausgleich zu empfangen.  In diesem Beispiel wird nur eine öffentliche IP-Adresse verwendet. Im folgenden Beispiel wird für die Erstellung der öffentlichen IP-Adresse kein DNS-Name konfiguriert.  Das Anwendungsgateway unterstützt keine benutzerdefinierten DNS-Namen für öffentliche IP-Adressen.  Falls für einen öffentlichen Endpunkt ein benutzerdefinierter Name erforderlich ist, muss ein CNAME-Eintrag erstellt werden, um auf den automatisch generierten DNS-Namen für die öffentliche IP-Adresse zu verweisen.

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name publicIP01 -location "West US" -AllocationMethod Dynamic
```

> [!NOTE]
> Dem Anwendungsgateway wird beim Starten des Diensts eine IP-Adresse zugewiesen.

## <a name="create-the-application-gateway-configuration-objects"></a>Erstellen der Konfigurationsobjekte für das Anwendungsgateway

Sie müssen alle Konfigurationselemente einrichten, bevor Sie das Anwendungsgateway erstellen. Die folgenden Schritten erstellen die Konfigurationselemente, die für eine Application Gateway-Ressource benötigt werden.

### <a name="step-1"></a>Schritt 1

Erstellen Sie für das Anwendungsgateway eine IP-Konfiguration mit dem Namen **gatewayIP01**. Beim Starten des Anwendungsgateways wird eine IP-Adresse aus dem konfigurierten Subnetz ausgewählt, und der Netzwerkdatenverkehr wird an die IP-Adressen im Back-End-IP-Pool weitergeleitet. Beachten Sie, dass jede Instanz eine eigene IP-Adresse benötigt.

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet
```

### <a name="step-2"></a>Schritt 2

Konfigurieren Sie den Back-End-IP-Adresspool namens **pool01** mit den IP-Adressen für **pool1**. Bei diesen IP-Adressen handelt es sich um die IP-Adressen der Ressourcen, die die Webanwendung hosten, die durch das Anwendungsgateway geschützt werden soll. Die Integrität dieser Mitglieder des Back-End-Pools wurde jeweils durch Basistests oder benutzerdefinierte Tests überprüft.  An sie wird Datenverkehr weitergeleitet, wenn Anforderungen beim Anwendungsgateway eingehen. Back-End-Pools können von mehreren Regeln innerhalb des Anwendungsgateways verwendet werden. Ein Back-End-Pool kann also für mehrere Webanwendungen verwendet werden, die sich auf dem gleichen Host befinden.

```powershell
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221, 134.170.185.50
```

Dieses Beispiel enthält zwei Back-End-Pools zum Weiterleiten von Netzwerkverkehr basierend auf dem URL-Pfad. Ein Pool empfängt Datenverkehr aus dem URL-Pfad „/video“, der andere Pool empfängt Datenverkehr aus dem Pfad „/image“. Ersetzen Sie die obigen IP-Adressen durch die IP-Adressendpunkte Ihrer eigenen Anwendung.

### <a name="step-3"></a>Schritt 3

Konfigurieren Sie die Anwendungsgatewayeinstellung **poolsetting01** für den Lastenausgleich des Netzwerkdatenverkehrs im Back-End-Pool. Jeder Back-End-Pool kann eine eigene Back-End-Pool-Einstellung aufweisen.  Back-End-HTTP-Einstellungen werden von Regeln verwendet, um Datenverkehr an die richtigen Back-End-Poolmitglieder weiterzuleiten. Back-End-HTTP-Einstellungen bestimmen das Protokoll und den Port, die beim Senden von Datenverkehr an die Back-End-Poolmitglieder verwendet werden. Cookiebasierte Sitzungen werden ebenfalls durch die Back-End-HTTP-Einstellungen gesteuert.  Im aktivierten Zustand sendet die cookiebasierte Sitzungsaffinität Datenverkehr an das gleiche Back-End wie vorherige Anforderungen für das jeweilige Paket.

```powershell
$poolSetting01 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled -RequestTimeout 120
```

### <a name="step-4"></a>Schritt 4

Konfigurieren Sie den Front-End-Port für ein Application Gateway. Das Front-End-Port-Konfigurationsobjekt wird von einen Listener verwendet, um den Port zu definieren, an dem das Anwendungsgateway auf Datenverkehr des Listeners lauscht.

```powershell
$fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80
```

### <a name="step-5"></a>Schritt 5

Konfigurieren Sie die Front-End-IP-Adresse mit dem öffentlichen IP-Endpunkt. Das Front-End-IP-Konfigurationsobjekt wird von einem Listener verwendet, um die von außen erreichbare IP-Adresse mit dem Listener zu verknüpfen.

```powershell
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip
```

### <a name="step-6"></a>Schritt 6

Konfigurieren Sie den Listener. Dieser Schritt konfiguriert den Listener für die öffentliche IP-Adresse und den Port zum Empfangen von eingehendem Netzwerkverkehr. Im folgenden Beispiel wird der Listener mit der zuvor konfigurierten Front-End-IP- und Front-End-Portkonfiguration sowie mit einem Protokoll (HTTP oder HTTPS) konfiguriert. In diesem Beispiel lauscht der Listener an Port 80 auf HTTP-Datenverkehr für die zuvor erstellte öffentliche IP-Adresse.

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp
```

### <a name="step-7"></a>Schritt 7

Erstellen Sie die Lastenausgleichs-Routingregel **rule01**, die das Verhalten des Lastenausgleichs konfiguriert. Die Regel setzt sich aus den Back-End-Pooleinstellungen, dem Listener und dem Back-End-Pool zusammen, die in den vorherigen Schritten erstellt wurden. Datenverkehr wird auf der Grundlage der definierten Kriterien an das entsprechende Back-End weitergeleitet.

```powershell
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting01 -HttpListener $listener -BackendAddressPool $pool
```

### <a name="step-8"></a>Schritt 8

Konfigurieren Sie die Anzahl der Instanzen und die Größe für das Application Gateway.

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2
```

> [!NOTE]
> Der Standardwert für **InstanceCount** ist 2, der Maximalwert ist 10. Der Standardwert für **GatewaySize** ist "Medium". Sie können zwischen **Standard_Small**, **Standard_Medium** und **Standard_Large** wählen.

## <a name="create-an-application-gateway-by-using-new-azurermapplicationgateway"></a>Erstellen eines Application Gateways mit dem Cmdlet „New-AzureRmApplicationGateway“

Erstellen Sie ein Anwendungsgateway mit allen Konfigurationselementen aus den vorherigen Schritten. In diesem Beispiel heißt das Anwendungsgateway **appgwtest**.

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku
```

Rufen Sie die DNS- und VIP-Details des Anwendungsgateways aus der öffentlichen IP-Ressource ab, die an das Anwendungsgateway angefügt ist.

```powershell
Get-AzureRmPublicIpAddress -Name publicIP01 -ResourceGroupName appgw-rg  
```

## <a name="delete-an-application-gateway"></a>Löschen eines Anwendungsgateways

So löschen Sie ein Anwendungsgateway:

### <a name="step-1"></a>Schritt 1

Rufen Sie das Anwendungsgatewayobjekt ab, und ordnen Sie es einer Variablen `$getgw` zu.

```powershell
$getgw = Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg
```

### <a name="step-2"></a>Schritt 2

Verwenden Sie `Stop-AzureRmApplicationGateway`, um das Anwendungsgateway zu beenden.

```powershell
Stop-AzureRmApplicationGateway -ApplicationGateway $getgw
```

Sobald das Anwendungsgateway beendet wurde, verwenden Sie das Cmdlet `Remove-AzureRmApplicationGateway`, um den Dienst zu entfernen.

```powershell
Remove-AzureRmApplicationGateway -Name $appgwtest -ResourceGroupName appgw-rg -Force
```

> [!NOTE]
> Mit dem optionalen Switch **-force** kann diese Bestätigungsmeldung unterdrückt werden.

Mithilfe des Cmdlets `Get-AzureRmApplicationGateway` können Sie sicherstellen, dass der Dienst entfernt wurde. Dieser Schritt ist nicht erforderlich.

```powershell
Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg
```

## <a name="get-application-gateway-dns-name"></a>Abrufen des DNS-Namens des Anwendungsgateways

Nach dem Erstellen des Gateways wird das Front-End für die Kommunikation konfiguriert. Wenn Sie eine öffentliche IP-Adresse verwenden, wird das Anwendungsgateway ein dynamisch zugewiesener DNS-Namen benötigt, der kein Anzeigename ist. Um sicherzustellen, dass die Endbenutzer auf das Anwendungsgateway zugreifen können, kann mit einem CNAME-Eintrag auf den öffentlichen Endpunkt des Anwendungsgateways verwiesen werden. [Konfigurieren eines benutzerdefinierten Domänennamens in Azure](../cloud-services/cloud-services-custom-domain-name-portal.md) Rufen Sie zum Ermitteln des dynamisch erstellten DNS-Namens mithilfe des PublicIPAddress-Elements, das an das Anwendungsgateway angefügt ist, Details zum Anwendungsgateway und dem zugeordneten IP/DNS-Namen ab. Verwenden Sie den DNS-Namen des Anwendungsgateways zum Erstellen eines CNAME-Eintrags, der die beiden Webanwendungen an diesen DNS-Namen verweist. Die Verwendung von A-Einträgen wird nicht empfohlen, da sich die VIP beim Neustart des Anwendungsgateways möglicherweise verändert.

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

Informationen zum Konfigurieren der SSL-Auslagerung finden Sie unter [Konfigurieren eines Anwendungsgateways für SSL-Auslagerung mit klassischem Bereitstellungsmodell](application-gateway-ssl.md).

Informationen zum Konfigurieren eines Anwendungsgateways für die Verwendung mit einem internen Lastenausgleich finden Sie unter [Erstellen eines Anwendungsgateways mit einem internen Lastenausgleich (ILB)](application-gateway-ilb.md).

Weitere grundsätzliche Informationen zu Lastenausgleichsoptionen finden Sie unter:

* [Azure-Lastenausgleich](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)


