---
title: Verwalten von Azure Data Lake Analytics mithilfe des Azure .NET SDK | Microsoft Docs
description: "Erfahren Sie, wie Sie Data Lake Analytics-Aufträge, -Datenquellen, und -Benutzer verwalten. "
services: data-lake-analytics
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 811d172d-9873-4ce9-a6d5-c1a26b374c79
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/3/2017
ms.author: jgao
translationtype: Human Translation
ms.sourcegitcommit: d9dad6cff80c1f6ac206e7fa3184ce037900fc6b
ms.openlocfilehash: bed6fa355f3b32bb53aee002e34ca61f2ea2aa5b
ms.lasthandoff: 03/06/2017


---
# <a name="manage-azure-data-lake-analytics-using-azure-net-sdk"></a>Verwalten von Azure Data Lake Analytics mithilfe des Azure .NET SDK
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Erfahren Sie, wie Sie Azure Data Lake Analytics-Konten, -Datenquellen, -Benutzer und -Aufträge mithilfe des Azure .NET SDK verwalten. Klicken Sie oben auf die Auswahlregisterkarte, um Themen anzuzeigen, in denen die Verwaltung mit anderen Tools stattfindet.

## <a name="prerequisites"></a>Voraussetzungen

* **Visual Studio 2015, Visual Studio 2013 Update 4 oder Visual Studio 2012 mit Installation von Visual C++**.
* **Microsoft Azure SDK für .NET-Version 2.5 oder höher**.  Führen Sie die Installation mit dem [Webplattform-Installer](http://www.microsoft.com/web/downloads/platform.aspx)durch.
* **Erforderliche NuGet-Pakete**

### <a name="install-nuget-packages"></a>Installieren von NuGet-Paketen
   
   1. Klicken Sie in Visual Studio im Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen und dann auf **NuGet-Pakete verwalten**.
   2. Stellen Sie auf der Registerkarte **NuGet-Paket-Manager** sicher, dass **Paketquelle** auf **nuget.org** festgelegt und das Kontrollkästchen **Vorabversion einbeziehen** aktiviert ist.

   3. Suchen und installieren Sie die folgenden NuGet-Pakete:

    - Microsoft.Rest.ClientRuntime.Azure.Authentication – Dieses Tutorial verwendet V2.2.12
    - Microsoft.Azure.Management.DataLake.Analytics – Dieses Tutorial verwendet die V2.1.0-Vorschau
    - Microsoft.Azure.Management.DataLake.Store – Dieses Tutorial verwendet die V2.1.0-Vorschau

   4. Schließen Sie den **NuGet-Paket-Manager**.

## <a name="client-management-objects"></a>Clientverwaltungsobjekte
Die Azure Data Lake Analytics- und Azure Data Lake Store-APIs enthalten Gruppen von Clientverwaltungsobjekten, von denen aus Sie den Großteil der Programmierung vornehmen. Diese Objekte befinden sich in diesen beiden Namespaces:
* Microsoft.Azure.Management.DataLake.Analytics
* Microsoft.Azure.Management.DataLake.Store

Die folgende Tabelle zeigt die Clientverwaltungsobjekte mit Variablen, die für die Codebeispiele in diesem Artikel verwendet wurden.

| Clientverwaltungsobjekt                  | Codevariable        |
| ----------------------------------------- | -------------------- |
| DataLakeStoreAccountManagementClient      | adlsClient           |
| DataLakeAnalyticsAccountManagementClient  | adlaClient           |
| DataLakeStoreFileSystemManagementClient   | adlsFileSystemClient |
| DataLakeAnalyticsCatalogManagementClient  | adlaCatalogClient    |
| DataLakeAnalyticsJobManagementClient      | adlaJobClient        |

### <a name="data-lake-store-management-client-objects"></a>Data Lake Store-Clientverwaltungsobjekte:
* DataLakeStoreAccountManagementClient: Verwenden Sie dieses Objekt zum Erstellen und Verwalten von Data Lake Store-Konten.
* DataLakeFileSystemAccountManagementClient: Verwenden Sie dieses Objekt für Dateisystemaufgaben (also beispielsweise zum Erstellen von Ordnern und Dateien, zum Hochladen von Dateien, zum Auflisten von Dateien, zum Zugreifen auf ACLs und Anmeldeinformationen oder zum Hinzufügen von Links zu Azure Storage-Blobs).

Sie können über Data Lake zwar Links zu Azure Storage erstellen, aber nicht auf den Inhalt zugreifen. Hierzu müssen die Azure Storage SDK-APIs verwendet werden. Sie können allerdings U-SQL-Skripts für Azure Storage-Blobs ausführen.

### <a name="data-lake-analytics-management-client-objects"></a>Data Lake Analytics-Clientverwaltungsobjekte:
* DataLakeAnalyticsAccountManagementClient – Verwenden Sie dieses Objekt zum Erstellen und Verwalten von Data Lake Analytics-Konten.
* DataLakeAnalyticsCatalogManagementClient – Verwenden Sie dieses Objekt zum Untersuchen von Katalogelementen in Data Lake Analytics.
* DataLakeAnalyticsJobManagementClient – Verwenden Sie dieses Objekt zum Übermitteln und Verwalten von Aufträgen in Data Lake Analytics.

### <a name="example"></a>Beispiel

Initialisieren Sie Clientverwaltungsobjekte mit Ihren Anmeldeinformationen, wie Sie sie von der bevorzugten Authentifizierungsmethode erhalten haben. Dies wird im Folgenden in diesem Artikel beschrieben. 

    // Only the Data Lake Analytics and Data Lake Store  
    // objects need a subscription ID.
    adlsClient = new DataLakeStoreAccountManagementClient(creds);
    adlsClient.SubscriptionId = <Subscription-ID>;

    adlaClient = new DataLakeAnalyticsAccountManagementClient(creds);
    adlaClient.SubscriptionId = <Subscription-ID>;

    adlsFileSystemClient = new DataLakeStoreFileSystemManagementClient(creds);
    adlaCatalogClient = new DataLakeAnalyticsCatalogManagementClient(creds);
    adlaJobClient = new DataLakeAnalyticsJobManagementClient(creds);


    // Methods to create and manage Data Lake Analytics
    . . .

## <a name="authenticate-and-connect-to-azure-data-lake-analytics"></a>Authentifizieren und Herstellen einer Verbindung mit Azure Data Lake Analytics
Sie haben mehrere Optionen für das Anmelden bei Azure Data Lake Analytics.

### <a name="interactive-with-provided-credentials"></a>Interaktiv mit den bereitgestellten Anmeldeinformationen
Der folgende Codeausschnitt zeigt die einfachste Authentifizierung durch den Benutzer durch die Eingabe von Anmeldeinformationen wie z.B. eines Benutzernamens, eines Kennworts oder einer PIN-Nummer.

    // User login via interactive popup
    // Use the client ID of an existing AAD "native nlient" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    var tenantId = "<Tenant ID>"; // Replace this string with the user's Azure Active Directory tenant ID.
    var clientId = "1950a258-227b-4e31-a9cf-717495945fc2"; // Sample client ID
    var activeDirectoryClientSettings = ActiveDirectoryClientSettings.UsePromptOnly(nativeClientApp_clientId, new Uri("urn:ietf:wg:oauth:2.0:oob"));
    var creds = UserTokenProvider.LoginWithPromptAsync(_tenantId, activeDirectoryClientSettings).Result;

Es wird empfohlen, dass Sie eine eigene Anwendung und einen Dienstprinzipal innerhalb Ihres Azure Active Directory-Mandanten erstellen und dann anstatt der hier verwendeten Beispiel-ID die Client-ID für diese Anwendung verwenden.

### <a name="non-interactive-with-a-client-secret"></a>Nicht interaktiv mit einem geheimen Clientschlüssel
Mit dem folgenden Codeausschnitt können Sie unter Verwendung des Clientgeheimnisses/Anwendungsschlüssels/Dienstprinzipals eine nicht interaktive Authentifizierung Ihrer Anwendung durchführen. Verwenden Sie diese Authentifizierungsoption mit einer vorhandenen [Azure AD-Anwendung vom Typ „Web-App“](../azure-resource-manager/resource-group-create-service-principal-portal.md).

    // Service principal / application authentication with client secret / key
    // Use the client ID and certificate of an existing AAD "Web App" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    var tenantId = "<Azure tenant ID>";
    var webApp_clientId = "<AAD-application-clientid>";
    var clientSecret = "<AAD-application-client-secret>";
    var clientCredential = new ClientCredential(webApp_clientId, clientSecret);
    var creds = ApplicationTokenProvider.LoginSilentAsync(tenantId, clientCredential).Result;

### <a name="non-interactive-with-a-service-principal"></a>Nicht interaktiv mit einem Dienstprinzipal
Als dritte Möglichkeit können Sie den folgenden Codeausschnitt verwenden, um Ihre Anwendung auf nicht interaktive Weise zu authentifizieren, indem Sie das Zertifikat für eine Anwendung bzw. einen Dienstprinzipal nutzen. Verwenden Sie diese Authentifizierungsoption mit einer vorhandenen [Azure AD-Anwendung vom Typ „Web-App“](../azure-resource-manager/resource-group-create-service-principal-portal.md).

    // Service principal / application authentication with certificate
    // Use the client ID and certificate of an existing AAD "Web App" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    var tenantId = "<Azure tenant ID>";
    var webApp_clientId = "<AAD-application-clientid>";
    System.Security.Cryptography.X509Certificates.X509Certificate2 clientCert = <AAD-application-client-certificate>
    var clientAssertionCertificate = new ClientAssertionCertificate(webApp_clientId, clientCert);
    var creds = ApplicationTokenProvider.LoginSilentWithCertificateAsync(tenantId, clientAssertionCertificate).Result;

## <a name="create-accounts"></a>Erstellen von Konten
Zur Ausführung von Data Lake Analytics-Aufträgen ist ein Data Lake Analytics-Konto erforderlich. Für jedes Data Lake Analytics-Konto ist außerdem mindestens ein Data Lake Store-Konto erforderlich. Weitere Informationen finden Sie unter [Azure Data Lake Analytics – Übersicht](data-lake-analytics-overview.md).

### <a name="create-an-azure-resource-group"></a>Erstellen einer Azure-Ressourcengruppe
Sie benötigen eine Azure-Ressourcengruppe, um Ihre Data Lake Analytics-Komponenten erstellen zu können. Sie benötigen Ihre Authentifizierungsanmeldeinformationen, Abonnement-ID und einen Speicherort. Der folgende Code zeigt, wie Sie eine Ressourcengruppe erstellen:

    string rgName == "<value>"; // specify name for the new resrouce group
    var resourceManagementClient = new ResourceManagementClient(credential) { SubscriptionId = subscriptionId };
    var resourceGroup = new ResourceGroup { Location = location };
    resourceManagementClient.ResourceGroups.CreateOrUpdate(groupName, rgName);

Weitere Informationen finden Sie unter [Azure-Ressourcengruppen und Data Lake Analytics](#Azure-Resource-Groups-and-Data-Lake-Analytics).


### <a name="create-a-data-lake-store-account"></a>Erstellen eines Data Lake-Speicherkontos
Der folgende Code zeigt, wie Sie ein Data Lake Store-Konto erstellen. Vor der Verwendung der Create-Methode müssen Sie deren Parameter definieren und einen Standort angeben.

    var adlsParameters = new DataLakeStoreAccount(location: _location);
    adlsClient.Account.Create(_resourceGroupName, _adlsAccountName, adlsParameters);

### <a name="create-a-data-lake-analytics-account"></a>Erstellen eines Data Lake Analytics-Kontos
Der folgende Code zeigt, wie ein Data Lake Analytics-Konto mit der asynchronen Methode erstellt wird. Die CreateAsync-Methode akzeptiert eine Auflistung von Data Lake Store-Konten als einen seiner Parameter. Diese Sammlung muss mit DataLakeStoreAccountInfo-Objektinstanzen gefüllt sein. In diesem Beispiel werden die DataLakeStoreAccountInfo-Objekte aus einer Hilfsmethode (AdlaFromAdlsStoreAccounts) abgerufen.

Für alle Data Lake Analytics-Konten müssen Sie nur die Data Lake Store-Konten angeben, die Sie zur Ausführung der erforderlichen Analysen benötigen. Eines dieser Data Lake Store-Konten muss das Data Lake Store-Standardkonto sein.

    try
    {
        var adlaAccount = new DataLakeAnalyticsAccount()
        {
            DefaultDataLakeStoreAccount = “Accounting”,
            Location = _location,
            DataLakeStoreAccounts = new DataLakeStoreAccountInfo[]{
                new DataLakeStoreAccountInfo(“Expenditures”),
                new DataLakeStoreAccountInfo(“Accounting”)
            }
        };
        adlaClient.Account.Create(_resourceGroupName, newAccountName, adlaAccount);
    }
    catch (Exception ex)
    {
        Console.WriteLine(ex.Message);
    }

## <a name="manage-accounts"></a>Konten verwalten

### <a name="list-data-lake-store-and-data-lake-analytics-accounts"></a>Auflisten von Data Lake Store- und Data Lake Analytics-Konten
Der folgende Code listet die Data Lake Store-Konten in einem Abonnement auf. Auflistungsvorgänge liefern nicht immer alle Eigenschaften eines Objekts, weshalb in einigen Fällen ein Get-Vorgang für das Objekt ausgeführt werden muss.

    var adlsAccounts = adlsClient.Account.List().ToList();
    foreach (var adls in adlsAccounts)
    {
        Console.WriteLine($"\t{adls.Name});

    }

    var adlaAccounts = adlaClient.Account.List().ToList();
    for (var adla in AdlaAccounts)
    {
        Console.WriteLine($"\t{adla.Name}");
    }



### <a name="get-an-account"></a>Abrufen eines Kontos
Der folgende Code gibt mithilfe eines DataLakeAnalyticsAccountManagementClient-Objekts ein Data Lake Analytics-Konto zurück. Zuerst wird geprüft, ob das Konto vorhanden ist.

    DataLakeAnalyticsAccount adlaGet;
    if (adlaClient.Account.Exists(_resourceGroupName, accountName))
    {
        adlaGet = adlaClient.Account.Get(_resourceGroupName, accountName);
        Console.WriteLine($"{adlaGet.Name}\tCreated: {adlaGet.CreationTime}");
    }

Analog dazu können Sie mithilfe eines DataLakeStoreAccountManagementClient-Objekts (adlsClient) ein Data Lake Store-Konto zurückgeben.

### <a name="delete-an-account"></a>Löschen eines Kontos
Der folgende Code löscht ein vorhandenes Data Lake Analytics-Konto.

    if (adlaClient.Account.Exists(_resourceGroupName, accountName))
    {
        adlaClient.Account.Delete(_resourceGroupName, accountName);
        Console.WriteLine($"{accountName} Deleted");
    }
    else
    {
        Console.WriteLine($"{accountName} does not exist.");
    }

Analog dazu können Sie mithilfe von „DataLakeStoreAccountManagementClient“ ein Data Lake Store-Konto löschen.

### <a name="get-the-default-data-lake-store-account"></a>Abrufen des Data Lake Store-Standardkontos
Für jedes Data Lake Analytics-Konto ist ein Data Lake Store-Standardkonto erforderlich. Verwenden Sie den folgenden Code, um das Store-Standardkonto für ein Analytics-Konto zu ermitteln.

    if (adlaClient.Account.Exists(_resourceGroupName, accountName))
    {
        DataLakeAnalyticsAccount adlaGet = adlaClient.Account.Get(_resourceGroupName, accountName);
        Console.WriteLine($"{adlaGet.Name} default DL store account: {adlaGet.DefaultDataLakeStoreAccount}");
    }


## <a name="manage-data-sources"></a>Verwalten von Datenquellen
Data Lake Analytics unterstützt derzeit die folgenden Datenquellen:

* [Azure Data Lake-Speicher](../data-lake-store/data-lake-store-overview.md)
* [Azure Storage-Konto](../storage/storage-introduction.md)

Beim Erstellen eines Analytics-Kontos müssen Sie ein Azure Data Lake Store-Konto als Data Lake Store-Standardkonto festlegen. Das Data Lake-Standardspeicherkonto dient zum Speichern von Auftragsmetadaten und -überwachungsprotokollen. Nachdem Sie ein Analytics-Konto erstellt haben, können Sie zusätzliche Data Lake Store-Konten und Links zu Azure Storage-Konten bzw. Azure Storage-Blobkonten hinzufügen.

### <a name="link-to-an-azure-storage-account-from-a-data-lake-analytics-account"></a>Verknüpfen mit einem Azure Storage-Konto von einem Data Lake Analytics-Konto
Sie können Links zu Azure Storage-Konten erstellen.

    AddStorageAccountParameters addParams = new AddStorageAccountParameters(<storage key value>);            
    adlaClient.StorageAccounts.Add(_resourceGroupName, _adlaAccountName, "<Azure Storage Account Name>", addParams);

### <a name="list-data-lake-store-data-sources"></a>Auflisten von Data Lake Store-Datenquellen
Der folgende Code listet die Data Lake Store-Konten und die Azure Storage-Konten auf, die für ein angegebenes Data Lake Analytics-Konto verwendet werden.

    var sAccnts = adlaClient.StorageAccounts.ListByAccount(_resourceGroupName, acctName);

    if (sAccnts != null)
    {
        Console.WriteLine("Azure Storage accounts:");
        foreach (var a in sAccnts)
        {
            Console.WriteLine($"\t{a.Name}");
        }
    }

    var stores = adlsClient.Account.List();
    if (stores != null)
    {
        Console.WriteLine("\nData stores:");
        foreach (var s in stores)
        {
            Console.WriteLine($"\t{s.Name}");
        }
    }

### <a name="upload-and-download-folders-and-files"></a>Hochladen und Herunterladen von Ordnern und Dateien
Sie können das Clientverwaltungsobjekt des Data Lake Store-Dateisystems zum Hoch- und Herunterladen einzelner Dateien oder Ordner von Azure auf Ihren lokalen Computer verwenden. Dafür gibt es folgende Methoden:

- UploadFolder
- UploadFile
- DownloadFolder
- DownloadFile

Der erste Parameter für diese Methoden ist der Name des Data Lake Store-Kontos, gefolgt von Parametern für den Quellpfad und den Zielpfad.

Im folgenden Beispiel wird gezeigt, wie ein Ordner im Data Lake Store heruntergeladen wird.


    try
    {
        if (adlsFileSystemClient.FileSystem.PathExists(account, sourcePath))
        {
            adlsFileSystemClient.FileSystem.DownloadFolder(account, sourcePath, destinationPath);
        }
        else
        {
            Console.WriteLine("Path does not exist");
        }
    }
    catch (IOException ioex)
    {
        Console.WriteLine(ioex.Message);
    }


### <a name="create-a-file-in-a-data-lake-store-account"></a>Erstellen einer Datei in einem Data Lake Store-Konto
Sie können .NET Framework-E/A-Vorgänge verwenden, um Inhalte für eine Datei in einem Data Lake Store-Konto zu erstellen. Der folgende Code schreibt die ersten vier Werte von 100 zufälligen Bytearrays in eine CSV-Datei.

    MemoryStream azMem = new MemoryStream();
    StreamWriter sw = new StreamWriter(azMem, UTF8Encoding.UTF8);

    for (int i = 0; i < 100; i++)
    {
        byte[] gA = Guid.NewGuid().ToByteArray();
        string dataLine = string.Format($"{gA[0].ToString()},{gA[1].ToString()},{gA[2].ToString()},{gA[3].ToString()},{gA[4].ToString()}");
        sw.WriteLine(dataLine);
    }
    sw.Flush();
    azMem.Position = 0;

    adlsFileSystemClient.FileSystem.Create(adlsAccoutName, "/Samples/Output/randombytes.csv", azMem);

    sw.Dispose();
    azMem.Dispose();

### <a name="list-blob-containers-of-an-azure-storage-account"></a>Auflisten von Blob-Containern eines Azure Storage-Kontos
Der folgende Code listet die Container für ein angegebenes Azure Storage-Konto auf.

    string ADLAName = "<specify Data Lake Analytics account name>";
    string azStorageName = "<specify Azure Storage account name>";
    var containers = adlaClient.StorageAccounts.ListStorageContainers(_resourceGroupName, ADLAName, azStorageName);
    foreach (var c in containers)
    {
       Console.WriteLine(c.Name);
    }

### <a name="verify-azure-storage-account-paths"></a>Überprüfen von Pfaden eines Azure Storage-Kontos
Der folgende Code überprüft, ob ein Azure Storage-Konto (StorageAccntName) in einem Data Lake Analytics-Konto (AnalyticsAccountName) vorhanden ist und ob ein Container (ContainerName) im Azure Storage-Konto vorhanden ist.

    bool accountExists = adlaClient.Account.StorageAccountExists(_resourceGroupName, analyticsAccountName, storageAccntName));
    bool containerExists = adlaClient.Account.StorageContainerExists(_resourceGroupName, analyticsAccountName, storageAccntName, containerName));

## <a name="manage-catalog-and-jobs"></a>Verwalten von Katalog und Aufträgen
Das DataLakeAnalyticsCatalogManagementClient-Objekt bietet Methoden zum Verwalten der SQL-Datenbank, die für jede Azure Data Lake Store-Instanz bereitgestellt wird. Das DataLakeAnalyticsJobManagementClient-Objekt bietet Methoden zum Übermitteln und Verwalten von Aufträgen, die für die Datenbank ausgeführt werden (mit U-SQL-Skripts).

### <a name="list-databases-and-schemas"></a>Auflisten von Datenbanken und Schemas
Zu den am häufigsten aufgelisteten Dingen gehören Datenbanken und das dazugehörige Schema. Der folgende Code ruft eine Sammlung von Datenbanken ab und zählt dann das Schema für die einzelnen Datenbank auf.

    var databases = adlaCatalogClient.Catalog.ListDatabases(adlaAccountName);
    foreach (var db in databases)
    {
        Console.WriteLine($"Database: {db.Name}");
        Console.WriteLine(" - Schemas:");
        var schemas = adlaCatalogClient.Catalog.ListSchemas(dlaAccountName, db.Name);
        foreach (var schm in schemas)
        {
            Console.WriteLine($"\t{schm.Name}");
        }
    }

Wenn der Code für die standardmäßige Masterdatenbank ausgeführt wird, sieht die Ausgabe dieses Beispiels wie folgt aus:

    Database: master
    - Schemas:
            dbo
            INFORMATION_SCHEMA
            sys
            usql

### <a name="list-table-columns"></a>Auflisten von Tabellenspalten
Der folgende Code zeigt, wie Sie mit einem Data Lake Analytics-Katalogverwaltungsclient auf die Datenbank zugreifen, um die Spalten einer bestimmten Tabelle aufzulisten.

    var tbl = adlaCatalogClient.Catalog.GetTable(_adlaAnalyticsAccountTest, "master", "dbo", "MyTableName");
    IEnumerable<USqlTableColumn> columns = tbl.ColumnList;

    foreach (USqlTableColumn utc in columns)
    {
        string scriptPath = "/Samples/Scripts/SearchResults_Wikipedia_Script.txt";
        Stream scriptStrm = adlsFileSystemClient.FileSystem.Open(_adlsAccountName, scriptPath);
        string scriptTxt = string.Empty;
        using (StreamReader sr = new StreamReader(scriptStrm))
        {
            scriptTxt = sr.ReadToEnd();
        }

        var jobName = "SR_Wikipedia";
        var jobId = Guid.NewGuid();
        var properties = new USqlJobProperties(scriptTxt);
        var parameters = new JobInformation(jobName, JobType.USql, properties, priority: 1, degreeOfParallelism: 1, jobId: jobId);
        var jobInfo = adlaJobClient.Job.Create(_adlaAnalyticsAccountTest, jobId, parameters);
        Console.WriteLine($"Job {jobName} submitted.");

    }
    catch (Exception ex)
    {
        Console.WriteLine(ex.Message);
    }


### <a name="list-failed-jobs"></a>Auflisten nicht erfolgreicher Aufträge
Der folgende Code listet Informationen zu nicht erfolgreichen Aufträgen auf.

    var jobs = adlaJobClient.Job.List(adlaClient,
        new ODataQuery<JobInformation> { Filter = "result eq 'Failed'" });
    foreach (var j in jobs)
    {
        Console.WriteLine($"{j.Name}\t{j.JobId}\t{j.Type}\t{j.StartTime}\t{j.EndTime}");
    }


## <a name="see-also"></a>Weitere Informationen
* [Übersicht über Microsoft Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [Erste Schritte mit Data Lake Analytics mithilfe des Azure-Portals](data-lake-analytics-get-started-portal.md)
* [Verwalten von Azure Data Lake Analytics mithilfe des Azure-Portals](data-lake-analytics-manage-use-portal.md)
* [Überwachung und Problembehandlung von Azure Data Lake Analytics-Aufträgen mithilfe des Azure-Portals](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)

