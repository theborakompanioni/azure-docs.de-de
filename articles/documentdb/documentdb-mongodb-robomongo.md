---
title: "Verwenden von Robomongo für Azure Cosmos DB | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Azure Cosmos DB mit Robomongo verwenden: API für MongoDB-Konto."
keywords: Robomongo
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
ms.date: 04/28/2017
ms.author: anhoh
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: bad2d57a6252bb30e4c8435c52da19c95304d8da
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
# <a name="use-robomongo-with-an-azure-cosmos-db-api-for-mongodb-account"></a>Verwenden von Robomongo mit Azure Cosmos DB: API für MongoDB-Konto
Zur Verbindung mit Azure Cosmos DB: API für MongoDB-Konto mithilfe von Robomongo müssen Sie Folgendes durchführen:

* Herunterladen und Installieren von [Robomongo](https://robomongo.org/)
* Über Informationen zur [Verbindungszeichenfolge](documentdb-connect-mongodb-account.md) für Azure Cosmos DB: API für MongoDB verfügen

## <a name="connect-using-robomongo"></a>Herstellen einer Verbindung mithilfe von Robomongo
Führen Sie die folgenden Schritte aus, um den MongoDB-Verbindungen von Robomongo Ihr Azure Cosmos DB: API für MongoDB-Konto hinzuzufügen.

1. Rufen Sie anhand der [hier](documentdb-connect-mongodb-account.md) bereitgestellten Anweisungen die Verbindungsinformationen für Ihr Azure Cosmos DB: API für MongoDB-Konto ab.

    ![Screenshot des Blatts „Verbindungszeichenfolge“](./media/documentdb-mongodb-robomongo/connectionstringblade.png)
2. Führen Sie *Robomongo.exe* aus.

3. Klicken Sie unter **Datei** auf die Verbindungsschaltfläche, um Ihre Verbindungen zu verwalten. Klicken Sie anschließend im Fenster mit den **MongoDB-Verbindungen** auf **Erstellen**. Daraufhin öffnet sich das Fenster mit den **Verbindungseinstellungen**.

4. Wählen Sie im Fenster mit den **Verbindungseinstellungen** einen Namen aus. Ermitteln Sie anschließend den **Host** und den **Port** in Ihren Verbindungsinformationen aus Schritt 1, und geben Sie sie unter **Adresse** bzw. **Port** ein.

    ![Screenshot der Verbindungsverwaltung von Robomongo](./media/documentdb-mongodb-robomongo/manageconnections.png)
5. Klicken Sie auf der Registerkarte **Authentifizierung** auf **Perform authentication** (Authentifizierung durchführen). Geben Sie anschließend die Datenbank (Standardeinstellung: *Admin*) sowie **Benutzername** und **Kennwort** ein.
Sowohl **Benutzername** als auch **Kennwort** finden Sie in den Verbindungsinformationen aus Schritt 1.

    ![Screenshot der Authentifizierungsregisterkarte von Robomongo](./media/documentdb-mongodb-robomongo/authentication.png)
6. Aktivieren Sie auf der Registerkarte **SSL** das Kontrollkästchen **Use SSL protocol** (SSL-Protokoll verwenden), und ändern Sie anschließend die **Authentifizierungsmethode** in **Selbstsigniertes Zertifikat**.

    ![Screenshot der SSL-Registerkarte von Robomongo](./media/documentdb-mongodb-robomongo/SSL.png)
7. Klicken Sie abschließend auf **Test**, um sich zu vergewissern, dass die Verbindung hergestellt werden kann, und klicken Sie schließlich auf **Speichern**.

## <a name="next-steps"></a>Nächste Schritte
* Untersuchen von Azure Cosmos DB: [Beispiele](documentdb-mongodb-samples.md) für die API für MongoDB

