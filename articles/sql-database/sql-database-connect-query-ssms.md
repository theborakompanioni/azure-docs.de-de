---
title: 'SSMS: Herstellen der Verbindung und Abfragen von Daten in Azure SQL-Datenbank | Microsoft-Dokumentation'
description: "Erfahren Sie, wie Sie über SQL Server Management Studio (SSMS) eine Verbindung mit einer SQL-Datenbank in Azure herstellen. Führen Sie anschließend T-SQL-Anweisungen (Transact-SQL) aus, um Daten abzufragen und zu bearbeiten."
metacanonical: 
keywords: Verbinden mit SQL-Datenbank, SQL Server Management Studio
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 7cd2a114-c13c-4ace-9088-97bd9d68de12
ms.service: sql-database
ms.custom: quick start manage
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 03/15/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 8c4e33a63f39d22c336efd9d77def098bd4fa0df
ms.openlocfilehash: 9ffad92e668b76c9a4e2941b20d075bf52132d16
ms.lasthandoff: 04/20/2017


---
# <a name="azure-sql-database-use-sql-server-management-studio-to-connect-and-query-data"></a>Azure SQL-Datenbank: Verwenden von SQL Server Management Studio zum Herstellen der Verbindung und Abfragen von Daten

[SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) (SSMS) ist ein Verwaltungstool zum Erstellen und Verwalten von SQL Server-Ressourcen über die Benutzeroberfläche oder in Skripts. In diesem Schnellstart wird veranschaulicht, wie Sie SSMS zum Herstellen einer Verbindung mit einer Azure SQL-Datenbank verwenden und anschließend Transact-SQL-Anweisungen nutzen, um Daten in der Datenbank abzufragen, einzufügen, zu aktualisieren und zu löschen. 

In diesem Schnellstart werden als Ausgangspunkt die Ressourcen verwendet, die in einem der folgenden Schnellstarts erstellt wurden:

- [Erstellen einer Datenbank – Portal](sql-database-get-started-portal.md)
- [Erstellen einer Datenbank – CLI](sql-database-get-started-cli.md)

Stellen Sie zunächst sicher, dass Sie die neueste Version von [SSMS](https://msdn.microsoft.com/library/mt238290.aspx) installiert haben. 

## <a name="get-connection-information"></a>Abrufen von Verbindungsinformationen

Rufen Sie die Verbindungsinformationen ab, die zum Herstellen einer Verbindung mit der Azure SQL-Datenbank erforderlich sind. In den weiteren Verfahren benötigen Sie den vollqualifizierten Servernamen, den Datenbanknamen und die Anmeldeinformationen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Wählen Sie im Menü auf der linken Seite die Option **SQL-Datenbanken**, und klicken Sie auf der Seite **SQL-Datenbanken** auf Ihre Datenbank. 
3. Überprüfen Sie auf der Seite **Übersicht** für Ihre Datenbank den vollqualifizierten Servernamen wie in der Abbildung unten dargestellt. Sie können auf den Servernamen zeigen, um die Option **Klicken Sie zum Kopieren** anzuzeigen.

   ![Verbindungsinformationen](./media/sql-database-connect-query-ssms/connection-information.png) 

4. Falls Sie die Anmeldeinformationen für Ihren Azure SQL-Datenbankserver vergessen haben, können Sie zur Seite des SQL-Datenbankservers navigieren, um den Serveradministrator-Benutzernamen anzuzeigen und ggf. das Kennwort zurückzusetzen. 

## <a name="connect-to-your-database-in-the-sql-database-logical-server"></a>Herstellen einer Verbindung mit Ihrer Datenbank im logischen SQL-Datenbankserver

Verwenden Sie SQL Server Management Studio, um eine Verbindung mit Ihrem Azure SQL-Datenbankserver einzurichten. 

> [!IMPORTANT]
> Ein logischer Azure SQL-Datenbankserver lauscht über Port 1433. Wenn Sie versuchen, durch eine Unternehmensfirewall eine Verbindung mit einem logischen Azure SQL-Datenbankserver herzustellen, muss dieser Port in der Unternehmensfirewall geöffnet sein, damit der Vorgang erfolgreich ist.
>

1. Öffnen Sie SQL Server Management Studio.

2. Geben Sie im Dialogfeld **Mit Server verbinden** die folgenden Informationen ein:
   - **Servertyp**: Geben Sie das Datenbankmodul ein.
   - **Servername**: Geben Sie den vollqualifizierten Servernamen ein, z.B. **mynewserver20170313.database.windows.net**.
   - **Authentifizierung**: Geben Sie die SQL Server-Authentifizierung an.
   - **Anmeldung**: Geben Sie Ihr Serveradministratorkonto ein.
   - **Kennwort**: Geben Sie das Kennwort für Ihr Serveradministratorkonto ein.

   ![Verbindung mit dem Server herstellen](./media/sql-database-connect-query-ssms/connect.png)  

3. Klicken Sie im Dialogfeld **Mit Server verbinden** auf **Optionen**. Geben Sie im Abschnitt **Verbindung mit Datenbank herstellen** den Text **mySampleDatabase** ein, um eine Verbindung mit dieser Datenbank herzustellen.

   ![Herstellen einer Verbindung mit der Datenbank auf dem Server](./media/sql-database-connect-query-ssms/options-connect-to-db.png)  

4. Klicken Sie auf **Verbinden**. Die Objekt-Explorer-Fenster wird in SSMS geöffnet. 

   ![Mit dem Server verbunden](./media/sql-database-connect-query-ssms/connected.png)  

5. Erweitern Sie im Objekt-Explorer die Option **Datenbanken** und anschließend die Option **mySampleDatabase**, um die Objekte in der Beispieldatenbank anzuzeigen.

## <a name="query-data"></a>Abfragen von Daten

Verwenden Sie den folgenden Code, um mit der [SELECT](https://msdn.microsoft.com/library/ms189499.aspx)-Transact-SQL-Anweisung die 20 wichtigsten Produkte nach Kategorie abzufragen.

1. Klicken Sie im Objekt-Explorer mit der rechten Maustaste auf **mySampleDatabase**, und wählen Sie **Neue Abfrage**. Ein leeres Abfragefenster mit einer Verbindung mit Ihrer Datenbank wird geöffnet.
2. Geben Sie im Abfragefenster die folgende Abfrage ein:

   ```sql
   SELECT pc.Name as CategoryName, p.name as ProductName
   FROM [SalesLT].[ProductCategory] pc
   JOIN [SalesLT].[Product] p
   ON pc.productcategoryid = p.productcategoryid;
   ```

3. Klicken Sie in der Symbolleiste auf **Ausführen**, um Daten aus den Tabellen „Product“ und „ProductCategory“ abzurufen.

    ![query](./media/sql-database-connect-query-ssms/query.png)

## <a name="insert-data"></a>Einfügen von Daten

Verwenden Sie den folgenden Code, um mit der [INSERT](https://msdn.microsoft.com/library/ms174335.aspx)-Transact-SQL-Anweisung ein neues Produkt in die Tabelle „SalesLT.Product“ einzufügen.

1. Ersetzen Sie im Abfragefenster die vorherige Abfrage durch die folgende Abfrage:

   ```sql
   INSERT INTO [SalesLT].[Product]
           ( [Name]
           , [ProductNumber]
           , [Color]
           , [ProductCategoryID]
           , [StandardCost]
           , [ListPrice]
           , [SellStartDate]
           )
     VALUES
           ('myNewProduct'
           ,123456789
           ,'NewColor'
           ,1
           ,100
           ,100
           ,GETDATE() );
   ```

2. Klicken Sie in der Symbolleiste auf **Ausführen**, um eine neue Zeile in die Tabelle „Product“ einzufügen.

    <img src="./media/sql-database-connect-query-ssms/insert.png" alt="insert" style="width: 780px;" />

## <a name="update-data"></a>Aktualisieren von Daten

Verwenden Sie den folgenden Code, um das neue zuvor hinzugefügte Produkt mit der [UPDATE](https://msdn.microsoft.com/library/ms177523.aspx)-Transact-SQL-Anweisung zu aktualisieren.

1. Ersetzen Sie im Abfragefenster die vorherige Abfrage durch die folgende Abfrage:

   ```sql
   UPDATE [SalesLT].[Product]
   SET [ListPrice] = 125
   WHERE Name = 'myNewProduct';
   ```

2. Klicken Sie in der Symbolleiste auf **Ausführen**, um die angegebene Zeile in der Tabelle „Product“ zu aktualisieren.

    <img src="./media/sql-database-connect-query-ssms/update.png" alt="update" style="width: 780px;" />

## <a name="delete-data"></a>Löschen von Daten

Verwenden Sie den folgenden Code, um das neue zuvor hinzugefügte Produkt mit der [DELETE](https://msdn.microsoft.com/library/ms189835.aspx)-Transact-SQL-Anweisung zu löschen.

1. Ersetzen Sie im Abfragefenster die vorherige Abfrage durch die folgende Abfrage:

   ```sql
   DELETE FROM [SalesLT].[Product]
   WHERE Name = 'myNewProduct';
   ```

2. Klicken Sie in der Symbolleiste auf **Ausführen**, um die angegebene Zeile in der Tabelle „Product“ zu löschen.

    <img src="./media/sql-database-connect-query-ssms/delete.png" alt="delete" style="width: 780px;" />

## <a name="next-steps"></a>Nächste Schritte

- Informationen zu SSMS finden Sie unter [Verwenden von SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx).
- Informationen zum Herstellen einer Verbindung mit Visual Studio finden Sie unter [Verbinden und Abfragen mit Visual Studio Code](sql-database-connect-query-vscode.md).
- Informationen zum Herstellen einer Verbindung und Senden von Abfragen mit .NET finden Sie unter [Verbinden und Abfragen mit .NET](sql-database-connect-query-dotnet.md).
- Informationen zum Herstellen einer Verbindung und Senden von Abfragen mit PHP finden Sie unter [Verbinden und Abfragen mit PHP](sql-database-connect-query-php.md).
- Informationen zum Herstellen einer Verbindung und Senden von Abfragen mit Node.js finden Sie unter [Verbinden und Abfragen mit Node.js](sql-database-connect-query-nodejs.md).
- Informationen zum Herstellen einer Verbindung und Senden von Abfragen mit Java finden Sie unter [Verbinden und Abfragen mit Java](sql-database-connect-query-java.md).
- Informationen zum Herstellen einer Verbindung und Senden von Abfragen mit Python finden Sie unter [Verbinden und Abfragen mit Python](sql-database-connect-query-python.md).
- Informationen zum Herstellen einer Verbindung und Senden von Abfragen mit Ruby finden Sie unter [Verbinden und Abfragen mit Ruby](sql-database-connect-query-ruby.md).

