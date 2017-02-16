---
title: Erste Schritte mit Node.js-Web-Apps in Azure App Service | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine Node.js-Anwendung in einer Web-App in Azure App Service bereitstellen.
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: wpickett
editor: 
ms.assetid: fb2b90c8-02b6-4700-929b-5de9a35d67cc
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: get-started-article
ms.date: 12/16/2016
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: 88405a9e67eb748acc9564022283004b5ebfcf48
ms.openlocfilehash: 63210a5539d1e5e5b7d1f5a60048d507e53038a5


---
# <a name="get-started-with-nodejs-web-apps-in-azure-app-service"></a>Erste Schritte mit Node.js-Web-Apps in Azure App Service
[!INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

In diesem Tutorial erfahren Sie, wie Sie eine einfache [Node.js]-Anwendung erstellen und über eine Befehlszeilenumgebung wie „cmd.exe“ oder Bash für [Azure App Service] bereitstellen. Die Anweisungen in diesem Tutorial gelten für alle Betriebssysteme, unter denen Node.js ausgeführt werden kann.

[!INCLUDE [app-service-linux](../../includes/app-service-linux.md)]

<a name="prereq"></a>

## <a name="cli-versions-to-complete-the-task"></a>CLI-Versionen zum Durchführen dieser Aufgabe

Führen Sie die Aufgabe mit einer der folgenden CLI-Versionen durch:

- [Azure CLI 1.0:](app-service-web-nodejs-get-started-cli-nodejs.md) Unsere CLI für das klassische Bereitstellungsmodell und das Resource Manager-Bereitstellungsmodell
- [Azure CLI 2.0 (Vorschau):](app-service-web-nodejs-get-started.md) Unsere CLI der nächsten Generation für das Resource Manager-Bereitstellungsmodell

## <a name="prerequisites"></a>Voraussetzungen
* [Node.js]
* [Bower]
* [Yeoman]
* [Git]
* [Azure CLI 2.0 Preview](/cli/azure/install-az-cli2)
* Ein Microsoft Azure-Konto. Falls Sie noch kein Konto haben, können Sie sich [für eine kostenlose Testversion registrieren] oder [Ihre Visual Studio-Abonnentenvorteile aktivieren].

> [!NOTE]
> Zum [Testen von App Service](http://go.microsoft.com/fwlink/?LinkId=523751) benötigen Sie kein Azure-Konto. Sie können eine Starter-App erstellen und bis zu einer Stunde damit experimentieren – ohne Kreditkarte und ohne jegliche Verpflichtungen.
> 
> 

## <a name="create-and-configure-a-simple-nodejs-app-for-azure"></a>Erstellen und Konfigurieren einer einfachen Node.js-App für Azure
1. Öffnen Sie das Befehlszeilenterminal Ihrer Wahl, und installieren Sie den [Express Generator für Yeoman].
   
        npm install -g generator-express

2. `CD` in ein Arbeitsverzeichnis, und generieren Sie mit der folgenden Syntax eine Express-App:
   
        yo express
   
    Wählen Sie bei Aufforderung die folgenden Optionen aus:  
   
    `? Would you like to create a new directory for your project?` **Ja**  
    `? Enter directory name` **{appname}**  
    `? Select a version to install:` **MVC**  
    `? Select a view engine to use:` **Jade**  
    `? Select a css preprocessor to use (Sass Requires Ruby):` **Keine**  
    `? Select a database to use:` **Keine**  
    `? Select a build tool to use:` **Grunt**

3. Wechseln Sie mit `CD` zum Stammverzeichnis der neuen App, und starten Sie sie, um sicherzustellen, dass sie in Ihrer Entwicklungsumgebung ausgeführt werden kann:
   
        npm start
   
    Navigieren Sie in Ihrem Browser zu <http://localhost:3000>, um zu prüfen, ob die Express-Startseite angezeigt wird. Nachdem Sie sich vergewissert haben, dass die App richtig ausgeführt wird, können Sie sie mit `Ctrl-C` beenden.

6. Öffnen Sie die Datei „./config/config.js“ im Stammverzeichnis der Anwendung, und ändern Sie den Produktionsport in `process.env.port`. Die `production`-Eigenschaft im `config`-Objekt muss wie im folgenden Beispiel aussehen:
   
        production: {
            root: rootPath,
            app: {
                name: 'express1'
            },
            port: process.env.port,
        }
   
    > [!NOTE] 
    > Azure App Service führt Node.js-Anwendungen standardmäßig mit den Umgebungsvariablen vom Typ `production` (`process.env.NODE_ENV="production"`) aus.
    > Daher kann die Node.js-App in Azure bei dieser Konfiguration auf Webanforderungen über den Standardport reagieren, an dem „iisnode“ lauscht.
    >
    >

7. Öffnen Sie zum [Angeben der gewünschten Node.js-Version](#version) die Datei „./package.json“, und fügen Sie die `engines`-Eigenschaft hinzu.
   
        "engines": {
            "node": "6.9.1"
        }, 

8. Speichern Sie Ihre Änderungen. Initialisieren Sie anschließend ein Git-Repository am Stamm Ihrer Anwendung, und committen Sie Ihren Code:
   
        git add .
        git add -f config
        git commit -m "{your commit message}"

## <a name="deploy-your-nodejs-app-to-azure"></a>Bereitstellen der Node.js-App in Azure

1. Melden Sie sich über [Azure CLI 2.0 Preview](#prereq) bei Azure an:
   
        az login
   
    Befolgen Sie die Aufforderung, um die Anmeldung in einem Browser mit einem Microsoft-Konto fortzusetzen, das über Ihr Azure-Abonnement verfügt.

3. Legen Sie den Bereitstellungsbenutzer für App Service fest. Später stellen Sie Code mit diesen Anmeldeinformationen bereit.
   
        az appservice web deployment user set --user-name <username> --password <password>

3. Erstellen Sie eine neue [Ressourcengruppe](../azure-resource-manager/resource-group-overview.md). In diesem node.js-Tutorial müssen Sie nicht unbedingt wissen, worum es sich dabei genau handelt.

        az group create --location "<location>" --name my-nodejs-app-group

    Welche Werte Sie für `<location>` verwenden können, erfahren Sie mithilfe des CLI-Befehls `az appservice list-locations`.

3. Erstellen Sie einen neuen [App Service-Plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) vom Typ „FREE“. In diesem node.js-Tutorial reicht es zu wissen, dass Ihnen für Web-Apps im Rahmen dieses Plans keine Kosten entstehen.

        az appservice plan create --name my-nodejs-appservice-plan --resource-group my-nodejs-app-group --sku FREE

4. Erstellen Sie in `<app_name>` eine neue Web-App mit einem eindeutigen Namen.

        az appservice web create --name <app_name> --resource-group my-nodejs-app-group --plan my-nodejs-appservice-plan

5. Konfigurieren Sie mit dem folgenden Befehl die lokale Git-Bereitstellung für Ihre neue Web-App:

        az appservice web source-control config-local-git --name <app_name> --resource-group my-nodejs-app-group

    Sie erhalten eine JSON-Ausgabe wie die folgende, was bedeutet, dass das Git-Remoterepository konfiguriert ist:

        {
        "url": "https://<deployment_user>@<app_name>.scm.azurewebsites.net/<app_name>.git"
        }

6. Fügen Sie die URL im JSON-Code als Git-Remotespeicherort für Ihr lokales Repository (der Einfachheit halber `azure` genannt) hinzu.

        git remote add azure https://<deployment_user>@<app_name>.scm.azurewebsites.net/<app_name>.git
   
7. Stellen Sie Ihren Beispielcode für den Git-Remotespeicherort `azure` bereit. Geben Sie die zuvor konfigurierten Anmeldeinformationen für die Bereitstellung an, wenn Sie dazu aufgefordert werden.

        git push azure master
   
    Vom Express-Generator wird bereits eine Datei vom Typ „.gitignore“ bereitgestellt, sodass von `git push` beim Hochladen des Verzeichnisses „node_modules/“ keine Bandbreite beansprucht wird.

9. Starten Sie die fertige Azure-App jetzt im Browser:
   
        az appservice web browse --name <app_name> --resource-group my-nodejs-app-group
   
    Sie sollten jetzt verfolgen können, dass Ihre Node.js-Web-App live in Azure App Service ausgeführt wird.
   
    ![Beispiel für das Zugreifen auf die bereitgestellte Anwendung][deployed-express-app]

## <a name="update-your-nodejs-web-app"></a>Aktualisieren der Node.js-Web-App
Führen Sie zum Durchführen von Updates für die Node.js-Web-App, die in App Service ausgeführt wird, einfach wie bei der ersten Bereitstellung der Web-App `git add`, `git commit` und `git push` aus.

## <a name="how-app-service-deploys-your-nodejs-app"></a>Bereitstellen der Node.js-App durch App Service
Von Azure App Service wird [iisnode] verwendet, um Node.js-Apps auszuführen. Azure CLI 2.0 Preview und das Kudu-Modul (Git-Bereitstellung) arbeiten zusammen, damit Sie eine optimierte Benutzeroberfläche erhalten, wenn Sie Node.js-Apps über die Befehlszeile entwickeln und bereitstellen. 

* Sie können in Ihrem Stammverzeichnis eine Datei vom Typ „iisnode.yml“ erstellen und damit die iisnode-Eigenschaften anpassen. Alle konfigurierbaren Einstellungen sind [hier](https://github.com/tjanczuk/iisnode/blob/master/src/samples/configuration/iisnode.yml) dokumentiert.
* Unter `git push azure master` automatisiert Kudu die folgenden Bereitstellungsaufgaben:
  
  * Führen Sie `npm install --production` aus, wenn sich „package.json“ im Repositorystamm befindet.
  * Generieren Sie die Datei „Web.config“ für iisnode, in der auf Ihr Startskript in „package.json“ (z.B. „server.js“ oder „app.js“) verwiesen wird.
  * Passen Sie die Datei „Web.config“ an, um die App auf das Debuggen mit Node-Inspector vorzubereiten.

## <a name="use-a-nodejs-framework"></a>Verwenden eines Node.js-Frameworks
Wenn Sie bei der App-Entwicklung ein gängiges Node.js-Framework wie [Sails.js][SAILSJS] oder [MEAN.js][MEANJS] verwenden, können Sie sie für App Service bereitstellen. Die gängigen Node.js-Frameworks verfügen über bestimmte Eigenheiten, und ihre Paketabhängigkeiten werden ständig aktualisiert. In App Service stehen jedoch die Protokolle „stdout“ und „stderr“ zur Verfügung, damit Sie genau nachvollziehen können, was mit Ihrer App passiert, und entsprechende Änderungen vornehmen können. Weitere Informationen finden Sie unter [Abrufen von stdout- und stderr-Protokollen von iisnode](#iisnodelog).

In den folgendes Tutorials wird gezeigt, wie Sie mit einem bestimmten Framework in App Service arbeiten:

* [Bereitstellen einer Sails.js-Web-App in Azure App Service]
* [Erstellen einer Node.js-Chat-Anwendung mit Socket.IO in Azure App Service]
* [Verwenden von io.js mit Azure App Service-Web-Apps]

<a name="version"></a>

## <a name="use-a-specific-nodejs-engine"></a>Verwenden eines bestimmten Node.js-Moduls
In einem typischen Workflow weisen Sie App Service an, ein bestimmtes Node.js-Modul zu verwenden, wie das sonst auch in „package.json“ der Fall ist.
Beispiel:

    "engines": {
        "node": "6.9.1"
    }, 

Das Kudu-Bereitstellungsmodul bestimmt in der folgenden Reihenfolge, welches Node.js-Modul verwendet werden soll:

* Sehen Sie sich zuerst „iisnode.yml“ an, um zu überprüfen, ob `nodeProcessCommandLine` angegeben ist. Wenn ja, sollten Sie dieses Element verwenden.
* Sehen Sie sich als Nächstes „package.json“ an, um zu prüfen, ob `"node": "..."` im `engines`-Objekt angegeben ist. Wenn ja, sollten Sie dieses Element verwenden.
* Wählen Sie standardmäßig eine Node.js-Version aus.

Die aktualisierte Liste mit allen unterstützten Node.js/NPM-Versionen in Azure App Service finden Sie unter der folgenden URL für Ihre App:

    https://<app_name>.scm.azurewebsites.net/api/diagnostics/runtime

> [!NOTE]
> Es empfiehlt sich, das gewünschte Node.js-Modul explizit zu definieren. Die Node.js-Standardversion kann sich ändern, und dies kann wiederum zu Fehlern in Ihrer Azure-Web-App führen, falls die Node.js-Standardversion nicht für Ihre Anwendung geeignet ist.
> 
> 

<a name="iisnodelog"></a>

## <a name="get-stdout-and-stderr-logs-from-iisnode"></a>Abrufen von stdout- und stderr-Protokollen von iisnode
Führen Sie zum Lesen von iisnode-Protokollen die folgenden Schritte aus.

> [!NOTE]
> Es kann sein, dass nach Abschluss dieser Schritte noch keine Protokolldateien vorhanden sind, sondern erst, wenn ein Fehler auftritt.
> 
> 

1. Öffnen Sie die von Azure CLI 2.0 Preview bereitgestellte Datei „iisnode.yml“.
2. Legen Sie die beiden folgenden Parameter fest: 
   
        loggingEnabled: true
        logDirectory: iisnode
   
    Zusammen wird iisnode in App Service damit angewiesen, die Ausgabe von stdout und stderror in das Verzeichnis „D:\home\site\wwwroot\**iisnode**“ einzufügen.
3. Speichern Sie die Änderungen, und übertragen Sie diese mit den folgenden Git-Befehlen per Pushvorgang an Azure:
   
        git add .
        git commit -m "{your commit message}"
        git push azure master
   
    iisnode ist jetzt konfiguriert. In den nächsten Schritten wird veranschaulicht, wie Sie auf diese Protokolle zugreifen.
4. Greifen Sie in Ihrem Browser auf die Kudu-Debugkonsole für Ihre App zu, die sich am folgenden Ort befindet:
   
        https://{appname}.scm.azurewebsites.net/DebugConsole 
   
    Diese URL unterscheidet sich von der Web-App-URL, da hier noch *.scm.* an den DNS-Namen angehängt ist. Sie erhalten einen 404-Fehler, wenn Sie diesen URL-Zusatz weglassen.
5. Navigieren Sie zu „D:\home\site\wwwroot\iisnode“.
   
    ![Navigieren zum Speicherort der Protokolldateien von iisnode][iislog-kudu-console-find]
6. Klicken Sie für das Protokoll, das Sie lesen möchten, auf das Symbol **Bearbeiten** . Sie können auch auf **Herunterladen** oder **Löschen** klicken.
   
    ![Öffnen einer Protokolldatei von iisnode][iislog-kudu-console-open]
   
    Jetzt wird das Protokoll angezeigt, das Ihnen als Hilfe beim Debuggen der App Service-Bereitstellung dient.
   
    ![Untersuchen einer Protokolldatei von iisnode][iislog-kudu-console-read]

## <a name="debug-your-app-with-node-inspector"></a>Debuggen der App mit Node-Inspector
Wenn Sie Node-Inspector zum Debuggen Ihrer Node.js-Apps verwenden, können Sie die Anwendung für Ihre live geschaltete App Service-App verwenden. Node-Inspector ist in der iisnode-Installation für App Service vorinstalliert. Falls Sie die Bereitstellung über Git durchführen, enthält die automatisch generierte Datei „Web.config“ aus Kudu bereits die gesamte Konfiguration, die Sie zum Aktivieren von Node-Inspector benötigen.

Führen Sie diese Schritte aus, um Node-Inspector zu aktivieren:

1. Öffnen Sie die Datei „iisnode.yml“ in Ihrem Repositorystamm, und geben Sie die folgenden Parameter an: 
   
        debuggingEnabled: true
        debuggerExtensionDll: iisnode-inspector.dll
2. Speichern Sie die Änderungen, und übertragen Sie diese mit den folgenden Git-Befehlen per Pushvorgang an Azure:
   
        git add .
        git commit -m "{your commit message}"
        git push azure master
3. Navigieren Sie jetzt einfach zur Startdatei Ihrer App, wie im Startskript in der Datei „package.json“ angegeben, indem Sie der URL „/debug“ hinzufügen. Beispiel:
   
        http://{appname}.azurewebsites.net/server.js/debug
   
    Oder
   
        http://{appname}.azurewebsites.net/app.js/debug

## <a name="more-resources"></a>Weitere Ressourcen
* [Festlegen einer Node.js-Version in einer Azure-Anwendung](../nodejs-specify-node-version-azure-apps.md)
* [Bewährte Methoden und Problembehandlungsschritte für Node.js-Anwendungen in Azure](app-service-web-nodejs-best-practices-and-troubleshoot-guide.md)
* [Debuggen einer Node.js-Web-App in Azure App Service](web-sites-nodejs-debug.md)
* [Verwenden von Node.js-Modulen mit Azure-Anwendungen](../nodejs-use-node-modules-azure-apps.md)
* [Azure App Service-Web-Apps: Node.js](http://blogs.msdn.com/b/silverlining/archive/2012/06/14/windows-azure-websites-node-js.aspx)
* [Node.js Developer Center](/develop/nodejs/)
* [Erste Schritte mit Web-Apps in Azure App Service](app-service-web-get-started.md)
* [Exploring the Super Secret Kudu Debug Console (Erkunden der geheimen Kudu-Debugkonsole)]

<!-- URL List -->

[Azure App Service]: ../app-service/app-service-value-prop-what-is.md
[Ihre Visual Studio-Abonnentenvorteile aktivieren]: http://go.microsoft.com/fwlink/?LinkId=623901
[Bower]: http://bower.io/
[Erstellen einer Node.js-Chat-Anwendung mit Socket.IO in Azure App Service]: ./web-sites-nodejs-chat-app-socketio.md
[Bereitstellen einer Sails.js-Web-App in Azure App Service]: ./app-service-web-nodejs-sails.md
[Exploring the Super Secret Kudu Debug Console (Erkunden der geheimen Kudu-Debugkonsole)]: /documentation/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[Express Generator für Yeoman]: https://github.com/petecoop/generator-express
[Git]: http://www.git-scm.com/downloads
[Verwenden von io.js mit Azure App Service-Web-Apps]: ./web-sites-nodejs-iojs.md
[iisnode]: https://github.com/tjanczuk/iisnode/wiki
[MEANJS]: http://meanjs.org/
[Node.js]: http://nodejs.org
[SAILSJS]: http://sailsjs.org/
[für eine kostenlose Testversion registrieren]: http://go.microsoft.com/fwlink/?LinkId=623901
[web app]: ./app-service-web-overview.md
[Yeoman]: http://yeoman.io/

<!-- IMG List -->

[deployed-express-app]: ./media/app-service-web-nodejs-get-started/deployed-express-app.png
[iislog-kudu-console-find]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-navigate.png
[iislog-kudu-console-open]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-open.png
[iislog-kudu-console-read]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-read.png



<!--HONumber=Jan17_HO3-->


