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
ms.date: 02/06/2017
ms.author: jgao
translationtype: Human Translation
ms.sourcegitcommit: 3ed1b4121e8e64b07abaeb1117f2b8a0cfd75406
ms.openlocfilehash: 7b2380e45c62684ed29fe819db7e254b968d55d0


---
# <a name="manage-azure-data-lake-analytics-using-azure-net-sdk"></a>Verwalten von Azure Data Lake Analytics mithilfe des Azure .NET SDK
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Erfahren Sie, wie Sie Azure Data Lake Analytics-Konten, -Datenquellen, -Benutzer und -Aufträge mithilfe des Azure .NET SDK verwalten. Klicken Sie oben auf die Auswahlregisterkarte, um Themen anzuzeigen, in denen die Verwaltung mit anderen Tools stattfindet.

**Voraussetzungen**

Bevor Sie mit diesem Tutorial beginnen, müssen folgende Voraussetzungen erfüllt sein:

* **Ein Azure-Abonnement**. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).

### <a name="create-an-azure-resource-group"></a>Erstellen einer Azure-Ressourcengruppe
Sie benötigen eine Azure-Ressourcengruppe, um Ihre Data Lake Analytics-Komponenten erstellen zu können. Der folgende Code zeigt, wie Sie eine Ressourcengruppe erstellen:

    public static async Task<ResourceGroup> CreateResourceGroupAsync(
        ServiceClientCredentials credential,
        string groupName,
        string subscriptionId,
        string location)
    {

        Console.WriteLine("Creating the resource group...");
        var resourceManagementClient = new ResourceManagementClient(credential)
        { SubscriptionId = subscriptionId };
        var resourceGroup = new ResourceGroup { Location = location };
        return await resourceManagementClient.ResourceGroups.CreateOrUpdateAsync(groupName, resourceGroup);
    }

Weitere Informationen finden Sie unter [Azure-Ressourcengruppen und Data Lake Analytics](## Azure Resource Groups and Data Lake Analytics).


## <a name="connect-to-azure-data-lake"></a>Herstellen einer Verbindung mit Azure Data Lake
Sie benötigen die folgenden NuGet-Pakete:

    Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Pre
    Install-Package Microsoft.Azure.Common
    Install-Package Microsoft.Azure.Common.Dependencies
    Install-Package Microsoft.Azure.Management.ResourceManager -Pre
    Install-Package Microsoft.Azure.Management.DataLake.Analytics -Pre
    Install-Package Microsoft.Azure.Management.DataLake.Store -Pre
    Install-Package Microsoft.Azure.Management.DataLake.StoreUploader -Pre
    Install-Package Microsoft.WindowsAzure.Common
    Install-Package Microsoft.WindowsAzure.Common.Dependencies


In der Main-Methode des folgenden Codebeispiels wird veranschaulicht, wie Sie eine Verbindung mit Azure herstellen und Data Lake-Clientverwaltungsobjekte für ein Analytics-Konto und ein Store-Konto initialisieren.

    using System;
    using System.Collections.Generic;
    using System.Threading;

    using Microsoft.Rest;
    using Microsoft.Rest.Azure.Authentication;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.DataLake.Analytics;
    using Microsoft.Azure.Management.DataLake.Analytics.Models;
    using Microsoft.Azure.Management.DataLake.Store;
    using Microsoft.Azure.Management.DataLake.Store.Models;
    using Microsoft.Azure.Management.DataLake.StoreUploader;

    namespace ConsoleAcplication1
    {
        class Program
        {

            private const string _SubID = "<Specify your Azure subscription ID>"; 
            private const string _ClientID = "1950a258-227b-4e31-a9cf-717495945fc2"; // An ID made availble for developers
            private const string _resourceGroupName ="<Specify your resource group name>";
            private static string _location = "East US 2"; // Specify your location

            // Replace 'common' with user's Azure Active Directory tenant ID or domain name, if needed.
            private const string _Domain = "common"; 

            // Data Lake client management objects
            private static DataLakeAnalyticsAccountManagementClient _adlaClient;
            private static DataLakeStoreAccountManagementClient _adlsClient;
            private static DataLakeAnalyticsAccountManagementClient _adlaClient;
            private static DataLakeStoreFileSystemManagementClient _adlsFileSystemClient;
            private static DataLakeAnalyticsCatalogManagementClient _adlaCatalogClient;
            private static DataLakeAnalyticsJobManagementClient _adlaJobsClient;

            private static void Main(string[] args)
            {

                // Call logon method
                var creds = AuthenticateAzure(_Domain, _ClientID);

                // Initialize Data Lake management client objects, using
                // your credentials (creds). Initialize others as needed.
                _adlsClient = new DataLakeStoreAccountManagementClient(creds);
                _adlsClient.SubscriptionId = _SubID;
                
                _adlaClient = new DataLakeAnalyticsAccountManagementClient(creds);
                _adlaClient.SubscriptionId = _SubID; 


                // Methods to create and manage Data Lake accounts and resources
                . . .

            }

            // Interactive logon
            public static ServiceClientCredentials AuthenticateAzure(string domainName, string nativeClientAppCLIENTID)
            {
                // User login via interactive popup
                SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());

                // Use the client ID of an existing AAD "Native Client" application.
                var activeDirectoryClientSettings = ActiveDirectoryClientSettings.UsePromptOnly(nativeClientAppCLIENTID, new Uri("urn:ietf:wg:oauth:2.0:oob"));
                
                return UserTokenProvider.LoginWithPromptAsync(domainName, activeDirectoryClientSettings).Result;
            }
        }
    }

## <a name="data-lake-client-management-objects"></a>Data Lake-Clientverwaltungsobjekte
Das Azure Data Lake SDK enthält Gruppen von Clientverwaltungsobjekten, die die Grundlage für den Großteil Ihrer Programmierung bilden und sich in den folgenden beiden Namespaces befinden:
* Mirosoft.Azure.Management.DataLake.Analytics
* Microsot.Azure.Management.DataLake.Store

Die folgende Tabelle enthält die Objekte und die dazugehörigen Variablen, die für Beispiele in diesem Artikel verwendet werden:

| Clientverwaltungsobjekt                  | Codevariable         |
| ----------------------------------------- | --------------------- |
| DataLakeStoreAccountManagementClient      | _adlsClient           |
| DataLakeAnalyticsAccountManagementClient  | _adlaClient           |
| DataLakeStoreFileSystemManagementClient   | _adlsFileSystemClient |
| DataLakeAnalyticsCatalogManagementClient  | _adlaCatalogClient    |
| DataLakeAnalyticsJobManagementClient      | _adlaJobsClient       |

### <a name="data-lake-store-management-client-objects"></a>Data Lake Store-Clientverwaltungsobjekte:
* DataLakeStoreAccountManagementClient: Verwenden Sie dieses Objekt zum Erstellen und Verwalten von Data Lake Store-Instanzen.
* DataLakeFileSystemAccountManagementClient: Verwenden Sie dieses Objekt für Dateisystemaufgaben (also beispielsweise zum Erstellen von Ordnern und Dateien, zum Hochladen von Dateien, zum Auflisten von Dateien, zum Zugreifen auf ACLs und Anmeldeinformationen oder zum Hinzufügen von Links zu Azure Storage-Blobs).

Sie können über Data Lake zwar Links zu Azure Storage erstellen, aber nicht auf den Inhalt zugreifen. Hierzu müssen die Azure Storage SDK-APIs verwendet werden. Sie können allerdings U-SQL-Skripts für Azure Storage-Blobs ausführen.

### <a name="data-lake-analytics-management-client-objects"></a>Data Lake Analytics-Clientverwaltungsobjekte:
* DataLakeAnalyticsAccountManagementClient: Verwenden Sie dieses Objekt zum Erstellen und Verwalten von Data Lake Analytics-Konten.
* DataLakeAnalyticsCatalogManagementClient: Verwenden Sie dieses Objekt zum Konfigurieren von SQL-Datenbanken (einschließlich Auflistungsschema).
* DataLakeAnalyticsJobManagementClient: Verwenden Sie dieses Objekt zum Erstellen und Verwalten von U-SQL-Aufträgen.

## <a name="create-accounts"></a>Erstellen von Konten
Zur Ausführung von Data Lake Analytics-Aufträgen ist ein Data Lake Analytics-Konto erforderlich. Im Gegensatz zu Azure HDInsight entstehen durch ein Analytics-Konto keine Kosten, solange darin kein Auftrag ausgeführt wird.  Sie bezahlen nur für die Zeit, in der ein Auftrag ausgeführt wird.  Weitere Informationen finden Sie unter [Azure Data Lake Analytics – Übersicht](data-lake-analytics-overview.md).

Für jedes Data Lake Analytics-Konto ist außerdem mindestens ein Data Lake Store-Konto erforderlich.
  
### <a name="create-a-data-lake-store-account"></a>Erstellen eines Data Lake-Speicherkontos
Der folgende Code zeigt, wie Sie ein Data Lake Store-Konto erstellen. Vor der Verwendung der Create-Methode müssen Sie deren Parameter definieren und einen Standort angeben.

    var adlsParameters = new DataLakeStoreAccount(location: _location);
    _adlsClient.Account.Create(_resourceGroupName, _adlsAccountName, adlsParameters);

### <a name="create-a-data-lake-analytics-account"></a>Erstellen eines Data Lake Analytics-Kontos
Der folgende Code zeigt, wie Sie ein Data Lake Analytics-Konto erstellen. Die Create-Methode des DataLakeAnalyticsAccountManagementClient-Objekts akzeptiert für einen der Parameter eine Sammlung von Data Lake Store-Konten. Diese Sammlung muss mit DataLakeStoreAccountInfo-Objektinstanzen gefüllt sein. In diesem Beispiel werden die DataLakeStoreAccountInfo-Objekte aus einer Hilfsmethode (AdlaFromAdlsStoreAccounts) abgerufen. Da sich die Data Lake Store-Konten eines Abonnements nicht unbedingt alle in einem einzelnen Data Lake Analytics-Konto befinden, überprüft der Code zudem die Namen anhand einer Genehmigungsliste.

        // create analytics account
        public void CreateAnalyticsAccount(string acctname)
        {
            IEnumerable<DataLakeStoreAccountInfo> dlaInfos = AdlaFromAdlsStoreAccounts();

            var dlInfo = new DataLakeAnalyticsAccount()
            {
                DefaultDataLakeStoreAccount = _adlsAccountName,
                Location = _location,
                DataLakeStoreAccounts = dlaInfos.ToList<DataLakeStoreAccountInfo>()
            };

            _adlaClient.Account.Create(_resourceGroupName, acctname, dlInfo);
        }

        // A helper method to collect Data Lake Store account information to create an  
        // an analytics account, and also validates accounts before including.
        public IEnumerable<DataLakeStoreAccountInfo> AdlaFromAdlsStoreAccounts()
        {
            List<DataLakeStoreAccount> adlsAccounts = _adlsClient.Account.List().ToList();

            // Create a collection for approved accounts
            List<DataLakeStoreAccount> approvedAccounts = new List<DataLakeStoreAccount>();

            foreach (DataLakeStoreAccount dlsa in adlsAccounts)
            {
                // The IsApprovedDataStore method (not shown) 
                // evaluates a Data Lake store name.
                if (IsApprovedDataStore(dlsa.Name))
                {
                    approvedAccounts.Add(dlsa);
                }
            }

            return approvedAccounts.Select(element => new DataLakeStoreAccountInfo(element.Name));
        }

## <a name="manage-accounts"></a>Konten verwalten

### <a name="list-data-lake-store-and-analytic-accounts"></a>Auflisten von Data Lake Store- und Analytics-Konten
Der folgende Code listet die Data Lake Store-Konten in einem Abonnement auf. Auflistungsvorgänge liefern nicht immer alle Eigenschaften eines Objekts, weshalb in einigen Fällen ein Get-Vorgang für das Objekt ausgeführt werden muss.
            
    var adlsAccounts = _adlsClient.Account.List().ToList();
    Console.WriteLine($"You have {adlsAccounts.Count} Data Lake Store accounts.");
    for (int i = 0; i < adlsAccounts.Count; i++)
    {
        Console.WriteLine($"\t{adlsAccounts[i].Name}");
    }

    var adlaAccounts = _adlaClient.Account.List().ToList();
    Console.WriteLine($"\nYou have {adlaAccounts.Count} Data Lake Analytic accounts.");
    for (int j = 0; j < adlaAccounts.Count; j++)
    {
        Console.WriteLine($"\t{adlaAccounts[j].Name}");
    }
        

        
### <a name="get-an-account"></a>Abrufen eines Kontos
Der folgende Code gibt mithilfe eines DataLakeAnalyticsAccountManagementClient-Objekts ein Data Lake Analytics-Konto zurück, sofern das Konto vorhanden ist. 

    public DataLakeAnalyticsAccount GetDlaAccount(string strName)
    {
        DataLakeAnalyticsAccount dlaGet;
        if (_adlaClient.Account.Exists(_resourceGroupName, strName))
        {
            dlaGet = _adlaClient.Account.Get(_resourceGroupName, strName);
            Console.WriteLine($"{dlaGet.Name}\tCreated: {dlaGet.CreationTime}");
            return dlaGet;
        }
        else
        {
            return null;
        }

Analog dazu können Sie mithilfe von „DataLakeStoreAccountManagementClient“ (_adlsClient) ein Data Lake Store-Konto zurückgeben.        
### <a name="delete-an-account"></a>Löschen eines Kontos
Der folgende Code löscht ein vorhandenes Data Lake Analytics-Konto: 

    public void DeleteAnalyticsAccount(string strName)
    {
        if (_adlaClient.Account.Exists(_resourceGroupName, strName))
        {
            _adlaClient.Account.Delete(_resourceGroupName, strName);
            Console.WriteLine($"{strName} Deleted");
        }
        else
        {
            Console.WriteLine($"{strName} does not exist.");
        }

    }

Analog dazu können Sie mithilfe von „DataLakeStoreAccountManagementClient“ ein Data Lake Store-Konto löschen.

### <a name="get-the-default-data-lake-store-account"></a>Abrufen des Data Lake Store-Standardkontos
Für jedes Data Lake Analytics-Konto ist ein Data Lake Store-Standardkonto erforderlich. Verwenden Sie den folgenden Code, um das Store-Standardkonto für ein Analytics-Konto zu ermitteln.

    public void GetDefaultDLStoreAccount(string DLAaccountName)
    {
        if (_adlaClient.Account.Exists(_resourceGroupName, DLAaccountName))
        {
            DataLakeAnalyticsAccount dlaGet = _adlaClient.Account.Get(_resourceGroupName, DLAaccountName);
            Console.WriteLine($"{dlaGet.Name} default DL store account: {dlaGet.DefaultDataLakeStoreAccount}");
        }
    }

## <a name="manage-data-sources"></a>Verwalten von Datenquellen
Data Lake Analytics unterstützt derzeit die folgenden Datenquellen:

* [Azure Data Lake-Speicher](../data-lake-store/data-lake-store-overview.md)
* [Azure Storage (in englischer Sprache)](../storage/storage-introduction.md)

Beim Erstellen eines Analytics-Kontos müssen Sie ein Azure Data Lake-Speicherkonto als Standardspeicherkonto festlegen. Das Data Lake-Standardspeicherkonto dient zum Speichern von Auftragsmetadaten und -überwachungsprotokollen. Nachdem Sie ein Analytics-Konto erstellt haben, können Sie zusätzliche Data Lake-Speicher und Links zu Azure Storage-Konten hinzufügen. 

### <a name="include-a-link-to-azure-storage-in-data-lake"></a>Einschließen eines Links zu Azure Storage in Data Lake
Sie können in Ihrer Data Lake-Umgebung Links zu Azure Storage-Blobs erstellen. 

    string storageKey = "<paste the key value here>";

    AddStorageAccountParameters addParams = new AddStorageAccountParameters(storageKey);            
    _adlaClient.StorageAccounts.Add(_resourceGroupName, _adlaAccountName, "<Azure Storage Account Name>", addParams);

### <a name="list-data-lake-data-sources"></a>Auflisten von Data Lake-Datenquellen
Der folgende Code listet die Data Lake Store-Konten und die Data Lake-Speicherkonten (für Azure Storage) für ein angegebenes Data Lake Analytics-Konto auf.

    var sAccnts = _adlaClient.StorageAccounts.ListByAccount(_resourceGroupName, acctName);

    if (sAccnts != null)
    {
        Console.WriteLine("Storage accounts:");
        foreach (var a in sAccnts)
        {
            Console.WriteLine($"\t{a.Name}");
        }
    }

    var stores = _adlsClient.Account.List();
    if (stores != null)
    {
        Console.WriteLine("\nData stores:");
        foreach (var s in stores)
        {
            Console.WriteLine($"\t{s.Name}");
        }
    }

### <a name="upload-a-file-to-a-data-lake-store-account"></a>Hochladen einer Datei in ein Data Lake Store-Konto
Der folgende Code lädt mithilfe eines DataLakeStoreFileSystemManagementClient-Objekts eine lokale Datei in ein Data Lake Store-Konto hoch.

    bool force = true;
    string adlsAccnt = "Accounting";
    string srcFilePath = @"c:\DataLakeTemp\localData.csv";
    string dstFilePath = "/Reports/FY2016/2016data.csv";
    var parameters = new UploadParameters(srcFilePath, dstFilePath, adlsAccnt, isOverwrite: force);
    var frontend = new DataLakeStoreFrontEndAdapter(adlsAccnt, _adlsFileSystemClient);
    var uploader = new DataLakeStoreUploader(parameters, frontend);
    uploader.Execute();

### <a name="create-a-file-in-a-data-lake-store-account"></a>Erstellen einer Datei in einem Data Lake Store-Konto
Neben dem Hochladen können Sie Dateien auch problemlos programmgesteuert für die Analyse in Ihrem Data Lake Store-Konto erstellen. Der folgende Code schreibt die ersten vier Werte von 100 zufälligen Bytearrays in eine CSV-Datei.

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

        _adlsFileSystemClient.FileSystem.Create(adlsAccoutName, "/Samples/Output/randombytes.csv", azMem);

        sw.Dispose();
        azMem.Dispose();

### <a name="copy-files-from-a-data-lake-store-account"></a>Kopieren von Dateien aus einem Data Lake Store-Konto
Der folgende Code veranschaulicht Dateisystemvorgänge unter Verwendung eines DataLakeFileSystemAccountManagementClient-Objekts. Er kopiert die CSV-Kalkulationstabellendateien aus dem Verzeichnis „Samples/Data/AmbulanceData“ in ein lokales Verzeichnis auf Ihrem Computer.

    // This method takes the name of a Data Lake Store account,
    // and the the path to a directory in the account. In this

    public void CopyCSVFiles(string accnt, string fPath)
    {
        try
        {
            if (_adlsFileSystemClient.FileSystem.PathExists(accnt,fPath))
            {
                var fStatus = _adlsFileSystemClient.FileSystem.ListFileStatus(accnt, fPath);
                foreach (var fs in fStatus.FileStatuses.FileStatus)
                {
                    string localF = string.Empty;
                    if (fs.Type == Microsoft.Azure.Management.DataLake.Store.Models.FileType.FILE &&
                        fs.PathSuffix.Contains("csv"))
                    {
                        Stream fStream = _adlsFileSystemClient.FileSystem.Open(accnt, fPath + "/" + fs.PathSuffix);
                        localF = @"c:\DataLakeTemp\" + fs.PathSuffix;
                        FileStream localStream = new FileStream(localF, FileMode.Create);
                        fStream.CopyTo(localStream);

                    }
                    Console.WriteLine($"Copied {localF}.");
                }
            }
            else
            {
                Console.WriteLine($"File path {fPath} does not exist.");
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine(ex.Message);
        }
    }

### <a name="list-azure-storage-containers"></a>Auflisten von Azure Storage-Containern
Der folgende Code listet die Container für ein angegebenes Azure-Speicherkonto auf.

    string DLAName = "<specify Data Lake Analytics account name>";
    string azStorageName = "<specify Azure Storage account name>";
    var containers = _adlaClient.StorageAccounts.ListStorageContainers(_resourceGroupName, DLAName, azStorageName);
    foreach (var c in containers)
    {
       Console.WriteLine(c.Name);
    }

### <a name="verify-azure-storage-account-paths"></a>Überprüfen von Pfaden eines Azure Storage-Kontos
Der folgende Code überprüft, ob ein Azure Storage-Konto (StorageAccntName) in einem Data Lake Analytics-Konto (AnalyticsAccountName) vorhanden ist und ob ein Container (ContainerName) im Azure Storage-Konto vorhanden ist. 

    bool accountExists = _adlaClient.Account.StorageAccountExists(_resourceGroupName, analyticsAccountName, storageAccntName));
    bool containerExists = _adlaClient.Account.StorageContainerExists(_resourceGroupName, analyticsAccountName, storageAccntName, containerName));

## <a name="manage-catalog-and-jobs"></a>Verwalten von Katalog und Aufträgen
Das DataLakeAnalyticsCatalogManagementClient-Objekt bietet Methoden zum Verwalten der SQL-Datenbank, die für jede Azure Data Lake Store-Instanz bereitgestellt wird. Das DataLakeAnalyticsJobManagementClient-Objekt bietet Methoden zum Übermitteln und Verwalten von Aufträgen, die für die Datenbank ausgeführt werden (mit U-SQL-Skripts).

### <a name="list-databases-and-schemas"></a>Auflisten von Datenbanken und Schemas
Sie können zwar verschiedene Dinge auflisten, am häufigsten werden jedoch Datenbanken und das dazugehörige Schema aufgelistet. Der folgende Code ruft eine Sammlung von Datenbanken ab und zählt dann das Schema für die einzelnen Datenbank auf.

    private void ListCatalogItems(string dlaAccountName)
    {
        var databases = _adlaCatalogClient.Catalog.ListDatabases(dlaAccountName);
        foreach (var db in databases)
        {
            Console.WriteLine($"Database: {db.Name}");
            Console.WriteLine(" - Schemas:");
            var schemas = _adlaCatalogClient.Catalog.ListSchemas(dlaAccountName, db.Name);
            foreach (var schm in schemas)
            {
                Console.WriteLine($"\t{schm.Name}");
            }
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

    var tbl = _adlaCatalogClient.Catalog.GetTable(_adlaAnalyticsAccountTest, "master", "dbo", "MyTableName");
    IEnumerable<USqlTableColumn> columns = tbl.ColumnList;

    foreach (USqlTableColumn utc in columns)
    {
        string scriptPath = "/Samples/Scripts/SearchResults_Wikipedia_Script.txt";
        Stream scriptStrm = _adlsFileSystemClient.FileSystem.Open(_adlsAccountName, scriptPath);
        string scriptTxt = string.Empty;
        using (StreamReader sr = new StreamReader(scriptStrm))
        {
            scriptTxt = sr.ReadToEnd();
        }

        var jobName = "SR_Wikipedia";
        var jobId = Guid.NewGuid();
        var properties = new USqlJobProperties(scriptTxt);
        var parameters = new JobInformation(jobName, JobType.USql, properties, priority: 1, degreeOfParallelism: 1, jobId: jobId);
        var jobInfo = _adlaJobsClient.Job.Create(_adlaAnalyticsAccountTest, jobId, parameters);
        Console.WriteLine($"Job {jobName} submitted.");

    }
    catch (Exception ex)
    {
        Console.WriteLine(ex.Message);
    }


### <a name="list-failed-jobs"></a>Auflisten nicht erfolgreicher Aufträge
Der folgende Code listet Informationen zu nicht erfolgreichen Aufträgen auf.

    var jobs = _adlaJobsClient.Job.List(_adlaAnalyticsAccountName);

    foreach (var j in jobs)
    {
        if (j.Result == JobResult.Failed)
        {
            Console.WriteLine($"{j.Name}\t{j.JobId}\t{j.Type}\t{j.StartTime}\t{j.EndTime}");
        }
    }
### <a name="reference-azure-storage-in-u-sql-scripts"></a>Verweisen auf Azure Storage in U-SQL-Skripts
Bei dem folgenden Code handelt es sich um den Anfang eines U-SQL-Skripts. Dieses Skript gibt an, dass Daten aus einer Datei eines Data Lake Store-Kontos gelesen werden sollen: „/Samples/Data/SearchLog.tsv“.

    @searchlog =
    EXTRACT UserId          int,
            Start           DateTime,
            Region          string,
            Query           string,
            Duration        int?,
            Urls            string,
            ClickedUrls     string
    FROM "/Samples/Data/SearchLog.tsv"
    USING Extractors.Tsv();

Um Daten aus einem Blob unter einem verknüpften Azure Storage-Konto zu lesen, müssen Sie die vollständige URL zu dem Blob im folgenden Format verwenden:

    wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/<path to source>

Wenn also beispielsweise eine Quelldatei (SearchLog.tsv) unter dem Speicherkonto „contoso_33“ in einem Blobcontainer namens „Samples“ gespeichert ist, lautet der Pfad für die FROM-Anweisung wie folgt:

    FROM: "wasb://samples@constoso_33.blob.core.windows.net/SearchLog.tsv"

## <a name="azure-resource-groups-and-data-lake-analytics"></a>Azure-Ressourcengruppen und Data Lake Analytics
Anwendungen bestehen normalerweise aus vielen Komponenten, z.B. Web-App, Datenbank, Datenbankserver, Speicher und Drittanbieterdienste. Mit Azure Resource Manager können Sie mit den Ressourcen in Ihrer Anwendung als Gruppe arbeiten, die als Azure-Ressourcengruppe bezeichnet wird. Sie können alle Ressourcen für Ihre Anwendung in einem einzigen, koordinierten Vorgang bereitstellen, aktualisieren, überwachen oder löschen. Sie verwenden eine Vorlage für die Bereitstellung, die für unterschiedliche Umgebungen geeignet sein kann, z.B. Testing, Staging und Produktion. Sie können die Abrechnung für Ihre Organisation vereinfachen, indem Sie die zusammengefassten Kosten für die gesamte Gruppe anzeigen. Weitere Informationen finden Sie unter [Übersicht über den Azure-Ressourcen-Manager](../azure-resource-manager/resource-group-overview.md). 

Ein Data Lake Analytics-Dienst kann folgende Komponenten enthalten:

* Azure Data Lake Analytics-Konto
* Erforderliches Azure Data Lake-Speicherkonto
* Mindestens ein Azure Data Lake Analytics-Konto
* Mindestens ein Azure Data Lake Store-Konto
* Zusätzliche verknüpfte Azure Data Lake-Speicherkonten
* Zusätzliche Azure-Speicherkonten

Alle diese Komponenten lassen sich zur einfacheren Verwaltung in einer Ressourcenverwaltungsgruppe erstellen.

![Azure Data Lake Analytics-Konto und -Speicher](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-arm-structure.png)

Ein Data Lake Analytics-Konto und die dazugehörigen Speicherkonten müssen sich im gleichen Azure-Rechenzentrum befinden.
Die Ressourcenverwaltungsgruppe kann sich jedoch in einem anderen Rechenzentrum befinden.  

## <a name="see-also"></a>Weitere Informationen
* [Übersicht über Microsoft Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [Erste Schritte mit Data Lake Analytics mithilfe des Azure-Portals](data-lake-analytics-get-started-portal.md)
* [Verwalten von Azure Data Lake Analytics mithilfe des Azure-Portals](data-lake-analytics-manage-use-portal.md)
* [Überwachung und Problembehandlung von Azure Data Lake Analytics-Aufträgen mithilfe des Azure-Portals](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)



<!--HONumber=Feb17_HO1-->


