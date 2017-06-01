---
title: "Azure AD B2C: Schützen einer Web-API mit Node.js | Microsoft Docs"
description: Erstellen einer Node.js-Web-API, die Token von einem B2C-Mandanten akzeptiert
services: active-directory-b2c
documentationcenter: 
author: xerners
manager: mbaldwin
editor: 
ms.assetid: fc2b9af8-fbda-44e0-962a-8b963449106a
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: hero-article
ms.date: 01/07/2017
ms.author: xerners
ms.translationtype: Human Translation
ms.sourcegitcommit: 3b5d9162e2d39e2b0f011383a478545644c57861
ms.openlocfilehash: c9742c7f505417577857889ef307083afb9bab18
ms.contentlocale: de-de
ms.lasthandoff: 02/14/2017


---
# <a name="azure-ad-b2c-secure-a-web-api-by-using-nodejs"></a>Azure AD B2C: Schützen einer Web-API mit „Node.js“
<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Mit Azure Active Directory (Azure AD) B2C können Sie eine Web-API mithilfe von OAuth 2.0-Zugriffstoken sichern. Mit diesen Token können Ihre Client-Apps, die Azure AD B2C verwenden, sich bei der API authentifizieren. In diesem Artikel erfahren Sie, wie Sie eine Art Aufgabenlisten-API erstellen, mit der Benutzer Aufgaben hinzufügen und auflisten können. Die Web-API wird mit Azure AD B2C geschützt, und nur authentifizierte Benutzer können ihre Aufgabenliste verwalten.

> [!NOTE]
> Dieses Beispiel wurde für unsere [iOS B2C-Beispielanwendung](active-directory-b2c-devquickstarts-ios.md)geschrieben. Führen Sie zunächst die aktuelle exemplarische Vorgehensweise durch, und fahren Sie dann mit diesem Beispiel fort.
>
>

**Passport** ist eine Authentifizierungs-Middleware für Node.js. Das flexible und modular aufgebaute Passport kann unauffällig in jeder Express- oder Restify-basierten Webanwendung installiert werden. Ein umfassender Satz an Strategien unterstützt die Authentifizierung mittels eines Benutzernamens und Kennworts in Facebook, Twitter und anderen Anwendungen. Wir haben eine Strategie für Azure Active Directory (Azure AD) entwickelt. Installieren Sie dieses Modul, und fügen Sie dann das Azure AD-Plug-In `passport-azure-ad` hinzu.

Gehen Sie für dieses Beispiel wie folgt vor:

1. Registrieren Sie eine Anwendung in Azure AD.
2. Richten Sie Ihre Anwendung für die Verwendung des Passport-Plug-Ins `azure-ad-passport` ein.
3. Konfigurieren Sie eine Clientanwendung für den Aufruf der Web-API „To Do List“.

## <a name="get-an-azure-ad-b2c-directory"></a>Erstellen eines Azure AD B2C-Verzeichnisses
Bevor Sie Azure AD B2C verwenden können, müssen Sie ein Verzeichnis oder einen Mandanten erstellen.  Ein Verzeichnis ist ein Container für alle Benutzer, Apps, Gruppen und Ähnliches.  [Erstellen Sie zunächst ein B2C-Verzeichnis](active-directory-b2c-get-started.md) , sofern noch keines vorhanden ist.

## <a name="create-an-application"></a>Erstellen einer Anwendung
Als nächstes müssen Sie eine App in Ihrem B2C-Verzeichnis erstellen, sodass Azure AD die Informationen erhält, die für die sichere Kommunikation mit Ihrer App erforderlich sind. In diesem Fall werden die Client-App und die Web-API durch eine einzelne **Anwendungs-ID**dargestellt, da sie zusammen eine logische App bilden. Befolgen Sie zum Erstellen einer App [diese Anweisungen](active-directory-b2c-app-registration.md). Führen Sie folgende Schritte aus:

* Fügen Sie der Anwendung eine **Web-App/Web-API** hinzu.
* Geben Sie `http://localhost/TodoListService` als **Antwort-URL**ein. Dies ist die Standard-URL für dieses Codebeispiel.
* Erstellen Sie einen **geheimen Schlüssel** für Ihre Anwendung, und kopieren Sie ihn. Diese Daten benötigen Sie später noch. Beachten Sie, dass dieser Wert vor der Verwendung in [XML-Escape-Zeichen](https://www.w3.org/TR/2006/REC-xml11-20060816/#dt-escape) gesetzt werden muss.
* Kopieren Sie die **Anwendungs-ID** , die Ihrer App zugewiesen ist. Diese Daten benötigen Sie später noch.

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Erstellen der Richtlinien
In Azure AD B2C wird jede Benutzererfahrung durch eine [Richtlinie](active-directory-b2c-reference-policies.md)definiert. Diese App enthält zwei identitätsbezogene Erfahrungen: Registrieren und Anmeldung. Sie müssen eine Richtlinie jedes Typs erstellen, wie im [Richtlinienreferenzartikel](active-directory-b2c-reference-policies.md#create-a-sign-up-policy)beschrieben.  Beachten Sie beim Erstellen der drei Richtlinien Folgendes:

* Wählen Sie den **Anzeigenamen** und andere Registrierungsattribute in der Registrierungsrichtlinie aus.
* Wählen Sie den **Anzeigenamen** und die **Objekt-ID** als Anwendungsansprüche in jeder Richtlinie aus.  Sie können auch andere Ansprüche auswählen.
* Notieren Sie sich die **Namen** der einzelnen Richtlinien nach ihrer Erstellung. Sie müssen das Präfix `b2c_1_`aufweisen.  Diese Richtliniennamen werden später benötigt.

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Nachdem Sie die drei Richtlinien erstellt haben, können Sie Ihre App erstellen.

Grundlegende Informationen zur Funktionsweise von Richtlinien in Azure AD B2C finden Sie im [Tutorial zu den ersten Schritten mit .NET-Web-Apps](active-directory-b2c-devquickstarts-web-dotnet.md).

## <a name="download-the-code"></a>Herunterladen des Codes
Der Code für dieses Tutorial wird [auf GitHub](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS)verwaltet. Zum Erstellen des Beispiels können Sie [ein Projektgerüst als ZIP-Datei herunterladen](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS/archive/skeleton.zip). Sie können das Gerüst auch klonen:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS.git
```

Die fertige App ist ebenfalls als [ZIP-Datei](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS/archive/complete.zip) oder unter der Verzweigung `complete` im gleichen Repository verfügbar.

## <a name="download-nodejs-for-your-platform"></a>Herunterladen der Datei „node.js“ für Ihre Plattform
Für die Verwendung dieses Beispiels benötigen Sie eine funktionierende Installation von Node.js.

Installieren Sie Node.js über [nodejs.org](http://nodejs.org).

## <a name="install-mongodb-for-your-platform"></a>Installieren von MongoDB für Ihre Plattform
Für die Verwendung dieses Beispiels benötigen Sie eine funktionierende Installation von MongoDB. Durch die Verwendung von MongoDB bleibt die REST-API über Serverinstanzen hinweg persistent.

Installieren Sie MongoDB über [mongodb.org](http://www.mongodb.org).

> [!NOTE]
> In dieser exemplarischen Vorgehensweise wird davon ausgegangen, dass Sie die Standardinstallation und die standardmäßigen Serverendpunkte für MongoDB verwenden: `mongodb://localhost` (korrekte Angabe zum Zeitpunkt der Artikelerstellung)
>
>

## <a name="install-the-restify-modules-in-your-web-api"></a>Installieren der Restify-Module in Ihrer Web-API
Zum Erstellen der REST-API verwenden wir Restify. Restify ist ein einfaches und flexibles, von Express abgeleitetes Node.js-Anwendungsframework. Er verfügt über eine Reihe robuster Funktionen für die Erstellung von REST-APIs für Connect.

### <a name="install-restify"></a>Installieren von Restify
Wechseln Sie über die Befehlszeile zum Verzeichnis `azuread`. Falls das Verzeichnis `azuread` nicht vorhanden ist, erstellen Sie es.

`cd azuread` oder `mkdir azuread;`

Geben Sie den folgenden Befehl ein:

`npm install restify`

Mit diesem Befehl wird Restify installiert.

#### <a name="did-you-get-an-error"></a>Bei einem Fehler
In einigen Betriebssystemen tritt bei Verwendung von `npm` möglicherweise der Fehler `Error: EPERM, chmod '/usr/local/bin/..'` auf, und Sie werden aufgefordert, das Konto als Administrator auszuführen. Verwenden Sie in diesem Fall den Befehl `sudo`, um `npm` mit einer höheren Berechtigungsstufe auszuführen.

#### <a name="did-you-get-a-dtrace-error"></a>Ist ein DTrace-Fehler aufgetreten?
Bei der Installation von Restify erhalten Sie eventuell eine Meldung wie die Folgende:

```Shell
clang: error: no such file or directory: 'HD/azuread/node_modules/restify/node_modules/dtrace-provider/libusdt'
make: *** [Release/DTraceProviderBindings.node] Error 1
gyp ERR! build error
gyp ERR! stack Error: `make` failed with exit code: 2
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

Restify stellt einen leistungsstarken Mechanismus zum Verfolgen von REST-Aufrufen mit DTrace bereit. Unter vielen Betriebssystemen steht DTrace jedoch nicht zur Verfügung. Sie können diese Fehler ignorieren.

Die Ausgabe dieses Befehls sollte in etwa wie folgt aussehen:

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
    └── bunyan@0.22.0 (mv@0.0.5)

## <a name="install-passport-in-your-web-api"></a>Installieren von Passport in Ihrer Web-API
Wechseln Sie über die Befehlszeile zum Verzeichnis `azuread`, sofern Sie sich nicht bereits dort befinden.

Installieren Sie Passport mithilfe des folgenden Befehls:

`npm install passport`

Die Ausgabe dieses Befehls sollte in etwa wie folgt aussehen:

    passport@0.1.17 node_modules\passport
    ├── pause@0.0.1
    └── pkginfo@0.2.3

## <a name="add-passport-azuread-to-your-web-api"></a>Hinzufügen von „passport-azuread“ zu Ihrer Web-API
Als Nächstes fügen Sie die OAuth-Strategie hinzu. Dazu verwenden Sie `passport-azuread` (eine Reihe von Strategien, die Azure AD mit Passport verbinden). Verwenden Sie diese Strategie für Bearertoken im Rest-API-Beispiel.

> [!NOTE]
> OAuth2 bietet zwar ein Framework, in dem alle bekannten Tokentypen ausgegeben werden können, allerdings findet hiervon meist nur ein eingeschränkter Tokensatz Anwendung. Die Token für den Schutz von Endpunkten sind Bearertoken. Diese Tokentypen werden in OAuth2 am häufigsten ausgestellt. Bei vielen Implementierungen wird davon ausgegangen, dass Bearertoken der einzige ausgestellte Tokentyp sind.
>
>

Wechseln Sie über die Befehlszeile zum Verzeichnis `azuread`, sofern Sie sich nicht bereits dort befinden.

Verwenden Sie den folgenden Befehl, um das Passport-Modul `passport-azure-ad` zu installieren:

`npm install passport-azure-ad`

Die Ausgabe dieses Befehls sollte in etwa wie folgt aussehen:

``
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
``

## <a name="add-mongodb-modules-to-your-web-api"></a>Hinzufügen der MongoDB-Module zu Ihrer Web-API
In diesem Beispiel wird MongoDB als Datenspeicher verwendet. Installieren Sie hierzu Mongoose – ein weit verbreitetes Plug-In zum Verwalten von Modellen und Schemas.

* `npm install mongoose`

## <a name="install-additional-modules"></a>Installieren zusätzlicher Module
Installieren Sie als nächstes weiteren erforderlichen Module.

Wechseln Sie über die Befehlszeile zum Verzeichnis `azuread`, sofern Sie sich nicht bereits dort befinden:

`cd azuread`

Installieren Sie die Module im Verzeichnis `node_modules` :

* `npm install assert-plus`
* `npm install ejs`
* `npm install ejs-locals`
* `npm install express`
* `npm install bunyan`

## <a name="create-a-serverjs-file-with-your-dependencies"></a>Erstellen der Datei „server.js“ mit Ihren Abhängigkeiten
Die Datei `server.js` stellt den Großteil der Funktionen für Ihren Web-API-Server bereit.

Wechseln Sie über die Befehlszeile zum Verzeichnis `azuread`, sofern Sie sich nicht bereits dort befinden:

`cd azuread`

Erstellen Sie in einem Editor eine Datei vom Typ `server.js` . Fügen Sie die folgenden Informationen hinzu:

```Javascript
'use strict';
/**
* Module dependencies.
*/
var fs = require('fs');
var path = require('path');
var util = require('util');
var assert = require('assert-plus');
var mongoose = require('mongoose/');
var bunyan = require('bunyan');
var restify = require('restify');
var config = require('./config');
var passport = require('passport');
var OIDCBearerStrategy = require('passport-azure-ad').BearerStrategy;
```

Speichern Sie die Datei. Sie wird später noch benötigt.

## <a name="create-a-configjs-file-to-store-your-azure-ad-settings"></a>Erstellen der Datei „config.js“ für die Azure AD-Einstellungen
Diese Codedatei übergibt Ihre Konfigurationsparameter aus dem Azure AD-Portal an die Datei `Passport.js` . Sie haben diese Konfigurationswerte erstellt, als Sie die Web-API im ersten Teil dieser exemplarischen Vorgehensweise zum Portal hinzufügten. Welche Werte Sie für diese Parameter eingeben sollen, erfahren Sie, nachdem Sie den Code kopiert haben.

Wechseln Sie über die Befehlszeile zum Verzeichnis `azuread`, sofern Sie sich nicht bereits dort befinden:

`cd azuread`

Erstellen Sie in einem Editor eine Datei vom Typ `config.js` . Fügen Sie die folgenden Informationen hinzu:

```Javascript
// Don't commit this file to your public repos. This config is for first-run
exports.creds = {
clientID: <your client ID for this Web API you created in the portal>
mongoose_auth_local: 'mongodb://localhost/tasklist', // Your mongo auth uri goes here
audience: '<your audience URI>', // the Client ID of the application that is calling your API, usually a web API or native client
identityMetadata: 'https://login.microsoftonline.com/<tenant name>/.well-known/openid-configuration', // Make sure you add the B2C tenant name in the <tenant name> area
tenantName:'<tenant name>',
policyName:'b2c_1_<sign in policy name>' // This is the policy you'll want to validate against in B2C. Usually this is your Sign-in policy (as users sign in to this API)
passReqToCallback: false // This is a node.js construct that lets you pass the req all the way back to any upstream caller. We turn this off as there is no upstream caller.
};

```

[!INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

### <a name="required-values"></a>Erforderliche Werte
`clientID`: Die Client-ID Ihrer Web-API-Anwendung.

`IdentityMetadata`: Hier sucht `passport-azure-ad` nach Ihren Konfigurationsdaten für den Identitätsanbieter. Außerdem wird hier nach den Schlüsseln zum Überprüfen der JSON-Webtoken gesucht.

`audience`: Der URI (Uniform Resource Identifier) aus dem Portal, der Ihre aufrufende Anwendung identifiziert.

`tenantName`: Der Name Ihres Mandanten (etwa **contoso.onmicrosoft.com**).

`policyName`: Die Richtlinie zur Überprüfung der auf Ihrem Server eingehenden Token. Hierbei muss es sich um die gleiche Richtlinie handeln, die Sie auch in der Clientanwendung für die Anmeldung verwenden.

> [!NOTE]
> Verwenden Sie einstweilen bei der Client- und Servereinrichtung dieselben Richtlinien. Wenn Sie bereits eine exemplarische Vorgehensweise abgeschlossen und diese Richtlinien erstellt haben, müssen Sie diese nicht erneut erstellen. Da Sie die exemplarische Vorgehensweise abgeschlossen haben, müssen Sie keine neuen Richtlinien einrichten, wenn Sie auf dieser Website exemplarische Vorgehensweisen für Clients befolgen.
>
>

## <a name="add-configuration-to-your-serverjs-file"></a>Hinzufügen der Konfiguration zur Datei „server.js“
Um die Werte aus der von Ihnen erstellten Datei `config.js` zu lesen, fügen Sie Ihrer Anwendung die Datei `.config` als erforderliche Ressource hinzu. Legen Sie anschließend für die globalen Variablen die im Dokument `config.js` angegebenen Werte fest.

Wechseln Sie über die Befehlszeile zum Verzeichnis `azuread`, sofern Sie sich nicht bereits dort befinden:

`cd azuread`

Öffnen Sie die Datei `server.js` in einem Editor. Fügen Sie die folgenden Informationen hinzu:

```Javascript
var config = require('./config');
```
Fügen Sie der Datei `server.js` einen neuen Abschnitt mit dem folgenden Code hinzu:

```Javascript
// We pass these options in to the ODICBearerStrategy.

var options = {
    // The URL of the metadata document for your app. We put the keys for token validation from the URL found in the jwks_uri tag of the in the metadata.
    identityMetadata: config.creds.identityMetadata,
    clientID: config.creds.clientID,
    tenantName: config.creds.tenantName,
    policyName: config.creds.policyName,
    validateIssuer: config.creds.validateIssuer,
    audience: config.creds.audience,
    passReqToCallback: config.creds.passReqToCallback

};
```

Als Nächstes fügen wir einige Platzhalter für die Benutzer hinzu, die wir von unseren aufrufenden Anwendungen erhalten.

```Javascript
// array to hold logged in users and the current logged in user (owner)
var users = [];
var owner = null;
```

Bei dieser Gelegenheit erstellen wir auch gleich unsere Protokollierung.

```Javascript
// Our logger
var log = bunyan.createLogger({
    name: 'Microsoft Azure Active Directory Sample'
});
```

## <a name="add-the-mongodb-model-and-schema-information-by-using-mongoose"></a>Hinzufügen der Modell- und Schemainformationen von MongoDB mithilfe von Moongoose
Die vorangegangenen Vorbereitungen machen sich nun bezahlt, wenn Sie die drei Dateien in einem REST-API-Dienst zusammenführen.

Wie zuvor beschrieben verwenden wir in dieser exemplarischen Vorgehensweise MongoDB zum Speichern der Aufgaben.

In der Datei `config.js` haben Sie die Datenbank **tasklist**genannt. Dieser Name wurde auch am Ende der `mongoose_auth_local` -Verbindungs-URL eingefügt. Sie müssen diese Datenbank nicht vorab in MongoDB erstellen. Die Datenbank wird bei der ersten Ausführung der Serveranwendung automatisch erstellt.

Nachdem Sie dem Server mitteilen, welche MongoDB-Datenbank er verwenden soll, müssen Sie weiteren Code schreiben, um das Modell und Schema für die Serveraufgaben zu erstellen.

### <a name="expand-the-model"></a>Erweiterung des Modells
Hierbei handelt es sich um ein einfaches Schemamodell. Sie können es nach Bedarf erweitern.

`owner`: Der Name des Benutzers, der der Aufgabe zugewiesen ist. Dieses Objekt ist ein **Zeichenfolge**.  

`Text`: Die eigentliche Aufgabe. Dieses Objekt ist ein **Zeichenfolge**.

`date`: Das Fälligkeitsdatum der Aufgabe. Dieses Objekt ist ein Datums-/Uhrzeitwert ( **datetime**).

`completed`: Gibt an, ob der Vorgang abgeschlossen ist. Dieses Objekt ist **boolescher Wert**.

### <a name="create-the-schema-in-the-code"></a>Erstellen des Schemas im Code
Wechseln Sie über die Befehlszeile zum Verzeichnis `azuread`, sofern Sie sich nicht bereits dort befinden:

`cd azuread`

Öffnen Sie die Datei `server.js` in einem Editor. Fügen Sie nach dem Konfigurationseintrag die folgenden Informationen hinzu:

```Javascript
// MongoDB setup
// Setup some configuration
var serverPort = process.env.PORT || 3000; // Note we are hosting our API on port 3000
var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;

// Connect to MongoDB
global.db = mongoose.connect(serverURI);
var Schema = mongoose.Schema;
log.info('MongoDB Schema loaded');

// Here we create a schema to store our tasks and users. Pretty simple schema for now.
var TaskSchema = new Schema({
    owner: String,
    Text: String,
    completed: Boolean,
    date: Date
});

// Use the schema to register a model
mongoose.model('Task', TaskSchema);
var Task = mongoose.model('Task');
```
Erstellen Sie zunächst das Schema und danach ein Modellobjekt, das Sie im gesamten Code zum Speichern der Daten verwenden, wenn Sie die **Routen**definieren.

## <a name="add-routes-for-your-rest-api-task-server"></a>Hinzufügen der Routen für den REST-API-Taskserver
Nachdem das Datenbankmodell erstellt ist, fügen Sie nun die Routen hinzu, die Sie für den REST-API-Server benötigen.

### <a name="about-routes-in-restify"></a>Routen in Restify
Routen funktionieren in Restify auf die gleiche Weise wie bei Verwendung des Express-Stapels. Sie definieren die Routen mit den URIs, die die Client-Anwendungen Ihrer Erwartung nach aufrufen werden.

Ein typisches Muster für eine Restify-Route ist:

```Javascript
function createObject(req, res, next) {
// do work on Object
_object.name = req.params.object; // passed value is in req.params under object
///...
return next(); // keep the server going
}
....
server.post('/service/:add/:object', createObject); // calls createObject on routes that match this.
```

Restify und Express bieten weitaus tiefere Funktionalität wie das Definieren von Anwendungstypen und Ausführen komplexer Weiterleitungen über mehrere Endpunkte. In diesem Tutorial halten wir die Routen eher einfach.

#### <a name="add-default-routes-to-your-server"></a>Hinzufügen von Standardrouten zum Server
In diesem Schritt fügen Sie die grundlegenden CRUD-Routen zum **Erstellen** und **Auflisten** für die REST-API hinzu. Weitere Routen finden Sie in der `complete` -Verzweigung des Beispiels.

Wechseln Sie über die Befehlszeile zum Verzeichnis `azuread`, sofern Sie sich nicht bereits dort befinden:

`cd azuread`

Öffnen Sie die Datei `server.js` in einem Editor. Fügen Sie unterhalb der vorherigen Datenbankeinträge folgende Informationen hinzu:

```Javascript
/**
 *
 * APIs for our REST Task server
 */

// Create a task

function createTask(req, res, next) {

    // Resitify currently has a bug which doesn't allow you to set default headers
    // This headers comply with CORS and allow us to mongodbServer our response to any origin

    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");

    // Create a new task model, fill it up and save it to Mongodb
    var _task = new Task();

    if (!req.params.Text) {
        req.log.warn({
            params: req.params
        }, 'createTodo: missing task');
        next(new MissingTaskError());
        return;
    }

    _task.owner = owner;
    _task.Text = req.params.Text;
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
```

```Javascript
/// Simple returns the list of TODOs that were loaded.

function listTasks(req, res, next) {
    // Resitify currently has a bug which doesn't allow you to set default headers
    // This headers comply with CORS and allow us to mongodbServer our response to any origin

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
            log.warn(err, "There is no tasks in the database. Add one!");
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


#### <a name="add-error-handling-for-the-routes"></a>Hinzufügen einer Fehlerbehandlungsroutine für Routen
Fügen Sie eine Fehlerbehandlung hinzu, sodass möglicherweise aufgetretene Probleme auf verständliche Weise an den Client zurückgegeben werden.

Fügen Sie den folgenden Code hinzu:

```Javascript
///--- Errors for communicating something interesting back to the client
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


## <a name="create-your-server"></a>Erstellen des Servers
Sie haben jetzt Ihre Datenbank definiert und die Routen erstellt. Fügen Sie abschließend noch die Serverinstanz hinzu, die Ihre Aufrufe verwaltet.

Restify und Express bieten zahlreiche Anpassungsmöglichkeiten für einen REST-API-Server, hier verwenden wir jedoch eine möglichst einfache Konfiguration.

```Javascript

/**
 * Our Server
 */


var server = restify.createServer({
    name: "Microsoft Azure Active Directroy TODO Server",
    version: "2.0.1"
});

// Ensure we don't drop data on uploads
server.pre(restify.pre.pause());

// Clean up sloppy paths like //todo//////1//
server.pre(restify.pre.sanitizePath());

// Handles annoying user agents (curl)
server.pre(restify.pre.userAgentConnection());

// Set a per request bunyan logger (with requestid filled in)
server.use(restify.requestLogger());

// Allow 5 requests/second by IP, and burst to 10
server.use(restify.throttle({
    burst: 10,
    rate: 5,
    ip: true,
}));

// Use the common stuff you probably want
server.use(restify.acceptParser(server.acceptable));
server.use(restify.dateParser());
server.use(restify.queryParser());
server.use(restify.gzipResponse());
server.use(restify.bodyParser({
    mapParams: true
})); // Allows for JSON mapping to REST
server.use(restify.authorizationParser()); // Looks for authorization headers

// Let's start using Passport.js

server.use(passport.initialize()); // Starts passport
server.use(passport.session()); // Provides session support


```
## <a name="add-the-routes-to-the-server-without-authentication"></a>Hinzufügen der Routen zum Server (ohne Authentifizierung)
```Javascript
server.get('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), listTasks);
server.get('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), listTasks);
server.get('/api/tasks/:owner', passport.authenticate('oauth-bearer', {
    session: false
}), getTask);
server.head('/api/tasks/:owner', passport.authenticate('oauth-bearer', {
    session: false
}), getTask);
server.post('/api/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
    session: false
}), createTask);
server.post('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), createTask);
server.del('/api/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
    session: false
}), removeTask);
server.del('/api/tasks/:owner', passport.authenticate('oauth-bearer', {
    session: false
}), removeTask);
server.del('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), removeTask);
server.del('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), removeAll, function respond(req, res, next) {
    res.send(204);
    next();
});


// Register a default '/' handler

server.get('/', function root(req, res, next) {
    var routes = [
        'GET     /',
        'POST    /api/tasks/:owner/:task',
        'POST    /api/tasks (for JSON body)',
        'GET     /api/tasks',
        'PUT     /api/tasks/:owner',
        'GET     /api/tasks/:owner',
        'DELETE  /api/tasks/:owner/:task'
    ];
    res.send(200, routes);
    next();
});
```

```Javascript

server.listen(serverPort, function() {

    var consoleMessage = '\n Microsoft Azure Active Directory Tutorial';
    consoleMessage += '\n +++++++++++++++++++++++++++++++++++++++++++++++++++++';
    consoleMessage += '\n %s server is listening at %s';
    consoleMessage += '\n Open your browser to %s/api/tasks\n';
    consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n';
    consoleMessage += '\n !!! why not try a $curl -isS %s | json to get some ideas? \n';
    consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n\n';

    //log.info(consoleMessage, server.name, server.url, server.url, server.url);

});

```

## <a name="add-authentication-to-your-rest-api-server"></a>Hinzufügen der Authentifizierung zum REST-API-Server
Nachdem der REST-API-Server nun ausgeführt wird, können Sie ihn mit Azure AD nutzen.

Wechseln Sie über die Befehlszeile zum Verzeichnis `azuread`, sofern Sie sich nicht bereits dort befinden:

`cd azuread`

### <a name="use-the-oidcbearerstrategy-that-is-included-with-passport-azure-ad"></a>Verwenden von „OIDCBearerStrategy“ (in „passport-azure-ad“ enthalten)
> [!TIP]
> Beim Schreiben von APIs sollten Sie die Daten immer mit einem eindeutigen Element aus dem Token verknüpfen, das der Benutzer nicht spoofen kann. Wenn der Server ToDo-Elemente speichert, tut er dies auf der Grundlage der **oid** des Benutzers im Token (aufgerufen über „token.oid“), die in das Feld „owner“ eingefügt wird. Durch den Wert wird sichergestellt, dass nur dieser Benutzer auf seine eigenen ToDo-Elemente zugreifen kann. Es ist keine Offenlegung in den APIs von „owner“ vorgesehen, also kann ein externer Benutzer ToDo-Elemente von anderen anfordern, selbst wenn eine Authentifizierung erfolgt ist.
>
>

Verwenden Sie als Nächstes die zu `passport-azure-ad`gehörende Bearer-Strategie.

```Javascript
var findById = function(id, fn) {
    for (var i = 0, len = users.length; i < len; i++) {
        var user = users[i];
        if (user.oid === id) {
            log.info('Found user: ', user);
            return fn(null, user);
        }
    }
    return fn(null, null);
};


var oidcStrategy = new OIDCBearerStrategy(options,
    function(token, done) {
        log.info('verifying the user');
        log.info(token, 'was the token retreived');
        findById(token.sub, function(err, user) {
            if (err) {
                return done(err);
            }
            if (!user) {
                // "Auto-registration"
                log.info('User was added automatically as they were new. Their sub is: ', token.oid);
                users.push(token);
                owner = token.oid;
                return done(null, token);
            }
            owner = token.sub;
            return done(null, user, token);
        });
    }
);

passport.use(oidcStrategy);
```

Passport verwendet für alle Strategien das gleiche Muster. Sie übergeben ein `function()`-Element mit `token` und `done` als Parameter. Die Strategie kehrt wieder an Ihren Ausgangspunkt zurück, sobald alle Aufgaben abgeschlossen sind. Sie sollten den Benutzer und das Token dann speichern, damit Sie beides nicht mehr erfragen müssen.

> [!IMPORTANT]
> Der obige Code erfasst alle Benutzer, die sich an Ihrem Server authentifizieren. Dieser Prozess wird als automatische Registrierung bezeichnet. Stellen Sie bei Produktionsservern sicher, dass Benutzer erst nach Durchlaufen eines Registrierungsprozesses auf die API zugreifen können. Dieses Muster wird üblicherweise bei Consumer-Apps verwendet, bei denen Sie sich über Facebook registrieren können und anschließend noch weitere Informationen angeben müssen. Wenn es sich hierbei nicht um ein Befehlszeilenprogramm handeln würde, könnten wir die E-Mail aus dem zurückgegebenen Tokenobjekt extrahieren und dann Benutzer dazu auffordern, zusätzliche Informationen einzugeben. In diesem Beispiel fügen wir sie einer In-Memory Database hinzu.
>
>

## <a name="run-your-server-application-to-verify-that-it-rejects-you"></a>Ausführen der Serveranwendung, um sicherzugehen, dass Sie abgelehnt werden
Mithilfe von `curl` können Sie ermitteln, ob Ihre Endpunkte nun durch OAuth2 geschützt sind. Die zurückgegebenen Header sollten bereits zeigen, dass Sie auf dem richtigen Weg sind.

Vergewissern Sie sich, dass die MongoDB-Instanz ausgeführt wird:

    $sudo mongodb

Wechseln Sie zum Verzeichnis, und führen Sie den Server aus:

    $ cd azuread
    $ node server.js

Führen Sie in einem neuen Terminalfenster `curl`

Probieren Sie es mit einem einfachen POST:

`$ curl -isS -X POST http://127.0.0.1:3000/api/tasks/brandon/Hello`

```Shell
HTTP/1.1 401 Unauthorized
Connection: close
WWW-Authenticate: Bearer realm="Users"
Date: Tue, 14 Jul 2015 05:45:03 GMT
Transfer-Encoding: chunked
```

Ein 401-Fehler ist die gewünschte Antwort. Sie gibt an, dass die Passport-Ebene eine Umleitung an den Autorisierungsendpunkt versucht.

## <a name="you-now-have-a-rest-api-service-that-uses-oauth2"></a>Sie verfügen jetzt über einen REST-API-Dienst, der OAuth2 verwendet
Sie haben eine REST-API mit Restify und OAuth implementiert! Nun verfügen Sie über genügend Code, um mit der Entwicklung Ihres Diensts fortzufahren und auf diesem Beispiel aufzubauen. Sie haben nun mit diesem Server alles getan, was noch ohne einem OAuth2-konformen Client geht. Verwenden Sie für den nächsten Schritt eine zusätzliche Anleitung (etwa die exemplarische Vorgehensweise [Azure AD B2C: Aufrufen einer Web-API aus einer iOS-Anwendung mit einer Drittanbieterbibliothek](active-directory-b2c-devquickstarts-ios.md) ).

## <a name="next-steps"></a>Nächste Schritte
Sie können nun mit den Themen für fortgeschrittenere Benutzer fortfahren. Beispiel:

[Herstellen einer Verbindung mit einer Web-API mithilfe von iOS mit B2C](active-directory-b2c-devquickstarts-ios.md)

