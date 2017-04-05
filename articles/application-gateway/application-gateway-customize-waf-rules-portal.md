---
title: Anpassen von Web Application Firewall-Regeln im Azure Application Gateway-Portal | Microsoft-Dokumentation
description: "Diese Seite enthält Informationen zur Anpassung von Web Application Firewall-Regeln (WAF) in Application Gateway mit dem Portal."
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
translationtype: Human Translation
ms.sourcegitcommit: 432752c895fca3721e78fb6eb17b5a3e5c4ca495
ms.openlocfilehash: b08d5543fd3b680b7d49be6e8d2c95ee71aff10c
ms.lasthandoff: 03/30/2017


---

# <a name="customize-web-application-firewall-rules-through-the-portal"></a>Anpassen von Web Application Firewall-Regeln über das Portal

Die Web Application Firewall von Application Gateway bietet Schutz für Webanwendungen. Dieser Schutz wird durch OWASP CRS-Regelsätze bereitgestellt. Einige Regeln können falsche positive Ergebnisse ausgeben und den realen Datenverkehr blockieren.  Aus diesem Grund bietet Application Gateway die Möglichkeit, Regelgruppen und Regeln in einem Web Application Firewall-fähigen Anwendungsgateway anzupassen. Weitere Informationen zu den jeweiligen Regelgruppen und Regeln finden Sie unter [CRS-Regelgruppen und -Regeln der Web Application Firewall](application-gateway-crs-rulegroups-rules.md).

>[!NOTE]
> Wenn Ihr Anwendungsgateway nicht auf die WAF-Ebene zurückgreift, wird die Option zum Aktualisieren des Anwendungsgateways auf die WAF-Ebene angezeigt, wie in der folgenden Abbildung dargestellt wird:

![WAF aktivieren][fig1]

## <a name="view-rule-groups-and-rules"></a>Anzeigen von Regelgruppen und Regeln

Navigieren Sie zu einem Anwendungsgateway und wählen Sie **Web Application Firewall** aus.  Klicken Sie auf **Erweiterte Regelkonfiguration**.  Daraufhin wird eine Tabelle auf der Seite aller Regelgruppen angezeigt, die mit dem gewählten Regelsatz bereitgestellt werden.

![Deaktivierte Regeln konfigurieren][1]

## <a name="search-for-rules-to-disable"></a>Suchen nach zu deaktivierenden Regeln

Das Blatt mit den Web Applicaton Firewall-Einstellungen bietet die Möglichkeit, die Regeln durch eine Textsuche zu filtern. Das Ergebnis enthält nur Regelgruppen und Regeln, die den gesuchten Text enthalten.

![Nach Regeln suchen][2]

## <a name="disable-rule-groups-and-rules"></a>Deaktivieren von Regelgruppen und Regeln

Bei der Deaktivierung von Regeln können Sie eine gesamte Regelgruppe oder bestimmte Regeln in einer oder in mehreren Regelgruppen deaktivieren.  Sobald die Auswahl für die zu deaktivierenden Regeln aufgehoben wurde, klicken Sie auf **Speichern**.  Hierdurch werden die Änderungen am Anwendungsgateway gespeichert.

![Änderungen speichern][3]

## <a name="next-steps"></a>Nächste Schritte

Nach der Konfiguration der deaktivierten Regeln erfahren Sie über [Application Gateway-Diagnosen](application-gateway-diagnostics.md#diagnostic-logging), wie Sie WAF-Protokolle anzeigen.

[fig1]: ./media/application-gateway-customize-waf-rules-portal/1.png
[1]: ./media/application-gateway-customize-waf-rules-portal/figure1.png
[2]: ./media/application-gateway-customize-waf-rules-portal/figure2.png
[3]: ./media/application-gateway-customize-waf-rules-portal/figure3.png
