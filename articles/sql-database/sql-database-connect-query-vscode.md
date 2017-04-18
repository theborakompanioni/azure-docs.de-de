---
title: 'VS Code: Herstellen der Verbindung und Abfragen von Daten in Azure SQL-Datenbank | Microsoft-Dokumentation'
description: "Es wird beschrieben, wie Sie mit Visual Studio Code eine Verbindung mit SQL-Datenbank in Azure herstellen. Führen Sie anschließend T-SQL-Anweisungen (Transact-SQL) aus, um Daten abzufragen und zu bearbeiten."
metacanonical: 
keywords: Herstellen einer Verbindung mit SQL-Datenbank
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 676bd799-a571-4bb8-848b-fb1720007866
ms.service: sql-database
ms.custom: quick start manage
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 03/17/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: ff5d156ab2b701233c4cdbf08e3d6e517c01b9fb
ms.lasthandoff: 04/12/2017


---
# <a name="azure-sql-database-use-visual-studio-code-to-connect-and-query-data"></a>Azure SQL-Datenbank: Verwenden von Visual Studio Code zum Herstellen einer Verbindung mit und Abfragen von Daten

[Visual Studio Code](https://code.visualstudio.com/docs) ist ein grafischer Code-Editor für Linux, Mac OS und Windows, der Erweiterungen unterstützt, z.B. die [mssql-Erweiterung](https://aka.ms/mssql-marketplace) zum Abfragen von Microsoft SQL Server, Azure SQL-Datenbank und SQL Data Warehouse. In diesem Schnellstart wird veranschaulicht, wie Sie Visual Studio Code zum Herstellen einer Verbindung mit einer Azure SQL-Datenbank verwenden und anschließend Transact-SQL-Anweisungen nutzen, um Daten in der Datenbank abzufragen, einzufügen, zu aktualisieren und zu löschen.

In diesem Schnellstart werden als Ausgangspunkt die Ressourcen verwendet, die in einem der folgenden Schnellstarts erstellt wurden:

- [Erstellen einer Datenbank – Portal](sql-database-get-started-portal.md)
- [Erstellen einer Datenbank – CLI](sql-database-get-started-cli.md)

Stellen Sie zunächst sicher, dass Sie die neueste Version von [Visual Studio Code](https://code.visualstudio.com/Download) installiert und die [mssql-Erweiterung](https://aka.ms/mssql-marketplace) geladen haben. Eine Installationsanleitung für die mssql-Erweiterung finden Sie unter [Install VS Code](https://docs.microsoft.com/sql/linux/sql-server-linux-develop-use-vscode#install-vs-code) (Installieren von VS Code) und [mssql for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-mssql.mssql) (mssql für Visual Studio Code). 

## <a name="configure-vs-code-mac-os-only"></a>Konfigurieren von VS Code (nur Mac OS)

### <a name="mac-os"></a>**Mac OS**
Für Mac OS müssen Sie OpenSSL installieren, da dies eine Voraussetzung für DotNet Core ist. DotNet Core wird von der mssql-Erweiterung verwendet. Öffnen Sie Ihr Terminal, und geben Sie die folgenden Befehle ein, um **brew** und **OpenSSL** zu installieren. 

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew update
brew install openssl
mkdir -p /usr/local/lib
ln -s /usr/local/opt/openssl/lib/libcrypto.1.0.0.dylib /usr/local/lib/
ln -s /usr/local/opt/openssl/lib/libssl.1.0.0.dylib /usr/local/lib/
```

## <a name="get-connection-information"></a>Abrufen von Verbindungsinformationen

Rufen Sie den vollqualifizierten Servernamen für Ihren Azure SQL-Datenbankserver im Azure-Portal ab. Sie verwenden den vollqualifizierten Servernamen, um mit Visual Studio Code eine Verbindung mit Ihrem Server herzustellen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Wählen Sie im Menü auf der linken Seite die Option **SQL-Datenbanken**, und klicken Sie auf der Seite **SQL-Datenbanken** auf Ihre Datenbank. 
3. Suchen Sie im Azure-Portal auf der Seite für Ihre Datenbank unter **Zusammenfassung** nach Ihrer Datenbank, und kopieren Sie den **Servernamen** zur späteren Verwendung in diesem Schnellstart.

    <img src="./media/sql-database-connect-query-vscode/connection-information.png" alt="connection information" style="width: 780px;" />

## <a name="set-language-mode-to-sql"></a>Festlegen des Sprachmodus auf SQL

Legen Sie den Sprachmodus in Visual Studio Code auf **SQL** fest, um die Verwendung von mssql-Befehlen und T-SQL IntelliSense zu ermöglichen.

1. Öffnen Sie ein neues Visual Studio Code-Fenster. 

2. Drücken Sie **⌘+K,M** bzw. **STRG+K,M** (Mac- bzw. Windows-Option), geben Sie **SQL** ein, und drücken Sie die **EINGABETASTE**, um den Sprachmodus auf SQL festzulegen. 

<img src="./media/sql-database-connect-query-vscode/vscode-language-mode.png" alt="SQL language mode" style="width: 780px;" />

## <a name="connect-to-the-server"></a>Herstellen einer Verbindung mit dem Server

Verwenden Sie Visual Studio Code, um eine Verbindung mit Ihrem Azure SQL-Datenbankserver herzustellen.

1. Drücken Sie in VS Code die Tastenkombination **STRG+UMSCHALT+P** (oder **F1**), um die Befehlspalette zu öffnen.

2. Geben Sie **sqlcon** ein, drücken Sie die **EINGABETASTE**, und legen Sie Ihre Sprache auf **SQL** fest.

3. Drücken Sie die **EINGABETASTE**, um die Option **Create Connection Profile** (Verbindungsprofil erstellen) auszuwählen. Ein Verbindungsprofil für Ihre SQL Server-Instanz wird erstellt.

4. Befolgen Sie die Eingabeaufforderungen, um die Verbindungseigenschaften für das neue Verbindungsprofil anzugeben. Drücken Sie nach dem Angeben der einzelnen Werte die **EINGABETASTE**, um fortzufahren. 

   In der folgenden Tabelle sind die Verbindungsprofileigenschaften beschrieben.

   | Einstellung | Beschreibung |
   |-----|-----|
   | **Servername** | Geben Sie den vollqualifizierten Servernamen ein, z.B. **mynewserver20170313.database.windows.net**. |
   | **Datenbankname** | Geben Sie den Datenbanknamen ein, z.B. **mySampleDatabase**. |
   | **Authentifizierung** | Auswählen der SQL-Anmeldung |
   | **Benutzername** | Geben Sie Ihr Serveradministratorkonto ein. |
   | **Kennwort (SQL-Anmeldung)** | Geben Sie das Kennwort für Ihr Serveradministratorkonto ein. | 
   | **Kennwort speichern?** | Wählen Sie **Ja** oder **Nein**. |
   | **[Optional] Geben Sie einen Namen für dieses Profil ein** | Geben Sie einen Namen für das Verbindungsprofil ein, z.B. **mySampleDatabase**. 

5. Drücken Sie die Taste **ESC**, um die Meldung mit dem Hinweis, dass das Profil erstellt und die Verbindung dafür hergestellt wurde, zu schließen.

6. Überprüfen Sie die Verbindung in der Statusleiste.

   <img src="./media/sql-database-connect-query-vscode/vscode-connection-status.png" alt="Connection status" style="width: 780px;" />

## <a name="query-data"></a>Abfragen von Daten

Verwenden Sie die [SELECT](https://msdn.microsoft.com/library/ms189499.aspx)-Transact-SQL-Anweisung, um Daten in Ihrer Azure SQL-Datenbank abzufragen.

1. Geben Sie im Fenster **Editor** die folgende Abfrage in das leere Abfragefenster ein:

   ```sql
   SELECT pc.Name as CategoryName, p.name as ProductName
   FROM [SalesLT].[ProductCategory] pc
   JOIN [SalesLT].[Product] p
   ON pc.productcategoryid = p.productcategoryid;
   ```

2. Drücken Sie **STRG+UMSCHALT+E**, um Daten aus den Tabellen „Product“ und „ProductCategory“ abzurufen.

    <img src="./media/sql-database-connect-query-vscode/query.png" alt="Query" style="width: 780px;" />

## <a name="insert-data"></a>Einfügen von Daten

Verwenden Sie die [INSERT](https://msdn.microsoft.com/library/ms174335.aspx)-Transact-SQL-Anweisung, um Daten in Ihre Azure SQL-Datenbank einzufügen.

1. Löschen Sie im Fenster **Editor** die vorherige Abfrage, und geben Sie die folgende Abfrage ein:

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

2. Drücken Sie **STRG+UMSCHALT+E**, um eine neue Zeile in die Tabelle „Product“ einzufügen.

## <a name="update-data"></a>Aktualisieren von Daten

Verwenden Sie die [UPDATE](https://msdn.microsoft.com/library/ms177523.aspx)-Transact-SQL-Anweisung, um Daten in Ihrer Azure SQL-Datenbank zu aktualisieren.

1.  Löschen Sie im Fenster **Editor** die vorherige Abfrage, und geben Sie die folgende Abfrage ein:

   ```sql
   UPDATE [SalesLT].[Product]
   SET [ListPrice] = 125
   WHERE Name = 'myNewProduct';
   ```

2. Drücken Sie **STRG+UMSCHALT+E**, um die angegebene Zeile in der Tabelle „Product“ zu aktualisieren.

## <a name="delete-data"></a>Löschen von Daten

Verwenden Sie die [DELETE](https://msdn.microsoft.com/library/ms189835.aspx)-Transact-SQL-Anweisung, um Daten in Ihrer Azure SQL-Datenbank zu löschen.

1. Löschen Sie im Fenster **Editor** die vorherige Abfrage, und geben Sie die folgende Abfrage ein:

   ```sql
   DELETE FROM [SalesLT].[Product]
   WHERE Name = 'myNewProduct';
   ```

2. Drücken Sie **STRG+UMSCHALT+E**, um die angegebene Zeile in der Tabelle „Product“ zu löschen.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum Herstellen einer Verbindung und Durchführen von Abfragen mit SQL Server Management Studio finden Sie unter [Verbinden und Abfragen mit SSMS](sql-database-connect-query-ssms.md).
- Informationen zum Herstellen einer Verbindung und Senden von Abfragen mit .NET finden Sie unter [Verbinden und Abfragen mit .NET](sql-database-connect-query-dotnet.md).
- Informationen zum Herstellen einer Verbindung und Senden von Abfragen mit PHP finden Sie unter [Verbinden und Abfragen mit PHP](sql-database-connect-query-php.md).
- Informationen zum Herstellen einer Verbindung und Senden von Abfragen mit Node.js finden Sie unter [Verbinden und Abfragen mit Node.js](sql-database-connect-query-nodejs.md).
- Informationen zum Herstellen einer Verbindung und Senden von Abfragen mit Java finden Sie unter [Verbinden und Abfragen mit Java](sql-database-connect-query-java.md).
- Informationen zum Herstellen einer Verbindung und Senden von Abfragen mit Python finden Sie unter [Verbinden und Abfragen mit Python](sql-database-connect-query-python.md).
- Informationen zum Herstellen einer Verbindung und Senden von Abfragen mit Ruby finden Sie unter [Verbinden und Abfragen mit Ruby](sql-database-connect-query-ruby.md).

