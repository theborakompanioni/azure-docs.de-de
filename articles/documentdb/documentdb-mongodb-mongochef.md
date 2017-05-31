---
title: "Verwenden von MongoChef für Azure Cosmos DB | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Azure Cosmos DB mit MongoChef verwenden: API für MongoDB-Konto."
keywords: MongoChef
services: cosmosdb
author: AndrewHoh
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: 352c5fb9-8772-4c5f-87ac-74885e63ecac
ms.service: cosmosdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2017
ms.author: anhoh
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 8363b73cfae818449e5a907a17e742bf97e5107a
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
# <a name="use-mongochef-with-an-azure-cosmos-db-api-for-mongodb-account"></a>Verwenden von Azure Cosmos DB mit MongoChef: API für MongoDB-Konto

Zur Verbindung mit Azure Cosmos DB: API für MongoDB-Konto, müssen Sie Folgendes durchführen:

* Herunterladen und Installieren von [MongoChef](http://3t.io/mongochef)
* Über Informationen zur [Verbindungszeichenfolge](documentdb-connect-mongodb-account.md) für Azure Cosmos DB: API für MongoDB verfügen

## <a name="create-the-connection-in-mongochef"></a>Erstellen der Verbindung in MongoChef
Führen Sie die folgenden Schritte aus, um dem Verbindungs-Manager von MongoChef Ihr Azure Cosmos DB: API für MongoDB-Konto hinzuzufügen.

1. Rufen Sie anhand der [hier](documentdb-connect-mongodb-account.md) bereitgestellten Anweisungen die Verbindungsinformationen für Ihr Azure Cosmos DB: API für MongoDB ab.

    ![Screenshot des Blatts „Verbindungszeichenfolge“](./media/documentdb-mongodb-mongochef/ConnectionStringBlade.png)
2. Klicken Sie auf **Connect** (Verbinden), um den Verbindungs-Manager zu öffnen, und klicken Sie anschließend auf **New Connection** (Neue Verbindung).

    ![Screenshot des Verbindungs-Managers von MongoChef](./media/documentdb-mongodb-mongochef/ConnectionManager.png)
3. Geben Sie im Fenster **Neue Verbindung** auf der Registerkarte **Server** den Host (FQDN) des Azure Cosmos DB: API für MongoDB sowie den Port ein.

    ![Screenshot der Registerkarte „Server“ des Verbindungs-Managers von MongoChef](./media/documentdb-mongodb-mongochef/ConnectionManagerServerTab.png)
4. Wählen Sie im Fenster **New Connection** (Neue Verbindung) auf der Registerkarte **Authentication** (Authentifizierung) den Authentifizierungsmodus **Standard (MONGODB-CR or SCARM-SHA-1)** (Standard [MONGODB-CR oder SCARM-SHA-1]) aus, und geben Sie Benutzername und Kennwort ein.  Übernehmen Sie den Standardwert für Authentifizierungs-DB („admin“), oder geben Sie einen eigenen Wert ein.

    ![Screenshot der Registerkarte „Authentication“ des Verbindungs-Managers von MongoChef](./media/documentdb-mongodb-mongochef/ConnectionManagerAuthenticationTab.png)
5. Aktivieren Sie im Fenster **New Connection** (Neue Verbindung) auf der Registerkarte **SSL** das Kontrollkästchen **Use SSL protocol to connect** (Verbindung über SSL-Protokoll herstellen) und das Optionsfeld **Accept server self-signed SSL certificates** (Selbstsignierte SSL-Serverzertifikate akzeptieren).

    ![Screenshot der Registerkarte „SSL“ des Verbindungs-Managers von MongoChef](./media/documentdb-mongodb-mongochef/ConnectionManagerSSLTab.png)
6. Klicken Sie auf die Schaltfläche **Test Connection** (Verbindung testen), um die Verbindungsinformationen zu überprüfen. Klicken Sie auf **OK**, um zum Fenster für neue Verbindungen zurückzukehren, und klicken Sie anschließend auf **Save** (Speichern).

    ![Screenshot des MongoChef-Fensters „Test Connection“](./media/documentdb-mongodb-mongochef/TestConnectionResults.png)

## <a name="use-mongochef-to-create-a-database-collection-and-documents"></a>Verwenden von MongoChef zum Erstellen von Datenbanken, Sammlungen und Dokumenten
Führen Sie die folgenden Schritte aus, um Datenbanken, Sammlungen und Dokumente mithilfe von MongoChef zu erstellen.

1. Markieren Sie die Verbindung im **Verbindungs-Manager**, und klicken Sie auf **Connect** (Verbinden).

    ![Screenshot des Verbindungs-Managers von MongoChef](./media/documentdb-mongodb-mongochef/ConnectToAccount.png)
2. Klicken Sie mit der rechten Maustaste auf den Host, und wählen Sie **Add Database**(Datenbank hinzufügen) aus.  Geben Sie einen Datenbanknamen an, und klicken Sie auf **OK**.

    ![Screenshot der MongoChef-Option „Add Database“](./media/documentdb-mongodb-mongochef/AddDatabase1.png)
3. Klicken Sie mit der rechten Maustaste auf die Datenbank, und wählen Sie **Add Collection**(Sammlung hinzufügen) aus.  Geben Sie einen Sammlungsnamen an, und klicken Sie auf **Create**(Erstellen).

    ![Screenshot der MongoChef-Option „Add Collection“](./media/documentdb-mongodb-mongochef/AddCollection.png)
4. Klicken Sie auf das Menüelement **Collection** (Sammlung) und anschließend auf **Add Document** (Dokument hinzufügen).

    ![Screenshot des MongoChef-Menüelements „Add Document“](./media/documentdb-mongodb-mongochef/AddDocument1.png)
5. Fügen Sie im Dialogfeld zum Hinzufügen von Dokumenten Folgendes ein, und klicken Sie dann auf **Add Document**(Dokument hinzufügen).

        {
        "_id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
               { "firstName": "Thomas" },
               { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "isRegistered": true
        }
6. Fügen Sie ein weiteres Dokument mit dem folgenden Inhalt hinzu.

        {
        "_id": "WakefieldFamily",
        "parents": [
            { "familyName": "Wakefield", "givenName": "Robin" },
            { "familyName": "Miller", "givenName": "Ben" }
        ],
        "children": [
            {
                "familyName": "Merriam",
                 "givenName": "Jesse",
                "gender": "female", "grade": 1,
                "pets": [
                    { "givenName": "Goofy" },
                    { "givenName": "Shadow" }
                ]
            },
            {
                "familyName": "Miller",
                 "givenName": "Lisa",
                 "gender": "female",
                 "grade": 8 }
        ],
        "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
        "isRegistered": false
        }
7. Führen Sie eine Beispielabfrage aus. Suchen Sie z.B. nach Familien mit dem Nachnamen „Andersen“, und geben Sie übergeordnete Felder und Felder mit Angaben zum Staat zurück.

    ![Screenshot der MongoChef-Abfrageergebnisse](./media/documentdb-mongodb-mongochef/QueryDocument1.png)

## <a name="next-steps"></a>Nächste Schritte
* Untersuchen von Azure Cosmos DB: [Beispiele](documentdb-mongodb-samples.md) für die API für MongoDB

