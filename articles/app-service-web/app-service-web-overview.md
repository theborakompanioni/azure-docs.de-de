<properties
	pageTitle="Web-Apps – Übersicht | Microsoft Azure"
	description="Es wird beschrieben, wie Azure App Service Sie beim Entwickeln und Hosten von Webanwendungen unterstützt."
	services="app-service\web"
	documentationCenter=""
	authors="jaime-espinosa"
	manager="wpickett"
	editor=""/>

<tags
	ms.service="app-service-web"
	ms.workload="web"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.date="05/25/2016"
	ms.author="tdykstra"/>

# Web-Apps – Übersicht

Bei *App Service-Web-Apps* handelt es sich um eine vollständig verwaltete Computeplattform, die für das Hosten von Websites und Webanwendungen optimiert ist. Mit diesem PaaS-Angebot ([Platform as a Service](https://en.wikipedia.org/wiki/Platform_as_a_service)) von Microsoft Azure können Sie sich auf Ihre Geschäftslogik konzentrieren, während Azure sich um die Infrastruktur zum Ausführen und Skalieren Ihrer Apps kümmert.

Ein fünfminütiges Video mit einer Übersicht finden Sie unter [Azure App Service Web Apps with Yochay Kiriaty](https://azure.microsoft.com/documentation/videos/azure-app-service-web-apps-with-yochay-kiriaty/) (Azure App Service-Web-Apps mit Yochay Kiriaty).

## Was ist eine Web-App in App Service?

In App Service handelt es sich bei einer *Web-App* um die Computeressourcen, die von Azure zum Hosten einer Website oder einer Webanwendung bereitgestellt werden.

Die Computeressourcen können sich je nach gewähltem Tarif auf freigegebenen oder dedizierten virtuellen Computern (VMs) befinden. Ihr Anwendungscode wird auf einer verwalteten VM ausgeführt, die von anderen Kunden isoliert ist.

Für Ihren Code kann jede Sprache und jedes Framework verwendet werden, die bzw. das von [Azure App Service](../app-service/app-service-value-prop-what-is.md) unterstützt wird, z.B. ASP.NET, Node.js, Java, PHP oder Python. Sie können auch Skripts ausführen, bei denen [PowerShell und andere Skriptsprachen](web-sites-create-web-jobs.md#acceptablefiles) in einer Web-App verwendet werden.

Beispiele für typische Anwendungsszenarien, für die Sie Web-Apps verwenden können, finden Sie unter [Szenarien mit Web-Apps](https://azure.microsoft.com/documentation/scenarios/web-app/).

## Gründe für die Verwendung von Web-Apps

Hier sind einige wichtige Features von App Service aufgeführt, die für Web-Apps gelten:

- **Vollständig verwaltete Plattform**: Automatisches Betriebssystem- und Framework-Patching, integrierter Support für die Sicherung und Notfallwiederherstellung. 

- **Nutzen Sie Ihre vorhandenen Kenntnisse:** Arbeiten Sie in Ihrer bevorzugten Programmiersprache und Framework- und Entwicklungsumgebung. App Service unterstützt .NET, Node.js, Java, PHP und Python.

- **Schnelle Bereitstellung:** Stellen Sie neue Apps und den Code dafür innerhalb weniger Sekunden bereit.

- **Continuous Integration**: Richten Sie mit Visual Studio Team Services, GitHub oder BitBucket [Continuous Integration und die fortlaufende Bereitstellung](../app-service-web/app-service-continous-deployment.md) ein.

- **Staging- und Testumgebungen**: Implementieren Sie die [Stagingbereitstellung](../app-service-web/web-sites-staged-publishing.md), um den Code in einer Präproduktionsumgebung zu überprüfen, die mit Ihrer Produktionsumgebung identisch ist. Wenn alles bereit ist, geben Sie durch einen Ersetzungsvorgang eine neue Version der App ohne Ausfallzeiten frei.

- **Test in der Produktion**: Heben Sie Stagingbereitstellungen auf die nächste Stufe, und führen Sie [A/B-Tests](../app-service-web/app-service-web-test-in-production-get-start.md) durch, um Ihren neuen Code mit einem konfigurierbaren Bruchteil Ihres aktiven Datenverkehrs zu überprüfen.

- **Authentifizierung und Autorisierung**: Schützen Sie eine App vor nicht authentifiziertem Zugriff, ohne dafür Ihren Code ändern zu müssen. Mit integrierten Authentifizierungsdiensten wird der Zugriff durch Benutzer, Benutzer repräsentierende Clients oder Dienste für Ihre Apps geschützt. Folgende Identitätsanbieter werden unterstützt: Azure Active Directory, Facebook, Twitter, Google und Microsoft-Konto. Weitere Informationen finden Sie unter [Authentifizierung und Autorisierung in Azure App Service](../app-service/app-service-authentication-overview.md).

- **Herstellen einer Verbindung mit beliebigen Diensten:** Verbinden Sie Ihre App mit integrierten [Connectors](../connectors/apis-list.md) in wenigen Minuten mit Unternehmenssystemen oder SaaS-Plattformen (Software as a Service). Wählen Sie aus über 50 Connectors für Unternehmenssysteme wie SAP, Siebel und Oracle, gängige SaaS-Dienste für Unternehmen, darunter Salesforce und Office 365, sowie gängige Internetdienste wie Facebook, Twitter und Dropbox.

- **Globale Skalierbarkeit:** Skalieren Sie [zentral hoch](../app-service/app-service-scale.md) oder [horizontal hoch](../azure-portal/insights-how-to-scale.md), um eingehende Datenlasten von Kunden zu verarbeiten. Wählen Sie Anzahl und Größe der virtuellen Computer manuell aus, oder richten Sie die automatische Skalierung basierend auf Last oder Zeitplan ein. Apps werden in der globalen Rechenzentrum-Infrastruktur von Microsoft gehostet und vereinfachen die Replizierung von Daten- und Hostingdiensten an mehreren Standorten.

- **Für Unternehmen konzipiert**: App Service ist auf das Entwickeln und Hosten sicherer, unternehmenskritischer Anwendungen ausgelegt. Entwickeln Sie in Active Directory integrierte Unternehmens-Apps, die sich sicher mit lokalen Ressourcen verbinden. Hosten Sie sie anschließend auf einer sicheren Cloudplattform, die die Anforderungen von [ISO, SOC und PCI](https://www.microsoft.com/TrustCenter/) erfüllt. All dies ist mit einer [Vereinbarung zum Servicelevel](https://azure.microsoft.com/support/legal/sla/app-service/) (SLA) für Unternehmen möglich.

- **Azure Marketplace**: Sie können eine Auswahl aus einer ständig wachsenden [Liste mit Anwendungsvorlagen](https://azure.microsoft.com/marketplace/) treffen. Nutzen Sie das Beste aus der OSS-App-Community mit 1-Klick-Installation von Paketen wie WordPress, Joomla und Drupal.

- **WebJobs**: [Führen Sie Programme oder Skripts auf App Service-VMs aus](../app-service-web/web-sites-create-web-jobs.md). Führen Sie Aufträge fortlaufend, nach einem Zeitplan oder per Auslösung durch Ereignisse aus. Das Azure [WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk.md) vereinfacht den Code, den Sie für die Integration in andere Azure- und Drittanbieterdienste schreiben.

- **Hybridverbindungen**: Zugriff auf lokale Daten mit [Hybridverbindungen](../biztalk-services/integration-hybrid-connection-overview.md) und [Azure Virtual Networks](../app-service-web/web-sites-integrate-with-vnet.md).

- **Visual Studio-Integration**: Dedizierte Tools in Visual Studio optimieren das Erstellen, Bereitstellen, Nutzen, Debuggen und Verwalten von Web-Apps, mobilen Apps und API-Apps.

Außerdem können für eine Web-App die Features verwendet werden, die von [API-Apps](../app-service-api/app-service-api-apps-why-best-platform.md) (z.B. CORS-Unterstützung) und [mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) (z.B. Pushbenachrichtigungen) bereitgestellt werden. Die einzigen Unterschiede zwischen diesen drei App-Typen (API, Web, mobil) sind der Name und das Symbol im Azure-Portal. Weitere Informationen zu App-Typen in App Service finden Sie unter [Azure App Service-Pläne – Detaillierte Übersicht](../app-service/app-service-value-prop-what-is.md).

Neben Web-Apps in App Service bietet Azure noch andere Dienste an, die zum Hosten von Websites und Webanwendungen verwendet werden können. In den meisten Fällen sind Web-Apps die beste Wahl. Erwägen Sie für eine Microservice-Architektur die Verwendung von [Service Fabric](https://azure.microsoft.com/documentation/services/service-fabric). Wenn Sie eine bessere Kontrolle über die VMs benötigen, auf denen Ihr Code ausgeführt wird, können Sie [Azure Virtual Machines](https://azure.microsoft.com/documentation/services/virtual-machines/) einsetzen. Weitere Informationen zur Wahl zwischen diesen Azure-Diensten finden Sie unter [Azure App Service, Virtual Machines, Service Fabric und Cloud Services – Vergleich](choose-web-site-cloud-service-vm.md).

## Erste Schritte

Führen Sie die Schritte im Tutorial [Bereitstellen Ihrer ersten Web-App für Azure in fünf Minuten](app-service-web-get-started.md) aus, um zu beginnen, indem Sie Beispielcode für eine neue Web-App in App Service bereitstellen. Sie benötigen ein kostenloses Azure-Konto.

Wenn Sie Azure App Service ausprobieren möchten, ehe Sie sich für ein Azure-Konto anmelden, können Sie unter [App Service testen](http://go.microsoft.com/fwlink/?LinkId=523751) sofort kostenlos eine kurzlebige Starter-Web-App in App Service erstellen. Keine Kreditkarte erforderlich, keine Verpflichtungen.

<!---HONumber=AcomDC_0601_2016-->