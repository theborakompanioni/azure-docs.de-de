---
title: "WebSocket-Unterstützung in Azure Application Gateway | Microsoft-Dokumentation"
description: "Diese Seite enthält eine Übersicht über die WebSocket-Unterstützung von Application Gateway."
documentationcenter: na
services: application-gateway
author: amsriva
manager: rossort
editor: amsriva
ms.assetid: 8968dac1-e9bc-4fa1-8415-96decacab83f
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/08/2017
ms.author: amsriva
ms.translationtype: Human Translation
ms.sourcegitcommit: 09f24fa2b55d298cfbbf3de71334de579fbf2ecd
ms.openlocfilehash: 75b06ddd02da231b7813c609c848c75e42116da5
ms.contentlocale: de-de
ms.lasthandoff: 06/07/2017


---
# <a name="overview-of-websocket-support-in-application-gateway"></a>Übersicht über die WebSocket-Unterstützung in Application Gateway

WebSocket wird von Application Gateway nativ für alle Gatewaygrößen unterstützt. Die WebSocket-Unterstützung kann von Benutzern nicht selektiv aktiviert oder deaktiviert werden. 

Das in [RFC6455](https://tools.ietf.org/html/rfc6455) standardisierte WebSocket-Protokoll ermöglicht die Vollduplexkommunikation zwischen einem Server und einem Client über eine TCP-Verbindung mit langer Laufzeit. Dieses Feature ermöglicht wiederum mehr Interaktivität bei der Kommunikation zwischen dem Webserver und dem Client, da die Kommunikation auch ohne die bei HTTP-basierten Implementierungen erforderlichen Abfragen bidirektional sein kann. Im Vergleich zu HTTP zeichnet sich WebSocket durch einen geringen Mehraufwand aus. Außerdem können sie die gleiche TCP-Verbindung für mehrere Anforderungen/Antworten verwenden, was eine effizientere Ressourcennutzung zur Folge hat. WebSocket-Protokolle sind für die Nutzung der herkömmlichen HTTP-Ports 80 und 443 konzipiert.

WebSocket-Datenverkehr kann mit einem standardmäßigen HTTP-Listener an den Ports 80 und 443 empfangen werden. Der empfangene WebSocket-Datenverkehr wird dann unter Verwendung des entsprechenden Back-End-Pools (gemäß Angabe in Anwendungsgatewayregeln) an den WebSocket-fähigen Back-End-Server weitergeleitet. Der Back-End-Server muss auf die Anwendungsgatewaytests reagieren. Diese werden im Abschnitt [Integritätstest – Übersicht](application-gateway-probe-overview.md) erläutert. Application Gateway-Integritätstests sind auf HTTP/HTTPS beschränkt. Jeder Back-End-Server muss auf HTTP-Tests für das Anwendungsgateway reagieren, damit WebSocket-Datenverkehr an den Server weitergeleitet wird.

## <a name="listener-configuration-element"></a>Listenerkonfigurationselement

Zur Unterstützung von WebSocket-Datenverkehr kann ein vorhandener HTTP-Listener verwendet werden. Es folgt ein Codeausschnitt des „httpListeners“-Elements in einer Beispielvorlagendatei. Zur Unterstützung von WebSocket-Datenverkehr benötigen Sie sowohl den HTTP- als auch den HTTPS-Listener. Analog dazu können Sie über das [Portal](application-gateway-create-gateway-portal.md) oder mithilfe von [PowerShell](application-gateway-create-gateway-arm.md) ein Anwendungsgateway mit Listenern für die Ports 80 und 443 erstellen, um WebSocket-Datenverkehr zu unterstützen.

```json
"httpListeners": [
        {
            "name": "appGatewayHttpsListener",
            "properties": {
                "FrontendIPConfiguration": {
                    "Id": "/subscriptions/{subscriptionId/resourceGroups/{resourceGroupName/providers/Microsoft.Network/applicationGateways/{applicationGatewayName/frontendIPConfigurations/DefaultFrontendPublicIP"
                },
                "FrontendPort": {
                    "Id": "/subscriptions/{subscriptionId/resourceGroups/{resourceGroupName/providers/Microsoft.Network/applicationGateways/{applicationGatewayName/frontendPorts/appGatewayFrontendPort443'"
                },
                "Protocol": "Https",
                "SslCertificate": {
                    "Id": "/subscriptions/{subscriptionId/resourceGroups/{resourceGroupName/providers/Microsoft.Network/applicationGateways/{applicationGatewayName/sslCertificates/appGatewaySslCert1'"
                },
            }
        },
        {
            "name": "appGatewayHttpListener",
            "properties": {
                "FrontendIPConfiguration": {
                    "Id": "/subscriptions/{subscriptionId/resourceGroups/{resourceGroupName/providers/Microsoft.Network/applicationGateways/{applicationGatewayName/frontendIPConfigurations/appGatewayFrontendIP'"
                },
                "FrontendPort": {
                    "Id": "/subscriptions/{subscriptionId/resourceGroups/{resourceGroupName/providers/Microsoft.Network/applicationGateways/{applicationGatewayName/frontendPorts/appGatewayFrontendPort80'"
                },
                "Protocol": "Http",
            }
        }
    ],
```

## <a name="backendaddresspool-backendhttpsetting-and-routing-rule-configuration"></a>Konfigurieren von „backendAddressPool“, „backendHttpSettings“ und Routingregel

„backendAddressPool“ dient zum Definieren eines Back-End-Pools mit WebSocket-fähigen Servern. „backendHttpSettings“ wird mit den Back-End-Ports 80 und 443 definiert. Die Eigenschaften für cookiebasierte Affinität und Anforderungstimeouts sind für WebSocket-Datenverkehr nicht relevant. Für die Routingregel ist keine Änderung erforderlich. Verwenden Sie weiterhin die Basisroutingregel, um den geeigneten Listener mit dem entsprechenden Back-End-Adresspool zu verknüpfen. 

```json
"requestRoutingRules": [{
    "name": "<ruleName1>",
    "properties": {
        "RuleType": "Basic",
        "httpListener": {
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/httpListeners/appGatewayHttpsListener')]"
        },
        "backendAddressPool": {
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendAddressPools/ContosoServerPool')]"
        },
        "backendHttpSettings": {
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
        }
    }

}, {
    "name": "<ruleName2>",
    "properties": {
        "RuleType": "Basic",
        "httpListener": {
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/httpListeners/appGatewayHttpListener')]"
        },
        "backendAddressPool": {
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendAddressPools/ContosoServerPool')]"
        },
        "backendHttpSettings": {
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
        }

    }
}]
```

## <a name="websocket-enabled-backend"></a>WebSocket-fähiges Back-End

WebSocket kann nur verwendet werden, wenn das Back-End über einen HTTP-/HTTPS-Webserver verfügt, der an dem konfigurierten Port (üblicherweise 80/443) ausgeführt wird. Dies ist erforderlich, da der anfängliche Handshake des WebSocket-Protokolls HTTP-basiert sein und über ein Headerfeld verfügen muss, das ein Upgrade auf das WebSocket-Protokoll angibt. Es folgt ein Beispiel eines Headers:

```
    GET /chat HTTP/1.1
    Host: server.example.com
    Upgrade: websocket
    Connection: Upgrade
    Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
    Origin: http://example.com
    Sec-WebSocket-Protocol: chat, superchat
    Sec-WebSocket-Version: 13
```

Ein weiterer Grund: Der Back-End-Integritätstest des Anwendungsgateways unterstützt nur das HTTP-/HTTPS-Protokoll. Wenn der Back-End-Server nicht auf HTTP- oder HTTPS-Tests reagiert, wird er aus dem Back-End-Pool entfernt.

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich im Anschluss an die WebSocket-Unterstützung unter [Erstellen eines Anwendungsgateways](application-gateway-create-gateway.md) über die ersten Schritte mit einer WebSocket-fähigen Webanwendung.


