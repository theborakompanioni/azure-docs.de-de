<properties
   pageTitle="Verwenden des Java-SDK für Data Lake-Speicher zur Anwendungsentwicklung | Azure"
   description="Verwenden Sie das Java-SDK für Azure Data Lake-Speicher, um Anwendungen zu entwickeln."
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/13/2016"
   ms.author="nitinme"/>

# Erste Schritte mit Azure Data Lake-Speicher mit Java

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST-API](data-lake-store-get-started-rest-api.md)
- [Azure-Befehlszeilenschnittstelle](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Erfahren Sie, wie Sie mithilfe des Java-SDK für Azure Data Lake-Speicher ein Azure Data Lake-Konto erstellen und grundlegende Vorgänge ausführen, z.B. Ordner erstellen, Datendateien hoch- und herunterladen, Ihr Konto löschen usw. Weitere Informationen über Data Lake finden Sie unter [Übersicht über Azure Data Lake-Speicher](data-lake-store-overview.md).

## Java-SDK für den Azure Data Lake-Speicher

Unter den folgenden Links finden Sie den Downloadpfad des Java-SDKs für den Data Lake-Speicher und die Referenz zum Java-SDK. Für dieses Tutorial müssen Sie das SDK nicht herunterladen und die Anweisungen im Referenzdokument nicht befolgen. Diese Links dienen nur zu Informationszwecken.

* Der Quellcode für das Java-SDK für den Data Lake-Speicher ist auf [GitHub](https://github.com/Azure/azure-sdk-for-java) verfügbar.
* Die Java-SDK-Referenz für den Data Lake-Speicher steht unter [https://azure.github.io/azure-sdk-for-java/](https://azure.github.io/azure-sdk-for-java/) zur Verfügung.

## Voraussetzungen

* Java Development Kit (JDK) 8 (mit Java-Version 1.8).
* IntelliJ oder eine andere geeignete Java-Entwicklungsumgebung. Ist optional, wird aber empfohlen. In der Anleitung unten wird IntelliJ verwendet.
* **Ein Azure-Abonnement**. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).
* **Erstellen einer Azure Active Directory-Anwendung**. Zur Authentifizierung mithilfe von Azure Active Directory stehen zwei Möglichkeiten zur Verfügung: **interaktiv** und **nicht interaktiv**. Je nach der gewählten Authentifizierung gelten unterschiedliche Voraussetzungen.
	* **Interaktive Authentifizierung** – Sie müssen in Azure Active Directory eine **native Clientanwendung** erstellen. Nach dem Erstellen der Anwendung rufen Sie die folgenden Werte ab, die mit der Anwendung in Zusammenhang stehen.
		- Abrufen von **Client-ID** und **Umleitungs-URI** für die Anwendung
		- Festlegen der delegierten Berechtigungen

	* **Nicht interaktive Authentifizierung** (in diesem Artikel verwendet) – Sie müssen in Azure Active Directory eine **Webanwendung** erstellen. Nach dem Erstellen der Anwendung rufen Sie die folgenden Werte ab, die mit der Anwendung in Zusammenhang stehen.
		- Abrufen von **Client-ID**,**geheimem Clientschlüssel** und **Umleitungs-URI** für die Anwendung
		- Festlegen der delegierten Berechtigungen
		- Weisen Sie die Azure Active Directory-Anwendung einer Rolle zu. Die Rolle kann sich auf der Ebene des Bereichs befinden, auf der Sie die Berechtigung für die Azure Active Directory-Anwendung gewähren möchten. Beispielsweise können Sie die Anwendung auf Abonnementebene oder auf der Ebene einer Ressourcengruppe zuweisen.

	Anweisungen dazu, wie Sie diese Werte abrufen, Berechtigungen festlegen und Rollen zuweisen, finden Sie unter [Erstellen einer Active Directory-Anwendung und eines Dienstprinzipals mithilfe des Portals](../resource-group-create-service-principal-portal.md).

## Wie authentifiziere ich mich mithilfe von Azure Active Directory?

Der unten angegebene Codeausschnitt enthält Code für die **nicht interaktive** Authentifizierung, bei der die Anwendung eigene Anmeldeinformationen bereitstellt.

Für dieses Tutorial müssen Sie Ihrer Anwendung die Berechtigung zum Erstellen von Ressourcen in Azure erteilen. Es wird **dringend empfohlen**, dass Sie dieser Anwendung für die Zwecke dieses Tutorials die Berechtigung „Mitwirkender“ nur für eine neue, unbenutzte und leere Ressourcengruppe in Ihrem Azure-Abonnement gewähren.

## Erstellen einer Java-Anwendung

1. Öffnen Sie IntelliJ, und erstellen Sie mithilfe der Vorlage **Befehlszeilen-App** ein neues Java-Projekt. Schließen Sie den Assistenten ab, um das Projekt zu erstellen.

2. Öffnen Sie **Datei** > **Projektstruktur** > **Module** (unter „Projekteinstellungen“) > **Abhängigkeiten** > **+** > **Bibliothek** -> **Von Maven**.

3. Suchen Sie nach den folgenden Maven-Paketen, und fügen Sie sie Ihrem Projekt hinzu:

    * com.microsoft.azure:azure-mgmt-datalake-store:1.0.0-beta1.2
    * com.microsoft.azure:azure-mgmt-datalake-store-uploader:1.0.0-beta1.2
    * com.microsoft.azure:azure-client-authentication:1.0.0-beta2

4. Navigieren Sie im linken Bereich, zu **src** > **main** > **java** > **<Paketname>**, öffnen Sie **Main.java**, und ersetzen Sie den vorhandenen Codeblock durch den folgenden Code. Geben Sie außerdem die im Codeausschnitt genannten Parameterwerte (beispielsweise **localFolderPath**, **DATA-LAKE-STORE-NAME** und **RESOURCE-GROUP-NAME**) an, und ersetzen Sie die Platzhalter **CLIENT-ID**, **CLIENT-SECRET**, **TENANT-ID** und **SUBSCRIPTION-ID** durch Informationen zu Ihrem Abonnement und zur entsprechenden Azure Active Directory-Instanz. Wie Sie diese Informationen finden, erfahren Sie in der [Azure-Anleitung zum Erstellen von Dienstprinzipalen](../resource-group-authenticate-service-principal.md).

    Dieser Code erstellt zunächst ein Konto im Data Lake-Speicher, erstellt dort Dateien, verkettet Dateien, lädt eine Datei herunter und löscht schließlich das Konto.

        package com.company;

        import com.microsoft.azure.CloudException;
        import com.microsoft.azure.credentials.ApplicationTokenCredentials;
        import com.microsoft.azure.management.datalake.store.implementation.DataLakeStoreAccountManagementClientImpl;
        import com.microsoft.azure.management.datalake.store.implementation.DataLakeStoreFileSystemManagementClientImpl;
        import com.microsoft.azure.management.datalake.store.models.*;
        import com.microsoft.azure.management.datalake.store.uploader.*;
        import com.microsoft.rest.credentials.ServiceClientCredentials;
        import java.io.*;
        import java.nio.charset.Charset;
        import java.util.ArrayList;
        import java.util.List;

        public class Main {
            final static String ADLS_ACCOUNT_NAME = <DATA-LAKE-STORE-NAME>;
            final static String RESOURCE_GROUP_NAME = "<RESOURCE-GROUP-NAME>";
            final static String LOCATION = "East US 2";
            final static String TENANT_ID = "<TENANT-ID>";
            final static String SUBSCRIPTION_ID =  "<SUBSCRIPTION-ID>";
            final static String CLIENT_ID = "<CLIENT-ID>";
            final static String CLIENT_SECRET = "<CLIENT-SECRET>"; // TODO: For production scenarios, we recommend that you replace this line with a more secure way of acquiring the application client secret, rather than hard-coding it in the source code.

            private static DataLakeStoreAccountManagementClientImpl _adlsClient;
            private static DataLakeStoreFileSystemManagementClientImpl _adlsFileSystemClient;

            public static void main(String[] args) throws Exception {
                String localFolderPath = "C:\\local_path\"; // TODO: Change this to any unused, new, empty folder on your local machine.

                // Authenticate
                ApplicationTokenCredentials creds = new ApplicationTokenCredentials(CLIENT_ID, TENANT_ID, CLIENT_SECRET, null);
                SetupClients(creds);

                // Create Data Lake Store account
                WaitForNewline("Authenticated.", "Creating NEW account.");
                CreateAccount();
                WaitForNewline("Account created.", "Displaying account(s).");

                // List Data Lake Store accounts that this app can access
                System.out.println(String.format("All ADL Store accounts that this app can access in subscription %s:", SUBSCRIPTION_ID));
                List<DataLakeStoreAccount> adlsListResult = _adlsClient.accounts().list().getBody();
                for (DataLakeStoreAccount acct : adlsListResult) {
                    System.out.println(acct.name());
                }
                WaitForNewline("Account(s) displayed.", "Uploading file.");

                // Upload a file to Data Lake Store: file1.csv
                UploadFile(localFolderPath + "file1.csv", "/file1.csv");
                WaitForNewline("File uploaded.", "Appending newline.");

                // Append newline to file1.csv
                AppendToFile("/file1.csv", "\r\n");
                WaitForNewline("Newline appended.", "Creating file.");

                // Create a new file in Data Lake Store: file2.csv
                CreateFile("/file2.csv", "456,def", true);
                WaitForNewline("File created.", "Concatenating files.");

                // Concatenate two files in Data Lake Store
                List<String> srcFilePaths = new ArrayList<String>();
                srcFilePaths.add("/file1.csv");
                srcFilePaths.add("/file2.csv");
                ConcatenateFiles(srcFilePaths, "/input.csv");
                WaitForNewline("Files concatenated.", "Downloading file.");

                // Download file from Data Lake Store
                DownloadFile("/input.csv", localFolderPath + "input.csv");
                WaitForNewline("File downloaded.", "Deleting file.");

                // Delete file from Data Lake Store
                DeleteFile("/input.csv");
                WaitForNewline("File deleted.", "Deleting account.");

                // Delete account
                DeleteAccount();
                WaitForNewline("Account deleted.", "DONE.");
            }

            //Set up clients
            public static void SetupClients(ServiceClientCredentials creds)
            {
                _adlsClient = new DataLakeStoreAccountManagementClientImpl(creds);
                _adlsFileSystemClient = new DataLakeStoreFileSystemManagementClientImpl(creds);
                _adlsClient.withSubscriptionId(SUBSCRIPTION_ID);
            }

            // Helper function to show status and wait for user input
            public static void WaitForNewline(String reason, String nextAction)
            {
                if (nextAction == null)
                    nextAction = "";
                if (!nextAction.isEmpty())
                {
                    System.out.println(reason + "\r\nPress ENTER to continue...");
                    try{System.in.read();}
                    catch(Exception e){}
                    System.out.println(nextAction);
                }
                else
                {
                    System.out.println(reason + "\r\nPress ENTER to continue...");
                    try{System.in.read();}
                    catch(Exception e){}
                }
            }

            // Create Data Lake Store account
            public static void CreateAccount() throws InterruptedException, CloudException, IOException {
                DataLakeStoreAccount adlsParameters = new DataLakeStoreAccount();
                adlsParameters.withLocation(LOCATION);

                _adlsClient.accounts().create(RESOURCE_GROUP_NAME, ADLS_ACCOUNT_NAME, adlsParameters);
            }

            // Create file
            public static void CreateFile(String path) throws IOException, AdlsErrorException {
                _adlsFileSystemClient.fileSystems().create(ADLS_ACCOUNT_NAME, path);
            }

            // Create file with contents
            public static void CreateFile(String path, String contents, boolean force) throws IOException, AdlsErrorException {
                byte[] bytesContents = contents.getBytes();

                _adlsFileSystemClient.fileSystems().create(ADLS_ACCOUNT_NAME, path, bytesContents, force);
            }

            // Append to file
            public static void AppendToFile(String path, String contents) throws IOException, AdlsErrorException {
                byte[] bytesContents = contents.getBytes();

                _adlsFileSystemClient.fileSystems().append(ADLS_ACCOUNT_NAME, path, bytesContents);
            }

            // Concatenate files
            public static void ConcatenateFiles(List<String> srcFilePaths, String destFilePath) throws IOException, AdlsErrorException {
                _adlsFileSystemClient.fileSystems().concat(ADLS_ACCOUNT_NAME, destFilePath, srcFilePaths);
            }

            // Delete concatenated file
            public static void DeleteFile(String filePath) throws IOException, AdlsErrorException {
                _adlsFileSystemClient.fileSystems().delete(ADLS_ACCOUNT_NAME, filePath);
            }

            // Get file or directory info
            public static FileStatusProperties GetItemInfo(String path) throws IOException, AdlsErrorException {
                return _adlsFileSystemClient.fileSystems().getFileStatus(ADLS_ACCOUNT_NAME, path).getBody().fileStatus();
            }

            // List files and directories
            public static List<FileStatusProperties> ListItems(String directoryPath) throws IOException, AdlsErrorException {
                return _adlsFileSystemClient.fileSystems().listFileStatus(ADLS_ACCOUNT_NAME, directoryPath).getBody().fileStatuses().fileStatus();
            }

            // Upload file
            public static void UploadFile(String srcPath, String destPath) throws Exception {
                UploadParameters parameters = new UploadParameters(srcPath, destPath, ADLS_ACCOUNT_NAME);
                FrontEndAdapter frontend = new DataLakeStoreFrontEndAdapterImpl(ADLS_ACCOUNT_NAME, _adlsFileSystemClient);
                DataLakeStoreUploader uploader = new DataLakeStoreUploader(parameters, frontend);
                uploader.execute();
            }

            // Download file
            public static void DownloadFile(String srcPath, String destPath) throws IOException, AdlsErrorException {
                InputStream stream = _adlsFileSystemClient.fileSystems().open(ADLS_ACCOUNT_NAME, srcPath).getBody();

                PrintWriter pWriter = new PrintWriter(destPath, Charset.defaultCharset().name());

                String fileContents = "";
                if (stream != null) {
                    Writer writer = new StringWriter();

                    char[] buffer = new char[1024];
                    try {
                        Reader reader = new BufferedReader(
                                new InputStreamReader(stream, "UTF-8"));
                        int n;
                        while ((n = reader.read(buffer)) != -1) {
                            writer.write(buffer, 0, n);
                        }
                    } finally {
                        stream.close();
                    }
                    fileContents =  writer.toString();
                }

                pWriter.println(fileContents);
                pWriter.close();
            }

            // Delete account
            public static void DeleteAccount() throws InterruptedException, CloudException, IOException {
                _adlsClient.accounts().delete(RESOURCE_GROUP_NAME, ADLS_ACCOUNT_NAME);
            }
        }


6. Führen Sie die Anwendung aus. Befolgen Sie die Anweisungen zum Ausführen und Fertigstellen der Anwendung.

## Nächste Schritte

- [Sichern von Daten in Data Lake-Speicher](data-lake-store-secure-data.md)
- [Verwenden von Azure Data Lake Analytics mit Data Lake-Speicher](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Verwenden von Azure HDInsight mit Data Lake-Speicher](data-lake-store-hdinsight-hadoop-use-portal.md)

<!---HONumber=AcomDC_0914_2016-->