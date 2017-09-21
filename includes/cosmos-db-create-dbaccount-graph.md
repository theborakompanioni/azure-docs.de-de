1. Melden Sie sich in einem neuen Fenster beim [Azure-Portal](https://portal.azure.com/) an.

2. Wählen Sie im linken Bereich die Optionen **Neu** > **Datenbanken** > **Azure Cosmos DB** > **Erstellen**.
   
   ![Bereich „Datenbanken“ im Azure-Portal](./media/cosmos-db-create-dbaccount-graph/create-nosql-db-databases-json-tutorial-1.png)

3. Geben Sie unter **Neues Konto** die gewünschte Konfiguration für dieses Azure Cosmos DB-Konto an. 

    Mit Azure Cosmos DB können Sie zwischen vier Programmiermodellen wählen: Gremlin (Graph), MongoDB, SQL (DocumentDB) und Table (Schlüssel-Wert). Für jedes Modell ist derzeit ein separates Konto erforderlich.
       
    In diesem Schnellstartartikel programmieren Sie mit der Graph-API. Wählen Sie beim Ausfüllen des Formulars hierfür **Gremlin (Graph)** aus. Beachten Sie, dass Azure Cosmos DB eine hoch verfügbare, weltweit verteilte Datenbank-Dienstplattform für Ihre unternehmenskritischen Anwendungen bereitstellen kann, wenn Sie über Dokumentdaten aus einer Katalog-App, Schlüssel-Wert-Daten (Tabelle) oder über aus einer MongoDB-App migrierte Daten verfügen.

    Füllen Sie die Felder auf dem Blatt **Neues Konto** mithilfe der Informationen im folgenden Screenshot aus. Es kann sein, dass sich Ihre Werte von den Werten im Screenshot unterscheiden.
 
    ![Blatt „Neues Konto“](./media/cosmos-db-create-dbaccount-graph/create-nosql-db-databases-json-tutorial-2.png)

    Einstellung|Empfohlener Wert|Beschreibung
    ---|---|---
    ID|*Eindeutiger Wert*|Ein eindeutiger Name, der das Azure Cosmos DB-Konto identifiziert. Da *documents.azure.com* an die ID angefügt wird, die Sie bereitstellen, um Ihren URI zu erstellen, sollten Sie eine eindeutige, aber identifizierbare ID verwenden. Die ID darf nur Kleinbuchstaben, Zahlen und den Bindestrich (-) enthalten. Sie muss zwischen drei und 50 Zeichen lang sein.
    API|Gremlin (Graph)|Weiter unten in diesem Artikel programmieren Sie mit der [Graph-API](../articles/cosmos-db/graph-introduction.md).|
    Abonnement|*Ihr Abonnement*|Das Azure-Abonnement, das Sie für das Azure Cosmos DB-Konto verwenden möchten. 
    Ressourcengruppe|*Derselbe Wert wie die ID*|Der neue Ressourcengruppenname für Ihr Konto. Der Einfachheit halber können Sie denselben Namen wie bei Ihrer ID verwenden. 
    Ort|*Die Region, die Ihren Benutzern am nächsten liegt*|Der geografische Standort, an dem Ihr Azure Cosmos DB-Konto gehostet werden soll. Wählen Sie den Speicherort, der Ihren Benutzern am nächsten liegt, um ihnen einen schnellen Zugriff auf die Daten zu gewähren.

4. Wählen Sie **Erstellen**, um das Konto zu erstellen.

5. Wählen Sie auf der Symbolleiste das Symbol **Benachrichtigungen** ![Benachrichtigungssymbol](./media/cosmos-db-create-dbaccount-graph/notification-icon.png), um den Bereitstellungsprozess zu überwachen.

    ![Bereich „Benachrichtigungen“ im Azure-Portal](./media/cosmos-db-create-dbaccount-graph/notification.png)

6. Wenn im Fenster **Benachrichtigungen** der Hinweis angezeigt wird, dass die Bereitstellung erfolgreich war, können Sie das Fenster schließen. Öffnen Sie das neue Konto im **Dashboard** auf der Kachel **Alle Ressourcen**. 

    ![Kachel „Alle Ressourcen“](./media/cosmos-db-create-dbaccount-graph/azure-documentdb-all-resources.png)