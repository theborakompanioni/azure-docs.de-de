---
title: Verwenden von .NET Core in Web-App unter Linux | Microsoft-Dokumentation
description: Verwenden Sie .NET Core in Web-App unter Linux.
keywords: Azure App Service, Web-App, DotNet, Core, Linux, OSS
services: app-service
documentationCenter: 
authors: michimune, rachelappel
manager: erikre
editor: 
ms.assetid: c02959e6-7220-496a-a417-9b2147638e2e
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: aelnably;wesmc;mikono;rachelap
ms.translationtype: HT
ms.sourcegitcommit: 3b15d6645b988f69f1f05b27aff6f726f34786fc
ms.openlocfilehash: cbcc43e04e58894b6505fdf6c48c7792343049b9
ms.contentlocale: de-de
ms.lasthandoff: 07/26/2017

---

# <a name="use-net-core-in-an-azure-web-app-on-linux"></a>Verwenden von .NET Core in einer Azure-Web-App unter Linux #

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

[Web-App](https://docs.microsoft.com/azure/app-service-web/app-service-linux-intro) unter Linux bietet einen hochgradig skalierbaren Webhosting-Dienst mit Self-Patching unter dem Linux-Betriebssystem. Dieses Tutorial enthält eine schrittweise Anleitung zum Erstellen einer [.NET Core](https://docs.microsoft.com/aspnet/core/)-App für Azure-Web-App unter Linux. 

![Web-App unter Linux][10]

Die folgenden Schritte können unter Mac, Windows oder Linux ausgeführt werden.

## <a name="prerequisites"></a>Voraussetzungen ##

Für dieses Tutorial benötigen Sie Folgendes: 

* Installieren Sie das [.NET Core SDK](https://www.microsoft.com/net/download/core).
* Installieren Sie [Git](https://git-scm.com/downloads).

[!INCLUDE [Free trial note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-local-net-core-application"></a>Erstellen einer lokalen .NET Core-Anwendung ##

Starten Sie eine neue Terminalsitzung. Erstellen Sie ein Verzeichnis mit dem Namen `hellodotnetcore`, und wechseln Sie dorthin. Geben Sie anschließend Folgendes ein: 

```
dotnet new web
``` 

  Dieser Befehl erstellt unter dem aktuellen Verzeichnis drei Dateien (*hellodotnetcore.csproj*, *Program.cs* und *Startup.cs*) und einen leeren Ordner (*wwwroot/*). Der Inhalt der Datei `.csproj` sollte in etwa wie folgt aussehen: 

```xml
  <!-- Empty lines are omitted. -->

  <Project Sdk="Microsoft.NET.Sdk.Web">
        <PropertyGroup>
        <TargetFramework>netcoreapp1.1</TargetFramework>
        </PropertyGroup>
        <ItemGroup>
        <Folder Include="wwwroot\" />
        </ItemGroup>
        <ItemGroup>
        <PackageReference Include="Microsoft.AspNetCore" Version="1.1.2" />
        </ItemGroup>
  </Project>
```

Da es sich bei dieser App um eine Webanwendung handelt, wurde der Datei *hellodotnetcore.csproj* automatisch ein Verweis auf ein ASP.NET Core-Paket hinzugefügt. Die Versionsnummer des Pakets wird gemäß dem gewählten Framework festgelegt. Dieses Beispiel verweist auf die ASP.NET Core-Version 1.1.2, da .NET Core 1.1 verwendet wird.

## <a name="build-and-test-the-application-locally"></a>Lokales Erstellen und Testen der Anwendung ##

Mit den Befehlen `dotnet restore` und `dotnet run` können Sie Ihre .NET Core-App erstellen und ausführen, wie hier gezeigt:

```
dotnet restore
dotnet run
```


Beim Start der Anwendung erscheint eine Meldung mit dem Hinweis, dass die App an einem Port auf eingehende Anforderungen lauscht. 

```bash
Hosting environment: Production
Content root path: C:\hellodotnetcore
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

Navigieren Sie in Ihrem Browser zu `http://localhost:5000/`, um die Anwendung zu testen. Wenn alles funktioniert, wird „Hello World!“ als Ergebnistext angezeigt.

![Testen mit dem Browser][7]

## <a name="create-a-net-core-app-in-the-azure-portal"></a>Erstellen einer .NET Core-App im Azure-Portal ##

Erstellen Sie zunächst eine leere Web-App. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an, und erstellen Sie eine neue [Web-App unter Linux](https://portal.azure.com/#create/Microsoft.AppSvcLinux).

![Erstellen einer Web-App][1]

Geben Sie auf der Seite **Erstellen** Details zu Ihrer Web-App an:

![Auswählen eines .NET Core-Laufzeitstapels][2]

Orientieren Sie sich beim Ausfüllen der Seite **Erstellen** an der folgenden Tabelle. Wählen Sie dann **OK** und anschließend **Erstellen** aus, um die App zu erstellen.

| Einstellung      | Empfohlener Wert  | Beschreibung                                        |
| ------------ | ---------------- | -------------------------------------------------- |
| App-Name | hellodotnetcore  | Der Name Ihrer App. Dieser Name muss eindeutig sein. |
| Abonnement | Ein vorhandenes Abonnement | Das Azure-Abonnement. |
| Ressourcengruppe | myResourceGroup |  Die Azure-Ressourcengruppe für die Web-App. |
| App Service-Plan | Name eines vorhandenen App Service-Plans |  Der App Service-Plan.  |
| Container konfigurieren | .NET Core 1.1 | Der Containertyp für diese Web-App: integriert, Docker oder private Registrierung. |
| Imagequelle  | Integriert  |  Die Quelle des Images. |
| Laufzeitstapel  | .NET Core 1.1  | Der Laufzeitstapel und die Version.  |

## <a name="deploy-your-application-via-git"></a>Bereitstellen der Anwendung über Git ##

Verwenden Sie Git, um die .NET Core-Anwendung für die Azure App Service-Web-App unter Linux bereitzustellen.

Die Git-Bereitstellung wurde für die neue Azure-Web-App bereits konfiguriert. Sie finden die URL der Git-Bereitstellung nach dem Einfügen des Namens Ihrer Web-App unter folgender URL:

```https://{your web app name}.scm.azurewebsites.net/api/scm/info```

Die Git-URL weist basierend auf dem Namen Ihrer Web-App das folgende Format auf:

```https://{your web app name}.scm.azurewebsites.net/{your web app name}.git```

Führen Sie die folgenden Befehle aus, um die lokale Anwendung für Ihre Azure-Web-App bereitzustellen: 
 
```bash
git init
git remote add azure <Git deployment URL from above>
git add *.csproj *.cs
git commit -m "Initial deployment commit"
git push azure master
```

Sie müssen keine Dateien in den Verzeichnissen *bin/* oder *obj/* pushen, da Ihre Anwendung in der Cloud erstellt wird, wenn die Quelldateien der Anwendung per Push an Azure übertragen werden. Nach Abschluss der Erstellung werden Binärdateien in das Anwendungsverzeichnis unter */home/site/wwwroot/* kopiert.

Vergewissern Sie sich, dass die Remotebereitstellungsvorgänge erfolgreich waren. Pushvorgänge dauern möglicherweise eine Weile, da die Paketauflösung und die Erstellung in der Cloud erfolgen. Es werden mehrere Statusmeldungen angezeigt – unter anderem welche mit dem Hinweis, dass Dateien kopiert wurden. Die Ausgabe sollte in etwa wie folgt aussehen:

```bash
/* some output has been removed for brevity */
remote: Copying file: 'System.Net.Websockets.dll' 
remote: Copying file: 'System.Runtime.CompilerServices.Unsafe.dll' 
remote: Copying file: 'System.Runtime.Serialization.Primitives.dll' 
remote: Copying file: 'System.Text.Encodings.Web.dll' 
remote: Copying file: 'hellodotnetcore.deps.json' 
remote: Copying file: 'hellodotnetcore.dll' 
remote: Omitting next output lines...
remote: Finished successfully.
remote: Running post deployment commands...
remote: Deployment successful.
To https://hellodotnetcore.scm.azurewebsites.net/
 * [new branch]           master -> master

```

Starten Sie Ihre Web-App nach Abschluss der Bereitstellung neu, damit die Bereitstellung wirksam wird. Navigieren Sie hierzu im Azure-Portal zur Übersichtsseite**** Ihrer Web-App. Wählen Sie auf der Seite die Schaltfläche **Neu starten** aus. Wenn ein Popupfenster erscheint, wählen Sie zur Bestätigung **Ja** aus. Anschließend können Sie Ihre Web-App verwenden, wie hier zu sehen:

![Verwenden einer für Azure App Service bereitgestellten .NET Core-App unter Linux][10]

[!INCLUDE [Clean-up section](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Nächste Schritte
* [Häufig gestellte Fragen zu Azure App Service-Web-App unter Linux](./app-service-linux-faq.md)

[1]: ./media/app-service-linux-using-dotnetcore/top-level-create.png
[2]: ./media/app-service-linux-using-dotnetcore/dotnet-new-webapp.png
[7]: ./media/app-service-linux-using-dotnetcore/dotnet-browse-local.png
[10]: ./media/app-service-linux-using-dotnetcore/dotnet-browse-azure.png

