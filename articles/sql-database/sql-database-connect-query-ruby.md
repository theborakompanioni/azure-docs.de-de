---
title: Abfragen von Azure SQL-Datenbank mithilfe von Ruby | Microsoft-Dokumentation
description: In diesem Thema erfahren Sie, wie Sie unter Verwendung von Ruby ein Programm erstellen, das eine Verbindung mit einer Azure SQL-Datenbank herstellt und sie mithilfe von Transact-SQL-Anweisungen abfragt.
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 94fec528-58ba-4352-ba0d-25ae4b273e90
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: hero-article
ms.date: 07/14/2017
ms.author: carlrab
ms.translationtype: HT
ms.sourcegitcommit: c999eb5d6b8e191d4268f44d10fb23ab951804e7
ms.openlocfilehash: 25ff9a9cfaa5494dbb006c84e235099fe51e6545
ms.contentlocale: de-de
ms.lasthandoff: 07/17/2017

---

# <a name="use-ruby-to-query-an-azure-sql-database"></a>Abfragen einer Azure SQL-Datenbank mithilfe von Ruby

In diesem Schnellstarttutorial erfahren Sie, wie Sie unter Verwendung von [Ruby](https://www.ruby-lang.org) ein Programm erstellen, um eine Verbindung mit einer Azure SQL-Datenbank herzustellen und mithilfe von Transact-SQL-Anweisungen Daten abzufragen.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Schnellstarttutorial benötigen Sie Folgendes:

- Eine Azure SQL-Datenbank. In dieser Schnellstartanleitung werden die Ressourcen aus einer der folgenden Schnellstartanleitungen verwendet: 

   - [Erstellen einer Datenbank – Portal](sql-database-get-started-portal.md)
   - [Erstellen einer Datenbank – CLI](sql-database-get-started-cli.md)
   - [Erstellen einer Datenbank – PowerShell](sql-database-get-started-powershell.md)

- Eine [Firewallregel auf Serverebene](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) für die öffentliche IP-Adresse des Computers, den Sie für dieses Schnellstarttutorial verwenden.
- Ruby und die dazugehörige Software müssen für das Betriebssystem installiert sein.
    - **MacOS**: Installieren Sie nacheinander Homebrew, rbenv und ruby-build, Ruby und FreeTDS. Weitere Informationen finden Sie unter [Schritt 1.2, 1.3, 1.4 und 1.5](https://www.microsoft.com/sql-server/developer-get-started/ruby/mac/).
    - **Ubuntu**: Installieren Sie nacheinander die erforderlichen Komponenten für Ruby, rbenv und ruby-build, Ruby und dann FreeTDS. Weitere Informationen finden Sie unter [Schritt 1.2, 1.3, 1.4 und 1.5](https://www.microsoft.com/sql-server/developer-get-started/ruby/ubuntu/).

## <a name="sql-server-connection-information"></a>SQL Server-Verbindungsinformationen

Rufen Sie die Verbindungsinformationen ab, die zum Herstellen einer Verbindung mit der Azure SQL-Datenbank erforderlich sind. In den weiteren Verfahren benötigen Sie den vollqualifizierten Servernamen, den Datenbanknamen und die Anmeldeinformationen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Wählen Sie im Menü auf der linken Seite die Option **SQL-Datenbanken**, und klicken Sie auf der Seite **SQL-Datenbanken** auf Ihre Datenbank. 
3. Überprüfen Sie auf der Seite **Übersicht** für Ihre Datenbank den vollqualifizierten Servernamen. Sie können den Cursor auf den Servernamen bewegen, um wie in der folgenden Abbildung die Option **Click to copy** (Klicken Sie zum Kopieren) anzuzeigen:

   ![Servername](./media/sql-database-connect-query-dotnet/server-name.png) 

4. Falls Sie die Anmeldeinformationen für Ihren Azure SQL-Datenbankserver vergessen haben, können Sie zur Seite des SQL-Datenbankservers navigieren, um den Serveradministrator-Benutzernamen anzuzeigen und ggf. das Kennwort zurückzusetzen.

> [!IMPORTANT]
> Sie benötigen eine Firewallregel für die öffentliche IP-Adresse des Computers, auf dem Sie dieses Tutorial ausführen. Wenn Sie an einem anderen Computer arbeiten oder eine andere öffentliche IP-Adresse verwenden, [erstellen Sie über das Azure-Portal eine Firewallregel auf Serverebene](sql-database-get-started-portal.md#create-a-server-level-firewall-rule). 

## <a name="insert-code-to-query-sql-database"></a>Einfügen von Code zum Abfragen der SQL-Datenbank

1. Erstellen Sie in Ihrem bevorzugten Text-Editor eine neue Datei namens **sqltest.rb**.

2. Ersetzen Sie den Inhalt durch den folgenden Code, und fügen Sie die entsprechenden Werte für Server, Datenbank, Benutzer und Kennwort hinzu.

```ruby
require 'tiny_tds'
server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
client = TinyTds::Client.new username: username, password: password, 
    host: server, port: 1433, database: database, azure: true

puts "Reading data from table"
tsql = "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
        FROM [SalesLT].[ProductCategory] pc
        JOIN [SalesLT].[Product] p
        ON pc.productcategoryid = p.productcategoryid"
result = client.execute(tsql)
result.each do |row|
    puts row
end
```

## <a name="run-the-code"></a>Ausführen des Codes

1. Führen Sie an der Eingabeaufforderung die folgenden Befehle aus:

   ```bash
   ruby sqltest.rb
   ```

2. Vergewissern Sie sich, dass die ersten 20 Zeilen zurückgegeben werden, und schließen Sie dann das Anwendungsfenster.


## <a name="next-steps"></a>Nächste Schritte
- [Entwurf Ihrer ersten Azure SQL-Datenbank](sql-database-design-first-database.md)
- [GitHub-Repository für TinyTDS](https://github.com/rails-sqlserver/tiny_tds)
- [Report issues or ask questions about TinyTDS](https://github.com/rails-sqlserver/tiny_tds/issues) (Melden von Problemen oder Stellen von Fragen zu TinyTDS)
- [Ruby Driver for SQL Server](https://docs.microsoft.com/sql/connect/ruby/ruby-driver-for-sql-server/) (Ruby-Treiber für SQL Server)

