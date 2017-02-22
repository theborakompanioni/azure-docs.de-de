### <a name="create-a-new-logical-sql-server-in-the-azure-portal"></a>Erstellen eines neuen logischen SQL-Servers im Azure-Portal

1. Klicken Sie auf **Neu**, suchen Sie nach dem **logischen Server**, und drücken Sie dann die **EINGABETASTE**.

    ![Suchen nach dem logischen Server](./media/sql-data-warehouse-create-logical-server/search-logical-server.png)
2. Wählen Sie die Option **SQL Server (logischer Server)**. 

    ![Auswählen des logischen Servers](./media/sql-data-warehouse-create-logical-server/select-logical-server.png)
  
3. Klicken Sie auf **Erstellen**, um das neue Blatt „SQL Server (logischer Server) zu öffnen.

   <kbd> ![Öffnen des Blatts für den logischen Server](./media/sql-data-warehouse-create-logical-server/open-logical-server-blade.png) </kbd>
    <kbd>![Blatt für den logischen Server](./media/sql-data-warehouse-create-logical-server/logical-server-blade.png) </kbd>
  
3. Geben Sie auf dem Blatt „SQL Server (logischer Server)“ im Textfeld für den Servernamen einen gültigen Namen für den neuen logischen Server an. Wenn der angegebene Name gültig ist, wird ein grünes Häkchen angezeigt.
    
    ![Neuer Servername](./media/sql-data-warehouse-create-logical-server/new-name-logical-server.png)

    > [!IMPORTANT]
    > Der vollqualifizierte Name für den neuen Server lautet „<Ihr_Servername>.database.windows.net“.
    >
    
4. Geben Sie im Textfeld „Serveradministratoranmeldung“ einen Benutzernamen für die SQL-Authentifizierungsanmeldung für diesen Server ein. Diese Anmeldung wird als Serverprinzipalanmeldung bezeichnet. Wenn der angegebene Name gültig ist, wird ein grünes Häkchen angezeigt.
    
    ![SQL-Administratoranmeldung](./media/sql-data-warehouse-create-logical-server/sql-admin-login.png)
5. Geben Sie in den Textfeldern **Kennwort** und **Kennwort bestätigen** ein Kennwort für das Konto für die Serverprinzipalanmeldung an. Wenn das angegebene Kennwort gültig ist, wird ein grünes Häkchen angezeigt.
    
    ![SQL-Administratorkennwort](./media/sql-data-warehouse-create-logical-server/sql-admin-password.png)
6. Wählen Sie ein Abonnement aus, für das Sie über die Berechtigung zum Erstellen von Objekten verfügen.

    ![Abonnement](./media/sql-data-warehouse-create-logical-server/subscription.png)
7. Wählen Sie im Textfeld „Ressourcengruppe“ die Option **Neu erstellen**, und geben Sie dann im Textfeld „Ressourcengruppe“ einen gültigen Namen für die neue Ressourcengruppe ein. (Sie können auch eine vorhandene Ressourcengruppe verwenden, wenn Sie bereits eine erstellt haben.) Wenn der angegebene Name gültig ist, wird ein grünes Häkchen angezeigt.

    ![Neue Ressourcengruppe](./media/sql-data-warehouse-create-logical-server/new-resource-group.png)

8. Wählen Sie im Textfeld **Standort** ein geeignetes Rechenzentrum für Ihren Standort aus, z.B. „Australien, Osten“.
    
    ![Serverstandort](./media/sql-data-warehouse-create-logical-server/server-location.png)
    
    > [!TIP]
    > Das Kontrollkästchen **Azure-Diensten Zugriff auf den Server erlauben** kann auf diesem Blatt nicht geändert werden. Sie können diese Einstellung auf dem Blatt für die Serverfirewall ändern. Weitere Informationen finden Sie unter [Erste Schritte mit der Sicherheit](../articles/sql-database/sql-database-manage-servers-portal.md).
    >
    
9. Klicken Sie auf **Erstellen**.

    ![Schaltfläche erstellen](./media/sql-data-warehouse-create-logical-server/create.png)



<!--HONumber=Feb17_HO3-->


