<properties
   pageTitle="Hosten mehrerer Websites mit Application Gateway| Microsoft Azure"
   description="Diese Seite bietet eine Übersicht über die Application Gateway-Unterstützung für mehrere Websites."
   documentationCenter="na"
   services="application-gateway"
   authors="amsriva"
   manager="rossort"
   editor="amsriva"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/07/2016"
   ms.author="amsriva"/>

# Anwendungsgateways – Hosten mehrerer Websites

Das Hosten mehrerer Websites ermöglicht es Ihnen, mehr als eine Webanwendung auf derselben Anwendungsgatewayinstanz zu konfigurieren. Mit diesem Feature können Sie eine effizientere Topologie für Ihre Bereitstellungen konfigurieren, indem Sie bis zu 20 Websites auf einem einzigen Anwendungsgateway bereitstellen. Jede Website kann an ihren eigenen Back-End-Pool weitergeleitet werden. Im folgenden Beispiel verarbeitet Application Gateway den Datenverkehr für „contoso.com“ und „fabrikam.com“ mit zwei Back-End-Serverpools: ContosoServerPool und FabrikamServerPool.

![imageURLroute](./media/application-gateway-multi-site-overview/multisite.png)

Anforderungen für http://contoso.com werden an ContosoServerPool weitergeleitet, Anforderungen für http://fabrikam.com an FabrikamServerPool.

Dementsprechend können zwei Unterdomänen derselben übergeordneten Domäne in der gleichen Anwendungsgatewaybereitstellung gehostet werden. Beispiele für die Verwendung von Unterdomänen umfassen http://blog.contoso.com und http://app.contoso.com, die in einer einzigen Anwendungsgatewaybereitstellung gehostet werden.


## Hostheader und Servernamensanzeige
Es gibt drei allgemeine Mechanismen, um das Hosten mehrerer Websites in der gleichen Infrastruktur zu ermöglichen.

1. Hosten von mehreren Webanwendungen, jeweils mit einer eindeutigen IP-Adresse.
2. Verwenden der Hostnamen zum Hosten mehrerer Webanwendungen mit der gleichen IP-Adresse.
3. Verwenden verschiedener Ports zum Hosten mehrerer Webanwendungen mit der gleichen IP-Adresse.

Derzeit bezieht ein Anwendungsgateway eine einzelne öffentliche IP-Adresse, an der es auf Datenverkehr lauscht. Daher wird das Hosten mehrerer Anwendungen mit jeweils einer eigenen IP-Adresse zurzeit nicht unterstützt. Application Gateway unterstützt das Hosten mehrerer Anwendungen, die jeweils an verschiedenen Ports lauschen. Für dieses Szenario müssten die Anwendungen aber Datenverkehr über nicht standardmäßige Ports akzeptieren. Diese Konfiguration ist aber häufig nicht erwünscht. Application Gateway verwendet HTTP 1.1-Hostheader, um mehrere Websites an der gleichen öffentlichen IP-Adresse und dem gleichen Port zu hosten. Die in Application Gateway gehosteten Websites können mit der TLS-Erweiterung für die Servernamensanzeige (Server Name Indication, SNI) auch die SSL-Auslagerung unterstützen. Das bedeutet, dass der Clientbrowser und die Back-End-Webfarm HTTP/1.1- und TLS-Erweiterungen unterstützen müssen, wie in RFC 6066 definiert.


## Listenerkonfigurationselement

Das vorhandene HTTPListener-Konfigurationselement wird erweitert, um Elemente zur Anzeige von Host- und Servernamen zu unterstützen – diese werden vom Anwendungsgateway verwendet, um den Datenverkehr an den geeigneten Back-End-Pool zu leiten. Das folgende Codebeispiel ist der Codeausschnitt des HttpListeners-Elements aus der Vorlagendatei.

    "httpListeners": [
                {
                    "name": "appGatewayHttpsListener1",
                    "properties": {
                        "FrontendIPConfiguration": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendIPConfigurations/DefaultFrontendPublicIP"
                        },
                        "FrontendPort": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendPorts/appGatewayFrontendPort443'"
                        },
                        "Protocol": "Https",
                        "SslCertificate": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/sslCertificates/appGatewaySslCert1'"
                        },
                        "HostName": "contoso.com",
                        "RequireServerNameIndication": "true"
                    }
                },
                {
                    "name": "appGatewayHttpListener2",
                    "properties": {
                        "FrontendIPConfiguration": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendIPConfigurations/appGatewayFrontendIP'"
                        },
                        "FrontendPort": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendPorts/appGatewayFrontendPort80'"
                        },
                        "Protocol": "Http",
                        "HostName": "fabrikam.com",
                        "RequireServerNameIndication": "false"
                    }
                }
            ],




Eine vollständige vorlagenbasierte Bereitstellung finden Sie unter [Application Gateway for Multi Site Hosting](https://github.com/Azure/azure-quickstart-templates/blob/master/201-application-gateway-multihosting) (Anwendungsgateway zum Hosten mehrerer Websites).

## Routingregel

Für die Routingregel ist keine Änderung erforderlich. Verwenden Sie weiterhin die Basisroutingregel, um den geeigneten Websitelistener mit dem entsprechenden Back-End-Adresspool zu verknüpfen.

	"requestRoutingRules": [
	{
		"name": "<ruleName1>",
		"properties": {
			"RuleType": "Basic",
			"httpListener": {
				"id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/httpListeners/appGatewayHttpsListener1')]"
			},
			"backendAddressPool": {
				"id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendAddressPools/ContosoServerPool')]"
			},
			"backendHttpSettings": {
				"id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
			}
		}

	},
	{
		"name": "<ruleName2>",
		"properties": {
			"RuleType": "Basic",
			"httpListener": {
				"id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/httpListeners/appGatewayHttpListener2')]"
			},
			"backendAddressPool": {
				"id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendAddressPools/FabrikamServerPool')]"
			},
			"backendHttpSettings": {
				"id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
			}
		}

	}
	]
	
## Nächste Schritte

Fahren Sie nun mit dem Artikel [Erstellen eines Anwendungsgateways zum Hosten mehrerer Webanwendungen](application-gateway-create-multisite-azureresourcemanager-powershell.md) fort, um ein Anwendungsgateway zu erstellen, das mehr als eine Webanwendung hosten kann.

<!---HONumber=AcomDC_0921_2016-->