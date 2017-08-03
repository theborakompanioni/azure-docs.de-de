---
title: Entwickeln von Anwendungen in Azure Data Lake Store per .NET SDK | Microsoft-Dokumentation
description: "Verwenden des Azure Data Lake Store .NET SDK zum Erstellen eines Data Lake Store-Kontos und Ausführen grundlegender Vorgänge im Data Lake Store"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: ea57d5a9-2929-4473-9d30-08227912aba7
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/09/2017
ms.author: nitinme
ms.translationtype: HT
ms.sourcegitcommit: 79bebd10784ec74b4800e19576cbec253acf1be7
ms.openlocfilehash: 70f94a07b0102e3135eaf85e5877e3502762d7e3
ms.contentlocale: de-de
ms.lasthandoff: 08/03/2017

---
# <a name="get-started-with-azure-data-lake-store-using-net-sdk"></a>Erste Schritte mit Azure Data Lake-Speicher mithilfe des .NET SDK
> [!div class="op_single_selector"]
> * [Portal](data-lake-store-get-started-portal.md)
> * [PowerShell](data-lake-store-get-started-powershell.md)
> * [.NET SDK](data-lake-store-get-started-net-sdk.md)
> * [Java SDK](data-lake-store-get-started-java-sdk.md)
> * [REST-API](data-lake-store-get-started-rest-api.md)
> * [Azure CLI 2.0](data-lake-store-get-started-cli-2.0.md)
> * [Node.js](data-lake-store-manage-use-nodejs.md)
> * [Python](data-lake-store-get-started-python.md)
>
>

Erfahren Sie, wie Sie das [Azure Data Lake Store .NET SDK](https://docs.microsoft.com/dotnet/api/overview/azure/data-lake-store?view=azure-dotnet) zum Durchführen von einfachen Vorgängen nutzen, z.B. Ordner erstellen, Datendateien hochladen und herunterladen usw. Weitere Informationen zu Data Lake finden Sie unter [Übersicht über Azure Data Lake Store](data-lake-store-overview.md).

## <a name="prerequisites"></a>Voraussetzungen
* **Visual Studio 2013, 2015 oder 2017** In den folgenden Anweisungen wird Visual Studio 2015 Update 2 verwendet.

* **Ein Azure-Abonnement**. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).

* **Azure Data Lake-Speicherkonto**. Eine Anleitung zur Erstellung eines Kontos finden Sie unter [Erste Schritte mit Azure Data Lake-Speicher mithilfe des Azure-Portals](data-lake-store-get-started-portal.md)

* **Erstellen einer Azure Active Directory-Anwendung**. Die Azure AD-Anwendung wird verwendet, um die Data Lake Store-Anwendung bei Azure AD zu authentifizieren. Für die Authentifizierung bei Azure AD stehen zwei unterschiedliche Konzepte zur Verfügung: **Endbenutzerauthentifizierung** und **Dienst-zu-Dienst-Authentifizierung**. Anweisungen und weitere Informationen zur Authentifizierung finden Sie unter [Endbenutzerauthentifizierung](data-lake-store-end-user-authenticate-using-active-directory.md) oder [Dienst-zu-Dienst-Authentifizierung](data-lake-store-authenticate-using-active-directory.md).

## <a name="create-a-net-application"></a>Erstellen einer .NET-Anwendung
1. Öffnen Sie Visual Studio, und erstellen Sie eine Konsolenanwendung.
2. Klicken Sie im Menü **Datei** auf **Neu** und dann auf **Projekt**.
3. Unter **Neues Projekt** können Sie die folgenden Werte eingeben bzw. auswählen:

   | Eigenschaft | Wert |
   | --- | --- |
   | Kategorie |Vorlagen/Visual C#/Windows |
   | Vorlage |Konsolenanwendung |
   | Name |CreateADLApplication |
4. Klicken Sie auf **OK** , um das Projekt zu erstellen.
5. Fügen Sie Ihrem Projekt die Nuget-Pakete hinzu.

   1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen und dann auf **NuGet-Pakete verwalten**.
   2. Stellen Sie auf der Registerkarte **NuGet-Paket-Manager** sicher, dass **Paketquelle** auf **nuget.org** festgelegt und das Kontrollkästchen **Vorabversion einbeziehen** aktiviert ist.
   3. Suchen und installieren Sie die folgenden NuGet-Pakete:

      * `Microsoft.Azure.Management.DataLake.Store`: In diesem Tutorial wird „v2.1.3-preview“ verwendet.
      * `Microsoft.Rest.ClientRuntime.Azure.Authentication`: In diesem Tutorial wird „v2.2.12“ verwendet.

        ![Hinzufügen einer Nuget-Quelle](./media/data-lake-store-get-started-net-sdk/data-lake-store-install-nuget-package.png "Erstellen eines neuen Azure Data Lake-Kontos")
   4. Schließen Sie den **NuGet-Paket-Manager**.
6. Öffnen Sie **Program.cs**, löschen Sie den vorhandenen Code, und fügen Sie dann die folgenden Anweisungen ein, um Verweise auf Namespaces hinzuzufügen.

        using System;
        using System.IO;
        using System.Security.Cryptography.X509Certificates; // Required only if you are using an Azure AD application created with certificates
        using System.Threading;

        using Microsoft.Azure.Management.DataLake.Store;
        using Microsoft.Azure.Management.DataLake.Store.Models;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Rest.Azure.Authentication;

7. Deklarieren Sie die Variablen wie unten gezeigt, und geben Sie die Werte für den Data Lake Store-Namen und den Ressourcengruppennamen an, die bereits vorhanden sind. Stellen Sie außerdem sicher, dass der hier angegebene lokale Pfad und der Dateiname auf dem Computer vorhanden sind. Fügen Sie nach den Namespacedeklarationen den folgenden Codeausschnitt hinzu.

        namespace SdkSample
        {
            class Program
            {
                private static DataLakeStoreAccountManagementClient _adlsClient;
                private static DataLakeStoreFileSystemManagementClient _adlsFileSystemClient;

                private static string _adlsAccountName;
                private static string _resourceGroupName;
                private static string _location;
                private static string _subId;

                private static void Main(string[] args)
                {
                    _adlsAccountName = "<DATA-LAKE-STORE-NAME>"; // TODO: Replace this value with the name of your existing Data Lake Store account.
                    _resourceGroupName = "<RESOURCE-GROUP-NAME>"; // TODO: Replace this value with the name of the resource group containing your Data Lake Store account.
                    _location = "East US 2";
                    _subId = "<SUBSCRIPTION-ID>";

                    string localFolderPath = @"C:\local_path\"; // TODO: Make sure this exists and can be overwritten.
                    string localFilePath = Path.Combine(localFolderPath, "file.txt"); // TODO: Make sure this exists and can be overwritten.
                    string remoteFolderPath = "/data_lake_path/";
                    string remoteFilePath = Path.Combine(remoteFolderPath, "file.txt");
                }
            }
        }

In den restlichen Abschnitten dieses Artikels erfahren Sie, wie Sie die verfügbaren .NET-Methoden verwenden, um Vorgänge wie Authentifizierung, Dateiupload usw. durchzuführen.

## <a name="authentication"></a>Authentifizierung

### <a name="if-you-are-using-end-user-authentication-recommended-for-this-tutorial"></a>Verwenden der Endbenutzerauthentifizierung (für dieses Tutorial empfohlen)

Verwenden Sie diese Option mit einer vorhandenen nativen Azure AD-Anwendung, um Ihre Anwendung **interaktiv** (also durch Eingabe Ihrer Azure-Anmeldeinformationen) zu authentifizieren.

Zur Vereinfachung werden im Codeausschnitt weiter unten Standardwerte für Client-ID und Umleitungs-URI verwendet, die für jedes Azure-Abonnement geeignet sind. Dieser Ansatz ermöglicht eine schnellere Bearbeitung des Tutorials. In dem Codeausschnitt muss lediglich der Wert für Ihre Mandanten-ID angegeben werden. Eine Anleitung zum Abrufen dieses Werts finden Sie unter [Erstellen einer Active Directory-Anwendung](data-lake-store-end-user-authenticate-using-active-directory.md).

    // User login via interactive popup
    // Use the client ID of an existing AAD Web application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    var tenant_id = "<AAD_tenant_id>"; // Replace this string with the user's Azure Active Directory tenant ID
    var nativeClientApp_clientId = "1950a258-227b-4e31-a9cf-717495945fc2";
    var activeDirectoryClientSettings = ActiveDirectoryClientSettings.UsePromptOnly(nativeClientApp_clientId, new Uri("urn:ietf:wg:oauth:2.0:oob"));
    var creds = UserTokenProvider.LoginWithPromptAsync(tenant_id, activeDirectoryClientSettings).Result;

Im Anschluss folgen einige wissenswerte Informationen zu diesem Codeausschnitt:

* Zur Vereinfachung des Tutorials werden in dem Codeausschnitt eine Azure AD-Domäne und eine Client-ID verwendet, die standardmäßig für alle Azure-Abonnements verfügbar sind. Dadurch können Sie **diesen Codeausschnitt unverändert in Ihrer Anwendung verwenden**.
* Wenn Sie allerdings Ihre eigene Azure AD-Domäne und Anwendungsclient-ID verwenden möchten, müssen Sie eine native Azure AD-Anwendung erstellen und dann die Azure AD-Mandanten-ID, die Client-ID und den Umleitungs-URI für die erstellte Anwendung verwenden. Eine entsprechende Anleitung finden Sie unter [Authentifizierung von Endbenutzern bei Data Lake Store mithilfe von Azure Active Directory](data-lake-store-end-user-authenticate-using-active-directory.md).

### <a name="if-you-are-using-service-to-service-authentication-with-client-secret"></a>Informationen zur Dienst-zu-Dienst-Authentifizierung mit geheimem Clientschlüssel
Mit dem folgenden Codeausschnitt können Sie unter Verwendung des Clientgeheimnisses/Anwendungsschlüssels/Dienstprinzipals eine **nicht interaktive** Authentifizierung Ihrer Anwendung durchführen. Verwenden Sie diese Option mit einer vorhandenen Azure AD-Anwendung vom Typ „Web-App“. Eine Anleitung zum Erstellen der Azure AD-Webanwendung sowie zum Abrufen der Client-ID und des Clientgeheimnisses für den weiter unten bereitgestellten Codeausschnitt finden Sie unter [Dienst-zu-Dienst-Authentifizierung mit Data Lake Store mithilfe von Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

    // Service principal / appplication authentication with client secret / key
    // Use the client ID of an existing AAD "Web App" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());

    var domain = "<AAD-directory-domain>";
    var webApp_clientId = "<AAD-application-clientid>";
    var clientSecret = "<AAD-application-client-secret>";
    var clientCredential = new ClientCredential(webApp_clientId, clientSecret);
    var creds = await ApplicationTokenProvider.LoginSilentAsync(domain, clientCredential);

### <a name="if-you-are-using-service-to-service-authentication-with-certificate"></a>Informationen zur Dienst-zu-Dienst-Authentifizierung mit Zertifikat

Als dritte Möglichkeit können Sie den folgenden Codeausschnitt verwenden, um Ihre Anwendung auf **nicht interaktive** Weise zu authentifizieren, indem Sie das Zertifikat für eine Azure Active Directory-Anwendung bzw. einen Dienstprinzipal nutzen. Verwenden Sie diese Option mit einer vorhandenen [Azure AD-Anwendung mit Zertifikaten](../azure-resource-manager/resource-group-authenticate-service-principal.md).

    // Service principal / application authentication with certificate
    // Use the client ID and certificate of an existing AAD "Web App" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());

    var domain = "<AAD-directory-domain>";
    var webApp_clientId = "<AAD-application-clientid>";
    var clientCert = <AAD-application-client-certificate>
    var clientAssertionCertificate = new ClientAssertionCertificate(webApp_clientId, clientCert);
    var creds = await ApplicationTokenProvider.LoginSilentWithCertificateAsync(domain, clientAssertionCertificate);

## <a name="create-client-objects"></a>Erstellen von Clientobjekten
Mit dem folgenden Codeausschnitt werden das Data Lake Store-Konto und die Dateisystem-Clientobjekte erstellt, die zum Ausführen von Anforderungen für den Dienst genutzt werden.

    // Create client objects and set the subscription ID
    _adlsClient = new DataLakeStoreAccountManagementClient(creds) { SubscriptionId = _subId };
    _adlsFileSystemClient = new DataLakeStoreFileSystemManagementClient(creds);

## <a name="list-all-data-lake-store-accounts-within-a-subscription"></a>Auflisten aller Data Lake Store-Konten in einem Abonnement
Mit dem folgenden Codeausschnitt werden alle Data Lake Store-Konten innerhalb eines bestimmten Azure-Abonnements aufgelistet:

    // List all ADLS accounts within the subscription
    public static async Task<List<DataLakeStoreAccount>> ListAdlStoreAccounts()
    {
        var response = await _adlsClient.Account.ListAsync();
        var accounts = new List<DataLakeStoreAccount>(response);

        while (response.NextPageLink != null)
        {
            response = _adlsClient.Account.ListNext(response.NextPageLink);
            accounts.AddRange(response);
        }

        return accounts;
    }

## <a name="create-a-directory"></a>Erstellen eines Verzeichnisses
Im folgenden Codeausschnitt wird eine `CreateDirectory` -Methode veranschaulicht, die Sie zum Erstellen eines Verzeichnisses in einem Data Lake Store-Konto verwenden können.

    // Create a directory
    public static async Task CreateDirectory(string path)
    {
        await _adlsFileSystemClient.FileSystem.MkdirsAsync(_adlsAccountName, path);
    }

## <a name="upload-a-file"></a>Hochladen einer Datei
Im folgenden Codeausschnitt wird eine `UploadFile` -Methode veranschaulicht, die Sie zum Hochladen von Dateien in ein Data Lake Store-Konto verwenden können.

    // Upload a file
    public static void UploadFile(string srcFilePath, string destFilePath, bool force = true)
    {
        _adlsFileSystemClient.FileSystem.UploadFile(_adlsAccountName, srcFilePath, destFilePath, overwrite:force);
    }

Das SDK unterstützt rekursive Upload- und Downloadvorgänge zwischen einem lokalen Dateipfad und einem Data Lake Store-Dateipfad.    

## <a name="get-file-or-directory-info"></a>Abrufen von Datei- oder Verzeichnisinformationen
Im folgenden Codeausschnitt wird eine `GetItemInfo` -Methode veranschaulicht, die Sie zum Abrufen von Informationen zu einer Datei oder einem Verzeichnis in Data Lake Store verwenden können.

    // Get file or directory info
    public static async Task<FileStatusProperties> GetItemInfo(string path)
    {
        return await _adlsFileSystemClient.FileSystem.GetFileStatusAsync(_adlsAccountName, path).FileStatus;
    }

## <a name="list-file-or-directories"></a>Auflisten von Dateien oder Verzeichnissen
Im folgenden Codeausschnitt wird eine `ListItem` -Methode veranschaulicht, die Sie zum Auflisten der Dateien und Verzeichnisse in einem Data Lake Store-Konto verwenden können.

    // List files and directories
    public static List<FileStatusProperties> ListItems(string directoryPath)
    {
        return _adlsFileSystemClient.FileSystem.ListFileStatus(_adlsAccountName, directoryPath).FileStatuses.FileStatus.ToList();
    }

## <a name="concatenate-files"></a>Verketten von Dateien
Im folgenden Codeausschnitt wird eine `ConcatenateFiles` -Methode veranschaulicht, die Sie zum Verketten von Dateien verwenden können.

    // Concatenate files
    public static Task ConcatenateFiles(string[] srcFilePaths, string destFilePath)
    {
        await _adlsFileSystemClient.FileSystem.ConcatAsync(_adlsAccountName, destFilePath, srcFilePaths);
    }

## <a name="append-to-a-file"></a>Anfügen an eine Datei
Im folgenden Codeausschnitt wird eine `AppendToFile` -Methode veranschaulicht, die Sie zum Anfügen von Daten an eine bereits im Data Lake Store-Konto gespeicherte Datei verwenden können.

    // Append to file
    public static async Task AppendToFile(string path, string content)
    {
        using (var stream = new MemoryStream(Encoding.UTF8.GetBytes(content)))
        {
            await _adlsFileSystemClient.FileSystem.AppendAsync(_adlsAccountName, path, stream);
        }
    }

## <a name="download-a-file"></a>Herunterladen einer Datei
Im folgenden Codeausschnitt wird eine `DownloadFile` -Methode veranschaulicht, die Sie zum Herunterladen einer Datei aus einem Data Lake Store-Konto verwenden können.

    // Download file
    public static void DownloadFile(string srcFilePath, string destFilePath)
    {
         _adlsFileSystemClient.FileSystem.DownloadFile(_adlsAccountName, srcFilePath, destFilePath);
    }

## <a name="next-steps"></a>Nächste Schritte
* [Sichern von Daten in Data Lake-Speicher](data-lake-store-secure-data.md)
* [Verwenden von Azure Data Lake Analytics mit Data Lake-Speicher](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Verwenden von Azure HDInsight mit Data Lake-Speicher](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Data Lake Store .NET SDK Reference (Referenz zum Data Lake Store .NET SDK)](https://docs.microsoft.com/dotnet/api/?view=azuremgmtdatalakestore-2.1.0-preview&term=DataLake.Store)
* [Data Lake Store REST Reference (Data Lake Store-REST-Referenz)](https://msdn.microsoft.com/library/mt693424.aspx)

