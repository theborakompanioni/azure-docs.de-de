---
title: "URL-basiertes Inhaltsrouting – Übersicht | Microsoft Docs"
description: "Diese Seite bietet eine Übersicht über das URL-basierte Inhaltsrouting mit Application Gateway sowie über das urlPathMap-Konfigurationselement und die PathBasedRouting-Regel."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 4409159b-e22d-4c9a-a103-f5d32465d163
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/16/2016
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: ee8cfffdbf054b4251ed269745f6b9ee5a5e6c64
ms.openlocfilehash: 1f273f3b55d719e37b9cdb6cefda30c3566e7226


---
# <a name="url-path-based-routing-overview"></a>Routing auf URL-Pfadbasis – Übersicht

Mit dem Routing auf URL-Pfadbasis kann Datenverkehr basierend auf URL-Pfaden von Anforderungen an Back-End-Serverpools weitergeleitet werden. Ein mögliches Szenario ist die Weiterleitung von Anforderungen für unterschiedliche Inhaltstypen an verschiedene Back-End-Serverpools.
Im folgenden Beispiel verarbeitet Application Gateway Datenverkehr für „contoso.com“ aus drei Back-End-Serverpools: VideoServerPool, ImageServerPool und DefaultServerPool.

![imageURLroute](./media/application-gateway-url-route-overview/figure1.png)

Anforderungen für „http://contoso.com/video“* werden an „VideoServerPool“ und Anforderungen für „http://contoso.com/images“* an „ImageServerPool“ geroutet. DefaultServerPool wird ausgewählt, wenn keines der Pfadmuster zutrifft.

## <a name="urlpathmap-configuration-element"></a>urlPathMap-Konfigurationselement

Mit dem urlPathMap-Element werden Pfadmuster für Zuordnungen zu Back-End-Serverpools angegeben. Das folgende Codebeispiel ist der Codeausschnitt des urlPathMap-Elements aus der Vorlagendatei.

```json
"urlPathMaps": [
{
"name": "<urlPathMapName>",
"id": "/subscriptions/<subscriptionId>/../microsoft.network/applicationGateways/<gatewayName>/ urlPathMaps/<urlPathMapName>",
"properties": {
    "defaultBackendAddressPool": {
        "id": "/subscriptions/<subscriptionId>/../microsoft.network/applicationGateways/<gatewayName>/backendAddressPools/<poolName>"
    },
    "defaultBackendHttpSettings": {
        "id": "/subscriptions/<subscriptionId>/../microsoft.network/applicationGateways/<gatewayName>/backendHttpSettingsList/<settingsName>"
    },
    "pathRules": [
        {
            "paths": [
                <pathPattern>
            ],
            "backendAddressPool": {
                "id": "/subscriptions/<subscriptionId>/../microsoft.network/applicationGateways/<gatewayName>/backendAddressPools/<poolName2>"
            },
            "backendHttpsettings": {
                "id": "/subscriptions/<subscriptionId>/../microsoft.network/applicationGateways/<gatewayName>/backendHttpsettingsList/<settingsName2>"
            },

        },

    ],

}
}
]
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

"name": "<ruleName>",
"id": "/subscriptions/<subscriptionId>/../microsoft.network/applicationGateways/<gatewayName>/requestRoutingRules/<ruleName>",
"properties": {
    "ruleType": "PathBasedRouting",
    "httpListener": {
        "id": "/subscriptions/<subscriptionId>/../microsoft.network/applicationGateways/<gatewayName>/httpListeners/<listenerName>"
    },
    "urlPathMap": {
        "id": "/subscriptions/<subscriptionId>/../microsoft.network/applicationGateways/<gatewayName>/ urlPathMaps/<urlPathMapName>"
    },

}
    }
]
```

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich mit dem URL-basierten Inhaltsrouting vertraut gemacht haben, können Sie mit [Erstellen eines Anwendungsgateways mit URL-basiertem Routing](application-gateway-create-url-route-portal.md) fortfahren, um ein Anwendungsgateway mit URL-Routingregeln zu erstellen.




<!--HONumber=Nov16_HO3-->


