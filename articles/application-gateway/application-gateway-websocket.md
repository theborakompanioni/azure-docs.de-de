<properties
   pageTitle="WebSocket-Unterstützung von Application Gateway | Microsoft Azure"
   description="Diese Seite enthält eine Übersicht über die WebSocket-Unterstützung von Application Gateway."
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
   ms.date="09/16/2016"
   ms.author="amsriva"/>

# WebSocket-Unterstützung von Application Gateway

Das in [RFC6455](https://tools.ietf.org/html/rfc6455) standardisierte WebSocket-Protokoll ermöglicht die Vollduplexkommunikation zwischen Server und Client über eine lange TCP-Verbindung. Dieses Feature ermöglicht wiederum mehr Interaktivität bei der Kommunikation zwischen Webserver und Client, da die Kommunikation auch ohne die bei HTTP-basierten Implementierungen erforderlichen Abfragen bidirektional sein kann. Im Vergleich zu HTTP zeichnen sich WebSockets durch einen geringen Mehraufwand aus. Außerdem können sie die gleiche TCP-Verbindung für mehrere Anforderungen/Antworten verwenden, was eine effizientere Ressourcennutzung zur Folge hat. WebSocket-Protokolle sind für die Nutzung der herkömmlichen HTTP-Ports 80 und 443 konzipiert.

WebSocket wird von Application Gateway nativ für alle Gatewaygrößen unterstützt. Die WebSocket-Unterstützung kann von Benutzern nicht selektiv aktiviert oder deaktiviert werden. WebSocket-Datenverkehr kann mit einem standardmäßigen HTTP-Listener an den Ports 80 und 443 empfangen werden. Der empfangene WebSocket-Datenverkehr wird dann unter Verwendung des entsprechenden Back-End-Pools (gemäß Angabe in Anwendungsgatewayregeln) an den WebSocket-fähigen Back-End-Server weitergeleitet.

Der Back-End-Server muss auf Anwendungsgatewaytests reagieren. Diese werden im Abschnitt [Systemüberwachung des Application Gateways – Übersicht](application-gateway-probe-overview.md) erläutert. Anwendungsgateway-Integritätstests sind rein HTTP-/HTTPS-basiert. Das bedeutet, dass jeder Back-End-Server auf HTTP-Tests reagieren muss, damit WebSocket-Datenverkehr vom Anwendungsgateway an den Server weitergeleitet werden kann.


## Listenerkonfigurationselement

Zur Unterstützung von WebSocket kann ein vorhandener HTTP-Listener verwendet werden. Im Anschluss sehen Sie einen Codeausschnitt des HttpListeners-Elements aus einer Beispielvorlagendatei. Zur Unterstützung von WebSocket-Datenverkehr benötigen Sie sowohl den HTTP- als auch den HTTPS-Listener. Analog dazu können Sie über das [Portal](application-gateway-create-gateway-portal.md) oder mithilfe von [PowerShell](application-gateway-create-gateway-arm.md) ein Anwendungsgateway mit Listenern für die Ports 80 und 443 erstellen, um WebSocket-Datenverkehr zu unterstützen.


    "httpListeners": [
                {
                    "name": "appGatewayHttpsListener",
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
                    }
                },
                {
                    "name": "appGatewayHttpListener",
                    "properties": {
                        "FrontendIPConfiguration": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendIPConfigurations/appGatewayFrontendIP'"
                        },
                        "FrontendPort": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendPorts/appGatewayFrontendPort80'"
                        },
                        "Protocol": "Http",
                    }
                }
            ],

## Konfigurieren von „backendAddressPool“, „backendHttpSetting“ und Routingregel

„backendAddressPool“ dient zum Definieren eines Back-End-Pools mit WebSocket-fähigen Servern. „backendHttpSetting“ sollte nur mit den Back-End-Ports 80 und 443 definiert werden. Eigenschaften für cookiebasierte Affinität und Anforderungstimeouts sind für WebSocket-Datenverkehr nicht relevant. Für die Routingregel ist keine Änderung erforderlich. Verwenden Sie weiterhin die Basisroutingregel, um den geeigneten Listener mit dem entsprechenden Back-End-Adresspool zu verknüpfen.

	"requestRoutingRules": [{
		"name": "<ruleName1>",
		"properties": {
			"RuleType": "Basic",
			"httpListener": {
				"id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/httpListeners/appGatewayHttpsListener')]"
			},
			"backendAddressPool": {
				"id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendAddressPools/ContosoServerPool')]"
			},
			"backendHttpSettings": {
				"id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
			}
		}

	}, {
		"name": "<ruleName2>",
		"properties": {
			"RuleType": "Basic",
			"httpListener": {
				"id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/httpListeners/appGatewayHttpListener')]"
			},
			"backendAddressPool": {
				"id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendAddressPools/ContosoServerPool')]"
			},
			"backendHttpSettings": {
				"id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
			}

		}
	}]

## WebSocket-fähiges Back-End

WebSocket kann nur verwendet werden, wenn das Back-End über einen HTTP-/HTTPS-Webserver verfügt, der an dem konfigurierten Port (üblicherweise 80/443) ausgeführt wird. Dies ist erforderlich, da der anfängliche Handshake des WebSocket-Protokolls HTTP-basiert sein und über ein Headerfeld verfügen muss, das ein Upgrade auf das WebSocket-Protokoll angibt.

	GET /chat HTTP/1.1
    Host: server.example.com
    Upgrade: websocket
    Connection: Upgrade
    Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
    Origin: http://example.com
    Sec-WebSocket-Protocol: chat, superchat
    Sec-WebSocket-Version: 13

Ein weiterer Grund: Der Back-End-Integritätstest des Anwendungsgateways unterstützt nur das HTTP-/HTTPS-Protokoll. Wenn der Back-End-Server nicht auf HTTP-/HTTPS-Tests reagiert, wird er aus dem Back-End-Pool entfernt, wodurch das Back-End für keine Anforderungen mehr erreichbar ist. Das gilt auch für WebSocket-Anforderungen.

## Nächste Schritte

Informieren Sie sich im Anschluss an die WebSocket-Unterstützung unter [Erstellen eines Anwendungsgateways](application-gateway-create-gateway.md) über die ersten Schritte mit einer WebSocket-fähigen Webanwendung.

<!---HONumber=AcomDC_0921_2016-->