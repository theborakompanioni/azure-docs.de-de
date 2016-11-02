<properties 
   pageTitle="Verwalten von Azure Data Lake-Speichern mithilfe des Azure SDK für Node.js | Microsoft Azure"
   description="Erfahren Sie mehr über das Verwalten von Data Lake-Speicher und dem Dateisystem." 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/13/2016"
   ms.author="nitinme"/>

# Verwalten von Azure Data Lake-Speicher mithilfe des Azure SDK für Node.js

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST-API](data-lake-store-get-started-rest-api.md)
- [Azure-Befehlszeilenschnittstelle](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)


Das Azure SDK für Node.js kann für die Verwaltung von Azure Data Lake-Speicherkonten und Dateisystemvorgänge verwendet werden.

Momentan werden folgende Versionen unterstützt:

  *  **Node.js-Version: 0.10.0 oder höher**
  *  **REST-API-Version für Konten: 2015-10-01-preview**
  *  **REST-API-Version für das Dateisystem: 2015-10-01-preview**

##Voraussetzungen

Bevor Sie mit diesem Artikel beginnen können, benötigen Sie Folgendes:

- **Ein Azure-Abonnement**. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).

## Features

- Kontoverwaltung: Erstellen, Abrufen, Auflisten, Aktualisieren und Löschen
- Dateisystemverwaltung: Erstellen, Abrufen, Hochladen, Anfügen, Herunterladen, Lesen, Löschen, Auflisten.

## Installation

```bash
npm install azure-arm-datalake-store
```

## Authentifizieren mit Azure Active Directory

 ```javascript
 var msrestAzure = require('ms-rest-azure');
 //user authentication
 var credentials = new msRestAzure.UserTokenCredentials('your-client-id', 'your-domain', 'your-username', 'your-password', 'your-redirect-uri');
 //service principal authentication
 var credentials = new msRestAzure.ApplicationTokenCredentials('your-client-id', 'your-domain', 'your-secret');
 ```

## Erstellen des Data Lake Analytics-Clients

```javascript
var adlsManagement = require("azure-arm-datalake-store");
var acccountClient = new adlsManagement.DataLakeStoreAccountClient(credentials, "your-subscription-id");
var filesystemClient = new adlsManagement.DataLakeStoreFileSystemClient(credentials);
```

## Erstellen eines Data Lake-Speicherkontos

```javascript
var util = require('util');
var resourceGroupName = 'testrg';
var accountName = 'testadlsacct';
var location = 'eastus2';

// account object to create
var accountToCreate = {
  tags: {
    testtag1: 'testvalue1',
    testtag2: 'testvalue2'
  },
  name: accountName,
  location: location
};

client.account.create(resourceGroupName, accountName, accountToCreate, function (err, result, request, response) {
  if (err) {
    console.log(err);
    /*err has reference to the actual request and response, so you can see what was sent and received on the wire.
      The structure of err looks like this:
      err: {
        code: 'Error Code',
        message: 'Error Message',
        body: 'The response body if any',
        request: reference to a stripped version of http request
        response: reference to a stripped version of the response
      }
    */
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## Erstellen einer Datei mit Inhalt
```javascript
var util = require('util');
var accountName = 'testadlsacct';
var fileToCreate = '/myfolder/myfile.txt';
var options = {
  streamContents: new Buffer('some string content')
}

filesystemClient.fileSystem.listFileStatus(accountName, fileToCreate, options, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    // no result is returned, only a 201 response for success.
    console.log('response is: ' + util.inspect(response, {depth: null}));
  }
});
```

## Abrufen einer Liste von Dateien und Ordnern

```javascript
var util = require('util');
var accountName = 'testadlsacct';
var pathToEnumerate = '/myfolder';
filesystemClient.fileSystem.listFileStatus(accountName, pathToEnumerate, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## Siehe auch

- [Microsoft Azure SDK für Node.js](https://github.com/azure/azure-sdk-for-node)
- [Microsoft Azure SDK für Node.js – Data Lake Analytics-Verwaltung](https://www.npmjs.com/package/azure-arm-datalake-analytics)

<!---HONumber=AcomDC_0914_2016-->