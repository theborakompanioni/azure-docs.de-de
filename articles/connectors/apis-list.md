<properties
	pageTitle="Liste der von Microsoft verwalteten Connectors | Microsoft Azure App Service"
	description="Vollständige Liste der von Microsoft verwalteten Connectors, über die Sie Logik-Apps in Azure App Service erstellen können"
	services="app-service\logic"
	documentationCenter=""
	authors="MSFTMAN"
	manager="erikre"
	editor=""
    tags="connectors"/>

<tags
	ms.service="app-service-logic"
	ms.workload="integration"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.date="05/19/2016"
	ms.author="deonhe"/>

# Liste verwalteter Connectors

>[AZURE.NOTE] Diese Version des Artikels gilt für die Schemaversion 2015-08-01-preview für Logik-Apps. Um die Schemaversion 2014-12-01-preview aufzurufen, klicken Sie auf [Liste der Connectors](../app-service-logic/app-service-logic-connectors-list.md).

Informationen zu Preisen und eine Liste der auf den einzelnen Dienstebenen verfügbaren Komponenten finden Sie unter [Azure App Service-Preise](https://azure.microsoft.com/pricing/details/app-service/).

> [AZURE.NOTE] Wenn Sie Azure-Logik-Apps ausprobieren möchten, ehe Sie sich für ein Azure-Konto anmelden, besuchen Sie [Logik-App testen](https://tryappservice.azure.com/?appservice=logic). Sie können sofort eine kurzlebige Starter-Logik-App in App Service erstellen. Keine Kreditkarte erforderlich, keine Verpflichtungen.

Wählen Sie ein Symbol aus, um zu erfahren, wie Sie über diese Connectors schnell Apps zum Aufrufen der Dienste erstellen können. Diese Connectors können zum Erstellen von Logik-Apps, PowerApps und Flows verwendet werden.

|Connectors||||
|-----------|-----------|-----------|-----------|
|[![API-Symbol][blobicon]<br/>**Azure Blob**][azureblobdoc]|[![API-Symbol][bingsearchicon]<br/>**Bing-Suche**][bingsearchdoc]|[![API-Symbol][boxicon]<br/>**Box**][boxDoc]|[![API-Symbol][crmonlineicon]<br/>**CRM Online**][crmonlinedoc]|
|[![API-Symbol][dropboxicon]<br/>**Dropbox**][dropboxdoc]|[![API-Symbol][facebookicon]<br/>**Facebook**][facebookdoc]|[![API-Symbol][ftpicon]<br/>**FTP**][ftpdoc]|[![API-Symbol][githubicon]<br/>**GitHub**][githubdoc]|
[![API-Symbol][googledriveicon]<br/>**Google Drive**][googledrivedoc]|[![API-Symbol][mailchimpicon]<br/>**MailChimp**][mailchimpdoc]|[![API-Symbol][microsofttranslatoricon]<br/>**Translator**][microsofttranslatordoc]|[![API-Symbol][office365icon]<br/>**Office 365**<br/>**Outlook**][office365outlookdoc]|
|[![API-Symbol][office365icon]<br/>**Office 365**<br/>**Users**][office365usersdoc]|[![API-Symbol][office365icon]<br/>**Office 365**<br/>**Video**][office365videodoc]|[![API-Symbol][onedriveicon]<br/>**OneDrive**][onedrivedoc]|[![API-Symbol][onedriveicon]<br/>**OneDrive<br/>for Business**][onedriveforbusinessdoc]|
|[![API-Symbol][outlookicon]<br/>**Outlook**][outlookdoc]|[![API-Symbol][projectonlineicon]<br/>**Project<br/>Online**][projectonlinedoc]|[![API-Symbol][rssicon]<br/>**RSS**][rssdoc]|[![API-Symbol][salesforceicon]<br/>**Salesforce**][salesforcedoc]|
|[![API-Symbol][sendgridicon]<br/>**SendGrid**][sendgriddoc]|[![API-Symbol][servicebusicon]<br/>**Service Bus**][servicebusdoc]|[![API-Symbol][sftpicon]<br/>**SFTP**][sftpdoc]|[![API-Symbol][sharepointicon]<br/>**SharePoint**<br/>**Online**][sharepointdoc]|
|[![API-Symbol][slackicon]<br/>**Slack**<br/>][slackdoc]|[![API-Symbol][smtpicon]<br/>**SMTP**][smtpdoc]|[![API-Symbol][sqlicon]<br/>**SQL Azure**][sqldoc]|[![API-Symbol][trelloicon]<br/>**Trello**][trellodoc]|
[![API-Symbol][twilioicon]<br/>**Twilio**][twiliodoc]|[![API-Symbol][twittericon]<br/>**Twitter**][twitterdoc]|[![API-Symbol][wunderlisticon]<br/>**Wunderlist**][wunderlistdoc]|[![API-Symbol][yammericon]<br/>**Yammer**][yammerdoc] | |

> [AZURE.NOTE] Wenn Sie Logik-Apps mit dem Schema „2014-12-01-preview“ erstellt haben, werden Sie bemerken, dass die Enterprise Integration-Connectors, z.B. für BizTalk, oben nicht aufgeführt sind. Wir wissen, dass dies wichtig ist, und wir arbeiten hart daran, diese schnellstmöglich für Sie bereitzustellen. Wir können derzeit zwar noch kein genaues Verfügbarkeitsdatum angeben, aber diese Aufgabe gehört bei uns zu den Aufgaben mit der höchsten Priorität. In der Zwischenzeit können Sie auf Ihre [v1-APIs und BizTalk-APIs über Logik-Apps zugreifen](https://blogs.msdn.microsoft.com/logicapps/2016/02/25/accessing-v1-apis-and-biztalk-apis-from-logic-apps/). Vielen Dank für Ihr Verständnis. Halten Sie sich hierüber auf dem Laufenden.


### Connectors können Trigger sein.
Mehrere Connectors umfassen Trigger, die Ihre App benachrichtigen können, wenn bestimmte Ereignisse eintreten. Der FTP-Connector umfasst beispielsweise den OnUpdatedFile-Trigger. Sie können Logik-Apps, PowerApps oder Flows erstellen, die auf diesen Trigger lauschen und immer dann eine Aktion ausführen, wenn der Trigger ausgelöst wird.

Es gibt zwei Arten von Triggern:

* Abfragetrigger: Diese Trigger fragen den Dienst in angegebenen Abständen auf neue Daten ab. Wenn neue Daten verfügbar sind, wird eine neue Instanz der App mit den Daten als Eingabe ausgeführt. Damit die gleichen Daten nicht mehrere Male verarbeitet werden, kann der Trigger Daten bereinigen, die gelesen und an die App übergeben wurden.
* Push-Trigger: Diese Trigger lauschen auf den Eingang von Daten an einem Endpunkt oder das Eintreten eines Ereignisses. Dann lösen sie eine neue Instanz der App aus. Ein Beispiel hierfür ist der Twitter-Connector.


### Connectors können Aktionen sein.
Connectors können innerhalb Ihrer Apps auch als Aktionen verwendet werden. Aktionen sind hilfreich für die Suche nach Daten, die dann bei der Ausführung der App verwendet werden können. Beispielsweise kann es sein, dass Sie bei der Verarbeitung einer Bestellung Daten in einer SQL-Datenbank suchen müssen. Oder Sie müssen möglicherweise Daten in eine Zieltabelle schreiben oder in dieser aktualisieren oder löschen. Sie können dazu die Aktionen verwenden, die von den Connectors bereitgestellt werden. Aktionen sind den in den Swagger-Metadaten definierten Vorgängen zugeordnet.


[Neuigkeiten](../app-service-logic/app-service-logic-schema-2015-08-01.md) [Erstellen einer Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md) [Erste Schritte mit PowerApps](../power-apps/powerapps-get-started-azure-portal.md) [Migrieren vorhandener Logik-Apps zur neuesten Schemaversion](connectors-schema-migration.md)

<!--Connectors Documentation-->
[azureblobdoc]: ./connectors-create-api-azureblobstorage.md "Verbindung mit Azure-Blob zum Verwalten der Dateien in Ihrem Blobcontainer."
[bingsearchDoc]: ./connectors-create-api-bingsearch.md "Für die Suche in Bing im Web und nach Bildern, Nachrichten, und Videos."
[boxDoc]: ./connectors-create-api-box.md "Stellt eine Verbindung mit Box her und kann zum Hochladen, Abrufen, Löschen, Auflisten und weitere Dateiaufgaben verwendet werden."
[crmonlinedoc]: ./connectors-create-api-crmonline.md "Verbindung mit Dynamics CRM Online und weitere Verwendung Ihrer CRM Online-Daten."
[dropboxdoc]: ./connectors-create-api-dropbox.md "Stellt eine Verbindung mit Dropbox her und kann zum Abrufen, Löschen, Auflisten und weitere Dateiaufgaben verwendet werden."
[exceldoc]: ./connectors-create-api-excel.md "Verbindung mit Excel."
[facebookdoc]: ./connectors-create-api-facebook.md "Verbindung mit Facebook zum Veröffentlichen einer Zeitachse, zum Abrufen eines Seitenfeeds und vielem mehr."
[ftpdoc]: ./connectors-create-api-ftp.md "Stellt eine Verbindung mit einem FTP-/FTPS-Server her und ermöglicht u. a. die Ausführung verschiedener FTP-Tasks, einschließlich Hochladen, Abrufen und Löschen von Dateien."
[googledrivedoc]: ./connectors-create-api-googledrive.md "Verbindung mit GoogleDrive und Interaktion mit Ihren Daten."
[microsofttranslatordoc]: ./connectors-create-api-microsofttranslator.md
[office365outlookdoc]: ./connectors-create-api-office365-outlook.md "Der Office 365-Connector kann über Ihr Office 365-Konto E-Mails senden und empfangen sowie Ihren Kalender und Ihre Kontakte verwalten."
[officeunifieddoc]: ./connectors-create-api-bingsearch.md
[office365usersdoc]: ./connectors-create-api-office365-users.md
[office365videodoc]: ./connectors-create-api-office365-video.md
[onedrivedoc]: ./connectors-create-api-onedrive.md "Stellt eine Verbindung mit Ihrem persönlichen Microsoft OneDrive-Ordner her und ermöglicht u. a. das Hochladen, Löschen und Auflisten von Dateien."
[onedriveforbusinessdoc]: ./connectors-create-api-onedriveforbusiness.md "Stellt eine Verbindung mit dem Microsoft OneDrive-Ordner Ihres Unternehmens her und ermöglicht u.a. das Hochladen, Löschen und Auflisten von Dateien."
[outlookdoc]: ./connectors-create-api-outlook.md "Stellt eine Verbindung mit Ihrem Outlook-Postfach her und ermöglicht u.a. den Zugriff auf Ihre E-Mails."
[projectonlinedoc]: ./connectors-create-api-projectonline.md "Stellt eine Verbindung mit Microsoft Project Online her."
[rssdoc]: ./connectors-create-api-rss.md "Der RSS-Connector ermöglicht Benutzers das Veröffentlichen und Abrufen von Feed-Elementen. Mit ihm können Benutzer auch Operationen auslösen, wenn ein Element im Feed veröffentlicht wird."
[salesforcedoc]: ./connectors-create-api-salesforce.md "Stellt eine Verbindung mit Ihrem Salesforce-Konto her und dient zum Verwalten von Konten, Leads, Verkaufschancen und mehr."
[sendgriddoc]: ./connectors-create-api-sendgrid.md "Stellt eine Verbindung mit Microsoft Project Online her."
[servicebusdoc]: ./connectors-create-api-servicebus.md "Kann Nachrichten aus Service Bus-Warteschlangen und -Themen senden sowie Nachrichten aus Service Bus-Warteschlangen und -Abonnements empfangen."
[sharepointdoc]: ./connectors-create-api-sharepointonline.md "Stellt eine Verbindung mit SharePoint Online her, um Dokumente und Listenelemente zu verwalten."
[slackdoc]: ./connectors-create-api-slack.md "Herstellen einer Verbindung mit Slack und Veröffentlichen von Nachrichten auf Slack-Kanälen."
[sftpdoc]: ./connectors-create-api-sftp.md "Stellt eine Verbindung mit SFTP her und kann zum Hochladen, Abrufen und Löschen von Dateien verwendet werden."
[githubdoc]: ./connectors-create-api-github.md "Stellt eine Verbindung mit GitHub her und ermöglicht das Nachverfolgen von Problemen."
[mailchimpdoc]: ./connectors-create-api-mailchimp.md "Bessere E-Mails versenden"
[smtpdoc]: ./connectors-create-api-smtp.md "Stellt eine Verbindung mit einem SMTP-Server her und kann E-Mails mit Anlagen senden."
[sqldoc]: ./connectors-create-api-sqlazure.md "Stellt eine Verbindung mit einer SQL Azure-Datenbank her. Sie können Einträge in einer SQL-Datenbanktabelle erstellen, aktualisieren, abrufen und löschen."
[trellodoc]: ./connectors-create-api-trello.md "Trello ist eine kostenlose, flexible und visuelle Möglichkeit, um etwas mit jemandem zu organisieren."
[twiliodoc]: ./connectors-create-api-twilio.md "Stellt eine Verbindung mit Twilio her und kann Nachrichten und verfügbare Nummern abrufen, eingehende Telefonnummern verwalten usw."
[twitterdoc]: ./connectors-create-api-twitter.md "Stellt eine Verbindung mit Twitter her und dient u. a. zum Abrufen von Zeitachsen und Posten von Tweets."
[wunderlistdoc]: ./connectors-create-api-wunderlist.md "Behalten Sie den Überblick."
[yammerdoc]: ./connectors-create-api-yammer.md "Stellt eine Verbindung mit Yammer her, um Nachrichten zu posten und neue Nachrichten abzurufen."

<!--Icon references-->
[blobicon]: ./media/apis-list/blobicon.png
[bingsearchicon]: ./media/apis-list/bingsearchicon.png
[boxicon]: ./media/apis-list/boxicon.png
[ftpicon]: ./media/apis-list/ftpicon.png
[githubicon]: ./media/apis-list/githubicon.png
[crmonlineicon]: ./media/apis-list/dynamicscrmicon.png
[dropboxicon]: ./media/apis-list/dropboxicon.png
[excelicon]: ./media/apis-list/excelicon.png
[facebookicon]: ./media/apis-list/facebookicon.png
[googledriveicon]: ./media/apis-list/googledriveicon.png
[mailchimpicon]: ./media/apis-list/mailchimpicon.png
[microsofttranslatoricon]: ./media/apis-list/translatoricon.png
[office365icon]: ./media/apis-list/office365icon.png
[onedriveicon]: ./media/apis-list/onedriveicon.png
[onedriveforbusinessicon]: ./media/apis-list/onedriveforbusinessicon.png
[outlookicon]: ./media/apis-list/outlookicon.png
[projectonlineicon]: ./media/apis-list/projectonlineicon.png
[rssicon]: ./media/apis-list/rssicon.png
[salesforceicon]: ./media/apis-list/salesforceicon.png
[sendgridicon]: ./media/apis-list/sendgridicon.png
[servicebusicon]: ./media/apis-list/servicebusicon.png
[sftpicon]: ./media/apis-list/sftpicon.png
[sharepointicon]: ./media/apis-list/sharepointicon.png
[slackicon]: ./media/apis-list/slackicon.png
[smtpicon]: ./media/apis-list/smtpicon.png
[sqlicon]: ./media/apis-list/sqlicon.png
[trelloicon]: ./media/apis-list/trelloicon.png
[twilioicon]: ./media/apis-list/twilioicon.png
[twittericon]: ./media/apis-list/twittericon.png
[wunderlisticon]: ./media/apis-list/wunderlisticon.png
[yammericon]: ./media/apis-list/yammericon.png

<!---HONumber=AcomDC_0525_2016-->