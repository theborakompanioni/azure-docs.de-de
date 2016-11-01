

## Erstellen neuer Datenbankbenutzer mit SSMS

Führen Sie die folgenden Schritte aus, um mithilfe von SSMS einen neuen Datenbankbenutzer in einer vorhandenen Datenbank zu erstellen.

Bei diesen Schritten wird davon ausgegangen, dass Sie mithilfe von SSMS eine Verbindung mit der SQL-Datenbank im Objekt-Explorer herstellt haben und mit dem logischen SQL-Datenbankserver als Prinzipaladministrator auf Serverebene oder mit einem Benutzerkonto, das über die Berechtigungen zum Erstellen eines neuen Benutzers verfügt, verbunden sind.

1. Erweitern Sie im Objekt-Explorer den Datenbankknoten, und wählen Sie die Datenbank aus, in der Sie ein neues Benutzerkonto erstellen möchten.

     ![SQL Server Management Studio: Verbinden mit einem SQL-Datenbankserver](./media/sql-database-create-new-database-user/sql-database-create-new-database-user-1.png)

2. Klicken Sie mit der rechten Maustaste auf die ausgewählte Datenbank, und klicken Sie dann auf **Abfrage**.

     ![SQL Server Management Studio: Verbinden mit einem SQL-Datenbankserver](./media/sql-database-create-new-database-user/sql-database-create-new-database-user-2.png)

3. Bearbeiten Sie im Abfragefenster die folgende Transact-SQL-Anweisungen, und verwenden Sie sie zum Erstellen eines enthaltenen Benutzers in Ihrer Benutzerdatenbank.

    ```CREATE USER user1 WITH PASSWORD ='p@ssw0rd1';
    ```

     ![SQL Server Management Studio: Verbinden mit einem SQL-Datenbankserver](./media/sql-database-create-new-database-user/sql-database-create-new-database-user-3.png)

<!---HONumber=AcomDC_0629_2016-->