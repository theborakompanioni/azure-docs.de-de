<properties
    pageTitle="Nutzen eines Machine Learning-Webdiensts | Microsoft Azure"
    description="Sobald ein Machine Learning-Dienst bereitgestellt wurde, kann der zur Verfügung gestellte RESTFul-Webdienst als Anforderung-/Antwort-Dienst oder als Stapelausführungsdienst genutzt werden."
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="tbd"
    ms.date="10/04/2016"
    ms.author="garye" />



# <a name="how-to-consume-an-azure-machine-learning-web-service-that-has-been-deployed-from-a-machine-learning-experiment"></a>Nutzen eines Azure Machine Learning-Webdiensts, der von einem Machine Learning-Experiment aus bereitgestellt wurde

## <a name="introduction"></a>Einführung

Beim Bereitstellen als Webdienst bieten Azure Machine Learning-Experimente eine REST-API und JSON-formatierte Meldungen, die von einer Vielzahl von Geräten und Plattformen genutzt werden kann. Das Azure Machine Learning-Portal bietet Code zum Aufrufen des Webdiensts in R, C# und Python. 

Dienste können in jeder beliebigen Programmiersprache und von jedem Gerät aus aufgerufen werden, sofern die drei folgenden Kriterien erfüllt sind:

* Netzwerkverbindung
* SSL-Funktionen zum Ausführen von HTTPS-Anforderungen
* Möglichkeit zum Analysieren von JSON (manuell oder durch Supportbibliotheken)

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]  

Ein Azure Machine Learning-Webdienst kann auf zwei Arten genutzt werden: als Request-Response Service Anforderung-/Antwort-Dienst oder als Batch Execution Service (Stapelausführungsdienst). In jedem Szenario wird die Funktionalität über den RESTFul-Webdienst bereitgestellt, der für die Nutzung verfügbar gemacht wird, sobald Sie das Experiment bereitstellen.

> [AZURE.TIP] Eine einfache Möglichkeit zum Erstellen einer Web-App für den Zugriff auf Ihren Vorhersagewebdienst finden Sie unter [Verwenden eines Azure Machine Learning-Webdiensts mit einer Web-App-Vorlage](machine-learning-consume-web-service-with-web-app-template.md).

<!-- When this article gets published, fix the link and uncomment
For more information on how to manage Azure Machine Learning Web service endpoints using the REST API, see **Azure machine learning Web service endpoints**.
-->

Weitere Informationen zum Erstellen und Bereitstellen eines Machine Learning-Webdiensts finden Sie unter [Bereitstellen eines Azure Machine Learning-Webdiensts][Veröffentlichen]. Eine detaillierte Anleitung zum Erstellen und Bereitstellen eines Machine Learning-Experiments finden Sie unter [Entwickeln einer Predictive Analytics-Lösung mit Azure Machine Learning][Exemplarische Vorgehensweise].

## <a name="requestresponse-service-rrs"></a>Anforderung-/Antwort-Dienst (Request-Response Service RRS)

Ein Anforderung-/Antwort-Dienst (Request-Response Service, RRS) ist ein überaus skalierbarer Webdienst mit niedriger Latenz, mit dem eine Schnittstelle für die aus einem Azure Machine Learning Studio-Experiment erstellten und bereitgestellten zustandslosen Modelle bereitgestellt werden kann. Dadurch werden Szenarien ermöglicht, bei denen die verarbeitende Anwendung eine Antwort in Echtzeit erwartet.

RRS akzeptiert eine einzelne oder mehrere Zeilen von Eingabeparametern und kann eine einzelne oder mehrere Zeilen als Ausgabe generieren. Die Ausgabereihen können jeweils mehrere Spalten enthalten.

Ein Beispiel für RRS ist das Überprüfen der Authentizität einer Anwendung. Hunderte bis Millionen von Installationen einer Anwendung können in diesem Fall erwartet werden. Wenn die Anwendung gestartet wird, ruft sie den RRS-Dienst mit der entsprechenden Eingabe auf. Die Anwendung empfängt dann eine Überprüfungsantwort vom Dienst, die das Ausführen der Anwendung zulässt oder blockiert.


## <a name="batch-execution-service-bes"></a>Stapelausführungsdienst (Batch Execution Service BES)

Ein Stapelausführungsdienst (Batch Execution Service, BES) ist ein Dienst zur asynchronen Bewertung eines umfangreichen Stapels von Datensätzen. Die Eingabe für den BES enthält einen Stapel von Datensätzen aus verschiedenen Quellen, z.B. Blobs, Tabellen in Azure, SQL Azure, HDInsight (beispielsweise Ergebnisse einer Hive-Abfrage) oder HTTP-Quellen. Die Ausgabe des BES enthält die Ergebnisse der Wertung. Die Ergebnisse werden in eine Datei im Azure-Blob-Speicher geschrieben, und Daten aus dem Speicherendpunkt werden in der Antwort zurückgegeben.

Ein BES ist nützlich, wenn Antworten nicht sofort benötigt werden, z. B. für regelmäßig geplante Bewertungen für einzelne Benutzer oder Geräte im Internet der Dinge (IOT).

## <a name="examples"></a>Beispiele

Zum Erläutern der Funktionsweise von RRS und BES greifen wir auf ein Beispiel eines Azure-Webdiensts zurück. Dieser Dienst wird in einem IOT-Szenario (Internet der Dinge) verwendet werden. Der Einfachheit halber sendet unser Gerät nur einen Wert, `cog_speed`, und erhält eine einzelne Antwort zurück.

Nachdem das Experiment bereitgestellt wurde, sind vier Arten von Informationen erforderlich, um den RRS- oder BES-Dienst aufzurufen.

* **API-Schlüssel** oder **Primärschlüssel** des Diensts
* **Anforderungs-URI** des Diensts
* **Anforderungsheader** und **Text** der erwarteten API
* **Antwortheader** und **Text** der erwarteten API

Die Art und Weise, in der Sie diese Informationen finden, hängt davon ab, welche Art von Dienst Sie bereitgestellt haben: einen neuen Webdienst oder klassischen Webdienst.

### <a name="information-location-in-the-azure-machine-learning-web-services-portal"></a>Auffinden von Informationen im Azure Machine Learning Web Services-Portal 

So finden Sie die benötigten Informationen

1. Melden Sie sich beim [Azure Machine Learning Web Services-Portal][Web Services-Portal] an.
2. Klicken Sie auf **Web Services** oder **Classic Web Services**.
3. Klicken Sie auf den Webdienst, mit dem Sie arbeiten. 
4. Klicken Sie bei Verwendung eines klassischen Webdiensts auf den Endpunkt, mit dem Sie arbeiten.

Die Informationen befinden sich auf diesen Seiten:

* Den **Primary key** (Primärschlüssel) finden Sie auf der Seite **Consume**.
* Den **Request URI ** (Anforderungs-URI) finden Sie auf der Seite **Consume**. 
* **Anforderungsheader**, **Antwortheader** und **Text** der erwarteten API stehen auf der Seite **Swagger API** zur Verfügung.

### <a name="information-locations-in-machine-learning-studio-classic-web-service-only"></a>Auffinden von Information in Machine Learning Studio (nur klassischer Webdienst)

Sie finden die benötigten Informationen an zwei Stellen: in Machine Learning Studio oder im Azure Machine Learning Web Services-Portal.

So finden Sie die benötigten Informationen in Machine Learning Studio

1. Melden Sie sich bei [Azure Machine Learning Studio][ML Studio] an.
2. Klicken Sie auf der linken Seite des Bildschirms auf **WEB SERVICES**.
3. Klicken Sie auf den Webdienst, mit dem Sie arbeiten. 

Die Informationen befinden sich auf diesen Seiten:

* Der **API-Schlüssel** ist im **Dashboard** des Diensts verfügbar. 
* Den **Request URI ** (Anforderungs-URI) finden Sie auf der API-Hilfeseite.
* **Anforderungsheader**, **Antwortheader** und **Text** der erwarteten API stehen auf der API-Hilfeseite zur Verfügung.


Um die API-Hilfeseite zuzugreifen, klicken Sie entsprechend Ihrer Aufgabe entweder auf den Link **REQUEST/RESPONSE** oder **BATCH EXECUTION**.

So finden Sie die benötigten Informationen im Azure Machine Learning Web Services-Portal

1. Melden Sie sich beim [Azure Machine Learning Web Services-Portal][Web Services-Portal] an.
2. Klicken Sie auf **Classic Web Services**.
3. Klicken Sie auf den Webdienst, mit dem Sie arbeiten. 
4. Klicken Sie auf den Endpunkt, mit dem Sie arbeiten.

Die Informationen befinden sich auf diesen Seiten:

* Den **Primary key** (Primärschlüssel) finden Sie auf der Seite **Consume**.
* Den **Request URI ** (Anforderungs-URI) finden Sie auf der Seite **Consume**. 
* **Anforderungsheader**, **Antwortheader** und **Text** der erwarteten API stehen auf der Seite **Swagger API** zur Verfügung.

In den folgenden beiden Beispielen wird die Sprache C# verwendet, um den erforderlichen Code zu veranschaulichen.

### <a name="rrs-example"></a>RRS-Beispiel



Die folgende Beispielanforderung zeigt die API-Eingabe der Nutzlast für den API-Aufruf unseres Beispieldiensts. Für einen klassischen Webdienst finden Sie Nutzlastbeispiele auf der **API-Hilfeseite** oder auf der Seite **Swagger API** im Machine Learning Web Services-Portal. Für einen neuen Webdienst finden Sie Nutzlastbeispiele auf der Seite **Swagger API** im Machine Learning Web Services-Portal.

**Beispiel für eine Anforderung**

    {
      "Inputs": {
        "input1": {
          "ColumnNames": [
            "cog_speed"
          ],
          "Values": [
            [
              "0"
            ],
            [
              "1"
            ]
          ]
        }
      },
      "GlobalParameters": {}
    }


Das folgende Beispiel zeigt die API-Antwort für den Dienst.

**Beispiel für eine Antwort**

    {
      "Results": {
        "output1": {
          "type": "DataTable",
          "value": {
            "ColumnNames": [
              "cog_speed"
            ],
            "ColumnTypes": [
              "Numeric"
            ].
          "Values": [
            [
              "0"
            ],
            [
              "1"
            ]
          ]
        }
      },
      "GlobalParameters": {}
    }

Es folgt das Codebeispiel für die C#-Implementierung. Für einen klassischen Webdienst finden Sie Codebeispiele unten auf der **API-Hilfeseite** oder unten auf der Seite **Consume**. Für einen neuen Webdienst finden Sie Codebeispiele unten auf der Seite **Consume**.

**Beispielcode in C#**

    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Net.Http;
    using System.Net.Http.Formatting;
    using System.Net.Http.Headers;
    using System.Text;
    using System.Threading.Tasks;

    namespace CallRequestResponseService
    {
        public class StringTable
        {
            public string[] ColumnNames { get; set; }
            public string[,] Values { get; set; }
        }

        class Program
        {
            static void Main(string[] args)
            {
                InvokeRequestResponseService().Wait();
            }

            static async Task InvokeRequestResponseService()
            {
                using (var client = new HttpClient())
                {
                    var scoreRequest = new
                    {
                        Inputs = new Dictionary<string, StringTable> () {
                            {
                                "input1",
                                new StringTable()
                                {
                                    ColumnNames = new string[] {"cog_speed"},
                                    Values = new string[,] {  { "0"},  { "1"}  }
                                }
                            },
                        GlobalParameters = new Dictionary<string, string>() { }
                    };

                    const string apiKey = "abc123"; // Replace this with the API key for the Web service
                    client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue( "Bearer", apiKey);

                    client.BaseAddress = new Uri("https://ussouthcentral.services.azureml.net/workspaces/<workspace id>/services/<service id>/execute?api-version=2.0&details=true");

                    // WARNING: The 'await' statement below can result in a deadlock if you are calling this code from the UI thread of an ASP.Net application.
                    // One way to address this would be to call ConfigureAwait(false) so that the execution does not attempt to resume on the original context.
                    // For instance, replace code such as:
                    //      result = await DoSomeTask()
                    // with the following:
                    //      result = await DoSomeTask().ConfigureAwait(false)

                    HttpResponseMessage response = await client.PostAsJsonAsync("", scoreRequest);

                    if (response.IsSuccessStatusCode)
                    {
                        string result = await response.Content.ReadAsStringAsync();
                        Console.WriteLine("Result: {0}", result);
                    }
                    else
                    {
                        Console.WriteLine("Failed with status code: {0}", response.StatusCode);
                    }
                }
            }
        }
    }

**Beispielcode in Java**

Der folgende Beispielcode veranschaulicht die Erstellung einer REST-API-Anforderung in Java. Es wird davon ausgegangen, dass die Variablen („apikey“ und „apiurl“) die erforderlichen API-Details aufweisen und dass die Variable „jsonBody“ über ein ordnungsgemäßes JSON-Objekt entsprechend der Erwartung der REST-API verfügt. Sie können den vollständigen Code von GitHub herunterladen: [https://github.com/nk773/AzureML_RRSApp](https://github.com/nk773/AzureML_RRSApp). Dieses Java-Beispiel erfordert die [Apache-HTTP-Clientbibliothek](https://hc.apache.org/downloads.cgi).

    /**
     * Download full code from github - [https://github.com/nk773/AzureML_RRSApp](https://github.com/nk773/AzureML_RRSApp)
     */
        /**
          * Call REST API for retrieving prediction from Azure ML 
          * @return response from the REST API
          */    
        public static String rrsHttpPost() {
        
            HttpPost post;
            HttpClient client;
            StringEntity entity;
        
            try {
                    // create HttpPost and HttpClient object
                    post = new HttpPost(apiurl);
                    client = HttpClientBuilder.create().build();
            
                    // setup output message by copying JSON body into 
                    // apache StringEntity object along with content type
                    entity = new StringEntity(jsonBody, HTTP.UTF_8);
                    entity.setContentEncoding(HTTP.UTF_8);
                    entity.setContentType("text/json");

                    // add HTTP headers
                    post.setHeader("Accept", "text/json");
                    post.setHeader("Accept-Charset", "UTF-8");
        
                    // set Authorization header based on the API key
                    post.setHeader("Authorization", ("Bearer "+apikey));
                    post.setEntity(entity);

                    // Call REST API and retrieve response content
                    HttpResponse authResponse = client.execute(post);
            
                    return EntityUtils.toString(authResponse.getEntity());
            
            }
            catch (Exception e) {
            
                    return e.toString();
            }
    
        }
    
        
 

### <a name="bes-example"></a>BES-Beispiel

Im Gegensatz zum RRS-Dienst ist der BES-Dienst asynchron. Dies bedeutet, dass die Bus-API einen auszuführenden Auftrag in die Warteschlange reiht und der Aufrufer den Auftragsstatus abruft, um zu sehen, wann dieser abgeschlossen wurde. Hier sind die Vorgänge, die derzeit für Batchaufträge unterstützt werden:

1. Erstellen (Einreichen) eines Batchauftrags
1. Starten des Batchauftrags
1. Abrufen des Status oder des Ergebnisses des Batchauftrags
1. Abbrechen eines Batchauftrag, der ausgeführt wird

**1. Erstellen Sie einen Batchausführungsauftrag**

Beim Erstellen eines Batchauftrags für Ihren Azure Machine Learning-Dienst können Sie mehrere Parameter angeben, die die Batchausführung definieren:

* **Input**: Ein Blobverweis auf den Speicherort der Eingabe des Batchauftrags.
* **GlobalParameters**: Eine Gruppe globaler Parameter, die für ein Experiment definiert werden können. Ein Azure Machine Learning-Experiment kann erforderliche und optionale Parameter haben, die die Ausführung des Diensts individuell anpassen. Außerdem wird vom Aufrufer erwartet, ggf. alle benötigten Parameter bereitzustellen. Diese Parameter werden als eine Auflistung von Schlüssel-Wert-Paaren angegeben.
* **Outputs**: Wenn für den Dienst eine oder mehrere Ausgaben definiert sind, kann der Aufrufer beliebig viele davon an einen Azure-Blobspeicherort umleiten. Durch Festlegen dieses Parameters können Sie die Ausgabe des Dienst an einem gewünschten Speicherort und unter einem vorhersagbaren Namen speichern, da ansonsten der Name des Ausgabeblobs zufällig generiert wird. 

    Der Dienst erwartet, dass der Inhalt der Ausgabe basierend auf seinem Typ in unterstützten Formaten gespeichert wird:
  - Datasetausgaben können als **.csv, .tsv, .arff** gespeichert werden.
  - Ausgaben trainierter Modelle müssen als **.ilearner** gespeichert werden.

    Sie geben die Überschreibungen des Ausgabespeicherorts als Sammlung von Ausgabenamen- oder Blobverweispaaren an. Der *Output*-Name ist der benutzerdefinierte Name eines bestimmten Ausgabeknotens. Der *Blobverweis* ist ein Verweis auf einen Azure-Blobspeicherort, an den die Ausgabe umgeleitet wird. Der *Output*-Name wird auf API-Hilfeseite des Diensts angezeigt.

Alle Auftragserstellungsparameter sind je nach Art Ihres Diensts optional. Dienste ohne definierten Eingabeknoten erfordern z.B. nicht, dass ein *Input*-Parameter an sie übergeben wird. Die Überschreibungsfunktion für den Ausgabespeicherort ist ebenso optional, da Ausgaben andernfalls im Standardspeicherkonto gespeichert werden, das für Ihren Azure Machine Learning-Arbeitsbereich eingerichtet wurde. Es folgt ein Beispiel einer Anforderungsnutzlast für einen Dienst, bei dem nur die Eingabe-Informationen bereitgestellt werden:

**Beispiel für eine Anforderung**

    {
      "Input": {
        "ConnectionString":     
        "DefaultEndpointsProtocol=https;AccountName=mystorageacct;AccountKey=mystorageacctKey",
        "RelativeLocation": "/mycontainer/mydatablob.csv",
        "BaseLocation": null,
        "SasBlobToken": null
      },
      "Outputs": null,
      "GlobalParameters": null
    }

Die Antwort auf die Batchauftragserstellungs-API ist die eindeutige Auftrags-ID, die mit Ihrem Auftrag verknüpft ist. Diese ID ist wichtig, da sie Ihre einzige Möglichkeit darstellt, für andere Vorgänge auf diesen Auftrag im System zu verweisen.  

**Beispiel für eine Antwort**

    "539d0bc2fde945b6ac986b851d0000f0" // The JOB_ID

**2. Starten eines Batchausführungsauftrags**

Beim Erstellen eines Batchauftrags wird dieser im System registriert und in den Zustand *Nicht gestartet* versetzt. Um die Ausführung des Auftrags zu planen, rufen Sie die **start**-API auf, die auf der API-Hilfeseite der Dienstendpunkts oder der Seite „Swagger API“ des Webdiensts beschrieben ist, und geben die Auftrags-ID an, die beim Erstellen des Auftrags vergeben wurde.

**3. Abrufen des Status eines Batchausführungsauftrags**

Sie können jederzeit den Status Ihres asynchronen Batchauftrags abfragen, indem Sie die Auftrags-ID an die *GetJobStatus*-API übergeben. Die API-Antwort enthält einen Indikator für den aktuellen Status des Auftrags sowie die Ergebnisse des Batchauftrags mit der Angabe, ob der Vorgang erfolgreich abgeschlossen wurde. Bei einem Fehler werden weitere Informationen über die tatsächlichen Gründe für den Fehler in der Eigenschaft *Details* zurückgegeben, wie in der folgenden Abbildung veranschaulicht:

**Antwortnutzlast**

    {
        "StatusCode": STATUS_CODE,
        "Results": RESULTS,
        "Details": DETAILS
    }

*StatusCode* kann wie folgt lauten:

* Nicht gestartet
* Wird ausgeführt
* Fehler
* Canceled
* Abgeschlossen

Die Eigenschaft *Results* wird nur aufgefüllt, wenn der Auftrag erfolgreich abgeschlossen wurde (andernfalls **NULL**). Nachdem der Auftrag abgeschlossen wurde und für den Dienst mindestens ein Ausgabeknoten definiert ist, werden die Ergebnisse als Auflistung von *[Ausgabename, Blobverweis]*-Paaren zurückgegeben, in der der Blobverweis ein schreibgeschützter SAS-Verweis auf das Blob mit dem Ergebnis ist.

**Beispiel für eine Antwort**

    {
        "Status Code": "Finished",
        "Results":
        {
            "dataOutput":
            {              
                "ConnectionString": null,
                "RelativeLocation": "outputs/dataOutput.csv",
                "BaseLocation": "https://mystorageaccount.blob.core.windows.net/",
                "SasBlobToken": "?sv=2013-08-15&sr=b&sig=ABCD&st=2015-04-04T05%3A39%3A55Z&se=2015-04-05T05%3A44%3A55Z&sp=r"              
            },
            "trainedModelOutput":
            {              
                "ConnectionString": null,
                "RelativeLocation": "models/trainedModel.ilearner",
                "BaseLocation": "https://mystorageaccount.blob.core.windows.net/",
                "SasBlobToken": "?sv=2013-08-15&sr=b&sig=EFGH%3D&st=2015-04-04T05%3A39%3A55Z&se=2015-04-05T05%3A44%3A55Z&sp=r"              
            },           
        },
        "Details": null
    }

**4. Abbrechen eines Batchausführungsauftrags**

Sie können einen Batchauftrag, der ausgeführt wird, jederzeit abbrechen, indem die festgelegte *CancelJob*-API aufgerufen und die Auftrags-ID übergeben wird. Das Abbrechen kann aus verschiedenen Gründen erfolgen, etwa weil der Auftrag zu lange dauert.

#### <a name="using-the-bes-sdk"></a>Verwenden des BES SDK

Das [NuGet-Paket mit dem BES SDK](http://www.nuget.org/packages/Microsoft.Azure.MachineLearning/) bietet Funktionen, die das Aufrufen des BES zur Bewertung im Batchmodus vereinfachen. Um das NuGet-Paket zu installieren, wählen Sie in Visual Studio im Menü **Tools** den **NuGet-Paket-Manager** aus und klicken dann auf **Paket-Manager-Konsole**.

Azure Machine Learning-Experimente, die als Webdienste bereitgestellt werden, können Webdienst-Eingabemodule einschließen. Dies bedeutet, dass die Eingabe in den Webdienst über den Webdienstaufruf in Form eines Verweises auf einen Blobspeicherort bereitgestellt wird. Es gibt auch die Möglichkeit, kein Webdienst-Eingabemodul zu verwenden und stattdessen ein **Import Data**-Modul zu nutzen. In diesem Fall liest das **Import Data**-Modul Daten aus einer Datenquelle wie einer SQL-Datenbank über eine Abfrage zur Laufzeit ein. Webdienst-Parameter können verwendet werden, um dynamisch auf andere Server oder Tabellen usw. zu verweisen. Das SDK unterstützt beide dieser Muster.

Im folgenden Codebeispiel wird veranschaulicht, wie Sie einen Batchauftrag für einen Azure Machine Learning-Dienst mithilfe des BES SDK senden und überwachen können. Die Kommentare enthalten Details zu den Einstellungen und Aufrufen.

#### <a name="sample-code"></a>**Beispielcode**

    // This code requires the Nuget package Microsoft.Azure.MachineLearning to be installed.
    // Instructions for doing this in Visual Studio:
    // Tools -> Nuget Package Manager -> Package Manager Console
    // Install-Package Microsoft.Azure.MachineLearning

      using System;
      using System.Collections.Generic;
      using System.Threading.Tasks;

      using Microsoft.Azure.MachineLearning;
      using Microsoft.Azure.MachineLearning.Contracts;
      using Microsoft.Azure.MachineLearning.Exceptions;

    namespace CallBatchExecutionService
    {
        class Program
        {
            static void Main(string[] args)
            {               
                InvokeBatchExecutionService().Wait();
            }

            static async Task InvokeBatchExecutionService()
            {
                // First collect and fill in the URI and access key for your Web service endpoint.
                // These are available on your service's API help page.
                var endpointUri = "https://ussouthcentral.services.azureml.net/workspaces/YOUR_WORKSPACE_ID/services/YOUR_SERVICE_ENDPOINT_ID/";
                string accessKey = "YOUR_SERVICE_ENDPOINT_ACCESS_KEY";

                // Create an Azure Machine Learning runtime client for this endpoint
                var runtimeClient = new RuntimeClient(endpointUri, accessKey);

                // Define the request information for your batch job. This information can contain:
                // -- A reference to the AzureBlob containing the input for your job run
                // -- A set of values for global parameters defined as part of your experiment and service
                // -- A set of output blob locations that allow you to redirect the job's results

                // NOTE: This sample is applicable, as is, for a service with explicit input port and
                // potential global parameters. Also, we choose to also demo how you could override the
                // location of one of the output blobs that could be generated by your service. You might
                // need to tweak these features to adjust the sample to your service.
                //
                // All of these properties of a BatchJobRequest shown below can be optional, depending on
                // your service, so it is not required to specify all with any request.  If you do not want to
                // use any of the parameters, a null value should be passed in its place.

                // Define the reference to the blob containing your input data. You can refer to this blob by its
                    // connection string / container / blob name values; alternatively, we also support references
                    // based on a blob SAS URI

                    BlobReference inputBlob = BlobReference.CreateFromConnectionStringData(connectionString:                                         "DefaultEndpointsProtocol=https;AccountName=YOUR_ACCOUNT_NAME;AccountKey=YOUR_ACCOUNT_KEY",
                        containerName: "YOUR_CONTAINER_NAME",
                        blobName: "YOUR_INPUT_BLOB_NAME");

                    // If desired, one can override the location where the job outputs are to be stored, by passing in
                    // the storage account details and name of the blob where we want the output to be redirected to.

                    var outputLocations = new Dictionary<string, BlobReference>
                        {
                          {
                           "YOUR_OUTPUT_NODE_NAME",
                           BlobReference.CreateFromConnectionStringData(                                     connectionString: "DefaultEndpointsProtocol=https;AccountName=YOUR_ACCOUNT_NAME;AccountKey=YOUR_ACCOUNT_KEY",
                                containerName: "YOUR_CONTAINER_NAME",
                                blobName: "YOUR_DESIRED_OUTPUT_BLOB_NAME")
                           }
                        };

                // If applicable, you can also set the global parameters for your service
                var globalParameters = new Dictionary<string, string>
                {
                    { "YOUR_GLOBAL_PARAMETER", "PARAMETER_VALUE" }
                };

                var jobRequest = new BatchJobRequest
                {
                    Input = inputBlob,
                    GlobalParameters = globalParameters,
                    Outputs = outputLocations
                };

                try
                {
                    // Register the batch job with the system, which will grant you access to a job object
                    BatchJob job = await runtimeClient.RegisterBatchJobAsync(jobRequest);

                    // Start the job to allow it to be scheduled in the running queue
                    await job.StartAsync();

                    // Wait for the job's completion and handle the output
                    BatchJobStatus jobStatus = await job.WaitForCompletionAsync();
                    if (jobStatus.JobState == JobState.Finished)
                    {
                        // Process job outputs
                        Console.WriteLine(@"Job {0} has completed successfully and returned {1} outputs", job.Id, jobStatus.Results.Count);
                        foreach (var output in jobStatus.Results)
                        {
                            Console.WriteLine(@"\t{0}: {1}", output.Key, output.Value.AbsoluteUri);
                        }
                    }
                    else if (jobStatus.JobState == JobState.Failed)
                    {
                        // Handle job failure
                        Console.WriteLine(@"Job {0} has failed with this error: {1}", job.Id, jobStatus.Details);
                    }
                }
                catch (ArgumentException aex)
                {
                    Console.WriteLine("Argument {0} is invalid: {1}", aex.ParamName, aex.Message);
                }
                catch (RuntimeException runtimeError)
                {
                    Console.WriteLine("Runtime error occurred: {0} - {1}", runtimeError.ErrorCode, runtimeError.Message);
                    Console.WriteLine("Error details:");
                    foreach (var errorDetails in runtimeError.Details)
                    {
                        Console.WriteLine("\t{0} - {1}", errorDetails.Code, errorDetails.Message);
                    }
                }
                catch (Exception ex)
                {
                    Console.WriteLine("Unexpected error occurred: {0} - {1}", ex.GetType().Name, ex.Message);
                }
            }
        }
    }

#### <a name="sample-code-in-java-for-bes"></a>Beispielcode in Java für BES

Die REST-API des Batchausführungsdiensst akzeptiert den JSON-Code, der aus einem Verweis auf eine CSV-Datei mit einem Eingabebeispiel und eine CSV-Datei mit einem Ausgabebeispiel besteht (siehe das folgende Beispiel), und erstellt in Azure ML einen Auftrag zur Ausführung der Batchvorhersagen. Sie finden den gesamten Code in [GitHub](https://github.com/nk773/AzureML_BESApp/tree/master/src/azureml_besapp). Dieses Java-Beispiel erfordert die [Apache-HTTP-Clientbibliothek](https://hc.apache.org/downloads.cgi). 


    { "GlobalParameters": {}, 
        "Inputs": { "input1": { "ConnectionString":     "DefaultEndpointsProtocol=https;
            AccountName=myAcctName; AccountKey=Q8kkieg==", 
            "RelativeLocation": "myContainer/sampleinput.csv" } }, 
        "Outputs": { "output1": { "ConnectionString":   "DefaultEndpointsProtocol=https;
            AccountName=myAcctName; AccountKey=kjC12xQ8kkieg==", 
            "RelativeLocation": "myContainer/sampleoutput.csv" } } 
    } 


##### <a name="create-a-bes-job"></a>Erstellen eines BES-Auftrags  
        
        /**
         * Call REST API to create a job to Azure ML 
         * for batch predictions
         * @return response from the REST API
         */ 
        public static String besCreateJob() {
            
            HttpPost post;
            HttpClient client;
            StringEntity entity;
            
            try {
                // create HttpPost and HttpClient object
                post = new HttpPost(apiurl);
                client = HttpClientBuilder.create().build();
                
                // setup output message by copying JSON body into 
                // apache StringEntity object along with content type
                entity = new StringEntity(jsonBody, HTTP.UTF_8);
                entity.setContentEncoding(HTTP.UTF_8);
                entity.setContentType("text/json");
    
                // add HTTP headers
                post.setHeader("Accept", "text/json");
                post.setHeader("Accept-Charset", "UTF-8");
            
                // set Authorization header based on the API key
                // note a space after the word "Bearer " - don't miss that
                post.setHeader("Authorization", ("Bearer "+apikey));
                post.setEntity(entity);
    
                // Call REST API and retrieve response content
                HttpResponse authResponse = client.execute(post);
                
                jobId = EntityUtils.toString(authResponse.getEntity()).replaceAll("\"", "");
                
                
                return jobId;
                
            }
            catch (Exception e) {
                
                return e.toString();
            }
        
        }
        
##### <a name="start-a-previously-created-bes-job"></a>Starten eines zuvor erstellten BES-Auftrags        
    
        /**
         * Call REST API for starting prediction job previously submitted 
         * 
         * @param job job to be started 
         * @return response from the REST API
         */ 
        public static String besStartJob(String job){
            HttpPost post;
            HttpClient client;
            StringEntity entity;
            
            try {
                // create HttpPost and HttpClient object
                post = new HttpPost(startJobUrl+"/"+job+"/start?api-version=2.0");
                client = HttpClientBuilder.create().build();
             
                // add HTTP headers
                post.setHeader("Accept", "text/json");
                post.setHeader("Accept-Charset", "UTF-8");
            
                // set Authorization header based on the API key
                post.setHeader("Authorization", ("Bearer "+apikey));
    
                // Call REST API and retrieve response content
                HttpResponse authResponse = client.execute(post);
                
                if (authResponse.getEntity()==null)
                {
                    return authResponse.getStatusLine().toString();
                }
                
                return EntityUtils.toString(authResponse.getEntity());
                
            }
            catch (Exception e) {
                
                return e.toString();
            }
        }
##### <a name="cancel-a-previously-created-bes-job"></a>Abbrechen eines zuvor erstellten BES-Auftrags
        
        /**
         * Call REST API for canceling the batch job 
         * 
         * @param job job to be started 
         * @return response from the REST API
         */ 
        public static String besCancelJob(String job) {
            HttpDelete post;
            HttpClient client;
            StringEntity entity;
            
            try {
                // create HttpPost and HttpClient object
                post = new HttpDelete(startJobUrl+job);
                client = HttpClientBuilder.create().build();
             
                // add HTTP headers
                post.setHeader("Accept", "text/json");
                post.setHeader("Accept-Charset", "UTF-8");
            
                // set Authorization header based on the API key
                post.setHeader("Authorization", ("Bearer "+apikey));
    
                // Call REST API and retrieve response content
                HttpResponse authResponse = client.execute(post);
             
                if (authResponse.getEntity()==null)
                {
                    return authResponse.getStatusLine().toString();
                }
                return EntityUtils.toString(authResponse.getEntity());
                
            }
            catch (Exception e) {
                
                return e.toString();
            }
        }
        
### <a name="other-programming-environments"></a>Andere Programmierumgebungen

Sie können den Code auch in vielen anderen Sprachen generieren, indem Sie die Anweisungen auf der Website [swagger.io](http://swagger.io/) befolgen. Für einen klassischen Webdienst erhalten Sie das Swagger-Dokument:

* Auf der API-Hilfeseite 
* Durch Aufrufen von „Get API Document for Endpoint“ auf der Seite „Swagger API“ im Machine Learning Web Services-Portal 

Wechseln Sie zu [swagger.io](http://swagger.io/swagger-codegen/), und befolgen Sie die Anweisungen zum Herunterladen von Swagger-Code, Java und Apache Maven. 

Es folgt eine Zusammenfassung der Anweisungen zum Einrichten von Swagger für andere Programmierumgebungen.

* Stellen Sie sicher, dass Java 7 oder höher installiert ist.
* Installieren Sie Apache Maven (unter Ubuntu können Sie *apt-get install mvn*verwenden).
* Rufen Sie Github für Swagger auf, und laden Sie das Swagger-Projekt als ZIP-Datei herunter
* Entzippen Sie Swagger
* Erstellen Sie die Swagger-Tools durch Ausführen von *mvn package* aus dem Swagger-Quellverzeichnis.

Jetzt können Sie beliebige Swagger-Tools verwenden. Hier sind die Anweisungen, um Java-Clientcode zu generieren. 

* Rufen Sie die Azure ML-API-Hilfeseite auf ( [hier](https://studio.azureml.net/apihelp/workspaces/afbd553b9bac4c95be3d040998943a4f/webservices/4dfadc62adcc485eb0cf162397fb5682/endpoints/26a3afce1767461ab6e73d5a206fbd62/jobs)finden Sie ein Beispiel).
* Suchen Sie die URL für „swagger.json“ für Azure ML-REST-APIs (zweitletztes Aufzählungszeichen oben auf der API-Hilfeseite)
* Klicken Sie auf den Link zum Swagger-Dokument ( [hier](https://management.azureml.net/workspaces/afbd553b9bac4c95be3d040998943a4f/webservices/4dfadc62adcc485eb0cf162397fb5682/endpoints/26a3afce1767461ab6e73d5a206fbd62/apidocument)finden Sie ein Beispiel).
* Verwenden Sie den folgenden Befehl, wie in der [Readme-Datei von Swagger](https://github.com/swagger-api/swagger-codegen/blob/master/README.md) gezeigt, um den Clientcode zu generieren.

**Beispielbefehlszeile zum Generieren von Clientcode**

    java -jar swagger-codegen-cli.jar generate\
     -i https://ussouthcentral.services.azureml.net:443/workspaces/\
    fb62b56f29fc4ba4b8a8f900c9b89584/services/26a3afce1767461ab6e73d5a206fbd62/swagger.json\
     -l java -o /home/username/sample

* Kombinieren Sie die Werte in den Feldern „host“, „basePath“ und „/swagger.json“ im unten gezeigten Beispiel einer Swagger-[API-Hilfeseite](https://management.azureml.net/workspaces/afbd553b9bac4c95be3d040998943a4f/webservices/4dfadc62adcc485eb0cf162397fb5682/endpoints/26a3afce1767461ab6e73d5a206fbd62/apidocument), um die in der vorherigen Befehlszeile verwendete Swagger-URL zu erstellen.

**Beispiel-API-Hilfeseite**


    {
      "swagger": "2.0",
      "info": {
        "version": "2.0",
        "title": "Sample 5: Binary Classification with Web service: Adult Dataset [Predictive Exp.]",
        "description": "No description provided for this Web service.",
        "x-endpoint-name": "default"
      },
      "host": "ussouthcentral.services.azureml.net:443",
      "basePath": "/workspaces/afbd553b9bac4c95be3d040998943a4f/services/26a3afce1767461ab6e73d5a206fbd62",
      "schemes": [
        "https"
      ],
      "consumes": [
        "application/json"
      ],
      "produces": [
        "application/json"
      ],
      "paths": {
        "/swagger.json": {
          "get": {
            "summary": "Get swagger API document for the Web service",
            "operationId": "getSwaggerDocument",
            
<!-- Relative Links -->

[Veröffentlichen]: machine-learning-publish-a-machine-learning-web-service.md
[Exemplarische Vorgehensweise]: machine-learning-walkthrough-develop-predictive-solution.md

<!-- External Links -->
[Web Services-Portal]: https://services.azureml.net/
[ML Studio]: https://studio.azureml.net


<!---HONumber=Oct16_HO2-->


