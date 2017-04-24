---
title: Herstellen von Verbindungen mit Azure SQL-Datenbank mithilfe von .NET (C#) | Microsoft-Dokumentation
description: "Zeigt ein .NET-Codebeispiel an, das Sie zum Herstellen einer Verbindung mit Azure SQL-Datenbank und Senden von entsprechenden Abfragen verwenden können."
services: sql-database
documentationcenter: 
author: ajlam
manager: jhubbard
editor: 
ms.assetid: 7faca033-24b4-4f64-9301-b4de41e73dfd
ms.service: sql-database
ms.custom: quick start connect
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 04/17/2017
ms.author: andrela;sstein;carlrab
translationtype: Human Translation
ms.sourcegitcommit: db7cb109a0131beee9beae4958232e1ec5a1d730
ms.openlocfilehash: 368ffc65382c75b0fe5f4c20ce1c6a487a764ed3
ms.lasthandoff: 04/18/2017


---
# <a name="azure-sql-database-use-net-c-to-connect-and-query-data"></a>Azure SQL-Datenbank: Verwenden von .NET (C#) zum Herstellen einer Verbindung und Abfragen von Daten

In diesem Schnellstart wird veranschaulicht, wie Sie [C# und ADO.NET](https://msdn.microsoft.com/library/kb9s9ks0.aspx) nutzen, um eine Verbindung mit einer Azure SQL-Datenbank herzustellen. Anschließend können Sie Transact-SQL-Anweisungen zum Abfragen, Einfügen, Aktualisieren und Löschen von Daten in der Datenbank über die Plattformen Windows, Mac OS und Ubuntu Linux verwenden.

In diesem Schnellstart werden als Ausgangspunkt die Ressourcen verwendet, die in einem der folgenden Schnellstarts erstellt wurden:

- [Erstellen einer Datenbank – Portal](sql-database-get-started-portal.md)
- [Erstellen einer Datenbank – CLI](sql-database-get-started-cli.md)

## <a name="install-net"></a>Installieren von .NET

### <a name="windows-net-framework-and-net-core"></a>**Windows .NET Framework und .NET Core**

Visual Studio 2017 Community ist eine vollwertige, kostenlose und erweiterbare IDE zum Erstellen von modernen Anwendungen für Android, iOS und Windows sowie zum Erstellen von Web- und Datenbankanwendungen und Clouddiensten. Sie können entweder das vollständige .NET Framework oder nur .NET Core installieren. Die Codeausschnitte im Schnellstart funktionieren für beide Szenarien. Falls Visual Studio bereits auf Ihrem Computer installiert ist, überspringen Sie die nächsten Schritte.

1. Laden Sie das [Installationsprogramm](https://go.microsoft.com/fwlink/?LinkId=691978) herunter. 
2. Führen Sie das Installationsprogramm aus, und befolgen Sie die Installationsanweisungen, um die Installation abzuschließen.

### <a name="mac-os"></a>**Mac OS**
Öffnen Sie das Terminal, und navigieren Sie zu einem Verzeichnis, in dem Sie Ihr .NET Core-Projekt erstellen möchten. Geben Sie die folgenden Befehle ein, um **brew**, **OpenSSL** und **.NET Core** zu installieren. 

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew update
brew install openssl
mkdir -p /usr/local/lib
ln -s /usr/local/opt/openssl/lib/libcrypto.1.0.0.dylib /usr/local/lib/
ln -s /usr/local/opt/openssl/lib/libssl.1.0.0.dylib /usr/local/lib/
```

Installieren Sie .NET Core unter Mac OS. Laden Sie das [offizielle Installationsprogramm](https://go.microsoft.com/fwlink/?linkid=843444) herunter. Dieses Installationsprogramm installiert die Tools und platziert Sie an Ihrem Pfad, sodass Sie „dotnet“ über die Konsole ausführen können.

### <a name="linux-ubuntu"></a>**Linux (Ubuntu)**
Öffnen Sie das Terminal, und navigieren Sie zu einem Verzeichnis, in dem Sie Ihr .NET Core-Projekt erstellen möchten. Geben Sie die folgenden Befehle ein, um **.NET Core** zu installieren.

```bash
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ xenial main" > /etc/apt/sources.list.d/dotnetdev.list'
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
sudo apt-get update
sudo apt-get install dotnet-dev-1.0.1
```

## <a name="get-connection-information"></a>Abrufen von Verbindungsinformationen

Rufen Sie die Verbindungszeichenfolge im Azure-Portal ab. Sie verwenden die Verbindungszeichenfolge zum Herstellen einer Verbindung mit der Azure SQL-Datenbank.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Wählen Sie im Menü auf der linken Seite die Option **SQL-Datenbanken**, und klicken Sie auf der Seite **SQL-Datenbanken** auf Ihre Datenbank. 
3. Überprüfen Sie auf der Seite **Übersicht** für Ihre Datenbank den vollqualifizierten Servernamen wie in der Abbildung unten dargestellt. Sie können auf den Servernamen zeigen, um die Option **Klicken Sie zum Kopieren** anzuzeigen. 

   ![Servername](./media/sql-database-connect-query-dotnet/server-name.png) 

4. Falls Sie die Anmeldeinformationen für Ihren Azure SQL-Datenbankserver vergessen haben, können Sie zur Seite des SQL-Datenbankservers navigieren, um den Serveradministrator-Benutzernamen anzuzeigen und ggf. das Kennwort zurückzusetzen.

5. Klicken Sie auf **Datenbankverbindungszeichenfolgen anzeigen**.

6. Überprüfen Sie die vollständige **ADO.NET**-Verbindungszeichenfolge.

    ![ADO.NET-Verbindungszeichenfolge](./media/sql-database-connect-query-dotnet/adonet-connection-string.png)
  
## <a name="add-systemdatasqlclient"></a>Hinzufügen von „System.Data.SqlClient“
Wenn Sie .NET Core verwenden, fügen Sie „System.Data.SqlClient“ der ***CSPROJ-Datei*** Ihres Projekts als Abhängigkeit hinzu.

```xml
<ItemGroup>
    <PackageReference Include="System.Data.SqlClient" Version="4.3.0" />
</ItemGroup>
```

## <a name="select-data"></a>Auswählen von Daten

1. Öffnen Sie in Ihrer Entwicklungsumgebung eine leere Codedatei.
2. Fügen Sie ```using System.Data.SqlClient``` Ihrer Codedatei hinzu ([System.Data.SqlClient-Namespace](https://msdn.microsoft.com/library/system.data.sqlclient.aspx)). 

3. Verwenden Sie [SqlCommand.ExecuteReader](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.executereader.aspx) mit einer [SELECT](https://msdn.microsoft.com/library/ms189499.aspx)-Transact-SQL-Anweisung, um Daten in Ihrer Azure SQL-Datenbank abzufragen. Fügen Sie die richtigen Werte für Ihren Server hinzu.
```csharp
using System;
using System.Data;
using System.Data.SqlClient;

namespace ConsoleApplication1
{
    class Program
    {
        static void Main(string[] args)
        {
            try 
            { 
                SqlConnectionStringBuilder builder = new SqlConnectionStringBuilder();
                builder.DataSource = "your_server.database.windows.net"; 
                builder.UserID = "your_user";            
                builder.Password = "your_password";     
                builder.InitialCatalog = "your_database";

                using (SqlConnection connection = new SqlConnection(builder.ConnectionString))
                {
                    Console.WriteLine("\nQuery data example:");
                    Console.WriteLine("=========================================\n");
                    
                    connection.Open();       
                    StringBuilder sb = new StringBuilder();
                    sb.Append("SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName ");
                    sb.Append("FROM [SalesLT].[ProductCategory] pc ");
                    sb.Append("JOIN [SalesLT].[Product] p ");
                    sb.Append("ON pc.productcategoryid = p.productcategoryid;");
                    String sql = sb.ToString();

                    using (SqlCommand command = new SqlCommand(sql, connection))
                    {
                        using (SqlDataReader reader = command.ExecuteReader())
                        {
                            while (reader.Read())
                            {
                                Console.WriteLine("{0} {1}", reader.GetString(0), reader.GetString(1));
                            }
                        }
                    }                    
                }
            }
            catch (SqlException e)
            {
                Console.WriteLine(e.ToString());
            }
        }
    }
}
```

## <a name="insert-data"></a>Einfügen von Daten

Verwenden Sie [SqlCommand.ExecuteNonQuery](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.executenonquery.aspx) mit einer [INSERT](https://msdn.microsoft.com/library/ms174335.aspx)-Transact-SQL-Anweisung, um Daten in Ihre Azure SQL-Datenbank einzufügen.

```csharp
using System;
using System.Data;
using System.Data.SqlClient;

namespace ConsoleApplication1
{
    class Program
    {
        static void Main(string[] args)
        {
            try 
            { 
                SqlConnectionStringBuilder builder = new SqlConnectionStringBuilder();
                builder.DataSource = "your_server.database.windows.net"; 
                builder.UserID = "your_user";            
                builder.Password = "your_password";     
                builder.InitialCatalog = "your_database";

                using (SqlConnection connection = new SqlConnection(builder.ConnectionString))
                {
                    Console.WriteLine("\nInsert data example:");
                    Console.WriteLine("=========================================\n");

                    connection.Open();       
                    StringBuilder sb = new StringBuilder();
                    sb.Append("INSERT INTO [SalesLT].[Product] ([Name], [ProductNumber], [Color], [StandardCost], [ListPrice], [SellStartDate]) ");
                    sb.Append("VALUES (@Name, @ProductNumber, @Color, @StandardCost, @ListPrice, @SellStartDate);");
                    String sql = sb.ToString();
                    using (SqlCommand command = new SqlCommand(sql, connection))
                    {
                        command.Parameters.AddWithValue("@Name", "BrandNewProduct");
                        command.Parameters.AddWithValue("@ProductNumber", "200989");
                        command.Parameters.AddWithValue("@Color", "Blue");
                        command.Parameters.AddWithValue("@StandardCost", 75);
                        command.Parameters.AddWithValue("@ListPrice", 80);
                        command.Parameters.AddWithValue("@SellStartDate", "7/1/2016");
                        int rowsAffected = command.ExecuteNonQuery();
                        Console.WriteLine(rowsAffected + " row(s) inserted");
                    }         
                }
            }
            catch (SqlException e)
            {
                Console.WriteLine(e.ToString());
            }
        }
    }
}
```

## <a name="update-data"></a>Aktualisieren von Daten

Verwenden Sie [SqlCommand.ExecuteNonQuery](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.executenonquery.aspx) mit einer [UPDATE](https://msdn.microsoft.com/library/ms177523.aspx)-Transact-SQL-Anweisung, um die Daten in Ihrer Azure SQL-Datenbank zu aktualisieren.

```csharp
using System;
using System.Data;
using System.Data.SqlClient;

namespace ConsoleApplication1
{
    class Program
    {
        static void Main(string[] args)
        {
            try 
            { 
                SqlConnectionStringBuilder builder = new SqlConnectionStringBuilder();
                builder.DataSource = "your_server.database.windows.net"; 
                builder.UserID = "your_user";            
                builder.Password = "your_password";     
                builder.InitialCatalog = "your_database";

                using (SqlConnection connection = new SqlConnection(builder.ConnectionString))
                {
                    Console.WriteLine("\nUpdate data example");
                    Console.WriteLine("=========================================\n");

                    connection.Open();       
                    StringBuilder sb = new StringBuilder();
                    sb.Append("UPDATE [SalesLT].[Product] SET ListPrice = @ListPrice WHERE Name = @Name;");
                    String sql = sb.ToString();
                    using (SqlCommand command = new SqlCommand(sql, connection))
                    {
                        command.Parameters.AddWithValue("@Name", "BrandNewProduct");
                        command.Parameters.AddWithValue("@ListPrice", 500);
                        int rowsAffected = command.ExecuteNonQuery();
                        Console.WriteLine(rowsAffected + " row(s) updated");
                    }         
                }
            }
            catch (SqlException e)
            {
                Console.WriteLine(e.ToString());
            }
        }
    }
}
```

## <a name="delete-data"></a>Löschen von Daten

Verwenden Sie [SqlCommand.ExecuteNonQuery](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.executenonquery.aspx) mit einer [DELETE](https://msdn.microsoft.com/library/ms189835.aspx)-Transact-SQL-Anweisung, um die Daten in Ihrer Azure SQL-Datenbank zu löschen.

```csharp
using System;
using System.Data;
using System.Data.SqlClient;

namespace ConsoleApplication1
{
    class Program
    {
        static void Main(string[] args)
        {
            try 
            { 
                SqlConnectionStringBuilder builder = new SqlConnectionStringBuilder();
                builder.DataSource = "your_server.database.windows.net"; 
                builder.UserID = "your_user";            
                builder.Password = "your_password";     
                builder.InitialCatalog = "your_database";

                using (SqlConnection connection = new SqlConnection(builder.ConnectionString))
                {
                    Console.WriteLine("\nDelete data example");
                    Console.WriteLine("=========================================\n");

                    connection.Open();       
                    StringBuilder sb = new StringBuilder();
                    sb.Append("DELETE FROM SalesLT.Product WHERE Name = @Name;");
                    String sql = sb.ToString();
                    using (SqlCommand command = new SqlCommand(sql, connection))
                    {
                        command.Parameters.AddWithValue("@Name", "BrandNewProduct");
                        int rowsAffected = command.ExecuteNonQuery();
                        Console.WriteLine(rowsAffected + " row(s) deleted");
                    }         
                }
            }
            catch (SqlException e)
            {
                Console.WriteLine(e.ToString());
            }
        }
    }
}
```

## <a name="next-steps"></a>Nächste Schritte

- Die .NET-Dokumentation finden Sie unter [.NET-Dokumentation](https://docs.microsoft.com/dotnet/).
- Informationen zum Herstellen einer Verbindung und Senden von Abfragen mit SQL Server Management Studio finden Sie unter [Verbinden und Abfragen mit SSMS](sql-database-connect-query-ssms.md).
- Informationen zum Herstellen einer Verbindung und Senden von Abfragen mit Visual Studio finden Sie unter [Verbinden und Abfragen mit Visual Studio Code](sql-database-connect-query-vscode.md).
- Informationen zum Herstellen einer Verbindung und Senden von Abfragen mit PHP finden Sie unter [Verbinden und Abfragen mit PHP](sql-database-connect-query-php.md).
- Informationen zum Herstellen einer Verbindung und Senden von Abfragen mit Node.js finden Sie unter [Verbinden und Abfragen mit Node.js](sql-database-connect-query-nodejs.md).
- Informationen zum Herstellen einer Verbindung und Senden von Abfragen mit Java finden Sie unter [Verbinden und Abfragen mit Java](sql-database-connect-query-java.md).
- Informationen zum Herstellen einer Verbindung und Senden von Abfragen mit Python finden Sie unter [Verbinden und Abfragen mit Python](sql-database-connect-query-python.md).
- Informationen zum Herstellen einer Verbindung und Senden von Abfragen mit Ruby finden Sie unter [Verbinden und Abfragen mit Ruby](sql-database-connect-query-ruby.md).

