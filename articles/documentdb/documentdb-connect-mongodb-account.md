---
title: "MongoDB-Verbindungszeichenfolge für DocumentDB-Konto | Microsoft Docs"
description: "Erfahren Sie, wie Sie Ihre MongoDB-App mithilfe einer MongoDB-Verbindungszeichenfolge zu einem Azure DocumentDB-Konto hinzufügen."
keywords: MongoDB-Verbindungszeichenfolge
services: documentdb
author: AndrewHoh
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: e36f7375-9329-403b-afd1-4ab49894f75e
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: anhoh
translationtype: Human Translation
ms.sourcegitcommit: a087df444c5c88ee1dbcf8eb18abf883549a9024
ms.openlocfilehash: 6d376b9db07a46c467bd772959de3f7acf9c123d
ms.lasthandoff: 03/15/2017


---

# <a name="connect-an-application-to-documentdb-api-for-mongodb"></a>Verbinden einer Anwendung mit DocumentDB: API für MongoDB
Erfahren Sie, wie Sie Ihre MongoDB-App mithilfe einer MongoDB-Verbindungszeichenfolge zu einem Azure DocumentDB-Konto hinzufügen. Indem Sie Ihre MongoDB-App mit einer Azure DocumentDB-Datenbank verbinden, können Sie die DocumentDB-Datenbank als Datenspeicher für die MongoDB-App verwenden. 

Dieses Tutorial zeigt zwei Möglichkeiten, um Informationen zur Verbindungszeichenfolge abzurufen:

- [Die Schnellstartmethode](#QuickstartConnection) zur Verwendung mit .NET-, Node.js-, MongoDB Shell-, Java- und Python-Treibern.
- [Benutzerdefinierte Verbindungszeichenfolgen](#GetCustomConnection) zur Verwendung mit anderen Treibern.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto. Wenn Sie noch kein Azure-Konto besitzen, erstellen Sie jetzt ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/). 
- Ein DocumentDB-Konto. Anweisungen finden Sie unter [Erstellen eines DocumentDB-Kontos zur Verwendung mit MongoDB-Apps](documentdb-create-mongodb-account.md).

## <a id="QuickstartConnection"></a>Abrufen der MongoDB-Verbindungszeichenfolge mithilfe der Schnellstartmethode
1. Melden Sie sich in einem Internetbrowser beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie auf dem Blatt **NoSQL (DocumentDB)** das DocumentDB: API für MongoDB-Konto aus. 
3. Klicken Sie auf der **linken Navigationsleiste** des Blatts „Konto“ auf **Schnellstart**. 
4. Wählen Sie Ihre Plattform aus (*.NET-Treiber*, *Node.js-Treiber*, *MongoDB Shell*, *Java-Treiber*, *Python-Treiber*). Wenn der gewünschte Treiber oder das gewünschte Tool nicht aufgeführt wird, machen Sie sich keine Sorgen: Wir stellen kontinuierlich weitere Codeausschnitte für Verbindungen zur Verfügung. Teilen Sie uns in einem Kommentar unten mit, welche Informationen Sie benötigen. Unter [Abrufen der Informationen zur Verbindungszeichenfolge für das Konto](#GetCustomConnection), erfahren Sie, wie Sie die Verbindung selbst erstellen können.
5. Kopieren Sie den Codeausschnitt, und fügen Sie ihn in Ihre MongoDB-App ein – und schon kann es losgehen.

    ![Screenshot des Blatts „Schnellstart“](./media/documentdb-connect-mongodb-account/QuickStartBlade.png)

## <a id="GetCustomConnection"></a> Abrufen der MongoDB-Verbindungszeichenfolge zum Anpassen
1. Melden Sie sich in einem Internetbrowser beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie auf dem Blatt **NoSQL (DocumentDB)** das DocumentDB: API für MongoDB-Konto aus. 
3. Klicken Sie auf der **linken Navigationsleiste** des Blatts „Konto“ auf **Verbindungszeichenfolge**. 
4. Das Blatt **Informationen zur Verbindungszeichenfolge** wird geöffnet. Es enthält alle erforderlichen Informationen, um mithilfe eines Treibers für MongoDB eine Verbindung mit dem Konto herzustellen, einschließlich einer vorab erstellten Verbindungszeichenfolge.

    ![Screenshot des Blatts „Verbindungszeichenfolge“](./media/documentdb-connect-mongodb-account/ConnectionStringBlade.png)

## <a name="connection-string-requirements"></a>Anforderungen an die Verbindungszeichenfolge
> [!Important]
> Für DocumentDB gelten strenge Sicherheitsanforderungen und -standards. DocumentDB-Konten erfordern eine Authentifizierung und eine sichere Kommunikation über **SSL**.
>
>

Beachten Sie, dass DocumentDB das standardmäßige URI-Format für MongoDB-Verbindungszeichenfolgen unterstützt. Es gelten jedoch einige besondere Anforderungen: DocumentDB-Konten erfordern eine Authentifizierung und eine sichere Kommunikation über SSL.  Folglich sieht das Format der Verbindungszeichenfolge wie folgt aus:

    mongodb://username:password@host:port/[database]?ssl=true

Die Werte dieser Zeichenfolge sind auf dem oben gezeigten Blatt „Verbindungszeichenfolge“ verfügbar.

* Benutzername (erforderlich)
  * Name des DocumentDB-Kontos
* Kennwort (erforderlich)
  * Kennwort des DocumentDB-Kontos
* Host (erforderlich)
  * FQDN des DocumentDB-Kontos
* Port (erforderlich)
  * 10250
* Datenbank (optional)
  * Die Standarddatenbank, die von der Verbindung verwendet wird (wenn keine Datenbank angegeben wird, lautet die Standarddatenbank „test“)
* ssl=true (erforderlich)

Betrachten Sie beispielsweise das Konto in den Informationen zur Verbindungszeichenfolge weiter oben.  Eine gültige Verbindungszeichenfolge ist:

    mongodb://contoso123:0Fc3IolnL12312asdfawejunASDF@asdfYXX2t8a97kghVcUzcDv98hawelufhawefafnoQRGwNj2nMPL1Y9qsIr9Srdw==@anhohmongo.documents.azure.com:10250/mydatabase?ssl=true

## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie, wie Sie mit einem DocumentDB: API für MongoDB-Konto [MongoChef verwenden](documentdb-mongodb-mongochef.md).
* Weitere Informationen über DocumentDB: API für MongoDB finden Sie unter [Beispiele](documentdb-mongodb-samples.md).

