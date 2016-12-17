---
title: "Aktivieren von Stretch-Datenbank für eine Datenbank | Microsoft Docs"
description: "Erfahren Sie, wie Sie eine Datenbank für Stretch-Datenbank konfigurieren."
services: sql-server-stretch-database
documentationcenter: 
author: douglaslMS
manager: jhubbard
editor: 
ms.assetid: 55fc4142-4be9-4664-8ea9-48a5e177838f
ms.service: sql-server-stretch-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/05/2016
ms.author: douglasl
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: c6d3a81c3c919e50c123cef00dfb3e4a5ab23a44


---
# <a name="enable-stretch-database-for-a-database"></a>Aktivieren von Stretch-Datenbank für eine Datenbank
Um eine vorhandene Datenbank für Stretch-Datenbank zu konfigurieren, wählen Sie **Aufgaben > Stretch > Aktivieren** für eine Datenbank in SQL Server Management Studio aus, um den Assistenten zum **Aktivieren einer Datenbank für Stretch** zu öffnen. Sie können auch Transact\-SQL verwenden, um Stretch-Datenbank für eine Datenbank zu aktivieren.

Wenn Sie **Aufgaben | Stretch | Aktivieren** für eine einzelne Tabelle auswählen und Sie die Datenbank noch nicht für Stretch-Datenbank aktiviert haben, konfiguriert der Assistent die Datenbank für Stretch-Datenbank und ermöglicht Ihnen im Zuge dieses Vorgangs auch die Auswahl von Tabellen. Führen Sie die Schritte in diesem Thema anstelle der Schritte in [Aktivieren von Stretch-Datenbank für eine Tabelle](sql-server-stretch-database-enable-database.md)aus.

Für das Aktivieren von Stretch-Datenbank für eine Datenbank oder eine Tabelle sind db\_owner-Berechtigungen erforderlich. Für das Aktivieren von Stretch-Datenbank für eine Datenbank sind ebenfalls CONTROL DATABASE-Berechtigungen erforderlich.

> [!NOTE]
> Bedenken Sie später beim Deaktivieren von Stretch-Datenbank Folgendes: Wenn Sie Stretch-Datenbank für eine Tabelle oder eine Datenbank deaktivieren, wird das Remoteobjekt nicht gelöscht. Wenn Sie die Remotetabelle oder die Remotedatenbank löschen möchten, müssen Sie sie mithilfe des Azure-Verwaltungsportals entfernen. Für die Remoteobjekte fallen weiterhin Azure-Kosten an, bis Sie die Objekte manuell löschen.
> 
> 

## <a name="before-you-get-started"></a>Bevor Sie beginnen
* Bevor Sie eine Datenbank für Stretch konfigurieren, empfiehlt sich das Ausführen von Stretch Database Advisor, um Datenbanken und Tabellen zu identifizieren, die für Stretch berechtigt sind. Der Stretch Database Advisor erkennt auch Hindernisse. Weitere Informationen finden Sie unter [Identifizieren von Datenbanken und Tabellen für Stretch-Datenbank durch Ausführen des Ratgebers für Stretch-Datenbank](sql-server-stretch-database-identify-databases.md).
* Lesen Sie [Einschränkungen für Stretch-Datenbank](sql-server-stretch-database-limitations.md).
* Stretch-Datenbank migriert Daten in Azure. Daher benötigen Sie für die Abrechnung ein Azure-Konto und ein Abonnement. [Klicken Sie hier](http://azure.microsoft.com/pricing/free-trial/), um ein Azure-Konto zu erstellen.
* Halten Sie die Verbindungs- und Anmeldedaten bereit, die Sie zum Erstellen eines neuen Azure-Servers oder zum Auswählen eines vorhandenen Azure-Servers benötigen.

## <a name="a-nameenabletsqlserveraprerequisite-enable-stretch-database-on-the-server"></a><a name="EnableTSQLServer"></a>Voraussetzung: Aktivieren von Stretch-Datenbank auf dem Server
Bevor Sie Stretch-Datenbank für eine Datenbank oder eine Tabelle aktivieren können, müssen Sie sie auf dem lokalen Server aktivieren. Für diesen Vorgang sind sysadmin- oder serveradmin-Berechtigungen erforderlich.

* Wenn Sie über die erforderlichen Administratorberechtigungen verfügen, konfiguriert der Assistent **Datenbank für Streckung aktivieren** den Server für Stretch.
* Wenn Sie nicht über die erforderlichen Berechtigungen verfügen, muss ein Administrator die Option manuell aktivieren, indem er vor dem Ausführen des Assistenten **sp\_configure** ausführt, oder der Assistent muss von einem Administrator ausgeführt werden.

Wenn Sie Stretch-Datenbank auf dem Server manuell aktivieren möchten, führen Sie **sp\_configure** aus, und aktivieren Sie die Option **Remotedatenarchiv**. Im folgenden Beispiel wird die Remotedatenarchiv-Option **** durch Festlegen auf den Wert „1“ aktiviert.

```
EXEC sp_configure 'remote data archive' , '1';
GO

RECONFIGURE;
GO
```
Weitere Informationen finden Sie unter [Konfigurieren der Serverkonfigurationsoption "Remote Data Archive"](https://msdn.microsoft.com/library/mt143175.aspx) sowie unter [sp_configure (Transact-SQL)](https://msdn.microsoft.com/library/ms188787.aspx).

## <a name="a-namewizardause-the-wizard-to-enable-stretch-database-on-a-database"></a><a name="Wizard"></a>Verwenden des Assistenten zum Aktivieren von Stretch-Datenbank für eine Datenbank
Informationen zum Assistenten „Datenbank für Streckung aktivieren“ (einschließlich der von Ihnen einzugebenden Informationen und der auszuwählenden Angaben) finden Sie unter [Get started by running the Enable Database for Stretch Wizard](sql-server-stretch-database-wizard.md)(Erste Schritte mit dem Assistenten „Datenbank für Streckung aktivieren“).

## <a name="a-nameenabletsqldatabaseause-transact-sql-to-enable-stretch-database-on-a-database"></a><a name="EnableTSQLDatabase"></a>Verwenden von Transact\-SQL zum Aktivieren von Stretch-Datenbank für eine Datenbank
Bevor Sie Stretch-Datenbank für einzelne Tabellen aktivieren können, müssen Sie sie auf dem lokalen Server aktivieren.

Für das Aktivieren von Stretch-Datenbank für eine Datenbank oder eine Tabelle sind db\_owner-Berechtigungen erforderlich. Für das Aktivieren von Stretch-Datenbank für eine Datenbank sind ebenfalls CONTROL DATABASE-Berechtigungen erforderlich.

1. Wählen Sie vor Beginn einen vorhandenen Azure-Server für die Daten aus, die Stretch-Datenbank migriert, oder erstellen Sie einen neuen Azure-Server.
2. Erstellen Sie auf dem Azure-Server eine Firewallregel mit dem IP-Adressbereich der SQL Server-Instanz, sodass SQL Server mit dem Remoteserver kommunizieren kann.
   
   Sie können die benötigten Werte leicht ermitteln und die Firewallregel erstellen, indem Sie versuchen, über den Objekt-Explorer in SQL Server Management Studio (SSMS) eine Verbindung mit dem Azure-Server herzustellen. SSMS unterstützt Sie beim Erstellen der Regel, indem das folgende Dialogfeld geöffnet wird, in dem die erforderlichen IP-Adressenwerte bereits enthalten sind.
   
   ![Erstellen einer Firewallregel in SSMS][FirewallRule]
3. Zum Konfigurieren einer SQL Server-Datenbank für Stretch-Datenbank muss die Datenbank über einen Datenbankhauptschlüssel verfügen. Der Datenbankhauptschlüssel sichert die Anmeldeinformationen, die Stretch-Datenbank für die Verbindung mit der Remotedatenbank verwendet. Hier sehen Sie ein Beispiel für das Erstellen eines neuen Datenbank-Hauptschlüssels.
   
   ```tsql
   USE <database>;
   GO
   
   CREATE MASTER KEY ENCRYPTION BY PASSWORD ='<password>';
   GO
   ```
   
   Weitere Informationen zum Datenbankhauptschlüssel finden Sie unter [CREATE MASTER KEY (Transact-SQL)](https://msdn.microsoft.com/library/ms174382.aspx) sowie unter [Erstellen eines Datenbank-Hauptschlüssels](https://msdn.microsoft.com/library/aa337551.aspx).
4. Wenn Sie eine Datenbank für Stretch-Datenbank konfigurieren, müssen Sie die Anmeldeinformationen für Stretch-Datenbank angeben, um die Kommunikation zwischen dem lokalen SQL Server und dem Azure-Remoteserver zu ermöglichen. Sie haben zwei Möglichkeiten.
   
   * Sie können die Anmeldeinformationen des Administrators bereitstellen.
     
     * Wenn Sie Stretch-Datenbank durch Ausführen des Assistenten aktivieren, können Sie die Anmeldeinformationen zu diesem Zeitpunkt erstellen.
     * Wenn Sie Stretch-Datenbank mithilfe von **ALTER DATABASE** aktivieren möchten, müssen Sie die Anmeldeinformationen manuell erstellen, bevor Sie **ALTER DATABASE** zum Aktivieren von Stretch-Datenbank ausführen.
     
     Hier sehen Sie ein Beispiel für das Erstellen der neuen Anmeldeinformationen.
     
     ```tsql
     CREATE DATABASE SCOPED CREDENTIAL <db_scoped_credential_name>
         WITH IDENTITY = '<identity>' , SECRET = '<secret>';
     GO
     ```
     
     Weitere Informationen zu den Anmeldeinformationen finden Sie unter [CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)](https://msdn.microsoft.com/library/mt270260.aspx). Zum Erstellen der Anmeldeinformationen sind die ALTER ANY CREDENTIAL-Berechtigungen erforderlich.
   * Sie können ein Verbunddienstkonto für den SQL Server verwenden, um mit dem Azure-Remoteserver zu kommunizieren, wenn alle der folgenden Bedingungen zutreffen.
     
     * Das Dienstkonto, unter dem die Instanz von SQL Server ausgeführt wird, ist ein Domänenkonto.
     * Das Domänenkonto gehört zu einer Domäne, deren Active Directory mit Azure Active Directory verbunden ist.
     * Der Azure-Remoteserver wird für die Unterstützung der Azure Active Directory-Authentifizierung konfiguriert.
     * Das Dienstkonto, unter dem die Instanz von SQL Server ausgeführt wird, muss als dbmanager- oder sysadmin-Konto auf dem Azure-Remoteserver konfiguriert werden.
5. Um eine Datenbank für Stretch-Datenbank zu konfigurieren, führen Sie den Befehl ALTER DATABASE aus.
   
   1. Stellen Sie für das SERVER-Argument den Namen eines vorhandenen Azure-Servers (einschließlich des `.database.windows.net`\-Teils des Namens) bereit. Beispiel: `MyStretchDatabaseServer.database.windows.net`.
   2. Stellen Sie vorhandene Administratoranmeldeinformationen mit dem CREDENTIAL-Argument bereit, oder geben Sie FEDERATED\_SERVICE\_ACCOUNT = ON an. Das folgenden Beispiel enthält vorhandene Anmeldeinformationen:
   
   ```tsql
   ALTER DATABASE <database name>
       SET REMOTE_DATA_ARCHIVE = ON
           (
               SERVER = '<server_name>',
               CREDENTIAL = <db_scoped_credential_name>
           ) ;
   GO
   ```

## <a name="next-steps"></a>Nächste Schritte
* [Enable Stretch Database for a table](sql-server-stretch-database-enable-table.md) (Aktivieren von Stretch-Datenbank für eine Tabelle) zum Aktivieren zusätzlicher Tabellen
* [Monitor Stretch Database](sql-server-stretch-database-monitor.md) (Überwachen von Stretch-Datenbank) zum Anzeigen des Status der Datenmigration
* [Anhalten und Fortsetzen von Stretch-Datenbank](sql-server-stretch-database-pause.md)
* [Verwalten von Stretch-Datenbank und Behandeln von Problemen](sql-server-stretch-database-manage.md)
* [Sichern von Stretch-fähigen Datenbanken](sql-server-stretch-database-backup.md)

## <a name="see-also"></a>Siehe auch
[Identifizieren von Datenbanken und Tabellen für Stretch-Datenbank](sql-server-stretch-database-identify-databases.md)

[ALTER DATABASE SET-Optionen (Transact-SQL)](https://msdn.microsoft.com/library/bb522682.aspx)

[FirewallRule]: ./media/sql-server-stretch-database-enable-database/firewall.png



<!--HONumber=Nov16_HO3-->


