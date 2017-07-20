---
title: "Verwenden von „mongoimport“ und „mongorestore“ mit der Azure Cosmos DB-API für MongoDB | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie „mongoimport“ und „mongorestore“ zum Importieren von Daten in eine API für ein MongoDB-Konto verwenden."
keywords: mongoimport, mongorestore
services: cosmos-db
author: AndrewHoh
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: 352c5fb9-8772-4c5f-87ac-74885e63ecac
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2017
ms.author: anhoh
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: 138f04f8e9f0a9a4f71e43e73593b03386e7e5a9
ms.openlocfilehash: 3823cfc0696c917a66d41e9dbde4349f635a4011
ms.contentlocale: de-de
ms.lasthandoff: 06/29/2017

---

# <a name="azure-cosmos-db-import-mongodb-data"></a>Azure Cosmos DB: Importieren von MongoDB-Daten 

Sie müssen wie folgt vorgehen, um Daten von MongoDB in ein Azure Cosmos DB-Konto für die Verwendung mit der API für MongoDB zu migrieren:

* Laden Sie entweder *mongoimport.exe* oder *mongorestore.exe* aus dem [MongoDB Download Center](https://www.mongodb.com/download-center) herunter.
* Rufen Sie Ihre [API für die MongoDB-Verbindungszeichenfolge](connect-mongodb-account.md) ab.

Wenn Sie Daten aus MongoDB importieren und diese mit der DocumentDB-API verwenden möchten, sollten Sie die Daten mithilfe des [Datenmigrationstools](import-data.md) importieren.

Dieses Tutorial enthält die folgenden Aufgaben:

> [!div class="checklist"]
> * Abrufen der Verbindungszeichenfolge
> * Importieren von MongoDB-Daten mithilfe von „mongoimport“
> * Importieren von MongoDB-Daten mithilfe von „mongorestore“

## <a name="prerequisites"></a>Voraussetzungen

* Erhöhung des Durchsatzes: Die Dauer der Datenmigration richtet sich nach der Durchsatzmenge, die Sie für Ihre Sammlungen einrichten. Achten Sie darauf, dass Sie den Durchsatz für größere Datenmigrationen erhöhen. Nachdem die Migration abgeschlossen ist, können Sie den Durchsatz wieder verringern, um Kosten zu sparen. Weitere Informationen zur Erhöhung des Durchsatzes im [Azure-Portal](https://portal.azure.com) finden Sie unter [Leistungsstufen und Tarife in Azure Cosmos DB](performance-levels.md).

* SSL-Aktivierung: Für Azure Cosmos DB gelten strenge Sicherheitsanforderungen und -standards. Achten Sie darauf, SSL für die Interaktion mit Ihrem Konto zu aktivieren. Die Verfahren im restlichen Artikel verdeutlichen, wie Sie SSL für „mongoimport“ und „mongorestore“ aktivieren.

## <a name="find-your-connection-string-information-host-port-username-and-password"></a>Suchen nach den Informationen für die Verbindungszeichenfolge (Host, Port, Benutzername und Kennwort)

1. Klicken Sie im [Azure-Portal](https://portal.azure.com) auf der linken Seite auf den Eintrag **Azure Cosmos DB**.
2. Wählen Sie unter **Abonnements** den Namen Ihres Kontos aus.
3. Klicken Sie auf dem Blatt **Verbindungszeichenfolge** auf **Verbindungszeichenfolge**.  
Der rechte Bereich enthält alle Informationen, die Sie zum erfolgreichen Verbinden des Kontos benötigen.

    ![Blatt „Verbindungszeichenfolge“](./media/mongodb-migrate/ConnectionStringBlade.png)

## <a name="import-data-to-the-api-for-mongodb-by-using-mongoimport"></a>Importieren von Daten in die API für MongoDB mit „mongoimport“

Verwenden Sie zum Importieren von Daten in Ihr Azure Cosmos DB-Konto die folgende Vorlage. Geben Sie für *host*, *username* und *password* die Werte ein, die für Ihr Konto gelten.  

Vorlage:

    mongoimport.exe --host <your_hostname>:10255 -u <your_username> -p <your_password> --db <your_database> --collection <your_collection> --ssl --sslAllowInvalidCertificates --type json --file C:\sample.json

Beispiel:  

    mongoimport.exe --host anhoh-host.documents.azure.com:10255 -u anhoh-host -p tkvaVkp4Nnaoirnouenrgisuner2435qwefBH0z256Na24frio34LNQasfaefarfernoimczciqisAXw== --ssl --sslAllowInvalidCertificates --db sampleDB --collection sampleColl --type json --file C:\Users\anhoh\Desktop\*.json

## <a name="import-data-to-the-api-for-mongodb-by-using-mongorestore"></a>Importieren von Daten in die API für MongoDB mit „mongorestore“

Verwenden Sie zum Wiederherstellen von Daten für Ihre API für das MongoDB-Konto die folgende Vorlage, um den Importvorgang durchzuführen. Geben Sie für *host*, *username* und *password* die Werte ein, die für Ihr Konto gelten.

Vorlage:

    mongorestore.exe --host <your_hostname>:10255 -u <your_username> -p <your_password> --db <your_database> --collection <your_collection> --ssl --sslAllowInvalidCertificates <path_to_backup>

Beispiel:

    mongorestore.exe --host anhoh-host.documents.azure.com:10255 -u anhoh-host -p tkvaVkp4Nnaoirnouenrgisuner2435qwefBH0z256Na24frio34LNQasfaefarfernoimczciqisAXw== --ssl --sslAllowInvalidCertificates ./dumps/dump-2016-12-07
    
## <a name="guide-for-a-successful-migration"></a>Anleitung für eine erfolgreiche Migration

1. Erstellen und skalieren Sie Ihre Sammlungen im Voraus:
        
    * Standardmäßig stellt Azure Cosmos DB eine neue MongoDB-Sammlung mit 1.000 Anforderungseinheiten (RUs) bereit. Erstellen Sie vor der Migration mithilfe von „mongoimport“, „mongorestore“ oder „mongomirror“ alle Sammlungen aus dem [Azure-Portal](https://portal.azure.com) oder von MongoDB-Treibern und Tools. Wenn Ihre Sammlung größer als 10 GB ist, müssen Sie eine [Shard-/partitionierte Sammlung](partition-data.md) mit einem entsprechenden Shardschlüssel erstellen.

    * Über das [Azure-Portal](https://portal.azure.com) können Sie den Durchsatz Ihrer Sammlungen von 1.000 RUs für eine einzelne partitionierte Sammlung und 2.500 RUs für eine Shard-Sammlung speziell für die Migration erhöhen. Mit dem höheren Durchsatz können Sie Einschränkungen vermeiden und in kürzerer Zeit migrieren. Mit der stündlichen Abrechnung in Azure Cosmos DB können Sie den Durchsatz sofort nach der Migration verringern, um Kosten zu sparen.

2. Berechnen Sie die ungefähre RU-Gebühr für das Schreiben eines einzelnen Dokuments:

    a. Stellen Sie eine Verbindung zu Ihrer MongoDB-Datenbank von Azure Cosmos DB über die MongoDB-Shell her. Eine Anleitung dazu finden Sie unter [Connect a MongoDB application to Azure Cosmos DB (Verbinden einer MongoDB-Anwendung mit Azure Cosmos DB)](connect-mongodb-account.md).
    
    b. Führen Sie einen Beispieleinfügebefehl mit einem Ihrer Beispieldokumente aus der MongoDB-Shell aus:
    
        ```db.coll.insert({ "playerId": "a067ff", "hashedid": "bb0091", "countryCode": "hk" })```
        
    c. Führen Sie ```db.runCommand({getLastRequestStatistics: 1})``` aus, und Sie erhalten eine Antwort wie diese:
     
        ```
        globaldb:PRIMARY> db.runCommand({getLastRequestStatistics: 1})
        {
            "_t": "GetRequestStatisticsResponse",
            "ok": 1,
            "CommandName": "insert",
            "RequestCharge": 10,
            "RequestDurationInMilliSeconds": NumberLong(50)
        }
        ```
        
    d. Beachten Sie die Gebühr für die Anforderung.
    
3. Ermitteln Sie die Latenz von Ihrem Computer zum Azure Cosmos DB-Clouddienst:
    
    a. Aktivieren Sie die ausführliche Protokollierung über die MongoDB-Shell mit diesem Befehl: ```setVerboseShell(true)```
    
    b. Führen Sie eine einfache Abfrage für die Datenbank aus: ```db.coll.find().limit(1)```. Sie erhalten eine Antwort wie diese:

        ```
        Fetched 1 record(s) in 100(ms)
        ```
        
4. Entfernen Sie das eingefügte Dokument vor der Migration, damit keine doppelten Dokumente vorhanden sind. Mit diesem Befehl können Sie Dokumente entfernen: ```db.coll.remove({})```

5. Berechnen Sie die ungefähren Werte für *batchSize* und *numInsertionWorkers*:

    * Berechnen Sie *batchSize*, indem Sie die Gesamtzahl der bereitgestellten RUs durch die RUs teilen, die für das Schreiben des einzelnen Dokuments in Schritt 3 verbraucht wurden.
    
    * Wenn die berechnete *batchSize* <= 24 ist, verwenden Sie diese Zahl als Wert für *batchSize*.
    
    * Wenn die berechnete *batchSize* > 24 ist, legen Sie den Wert von *batchSize* auf 24 fest.
    
    * Berechnen Sie *numInsertionWorkers* mit dieser Formel:   *numInsertionWorkers = (bereitgestellter Durchsatz * Latenz in Sekunden) / (Batchgröße * Für einen einzelnen Schreibvorgang verbrauchte RUs)*.
        
    |Eigenschaft|Wert|
    |--------|-----|
    |batchSize| 24 |
    |Bereitgestellte RUs | 10000 |
    |Latenz | 0,100 s |
    |Für 1 Schreibvorgang berechnete RUs | 10 RUs |
    |numInsertionWorkers | ? |
    
    *numInsertionWorkers = (10.000 RUs x 0,1 s) / (24 x 10 RUs) = 4,1666*

6. Führen Sie den endgültigen Migrationsbefehl aus:

   ```
   mongoimport.exe --host anhoh-mongodb.documents.azure.com:10255 -u anhoh-mongodb -p wzRJCyjtLPNuhm53yTwaefawuiefhbauwebhfuabweifbiauweb2YVdl2ZFNZNv8IU89LqFVm5U0bw== --ssl --sslAllowInvalidCertificates --jsonArray --db dabasename --collection collectionName --file "C:\sample.json" --numInsertionWorkers 4 --batchSize 24
   ```

## <a name="next-steps"></a>Nächste Schritte

Sie können mit dem nächsten Tutorial fortfahren und sich darüber informieren, wie Sie MongoDB-Daten mithilfe von Azure Cosmos DB abfragen können. 

> [!div class="nextstepaction"]
>[Abfragen von MongoDB-Daten](../cosmos-db/tutorial-query-mongodb.md)

