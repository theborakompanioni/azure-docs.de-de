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
	ms.date="05/16/2016"
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

Hier sind einige wichtige Funktionen von Web-Apps aufgeführt:

- **Vertraut und schnell**: Nutzen Sie Ihre vorhandenen Kenntnisse, und arbeiten Sie in Ihrer bevorzugten Programmiersprache, Frameworkumgebung und integrierten Entwicklungsumgebung (IDE). Stellen Sie neue Web-Apps und den Code dafür innerhalb weniger Sekunden bereit.

- **Für Unternehmen konzipiert**: Web-Apps sind auf das Entwickeln und Hosten sicherer, unternehmenskritischer Anwendungen ausgelegt. Entwickeln Sie in Active Directory integrierte Unternehmens-Apps, die sich sicher mit lokalen Ressourcen verbinden. Hosten Sie sie anschließend auf einer sicheren Cloudplattform, die die Anforderungen von [ISO](https://www.microsoft.com/TrustCenter/Compliance/ISO-IEC-27001), [SOC](https://www.microsoft.com/TrustCenter/Compliance/SOC) und [PCI](https://www.microsoft.com/TrustCenter/Compliance/pci) erfüllt. All dies ist mit einer [Vereinbarung zum Servicelevel](https://azure.microsoft.com/support/legal/sla/app-service/) für Unternehmen möglich.

- **Globale Skalierbarkeit**: Web-Apps ermöglichen Ihnen das schnelle [zentrale Hochskalieren](../app-service/app-service-scale.md) oder [horizontale Hochskalieren](../azure-portal/insights-how-to-scale.md), um alle eingehenden Kundenanforderungen zu bewältigen. Wählen Sie Anzahl und Größe der virtuellen Computer manuell aus, oder richten Sie die automatische Skalierung basierend auf Last oder Zeitplan ein. Web-Apps werden in der globalen Rechenzentrum-Infrastruktur von Microsoft gehostet und vereinfachen die Replizierung von Daten- und Hostingdiensten an mehreren Standorten.

- **Azure Marketplace**: Sie können eine Auswahl aus einer ständig wachsenden [Liste mit Webanwendungsvorlagen](https://azure.microsoft.com/marketplace/) treffen. Nutzen Sie das Beste aus der OSS-App-Community mit 1-Klick-Installation von Paketen wie z. B. Wordpress, Joomla und Drupal.

- **Continuous Integration**: Richten Sie mit Visual Studio Team Services, GitHub oder BitBucket Workflows für [Continuous Integration und die fortlaufende Bereitstellung](app-service-continous-deployment.md) ein. Sie können Ihre Web-App bei jedem erfolgreichen Eincheckvorgang für Code oder bei Integrationstests automatisch erstellen, testen und bereitstellen lassen.

- **Staging- und Testumgebungen**: Implementieren Sie die [Stagingbereitstellung](web-sites-staged-publishing.md), um den Code in einer Präproduktionsumgebung zu überprüfen, die mit Ihrer Produktionsumgebung identisch ist. Wenn alles bereit ist, geben Sie durch einen Ersetzungsvorgang eine neue Version der App ohne Ausfallzeiten frei.

- **Test in der Produktion**: Heben Sie Stagingbereitstellungen auf die nächste Stufe, und führen Sie [A/B-Tests](app-service-web-test-in-production-get-start.md) durch, um Ihren neuen Code mit einem konfigurierbaren Bruchteil Ihres aktiven Datenverkehrs zu überprüfen.

- **WebJobs**: [Führen Sie Programme oder Skripts auf virtuellen Web-Apps-Computern aus](web-sites-create-web-jobs.md). Führen Sie Aufträge fortlaufend, nach einem Zeitplan oder per Auslösung durch Ereignisse aus. Das Azure [WebJobs SDK](websites-dotnet-webjobs-sdk-get-started.md) vereinfacht für den Code, den Sie schreiben, die Integration in andere Azure-Dienste, z.B. Speicherwarteschlangen, -blobs und -tabellen sowie Service Bus-Warteschlangen und -Themen.

- **Hybridverbindungen**: Zugriff auf lokale Daten mit [Hybridverbindungen](../biztalk-services/integration-hybrid-connection-overview.md) und [VNET](../app-service-web/web-sites-integrate-with-vnet.md).

- **Visual Studio-Integration**: Dedizierte Tools in Visual Studio optimieren das Erstellen, Bereitstellen, Nutzen, Debuggen und Verwalten von Web-Apps. Weitere Informationen finden Sie unter [Announcing the Azure SDK 2.8.1 for .NET](https://azure.microsoft.com/blog/announcing-azure-sdk-2-8-1-for-net/) (in englischer Sprache).

Außerdem können für eine Web-App die Features verwendet werden, die von [API-Apps](../app-service-api/app-service-api-apps-why-best-platform.md) (z.B. CORS-Unterstützung) und [mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) (z.B. Pushbenachrichtigungen) bereitgestellt werden. Umgekehrt ist dies auch möglich: Sie können eine API-App oder mobile App verwenden, um eine Webanwendung zu hosten und die Web-Apps-Features wie automatische Skalierung und Stagingbereitstellung zu nutzen. Die einzigen Unterschiede zwischen diesen drei App-Typen (API, Web, mobil) sind der Name und das Symbol im Azure-Portal. Weitere Informationen zu App-Typen in App Service finden Sie unter [Azure App Service-Pläne – Detaillierte Übersicht](../app-service/app-service-value-prop-what-is.md).

## Erste Schritte

Führen Sie die Schritte im Tutorial [Bereitstellen Ihrer ersten Web-App für Azure in fünf Minuten](app-service-web-get-started.md) aus, um zu beginnen, indem Sie Beispielcode für eine neue Web-App in App Service bereitstellen. Sie benötigen ein kostenloses Azure-Konto.

Wenn Sie Azure App Service ausprobieren möchten, ehe Sie sich für ein Azure-Konto anmelden, können Sie unter [App Service testen](http://go.microsoft.com/fwlink/?LinkId=523751) sofort kostenlos eine kurzlebige Starter-Web-App in App Service erstellen. Keine Kreditkarte erforderlich, keine Verpflichtungen.

<!---HONumber=AcomDC_0525_2016-->