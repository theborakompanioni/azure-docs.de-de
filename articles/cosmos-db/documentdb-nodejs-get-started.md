---
title: "Node.js-Tutorial für die DocumentDB-API für Azure Cosmos DB | Microsoft-Dokumentation"
description: "Enthält ein Node.js-Tutorial, in dem mit der DocumentDB-API eine Cosmos DB erstellt wird."
keywords: node.js tutorial, node database
services: cosmos-db
documentationcenter: node.js
author: AndrewHoh
manager: jhubbard
editor: monicar
ms.assetid: 14d52110-1dce-4ac0-9dd9-f936afccd550
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: node
ms.topic: article
ms.date: 08/14/2017
ms.author: anhoh
ms.translationtype: HT
ms.sourcegitcommit: fda37c1cb0b66a8adb989473f627405ede36ab76
ms.openlocfilehash: 02e98aadc6a001c7275266d89a196a57bb366b3c
ms.contentlocale: de-de
ms.lasthandoff: 09/14/2017

---
# <a name="nodejs-tutorial-use-the-documentdb-api-in-azure-cosmos-db-to-create-a-nodejs-console-application"></a>Node.js-Tutorial: Verwenden der DocumentDB-API in Azure Cosmos DB zum Erstellen einer Node.js-Konsolenanwendung
> [!div class="op_single_selector"]
> * [.NET](documentdb-get-started.md)
> * [.NET Core](documentdb-dotnetcore-get-started.md)
> * [Node.js für MongoDB](mongodb-samples.md)
> * [Node.js](documentdb-nodejs-get-started.md)
> * [Java](documentdb-java-get-started.md)
> * [C++](documentdb-cpp-get-started.md)
>  
> 

Willkommen beim Node.js-Tutorial für das Azure Cosmos DB Node.js SDK! Im Rahmen dieses Tutorials erstellen Sie eine Konsolenanwendung, mit der Azure Cosmos DB-Ressourcen erstellt und abgefragt werden können.

Folgende Themen werden behandelt:

* Erstellen eines Azure Cosmos DB-Kontos und Herstellen der Verbindung
* Einrichten der Anwendung
* Erstellen einer Node-Datenbank
* Erstellen einer Sammlung
* Erstellen von JSON-Dokumenten
* Abfragen der Sammlung
* Ersetzen eines Dokuments
* Löschen eines Dokuments
* Löschen der Node-Datenbank

Sie haben nicht genügend Zeit? Keine Sorge! Die vollständige Lösung ist auf [GitHub](https://github.com/Azure-Samples/documentdb-node-getting-started)verfügbar. In [Abrufen der vollständigen Lösung](#GetSolution) finden Sie eine Kurzanleitung.

Bitte verwenden Sie nach Abschluss des Node.js-Tutorials die Abstimmungsschaltflächen am Anfang und am Ende dieser Seite, um uns Ihre Meinung mitzuteilen. Wenn wir mit Ihnen Kontakt aufnehmen sollen, können Sie Ihre E-Mail-Adresse im Kommentar hinterlassen.

Lassen Sie uns anfangen.

## <a name="prerequisites-for-the-nodejs-tutorial"></a>Voraussetzungen für das Node.js-Tutorial
Stellen Sie sicher, dass Sie über Folgendes verfügen:

* Ein aktives Azure-Konto. Wenn Sie kein Konto haben, können Sie sich für eine [kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/)registrieren. 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* [Node.js](https://nodejs.org/) Version v0.10.29 oder höher.

## <a name="step-1-create-an-azure-cosmos-db-account"></a>Schritt 1: Erstellen eines Azure Cosmos DB-Kontos
Wir erstellen nun ein Azure Cosmos DB-Konto. Wenn Sie bereits über ein Konto verfügen, das Sie verwenden möchten, können Sie diesen Schritt überspringen und mit [Einrichten der Node.js-Anwendung](#SetupNode) fortfahren. Wenn Sie den Azure Cosmos DB-Emulator verwenden, führen Sie die Schritte unter [Azure Cosmos DB-Emulator](local-emulator.md) zum Einrichten des Emulators aus, und fahren Sie dann mit [Einrichten der Node.js-Anwendung](#SetupNode) fort.

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="SetupNode"></a>Schritt 2: Einrichten der Node.js-Anwendung
1. Öffnen Sie den bevorzugten Terminaldienst.
2. Suchen Sie nach dem Ordner oder Verzeichnis, in dem Sie die Node.js-Anwendung speichern möchten.
3. Erstellen Sie mit den folgenden Befehlen zwei leere JavaScript-Dateien:
   * Windows:
     * ```fsutil file createnew app.js 0```
     * ```fsutil file createnew config.js 0```
   * Linux/OS X:
     * ```touch app.js```
     * ```touch config.js```
4. Installieren Sie das documentdb-Modul über npm. Verwenden Sie den folgenden Befehl:
   * ```npm install documentdb --save```

Prima. Damit ist die Einrichtung abgeschlossen, und wir können mit dem Schreiben von Code beginnen.

## <a id="Config"></a>Schritt 3: Festlegen der Konfigurationen der App
Öffnen Sie ```config.js``` in einem Text-Editor Ihrer Wahl.

Kopieren Sie dann den folgenden Codeausschnitt, und fügen Sie ihn ein. Legen Sie die Eigenschaften ```config.endpoint``` und ```config.primaryKey``` auf den Endpunkt-URI und den Primärschlüssel Ihrer Azure Cosmos DB-Instanz fest. Beide Konfigurationen finden Sie im [Azure-Portal](https://portal.azure.com).

![Node.js-Tutorial – Screenshot des Azure-Portals mit einem Azure Cosmos DB-Konto, bei dem der AKTIVE Hub, die Schaltfläche SCHLÜSSEL auf dem Blatt „Azure Cosmos DB-Konto“ und auf dem Blatt „Schlüssel“ die Werte URI, PRIMÄRSCHLÜSSEL und SEKUNDÄRSCHLÜSSEL hervorgehoben sind – Node-Datenbank][keys]

    // ADD THIS PART TO YOUR CODE
    var config = {}

    config.endpoint = "~your Azure Cosmos DB endpoint uri here~";
    config.primaryKey = "~your primary key here~";

Fügen Sie ```database id```, ```collection id```, und ```JSON documents``` an der Stelle in Ihr ```config```-Objekt ein, an der die Eigenschaften ```config.endpoint``` und ```config.primaryKey``` festgelegt werden. Wenn Sie bereits über Daten verfügen, die Sie in der Datenbank speichern möchten, können Sie das [Datenmigrationstool](import-data.md) von Azure Cosmos DB verwenden, anstatt Dokumentdefinitionen hinzuzufügen.

    config.endpoint = "~your Azure Cosmos DB endpoint uri here~";
    config.primaryKey = "~your primary key here~";

    // ADD THIS PART TO YOUR CODE
    config.database = {
        "id": "FamilyDB"
    };

    config.collection = {
        "id": "FamilyColl"
    };

    config.documents = {
        "Andersen": {
            "id": "Anderson.1",
            "lastName": "Andersen",
            "parents": [{
                "firstName": "Thomas"
            }, {
                    "firstName": "Mary Kay"
                }],
            "children": [{
                "firstName": "Henriette Thaulow",
                "gender": "female",
                "grade": 5,
                "pets": [{
                    "givenName": "Fluffy"
                }]
            }],
            "address": {
                "state": "WA",
                "county": "King",
                "city": "Seattle"
            }
        },
        "Wakefield": {
            "id": "Wakefield.7",
            "parents": [{
                "familyName": "Wakefield",
                "firstName": "Robin"
            }, {
                    "familyName": "Miller",
                    "firstName": "Ben"
                }],
            "children": [{
                "familyName": "Merriam",
                "firstName": "Jesse",
                "gender": "female",
                "grade": 8,
                "pets": [{
                    "givenName": "Goofy"
                }, {
                        "givenName": "Shadow"
                    }]
            }, {
                    "familyName": "Miller",
                    "firstName": "Lisa",
                    "gender": "female",
                    "grade": 1
                }],
            "address": {
                "state": "NY",
                "county": "Manhattan",
                "city": "NY"
            },
            "isRegistered": false
        }
    };


Die Datenbank-, Sammlungs- und Dokumentdefinitionen dienen als ```database id```, ```collection id``` und Dokumentdaten Ihrer Azure Cosmos DB-Instanz.

Exportieren Sie abschließend das ```config```-Objekt, um in der Datei ```app.js``` darauf verweisen zu können.

            },
            "isRegistered": false
        }
    };

    // ADD THIS PART TO YOUR CODE
    module.exports = config;

## <a id="Connect"></a>Schritt 4: Herstellen einer Verbindung mit einem Azure Cosmos DB-Konto
Öffnen Sie die leere Datei ```app.js``` im Text-Editor. Fügen Sie den folgenden Code ein, um das ```documentdb```-Modul und das neu erstellte ```config```-Modul zu importieren.

    // ADD THIS PART TO YOUR CODE
    "use strict";

    var documentClient = require("documentdb").DocumentClient;
    var config = require("./config");
    var url = require('url');

Fügen Sie den Code ein, um unter Verwendung der zuvor gespeicherten Eigenschaften ```config.endpoint``` und ```config.primaryKey``` ein neues DocumentClient-Element zu erstellen.

    var config = require("./config");
    var url = require('url');

    // ADD THIS PART TO YOUR CODE
    var client = new documentClient(config.endpoint, { "masterKey": config.primaryKey });

Nachdem Sie nun über den Code zum Initialisieren des Azure Cosmos DB-Clients verfügen, beschäftigen wir uns im nächsten Schritt mit der Verwendung von Azure Cosmos DB-Ressourcen.

## <a name="step-5-create-a-node-database"></a>Schritt 5: Erstellen einer Node-Datenbank
Fügen Sie den folgenden Code ein, um den HTTP-Status für „Nicht gefunden“, die Datenbank-URL und die Sammlungs-URL festzulegen. Anhand dieser URLs ermittelt der Azure Cosmos DB-Client die richtige Datenbank und Sammlung.

    var client = new documentClient(config.endpoint, { "masterKey": config.primaryKey });

    // ADD THIS PART TO YOUR CODE
    var HttpStatusCodes = { NOTFOUND: 404 };
    var databaseUrl = `dbs/${config.database.id}`;
    var collectionUrl = `${databaseUrl}/colls/${config.collection.id}`;

Eine [Datenbank](documentdb-resources.md#databases) kann mithilfe der [createDatabase](https://azure.github.io/azure-documentdb-node/DocumentClient.html)-Funktion der **DocumentClient**-Klasse erstellt werden. Eine Datenbank ist ein logischer Container für Dokumentspeicher, der auf Sammlungen aufgeteilt ist.

Fügen Sie die kopierte **getDatabase**-Funktion zum Erstellen der neuen Datenbank in der Datei „app.js“ mit der ```id``` aus dem ```config```-Objekt hinzu. Die Funktion überprüft, ob die Datenbank mit der gleichen ```FamilyRegistry``` -ID bereits vorhanden ist. Wenn eine vorhanden ist, verwenden wir diese Datenbank, statt eine neue zu erstellen.

    var collectionUrl = `${databaseUrl}/colls/${config.collection.id}`;

    // ADD THIS PART TO YOUR CODE
    function getDatabase() {
        console.log(`Getting database:\n${config.database.id}\n`);

        return new Promise((resolve, reject) => {
            client.readDatabase(databaseUrl, (err, result) => {
                if (err) {
                    if (err.code == HttpStatusCodes.NOTFOUND) {
                        client.createDatabase(config.database, (err, created) => {
                            if (err) reject(err)
                            else resolve(created);
                        });
                    } else {
                        reject(err);
                    }
                } else {
                    resolve(result);
                }
            });
        });
    }

Fügen Sie den folgenden kopierten Code an der Stelle ein, an der die **getDatabase**-Funktion festgelegt wird. Dadurch wird die **exit**-Hilfsfunktion hinzugefügt, die die Beendigungsmeldung ausgibt und die **getDatabase**-Funktion aufruft.

                } else {
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART TO YOUR CODE
    function exit(message) {
        console.log(message);
        console.log('Press any key to exit');
        process.stdin.setRawMode(true);
        process.stdin.resume();
        process.stdin.on('data', process.exit.bind(process, 0));
    }

    getDatabase()
    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

Suchen Sie auf Ihrem Terminal nach der Datei ```app.js```, und führen Sie den Befehl ```node app.js``` aus.

Glückwunsch! Sie haben erfolgreich eine Azure Cosmos DB-Datenbank erstellt.

## <a id="CreateColl"></a>Schritt 6: Erstellen einer Sammlung
> [!WARNING]
> **createCollection** erstellt eine neue Sammlung, die Auswirkungen auf die Preise hat. Weitere Informationen finden Sie auf unserer [Preisseite](https://azure.microsoft.com/pricing/details/cosmos-db/).
> 
> 

Eine [Sammlung](documentdb-resources.md#collections) kann mithilfe der [createCollection](https://azure.github.io/azure-documentdb-node/DocumentClient.html)-Funktion der **DocumentClient**-Klasse erstellt werden. Eine Sammlung ist ein Container für JSON-Dokumente und die zugehörige JavaScript-Anwendungslogik.

Fügen Sie in der Datei „app.js“ unterhalb der **getDatabase**-Funktion die kopierte **getCollection**-Funktion ein, um die neue Sammlung mit der ```id``` aus dem ```config```-Objekt zu erstellen. Auch hier vergewissern wir uns, dass noch keine Sammlung mit der gleichen ```FamilyCollection``` -ID vorhanden ist. Wenn eine vorhanden ist, verwenden wir diese Sammlung, statt eine neue zu erstellen.

                } else {
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART TO YOUR CODE
    function getCollection() {
        console.log(`Getting collection:\n${config.collection.id}\n`);

        return new Promise((resolve, reject) => {
            client.readCollection(collectionUrl, (err, result) => {
                if (err) {
                    if (err.code == HttpStatusCodes.NOTFOUND) {
                        client.createCollection(databaseUrl, config.collection, { offerThroughput: 400 }, (err, created) => {
                            if (err) reject(err)
                            else resolve(created);
                        });
                    } else {
                        reject(err);
                    }
                } else {
                    resolve(result);
                }
            });
        });
    }

Fügen Sie den kopierten Code unterhalb des Aufrufs von **getDatabase** ein, um die **getCollection**-Funktion auszuführen.

    getDatabase()

    // ADD THIS PART TO YOUR CODE
    .then(() => getCollection())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

Suchen Sie auf Ihrem Terminal nach der Datei ```app.js```, und führen Sie den Befehl ```node app.js``` aus.

Glückwunsch! Sie haben erfolgreich eine Azure Cosmos DB-Sammlung erstellt.

## <a id="CreateDoc"></a>Schritt 7: Erstellen eines Dokuments
Ein [Dokument](documentdb-resources.md#documents) kann mithilfe der [createDocument](https://azure.github.io/azure-documentdb-node/DocumentClient.html)-Funktion der **DocumentClient**-Klasse erstellt werden. Dokumente sind benutzerdefinierter (beliebiger) JSON-Inhalt. Sie können nun ein Dokument in Azure Cosmos DB einfügen.

Fügen Sie unterhalb der **getCollection**-Funktion die kopierte **getFamilyDocument**-Funktion ein, um die Dokumente mit den im ```config```-Objekt gespeicherten JSON-Daten zu erstellen. Auch hier stellen wir sicher, dass ein Dokument mit der gleichen ID noch nicht vorhanden ist.

                } else {
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART TO YOUR CODE
    function getFamilyDocument(document) {
        let documentUrl = `${collectionUrl}/docs/${document.id}`;
        console.log(`Getting document:\n${document.id}\n`);

        return new Promise((resolve, reject) => {
            client.readDocument(documentUrl, { partitionKey: document.district }, (err, result) => {
                if (err) {
                    if (err.code == HttpStatusCodes.NOTFOUND) {
                        client.createDocument(collectionUrl, document, (err, created) => {
                            if (err) reject(err)
                            else resolve(created);
                        });
                    } else {
                        reject(err);
                    }
                } else {
                    resolve(result);
                }
            });
        });
    };

Fügen Sie den kopierten Code unterhalb des Aufrufs von **getCollection** ein, um die **getFamilyDocument**-Funktion auszuführen.

    getDatabase()
    .then(() => getCollection())

    // ADD THIS PART TO YOUR CODE
    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

Suchen Sie auf Ihrem Terminal nach der Datei ```app.js```, und führen Sie den Befehl ```node app.js``` aus.

Glückwunsch! Sie haben erfolgreich ein Azure Cosmos DB-Dokument erstellt.

![Node.js-Tutorial – Diagramm zur Veranschaulichung der hierarchischen Beziehung zwischen dem Konto, der Datenbank, der Sammlung und den Dokumenten – Node-Datenbank](./media/documentdb-nodejs-get-started/node-js-tutorial-cosmos-db-account.png)

## <a id="Query"></a>Schritt 8: Abfragen von Azure Cosmos DB-Ressourcen
Azure Cosmos DB unterstützt umfassende [Abfragen](documentdb-sql-query.md) der in jeder Sammlung gespeicherten JSON-Dokumente. Der folgende Beispielcode zeigt eine Abfrage, die Sie für die Dokumente in Ihrer Sammlung ausführen können.

Fügen Sie die kopierte **queryCollection**-Funktion in der Datei „app.js“ unterhalb der **getFamilyDocument**-Funktion ein. Azure Cosmos DB unterstützt SQL-ähnliche Abfragen, wie unten dargestellt. Weitere Informationen zum Erstellen von komplexen Abfragen finden Sie unter [Query Playground](https://www.documentdb.com/sql/demo) (in englischer Sprache) und in der [Abfragedokumentation](documentdb-sql-query.md).

                } else {
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART TO YOUR CODE
    function queryCollection() {
        console.log(`Querying collection through index:\n${config.collection.id}`);

        return new Promise((resolve, reject) => {
            client.queryDocuments(
                collectionUrl,
                'SELECT VALUE r.children FROM root r WHERE r.lastName = "Andersen"'
            ).toArray((err, results) => {
                if (err) reject(err)
                else {
                    for (var queryResult of results) {
                        let resultString = JSON.stringify(queryResult);
                        console.log(`\tQuery returned ${resultString}`);
                    }
                    console.log();
                    resolve(results);
                }
            });
        });
    };


Das folgende Diagramm veranschaulicht, wie die Azure Cosmos DB-SQL-Abfragesyntax für die erstellte Sammlung aufgerufen wird.

![Node.js-Tutorial – Diagramm zur Veranschaulichung des Bereichs und der Bedeutung der Abfrage – Node-Datenbank](./media/documentdb-nodejs-get-started/node-js-tutorial-collection-documents.png)

Das Schlüsselwort [FROM](documentdb-sql-query.md#FromClause) ist in der Abfrage optional, da Azure Cosmos DB bereits Abfragen auf eine einzelne Sammlung begrenzt. Aus diesem Grund ist "FROM Familien f" austauschbar mit "FROM Stamm r" oder einem anderen variablen Namen, den Sie auswählen. Azure Cosmos DB leitet ab, dass Familien, der Stamm oder der variable Name, den Sie ausgewählt haben, standardmäßig auf die aktuelle Sammlung verweist bzw. verweisen.

Fügen Sie den kopierten Code unterhalb des Aufrufs von **getFamilyDocument** ein, um die **queryCollection**-Funktion auszuführen.

    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))

    // ADD THIS PART TO YOUR CODE
    .then(() => queryCollection())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

Suchen Sie auf Ihrem Terminal nach der Datei ```app.js```, und führen Sie den Befehl ```node app.js``` aus.

Glückwunsch! Sie haben erfolgreich Azure Cosmos DB-Dokumente abgefragt.

## <a id="ReplaceDocument"></a>Schritt 9: Ersetzen eines Dokuments
Azure Cosmos DB unterstützt das Ersetzen von JSON-Dokumenten.

Fügen Sie die kopierte **replaceFamilyDocument**-Funktion in der Datei „app.js“ unterhalb der **queryCollection**-Funktion ein.

                    }
                    console.log();
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART TO YOUR CODE
    function replaceFamilyDocument(document) {
        let documentUrl = `${collectionUrl}/docs/${document.id}`;
        console.log(`Replacing document:\n${document.id}\n`);
        document.children[0].grade = 6;

        return new Promise((resolve, reject) => {
            client.replaceDocument(documentUrl, document, (err, result) => {
                if (err) reject(err);
                else {
                    resolve(result);
                }
            });
        });
    };

Fügen Sie den kopierten Code unterhalb des Aufrufs von **queryCollection** ein, um die **replaceDocument**-Funktion auszuführen. Fügen Sie außerdem den Code zum erneuten Aufrufen von **queryCollection** hinzu, um sich zu vergewissern, dass das Dokument erfolgreich geändert wurde.

    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))
    .then(() => queryCollection())

    // ADD THIS PART TO YOUR CODE
    .then(() => replaceFamilyDocument(config.documents.Andersen))
    .then(() => queryCollection())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

Suchen Sie auf Ihrem Terminal nach der Datei ```app.js```, und führen Sie den Befehl ```node app.js``` aus.

Glückwunsch! Sie haben erfolgreich ein Azure Cosmos DB-Dokument ersetzt.

## <a id="DeleteDocument"></a>Schritt 10: Löschen eines Dokuments
Azure Cosmos DB unterstützt das Löschen von JSON-Dokumenten.

Fügen Sie die kopierte **deleteFamilyDocument**-Funktion unterhalb der **replaceFamilyDocument**-Funktion ein.

                else {
                    resolve(result);
                }
            });
        });
    };

    // ADD THIS PART TO YOUR CODE
    function deleteFamilyDocument(document) {
        let documentUrl = `${collectionUrl}/docs/${document.id}`;
        console.log(`Deleting document:\n${document.id}\n`);

        return new Promise((resolve, reject) => {
            client.deleteDocument(documentUrl, (err, result) => {
                if (err) reject(err);
                else {
                    resolve(result);
                }
            });
        });
    };

Fügen Sie den kopierten Code unterhalb des zweiten Aufrufs von **queryCollection** ein, um die **deleteDocument**-Funktion auszuführen.

    .then(() => queryCollection())
    .then(() => replaceFamilyDocument(config.documents.Andersen))
    .then(() => queryCollection())

    // ADD THIS PART TO YOUR CODE
    .then(() => deleteFamilyDocument(config.documents.Andersen))
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

Suchen Sie auf Ihrem Terminal nach der Datei ```app.js```, und führen Sie den Befehl ```node app.js``` aus.

Glückwunsch! Sie haben erfolgreich ein Azure Cosmos DB-Dokument gelöscht.

## <a id="DeleteDatabase"></a>Schritt 11: Löschen der Node-Datenbank
Das Löschen der erstellten Datenbank entfernt die Datenbank und alle untergeordneten Ressourcen (Sammlungen, Dokumente usw.).

Kopieren Sie die **cleanup**-Funktion, und fügen Sie sie unterhalb der **deleteFamilyDocument**-Funktion ein, um die Datenbank und alle untergeordneten Ressourcen zu entfernen.

                else {
                    resolve(result);
                }
            });
        });
    };

    // ADD THIS PART TO YOUR CODE
    function cleanup() {
        console.log(`Cleaning up by deleting database ${config.database.id}`);

        return new Promise((resolve, reject) => {
            client.deleteDatabase(databaseUrl, (err) => {
                if (err) reject(err)
                else resolve(null);
            });
        });
    }

Fügen Sie den kopierten Code unterhalb des Aufrufs von **deleteFamilyDocument** ein, um die **cleanup**-Funktion auszuführen.

    .then(() => deleteFamilyDocument(config.documents.Andersen))

    // ADD THIS PART TO YOUR CODE
    .then(() => cleanup())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

## <a id="Run"></a>Schritt 12: Ausführen der gesamten Node.js-Anwendung
Die Aufrufsequenz der Funktionen muss wie folgt aussehen:

    getDatabase()
    .then(() => getCollection())
    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))
    .then(() => queryCollection())
    .then(() => replaceFamilyDocument(config.documents.Andersen))
    .then(() => queryCollection())
    .then(() => deleteFamilyDocument(config.documents.Andersen))
    .then(() => cleanup())
    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

Suchen Sie auf Ihrem Terminal nach der Datei ```app.js```, und führen Sie den Befehl ```node app.js``` aus.

Die Ausgabe der GetStarted-Anwendung sollte angezeigt werden. Die Ausgabe sollte dem folgenden Beispieltext entsprechen.

    Getting database:
    FamilyDB

    Getting collection:
    FamilyColl

    Getting document:
    Anderson.1

    Getting document:
    Wakefield.7

    Querying collection through index:
    FamilyColl
        Query returned [{"firstName":"Henriette Thaulow","gender":"female","grade":5,"pets":[{"givenName":"Fluffy"}]}]

    Replacing document:
    Anderson.1

    Querying collection through index:
    FamilyColl
        Query returned [{"firstName":"Henriette Thaulow","gender":"female","grade":6,"pets":[{"givenName":"Fluffy"}]}]

    Deleting document:
    Anderson.1

    Cleaning up by deleting database FamilyDB
    Completed successfully
    Press any key to exit

Glückwunsch! Sie haben das Node.js-Tutorial abgeschlossen und Ihre erste Azure Cosmos DB-Konsolenanwendung erstellt.

## <a id="GetSolution"></a>Abrufen der vollständigen Projektmappe für das Node.js-Tutorial
Wenn Sie keine Zeit hatten, die Schritte in diesem Tutorial auszuführen, oder nur den Code herunterladen möchten, finden Sie ihn auf [GitHub](https://github.com/Azure-Samples/documentdb-node-getting-started).

Zum Ausführen der GetStarted-Lösung, die alle Beispiele dieses Artikels enthält, ist Folgendes erforderlich:

* [Azure Cosmos DB-Konto][create-account].
* [GetStarted-Projektmappe](https://github.com/Azure-Samples/documentdb-node-getting-started) (erhältlich auf GitHub)

Installieren Sie das **documentdb** -Modul über npm. Verwenden Sie den folgenden Befehl:

* ```npm install documentdb --save```

Aktualisieren Sie dann in der Datei ```config.js``` die Werte für „config.endpoint“ und „config.primaryKey“, wie unter [Schritt 3: Festlegen der Konfigurationen der App](#Config) beschrieben. 

Suchen Sie dann auf Ihrem Terminal nach der Datei ```app.js```, und führen Sie den Befehl ```node app.js``` aus.

Das ist alles, nun müssen Sie nur noch die Erstellung durchführen. 

## <a name="next-steps"></a>Nächste Schritte
* Benötigen Sie ein komplexeres Node.js-Beispiel? Sie finden eines unter [Erstellen einer Node.js-Webanwendung mithilfe von Azure Cosmos DB](documentdb-nodejs-application.md).
* Informieren Sie sich über das [Überwachen eines Azure Cosmos DB-Kontos](monitor-accounts.md).
* Fragen Sie unser Beispieldataset im [Query Playground](https://www.documentdb.com/sql/demo)ab.
* Weitere Informationen zum Programmiermodell finden Sie auf der [Azure Cosmos DB-Dokumentationsseite](https://azure.microsoft.com/documentation/services/documentdb/) im Abschnitt „Develop“ (Entwickeln).

[create-account]: create-documentdb-dotnet.md#create-account
[keys]: media/documentdb-nodejs-get-started/node-js-tutorial-keys.png

