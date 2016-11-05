---
title: 'Tutorial: Erstellen einer benutzerdefinierten API mit einer ASP.Net-Web-App in PowerApps und Logic Flows | Microsoft Docs'
description: ASP.Net-Web-App-Tutorial zum Erstellen einer benutzerdefinierten API in PowerApps und Logic Flows
services: ''
suite: powerapps
documentationcenter: ''
author: sunaysv
manager: erikre
editor: ''

ms.service: powerapps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/12/2016
ms.author: mandia

---
# Tutorial: Erstellen einer benutzerdefinierten AAD-geschützten Web-API für PowerApps und Logic Flows
In diesem Tutorial erfahren Sie, wie Sie eine ASP.Net-Web-API erstellen, sie in Azure Webapps hosten, die AAD-Authentifizierung aktivieren (über mühelose Authentifizierung) und dann die Web-API in PowerApps und Logic Flows registrieren.

> [!IMPORTANT]
> Dieses Thema finden Sie nun auf „powerapps.microsoft.com“ unter [Tutorial: Create a custom AAD protected Web API for PowerApps and Flows](https://powerapps.microsoft.com/tutorials/customapi-web-api-tutorial/) (Tutorial: Erstellen einer mit AAD geschützten benutzerdefinierten Web-API für PowerApps und Flow) verschoben. Die aktuelle Version finden Sie in PowerApps. Dieser Azure-Link wird archiviert.
> 
> 

## Voraussetzungen
* Ein Azure-Abonnement
* Ein PowerApps-Konto
* Visual Studio 2013 oder höher

## Schritt 1: Erstellen einer Web-API und ihre Bereitstellung in Azure
1. Öffnen Sie Visual Studio, und erstellen Sie eine neue C#-ASP.NET-Webanwendung: ![](./media/powerapps-web-api-tutorial/newwebapp.png "Neue WebApp")
2. Wählen Sie auf der nächsten Seite die Web-API-Vorlage und dann **Keine Authentifizierung**: ![](./media/powerapps-web-api-tutorial/noauth.png "Keine Autorisierung")
   
   > [!IMPORTANT]
   > Stellen Sie sicher, dass Sie für die Authentifizierung „Keine Authentifizierung“ festlegen.
   > 
   > 
3. Wenn das Projekt erstellt wird, müssen Sie die Web-API für Ihre Ressourcen erstellen. In diesem Tutorial gehen wir nicht auf die Einzelheiten der Erstellung einer Web-API ein.
4. Generieren Sie als Nächstes eine Swagger-Datei für die Web-API. Sie können hierzu einfach die **Paket-Manager-Konsole** öffnen und **Swashbuckle** installieren: ![](./media/powerapps-web-api-tutorial/swashbuckle-console.png "Swashbuckle-Konsole")
5. Durchsuchen Sie nach Installation und Aktivierung die folgenden jeweiligen Swagger-Dokumente und Benutzeroberflächen-Endpunkte: **<Ihre-Stamm-URL>/swagger/docs/v1**.
   
     **<Ihre-Stamm-URL>/swagger**
6. Sobald Sie mit Ihrer API vertraut sind, veröffentlichen Sie sie in Azure. Gehen Sie zum Veröffentlichen in Visual Studio zu **BUILD**, und wählen Sie **VERÖFFENTLICHEN**.
7. Extrahieren Sie den Swagger-JSON-Inhalt, indem Sie zu ***https://\<Azure-Webapp-URL>/swagger/docs/v1*** navigieren.
   
   > [!IMPORTANT]
   > Ein Swagger-Dokument mit doppelten Vorgangs-ID(s) ist ungültig. Wenn Sie die C#-Beispielvorlage verwenden, wird die Vorgangs-ID „Values\_Get“ zweimal wiederholt. Ändern Sie eine Instanz in „Value\_Get“.
   > 
   > 

Sie können das in diesem Tutorial verwendete Swagger [hier][6] herunterladen. Achten Sie darauf, die Kommentare vor der Verwendung zu ersetzen/entfernen. Kommentare beginnen mit `//`.

## Schritt 2: Einrichten der AAD-Authentifizierung
Dieses Tutorial setzt voraus, dass Sie wissen, wie AAD-Anwendungen in Azure erstellt werden. Weitere Informationen zum Erstellen einer AAD-Anwendung finden Sie im [Azure Resource Manager-Tutorial](powerapps-azure-resource-manager-tutorial.md). Wir benötigen zwei AAD-Anwendungen für dieses Tutorial.

1. Die erste AAD-Anwendung wird zum Sichern der Web-API verwendet. Nennen Sie sie **webAPI**.
2. Die zweite AAD-Anwendung dient zum Sichern der benutzerdefinierten API-Registrierung und zum Abrufen des delegierten Zugriffs auf die Web-API, die durch die erste App geschützt wird. Nennen Sie diese **webAPI-customAPI**.
3. Verwenden Sie für **webAPI** die folgende Konfiguration:
   
   1. Anmelde-URL: ***https://login.windows.net***
   2. App-ID-URI: ***https://\<Ihre-Stamm-URL>*** (in der Regel die in Azure bereitgestellte URL Ihrer Website)
   3. Antwort-URL: ***https://\<Ihre-Stamm-URL>/.auth/login/aad/callback***
      
      > [!IMPORTANT]
      > Sie benötigen die Client-ID dieser App später, darum notieren Sie sie.
      > 
      > 
4. Verwenden Sie für **webAPI-customAPI** die folgende Konfiguration:
   
   1. Anmelde-URL: **https://login.windows.net**
   2. App-ID-URI: ***kann jede eindeutige URL sein***
   3. Antwort-URL: ***https://msmanaged-na.consent.azure-apim.net/redirect***
   4. Fügen Sie Berechtigungen für den delegierten Zugriff auf webAPI hinzu.
   5. Sie benötigen die Client-ID dieser App später ebenfalls, darum notieren Sie sie.
   6. Generieren Sie einen Schlüssel, und speichern Sie ihn an einem sicheren Ort. Wir benötigen diesen Schlüssel später.

> [!IMPORTANT]
> Beide Apps müssen sich im gleichen Verzeichnis befinden.
> 
> 

## Schritt 3: Einrichten von müheloser Authentifizierung auf Ihrer Web-App
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und wechseln Sie zu Ihrer Web-App, die Sie in **Schritt 1** (in diesem Thema) bereitgestellt haben.
2. Wählen Sie unter **Einstellungen** die Option **„Authentifizierung/Autorisierung“**.
3. Aktivieren Sie **App Service-Authentifizierung**, und wählen Sie **Azure Active Directory**. Wählen Sie auf dem nächsten Blatt **Express**.
4. Klicken Sie auf **Vorhandene AD-App auswählen**, und wählen Sie die erste AAD-Anwendung, die Sie als Teil des zweiten Schritts erstellt haben. Wählen Sie in diesem Fall **webAPI**.

So sollte die AAD-Authentifizierung für Ihre Web-App eingerichtet werden.

## Schritt 4: Einrichten der benutzerdefinierten API
1. Wir müssen unser Swagger etwas ändern, um das `securityDefintions`-Objekt und die für die Web-App verwendete AAD-Authentifizierung einzugeben. Fügen Sie die folgenden Codezeilen hinzu:
   
    ```javascript
   "host": "<your-root-url>",
   "schemes": [
    "https"                        //Change scheme to https 
   ],
   "securityDefinitions": {
    "AAD": {
      "type": "oauth2",
      "flow": "implicit",
      "authorizationUrl": "https://login.windows.net/common/oauth2/authorize",
      "scopes": []
    }
   },
    ```
2. Wechseln Sie zum PowerApps-[Webportal][1], und fügen Sie eine benutzerdefinierte API hinzu. Unter [Use Custom APIs in Logic Flows and PowerApps](powerapps-register-custom-api.md) (Verwenden benutzerdefinierter APIs in Logic Flows und PowerApps) sind die Schritte aufgelistet.
3. Nachdem Sie Ihr Swagger hochgeladen haben, erkennt der Assistent automatisch, dass Sie die AAD-Authentifizierung für Ihre „webAPI“ verwenden.
4. Konfigurieren Sie die AAD-Authentifizierung für Ihre benutzerdefinierte API:
   
   1. Client-ID: ***Client-ID von webAPI-CustomAPI*** aus 4.5 in **Schritt 2** (in diesem Thema)
   2. Geheimer Schlüssel: ***Schlüssel aus webPI-CustomAPI*** aus 4.6 in **Schritt 2** (in diesem Thema)
   3. Anmelde-URL: ***https://login.windows.net***
   4. Ressourcen-URI: ***Client-ID von webAPI*** aus 3.4 in **Schritt 2** (in diesem Thema)
5. Wählen Sie **Erstellen**, und versuchen Sie, eine Verbindung mit der benutzerdefinierten API herzustellen. Wenn alles richtig eingerichtet ist, sollten Sie sich erfolgreich anmelden können.

Ausführlichere Informationen zum Erstellen von PowerApps und Logic Flow finden Sie hier:

* [Connect to Office 365, Twitter, and Microsoft Translator (Verbinden mit Office 365, Twitter und Microsoft Translator)][5]
* [Show data from Office 365 (Anzeigen von Daten aus Office 365)][4]
* [Create an app from a set of data (Erstellen einer App aus einem Satz von Daten)][3]
* [Create Logic Flows (Erstellen von Logic Flows)][2]

Bei Fragen oder Kommentaren können Sie eine E-Mail an [customapishelp@microsoft.com](mailto:customapishelp@microsoft.com) senden.

<!--Reference links in article-->
[1]: https://web.powerapps.com
[2]: https://powerapps.microsoft.com/tutorials/get-started-logic-flow/
[3]: https://powerapps.microsoft.com/tutorials/get-started-create-from-data/
[4]: https://powerapps.microsoft.com/tutorials/show-office-data/
[5]: https://powerapps.microsoft.com/tutorials/powerapps-api-functions/
[6]: http://pwrappssamples.blob.core.windows.net/samples/webAPI.json

<!---HONumber=AcomDC_0713_2016-->