<properties
    pageTitle="Liste der verfügbaren Connectors und API-Apps | Microsoft Azure App Service"
    description="Erfahren Sie etwas über die Connectors und API-Apps in Azure App Service"
    services="logic-apps"
    documentationCenter=""
    authors="MandiOhlinger"
    manager="erikre"
    editor="cgronlun"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/01/2016"
    ms.author="mandia"/>



# <a name="list-of-connectors-and-api-apps-to-use-in-your-logic-apps"></a>Liste der Connectors und API-Apps in Logik-Apps
>[AZURE.NOTE] Diese Version des Artikels gilt für die Logik-Apps-Schemaversion 2014-12-01-preview. Informationen zur allgemein verfügbaren Version von Logik-Apps finden Sie unter [Neue Connectorliste](../connectors/apis-list.md).

Hier werden alle verfügbaren Connectors und API-Apps aufgelistet, die von Microsoft für die Verwendung in Logik-Apps erstellt wurden.

Informationen zu Preisen und eine Liste der auf den einzelnen Dienstebenen verfügbaren Komponenten finden Sie unter [Azure App Service-Preise](https://azure.microsoft.com/pricing/details/app-service/).

> [AZURE.NOTE] Wenn Sie Logik-Apps ausprobieren möchten, ehe Sie sich für ein Azure-Konto anmelden, besuchen Sie [Logik-App testen](https://tryappservice.azure.com/?appservice=logic). Sie können sofort eine kurzlebige Starter-Logik-App in App Service erstellen. Keine Kreditkarte erforderlich, keine Verpflichtungen.

## <a name="core-connectors"></a>Wichtige Connectors
Die folgende Tabelle listet alle verfügbaren von Microsoft erstellten Connectors und API-Apps auf, die als Haupt-Connectors zur Verfügung stehen:

Name | Beschreibung
--- | ---
[Bing-Übersetzer](https://azure.microsoft.com/marketplace/partners/bing/microsofttranslator/) | Für Übersetzungen von Text in eine andere Sprache in Bing.
[HTTP](app-service-logic-connector-http.md) | Der HTTP-Listener öffnet einen Endpunkt, der als HTTP-Server fungiert und auf eingehende HTTP- oder HTTPS-Anforderungen lauscht. Die HTTP-Aktion erfordert keine API-App und wird nativ innerhalb der Logik-Apps unterstützt.
[Puffer](app-service-logic-connector-slack.md) | Herstellen einer Verbindung mit Slack und Veröffentlichen von Nachrichten auf Slack-Kanälen.


## <a name="enterprise-integration-connectors"></a>Enterprise-Integrationsconnectors
Die folgende Tabelle enthält alle verfügbaren von Microsoft erstellten Connectors und API-Apps, die als Enterprise-Integrationsconnectors zur Verfügung stehen:

Name  | Beschreibung
------------- | -------------
[BizTalk-Regeln](app-service-logic-use-biztalk-rules.md) | Verwenden Sie BizTalk-Regeln zum Definieren und Steuern der Geschäftslogik innerhalb einer Organisation. Geschäftsrichtlinien können ohne erneute Kompilierung oder ohne erneute Bereitstellung der verbundenen Anwendungen aktualisiert werden.
[BizTalk XPath Extractor](app-service-logic-xpath-extract.md) | Sucht und extrahiert Daten aus XML-Inhalt basierend auf der ausgewählten XPath-Sprache.
[DB2-Connector](app-service-logic-connector-db2.md) | Stellt lokal und auf einem virtuellen Azure-Computer unter einem Windows-Betriebssystem eine Verbindung mit einer IBM DB2-Datenbank her. Er kann Befehlen der Informix Structured Query Language Web-API- und OData-API-Vorgänge zuordnen. <br/><br/>Keine Trigger. Aktionen umfassen die Auswahl von Tabellen, das Einfügen, Aktualisieren und Löschen sowie benutzerdefinierte Anweisungen.<br/><br/>Dieser Connector enthält auch den Microsoft-Client für DRDA zum Herstellen einer Verbindung mit einem Informix-Server über ein TCP/IP-Netzwerk.
[Datei](app-service-logic-connector-file.md) | Mit diesem Connector können Sie eine Verbindung mit dem lokalen Dateisystem oder Netzwerk herstellen und verschiedene Dateiaufgaben, einschließlich Hochladen, Löschen und Auflisten von Dateien, und vieles mehr ausführen.
[Informix](app-service-logic-connector-informix.md) | Stellt lokal und auf einem virtuellen Azure-Computer unter einem Windows-Betriebssystem eine Verbindung mit einer IBM Informix-Datenbank her. Er kann Befehlen der Informix Structured Query Language Web-API- und OData-API-Vorgänge zuordnen.<br/><br/>Keine Trigger. Aktionen umfassen die Auswahl von Tabellen, das Einfügen, Aktualisieren und Löschen sowie benutzerdefinierte Anweisungen.<br/><br/>Lokal oder mit VPN oder Azure kann ExpressRoute verwendet werden. Dieser Connector enthält auch einen Microsoft-Client für DRDA zum Herstellen einer Verbindung mit einem Informix-Server über ein TCP/IP-Netzwerk.
[Microsoft SQL Server](app-service-logic-connector-sql.md) | Stellt eine Verbindung mit dem lokalen SQL Server oder einer Azure SQL-Datenbank her. Sie können Einträge in einer SQL-Datenbanktabelle erstellen, aktualisieren, abrufen und löschen.
MQ | Stellt lokal und auf einem virtuellen Azure-Computer unter einem Windows-Betriebssystem eine Verbindung mit einem IBM WebSphere MQ-Server, Version 8, her. Lokal oder mit VPN oder Azure kann ExpressRoute verwendet werden. Der Connector enthält außerdem den Microsoft-Client für MQ.<br/><br/>Keine Trigger. Keine Aktionen.<br/><br/>**Hinweis** Kann derzeit nicht mit Logic Apps verwendet werden.

## <a name="connectors-as-triggers"></a>Connectors als Trigger
Mehrere Connectors bieten Trigger für Logik-Apps. Diese Trigger haben zwei Typen:

1. Abfragetrigger: Diese Trigger fragen den Dienst in angegebenen Abständen auf neue Daten ab. Sobald neue Daten verfügbar sind, wird eine neue Instanz der Logik-App mit den Daten als Eingabe ausgeführt. Damit die gleichen Daten nicht mehrere Male verarbeitet werden, kann der Trigger Daten bereinigen, die gelesen und an die Logik-App übergeben wurden. Beispiele für solche Connectors sind "Datei", "SQL" und "Azure Storage".
2. Push-Trigger: Diese Trigger lauschen auf den Eingang von Daten an einem Endpunkt oder das Eintreten eines Ereignisses. Dann lösen sie eine neue Instanz einer Logik-App aus. Beispiele für solche Connectors sind "HTTP-Listener" und "Twitter".

## <a name="connectors-as-actions"></a>Connectors als Aktionen
Connectors können innerhalb Ihrer Logik-App auch als Aktionen verwendet werden. Aktionen sind hilfreich für die Suche nach Daten in der Logik-App, die dann bei der Ausführung verwendet werden können. Beispielsweise kann es sein, dass Sie Daten aus einer SQL-­Datenbank für zusätzliche Informationen zu einem Kunden bei der Verarbeitung einer Bestellung nachschauen müssen. Oder Sie müssen möglicherweise Daten in ein Ziel schreiben, darin aktualisieren oder löschen. Sie können dazu die Aktionen verwenden, die von den Connectors bereitgestellt werden. Aktionen werden Vorgängen in API-Apps zugeordnet (gemäß ihren Swagger-Metadaten).

## <a name="create-your-own-connectors-and-api-apps"></a>Erstellen Sie eigene Connectors und API-Apps
[Referenz zu Connectors und API-Apps](http://aka.ms/appservicesconnectorreference)  
[Azure App Service – API-App-Trigger](../app-service-api/app-service-api-dotnet-triggers.md)  
[Referenz zu Logik-Apps](https://msdn.microsoft.com/library/azure/dn948510.aspx)

## <a name="more-on-connectors-and-api-apps"></a>Weitere Informationen zu Connectors und API-Apps
[Was sind Connectors und BizTalk-API-Apps?](app-service-logic-what-are-biztalk-api-apps.md)  
[Verwenden des Hybrid Connection Managers in Azure App Service](app-service-logic-hybrid-connection-manager.md)  
[Verwalten und Überwachen integrierter API-Apps und Connectors](app-service-logic-monitor-your-connectors.md)



<!--HONumber=Oct16_HO2-->


