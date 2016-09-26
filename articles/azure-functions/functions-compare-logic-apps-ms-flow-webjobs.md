<properties
	pageTitle="Auswahl zwischen Flow, Logic Apps, Functions und WebJobs | Microsoft Azure"
	description="Anhand eines Vergleichs und einer Gegenüberstellung der vier Cloudintegrationsdienste von Microsoft können Sie ermitteln, welche am besten für Sie geeignet sind."
	services="functions,app-service\logic"
	documentationCenter="na"
	authors="cephalin"
	manager="wpickett"
	tags=""
	keywords="Microsoft Flow, Flow, Logic Apps, Azure Functions, Functions, Azure WebJobs, WebJobs, Ereignisverarbeitung, dynamisches Compute, serverlose Architektur"/>

<tags
	ms.service="functions"
	ms.devlang="multiple"
	ms.topic="article"
	ms.tgt_pltfrm="multiple"
	ms.workload="na"
	ms.date="09/08/2016"
	ms.author="chrande; glenga"/>

# Auswahl zwischen Flow, Logic Apps, Functions und WebJobs

In diesem Artikel werden die folgenden Dienste in der Microsoft Cloud verglichen und gegenübergestellt, die alle zur Behebung von Integrationsproblemen und zur Automatisierung von Geschäftsprozessen dienen:

- [Microsoft Flow](https://flow.microsoft.com/)
- [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/)
- [Azure-Funktionen](https://azure.microsoft.com/services/functions/)
- [WebJobs in Azure App Service](../app-service-web/web-sites-create-web-jobs.md)

Alle diese Dienste sind nützlich, wenn verschiedenartige Systeme miteinander verbunden werden. Jeder der Dienste kann Eingaben, Aktionen, Bedingungen und Ausgaben definieren. Sie können die Dienste individuell nach einem Zeitplan oder auslöserbasiert ausführen. Jeder Dienst fügt jedoch einen eindeutigen Satz von Werten hinzu, und der Vergleich basiert nicht auf der Frage „Welcher Dienst ist der beste?“ sondern „Welcher Dienst ist für diese Situation am besten geeignet?“. Eine Kombination dieser Dienste ist häufig die beste Möglichkeit, um schnell eine skalierbare Integrationslösung mit vollem Funktionsumfang zu erstellen.

<a name="flow"></a>
## Flow im Vergleich zu Logik-Apps

Microsoft Flow und Azure Logic Apps können gemeinsam beschrieben werden, da es sich bei beiden um Integrationsdienste mit *Vorabkonfiguration* handelt. Dies erleichtert das Erstellen von Prozessen und Workflows sowie die Integration verschiedener SaaS- und Unternehmensanwendungen.

- Flow baut auf Logic Apps auf
- Sie haben den gleichen Workflow-Designer
- [Connectors](../connectors/apis-list.md) sind immer mit beiden Diensten kompatibel

Flow ermöglicht allen Mitarbeitern im Büro das Ausführen einfacher Integrationen (z. B. SMS für wichtige E-Mails) ohne Umweg über Entwickler oder IT. Auf der anderen Seite ermöglicht Logic Apps erweiterte oder unternehmenskritische Integrationen (z. B. B2B-Prozesse), die DevOps und Sicherheitsvorkehrungen auf Unternehmensebene erfordern. In der Regel werden Geschäftsworkflows im Laufe der Zeit immer komplexer. Entsprechend können Sie zunächst mit einem Datenfluss beginnen und diesen dann nach Bedarf in eine Logik-App konvertieren.

Anhand der folgenden Tabelle können Sie ermitteln, ob Flow oder Logic Apps für eine bestimmte Integration am besten geeignet ist.

| | Flow | Logik-Apps |
|---------------|----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| Zielgruppe | Büroangestellte, geschäftliche Benutzer | IT-Experten, Entwickler |
| Szenarios | Self-Service | Unternehmenskritisch |
| Designtool | Im Browser, nur über die Benutzeroberfläche | Im Browser und [Visual Studio](../app-service/logic/app-service-logic-deploy-from-vs.md), [Codeansicht](../app-service-logic/app-service-logic-author-definitions.md) verfügbar |
| DevOps | Ad-hoc-Entwicklung in der Produktion | Quellcodeverwaltung, Tests, Support, Automatisierung und Verwaltung in [Azure Resource Management](../app-service-logic/app-service-logic-arm-provision.md)|
| Administratorerfahrung| [https://flow.microsoft.com](https://flow.microsoft.com) | [https://portal.azure.com](https://portal.azure.com) |
| Sicherheit | Standardmethoden: [Datensouveränität](https://wikipedia.org/wiki/Technological_Sovereignty), [Verschlüsselung ruhender Daten](https://wikipedia.org/wiki/Data_at_rest#Encryption) für vertrauliche Daten usw. | Sicherheitsgarantie von Azure: [Azure Security](https://www.microsoft.com/trustcenter/Security/AzureSecurity), [Security Center](https://azure.microsoft.com/services/security-center/), [Überwachungsprotokolle](https://azure.microsoft.com/blog/azure-audit-logs-ux-refresh/) und vieles mehr. |

<a name="function"></a>
## Functions im Vergleich zu WebJobs

Azure Functions und WebJobs in Azure App Service können gemeinsam beschrieben werden, da es sich bei beiden um Integrationsdienste mit *Vorabcodierung* handelt, die für Entwickler entworfen wurden. Sie können damit ein Skript oder eine Codepassage als Reaktion auf verschiedene Ereignisse ausführen, z. B. [neue Speicherblobs](functions-bindings-storage.md) oder [eine Webhookanforderung](functions-bindings-http-webhook.md). Ähnlichkeiten zwischen den Diensten:

- Beide basieren auf [Azure App Service](../app-service/app-service-value-prop-what-is.md) verfügen über Funktionen wie[Quellcodeverwaltung](../app-service-web/app-service-continuous-deployment.md), [Authentifizierung](../app-service/app-service-authentication-overview.md) und [Überwachung](../app-service-web/web-sites-monitor.md).
- Beide sind entwicklerorientiert.
- Beide unterstützen standardmäßige Skript- und Programmiersprachen.
- Beide verfügen über NuGet- und NPM-Unterstützung.

Functions ist die natürliche Weiterentwicklung von WebJobs, wobei die größten Vorteile von WebJobs übernommen und verbessert werden. Die Verbesserungen umfassen:

- Optimierte Entwicklung, Tests und Codeausführung direkt im Browser.
- Standardmäßige Integration in mehr Azure-Dienste und Drittanbieterdienste wie [GitHub WebHooks](https://developer.github.com/webhooks/creating/).
- Nutzungsbasierte Bezahlung; es fallen keine Kosten für einen [App Service-Plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) an.
- Automatische, [dynamische Skalierung](functions-scale.md).
- Für bestehende Kunden von App Service ist die Ausführung nach einem App Service-Plan weiterhin möglich (zur Nutzung von nicht ausgelasteten Ressourcen).
- Integration in Logic Apps.

In der folgenden Tabelle werden die Unterschiede zwischen Functions und WebJobs erläutert:

| | Functions | WebJobs |
|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------|
| Skalieren | Skalierung ohne Konfiguration | Skalierung mit App Service-Plan |
| Preise | Nutzungsbasierte Bezahlung oder als Teil eines App Service-Plans | Teil eines App Service-Plans |
| Ausführungstyp | ausgelöst, geplant (durch Trigger mit Timer) | ausgelöst, fortlaufend, geplant |
| Auslösende Ereignisse | [Timer](functions-bindings-timer.md), [Azure DocumentDB](functions-bindings-documentdb.md), [Azure Event Hubs](functions-bindings-event-hubs), [HTTP/WebHook (GitHub, Slack)](functions-bindings-http-webhook.md), [Azure App Service Mobile-Apps](functions-bindings-mobile-apps.md), [Azure Notification Hubs](functions-bindings-notification-hubs.md), [Azure Service Bus](functions-bindings-service-bus.md), [Azure Storage](articles/functions-bindings-storage.md) | [Azure Storage](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md), [Azure Service Bus](websites-dotnet-webjobs-sdk-service-bus.md) |
| Entwicklung im Browser | x | |
| Windows-Skripterstellung | experimentell | x |
| PowerShell | experimentell | x |
| C# | x | x |
| F# | x | |
| Bash | experimentell | x |
| PHP | experimentell | x |
| Python | experimentell | x |
| JavaScript | x | x |
| Java | experimentell | x |

Ob Sie Functions oder WebJobs verwenden sollten, hängt letztendlich davon ab, wofür Sie App Service bereits nutzen. Wenn Sie eine App Service-App haben, für die Sie Codeausschnitte ausführen und diese gemeinsam in der gleichen DevOps-Umgebung verwalten möchten, sollten Sie WebJobs verwenden. Wenn Sie Codeausschnitte für andere Azure-Dienste oder sogar Drittanbieter-Apps ausführen, Codeausschnitte unabhängig von Ihren App Service-Apps verwalten oder Ihre Codeausschnitte von einer Logik-App aufrufen möchten, sollten Sie alle Verbesserungen in Functions nutzen.

<a name="together"></a>
## Kombination aus Flow, Logic Apps und Functions

Wie bereits erwähnt hängt es von der jeweiligen Situation ab, welcher Dienst für Sie am besten geeignet ist.

- Für eine einfache Geschäftsoptimierung verwenden Sie dann Flow.
- Wenn Ihr Integrationsszenario zu komplex für Flow ist, oder wenn Sie DevOps-Funktionen und -Sicherheitskonformität benötigen, dann verwenden Sie Logic Apps.
- Wenn ein Schritt in Ihrem Integrationsszenario eine stark benutzerdefinierte Transformation oder speziellen Code erfordert, schreiben Sie eine Funktionen-App und lösen dann eine Funktion als Aktion in Ihrer Logik-App aus.

Sie können eine Logik-App in einem Fluss aufrufen. Sie können auch eine Funktion in einer Logik-App und eine Logik-App in einer Funktion aufrufen. Die Integration zwischen Flow, Logic Apps und Functions wird im Laufe der Zeit weiter verbessert. Sie können etwas in einem Dienst erstellen und in den anderen Diensten verwenden. Daher lohnt sich jede Investition in diese drei Technologien.

## Nächste Schritte

Machen Sie sich mit den einzelnen Diensten vertraut, indem Sie Ihren ersten Fluss, eine Logik-App, eine Funktionen-App oder einen Webauftrag erstellen. Klicken Sie auf einen der folgenden Links:

- [Erste Schritte mit Microsoft Flow](https://flow.microsoft.com/de-DE/documentation/getting-started/)
- [Erstellen einer Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md)
- [Erstellen Sie Ihre erste Funktion in Azure Functions](../azure-functions/functions-create-first-azure-function.md)
- [Bereitstellen von Webaufträgen mit Visual Studio](../app-service-web/websites-dotnet-deploy-webjobs.md)

Weitere Informationen zu diesen Integrationsdiensten finden Sie auch unter den folgenden Links:

- [Leveraging Azure Functions & Azure App Service for integration scenarios (Nutzung von Azure Functions und Azure App Service für Integrationszenarien) von Christopher Anderson](http://www.biztalk360.com/integrate-2016-resources/leveraging-azure-functions-azure-app-service-integration-scenarios/)
- [Integrations Made Simple (Integrationen leicht gemacht) von Charles Lamanna](http://www.biztalk360.com/integrate-2016-resources/integrations-made-simple/)
- [Logic Apps-Livewebcast](http://aka.ms/logicappslive)
- [Häufig gestellte Fragen zu Microsoft Flow](https://flow.microsoft.com/documentation/frequently-asked-questions/)
- [Dokumentationsressourcen zu Azure WebJobs](../app-service-web/websites-webjobs-resources.md)

<!---HONumber=AcomDC_0914_2016-->