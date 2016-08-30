<properties 
	pageTitle="Tutorial: Erstellen einer Pipeline mit Kopieraktivität mithilfe der .NET-API | Microsoft Azure" 
	description="In diesem Tutorial erstellen Sie eine Azure Data Factory-Pipeline mit Kopieraktivität mithilfe der .NET-API." 
	services="data-factory" 
	documentationCenter="" 
	authors="spelluru" 
	manager="jhubbard" 
	editor="monicar"/>

<tags 
	ms.service="data-factory" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="get-started-article" 
	ms.date="08/19/2016" 
	ms.author="spelluru"/>

# Tutorial: Erstellen einer Pipeline mit Kopieraktivität mithilfe der .NET-API
> [AZURE.SELECTOR]
- [Übersicht über das Tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Verwenden des Data Factory-Editors](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Verwenden von PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [Verwenden von Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [Verwenden der REST-API](data-factory-copy-activity-tutorial-using-rest-api.md)
- [Verwenden der .NET-API](data-factory-copy-activity-tutorial-using-dotnet-api.md)
- [Verwenden des Kopier-Assistenten](data-factory-copy-data-wizard-tutorial.md)

In diesem Tutorial erfahren Sie, wie Sie eine Azure Data Factory mit der .NET-API erstellen und überwachen. Die Pipeline in der Data Factory verwendet eine Kopieraktivität zum Kopieren von Daten aus Azure Blob Storage in Azure SQL-Datenbank.

Die Kopieraktivität dient zum Verschieben von Daten in Azure Data Factory. Sie basiert auf einem global verfügbaren Dienst, mit dem Daten zwischen verschiedenen Datenspeichern sicher, zuverlässig und skalierbar kopiert werden können. Ausführliche Informationen zur Kopieraktivität finden Sie im Artikel [Datenverschiebungsaktivitäten](data-factory-data-movement-activities.md).

> [AZURE.NOTE] 
In diesem Artikel wird nicht die gesamte Data Factory-.NET-API behandelt. Ausführlichere Informationen zum Data Factory .NET SDK finden Sie in der [Data Factory-.NET-API-Referenz](https://msdn.microsoft.com/library/mt415893.aspx).

## Voraussetzungen
- Verschaffen Sie sich in der [Tutorialübersicht und in den Voraussetzungen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) einen Überblick über das Tutorial und die zu erfüllenden Voraussetzungen.
- Visual Studio 2012, 2013 oder 2015
- Herunterladen und Installieren des [Azure .NET SDK](http://azure.microsoft.com/downloads/)
- Azure PowerShell. Befolgen Sie die Anweisungen im Artikel [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) zum Installieren von Azure PowerShell auf Ihrem Computer. Azure PowerShell wird zum Erstellen einer Azure Active Directory-Anwendung benötigt.

### Erstellen einer Anwendung in Azure Active Directory
Erstellen Sie eine Azure Active Directory-Anwendung, erstellen Sie einen Dienstprinzipal für die Anwendung, und weisen Sie ihn der Rolle **Data Factory-Mitwirkender** zu.

1. Starten Sie **PowerShell**.
1. Führen Sie den folgenden Befehl aus, und geben Sie den Benutzernamen und das Kennwort ein, den bzw. das Sie bei der Anmeldung beim Azure-Portal verwendet haben:
	
		Login-AzureRmAccount   
2. Führen Sie den folgenden Befehl aus, um alle Abonnements für dieses Konto anzuzeigen:

		Get-AzureRmSubscription 
3. Führen Sie den folgenden Befehl aus, um das gewünschte Abonnement auszuwählen: Ersetzen Sie **&lt;NameOfAzureSubscription&gt;** durch den Namen Ihres Azure-Abonnements.

		Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext

	> [AZURE.IMPORTANT] Notieren Sie sich **SubscriptionId** und **TenantId** aus der Ausgabe dieses Befehls.
4. Erstellen Sie eine Azure-Ressourcengruppe mit dem Namen **ADFTutorialResourceGroup**, indem Sie in PowerShell den folgenden Befehl ausführen.

		New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"

	Falls die Ressourcengruppe bereits vorhanden ist, können Sie angeben, ob sie aktualisiert (J) oder beibehalten (N) werden soll.

	Bei Verwendung einer anderen Ressourcengruppe müssen Sie den Namen Ihrer Ressourcengruppe anstelle von ADFTutorialResourceGroup in diesem Tutorial verwenden.
5. Erstellen Sie eine Azure Active Directory-Anwendung.

		$azureAdApplication = New-AzureRmADApplication -DisplayName "ADFCopyTutotiralApp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.adfcopytutorialapp.org/example" -Password "Pass@word1"

	Sollte der folgende Fehler auftreten, wiederholen Sie den Befehl mit einer anderen URL:

		Another object with the same value for property identifierUris already exists.

6. Erstellen Sie den AD-Dienstprinzipal.

		New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

7. Fügen Sie den Dienstprinzipal der Rolle **Data Factory-Mitwirkender** hinzu.

		New-AzureRmRoleAssignment -RoleDefinitionName "Data Factory Contributor" -ServicePrincipalName $azureAdApplication.ApplicationId.Guid

8. Rufen Sie die Anwendungs-ID ab.

		$azureAdApplication

	Notieren Sie sich die Anwendungs-ID (**ApplicationID** aus der Ausgabe).

Nach diesen Schritten sollten Sie über vier Werte verfügen:

- Mandanten-ID
- Abonnement-ID
- Anwendungs-ID
- Kennwort (angegeben im ersten Befehl)

## Exemplarische Vorgehensweise
1. Erstellen Sie mithilfe von Visual Studio 2012/2013/2015 eine C# .NET-Konsolenanwendung.
	1. Starten Sie **Visual Studio** (2012/2013/2015).
	2. Klicken Sie auf **Datei**, zeigen Sie auf **Neu**, und klicken Sie auf **Projekt**.
	3. Erweitern Sie **Vorlagen**, und wählen Sie **Visual C#** aus. In dieser exemplarischen Vorgehensweise verwenden Sie C#, aber Sie können jede .NET-Sprache verwenden.
	4. Wählen Sie in der Liste der Projekttypen auf der rechten Seite **Konsolenanwendung** aus.
	5. Geben Sie als Name **DataFactoryAPITestApp** ein.
	6. Wählen Sie als Speicherort **C:\\ADFGetStarted** aus.
	7. Klicken Sie auf **OK**, um das Projekt zu erstellen.
2. Klicken Sie auf **Tools**, zeigen Sie auf **NuGet-Paket-Manager**, und klicken Sie auf **Paket-Manager-Konsole**.
3.	Führen Sie in der **Paket-Manager-Konsole** nacheinander die folgenden Befehle aus.

		Install-Package Microsoft.Azure.Management.DataFactories
		Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.19.208020213
6. Fügen Sie den folgenden **appSettings**-Abschnitt zur Datei **App.config** hinzu. Diese Einstellungen werden von der Hilfsmethode **GetAuthorizationHeader** verwendet.

	Ersetzen Sie die Werte für **&lt;Application ID&gt;** (Anwendungs-ID), **&lt;Password&gt;** (Kennwort), **&lt;Subscription ID&gt;** (Abonnement) und **&lt;tenant ID**&gt; (Mandanten-ID) durch Ihre eigenen Werte.

		<appSettings>
		    <add key="ActiveDirectoryEndpoint" value="https://login.windows.net/" />
		    <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
		    <add key="WindowsManagementUri" value="https://management.core.windows.net/" />

		    <!-- Replace the following values with your own -->
    		<add key="ApplicationId" value="<Application ID>" />
    		<add key="Password" value="<Password>" />    
		    <add key="SubscriptionId" value= "Subscription ID" />
    		<add key="ActiveDirectoryTenantId" value="tenant ID" />
		</appSettings>
6. Fügen Sie die folgenden **using**-Anweisungen zur Quelldatei (Program.cs) im Projekt hinzu.

		using System.Threading;
		using System.Configuration;
		using System.Collections.ObjectModel;
		
		using Microsoft.Azure.Management.DataFactories;
		using Microsoft.Azure.Management.DataFactories.Models;
		using Microsoft.Azure.Management.DataFactories.Common.Models;
		
		using Microsoft.IdentityModel.Clients.ActiveDirectory;
		using Microsoft.Azure;
6. Fügen Sie folgenden Code, der eine Instanz der **DataPipelineManagementClient**-Klasse erstellt, zur Methode **Main** hinzu. Sie verwenden dieses Objekt, um eine Data Factory, einen verknüpften Dienst, Eingabe- und Ausgabedatasets sowie eine Pipeline zu erstellen. Außerdem dient das Objekt zum Überwachen von Datenslices eines Datasets zur Laufzeit.

			// create data factory management client
        	string resourceGroupName = "ADFTutorialResourceGroup";
        	string dataFactoryName = "APITutorialFactory";

            TokenCloudCredentials aadTokenCredentials =
                new TokenCloudCredentials(
                    ConfigurationManager.AppSettings["SubscriptionId"],
                    GetAuthorizationHeader());

            Uri resourceManagerUri = new Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

            DataFactoryManagementClient client = new DataFactoryManagementClient(aadTokenCredentials, resourceManagerUri);

	> [AZURE.IMPORTANT] 
	Ersetzen Sie den Wert von **ResourceGroupName** durch den Namen Ihrer Azure-Ressourcengruppe.
	> 
	> Legen Sie den Namen der Data Factory (**dataFactoryName**) auf einen eindeutigen Namen fest. Der Name der Data Factory muss global eindeutig sein. Im Thema [Data Factory – Benennungsregeln](data-factory-naming-rules.md) finden Sie Benennungsregeln für Data Factory-Artefakte.

7. Fügen Sie den folgenden Code, der eine **Data Factory** erstellt, zur Methode **Main** hinzu.

            // create a data factory
            Console.WriteLine("Creating a data factory");
            client.DataFactories.CreateOrUpdate(resourceGroupName,
                new DataFactoryCreateOrUpdateParameters()
                {
                    DataFactory = new DataFactory()
                    {
                        Name = dataFactoryName,
                        Location = "westus",
                        Properties = new DataFactoryProperties() { }
                    }
                }
            );

8. Fügen Sie der Methode **Main** den folgenden Code hinzu, um einen **verknüpften Azure Storage-Dienst** zu erstellen:

	> [AZURE.IMPORTANT] Ersetzen Sie **storageaccountname** und **accountkey** durch den Namen bzw. Schlüssel Ihres Azure-Speicherkontos.

            // create a linked service for input data store: Azure Storage
            Console.WriteLine("Creating Azure Storage linked service");
            client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new LinkedServiceCreateOrUpdateParameters()
                {
                    LinkedService = new LinkedService()
                    {
                        Name = "AzureStorageLinkedService",
                        Properties = new LinkedServiceProperties
                        (
                            new AzureStorageLinkedService("DefaultEndpointsProtocol=https;AccountName=<storageaccountname>;AccountKey=<accountkey>")
                        )
                    }
                }
            );
9. Fügen Sie der Methode **Main** den folgenden Code hinzu, um einen **verknüpften Azure SQL-Dienst** zu erstellen:
 
	> [AZURE.IMPORTANT] Ersetzen Sie **servername**, **databasename**, **username** und **password** durch Ihren Azure SQL-Server, Ihre Datenbank, Ihren Benutzernamen und Ihr Kennwort.

			// create a linked service for output data store: Azure SQL Database
            Console.WriteLine("Creating Azure SQL Database linked service");
            client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new LinkedServiceCreateOrUpdateParameters()
                {
                    LinkedService = new LinkedService()
                    {
                        Name = "AzureSqlLinkedService",
                        Properties = new LinkedServiceProperties
                        (
                            new AzureSqlDatabaseLinkedService("Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>;Password=<password>;Integrated Security=False;Encrypt=True;Connect Timeout=30")
                        )
                    }
                }
            );
10. Fügen Sie den folgenden Code, der **Eingabe- und Ausgabedatasets** erstellt, zur Methode **Main** hinzu.

            // create input and output datasets
            Console.WriteLine("Creating input and output datasets");
            string Dataset_Source = "DatasetBlobSource";
            string Dataset_Destination = "DatasetAzureSqlDestination";

            Console.WriteLine("Creating input dataset of type: Azure Blob");
            client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,


            new DatasetCreateOrUpdateParameters()
            {
                Dataset = new Dataset()
                {
                    Name = Dataset_Source,
                    Properties = new DatasetProperties()
                    {
                        Structure = new List<DataElement>()
                        {
                            new DataElement() { Name = "FirstName", Type = "String" },
                            new DataElement() { Name = "LastName", Type = "String" }
                        },
                        LinkedServiceName = "AzureStorageLinkedService",
                        TypeProperties = new AzureBlobDataset()
                        {
                            FolderPath = "adftutorial/",
                            FileName = "emp.txt"
                        },
                        External = true,
                        Availability = new Availability()
                        {
                            Frequency = SchedulePeriod.Hour,
                            Interval = 1,
                        },

                        Policy = new Policy()
                        {
                            Validation = new ValidationPolicy()
                            {
                                MinimumRows = 1
                            }
                        }
                    }
                }
            });


            Console.WriteLine("Creating output dataset of type: Azure SQL");
            client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new DatasetCreateOrUpdateParameters()
                {
                    Dataset = new Dataset()
                    {
                        Name = Dataset_Destination,
                        Properties = new DatasetProperties()
                        {
                            Structure = new List<DataElement>()
                            {
                                new DataElement() { Name = "FirstName", Type = "String" },
                                new DataElement() { Name = "LastName", Type = "String" }
                            },
                            LinkedServiceName = "AzureSqlLinkedService",
                            TypeProperties = new AzureSqlTableDataset()
                            {
                                TableName = "emp"
                            },

                            Availability = new Availability()
                            {
                                Frequency = SchedulePeriod.Hour,
                                Interval = 1,
                            },
                        }
                    }
                });

11. Fügen Sie den folgenden Code, der eine **Pipeline erstellt und aktiviert**, zur Methode **Main** hinzu. Diese Pipeline verfügt über eine **CopyActivity**, die **BlobSource** als Quelle und **BlobSink** als Senke verwendet.

            // create a pipeline
            Console.WriteLine("Creating a pipeline");
            DateTime PipelineActivePeriodStartTime = new DateTime(2016, 8, 9, 0, 0, 0, 0, DateTimeKind.Utc);
            DateTime PipelineActivePeriodEndTime = PipelineActivePeriodStartTime.AddMinutes(60);
            string PipelineName = "ADFTutorialPipeline";

            client.Pipelines.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new PipelineCreateOrUpdateParameters()
                {
                    Pipeline = new Pipeline()
                    {
                        Name = PipelineName,
                        Properties = new PipelineProperties()
                        {
                            Description = "Demo Pipeline for data transfer between blobs",

                    // Initial value for pipeline's active period. With this, you won't need to set slice status
                    Start = PipelineActivePeriodStartTime,
                            End = PipelineActivePeriodEndTime,

                            Activities = new List<Activity>()
                            {
                                new Activity()
                                {
                                    Name = "BlobToAzureSql",
                                    Inputs = new List<ActivityInput>()
                                    {
                                        new ActivityInput() {
                                            Name = Dataset_Source
                                        }
                                    },
                                    Outputs = new List<ActivityOutput>()
                                    {
                                        new ActivityOutput()
                                        {
                                            Name = Dataset_Destination
                                        }
                                    },
                                    TypeProperties = new CopyActivity()
                                    {
                                        Source = new BlobSource(),
                                        Sink = new BlobSink()
                                        {
                                            WriteBatchSize = 10000,
                                            WriteBatchTimeout = TimeSpan.FromMinutes(10)
                                        }
                                    }
                                }
                            },
                        }
                    }
                });	

12. Fügen Sie den folgenden Code zur Methode **Main** hinzu, um den Status eines Datenslices des Ausgabedatasets abzurufen. Es gibt nur ein Slice in diesem Beispiel erwartet.
 
            // Pulling status within a timeout threshold
            DateTime start = DateTime.Now;
            bool done = false;

            while (DateTime.Now - start < TimeSpan.FromMinutes(5) && !done)
            {
                Console.WriteLine("Pulling the slice status");
                // wait before the next status check
                Thread.Sleep(1000 * 12);

                var datalistResponse = client.DataSlices.List(resourceGroupName, dataFactoryName, Dataset_Destination,
                    new DataSliceListParameters()
                    {
                        DataSliceRangeStartTime = PipelineActivePeriodStartTime.ConvertToISO8601DateTimeString(),
                        DataSliceRangeEndTime = PipelineActivePeriodEndTime.ConvertToISO8601DateTimeString()
                    });

                foreach (DataSlice slice in datalistResponse.DataSlices)
                {
                    if (slice.State == DataSliceState.Failed || slice.State == DataSliceState.Ready)
                    {
                        Console.WriteLine("Slice execution is done with status: {0}", slice.State);
                        done = true;
                        break;
                    }
                    else
                    {
                        Console.WriteLine("Slice status is: {0}", slice.State);
                    }
                }
            }

14. Fügen Sie der Methode **Main** den folgenden Code hinzu, um Ausführungsdetails für einen Datenslice abzurufen:

            Console.WriteLine("Getting run details of a data slice");

            // give it a few minutes for the output slice to be ready
            Console.WriteLine("\nGive it a few minutes for the output slice to be ready and press any key.");
            Console.ReadKey();

            var datasliceRunListResponse = client.DataSliceRuns.List(
                    resourceGroupName,
                    dataFactoryName,
                    Dataset_Destination,
                    new DataSliceRunListParameters()
                    {
                        DataSliceStartTime = PipelineActivePeriodStartTime.ConvertToISO8601DateTimeString()
                    }
                );

            foreach (DataSliceRun run in datasliceRunListResponse.DataSliceRuns)
            {
                Console.WriteLine("Status: \t\t{0}", run.Status);
                Console.WriteLine("DataSliceStart: \t{0}", run.DataSliceStart);
                Console.WriteLine("DataSliceEnd: \t\t{0}", run.DataSliceEnd);
                Console.WriteLine("ActivityId: \t\t{0}", run.ActivityName);
                Console.WriteLine("ProcessingStartTime: \t{0}", run.ProcessingStartTime);
                Console.WriteLine("ProcessingEndTime: \t{0}", run.ProcessingEndTime);
                Console.WriteLine("ErrorMessage: \t{0}", run.ErrorMessage);
            }

            Console.WriteLine("\nPress any key to exit.");
            Console.ReadKey();

13. Fügen Sie die folgende Hilfsmethode, die von der Methode **Main** verwendet wird, zur **Program**-Klasse hinzu.
 
        public static string GetAuthorizationHeader()
        {
            AuthenticationResult result = null;
            var thread = new Thread(() =>
            {
                try
                {
                    var context = new AuthenticationContext(ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] + ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);

                    ClientCredential credential = new ClientCredential(ConfigurationManager.AppSettings["ApplicationId"], ConfigurationManager.AppSettings["Password"]);
                    result = context.AcquireToken(resource: ConfigurationManager.AppSettings["WindowsManagementUri"], clientCredential: credential);
                }
                catch (Exception threadEx)
                {
                    Console.WriteLine(threadEx.Message);
                }
            });

            thread.SetApartmentState(ApartmentState.STA);
            thread.Name = "AcquireTokenThread";
            thread.Start();
            thread.Join();

            if (result != null)
            {
                return result.AccessToken;
            }

            throw new InvalidOperationException("Failed to acquire token");
        }  


15. Erweitern Sie im Projektmappen-Explorer das Projekt (**DataFactoryAPITestApp**), klicken Sie mit der rechten Maustaste auf **Verweise**, und klicken Sie auf **Verweis hinzufügen**. Aktivieren Sie das Kontrollkästchen für die Assembly **System.Configuration**, und klicken Sie auf **OK**.
16. Erstellen Sie die Konsolenanwendung. Klicken Sie im Menü auf **Erstellen** und dann auf **Projektmappe erstellen**.
16. Vergewissern Sie sich, dass sich in Ihrem Azure-Blobspeicher mindestens eine Datei im Container **adftutorial** befindet. Erstellen Sie andernfalls im Editor die Datei **Emp.txt** mit folgendem Inhalt, und laden Sie sie anschließend in den Container „adftutorial“ hoch:

        John, Doe
		Jane, Doe
	 
17. Führen Sie das Beispiel aus, indem Sie im Menü auf **Debuggen** -> **Debuggen starten** klicken. Wenn angezeigt wird, dass die **Ausführungsdetails für einen Datenslice** abgerufen werden, warten Sie einige Minuten, und drücken Sie dann die **EINGABETASTE**.
18. Vergewissern Sie sich mithilfe des Azure-Portals, dass die Data Factory **APITutorialFactory** mit folgenden Artefakten erstellt wird:
	- Verknüpfter Dienst: **LinkedService\_AzureStorage**
	- DataSet: **DatasetBlobSource** und **DatasetBlobDestination**.
	- Pipeline: **PipelineBlobSample**
18. Vergewissern Sie sich, dass im Ordner **apifactoryoutput** im Container **adftutorial** eine Ausgabedatei erstellt wird.

## Nächste Schritte

- Lesen Sie den [Artikel über Datenverschiebungsaktivitäten](data-factory-data-movement-activities.md). Dieser enthält ausführliche Informationen zur in diesem Tutorial verwendeten Kopieraktivität.
- Ausführlichere Informationen zum Data Factory .NET SDK finden Sie in der [Data Factory-.NET-API-Referenz](https://msdn.microsoft.com/library/mt415893.aspx). In diesem Artikel wird nicht die gesamte Data Factory-.NET-API behandelt.

 

<!---HONumber=AcomDC_0824_2016-->