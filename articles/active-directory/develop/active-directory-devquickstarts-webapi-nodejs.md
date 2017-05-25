---
title: Erste Schritte in Azure AD Node.js | Microsoft-Dokumentation
description: "In diesem Thema erfahren Sie, wie Sie eine Node.js-REST-Web-API erstellen, die für die Authentifizierung in Azure AD integriert wird."
services: active-directory
documentationcenter: nodejs
author: brandwe
manager: mbaldwin
editor: 
ms.assetid: 7654ab4c-4489-4ea5-aba9-d7cdc256e42a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 01/07/2017
ms.author: xerners
ms.custom: aaddev
ms.translationtype: Human Translation
ms.sourcegitcommit: bb1ca3189e6c39b46eaa5151bf0c74dbf4a35228
ms.openlocfilehash: 707bb2b7cd9796e0e05234aa08002bbc1820f871
ms.contentlocale: de-de
ms.lasthandoff: 03/18/2017


---
# <a name="get-started-with-web-apis-for-nodejs"></a>Erste Schritte mit Web-APIs für Node.js
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

*Passport* ist eine Authentifizierungs-Middleware für Node.js. Passport ist flexibel und modular aufgebaut und kann unauffällig in jede Express- oder Restify-basierte Webanwendung integriert werden. Ein umfassender Satz an Strategien unterstützt die Authentifizierung mithilfe eines Benutzernamens und Kennworts in Facebook, Twitter und anderen Anwendungen. Wir haben eine Strategie für Microsoft Azure Active Directory (Azure AD) entwickelt. Dieses Modul installieren Sie nun und fügen dann das Microsoft Azure Active Directory-Plug-In `passport-azure-ad` hinzu.

Gehen Sie hierzu wie folgt vor:

1. Registrieren Sie eine Anwendung in Azure AD.
2. Richten Sie Ihre App für die Verwendung des `passport-azure-ad`-Plug-Ins von Passport ein.
3. Konfigurieren Sie eine Clientanwendung für den Aufruf der Web-API „To Do List“.

Der Code für dieses Tutorial wird [auf GitHub](https://github.com/Azure-Samples/active-directory-node-webapi)verwaltet.

> [!NOTE]
> In diesem Artikel wird das Implementieren der Anmeldung, Registrierung oder Profilverwaltung mit Azure AD B2C nicht behandelt. Er konzentriert sich auf das Aufrufen von Web-APIs aufrufen, nachdem der Benutzer bereits authentifiziert wurde.  Wir empfehlen Ihnen, zunächst das Dokument [Gewusst wie: Integration mit Azure Active Directory](active-directory-how-to-integrate.md) zu lesen, um mehr über die Grundlagen von Azure Active Directory zu erfahren.
>
>

Der vollständige Quellcode dieses funktionsfähigen Beispiels wurde auf GitHub unter einer MIT-Lizenz veröffentlicht. Sie können diesen Code gerne klonen oder sogar abwandeln sowie Feedback und Pullanforderungen (Pull Requests) senden.

## <a name="about-nodejs-modules"></a>Informationen zu Node.js-Modulen
In dieser exemplarischen Vorgehensweise verwenden wir Node.js-Module. Module sind ladbare JavaScript-Pakete, die bestimmte Funktionen für die Anwendung bieten. Normalerweise installieren Sie Module, indem Sie das NPM-Befehlszeilentool von Node.js im NPM-Installationsverzeichnis verwenden. Einige Module, z.B. das HTTP-Modul, sind aber im Node.js-Kernpaket enthalten.

Installierte Module werden im Verzeichnis **node_modules** im Stammverzeichnis Ihres Node.js-Installationsverzeichnisses gespeichert. Jedes Modul im Verzeichnis **node_modules** enthält wiederum ein eigenes **node_modules**-Verzeichnis mit allen Modulen, von denen es abhängig ist. Zudem verfügt jedes erforderliche Modul über das Verzeichnis **node_modules**. Diese rekursive Verzeichnisstruktur stellt die Abhängigkeitskette dar.

Diese Struktur mit einer Abhängigkeitskette führt zu einem größeren Platzbedarf für die Anwendung. Gleichzeitig wird hiermit aber auch sichergestellt, dass alle Abhängigkeiten erfüllt werden und dass die Version der Module, die während der Entwicklung verwendet wird, auch in der Produktion zum Einsatz kommt. Dadurch wird das Verhalten der in der Produktion verwendeten Anwendung berechenbarer und Versionsprobleme, die sich auf die Benutzer auswirken, werden verhindert.

## <a name="step-1-register-an-azure-ad-tenant"></a>Schritt 1: Registrieren eines Azure AD-Mandanten
Für die Verwendung dieses Beispiels benötigen Sie einen Azure Active Directory-Mandanten. Wenn Sie nicht wissen, was ein Mandant ist oder wie Sie einen erhalten, helfen Ihnen die Informationen unter [Einrichten eines Azure Active Directory-Mandanten](active-directory-howto-tenant.md) weiter.

## <a name="step-2-create-an-application"></a>Schritt 2: Erstellen einer Anwendung
Als Nächstes erstellen Sie eine App in Ihrem Verzeichnis, damit Azure AD die Informationen erhält, die für die sichere Kommunikation mit Ihrer App erforderlich sind.  In diesem Fall werden die Client-App und die Web-API durch eine einzelne **Anwendungs-ID** dargestellt, da sie zusammen eine logische App bilden.  Befolgen Sie zum Erstellen einer App [diese Anweisungen](active-directory-how-applications-are-added.md). Bei der Erstellung einer Branchen-App sind unter Umständen diese [zusätzlichen Anweisungen](../active-directory-applications-guiding-developers-for-lob-applications.md) nützlich.

Gehen Sie wie folgt vor, um eine Anwendung zu erstellen:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.

2. Wählen Sie Ihr Konto im oberen Menü aus. Wählen Sie in der **Verzeichnisliste** anschließend den Active Directory-Mandanten aus, bei dem Sie Ihre Anwendung registrieren möchten.

3. Wählen Sie im Menü auf der linken Seite die Option **Weitere Dienste** und dann **Azure Active Directory**.

4. Wählen Sie **App-Registrierungen** und dann **Hinzufügen** aus.

5. Folgen Sie den Bildschirmaufforderungen, und erstellen Sie eine **Webanwendung und/oder Web-API**.

      * Am **Namen** der Anwendung sollten die Endbenutzer die Funktion der Anwendung ablesen können.

      * Die **Anmelde-URL** ist die Basis-URL Ihrer Anwendung.  Die Standard-URL des Beispielcodes lautet `https://localhost:8080`.

6. Nach Abschluss der Registrierung weist Azure AD Ihrer App eine eindeutige Anwendungs-ID zu. Diesen Wert benötigen Sie in den nächsten Abschnitten. Daher sollten Sie ihn von der Anwendungsseite kopieren.

7. Aktualisieren Sie auf der Seite **Einstellungen** -> **Eigenschaften** für Ihre Anwendung den App-ID-URI. Die **App-ID-URI** ist eine eindeutige Kennung für die Anwendung. Standardmäßig wird `https://<tenant-domain>/<app-name>` verwendet, z.B. `https://contoso.onmicrosoft.com/my-first-aad-app`.

8. Erstellen Sie auf der Seite **Einstellungen** einen **Schlüssel** für Ihre Anwendung, und kopieren Sie ihn an einen zugänglichen Ort. Sie benötigen ihn in Kürze.

## <a name="step-3-download-nodejs-for-your-platform"></a>Schritt 3: Herunterladen von „Node.js“ für Ihre Plattform
Für die Verwendung dieses Beispiels benötigen Sie eine funktionsfähige Installation von Node.js.

Installieren Sie Node.js von [http://nodejs.org](http://nodejs.org).

## <a name="step-4-install-mongodb-on-your-platform"></a>Schritt 4: Installieren von MongoDB auf Ihrer Plattform
Für die Verwendung dieses Beispiels benötigen Sie eine funktionsfähige Installation von MongoDB. Sie verwenden MongoDB, um die REST-API über Serverinstanzen hinweg persistent zu machen.

Installieren Sie MongoDB von [http://mongodb.org](http://www.mongodb.org).

> [!NOTE]
> In dieser exemplarischen Vorgehensweise wird davon ausgegangen, dass Sie die Standardinstallation und die Standardserverendpunkte für MongoDB verwenden. Zum Zeitpunkt der Erstellung dieses Artikels ist dies „mongodb://localhost“.
>
>

## <a name="step-5-install-the-restify-modules-in-your-web-api"></a>Schritt 5: Installieren der Restify-Module in Ihrer Web-API
Wir verwenden Restify zum Erstellen unserer REST-API. Restify ist ein einfaches und flexibles Node.js-Anwendungsframework, das von Express abgeleitet ist. Er verfügt über eine Reihe robuster Funktionen für die Erstellung von REST-APIs für Connect.

### <a name="install-restify"></a>Installieren von Restify
1. Wechseln Sie an der Befehlszeile zum Verzeichnis **azuread**. Falls das Verzeichnis **azuread** nicht vorhanden ist, erstellen Sie es.

        `cd azuread - or- mkdir azuread; cd azuread`

2. Geben Sie folgenden Befehl ein:

    `npm install restify`

    Mit diesem Befehl wird Restify installiert.

#### <a name="did-you-get-an-error"></a>Bei einem Fehler
Beim Verwenden von NPM erhalten Sie für einige Betriebssysteme einen Fehler der Art **Error: EPERM, chmod '/usr/local/bin/..'**, und es wird vorgeschlagen, es mit der Ausführung des Kontos als Administrator zu versuchen. Verwenden Sie in diesem Fall den Befehl „sudo“, um NPM mit einer höheren Berechtigungsstufe auszuführen.

#### <a name="did-you-get-an-error-regarding-dtrace"></a>Haben Sie einen DTRACE-Fehler erhalten?
Bei der Installation von Restify erhalten Sie unter Umständen folgenden Fehler:

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
Restify stellt einen leistungsstarken Mechanismus zum Verfolgen von REST-Aufrufen mit DTrace bereit. Unter vielen Betriebssystemen ist DTrace aber nicht verfügbar. Sie können diese Fehler ignorieren.

Die Ausgabe dieses Befehls sollte ungefähr wie folgt aussehen:

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


## <a name="step-6-install-passportjs-in-your-web-api"></a>Schritt 6: Installieren von Passport.js in Ihrer Web-API
[Passport](http://passportjs.org/) ist eine Authentifizierungs-Middleware für Node.js. Passport ist flexibel und modular aufgebaut und kann unauffällig in jede Express- oder Restify-basierte Webanwendung integriert werden. Ein umfassender Satz an Strategien unterstützt die Authentifizierung mithilfe eines Benutzernamens und Kennworts in Facebook, Twitter und anderen Anwendungen.

Wir haben eine Strategie für Azure Active Directory entwickelt. Wir installieren dieses Modul und fügen dann das Plug-In für die Azure Active Directory-Strategie hinzu.

1. Wechseln Sie an der Befehlszeile zum Verzeichnis **azuread**.

2. Geben Sie für die Installation von Passport.js den folgenden Befehl ein:

    `npm install passport`

    Die Ausgabe des Befehls sollte in etwa wie folgt aussehen:

``
        passport@0.1.17 node_modules\passport
        ├── pause@0.0.1
        └── pkginfo@0.2.3
``

## <a name="step-7-add-passport-azure-ad-to-your-web-api"></a>Schritt 7: Hinzufügen von Passport-Azure-AD zu Ihrer Web-API
Als Nächstes fügen Sie die OAuth-Strategie hinzu. Hierzu verwenden Sie `passport-azure-ad` (Gruppe von Strategien, mit denen Azure Active Directory mit Passport verbunden wird). Wir verwenden diese Strategie für Bearertoken in diesem REST-API-Beispiel.

> [!NOTE]
> OAuth2 bietet zwar ein Framework, in dem alle bekannten Tokentypen ausgestellt werden können, aber es werden meist nur bestimmte Tokentypen verwendet. Bearertoken sind die am häufigsten verwendeten Token zum Schützen von Endpunkten. Diese Tokentypen werden in OAuth2 am häufigsten ausgestellt. Bei vielen Implementierungen wird davon ausgegangen, dass Bearertoken der einzige ausgestellte Tokentyp sind.
>
>

Wechseln Sie an der Befehlszeile zum Verzeichnis **azuread**.

Geben Sie für die Installation des `passport-azure-ad module` von Passport.js den folgenden Befehl ein:

`npm install passport-azure-ad`

Die Ausgabe dieses Befehls sollte ungefähr wie folgt aussehen:


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



## <a name="step-8-add-mongodb-modules-to-your-web-api"></a>Schritt 8: Hinzufügen der MongoDB-Module zu Ihrer Web-API
Wir verwenden MongoDB als Datenspeicher. Aus diesem Grund müssen wir das häufig genutzte Plug-In mit dem Namen Mongoose zum Verwalten von Modellen und Schemas installieren. Wir müssen auch den Datenbanktreiber für MongoDB (ebenfalls als MongoDB bezeichnet) installieren.

 `npm install mongoose`

## <a name="step-9-install-additional-modules"></a>Schritt 9: Installieren zusätzlicher Module
Nun installieren Sie alle weiteren erforderlichen Module.

1. Wechseln Sie an der Befehlszeile zum Ordner **azuread**, sofern noch nicht geschehen.

    `cd azuread`

2. Geben Sie zum Installieren dieser Module im Verzeichnis **node_modules** die folgenden Befehle ein:

    * `npm install assert-plus`
    * `npm install bunyan`
    * `npm update`

## <a name="step-10-create-a-serverjs-with-your-dependencies"></a>Schritt 10: Erstellen der Datei „server.js“ mit Ihren Abhängigkeiten
Die Datei „server.js“ enthält den Großteil der Funktionalität für unseren API-Server. Sie fügen den größten Teil unseres Codes dieser Datei hinzu. Für Produktionszwecke empfehlen wir Ihnen, die Funktionalität auf kleinere Dateien aufzuteilen und so beispielsweise Routen und Controller getrennt zu behandeln. In dieser Demonstration verwenden wir „server.js“ für diese Funktionalität.

1. Wechseln Sie an der Befehlszeile zum Ordner **azuread**, sofern noch nicht geschehen.

    `cd azuread`

2. Erstellen Sie die Datei `server.js` in Ihrem bevorzugten Editor, und fügen Sie ihr dann die folgenden Informationen hinzu:

    ```Javascript
        'use strict';

        /**
         * Module dependencies.
         */

        var fs = require('fs');
        var path = require('path');
        var util = require('util');
        var assert = require('assert-plus');
        var bunyan = require('bunyan');
        var getopt = require('posix-getopt');
        var mongoose = require('mongoose/');
        var restify = require('restify');
        var passport = require('passport');
      var BearerStrategy = require('passport-azure-ad').BearerStrategy;
    ```

3. Speichern Sie die Datei. Wir werden schon bald wieder auf diese Datei zurückkommen.

## <a name="step-11-create-a-config-file-to-store-your-azure-ad-settings"></a>Schritt 11: Erstellen einer Konfigurationsdatei für die Azure AD-Einstellungen
Diese Codedatei übergibt Ihre Konfigurationsparameter aus dem Azure Active Directory-Portal an Passport.js. Sie haben diese Konfigurationswerte erstellt, als Sie die Web-API im ersten Teil dieser exemplarischen Vorgehensweise dem Portal hinzugefügt haben. Welche Werte Sie für diese Parameter eingeben sollen, erfahren Sie, nachdem Sie den Code kopiert haben.

1. Wechseln Sie an der Befehlszeile zum Ordner **azuread**, sofern noch nicht geschehen.

    `cd azuread`

2. Erstellen Sie die Datei `config.js` in Ihrem bevorzugten Editor, und fügen Sie ihr dann die folgenden Informationen hinzu:

    ```Javascript
         exports.creds = {
             mongoose_auth_local: 'mongodb://localhost/tasklist', // Your mongo auth uri goes here
             clientID: 'your client ID',
             audience: 'your application URL',
            // you cannot have users from multiple tenants sign in to your server unless you use the common endpoint
          // example: https://login.microsoftonline.com/common/.well-known/openid-configuration
             identityMetadata: 'https://login.microsoftonline.com/<your tenant id>/.well-known/openid-configuration',
             validateIssuer: true, // if you have validation on, you cannot have users from multiple tenants sign in to your server
             passReqToCallback: false,
             loggingLevel: 'info' // valid are 'info', 'warn', 'error'. Error always goes to stderr in Unix.

         };
    ```
3. Speichern Sie die Datei .

## <a name="step-12-add-configuration-values-to-your-serverjs-file"></a>Schritt 12: Hinzufügen von Konfigurationswerten zur Datei „server.js“
Diese Werte müssen aus der Konfigurationsdatei gelesen werden, die Sie übergreifend für die Anwendung erstellt haben. Fügen Sie die Konfigurationsdatei in der Anwendung hierzu als erforderliche Ressource hinzu. Anschließend legen wir die globalen Variablen so fest, dass sie mit den Variablen im Dokument „config.js“ übereinstimmen.

1. Wechseln Sie an der Befehlszeile zum Ordner **azuread**, sofern noch nicht geschehen.

    `cd azuread`

2. Öffnen Sie die Datei `server.js` in Ihrem bevorzugten Editor, und fügen Sie ihr dann die folgenden Informationen hinzu:

    ```Javascript
    var config = require('./config');
    ```
3. Fügen Sie der Datei `server.js` dann einen neuen Abschnitt mit dem folgenden Code hinzu:

    ```Javascript
    var options = {
        // The URL of the metadata document for your app. We will put the keys for token validation from the URL found in the jwks_uri tag of the in the metadata.
        identityMetadata: config.creds.identityMetadata,
        clientID: config.creds.clientID,
        validateIssuer: config.creds.validateIssuer,
        audience: config.creds.audience,
        passReqToCallback: config.creds.passReqToCallback,
        loggingLevel: config.creds.loggingLevel

    };

    // Array to hold logged in users and the current logged in user (owner).
    var users = [];
    var owner = null;

    // Our logger.
    var log = bunyan.createLogger({
        name: 'Azure Active Directory Bearer Sample',
             streams: [
            {
                stream: process.stderr,
                level: "error",
                name: "error"
            },
            {
                stream: process.stdout,
                level: "warn",
                name: "console"
            }, ]
    });

      // If the logging level is specified, switch to it.
      if (config.creds.loggingLevel) { log.levels("console", config.creds.loggingLevel); }

    // MongoDB setup.
    // Set up some configuration.
    var serverPort = process.env.PORT || 8080;
    var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;
    ```

4. Speichern Sie die Datei .

## <a name="step-13-add-the-mongodb-model-and-schema-information-by-using-mongoose"></a>Schritt 13: Hinzufügen der Modell- und Schemainformationen von MongoDB mithilfe von Mongoose
Die vorangegangenen Vorbereitungen werden nun, da wir diese drei Dateien zu einem REST-API-Dienst kombinieren, Früchte tragen.

In dieser exemplarischen Vorgehensweise verwenden wir MongoDB zum Speichern unserer Aufgaben, wie unter Schritt 4 beschrieben.

In der Datei `config.js`, die wir in Schritt 11 erstellt haben, haben wir der Datenbank den Namen `tasklist` gegeben, denn dies war die Zeichenfolge, die wir am Ende der Verbindungs-URL **mongoose_auth_local** eingegeben haben. Sie müssen diese Datenbank nicht vorab in MongoDB erstellen. Stattdessen wird sie von MongoDB für uns bei der ersten Ausführung unserer Serveranwendung erstellt (sofern die Datenbank nicht bereits vorhanden ist).

Nachdem der Server nun weiß, welche MongoDB-Datenbank er verwenden soll, müssen wir weiteren Code schreiben, mit dem Modell und Schema der Serveraufgaben erstellt werden.

### <a name="discussion-of-the-model"></a>Beschreibung des Modells
Hierbei handelt es sich um ein einfaches Schemamodell. Sie können es nach Bedarf erweitern.

NAME: Der Name der Person, die der Aufgabe zugewiesen ist. Dies ist eine **Zeichenfolge**.

TASK: Die eigentliche Aufgabe. Dies ist eine **Zeichenfolge**.

DATE: Das Datum, an dem die Aufgabe fällig ist. Ein **DATETIME**-Wert.

COMPLETED: Gibt an, ob die Aufgabe abgeschlossen wurde. Ein **BOOLESCHER** Wert.

### <a name="creating-the-schema-in-the-code"></a>Erstellen des Schemas im Code
1. Wechseln Sie an der Befehlszeile zum Ordner **azuread**, sofern noch nicht geschehen.

    `cd azuread`

2. Öffnen Sie die Datei `server.js` in Ihrem bevorzugten Editor, und fügen Sie ihr nach dem Konfigurationseintrag dann die folgenden Informationen hinzu:

    ```Javascript
    // Connect to MongoDB.
    global.db = mongoose.connect(serverURI);
    var Schema = mongoose.Schema;
    log.info('MongoDB Schema loaded');

    // Here we create a schema to store our tasks and users. It's a fairly simple schema for now.
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
Wie im Code zu sehen ist, erstellen wir zuerst das Schema. Anschließend erstellen wir ein Modellobjekt, das wir zum Speichern unserer Daten im gesamten Code verwenden, wenn wir unsere **Routen** definieren.

## <a name="step-14-add-our-routes-for-our-task-rest-api-server"></a>Schritt 14: Hinzufügen der Routen für den Aufgaben-REST-API-Server
Nachdem das Datenbankmodell erstellt ist, fügen wir die Routen hinzu, die wir für den REST-API-Server benötigen.

### <a name="about-routes-in-restify"></a>Routen in Restify
Routen funktionieren in Restify genauso wie im Express-Stapel. Sie definieren die Routen mit den URIs, die die Client-Anwendungen Ihrer Erwartung nach aufrufen werden. In der Regel werden Routen in einer separaten Datei definiert. Für die Zwecke dieser Demonstration fügen wir die Routen aber der Datei „server.js“ hinzu. Wenn Sie das Beispiel für Produktionszwecke verwenden möchten, empfehlen wir Ihnen, die Routen in einer eigenen Datei zu definieren.

Ein typisches Muster für eine Restify-Route lautet:

```Javascript
function createObject(req, res, next) {

// Do work on object.

 _object.name = req.params.object; // passed value is in req.params under object

 ///...

return next(); // Keep the server going.
}

....

server.post('/service/:add/:object', createObject); // Calls createObject on routes that match this.

```


Dies ist das einfachste Muster. Restify (und Express) bieten weitaus tiefere Funktionalität, z.B. das Definieren von Anwendungstypen und Bereitstellen von komplexen Weiterleitungen über mehrere Endpunkte. Für unsere Zwecke halten wir diese Routen möglichst einfach.

### <a name="add-default-routes-to-our-server"></a>Hinzufügen von Standardrouten zum Server
Nachfolgend fügen Sie die CRUD-Routen (Create (Erstellen), Retrieve (Abrufen), Update (Aktualisieren) und Delete (Löschen)) hinzu.

1. Wechseln Sie an der Befehlszeile zum Ordner **azuread**, sofern noch nicht geschehen:

    `cd azuread`

2. Öffnen Sie die Datei `server.js` in Ihrem bevorzugten Editor, und fügen Sie dann unterhalb Ihrer vorherigen Datenbankeinträge die folgenden Informationen hinzu:

```Javascript

/**
 *
 * APIs for our REST Task server.
 */

// Create a task.

function createTask(req, res, next) {

    // Restify currently has a bug which doesn't allow you to set default headers.
    // These headers comply with CORS and allow us to mongodbServer our response to any origin.

    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");

    // Create a new task model, fill it, and save it to Mongodb.
    var _task = new Task();

    if (!req.params.task) {
        req.log.warn('createTodo: missing task');
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

/// Simple returns the list of TODOs that were loaded.

function listTasks(req, res, next) {
    // Restify currently has a bug which doesn't allow you to set default headers.
    // These headers comply with CORS and allow us to mongodbServer our response to any origin.

    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");

    log.info("listTasks was called for: ", owner);

    Task.find({
        owner: owner
    }).limit(20).sort('date').exec(function(err, data) {

        if (err) {
            return next(err);
        }

        if (data.length > 0) {
            log.info(data);
        }

        if (!data.length) {
            log.warn(err, "There is no tasks in the database. Did you initialize the database as stated in the README?");
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

### <a name="add-error-handling-in-our-apis"></a>Hinzufügen der Fehlerbehandlung in den APIs
```

///--- Errors for communicating something interesting back to the client.

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


## <a name="step-15-create-your-server"></a>Schritt 15: Erstellen des Servers
Wir haben unsere Datenbank definiert, und unsere Routen sind vorhanden. Als Letztes müssen wir nur noch die Serverinstanz zum Verwalten unserer Aufrufe hinzufügen.

In Restify (und Express) haben Sie für einen REST-API-Server sehr viele Anpassungsmöglichkeiten, aber wir verwenden für unsere Zwecke auch hier nur die grundlegenden Einstellungen.

```Javascript
/**
 * Our server.
 */


var server = restify.createServer({
    name: "Azure Active Directroy TODO Server",
    version: "2.0.1"
});

// Ensure we don't drop data on uploads.
server.pre(restify.pre.pause());

// Clean up sloppy paths like //todo//////1//.
server.pre(restify.pre.sanitizePath());

// Handle annoying user agents (curl).
server.pre(restify.pre.userAgentConnection());

// Set a per request bunyan logger (with requestid filled in).
server.use(restify.requestLogger());

// Allow five requests per second by IP, and burst to 10.
server.use(restify.throttle({
    burst: 10,
    rate: 5,
    ip: true,
}));

// Use the common stuff you probably want.
server.use(restify.acceptParser(server.acceptable));
server.use(restify.dateParser());
server.use(restify.queryParser());
server.use(restify.gzipResponse());
server.use(restify.bodyParser({
    mapParams: true
})); // Allow for JSON mapping to REST.
```

## <a name="step-16-add-the-routes-to-the-server-without-authentication-for-now"></a>Schritt 16: Hinzufügen der Routen zum Server (vorläufig ohne Authentifizierung)
```Javascript
/// Now the real handlers. Here we just CRUD.
/**
/*
/* Each of these handlers is protected by our OIDCBearerStrategy by invoking 'oidc-bearer'.
/* In the pasport.authenticate() method. We set 'session: false' because REST is stateless and
/* we don't need to maintain session state. You can experiment with removing API protection
/* by removing the passport.authenticate() method as follows:
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
// Register a default '/' handler.
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

## <a name="step-17-run-the-server-before-adding-oauth-support"></a>Schritt 17: Ausführen des Servers (vor dem Hinzufügen der OAuth-Unterstützung)
Testen Sie den Server vor dem Hinzufügen der Authentifizierung.

Am einfachsten können Sie Ihren Server testen, indem Sie „curl“ an einer Befehlszeile verwenden. Hierzu benötigen wir aber ein Hilfsprogramm, mit dem die Ausgabe als JSON-Code analysiert werden kann.

1. Installieren Sie das folgende JSON-Tool (es wird in allen folgenden Beispielen verwendet):

    `$npm install -g jsontool`

    Dadurch wird das Tool JSON global installiert. Jetzt können wir uns dem Server zuwenden:

2. Vergewissern Sie sich zuerst, dass die MongoDB-Instanz ausgeführt wird:

    `$sudo mongod`

3. Wechseln Sie danach zum Verzeichnis, und beginnen Sie mit dem „Curling“:

    `$ cd azuread`
    `$ node server.js`

    `$ curl -isS http://127.0.0.1:8080 | json`

    ```Shell
    HTTP/1.1 200 OK
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

4. Nun können Sie wie folgt eine Aufgabe hinzufügen:

    `$ curl -isS -X POST http://127.0.0.1:8080/tasks/brandon/Hello`

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
    Brandons Aufgaben können wir nun beispielsweise wie folgt auflisten:

        `$ curl -isS http://127.0.0.1:8080/tasks/brandon/`

Wenn all dies funktioniert, können wir OAuth dem REST-API-Server hinzufügen.

Sie verfügen über einen REST-API-Server mit MongoDB.

## <a name="step-18-add-authentication-to-our-rest-api-server"></a>Schritt 18: Hinzufügen der Authentifizierung zum REST-API-Server
Nachdem wir nun über eine ausgeführte REST-API verfügen, können wir sie für die Nutzung mit Azure AD einrichten.

Wechseln Sie an der Befehlszeile zum Ordner **azuread**, sofern noch nicht geschehen.

`cd azuread`

### <a name="use-the-oidcbearerstrategy-that-is-included-with-passport-azure-ad"></a>Verwenden von „OIDCBearerStrategy“ (in „passport-azure-ad“ enthalten)
Bisher haben wir einen normalen REST-TODO-Server ohne jegliche Autorisierung erstellt. Jetzt beginnen wir damit, dies zusammenzusetzen.

1. Zunächst müssen wir darauf hinweisen, dass wir Passport verwenden möchten. Fügen Sie diesen Hinweis direkt nach der sonstigen Serverkonfiguration ein:

    ```Javascript
            // Let's start using Passport.js.

            server.use(passport.initialize()); // Starts passport.
            server.use(passport.session()); // Provides session support.
    ```
    > [!TIP]
    > Für das Schreiben von APIs empfehlen wir Ihnen, die Daten immer mit einem eindeutigen Element aus dem Token zu verknüpfen, das der Benutzer nicht spoofen kann. Wenn dieser Server TODO-Elemente speichert, speichert er sie basierend auf der Objekt-ID des Benutzers im Token (Aufruf per „token.oid“), die wir in das Feld „owner“ aufnehmen. So wird sichergestellt, dass nur dieser Benutzer auf seine TODO-Elemente zugreifen kann. Es ist keine Offenlegung in der API von „owner“ vorgesehen. Also kann ein externer Benutzer die TODO-Elemente von anderen auch dann anfordern, wenn eine Authentifizierung durchgeführt wurde.                    

2. Verwenden Sie als Nächstes die zu `passport-azure-ad` gehörende Bearerstrategie. Sehen Sie sich zuerst einmal den Code an. Der Rest wird in Kürze beschrieben. Fügen Sie diesen Code nach dem Code ein, den Sie weiter oben hinzugefügt haben:

```Javascript
    /**
    /*
    /* Calling the OIDCBearerStrategy and managing users.
    /*
    /* Passport pattern provides the need to manage users and info tokens
    /* with a FindorCreate() method that must be provided by the implementor.
    /* Here we just auto-register any user and implement a FindById().
    /* You'll want to do something smarter.
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


    var bearerStrategy = new BearerStrategy(options,
        function(token, done) {
            log.info('verifying the user');
            log.info(token, 'was the token retreived');
            findById(token.sub, function(err, user) {
                if (err) {
                    return done(err);
                }
                if (!user) {
                    // "Auto-registration"
                    log.info('User was added automatically as they were new. Their sub is: ', token.sub);
                    users.push(token);
                    owner = token.sub;
                    return done(null, token);
                }
                owner = token.sub;
                return done(null, user, token);
            });
        }
    );

    passport.use(bearerStrategy);
```

Passport verwendet ein ähnliches Muster für alle Strategien (Twitter, Facebook usw.), das beim Schreiben aller Strategien eingehalten wird. Beim Betrachten der Strategie sehen Sie, dass eine Funktion übergeben wird, die token- und done-Elemente als Parameter aufweist. Die Strategie kehrt wieder an den Ausgangspunkt zurück, sobald die Arbeit abgeschlossen ist. Wenn dies der Fall ist, speichern wir den Benutzer und das Token, damit wir beides nicht mehr erfragen müssen.

> [!IMPORTANT]
> Der obige Code erfasst alle Benutzer, die sich an unserem Server authentifizieren. Dies wird als automatische Registrierung bezeichnet. Für Produktionsserver empfehlen wir, Benutzern den Zugriff nur zu erlauben, nachdem sie einen von Ihnen festgelegten Registrierungsprozess durchlaufen haben. Dies ist normalerweise das Muster, das Sie bei Consumer-Apps finden. Es ermöglicht Ihnen, sich bei Facebook zu registrieren, aber dann werden Sie aufgefordert, zusätzliche Informationen anzugeben. Wenn dies kein Befehlszeilenprogramm wäre, könnten wir die E-Mail aus dem Tokenobjekt extrahieren, das zurückgegeben wird, und dann Benutzer dazu auffordern, zusätzliche Informationen einzugeben. Da es sich um einen Testserver handelt, fügen wir sie einfach der Datenbank im Arbeitsspeicher hinzu.
>
>

### <a name="protect-some-endpoints"></a>Schützen einiger Endpunkte
Endpunkte schützen Sie mit dem Aufruf `passport.authenticate()`. Darin geben Sie das Protokoll an, das Sie verwenden möchten.

Wir bearbeiten nun die Route, um das Ergebnis des Servercodes interessanter zu gestalten.

```Javascript
server.get('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), listTasks);
server.get('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), listTasks);
server.get('/tasks/:owner', passport.authenticate('oauth-bearer', {
session: false
}), getTask);
server.head('/tasks/:owner', passport.authenticate('oauth-bearer', {
session: false
}), getTask);
server.post('/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
session: false
}), createTask);
server.post('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), createTask);
server.del('/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
session: false
}), removeTask);
server.del('/tasks/:owner', passport.authenticate('oauth-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), removeAll, function respond(req, res, next) {
res.send(204);
next();
});
```

## <a name="step-19-run-your-server-application-again-and-ensure-it-rejects-you"></a>Schritt 19: Erneutes Ausführen der Serveranwendung, wobei Sie nun abgelehnt werden sollten
Wir führen `curl` erneut aus, um festzustellen, ob unsere Endpunkte nun durch OAuth2 geschützt sind. Wir führen diesen Test durch, bevor eines unserer Client-SDKs an diesen Endpunkten ausgeführt wird. Die zurückgegebenen Header sollten bereits zeigen, dass wir uns auf dem richtigen Weg befinden.

1. Vergewissern Sie sich zuerst, dass die MongoDB-Instanz ausgeführt wird:

    `$sudo mongod`

2. Wechseln Sie danach zum Verzeichnis, und beginnen Sie mit dem „Curling“.

      `$ cd azuread`
      `$ node server.js`

3. Probieren Sie es mit einem einfachen POST.

    `$ curl -isS -X POST http://127.0.0.1:8080/tasks/brandon/Hello`

    ```Shell
    HTTP/1.1 401 Unauthorized
    Connection: close
    WWW-Authenticate: Bearer realm="Users"
    Date: Tue, 14 Jul 2015 05:45:03 GMT
    Transfer-Encoding: chunked
    ```

In diesem Fall möchten Sie eine 401-Antwort erhalten. Mit dieser Antwort wird angegeben, dass die Passportebene versucht, eine Umleitung an den autorisierten Endpunkt durchzuführen. Dies ist genau das, was wir erreichen möchten.

## <a name="next-steps"></a>Nächste Schritte
Sie haben für diesen Server nun alle Schritte ausgeführt, die ohne einen OAuth2-kompatiblen Client möglich sind. Für alle weiteren Schritte müssen Sie die nächste exemplarische Vorgehensweise durchführen.

Sie haben gelernt, wie Sie eine REST-API mit Restify und OAuth2 implementieren. Außerdem verfügen Sie über mehr als genug Code zum Weiterentwickeln Ihres Diensts und können auf diesem Beispiel aufbauen.

Wenn Sie Ihren Weg auf den Spuren von ADAL weiterverfolgen möchten, bieten wir Ihnen hier weitere unterstützte ADAL-Clients an, die wir Ihnen für Ihre Arbeit empfehlen können.

Erstellen Sie einem Klon auf Ihrem Entwicklungscomputer, und führen Sie die Konfiguration wie in der exemplarischen Vorgehensweise beschrieben durch.

[ADAL für iOS](https://github.com/MSOpenTech/azure-activedirectory-library-for-ios)

[ADAL für Android](https://github.com/MSOpenTech/azure-activedirectory-library-for-android)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]

