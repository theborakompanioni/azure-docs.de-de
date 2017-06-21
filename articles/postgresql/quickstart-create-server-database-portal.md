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
ms.date: 06/19/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 7948c99b7b60d77a927743c7869d74147634ddbf
ms.openlocfilehash: 7bbf70786bff83ad3cfae9cb9b893f41736874b5
ms.contentlocale: de-de
ms.lasthandoff: 06/20/2017

---

# <a name="create-an-azure-database-for-postgresql-in-the-azure-portal"></a>Erstellen einer Azure-Datenbank für PostgreSQL im Azure-Portal

Azure-Datenbank für PostgreSQL ist ein verwalteter Dienst, mit dem Sie hochverfügbare PostgreSQL-Datenbanken in der Cloud ausführen, verwalten und skalieren können. In diesem Schnellstart wird veranschaulicht, wie Sie eine Azure-Datenbank für PostgreSQL-Server über das Azure-Portal erstellen.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="log-in-to-the-azure-portal"></a>Anmelden beim Azure-Portal

Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.

## <a name="create-an-azure-database-for-postgresql"></a>Erstellen einer Azure-Datenbank für PostgreSQL

Eine Azure-Datenbank für PostgreSQL-Server wird mit einer definierten Gruppe von [Compute- und Speicherressourcen](./concepts-compute-unit-and-storage.md) erstellt. Der Server wird in einer [Azure-Ressourcengruppe](../azure-resource-manager/resource-group-overview.md) erstellt.

Führen Sie die folgenden Schritte aus, um eine Azure-Datenbank für PostgreSQL-Server zu erstellen:
1.  Klicken Sie in der linken oberen Ecke des Azure-Portals auf die Schaltfläche **Neu**.
2.  Wählen Sie auf der Seite **Neu** die Option **Datenbanken** und dann auf der Seite **Datenbanken** die Option **Azure-Datenbank für PostgreSQL** aus.
 ![Azure-Datenbank für PostgreSQL – Erstellen der Datenbank](./media/quickstart-create-database-portal/1-create-database.png)

3.  Geben Sie die folgenden Informationen in das Formular für den neuen Server ein, wie in der obigen Abbildung dargestellt:
    - Servername: **mypgserver-20170401** (der Name eines Servers wird dem DNS-Namen zugeordnet und muss deshalb global eindeutig sein) 
    - Abonnement: Wenn Sie über mehrere Abonnements verfügen, wählen Sie das entsprechende Abonnement aus, in dem die Ressource vorhanden ist oder in Rechnung gestellt wird.
    - Ressourcengruppe: **myresourcegroup**
    - Serveradministrator-Anmeldename und ein Kennwort Ihrer Wahl
    - Ort
    - PostgreSQL-Version

  > [!IMPORTANT]
  > Der hier angegebene Benutzername bzw. das Kennwort für den Serveradministrator sind erforderlich, um später in diesem Schnellstart die Anmeldung am Server und an den dazugehörigen Datenbanken durchzuführen. Behalten Sie diese Angaben im Kopf, oder notieren Sie sie zur späteren Verwendung.

4.  Klicken Sie auf **Tarif**, um die Dienstebene und die Leistungsstufe für die neue Datenbank anzugeben. Wählen Sie für diesen Schnellstart Ebene **Basic**, **50 Compute-Einheiten** und **50 GB** enthaltenen Speicher aus.
 ![Azure-Datenbank für PostgreSQL – Auswahl der Dienstebene](./media/quickstart-create-database-portal/2-service-tier.png)
5.  Klicken Sie auf **OK**.
6.  Klicken Sie auf **Erstellen**, um den Server bereitzustellen. Die Bereitstellung dauert einige Minuten.

  > [!TIP]
  > Aktivieren Sie die Option **An Dashboard anheften**, um leichtes Nachverfolgen Ihrer Bereitstellungen zu ermöglichen.

7.  Klicken Sie in der Symbolleiste auf **Benachrichtigungen**, um den Bereitstellungsprozess zu überwachen.
 ![Azure-Datenbank für PostgreSQL – Benachrichtigungen anzeigen](./media/quickstart-create-database-portal/3-notifications.png)
   
  Standardmäßig wird die **postgres**-Datenbank unter dem Server erstellt. Die [postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html)-Datenbank ist eine Standarddatenbank für die Verwendung durch Benutzer, Hilfsprogramme und Drittanbieteranwendungen. 

## <a name="configure-a-server-level-firewall-rule"></a>Konfigurieren einer Firewallregel auf Serverebene

Der Azure-Datenbank für PostgreSQL-Dienst erstellt eine Firewall auf der Serverebene. Diese Firewall verhindert, dass externe Anwendungen und Tools eine Verbindung mit dem Server und Datenbanken auf dem Server herstellen – sofern keine Firewallregel erstellt wird, um die Firewall für bestimmte IP-Adressen zu öffnen. 

1.  Klicken Sie nach Abschluss der Bereitstellung im linken Menü auf **Alle Ressourcen**, und geben Sie den Namen **mypgserver-20170401** ein, um nach dem neu erstellten Server zu suchen. Klicken Sie auf den im Suchergebnis aufgelisteten Servernamen. Die Seite **Übersicht** für Ihren Server wird geöffnet und enthält Optionen für die weitere Konfiguration.
 
 ![Azure-Datenbank für PostgreSQL – Suchen nach dem Server ](./media/quickstart-create-database-portal/4-locate.png)

2.  Wählen Sie auf dem Serverblatt **Verbindungssicherheit** aus. 
3.  Klicken Sie in das Textfeld unter **Regelname**, und fügen Sie eine neue Firewallregel hinzu, um den IP-Adressbereich auf die Whitelist für die Konnektivität zu setzen. Für diesen Schnellstart lassen wir durch Eingabe von **Regelname = AllowAllIps**, **Start-IP = 0.0.0.0** und **End-IP = 255.255.255.255** alle IP-Adressen zu und klicken dann auf **Speichern**. Sie können eine Firewallregel festlegen, die einen Bereich von IP-Adressen abdeckt, mit denen Verbindungen aus dem Netzwerk hergestellt werden können.

 ![Azure-Datenbank für PostgreSQL – Erstellen von Firewallregeln](./media/quickstart-create-database-portal/5-firewall-2.png)

4.  Klicken Sie auf **Speichern** und dann auf **X**, um die Seite **Verbindungssicherheit** zu schließen.

  > [!NOTE]
  > Der Azure-PostgreSQL-Server kommuniziert über Port 5432. Wenn Sie versuchen, eine Verbindung aus einem Unternehmensnetzwerk heraus herzustellen, wird der ausgehende Datenverkehr über Port 5432 von der Firewall Ihres Netzwerks unter Umständen nicht zugelassen. In diesem Fall können Sie nur dann eine Verbindung mit Ihrem Azure SQL-Datenbankserver herstellen, wenn Ihre IT-Abteilung Port 5432 öffnet.
  >

## <a name="get-the-connection-information"></a>Abrufen der Verbindungsinformationen

Sobald wir unsere Azure-Datenbank für PostgreSQL-Server erstellt haben, wird die Standarddatenbank **postgres** auch erstellt. Zum Herstellen einer Verbindung mit dem Datenbankserver müssen Sie Hostinformationen und Anmeldeinformationen für den Zugriff angeben.

1. Klicken Sie im Azure-Portal im linken Menü auf **Alle Ressourcen**, und suchen Sie nach dem Server **mypgserver-20170401**, den Sie soeben erstellt haben.

  ![Azure-Datenbank für PostgreSQL – Suchen nach dem Server ](./media/quickstart-create-database-portal/4-locate.png)

2. Klicken Sie auf den Servernamen **mypgserver-20170401**.
3. Wählen Sie die Seite **Übersicht** des Servers aus. Notieren Sie sich den **Servernamen** und den **Anmeldenamen des Serveradministrators**.

 ![Azure-Datenbank für PostgreSQL-Server – Anmeldename des Serveradministrators](./media/quickstart-create-database-portal/6-server-name.png)

## <a name="connect-to-postgresql-database-using-psql-in-cloud-shell"></a>Herstellen einer Verbindung mit der PostgreSQL-Datenbank mithilfe von psql in Cloud Shell

Wir stellen jetzt mit dem Befehlszeilen-Hilfsprogramm psql eine Verbindung mit der Azure-Datenbank für PostgreSQL-Server her. 
1. Starten Sie die Azure Cloud Shell über das Terminalsymbol im oberen Navigationsbereich.

   ![Azure-Datenbank für PostgreSQL – Azure Cloud Shell-Terminalsymbol](./media/quickstart-create-database-portal/7-cloud-console.png)

2. Die Azure Cloud Shell wird in Ihrem Browser geöffnet, sodass Sie Bash-Befehle eingeben können.

   ![Azure-Datenbank für PostgreSQL – Azure Shell-Bash-Eingabeaufforderung](./media/quickstart-create-database-portal/8-bash.png)

3. Stellen Sie an der Cloud Shell-Eingabeaufforderung mit den psql-Befehlen eine Verbindung mit Ihrem Azure-Datenbank für PostgreSQL-Server her. Das folgende Format wird verwendet, um mit dem Hilfsprogramm [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) eine Verbindung mit einer Azure-Datenbank für PostgreSQL-Server herzustellen:
   ```bash
   psql --host=<myserver> --port=<port> --username=<server admin login> --dbname=<database name>
   ```

   Der folgende Befehl stellt z.B. mithilfe der Zugriffsanmeldeinformationen eine Verbindung mit der Standarddatenbank **postgres** auf Ihrem PostgreSQL-Server **mypgserver-20170401.postgres.database.azure.com** her. Geben Sie Ihr Serveradministrator-Kennwort ein, wenn Sie dazu aufgefordert werden.

   ```bash
   psql --host=mypgserver-20170401.postgres.database.azure.com --port=5432 --username=mylogin@mypgserver-20170401 --dbname=postgres
   ```
4.  Sobald Sie mit dem Server verbunden sind, erstellen Sie eine leere Datenbank an der Eingabeaufforderung.
```bash
CREATE DATABASE mypgsqldb;
```

5.  Führen Sie an der Eingabeaufforderung den folgenden Befehl zum Wechseln der Verbindung zur neu erstellten Datenbank **mypgsqldb** aus.
```bash
\c mypgsqldb
```

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

