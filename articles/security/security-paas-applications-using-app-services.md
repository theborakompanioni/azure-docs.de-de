---
title: "Schützen webbasierter und mobiler PaaS-Anwendungen mit Azure App Services | Microsoft-Dokumentation"
description: " Weitere Informationen zu den empfohlenen Vorgehensweisen in Azure App Services zum Schutz Ihrer webbasierten und mobilen PaaS-Anwendungen. "
services: security
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/31/2017
ms.author: terrylan
translationtype: Human Translation
ms.sourcegitcommit: b3db1281bbd37fcfbec54bdeb998e6338bb08369
ms.openlocfilehash: 50421a70ede62250a0ab17a38200995c90fdcd55


---
# <a name="securing-paas-web-and-mobile-applications-using-azure-app-services"></a>Schützen webbasierter und mobiler PaaS-Anwendungen mit Azure App Services

In diesem Artikel erläutern wir eine Sammlung empfohlener Vorgehensweisen in [Azure App Services](https://azure.microsoft.com/services/app-service/) zum Schutz Ihrer webbasierten und mobilen PaaS-Anwendungen. Diese empfohlenen Vorgehensweisen sind aus unseren Erfahrungen mit Azure und den Erfahrungen von Kunden wie Ihnen abgeleitet.

## <a name="azure-app-services"></a>Azure App Services
[Azure App Services](../app-service/app-service-value-prop-what-is.md) sind ein PaaS-Angebot, mit dem Sie webbasierte und mobile Apps für beliebige Plattformen oder Geräte erstellen und eine Verbindung mit Daten herstellen können, die in der Cloud oder lokal gespeichert sind. App Services umfassen die Web- und Mobilfunktionen, die vorher separat als Azure Websites und Azure Mobile Services bereitgestellt wurden. Außerdem sind neue Funktionen zum Automatisieren von Geschäftsprozessen und Hosten von Cloud-APIs enthalten. Als einzelner integrierter Dienst stellen App Services einen umfangreichen Satz von Funktionen für mobile, Web- und Integrationsszenarien bereit.

Weitere Informationen finden Sie in den Übersichten zu [Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) und [Web-Apps](../app-service-web/app-service-web-overview.md).

## <a name="best-practices"></a>Bewährte Methoden

Befolgen bei Verwendung von App Services diese bewährten Methoden:

- [Authentifizieren Sie sich über Azure Active Directory (AD)](../app-service-web/web-sites-authentication-authorization.md#authenticate-through-azure-active-directory). App Services bieten einen OAuth 2.0-Dienst für Ihren Identitätsanbieter an. In OAuth 2.0 liegt der Schwerpunkt auf der Vereinfachung der Cliententwicklung. Gleichzeitig werden bestimmte Autorisierungsabläufe für Webanwendungen, Desktopanwendungen und Mobiltelefone bereitgestellt. Azure AD verwendet OAuth 2.0, um Ihnen die Autorisierung des Zugriffs auf mobile und Webanwendungen zu ermöglichen.
- Schränken Sie den Zugriff auf Grundlage der Sicherheitsprinzipien „Need-to-know“ und „geringste Rechte“ ein. Das Einschränken des Zugriffs ist für Organisationen zwingend erforderlich, die Sicherheitsrichtlinien für den Datenzugriff durchsetzen möchten. Die rollenbasierte Zugriffssteuerung (RBAC) kann verwendet werden, um Benutzern, Gruppen und Anwendungen Berechtigungen für einen bestimmten Bereich zu erteilen. Weitere Informationen zum Erteilen des Zugriffs auf Anwendungen für Benutzer finden Sie unter [Erste Schritte mit der Zugriffsverwaltung](../active-directory/role-based-access-control-what-is.md).
- Schützen Sie Ihre Schlüssel. Ihre Sicherheitsfunktionen können noch so ausgereift sein, wenn Sie Ihre Abonnementschlüssel verlieren. Der Azure-Schlüsseltresor unterstützt Sie dabei, kryptografische Schlüssel und geheime Schlüssel zu schützen, die von Cloudanwendungen und -diensten verwendet werden. Durch Verwenden von Key Vault können Sie Schlüssel und Geheimnisse (beispielsweise Authentifizierungsschlüssel, Schlüssel für Speicherkonten, Datenverschlüsselungsschlüssel, PFX-Dateien und Kennwörter) verschlüsseln, indem Sie Schlüssel verwenden, die durch Hardwaresicherheitsmodule (HSMs) geschützt werden. Zur Steigerung der Sicherheit können Sie Schlüssel in HSMs importieren oder in diesen generieren. Weitere Informationen finden Sie unter [Azure Key Vault](../key-vault/key-vault-whatis.md). Sie können auch Key Vault zum Verwalten Ihrer TLS-Zertifikate mit der automatischen Verlängerung verwenden.
- Schränken Sie eingehende Quell-IP-Adressen ein. App Services verfügen über ein Feature zur Integration virtueller Netzwerke, mit dem Sie eingehende Quell-IP-Adressen über Netzwerksicherheitsgruppen (NSGs) einschränken können. Falls Sie sich mit Azure Virtual Networks (VNETs) noch nicht auskennen, hilft Ihnen vielleicht diese Beschreibung weiter: Es handelt sich um eine Funktion, mit der Sie viele Azure-Ressourcen in einem nicht über das Internet routbaren Netzwerk anordnen können, für das Sie den Zugriff kontrollieren. Weitere Informationen hierzu finden Sie unter [Integrieren Ihrer App in ein Azure Virtual Network](../app-service-web/web-sites-integrate-with-vnet.md).

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel wurde Ihnen eine Sammlung empfohlener Vorgehensweisen in Azure App Services zum Schutz Ihrer webbasierten und mobilen PaaS-Anwendungen vorgestellt. Weitere Informationen zum Schutz Ihrer PaaS-Bereitstellungen finden Sie unter:

- [Schützen von PaaS-Bereitstellungen](security-paas-deployments.md)
- [Schützen webbasierter und mobiler PaaS-Anwendungen mit Azure SQL-Datenbank und SQL Data Warehouse](security-paas-applications-using-sql.md)



<!--HONumber=Feb17_HO1-->


