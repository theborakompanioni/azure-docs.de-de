---
title: "Verwenden von „mongoimport“ und „mongorestore“ mit Azure DocumentDB | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie „mongoimport“ und „mongorestore“ zum Importieren von Daten in ein DocumentDB verwenden: API für MongoDB-Konto"
keywords: mongoimport, mongorestore
services: documentdb
author: AndrewHoh
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: 352c5fb9-8772-4c5f-87ac-74885e63ecac
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: anhoh
translationtype: Human Translation
ms.sourcegitcommit: 72b2d9142479f9ba0380c5bd2dd82734e370dee7
ms.openlocfilehash: 2af8691326550c631e6668890cb0d6b893fa7740
ms.lasthandoff: 03/08/2017


---
# <a name="migrate-data-to-documentdb-by-using-mongoimport-and-mongorestore"></a>Migrieren von Daten zu DocumentDB mit „mongoimport“ und „mongorestore“
> [!div class="op_single_selector"]
> * [Importieren in DocumentDB](documentdb-import-data.md)
> * [Importieren in die API für MongoDB](documentdb-mongodb-migrate.md)
>
>

Zum Migrieren von Daten in ein Azure DocumentDB: API für MongoDB-Konto, müssen Sie folgendes durchführen:

* Laden Sie entweder *mongoimport.exe* oder *mongorestore.exe* aus dem [MongoDB Download Center](https://www.mongodb.com/download-center) herunter.
* Beschaffen Sie sich die [DocumentDB-Unterstützung für die MongoDB-Verbindungszeichenfolge](documentdb-connect-mongodb-account.md).

## <a name="before-you-begin"></a>Voraussetzungen

* Erhöhung des Durchsatzes: Die Dauer der Datenmigration richtet sich nach der Durchsatzmenge, die Sie für Ihre Sammlungen einrichten. Achten Sie darauf, dass Sie den Durchsatz für größere Datenmigrationen erhöhen. Nachdem die Migration abgeschlossen ist, können Sie den Durchsatz wieder verringern, um Kosten zu sparen. Weitere Informationen zur Erhöhung des Durchsatzes im [Azure-Portal](https://portal.azure.com) finden Sie unter [Leistungsstufen und Tarife in DocumentDB](documentdb-performance-levels.md).

* SSL-Aktivierung: Für DocumentDB gelten strenge Sicherheitsanforderungen und -standards. Achten Sie darauf, SSL für die Interaktion mit Ihrem Konto zu aktivieren. Die Verfahren im restlichen Artikel verdeutlichen, wie Sie SSL für *mongoimport* und *mongorestore* aktivieren.

## <a name="find-your-connection-string-information-host-port-username-and-password"></a>Suchen nach den Informationen für die Verbindungszeichenfolge (Host, Port, Benutzername und Kennwort)

1. Klicken Sie im [Azure-Portal](https://portal.azure.com) auf der linken Seite auf den Eintrag **NoSQL (DocumentDB)**.
2. Wählen Sie unter **Abonnements** den Namen Ihres Kontos aus.
3. Klicken Sie auf dem Blatt **Verbindungszeichenfolge** auf **Verbindungszeichenfolge**.  
Der rechte Bereich enthält alle Informationen, die Sie zum erfolgreichen Verbinden des Kontos benötigen.

    ![Blatt „Verbindungszeichenfolge“](./media/documentdb-mongodb-migrate/ConnectionStringBlade.png)

## <a name="import-data-to-api-for-mongodb-with-mongoimport"></a>Importieren von Daten in die API für MongoDB mit „mongoimport“

Verwenden Sie zum Importieren von Daten in Ihr DocumentDB-Konto die folgende Vorlage, um den Importvorgang durchzuführen. Geben Sie für *host*, *username* und *password* die Werte ein, die für Ihr Konto gelten.  

Vorlage:

    mongoimport.exe --host <your_hostname>:10250 -u <your_username> -p <your_password> --db <your_database> --collection <your_collection> --ssl --sslAllowInvalidCertificates --type json --file C:\sample.json

Beispiel:  

    mongoimport.exe --host anhoh-host.documents.azure.com:10250 -u anhoh-host -p tkvaVkp4Nnaoirnouenrgisuner2435qwefBH0z256Na24frio34LNQasfaefarfernoimczciqisAXw== --ssl --sslAllowInvalidCertificates --db sampleDB --collection sampleColl --type json --file C:\Users\anhoh\Desktop\*.json

## <a name="import-data-to-api-for-mongodb-with-mongorestore"></a>Importieren von Daten in die API für MongoDB mit „mongorestore“

Verwenden Sie zum Wiederherstellen von Daten in Ihr DocumentDB-Konto die folgende Vorlage, um den Importvorgang durchzuführen. Geben Sie für *host*, *username* und *password* die Werte ein, die für Ihr Konto gelten.

Vorlage:

    mongorestore.exe --host <your_hostname>:10250 -u <your_username> -p <your_password> --db <your_database> --collection <your_collection> --ssl --sslAllowInvalidCertificates <path_to_backup>

Beispiel:

    mongorestore.exe --host anhoh-host.documents.azure.com:10250 -u anhoh-host -p tkvaVkp4Nnaoirnouenrgisuner2435qwefBH0z256Na24frio34LNQasfaefarfernoimczciqisAXw== --ssl --sslAllowInvalidCertificates ./dumps/dump-2016-12-07

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen finden Sie unter [DocumentDB: API for MongoDB samples (Beispiele für die DocumentDB: API für MongoDB)](documentdb-mongodb-samples.md).

