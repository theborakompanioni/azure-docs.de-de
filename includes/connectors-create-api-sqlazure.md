### <a name="prerequisites"></a>Voraussetzungen
* Ein Azure-Konto. Sie können ein [kostenloses Konto](https://azure.microsoft.com/free) erstellen.
* Eine [Azure SQL-Datenbank](../articles/sql-database/sql-database-get-started.md) mit zugehörigen Verbindungsinformationen, einschließlich Servername, Datenbankname und Benutzername/Kennwort. Diese Informationen sind in der Verbindungszeichenfolge der SQL-Datenbank enthalten:
  
    Server=tcp:*IhrSQLServerName*.database.windows.net,1433;Initial Catalog=*IhrSQLDBName*;Persist Security Info=False;User ID={Ihr_Benutzername};Password={Ihr_Kennwort};MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
  
    Erfahren Sie mehr über [Azure SQL-Datenbanken](https://azure.microsoft.com/services/sql-database).

> [!NOTE]
> Wenn Sie eine Azure SQL-Datenbank erstellen, können Sie auch die in SQL enthaltenen Beispieldatenbanken erstellen. 
> 
> 

Stellen Sie eine Verbindung mit der SQL-Datenbank her, bevor Sie die Azure SQL-Datenbank in einer Logik-App verwenden. Sie können dies ganz einfach in Ihrer Logik-App im Azure-Portal durchführen.  

Stellen Sie die Verbindung mit Ihrer Azure SQL-Datenbank mit den folgenden Schritten her:  

1. Erstellen Sie eine Logik-App. Fügen Sie im Designer für Logik-Apps einen Trigger hinzu, und fügen Sie dann eine Aktion hinzu. Wählen Sie in der Dropdownliste **Von Microsoft verwaltete APIs anzeigen** aus, und geben Sie dann „SQL“ in das Suchfeld ein. Wählen Sie eine der Aktionen aus:  
   
    ![Schritt zur Erstellung der SQL Azure-Verbindung](./media/connectors-create-api-sqlazure/sql-actions.png)
2. Wenn Sie zuvor noch keine Verbindungen mit der SQL-Datenbank hergestellt haben, werden Sie aufgefordert, die Details der Verbindung anzugeben:  
   
    ![Schritt zur Erstellung der SQL Azure-Verbindung](./media/connectors-create-api-sqlazure/connection-details.png) 
3. Geben Sie die SQL-Datenbank-Details ein. Mit einem Sternchen gekennzeichnete Eigenschaften müssen angegeben werden.
   
   | Eigenschaft | Details |
   | --- | --- |
   | Connect via Gateway (Verbinden über Gateway) |Lassen Sie dieses Kontrollkästchen deaktiviert. Diese Option wird bei der Verbindungsherstellung mit einer lokalen SQL Server-Instanz verwendet. |
   | Verbindungsname* |Geben Sie einen beliebigen Namen für Ihre Verbindung ein. |
   | SQL Server-Name* |Geben Sie den Servernamen ein. Dieser sieht in etwa wie folgt aus: *servername.database.windows.net*. Der Servername wird sowohl im Azure-Portal als auch in der Verbindungszeichenfolge in den SQL-Datenbankeigenschaften angezeigt. |
   | SQL-Datenbankname* |Geben Sie den Namen Ihrer SQL-Datenbank ein. Dieser wird in den SQL-Datenbankeigenschaften in der Verbindungszeichenfolge aufgeführt: Initial Catalog=*IhrSQLDBName*. |
   | Benutzername* |Geben Sie den Benutzernamen ein, den Sie beim Erstellen der SQL-Datenbank erstellt haben. Dieser wird im Azure-Portal in den SQL-Datenbankeigenschaften aufgeführt. |
   | Kennwort* |Geben Sie das Kennwort ein, das Sie beim Erstellen der SQL-Datenbank erstellt haben. |
   
    Mit diesen Anmeldeinformationen kann Ihre Logik-App eine Verbindung herstellen und auf Ihre SQL-Daten zugreifen. Nach Abschluss des Vorgangs sehen Ihre Verbindungsdetails in etwa wie folgt aus:  
   
    ![Schritt zur Erstellung der SQL Azure-Verbindung](./media/connectors-create-api-sqlazure/sample-connection.png) 
4. Klicken Sie auf **Erstellen**. 
5. Beachten Sie, dass die Verbindung erstellt wurde. Fahren Sie nun mit den weiteren Schritten in Ihrer Logik-App fort: 
   
    ![Schritt zur Erstellung der SQL Azure-Verbindung](./media/connectors-create-api-sqlazure/table.png)

