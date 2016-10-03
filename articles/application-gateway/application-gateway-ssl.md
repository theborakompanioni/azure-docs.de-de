<properties
   pageTitle="Konfigurieren eines Application Gateways für SSL-Auslagerung mit klassischer Bereitstellung | Microsoft Azure"
   description="Dieser Artikel enthält Anweisungen zum Erstellen eines Application Gateways mit SSL-Auslagerung mit dem klassischen Azure-Bereitstellungsmodell."
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/09/2016"
   ms.author="gwallace"/>

# Konfigurieren eines Application Gateways für SSL-Auslagerung mit klassischem Bereitstellungsmodell

> [AZURE.SELECTOR]
-[Azure portal](application-gateway-ssl-portal.md)
-[Azure Resource Manager PowerShell](application-gateway-ssl-arm.md)
-[Azure Classic PowerShell](application-gateway-ssl.md)

Azure Application Gateway kann so konfiguriert werden, dass damit die Secure Sockets Layer-Sitzung (SSL) auf dem Gateway beendet wird. Auf diese Weise wird die aufwändige SSL-Entschlüsselung in der Webfarm vermieden. Die SSL-Auslagerung vereinfacht zudem die Einrichtung und Verwaltung der Webanwendung auf dem Front-End-Server.


## Voraussetzungen

1. Installieren Sie mit dem Webplattform-Installer die aktuelle Version der Azure PowerShell-Cmdlets. Sie können die neueste Version aus dem Abschnitt **Windows PowerShell** der [Downloadseite](https://azure.microsoft.com/downloads/) herunterladen und installieren.
2. Stellen Sie sicher, dass Sie über ein funktionierendes virtuelles Netzwerk mit einem gültigen Subnetz verfügen. Stellen Sie sicher, dass keine virtuellen Maschinen oder Cloudbereitstellungen das Subnetz verwenden. Das Application Gateway muss sich allein im Subnetz eines virtuellen Netzwerks befinden.
3. Die Server, die Sie für die Verwendung des Anwendungsgateways konfigurieren, müssen vorhanden sein oder Endpunkte aufweisen, die im virtuellen Netzwerk erstellt wurden oder denen eine öffentliche IP-Adresse/VIP zugewiesen wurde.

Führen Sie die folgenden Schritte in der angegebenen Reihenfolge aus, um die SSL-Auslagerung auf einem Application Gateway zu konfigurieren:

1. [Erstellen eines Anwendungsgateways](#create-an-application-gateway)
2. [Hochladen von SSL-Zertifikaten](#upload-ssl-certificates)
3. [Konfigurieren des Gateways](#configure-the-gateway)
4. [Festlegen der Gatewaykonfiguration](#set-the-gateway-configuration)
5. [Starten des Gateways](#start-the-gateway)
6. [Überprüfen des Gatewaystatus](#verify-the-gateway-status)


## Erstellen eines Anwendungsgateways

Verwenden Sie zum Erstellen des Gateways das **New-AzureApplicationGateway**-Cmdlet, und ersetzen Sie die Werte durch Ihre eigenen Werte. Die Abrechnung für das Gateway beginnt jetzt noch nicht. Die Abrechnung beginnt in einem späteren Schritt, wenn das Gateway erfolgreich gestartet wurde.

	New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")

Sie können das **Get-AzureApplicationGateway**-Cmdlet verwenden, um zu überprüfen, ob das Gateway erstellt wurde.

In diesem Beispiel sind *Description*, *InstanceCount* und *GatewaySize* optionale Parameter. Der Standardwert für *InstanceCount* ist 2, der Maximalwert ist 10. Der Standardwert für *GatewaySize* ist "Medium". "Small" und "Large" sind weitere verfügbare Werte. *VirtualIPs* und *DnsName* werden leer angezeigt, da das Gateway noch nicht gestartet wurde. Diese Werte werden erstellt, sobald das Gateway ausgeführt wird.

	Get-AzureApplicationGateway AppGwTest

## Hochladen von SSL-Zertifikaten

Verwenden Sie **Add-AzureApplicationGatewaySslCertificate**, um das Serverzertifikat im *pfx*-Format auf das Application Gateway hochzuladen. Der Name des Zertifikats wird vom Benutzer ausgewählt, und er muss im Anwendungsgateway eindeutig sein. Für dieses Zertifikat wird in allen Zertifikatverwaltungsvorgängen auf dem Anwendungsgateway dieser Name verwendet.

Das folgende Beispiel zeigt das Cmdlet. Ersetzen Sie die Werte im Beispiel durch Ihre eigenen Werte.

	Add-AzureApplicationGatewaySslCertificate  -Name AppGwTest -CertificateName GWCert -Password <password> -CertificateFile <full path to pfx file>

Überprüfen Sie als Nächstes den Zertifikatupload. Verwenden Sie das **Get-AzureApplicationGatewayCertificate**-Cmdlet.

Dieses Beispiel zeigt das Cmdlet in der ersten Zeile, gefolgt von der Ausgabe.

	Get-AzureApplicationGatewaySslCertificate AppGwTest

	VERBOSE: 5:07:54 PM - Begin Operation: Get-AzureApplicationGatewaySslCertificate
	VERBOSE: 5:07:55 PM - Completed Operation: Get-AzureApplicationGatewaySslCertificate
	Name           : SslCert
	SubjectName    : CN=gwcert.app.test.contoso.com
	Thumbprint     : AF5ADD77E160A01A6......EE48D1A
	ThumbprintAlgo : sha1RSA
	State..........: Provisioned

>[AZURE.NOTE] Das Zertifikatkennwort darf aus Buchstaben und Ziffern bestehen und muss zwischen vier und 12 Zeichen lang sein. Sonderzeichen werden nicht akzeptiert.

## Konfigurieren des Gateways

Eine Anwendungsgatewaykonfiguration besteht aus mehreren Werten. Die Werte können verknüpft werden, um die Konfiguration zu erstellen.

Die Werte sind:

- **Back-End-Serverpool:** Die Liste der IP-Adressen der Back-End-Server. Die aufgelisteten IP-Adressen sollten entweder dem Subnetz des virtuellen Netzwerks angehören oder eine öffentliche IP-Adresse/VIP sein.
- **Einstellungen für den Back-End-Serverpool:** Jeder Pool weist Einstellungen wie Port, Protokoll und cookiebasierte Affinität auf. Diese Einstellungen sind an einen Pool gebunden und gelten für alle Server innerhalb des Pools.
- **Front-End-Port:** Dieser Port ist der öffentliche Port, der im Application Gateway geöffnet ist. Datenverkehr erreicht diesen Port und wird dann an einen der Back-End-Server umgeleitet.
- **Listener:** Der Listener verfügt über einen Front-End-Port, ein Protokoll („Http“ oder „Https“; jeweils unter Beachtung der Groß-/Kleinschreibung) und den Namen des SSL-Zertifikats (falls die SSL-Auslagerung konfiguriert wird).
- **Regel:** Mit der Regel werden der Listener und der Back-End-Serverpool gebunden, und es wird definiert, an welchen Back-End-Serverpool der Datenverkehr gesendet werden soll, wenn er einen bestimmten Listener erreicht. Derzeit wird nur die Regel *basic* unterstützt. Die Regel *basic* ist eine Round-Robin-Lastverteilung.

**Zusätzliche Konfigurationshinweise**

Für die Konfiguration von SSL-Zertifikaten sollte das Protokoll in **HttpListener** in *Https*(Groß-/Kleinschreibung beachten) geändert werden. Das **SslCert**-Element wird zu **HttpListener** hinzugefügt. Dabei muss der Wert auf den Namen festgelegt werden, der im obigen Abschnitt zum Hochladen von SSL-Zertifikaten verwendet wurde. Der Front-End-Port sollte auf 443 aktualisiert werden.

**So aktivieren Sie cookiebasierte Affinität** Ein Anwendungsgateway kann so konfiguriert werden, dass es sicherstellt, dass die Anforderung von einer Clientsitzung immer an denselben virtuellen Computer in der Webfarm weitergeleitet wird. Für dieses Szenario wird das Einfügen eines Sitzungscookies genutzt, damit das Gateway den Datenverkehr entsprechend weiterleiten kann. Legen Sie zum Aktivieren der cookiebasierten Affinität **CookieBasedAffinity** im **BackendHttpSettings**-Element auf *Enabled* fest.



Sie können die Konfiguration erzeugen, indem Sie ein Konfigurationsobjekt erstellen oder eine XML-Konfigurationsdatei verwenden. Um die Konfiguration mithilfe einer XML-Konfigurationsdatei zu erstellen, verwenden Sie das folgende Beispiel:

**XML-Konfigurationsbeispiel**

	<?xml version="1.0" encoding="utf-8"?>
	<ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
	    <FrontendIPConfigurations />
	    <FrontendPorts>
	        <FrontendPort>
	            <Name>FrontendPort1</Name>
	            <Port>443</Port>
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
	            <Protocol>Https</Protocol>
	            <SslCert>GWCert</SslCert>
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


## Festlegen der Gatewaykonfiguration

Dann legen Sie das Anwendungsgateway fest. Sie können das **Set-AzureApplicationGatewayConfig**-Cmdlet entweder mit einem Konfigurationsobjekt oder mit einer XML-Konfigurationsdatei verwenden.

	Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile D:\config.xml

## Starten des Gateways

Verwenden Sie nach dem Konfigurieren des Gateways das **Start-AzureApplicationGateway**-Cmdlet, um das Gateway zu starten. Die Abrechnung für ein Application Gateway beginnt, nachdem das Gateway erfolgreich gestartet wurde.


**Hinweis:** Es kann 15 bis 20 Minuten dauern, bis die Ausführung des **Start-AzureApplicationGateway**-Cmdlets abgeschlossen ist.

	Start-AzureApplicationGateway AppGwTest

## Überprüfen des Gatewaystatus

Verwenden Sie das **Get-AzureApplicationGateway**-Cmdlet, um den Status des Gateways zu überprüfen. Wenn **Start-AzureApplicationGateway** im vorherigen Schritt erfolgreich ausgeführt wurde, sollte der *Status* „Wird ausgeführt“ lauten, und *VirtualIPs* und *DnsName* sollten gültige Einträge aufweisen.

Dieses Beispiel zeigt ein Application Gateway, das ausgeführt wird und Datenverkehr verarbeiten kann.

	Get-AzureApplicationGateway AppGwTest

	Name          : AppGwTest2
	Description   :
	VnetName      : testvnet1
	Subnets       : {Subnet-1}
	InstanceCount : 2
	GatewaySize   : Medium
	State         : Running
	VirtualIPs    : {23.96.22.241}
	DnsName       : appgw-4c960426-d1e6-4aae-8670-81fd7a519a43.cloudapp.net


## Nächste Schritte


Weitere Informationen zu Lastenausgleichsoptionen im Allgemeinen finden Sie unter:

- [Azure-Lastenausgleich](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)

<!---HONumber=AcomDC_0921_2016-->