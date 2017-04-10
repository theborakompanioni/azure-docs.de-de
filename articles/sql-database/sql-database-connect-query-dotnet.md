---
title: Herstellen von Verbindungen mit Azure SQL-Datenbank mithilfe von .NET (C#) | Microsoft-Dokumentation
description: "Verwenden Sie den Beispielcode in diesem Schnelleinstieg zum Erstellen einer modernen Anwendung in C#, die mit Azure SQL-Datenbank durch eine leistungsfähige relationale Datenbank in der Cloud unterstützt wird."
services: sql-database
documentationcenter: 
author: ajlam
manager: jhubbard
editor: 
ms.assetid: 7faca033-24b4-4f64-9301-b4de41e73dfd
ms.service: sql-database
ms.custom: quick start
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 03/28/2017
ms.author: andrela;sstein;carlrab
translationtype: Human Translation
ms.sourcegitcommit: 432752c895fca3721e78fb6eb17b5a3e5c4ca495
ms.openlocfilehash: c6c0c218b8d0456d37a4514238675fd8e75faf9d
ms.lasthandoff: 03/30/2017


---
# <a name="azure-sql-database-use-net-c-to-connect-and-query-data"></a>Azure SQL-Datenbank: Verwenden von .NET (C#) zum Herstellen einer Verbindung und Abfragen von Daten

Verwenden Sie [C# und ADO.NET](https://msdn.microsoft.com/library/kb9s9ks0.aspx), um eine Verbindung mit einer Azure SQL-Datenbank herzustellen und Abfragen dafür durchzuführen. In diesem Leitfaden wird beschrieben, wie Sie C# zum Herstellen einer Verbindung mit einer Azure SQL-Datenbank verwenden und dann Anweisungen zum Abfragen, Einfügen, Aktualisieren und Löschen ausführen.

In diesem Schnellstart werden als Ausgangspunkt die Ressourcen verwendet, die in einem der folgenden Schnellstarts erstellt wurden:

- [Erstellen einer Datenbank – Portal](sql-database-get-started-portal.md)
- [Erstellen einer Datenbank – CLI](sql-database-get-started-cli.md)

## <a name="configure-development-environment"></a>Konfigurieren der Entwicklungsumgebung

In den folgenden Abschnitten erfahren Sie, wie Sie Ihre vorhandenen Entwicklungsumgebungen für Mac OS, Linux (Ubuntu) und Windows für die Arbeit mit Azure SQL-Datenbank konfigurieren.

### <a name="mac-os"></a>**Mac OS**
Öffnen Sie das Terminal, und navigieren Sie zu einem Verzeichnis, in dem Sie Ihr .NET Core-Projekt erstellen möchten. Geben Sie die folgenden Befehle ein, um **brew**, **OpenSSL** und **.NET Core** zu installieren. 

```C#
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

```C#
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ xenial main" > /etc/apt/sources.list.d/dotnetdev.list'
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
sudo apt-get update
sudo apt-get install dotnet-dev-1.0.1
```

### <a name="windows"></a>**Windows**
Installieren Sie Visual Studio 2015 Community Edition und .NET Framework. Falls Visual Studio bereits auf Ihrem Computer installiert ist, überspringen Sie die nächsten Schritte.

Visual Studio 2015 Community ist eine vollwertige, kostenlose und erweiterbare IDE zum Erstellen von modernen Anwendungen für Android, iOS und Windows sowie zum Erstellen von Web-/Datenbankanwendungen und Clouddiensten.

1. Laden Sie das [Installationsprogramm](https://go.microsoft.com/fwlink/?LinkId=691978) herunter. 
2. Führen Sie das Installationsprogramm aus, und befolgen Sie die Installationsanweisungen, um die Installation abzuschließen.

## <a name="get-connection-information"></a>Abrufen von Verbindungsinformationen

Rufen Sie die Verbindungszeichenfolge im Azure-Portal ab. Sie verwenden die Verbindungszeichenfolge zum Herstellen einer Verbindung mit der Azure SQL-Datenbank.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Wählen Sie im Menü auf der linken Seite die Option **SQL-Datenbanken**, und klicken Sie auf der Seite **SQL-Datenbanken** auf Ihre Datenbank. 
3. Überprüfen Sie im Bereich **Zusammenfassung** für Ihre Datenbank den vollqualifizierten Servernamen. 

    <img src="./media/sql-database-connect-query-dotnet/connection-strings.png" alt="connection strings" style="width: 780px;" />

4. Klicken Sie auf **Datenbankverbindungszeichenfolgen anzeigen**.

5. Überprüfen Sie die vollständige **ADO.NET**-Verbindungszeichenfolge.

    <img src="./media/sql-database-connect-query-dotnet/adonet-connection-string.png" alt="ADO.NET connection string" style="width: 780px;" />
    
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

3. Verwenden Sie [SqlCommand.ExecuteReader](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.executereader.aspx) mit einer [SELECT](https://msdn.microsoft.com/library/ms189499.aspx)-Transact-SQL-Anweisung, um Daten in Ihrer Azure SQL-Datenbank abzufragen. Hinzufügen der richtigen Werte für Ihren Server

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
- Informationen zum Abfragen und Bearbeiten von Daten mit Visual Studio Code finden Sie unter [Visual Studio Code](https://code.visualstudio.com/docs).

