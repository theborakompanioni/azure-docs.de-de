
<!--
includes/sql-database-create-new-server-firewall-portal.md

Latest Freshness check:  2016-11-28 , rickbyh.

As of circa 2016-04-11, the following topics might include this include:
articles/sql-database/sql-database-get-started-tutorial.md
articles/sql-database/sql-database-configure-firewall-settings

-->
## <a name="create-a-new-azure-sql-server-level-firewall"></a>Erstellen einer neuen Azure SQL-Firewall auf Serverebene
Führen Sie im Azure-Portal die folgenden Schritte aus, um eine Firewallregel auf Serverebene zu erstellen, die Verbindungen von einer einzelnen IP-Adresse (Ihr Clientcomputer) oder einem gesamten IP-Adressbereich mit einem logischen SQL-Datenbankserver zulässt.

1. Stellen Sie eine Verbindung mit dem [Azure-Portal](http://portal.azure.com)her (sofern noch keine Verbindung besteht).
2. Klicken Sie auf dem Standardblatt auf **SQL Server**.
   
      ![Neue Serverfirewall](./media/sql-database-create-new-server-firewall-portal/sql-database-create-new-server-firewall-portal-1.png)
3. Klicken Sie auf dem Blatt **SQL Server** auf den Server, für den die Firewallregel erstellt werden soll.
   
     ![Neue Serverfirewall](./media/sql-database-create-new-server-firewall-portal/sql-database-create-new-server-firewall-portal-2.png)
4. Überprüfen Sie die Eigenschaften des Servers, und klicken Sie dann auf **Firewall**.
   
     ![Neue Serverfirewall](./media/sql-database-create-new-server-firewall-portal/sql-database-create-new-server-firewall-portal-3.png)
   
   > [!NOTE]
   > Sie können auch auf dem Blatt **Datenbank** über die Symbolleiste auf das Blatt **Firewalleinstellungen** der Serverebene zugreifen.
    
    
6. Klicken Sie auf **Client-IP hinzufügen**, damit Azure die Felder für die Regel mit Ihrer IP-Adresse ausfüllt.
   
      ![Neue Serverfirewall](./media/sql-database-create-new-server-firewall-portal/sql-database-create-new-server-firewall-portal-5.png)
7. Klicken Sie optional auf die hinzugefügte IP-Adresse, um die Firewalladresse zu bearbeiten und Zugriff auf einen IP-Adressbereich zu gewähren.
   
      ![Neue Serverfirewall](./media/sql-database-create-new-server-firewall-portal/sql-database-create-new-server-firewall-portal-6.png)
8. Klicken Sie auf **Speichern** , um die Firewallregel auf Serverebene zu erstellen.
   
     ![Neue Serverfirewall](./media/sql-database-create-new-server-firewall-portal/sql-database-create-new-server-firewall-portal-7.png)
   
   > [!IMPORTANT]
   > Die Client-IP-Adresse kann sich von Zeit zu Zeit ändern, und Sie können dann möglicherweise nicht auf den Server zugreifen, bis Sie eine neue Firewallregel erstellt haben. Sie können Ihre IP-Adresse mit [Bing](http://www.bing.com/search?q=my%20ip%20address)überprüfen. Fügen Sie dann eine einzelne IP-Adresse oder einen Bereich von IP-Adressen hinzu. Ausführlichere Informationen finden Sie unter [Verwalten von Firewalleinstellungen](../articles/sql-database/sql-database-configure-firewall-settings.md#manage-existing-server-level-firewall-rules-through-the-azure-portal).
   > 
   > 



<!--HONumber=Nov16_HO5-->


