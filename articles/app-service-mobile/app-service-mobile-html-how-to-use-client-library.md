<properties
	pageTitle="Verwenden des JavaScript-SDKs für Azure Mobile Apps"
	description="Vorgehensweise beim Verwenden von V for Azure Mobile Apps"
	services="app-service\mobile"
	documentationCenter="javascript"
	authors="adrianhall"
	manager="erikre"
	editor=""/>

<tags
	ms.service="app-service-mobile"
	ms.workload="mobile"
	ms.tgt_pltfrm="html"
	ms.devlang="javascript"
	ms.topic="article"
	ms.date="06/29/2016"
	ms.author="adrianha;ricksal"/>

# Verwenden der JavaScript-Clientbibliothek für Azure Mobile Apps

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

Dieser Artikel beschreibt gängige Szenarien für die Verwendung des neuesten [JavaScript-SDKs für Azure Mobile Apps]. Wenn Sie mit Azure Mobile Apps noch nicht vertraut sind, führen Sie zunächst den [Schnellstart von Azure Mobile Apps] durch, um ein Back-End und eine Tabelle zu erstellen. In diesem Leitfaden liegt der Schwerpunkt auf der Verwendung des mobilen Back-Ends in HTML/JavaScript-Webanwendungen.

##<a name="Setup"></a>Einrichtung und Voraussetzungen

Dieses Lernprogramm setzt voraus, dass Sie ein Back-End mit einer Tabelle erstellt haben. In dieser Anleitung wird davon ausgegangen, dass die Tabelle das gleiche Schema wie die Tabellen in diesen Lernprogrammen aufweist.

Die Installation des JavaScript SDKs für Azure Mobile Apps kann durch den Befehl `npm` vorgenommen werden.

```
npm install azure-mobile-apps-client --save
```

Wenn die Installation abgeschlossen ist, befindet sich die Bibliothek in `node_modules/azure-mobile-apps-client/dist/MobileServices.Web.min.js`. Kopieren Sie diese Datei in Ihren Web-Bereich.

```
<script src="path/to/MobileServices.Web.min.js"></script>
```

Die Bibliothek kann auch als ES2015-Modul in CommonJS-Umgebungen verwendet werden, wie z. B. Browserify und Webpack sowie als eine AMD-Bibliothek. Beispiel:

```
# For ECMAScript 5.1 CommonJS
var WindowsAzure = require('azure-mobile-apps-client');
# For ES2015 modules
import * as WindowsAzure from 'azure-mobile-apps-client';
```

[AZURE.INCLUDE [app-service-mobile-html-js-library](../../includes/app-service-mobile-html-js-library.md)]

##<a name="auth"></a>Gewusst wie: Authentifizieren von Benutzern

Azure App Service unterstützt die Authentifizierung und Autorisierung von Anwendungsbenutzern mit einer Vielzahl externer Identitätsanbieter: Facebook, Google, Microsoft-Konto und Twitter. Sie können Berechtigungen für Tabellen vergeben, um den Zugriff auf bestimmte Operationen auf authentifizierte Benutzer zu beschränken. Außerdem können Sie die Identität authentifizierter Benutzer verwenden, um Autorisierungsregeln in Serverskripts zu implementieren. Weitere Informationen finden Sie im Lernprogramm [Erste Schritte mit der Authentifizierung].

Insgesamt werden zwei Authentifizierungsflüsse unterstützt: ein Serverfluss und ein Clientfluss. Der Serverfluss bietet die einfachste Authentifizierungsform, da in diesem Fall die Authentifizierungs-Webschnittstelle des Anbieters verwendet wird. Der Clientfluss ermöglicht eine tiefere Integration mit gerätespezifischen Fähigkeiten wie z. B. einmalige Anmeldung, da in diesem Fall anbieterspezifische SDKs verwendet werden.

[AZURE.INCLUDE [app-service-mobile-html-js-auth-library](../../includes/app-service-mobile-html-js-auth-library.md)]

###<a name="configure-external-redirect-urls"></a>So wird's gemacht: Konfigurieren Ihres Mobile App Service für externe Umleitungs-URLs

Mehrere Arten von JavaScript-Anwendungen verwenden zur Behandlung von OAuth-UI-Datenflüssen eine Loopback-Funktion – etwa, wenn Sie Ihren Dienst lokal ausführen, Live Reload im Ionic-Framework verwenden oder zur Authentifizierung eine Umleitung zu App Service durchführen. Dies kann problematisch sein, da die App Service-Authentifizierung in der Standardkonfiguration nur Zugriffe von Ihrem mobilen App-Back-End zulässt.

Führen Sie die folgenden Schritte durch, um die App Service-Einstellungen zu ändern und die Authentifizierung über Ihren lokalen Host zu ermöglichen:

1. Melden Sie sich beim [Azure-Portal] an, navigieren Sie zu Ihrem mobilen App-Back-End, und klicken Sie dann auf **Tools** > **Ressourcen-Explorer** > **Start**, um ein neues Ressourcen-Explorer-Fenster für Ihr mobiles App-Back-End (Website) zu öffnen.

2. Erweitern Sie den Knoten **config** für Ihre App. Klicken Sie anschließend auf **authsettings** > **Bearbeiten**, navigieren Sie zum Element **AllowedExternalRedirectUrls** (sollte NULL sein), und ändern Sie es wie folgt:

         "allowedExternalRedirectUrls": [
             "http://localhost:3000",
             "https://localhost:3000"
         ],

    Ersetzen Sie die URLs im Array durch die URLs Ihres Diensts (in diesem Beispiel `http://localhost:3000` für den lokalen Node.js-Beispieldienst). Alternativ könnten Sie auch `http://localhost:4400` für den Ripple-Dienst oder eine andere URL verwenden (je nach Konfiguration Ihrer App).
    
3. Klicken Sie oben auf der Seite auf **Lesen/Schreiben** und anschließend auf **PUT**, um Ihre Änderungen zu speichern.

    Nun müssen die gleichen Loopback-URLs noch den CORS-Positivlisteneinstellungen hinzugefügt werden:

4. Klicken Sie im [Azure-Portal] unter Ihrem mobilen App-Back-End auf **Alle Einstellungen** > **CORS**, fügen Sie die Loopback-URLs der Positivliste hinzu, und klicken Sie anschließend auf **Speichern**.

Nach der Aktualisierung des Back-Ends können Sie die neuen Loopback-URLs in Ihrer App verwenden.

<!-- URLs. -->
[Schnellstart von Azure Mobile Apps]: app-service-mobile-cordova-get-started.md
[Erste Schritte mit der Authentifizierung]: app-service-mobile-cordova-get-started-users.md
[Hinzufügen von Authentifizierung zu Ihrer App]: app-service-mobile-cordova-get-started-users.md

[JavaScript-SDKs für Azure Mobile Apps]: https://www.npmjs.com/package/azure-mobile-apps-client
[Dokumentation „Query-Objekt“]: https://msdn.microsoft.com/de-DE/library/azure/jj613353.aspx

<!---HONumber=AcomDC_0629_2016-->