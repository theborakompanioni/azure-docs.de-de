---
title: "Einführung in Azure App Service-Umgebungen"
description: "Kurze Übersicht über Azure App Service-Umgebungen"
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
ms.translationtype: HT
ms.sourcegitcommit: 79bebd10784ec74b4800e19576cbec253acf1be7
ms.openlocfilehash: e21c4c3e2c212d86a0dbe2211564c2e3a1acf819
ms.contentlocale: de-de
ms.lasthandoff: 08/03/2017

---
# <a name="introduction-to-app-service-environments"></a>Einführung in App Service-Umgebungen #
 
## <a name="overview"></a>Übersicht ##

Eine Azure App Service-Umgebung ist ein Feature von Azure App Service, das eine vollständig isolierte und dedizierte Umgebung zur sicheren Ausführung von App Service-Apps mit umfangreicher Skalierung bereitstellt. Diese Funktion kann Ihre [Web-Apps][webapps], [mobilen Apps][mobileapps], [API-Apps][APIapps] und [Funktionen][Functions] hosten.

App Service-Umgebungen (App Service Environments, ASEs) sind ideal geeignet für Anwendungsworkloads mit folgenden Anforderungen:

- Unterstützung sehr vieler Apps
- Isolierung und sicherer Netzwerkzugriff
- Hohe Speicherauslastung

Kunden können mehrere ASEs innerhalb einer einzelnen Azure-Region oder über mehrere Azure-Regionen verteilt einrichten. Durch diese Flexibilität eignen sich ASEs hervorragend für die horizontale Skalierung zustandsloser Anwendungsebenen zur Unterstützung hoher RPS-Workloads.

Aufgrund der Isolierung werden in ASEs nur Anwendungen eines einzelnen Kunden ausgeführt. Die Umgebungen werden zudem immer in einem virtuellen Netzwerk bereitgestellt. Kunden haben präzise Kontrolle über den eingehenden und ausgehenden Netzwerkdatenverkehr der Anwendung. Anwendungen können schnelle, sichere Verbindungen über VPNs mit lokalen Unternehmensressourcen einrichten.

Alle Artikel und Anweisungen zu ASEs stehen in der [Infodatei für App Service-Umgebungen][ASEReadme] zur Verfügung:

* ASEs ermöglichen hoch skalierbares App-Hosting mit sicherem Netzwerkzugriff. Weitere Informationen finden Sie unter [AzureCon Deep Dive](https://azure.microsoft.com/documentation/videos/azurecon-2015-deploying-highly-scalable-and-secure-web-and-mobile-apps/) in ASEs.
* Für die horizontale Skalierung können mehrere ASEs verwendet werden. Weitere Informationen finden Sie unter [Vorgehensweise zum Einrichten einer geografisch verteilten App-Umgebung](https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-geo-distributed-scale/).
* ASEs können verwendet werden, um eine Sicherheitsarchitektur zu konfigurieren, wie in AzureCon Deep Dive dargestellt wird. Informationen darüber, wie die in AzureCon Deep Dive gezeigte Sicherheitsarchitektur konfiguriert wurde, finden Sie im Artikel zum [Implementieren einer Sicherheitsarchitektur mit Ebenen](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-app-service-environment-layered-security) in App Service-Umgebungen.
* Der Zugriff von Apps in ASEs kann durch Upstreamgeräte wie z.B. Web Application Firewalls (WAFs) abgegrenzt werden. Weitere Informationen finden Sie unter [Konfigurieren einer WAF für App Service-Umgebungen](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-app-service-environment-web-application-firewall).

## <a name="dedicated-environment"></a>Dedizierte Umgebung ##

Eine ASE wird dediziert für ausschließlich ein einzelnes Abonnement verwendet und kann 100 Instanzen hosten. Dabei kann es sich um 100 Instanzen in einem einzelnen App Service-Plan oder auch um 100 App Service-Einzelinstanzpläne und alles dazwischen handeln.

Eine ASE besteht aus Front-Ends und Worker. Front-Ends sind für die HTTP/HTTPS-Beendigung und den automatischen Lastenausgleich von App-Anforderungen in einer ASE zuständig. Front-Ends werden automatisch hinzugefügt, wenn die App Service-Pläne in der ASE horizontal hochskaliert werden.

Worker sind Rollen, die Kunden-Apps hosten. Worker sind in drei festen Größen verfügbar:

* Ein Kern/3,5 GB RAM
* Zwei Kerne/7 GB RAM
* Vier Kerne/14 GB RAM

Kunden müssen keine Front-Ends und Worker verwalten. Die gesamte Infrastruktur wird automatisch hinzugefügt, wenn Kunden ihre App Service-Pläne horizontal hochskalieren. Wenn App Service-Pläne erstellt oder in einer ASE skaliert werden, wird die erforderliche Infrastruktur nach Bedarf hinzugefügt bzw. entfernt.

Es gibt eine monatliche Pauschalgebühr für eine ASE, mit der die Infrastruktur abgedeckt ist. Dies ändert sich nicht mit der Größe der ASE. Darüber hinaus gibt es einen Kostenplan pro Kern-App Service-Plan. Alle in einer ASE gehosteten Apps befinden sich in der isolierten Preis-SKU. Um Informationen zu den Preisen für eine ASE zu erhalten, lesen Sie die Seite [App Service-Preise][Pricing], und überprüfen Sie die verfügbaren Optionen für ASEs.

## <a name="virtual-network-support"></a>Unterstützung für virtuelle Netzwerke ##

Eine ASE kann nur in einem virtuellen Azure Resource Manager-Netzwerk erstellt werden. Weitere Informationen zu virtuellen Azure-Netzwerken finden Sie unter [Virtuelle Azure-Netzwerke – FAQs](https://azure.microsoft.com/documentation/articles/virtual-networks-faq/). Eine ASE befindet sich immer in einem virtuellen Netzwerk, genauer gesagt, in einem Subnetz eines virtuellen Netzwerks. Mithilfe der Sicherheitsfunktionen virtueller Netzwerke können Sie ein- und ausgehende Netzwerkkommunikation für Ihre Apps steuern.

Eine ASE kann entweder für Internetzugriff mit einer öffentlichen IP-Adresse oder für die ausschließliche interne Verwendung mit einer Azure-ILB-Adresse (Internal Load Balancer) eingerichtet werden.

Mithilfe von [Netzwerksicherheitsgruppen][NSGs] können Sie die eingehende Netzwerkkommunikation mit dem Subnetz einschränken, das eine ASE enthält. Durch NSGs können Sie Apps hinter Upstreamgeräten und -diensten ausführen, wie WAFs und Netzwerk-SaaS-Anbietern.

Apps müssen häufig auch auf Unternehmensressourcen wie interne Datenbanken und Webdienste zugreifen. Wenn Sie die ASE in einem virtuellen Netzwerk bereitstellen, das über eine VPN-Verbindung mit dem lokalen Netzwerk verfügt, können die Apps in der ASE auf die lokalen Ressourcen zugreifen. Diese Funktion besteht unabhängig davon, ob es sich um ein [Site-to-Site-VPN](https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/) oder ein [Azure ExpressRoute-VPN](http://azure.microsoft.com/services/expressroute/) handelt.

Weitere Informationen zur Funktionsweise von ASEs mit virtuellen Netzwerken und lokalen Netzwerken finden Sie unter [Überlegungen zu Netzwerken mit einer App Service-Umgebung][ASENetwork].

## <a name="app-service-environment-v1"></a>App Service-Umgebung v1 ##

Es gibt zwei Versionen der App Service-Umgebung: ASEv1 und ASEv2. Die oben aufgeführten Informationen basieren auf ASEv2. In diesem Abschnitt werden die Unterschiede zwischen ASEv1 und ASEv2 aufgezeigt. 

In ASEv1 müssen Sie alle Ressourcen manuell verwalten. Dies schließt Front-Ends, Worker und IP-Adressen ein, die für IP-basiertes SSL verwendet werden. Bevor Sie Ihren App Service-Plan horizontal hochskalieren können, müssen Sie zunächst den Workerpool horizontal hochskalieren, den Sie zum Hosten verwenden möchten.

ASEv1 verwendet ein anderes Preismodell als ASEv2. In ASEv1 bezahlen Sie jeden zugewiesenen Prozessorkern. Dazu gehören Prozessorkerne für Front-Ends oder Worker, die keine Workloads hosten. In ASEv1 beträgt die maximale Standardskalierungsgröße einer ASE insgesamt 55 Hosts. Dazu gehören Worker und Front-Ends. ASEv1 hat den Vorteil, dass die Bereitstellung in einem klassischen virtuellen Netzwerk sowie in einem virtuellen Resource Manager-Netzwerk möglich ist. Weitere Informationen zu ASEv1 finden Sie unter [Einführung in die App Service-Umgebung v1][ASEv1Intro].

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

