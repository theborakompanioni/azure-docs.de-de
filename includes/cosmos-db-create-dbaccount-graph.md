1. Melden Sie sich in einem neuen Fenster beim [Azure-Portal](https://portal.azure.com/) an.
2. Klicken Sie im linken Bereich auf **Neu**, dann auf **Datenbanken** und anschließend auf **Azure Cosmos DB**.
   
   ![Bereich „Datenbanken“ im Azure-Portal](./media/cosmos-db-create-dbaccount-graph/create-nosql-db-databases-json-tutorial-1.png)

3. Legen Sie auf dem Blatt **Neues Konto** die gewünschte Konfiguration für das Azure Cosmos DB-Konto fest. 

    Mit Azure Cosmos DB können Sie zwischen vier Programmiermodellen wählen: Gremlin (Graph), MongoDB, SQL (DocumentDB) und Table (Schlüssel-Wert).  
       
    In diesem Schnellstartartikel programmieren Sie mit der Graph-API. Wählen Sie dazu **Gremlin (Graph)** beim Ausfüllen des Formulars aus. Beachten Sie, dass Azure Cosmos DB eine hoch verfügbare, weltweit verteilte Datenbank-Dienstplattform für Ihre unternehmenskritischen Anwendungen bereitstellen kann, wenn Sie über Dokumentdaten aus einer Katalog-App, Schlüssel-Wert-Daten (Tabelle) oder über aus einer MongoDB-App migrierte Daten verfügen.

    Füllen Sie die Felder im Blatt **Neues Konto** aus, und verwenden Sie die Informationen im folgenden Screenshot lediglich als Leitfaden. Da Ihre Werte nicht mit dem Screenshot übereinstimmen werden, müssen Sie beim Einrichten Ihres Kontos eindeutige Werte auswählen. 
 
    ![Das Blatt „Azure Cosmos DB“](./media/cosmos-db-create-dbaccount-graph/create-nosql-db-databases-json-tutorial-2.png)

    Einstellung|Empfohlener Wert|Beschreibung
    ---|---|---
    ID|*Eindeutiger Wert*|Ein eindeutiger Name, den Sie auswählen, um das Azure Cosmos DB-Konto zu identifizieren. Da *documents.azure.com* an die ID angefügt wird, die Sie bereitstellen, um Ihren URI zu erstellen, sollten Sie eine eindeutige, aber identifizierbare ID verwenden. Die ID darf nur Kleinbuchstaben, Zahlen und den Bindestrich (-) enthalten, und sie muss zwischen 3 und 50 Zeichen lang sein.
    API|Gremlin (Graph)|Weiter unten in diesem Artikel programmieren Sie mit der [Graph-API](../articles/cosmos-db/graph-introduction.md).|
    Abonnement|*Ihr Abonnement*|Das Azure-Abonnement, das Sie für das Azure Cosmos DB-Konto verwenden möchten. 
    Ressourcengruppe|*Derselbe Wert wie die ID*|Der neue Ressourcengruppenname für Ihr Konto. Der Einfachheit halber können Sie denselben Namen wie bei Ihrer ID verwenden. 
    Ort|*Die Region, die Ihren Benutzern am nächsten liegt*|Der geografische Standort, an dem Ihr Azure Cosmos DB-Konto gehostet werden soll. Wählen Sie den Speicherort, der Ihren Benutzern am nächsten liegt, um ihnen einen schnellen Zugriff auf die Daten zu gewähren.

4. Klicken Sie auf **Erstellen** , um das Konto zu erstellen.
5. Klicken Sie in der Symbolleiste auf **Benachrichtigungen**, um den Bereitstellungsprozess zu überwachen.

    ![Benachrichtigung „Bereitstellung gestartet“](./media/cosmos-db-create-dbaccount-graph/azure-documentdb-nosql-notification.png)

6.  Wenn die Bereitstellung abgeschlossen wurde, öffnen Sie das neue Konto über die Kachel **Alle Ressourcen**. 

    ![DocumentDB-Konto auf der Kachel „Alle Ressourcen“](./media/cosmos-db-create-dbaccount-graph/azure-documentdb-all-resources.png)