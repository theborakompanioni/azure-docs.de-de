---
title: Erstellen einer Wordpress-Web-App in Azure App Service | Microsoft Docs
description: Erfahren Sie, wie Sie über das Azure-Portal eine neue Azure-Web-App für einen WordPress-Blog erstellen.
services: app-service\web
documentationcenter: php
author: rmcmurray
manager: wpickett
editor: ''

ms.service: app-service-web
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: hero-article
ms.date: 08/11/2016
ms.author: robmcm

---
# Erstellen einer WordPress-Web-App in Azure App Service
[!INCLUDE [Registerkarten](../../includes/app-service-web-get-started-nav-tabs.md)]

Dieses Lernprogramm zeigt, wie eine WordPress-Blog-Website aus dem Azure Marketplace bereitgestellt wird.

Wenn Sie das Tutorial beendet haben, verfügen Sie über eine eigene WordPress-Blogwebsite, die in der Cloud ausgeführt wird.

![WordPress-Website](./media/web-sites-php-web-site-gallery/wpdashboard.png)

Sie lernen Folgendes:

* Suchen einer Anwendungsvorlage im Azure Marketplace
* Erstellen einer Web-App in Azure App Service basierend auf der Vorlage
* Konfigurieren der Azure App Service-Einstellungen für die neue Web-App und die Datenbank

Azure Marketplace bietet eine breite Auswahl an beliebten Web-Apps, die von Microsoft, anderen Unternehmen oder Open Source-Softwareinitiativen entwickelt wurden. Die Web-Apps basieren auf einer Vielzahl beliebter Frameworks, wie z. B. [PHP](/develop/nodejs/) (in diesem WordPress-Beispiel), [.NET](/develop/net/), [Node.js](/develop/nodejs/), [Java](/develop/java/) und [Python](/develop/python/), um nur einige zu nennen. Die einzige Software, die Sie zum Erstellen der Web-App über den Azure Marketplace benötigen, ist der Browser, den Sie für das [Azure-Portal](https://portal.azure.com/) verwenden.

Die WordPress-Website, die Sie in diesem Lernprogramm bereitstellen, verwendet MySQL als Datenbank. Wenn Sie stattdessen die SQL-Datenbank verwenden möchten, finden Sie unter [Project Nami](http://projectnami.org/) entsprechende Informationen. **Project Nami** ist auch über den Marketplace vorhanden.

> [!NOTE]
> Sie benötigen ein Microsoft Azure-Konto, um dieses Tutorial auszuführen. Wenn Sie kein Konto haben, können Sie [Ihre Visual Studio-Abonnentenvorteile aktivieren](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) oder sich [für eine kostenlose Testversion registrieren](/de-DE/pricing/free-trial/?WT.mc_id=A261C142F).
> 
> Wenn Sie Azure App Service ausprobieren möchten, ehe Sie sich für ein Azure-Konto anmelden, besuchen Sie [Azure App Service-App erstellen](http://go.microsoft.com/fwlink/?LinkId=523751). Dort können Sie direkt eine kurzzeitige Start-Web-App in App Service erstellen – keine Kreditkarte erforderlich, keine weiteren Verpflichtungen.
> 
> 

## Auswählen von WordPress und Konfigurieren für Azure App Service
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
2. Klicken Sie auf **Neu**.
   
    ![Create New][5]
3. Suchen Sie nach **WordPress**, und klicken Sie dann auf **WordPress**. Wenn Sie die SQL-Datenbank statt MySQL verwenden möchten, suchen Sie nach **Project Nami**.
   
    ![WordPress-Symbol in der Liste][7]
4. Lesen Sie die Beschreibung der WordPress-App, und klicken Sie dann auf **Erstellen**.
   
    ![Erstellen](./media/web-sites-php-web-site-gallery/create.png)
5. Geben Sie im Feld **Web-App** einen Namen für die Web-App ein.
   
    Der Name muss innerhalb der Domäne "azurewebsites.net" eindeutig sein, da die URL der Web-App "{name}.azurewebsites.net" lauten wird. Wenn der eingegebene Name nicht eindeutig ist, wird im Textfeld ein rotes Ausrufezeichen angezeigt.
6. Falls Sie über mehrere Abonnements verfügen, wählen Sie das gewünschte Abonnement aus.
7. Wählen Sie eine vorhandene **Ressourcengruppe** aus, oder erstellen Sie eine neue.
   
    Weitere Informationen zu Ressourcengruppen finden Sie unter [Übersicht über den Azure Resource Manager](../resource-group-overview.md).
8. Wählen Sie einen **App Service-Plan/-Standort** aus, oder erstellen Sie einen neuen Plan und Standort.
   
    Weitere Informationen zu App Service-Plänen finden Sie unter [Azure App Service-Pläne – Detaillierte Übersicht](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
9. Klicken Sie auf **Datenbank**, und geben Sie auf dem Blatt **Neue MySQL-Datenbank** die erforderlichen Werte für die Konfiguration Ihrer MySQL-Datenbank ein.
   
    a. Geben Sie einen neuen Namen ein, oder übernehmen Sie den Standardnamen.
   
    b. Belassen Sie den **Datenbanktyp** bei **Freigegeben**.
   
    c. Wählen Sie den gleichen Standort wie für die Web-App.
   
    d. Wählen Sie einen Tarif aus. "Mercury" (kostenlos mit minimaler Anzahl von Verbindungen und geringem Datenträgerspeicherplatz) ist für die Zwecke dieses Tutorials ausreichend.
10. Klicken Sie auf dem Blatt **Neue MySQL-Datenbank** auf **OK**.
11. Akzeptieren Sie auf dem Blatt **WordPress** die rechtlichen Hinweise, und klicken Sie anschließend auf **Erstellen**.
    
     ![Web-App konfigurieren](./media/web-sites-php-web-site-gallery/configure.png)
    
     Azure App Service erstellt die Web-App üblicherweise in weniger als einer Minute. Sie können den Fortschritt verfolgen, indem Sie oben auf der Portalseite auf das Glockensymbol klicken.
    
     ![Statusanzeige](./media/web-sites-php-web-site-gallery/progress.png)

## Starten und Verwalten Ihrer WordPress-Web-App
1. Wenn die Erstellung der Web-App abgeschlossen ist, navigieren Sie im Azure-Portal zu der Ressourcengruppe, in der Sie die Anwendung erstellt haben. Dort werden die Web-App und die Datenbank angezeigt.
   
    Bei der zusätzlichen Ressource mit dem Glühbirnensymbol handelt es sich um den Dienst [Application Insights](/services/application-insights/), der Überwachungsdienste für Ihre Web-App bereitstellt.
2. Klicken Sie auf dem Blatt **Ressourcengruppe** auf die Zeile der Web-App.
   
    ![Web-App konfigurieren](./media/web-sites-php-web-site-gallery/resourcegroup.png)
3. Klicken Sie auf dem Blatt der Web-App auf **Durchsuchen**.
   
    ![Website-URL][browse]
4. Geben Sie auf der WordPress-Seite **Willkommen** die Konfigurationsinformationen ein, die von WordPress benötigt werden, und klicken Sie dann auf **WordPress installieren**.
   
    ![WordPress konfigurieren](./media/web-sites-php-web-site-gallery/wpconfigure.png)
5. Melden Sie sich auf der Seite **Willkommen** mit den erstellten Anmeldeinformationen an.
6. Das Website-Dashboard wird geöffnet.
   
    ![WordPress-Website](./media/web-sites-php-web-site-gallery/wpdashboard.png)

## Nächste Schritte
Sie haben gesehen, wie Sie eine PHP-Web-App über den Katalog erstellen und bereitstellen. Weitere Informationen zum Verwenden von PHP in Azure finden Sie im [PHP Developer Center](/develop/php/).

Zusätzliche Dokumentation zur Arbeit mit App Service-Web-Apps finden Sie in den Links auf der linken Seite der Seite (bei breiten Browserfenstern) oder am oberen Rand der Seite (bei schmalen Browserfenstern).

## Änderungen
* Hinweise zu den Änderungen in App Service im Vergleich zu Websites finden Sie unter [Azure App Service und vorhandene Azure-Dienste](http://go.microsoft.com/fwlink/?LinkId=529714).

[5]: ./media/web-sites-php-web-site-gallery/startmarketplace.png
[7]: ./media/web-sites-php-web-site-gallery/search-web-app.png
[browse]: ./media/web-sites-php-web-site-gallery/browse-web.png

<!---HONumber=AcomDC_0817_2016-->