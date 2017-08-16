---
title: "Anpassen von Web Application Firewall-Regeln im Azure Application Gateway-Portal – PowerShell | Microsoft-Dokumentation"
description: "Diese Seite enthält Informationen zur Anpassung von Web Application Firewall-Regeln (WAF) in Application Gateway mit PowerShell."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: 
ms.workload: infrastructure-services
ms.date: 07/26/2017
ms.author: gwallace
ms.translationtype: HT
ms.sourcegitcommit: 8b857b4a629618d84f66da28d46f79c2b74171df
ms.openlocfilehash: 7ed79af5654939c86ca9449be2a59aa0be1136dc
ms.contentlocale: de-de
ms.lasthandoff: 08/04/2017

---

# <a name="customize-web-application-firewall-rules-through-powershell"></a>Anpassen von Web Application Firewall-Regeln über PowerShell

> [!div class="op_single_selector"]
> * [Azure-Portal](application-gateway-customize-waf-rules-portal.md)
> * [PowerShell](application-gateway-customize-waf-rules-powershell.md)
> * [Azure CLI 2.0](application-gateway-customize-waf-rules-cli.md)

Die Web Application Firewall von Application Gateway bietet Schutz für Webanwendungen. Dieser Schutz wird durch OWASP CRS-Regelsätze bereitgestellt. Einige Regeln können falsche positive Ergebnisse ausgeben und den realen Datenverkehr blockieren.  Aus diesem Grund bietet Application Gateway die Möglichkeit, Regelgruppen und Regeln in einem Web Application Firewall-fähigen Anwendungsgateway anzupassen. Weitere Informationen zu den jeweiligen Regelgruppen und Regeln finden Sie unter [CRS-Regelgruppen und -Regeln der Web Application Firewall](application-gateway-crs-rulegroups-rules.md).

## <a name="view-rule-groups-and-rules"></a>Anzeigen von Regelgruppen und Regeln

Die folgenden Beispiele veranschaulichen, wie Regeln und Regelgruppen angezeigt werden, die in einem WAF-fähigen Anwendungsgateway konfigurierbar sind.

### <a name="view-rule-groups"></a>Anzeigen von Regelgruppen

```powershell
Get-AzureRmApplicationGatewayAvailableWafRuleSets
```

Im Folgenden sehen Sie eine abgeschnittene Antwort aus dem vorherigen Beispiel:

```
OWASP (Ver. 3.0):

    REQUEST-910-IP-REPUTATION:
        Description:
            
        Rules:
            RuleId     Description
            ------     -----------
            910011     Rule 910011
            910012     Rule 910012
            ...        ...

    REQUEST-911-METHOD-ENFORCEMENT:
        Description:
            
        Rules:
            RuleId     Description
            ------     -----------
            911011     Rule 911011
            ...        ...

OWASP (Ver. 2.2.9):

    crs_20_protocol_violations:
        Description:
            
        Rules:
            RuleId     Description
            ------     -----------
            960911     Invalid HTTP Request Line
            981227     Apache Error: Invalid URI in Request.
            960000     Attempted multipart/form-data bypass
            ...        ...
```

## <a name="disable-rules"></a>Deaktivieren von Regeln

Das folgende Beispiel deaktiviert die Regeln `910018` und `910017` für ein Anwendungsgateway:

```azurecli
az network application-gateway waf-config set --resource-group AdatumAppGatewayRG --gateway-name AdatumAppGateway --enabled true --rule-set-version 3.0 --disabled-rules 910018 910017
```

## <a name="next-steps"></a>Nächste Schritte

Nach der Konfiguration der deaktivierten Regeln erfahren Sie über [Application Gateway-Diagnosen](application-gateway-diagnostics.md#diagnostic-logging), wie Sie WAF-Protokolle anzeigen.

[fig1]: ./media/application-gateway-customize-waf-rules-portal/1.png
[1]: ./media/application-gateway-customize-waf-rules-portal/figure1.png
[2]: ./media/application-gateway-customize-waf-rules-portal/figure2.png
[3]: ./media/application-gateway-customize-waf-rules-portal/figure3.png
