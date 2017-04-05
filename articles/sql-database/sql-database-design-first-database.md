---
title: Entwurf Ihrer ersten Azure SQL-Datenbank | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Ihre erste Azure SQL-Datenbank erstellen.
services: sql-database
documentationcenter: 
author: janeng
manager: jstrauss
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: tutorial
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 03/23/2017
ms.author: janeng
translationtype: Human Translation
ms.sourcegitcommit: 07635b0eb4650f0c30898ea1600697dacb33477c
ms.openlocfilehash: 313bcf4fbc0ab7f251dd62b1e2151afef8392a55
ms.lasthandoff: 03/28/2017


---

# <a name="design-your-first-azure-sql-database"></a>Entwurf Ihrer ersten Azure SQL-Datenbank

In diesem Tutorial verwenden Sie das Azure-Portal, um eine Datenbank auf einem neuen Server mit einer Firewall auf Serverebene zu erstellen. Sie verwenden anschließend SQL Server Management Studio, um eine Tabelle zu erstellen, Daten in diese Tabelle zu laden, die Tabelle abzufragen und der Tabelle einen Index hinzuzufügen. Abschließend verwenden Sie die automatisierten Sicherungen des SQL-Datenbankdiensts, um die Datenbank in einem früheren Zustand wiederherzustellen, als diese neue Tabelle noch nicht vorhanden war.

Stellen Sie zur Durchführung dieses Tutorials sicher, dass Sie die neueste Version von [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) (SSMS) installiert haben. 

## <a name="step-1---log-in-to-the-azure-portal"></a>Schritt 1: Anmeldung beim Azure-Portal

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

## <a name="step-2---create-a-sql-database"></a>Schritt 2: Erstellen einer SQL-Datenbank

Eine Azure SQL-Datenbank wird mit einer definierten Gruppe von [Compute- und Speicherressourcen](sql-database-service-tiers.md) erstellt. Die Datenbank wird in einer [Azure-Ressourcengruppe](../azure-resource-manager/resource-group-overview.md) und auf einem [logischen Azure SQL-Datenbankserver](sql-database-features.md) erstellt. 

Führen Sie diese Schritte aus, um eine SQL-Datenbank mit den Adventure Works LT-Beispieldaten zu erstellen. 

1. Klicken Sie in der linken oberen Ecke des Azure-Portals auf die Schaltfläche **Neu**.

2. Wählen Sie auf der Seite **Neu** die Option **Datenbanken** und dann auf der Seite **Datenbanken** die Option **SQL-Datenbank**.

3. Geben Sie die erforderlichen Informationen in das Formular für die SQL-Datenbank ein: 
   - Datenbankname: Geben Sie einen Datenbanknamen an.
   - Abonnement: Wählen Sie Ihr Abonnement aus.
   - Ressourcengruppe: Wählen Sie eine neue oder vorhandene Ressourcengruppe aus.
   - Quelle: Wählen Sie **Beispiel (AdventureWorksLT)**.
   - Server: Erstellen Sie einen neuen Server (der Name für **Server** muss global eindeutig sein).
   - Elastischer Pool: Wählen Sie für diesen Schnellstart die Option **Nicht jetzt**.
   - Tarif: Wählen Sie **20 DTUs** und **250** GB Speicher.
   - Sortierung: Sie können diesen Wert beim Importieren der Beispieldatenbank nicht ändern. 
   - An Dashboard anheften: Aktivieren Sie dieses Kontrollkästchen.

      ![Datenbank erstellen](./media/sql-database-get-started/create-database-s1.png)

4. Klicken Sie auf **Erstellen**, wenn Sie alles eingegeben haben. Die Bereitstellung dauert einige Minuten.
5. Nachdem die Bereitstellung der SQL-Datenbank beendet wurde, können Sie im Dashboard die Option **SQL-Datenbanken** wählen oder im Menü auf der linken Seite auf **SQL-Datenbanken** klicken und dann auf der Seite **SQL-Datenbanken** auf die neue Datenbank klicken. Eine Übersichtsseite für Ihre Datenbank wird geöffnet, die den vollqualifizierten Servernamen (z.B. **mynewserver20170313.database.windows.net**) und Optionen für die weitere Konfiguration enthält.

      ![Neue SQL-Datenbank](./media/sql-database-get-started/new-database-s1-overview.png) 

## <a name="step-3---create-a-server-level-firewall-rule"></a>Schritt 3: Erstellen einer Firewallregel auf Serverebene

Mit dem SQL-Datenbankdienst wird eine Firewall erstellt, die verhindert, dass externe Anwendungen und Tools eine Verbindung mit Ihrem Server und der Datenbank herstellen. Führen Sie die hier angegebenen Schritte zum Erstellen einer [SQL-Datenbank-Firewallregel auf Serverebene](sql-database-firewall-configure.md) für Ihre IP-Adresse aus, um die externe Verbindung durch die Firewall der SQL-Datenbank zuzulassen. 

1. Klicken Sie in der Symbolleiste für Ihre Datenbank auf **Set server firewall** (Serverfirewall festlegen). Die Seite **Firewalleinstellungen** für den SQL-Datenbankserver wird geöffnet. 

      ![Serverfirewallregel](./media/sql-database-get-started/server-firewall-rule.png) 

2. Klicken Sie in der Symbolleiste auf **Client-IP-Adresse hinzufügen** und dann auf **Speichern**. Eine Firewallregel auf Serverebene wird für Ihre aktuelle IP-Adresse erstellt.

3. Klicken Sie auf **OK** und dann auf das **X**, um die Seite mit den Firewalleinstellungen zu schließen.

Nun können Sie die Verbindung mit der Datenbank und dem zugehörigen Server herstellen, indem Sie SQL Server Management Studio oder ein anderes Tool Ihrer Wahl verwenden.

## <a name="step-4---get-connection-information"></a>Schritt 4: Abrufen von Verbindungsinformationen

Rufen Sie den vollqualifizierten Servernamen für Ihren Azure SQL-Datenbankserver im Azure-Portal ab. Sie verwenden den vollqualifizierten Servernamen, um mit SQL Server Management Studio eine Verbindung mit Ihrem Server herzustellen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Wählen Sie im Menü auf der linken Seite die Option **SQL-Datenbanken**, und klicken Sie auf der Seite **SQL-Datenbanken** auf Ihre Datenbank. 
3. Suchen Sie im Azure-Portal auf der Seite für Ihre Datenbank unter **Zusammenfassung** nach Ihrer Datenbank, und kopieren Sie den **Servernamen**.

    <img src="./media/sql-database-connect-query-ssms/connection-information.png" alt="connection information" style="width: 780px;" />

## <a name="step-5---connect-to-the-server-using-ssms"></a>Schritt 5: Herstellen einer Verbindung mit dem Server unter Verwendung von SSMS

Verwenden Sie SQL Server Management Studio, um eine Verbindung mit Ihrem Azure SQL-Datenbankserver einzurichten.

1. Geben Sie im Windows-Suchfeld den Suchbegriff **SSMS** ein, und klicken Sie auf die **Eingabeschaltfläche**, um SSMS zu öffnen.

2. Geben Sie im Dialogfeld **Mit Server verbinden** die folgenden Informationen ein:
   - **Servertyp**: Geben Sie das Datenbankmodul ein.
   - **Servername**: Geben Sie den vollqualifizierten Servernamen ein, z.B. **mynewserver20170313.database.windows.net**.
   - **Authentifizierung**: Geben Sie die SQL Server-Authentifizierung an.
   - **Anmeldung**: Geben Sie Ihr Serveradministratorkonto ein.
   - **Kennwort**: Geben Sie das Kennwort für Ihr Serveradministratorkonto ein.
 
    <img src="./media/sql-database-connect-query-ssms/connect.png" alt="connect to server" style="width: 780px;" />

3. Klicken Sie auf **Verbinden**. Die Objekt-Explorer-Fenster wird in SSMS geöffnet. 

    <img src="./media/sql-database-connect-query-ssms/connected.png" alt="connected to server" style="width: 780px;" />

4. Erweitern Sie im Objekt-Explorer die Option **Datenbanken** und anschließend die Option **mySampleDatabase**, um die Objekte in der Beispieldatenbank anzuzeigen.

## <a name="step-6---create-and-query-a-table"></a>Schritt 6: Erstellen und Abfragen einer Tabelle 
1. Klicken Sie im Objekt-Explorer mit der rechten Maustaste auf **mySampleDatabase**, und wählen Sie **Neue Abfrage**. Ein leeres Abfragefenster mit einer Verbindung mit Ihrer Datenbank wird geöffnet.
2. Führen Sie im Abfragefenster die folgende Abfrage aus:

   ```sql 
   CREATE TABLE [dbo].[Students]
   (
     [student_id] int, 
     [name] varchar(100),
     [age] int,
     [email] varchar(100),
     [AddressID] int REFERENCES [SalesLT].[Address] (AddressID)
   );
   ```

   Nach Abschluss der Abfrage haben Sie eine leere Tabelle in Ihrer Datenbank namens „Students“ erstellt.

3. Führen Sie in einem SSMS-Abfragefenster die folgende Abfrage aus: 

   ```sql
   SELECT name, age, email 
   FROM [dbo].[Students]
   ```

   Die Tabelle „Students“ gibt keine Daten zurück.

## <a name="step-7---load-data-into-the-table"></a>Schritt 7: Laden von Daten in die Tabelle 
1. Öffnen Sie ein Eingabeaufforderungsfenster.

2. Führen Sie den folgenden PowerShell-Befehl aus, um eine Beispieltextdatei in Ihr aktuelles Verzeichnis herunterzuladen.

   ```powershell
   powershell -command "& { (New-Object Net.WebClient).DownloadFile('https://sqldbtutorial.blob.core.windows.net/tutorials/SampleStudentData.txt', 'SampleStudentData.txt'); echo 'Download complete' }" 
   ``` 

3. Führen Sie nach dem Abschluss des Vorgangs den folgenden Befehl aus, um 1.000 Zeilen in die Tabelle „Students“ einzufügen. Ersetzen Sie hierbei die Werte für **ServerName**, **DatabaseName**, **UserName** und **Password** durch Werte für Ihre Umgebung.

   ```bcp
   bcp Students in SampleStudentData.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <password> -q -c -t ","
   ```

Sie haben jetzt Beispieldaten in die Tabelle geladen, die Sie zuvor erstellt haben.

## <a name="step-8---add-an-index-to-a-table"></a>Schritt 8: Hinzufügen eines Index zu einer Tabelle
Um in der Tabelle effizienter nach bestimmten Werten suchen zu können, erstellen Sie einen Index für die Tabelle „Students“. Mit einem Index werden die Daten so strukturiert, dass jetzt alle Daten durchsucht werden müssen, um einen bestimmten Wert zu finden.

1. Führen Sie in einem SSMS-Abfragefenster die folgende Abfrage aus:

   ```sql 
   CREATE NONCLUSTERED INDEX IX_Age ON Students (age);
   ```

2. Führen Sie in einem SSMS-Abfragefenster die folgende Abfrage aus:

   ```sql
   SELECT name, age, email 
   FROM [dbo].[Students]
   WHERE age > 20
   ```

   Diese Abfrage gibt den Namen, das Alter und die E-Mail-Adresse der Studenten zurück, die älter sind als 20 Jahre.

## <a name="step-9---restore-a-database-to-a-point-in-time"></a>Schritt 9: Wiederherstellen einer Datenbank bis zu einem bestimmten Zeitpunkt 
Für Datenbanken in Azure werden [fortlaufende Sicherungen](sql-database-automated-backups.md) durchgeführt, die automatisch alle 5 bis 10 Minuten erstellt werden. Diese Sicherungen ermöglichen es Ihnen, Ihre Datenbank mit dem Stand zu einem früheren Zeitpunkt wiederherzustellen. Beim Wiederherstellen einer Datenbank in einem Zustand zu einem früheren Zeitpunkt wird ein Duplikat der Datenbank mit dem von Ihnen angegebenen Zeitpunkt auf demselben Server wie die ursprüngliche Datenbank hergestellt (innerhalb der für Ihren Diensttarif geltenden Beibehaltungsdauer). Mithilfe der folgenden Schritte wird die Beispieldatenbank zu einem Zeitpunkt wiederhergestellt, in dem sie sich vor dem Hinzufügen der Tabelle **Students** befand. 

1. Klicken Sie auf der Seite „SQL-Datenbank“ für Ihre Datenbank auf der Symbolleiste auf **Wiederherstellen**. Die Seite **Wiederherstellen** wird geöffnet.

    <img src="./media/sql-database-design-first-database/restore.png" alt="restore" style="width: 780px;" />

2. Geben Sie im Formular **Wiederherstellen** die erforderlichen Informationen ein:
    * Datenbankname: Geben Sie einen Datenbanknamen an. 
    * Zeitpunkt: Wählen Sie die Registerkarte **Zeitpunkt** im Formular „Wiederherstellen“ aus. 
    * Wiederherstellungspunkt: Wählen Sie einen Zeitpunkt vor der Änderung der Datenbank aus.
    * Zielserver: Sie können diesen Wert beim Wiederherstellen einer Datenbank nicht ändern. 
    * Pool für elastische Datenbanken: Wählen Sie **Keiner** aus.  
    * Tarif: Wählen Sie **20 DTUs** und **250 GB** Speicher aus.

    <img src="./media/sql-database-design-first-database/restore-point.png" alt="restore-point" style="width: 780px;" />

3. Klicken Sie auf **OK**, um die Datenbank zu einem Zeitpunkt wiederherzustellen, in dem sie sich vor dem Hinzufügen der Tabelle *Students* befand.

## <a name="next-steps"></a>Nächste Schritte 
PowerShell-Beispiele für gängige Aufgaben finden Sie unter [Azure PowerShell-Beispiele für Azure SQL-Datenbank](sql-database-powershell-samples.md).

