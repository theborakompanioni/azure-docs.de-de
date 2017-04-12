---
title: Bereitstellen der App in Azure App Service | Microsoft Docs
description: "Erfahren Sie, wie Sie Ihre App für Azure App Service bereitstellen."
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: f1464f71-2624-400e-86a2-e687e385804f
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2017
ms.author: cephalin;dariac
translationtype: Human Translation
ms.sourcegitcommit: 6ea03adaabc1cd9e62aa91d4237481d8330704a1
ms.openlocfilehash: 99ec39d3f0f6e82409de571db1e7c7c9468eb068
ms.lasthandoff: 04/06/2017


---
# <a name="deploy-your-app-to-azure-app-service"></a>Bereitstellen der App in Azure App Service
Dieser Artikel hilft Ihnen beim Ermitteln der besten Option für die Bereitstellung der Dateien für Ihre Web-App, das Back-End der mobilen App oder der API-App in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714). Er enthält auch Verweise zu den entsprechenden Ressourcen mit Anleitungen für die jeweils bevorzugte Option.

## <a name="overview"></a>Übersicht über die Azure App Service-Bereitstellung
Azure App Service verwaltet das Anwendungsframework für Sie (ASP.NET, PHP, Node.js, usw). Einige Frameworks sind standardmäßig aktiviert, während für andere, z. B. Java und Python, unter Umständen eine einfache Konfiguration durch das Setzen eines Häkchens erforderlich ist, um sie zu aktivieren. Außerdem können Sie Ihr Anwendungsframework anpassen, z. B. die PHP-Version oder die Bitanzahl der Laufzeit. Weitere Informationen finden Sie unter [Konfigurieren der App in Azure App Service](web-sites-configure.md).

Da Sie sich nicht um den Webserver oder das Anwendungsframework kümmern müssen, geht es bei der Bereitstellung Ihrer App für App Service nur darum, Ihren Code sowie Ihre Binärdateien, Inhaltsdateien und die entsprechenden Verzeichnisstrukturen im Verzeichnis [**/site/wwwroot**](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure) in Azure (bzw. für WebJobs im Verzeichnis **/site/wwwroot/App_Data/Jobs/**) bereitzustellen. App Service unterstützt drei unterschiedliche Bereitstellungsprozesse. Alle Bereitstellungsmethoden in diesem Artikel verwenden eines der folgenden Verfahren: 

* [FTP oder FTPS:](https://en.wikipedia.org/wiki/File_Transfer_Protocol) Verwenden Sie das gewünschte FTP- oder FTPS-fähige Tool, um die Dateien nach Azure zu verschieben – von [FileZilla](https://filezilla-project.org) bis hin zu IDEs mit vollem Funktionsumfang wie [NetBeans](https://netbeans.org). Genau genommen handelt es sich hierbei um das Hochladen von Dateien. Von App Service werden keine zusätzlichen Dienste bereitgestellt, wie Versionskontrolle, Dateistrukturverwaltung usw. 
* [Kudu (Git/Mercurial oder OneDrive/Dropbox):](https://github.com/projectkudu/kudu/wiki/Deployment) Kudu ist das [Bereitstellungsmodul](https://github.com/projectkudu/kudu/wiki) in App Service. Sie können Ihren Code direkt aus einem Repository an Kudu übertragen. Kudu stellt außerdem weitere Dienste bereit, wenn Code übertragen wird, z.B. Versionskontrolle, Paketwiederherstellung, MSBuild und [Webhooks](https://github.com/projectkudu/kudu/wiki/Web-hooks) für die kontinuierliche Bereitstellung und andere Automatisierungsaufgaben. Das Kudu-Bereitstellungsmodul unterstützt drei verschiedene Arten von Bereitstellungsquellen:   
  
  * Inhaltssynchronisierung aus OneDrive und Dropbox   
  * Repositorybasierte kontinuierliche Bereitstellung mit automatischer Synchronisierung aus GitHub, Bitbucket und Visual Studio Team Services  
  * Repositorybasierte Bereitstellung mit manueller Synchronisierung aus lokalem Git  
* [Web Deploy:](http://www.iis.net/learn/publish/using-web-deploy/introduction-to-web-deploy) Stellen Sie Code direkt aus Ihren bevorzugten Microsoft-Tools, z.B. Visual Studio, in App Service bereit. Hierbei kommen die gleichen Tools zum Einsatz, die auch die Bereitstellung für IIS-Server automatisieren. Dieses Tool unterstützt die Differenzbereitstellung (diff-only), Datenbankerstellung, Transformationen von Verbindungszeichenfolgen usw. Web Deploy unterscheidet sich von Kudu darin, dass Anwendungsbinärdateien erstellt werden, bevor sie für Azure bereitgestellt werden. Ähnlich wie bei FTP werden von App Service keine weiteren Dienste bereitgestellt.

Gängige Webentwicklungstools unterstützen einen oder mehrere dieser Bereitstellungsprozesse. Das von Ihnen gewählte Tool bestimmt zwar die Bereitstellungsprozesse, die Sie nutzen können, aber die eigentliche verfügbare DevOps-Funktionalität hängt von der Kombination aus Bereitstellungsprozess und den von Ihnen gewählten speziellen Tools ab. Wenn Sie den Web Deploy-Vorgang beispielsweise über [Visual Studio mit Azure SDK](#vspros)durchführen, können Sie die Paketwiederherstellungs- und MSBuild-Automatisierung in Visual Studio nutzen, auch wenn eine Automatisierung über Kudu nicht möglich ist. 

> [!NOTE]
> Diese Bereitstellungsprozesse umfassen nicht die eigentliche [Bereitstellung der Azure-Ressourcen](../azure-resource-manager/resource-group-template-deploy-portal.md), die für Ihre App ggf. erforderlich sind. In den meisten verlinkten Gewusst-wie-Artikeln wird aber ausführlich gezeigt, wie Sie die App UND den Code dafür bereitstellen. Zusätzliche Optionen für die Bereitstellung von Azure-Ressourcen finden Sie auch im Abschnitt [Automatisieren der Bereitstellung mit Befehlszeilentools](#automate).
> 
> 

## <a name="ftp"></a>Manuelles Bereitstellen durch Hochladen von Dateien über FTP
Wenn Sie es gewohnt sind, Ihre Webinhalte manuell auf einen Webserver zu kopieren, können Sie Dateien mithilfe eines [FTP](http://en.wikipedia.org/wiki/File_Transfer_Protocol)-Tools wie Windows-Explorer oder [FileZilla](https://filezilla-project.org/) kopieren.

Vorteile des manuellen Kopierens von Dateien:

* Sie können vertraute, einfache FTP-Tools verwenden. 
* Sie wissen genau, wohin Ihre Dateien kopiert werden.
* FTPS erhöht die Sicherheit.

Nachteile des manuellen Kopierens von Dateien:

* Sie müssen wissen, wie Sie Dateien in den richtigen Verzeichnissen in App Service bereitstellen. 
* Es gibt keine Versionskontrolle für ein Rollback, wenn Fehler auftreten.
* Es gibt keinen integrierten Bereitstellungsverlauf zur Behandlung von Problemen bei der Bereitstellung.
* Die Bereitstellung kann lange dauern, da viele FTP-Tools keine Differenzbereitstellung (diff-only) ermöglichen, es werden einfach alle Dateien kopiert.  

### <a name="howtoftp"></a>Hochladen von Dateien mit FTP
Das [Azure-Portal](https://portal.azure.com) stellt alle Informationen bereit, die Sie zum Herstellen von Verbindungen mit Ihren App-Verzeichnissen über FTP oder FTPS benötigen.

* [Bereitstellen der App in Azure App Service mithilfe von FTP](app-service-deploy-ftp.md)

## <a name="dropbox"></a>Bereitstellen per Synchronisierung mit einem Cloudordner
Eine gute Alternative zum [manuellen Kopieren von Dateien](#ftp) ist das Synchronisieren von Dateien und Ordnern mit App Service über einen Cloudspeicherdienst wie OneDrive und Dropbox. Bei der Synchronisierung mit einem Cloudordner wird der Kudu-Prozess für die Bereitstellung verwendet (siehe [Übersicht über die Bereitstellungsprozesse](#overview)).

Vorteile der Synchronisierung mit einem Cloudordner:

* Einfache Bereitstellung. Dienste wie OneDrive und Dropbox verfügen über Desktopclients für die Synchronisierung, sodass Ihr lokales Arbeitsverzeichnis auch Ihr Bereitstellungsverzeichnis ist.
* Bereitstellung mit nur einem Klick
* Alle Funktionen des Kudu-Bereitstellungsmoduls sind verfügbar (z.B. Paketwiederherstellung, Automatisierung).

Nachteile der Synchronisierung mit einem Cloudordner:

* Es gibt keine Versionskontrolle für ein Rollback, wenn Fehler auftreten.
* Es gibt keine automatisierte Bereitstellung; manuelle Synchronisierung ist erforderlich.

### <a name="howtodropbox"></a>Bereitstellen per Synchronisierung mit einem Cloudordner
Im [Azure-Portal](https://portal.azure.com)können Sie einen Ordner für die Inhaltssynchronisierung mit Ihrem OneDrive- oder Dropbox-Cloudspeicher bestimmen, mit Ihrem App-Code und den App-Inhalten in diesem Ordner arbeiten und per Klick auf eine Schaltfläche die Synchronisierung mit App Service durchführen.

* [Synchronisieren von Inhalten aus einem Cloudordner mit Azure App Service](app-service-deploy-content-sync.md) 

## <a name="continuousdeployment"></a>Kontinuierliche Bereitstellung über einen cloudbasierten Quellcodeverwaltungsdienst
Wenn Ihr Entwicklungsteam einen cloudbasierten Quellcodeverwaltungsdienst wie [Visual Studio Team Services](http://www.visualstudio.com/), [GitHub](https://www.github.com) oder [Bitbucket](https://bitbucket.org/) verwendet, können Sie App Service für die Integration in Ihr Repository und eine kontinuierliche Bereitstellung konfigurieren. 

Vorteile der Bereitstellung über einen cloudbasierten Quellcodeverwaltungsdienst:

* Es steht eine Versionskontrolle bereit, um Rollbacks zu ermöglichen.
* Sie können eine kontinuierliche Bereitstellung für Git-Repositorys (und ggf. Mercurial-Repositorys) konfigurieren. 
* Verzweigungsspezifische Bereitstellung: Sie können verschiedene Verzweigungen in unterschiedlichen [Slots](web-sites-staged-publishing.md)bereitstellen.
* Alle Funktionen des Kudu-Bereitstellungsmoduls sind verfügbar (z.B. Versionsverwaltung für die Bereitstellung, Rollback, Paketwiederherstellung, Automatisierung).

Nachteile der Bereitstellung über einen cloudbasierten Quellcodeverwaltungsdienst:

* Sie benötigen Kenntnisse des jeweils erforderlichen Quellcodeverwaltungsdiensts.

### <a name="vsts"></a>Kontinuierliche Bereitstellung über einen cloudbasierten Quellcodeverwaltungsdienst
Im [Azure-Portal](https://portal.azure.com)können Sie die kontinuierliche Bereitstellung aus GitHub, Bitbucket und Visual Studio Team Services konfigurieren.

* [Kontinuierliche Bereitstellung in Azure App Service](app-service-continuous-deployment.md) 

Informationen zum manuellen Konfigurieren der kontinuierlichen Bereitstellung in einem Cloudrepository, das nicht im Azure-Portal aufgeführt ist (etwa [GitLab](https://gitlab.com/)), finden Sie unter [Setting up continuous deployment using manual steps](https://github.com/projectkudu/kudu/wiki/Continuous-deployment#setting-up-continuous-deployment-using-manual-steps) (Manuelles Einrichten der kontinuierlichen Bereitstellung).

## <a name="localgitdeployment"></a>Bereitstellen aus lokalem Git
Wenn Ihr Entwicklungsteam einen lokalen, auf Git basierenden Quellcodeverwaltungsdienst verwendet, können Sie diesen als Bereitstellungsquelle für App Service verwenden. 

Vorteile der Bereitstellung aus lokalem Git:

* Es steht eine Versionskontrolle bereit, um Rollbacks zu ermöglichen.
* Verzweigungsspezifische Bereitstellung: Sie können verschiedene Verzweigungen in unterschiedlichen [Slots](web-sites-staged-publishing.md)bereitstellen.
* Alle Funktionen des Kudu-Bereitstellungsmoduls sind verfügbar (z.B. Versionsverwaltung für die Bereitstellung, Rollback, Paketwiederherstellung, Automatisierung).

Nachteile der Bereitstellung aus lokalem Git:

* Sie benötigen Kenntnisse des jeweils erforderlichen Quellcodeverwaltungsdiensts.
* Es gibt keine sofort einsetzbaren Lösungen für die kontinuierliche Bereitstellung. 

### <a name="vsts"></a>Bereitstellen aus lokalem Git
Im [Azure-Portal](https://portal.azure.com)können Sie die lokale Git-Bereitstellung konfigurieren.

* [Lokale Git-Bereitstellung in Azure App Service](app-service-deploy-local-git.md) 
* [Publishing to Azure Web Sites from any git/hg repo (Veröffentlichen auf Azure-Websites aus beliebigen git/hg-Repositorys, in englischer Sprache)](http://blog.davidebbo.com/2013/04/publishing-to-azure-web-sites-from-any.html).  

## <a name="deploy-using-an-ide"></a>Bereitstellen über eine IDE
Wenn Sie bereits [Visual Studio](https://www.visualstudio.com/products/visual-studio-community-vs.aspx) mit einem [Azure SDK](https://azure.microsoft.com/downloads/) oder andere IDE-Suites wie [Xcode](https://developer.apple.com/xcode/), [Eclipse](https://www.eclipse.org) oder [IntelliJ IDEA](https://www.jetbrains.com/idea/) verwenden, können Sie die Bereitstellung für Azure direkt aus der IDE durchführen. Diese Option ist ideal für allein arbeitende Entwickler.

Visual Studio unterstützt alle drei Bereitstellungsprozesse (FTP, Git und Web Deploy). Mit anderen IDEs ist die Bereitstellung in App Service möglich, wenn diese über eine FTP- oder Git-Integration verfügen (siehe [Übersicht über Bereitstellungsprozesse](#overview)).

Vorteile der Bereitstellung mit einer IDE:

* Potenzielle Verringerung des Toolaufwands für Ihren End-to-End-Anwendungslebenszyklus. Entwickeln, Debuggen, Nachverfolgen und Bereitstellen Ihrer App für Azure ohne Verlassen Ihrer IDE. 

Nachteile der Bereitstellung mit einer IDE:

* Höhere Komplexität in Bezug auf Tools
* Für ein Teamprojekt ist weiterhin ein Quellcodeverwaltungssystem erforderlich

<a name="vspros"></a> Weitere Vorteile der Bereitstellung über Visual Studio mit Azure SDK:

* Das Azure SDK macht Azure-Ressourcen in Visual Studio zu bevorzugten Elementen. Erstellen, Löschen, Bearbeiten, Starten und Beenden von Apps, Abfragen der Back-End-SQL-Datenbank, Live-Debuggen der Azure-App und vieles mehr. 
* Live-Bearbeitung von Codedateien in Azure
* Live-Debuggen von Apps in Azure
* Integrierter Azure Explorer
* Differenzbereitstellung (diff-only) 

### <a name="vs"></a>Gewusst wie: direktes Bereitstellen aus Visual Studio
* [Erste Schritte mit Azure und ASP.NET](app-service-web-get-started-dotnet.md). In dieser Ressource wird das Erstellen und Bereitstellen eines einfachen ASP.NET MVC-Webprojekts mithilfe von Visual Studio und Web Deploy erläutert.
* [Bereitstellen von Azure-Webaufträgen mit Visual Studio](websites-dotnet-deploy-webjobs.md). Hier erfahren Sie, wie Sie Konsolenanwendungsprojekte für die Bereitstellung als WebJobs konfigurieren.  
* [Bereitstellen einer sicheren ASP.NET MVC 5-Anwendung mit Mitgliedschafts-, OAuth- und SQL-Datenbank in Web-Apps](web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md). In dieser Ressource wird das Erstellen und Bereitstellen eines ASP.NET MVC-Webprojekts mit einer SQL-Datenbank unter Verwendung von Visual Studio, Web Deploy und Entity Framework Code First-Migrationen erläutert.
* [ASP.NET-Webbereitstellung mithilfe von Visual Studio](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/introduction)Dabei handelt es sich um eine Lernprogrammserie mit zwölf Teilen, in der die Bereitstellungsaufgaben vollständig vorgestellt werden. Seit dem Erscheinen des Lernprogramms sind einige Azure-Bereitstellungsfunktionen hinzugefügt worden. Hinweise erläutern, welche Informationen fehlen.
* [Deploying an ASP.NET Website to Azure in Visual Studio 2012 from a Git Repository directly](http://www.dotnetcurry.com/ShowArticle.aspx?ID=881) (Direktes Bereitstellen einer ASP.NET-Website auf Azure in Visual Studio 2012 über ein Git-Repository). In diesem Artikel wird erläutert, wie ein ASP.NET-Webprojekt in Visual Studio bereitgestellt wird und wie das Git-Plug-In verwendet wird, um den Code für Git zu übernehmen und Azure mit dem Git-Repository zu verknüpfen. Visual Studio 2013 und höhere Versionen bieten eine integrierte Git-Unterstützung. Daher ist die Installation eines Plug-Ins nicht erforderlich.

### <a name="aztk"></a>Gewusst wie: Bereitstellen mithilfe der Azure-Toolkits für Eclipse und IntelliJ IDEA
Microsoft ermöglicht das Bereitstellen von Web-Apps in Azure direkt aus Eclipse und IntelliJ mithilfe von [Azure Toolkit für Eclipse](../azure-toolkit-for-eclipse.md) und [Azure Toolkit für IntelliJ](../azure-toolkit-for-intellij.md). Die folgenden Tutorials veranschaulichen die Schritte für die Bereitstellung einer einfachen „Hello World“-Web-App in Azure mithilfe einer der IDEs:

* [Erstellen einer „Hello World“-Web-App für Azure in Eclipse](app-service-web-eclipse-create-hello-world-web-app.md). In diesem Tutorial erfahren Sie, wie Sie mit dem Azure-Toolkit für Eclipse eine „Hello World“-Web-App für Azure erstellen und bereitstellen.
* [Erstellen einer „Hello World“-Web-App für Azure in IntelliJ](app-service-web-intellij-create-hello-world-web-app.md). In diesem Tutorial erfahren Sie, wie Sie mit dem Azure-Toolkit für IntelliJ eine „Hello World“-Web-App für Azure erstellen und bereitstellen.

## <a name="automate"></a>Automatisieren der Bereitstellung mit Befehlszeilentools
Wenn Sie ein Befehlszeilenterminal als Entwicklungsumgebung bevorzugen, können Sie mithilfe von Befehlszeilentools Skripts für die Bereitstellungsaufgaben Ihre App Service-App schreiben. 

Vorteile der Bereitstellung mithilfe von Befehlszeilentools:

* Skriptgestützte Bereitstellungsszenarien
* Kombination der Bereitstellung von Azure-Ressourcen mit der von Code
* Integration der Azure-Bereitstellung in vorhandene Skripts zur Continous Integration

Nachteile der Bereitstellung mithilfe von Befehlszeilentools:

* Nicht geeignet für Entwickler, die eine GUI bevorzugen

### <a name="automatehow"></a>Automatisieren der Bereitstellung mithilfe von Befehlszeilentools

Eine Liste der Befehlszeilentools sowie Links zu Tutorials zu finden Sie unter [Automatisieren der Bereitstellung der Azure-App mit Befehlszeilentools](app-service-deploy-command-line.md). 

## <a name="nextsteps"></a>Nächste Schritte
In einigen Szenarios möchten Sie ggf. in der Lage sein, bequem zwischen einer Staging- und einer Produktionsversion Ihrer App zu wechseln. Weitere Informationen finden Sie unter [Stagingbereitstellung in Web-Apps](web-sites-staged-publishing.md).

Die Aufstellung eines Sicherungs- und Wiederherstellungsplans ist ein wesentlicher Bestandteil jedes Bereitstellungsworkflows. Weitere Informationen zum Sicherungs- und Wiederherstellungsfeature von App Service finden Sie unter [Sichern von Web-Apps](web-sites-backup.md).  

Informationen zur Verwendung der rollenbasierten Zugriffssteuerung von Azure zum Verwalten des Zugriffs auf die App Service-Bereitstellung finden Sie unter [RBAC and Web App Publishing](https://azure.microsoft.com/blog/2015/01/05/rbac-and-azure-websites-publishing/)(Rollenbasierte Zugriffssteuerung und Web-App-Veröffentlichung).


