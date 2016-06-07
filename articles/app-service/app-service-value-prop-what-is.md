<properties 
	pageTitle="Was ist Azure App Service? | Microsoft Azure" 
	description="Es wird beschrieben, wie Sie Azure App Service verwenden, um Web-Apps und mobile Apps zu entwickeln, bereitzustellen und zu verwalten." 
	keywords="App Service, Azure App Service, App Service-Kosten, Skalierung, skalierbar, App-Bereitstellung, Azure-App-Bereitstellung, PaaS, Platform as a Service"
	services="app-service" 
	documentationCenter="" 
	authors="omarkmsft" 
	manager="dwrede" 
	editor="jimbe"/>

<tags 
	ms.service="app-service" 
	ms.workload="na" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="get-started-article" 
	ms.date="05/25/2016" 
	ms.author="omark"/>

# Was ist Azure App Service?

*App Service* ist ein [PaaS-Angebot (Platform as a Service)](https://en.wikipedia.org/wiki/Platform_as_a_service) von Microsoft Azure, mit dem Sie Web-Apps und mobile Apps für alle Plattformen und Geräte erstellen können. Sie können Ihre Apps leicht in SaaS-Lösungen integrieren (z.B. Office 365, Dynamics CRM, Salesforce, Twilio), leicht eine Verbindung mit lokalen Anwendungen herstellen (z.B. SAP, Oracle, Siebel) und leicht Geschäftsprozesse automatisieren, während gleichzeitig strenge Sicherheitsanforderungen erfüllt werden und für Zuverlässigkeit und Skalierbarkeit gesorgt ist.

App Service umfasst die Webfunktionen und mobilen Funktionen, die wir vorher separat als Azure Websites und Azure Mobile Services bereitgestellt haben. Außerdem sind neue Funktionen zum Automatisieren von Geschäftsprozessen und Hosten von Cloud-APIs enthalten.

## App-Typen in App Service

App Service verfügt über die folgenden App-Typen zum Ausführen Ihres Anwendungscodes bzw. Ihrer Workflowprozesse.

- [**Web-Apps**](../app-service-web/app-service-web-overview.md): Hosten von Websites und Webanwendungen

- [**Mobile Apps**](../app-service-mobile/app-service-mobile-value-prop.md): Hosten von Back-Ends für mobile Apps
   
- [**API-Apps**](../app-service-api/app-service-api-apps-why-best-platform.md): Hosten von Cloud-APIs
 
- [**Logik-Apps**](../app-service-logic/app-service-logic-what-are-logic-apps.md): Automatisieren des Zugriffs und der cloudübergreifenden Nutzung von Daten, ohne Code schreiben zu müssen

Als einzelner integrierter Dienst erleichtert App Service die Zusammenfassung mehrerer App-Typen zu einer Gesamtlösung.

## App Service-Pläne und -Umgebungen

Mit [App Service-Plänen](azure-web-sites-web-hosting-plans-in-depth-overview.md) werden die Computeressourcen dargestellt, auf denen Ihre Apps ausgeführt werden. Bei niedrigeren Tarifen werden Ihre Apps auf freigegebenen virtuellen Computern (VMs) ausgeführt. Bei höheren Tarifen werden Ihre Apps auf dedizierten VMs ausgeführt. Sie können zwischen mehreren VM-Größen wählen, und Sie können Ihren Tarif ändern, ohne dass es zu Ausfallzeiten kommt. Wenn Sie eine bessere Skalierbarkeit und Netzwerkisolation benötigen, können Sie die Apps in einer [App Service-Umgebung](../app-service-web/app-service-app-service-environment-intro.md) ausführen.

## Gründe für die Verwendung von App Service

Hier sind einige wichtige Features und Funktionen von App Service aufgeführt:

- **Vollständig verwaltete Plattform**: Automatisches Betriebssystem- und Framework-Patching, integrierter Support für die Sicherung und Notfallwiederherstellung. 

- **Nutzen Sie Ihre vorhandenen Kenntnisse:** Arbeiten Sie in Ihrer bevorzugten Programmiersprache und Framework- und Entwicklungsumgebung. App Service unterstützt .NET, Node.js, Java, PHP und Python.

- **Schnelle Bereitstellung:** Stellen Sie neue Apps und den Code dafür innerhalb weniger Sekunden bereit.

- **Continuous Integration**: Richten Sie mit Visual Studio Team Services, GitHub oder BitBucket [Continuous Integration und die fortlaufende Bereitstellung](../app-service-web/app-service-continous-deployment.md) ein.

- **Staging- und Testumgebungen**: Implementieren Sie die [Stagingbereitstellung](../app-service-web/web-sites-staged-publishing.md), um den Code in einer Präproduktionsumgebung zu überprüfen, die mit Ihrer Produktionsumgebung identisch ist. Wenn alles bereit ist, geben Sie durch einen Ersetzungsvorgang eine neue Version der App ohne Ausfallzeiten frei.

- **Test in der Produktion**: Heben Sie Stagingbereitstellungen auf die nächste Stufe, und führen Sie [A/B-Tests](../app-service-web/app-service-web-test-in-production-get-start.md) durch, um Ihren neuen Code mit einem konfigurierbaren Bruchteil Ihres aktiven Datenverkehrs zu überprüfen.

- **Authentifizierung und Autorisierung**: Schützen Sie eine App vor nicht authentifiziertem Zugriff, ohne dafür Ihren Code ändern zu müssen. Mit integrierten Authentifizierungsdiensten wird der Zugriff durch Benutzer, Benutzer repräsentierende Clients oder Dienste für Ihre Apps geschützt. Folgende Identitätsanbieter werden unterstützt: Azure Active Directory, Facebook, Twitter, Google und Microsoft-Konto. Weitere Informationen finden Sie unter [Authentifizierung und Autorisierung in Azure App Service](app-service-authentication-overview.md).

- **Herstellen einer Verbindung mit beliebigen Diensten:** Verbinden Sie Ihre App mit integrierten [Connectors](../connectors/apis-list.md) in wenigen Minuten mit Unternehmenssystemen oder SaaS-Plattformen (Software as a Service). Wählen Sie aus über 50 Connectors für Unternehmenssysteme wie SAP, Siebel und Oracle, gängige SaaS-Dienste für Unternehmen, darunter Salesforce und Office 365, sowie gängige Internetdienste wie Facebook, Twitter und Dropbox.

- **Globale Skalierbarkeit:** Skalieren Sie [zentral hoch](../app-service/app-service-scale.md) oder [horizontal hoch](../azure-portal/insights-how-to-scale.md), um eingehende Datenlasten von Kunden zu verarbeiten. Wählen Sie Anzahl und Größe der virtuellen Computer manuell aus, oder richten Sie die automatische Skalierung basierend auf Last oder Zeitplan ein. Apps werden in der globalen Rechenzentrum-Infrastruktur von Microsoft gehostet und vereinfachen die Replizierung von Daten- und Hostingdiensten an mehreren Standorten.

- **Für Unternehmen konzipiert**: App Service ist auf das Entwickeln und Hosten sicherer, unternehmenskritischer Anwendungen ausgelegt. Entwickeln Sie in Active Directory integrierte Unternehmens-Apps, die sich sicher mit lokalen Ressourcen verbinden. Hosten Sie sie anschließend auf einer sicheren Cloudplattform, die die Anforderungen von [ISO, SOC und PCI](https://www.microsoft.com/TrustCenter/) erfüllt. All dies ist mit einer [Vereinbarung zum Servicelevel](https://azure.microsoft.com/support/legal/sla/app-service/) (SLA) für Unternehmen möglich.

- **Azure Marketplace**: Sie können eine Auswahl aus einer ständig wachsenden [Liste mit Anwendungsvorlagen](https://azure.microsoft.com/marketplace/) treffen. Nutzen Sie das Beste aus der OSS-App-Community mit 1-Klick-Installation von Paketen wie WordPress, Joomla und Drupal.

- **WebJobs**: [Führen Sie Programme oder Skripts auf App Service-VMs aus](../app-service-web/web-sites-create-web-jobs.md). Führen Sie Aufträge fortlaufend, nach einem Zeitplan oder per Auslösung durch Ereignisse aus. Das Azure [WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk.md) vereinfacht den Code, den Sie für die Integration in andere Azure- und Drittanbieterdienste schreiben.

- **Hybridverbindungen**: Zugriff auf lokale Daten mit [Hybridverbindungen](../biztalk-services/integration-hybrid-connection-overview.md) und [Azure Virtual Networks](../app-service-web/web-sites-integrate-with-vnet.md).

- **Visual Studio-Integration**: Dedizierte Tools in Visual Studio optimieren das Erstellen, Bereitstellen, Nutzen, Debuggen und Verwalten von Web-Apps, mobilen Apps und API-Apps.

## Erste Schritte mit App Service

[Hier können Sie schnell und kostenlos eine temporäre Web-App, mobile App oder Logik-App erstellen](http://go.microsoft.com/fwlink/?LinkId=523751) – keine Kreditkarte erforderlich, keine Verpflichtungen, kein Ärger.

Oder eröffnen Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/pricing/free-trial/), und probieren Sie eines unserer Tutorials für die ersten Schritte aus:

* [Web-Apps](https://azure.microsoft.com/documentation/services/app-service/web/)
* [Mobile Apps](https://azure.microsoft.com/documentation/services/app-service/mobile/)
* [API-Apps](https://azure.microsoft.com/documentation/services/app-service/api/)
* [Logik-Apps](https://azure.microsoft.com/documentation/services/app-service/logic/)

<!---HONumber=AcomDC_0601_2016-->