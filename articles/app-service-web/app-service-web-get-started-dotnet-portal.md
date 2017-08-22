---
title: "Bereitstellen einer Umbraco-Web-App im Azure-Portal in fünf Minuten | Microsoft-Dokumentation"
description: "Erfahren Sie, wie einfach die Ausführung von Web-Apps in App Service ist, indem Sie eine ASP.NET-Beispiel-App bereitstellen. Sehen Sie sofort Ergebnisse."
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: b1e6bd58-48d1-4007-9d6c-53fd6db061e3
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2017
ms.author: cephalin
ms.translationtype: Human Translation
ms.sourcegitcommit: 6ea03adaabc1cd9e62aa91d4237481d8330704a1
ms.openlocfilehash: 6b1dede903083d1771733330a069b6ab533d9f00
ms.contentlocale: de-de
ms.lasthandoff: 04/06/2017

---
# <a name="deploy-an-umbraco-web-app-in-the-azure-portal-in-five-minutes"></a>Bereitstellen einer Umbraco-Web-App im Azure-Portal in fünf Minuten

In diesem Tutorial erfahren Sie, wie Sie innerhalb weniger Minuten eine einfache [Umbraco](https://our.umbraco.org/)-Web-App für [Azure App Service](../app-service/app-service-value-prop-what-is.md) bereitstellen.

![Umbraco-App](./media/app-service-web-get-started-dotnet-portal/defaultpage.png)

## <a name="prerequisites"></a>Voraussetzungen
Sie benötigen ein Microsoft Azure-Konto. Falls Sie noch kein Konto haben, können Sie sich [für eine kostenlose Testversion registrieren](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) oder [Ihre Visual Studio-Abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

> [!NOTE]
> Zum [Testen von App Service](https://azure.microsoft.com/try/app-service/) benötigen Sie kein Azure-Konto. Sie können eine Starter-App erstellen und bis zu einer Stunde damit experimentieren – ohne Kreditkarte und ohne jegliche Verpflichtungen.
> 
> 

## <a name="deploy-the-aspnet-app"></a>Bereitstellen der ASP.NET-App
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.

2. Öffnen Sie [https://portal.azure.com/#create/umbracoorg.UmbracoCMS](https://portal.azure.com/#create/umbracoorg.UmbracoCMS).

    Über diesen Link gelangen Sie direkt zur Konfiguration einer neuen Umbraco-App im Azure-Portal.

3. Geben Sie unter **App-Name** einen Web-App-Namen ein. Wenn der Name in der Domäne `azurewebsites.net` eindeutig ist, wird im Feld ein grünes Häkchen angezeigt.
   
5. Klicken Sie unter **Ressourcengruppe** auf **Neu erstellen**, um eine neue [Ressourcengruppe](../azure-resource-manager/resource-group-overview.md) zu erstellen, und benennen Sie sie.

7. Klicken Sie auf **App Service-Plan/Standort** > **Neu erstellen**. Konfigurieren Sie den [App Service-Plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) wie folgt:

    - Geben Sie unter **App Service-Plan** den gewünschten Namen ein.
    - Wählen Sie unter **Standort** einen Hostingstandort für Ihren Plan aus.
    - Klicken Sie auf **Tarif**, wählen Sie **F1 Free** oder einen anderen passenden Tarif aus, und klicken Sie anschließend auf **Auswählen**.
    - Klicken Sie auf **OK**.

    Ihre Umbraco-CMS-Konfiguration sollte nun wie folgt aussehen:

    ![Konfigurationsphase – erste Umbraco-App in Azure App Service](./media/app-service-web-get-started-dotnet-portal/configure-in-progress.png)

12. Klicken Sie auf **SQL-Datenbank** > **Neue Datenbank erstellen**. Konfigurieren Sie die SQL-Datenbank wie folgt:

    - Geben Sie unter **Name** einen Namen ein (beispielsweise **myDB**).
    - Klicken Sie auf **Tarif**, wählen Sie **F1 Free** oder einen anderen passenden Tarif aus, und klicken Sie anschließend auf **Auswählen**.
    - Klicken Sie auf **Zielserver** > **Neuen Server erstellen**. Konfigurieren Sie den Datenbankserver wie folgt:

        - Geben Sie unter **Servername** einen Servernamen ein. Wenn der Name in der Domäne `.database.windows.net` eindeutig ist, wird im Feld ein grünes Häkchen angezeigt.
        - Geben Sie unter **Serveradministratoranmeldung** den gewünschten Administratorbenutzernamen ein.
        - Geben Sie unter **Kennwort** und **Kennwort bestätigen** das gewünschte Kennwort ein.
        - Wählen Sie als Standort den gleichen Standort aus, den Sie auch für die Web-App verwenden.
        - Achten Sie darauf, dass **Azure-Diensten Zugriff auf den Server erlauben** aktiviert ist.
        - Klicken Sie auf **Auswählen**.
    
    - Klicken Sie auf **Auswählen**.

13. Klicken Sie auf **Web-App-Einstellungen**, geben Sie den Benutzernamen und das Kennwort für die Datenbank an, und klicken Sie auf **OK**.

    Ihre Umbraco-CMS-Konfiguration sollte nun wie folgt aussehen:

    ![Konfiguration abgeschlossen – erste Umbraco-App in Azure App Service](./media/app-service-web-get-started-dotnet-portal/configure-complete.png)

14. Klicken Sie auf **Erstellen**.
    
    Daraufhin erstellt Azure Ihre Umbraco-App gemäß Ihrer Konfiguration. Rechts oben erscheint die Benachrichtigung **Bereitstellung gestartet...**.

    ![Bereitstellung erfolgreich – erste Umbraco-App in Azure App Service](./media/app-service-web-get-started-dotnet-portal/deployment-started.png)
   
## <a name="launch-and-manage-your-umrbaco-web-app"></a>Starten und Verwalten der Umbraco-Web-App

Nach Abschluss der App-Bereitstellung erscheint eine weitere Benachrichtigung.

![Bereitstellung erfolgreich – erste Umbraco-App in Azure App Service](./media/app-service-web-get-started-dotnet-portal/deployment-succeeded.png)

1. Klicken Sie auf die Benachrichtigung. Sollten Sie die Benachrichtigung verpassen, können Sie auch auf die Benachrichtigungsglocke (![Benachrichtigungsglocke – erste Umbraco-App in Azure App Service](./media/app-service-web-get-started-dotnet-portal/notification.png)) klicken, um darauf zuzugreifen.

    Nun sollte das [Blatt](../azure-resource-manager/resource-group-portal.md#manage-resources) für die Verwaltung Ihrer Web-App angezeigt werden. (*Blatt:* Eine horizontal geöffnete Portalseite.)

3. Klicken Sie im oberen Bereich der Übersichtsseite auf **Durchsuchen**.
   
    ![Durchsuchen – erste Umbraco-App in Azure App Service](./media/app-service-web-get-started-dotnet-portal/browse.png)

    Nun wird die **Willkommensseite** von Umbraco angezeigt. Konfigurieren Sie die Umbraco-Installation, und experimentieren Sie mit der App.

    ![Umbraco-Konfiguration – erste Umbraco-App in Azure App Service](./media/app-service-web-get-started-dotnet-portal/umbraco-config.png)
    
## <a name="next-steps"></a>Nächste Schritte
* [Bereitstellen einer ASP.NET-Web-App für Azure App Service mit Visual Studio:](app-service-web-get-started-dotnet.md) Hier erfahren Sie, wie Sie unter Verwendung einer der enthaltenen Anwendungsvorlagen eine neue Azure-Web App über Visual Studio erstellen.
* [Bereitstellen der App in Azure App Service:](web-sites-deploy.md) Hier erfahren Sie, wie Sie Ihre App über FTP oder über Repositorys zur Quellcodeverwaltung bereitstellen.
* [Hinzufügen von Funktionen zu Ihrer ersten Web-App:](app-service-web-get-started-2.md) Hier erfahren Sie, wie Sie Ihre Azure-App weiterentwickeln. Authentifizieren Sie Ihre Benutzer. Skalieren Sie die App je nach Bedarf. Richten Sie einige Leistungswarnungen ein. Es sind jeweils nur wenige Klicks erforderlich.

