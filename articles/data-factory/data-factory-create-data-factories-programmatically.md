---
title: Erstellen von Datenpipelines mithilfe des Azure .NET SDK | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie Azure Data Factorys mithilfe des Data Factory .NET SDK programmgesteuert erstellen, überwachen und verwalten."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: b0a357be-3040-4789-831e-0d0a32a0bda5
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/17/2017
ms.author: spelluru
translationtype: Human Translation
ms.sourcegitcommit: cfe4957191ad5716f1086a1a332faf6a52406770
ms.openlocfilehash: 6a76c399e626ea85581d5f8fb863da878bdbf50b
ms.lasthandoff: 03/09/2017


---
# <a name="create-monitor-and-manage-azure-data-factories-using-azure-data-factory-net-sdk"></a>Erstellen, Überwachen und Verwalten von Azure Data Factorys mithilfe des Azure Data Factory .NET SDK
## <a name="overview"></a>Übersicht
Sie können Azure Data Factorys mithilfe des Data Factory .NET SDK programmgesteuert erstellen, überwachen und verwalten. Dieser Artikel enthält eine exemplarische Vorgehensweise, die Sie befolgen können, um eine .NET-Beispielkonsolenanwendung zu erstellen, die eine Data Factory erstellt und überwacht. Weitere Informationen zum Data Factory .NET SDK finden Sie unter [Data Factory-Klassenbibliotheksreferenz](https://msdn.microsoft.com/library/mt415893.aspx) .

## <a name="prerequisites"></a>Voraussetzungen
* Visual Studio 2012, 2013 oder 2015
* Laden Sie das [Azure .NET SDK](http://azure.microsoft.com/downloads/)herunter, und installieren Sie es.
* Fügen Sie Azure Active Directory eine native Clientanwendung hinzu. Informationen zu Schritten zum Hinzufügen der Anwendung finden Sie unter [Integrieren von Anwendungen in Azure Active Directory](../active-directory/active-directory-integrating-applications.md) . Notieren Sie sich **CLIENT-ID** und **UMLEITUNGS-URI** auf der Seite **KONFIGURIEREN**.
* Rufen Sie Ihre Azure-**Abonnement-ID** und **Mandanten-ID** ab. Anweisungen finden Sie unter [Abrufen von Azure-Abonnement- und Mandanten-ID](#get-azure-subscription-and-tenant-ids) .
* Herunterladen und Installieren von NuGet-Paketen für Azure Data Factory Entsprechende Anweisungen sind in der exemplarischen Vorgehensweise enthalten.

## <a name="walkthrough"></a>Exemplarische Vorgehensweise
1. Erstellen Sie mithilfe von Visual Studio 2012 oder 2013 eine C# .NET-Konsolenanwendung.
   1. Starten Sie **Visual Studio 2012/2013/2015**.
   2. Klicken Sie auf **Datei**, zeigen Sie auf **Neu**, und klicken Sie auf **Projekt**.
   3. Erweitern Sie **Vorlagen**, und wählen Sie **Visual C#** aus. In dieser exemplarischen Vorgehensweise verwenden Sie C#, aber Sie können jede .NET-Sprache verwenden.
   4. Wählen Sie in der Liste der Projekttypen auf der rechten Seite **Konsolenanwendung** aus.
   5. Geben Sie als **Name** **DataFactoryAPITestApp** ein.
   6. Wählen Sie **C:\ADFGetStarted** als **Speicherort**.
   7. Klicken Sie auf **OK** , um das Projekt zu erstellen.
2. Klicken Sie auf **Extras**, zeigen Sie auf **NuGet-Paket-Manager**, und klicken Sie auf **Paket-Manager-Konsole**.
3. Führen Sie in der **Paket-Manager-Konsole**nacheinander die folgenden Befehle aus.

    ```
    Install-Package Microsoft.Azure.Management.DataFactories
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.19.208020213
    ```
4. Fügen Sie der Datei **App.config** den folgenden **appSettings**-Abschnitt hinzu. Diese Konfigurationswerte werden von der **GetAuthorizationHeader** -Methode verwendet.

   > [!IMPORTANT]
   > Ersetzen Sie die Werte für **AdfClientId**, **RedirectUri**, **SubscriptionId** und **ActiveDirectoryTenantId** durch Ihre eigenen Werte.

    ```XML
    <appSettings>
        <add key="ActiveDirectoryEndpoint" value="https://login.windows.net/" />
        <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
        <add key="WindowsManagementUri" value="https://management.core.windows.net/" />
    
        <!-- Replace the following values with your own -->
        <add key="AdfClientId" value="Your AAD application ID" />
        <add key="RedirectUri" value="Your AAD application's redirect URI" />
        <add key="SubscriptionId" value="your subscription ID" />
        <add key="ActiveDirectoryTenantId" value="your tenant ID" />
    </appSettings>
    ```
5. Fügen Sie die folgenden **using** -Anweisungen zur Quelldatei (Program.cs) im Projekt hinzu.

    ```csharp
    using System.Configuration;
    using System.Collections.ObjectModel;
    using System.Threading;
    using System.Threading.Tasks;

    using Microsoft.Azure;
    using Microsoft.Azure.Management.DataFactories;
    using Microsoft.Azure.Management.DataFactories.Models;
    using Microsoft.Azure.Management.DataFactories.Common.Models;

    using Microsoft.IdentityModel.Clients.ActiveDirectory;

    ```
6. Fügen Sie folgenden Code, mit dem eine Instanz der **DataPipelineManagementClient**-Klasse erstellt wird, der **Main**-Methode hinzu. Sie verwenden dieses Objekt, um eine Data Factory, einen verknüpften Dienst, Eingabe- und Ausgabedatasets sowie eine Pipeline zu erstellen. Außerdem dient das Objekt zum Überwachen von Datenslices eines Datasets zur Laufzeit.

    ```csharp
    // create data factory management client
    string resourceGroupName = "resourcegroupname";
    string dataFactoryName = "APITutorialFactorySP";
    
    TokenCloudCredentials aadTokenCredentials = new TokenCloudCredentials(
            ConfigurationManager.AppSettings["SubscriptionId"],
        GetAuthorizationHeader().Result);
    
    Uri resourceManagerUri = new Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);
    
    DataFactoryManagementClient client = new DataFactoryManagementClient(aadTokenCredentials, resourceManagerUri);
    ```

   > [!NOTE]
   > Ersetzen Sie die **resourcegroupname** durch den Namen Ihrer Azure-Ressourcengruppe. Mit dem Cmdlet [New-AzureResourceGroup](https://msdn.microsoft.com/library/Dn654594.aspx) können Sie eine Ressourcengruppe erstellen.
7. Fügen Sie den folgenden Code, der eine **Data Factory** erstellt, der **Main**-Methode hinzu.

    ```csharp
    // create a data factory
    Console.WriteLine("Creating a data factory");
    client.DataFactories.CreateOrUpdate(resourceGroupName,
        new DataFactoryCreateOrUpdateParameters()
        {
            DataFactory = new DataFactory()
            {
                Name = dataFactoryName,
                Location = "westus",
                Properties = new DataFactoryProperties()
            }
        }
    );
    ```
8. Fügen Sie den folgenden Code, der einen **verknüpften Dienst** erstellt, zur Methode **Main** hinzu.

   > [!NOTE]
   > Verwenden Sie **Kontoname** und **Kontoschlüssel** Ihres Azure-Speicherkontos für **ConnectionString**.

    ```csharp
    // create a linked service
    Console.WriteLine("Creating a linked service");
    client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
        new LinkedServiceCreateOrUpdateParameters()
        {
            LinkedService = new LinkedService()
            {
                Name = "LinkedService-AzureStorage",
                Properties = new LinkedServiceProperties
                (
                    new AzureStorageLinkedService("DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>")
                )
            }
        }
    );
    ```
9. Fügen Sie den folgenden Code, mit dem **Eingabe- und Ausgabedatasets** erstellt werden, der **Main**-Methode hinzu.

    Für den **FolderPath** des Eingabeblobs ist der Wert **adftutorial/** festgelegt, wobei **adftutorial** dem Namen des Containers im Blobspeicher entspricht. Wenn dieser Container nicht in Ihrem Azure-Blobspeicher enthalten ist, erstellen Sie einen Container mit dem Namen **adftutorial** und laden eine Textdatei in den Container hoch.

    Für den FolderPath des Ausgabeblobs ist **adftutorial/apifactoryoutput/{Slice}** festgelegt, wobei **Slice** auf Basis des Werts von **SliceStart** (Startdatum und -zeit für jeden Slice) dynamisch berechnet wird.

    ```csharp
    // create input and output datasets
    Console.WriteLine("Creating input and output datasets");
    string Dataset_Source = "DatasetBlobSource";
    string Dataset_Destination = "DatasetBlobDestination";
    
    client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,
    new DatasetCreateOrUpdateParameters()
    {
        Dataset = new Dataset()
        {
            Name = Dataset_Source,
            Properties = new DatasetProperties()
            {
                LinkedServiceName = "LinkedService-AzureStorage",
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
    
    client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,
    new DatasetCreateOrUpdateParameters()
    {
        Dataset = new Dataset()
        {
            Name = Dataset_Destination,
            Properties = new DatasetProperties()
            {
    
                LinkedServiceName = "LinkedService-AzureStorage",
                TypeProperties = new AzureBlobDataset()
                {
                    FolderPath = "adftutorial/apifactoryoutput/{Slice}",
                    PartitionedBy = new Collection<Partition>()
                    {
                        new Partition()
                        {
                            Name = "Slice",
                            Value = new DateTimePartitionValue()
                            {
                                Date = "SliceStart",
                                Format = "yyyyMMdd-HH"
                            }
                        }
                    }
                },
    
                Availability = new Availability()
                {
                    Frequency = SchedulePeriod.Hour,
                    Interval = 1,
                },
            }
        }
    });
    ```
10. Fügen Sie den folgenden Code, mit dem eine **Pipeline erstellt und aktiviert** wird, der **Main**-Methode hinzu. Diese Pipeline verfügt über eine **CopyActivity**, die **BlobSource** als Quelle und **BlobSink** als Senke verwendet.

    Die Kopieraktivität dient zum Verschieben von Daten in Azure Data Factory. Sie basiert auf einem global verfügbaren Dienst, mit dem Daten zwischen verschiedenen Datenspeichern sicher, zuverlässig und skalierbar kopiert werden können. Ausführliche Informationen zur Kopieraktivität finden Sie im Artikel [Datenverschiebungsaktivitäten](data-factory-data-movement-activities.md) .

    ```csharp
    // create a pipeline
    Console.WriteLine("Creating a pipeline");
    DateTime PipelineActivePeriodStartTime = new DateTime(2014, 8, 9, 0, 0, 0, 0, DateTimeKind.Utc);
    DateTime PipelineActivePeriodEndTime = PipelineActivePeriodStartTime.AddMinutes(60);
    string PipelineName = "PipelineBlobSample";
    
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
                        Name = "BlobToBlob",
                        Inputs = new List<ActivityInput>()
                        {
                            new ActivityInput()
                {
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
    ```
11. Fügen Sie die folgende Hilfsmethode, die von der **Main**-Methode verwendet wird, der **Program**-Klasse hinzu. Diese Methode öffnet ein Dialogfeld, in dem Sie den **Benutzernamen** und das **Kennwort** bereitstellen können, mit denen Sie sich beim Azure-Portal anmelden.

    ```csharp
    public static async Task<string> GetAuthorizationHeader()
    {
        var context = new AuthenticationContext(ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] + ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);
        AuthenticationResult result = await context.AcquireTokenAsync(
            resource: ConfigurationManager.AppSettings["WindowsManagementUri"],
            clientId: ConfigurationManager.AppSettings["AdfClientId"],
            redirectUri: new Uri(ConfigurationManager.AppSettings["RedirectUri"]),
            promptBehavior: PromptBehavior.Always);

        if (result != null)
            return result.AccessToken;

        throw new InvalidOperationException("Failed to acquire token");
    }
    ```
12. Fügen Sie den folgenden Code zur Methode **Main** hinzu, um den Status eines Datenslices des Ausgabedatasets abzurufen. In diesem Beispiel wird nur ein Slice erwartet.

    ```csharp
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
    ```
13. **(optional)** Fügen Sie der **Main**-Methode den folgenden Code hinzu, um Ausführungsdetails für einen Datenslice abzurufen.

    ```csharp
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
        });
    
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
    ```
14. Erweitern Sie im Projektmappen-Explorer das Projekt (**DataFactoryAPITestApp**), klicken Sie mit der rechten Maustaste auf **Verweise**, und klicken Sie auf **Verweis hinzufügen**. Aktivieren Sie das Kontrollkästchen für das `System.Configuration` -Assembly, und klicken Sie auf **OK**.
15. Erstellen Sie die Konsolenanwendung. Klicken Sie im Menü auf **Erstellen** und dann auf **Projektmappe erstellen**.
16. Vergewissern Sie sich, dass sich mindestens eine Datei im "adftutorial"-Container im Azure Blob-Speicher befindet. Ist dies nicht der Fall, erstellen Sie die Datei "Emp.txt" mit folgendem Inhalt im Editor, und laden Sie sie anschließend in den "adftutorial"-Container hoch.

    ```
    John, Doe
    Jane, Doe
    ```
17. Führen Sie das Beispiel aus, indem Sie im Menü auf **Debuggen** -> **Debuggen starten** klicken. Wenn angezeigt wird, dass die **Ausführungsdetails für einen Datenslice abgerufen werden**, warten Sie einige Minuten, und drücken Sie dann die **EINGABETASTE**.
18. Vergewissern Sie sich mithilfe des Azure-Portals, dass die Data Factory **APITutorialFactory** mit folgenden Artefakten erstellt wird:
    * Verknüpfter Dienst: **LinkedService_AzureStorage**
    * Dataset: **DatasetBlobSource** und **DatasetBlobDestination**
    * Pipeline: **PipelineBlobSample**
19. Vergewissern Sie sich, dass im Ordner **apifactoryoutput** im Container **adftutorial** eine Ausgabedatei erstellt wird.

## <a name="log-in-without-popup-dialog-box"></a>Anmeldung ohne Popupdialogfeld
Der obige Beispielcode in der exemplarischen Vorgehensweise startet ein Dialogfeld zur Eingabe der Azure-Anmeldeinformationen. Weitere Informationen für den Fall, dass die Anmeldung programmgesteuert ohne ein Dialogfeld erfolgen soll, finden Sie unter [Authentifizieren eines Dienstprinzipals mit dem Azure Resource Manager](../azure-resource-manager/resource-group-authenticate-service-principal.md).

> [!IMPORTANT]
> Fügen Sie Azure Active Directory eine Webanwendung hinzu, und notieren Sie die Client-ID und den geheimen Clientschlüssel der Anwendung.
>
>

### <a name="example"></a>Beispiel
Erstellen Sie die GetAuthorizationHeaderNoPopup-Methode.

```csharp
public static async Task<string> GetAuthorizationHeaderNoPopup()
{
    var authority = new Uri(new Uri("https://login.windows.net"), ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);
    var context = new AuthenticationContext(authority.AbsoluteUri);
    var credential = new ClientCredential(
        ConfigurationManager.AppSettings["AdfClientId"],
    ConfigurationManager.AppSettings["AdfClientSecret"]);
    
    AuthenticationResult result = await context.AcquireTokenAsync(
        ConfigurationManager.AppSettings["WindowsManagementUri"],
    credential);

    if (result != null)
        return result.AccessToken;

    throw new InvalidOperationException("Failed to acquire token");
}
```

Ersetzen Sie in der **Main**-Funktion den Aufruf **GetAuthorizationHeader** durch einen Aufruf von **GetAuthorizationHeaderNoPopup**:

```csharp
TokenCloudCredentials aadTokenCredentials =
    new TokenCloudCredentials(
    ConfigurationManager.AppSettings["SubscriptionId"],
    GetAuthorizationHeaderNoPopup().Result);
```

Hier wird gezeigt, wie Sie die Active Directory-Anwendung und den Dienstprinzipal erstellen und ihn anschließend der Rolle „Mitwirkender von Data Factory“ zuweisen:

1. Erstellen Sie die AD-Anwendung.

    ```PowerShell
    $azureAdApplication = New-AzureRmADApplication -DisplayName "MyADAppForADF" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.myadappforadf.org/example" -Password "Pass@word1"
    ```
2. Erstellen Sie den AD-Dienstprinzipal.

    ```PowerShell
    New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId
    ```
3. Fügen Sie den Dienstprinzipal der Rolle „Mitwirkender von Data Factory“ hinzu.

    ```PowerShell
    New-AzureRmRoleAssignment -RoleDefinitionName "Data Factory Contributor" -ServicePrincipalName $azureAdApplication.ApplicationId.Guid
    ```
4. Rufen Sie die Anwendungs-ID ab.

    ```PowerShell
    $azureAdApplication
    ```

Notieren Sie sich die Anwendungs-ID und das Kennwort (geheimer Clientschlüssel), und verwenden Sie beides in der exemplarischen Vorgehensweise.

## <a name="get-azure-subscription-and-tenant-ids"></a>Abrufen von Azure-Abonnement- und Mandanten-ID
Wenn Sie nicht die aktuelle Version von PowerShell auf Ihrem Computer installiert haben, befolgen Sie die Anweisungen im Artikel [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs) , um sie zu installieren.

1. Starten Sie Azure PowerShell, und führen Sie den folgenden Befehl aus.
2. Führen Sie den folgenden Befehl aus, und geben Sie den Benutzernamen und das Kennwort ein, den bzw. das Sie bei der Anmeldung beim Azure-Portal verwendet haben:

    ```PowerShell
    Login-AzureRmAccount
    ```

    Wenn Sie nur ein einziges Azure-Abonnement mit diesem Konto verknüpft haben, müssen Sie die nächsten beiden Schritte nicht ausführen.
3. Führen Sie den folgenden Befehl aus, um alle Abonnements für dieses Konto anzuzeigen:

    ```PowerShell
    Get-AzureRmSubscription
    ```
4. Führen Sie den folgenden Befehl aus, um das gewünschte Abonnement auszuwählen: Ersetzen Sie **NameOfAzureSubscription** durch den Namen Ihres Azure-Abonnements:

    ```PowerShell
    Get-AzureRmSubscription -SubscriptionName NameOfAzureSubscription | Set-AzureRmContext
    ```PowerShell

Note down the **SubscriptionId** and **TenantId** values.

