---
title: "Erstellen Ihrer ersten ASP.NET-Web-App in Azure in fünf Minuten | Microsoft-Dokumentation"
description: "Hier erfahren Sie anhand der Bereitstellung einer einfachen ASP.NET-App, wie einfach sich Web-Apps in App Service ausführen lassen."
services: app-service\web
documentationcenter: 
author: cephalin
manager: wpickett
editor: 
ms.assetid: b1e6bd58-48d1-4007-9d6c-53fd6db061e3
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 03/27/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: db7cb109a0131beee9beae4958232e1ec5a1d730
ms.openlocfilehash: 24ddf0d81e52960bd582861d324585b8ba0ca357
ms.lasthandoff: 04/18/2017


---
# <a name="create-your-first-aspnet-web-app-in-azure-in-five-minutes"></a>Erstellen Ihrer ersten ASP.NET-Web-App in Azure in fünf Minuten

[!INCLUDE [app-service-web-selector-get-started](../../includes/app-service-web-selector-get-started.md)] 

Dieser Schnellstart hilft Ihnen bei der Bereitstellung Ihrer ersten ASP.NET-Web-App in [Azure App Service](../app-service/app-service-value-prop-what-is.md) in nur wenigen Minuten. Am Ende des Tutorials verfügen Sie über eine einfache Web-App, die in der Cloud ausgeführt wird.

![ASP.NET-Web-App in Azure App Service](./media/app-service-web-get-started-dotnet/updated-azure-web-app.png)

## <a name="before-you-begin"></a>Voraussetzungen

Dieses Tutorial veranschaulicht, wie Sie mithilfe von Visual Studio 2017 eine ASP.NET-Web-App erstellen und für Azure bereitstellen. Falls Sie Visual Studio 2017 noch nicht installiert haben, können Sie die **kostenlose** [Visual Studio 2017 Community-Edition](https://www.visualstudio.com/downloads/) herunterladen und verwenden. Aktivieren Sie beim Setup von Visual Studio die Option **Azure-Entwicklung**.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-an-aspnet-web-app"></a>Erstellen einer ASP.NET-Web-App

Erstellen Sie in Visual Studio durch Drücken von `Ctrl`+`Shift`+`N` ein neues Projekt.

Klicken Sie im Dialogfeld **Neues Projekt** auf **Visual C# > Web > ASP.NET-Webanwendung (.NET Framework)**.

Nennen Sie die Anwendung **myFirstAzureWebApp**, und klicken Sie anschließend auf **OK**.
   
![Dialogfeld "Neues Projekt"](./media/app-service-web-get-started-dotnet/new-project.png)

Sie können jede Art von ASP.NET Web-App für Azure bereitstellen. Wählen Sie in diesem Tutorial die Vorlage **MVC** aus, und vergewissern Sie sich, dass die Authentifizierung auf **Keine Authentifizierung** festgelegt ist.
      
Klicken Sie auf **OK**.

![Dialogfeld "Neues ASP.NET-Projekt"](./media/app-service-web-get-started-dotnet/select-mvc-template.png)

## <a name="publish-to-azure"></a>Veröffentlichen in Azure

Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **myFirstAzureWebApp**, und wählen Sie **Veröffentlichen** aus.

![Veröffentlichen über den Projektmappen-Explorer](./media/app-service-web-get-started-dotnet/solution-explorer-publish.png)

Vergewissern Sie sich, dass **Microsoft Azure App Service** ausgewählt ist, und klicken Sie auf **Veröffentlichen**.

![Veröffentlichen über die Projektübersichtsseite](./media/app-service-web-get-started-dotnet/publish-to-app-service.png)

Daraufhin öffnet sich das Dialogfeld **App Service erstellen**, das Sie beim Erstellen sämtlicher Azure-Ressourcen unterstützt, die benötigt werden, um Ihre ASP.NET Web-App in Azure auszuführen.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Klicken Sie im Dialogfeld **App Service erstellen** auf **Konto hinzufügen**, und melden Sie sich bei Ihrem Azure-Abonnement an. Falls Sie bereits bei einem Microsoft-Konto angemeldet sind, vergewissern Sie sich, dass dieses Konto Ihr Azure-Abonnement enthält. Wenn das Microsoft-Konto, bei dem Sie angemeldet sind, nicht Ihr Azure-Abonnement enthält, klicken Sie darauf, um das korrekte Konto hinzuzufügen.
   
![Anmelden bei Azure](./media/app-service-web-get-started-dotnet/sign-in-azure.png)

Nach der Anmeldung können Sie in diesem Dialogfeld sämtliche Ressourcen erstellen, die Sie für Ihre Azure-Web-App benötigen.

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Als erstes benötigen Sie eine _Ressourcengruppe_. 

> [!NOTE] 
> Eine Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen wie Web-Apps, Datenbanken und Speicherkonten bereitgestellt und verwaltet werden.
>
>

Klicken Sie neben **Ressourcengruppe** auf **Neu**.

Nennen Sie Ihre Ressourcengruppe **myResourceGroup**, und klicken Sie anschließend auf **OK**.

## <a name="create-an-app-service-plan"></a>Wie erstelle ich einen Plan?

Ihre Azure Web-App benötigt außerdem einen _App Service-Plan_. 

> [!NOTE]
> Ein App Service-Plan stellt die Sammlung physischer Ressourcen dar, die zum Hosten Ihrer Apps verwendet werden. Alle einem App Service-Plan zugewiesenen Apps teilen sich die durch den Plan definierten Ressourcen. Das spart Kosten, wenn Sie mehrere Apps hosten. 
>
> In App Service-Plänen wird Folgendes definiert:
>
> - Region („Europa, Norden“, „USA, Osten“, „Asien, Südosten“)
> - Instanzgröße (klein, mittel, groß)
> - Skalierung (Instanzenanzahl) 
> - SKU (Free, Shared, Basic, Standard, Premium)
>
>

Klicken Sie neben **App Service-Plan** auf **Neu**. 

Konfigurieren Sie den neuen App Service-Plan im Dialogfeld **App Service-Plan konfigurieren** mit den folgenden Einstellungen:

- **App Service-Plan**: Geben Sie **myAppServicePlan** ein. 
- **Standort**: Wählen Sie **Europa, Westen** oder eine beliebige andere Region aus.
- **Größe**: Wählen Sie **Free** oder einen beliebigen anderen [Tarif](https://azure.microsoft.com/pricing/details/app-service/) aus.

Klicken Sie auf **OK**.

![Erstellen eines neuen App Service-Plans](./media/app-service-web-get-started-dotnet/configure-app-service-plan.png)

## <a name="create-and-publish-the-web-app"></a>Erstellen und Veröffentlichen der Web-App

Nun muss die Web-App nur noch benannt werden. Geben Sie unter **Web-App-Name** einen eindeutigen Web-App-Namen ein. Da dieser Name als Teil des DNS-Standardnamens für Ihre App (`<app_name>.azurewebsites.net`) verwendet wird, muss er für alle Apps in Azure eindeutig sein. Sie können Ihrer App später einen benutzerdefinierten Domänennamen zuordnen, bevor Sie sie für Ihre Benutzer verfügbar machen.

Sie können auch den automatisch generierten Namen übernehmen. Dieser ist bereits eindeutig.

Klicken Sie auf **Erstellen**, um mit der Erstellung der Azure-Ressourcen zu beginnen.

![Konfigurieren des Web-App-Namens](./media/app-service-web-get-started-dotnet/web-app-name.png)

Nachdem der Assistent die Azure-Ressourcen erstellt hat, veröffentlicht er Ihre ASP.NET-Web-App erstmals für Azure und startet die veröffentlichte Azure-Web-App anschließend in Ihrem Standardbrowser.

![Veröffentlichte ASP.NET-Web-App in Azure](./media/app-service-web-get-started-dotnet/published-azure-web-app.png)

In der URL wird der von Ihnen angegebene Web-App-Name im Format `http://<app_name>.azurewebsites.net` verwendet. 

Herzlichen Glückwunsch, Ihre erste ASP.NET-Web-App wird live in Azure App Service ausgeführt.

## <a name="update-the-app-and-redeploy"></a>Aktualisieren der App und erneutes Bereitstellen

Ein Update lässt sich ganz einfach erneut für Azure bereitstellen. Zur Veranschaulichung führen wir eine Aktualisierung der Startseite durch.

Öffnen Sie im **Projektmappen-Explorer** Folgendes: **Views\Home\Index.cshtml**.

Suchen Sie im oberen Bereich nach dem HTML-Tag `<div class="jumbotron">`, und ersetzen Sie das gesamte Tag durch folgenden Code:

```HTML
<div class="jumbotron">
    <h1>ASP.NET in Azure!</h1>
    <p class="lead">This is a simple app that we’ve built that demonstrates how to deploy a .NET app to Azure App Service.</p>
</div>
```

Klicken Sie zur erneuten Bereitstellung für Azure im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **myFirstAzureWebApp**, und wählen Sie **Veröffentlichen** aus.

Klicken Sie auf der Veröffentlichungsseite auf **Veröffentlichen**.

Nach Abschluss des Vorgangs startet Visual Studio die aktualisierte Azure-Web-App in Ihrem Browser.

![Aktualisierte ASP.NET-Web-App in Azure](./media/app-service-web-get-started-dotnet/updated-azure-web-app.png)

## <a name="manage-your-new-azure-web-app"></a>Verwalten Ihrer neuen Azure-Web-App

Sehen Sie sich die soeben erstellte Web-App im Azure-Portal an. 

Melden Sie sich hierzu bei [https://portal.azure.com](https://portal.azure.com) an.

Klicken Sie im linken Menü auf **App Services** und anschließend auf den Namen Ihrer Azure-Web-App.

![Portalnavigation zur Azure-Web-App](./media/app-service-web-get-started-dotnet/access-portal.png)

Dadurch gelangen Sie auf das _Blatt_ Ihrer Web-App (eine Portalseite, die horizontal geöffnet wird). 

Auf dem Blatt Ihrer Web-App wird standardmäßig die Seite **Übersicht** angezeigt. Diese Seite bietet einen Überblick über den Status Ihrer App. Hier können Sie auch einfache Verwaltungsaufgaben wie Durchsuchen, Beenden, Neustarten und Löschen durchführen. Die Registerkarten auf der linken Seite des Blatts zeigen die verschiedenen Konfigurationsseiten, die Sie öffnen können. 

![App Service-Blatt im Azure-Portal](./media/app-service-web-get-started-dotnet/web-app-blade.png)

Die Registerkarten auf dem Blatt zeigen die vielen tollen Features, mit denen Sie Ihre Web-App ausstatten können. Ein paar Beispiele:

- Zuordnen eines benutzerdefinierten DNS-Namens
- Binden eines benutzerdefinierten SSL-Zertifikats
- Konfigurieren von Continuous Deployment
- Zentrales und horizontales Hochskalieren
- Hinzufügen einer Benutzerauthentifizierung

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Zum Löschen Ihrer ersten Azure-Web-App können Sie auf der Seite **Übersicht** auf **Löschen** klicken. Es gibt jedoch noch eine bessere Möglichkeit, um alles zu löschen, was Sie im Rahmen dieses Schnellstarttutorials erstellt haben. Klicken Sie auf der Web-App-Seite **Übersicht** auf die Ressourcengruppe, um das entsprechende Blatt zu öffnen. 

![Zugreifen auf die Ressourcengruppe über das Blatt „App Service“](./media/app-service-web-get-started-dotnet/access-resource-group.png)

Auf dem Ressourcengruppenblatt finden Sie sowohl den App Service-Plan als auch die App Service-App, die Visual Studio für Sie erstellt hat. 

Klicken Sie im oberen Bereich des Blatts auf **Löschen**. 

<!--![Delete resource group in Azure portal](./media/app-service-web-get-started-dotnet/delete-resource-group.png)-->

Geben Sie zur Bestätigung auf dem Bestätigungsblatt den Namen der Ressourcengruppe **myResourceGroup** in das Textfeld ein, und klicken Sie auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

Machen Sie sich mit vorgefertigten [Azure PowerShell-Beispielen](app-service-powershell-samples.md) vertraut.

