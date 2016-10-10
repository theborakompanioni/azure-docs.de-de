<properties
    pageTitle="Konfigurieren der SSL-Richtlinie und von End-to-End-SSL mit Application Gateway | Microsoft Azure"
    description="Dieser Artikel beschreibt das Konfigurieren von End-to-End-SSL mit Application Gateway über Azure Resource Manager-PowerShell"
    services="application-gateway"
    documentationCenter="na"
    authors="georgewallace"
    manager="carmonmills"
    editor="tysonn"/>

<tags
    ms.service="application-gateway"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/26/2016"
    ms.author="gwallace"/>

# Konfigurieren der SSL-Richtlinie und von End-to-End-SSL mit Application Gateway über PowerShell

## Übersicht

Application Gateway unterstützt die End-to-End-Verschlüsselung des Datenverkehrs. Hierfür wird in Application Gateway die SSL-Verbindung am Anwendungsgateway beendet. Das Gateway wendet dann die Routingregeln auf den Datenverkehr an, verschlüsselt das Paket erneut und leitet das Paket basierend auf den definierten Routingregeln an das entsprechende Back-End weiter. Antworten vom Webserver durchlaufen denselben Prozess zurück an den Endbenutzer.

Eine weitere Funktion, die von Application Gateway unterstützt wird, ist das Deaktivieren bestimmter Versionen des SSL-Protokolls. Application Gateway unterstützt das Deaktivieren der folgenden Protokollversionen: TLSv1.0, TLSv1.1 und TLSv1.2.

![Szenarioabbildung][scenario]

## Szenario

In diesem Szenario erfahren Sie, wie Sie mithilfe von PowerShell ein Anwendungsgateway über End-to-End-SSL erstellen.

Dieses Szenario umfasst Folgendes:

- Erstellen einer Ressourcengruppe namens „appgw-rg“
- Erstellen eines virtuellen Netzwerks namens „appgwvnet“ mit einem reservierten CIDR-Block 10.0.0.0/16.
- Erstellen zweier Subnetze namens „appgwsubnet“ und „appsubnet“.
- Erstellen eines kleinen Anwendungsgateways mit Unterstützung der End-to-End-SSL-Verschlüsselung, das bestimmte SSL-Protokolle deaktiviert.

## Voraussetzungen

Zum Konfigurieren von End-to-End-SSL mit einem Anwendungsgateway wird ein Zertifikat für das Gateway benötigt, und weitere Zertifikate sind für die Back-End-Server erforderlich. Das Gatewayzertifikat wird zum Verschlüsseln und Entschlüsseln des Datenverkehrs verwendet, der über SSL gesendet wird. Das Gatewayzertifikat muss im PFX-Format (Personal Information Exchange, privater Informationsaustausch) vorliegen. In diesem Dateiformat kann der private Schlüssel exportiert werden, was erforderlich ist, damit das Anwendungsgateway die Ver- und Entschlüsselung des Datenverkehrs durchführen kann.

Für die End-to-End-SSL-Verschlüsselung muss das Back-End beim Anwendungsgateway auf der Whitelist stehen. Dies geschieht durch Hochladen des öffentlichen Zertifikats der Back-Ends auf das Anwendungsgateway. Dadurch wird sichergestellt, dass das Anwendungsgateway nur mit bekannten Back-End-Instanzen kommuniziert und so die End-to-End-Kommunikation gesichert ist. Dieser Prozess wird in den folgenden Schritten beschrieben:

## Erstellen der Ressourcengruppe

Dieser Abschnitt führt Sie durch die Erstellung einer Ressourcengruppe, die das Anwendungsgateway enthält.

### Schritt 1

Melden Sie sich beim Azure-Konto an.

    Login-AzureRmAccount

### Schritt 2

Wählen Sie das Abonnement aus, das für dieses Szenario verwendet werden soll.

    Select-AzureRmsubscription -SubscriptionName "<Subscription name>"

### Schritt 3

Erstellen Sie eine Ressourcengruppe. (Überspringen Sie diesen Schritt, wenn Sie eine vorhandene Ressourcengruppe verwenden.)

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

## Erstellen eines virtuelles Netzwerkes und eines Subnetzes für das Application Gateway

Im folgenden Beispiel werden ein virtuelles Netzwerk und zwei Subnetze erstellt. In einem Subnetz wird sich das Anwendungsgateway befinden. Das andere Subnetz wird für die Back-End-Server verwendet, die die Webanwendung hosten.

### Schritt 1

Weisen Sie einen Adressbereich für das Subnetz zu, das für das Anwendungsgateway verwendet werden soll.

    $gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24

### Schritt 2

Weisen Sie einen Adressbereich zu, der für den Back-End-Adresspool verwendet werden soll.

    $nicSubnet = New-AzureRmVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24

### Schritt 3

Erstellen Sie ein virtuelles Netzwerk mit den Subnetzen, die in den vorhergehenden Schritten definiert wurden.

    $vnet = New-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet

### Schritt 4

Rufen Sie die virtuelle Netzwerkressource und die Subnetzressourcen ab, die in den folgenden Schritten verwendet werden sollen:

    $vnet = Get-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg
    $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -VirtualNetwork $vnet
    $nicSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appsubnet' -VirtualNetwork $vnet

## Erstellen der öffentlichen IP-Adresse für die Front-End-Konfiguration

Erstellen Sie eine öffentliche IP-Ressource, die für das Anwendungsgateway verwendet werden soll. Diese öffentliche IP-Adresse wird in einem folgenden Schritt verwendet.

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -Name 'appgwpip' -Location "West US" -AllocationMethod Dynamic

> [AZURE.IMPORTANT] Application Gateway unterstützt nicht die Verwendung einer öffentlichen IP-Adresse, die mit einer definierten Domänenbezeichnung erstellt wird. Nur eine öffentliche IP-Adresse mit einer dynamisch erstellten Domänenbezeichnung wird unterstützt. Wenn Sie einen benutzerfreundlichen DNS-Namen für das Anwendungsgateway benötigen, wird empfohlen, einen cname-Datensatz als Alias zu verwenden.

## Erstellen eines Konfigurationsobjekts für das Anwendungsgateway

Sie müssen alle Konfigurationselemente einrichten, bevor Sie das Anwendungsgateway erstellen. Die folgenden Schritten erstellen die Konfigurationselemente, die für eine Application Gateway-Ressource benötigt werden.

### Schritt 1

Erstellen Sie eine Anwendungsgateway-IP-Konfiguration. Diese Einstellung konfiguriert, welches Subnetz vom Anwendungsgateway verwendet wird. Beim Starten des Anwendungsgateways wird eine IP-Adresse aus dem konfigurierten Subnetz ausgewählt, und der Netzwerkdatenverkehr wird an die IP-Adressen im Back-End-IP-Pool weitergeleitet. Beachten Sie, dass jede Instanz eine eigene IP-Adresse benötigt.

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet

### Schritt 2

Erstellen Sie eine Front-End-IP-Konfiguration. Diese Einstellung ordnet dem Front-End des Anwendungsgateways eine private oder öffentliche IP-Adresse zu. Im folgenden Schritt wird die öffentliche IP-Adresse aus dem vorherigen Schritt der Front-End-IP-Konfiguration zugeordnet.

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip

### Schritt 3

Konfigurieren Sie den Back-End-IP-Adresspool mit den IP-Adressen der Back-End-Webserver. Dies sind die IP-Adressen, die den Netzwerkdatenverkehr vom Front-End-IP-Endpunkt empfangen. Sie ersetzen die folgenden IP-Adressen durch die IP-Adressendpunkte Ihrer eigenen Anwendung.

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 1.1.1.1, 2.2.2.2, 3.3.3.3

> [AZURE.NOTE] Ein vollqualifizierter Domänenname (FQDN) ist ebenfalls ein gültiger Wert anstelle einer IP-Adresse für die Back-End-Server.

### Schritt 4

Konfigurieren Sie den Front-End-IP-Port für den öffentlichen IP-Endpunkt. Dieser Port ist der Port, mit dem Endbenutzer eine Verbindung herstellen.

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name 'port01'  -Port 443

### Schritt 5

Konfigurieren Sie das Zertifikat für das Anwendungsgateway. Dieses Zertifikat wird zum Entschlüsseln und erneuten Verschlüsseln des Datenverkehrs auf dem Anwendungsgateway verwendet.

	$cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path to .pfx file> -Password <password for certificate file>

> [AZURE.NOTE] In diesem Beispiel wird das Zertifikat für SSL-Verbindungen konfiguriert. Das Zertifikat muss im PFX-Format vorliegen, und das Kennwort muss zwischen 4 und 12 Zeichen umfassen.

### Schritt 6

Erstellen Sie den HTTP-Listener für das Anwendungsgateway. Weisen Sie die zu verwendende Front-End-IP-Konfiguration, den Port und das SSL-Zertifikat zu.

	$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert

### Schritt 7

Laden Sie das Zertifikat hoch, das auf den SSL-fähigen Ressourcen des Back-End-Pools verwendet werden soll.

    $authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name 'whitelistcert1' -CertificateFile C:\users\gwallace\Desktop\cert.cer

> [AZURE.NOTE] Das in diesem Schritt bereitgestellte Zertifikat sollte dem öffentlichen Schlüssel des PFX-Zertifikats entsprechen, das auf dem Back-End vorliegt. Durch diesen Schritt wird der Back-End beim Anwendungsgateway auf die Whitelist gesetzt.

### Schritt 8

Konfigurieren Sie die HTTP-Einstellungen auf dem Back-End des Anwendungsgateways. Weisen Sie den HTTP-Einstellungen das Zertifikat zu, das im vorherigen Schritt hochgeladen wurde.

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name 'setting01' -Port 443 -Protocol Https -CookieBasedAffinity Enabled -AuthenticationCertificates $authcert

### Schritt 9

Erstellen Sie eine Routingregel für den Lastenausgleich, durch die das Verhalten des Lastenausgleichs konfiguriert wird. In diesem Beispiel wird eine Roundrobin-Basisregel erstellt.

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

### Schritt 10

Konfigurieren Sie die Instanzgröße des Application Gateways. Die verfügbaren Größen sind **Standard\_Small**, **Standard\_Medium** und **Standard\_Large**. Für die Kapazität stehen die Werte 1 bis 10 zur Verfügung.

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

>[AZURE.NOTE] Zu Testzwecken kann für die Anzahl von Instanzen der Wert 1 ausgewählt werden. Beachten Sie, dass eine Anzahl von weniger als zwei Instanzen nicht durch die SLA abgedeckt ist und daher nicht empfohlen wird. Gateways mit niedriger Nutzungsdauer sind für Dev/Test-Umgebungen vorgesehen und sollten nicht zu Produktionszwecken verwendet werden.

### Schritt 11

Konfigurieren Sie die SSL-Richtlinie, die auf dem Anwendungsgateway verwendet werden soll. Application Gateway unterstützt die Möglichkeit, bestimmte Versionen des SSL-Protokolls zu deaktivieren.

Die folgenden Werte sind eine Liste der Protokollversionen, die deaktiviert werden können.

- **TLSv1\_0**
- **TLSv1\_1**
- **TLSv1\_2**

Im folgenden Beispiel werden TLSv1\_0 und TLSv1\_1 deaktiviert.

    $sslPolicy = New-AzureRmApplicationGatewaySslPolicy -DisabledSslProtocols TLSv1_0, TLSv1_1

## Erstellen des Anwendungsgateways

Erstellen Sie das Anwendungsgateway mithilfe der oben aufgeführten Schritte. Die Erstellung des Gateways ist ein langwieriger Prozess.

    $appgw = New-AzureRmApplicationGateway -Name appgateway -SslCertificates $cert -ResourceGroupName "appgw-rg" -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SslPolicy $sslPolicy -AuthenticationCertificates $authcert -Verbose

## Deaktivieren von SSL-Protokollversionen auf einem vorhandenen Anwendungsgateway

Die vorangehenden Schritte führen Sie durch die Erstellung einer Anwendung mit End-to-End-SSL und durch das Deaktivieren bestimmter Versionen des SSL-Protokolls. Im folgenden Beispiel werden bestimmte SSL-Richtlinien auf einem vorhandenen Anwendungsgateway deaktiviert.

### Schritt 1

Rufen Sie das Anwendungsgateway ab, das aktualisiert werden soll.

    $gw = Get-AzureRmApplicationGateway -Name AdatumAppGateway -ResourceGroupName AdatumAppGatewayRG

### Schritt 2

Definieren Sie eine SSL-Richtlinie. Im folgenden Beispiel wird TLSv1.0 deaktiviert.

    Set-AzureRmApplicationGatewaySslPolicy -DisabledSslProtocols TLSv1_0 -ApplicationGateway $gw

### Schritt 3

Zum Schluss aktualisieren Sie das Gateway. Es ist wichtig zu beachten, dass dieser letzte Schritt einen längeren Zeitraum in Anspruch nimmt. Wenn dies abgeschlossen ist, ist End-to-End-SSL auf dem Anwendungsgateway konfiguriert.

    $gw | Set-AzureRmApplicationGateway

## Nächste Schritte

Informationen über das Verstärken der Sicherheit Ihrer Webanwendungen mit Web Application Firewall über Application Gateway finden Sie unter [Web Application Firewall – Übersicht](application-gateway-webapplicationfirewall-overview.md)

[scenario]: ./media/application-gateway-end-to-end-ssl-powershell/scenario.png

<!---HONumber=AcomDC_0928_2016-->