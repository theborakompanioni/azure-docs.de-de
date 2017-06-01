---
title: "Erstellen einer statischen HTML-Web-App in Azure in fünf Minuten | Microsoft-Dokumentation"
description: "Erfahren Sie, wie einfach die Ausführung von Web-Apps in App Service ist, indem Sie eine Beispiel-App bereitstellen."
services: app-service\web
documentationcenter: 
author: rick-anderson
manager: wpickett
editor: 
ms.assetid: 60495cc5-6963-4bf0-8174-52786d226c26
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 05/08/2017
ms.author: riande
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 895906e1ab4bc50093ed3b18f043c3dd515ca054
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017

---
# <a name="create-a-static-html-web-app-in-azure-in-five-minutes"></a>Erstellen einer statischen HTML-Web-App in Azure in fünf Minuten

In diesem Schnellstart wird erläutert, wie Sie eine einfache Website mit HTML und CSS an Azure bereitstellen. Sie führen die App mit einem [Azure App Service-Plan](https://docs.microsoft.com/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview) aus und erstellen darin eine Web-App über die [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/en-us/cli/azure/get-started-with-azure-cli). Sie verwenden Git, um die App an Azure bereitzustellen. Nachdem die erforderlichen Komponenten installiert wurden, kann das Tutorial in etwa fünf Minuten durchgeführt werden.

![hello-world-in-browser](media/app-service-web-get-started-html/hello-world-in-browser-az.png)

## <a name="prerequisites"></a>Voraussetzungen

Laden Sie vor dem Erstellen dieses Beispiels folgende Komponenten herunter und installieren Sie sie:

- [Git](https://git-scm.com/)
- [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Herunterladen des Beispiels

Klonen Sie das Beispiel-App-Repository in einem Terminalfenster auf Ihren lokalen Computer:

```bash
git clone https://github.com/Azure-Samples/html-docs-hello-world.git
```

## <a name="view-the-html"></a>Anzeigen der HTML

Navigieren Sie zum Verzeichnis mit der Beispiel-HTML. Öffnen Sie die Datei *index.html* in Ihrem Browser.

![hello-world-in-browser](media/app-service-web-get-started-html/hello-world-in-browser.png)

[!INCLUDE [login-to-azure](../../includes/login-to-azure.md)] 
[!INCLUDE [configure-deployment-user](../../includes/configure-deployment-user.md)] 

[!INCLUDE [app-service-web-quickstart1](../../includes/app-service-web-quickstart1.md)] 

Erstellen Sie eine [Web-App](app-service-web-overview.md) im App Service-Plan `quickStartPlan`. Die Web-App bietet Ihnen eine Hostingumgebung für Ihren Code sowie eine URL, unter der Sie sich die bereitgestellte App ansehen können.

[!INCLUDE [app-service-web-quickstart2](../../includes/app-service-web-quickstart2.md)] 

Die Seite wird als Azure App Service-Web-App ausgeführt:

![hello-world-in-browser](media/app-service-web-get-started-html/hello-world-in-browser-az.png)

## <a name="update-and-redeploy-the-app"></a>Aktualisieren und erneutes Bereitstellen der App

Öffnen Sie die Datei *index.html* . Nehmen Sie eine Änderung am Markup vor. Ändern Sie zum Beispiel `Hello world!` in `Hello Azure!`.

Führen Sie für Ihre Änderungen in Git einen Commit aus, und übertragen Sie dann die Codeänderungen mithilfe von Push an Azure.

```bash
git commit -am "updated HTML"
git push azure master
```

Aktualisieren Sie nach Abschluss der Bereitstellung Ihren Browser, um die Änderungen anzuzeigen.

[!INCLUDE [manage-azure-web-app](../../includes/manage-azure-web-app.md)]


[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Nächste Schritte

- Machen Sie sich mit [CLI-Skripts für Beispiel-Web-Apps](app-service-cli-samples.md) vertraut.
- Erfahren Sie, wie Sie einen [benutzerdefinierten Domänennamen](app-service-web-tutorial-custom-domain.md) wie „contoso.com“ einer [App Service-App](app-service-web-tutorial-custom-domain.md) zuordnen.
