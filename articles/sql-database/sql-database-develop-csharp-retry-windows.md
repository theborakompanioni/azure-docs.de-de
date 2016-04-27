<properties
	pageTitle="Wiederholungslogik in C# für SQL-Datenbank | Microsoft Azure"
	description="Das C#-Beispiel enthält die Wiederholungslogik für die zuverlässige Interaktion mit einer Azure SQL-Datenbank."
	services="sql-database"
	documentationCenter=""
	authors="annemill"
	manager="jhubbard"
	editor=""/>


<tags
	ms.service="sql-database"
	ms.workload="data-management"
	ms.tgt_pltfrm="na"
	ms.devlang="dotnet"
	ms.topic="article"
	ms.date="04/08/2016"
	ms.author="annemill"/>


# Codebeispiel: Wiederholungslogik in C# für das Herstellen einer Verbindung mit einer SQL-Datenbank



[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)]



Dieses Thema enthält ein C#-Codebeispiel, mit dem die benutzerdefinierte Wiederholungslogik veranschaulicht wird. Die Wiederholungslogik dient zum ordnungsgemäßen Verarbeiten von temporären Fehlern oder *vorübergehende Fehler*, die meist nicht mehr vorhanden sind, wenn das Programm mehrere Sekunden wartet und es dann erneut versucht.


Die ADO.NET-Klassen zum Herstellen einer Verbindung mit dem lokalen Microsoft SQL Server können auch eine Verbindung mit der Azure SQL-Datenbank herstellen. Allein können die ADO.NET-Klassen aber nicht für die Robustheit und Zuverlässigkeit sorgen, die für die Verwendung in der Produktion erforderlich ist. Ihr Clientprogramm kann auf vorübergehende Fehler treffen, die vom Programm selbst ordnungsgemäß und kontrolliert behoben werden sollten. Es sollte seine Ausführung daraufhin von alleine fortführen.


Beispiele für vorübergehende Fehler:


- Eine Verbindung über das Internet unterliegt kurzen Netzwerkausfällen, nach denen die Verbindung wiederhergestellt werden kann.
- Cloud Computing umfasst auch den Lastenausgleich, wobei Versuche, eine Verbindung herzustellen oder Abfragen durchzuführen, kurz blockiert werden können.


## A: Ermitteln von vorübergehenden Fehlern


Ihr Programm muss zwischen vorübergehenden Fehlern und beständigen Fehlern unterscheiden. Wenn im Programm der Name der Zieldatenbank falsch geschrieben ist, bleibt der Fehler der Art „Keine Datenbank gefunden“ bestehen und tritt immer auf, wenn Sie das Programm erneut ausführen.

Die Liste mit den Fehlernummern, die als vorübergehende Fehler kategorisiert werden, finden Sie unter:

- [Fehlermeldungen für Clientprogramme der SQL-Datenbank](sql-database-develop-error-messages.md#bkmk_connection_errors)
  - Siehe den oberen Abschnitt *Vorübergehende Fehler, Verbindungsabbruchfehler*.


Das C#-Codebeispiel weiter unten in diesem Thema listet die Anzahl der vorübergehenden Fehler in einem Feld namens **TransientErrorNumbers** auf.



## B. C#-Codebeispiel


Das C#-Codebeispiel in diesem Thema enthält eine benutzerdefinierte Erkennungs- und Wiederholungslogik zum Behandeln von vorübergehenden Fehlern. Bei dem Beispiel wird davon ausgegangen, dass .NET Framework 4.5.1 oder höher installiert ist.


Im Codebeispiel werden einige grundlegende Richtlinien oder Empfehlungen befolgt, die unabhängig davon gelten, welche Technologie Sie zum Interagieren mit Azure SQL-Datenbank verwenden. Allgemeine Empfehlungen finden Sie unter:


- [Verbindungen mit SQL-Datenbanken: Wichtige Empfehlungen](sql-database-connect-central-recommendations.md)


Das C#-Codebeispiel besteht aus einer Datei mit dem Namen "Program.cs". Den Code dazu finden Sie im nächsten Abschnitt.


### B.1 Erfassen und Kompilieren des Codebeispiels


Sie können das Beispiel mit den folgenden Schritten kompilieren:


1. Erstellen Sie in der [kostenlosen Visual Studio Community Edition](https://www.visualstudio.com/products/visual-studio-community-vs) ein neues Projekt mit der Vorlage für die C#-Konsolenanwendung.
  - "Datei" > "Neu" > Projekt" > "Installiert" > "Vorlagen" > "Visual C#" > "Windows" > "Klassischer Desktop" > "Konsolenanwendung"
  - Geben Sie dem Projekt den Namen **RetryAdo2**.
2. Öffnen Sie den Projektmappen-Explorer.
  - Beachten Sie den Namen des Projekts.
  - Beachten Sie den Namen der Datei "Program.cs".
3. Öffnen Sie die Datei "Program.cs".
4. Ersetzen Sie den gesamten Inhalt der Datei "Program.cs" durch den Code im folgenden Codeblock.
5. Klicken Sie auf das Menü "Build" > "Projektmappe erstellen".


#### Einzufügender C#-Quellcode


Fügen Sie diesen Code in die Datei **Program.cs** ein.


Anschließend müssen Sie die Zeichenfolgen für Servername, Kennwort und usw. bearbeiten. Sie finden diese Zeichenfolgen in der Methode **GetSqlConnectionStringBuilder**.



```
using System;   // C#
using G = System.Collections.Generic;
using S = System.Data.SqlClient;
using T = System.Threading;
   
namespace RetryAdo2
{
   public class Program
   {
      static public int Main(string[] args)
      {
         bool succeeded = false;
         int totalNumberOfTimesToTry = 4;
         int retryIntervalSeconds = 10;
   
         for (int tries = 1;
            tries <= totalNumberOfTimesToTry;
            tries++)
         {
            try
            {
               if (tries > 1)
               {
                  Console.WriteLine
                     ("Transient error encountered. Will begin attempt number {0} of {1} max...",
                     tries, totalNumberOfTimesToTry
                     );
                  T.Thread.Sleep(1000 * retryIntervalSeconds);
                  retryIntervalSeconds = Convert.ToInt32
                     (retryIntervalSeconds * 1.5);
               }
               AccessDatabase();
               succeeded = true;
               break;
            }
   
            catch (S.SqlException sqlExc)
            {
               if (TransientErrorNumbers.Contains
                  (sqlExc.Number) == true)
               {
                  Console.WriteLine("{0}: transient occurred.", sqlExc.Number);
                  continue;
               }
               else
               {
                  Console.WriteLine(sqlExc);
                  succeeded = false;
                  break;
               }
            }
   
            catch (TestSqlException sqlExc)
            {
               if (TransientErrorNumbers.Contains
                  (sqlExc.Number) == true)
               {
                  Console.WriteLine("{0}: transient occurred. (TESTING.)", sqlExc.Number);
                  continue;
               }
               else
               {
                  Console.WriteLine(sqlExc);
                  succeeded = false;
                  break;
               }
            }
   
            catch (Exception Exc)
            {
               Console.WriteLine(Exc);
               succeeded = false;
               break;
            }
         }
   
         if (succeeded == true)
         {
            return 0;
         }
         else
         {
            Console.WriteLine("ERROR: Unable to access the database!");
            return 1;
         }
      }
   
      /// <summary>
      /// Connects to the database, reads,
      /// prints results to the console.
      /// </summary>
      static public void AccessDatabase()
      {
         //throw new TestSqlException(4060); //(7654321);  // Uncomment for testing.
   
         using (var sqlConnection = new S.SqlConnection
                  (GetSqlConnectionString()))
         {
            using (var dbCommand = sqlConnection.CreateCommand())
            {
               dbCommand.CommandText = @"
SELECT TOP 3
      ob.name,
      CAST(ob.object_id as nvarchar(32)) as [object_id]
   FROM sys.objects as ob
   WHERE ob.type='IT'
   ORDER BY ob.name;";
   
               sqlConnection.Open();
               var dataReader = dbCommand.ExecuteReader();
   
               while (dataReader.Read())
               {
                  Console.WriteLine("{0}\t{1}",
                     dataReader.GetString(0),
                     dataReader.GetString(1));
               }
            }
         }
      }
   
      /// <summary>
      /// You must edit the four 'my' string values.
      /// </summary>
      /// <returns>An ADO.NET connection string.</returns>
      static private string GetSqlConnectionString()
      {
         // Prepare the connection string to Azure SQL Database.
         var sqlConnectionSB = new S.SqlConnectionStringBuilder();
   
         // Change these values to your values.
         sqlConnectionSB.DataSource = "tcp:myazuresqldbserver.database.windows.net,1433"; //["Server"]
         sqlConnectionSB.InitialCatalog = "MyDatabase"; //["Database"]
   
         sqlConnectionSB.UserID = "MyLogin";  // "@yourservername"  as suffix sometimes.
         sqlConnectionSB.Password = "MyPassword";
         sqlConnectionSB.IntegratedSecurity = false;
   
         // Adjust these values if you like. (ADO.NET 4.5.1 or later.)
         sqlConnectionSB.ConnectRetryCount = 3;
         sqlConnectionSB.ConnectRetryInterval = 10;  // Seconds.
   
         // Leave these values as they are.
         sqlConnectionSB.IntegratedSecurity = false;
         sqlConnectionSB.Encrypt = true;
         sqlConnectionSB.ConnectTimeout = 30;
   
         return sqlConnectionSB.ToString();
      }
   
      static public G.List<int> TransientErrorNumbers =
         new G.List<int> { 4060, 40197, 40501, 40613,
         49918, 49919, 49920, 11001 };
   }
   
   /// <summary>
   /// For testing retry logic, you can have method
   /// AccessDatabase start by throwing a new
   /// TestSqlException with a Number that does
   /// or does not match a transient error number
   /// present in TransientErrorNumbers.
   /// </summary>
   internal class TestSqlException : ApplicationException
   {
      internal TestSqlException(int testErrorNumber)
      { this.Number = testErrorNumber; }
   
      internal int Number
      { get; set; }
   }
}
```



## C. Ausführen des Programms


Die ausführbare Datei **RetryAdo2.exe** gibt keine Parameter ein. So führen Sie die EXE-Datei aus:

1. Öffnen Sie ein Konsolenfenster, wohin Sie die Binärdatei RetryAdo2.exe kompiliert haben.
2. Führen Sie RetryAdo2.exe ohne Eingabeparameter aus.



```
database_firewall_rules_table   245575913
filestream_tombstone_2073058421 2073058421
filetable_updates_2105058535    2105058535
```



## D. Möglichkeiten um Ihre Wiederholungslogik zu testen

Es gibt verschiedene Wege, wie Sie einen vorübergehenden Fehler simulieren können, um Ihre Wiederholungslogik zu testen.


### D.1 Auslösen einer Testausnahme

Das Codebeispiel beinhaltet:

- Eine kleine zweite Klasse namens **TestSqlException**, mit einer Eigenschaft namens **Number**.
- `//throw new TestSqlException(4060);`, wofür Sie die Auskommentierung aufheben können.

Falls Sie die Auskommentierung für die Auslösung aufheben und neukompilieren, gibt die nächste Ausführung von **RetryAdo2.exe** etwas aus, was dem Folgenden ähnelt.



```
[C:\_MainW\VS15\RetryAdo2\RetryAdo2\bin\Debug]
>> RetryAdo2.exe
4060: transient occurred. (TESTING.)
Transient error encountered. Will begin attempt number 2 of 4 max...
4060: transient occurred. (TESTING.)
Transient error encountered. Will begin attempt number 3 of 4 max...
4060: transient occurred. (TESTING.)
Transient error encountered. Will begin attempt number 4 of 4 max...
4060: transient occurred. (TESTING.)
ERROR: Unable to access the database!

[C:\_MainW\VS15\RetryAdo2\RetryAdo2\bin\Debug]
>>
```



#### Erneutes Testen mit einem beständigen Fehler


Um zu beweisen, dass der Code beständige Fehler richtig handhabt, führen Sie den vorherigen Test noch einmal aus. Verwenden Sie dieses mal keinen echten vorrübergehenden Fehler wie „4060“. Verwenden Sie stattdessen die sinnlose Zahl „7654321“. Das Programm sollte dies als einen beständigen Fehler behandeln und jeden Wiederholungsversuch umgehen.



### D.2 Trennen vom Netzwerk

1. Trennen Sie den Clientcomputer vom Netzwerk.
  - Bei einem Desktop-Computer stecken Sie das Netzwerkkabel aus.
  - Bei einem Laptop drücken Sie die Tastenkombination um den Netzwerkadapter auszuschalten.
2. Starten Sie RetryAdo2.exe und warten Sie, bis die Konsole den ersten vorübergehenden Fehler, wahrscheinlich „11001“, anzeigt.
3. Verbinden Sie sich wieder mit dem Netzwerk, während RetryAdo2.exe weiterhin ausgeführt wird.
4. Beobachten Sie in einem späteren erneuten Versuch, wie die Konsole den Erfolg berichtet.


### D.3 T Temporäres Falschschreiben des Servernamens

1. Fügen Sie **TransientErrorNumbers** temporär 40615 als weitere Fehlernummer hinzu und kompilieren Sie neu.
2. Setzen Sie einen Haltepunkt in der Zeile: `new S.SqlConnectionStringBuilder()`.
3. Verwenden Sie die Funktion *Bearbeiten und Fortfahren*, um einige Zeilen unterhalb den Servernamen absichtlich falsch zu schreiben.
  - Lassen Sie das Programm laufen und kehren Sie zu Ihrem Haltepunkt zurück.
  - Der Fehler „40615“ tritt auf.
4. Beheben Sie den Rechtschreibfehler.
5. Lassen Sie das Programm laufen und erfolgreich abschließen.
6. Entfernen Sie „40615“ und kompilieren Sie erneut.


## E. Verwandte Links

- [Clientcodebeispiele für die ersten Schritte mit SQL-Datenbank](sql-database-develop-quick-start-client-code-samples.md)
- [Ausprobieren von SQL-Datenbank: Verwenden von C# zum Erstellen einer SQL-Datenbank mithilfe der SQL-Datenbankbibliothek für .NET](sql-database-get-started-csharp.md)

<!---HONumber=AcomDC_0413_2016-->