---
title: "Bereitstellen einer WordPress-App im Azure-Portal in fünf Minuten | Microsoft-Dokumentation"
description: "Erfahren Sie, wie einfach die Ausführung von Web-Apps in App Service ist, indem Sie eine WordPress-App bereitstellen. Sie sehen sofort Ergebnisse."
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: 6feac128-c728-4491-8b79-962da9a40788
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 02/13/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: 0921b01bc930f633f39aba07b7899ad60bd6a234
ms.openlocfilehash: 7ef5802866bf96859d3f44cdb58cbb412b08a700
ms.lasthandoff: 03/01/2017


---
# <a name="deploy-a-wordpress-app-in-the-azure-portal-in-five-minutes"></a>Bereitstellen einer WordPress-App im Azure-Portal in fünf Minuten

In diesem Tutorial wird veranschaulicht, wie Sie in wenigen Minuten Ihre erste [WordPress](https://wordpress.org/)-Web-App für [Azure App Service](../app-service/app-service-value-prop-what-is.md) bereitstellen.

![WordPress-Website](./media/app-service-web-get-started-php-portal/wpdashboard.png)

## <a name="prerequisites"></a>Voraussetzungen
Sie benötigen ein Microsoft Azure-Konto. Falls Sie noch kein Konto haben, können Sie sich [für eine kostenlose Testversion registrieren](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) oder [Ihre Visual Studio-Abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

> [!NOTE]
> Zum [Testen von App Service](https://azure.microsoft.com/try/app-service/) benötigen Sie kein Azure-Konto. Sie können eine Starter-App erstellen und bis zu einer Stunde damit experimentieren – ohne Kreditkarte und ohne jegliche Verpflichtungen.
> 
> 

## <a name="deploy-the-wordpress-app"></a>Bereitstellen der WordPress-App
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.

2. Öffnen Sie [https://portal.azure.com/#create/WordPress.WordPress](https://portal.azure.com/#create/WordPress.WordPress).

    Über diesen Link gelangen Sie direkt zur Konfiguration einer neuen WordPress-App im Azure-Portal.

3. Geben Sie unter **App-Name** einen Web-App-Namen ein. Wenn der Name in der Domäne `azurewebsites.net` eindeutig ist, wird im Feld ein grünes Häkchen angezeigt.
   
5. Klicken Sie unter **Ressourcengruppe** auf **Neu erstellen**, um eine neue [Ressourcengruppe](../azure-resource-manager/resource-group-overview.md) zu erstellen, und benennen Sie sie.

6. Wählen Sie unter **Datenbankanbieter** die Option **ClearDB**.

7. Klicken Sie auf **App Service-Plan/Standort** > **Neu erstellen**. Konfigurieren Sie den [App Service-Plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) wie folgt:

    - Geben Sie unter **App Service-Plan** den gewünschten Namen ein.
    - Wählen Sie unter **Standort** einen Hostingstandort für Ihren Plan aus.
    - Klicken Sie auf **Tarif**, wählen Sie **F1 Free** oder einen anderen passenden Tarif aus, und klicken Sie anschließend auf **Auswählen**.
    - Klicken Sie auf **OK**.

8. Klicken Sie auf **Datenbank** > **Neu erstellen**. Konfigurieren Sie die SQL-Datenbank wie folgt:

    - Geben Sie unter **Datenbankname** einen Namen für die Datenbank ein. 
    - Wählen Sie unter **Standort** den gleichen Standort wie für den App Service-Plan aus.
    - Klicken Sie auf **Tarif**, wählen Sie **Mercury** oder einen anderen passenden Tarif aus, und klicken Sie anschließend auf **Auswählen**.
    - Klicken Sie auf **Rechtliche Bedingungen** und dann auf **Kaufen**.
    - Klicken Sie auf **OK**.

9. Klicken Sie auf **Erstellen**.

    Daraufhin erstellt Azure Ihre WordPress-App gemäß Ihrer Konfiguration. Oben rechts im Browserfenster sollte die Benachrichtigung **Bereitstellung gestartet...** angezeigt werden.

    ![Bereitstellung gestartet – erste WordPress-App in Azure App Service](./media/app-service-web-get-started-php-portal/deployment-started.png)
   
## <a name="launch-and-manage-your-wordpress-web-app"></a>Starten und Verwalten Ihrer WordPress-Web-App

Nach Abschluss der App-Bereitstellung erscheint eine weitere Benachrichtigung.

![Bereitstellung erfolgreich – erste WordPress-App in Azure App Service](./media/app-service-web-get-started-php-portal/deployment-succeeded.png)

1. Klicken Sie auf die Benachrichtigung. Sollten Sie die Benachrichtigung verpassen, können Sie auch auf die Benachrichtigungsglocke (![Benachrichtigungsglocke – erste WordPress-App in Azure App Service](./media/app-service-web-get-started-dotnet-portal/notification.png)) klicken, um darauf zuzugreifen.

    Nun sollte das [Blatt](../azure-resource-manager/resource-group-portal.md#manage-resources) für die Verwaltung Ihrer Web-App angezeigt werden. (*Blatt:* Eine horizontal geöffnete Portalseite.)

3. Klicken Sie im oberen Bereich der Übersichtsseite auf **Durchsuchen**.
   
    ![Durchsuchen – erste WordPress-App in Azure App Service](./media/app-service-web-get-started-php-portal/browse.png)

    Die Seite **Willkommen** für die WordPress-App wird angezeigt. Konfigurieren Sie die WordPress-Installation, und experimentieren Sie mit der App.

    ![WordPress-Konfiguration – erste WordPress-App in Azure App Service](./media/app-service-web-get-started-php-portal/wordpress-config.png)
    
## <a name="next-steps"></a>Nächste Schritte
* [Erstellen, Konfigurieren und Bereitstellen einer Laravel-Web-App in Azure](app-service-web-php-get-started.md): Es werden die grundlegenden Fähigkeiten beschrieben, die Sie zum Ausführen von PHP-Web-Apps in Azure benötigen, z.B.:

    * Erstellen und Konfigurieren von Apps in Azure über PowerShell/Bash
    * Festlegen der PHP-Version
    * Verwenden einer Startdatei, die sich nicht im Stammverzeichnis der Anwendung befindet
    * Aktivieren der Composer-Automatisierung
    * Zugreifen auf umgebungsspezifische Variablen
    * Problembehandlung für allgemeine Fehler

* [Bereitstellen der App in Azure App Service:](web-sites-deploy.md) Hier erfahren Sie, wie Sie Ihre App über FTP oder über Repositorys zur Quellcodeverwaltung bereitstellen.
* [Hinzufügen von Funktionen zu Ihrer ersten Web-App:](app-service-web-get-started-2.md) Hier erfahren Sie, wie Sie Ihre Azure-App weiterentwickeln. Authentifizieren Sie Ihre Benutzer. Skalieren Sie die App je nach Bedarf. Richten Sie einige Leistungswarnungen ein. Es sind jeweils nur wenige Klicks erforderlich.

