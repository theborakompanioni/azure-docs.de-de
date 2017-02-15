---
title: "Verwalten von Azure Data Lake Analytics mithilfe des Azure SDK für Node.js | Microsoft Docs"
description: "Hier erfahren Sie, wie Sie mithilfe des Azure SDK für Node.js Data Lake Analytics-Konten, -Datenquellen, -Aufträge und -Benutzer verwalten."
services: data-lake-analytics
documentationcenter: 
author: edmacauley
manager: jhubbard
editor: cgronlun
ms.assetid: 9de1bcf4-b15b-4d0b-9284-8889ecf0c438
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/05/2016
ms.author: edmaca
translationtype: Human Translation
ms.sourcegitcommit: 194b5d79505afbfd0208f63dd182a0e03227ba69
ms.openlocfilehash: e6440522ced33a48925cfabc64da055b8700b253


---
# <a name="manage-azure-data-lake-analytics-using-azure-sdk-for-nodejs"></a>Verwalten von Azure Data Lake Analytics mithilfe des Azure SDK für Node.js
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Das Azure SDK für Node.js kann für die Verwaltung von Azure Data Lake Analytics-Konten, -Aufträgen und -Katalogen verwendet werden. Wenn Sie das Verwaltungsthema für andere Tools anzeigen möchten, klicken Sie weiter oben auf die gewünschte Registerkarte.

Momentan werden folgende Versionen unterstützt:

* **Node.js-Version: 0.10.0 oder höher**
* **REST-API-Version für Konten: 2015-10-01-preview**
* **REST-API-Version für Kataloge: 2015-10-01-preview**
* **REST-API-Version für Aufträge: 2016-03-20-preview**

## <a name="features"></a>Features
* Kontoverwaltung: Erstellen, Abrufen, Auflisten, Aktualisieren und Löschen
* Auftragsverwaltung: Senden, Abrufen, Auflisten, Abbrechen
* Katalogverwaltung: Abrufen, Auflisten, Erstellen (geheimer Schlüssel), Aktualisieren (geheimer Schlüssel), Löschen (geheimer Schlüssel)

## <a name="how-to-install"></a>Installation
```bash
npm install azure-arm-datalake-analytics
```

## <a name="authenticate-using-azure-active-directory"></a>Authentifizieren mit Azure Active Directory
 ```javascript
 var msrestAzure = require('ms-rest-azure');
 //user authentication
 var credentials = new msRestAzure.UserTokenCredentials('your-client-id', 'your-domain', 'your-username', 'your-password', 'your-redirect-uri');
 //service principal authentication
 var credentials = new msRestAzure.ApplicationTokenCredentials('your-client-id', 'your-domain', 'your-secret');
 ```

## <a name="create-the-data-lake-analytics-client"></a>Erstellen des Data Lake Analytics-Clients
```javascript
var adlaManagement = require("azure-arm-datalake-analytics");
var acccountClient = new adlaManagement.DataLakeAnalyticsAccountClient(credentials, 'your-subscription-id');
var jobClient = new adlaManagement.DataLakeAnalyticsJobClient(credentials, 'azuredatalakeanalytics.net');
var catalogClient = new adlaManagement.DataLakeAnalyticsCatalogClient(credentials, 'azuredatalakeanalytics.net');
```

## <a name="create-a-data-lake-analytics-account"></a>Erstellen eines Data Lake Analytics-Kontos
```javascript
var util = require('util');
var resourceGroupName = 'testrg';
var accountName = 'testadlaacct';
var location = 'eastus2';

// A Data Lake Store account must already have been created to create
// a Data Lake Analytics account. See the Data Lake Store readme for
// information on doing so. For now, we assume one exists already.
var datalakeStoreAccountName = 'existingadlsaccount';

// account object to create
var accountToCreate = {
  tags: {
    testtag1: 'testvalue1',
    testtag2: 'testvalue2'
  },
  name: accountName,
  location: location,
  properties: {
    defaultDataLakeStoreAccount: datalakeStoreAccountName,
    dataLakeStoreAccounts: [
      {
        name: datalakeStoreAccountName
      }
    ]
  }
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

## <a name="get-a-list-of-jobs"></a>Abrufen einer Liste von Aufträgen
```javascript
var util = require('util');
var accountName = 'testadlaacct';
jobClient.job.list(accountName, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## <a name="get-a-list-of-databases-in-the-data-lake-analytics-catalog"></a>Abrufen einer Liste von Datenbanken im Data Lake Analytics-Katalog
```javascript
var util = require('util');
var accountName = 'testadlaacct';
catalogClient.catalog.listDatabases(accountName, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## <a name="see-also"></a>Siehe auch
* [Microsoft Azure SDK für Node.js](https://github.com/azure/azure-sdk-for-node)
* [Microsoft Azure SDK für Node.js – Data Lake-Speicherverwaltung](https://github.com/Azure/azure-sdk-for-node/tree/autorest/lib/services/dataLake.Store)




<!--HONumber=Dec16_HO2-->


