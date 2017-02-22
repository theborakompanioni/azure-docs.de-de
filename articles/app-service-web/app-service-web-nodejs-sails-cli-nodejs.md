---
title: Bereitstellen einer Sails.js-Web-App in Azure App Service | Microsoft Docs
description: Erfahren Sie, wie Sie eine Node.js-Anwendung in Azure App Service bereitstellen. In diesem Tutorial wird gezeigt, wie Sie eine Sails.js-Web-App bereitstellen.
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: wpickett
editor: 
ms.assetid: 8877ddc8-1476-45ae-9e7f-3c75917b4564
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 12/16/2016
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: b1a633a86bd1b5997d5cbf66b16ec351f1043901
ms.openlocfilehash: 08b4cd3fe4a246bc62fb4a6cc596630968d7f9d7


---
# <a name="deploy-a-sailsjs-web-app-to-azure-app-service"></a>Bereitstellen einer Sails.js-Web-App in Azure App Service
In diesem Tutorial wird gezeigt, wie Sie eine Sails.js-Web-App in Azure App Service bereitstellen. Bei diesem Vorgang können Sie einige allgemeine Kenntnisse über das Konfigurieren Ihrer Node.js-App für die Ausführung in App Service erwerben.

Hier werden die folgenden nützlichen Fähigkeiten vermittelt:

* Konfigurieren der App „Sails.js“ in App Service
* Bereitstellen einer App für App Service über die Befehlszeile
* Überprüfen aller stderr- und stdout-Protokolle zur Behandlung von Bereitstellungsfehlern
* Speichern von Umgebungsvariablen außerhalb der Quellcodeverwaltung
* Zugreifen auf Azure-Umgebungsvariablen über Ihre App
* Herstellen einer Verbindung mit einer Datenbank (MongoDB)

Sie sollten über ausreichende Kenntnisse zu Sails.js verfügen. Dieses Tutorial ist nicht dazu gedacht, Ihnen bei Problemen im Zusammenhang mit Sail.js im Allgemeinen zu helfen.

## <a name="cli-versions-to-complete-the-task"></a>CLI-Versionen zum Durchführen dieser Aufgabe

Führen Sie die Aufgabe mit einer der folgenden CLI-Versionen durch:

- [Azure CLI 1.0:](app-service-web-nodejs-sails-cli-nodejs.md) Unsere CLI für das klassische Bereitstellungsmodell und das Resource Manager-Bereitstellungsmodell
- [Azure CLI 2.0 (Vorschau):](app-service-web-nodejs-sails.md) Unsere CLI der nächsten Generation für das Resource Manager-Bereitstellungsmodell

## <a name="prerequisites"></a>Voraussetzungen
* [Node.js](https://nodejs.org/)
* [Sails.js](http://sailsjs.org/get-started)
* [Git](http://www.git-scm.com/downloads)
* [Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md)
* Ein Microsoft Azure-Konto. Falls Sie noch kein Konto haben, können Sie sich [für eine kostenlose Testversion registrieren](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) oder [Ihre Visual Studio-Abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

> [!NOTE]
> Zum [Testen von App Service](https://azure.microsoft.com/try/app-service/) benötigen Sie kein Azure-Konto. Sie können eine Starter-App erstellen und bis zu einer Stunde damit experimentieren – ohne Kreditkarte und ohne jegliche Verpflichtungen.
> 
> 

## <a name="step-1-create-a-sailsjs-app-locally"></a>Schritt 1: Lokales Erstellen einer Sails.js-App
Erstellen Sie zuerst schnell eine standardmäßige Sails.js-App in Ihrer Entwicklungsumgebung, indem Sie die folgenden Schritte ausführen:

1. Öffnen Sie ein Befehlszeilenterminal Ihrer Wahl, und wechseln Sie mit `CD` in ein Arbeitsverzeichnis.
2. Erstellen Sie eine Sails.js-App, und führen Sie sie aus:

        sails new <appname>
        cd <appname>
        sails lift

    Stellen Sie sicher, dass Sie zur Standardstartseite unter „ http://localhost:1377 “ navigieren können.

## <a name="step-2-create-the-azure-app-resource"></a>Schritt 2: Erstellen der Azure-App-Ressource
Als Nächstes erstellen Sie die App Service-Ressource in Azure. Sie werden ihr später Ihre Sails.js-App bereitstellen.

1. Melden Sie sich wie folgt bei Azure an:
2. Wechseln Sie im gleichen Terminal in den ASM-Modus, und melden Sie sich bei Azure an:

        azure config mode asm
        azure login

    Befolgen Sie die Aufforderung, um die Anmeldung in einem Browser mit einem Microsoft-Konto fortzusetzen, das über Ihr Azure-Abonnement verfügt.

3. Legen Sie den Bereitstellungsbenutzer für App Service fest. Sie werden später unter Verwendung der Anmeldeinformationen Code bereitstellen.
   
        azure site deployment user set --username <username> --pass <password>

3. Stellen Sie sicher, dass Sie sich noch im Stammverzeichnis Ihres Sails.js-Projekts befinden. Erstellen Sie die App Service-App-Ressource in Azure mit einem eindeutigen App-Namen mit dem nächsten Befehl. Die URL der Web-App lautet „http://&lt;appname>.azurewebsites.net“.

        azure site create --git <appname>

    Folgen Sie der Aufforderung zum Auswählen einer Azure-Region für die Bereitstellung. Sobald die App Service-App-Ressource erstellt wurde:

   * Die Sails.js-App wird für Git initialisiert.
   * Ihr lokales für Git initialisiertes Repository wird mit der neuen App Service-App als ein Git-Remoterepository mit dem passenden Namen „Azure“ verbunden.
   * Die Datei „iisnode.yml“ wird im Stammverzeichnis erstellt. Sie können mit dieser Datei [iisnode](https://github.com/tjanczuk/iisnode) konfigurieren. Damit werden in App Service Node.js-Apps ausgeführt.

## <a name="step-3-configure-and-deploy-your-sailsjs-app"></a>Schritt 3: Konfigurieren und Bereitstellen Ihrer Sails.js-App
 Das Arbeiten mit einer Sails.js-App in App Service umfasst drei Hauptschritte:

* Konfigurieren der App für die Ausführung in App Service
* Bereitstellen der App in App Service
* Überprüfen aller stderr- und stdout-Protokolle zur Behandlung von Bereitstellungsfehlern

Folgen Sie diesen Schritten:

1. Öffnen Sie die neue Datei „iisnode.yml“ im Stammverzeichnis, und fügen Sie die beiden folgenden Zeilen hinzu:

        loggingEnabled: true
        logDirectory: iisnode

    Die Protokollierung ist jetzt für den Server [iisnode](https://github.com/tjanczuk/iisnode) aktiviert, der von Azure App Service zum Ausführen von Node.js-Apps verwendet wird. 
    Weitere Informationen zur Funktionsweise finden Sie unter  [Abrufen von stdout- und stderr-Protokollen von iisnode](app-service-web-nodejs-get-started.md#iisnodelog).
2. Öffnen Sie „config/env/production.js“, um Ihre Produktionsumgebung zu konfigurieren, und legen Sie `port` und `hookTimeout` fest:

        module.exports = {

            // Use process.env.port to handle web requests to the default HTTP port
            port: process.env.port,
            // Increase hooks timout to 30 seconds
            // This avoids the Sails.js error documented at https://github.com/balderdashy/sails/issues/2691
            hookTimeout: 30000,

            ...
        };

    Sie finden die Dokumentation für diese Konfigurationseinstellungen in der  [Sails.js-Dokumentation](http://sailsjs.org/documentation/reference/configuration/sails-config).

4. Fügen Sie in „package.json“ die folgende `engines` -Eigenschaft hinzu, um die gewünschte Node.js-Version festzulegen.

        "engines": {
            "node": "6.9.1"
        },
5. Speichern und testen Sie Ihre Änderungen, um sicherzustellen, dass Ihre App noch lokal ausgeführt wird. Löschen Sie hierzu den Ordner `node_modules` , und führen Sie dann Folgendes aus:

        npm install
        sails lift
6. Verwenden Sie jetzt Git, um Ihre App in Azure bereitzustellen:

        git add .
        git commit -m "<your commit message>"
        git push azure master
7. Starten Sie die fertige Azure-App jetzt einfach im Browser:

        azure site browse

    Sie sollten jetzt die gleiche Sails.js-Startseite sehen.

    ![](./media/app-service-web-nodejs-sails/sails-in-azure.png)

## <a name="troubleshoot-your-deployment"></a>Problembehandlung bei der Bereitstellung
Wenn Ihre Sails.js-Anwendung aus irgendeinem Grund in App Service ausfällt, suchen Sie in den stderr-Protokollen nach Informationen zur Problembehandlung.
Weitere Informationen finden Sie unter [Abrufen von stdout- und stderr-Protokollen von iisnode](app-service-web-nodejs-get-started.md#get-stdout-and-stderr-logs-from-iisnode).
Wenn sie erfolgreich gestartet wurde, sollte das stdout-Protokoll die vertraute Meldung anzeigen:

                   .-..-.
    
       Sails              <|    .-..-.
       v0.12.11            |\
                          /|.\
                         / || \
                       ,'  |'  \
                    .-'.-==|/_--'
                    `--'-------' 
       __---___--___---___--___---___--___
     ____---___--___---___--___---___--___-__

    Server lifted in `D:\home\site\wwwroot`
    To see your app, visit http://localhost:\\.\pipe\c775303c-0ebc-4854-8ddd-2e280aabccac
    To shut down Sails, press <CTRL> + C at any time.

Die Granularität der stdout-Protokolle kann in der Datei [config/log.js](http://sailsjs.org/#!/documentation/concepts/Logging) gesteuert werden.

## <a name="connect-to-a-database-in-azure"></a>Herstellen einer Verbindung mit einer Datenbank in Azure
Erstellen Sie zum Herstellen einer Verbindung mit einer Datenbank in Azure die gewünschte Datenbank in Azure (Azure SQL-Datenbank, MySQL, MongoDB, Azure [Redis] Cache oder Ähnliches), und verwenden Sie den entsprechenden [Datenspeicheradapter](https://github.com/balderdashy/sails#compatibility) , um die Verbindung herzustellen. Die Schritte in diesem Abschnitt veranschaulichen, wie Sie eine Verbindung mit MongoDB herstellen, indem Sie eine [Azure DocumentDB](../documentdb/documentdb-protocol-mongodb.md)-Datenbank verwenden, die MongoDB-Clientverbindungen unterstützt.

1. [Erstellen Sie ein DocumentDB-Konto mit Protokollunterstützung für MongoDB](../documentdb/documentdb-create-mongodb-account.md).
2. [Erstellen Sie eine DocumentDB-Sammlung und -Datenbank](../documentdb/documentdb-create-collection.md). Der Name der Sammlung spielt keine Rolle, aber Sie benötigen den Namen der Datenbank, wenn Sie eine Verbindung von „Sails.js“ herstellen.
3. [Suchen Sie nach den Verbindungsinformationen für Ihre DocumentDB-Datenbank](../documentdb/documentdb-connect-mongodb-account.md#a-idgetcustomconnectiona-get-the-mongodb-connection-string-to-customize).
2. Installieren Sie den MongoDB-Adapter über das Befehlszeilenterminal:

        npm install sails-mongo --save

3. Öffnen Sie „config/connections.js“, und fügen Sie der Liste das folgende Verbindungsobjekt hinzu:

        docDbMongo: {
            adapter: 'sails-mongo',
            user: process.env.dbuser,
            password: process.env.dbpassword,
            host: process.env.dbhost,
            port: process.env.dbport,
            database: process.env.dbname,
            ssl: true
        },

    > [!NOTE] 
    > Die Option `ssl: true` ist wichtig, weil sie [für Azure DocumentDB erforderlich ist](../documentdb/documentdb-connect-mongodb-account.md#connection-string-requirements). 
    >
    >

4. Dies muss in App Service für jede Umgebungsvariable (`process.env.*`) festgelegt werden. Führen Sie dazu die folgenden Befehle über das Terminal aus. Verwenden Sie die Verbindungsinformationen für Ihre DocumentDB-Datenbank.

        azure site appsetting add dbuser="<database user>"
        azure site appsetting add dbpassword="<database password>"
        azure site appsetting add dbhost="<database hostname>"
        azure site appsetting add dbport="<database port>"
        azure site appsetting add dbname="<database name>"

    Durch die Platzierung Ihrer Einstellungen in den Azure-App-Einstellungen bleiben vertrauliche Daten von Ihrer Quellcodeverwaltung (Git) getrennt. Als Nächstes konfigurieren Sie Ihre Entwicklungsumgebung für die Verwendung der gleichen Verbindungsinformationen.
5. Öffnen Sie „config/local.js“, und fügen Sie das folgende Verbindungsobjekt hinzu:

        connections: {
            docDbMongo: {
                user: "<database user>",
                password: "<database password>",
                host: "<database hostname>",
                database: "<database name>",
                ssl: true
            },
        },

    Diese Konfiguration überschreibt die Einstellungen in der Datei „config/connections.js“ für die lokale Umgebung. Die Datei wird durch die standardmäßige GITIGNORE-Datei in Ihrem Projekt ausgeschlossen und somit nicht in Git gespeichert. Nun können Sie sowohl über Ihre Azure-Web-App als auch über Ihre lokale Entwicklungsumgebung eine Verbindung mit der DocumentDB-Datenbank (MongoDB) herstellen.
6. Öffnen Sie „config/env/production.js“, um Ihre Produktionsumgebung zu konfigurieren, und fügen Sie das folgende `models` -Objekt hinzu:

        models: {
            connection: 'docDbMongo',
            migrate: 'safe'
        },
7. Öffnen Sie „config/env/development.js“, um Ihre Entwicklungsumgebung zu konfigurieren, und fügen Sie das folgende `models`-Objekt hinzu:

        models: {
            connection: 'docDbMongo',
            migrate: 'alter'
        },

    Mit `migrate: 'alter'` können Sie Features für die Datenbankmigration verwenden, um die Datenbanksammlungen oder -tabellen leicht zu erstellen und zu aktualisieren. Für die Azure-Umgebung (Produktion) wird allerdings `migrate: 'safe'` verwendet, da „Sails.js“ die Verwendung von `migrate: 'alter'` in einer Produktionsumgebung nicht zulässt. (Weitere Informationen finden Sie in der  [Sails.js-Dokumentation](http://sailsjs.org/documentation/concepts/models-and-orm/model-settings)).
8. [Generieren](http://sailsjs.org/documentation/reference/command-line-interface/sails-generate) Sie über das Terminal wie gewohnt eine Sails.js-[Blaupausen-API](http://sailsjs.org/documentation/concepts/blueprints). Führen Sie dann `sails lift` aus, um die Datenbank mit Sails.js-Datenbankmigration zu erstellen. Beispiel:

         sails generate api mywidget
         sails lift

    Das durch diesen Befehl erstellte `mywidget` -Modell ist zwar leer, wir können damit jedoch zeigen, dass eine Verbindung mit der Datenbank besteht.
    Wenn Sie `sails lift` ausführen, werden die fehlenden Sammlungen oder Tabellen für die von Ihrer App verwendeten Modelle erstellt.
9. Greifen Sie über den Browser auf die soeben erstellte Blaupausen-API zu. Beispiel:

        http://localhost:1337/mywidget/create

    Die API sollte den erstellen Eintrag im Browserfenster zurückgeben. So wissen Sie, dass die Datenbank erfolgreich erstellt wurde.

        {"id":1,"createdAt":"2016-09-23T13:32:00.000Z","updatedAt":"2016-09-23T13:32:00.000Z"}
10. Übertragen Sie Ihre Änderungen mittel Push an Azure, und navigieren Sie zu Ihrer App, um sich zu vergewissern, dass sie weiterhin funktioniert.

         git add .
         git commit -m "<your commit message>"
         git push azure master
         azure site browse
11. Greifen Sie auf die Blaupausen-API Ihrer Azure-Web-App zu. Beispiel:

         http://<appname>.azurewebsites.net/mywidget/create

     Wenn die API einen weiteren neuen Eintrag zurückgibt, kommuniziert Ihre Azure-Web-App mit der DocumentDB-Datenbank (MongoDB).

## <a name="more-resources"></a>Weitere Ressourcen
* [Erste Schritte mit Node.js-Web-Apps in Azure App Service](app-service-web-nodejs-get-started.md)
* [Verwenden von Node.js-Modulen mit Azure-Anwendungen](../nodejs-use-node-modules-azure-apps.md)



<!--HONumber=Feb17_HO3-->


