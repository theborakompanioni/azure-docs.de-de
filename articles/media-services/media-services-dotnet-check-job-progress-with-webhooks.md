---
title: "Verwenden von Azure-Webhooks zum Überwachen von Media Services-Auftragsbenachrichtigungen mit .NET | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie Media Services-Auftragsbenachrichtigungen mithilfe von Azure-Webhooks überwachen. Das Codebeispiel ist in C# geschrieben und verwendet das Media Services SDK für .NET."
services: media-services
documentationcenter: 
author: juliako
manager: erikre
editor: 
ms.assetid: a61fe157-81b1-45c1-89f2-224b7ef55869
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/06/2017
ms.author: juliako
translationtype: Human Translation
ms.sourcegitcommit: 094729399070a64abc1aa05a9f585a0782142cbf
ms.openlocfilehash: c0cf8a3d4e257f88f81fca9a6a1161c158b335b8
ms.lasthandoff: 03/07/2017


---
# <a name="use-azure-webhooks-to-monitor-media-services-job-notifications-with-net"></a>Verwenden von Azure-Webhooks zum Überwachen von Media Services-Auftragsbenachrichtigungen mit .NET
Beim Ausführen von Aufträgen ist es nützlich, deren Fortschritt verfolgen zu können. Sie können Media Services-Auftragsbenachrichtigungen mit Azure-Webhooks oder [Azure Queue Storage](media-services-dotnet-check-job-progress-with-queues.md) überwachen. In diesem Thema wird die Verwendung von Webhooks veranschaulicht.

## <a name="prerequisites"></a>Voraussetzungen

Zum Abschließen dieses Lernprogramms müssen folgende Voraussetzungen erfüllt sein:

* Ein Azure-Konto. Ausführliche Informationen finden Sie unter [Einen Monat kostenlos testen](https://azure.microsoft.com/pricing/free-trial/).
* Media Services-Konto. Informationen zum Erstellen eines Media Services-Kontos finden Sie unter [Gewusst wie: Erstellen eines Media Services Kontos](media-services-portal-create-account.md).
* .NET Framework 4.0 oder höher.
* Visual Studio.
* Erfahrung beim [Verwenden von Azure Functions](../azure-functions/functions-overview.md). Lesen Sie auch [HTTP- und Webhookbindungen in Azure Functions](../azure-functions/functions-bindings-http-webhook.md).

Dieses Thema beschreibt Folgendes

*  Definieren einer Azure-Funktion, die zum Reagieren auf Webhooks angepasst wird. 
    
    In diesem Fall wird der Webhook von Media Services ausgelöst, wenn sich bei Ihrem Verschlüsselungsauftrag der Status ändert. Die Funktion lauscht auf den Webhookrückruf von Media Services-Benachrichtigungen und veröffentlicht das Ausgabemedienobjekt, sobald der Auftrag beendet wurde. 
    
    >[!NOTE]
    >Bevor Sie fortfahren, stellen Sie sicher, dass Sie die Funktionsweise von [HTTP- und Webhookbindungen in Azure Functions](../azure-functions/functions-bindings-http-webhook.md) verstehen.
    >
    
* Hinzufügen eines Webhooks zu Ihrer Verschlüsselungsaufgabe und Angeben der Webhook-URL und des geheimen Schlüssels, auf die dieser Webhook reagiert. Im hier gezeigten Beispiel ist der Code, mit dem die Verschlüsselungsaufgabe erstellt wird, eine Konsolen-App.

## <a name="setting-up-webhook-notification-azure-functions"></a>Einrichten von Azure-Funktionen für „Webhook-Benachrichtigung“

Der Code in diesem Abschnitt zeigt eine Implementierung einer Azure-Funktion, die ein Webhook ist. In diesem Beispiel lauscht die Funktion auf den Webhookrückruf von Media Services-Benachrichtigungen und veröffentlicht das Ausgabemedienobjekt, sobald der Auftrag beendet wurde.

Der Webhook erwartet einen Signaturschlüssel (Anmeldeinformationen), der mit dem übereinstimmt, den Sie beim Konfigurieren des Benachrichtigungsendpunkts übergeben haben. Der Signaturschlüssel ist der Base64-codierte 64-Byte-Wert, der zum Schützen und Sichern Ihrer Webhookrückrufe von Azure Media Services verwendet wird. 

Im folgenden Code führt die **VerifyWebHookRequestSignature**-Methode die Überprüfung auf die Benachrichtigungsmeldung durch. Mit dieser Überprüfung soll sichergestellt werden, dass die Meldung von Azure Media Services gesendet und nicht manipuliert wurde. Die Signatur ist für Azure-Funktionen optional, da der **Code**-Wert als Abfrageparameter für Transport Layer Security (TLS) vorhanden ist. 

Sie finden die Definition der folgenden .NET-Azure-Funktion für Media Services [hier](https://github.com/Azure-Samples/media-services-dotnet-functions-integration/tree/master/Notification_Webhook_Function).

Das folgende Codebeispiel veranschaulicht die Definitionen von Azure-Funktionsparametern und drei Dateien, die mit der Azure-Funktion verknüpft sind: function.json, project.json und run.csx.

### <a name="application-settings"></a>Anwendungseinstellungen 

In der folgenden Tabelle werden die Parameter aufgeführt, die von der in diesem Abschnitt definierten Azure-Funktion verwendet werden. 

|Name|Definition|Beispiel| 
|---|---|---|
|AMSAccount|Ihr AMS-Kontoname |juliakomediaservices|
|AMSKey |Ihr AMS-Kontoschlüssel | JUWJdDaOHQQqsZeiXZuE76eDt2SO+YMJk25Lghgy2nY=|
|MediaServicesStorageAccountName |Ein Name des Speicherkontos, das mit Ihrem AMS-Konto verknüpft ist.| storagepkeewmg5c3peq|
|MediaServicesStorageAccountKey |Ein Schlüssel des Speicherkontos, das mit Ihrem AMS-Konto verknüpft ist.|
|SigningKey |Ein Signaturschlüssel| j0txf1f8msjytzvpe40nxbpxdcxtqcgxy0nt|
|WebHookEndpoint | Eine Webhook-Endpunktadresse | https://juliakofuncapp.azurewebsites.net/api/Notification_Webhook_Function?code=iN2phdrTnCxmvaKExFWOTulfnm4C71mMLIy8tzLr7Zvf6Z22HHIK5g==.|

### <a name="functionjson"></a>function.json

Die Datei „function.json“ definiert die Funktionsbindungen und weitere Konfigurationseinstellungen. Die Laufzeit verwendet diese Datei, um zu ermitteln, welche Ereignisse überwacht werden sollen und wie Daten in die Funktionsausführung übergeben und aus dieser zurückgegeben werden. 

    {
      "bindings": [
        {
          "type": "httpTrigger",
          "name": "req",
          "direction": "in",
          "methods": [
        "post",
        "get",
        "put",
        "update",
        "patch"
          ]
        },
        {
          "type": "http",
          "name": "res",
          "direction": "out"
        }
      ]
    }
    
### <a name="projectjson"></a>project.json

Die Datei „project.json“ enthält die Abhängigkeiten. 

    {
      "frameworks": {
        "net46":{
          "dependencies": {
        "windowsazure.mediaservices": "3.8.0.5",
        "windowsazure.mediaservices.extensions": "3.8.0.3"
          }
        }
       }
    }
    
### <a name="runcsx"></a>run.csx

Der folgende C#-Code zeigt eine Definition einer Azure-Funktion, die ein Webhook ist. Die Funktion lauscht auf den Webhookrückruf von Media Services-Benachrichtigungen und veröffentlicht das Ausgabemedienobjekt, sobald der Auftrag beendet wurde. 

    ///////////////////////////////////////////////////
    #r "Newtonsoft.Json"

    using System;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using System.IO;
    using System.Globalization;
    using Newtonsoft.Json;
    using Microsoft.Azure;
    using System.Net;
    using System.Security.Cryptography;

    internal const string SignatureHeaderKey = "sha256";
    internal const string SignatureHeaderValueTemplate = SignatureHeaderKey + "={0}";
    static string _webHookEndpoint = Environment.GetEnvironmentVariable("WebHookEndpoint");
    static string _signingKey = Environment.GetEnvironmentVariable("SigningKey");
    static string _mediaServicesAccountName = Environment.GetEnvironmentVariable("AMSAccount");
    static string _mediaServicesAccountKey = Environment.GetEnvironmentVariable("AMSKey");

    static CloudMediaContext _context = null;

    public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
    {
        log.Info($"C# HTTP trigger function processed a request. RequestUri={req.RequestUri}");

        Task<byte[]> taskForRequestBody = req.Content.ReadAsByteArrayAsync();
        byte[] requestBody = await taskForRequestBody;

        string jsonContent = await req.Content.ReadAsStringAsync();
        log.Info($"Request Body = {jsonContent}");

        IEnumerable<string> values = null;
        if (req.Headers.TryGetValues("ms-signature", out values))
        {
        byte[] signingKey = Convert.FromBase64String(_signingKey);
        string signatureFromHeader = values.FirstOrDefault();

        if (VerifyWebHookRequestSignature(requestBody, signatureFromHeader, signingKey))
        {
            string requestMessageContents = Encoding.UTF8.GetString(requestBody);

            NotificationMessage msg = JsonConvert.DeserializeObject<NotificationMessage>(requestMessageContents);

            if (VerifyHeaders(req, msg, log))
            { 
            string newJobStateStr = (string)msg.Properties.Where(j => j.Key == "NewState").FirstOrDefault().Value;
            if (newJobStateStr == "Finished")
            {
                _context = new CloudMediaContext(new MediaServicesCredentials(
                _mediaServicesAccountName,
                _mediaServicesAccountKey));

                if(_context!=null)   
                {                        
                string urlForClientStreaming = PublishAndBuildStreamingURLs(msg.Properties["JobId"]);
                log.Info($"URL to the manifest for client streaming using HLS protocol: {urlForClientStreaming}");
                }
            }

            return req.CreateResponse(HttpStatusCode.OK, string.Empty);
            }
            else
            {
            log.Info($"VerifyHeaders failed.");
            return req.CreateResponse(HttpStatusCode.BadRequest, "VerifyHeaders failed.");
            }
        }
        else
        {
            log.Info($"VerifyWebHookRequestSignature failed.");
            return req.CreateResponse(HttpStatusCode.BadRequest, "VerifyWebHookRequestSignature failed.");
        }
        }

        return req.CreateResponse(HttpStatusCode.BadRequest, "Generic Error.");
    }

    private static string PublishAndBuildStreamingURLs(String jobID)
    {
        IJob job = _context.Jobs.Where(j => j.Id == jobID).FirstOrDefault();
        IAsset asset = job.OutputMediaAssets.FirstOrDefault();

        // Create a 30-day readonly access policy. 
        // You cannot create a streaming locator using an AccessPolicy that includes write or delete permissions.
        IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
        TimeSpan.FromDays(30),
        AccessPermissions.Read);

        // Create a locator to the streaming content on an origin. 
        ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
        policy,
        DateTime.UtcNow.AddMinutes(-5));


        // Get a reference to the streaming manifest file from the  
        // collection of files in the asset. 
        var manifestFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                    EndsWith(".ism")).
                    FirstOrDefault();

        // Create a full URL to the manifest file. Use this for playback
        // in streaming media clients. 
        string urlForClientStreaming = originLocator.Path + manifestFile.Name + "/manifest" +  "(format=m3u8-aapl)";
        return urlForClientStreaming;

    }

    private static bool VerifyWebHookRequestSignature(byte[] data, string actualValue, byte[] verificationKey)
    {
        using (var hasher = new HMACSHA256(verificationKey))
        {
        byte[] sha256 = hasher.ComputeHash(data);
        string expectedValue = string.Format(CultureInfo.InvariantCulture, SignatureHeaderValueTemplate, ToHex(sha256));

        return (0 == String.Compare(actualValue, expectedValue, System.StringComparison.Ordinal));
        }
    }

    private static bool VerifyHeaders(HttpRequestMessage req, NotificationMessage msg, TraceWriter log)
    {
        bool headersVerified = false;

        try
        {
        IEnumerable<string> values = null;
        if (req.Headers.TryGetValues("ms-mediaservices-accountid", out values))
        {
            string accountIdHeader = values.FirstOrDefault();
            string accountIdFromMessage = msg.Properties["AccountId"];

            if (0 == string.Compare(accountIdHeader, accountIdFromMessage, StringComparison.OrdinalIgnoreCase))
            {
            headersVerified = true;
            }
            else
            {
            log.Info($"accountIdHeader={accountIdHeader} does not match accountIdFromMessage={accountIdFromMessage}");
            }
        }
        else
        {
            log.Info($"Header ms-mediaservices-accountid not found.");
        }
        }
        catch (Exception e)
        {
        log.Info($"VerifyHeaders hit exception {e}");
        headersVerified = false;
        }

        return headersVerified;
    }

    private static readonly char[] HexLookup = new char[] { '0', '1', '2', '3', '4', '5', '6', '7', '8', '9', 'A', 'B', 'C', 'D', 'E', 'F' };

    /// <summary>
    /// Converts a <see cref="T:byte[]"/> to a hex-encoded string.
    /// </summary>
    private static string ToHex(byte[] data)
    {
        if (data == null)
        {
        return string.Empty;
        }

        char[] content = new char[data.Length * 2];
        int output = 0;
        byte d;
        for (int input = 0; input < data.Length; input++)
        {
        d = data[input];
        content[output++] = HexLookup[d / 0x10];
        content[output++] = HexLookup[d % 0x10];
        }
        return new string(content);
    }

    internal enum NotificationEventType
    {
        None = 0,
        JobStateChange = 1,
        NotificationEndPointRegistration = 2,
        NotificationEndPointUnregistration = 3,
        TaskStateChange = 4,
        TaskProgress = 5
    }
    
    internal sealed class NotificationMessage
    {
        public string MessageVersion { get; set; }
        public string ETag { get; set; }
        public NotificationEventType EventType { get; set; }
        public DateTime TimeStamp { get; set; }
        public IDictionary<string, string> Properties { get; set; }
    }

### <a name="function-output"></a>Funktionsausgabe

Das obige Beispiel erzeugt die folgende Ausgabe. Ihre Werte können davon abweichen.

    C# HTTP trigger function processed a request. RequestUri=https://juliako001-functions.azurewebsites.net/api/Notification_Webhook_Function?code=9376d69kygoy49oft81nel8frty5cme8hb9xsjslxjhalwhfrqd79awz8ic4ieku74dvkdfgvi
    Request Body = {
      "MessageVersion": "1.1",
      "ETag": "b8977308f48858a8f224708bc963e1a09ff917ce730316b4e7ae9137f78f3b20",
      "EventType": 4,
      "TimeStamp": "2017-02-16T03:59:53.3041122Z",
      "Properties": {
        "JobId": "nb:jid:UUID:badd996c-8d7c-4ae0-9bc1-bd7f1902dbdd",
        "TaskId": "nb:tid:UUID:80e26fb9-ee04-4739-abd8-2555dc24639f",
        "NewState": "Finished",
        "OldState": "Processing",
        "AccountName": "mediapkeewmg5c3peq",
        "AccountId": "301912b0-659e-47e0-9bc4-6973f2be3424",
        "NotificationEndPointId": "nb:nepid:UUID:cb5d707b-4db8-45fe-a558-19f8d3306093"
      }
    }
    
    URL to the manifest for client streaming using HLS protocol: http://mediapkeewmg5c3peq.streaming.mediaservices.windows.net/0ac98077-2b58-4db7-a8da-789a13ac6167/BigBuckBunny.ism/manifest(format=m3u8-aapl)

## <a name="adding-webhook-to-your-encoding-task"></a>Hinzufügen eines Webhooks zu Ihrer Verschlüsselungsaufgabe

In diesem Abschnitt wird der Code gezeigt, der eine Webhookbenachrichtigung zu einer Aufgabe hinzufügt. Sie können auch eine Benachrichtigung auf Auftragsebene hinzufügen. Dies wäre für einen Auftrag mit verketteten Aufgaben hilfreicher.  

1. Erstellen Sie eine neue C#-Konsolenanwendung in Visual Studio. Geben Sie Name, Ort und Lösungsname ein und klicken Sie auf OK.
2. Verwenden Sie [NuGet](https://www.nuget.org/packages/windowsazure.mediaservices), um Azure Media Services zu installieren.
3. Aktualisieren Sie Datei „App.config“ mit den geeigneten Werte: 
    
    * Azure Media Services-Name und -Schlüssel zum Senden von Benachrichtigungen 
    * Webhook-URL, die die Benachrichtigungen erwartet 
    * Signaturschlüssel, der mit dem Schlüssel übereinstimmt, den Ihr Webhook erwartet Der Signaturschlüssel ist der Base64-codierte 64-Byte-Wert, der zum Schützen und Sichern Ihrer Webhookrückrufe von Azure Media Services verwendet wird. 

            <appSettings>
              <add key="MediaServicesAccountName" value="AMSAcctName" />
              <add key="MediaServicesAccountKey" value="AMSAcctKey" />
              <add key="WebhookURL" value="https://<yourapp>.azurewebsites.net/api/<function>?code=<ApiKey>" />
              <add key="WebhookSigningKey" value="j0txf1f8msjytzvpe40nxbpxdcxtqcgxy0nt" />
            </appSettings>
            
4. Aktualisieren Sie die Datei „Program.cs“ mit dem folgenden Code:

        using System;
        using System.Configuration;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;

        namespace NotificationWebHook
        {
            class Program
            {
            // Read values from the App.config file.
            private static readonly string _mediaServicesAccountName =
                ConfigurationManager.AppSettings["MediaServicesAccountName"];
            private static readonly string _mediaServicesAccountKey =
                ConfigurationManager.AppSettings["MediaServicesAccountKey"];
            private static readonly string _webHookEndpoint =
                ConfigurationManager.AppSettings["WebhookURL"];
            private static readonly string _signingKey =
                 ConfigurationManager.AppSettings["WebhookSigningKey"];

            // Field for service context.
            private static CloudMediaContext _context = null;

            static void Main(string[] args)
            {

                // Used the cached credentials to create CloudMediaContext.
                _context = new CloudMediaContext(new MediaServicesCredentials(
                        _mediaServicesAccountName,
                        _mediaServicesAccountKey));

                byte[] keyBytes = Convert.FromBase64String(_signingKey);

                IAsset newAsset = _context.Assets.FirstOrDefault();

                // Check for existing Notification Endpoint with the name "FunctionWebHook"

                var existingEndpoint = _context.NotificationEndPoints.Where(e => e.Name == "FunctionWebHook").FirstOrDefault();
                INotificationEndPoint endpoint = null;

                if (existingEndpoint != null)
                {
                Console.WriteLine("webhook endpoint already exists");
                endpoint = (INotificationEndPoint)existingEndpoint;
                }
                else
                {
                endpoint = _context.NotificationEndPoints.Create("FunctionWebHook",
                    NotificationEndPointType.WebHook, _webHookEndpoint, keyBytes);
                Console.WriteLine("Notification Endpoint Created with Key : {0}", keyBytes.ToString());
                }

                // Declare a new encoding job with the Standard encoder
                IJob job = _context.Jobs.Create("MES Job");

                // Get a media processor reference, and pass to it the name of the 
                // processor to use for the specific task.
                IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

                ITask task = job.Tasks.AddNew("My encoding task",
                processor,
                "Adaptive Streaming",
                TaskOptions.None);

                // Specify the input asset to be encoded.
                task.InputAssets.Add(newAsset);

                // Add an output asset to contain the results of the job. 
                // This output is specified as AssetCreationOptions.None, which 
                // means the output asset is not encrypted. 
                task.OutputAssets.AddNew(newAsset.Name, AssetCreationOptions.None);

                // Add the WebHook notification to this Task and request all notification state changes.
                // Note that you can also add a job level notification
                // which would be more useful for a job with chained tasks.  
                if (endpoint != null)
                {
                task.TaskNotificationSubscriptions.AddNew(NotificationJobState.All, endpoint, true);
                Console.WriteLine("Created Notification Subscription for endpoint: {0}", _webHookEndpoint);
                }
                else
                {
                Console.WriteLine("No Notification Endpoint is being used");
                }

                job.Submit();

                Console.WriteLine("Expect WebHook to be triggered for the Job ID: {0}", job.Id);
                Console.WriteLine("Expect WebHook to be triggered for the Task ID: {0}", task.Id);

                Console.WriteLine("Job Submitted");

            }
            private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
            {
                var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
                ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();

                if (processor == null)
                throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));

                return processor;
            }

            }
        }

## <a name="next-step"></a>Nächster Schritt
Media Services-Lernpfade ansehen

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geben
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

