---
title: Infodatei zur Azure App Service-Umgebung
description: Auflistung der Dokumentation, die die Azure App Service-Umgebung beschreibt
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 77452413-5193-4762-8b3d-5fa8e4edf1ca
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: ccompy
ms.translationtype: HT
ms.sourcegitcommit: 79bebd10784ec74b4800e19576cbec253acf1be7
ms.openlocfilehash: 5b1362854dbc3b0098718bd2ea3cffb06366000c
ms.contentlocale: de-de
ms.lasthandoff: 08/03/2017

---

# <a name="app-service-environment-documentation"></a>Dokumentation zur App Service-Umgebung
 Eine Azure App Service-Umgebung ist ein Feature von Azure App Service, das eine vollständig isolierte und dedizierte Umgebung zur sicheren und umfangreichen Ausführung von App Service-Apps bereitstellt. Diese Funktion kann Ihre [Web-Apps][webapps], [mobilen Apps][mobileapps], [API-Apps][APIApps] und [Funktionen][Functions] hosten.

App Service-Umgebungen (ASEs) sind ideal geeignet für Anwendungsworkloads mit folgenden Anforderungen:

* Unterstützung sehr vieler Apps.
* Isolierung und sicherer Netzwerkzugriff.

Kunden können mehrere ASEs innerhalb einer einzelnen Azure-Region oder über mehrere Azure-Regionen verteilt einrichten. Durch diese Vielseitigkeit eignen sich ASEs hervorragend für die horizontale Skalierung zustandsloser Anwendungsebenen zur Unterstützung hoher RPS-Workloads.

Aufgrund der Isolierung werden in ASEs nur Anwendungen eines einzelnen Kunden ausgeführt. Die Umgebungen werden zudem immer in einem virtuellen Azure-Netzwerk bereitgestellt. Kunden haben präzise Kontrolle über den eingehenden und ausgehenden Netzwerkdatenverkehr der Anwendung, indem sie [Netzwerksicherheitsgruppen][NSGs] verwenden. Anwendungen können auch schnelle, sichere Verbindungen über virtuelle Netzwerke zu lokalen Unternehmensressourcen einrichten.

Apps müssen häufig auf Unternehmensressourcen wie interne Datenbanken und Webdienste zugreifen. In ASEs ausgeführte Apps können über [Site-to-Site][SiteToSite]-VPN- und [Azure ExpressRoute][ExpressRoute]-Verbindungen auf Ressourcen zugreifen.

* [Was ist eine App Service-Umgebung?][Intro]
* [Erstellen einer App Service-Umgebung][MakeExternalASE]
* [Erstellen einer App Service-Umgebung für internen Lastenausgleich][MakeILBASE]
* [Verwenden einer App Service-Umgebung][UsingASE]
* [Überlegungen zum Netzwerkbetrieb und zur App Service-Umgebung][ASENetwork]
* [Erstellen einer App Service-Umgebung über eine Vorlage][MakeASEfromTemplate]


## <a name="videos"></a>Videos
Master Modern PaaS for the Enterprise with Azure App Service (Modernes PaaS für Unternehmen mit Azure App Service)
>[!VIDEO https://channel9.msdn.com/Events/Ignite/2016/BRK3205/player]

Deploying Highly Scalable and Secure Apps (Bereitstellen hochgradig skalierbarer und sicherer Apps)
>[!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON325/player]

Running Enterprise Web and Mobile Apps on Azure App Service (Ausführen von Unternehmens-Web-Apps und mobilen Apps in Azure App Service)
>[!VIDEO https://channel9.msdn.com/Events/Ignite/2015/BRK3715/player]

## <a name="app-service-environment-v1"></a>App Service-Umgebung v1 ##
Es gibt zwei Versionen der App Service-Umgebung: ASEv1 und ASEv2. Informationen zur ASEv1 finden Sie in der [Dokumentation zur App Service-Umgebung v1][ASEv1README].


<!--Links-->
[Intro]: ./intro.md
[MakeExternalASE]: ./create-external-ase.md
[MakeASEfromTemplate]: ./create-from-template.md
[MakeILBASE]: ./create-ilb-ase.md
[ASENetwork]: ./network-info.md
[ASEReadme]: ./readme.md
[UsingASE]: ./using-an-ase.md
[UDRs]: ../../virtual-network/virtual-networks-udr-overview.md
[NSGs]: ../../virtual-network/virtual-networks-nsg.md
[ConfigureASEv1]: ../../app-service-web/app-service-web-configure-an-app-service-environment.md
[ASEv1Intro]: ../../app-service-web/app-service-app-service-environment-intro.md
[webapps]: ../../app-service-web/app-service-web-overview.md
[mobileapps]: ../../app-service-mobile/app-service-mobile-value-prop.md
[APIapps]: ../../app-service-api/app-service-api-apps-why-best-platform.md
[Functions]: ../../azure-functions/index.yml
[Pricing]: http://azure.microsoft.com/pricing/details/app-service/
[ARMOverview]: ../../azure-resource-manager/resource-group-overview.md
[ConfigureSSL]: ../../app-service-web/web-sites-purchase-ssl-web-site.md
[Kudu]: http://azure.microsoft.com/resources/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[AppDeploy]: ../../app-service-web/web-sites-deploy.md
[ASEWAF]: ../../app-service-web/app-service-app-service-environment-web-application-firewall.md
[AppGW]: ../../application-gateway/application-gateway-web-application-firewall-overview.md
[PremiumTier]: http://azure.microsoft.com/pricing/details/app-service/
[ASEv1README]: ../app-service-app-service-environments-readme.md
[SiteToSite]: ../../vpn-gateway/vpn-gateway-site-to-site-create.md
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/

