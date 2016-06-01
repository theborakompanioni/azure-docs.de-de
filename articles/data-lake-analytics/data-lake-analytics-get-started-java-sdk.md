<properties
   pageTitle="Verwenden des Java-SDK für Data Lake Analytics zur Anwendungsentwicklung | Azure"
   description="Verwenden Sie das Java-SDK für Azure Data Lake Analytics zur Anwendungsentwicklung"
   services="data-lake-analytics"
   documentationCenter=""
   authors="edmacauley"
   manager="paulettm"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="04/22/2016"
   ms.author="edmaca"/>

# Tutorial: Erste Schritte mit Azure Data Lake Analytics mithilfe des Java-SDK

[AZURE.INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Erfahren Sie, wie Sie mithilfe des Java-SDK für Azure Data Lake Analytics ein Azure Data Lake-Konto erstellen und grundlegende Vorgänge ausführen, z.B. Ordner erstellen, Datendateien hoch- und herunterladen, Ihr Konto löschen und mit Aufträgen arbeiten. Weitere Informationen über Data Lake finden Sie unter [Azure Data Lake Analytics](data-lake-analytics-overview.md).

In diesem Tutorial entwickeln Sie eine Java-Konsolenanwendung, die Beispiele für allgemeine Verwaltungsaufgaben enthält. Zudem erstellen Sie Testdaten und übermitteln einen Auftrag. Um das gleiche Lernprogramm unter Verwendung anderer unterstützter Tools zu durchlaufen, klicken Sie auf die Registerkarten oben in diesem Abschnitt.

[AZURE.INCLUDE [basic-process-include](../../includes/data-lake-analytics-basic-process.md)]

## Voraussetzungen

* Java Development Kit (JDK) 8 (mit Java-Version 1.8).
* IntelliJ oder eine andere geeignete Java-Entwicklungsumgebung. Ist optional, wird aber empfohlen. In der Anleitung unten wird IntelliJ verwendet.
* **Ein Azure-Abonnement**. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).
* **Aktivieren Sie Ihr Azure-Abonnement** für die öffentliche Vorschauversion von Data Lake Analytics. Weitere Informationen finden Sie in den [Anweisungen](data-lake-analytics-get-started-portal.md#signup).
* Erstellen Sie eine AAD-Anwendung (Azure Active Directory), und rufen Sie dafür **Client-ID**, **Mandanten-ID** und **Schlüssel** ab. Weitere Informationen über AAD-Anwendungen und Anweisungen zum Abrufen einer Client-ID finden Sie unter [Erstellen einer Active Directory-Anwendung und eines Dienstprinzipals mithilfe des Portals](../resource-group-create-service-principal-portal.md). Der Antwort-URI und der Schlüssel stehen auch über das Portal zur Verfügung, sobald Sie die Anwendung erstellt und den Schlüssel generiert haben.

## Wie authentifiziere ich mich mithilfe von Azure Active Directory?

Der unten angegebene Codeausschnitt enthält Code für die **nicht interaktive** Authentifizierung, bei der die Anwendung eigene Anmeldeinformationen bereitstellt.

Sie müssen Ihrer Anwendung die Berechtigung zum Erstellen von Ressourcen in Azure erteilen, damit dieses Tutorial funktioniert. Es wird **dringend empfohlen**, dass Sie dieser Anwendung für die Zwecke dieses Tutorials nur die Berechtigung „Mitwirkender“ für eine neue, ungenutzte und leere Ressourcengruppe in Ihrem Azure-Abonnement gewähren.

## Erstellen einer Java-Anwendung

1. Öffnen Sie IntelliJ, und erstellen Sie ein neues Java-Projekt, indem Sie die Vorlage **Befehlszeilen-App** verwenden.

2. Klicken Sie auf der linken Seite des Bildschirms mit der rechten Maustaste auf das Projekt, und klicken Sie dann auf **Frameworksupport hinzufügen**. Wählen Sie **Maven** aus, und klicken Sie auf **OK**.

3. Öffnen Sie die neu erstellte Datei **pom.xml**, und fügen Sie den folgenden Textausschnitt zwischen dem Tag **</version>** und dem Tag **</project>** hinzu:

    HINWEIS: Dieser Schritt ist vorläufiger Art, bis das Azure Data Lake Analytics-SDK in Maven verfügbar ist. Der Artikel wird aktualisiert, sobald das SDK in Maven verfügbar ist. Alle zukünftigen Aktualisierungen dieses SDK sind über Maven erhältlich.

        <repositories>
        	<repository>
	            <id>adx-snapshots</id>
	            <name>Azure ADX Snapshots</name>
	            <url>http://adxsnapshots.azurewebsites.net/</url>
	            <layout>default</layout>
	            <snapshots>
                	<enabled>true</enabled>
            	</snapshots>
        	</repository>
        	<repository>
	            <id>oss-snapshots</id>
	            <name>Open Source Snapshots</name>
	            <url>https://oss.sonatype.org/content/repositories/snapshots/</url>
	            <layout>default</layout>
	            <snapshots>
	                <enabled>true</enabled>
	                <updatePolicy>always</updatePolicy>
	            </snapshots>
        	</repository>
    	</repositories>
    	<dependencies>
	        <dependency>
	            <groupId>com.microsoft.azure</groupId>
	            <artifactId>azure-client-authentication</artifactId>
	            <version>1.0.0-SNAPSHOT</version>
	        </dependency>
	        <dependency>
	            <groupId>com.microsoft.azure</groupId>
	            <artifactId>azure-mgmt-datalake-store</artifactId>
	            <version>1.0.0-SNAPSHOT</version>
	        </dependency>
	        <dependency>
	            <groupId>com.microsoft.azure</groupId>
	            <artifactId>azure-mgmt-datalake-analytics</artifactId>
	            <version>1.0.0-SNAPSHOT</version>
	        </dependency>
    	</dependencies>


4. Navigieren Sie zu **Datei**, **Einstellungen** und dann zu **Build**, **Ausführung**, **Bereitstellung**. Wählen Sie **Buildtools**, **Maven**, **Importieren** aus. Aktivieren Sie anschließend die Option **Maven-Projekte automatisch importieren**.

5. Öffnen Sie die Datei **Main.java**, und ersetzen Sie den vorhandenen Codeblock durch den unten angegebenen Code. Geben Sie zudem die im Codeausschnitt genannten Werte für Parameter an, z.B. **localFolderPath**, **\_adlaAccountName**, **\_adlsAccountName**, **\_resourceGroupName**, und ersetzen Sie die Platzhalter für **CLIENT-ID**, **CLIENT-SECRET**, **TENANT-ID** und **SUBSCRIPTION-ID**.

	Mit diesem Code werden Konten für Data Lake-Speicher und Data Lake Analytics erstellt und Dateien im Speicher erstellt. Zudem wird ein Auftrag ausgeführt, der Auftragsstatus abgerufen und die Auftragsausgabe heruntergeladen. Schließlich wird das Konto gelöscht.
 

		package com.company;

		import com.microsoft.azure.CloudException;
		import com.microsoft.azure.credentials.ApplicationTokenCredentials;
		import com.microsoft.azure.management.datalake.store.*;
		import com.microsoft.azure.management.datalake.store.models.*;
		import com.microsoft.azure.management.datalake.analytics.*;
		import com.microsoft.azure.management.datalake.analytics.models.*;
		import com.microsoft.rest.credentials.ServiceClientCredentials;
		import java.io.*;
		import java.nio.charset.Charset;
		import java.nio.file.Files;
		import java.nio.file.Paths;
		import java.util.ArrayList;
		import java.util.UUID;
		import java.util.List;
		
		public class Main {
		    private static String _adlsAccountName;
		    private static String _adlaAccountName;
		    private static String _resourceGroupName;
		    private static String _location;
		
		    private static String _tenantId;
		    private static String _subId;
		    private static String _clientId;
		    private static String _clientSecret;
		
		    private static DataLakeStoreAccountManagementClient _adlsClient;
		    private static DataLakeStoreFileSystemManagementClient _adlsFileSystemClient;
		    private static DataLakeAnalyticsAccountManagementClient _adlaClient;
		    private static DataLakeAnalyticsJobManagementClient _adlaJobClient;
		    private static DataLakeAnalyticsCatalogManagementClient _adlaCatalogClient;
		
		    public static void main(String[] args) throws Exception {
		        _adlsAccountName = "<DATA-LAKE-STORE-NAME>";
		        _adlaAccountName = "<DATA-LAKE-ANALYTICS-NAME>";
		        _resourceGroupName = "<RESOURCE-GROUP-NAME>";
		        _location = "East US 2";
		
		        _tenantId = "<TENANT-ID>";
		        _subId =  "<SUBSCRIPTION-ID>";
		        _clientId = "<CLIENT-ID>";
		
		        _clientSecret = "<CLIENT-SECRET>"; // TODO: For production scenarios, we recommend that you replace this line with a more secure way of acquiring the application client secret, rather than hard-coding it in the source code.
		
		        String localFolderPath = "C:\\local_path\"; // TODO: Change this to any unused, new, empty folder on your local machine.
		
		        // Authenticate
		        ApplicationTokenCredentials creds = new ApplicationTokenCredentials(_clientId, _tenantId, _clientSecret, null);
		        SetupClients(creds);
		
		        // Create Data Lake Store and Analytics accounts
		        WaitForNewline("Authenticated.", "Creating NEW accounts.");
		        CreateAccounts();
		        WaitForNewline("Accounts created.", "Displaying accounts.");
		
		        // List Data Lake Store and Analytics accounts that this app can access
		        System.out.println(String.format("All ADL Store accounts that this app can access in subscription %s:", _subId));
		        List<DataLakeStoreAccount> adlsListResult = _adlsClient.getAccountOperations().list().getBody();
		        for (DataLakeStoreAccount acct : adlsListResult) {
		            System.out.println(acct.getName());
		        }
		        System.out.println(String.format("All ADL Analytics accounts that this app can access in subscription %s:", _subId));
		        List<DataLakeAnalyticsAccount> adlaListResult = _adlaClient.getAccountOperations().list().getBody();
		        for (DataLakeAnalyticsAccount acct : adlaListResult) {
		            System.out.println(acct.getName());
		        }
		        WaitForNewline("Accounts displayed.", "Creating files.");
		
		        // Create a file in Data Lake Store: input1.csv
		        CreateFile("/input1.csv", "123,abc", true);
		        WaitForNewline("File created.", "Submitting a job.");
		
		        // Submit a job to Data Lake Analytics
		        UUID jobId = SubmitJobByScript("@input =  EXTRACT Data string FROM "/input1.csv" USING Extractors.Csv(); OUTPUT @input TO @"/output1.csv" USING Outputters.Csv();", "testJob");
		        WaitForNewline("Job submitted.", "Getting job status.");
		
		        // Wait for job completion and output job status
		        System.out.println(String.format("Job status: %s", GetJobStatus(jobId)));
		        System.out.println("Waiting for job completion.");
		        WaitForJob(jobId);
		        System.out.println(String.format("Job status: %s", GetJobStatus(jobId)));
		        WaitForNewline("Job completed.", "Downloading job output.");
		
		        // Download job output from Data Lake Store
		        DownloadFile("/output1.csv", localFolderPath + "output1.csv");
		        WaitForNewline("Job output downloaded.", "Deleting file.");
		
		        // Delete file from Data Lake Store
		        DeleteFile("/output1.csv");
		        WaitForNewline("File deleted.", "Deleting account.");
		
		        // Delete account
		        DeleteAccounts();
		        WaitForNewline("Account deleted.", "DONE.");
			}
	
		    //Set up clients
		    public static void SetupClients(ServiceClientCredentials creds)
		    {
		        _adlsClient = new DataLakeStoreAccountManagementClientImpl(creds);
		        _adlsFileSystemClient = new DataLakeStoreFileSystemManagementClientImpl(creds);
		        _adlaClient = new DataLakeAnalyticsAccountManagementClientImpl(creds);
		        _adlaJobClient = new DataLakeAnalyticsJobManagementClientImpl(creds);
		        _adlaCatalogClient = new DataLakeAnalyticsCatalogManagementClientImpl(creds);
		        _adlsClient.setSubscriptionId(_subId);
		        _adlaClient.setSubscriptionId(_subId);
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
		
		    // Create Data Lake Store and Analytics accounts
		    public static void CreateAccounts() throws InterruptedException, CloudException, IOException {
		        // Create ADLS account
		        DataLakeStoreAccount adlsParameters = new DataLakeStoreAccount();
		        adlsParameters.setLocation(_location);
		
		
		        _adlsClient.getAccountOperations().create(_resourceGroupName, _adlsAccountName, adlsParameters);
		
		        // Create ADLA account
		        DataLakeStoreAccountInfo adlsInfo = new DataLakeStoreAccountInfo();
		        adlsInfo.setName(_adlsAccountName);
		
		        DataLakeStoreAccountInfoProperties adlsInfoProperties = new DataLakeStoreAccountInfoProperties();
		        adlsInfo.setProperties(adlsInfoProperties);
		
		        List<DataLakeStoreAccountInfo> adlsInfoList = new ArrayList<DataLakeStoreAccountInfo>();
		        adlsInfoList.add(adlsInfo);
		
		        DataLakeAnalyticsAccountProperties adlaProperties = new DataLakeAnalyticsAccountProperties();
		        adlaProperties.setDataLakeStoreAccounts(adlsInfoList);
		        adlaProperties.setDefaultDataLakeStoreAccount(_adlsAccountName);
		
		        DataLakeAnalyticsAccount adlaParameters = new DataLakeAnalyticsAccount();
		        adlaParameters.setLocation(_location);
		        adlaParameters.setName(_adlaAccountName);
		        adlaParameters.setProperties(adlaProperties);
		
		        _adlaClient.getAccountOperations().create(_resourceGroupName, _adlaAccountName, adlaParameters);
		    }
		
		    // Create file
		    public static void CreateFile(String path) throws IOException, CloudException {
		        _adlsFileSystemClient.getFileSystemOperations().create(path, _adlsAccountName);
		    }
		
		    // Create file with contents
		    public static void CreateFile(String path, String contents, boolean force) throws IOException, CloudException {
		        byte[] bytesContents = contents.getBytes();
		
		        _adlsFileSystemClient.getFileSystemOperations().create(path, _adlsAccountName, bytesContents, force);
		    }
		
		    // Append to file
		    public static void AppendToFile(String path, String contents) throws IOException, CloudException {
		        byte[] bytesContents = contents.getBytes();
		
		        _adlsFileSystemClient.getFileSystemOperations().append(path, _adlsAccountName, bytesContents);
		    }
		
		    // Concatenate files
		    public static void ConcatenateFiles(List<String> srcFilePaths, String destFilePath) throws IOException, CloudException {
		        _adlsFileSystemClient.getFileSystemOperations().concat(destFilePath, _adlsAccountName, srcFilePaths);
		    }
		
		    // Delete concatenated file
		    public static void DeleteFile(String filePath) throws IOException, CloudException {
		        _adlsFileSystemClient.getFileSystemOperations().delete(filePath, _adlsAccountName);
		    }
		
		    // Get file or directory info
		    public static FileStatusProperties GetItemInfo(String path) throws IOException, CloudException {
		        return _adlsFileSystemClient.getFileSystemOperations().getFileStatus(path, _adlsAccountName).getBody().getFileStatus();
		    }
		
		    // List files and directories
		    public static List<FileStatusProperties> ListItems(String directoryPath) throws IOException, CloudException {
		        return _adlsFileSystemClient.getFileSystemOperations().listFileStatus(directoryPath, _adlsAccountName).getBody().getFileStatuses().getFileStatus();
		    }
		
		    // Download file
		    public static void DownloadFile(String srcPath, String destPath) throws IOException, CloudException {
		        InputStream stream = _adlsFileSystemClient.getFileSystemOperations().open(srcPath, _adlsAccountName).getBody();
		
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
		
		    // Submit a U-SQL job by providing script contents.
		    // Returns the job ID
		    public static UUID SubmitJobByScript(String script, String jobName) throws IOException, CloudException {
		        UUID jobId = java.util.UUID.randomUUID();
		        USqlJobProperties properties = new USqlJobProperties();
		        properties.setScript(script);
		        JobInformation parameters = new JobInformation();
		        parameters.setName(jobName);
		        parameters.setJobId(jobId);
		        parameters.setType(JobType.USQL);
		        parameters.setProperties(properties);
		
		        JobInformation jobInfo = _adlaJobClient.getJobOperations().create(_adlaAccountName, jobId, parameters).getBody();
		
		        return jobId;
		    }
		
		    // Submit a U-SQL job by providing a path to the script
		    public static UUID SubmitJobByPath(String scriptPath, String jobName) throws IOException, CloudException {
		        byte[] scriptFileContents = Files.readAllBytes(Paths.get(scriptPath));
		        String script = new String(scriptFileContents, Charset.defaultCharset());
		        return SubmitJobByScript(script, jobName);
		    }
		
		    // Wait for job completion
		    public static JobResult WaitForJob(UUID jobId) throws IOException, CloudException {
		        JobInformation jobInfo = _adlaJobClient.getJobOperations().get(_adlaAccountName, jobId).getBody();
		        while (jobInfo.getState() != JobState.ENDED)
		        {
		            jobInfo = _adlaJobClient.getJobOperations().get(_adlaAccountName,jobId).getBody();
		        }
		        return jobInfo.getResult();
		    }
		
		    // Get job status
		    public static String GetJobStatus(UUID jobId) throws IOException, CloudException {
		        JobInformation jobInfo = _adlaJobClient.getJobOperations().get(_adlaAccountName, jobId).getBody();
		        return jobInfo.getState().toValue();
		    }
		
		    // List jobs
		    public static List<JobInformation> ListJobs() throws IOException, CloudException {
		        return _adlaJobClient.getJobOperations().list(_adlaAccountName).getBody();
		    }
		
		    // Delete accounts
		    public static void DeleteAccounts() throws InterruptedException, CloudException, IOException {
		        _adlsClient.getAccountOperations().delete(_resourceGroupName, _adlsAccountName);
		        _adlaClient.getAccountOperations().delete(_resourceGroupName, _adlaAccountName);
		    }
		}

6. Befolgen Sie die Anweisungen zum Ausführen und Fertigstellen der Anwendung.


## Weitere Informationen

- Wenn Sie dasselbe Tutorial mit anderen Tools verwenden möchten, klicken Sie oben auf der Seite auf die Registerkartenauswahl.
- Eine komplexere Abfrage finden Sie unter [Analysieren von Websiteprotokollen mit Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).
- Informationen zu den ersten Schritten der Entwicklung von U-SQL-Anwendungen finden Sie unter [Entwickeln von U-SQL-Skripts mit Data Lake-Tools für Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
- Weitere Informationen zu U-SQL finden Sie unter [Erste Schritte mit der Azure Data Lake Analytics-U-SQL-Sprache](data-lake-analytics-u-sql-get-started.md) und in der [Referenz zur U-SQL-Sprache](http://go.microsoft.com/fwlink/?LinkId=691348).
- Informationen zu Verwaltungsaufgaben finden Sie unter [Verwalten von Azure Data Lake Analytics mithilfe des Azure-Portals](data-lake-analytics-manage-use-portal.md).
- Eine Übersicht über Data Lake Analytics finden Sie unter [Azure Data Lake Analytics – Übersicht](data-lake-analytics-overview.md).

<!---HONumber=AcomDC_0518_2016-->