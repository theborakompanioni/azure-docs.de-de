
<a name="cs_0_csharpprogramexample_h2"/>

<a id="c-program-example" class="xliff"></a>

## Beispiel für C#-Programm

In den nächsten Abschnitten dieses Artikels wird ein C#-Programm vorgestellt, bei dem ADO.NET zum Senden von Transact-SQL-Anweisungen an die SQL-Datenbank verwendet wird. Mit dem C#-Programm werden die folgenden Aktionen ausgeführt:

1. [Herstellen einer Verbindung mit unserer SQL-Datenbank per ADO.NET](#cs_1_connect)
2. [Erstellen von Tabellen](#cs_2_createtables)
3. [Ausfüllen der Tabellen mit Daten, indem T-SQL-INSERT-Anweisungen ausgegeben werden](#cs_3_insert)
4. [Aktualisieren der Daten per Verknüpfung (Join)](#cs_4_updatejoin)
5. [Löschen der Daten per Verknüpfung (Join)](#cs_5_deletejoin)
6. [Auswählen von Datenzeilen per Verknüpfung (Join)](#cs_6_selectrows)
7. Schließen der Verbindung (temporäre Tabellen aus tempdb verwerfen)

Das C#-Programm enthält Folgendes:

- C#-Code zum Herstellen der Verbindung mit der Datenbank
- Methoden zum Zurückgeben des T-SQL-Quellcodes
- Zwei Methoden zum Übermitteln von T-SQL an die Datenbank

<a id="to-compile-and-run" class="xliff"></a>

#### Kompilieren und Ausführen

Dieses C#-Programm ist in logischer Hinsicht eine CS-Datei. Hier ist das Programm aber physisch in mehrere Codeblöcke unterteilt, damit jeder Block leichter zu sehen und zu verstehen ist. Gehen Sie wie folgt vor, um dieses Programm zu kompilieren und auszuführen:

1. Erstellen Sie in Visual Studio ein C#-Projekt.
    - Das Projekt sollte eine *Konsolen*anwendung sein und etwa folgende Hierarchie aufweisen: **Vorlagen** > **Visual C#** > **Klassischer Windows-Desktop** > **Konsolen-App (.NET Framework)**.
3. Entfernen Sie in der Datei **Program.cs** die ersten kurzen Codezeilen.
3. Führen Sie für „Program.cs“ für die folgenden Blöcke jeweils das Kopieren und Einfügen durch, und zwar in derselben Reihenfolge, die hier angegeben ist.
4. Bearbeiten Sie in „Program.cs“ die folgenden Werte in der **Main**-Methode:

   - **cb.DataSource**
   - **cd.UserID**
   - **cb.Password**
   - **InitialCatalog**

5. Stellen Sie sicher, dass auf die Assembly **System.Data.dll** verwiesen wird. Erweitern Sie für die Sicherstellung den Knoten **Verweise** im **Projektmappen-Explorer**-Bereich.
6. Klicken Sie zum Erstellen des Programms in Visual Studio auf das Menü **Erstellen**.
7. Klicken Sie auf die Schaltfläche **Start**, um das Programm aus Visual Studio auszuführen. Die Berichtsausgabe wird in einem cmd.exe-Fenster angezeigt.

> [!NOTE]
> Sie können den T-SQL-Code bearbeiten, um den Tabellennamen ein **#**-Zeichen voranzustellen, damit sie in **tempdb** als temporäre Tabellen erstellt werden. Dies kann zu Demonstrationszwecken nützlich sein, wenn keine Testdatenbank verfügbar ist. Temporäre Tabellen werden automatisch gelöscht, wenn die Verbindung geschlossen wird. Verweise (REFERENCES) für Fremdschlüssel werden für temporäre Tabellen nicht erzwungen.
>

<a name="cs_1_connect"/>
<a id="c-block-1-connect-by-using-adonet" class="xliff"></a>

### C#-Block 1: Herstellen der Verbindung per ADO.NET

- [Weiter](#cs_2_createtables)


```csharp
using System;
using System.Data.SqlClient;   // System.Data.dll 
//using System.Data;           // For:  SqlDbType , ParameterDirection

namespace csharp_db_test
{
   class Program
   {
      static void Main(string[] args)
      {
         try
         {
            var cb = new SqlConnectionStringBuilder();
            cb.DataSource = "your_server.database.windows.net";
            cb.UserID = "your_user";
            cb.Password = "your_password";
            cb.InitialCatalog = "your_database";

            using (var connection = new SqlConnection(cb.ConnectionString))
            {
               connection.Open();

               Submit_Tsql_NonQuery(connection, "2 - Create-Tables",
                  Build_2_Tsql_CreateTables());

               Submit_Tsql_NonQuery(connection, "3 - Inserts",
                  Build_3_Tsql_Inserts());

               Submit_Tsql_NonQuery(connection, "4 - Update-Join",
                  Build_4_Tsql_UpdateJoin(),
                  "@csharpParmDepartmentName", "Accounting");

               Submit_Tsql_NonQuery(connection, "5 - Delete-Join",
                  Build_5_Tsql_DeleteJoin(),
                  "@csharpParmDepartmentName", "Legal");

               Submit_6_Tsql_SelectEmployees(connection);
            }
         }
         catch (SqlException e)
         {
            Console.WriteLine(e.ToString());
         }
         Console.WriteLine("View the report output here, then press any key to end the program...");
         Console.ReadKey();
      }
```


<a name="cs_2_createtables"/>
<a id="c-block-2-t-sql-to-create-tables" class="xliff"></a>

### C#-Block 2: T-SQL-Code zum Erstellen von Tabellen

- [Vorheriger](#cs_1_connect) &nbsp; / &nbsp; [Nächster](#cs_3_insert)

```csharp
      static string Build_2_Tsql_CreateTables()
      {
         return @"
DROP TABLE IF EXISTS tabEmployee;
DROP TABLE IF EXISTS tabDepartment;  -- Drop parent table last.


CREATE TABLE tabDepartment
(
   DepartmentCode  nchar(4)          not null
      PRIMARY KEY,
   DepartmentName  nvarchar(128)     not null
);

CREATE TABLE tabEmployee
(
   EmployeeGuid    uniqueIdentifier  not null  default NewId()
      PRIMARY KEY,
   EmployeeName    nvarchar(128)     not null,
   EmployeeLevel   int               not null,
   DepartmentCode  nchar(4)              null
      REFERENCES tabDepartment (DepartmentCode)  -- (REFERENCES would be disallowed on temporary tables.)
);
";
      }
```

<a id="entity-relationship-diagram-erd" class="xliff"></a>

#### Entitätsbeziehungsdiagramm

Die vorherigen CREATE TABLE-Anweisungen verfügen über das Schlüsselwort **REFERENCES**, um eine *Fremdschlüssel*-Beziehung (Foreign Key, FK) zwischen zwei Tabellen zu erstellen.  Bei Verwendung von tempdb kommentieren Sie das Schlüsselwort `--REFERENCES` aus, indem Sie zwei vorangestellte Bindestriche verwenden.

Als Nächstes ist hier in einem Entitätsbeziehungsdiagramm die Beziehung zwischen den beiden Tabellen dargestellt. Die Werte in der *untergeordneten* Spalte „#tabEmployee.DepartmentCode“ sind auf die Werte beschränkt, die in der *übergeordneten* Spalte „#tabDepartment.Department“ angegeben sind.

![Entitätsbeziehungsdiagramm mit Fremdschlüssel](./media/sql-database-csharp-adonet-create-query-2/erd-dept-empl-fky-2.png)


<a name="cs_3_insert"/>
<a id="c-block-3-t-sql-to-insert-data" class="xliff"></a>

### C#-Block 3: T-SQL-Code zum Einfügen von Daten

- [Vorheriger](#cs_2_createtables) &nbsp; / &nbsp; [Nächster](#cs_4_updatejoin)


```csharp
      static string Build_3_Tsql_Inserts()
      {
         return @"
-- The company has these departments.
INSERT INTO tabDepartment
   (DepartmentCode, DepartmentName)
      VALUES
   ('acct', 'Accounting'),
   ('hres', 'Human Resources'),
   ('legl', 'Legal');

-- The company has these employees, each in one department.
INSERT INTO tabEmployee
   (EmployeeName, EmployeeLevel, DepartmentCode)
      VALUES
   ('Alison'  , 19, 'acct'),
   ('Barbara' , 17, 'hres'),
   ('Carol'   , 21, 'acct'),
   ('Deborah' , 24, 'legl'),
   ('Elle'    , 15, null);
";
      }
```


<a name="cs_4_updatejoin"/>
<a id="c-block-4-t-sql-to-update-join" class="xliff"></a>

### C#-Block 4: T-SQL-Code für Aktualisierung/Verknüpfung

- [Vorheriger](#cs_3_insert) &nbsp; / &nbsp; [Nächster](#cs_5_deletejoin)


```csharp
      static string Build_4_Tsql_UpdateJoin()
      {
         return @"
DECLARE @DName1  nvarchar(128) = @csharpParmDepartmentName;  --'Accounting';


-- Promote everyone in one department (see @parm...).
UPDATE empl
   SET
      empl.EmployeeLevel += 1
   FROM
      tabEmployee   as empl
      INNER JOIN
      tabDepartment as dept ON dept.DepartmentCode = empl.DepartmentCode
   WHERE
      dept.DepartmentName = @DName1;
";
      }
```


<a name="cs_5_deletejoin"/>
<a id="c-block-5-t-sql-to-delete-join" class="xliff"></a>

### C#-Block 5: T-SQL-Code für Löschung/Verknüpfung

- [Vorheriger](#cs_4_updatejoin) &nbsp; / &nbsp; [Nächster](#cs_6_selectrows)


```csharp
      static string Build_5_Tsql_DeleteJoin()
      {
         return @"
DECLARE @DName2  nvarchar(128);
SET @DName2 = @csharpParmDepartmentName;  --'Legal';


-- Right size the Legal department.
DELETE empl
   FROM
      tabEmployee   as empl
      INNER JOIN
      tabDepartment as dept ON dept.DepartmentCode = empl.DepartmentCode
   WHERE
      dept.DepartmentName = @DName2

-- Disband the Legal department.
DELETE tabDepartment
   WHERE DepartmentName = @DName2;
";
      }
```



<a name="cs_6_selectrows"/>
<a id="c-block-6-t-sql-to-select-rows" class="xliff"></a>

### C#-Block 6: T-SQL-Code zum Auswählen von Zeilen

- [Vorheriger](#cs_5_deletejoin) &nbsp; / &nbsp; [Nächster](#cs_6b_datareader)


```csharp
      static string Build_6_Tsql_SelectEmployees()
      {
         return @"
-- Look at all the final Employees.
SELECT
      empl.EmployeeGuid,
      empl.EmployeeName,
      empl.EmployeeLevel,
      empl.DepartmentCode,
      dept.DepartmentName
   FROM
      tabEmployee   as empl
      LEFT OUTER JOIN
      tabDepartment as dept ON dept.DepartmentCode = empl.DepartmentCode
   ORDER BY
      EmployeeName;
";
      }
```


<a name="cs_6b_datareader"/>
<a id="c-block-6b-executereader" class="xliff"></a>

### C#-Block 6b: ExecuteReader

- [Vorheriger](#cs_6_selectrows) &nbsp; / &nbsp; [Nächster](#cs_7_executenonquery)

Diese Methode ist für das Ausführen der T-SQL-SELECT-Anweisung ausgelegt, die mit der **Build_6_Tsql_SelectEmployees**-Methode erstellt wird.


```csharp
      static void Submit_6_Tsql_SelectEmployees(SqlConnection connection)
      {
         Console.WriteLine();
         Console.WriteLine("=================================");
         Console.WriteLine("Now, SelectEmployees (6)...");

         string tsql = Build_6_Tsql_SelectEmployees();

         using (var command = new SqlCommand(tsql, connection))
         {
            using (SqlDataReader reader = command.ExecuteReader())
            {
               while (reader.Read())
               {
                  Console.WriteLine("{0} , {1} , {2} , {3} , {4}",
                     reader.GetGuid(0),
                     reader.GetString(1),
                     reader.GetInt32(2),
                     (reader.IsDBNull(3)) ? "NULL" : reader.GetString(3),
                     (reader.IsDBNull(4)) ? "NULL" : reader.GetString(4));
               }
            }
         }
      }
```


<a name="cs_7_executenonquery"/>
<a id="c-block-7-executenonquery" class="xliff"></a>

### C#-Block 7: ExecuteNonQuery

- [Vorheriger](#cs_6b_datareader) &nbsp; / &nbsp; [Nächster](#cs_8_output)

Diese Methode wird für Vorgänge aufgerufen, bei denen der Dateninhalt von Tabellen geändert wird, ohne Datenzeilen zurückzugeben.


```csharp
      static void Submit_Tsql_NonQuery(
         SqlConnection connection,
         string tsqlPurpose,
         string tsqlSourceCode,
         string parameterName = null,
         string parameterValue = null
         )
      {
         Console.WriteLine();
         Console.WriteLine("=================================");
         Console.WriteLine("T-SQL to {0}...", tsqlPurpose);

         using (var command = new SqlCommand(tsqlSourceCode, connection))
         {
            if (parameterName != null)
            {
               command.Parameters.AddWithValue(  // Or, use SqlParameter class.
                  parameterName,
                  parameterValue);
            }
            int rowsAffected = command.ExecuteNonQuery();
            Console.WriteLine(rowsAffected + " = rows affected.");
         }
      }
   } // EndOfClass
}
```


<a name="cs_8_output"/>
<a id="c-block-8-actual-test-output-to-the-console" class="xliff"></a>

### C#-Block 8: Tatsächliche Testausgabe auf der Konsole

- [Vorheriger](#cs_7_executenonquery)

In diesem Abschnitt wird die Ausgabe erfasst, die vom Programm an die Konsole gesendet wurde. Nur die GUID-Werte variieren zwischen Testläufen.


```text
[C:\csharp_db_test\csharp_db_test\bin\Debug\]
>> csharp_db_test.exe

=================================
Now, CreateTables (10)...

=================================
Now, Inserts (20)...

=================================
Now, UpdateJoin (30)...
2 rows affected, by UpdateJoin.

=================================
Now, DeleteJoin (40)...

=================================
Now, SelectEmployees (50)...
0199be49-a2ed-4e35-94b7-e936acf1cd75 , Alison , 20 , acct , Accounting
f0d3d147-64cf-4420-b9f9-76e6e0a32567 , Barbara , 17 , hres , Human Resources
cf4caede-e237-42d2-b61d-72114c7e3afa , Carol , 22 , acct , Accounting
cdde7727-bcfd-4f72-a665-87199c415f8b , Elle , 15 , NULL , NULL

[C:\csharp_db_test\csharp_db_test\bin\Debug\]
>>
```
