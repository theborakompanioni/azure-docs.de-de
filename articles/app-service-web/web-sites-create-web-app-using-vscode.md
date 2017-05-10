---
title: Erstellen einer ASP.NET Core-Web-App in Visual Studio Code
description: In diesem Tutorial erfahren Sie, wie Sie mithilfe von Visual Studio Code eine ASP.NET Core-Web-App erstellen.
services: app-service\web
documentationcenter: .net
author: erikre
manager: erikre
editor: jimbe
ms.assetid: 877bff08-9ef7-405a-a1ca-1194f33c55f2
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: dotnet
ms.devlang: na
ms.topic: article
ms.date: 02/26/2016
ms.author: cephalin
ms.translationtype: Human Translation
ms.sourcegitcommit: 64bd7f356673b385581c8060b17cba721d0cf8e3
ms.openlocfilehash: e8f78602b293863b2e58160a5eb2cf8a5855576b
ms.contentlocale: de-de
ms.lasthandoff: 05/02/2017


---
# <a name="create-an-aspnet-core-web-app-in-visual-studio-code"></a>Erstellen einer ASP.NET Core-Web-App in Visual Studio Code
## <a name="overview"></a>Übersicht
In diesem Tutorial erfahren Sie, wie Sie eine ASP.NET 5-Web-App mit [Visual Studio Code (VS Code)](http://code.visualstudio.com//Docs/whyvscode) erstellen und in [Azure App Service](../app-service/app-service-value-prop-what-is.md) bereitstellen. 

> [!NOTE]
> Obwohl sich dieser Artikel auf Web-Apps bezieht, gilt er auch für API-Apps und mobile Apps. 
> 
> 

Das Design von ASP.NET wurde für ASP.NET Core deutlich verändert. ASP.NET Core ist ein neues plattformübergreifendes Open-Source-Framework zum Erstellen moderner cloudbasierter Web-Apps mithilfe von .NET. Weitere Informationen finden Sie unter [Introduction to ASP.NET Core](http://docs.asp.net/latest/conceptual-overview/aspnet.html)(Einführung in ASP.NET Core). Informationen zu Azure App Service-Web-Apps finden Sie unter [Web-Apps – Übersicht](app-service-web-overview.md).

[!INCLUDE [app-service-web-try-app-service.md](../../includes/app-service-web-try-app-service.md)]

## <a name="prerequisites"></a>Voraussetzungen
* Installieren Sie [VS Code](http://code.visualstudio.com/Docs/setup).
* Installieren Sie Git – entweder über [Chocolatey](https://chocolatey.org/packages/git) oder über [git-scm.com](http://git-scm.com/downloads). Falls Sie noch keine Erfahrung mit Git haben, entscheiden Sie sich für [git-scm.com](http://git-scm.com/downloads) und wählen Sie die Option **Git über die Windows-Eingabeaufforderung verwenden**aus. Legen Sie nach der Installation von Git den Git-Benutzernamen und die dazugehörige E-Mail-Adresse fest, da diese Angaben später in diesem Lernprogramm (beim Ausführen eines Commits in VS Code) benötigt werden.  

## <a name="install-aspnet-core"></a>Installieren von ASP.NET Core
ASP.NET Core ist ein schlanker .NET-Stapel für die Erstellung moderner Cloud- und Web-Apps zum Ausführen unter OS X, Linux und Windows. Er wurde von Grund auf neu als optimiertes Entwicklungsframework für Apps konzipiert, die entweder in der Cloud bereitgestellt oder lokal ausgeführt werden. Er besteht aus modularen Komponenten mit minimalem Mehraufwand und ermöglicht Ihnen eine flexible Lösungsentwicklung.

Dieses Tutorial unterstützt Sie beim Einstieg in die Anwendungserstellung mit den neuesten Entwicklungsversionen von ASP.NET Core. Die folgenden Anweisungen beziehen sich explizit auf Windows. Anweisungen zur Installation unter OS X, Linux und Windows finden Sie unter [Erste Schritte mit ASP.NET Core](https://docs.microsoft.com/aspnet/core/getting-started). 


> [!NOTE]
> Ausführlichere Installationsanweisungen für OS X, Linux und Windows finden Sie unter [Installing ASP.NET Core](https://code.visualstudio.com/Docs/ASPnet5#_installing-aspnet-5-and-dnx)(Installieren von ASP.NET Core). 
> 
> 

## <a name="create-the-web-app"></a>Erstellen der Web-App
In diesem Abschnitt erfahren Sie, wie Sie mit der CLI von .NET das Gerüst für eine neue ASP.NET-Web-App erstellen. 

1. Geben Sie an der Eingabeaufforderung Folgendes ein, um den Projektordner und das Gerüst für die App zu erstellen:
   
```terminal
mkdir SampleWebApp
cd SampleWebApp
dotnet new mvc
```
![.NET CLI: ASP.NET Core-Generator](./media/web-sites-create-web-app-using-vscode/dotnetcore-mvc-01.png)

2. Führen Sie zum Installieren der benötigten NuGet-Pakete den folgenden Befehl aus:
   
    ```terminal
    dotnet restore
    ```

## <a name="run-the-web-app-locally"></a>Ausführen der Web-App (lokal)
Nachdem Sie die Web-App erstellt und alle erforderlichen NuGet-Pakete abgerufen haben, können Sie die Web-App lokal ausführen.

1. Führen Sie die App aus (mit dem Befehl `dotnet run` wird die App erstellt, falls sie nicht mehr aktuell ist):
    ```terminal
    dotnet run
    ```
2. Öffnen Sie einen Browser, und navigieren Sie zur folgenden URL:
   
    **http://localhost:5000**
   
    Die Standardseite der Web-App wird wie folgt angezeigt:
   
    ![Lokale Web-App in einem Browser](./media/web-sites-create-web-app-using-vscode/08-web-app.png)
3. Schließen Sie Ihren Browser. Drücken Sie im **Befehlsfenster** die Tastenkombination **STRG+C**, um die Anwendung zu beenden und das **Befehlsfenster** zu schließen. 

## <a name="create-a-web-app-in-the-azure-portal"></a>Erstellen einer Web-App im Azure-Portal
Die folgenden Schritte dienen zum Erstellen einer Web-App im Azure-Portal:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Klicken Sie links oben im Portal auf **NEU** .
3. Klicken Sie auf **Web-Apps > Web-App**.
   
    ![Azure – neue Web-App](./media/web-sites-create-web-app-using-vscode/09-azure-newwebapp.png)
4. Geben Sie einen Wert für **Name** ein (beispielsweise **SampleWebAppDemo**). Dieser Name muss eindeutig sein. Die Eindeutigkeit wird vom Portal bei der Eingabe überprüft. Falls Sie sich für einen anderen Wert entscheiden, müssen Sie im weiteren Verlauf dieses Tutorials jedes Vorkommen von **SampleWebAppDemo** durch diesen anderen Wert ersetzen. 
5. Wählen Sie einen vorhandenen **App Service-Plan** aus, oder erstellen Sie einen neuen Plan. Wenn Sie einen neuen Plan erstellen, wählen Sie Tarif, Standort und weitere Optionen. Weitere Informationen zu App Service-Plänen finden Sie unter [Azure App Service-Pläne – Detaillierte Übersicht](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
   
    ![Azure – Blatt für neue Web-App](./media/web-sites-create-web-app-using-vscode/10-azure-newappblade.png)
6. Klicken Sie auf **Erstellen**.
   
    ![Blatt für Web-App](./media/web-sites-create-web-app-using-vscode/11-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a>Aktivieren der Git-Veröffentlichung für die neue Web-App
Git ist ein verteiltes Versionskontrollsystem, mit dem Sie Ihre Azure App Service-Web-App bereitstellen können. Dazu speichern Sie den Code, den Sie für Ihre Web-App geschrieben haben, in einem lokalen Git-Repository und stellen ihn anschließend mithilfe eines Pushvorgangs zu einem Remoterepository in Azure bereit.   

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Klicken Sie auf **Durchsuchen**.
3. Klicken Sie auf **Web-Apps** , um eine Liste mit den Web-Apps anzuzeigen, die Ihrem Azure-Abonnement zugeordnet sind.
4. Wählen Sie die Web-App aus, die Sie in diesem Lernprogramm erstellt haben.
5. Klicken Sie auf dem Blatt der Web-App auf **Einstellungen** > **Continuous Deployment**. 
   
    ![Azure – Web-App-Host](./media/web-sites-create-web-app-using-vscode/14-azure-deployment.png)
6. Klicken Sie auf **Quelle auswählen > Lokales Git-Repository**.
7. Klicken Sie auf **OK**.
   
    ![Lokales Git-Repository in Azure](./media/web-sites-create-web-app-using-vscode/15-azure-localrepository.png)
8. Falls Sie noch keine Anmeldeinformationen für die Bereitstellung einer Web-App oder einer anderen App Service-App eingerichtet haben, holen Sie dies jetzt nach:
   
   * Klicken Sie auf **Einstellungen** > **Anmeldeinformationen für die Bereitstellung**. Das Blatt **Anmeldeinformationen für die Bereitstellung festlegen** wird angezeigt.
   * Geben Sie einen Benutzernamen und ein Kennwort ein.  Dieses Kennwort benötigen Sie später bei der Git-Einrichtung.
   * Klicken Sie auf **Speichern**.
9. Klicken Sie auf dem Blatt der Web-App auf **Einstellungen > Eigenschaften**. Die URL des Git-Remoterepositorys, in dem Sie die Bereitstellung durchführen möchten, wird unter **GIT-URL**angezeigt.
10. Kopieren Sie den unter **GIT-URL** angegebenen Wert zur späteren Verwendung im Lernprogramm.
    
    ![Git-URL in Azure](./media/web-sites-create-web-app-using-vscode/17-azure-giturl.png)

## <a name="publish-your-web-app-to-azure-app-service"></a>Veröffentlichen Ihrer Web-App in Azure App Service
In diesem Abschnitt erfahren Sie, wie Sie ein lokales Repository erstellen und einen Pushvorgang von diesem Repository an Azure durchführen, um Ihre Web-App in Azure bereitzustellen.

1. Wählen Sie in VS Code auf der linken Navigationsleiste die Option **Git** aus.
   
    ![Git-Symbol in VS Code](./media/web-sites-create-web-app-using-vscode/git-icon.png)
2. Wählen Sie **Git-Repository initialisieren** aus, um sicherzustellen, dass in Ihrem Arbeitsbereich die Git-Quellcodeverwaltung aktiviert ist. 
   
    ![Git-Initialisierung](./media/web-sites-create-web-app-using-vscode/19-initgit.png)
3. Öffnen Sie das Befehlsfenster, und wechseln Sie zum Verzeichnis Ihrer Web-App. Geben Sie dann den folgenden Befehl ein:
   
        git config core.autocrlf false
   
    Mit diesem Befehl wird ein Problem mit Text bei CRLF- und LF-Endungen verhindert.
4. Fügen Sie in VS Code eine Commitnachricht hinzu, und klicken Sie anschließend auf das Häkchen **Commit für alle** .
   
    ![Git – umfassendes Commit](./media/web-sites-create-web-app-using-vscode/20-git-commit.png)
5. Nach Abschluss der Verarbeitung durch Git sehen Sie, dass im Git-Fenster unter **Änderungen**keine Dateien aufgeführt sind. 
   
    ![Git – keine Änderungen](./media/web-sites-create-web-app-using-vscode/no-changes.png)
6. Kehren Sie zum Befehlsfenster zurück, in dem die Eingabeaufforderung auf das Verzeichnis mit Ihrer Web-App zeigt.
7. Erstellen Sie einen Remotebezug, um Updates mittels Push an Ihre Web-App zu übertragen. Verwenden Sie hierbei die zuvor kopierte (auf ".git" endende) Git-URL:
   
        git remote add azure [URL for remote repository]
8. Konfigurieren Sie Git, um Ihre Anmeldeinformationen lokal zu speichern, sodass sie automatisch an Ihre Push-Befehle angehängt werden, die vom Visual Studio-Code generiert werden.
   
        git config credential.helper store
9. Übertragen Sie Ihre Änderungen mithilfe des folgenden Befehls per Push an Azure: Nach diesem ersten Push in Azure können Sie alle Push-Befehle über Visual Studio-Code ausführen. 
   
        git push -u azure master
   
    Sie werden aufgefordert, das zuvor in Azure erstellte Kennwort einzugeben. **Hinweis: Das Kennwort ist nicht sichtbar.**
   
    Die Ausgabe des obigen Befehls endet mit dem Hinweis, dass die Bereitstellung erfolgreich durchgeführt wurde:
   
        remote: Deployment successful.
        To https://user@testsite.scm.azurewebsites.net/testsite.git
        [new branch]      master -> master

> [!NOTE]
> Wenn Sie Änderungen an Ihrer App vornehmen, können Sie mithilfe der integrierten Git-Funktionen direkt in Visual Studio-Code die erneute Veröffentlichung ausführen, indem Sie die Option **Commit für alle** gefolgt von der Option **Push** verwenden. Sie finden Sie die Option **Push** im Dropdownmenü neben den Schaltflächen **Commit für alle** und **Aktualisieren**.
> 
> 

Wenn Sie an einem Projekt zusammenarbeiten müssen, sollten Sie zwischen Push-Vorgängen auf Azure Push-Vorgänge auf GitHub vornehmen.

## <a name="run-the-app-in-azure"></a>Ausführen der App in Azure
Nachdem Sie Ihre Web-App nun bereitgestellt haben, können wir sie ausführen, während sie in Azure gehostet ist. 

Dies kann auf zwei Arten erfolgen:

* Öffnen Sie einen Browser, und geben Sie den Namen Ihrer Web-App wie folgt ein:   
  
        http://SampleWebAppDemo.azurewebsites.net
* Navigieren Sie im Azure-Portal zum Blatt für Ihre Web-App und klicken Sie auf **Durchsuchen** , um Ihre App anzuzeigen. 
* in Ihrem Standardbrowser anzuzeigen.

![Azure-Web-App](./media/web-sites-create-web-app-using-vscode/21-azurewebapp.png)

## <a name="summary"></a>Zusammenfassung
In diesem Lernprogramm wurde gezeigt, wie Sie eine Web-App in VS Code erstellen und in Azure bereitstellen. Weitere Informationen zu VS Code finden Sie im Artikel [Why Visual Studio Code?](https://code.visualstudio.com/Docs/) (Argumente für Visual Studio Code). Informationen zu App Service-Web-Apps finden Sie unter [Web-Apps – Übersicht](app-service-web-overview.md). 


