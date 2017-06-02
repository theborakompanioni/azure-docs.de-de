---
title: "Entwerfen Ihrer ersten Azure-Datenbank für MySQL – Azure-Portal | Microsoft-Dokumentation"
description: "In diesem Tutorial wird das Erstellen und Verwalten von Azure-Datenbank für MySQL-Server und der Datenbank mithilfe des Azure-Portals erläutert."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonh
ms.assetid: 
ms.service: mysql-database
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: portal
ms.date: 05/10/2017
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 2958486a0ec055cf1fe334e97389536b2c2bb01f
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017

---

# <a name="design-your-first-azure-database-for-mysql-database"></a>Entwerfen Ihrer ersten Azure-Datenbank für MySQL

Azure-Datenbank für MySQL ist ein verwalteter Dienst, mit dem Sie hochverfügbare MySQL-Datenbanken in der Cloud ausführen, verwalten und skalieren können. Mit dem Azure-Portal können Sie mühelos Ihren Server verwalten und eine Datenbank entwerfen.

In diesem Tutorial verwenden Sie das Azure-Portal, um Folgendes zu lernen:

> [!div class="checklist"]
> * Erstellen einer Azure-Datenbank für MySQL
> * Konfigurieren der Serverfirewall
> * Verwendung des [mysql-Befehlszeilentools](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) zum Erstellen einer Datenbank
> * Laden von Beispieldaten
> * Abfragen von Daten
> * Aktualisieren von Daten
> * Wiederherstellen von Daten

## <a name="log-in-to-the-azure-portal"></a>Anmelden beim Azure-Portal
Öffnen Sie Ihren bevorzugten Browser, und besuchen Sie das [Microsoft Azure-Portal](https://portal.azure.com/). Geben Sie Ihre Anmeldeinformationen zur Anmeldung beim Portal ein. Die Standardansicht ist das Dashboard Ihres Diensts.

## <a name="create-an-azure-database-for-mysql-server"></a>Erstellen einer Azure-Datenbank für MySQL-Server
Ein Azure-Datenbank für MySQL-Server wird mit einer definierten Gruppe von [Compute- und Speicherressourcen](./concepts-compute-unit-and-storage.md) erstellt. Der Server wird in einer [Azure-Ressourcengruppe](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) erstellt.

1.    Navigieren Sie zu **Datenbanken** -> **MySQL**. Wenn Sie MySQL-Server nicht unter der Kategorie **Datenbanken** finden, klicken Sie auf **Alle anzeigen**, um alle verfügbaren Datenbankdienste anzuzeigen. Sie können auch **MySQL** in das Suchfeld eingeben, um den Dienst schnell zu finden.
![2-1 Navigieren zu MySQL](./media/tutorial-design-database-using-portal/2_1-Navigate-to-MySQL.png)

2.    Klicken Sie auf das **MySQL**-Symbol und anschließend auf **Erstellen**.

Füllen Sie in unserem Beispiel das Azure-Datenbank für MySQL-Formular mit den folgenden Informationen:

| **Formularfeld** | **Feldbeschreibung** |
|----------------|-----------------------|
| *Servername* | mysqlserver4demo (Servername ist global eindeutig) |
| *Abonnement* | MySQLaaS (aus der Dropdownliste auswählen) |
| *Ressourcengruppe* | myresource (Ressourcengruppe erstellen oder eine bereits vorhandene verwenden) |
| *Serveradministratoranmeldung* | myadmin (Administratorkontoname einrichten) |
| *Kennwort* | Kennwort des Administratorkontos einrichten |
| *Kennwort bestätigen* | Kennwort des Administratorkontos bestätigen |
| *Standort* | Europa, Norden (zwischen **Europa, Norden** und **USA, Westen** wählen) |
| *Version* | 5.6 (MySQL-Serverversion auswählen) |
| *Leistung konfigurieren* | Basic (**Leistungsebene**, **Compute-Einheiten**, **Speicher** wählen und dann auf **OK** klicken) |

Klicken Sie dann auf **Erstellen**. In ein oder zwei Minuten haben Sie einen neuen Azure-Datenbank für MySQL-Server, der in der Cloud ausgeführt wird. Sie können in der Symbolleiste auf die Schaltfläche **Benachrichtigungen** klicken, um den Bereitstellungsprozess zu überwachen.

> [!TIP]
> Sie sollten Ihre Azure-Dienste in der gleichen Region einrichten und den nächstgelegenen Speicherort wählen. Außerdem können Sie die Option **An Dashboard anheften** aktivieren, um leichtes Nachverfolgen Ihrer Bereitstellungen zu ermöglichen.

![2-2 Erstellen des Servers](./media/tutorial-design-database-using-portal/2_2-Create-server.png)

## <a name="configure-firewall"></a>Konfigurieren der Firewall
Azure SQL-Datenbanken für MySQL werden durch eine Firewall geschützt. Standardmäßig werden alle Verbindungen mit dem Server und den Datenbanken im Server abgelehnt. Vor dem erstmaligen Herstellen der Verbindung mit Azure-Datenbank für MySQL von Ihrem Client aus müssen Sie die Firewall konfigurieren und die öffentliche Netzwerk-IP-Adresse (oder den IP-Adressbereich) des Clients der Whitelist hinzufügen.

1.    Klicken Sie auf den neu erstellten Server und dann auf **Verbindungssicherheit**.

![3-1 Verbindungssicherheit](./media/tutorial-design-database-using-portal/3_1-Connection-security.png)

2.    Wählen Sie **Meine IP-Adresse hinzufügen**, oder konfigurieren Sie hier Firewallregeln. Denken Sie nach dem Erstellen der Regeln daran, auf **Speichern** zu klicken.

Sie können jetzt mit dem mysql-Befehlszeilentool oder dem Benutzeroberflächentool MySQL Workbench eine Verbindung mit dem Server herstellen.

> Azure-Datenbank für MySQL-Server kommuniziert über Port 3306. Wenn Sie versuchen, eine Verbindung aus einem Unternehmensnetzwerk heraus herzustellen, wird der ausgehende Datenverkehr über Port 3306 von der Firewall Ihres Netzwerks unter Umständen nicht zugelassen. In diesem Fall können Sie nur dann eine Verbindung mit Ihrem Azure MySQL-Server herstellen, wenn Ihre IT-Abteilung Port 3306 öffnet.

## <a name="get-connection-information"></a>Abrufen von Verbindungsinformationen
Rufen Sie den vollqualifizierten Servernamen für Ihren Azure MySQL-Server im Azure-Portal ab. Sie verwenden den vollqualifizierten Servernamen, um mit dem mysql-Befehlszeilentool eine Verbindung mit Ihrem Server herzustellen.

1.    Klicken Sie im [Azure-Portal](https://portal.azure.com/) im linken Menü auf **Alle Ressourcen** und dann auf Ihren Azure-Datenbank für MySQL-Server.

2.    Klicken Sie auf **Eigenschaften**. Notieren Sie sich **SERVERNAME** und **SERVERADMINISTRATORANMELDUNG**.
![4-2 Servereigenschaften](./media/tutorial-design-database-using-portal/4_2-server-properties.png)

In diesem Beispiel lautet der Servername *mysql4doc.database.windows.net* und die Serveradministratoranmeldung *mysqladmin@mysql4doc*.

## <a name="connect-to-the-server-using-mysql"></a>Herstellen einer Verbindung mit dem Server unter Verwendung von mysql
Verwenden Sie das [mysql-Befehlszeilentool](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) zum Herstellen einer Verbindung mit Ihrem Azure-Datenbank für MySQL-Server. In diesem Beispiel lautet der Befehl:
```cmd
mysql -h mysqlserver4demo.database.windows.net -u myadmin@mysqlserver4demo -p
```

## <a name="create-a-blank-database"></a>Leere Datenbank erstellen
Sobald Sie mit dem Server verbunden sind, erstellen Sie eine leere Datenbank, um damit zu arbeiten.
```sql
CREATE DATABASE mysampledb;
```

Führen Sie an der Eingabeaufforderung den folgenden Befehl zum Wechseln der Verbindung zu dieser neu erstellten Datenbank aus:
```sql
USE mysampledb;
```

## <a name="create-tables-in-the-database"></a>Erstellen von Tabellen in der Datenbank
Da Sie nun wissen, wie Sie eine Verbindung mit Azure-Datenbank für MySQL herstellen, können wir die Ausführung einiger grundlegender Aufgaben erörtern.

Zunächst können wir eine Tabelle erstellen und mit einigen Daten füllen. Wir erstellen eine Tabelle, die Bestandsinformationen speichert.
```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
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

## <a name="restore-a-database-to-a-previous-point-in-time"></a>Wiederherstellen eines früheren Zustands einer Datenbank
Stellen Sie sich vor, Sie haben versehentlich diese Tabelle gelöscht. Dies ist eine Situation, in der Sie die Datenbank nicht einfach wiederherstellen können. Azure-Datenbank für MySQL ermöglicht Ihnen, zu einem beliebigen Zeitpunkt innerhalb der letzten 35 Tage zurückzugehen und diesen Zeitpunkt in einem neuen Server wiederherzustellen. Sie können diesen neuen Server zur Wiederherstellung gelöschter Daten verwenden. Mithilfe der folgenden Schritte wird der Status des Beispielservers zu einem Zeitpunkt wiederhergestellt, der vor dem Hinzufügen der Tabelle liegt.

1- Klicken Sie auf der Azure-Datenbank für MySQL-Seite für Ihren Server auf der Symbolleiste auf **Wiederherstellen**. Die Seite **Wiederherstellen** wird geöffnet.
![10-1 Wiederherstellen einer Datenbank](./media/tutorial-design-database-using-portal/10_1-restore-a-db.png)

2- Geben Sie im Formular **Wiederherstellen** die erforderlichen Informationen ein:

-    Wiederherstellungspunkt: Wählen Sie einen Zeitpunkt vor der Änderung des Servers aus.
-    Zielserver: Geben Sie einen neuen Servernamen für die Wiederherstellung ein.
-    Speicherort: Sie können nicht die Region wählen. Standardmäßig ist dieser Wert mit dem Quellserver identisch.
-    Tarif: Sie können diesen Wert beim Wiederherstellen eines Servers nicht ändern. Er ist mit dem Wert für den Quellserver identisch.
![10-2 Wiederherstellungsformular](./media/tutorial-design-database-using-portal/10_2-restore-form.png)

3- Klicken Sie auf **OK**, um den Status des Servers [für einen Zeitpunkt wiederherzustellen](./howto-restore-server-portal.md), der vor dem Löschen der Tabellen liegt. Beim Wiederherstellen eines Servers im Zustand eines anderen Zeitpunkts wird ein Duplikat des ursprünglichen Servers im Zustand des von Ihnen angegebenen Zeitpunkts als neuer Server erstellt, vorausgesetzt, dass dieser Zeitpunkt innerhalb der für Ihre Dienstebene geltenden Beibehaltungsdauer liegt.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial verwenden Sie das Azure-Portal, um Folgendes zu lernen:

> [!div class="checklist"]
> * Erstellen einer Azure-Datenbank für MySQL
> * Konfigurieren der Serverfirewall
> * Verwendung des [mysql-Befehlszeilentools](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) zum Erstellen einer Datenbank
> * Laden von Beispieldaten
> * Abfragen von Daten
> * Aktualisieren von Daten
> * Wiederherstellen von Daten

[Erstellen und Verwalten von Firewallregeln für Azure-Datenbank für MySQL mithilfe des Azure-Portals](./howto-manage-firewall-using-portal.md)

