---
title: "Entwerfen Ihrer ersten Azure-Datenbank für PostgreSQL mithilfe des Azure-Portals | Microsoft-Dokumentation"
description: "In diesem Tutorial lernen Sie, wie Sie Ihre erste Azure-Datenbank für PostgreSQL mithilfe des Azure-Portals entwerfen."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql-database
ms.custom: tutorial, mvc
ms.topic: article
ms.date: 05/10/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 977d47fb72d1c9e2a482602aacc456ae95547676
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017

---
# <a name="design-your-first-azure-database-for-postgresql-using-the-azure-portal"></a>Entwerfen Ihrer ersten Azure-Datenbank für PostgreSQL mithilfe des Azure-Portals

Azure-Datenbank für PostgreSQL ist ein verwalteter Dienst, mit dem Sie hoch verfügbare PostgreSQL-Datenbanken in der Cloud ausführen, verwalten und skalieren können. Mit dem Azure-Portal können Sie mühelos Ihren Server verwalten und eine Datenbank entwerfen.

In diesem Tutorial verwenden Sie das Azure-Portal, um Folgendes zu lernen:
> [!div class="checklist"]
> * Erstellen einer Azure-Datenbank für PostgreSQL
> * Konfigurieren der Serverfirewall 
> * Erstellen einer Datenbank mit dem Hilfsprogramm [**psql**](https://www.postgresql.org/docs/9.6/static/app-psql.html)
> * Laden von Beispieldaten
> * Abfragen von Daten
> * Aktualisieren von Daten
> * Wiederherstellen von Daten

## <a name="prerequisites"></a>Voraussetzungen
Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="log-in-to-the-azure-portal"></a>Anmelden beim Azure-Portal
Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.

## <a name="create-an-azure-database-for-postgresql"></a>Erstellen einer Azure-Datenbank für PostgreSQL

Ein Azure-Datenbank für PostgreSQL-Server wird mit einer definierten Gruppe von [Compute- und Speicherressourcen](./concepts-compute-unit-and-storage.md) erstellt. Der Server wird in einer [Azure-Ressourcengruppe](../azure-resource-manager/resource-group-overview.md) erstellt.

Erstellen Sie einen Azure-Datenbank für PostgreSQL-Server in diesen Schritten:
1.  Klicken Sie in der linken oberen Ecke des Azure-Portals auf die Schaltfläche **+ Neu**.
2.  Wählen Sie auf der Seite **Neu** die Option **Datenbanken** und dann auf der Seite **Datenbanken** die Option **Azure-Datenbank für PostgreSQL**.
 ![Azure-Datenbank für PostgreSQL – Erstellen der Datenbank](./media/tutorial-design-database-using-azure-portal/1-create-database.png)

3.  Geben Sie die folgenden Informationen in das Formular für den neuen Server ein, wie in der obigen Abbildung dargestellt:
    - Servername: **mypgserver-20170401** (Der Name eines Servers wird dem DNS-Namen zugeordnet und muss deshalb global eindeutig sein) 
    - Abonnement: Wenn Sie über mehrere Abonnements verfügen, wählen Sie das entsprechende Abonnement aus, in dem die Ressource vorhanden ist oder in Rechnung gestellt wird.
    - Ressourcengruppe: **myresourcegroup**
    - Serveradministrator-Anmeldename und ein Kennwort Ihrer Wahl
    - Ort
    - PostgreSQL-Version

  > [!IMPORTANT]
  > Der hier angegebene Benutzername bzw. das Kennwort für den Serveradministrator sind erforderlich, um später in diesem Schnellstart die Anmeldung am Server und an den dazugehörigen Datenbanken durchzuführen. Behalten Sie diese Angaben im Kopf, oder notieren Sie sie zur späteren Verwendung.

4.  Klicken Sie auf **Tarif**, um die Dienstebene und die Leistungsstufe für die neue Datenbank anzugeben. Wählen Sie für diesen Schnellstart Ebene **Basic**, **50 Compute-Einheiten** und **50GB** enthaltenen Speicher.
 ![Azure-Datenbank für PostgreSQL – Auswahl der Dienstebene](./media/tutorial-design-database-using-azure-portal/2-service-tier.png)
5.  Klicken Sie auf **OK**.
6.  Klicken Sie auf **Erstellen**, um den Server bereitzustellen. Die Bereitstellung dauert einige Minuten.

  > [!TIP]
  > Aktivieren Sie die Option **An Dashboard anheften**, um leichtes Nachverfolgen Ihrer Bereitstellungen zu ermöglichen.

7.  Klicken Sie in der Symbolleiste auf **Benachrichtigungen**, um den Bereitstellungsprozess zu überwachen.
 ![Azure-Datenbank für PostgreSQL – Benachrichtigungen anzeigen](./media/tutorial-design-database-using-azure-portal/3-notifications.png)
   
  Standardmäßig wird die **postgres**-Datenbank unter dem Server erstellt. Die [postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html)-Datenbank ist eine Standarddatenbank für die Verwendung durch Benutzer, Hilfsprogramme und Drittanbieteranwendungen. 

## <a name="configure-a-server-level-firewall-rule"></a>Konfigurieren einer Firewallregel auf Serverebene

Der Azure-Datenbank für PostgreSQL-Dienst erstellt eine Firewall auf der Serverebene. Diese Firewall verhindert, dass externe Anwendungen und Tools eine Verbindung mit dem Server und Datenbanken auf dem Server herstellen – sofern keine Firewallregel erstellt wird, um die Firewall für bestimmte IP-Adressen zu öffnen. 

1.  Klicken Sie nach Abschluss der Bereitstellung im linken Menü auf **Alle Ressourcen**, und geben Sie den Namen **mypgserver-20170401** ein, um nach dem neu erstellten Server zu suchen. Klicken Sie auf den im Suchergebnis aufgelisteten Servernamen. Die Seite **Übersicht** für Ihren Server wird geöffnet und enthält Optionen für die weitere Konfiguration.
 
 ![Azure-Datenbank für PostgreSQL – Suche nach dem Server ](./media/tutorial-design-database-using-azure-portal/4-locate.png)

2.  Wählen Sie auf dem Serverblatt **Verbindungssicherheit**. 
3.  Klicken Sie in das Textfeld unter **Regelname**, und fügen Sie eine neue Firewallregel hinzu, um den IP-Adressbereich auf die Whitelist für die Konnektivität zu setzen. Für dieses Tutorial lassen wir durch Eingabe von **Regelname = AllowAllIps**, **Start-IP = 0.0.0.0** und **End-IP = 255.255.255.255** alle IP-Adressen zu und klicken dann auf **Speichern**. Sie können eine Firewallregel festlegen, die einen Bereich von IP-Adressen abdeckt, mit denen Verbindungen aus dem Netzwerk hergestellt werden können.
 
 ![Azure-Datenbank für PostgreSQL – Erstellen von Firewallregeln](./media/tutorial-design-database-using-azure-portal/5-firewall-2.png)

4.  Klicken Sie auf **Speichern** und dann auf **X**, um die Seite **Verbindungssicherheit** zu schließen.

  > [!NOTE]
  > Der Azure-PostgreSQL-Server kommuniziert über Port 5432. Wenn Sie versuchen, eine Verbindung aus einem Unternehmensnetzwerk heraus herzustellen, wird der ausgehende Datenverkehr über Port 5432 von der Firewall Ihres Netzwerks unter Umständen nicht zugelassen. In diesem Fall können Sie nur dann eine Verbindung mit Ihrem Azure SQL-Datenbankserver herstellen, wenn Ihre IT-Abteilung Port 5432 öffnet.
  >


## <a name="get-the-connection-information"></a>Abrufen der Verbindungsinformationen

Sobald wir unsere Azure-Datenbank für PostgreSQL-Server erstellt haben, wird die Standarddatenbank **postgres** auch erstellt. Zum Herstellen einer Verbindung mit dem Datenbankserver müssen Sie Hostinformationen und Anmeldeinformationen für den Zugriff angeben.

1. Klicken Sie im Azure-Portal im linken Menü auf **Alle Ressourcen**, und suchen Sie nach dem Server **mypgserver-20170401**, den Sie soeben erstellt haben.

  ![Azure-Datenbank für PostgreSQL – Suche nach dem Server ](./media/tutorial-design-database-using-azure-portal/4-locate.png)

3. Klicken Sie auf den Servernamen **mypgserver-20170401**.
4. Wählen Sie die Seite **Übersicht** des Servers aus. Notieren Sie sich den **Servernamen** und den **Anmeldenamen des Serveradministrators**.

 ![Azure-Datenbank für PostgreSQL-Server – Anmeldename des Serveradministrators](./media/tutorial-design-database-using-azure-portal/6-server-name.png)


## <a name="connect-to-postgresql-database-using-psql-in-cloud-shell"></a>Herstellen einer Verbindung mit der PostgreSQL-Datenbank mithilfe von psql in Cloud Shell

Wir stellen jetzt mit dem Befehlszeilen-Hilfsprogramm psql eine Verbindung mit der Azure-Datenbank für PostgreSQL-Server her. 
1. Starten Sie die Azure Cloud Shell über das Terminalsymbol im oberen Navigationsbereich.

   ![Azure-Datenbank für PostgreSQL – Azure Cloud Shell-Terminalsymbol](./media/tutorial-design-database-using-azure-portal/7-cloud-shell.png)

2. Die Azure Cloud Shell wird in Ihrem Browser geöffnet, sodass Sie Bash-Befehle eingeben können.

   ![Azure-Datenbank für PostgreSQL – Azure Shell-Bash-Eingabeaufforderung](./media/tutorial-design-database-using-azure-portal/8-bash.png)

3. Stellen Sie an der Cloud Shell-Eingabeaufforderung mit den psql-Befehlen eine Verbindung mit Ihrem Azure-Datenbank für PostgreSQL-Server her. Das folgende Format wird verwendet, um mit dem Hilfsprogramm [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) eine Verbindung mit einem Azure-Datenbank für PostgreSQL-Server herzustellen:
   ```bash
   psql --host=<myserver> --port=<port> --username=<server admin login> --dbname=<database name>
   ```

   Der folgende Befehl stellt z.B. mithilfe der Zugriffsanmeldeinformationen eine Verbindung mit der Standarddatenbank **postgres** auf Ihrem PostgreSQL-Server **mypgserver-20170401.postgres.database.azure.com** her. Geben Sie Ihr Serveradministrator-Kennwort ein, wenn Sie dazu aufgefordert werden.

   ```bash
   psql --host=mypgserver-20170401.postgres.database.azure.com --port=5432 --username=mylogin@mypgserver-20170401 --dbname=postgres
   ```

## <a name="create-a-new-database"></a>Erstellen einer neuen Datenbank
Sobald Sie mit dem Server verbunden sind, erstellen Sie eine leere Datenbank an der Eingabeaufforderung.
```bash
CREATE DATABASE mypgsqldb;
```

Führen Sie an der Eingabeaufforderung den folgenden Befehl zum Wechseln der Verbindung zur neu erstellten Datenbank **mypgsqldb** aus.
```bash
\c mypgsqldb
```
## <a name="create-tables-in-the-database"></a>Erstellen von Tabellen in der Datenbank
Da Sie nun wissen, wie Sie eine Verbindung mit der Azure-Datenbank für PostgreSQL herstellen, können wir die Ausführung einiger grundlegender Aufgaben erörtern.

Zunächst können wir eine Tabelle erstellen und mit einigen Daten füllen. Wir erstellen eine Tabelle, die Bestandsinformationen verfolgt.
```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
```

Um die neu erstellte Tabelle in der Liste der Tabellen anzuzeigen, geben Sie Folgendes ein:
```sql
\dt
```

## <a name="load-data-into-the-tables"></a>Laden von Daten in die Tabellen
Jetzt können wir einige Daten in die Tabelle einfügen. Führen Sie im geöffneten Eingabeaufforderungsfenster die folgende Abfrage zum Einfügen einiger Datenzeilen aus.
```sql
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150); 
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
```

Die Tabelle, die Sie zuvor erstellt haben, enthält nun zwei Zeilen mit Beispieldaten.

## <a name="query-and-update-the-data-in-the-tables"></a>Abfragen und Aktualisieren der Daten in den Tabellen
Führen Sie die folgende Abfrage aus, um Informationen aus der Datenbanktabelle abzurufen. 
```sql
SELECT * FROM inventory;
```

Sie können auch die Daten in den Tabellen aktualisieren.
```sql
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
```

Die Zeile wird entsprechend aktualisiert, wenn Sie Daten abrufen.
```sql
SELECT * FROM inventory;
```

## <a name="restore-data-to-a-previous-point-in-time"></a>Wiederherstellung des Zustands der Daten zu einem früheren Zeitpunkt
Stellen Sie sich vor, Sie haben versehentlich diese Tabelle gelöscht. Dies ist eine Situation, in der Sie die Datenbank nicht einfach wiederherstellen können. Die Azure-Datenbank für PostgreSQL ermöglicht Ihnen, zu einem beliebigen Zeitpunkt (innerhalb der letzten 7 Tage [Basic] und 35 Tage [Standard]) zurückzugehen und den Status dieses Zeitpunkts auf einem neuen Server wiederherzustellen. Sie können diesen neuen Server zur Wiederherstellung gelöschter Daten verwenden. Mithilfe der folgenden Schritte wird der Status des Beispielservers zu einem Zeitpunkt wiederhergestellt, der vor dem Hinzufügen der Tabelle liegt.

1.  Klicken Sie auf der Azure-Datenbank für PostgreSQL-Seite für Ihren Server auf der Symbolleiste auf **Wiederherstellen**. Die Seite **Wiederherstellen** wird geöffnet.
  ![Azure-Portal – Optionen des Wiederherstellungsformulars](./media/tutorial-design-database-using-azure-portal/9-azure-portal-restore.png)
2.  Geben Sie im Formular **Wiederherstellen** die erforderlichen Informationen ein:

  ![Azure-Portal – Optionen des Wiederherstellungsformulars](./media/tutorial-design-database-using-azure-portal/10-azure-portal-restore.png)
  - **Wiederherstellungspunkt**: Wählen Sie einen Zeitpunkt vor der Änderung des Servers aus.
  - **Zielserver**: Geben Sie einen neuen Servernamen für die Wiederherstellung ein.
  - **Speicherort**: Sie können nicht die Region wählen. Standardmäßig ist dieser Wert mit dem Quellserver identisch.
  - **Tarif**: Sie können diesen Wert beim Wiederherstellen eines Servers nicht ändern. Er ist mit dem Wert für den Quellserver identisch. 
3.  Klicken Sie auf **OK**, um den Status des Servers [für einen Zeitpunkt wiederherzustellen](./howto-restore-server-portal.md), der vor dem Löschen der Tabellen liegt. Beim Wiederherstellen eines Servers im Zustand eines anderen Zeitpunkts wird ein Duplikat des ursprünglichen Servers im Zustand des von Ihnen angegebenen Zeitpunkts als neuer Server erstellt, vorausgesetzt, dass dieser Zeitpunkt innerhalb der für Ihre [Dienstebene](./concepts-service-tiers.md) geltenden Beibehaltungsdauer liegt.

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie gelernt, wie Sie das Azure-Portal und andere Hilfsprogramme für Folgendes verwenden:
> [!div class="checklist"]
> * Erstellen einer Azure-Datenbank für PostgreSQL
> * Konfigurieren der Serverfirewall 
> * Erstellen einer Datenbank mit dem Hilfsprogramm [**psql**](https://www.postgresql.org/docs/9.6/static/app-psql.html)
> * Laden von Beispieldaten
> * Abfragen von Daten
> * Aktualisieren von Daten
> * Wiederherstellen von Daten

Um als Nächstes zu erfahren, wie Sie Azure CLI verwenden, um ähnliche Aufgaben auszuführen, absolvieren Sie dieses Tutorial: [Entwerfen Ihrer ersten Azure-Datenbank für PostgreSQL mithilfe von Azure CLI](tutorial-design-database-using-azure-cli.md).

