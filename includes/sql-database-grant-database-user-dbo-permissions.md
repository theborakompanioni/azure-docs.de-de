## Erstellen neuer Datenbankbenutzer mit SSMS

In den folgenden Schritten wird davon ausgegangen, dass Sie SSMS verwenden und eine Verbindung mit der SQL-Datenbank im Objekt-Explorer hergestellt haben. Darüber hinaus wird angenommen, dass die Verbindung mit dem logischen Server der SQL-Datenbank als Prinzipaladministrator auf Serverebene oder mit einem Benutzerkonto mit Berechtigungen zum Erteilen von Benutzerberechtigungen hergestellt wurde. Zudem wird in den folgenden Schritten davon ausgegangen, dass ein Benutzer in der Datenbank vorhanden ist, dem Sie DBO-Berechtigungen erteilen möchten.

1. Erweitern Sie im Objekt-Explorer den Datenbankknoten, und wählen Sie die Datenbank mit dem Benutzer aus, dem Sie DBO-Berechtigungen erteilen möchten.

     ![SQL Server Management Studio: Verbinden mit einem SQL-Datenbankserver](./media/sql-database-create-new-database-user/sql-database-create-new-database-user-1.png)

2. Klicken Sie mit der rechten Maustaste auf die ausgewählte Datenbank, und klicken Sie dann auf **Abfrage**.

     ![SQL Server Management Studio: Verbinden mit einem SQL-Datenbankserver](./media/sql-database-create-new-database-user/sql-database-create-new-database-user-2.png)

3. Bearbeiten Sie im Abfragefenster die folgende Transact-SQL-Anweisungen, und verwenden Sie sie zum Erteilen von DBO-Berechtigungen für einen bestimmten Benutzer.

    '''ALTER ROLE db\_owner ADD MEMBER user1;

     ![SQL Server Management Studio: Verbinden mit einem SQL-Datenbankserver](./media/sql-database-grant-database-user-dbo-permissions/sql-database-grant-database-user-dbo-permissions-1.png)

<!---HONumber=AcomDC_0420_2016-->