<a id="log-in-to-the-azure-portal" class="xliff"></a>

## Anmelden beim Azure-Portal

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.

<a id="create-a-blank-sql-database-using-the-azure-portal" class="xliff"></a>

## Erstellen einer leeren SQL-Datenbank mithilfe des Azure-Portals

Eine Azure SQL-Datenbank wird mit einer definierten Gruppe von [Compute- und Speicherressourcen](../articles/sql-database/sql-database-service-tiers.md) erstellt. Die Datenbank wird in einer [Azure-Ressourcengruppe](../articles/azure-resource-manager/resource-group-overview.md) und auf einem [logischen Azure SQL-Datenbankserver](../articles/sql-database/sql-database-features.md) erstellt. 

Führen Sie die folgenden Schritte aus, um eine leere SQL-­Datenbank zu erstellen. 

1. Klicken Sie in der linken oberen Ecke des Azure-Portals auf die Schaltfläche **Neu**.

2. Wählen Sie auf der Seite **Neu** die Option **Datenbanken** und dann auf der Seite **Datenbanken** die Option **SQL-Datenbank**. 

   ![Leere Datenbank erstellen](../articles/sql-database/media/sql-database-design-first-database/create-empty-database.png)

3. Geben Sie die folgenden Informationen in das SQL-Datenbank-Formular ein, wie in der obigen Abbildung dargestellt:   

   | Einstellung | Empfohlener Wert | Beschreibung |
   | --------| --------------- | ----------- | 
   | **Datenbankname** | mySampleDatabase | Gültige Datenbanknamen finden Sie unter [Database Identifiers](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers) (Datenbankbezeichner). | 
   | **Abonnement** | Ihr Abonnement  | Ausführliche Informationen zu Ihren Abonnements finden Sie unter [Abonnements](https://account.windowsazure.com/Subscriptions). |
   | **Ressourcengruppe** | myResourceGroup | Gültige Ressourcengruppennamen finden Sie unter [Naming rules and restrictions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Benennungsregeln und Einschränkungen). |
   | **Quelle auswählen** | Leere Datenbank | Gibt an, dass eine leere Datenbank erstellt werden soll. |
   ||||

4. Klicken Sie auf **Server**, um einen neuen Server für Ihre neue Datenbank zu erstellen und zu konfigurieren. Füllen Sie das Formular **Neuer Server** mit den folgenden Informationen aus: 

   | Einstellung | Empfohlener Wert | Beschreibung |
   | --------| --------------- | ----------- | 
   | **Servername** | Ein global eindeutiger Name | Gültige Servernamen finden Sie unter [Naming rules and restrictions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Benennungsregeln und Einschränkungen). | 
   | **Serveradministratoranmeldung** | Ein gültiger Name | Gültige Anmeldenamen finden Sie unter [Database Identifiers](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers) (Datenbankbezeichner).|
   | **Kennwort** | Ein gültiges Kennwort | Ihr Kennwort muss mindestens acht Zeichen umfassen und Zeichen aus drei der folgenden Kategorien enthalten: Großbuchstaben, Kleinbuchstaben, Zahlen und nicht alphanumerische Zeichen. |
   | **Standort** | Ein gültiger Standort | Informationen zu Regionen finden Sie unter [Azure-Regionen](https://azure.microsoft.com/regions/). |
   ||||

   ![Erstellung des Datenbankservers](../articles/sql-database/media/sql-database-design-first-database/create-database-server.png)

5. Klicken Sie auf **Auswählen**.

6. Klicken Sie auf **Tarif**, um die Dienstebene und die Leistungsstufe für die neue Datenbank anzugeben. Wählen Sie für dieses Tutorial **20 DTUs** und **250** GB Speicher aus.

   ![Datenbankerstellung s1](../articles/sql-database/media/sql-database-design-first-database/create-empty-database-pricing-tier.png)

7. Klicken Sie auf **Anwenden**.  

8. Wählen Sie eine **Sortierung** für die leere Datenbank aus (in diesem Tutorial verwenden Sie den Standardwert). Weitere Informationen über Sortierungen finden Sie unter [Sortierungen](https://docs.microsoft.com/sql/t-sql/statements/collations).

9. Klicken Sie auf **Erstellen**, um die Datenbank bereitzustellen. Es dauert ungefähr eineinhalb Minuten, bis die Bereitstellung abgeschlossen ist. 

10. Klicken Sie in der Symbolleiste auf **Benachrichtigungen**, um den Bereitstellungsprozess zu überwachen.

   ![Benachrichtigung](../articles/sql-database/media/sql-database-get-started-portal/notification.png)

<a id="create-a-server-level-firewall-rule-using-the-azure-portal" class="xliff"></a>

## Erstellen einer Firewallregel auf Serverebene mit dem Azure-Portal

Der SQL-Datenbankdienst erstellt eine Firewall auf Serverebene. Zunächst verhindert die Firewall, dass externe Tools und Anwendungen eine Verbindung mit dem Server oder mit Datenbanken auf dem Server herstellen. Verbindungen werden zugelassen, nachdem eine Firewallregel für das Öffnen bestimmter IP-Adressen erstellt wurde. Führen Sie die hier angegebenen Schritte zum Erstellen einer [SQL-Datenbank-Firewallregel auf Serverebene](../articles/sql-database/sql-database-firewall-configure.md) für die IP-Adresse Ihres Clients und zum Zulassen der externen Konnektivität durch die SQL-Datenbankfirewall nur für Ihre IP-Adresse aus. 


> [!NOTE]
> Azure SQL-Datenbank kommuniziert über Port 1433. Sie können erst eine Verbindung mit der SQL-Datenbank herstellen, nachdem die Firewall Ihres Netzwerks ausgehenden Datenverkehr über Port 1433 zulässt.


1. Klicken Sie nach Abschluss der Bereitstellung im Menü auf der linken Seite auf **SQL-Datenbanken**, und klicken Sie dann auf der Seite **SQL-Datenbanken** auf **mySampleDatabase**. Die Übersichtsseite für Ihre Datenbank wird geöffnet, die den vollqualifizierten Servernamen (z.B. **mynewserver20170313.database.windows.net**) und Optionen für die weitere Konfiguration enthält. Kopieren Sie diesen vollqualifizierten Servernamen zur späteren Verwendung.

   > [!IMPORTANT]
   > Sie benötigen diesen vollqualifizierten Servernamen, um in den nachfolgenden Schnellstarts eine Verbindung mit Ihrem Server und den Datenbanken herzustellen.
   > 

   ![Servername](../articles/sql-database/media/sql-database-get-started-portal/server-name.png) 

2. Klicken Sie auf der Symbolleiste wie in der obigen Abbildung dargestellt auf **Serverfirewall festlegen**. Die Seite **Firewalleinstellungen** für den SQL-Datenbankserver wird geöffnet. 

   ![Serverfirewallregel](../articles/sql-database/media/sql-database-get-started-portal/server-firewall-rule.png) 


3. Klicken Sie in der Symbolleiste auf **Client-IP-Adresse hinzufügen**, um Ihre aktuelle IP-Adresse einer neuen Firewallregel hinzuzufügen. Eine Firewallregel kann Port 1433 für eine einzelne IP-Adresse oder einen Bereich von IP-Adressen öffnen.

4. Klicken Sie auf **Speichern**. Für Ihre aktuelle IP-Adresse wird eine Firewallregel auf Serverebene erstellt, und auf dem logischen Server wird Port 1433 geöffnet.

   ![Festlegen der Serverfirewallregel](../articles/sql-database/media/sql-database-get-started-portal/server-firewall-rule-set.png) 

4. Klicken Sie auf **OK**, und schließen Sie anschließend die Seite **Firewalleinstellungen**.

Sie können jetzt mithilfe eines Tools wie z.B. SQL Server Management Studio (SSMS) eine Verbindung mit dem Azure SQL-Datenbankserver und seinen Datenbanken herstellen. Die Verbindung wird unter Verwendung des zuvor erstellten Serveradministratorkontos mit dieser IP-Adresse hergestellt.


> [!IMPORTANT]
> Standardmäßig ist der Zugriff über die SQL-Datenbank-Firewall für alle Azure-Dienste aktiviert. Klicken Sie auf dieser Seite auf **AUS**, um dies für alle Azure-Dienste zu deaktivieren.


<a id="get-connection-string-values-using-the-azure-portal" class="xliff"></a>

## Abrufen von Verbindungszeichenfolgewerten über das Azure-Portal

Rufen Sie den vollqualifizierten Servernamen für Ihren Azure SQL-Datenbankserver im Azure-Portal ab. Sie verwenden den vollqualifizierten Servernamen, um mit SQL Server Management Studio eine Verbindung mit Ihrem Server herzustellen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.

2. Wählen Sie im Menü auf der linken Seite die Option **SQL-Datenbanken**, und klicken Sie auf der Seite **SQL-Datenbanken** auf Ihre Datenbank. 

3. Suchen Sie im Azure-Portal auf der Seite für Ihre Datenbank unter **Zusammenfassung** nach Ihrer Datenbank, und kopieren Sie den **Servernamen**.

   ![Verbindungsinformationen](../articles/sql-database/media/sql-database-get-started-portal/server-name.png) 
