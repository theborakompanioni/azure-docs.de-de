---
title: Agile Softwareentwicklung mit Azure App Service
description: "Erfahren Sie, wie Sie umfangreiche komplexe Anwendungen mit Azure App Service auf eine Weise erstellen, die die agile Softwareentwicklung unterstützt."
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: c0fdb676-36a6-4738-925f-65b4835d187f
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/01/2016
ms.author: cephalin
ms.translationtype: HT
ms.sourcegitcommit: b309108b4edaf5d1b198393aa44f55fc6aca231e
ms.openlocfilehash: 5ed888cbb422766cf2094f5980dfd1c599bd431c
ms.contentlocale: de-de
ms.lasthandoff: 08/15/2017

---
# <a name="agile-software-development-with-azure-app-service"></a>Agile Softwareentwicklung mit Azure App Service
In diesem Tutorial erfahren Sie, wie Sie umfangreiche komplexe Anwendungen mit [Azure App Service](/azure/app-service/) auf eine Weise erstellen, die die [agile Softwareentwicklung](https://en.wikipedia.org/wiki/Agile_software_development) unterstützt. Es wird davon ausgegangen, dass Sie bereits wissen, wie Sie [komplexe Anwendungen in Azure vorhersehbar bereitstellen](app-service-deploy-complex-application-predictably.md).

Einschränkungen bei den technischen Prozessen stehen einer erfolgreichen Umsetzung agiler Methoden mitunter im Weg. Azure App Service kann mit Features wie [kontinuierliche Veröffentlichung](app-service-continuous-deployment.md), [Stagingumgebungen](web-sites-staged-publishing.md) (Slots) und [Überwachung](web-sites-monitor.md) und bei überlegter Kombination mit der Orchestrierung und Verwaltung der Bereitstellung in [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) Teil einer überzeugenden Lösung für Entwickler sein, die die agile Softwareentwicklung bevorzugen.

Die folgende Tabelle enthält eine kurze Liste mit Anforderungen an die agile Entwicklung und gibt Aufschluss darüber, wie Azure-Dienste zu deren Erfüllung beitragen.

| Voraussetzung | Beitrag von Azure zur Erfüllung |
| --- | --- |
| – Build mit jedem Commit<br>– Automatischer und schneller Build |Bei Konfiguration mit fortlaufender Bereitstellung kann Azure App Service basierend auf einer Entwicklungsverzweigung als aktiv ausgeführte Builds fungieren. Jedes Mal, wenn Code per Push in die Verzweigung übertragen wird, erfolgt automatisch ein Build und die aktive Ausführung in Azure. |
| – Builds mit Selbsttests gestalten |Auslastungstests, Webtests usw. können mit der Azure-Ressourcen-Manager-Vorlage bereitgestellt werden. |
| – Tests in einem Klon der Produktionsumgebung durchführen |Azure Ressourcen-Manager-Vorlagen können verwendet werden, um Klone der Azure-Produktionsumgebung (einschließlich App-Einstellungen, Vorlagen mit Verbindungszeichenfolgen, Skalierung usw.) für schnelle und vorhersagbare Tests zu erstellen. |
| – Ergebnis des neuesten Builds problemlos anzeigen |Die kontinuierliche Bereitstellung in Azure aus einem Repository bedeutet, dass Sie unmittelbar nach dem Commit Ihrer Änderungen neuen Code in einer aktiven Anwendung testen können. |
| – Täglicher Commit in der MAIN-Verzweigung<br>– Bereitstellung automatisieren |Durch die fortlaufende Integration einer Produktionsanwendung mit der MAIN-Verzweigung eines Repositorys wird jeder Commit-/Zusammenführungsvorgang automatisch in der MAIN-Verzweigung in der Produktion bereitgestellt. |

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="what-you-will-do"></a>Aufgaben
Sie durchlaufen einen typischen Produktionsworkflow aus Entwicklung/Test/Staging, um neue Änderungen an der Beispielanwendung [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) zu veröffentlichen, die aus zwei [Web-Apps](/services/app-service/web/), von denen eine ein Front-End (FE) und die andere ein Web-API-Back-End (BE) ist, und einer [SQL-Datenbank](/services/sql-database/) besteht. Dabei wird die folgende Bereitstellungsarchitektur verwendet:

![](./media/app-service-agile-software-development/what-1-architecture.png)

Erläuterung der Abbildung:

* Die Bereitstellungsarchitektur ist in drei verschiedene Umgebungen (bzw. [Ressourcengruppen](../azure-resource-manager/resource-group-overview.md) in Azure) aufgeteilt, von denen jede einen eigenen [App Service-Plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md), eigene [Skalierungseinstellungen](web-sites-scale.md) und eine eigene SQL-Datenbank hat. 
* Jede Umgebung kann separat verwaltet werden. Sie können sogar in verschiedenen Abonnements vorhanden sind.
* Die Staging- und Produktionsumgebung werden als zwei Slots derselben App Service-App implementiert. Die Hauptverzweigung ist für die Continuous Integration mit dem Stagingslot eingerichtet.
* Wenn ein Commit für die Hauptverzweigung im Stagingslot (mit Produktionsdaten) bestätigt wird, wird die überprüfte Staging-App im Produktionsslot [ohne Ausfallzeiten](web-sites-staged-publishing.md)ausgetauscht.

Die Produktions- und Stagingumgebung ist durch die Vorlage unter [*&lt;Repositorystammverzeichnis>*/ARMTemplates/ProdandStage.json](https://github.com/azure-appservice-samples/ToDoApp/blob/master/ARMTemplates/ProdAndStage.json) definiert.

Die Entwicklungs- und Testumgebungen sind durch die Vorlage unter [*&lt;Repositorystammverzeichnis>*/ARMTemplates/Dev.json](https://github.com/azure-appservice-samples/ToDoApp/blob/master/ARMTemplates/Dev.json) definiert.

Sie verwenden auch die übliche Verzweigungsstrategie, bei der Code aus der Entwicklungsverzweigung in die Testverzweigung und dann in die Hauptverzweigung verschoben wird (wobei jedes Mal die Qualität verbessert wird).

![](./media/app-service-agile-software-development/what-2-branches.png) 

## <a name="what-you-need"></a>Erforderliches Element
* Ein Azure-Konto
* Ein [GitHub](https://github.com/) -Konto
* Git-Shell (wird zusammen mit [GitHub für Windows](https://windows.github.com/) installiert) zum Ausführen von Git- und PowerShell-Befehlen in der gleichen Sitzung 
* Neueste [Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps) -Version
* Grundlegende Kenntnisse der folgenden Tools:
  * Bereitstellung der [Azure Resource Manager-Vorlage](../azure-resource-manager/resource-group-overview.md) (siehe auch [Vorhersagbares Bereitstellen einer komplexen Anwendung in Azure](app-service-deploy-complex-application-predictably.md))
  * [Git](http://git-scm.com/documentation)
  * [PowerShell](https://technet.microsoft.com/library/bb978526.aspx)

> [!NOTE]
> Sie benötigen ein Microsoft Azure-Konto, um dieses Tutorial durchführen zu können:
> 
> * Sie können ein [Azure-Konto kostenlos erstellen](https://azure.microsoft.com/pricing/free-trial/) : Sie erhalten ein Guthaben, das Sie zum Ausprobieren der zahlungspflichtigen Azure-Dienste nutzen können, und Sie können das Konto selbst dann behalten und kostenlose Azure-Dienste wie etwa Web-Apps nutzen, wenn das Guthaben aufgebraucht ist.
> * Sie können Ihre [Vorteile für Visual Studio-Abonnenten aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) : Ihr Visual Studio-Abonnement schließt ein monatliches Guthaben ein, das Sie für kostenpflichtige Azure-Dienste nutzen können.
> 
> Wenn Sie Azure App Service ausprobieren möchten, ehe Sie sich für ein Azure-Konto anmelden, können Sie unter [App Service testen](https://azure.microsoft.com/try/app-service/)sofort kostenlos eine kurzlebige Starter-Web-App in App Service erstellen. Keine Kreditkarte erforderlich, keine Verpflichtungen.
> 
> 

## <a name="set-up-your-production-environment"></a>Einrichten der Produktionsumgebung
> [!NOTE]
> Das Skript in diesem Tutorial konfiguriert automatisch die kontinuierliche Veröffentlichung aus Ihrem GitHub-Repository. Dazu müssen Ihre Anmeldeinformationen für GitHub bereits in Azure gespeichert sein. Andernfalls ist die skriptgesteuerte Bereitstellung nicht erfolgreich, wenn versucht wird, Quellcodeverwaltungseinstellungen für die Web-Apps zu konfigurieren. 
> 
> Um Ihre GitHub-Anmeldeinformationen in Azure zu speichern, erstellen Sie eine Web-App im [Azure-Portal](https://portal.azure.com/) und [konfigurieren die GitHub-Bereitstellung](app-service-continuous-deployment.md). Dies ist nur einmalig erforderlich. 
> 
> 

In einem typischen DevOps-Szenario verfügen Sie über eine Anwendung, die aktiv in Azure ausgeführt wird und an der Sie über die fortlaufende Veröffentlichung Änderungen vornehmen möchten. In diesem Szenario haben Sie eine Vorlage, die Sie entwickelt, getestet und zum Bereitstellen der Produktionsumgebung verwendet haben. Diese Vorlage werden Sie in diesem Abschnitt einrichten.

1. Erstellen Sie Ihre eigene Verzweigung des [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) -Repositorys. Informationen zum Erstellen Ihrer Verzweigung finden Sie unter [Verzweigen eines Repositorys](https://help.github.com/articles/fork-a-repo/). Sobald die Verzweigung erstellt wurde, können Sie sie in Ihrem Browser einsehen.
   
    ![](./media/app-service-agile-software-development/production-1-private-repo.png)
2. Öffnen Sie eine Git-Shell-Sitzung. Wenn Sie noch nicht über Git-Shell verfügen, installieren Sie nun [GitHub für Windows](https://windows.github.com/) .
3. Erstellen Sie einen lokalen Klon Ihrer Verzweigung, indem Sie den folgenden Befehl ausführen:

        git clone https://github.com/<your_fork>/ToDoApp.git 
4. Sobald Sie Ihre lokalen Klon haben, navigieren Sie zu *&lt;Repositorystammverzeichnis>*\ARMTemplates, und führen Sie das Skript „deploy.ps1“ wie folgt aus:
   
        .\deploy.ps1 –RepoUrl https://github.com/<your_fork>/todoapp.git
5. Geben Sie bei Aufforderung den gewünschten Benutzernamen und das Kennwort für den Datenbankzugriff ein.
   
   Der Fortschritt der Bereitstellung verschiedener Azure-Ressourcen sollte angezeigt werden. Nach Abschluss der Bereitstellung startet das Skript die Anwendung im Browser, und Sie hören einen Signalton.
   
    ![](./media/app-service-agile-software-development/production-2-app-in-browser.png)
   
   > [!TIP]
   > Wenn Sie sich *&lt;Repositorystammverzeichnis>*\ARMTemplates\Deploy.ps1 ansehen, erkennen Sie, wie das Skript Ressourcen mit eindeutigen IDs erstellt. Sie können den gleichen Ansatz zum Erstellen von Klonen der gleichen Bereitstellung befolgen, ohne sich Gedanken über in Konflikt stehende Ressourcennamen machen zu müssen.
   > 
   > 
6. Zurück in der Git-Shell-Sitzung führen Sie Folgendes aus:
   
        .\swap –Name ToDoApp<unique_string>master
   
    ![](./media/app-service-agile-software-development/production-4-swap.png)
7. Wenn das Skript abgeschlossen ist, navigieren Sie zurück zur Adresse des Front-Ends (http://ToDoApp*&lt;unique_string>*master.azurewebsites.net/), um zu prüfen, ob die Anwendung in der Produktionsumgebung ausgeführt wird.
8. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an, und sehen Sie sich an, was erstellt wurde.
   
   Sie sollten zwei Web-Apps in derselben Ressourcengruppe sehen, von denen eine das Suffix `Api` im Namen enthält. Wenn Sie die Ressourcengruppenansicht betrachten, sehen Sie auch SQL-Datenbank und -Server, den App Service-Plan und die Stagingslots für die Web-Apps. Navigieren Sie durch die verschiedenen Ressourcen, und vergleichen Sie diese mit *&lt;Repositorystammverzeichnis>*\ARMTemplates\ProdAndStage.json, um zu prüfen, wie sie in der Vorlage konfiguriert sind.
   
    ![](./media/app-service-agile-software-development/production-3-resource-group-view.png)

Sie haben nun die Produktionsumgebung eingerichtet. Als Nächstes leiten Sie ein neues Update für die Anwendung ein.

## <a name="create-dev-and-test-branches"></a>Erstellen von Entwicklungs- und Testverzweigungen
Nun da Sie über eine komplexe Anwendung verfügen, die in Azure in der Produktionsumgebung ausgeführt wird, nehmen Sie in Übereinstimmung mit der agilen Methodik ein Update an Ihrer Anwendung vor. In diesem Abschnitt erstellen Sie die Entwicklungs- und Testverzweigungen, die Sie benötigen, um die erforderlichen Aktualisierungen vorzunehmen.

1. Erstellen Sie zunächst die Testumgebung. Führen Sie in der Git-Shell-Sitzung die folgenden Befehle zum Erstellen der Umgebung für eine neue Verzweigung namens **NewUpdate**aus. 
   
        git checkout -b NewUpdate
        git push origin NewUpdate 
        .\deploy.ps1 -TemplateFile .\Dev.json -RepoUrl https://github.com/<your_fork>/ToDoApp.git -Branch NewUpdate
2. Geben Sie bei Aufforderung den gewünschten Benutzernamen und das Kennwort für den Datenbankzugriff ein. 
   
   Nach Abschluss der Bereitstellung startet das Skript die Anwendung im Browser, und Sie hören einen Signalton. Sie verfügen nun über eine neue Verzweigung mit eigener Testumgebung. Nehmen Sie sich einen Moment Zeit, um ein paar Aspekte dieser Testumgebung zu überprüfen:
   
   * Sie können sie in einem beliebigen Azure-Abonnement erstellen. Das bedeutet, dass die Produktionsumgebung von Ihrer Testumgebung getrennt verwaltet werden kann.
   * Ihre Testumgebung wird aktiv in Azure ausgeführt.
   * Die Testumgebung ist identisch mit der Produktionsumgebung mit Ausnahme der Stagingslots und Skalierungseinstellungen. Das ist bekannt, da es sich hierbei um die einzigen Unterschiede zwischen „ProdandStage.json“ und „Dev.json“ handelt.
   * Sie können Ihre Testumgebung in einem eigenen App Service-Plan mit einem anderen Tarif (z. B. **Free**) verwalten.
   * Das Löschen dieser Testumgebung ist so einfach wie das Löschen der Ressourcengruppe. Die dazu erforderlichen Schritte lernen Sie [später](#delete)kennen.
3. Fahren Sie mit dem Erstellen einer Entwicklungsverzweigung fort, indem Sie die folgenden Befehle ausführen:
   
        git checkout -b Dev
        git push origin Dev
        .\deploy.ps1 -TemplateFile .\Dev.json -RepoUrl https://github.com/<your_fork>/ToDoApp.git -Branch Dev
4. Geben Sie bei Aufforderung den gewünschten Benutzernamen und das Kennwort für den Datenbankzugriff ein. 
   
   Nehmen Sie sich einen Moment Zeit, um ein paar Aspekte dieser Entwicklungsumgebung zu überprüfen: 
   
   * Ihre Entwicklungsumgebung hat eine mit der Testumgebung identische Konfiguration, da sie mit derselben Vorlage bereitgestellt wird.
   * Jede Entwicklungsumgebung kann im eigenen Azure-Abonnement des Entwicklers erstellt werden, sodass die Testumgebung separat verwaltet werden kann.
   * Ihre Entwicklungsumgebung wird aktiv in Azure ausgeführt.
   * Das Löschen der Entwicklungsumgebung ist so einfach wie das Löschen der Ressourcengruppe. Die dazu erforderlichen Schritte lernen Sie [später](#delete)kennen.

> [!NOTE]
> Wenn mehrere Entwickler am neuen Update arbeiten, kann jeder über die folgenden Schritte problemlos eine Verzweigung und dedizierte Entwicklungsumgebung erstellen:
> 
> 1. Erstellen einer eigenen Verzweigung des Repositorys in GitHub (siehe [Verzweigen eines Repositorys](https://help.github.com/articles/fork-a-repo/))
> 2. Klonen der Verzweigung auf ihrem lokalen Computer
> 3. Ausführen derselben Befehle zum Erstellen ihrer eigenen Entwicklungsverzweigung und -umgebung
> 
> 

Wenn Sie fertig sind, sollte Ihre GitHub-Verzweigung drei Verzweigungen aufweisen:

![](./media/app-service-agile-software-development/test-1-github-view.png)

Und Sie sollten über sechs Web-Apps (drei Gruppen mit je zwei) in drei separaten Ressourcengruppen verfügen:

![](./media/app-service-agile-software-development/test-2-all-webapps.png)

> [!NOTE]
> „ProdandStage.json“ gibt an, dass die Produktionsumgebung den Tarif **Standard** verwenden soll, der für die Skalierbarkeit der Produktionsumgebung geeignet ist.
> 
> 

## <a name="build-and-test-every-commit"></a>Erstellen von Builds und Testen aller Commits
Die Vorlagendateien "ProdAndStage.json" und "Dev.json" geben bereits die Quellverwaltungsparameter an, die standardmäßig die fortlaufende Veröffentlichung für die Web-App einrichten. Daher löst jeder Commit in der GitHub-Verzweigung eine automatische Bereitstellung in Azure aus dieser Verzweigung aus. Lassen Sie uns prüfen, wie Ihre Einrichtung jetzt funktioniert.

1. Stellen Sie sicher, dass Sie sich in der Entwicklungsverzweigung des lokalen Repositorys befinden. Zu diesem Zweck führen Sie den folgenden Befehl auf der Git-Shell aus:
   
        git checkout Dev
2. Nehmen Sie eine Änderung an der Benutzeroberflächenebene der App vor, indem Sie den Code so ändern, dass [Bootstrap](http://getbootstrap.com/components/)-Listen verwendet werden. Öffnen Sie *&lt;Repositorystammverzeichnis>*\src\MultiChannelToDo.Web\app\index.cshtml, und nehmen Sie die folgende hervorgehobene Änderung vor:
   
    ![](./media/app-service-agile-software-development/commit-1-changes.png)
   
    > [!NOTE]
    > Falls Sie das obige Bild nicht lesen können: 
    > 
    > * Ändern Sie in Zeile 18 `check-list` in `list-group`.
    > * Ändern Sie in Zeile 19 `class="check-list-item"` in `class="list-group-item"`.
    > 
    > 
3. Speichern Sie die Änderungen. Zurück auf der Git-Shell führen Sie die folgenden Befehle aus:
   
        cd <repository_root>
        git add .
        git commit -m "changed to bootstrap style"
        git push origin Dev
   
   Diese Git-Befehle sind ähnlich wie zum "Einchecken Ihres Code" in einem anderen Quellcodeverwaltungssystem wie TFS. Beim Ausführen von `git push` löst der neue Commit eine automatische Pushübertragung des Codes in Azure aus. Anschließend wird die Anwendung neu erstellt, um die Änderung an der Entwicklungsumgebung wiederzugeben.
4. Vergewissern Sie sich, dass der Code mittels Push an Ihre Entwicklungsumgebung übertragen wurde. Wechseln Sie hierzu zum Web-App-Blatt Ihrer Entwicklungsumgebung, und sehen Sie sich den Abschnitt **Bereitstellung** an. Ihre letzte Commitnachricht sollte angezeigt werden.
   
    ![](./media/app-service-agile-software-development/commit-2-deployed.png)
5. Klicken Sie hier auf **Durchsuchen** , um die neue Änderung in der aktiven Anwendung in Azure anzuzeigen.
   
    ![](./media/app-service-agile-software-development/commit-3-webapp-in-browser.png)
   
   Dies ist eine geringfügige Anwendungsänderung. Allerdings haben neue Änderungen an einer komplexen Webanwendung häufig unbeabsichtigte und unerwünschte Nebeneffekte. Die Fähigkeit zum einfachen Testen jedes Commits in aktiven Builds ermöglicht Ihnen das Abfangen dieser Probleme, bevor Ihre Kunden davon betroffen sind.

Nun können Sie sich über die Tatsache freuen, dass Sie als Entwickler am Projekt **NewUpdate** für sich eine Entwicklungsumgebung einrichten, für jeden Commit einen Build erstellen und jeden Build testen können.

## <a name="merge-code-into-test-environment"></a>Zusammenführen von Code in der Testumgebung
Wenn Sie bereit sind, Code aus der Verzweigung "Dev" per Push in die Verzweigung "NewUpdate" zu übertragen, befolgen Sie den standardmäßigen Git-Prozess:

1. Führen Sie neue Commits für "NewUpdate" in der Verzweigung "Dev" in GitHub zusammen, wie z. B. Commits, die von anderen Entwicklern erstellt wurden. Bei jedem neuen Commit auf GitHub wird eine Pushübertragung von Code ausgelöst und ein Build in der Entwicklungsumgebung erstellt. Sie können anschließend sicherstellen, dass Ihr Code in der Verzweigung "Dev" mit den neuesten Bits aus der Verzweigung "NewUpdate" weiterhin funktioniert.
2. Führen Sie alle neuen Commits aus der Verzweigung "Dev" in der Verzweigung "NewUpdate" auf GitHub zusammen. Diese Aktion löst eine Pushübertragung von Code und das Erstellen eines Builds in der Testumgebung aus. 

Da Continuous Deployment bereits für diese Git-Verzweigungen eingerichtet ist, sind keine weiteren Aktionen wie etwa das Ausführen von Integrationsbuilds erforderlich. Sie müssen lediglich die standardmäßigen Quellcodeverwaltungsaufgaben mithilfe von Git ausführen; Azure übernimmt sämtliche Buildprozesse für Sie.

Lassen Sie uns nun Ihren Code per Push in die Verzweigung **NewUpdate** übertragen. Führen Sie die folgenden Befehle auf der Git-Shell aus:

    git checkout NewUpdate
    git pull origin NewUpdate
    git merge Dev
    git push origin NewUpdate

Fertig! 

Wechseln Sie zum Web-App-Blatt für Ihre Testumgebung, um zu prüfen, ob Ihr neuer (mit der Verzweigung „NewUpdate“ zusammengeführter) Commit per Push in die Testumgebung übertragen wurde. Klicken Sie dann auf **Durchsuchen** , um zu überprüfen, ob die Formatänderung jetzt aktiv in Azure ausgeführt wird.

## <a name="deploy-update-to-production"></a>Bereitstellen des Updates in der Produktionsumgebung
Die Pushübertragung von Code aus der Staging- in die Produktionsumgebung sollte sich nicht von der Übertragung von Code in die Testumgebung unterscheiden. Es ist wirklich so einfach. 

Führen Sie die folgenden Befehle auf der Git-Shell aus:

    git checkout master
    git pull origin master
    git merge NewUpdate
    git push origin master

Denken Sie daran, dass basierend auf der Einrichtung der Produktionsumgebung in „ProdandStage.json“ Ihr neuer Code per Push in den **Stagingslot** übertragen und dort ausgeführt wird. Wenn Sie also zur URL des Stagingslots navigieren, wird der neue Code dort ausgeführt. Führen Sie hierzu in Git Shell das folgende Cmdlet aus:

    Start-Process -FilePath "http://ToDoApp<unique_string>master-Staging.azurewebsites.net"

Nachdem Sie zuvor das Update im Stagingslot überprüft haben, ist der letzte verbleibende Schritt das Überführen in die Produktionsumgebung. Führen Sie dazu die folgenden Befehle auf der Git-Shell aus:

    cd <repository_root>\ARMTemplates
    .\swap.ps1 -Name ToDoApp<unique_string>master

Glückwunsch! Sie haben erfolgreich ein neues Update für Ihre Webanwendung in der Produktion veröffentlicht. Dazu haben Sie Entwicklungs- und Testumgebungen erstellt, Builds erstellt und jeden Commit getestet. Dies sind die entscheidenden Bausteine für die agile Softwareentwicklung.

<a name="delete"></a>

## <a name="delete-dev-and-test-environments"></a>Löschen von Entwicklungs- und Testumgebungen
Da Sie Ihre Entwicklungs- und Testumgebung als eigenständige Ressourcengruppen entworfen haben, lassen sie sich ganz einfach wieder löschen. Um diejenigen zu löschen, die Sie in diesem Tutorial erstellt haben (sowohl die GitHub-Verzweigungen als auch die Azure-Artefakte), führen Sie einfach die folgenden Befehle auf der Git-Shell aus:

    git branch -d Dev
    git push origin :Dev
    git branch -d NewUpdate
    git push origin :NewUpdate
    Remove-AzureRmResourceGroup -Name ToDoApp<unique_string>dev-group -Force -Verbose
    Remove-AzureRmResourceGroup -Name ToDoApp<unique_string>newupdate-group -Force -Verbose

## <a name="summary"></a>Zusammenfassung
Eine agile Softwareentwicklung ist für viele Unternehmen unverzichtbar, die Azure als Anwendungsplattform übernehmen möchten. In diesem Tutorial haben Sie erfahren, wie Sie exakte bzw. nahezu exakte Replikate der Produktionsumgebung selbst für komplexe Anwendungen mühelos erstellen. Sie haben auch gelernt, wie Sie diese Fähigkeit nutzen können, um einen Entwicklungsprozess einzurichten, in dem Sie Builds erstellen und jeden einzelnen Commit in Azure testen können. Dieses Tutorial konnte Ihnen hoffentlich zeigen, wie Sie Azure App Service und den Azure-Ressourcen-Manager gemeinsam zum Erstellen einer DevOps-Lösung optimal einsetzen, die agile Methoden befolgt. Als Nächstes können Sie dieses Szenario durch Ausführen erweiterter DevOps-Techniken wie z. B. [Test-in-Produktion](app-service-web-test-in-production-get-start.md) ergänzen. Ein häufiges Szenario für Tests in der Produktion finden Sie unter [Flighting-Bereitstellung (Betatestversion) in Azure App Service](app-service-web-test-in-production-controlled-test-flight.md).

## <a name="more-resources"></a>Weitere Ressourcen
* [Vorhersagbares Bereitstellen einer komplexen Anwendung in Azure](app-service-deploy-complex-application-predictably.md)
* [Agile Entwicklung in der Praxis: Tipps und Tricks für die Modernisierung des Entwicklungszyklus](http://channel9.msdn.com/Events/Ignite/2015/BRK3707)
* [Erweiterte Bereitstellungsstrategien für Azure-Web-Apps mithilfe von Ressourcen-Manager-Vorlagen](http://channel9.msdn.com/Events/Build/2015/2-620)
* [Erstellen von Azure-Ressourcen-Manager-Vorlagen](../azure-resource-manager/resource-group-authoring-templates.md)
* [JSONLint – The JSON Validator](http://jsonlint.com/)
* [ARMClient – Set up GitHub publishing to site](https://github.com/projectKudu/ARMClient/wiki/Setup-GitHub-publishing-to-Site)
* [Git Branching – Basic Branching and Merging](http://www.git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)
* [Blog von David Ebbo](http://blog.davidebbo.com/)
* [Azure PowerShell](/powershell/azure/overview)
* [Installieren und Konfigurieren der Azure-Befehlszeilenschnittstelle](../cli-install-nodejs.md)
* [Erstellen oder Bearbeiten von Benutzern in Azure AD](https://msdn.microsoft.com/library/azure/hh967632.aspx#BKMK_1)
* [Wiki zum Kudu-Projekt](https://github.com/projectkudu/kudu/wiki)


