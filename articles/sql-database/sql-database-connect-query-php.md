---
title: Abfragen von Azure SQL-Datenbank mithilfe von PHP | Microsoft-Dokumentation
description: In diesem Thema erfahren Sie, wie Sie unter Verwendung von PHP ein Programm erstellen, das eine Verbindung mit einer Azure SQL-Datenbank herstellt und sie mithilfe von Transact-SQL-Anweisungen abfragt.
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 4e71db4a-a 22f-4f1c-83e5-4a34a036ecf3
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: php
ms.topic: quickstart
ms.date: 08/08/2017
ms.author: carlrab
ms.translationtype: HT
ms.sourcegitcommit: 0aae2acfbf30a77f57ddfbaabdb17f51b6938fd6
ms.openlocfilehash: 3a43472ad2be4a0fd6f7126f72433acd8b5f25fd
ms.contentlocale: de-de
ms.lasthandoff: 08/09/2017

---
# <a name="use-php-to-query-an-azure-sql-database"></a>Abfragen einer Azure SQL-Datenbank mithilfe von PHP

In diesem Schnellstarttutorial erfahren Sie, wie Sie unter Verwendung von [PHP](http://php.net/manual/en/intro-whatis.php) ein Programm erstellen, um eine Verbindung mit einer Azure SQL-Datenbank herzustellen und mithilfe von Transact-SQL-Anweisungen Daten abzufragen.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Schnellstarttutorial benötigen Sie Folgendes:

- Eine Azure SQL-Datenbank. In dieser Schnellstartanleitung werden die Ressourcen aus einer der folgenden Schnellstartanleitungen verwendet: 

   - [Erstellen einer Datenbank – Portal](sql-database-get-started-portal.md)
   - [Erstellen einer Datenbank – CLI](sql-database-get-started-cli.md)
   - [Erstellen einer Datenbank – PowerShell](sql-database-get-started-powershell.md)

- Eine [Firewallregel auf Serverebene](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) für die öffentliche IP-Adresse des Computers, den Sie für dieses Schnellstarttutorial verwenden.

- PHP und die dazugehörige Software müssen für das Betriebssystem installiert sein.

    - **MacOS:** Installieren Sie Homebrew und PHP, installieren Sie den ODBC-Treiber und SQLCMD, und installieren Sie anschließend den PHP-Treiber für SQL Server. Informationen finden Sie unter [Schritt 1.2, 1.3 und 2.1](https://www.microsoft.com/en-us/sql-server/developer-get-started/php/mac/).
    - **Ubuntu:** Installieren Sie PHP und andere erforderliche Pakete, und installieren Sie anschließend den PHP-Treiber für SQL Server. Informationen finden Sie unter [Schritt 1.2 und 2.1](https://www.microsoft.com/sql-server/developer-get-started/php/ubuntu/).
    - **Windows**: Installieren Sie die neueste Version von PHP für IIS Express, die neueste Version der Microsoft-Treiber für SQL Server in IIS Express, Chocolatey, den ODBC-Treiber und SQLCMD. Informationen finden Sie unter [Schritt 1.2 und 1.3](https://www.microsoft.com/sql-server/developer-get-started/php/windows/).    

## <a name="sql-server-connection-information"></a>SQL Server-Verbindungsinformationen

Rufen Sie die Verbindungsinformationen ab, die zum Herstellen einer Verbindung mit der Azure SQL-Datenbank erforderlich sind. In den weiteren Verfahren benötigen Sie den vollqualifizierten Servernamen, den Datenbanknamen und die Anmeldeinformationen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Wählen Sie im Menü auf der linken Seite die Option **SQL-Datenbanken**, und klicken Sie auf der Seite **SQL-Datenbanken** auf Ihre Datenbank. 
3. Überprüfen Sie auf der Seite **Übersicht** für Ihre Datenbank den vollqualifizierten Servernamen wie in der folgenden Abbildung dargestellt. Sie können auf den Servernamen zeigen, um die Option **Klicken Sie zum Kopieren** anzuzeigen.  

   ![Servername](./media/sql-database-connect-query-dotnet/server-name.png) 

4. Falls Sie die Anmeldeinformationen für Ihren Server vergessen, können Sie zur Seite des SQL-Datenbankservers navigieren, um den Serveradministrator-Benutzernamen anzuzeigen und ggf. das Kennwort zurückzusetzen.     
    
## <a name="insert-code-to-query-sql-database"></a>Einfügen von Code zum Abfragen der SQL-Datenbank

1. Erstellen Sie in Ihrem bevorzugten Text-Editor eine neue Datei namens **sqltest.php**.  

2. Ersetzen Sie den Inhalt durch den folgenden Code, und fügen Sie die entsprechenden Werte für Server, Datenbank, Benutzer und Kennwort hinzu.

   ```PHP
   <?php
   $serverName = "your_server.database.windows.net";
   $connectionOptions = array(
       "Database" => "your_database",
       "Uid" => "your_username",
       "PWD" => "your_password"
   );
   //Establishes the connection
   $conn = sqlsrv_connect($serverName, $connectionOptions);
   $tsql= "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
           FROM [SalesLT].[ProductCategory] pc
           JOIN [SalesLT].[Product] p
        ON pc.productcategoryid = p.productcategoryid";
   $getResults= sqlsrv_query($conn, $tsql);
   echo ("Reading data from table" . PHP_EOL);
   if ($getResults == FALSE)
       echo (sqlsrv_errors());
   while ($row = sqlsrv_fetch_array($getResults, SQLSRV_FETCH_ASSOC)) {
    echo ($row['CategoryName'] . " " . $row['ProductName'] . PHP_EOL);
   }
   sqlsrv_free_stmt($getResults);
   ?>
   ```

## <a name="run-the-code"></a>Ausführen des Codes

1. Führen Sie an der Eingabeaufforderung die folgenden Befehle aus:

   ```php
   php sqltest.php
   ```

2. Vergewissern Sie sich, dass die ersten 20 Zeilen zurückgegeben werden, und schließen Sie dann das Anwendungsfenster.

## <a name="next-steps"></a>Nächste Schritte
- [Entwurf Ihrer ersten Azure SQL-Datenbank](sql-database-design-first-database.md)
- [Microsoft PHP Drivers for SQL Server](https://github.com/Microsoft/msphpsql/) (Microsoft-PHP-Treiber für SQL Server)
- [Seite für Problemmeldungen und Fragen](https://github.com/Microsoft/msphpsql/issues)

