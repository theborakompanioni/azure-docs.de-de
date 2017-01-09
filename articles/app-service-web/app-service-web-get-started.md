---
title: "Bereitstellen Ihrer ersten Web-App für Azure in fünf Minuten | Microsoft Docs"
description: "Erfahren Sie, wie einfach die Ausführung von Web-Apps in App Service ist, indem Sie eine Beispiel-App bereitstellen. Sie können in kürzester Zeit mit der Entwicklung beginnen und sofort Ergebnisse erzielen."
services: app-service\web
documentationcenter: 
author: cephalin
manager: wpickett
editor: 
ms.assetid: 65c9bdd9-8763-4c56-8e15-f790992e951e
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 01/04/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: 4e86c1c1460f7b6eb312f10a0666f92b33697763
ms.openlocfilehash: f6356a5a647940796c337e345a8b901dae9eb9b4


---
# <a name="deploy-your-first-web-app-to-azure-in-five-minutes"></a>Bereitstellen Ihrer ersten Web-App für Azure in fünf Minuten
In diesem Tutorial erfahren Sie, wie Sie Ihre erste Web-App für [Azure App Service](../app-service/app-service-value-prop-what-is.md)bereitstellen.
Mit App Service können Sie Web-Apps, [Mobile App-Back-Ends](/documentation/learning-paths/appservice-mobileapps/) und [API-Apps](../app-service-api/app-service-api-apps-why-best-platform.md) erstellen.

In diesem Tutorial führen Sie folgende Schritte aus: 

* Erstellen einer Web-App in Azure App Service
* Bereitstellen von Beispielcode (Auswahl: ASP.NET, PHP, Node.js, Java oder Python)
* Anzeigen des live in der Produktion ausgeführten Codes
* Aktualisieren Ihrer Web-App wie beim [Ausführen von Git-Commits mittels Push](https://git-scm.com/docs/git-push)

[!INCLUDE [app-service-linux](../../includes/app-service-linux.md)]

## <a name="cli-versions-to-complete-the-task"></a>CLI-Versionen zum Durchführen dieser Aufgabe

Führen Sie die Aufgabe mit einer der folgenden CLI-Versionen durch:

- [Azure CLI 1.0:](app-service-web-get-started-cli-nodejs.md) Unsere CLI für das klassische Bereitstellungsmodell und das Resource Manager-Bereitstellungsmodell
- [Azure CLI 2.0 (Vorschau):](app-service-web-get-started.md) Unsere CLI der nächsten Generation für das Resource Manager-Bereitstellungsmodell

## <a name="prerequisites"></a>Voraussetzungen
* [Git](http://www.git-scm.com/downloads).
* [Azure CLI 2.0 Preview](/cli/azure/install-az-cli2).
* Ein Microsoft Azure-Konto. Falls Sie noch kein Konto haben, können Sie sich [für eine kostenlose Testversion registrieren](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) oder [Ihre Visual Studio-Abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

> [!NOTE]
> Zum [Testen von App Service](http://go.microsoft.com/fwlink/?LinkId=523751) benötigen Sie kein Azure-Konto. Sie können eine Starter-App erstellen und bis zu einer Stunde damit experimentieren – ohne Kreditkarte und ohne jegliche Verpflichtungen.
> 
> 

## <a name="deploy-a-web-app"></a>Bereitstellen einer Web-App
Wir stellen jetzt eine Web-App unter Azure App Service bereit.

1. Öffnen Sie eine neue Windows-Eingabeaufforderung, ein PowerShell-Fenster, eine Linux-Shell oder ein OS X-Terminal. Vergewissern Sie sich durch Ausführen von `git --version` und `azure --version`, dass Git und die Azure-Befehlszeilenschnittstelle auf Ihrem Computer installiert sind.
   
    ![Testen der Installation der CLI-Tools für Ihre erste Web-App in Azure](./media/app-service-web-get-started/1-test-tools-2.0.png)
   
    Entsprechende Downloadlinks finden Sie bei Bedarf unter [Voraussetzungen](#Prerequisites) .

2. Melden Sie sich wie folgt bei Azure an:
   
        az login
   
    Folgen Sie der Hilfemeldung, um den Anmeldeprozess fortzusetzen.
   
    ![Anmelden bei Azure zur Erstellung Ihrer ersten Web-App](./media/app-service-web-get-started/3-azure-login-2.0.png)

3. Legen Sie den Bereitstellungsbenutzer für App Service fest. Später stellen Sie Code mit diesen Anmeldeinformationen bereit.
   
        az appservice web deployment user set --user-name <username> --password <password>

3. Erstellen Sie eine neue [Ressourcengruppe](../azure-resource-manager/resource-group-overview.md). In diesem ersten App Service-Tutorial müssen Sie nicht unbedingt wissen, worum es sich dabei genau handelt.

        az group create --location "<location>" --name my-first-app-group

    Welche Werte Sie für `<location>` verwenden können, erfahren Sie mithilfe des CLI-Befehls `az appservice list-locations`.

3. Erstellen Sie einen neuen [App Service-Plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) vom Typ „FREE“. In diesem ersten App Service-Tutorial reicht es, wenn Sie wissen, dass Ihnen für Web-Apps im Rahmen dieses Plans keine Kosten entstehen.

        az appservice plan create --name my-free-appservice-plan --resource-group my-first-app-group --sku FREE

4. Erstellen Sie in `<app_name>` eine neue Web-App mit einem eindeutigen Namen.

        az appservice web create --name <app_name> --resource-group my-first-app-group --plan my-free-appservice-plan

4. Besorgen Sie sich als Nächstes den Beispielcode, den Sie bereitstellen möchten. Wechseln Sie in ein Arbeitsverzeichnis (`CD`), und klonen Sie die Beispiel-App wie folgt:
   
        cd <working_directory>
        git clone <github_sample_url>
   
    Verwenden Sie für *&lt;github_sample_url>* je nach gewünschtem Framework eine der folgenden URLs:
   
   * HTML, CSS, JS: [https://github.com/Azure-Samples/app-service-web-html-get-started.git](https://github.com/Azure-Samples/app-service-web-html-get-started.git)
   * ASP.NET: [https://github.com/Azure-Samples/app-service-web-dotnet-get-started.git](https://github.com/Azure-Samples/app-service-web-dotnet-get-started.git)
   * PHP (CodeIgniter): [https://github.com/Azure-Samples/app-service-web-php-get-started.git](https://github.com/Azure-Samples/app-service-web-php-get-started.git)
   * Node.js (Express): [https://github.com/Azure-Samples/app-service-web-nodejs-get-started.git](https://github.com/Azure-Samples/app-service-web-nodejs-get-started.git)
   * Java: [https://github.com/Azure-Samples/app-service-web-java-get-started.git](https://github.com/Azure-Samples/app-service-web-java-get-started.git)
   * Python (Django): [https://github.com/Azure-Samples/app-service-web-python-get-started.git](https://github.com/Azure-Samples/app-service-web-python-get-started.git)

    ![Klonen des App-Beispielcodes für Ihre erste Web-App in Azure](./media/app-service-web-get-started/2-clone-sample.png)
   
5. Wechseln Sie in das Repository Ihrer Beispiel-App. Beispiel:
   
        cd app-service-web-html-get-started

5. Konfigurieren Sie mit dem folgenden Befehl die lokale Git-Bereitstellung für Ihre App Service-Web-App:

        az appservice web source-control config-local-git --name <app_name> --resource-group my-first-app-group

    Sie erhalten eine JSON-Ausgabe wie die folgende, was bedeutet, dass das Git-Remoterepository konfiguriert ist:

        {
        "url": "https://<deployment_user>@<app_name>.scm.azurewebsites.net/<app_name>.git"
        }

6. Fügen Sie die URL im JSON-Code als Git-Remotespeicherort für Ihr lokales Repository (der Einfachheit halber `azure` genannt) hinzu.

        git remote add azure https://<deployment_user>@<app_name>.scm.azurewebsites.net/<app_name>.git
   
7. Stellen Sie Ihren Beispielcode für den Git-Remotespeicherort `azure` bereit. Geben Sie die zuvor konfigurierten Anmeldeinformationen für die Bereitstellung an, wenn Sie dazu aufgefordert werden.
   
        git push azure master
   
    ![Übertragen von Code an Ihre ersten Web-App in Azure mittels Push](./media/app-service-web-get-started/5-push-code.png)
   
    Bei Verwendung eines der Sprachen-Frameworks wird eine andere Ausgabe angezeigt. Mit `git push` wird nicht nur Code in Azure eingefügt, sondern es werden auch Bereitstellungsaufgaben im Bereitstellungsmodul ausgelöst. Falls Ihr Projektstamm (bzw. Repositorystamm) die Datei „package.json“ (Node.js) oder „requirements.txt“ (Python) enthält oder falls Ihr ASP.NET-Projekt die Datei „packages.config“ enthält, stellt das Bereitstellungsskript die erforderlichen Pakete wieder für Sie her. Sie können auch die [Composer-Erweiterung aktivieren](web-sites-php-mysql-deploy-use-git.md#composer) , um Dateien vom Typ „composer.json“ in Ihrer PHP-App automatisch zu verarbeiten.

Herzlichen Glückwunsch! Sie haben Ihre App in Azure App Service bereitgestellt.

## <a name="see-your-app-running-live"></a>Verfolgen der Liveausführung der App

Führen Sie diesen Befehl aus, um die Liveausführung der App in Azure zu verfolgen:

    az appservice web browse --name <app_name> --resource-group my-first-app-group

## <a name="make-updates-to-your-app"></a>Durchführen von Updates für die App

Sie können jetzt Git verwenden, um aus Ihrem Projektstamm (Repositorystamm) jederzeit einen Pushvorgang durchzuführen und so ein Update für die Live-Website vorzunehmen. Dazu gehen Sie wie bei der erstmaligen Bereitstellung Ihres Codes vor. Wenn Sie beispielsweise eine neue Änderung übertragen möchten, die Sie lokal getestet haben, führen Sie einfach die folgenden Befehle in Ihrem Projektstamm (Repositorystamm) aus:

    git add .
    git commit -m "<your_message>"
    git push azure master

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie den Artikel zu den bevorzugten Schritten für die Entwicklung und Bereitstellung für Ihr Sprachen-Framework:

* [.NET](web-sites-dotnet-get-started.md)
* [PHP](app-service-web-php-get-started.md)
* [Node.js](app-service-web-nodejs-get-started.md)
* [Python](web-sites-python-ptvs-django-mysql.md)
* [Java](web-sites-java-get-started.md)

Sie können auch weiter mit Ihrer ersten Web-App arbeiten. Beispiel:

* Testen Sie [weitere Methoden zum Bereitstellen Ihres Codes in Azure](web-sites-deploy.md). Wenn Sie eines Ihrer GitHub-Repositorys zur Bereitstellung verwenden möchten, wählen Sie beispielsweise unter **Bereitstellungsoptionen** einfach **GitHub** anstelle von **Lokales Git-Repository** aus.
* Entwickeln Sie Ihre Azure-App weiter. Authentifizieren Sie Ihre Benutzer. Skalieren Sie die App je nach Bedarf. Richten Sie einige Leistungswarnungen ein. Es sind jeweils nur wenige Klicks erforderlich. Weitere Informationen finden Sie unter [Hinzufügen von Funktionen zu Ihrer ersten Web-App](app-service-web-get-started-2.md).




<!--HONumber=Dec16_HO3-->


