---
title: Django und SQL-Datenbank in Azure mit Python Tools 2.2 für Visual Studio
description: Erfahren Sie, wie Sie mithilfe der Python-Tools für Visual Studio eine Django-Web-App erstellen, die Daten in einer SQL-Datenbankinstanz speichert, und die Web-App für Azure App Service-Web-Apps bereitstellen.
services: app-service\web
tags: python
documentationcenter: python
author: huguesv
manager: wpickett
editor: ''

ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 07/07/2016
ms.author: huguesv

---
# Django und SQL-Datenbank in Azure mit Python Tools 2.2 für Visual Studio
In diesem Tutorial erstellen wir mit [Python-Tools für Visual Studio] eine einfache Web-App für Umfragen mithilfe einer der PTVS-Beispielvorlagen. Dieses Tutorial steht auch als [Video](https://www.youtube.com/watch?v=ZwcoGcIeHF4) zur Verfügung.

Sie erfahren, wie Sie eine in Azure gehostete SQL-Datenbank verwenden, wie Sie die Web-App für die Nutzung einer SQL-Datenbank konfigurieren und wie Sie sie für [Azure App Service-Web-Apps](http://go.microsoft.com/fwlink/?LinkId=529714) veröffentlichen.

Weitere Artikel finden Sie im [Python Developer Center]. Diese Artikel behandeln die Entwicklung von Azure App Service-Web-Apps mit PTVS unter Verwendung der Webframeworks Bottle, Flask und Django sowie mithilfe der Dienste Azure Table Storage, MySQL und SQL-Datenbank. Der Schwerpunkt dieses Artikels liegt zwar auf App Service, die Schritte sind jedoch vergleichbar mit der Entwicklung von [Azure Cloud Services].

## Voraussetzungen
* Visual Studio 2015
* [Python 2.7 (32 Bit)]
* [Python Tools 2.2 für Visual Studio]
* [Python Tools 2.2 für Visual Studio, Beispiel-VSIX]
* [Azure SDK-Tools für Visual Studio 2015]
* Django 1.9 oder höher

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> Wenn Sie Azure App Service ausprobieren möchten, ehe Sie sich für ein Azure-Konto anmelden, können Sie unter [App Service testen](http://go.microsoft.com/fwlink/?LinkId=523751) sofort kostenlos eine kurzlebige Starter-Web-App in App Service erstellen. Keine Kreditkarte erforderlich, keine Verpflichtungen.
> 
> 

## Erstellen des Projekts
In diesem Abschnitt erstellen wir ein Visual Studio-Projekt mithilfe einer Beispielvorlage. Wir erstellen eine virtuelle Umgebung und installieren die erforderlichen Pakete. Wir erstellen eine lokale Datenbank mithilfe von SQLite. Anschließend führen wir die Web-App lokal aus.

1. Wählen Sie in Visual Studio **Datei** -> **Neues Projekt** aus.
2. Die Projektvorlagen aus den [Python Tools 2.2 für Visual Studio, Beispiel-VSIX] sind unter **Python** > **Beispiele** verfügbar. Wählen Sie **Polls Django Web Project** aus, und klicken Sie auf "OK", um das Projekt zu erstellen.
   
     ![Dialogfeld "Neues Projekt"](./media/web-sites-python-ptvs-django-sql/PollsDjangoNewProject.png)
3. Sie werden aufgefordert, externe Pakete zu installieren. Wählen Sie **In einer virtuellen Umgebung installieren** aus.
   
     ![Dialogfeld für externe Pakete](./media/web-sites-python-ptvs-django-sql/PollsDjangoExternalPackages.png)
4. Wählen Sie **Python 2.7** als Basisinterpreter aus.
   
     ![Dialogfeld für das Hinzufügen der virtuellen Umgebung](./media/web-sites-python-ptvs-django-sql/PollsCommonAddVirtualEnv.png)
5. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Projektknoten, und wählen Sie **Python** und anschließend **Django Migrate**. Wählen Sie dann **Django Create Superuser**.
6. Dadurch wird eine Django-Verwaltungskonsole geöffnet und im Projektordner eine SQLite-Datenbank erstellt. Folgen Sie den Anweisungen zum Erstellen eines Benutzers.
7. Vergewissern Sie sich, dass die App funktioniert, indem Sie <kbd>F5</kbd> drücken.
8. Klicken Sie oben auf der Navigationsleiste auf **Anmelden**.
   
     ![Webbrowser](./media/web-sites-python-ptvs-django-sql/PollsDjangoCommonBrowserLocalMenu.png)
9. Geben Sie die Anmeldeinformationen für den Benutzer ein, den Sie beim Synchronisieren der Datenbank erstellt haben.
   
     ![Webbrowser](./media/web-sites-python-ptvs-django-sql/PollsDjangoCommonBrowserLocalLogin.png)
10. Klicken Sie auf **Beispielumfrage erstellen**.
    
      ![Webbrowser](./media/web-sites-python-ptvs-django-sql/PollsDjangoCommonBrowserNoPolls.png)
11. Klicken Sie auf eine Umfrage und stimmen Sie ab.
    
      ![Webbrowser](./media/web-sites-python-ptvs-django-sql/PollsDjangoSqliteBrowser.png)

## Erstellen einer SQL-Datenbank
Für die Datenbank erstellen Sie eine Azure SQL-Datenbank.

Mit den folgenden Schritten können Sie eine Datenbank erstellen.

1. Melden Sie sich beim [Azure-Portal] an.
2. Klicken Sie am unteren Rand des Navigationsbereichs auf **NEU**. Klicken Sie anschließend auf **Daten + Speicher** > **SQL-Datenbank**.
3. Konfigurieren Sie die neue SQL-Datenbank, indem Sie eine neue Ressourcengruppe erstellen und den entsprechenden Speicherort auswählen.
4. Klicken Sie nach der Erstellung der SQL-Datenbank auf dem Datenbankblatt auf **In Visual Studio öffnen**.
5. Klicken Sie auf **Firewall konfigurieren**.
6. Fügen Sie auf dem Blatt **Firewalleinstellungen** eine Firewall-Regel hinzu, und konfigurieren Sie sie mit der **START-IP** und **END-IP** der öffentlichen IP-Adresse Ihres Entwicklungscomputers. Klicken Sie auf **Speichern**.
   
   Damit sind Verbindungen von Ihrem Entwicklungsrechner zum Datenbankserver möglich.
7. Klicken Sie auf dem Datenbankblatt auf **Eigenschaften** und dann auf **Verbindungszeichenfolgen anzeigen**.
8. Mithilfe der Schaltfläche "Kopieren" können Sie den Wert für **ADO.NET** in die Zwischenablage kopieren.

## Konfigurieren des Projekts
In diesem Abschnitt konfigurieren wir unsere Web-App, sodass sie die soeben erstellte SQL-Datenbank verwendet. Wir installieren außerdem weitere Python-Pakete, die für die Verwendung von SQL-Datenbanken mit Django erforderlich sind. Anschließend führen wir die Web-App lokal aus.

1. Öffnen Sie in Visual Studio die Datei **settings.py** (im Ordner *<Projektname>*). Fügen Sie die Verbindungszeichenfolge temporär in den Editor ein. Die Verbindungszeichenfolge ist in folgendem Format:
   
       Server=<ServerName>,<ServerPort>;Database=<DatabaseName>;User ID=<UserName>;Password={your_password_here};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;

Bearbeiten Sie die Definition von `DATABASES`, sodass die oben angegebenen Werte verwendet werden:

        DATABASES = {
            'default': {
                'ENGINE': 'sql_server.pyodbc',
                'NAME': '<DatabaseName>',
                'USER': '<UserName>',
                'PASSWORD': '{your_password_here}',
                'HOST': '<ServerName>',
                'PORT': '<ServerPort>',
                'OPTIONS': {
                    'driver': 'SQL Server Native Client 11.0',
                    'MARS_Connection': 'True',
                }
            }
        }

1. Klicken Sie im Projektmappen-Explorer unter **Python-Umgebungen** mit der rechten Maustaste auf die virtuelle Umgebung, und wählen Sie **Python-Paket installieren** aus.
2. Installieren Sie das Paket `pyodbc` mithilfe von **pip**.
   
     ![Dialogfeld zur Installation des Python-Pakets](./media/web-sites-python-ptvs-django-sql/PollsDjangoSqlInstallPackagePyodbc.png)
3. Installieren Sie das Paket `django-pyodbc-azure` mithilfe von **pip**.
   
     ![Dialogfeld zur Installation des Python-Pakets](./media/web-sites-python-ptvs-django-sql/PollsDjangoSqlInstallPackageDjangoPyodbcAzure.png)
4. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Projektknoten, und wählen Sie **Python** und anschließend **Django Migrate**. Wählen Sie dann **Django Create Superuser**.
   
   Damit werden die Tabellen für die SQL-Datenbank erstellt, die wir im vorherigen Abschnitt angelegt haben. Folgen Sie den Aufforderungen zum Erstellen eines Benutzers. Er muss nicht demjenigen der im ersten Abschnitt angelegten SQLite-Datenbank entsprechen.
5. Drücken Sie `F5`, um die Anwendung auszuführen. Die mittels **Beispielumfrage erstellen** erstellten Umfragen sowie die bei der Abstimmung erfassten Daten werden in der SQL-Datenbank serialisiert.

## Veröffentlichen der Web-App in Azure App Service
Das Azure .NET SDK bietet eine einfache Möglichkeit zum Bereitstellen Ihrer Web-App in Azure App Service-Web-Apps.

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektknoten, und wählen Sie **Veröffentlichen** aus.
   
     ![Dialogfeld "Web veröffentlichen"](./media/web-sites-python-ptvs-django-sql/PollsCommonPublishWebSiteDialog.png)
2. Klicken Sie auf **Microsoft Azure-Web-Apps**.
3. Klicken Sie auf **Neu**, um eine neue Web-App zu erstellen.
4. Füllen Sie die folgenden Felder aus, und klicken Sie auf **Erstellen**.
   
   * **Name der Web-App**
   * **App Service-Plan**
   * **Ressourcengruppe**
   * **Region**
   * Lassen Sie für **Datenbankserver** die Option **Keine Datenbank** festlegt.
5. Übernehmen Sie alle anderen Standardwerte, und klicken Sie auf **Veröffentlichen**.
6. Ihr Webbrowser öffnet automatisch mit der veröffentlichten Web-App. Die Web-App funktioniert nun wie erwartet und verwendet die von Azure gehostete **SQL**-Datenbank.
   
   Glückwunsch!
   
     ![Webbrowser](./media/web-sites-python-ptvs-django-sql/PollsDjangoAzureBrowser.png)

## Nächste Schritte
Folgen Sie diesen Links, wenn Sie mehr über Python Tools für Visual Studio, Django und SQL-Datenbank erfahren möchten.

* [Python Tools für Visual Studio – Dokumentation]
  * [Webprojekte]
  * [Cloud Service-Projekte]
  * [Remotedebugging in Microsoft Azure]
* [Dokumentation zu Django]
* [SQL-Datenbank]

## Änderungen
* Hinweise zu den Änderungen von Websites zum App Service finden Sie unter: [Azure App Service und vorhandene Azure-Dienste](http://go.microsoft.com/fwlink/?LinkId=529714).

<!--Link references-->
[Python Developer Center]: /develop/python/
[Azure Cloud Services]: ../cloud-services-python-ptvs.md

<!--External Link references-->
[Azure-Portal]: https://portal.azure.com
[Python-Tools für Visual Studio]: http://aka.ms/ptvs
[Python Tools 2.2 für Visual Studio]: http://go.microsoft.com/fwlink/?LinkID=624025
[Python Tools 2.2 für Visual Studio, Beispiel-VSIX]: http://go.microsoft.com/fwlink/?LinkID=624025
[Python Tools 2.2 für Visual Studio, Beispiel-VSIX]: http://go.microsoft.com/fwlink/?LinkID=624025
[Azure SDK-Tools für Visual Studio 2015]: http://go.microsoft.com/fwlink/?LinkId=518003
[Python 2.7 (32 Bit)]: http://go.microsoft.com/fwlink/?LinkId=517190
[Python Tools für Visual Studio – Dokumentation]: http://aka.ms/ptvsdocs
[Remotedebugging in Microsoft Azure]: http://go.microsoft.com/fwlink/?LinkId=624026
[Webprojekte]: http://go.microsoft.com/fwlink/?LinkId=624027
[Cloud Service-Projekte]: http://go.microsoft.com/fwlink/?LinkId=624028
[Dokumentation zu Django]: https://www.djangoproject.com/
[SQL-Datenbank]: /documentation/services/sql-database/

<!---HONumber=AcomDC_0713_2016-->