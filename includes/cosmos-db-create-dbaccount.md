1. Melden Sie sich in einem neuen Fenster beim [Azure-Portal](https://portal.azure.com/) an.
2. Klicken Sie im linken Menü auf **Neu**, dann auf **Datenbanken** und anschließend auf **Azure Cosmos DB**.
   
   ![Screenshot des Azure-Portals mit Hervorhebung von „Weitere Dienste“ und „Azure Cosmos DB“](./media/cosmos-db-create-dbaccount/create-nosql-db-databases-json-tutorial-1.png)

3. Legen Sie auf dem Blatt **Neues Konto** die gewünschte Konfiguration für das Azure Cosmos DB-Konto fest. 

    Mit Azure Cosmos DB können Sie zwischen vier Programmiermodellen wählen: Gremlin (Graph), MongoDB, SQL (DocumentDB) und Table (Schlüssel-Wert). 
    
    In diesem Schnellstart programmieren Sie mit der DocumentDB-API. Wählen Sie dazu **SQL (DocumentDB)** beim Ausfüllen des Formulars aus. Azure Cosmos DB stellt eine hoch verfügbare, global verteilte Datenbankdienst-Plattform für all Ihre unternehmenskritischen Anwendungen bereit. Dies kann für Sie besonders interessant sein, wenn Sie Graphdaten für eine Social Media-App, Schlüssel-/Wertdaten (Tabelle) oder Daten haben, die aus einer MongoDB-App stammen.

    Füllen Sie das Blatt „Neues Konto“ mithilfe der Informationen auf dem Screenshot aus. Sie wählen eindeutige Werte aus, da Sie Ihr Konto einrichten, sodass Ihre Werte mit dem Screenshot nicht genau übereinstimmen. 
 
    ![Screenshot des Blatts „Neue Azure Cosmos DB“](./media/cosmos-db-create-dbaccount/create-nosql-db-databases-json-tutorial-2.png)

    Einstellung|Empfohlener Wert|Beschreibung
    ---|---|---
    ID|*Eindeutiger Wert*|Ein eindeutiger Name, den Sie für die Identifikation Ihres Azure Cosmos DB-Kontos auswählen. *documents.azure.com* wird an die ID angehängt, die Sie bereitstellen, um Ihren URI zu erstellen. Verwenden Sie deshalb eine eindeutige und gleichzeitig identifizierbare ID. Die ID darf nur Kleinbuchstaben, Ziffern und den Bindestrich „-“ enthalten und muss zwischen 3 und 50 Zeichen lang sein.
    API|SQL (DocumentDB)|Weiter unten in diesem Artikel programmieren Sie mit der [DocumentDB-API](../articles/cosmos-db/documentdb-introduction.md).|
    Abonnement|*Ihr Abonnement*|Das Azure-Abonnement, das Sie für Ihr Azure Cosmos DB-Konto verwenden möchten. 
    Ressourcengruppe|*Derselbe Wert wie die ID*|Der neue Ressourcengruppenname für Ihr Konto. Der Einfachheit halber können Sie denselben Namen wie bei Ihrer ID verwenden. 
    Ort|*Die Region, die Ihren Benutzern am nächsten liegt*|Der geografische Standort, an dem Ihr Azure Cosmos DB-Konto gehostet werden soll. Wählen Sie den Speicherort, der Ihren Benutzern am nächsten liegt, um ihnen einen schnellen Zugriff auf die Daten zu gewähren.
4. Klicken Sie auf **Erstellen** , um das Konto zu erstellen.
5. Klicken Sie in der Symbolleiste oben auf **Benachrichtigungen**, um den Bereitstellungsprozess zu überwachen.

    ![Benachrichtigung „Bereitstellung gestartet“](./media/cosmos-db-create-dbaccount/notification.png)

6.  Wenn die Bereitstellung abgeschlossen wurde, öffnen Sie das neue Konto über die Kachel „Alle Ressourcen“. 

    ![DocumentDB-Konto auf der Kachel „Alle Ressourcen“](./media/cosmos-db-create-dbaccount/all-resources.png)