---
title: Erstellen einer .NET Core-Web-App in einem Linux-Container in Azure | Microsoft-Dokumentation
description: "Stellen Sie in nur wenigen Minuten Ihre erste .NET Core-App „Hallo Welt“ für Web-Apps für Container bereit."
keywords: Azure App Service, Web-App, DotNet, Core, Linux, OSS
services: app-service
documentationCenter: 
authors: cephalin
manager: syntaxc4
editor: 
ms.assetid: c02959e6-7220-496a-a417-9b2147638e2e
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: linux
ms.devlang: na
ms.topic: quickstart
ms.date: 08/30/2017
ms.author: aelnably;wesmc;mikono;rachelap;cephalin;cfowler
ms.translationtype: HT
ms.sourcegitcommit: 12c20264b14a477643a4bbc1469a8d1c0941c6e6
ms.openlocfilehash: 5d84e558e2fd998df31725b71d1474c0a774490b
ms.contentlocale: de-de
ms.lasthandoff: 09/07/2017

---
# <a name="create-a-net-core-web-app-in-a-linux-container-in-azure"></a>Erstellen einer .NET Core-Web-App in einem Linux-Container in Azure

[Web-Apps für Container](app-service-linux-intro.md) bieten einen hochgradig skalierbaren Webhostingdienst mit Self-Patching unter dem Linux-Betriebssystem. In diesem Schnellstartartikel wird erläutert, wie eine [.NET Core](https://docs.microsoft.com/aspnet/core/)-App in Azure-Web-Apps für Container erstellt wird. Sie erstellen die Web-App mithilfe der [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) und stellen mit Git den .NET Core-Code für die Web-App bereit.

![In Azure ausgeführte Beispiel-App](media/quickstart-dotnetcore/dotnet-browse-azure.png)

Die folgenden Schritte können unter Mac, Windows oder Linux ausgeführt werden. 

## <a name="prerequisites"></a>Voraussetzungen

So führen Sie diesen Schnellstart durch:

* [Installation von Git](https://git-scm.com/)
* [Installieren des .NET Core SDK](https://www.microsoft.com/net/download/core)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="create-the-app-locally"></a>Lokales Erstellen der App ##

Erstellen Sie in einem Terminalfenster auf Ihrem Computer ein Verzeichnis mit dem Namen `hellodotnetcore`, und wechseln Sie dorthin. 

```bash
md hellodotnetcore
cd hellodotnetcore
``` 

Erstellen Sie eine neue .NET Core-Web-App.

```bash
dotnet new web
``` 

## <a name="run-the-app-locally"></a>Lokales Ausführen der App

Stellen Sie die NuGet-Pakete wieder her, und führen Sie die App aus.

```bash
dotnet restore
dotnet run
```

Öffnen Sie einen Webbrowser, und navigieren Sie zur App unter „http://localhost:5000“.

Auf der Seite wird die Nachricht **Hello World** aus der Beispiel-App angezeigt.

![Testen mit dem Browser](media/quickstart-dotnetcore/dotnet-browse-local.png)

Drücken Sie in Ihrem Terminalfenster **STRG+C**, um den Webserver zu beenden. Initialisieren Sie ein Git-Repository für das .NET Core-Projekt.

```bash
git init
git add .
git commit -m "first commit"
```

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Configure deployment user](../../../includes/configure-deployment-user.md)] 

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group.md)] 

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux.md)] 

## <a name="create-a-web-app"></a>Erstellen einer Web-App

Erstellen Sie eine [Web-App](../../app-service-web/app-service-web-overview.md) im App Service-Plan `myAppServicePlan` mit dem Befehl [az webapp create](/cli/azure/webapp#create). Vergessen Sie nicht, `<app name>` durch einen eindeutigen App-Namen zu ersetzen.

Die Laufzeit im folgenden Befehl ist auf `DOTNETCORE|1.1` festgelegt. Führen Sie zum Anzeigen aller unterstützten Laufzeiten den Befehl [az webapp list-runtimes](/cli/azure/webapp#list-runtimes) aus.

```azurecli-interactive
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app name> --runtime "DOTNETCORE|1.1" --deployment-local-git
```

[!INCLUDE [Create web app](../../../includes/app-service-web-create-web-app-result.md)] 

![Leere Web-App-Seite](media/quickstart-dotnetcore/dotnet-browse-created.png)

Sie haben eine leere neue Web-App in einem Linux-Container mit aktivierter Git-Bereitstellung erstellt.

[!INCLUDE [Push to Azure](../../../includes/app-service-web-git-push-to-azure.md)] 

```bash
Counting objects: 22, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (18/18), done.
Writing objects: 100% (22/22), 51.21 KiB | 3.94 MiB/s, done.
Total 22 (delta 1), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id '741f16d1db'.
remote: Generating deployment script.
remote: Project file path: ./hellodotnetcore.csproj
remote: Generated deployment script files
remote: Running deployment command...
remote: Handling ASP.NET Core Web Application deployment.
remote: ...............................................................................................
remote:   Restoring packages for /home/site/repository/hellodotnetcore.csproj...
remote: ....................................
remote:   Installing System.Xml.XPath 4.0.1.
remote:   Installing System.Diagnostics.Tracing 4.1.0.
remote:   Installing System.Threading.Tasks.Extensions 4.0.0.
remote:   Installing System.Reflection.Emit.ILGeneration 4.0.1.
remote:   ...
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
To https://cephalin-dotnetcore.scm.azurewebsites.net/cephalin-dotnetcore.git
 * [new branch]      master -> master
```

## <a name="browse-to-the-app"></a>Navigieren zur App

Navigieren Sie in Ihrem Webbrowser zu der bereitgestellten Anwendung.

```bash
http://<app_name>.azurewebsites.net
```

Der Node.js-Beispielcode wird in einer Azure App Service-Web-App ausgeführt.

![In Azure ausgeführte Beispiel-App](media/quickstart-dotnetcore/dotnet-browse-azure.png)

**Glückwunsch!** Sie haben Ihre erste Node.js-App für App Service bereitgestellt.

## <a name="update-and-redeploy-the-code"></a>Aktualisieren und erneutes Bereitstellen des Codes

Öffnen Sie im lokalen Verzeichnis die Datei _Startup.cs_. Nehmen Sie eine kleine Änderung am Text im Methodenaufruf `context.Response.WriteAsync` vor:

```csharp
await context.Response.WriteAsync("Hello Azure!");
```

Führen Sie für Ihre Änderungen in Git einen Commit aus, und übertragen Sie dann die Codeänderungen mithilfe von Push an Azure.

```bash
git commit -am "updated output"
git push azure master
```

Wechseln Sie nach Abschluss der Bereitstellung wieder zu dem Browserfenster, das im Schritt **Navigieren zur App** geöffnet wurde, und wählen Sie die Option „Aktualisieren“ aus.

![In Azure ausgeführte aktualisierte Beispiel-App](media/quickstart-dotnetcore/dotnet-browse-azure-updated.png)

## <a name="manage-your-new-azure-web-app"></a>Verwalten Ihrer neuen Azure-Web-App

Wechseln Sie zum <a href="https://portal.azure.com" target="_blank">Azure-Portal</a>, um die erstellte Web-App zu verwalten.

Klicken Sie im linken Menü auf **App Services** und anschließend auf den Namen Ihrer Azure-Web-App.

![Portalnavigation zur Azure-Web-App](./media/quickstart-dotnetcore/portal-app-service-list.png)

Die Übersichtsseite Ihrer Web-App wird angezeigt. Hier können Sie einfache Verwaltungsaufgaben wie Durchsuchen, Beenden, Neustarten und Löschen durchführen. 

![App Service-Seite im Azure-Portal](media/quickstart-dotnetcore/portal-app-overview.png)

Im linken Menü werden verschiedene Seiten für die Konfiguration Ihrer App angezeigt. 

[!INCLUDE [cli-samples-clean-up](../../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen einer .NET Core- und SQL-Datenbank-Web-App in Azure-Web-Apps für Container](tutorial-dotnetcore-sqldb-app.md)

