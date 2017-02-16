---
title: Importieren von Daten in Analytics in Azure Application Insights | Microsoft-Dokumentation
description: "Importieren Sie statische Daten zur Verknüpfung mit App-Telemetrie, oder importieren Sie einen separaten Datenstrom zum Abfragen durch Analytics."
services: application-insights
documentationcenter: 
author: alancameronwills
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 02/07/2017
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: 47c3491b067d5e112db589672b68e7cfc7cbe921
ms.openlocfilehash: eb89c6f485f2321f729dcfe650af4355de84a9ac


---
# <a name="import-data-into-analytics"></a>Importieren von Daten in Analytics

Importieren Sie Tabellendaten in [Analytics](app-insights-analytics.md), um sie entweder mit [Application Insights](app-insights-overview.md)-Telemetriedaten aus Ihrer App zu verknüpfen oder als separaten Datenstrom analysieren zu können. Analytics ist eine leistungsstarke Abfragesprache, die sich gut für die Analyse großer Volumen von Telemetriedatenströmen mit Zeitstempel eignet.

Sie können Daten mithilfe Ihres eigenen Schemas in Analytics importieren. Sie müssen nicht die Application Insights-Standardschemas wie „Anforderung“ oder „Ablaufverfolgung“ nutzen.

Derzeit können Sie CSV-Dateien oder ähnliche Formate importieren, die Tabulatoren oder Semikolons als Trennzeichen aufweisen.

Es gibt drei Situationen, in denen das Importieren von Daten in Analytics nützlich ist:

* **Verknüpfen mit App-Telemetriedaten.** Sie können z.B. eine Tabelle importieren, die URLs auf Ihrer Website besser lesbaren Seitentiteln zuordnet. In Analytics können Sie einen Diagrammbericht für das Dashboard erstellen, aus dem die zehn beliebtesten Seiten auf Ihrer Website hervorgehen. Nun werden Seitentitel anstelle der URLs angezeigt.
* **Korrelieren Ihrer Anwendungstelemetriedaten** mit anderen Quellen wie Netzwerkdatenverkehr, Serverdaten oder CDN-Protokolldateien.
* **Anwenden von Analytics auf einen separaten Datenstrom.** Application Insights Analytics ist ein leistungsstarkes Tool, das sich gut für Datenströme mit Zeitstempeln und geringer Dichte eignet (und zwar in vielen Fällen wesentlich besser als SQL). Wenn Sie über einen solchen Datenstrom aus einer anderen Quelle verfügen, können Sie ihn mit Analytics analysieren.

Das Senden von Daten an Ihre Datenquelle ist einfach. 

1. (Einmalig) Definieren Sie das Schema Ihrer Daten in einer „Datenquelle“.
2. (Regelmäßig) Laden Sie Ihre Daten in Azure-Speicher hoch, und rufen Sie die REST-API auf, um uns informieren, dass neue Daten auf ihre Erfassung warten. Binnen weniger Minuten sind die Daten für die Abfrage in Analytics verfügbar.

Die Häufigkeit des Hochladens und Geschwindigkeit, mit der die Daten für Abfragen verfügbar sein sollen, legen Sie fest. Es ist effizienter, Daten in größeren Blöcken hochzuladen, die aber nicht größer als 1 GB sein sollten.

> [!NOTE]
> *Haben Sie viele zu analysierende Datenquellen?* [*Erwägen Sie den Einsatz von *logstash* zum Übertragen Ihrer Daten in Application Insights.*](https://github.com/Microsoft/logstash-output-application-insights)
> 

## <a name="before-you-start"></a>Vorbereitung

Anforderungen:

1. Eine Application Insights-Ressource in Microsoft Azure.

 * Wenn Sie Ihre Daten getrennt von anderen Telemetriedaten analysieren möchten, [erstellen Sie eine neue Application Insights-Ressource](app-insights-create-new-resource.md).
 * Wenn Sie Ihre Daten mit Telemetriedaten aus einer App verknüpfen oder vergleichen, die bereits für Application Insights eingerichtet ist, können Sie die Ressource für diese App verwenden.
 * Zugriff als Besitzer oder Mitwirkender auf diese Ressource
 
2. Azure-Speicher. Sie laden Ihre Daten in Azure-Speicher hoch, aus dem Analytics sie abruft. 

 * Es wird empfohlen, ein dediziertes Speicherkonto für Ihre Blobs zu erstellen. Wenn Ihre Blobs mit anderen Prozessen gemeinsam genutzt werden, benötigen unsere Prozesse mehr Zeit zum Lesen Ihrer Blobs.

2. Solange dieses Feature in der Vorschauphase ist, müssen Sie den Zugriff beantragen.

 * Öffnen Sie Analytics im [Azure-Portal](https://portal.azure.com) in Ihrer Application Insights-Ressource. 
 * Klicken Sie unten im Schemabereich unter **Andere Datenquellen** auf den Link „Kontakt“. 
 * Wenn „Datenquelle hinzufügen“ angezeigt wird, haben Sie bereits Zugriff.


## <a name="define-your-schema"></a>Definieren des Schemas

Bevor Sie Daten importieren können, müssen Sie eine *Datenquelle* definieren, die das Schema Ihrer Daten angibt.

1. Starten Sie den Datenquellen-Assistenten.

    ![Hinzufügen einer neuen Datenquelle](./media/app-insights-analytics-import/add-new-data-source.png)

2. Befolgen Sie die Anweisungen zum Hochladen einer Beispieldatendatei.

 * Die erste Zeile der Beispieldaten kann Spaltenüberschriften enthalten. (Sie können die Feldnamen im nächsten Schritt ändern.)
 * Die Beispieldaten sollten mindestens 10 Zeilen enthalten.

3. Überprüfen Sie das Schema, das der Assistent von Ihren Beispieldaten abgeleitet hat. Bei Bedarf können Sie die abgeleiteten Typen der Spalten anpassen.

4. Wählen Sie einen Zeitstempel aus. Alle Daten in Analytics müssen ein Zeitstempelfeld aufweisen. Es muss den Typ `datetime` haben, jedoch nicht als „timestamp“ benannt sein. Wenn Ihre Daten eine Spalte mit einer Datums- und Uhrzeitangabe im ISO-Format aufweisen, wählen Sie diese als Zeitstempelspalte. Wählen Sie andernfalls „gemäß Dateneingang“, woraufhin der Importvorgang ein Zeitstempelfeld hinzufügt.

    ![Überprüfen des Schemas](./media/app-insights-analytics-import/data-source-review-schema.png)

5. Erstellen Sie die Datenquelle.


## <a name="import-data"></a>Importieren von Daten

Zum Importieren von Daten laden Sie sie in Azure-Speicher hoch, erstellen einen Zugriffsschlüssel dafür und rufen dann eine REST-API auf.

![Hinzufügen einer neuen Datenquelle](./media/app-insights-analytics-import/analytics-upload-process.png)

Sie können den folgenden Vorgang manuell ausführen oder ein automatisiertes System einrichten, um ihn in regelmäßigen Abständen ausführen zu lassen. Sie müssen diese Schritte für jeden Datenblock ausführen, den Sie importieren möchten.

1. Laden Sie die Daten in [Azure-Blobspeicher](../storage/storage-dotnet-how-to-use-blobs.md) hoch. 

 * Blobs können eine maximale unkomprimierte Größe bis zu 1 GB haben. Große Blobs mit Hunderten von MB sind aus Leistungssicht ideal.
 * Sie können Blobs mit Gzip komprimieren, um die Hochladezeit und Latenz der Daten zu verbessern, die zum Abfragen verfügbar sein sollen. Verwenden Sie die Dateierweiterung `.gz`.
 
2. [Erstellen Sie einen SAS-Schlüssel (Shared Access Signature) für das Blob](../storage/storage-dotnet-shared-access-signature-part-2.md). Der Schlüssel muss eine Gültigkeitsdauer von einem Tag haben und Lesezugriff bieten.
3. Erstellen Sie einen REST-Aufruf zum Benachrichtigen von Application Insights, dass Daten warten.

 * Endpunkt: `https://dc.services.visualstudio.com/v2/track`
 * HTTP-Methode: POST
 * Nutzlast:

```JSON

    {
       "data": {
            "baseType":"OpenSchemaData",
            "baseData":{
               "ver":"2",
               "blobSasUri":"<Blob URI with Shared Access Key>",
               "sourceName":"<Schema ID>",
               "sourceVersion":"1.0"
             }
       },
       "ver":1,
       "name":"Microsoft.ApplicationInsights.OpenSchema",
       "time":"<DateTime>",
       "iKey":"<instrumentation key>"
    }
```

Die Platzhalter:

* `Blob URI with Shared Access Key`: Sie erhalten diese Angabe im Verfahren zum Erstellen eines Schlüssels. Sie ist für das Blob spezifisch.
* `Schema ID`: Schema-ID, die für Ihr definiertes Schema generiert wurde. Die Daten in diesem Blob müssen dem Schema entsprechen.
* `DateTime`: Die Uhrzeit, zu der die Anforderung gesendet wurde (in UTC). Wir akzeptieren diese Formate: ISO8601 (z.B. "2016-01-01 13:45:01"); RFC822 ("Mi, 14. Dez 16 14:57:01 +0000"); RFC850 ("Mittwoch, 14-Dez-16 14:57:00 UTC"); RFC1123 ("Mi, 14. Dez. 2016 14:57:00 +0000").
* `Instrumentation key` Ihrer Application Insights-Ressource.

Die Daten stehen nach wenigen Minuten in Analytics zur Verfügung.

## <a name="error-responses"></a>Fehlercodes

* **400 – Ungültige Anforderung**: Gibt an, dass die Nutzlast der Anforderung ungültig ist. Prüfen Sie Folgendes:
 * Die Richtigkeit des Instrumentierungsschlüssels.
 * Die Gültigkeit des Uhrzeitwerts. (Aktuelle Uhrzeit in UTC.)
 * Ob Daten dem Schema entsprechen.
* **403 – Verboten**: Auf das Blob, das Sie gesendet haben, kann nicht zugegriffen werden. Stellen Sie sicher, dass der SAS-Schlüssel gültig und nicht abgelaufen ist.
* **404 – Nicht gefunden**:
 * Das Blob ist nicht vorhanden.
 * Der Datenquellenname ist falsch.

Ausführlichere Informationen sind in der zurückgegebenen Fehlermeldung verfügbar.

## <a name="sample-code"></a>Beispielcode

Dieser Code verwendet das NuGet-Paket [Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/9.0.1).

### <a name="classes"></a>Klassen

```C#


namespace IngestionClient 
{ 
    using System; 
    using Newtonsoft.Json; 

    public class AnalyticsDataSourceIngestionRequest 
    { 
        #region Members 
        private const string BaseDataRequiredVersion = "2"; 
        private const string RequestName = "Microsoft.ApplicationInsights.OpenSchema"; 
        #endregion Members 

        public AnalyticsDataSourceIngestionRequest(string ikey, string schemaId, string blobSasUri, int version = 1) 
        { 
            Ver = version; 
            IKey = ikey; 
            Data = new Data 
            { 
                BaseData = new BaseData 
                { 
                    Ver = BaseDataRequiredVersion, 
                    BlobSasUri = blobSasUri, 
                    SourceName = schemaId, 
                    SourceVersion = version.ToString() 
                } 
            }; 
        } 


        [JsonProperty("data")] 
        public Data Data { get; set; } 

        [JsonProperty("ver")] 
        public int Ver { get; set; } 

        [JsonProperty("name")] 
        public string Name { get { return RequestName; } } 

        [JsonProperty("time")] 
        public DateTime Time { get { return DateTime.UtcNow; } } 

        [JsonProperty("iKey")] 
        public string IKey { get; set; } 
    } 

    #region Internal Classes 

    public class Data 
    { 
        private const string DataBaseType = "OpenSchemaData"; 

        [JsonProperty("baseType")] 
        public string BaseType 
        { 
            get { return DataBaseType; } 
        } 

        [JsonProperty("baseData")] 
        public BaseData BaseData { get; set; } 
    } 


    public class BaseData 
    { 
        [JsonProperty("ver")] 
        public string Ver { get; set; } 

        [JsonProperty("blobSasUri")] 
        public string BlobSasUri { get; set; } 

        [JsonProperty("sourceName")] 
        public string SourceName { get; set; } 

        [JsonProperty("sourceVersion")] 
        public string SourceVersion { get; set; } 
    } 

    #endregion Internal Classes 
} 


namespace IngestionClient 
{ 
    using System; 
    using System.IO; 
    using System.Net; 
    using System.Text; 
    using System.Threading.Tasks; 
    using Newtonsoft.Json; 

    public class AnalyticsDataSourceClient 
    { 
        #region Members 
        private readonly Uri endpoint = new Uri("https://dc.services.visualstudio.com/v2/track"); 
        private const string RequestContentType = "application/json; charset=UTF-8"; 
        private const string RequestAccess = "application/json"; 
        #endregion Members 

        #region Public 

        public async Task<bool> RequestBlobIngestion(AnalyticsDataSourceIngestionRequest ingestionRequest) 
        { 
            HttpWebRequest request = WebRequest.CreateHttp(endpoint); 
            request.Method = WebRequestMethods.Http.Post; 
            request.ContentType = RequestContentType; 
            request.Accept = RequestAccess; 

            string notificationJson = Serialize(ingestionRequest); 
            byte[] notificationBytes = GetContentBytes(notificationJson); 
            request.ContentLength = notificationBytes.Length; 

            Stream requestStream = request.GetRequestStream(); 
            requestStream.Write(notificationBytes, 0, notificationBytes.Length); 
            requestStream.Close(); 

            HttpWebResponse response; 
            try 
            { 
                using (var response = (HttpWebResponse)await request.GetResponseAsync())
                {
                    return response.StatusCode == HttpStatusCode.OK;
                }
            } 
            catch (WebException e) 
            { 
                HttpWebResponse httpResponse = e.Response as HttpWebResponse; 
                if (httpResponse != null) 
                { 
                    Console.WriteLine( 
                        "Ingestion request failed with status code: {0}. Error: {1}", 
                        httpResponse.StatusCode, 
                        httpResponse.StatusDescription); 
                    return false; 
                }
                throw; 
            } 
        } 
        #endregion Public 

        #region Private 
        private byte[] GetContentBytes(string content) 
        { 
            return Encoding.UTF8.GetBytes(content);
        } 


        private string Serialize(AnalyticsDataSourceIngestionRequest ingestionRequest) 
        { 
            return JsonConvert.SerializeObject(ingestionRequest); 
        } 
        #endregion Private 
    } 
} 

```

### <a name="ingest-data"></a>Erfassen von Daten

Verwenden Sie für jedes Blob diesen Code. 

```C#


   AnalyticsDataSourceClient client = new AnalyticsDataSourceClient(); 

   var ingestionRequest = new AnalyticsDataSourceIngestionRequest("iKey", "tableId/sourceId", "blobUrlWithSas"); 

   bool success = await client.RequestBlobIngestion(ingestionRequest);

```

## <a name="next-steps"></a>Nächste Schritte

* [Tour durch die Abfragesprache Analytics](app-insights-analytics-tour.md)
* [Verwenden von *Logstash* zum Senden von Daten an Application Insights](https://github.com/Microsoft/logstash-output-application-insights)



<!--HONumber=Jan17_HO3-->


