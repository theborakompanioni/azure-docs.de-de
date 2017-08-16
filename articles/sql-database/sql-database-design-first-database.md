---
title: Entwurf Ihrer ersten Azure SQL-Datenbank | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Ihre erste Azure SQL-Datenbank erstellen.
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,develop databases
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 08/03/2017
ms.author: carlrab
ms.translationtype: HT
ms.sourcegitcommit: 9633e79929329470c2def2b1d06d95994ab66e38
ms.openlocfilehash: 69cfffdae5ce2db53acc6d668dbe468c3ef22dc2
ms.contentlocale: de-de
ms.lasthandoff: 08/04/2017

---

# <a name="design-your-first-azure-sql-database"></a>Entwurf Ihrer ersten Azure SQL-Datenbank

Azure SQL-Datenbank ist eine relationale DBaaS-Lösung (Database as a Service) in Microsoft Cloud („Azure“). In diesem Tutorial erfahren Sie, wie Sie das Azure-Portal und [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) (SSMS) für folgende Zwecke verwenden: 

> [!div class="checklist"]
> * Erstellen einer Datenbank im Azure-Portal
> * Einrichten einer Firewallregel auf Serverebene im Azure-Portal
> * Herstellen einer Verbindung für die Datenbank mit SSMS
> * Erstellen von Tabellen mit SSMS
> * Massenladen von Daten mit BCP
> * Abfragen dieser Daten mit SSMS
> * Wiederherstellen der Datenbank zu einem früheren Zeitpunkt der [Point-in-Time-Wiederherstellung](sql-database-recovery-using-backups.md#point-in-time-restore) im Azure-Portal

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Damit Sie dieses Tutorial ausführen können, müssen folgende Komponenten installiert sein:
- Die neueste Version von [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) (SSMS).
- Die neueste Version von [BCP und SQLCMD](https://www.microsoft.com/download/details.aspx?id=36433).

## <a name="log-in-to-the-azure-portal"></a>Anmelden beim Azure-Portal

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.

## <a name="create-a-blank-sql-database"></a>Erstellen einer leeren SQL-­Datenbank

Eine Azure SQL-Datenbank wird mit einer definierten Gruppe von [Compute- und Speicherressourcen](sql-database-service-tiers.md) erstellt. Die Datenbank wird in einer [Azure-Ressourcengruppe](../azure-resource-manager/resource-group-overview.md) und auf einem [logischen Azure SQL-Datenbankserver](sql-database-features.md) erstellt. 

Führen Sie die folgenden Schritte aus, um eine leere SQL-­Datenbank zu erstellen. 

1. Klicken Sie in der linken oberen Ecke des Azure-Portals auf die Schaltfläche **Neu**.

2. Wählen Sie auf der Seite **Neu** die Option **Datenbanken** und dann auf der Seite **Datenbanken** die Option **SQL-Datenbank**. 

   ![Leere Datenbank erstellen](./media/sql-database-design-first-database/create-empty-database.png)

3. Geben Sie die folgenden Informationen in das SQL-Datenbank-Formular ein, wie in der obigen Abbildung dargestellt:   

   | Einstellung       | Empfohlener Wert | Beschreibung | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Datenbankname** | mySampleDatabase | Gültige Datenbanknamen finden Sie unter [Database Identifiers](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers) (Datenbankbezeichner). | 
   | **Abonnement** | Ihr Abonnement  | Ausführliche Informationen zu Ihren Abonnements finden Sie unter [Abonnements](https://account.windowsazure.com/Subscriptions). |
   | **Ressourcengruppe** | myResourceGroup | Gültige Ressourcengruppennamen finden Sie unter [Naming rules and restrictions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Benennungsregeln und Einschränkungen). |
   | **Quelle auswählen** | Leere Datenbank | Gibt an, dass eine leere Datenbank erstellt werden soll. |

4. Klicken Sie auf **Server**, um einen neuen Server für Ihre neue Datenbank zu erstellen und zu konfigurieren. Füllen Sie das Formular **Neuer Server** mit den folgenden Informationen aus: 

   | Einstellung       | Empfohlener Wert | Beschreibung | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Servername** | Ein global eindeutiger Name | Gültige Servernamen finden Sie unter [Naming rules and restrictions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Benennungsregeln und Einschränkungen). | 
   | **Serveradministratoranmeldung** | Ein gültiger Name | Gültige Anmeldenamen finden Sie unter [Database Identifiers](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers) (Datenbankbezeichner).|
   | **Kennwort** | Ein gültiges Kennwort | Ihr Kennwort muss mindestens acht Zeichen umfassen und Zeichen aus drei der folgenden Kategorien enthalten: Großbuchstaben, Kleinbuchstaben, Zahlen und nicht alphanumerische Zeichen. |
   | **Location** | Gültiger Standort | Informationen zu Regionen finden Sie unter [Azure-Regionen](https://azure.microsoft.com/regions/). |

   ![Erstellung des Datenbankservers](./media//sql-database-design-first-database/create-database-server.png)

5. Klicken Sie auf **Auswählen**.

6. Klicken Sie auf **Tarif**, um die Dienstebene und die Leistungsstufe für die neue Datenbank anzugeben. Wählen Sie für dieses Tutorial **20 DTUs** und **250** GB Speicher aus.

   ![Datenbankerstellung s1](./media/sql-database-design-first-database/create-empty-database-pricing-tier.png)

7. Klicken Sie auf **Anwenden**.  

8. Wählen Sie eine **Sortierung** für die leere Datenbank aus (verwenden Sie in diesem Tutorial den Standardwert). Weitere Informationen über Sortierungen finden Sie unter [Sortierungen](https://docs.microsoft.com/sql/t-sql/statements/collations).

9. Klicken Sie auf **Erstellen**, um die Datenbank bereitzustellen. Es dauert ungefähr eineinhalb Minuten, bis die Bereitstellung abgeschlossen ist. 

10. Klicken Sie in der Symbolleiste auf **Benachrichtigungen**, um den Bereitstellungsprozess zu überwachen.

   ![Benachrichtigung](./media/sql-database-get-started-portal/notification.png)

## <a name="create-a-server-level-firewall-rule"></a>Erstellen einer Firewallregel auf Serverebene

Der SQL-Datenbankdienst erstellt eine Firewall auf Serverebene, um zu verhindern, dass externe Anwendungen und Tools eine Verbindung mit dem Server oder Datenbanken auf dem Server herstellen – sofern keine Firewallregel erstellt wird, um die Firewall für bestimmte IP-Adressen zu öffnen. Führen Sie die hier angegebenen Schritte zum Erstellen einer [SQL-Datenbank-Firewallregel auf Serverebene](sql-database-firewall-configure.md) für die IP-Adresse Ihres Clients und Zulassen der externen Konnektivität durch die SQL-Datenbankfirewall nur für Ihre IP-Adresse aus. 

> [!NOTE]
> SQL-Datenbank kommuniziert über Port 1433. Wenn Sie versuchen, eine Verbindung aus einem Unternehmensnetzwerk heraus herzustellen, wird der ausgehende Datenverkehr über Port 1433 von der Firewall Ihres Netzwerks unter Umständen nicht zugelassen. In diesem Fall können Sie nur dann eine Verbindung mit Ihrem Azure SQL-Datenbankserver herstellen, wenn Ihre IT-Abteilung Port 1433 öffnet.
>

1. Klicken Sie nach Abschluss der Bereitstellung im Menü auf der linken Seite auf **SQL-Datenbanken**, und klicken Sie dann auf der Seite **SQL-Datenbanken** auf **mySampleDatabase**. Die Übersichtsseite für Ihre Datenbank wird geöffnet, die den vollqualifizierten Servernamen (z.B. **mynewserver20170313.database.windows.net**) und Optionen für die weitere Konfiguration enthält. Kopieren Sie diesen vollqualifizierten Servernamen zur späteren Verwendung.

   > [!IMPORTANT]
   > Sie benötigen diesen vollqualifizierten Servernamen, um in den nachfolgenden Schnellstarts eine Verbindung mit Ihrem Server und den Datenbanken herzustellen.
   > 

   ![Servername](./media/sql-database-connect-query-dotnet/server-name.png) 

2. Klicken Sie auf der Symbolleiste wie in der obigen Abbildung dargestellt auf **Serverfirewall festlegen**. Die Seite **Firewalleinstellungen** für den SQL-Datenbankserver wird geöffnet. 

   ![Serverfirewallregel](./media/sql-database-get-started-portal/server-firewall-rule.png) 


3. Klicken Sie in der Symbolleiste auf **Client-IP-Adresse hinzufügen**, um Ihre aktuelle IP-Adresse einer neuen Firewallregel hinzuzufügen. Eine Firewallregel kann Port 1433 für eine einzelne IP-Adresse oder einen Bereich von IP-Adressen öffnen.

4. Klicken Sie auf **Speichern**. Für Ihre aktuelle IP-Adresse wird eine Firewallregel auf Serverebene erstellt, und auf dem logischen Server wird Port 1433 geöffnet.

   ![Festlegen der Serverfirewallregel](./media/sql-database-get-started-portal/server-firewall-rule-set.png) 

4. Klicken Sie auf **OK**, und schließen Sie anschließend die Seite **Firewalleinstellungen**.

Sie können für diese IP-Adresse jetzt eine Verbindung mit dem SQL-Datenbankserver und den dazugehörigen Datenbanken herstellen. Verwenden Sie hierfür SQL Server Management Studio oder ein anderes Tool Ihrer Wahl sowie das zuvor erstellte Serveradministratorkonto.

> [!IMPORTANT]
> Standardmäßig ist der Zugriff über die SQL-Datenbank-Firewall für alle Azure-Dienste aktiviert. Klicken Sie auf dieser Seite auf **AUS**, um dies für alle Azure-Dienste zu deaktivieren.

## <a name="sql-server-connection-information"></a>SQL Server-Verbindungsinformationen

Rufen Sie den vollqualifizierten Servernamen für Ihren Azure SQL-Datenbankserver im Azure-Portal ab. Sie verwenden den vollqualifizierten Servernamen, um mit SQL Server Management Studio eine Verbindung mit Ihrem Server herzustellen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Wählen Sie im Menü auf der linken Seite die Option **SQL-Datenbanken**, und klicken Sie auf der Seite **SQL-Datenbanken** auf Ihre Datenbank. 
3. Suchen Sie im Azure-Portal auf der Seite für Ihre Datenbank unter **Zusammenfassung** nach Ihrer Datenbank, und kopieren Sie den **Servernamen**.

   ![Verbindungsinformationen](./media/sql-database-connect-query-dotnet/server-name.png)

## <a name="connect-to-the-database-with-ssms"></a>Herstellen einer Verbindung für die Datenbank mit SSMS

Verwenden Sie [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms), um eine Verbindung mit Ihrem Azure SQL-Datenbankserver herzustellen.

1. Öffnen Sie SQL Server Management Studio.

2. Geben Sie im Dialogfeld **Mit Server verbinden** die folgenden Informationen ein:

   | Einstellung       | Empfohlener Wert | Beschreibung | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | Servertyp | Datenbankmodul | Dieser Wert ist erforderlich. |
   | Servername | Der vollqualifizierte Servername | Der Name sollte etwa wie folgt lauten: **mynewserver20170313.database.windows.net**. |
   | Authentifizierung | SQL Server-Authentifizierung | In diesem Tutorial haben wir als einzigen Authentifizierungstyp die SQL-Authentifizierung konfiguriert. |
   | Anmeldung | Das Serveradministratorkonto | Hierbei handelt es sich um das Konto, das Sie beim Erstellen des Servers angegeben haben. |
   | Kennwort | Das Kennwort für das Serveradministratorkonto | Hierbei handelt es sich um das Kennwort, das Sie beim Erstellen des Servers angegeben haben. |

   ![Verbindung mit dem Server herstellen](./media/sql-database-connect-query-ssms/connect.png)

3. Klicken Sie im Dialogfeld **Mit Server verbinden** auf **Optionen**. Geben Sie im Abschnitt **Verbindung mit Datenbank herstellen** den Text **mySampleDatabase** ein, um eine Verbindung mit dieser Datenbank herzustellen.

   ![Herstellen einer Verbindung mit der Datenbank auf dem Server](./media/sql-database-connect-query-ssms/options-connect-to-db.png)  

4. Klicken Sie auf **Verbinden**. Die Objekt-Explorer-Fenster wird in SSMS geöffnet. 

5. Erweitern Sie im Objekt-Explorer die Option **Datenbanken** und anschließend die Option **mySampleDatabase**, um die Objekte in der Beispieldatenbank anzuzeigen.

   ![Datenbankobjekte](./media/sql-database-connect-query-ssms/connected.png)  

## <a name="create-tables-in-the-database"></a>Erstellen von Tabellen in der Datenbank 

Erstellen Sie mit [Transact-SQL-](https://docs.microsoft.com/sql/t-sql/language-reference) ein Datenbankschema mit vier Tabellen, die ein Studentenverwaltungssystem für Universitäten modellieren:

- Person
- Course (Lehrveranstaltung)
- Student
- Credit (Schein)

Die folgende Abbildung zeigt, wie diese Tabellen miteinander verknüpft sind. Aus einigen dieser Tabellen wird auf Spalten in anderen Tabellen verwiesen. Beispielsweise wird aus der Tabelle „Student“ auf die **PersonId**-Spalte der Tabelle **Person** verwiesen. Sehen Sie sich die Abbildung an, um zu verstehen, wie die Tabellen in diesem Tutorial miteinander verknüpft sind. Eine ausführliche Beschreibung, wie effiziente Datenbanktabellen erstellt werden, finden Sie unter [Erstellen von effizienten Datenbanktabellen](https://msdn.microsoft.com/library/cc505842.aspx). Informationen zum Auswählen von Datentypen finden Sie unter [Datentypen](https://docs.microsoft.com/sql/t-sql/data-types/data-types-transact-sql).

> [!NOTE]
> Sie können auch den [Tabellen-Designer in SQL Server Management Studio](https://msdn.microsoft.com/library/hh272695.aspx) verwenden, um diese Tabellen zu entwerfen und zu erstellen. 

![Tabellenbeziehungen](./media/sql-database-design-first-database/tutorial-database-tables.png)

1. Klicken Sie im Objekt-Explorer mit der rechten Maustaste auf **mySampleDatabase**, und wählen Sie **Neue Abfrage**. Ein leeres Abfragefenster mit einer Verbindung mit Ihrer Datenbank wird geöffnet.

2. Führen Sie im Abfragefenster die folgende Abfrage aus, um vier Tabellen in der Datenbank zu erstellen: 

   ```sql 
   -- Create Person table

   CREATE TABLE Person
   (
   PersonId   INT IDENTITY PRIMARY KEY,
   FirstName   NVARCHAR(128) NOT NULL,
   MiddelInitial NVARCHAR(10),
   LastName   NVARCHAR(128) NOT NULL,
   DateOfBirth   DATE NOT NULL
   )
   
   -- Create Student table
 
   CREATE TABLE Student
   (
   StudentId INT IDENTITY PRIMARY KEY,
   PersonId  INT REFERENCES Person (PersonId),
   Email   NVARCHAR(256)
   )
   
   -- Create Course table
 
   CREATE TABLE Course
   (
   CourseId  INT IDENTITY PRIMARY KEY,
   Name   NVARCHAR(50) NOT NULL,
   Teacher   NVARCHAR(256) NOT NULL
   ) 

   -- Create Credit table
 
   CREATE TABLE Credit
   (
   StudentId   INT REFERENCES Student (StudentId),
   CourseId   INT REFERENCES Course (CourseId),
   Grade   DECIMAL(5,2) CHECK (Grade <= 100.00),
   Attempt   TINYINT,
   CONSTRAINT  [UQ_studentgrades] UNIQUE CLUSTERED
   (
   StudentId, CourseId, Grade, Attempt
   )
   )
   ```

   ![Erstellen von Tabellen.](./media/sql-database-design-first-database/create-tables.png)

3. Erweitern Sie in SQL Server Management Studio im Objekt-Explorer den Knoten „Tabellen“, damit die von Ihnen erstellten Tabellen angezeigt werden.

   ![Erstellte Tabellen in SMS](./media/sql-database-design-first-database/ssms-tables-created.png)

## <a name="load-data-into-the-tables"></a>Laden von Daten in die Tabellen

1. Erstellen Sie in Ihrem Ordner „Downloads“ einen Ordner namens **SampleTableData**, in dem die Beispieldaten für Ihre Datenbank gespeichert werden. 

2. Klicken Sie mit der rechten Maustaste auf die folgenden Links, und speichern Sie sie im Ordner **SampleTableData**. 

   - [SampleCourseData](https://sqldbtutorial.blob.core.windows.net/tutorials/SampleCourseData)
   - [SamplePersonData](https://sqldbtutorial.blob.core.windows.net/tutorials/SamplePersonData)
   - [SampleStudentData](https://sqldbtutorial.blob.core.windows.net/tutorials/SampleStudentData)
   - [SampleCreditData](https://sqldbtutorial.blob.core.windows.net/tutorials/SampleCreditData)

3. Öffnen Sie ein Eingabeaufforderungsfenster, und navigieren Sie zum Ordner „SampleTableData“.

4. Führen Sie die folgenden Befehle aus, um die Beispieldaten in Ihre Tabellen einzufügen. Ersetzen Sie dazu die Werte für **ServerName**, **DatabaseName**, **UserName** und **Password** durch die Werte für Ihre Umgebung.
  
   ```bcp
   bcp Course in SampleCourseData -S <ServerName>.database.windows.net -d <DatabaseName> -U <Username> -P <password> -q -c -t ","
   bcp Person in SamplePersonData -S <ServerName>.database.windows.net -d <DatabaseName> -U <Username> -P <password> -q -c -t ","
   bcp Student in SampleStudentData -S <ServerName>.database.windows.net -d <DatabaseName> -U <Username> -P <password> -q -c -t ","
   bcp Credit in SampleCreditData -S <ServerName>.database.windows.net -d <DatabaseName> -U <Username> -P <password> -q -c -t ","
   ```

Sie haben jetzt Beispieldaten in die Tabellen geladen, die Sie zuvor erstellt haben.

## <a name="query-data"></a>Abfragen von Daten

Führen Sie die folgenden Abfragen aus, um Informationen aus den Datenbanktabellen abzurufen. Weitere Informationen zum Schreiben von SQL-Abfragen finden Sie unter [Schreiben von SQL-Abfragen](https://technet.microsoft.com/library/bb264565.aspx). In der ersten Abfrage werden alle vier Tabellen verknüpft, um alle Studenten zu finden, die von „Dominick Pope“ unterrichtet werden und in dessen Kurs ein Ergebnis (Grade) haben, das über 75 % liegt. In der zweite Abfrage werden alle vier Tabellen verknüpft und alle Lehrveranstaltungen gefunden, für die „Noe Coleman“ jemals eingeschrieben war.

1. Führen Sie in einem Abfragefenster von SQL Server Management Studio die folgende Abfrage aus:

   ```sql 
   -- Find the students taught by Dominick Pope who have a grade higher than 75%

   SELECT  person.FirstName,
   person.LastName,
   course.Name,
   credit.Grade
   FROM  Person AS person
   INNER JOIN Student AS student ON person.PersonId = student.PersonId
   INNER JOIN Credit AS credit ON student.StudentId = credit.StudentId
   INNER JOIN Course AS course ON credit.CourseId = course.courseId
   WHERE course.Teacher = 'Dominick Pope' 
   AND Grade > 75
   ```

2. Führen Sie in einem Abfragefenster von SQL Server Management Studio folgende Abfrage aus:

   ```sql
   -- Find all the courses in which Noe Coleman has ever enrolled

   SELECT  course.Name,
   course.Teacher,
   credit.Grade
   FROM  Course AS course
   INNER JOIN Credit AS credit ON credit.CourseId = course.CourseId
   INNER JOIN Student AS student ON student.StudentId = credit.StudentId
   INNER JOIN Person AS person ON person.PersonId = student.PersonId
   WHERE person.FirstName = 'Noe'
   AND person.LastName = 'Coleman'
   ```

## <a name="restore-a-database-to-a-previous-point-in-time"></a>Wiederherstellen eines früheren Zustands einer Datenbank

Stellen Sie sich vor, Sie haben versehentlich eine Tabelle gelöscht. Dies ist eine Situation, in der Sie die Datenbank nicht einfach wiederherstellen können. Azure SQL-Datenbank ermöglicht es Ihnen, zu einem beliebigen Zeitpunkt innerhalb der letzten 35 Tage zurückzugehen und diesen Zeitpunkt in einer neuen Datenbank wiederherzustellen. Diese Datenbank können Sie dann verwenden, um die gelöschten Daten wiederherzustellen. In den folgenden Schritten wird die Beispieldatenbank für einen Zeitpunkt wiederhergestellt, der vor dem Hinzufügen der Tabellen lag.

1. Klicken Sie auf der Seite „SQL-Datenbank“ für Ihre Datenbank auf der Symbolleiste auf **Wiederherstellen**. Die Seite **Wiederherstellen** wird geöffnet.

   ![Wiederherstellen](./media/sql-database-design-first-database/restore.png)

2. Geben Sie im Formular **Wiederherstellen** die erforderlichen Informationen ein:
    * Datenbankname: Geben Sie einen Datenbanknamen an. 
    * Zeitpunkt: Wählen Sie die Registerkarte **Zeitpunkt** im Formular „Wiederherstellen“ aus. 
    * Wiederherstellungspunkt: Wählen Sie einen Zeitpunkt vor der Änderung der Datenbank aus.
    * Zielserver: Sie können diesen Wert beim Wiederherstellen einer Datenbank nicht ändern. 
    * Pool für elastische Datenbanken: Wählen Sie **Keiner** aus.  
    * Tarif: Wählen Sie **20 DTUs** und **250 GB** Speicher aus.

   ![Wiederherstellungspunkt](./media/sql-database-design-first-database/restore-point.png)

3. Klicken Sie auf **OK**, um die Datenbank [für einen Zeitpunkt wiederherzustellen](sql-database-recovery-using-backups.md#point-in-time-restore), der vor dem Hinzufügen der Tabellen lag. Beim Wiederherstellen einer Datenbank im Zustand eines früheren Zeitpunkts wird ein Duplikat der Datenbank für den von Ihnen angegebenen Zeitpunkt auf demselben Server wie die ursprüngliche Datenbank hergestellt, solange sich dieser Zeitpunkt innerhalb der für Ihre [Dienstebene](sql-database-service-tiers.md) geltenden Beibehaltungsdauer befindet.

## <a name="next-steps"></a>Nächste Schritte 
In diesem Tutorial wurden grundlegende Datenbankaufgaben behandelt, z.B. das Erstellen einer Datenbank und von Tabellen, das Laden und Abfragen von Daten und das Wiederherstellen der Datenbank zu einem früheren Zeitpunkt. Es wurde Folgendes vermittelt:
> [!div class="checklist"]
> * Erstellen einer Datenbank
> * Einrichten einer Firewallregel
> * Herstellen einer Verbindung mit der Datenbank mit [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) (SSMS)
> * Erstellen von Tabellen.
> * Massenladen von Daten
> * Abfragen der Daten
> * Wiederherstellen eine früheren Zeitpunkts für die Datenbank mit den SQL-Datenbank-Funktionen für die [Point-in-Time-Wiederherstellung](sql-database-recovery-using-backups.md#point-in-time-restore)

Im nächsten Tutorial erfahren Sie, wie Sie eine Datenbank mit Visual Studio und C# entwerfen.

> [!div class="nextstepaction"]
>[Entwerfen einer Azure SQL-Datenbank und Herstellen einer Verbindung mit C# und ADO.NET](sql-database-design-first-database-csharp.md)

