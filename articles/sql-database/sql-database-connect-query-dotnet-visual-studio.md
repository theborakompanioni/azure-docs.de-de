---
title: Abfragen von Azure SQL-Datenbank mit Visual Studio und .NET | Microsoft-Dokumentation
description: In diesem Thema erfahren Sie, wie Sie unter Verwendung von Visual Studio ein Programm erstellen, das eine Verbindung mit einer Azure SQL-Datenbank herstellt und sie mithilfe von Transact-SQL-Anweisungen abfragt.
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 07/05/2017
ms.author: carlrab
ms.translationtype: HT
ms.sourcegitcommit: 2ad539c85e01bc132a8171490a27fd807c8823a4
ms.openlocfilehash: c7d54f8355803933e0d581470804ee98a0172937
ms.contentlocale: de-de
ms.lasthandoff: 07/12/2017

---
# <a name="use-net-c-with-visual-studio-to-connect-and-query-an-azure-sql-database"></a>Herstellen einer Verbindung mit einer Azure SQL-Datenbank und Abfragen der Datenbank mit .NET (C#) und Visual Studio

In diesem Schnellstarttutorial erfahren Sie, wie Sie unter Verwendung von [.NET Framework](https://www.microsoft.com/net/) ein C#-Programm mit Visual Studio erstellen, um eine Verbindung mit einer Azure SQL-Datenbank herzustellen und mithilfe von Transact-SQL-Anweisungen Daten abzufragen.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Schnellstarttutorial benötigen Sie Folgendes:

- Eine Azure SQL-Datenbank. In dieser Schnellstartanleitung werden die Ressourcen aus einer der folgenden Schnellstartanleitungen verwendet: 

   - [Erstellen einer Datenbank – Portal](sql-database-get-started-portal.md)
   - [Erstellen einer Datenbank – CLI](sql-database-get-started-cli.md)
   - [Erstellen einer Datenbank – PowerShell](sql-database-get-started-powershell.md)

- Eine [Firewallregel auf Serverebene](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) für die öffentliche IP-Adresse des Computers, den Sie für dieses Schnellstarttutorial verwenden.
- Eine Installation von [Visual Studio Community 2017, Visual Studio Professional 2017 oder Visual Studio Enterprise 2017](https://www.visualstudio.com/downloads/).

## <a name="sql-server-connection-information"></a>SQL Server-Verbindungsinformationen

Rufen Sie die Verbindungsinformationen ab, die zum Herstellen einer Verbindung mit der Azure SQL-Datenbank erforderlich sind. In den weiteren Verfahren benötigen Sie den vollqualifizierten Servernamen, den Datenbanknamen und die Anmeldeinformationen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Wählen Sie im Menü auf der linken Seite die Option **SQL-Datenbanken**, und klicken Sie auf der Seite **SQL-Datenbanken** auf Ihre Datenbank. 
3. Überprüfen Sie auf der Seite **Übersicht** für Ihre Datenbank den vollqualifizierten Servernamen wie in der folgenden Abbildung dargestellt. Sie können auf den Servernamen zeigen, um die Option **Klicken Sie zum Kopieren** anzuzeigen. 

   ![Servername](./media/sql-database-connect-query-dotnet/server-name.png) 

4. Falls Sie die Anmeldeinformationen für Ihren Azure SQL-Datenbankserver vergessen, können Sie zur Seite des SQL-Datenbankservers navigieren, um den Serveradministratornamen anzuzeigen. Das Kennwort kann bei Bedarf zurückgesetzt werden.

5. Klicken Sie auf **Datenbankverbindungszeichenfolgen anzeigen**.

6. Überprüfen Sie die vollständige **ADO.NET**-Verbindungszeichenfolge.

    ![ADO.NET-Verbindungszeichenfolge](./media/sql-database-connect-query-dotnet/adonet-connection-string.png)

> [!IMPORTANT]
> Sie benötigen eine Firewallregel für die öffentliche IP-Adresse des Computers, auf dem Sie dieses Tutorial ausführen. Wenn Sie an einem anderen Computer arbeiten oder eine andere öffentliche IP-Adresse verwenden, [erstellen Sie über das Azure-Portal eine Firewallregel auf Serverebene](sql-database-get-started-portal.md#create-a-server-level-firewall-rule). 
>
  
## <a name="create-a-new-visual-studio-project"></a>Erstellen eines neuen Visual Studio-Projekts

1. Wählen Sie in Visual Studio **Datei** > **Neu** > **Projekt** aus. 
2. Erweitern Sie im Dialogfeld **Neues Projekt** die Option **Visual C#**.
3. Wählen Sie **Konsolen-App** aus, und geben Sie als Projektname *sqltest* ein.
4. Klicken Sie auf **OK**, um das neue Projekt zu erstellen und in Visual Studio zu öffnen.
4. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf **sqltest**, und klicken Sie anschließend auf **NuGet-Pakete verwalten**. 
5. Suchen Sie im Bereich **Durchsuchen** nach ```System.Data.SqlClient```, und wählen Sie es aus.
6. Klicken Sie auf der Seite **System.Data.SqlClient** auf **Installieren**.
7. Überprüfen Sie nach Abschluss der Installation die Änderungen, und klicken Sie anschließend auf **OK**, um das Vorschaufenster**** zu schließen. 
8. Sollte das Fenster **Zustimmung zur Lizenz** angezeigt werden, klicken Sie auf **Ich stimme zu**.

## <a name="insert-code-to-query-sql-database"></a>Einfügen von Code zum Abfragen der SQL-Datenbank

1. Wechseln Sie zu **Program.cs**. (Muss ggf. erst noch geöffnet werden.)

2. Ersetzen Sie den Inhalt von **Program.cs** durch den folgenden Code, und fügen Sie die entsprechenden Werte für Server, Datenbank, Benutzer und Kennwort hinzu.

```csharp
using System;
using System.Data.SqlClient;
using System.Text;

namespace sqltest
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
            Console.ReadLine();
        }
    }
}
```

## <a name="run-the-code"></a>Ausführen des Codes

1. Drücken Sie **F5**, um die Anwendung auszuführen.
2. Vergewissern Sie sich, dass die ersten 20 Zeilen zurückgegeben werden, und schließen Sie dann das Anwendungsfenster.

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich darüber, wie Sie unter Linux/Windows/macOS [unter Verwendung von .NET Core eine Verbindung mit einer Azure SQL-Datenbank herstellen und die Datenbank abfragen](sql-database-connect-query-dotnet-core.md).  
- Informieren Sie sich über [die ersten Schritte mit .NET Core unter Windows/Linux/macOS bei Verwendung der Befehlszeile](/dotnet/core/tutorials/using-with-xplat-cli.md).
- Erfahren Sie, wie Sie [Ihre erste Azure SQL-Datenbank mithilfe von SSMS entwerfen](sql-database-design-first-database.md) oder wie Sie [Ihre erste Azure SQL-Datenbank mithilfe von .NET entwerfen](sql-database-design-first-database-csharp.md).
- Weitere Informationen zu .NET finden Sie in der [.NET-Dokumentation](https://docs.microsoft.com/dotnet/).

