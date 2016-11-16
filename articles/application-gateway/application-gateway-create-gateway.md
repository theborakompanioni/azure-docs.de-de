---
title: "Erstellen, Starten oder Löschen eines Anwendungsgateways | Microsoft Docs"
description: "Diese Seite enthält Anweisungen zum Erstellen, Konfigurieren, Starten und Löschen eines Azure-Anwendungsgateways."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: carmonm
editor: tysonn
ms.assetid: 577054ca-8368-4fbf-8d53-a813f29dc3bc
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/10/2016
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 996bac38e6b67cfe7b72e11bf29831b12086bf1b


---
# <a name="create-start-or-delete-an-application-gateway"></a>Erstellen, Starten oder Löschen eines Anwendungsgateways
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

## <a name="before-you-begin"></a>Voraussetzungen
1. Installieren Sie mit dem Webplattform-Installer die aktuelle Version der Azure PowerShell-Cmdlets. Sie können die neueste Version aus dem Abschnitt **Windows PowerShell** der [Downloadseite](https://azure.microsoft.com/downloads/)herunterladen und installieren.
2. Wenn Sie über ein vorhandenes Gateway verfügen, können Sie entweder ein vorhandenes leeres Subnetz auswählen oder im vorhandenen virtuellen Netzwerk ein neues Netzwerk erstellen, das ausschließlich vom Anwendungsgateway verwendet wird. Sie können das Anwendungsgateway nicht in einem anderen virtuellen Netzwerk als dem Netzwerk mit den Ressourcen bereitstellen, die Sie hinter dem Anwendungsgateway bereitstellen möchten, außer bei der Verwendung von VNet-Peering. Weitere Informationen finden Sie unter [Vnet Peering](../virtual-network/virtual-network-peering-overview.md).
3. Stellen Sie sicher, dass Sie über ein funktionierendes virtuelles Netzwerk mit einem gültigen Subnetz verfügen. Stellen Sie sicher, dass keine virtuellen Maschinen oder Cloudbereitstellungen das Subnetz verwenden. Das Application Gateway muss sich allein im Subnetz eines virtuellen Netzwerks befinden.
4. Die Server, die Sie für die Verwendung des Anwendungsgateways konfigurieren, müssen vorhanden sein oder Endpunkte aufweisen, die im virtuellen Netzwerk erstellt wurden oder denen eine öffentliche IP-Adresse/VIP zugewiesen wurde.

## <a name="what-is-required-to-create-an-application-gateway"></a>Was ist zum Erstellen eines Application Gateways erforderlich?
Wenn Sie den Befehl **New-AzureApplicationGateway** zum Erstellen des Anwendungsgateways verwenden, wird zu diesem Zeitpunkt keine Konfiguration festgelegt, und die neu erstellten Ressourcen werden entweder per XML oder mithilfe eines Konfigurationsobjekts konfiguriert.

Die Werte sind:

* **Back-End-Serverpool:** Die Liste der IP-Adressen der Back-End-Server. Die aufgelisteten IP-Adressen sollten entweder dem Subnetz des virtuellen Netzwerks angehören oder eine öffentliche IP-Adresse/VIP sein.
* **Einstellungen für den Back-End-Serverpool:** Jeder Pool weist Einstellungen wie Port, Protokoll und cookiebasierte Affinität auf. Diese Einstellungen sind an einen Pool gebunden und gelten für alle Server innerhalb des Pools.
* **Front-End-Port:** Dieser Port ist der öffentliche Port, der im Application Gateway geöffnet ist. Datenverkehr erreicht diesen Port und wird dann an einen der Back-End-Server umgeleitet.
* **Listener:** Der Listener verfügt über einen Front-End-Port, ein Protokoll („Http“ oder „Https“; jeweils unter Beachtung der Groß-/Kleinschreibung) und den Namen des SSL-Zertifikats (falls die SSL-Auslagerung konfiguriert wird).
* **Regel:** Mit der Regel werden der Listener und der Back-End-Serverpool gebunden, und es wird definiert, an welchen Back-End-Serverpool der Datenverkehr gesendet werden soll, wenn er einen bestimmten Listener erreicht.

## <a name="create-an-application-gateway"></a>Erstellen eines Anwendungsgateways
So erstellen Sie ein Application Gateway

1. Erstellen Sie eine neue Application Gateway-Ressource.
2. Erstellen Sie eine XML-Konfigurationsdatei oder ein Konfigurationsobjekt.
3. Ordnen Sie die Konfiguration der neu erstellten Anwendungsgatewayressource zu.

> [!NOTE]
> Falls Sie einen benutzerdefinierten Test für Ihr Anwendungsgateway konfigurieren müssen, finden Sie die entsprechenden Informationen unter [Erstellen eines Anwendungsgateways mit benutzerdefinierten Tests mithilfe von PowerShell](application-gateway-create-probe-classic-ps.md). Weitere Informationen finden Sie unter [Benutzerdefinierte Tests und Systemüberwachung](application-gateway-probe-overview.md) .
> 
> 

![Beispielszenario][scenario]

### <a name="create-an-application-gateway-resource"></a>Erstellen einer Anwendungsgatewayressource
Verwenden Sie zum Erstellen des Gateways das **New-AzureApplicationGateway** -Cmdlet, und ersetzen Sie die Werte durch Ihre eigenen Werte. Die Abrechnung für das Gateway beginnt jetzt noch nicht. Die Abrechnung beginnt in einem späteren Schritt, wenn das Gateway erfolgreich gestartet wurde.

Das folgende Beispiel erstellt ein Anwendungsgateway mithilfe eines virtuellen Netzwerks mit dem Namen „testvnet1“ und eines Subnetzes mit dem Namen „subnet-1“.

```powershell
New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")
```

*Description*, *InstanceCount* und *GatewaySize* sind optionale Parameter.

Sie können das **Get-AzureApplicationGateway** -Cmdlet verwenden, um zu überprüfen, ob das Gateway erstellt wurde.

```powershell
Get-AzureApplicationGateway AppGwTest
```

```
Name          : AppGwTest
Description   :
VnetName      : testvnet1
Subnets       : {Subnet-1}
InstanceCount : 2
GatewaySize   : Medium
State         : Stopped
VirtualIPs    : {}
DnsName       :
```

> [!NOTE]
> Der Standardwert für *InstanceCount* ist 2, der Maximalwert ist 10. Der Standardwert für *GatewaySize* ist "Medium". Sie können zwischen "Small", "Medium" und "Large" auswählen.
> 
> 

*VirtualIPs* und *DnsName* werden leer angezeigt, da das Gateway noch nicht gestartet wurde. Diese Werte werden erstellt, sobald das Gateway ausgeführt wird.

## <a name="configure-the-application-gateway"></a>Konfigurieren des Anwendungsgateways
Sie können das Anwendungsgateway per XML oder mit einem Konfigurationsobjekt konfigurieren.

## <a name="configure-the-application-gateway-by-using-xml"></a>Konfigurieren des Anwendungsgateways per XML
Im folgenden Beispiel verwenden Sie eine XML-Datei, um alle Einstellungen des Anwendungsgateways zu konfigurieren und auf die Anwendungsgatewayressource zu übertragen.  

### <a name="step-1"></a>Schritt 1
Kopieren Sie den folgenden Text in Editor.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
    <FrontendPorts>
        <FrontendPort>
            <Name>(name-of-your-frontend-port)</Name>
            <Port>(port number)</Port>
        </FrontendPort>
    </FrontendPorts>
    <BackendAddressPools>
        <BackendAddressPool>
            <Name>(name-of-your-backend-pool)</Name>
            <IPAddresses>
                <IPAddress>(your-IP-address-for-backend-pool)</IPAddress>
                <IPAddress>(your-second-IP-address-for-backend-pool)</IPAddress>
            </IPAddresses>
        </BackendAddressPool>
    </BackendAddressPools>
    <BackendHttpSettingsList>
        <BackendHttpSettings>
            <Name>(backend-setting-name-to-configure-rule)</Name>
            <Port>80</Port>
            <Protocol>[Http|Https]</Protocol>
            <CookieBasedAffinity>Enabled</CookieBasedAffinity>
        </BackendHttpSettings>
    </BackendHttpSettingsList>
    <HttpListeners>
        <HttpListener>
            <Name>(name-of-the-listener)</Name>
            <FrontendPort>(name-of-your-frontend-port)</FrontendPort>
            <Protocol>[Http|Https]</Protocol>
        </HttpListener>
    </HttpListeners>
    <HttpLoadBalancingRules>
        <HttpLoadBalancingRule>
            <Name>(name-of-load-balancing-rule)</Name>
            <Type>basic</Type>
            <BackendHttpSettings>(backend-setting-name-to-configure-rule)</BackendHttpSettings>
            <Listener>(name-of-the-listener)</Listener>
            <BackendAddressPool>(name-of-your-backend-pool)</BackendAddressPool>
        </HttpLoadBalancingRule>
    </HttpLoadBalancingRules>
</ApplicationGatewayConfiguration>
```

Bearbeiten Sie die Werte zwischen den Klammern für die Konfigurationselemente. Speichern Sie die Datei mit der Erweiterung XML.

> [!IMPORTANT]
> Für die Protokollelemente Http oder Https muss die Groß-/Kleinschreibung beachtet werden.
> 
> 

Das folgende Beispiel zeigt, wie Sie das Anwendungsgateway mithilfe einer Konfigurationsdatei konfigurieren. In dem Beispiel findet ein Lastenausgleich für HTTP-Datenverkehr am öffentlichen Port 80 statt, und Netzwerkdatenverkehr wird zwischen zwei IP-Adressen an den Back-End-Port 80 gesendet.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
    <FrontendPorts>
        <FrontendPort>
            <Name>FrontendPort1</Name>
            <Port>80</Port>
        </FrontendPort>
    </FrontendPorts>
    <BackendAddressPools>
        <BackendAddressPool>
            <Name>BackendPool1</Name>
            <IPAddresses>
                <IPAddress>10.0.0.1</IPAddress>
                <IPAddress>10.0.0.2</IPAddress>
            </IPAddresses>
        </BackendAddressPool>
    </BackendAddressPools>
    <BackendHttpSettingsList>
        <BackendHttpSettings>
            <Name>BackendSetting1</Name>
            <Port>80</Port>
            <Protocol>Http</Protocol>
            <CookieBasedAffinity>Enabled</CookieBasedAffinity>
        </BackendHttpSettings>
    </BackendHttpSettingsList>
    <HttpListeners>
        <HttpListener>
            <Name>HTTPListener1</Name>
            <FrontendPort>FrontendPort1</FrontendPort>
            <Protocol>Http</Protocol>
        </HttpListener>
    </HttpListeners>
    <HttpLoadBalancingRules>
        <HttpLoadBalancingRule>
            <Name>HttpLBRule1</Name>
            <Type>basic</Type>
            <BackendHttpSettings>BackendSetting1</BackendHttpSettings>
            <Listener>HTTPListener1</Listener>
            <BackendAddressPool>BackendPool1</BackendAddressPool>
        </HttpLoadBalancingRule>
    </HttpLoadBalancingRules>
</ApplicationGatewayConfiguration>
```

### <a name="step-2"></a>Schritt 2
Legen Sie anschließend das Anwendungsgateway fest. Verwenden Sie das **Set-AzureApplicationGatewayConfig** -Cmdlet mit einer XML-Konfigurationsdatei.

```powershell
Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile "D:\config.xml"
```

## <a name="configure-the-application-gateway-by-using-a-configuration-object"></a>Konfigurieren des Anwendungsgateways mit einem Konfigurationsobjekt
Das folgende Beispiel zeigt, wie Sie das Anwendungsgateway mithilfe von Konfigurationsobjekten konfigurieren. Alle Konfigurationselemente müssen einzeln konfiguriert und anschließend einem Konfigurationsobjekt für das Anwendungsgateway hinzugefügt werden. Verwenden Sie nach dem Erstellen des Konfigurationsobjekts den Befehl **Set-AzureApplicationGateway** , um die Konfiguration der zuvor erstellten Anwendungsgatewayressource zuzuordnen.

> [!NOTE]
> Bevor Sie dem Konfigurationsobjekt einen Wert zuweisen, müssen Sie deklarieren, welcher Objekttyp in PowerShell zum Speichern verwendet wird. Die erste Zeile zum Erstellen der einzelnen Elemente definiert, welches Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model(Objektname) verwendet wird.
> 
> 

### <a name="step-1"></a>Schritt 1
Erstellen Sie die einzelnen Konfigurationselemente.

Erstellen Sie die Front-End-IP wie im folgenden Beispiel gezeigt.

```powershell
$fip = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendIPConfiguration
$fip.Name = "fip1"
$fip.Type = "Private"
$fip.StaticIPAddress = "10.0.0.5"
```

Erstellen Sie den Front-End-Port wie im folgenden Beispiel gezeigt.

```powershell
$fep = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendPort
$fep.Name = "fep1"
$fep.Port = 80
```

Erstellen Sie den Back-End-Serverpool.

Definieren Sie die IP-Adressen, die dem Back-End-Serverpool hinzugefügt werden, wie im folgenden Beispiel gezeigt.

```powershell
$servers = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendServerCollection
$servers.Add("10.0.0.1")
$servers.Add("10.0.0.2")
```

Verwenden Sie das $server-Objekt, um die Werte dem Back-End-Poolobjekt ($pool) hinzuzufügen.

```powershell
$pool = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendAddressPool
$pool.BackendServers = $servers
$pool.Name = "pool1"
```

Erstellen Sie die Einstellung für den Back-End-Serverpool.

```powershell
$setting = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendHttpSettings
$setting.Name = "setting1"
$setting.CookieBasedAffinity = "enabled"
$setting.Port = 80
$setting.Protocol = "http"
```

Erstellen Sie den Listener.

```powershell
$listener = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpListener
$listener.Name = "listener1"
$listener.FrontendPort = "fep1"
$listener.FrontendIP = "fip1"
$listener.Protocol = "http"
$listener.SslCert = ""
```

Erstellen Sie die Regel.

```powershell
$rule = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpLoadBalancingRule
$rule.Name = "rule1"
$rule.Type = "basic"
$rule.BackendHttpSettings = "setting1"
$rule.Listener = "listener1"
$rule.BackendAddressPool = "pool1"
```

### <a name="step-2"></a>Schritt 2
Weisen Sie die einzelnen Konfigurationselemente einem Konfigurationsobjekt für das Anwendungsgateway ($appgwconfig) zu.

Fügen Sie die Front-End-IP der Konfiguration hinzu.

```powershell
$appgwconfig = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.ApplicationGatewayConfiguration
$appgwconfig.FrontendIPConfigurations = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendIPConfiguration]"
$appgwconfig.FrontendIPConfigurations.Add($fip)
```

Fügen Sie den Front-End-Port der Konfiguration hinzu.

```powershell
$appgwconfig.FrontendPorts = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendPort]"
$appgwconfig.FrontendPorts.Add($fep)
```
Fügen Sie den Back-End-Serverpool der Konfiguration hinzu.

```powershell
$appgwconfig.BackendAddressPools = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendAddressPool]"
$appgwconfig.BackendAddressPools.Add($pool)
```

Fügen Sie die Einstellung des Back-End-Pools der Konfiguration hinzu.

```powershell
$appgwconfig.BackendHttpSettingsList = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendHttpSettings]"
$appgwconfig.BackendHttpSettingsList.Add($setting)
```

Fügen Sie den Listener der Konfiguration hinzu.

```powershell
$appgwconfig.HttpListeners = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpListener]"
$appgwconfig.HttpListeners.Add($listener)
```

Fügen Sie die Regel der Konfiguration hinzu.

```powershell
$appgwconfig.HttpLoadBalancingRules = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpLoadBalancingRule]"
$appgwconfig.HttpLoadBalancingRules.Add($rule)
```

### <a name="step-3"></a>Schritt 3
Ordnen Sie das Konfigurationsobjekt mithilfe von **Set-AzureApplicationGatewayConfig**der Anwendungsgatewayressource zu.

```powershell
Set-AzureApplicationGatewayConfig -Name AppGwTest -Config $appgwconfig
```

## <a name="start-the-gateway"></a>Starten des Gateways
Verwenden Sie nach dem Konfigurieren des Gateways das **Start-AzureApplicationGateway** -Cmdlet, um das Gateway zu starten. Die Abrechnung für ein Anwendungsgateway beginnt, nachdem das Gateway erfolgreich gestartet wurde.

> [!NOTE]
> Es kann 15 bis 20 Minuten dauern, bis die Ausführung des **Start-AzureApplicationGateway** -Cmdlets abgeschlossen ist.
> 
> 

```powershell
Start-AzureApplicationGateway AppGwTest
```

## <a name="verify-the-gateway-status"></a>Überprüfen des Gatewaystatus
Verwenden Sie das **Get-AzureApplicationGateway** -Cmdlet, um den Status des Gateways zu überprüfen. Wenn **Start-AzureApplicationGateway** im vorherigen Schritt erfolgreich ausgeführt wurde, sollte der *Zustand* nun „Wird ausgeführt“ lauten, und *Vip* und *DnsName* sollten gültige Einträge besitzen.

Dieses Beispiel zeigt ein Anwendungsgateway, das ausgeführt wird und Datenverkehr verarbeiten kann, der für `http://<generated-dns-name>.cloudapp.net`vorgesehen ist.

```powershell
Get-AzureApplicationGateway AppGwTest
```

```
VERBOSE: 8:09:28 PM - Begin Operation: Get-AzureApplicationGateway
VERBOSE: 8:09:30 PM - Completed Operation: Get-AzureApplicationGateway
Name          : AppGwTest
Description   :
VnetName      : testvnet1
Subnets       : {Subnet-1}
InstanceCount : 2
GatewaySize   : Medium
State         : Running
Vip           : 138.91.170.26
DnsName       : appgw-1b8402e8-3e0d-428d-b661-289c16c82101.cloudapp.net
```

## <a name="delete-an-application-gateway"></a>Löschen eines Anwendungsgateways
So löschen Sie ein Anwendungsgateway

1. Verwenden Sie das **Stop-AzureApplicationGateway** -Cmdlet, um das Gateway zu beenden.
2. Verwenden Sie das **Remove-AzureApplicationGateway** -Cmdlet, um das Gateway zu entfernen.
3. Stellen Sie sicher, dass das Gateway entfernt wurde, indem Sie das **Get-AzureApplicationGateway** -Cmdlet verwenden.

Das folgende Beispiel zeigt das **Stop-AzureApplicationGateway** -Cmdlet in der ersten Zeile, gefolgt von der Ausgabe.

```powershell
Stop-AzureApplicationGateway AppGwTest
```

```
VERBOSE: 9:49:34 PM - Begin Operation: Stop-AzureApplicationGateway
VERBOSE: 10:10:06 PM - Completed Operation: Stop-AzureApplicationGateway
Name       HTTP Status Code     Operation ID                             Error
----       ----------------     ------------                             ----
Successful OK                   ce6c6c95-77b4-2118-9d65-e29defadffb8
```

Verwenden Sie nach erfolgreicher Beendigung des Anwendungsgateways das **Remove-AzureApplicationGateway** -Cmdlet, um den Dienst zu entfernen.

```powershell
Remove-AzureApplicationGateway AppGwTest
```

```
VERBOSE: 10:49:34 PM - Begin Operation: Remove-AzureApplicationGateway
VERBOSE: 10:50:36 PM - Completed Operation: Remove-AzureApplicationGateway
Name       HTTP Status Code     Operation ID                             Error
----       ----------------     ------------                             ----
Successful OK                   055f3a96-8681-2094-a304-8d9a11ad8301
```

Verwenden Sie ggf. das **Get-AzureApplicationGateway**-Cmdlet, um sich zu vergewissern, dass der Dienst entfernt wurde. Dieser Schritt ist nicht erforderlich.

```powershell
Get-AzureApplicationGateway AppGwTest
```

```
VERBOSE: 10:52:46 PM - Begin Operation: Get-AzureApplicationGateway

Get-AzureApplicationGateway : ResourceNotFound: The gateway does not exist.
.....
```

## <a name="next-steps"></a>Nächste Schritte
Wenn Sie die SSL-Auslagerung konfigurieren möchten, finden Sie weitere Informationen im Abschnitt [Konfigurieren eines Application Gateways für die SSL-Auslagerung](application-gateway-ssl.md).

Wenn Sie ein Anwendungsgateway für die Verwendung mit einem internen Load Balancer konfigurieren möchten, ist es ratsam, den Abschnitt [Erstellen eines Application Gateways mit einem internen Lastenausgleich (ILB)](application-gateway-ilb.md)zu lesen.

Weitere Informationen zu Lastenausgleichsoptionen im Allgemeinen finden Sie unter:

* [Azure-Lastenausgleich](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)

[scenario]: ./media/application-gateway-create-gateway/scenario.png



<!--HONumber=Nov16_HO2-->


