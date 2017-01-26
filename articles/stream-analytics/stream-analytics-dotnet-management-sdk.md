---
title: "Management .NET SDK für Stream Analytics | Microsoft Docs"
description: "Führen Sie erste Schritte mit dem Stream Analytics Management .NET SDK durch. Erfahren Sie, wie Sie Analyseaufträge einrichten und ausführen: Erstellen Sie ein Projekt, Eingaben, Ausgaben und Transformationen."
keywords: .NET SDK, Analyse-API
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 5e93de87-0c6f-4f4b-be98-08d63f832897
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 01/24/2017
ms.author: jeffstok
translationtype: Human Translation
ms.sourcegitcommit: 27df1166a23e3ed89fdc86f861353c80a4a467ad
ms.openlocfilehash: 55fc17009a31c84f7ef23140b31bf53858e56ec7


---
# <a name="management-net-sdk-set-up-and-run-analytics-jobs-using-the-azure-stream-analytics-api-for-net"></a>Management .NET SDK: Einrichten und Ausführen von Analyseaufträgen mit der Azure Stream Analytics-API für .NET
Erfahren Sie, wie Sie mit der Azure Stream Analytics-API für .NET über das Management .NET SDK Analyseaufträge einrichten und ausführen. Richten Sie ein Projekt ein, erstellen Sie Eingabe- und Ausgabequellen sowie Transformationen, und starten und beenden Sie Aufträge. Für Ihre Analyseaufträge können Sie Daten aus dem Blob-Speicher oder einem Event Hub streamen.

Weitere Informationen finden Sie in der [Referenzdokumentation zur Verwaltung für die Stream Analytics-API für .NET](https://msdn.microsoft.com/library/azure/dn889315.aspx).

Azure Stream Analytics ist ein vollständig verwalteter Dienst, der eine geringe Latenz, Hochverfügbarkeit und eine skalierbare komplexe Ereignisverarbeitung durch das Streaming von Daten in der Cloud bietet. Stream Analytics ermöglicht Kunden, Streaming-Aufträge zur Analyse von Datenströmen einzurichten und Analysen nahezu in Echtzeit durchzuführen.  

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie mit diesem Artikel beginnen können, benötigen Sie Folgendes:

* Installieren Sie Visual Studio 2012 oder 2013.
* Laden Sie das [Azure .NET SDK](https://azure.microsoft.com/downloads/)herunter, und installieren Sie es.
* Erstellen Sie in Ihrem Abonnement eine Azure-Ressourcengruppe. Nachfolgend ist ein Azure PowerShell-Beispielskript aufgeführt. Informationen zu Azure PowerShell finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs).  

        # Log in to your Azure account
        Add-AzureAccount

        # Select the Azure subscription you want to use to create the resource group
        Select-AzureSubscription -SubscriptionName <subscription name>

            # If Stream Analytics has not been registered to the subscription, remove the remark symbol (#) to run the Register-AzureRMProvider cmdlet to register the provider namespace
            #Register-AzureRMProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'

        # Create an Azure resource group
        New-AzureResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>


* Richten Sie eine Eingabequelle und ein Ausgabeziel ein. Anweisungen zum Einrichten einer Beispieleingabe finden Sie unter [Hinzufügen von Eingaben](stream-analytics-add-inputs.md), und Anweisungen zum Einrichten einer Beispielausgabe finden Sie unter [Hinzufügen von Ausgaben](stream-analytics-add-outputs.md).

## <a name="set-up-a-project"></a>Einrichten eines Projekts
Verwenden Sie zum Erstellen eines Analyseauftrags die Stream Analytics-API für .NET, und richten Sie zuerst das Projekt ein.

1. Erstellen Sie eine Visual Studio C# .NET-Konsolenanwendung.
2. Führen Sie in der Paket-Manager-Konsole die folgenden Befehle zum Installieren der NuGet-Pakete aus. Das erste ist das Azure Stream Analytics Management .NET SDK. Das zweite ist der Azure Active Directory-Client, der für die Authentifizierung verwendet wird.
   
        Install-Package Microsoft.Azure.Management.StreamAnalytics
        Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
3. Fügen Sie der Datei "App.config" den folgenden **appSettings** -Abschnitt hinzu.
   
        <appSettings>
          <!--CSM Prod related values-->
          <add key="ActiveDirectoryEndpoint" value="https://login.windows.net/" />
          <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
          <add key="WindowsManagementUri" value="https://management.core.windows.net/" />
          <add key="AsaClientId" value="1950a258-227b-4e31-a9cf-717495945fc2" />
          <add key="RedirectUri" value="urn:ietf:wg:oauth:2.0:oob" />
          <add key="SubscriptionId" value="YOUR AZURE SUBSCRIPTION" />
          <add key="ActiveDirectoryTenantId" value="YOU TENANT ID" />
        </appSettings>

    Ersetzen Sie die Werte für **SubscriptionId** und **ActiveDirectoryTenantId** durch die IDs Ihres Azure-Abonnements und -Mandanten. Sie können diese Werte durch Ausführen des folgenden Azure PowerShell-Cmdlets abrufen:

        Get-AzureAccount

1. Fügen Sie die folgenden **using** -Anweisungen zur Quelldatei (Program.cs) im Projekt hinzu.
   
        using System;
        using System.Configuration;
        using System.Threading;
        using Microsoft.Azure;
        using Microsoft.Azure.Management.StreamAnalytics;
        using Microsoft.Azure.Management.StreamAnalytics.Models;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
2. Fügen Sie eine Authentifizierungshilfsmethode hinzu:

   ```   
   public static string GetAuthorizationHeader()
   {
   
       AuthenticationResult result = null;
       var thread = new Thread(() =>
       {
           try
           {
               var context = new AuthenticationContext(
                   ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] +
                   ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);
   
               result = context.AcquireToken(
                   resource: ConfigurationManager.AppSettings["WindowsManagementUri"],
                   clientId: ConfigurationManager.AppSettings["AsaClientId"],
                   redirectUri: new Uri(ConfigurationManager.AppSettings["RedirectUri"]),
                   promptBehavior: PromptBehavior.Always);
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
   ```  

## <a name="create-a-stream-analytics-management-client"></a>Erstellen eines Stream Analytics-Verwaltungsclients
Mithilfe eines **StreamAnalyticsManagementClient** -Objekts können Sie den Auftrag und die Auftragskomponenten wie Eingabe, Ausgabe und Transformation verwalten.

Fügen Sie den folgenden Code am Anfang der Methode **Main** hinzu:

    string resourceGroupName = "<YOUR AZURE RESOURCE GROUP NAME>";
    string streamAnalyticsJobName = "<YOUR STREAM ANALYTICS JOB NAME>";
    string streamAnalyticsInputName = "<YOUR JOB INPUT NAME>";
    string streamAnalyticsOutputName = "<YOUR JOB OUTPUT NAME>";
    string streamAnalyticsTransformationName = "<YOUR JOB TRANSFORMATION NAME>";

    // Get authentication token
    TokenCloudCredentials aadTokenCredentials =
        new TokenCloudCredentials(
            ConfigurationManager.AppSettings["SubscriptionId"],
            GetAuthorizationHeader());

    // Create Stream Analytics management client
    StreamAnalyticsManagementClient client = new StreamAnalyticsManagementClient(aadTokenCredentials);

Der Wert der Variablen **resourceGroupName** sollte mit dem Namen der Ressourcengruppe übereinstimmen, die Sie in den Vorbereitungsschritten erstellt oder ausgewählt haben.

Informationen zum Automatisieren der Darstellung von Anmeldeinformationen bei der Auftragserstellung finden Sie unter [Authentifizieren eines Dienstprinzipals mit dem Azure-Ressourcen-Manager](../azure-resource-manager/resource-group-authenticate-service-principal.md).

In den verbleibenden Abschnitten dieses Artikels wird davon ausgegangen, dass dieser Code am Anfang der Methode **Main** enthalten ist.

## <a name="create-a-stream-analytics-job"></a>Erstellen eines Stream Analytics-Auftrags
Der folgende Code erstellt einen Stream Analytics-Auftrag unter der Ressourcengruppe, die Sie definiert haben. Sie fügen dem Auftrag später eine Eingabe, Ausgabe und Transformation hinzu.

    // Create a Stream Analytics job
    JobCreateOrUpdateParameters jobCreateParameters = new JobCreateOrUpdateParameters()
    {
        Job = new Job()
        {
            Name = streamAnalyticsJobName,
            Location = "<LOCATION>",
            Properties = new JobProperties()
            {
                EventsOutOfOrderPolicy = EventsOutOfOrderPolicy.Adjust,
                Sku = new Sku()
                {
                    Name = "Standard"
                }
            }
        }
    };

    JobCreateOrUpdateResponse jobCreateResponse = client.StreamingJobs.CreateOrUpdate(resourceGroupName, jobCreateParameters);


## <a name="create-a-stream-analytics-input-source"></a>Erstellen einer Stream Analytics-Eingabequelle
Der folgende Code erstellt eine Stream Analytics-Eingabequelle mit dem Blob-Eingabequellentyp und mit CSV-Serialisierung. Verwenden Sie zum Erstellen einer Event Hub-Eingabequelle **EventHubStreamInputDataSource** anstelle von **BlobStreamInputDataSource**. In ähnlicher Weise können Sie den Serialisierungstyp der Eingabequelle anpassen.

    // Create a Stream Analytics input source
    InputCreateOrUpdateParameters jobInputCreateParameters = new InputCreateOrUpdateParameters()
    {
        Input = new Input()
        {
            Name = streamAnalyticsInputName,
            Properties = new StreamInputProperties()
            {
                Serialization = new CsvSerialization
                {
                    Properties = new CsvSerializationProperties
                    {
                        Encoding = "UTF8",
                        FieldDelimiter = ","
                    }
                },
                DataSource = new BlobStreamInputDataSource
                {
                    Properties = new BlobStreamInputDataSourceProperties
                    {
                        StorageAccounts = new StorageAccount[]
                        {
                            new StorageAccount()
                            {
                                AccountName = "<YOUR STORAGE ACCOUNT NAME>",
                                AccountKey = "<YOUR STORAGE ACCOUNT KEY>"
                            }
                        },
                        Container = "samples",
                        PathPattern = ""
                    }
                }
            }
        }
    };

    InputCreateOrUpdateResponse inputCreateResponse =
        client.Inputs.CreateOrUpdate(resourceGroupName, streamAnalyticsJobName, jobInputCreateParameters);

Eingabequellen, gleichgültig ob vom Blob-Speicher oder einem Event Hub, sind an einen bestimmten Auftrag gebunden. Um die gleiche Eingabequelle für verschiedene Aufträge zu verwenden, müssen Sie die Methode erneut aufrufen und einen anderen Auftragsnamen angeben.

## <a name="test-a-stream-analytics-input-source"></a>Testen einer Stream Analytics-Eingabequelle
Die Methode **TestConnection** überprüft, ob der Stream Analytics-Auftrag sich mit der Eingabequelle verbinden kann. Ferner werden andere Aspekte getestet, die für die Eingabequelle spezifisch sind. Beispielsweise überprüft die Methode in der Blob-Eingabequelle, die Sie in einem früheren Schritt erstellt haben, ob das Paar aus dem Namen des Storage-Kontos und dem Schlüssel verwendet werden kann, um eine Verbindung mit dem Storage-Konto herzustellen. Zudem wird überprüft, ob der angegebene Container vorhanden ist.

    // Test input source connection
    DataSourceTestConnectionResponse inputTestResponse =
        client.Inputs.TestConnection(resourceGroupName, streamAnalyticsJobName, streamAnalyticsInputName);

## <a name="create-a-stream-analytics-output-target"></a>Erstellen eines Stream Analytics-Ausgabeziels
Das Erstellen eines Ausgabeziels ähnelt stark dem Erstellen einer Stream Analytics-Eingabequelle. Genau wie Eingabequellen sind Ausgabeziele an einen bestimmten Auftrag gebunden. Um dasselbe Ausgabeziel für verschiedene Aufträge zu verwenden, müssen Sie die Methode erneut aufrufen und einen anderen Auftragsnamen angeben.

Der folgende Code erstellt ein Ausgabeziel (Azure SQL-Datenbank). Sie können den Datentyp des Ausgabeziels und/oder den Serialisierungstyp anpassen.

    // Create a Stream Analytics output target
    OutputCreateOrUpdateParameters jobOutputCreateParameters = new OutputCreateOrUpdateParameters()
    {
        Output = new Output()
        {
            Name = streamAnalyticsOutputName,
            Properties = new OutputProperties()
            {
                DataSource = new SqlAzureOutputDataSource()
                {
                    Properties = new SqlAzureOutputDataSourceProperties()
                    {
                        Server = "<YOUR DATABASE SERVER NAME>",
                        Database = "<YOUR DATABASE NAME>",
                        User = "<YOUR DATABASE LOGIN>",
                        Password = "<YOUR DATABASE LOGIN PASSWORD>",
                        Table = "<YOUR DATABASE TABLE NAME>"
                    }
                }
            }
        }
    };

    OutputCreateOrUpdateResponse outputCreateResponse =
        client.Outputs.CreateOrUpdate(resourceGroupName, streamAnalyticsJobName, jobOutputCreateParameters);

## <a name="test-a-stream-analytics-output-target"></a>Testen eines Stream Analytics-Ausgabeziels
Ein Stream Analytics-Ausgabeziel verfügt auch über die Methode **TestConnection** zum Testen von Verbindungen.

    // Test output target connection
    DataSourceTestConnectionResponse outputTestResponse =
        client.Outputs.TestConnection(resourceGroupName, streamAnalyticsJobName, streamAnalyticsOutputName);

## <a name="create-a-stream-analytics-transformation"></a>Erstellen einer Stream Analytics-Transformation
Der folgende Code erstellt eine Stream Analytics-Transformation mit der Abfrage "select * from Input" und gibt an, dass eine Streamingeinheit für den Stream Analytics-Auftrag zuordnet werden soll. Weitere Informationen zum Anpassen von Streamingeinheiten finden Sie unter [Skalieren von Azure Stream Analytics-Aufträgen](stream-analytics-scale-jobs.md).

    // Create a Stream Analytics transformation
    TransformationCreateOrUpdateParameters transformationCreateParameters = new TransformationCreateOrUpdateParameters()
    {
        Transformation = new Transformation()
        {
            Name = streamAnalyticsTransformationName,
            Properties = new TransformationProperties()
            {
                StreamingUnits = 1,
                Query = "select * from Input"
            }
        }
    };

    var transformationCreateResp =
        client.Transformations.CreateOrUpdate(resourceGroupName, streamAnalyticsJobName, transformationCreateParameters);

Wie Eingabe und Ausgabe ist auch eine Transformation an den jeweiligen Streaming Analytics-Auftrag gebunden, unter dem Sie erstellt wurde.

## <a name="start-a-stream-analytics-job"></a>Starten eines Stream Analytics-Auftrags
Nach dem Erstellen eines Stream Analytics-Auftrags und seiner Eingabe(n), Ausgabe(n) und Transformation können Sie den Auftrag durch Aufrufen der Methode **Start** starten.

Der folgende Beispielcode startet einen Stream Analytics-Auftrag mit der benutzerdefinierten Ausgabestartzeit "12. Dezember 2012, 12:12:12 UTC":

    // Start a Stream Analytics job
    JobStartParameters jobStartParameters = new JobStartParameters
    {
        OutputStartMode = OutputStartMode.CustomTime,
        OutputStartTime = new DateTime(2012, 12, 12, 0, 0, 0, DateTimeKind.Utc)
    };

    LongRunningOperationResponse jobStartResponse = client.StreamingJobs.Start(resourceGroupName, streamAnalyticsJobName, jobStartParameters);



## <a name="stop-a-stream-analytics-job"></a>Beenden eines Stream Analytics-Auftrags
Sie können einen laufenden Stream Analytics-Auftrag beenden, indem Sie die Methode **Stop** aufrufen.

    // Stop a Stream Analytics job
    LongRunningOperationResponse jobStopResponse = client.StreamingJobs.Stop(resourceGroupName, streamAnalyticsJobName);

## <a name="delete-a-stream-analytics-job"></a>Löschen eines Stream Analytics-Auftrags
Die Methode **Delete** löscht den Auftrag sowie die zugrunde liegenden Unterressourcen, einschließlich Eingabe(n), Ausgabe(n) und Transformation des Auftrags.

    // Delete a Stream Analytics job
    LongRunningOperationResponse jobDeleteResponse = client.StreamingJobs.Delete(resourceGroupName, streamAnalyticsJobName);


## <a name="get-support"></a>Support
Um Hilfe zu erhalten, nutzen Sie unser [Azure Stream Analytics-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Nächste Schritte
Sie haben nun die Grundlagen der Verwendung eines .NET SDK zum Erstellen und Ausführen von Analyseaufträgen kennengelernt. Weitere Informationen finden Sie in den folgenden Artikeln:

* [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
* [Erste Schritte mit Azure Stream Analytics](stream-analytics-get-started.md)
* [Skalieren von Azure Stream Analytics-Aufträgen](stream-analytics-scale-jobs.md)
* [Verwenden des Azure Stream Analytics Management .NET SDK](https://msdn.microsoft.com/library/azure/dn889315.aspx).
* [Stream Analytics Query Language Reference (in englischer Sprache)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenz zur Azure Stream Analytics-Verwaltungs-REST-API](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Image references-->
[5]: ./media/markdown-template-for-new-articles/octocats.png
[6]: ./media/markdown-template-for-new-articles/pretty49.png
[7]: ./media/markdown-template-for-new-articles/channel-9.png


<!--Link references-->
[azure.blob.storage]: http://azure.microsoft.com/documentation/services/storage/
[azure.blob.storage.use]: http://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/

[azure.event.hubs]: http://azure.microsoft.com/services/event-hubs/
[azure.event.hubs.developer.guide]: http://msdn.microsoft.com/library/azure/dn789972.aspx

[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.forum]: http://go.microsoft.com/fwlink/?LinkId=512151

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.developer.guide]: stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301



<!--HONumber=Dec16_HO4-->


