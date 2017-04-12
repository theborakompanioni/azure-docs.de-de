---
title: Erstellen einer ASP.NET MVC-App mit Authentifizierung, SQL-Datenbank und Bereitstellung in Azure App Service
description: "Erfahren Sie, wie Sie eine ASP.NET-MVC 5-App mit einem SQL-Datenbank-Back-End und Authentifizierung sowie Autorisierung entwickeln und in Azure bereitstellen."
services: app-service\web
documentationcenter: .net
author: Rick-Anderson
writer: Rick-Anderson
manager: erikre
editor: 
ms.assetid: c0de419d-db6f-4157-94ca-f75d0ba6c0e3
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/21/2016
ms.author: riande
translationtype: Human Translation
ms.sourcegitcommit: 6ea03adaabc1cd9e62aa91d4237481d8330704a1
ms.openlocfilehash: b1d9c1c75d29e39bbd9433197ad06a56da5dc799
ms.lasthandoff: 04/06/2017


---
# <a name="create-an-aspnet-mvc-app-with-auth-and-sql-db-and-deploy-to-azure-app-service"></a>Erstellen einer ASP.NET MVC-App mit Authentifizierung, SQL-Datenbank und Bereitstellung in Azure App Service
In diesem Lernprogramm wird die Erstellung einer sicheren ASP.NET MVC 5-Web-App erläutert, mit der Benutzer sich mithilfe der Anmeldeinformationen von Facebook oder Google anmelden können. Die App ist eine einfache Kontaktliste, die das ADO.NET Entity Framework für den Datenbankzugriff verwendet. Sie stellen die App für den [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714)bereit. 

Nach Abschluss dieses Lernprogramms verfügen Sie über eine sichere datengesteuerte Webanwendung, die unter Rückgriff auf eine Clouddatenbank in der Cloud ausgeführt wird. In der folgenden Abbildung wird die Anmeldeseite der fertiggestellten Anwendung dargestellt:

![Anmeldeseite][rxb]

Sie lernen Folgendes:

* Erstellen eines sicheren ASP.NET MVC 5-Webprojekts in Visual Studio.
* Authentifizieren und Autorisieren von Benutzern, die sich mit den Anmeldeinformationen ihres Google- oder Facebook-Kontos (Authentifizierung über Anbieter sozialer Dienste mit [OAuth 2.0](http://oauth.net/2 "http://oauth.net/2")) anmelden.
* Authentifizieren und Autorisieren von Benutzern, die sich bei einer von der Anwendung verwalteten Datenbank registrieren (lokale Authentifizierung mit [ASP.NET-Identität](http://asp.net/identity/)).
* Verwendung von ADO.NET Entity Framework 6 Code First zum Lesen und Schreiben von Daten in eine SQL­-Datenbank.
* Verwenden von Entity Framework Code First-Migrationen zum Bereitstellen einer Datenbank.
* Speichern relationaler Daten in der Cloud mithilfe der Azure SQL-Datenbank.
* Bereitstellen eines Webprojekts, das eine Datenbank für eine [Web-App](http://go.microsoft.com/fwlink/?LinkId=529714) in Azure App Service verwendet.

> [!NOTE]
> Dies ist ein langes Lernprogramm. Wenn Sie eine schnelle Einführung in Azure App Service und Visual Studio-Webprojekte wünschen, informieren Sie sich unter [Erstellen von ASP.NET-Web-Apps in Azure App Service](app-service-web-get-started-dotnet.md). Weitere Informationen zur Problembehandlung finden Sie im Abschnitt [Problembehandlung](#troubleshooting) .
> 
> Wenn Sie Azure App Service ausprobieren möchten, bevor Sie sich für ein Azure-Konto anmelden, können Sie unter [App Service testen](https://azure.microsoft.com/try/app-service/)sofort kostenlos eine kurzlebige Starter-Web-App in App Service erstellen. Keine Kreditkarte erforderlich, keine Verpflichtungen.
> 
> 

## <a name="prerequisites"></a>Voraussetzungen
Sie benötigen ein Microsoft Azure-Konto, um dieses Tutorial auszuführen. Wenn Sie kein Konto haben, können Sie [Ihre Visual Studio-Abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) oder [sich für eine kostenlose Testversion registrieren](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).

Zur Einrichtung Ihrer Entwicklungsumgebung müssen Sie [Visual Studio 2013 Update 5](http://go.microsoft.com/fwlink/?LinkId=390521) oder höher und die neueste Version von [Azure SDK für .NET](http://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409) installieren. Dieser Artikel wurde für Visual Studio Update 4 und SDK 2.8.1 geschrieben. Die gleichen Anweisungen funktionieren mit Visual Studio 2015 mit dem neuesten installierten [Azure SDK für .NET](http://go.microsoft.com/fwlink/?linkid=518003&clcid=0x409) , doch einige Bildschirme unterscheiden sich von den Abbildungen.

## <a name="create-an-aspnet-mvc-5-application"></a>Erstellen einer ASP.NET MVC 5-Anwendung
### <a name="create-the-project"></a>Erstellen des Projekts
1. Klicken Sie im Menü **Datei** auf **Neues Projekt**.
   
    !["Neues Projekt" im Menü "Datei"](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/gs13newproj.png)
2. Erweitern Sie im Dialogfeld **Neues Projekt** den Eintrag **C#**, und wählen Sie unter **Installierte Vorlagen** die Option **Web** aus. Wählen Sie dann **ASP.NET-Webanwendung** aus. Nennen Sie die Anwendung **ContactManager**, und klicken Sie auf **OK**.
   
    ![Dialogfeld "Neues Projekt"](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/GS13newprojdb.png)
   
    **Hinweis:** Geben Sie "ContactManager" ein. In den Codeblöcken, die Sie später kopieren, wird davon ausgegangen, dass der Projektname "ContactManager" lautet. 
3. Wählen Sie im Dialogfeld **Neues ASP.NET-Projekt** die Vorlage **MVC** aus. Überprüfen Sie, ob die **Authentifizierung** auf **Einzelne Benutzerkonten** festgelegt, **Host in der Cloud** aktiviert und **App Service** ausgewählt ist.
   
    ![Dialogfeld "Neues ASP.NET-Projekt"](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/newproject.png)
4. Klicken Sie auf **OK**.
5. Das Dialogfeld **Einstellungen der Microsoft Azure-Web-App konfigurieren** wird angezeigt. Möglicherweise müssen Sie sich anmelden, wenn Sie noch nicht angemeldet sind. Wenn Ihre Anmeldung abgelaufen ist, geben Sie Ihre Anmeldeinformationen erneut ein.
6. Optional: Ändern Sie den Wert im Feld **Web-App-Name** (siehe Abbildung unten).
   
    Der Name muss innerhalb der Domäne "azurewebsites.net" eindeutig sein, da die URL der Web-App "{name}.azurewebsites.net" lauten wird. Der Konfigurations-Assistent schlägt einen eindeutigen Namen vor, für den eine Nummer an Projektnamen "ContactManager" angefügt wird, was für dieses Tutorial in Ordnung ist.
7. Wählen Sie in der Dropdownliste **Ressourcengruppe** eine vorhandene Gruppe aus, oder **erstellen Sie eine neue Ressourcengruppe** (siehe Abbildung unten). 
   
    Falls gewünscht, können Sie eine Ressourcengruppe auswählen, über die Sie bereits verfügen. Doch wenn Sie eine neue Ressourcengruppe erstellen, die Sie nur für dieses Tutorial verwenden, können Sie einfach alle Azure-Ressourcen für das Tutorial löschen, wenn Sie sie nicht mehr benötigen. Informationen zu Ressourcengruppen finden Sie unter [Übersicht über den Azure Resource Manager](../azure-resource-manager/resource-group-overview.md). 
8. Wählen Sie in der Dropdownliste **App Service-Plan** einen vorhandenen Plan aus, oder **erstellen Sie einen neuen App Service-Plan** (siehe Abbildung unten).
   
    Falls gewünscht, können Sie einen App Service-Plan auswählen, über den Sie bereits verfügen. Informationen zu App Service-Plänen finden Sie unter [Azure App Service-Pläne – Detaillierte Übersicht](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md). 
9. Klicken Sie auf **Zusätzliche Azure-Dienste durchsuchen** , um eine SQL-­Datenbank hinzuzufügen.
   
    ![Hinzufügen neuer Dienste](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/n2.png)
10. Klicken Sie auf das Pluszeichen **+** , um eine SQL-Datenbank hinzuzufügen.
    
     ![Neue SQL-Datenbank](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/nsql.png)
11. Klicken Sie im Dialogfeld **SQL-Datenbank konfigurieren** auf **Neu**:
    
     ![Name und Kennwort für SQL-Administrator](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/nc.png)
12. Geben Sie einen Namen für den Administrator und ein sicheres Kennwort ein.
    
     ![Neue SQL-Datenbank](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/np.png)
    
     Der Servername muss eindeutig sein. Er kann Kleinbuchstaben, Ziffern und Bindestriche enthalten. Ein nachgestellter Bindestrich ist nicht zulässig. Der Benutzername und das Kennwort sind neue Anmeldeinformationen, die Sie für den neuen Server erstellen. 
    
     Wenn Sie bereits einen Datenbankserver haben, können Sie ihn auswählen, anstatt einen zu erstellen. Datenbankserver sind wertvolle Ressourcen, und Sie möchten in der Regel zwecks Test und Entwicklung mehrere Datenbanken auf demselben Server erstellen, anstatt einen Datenbankserver pro Datenbank zu erstellen. Für dieses Tutorial brauchen Sie den Server allerdings nur vorübergehend. Wenn Sie den Server in derselben Ressourcengruppe wie die Website erstellen, vereinfachen Sie das Löschen von Web-App- und Datenbankressourcen, indem Sie die Ressourcengruppe nach Durcharbeiten des Tutorials löschen. 
    
     Wenn Sie einen vorhandenen Datenbankserver auswählen, stellen Sie sicher, dass sich Ihre Web-App und die Datenbank in derselben Region befinden.
    
     ![Verwenden einer neuen Datenbank](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/newdb.png)
13. Tippen Sie auf **Erstellen**.
    
     Visual Studio erstellt das Webprojekt "ContactManager", die Ressourcengruppe und den App Service-Plan, den Sie angegeben haben, und eine Web-App in Azure App Service mit dem angegebenen Namen.

### <a name="set-the-page-header-and-footer"></a>Einrichten der Seitenkopf- und -fußzeile
1. Öffnen Sie im **Projektmappen-Explorer** die Datei *Layout.cshtml* im Ordner *Views\Shared*.
   
    !["Layout.cshtml" im Projektmappen-Explorer][newapp004]
2. Ersetzen Sie den ActionLink in der Datei *Layout.cshtml* durch den folgenden Code.

```
   @Html.ActionLink("CM Demo", "Index", "Contacts", new { area = "" }, new { @class = "navbar-brand" })
```
   Stellen Sie sicher, dass Sie den dritten Parameter von „Home“ in „Contacts“ ändern. Mit dem Markup oben wird der Link „Contacts“ auf jeder Seite zur Index-Methode des Contacts-Controllers erstellt. Ändern Sie den Namen der Anwendung in der Kopf- und Fußzeile von „My ASP.NET Application“ und „Application name“ in „Contact Manager“ bzw. „CM Demo“. 

### <a name="run-the-application-locally"></a>Lokales Ausführen der Anwendung
1. Drücken Sie STRG+F5, um die Anwendung auszuführen.
   
    Die Startseite der Anwendung wird im Standardbrowser angezeigt.
   
    ![Lokal ausgeführte Web-App](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/rr2.png)

Darüber hinaus müssen Sie keine weiteren Aktionen ausführen, um die Anwendung zu erstellen, die Sie für Azure bereitstellen. 

## <a name="deploy-the-application-to-azure"></a>Bereitstellen der Anwendung in Azure
1. Klicken Sie im **Projektmappen-Explorer** von Visual Studio mit der rechten Maustaste auf das Projekt, und wählen Sie im Kontextmenü **Veröffentlichen** aus.
   
    !["Veröffentlichen" im Kontextmenü des Projekts](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/GS13publish.png)
   
    Der Assistent **Web veröffentlichen** wird geöffnet.
2. Klicken Sie im Dialogfeld **Web veröffentlichen** auf **Veröffentlichen**.
   
    ![Veröffentlichen](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/rr3.png)
   
    Die erstellte Anwendung wird nun in der Cloud ausgeführt. Bei der nächsten Bereitstellung der Anwendung werden nur die geänderten (oder neuen) Dateien bereitgestellt.
   
    ![In Cloud ausgeführt](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/ss4.PNG)

## <a name="enable-ssl-for-the-project"></a>Aktivieren von SSL für das Projekt
1. Klicken Sie im **Projektmappen-Explorer** auf das Projekt **ContactManager**, und drücken Sie dann F4, um das Fenster **Eigenschaften** zu öffnen.
2. Ändern Sie **SSL-aktiviert** in **True**. 
3. Kopieren Sie die **SSL-URL**.
   
    Die SSL-URL lautet "https://localhost:44300/", sofern Sie nicht zuvor SSL-Web-Apps erstellt haben.
   
    ![SSL aktivieren][rxSSL]
4. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **Contact Manager** und dann auf **Eigenschaften**.
5. Klicken Sie auf die Registerkarte **Web** .
6. Ändern Sie die **Projekt-URL** so, dass die **SSL-URL** verwendet wird, und speichern Sie die Seite (STRG+S).
   
    ![SSL aktivieren](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/rrr1.png)
7. Stellen Sie sicher, dass Internet Explorer von Visual Studio gestartet wird, wie in der folgenden Abbildung gezeigt:
   
    ![Standardbrowser](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/ss12.PNG)
   
    In der Browserauswahl können Sie den Browser angeben, der von Visual Studio gestartet wird. Sie können mehrere Browser auswählen und die Browser von Visual Studio aktualisieren lassen, wenn Sie Änderungen vornehmen. Weitere Informationen finden Sie unter [Using Browser Link in Visual Studio 2013](http://www.asp.net/visual-studio/overview/2013/using-browser-link)(in englischer Sprache).
   
     ![Browserauswahl](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/ss13.png)
8. Drücken Sie STRG+F5, um die Anwendung auszuführen. Klicken Sie auf **Ja** , um den Prozess zu starten, mit dem das von IIS Express generierte selbstsignierte Zertifikat als vertrauenswürdig eingestuft wird.
   
     ![Anweisungen, um das von IIS Express generierte selbstsignierte Zertifikat als vertrauenswürdig einzustufen](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/ss26.PNG)
9. Lesen Sie das Dialogfeld **Sicherheitswarnung**, und klicken Sie dann auf **Ja**, wenn Sie das Zertifikat installieren möchten, das **localhost** darstellt.
   
     ![IIS Express-Zertifikatwarnung für Localhost ](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/ss27.PNG)
10. Die Seite *Home* wird in IE angezeigt, und es gibt keine SSL-Warnungen.
    
      ![IE mit Localhost-SSL, keine Warnungen](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/ss28.PNG)
    
      Internet Explorer ist eine gute Wahl, wenn Sie SSL verwenden, da das Zertifikat akzeptiert und HTTPS-Inhalt ohne Warnung angezeigt wird. Microsoft Edge und Google Chrome akzeptieren auch das Zertifikat. Firefox verwendet einen eigenen Zertifikatspeicher, weshalb eine Warnung angezeigt wird.
    
      ![FireFox-Zertifikatwarnung](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/ss30.PNG)

## <a name="add-a-database-to-the-application"></a>Hinzufügen einer Datenbank zur Anwendung
Als Nächstes aktualisieren Sie die App, um Funktionen zum Anzeigen und Aktualisieren von Kontakten sowie zum Speichern der Daten in einer Datenbank hinzuzufügen. Die App verwendet das Entity Framework (EF), um die Datenbank zu erstellen und Daten zu lesen und zu aktualisieren.

### <a name="add-data-model-classes-for-the-contacts"></a>Hinzufügen von Datenmodellklassen für die Kontakte
Sie beginnen mit der Erstellung eines einfachen Datenmodells in Code.

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Ordner Models, und klicken Sie dann auf **Hinzufügen** > **Klasse**.
   
    ![Klasse hinzufügen im Kontextmenü des Ordners "Models"](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/rr5.png)
2. Geben Sie der neuen Klassendatei im Dialogfeld **Neues Element hinzufügen** den Namen *Contact.cs*, und klicken Sie dann auf **Hinzufügen**.
   
    ![Dialogfeld "Neues Element hinzufügen"][adddb002]
3. Ersetzen Sie den Inhalt der Datei "Contacts.cs" durch den folgenden Code.
   
        using System.ComponentModel.DataAnnotations;
        using System.Globalization;
        namespace ContactManager.Models
        {
            public class Contact
            {
                public int ContactId { get; set; }
                public string Name { get; set; }
                public string Address { get; set; }
                public string City { get; set; }
                public string State { get; set; }
                public string Zip { get; set; }
                [DataType(DataType.EmailAddress)]
                public string Email { get; set; }
            }
        }
   Mit der **Contacts** -Klasse werden die Daten definiert, die für die einzelnen Kontakte gespeichert werden, sowie ein Primärschlüssel ( *ContactID*), der von der Datenbank benötigt wird.

### <a name="create-web-pages-that-enable-app-users-to-work-with-the-contacts"></a>Erstellen von Webseiten, die Anwendungsbenutzern das Arbeiten mit den Kontakten ermöglichen
Mit dem ASP.NET MVC-Gerüstfeature kann automatisch Code generiert werden, der Erstellungs-, Lese-, Aktualisierungs- und Löschaktionen (CRUD) durchführt. 

1. Erstellen Sie das Projekt **(STRG+UMSCHALT+B)**. (Sie müssen das Projekt vor der Verwendung des Gerüstmechanismus erstellen.)
2. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Ordner „Controllers“, und klicken Sie dann auf **Hinzufügen** > **Controller**.
   
    ![Controller hinzufügen im Kontextmenü des Ordners "Controllers"][addcode001]
3. Wählen Sie im Dialogfeld **Gerüst hinzufügen** die Option **MVC 5-Controller mit Ansichten unter Verwendung von Entity Framework** aus, und klicken Sie dann auf **Hinzufügen**.
   
    ![Dialogfeld "Add Scaffold"](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/rr6.png)
4. Wählen Sie im Dropdownfeld **Modellklasse** die Option **Contact (ContactManager.Models)** aus. (Siehe Abbildung unten.)
5. Wählen Sie unter **Datenkontextklasse** die Option **ApplicationDbContext (ContactManager.Models)** aus. **ApplicationDbContext** wird für die Mitgliedschaftsdatenbank und die Kontaktdaten verwendet.

    ![Kontextdialogfeld "Neue Daten"](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/ss5.PNG)

1. Klicken Sie auf **Hinzufügen**.
   
   In Visual Studio wird ein Controller mit Methoden und Ansichten für CRUD-Datenbankvorgänge für **Contact** -Objekte erstellt.

## <a name="enable-migrations-create-the-database-add-sample-data-and-a-data-initializer"></a>Aktivieren von Migrationen, Erstellen der Datenbank, Hinzufügen von Beispieldaten und eines Dateninitialisierers
Die nächste Aufgabe besteht darin, das Feature [Code First-Migrationen](http://msdn.microsoft.com/library/hh770484.aspx) zu aktivieren, um die Datenbanktabellen basierend auf dem definierten Datenmodell zu erstellen.

1. Wählen Sie im Menü **Extras** den Eintrag **NuGet Package Manager** und danach **Paket-Manager-Konsole** aus.
   
    !["Paket-Manager-Konsole" im Menü "Extras"](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/SS6.png)
2. Geben Sie im Fenster **Paket-Manager-Konsole** den folgenden Befehl ein:
   
        enable-migrations
   
    Mit dem Befehl **enable-migrations** wird der Ordner *Migrations* erstellt und die Datei *Configuration.cs* darin abgelegt. Sie können diese Datei bearbeiten, um ein Seeding für die Datenbank auszuführen und „Migrations“ zu konfigurieren. 
3. Geben Sie im Fenster **Paket-Manager-Konsole** den folgenden Befehl ein:
   
        add-migration Initial

    Mit dem Befehl **add-migration Initial** wird eine Datei namens **&lt;Zeitstempel&gt;Initial** im Ordner *Migrations* generiert. Mit dem Code in dieser Datei werden die Datenbanktabellen erstellt. Der erste Parameter ( **Initial** ) wird zum Erstellen des Dateinamens verwendet. Sie können die neue Klasse im **Projektmappen-Explorer**anzeigen.

    In der **Initial**-Klasse wird mit der **Up**-Methode die Tabelle „Contacts“ erstellt und mit der **Down**-Methode (wird verwendet, wenn Sie zum vorherigen Status zurückkehren möchten) wieder verworfen.

1. Öffnen Sie die Datei *Migrations\Configuration.cs*. 
2. Fügen Sie die folgende `using` -Anweisung hinzu. 
   
         using ContactManager.Models;
3. Ersetzen Sie die *Seed* -Methode durch den folgenden Code:
   
        protected override void Seed(ContactManager.Models.ApplicationDbContext context)
        {
            context.Contacts.AddOrUpdate(p => p.Name,
               new Contact
               {
                   Name = "Debra Garcia",
                   Address = "1234 Main St",
                   City = "Redmond",
                   State = "WA",
                   Zip = "10999",
                   Email = "debra@example.com",
               },
                new Contact
                {
                    Name = "Thorsten Weinrich",
                    Address = "5678 1st Ave W",
                    City = "Redmond",
                    State = "WA",
                    Zip = "10999",
                    Email = "thorsten@example.com",
                },
                new Contact
                {
                    Name = "Yuhong Li",
                    Address = "9012 State st",
                    City = "Redmond",
                    State = "WA",
                    Zip = "10999",
                    Email = "yuhong@example.com",
                },
                new Contact
                {
                    Name = "Jon Orton",
                    Address = "3456 Maple St",
                    City = "Redmond",
                    State = "WA",
                    Zip = "10999",
                    Email = "jon@example.com",
                },
                new Contact
                {
                    Name = "Diliana Alexieva-Bosseva",
                    Address = "7890 2nd Ave E",
                    City = "Redmond",
                    State = "WA",
                    Zip = "10999",
                    Email = "diliana@example.com",
                }
                );
        }
   
    Mit diesem Code wird die Datenbank mit den Kontaktinformationen initialisiert (es wird ein Seeding dafür ausgeführt). Weitere Informationen zum Ausführen eines Seedings für die Datenbank finden Sie unter [Seeding and Debugging Entity Framework (EF) DBs](http://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx)(in englischer Sprache). Erstellen Sie das Projekt, um sich zu vergewissern, dass keine Kompilierungsfehler vorhanden sind.
4. Geben Sie im Fenster **Paket-Manager-Konsole** den folgenden Befehl ein:
   
        update-database
   
    ![Befehle in der Paket-Manager-Konsole][addcode009]
   
    Mit **update-database** wird die erste Migration ausgeführt, wodurch die Datenbank erstellt wird. Standardmäßig wird die Datenbank als SQL Server Express LocalDB-Datenbank erstellt. 
5. Drücken Sie STRG+F5, um die Anwendung auszuführen, und klicken Sie dann auf den Link **CM Demo**, oder navigieren Sie zu „http://localhost:(Portnr.)/Cm“. 
   
    In der Anwendung werden die Seeddaten angezeigt und Links zum Bearbeiten und Löschen und zu Details bereitgestellt. Sie können Daten erstellen, bearbeiten, löschen und anzeigen.
   
    ![MVC-Datenansicht][rx2]

## <a name="add-an-oauth2-provider"></a>Hinzufügen eines OAuth2-Anbieters
> [!NOTE]
> Für detaillierte Anweisungen zur Verwendung der Portalwebsites für Google- und Facebook-Entwickler bietet dieses Tutorial Links zu Tutorials auf der ASP.NET-Website. Google und Facebook ändern jedoch ihre Websites öfter, als diese Tutorials aktualisiert werden, sodass sie nun veraltet sind. Wenn Sie Probleme beim Befolgen der Anweisungen haben, finden Sie im Disqus-Kommentar am Ende dieses Tutorials eine Liste der Änderungen. 
> 
> 

[OAuth](http://oauth.net/ "http://oauth.net/") ist ein offenes Protokoll, das die sichere Autorisierung in einer einfachen und Standardmethode von Web-, Mobil- und Desktopanwendungen ermöglicht. OAuth wird von der ASP.NET MVC-Internetvorlage verwendet, um Facebook, Twitter, Google und Microsoft als Authentifizierungsanbieter verfügbar zu machen. Auch wenn in diesem Lernprogramm nur Google als Authentifizierungsanbieter eingesetzt wird, können Sie den Code problemlos für die Verwendung einer der anderen Anbieter anpassen. Die Schritte zur Implementierung anderer Anbieter unterscheiden sich kaum von den Schritten in diesem Tutorial. Informationen zum Verwenden von Facebook als Authentifizierungsanbieter finden Sie im Tutorial [MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on ](http://www.asp.net/mvc/tutorials/mvc-5/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on)(in englischer Sprache).

Abgesehen von der Authentifizierung werden in diesem Tutorial auch Rollen verwendet, um die Autorisierung zu implementieren. Nur Benutzer, die Sie der Rolle *canEdit* hinzufügen, können Daten ändern (d. h. Kontakte erstellen, bearbeiten oder löschen).

1. Folgen Sie den Anweisungen in [MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on ](http://www.asp.net/mvc/tutorials/mvc-5/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on#goog) unter **Creating a Google app for OAuth 2 to set up a Google app for OAuth2** (in englischer Sprache).
2. Führen Sie die App aus, und testen Sie, ob Sie sich mit der Google-Authentifizierung anmelden können.
3. Wenn Sie Anmeldeschaltflächen für soziale Netzwerke mit anbieterspezifischen Symbolen erstellen möchten, informieren Sie sich unter [Ansprechende Anmeldeschaltflächen für soziale Netzwerke für ASP.NET MVC 5](http://www.jerriepelser.com/blog/pretty-social-login-buttons-for-asp-net-mvc-5)

## <a name="using-the-membership-api"></a>Verwenden der Mitgliedschafts-API
In diesem Abschnitt fügen Sie der Mitgliedschaftsdatenbank einen lokalen Benutzer und die Rolle *canEdit* hinzu. Nur Benutzer mit der Rolle *canEdit* können Daten bearbeiten. Es wird empfohlen, Rollen nach den Aktionen zu benennen, die damit ausgeführt werden können. Entsprechend ist *canEdit* einer Rolle mit dem Namen *admin* vorzuziehen. Mit fortschreitender Fertigstellung der Anwendung können Sie neue Rollen wie *canDeleteMembers* anstelle von weniger aussagekräftigen Rollen wie *superAdmin* hinzufügen.

1. Öffnen Sie die Datei *migrations\configuration.cs*, und fügen Sie die folgenden `using`-Anweisungen hinzu:
   
        using Microsoft.AspNet.Identity;
        using Microsoft.AspNet.Identity.EntityFramework;
2. Fügen Sie der Klasse die folgende **AddUserAndRole** -Methode hinzu:
   
        bool AddUserAndRole(ContactManager.Models.ApplicationDbContext context)
        {
            IdentityResult ir;
            var rm = new RoleManager<IdentityRole>
                (new RoleStore<IdentityRole>(context));
            ir = rm.Create(new IdentityRole("canEdit"));
            var um = new UserManager<ApplicationUser>(
                new UserStore<ApplicationUser>(context));
            var user = new ApplicationUser()
            {
                UserName = "user1@contoso.com",
            };
            ir = um.Create(user, "P_assw0rd1");
            if (ir.Succeeded == false)
                return ir.Succeeded;
            ir = um.AddToRole(user.Id, "canEdit");
            return ir.Succeeded;
        }
3. Rufen Sie die neue Methode über die **Seed** -Methode auf:
   
        protected override void Seed(ContactManager.Models.ApplicationDbContext context)
        {
            AddUserAndRole(context);
            context.Contacts.AddOrUpdate(p => p.Name,
                // Code removed for brevity
        }
   
    In den folgenden Bildern werden die Änderungen an der *Seed* -Methode gezeigt:
   
    ![Codebild](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/ss24.PNG)
   
    Dieser Code erstellt die neue Rolle *canEdit* sowie den neuen lokalen Benutzer *user1@contoso.com* und fügt *user1@contoso.com* der Rolle *canEdit* hinzu. Weitere Informationen finden Sie in den [ASP.NET Identity-Tutorials](http://www.asp.net/identity/overview/features-api) auf der ASP.NET-Website.

## <a name="use-temporary-code-to-add-new-social-login-users-to-the-canedit-role"></a>Verwenden von temporärem Code zum Hinzufügen neuer Benutzer mit Anmeldung zu sozialen Netzwerken zur Rolle "canEdit"
In diesem Abschnitt ändern Sie die Methode **ExternalLoginConfirmation** im Kontocontroller vorübergehend, um neue Benutzer hinzuzufügen, die sich mit einem OAuth-Anbieter bei der Rolle *canEdit* registrieren. Es ist geplant, in Zukunft ein mit [WSAT](http://msdn.microsoft.com/library/ms228053.aspx) vergleichbares Tool bereitzustellen, mit dem Benutzerkonten und -rollen erstellt und bearbeitet werden können. Bis dahin können Sie diese Aufgabe mithilfe von temporärem Code erledigen.

1. Öffnen Sie die Datei **Controllers\AccountController.cs**, und navigieren Sie zur **ExternalLoginConfirmation**-Methode.
2. Fügen Sie **AddToRoleAsync** unmittelbar vor dem Aufruf **SignInAsync** den folgenden Aufruf hinzu.
   
        await UserManager.AddToRoleAsync(user.Id, "canEdit");
   
   Mit dem oben dargestellten Code wird der neu registrierte Benutzer der Rolle "canEdit" hinzugefügt, sodass er auf Aktionsmethoden zugreifen kann, die das Ändern (Bearbeiten) von Daten erlauben. Der folgende Codeausschnitt zeigt die neue Zeile des Codes im Kontext.
   
          // POST: /Account/ExternalLoginConfirmation
          [HttpPost]
          [AllowAnonymous]
          [ValidateAntiForgeryToken]
          public async Task ExternalLoginConfirmation(ExternalLoginConfirmationViewModel model, string returnUrl)
          {
             if (User.Identity.IsAuthenticated)
             {
                return RedirectToAction("Index", "Manage");
             }
             if (ModelState.IsValid)
             {
                // Get the information about the user from the external login provider
                var info = await AuthenticationManager.GetExternalLoginInfoAsync();
                if (info == null)
                {
                   return View("ExternalLoginFailure");
                }
                var user = new ApplicationUser { UserName = model.Email, Email = model.Email };
                var result = await UserManager.CreateAsync(user);
                if (result.Succeeded)
                {
                   result = await UserManager.AddLoginAsync(user.Id, info.Login);
                   if (result.Succeeded)
                   {
                      await UserManager.AddToRoleAsync(user.Id, "canEdit");
                      await SignInManager.SignInAsync(user, isPersistent: false, rememberBrowser: false);
                      return RedirectToLocal(returnUrl);
                   }
                }
                AddErrors(result);
             }
             ViewBag.ReturnUrl = returnUrl;
             return View(model);
          }

An späterer Stelle in diesem Lernprogramm stellen Sie die Anwendung für Azure bereit. Die Anmeldung erfolgt dann über Google oder einen anderen Authentifizierungsdrittanbieter. Dadurch wird Ihr neu registriertes Konto der Rolle *canEdit* hinzugefügt. Jede Person, die die URL Ihrer Web-App findet und über eine Google-ID verfügt, kann sich dann registrieren und die Datenbank aktualisieren. Sie können die Website beenden, um dies zu verhindern. Durch eine Untersuchung der Datenbank können Sie überprüfen, wer der Rolle *canEdit* angehört.

Drücken Sie in der **Paket-Manager-Konsole** die NACH-OBEN-TASTE, um den folgenden Befehl aufzurufen:

        Update-Database

Mit dem **Update-Database**-Befehl wird die **Seed**-Methode ausgeführt, und mit dieser wird die **AddUserAndRole**-Methode ausgeführt, die Sie zuvor hinzugefügt haben. Die **AddUserAndRole**-Methode erstellt den Benutzer *user1@contoso.com* und fügt ihn der Rolle *canEdit* hinzu.

## <a name="protect-the-application-with-ssl-and-the-authorize-attribute"></a>Schützen der Anwendung durch SSL und das Attribut "Authorize"
In diesem Abschnitt wenden Sie das [Authorize](http://msdn.microsoft.com/library/system.web.mvc.authorizeattribute.aspx) -Attribut an, um den Zugriff auf die Aktionsmethoden einzuschränken. Anonyme Benutzer können nur die **Index** -Aktionsmethode des Controllers "Home" anzeigen. Registrierten Benutzern werden die Kontaktdaten (die Seiten **Index** und **Details** des Controllers „Cm“) sowie die Info- und Kontaktseiten angezeigt. Nur Benutzer mit der Rolle *canEdit* können auf Aktionsmethoden zugreifen, mit denen Daten geändert werden.

1. Öffnen Sie die Datei *App_Start\FilterConfig.cs*, und ersetzen Sie die *RegisterGlobalFilters*-Methode durch den folgenden Code (damit werden die zwei Filter hinzugefügt):
   
        public static void RegisterGlobalFilters(GlobalFilterCollection filters)
        {
            filters.Add(new HandleErrorAttribute());
            filters.Add(new System.Web.Mvc.AuthorizeAttribute());
            filters.Add(new RequireHttpsAttribute());
        }
   
    Dieser Code fügt der Anwendung die Filter [Authorize](http://msdn.microsoft.com/library/system.web.mvc.authorizeattribute.aspx) und [RequireHttps](http://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) hinzu. Der Filter [Authorize](http://msdn.microsoft.com/library/system.web.mvc.authorizeattribute.aspx) verhindert, dass anonyme Benutzer auf Methoden in der Anwendung zugreifen. Sie verwenden das [AllowAnonymous](http://blogs.msdn.com/b/rickandy/archive/2012/03/23/securing-your-asp-net-mvc-4-app-and-the-new-allowanonymous-attribute.aspx) -Attribut, um die Autorisierungsanforderung in einigen Methoden zu deaktivieren, sodass anonyme Benutzer sich anmelden und die Startseite anzeigen können. [RequireHttps](http://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) erfordert, dass der gesamte Zugriff auf die Web-App über HTTPS erfolgen muss.
   
    Alternativ können Sie das [Authorize](http://msdn.microsoft.com/library/system.web.mvc.authorizeattribute.aspx)-Attribut und das [RequireHttps](http://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx)-Attribut jedem Controller hinzufügen, es wird jedoch als Best Practice im Bereich Sicherheit betrachtet, die Attribute der gesamten Anwendung hinzuzufügen. Auf diese Weise wird jeder neu hinzugefügte Controller und jede neu hinzugefügte Aktionsmethode automatisch geschützt, und Sie müssen nicht daran denken, die Attribute anzuwenden. Weitere Informationen finden Sie unter [Securing your ASP.NET MVC App and the new AllowAnonymous Attribute](http://blogs.msdn.com/b/rickandy/archive/2012/03/23/securing-your-asp-net-mvc-4-app-and-the-new-allowanonymous-attribute.aspx) (in englischer Sprache). 
2. Fügen Sie der [Index](http://blogs.msdn.com/b/rickandy/archive/2012/03/23/securing-your-asp-net-mvc-4-app-and-the-new-allowanonymous-attribute.aspx) -Methode des Controllers "Home" das **AllowAnonymous** -Attribut hinzu. Mit dem [AllowAnonymous](http://blogs.msdn.com/b/rickandy/archive/2012/03/23/securing-your-asp-net-mvc-4-app-and-the-new-allowanonymous-attribute.aspx) -Attribut lassen Sie die Methoden zu, die von der Autorisierung ausgenommen werden sollen. 
   
        public class HomeController : Controller
        {
          [AllowAnonymous]
          public ActionResult Index()
          {
             return View();
          }
   
    Führen Sie eine globale Suche nach *AllowAnonymous* durch. Sie werden feststellen, dass es in den Anmelde- und Registrierungsmethoden des Kontocontrollers verwendet wird.
3. Fügen Sie in der Datei *ContactsController.cs* den Code `[Authorize(Roles = "canEdit")]` zu den HttpGet- und HttpPost-Methoden hinzu, mit denen Daten im Controller *Cm* geändert werden (Erstellen, Bearbeiten, Löschen sowie jede Aktionsmethode abgesehen von Index und Details). Im Folgenden wird ein Teil des fertigen Codes angezeigt: 
   
        // GET: Cm/Create
        [Authorize(Roles = "canEdit")]
        public ActionResult Create()
        {
           return View(new Contact { Address = "123 N 456 W",
            City="Great Falls", Email = "ab@cd.com", Name="Joe Smith", State="MT",
           Zip = "59405"});
        }
        // POST: Cm/Create
        // To protect from overposting attacks, please enable the specific properties you want to bind to, for 
        // more details see http://go.microsoft.com/fwlink/?LinkId=317598.
        [HttpPost]
        [ValidateAntiForgeryToken]
         [Authorize(Roles = "canEdit")]
        public ActionResult Create([Bind(Include = "ContactId,Name,Address,City,State,Zip,Email")] Contact contact)
        {
            if (ModelState.IsValid)
            {
                db.Contacts.Add(contact);
                db.SaveChanges();
                return RedirectToAction("Index");
            }
            return View(contact);
        }
        // GET: Cm/Edit/5
        [Authorize(Roles = "canEdit")]
        public ActionResult Edit(int? id)
        {
            if (id == null)
            {
                return new HttpStatusCodeResult(HttpStatusCode.BadRequest);
            }
            Contact contact = db.Contacts.Find(id);
            if (contact == null)
            {
                return HttpNotFound();
            }
            return View(contact);
        }
4. Drücken Sie STRG+F5, um die Anwendung auszuführen.
5. Wenn Sie immer noch von einer vorherigen Sitzung angemeldet sind, klicken Sie auf den Link **Abmelden** .
6. Klicken Sie auf den Link **About** oder **Contact**. Sie werden zur Anmeldeseite weitergeleitet, da anonyme Benutzer diese Seiten nicht anzeigen können.
7. Klicken Sie auf den Link **Als neuer Benutzer registrieren**, und fügen Sie einen lokalen Benutzer mit der E-Mail-Adresse *joe@contoso.com* hinzu. Stellen Sie sicher, dass *Joe* die Start-, Info- und Kontaktseiten anzeigen kann. 
   
    ![Anmeldung](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/ss14.PNG)
8. Klicken Sie auf den Link *CM Demo* , und überprüfen Sie, ob die Daten sichtbar sind.
9. Klicken Sie auf einen Bearbeitungslink auf der Seite. Sie werden zur Anmeldeseite weitergeleitet (da der Rolle *canEdit* kein neuer lokaler Benutzer hinzugefügt wurde).
10. Melden Sie sich als *user1@contoso.com* mit dem Kennwort „P_assw0rd1“ an (das „0“ in „word“ ist eine Null). Sie werden zu der Bearbeitungsseite weitergeleitet, die Sie zuvor ausgewählt haben. 
11.  Wenn Sie sich nicht mit diesem Konto und Kennwort anmelden können, versuchen Sie, das Kennwort aus dem Quellcode zu kopieren und einzufügen. Sollten Sie sich dennoch nicht anmelden können, überprüfen Sie in der Spalte **UserName** der Tabelle **AspNetUsers**, ob *user1@contoso.com* wirklich hinzugefügt wurde. 
12. Stellen Sie sicher, dass Sie Daten ändern können.

## <a name="deploy-the-app-to-azure"></a>Bereitstellen der Anwendung in Azure
1. Klicken Sie im **Projektmappen-Explorer** von Visual Studio mit der rechten Maustaste auf das Projekt, und wählen Sie im Kontextmenü **Veröffentlichen** aus.
   
    !["Veröffentlichen" im Kontextmenü des Projekts][firsdeploy003]
   
    Der Assistent **Web veröffentlichen** wird geöffnet.
2. Klicken Sie im Dialogfeld **Web veröffentlichen** auf der linken Seite auf **Einstellungen**. 
3. Wählen Sie unter **ApplicationDbContext** die Datenbank aus, die Sie zusammen mit dem Projekt erstellt haben.
4. Wählen Sie unter **ContactManagerContext** die Option **Code First-Migrationen ausführen** aus.
   
    ![Einstellungen](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/rrc2.png)
5. Klicken Sie auf **Veröffentlichen**.
6. Melden Sie sich als *user1@contoso.com* mit dem Kennwort „P_assw0rd1“ an, und überprüfen Sie, ob Sie die Daten bearbeiten können.
7. Melden Sie sich ab.
8. Wechseln Sie zur [Google Developers Console](https://console.developers.google.com/) , und aktualisieren Sie auf der Registerkarte **Anmeldeinformationen** die Umleitungs-URIs und JavaScript-Ursprünge so , dass die Azure-URL verwendet wird.
9. Melden Sie sich über Google oder Facebook an. Damit wird das Google- oder Facebook-Konto der Rolle **canEdit** hinzugefügt. Wenn der HTTP 400-Fehler mit der Meldung *The redirect URI in the request: https://contactmanager{meine Version}.azurewebsites.net/signin-google did not match a registered redirect URI* angezeigt wird, müssen Sie warten, bis Ihre Änderungen übermittelt wurden. Wenn dieser Fehler auch nach mehreren Minuten noch angezeigt wird, überprüfen Sie die URIs.

### <a name="stop-the-web-app-to-prevent-other-people-from-registering"></a>Beenden der Web-App, um die Registrierung anderer Personen zu verhindern
1. Navigieren Sie in **Server-Explorer** zu **Azure > App Service > {Ihre Ressourcengruppe} > {Ihre Web-App}**.
2. Klicken Sie mit der rechten Maustaste auf die Web-App, und wählen Sie **Beenden**aus. 
   
    Sie können alternativ im [Azure-Portal](https://portal.azure.com/)zum Blatt der Web-App wechseln und dann oben auf dem Blatt auf das Symbol **Beenden** klicken.
   
    ![Web-App-Portal beenden](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/stopweb.png)

### <a name="remove-addtoroleasync-publish-and-test"></a>Entfernen von "AddToRoleAsync", "Publish" und "Test"
1. Kommentieren Sie den folgenden Code aus der **ExternalLoginConfirmation** -Methode im Kontocontroller aus, oder entfernen Sie ihn:
   
        await UserManager.AddToRoleAsync(user.Id, "canEdit");
2. Erstellen Sie das Projekt (die Dateiänderungen werden dabei gespeichert und es wird sichergestellt, dass keine Kompilierungsfehler vorhanden sind).
3. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und wählen Sie **Veröffentlichen** aus.
   
       ![Publish in project context menu](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/GS13publish.png)
4. Klicken Sie auf die Schaltfläche **Vorschau starten** . Es werden nur die Dateien bereitgestellt, die aktualisiert werden müssen.
5. Starten Sie die Web-App in Visual Studio oder über das Portal. **Eine Veröffentlichung ist bei angehaltener Web-App nicht möglich**.
   
    ![Web-App starten](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/ss15.png)
6. Kehren Sie zu Visual Studio zurück, und klicken Sie auf **Veröffentlichen**.
7. Die Azure-Anwendung wird im Standardbrowser geöffnet. Wenn Sie angemeldet sind, melden Sie sich ab, sodass Sie die Startseite als anonymer Benutzer anzeigen können.  
8. Klicken Sie auf den Link **About** . Sie werden zur Anmeldeseite weitergeleitet.
9. Klicken Sie auf den Link **Registrieren** auf der Anmeldeseite, und erstellen Sie ein lokales Konto. Dieses lokale Konto wird verwendet, um zu überprüfen, ob Sie auf die schreibgeschützten Seiten zugreifen können, jedoch nicht auf die Seiten mit Datenänderungen (die durch die Rolle *canEdit* geschützt sind). An späterer Stelle in diesem Tutorial entfernen Sie den lokalen Kontozugriff. 
   
    ![Registrieren](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/ss16.PNG)
10. Stellen Sie sicher, dass Sie zu den Seiten *Info* und *Kontakt* navigieren können.
    
     ![Abmelden](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/ss17.PNG)
11. Klicken Sie auf den Link **CM Demo**, um zum Controller **Cm** zu navigieren. Alternativ können Sie *Cm* an die URL anhängen. 
    
     ![CM-Seite](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/rrr4.png)
12. Klicken Sie auf einen Bearbeitungslink. 
    
     Sie werden zur Anmeldeseite umgeleitet. 
13. Klicken Sie unter **Use another service to log in**auf Google oder Facebook, und melden Sie sich mit dem zuvor registrierten Konto an. (Wenn Sie schnell arbeiten und das Sitzungscookie noch nicht abgelaufen ist, werden Sie automatisch mit dem zuvor verwendeten Google- oder Facebook-Konto angemeldet.)
14. Stellen Sie sicher, dass Sie Daten bearbeiten können, während Sie beim Konto angemeldet sind.
    
     **Hinweis:** Sie können sich von dieser App aus nicht von Google abmelden und mit demselben Browser bei einem anderen Google-Konto anmelden. Wenn Sie einen einzigen Browser verwenden, müssen Sie zu Google navigieren und sich abmelden. Sie können sich über den Drittanbieterauthentifikator (wie Google) mithilfe eines anderen Browsers anmelden.
    
     Falls Sie Ihren Vornamen und Nachnamen im Google-Konto nicht ausgefüllt haben, tritt ein NullReferenceException-Ausnahmefehler auf.

## <a name="examine-the-sql-azure-db"></a>Überprüfen der SQL Azure-DB
1. Navigieren Sie in **Server-Explorer** zu **Azure > SQL-Datenbanken > {Ihre Datenbank}**
2. Klicken Sie mit der rechten Maustaste auf Ihre Datenbank, und wählen Sie **In SQL Server-Objekt-Explorer öffnen**aus.
   
    ![In SSOX öffnen](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/rrr12.png)
3. Bei der erstmaligen Verbindung mit dieser Datenbank werden Sie unter Umständen zum Hinzufügen einer Firewallregel aufgefordert, die den Zugriff Ihrer aktuellen IP-Adresse erlaubt. Die IP-Adresse ist bereits eingetragen. Klicken Sie einfach auf **Firewallregel hinzufügen** , um den Zugriff zu erlauben.
   
    ![Firewallregel hinzufügen](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/addfirewallrule.png)
4. Melden Sie sich bei der Datenbank mit dem Benutzernamen und Kennwort an, das Sie bei der Erstellung der Datenbank angegeben haben. 
5. Klicken Sie mit der rechten Maustaste auf die Tabelle **AspNetUsers**, und wählen Sie **Daten anzeigen** aus.
   
    ![CM-Seite](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/rrr8.png)
6. Beachten Sie, dass sich die ID des Google-Kontos, bei dem Sie sich registriert haben, und die ID *user1@contoso.com* in der Rolle **canEdit** befinden. Dies sollten die einzigen Benutzer in der Rolle **canEdit** sein. (Wird im nächsten Schritt überprüft.)
   
    ![CM-Seite](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/s2.png)
7. Klicken Sie im **SQL Server-Objekt-Explorer** mit der rechten Maustaste auf **AspNetUserRoles**, und wählen Sie **Daten anzeigen** aus.
   
    ![CM-Seite](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/rs1.png)
8. Stellen Sie sicher, dass die **UserId** von *user1@contoso.com* und dem registrierten Google-Konto stammt. 

## <a name="troubleshooting"></a>Problembehandlung
Falls Probleme auftreten, finden Sie hier einige Vorschläge für Lösungsansätze.

* Fehler bei der Bereitstellung der SQL-Datenbank – Stellen Sie sicher, dass das aktuelle SDK installiert ist. Versionen vor 2.8.1 haben einen Programmierfehler, der in einigen Szenarien Fehler verursacht, wenn Visual Studio versucht, den Datenbankserver oder die Datenbank zu erstellen.
* Fehlermeldung „Der Vorgang wird für Ihren Abonnementangebotstyp nicht unterstützt“ beim Erstellen von Azure-Ressourcen – Siehe oben.
* Fehler bei der Bereitstellung – Erwägen Sie, den Artikel [Erstellen von ASP.NET-Web-Apps in Azure App Service](app-service-web-get-started-dotnet.md) durchzugehen. Dieses Bereitstellungsszenario ist einfacher. Wenn Sie dort das gleiche Problem haben, ist es möglicherweise einfacher zu isolieren. Beispielsweise kann in bestimmten Unternehmensumgebungen eine Unternehmensfirewall Web Deploy am Herstellen erforderlicher Verbindungen mit Azure hindern.
* Keine Option zum Auswählen der Verbindungszeichenfolge im Assistenten „Web veröffentlichen“ bei der Bereitstellung – Wenn Sie eine andere Methode zum Erstellen Ihrer Azure-Ressourcen verwendet haben (z. B. versuchen, eine im Portal erstellte Web-App und SQL-Datenbank bereitzustellen), ist die SQL-Datenbank ggf. nicht der Web-App zugeordnet. Die einfachste Lösung ist das Erstellen einer neuen Web-App und Datenbank mit Visual Studio, wie in diesem Tutorial gezeigt. Sie müssen das Tutorial nicht von vorn durchlaufen. Im Assistenten „Web veröffentlichen“ finden Sie die Option zum Erstellen einer neuen Web-App, über die Ihnen dasselbe Dialogfeld zum Erstellen von Azure-Ressourcen wie beim Erstellen des Projekts angezeigt wird.
* Anweisungen für das Google- und Facebook-Entwicklerportal sind veraltet. Weitere Informationen finden Sie im Disqus-Kommentar am Ende dieses Tutorials.

## <a name="next-steps"></a>Nächste Schritte
Sie haben eine einfache ASP.NET MVC-Web-Anwendung erstellt, die Benutzer authentifiziert. Weitere Informationen zu allgemeinen Authentifizierungsaufgaben und zum Schutz vertraulicher Daten finden Sie in den folgenden Tutorials.

* [Erstellen einer sicheren ASP.NET MVC 5-Webanwendung mit Anmeldung, E-Mail-Bestätigung und Kennwortzurücksetzung](http://www.asp.net/mvc/overview/getting-started/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset)
* [ASP.NET MVC 5-Anwendung mit SMS und Two-Factor Authentication über E-Mail](http://www.asp.net/mvc/overview/getting-started/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication)
* [Best Practices für das Bereitstellen von Kennwörtern und anderen vertraulichen Daten auf ASP.NET und Azure](http://www.asp.net/identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure) 
* [Erstellen einer ASP.NET MVC 5-App mit Facebook und Google OAuth2](http://www.asp.net/mvc/tutorials/mvc-5/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on) Dieser Artikel enthält Anweisungen zum Hinzufügen von Profildaten zur Benutzerregistrierungsdatenbank sowie ausführliche Anweisungen zur Verwendung von Facebook als Authentifizierungsanbieter.
* [Erste Schritte mit ASP.NET MVC 5](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started)

Ein erweitertes Tutorial zur Verwendung von Entity Framework finden Sie unter [Erste Schritte mit EF und MVC](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application).

Dieses Tutorial wurden von [Rick Anderson](http://blogs.msdn.com/b/rickandy/) (Twitter [@RickAndMSFT](https://twitter.com/RickAndMSFT)) sowie Tom Dykstra und Barry Dorrans (Twitter [@blowdart](https://twitter.com/blowdart)) geschrieben. 

***Bitte teilen Sie uns mit***, was Ihrer Meinung nach beim Tutorial wie auch den darin vorgestellten Produkten gelungen ist und was vielleicht verbessert werden kann. Ihr Feedback unterstützt uns dabei, eine entsprechende Priorität für Verbesserungen festzulegen. Unter [Show Me How With Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code) (in englischer Sprache) können Sie neue Themen anfordern oder eine Stimme dafür abgeben.

## <a name="whats-changed"></a>Änderungen
* Hinweise zu den Änderungen von Websites zum App Service finden Sie unter: [Azure App Service und vorhandene Azure-Dienste](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- bookmarks -->
[Add an OAuth Provider]: #addOauth
[Using the Membership API]:#mbrDB
[Create a Data Deployment Script]:#ppd
[Update the Membership Database]:#ppd2

[setupwindowsazureenv]: #bkmk_setupwindowsazure
[createapplication]: #bkmk_createmvc4app
[deployapp1]: #bkmk_deploytowindowsazure1
[deployapp11]: #bkmk_deploytowindowsazure11
[adddb]: #bkmk_addadatabase


<!-- images-->
[rx2]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/rx2.png

[rx5]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/rx5.png
[rx6]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/rx6.png
[rx7]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/rx7.png
[rx8]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/rx8.png
[rx9]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/rx9.png

[rxb]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/rxb.png


[rxSSL]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/rxSSL.png

[rxNOT]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/rxNOT.png
[rxNOT2]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/rxNOT2.png

[rxNOT]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/rxNOT.png
[rxNOT]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/rxNOT.png
[rxNOT]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/rxNOT.png
[rr1]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/rr1.png

[rxPrevDB]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/rxPrevDB.png

[rxWSnew]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/rxWSnew2.png
[rxCreateWSwithDB]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/rxCreateWSwithDB.png

[setup007]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/dntutmobile-setup-azure-site-004.png

[newapp004]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/dntutmobile-createapp-004.png

[firsdeploy003]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/dntutmobile-deploy1-publish-001.png

[adddb002]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/dntutmobile-adddatabase-002.png
[addcode001]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/dntutmobile-controller-add-context-menu.png

[addcode008]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/dntutmobile-migrations-package-manager-menu.png
[addcode009]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/dntutmobile-migrations-package-manager-console.png


[Important information about ASP.NET in Azure web apps]: #aspnetwindowsazureinfo
[Next steps]: #nextsteps

[ImportPublishSettings]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/ImportPublishSettings.png


