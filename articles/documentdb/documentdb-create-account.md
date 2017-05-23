---
title: Erstellen eines Azure Cosmos DB-Kontos | Microsoft-Dokumentation
description: Erstellen Sie eine NoSQL-Datenbank mit Azure Cosmos DB. Folgen Sie diesen Anweisungen, um ein Azure Cosmos DB-Konto zu erstellen und mit der Erstellung einer extrem schnellen, globalen NoSQL-Datenbank zu beginnen.
keywords: Datenbank erstellen
services: cosmosdb
documentationcenter: 
author: mimig1
manager: jhubbard
editor: monicar
ms.assetid: 0e7f8488-7bb7-463e-b6fd-3ae91a02c03a
ms.service: cosmosdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/17/2017
ms.author: mimig
redirect_url: https://aka.ms/acdbnetqa
ROBOTS: NOINDEX, NOFOLLOW
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 13621d942f2880f4dd1523315f43099eca2607d8
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
# <a name="how-to-create-an-azure-cosmos-db-nosql-account-using-the-azure-portal"></a>Erstellen eines Azure Cosmos DB-NoSQL-Kontos im Azure-Portal
> [!div class="op_single_selector"]
> * [Azure-Portal](documentdb-create-account.md)
> * [Azure CLI 1.0](documentdb-automation-resource-manager-cli-nodejs.md)
> * [Azure CLI 2.0](documentdb-automation-resource-manager-cli.md)
> * [Azure PowerShell](documentdb-manage-account-with-powershell.md)

Zum Erstellen einer Datenbank mit Microsoft Azure Cosmos DB ist Folgendes erforderlich:

* Ein Azure-Konto. Sie können ein [kostenloses Azure-Konto](https://azure.microsoft.com/free) einrichten, falls Sie noch kein Konto haben.
* Erstellung eines Azure Cosmos DB-Kontos.  

Zum Erstellen eines Azure Cosmos DB-Kontos können Sie das Azure-Portal, die Azure Resource Manager-Vorlagen oder die Azure-CLI (Befehlszeilenschnittstelle) verwenden. In diesem Artikel wird beschrieben, wie Sie ein Azure Cosmos DB-Konto im Azure-Portal erstellen. Informationen zum Erstellen eines Kontos mit Azure Resource Manager oder der Azure-CLI finden Sie unter [Automatisieren der Erstellung von Azure Cosmos DB-Datenbankkonten](documentdb-automation-resource-manager-cli.md).

Sie kennen Azure Cosmos DB noch nicht? In [diesem](https://azure.microsoft.com/documentation/videos/create-documentdb-on-azure/) vierminütigen Video von Scott Hanselman wird die Durchführung der gängigsten Aufgaben im Onlineportal erläutert.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Klicken Sie im Navigationsbereich auf der linken Seite auf **Neu**, **Datenbanken** und dann auf **Azure Cosmos DB**.

   ![Screenshot des Azure-Portals mit Hervorhebung von „Weitere Dienste“ und „NoSQL“ (Azure Cosmos DB)](./media/documentdb-create-account/create-nosql-db-databases-json-tutorial-1.png)  
3. Legen Sie auf dem Blatt **Neues Konto** die gewünschte Konfiguration für das Azure Cosmos DB-Konto fest.

    ![Screenshot des Blatts „Neue Azure Cosmos DB“](./media/documentdb-create-account/create-nosql-db-databases-json-tutorial-2.png)

   * Geben Sie im Feld **ID** einen Namen zur Identifizierung des Azure Cosmos DB-Kontos ein.  Nachdem die **ID** überprüft wurde, wird im Feld **ID** ein grünes Häkchen angezeigt. Der Wert von **ID** wird zum Hostnamen im URI. Die **ID** darf nur Kleinbuchstaben, Ziffern und den Bindestrich "-" enthalten und muss zwischen 3 und 50 Zeichen lang sein. Beachten Sie, dass *documents.azure.com* an den von Ihnen ausgewählten Endpunktnamen angehängt wird. Der Name mit diesem Zusatz wird zu Ihrem Azure Cosmos DB-Kontoendpunkt.
   * Wählen Sie im Feld **NoSQL-API** das gewünschte Programmiermodell aus:

     * **DocumentDB**: Die DocumentDB-API steht über .NET, Java, Node.js, Python und JavaScript [SDKs](documentdb-sdk-dotnet.md) sowie HTTP [REST](https://msdn.microsoft.com/library/azure/dn781481.aspx) zur Verfügung und bietet programmgesteuerten Zugriff auf alle DocumentDB-Funktionen.
     * **MongoDB**: DocumentDB bietet auch [Unterstützung auf Protokollebene](documentdb-protocol-mongodb.md) für **MongoDB**-APIs. Wenn Sie die Option für die MongoDB-API auswählen, können Sie vorhandene MongoDB SDKs und - [Tools](documentdb-mongodb-mongochef.md) zur Kommunikation mit DocumentDB verwenden. Sie können Ihre vorhandenen MongoDB-Apps [ohne Änderungen am Code](documentdb-connect-mongodb-account.md) zur Verwendung von DocumentDB [verschieben](documentdb-import-data.md) und von den Vorteilen einer vollständig verwalteten Datenbank als Dienst (Database-as-a-Service) mit unbegrenzter Skalierung, globaler Replikation und anderen Funktionen profitieren.
   * Wählen Sie unter **Abonnement** das Azure-Abonnement aus, das Sie mit dem Azure Cosmos DB-Konto verwenden möchten. Wenn Ihr Konto nur über ein Abonnement verfügt, wird standardmäßig dieses Konto ausgewählt.
   * Unter **Ressourcengruppe** können Sie eine Ressourcengruppe für Ihr Azure Cosmos DB-Konto auswählen oder erstellen.  Standardmäßig wird eine neue Ressourcengruppe erstellt. Weitere Informationen finden Sie unter [Verwalten von Azure-Ressourcen über das Portal](../azure-portal/resource-group-portal.md).
   * Unter **Standort** können Sie einen geografischen Standort angeben, an dem Ihr Azure Cosmos DB-Konto gehostet wird.
4. Klicken Sie nach der Konfiguration der Optionen für das neue Azure Cosmos DB-Konto auf **Erstellen**. Der Status der Bereitstellung kann im Benachrichtigungs-Hub überprüft werden.  

   ![Datenbanken schnell erstellen – Screenshot des Benachrichtigungs-Hubs, in dem die Erstellung des Azure Cosmos DB-Kontos angezeigt wird](./media/documentdb-create-account/create-nosql-db-databases-json-tutorial-4.png)  

   ![Screenshot des Benachrichtigungs-Hubs, in dem die erfolgreiche Erstellung des Azure Cosmos DB-Kontos und die Bereitstellung in einer Ressourcengruppe bestätigt werden – Benachrichtigung für Onlinedatenbankersteller](./media/documentdb-create-account/create-nosql-db-databases-json-tutorial-5.png)
5. Nachdem das Azure Cosmos DB-Konto erstellt wurde, steht es mit den Standardeinstellungen zur Verfügung. Die Standardkonsistenz des Azure Cosmos DB-Kontos ist auf **Sitzung** festgelegt.  Sie können die Standardkonsistenz anpassen, indem Sie im Ressourcenmenü auf **Standardkonsistenz** klicken. Weitere Informationen zu den Konsistenzebenen von Azure Cosmos DB finden Sie unter [Konsistenzebenen in Azure Cosmos DB](documentdb-consistency-levels.md).

   ![Screenshot des Blatts „Ressourcengruppe“ – Anwendungsentwicklung beginnen](./media/documentdb-create-account/create-nosql-db-databases-json-tutorial-6.png)  

   ![Screenshot des Blatts „Konsistenzebene“ – Sitzungskonsistenz](./media/documentdb-create-account/create-nosql-db-databases-json-tutorial-7.png)  

[How to: Create an Azure Cosmos DB account]: #Howto
[Next steps]: #NextSteps


## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie nun über ein Azure Cosmos DB-Konto verfügen, ist der nächste Schritt die Erstellung einer Azure Cosmos DB-Sammlung und -Datenbank.

Zum Erstellen einer neuen Sammlung und Datenbank stehen Ihnen die folgenden Ressourcen zur Verfügung:

* Das Azure-Portal, wie unter [Create an Azure Cosmos DB collection using the Azure portal](documentdb-create-collection.md) (Erstellen einer Azure Cosmos DB-Sammlung mit dem Azure-Portal) beschrieben.
* Die umfassenden Tutorials mit Beispieldaten: [.NET](documentdb-get-started.md), [.NET MVC](documentdb-dotnet-application.md), [Java](documentdb-java-application.md), [Node.js](documentdb-nodejs-application.md) oder [Python](documentdb-python-application.md).
* Beispielcode für [.NET](documentdb-dotnet-samples.md#database-examples), [Node.js](documentdb-nodejs-samples.md#database-examples) oder [Python](documentdb-python-samples.md#database-examples) in GitHub.
* SDKs vom Typ [.NET](documentdb-sdk-dotnet.md), [.NET Core](documentdb-sdk-dotnet-core.md), [Node.js](documentdb-sdk-node.md), [Java](documentdb-sdk-java.md), [Python](documentdb-sdk-python.md) und [REST](https://msdn.microsoft.com/library/azure/mt489072.aspx).

Nach dem Erstellen der Datenbank und Sammlung müssen Sie den Sammlungen [Dokumente hinzufügen](documentdb-view-json-document-explorer.md).

Nachdem Sie einer Sammlung Dokumente hinzugefügt haben, können Sie [DocumentDB-SQL](documentdb-sql-query.md) zum [Ausführen von Abfragen](documentdb-sql-query.md#ExecutingSqlQueries) für Ihre Dokumente verwenden. Sie können Abfragen mit dem [Abfrage-Explorer](documentdb-query-collections-query-explorer.md) im Portal, der [REST-API](https://msdn.microsoft.com/library/azure/dn781481.aspx) oder einem der [SDKs](documentdb-sdk-dotnet.md) ausführen.

### <a name="learn-more"></a>Weitere Informationen
Weitere Informationen zu Azure Cosmos DB finden Sie in den folgenden Ressourcen:

* [Einführung in Azure Cosmos DB](../cosmos-db/introduction.md)

