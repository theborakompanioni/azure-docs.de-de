---
title: "Erstellen eines benutzerdefinierten Tests – Azure Application Gateway (PowerShell – klassisch) | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie mithilfe von PowerShell einen benutzerdefinierten Test für ein Application Gateway im klassischen Bereitstellungsmodell erstellen."
services: application-gateway
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 338a7be1-835c-48e9-a072-95662dc30f5e
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: fd5960a4488f2ecd93ba117a7d775e78272cbffd
ms.openlocfilehash: 4787a382b837b71a28c45211a26aa512e8fb177e


---
# <a name="create-a-custom-probe-for-azure-application-gateway-classic-by-using-powershell"></a>Erstellen eines benutzerdefinierten Tests für ein Azure Application Gateway (klassisch) mithilfe von PowerShell

> [!div class="op_single_selector"]
> * [Azure-Portal](application-gateway-create-probe-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-probe-ps.md)
> * [Klassische Azure PowerShell](application-gateway-create-probe-classic-ps.md)


[!INCLUDE [azure-probe-intro-include](../../includes/application-gateway-create-probe-intro-include.md)]

> [!IMPORTANT]
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Resource Manager- und klassische Bereitstellung](../azure-resource-manager/resource-manager-deployment-model.md). Dieser Artikel befasst sich mit der Verwendung des klassischen Bereitstellungsmodells. Microsoft empfiehlt für die meisten neuen Bereitstellungen die Verwendung des Ressourcen-Manager-Modells. Erfahren Sie, wie Sie [diese Schritte mit dem Resource Manager-Modell ausführen](application-gateway-create-probe-ps.md).

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-an-application-gateway"></a>Erstellen eines Anwendungsgateways

So erstellen Sie ein Application Gateway

1. Erstellen Sie eine neue Application Gateway-Ressource.
2. Erstellen Sie eine XML-Konfigurationsdatei oder ein Konfigurationsobjekt.
3. Ordnen Sie die Konfiguration der neu erstellten Application Gateway-Ressource zu.

### <a name="create-an-application-gateway-resource"></a>Erstellen einer Anwendungsgatewayressource

Erstellen Sie das Gateway mithilfe des Cmdlets `New-AzureApplicationGateway`. Ersetzen Sie dabei die Werte durch eigene Werte. Die Abrechnung für das Gateway beginnt jetzt noch nicht. Die Abrechnung beginnt in einem späteren Schritt, wenn das Gateway erfolgreich gestartet wurde.

Das folgende Beispiel erstellt ein Anwendungsgateway mithilfe eines virtuellen Netzwerks mit dem Namen „testvnet1“ und eines Subnetzes mit dem Namen „subnet-1“.

```powershell
New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")
```

Mithilfe des Cmdlets `Get-AzureApplicationGateway` können Sie überprüfen, ob das Gateway erstellt wurde.

```powershell
Get-AzureApplicationGateway AppGwTest
```

> [!NOTE]
> Der Standardwert für *InstanceCount* ist 2, der Maximalwert ist 10. Der Standardwert für *GatewaySize* ist "Medium". Sie können zwischen „Small“, „Medium“ und „Large“ wählen.
> 
> 

*VirtualIPs* und *DnsName* werden leer angezeigt, da das Gateway noch nicht gestartet wurde. Diese Werte werden erstellt, sobald das Gateway ausgeführt wird.

## <a name="configure-an-application-gateway"></a>Konfigurieren des Application Gateways

Sie können das Application Gateway per XML oder mit einem Konfigurationsobjekt konfigurieren.

## <a name="configure-an-application-gateway-by-using-xml"></a>Konfigurieren des Application Gateways per XML

Im folgenden Beispiel verwenden Sie eine XML-Datei, um alle Einstellungen des Anwendungsgateways zu konfigurieren und auf die Anwendungsgatewayressource zu übertragen.  

### <a name="step-1"></a>Schritt 1

Kopieren Sie den folgenden Text in Editor.

```xml
<ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
<FrontendIPConfigurations>
    <FrontendIPConfiguration>
        <Name>fip1</Name>
        <Type>Private</Type>
    </FrontendIPConfiguration>
</FrontendIPConfigurations>
<FrontendPorts>
    <FrontendPort>
        <Name>port1</Name>
        <Port>80</Port>
    </FrontendPort>
</FrontendPorts>
<Probes>
    <Probe>
        <Name>Probe01</Name>
        <Protocol>Http</Protocol>
        <Host>contoso.com</Host>
        <Path>/path/custompath.htm</Path>
        <Interval>15</Interval>
        <Timeout>15</Timeout>
        <UnhealthyThreshold>5</UnhealthyThreshold>
    </Probe>
    </Probes>
    <BackendAddressPools>
    <BackendAddressPool>
        <Name>pool1</Name>
        <IPAddresses>
            <IPAddress>1.1.1.1</IPAddress>
            <IPAddress>2.2.2.2</IPAddress>
        </IPAddresses>
    </BackendAddressPool>
</BackendAddressPools>
<BackendHttpSettingsList>
    <BackendHttpSettings>
        <Name>setting1</Name>
        <Port>80</Port>
        <Protocol>Http</Protocol>
        <CookieBasedAffinity>Enabled</CookieBasedAffinity>
        <RequestTimeout>120</RequestTimeout>
        <Probe>Probe01</Probe>
    </BackendHttpSettings>
</BackendHttpSettingsList>
<HttpListeners>
    <HttpListener>
        <Name>listener1</Name>
        <FrontendIP>fip1</FrontendIP>
    <FrontendPort>port1</FrontendPort>
        <Protocol>Http</Protocol>
    </HttpListener>
</HttpListeners>
<HttpLoadBalancingRules>
    <HttpLoadBalancingRule>
        <Name>lbrule1</Name>
        <Type>basic</Type>
        <BackendHttpSettings>setting1</BackendHttpSettings>
        <Listener>listener1</Listener>
        <BackendAddressPool>pool1</BackendAddressPool>
    </HttpLoadBalancingRule>
</HttpLoadBalancingRules>
</ApplicationGatewayConfiguration>
```

Bearbeiten Sie die Werte zwischen den Klammern für die Konfigurationselemente. Speichern Sie die Datei mit der Erweiterung XML.

Das folgende Beispiel zeigt, wie Sie mithilfe einer Konfigurationsdatei das Application Gateway für den Lastenausgleich von HTTP-Datenverkehr am öffentlichen Port 80 und zum Senden des Netzwerkdatenverkehrs an den Back-End-Port 80 zwischen zwei IP-Adressen einrichten, indem Sie einen benutzerdefinierten Test verwenden.

> [!IMPORTANT]
> Für die Protokollelemente Http oder Https muss die Groß-/Kleinschreibung beachtet werden.

Es wird ein neues Konfigurationselement vom Typ \<Test\> hinzugefügt, um benutzerdefinierte Tests zu konfigurieren.

Die Konfigurationsparameter sind:

* **Name** : Referenzname für den benutzerdefinierten Test.
* **Protocol** : Das verwendete Protokoll (mögliche Werte: „HTTP“ und „HTTPS“).
* **Host** und **Pfad**: Vollständiger URL-Pfad, der vom Anwendungsgateway aufgerufen wird, um die Integrität der Instanz zu ermitteln. Beispiel: Für die Website „http://contoso.com/“ können Sie den benutzerdefinierten Test für „http://contoso.com/path/custompath.htm“ konfigurieren, damit die HTTP-Antwort bei den Überprüfungen des Tests erfolgreich ist.
* **Interval** : Konfiguriert die Intervalle der Testausführungen (in Sekunden).
* **Timeout** : Definiert das Timeout des Tests für eine HTTP-Antwortprüfung.
* **UnhealthyThreshold** : Die Anzahl von HTTP-Antworten mit Fehlern, ab der die Back-End-Instanz als *fehlerhaft*gekennzeichnet wird.

Auf den Namen der Überprüfung wird in der \<BackendHttpSettings\>-Konfiguration verwiesen, um festzulegen, welcher Back-End-Pool die Einstellungen für die benutzerdefinierte Überprüfung verwenden soll.

## <a name="add-a-custom-probe-configuration-to-an-existing-application-gateway"></a>Hinzufügen der Konfiguration eines benutzerdefinierten Tests zu einem vorhandenen Application Gateway

Zum Ändern der aktuellen Konfiguration eines Application Gateways sind drei Schritte nötig: das Abrufen der aktuellen XML-Konfigurationsdatei, das Einfügen des benutzerdefinierten Tests in die XML-Datei und das Konfigurieren des Application Gateways mit den neuen XML-Einstellungen.

### <a name="step-1"></a>Schritt 1

Rufen Sie die XML-Datei mit `Get-AzureApplicationGatewayConfig` ab. Dieses Cmdlet exportiert die XML-Konfigurationsdatei, damit ihr die Einstellungen für die Überprüfung hinzugefügt werden können.

```powershell
Get-AzureApplicationGatewayConfig -Name "<application gateway name>" -Exporttofile "<path to file>"
```

### <a name="step-2"></a>Schritt 2

Öffnen Sie die XML-Datei in einem Texteditor. Fügen Sie nach `<frontendport>` den Abschnitt `<probe>` hinzu.

```xml
<Probes>
    <Probe>
        <Name>Probe01</Name>
        <Protocol>Http</Protocol>
        <Host>contoso.com</Host>
        <Path>/path/custompath.htm</Path>
        <Interval>15</Interval>
        <Timeout>15</Timeout>
        <UnhealthyThreshold>5</UnhealthyThreshold>
    </Probe>
</Probes>
```

Fügen Sie den Namen des Tests im Abschnitt „backendHttpSettings“ der XML-Datei wie im folgenden Beispiel hinzu:

```xml
    <BackendHttpSettings>
        <Name>setting1</Name>
        <Port>80</Port>
        <Protocol>Http</Protocol>
        <CookieBasedAffinity>Enabled</CookieBasedAffinity>
        <RequestTimeout>120</RequestTimeout>
        <Probe>Probe01</Probe>
    </BackendHttpSettings>
```

Speichern Sie die XML-Datei.

### <a name="step-3"></a>Schritt 3

Aktualisieren Sie die Konfiguration des Anwendungsgateways mit der neuen XML-Datei, und verwenden Sie dazu `Set-AzureApplicationGatewayConfig`. Dieses Cmdlet aktualisiert das Anwendungsgateway mit der neuen Konfiguration.

```powershell
Set-AzureApplicationGatewayConfig -Name "<application gateway name>" -Configfile "<path to file>"
```

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie die SSL-Auslagerung (Secure Sockets Layer) konfigurieren möchten, lesen Sie den Abschnitt [Konfigurieren eines Anwendungsgateways für die SSL-Auslagerung](application-gateway-ssl.md).

Wenn Sie ein Anwendungsgateway für die Verwendung mit einem internen Lastenausgleich konfigurieren möchten, lesen Sie den Abschnitt [Erstellen eines Anwendungsgateways mit einem internen Lastenausgleich (ILB)](application-gateway-ilb.md).




<!--HONumber=Jan17_HO4-->


