---
title: "Konfigurieren der SSL-Auslagerung – Azure Application Gateway (PowerShell) | Microsoft-Dokumentation"
description: "Dieser Artikel enthält Anweisungen zum Erstellen eines Anwendungsgateways mit SSL-Auslagerung mit Azure Resource Manager."
documentationcenter: na
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.assetid: 3c3681e0-f928-4682-9d97-567f8e278e13
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/19/2017
ms.author: davidmu
ms.translationtype: HT
ms.sourcegitcommit: 1c730c65194e169121e3ad1d1423963ee3ced8da
ms.openlocfilehash: 4bcd002a805f1584323f5baf75f791f2fb64201f
ms.contentlocale: de-de
ms.lasthandoff: 08/30/2017

---
# <a name="configure-an-application-gateway-for-ssl-offload-by-using-azure-resource-manager"></a>Konfigurieren eines Anwendungsgateways für die SSL-Auslagerung mit Azure Resource Manager

> [!div class="op_single_selector"]
> * [Azure-Portal](application-gateway-ssl-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-ssl-arm.md)
> * [Klassische Azure PowerShell](application-gateway-ssl.md)
> * [Azure CLI 2.0](application-gateway-ssl-cli.md)

Azure Application Gateway kann so konfiguriert werden, dass damit die Secure Sockets Layer-Sitzung (SSL) auf dem Gateway beendet wird. Auf diese Weise wird die aufwändige SSL-Entschlüsselung in der Webfarm vermieden. Die SSL-Auslagerung vereinfacht zudem die Einrichtung und Verwaltung der Webanwendung auf dem Front-End-Server.

## <a name="before-you-begin"></a>Voraussetzungen

1. Installieren Sie mit dem Webplattform-Installer die aktuelle Version der Azure PowerShell-Cmdlets. Sie können die neueste Version aus dem Abschnitt **Windows PowerShell** der [Downloadseite](https://azure.microsoft.com/downloads/)herunterladen und installieren.
2. Erstellen Sie ein virtuelles Netzwerk und ein Subnetz für das Anwendungsgateway. Stellen Sie sicher, dass keine virtuellen Maschinen oder Cloudbereitstellungen das Subnetz verwenden. Das Application Gateway muss sich allein im Subnetz eines virtuellen Netzwerks befinden.
3. Die Server, die Sie für die Verwendung des Anwendungsgateways konfigurieren, müssen vorhanden sein oder Endpunkte aufweisen, die im virtuellen Netzwerk erstellt wurden oder denen eine öffentliche IP-Adresse oder eine virtuelle IP-Adresse (VIP) zugewiesen wurde.

## <a name="what-is-required-to-create-an-application-gateway"></a>Was ist zum Erstellen eines Anwendungsgateways erforderlich?

* **Back-End-Serverpool:** Die Liste der IP-Adressen der Back-End-Server. Die aufgelisteten IP-Adressen sollten dem Subnetz des virtuellen Netzwerks angehören oder eine öffentliche IP-Adresse/VIP sein.
* **Einstellungen für den Back-End-Serverpool:** Jeder Pool weist Einstellungen wie Port, Protokoll und cookiebasierte Affinität auf. Diese Einstellungen sind an einen Pool gebunden und gelten für alle Server innerhalb des Pools.
* **Front-End-Port:** Dieser Port ist der öffentliche Port, der im Anwendungsgateway geöffnet ist. Datenverkehr erreicht diesen Port und wird dann an einen der Back-End-Server umgeleitet.
* **Listener:** Der Listener verfügt über einen Front-End-Port, ein Protokoll (Http oder Https, jeweils mit Beachtung der Groß-/Kleinschreibung) und den Namen des SSL-Zertifikats (falls SSL-Auslagerung konfiguriert wird).
* **Regel:** Mit der Regel werden der Listener und der Back-End-Serverpool gebunden, und es wird definiert, an welchen Back-End-Serverpool der Datenverkehr gesendet werden soll, wenn er einen bestimmten Listener erreicht. Derzeit wird nur die Regel *basic* unterstützt. Die Regel *basic* ist eine Round-Robin-Lastverteilung.

**Zusätzliche Konfigurationshinweise**

Für die Konfiguration von SSL-Zertifikaten sollte das Protokoll in **HttpListener** in **Https** (Groß-/Kleinschreibung beachten) geändert werden. Fügen Sie das Element **SslCertificate** zu **HttpListener** mit dem Variablenwert hinzu, der für das SSL-Zertifikat konfiguriert wurde. Der Front-End-Port sollte auf **443** aktualisiert werden.

**So aktivieren Sie cookiebasierte Affinität**: Sie können ein Anwendungsgateway so konfigurieren, dass sicherstellt ist, dass eine Anforderung von einer Clientsitzung immer an denselben virtuellen Computer in der Webfarm weitergeleitet wird. Fügen Sie dazu ein Sitzungscookie ein, damit das Gateway den Datenverkehr entsprechend weiterleiten kann. Legen Sie zum Aktivieren der cookiebasierten Affinität **CookieBasedAffinity** im **BackendHttpSettings**-Element auf **Enabled** fest.

## <a name="create-an-application-gateway"></a>Erstellen eines Anwendungsgateways

Der Unterschied zwischen dem klassischen Azure-Bereitstellungsmodell und Azure Resource Manager besteht in der Reihenfolge, in der Sie ein Anwendungsgateway und die Elemente erstellen, die konfiguriert werden müssen.

Bei Resource Manager werden alle Komponenten eines Anwendungsgateways einzeln konfiguriert und dann zusammengefügt, um eine Anwendungsgatewayressource zu erstellen.

Hier sind die erforderlichen Schritte zum Erstellen eines Anwendungsgateways angegeben:

1. [Erstellen einer Ressourcengruppe für Resource Manager](#create-a-resource-group-for-resource-manager)
2. [Erstellen eines virtuellen Netzwerks, Subnetzes und einer öffentlichen IP-Adresse für das Anwendungsgateway](#create-virtual-network-subnet-and-public-IP-for-the-application-gateway)
3. [Erstellen eines Konfigurationsobjekts für das Anwendungsgateway](#create-an-application-gateway-configuration-object)
4. [Erstellen einer Anwendungsgatewayressource](#create-an-application-gateway-resource)

## <a name="create-a-resource-group-for-resource-manager"></a>Erstellen einer Ressourcengruppe für den Resource Manager

Stellen Sie sicher, dass Sie in den PowerShell-Modus wechseln, um die Azure-Ressourcen-Manager-Cmdlets zu verwenden. Weitere Informationen finden Sie unter [Verwenden von Windows PowerShell mit Resource Manager](../powershell-azure-resource-manager.md).

   1. Geben Sie den folgenden Befehl ein:

   ```powershell
   Login-AzureRmAccount
   ```

   2. Um die Abonnements für das Konto zu überprüfen, geben Sie die folgenden Befehle ein:

   ```powershell
   Get-AzureRmSubscription
   ```

   Sie werden zur Authentifizierung mit Ihren Anmeldeinformationen aufgefordert.

   3. Um auszuwählen, welches Azure-Abonnement Sie verwenden möchten, geben Sie den folgenden Befehl ein:

   ```powershell
   Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
   ```

   4. Um eine Ressourcengruppe zu erstellen, geben Sie den folgenden Befehl ein. (Überspringen Sie diesen Schritt, wenn Sie eine vorhandene Ressourcengruppe verwenden.)

   ```powershell
   New-AzureRmResourceGroup -Name appgw-rg -Location "West US"
   ```

Der Azure Resource Manager erfordert, dass alle Ressourcengruppen einen Speicherort angeben. Diese Einstellung wird als Standardspeicherort für Ressourcen in dieser Ressourcengruppe verwendet. Stellen Sie sicher, dass alle Befehle, mit denen ein Anwendungsgateway erstellt wird, die gleiche Ressourcengruppe verwenden.

Im obigen Beispiel haben wir eine Ressourcengruppe namens **appgw-RG** mit dem Standort **USA, Westen** erstellt.

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Erstellen eines virtuelles Netzwerkes und eines Subnetzes für das Application Gateway

Das folgende Beispiel zeigt, wie Sie mit dem Ressourcen-Manager ein virtuelles Netzwerk erstellen:

   1. Geben Sie den folgenden Befehl ein:

   ```powershell
   $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24
   ```

   In diesem Beispiel wird der Adressbereich **10.0.0.0/24** einer Subnetzvariablen zugewiesen, die zum Erstellen eines virtuellen Netzwerks verwendet wird.

   2. Geben Sie den folgenden Befehl ein:

   ```powershell
   $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet
   ```

   In diesem Beispiel wird ein virtuelles Netzwerk mit dem Namen **appgwvnet** in der Ressourcengruppe **appgw-rg** für die Region **USA, Westen** mit dem Präfix **10.0.0.0/16** und dem Subnetz **10.0.0.0/24** erstellt.

   3. Geben Sie den folgenden Befehl ein:

   ```powershell
   $subnet = $vnet.Subnets[0]
   ```

   In diesem Beispiel wird der Variablen **$subnet** das Subnetzobjekt für die nächsten Schritte zugewiesen.

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Erstellen der öffentlichen IP-Adresse für die Front-End-Konfiguration

Um eine öffentliche IP-Adresse für die Front-End-Konfiguration zu erstellen, geben Sie den folgenden Befehl ein:

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name publicIP01 -location "West US" -AllocationMethod Dynamic
```

In diesem Beispiel wird die öffentliche IP-Ressource **publicIP01** in der Ressourcengruppe **appgw-rg** für die Region **USA, Westen** erstellt.

## <a name="create-an-application-gateway-configuration-object"></a>Erstellen eines Konfigurationsobjekts für das Anwendungsgateway

   1. Um das Konfigurationsobjekt für ein Anwendungsgateway zu erstellen, geben Sie den folgenden Befehl ein:

   ```powershell
   $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet
   ```

   In diesem Beispiel wird eine IP-Konfiguration für das Anwendungsgateway mit dem Namen **gatewayIP01** erstellt. Beim Starten von Application Gateway wird eine IP-Adresse aus dem konfigurierten Subnetz ausgewählt, und der Netzwerkdatenverkehr wird an die IP-Adressen im Back-End-IP-Pool weitergeleitet. Beachten Sie, dass jede Instanz eine eigene IP-Adresse benötigt.

   2. Geben Sie den folgenden Befehl ein:

   ```powershell
   $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50
   ```

   In diesem Beispiel wird der Back-End-IP-Adresspool **pool01** mit den IP-Adressen **134.170.185.46**, **134.170.188.221** und **134.170.185.50** erstellt. Diese Werte sind die IP-Adressen, die den Netzwerkdatenverkehr vom Front-End-IP-Endpunkt empfangen. Ersetzen Sie die IP-Adressen aus dem obigen Beispiel durch die IP-Adressen der Endpunkte Ihrer Webanwendung.

   3. Geben Sie den folgenden Befehl ein:

   ```powershell
   $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Enabled
   ```

   In diesem Beispiel wird die Anwendungsgatewayeinstellung **poolsetting01** für den Lastenausgleich des Netzwerkdatenverkehrs im Back-End-Pool konfiguriert.

   4. Geben Sie den folgenden Befehl ein:

   ```powershell
   $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 443
   ```

   In diesem Beispiel wird der Front-End-IP-Port mit dem Namen **frontendport01** für den öffentlichen IP-Adressendpunkt konfiguriert.

   5. Geben Sie den folgenden Befehl ein:

   ```powershell
   $cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path for certificate file> -Password "<password>"
   ```

   In diesem Beispiel wird das Zertifikat für SSL-Verbindungen konfiguriert. Das Zertifikat muss im PFX-Format vorliegen, und das Kennwort muss zwischen 4 und 12 Zeichen umfassen.

   6. Geben Sie den folgenden Befehl ein:

   ```powershell
   $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip
   ```

   In diesem Beispiel wird die Front-End-IP-Adresskonfiguration mit dem Namen **fipconfig01** erstellt, und die öffentliche IP-Adresse wird der Front-End-IP-Adresskonfiguration zugeordnet.

   7. Geben Sie den folgenden Befehl ein:

   ```powershell
   $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert
   ```

   Dieses Beispiel dient zum Erstellen des Listeners **listener01** und zum Zuordnen des Front-End-Ports zur Front-End-IP-Adresskonfiguration und zum Zertifikat.

   8. Geben Sie den folgenden Befehl ein:

   ```powershell
   $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
   ```

   In diesem Beispiel wird die Routingregel für den Lastenausgleich mit dem Namen **rule01** erstellt, mit der das Lastenausgleichsverhalten konfiguriert wird.

   9. Geben Sie den folgenden Befehl ein:

   ```powershell
   $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2
   ```

   In diesem Beispiel wird die Instanzgröße des Anwendungsgateways konfiguriert.

   > [!NOTE]
   > Der Standardwert für **InstanceCount** ist **2**, der Maximalwert ist 10. Der Standardwert für **GatewaySize** ist **Medium**. Sie können zwischen „Standard_Small“, „Standard_Medium“ und „Standard_Large“ wählen.

   10. Geben Sie den folgenden Befehl ein:

   ```powershell
   $policy = New-AzureRmApplicationGatewaySslPolicy -PolicyType Predefined -PolicyName AppGwSslPolicy20170401S
   ```

   In diesem Schritt wird die SSL-Richtlinie festgelegt, die für das Anwendungsgateway verwendet werden soll. Weitere Informationen finden Sie unter [Konfigurieren von SSL-Richtlinienversionen und Verschlüsselungssammlungen für Application Gateway](application-gateway-configure-ssl-policy-powershell.md).

## <a name="create-an-application-gateway-by-using-new-azureapplicationgateway"></a>Erstellen eines Anwendungsgateways mit dem New-AzureApplicationGateway-Cmdlet

Geben Sie den folgenden Befehl ein:

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SslCertificates $cert -SslPolicy $policy
```

In diesem Beispiel wird ein Anwendungsgateway mit allen Konfigurationselementen aus den vorherigen Schritten erstellt. Im Beispiel heißt das Anwendungsgateway **appgwtest**.

## <a name="get-the-application-gateway-dns-name"></a>Abrufen des DNS-Namens für das Anwendungsgateway

Nach dem Erstellen des Gateways wird das Front-End für die Kommunikation konfiguriert. Application Gateway erfordert einen dynamisch zugewiesenen DNS-Namen, wenn eine öffentliche IP-Adresse verwendet wird, die nicht benutzerfreundlich ist. Um zu gewährleisten, dass Endbenutzer auf das Anwendungsgateway zugreifen können, können Sie mit einem CNAME-Eintrag auf den öffentlichen Endpunkt des Anwendungsgateways verweisen. Weitere Informationen finden Sie unter [Konfigurieren eines benutzerdefinierten Domänennamens in Azure](../cloud-services/cloud-services-custom-domain-name-portal.md). 

Um den DNS-Namen des Anwendungsgateways zu ermitteln, rufen Sie mithilfe des **PublicIPAddress**-Elements, das an das Anwendungsgateway angefügt ist, die Details zum Anwendungsgateway sowie die zugeordnete IP-Adresse/den zugeordneten DNS-Namen ab. Verwenden Sie den DNS-Namen des Anwendungsgateways zum Erstellen eines CNAME-Eintrags, der die beiden Webanwendungen an diesen DNS-Namen verweist. Die Verwendung von A-Einträgen wird nicht empfohlen, da sich die VIP beim Neustart des Anwendungsgateways möglicherweise ändert.


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

Wenn Sie ein Anwendungsgateway für die Verwendung mit einem internen Lastenausgleich konfigurieren möchten, lesen Sie den Abschnitt [Erstellen eines Anwendungsgateways mit einem internen Lastenausgleich](application-gateway-ilb.md).

Weitere Informationen zu Lastenausgleichsoptionen im Allgemeinen finden Sie unter:

* [Azure-Lastenausgleich](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)

