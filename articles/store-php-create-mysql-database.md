---
title: Erstellen einer MySQL-Datenbank und Herstellen einer Verbindung in Azure
description: "Hier erfahren Sie, wie Sie über das Azure-Portal eine MySQL-Datenbank erstellen und anschließend über eine PHP-Web-App in Azure eine Verbindung mit dieser Datenbank herstellen."
documentationcenter: php
services: app-service\web
author: cephalin
manager: erikre
editor: 
tags: mysql
ms.assetid: 55465a9a-7e65-4fd9-8a65-dd83ee41f3e5
ms.service: multiple
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 12/22/2016
ms.author: robmcm;cephalin
translationtype: Human Translation
ms.sourcegitcommit: cc9e81de9bf8a3312da834502fa6ca25e2b5834a
ms.openlocfilehash: e3d44bc4ff6ce7ebfcd510ef8b40fd0c614bc31e
ms.lasthandoff: 04/11/2017


---
# <a name="create-and-connect-to-a-mysql-database-in-azure"></a>Erstellen einer MySQL-Datenbank und Herstellen einer Verbindung in Azure
In diesem Tutorial erfahren Sie, wie Sie über das [Azure-Portal](https://portal.azure.com) (Anbieter: [ClearDB](http://www.cleardb.com/)) eine MySQL-Datenbank erstellen und über eine in [Azure App Service](app-service/app-service-value-prop-what-is.md) ausgeführte PHP-Web-App eine Verbindung mit der Datenbank herstellen.

> [!NOTE]
> Sie können auch eine MySQL-Datenbank auf der Grundlage einer [Marketplace-App-Vorlage](app-service-web/app-service-web-create-web-app-from-marketplace.md)erstellen.
>
>

## <a name="create-a-mysql-database-in-azure-portal"></a>Erstellen einer MySQL-Datenbank über das Azure-Portal
Gehen Sie wie folgt vor, um eine MySQL-Datenbank über das Azure-Portal zu erstellen:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Klicken Sie im linken Menü auf **Neu** > **Daten und Speicher** > **MySQL-Datenbank**.

    ![Erstellen einer MySQL-Datenbank in Azure – Start](./media/store-php-create-mysql-database/create-db-1-start.png)
3. Konfigurieren Sie Ihre neue MySQL-Datenbank auf dem [Blatt](azure-portal-overview.md) „MySQL-Datenbank“ wie im Anschluss beschrieben. (*Blatt*: eine Portalseite, die horizontal geöffnet wird.)

   * **Datenbankname**: Geben Sie einen eindeutigen Namen ein.
   * **Abonnement**: Wählen Sie das zu verwendende Abonnement aus.
   * **Datenbanktyp**: Wählen Sie **Shared** (kostengünstige/kostenlose Tarife) oder **Dediziert** (dedizierte Ressourcen) aus.
   * **Ressourcengruppe**: Fügen Sie die MySQL-Datenbank einer bereits vorhandenen oder einer neuen [Ressourcengruppe](azure-resource-manager/resource-group-overview.md) hinzu. Ressourcen, die sich in der gleichen Gruppe befinden, lassen sich komfortabel gemeinsam verwalten.
   * **Standort**: Wählen Sie einen Standort in Ihrer Nähe aus. Beim Hinzufügen zu einer bereits vorhandenen Ressourcengruppe ist als Standort der Standort dieser Ressourcengruppe vorgegeben.
   * **Tarif**: Klicken Sie auf **Tarif**, wählen Sie eine Preisoption aus (**Mercury** ist kostenlos), und klicken Sie anschließend auf **Auswählen**.
   * **Rechtliche Bedingungen**: Klicken Sie auf **Rechtliche Bedingungen**, überprüfen Sie die Details der Bestellung, und klicken Sie auf **Kaufen**.
   * **An Dashboard anheften**: Wählen Sie aus, ob Sie direkt über das Dashboard darauf zugreifen möchten. Dies ist besonders hilfreich, wenn Sie noch nicht mit der Portalnavigation vertraut sind.

     Der folgende Screenshot zeigt eine Beispielkonfiguration für eine MySQL-Datenbank:  
     ![Erstellen einer MySQL-Datenbank in Azure – Konfigurieren](./media/store-php-create-mysql-database/create-db-2-configure.png)
4. Klicken Sie nach Abschluss der Konfiguration auf **Erstellen**.

    ![Erstellen einer MySQL-Datenbank in Azure – Erstellen](./media/store-php-create-mysql-database/create-db-3-create.png)

    Eine Popupbenachrichtigung informiert darüber, dass die Bereitstellung gestartet wurde.

    ![Erstellen einer MySQL-Datenbank in Azure – in Bearbeitung](./media/store-php-create-mysql-database/create-db-4-started-status.png)

    Die erfolgreiche Bereitstellung wird durch eine weitere Popupbenachrichtigung signalisiert. Im Portal wird außerdem automatisch das Blatt für Ihre MySQL-Datenbank geöffnet.

<a name="connect"></a>

## <a name="connect-to-your-mysql-database"></a>Herstellen von Verbindungen mit der MySQL-Datenbank
Klicken Sie zum Anzeigen der Verbindungsinformationen für Ihre neue MySQL-Datenbank auf dem Blatt Ihrer Web-App einfach auf **Eigenschaften** .

![Erstellen einer MySQL-Datenbank in Azure – Blatt „MySQL-Datenbank“](./media/store-php-create-mysql-database/create-db-5-finished-db-blade.png)

Die Verbindungsinformationen können in jeder beliebigen Web-App verwendet werden. Ein Beispiel zur Veranschaulichung der Verwendung von Verbindungsinformationen in einer einfachen PHP-App finden Sie [hier](https://github.com/WindowsAzure/azure-sdk-for-php-samples/tree/master/tasklist-mysql).

## <a name="connect-a-laravel-web-app-from-the-php-get-started-tutorial"></a>Herstellen einer Verbindung mit einer Laravel-Web-App (aus dem Tutorial zu ersten Schritten mit PHP)
Wenn Sie bereits das Tutorial [Erstellen, Konfigurieren und Bereitstellen einer PHP-Web-App in Azure](app-service-web/app-service-web-php-get-started.md) abgeschlossen haben und über eine in Azure ausgeführte Web-App vom Typ [Larave](https://www.laravel.com/) verfügen, können Sie Ihrer Laravel-App problemlos Datenbankfunktionen hinzufügen. Gehen Sie dazu einfach wie folgt vor:

> [!NOTE]
> Bei den folgenden Schritten wird davon ausgegangen, dass Sie das Tutorial [Erstellen, Konfigurieren und Bereitstellen einer PHP-Web-App in Azure](app-service-web/app-service-web-php-get-started.md)abgeschlossen haben.
>
>

1. Konfigurieren Sie die Laravel-App in Ihrer lokalen Entwicklungsumgebung mit einem Verweis auf die MySQL-Datenbank. Öffnen Sie hierzu `.env` im Stammverzeichnis Ihrer Laravel-App, und konfigurieren Sie die Optionen der MySQL-Datenbank.

        DB_CONNECTION=mysql
        DB_HOST=<HOSTNAME_from_properties_blade>
        DB_PORT=<PORT_from_properties_blade>
        DB_DATABASE=<see_note_below>
        DB_USERNAME=<USERNAME_from_properties_blade>
        DB_PASSWORD=<PASSWORD_from_properties_blade>

   > [!NOTE]
   > Auf dem Blatt **Eigenschaften** wird im Feld **DATENBANKNAME** möglicherweise der Name Ihrer MySQL-Datenbank angezeigt. Es empfiehlt sich allerdings, den Database-Parameter im Feld **VERBINDUNGSZEICHENFOLGE** zu prüfen.    
   >
   > ![Erstellen einer MySQL-Datenbank in Azure – in Bearbeitung](./media/store-php-create-mysql-database/connect-db-1-database-name.png)
   >
   >
2. Der MySQL-Zugriff lässt sich am schnellsten mithilfe des [standardmäßigen Authentifizierungsgerüsts von Laravel](https://laravel.com/docs/5.2/authentication#authentication-quickstart)überprüfen.
   Führen Sie über das Befehlszeilenterminal die folgenden Befehle im Stammverzeichnis Ihrer Laravel-App aus:

         php artisan migrate
         php artisan make:auth

    Der erste Befehl erstellt die Tabellen in Azure auf der Grundlage vordefinierter Migrationen im Verzeichnis `database/migrations`. Der zweite Befehl erstellt das Gerüst für die grundlegenden Ansichten und Routen für Benutzerregistrierung und -authentifizierung.
3. Führen Sie nun den Entwicklungsserver aus:

        php artisan serve
4. Navigieren Sie im Browser zu „http://localhost:8000“, und registrieren Sie wie folgt einen neuen Benutzer:

    ![Herstellen einer Verbindung mit der MySQL-Datenbank in Azure – Benutzerregistrierung](./media/store-php-create-mysql-database/connect-db-2-development-server.png)

    Folgen Sie der Aufforderung auf der Benutzeroberfläche, um die Registrierung abzuschließen. Nach Abschluss der Registrierung werden Sie angemeldet.

    ![Herstellen einer Verbindung mit der MySQL-Datenbank in Azure – Benutzerregistrierung](./media/store-php-create-mysql-database/connect-db-3-registered-user.png)

    Sie entwickeln Ihre App nun unter Verwendung der MySQL-Datenbank in Azure.
5. Nun müssen Sie nur noch Ihre `.env` -Einstellungen in Ihre Azure-Web-App replizieren. Führen Sie über die Azure-Befehlszeilenschnittstelle folgende Befehle aus:

        azure site appsetting add DB_CONNECTION=mysql
        azure site appsetting add DB_HOST=<HOSTNAME_from_properties_blade>
        azure site appsetting add DB_PORT=<PORT_from_properties_blade>
        azure site appsetting add DB_DATABASE=<Database_param_from_CONNECTION_INFO_from_properties_blade>
        azure site appsetting add DB_USERNAME=<USERNAME_from_properties_blade>
        azure site appsetting add DB_PASSWORD=<PASSWORD_from_properties_blade>

    Informationen zur Vorgehensweise finden Sie unter [Konfigurieren der Azure-Web-App](app-service-web/app-service-web-get-started-php.md#configure-to-use-php).
6. Führen Sie als Nächstes für die lokalen Änderungen, die Sie zuvor beim Ausführen von `php artisan make:auth`vorgenommen haben, ein Commit und einen Pushvorgang an Azure durch.

        git add .
        git commit -m "scaffold auth views and routes"
        git push azure master
7. Navigieren Sie zur Azure-Web-App.

        azure site browse
8. Melden Sie sich mithilfe der zuvor erstellten Benutzeranmeldeinformationen an.

    ![Herstellen einer Verbindung mit der MySQL-Datenbank in Azure – Navigieren zur Azure-Web-App](./media/store-php-create-mysql-database/connect-db-4-browse-azure-webapp.png)

    Der benutzerfreundliche Bildschirm nach der Anmeldung erscheint.

    ![Herstellen einer Verbindung mit der MySQL-Datenbank in Azure – angemeldet](./media/store-php-create-mysql-database/connect-db-5-logged-in.png)

    Herzlichen Glückwunsch! Ihre PHP-Web-App in Azure greift nun auf Daten aus Ihrer MySQL-Datenbank zu.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie im [PHP Developer Center](/develop/php/).

