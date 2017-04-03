---
title: Erste Schritte mit Azure Data Lake Store mithilfe des Python SDK | Microsoft-Dokumentation
description: "Erfahren Sie mehr über die Verwendung des Python SDK, um mit Data Lake Store und dem Dateisystem zu arbeiten."
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 75f6de6f-6fd8-48f4-8707-cb27d22d27a6
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/03/2017
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: a939a0845d7577185ff32edd542bcb2082543a26
ms.openlocfilehash: 8a3f3d8bfe670f2a4d1a4642b2380764aa6daeb4
ms.lasthandoff: 01/24/2017


---

# <a name="get-started-with-azure-data-lake-store-using-python"></a>Erste Schritte mit Azure Data Lake Store mit Python

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

Erfahren Sie, wie Sie mit dem Python SDK für Azure und Azure Data Lake Store einfache Vorgänge wie das Erstellen von Ordnern oder das Hoch- und Herunterladen von Datendateien etc. ausführen. Weitere Informationen zu Data Lake finden Sie unter [Übersicht über Azure Data Lake Store](data-lake-store-overview.md).

## <a name="prerequisites"></a>Voraussetzungen

* **Python**. Sie können Python [hier](https://www.python.org/downloads/) herunterladen. In diesem Artikel wird Python 3.5.2 verwendet.

* **Ein Azure-Abonnement**. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).

* **Erstellen einer Azure Active Directory-Anwendung**. Die Azure AD-Anwendung wird verwendet, um die Data Lake Store-Anwendung bei Azure AD zu authentifizieren. Für die Authentifizierung bei Azure AD stehen zwei unterschiedliche Konzepte zur Verfügung: **Endbenutzerauthentifizierung** und **Dienst-zu-Dienst-Authentifizierung**. Anweisungen und weitere Informationen zur Authentifizierung finden Sie unter [Authenticate with Data Lake Store using Azure Active Directory](data-lake-store-authenticate-using-active-directory.md)(Authentifizieren bei Data Lake Store mithilfe von Azure Active Directory).

## <a name="install-the-modules"></a>Installieren der Module

Zum Verwenden von Data Lake Store mit Python müssen Sie drei Module installieren.

* Das Modul `azure-mgmt-resource`. Es beinhaltet Azure-Module für Active Directory etc.
* Das Modul `azure-mgmt-datalake-store`. Es beinhaltet die Verwaltungsvorgänge für das Azure Data Lake Store-Konto. Weitere Informationen zu diesem Modul finden Sie in der [Referenz zum Verwaltungsmodul für Azure Data Lake Store](http://azure-sdk-for-python.readthedocs.io/en/latest/sample_azure-mgmt-datalake-store.html).
* Das Modul `azure-datalake-store`. Es beinhaltet die Dateisystemvorgänge für Azure Data Lake Store. Weitere Informationen zu diesem Modul finden Sie in der [Referenz zum Dateisystemmodul für Azure Data Lake Store](http://azure-datalake-store.readthedocs.io/en/latest/).

Verwenden Sie die folgenden Befehle, um die Module zu installieren:

```
pip install azure-mgmt-resource
pip install azure-mgmt-datalake-store
pip install azure-datalake-store
```

## <a name="create-a-new-python-application"></a>Erstellen einer neuen Python-Anwendung

1. Erstellen Sie in der IDE Ihrer Wahl eine neue Python-Anwendung, z.B. **mysample.py**.

2. Fügen Sie die folgenden Zeilen hinzu, um die erforderlichen Module zu importieren:

    ```
    ## Use this only for Azure AD service-to-service authentication
    from azure.common.credentials import ServicePrincipalCredentials

    ## Use this only for Azure AD end-user authentication
    from azure.common.credentials import UserPassCredentials

    ## Use this only for Azure AD multi-factor authentication
    from msrestazure.azure_active_directory import AADTokenCredentials

    ## Required for Azure Data Lake Store account management
    from azure.mgmt.datalake.store import DataLakeStoreAccountManagementClient
    from azure.mgmt.datalake.store.models import DataLakeStoreAccount

    ## Required for Azure Data Lake Store filesystem management
    from azure.datalake.store import core, lib, multithread

    # Common Azure imports
    from azure.mgmt.resource.resources import ResourceManagementClient
    from azure.mgmt.resource.resources.models import ResourceGroup

    ## Use these as needed for your application
    import logging, getpass, pprint, uuid, time
    ```

3. Speichern Sie Ihre Änderungen an „mysample.py“.

## <a name="authentication"></a>Authentifizierung

In diesem Abschnitt werden die unterschiedlichen Möglichkeiten zur Authentifizierung mit Azure AD beschrieben. Die verfügbaren Optionen sind:

* Authentifizierung von Endbenutzern
* Dienst-zu-Dienst-Authentifizierung
* Multi-Factor Authentication

Sie müssen diese Authentifizierungsoptionen sowohl für Kontoverwaltungs- als auch für Dateisystem-Verwaltungsmodule verwenden.

### <a name="end-user-authentication-for-account-management"></a>Endbenutzerauthentifizierung für die Kontoverwaltung

Verwenden Sie dieses Vorgehen zur Authentifizierung bei Azure AD für Kontoverwaltungsvorgänge (Erstellen/Löschen des Data Lake Store-Kontos usw.). Sie müssen den Benutzernamen und das Kennwort für einen Azure AD-Benutzer angeben. Beachten Sie, dass keine mehrstufige Authentifizierung für den Benutzer konfiguriert werden sollte.

    user = input('Enter the user to authenticate with that has permission to subscription: ')
    password = getpass.getpass()

    credentials = UserPassCredentials(user, password)

### <a name="end-user-authentication-for-filesystem-operations"></a>Endbenutzerauthentifizierung für Dateisystemvorgänge

Verwenden Sie dieses Vorgehen für die Authentifizierung gegenüber Azure AD für Dateisystemvorgänge (Ordner erstellen, Datei hochladen usw.). Verwenden Sie diese Methode mit einer vorhandenen Azure AD-Anwendung des **nativen Clients**. Der Azure AD-Benutzer, für den Sie Anmeldeinformationen bereitstellen, sollte nicht für die mehrstufige Authentifizierung konfiguriert werden.

    tenant_id = 'FILL-IN-HERE'
    client_id = 'FILL-IN-HERE'
    user = input('Enter the user to authenticate with that has permission to subscription: ')
    password = getpass.getpass()

    token = lib.auth(tenant_id, user, password, client_id)

### <a name="service-to-service-authentication-with-client-secret-for-account-management"></a>Dienst-zu-Dienst-Authentifizierung mit Clientgeheimnis für die Kontoverwaltung

Verwenden Sie dieses Vorgehen zur Authentifizierung bei Azure AD für Kontoverwaltungsvorgänge (Erstellen/Löschen des Data Lake Store-Kontos usw.). Mit dem folgenden Codeausschnitt können Sie unter Verwendung des Clientgeheimnisses für eine Anwendung bzw. einen Dienstprinzipal eine nicht interaktive Authentifizierung Ihrer Anwendung durchführen. Verwenden Sie diese Option mit einer vorhandenen Azure AD-Anwendung vom Typ „Web-App“.

    credentials = ServicePrincipalCredentials(client_id = 'FILL-IN-HERE', secret = 'FILL-IN-HERE', tenant = 'FILL-IN-HERE')

### <a name="service-to-service-authentication-with-client-secret-for-filesystem-operations"></a>Dienst-zu-Dienst-Authentifizierung mit Clientgeheimnis für Dateisystemvorgänge

Verwenden Sie dieses Vorgehen für die Authentifizierung gegenüber Azure AD für Dateisystemvorgänge (Ordner erstellen, Datei hochladen usw.). Mit dem folgenden Codeausschnitt können Sie unter Verwendung des Clientgeheimnisses für eine Anwendung bzw. einen Dienstprinzipal eine nicht interaktive Authentifizierung Ihrer Anwendung durchführen. Verwenden Sie diese Option mit einer vorhandenen Azure AD-Anwendung vom Typ „Web-App“.

    token = lib.auth(tenant_id = 'FILL-IN-HERE', client_secret = 'FILL-IN-HERE', client_id = 'FILL-IN-HERE')

### <a name="multi-factor-authentication-for-account-management"></a>Multi-Factor Authentication für die Kontoverwaltung

Verwenden Sie dieses Vorgehen zur Authentifizierung bei Azure AD für Kontoverwaltungsvorgänge (Erstellen/Löschen des Data Lake Store-Kontos usw.). Sie können den folgenden Codeausschnitt verwenden, um Ihre Anwendung per Multi-Factor Authentication zu authentifizieren. Verwenden Sie diese Option mit einer vorhandenen Azure AD-Anwendung vom Typ „Web-App“.

    authority_host_url = "https://login.microsoftonline.com"
    tenant = "FILL-IN-HERE"
    authority_url = authority_host_url + '/' + tenant
    client_id = 'FILL-IN-HERE'
    redirect = 'urn:ietf:wg:oauth:2.0:oob'
    RESOURCE = 'https://management.core.windows.net/'
    
    context = adal.AuthenticationContext(authority_url)
    code = context.acquire_user_code(RESOURCE, client_id)
    print(code['message'])
    mgmt_token = context.acquire_token_with_device_code(RESOURCE, code, client_id)
    credentials = AADTokenCredentials(mgmt_token, client_id)

### <a name="multi-factor-authentication-for-filesystem-management"></a>Multi-Factor Authentication für Dateisystemverwaltung

Verwenden Sie dieses Vorgehen für die Authentifizierung gegenüber Azure AD für Dateisystemvorgänge (Ordner erstellen, Datei hochladen usw.). Sie können den folgenden Codeausschnitt verwenden, um Ihre Anwendung per Multi-Factor Authentication zu authentifizieren. Verwenden Sie diese Option mit einer vorhandenen Azure AD-Anwendung vom Typ „Web-App“.

    token = lib.auth(tenant_id='FILL-IN-HERE')

## <a name="create-an-azure-resource-group"></a>Erstellen einer Azure-Ressourcengruppe

Verwenden Sie den folgenden Codeausschnitt, um eine Azure-Ressourcengruppe zu erstellen:

    ## Declare variables
    subscriptionId= 'FILL-IN-HERE'
    resourceGroup = 'FILL-IN-HERE'
    location = 'eastus2'
    
    ## Create management client object
    resourceClient = ResourceManagementClient(
        credentials,
        subscriptionId
    )
    
    ## Create an Azure Resource Group
    resourceClient.resource_groups.create_or_update(
        resourceGroup,
        ResourceGroup(
            location=location
        )
    )

## <a name="create-clients-and-data-lake-store-account"></a>Erstellen von Clients und eines Data Lake Store-Kontos

Der folgende Codeausschnitt erstellt zunächst den Data Lake Store-Kontoclient. Er verwendet das Clientobjekt zum Erstellen eines Data Lake Store-Kontos. Schließlich erstellt der Codeausschnitt ein Dateisystem-Clientobjekt.

    ## Declare variables
    subscriptionId = 'FILL-IN-HERE'
    adlsAccountName = 'FILL-IN-HERE'

    ## Create management client object
    adlsAcctClient = DataLakeStoreAccountManagementClient(credentials, subscriptionId)

    ## Create a Data Lake Store account
    adlsAcctResult = adlsAcctClient.account.create(
        resourceGroup,
        adlsAccountName,
        DataLakeStoreAccount(
            location=location
        )
    ).wait()

    ## Create a filesystem client object
    adlsFileSystemClient = core.AzureDLFileSystem(token, store_name=adlsAccountName)

## <a name="list-the-data-lake-store-accounts"></a>Auflisten der Data Lake Store-Konten

    ## List the existing Data Lake Store accounts
    result_list_response = adlsAcctClient.account.list()
    result_list = list(result_list_response)
    for items in result_list:
        print(items)

## <a name="create-a-directory"></a>Erstellen eines Verzeichnisses

    ## Create a directory
    adlsFileSystemClient.mkdir('/mysampledirectory')

## <a name="upload-a-file"></a>Hochladen einer Datei


    ## Upload a file
    multithread.ADLUploader(adlsFileSystemClient, lpath='C:\\data\\mysamplefile.txt', rpath='/mysampledirectory/mysamplefile.txt', nthreads=64, overwrite=True, buffersize=4194304, blocksize=4194304)


## <a name="download-a-file"></a>Herunterladen einer Datei

    ## Download a file
    multithread.ADLDownloader(adlsFileSystemClient, lpath='C:\\data\\mysamplefile.txt.out', rpath='/mysampledirectory/mysamplefile.txt', nthreads=64, overwrite=True, buffersize=4194304, blocksize=4194304)

## <a name="delete-a-directory"></a>Löschen eines Verzeichnisses

    ## Delete a directory
    adlsFileSystemClient.rm('/mysampledirectory', recursive=True)

## <a name="see-also"></a>Siehe auch

- [Sichern von Daten in Data Lake-Speicher](data-lake-store-secure-data.md)
- [Verwenden von Azure Data Lake Analytics mit Data Lake-Speicher](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Verwenden von Azure HDInsight mit Data Lake-Speicher](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Data Lake Store .NET SDK Reference (Referenz zum Data Lake Store .NET SDK)](https://msdn.microsoft.com/library/mt581387.aspx)
- [Data Lake Store REST Reference (Data Lake Store-REST-Referenz)](https://msdn.microsoft.com/library/mt693424.aspx)

