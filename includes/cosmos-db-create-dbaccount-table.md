1. Melden Sie sich in einem neuen Fenster beim [Azure-Portal](https://portal.azure.com/) an.
2. Klicken Sie im linken Menü auf **Neu**, dann auf **Datenbanken** und anschließend unter **Azure Cosmos DB** auf **Erstellen**.
   
   ![Screenshot des Azure-Portals mit Hervorhebung von „Weitere Dienste“ und „Azure Cosmos DB“](./media/cosmos-db-create-dbaccount-table/create-nosql-db-databases-json-tutorial-1.png)

3. Legen Sie auf dem Blatt **Neues Konto** die gewünschte Konfiguration für das Azure Cosmos DB-Konto fest. 

    Mit Azure Cosmos DB können Sie zwischen vier Programmiermodellen wählen: Gremlin (Graph), MongoDB, SQL (DocumentDB) und Table (Schlüssel-Wert). 
    
    In diesem Schnellstart werden wir mit der Table-API programmieren, sodass Sie **Tabelle (Schlüssel-Wert)** auswählen, wenn Sie das Formular ausfüllen. Beachten Sie, dass Azure Cosmos DB eine hoch verfügbare, weltweit verteilte Datenbank-Dienstplattform für Ihre unternehmenskritischen Anwendungen bereitstellen kann, wenn Sie über Diagrammdaten aus einer Social Media-App, Dokumentdaten aus einer Katalog-App oder aus einer MongoDB-App migrierte Daten verfügen.

    Füllen Sie das Blatt „Neues Konto“ mithilfe der Informationen im Screenshot aus. Sie wählen eindeutige Werte aus, da Sie Ihr Konto einrichten, sodass Ihre Werte mit dem Screenshot nicht genau übereinstimmen. 
 
    ![Screenshot des Blatts „Neue Azure Cosmos DB“](./media/cosmos-db-create-dbaccount-table/create-nosql-db-databases-json-tutorial-2.png)

    Einstellung|Empfohlener Wert|Beschreibung
    ---|---|---
    ID|*Eindeutiger Wert*|Ein eindeutiger Name, den Sie auswählen, um das Azure Cosmos DB-Konto zu identifizieren. *documents.azure.com* wird an die ID angefügt, die Sie bereitstellen, um Ihren URI zu erstellen, sodass Sie eine eindeutige aber identifizierbare ID verwenden sollten. Die ID darf nur Kleinbuchstaben, Ziffern und den Bindestrich „-“ enthalten und muss zwischen 3 und 50 Zeichen lang sein.
    API|Tabelle (Schlüssel-Wert)|Wir werden weiter unten in diesem Artikel mit der [Table-API](../articles/cosmos-db/table-introduction.md) programmieren.|
    Abonnement|*Ihr Abonnement*|Das Azure-Abonnement, das Sie für das Azure Cosmos DB-Konto verwenden möchten. 
    Ressourcengruppe|*Derselbe Wert wie die ID*|Der neue Ressourcengruppenname für Ihr Konto. Der Einfachheit halber können Sie denselben Namen wie bei Ihrer ID verwenden. 
    Ort|*Die Region, die Ihren Benutzern am nächsten liegt*|Der geografische Standort, an dem Ihr Azure Cosmos DB-Konto gehostet werden soll. Wählen Sie den Speicherort, der Ihren Benutzern am nächsten liegt, um ihnen einen schnellen Zugriff auf die Daten zu gewähren.   

4. Klicken Sie auf **Erstellen** , um das Konto zu erstellen.
5. Klicken Sie in der Symbolleiste auf **Benachrichtigungen**, um den Bereitstellungsprozess zu überwachen.

    ![Benachrichtigung „Bereitstellung gestartet“](./media/cosmos-db-create-dbaccount-table/notification.png)

6.  Wenn die Bereitstellung abgeschlossen wurde, öffnen Sie das neue Konto über die Kachel „Alle Ressourcen“. 

    ![Azure Cosmos DB-Konto auf der Kachel „Alle Ressourcen“](./media/cosmos-db-create-dbaccount-table/all-resources.png)
