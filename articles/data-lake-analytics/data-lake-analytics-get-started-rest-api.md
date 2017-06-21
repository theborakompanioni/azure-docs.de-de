---
title: Erste Schritte mit Data Lake Analytics mit REST-APIs | Microsoft Docs
description: "Verwenden von WebHDFS-REST-APIs, um Vorgänge in Data Lake Analytics auszuführen"
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.assetid: 5e133d92-baaa-44c9-890c-ab2d85c91122
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/03/2017
ms.author: jgao
ms.translationtype: Human Translation
ms.sourcegitcommit: a1ba750d2be1969bfcd4085a24b0469f72a357ad
ms.openlocfilehash: 332d7af2539eea8890745005104ac5b0921c2b7f
ms.contentlocale: de-de
ms.lasthandoff: 06/20/2017


---
# <a name="get-started-with-azure-data-lake-analytics-using-rest-apis"></a>Erste Schritte mit Azure Data Lake Analytics mit REST-APIs
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Es wird beschrieben, wie Sie WebHDFS-REST-APIs und Data Lake Analytics-REST-APIs verwenden, um Data Lake Analytics-Konten, -Aufträge und -Kataloge zu verwalten. 

## <a name="prerequisites"></a>Voraussetzungen
* **Ein Azure-Abonnement**. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).
* **Erstellen einer Azure Active Directory-Anwendung**. Die Azure AD-Anwendung wird verwendet, um die Data Lake Analytics-Anwendung bei Azure AD zu authentifizieren. Für die Authentifizierung bei Azure AD stehen zwei unterschiedliche Konzepte zur Verfügung: **Endbenutzerauthentifizierung** und **Dienst-zu-Dienst-Authentifizierung**. Anweisungen und weitere Informationen zur Authentifizierung finden Sie unter [Authenticate with Data Lake Analytics using Azure Active Directory](../data-lake-store/data-lake-store-authenticate-using-active-directory.md) (Authentifizieren bei Data Lake Analytics mithilfe von Azure Active Directory).
* [cURL](http://curl.haxx.se/). Dieser Artikel zeigt anhand von cURL, wie Sie REST-API-Aufrufe eines Data Lake Analytics-Kontos ausführen.

## <a name="authenticate-with-azure-active-directory"></a>Authentifizieren mit Azure Active Directory
Es gibt zwei Methoden für die Authentifizierung mit Azure Active Directory.

### <a name="end-user-authentication-interactive"></a>Endbenutzerauthentifizierung (interaktiv)
Mit dieser Methode wird der Benutzer in der Anwendung zum Anmelden aufgefordert. Alle Vorgänge werden im Kontext des Benutzers durchgeführt. 

Gehen Sie wie folgt vor, um die interaktive Authentifizierung durchzuführen:

1. Leiten Sie den Benutzer über die Anwendung an die folgende URL um:
   
        https://login.microsoftonline.com/<TENANT-ID>/oauth2/authorize?client_id=<CLIENT-ID>&response_type=code&redirect_uri=<REDIRECT-URI>
   
   > [!NOTE]
   > \<REDIRECT-URI> muss für die Verwendung in einer URL codiert werden. Verwenden Sie für „https://localhost“ also `https%3A%2F%2Flocalhost`.
   > 
   > 
   
    In diesem Tutorial können Sie die Platzhalterwerte in der URL oben ersetzen und in der Adressleiste des Webbrowsers einfügen. Sie werden umgeleitet und authentifizieren sich mit Ihrer Azure-Anmeldung. Nachdem Sie sich erfolgreich angemeldet haben, wird die Antwort in der Adressleiste des Browsers angezeigt. Die Antwort wird im folgenden Format angezeigt:
   
        http://localhost/?code=<AUTHORIZATION-CODE>&session_state=<GUID>
2. Erfassen Sie den in der Antwort angegebenen Autorisierungscode. In diesem Tutorial können Sie den Autorisierungscode aus der Adressleiste des Webbrowsers kopieren und wie nachstehend gezeigt in der POST-Anforderung an den Token-Endpunkt übergeben:
   
        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token \
        -F redirect_uri=<REDIRECT-URI> \
        -F grant_type=authorization_code \
        -F resource=https://management.core.windows.net/ \
        -F client_id=<CLIENT-ID> \
        -F code=<AUTHORIZATION-CODE>
   
   > [!NOTE]
   > In diesem Fall muss \<REDIRECT-URI> nicht codiert werden.
   > 
   > 
3. Die Antwort ist ein JSON-Objekt, das ein Zugriffstoken (z.B. `"access_token": "<ACCESS_TOKEN>"`) und ein Aktualisierungstoken (z.B. `"refresh_token": "<REFRESH_TOKEN>"`) enthält. Die Anwendung verwendet das Zugriffstoken beim Zugriff auf Azure Data Lake-Speicher und das Aktualisierungstoken zum Abrufen eines anderen Zugriffstokens, wenn ein Zugriffstoken abläuft.
   
        {"token_type":"Bearer","scope":"user_impersonation","expires_in":"3599","expires_on":"1461865782","not_before":    "1461861882","resource":"https://management.core.windows.net/","access_token":"<REDACTED>","refresh_token":"<REDACTED>","id_token":"<REDACTED>"}
4. Wenn das Zugriffstoken abläuft, können Sie wie nachstehend gezeigt mithilfe des Aktualisierungstokens ein neues Zugriffstoken anfordern:
   
        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
             -F grant_type=refresh_token \
             -F resource=https://management.core.windows.net/ \
             -F client_id=<CLIENT-ID> \
             -F refresh_token=<REFRESH-TOKEN>

Weitere Informationen zur interaktiven Benutzerauthentifizierung finden Sie unter [Autorisieren des Zugriffs auf Webanwendungen mit OAuth 2.0 und Azure Active Directory](https://msdn.microsoft.com/library/azure/dn645542.aspx).

### <a name="service-to-service-authentication-non-interactive"></a>Dienst-zu-Dienst-Authentifizierung (nicht interaktiv)
Mit dieser Methode stellt die Anwendung eigene Anmeldeinformationen zum Durchführen der Vorgänge bereit. Hierzu müssen Sie eine POST-Anforderung wie unten abgebildet ausgeben: 

    curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
      -F grant_type=client_credentials \
      -F resource=https://management.core.windows.net/ \
      -F client_id=<CLIENT-ID> \
      -F client_secret=<AUTH-KEY>

Die Ausgabe dieser Anforderung enthält ein Autorisierungstoken (gekennzeichnet durch `access-token` in der folgenden Ausgabe), das Sie anschließend mit Ihren REST-API-Aufrufen übergeben. Speichern Sie dieses Authentifizierungstoken in einer Textdatei, die Sie später in diesem Artikel benötigen werden.

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1458245447","not_before":"1458241547","resource":"https://management.core.windows.net/","access_token":"<REDACTED>"}

In diesem Artikel wird der **nicht interaktive** Ansatz verwendet. Weitere Informationen zur nicht interaktiven Authentifizierung (Dienst-zu-Dienst-Aufrufe) finden Sie unter [Aufrufe zwischen Diensten mithilfe von Clientanmeldeinformationen](https://msdn.microsoft.com/library/azure/dn645543.aspx).

## <a name="create-a-data-lake-analytics-account"></a>Erstellen eines Data Lake Analytics-Kontos
Sie müssen eine Azure-Ressourcengruppe und ein Data Lake Store-Konto erstellen, bevor Sie ein Data Lake Analytics-Konto erstellen können.  Weitere Informationen finden Sie unter [Erstellen eines Data Lake-Speicherkontos](../data-lake-store/data-lake-store-get-started-rest-api.md#create-a-data-lake-store-account).

Der folgende Curl-Befehl zeigt, wie Sie ein Konto erstellen:

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -H "Content-Type: application/json" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<NewAzureDataLakeAnalyticsAccountName>?api-version=2016-11-01 -d@"C:\tutorials\adla\CreateDataLakeAnalyticsAccountRequest.json"

Ersetzen Sie \<`REDACTED`\> durch das Autorisierungstoken, \<`AzureSubscriptionID`\> durch Ihre Abonnement-ID, \<`AzureResourceGroupName`\> durch den Namen einer vorhandenen Azure-Ressourcengruppe und \<`NewAzureDataLakeAnalyticsAccountName`\> durch den Namen eines neuen Data Lake Analytics-Kontos. Die Anforderungsnutzlast für diesen Befehl ist in der Datei **CreateDatalakeAnalyticsAccountRequest.json** enthalten, die für den oben genannten Parameter `-d` bereitgestellt wird. Der Inhalt der Datei „input.json“ sieht folgendermaßen aus:

    {  
        "location": "East US 2",  
        "name": "myadla1004",  
        "tags": {},  
        "properties": {  
            "defaultDataLakeStoreAccount": "my1004store",  
            "dataLakeStoreAccounts": [  
                {  
                    "name": "my1004store"  
                }     
            ]
        }  
    }  


## <a name="list-data-lake-analytics-accounts-in-a-subscription"></a>Auflisten von Data Lake Analytics-Konten in einem Abonnement
Der folgende Curl-Befehl veranschaulicht, wie Sie Konten in einem Abonnement auflisten:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/providers/Microsoft.DataLakeAnalytics/Accounts?api-version=2016-11-01

Ersetzen Sie \<`REDACTED`\> durch das Autorisierungstoken und \<`AzureSubscriptionID`\> durch Ihre Abonnement-ID. Die Ausgabe sieht in etwa wie folgt aus:

    {
        "value": [
            {
            "properties": {
                "provisioningState": "Succeeded",
                "state": "Active",
                "endpoint": "myadla0831.azuredatalakeanalytics.net",
                "accountId": "21e74660-0941-4880-ae72-b143c2615ea9",
                "creationTime": "2016-09-01T12:49:12.7451428Z",
                "lastModifiedTime": "2016-09-01T12:49:12.7451428Z"
            },
            "location": "East US 2",
            "tags": {},
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla0831rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla0831",
            "name": "myadla0831",
            "type": "Microsoft.DataLakeAnalytics/accounts"
            },
            {
            "properties": {
                "provisioningState": "Succeeded",
                "state": "Active",
                "endpoint": "myadla1004.azuredatalakeanalytics.net",
                "accountId": "3ff9b93b-11c4-43c6-83cc-276292eeb350",
                "creationTime": "2016-10-04T20:46:42.287147Z",
                "lastModifiedTime": "2016-10-04T20:46:42.287147Z"
            },
            "location": "East US 2",
            "tags": {},
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004",
            "name": "myadla1004",
            "type": "Microsoft.DataLakeAnalytics/accounts"
            }
        ]
    }

## <a name="get-information-about-a-data-lake-analytics-account"></a>Abrufen von Informationen zu einem Data Lake Analytics-Konto
Der folgende Curl-Befehl veranschaulicht, wie Sie Kontoinformationen abrufen:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<DataLakeAnalyticsAccountName>?api-version=2015-11-01

Ersetzen Sie \<`REDACTED`\> durch das Autorisierungstoken, \<`AzureSubscriptionID`\> durch Ihre Abonnement-ID, \<`AzureResourceGroupName`\> durch den Namen einer vorhandenen Azure-Ressourcengruppe und \<`DataLakeAnalyticsAccountName`\> durch den Namen eines vorhandenen Data Lake Analytics-Kontos. Die Ausgabe sieht in etwa wie folgt aus:

    {
        "properties": {
            "defaultDataLakeStoreAccount": "my1004store",
            "dataLakeStoreAccounts": [
            {
                "properties": {
                "suffix": "azuredatalakestore.net"
                },
                "name": "my1004store"
            }
            ],
            "provisioningState": "Creating",
            "state": null,
            "endpoint": null,
            "accountId": "3ff9b93b-11c4-43c6-83cc-276292eeb350",
            "creationTime": null,
            "lastModifiedTime": null
        },
        "location": "East US 2",
        "tags": {},
        "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004",
        "name": "myadla1004",
        "type": "Microsoft.DataLakeAnalytics/accounts"
    }

## <a name="list-data-lake-stores-of-a-data-lake-analytics-account"></a>Auflisten von Data Lake Stores eines Data Lake Analytics-Kontos
Der folgende Curl-Befehl veranschaulicht, wie Sie die Data Lake Stores eines Kontos auflisten:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<DataLakeAnalyticsAccountName>/DataLakeStoreAccounts/?api-version=2016-11-01

Ersetzen Sie \<`REDACTED`\> durch das Autorisierungstoken, \<`AzureSubscriptionID`\> durch Ihre Abonnement-ID, \<`AzureResourceGroupName`\> durch den Namen einer vorhandenen Azure-Ressourcengruppe und \<`DataLakeAnalyticsAccountName`\> durch den Namen eines vorhandenen Data Lake Analytics-Kontos. Die Ausgabe sieht in etwa wie folgt aus:

    {
        "value": [
            {
            "properties": {
                "suffix": "azuredatalakestore.net"
            },
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004/dataLakeStoreAccounts/my1004store",
            "name": "my1004store",
            "type": "Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts"
            }
        ]
    }

## <a name="submit-u-sql-jobs"></a>Übermitteln von U-SQL-Aufträgen
Der folgende Curl-Befehl veranschaulicht, wie Sie einen U-SQL-Auftrag übermitteln:

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/Jobs/<NewGUID>?api-version=2016-03-20-preview -d@"C:\tutorials\adla\SubmitADLAJob.json"

Ersetzen Sie \<`REDACTED`\> durch das Autorisierungstoken und \<`DataLakeAnalyticsAccountName`\> durch den Namen eines vorhandenen Data Lake Analytics-Kontos. Die Anforderungsnutzlast für diesen Befehl ist in der Datei **SubmitADLAJob.json** enthalten, die für den oben genannten Parameter `-d` bereitgestellt wird. Der Inhalt der Datei „input.json“ sieht folgendermaßen aus:

    {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "degreeOfParallelism": 1,
        "priority": 1000,
        "properties": {
            "type": "USql",
            "script": "@searchlog =\n    EXTRACT UserId          int,\n            Start           DateTime,\n            Region          string,\n            Query          
        string,\n            Duration        int?,\n            Urls            string,\n            ClickedUrls     string\n    FROM \"/Samples/Data/SearchLog.tsv\"\n    US
        ING Extractors.Tsv();\n\nOUTPUT @searchlog   \n    TO \"/Output/SearchLog-from-Data-Lake.csv\"\nUSING Outputters.Csv();"
        }
    }

Die Ausgabe sieht in etwa wie folgt aus:

    {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "myadl@SPI",
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "2016-10-05T13:54:59.9871859+00:00",
        "state": "Compiling",
        "result": "Succeeded",
        "stateAuditRecords": [
            {
            "newState": "New",
            "timeStamp": "2016-10-05T13:54:59.9871859+00:00",
            "details": "userName:myadl@SPI;submitMachine:N/A"
            }
        ],
        "properties": {
            "owner": "myadl@SPI",
            "resources": [],
            "runtimeVersion": "default",
            "rootProcessNodeId": "00000000-0000-0000-0000-000000000000",
            "algebraFilePath": "adl://myadls0831.azuredatalakestore.net/system/jobservice/jobs/Usql/2016/10/05/13/54/8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a/algebra.xml",
            "compileMode": "Semantic",
            "errorSource": "Unknown",
            "totalCompilationTime": "PT0S",
            "totalPausedTime": "PT0S",
            "totalQueuedTime": "PT0S",
            "totalRunningTime": "PT0S",
            "type": "USql"
        }
    }


## <a name="list-u-sql-jobs"></a>Auflisten von U-SQL-Aufträgen
Der folgende Curl-Befehl veranschaulicht, wie Sie U-SQL-Aufträge auflisten:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/Jobs?api-version=2016-11-01 

Ersetzen Sie \<`REDACTED`\> durch das Autorisierungstoken und \<`DataLakeAnalyticsAccountName`\> durch den Namen eines vorhandenen Data Lake Analytics-Kontos. 

Die Ausgabe sieht in etwa wie folgt aus:

    {
    "value": [
        {
        "jobId": "65cf1691-9dbe-43cd-90ed-1cafbfb406fb",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "someone@microsoft.com",
        "account": null,
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "Wed, 05 Oct 2016 13:46:53 GMT",
        "startTime": "Wed, 05 Oct 2016 13:47:33 GMT",
        "endTime": "Wed, 05 Oct 2016 13:48:07 GMT",
        "state": "Ended",
        "result": "Succeeded",
        "errorMessage": null,
        "storageAccounts": null,
        "stateAuditRecords": null,
        "logFilePatterns": null,
        "properties": null
        },
        {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "someoneadl@SPI",
        "account": null,
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "Wed, 05 Oct 2016 13:54:59 GMT",
        "startTime": "Wed, 05 Oct 2016 13:55:43 GMT",
        "endTime": "Wed, 05 Oct 2016 13:56:11 GMT",
        "state": "Ended",
        "result": "Succeeded",
        "errorMessage": null,
        "storageAccounts": null,
        "stateAuditRecords": null,
        "logFilePatterns": null,
        "properties": null
        }
    ],
    "nextLink": null,
    "count": null
    }


## <a name="get-catalog-items"></a>Abrufen von Katalogelementen
Der folgende Curl-Befehl veranschaulicht, wie Sie die Datenbanken aus dem Katalog abrufen:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/catalog/usql/databases?api-version=2016-11-01

Die Ausgabe sieht in etwa wie folgt aus:

    {
    "@odata.context":"https://myadla0831.azuredatalakeanalytics.net/sqlip/$metadata#databases","value":[
        {
        "computeAccountName":"myadla0831","databaseName":"mytest","version":"f6956327-90b8-4648-ad8b-de3ff09274ea"
        },{
        "computeAccountName":"myadla0831","databaseName":"master","version":"e8bca908-cc73-41a3-9564-e9bcfaa21f4e"
        }
    ]
    }

## <a name="see-also"></a>Weitere Informationen
* Eine komplexere Abfrage finden Sie unter [Analysieren von Websiteprotokollen mit Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).
* Informationen zu den ersten Schritten der Entwicklung von U-SQL-Anwendungen finden Sie unter [Entwickeln von U-SQL-Skripts mit Data Lake-Tools für Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
* Informationen zum Erlernen von U-SQL finden Sie unter [Erste Schritte mit der Sprache U-SQL für Azure Data Lake Analytics](data-lake-analytics-u-sql-get-started.md).
* Informationen zu Verwaltungsaufgaben finden Sie unter [Verwalten von Azure Data Lake Analytics mithilfe des Azure-Portals](data-lake-analytics-manage-use-portal.md).
* Eine Übersicht über Data Lake Analytics finden Sie unter [Azure Data Lake Analytics – Übersicht](data-lake-analytics-overview.md).
* Wenn Sie dasselbe Tutorial mit anderen Tools verwenden möchten, klicken Sie oben auf der Seite auf die Registerkartenauswahl.


