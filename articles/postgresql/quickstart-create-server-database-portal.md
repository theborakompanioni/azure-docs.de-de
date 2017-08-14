---
title: "Azure-Portal: Erstellen einer Azure-Datenbank für PostgreSQL | Microsoft-Dokumentation"
description: "Schnellstart-Anleitung zum Erstellen und Verwalten von Azure-Datenbank für PostgreSQL-Server über die Benutzeroberfläche des Azure-Portals."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql-database
ms.custom: mvc
ms.topic: hero-article
ms.date: 08/04/2017
ms.translationtype: HT
ms.sourcegitcommit: 0aae2acfbf30a77f57ddfbaabdb17f51b6938fd6
ms.openlocfilehash: e89058a500aeb542ae4c7dc6bb4a9b9ae54db7f2
ms.contentlocale: de-de
ms.lasthandoff: 08/09/2017

---

# <a name="create-an-azure-database-for-postgresql-in-the-azure-portal"></a>Erstellen einer Azure-Datenbank für PostgreSQL im Azure-Portal

Azure-Datenbank für PostgreSQL ist ein verwalteter Dienst, mit dem Sie hochverfügbare PostgreSQL-Datenbanken in der Cloud ausführen, verwalten und skalieren können. In diesem Schnellstart wird veranschaulicht, wie Sie eine Azure-Datenbank für PostgreSQL-Server über das Azure-Portal erstellen.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="log-in-to-the-azure-portal"></a>Anmelden beim Azure-Portal

Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.

## <a name="create-an-azure-database-for-postgresql"></a>Erstellen einer Azure-Datenbank für PostgreSQL

Eine Azure-Datenbank für PostgreSQL-Server wird mit einer definierten Gruppe von [Compute- und Speicherressourcen](./concepts-compute-unit-and-storage.md) erstellt. Der Server wird in einer [Azure-Ressourcengruppe](../azure-resource-manager/resource-group-overview.md) erstellt.

Führen Sie die folgenden Schritte aus, um eine Azure-Datenbank für PostgreSQL-Server zu erstellen:
1.  Klicken Sie in der linken oberen Ecke des Azure-Portals auf die Schaltfläche **Neu** (+).
2.  Wählen Sie auf der Seite **Neu** die Option **Datenbanken** und dann auf der Seite **Datenbanken** die Option **Azure-Datenbank für PostgreSQL** aus.
 ![Azure-Datenbank für PostgreSQL – Erstellen der Datenbank](./media/quickstart-create-database-portal/1-create-database.png)

3.  Geben Sie die folgenden Informationen in das Formular für den neuen Server ein, wie in der obigen Abbildung dargestellt:

    Einstellung|Empfohlener Wert|Beschreibung
    ---|---|---
    Servername |*mypgserver-20170401*|Wählen Sie einen eindeutigen Namen, der Ihre Azure-Datenbank für PostgreSQL-Server identifiziert. Der Domänenname *postgres.database.azure.com* wird an den Servernamen angefügt, den Sie für Anwendungen angeben, damit diese eine Verbindung damit herstellen können. Der Servername darf nur Kleinbuchstaben, Zahlen und den Bindestrich (-) enthalten und muss zwischen drei und 63 Zeichen lang sein.
    Abonnement|*Ihr Abonnement*|Das Azure-Abonnement, das Sie für Ihren Server verwenden möchten. Falls Sie über mehrere Abonnements verfügen, wählen Sie das Abonnement aus, über das die Ressource abgerechnet wird.
    Ressourcengruppe|*myresourcegroup*| Sie können entweder einen neuen Ressourcengruppennamen erstellen oder einen bereits vorhandenen Namen aus Ihrem Abonnement verwenden.
    Serveradministratoranmeldung |*mylogin*| Erstellen Sie Ihr eigenes Anmeldekonto für die Verbindungsherstellung mit dem Server. Der Administratoranmeldename darf nicht „azure_superuser“, „azure_pg_admin“, „admin“, „administrator“, „root“, „guest“ oder „public“ lauten und nicht mit „pg_“ beginnen.
    Kennwort |*Beliebig* | Erstellen Sie ein neues Kennwort für das Serveradministratorkonto. Es muss zwischen 8 und 128 Zeichen lang sein. Ihr Kennwort muss Zeichen aus drei der folgenden Kategorien enthalten: englische Großbuchstaben, englische Kleinbuchstaben, Zahlen (0-9) und nicht alphanumerische Zeichen (!, $, #, % usw.).
    Standort|*Die Region, die Ihren Benutzern am nächsten liegt*| Wählen Sie den Standort aus, der Ihren Benutzern am nächsten ist.
    PostgreSQL-Version|*Wählen Sie die neueste Version aus.*| Wählen Sie die neueste Version aus (es sei denn, Sie haben besondere Anforderungen).
    Preisstufe | **Basic**, **50 Compute-Einheiten** **50 GB** | Klicken Sie auf **Tarif**, um die Dienstebene und die Leistungsstufe für die neue Datenbank anzugeben. Wählen Sie auf der oberen Registerkarte den Tarif „Basic“ aus. Klicken Sie auf das linke Ende des Schiebereglers „Compute-Einheiten“, um für diese Schnellstartanleitung den kleinstmöglichen Wert festzulegen. Klicken Sie auf **OK**, um die Tarifauswahl zu speichern. Der folgende Screenshot zeigt dies.
    | An Dashboard anheften | Prüfen | Aktivieren Sie die Option **An Dashboard anheften**, um Ihren Server komfortabel auf der Titelseite des Dashboards im Azure-Portal nachverfolgen zu können.

  > [!IMPORTANT]
  > Der hier angegebene Benutzername bzw. das Kennwort für den Serveradministrator sind erforderlich, um später in diesem Schnellstart die Anmeldung am Server und an den dazugehörigen Datenbanken durchzuführen. Behalten Sie diese Angaben im Kopf, oder notieren Sie sie zur späteren Verwendung.

    ![Azure-Datenbank für PostgreSQL – Auswählen des Tarifs](./media/quickstart-create-database-portal/2-service-tier.png)

4.  Klicken Sie auf **Erstellen**, um den Server bereitzustellen. Die Bereitstellung kann bis zu 20 Minuten dauern.

5.  Klicken Sie in der Symbolleiste auf **Benachrichtigungen**, um den Bereitstellungsprozess zu überwachen.
 ![Azure-Datenbank für PostgreSQL – Benachrichtigungen anzeigen](./media/quickstart-create-database-portal/3-notifications.png)
   
  Standardmäßig wird die **postgres**-Datenbank unter dem Server erstellt. Die [postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html)-Datenbank ist eine Standarddatenbank für die Verwendung durch Benutzer, Hilfsprogramme und Drittanbieteranwendungen. 

## <a name="configure-a-server-level-firewall-rule"></a>Konfigurieren einer Firewallregel auf Serverebene

Der Azure-Datenbank für PostgreSQL-Dienst erstellt eine Firewall auf der Serverebene. Diese Firewall verhindert, dass externe Anwendungen und Tools eine Verbindung mit dem Server und Datenbanken auf dem Server herstellen – sofern keine Firewallregel erstellt wird, um die Firewall für bestimmte IP-Adressen zu öffnen. 

1.  Navigieren Sie nach Abschluss der Bereitstellung zu Ihrem Server. Bei Bedarf können Sie danach suchen. Klicken Sie beispielsweise im linken Menü auf **Alle Ressourcen**, und geben Sie den Servernamen (beispielsweise „mypgserver-20170401“) ein, um nach dem neu erstellten Server zu suchen. Klicken Sie in der Suchergebnisliste auf den Namen Ihres Servers. Die Seite „Übersicht“ für Ihren Server mit Optionen für die weitere Konfiguration wird geöffnet.
 
    ![Azure-Datenbank für PostgreSQL – Suchen nach dem Server ](./media/quickstart-create-database-portal/4-locate.png)

2.  Wählen Sie auf der Serverseite die Option **Verbindungssicherheit** aus. 
    ![Azure-Datenbank für PostgreSQL – Erstellen einer Firewallregel](./media/quickstart-create-database-portal/5-firewall-2.png)

3.  Klicken Sie unter der Überschrift **Firewallregeln** in der Spalte **Regelname** auf das leere Textfeld, um mit der Erstellung der Firewallregel zu beginnen. 

    Im Rahmen dieser Schnellstartanleitung lassen wir für den Server alle IP-Adressen zu, indem wir das Textfeld in jeder Spalte mit folgenden Werten ausfüllen:

    Regelname | Start-IP | End-IP 
    ---|---|---
    AllowAllIps |  0.0.0.0 | 255.255.255.255

4. Klicken Sie auf der oberen Symbolleiste der Seite „Verbindungssicherheit“ auf **Speichern**. Klicken Sie anschließend auf das **X**, um die Seite „Verbindungssicherheit“ zu schließen.

    > [!NOTE]
    > Der Azure-PostgreSQL-Server kommuniziert über Port 5432. Wenn Sie versuchen, eine Verbindung aus einem Unternehmensnetzwerk heraus herzustellen, wird der ausgehende Datenverkehr über Port 5432 von der Firewall Ihres Netzwerks unter Umständen nicht zugelassen. In diesem Fall können Sie nur dann eine Verbindung mit Ihrem Azure SQL-Datenbankserver herstellen, wenn Ihre IT-Abteilung Port 5432 öffnet.
  >

## <a name="get-the-connection-information"></a>Abrufen der Verbindungsinformationen

Wenn wir unseren Azure-Datenbank für PostgreSQL-Server erstellt haben, wird eine Standarddatenbank namens **postgres** erstellt. Für die Verbindungsherstellung mit Ihrem Datenbankserver benötigen Sie den vollständigen Servernamen und die Administratoranmeldeinformationen. Diese Werte haben Sie möglicherweise zuvor im Rahmen dieses Schnellstartartikels notiert. Falls nicht, finden Sie den Servernamen und die Anmeldeinformationen auf der Serverübersichtsseite im Azure-Portal.

1. Öffnen Sie die Seite **Übersicht** für Ihren Server. Notieren Sie sich den **Servernamen** und den **Anmeldenamen des Serveradministrators**.
    Zeigen Sie mit dem Cursor auf das jeweilige Feld. Daraufhin erscheint rechts neben dem Text ein Kopiersymbol. Klicken Sie auf das Kopiersymbol, um die Werte zu kopieren.

 ![Azure-Datenbank für PostgreSQL-Server – Anmeldename des Serveradministrators](./media/quickstart-create-database-portal/6-server-name.png)

## <a name="connect-to-postgresql-database-using-psql-in-cloud-shell"></a>Herstellen einer Verbindung mit der PostgreSQL-Datenbank mithilfe von psql in Cloud Shell

Es gibt verschiedene Anwendungen, mit denen Sie eine Verbindung mit Ihrem Azure-Datenbank für PostgreSQL-Server herstellen können. Als Erstes verwenden wir das psql-Befehlszeilenprogramm, um das Herstellen einer Verbindung mit dem Server zu veranschaulichen.  Sie können wie hier beschrieben einen Webbrowser und die Azure Cloud Shell verwenden, ohne zusätzliche Software zu installieren. Wenn das psql-Hilfsprogramm lokal auf Ihrem eigenen Computer installiert ist, können Sie auch von dort eine Verbindung herstellen.

1. Starten Sie die Azure Cloud Shell über das Terminalsymbol im oberen Navigationsbereich.

   ![Azure-Datenbank für PostgreSQL – Azure Cloud Shell-Terminalsymbol](./media/quickstart-create-database-portal/7-cloud-console.png)

2. Die Azure Cloud Shell wird in Ihrem Browser geöffnet, sodass Sie Bash-Shellbefehle eingeben können.

   ![Azure-Datenbank für PostgreSQL – Azure Shell-Bash-Eingabeaufforderung](./media/quickstart-create-database-portal/8-bash.png)

3. Stellen Sie an der Cloud Shell-Eingabeaufforderung eine Verbindung mit einer Datenbank auf Ihrem Azure-Datenbank für PostgreSQL-Server her, indem Sie die psql-Befehlszeile an der grünen Eingabeaufforderung eingeben.

    Das folgende Format wird verwendet, um mit dem Hilfsprogramm [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) eine Verbindung mit einer Azure-Datenbank für PostgreSQL-Server herzustellen:
    ```bash
    psql --host=<yourserver> --port=<port> --username=<server admin login> --dbname=<database name>
    ```

    Mithilfe des folgenden Befehls wird beispielsweise eine Verbindung mit einem Beispielserver hergestellt:

    ```bash
    psql --host=mypgserver-20170401.postgres.database.azure.com --port=5432 --username=mylogin@mypgserver-20170401 --dbname=postgres
    ```

    psql-Parameter |Empfohlener Wert|Beschreibung
    ---|---|---
    --host | *Servername* | Geben Sie den Servernamen an, den Sie zuvor beim Erstellen der Azure-Datenbank für PostgreSQL verwendet haben. Unser Beispielserver heißt „mypgserver-20170401.postgres.database.azure.com“. Verwenden Sie den vollqualifizierten Domänennamen (\*.postgres.database.azure.com), wie im Beispiel gezeigt. Sollten Sie sich nicht an Ihren Servernamen erinnern, ermitteln Sie die Verbindungsinformationen gemäß der Anleitung aus dem vorherigen Abschnitt. 
    --port | **5432** | Verwenden Sie immer den Port 5432, wenn Sie eine Verbindung mit der Azure-Datenbank für PostgreSQL herstellen. 
    --username | *Anmeldename des Serveradministrators* |Geben Sie den Anmeldename des Serveradministrators ein, den Sie zuvor beim Erstellen der Azure-Datenbank für PostgreSQL verwendet haben. Sollten Sie sich nicht an Benutzernamen erinnern, ermitteln Sie die Verbindungsinformationen gemäß der Anleitung aus dem vorherigen Abschnitt.  Zu verwendendes Format: *username@servername*.
    --dbname | **postgres** | Verwenden Sie für die erste Verbindung den vom System generierten Standarddatenbanknamen *postgres*. Später erstellen Sie dann Ihre eigene Datenbank.

    Nachdem Sie den psql-Befehl mit Ihren eigenen Parameterwerten ausgeführt haben, werden Sie zur Eingabe des Serveradministratorkennworts aufgefordert. Hierbei handelt es sich um das Kennwort, das Sie beim Erstellen des Servers angegeben haben. 

    psql-Parameter |Empfohlener Wert|Beschreibung
    ---|---|---
    password | *Ihr Administratorkennwort* | Hinweis: Die für das Kennwort eingegebenen Zeichen werden in der Bash-Eingabeaufforderung nicht angezeigt. Drücken Sie nach der Eingabe aller Zeichen die EINGABETASTE, um die Authentifizierung durchzuführen und die Verbindung herzustellen.

4.  Erstellen Sie nach dem Herstellen der Serververbindung eine leere Datenbank, indem Sie an der Eingabeaufforderung den folgenden Befehl eingeben:
    ```bash
    CREATE DATABASE mypgsqldb;
    ```

5.  Führen Sie an der Eingabeaufforderung den folgenden Befehl zum Wechseln der Verbindung zur neu erstellten Datenbank **mypgsqldb** aus.
    ```bash
    \c mypgsqldb
    ```

6.  Geben Sie „\q“ ein, und drücken Sie die EINGABETASTE, um psql zu beenden. Schließen Sie die Azure Cloud Shell.

Sie haben nun eine Verbindung mit der Azure-Datenbank für PostgreSQL hergestellt und eine leere Benutzerdatenbank erstellt. Im nächsten Abschnitt stellen Sie eine Verbindung über pgAdmin (ein anderes gängiges Tool) her.

## <a name="connect-to-postgresql-database-using-pgadmin"></a>Herstellen einer Verbindung mit einer PostgreSQL-Datenbank mithilfe von pgAdmin

So stellen Sie eine Verbindung mit der Azure-PostgreSQL-Serverinstanz mithilfe des GUI-Tools _pgAdmin_ her
1.  Starten Sie die Anwendung _pgAdmin_ auf dem Clientcomputer. Sie können _pgAdmin_ über http://www.pgadmin.org/ installieren.
2.  Wählen Sie im Menü **Kurzlinks** die Option **Neuen Server hinzufügen** aus.
3.  Geben Sie im Dialogfeld **Erstellen - Server** auf der Registerkarte **Allgemein** einen eindeutigen Anzeigenamen für den Server ein, z.B. **Azure PostgreSQL Server**.
![pgAdmin-Tool – Erstellen – Server](./media/quickstart-create-database-portal/9-pgadmin-create-server.png)
4.  Übernehmen Sie im Dialogfeld **Erstellen - Server** auf der Registerkarte **Verbindung** die angegebenen Einstellungen, und klicken Sie auf **Speichern**.
   ![pgAdmin – Erstellen – Server](./media/quickstart-create-database-portal/10-pgadmin-create-server.png)
    - **Hostname/-adresse**: mypgserver-20170401.postgres.database.azure.com 
        - Vollqualifizierter Servername.
    - **Port:** 5432
        - Die von diesem Datenbankserver verwendete Portnummer ist 5432.
    - **Wartungsdatenbank**: postgres 
        - Der vom System generierte Standardname der Datenbank.
    - **Benutzername:** mylogin@mypgserver-20170401 
        - Der Serveradministrator-Anmeldename (user@mypgserver), der in diesem Schnellstart an früherer Stelle abgerufen wurde.
    - **Passwort**: Das beim Erstellen des Servers in diesem Schnellstart an früherer Stelle ausgewählte Kennwort.
    - **SSL-Modus**: Require
        - In der Standardeinstellung werden alle Azure PostgreSQL-Server mit aktivierter SSL-Erzwingung erstellt. Informationen zum Deaktivieren der SSL-Erzwingung finden Sie unter [Erzwingen von SSL](./concepts-ssl-connection-security.md).
5.  Klicken Sie auf **Speichern**.
6.  Erweitern Sie im linken Browserbereich die **Servergruppen**. Wählen Sie Ihren Server **Azure PostgreSQL-Server** aus.
7.  Wählen Sie den **Server** aus, mit dem Sie eine Verbindung hergestellt haben, und wählen Sie darunter die Option **Datenbanken** aus. 
8.  Klicken Sie mit der rechten Maustaste auf **Datenbanken**, um eine Datenbank zu erstellen.
9.  Wählen Sie den Datenbanknamen **mypgsqldb** und für den Besitzer den Serveradministrator-Anmeldenamen **mylogin** aus.
10. Klicken Sie auf **Speichern**, um eine leere Datenbank zu erstellen.
11. Erweitern Sie im **Browser** die Gruppe **Server**. Erweitern Sie den erstellten Server. Sie bemerken darunter die Datenbank **mypgsqldb**.
 ![pgAdmin – Erstellen – Datenbank](./media/quickstart-create-database-portal/11-pgadmin-database.png)


## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Bereinigen Sie alle im Schnellstart erstellten Ressourcen, indem Sie die [Azure-Ressourcengruppe](../azure-resource-manager/resource-group-overview.md) löschen.

> [!TIP]
> Andere Schnellstarts dieser Sammlung bauen auf diesem Schnellstart auf. Wenn Sie planen, mit den nachfolgenden Schnellstarts fortzufahren, sollten Sie die in diesem Schnellstart erstellten Ressourcen nicht bereinigen. Falls Sie nicht fortfahren möchten, können Sie die folgenden Schritte ausführen, um alle erstellten Ressourcen dieses Schnellstarts im Azure-Portal zu löschen.

1.  Klicken Sie im Azure-Portal im Menü auf der linken Seite auf **Ressourcengruppen** und dann auf **myresourcegroup**.
2.  Klicken Sie auf der Seite mit Ihrer Ressourcengruppe auf **Löschen**, geben Sie im Textfeld **myresourcegroup** ein, und klicken Sie anschließend auf „Löschen“.

Wenn Sie lediglich den neu erstellten Server löschen möchten:
1.  Klicken Sie im Azure-Portal im linken Menü auf „PostgreSQL-Server“, und suchen Sie nach dem soeben erstellten Server.
2.  Klicken Sie auf der Seite „Übersicht“ im oberen Bereich ![Azure Database für PostgreSQL – Server löschen](./media/quickstart-create-database-portal/12-delete.png) auf die Schaltfläche „Löschen“.
3.  Bestätigen Sie, dass Sie den Server wirklich löschen möchten, und zeigen Sie die darunter liegenden betroffenen Datenbanken an. Geben Sie im Textfeld **mypgserver-20170401** ein, und klicken Sie dann auf „Löschen“.

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Migrieren der Datenbank mit Export und Import](./howto-migrate-using-export-and-import.md)

