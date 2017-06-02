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
ms.date: 05/22/2017
ms.author: anhoh
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: a643f139be40b9b11f865d528622bafbe7dec939
ms.openlocfilehash: ab4fb39f8784a2d6aa956056a30649aab1d08487
ms.contentlocale: de-de
ms.lasthandoff: 05/31/2017

---

# <a name="azure-cosmos-db-how-to-import-mongodb-data"></a>Azure Cosmos DB: Importieren von MongoDB-Daten 

Sie müssen wie folgt vorgehen, um Daten von MongoDB in ein auf Azure Cosmos DB-Konto für die Verwendung mit der MongoDB-API zu migrieren:

* Laden Sie entweder *mongoimport.exe* oder *mongorestore.exe* aus dem [MongoDB Download Center](https://www.mongodb.com/download-center) herunter.
* Rufen Sie Ihre [API für die MongoDB-Verbindungszeichenfolge](connect-mongodb-account.md) ab.

Wenn Sie Daten aus MongoDB importieren und diese mit der DocumentDB-API verwenden möchten, sollten Sie die Daten mithilfe des Datenmigrationstool importieren. Weitere Informationen finden Sie unter [Datenmigrationstool](import-data.md).

Dieses Tutorial enthält die folgenden Aufgaben:

> [!div class="checklist"]
> * Abrufen der Verbindungszeichenfolge
> * Importieren von MongoDB-Daten mithilfe von „mongoimport“
> * Importieren von MongoDB-Daten mithilfe von „mongorestore“

## <a name="prerequisites"></a>Voraussetzungen

* Erhöhung des Durchsatzes: Die Dauer der Datenmigration richtet sich nach der Durchsatzmenge, die Sie für Ihre Sammlungen einrichten. Achten Sie darauf, dass Sie den Durchsatz für größere Datenmigrationen erhöhen. Nachdem die Migration abgeschlossen ist, können Sie den Durchsatz wieder verringern, um Kosten zu sparen. Weitere Informationen zur Erhöhung des Durchsatzes im [Azure-Portal](https://portal.azure.com) finden Sie unter [Leistungsstufen und Tarife in Azure Cosmos DB](performance-levels.md).

* SSL-Aktivierung: Für Azure Cosmos DB gelten strenge Sicherheitsanforderungen und -standards. Achten Sie darauf, SSL für die Interaktion mit Ihrem Konto zu aktivieren. Die Verfahren im restlichen Artikel verdeutlichen, wie Sie SSL für *mongoimport* und *mongorestore* aktivieren.

## <a name="find-your-connection-string-information-host-port-username-and-password"></a>Suchen nach den Informationen für die Verbindungszeichenfolge (Host, Port, Benutzername und Kennwort)

1. Klicken Sie im [Azure-Portal](https://portal.azure.com) auf der linken Seite auf den Eintrag **Azure Cosmos DB**.
2. Wählen Sie unter **Abonnements** den Namen Ihres Kontos aus.
3. Klicken Sie auf dem Blatt **Verbindungszeichenfolge** auf **Verbindungszeichenfolge**.  
Der rechte Bereich enthält alle Informationen, die Sie zum erfolgreichen Verbinden des Kontos benötigen.

    ![Blatt „Verbindungszeichenfolge“](./media/mongodb-migrate/ConnectionStringBlade.png)

## <a name="import-data-to-api-for-mongodb-with-mongoimport"></a>Importieren von Daten in die API für MongoDB mit „mongoimport“

Verwenden Sie zum Importieren von Daten in Ihr Azure Cosmos DB-Konto die folgende Vorlage, um den Importvorgang durchzuführen. Geben Sie für *host*, *username* und *password* die Werte ein, die für Ihr Konto gelten.  

Vorlage:

    mongoimport.exe --host <your_hostname>:10250 -u <your_username> -p <your_password> --db <your_database> --collection <your_collection> --ssl --sslAllowInvalidCertificates --type json --file C:\sample.json

Beispiel:  

    mongoimport.exe --host anhoh-host.documents.azure.com:10250 -u anhoh-host -p tkvaVkp4Nnaoirnouenrgisuner2435qwefBH0z256Na24frio34LNQasfaefarfernoimczciqisAXw== --ssl --sslAllowInvalidCertificates --db sampleDB --collection sampleColl --type json --file C:\Users\anhoh\Desktop\*.json

## <a name="import-data-to-api-for-mongodb-with-mongorestore"></a>Importieren von Daten in die API für MongoDB mit „mongorestore“

Verwenden Sie zum Wiederherstellen von Daten für Ihre API für das MongoDB-Konto die folgende Vorlage, um den Importvorgang durchzuführen. Geben Sie für *host*, *username* und *password* die Werte ein, die für Ihr Konto gelten.

Vorlage:

    mongorestore.exe --host <your_hostname>:10250 -u <your_username> -p <your_password> --db <your_database> --collection <your_collection> --ssl --sslAllowInvalidCertificates <path_to_backup>

Beispiel:

    mongorestore.exe --host anhoh-host.documents.azure.com:10250 -u anhoh-host -p tkvaVkp4Nnaoirnouenrgisuner2435qwefBH0z256Na24frio34LNQasfaefarfernoimczciqisAXw== --ssl --sslAllowInvalidCertificates ./dumps/dump-2016-12-07

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie die folgenden Aufgaben ausgeführt:

> [!div class="checklist"]
> * Abrufen der Verbindungszeichenfolge
> * Importieren von MongoDB-Daten mithilfe von „mongoimport“
> * Importieren von MongoDB-Daten mithilfe von „mongorestore“

Sie können nun mit dem nächsten Tutorial fortfahren und sich darüber informieren, wie Sie MongoDB-Daten mithilfe von Azure Cosmos DB abfragen können. 

> [!div class="nextstepaction"]
>[Abfragen von MongoDB-Daten](../cosmos-db/tutorial-query-mongodb.md)

