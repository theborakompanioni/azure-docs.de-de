### Voraussetzungen

- Ein [SQL Azure](https://www.microsoft.com/sql)-Konto  


Bevor Sie Ihr SQL Azure-Konto in einer Logik-App verwenden können, müssen Sie die Logik-App für die Verbindung mit Ihrem SQL Azure-Konto autorisieren. Glücklicherweise können Sie dies ganz einfach in Ihrer Logik-App im Azure-Portal durchführen.

Hier finden Sie die Schritte zum Autorisieren Ihrer Logik-App für Verbindungen mit Ihrem SQL Azure-Konto:
1. Um eine Verbindung mit SQL Azure herzustellen, wählen Sie im Logik-App-Designer in der Dropdownliste **Verwaltete Microsoft-APIs anzeigen** aus, und geben Sie dann *SQL Azure* in das Suchfeld ein. Wählen Sie den zu verwendenden Trigger oder die gewünschte Aktion aus: ![Schritt zur Erstellung der SQL Azure-Verbindung](./media/connectors-create-api-sql/sql-1.png)  
2. Wenn Sie noch keine Verbindungen mit SQL Azure hergestellt haben, werden Sie aufgefordert, Ihre SQL Azure-Anmeldeinformationen anzugeben. Mit diesen Anmeldeinformationen kann Ihre Logik-App eine Verbindung mit dem SQL Azure-Konto herstellen und auf dessen Daten zugreifen: ![Schritt zur Erstellung der SQL Azure-Verbindung](./media/connectors-create-api-sql/sql-2.png)  
3. Beachten Sie, dass die Verbindung erstellt wurde und Sie nun mit anderen Schritten in Ihrer Logik-App fortfahren können: ![Schritt zur Erstellung der SQL Azure-Verbindung](./media/connectors-create-api-sql/sql-3.png)  

<!---HONumber=AcomDC_0525_2016-->