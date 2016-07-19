<properties 
	pageTitle="Was sind Logik-Apps?" 
	description="Weitere Informationen zu App Service-Logik-Apps" 
	authors="kevinlam1" 
	manager="dwrede" 
	editor="" 
	services="app-service\logic" 
	documentationCenter=""/>

<tags
	ms.service="app-service-logic"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="hero-article" 
	ms.date="07/12/2016"
	ms.author="klam"/>

#Was sind Logik-Apps?

| Kurzübersicht |
| --------------- |
| [Logik-Apps – Definitionssprache](https://msdn.microsoft.com/library/azure/mt643789.aspx) |
| [Logik-Apps – Connector-Dokumentation](../connectors/apis-list.md) |
| [Logik-Apps – Forum](https://social.msdn.microsoft.com/Forums/de-DE/home?forum=azurelogicapps) |

Mit Logik-Apps können Sie komplexe Geschäftsprozesse vereinfachen und implementieren. Es wird ein visueller Designer bereitgestellt, mit dem Sie Ihre Geschäftsprozesse in Form von Schritten (als Workflow bezeichnet) modellieren und automatisieren können. Sie können Workflows so entwerfen, dass sie über einen „Trigger“ gestartet und dann die einzelnen Schritte ausgeführt werden. Bei jedem Schritt wird eine API aufgerufen, und zugleich werden eine sichere Authentifizierung sowie die Einhaltung bewährter Methoden wie Prüfpunktausführung und beständige Ausführung gewährleistet.

Die Verwendung von Logik-Apps hat u.a. die folgenden Vorteile:

- Sparen von Zeit, indem komplexe Prozesse mit einfachen Designtools entworfen werden
- Einfaches Implementieren von Aufgaben, die im Code sonst nur schwer zu implementieren sind
- Schnelles Starten mit Vorlagen
- Anpassen von Logik-Apps mit APIs
- Verbinden unterschiedlicher Systeme
- Monetarisieren Ihrer Logik-App über den Marketplace
- Problemloses Starten und Wachsen nach Bedarf

Bei Logik-Apps handelt es sich um einen vollständig verwalteten Dienst vom Typ „iPaaS“ (integration Platform as a Service), mit dem Entwickler sich keine Sorgen müssen, dass sie in Bezug auf Hosting, Skalierbarkeit, Verfügbarkeit und Verwaltung zusätzliche Arbeitsschritte ausführen müssen.

Und was am wichtigsten ist: Logik-Apps können mit integrierten [verwalteten Connectors][managedapis] kombiniert werden, um selbst schwierige Integrationsszenarien problemlos zu meistern:

![Datenfluss-App-Designer](./media/app-service-logic-what-are-logic-apps/LogicAppCapture2.png)

Wie erwähnt können Sie mit Logik-Apps Geschäftsprozesse automatisieren. Hier sind einige Beispiele:
 
* Sie können z. B. neue Datensätze in Ihrer SQL-Datenbank automatisch replizieren und per E-Mail an die Mitarbeiter mit Kundenkontakt senden.
* Sie können automatisch negative Tweets suchen und an einen Slack-Kanal senden.
* Gehen Sie wie folgt vor, wenn auf einem FTP-Server eine Datei erstellt wird: Analysieren Sie die Datei, fügen Sie Dynamics CRM neue Datensätze hinzu, und erstellen Sie ein Element in einer SharePoint-Liste.

Ähnliche Szenarios können über den visuellen Designer konfiguriert werden, ohne eine einzige Codezeile schreiben zu müssen. Erste Schritte zum sofortigen [Erstellen Ihrer Logik-App][create].

## Gründe für Logik-Apps

Wenn Sie einen beliebigen Geschäftsprozess automatisieren möchten (z.B. negative Tweets finden und an Ihren internen Slack-Kanal senden oder neue Kundendatensätze aus SQL gleich bei ihrem Eintreffen in das CRM-System replizieren), können Sie mit Logik-Apps ganz leicht verschiedene Datenquellen integrieren – ob in der Cloud oder lokal. Lesen Sie sich die interessanten Informationen zu unseren [verwalteten Connectors][managedapis] durch. Führen Sie jetzt die [ersten Schritte][create] aus, um Ihre Optionen kennenzulernen.

Mit unserem [Unternehmensintegrationskonto][biztalk] können Sie erweiterte Integrationsszenarien nutzen, z.B. mit [XML-Messaging][xml], [Handelspartnerverwaltung][tpm] und vielem mehr.

- **Leicht zu bedienende Entwurfstools** – Logik-Apps können von A bis Z im Browser entwickelt werden. Beginnen Sie mit einem Trigger - von einem einfachen Zeitplan bis zum Auftauchen eines Tweets zu Ihrem Unternehmen. Orchestrieren Sie anschließend eine beliebige Anzahl von Aktionen mithilfe des umfassenden Angebots an Connectors.

- **Einfaches Zusammensetzen von SaaS** – Zusammensetzungsaufgaben, die einfach zu beschreiben sind, lassen sich nur schwierig in Code implementieren. Mit Logik-Apps ist es kinderleicht, verschiedenartige Systeme miteinander zu verbinden. Möchten Sie in Ihrer CRM-Software eine Aufgabe erstellen, die auf Aktivität in Ihren Facebook- oder Twitter-Konten basiert? Wollen Sie Ihre Marketinglösung in der Cloud mit Ihrem lokalen Abrechnungssystem verbinden? Logik-Apps sind die schnellste und zuverlässigste Möglichkeit, Lösungen für diese Probleme bereitzustellen.

- **Schneller Einstieg mit Vorlagen** – Um Ihnen den Einstieg zu erleichtern, bieten wir einen [Katalog mit Vorlagen][templates], mit deren Hilfe Sie einige allgemeine Lösungen rasch erstellen können. Von BizTalk-Lösungen bis zu einfachen SaaS-Konnektivitätslösungen und sogar einigen, die "just for fun" sind – der Katalog ist die schnellste Möglichkeit, sich mit der Leistungsfähigkeit von Logik-Apps vertraut zu machen.

- **Integrierte Erweiterbarkeit** – Sie finden den benötigten Connector nicht? Logik-Apps sind für das Arbeiten mit API-Apps ausgelegt. Sie können problemlos eine eigene API-App entwickeln und anschließend als benutzerdefinierte API nutzen. Entwickeln Sie eine neue App nur für sich allein, oder veröffentlichen Sie sie im Marketplace, um sie kommerziell zu verwerten.

- **Hohes Integrationspotenzial** – Fangen Sie einfach an, und wachsen Sie Ihrem Bedarf entsprechend. Logik-Apps können problemlos die Leistungsstärke von BizTalk, der branchenführenden Integrationslösung von Microsoft nutzen, damit Integrationsexperten die Lösungen erstellen können, die sie brauchen. Informieren Sie sich eingehender über das [Enterprise Integration Pack](./app-service-logic-enterprise-integration-overview.md).

## Logik-App – Konzepte

Es folgt eine Beschreibung einiger der wichtigsten Elemente von Logik-Apps.

- **Workflow** – Logik-Apps bieten eine grafische Möglichkeit zum Abbilden Ihrer Geschäftsprozesse als Folge von Schritten bzw. Workflow.
- **Verwaltete Connectors** – Ihre Logik-Apps benötigen Zugriff auf Daten und Dienste. Verwaltete Connectors werden speziell erstellt, um Ihnen beim Herstellen von Verbindungen und Arbeiten mit Ihren Daten zu helfen. Die Liste der aktuell verfügbaren Connectors finden Sie unter [Verwaltete Connectors][managedapis].
- **Trigger** – Einige verwaltete Connectors können auch als Trigger fungieren. Ein Trigger startet eine neue Instanz eines Workflows basierend auf einem bestimmten Ereignis, z. B. Eingang einer E-Mail oder einer Änderung in Ihrem Azure Storage-Konto.
-  **Aktionen** – Jeder Schritt nach dem Trigger in einem Workflow wird als Aktion bezeichnet. Jede Aktion wird in der Regel einem Vorgang in Ihrem verwalteten Connector oder benutzerdefinierten API-Apps zugeordnet.
- **Enterprise Integration Pack** – Für anspruchsvollere Integrationsszenarien enthalten Logik-Apps Funktionen von BizTalk. BizTalk ist die branchenführende Integrationsplattform von Microsoft. Mit den Enterprise Integration Pack-Connectors können Sie in Ihre Logik-App-Workflows auf einfache Weise eine Überprüfung, Transformation usw. einbinden.

## Erste Schritte  

 - Befolgen Sie zum Einstieg in Logik-Apps das Lernprogramm [Erstellen einer Logik-App][create].
 - [Anzeigen allgemeiner Beispiele und Szenarien](app-service-logic-examples-and-scenarios.md)
 - [Sie können Geschäftsprozesse mit Logik-Apps automatisieren.](http://channel9.msdn.com/Events/Build/2016/T694)
 - [Erfahren Sie, wie Sie Ihre Systeme in Logik-Apps integrieren.](http://channel9.msdn.com/Events/Build/2016/P462)
- Weitere Informationen zur Azure App Service-Plattform finden Sie unter [Azure App Service][appservice].

[biztalk]: app-service-logic-enterprise-integration-accounts.md
[appservice]: ../app-service/app-service-value-prop-what-is.md
[create]: app-service-logic-create-a-logic-app.md
[managedapis]: ../connectors/apis-list.md
[tpm]: app-service-logic-enterprise-integration-accounts.md
[xml]: app-service-logic-enterprise-integration-b2b.md
[templates]: app-service-logic-use-logic-app-templates.md

<!---HONumber=AcomDC_0713_2016-->