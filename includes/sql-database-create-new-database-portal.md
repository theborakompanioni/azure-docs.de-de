
<!--
includes/sql-database-create-new-database-portal.md

Latest Freshness check:  2016-04-11 , carlrab.

As of circa 2016-04-11, the following topics might include this include:
articles/sql-database/sql-database-get-started-tutorial.md

-->
## Erstellen einer neuen Azure SQL-Datenbank
Führen Sie im Azure-Portal folgende Schritte aus, um auf einem neuen oder bereits vorhandenen logischen Azure SQL-Datenbankserver eine neue Azure SQL-Datenbank zu erstellen.

1. Stellen Sie eine Verbindung mit dem [Azure-Portal](http://portal.azure.com) her (sofern noch keine Verbindung besteht).
2. Klicken Sie auf **Neu**, geben Sie **SQL-Datenbank** ein, und klicken Sie anschließend auf **SQL Database (new database)** (SQL-Datenbank (neue Datenbank)).
   
     ![Neue Datenbank](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-1.png)
3. Klicken Sie auf **SQL Database (new database)** (SQL-Datenbank (neue Datenbank)).
   
     ![Neue Datenbank](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-2.png)
4. Klicken Sie auf **Erstellen**, um eine neue Datenbank im SQL-Datenbankdienst zu erstellen.
   
     ![Neue Datenbank](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-3.png)
5. Geben Sie Werte für die folgenden Servereigenschaften ein:
   
   * Datenbankname
   * Abonnement: Dies ist nur relevant, wenn Sie über mehrere Abonnements verfügen.
   * Ressourcengruppe: Verwenden Sie zum Einstieg die Ressourcengruppe des logischen Servers.
   * Quelle auswählen: Wählen Sie eine leere Datenbank, Beispieldaten oder eine Datenbanksicherung aus. Informationen zum Migrieren einer lokalen SQL Server-Datenbank oder zum Laden von Daten mithilfe des BCP-Befehlszeilentools finden Sie unter den Links am Ende dieses Artikels.
   * Server: Ein neuer oder bereits vorhandener logischer Server
   * Serveradministratoranmeldung
   * Kennwort
   * Tarif: Behalten Sie zum Einstieg den Standardwert „S0“ bei.
   * Sortierung: Diese Option ist nur relevant, wenn eine leere Datenbank ausgewählt wurde.
     
        ![New database](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-4.png)
6. Klicken Sie auf **Create**. Im Infobereich wird auf den Beginn der Bereitstellung hingewiesen.
   
    ![Neue Datenbank](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-5.png)
7. Warten Sie, bis die Bereitstellung abgeschlossen ist, bevor Sie mit dem nächsten Schritt fortfahren.
   
     ![Neue Datenbank](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-6.png)

<!---HONumber=AcomDC_0907_2016-->