---
title: "Anpassen von Web Application Firewall-Regeln in Azure Application Gateway – Azure-Portal | Microsoft-Dokumentation"
description: "Dieser Artikel enthält Informationen zum Anpassen von Web Application Firewall-Regeln (WAF) in Application Gateway mit dem Azure-Portal."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 1159500b-17ba-41e7-88d6-b96986795084
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: 
ms.workload: infrastructure-services
ms.date: 03/28/2017
ms.author: gwallace
ms.translationtype: HT
ms.sourcegitcommit: cf381b43b174a104e5709ff7ce27d248a0dfdbea
ms.openlocfilehash: cdcbadbc3765dfc583c26e1b1453863d421c9a72
ms.contentlocale: de-de
ms.lasthandoff: 08/23/2017

---

# <a name="customize-web-application-firewall-rules-through-the-azure-portal"></a>Anpassen von Web Application Firewall-Regeln mit dem Azure-Portal

> [!div class="op_single_selector"]
> * [Azure-Portal](application-gateway-customize-waf-rules-portal.md)
> * [PowerShell](application-gateway-customize-waf-rules-powershell.md)
> * [Azure CLI 2.0](application-gateway-customize-waf-rules-cli.md)

Die Web Application Firewall (WAF) von Azure Application Gateway bietet Schutz für Webanwendungen. Diese Schutzmaßnahmen werden durch die Kernregeln (Core Rule Set, CRS) des Open Web Application Security-Projekts (OWASP) bereitgestellt. Einige Regeln können falsche positive Ergebnisse ausgeben und den realen Datenverkehr blockieren. Aus diesem Grund bietet Application Gateway die Möglichkeit, Regelgruppen und Regeln anzupassen. Weitere Informationen zu den jeweiligen Regelgruppen und Regeln finden Sie in der [Liste der CRS-Regelgruppen und -Regeln der Web Application Firewall](application-gateway-crs-rulegroups-rules.md).

>[!NOTE]
> Wenn Ihr Anwendungsgateway nicht auf die WAF-Ebene zurückgreift, wird die Option zum Aktualisieren des Anwendungsgateways auf die WAF-Ebene im rechten Bereich angezeigt. 

![Aktivieren der WAF][fig1]

## <a name="view-rule-groups-and-rules"></a>Anzeigen von Regelgruppen und Regeln

**So zeigen Sie Regelgruppen und Regeln an**
   1. Browsen Sie zum Anwendungsgateway, und wählen Sie **Web Application Firewall** aus.  
   2. Wählen Sie **Erweiterte Regelkonfiguration** aus.  
   Diese Ansicht zeigt auf der Seite eine Tabelle aller Regelgruppen an, die mit dem ausgewählten Regelsatz bereitgestellt werden. Alle Kontrollkästchen der Regel sind aktiviert.

![Konfigurieren deaktivierter Regeln][1]

## <a name="search-for-rules-to-disable"></a>Suchen nach zu deaktivierenden Regeln

Das Blatt **Web Applicaton Firewall-Einstellungen** bietet die Möglichkeit, die Regeln durch eine Textsuche zu filtern. Das Ergebnis enthält nur die Regelgruppen und Regeln, die den gesuchten Text enthalten.

![Suchen nach Regeln][2]

## <a name="disable-rule-groups-and-rules"></a>Deaktivieren von Regelgruppen und Regeln

Wenn Sie Regeln deaktivieren, können Sie eine gesamte Regelgruppe oder bestimmte Regeln in einer oder mehreren Regelgruppen deaktivieren. 

**So deaktivieren Sie Regelgruppen oder bestimmte Regeln**

   1. Suchen Sie nach den Regeln oder Regelgruppen, die Sie deaktivieren möchten.
   2. Aktivieren Sie die Kontrollkästchen für die Regeln, die Sie deaktivieren möchten. 
   2. Wählen Sie **Speichern** aus. 

![Speichern der Änderungen][3]

## <a name="next-steps"></a>Nächste Schritte

Nach dem Konfigurieren Ihrer deaktivierten Regeln können Sie sich darüber informieren, wie Sie WAF-Protokolle anzeigen. Weitere Informationen finden Sie unter [Application Gateway-Diagnose](application-gateway-diagnostics.md#diagnostic-logging).

[fig1]: ./media/application-gateway-customize-waf-rules-portal/1.png
[1]: ./media/application-gateway-customize-waf-rules-portal/figure1.png
[2]: ./media/application-gateway-customize-waf-rules-portal/figure2.png
[3]: ./media/application-gateway-customize-waf-rules-portal/figure3.png

