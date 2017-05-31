---
title: "Azure Active Directory v2.0 – Node.js-Web-App – Anmeldung | Microsoft-Dokumentation"
description: "Informationen zum Erstellen einer Node.js-Web-App zum Anmelden eines Benutzers über sowohl ein persönliches Microsoft-Konto als auch ein Geschäfts-, Schul- oder Unikonto."
services: active-directory
documentationcenter: nodejs
author: brandwe
manager: mbaldwin
editor: 
ms.assetid: 1b889e72-f5c3-464a-af57-79abf5e2e147
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 05/13/2017
ms.author: brandwe
ms.custom: aaddev
ms.translationtype: Human Translation
ms.sourcegitcommit: 5e92b1b234e4ceea5e0dd5d09ab3203c4a86f633
ms.openlocfilehash: a77337b582f337723b9e4b1befb2c638870a1c4b
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
# <a name="add-sign-in-to-a-nodejs-web-app"></a>Hinzufügen der Anmeldung zu einer Node.js-Web-App

> [!NOTE]
> Nicht alle Azure Active Directory-Szenarien und -Features unterstützen den v2.0-Endpunkt. Lesen Sie die Informationen zu den [Einschränkungen des v2.0-Endpunkts](active-directory-v2-limitations.md), um herauszufinden, ob Sie den v2.0-Endpunkt oder v1.0-Endpunkt verwenden sollten.
> 

In diesem Tutorial verwenden wir Passport für die folgenden Aufgaben:

* Anmelden des Benutzers bei einer Web-App mithilfe von Azure Active Directory (Azure AD) und des v2.0-Endpunkts.
* Anzeigen einiger Informationen zum Benutzer
* Abmelden des Benutzers von der App

**Passport** ist eine Authentifizierungs-Middleware für Node.js. Das flexible und modular aufgebaute Passport kann unauffällig in jede Express- oder Restify-basierte Webanwendung integriert werden. In Passport unterstützt ein umfassender Satz an Strategien eine Authentifizierung mittels eines Benutzernamens und Kennworts, Facebook, Twitter und anderer Optionen. Wir haben eine Strategie für Azure AD entwickelt. In diesem Artikel erfahren Sie, wie Sie das Modul installieren und dann das Azure AD-Plug-In `passport-azure-ad` hinzufügen.

## <a name="download"></a>Download
Der Code für dieses Tutorial wird [auf GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs)verwaltet. Um dem Tutorial folgen zu können, laden Sie [das App-Gerüst als ZIP-Datei herunter](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs/archive/skeleton.zip), oder klonen Sie das Gerüst:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs.git```

Die fertige Anwendung wird Ihnen zudem am Ende dieses Tutorials bereitgestellt.

## <a name="1-register-an-app"></a>1. Registrieren einer App
Erstellen Sie eine neue App auf [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), oder führen Sie die folgenden [detaillierten Schritte](active-directory-v2-app-registration.md) aus, um eine App zu registrieren. Stellen Sie sicher, dass Sie:

* die **Anwendungs-ID** kopieren, die Ihrer App zugewiesen ist. Sie benötigen sie für dieses Tutorial.
* die **Web** -Plattform für Ihre App hinzufügen.
* den **Umleitungs-URI** aus dem Portal kopieren. Sie müssen den URI-Standardwert `urn:ietf:wg:oauth:2.0:oob` verwenden.

## <a name="2-add-prerequisities-to-your-directory"></a>2. Hinzufügen erforderlicher Komponenten zu Ihrem Verzeichnis
Wechseln Sie an einer Eingabeaufforderung vom aktuellen Verzeichnis zu Ihrem Stammordner, sofern noch nicht geschehen. Führen Sie die folgenden Befehle aus:

* `npm install express`
* `npm install ejs`
* `npm install ejs-locals`
* `npm install restify`
* `npm install mongoose`
* `npm install bunyan`
* `npm install assert-plus`
* `npm install passport`
* `npm install webfinger`
* `npm install body-parser`
* `npm install express-session`
* `npm install cookie-parser`

Zusätzlich verwenden wir `passport-azure-ad` im Gerüst des Schnellstarts:

* `npm install passport-azure-ad`

Dadurch werden die Bibliotheken installiert, die `passport-azure-ad` nutzt.

## <a name="3-set-up-your-app-to-use-the-passport-node-js-strategy"></a>3. Einrichten Ihrer App zur Nutzung der Strategie „passport-node-js“
Richten Sie die Express-Middleware für die Verwendung des OpenID Connect-Authentifizierungsprotokolls ein. Passport verwenden Sie u.a. für das Ausgeben von Anmelde- und Abmeldeanforderungen, das Verwalten der Benutzersitzung und das Abrufen der Benutzerinformationen.

1.  Öffnen Sie im Stammverzeichnis des Projekts die Datei „Config.js“. Geben Sie im Abschnitt `exports.creds` die Konfigurationswerte Ihrer App ein.
  
  * `clientID`: Die **Anwendungs-ID**, die Ihrer App im Azure-Portal zugewiesen ist.
  * `returnURL`: Der **Umleitungs-URI**, den Sie im Portal eingegeben haben.
  * `clientSecret`: Das Geheimnis, das Sie im Portal generiert haben.

2.  Öffnen Sie im Stammverzeichnis des Projekts die Datei „App.js“. Um die Strategie „OIDCStrategy“ im Funktionsumfang von `passport-azure-ad` aufzurufen, fügen Sie den folgenden Aufruf hinzu:

  ```JavaScript
  var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

  // Add some logging
  var log = bunyan.createLogger({
      name: 'Microsoft OIDC Example Web Application'
  });
  ```

3.  Verwenden Sie die gerade angegebene Strategie zum Verarbeiten von Anmeldeanforderungen:

  ```JavaScript
  // Use the OIDCStrategy within Passport (section 2)
  //
  //   Strategies in Passport require a `validate` function. The function accepts
  //   credentials (in this case, an OpenID identifier), and invokes a callback
  //   with a user object.
  passport.use( new OIDCStrategy({
      callbackURL: config.creds.returnURL,
      realm: config.creds.realm,
      clientID: config.creds.clientID,
      clientSecret: config.creds.clientSecret,
      oidcIssuer: config.creds.issuer,
      identityMetadata: config.creds.identityMetadata,
      responseType: config.creds.responseType,
      responseMode: config.creds.responseMode,
      skipUserProfile: config.creds.skipUserProfile
      scope: config.creds.scope
    },
    function(iss, sub, profile, accessToken, refreshToken, done) {
      log.info('Example: Email address we received was: ', profile.email);
      // Asynchronous verification, for effect...
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

Passport verwendet ein ähnliches Muster für alle Strategien (Twitter, Facebook usw.). Beim Schreiben von Strategien wird dieses Muster befolgt. Übergeben Sie die Strategie als `function()`, die ein Token und `done` als Parameter verwendet. Die Strategie wird zurückgegeben, nachdem sie alle ihre Aufgaben erledigt hat. Speichern Sie den Benutzer und das Token, damit Sie beides nicht mehr erfragen müssen.

  > [!IMPORTANT]
  > Der vorherige Code verwendet Benutzer, die sich bei Ihrem Server authentifizieren können. Dies wird als automatische Registrierung bezeichnet. Auf Produktionsservern erlauben Sie den Zugriff erst, nachdem ein von Ihnen gewählter Registrierungsvorgang durchlaufen wurde. Dies ist normalerweise das Muster bei Apps im Privatkundensegment. Die App ermöglicht möglicherweise die Registrierung bei Facebook, fordert aber anschließend zur Eingabe weiterer Informationen auf. Wenn Sie kein Befehlszeilenprogramm für dieses Tutorial verwenden würden, könnten Sie die E-Mail-Adresse aus dem Tokenobjekt extrahieren, das zurückgegeben wird. Dann könnten Sie den Benutzer um die Eingabe zusätzlicher Informationen bitten. Da es sich um einen Testserver handelt, fügen Sie den Benutzer direkt der Datenbank im Arbeitsspeicher hinzu.
  > 
  > 

4.  Fügen Sie, wie von Passport gefordert, die Methoden hinzu, mit denen Sie angemeldete Benutzer nachverfolgen. Dies umfasst die Serialisierung und Deserialisierung von Informationen des Benutzers:

  ```JavaScript

  // Passport session setup (section 2)

  //   To support persistent login sessions, Passport needs to be able to
  //   serialize users into, and deserialize users out of, the session. Typically,
  //   this is as simple as storing the user ID when serializing, and finding
  //   the user by ID when deserializing.
  passport.serializeUser(function(user, done) {
    done(null, user.email);
  });

  passport.deserializeUser(function(id, done) {
    findByEmail(id, function (err, user) {
      done(err, user);
    });
  });

  // Array to hold signed-in users
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

5.  Fügen Sie den Code zum Laden des Express-Moduls hinzu. Sie verwenden die standardmäßigen Muster für „/views“ und „/routes“, die Express bietet:

  ```JavaScript

  // Set up Express (section 2)

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

6.  Fügen Sie die POST-Routen hinzu, die die eigentlichen Anmeldeanforderungen an das Modul `passport-azure-ad` übergeben:

  ```JavaScript

  // Auth routes (section 3)

  // GET /auth/openid
  //   Use passport.authenticate() as route middleware to authenticate the
  //   request. The first step in OpenID authentication involves redirecting
  //   the user to the user's OpenID provider. After authenticating, the OpenID
  //   provider redirects the user back to this application at
  //   /auth/openid/return.

  app.get('/auth/openid',
    passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
    function(req, res) {
      log.info('Authentication was called in the sample');
      res.redirect('/');
    });

  // GET /auth/openid/return
  //   Use passport.authenticate() as route middleware to authenticate the
  //   request. If authentication fails, the user is redirected back to the
  //   sign-in page. Otherwise, the primary route function is called.
  //   In this example, it redirects the user to the home page.
  app.get('/auth/openid/return',
    passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
    function(req, res) {

      res.redirect('/');
    });

  // POST /auth/openid/return
  //   Use passport.authenticate() as route middleware to authenticate the
  //   request. If authentication fails, the user is redirected back to the
  //   sign-in page. Otherwise, the primary route function is called. 
  //   In this example, it redirects the user to the home page.

  app.post('/auth/openid/return',
    passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
    function(req, res) {

      res.redirect('/');
    });
  ```

## <a name="4-use-passport-to-issue-sign-in-and-sign-out-requests-to-azure-ad"></a>4. Verwenden von Passport zum Ausgeben von An- und Abmeldeanforderungen für Azure AD
Ihre App ist nun für die Kommunikation mit dem v2.0-Endpunkt über das Authentifizierungsprotokoll OpenID Connect eingerichtet. Die Strategie `passport-azure-ad` kümmert sich um alle Details der Erstellung von Authentifizierungsnachrichten, die Überprüfung der Azure AD-Token und Verwaltung der Benutzersitzung. Sie müssen jetzt Ihren Benutzern nur noch die An- und Abmeldung ermöglichen und weitere Informationen zum angemeldeten Benutzer sammeln.

1.  Fügen Sie die Methoden **default**, **login**, **account** und **logout** Ihrer Datei „App.js“ hinzu:

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
      log.info('Login was called in the sample');
      res.redirect('/');
  });

  app.get('/logout', function(req, res){
    req.logout();
    res.redirect('/');
  });

  ```

  Es folgen die Details:
    
    * Die Route `/` wird zur Ansicht „index.ejs“ umleitet. Sie übergibt den Benutzer in der Anforderung (sofern vorhanden).
    * Die Route `/account` stellt zunächst *sicher, dass Sie authentifiziert werden* (dies implementieren Sie im folgenden Code). Dann übergibt sie den Benutzer in der Anforderung. Dadurch erhalten Sie weitere Informationen zum Benutzer.
    * Die Route `/login` ruft den Authentifikator `azuread-openidconnect` von `passport-azuread` ab. Falls nicht erfolgreich, leitet die Route den Benutzer wieder an `/login` zurück.
    * Die Route `/logout` ruft die Ansicht „logout.ejs“ (und Route) auf. Dadurch werden Cookies gelöscht, und der Benutzer kehrt zu „index.ejs“ zurück.

2.  Fügen Sie die **EnsureAuthenticated**-Methode hinzu, die Sie zuvor in `/account` verwendet haben:

  ```JavaScript

  // Route middleware to ensure the user is authenticated (section 4)

  //   Use this route middleware on any resource that needs to be protected. If
  //   the request is authenticated (typically via a persistent login session),
  //   the request proceeds. Otherwise, the user is redirected to the
  //   sign-in page.
  function ensureAuthenticated(req, res, next) {
    if (req.isAuthenticated()) { return next(); }
    res.redirect('/login')
  }

  ```

3.  Erstellen Sie in „App.js“ den Server:

  ```JavaScript

  app.listen(3000);

  ```


## <a name="5-create-the-views-and-routes-in-express-that-you-show-your-user-on-the-website"></a>5. Erstellen von Ansichten und Routen in Express, die dem Benutzer auf der Website angezeigt werden
Fügen Sie die Routen und Ansichten hinzu, die dem Benutzer Informationen anzeigen. Die Routen und Ansichten berücksichtigen ebenfalls die Routen `/logout` und `/login`, die Sie erstellt haben.

1. Erstellen Sie im Stammverzeichnis die Route `/routes/index.js`.

  ```JavaScript

  /*
  * GET home page.
  */

  exports.index = function(req, res){
    res.render('index', { title: 'Express' });
  };
  ```

2.  Erstellen Sie im Stammverzeichnis die Route `/routes/user.js`.

  ```JavaScript

  /*
  * GET users listing.
  */

  exports.list = function(req, res){
    res.send("respond with a resource");
  };
  ```

  `/routes/index.js` und `/routes/user.js` sind einfache Routen, die lediglich die Anforderung an die Ansichten übergeben, einschließlich des Benutzers, falls vorhanden.

3.  Erstellen Sie im Stammverzeichnis die Ansicht `/views/index.ejs`. Diese Seite ruft die Methoden **login** und **logout** auf. Kontoinformationen können Sie auch in der Ansicht `/views/index.ejs` erfassen. Sie können das bedingte `if (!user)` verwenden, da der Benutzer über die Anforderung übergeben wird. Es ist der Beweis, dass sich ein Benutzer angemeldet hat.

  ```JavaScript
  <% if (!user) { %>
      <h2>Welcome! Please sign in.</h2>
      <a href="/login">Sign in</a>
  <% } else { %>
      <h2>Hello, <%= user.displayName %>.</h2>
      <a href="/account">Account info</a></br>
      <a href="/logout">Sign out</a>
  <% } %>
  ```

4.  Erstellen Sie im Stammverzeichnis die Ansicht `/views/account.ejs`. In der Ansicht `/views/account.ejs` können Sie zusätzliche Informationen anzeigen, die `passport-azuread` der Anforderung des Benutzers hinzufügt.

  ```Javascript
  <% if (!user) { %>
      <h2>Welcome! Please sign in.</h2>
      <a href="/login">Sign in</a>
  <% } else { %>
  <p>displayName: <%= user.displayName %></p>
  <p>givenName: <%= user.name.givenName %></p>
  <p>familyName: <%= user.name.familyName %></p>
  <p>UPN: <%= user._json.upn %></p>
  <p>Profile ID: <%= user.id %></p>
  <p>Full Claimes</p>
  <%- JSON.stringify(user) %>
  <p></p>
  <a href="/logout">Sign out</a>
  <% } %>
  ```

5.  Fügen Sie ein Layout hinzu. Erstellen Sie im Stammverzeichnis die Ansicht `/views/layout.ejs`.

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
              <a href="/login">Sign in</a>
              </p>
          <% } else { %>
              <p>
              <a href="/">Home</a> |
              <a href="/account">Account</a> |
              <a href="/logout">Sign out</a>
              </p>
          <% } %>
          <%- body %>
      </body>
  </html>
  ```

6.  Führen Sie `node app.js` aus, um Ihre App zu erstellen und auszuführen. Wechseln Sie anschließend zu `http://localhost:3000`.

7.  Melden Sie sich entweder mit einem persönlichen Microsoft-Konto oder einem Geschäfts-, Schul- oder Unikonto an. Beachten Sie, dass die Identität des Benutzers in der Liste „/account“ wiedergegeben wird. 

Sie haben jetzt eine Web-App, die von branchenüblichen Protokollen geschützt wird. Sie können Benutzer in Ihrer App anhand ihres persönlichen Kontos bzw. Geschäfts-, Schul- oder Unikontos authentifizieren.

## <a name="next-steps"></a>Nächste Schritte
Als Referenz stellen wir das vollständige Beispiel (ohne Ihre Konfigurationswerte) [als ZIP-Datei](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs/archive/complete.zip)bereit. Sie können sie auch von GitHub klonen:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs.git```

Sie können als Nächstes mit den Themen für fortgeschrittenere Benutzer fortfahren. Wie wäre es zum Beispiel mit Folgendem:

[Schützen einer Node.js-Web-API mit dem v2.0-Endpunkt](active-directory-v2-devquickstarts-node-api.md)

Hier sind einige zusätzlichen Ressourcen:

* [Azure AD v2.0-Entwicklerhandbuch](active-directory-appmodel-v2-overview.md)
* [StackOverflow-Tag „azure-active-directory“](http://stackoverflow.com/questions/tagged/azure-active-directory)

### <a name="get-security-updates-for-our-products"></a>Abrufen von Sicherheitsupdates für unsere Produkte
Wir empfehlen Ihnen, sich zu registrieren, um bei Auftreten von Sicherheitsvorfällen benachrichtigt zu werden. Abonnieren Sie auf der Seite [Technische Sicherheitsbenachrichtigungen von Microsoft](https://technet.microsoft.com/security/dd252948) den Microsoft-Sicherheitsnewsletter.


