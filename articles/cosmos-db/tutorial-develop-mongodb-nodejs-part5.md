---
title: "MongoDB-, Angular- und Note-Tutorial für Azure – Teil 5 | Microsoft-Dokumentation"
description: "Teil 5 der Tutorialreihe zur Erstellung einer MongoDB-App mit Angular und Note in Azure Cosmos DB unter Verwendung der gleichen APIs wie für MongoDB"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 
ms.service: cosmos-db
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: hero-article
ms.date: 09/05/2017
ms.author: mimig
ms.translationtype: HT
ms.sourcegitcommit: 4eb426b14ec72aaa79268840f23a39b15fee8982
ms.openlocfilehash: e752e18f6d579633c0cf553224ae7617b774ad0f
ms.contentlocale: de-de
ms.lasthandoff: 09/06/2017

---
# <a name="create-a-mongodb-app-with-angular-and-azure-cosmos-db---part-5-use-mongoose-to-connect-to-azure-cosmos-db"></a>Erstellen einer MongoDB-App mit Angular und Azure Cosmos DB – Teil 5: Herstellen einer Verbindung mit Azure Cosmos DB mithilfe von Mongoose

In diesem mehrteiligen Tutorial erfahren Sie, wie Sie eine neue, in Node.js geschriebene [MongoDB-API](mongodb-introduction.md)-App mit Express, Angular Ihrer Azure Cosmos DB-Datenbank erstellen.

Teil 5 des Tutorials baut auf [Teil 4](tutorial-develop-mongodb-nodejs-part4.md) auf und umfasst folgende Aufgaben:

> [!div class="checklist"]
> * Herstellen einer Verbindung mit Azure Cosmos DB mithilfe von Mongoose
> * Abrufen von Informationen zur Verbindungszeichenfolge aus Azure Cosmos DB
> * Erstellen des hero-Modells
> * Erstellen des hero-Diensts zum Abrufen von Heldendaten
> * Lokales Ausführen der App

## <a name="video-walkthrough"></a>Exemplarische Vorgehensweise per Video

> [!VIDEO https://www.youtube.com/embed/sI5hw6KPPXI]


## <a name="prerequisites"></a>Voraussetzungen

Für diesen Teil des Tutorials wird vorausgesetzt, dass Sie die Schritte aus [Teil 4](tutorial-develop-mongodb-nodejs-part4.md) des Tutorials ausgeführt haben.

> [!TIP]
> In diesem Tutorial wird die Anwendungserstellung Schritt für Schritt erläutert. Für den Fall, dass Sie das fertige Projekt herunterladen möchten, steht die fertige Anwendung auf GitHub im [Repository „angular-cosmosdb“](https://github.com/Azure-Samples/angular-cosmosdb) zur Verfügung.

## <a name="use-mongoose-to-connect-to-azure-cosmos-db"></a>Herstellen einer Verbindung mit Azure Cosmos DB mithilfe von Mongoose

1. Installieren Sie das Mongoose-npm-Modul (eine API, die normalerweise für die Kommunikation mit MongoDB verwendet wird).

    ```bash
    npm i mongoose --save
    ```

2. Erstellen Sie als Nächstes im Ordner **server** eine neue Datei namens **mongo.js**. Dieser Datei fügen Sie alle Ihre Verbindungsinformationen für die Azure Cosmos DB-Datenbank hinzu.

3. Kopieren Sie den unten angegebenen Code in **mongo.js**. Für diesen Code gilt Folgendes:
    * Er erfordert Mongoose.
    * Er setzt die MongoDB-Zusage außer Kraft, um die grundlegende Zusage zu verwenden, die in ES6/ES2015 und höhere Versionen integriert ist.
    * Er ruft eine env-Datei auf, mit der Sie bestimmte Aspekte abhängig von der aktuellen Phase (Staging, Produktion oder Entwicklung) einrichten können. Diese Datei erstellen wir in Kürze.
    * Er enthält unsere MongoDB-Verbindungszeichenfolge, die in der env-Datei festgelegt wird.
    * Er erstellt eine connect-Funktion, die Mongoose aufruft.

    ```javascript
    const mongoose = require('mongoose');
    /**
     * Set to Node.js native promises
     * Per http://mongoosejs.com/docs/promises.html
     */
    mongoose.Promise = global.Promise;

    const env = require('./env/environment');

    // eslint-disable-next-line max-len
    const mongoUri = `mongodb://${env.dbName}:${env.key}@${env.dbName}.documents.azure.com:${env.cosmosPort}/?ssl=true`; //&replicaSet=globaldb`;

    function connect() {
     mongoose.set('debug', true);
     return mongoose.connect(mongoUri, { useMongoClient: true });
    }

    module.exports = {
      connect,
      mongoose
    };
    ```
    
4. Erstellen Sie im Explorer-Bereich unter **server** einen Ordner namens **environment** und im Ordner **environment** eine neue Datei namens **environment.js**.

5. Aus der Datei „mongo.js“ wissen wir, dass wir `dbName`, `key`, und `cosmosPort` einschließen müssen. Kopieren Sie daher den folgenden Code in **environment.js**:

    ```javascript
    const cosmosPort = 1234; // replace with your port
    const dbName = 'your-cosmos-db-name-goes-here';
    const key = 'your-key-goes-here';

    module.exports = {
      dbName,
      key,
      cosmosPort
    };
    ```

## <a name="get-the-connection-string-information"></a>Abrufen der Informationen zur Verbindungszeichenfolge

1. Legen Sie in **environment.js** den Wert von `cosmosPort` auf „10255“ fest. (Ihren Cosmos DB-Port können Sie im Azure-Portal ermitteln.)

    ```javascript
    const cosmosPort = 10255;
    ```

2. Legen Sie in **environment.js** den Wert von `dbName` auf den Namen des Azure Cosmos DB-Kontos fest, das Sie in [Schritt 4](tutorial-develop-mongodb-nodejs-part4.md) erstellt haben. 

3. Rufen Sie den Primärschlüssel für das Azure Cosmos DB-Konto mithilfe des folgenden CLI-Befehls im Terminalfenster ab: 

    ```azure-cli-interactive
    az cosmosdb list-keys --name <cosmosdb-name> -g myResourceGroup
    ```    
    
    * `<cosmosdb-name>` ist der Name des Azure Cosmos DB-Kontos, das Sie in [Schritt 4](tutorial-develop-mongodb-nodejs-part4.md) erstellt haben.

4. Kopieren Sie den Primärschlüssel als Wert für `key` in die Datei „environment.js“.

    Ihre App verfügt nun über alle Informationen, die sie zum Herstellen einer Verbindung mit Azure Cosmos DB benötigt. Diese Informationen können auch über das Portal abgerufen werden. Weitere Informationen finden Sie unter [Abrufen der MongoDB-Verbindungszeichenfolge zum Anpassen](connect-mongodb-account.md#GetCustomConnection). Der Benutzername im Portal entspricht dem dbName-Wert in „environments.js“. 

## <a name="create-a-hero-model"></a>Erstellen eines hero-Modells

1.  Erstellen Sie im Explorer-Bereich unter dem Ordner **server** die Datei **hero.model.js**.

2. Kopieren Sie den unten angegebenen Code in **hero.model.js**. Für diesen Code gilt Folgendes:
   * Er erfordert Mongoose.
   * Er erstellt ein neues Schema mit einer ID, einem Namen und einem Spruch.
   * Er erstellt ein Modell mit dem Schema.
   * Er exportiert das Modell. 
   * Nennen Sie die Sammlung „Heroes“ (anstelle von „Heros“, was gemäß den Mongoose-Benennungsregeln der standardmäßige Pluralname der Auflistung wäre).

   ```javascript
   const mongoose = require('mongoose');

   const Schema = mongoose.Schema;

   const heroSchema = new Schema(
     {
       id: { type: Number, required: true, unique: true },
       name: String,
       saying: String
     },
     {
       collection: 'Heroes'
     }
   );

   const Hero = mongoose.model('Hero', heroSchema);

   module.exports = Hero;
   ```

## <a name="create-a-hero-service"></a>Erstellen eines hero-Diensts

1.  Erstellen Sie im Explorer-Bereich unter dem Ordner **server** die Datei **hero.service.js**.

2. Kopieren Sie den unten angegebenen Code in **hero.service.js**. Für diesen Code gilt Folgendes:
   * Er ruft das Modell ab, das Sie gerade erstellt haben.
   * Er stellt die Verbindung mit der Datenbank her.
   * Er erstellt eine docquery-Variable, die mithilfe der hero.find-Methode eine Abfrage definiert, die alle Helden zurückgibt.
   * Er führt eine Abfrage mit „docquery.exec“ und einer Zusage zum Abrufen einer Liste mit allen Helden ab, wobei der Antwortstatus 200 lauten muss. 
   * Lautet der Status 500, wird die entsprechende Fehlermeldung zurückgegeben.
   * Da wir Module verwenden, ruft er die Helden ab. 

   ```javascript
   const Hero = require('./hero.model');

   require('./mongo').connect();

   function getHeroes() {
     const docquery = Hero.find({});
     docquery
       .exec()
       .then(heroes => {
         res.status(200).json(heroes);
       })
       .catch(error => {
         res.status(500).send(error);
         return;
       });
   }

   module.exports = {
     getHeroes
   };
   ```

## <a name="add-the-hero-service-to-routesjs"></a>Hinzufügen des hero-Diensts zu „routes.js“

1. Kommentieren Sie in Visual Studio Code in der Datei **routes.js** die Funktion `res.send` aus, die die Beispieldaten für die Helden sendet, und fügen Sie stattdessen eine Zeile mit einem Aufruf der Funktion `heroService.getHeroes` hinzu.

    ```javascript
    router.get('/heroes', (req, res) => {
      heroService.getHeroes(req, res);
    //  res.send(200, [
    //      {"id": 10, "name": "Starlord", "saying": "oh yeah"}
    //  ])
    });
    ```

2. Machen Sie in **routes.js** den hero-Dienst erforderlich:

    ```javascript
    const heroService = require('./hero.service'); 
    ```

3. Aktualisieren Sie in **hero.service.js** die getHeroes-Funktion mit den Parametern `req` und `res`:

    ```javascript
    function getHeroes(req, res) {
    ```

    An dieser Stelle sollten wir uns kurz die Aufrufkette genauer ansehen. Zuerst wird durch `index.js` der Knotenserver eingerichtet. (Das bedeutet, dass unsere Routen eingerichtet und definiert werden.) Anschließend kommuniziert die Datei „routes.js“ mit dem hero-Dienst und weist ihn an, unsere Funktionen wie „getHeroes“ abzurufen und die Anforderung und Antwort zu übergeben. Hier ruft „hero.service.js“ das Modell ab und stellt eine Verbindung mit Mongo her. Anschließend wird „getHeroes“ ausgeführt, wenn wir die Funktion aufrufen, und die Antwort „200“ zurückgegeben. Danach verlässt er die Kette wieder. 

## <a name="run-the-app"></a>Ausführen der App

1. Führen Sie die App erneut aus. Speichern Sie alle Ihre Änderungen in Visual Studio Code. Klicken Sie im linken Bereich auf die Schaltfläche **Debuggen** ![Symbol „Debuggen“ in Visual Studio Code](./media/tutorial-develop-mongodb-nodejs-part5/debug-button.png) und anschließend auf die Schaltfläche **Debugging starten** ![Symbol „Debugging starten“ in Visual Studio Code](./media/tutorial-develop-mongodb-nodejs-part5/start-debugging-button.png).

3. Wechseln Sie zum Browser. Öffnen Sie die Entwicklungstools und die Registerkarte „Netzwerk“, und navigieren Sie zu http://localhost:3000, um zur Anwendung zu gelangen.

    ![Neues Azure Cosmos DB-Konto im Azure-Portal](./media/tutorial-develop-mongodb-nodejs-part5/azure-cosmos-db-heroes-app.png)

   In der App sind noch keine Helden gespeichert. Im nächsten Schritt des Tutorials fügen wir daher Put-, Push- und Löschfunktionen hinzu, um Helden über die Benutzeroberfläche unter Verwendung von Mongoose-Verbindungen mit unserer Azure Cosmos DB-Datenbank hinzufügen, aktualisieren und löschen zu können. 

## <a name="next-steps"></a>Nächste Schritte

In diesem Teil des Tutorials haben Sie die folgenden Aufgaben ausgeführt:

> [!div class="checklist"]
> * Herstellen einer Verbindung zwischen Ihrer Helden-App und Azure Cosmos DB mithilfe von Mongoose-APIs 
> * Hinzufügen der Heldenabruffunktion zu Ihrer App

Im nächsten Teil des Tutorials werden der App Post-, Put- und Delete-Funktionen hinzugefügt.

> [!div class="nextstepaction"]
> [Hinzufügen von Post-, Put- und Delete-Funktionen zur App](tutorial-develop-mongodb-nodejs-part6.md)

