---
title: "MongoDB-Verbindungszeichenfolge für Azure Cosmos DB-Konto | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Ihre MongoDB-App mithilfe einer MongoDB-Verbindungszeichenfolge zu einem Azure Cosmos DB-Konto hinzufügen."
keywords: MongoDB-Verbindungszeichenfolge
services: cosmosdb
author: AndrewHoh
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: e36f7375-9329-403b-afd1-4ab49894f75e
ms.service: cosmosdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: anhoh
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 95fe71c87a3f70174b59cc866d7d399b7e91720c
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---

# <a name="connect-a-mongodb-application-to-azure-cosmos-db"></a>Verbinden einer MongoDB-Anwendung mit Azure Cosmos DB
Erfahren Sie, wie Sie Ihre MongoDB-App mithilfe einer MongoDB-Verbindungszeichenfolge zu einem Azure Cosmos DB-Konto hinzufügen. Indem Sie Ihre MongoDB-App mit einer Azure Cosmos DB-Datenbank verbinden, können Sie die Azure Cosmos DB-Datenbank als Datenspeicher für die MongoDB-App verwenden. 

Dieses Tutorial zeigt zwei Möglichkeiten, um Informationen zur Verbindungszeichenfolge abzurufen:

- [Die Schnellstartmethode](#QuickstartConnection) zur Verwendung mit .NET-, Node.js-, MongoDB Shell-, Java- und Python-Treibern.
- [Benutzerdefinierte Verbindungszeichenfolgen](#GetCustomConnection) zur Verwendung mit anderen Treibern.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto. Wenn Sie noch kein Azure-Konto besitzen, erstellen Sie jetzt ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/). 
- Ein Azure Cosmos DB-Konto. Anweisungen finden Sie unter [Erstellen eines Azure Cosmos DB-Kontos zur Verwendung mit MongoDB-Apps](documentdb-create-mongodb-account.md).

## <a id="QuickstartConnection"></a>Abrufen der MongoDB-Verbindungszeichenfolge mithilfe der Schnellstartmethode
1. Melden Sie sich in einem Internetbrowser beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie auf dem Blatt **Azure Cosmos DB** das MongoDB-API-Konto aus. 
3. Klicken Sie auf der **linken Navigationsleiste** des Blatts „Konto“ auf **Schnellstart**. 
4. Wählen Sie Ihre Plattform aus (*.NET-Treiber*, *Node.js-Treiber*, *MongoDB Shell*, *Java-Treiber*, *Python-Treiber*). Wenn der gewünschte Treiber oder das gewünschte Tool nicht aufgeführt wird, machen Sie sich keine Sorgen: Wir stellen kontinuierlich weitere Codeausschnitte für Verbindungen zur Verfügung. Teilen Sie uns in einem Kommentar unten mit, welche Informationen Sie benötigen. Unter [Abrufen der Informationen zur Verbindungszeichenfolge für das Konto](#GetCustomConnection), erfahren Sie, wie Sie die Verbindung selbst erstellen können.
5. Kopieren Sie den Codeausschnitt, und fügen Sie ihn in Ihre MongoDB-App ein – und schon kann es losgehen.

    ![Screenshot des Blatts „Schnellstart“](./media/documentdb-connect-mongodb-account/QuickStartBlade.png)

## <a id="GetCustomConnection"></a> Abrufen der MongoDB-Verbindungszeichenfolge zum Anpassen
1. Melden Sie sich in einem Internetbrowser beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie auf dem Blatt **Azure Cosmos DB** das MongoDB-API-Konto aus. 
3. Klicken Sie auf der **linken Navigationsleiste** des Blatts „Konto“ auf **Verbindungszeichenfolge**. 
4. Das Blatt **Informationen zur Verbindungszeichenfolge** wird geöffnet. Es enthält alle erforderlichen Informationen, um mithilfe eines Treibers für MongoDB eine Verbindung mit dem Konto herzustellen, einschließlich einer vorab erstellten Verbindungszeichenfolge.

    ![Screenshot des Blatts „Verbindungszeichenfolge“](./media/documentdb-connect-mongodb-account/ConnectionStringBlade.png)

## <a name="connection-string-requirements"></a>Anforderungen an die Verbindungszeichenfolge
> [!Important]
> Für Azure Cosmos DB gelten strenge Sicherheitsanforderungen und -standards. Azure Cosmos DB-Konten erfordern eine Authentifizierung und eine sichere Kommunikation über **SSL**.
>
>

Beachten Sie, dass Azure Cosmos DB das standardmäßige URI-Format für MongoDB-Verbindungszeichenfolgen unterstützt. Es gelten jedoch einige besondere Anforderungen: Azure Cosmos DB-Konten erfordern eine Authentifizierung und eine sichere Kommunikation über SSL.  Folglich sieht das Format der Verbindungszeichenfolge wie folgt aus:

    mongodb://username:password@host:port/[database]?ssl=true

Die Werte dieser Zeichenfolge sind auf dem oben gezeigten Blatt „Verbindungszeichenfolge“ verfügbar.

* Benutzername (erforderlich)
  * Azure Cosmos DB-Kontoname
* Kennwort (erforderlich)
  * Azure Cosmos DB-Kontokennwort
* Host (erforderlich)
  * FQDN von Azure Cosmos DB-Konto
* Port (erforderlich)
  * 10250
* Datenbank (optional)
  * Die Standarddatenbank, die von der Verbindung verwendet wird (wenn keine Datenbank angegeben wird, lautet die Standarddatenbank „test“)
* ssl=true (erforderlich)

Betrachten Sie beispielsweise das Konto in den Informationen zur Verbindungszeichenfolge weiter oben.  Eine gültige Verbindungszeichenfolge ist:

    mongodb://contoso123:0Fc3IolnL12312asdfawejunASDF@asdfYXX2t8a97kghVcUzcDv98hawelufhawefafnoQRGwNj2nMPL1Y9qsIr9Srdw==@anhohmongo.documents.azure.com:10250/mydatabase?ssl=true

## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie, wie Sie Azure Cosmos DB mit [MongoChef verwenden](documentdb-mongodb-mongochef.md): API für MongoDB-Konto.
* Untersuchen von Azure Cosmos DB: [Beispiele](documentdb-mongodb-samples.md) für die API für MongoDB

