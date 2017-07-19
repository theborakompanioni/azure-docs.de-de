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
ms.translationtype: Human Translation
ms.sourcegitcommit: cb4d075d283059d613e3e9d8f0a6f9448310d96b
ms.openlocfilehash: 8b401036d4fa8e6c4e6430433b1641f5cb4ae010
ms.contentlocale: de-de
ms.lasthandoff: 06/26/2017

---

# <a name="app-service-environment-documentation"></a>Dokumentation zur App Service-Umgebung
Die App Service-Umgebung ist ein Feature von Azure App Service, das eine vollständig isolierte und dedizierte Umgebung zur sicheren und umfangreichen Ausführung von Azure App Service-Apps bereitstellt. Diese Funktion kann Ihre [Web-Apps][webapps], [mobilen Apps][mobileapps], [API-Apps][APIApps] und [Functions][Functions] hosten.

App Service-Umgebungen sind ideal für Anwendungsworkloads mit folgenden Anforderungen:

* Unterstützung sehr vieler Apps
* Isolierung und sicherer Netzwerkzugriff

Kunden können mehrere App Service-Umgebungen innerhalb einer einzelnen Azure-Region sowie über mehrere Azure-Regionen verteilt einrichten. Dadurch eignen sich App Service-Umgebungen hervorragend für die horizontale Skalierung zustandsloser Anwendungsebenen zur Unterstützung hoher RPS-Workloads.

Aufgrund der Isolierung werden in App Service-Umgebungen nur Anwendungen eines einzelnen Kunden ausgeführt. Die Umgebungen werden zudem immer in einem virtuellen Netzwerk bereitgestellt. Kunden haben präzise Kontrolle über den eingehenden und ausgehenden Netzwerkdatenverkehr der Anwendung, indem sie [Netzwerksicherheitsgruppen][NSGs] verwenden. Anwendungen können auch schnelle, sichere Verbindungen über virtuelle Netzwerke zu lokalen Unternehmensressourcen einrichten.

Apps müssen häufig auf Unternehmensressourcen wie interne Datenbanken und Webdienste zugreifen. In App Service-Umgebungen ausgeführte Apps haben Zugriff auf Ressourcen über [Site-to-Site-VPN][SiteToSite] und [Azure ExpressRoute][ExpressRoute]-Verbindungen.

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

## <a name="asev1"></a>ASEv1 ##
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

