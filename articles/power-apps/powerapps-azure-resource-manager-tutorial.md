<properties
	pageTitle="Tutorial: Erstellen einer benutzerdefinierten API mit Azure Resource Manager in PowerApps und Logic Flows | Microsoft Azure"
	description="Azure Resource Manager-Tutorial zum Erstellen einer benutzerdefinierten API in PowerApps und Logic Flows"
	services=""
    suite="powerapps"
	documentationCenter="" 
	authors="sunaysv"
	manager="erikre"
	editor=""/>

<tags
   ms.service="powerapps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="07/12/2016"
   ms.author="mandia"/>


# Tutorial: Erstellen einer benutzerdefinierten AAD-geschützten ARM-API für PowerApps und Logic Flows 

Dieses Tutorial führt Sie durch die erforderlichen Schritte zum Registrieren einer Swagger-Datei, die die [ARM-API][6] beschreibt, und zum anschließenden Verbinden mit der benutzerdefinierten API in PowerApps.

>[AZURE.IMPORTANT] Dieses Thema finden Sie nun auf „powerapps.microsoft.com“ unter [Tutorial: Create a custom AAD protected ARM API for PowerApps and Flows](https://powerapps.microsoft.com/tutorials/customapi-azure-resource-manager-tutorial/) (Tutorial: Erstellen einer mit AAD geschützten benutzerdefinierten ARM-API für PowerApps und Flow) verschoben. Die aktuelle Version finden Sie in PowerApps. Dieser Azure-Link wird archiviert.


## Voraussetzungen

- Ein Azure-Abonnement
- Ein PowerApps-Konto

## Aktivieren der Authentifizierung in Azure Active Directory

Zunächst müssen wir eine Azure Active Directory-Anwendung (AAD) erstellen, die die Authentifizierung beim Aufrufen des ARM-API-Endpunkts durchführt.

1. Um eine AAD-Anwendung zu erstellen, melden Sie sich bei Ihrem [Azure-Abonnement][7] an, und wechseln Sie zu **Active Directory**: ![](./media/powerapps-azure-resource-manager-tutorial/azureaad.png "Azure AD")  

2. Wählen Sie auf dieser Seite das Verzeichnis, in dem Sie Ihre AAD-Anwendung erstellen möchten. Wählen Sie das Verzeichnis aus, klicken Sie auf die Registerkarte **Anwendungen**, und wählen Sie **Hinzufügen**: ![](./media/powerapps-azure-resource-manager-tutorial/azureapplication.png "Azure-Anwendung")

3. Geben Sie einen Namen für Ihre Anwendung ein, wählen Sie **Webanwendung und/oder Web-API** und anschließend **Weiter**: ![](./media/powerapps-azure-resource-manager-tutorial/newapplication.png "Neue Anwendung")

4. Geben Sie unter **Anmelde-URL** Folgendes ein: *http://login.windows.net*. Geben Sie unter **App-ID-URI** einen beliebigen eindeutigen URI ein. Wählen Sie dann **Fertig stellen**: ![](./media/powerapps-azure-resource-manager-tutorial/newapplication2.png "Neue zweite Anwendung")

5. Sobald die AAD-Anwendung erstellt ist, wechseln Sie zur Registerkarte **Konfigurieren**. Auf dieser Registerkarte konfigurieren wir die Berechtigungen für die Anwendung.

6. Klicken Sie unter **Berechtigungen für andere Anwendungen** auf **Anwendung hinzufügen**, und geben Sie die folgenden Berechtigungen wie gezeigt ein: ![](./media/powerapps-azure-resource-manager-tutorial/permissions.png "Berechtigungen")

	Stellen Sie sicher, dass Sie die erforderlichen Berechtigungen für Ihre Anwendung delegieren: ![](./media/powerapps-azure-resource-manager-tutorial/permissions2.png "Berechtigungen delegieren")

7. Wählen Sie unter **Schlüssel** einen beliebigen Zeitraum. **Kopieren Sie den Schlüssel, und speichern Sie ihn an einem sicheren Ort**. Wir benötigen ihn später. Notieren Sie sich auch die __Client-ID__: ![](./media/powerapps-azure-resource-manager-tutorial/configurekeys.png "Schlüssel konfigurieren")

8. Geben Sie unter **Einmaliges Anmelden** die folgende URL in __Antwort-URL__ ein: https://msmanaged-na.consent.azure-apim.net/redirect: ![](./media/powerapps-azure-resource-manager-tutorial/redirecturl.png "URL umleiten")

9. **Speichern** Sie die Änderungen. **Kopieren Sie den Schlüssel, und speichern Sie ihn an einem sicheren Ort**.

## Hinzufügen der Verbindung in PowerApps oder Logic Flows

Die AAD-Anwendung ist nun konfiguriert, und wir können die benutzerdefinierte API hinzufügen.

1. Öffnen Sie das [PowerApps-Webportal][1], wechseln Sie zur Registerkarte **Verbindungen**, und wählen Sie in der oberen rechten Ecke __Neue Verbindung hinzufügen__: ![](./media/powerapps-azure-resource-manager-tutorial/createnewconnection.png "Benutzerdefinierte API erstellen")  

2. Wählen Sie __Benutzerdefinierte API hinzufügen__: ![](./media/powerapps-azure-resource-manager-tutorial/connecttocustomapi.png "Benutzerdefinierte API erstellen")

3. Laden Sie die ARM-Swagger-Datei hoch, die zum [Herunterladen][8] verfügbar ist: ![](./media/powerapps-azure-resource-manager-tutorial/createcustom.png "Benutzerdefinierte API erstellen")

4. Da erkannt wird, dass unsere Swagger-Datei die AAD-Authentifizierung nutzt, müssen wir auf dem nächsten Bildschirm die AAD-Client-ID, den geheimen Clientschlüssel (den **Schlüssel**, den Sie an einem sicheren Ort gespeichert haben) und andere Einstellungen eingeben: ![](./media/powerapps-azure-resource-manager-tutorial/oauthsettings.png "OAuth-Einstellungen")

5. Wenn alles richtig eingerichtet ist, können Sie die benutzerdefinierte ARM-API verwenden, indem Sie eine Verbindung herstellen und dann beim Erstellen Ihrer PowerApp- oder Logic Flow-Instanz darauf verweisen: ![](./media/powerapps-azure-resource-manager-tutorial/createdcustomapi.png "Benutzerdefinierte API erstellt")

Sie können auf ähnliche Weise auf beliebige Daten zugreifen, die mit RESTful-APIs verfügbar gemacht und mit AAD-OAuth2 authentifiziert werden.

Ausführlichere Informationen zum Erstellen von PowerApps und Logic Flow finden Sie hier:

- [Connect to Office 365, Twitter, and Microsoft Translator][5] \(Verbinden mit Office 365, Twitter und Microsoft Translator)
- [Show data from Office 365][4] \(Anzeigen von Daten aus Office 365)
- [Create an app from a set of data][3] \(Erstellen einer App aus einem Satz von Daten)
- [Create Logic Flows][2] \(Erstellen von Logic Flows)

Bei Fragen oder Kommentaren zu benutzerdefinierten APIs können Sie eine E-Mail an [customapishelp@microsoft.com](mailto:customapishelp@microsoft.com) senden.


<!--Reference links in article-->
[1]: https://web.powerapps.com
[2]: https://powerapps.microsoft.com/tutorials/get-started-logic-flow/
[3]: https://powerapps.microsoft.com/tutorials/get-started-create-from-data/
[4]: https://powerapps.microsoft.com/tutorials/show-office-data/
[5]: https://powerapps.microsoft.com/tutorials/powerapps-api-functions/
[6]: https://msdn.microsoft.com/library/azure/dn790568.aspx
[7]: https://manage.windowsazure.com
[8]: http://pwrappssamples.blob.core.windows.net/samples/AzureResourceManager.json

<!---HONumber=AcomDC_0713_2016-->