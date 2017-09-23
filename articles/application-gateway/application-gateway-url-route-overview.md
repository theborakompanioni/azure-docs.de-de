---
title: "URL-basiertes Inhaltsrouting – Übersicht | Microsoft Docs"
description: "Diese Seite bietet eine Übersicht über das URL-basierte Inhaltsrouting mit Application Gateway sowie über das urlPathMap-Konfigurationselement und die PathBasedRouting-Regel."
documentationcenter: na
services: application-gateway
author: davidmu1
manager: timlt
editor: 
ms.assetid: 4409159b-e22d-4c9a-a103-f5d32465d163
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/09/2017
ms.author: davidmu
ms.translationtype: HT
ms.sourcegitcommit: 8b857b4a629618d84f66da28d46f79c2b74171df
ms.openlocfilehash: 75c3279d2d02cb3c6e949d191c88a1eb18b58a27
ms.contentlocale: de-de
ms.lasthandoff: 08/04/2017

---
# <a name="url-path-based-routing-overview"></a>Routing auf URL-Pfadbasis – Übersicht

Mit dem Routing auf URL-Pfadbasis kann Datenverkehr basierend auf URL-Pfaden von Anforderungen an Back-End-Serverpools weitergeleitet werden. 

Ein mögliches Szenario ist die Weiterleitung von Anforderungen für unterschiedliche Inhaltstypen an verschiedene Back-End-Serverpools.

Im folgenden Beispiel verarbeitet Application Gateway Datenverkehr für „contoso.com“ aus drei Back-End-Serverpools: VideoServerPool, ImageServerPool und DefaultServerPool.

![imageURLroute](./media/application-gateway-url-route-overview/figure1.png)

Anforderungen für http://contoso.com/video* werden an VideoServerPool und Anforderungen für http://contoso.com/images* an ImageServerPool geroutet. DefaultServerPool wird ausgewählt, wenn keines der Pfadmuster zutrifft.

> [!IMPORTANT]
> Regeln werden in der Reihenfolge verarbeitet, in der sie im Portal aufgeführt sind. Es wird dringend empfohlen, vor dem Konfigurieren eines einfachen Listeners zuerst Listener für mehrere Standorte zu konfigurieren.  So wird sichergestellt, dass der Datenverkehr an das richtige Back-End geleitet wird. Wenn ein einfacher Listener zuerst aufgeführt wird und sich dafür eine Übereinstimmung mit einer eingehenden Anforderung ergibt, wird die Verarbeitung von diesem Listener durchgeführt.

## <a name="urlpathmap-configuration-element"></a>urlPathMap-Konfigurationselement

Mit dem urlPathMap-Element werden Pfadmuster für Zuordnungen zu Back-End-Serverpools angegeben. Das folgende Codebeispiel ist der Codeausschnitt des urlPathMap-Elements aus der Vorlagendatei.

```json
"urlPathMaps": [{
    "name": "{urlpathMapName}",
    "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/urlPathMaps/{urlpathMapName}",
    "properties": {
        "defaultBackendAddressPool": {
            "id": "/subscriptions/    {subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/backendAddressPools/{poolName1}"
        },
        "defaultBackendHttpSettings": {
            "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/backendHttpSettingsList/{settingname1}"
        },
        "pathRules": [{
            "name": "{pathRuleName}",
            "properties": {
                "paths": [
                    "{pathPattern}"
                ],
                "backendAddressPool": {
                    "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/backendAddressPools/{poolName2}"
                },
                "backendHttpsettings": {
                    "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/backendHttpsettingsList/{settingName2}"
                }
            }
        }]
    }
}]
```

> [!NOTE]
> PathPattern: Diese Einstellung ist eine Liste der abzustimmenden Pfadmuster. Jedes muss mit „/“ beginnen, und ein „*“ ist nur am Ende nach „/“ zulässig. Die Zeichenfolge, die für den Pfadabgleicher bereitgestellt wird, enthält keinen Text nach dem ersten „?“ oder „#“, und diese Zeichen sind hier nicht zulässig.

Weitere Informationen erhalten Sie in einer [Resource Manager-Vorlage mit URL-basiertem Routing](https://azure.microsoft.com/documentation/templates/201-application-gateway-url-path-based-routing) .

## <a name="pathbasedrouting-rule"></a>PathBasedRouting-Regel

RequestRoutingRule vom Typ PathBasedRouting wird verwendet, um einen Listener an ein urlPathMap-Element zu binden. Alle Anforderungen, die für diesen Listener empfangen werden, werden basierend auf einer in urlPathMap angegebenen Richtlinie weitergeleitet.
Codeausschnitt einer PathBasedRouting-Regel:

```json
"requestRoutingRules": [
    {

"name": "{ruleName}",
"id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/requestRoutingRules/{ruleName}",
"properties": {
    "ruleType": "PathBasedRouting",
    "httpListener": {
        "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/httpListeners/<listenerName>"
    },
    "urlPathMap": {
        "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/ urlPathMaps/{urlpathMapName}"
    },

}
    }
]
```

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich mit dem URL-basierten Inhaltsrouting vertraut gemacht haben, können Sie mit [Erstellen eines Anwendungsgateways mit URL-basiertem Routing](application-gateway-create-url-route-portal.md) fortfahren, um ein Anwendungsgateway mit URL-Routingregeln zu erstellen.

