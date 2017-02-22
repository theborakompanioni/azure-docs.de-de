<!--
includes/sql-database-include-connection-string-20-portalshots.md

Latest Freshness check:  2015-09-02 , GeneMi.

## Connection string
-->


### <a name="obtain-the-connection-string-from-the-azure-portal"></a>Abrufen der Verbindungszeichenfolge aus dem Azure-Portal
Nutzen Sie das [Azure-Portal](https://portal.azure.com/) zum Abrufen der Verbindungszeichenfolge, die für die Interaktion des Clientprogramms mit Azure SQL-Datenbank benötigt wird:

1. Klicken Sie auf **DURCHSUCHEN** > **SQL-Datenbanken**.
   
    ![SQL auswählen][1-select-sql]
2. Geben Sie in das Textfeld "Filter" nahe der oberen linken Ecke des Blattes **SQL-Datenbanken** den Namen der Datenbank ein.
   
    ![Datenbank auswählen][2-select-database]
3. Klicken Sie auf die Zeile für die Datenbank.
4. Nachdem das Blatt für die Datenbank angezeigt wird, können Sie der visuellen Einfachheit halber auf die Standardsteuerelemente zum Minimieren klicken, um die Blätter auszublenden, die Sie zum Durchsuchen und Filtern verwendet haben.
5. Notieren Sie sich den Namen der **SQL-Datenbank** und den **Servernamen**.  Der Benutzername lautet yourusername@yourserver.
   
    ![Verbindungsdetails abrufen][3-get-connection-details]
6. Fügen Sie die Verbindungsdetails in den Code der Clientanwendung ein.  Ersetzen Sie {your_password_here} durch Ihr tatsächliches Kennwort.

<!--
Could not find a good link for PHP

For more information, see:<br/>[Connection Strings and Configuration Files](https://msdn.microsoft.com/library/ms378428.aspx).
-->


<!-- Image references. -->

[1-select-sql]: ./media/sql-database-include-connection-string-20-portalshots/connection-string-select-sql.png

[2-select-database]: ./media/sql-database-include-connection-string-20-portalshots/connection-string-select-database.PNG

[3-get-connection-details]: ./media/sql-database-include-connection-string-20-portalshots/connection-string-details.PNG


<!--
These three includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-connection-string-20-portalshots.md
includes/sql-database-include-connection-string-30-compare.md
includes/sql-database-include-connection-string-40-config.md
-->


<!--HONumber=Feb17_HO3-->


