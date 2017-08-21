
## <a name="setting-up-your-web-server-or-project"></a>Einrichten Ihres Webservers oder Projekts

> Möchten Sie stattdessen das Visual Studio-Projekt dieses Beispiels herunterladen? [Laden Sie ein Projekt herunter](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/master.zip), und fahren Sie mit dem Schritt [Konfiguration](#create-an-application-express) fort, um das Codebeispiel vor der Ausführung zu konfigurieren.

## <a name="prerequisites"></a>Voraussetzungen
Zum Ausführen dieser geführten Einrichtung ist ein lokaler Webserver wie z.B. [http-server](https://www.npmjs.com/package/http-server/), [Node.js](https://nodejs.org/en/download/), [.NET Core](https://www.microsoft.com/net/core) oder eine IIS Express-Integration in [Visual Studio 2017](https://www.visualstudio.com/downloads/) erforderlich. 

Die Anweisungen in diesem Leitfaden basieren auf Visual Studio 2017, aber Sie können auch eine andere Entwicklungsumgebung oder einen HTML-/JavaScript-Editor verwenden.


## <a name="create-your-project-visual-studio-only"></a>Erstellen Ihres Projekts (nur Visual Studio)

Wenn Sie Visual Studio verwenden und ein neues Projekt erstellen, führen Sie die folgenden Schritte aus, um eine neue Visual Studio-Projektmappe zu erstellen:
1.  In Visual Studio:  `File` > `New` > `Project`
2.  Wählen Sie unter `Visual C#\Web` die Option `ASP.NET Web Application (.NET Framework)` aus.
3.  Geben Sie der Anwendung einen Namen, und klicken Sie auf *OK*.
4.  Wählen Sie unter `New ASP.NET Web Application` die Option `Empty` aus.


## <a name="create-your-single-page-applications-ui"></a>Erstellen der Benutzeroberfläche für die Einzelseitenanwendung
1.  Erstellen Sie eine Datei „index.html“ für Ihre JavaScript-Einzelseitenanwendung. Wenn Sie Visual Studio verwenden, wählen Sie das Projekt aus (Stammordner des Projekts), klicken Sie mit der rechten Maustaste, und wählen Sie Folgendes aus: `Add` > `New Item` > `HTML page`. Geben Sie der Datei den Namen „index.html“.
2.  Fügen Sie Ihrer Seite den folgenden Code hinzu:
```html
<!DOCTYPE html>
<html>
    <head>
        <link href="//maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
        <title>JavaScript SPA Guided Setup</title>
    </head>
    <body style="margin: 40px">
        <button id="callGraphButton" type="button" class="btn btn-primary" onclick="callGraphAPI()">Call Microsoft Graph API</button>
        <div id="errorMessage" class="text-danger"></div>
        <div class="hidden">
            <h3>Graph API Call Response</h3>
            <pre class="well" id="graphResponse"></pre>
        </div>
        <div class="hidden">
            <h3>Access Token</h3>
            <pre class="well" id="accessToken"></pre>
        </div>
        <div class="hidden">
            <h3>ID Token Claims</h3>
            <pre class="well" id="userInfo"></pre>
        </div>
        <button id="signOutButton" type="button" class="btn btn-primary hidden" onclick="signOut()">Sign out</button>

        <script src="//secure.aadcdn.microsoftonline-p.com/lib/0.1.1/js/msal.min.js"></script>
        <script type="text/javascript" src="msalconfig.js"></script>
    
        <!-- The 'bluebird' and 'fetch' references below are required if you need to run this application on Internet Explorer -->
        <script src="//cdnjs.cloudflare.com/ajax/libs/bluebird/3.3.4/bluebird.min.js"></script>
        <script src="//cdnjs.cloudflare.com/ajax/libs/fetch/2.0.3/fetch.min.js"></script>
    </body>
</html>
````
