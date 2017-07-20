---
title: "Einführung in die Azure App Service-Umgebung"
description: "Kurze Übersicht über die Azure App Service-Umgebung"
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 3c7eaefa-1850-4643-8540-428e8982b7cb
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: ccompy
ms.translationtype: Human Translation
ms.sourcegitcommit: cb4d075d283059d613e3e9d8f0a6f9448310d96b
ms.openlocfilehash: 0d078aefbf73a45298f397d02ab24b2c8232ecef
ms.contentlocale: de-de
ms.lasthandoff: 06/26/2017

---
# <a name="introduction-to-the-app-service-environment"></a>Einführung in die App Service-Umgebung #
 
## <a name="overview"></a>Übersicht ##

Eine App Service-Umgebung ist ein Feature von Azure App Service, das eine vollständig isolierte und dedizierte Umgebung zur sicheren und umfangreichen Ausführung von Azure App Service-Apps bereitstellt. Diese Funktion kann Ihre [Web-Apps][webapps], [mobilen Apps][mobileapps], [API-Apps][APIapps] und [Functions][Functions] hosten.

App Service-Umgebungen sind ideal für Anwendungsworkloads mit folgenden Anforderungen:

- Unterstützung sehr vieler Apps
- Isolierung und sicherer Netzwerkzugriff
- Hohe Speicherauslastung

Kunden können mehrere App Service-Umgebungen innerhalb einer einzelnen Azure-Region oder über mehrere Azure-Regionen verteilt einrichten. Dadurch eignen sich App Service-Umgebungen hervorragend für die horizontale Skalierung zustandsloser Anwendungsebenen zur Unterstützung hoher RPS-Workloads.

Aufgrund der Isolierung werden in App Service-Umgebungen nur Anwendungen eines einzelnen Kunden ausgeführt. Die Umgebungen werden zudem immer in einem virtuellen Netzwerk bereitgestellt. Kunden haben eine genauere Kontrolle über den eingehenden und ausgehenden Anwendungs-Netzwerkdatenverkehr, und Anwendungen können über VPNs sichere Hochgeschwindigkeitsverbindungen mit lokalen Unternehmensressourcen herstellen.

Alle Artikel und Anleitungen zu App Service-Umgebungen stehen in der [Dokumentation zu App Service-Umgebungen][ASEReadme]zur Verfügung.

Eine Übersicht darüber, wie App Service-Umgebungen hochskalierbaren und sicheren Netzwerkzugriff ermöglichen, finden Sie in der [ausführlichen Betrachtung zu AzureCon](https://azure.microsoft.com/documentation/videos/azurecon-2015-deploying-highly-scalable-and-secure-web-and-mobile-apps/) in App Service-Umgebungen.

Eine ausführliche Betrachtung von horizontaler Skalierung mit mehreren App Service-Umgebungen finden Sie im Artikel zum Einrichten einer [geografisch verteilten App](https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-geo-distributed-scale/).

Informationen darüber, wie die in AzureCon Deep Dive gezeigte Sicherheitsarchitektur konfiguriert wurde, finden Sie im Artikel über das Implementieren einer [Mehrschicht-Sicherheitsarchitektur](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-app-service-environment-layered-security) in App Service-Umgebungen.

Der Zugriff von Apps für App-Umgebungen kann durch Upstreamgeräte wie z. B. Web Application Firewalls (WAF) abgegrenzt werden. Im Artikel zum [Konfigurieren einer Web Application Firewall (WAF) für eine App Service-Umgebung](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-app-service-environment-web-application-firewall) wird dieses Szenario behandelt.

## <a name="dedicated-environment"></a>Dedizierte Umgebung ##

Eine App Service-Umgebung wird dediziert für ausschließlich ein einzelnes Abonnement verwendet und kann 100 Instanzen hosten. Dabei kann es sich um 100 Instanzen in einem einzelnen App Service-Plan oder auch um 100 App Service-Einzelinstanzpläne und alles dazwischen handeln.

Eine App Service-Umgebung besteht aus-Front-Ends und Workers. Die Front-Ends sind für die HTTP/HTTPS-Beendigung sowie für den automatischen Lastenausgleich von App-Anforderungen in einer App Service-Umgebung zuständig. Front-Ends werden automatisch hinzugefügt, wenn die App Service-Pläne in der App Service-Umgebung hochskaliert werden.

Die Worker sind Rollen, die Kunden-Apps hosten. Die Worker sind in 3 festen Größen verfügbar:
* 1 Prozessorkern/3,5 GB RAM
* 2 Prozessorkerne/7 GB RAM
* 4 Prozessorkerne/14 GB RAM

Kunden müssen die Front-Ends und Worker nicht verwalten. Die gesamte Infrastruktur wird automatisch hinzugefügt, wenn Kunden ihre App Service-Pläne horizontal hochskalieren. Wenn App Service-Pläne erstellt oder in einer App Service-Umgebung skaliert werden, wird die erforderliche Infrastruktur nach Bedarf hinzugefügt bzw. entfernt.

Es gibt eine monatliche Pauschalgebühr für eine App Service-Umgebung, mit der die Infrastruktur abgedeckt ist. Dies ändert sich nicht mit der Größe der App Service-Umgebung. Darüber hinaus gibt es einen Kostenplan pro Kern-App Service-Plan. Alle in einer App Service-Umgebung gehosteten Apps befinden sich in der isolierten Preis-SKU. Weitere Informationen zu den Preisen für eine App Service-Umgebung finden Sie auf der Seite [App Service – Preise][Pricing]. Sehen Sie sich die verfügbaren Optionen für App Service-Umgebungen an.

## <a name="virtual-network-support"></a>Unterstützung für virtuelle Netzwerke ##

Eine App Service-Umgebung kann nur in einem virtuellen Azure Resource Manager-Netzwerk erstellt werden. Weitere Informationen zu virtuellen Azure Virtual-Netzwerken finden Sie unter [Azure Virtual Network – häufig gestellte Fragen](https://azure.microsoft.com/documentation/articles/virtual-networks-faq/). Da sich eine App Service-Umgebung immer in einem virtuellen Netzwerk, genauer gesagt, in einem Subnetz eines virtuellen Netzwerks befindet, können Sie die Sicherheitsfunktionen virtueller Netzwerke zum Steuern sowohl der eingehenden als auch der ausgehenden Netzwerkkommunikation für Ihre Apps nutzen.

Eine App Service-Umgebung kann entweder für Internetzugriff mit einer öffentlichen IP-Adresse oder für die ausschließliche interne Verwendung mit einer Azure ILB-Adresse (Internal Load Balancer) eingerichtet werden.

Mithilfe von [Netzwerksicherheitsgruppen][NSGs] können Sie die eingehende Netzwerkkommunikation mit dem Subnetz einschränken, das eine App Service-Umgebung enthält. Dadurch können Sie Apps hinter Upstreamgeräten und -diensten ausführen wie z. B. Web Application Firewalls und Netzwerk-SaaS-Anbietern.

Apps müssen häufig auch auf Unternehmensressourcen wie interne Datenbanken und Webdienste zugreifen. Wenn die App Service-Umgebung in einem virtuellen Azure-Netzwerk mit einer VPN-Verbindung zum lokalen Netzwerk bereitgestellt wird, können die Apps in der App Service-Umgebung auf die lokalen Ressourcen zugreifen. Dies gilt unabhängig davon, ob es sich um ein [Site-to-Site-VPN](https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/) oder ein [Azure ExpressRoute-VPN](http://azure.microsoft.com/services/expressroute/) handelt.

Weitere Informationen zur Funktionsweise der App Service-Umgebungen mit virtuellen Netzwerken und lokalen Netzwerken finden Sie unter [Netzwerküberlegungen zur App Service-Umgebung][ASENetwork].

## <a name="asev1"></a>ASEv1 ##

Es gibt zwei Versionen der App Service-Umgebung: ASEv1 und ASEv2. Schwerpunkt der vorherigen Informationen war ASEv2. In diesem Abschnitt werden die Unterschiede zwischen ASEv1 und ASEv2 aufgezeigt. 

In ASEv1 müssen Sie alle Ressourcen manuell verwalten. Dies schließt Front-Ends, Worker und IP-Adressen ein, die für IP-basiertes SSL verwendet werden. Bevor Sie Ihren App Service-Plan horizontal hochskalieren können, müssen Sie zunächst den Workerpool skalieren, den Sie zum Hosten verwenden möchten.

ASEv1 verwendet ein anderes Preismodell als ASEv2. In ASEv1 müssen Sie jeden zugewiesenen Prozessorkern bezahlen. Dazu gehören Prozessorkerne, die für Front-Ends oder Worker verwendet werden, die keine Workloads hosten. In ASEv1 beträgt die maximale Standardgröße einer App Service-Umgebung insgesamt 55 Hosts. Dazu gehören Worker und Front-Ends. ASEv1 hat den einen Vorteil, dass die Bereitstellung in einem klassischen virtuellen Netzwerk wie auch als virtuelles Ressourcen-Manager-Netzwerk möglich ist. Weitere Informationen zu ASEv1 finden Sie unter [Einführung in die App Service-Umgebung v1][ASEv1Intro].

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

