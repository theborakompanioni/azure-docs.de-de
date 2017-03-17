---
title: "Verwenden der Paketerfassung für die proaktive Netzwerküberwachung mit Azure Functions | Microsoft-Dokumentation"
description: "In diesem Artikel wird beschrieben, wie Sie eine per Warnung ausgelöste Paketerfassung mit Azure Network Watcher erstellen."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 75e6e7c4-b3ba-4173-8815-b00d7d824e11
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 6c7a0402a913c836f3248fa6f2f9b27cbf2b0d04
ms.openlocfilehash: a23d569451d2cc776b2e8fc84ec4d01259f74c63
ms.lasthandoff: 02/23/2017


---
# <a name="use-packet-capture-to-do-proactive-network-monitoring-with-azure-functions"></a>Verwenden der Paketerfassung für die proaktive Netzwerküberwachung mit Azure Functions

Mit der Network Watcher-Paketerfassung werden Erfassungssitzungen zum Nachverfolgen des eingehenden und ausgehenden Datenverkehrs eines virtuellen Computers erstellt. Die Erfassungsdatei kann über einen Filter verfügen, der so definiert ist, dass nur der Datenverkehr nachverfolgt wird, den Sie überwachen möchten. Diese Daten werden dann in einem Speicherblob oder lokal auf dem Gastcomputer gespeichert. Diese Funktion kann per Remotezugriff über Automationsszenarien wie Azure Functions gestartet werden. Mit der Paketerfassung können Sie proaktive Erfassungen basierend auf definierten Netzwerkanomalien durchführen. Weitere Verwendungszwecke sind das Erfassen von Netzwerkstatistiken, das Gewinnen von Informationen zu Netzwerkangriffen, das Debuggen der Kommunikation zwischen Client und Server und vieles mehr.

In Azure bereitgestellte Ressourcen werden rund um die Uhr ausgeführt. Für Sie bzw. Ihre Mitarbeiter ist es nicht möglich, den Status aller Ressourcen rund um die Uhr lückenlos aktiv zu überwachen. Was passiert, wenn nachts um 2 Uhr ein Problem auftritt?

Durch den Einsatz von Network Watcher, Warnungen und Functions im Azure-Ökosystem können Sie mit den entsprechenden Daten und Tools proaktiv auf Probleme im Netzwerk reagieren, um diese zu lösen.

## <a name="before-you-begin"></a>Voraussetzungen

In diesem Beispiel werden von Ihrer VM ungewöhnlich viele TCP-Segmente gesendet, und Sie möchten dazu eine Warnung erhalten. Als Beispiel werden TCP-Segmente verwendet, aber Sie können eine beliebige Warnungsbedingung nutzen. Bei einer Warnung möchten Sie über Paketebenendaten verfügen, um zu verstehen, warum es zu einem Anstieg der Kommunikation gekommen ist. Sie können dann Maßnahmen einleiten, um die reguläre Kommunikation des Computers wiederherzustellen.
Bei diesem Szenario wird angenommen, dass Sie über eine vorhandene Instanz von Network Watcher und eine Ressourcengruppe mit einem gültigen virtuellen Computer verfügen, der genutzt werden kann.

[!INCLUDE [network-watcher-preview](../../includes/network-watcher-public-preview-notice.md)]

## <a name="scenario"></a>Szenario

Zur Automatisierung dieses Prozesses erstellen und verbinden wir eine Warnung auf unserer VM, die ausgelöst wird, wenn der Vorfall eintritt, sowie eine Azure-Funktion für den Aufruf von Network Watcher.

Dieses Szenario umfasst Folgendes:

* Erstellen einer Azure-Funktion zum Starten einer Paketerfassung
* Erstellen einer Warnungsregel auf einem virtuellen Computer
* Konfigurieren der Warnungsregel zum Aufrufen der Azure-Funktion

## <a name="creating-an-azure-function-and-overview"></a>Erstellen einer Azure-Funktion und Übersicht

Im ersten Schritt wird eine Azure-Funktion zum Verarbeiten der Warnung und Erstellen einer Paketerfassung erstellt. 

Die folgende Liste enthält eine Übersicht über den Workflow.

1. Auf Ihrer VM wird eine Warnung ausgelöst.
1. Für die Warnung wird Ihre Azure-Funktion per Webhook aufgerufen.
1. Ihre Azure-Funktion verarbeitet die Warnung und startet eine Network Watcher-Paketerfassungssitzung.
1. Die Paketerfassung wird auf dem virtuellen Computer ausgeführt, und der Datenverkehr wird erfasst. 
1. Die Erfassungsdatei wird für die Überprüfung und Diagnose in ein Speicherkonto hochgeladen. 

Die Erstellung einer Azure-Funktion ist im Portal möglich. Informationen hierzu finden Sie unter [Erstellen Sie Ihre erste Funktion in Azure Functions](../azure-functions/functions-create-first-azure-function.md). Für dieses Beispiel haben wir eine Funktion vom Typ „HttpTrigger-CSharp“ gewählt. 

> [!NOTE]
> Für Azure-Funktionen werden auch andere Sprachen bereitgestellt, aber dies geschieht unter Umständen nur zu Testzwecken und ohne vollständige Unterstützung, z.B. PowerShell und Python.

## <a name="processing-the-alert-and-starting-a-packet-capture-session"></a>Verarbeiten der Warnung und Starten einer Paketerfassungssitzung

Nun können Sie über die Azure-Funktion Aufrufe an Network Watcher senden. Die Implementierung dieser Funktion unterscheidet sich je nach den geltenden Anforderungen. Für den allgemeinen Datenfluss des Codes gilt aber folgender Ablauf:

1. Verarbeiten von Eingabeparametern
2. Abfragen von vorhandenen Paketerfassungen, um Grenzwerte zu überprüfen und Namenskonflikte zu lösen
3. Erstellen einer Paketerfassung mit den geeigneten Parametern
4. Regelmäßiges Abfragen der Paketerfassung bis zum Abschluss des Vorgangs
5. Benachrichtigen des Benutzers über den Abschluss der Paketerfassungssitzung

Hier ist ein C#-Beispiel angegeben, das in der Azure-Funktion verwendet werden kann. Darin sind Werte für Abonnement, Client-ID, Mandanten-ID und Clientgeheimnis enthalten, die ersetzt werden müssen.

```CSharp
using System.Net;
using Newtonsoft;
using Newtonsoft.Json;
using Microsoft.Rest.Azure.Authentication;
using Microsoft.Azure.Management.ResourceManager;
using Microsoft.Azure.Management.ResourceManager.Models;
using Microsoft.IdentityModel.Clients.ActiveDirectory;

public static TraceWriter log;
public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter twlog)
{
    log = twlog;
    log.Info($"C# HTTP trigger function processed a request. RequestUri={req.RequestUri}");

    HttpContent requestContent = req.Content;
    string jsonContent = requestContent.ReadAsStringAsync().Result;
    
    //Deserialize json input
    WebhookInputParameters inParams = JsonConvert.DeserializeObject<WebhookInputParameters>(jsonContent);

    log.Info($@"subscriptionId: {inParams.subscriptionId}  
                networkWatcherResourceGroup: {inParams.networkWatcherResourceGroup} 
                networkWatcherName: {inParams.networkWatcherName} 
                packetCaptureName: {inParams.packetCaptureName} 
                storageID: {inParams.storageID} 
                timeLimit: {inParams.timeLimit} 
                targetVM: {inParams.targetVM}");

    //Get JWT Token
    string token = GetAuthorizationToken();
    
    //Create URI and Delete existing Packet Capture if it exists
    string endpoint = @"https://management.azure.com";
    string PacketCaptureRequestURI = $@"{endpoint}/subscriptions/{inParams.subscriptionId}/resourceGroups/{inParams.networkWatcherResourceGroup}/providers/Microsoft.Network/networkWatchers/{inParams.networkWatcherName}/packetCaptures/{inParams.packetCaptureName}?api-version=2016-03-30";

    //Delete Packet Capture
    HttpWebRequest packetCaptureDelete = Request(PacketCaptureRequestURI, token, "Delete");
    //Errors from delete (404 not found) indicate that the packet capture does not exist already
    HttpWebResponse pcDeleteResponse = getHttpResponse(packetCaptureDelete);

    //Create Packet Capture Request Body
    PacketCaptureRequestBody pcrb = new PacketCaptureRequestBody();
    pcrb.properties.timeLimitInSeconds = inParams.timeLimit;
    pcrb.properties.target = inParams.targetVM;
    pcrb.properties.storageLocation.storageId = inParams.storageID;

    //serialize PacketCaptureRequestBody
    var jSetting = new JsonSerializerSettings();
    jSetting.NullValueHandling = NullValueHandling.Ignore;
    jSetting.Formatting = Formatting.Indented;
    string pcRequestBody = JsonConvert.SerializeObject(pcrb, jSetting);
    
    //Create Packet Capture via PUT 
    HttpWebRequest packetCaptureCreate = Request(PacketCaptureRequestURI, token, "Put", pcRequestBody);
    HttpWebResponse pcPostResponse = getHttpResponse(packetCaptureCreate);
    
    //if packet capture creation successful return to user created storagePath
    if (!(pcPostResponse == null) && pcPostResponse.StatusCode == HttpStatusCode.Created)
    {
        string pcPostResponseLocation = getHeaderContent(pcPostResponse, "Location") ?? "Header null";
        string pcPostResponseContent = httpWebResponseContent(pcPostResponse) ?? "Response Content null";

        //Deserialize response body into object
        PacketCaptureResponseBody pcrbObj = JsonConvert.DeserializeObject<PacketCaptureResponseBody>(pcPostResponseContent);
        return req.CreateResponse(HttpStatusCode.OK, $"Packet Capture successfully created and will upload to StoragePath: {pcrbObj.properties.storageLocation.storagePath}");
    }
    return req.CreateResponse(HttpStatusCode.OK, "Error creating packet capture");
}

// Creates and returns the HTTPWebRequest
public static HttpWebRequest Request(string requestURI, string token, string requestType, string requestContent = null)
{
    
    var httpWebRequest = (HttpWebRequest)WebRequest.Create(requestURI);
    httpWebRequest.Headers.Add(HttpRequestHeader.Authorization, "Bearer " + token);
    httpWebRequest.ContentType = "application/json";
    httpWebRequest.Method = requestType;
    if (requestContent != null)
    {
        httpWebRequest.ContentLength = (requestContent != null) ? requestContent.Length : 0;
    }
    log.Info("Sending Request: " + requestURI + (requestContent ?? ""));

    //ensure requestType is in uppercase
    requestType = requestType.ToUpper();
    if (requestType.Equals("PUT") || requestType.Equals("POST"))
    {
        using (var requestStream = new StreamWriter(httpWebRequest.GetRequestStream()))
        {
            requestStream.Write(requestContent);
        }
    }
    log.Info("Request Sent");
    return httpWebRequest;
}

public static HttpWebResponse getHttpResponse(HttpWebRequest httpWebRequest)
{
    HttpWebResponse httpWebResponse = null;
    try
    {
        httpWebResponse = (HttpWebResponse)httpWebRequest.GetResponse();
    }
    catch (Exception ex)
    {
        log.Info("Error from " + ex.Message, "HttpWebResponsexeption");
    }
    return httpWebResponse;
}

public static string httpWebResponseContent(HttpWebResponse httpWebResponse)
{
    string content = null;
    using (StreamReader reader = new StreamReader(httpWebResponse.GetResponseStream()))
    {
        content = reader.ReadToEnd();
    }
    return content;
}

public static string getHeaderContent(HttpWebResponse httpWebResponse, string param)
{
    log.Info((int)httpWebResponse.StatusCode + "\tStatus Description: " + httpWebResponse.StatusDescription + "\r\n Headers: " + httpWebResponse.Headers);
    return httpWebResponse.Headers.Get(param);
}

//Gets the JWT Token to be able to make REST calls
public static string GetAuthorizationToken()
{
    var tenantId = "<insert tenant id>"; 
    var clientId = "<insert client id>"; 
    var secret = "<insert client secret>";
    var subscriptionId = "<insert subscription id>"; 

    string authContextURL = "https://login.windows.net/" + tenantId;
    var authenticationContext = new AuthenticationContext(authContextURL);
    var credential = new ClientCredential(clientId, secret);
    var result = authenticationContext.AcquireToken(resource: "https://management.azure.com/", clientCredential: credential);
    if (result == null)
    {
        throw new InvalidOperationException("Failed to obtain the JWT token");
    }
    return result.AccessToken;
}

public class WebhookInputParameters
{
    public string subscriptionId { get; set; }
    public string networkWatcherResourceGroup { get; set; }
    public string networkWatcherName { get; set; }
    public string packetCaptureName { get; set; }
    public string storageID { get; set; }
    public int timeLimit { get; set; }
    public string targetVM { get; set; }
}

public class PacketCaptureRequestBody
{
    public PacketCaptureRequestBody()
    {
        properties = new Properties();
    }
    public Properties properties { get; set; }
}

public class StorageLocation
{
    public string storageId { get; set; }
    public string storagePath { get; set; }
    public string filePath { get; set; }
}

public class Filter
{
    public string protocol { get; set; }
    public string localIP { get; set; }
    public string localPort { get; set; }
    public string remoteIP { get; set; }
    public string remotePort { get; set; }
}

public class Properties
{
    public Properties()
    {
        filters = new List<Filter>();
        storageLocation = new StorageLocation();
    }

    public string target { get; set; }
    public int bytestToCapturePerPacket { get; set; }
    public int totalBytesPerSession { get; set; }
    public int timeLimitInSeconds { get; set; }

    public StorageLocation storageLocation { get; set; }
    public List<Filter> filters { get; set; }
}

public class PacketCaptureResponseBody
{
    public PacketCaptureResponseBody()
    {
        properties = new PacketCaptureResponseProperties();
    }
    public string name { get; set; }
    public string id { get; set; }
    public string etag { get; set; }
    public PacketCaptureResponseProperties properties { get; set; }
}

public class PacketCaptureResponseProperties
{
    public PacketCaptureResponseProperties()
    {
        filters = new List<Filter>();
        storageLocation = new StorageLocation();
    }
    public string provisioningState { get; set; }
    public string target { get; set; }
    public int bytesToCapturePerPacket { get; set; }
    public int totalBytesPerSession { get; set; }
    public int timeLimitInSeconds { get; set; }
    public StorageLocation storageLocation { get; set; }
    public List<Filter> filters { get; set; }
    public string captureStartTime { get; set; }
    public string packetCaptureStatus { get; set; }
    public List<object> packetCaptureError { get; set; }
}
``` 

Nachdem Sie die Funktion erstellt haben, müssen Sie Ihre Warnung konfigurieren, um die URL aufzurufen, die der Funktion zugeordnet ist. Zum Abrufen dieses Werts kopieren Sie die Funktions-URL aus Ihrer Funktionen-App.

![Ermitteln der Funktions-URL][2]

Wenn Sie in der Nutzlast der Webhook-POST-Anforderung benutzerdefinierte Eigenschaften benötigen, helfen Ihnen die Informationen unter [Konfigurieren eines Webhooks für eine Azure-Metrikwarnung](../monitoring-and-diagnostics/insights-webhooks-alerts.md) weiter.

## <a name="configure-an-alert-on-a-vm"></a>Konfigurieren einer Warnung auf einem virtuellen Computer

Warnungen können konfiguriert werden, um Personen zu benachrichtigen, wenn eine bestimmte Metrik einen zugewiesenen Schwellenwert überschreitet. In diesem Beispiel wird die Warnung für die gesendeten TCP-Segmente ausgelöst, aber sie kann auch für viele andere Metriken ausgelöst werden. Hier wird eine Warnung konfiguriert, um einen Webhook zum Aufrufen der Funktion aufzurufen.

### <a name="create-the-alert-rule"></a>Erstellen der Warnungsregel

Navigieren Sie zu einem vorhandenen virtuellen Computer, und fügen Sie eine Warnungsregel hinzu. Eine ausführlichere Dokumentation zur Konfiguration von Warnungen finden Sie unter [Erstellen von Warnungen in Azure Monitor für Azure-Dienste – Azure-Portal](../monitoring-and-diagnostics/insights-alerts-portal.md). 

![Hinzufügen einer VM-Warnungsregel zu einem virtuellen Computer][1]

> [!NOTE]
> Einige Metriken sind standardmäßig nicht aktiviert. Weitere Informationen dazu, wie Sie weitere Metriken aktivieren, finden Sie unter [Aktivieren von Überwachung und Diagnose](../monitoring-and-diagnostics/insights-how-to-use-diagnostics.md).

Fügen Sie abschließend die URL aus dem vorherigen Schritt in das Textfeld „Webhook“ Ihrer Warnung ein. Klicken Sie auf **OK**, um die Warnungsregel zu speichern.

![Einfügen der URL in die Warnungsregel][3]

## <a name="downloading-and-viewing-the-capture-file"></a>Herunterladen und Anzeigen der Erfassungsdatei

Wenn Sie Ihre Erfassung in einem Speicherkonto speichern, kann die Erfassungsdatei über das Portal oder programmgesteuert heruntergeladen werden. Wenn die Erfassungsdatei lokal gespeichert ist, wird sie abgerufen, indem die Anmeldung am virtuellen Computer durchgeführt wird. 

Anweisungen zum Herunterladen von Dateien von Azure-Speicherkonten finden Sie unter [Erste Schritte mit Azure Blob Storage mit .NET](../storage/storage-dotnet-how-to-use-blobs.md). Ein weiteres Tool, das verwendet werden kann, ist der Speicher-Explorer. Weitere Informationen zum Speicher-Explorer finden Sie unter [Speicher-Explorer](http://storageexplorer.com/).

Nach dem Herunterladen Ihrer Erfassung können Sie diese mit einem beliebigen Tool anzeigen, das für **CAP**-Dateien geeignet ist. Hier sind Links zu zwei Tools dieser Art angegeben:

[Microsoft Message Analyzer](https://technet.microsoft.com/en-us/library/jj649776.aspx)  
[WireShark](https://www.wireshark.org/)  

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Anzeigen der Paketerfassungen finden Sie unter [Verwenden der Paketerfassung für die proaktive Netzwerküberwachung mit Azure Functions](network-watcher-alert-triggered-packet-capture.md).

[1]: ./media/network-watcher-alert-triggered-packet-capture/figure1.png
[2]: ./media/network-watcher-alert-triggered-packet-capture/figure2.png
[3]: ./media/network-watcher-alert-triggered-packet-capture/figure3.png

