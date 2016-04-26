## Herstellen einer Verbindung mit einer Azure SQL-Datenbank mithilfe einer Prinzipalanmeldung auf Serverebene

Führen Sie die folgenden Schritte aus, um mit SSMS mithilfe einer Prinzipalanmeldung auf Serverebene eine Verbindung mit der Azure SQL-Datenbank herzustellen.

1. Geben Sie im Windows-Suchfeld „Microsoft SQL Server Management Studio“ ein, und klicken Sie dann auf die Desktop-App, um SSMS zu starten.

2. Geben Sie im Fenster „Mit Server verbinden“ die folgenden Informationen ein:

 - **Servertyp:** Standardmäßig ist „Datenbankmodul“ angegeben. Ändern Sie diesen Wert nicht.
 - **Servername:** Geben Sie den Namen des Servers, auf dem die SQL-Datenbank gehostet wird, im folgenden Format ein: *&lt;Servername>*.**database.windows.net**
 - **Authentifizierungstyp:** Wählen Sie „SQL-Authentifizierung“, wenn Sie gerade die ersten Schritte ausführen. Wenn Sie Active Directory für Ihren logischen SQL-Datenbankserver aktiviert haben, können Sie entweder „Active Directory-Kennwortauthentifizierung“ oder „Integrierte Active Directory-Authentifizierung“ wählen.
 - **Benutzername:** Wenn Sie „SQL-Authentifizierung“ oder „Active Directory-Kennwortauthentifizierung“ gewählt haben, geben Sie den Namen eines Benutzers ein, der Zugriff auf eine Datenbank auf dem Server hat.
 - **Kennwort:** Wenn Sie „SQL-Authentifizierung“ oder „Active Directory-Kennwortauthentifizierung“ gewählt haben, geben Sie das Kennwort für den angegebenen Benutzer ein.
   
       ![SQL Server Management Studio: Verbinden mit einem SQL-Datenbankserver](./media/sql-database-sql-server-management-studio-connect-server-principal/connect-server-principal-1.png)

3. Klicken Sie auf **Verbinden**.
 
4. Wenn die IP-Adresse des Clients keinen Zugriff auf den logischen SQL-Datenbankserver hat, werden Sie aufgefordert, sich an einem Azure-Konto anzumelden und eine Firewallregel auf Serverebene zu erstellen. Wenn Sie ein Administrator des Azure-Abonnements sind, können Sie auf **Anmelden** klicken, um eine Firewall auf Serverebene zu erstellen. Wenn dies nicht der Fall ist, lassen Sie einen Azure-Administrator eine Firewallregel auf Serverebene erstellen.
 
      ![SQL Server Management Studio: Verbinden mit einem SQL-Datenbankserver](./media/sql-database-sql-server-management-studio-connect-server-principal/connect-server-principal-2.png)
 
1. Wenn Sie ein Administrator des Azure-Abonnements sind und sich auf der angezeigten Anmeldeseite anmelden müssen, geben Sie die Anmeldeinformationen für Ihr Abonnement an, und melden Sie sich an.

      ![Anmelden](./media/sql-database-sql-server-management-studio-connect-server-principal/connect-server-principal-3.png)
 
1. Überprüfen Sie nach der erfolgreichen Anmeldung bei Azure die vorgeschlagene Firewallregel auf Serverebene (Sie können sie ändern, um einen IP-Adressbereich zuzulassen). Klicken Sie anschließend auf **OK**, um die Firewallregel zu erstellen und die Verbindung mit der SQL-Datenbank herzustellen.
 
      ![Neue Firewall auf Serverebene](./media/sql-database-sql-server-management-studio-connect-server-principal/connect-server-principal-4.png)
 
5. Wenn Sie mit Ihren Anmeldeinformationen Zugriff haben, wird der Objekt-Explorer geöffnet, und Sie können Verwaltungsaufgaben durchführen oder Daten abfragen.
 
     ![Neue Firewall auf Serverebene](./media/sql-database-sql-server-management-studio-connect-server-principal/connect-server-principal-5.png)
 
     
 ## Durchführen der Problembehandlung für Verbindungsfehler

Die häufigste Ursache für Verbindungsfehler sind Unregelmäßigkeiten beim Servernamen (Erinnerung: <*Servername*> ist der Name des logischen Servers, nicht der Datenbank), Benutzernamen oder Kennwort sowie das Verbieten der Verbindung durch den Server aus Sicherheitsgründen.

<!---HONumber=AcomDC_0420_2016-->