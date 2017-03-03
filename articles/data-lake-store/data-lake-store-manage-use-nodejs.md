---
title: "Erste Schritte mit Azure Data Lake Store mithilfe des Azure SDK für Node.js | Microsoft Docs"
description: "Erfahren Sie mehr über die Verwendung von Node.js, um mit Data Lake Store-Konten und dem Dateisystem zu arbeiten."
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 2fee173c-69ae-4e1d-8773-48618cda9e16
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/31/2017
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: f33ccee7dd520adf074856616005c929040116dd
ms.openlocfilehash: 091ab246826c96b9d816c87b27014c1e54039429
ms.lasthandoff: 02/01/2017


---
# <a name="get-started-with-azure-data-lake-store-using-azure-sdk-for-nodejs"></a>Erste Schritte mit Azure Data Lake Store mithilfe des Azure SDK für Node.js
> [!div class="op_single_selector"]
> * [Portal](data-lake-store-get-started-portal.md)
> * [PowerShell](data-lake-store-get-started-powershell.md)
> * [.NET SDK](data-lake-store-get-started-net-sdk.md)
> * [Java SDK](data-lake-store-get-started-java-sdk.md)
> * [REST-API](data-lake-store-get-started-rest-api.md)
> * [Azure-Befehlszeilenschnittstelle](data-lake-store-get-started-cli.md)
> * [Node.js](data-lake-store-manage-use-nodejs.md)
> * [Python](data-lake-store-get-started-python.md)
>
> 

> [!NOTE]
> Für das Hochladen und Herunterladen großer Datenmengen (große Dateien, eine große Anzahl von Dateien oder beides) empfehlen wir die Verwendung von [Python-SDK](data-lake-store-get-started-python.md), [.NET-SDK](data-lake-store-get-started-net-sdk.md) oder [Azure PowerShell](data-lake-store-get-started-powershell.md). Diese Optionen bieten bessere Leistung, weil sie mehrere Threads verwenden, um die Datenverschiebung zu parallelisieren.
> 
> 

Hier erfahren Sie, wie Sie im Azure SDK für Node.js ein Azure Data Lake Store-Konto erstellen und grundlegende Vorgänge ausführen, z.B. Ordner erstellen, Datendateien hoch- und herunterladen, Ihr Konto löschen usw. Weitere Informationen zu Data Lake Store finden Sie unter [Übersicht über Data Lake Store](data-lake-store-overview.md). Das SDK unterstützt derzeit

* **Node.js-Version: 0.10.0 oder höher**
* **REST-API-Version für Konten: 2015-10-01-preview**
* **REST-API-Version für das Dateisystem: 2015-10-01-preview**

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie mit diesem Artikel beginnen können, benötigen Sie Folgendes:

* **Ein Azure-Abonnement**. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).
* **Erstellen einer Azure Active Directory-Anwendung**. Die Azure AD-Anwendung wird verwendet, um die Data Lake Store-Anwendung bei Azure AD zu authentifizieren. Für die Authentifizierung bei Azure AD stehen zwei unterschiedliche Konzepte zur Verfügung: **Endbenutzerauthentifizierung** und **Dienst-zu-Dienst-Authentifizierung**. Anweisungen und weitere Informationen zur Authentifizierung finden Sie unter [Authenticate with Data Lake Store using Azure Active Directory](data-lake-store-authenticate-using-active-directory.md)(Authentifizieren bei Data Lake Store mithilfe von Azure Active Directory).

## <a name="how-to-install"></a>Installation
```bash
npm install azure-arm-datalake-store
```

## <a name="authenticate-using-azure-active-directory"></a>Authentifizieren mit Azure Active Directory
Die folgenden Codeausschnitte zeigen zwei unterschiedliche Methoden zum Authentifizieren bei Data Lake Store mithilfe von Azure AD. Eine ausführliche Erläuterung der verschiedenen Methoden zur Authentifizierung bei Data Lake Store finden Sie unter [Authentifizieren bei Data Lake Store mithilfe von Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

Der Codeausschnitt unten erfordert Eingaben wie den Azure AD-Domänennamen, die Client-ID für eine Azure AD-App usw. Alle diese Details können aus einer Azure AD-Anwendung abgerufen werden, die Sie erstellen müssen. Die entsprechenden Details finden Sie ebenfalls unter dem oben aufgeführten Link.

 ```javascript
 var msrestAzure = require('ms-rest-azure');
 //user authentication
 var credentials = new msRestAzure.UserTokenCredentials('your-client-id', 'your-domain', 'your-username', 'your-password', 'your-redirect-uri');
 //service principal authentication
 var credentials = new msRestAzure.ApplicationTokenCredentials('your-client-id', 'your-domain', 'your-secret');
 ```

## <a name="create-the-data-lake-store-clients"></a>Erstellen des Data Lake Store-Clients
```javascript
var adlsManagement = require("azure-arm-datalake-store");
var acccountClient = new adlsManagement.DataLakeStoreAccountClient(credentials, "your-subscription-id");
var filesystemClient = new adlsManagement.DataLakeStoreFileSystemClient(credentials);
```

## <a name="create-a-data-lake-store-account"></a>Erstellen eines Data Lake-Speicherkontos
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

## <a name="create-a-file-with-content"></a>Erstellen einer Datei mit Inhalt
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

## <a name="get-a-list-of-files-and-folders"></a>Abrufen einer Liste von Dateien und Ordnern
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

## <a name="see-also"></a>Siehe auch
* [Microsoft Azure SDK für Node.js](https://github.com/azure/azure-sdk-for-node)
* [Microsoft Azure SDK für Node.js – Data Lake Analytics-Verwaltung](https://www.npmjs.com/package/azure-arm-datalake-analytics)


