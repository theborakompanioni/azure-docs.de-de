<properties
	pageTitle="Bereitstellen einer Sails.js-Web-App in Azure App Service"
	description="Erfahren Sie, wie Sie eine Node.js-Anwendung in Azure App Service bereitstellen. In diesem Tutorial wird gezeigt, wie Sie eine Sails.js-Web-App bereitstellen."
	services="app-service\web"
	documentationCenter="nodejs"
	authors="cephalin"
	manager="wpickett"
	editor=""/>

<tags
	ms.service="app-service-web"
	ms.workload="web"
	ms.tgt_pltfrm="na"
	ms.devlang="nodejs"
	ms.topic="article"
	ms.date="03/31/2016"
	ms.author="cephalin"/>

# Bereitstellen einer Sails.js-Web-App in Azure App Service

In diesem Tutorial wird gezeigt, wie Sie eine Sails.js-Web-App in Azure App Service bereitstellen. Bei diesem Vorgang können Sie einige allgemeine Kenntnisse über das Konfigurieren Ihrer Node.js-App für die Ausführung in App Service erwerben.

## Voraussetzungen

- Node.js. Binärdateien für die Installation finden Sie [hier](https://nodejs.org/).
- Sails.js. Eine Installationsanleitung finden Sie [hier](http://sailsjs.org/get-started).
- Ausreichende Kenntnisse zu Sails.js. Dieses Tutorial ist nicht dazu gedacht, Ihnen bei Problemen im Zusammenhang mit Sail.js im Allgemeinen zu helfen.
- Git. Binärdateien für die Installation finden Sie [hier](http://www.git-scm.com/downloads).
- Azure-Befehlszeilenschnittstelle. Eine Installationsanleitung finden Sie [hier](../xplat-cli-install.md).
- Ein Microsoft Azure-Konto. Wenn Sie kein Konto haben, können Sie sich [für eine kostenlose Testversion registrieren](/pricing/free-trial/?WT.mc_id=A261C142F) oder [Ihre Visual Studio-Abonnentenvorteile aktivieren](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

>[AZURE.NOTE] Unter [Azure App Service testen](http://go.microsoft.com/fwlink/?LinkId=523751) können Sie Azure App Service in Aktion erleben, bevor Sie sich für ein Azure-Konto registrieren. Dort können Sie sofort eine kurzzeitige Start-App in App Service erstellen – ohne Kreditkarte und weitere Verpflichtungen.

## Schritt 1: Erstellen einer Sails.js-App in der Entwicklungsumgebung

Erstellen Sie zuerst schnell eine standardmäßige Sails.js-App, indem Sie die folgenden Schritte ausführen:

1. Öffnen Sie ein Befehlszeilenterminal Ihrer Wahl, und wechseln Sie mit `CD` in ein Arbeitsverzeichnis.

2. Erstellen Sie eine neue Sails.js-App, und führen Sie sie aus:

        sails new <appname>
        cd <appname>
        sails lift

    Stellen Sie sicher, dass Sie zur Standardstartseite unter http://localhost:1377 navigieren können.

## Schritt 2: Erstellen der App Service-App-Ressource in Azure

Erstellen Sie als Nächstes die App Service-App-Ressource. Sie werden ihr später Ihre Sails.js-App bereitstellen.

1. Melden Sie sich im gleichen Terminal wie folgt bei Azure an:

        azure login

    Befolgen Sie die Aufforderung, um die Anmeldung in einem Browser mit einem Microsoft-Konto fortzusetzen, das über Ihr Azure-Abonnement verfügt.

2. Stellen Sie sicher, dass Sie sich noch im Stammverzeichnis Ihres Sails.js-Projekts befinden. Erstellen Sie die App Service-App-Ressource in Azure mit einem eindeutigen App-Namen mit dem nächsten Befehl. Die URL der Web-App lautet „http://&lt;appname>.azurewebsites.net“.

        azure site create --git <appname>

    Folgen Sie der Aufforderung zum Auswählen einer Azure-Region für die Bereitstellung. Wenn Sie für Ihr Azure-Abonnement noch nie zuvor Git/FTP-Anmeldeinformationen für die Bereitstellung eingerichtet haben, werden Sie aufgefordert, diese zu erstellen.

    Sobald die App Service-App-Ressource erstellt wurde:

    - Die Sails.js-App wird für Git initialisiert.
    - Ihr lokales für Git initialisiertes Repository wird mit der neuen App Service-App als ein Git-Remoterepository mit dem passenden Namen „Azure“ verbunden.
    - Die Datei „iisnode.yml“ wird im Stammverzeichnis erstellt. Sie können mit dieser Datei [iisnode](https://github.com/tjanczuk/iisnode) konfigurieren. Damit werden in App Service Node.js-Apps ausgeführt.

## Schritt 3: Konfigurieren und Bereitstellen Ihrer Sails.js-App

 Das Arbeiten mit einer Sails.js-App in App Service umfasst drei Hauptschritte:

 - Konfigurieren der App für die Ausführung in App Service
 - Bereitstellen der App in App Service
 - Überprüfen aller stderr- und stdout-Protokolle zur Behandlung von Bereitstellungsfehlern

Folgen Sie diesen Schritten:

1. Öffnen Sie die neue Datei „iisnode.yml“ im Stammverzeichnis, und fügen Sie die beiden folgenden Zeilen hinzu:

        loggingEnabled: true
        logDirectory: iisnode

    Die Protokollierung ist jetzt für „iisnode“ aktiviert. Weitere Informationen zur Funktionsweise finden Sie unter [Abrufen von stdout- und stderr-Protokollen von iisnode](app-service-web-nodejs-sails.md#iisnodelog).

2. Öffnen Sie „config/env/production.js“, um Ihre Produktionsumgebung zu konfigurieren, und legen Sie `port` und `hookTimeout` fest:

        module.exports = {

            // Use process.env.port to handle web requests to the default HTTP port
            port: process.env.port,
            // Increase hooks timout to 30 seconds
            // This avoids the Sails.js error documented at https://github.com/balderdashy/sails/issues/2691
            hookTimeout: 30000,

            ...
        };

    Sie finden die Dokumentation für diese Konfigurationseinstellungen in der [Sails.js-Dokumentation](http://sailsjs.org/documentation/reference/configuration/sails-config).

    Als Nächstes müssen Sie sicherstellen, dass [Grunt](https://www.npmjs.com/package/grunt) mit den Netzlaufwerken von Azure kompatibel ist. Zum Zeitpunkt, als dieser Artikel verfasst wurde, konnte Grunt den Fehler [ENOTSUP: operation not supported on socket](https://github.com/isaacs/node-glob/issues/205) erzeugen, da aktuell ein veraltetes [Glob](https://www.npmjs.com/package/glob)-Paket (v3.1.21) verwendet wird, das Netzlaufwerke nicht unterstützt. In den nächsten Schritten wird gezeigt, wie Sie Grunt für die Verwendung von [Glob v5.0.14 oder höher](https://github.com/isaacs/node-glob/commit/bf3381e90e283624fbd652835e1aefa55d45e2c7) einrichten.

3. Da `npm install` bereits ausgeführt wurde, als Ihre App erstellt wurde, generieren Sie „npm-shrinkwrap.json“ im Stammverzeichnis des Projekts:

        npm shrinkwrap

4. Öffnen Sie „npm-shrinkwrap.json“, suchen Sie den JSON-Code für `"grunt":`, und fügen Sie dann die Abhängigkeit für die gewünschte Glob-Version hinzu. Der fertige JSON-Code sollte wie folgt aussehen:

        "grunt": {
            "version": "0.4.5",
            "from": "grunt@0.4.5",
            "resolved": "https://registry.npmjs.org/grunt/-/grunt-0.4.5.tgz",
            "dependencies": {
                "glob": {
                    "version": "5.0.14",
                    "from": "glob@5.0.14",
                    "resolved": "https://registry.npmjs.org/glob/-/glob-5.0.14.tgz"
                }
            }
        },

5. Suchen Sie mit `"glob":` alle Verweise auf Glob. Wenn ein Verweis v3.1.21 oder niedriger ist, ändern Sie den JSON-Code in:

        "glob": {
            "version": "5.0.14",
            "from": "glob@5.0.14",
            "resolved": "https://registry.npmjs.org/glob/-/glob-5.0.14.tgz"
        }

6. Speichern Sie die Änderungen, und testen Sie die Änderungen, um sicherzustellen, dass Ihre App noch lokal ausgeführt wird:

        npm install
        sails lift

4. Verwenden Sie jetzt Git, um Ihre App in Azure bereitzustellen:

        git add .
        git commit -m "<your commit message>"
        git push azure master

5. Starten Sie die fertige Azure-App jetzt einfach im Browser:

        azure site browse

    Sie sollten jetzt die gleiche Sails.js-Startseite sehen.
    
    ![](./media/app-service-web-nodejs-sails/sails-in-azure.png)

## Problembehandlung bei der Bereitstellung

Wenn Ihre Sails.js-Anwendung aus irgendeinem Grund in App Service ausfällt, suchen Sie in den stderr-Protokollen nach Informationen zur Problembehandlung. Weitere Informationen finden Sie unter [Abrufen von stdout- und stderr-Protokollen von iisnode](app-service-web-nodejs-sails.md#iisnodelog). Wenn sie erfolgreich gestartet wurde, sollte das stdout-Protokoll die vertraute Meldung anzeigen:

                .-..-.

    Sails              <|    .-..-.
    v0.12.1             |\
                        /|.\
                        / || \
                    ,'  |'  \
                    .-'.-==|/_--'
                    `--'-------' 
    __---___--___---___--___---___--___
    ____---___--___---___--___---___--___-__

    Server lifted in `D:\home\site\wwwroot`
    To see your app, visit http://localhost:\\.\pipe\a76e8111-663e-449d-956e-5c5deff2d304
    To shut down Sails, press <CTRL> + C at any time.

## Herstellen einer Verbindung mit einer Datenbank in Azure

Für eine Verbindung mit einer Datenbank in Azure erstellen Sie die Datenbank Ihrer Wahl in Azure, z. B. Azure SQL-Datenbank, MySQL, MongoDB, Azure (Redis) Cache usw., und verwenden den entsprechenden [Datenspeicheradapter](https://github.com/balderdashy/sails#compatibility), um die Verbindung herzustellen. Mit den Schritten in diesem Abschnitt wird gezeigt, wie eine Verbindung mit einer Azure SQL-Datenbank hergestellt wird.

1. Führen Sie das Tutorial [hier](../sql-database/sql-database-get-started.md) aus, um eine leere Azure SQL-Datenbank auf einer neuen SQL Server-Instanz zu erstellen. Die Standardeinstellungen der Firewall ermöglichen es, dass Azure-Dienste (z. B. App Service) eine Verbindung herstellen.

2. Installieren Sie über das Befehlszeilenterminal den SQL Server-Adapter:

        npm install sails-sqlserver --save

    Da Sie „package.json“ geändert haben, müssen Sie „npm-shrinkwrap.json“ neu generieren. Sie machen dies als Nächstes.
    
3. Löschen Sie das Verzeichnis „node\_modules/“.

4. Führen Sie `npm shrinkwrap` aus.

5. Öffnen Sie „npm-shrinkwrap.json“ erneut, und aktualisieren Sie die `glob`-Paketversionen wie im vorherigen Abschnitt.

    Jetzt zurück zur Hauptaufgabe.
        
3. Öffnen Sie „config/connections.js“, und fügen Sie der Liste der Adapter den folgenden JSON-Code hinzu:

        sqlserver: {
            adapter: 'sails-sqlserver',
            user: process.env.dbuser,
            password: process.env.dbpassword,
            host: process.env.sqlserver, 
            database: process.env.dbname,
            options: {
                encrypt: true   // use this for Azure databases
            }
        },

4. Für jede Umgebungsvariable (`process.env.*`) müssen Sie dies in App Service festlegen. Führen Sie dazu den folgenden Befehl über das Terminal aus:

        azure site appsetting add dbuser="<database server administrator>"
        azure site appsetting add dbpassword="<database server password>"
        azure site appsetting add sqlserver="<database server name>.database.windows.net"
        azure site appsetting add dbname="<database name>"
        
4. Öffnen Sie „config/env/production.js“, um Ihre Produktionsumgebung zu konfigurieren, und legen Sie `connection` und `migrate` im JSON-Objekt `models` fest:

        models: {
            connection: 'sqlserver',
            migrate: 'alter'
        },

4. [Generieren](http://sailsjs.org/documentation/reference/command-line-interface/sails-generate) Sie über das Terminal wie gewohnt eine Sails.js-[Blaupausen-API](http://sailsjs.org/documentation/concepts/blueprints). Beispiel:

         sails generate api mywidget
     
5. Speichern Sie alle Änderungen, übertragen Sie die Änderungen in Azure, und navigieren Sie zu Ihrer App, um sicherzustellen, dass sie weiterhin funktioniert.

        git add .
        git commit -m "<your commit message>"
        git push azure master
        azure site browse

6. Greifen Sie nun auf die Blaupausen-API zu, die Sie gerade im Browser erstellt haben. Beispiel:

        http://<appname>.azurewebsites.net/widget/create
    
    Die API sollte Ihnen den erstellten Eintrag im Browserfenster zurückgeben:
    
        {"id":1,"createdAt":"2016-03-28T23:08:01.000Z","updatedAt":"2016-03-28T23:08:01.000Z"}

## Weitere Ressourcen

- [Erste Schritte mit Node.js-Web-Apps in Azure App Service](app-service-web-nodejs-get-started.md)
- [Verwenden von Node.js-Modulen mit Azure-Anwendungen](../nodejs-use-node-modules-azure-apps.md)

<!---HONumber=AcomDC_0518_2016-->