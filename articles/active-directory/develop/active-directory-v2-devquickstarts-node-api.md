---
title: Sichern einer Azure Active Directory v2.0-Web-API mithilfe von Node.js | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie eine Node.js-Web-API erstellen, die sowohl Token von einem persönlichen Microsoft-Konto als auch von Geschäfts-, Uni- oder Schulkonten akzeptiert."
services: active-directory
documentationcenter: nodejs
author: brandwe
manager: mbaldwin
editor: 
ms.assetid: 0b572fc1-2aaf-4cb6-82de-63010fb1941d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 05/13/2017
ms.author: brandwe
ms.custom: aaddev
ms.translationtype: Human Translation
ms.sourcegitcommit: de674af369080ad7eb608608685e293f2326c8e6
ms.openlocfilehash: d42b96b0fb02b6c2df364d5e19f3345991ee03b1
ms.contentlocale: de-de
ms.lasthandoff: 05/04/2017


---
# <a name="secure-a-web-api-by-using-nodejs"></a>Schützen einer Web-API mit Node.js
> [!NOTE]
> Nicht alle Azure Active Directory-Szenarios und -Features unterstützen den v2.0-Endpunkt. Lesen Sie die Informationen zu den [Einschränkungen des v2.0-Endpunkts](active-directory-v2-limitations.md), um herauszufinden, ob Sie den v2.0-Endpunkt oder v1.0-Endpunkt verwenden sollten.
> 
> 

Wenn Sie den Endpunkt von Azure Active Directory v2.0 (Azure AD) verwenden, können Sie [OAuth 2.0](active-directory-v2-protocols.md)-Zugriffstoken zum Schutz Ihrer Web-API verwenden. Mit OAuth 2.0-Zugriffstoken können Benutzer, die jeweils ein persönliches Microsoft-Konto sowie Geschäfts-, Uni- oder Schulkonten besitzen, sicher auf Ihre Web-API zugreifen.

*Passport* ist eine Authentifizierungs-Middleware für Node.js. Passport ist flexibel und modular und kann unauffällig in jede Express- oder Restify-basierte Webanwendung integriert werden. Ein umfassender Satz an Strategien unterstützt in Passport die Authentifizierung mittels eines Benutzernamens und Kennworts in Facebook, Twitter und anderen Optionen. Wir haben eine Strategie für Azure AD entwickelt. In diesem Artikel erfahren Sie, wie Sie das Modul installieren und dann das Azure AD-Plug-In `passport-azure-ad` hinzufügen.

## <a name="download"></a>Download
Der Code für dieses Tutorial wird [auf GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs)verwaltet. Um dem Tutorial folgen zu können, können Sie [das App-Gerüst als ZIP-Datei herunterladen](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs/archive/skeleton.zip), oder klonen Sie das Gerüst:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs.git```

Die fertige Anwendung wird Ihnen zudem am Ende dieses Tutorials bereitgestellt.

## <a name="1-register-an-app"></a>1. Registrieren einer App
Erstellen Sie eine neue App auf [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), oder führen Sie die folgenden [ausführlichen Schritte](active-directory-v2-app-registration.md) aus, um eine App zu registrieren. Stellen Sie sicher, dass Sie:

* die **Anwendungs-ID** kopieren, die Ihrer App zugewiesen ist. Sie benötigen sie für dieses Tutorial.
* Fügen Sie die **Mobile** -Plattform Ihrer App hinzu.
* Kopieren Sie den **Umleitungs-URI** aus dem Portal. Sie müssen den URI-Standardwert `urn:ietf:wg:oauth:2.0:oob` verwenden.

## <a name="2-install-nodejs"></a>2: Installieren von Node.js
Um das Beispiel für dieses Tutorial zu verwenden, müssen Sie [Node.js installieren](http://nodejs.org).

## <a name="3-install-mongodb"></a>3: Installieren von MongoDB
Um dieses Beispiel erfolgreich nutzen zu können, müssen Sie [MongoDB installieren](http://www.mongodb.org). In diesem Beispiel verwenden Sie MongoDB, damit die REST-API über Serverinstanzen hinweg persistent bleibt.

> [!NOTE]
> In diesem Artikel wird davon ausgegangen, dass Sie die Standardinstallation und -serverendpunkte für MongoDB verwenden: mongodb://localhost.
> 
> 

## <a name="4-install-the-restify-modules-in-your-web-api"></a>4: Installieren der Restify-Module in Ihrer Web-API
Zur Erstellung der REST-API verwenden wir Restify. Restify ist ein einfaches und flexibles Node.js-Anwendungsframework, das von Express abgeleitet ist. Restify verfügt über eine Reihe robuster Funktionen, die Sie zum Erstellen von REST-APIs für Connect verwenden können.

### <a name="install-restify"></a>Installieren von Restify
1.  Wechseln Sie bei einer Eingabeaufforderung in das Verzeichnis **azuread**:

    `cd azuread`

    Falls das Verzeichnis **azuread** nicht vorhanden ist, erstellen Sie es:

    `mkdir azuread`

2.  Installieren Sie Restify:

    `npm install restify`

    Die Ausgabe dieses Befehls sollte wie folgt aussehen:

    ```
    restify@2.6.1 node_modules/restify
    ├── assert-plus@0.1.4
    ├── once@1.3.0
    ├── deep-equal@0.0.0
    ├── escape-regexp-component@1.0.2
    ├── qs@0.6.5
    ├── tunnel-agent@0.3.0
    ├── keep-alive-agent@0.0.1
    ├── lru-cache@2.3.1
    ├── node-uuid@1.4.0
    ├── negotiator@0.3.0
    ├── mime@1.2.11
    ├── semver@2.2.1
    ├── spdy@1.14.12
    ├── backoff@2.3.0
    ├── formidable@1.0.14
    ├── verror@1.3.6 (extsprintf@1.0.2)
    ├── csv@0.3.6
    ├── http-signature@0.10.0 (assert-plus@0.1.2, asn1@0.1.11, ctype@0.5.2)
    └── bunyan@0.22.0(mv@0.0.5)
    ```

#### <a name="did-you-get-an-error"></a>Bei einem Fehler
Wenn Sie auf einigen Betriebssystemen den `npm`-Befehl verwenden, wird diese Meldung angezeigt: `Error: EPERM, chmod '/usr/local/bin/..'`. Nach dem Fehler wird eine Aufforderung angezeigt, die Ihnen vorschlägt, es mit der Ausführung des Kontos als Administrator zu versuchen. Verwenden Sie in diesem Fall den Befehl `sudo`, um `npm` mit einer höheren Berechtigungsstufe auszuführen.

#### <a name="did-you-get-an-error-related-to-dtrace"></a>Haben Sie einen Fehler im Zusammenhang mit DTrace erhalten?
Wenn Sie Restify installieren, wird Ihnen möglicherweise folgende Meldung angezeigt:

```Shell
clang: error: no such file or directory: 'HD/azuread/node_modules/restify/node_modules/dtrace-provider/libusdt'
make: *** [Release/DTraceProviderBindings.node] Error 1
gyp ERR! build error
gyp ERR! stack Error: `make` failed with exit code: two
gyp ERR! stack     at ChildProcess.onExit (/usr/local/lib/node_modules/npm/node_modules/node-gyp/lib/build.js:267:23)
gyp ERR! stack     at ChildProcess.EventEmitter.emit (events.js:98:17)
gyp ERR! stack     at Process.ChildProcess._handle.onexit (child_process.js:789:12)
gyp ERR! System Darwin 13.1.0
gyp ERR! command "node" "/usr/local/lib/node_modules/npm/node_modules/node-gyp/bin/node-gyp.js" "rebuild"
gyp ERR! cwd /Volumes/Development HD/azuread/node_modules/restify/node_modules/dtrace-provider
gyp ERR! node -v v0.10.11
gyp ERR! node-gyp -v v0.10.0
gyp ERR! not ok
npm WARN optional dep failed, continuing dtrace-provider@0.2.8
```

Restify verfügt über einen leistungsstarken Mechanismus zum Verfolgen von REST-Aufrufen mit DTrace. DTrace ist jedoch auf vielen Betriebssystemen nicht verfügbar. Sie können diese Fehlermeldung ignorieren.


## <a name="5-install-passportjs-in-your-web-api"></a>5: Installieren von passport.js in Ihrer Web-API
1.  Wechseln Sie an der Eingabeaufforderung in das Verzeichnis **azuread**.

2.  Installieren Sie Passport.js:

    `npm install passport`

    Die Ausgabe des Befehls sollte wie folgt aussehen:

    ```
     passport@0.1.17 node_modules\passport
    ├── pause@0.0.1
    └── pkginfo@0.2.3
    ```

## <a name="6-add-passport-azure-ad-to-your-web-api"></a>6: Hinzufügen von Passport-Azure-AD zu Ihrer Web-API
Fügen Sie als Nächstes die OAuth-Strategie mithilfe von „passport-azuread“ hinzu. `passport-azuread` ist eine Reihe von Strategien, die Azure AD mit Passport verbinden. Wir verwenden diese Strategie für Bearertoken in diesem REST-API-Beispiel.

> [!NOTE]
> OAuth 2.0 bietet zwar ein Framework, in dem alle bekannten Tokentypen ausgestellt werden können, aber es werden meist nur bestimmte Tokentypen verwendet. Bearertoken werden häufig zum Schutz von Endpunkten verwendet. Bearertoken werden in OAuth 2.0 am häufigsten ausgestellt. Bei vielen Implementierungen von OAuth 2.0 wird davon ausgegangen, dass Bearertoken der einzige ausgestellte Tokentyp sind.
> 
> 

1.  Wechseln Sie an einer Eingabeaufforderung in das Verzeichnis **azuread**.

    `cd azuread`

2.  Installieren Sie das Passport.js `passport-azure-ad`-Modul:

    `npm install passport-azure-ad`

    Die Ausgabe des Befehls sollte wie folgt aussehen:

    ```
    passport-azure-ad@1.0.0 node_modules/passport-azure-ad
    ├── xtend@4.0.0
    ├── xmldom@0.1.19
    ├── passport-http-bearer@1.0.1 (passport-strategy@1.0.0)
    ├── underscore@1.8.3
    ├── async@1.3.0
    ├── jsonwebtoken@5.0.2
    ├── xml-crypto@0.5.27 (xpath.js@1.0.6)
    ├── ursa@0.8.5 (bindings@1.2.1, nan@1.8.4)
    ├── jws@3.0.0 (jwa@1.0.1, base64url@1.0.4)
    ├── request@2.58.0 (caseless@0.10.0, aws-sign2@0.5.0, forever-agent@0.6.1, stringstream@0.0.4, tunnel-agent@0.4.1, oauth-sign@0.8.0, isstream@0.1.2, extend@2.0.1, json-stringify-safe@5.0.1, node-uuid@1.4.3, qs@3.1.0, combined-stream@1.0.5, mime-types@2.0.14, form-data@1.0.0-rc1, http-signature@0.11.0, bl@0.9.4, tough-cookie@2.0.0, hawk@2.3.1, har-validator@1.8.0)
    └── xml2js@0.4.9 (sax@0.6.1, xmlbuilder@2.6.4)
    ```

## <a name="7-add-mongodb-modules-to-your-web-api"></a>7: Hinzufügen der MongoDB-Module zu Ihrer Web-API
In diesem Beispiel verwenden wir MongoDB als Datenspeicher. 

1.  Installieren Sie Mongoose – ein weit verbreitetes Plug-In zum Verwalten von Modellen und Schemas: 

    `npm install mongoose`

2.  Installieren Sie den Datenbanktreiber für MongoDB (ebenfalls als MongoDB bezeichnet):

    `npm install mongodb`

## <a name="8-install-additional-modules"></a>8: Installieren zusätzlicher Module
Installieren Sie die weiteren erforderlichen Module.

1.  Wechseln Sie bei einer Eingabeaufforderung in das Verzeichnis **azuread**:

    `cd azuread`

2.  Geben Sie die folgenden Befehle ein. Die Befehle installieren die folgenden Module in Ihrem „node_modules“-Verzeichnis:

    *   `npm install crypto`
    *   `npm install assert-plus`
    *   `npm install posix-getopt`
    *   `npm install util`
    *   `npm install path`
    *   `npm install connect`
    *   `npm install xml-crypto`
    *   `npm install xml2js`
    *   `npm install xmldom`
    *   `npm install async`
    *   `npm install request`
    *   `npm install underscore`
    *   `npm install grunt-contrib-jshint@0.1.1`
    *   `npm install grunt-contrib-nodeunit@0.1.2`
    *   `npm install grunt-contrib-watch@0.2.0`
    *   `npm install grunt@0.4.1`
    *   `npm install xtend@2.0.3`
    *   `npm install bunyan`
    *   `npm update`

## <a name="9-create-a-serverjs-file-for-your-dependencies"></a>9: Erstellen der Datei „Server.js“ für Ihre Abhängigkeiten
Eine Server.js-Datei enthält den Großteil der Funktionen für Ihren Web-API-Server. Fügen Sie den größten Teil Ihres Codes dieser Datei hinzu. Für Produktionszwecke können Sie die Funktionalität auf kleinere Dateien aufteilen und so zum Beispiel Routen und Controller getrennt behandeln. In diesem Artikel verwenden wir zu diesem Zweck „Server.js“.

1.  Wechseln Sie bei einer Eingabeaufforderung in das Verzeichnis **azuread**:

    `cd azuread`

2.  Erstellen Sie mithilfe eines Editors Ihrer Wahl eine Server.js-Datei. Fügen Sie der Datei die folgenden Informationen hinzu:

    ```Javascript
    'use strict';
    /**
    * Module dependencies.
    */
    var util = require('util');
    var assert = require('assert-plus');
    var mongoose = require('mongoose/');
    var bunyan = require('bunyan');
    var restify = require('restify');
    var config = require('./config');
    var passport = require('passport');
    var OIDCBearerStrategy = require('passport-azure-ad').OIDCStrategy;
    ```

3.  Speichern Sie die Datei. Sie werden in Kürze auf diese Datei zurückkommen.

## <a name="10-create-a-config-file-to-store-your-azure-ad-settings"></a>10: Erstellen einer Konfigurationsdatei für die Azure AD-Einstellungen
Diese Codedatei übergibt Ihre Konfigurationsparameter aus dem Azure AD-Portal an „Passport.js“. Sie haben diese Konfigurationswerte erstellt, als Sie die Web-API zu Beginn dieses Artikels dem Portal hinzugefügt haben. Welche Werte Sie für diese Parameter eingeben sollen, erfahren Sie, nachdem Sie den Code kopiert haben.

1.  Wechseln Sie bei einer Eingabeaufforderung in das Verzeichnis **azuread**:

    `cd azuread`

2.  Erstellen Sie eine Config.js-Datei in einem Editor. Fügen Sie die folgenden Informationen hinzu:

    ```Javascript
    // Don't commit this file to your public repos. This config is for first-run.
    exports.creds = {
    mongoose_auth_local: 'mongodb://localhost/tasklist', // Your Mongo auth URI goes here.
    issuer: 'https://sts.windows.net/**<your application id>**/',
    audience: '<your redirect URI>',
    identityMetadata: 'https://login.microsoftonline.com/common/.well-known/openid-configuration' // For Microsoft, you should never need to change this.
    };

    ```



### <a name="required-values"></a>Erforderliche Werte

*   **IdentityMetadata**: Hier sucht `passport-azure-ad` nach den Konfigurationsdaten für den Identitätsanbieter (Identity Provider, IDP) und nach den Schlüsseln zum Überprüfen der JWT-Token (JSON Web Token). Wenn Sie Azure AD verwenden, sollten Sie das nicht ändern.

*   **audience**: Die Umleitungs-URI aus dem Portal.

> [!NOTE]
> Setzen Sie Ihre Schlüssel in regelmäßigen Abständen zurück. Stellen Sie sicher, dass Sie Ihre Schlüssel immer über die „openid_keys“-URL abrufen und die App auf das Internet zugreifen kann.
> 
> 

## <a name="11-add-the-configuration-to-your-serverjs-file"></a>11: Hinzufügen der Konfiguration zur Datei "Server.js"
Ihre Anwendung muss aus der eben erstellten Konfigurationsdatei gelesen werden. Fügen Sie die Konfigurationsdatei in der Anwendung hierzu als erforderliche Ressource hinzu. Legen Sie die globalen Variablen auf diejenigen fest, die sich in „Config.js“ befinden.

1.  Wechseln Sie an der Eingabeaufforderung in das Verzeichnis **azuread**:

    `cd azuread`

2.  Öffnen Sie „Server.js“ in einem Editor. Fügen Sie die folgenden Informationen hinzu:

    ```Javascript
    var config = require('./config');
    ```

3.  Fügen Sie einen neuen Abschnitt zu „Server.js“ hinzu:

    ```Javascript
    // Pass these options in to the ODICBearerStrategy.
    var options = {
    // The URL of the metadata document for your app. Put the keys for token validation from the URL found in the jwks_uri tag in the metadata.
    identityMetadata: config.creds.identityMetadata,
    issuer: config.creds.issuer,
    audience: config.creds.audience
    };
    // Array to hold signed-in users and the current signed-in user (owner).
    var users = [];
    var owner = null;
    // Your logger
    var log = bunyan.createLogger({
    name: 'Microsoft Azure Active Directory Sample'
    });
    ```

## <a name="12-add-the-mongodb-model-and-schema-information-by-using-mongoose"></a>12: Hinzufügen der Modell- und Schemainformationen von MongoDB mithilfe von Mongoose
Verbinden Sie als Nächstes diese drei Dateien in einem REST-API-Dienst.

In diesem Artikel verwenden wir MongoDB, um unsere Aufgaben zu speichern. Dies wird in *Schritt 4* erläutert.

In der Config.js-Datei, die Sie in Schritt 11 erstellt haben, heißt Ihre Datenbank *tasklist*. Diese haben Sie am Ende Ihrer mongoose_auth_local-Verbindungs-URL hinzugefügt. Sie müssen diese Datenbank nicht vorab in MongoDB erstellen. Die Datenbank wird bei der ersten Ausführung Ihrer Serveranwendung erstellt (vorausgesetzt, die Datenbank war noch nicht vorhanden).

Sie haben dem Server mitgeteilt, welche MongoDB-Datenbank verwendet werden soll. Als Nächstes müssen Sie zusätzlichen Code schreiben, um das Modell und Schema für Ihre Serveraufgaben zu erstellen.

### <a name="the-model"></a>Das Modell
Das Schemamodell ist sehr einfach gehalten. Sie können es bei Bedarf erweitern. 

Das Schemamodell besitzt folgende Werte:

*   **NAME**. Die Person, die der Aufgabe zugewiesen ist. Dies ist ein **string**-Wert.
*   **TASK**. Der Name des Tasks. Dies ist ein **string**-Wert.
*   **DATE** Das Fälligkeitsdatum der Aufgabe. Dies ist ein **DateTime**-Wert.
*   **COMPLETED**. Gibt an, ob der Vorgang abgeschlossen ist. Dies ist ein **boolescher** Wert.

### <a name="create-the-schema-in-the-code"></a>Erstellen des Schemas im Code
1.  Wechseln Sie bei einer Eingabeaufforderung in das Verzeichnis **azuread**:

    `cd azuread`

2.  Öffnen Sie „Server.js“ in Ihrem Editor. Fügen Sie unter dem Konfigurationseintrag die folgenden Informationen hinzu:

    ```Javascript
    // MongoDB setup.
    // Set up some configuration.
    var serverPort = process.env.PORT || 8080;
    var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;
    // Connect to MongoDB.
    global.db = mongoose.connect(serverURI);
    var Schema = mongoose.Schema;
    log.info('MongoDB Schema loaded');
    ```

Dieser Code stellt eine Verbindung mit dem MongoDB-Server her. Er gibt auch ein Schemaobjekt zurück.

#### <a name="using-the-schema-create-your-model-in-the-code"></a>Erstellen Sie mithilfe des Schemas Ihr Modell im Code
Fügen Sie unter dem vorherigen Code den folgenden Code hinzu:

```Javascript
// Create a basic schema to store your tasks and users.
var TaskSchema = new Schema({
owner: String,
task: String,
completed: Boolean,
date: Date
});
// Use the schema to register a model.
mongoose.model('Task', TaskSchema);
var Task = mongoose.model('Task');
```

Wie im Code zu sehen ist, erstellen Sie zuerst Ihr Schema. Als Nächstes erstellen Sie ein Modellobjekt. Sie verwenden das Modellobjekt, um Ihre Daten im gesamten Code zu speichern, wenn Sie Ihre **Routen** definieren.

## <a name="13-add-your-routes-for-your-task-rest-api-server"></a>13: Hinzufügen Ihrer Routen für den Task-REST-API-Server
Nachdem das Datenbankmodell erstellt ist, fügen Sie nun die Routen hinzu, die Sie für den REST-API-Server benötigen.

### <a name="about-routes-in-restify"></a>Routen in Restify
Routen in Restify funktionieren genauso wie bei der Verwendung des Express-Stapels. Sie definieren die Routen mit den URIs, die die Client-Anwendungen Ihrer Erwartung nach aufrufen werden. In der Regel werden Routen in einer separaten Datei definiert. In diesem Tutorial platzieren Sie unsere Routen in „Server.js“. Für Produktionszwecke empfiehlt es sich, Routen in einer eigenen Datei zu definieren.

Ein typisches Muster für eine Restify-Route:

```Javascript
function createObject(req, res, next) {
// Do work on object.
_object.name = req.params.object; // Passed value is in req.params under object.
///...
return next(); // Keep the server going.
}
....
server.post('/service/:add/:object', createObject); // calls createObject on routes that match this.
```


Dies ist das einfachste Muster. Restify (und Express) bieten weitaus tiefere Funktionalität, z.B. die Möglichkeit, Anwendungstypen zu definieren sowie die komplexe Weiterleitung über mehrere Endpunkte.

#### <a name="add-default-routes-to-your-server"></a>Hinzufügen von Standardrouten zum Server
Fügen Sie die grundlegenden CRUD-Routen hinzu: **create** (Erstellen), **retrieve** (Abrufen), **update** (Aktualisieren) und **delete** (Löschen).

1.  Wechseln Sie bei einer Eingabeaufforderung in das Verzeichnis **azuread**:

    `cd azuread`

2.  Öffnen Sie „Server.js“ in einem Editor. Fügen Sie unterhalb der vorherigen Datenbankeinträge folgende Informationen hinzu:

    ```Javascript
    /**
    *
    * APIs for your REST task server
    */
    // Create a task.
    function createTask(req, res, next) {
    // Resitify currently has a bug that doesn't allow you to set default headers.
    // These headers comply with CORS, and allow you to use MongoDB Server as your response to any origin.
    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");
    // Create a new task model, fill it, and save it to MongoDB.
    var _task = new Task();
    if (!req.params.task) {
    req.log.warn({
    params: p
    }, 'createTodo: missing task');
    next(new MissingTaskError());
    return;
    }
    _task.owner = owner;
    _task.task = req.params.task;
    _task.date = new Date();
    _task.save(function(err) {
    if (err) {
    req.log.warn(err, 'createTask: unable to save');
    next(err);
    } else {
    res.send(201, _task);
    }
    });
    return next();
    }
    // Delete a task by name.
    function removeTask(req, res, next) {
    Task.remove({
    task: req.params.task,
    owner: owner
    }, function(err) {
    if (err) {
    req.log.warn(err,
    'removeTask: unable to delete %s',
    req.params.task);
    next(err);
    } else {
    log.info('Deleted task:', req.params.task);
    res.send(204);
    next();
    }
    });
    }
    // Delete all tasks.
    function removeAll(req, res, next) {
    Task.remove();
    res.send(204);
    return next();
    }
    // Get a specific task based on name.
    function getTask(req, res, next) {
    log.info('getTask was called for: ', owner);
    Task.find({
    owner: owner
    }, function(err, data) {
    if (err) {
    req.log.warn(err, 'get: unable to read %s', owner);
    next(err);
    return;
    }
    res.json(data);
    });
    return next();
    }
    /// Returns the list of TODOs that were loaded.
    function listTasks(req, res, next) {
    // Resitify currently has a bug that doesn't allow you to set default headers.
    // These headers comply with CORS, and allow us to use MongoDB Server as our response to any origin.
    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");
    log.info("listTasks was called for: ", owner);
    Task.find({
    owner: owner
    }).limit(20).sort('date').exec(function(err, data) {
    if (err)
    return next(err);
    if (data.length > 0) {
    log.info(data);
    }
    if (!data.length) {
    log.warn(err, "There are no tasks in the database. Add one!");
    }
    if (!owner) {
    log.warn(err, "You did not pass an owner when listing tasks.");
    } else {
    res.json(data);
    }
    });
    return next();
    }
    ```

### <a name="add-error-handling-for-the-routes"></a>Hinzufügen einer Fehlerbehandlungsroutine für Routen
Fügen Sie einige Fehlerbehandlungsroutinen hinzu, damit Sie mit dem Client über das von Ihnen gefundene Problem kommunizieren können.

Fügen Sie den folgenden Code unter dem Code hinzu, den Sie bereits geschrieben haben:

```Javascript
///--- Errors for communicating something more information back to the client.
function MissingTaskError() {
restify.RestError.call(this, {
statusCode: 409,
restCode: 'MissingTask',
message: '"task" is a required parameter',
constructorOpt: MissingTaskError
});
this.name = 'MissingTaskError';
}
util.inherits(MissingTaskError, restify.RestError);
function TaskExistsError(owner) {
assert.string(owner, 'owner');
restify.RestError.call(this, {
statusCode: 409,
restCode: 'TaskExists',
message: owner + ' already exists',
constructorOpt: TaskExistsError
});
this.name = 'TaskExistsError';
}
util.inherits(TaskExistsError, restify.RestError);
function TaskNotFoundError(owner) {
assert.string(owner, 'owner');
restify.RestError.call(this, {
statusCode: 404,
restCode: 'TaskNotFound',
message: owner + ' was not found',
constructorOpt: TaskNotFoundError
});
this.name = 'TaskNotFoundError';
}
util.inherits(TaskNotFoundError, restify.RestError);
```


## <a name="14-create-your-server"></a>14: Erstellen des Servers
Als Letztes fügen Sie Ihre Serverinstanz hinzu. Die Serverinstanz verwaltet Ihre Aufrufe.

Restify (und Express) verfügen über Möglichkeiten zur umfassenden Anpassung, die Sie mit einem REST-API-Server nutzen können. In diesem Tutorial wird das einfachste Setup verwendet.

```Javascript
/**
* Your server
*/
var server = restify.createServer({
name: "Microsoft Azure Active Directory TODO Server",
version: "2.0.1"
});
// Ensure that you don't drop data on uploads.
server.pre(restify.pre.pause());
// Clean up imprecise paths like //todo//////1//.
server.pre(restify.pre.sanitizePath());
// Handle annoying user agents (curl).
server.pre(restify.pre.userAgentConnection());
// Set a per-request Bunyan logger (with requestid filled in).
server.use(restify.requestLogger());
// Allow 5 requests/second by IP address, and burst to 10.
server.use(restify.throttle({
burst: 10,
rate: 5,
ip: true,
}));
// Use common commands, such as:
server.use(restify.acceptParser(server.acceptable));
server.use(restify.dateParser());
server.use(restify.queryParser());
server.use(restify.gzipResponse());
server.use(restify.bodyParser({
mapParams: true
}));
```
## <a name="15-add-the-routes-without-authentication-for-now"></a>15: Hinzufügen der Routen (vorläufig ohne Authentifizierung)
```Javascript
/// Use CRUD to add the real handlers.
/**
/*
/* Each of these handlers is protected by your Open ID Connect Bearer strategy. Invoke 'oidc-bearer'
/* in the pasport.authenticate() method. Because REST is stateless, set 'session: false'. You 
/* don't need to maintain session state. You can experiment with removing API protection.
/* To do this, remove the passport.authenticate() method:
/*
/* server.get('/tasks', listTasks);
/*
**/
server.get('/tasks', listTasks);
server.get('/tasks', listTasks);
server.get('/tasks/:owner', getTask);
server.head('/tasks/:owner', getTask);
server.post('/tasks/:owner/:task', createTask);
server.post('/tasks', createTask);
server.del('/tasks/:owner/:task', removeTask);
server.del('/tasks/:owner', removeTask);
server.del('/tasks', removeTask);
server.del('/tasks', removeAll, function respond(req, res, next) {
res.send(204);
next();
});
// Register a default '/' handler
server.get('/', function root(req, res, next) {
var routes = [
'GET /',
'POST /tasks/:owner/:task',
'POST /tasks (for JSON body)',
'GET /tasks',
'PUT /tasks/:owner',
'GET /tasks/:owner',
'DELETE /tasks/:owner/:task'
];
res.send(200, routes);
next();
});
server.listen(serverPort, function() {
var consoleMessage = '\n Microsoft Azure Active Directory Tutorial';
consoleMessage += '\n +++++++++++++++++++++++++++++++++++++++++++++++++++++';
consoleMessage += '\n %s server is listening at %s';
consoleMessage += '\n Open your browser to %s/tasks\n';
consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n';
consoleMessage += '\n !!! why not try a $curl -isS %s | json to get some ideas? \n';
consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n\n';
});
```
## <a name="16-run-the-server"></a>16: Ausführen des Servers
Es ist eine gute Idee, Ihren Server vor dem Hinzufügen von Authentifizierungen zu testen.

Am einfachsten können Sie Ihren Server testen, indem Sie „curl“ bei einer Eingabeaufforderung verwenden. Hierzu verwenden Sie ein einfaches Hilfsprogramm, mit dem die Ausgabe als JSON-Code analysiert werden kann. 

1.  Installieren Sie das JSON-Tool, das wir in folgenden Beispielen verwenden:

    `$npm install -g jsontool`

    Dadurch wird das Tool JSON global installiert.

2.  Vergewissern Sie sich, dass die MongoDB-Instanz ausgeführt wird:

    `$sudo mongod`

3.  Ändern Sie das Verzeichnis in **azuread**, und führen Sie dann „curl“ aus:

    `$ cd azuread`
    `$ node server.js`

    `$ curl -isS http://127.0.0.1:8080 | json`

    ```Shell
    HTTP/1.1 2.0OK
    Connection: close
    Content-Type: application/json
    Content-Length: 171
    Date: Tue, 14 Jul 2015 05:43:38 GMT
    [
    "GET /",
    "POST /tasks/:owner/:task",
    "POST /tasks (for JSON body)",
    "GET /tasks",
    "PUT /tasks/:owner",
    "GET /tasks/:owner",
    "DELETE /tasks/:owner/:task"
    ]
    ```

4.  So fügen Sie eine Aufgabe hinzu:

    `$ curl -isS -X POST http://127.0.0.1:8888/tasks/brandon/Hello`

    Die Antwort sollte wie folgt lauten:

    ```Shell
    HTTP/1.1 201 Created
    Connection: close
    Access-Control-Allow-Origin: *
    Access-Control-Allow-Headers: X-Requested-With
    Content-Type: application/x-www-form-urlencoded
    Content-Length: 5
    Date: Tue, 04 Feb 2014 01:02:26 GMT
    Hello
    ```

5.  Listet Aufgaben für Brandon auf:

    `$ curl -isS http://127.0.0.1:8080/tasks/brandon/`

Wenn all diese Befehle ohne Fehler ausgeführt werden, sind Sie bereit, OAuth zum REST-API-Server hinzuzufügen.

*Sie verfügen nun über einen REST-API-Server mit MongoDB.*

## <a name="17-add-authentication-to-your-rest-api-server"></a>17: Hinzufügen der Authentifizierung zum REST-API-Server
Da Sie jetzt über eine ausgeführte REST-API verfügen, richten Sie diese für die Nutzung mit Azure AD ein.

Wechseln Sie bei einer Eingabeaufforderung in das Verzeichnis **azuread**:

`cd azuread`

### <a name="use-the-oidcbearerstrategy-thats-included-with-passport-azure-ad"></a>Verwenden von „oidcbearerstrategy“ (in „passport-azure-ad“ enthalten)
Bisher haben Sie einen normalen REST-TODO-Server ohne jegliche Autorisierung erstellt. Fügen Sie nun die Authentifizierung hinzu.

Geben Sie zunächst an, dass Sie Passport verwenden möchten. Fügen Sie diesen Hinweis direkt nach Ihrer früheren Serverkonfiguration ein:

```Javascript
// Start using Passport.js.

server.use(passport.initialize()); // Starts passport
server.use(passport.session()); // Provides session support
```

> [!TIP]
> Beim Schreiben von APIs sollten Sie die Daten immer mit einem eindeutigen Element aus dem Token verknüpfen, das der Benutzer nicht spoofen kann. Wenn dieser Server TODO-Elemente speichert, speichert er sie entsprechend der Benutzerabonnement-ID im Token (wird über „token.sub“ aufgerufen). Fügen Sie „token.sub“ in das Feld „owner“ (Besitzer) ein. So wird sichergestellt, dass nur dieser Benutzer auf die TODO-Elemente des Benutzers zugreifen kann. Es kann niemand anderes auf TODO-Elemente zugreifen, die wir eingegeben haben. In der API existiert keine Offenlegung für „owner“. Ein externer Benutzer kann TODO-Elemente anderer Benutzer anfragen, auch wenn diese authentifiziert sind.
> 
> 

Als Nächstes verwenden Sie die Strategie „Open ID Connect Bearer“, die zu `passport-azure-ad` gehört. Fügen Sie diesen Code nach dem Code ein, den Sie zuvor hinzugefügt haben:

```Javascript
/**
/*
/* Calling the OIDCBearerStrategy and managing users.
/*
/* Because of the Passport pattern, you need to manage users and info tokens
/* with a FindorCreate() method. The method must be provided by the implementor.
/* In the following code, you autoregister any user and implement a FindById().
/* It's a good idea to do something more advanced.
**/
var findById = function(id, fn) {
for (var i = 0, len = users.length; i < len; i++) {
var user = users[i];
if (user.sub === id) {
log.info('Found user: ', user);
return fn(null, user);
}
}
return fn(null, null);
};
var oidcStrategy = new OIDCBearerStrategy(options,
function(token, done) {
log.info('verifying the user');
log.info(token, 'was the token retrieved');
findById(token.sub, function(err, user) {
if (err) {
return done(err);
}
if (!user) {
// "Auto-registration"
log.info('User was added automatically, because they were new. Their sub is: ', token.sub);
users.push(token);
owner = token.sub;
return done(null, token);
}
owner = token.sub;
return done(null, user, token);
});
}
);
passport.use(oidcStrategy);
```

Passport verwendet ein ähnliches Muster für alle Strategien (Twitter, Facebook usw.). Beim Schreiben von Strategien wird dieses Muster befolgt. Übergeben Sie der Strategie ein `function()`-Objekt, das ein Token und `done` als Parameter verwendet. Die Strategie wird zurückgegeben, nachdem sie alle ihre Aufgaben erledigt hat. Speichern Sie den Benutzer und das Token, damit Sie beides nicht mehr erfragen müssen.

> [!IMPORTANT]
> Der vorherige Code verwendet Benutzer, die sich bei Ihrem Server authentifizieren können. Dies wird als automatische Registrierung bezeichnet. Auf Produktionsservern erlauben Sie den Zugriff erst, nachdem ein von Ihnen gewählter Registrierungsvorgang durchlaufen wurde. Dies ist normalerweise das Muster bei Apps im Privatkundenbereich. Die App ermöglicht möglicherweise die Registrierung bei Facebook, fordert Sie aber anschließend zur Eingabe weiterer Informationen auf. Wenn Sie kein Befehlszeilenprogramm für dieses Tutorial verwenden würden, könnten Sie die E-Mail-Adresse aus dem Tokenobjekt extrahieren, das zurückgegeben wird. Dann könnten Sie den Benutzer um die Eingabe zusätzlicher Informationen bitten. Da es sich um einen Testserver handelt, fügen Sie den Benutzer direkt der Datenbank im Arbeitsspeicher hinzu.
> 
> 

### <a name="protect-endpoints"></a>Schützen von Endpunkten
Schützen Sie Endpunkte mit dem Aufruf **passport.authenticate()**, indem Sie darin das Protokoll angeben, das Sie verwenden möchten.

Sie können Ihre Route in Ihrem Servercode für erweiterte Optionen bearbeiten:

```Javascript
server.get('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), listTasks);
server.get('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), listTasks);
server.get('/tasks/:owner', passport.authenticate('oidc-bearer', {
session: false
}), getTask);
server.head('/tasks/:owner', passport.authenticate('oidc-bearer', {
session: false
}), getTask);
server.post('/tasks/:owner/:task', passport.authenticate('oidc-bearer', {
session: false
}), createTask);
server.post('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), createTask);
server.del('/tasks/:owner/:task', passport.authenticate('oidc-bearer', {
session: false
}), removeTask);
server.del('/tasks/:owner', passport.authenticate('oidc-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), removeAll, function respond(req, res, next) {
res.send(204);
next();
});
```

## <a name="18-run-your-server-application-again"></a>18: Erneutes Ausführen Ihrer Serveranwendung
Mithilfe von „curl“ können Sie ermitteln, ob Ihre Endpunkte nun durch OAuth 2.0 geschützt sind. Tun Sie dies, bevor Sie eines Ihrer Client SDKs an diesem Endpunkten ausführen. Die zurückgegebenen Header sollten Ihnen mitteilen, ob Ihre Authentifizierung ordnungsgemäß ausgeführt wird.

1.  Vergewissern Sie sich, dass die MongoDB-Instanz ausgeführt wird:

    `$sudo mongod`

2.  Wechseln Sie in das Verzeichnis **azuread**, und verwenden Sie anschließend „curl“:

    `$ cd azuread`

    `$ node server.js`

3.  Probieren Sie es mit einem einfachen POST:

    `$ curl -isS -X POST http://127.0.0.1:8080/tasks/brandon/Hello`

    ```Shell
    HTTP/1.1 401 Unauthorized
    Connection: close
    WWW-Authenticate: Bearer realm="Users"
    Date: Tue, 14 Jul 2015 05:45:03 GMT
    Transfer-Encoding: chunked
    ```

Eine 401-Antwort gibt an, dass die Passportebene versucht, eine Umleitung an den autorisierten Endpunkt durchzuführen. Dies ist genau das, was wir erreichen möchten.

*Sie verfügen jetzt über einen REST-API-Dienst, der OAuth 2.0 verwendet.*

Sie haben für diesen Server nun alle Schritte ausgeführt, die ohne einen OAuth 2.0-kompatiblen Client möglich sind. Dafür müssen Sie ein zusätzliches Tutorial durcharbeiten.

## <a name="next-steps"></a>Nächste Schritte
Als Referenz stellen wir das vollständige Beispiel (ohne Ihre Konfigurationswerte) [als ZIP-Datei](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs/archive/complete.zip) bereit. Sie können es auch von GitHub klonen:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs.git```

Sie können nun mit den Themen für fortgeschrittenere Benutzer fortfahren. Vielleicht möchten Sie das Thema [Schützen einer Node.js-Web-App mit dem v2.0-Endpunkt](active-directory-v2-devquickstarts-node-web.md) ausprobieren.

Hier sind einige zusätzliche Ressourcen:

* [Azure AD v2.0-Entwicklerhandbuch](active-directory-appmodel-v2-overview.md)
* [StackOverflow-Tag „azure-active-directory“](http://stackoverflow.com/questions/tagged/azure-active-directory)

### <a name="get-security-updates-for-our-products"></a>Abrufen von Sicherheitsupdates für unsere Produkte
Wir empfehlen Ihnen, sich zu registrieren, um bei Auftreten von Sicherheitsvorfällen benachrichtigt zu werden. Abonnieren Sie auf der Seite [Technische Sicherheitsbenachrichtigungen von Microsoft](https://technet.microsoft.com/security/dd252948) den Microsoft-Sicherheitsnewsletter.


