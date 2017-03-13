---
title: 'NoSQL-Tutorial: Azure DocumentDB Java SDK | Microsoft-Dokumentation'
description: "Ein NoSQL-Tutorial, in dem eine Onlinedatenbank und eine Java-Konsolenanwendung mit dem DocumentDB Java SDK erstellt werden. Azure DocumentDB ist eine NoSQL-Datenbank für JSON."
keywords: NoSQL-Tutorial, Onlinedatenbank, Java-Konsolenanwendung
services: documentdb
documentationcenter: Java
author: arramac
manager: jhubbard
editor: monicar
ms.assetid: 75a9efa1-7edd-4fed-9882-c0177274cbb2
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: hero-article
ms.date: 01/05/2017
ms.author: arramac
translationtype: Human Translation
ms.sourcegitcommit: 72b2d9142479f9ba0380c5bd2dd82734e370dee7
ms.openlocfilehash: 74af5fda495adc726bfa85ad48a407fd61d4dd88
ms.lasthandoff: 03/08/2017


---
# <a name="nosql-tutorial-build-a-documentdb-java-console-application"></a>NoSQL-Tutorial: Erstellen einer DocumentDB-Java-Konsolenanwendung
> [!div class="op_single_selector"]
> * [.NET](documentdb-get-started.md)
> * [.NET Core](documentdb-dotnetcore-get-started.md)
> * [Node.js für MongoDB](documentdb-mongodb-samples.md)
> * [Node.js](documentdb-nodejs-get-started.md)
> * [Java](documentdb-java-get-started.md)
> * [C++](documentdb-cpp-get-started.md)
>  
> 

Willkommen beim NoSQL-Tutorial für das Azure DocumentDB Java SDK! Im Rahmen dieses Lernprogramms erstellen Sie eine Konsolenanwendung, mit der DocumentDB-Ressourcen erstellt und abgefragt werden können.

Folgendes wird behandelt:

* Erstellen eines DocumentDB-Kontos und Verbindungsaufbau
* Konfigurieren Ihrer Visual Studio-Projektmappe
* Erstellen einer Onlinedatenbank
* Erstellen einer Sammlung
* Erstellen von JSON-Dokumenten
* Abfragen der Sammlung
* Erstellen von JSON-Dokumenten
* Abfragen der Sammlung
* Ersetzen eines Dokuments
* Löschen eines Dokuments
* Löschen der Datenbank

Lassen Sie uns anfangen.

## <a name="prerequisites"></a>Voraussetzungen
Stellen Sie sicher, dass Sie über Folgendes verfügen:

* Ein aktives Azure-Konto. Wenn Sie keines besitzen, können Sie sich für ein [kostenloses Konto](https://azure.microsoft.com/free/)registrieren. Als Alternative können Sie den [Azure DocumentDB-Emulator](documentdb-nosql-local-emulator.md) für dieses Tutorial verwenden.
* [Git](https://git-scm.com/downloads)
* [Java Development Kit (JDK) 7+](http://www.oracle.com/technetwork/java/javase/downloads/index.html).
* [Maven](http://maven.apache.org/download.cgi).

## <a name="step-1-create-a-documentdb-account"></a>Schritt 1: Erstellen eines DocumentDB-Kontos
In diesem Schritt erstellen Sie ein DocumentDB-Konto. Wenn Sie bereits über ein Konto verfügen, das Sie verwenden möchten, können Sie mit [Klonen des GitHub-Projekts](#GitClone) fortfahren. Falls Sie den DocumentDB-Emulator verwenden, können Sie die Schritte unter [Azure DocumentDB-Emulator](documentdb-nosql-local-emulator.md) zum Einrichten des Emulators ausführen und dann mit [Klonen des GitHub-Projekts](#GitClone) fortfahren.

[!INCLUDE [documentdb-create-dbaccount](../../includes/documentdb-create-dbaccount.md)]

## <a id="GitClone"></a>Schritt 2: Klonen des GitHub-Projekts
Sie können beginnen, indem Sie das GitHub-Repository für [Get Started with DocumentDB and Java](https://github.com/Azure-Samples/documentdb-java-getting-started) (Erste Schritte mit DocumentDB und Java) klonen. Führen Sie in einem lokalen Verzeichnis beispielsweise Folgendes aus, um das Beispielprojekt lokal abzurufen:

    git clone git@github.com:Azure-Samples/documentdb-java-getting-started.git

    cd documentdb-java-getting-started

Das Verzeichnis enthält die Datei `pom.xml` für das Projekt und den Ordner `src` mit dem Java-Quellcode, einschließlich `Program.java`. Hiermit wird veranschaulicht, wie Sie einfache Vorgänge mit Azure DocumentDB durchführen, z.B. das Erstellen von Dokumenten und Abfragen von Daten in einer Sammlung. Die Datei `pom.xml` enthält eine Abhängigkeit im [DocumentDB Java SDK in Maven](https://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb).

    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-documentdb</artifactId>
        <version>LATEST</version>
    </dependency>

## <a id="Connect"></a>Schritt 3: Herstellen einer Verbindung mit einem DocumentDB-Konto
Navigieren Sie als Nächstes wieder zum [Azure-Portal](https://portal.azure.com), um Ihren Endpunkt und den primären Hauptschlüssel abzurufen. Der DocumentDB-Endpunkt und der Primärschlüssel sind erforderlich, damit Ihre Anwendung weiß, womit die Verbindung hergestellt werden soll, und damit DocumentDB weiß, dass die Verbindung Ihrer Anwendung vertrauenswürdig ist.

Navigieren Sie im Azure-Portal zu Ihrem DocumentDB-Konto, und klicken Sie auf **Schlüssel**. Kopieren Sie den URI aus dem Portal, und fügen Sie ihn in `<your endpoint URI>` in der Datei „Program.java“ ein. Kopieren Sie anschließend den PRIMÄRSCHLÜSSEL aus dem Portal, und fügen Sie ihn in `<your key>` ein.

    this.client = new DocumentClient(
        "<your endpoint URI>",
        "<your key>"
        , new ConnectionPolicy(),
        ConsistencyLevel.Session);

![Screenshot des Azure-Portals, das vom NoSQL-Tutorial zum Erstellen einer Java-Konsolenanwendung verwendet wird. Zeigt ein DocumentDB-Konto, bei dem der ACTIVE-Hub, die Schaltfläche „SCHLÜSSEL“ auf dem Blatt „DocumentDB-Konto“ sowie auf dem Blatt „Schlüssel“ die Werte URI, PRIMÄRSCHLÜSSEL und SEKUNDÄRSCHLÜSSEL hervorgehoben sind.][keys]

## <a name="step-4-create-a-database"></a>Schritt 4: Erstellen einer Datenbank
Ihre [DocumentDB](documentdb-resources.md#databases)-Datenbank kann mithilfe der [createDatabase](http://azure.github.io/azure-documentdb-java/com/microsoft/azure/documentdb/DocumentClient.html#createDatabase-com.microsoft.azure.documentdb.Database-com.microsoft.azure.documentdb.RequestOptions-)-Methode der **DocumentClient**-Klasse erstellt werden. Eine Datenbank ist ein logischer Container für JSON-Dokumentspeicher, der auf Sammlungen aufgeteilt ist.

    Database database = new Database();
    database.setId("familydb");
    this.client.createDatabase(database, null);

## <a id="CreateColl"></a>Schritt 5: Erstellen einer Sammlung
> [!WARNING]
> Mit **createCollection** wird eine neue Sammlung mit reserviertem Durchsatz erstellt. Dies wirkt sich auf die Kosten aus. Weitere Informationen finden Sie auf unserer [Preisseite](https://azure.microsoft.com/pricing/details/documentdb/).
> 
> 

Eine [Sammlung](documentdb-resources.md#collections) kann mithilfe der [createCollection](http://azure.github.io/azure-documentdb-java/com/microsoft/azure/documentdb/DocumentClient.html#createCollection-java.lang.String-com.microsoft.azure.documentdb.DocumentCollection-com.microsoft.azure.documentdb.RequestOptions-)-Methode der **DocumentClient**-Klasse erstellt werden. Eine Sammlung ist ein Container für JSON-Dokumente und die zugehörige JavaScript-Anwendungslogik.


    DocumentCollection collectionInfo = new DocumentCollection();
    collectionInfo.setId("familycoll");

    // DocumentDB collections can be reserved with throughput specified in request units/second. 
    // Here we create a collection with 400 RU/s.
    RequestOptions requestOptions = new RequestOptions();
    requestOptions.setOfferThroughput(400);

    this.client.createCollection("/dbs/familydb", collectionInfo, requestOptions);

## <a id="CreateDoc"></a>Schritt 6: Erstellen von JSON-Dokumenten
Ein [Dokument](documentdb-resources.md#documents) kann mithilfe der [createDocument](http://azure.github.io/azure-documentdb-java/com/microsoft/azure/documentdb/DocumentClient.html#createDocument-java.lang.String-java.lang.Object-com.microsoft.azure.documentdb.RequestOptions-boolean-)-Methode der **DocumentClient**-Klasse erstellt werden. Dokumente sind benutzerdefinierter (beliebiger) JSON-Inhalt. Wir können jetzt eines oder mehrere Dokumente einfügen. Wenn Sie bereits über Daten verfügen, die Sie in der Datenbank speichern möchten, können Sie das [Datenmigrationstool](documentdb-import-data.md) von DocumentDB verwenden, um die Daten in die Datenbank zu importieren.

    // Insert your Java objects as documents 
    Family andersenFamily = new Family();
    andersenFamily.setId("Andersen.1");
    andersenFamily.setLastName("Andersen")

    // More initialization skipped for brevity. You can have nested references
    andersenFamily.setParents(new Parent[] { parent1, parent2 });
    andersenFamily.setDistrict("WA5");
    Address address = new Address();
    address.setCity("Seattle");
    address.setCounty("King");
    address.setState("WA");

    andersenFamily.setAddress(address);
    andersenFamily.setRegistered(true);

    this.client.createDocument("/dbs/familydb/colls/familycoll", family, new RequestOptions(), true);

![Diagramm zur hierarchischen Beziehung zwischen dem Konto, der Onlinedatenbank, der Sammlung und den Dokumenten, die vom NoSQL-Tutorial zum Erstellen einer Java-Konsolenanwendung verwendet werden.](./media/documentdb-get-started/nosql-tutorial-account-database.png)

## <a id="Query"></a>Schritt 7: Abfragen von DocumentDB-Ressourcen
DocumentDB unterstützt umfassende [Abfragen](documentdb-sql-query.md) der in jeder Sammlung gespeicherten JSON-Dokumente.  Der folgende Beispielcode veranschaulicht, wie Sie Dokumente in DocumentDB abfragen, indem Sie SQL-Syntax mit der [queryDocuments](http://azure.github.io/azure-documentdb-java/com/microsoft/azure/documentdb/DocumentClient.html#queryDocuments-java.lang.String-com.microsoft.azure.documentdb.SqlQuerySpec-com.microsoft.azure.documentdb.FeedOptions-)-Methode verwenden.

    FeedResponse<Document> queryResults = this.client.queryDocuments(
        "/dbs/familydb/colls/familycoll",
        "SELECT * FROM Family WHERE Family.lastName = 'Andersen'", 
        null);

    System.out.println("Running SQL query...");
    for (Document family : queryResults.getQueryIterable()) {
        System.out.println(String.format("\tRead %s", family));
    }

## <a id="ReplaceDocument"></a>Schritt 8: Ersetzen eines JSON-Dokuments
DocumentDB unterstützt die Aktualisierung von JSON-Dokumenten per [replaceDocument](http://azure.github.io/azure-documentdb-java/com/microsoft/azure/documentdb/DocumentClient.html#replaceDocument-com.microsoft.azure.documentdb.Document-com.microsoft.azure.documentdb.RequestOptions-)-Methode.

    // Update a property
    andersenFamily.Children[0].Grade = 6;

    this.client.replaceDocument(
        "/dbs/familydb/colls/familycoll/docs/Andersen.1", 
        andersenFamily,
        null);

## <a id="DeleteDocument"></a>Schritt 9: Löschen eines JSON-Dokuments
DocumentDB unterstützt auch das Löschen von JSON-Dokumenten mit der [deleteDocument](http://azure.github.io/azure-documentdb-java/com/microsoft/azure/documentdb/DocumentClient.html#deleteDocument-java.lang.String-com.microsoft.azure.documentdb.RequestOptions-)-Methode.  

    this.client.delete("/dbs/familydb/colls/familycoll/docs/Andersen.1", null);

## <a id="DeleteDatabase"></a>Schritt 10: Löschen der Datenbank
Wenn Sie die erstellte Datenbank löschen, werden auch alle untergeordneten Ressourcen (Sammlungen, Dokumente usw.) gelöscht.

    this.client.deleteDatabase("/dbs/familydb", null);

## <a id="Run"></a>Schritt 11: Ausführen der gesamten Java-Konsolenanwendung
Zum Ausführen der Anwendung über die Konsole müssen Sie zuerst eine Kompilierung mit Maven durchführen:
    
    mvn package

Wenn Sie `mvn package` ausführen, wird die aktuelle DocumentDB-Bibliothek aus Maven heruntergeladen, und die Datei `GetStarted-0.0.1-SNAPSHOT.jar` wird erzeugt. Führen Sie die App dann wie folgt aus:

    mvn exec:java -D exec.mainClass=GetStarted.Program

Glückwunsch! Sie haben dieses NoSQL-Tutorial abgeschlossen und verfügen über eine funktionierende Java-Konsolenanwendung!

## <a name="next-steps"></a>Nächste Schritte
* Sind Sie an einem Tutorial zu Java-Web-Apps interessiert? Weitere Informationen finden Sie unter [Erstellen einer Java-Webanwendung mithilfe von DocumentDB](documentdb-java-application.md).
* Sie erfahren, wie Sie ein [DocumentDB-Konto überwachen](documentdb-monitor-accounts.md).
* Fragen Sie unser Beispieldataset im [Query Playground](https://www.documentdb.com/sql/demo)ab.
* Weitere Informationen zum Programmiermodell finden Sie auf der [DocumentDB-Dokumentationsseite](https://azure.microsoft.com/documentation/services/documentdb/)im Abschnitt "Entwickeln".

[documentdb-create-account]: documentdb-create-account.md
[keys]: media/documentdb-get-started/nosql-tutorial-keys.png

