---
redirect_url: https://docs.microsoft.com/azure/documentdb/documentdb-create-account
ROBOTS: NOINDEX, NOFOLLOW
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 4b2ae4a8d3b93b5c27e180a5875ae9e90483068a
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017



---

# <a name="create-an-azure-cosmos-db-account-with-mongodb-api"></a>Erstellen eines Azure Cosmos DB-Kontos mit der MongoDB-API
Azure Cosmos DB-Datenbanken können jetzt als Datenspeicher für Apps verwendet werden, die für MongoDB geschrieben wurden. Um diese Funktion verwenden zu können, benötigen Sie ein Azure-Konto und ein Azure Cosmos DB-Konto. Dieses Tutorial führt Sie durch die Erstellung eines Azure Cosmos DB-Kontos zur Verwendung mit MongoDB-Apps. 

Zum Erstellen eines Azure Cosmos DB-Kontos mit Unterstützung für MongoDB können Sie das Azure-Portal oder die Azure-Befehlszeilenschnittstelle mit Azure Resource Manager-Vorlagen verwenden. In diesem Artikel wird beschrieben, wie Sie ein Azure Cosmos DB-Konto mit Unterstützung für MongoDB im Azure-Portal erstellen. Informationen zum Erstellen eines Kontos mit der Azure-Befehlszeilenschnittstelle und Azure Resource Manager finden Sie unter [Automatisieren der Azure Cosmos DB-Kontoverwaltung mit Azure CLI 2.0](documentdb-automation-resource-manager-cli.md).

## <a name="prerequisite"></a>Voraussetzung
Ein Azure-Konto. Wenn Sie noch kein Azure-Konto besitzen, erstellen Sie jetzt ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/).
## <a name="create-an-azure-cosmos-db-account"></a>Erstellen eines Azure Cosmos DB-Kontos

1. Melden Sie sich in einem Internetbrowser beim [Azure-Portal](https://portal.azure.com) an.
2. Klicken Sie im linken Navigationsbereich auf **Azure Cosmos DB**.

    ![Screenshot des linken Navigationsbereichs im Portal mit hervorgehobenem Eintrag „NoSQL (DocumentDB)“](./media/documentdb-create-mongodb-account/portalleftnav.png)

3. Alternativ dazu klicken Sie auf **Weitere Dienste >**, geben in der oberen Suchleiste den Begriff **DocumentDB** ein und klicken auf **Azure Cosmos DB**.

    ![Screenshot des Blatts „Weitere Dienste“ zum Suchen nach dem Eintrag für NoSQL (DocumentDB)](./media/documentdb-create-mongodb-account/more-services-search.PNG)

4. Klicken Sie auf dem Blatt **Azure Cosmos DB** in der Aktionsleiste oben auf **+ Hinzufügen**.

    ![Screenshot der Schaltfläche „Hinzufügen“ auf dem Ressourcenblatt „Cosmos DB“](./media/documentdb-create-mongodb-account/add-documentdb-account.png)

5. Legen Sie auf dem Blatt **Azure Cosmos DB-Konto** die gewünschte Konfiguration für das Konto fest.

   ![Screenshot des Blatts für ein neues Azure Cosmos DB-Konto mit Protokollunterstützung für MongoDB](./media/documentdb-create-mongodb-account/create-documentdb-mongodb-account.PNG)

    - Geben Sie im Feld **ID** einen Namen zur Identifizierung des Kontos ein.  Nachdem die **ID** überprüft wurde, wird im Feld **ID** ein grünes Häkchen angezeigt. Der Wert von **ID** wird zum Hostnamen im URI. Die **ID** darf nur Kleinbuchstaben, Ziffern und den Bindestrich "-" enthalten und muss zwischen 3 und 50 Zeichen lang sein. Beachten Sie, dass *documents.azure.com* an den gewählten Namen des Endpunkts angehängt wird, um den Endpunkt des Kontos zu erstellen.

    - Als **API** wählen Sie **MongoDB** aus. Damit wird die Kommunikations-API angegeben, die Sie zur Interaktion mit Ihrer Azure Cosmos DB-Datenbank verwenden möchten.

    - Wählen Sie unter **Abonnement**das Azure-Abonnement aus, das Sie für das Konto verwenden möchten. Wenn Ihr Konto nur über ein Abonnement verfügt, wird dieses Konto standardmäßig ausgewählt.

    - Unter **Ressourcengruppe**können Sie eine Ressourcengruppe für das Konto auswählen oder erstellen.  Standardmäßig wird eine vorhandene Ressourcengruppe im Azure-Abonnement ausgewählt.  Sie können jedoch eine neue Ressourcengruppe erstellen, zu der Sie Ihr Konto hinzufügen möchten. Weitere Informationen finden Sie unter [Verwenden des Azure-Portals zum Verwalten Ihrer Azure-Ressourcen](../azure-portal/resource-group-portal.md).

    - Unter **Standort** können Sie einen geografischen Standort angeben, an dem das Konto gehostet wird.

6. Klicken Sie nach der Konfiguration der Optionen für das neue Konto auf **Erstellen**.  Die Erstellung des Kontos kann einige Minuten dauern.

   Sie können den Status auch über den Notification Hub überwachen.  

   ![Screenshot des Notification Hubs, in dem gerade das Azure Cosmos DB-Konto erstellt wird](./media/documentdb-create-mongodb-account/create-documentdb-mongodb-deployment-status.png)  

7. Klicken Sie zum Zugreifen auf Ihr neues Konto links im Menü auf **Azure Cosmos DB**. Klicken Sie in der Liste der regulären DocumentDB-Konten und DocumentDB-Konten mit Mongo-Protokollunterstützung auf den Namen Ihres neuen Kontos.
8. Ihr Azure Cosmos DB-Konto ist jetzt für den Einsatz mit Ihrer MongoDB-App bereit.

   ![Screenshot des Blatts für Standardkonten](./media/documentdb-create-mongodb-account/defaultaccountblade.png)

## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie, wie Sie eine [Verbindung](documentdb-connect-mongodb-account.md) mit einem DocumentDB-Konto mit Protokollunterstützung für MongoDB herstellen.

