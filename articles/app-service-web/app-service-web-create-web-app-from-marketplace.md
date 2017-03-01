---
title: Erstellen einer Web-App aus dem Azure Marketplace | Microsoft Docs
description: Hier erfahren Sie, wie Sie unter Verwendung des Azure-Portals eine neue WordPress-Web-App aus dem Azure Marketplace erstellen.
services: app-service\web
documentationcenter: 
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 972a296d-f927-470b-8534-0f2cb9eac223
ms.service: app-service-web
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/22/2016
ms.author: robmcm
translationtype: Human Translation
ms.sourcegitcommit: b1a633a86bd1b5997d5cbf66b16ec351f1043901
ms.openlocfilehash: a04c7129cd2e16c129f3e4b8e8e40f76ff37114d
ms.lasthandoff: 01/20/2017


---
# <a name="create-a-web-app-from-the-azure-marketplace"></a>Erstellen einer Web-App aus dem Azure Marketplace
<!-- Note: This article replaces web-sites-php-web-site-gallery.md -->

[!INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Azure Marketplace bietet eine breite Auswahl an beliebten Web-Apps, die von Microsoft, anderen Unternehmen oder Open Source-Softwareinitiativen entwickelt wurden. Beispiele wären etwa WordPress, Umbraco CMS und Drupal. Diese Web-Apps basieren auf einer Vielzahl beliebter Frameworks wie [PHP] (in diesem WordPress-Beispiel), [.NET], [Node.js], [Java] und [Python], um nur einige zu nennen. Die einzige Software, die Sie zum Erstellen der Web-App über den Azure Marketplace benötigen, ist der Browser, den Sie für das [Azure-Portal] verwenden.

In diesem Tutorial lernen Sie Folgendes:

* Suchen und Erstellen einer Web-App in Azure App Service, die auf einer Azure Marketplace-Vorlage basiert
* Konfigurieren der Azure App Service-Einstellungen für die neue Web-App
* Starten und Verwalten der Web-App

Im Rahmen dieses Tutorials stellen Sie eine WordPress-Blog-Website aus dem Azure Marketplace bereit. Nach Abschluss dieses Tutorials verfügen Sie über eine eigene, in der Cloud ausgeführte WordPress-Website.

![Dashboard für WordPress-Beispiel-Web-App][WordPressDashboard1]

Die in diesem Tutorial bereitgestellte WordPress-Website verwendet MySQL als Datenbank. Wenn Sie stattdessen die SQL-Datenbank verwenden möchten, navigieren Sie zu [Project Nami](ebenfalls über den Azure Marketplace erhältlich).

> [!NOTE]
> Sie benötigen ein Microsoft Azure-Konto, um dieses Tutorial auszuführen. Wenn Sie kein Konto haben, können Sie Ihre [Visual Studio-Abonnentenvorteile aktivieren][activate] oder [sich für eine kostenlose Testversion registrieren][free trial].
> 
> Wenn Sie Azure App Service ausprobieren möchten, ehe Sie sich für ein Azure-Konto anmelden, besuchen Sie [Azure App Service-App erstellen]. Dort können Sie ohne Angabe von Kreditkartendaten oder anderen Verpflichtungen umgehend eine kurzzeitige Start-Web-App in App Service erstellen.
> 
> 

## <a name="find-and-create-a-web-app-in-azure-app-service"></a>Suchen und Erstellen einer Web-App in Azure App Service
1. Melden Sie sich beim [Azure-Portal]an.
2. Klicken Sie auf **Neu**.
   
    ![Erstellen einer neuen Azure-Ressource][MarketplaceStart]
3. Suchen Sie nach **WordPress**, und klicken Sie dann auf **WordPress**. (Wenn Sie anstelle von MySQL die SQL-Datenbank verwenden möchten, suchen Sie nach **Project Nami**.)
   
    ![Suchen nach WordPress im Marketplace][MarketplaceSearch]
4. Lesen Sie die Beschreibung der WordPress-App, und klicken Sie dann auf **Erstellen**.
   
    ![Erstellen einer WordPress-Web-App][MarketplaceCreate]

## <a name="configure-azure-app-service-settings-for-your-new-web-app"></a>Konfigurieren von Azure App Service-Einstellungen für Ihre neue Web-App
1. Nachdem Sie eine neue Web-App erstellt haben, wird das Blatt mit den WordPress-Einstellungen angezeigt. Führen Sie auf diesem Blatt folgende Schritte aus:
   
    ![Konfigurieren der Einstellungen für die WordPress-Web-App][ConfigStart]
2. Geben Sie im Feld **Web-App** einen Namen für die Web-App ein.
   
    Der Name muss innerhalb der Domäne „azurewebsites.net“ eindeutig sein, da die URL der Web-App „ *{name}*.azurewebsites.net“ lauten wird. Wenn der eingegebene Name nicht eindeutig ist, wird im Textfeld ein rotes Ausrufezeichen angezeigt.
   
    ![Konfigurieren des Namens der WordPress-Web-App][ConfigAppName]
3. Wenn Sie über mehr als ein Abonnement verfügen, wählen Sie das Abonnement aus, das Sie verwenden möchten.
   
    ![Konfigurieren des Abonnements für die Web-App][ConfigSubscription]
4. Wählen Sie eine **Ressourcengruppe** aus, oder erstellen Sie eine neue.
   
    Weitere Informationen zu Ressourcengruppen finden Sie unter [Azure Resource Manager – Übersicht][ResourceGroups].
   
    ![Konfigurieren der Ressourcengruppe für die Web-App][ConfigResourceGroup]
5. Wählen Sie einen **App Service-Plan/-Standort** aus, oder erstellen Sie einen neuen Plan und Standort.
   
    Weitere Informationen zu App Service-Plänen finden Sie unter [Azure App Service-Pläne – Detaillierte Übersicht][AzureAppServicePlans].
   
    ![Konfigurieren des Serviceplans für die Web-App][ConfigServicePlan]
6. Klicken Sie auf **Datenbank**, und geben Sie auf dem Blatt **Neue MySQL-Datenbank** die erforderlichen Werte für die Konfiguration Ihrer MySQL-Datenbank ein.
   
    a. Geben Sie einen neuen Namen ein, oder übernehmen Sie den Standardnamen.
   
    b. Belassen Sie den **Datenbanktyp** bei **Freigegeben**.
   
    c. Wählen Sie den gleichen Standort wie für die Web-App.
   
    d. Wählen Sie einen Tarif aus. **Mercury** (kostenlos mit minimaler Anzahl von Verbindungen und geringem Datenträgerspeicherplatz) ist für die Zwecke dieses Tutorials ausreichend.
   
    e. Akzeptieren Sie auf dem Blatt **Neue MySQL-Datenbank** die rechtlichen Hinweise, und klicken Sie anschließend auf **OK**.
   
    ![Konfigurieren der Datenbankeinstellungen für die Web-App][ConfigDatabase]
7. Akzeptieren Sie auf dem Blatt **WordPress** die rechtlichen Hinweise, und klicken Sie anschließend auf **Erstellen**.
   
    ![Fertigstellen der Web-App-Einstellungen und klicken auf „OK“][ConfigFinished]
   
    Azure App Service erstellt die Web-App üblicherweise in weniger als einer Minute. Sie können den Fortschritt verfolgen, indem Sie oben auf der Portalseite auf das Glockensymbol klicken.
   
    ![Statusanzeige][ConfigProgress]

## <a name="launch-and-manage-your-wordpress-web-app"></a>Starten und Verwalten Ihrer WordPress-Web-App
1. Wenn die Erstellung der Web-App abgeschlossen ist, navigieren Sie im Azure-Portal zu der Ressourcengruppe, in der Sie die Anwendung erstellt haben. Dort werden die Web-App und die Datenbank angezeigt.
   
    Bei der zusätzlichen Ressource mit dem Glühbirnensymbol handelt es sich um den Dienst [Application Insights][ApplicationInsights], der Überwachungsdienste für Ihre Web-App bereitstellt.
2. Klicken Sie auf dem Blatt **Ressourcengruppe** auf die Zeile der Web-App.
   
    ![Auswählen Ihrer WordPress-Web-App][WordPressSelect]
3. Klicken Sie auf dem Blatt der Web-App auf **Durchsuchen**.
   
    ![Navigieren zu Ihrer WordPress-Web-App][WordPressBrowse]
4. Wählen Sie die gewünschte Sprache für Ihre WordPress-Blogs aus, wenn Sie dazu aufgefordert werden, und klicken Sie anschließend auf **Weiter**.
   
    ![Konfigurieren der Sprache für Ihre WordPress-Web-App][WordPressLanguage]
5. Geben Sie auf der WordPress-Seite **Willkommen** die Konfigurationsinformationen ein, die von WordPress benötigt werden, und klicken Sie dann auf **WordPress installieren**.
   
    ![Konfigurieren der Einstellungen für Ihre WordPress-Web-App][WordPressConfigure]
6. Melden Sie sich auf der Seite **Willkommen** mit den erstellten Anmeldeinformationen an.  
7. Die Dashboard-Seite Ihrer Website wird geöffnet und zeigt die von Ihnen angegebenen Informationen.    
   
    ![Anzeigen Ihres WordPress-Dashboards][WordPressDashboard2]

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie gelernt, wie Sie eine Beispiel-Web-App aus dem Azure Marketplace erstellen und bereitstellen.

Zusätzliche Dokumentation zur Arbeit mit App Service-Web-Apps finden Sie in den Links auf der linken Seite der Seite (bei breiten Browserfenstern) oder am oberen Rand der Seite (bei schmalen Browserfenstern).

Weitere Informationen zum Entwickeln von WordPress-Web-Apps in Azure finden Sie unter [Entwickeln von WordPress in Azure App Service][WordPressOnAzure].

<!-- URL List -->

[PHP]: https://azure.microsoft.com/develop/php/
[.NET]: https://azure.microsoft.com/develop/net/
[Node.js]: https://azure.microsoft.com/develop/nodejs/
[Java]: https://azure.microsoft.com/develop/java/
[Python]: https://azure.microsoft.com/develop/python/
[activate]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[free trial]: https://azure.microsoft.com/pricing/free-trial/
[Azure App Service-App erstellen]: https://azure.microsoft.com/try/app-service/
[ResourceGroups]: ../azure-resource-manager/resource-group-overview.md
[AzureAppServicePlans]: ../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md
[ApplicationInsights]: https://azure.microsoft.com/services/application-insights/
[Azure-Portal]: https://portal.azure.com/
[Project Nami]: http://projectnami.org/
[WordPressOnAzure]: ./develop-wordpress-on-app-service-web-apps.md

<!-- IMG List -->

[MarketplaceStart]: ./media/app-service-web-create-web-app-from-marketplace/marketplacestart.png
[MarketplaceSearch]: ./media/app-service-web-create-web-app-from-marketplace/marketplacesearch.png
[MarketplaceCreate]: ./media/app-service-web-create-web-app-from-marketplace/marketplacecreate.png
[ConfigStart]: ./media/app-service-web-create-web-app-from-marketplace/configstart.png
[ConfigAppName]: ./media/app-service-web-create-web-app-from-marketplace/configappname.png
[ConfigSubscription]: ./media/app-service-web-create-web-app-from-marketplace/configsubscription.png
[ConfigResourceGroup]: ./media/app-service-web-create-web-app-from-marketplace/configresourcegroup.png
[ConfigServicePlan]: ./media/app-service-web-create-web-app-from-marketplace/configserviceplan.png
[ConfigDatabase]: ./media/app-service-web-create-web-app-from-marketplace/configdatabase.png
[ConfigFinished]: ./media/app-service-web-create-web-app-from-marketplace/configfinished.png
[ConfigProgress]: ./media/app-service-web-create-web-app-from-marketplace/configprogress.png
[WordPressSelect]: ./media/app-service-web-create-web-app-from-marketplace/wpselect.png
[WordPressBrowse]: ./media/app-service-web-create-web-app-from-marketplace/wpbrowse.png
[WordPressLanguage]: ./media/app-service-web-create-web-app-from-marketplace/wplanguage.png
[WordPressDashboard1]: ./media/app-service-web-create-web-app-from-marketplace/wpdashboard1.png
[WordPressDashboard2]: ./media/app-service-web-create-web-app-from-marketplace/wpdashboard2.png
[WordPressConfigure]: ./media/app-service-web-create-web-app-from-marketplace/wpconfigure.png

