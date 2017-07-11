---
title: 'Azure Cosmos DB: Erstellen einer Konsolen-App mit einer MongoDB-API mit Golang und dem Azure-Portal | Microsoft-Dokumentation'
description: "Hier finden Sie ein Golang-Codebeispiel, das Sie zum Herstellen einer Verbindung mit und zum Abfragen von Azure Cosmos DB verwenden können."
services: cosmos-db
author: Durgaprasad-Budhwani
manager: jhubbard
editor: mimig1
ms.service: cosmos-db
ms.topic: hero-article
ms.date: 06/22/2017
ms.author: mimig
ms.translationtype: Human Translation
ms.sourcegitcommit: 138f04f8e9f0a9a4f71e43e73593b03386e7e5a9
ms.openlocfilehash: a7da42f4b84b77ad0a82004a56f7712c31567449
ms.contentlocale: de-de
ms.lasthandoff: 06/29/2017

---

<a id="azure-cosmos-db-build-a-mongodb-api-console-app-with-golang-and-the-azure-portal" class="xliff"></a>

# Azure Cosmos DB: Erstellen einer Konsolen-App mit einer MongoDB-API mit Golang und dem Azure-Portal

Azure Cosmos DB ist der global verteilte Datenbankdienst von Microsoft mit mehreren Modellen. Sie können schnell Dokument-, Schlüssel/Wert- und Graph-Datenbanken erstellen und abfragen und dabei stets von den Vorteilen der globalen Verteilung und der horizontalen Skalierung profitieren, die Azure Cosmos DB zugrunde liegen.

Dieser Schnellstart veranschaulicht, wie Sie eine vorhandene, in [Golang](https://golang.org/) geschriebene [MongoDB](https://docs.microsoft.com/en-us/azure/cosmos-db/mongodb-introduction)-App verwenden und sie mit Ihrer Azure Cosmos DB-Datenbank verbinden, die MongoDB-Clientverbindungen unterstützt.

Anders ausgedrückt: Die Golang-Anwendung verfügt nur über Informationen darüber, dass sie mithilfe von MongoDB-APIs eine Verbindung mit einer Datenbank herstellt. Für die Anwendung ist es ersichtlich, dass die Daten in Azure Cosmos DB gespeichert sind.

<a id="prerequisites" class="xliff"></a>

## Voraussetzungen

- Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free) erstellen, bevor Sie beginnen.
- [Go](https://golang.org/dl/) und Grundkenntnisse der Sprache [Go](https://golang.org/).
- Eine integrierte Entwicklungsumgebung (IDE): [Gogland](https://www.jetbrains.com/go/) von Jetbrains, [Visual Studio Code](https://code.visualstudio.com/) von Microsoft oder [Atom](https://atom.io/). In diesem Lernprogramm verwende ich Goglang.

<a id="create-account"></a>
<a id="create-a-database-account" class="xliff"></a>

## Erstellen eines Datenbankkontos

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

<a id="clone-the-sample-application" class="xliff"></a>

## Klonen der Beispielanwendung

Klonen Sie die Beispielanwendung, und installieren Sie die erforderlichen Pakete.

1. Erstellen Sie einen Ordner mit dem Namen „CosmosDBSample“ im Ordner „GOROOT\src“, der sich standardmäßig unter „C:\Go\“ befindet.
2. Führen Sie über ein Git-Terminalfenster, z.B. Git Bash, den folgenden Befehl aus, um das Beispielrepository im Ordner „CosmosDBSample“ zu klonen. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-mongodb-golang-getting-started.git
    ```
3.  Führen Sie den unten angegebenen Befehl aus, um das mgo-Paket abzurufen. 

    ```
    go get gopkg.in/mgo.v2
    ```

Der [mgo](http://labix.org/mgo)-Treiber (Aussprache: *mango*) ist ein [MongoDB](http://www.mongodb.org/)-Treiber für die [Sprache Go](http://golang.org/), mit dem eine umfassende und gründlich getestete Auswahl von Funktionen unter einer sehr einfachen API auf Grundlage von Go-Standardbegriffen implementiert wird.

<a id="connection-string"></a>

<a id="update-your-connection-string" class="xliff"></a>

## Aktualisieren der Verbindungszeichenfolge

Wechseln Sie nun zurück zum Azure-Portal, um die Informationen der Verbindungszeichenfolge abzurufen und in die App zu kopieren.

1. Klicken Sie im Navigationsmenü auf der linken Seite auf **Schnellstart** und dann auf **Andere**, um die Verbindungszeichenfolgen-Informationen anzuzeigen, die für die Go-Anwendung erforderlich sind.

2. Öffnen Sie in Goglang die Datei „main.go“ im Verzeichnis „GOROOT\CosmosDBSample“, und aktualisieren Sie die folgenden Codezeilen, indem Sie die Verbindungszeichenfolgen-Informationen aus dem Azure-Portal verwenden. Dies ist im folgenden Screenshot dargestellt. 

    Der Datenbankname ist das Präfix des Werts **Host** im Bereich des Azure-Portals mit der Verbindungszeichenfolge. Für das Konto, das in der Abbildung unten dargestellt ist, lautet der Datenbankname „golang-coach“.

    ```go
    Database: "The prefix of the Host value in the Azure portal",
    Username: "The Username in the Azure portal",
    Password: "The Password in the Azure portal",
    ```

    ![Schnellstart-Bereich: Registerkarte „Andere“ im Azure-Portal mit den Verbindungszeichenfolgen-Informationen](./media/create-mongodb-golang/cosmos-db-golang-connection-string.png)

3. Speichern Sie die Datei „main.go“.

<a id="review-the-code" class="xliff"></a>

## Überprüfen des Codes

Es folgt ein kurzer Überblick, was in der Datei „main.go“ geschieht. 

<a id="connecting-the-go-app-to-azure-cosmos-db" class="xliff"></a>

### Herstellen einer Verbindung für die Go-App mit Azure Cosmos DB

Azure Cosmos DB unterstützt die SSL-fähige MongoDB. Zum Herstellen einer Verbindung mit einer SSL-fähigen MongoDB müssen Sie die Funktion **DialServer** in [mgo.DialInfo](http://gopkg.in/mgo.v2#DialInfo) definieren und die Funktion [tls.*Dial*](http://golang.org/pkg/crypto/tls#Dial) nutzen, um die Verbindung herzustellen.

Mit dem folgenden Golang-Codeausschnitt wird die Go-App mit der Azure Cosmos DB-MongoDB-API verbunden. Die *DialInfo*-Klasse enthält Optionen zum Einrichten einer Sitzung mit einem MongoDB-Cluster.

```go
// DialInfo holds options for establishing a session with a MongoDB cluster.
dialInfo := &mgo.DialInfo{
    Addrs:    []string{"golang-couch.documents.azure.com:10255"}, // Get HOST + PORT
    Timeout:  60 * time.Second,
    Database: "golang-coach", // It can be anything
    Username: "golang-coach", // Username
    Password: "Azure database connect password from Azure Portal", // PASSWORD
    DialServer: func(addr *mgo.ServerAddr) (net.Conn, error) {
        return tls.Dial("tcp", addr.String(), &tls.Config{})
    },
}

// Create a session which maintains a pool of socket connections
// to our Azure Cosmos DB MongoDB database.
session, err := mgo.DialWithInfo(dialInfo)

if err != nil {
    fmt.Printf("Can't connect to mongo, go error %v\n", err)
    os.Exit(1)
}

defer session.Close()

// SetSafe changes the session safety mode.
// If the safe parameter is nil, the session is put in unsafe mode, 
// and writes become fire-and-forget,
// without error checking. The unsafe mode is faster since operations won't hold on waiting for a confirmation.
// 
session.SetSafe(&mgo.Safe{})
```

Die **mgo.Dial()**-Methode wird verwendet, wenn keine SSL-Verbindung vorhanden ist. Für eine SSL-Verbindung ist die **mgo.DialWithInfo()**-Methode erforderlich.

Eine Instanz des **DialWIthInfo{}**-Objekts wird zum Erstellen des Sitzungsobjekts verwendet. Nach dem Einrichten der Sitzung können Sie auf die Sammlung zugreifen, indem Sie den folgenden Codeausschnitt verwenden:

```go
collection := session.DB(“golang-couch”).C(“package”)
```

<a id="create-document"></a>

<a id="create-a-document" class="xliff"></a>

### Erstellen eines Dokuments

```go
// Model
type Package struct {
    Id bson.ObjectId  `bson:"_id,omitempty"`
    FullName      string
    Description   string
    StarsCount    int
    ForksCount    int
    LastUpdatedBy string
}

// insert Document in collection
err = collection.Insert(&Package{
    FullName:"react",
    Description:"A framework for building native apps with React.",
    ForksCount: 11392,
    StarsCount:48794,
    LastUpdatedBy:"shergin",

})

if err != nil {
    log.Fatal("Problem inserting data: ", err)
    return
}
```

<a id="query-or-read-a-document" class="xliff"></a>

### Abfragen oder Lesen eines Dokuments

Azure Cosmos DB unterstützt umfassende Abfragen der in jeder Sammlung gespeicherten JSON-Dokumente. Der folgende Beispielcode zeigt eine Abfrage, die Sie für die Dokumente in Ihrer Sammlung ausführen können.

```go
// Get a Document from the collection
result := Package{}
err = collection.Find(bson.M{"fullname": "react"}).One(&result)
if err != nil {
    log.Fatal("Error finding record: ", err)
    return
}

fmt.Println("Description:", result.Description)
```


<a id="update-a-document" class="xliff"></a>

### Aktualisieren eines Dokuments

```go
// Update a document
updateQuery := bson.M{"_id": result.Id}
change := bson.M{"$set": bson.M{"fullname": "react-native"}}
err = collection.Update(updateQuery, change)
if err != nil {
    log.Fatal("Error updating record: ", err)
    return
}
```

<a id="delete-a-document" class="xliff"></a>

### Löschen eines Dokuments

Azure Cosmos DB unterstützt das Löschen von JSON-Dokumenten.

```go
// Delete a document
query := bson.M{"_id": result.Id}
err = collection.Remove(query)
if err != nil {
   log.Fatal("Error deleting record: ", err)
   return
}
```
    
<a id="run-the-app" class="xliff"></a>

## Ausführen der App

1. Stellen Sie in Goglang sicher, dass Ihr GOPATH (verfügbar unter **Datei**, **Einstellungen**, **Go**, **GOPATH**) den Speicherort enthält, an dem gopkg installiert wurde (standardmäßig unter „USERPROFILE\go“). 
2. Kommentieren Sie die Zeilen aus, mit denen das Dokument gelöscht wird (Zeilen 91 - 96), damit Sie das Dokument nach dem Ausführen der App sehen können.
3. Klicken Sie in Goglang auf **Run** (Ausführen) und anschließend auf **Run 'Build main.go and run'** ('main.go erstellen und ausführen' ausführen).

    Die App wird beendet, und die Beschreibung des Dokuments, das unter [Erstellen eines Dokuments](#create-document) erstellt wurde, wird angezeigt.
    
    ```
    Description: A framework for building native apps with React.
    
    Process finished with exit code 0
    ```

    ![Goglang mit der Ausgabe der App](./media/create-mongodb-golang/goglang-cosmos-db.png)
    
<a id="review-your-document-in-data-explorer" class="xliff"></a>

## Prüfen Ihres Dokuments im Daten-Explorer

Wechseln Sie zurück in das Azure-Portal, um Ihr Dokument im Daten-Explorer anzuzeigen.

1. Klicken Sie im Navigationsmenü auf der linken Seite auf **Daten-Explorer (Vorschau)**, erweitern Sie **golang-coach** und **package**, und klicken Sie dann auf **Dokumente**. Klicken Sie auf der Registerkarte **Dokumente** auf die „\_id“, um das Dokument im rechten Bereich anzuzeigen. 

    ![Daten-Explorer mit dem neu erstellten Dokument](./media/create-mongodb-golang/golang-cosmos-db-data-explorer.png)
    
2. Sie können dann inline mit dem Dokument arbeiten und auf **Aktualisieren** klicken, um es zu speichern. Außerdem können Sie das Dokument löschen oder neue Dokumente oder Abfragen erstellen.

<a id="review-slas-in-the-azure-portal" class="xliff"></a>

## Überprüfen von SLAs im Azure-Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

<a id="clean-up-resources" class="xliff"></a>

## Bereinigen von Ressourcen

Wenn Sie diese App nicht weiter verwenden möchten, löschen Sie alle von diesem Schnellstart erstellten Ressourcen im Azure-Portal. Führen Sie dazu folgende Schritte durch:

1. Klicken Sie im Azure-Portal im Menü auf der linken Seite auf **Ressourcengruppen**, und klicken Sie auf den Namen der erstellten Ressource. 
2. Klicken Sie auf der Seite mit Ihrer Ressourcengruppe auf **Löschen**, geben Sie im Textfeld den Namen der zu löschenden Ressource ein, und klicken Sie dann auf **Löschen**.

<a id="next-steps" class="xliff"></a>

## Nächste Schritte

In diesem Schnellstart haben Sie gelernt, wie Sie ein Azure Cosmos DB-Konto erstellen und eine Golang-App mithilfe der API für MongoDB ausführen. Jetzt können Sie zusätzliche Daten in Ihr Cosmos DB-Konto importieren. 

> [!div class="nextstepaction"]
> [Importieren von Daten in Azure Cosmos DB für die MongoDB-API](mongodb-migrate.md)

