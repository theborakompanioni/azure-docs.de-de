---
title: "Azure-Portal: Erstellen eines Azure-Datenbank für PostgreSQL-Servers | Microsoft-Dokumentation"
description: "Schnellstart-Anleitung zum Erstellen und Verwalten eines Azure-Datenbank für PostgreSQL-Servers über die Benutzeroberfläche des Azure-Portals."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: mvc
ms.topic: quickstart
ms.date: 08/10/2017
ms.translationtype: HT
ms.sourcegitcommit: 07e5e15f4f4c4281a93c8c3267c0225b1d79af45
ms.openlocfilehash: f806a8c1f06a58d0c8440d54c91ecfa41988d8ef
ms.contentlocale: de-de
ms.lasthandoff: 08/31/2017

---

# <a name="create-an-azure-database-for-postgresql-server-in-the-azure-portal"></a>Erstellen eines Azure-Datenbank für PostgreSQL-Servers im Azure-Portal

Azure-Datenbank für PostgreSQL ist ein verwalteter Dienst, mit dem Sie hochverfügbare PostgreSQL-Datenbanken in der Cloud ausführen, verwalten und skalieren können. In dieser Schnellstartanleitung wird veranschaulicht, wie Sie in etwa fünf Minuten über das Azure-Portal einen Azure-Datenbank für PostgreSQL-Server erstellen.

Wenn Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.
Browsen Sie zum [Portal](https://portal.azure.com/). Geben Sie Ihre Anmeldeinformationen ein, um sich beim Portal anzumelden. Die Standardansicht ist Ihr Dienstdashboard.

## <a name="create-an-azure-database-for-postgresql-server"></a>Erstellen einer Azure-Datenbank für PostgreSQL-Server

Eine Azure-Datenbank für PostgreSQL-Server wird mit einer definierten Gruppe von [Compute- und Speicherressourcen](./concepts-compute-unit-and-storage.md) erstellt. Der Server wird in einer [Azure-Ressourcengruppe](../azure-resource-manager/resource-group-overview.md) erstellt.

Führen Sie die folgenden Schritte aus, um einen Azure-Datenbank für PostgreSQL-Server zu erstellen:
1. Wählen Sie in der linken oberen Ecke des Portals die Schaltfläche **Neu** (+) aus.

2. Wählen Sie **Datenbanken** > **Azure-Datenbank für PostgreSQL** aus.

    ![Die Option „Azure-Datenbank für PostgreSQL“](./media/quickstart-create-database-portal/1-create-database.png)

3. Geben Sie die folgenden Informationen in das Formular für den neuen Server ein, wie in der obigen Abbildung dargestellt:

    Einstellung|Empfohlener Wert|Beschreibung
    ---|---|---
    Servername |*mypgserver-20170401*|Ein eindeutiger Name, der Ihren Azure-Datenbank für PostgreSQL-Server identifiziert. Der Domänenname *postgres.database.azure.com* wird an den angegebenen Servernamen angefügt. Der Servername darf nur Kleinbuchstaben, Zahlen und den Bindestrich (-) enthalten. Er muss 3 bis 63 Zeichen umfassen.
    Abonnement|Ihr Abonnement|Das Azure-Abonnement, das Sie für Ihren Server verwenden möchten. Falls Sie über mehrere Abonnements verfügen, wählen Sie das Abonnement aus, über das die Ressource abgerechnet wird.
    Ressourcengruppe|*myresourcegroup*| Ein neuer Ressourcengruppenname oder ein bereits vorhandener Name aus Ihrem Abonnement
    Serveradministratoranmeldung |*mylogin*| Ihr eigenes Anmeldekonto für die Verbindungsherstellung mit dem Server. Der Administratoranmeldename darf nicht **azure_superuser,** **azure_pg_admin,** **admin,** **administrator,** **root,** **guest,** oder **public** lauten. Er kann nicht mit **pg_** beginnen.
    Kennwort |Beliebig | Ein neues Kennwort für das Serveradministratorkonto. Es muss zwischen acht und 128 Zeichen lang sein. Das Kennwort muss Zeichen aus drei der folgenden Kategorien enthalten: englische Großbuchstaben, englische Kleinbuchstaben, Zahlen (0–9) und nicht alphanumerische Zeichen (!, $, #, % usw.).
    Standort|Die Region, die Ihren Benutzern am nächsten liegt| Der Standort, der Ihren Benutzern am nächsten ist
    PostgreSQL-Version|Die neueste Version| Die neueste Version (es sei denn, Sie haben besondere Anforderungen).
    Tarif  | **Basic**, **50 Compute-Einheiten**, **50 GB** | Dienstebene und Leistungsstufe für Ihre neue Datenbank. Wählen Sie **Tarif**aus. Wählen Sie als Nächstes die Registerkarte **Basic** aus. Wählen Sie dann das linke Ende des Schiebereglers **Compute-Einheiten** aus, um für diese Schnellstartanleitung den kleinstmöglichen Wert festzulegen. Wählen Sie **OK** aus, um die Tarifauswahl zu speichern. Weitere Informationen erhalten Sie auf dem folgenden Screenshot. 
    An Dashboard anheften | Prüfen | Ermöglicht die komfortable Nachverfolgung Ihres Servers auf der Titelseite des Dashboards im Portal.

    > [!IMPORTANT]
    > Der hier angegebene Benutzername und das Kennwort für den Serveradministrator sind erforderlich, um später in diesem Schnellstart die Anmeldung auf dem Server und in den zugehörigen Datenbanken durchzuführen. Behalten Sie diese Angaben im Kopf, oder notieren Sie sie zur späteren Verwendung.

    ![Der Bereich „Tarif“](./media/quickstart-create-database-portal/2-service-tier.png)

4. Wählen Sie **Erstellen** aus, um den Server bereitzustellen. Die Bereitstellung kann bis zu 20 Minuten dauern.

5. Wählen Sie auf der Symbolleiste das Symbol **Benachrichtigungen** aus, um den Bereitstellungsprozess zu überwachen.

    ![Der Bereich „Benachrichtigungen“](./media/quickstart-create-database-portal/3-notifications.png)
   
  Standardmäßig werden **postgres**-Datenbanken unter dem Server erstellt. Die [postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html)-Datenbank ist eine Standarddatenbank für die Verwendung durch Benutzer, Hilfsprogramme und Drittanbieteranwendungen. 

## <a name="configure-a-server-level-firewall-rule"></a>Konfigurieren einer Firewallregel auf Serverebene

Azure-Datenbank für PostgreSQL erstellt eine Firewall auf Serverebene. Sie verhindert, dass externe Anwendungen und Tools eine Verbindung mit dem Server oder Datenbanken auf dem Server herstellen – es sei denn, Sie erstellen eine Regel, um die Firewall für bestimmte IP-Adressen zu öffnen. 

1. Navigieren Sie nach Abschluss der Bereitstellung zu Ihrem Server. Bei Bedarf können Sie danach suchen. Wählen Sie beispielsweise im Menü auf der linken Seite die Option **Alle Ressourcen** aus. Geben Sie den Servernamen (beispielsweise **mypgserver-20170401**) ein, um nach dem neu erstellten Server zu suchen. Wählen Sie den Servernamen in der Liste mit den Suchergebnissen aus. Die Seite **Übersicht** für Ihren Server wird geöffnet und enthält Optionen für die weitere Konfiguration.
 
    ![Suchen nach dem Servernamen](./media/quickstart-create-database-portal/4-locate.png)

2. Wählen Sie auf der Serverseite die Option **Verbindungssicherheit** aus.

    ![Sie Einstellung „Verbindungssicherheit“](./media/quickstart-create-database-portal/5-firewall-2.png)

3. Wählen Sie unter der Überschrift **Firewallregeln** in der Spalte **Regelname** das leere Textfeld aus, um mit der Erstellung der Firewallregel zu beginnen. 

    In dieser Schnellstartanleitung lassen Sie auf dem Server alle IP-Adressen zu. Füllen Sie das Textfeld in jeder Spalte mit den folgenden Werten aus:

    Regelname | Start-IP | End-IP 
    ---|---|---
    AllowAllIps | 0.0.0.0 | 255.255.255.255

4. Wählen Sie auf der oberen Symbolleiste der Seite **Verbindungssicherheit** die Option **Speichern** aus. Warten Sie, bis die Benachrichtigung mit dem Hinweis angezeigt wird, dass das Update der Verbindungssicherheit erfolgreich abgeschlossen wurde, bevor Sie fortfahren.

    > [!NOTE]
    > Die Kommunikation für Verbindungen mit Azure-Datenbank für PostgreSQL erfolgt über Port 5432. Wenn Sie versuchen, eine Verbindung aus einem Unternehmensnetzwerk heraus herzustellen, wird der ausgehende Datenverkehr über Port 5432 von der Firewall Ihres Netzwerks unter Umständen nicht zugelassen. In diesem Fall können Sie nur dann eine Verbindung mit Ihrem Server herstellen, wenn Ihre IT-Abteilung Port 5432 öffnet.
    >

## <a name="get-the-connection-information"></a>Abrufen der Verbindungsinformationen

Wenn wir unseren Azure-Datenbank für PostgreSQL-Server erstellt haben, wird eine Standarddatenbank namens **postgres** erstellt. Für die Verbindungsherstellung mit Ihrem Datenbankserver benötigen Sie den vollständigen Servernamen und die Administratoranmeldeinformationen. Diese Werte haben Sie möglicherweise zuvor im Rahmen des Schnellstartartikels notiert. Andernfalls finden Sie den Servernamen und die Anmeldeinformationen im Portal auf der Seite **Übersicht** für Ihren Server.

Öffnen Sie die Seite **Übersicht** für Ihren Server. Notieren Sie sich den **Servernamen** und den **Anmeldenamen des Serveradministrators**. Zeigen Sie mit dem Cursor auf das jeweilige Feld. Daraufhin wird rechts neben dem Text ein Kopiersymbol angezeigt. Wählen Sie das Kopiersymbol aus, um die Werte zu kopieren.

 ![Sie Seite „Übersicht“ für den Server](./media/quickstart-create-database-portal/6-server-name.png)

## <a name="connect-to-the-postgresql-database-by-using-psql-in-cloud-shell"></a>Herstellen einer Verbindung mit der PostgreSQL-Datenbank mithilfe von psql in Cloud Shell

Es gibt verschiedene Anwendungen, mit denen Sie eine Verbindung mit Ihrem Azure-Datenbank für PostgreSQL-Server herstellen können. Als Erstes verwenden wir das psql-Befehlszeilenprogramm, um das Herstellen einer Verbindung mit dem Server zu veranschaulichen. Sie können wie hier beschrieben einen Webbrowser und Azure Cloud Shell verwenden, ohne zusätzliche Software zu installieren. Wenn das psql-Hilfsprogramm lokal auf Ihrem eigenen Computer installiert ist, können Sie auch von dort eine Verbindung herstellen.

1. Wählen Sie oben im Navigationsbereich das Terminalsymbol aus, um Cloud Shell zu öffnen.

   ![Terminalsymbol für Azure Cloud Shell](./media/quickstart-create-database-portal/7-cloud-console.png)

2. Cloud Shell wird in Ihrem Browser geöffnet und ermöglicht die Eingabe von Bash-Shellbefehlen.

   ![Bash-Eingabeaufforderung in Cloud Shell](./media/quickstart-create-database-portal/8-bash.png)

3. Stellen Sie an der Cloud Shell-Eingabeaufforderung eine Verbindung mit einer Datenbank auf Ihrem Azure-Datenbank für PostgreSQL-Server her, indem Sie die psql-Befehlszeile eingeben.

    Verwenden Sie das folgende Format, um mit dem Hilfsprogramm [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) eine Verbindung mit einem Azure-Datenbank für PostgreSQL-Server herzustellen:
    ```bash
    psql --host=<yourserver> --port=<port> --username=<server admin login> --dbname=<database name>
    ```

    Mithilfe des folgenden Befehls wird beispielsweise eine Verbindung mit einem Beispielserver hergestellt:

    ```bash
    psql --host=mypgserver-20170401.postgres.database.azure.com --port=5432 --username=mylogin@mypgserver-20170401 --dbname=postgres
    ```

    psql-Parameter |Empfohlener Wert|Beschreibung
    ---|---|---
    --host | Servername | Der Servername, den Sie weiter oben beim Erstellen des Azure-Datenbank für PostgreSQL-Servers verwendet haben. Der dargestellte Beispielserver heißt **mypgserver-20170401.postgres.database.azure.com**. Verwenden Sie den vollqualifizierten Domänennamen (**\*.postgres.database.azure.com**), wie im Beispiel gezeigt. Sollten Sie sich nicht an Ihren Servernamen erinnern, ermitteln Sie die Verbindungsinformationen gemäß den Schritten aus dem vorherigen Abschnitt. 
    --port | 5432 | Der Port zum Herstellen einer Verbindung mit dem Azure-Datenbank für PostgreSQL-Server 
    --username | Anmeldename des Serveradministrators |Der Anmeldename des Serveradministrators, den Sie zuvor beim Erstellen des Azure-Datenbank für PostgreSQL-Servers verwendet haben. Sollten Sie sich nicht an den Benutzernamen erinnern, können Sie die Verbindungsinformationen anhand der Schritten im vorherigen Abschnitt ermitteln. Zu verwendendes Format: *username@servername*.
    --dbname | *postgres* | Der für die erste Verbindung vom System generierte Standarddatenbankname. Später erstellen Sie dann Ihre eigene Datenbank.

    Nachdem Sie den psql-Befehl mit Ihren eigenen Parameterwerten ausgeführt haben, werden Sie zur Eingabe des Serveradministratorkennworts aufgefordert. Hierbei handelt es sich um das Kennwort, das Sie beim Erstellen des Servers angegeben haben. 

    psql-Parameter |Empfohlener Wert|Beschreibung
    ---|---|---
    password | Ihr Administratorkennwort | Die für das Kennwort eingegebenen Zeichen werden in der Bash-Eingabeaufforderung nicht angezeigt. Drücken Sie nach der Eingabe aller Zeichen die **EINGABETASTE**, um die Authentifizierung durchzuführen und die Verbindung herzustellen.

    Nachdem die Verbindung hergestellt wurde, wird vom psql-Hilfsprogramm eine PostgreSQL-Eingabeaufforderung für die Eingabe von SQL-Befehlen angezeigt. In der Ausgabe der Erstverbindung wird unter Umständen eine Warnung angezeigt, wenn die Version des psql-Programms in Cloud Shell von der Version des Azure-Datenbank für PostgreSQL-Servers abweicht. 
    
    psql-Beispielausgabe:
    ```bash
    psql (9.5.7, server 9.6.2)
    WARNING: psql major version 9.5, server major version 9.6.
        Some psql features might not work.
    SSL connection (protocol: TLSv1.2, cipher: ECDHE-RSA-AES256-SHA384, bits: 256, compression: off)
    Type "help" for help.
   
    postgres=> 
    ```

    > [!TIP]
    > Wenn die Firewall nicht zum Zulassen der IP-Adresse von Cloud Shell konfiguriert ist, tritt der folgende Fehler auf:
    > 
    > "psql: FATAL:  no pg_hba.conf entry for host "138.91.195.82", user "mylogin", database "postgres", SSL on FATAL: SSL connection is required.Specify SSL options and retry.
    > 
    > Vergewissern Sie sich zur Behebung des Fehlers, dass die Serverkonfiguration gemäß den Schritten im Abschnitt „Konfigurieren einer Firewallregel auf Serverebene“ in diesem Artikel durchgeführt wurde.

4. Erstellen Sie eine leere Datenbank, indem Sie an der Eingabeaufforderung den folgenden Befehl eingeben:
    ```bash
    CREATE DATABASE mypgsqldb;
    ```
    Die Ausführung des Befehls kann einige Minuten dauern. 

5. Führen Sie an der Eingabeaufforderung den folgenden Befehl zum Wechseln der Verbindung zur neu erstellten Datenbank **mypgsqldb** aus:
    ```bash
    \c mypgsqldb
    ```

6. Geben Sie `\q` ein, und drücken Sie anschließend die **EINGABETASTE**, um psql zu beenden. Anschließend können Sie Cloud Shell schließen.

Sie haben nun eine Verbindung mit dem Azure-Datenbank für PostgreSQL-Server hergestellt und eine leere Benutzerdatenbank erstellt. Im nächsten Abschnitt stellen Sie eine Verbindung über pgAdmin (ein anderes gängiges Tool) her.

## <a name="connect-to-the-postgresql-database-by-using-pgadmin"></a>Herstellen einer Verbindung mit einer PostgreSQL-Datenbank mithilfe von pgAdmin

So stellen Sie eine Verbindung mit dem Azure-PostgreSQL-Server mithilfe des GUI-Tools pgAdmin her
1. Öffnen Sie die Anwendung pgAdmin auf dem Clientcomputer. Sie können pgAdmin von der [pgAdmin-Website](http://www.pgadmin.org/) installieren.

2. Wählen Sie auf der Dashboardseite unter dem Abschnitt **Quicklinks** das Symbol **Neuen Server hinzufügen** aus.

3. Geben Sie im Dialogfeld **Erstellen – Server** auf der Registerkarte **Allgemein** einen eindeutigen Anzeigenamen für den Server ein, z.B. **Azure PostgreSQL Server**.

    ![Die Registerkarte „Allgemein“](./media/quickstart-create-database-portal/9-pgadmin-create-server.png)

4. Übernehmen Sie im Dialogfeld **Erstellen – Server** auf der Registerkarte **Verbindung** die angegebenen Einstellungen, und wählen Sie dann **Speichern** aus.

   ![Die Registerkarte „Verbindung“](./media/quickstart-create-database-portal/10-pgadmin-create-server.png)

    pgAdmin-Parameter |Empfohlener Wert|Beschreibung
    ---|---|---
    Hostname/Adresse | Servername | Der Servername, den Sie weiter oben beim Erstellen des Azure-Datenbank für PostgreSQL-Servers verwendet haben. Unser Beispielserver heißt **mypgserver-20170401.postgres.database.azure.com**. Verwenden Sie den vollqualifizierten Domänennamen (**\*.postgres.database.azure.com**), wie im Beispiel gezeigt. Sollten Sie sich nicht an Ihren Servernamen erinnern, ermitteln Sie die Verbindungsinformationen gemäß den Schritten aus dem vorherigen Abschnitt. 
    Port | 5432 | Der Port zum Herstellen einer Verbindung mit dem Azure-Datenbank für PostgreSQL-Server 
    Wartungsdatenbank | *postgres* | Der vom System generierte Standardname der Datenbank
    Benutzername | Anmeldename des Serveradministrators | Der Anmeldename des Serveradministrators, den Sie zuvor beim Erstellen des Azure-Datenbank für PostgreSQL-Servers verwendet haben. Sollten Sie sich nicht an Benutzernamen erinnern, ermitteln Sie die Verbindungsinformationen gemäß den Schritten aus dem vorherigen Abschnitt. Zu verwendendes Format: *username@servername*.
    Kennwort | Ihr Administratorkennwort | Das Kennwort, das Sie beim Erstellen des Servers weiter oben in dieser Schnellstartanleitung ausgewählt haben
    Rolle | Nicht ausfüllen | Zu diesem Zeitpunkt müssen Sie keinen Rollennamen angeben. Lassen Sie das Feld leer.
    SSL-Modus | Erforderlich | In der Standardeinstellung werden alle Azure PostgreSQL-Server mit aktivierter SSL-Erzwingung erstellt. Weitere Informationen dazu, wie Sie das Erzwingen von SSL deaktivieren, finden Sie unter [Erzwingen von SSL](./concepts-ssl-connection-security.md).
    
5. Wählen Sie **Speichern** aus.

6. Erweitern Sie im linken Bereich **Browser** den Knoten **Server**. Wählen Sie Ihren Server aus (z.B. **Azure PostgreSQL Server**). Durch Klicken stellen Sie eine Verbindung her.

7. Erweitern Sie den Serverknoten und anschließend darunter den Eintrag **Datenbanken**. Die Liste sollte Ihre vorhandene *postgres*-Datenbank und alle im vorherigen Abschnitt neu erstellten Benutzerdatenbanken enthalten, beispielsweise **mypgsqldb**. Beachten Sie, dass Sie mit Azure-Datenbank für PostgreSQL mehrere Datenbanken pro Server erstellen können.

8. Klicken Sie mit der rechten Maustaste auf **Datenbanken**, wählen Sie das Menü **Erstellen** und dann **Datenbank** aus.

9. Geben Sie einen Datenbanknamen Ihrer Wahl in das Feld **Datenbank** ein, etwa **mypgsqldb** wie im gezeigten Beispiel.

10. Wählen Sie im Listenfeld den **Besitzer** der Datenbank aus. Wählen Sie den Anmeldenamen des Serveradministrators aus (in unserem Beispiel **mylogin**).

11. Wählen Sie **Speichern** aus, um eine neue leere Datenbank zu erstellen.

12. Im Bereich **Browser** sehen Sie die erstellte Datenbank in der Liste der Datenbanken unter Ihrem Servernamen.

    ![Der Bereich „Browser“](./media/quickstart-create-database-portal/11-pgadmin-database.png)


## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Die im Rahmen dieser Schnellstartanleitung erstellten Ressourcen können auf zwei Arten bereinigt werden. Sie können die [Azure-Ressourcengruppe](../azure-resource-manager/resource-group-overview.md) einschließlich aller darin enthaltenen Ressourcen löschen. Falls die anderen Ressourcen erhalten bleiben sollen, löschen Sie nur die einzelne Serverressource.

> [!TIP]
> Andere Schnellstartanleitungen in dieser Sammlung bauen auf dieser Schnellstartanleitung auf. Wenn Sie weitere Schnellstartanleitungen verwenden möchten, überspringen Sie die Bereinigung der in dieser Schnellstartanleitung erstellten Ressourcen. Falls Sie nicht fortfahren möchten, können Sie die in dieser Schnellstartanleitung erstellten Ressourcen anhand der folgenden Schritte im Azure-Portal löschen.

So löschen Sie die gesamte Ressourcengruppe, einschließlich des neu erstellten Servers
1. Navigieren Sie im Portal zu Ihrer Ressourcengruppe. Wählen Sie im Menü auf der linken Seite **Ressourcengruppen** aus. Wählen Sie anschließend den Namen Ihrer Ressourcengruppe (in unserem Beispiel **myresourcegroup**) aus.

2. Wählen Sie auf der Ressourcengruppenseite die Option **Löschen** aus. Geben Sie dann den Namen der Ressourcengruppe (in unserem Beispiel **myresourcegroup**) in das Textfeld ein, um das Löschen zu bestätigen. Klicken Sie auf **Löschen**.

So löschen Sie ausschließlich den neu erstellten Server
1. Navigieren Sie im Portal zu Ihrem Server, falls er nicht geöffnet ist. Wählen Sie im Menü auf der linken Seite die Option **Alle Ressourcen** aus. Suchen Sie anschließend nach dem Server, den Sie erstellt haben.

2. Wählen Sie auf der Seite **Übersicht** die Option **Löschen** aus.

    ![Die Schaltfläche „Löschen“](./media/quickstart-create-database-portal/12-delete.png)

3. Bestätigen Sie, dass Sie den Server wirklich löschen möchten, und zeigen Sie die darunter liegenden betroffenen Datenbanken an. Geben Sie den Servernamen (in unserem Beispiel **mypgserver-20170401**) in das Textfeld ein. Klicken Sie auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Migrieren der Datenbank mit Export und Import](./howto-migrate-using-export-and-import.md)

