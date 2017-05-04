---
title: "Erste Schritte für das Anmelden und Abmelden bei Azure AD mit node.js | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie eine Express-MVC-Web-App mit node.js erstellen, die für die Anmeldung in Azure AD integriert wird."
services: active-directory
documentationcenter: nodejs
author: xerners
manager: mbaldwin
editor: 
ms.assetid: 81deecec-dbe2-4e75-8bc0-cf3788645f99
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 01/07/2017
ms.author: xerners
translationtype: Human Translation
ms.sourcegitcommit: 8c4e33a63f39d22c336efd9d77def098bd4fa0df
ms.openlocfilehash: 8d91d551cbb508485ca27d77b23eb319293e4237
ms.lasthandoff: 04/20/2017


---
# <a name="nodejs-web-app-sign-in-and-sign-out-with-azure-ad"></a>An- und Abmeldung bei NodeJS-Web-Apps mit Azure AD
Hier wird Passport für Folgendes verwendet:

* Anmelden des Benutzers bei der App mit Azure Active Directory (Azure AD).
* Anzeigen einiger Informationen zum Benutzer
* Abmelden des Benutzers von der App

Passport ist eine Authentifizierungs-Middleware für Node.js. Das flexible und modular aufgebaute Passport kann unauffällig in jede Express- oder Restify-basierte Webanwendung integriert werden. Ein umfassender Satz an Strategien unterstützt die Authentifizierung mittels eines Benutzernamens und Kennworts in Facebook, Twitter und anderen Anwendungen. Wir haben eine Strategie für Microsoft Azure Active Directory entwickelt. Dieses Modul installieren Sie nun und fügen dann das Microsoft Azure Active Directory-Plug-In `passport-azure-ad` hinzu.

Führen Sie dazu die folgenden Schritte aus:

1. Registrieren einer App
2. Legen Sie fest, dass Ihre App die Strategie `passport-azure-ad` verwendet.
3. Verwenden Sie Passport zur Ausgabe von An- und Abmeldeanforderungen für Azure AD.
4. Drucken Sie Informationen zum Benutzer aus.

Der Code für dieses Tutorial wird [auf GitHub](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS)verwaltet.  Um folgen zu können, laden Sie [das App-Gerüst als ZIP-Datei herunter](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/skeleton.zip) oder klonen Sie das Gerüst:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS.git```

Die fertige Anwendung wird außerdem am Ende dieses Lernprogramms bereitgestellt.

## <a name="step-1-register-an-app"></a>Schritt 1: Registrieren einer App
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.

2. Wählen im Menü oben auf der Seite Ihr Konto aus. Wählen Sie in der **Verzeichnisliste** den Active Directory-Mandanten aus, bei dem Sie Ihre Anwendung registrieren möchten.

3. Wählen Sie im Menü auf der linken Seite **Weitere Dienste** und dann **Azure Active Directory**.

4. Wählen Sie **App-Registrierungen** und dann **Hinzufügen** aus.

5. Folgen Sie den Bildschirmaufforderungen, und erstellen Sie eine neue **Webanwendung** und/oder **Web-API**.
  * Am **Namen** der Anwendung sollten die Benutzer die Funktion der Anwendung ablesen können.

  * Die **Anmelde-URL** ist die Basis-URL Ihrer Anwendung.  Der Standardwert des Gerüsts lautet `http://localhost:3000/auth/openid/return``.

6. Nach Abschluss der Registrierung weist Azure AD Ihrer App eine eindeutige Anwendungs-ID zu. Diesen Wert benötigen Sie in den nächsten Abschnitten. Daher sollten Sie ihn von der Anwendungsseite kopieren.
7. Aktualisieren Sie auf der Seite **Einstellungen** -> **Eigenschaften** für Ihre Anwendung den App-ID-URI. Die **App-ID-URI** ist eine eindeutige Kennung für die Anwendung. Üblicherweise wird das Format `https://<tenant-domain>/<app-name>` verwendet, z. B.: `https://contoso.onmicrosoft.com/my-first-aad-app`.

## <a name="step-2-add-prerequisites-to-your-directory"></a>Schritt 2: Hinzufügen von Voraussetzungen zu Ihrem Verzeichnis
1. Wechseln Sie über die Befehlszeile vom Verzeichnis auf Ihren Stammordner, wenn dies noch nicht der Fall ist, und führen Sie dann die folgenden Befehle aus:

    * `npm install express`
    * `npm install ejs`
    * `npm install ejs-locals`
    * `npm install restify`
    * `npm install mongoose`
    * `npm install bunyan`
    * `npm install assert-plus`
    * `npm install passport`

2. Zudem benötgien Sie `passport-azure-ad`:
    * `npm install passport-azure-ad`

Dadurch werden die Bibliotheken installiert, von denen `passport-azure-ad` abhängt.

## <a name="step-3-set-up-your-app-to-use-the-passport-node-js-strategy"></a>Schritt 3: Einrichten Ihrer App zur Nutzung der „passport-node-js“-Strategie
Hier konfigurieren wir Express für die Verwendung des OpenID Connect-Authentifizierungsprotokolls.  Passport wird für verschiedene Zwecke verwendet, unter anderem für die Ausgabe von Anmelde- und Abmeldeanforderungen, für die Verwaltung der Benutzerssitzungen und für das Abrufen der Benutzerinformationen.

1. Öffnen Sie zunächst die Datei `config.js` im Stammverzeichnis des Projekts, und geben Sie dann die Konfigurationswerte Ihrer App im Abschnitt `exports.creds` ein.

  * `clientID` ist die **Anwendungs-ID** , die Ihrer App im Registrierungsportal zugewiesen ist.

  * `returnURL` ist der **Umleitungs-URI** , den Sie im Portal eingegeben haben.

  * `clientSecret` ist der geheime Schlüssel, den Sie im Portal generiert haben.

2. Öffnen Sie als Nächstes die Datei `app.js` im Stammverzeichnis des Projekts. Fügen Sie den folgenden Aufruf hinzu, um die Strategie `OIDCStrategy` aufzurufen, die zu `passport-azure-ad` gehört.

    ```JavaScript
    var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

    // add a logger

    var log = bunyan.createLogger({
    name: 'Microsoft OIDC Example Web Application'
    });
    ```

3. Verwenden Sie danach die Strategie, auf die gerade verwiesen wurde, um die Anmeldeanforderungen zu verarbeiten.

    ```JavaScript
    // Use the OIDCStrategy within Passport. (Section 2)
    //
    //   Strategies in passport require a `validate` function that accepts
    //   credentials (in this case, an OpenID identifier), and invokes a callback
    //   with a user object.
    passport.use(new OIDCStrategy({
        callbackURL: config.creds.returnURL,
        realm: config.creds.realm,
        clientID: config.creds.clientID,
        clientSecret: config.creds.clientSecret,
        oidcIssuer: config.creds.issuer,
        identityMetadata: config.creds.identityMetadata,
        skipUserProfile: config.creds.skipUserProfile,
        responseType: config.creds.responseType,
        responseMode: config.creds.responseMode
    },
    function(iss, sub, profile, accessToken, refreshToken, done) {
        if (!profile.email) {
        return done(new Error("No email found"), null);
        }
        // asynchronous verification, for effect...
        process.nextTick(function () {
        findByEmail(profile.email, function(err, user) {
            if (err) {
            return done(err);
            }
            if (!user) {
            // "Auto-registration"
            users.push(profile);
            return done(null, profile);
            }
            return done(null, user);
        });
        });
    }
    ));
    ```
Passport verwendet ein ähnliches Muster für alle Strategien (Twitter, Facebook usw.), das beim Schreiben aller Strategien eingehalten wird. Beim Betrachten der Strategie sehen Sie, dass ein function-Element übergeben wird, das Token- und done-Elemente als Parameter aufweist. Die Strategie kehrt wieder an den Ausgangspunkt zurück, sobald die Arbeit abgeschlossen ist. Dann speichern wir den Benutzer und das Token, damit wir beides nicht mehr erfragen müssen.

    > [!IMPORTANT]
    > The previous code takes any user that happens to authenticate to our server. This is known as auto-registration. We recommend that you don't let anyone authenticate to a production server without first having them register via a process that you decide on. This is usually the pattern you see in consumer apps, which allow you to register with Facebook but then ask you to provide additional information. If this weren't a sample application, we could have extracted the user's email address from the token object that is returned and then asked the user to fill out additional information. Because this is a test server, we add them to the in-memory database.


4. Als Nächstes fügen Sie die Methoden hinzu, mit denen die angemeldeten Benutzer nachverfolgt werden können, wie es von Passport gefordert wird. Diese Methoden umfassen die Serialisierung und Deserialisierung von Informationen des Benutzers.

    ```JavaScript

            // Passport session setup. (Section 2)

            //   To support persistent sign-in sessions, Passport needs to be able to
            //   serialize users into the session and deserialize them out of the session. Typically,
            //   this is done simply by storing the user ID when serializing and finding
            //   the user by ID when deserializing.
            passport.serializeUser(function(user, done) {
            done(null, user.email);
            });

            passport.deserializeUser(function(id, done) {
            findByEmail(id, function (err, user) {
                done(err, user);
            });
            });

            // array to hold signed-in users
            var users = [];

            var findByEmail = function(email, fn) {
            for (var i = 0, len = users.length; i < len; i++) {
                var user = users[i];
            log.info('we are using user: ', user);
                if (user.email === email) {
                return fn(null, user);
                }
            }
            return fn(null, null);
            };
    ```

5.  Als Nächstes fügen Sie den Code hinzu, um das Express-Modul zu laden. Hier verwend wir die standardmäßigen Muster für „/views“ und „/routes“, die Express bietet.

    ```JavaScript

        // configure Express (section 2)

            var app = express();
            app.configure(function() {
          app.set('views', __dirname + '/views');
          app.set('view engine', 'ejs');
          app.use(express.logger());
          app.use(express.methodOverride());
          app.use(cookieParser());
          app.use(expressSession({ secret: 'keyboard cat', resave: true, saveUninitialized: false }));
          app.use(bodyParser.urlencoded({ extended : true }));
          // Initialize Passport!  Also use passport.session() middleware, to support
          // persistent login sessions (recommended).
          app.use(passport.initialize());
          app.use(passport.session());
          app.use(app.router);
          app.use(express.static(__dirname + '/../../public'));
        });

    ```

6. Fügen Sie abschließend die Routen hinzu, die die eigentlichen Anmeldeanforderungen an das Modul `passport-azure-ad` übergeben:

       
       ```JavaScript
 
        // Our Auth routes (section 3)

        // GET /auth/openid
        //   Use passport.authenticate() as route middleware to authenticate the
        //   request. The first step in OpenID authentication involves redirecting
        //   the user to their OpenID provider. After authenticating, the OpenID
        //   provider redirects the user back to this application at
        //   /auth/openid/return.
        app.get('/auth/openid',
        passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
        function(req, res) {
            log.info('Authentication was called in the Sample');
            res.redirect('/');
        });

            // GET /auth/openid/return
            //   Use passport.authenticate() as route middleware to authenticate the
            //   request. If authentication fails, the user is redirected back to the
            //   sign-in page. Otherwise, the primary route function is called,
            //   which, in this example, redirects the user to the home page.
            app.get('/auth/openid/return',
              passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
              function(req, res) {
                log.info('We received a return from AzureAD.');
                res.redirect('/');
              });

            // POST /auth/openid/return
            //   Use passport.authenticate() as route middleware to authenticate the
            //   request. If authentication fails, the user is redirected back to the
            //   sign-in page. Otherwise, the primary route function is called,
            //   which, in this example, redirects the user to the home page.
            app.post('/auth/openid/return',
              passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
              function(req, res) {
                log.info('We received a return from AzureAD.');
                res.redirect('/');
              });
       ```


## <a name="step-4-use-passport-to-issue-sign-in-and-sign-out-requests-to-azure-ad"></a>Schritt 4: Verwenden von Passport zum Übergeben von An- und Abmeldeanforderungen an Azure AD
Ihre Anwendung ist nun ordnungsgemäß für die Kommunikation mit dem Endpunkt über das Authentifizierungsprotokoll OpenID Connect konfiguriert.  `passport-azure-ad` kümmert sich um alle Details der Erstellung von Authentifizierungsnachrichten, die Überprüfung der Azure AD-Token und Verwaltung von Benutzersitzungen. Sie müssen Ihren Benutzern nur noch die An- und Abmeldung ermöglichen und zusätzliche Informationen zu den angemeldeten Benutzern sammeln.

1. Lassen Sie uns zunächst Ihrer Datei `app.js` die Standard-, Anmelde-, Konto- und Abmeldemethoden hinzufügen:

    ```JavaScript

        //Routes (section 4)

        app.get('/', function(req, res){
          res.render('index', { user: req.user });
        });

        app.get('/account', ensureAuthenticated, function(req, res){
          res.render('account', { user: req.user });
        });

        app.get('/login',
          passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
          function(req, res) {
            log.info('Login was called in the Sample');
            res.redirect('/');
        });

        app.get('/logout', function(req, res){
          req.logout();
          res.redirect('/');
        });

    ```

2.  Betrachten Sie diese im Detail:

  * Die Route `/` leitet an die Ansicht „index.ejs“ weiter und übergibt den Benutzer in der Anforderung (falls vorhanden).
  * Die Route `/account` *stellt sicher, dass wir authentifiziert sind* (wird im folgenden Beispiel implementiert) und übergibt dann den Benutzer in der Anforderung, damit wir zusätzliche Informationen zum Benutzer abrufen können.
  * Die Route `/login` ruft unsere azuread-openidconnect-Authentifizierung von `passport-azuread` auf. Wenn dies nicht erfolgreich ist, leitet die Route den Benutzer wieder an „/login“ zurück.
  * `/logout` ruft einfach „logout.ejs“ (und die Route) auf, um Cookies zu löschen, und leitet dann den Benutzer zu „index.ejs“ zurück.

3. Im letzten Teil von `app.js` fügen wir die **EnsureAuthenticated**-Methode hinzu, die in `/account` oben verwendet wird.

    ```JavaScript

        // Simple route middleware to ensure user is authenticated. (section 4)

        //   Use this route middleware on any resource that needs to be protected. If
        //   the request is authenticated (typically via a persistent sign-in session),
        //   the request proceeds. Otherwise, the user is redirected to the
        //   sign-in page.
        function ensureAuthenticated(req, res, next) {
          if (req.isAuthenticated()) { return next(); }
          res.redirect('/login')
        }
    ```

4. Schließlich erstellen wir den Server selbst in `app.js`:

```JavaScript

        app.listen(3000);

```


## <a name="step-5-to-display-our-user-in-the-website-create-the-views-and-routes-in-express"></a>Schritt 5: Erstellen von Ansichten und Routen in Express, um Benutzer auf der Website anzuzeigen
`app.js` ist jetzt vollständig. Wir müssen einfach die Routen und Ansichten hinzufügen, die die Informationen für die Benutzer anzeigen und die erstellten Routen `/logout` und `/login` verarbeiten.

1. Erstellen der Route `/routes/index.js` im Stammverzeichnis

    ```JavaScript
                /*
                 * GET home page.
                 */

                exports.index = function(req, res){
                  res.render('index', { title: 'Express' });
                };
    ```

2. Erstellen der Route `/routes/user.js` im Stammverzeichnis

                ```JavaScript
                /*
                 * GET users listing.
                 */

                exports.list = function(req, res){
                  res.send("respond with a resource");
                };
                ```

 Diese übergeben lediglich die Anforderung an die Ansichten, einschließlich des Benutzers, falls vorhanden.

3. Erstellen Sie die Ansicht `/views/index.ejs` im Stammverzeichnis. Dies ist eine einfache Seite, die die Anmelde- und Abmeldemethoden aufruft und uns ermöglicht, Kontoinformationen zu erfassen. Beachten Sie, dass eine bedingte `if (!user)`-Anweisung verwendet werden kann, da das Übergeben des Benutzers in der Anforderung belegt, dass er ein angemeldeter Benutzer ist.

    ```JavaScript
    <% if (!user) { %>
        <h2>Welcome! Please log in.</h2>
        <a href="/login">Log In</a>
    <% } else { %>
        <h2>Hello, <%= user.displayName %>.</h2>
        <a href="/account">Account Info</a></br>
        <a href="/logout">Log Out</a>
    <% } %>
    ```

4. Erstellen Sie die Ansicht `/views/account.ejs` im Stammverzeichnis, sodass zusätzliche Informationen angezeigt werden können, die von `passport-azuread` in die Benutzeranforderung eingefügt wurden.

    ```Javascript
    <% if (!user) { %>
        <h2>Welcome! Please log in.</h2>
        <a href="/login">Log In</a>
    <% } else { %>
    <p>displayName: <%= user.displayName %></p>
    <p>givenName: <%= user.name.givenName %></p>
    <p>familyName: <%= user.name.familyName %></p>
    <p>UPN: <%= user._json.upn %></p>
    <p>Profile ID: <%= user.id %></p>
  ##Next steps  <p>Full Claimes</p>
    <%- JSON.stringify(user) %>
    <p></p>
    <a href="/logout">Log Out</a>
    <% } %>
    ```

5. Wir optimieren die Darstellung durch Hinzufügen eines Layouts. Erstellen Sie die Ansicht „/views/layout.ej“ im Stammverzeichnis.

    ```HTML

    <!DOCTYPE html>
    <html>
        <head>
            <title>Passport-OpenID Example</title>
        </head>
        <body>
            <% if (!user) { %>
                <p>
                <a href="/">Home</a> |
                <a href="/login">Log In</a>
                </p>
            <% } else { %>
                <p>
                <a href="/">Home</a> |
                <a href="/account">Account</a> |
                <a href="/logout">Log Out</a>
                </p>
            <% } %>
            <%- body %>
        </body>
    </html>
    ```

##<a name="next-steps"></a>Nächste Schritte
Erstellen und führen Sie Ihre Anwendung zum Schluss aus. Führen Sie `node app.js` aus, und navigieren Sie zu `http://localhost:3000`.

Melden Sie sich entweder mit einem persönlichen Microsoft-Konto oder einem Geschäfts- oder Schulkonto an, und beachten Sie, wie die Identität des Benutzers in der Liste "/account" dargestellt wird. Sie verfügen jetzt über eine mit branchenüblichen Protokollen gesicherte Web-App, die Benutzer mit ihren persönlichen Konten oder ihren Geschäfts- oder Schulkonten authentifizieren kann.

Als Referenz stellen wir das vollständige Beispiel (ohne Ihre Konfigurationswerte) [als ZIP-Datei](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/complete.zip)bereit. Sie können es alternativ aus GitHub klonen:

```git clone --branch complete https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS.git```

Sie können nun mit den Themen für fortgeschrittenere Benutzer fortfahren. Wie wäre es zum Beispiel mit Folgendem:

[Schützen einer Web-API mit Azure AD](active-directory-devquickstarts-webapi-nodejs.md)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]

