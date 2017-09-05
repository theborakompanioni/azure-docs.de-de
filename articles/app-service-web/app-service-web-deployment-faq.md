---
title: "Häufig gestellte Fragen zur Bereitstellung von Azure-Web-Apps | Microsoft-Dokumentation"
description: "Hier erhalten Sie Antworten auf häufig gestellte Fragen zur Bereitstellung für das Web-Apps-Feature von Azure App Service."
services: app-service\web
documentationcenter: 
author: genlin
manager: cshepard
editor: 
tags: top-support-issue
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: genli
ms.translationtype: Human Translation
ms.sourcegitcommit: db18dd24a1d10a836d07c3ab1925a8e59371051f
ms.openlocfilehash: 0a184d21a2c5ed1166aa9011ad7f077bcacc0d94
ms.contentlocale: de-de
ms.lasthandoff: 06/15/2017

---
# <a name="deployment-faqs-for-web-apps-in-azure"></a>Häufig gestellte Fragen zur Bereitstellung von Web-Apps in Azure

In diesem Artikel erhalten Sie Antworten auf häufig gestellte Fragen zu Problemen bei der Bereitstellung für das bei [Web-Apps-Feature von Azure App Service](https://azure.microsoft.com/services/app-service/web/).

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="i-am-just-getting-started-with-app-service-web-apps-how-do-i-publish-my-code"></a>Ich mache gerade die ersten Schritte mit den App Service-Web-Apps. Wie veröffentliche ich meinen Code?

Hier finden Sie einige Möglichkeiten für die Veröffentlichung Ihres Web-App-Codes:

*   Bereitstellen über Visual Studio. Wenn Sie über die Visual Studio-Lösung verfügen, klicken Sie mit der rechten Maustaste auf das Webanwendungsprojekt, und wählen Sie dann **Veröffentlichen** aus.
*   Bereitstellen über einen FTP-Client. Laden Sie im Azure-Portal das Veröffentlichungsprofil für die Web-App herunter, für die Sie Code bereitstellen möchten. Laden Sie die Dateien anschließend in das Verzeichnis „\site\wwwroot“ hoch. Verwenden Sie dabei die FTP-Anmeldeinformationen desselben Veröffentlichungsprofils.

Weitere Informationen finden Sie unter [Bereitstellen der App in App Service](web-sites-deploy.md).

## <a name="i-see-an-error-message-when-i-try-to-deploy-from-visual-studio-how-do-i-resolve-this"></a>Wenn ich die Bereitstellung über Visual Studio versuche, wird eine Fehlermeldung angezeigt. Wie löse ich dieses Problem?

Wenn die folgende Meldung angezeigt wird, verwenden Sie möglicherweise eine ältere Version des SDK: „Fehler bei der Bereitstellung für die Ressource ‚IhrRessourcenname‘ in der Ressourcengruppe ‚IhreRessourcengruppe‘: MissingRegistrationForLocation: Das Abonnement ist nicht für den Ressourcentyp ‚components‘ am Ort ‚USA, Mitte‘ registriert. Bitte registrieren Sie sich erneut für diesen Anbieter, um Zugang zu diesem Ort zu erhalten.“ 

Um diesen Fehler zu beheben, führen Sie eine Aktualisierung auf das [neueste SDK](https://azure.microsoft.com/downloads/) aus. Wenn diese Meldung angezeigt wird und Sie über das neueste SDK verfügen, senden Sie eine Supportanfrage.

## <a name="how-do-i-deploy-an-aspnet-application-from-visual-studio-to-app-service"></a>Wie stelle ich eine ASP.NET-Anwendung über Visual Studio in App Service bereit?
<a id="deployasp"></a>

Im Tutorial [Erstellen Ihrer ersten ASP.NET-Web-App in Azure in fünf Minuten](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-get-started/) erfahren Sie, wie Sie eine ASP.NET-Webanwendung mithilfe von Visual Studio 2015 in einer Web-App in App Service bereitstellen.

## <a name="what-are-the-different-types-of-deployment-credentials"></a>Welche verschiedenen Typen von Anmeldeinformationen für die Bereitstellung gibt es?

App Service unterstützt zwei Arten von Anmeldeinformationen für lokale Git-Bereitstellungen und FTP/S-Bereitstellungen. Weitere Informationen zum Konfigurieren der Anmeldeinformationen für die Bereitstellung finden Sie unter [Konfigurieren der Anmeldeinformationen für die Bereitstellung für App Service](app-service-deployment-credentials.md).

## <a name="what-is-the-file-or-directory-structure-of-my-app-service-web-app"></a>Wie lautet die Datei- oder Verzeichnisstruktur meiner App Service-Web-App?

Informationen zur Dateistruktur Ihrer App Service-App finden Sie unter [File Structure in Azure](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure) (Dateistruktur in Azure).

## <a name="how-do-i-resolve-ftp-error-550---there-is-not-enough-space-on-the-disk-when-i-try-to-ftp-my-files"></a>Wie behebe ich den Fehler „FTP-Fehler 550 - Es steht nicht genug Speicherplatz auf dem Datenträger zur Verfügung“, wenn ich FTP für meine Dateien verwenden möchte?

Wenn diese Meldung angezeigt wird, ist es wahrscheinlich, dass Sie auf ein Datenträgerkontingent im Serviceplan Ihrer Web-App gestoßen sind. Möglicherweise müssen Sie je nach Speicherbedarf auf eine höhere Dienstebene zentral hochskalieren. Weitere Informationen zu den Tarifen und Ressourcenlimits finden Sie unter [App Service – Preise](https://azure.microsoft.com/pricing/details/app-service/).

## <a name="how-do-i-set-up-continuous-deployment-for-my-app-service-web-app"></a>Wie richte ich für meine App Service-Web-App die kontinuierliche Bereitstellung ein?

Sie können die kontinuierliche Bereitstellung über mehrere Ressourcen einrichten, z.B. Visual Studio Team Services, OneDrive, GitHub, Bitbucket, Dropbox und andere Git-Repositorys. Diese Optionen sind im Portal verfügbar. [Kontinuierliche Bereitstellung in App Service](app-service-continuous-deployment.md) ist ein nützliches Tutorial, das erläutert, wie Sie die kontinuierliche Bereitstellung einrichten.

## <a name="how-do-i-troubleshoot-issues-with-continuous-deployment-from-github-and-bitbucket"></a>Wie behebe ich Probleme mit der kontinuierlichen Bereitstellung über GitHub und Bitbucket?

Informationen zum Untersuchen von Problemen mit der kontinuierlichen Bereitstellung über GitHub oder Bitbucket finden Sie unter [Investigating continuous Deployment](https://github.com/projectkudu/kudu/wiki/Investigating-continuous-deployment) (Untersuchen der kontinuierlichen Bereitstellung).

## <a name="i-cant-ftp-to-my-site-and-publish-my-code-how-do-i-resolve-this"></a>Ich kann FTP nicht für meine Website verwenden und meinen Code veröffentlichen. Wie löse ich dieses Problem?

So beheben Sie FTP-Probleme:

1. Überprüfen Sie, ob Sie den richtigen Hostnamen und die richtigen Anmeldeinformationen eingegeben haben. Ausführliche Informationen zu den verschiedenen Typen von Anmeldeinformationen und deren Verwendung finden Sie unter [Deployment Credentials](https://github.com/projectkudu/kudu/wiki/Deployment-credentials) (Anmeldeinformationen für die Bereitstellung).
2. Vergewissern Sie sich, dass die FTP-Ports nicht durch eine Firewall blockiert werden. Die Ports sollten diese Einstellungen haben:
    * Steuerungsport für FTP-Verbindung: 21
    * Port für FTP-Datenverbindung: 989, 10001-10300

## <a name="how-do-i-publish-my-code-to-app-service"></a>Wie veröffentliche ich meinen Code in App Service?

Der Azure-Schnellstart hilft Ihnen bei der Bereitstellung Ihrer App, indem Sie den Bereitstellungsstapel und die Methode Ihrer Wahl verwenden. Um den Schnellstart zu verwenden, wechseln Sie im Azure-Portal zu **Einstellungen** > **App-Bereitstellung**.

## <a name="why-does-my-app-sometimes-restart-after-deployment-to-app-service"></a>Warum wird meine App nach der Bereitstellung in App Service manchmal neu gestartet?

Informationen zu den Umständen, unter denen eine Anwendungsbereitstellung zu einem Neustart führen kann, finden Sie unter [Deployment vs. runtime issues](https://github.com/projectkudu/kudu/wiki/Deployment-vs-runtime-issues#deployments-and-web-app-restarts") (Probleme mit Bereitstellung oder Laufzeit). Wie im Artikel beschrieben, stellt App Service Dateien im Ordner „wwwroot“ bereit. Ihre App wird nie direkt neu gestartet.

## <a name="how-do-i-integrate-visual-studio-team-services-code-with-app-service"></a>Wie integriere ich Visual Studio Team Services-Code in App Service?

Sie haben zwei Möglichkeiten für die kontinuierliche Bereitstellung mit Visual Studio Team Services:

*   Verwenden Sie ein Git-Projekt. Stellen Sie eine Verbindung über App Service her, indem Sie die Bereitstellungsoptionen für dieses Repository verwenden.
*   Verwenden Sie ein TFVC-Projekt (Team Foundation Version Control). Führen Sie die Bereitstellung mithilfe des Build-Agents für App Service durch.

Die kontinuierliche Codebereitstellung für beide Optionen hängt von vorhandenen Entwicklerworkflows und Eincheckverfahren ab. Weitere Informationen und Beispiele finden Sie in diesen Artikeln: 

*   [Implement continuous deployment of your app to an Azure website](https://www.visualstudio.com/docs/release/examples/azure/azure-web-apps-from-build-and-release-hubs) (Implementieren der kontinuierlichen Bereitstellung Ihrer App in einer Azure-Website)
*   [Set up a Visual Studio Team Services account so it can deploy to a web app](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App) (Einrichten eines Visual Studio Team Services-Kontos, um die Bereitstellung in einer Web-App zu ermöglichen)

## <a name="how-do-i-use-ftp-or-ftps-to-deploy-my-app-to-app-service"></a>Wie verwende ich FTP oder FTPS zur Bereitstellung meiner App in App Service?

Informationen zur Verwendung von FTP oder FTPS zur Bereitstellung Ihrer Web-App in App Service finden Sie unter [Deploy your app to App Service by using FTP/S](app-service-deploy-ftp.md) (Bereitstellen Ihrer App in App Service über FTP/S).

