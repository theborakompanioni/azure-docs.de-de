1. Melden Sie sich in einem neuen Fenster beim [Azure-Portal](https://portal.azure.com/) an.
2. Klicken Sie im linken Bereich auf **Neu**, dann auf **Datenbanken** und anschließend unter **Azure Cosmos DB** auf **Erstellen**.
   
   ![Die Bereich „Datenbanken“ im Azure-Portal](./media/cosmos-db-create-dbaccount/create-nosql-db-databases-json-tutorial-1.png)

3. Geben Sie auf dem Blatt **Neues Konto** die gewünschte Konfiguration für dieses Azure Cosmos DB-Konto an. 

    Mit Azure Cosmos DB können Sie zwischen vier Programmiermodellen wählen: Gremlin (Graph), MongoDB, SQL (DocumentDB) und Table (Schlüssel-Wert). Für jedes Modell ist derzeit ein separates Konto erforderlich.
    
    In diesem Schnellstartartikel programmieren Sie mit der DocumentDB-API. Wählen Sie dazu **SQL (DocumentDB)** beim Ausfüllen des Formulars aus. Azure Cosmos DB stellt eine hoch verfügbare, global verteilte Datenbankdienst-Plattform für all Ihre unternehmenskritischen Anwendungen bereit. Dies kann für Sie besonders interessant sein, wenn Sie Graphdaten für eine Social Media-App, Schlüssel-Wert-Daten (Tabelle) oder Daten haben, die aus einer MongoDB-App stammen.

    Füllen Sie die Felder auf dem Blatt **Neues Konto** mithilfe der Informationen auf dem folgenden Screenshot aus. Ihre Werte können sich von den Werten auf dem Screenshot unterscheiden.
 
    ![Das Blatt „Neues Konto“ für Azure Cosmos DB](./media/cosmos-db-create-dbaccount/create-nosql-db-databases-json-tutorial-2.png)

    Einstellung|Empfohlener Wert|Beschreibung
    ---|---|---
    ID|*Eindeutiger Wert*|Ein eindeutiger Name, der das Azure Cosmos DB-Konto identifiziert. Da *documents.azure.com* an die ID angefügt wird, die Sie bereitstellen, um Ihren URI zu erstellen, sollten Sie eine eindeutige, aber identifizierbare ID verwenden. Die ID darf nur Kleinbuchstaben, Zahlen und den Bindestrich (-) enthalten, und sie muss zwischen 3 und 50 Zeichen lang sein.
    API|SQL (DocumentDB)|Weiter unten in diesem Artikel programmieren Sie mit der [DocumentDB-API](../articles/documentdb/documentdb-introduction.md).|
    Abonnement|*Ihr Abonnement*|Das Azure-Abonnement, das Sie für das Azure Cosmos DB-Konto verwenden möchten. 
    Ressourcengruppe|*Derselbe Wert wie die ID*|Der neue Ressourcengruppenname für Ihr Konto. Der Einfachheit halber können Sie denselben Namen wie bei Ihrer ID verwenden. 
    Ort|*Die Region, die Ihren Benutzern am nächsten liegt*|Der geografische Standort, an dem Ihr Azure Cosmos DB-Konto gehostet werden soll. Wählen Sie den Speicherort, der Ihren Benutzern am nächsten liegt, um ihnen einen schnellen Zugriff auf die Daten zu gewähren.
4. Klicken Sie auf **Erstellen** , um das Konto zu erstellen.
5. Klicken Sie auf der Symbolleiste oben auf das Symbol **Benachrichtigungen** ![Benachrichtigungssymbol](./media/cosmos-db-create-dbaccount/notification-icon.png), um den Bereitstellungsprozess zu überwachen.

    ![Der Bereich „Benachrichtigungen“ im Azure-Portal](./media/cosmos-db-create-dbaccount-graph/azure-documentdb-nosql-notification.png)

6.  Wenn im Fenster „Benachrichtigungen“ angezeigt wird, dass die Bereitstellung erfolgreich war, schließen Sie das Benachrichtigungsfenster, und öffnen Sie das neue Konto über die Kachel **Alle Ressourcen** auf dem Dashboard. 

    ![Das DocumentDB-Konto auf der Kachel „Alle Ressourcen“](./media/cosmos-db-create-dbaccount/all-resources.png)
 
